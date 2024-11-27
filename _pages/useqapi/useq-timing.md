---
permalink: /useqapi/useq-timing/
title: "uSEQ API: Timing System"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

Most digital music systems run on a fixed update loop frequency, but uSEQ is a bit different: It tries to run and update as frequently as possible, given the task(s) it's been asked to perform. This is much like how many videogames will try to run at the highest FPS possible, given the complexity of the scene they have to render at each moment. 

Timing functions take a functional rendering approach; they are "driven" by the current time ```t``` (or any time-varying value that's derived from it), and react accordingly. 

Much of the sequencing in uSEQ is done using _phasors_: ramps that rise from 0 to 1 over a fixed time period. There are a few pre-defined phasors for convenience (see below), but you don't have to use them and you can always define your own. 

For example, to get a phasor that loops from 0 to 1 over any arbitrary amount of time, we can use something like the following:

```clojure
(def my-phasors-duration 1234)
(def my-phasor (/ (% t my-phasors-duration)
                  my-phasors-duration))
```

## Timing Variables

### `time`

The number of seconds since the module was switched on. This is _always_ absolute and cannot be affected by any speeding-up, slowing-down, or any other temporal transformations (e.g. `sample-at-time`, `fast`, `slow`, and `offset`).

### `t`

Similar to `time`, but represents _logical_ time instead. If you are familiar with Digital Audio Workstations (DAWs), this is similar to the notion of the transport marker/playhead: the number of seconds from the beginning of the project's timeline. 

Calling `(useq-rewind)` will reset this to 0, effectively rewinding the module's "playback position marker". Unless rewound, this will simply be the same as `time`, but unlike `time` it can be subject to temporal transformations (e.g. `sample-at-time`, `fast`, `slow`, and `offset`).

### `beat` 

A phasor, rising from 0-1 over the length of a beat (dependent on the BPM and time signature).

To define your own `beat`-style phasor, please refer to 

```clojure
(d1 (sqr beat))
```

### `bar` 

A phasor, rising from 0-1 over the length of a bar (dependent on the BPM and time signature).

```clojure
(d1 (sqr bar))

(a1 (from-list [0 0.25 0.5 0.75 1] bar))
```

### `phrase` 

A phasor, rising from 0-1 over the length of a phrase (dependent on the BPM, time signature, and phrase length in bars).

### `section` 

A phasor, rising from 0-1 over the length of a section (dependent on the BPM, time signature, and section length in bars).

### `bpm`

The tempo in beats per minute.

### `bps`

The tempo in beats per second.

## Time Modification Functions

### `sample-at-time <time> <sig>`

Returns the value of a signal at a particular moment in time.

```clojure

```

### `slow`

## Timing Functions



### `set-bpm <bpm>`

Set the speed of the sequencer in beats per minute.

| Parameter | Description | Range |
| --- | --- | --- |
| bpm | beats per minute | > 0 |


### `set-time-sig <numerator> <denominator>`

Set the time signature of the sequencer.

| Parameter | Description | Range |
| --- | --- | --- |
| numerator | number of beats in a measure | > 0 |
| denominator | the length of a beat | > 0 |

### `get-clock-source`

Returns the source for the clock, either internal (0), gate input 1 (1) or gate input 2 (2).

### `set-clock-int`

Sets the clock source to the internal clock

### `set-clock-ext`

Sets the clock source to be driven by either gate input 1 or 2.  uSEQ will calculate the current bpm from the time between rising edges of the gate.  It will also stay in phase by reseting uSEQs logical time to zero, either at the start of each phrase if the bpm tracking is stable, or every bar if the bpm tracking is unstable.  If you change the speed of the external signal, uSEQ will try to adapt to it as quickly as possible. uSEQ will track bars and phrases by counting from the first rising edge it receives.  You can reset this information using `reset-clock-ext`. 

| Parameter | Description | Range |
| --- | --- | --- |
| input number | which gate input should uSEQ synchronise to? | 1 or 2|
| clock divisor | the number of clocks per beat | >0 |

### `reset-clock-int`

Reset the internal clock, starting the logical time from 0.

### `reset-clock-ext`

Reset the data used by the external clock tracker.  If you are changing from one external clock to another, it might be useful to run this.  After running this function, the first rising edge to be detected will be assumed to be the start of the first bar.
