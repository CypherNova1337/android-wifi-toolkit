# VoidWave

A WiFi and network assessment toolkit for Android that tells you the truth about what your phone can do.

![license](https://img.shields.io/badge/license-MIT-blue?style=flat-square)
![kotlin](https://img.shields.io/badge/kotlin-Android%208%2B-7F52FF?style=flat-square)
![status](https://img.shields.io/badge/status-early%20alpha-orange?style=flat-square)

## What it does

Search for a WiFi security app and you'll find dozens promising handshake
capture and packet injection from a stock phone. They cannot do it. Since
Android 10 the platform will not put a phone's built-in WiFi chipset into
monitor mode, and without monitor mode there is no raw 802.11 capture, no
handshake grabbing and no injection — no matter how the app is written. The ones
that claim otherwise either mislead you or quietly do nothing while showing a
progress bar.

VoidWave starts from that fact instead of pretending otherwise. Every capability
is sorted into a tier by what it genuinely requires, and the app probes your
actual device at launch and tells you which tier you're in. You find out what
works *before* you're standing in a car park wondering why the capture is empty.

Within those honest limits a stock phone can do a surprising amount: survey and
grade nearby networks, spot rogue access points, map the subnet you're on, scan
services, audit TLS certificates, and capture full traffic to a pcap — all
without root.

## The tiers

| Tier | Needs | What you get |
|---|---|---|
| **0 — Stock** | Nothing | AP survey and security grading, rogue-AP correlation, subnet discovery, TCP service scanning, TLS and certificate audit, full traffic capture to pcap |
| **1 — Root** | A working `su` | tcpdump on a live interface, raw sockets, firewall control, bundled binaries |
| **2 — Monitor** | Root plus a kernel carrying an external adapter's driver | Monitor-mode verification and raw 802.11 capture |

Most people are Tier 0, and Tier 0 is genuinely useful. Tier 2 needs an external
USB adapter and a kernel that supports it — not a setting you can turn on.

## Why you'd use it

- **It won't lie to you about your hardware.** Capability is probed, not
  assumed.
- **Rootless packet capture** to standard pcap, readable in Wireshark.
- **Real network assessment** — service scanning, TLS auditing, segmentation
  checks — not just a list of SSIDs.
- **Tells you what your device is leaking** about itself.
- **Everything stays on the phone.** No accounts, no cloud, no uploads.

## Install

Build it yourself:

```bash
git clone https://github.com/CypherNova1337/android-wifi-toolkit
cd android-wifi-toolkit
./gradlew assembleDebug
```

The APK lands in `app/build/outputs/apk/debug/`.

Needs Android 8.0 or newer. Location permission is required for WiFi scanning —
that's an Android rule, not a choice this app makes.

## Using it

Open it and run the capability check first. It tells you your tier and, more
usefully, what *won't* work on your device and why. Everything else follows from
that.

From there the modules are grouped by what they do — surveying, discovery,
capture, auditing — with anything above your tier clearly marked rather than
hidden or silently broken.

For the full detail on external adapters, raw beacon elements, capture
internals, enterprise WiFi and segmentation testing, see the notes in the
repository.

## Good to know

- **Early alpha.** It builds and runs, and the Tier 0 set is implemented. Tier 1
  and Tier 2 modules exist but can only be exercised on hardware that meets
  their requirements.
- **Scanning a network is an interaction with it.** Service scanning and TLS
  auditing send traffic. On a network you don't own, that's unauthorised.
- **Surveying is passive; joining is not.** Listing nearby access points is
  receiving. Connecting to one is a different act with different consequences.
- **Android throttles WiFi scanning.** Results refresh slower than you'd expect,
  and that's the platform, not a bug here.
- **A capture is a real capture.** It contains whatever crossed the interface,
  including things you didn't intend to collect. Handle the pcap accordingly.

## Authorised use

Only use this on networks you own or have written permission to assess. Passive
surveying of what's broadcast around you is generally lawful; scanning,
capturing traffic and testing segmentation on a network you don't own is not.

## License

MIT — see [LICENSE](LICENSE).
