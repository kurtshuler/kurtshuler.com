---
title: "Replacing the Orange España Livebox with a Huawei HG8310M ONT"
slug: "orange-livebox-to-huawei-hg8310m-ont"
date: 2026-11-02
lastmod: 2026-11-03
draft: false
# Staging: reachable by URL, hidden from lists, feeds and sitemap. Remove this block to publish.
build:
  list: never
  render: always
sitemap:
  disable: true
description: "How to take the Orange España Livebox 6 out of your network and run your own router directly on the fiber with a 15 euro Huawei HG8310M ONT. What to copy from the Livebox, the one setting that bit me, and the numbers before and after."
summary: "I took the Orange Livebox out of my network closet and put a 15 euro Huawei HG8310M ONT in its place, with a UniFi gateway doing the routing. Here is what you need to copy from the Livebox before you unplug it, the one authentication setting that cost me a registration attempt, and the before and after numbers."
tags: ["fiber", "orange", "unifi", "ont", "livebox"]
categories: ["Homelab"]
series: ["Orange España fiber"]
showTableOfContents: true
showHero: false
---

For three years the Orange España Livebox 6 sat on a shelf in my network closet doing exactly one useful thing: converting light into Ethernet. Everything else it did was in the way. It was a second router in front of my UniFi UDM Pro Max gateway (double NAT), it ran its own Wi-Fi radios whether I wanted them or not, and one week in August I caught it broadcasting a copy of my IoT SSID outside my firewall. On 2 November 2026 I pulled the fiber out of it and plugged it into a second-hand Huawei HG8310M, a 15 euro optical network terminal (ONT) with no router inside at all. It took 42 minutes from first lost ping to a working public lease, and the Livebox is now in a drawer.

This post is the write-up I wished existed when I started: what Orange España actually requires on the wire, what you must copy from the Livebox before you unplug it, which ONT to buy, and the one setting that is wrong in most of the forum guides.

One scope note before anything else: this is for a line on Orange's own fiber network (what the forums call *fibra directa*). If your Orange service is delivered over Movistar's plant as an indirect NEBA or VULA line, the ONT registration uses the hex form of the password and some details below will differ.

## Should you do this at all?

Probably not, unless something about the Livebox is actively bothering you. I want to be honest about the size of the win, because the forums are not.

What you gain:

- One router instead of two, so port forwards are a single hop and NAT loopback works.
- No ISP-controlled device inside your perimeter. No TR-069 remote management, no surprise firmware, no radios you did not turn on.
- A server closet shelf with a lot more space. This was my number one reason!

{{< figure src="img/11-huawei-on-shelf.jpg" figureClass="fig-right fig-30" alt="The same closet after: a palm-sized Huawei HG8310M lying flat on the shelf next to a mini PC" caption="After. The HG8310M is the size of a deck of cards and lies flat on the shelf. The top of the rack is empty." >}}

{{< figure src="img/12-closet-before.jpg" figureClass="fig-right fig-30" alt="Network closet before: a Livebox 6 standing upright on top of a small rack above a Synology NAS, two mini PCs and a UniFi gateway" caption="Before. The Livebox 6 standing on top of the rack, the only place it fit, with its own power brick and a cable run down to the gateway." >}}

What you lose:

- **The landline.** Orange has blocked third-party SIP registration since about 2019. If you use the fixed phone, stop here.
- **First-line support.** Expect Orange to ask you to plug the Livebox back in before they will look at anything. Keep it; it is their property and you return it when you cancel.
- **Firmware updates and remote diagnostics** on the ONT side. The HG8310M you buy second-hand runs whatever it runs.
- **Orange TV, only *IF* you use their separate DECODER BOXES.** You're fine if you watch Orange TV using Orange's app on your TV, Apple TV, etc.

