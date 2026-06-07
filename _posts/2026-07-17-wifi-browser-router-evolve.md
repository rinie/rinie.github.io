---
layout: post
title: "WiFi, the Browser and the Router Just Evolve. Matter and MQTT Try to Fix the User."
date: 2026-07-17
tags: [gutenberg-semantic, smart-home, mqtt, matter, dns, router, rflink, domoticz]
level: general
description: "Ten router transitions over thirty years. Dialup to fibre. Chrome updated every week. Not one bookmark broke. Not one URL needed updating. The waterline held because DNS, HTTP and the router stayed on the correct seams. Matter and MQTT try to fix the user. The user is not the problem. The seam is in the wrong place."
---

Ten router transitions over thirty years. Dialup to ISDN to ADSL to fibre, multiple providers, different hardware, different IP addresses, different everything below the waterline.

Chrome updated every week throughout. Not one website broke. Not one bookmark died. Not one URL needed updating.

The waterline held. WiFi, the browser, and the router just evolved on the correct seams. Matter and MQTT look at the same problem and try to fix the user instead.

The user is not the problem. The seam is in the wrong place.

---

## The Router Is Indifferent

The router hands out IP addresses without opinions. DHCP assigns your laptop an address when it connects. DNS resolves the hostname to an IP when you type a URL. NAT handles the address translation between your private network and the internet. The router does not know if you are streaming Netflix or controlling a light bulb. It just routes packets.

This is the O(1) resolver at the network layer. No pairing ceremony. No QR code. No commissioning. No app. The device appears on the network. The router notes its presence. Life continues.

The browser sits above this infrastructure and inherits its indifference. The browser does not know which router model you have. The browser does not know your ISP. The browser does not know your IP address changed when you switched from ADSL to fibre. DNS resolved the hostname. HTTP fetched the page. The browser showed it. The seam between "the network changed" and "the page loaded" is clean. Below the waterline, everything changed. Above it, nothing did.

**The MQTT device across the same transition:**

Every MQTT device provisioned before the router change: potentially broken. The broker address was hardcoded at provisioning time. The network topology changed. The broker may be at a different IP. The device is looking for a parking space that moved. The user is the resolver. The weekend reconfiguration begins.

Chrome user: opened browser, everything worked.
MQTT user: spent a weekend tracking down devices.

---

## MQTT Gets the Pages Wrong

MQTT's QoS levels try to solve reliability at the Gutenberg layer. QoS 2 guarantees exactly-once delivery — the right semantic guarantee. The cost: four message exchanges per message. The microkernel price for semantic guarantees at the wrong layer. The protocol is paying a context switch for every message to maintain a semantic invariant that the application layer could have handled more cheaply.

The broker address is the deeper problem. MQTT does not use DNS for broker discovery. The device needs to know the broker's address at provisioning time — an IP address or hostname provided by the user. When the network changes, the device cannot find its own resolver. The plate is attached to the parking space, not to the car.

DNS would have solved it. `mqtt.home.local` resolving to wherever the broker currently lives. The plate stays. The parking space changes. The device follows the DNS record. MQTT chose not to use the resolver that the rest of the network already had.

The user became the resolver. The seam that should be below the waterline surfaced above it.

---

## The Gemba Walk: Hello World Across Platforms

The correct measurement for a smart home platform is not the feature list. It is: how long does hello world take, and how many times do you redo it after a platform update?

**Alexa** — the cloud resolver that moves without asking. The skill API changed in 2022. Third-party integrations broke. The hello world that worked in 2019 stopped working — not because you touched anything, but because Amazon moved the seam. The plate is readable. The parking lot moved. The car is still in the space Amazon points you to now.

**Google Home** — the same arc on a different schedule. Works with Google Assistant deprecated. Nest/Home unification. OAuth token rotations that require reauthorisation. The integration that worked last month needs attention this month. The seam moves when Google decides it should move.

**Four Raspberry Pi generations — Pi 1 through Pi 4** — the most honest platform in the list. Different processor architectures on every generation. Each one faster. Raspberry Pi OS is Debian underneath. The `apt` package for Domoticz installed the same way on Pi 4 as it did on Pi 2. The application above the waterline travelled across four hardware generations without reconfiguration. The Gutenberg layer improved. The Semantic layer collected the gains. This is Moore's Law working as designed.

**Domoticz** — the Debian of smart home. SQLite configuration. Local control. No cloud dependency. The Z-Wave dongle that worked on Pi 2 still works on Pi 4. Scripts written in 2015 still run. Slow. Conservative. Slightly behind on features. Completely predictable. The boat trip, not rafting.

