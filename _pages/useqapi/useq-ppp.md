---
permalink: /useqapi/useq-ppp/
title: "uSEQ API: DSP with the Patchable Programmable Processor"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

# What is the PPP?

uSEQ runs on a dual core RP2xxx microcontroller, but until now (that is, the release of firmware 1.2) we've been running everything on core 0, with an entire core going spare. So, we've been thinking about how to best use it.  We're now introducing the Patchable Programmable Processor, a DSP engine that runs on core 1.  Think of it a bit like a digital mini-modular running alongside uSeq.  You can load up a variety of processing blocks, patch them together, control them from code, and pipe the outputs back into the modulisp engine.  You can also send signals or audio to a DAC if available (e.g. on the [MEMLNaut](https://musicallyembodiedml.github.io/memlnaut/) platform).

## Signal Processing Paradigms

The PPP follows a more traditional signal processing paradigm compared to the Modulisp engine.  It runs at a regular sample rate where Modulisp runs with variable speed. The processors it runs are stateful whereas Modulisp is a mostly stateless system. This means that the PPP can complement Modulisp by providing functionality such as triggered events, precise filtering, and sample playback.

## DSP Blocks

PPP is based on a version of the [DSPatch library](https://github.com/Emute-Lab-Instruments/dspatch) that we've modified to run on microcontrollers. The system offers a set of signal processing blocks that can be mounted and connected together into a signal processing graph.  The graph can be run at any sample rate. Functions in Modulisp allow you to create, configure and communicate with the DSP graph.

## Terminology

Borrowing from [SuperCollider](https://supercollider.github.io/), we call our signal processing blocks *UGens*, short for *unit generators*.  A UGen is a program that can generate or process signals, and send and receive signals from other UGens.  You can create as many UGens as the system has CPU capacity for. You can create multiple instances of a single type of UGen (e.g. lots of oscillators).

# Loading Sample Data

For hardware with a DAC, you can play samples with the sampler UGen.  Sample data is loaded into flash memory, giving around 14-15mb memory for audio data, at the sample rate of your choosing.  Sample libraries are created using a Python script, which is in the ```samples``` folder in the uSEQ [github repo](https://github.com/Emute-Lab-Instruments/uSEQ).  Follow the instructions in the readme.

# API

## DSP Graph Control 

### `ppp-go [sample rate]`

Start the DSP graph at a specific sample rate

``` clojure
;for low frequency CV processing
(ppp-go 500)
```

``` clojure
;for audio, use higher sample rates
(ppp-go 22050)
```

### `ppp-stop`

Stop the DSP graph

``` clojure
(ppp-stop)
```

### `ppp-reset`

Stop and reset the DSP graph, clearing any UGens that have been mounted

``` clojure
(ppp-reset)
```

## Managing UGens

### `ppp-mount [name] [processor type]`

Create a new instance of a UGen. [name] is used later to for communication with the UGen.

``` clojure
(ppp-mount mymixer ugen-mixer)
```

### `ppp-unmount [name]`

Remove an instance of a UGen.

``` clojure
(ppp-unmount mymixer)
```

## Connecting UGens

### `ppp-patch [source] [output index] [destination] [input index]`

Connect an output of a UGen instance to the input of another one.

``` clojure
;prints a counter to the post window
(ppp-go 1)
(ppp-mount counter ugen-counter)
(ppp-mount print ugen-serial-print)
(ppp-patch counter 0 print 0)
```

### `ppp-unpatch [ugen] [input index]`

Disconnect an input of a UGen

``` clojure
;prints a counter to the post window
(ppp-go 1)
(ppp-mount counter ugen-counter)
(ppp-mount print ugen-serial-print)
(ppp-patch counter 0 print 0)

;
(ppp-unpatch print 0)
;the serial-print UGen will now print null as there's nothing connected to its input
```

## Sending and receiving data with Modulisp

There are two ways to communicate between Modulisp and the UGens.  For less frequent communication (e.g. occasional configuration of UGens, you can use the messaging system.  For higher frrequency communications, you can use ```queues```.

Messages are sent as follows:

### `ppp-msg [ugen] [msg] [value]`

Send a message to a UGen.  See the UGen documentation on which messages they receive.

``` clojure
;prints a phasor to the post window
(ppp-go 2)
(ppp-mount phasor ugen-phasor)
(ppp-mount print ugen-serial-print)
(ppp-patch phasor 0 print 0)
(ppp-msg phasor freq 0.1)
```

### Queues

```Queues``` allow higher frequency communication.  Queues can be created by UGens so you can communicate directly with them. There are also some UGens that allow you to create your own queues.

If a UGen creates its own queues, you can see these listed in the console when you mount the UGen. E.g. when you mount ```ugen-sampler``` 

``` clojure
(ppp-mount samp ugen-sampler)
```

you'll see something like this in the console:

```
> uSEQ: Created processor: 4 sampler

> uSEQ: Created input queue: samp-in0
```

The UGens ```ugen-queue-input``` and ```ugen-queue-output``` create queues to allow you to input and output data anywhere in your DSP graph.

To send and receive data, use these functions:

### `ppp-set [queue name] [value]`

Send a value to a queue, which is received by a UGen.

``` clojure
(ppp-go 5)

;queue input -> random generator -> serial print
(ppp-mount qtrig ugen-queue-input)
(ppp-mount rand ugen-rand)
(ppp-mount print ugen-serial-print)

(ppp-patch qtrig 0 rand 0)
(ppp-patch rand 0 print 0)

;keep running these two lines of code to trigger new random numbers
(ppp-set qtrig-in0 0)
(ppp-set qtrig-in0 1)

;send a square wave to the queue, to trigger new random numbers periodically
(q0 (ppp-set qtrig-in0 (sqr bar)))

```

### `ppp-get [queue name]`

Receive a value from a queue, which is sent by a UGen.

``` clojure
(ppp-go 5)
;create a phasor and connect to an output queue
(ppp-mount qphasor ugen-queue-output)
(ppp-mount phasor ugen-phasor)
(ppp-patch phasor 0 qphasor 0)
(ppp-msg phasor freq 0.1)

;run this line to get the latest phasor value
(ppp-get qphasor-out0)
```


### `ppp-qlist [ugen]`

List the input and output queues for a UGen, in the console.

``` clojure
(ppp-mount samp ugen-sampler)
(ppp-qlist samp)
```

### Using the q0 function

```q0``` lets you set a function to run at the start of each quantum in the uSeq sequencer.  It'a a bit like using the ```a[x]``` or ```d[x]``` functions, except that the output is not sent anywhere.

```q0``` is useful with uGens because you can use it to trigger messages to be sent or received to queues, for applications such as sequencing or pitch modulation.  It binds the flexibility of livecoding in uSeq with the functions of the PPP systemn.

<!-- add an example here? -->

# Processors

### `ugen-phasor`

A ramp generator that rises linearly from 0 to 1

| Messages |
| Name | Description | Range |
| --- | --- | --- |
| freq | Sets the frequency of the phasor | >= 0 |


| Outputs |
| Index | Description | Range |
| --- | --- | --- |
| 0 | Phase | 0 - 1 |



``` clojure
;prints a counter to the post window
(ppp-go 1)
(ppp-mount counter ugen-counter)
(ppp-mount print ugen-serial-print)
(ppp-patch counter 0 print 0)
```

### `ugen-lfsaw`

A saw wave generator. No antialiasing, so only use this for low frequencies.

| Messages |
| Name | Description | Range |
| --- | --- | --- |
| freq | Sets the frequency of the saw | >= 0 |


| Outputs |
| Index | Description | Range |
| --- | --- | --- |
| 0 | Saw | -1 - 1 |



``` clojure
;play a saw wave
(ppp-go 22050)
(ppp-mount lfsaw ugen-lfsaw)
(ppp-mount dac ugen-dac)
(ppp-patch lfsaw 0 dac 0)
(ppp-patch lfsaw 0 dac 1)
(ppp-msg lfsaw freq 100)
```


### `ugen-rand`

A random number generator that generates a new random number every time it's triggered by a positive zero-crossing.



| Inputs |
| Index | Description | Range |
| --- | --- | --- |
| 0 | Trigger | any |

| Outputs |
| Index | Description | Range |
| --- | --- | --- |
| 0 | A random number | 0 - 1 |



``` clojure
(ppp-go 5)

;queue input -> random generator -> serial print
(ppp-mount qtrig ugen-queue-input)
(ppp-mount rand ugen-rand)
(ppp-mount print ugen-serial-print)

(ppp-patch qtrig 0 rand 0)
(ppp-patch rand 0 print 0)

;keep running these two lines of code to trigger new random numbers
(ppp-set qtrig-in0 0)
(ppp-set qtrig-in0 1)

;send a square wave to the queue, to trigger new random numbers periodically
(q0 (ppp-set qtrig-in0 (sqr bar)))

```


<!-- ### `ugen-lpf`

### `ugen-hpf` -->

### `ugen-mixer`

Mix signals together.

| Messages |
| Name | Description | Range |
| --- | --- | --- |
| master_gain | The overall gain of the mixer. Negative values to invert the output. | any |
| set_inputs | Set the number of inputs to the mixer | >0 |

| Inputs |
| Index | Description | Range |
| --- | --- | --- |
| Nn| An input signal. N is the number of the input channel, starting from 0. | 0 or more |

| Outputs |
| Index | Description | Range |
| --- | --- | --- |
| 0 | The mix of inputs | 0 - 1 |

``` clojure
; mix together 4 saw waves, at audio rate
(ppp-go 22050)
(ppp-mount lfsaw1 ugen-lfsaw)
(ppp-mount lfsaw2 ugen-lfsaw)
(ppp-mount lfsaw3 ugen-lfsaw)
(ppp-mount lfsaw4 ugen-lfsaw)
(ppp-mount mix ugen-mixer)
(ppp-mount dac ugen-dac)

(ppp-msg mix set_inputs 4)
(ppp-msg lfsaw1 freq 100)
(ppp-msg lfsaw2 freq 101)
(ppp-msg lfsaw3 freq 102)
(ppp-msg lfsaw4 freq 103)
(ppp-msg mix master_gain 0.2)


(ppp-patch lfsaw1 0 mix 0)
(ppp-patch lfsaw2 0 mix 1)
(ppp-patch lfsaw3 0 mix 2)
(ppp-patch lfsaw4 0 mix 0)
(ppp-patch mix 0 dac 0)
```

### `ugen-sampler`

Play and manipulate audio samples.

| Messages |
| Name | Description | Range |
| --- | --- | --- |
| sample | Load a sample | text |
| loop | Turn looping on or off | 0 or 1 |
| rate | Change the playback rate, as a multiple of the original speed. Negative rates play the sample in reverse. This message is ignored if a ugen is connected to the rate input. | any |


| Queues |
| Index | Description | Range |
| --- | --- | --- |
| 0 | Retrigger sample on positive zero crossing | any |

| Inputs |
| Index | Description | Range |
| --- | --- | --- |
| 0 | Playback rate (works identically to the ```rate``` message above) | any |
| 1 | Phase trigger. When there there is a change in this input, the sample player will read this input and jump to the corresponding position in the sample, where 0 is ther start of the sample and 1 is the end.| 0 - 1 |



| Outputs |
| Index | Description | Range |
| --- | --- | --- |
| 0 | Sample playback | any |

Examples:

``` clojure
;loop a sample
(ppp-go 22050)
(ppp-mount samp ugen-sampler)
(ppp-mount dac ugen-dac)
(ppp-patch samp 0 dac 0)
(ppp-msg samp sample "drone1")
(ppp-msg samp loop 1)
(ppp-msg samp rate 1.2)

;retrigger the sample at the start of each bar
(q0 (ppp-set samp-in0 (sqr bar)))
```


```clojure
;control a sample using the rate and phase inputs
(ppp-go 22050)
(set-bpm 115.38)

;mount ugens
(ppp-mount samp ugen-sampler)
(ppp-mount dac ugen-dac)
(ppp-mount qrate ugen-queue-input)
(ppp-mount qphase ugen-queue-input)

;patching
(ppp-patch samp 0 dac 0)
(ppp-patch qrate 0 samp 0)
(ppp-patch qphase 0 samp 1)

;load a sample
(ppp-msg samp sample "buttercup")
(ppp-msg samp loop 1)

;try running these with different values
(ppp-set qrate-in0 1)
(ppp-set qphase-in0 0.5)
```

```clojure
;slicing a sample
(ppp-go 22050)
(set-bpm 115.38)

;mount ugens
(ppp-mount samp ugen-sampler)
(ppp-mount dac ugen-dac)
(ppp-mount qrate ugen-queue-input)
(ppp-mount qphase ugen-queue-input)

;patching
(ppp-patch samp 0 dac 0)
(ppp-patch qrate 0 samp 0)
(ppp-patch qphase 0 samp 1)

;load a sample
(ppp-msg samp sample "buttercup")
(ppp-msg samp loop 1)

;This sends sequences to the rate and phase inputs
;The rate sequence is scaled by the the value of (knob). Try playing with the sequence [1 0.5]; try out negative values.
;The phase sequence splits the sample into 8 sections and plays them back in any order. Try changing this sequence to explore different parts of the loop.
(q0 (do
      (ppp-set qrate-in0 (* (scale -1 1 (knob))
                           (seq [1 0.5] (offset 0 (fast 1 bar)))))
      (ppp-set qphase-in0 (* (seq [0 1 2 3 4 5 6 7] (fast 1 bar)) 0.125))
      ))

```

``` clojure
;create a sequence of triggered samples
(ppp-go 22050)
;create 3 samplers
(ppp-mount samp ugen-sampler)
(ppp-mount samp2 ugen-sampler)
(ppp-mount samp3 ugen-sampler)

;output section, using a mixer
(ppp-mount dac ugen-dac)
(ppp-mount mix ugen-mixer)
(ppp-msg mix set_inputs 3)

;make connections
(ppp-patch samp 0 mix 0)
(ppp-patch samp2 0 mix 1)
(ppp-patch samp3 0 mix 2)
(ppp-patch mix 0 dac 0)

;load samples
(ppp-msg samp sample "knock")
(ppp-msg samp2 sample "drr")
(ppp-msg samp3 sample "laekur")

;send a sequence to trigger each sampler
(q0
  (do
    (ppp-set samp-in0 (rpulse [3 3 2] 0.1 bar))
    (ppp-set samp2-in0 (rpulse [2 6 2] 0.1 bar))
    (ppp-set samp3-in0 (rpulse [1 2] 0.1 (offset 0.5 (fast 2 bar))))
    ))
```




### `ugen-dac`

If running uSeq on hardware with a DAC, this uGen outputs signals to the DAC.

| Inputs |
| Index | Description | Range |
| --- | --- | --- |
| 0 | Input signal | any |
| 1 | Input signal | any |



``` clojure
;play a saw wave
(ppp-go 22050)
(ppp-mount lfsaw ugen-lfsaw)
(ppp-mount dac ugen-dac)
(ppp-patch lfsaw 0 dac 0)
(ppp-patch lfsaw 0 dac 1)
(ppp-msg lfsaw freq 100)
```

### `ugen-counter`

Output an integer that increments by 1 every quantum.


| Outputs |
| Index | Description | Range |
| --- | --- | --- |
| 0 | Count | 0 or more |



``` clojure
;prints a counter to the post window
(ppp-go 1)
(ppp-mount counter ugen-counter)
(ppp-mount print ugen-serial-print)
(ppp-patch counter 0 print 0)
```

### `ugen-serial-print`

Print the value of the input to the serial console.


| Inputs |
| Index | Description | Range |
| --- | --- | --- |
| 0 | The number to print | any |


*Note - don't run this UGen at high sample rates*


``` clojure
;prints a counter to the post window
(ppp-go 1)
(ppp-mount counter ugen-counter)
(ppp-mount print ugen-serial-print)
(ppp-patch counter 0 print 0)
```

