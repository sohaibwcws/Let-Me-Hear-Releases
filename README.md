# Let Me Hear

Local microphone noise cancellation for macOS. Everything runs on your Mac and no audio
is sent anywhere.

This repository exists only to host the download. It contains no application source.

## Download

Get the latest disk image from the [Releases page](../../releases/latest).

Requires macOS 14 or newer on Apple silicon.

## Install

1. Open the disk image and drag **Let Me Hear** into Applications.
2. Launch it. It lives in the menu bar, not the Dock.
3. Allow microphone access when macOS asks.
4. Open the panel, click **Install** in the Virtual Devices section, and approve the
   administrator prompt. This installs the audio driver into
   `/Library/Audio/Plug-Ins/HAL` and restarts CoreAudio. Every virtual audio device on
   macOS needs this, not just this one.
5. Both devices should read **Available**.
6. In your call app, set the microphone to **Let Me Hear Microphone**.

Step 6 is the one people miss. The app can be cleaning perfectly, but if your call app is
still pointed at the built in microphone then nobody hears a difference.

## Signed and notarized

The app is signed with a Developer ID certificate, runs under the hardened runtime, and is
notarized by Apple. The ticket is stapled to both the disk image and the app inside it, so
it validates offline. There is no Gatekeeper warning and no right click workaround needed.

Check it yourself after installing:

```sh
spctl -a -t exec -vv "/Applications/Let Me Hear.app"
```

That should report `accepted` and `source=Notarized Developer ID`.

## Verifying the download

Each release ships a `.sha256` file next to the disk image:

```sh
shasum -a 256 -c Let-Me-Hear-1.0.0-arm64.dmg.sha256
```

## What it does

- Removes background noise from your microphone before any app hears it, using a neural
  speech enhancement model running locally at 48 kHz.
- Publishes a virtual microphone, `Let Me Hear Microphone`, for call and recording apps.
- Publishes a virtual speaker, `Let Me Hear Speaker`, which can clean incoming call audio
  so a noisy caller on the other end is denoised too.
- Shows a live graph of input against cleaned level so you can see it working.

## Licensing

Released under the MIT License. See [LICENSE](LICENSE) for the full text, including the
retained copyright of the original author whose MIT licensed work this was built from,
and [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for the bundled model and runtime.

Copyright & Develop By Sohaib Khan

https://sohaib.com
