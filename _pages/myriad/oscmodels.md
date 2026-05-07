---
permalink: /myriad/oscmodels
title: "Myriad Oscillator Models"
sidebar:
  nav: "myriad"
toc: true
toc_sticky: true
---


Myriad has a selection of *oscillator models*; each one is driven by a digital algorithm, using Bitwave Synthesis. Bitwave is a collection of methods for creating high-frequency streams of binary patterns, that are converted into sound using analog circuitry.  Each model has two paramters of control: frequency and ***&#x03F5;***.  The ***&#x03F5;*** parameter creates tonal changes, and works in a different way with each model.  


## (0) Saw

Generates a saw wave with variable pulse width.

***&#x03F5;*** controls the pulse width of the saw wave, from around 25% of wavelength to 0.25%, moving from a full sound to a thiner sound.

![Saw spectrum](../../assets/images/myriad/waveforms/oscmodel0.png)

{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_saw.ogg|Saw Model"
   height = "300"
   color  = "ffaa00"
%}

## (1) Chirp

The Chirp oscillator creates sound with sequences of tiny pulses.

***&#x03F5;*** controls how the spread and frequency of the pulses changes, creating tonal variations.

![Chirp spectrum](../../assets/images/myriad/waveforms/oscmodel1.png)

{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_chirp.ogg|Chirp Model"
   height = "300"
   color  = "ffaa00"
%}

## (2) Sharkteeth

Multiple sawtooth ramps enveloped by a square wave.

***&#x03F5;*** controls the number of teeth per cycle (1 to 20), allowing you to precisely set the dominant upper harmonic.

![Sharkteeth spectrum](../../assets/images/myriad/waveforms/oscmodel2.png)


{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_sharkteeth.ogg|Sharkteeth Model"
   height = "300"
   color  = "ffaa00"
%}

## (3) Pulse

A pulse wave.

***&#x03F5;*** controls the pulse width.

![Pulse spectrum](../../assets/images/myriad/waveforms/oscmodel3.png)

{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_pulse.ogg|Pulse Model"
   height = "300"
   color  = "ffaa00"
%}

## (4) Resonant Chirp

A variation of the chirp model, with a more resonant tone.

***&#x03F5;*** controls how the width and frequency of the pulses changes, creating tonal variations.

![Resonant Chirp spectrum](../../assets/images/myriad/waveforms/oscmodel4.png)

{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_resochirp.ogg|Resonant Chirp Model"
   height = "300"
   color  = "ffaa00"
%}

## (5) Triangle

***&#x03F5;*** varies the position of the peak point of the triangle.

![Triangle spectrum](../../assets/images/myriad/waveforms/oscmodel5.png)

{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_tri.ogg|Triangle Model"
   height = "300"
   color  = "ffaa00"
%}

## (6) Piranha

The name will make sense when you see the waveform: a triangle wave with a secondary triangle sub-oscillator running at a higher rate.  The sub-oscillator creates teeth within the triangle envelope, adding harmonics that ride on top of the triangle's natural −12 dB/octave rolloff.

***&#x03F5;*** controls the number of teeth created by the sub-oscillator  — low numbers give a gentle shimmer, high numbers give a dense, bright texture and bite. 

![Piranha spectrum](../../assets/images/myriad/waveforms/oscmodel6.png)

{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_piranha.ogg|Piranha Model"
   height = "300"
   color  = "ffaa00"
%}

## (7) Parasine

A parabolic approximation of a sine wave oscillator.

***&#x03F5;*** applies waveshaping, adding subtle harmonics.

![Parasine spectrum](../../assets/images/myriad/waveforms/oscmodel7.png)

{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_parasine.ogg|Parasine Model"
   height = "300"
   color  = "ffaa00"
%}

## (8) Formant

An oscillator with voice-like harmonics

***&#x03F5;*** varies from 'ahh' to 'ooo' tonality.

![Formant spectrum](../../assets/images/myriad/waveforms/oscmodel8.png)

{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_formant.ogg|Formant Model"
   height = "300"
   color  = "ffaa00"
%}

## (9) Metallic

FM style metallic sounds

***&#x03F5;*** increases inharmonic partials.

![Metallic spectrum](../../assets/images/myriad/waveforms/oscmodel9.png)

{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_metallic.ogg|Metallic Model"
   height = "300"
   color  = "ffaa00"
%}

## (10) Random Walk Noise

Generates varied types of noise, using a model of a random walk.

***Frequency*** changes the speed of the random generator, and the balance of low and high frequencies in the noise.

***&#x03F5;*** changes the type of noise.  This control is a bit like tuning a radio; it moves through different sounds from pops and crackles to whistles to full frequency noise.

![Noise spectrum](../../assets/images/myriad/waveforms/oscmodel10.png)

{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_randomwalk.ogg|Random Walk Noise Model"
   height = "300"
   color  = "ffaa00"
%}

## (11) Bitnoise

A noise generator with variable tone.  Sounds vary from white(ish) noise to occasional pops and crackles.

***&#x03F5;*** controls the probability of small changes in a bitstream.

***Frequency*** controls the probability of large changes in a bitstream.

![Bitnoise spectrum](../../assets/images/myriad/waveforms/oscmodel11.png)

{% include spectro_player.html
   files  = "../../assets/audio/myriad/myriad_bitnoise.ogg|Bitnoise Model"
   height = "300"
   color  = "ffaa00"
%}

## (12) Silence

This setting switches off the oscillator bank.  Use this to create thinner sounds by switching off banks so that you use less oscillators in the final mix.

<!-- ![Silence](../../assets/images/myriad/waveforms/oscmodel12.png) -->


# Bitwave Synthesis

This is a collection of methods that involve the manipulation of high-frequency binary pulse streams (streams of zeros and ones) which are generated on microcontrollers, and then converted into waveforms using analog circuitry.  The density of the binary streams correlates with the amplitude of the analog waveform.

The collection of methods fall under these categories:

## Variations on delta-sigma modulation

Delta-sigma modulation (DSM) uses an accumulator with feedback to create pulse streams.  You can use this to accurately create waveforms (e.g. the saw and pulse oscillators), with either numerical calculation or wavetables. 

## Chirp models

These methods generate sequences of tiny chirps (pulse patterns) that vary over time.  These pulses can be sequenced to create waveforms.

## Noise models

Noise methods use different approaches to random generation of pulse streams.

## Technical Details

Bitwave models run on RP2xxx microcontrollers, taking advantage of the *programmable IO* state machines to generate bitstreams at high frequencies.  The models run in a callback loop, using DMA to feed buffered data into the state machines. Buffers are around 250-500 microseconds long.  The models vary in speed, depending on how CPU-intensive it is to calculate the buffers.  Each microcontroller core can run three of these models.  Myriad has four cores, but one is saved for non-synthesis tasks: running the meta-modulators and drawing the screen.

