---
title: "Turning a dead 2015 27-inch iMac into a 5K monitor"
slug: "imac-27-2015-to-5k-monitor-r1811"
date: 2025-09-02
lastmod: 2026-11-03
draft: false
# Staging: reachable by URL, hidden from lists, feeds and sitemap. Remove this block to publish.
build:
  list: never
  render: always
sitemap:
  disable: true
description: "How I converted a 120 euro 2015 iMac 27 (A1419) with a dead logic board into a 5K monitor with a Stonetaskin R1811 driver board, including the mistakes, the flicker trap with USB-C power, and what it all cost."
summary: "I bought a Late 2015 iMac 27 with a dead logic board for 120 euros on Wallapop and turned it into a 5K monitor with an R1811 driver board, saving at least 1,000 euros over a new 5K display. Written as I went, failures included: the recessed T8 screw, the backlight cable orientation, the USB-C power flicker, and the pink edges on 2015 panels."
tags: ["imac", "5k", "r1811", "monitor", "diy", "3d-printing"]
categories: ["Making"]
showTableOfContents: true
showHero: false
---

This is how I converted a 120 euro ($140) Late 2015 iMac 27 inch into a 5K monitor, and saved myself at least 1,000 euros ($1,200). The write-up started life as [a README on GitHub](https://github.com/kurtshuler/imac-5k-monitor-conversion); this is the same material, tidied up.

It is based on the work of the community at the [MacRumors DIY 5K Monitor Success thread](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/).

{{< figure src="img/2015-imac-27.jpg" alt="A Late 2015 27-inch iMac on a white sheet, screen off" caption="From this: the \"Mystery Mac\", a 2015 iMac 27 with a dead logic board, 120 euros delivered." figureClass="fig-right fig-40" >}}

## Thank you to the DIY 5K community

This project would not exist without the tinkerers in the [MacRumors DIY 5K Monitor Success thread](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/). The contributors whose knowledge helped me the most:

- [**@PaulD-UK**](https://forums.macrumors.com/members/pauld-uk.374804/): too many great posts to count. Countless diagrams, wiring tips and practical wisdom.
- [**@Regulus67**](https://forums.macrumors.com/members/regulus67.1356758/): always helpful answering questions, and very patient with newbies.
- [**@Aiwi**](https://forums.macrumors.com/members/aiwi.505077/): deep dives and detailed project logs that demystified the R1811. His own build is on GitHub at [aiwipro/5K-iMac-Studio-Display-Stock-Look](https://github.com/aiwipro/5K-iMac-Studio-Display-Stock-Look).
- [**@ItsAShaunParty**](https://forums.macrumors.com/members/itsashaunparty.823966/): created the [DIY 5K at MacRumors GPT](https://chatgpt.com/g/g-683af4dc61448191ad6868a38e8cb1b5-diy-5k-monitor-gpt) for ChatGPT.

If you are diving into this project, [browse the full thread](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/) before asking people questions.

## How this write-up works

Rather than show my successes and say "Tada!" like a magician pulling a rabbit out of a hat, I wrote this as I was doing the project, failures and false paths included. ~~Strikethrough text~~ means I made a mistake or chose a different path. Hopefully that is more useful to others than a completely linear success story.

{{< clear >}}

## Why I did this

I recently bought a 3D printer and went looking for a larger monitor to use with my 2020 MacBook Pro M1 and Fusion 360. There are lots of affordable 4K monitors, but the scaling of text and details on them has always looked off to me on a Mac. So I started looking for 5K monitors and was surprised to find... not much. What exists is hugely expensive. While searching for 5K monitor reviews I found the MacRumors thread, and my life changed for the better.

I searched for used 27-inch iMacs on Wallapop, Spain's eBay. Imagine my surprise when I found a Late 2015 iMac 27 (A1419) with a dead logic board for 100 euros. The seller said the screen worked fine before the logic board died, and that a shop had quoted a repair that was too expensive. I took a gamble, betting that both the seller and the technician were honest.

{{< figure src="img/wallapop-receipt.jpg" alt="Wallapop order: iMac 27 2015 (Apple) - Piezas, transaction completed 19 Aug 2025, 120.28 euros" caption="What I actually paid: 120.28 euros including Wallapop's shipping and fees, 19 August 2025." >}}

When the package arrived I found an `LM270QQ1 (SD)(B1)` panel. Of course, without a working logic board or a display driver board I had no idea whether it worked. I called my new possession the "Mystery Mac".

{{< pair src1="img/panel-model-number.jpg" alt1="Label on the back of the LG panel reading LM270QQ1 (SD)(B1)" cap1="The Mystery Mac's panel model number." src2="img/2015-imac-27.jpg" alt2="The iMac before gutting" cap2="The Mystery Mac before gutting. At this point I had no idea if the panel would work." >}}

## What did I want from it?

With the iMac bought and the panel identified, I had to choose a driver board and the rest of the parts. Before that, the requirements:

1. **No audio.** I did not want to enable the iMac's built-in speakers. People on the forum were having issues with them, and I am designing a 3D-printed 2.1 sound system anyway.
2. **No webcam or mic.** I already have an Obsbot Meet SE webcam with better video than the iMac's internal camera, and the forum members seemed to have a lot of trouble making a good internal mic solution. Little value for a lot of work.
3. **No USB-C charging.** I have a CalDigit TS3 Plus dock and want the dock to charge the Mac. The catch is that the dock can only pass 5K through its USB-C / Thunderbolt ports, and I did not know whether that would make the driver board try to charge the Mac through USB-C Power Delivery. If so, I would connect the Mac to the monitor with a separate DisplayPort 1.4 cable.
   - What I learned later: in the *Mac to Thunderbolt cable to monitor* direct configuration, with no dock, the driver board does try to power the Mac, and the supplied power supply is too weak for that (see the flicker section below). **Connected directly, I need a USB-C to DisplayPort cable from the Mac to the board.** Confirmed only for the direct configuration.
   - The *Mac to CalDigit dock to Thunderbolt cable to monitor* configuration still needs testing.
4. **Silent.**
   - **I kept the iMac's internal case fan.** The case was designed to pull air in through the back with the centrifugal fan and push it out the bottom, which helps cool every board inside. The cons: it needs a 24 V to 12 V step-down converter, and it limits where the R1811 can go if the inputs are to stay accessible. I already had a [Noctua NA-FC1](https://noctua.at/en/na-fc1) fan controller to dial the RPM down to silent.
   - **The R1811's own fan gets a resistor.** The board ships with a fan that people find noisy. I tested [Noctua NA-RC7 Low-Noise Adaptors](https://noctua.at/en/na-src7) from another project and they work; I used 180 ohm 2 A resistors that I have on hand to replace them.
   - **External power supply**, so it does not heat the inside of the case. Stonetaskin's is **24 V 5 A** with a **5.5 mm / 2.5 mm barrel plug**. Charging a laptop through USB-C PD would need more amperage, and I do not want that anyway.
5. **IR remote.** I hate reaching around a display to change settings. I want a remote I can point at the front to reach the driver board's OSD.

## Which driver board, and from whom?

Bottom line up front: the [R1811 V4 with DP 1.4 and HDMI 2.1](https://stonetaskin.com/products/diy-5k-universal-r1811-v-4-driver-board-usb-c-freesync-edp-dp1-4-compatible-for-imac-lm270qq1-lm270qq2-screen-external-monitor-1) and the companion [DZ-LP0818 50 W constant current board](https://stonetaskin.com/products/high-quality-diy-universal-driver-board-50w-current-board-dz-lp0818-resolve-the-panel-brightness-problem-lm270qq1-lm270qq2-1?variant=45363844120730), both from [Stonetaskin](https://stonetaskin.com).

Why the R1811, even though it is the most expensive driver board:

1. It gets more frequent firmware updates than the other boards.
2. Users in the thread report fewer issues and quirks with it.
3. It comes with an IR remote and a power supply.
4. The optional DZ-LP0818 board can raise the backlight brightness if needed. I bought it because I did not know how many hours the 2015 panel had on it, or whether the backlight had dimmed.

I bought from Stonetaskin because of their reputation on the forum, and the decision was confirmed when their online chat turned out to be quick and helpful during the order. The two boards arrived in Spain 8 days after ordering.

## What did it all cost?

Prices all-in. Shipping from China was free.

| Item | Price |
|---|---|
| Late 2015 iMac 27 A1419, dead logic board, Wallapop, delivered | 120.28 € |
| [Stonetaskin R1811 V4 driver board, Package B](https://stonetaskin.com/products/diy-5k-universal-r1811-v-4-driver-board-usb-c-freesync-edp-dp1-4-compatible-for-imac-lm270qq1-lm270qq2-screen-external-monitor-1) (board, control strip, IR remote, 24 V 5 A supply, cables) | $298.00 |
| [DZ-LP0818 50 W constant current board](https://stonetaskin.com/products/high-quality-diy-universal-driver-board-50w-current-board-dz-lp0818-resolve-the-panel-brightness-problem-lm270qq1-lm270qq2-1?variant=45363844120730) | $36.30 |
| Barrel splitter, 24 V to 12 V step-down converter, JST connector kit (Amazon.es) | about 25 € |
| Two 180 ohm 2 A resistors for the R1811 fan | 0.50 € |
| Noctua NA-FC1 fan controller | already owned |

Call it 450 euros for a 5K display. A new 5K monitor starts at four times that.

## Gutting the iMac

I pulled out nearly everything, because I am not reusing the speakers. The case did not come with a RAM cover, so I will 3D print one later. I removed the AC power socket because the power brick stays outside the case.

1. **I kept the case fan** and soldered longer wires onto its leads for the Noctua NA-FC1 controller.
2. **I left the power button** and extended its wires, to attach later to the on/off button of the R1811 control strip.

The teardown itself is on iFixit: [iMac Intel 27-inch Retina 5K Display logic board replacement](https://www.ifixit.com/Guide/iMac+Intel+27-Inch+Retina+5K+Display+Logic+Board+Replacement/30529). An iFixit toolkit was very helpful. You need **T5, T8, T10 and T25** drivers plus an **extension**, for one screw that sits below the logic board and is reached through a hole in it. On the A1419 that screw is a **T8, not the T10 the iFixit guide says**. There is more on this in the MacRumors thread.

{{< pair src1="img/board-speakers-ps.jpg" alt1="iMac interior with speakers and power supply removed, the recessed logic board screw location circled" cap1="Speakers and power supply out. The pesky T8 screw location is circled." src2="img/logic-board-screw.jpg" alt2="Close-up of the recessed T8 logic board screw through its access hole" cap2="The recessed T8 logic board screw." >}}

While pulling parts I noticed that both the logic board and the power supply had damage that looked like arcing. Now I was not so sure the panel would work.

{{< figure src="img/imac-case-empty.jpg" alt="Empty iMac case with only the fan and power button remaining" caption="The case with only the fan and the power button left." >}}

{{< pair src1="img/power-cable-extended.jpg" alt1="iMac power button with extended wires soldered on" cap1="The power button with its extended wires." src2="img/imac-fan.jpg" alt2="iMac case fan with new wires, color-coded for the Noctua controller" cap2="The case fan with new wires, color-coded to match the Noctua controller." >}}

## Setting up the first power-on test

The plan was to test the panel with the R1811 alone, and add the DZ-LP0818 backlight board only once I knew the R1811 and panel worked.

### Connecting the R1811 cables to the panel

The display data cable is finicky to seat. A couple of YouTube videos helped.

{{< figure src="img/panel-cables.jpg" alt="Back of the LG panel with the backlight cable on the left and the data cable on the right" caption="Both cables connected. Left: backlight. Right: data." >}}

{{< pair src1="img/display-data-cable.jpg" alt1="Display data cable plugged into the panel" cap1="Display data cable." src2="img/display-data-cable-closeup.jpg" alt2="Close-up of the display data connector seated in the panel" cap2="Display data connector close-up." >}}

{{< figure src="img/backlight-cable.jpg" alt="Close-up of the R1811 backlight cable joined to the panel's backlight cable" caption="Backlight cable connection close-up." >}}

**Orientation matters.** The black-circle side mates with the white side that has the line and the recesses left and right. There is more guidance in the forum thread. If you connect it backwards the backlight will not come on, but apparently you will not fry anything.

### Connecting the R1811 to the panel

{{< figure src="img/panel-r1811-power-on.jpg" alt="R1811 driver board lying next to the panel with backlight and data cables connected" caption="R1811 backlight and data connections." >}}

{{< figure src="img/r1811-connections.jpg" alt="Close-up of the R1811 board with its cables" caption="R1811 connections close-up. The backlight wiring changes later, once the DZ-LP0818 goes in." >}}

## Power up: the panel works

OSD! I flipped the panel over and propped it up on foam blocks so I could see it.

{{< figure src="img/panel-osd.jpg" alt="The panel showing the R1811 on-screen display at first power-on" caption="The R1811 startup OSD. The panel lives." >}}

### Changing the OSD to English

{{< pair src1="img/osd-chinese.jpg" alt1="R1811 OSD in Chinese" cap1="OSD in Chinese, as shipped." src2="img/osd-english.jpg" alt2="R1811 OSD with Language set to English" cap2="Other > Language > English." >}}

### Verifying 5K from the Mac

I connected my 2020 MacBook Pro M1 with a Thunderbolt 4 cable I had around. Both the OSD and macOS show 5K.

{{< pair src1="img/mac-retina-resolution.jpg" alt1="The converted panel showing the macOS desktop at 5K resolution" cap1="5K resolution." src2="img/mac-usb-c.jpg" alt2="The R1811 OSD input indicator showing USB Type-C" cap2="USB Type-C input." >}}

{{< figure src="img/mac-brightness.jpg" alt="The converted panel at 50 percent brightness showing a satellite desktop image" caption="The panel at 50% brightness." >}}

### USB-C Power Delivery and screen flicker

At this stage the screen flickered, which was disheartening. I eventually realized that the MacBook was drawing power through the USB-C cable from the driver board's 24 V 5 A supply, even though the MacBook was plugged into its own power supply.

**If you connect your computer to the R1811's USB-C port with an underpowered supply, the board tries to power the computer and the display flickers.** Stonetaskin's 24 V 5 A supply is fine for the panel alone but cannot also feed a laptop. Two ways out:

1. A **USB-C to DisplayPort cable** instead, which gives full resolution without drawing power from the monitor's supply.
2. A **bigger power supply**, like one of the Mean Well units discussed in the forum.

I did not want Power Delivery from the monitor anyway, so a USB-C to DisplayPort cable fixed it. No more flicker.

### Pink edges on 2015 LG LM270QQ1 SD(B1) panels

My panel has the pink edge coloration common on LM270QQ1 SD(B1) panels, explained in the thread [here](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/post-33552575). It does not occur on 2017 and later iMacs with the SD(C1/D1/E1/F1) panels.

On mine the pink shows against a white or gray background near the edges, as a translucent gradient, darkest at the edge and gone about 2.5 cm (1 inch) in. Noticeable, but not a deal-breaker at this price. If I do another conversion I would probably go for a 2017 iMac 27, even though those sell for 350 to 400 euros on Wallapop.

## Adding the DZ-LP0818 constant current board

This board supplies and maintains extra current to the backlight. Backlights dim as they age, and this is the fix. Since I had already bought it, I installed it.

{{< pair src1="img/r1811-dz-lp0818-connections.jpg" alt1="R1811 and DZ-LP0818 boards wired together with the panel backlight cable" cap1="R1811 and DZ-LP0818 connections." src2="img/r1811-dz-lp0818-connections-closeup.jpg" alt2="Close-up of the DZ-LP0818 board connections" cap2="Close-up." >}}

I also tried USB-C power again with the constant current board in place, to see whether the flicker went away. It does not.

### A color profile with the Datacolor Spyder5

Using the Spyder5 was a pain because it is no longer supported and the software did not work well on my Mac. I did make a profile that looked better than the "iMacPro5K" one macOS assigned when the panel was first connected. More on this once the internals are finished.

## Quieting the R1811 fan

To cut the fan noise I added resistance to drop its voltage. First with two [Noctua NA-RC7 Low-Noise Adaptors](https://noctua.at/en/na-src7) I had lying around, connected in series with the fan using JST XH-2P connectors from [this kit](https://www.amazon.es/dp/B0CKSJPVZF). That is what the photo shows. I later replaced the Noctua adaptors with two 180 ohm 2 A resistors, 25 cents each, wired the same way. I need JSTs for another project, so this was practice. I have fat fingers and bad eyes.

{{< figure src="img/board-fan-resistors.jpg" alt="The R1811 board fan with two Noctua low-noise adaptors spliced in using JST connectors" caption="R1811 fan with the Noctua adaptors spliced in; the 180 ohm resistors went in the same place later. Yellow and red are positive, black is ground." >}}

The fan is now nearly silent.

## Powering the iMac case fan

The case fan had its extended wires; it needed power. Rather than tap the 12 V pads on the R1811, I split the output of the 24 V brick, added a 24 V to 12 V step-down converter (with a fuse between it and the brick, just in case), and the [Noctua NA-FC1](https://noctua.at/en/na-fc1) controller.

{{< figure src="img/power-setup.jpg" alt="Power brick, barrel splitter, fuse, step-down converter and Noctua fan controller laid out and connected" caption="The power setup. The step-down converter's output goes to the case fan." >}}

### The barrel plug size trap

The R1811's power socket takes a **5.5 mm outside / 2.5 mm inside** barrel plug. The "standard" size is 5.5 / 2.1 mm, and that is the [splitter](https://www.amazon.es/dp/B0DX6ZDM3J) I bought. It fit the Stonetaskin brick but needed an adapter for the R1811 socket. I had one in the junk drawer.

{{< figure src="img/y-splitter-plus-adapter.jpg" alt="Barrel plugs of the R1811 board, power supply, splitter and adapter side by side" caption="Barrel plug sizes for the board, the supply, the splitter and the adapter." >}}

### Setting the step-down converter

The converter is [this one](https://www.amazon.es/dp/B09DYDV3S5).

{{< pair src1="img/step-down-converter.jpg" alt1="The step-down converter with its inputs, outputs and adjustment controls labelled" cap1="Step-down converter inputs, outputs and controls." src2="img/set-step-down-converter.jpg" alt2="Multimeter reading slightly above 12 volts on the converter output" cap2="Set to 12 V on the converter's display; the multimeter reads a little more." >}}

## Where it stands, and what is next

As of November 2026 the monitor is in daily use as a 5K display. The electronics live inside the iMac case, and the OSD control strip hangs out the back, which works but is not pretty. Still on the list:

- **OSD buttons.** The plan is to move the keypad into the rear port cutouts using [this MakerWorld mount](https://makerworld.com/en/models/3519595-imac-27-conversion-to-monitor-r1820-cy-5key#profileId-4022780), designed for the 5-key keypad, if mine turns out to be the matching board. Forum member @jag001 did a similar job from scratch, [documented here](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/page-33?post=32702935#post-32702935).
- **Case fan inlet shroud.** A 3D-printed duct between the fan and the rear vent. @Xarli published one on [Thingiverse](https://www.thingiverse.com/thing:6775247), and there is [another make](https://www.thingiverse.com/thing:7100773).
- **Mounts for the boards and the converter.** 3D printed; I have not found ones for the R1811 and DZ-LP0818 that suit my layout.
- **Move the IR receiver** from the control strip to where the FaceTime camera was, so the remote works from the front.
- **A RAM cover**, since the Mystery Mac came without one.

## The bottom line

A dead 27-inch 5K iMac is a 120 euro panel with a free case, and for about 330 dollars of boards from Stonetaskin it becomes a monitor that macOS treats as a native Retina display. Buy the R1811 if you want the fewest surprises; check the panel code before you buy the iMac if pink edges would bother you (2017 and later are clean); use a USB-C to DisplayPort cable unless you also buy a power supply big enough to charge your laptop; and read the MacRumors thread before you post in it.

## Sources and useful links

- [MacRumors DIY 5K Monitor Success thread](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/), and its [WikiPost index of builds](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/page-1#post-30975623).
- [@PaulD-UK on wiring and power (#1822)](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/page-109#post-32717747)
- [@Aiwi on R1811 setup (#822)](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/page-35#post-31094713)
- [@Kaeslin's R9A18 project (#1409)](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/page-57#post-32247683)
- [iFixit logic board replacement guide](https://www.ifixit.com/Guide/iMac+Intel+27-Inch+Retina+5K+Display+Logic+Board+Replacement/30529)
- [Stonetaskin](https://stonetaskin.com), the board vendor
- [Noctua NA-FC1](https://noctua.at/en/na-fc1) fan controller
- [Ugreen USB-C to DisplayPort 1.4 cable](https://www.ugreen.com/products/usb-c-to-displayport-8k-cable), tested by many in the thread
- The original write-up: [kurtshuler/imac-5k-monitor-conversion](https://github.com/kurtshuler/imac-5k-monitor-conversion) on GitHub
