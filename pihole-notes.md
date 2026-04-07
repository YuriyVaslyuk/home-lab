# Pi-hole DNS Sinkhole - Home Lab Setup

## Overview

Set up Pi-hole as a network-wide DNS ad and tracker blocker on my home network. Running on a Raspberry Pi 5 4GB, covering all devices on the network at the DNS level without any per-device configuration.

Hostname: `pihole`  
IP: `x.x.x.x`  
Setup date: March 29, 2026  
Pi-hole version: v6  

---

## Hardware

- **Raspberry Pi 5 4GB** as my main device running Pi-hole for now
- **MicroSD card**: had a bad card on the first attempt, spent time troubleshooting what I thought was a driver issue with Raspberry Pi Imager, turned out the card itself was the problem. Swapped it out and the flash worked fine on the first try.
- **3D printed case**: printed on my Bambu Lab P1S after getting everything running. Keeps it clean on the shelf.

---

## Setup

Flashed Raspberry Pi OS 64-bit using Raspberry Pi Imager with SSH enabled and hostname/credentials pre-configured before first boot. Installed Pi-hole after confirming SSH access. Set a static IP on the router and pointed DNS to the Pi-hole address.
Initially, I wanted to get a micro HDMI adapter to use the monitor for it, but it was better to practice with SSH anyway, plus it was really easy after all the rooms in THM.

---

## Blocklists

Three blocklists configured:
I googled some other lists to see which worked better with minimal issues. These have been working good for me, I did not have to white list anything yet.

| List | Description |
|---|---|
| StevenBlack Hosts | General ads and malware |
| Hagezi Pro | Broader tracker and ad blocking |
| Hagezi Threat Intelligence (tif.txt) | Known malicious domains |

## These are the stats from the first day:

- Total domains on blocklist: ~1,352,475 
- Block rate: ~33% of all DNS queries  
- Active clients: 32  

---

## Observations

The stats were more interesting than I expected. I knew devices like smart plugs, robot vacuums, and especially streaming sticks were phoning home in the background, that part was not a surprise. What I did not expect was how often they do it. Even when a device is idle, it is constantly making DNS requests.

The most interesting find was a Firestick in my bedroom running sideloaded apps. Pi-hole flagged it as the heaviest blocked client on the network. Digging into it, one of the blocked domains was a Yandex.ru analytics tracker embedded in a sideloaded APK which is a Russian analytics service that had no business being in that app. The app itself had nothing to do with Russia.

I am originally from Ukraine, so anything routing data to Russian servers gets my attention. This was a good real-world example of why people should be careful with sideloaded apps. Most of us don't know what is baked into the APK. The app developer may not even know if they used a third-party SDK that includes it.

Pi-hole operates at the DNS level, so it blocked that traffic across the whole network without touching the device itself. No app to install, no VPN profile, no per-device configuration. One change at the DNS layer covers everything.

The downside to seeing all of this is that now I want to do more. I am considering segmenting my network with vlans to isolate IoT devices from the rest of the traffic, especially given how many of them I have and what I now see they are doing in the background. Pi-hole is a good first step, but it's not the whole picture, and it is definitely a rabbit hole, haha.

---

## What's Next

The Pi 5 is more than enough to run Pi-hole, but it is also more than Pi-hole needs. Planning to replace it with a Pi Zero 2 W as a dedicated Pi-hole device and repurpose the Pi 5 for a more demanding lab project  like a SIEM deployment (Wazuh or Graylog) or a WireGuard VPN server for remote network access.
