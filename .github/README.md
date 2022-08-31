[![License](https://img.shields.io/github/license/energywave/multinotify.svg)](LICENSE)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/energywave/multinotify)
![GitHub all releases](https://img.shields.io/github/downloads/energywave/multinotify/total)
![GitHub commit activity](https://img.shields.io/github/commit-activity/y/energywave/multinotify)
# Multinotify by [@energywave](https://github.com/energywave) <!-- omit in toc -->
This is a service that use nearly all notify services of **Home Assistant** to give you one consistent interface to use in all your automations and powercharges your notifications at the next level with the maximum ease of use possible. Advanced features are easy to use here :)

## Services that multinotify can use:
- [**Alexa**](https://github.com/custom-components/alexa_media_player) (pause music, set desired volume then set previous volume level and resume your music)
- [**Google Home**](https://www.home-assistant.io/integrations/tts/) (set desired volume then set previous volume level **TODO**)
- Other [**TTS**](https://www.home-assistant.io/integrations/tts/) (to be tested)
- [**Android Home Assistant Companion App**](https://companion.home-assistant.io/download) (supports nearly all advanced options!)
- [**iOS Home Assistant Companion App**](https://companion.home-assistant.io/download) (supporta nearly all advanced options!)
- [**Pushover**](https://www.home-assistant.io/integrations/pushover/)
- [**HTML5 push notifications**](https://www.home-assistant.io/integrations/html5/)


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
   - You can pass an **entity_id of a camera** you have in your Home Assistant to let Multinotify send the best snapshot it can to every involved service. For example: it will send a snapshot to iOS and, when you enlarge the notification, you'll see a live stream of the camera. For pushover it will automatically save a snapshot an attach. For services that supports that it will use camera proxy. Don't bother about details, **just pass a camera entity_id and you'll receive a snapshot of that camera!**


## Table of Content <!-- omit in toc -->

- [Why this work](#why-this-work)
- [Dependencies](#dependencies)
- Installation
  - [New installation](#new-installation)
  - [Updating](#updating)
- Changing package parameters
  - [Anchor parameters](#anchor-parameters)
  - iPhones list
  - Alexa groups
  - Companion app groups
- [Create UI cards](#create-ui-cards)
- How to use it
  - Parameters syntax
  - Alexa details
  - Google Home and other TTS details
  - Companion app details
  - Pushover details
  - HTML5 details
- Examples

## Why this work
Notifications and announcments are a fundamental part of my smart home. So my first automations where full of redundant service calls to the app, to Alexa and eventually to other services.
Same message was specified at least two times, at every call.
A developer (as I am) is a lazy creature by definition. And multiple strings definitions where unacceptable. So multinotify was born.

But then it became much more than that! Alexa announcments without a separate volume where terrible. I use to automatically play soft music at my entrance to my home, at a low volume. If an announcment arrive at that volume it's nearly impossible to understand it. So all the Alexa part where born.

Then it became a challenge with myself to what could be done using yaml. And how to do things in an elegant way (like volume and play status backup/restore of every Alexa device involved without the need to define some helpers, I'm proud of that)

Then it was adopted and I began to implement other services and keep the work as clean as possible, in my (very) compressed time.

I wrote an [article about multinotify on my blog site](https://henriksozzi.it/2022/01/package-multinotify-notifiche-su-alexa-e-app/), it's in Italian language but you can translate using Google Chrome. The multinotify described there is usually older than the one you can find here.

## Dependencies
 - [Alexa Media Player](https://github.com/custom-components/alexa_media_player) only if you want to play announcments on Alexa devices
 - [Companion apps for Android or iOS](https://companion.home-assistant.io/) only if you want notifications to the apps.
 - Specific `set_state` python script (needed only for Alexa and Google Home/TTS announcments):
    - [File from @xannor repo](https://github.com/xannor/hass_py_set_state) or
    
    - [File from my website, with complete explanation in IT language](https://henriksozzi.it/2021/04/impostare-lo-stato-di-unentita-in-home-assistant/)
  
**WARNING**: check that your script is the correct one as many set_state you can find on the web doesn't allow to create entities like this specific one!
 - If you want to send your camera snapshot to Pushover you'll have to create the folder /config/tmp and you'll have to ensure that the path is included in [`allowlist_external_dirs`](https://www.home-assistant.io/docs/configuration/basic/#allowlist_external_dirs)
 - If you want to send your camera snapshot to HTML5 you'll have to create the folder /config/www/cam and you'll have to ensure that the path is included in [`allowlist_external_dirs`](https://www.home-assistant.io/docs/configuration/basic/#allowlist_external_dirs)

## New installation
If you install this package for the first time you'll have to check that you have a correctly configured packages folder as explained [here](https://www.home-assistant.io/docs/configuration/packages/#create-a-packages-folder).

Then you'll have to create the `multinotify` folder inside your `packages` folder and copy there all the files in this repo or in the release zip file.

The last step you'll have to do will be to set [parameters](#changing-package-parameters) and [create UI cards](#create-ui-cards).

A restart of the core will be the last step to get you ready to use `multinotify`.

## Updating
**WORK IN PROGRESS**
## Changing package parameters
**WORK IN PROGRESS**
### Anchor parameters
**WORK IN PROGRESS**
## Create UI cards
**WORK IN PROGRESS**




