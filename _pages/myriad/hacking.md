---
permalink: /myriad/hacking/
title: "Myriad Hacker's Guide"
sidebar:
  nav: "myriad"
toc: true
toc_sticky: true

---

Myriad is open-source; source code and schematics can be found on our [github](github.com/Emute-Lab-Instruments/Myriad/)



Here are some ideas for how to modify Myriad.

# Firmware Hacking

We suggest pointing your favourite LLM at the code base and asking it about particular features, or come and ask for advice on our Discord server.

## Easier

- Customise colours of the animations
- Customise interval mappings
- Customise meta-modulator parameters, e.g.
  - initial conditions for the attractors
  - cohesion and separation strength of the boids
  - different initialisation for the neural network
  - speed and depth ranges for modulation
- Customise oscillator models e.g. change how they respond to the epsilon control
- Customise the limits of mappings of the spread control

## Intermediate

- Add new meta-modulators
- Add new oscillator models

## Advanced

The Myriad hardware provides you with 4 attenverted ADC inputs, stereo outputs for bitstreams, three rotaty encoders, 4 MCU cores and potential for intermodule connectivity with I2C. You can design your own synth that sits on this platform.


<!-- # Hardware Hacking Ideas

- change the gain structure of the shaped outputs section -->



