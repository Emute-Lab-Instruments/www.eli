---
permalink: /tutorials/polyrhythms_and_polymeters/
title: "uSEQ Tutorial: Polyrhythms, Polymeters, and Polytempos (multiple BPMs)"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## Introduction
While uSEQ has a number of [built-in timing utilities](_pages/useqapi/useq-timing.md#timing-variables) that default to working with a 'global' BPM for convenience, sometimes you may want a more flexible timing system. Creating your own phasors is easy and can open up a whole lot of avenues for temporal exploration and experimentation such as polyrhythms, polymeters, and polytempos.

## Polyrhythms
The term 'polyrhythm' refers to having multiple rhythms playing at the same time and with the the same period of time. For example, if an instrument plays 3 evenly-spaced hits per bar, while another plays 4 evenly-spaced hits per bar, their combination is known as the "3-4 polyrhythm".

Polyrhythms are very easy to achieve in uSEQ:

With `sqr`:
```clojure
(d1 (sqr (fast 3 bar)))
(d2 (sqr (fast 4 bar)))
```

With `gates`:
```clojure
(d1 (gates [1 1 1] bar))
(d2 (gates [1 1 1 1] bar))
```

With `trigs`:
```clojure
(d1 (trigs [9 9 9] bar))
(d2 (trigs [9 9 9 9] bar))
```

You can also sum these patterns together to play the polyrhythm out of a single output:

```clojure
(d1 (+ (gates [1 1 1] 0.2 bar)
       (gates [1 1 1 1] 0.2 bar)))
        
```

## Polymeters
The term 'polymeter' refers to having multiple meters, a musical term for "how many beats we call a bar". In contrast with polyrhythms, where the duration of each beat is compressed or expanded to fit the desired number of beats 

## Polytempos
(do
  (def my-bpm-1 120.0)
  (set-bpm my-bpm-1)
  (def my-bpm-2 115.0)
  (def slow-amt (/ my-bpm-1 my-bpm-2))
  (def bar2 (slow slow-amt bar))
  (def beat2 (slow slow-amt beat))
  (d1 (sqr beat))
  (d2 (sqr beat2)))


