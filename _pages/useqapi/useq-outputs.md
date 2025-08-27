---
permalink: /useqapi/useq-outputs/
title: "uSEQ API: Outputs and Callbacks"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## Sequencing Callback Functions for (continuous) CV

### `a[1/2/3/...] <form>`

Specify a function to calculate the value of analog output 1/2/3/etc. This function will be evaluated once every update loop.

```
(a1 (* 3 (from-list [0.4 0.1] bar)))
```
```
(a2 0)
```
```
;; Silence all analog outs at the same time
(do 
  (a1 0)
  (a2 0)
  (a3 0))

```

## Sequencing Callback Functions for Gates & Triggers

### `d[1/2/3/...] <form>`

Specify a function to calculate the value of digital (i.e. binary) output 1/2/3/etc. This function will be evaluated once every update loop.

``` clojure
(d1 (square (slow 2 beat)))
```
``` clojure
(d2 (from-list [0 0 1 0 1 1 0 1] (slow 2 bar)))
```
``` clojure
;; Silence all digital outs at the same time
(do 
  (d1 0)
  (d2 0)
  (d3 0))

```

## Sequencing Callback Functions for Serial/MIDI

### `s[x] <form>`

(where x is a number between 1 and 8)

Send a sequence over the USB serial connection.  This sequence can be decoded by your USB client software.  To differentiate from other text send by the module, serial data is send in a 11 byte message in the following format:
[31][0][index of serial stream][8 bytes representing a double]

The useq editor can decode these messages and forward them as MIDI.


## Timed Sequencing Callback Functions

### `q0 <form>`

Specify a function to run at the start of each quantum


``` clojure
;modulate a variable and send to a1
(define x 0.001)
(q0 (set x (usin t)))
(a1 x)
```

``` clojure
; reset the q0 function
(q0 0)
```

```q0``` is useful for working with the dsp engine.
