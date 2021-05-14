---
layout: projects
categories: project
title: "16x16 RGB LED Matrix"
description: "256 individually addressable LEDs controllable with integrated website."
fontawesome: true
---

Insipired by those expensive [Nanoleafs](https://nanoleaf.me/en-US/) that are popular on Amazon right now, I wanted to make a cool piece built around the rather infamous Neopixel LEDs that I could hang on my wall and amaze my friends.

## Demo
At this point, custom animations can be implemended very easily. I will explore this in later posts. 
I made a quick demo that you [view here](https://www.youtube.com/watch?v=YyiG6wzC1bk){:target="_blank"}, this shows off a quick iOS shortcut I wrote that sends POST requests to the web-server to quickly change the animation from my homescreen. [Ignore the upside-down heart! I was testing bitmaps and got a parameter flipped]

## Assembly
After assembly (see the CAD below -- its not too complicated but feel free to reach out if you have any questions),
I uploaded one of the example sketches from the Adafruit Neopixel library. This interfaced to the 256 LEDS with no issue, drawing about 3.5A at 5V (17.5 Watts). 

![Testing the matrix with Adafruit Example Sketch]({{ site.baseurl }}/images/matrix-test.jpg)

## Firmware
Initally, I began to write my own driving firmware. However, this proved to be extremely difficult to manage on the ESP8266 processor. What needs to be done is a careful juggle between the WiFi Calls and updating the pixels. Since the pixels are bitbanged through a single SPI bus, each 'frame' takes a considerable amount of time to send out. If the WiFi interrupts in the middle, the resulting animation becomes jerky. I haven't gotten into interrupts and priority levels on the ESP8266 yet, but I will explore that approach in the future.

Naturally, I decided to look for similar projects, and I found a fairly bare-bones web server that has an animation framework for FastLED built-in. I ended up using [/stnkl/FastLEDManager](https://github.com/stnkl/FastLEDManager) for the time being. FastLEDManager was built to support 1-dimensional strips of LEDS, so began by adding support to be able to easily address the 2-dimensional topology of this matrix.

In the Animation class, I added a function `topoXY()` that is used to address the 1-D FastLED output buffer in 2-d space. 
This funcion addresses the snaking pattern used to hook up the LEDS. The are addressed sequentially, so we must alternate on odd rows and compute their `x, y` position in reverse. This `returns` the position in the 1-d buffer of any `x, y` coordinate passed in.
```cpp
uint16_t topoXY(uint16_t x, uint16_t y) {
    uint16_t i;
    if( y & 0x01) {
      // Odd rows run backwards
      uint8_t reverseX = (MATRIX_WIDTH - 1) - x;
      i = (y * MATRIX_WIDTH) + reverseX;
    } else {
      // Even rows run forwards
      i = (y * MATRIX_WIDTH) + x;
    }  
  return i;
}
```


## BOM:
- 256 WS2812B LEDs, 60 pixels/m
- 5V 10A DC power supply
- NodeMCU ESP8266 or ESP32
- Scrap cardboard
- 12" x 12" Cardstock or similarly thick paper
- ~12" x 12" Parchmant paper (to diffuse)

The scrap cardboard was laser-cut with the 'Divider' template in the CAD Files below.

More Information
: <i class="fas fa-fw fa-code-branch"></i> [[Github Repo](https://github.com/jamesmendel/16x16-Wall-Matrix){:target="_blank"}]
: <i class="fas fa-fw fa-ruler-combined"></i> [[CAD Files](https://cad.onshape.com/documents/42e5a1dabde14f0667ec5ba2/w/2ec6fdbe93ad883e39ab747d/e/4e9ff106828c43007720c2b8){:target="_blank"}]