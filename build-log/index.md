---
title: Build Log
nav_order: 300
---

# Build Log

// TODO: picture

## Motivation

2020 had just finished, Melbourne had emerged from its first year of COVID lockdowns, and it was summer in Australia. I was meeting up with a friend, David (todo handle), another keyboard geek, who I'd been trying to convince of the joys of trackballs. He'd recently borrowed my [Kensington Expert Mouse](https://www.kensington.com/en-au/p/products/control/trackballs/expert-mouse-wired-trackball/) (a trackball). He mentioned that he liked it, but wished it had a bit more travel- meaning he wished he could spin the trackball and see his mouse cursor fly accross the screen- as it was, it was a bit sticky.

I'd just finished up my writeup of my [Oddball keyboard](https://atulloh.github.io/oddball/), so I was ready for a new challenge. David mentioned he was interested in starting a keyboard shop. With the stars aligned, we decided to tackle a new project. We'd aim to make a trackball that took on aspects of the mechanical keyboard world:
- Keyboard style switches and keycaps (as opposed to trackball/mouse style micro-switch)
- Premium look and feel; something like CNC'd aluminium
- Standard trackball size (pool ball), so the ball could be easily changed, similar to how keyboard enthusiats like to switch out their keys
- OLED screen for information and feedback
- Encoder to control trackball sensitivity

With that in mind, we ended up effetcively aiming to create a premium style macropad with a trackball.

![Concept 1]({{site.baseurl}}/assets/images/sketch-1.jpg)

## Protype week

A week later, my family met David and his partner at the beach. He brought his [Planck](https://olkb.com/collections/planck), and I brought some 3D prints, and we tried to not get sand in anything. I tried out his latest build; it felt good. It was two years ago, but I can still remember the feel of IFK BoH hammering down on aluminium at 32°C/90°F. He tried the 3D prints of the macropad. They were rough, and somehow had got blood on them during assembly; but they also felt good. We decided to forge ahead.

<figure>
  <img src="{{site.baseurl}}/assets/images/proof-of-concept.jpg" alt="Proof of concept"/>
  <figcaption>3D draft print to check the feel</figcaption>
</figure>

## First prototype build

With a bit of leave and some enthusiam, we got together a couple of PCB designs. We are by no means experts, but between [ai03's](https://www.instagram.com/ai03_2725/) [keyboard design tutorials](https://wiki.ai03.com/books/pcb-design) (I think a few of us have been there), and my past experience with the Oddball, we were able to come up with a solution.

We settled on a pair of PCBs. The smaller board (daugherboard) would connect to the USB cable, and also house the PMW3600 motion sensor to track movement. This would connect via an 8 pin cable to a larger board, which would house the [ATmega32U4](https://www.microchip.com/en-us/product/ATmega32U4) (a commonly used keyboard controller), along with the key switches, OLED display and encoder.

Initially we just sent for the daughterboard, as this was the novel twist on the keyboard design, and we wanted to check it worked. After hooking it up to a Pro Micro, we could seen mouse movement: it was alive!

<figure>
  <img src="{{site.baseurl}}/assets/images/daughterboard-1.jpg" alt="Daughterboard prototype"/>
  <figcaption>3D draft print to check the feel</figcaption>
</figure>

<figure>
  <video src="{{site.baseurl}}/assets/videos/daughterboard-in-action.mp4" controls preload></video>
  <figcaption>Daughtboard prototype in action</figcaption>
</figure>

## Firmware

At some point (2021 is a blur), my 3D printer had a few health issues, so I turned to the firmware. I leveraged the open source [QMK](https://qmk.fm/) firmware/code, which is easy to modify and extend.

<figure>
  <img src="{{site.baseurl}}/assets/images/oled-1.jpg" alt="Rendering with QMK"/>
  <figcaption>Rendering text with QMK</figcaption>
</figure>

I started by trying to get a few simple words to display- it worked fine. Next I tried to get some images to display. This was a bit more fiddly, as the bytes of each image had to be in a certain format. I spent a while trying to optimise the way the pixels were stored and rendered.

To speed up the process, I made a little emulator for QMKs draw calls, so I could just test/see how my images and animations looked on my PC, without having to worry about compiling and flashing the firmware to the device.

<figure>
  <img src="{{site.baseurl}}/assets/images/oled-emulator.png" alt="OLED emulator"/>
  <figcaption>Simple emulator to speed up iteration, and saving me having to flash controller every time</figcaption>
</figure>

I also wanted to have some sort of animation- maybe a game- for fun, so I did a little work on the sprite rendering:
- Allowed sprites to have an "alpha" (see through) channel
- Allowed sprites to render partially off the screen (sounds super trivial, but doesn't come out-of-the-box)
- Simulated a little XYZ coordinate system, so you can move sprites around, and in front of other sprites
- Added a sprite and animation format that was relatively lean, and made a little program that could spit these out

All in all, this was probably overkill, especially as it would have just been simpler to get a better processor (ATMega32s aren't super powerful) and do things more simply, but the constraints were fun, and it worked in the end.

<figure>
  <img src="{{site.baseurl}}/assets/images/oled-sprites.jpg" alt="Sprites"/>
  <figcaption>Sprites</figcaption>
</figure>

<figure>
  <video src="{{site.baseurl}}/assets/videos/oled-animations.mp4" controls preload></video>
  <figcaption>Animations</figcaption>
</figure>

I then added a few things that were _actually_ useful: a volume control, a CPI (trackball sensitivity) control, a trackball scroll sensitivity control, and of course, a minigame.

Lastly, I cleaned up the code a bit, and split the functionality out into "modes". These are just little files and functions that say what the OLED and keyboard should do (e.g. "volume mode"). They can be switched (currently cycled with the encoder click), to jump to the next mode. I added a little transtition animation to make it look nice.

// TOOD: compress and show functionality

## Plate and cutouts

We opted to go for an integrated plate. Putting a separate plate into the build would've incrased the price for a small run, as more parts add more setup costs. The unit's plate design, being small and u-shaped, also some of the benefits of a separate plate, being quite rigid and not able to add much in terms of flex.

Going with an integrated plate, CNC'd out of aluminium, gave us the option to go with quite a hefty plate, 4mm thick. This was nice as it gave a bit more density to such a small unit. I followed the specifications from [Cherry](https://www.cherrymx.de/en/dev.html) on their MX switches, along with some advice from the community, to get the switch and stablizer cutouts a tight fit.

<figure>
  <img src="{{site.baseurl}}/assets/images/plate-scad.png" alt="Plate SCAD"/>
  <figcaption>Plate in OpenSCAD</figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/plate-scad-closeup.png" alt="Plate SCAD closeup"/>
  <figcaption>Plate switch cutouts are quite detailed, to give the switches a tight fit</figcaption>
</figure>

### OLEDs

Back in 2021, lots of OLED usage had moved on from backlighting and underglow, to bands and highlights.

<figure>
  <img src="{{site.baseurl}}/assets/images/highfinger-example.jpg" alt="Highfinger Example"/>
  <figcaption>The highfinger as an example: containing a vertical, translucent stript for LEDs to shine through</figcaption>
</figure>

So we decided to put our spin on it: a glowing ring around the trackball. While adding a few few complications, and added a new part of CNC'd polycarbonate, it added a bit of fun and flair.

<figure>
  <img src="{{site.baseurl}}/assets/images/daughterboard-2.jpg" alt="Proto glow open"/>
  <figcaption>A larger version of the daughterboard, now housing LEDs along with the trackball sensor</figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/glow-1.jpg" alt="Proto glow"/>
  <figcaption>An early prototype with </figcaption>
</figure>

// final pic

## Mounting

Mounting the trackball was also a little tricky to get right. In the spirit of mechanical keyboards and their customisation, we decided to give a couple of different options.

### Static

// few comments/pics

### Dynamic

// few comments/pics

## Builds

### Glow

### blue

### final


