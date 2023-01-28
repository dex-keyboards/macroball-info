---
title: Build Log
nav_order: 300
---

# Build Log

// TODO: picture

## Motivation

2020 had just finished, Melbourne had emerged from its first year of COVID lockdowns, and it was summer in Australia. I was meeting up with a friend, David (todo handle), another keyboard geek, who I'd been trying to convince of the joys of trackballs. He'd recently borrowed my [Kensington Expert Mouse](https://www.kensington.com/en-au/p/products/control/trackballs/expert-mouse-wired-trackball/) (a trackball). He mentioned that he liked it, but wished it had a bit more travel; meaning he wished he could spin the trackball and see his mouse cursor fly accross the screen; as it was, it was a bit sticky.

I'd just finished up my writeup of my [Oddball keyboard](https://atulloh.github.io/oddball/), so was ready for a new challenge. David mentioned he was interested in starting a keyboard shop; with the stars aligned, we decided to tackle a new project. We'd aim to make a trackball that took on aspects of the mechanical keyboard world:
- Keyboard style switches and keycaps (as opposed to trackball/mouse style micro-switch)
- Premium look and feel; something like CNC'd aluminium
- Standard trackball size (pool ball), so the ball could be easily changed, similar to how keyboard enthusiats like to switch out their keys
- OLED screen for information and feedback
- Encoder to control trackball sensitivity

With that in mind, we ended up effetcively aiming to create a premium style macropad with a trackball.

todo: image/sketch

## Protype week

A week later, my family met David and his partner at the beach. He brought his Planck, and I brought some 3D prints, and we tried to not get sand in anything. I tried out his latest build; it felt good. It was two years ago, but I can still remember the sweet feel of IFK BoH hammering down on aluminium at 32°C/90°F. He tried the 3D prints of the macropad. They were rough, and somehow had got blood on them during assembly; but they also felt good. We decided to forge ahead.

## First prototype build

With a bit of leave and some enthusiam, we got together a couple of PCB designs. We are by no means experts, but between [ai03's](https://www.instagram.com/ai03_2725/) [keyboard design tutorials](https://wiki.ai03.com/books/pcb-design) (I think a few of us have been there), and my past experience with the Oddball, we were able to come up with a solution.

We settled on a pair of PCBs. The smaller board (daugherboard) would connect to the USB cable, and also house the PMW3600 motion sensor to track movement. This would connect via an 8 pin cable to a larger board, which would house the [ATmega32U4](https://www.microchip.com/en-us/product/ATmega32U4) (a commonly used keyboard controller), along with the key switches, OLED display and encoder.

Initially we just sent for the daughterboard, as this was the novel twist on the keyboard design, and we wanted to check it worked. After hooking it up to a Pro Micro, we could seen mouse movement: it was alive!

## Firmware



- Emulator
- Rendering
  - Sprints with alpha
  - Z axis
  - Go off edge of screen
  - Tool to convert images
- Modes
  - Some actually quite useful
  - Mini game

## Second prototype

- LEDs seemed good

## Third prototype

- Tweaks
- 

## Forth prototype

- Hotswap
- 



