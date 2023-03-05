---
title: Build Log
nav_order: 300
---

# Build Log

![]({{site.baseurl}}/assets/images/DEX Blue_06.jpg)

## Motivation

2020 had just finished, Melbourne had emerged from its first year of COVID lockdowns, and it was summer in Australia. I was meeting up with a friend, David (todo handle), another keyboard fan, who I'd been trying to convince of the joys of trackballs. He'd recently borrowed my [Kensington Expert Mouse](https://www.kensington.com/en-au/p/products/control/trackballs/expert-mouse-wired-trackball/) (a trackball). He mentioned that he liked it, but he wished it had a bit more travel- meaning he wished he could spin the trackball and see his mouse cursor fly accross the screen- as it was, it was a bit "sticky".

I'd just finished up my writeup of my [Oddball keyboard](https://atulloh.github.io/oddball/), so I was ready for a new challenge. David mentioned he was interested in starting a keyboard shop. With the stars aligned, we decided to tackle a new project. We'd aim to make a trackball that took on aspects of the mechanical keyboard world:
- Keyboard style switches and keycaps (as opposed to trackball/mouse style micro-switch)
- Premium look and feel; something like CNC'd aluminium
- Standard trackball size (pool ball), so the ball could be easily changed, similar to how keyboard enthusiats like to switch out their keys
- OLED screen for information and feedback
- Encoder to control trackball sensitivity

**TL;DR: we were aiming to create a premium style macropad with a trackball.**

<figure>
  <img src="{{site.baseurl}}/assets/images/sketch-1.jpg" alt="Concept"/>
  <figcaption>First ideas for a macropad with a giant trackball</figcaption>
</figure>

## Protype week

A week later, my family met David and his partner at the beach. He brought his [Planck](https://olkb.com/collections/planck), and I brought some 3D prints, and we tried to not get sand in anything. I tried out his latest build; it felt good. It was two years ago, but I can still remember the feel of IFK BoH hammering down on aluminium at 32°C/90°F. He tried the 3D prints of the macropad. They were rough, and somehow had got blood on them during assembly; but they also felt good. We decided to forge ahead.

<figure>
  <img src="{{site.baseurl}}/assets/images/proof-of-concept.jpg" alt="Proof of concept"/>
  <figcaption>3D draft print to check the layout and feel</figcaption>
</figure>

## First prototype build

With a bit of leave and some enthusiam, we got together a couple of PCB designs. We are by no means experts, but between [ai03's](https://www.instagram.com/ai03_2725/) [keyboard design tutorials](https://wiki.ai03.com/books/pcb-design) (I think a few of us have been there), and my past experience with the Oddball, we were able to come up with a solution.

We settled on a pair of PCBs. The smaller board (daugherboard) would connect to the USB cable, and also house the PMW3600 motion sensor to track movement. This would connect via an 8 pin cable to a larger board, which would house the [ATmega32U4](https://www.microchip.com/en-us/product/ATmega32U4) (a commonly used keyboard controller), along with the key switches, OLED display and encoder.

Initially we just sent for the daughterboard, as this was the novel twist on the keyboard design, and we wanted to check it worked. After hooking it up to a Pro Micro, we could seen mouse cursor movement: it was alive!

<figure>
  <img src="{{site.baseurl}}/assets/images/daughterboard-1.jpg" alt="Daughterboard prototype"/>
  <figcaption>Daughterboard v1 with USB C and a PMW3360 sensor</figcaption>
</figure>

<video src="{{site.baseurl}}/assets/videos/daughterboard-in-action.mp4" controls preload></video>

## Firmware

At some point (2021 is a blur), my 3D printer had a few health issues, so I decided to work on the firmware for a bit. I leveraged the open source [QMK](https://qmk.fm/) firmware/code, which is easy to modify and extend.

<figure>
  <img src="{{site.baseurl}}/assets/images/oled-1.jpg" alt="Rendering with QMK"/>
  <figcaption>Step 1: Rendering text with QMK</figcaption>
</figure>

I started by trying to get a few simple words to display- it worked fine. Next I tried to get some images to display. This was a bit more fiddly, as the bytes of each image had to be in a certain format. I spent a while trying to optimise the way the pixels were stored and rendered.

To speed up the process, I made a little emulator for QMKs draw calls, so I could just test/see how my images and animations looked on my PC, without having to worry about compiling and flashing the firmware to the device.

<figure>
  <img src="{{site.baseurl}}/assets/images/oled-emulator.png" alt="OLED emulator"/>
  <figcaption>Simple QMK OLED emulator to speed up iteration, and saving me having to flash controller every time</figcaption>
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

<video src="{{site.baseurl}}/assets/videos/oled-animations.mp4" controls preload></video>

I then added a few things that were _actually_ useful: a volume control, a CPI (trackball sensitivity) control, a trackball scroll sensitivity control, and of course, a minigame.

Lastly, I cleaned up the code a bit, and split the functionality out into "modes". These are just little files and functions that say what the OLED and keyboard should do (e.g. "volume mode"). They can be switched (currently cycled with the encoder click), to jump to the next mode. I added a little transtition animation to make it look nice.

<video src="{{site.baseurl}}/assets/videos/oled-functionality.mp4" controls preload></video>

## Plate and cutouts

We opted to go for an integrated plate. Putting a separate plate into the build would've increased the price for a small run, as more parts add more setup costs. The unit's plate design, being small and u-shaped, also some of the benefits of a separate plate, being quite rigid and not able to add much in terms of flex.

Going with an integrated plate, CNC'd out of aluminium, gave us the option to go with quite a hefty plate, 4mm thick. This was nice as it gave a bit more density to such a small unit. I followed the specifications from [Cherry](https://www.cherrymx.de/en/dev.html) on their MX switches, along with some advice from the community, to get the switch and stablizer cutouts a tight fit.

<figure>
  <img src="{{site.baseurl}}/assets/images/plate-scad.png" alt="Plate SCAD"/>
  <figcaption>Plate in OpenSCAD</figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/plate-scad-closeup.png" alt="Plate SCAD closeup"/>
  <figcaption>Plate switch cutouts are quite detailed, to give the switches a tight fit</figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/cutouts-cnc.jpg" alt="Proto cutouts"/>
  <figcaption>The final switch cutouts were complex, but came out rather nicely</figcaption>
</figure>

## Mounting

Mounting the trackball was also a little tricky to get right. In the spirit of mechanical keyboards and their customisation, we decided to give a couple of different options. First, a static option, where three, smooth (e.g. silicon nitride, POM, etc), small balls are fixed to the main case with bolts, against which the trackball slides past. The second option was to insert threaded ball transfer units (BTUs), which are small balls that can spin freely atop several bearing, allowing the trackball to spin for quite a while, at the cost of noise.

<figure>
  <img src="{{site.baseurl}}/assets/images/mounting-1.jpg" alt="Mounting Static"/>
  <figcaption>6 cutouts: 3 for static balls, 3 for the BTUs </figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/mounting-2.png" alt="Mounting SCAD"/>
  <figcaption>Prototyping attaching silicon nitride balls</figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/mounting-3.jpg" alt="Mounting Dynamic"/>
  <figcaption>Prototyping attaching threaded BTUs</figcaption>
</figure>

### LEDs

Back in 2021, lots of LED usage had moved on from backlighting and underglow, to bands and highlights.

<figure>
  <img src="{{site.baseurl}}/assets/images/highfinger-example.jpg" alt="Highfinger Example"/>
  <figcaption>The highfinger as an example: containing a vertical, translucent stript for LEDs to shine through</figcaption>
</figure>

So we decided to put our spin on it: a glowing ring around the trackball. While adding a few few complications, and added a new part, a ring of CNC'd polycarbonate, it also added a bit of fun and flair.

<figure>
  <img src="{{site.baseurl}}/assets/images/daughterboard-2.jpg" alt="Proto glow open"/>
  <figcaption>A larger version of the daughterboard (right), now housing LEDs along with the trackball sensor</figcaption>
</figure>

<video src="{{site.baseurl}}/assets/videos/glow-spin.mp4" controls preload></video>

<figure>
  <img src="{{site.baseurl}}/assets/images/blue-build-2.jpg" alt="Blue build 2"/>
  <figcaption>In one build I tried using silicone for the diffuser, which was quite messy</figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/blue-build-3.jpg" alt="Blue build 3"/>
  <figcaption>The silicone ring was a nice finish, but didn't diffuse the light too well</figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/led-final.jpg" alt="LED ring"/>
  <figcaption>The final version diffused the LEDs through a wider, PC ring looked much better</figcaption>
</figure>

## Builds

<figure>
  <img src="{{site.baseurl}}/assets/images/build-1.jpg" alt="Early build"/>
  <figcaption>Early build printed with the PLA I had available. The translucent bottom piece actually looked kind of cool.</figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/blue-build-1.jpg" alt="Blue build"/>
  <figcaption>Putting together a "nicer" prototype build</figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/blue-build-1.jpg" alt="Blue build 1"/>
  <figcaption>A "nicer" 3DP build: paiting</figcaption>
</figure>

## Manufacturing

This was the first time I'd designed something for CNC, so it was a bit of a learning curve. For most of the 3D work I'd used OpenSCAD, which is great for being really explicit about measurements, as everything is in code, but it doesn't export to STEP, which is a pretty useful file format for CNC machines. OpenSCAD also has a pretty limited feature set around describing bevels, chamfers, etc; if you want to pursue these, you have to write some pretty hairy code.

I ended up using [FreeCAD](https://www.freecad.org/) as a stepping stone. FreeCAD has a module that allows that allows import of SCAD files, which I could then layer on some nice transfers to smooth out the hard corners, and then export to STEP, which I could then in turn send to the manufacturer.

<figure>
  <img src="{{site.baseurl}}/assets/images/schematic.png" alt="Schematic"/>
  <figcaption>Creating files for manufacturing was new to me</figcaption>
</figure>

The final prototype came out pretty well. There are a few issues (I had to dremel the PC ring a little to fit 😬) that we'll need to tweak before a final production run. But for a first go, I was quite relieved that generally it all fit together and worked.

<figure>
  <img src="{{site.baseurl}}/assets/images/cutouts-mounted.jpg" alt="Proto mounting"/>
  <figcaption>The BTUs screwed in very snugly</figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/usb-cutout.jpg" alt="USB cutout"/>
  <figcaption>The USB cutout and port recession was perfect</figcaption>
</figure>

<figure>
  <img src="{{site.baseurl}}/assets/images/proto-pre-photo.jpg" alt="Protos together"/>
  <figcaption>A happy pair</figcaption>
</figure>