**Orange TV, if you watch it through the app.** This is the question that stops most people, and it is a decoder problem, not a network problem. The physical Orange decoder needs multicast IPTV on a second VLAN (838) delivered to a dedicated router port, and that is fiddly on any third-party router. The Orange TV app on an Apple TV or a phone is plain HTTPS streaming and does not care what is upstream. Orange sells the same app to Digi and Movistar customers as ["Orange TV Libre"](https://bandaancha.eu/articulos/orange-tv-libre-abre-clientes-cualquier-11347) (a Digi user on bandaancha confirmed in 2024 that the decoder fails off-net with error S80 while [the app works fine outside the Orange network](https://bandaancha.eu/foros/configurar-orange-router-neutro-mantener-1752283)): the app cannot depend on an Orange router. We have no decoder, so nothing changed.

The speed is a wash. My 53-day baseline on the Livebox was a median of 869 Mbps down and 893 up. The Huawei's first test was 837 down, 840 up. That is inside normal variation for the hour of day, and the point of the exercise was never speed.

## What does Orange España actually need from your equipment?

Three things, and the answer is simpler than the guides make it look.

1. **A GPON ONT registered with your line's PLOAM password.** This is a 10-character string that identifies your subscription to the optical line terminal (OLT) in the exchange. The Livebox management UI has it and you copy it to the new ONT.
2. **Internet traffic tagged VLAN 832, 802.1p priority 0, plain DHCP.** No PPPoE, no username, no DHCP options. Your router's WAN port does the tagging.
3. **That is all.** No MAC cloning was needed on my line. The public IP stayed the same before and after, which tells you Orange ties the lease to the PLOAM credential and not to the hardware.

## The (Orange) French Disconnection

Now the trap if you research how to do this online: **Orange France also uses VLAN 832**, and the French guides outnumber the Spanish ones ten to one. But the setup is different for France than in Spain!

France needs 802.1p priority 6 and DHCP options 60, 77 and 90 carrying an `fti/` account identity. **Spain needs none of that.** If a guide mentions `fti/`, option 90 or CoS 6, it is French, and following it will fail. Orange España's requirement is strictly the easier one.

## Which ONT should you buy?

The one that matches the OLT vendor in your area, and the problem is that you cannot find that out remotely. A mismatched ONT does not fail outright; it registers and then runs slowly, which is the worst possible failure to debug. [Reports on bandaancha.eu](https://bandaancha.eu/foros/que-ont-compatibles-olt-nokia-alcatel-1745781) document Nokia ONTs on ZTE plant dropping from 600/600 to roughly 300/86 Mbps.

| OLT vendor in your area | ONT to buy |
|---|---|
| Alcatel-Lucent / Nokia (older Orange deployments) | Nokia G-010G-P |
| ZTE (ex-Jazztel plant) or Huawei | Huawei HG8310M or HG8010H |

Both are 10 to 15 euros second-hand on Wallapop. I bought one of each as a hedge and the Huawei worked first, so the Nokia is still in its box. The HG8310M choice came from [a bandaancha thread from April 2026](https://bandaancha.eu/foros/orange-tv-funcionando-ont-huawei-hg8310m-1759776) where a user got it working on Orange España, television included. Avoid the ZTE F601 ([documented failures on Orange direct fiber](https://bandaancha.eu/foros/ftth-orange-directo-no-consigo-funcionar-1745136) with correct settings) and the Ubiquiti UFiber Loco ([its PLOAM field is the wrong length](https://bandaancha.eu/foros/ubiquiti-ufiber-loco-router-asus-rt-1745605) for some Liveboxes).

You also need a fiber patch cable only if the one in the wall does not reach. It is **SC/APC, the green angled connector**, single-mode. Not the blue SC/UPC.

## What do you copy from the Livebox before you touch anything?

Three values, all in the Livebox admin UI at `192.168.1.1`:

1. **The PLOAM password.** *Información y diagnóstico > Parámetros del sistema > FIBRA*, row 2.7, "contraseña de ONT". The Livebox shows it twice: the 10-character ASCII form, and the hex form in brackets starting `0x`. Write down both exactly as shown. Some ONT firmwares want one form, some the other; the Huawei takes ASCII. The bandaancha thread on [getting the GPON key out of the Livebox](https://bandaancha.eu/foros/sacar-clave-gpon-idont-ploam-password-1742394) covers the older models and the edge cases.
2. **The Livebox WAN MAC address.** You probably will not need it, but if no lease arrives in 30 minutes, cloning it onto your router's WAN is the first thing to try.
3. **The optical receive power.** Same page, row 2.6, "potencia recepción". Mine read -19.0 dBm. After the swap, a reading much worse than this on the new ONT means the connector, not the ONT.

{{< figure src="img/00-livebox-fibra-ploam.jpg" alt="Livebox Fibra admin page, Parámetros del sistema, FIBRA tab, with row 2.7 contraseña de ONT highlighted" caption="The Livebox FIBRA page. Row 2.7 is the PLOAM password in both forms, row 2.6 the receive power. Screenshot by a bandaancha member, from [the PLOAM thread](https://bandaancha.eu/foros/sacar-clave-gpon-idont-ploam-password-1742394); values blurred by the original poster." >}}

{{< figure src="img/01-mac-static-ip-bench.jpg" figureClass="fig-right fig-40" alt="macOS network settings with a manual IP of 192.168.100.2 and subnet 255.255.255.0" caption="The bench setup: the laptop on a static 192.168.100.2 so it can reach the ONT's web UI at 192.168.100.1." >}}

## How did I prepare before I started?

- A laptop set to a static address for the ONT's bench network.
- Fiber cleaning supplies (isopropyl alcohol and lint-free wipes, or a one-click cleaner) and dust caps for the Livebox optic and the wall pigtail.
- The UniFi console, an SSH window and the Livebox UI, all open before unplugging anything.

## How do you configure the Huawei HG8310M?

On the bench first, with the fiber still in the Livebox. 

1. Connect a laptop to the Huawei's single Ethernet port, give the laptop a static IP of `192.168.100.2/24`, and browse to `http://192.168.100.1`. 
2. Log into the Huawei page. There are no credentials printed on the unit. The defaults are `telecomadmin` / `admintelecom`, with `root` / `adminHG` as the fallback ([hack-gpon.org](https://hack-gpon.org/ont-huawei-hg8010h/) keeps the list for the whole HG80xx family, and [maquinasvirtuales.eu](https://www.maquinasvirtuales.eu/ont-configurar-huawei-hg8010h/) has screenshots of the same web UI).

{{< figure src="img/02-huawei-device-info-o1.jpg" alt="Huawei HG8310M Device Information page showing ONT Registration Status O1 (Initial state) and ONT ID 255" caption="Fresh out of the box on the bench, fiber not yet connected: registration status O1, ONT ID 255. Serial and product ID redacted." >}}

{{< figure src="img/03-huawei-optical-no-fiber.jpg" alt="Huawei optical information page with TX and RX power shown as dashes" caption="Optical Information with no fiber: dashes for TX and RX. Worth a look now so you recognize the page later." >}}

3. Set the **ONT Authentication**. Here is where I screwed up my first registration attempt, and where I think most of the Spanish guides are confusing.
   
   Set **Authentication Mode** to **Password**, enter the **PLOAM** in ASCII, **apply**, and **reboot the ONT** (*System Tools > Reboot*). Once I did this it went to O5 within about ten minutes of the fiber going in.

   The first time I did this I screwed it up! I chose **LOID** instead of  **Password** for the **Authentication Mode** because the forum guides say "enter the PLOAM" without saying which, and the PLOAM field under LOID is the obvious place to type a 10-character string. I did that. The ONT bounced between O3 and O2 for ten minutes: the OLT discovered it, then deactivated it. If you end up at O2, you probably made the same mistake I did. Do not keep retrying. Repeated failed registrations can get your PLOAM blocked at the OLT, and that takes the Livebox down too until Orange clears it.


{{< figure src="img/04-huawei-auth-loid-wrong.jpg" alt="Huawei ONT Authentication page with Authentication Mode set to LOID and the PLOAM entered in the LOID field" caption="Wrong. Authentication Mode = LOID with the PLOAM in the LOID field. The OLT discovers the ONT and immediately deactivates it (O3 to O2, over and over). This cost me my first registration attempt." >}}

{{< figure src="img/07-huawei-auth-password-right.jpg" alt="Huawei ONT Authentication page with Authentication Mode set to Password, Password Mode ASCII string, and the GPON serial left as shipped" caption="Right. Authentication Mode = Password, Password Mode = ASCII string, the PLOAM in the Password field, serial untouched. Apply, reboot, O5 on the next attempt." >}}

Leave the GPON serial number alone. Every documented Spanish success kept the ONT's own serial; serial spoofing is a French and Polish complication that does not apply here.

One more thing to check on the bench: my unit's firmware (V3R015C10S106) has four tabs, Status, LAN, Security, System Tools. There is **no WAN tab**, which means the ONT cannot tag VLAN 832 itself. The OLT passes the VLAN through tagged, and your router has to handle it. This contradicts at least one widely shared guide that was actually written about [the HG8120C](https://bandaancha.eu/foros/ont-router-neutro-sin-perder-tv-1759672), a different unit with a router inside.

## How do you configure the router?

{{< figure src="img/09-unifi-wan1-vlan-832.jpg" figureClass="fig-right fig-30" alt="UniFi Internet 1 settings panel with VLAN ID enabled and set to 832, QoS Tag and MAC Address Clone unticked, IPv4 connection DHCP" caption="The whole router-side configuration on a UniFi gateway: VLAN ID 832, nothing else ticked, DHCP." >}}

I run a UniFi Dream Machine Pro Max, so these are UniFi fields, but every serious router has the same four settings.

*Settings > Internet > your WAN*:

| Field             | Value                                                      |
| ----------------- | ---------------------------------------------------------- |
| IPv4 connection   | DHCPv4                                                     |
| VLAN ID           | Enabled, **832**                                           |
| QoS tag           | None                                                       |
| DHCP CoS          | None                                                       |
| MAC address clone | Off, unless no lease after 30 minutes                      |
| IPv6              | Off (Orange España residential fiber has none to give you) |

"*None*" on the two priority fields is correct. A tag sent without an explicit priority carries 802.1p 0, which is exactly what Orange España wants. Do not set the QoS tag to anything; it applies to all egress traffic and there are reports of a UDM dropping to 30 Mbps with it set. If you run OPNsense instead of UniFi, [this bandaancha thread](https://bandaancha.eu/foros/opnsense-orange-ftth-espana-ont-propia-1758841) and [this OPNsense forum thread](https://forum.opnsense.org/index.php?topic=50206.0) cover the same VLAN 832 setup, the second one including the TV VLAN for a decoder.

You can do this while the Livebox is still live if you configure a spare router port as a second WAN, which turns the swap night into a cable move. I did it the simple way, on the primary WAN, during the outage.

## What does the swap itself look like?

In order, with my timestamps from 2 November:

1. **12:25** Fiber out of the Livebox. Cap the Livebox optic. Inspect and clean the connector on the wall pigtail. Fiber into the Huawei, once. Every mate cycle on that SC/APC connector is wear on the one part you cannot replace yourself, so plug it in and leave it.
2. Ethernet from the Huawei's LAN port to the router's WAN port, replacing the cable that went to Livebox port 1.
3. **12:35** Huawei Status page shows **O5 (Operation state)**. If it sits in O1 to O4 for ten minutes, then you have something wrong. Note the RX power.
4. **12:54** Public lease visible on the UniFi WAN, once VLAN 832 was enabled on it. A `192.168.1.x` address here means your tag is not being applied.
5. Check inbound from a phone on cellular. My Cloudflare-fronted services came straight back because the public IP had not changed.

{{< figure src="img/08-huawei-device-info-o5.jpg" alt="Huawei Device Information page showing ONT Registration Status O5 (Operation state) and ONT ID 90" caption="Registered: O5 (Operation state), and the OLT has assigned ONT ID 90 in place of the placeholder 255." >}}

{{< figure src="img/05-huawei-optical-after.jpg" alt="Huawei optical information page showing TX 2.37 dBm and RX -22.60 dBm" caption="Optical Information with the fiber in: TX +2.37 dBm, RX -22.60 dBm, 44 C. The Livebox read -19.0 on the same fiber." >}}

{{< figure src="img/10-unifi-latency-gap.jpg" alt="UniFi dashboard latency graph for the last 24 hours with a gap around 12:25 and otherwise unchanged latency" caption="The UniFi latency graph across the swap. The gap at 12:25 is the fiber move; everything either side of it looks the same." >}}

Then leave it alone and let your monitoring run. Mine kept logging latency and speed tests through the whole thing, which is where the numbers below come from.

## What were the performance numbers?

| | Livebox 6 (53-day baseline) | Huawei HG8310M (day one) |
|---|---|---|
| Download, median | 869 Mbps | 837 Mbps (one midday sample) |
| Upload, median | 893 Mbps | 840 Mbps |
| Latency to Microsoft / Google / Cloudflare | 13 / 13 / 5 ms | 13 / 13 / 5 ms |
| Optical RX | -19.0 dBm | **-22.6 dBm** |
| Optical TX | +3.0 dBm | +2.4 dBm |
| Public IP | unchanged | unchanged |
| Reboots survived unattended | n/a | 1 (ONT power cycle, lease back in 5 minutes) |

The RX figure is the one I am watching. A 3.6 dB drop on the same fiber is more than I expected, and it is either the Huawei's optic reading differently from the Livebox's or a connector I did not clean well enough. It is comfortably inside the GPON receive window (the spec floor is around -27 dBm) and the error counters are at zero. So it stays until it drifts or errors appear.

One thing I did not plan: the Huawei shipped with a power supply with a really short cable. It wants 12 V, 0.5A, center positive, and the Livebox's brick is 12 V, 2 A, center positive, so I just reused the already installed Livebox power supply. The five-minute outage at 13:00 on my latency graph is that swap.

## What would I do differently?

- **Password mode, not LOID, from the start.** That is the whole reason this post exists.
- **Pre-configure the VLAN on a spare WAN port** before the night. Then the outage is the length of a cable move, not 29 minutes.
- **Clean the wall connector more carefully**, or at least photograph the ferrule with a phone macro before plugging in. I would like to know whether that -22.6 is the fiber or the optic.

## The bottom line

If the Livebox is just converting light to Ethernet for you, a 15 euro HG8310M does the same job without the second router, the extra radios, the remote management, and the physical space. Copy the PLOAM password and the WAN MAC off the Livebox first, buy the ONT that matches your area's OLT vendor, set Authentication Mode to **Password**, and tag **VLAN 832** on your router's WAN with no priority and plain DHCP. The landline will not work, the TV app does not notice, and the speed does not change.

Keep the Livebox. It is Orange's, and the day you need support it is the first thing they will ask for.

## Sources

The Spanish fiber community on bandaancha.eu did most of the groundwork here. The threads I leaned on:

- [Orange TV funcionando con ONT Huawei HG8310M](https://bandaancha.eu/foros/orange-tv-funcionando-ont-huawei-hg8310m-1759776), April 2026. The success report that made me buy this unit.
- [ONT + router neutro sin perder la TV](https://bandaancha.eu/foros/ont-router-neutro-sin-perder-tv-1759672), April 2026. The HG8120C thread, useful for the TV VLAN, misleading on the HG8310M.
- [Qué ONT son compatibles con OLT Nokia/Alcatel](https://bandaancha.eu/foros/que-ont-compatibles-olt-nokia-alcatel-1745781) and [Qué ONTs recomendáis](https://bandaancha.eu/foros/que-onts-recomendais-conexion-fibra-1742395). The OLT vendor question.
- [Sacar clave GPON / ID ONT / PLOAM password](https://bandaancha.eu/foros/sacar-clave-gpon-idont-ploam-password-1742394) and [Solución: cambia la ONT, password Livebox 6](https://bandaancha.eu/foros/solucion-cambia-ont-password-livebox-6-1744855).
- [VLAN y prioridad por operadora](https://bandaancha.eu/foros/vlan-prioridad-operadora-internet-voz-tv-1742983). The table of VLAN IDs and priorities per Spanish ISP.
- [OPNsense + Orange FTTH España con ONT propia](https://bandaancha.eu/foros/opnsense-orange-ftth-espana-ont-propia-1758841) and the [ZTE DHCP follow-up](https://bandaancha.eu/foros/opnsense-orange-ftth-ont-propia-zte-dhcp-1760854).
- [Orange TV Libre se abre a clientes de cualquier operador](https://bandaancha.eu/articulos/orange-tv-libre-abre-clientes-cualquier-11347), bandaancha's article on the app.
- [hack-gpon.org](https://hack-gpon.org/), the reference for ONT default credentials and firmware quirks: [Huawei HG8010H](https://hack-gpon.org/ont-huawei-hg8010h/), [Nokia G-010G-P](https://hack-gpon.org/ont-nokia-g-010g-p/).
- [ADSLZone tutorial for the Nokia G-010G-P](https://foro.adslzone.net/fibra-optica.94/tutorial-configurar-ont-nokia-g-010g-p.441070/), if you end up on the Nokia path.
- [OPNsense forum, Orange Spain FTTH with own ONT](https://forum.opnsense.org/index.php?topic=50206.0), December 2025. The best write-up of the decoder (VLAN 838) case, which I did not need.
