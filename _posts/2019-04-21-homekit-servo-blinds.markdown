---
layout: post
title:  "Homekit Servo Blinds"
date:   2019-04-21 12:15:40 -0700
categories: side-projects
tags: []
image: blinds.png
imageAlt: "homekit blinds UI"
---

I have little experience with hardware, but had a goal that I wanted to retrofit my completely manually operated blinds so that I could control them via [HomeKit](https://developer.apple.com/homekit/). This post is about how I accomplished exactly that! The final solution is controllable down to a percentage via siri and the home app.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Best thing I’ve done all year. <a href="https://twitter.com/hashtag/HomeKit?src=hash&amp;ref_src=twsrc%5Etfw">#HomeKit</a> <a href="https://twitter.com/hashtag/homebridge?src=hash&amp;ref_src=twsrc%5Etfw">#homebridge</a> <a href="https://t.co/4uWfUkBkt0">pic.twitter.com/4uWfUkBkt0</a></p>&mdash; Zeke (@ZekeSnider) <a href="https://twitter.com/ZekeSnider/status/1118720403424153600?ref_src=twsrc%5Etfw">April 18, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

# The Equipment
+ [Raspberry Pi 3](https://www.amazon.com/gp/product/B07BDR5PDW/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B07BDR5PDW&linkCode=as2&tag=zeke082-20&linkId=878ec655cdbf6a40bf226475b7c170d3)
	+ Alternatively, you can use a [Raspberry Pi Zero](https://www.amazon.com/gp/product/B06XFZC3BX/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B06XFZC3BX&linkCode=as2&tag=zeke082-20&linkId=b2b6e81aae6b419ae2449eb99dce3a82) with a headboard soldered on (or solder directly)
+ [360° servo](https://www.amazon.com/gp/product/B07F9P32PF/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B07F9P32PF&linkCode=as2&tag=zeke082-20&linkId=27ce384d7acc170d42d9cb6a80122fd6)
	+ Make sure you get a 360° servo.
+ [Jumper wires](https://www.amazon.com/gp/product/B072L1XMJR/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B072L1XMJR&linkCode=as2&tag=zeke082-20&linkId=c85aa8e645e3a2830e4380c1ca382642)
+ [Screwdriver extension](https://www.amazon.com/gp/product/B0012Q54KW/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B0012Q54KW&linkCode=as2&tag=zeke082-20&linkId=61790ac13b129e3531c8635082b29393)
	+ This one is overkill for this use case, but I figure they'd be useful to have anyway.
+ [Command strip](https://www.amazon.com/gp/product/B01FEJ3OA4/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B01FEJ3OA4&linkCode=as2&tag=zeke082-20&linkId=adaae4c8e3592d64ceffe3cce58ab598)
+ [Wand grip](https://blindparts.com/product/vertical-blind-wand-grip/)
	+ Note: I haven't tested this one specifically because my blind already had a grip. I ordered a few of these and am planning on testing them with my other blinds.

(Note: the amazon links are affiliate links.)

# The hardware
{% include sideByImage.html file="originalBlinds.jpeg" description="The original blinds" %}

The specifics will obviously differ based on what type of blinds you have. My blinds are twist blinds, which conveniently have a removable grip on the bottom of the rod, with has a hole on the bottom. I noticed this, and thought it would be perfect for screwing to a servo.

{% include fullWidthImage.html file="screwdriver.jpeg" description="screwing into the servo" %}

You'll need a screwdriver extension to properly screw into the servo through the grip. My extension was overkill on length, but got the job done.

{% include fullWidthImage.html file="screwed.jpeg" description="now how to attach to the wall" %}

I attached the grip back onto the blind rod, and we're almost good to go. Except I needed to attach it to the wall, and still have the flexibility to remove it in case I need to manually slide the blinds. I'm also renting this apartment so I didn't want a permanent solution like screwing into the wall.

{% include fullWidthImage.html file="commandstrips.jpeg" description="modern problems require modern solutions" %}

Command strips were a good solution for me. I'm sure this is super amateur hour, but it worked for my use case.

{% include fullWidthImage.html file="attached.jpeg" description="servo attached to wall" %}

I then plugged in my raspberry pi to the wall (running raspbian), and wired up the servo to the pi using the [wiring guide here](https://github.com/fivdi/pigpio#servo-control).


# The software

Because my ultimate goal was to connect to homekit, I installed [homebridge](https://github.com/nfarina/homebridge), and [set it up to run in the background](/running-homebridge-in-background/). Then I stumbled upon the [homebridge-minimal-http-blinds plugin](https://github.com/Nicnl/homebridge-minimal-http-blinds), which allowed me to bind a blinds accessory to an http server. So then I set out to write a server that exposed the correct endpoints.

I ended up writing it in Node, and using [pigpio](https://github.com/fivdi/pigpio) to control the servo. You can see (and use) the source [here](https://github.com/ZekeSnider/ServoBlinds). The nice thing about this solution is it allowed me to implement percentage based setting of the blinds. 

The server's implementation also includes rubber-banding on request. So for example, if the blinds are at 0%, you request 100% then request 20% after they reach 50%, they will immediately turn back. Newest request takes priority, and it checks which direction it should be turning [on every iteration of the control loop](https://github.com/ZekeSnider/ServoBlinds/blob/master/blinds.js#L83).

After much tinkering via HTTP requests to determine the correct [config parameters](https://github.com/ZekeSnider/ServoBlinds/blob/master/config.json), I arrived at a final set of values. Then I setup my application to run persistently using systemd, and updated my homebridge config file to point at the [new accessory](https://github.com/ZekeSnider/ServoBlinds/blob/master/accessoryConfig.json). Afterwards it was all good to go.

I'm very happy with my final product and am currently working on implementing on the other blinds in my apartment!
