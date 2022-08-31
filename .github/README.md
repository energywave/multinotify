[![License](https://img.shields.io/github/license/energywave/multinotify.svg)](LICENSE)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/energywave/multinotify)
![GitHub all releases](https://img.shields.io/github/downloads/energywave/multinotify/total)
![GitHub commit activity](https://img.shields.io/github/commit-activity/y/energywave/multinotify)
# Multinotify by [@energywave](https://github.com/energywave) <!-- omit in toc -->
This is a service that use nearly all notify services of **Home Assistant** to give you one consistent interface to use in all your automations and powercharges your notifications at the next level with the maximum ease of use possible.

Multinotify can do it's magic using:
- [**Alexa**](https://github.com/custom-components/alexa_media_player) (pause music, set desired volume then set previous volume level and resume your music)
- [**Google Home**](https://www.home-assistant.io/integrations/tts/) (set desired volume then set previous volume level **TODO**)
- Other [**TTS**](https://www.home-assistant.io/integrations/tts/) (to be tested)
- [**Android Home Assistant Companion App**](https://companion.home-assistant.io/download) (supports nearly all advanced options!)
- [**iOS Home Assistant Companion App**](https://companion.home-assistant.io/download) (supporta nearly all advanced options!)
- [**Pushover**](https://www.home-assistant.io/integrations/pushover/)
- [**HTML5 push notifications**](https://www.home-assistant.io/integrations/html5/)

You can attach ad image as well as a snapshot of a camera entity by just specifiying the entity_id of your camera. Multinotify will take care of different ways of doing that for all the services!

## Features
 - One call, multiple notification services, easy and compact
 - You can specifiy different messages for Alexa or Google Home/TTS, if you want (so that you can use SSML markup, for example)
 - **ALEXA**: you can specify a single device, an Alexa multi room group of devices, an Home Assistant group of devices or a list of single devices as a target.
 - **ALEXA**: notification volume you choose, your previous volume for every device will be restored
 - **ALEXA**: music will be paused and automatically resumed after the announce
 - **ALEXA**: some volume bad behavior of Alexa Media Player are mitigated
 - **ALEXA**: the service will wait during announce so that, if used in a sequence, the next command will be executed AFTER the announce has been terminated. You can say, for example "Now I'll raise your shutters" and only then your shutters move.
 - **ALEXA**/**Google Home**/**TTS**: you can select a Do Not Disturb period, by default from 11:00 pm to 9:00 am. In this period no announce will be played if not specified the "force" parameter
 - **ALEXA**: you can choose between announce and tts service
 - **Google Home**/**TTS**: you can choose between tts.cloud_say (Nabu Casa) and tts.google_translate_say
 - **ALEXA**/**Google Home**/**TTS**: you can set a default announce volume but you can specify a different volume when you call multinotify, if you want
 - **APP**: Android app and iOS app have different syntaxes. Multinotify will take care of that for you. You'll have only to use multinotify, ignoring that
 - **APP**: you can specify (optionally) tag, group, channel, subtitle/subject, critical (to send an important notification that ignore phone silence mode), critical volume (iOS only), URL to open when you click, actions (max 3 on android, max 10 on iOS), attachment
 - **PUSHOVER**: supports multiple services, title, message, URL and attachment
 - **HTML5**: supports multiple services, title, message, icon, tag, critical, URL, actions and attachment
 - **ATTACHMENT**:
   - You can pass a **relative or absolute link to a media file** (image, video, audio file. Services limitations apply) to show that in the notification
   - You can pass an **entity_id of a camera** you have in your Home Assistant to let Multinotify send the best snapshot it can to every involved service. For example: it will send a snapshot to iOS and, when you enlarge the notification, you'll see a live stream of the camera. For pushover it will automatically save a snapshot an attach. For services that supports that it will use camera proxy.


## Table of Content <!-- omit in toc -->

- Why this work
- Dependencies
- Installation
- Changing package parameters
  - Anchor parameters
  - iPhones list
  - Alexa groups
  - Companion app groups
- How to use it
  - Parameters syntax
  - Alexa details
  - Google Home and other TTS details
  - Companion app details
  - Pushover details
  - HTML5 details
- Examples

**TO BE CONTINUED**