**Home Assistant** — the Ubuntu of smart home. Better features. Better UI. Active development. Breaking changes. Configuration migrations. Entity deprecations. The `configuration.yaml` from 2020 needs updating in 2024. Worth it if you follow the updates. Exhausting if you just want the lights to work. Rafting instead of a boat trip.

**RFLink** — the honest primitive. A serial-to-RF bridge. Text commands over USB serial. Sends 433MHz RF signals. Receives them and sends text back. No cloud. No broker required. No commissioning ceremony. No QR code. The seam is a serial port. You can talk to it with a terminal emulator.

RFLink makes no assumptions about what is above it. Domoticz decides what the bytes mean. Home Assistant decides. A Python script decides. RFLink does not care. The brick holds the wall up.

The limitation is the same as the honesty: 433MHz is noisy, unencrypted, one-way-mostly. No guaranteed delivery. No security. No mesh. The seam is clean because the protocol is simple. Simple protocols have clean seams. Complex protocols try to fix the user.

**The verdict from the Gemba:**

Domoticz: still running. Scripts still working. Pi 4 runs what Pi 2 ran.
RFLink: serial port still works. Bytes still travel. Nothing to reconfigure.
Alexa: third-party skill broke. Reconfigure.
Google Home: OAuth rotation. Reauthorise.
Home Assistant: migration needed. Follow the updates.

The platform that won across ten router transitions, four Pi generations, and fifteen years: the one with the most boring seams. Not the most features. Not the most integrations. The most stable seams.

---

## Matter's Commissioning Ceremony

Matter promised to fix smart home fragmentation — one protocol, every device, every platform. The architecture is correct in principle: Thread for mesh, IP for transport, Matter for the application layer. The seams are explicitly designed.

The commissioning process is where the user is asked to become the resolver.

To add a Matter device: find the QR code or numeric code on the device, open the controller app, ensure the controller and device are on the same network, ensure you have a Thread border router if the device uses Thread, sometimes update the device firmware first, wait for the commissioning to complete, troubleshoot when it fails silently at step 4.

The browser does not have a commissioning ceremony. DNS does not ask you to scan a QR code. DHCP does not require an app. The router hands the device an address. The device appears. The user does not need to know any of this happened.

Matter's commissioning is the seam above the waterline masquerading as the seam below it. The user is asked to perform the function the router already performs for every other networked device — for free, invisibly, without a ceremony.

The router is indifferent. Matter is not. The difference is where the seam is.

---

## The Correct Model

Every smart home device should:
- Get a local IP address from DHCP — the same way your laptop does, zero configuration
- Be discoverable via mDNS — already exists, works today
- Be controllable via a local HTTP or WebSocket API — the same transport the browser uses
- Be controllable from the browser directly

The controller is the browser. The hub is the router. The commissioning ceremony is typing a URL. The platform is what the web already is.

Home Assistant gets closest. Local control. mDNS discovery. REST and WebSocket APIs. Still requires setup — still not zero-configuration — but the seams are at least in the right places.

The router is the unsung hero. DHCP without opinions. DNS without opinions. NAT without opinions. Every smart home protocol that adds a hub, a bridge, a controller, a commissioning ceremony is adding opinions where the router has none. The brick asking which room it is in. The seam above the waterline when it should be below it.

---

## The Seam Is in the Wrong Place

WiFi, the browser, and the router evolved on the correct seams. They did not try to fix the user. They put the complexity below the waterline and left the user above it.

Ten router transitions. Chrome updated every week. Nothing broke.

Matter and MQTT put the seam in the wrong place and handed the user a QR code and a weekend of reconfiguration.

The user is not the problem. The seam is in the wrong place.

Simple protocols have clean seams. Complex protocols try to fix the user. The router is indifferent. The lights should be too.

Evolution will tell who suxed less. The evidence so far: the protocols that stayed below the waterline are still running. The protocols that surfaced above it are still asking the user to fix things.

---

*This post is part of the [Gutenberg/Semantic series](https://rinie.github.io/2026/05/14/gutenberg-vs-semantic/). Related: [Your Lights Don't Know Your Name](https://rinie.github.io/2026/06/03/bluetooth-matter-rdf-naming/) on Bluetooth, Matter, and Gutenberg identifiers crossing the boundary, [The Streaming Billy Is Not That Far Away](https://rinie.github.io/2026/07/11/streaming-billy/) on DNS, URL, and IP as the three correctly layered identifiers, [Marvin Remembers the Serial Number of Every Car](https://rinie.github.io/2026/06/30/marvin-remembers-serial-number/) on the plate versus the parking space, and [Should I Click? Trusting the Letter by the Envelope](https://rinie.github.io/2026/07/10/should-i-click/) on what happens when the resolver is taken away from the user.*
