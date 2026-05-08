---
permalink: /myriad/build/
title: "Myriad Build Guide"
sidebar:
  nav: "myriad"
toc: true
toc_sticky: true

---


# Kit Contents

| Component | Quantity |
|---|---|
| Myriad PCB | 1 |
| ELI2040c PCB | 2 |
| Panel | 1 |
| Capacitors | 4 |
| 40 pin header male | 2 |
| 40 pin header male | 2 |
| 12mm spacer M3 | 2 |
| M3 screw 6mm | 2 |
| M3 nut for spacer | 2 |
| 6mm spacer M2 | 4 |
| M2 nut for spacer | 4 |
| M2 screw | 4 |
| Round TFT screen | 1 |
| Rotary encoder | 3 |
| B100k pot d shaft | 5 |
| B100k dual | 1 |
| Jack sockets + nuts | 9 |
| sifam knob d shaft  (small skirt) | 8 |
| sifam knob round shaft  (small skirt) | 1 |
| Lupin cap | 4 |
| Pale blue cap | 2 |
| Black encoder cap | 3 |
| Blue LED | 2 |
| Pink LED | 2 |
| IDC Cable | 1 |

# Tools

You will need:

1. Soldering equipment
2. Crosshead screwdriver
3. Pliers
4. Cutters
5. A computer
6. A cable to connect USB-C devices to your computer


# Build Steps

![PCB Back](../../assets/images/myriad/build/build1_pcbback.jpg)

![PCB Front](../../assets/images/myriad/build/build2_pcbfront.jpg)

## Solder capacitors C27, C31, C32, C34

They are all identical, and non-polarised.  Solder them to the back side of the board.

![Caps](../../assets/images/myriad/build/build3_caps.jpg)

## ELI2040 boards

![ELI2040s](../../assets/images/myriad/build/build4_eli2040s.jpg)

### Clip the male pin headers to 35 pins long

Remove 5 pins from the end.  This can be done by holding the 35th pin with pliers and snapping the other 5 off with your fingers.  Be careful not to snap too many off.


### Clip the female pin headers to 35 pins long

Cut direcly along pin 36, and then carefully trim excess plastic from the end


![Headers](../../assets/images/myriad/build/build5_headers.jpg)

### Mount the 12mm spacers

Use M3 nuts on the front size, and twist the spacers until tight.

![Spacers](../../assets/images/myriad/build/build6_spacers.jpg)

### Mount the ELI2040 boards and screw in the spacer

Place tghe male headers into the female headers and put the female headers onto the back side of the boar in J2 and J3.  Mount the ELI2040 daughter boards on top, and secure to the spacer with an M3 screw.

![Headers](../../assets/images/myriad/build/build7_headers.jpg)

![PCB With Headers](../../assets/images/myriad/build/build8_pcbheaders.png)


### Solder the pins on both sides of the pin headers

![PCB With ELI2040s](../../assets/images/myriad/build/build9_elis.png)


### Unscrew the spacer screws and remove the ELI2040 boards

We'll put them back later, but we need access to the back side for soldering.

![PCB Without ELI2040s](../../assets/images/myriad/build/build10_removeelis.jpg)

## Front side LEDs

Place the two pink LEDs in the top row, and the blue LEDs in the other.  It may help to tape the LEDs in place while you solder to ensure they are straight.

![LEDs](../../assets/images/myriad/build/build11_leds.jpg)

## Screen

Loosely mount the M2 spacers on the front side of the board. 

![Screen Spacers](../../assets/images/myriad/build/build12_screenspacers.jpg)

Place the screen onto the board with the pins going through the holes. Screw and tighten up the M2 screws into the spacers, and then tighten up the nuts on the back side.  Now you can solder the screen pins.  Peel away the plastic screen protector.

![Screen](../../assets/images/myriad/build/build13_screen.jpg)

## Front side components

Mount the dual 100k pot in RV1, and then the 5 100k pots into RV-CV1-4 and RV4_VCA_Shape1. Mount the three rotary encoders at the top, and all the jack sockets. 

![Pots and Jacks](../../assets/images/myriad/build/build14_knobsjacks.jpg)

Place the panel over the front components, and secure (loosely) with the nuts.


Solder the components, and then tighten up the nuts on the front panel.

![Front Panel](../../assets/images/myriad/build/build15_frontpanel.jpg)


## Knobs

Mount the round shaft knob onto the overdrive pot shaft, and then the d-shaft knobs on the other pots.  

![Knobs](../../assets/images/myriad/build/build16_knobs.jpg)

Press in the coloured knob caps.

![Caps](../../assets/images/myriad/build/build17_caps.jpg)

## ELI2040 boards

Remount the daughter boards.

## Firmware

Follow the instructions [here](./firmware.md)

## Calibration

[coming soon]

# Reference Materials

[Schematic PDF](../../assets/myriad/Myriad_0.4_schematic.pdf)


The Kicad project is in our [Git Repo](https://github.com/Emute-Lab-Instruments/Myriad/)



