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
| A100k dual | 1 |
| Jack sockets + nuts | 9 |
| sifam knob d shaft  (small skirt) | 8 |
| sifam knob T18 shaft  (small skirt) | 1 |
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


# Build Steps

![PCB Back](../../assets/images/myriad/build/build1_pcbback.jpg)
PCB Back

![PCB Front](../../assets/images/myriad/build/build2_pcbfront.jpg)
PCB Front

## [1] Solder capacitors C27, C31, C32, C34

They are all identical, and non-polarised.  Solder them to the back side of the board.

![Caps](../../assets/images/myriad/build/build3_caps.jpg)

## [2] ELI2040 boards

![ELI2040s](../../assets/images/myriad/build/build4_eli2040s.jpg)

One of the boards has a green dot on it; this is the Myriad A unit, it should be placed on the left side of the back of the board. The Myriad B unit, without a green dot, goes on the right hand side.

### [3] Clip the male pin headers to 35 pins long

Remove 5 pins from the end.  This can be done by holding the 35th pin with pliers and snapping the other 5 off with your fingers.  Be careful not to snap too many off.


### [4] Clip the female pin headers to 35 pins long

Cut direcly along pin 36, and then carefully trim excess plastic from the end


![Headers](../../assets/images/myriad/build/build5_headers.jpg)

### [5] Mount the 12mm spacers

Use M3 nuts on the front size, and twist the spacers until tight.

![Spacers](../../assets/images/myriad/build/build6_spacers.jpg)
![Spacers](../../assets/images/myriad/build/build6_spacers_front.jpg)

### [6] Mount the ELI2040 boards and screw in the spacer

Place the male headers into the female headers and put the female headers onto the back side of the boar in J2 and J3.  Mount the ELI2040 daughter boards on top, and secure to the spacer with an M3 screw.

![Headers](../../assets/images/myriad/build/build7_headers.jpg)

![PCB With Headers](../../assets/images/myriad/build/build8_pcbheaders.png)


### [7] Solder the pins on both sides of the pin headers

![PCB With ELI2040s](../../assets/images/myriad/build/build9_elis.png)


### [8] Unscrew the spacer screws and remove the ELI2040 boards

We'll put them back later, but we need access to the back side for soldering.

![PCB Without ELI2040s](../../assets/images/myriad/build/build10_removeelis.jpg)

## [9] Front side LEDs

Place the two pink LEDs in the top row, and the blue LEDs in the other. These should be flush to the PCB. It may help to tape the LEDs in place while you solder to ensure they are straight.

![LEDs](../../assets/images/myriad/build/build11_leds.jpg)

## [10] Screen

Loosely mount the M2 spacers on the front side of the board. 

![Screen Spacers](../../assets/images/myriad/build/build12_screenspacers.jpg)

Place the screen onto the board with the pins going through the holes. Screw and tighten up the M2 screws into the spacers, and then tighten up the nuts on the back side.  

![Screen](../../assets/images/myriad/build/build13_screen.jpg)

Now you can solder the screen pins.  Peel away the plastic screen protector.

![Screen](../../assets/images/myriad/build/build13_screen_pins.jpg)

## [11] Front side components

Mount the dual 100k pot in RV1, and then the 5 100k pots into RV-CV1-4 and RV4_VCA_Shape1. Mount the three rotary encoders at the top, and all the jack sockets. 

![Pots and Jacks](../../assets/images/myriad/build/build14_knobsjacks.jpg)

Place the panel over the front components, and secure (loosely) with the nuts.


Solder the components, and then tighten up the nuts on the front panel.

![Soldered Pots and Jacks - Back](../../assets/images/myriad/build/build14_soldered.jpg)

![Front Panel](../../assets/images/myriad/build/build15_frontpanel.jpg)




## [12] Knobs

Mount the T18 shaft knob onto the overdrive pot shaft, and then the d-shaft knobs on the other pots.  

![Knobs](../../assets/images/myriad/build/build16_knobs.jpg)

Press in the coloured knob caps.

![Caps](../../assets/images/myriad/build/build17_caps.jpg)

## [13] ELI2040 boards


Remount the ELI2020 boards, making sure the unit with the green dot is on the left.

![Front Panel](../../assets/images/myriad/build/build_elis_back_on.jpg)


# Reference Materials

[Schematic PDF](../../assets/myriad/Myriad_0.5_schematic.pdf)


The Kicad project is in our [Git Repo](https://github.com/Emute-Lab-Instruments/Myriad/)



