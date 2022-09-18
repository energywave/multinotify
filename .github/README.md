[![License](https://img.shields.io/github/license/energywave/multinotify.svg)](LICENSE)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/energywave/multinotify)
![GitHub all releases](https://img.shields.io/github/downloads/energywave/multinotify/total)
![GitHub commit activity](https://img.shields.io/github/commit-activity/y/energywave/multinotify)
# Multinotify by [Henrik Sozzi](https://github.com/energywave) <!-- omit in toc -->
This is a service that use nearly all notify services of **Home Assistant** to give you one consistent interface to use in all your automations and powercharges your notifications at the next level with the maximum ease of use possible. Advanced features are easy to use here :)

## Services that multinotify can use:
- [**Alexa**](https://github.com/custom-components/alexa_media_player) (pause music, set desired volume then set previous volume level and resume your music)
- [**Google Home**](https://www.home-assistant.io/integrations/tts/) (set desired volume then set previous volume level **TODO**)
- Other [**TTS**](https://www.home-assistant.io/integrations/tts/) (to be tested)
- [**Android Home Assistant Companion App**](https://companion.home-assistant.io/download) (supports nearly all advanced options!)
- [**iOS Home Assistant Companion App**](https://companion.home-assistant.io/download) (supporta nearly all advanced options!)
- [**Pushover**](https://www.home-assistant.io/integrations/pushover/)
- [**HTML5 push notifications**](https://www.home-assistant.io/integrations/html5/)
- [**Persistent notification**](https://www.home-assistant.io/integrations/persistent_notification/)

**PLEASE READ THE [CHANGELOG](/CHANGELOG.md) TO SEE WHAT'S CHANGED FROM YOUR INSTALLED VERSION BEFORE TO UPDATE!**

If you like my work and want to help me to improve and implement new features please consider a donation to support my long nights of work:
- [![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](henrik_sozzi@hotmail.com) <--- Donate using **Paypal**
- <a href="https://www.buymeacoffee.com/henriksozzi" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/guidelines/download-assets-sm-2.svg" alt="Buy Me A Coffee" style="height: 31px;width: 96px" ></a> <--- Donate using **Buy me a coffee**

## Features
 - One call, multiple notification services, easy, compact and consistent interface.
 - You can specifiy different messages for Alexa or Google Home/TTS, if you want (so that you can leverage SSML markup just for Alexa announcement, for example)
 - **ALEXA**: you can specify a single device, an Alexa multi room group of devices, an Home Assistant group of devices or a list of single devices as a target.
 - **ALEXA**: notification volume you choose, your previous volume for every device will be restored
 - **ALEXA**: music will be paused and automatically resumed after the announce
 - **ALEXA**: some volume bad behavior of Alexa Media Player are mitigated
 - **ALEXA**: the service will wait during announce so that, if used in a sequence, the next command will be executed AFTER the announce has been terminated. You can say, for example "Now I'll raise your shutters" and only then your shutters move.
 - **ALEXA**/**Google Home**/**TTS**: you can select a *Do Not Disturb* period, by default from 11:00 pm to 9:00 am. In this period no announce will be played if not specified the "force" parameter
 - **ALEXA**: you can choose between announce and tts service
 - **Google Home**/**TTS**: you can choose between tts.cloud_say (Nabu Casa) and tts.google_translate_say
 - **ALEXA**/**Google Home**/**TTS**: you can set a default announce volume but you can specify a different volume when you call multinotify, if you want
 - **APP**: Android app and iOS app have different syntaxes. Multinotify will take care of that for you. You'll have only to use multinotify, ignoring that
 - **APP**: you can specify (optionally) tag, group, channel, subtitle/subject, critical (to send an important notification that ignore phone *Do Not Disturb* mode), critical volume (iOS only), URL to open when you click, actions (max 3 on android, max 10 on iOS), attachment. On Android you can even play a TTS message.
 - **PUSHOVER**: supports multiple services, title, message, URL and attachment
 - **HTML5**: supports multiple services, title, message, icon, tag, critical, URL, actions and attachment
  - **ATTACHMENT**:
   - You can pass a **relative or absolute link to a media file** (image, video, audio file. Services limitations apply) to show that in the notification
   - You can pass an **entity_id of a camera** you have in your Home Assistant to let Multinotify send the best snapshot it can to every involved service. For example: it will send a snapshot to iOS and, when you enlarge the notification, you'll see a live stream of the camera. For pushover it will automatically save a snapshot an attach. For services that supports that it will use camera proxy. Don't bother about details, **just pass a camera entity_id and you'll receive a snapshot of that camera!**


# Table of Content <!-- omit in toc -->

- [Why this work](#why-this-work)
- [Dependencies](#dependencies)
- [Installation](#installation)
  - [New installation](#new-installation)
  - [Updating](#updating)
- [Changing package parameters](#changing-package-parameters)
  - [Anchor parameters](#anchor-parameters)
  - [Alexa, Google Home and apps groups](#alexa-google-home-and-apps-groups)
- [Create UI cards](#create-ui-cards)
- [How to use it](#how-to-use-it)
  - [Parameters syntax](#parameters-syntax)
  - [Alexa details](#alexa-details)
  - [Google Home and other TTS details](#google-home-and-other-tts-details)
  - [Companion app details](#companion-app-details)
  - [Pushover details](#pushover-details)
  - [HTML5 details](#html5-details)
  - [URL](#url)
  - [Attachments and camera snapshot](#attachments-and-camera-snapshot)
  - [Critical notifications](#critical-notifications)
  - [App actions](#app-actions)
- [Examples](#examples)

# Why this work
Notifications and announcments are a fundamental part of my smart home. So my first automations where full of redundant service calls to the app, to Alexa and eventually to other services.
Same message was specified at least two times, at every call.
A developer (as I am) is a lazy creature by definition. And multiple strings definitions where unacceptable. So multinotify was born.

But then it became much more than that! Alexa announcments without a separate volume where terrible. I use to automatically play soft music at my entrance to my home, at a low volume. If an announcment arrive at that volume it's nearly impossible to understand it. So all the Alexa part where born.

Then it became a challenge with myself to what could be done using yaml. And how to do things in an elegant way (like volume and play status backup/restore of every Alexa device involved without the need to define some helpers, I'm proud of that)

Then it was adopted and I began to implement other services and keep the work as clean as possible, in my (very) compressed time.

I wrote an [article about multinotify on my blog site](https://henriksozzi.it/2022/01/package-multinotify-notifiche-su-alexa-e-app/), it's in Italian language but you can translate using Google Chrome. The multinotify described there is usually older than the one you can find here.

# Dependencies
 - [Alexa Media Player](https://github.com/custom-components/alexa_media_player) only if you want to play announcments on Alexa devices
 - [Companion apps for Android or iOS](https://companion.home-assistant.io/) only if you want notifications to the apps. (note: **app ver. 2022.8 or later required for Android**)
 - Specific `set_state` python script (needed only for Alexa and Google Home/TTS announcments):
    - [File from @xannor repo](https://github.com/xannor/hass_py_set_state) or
    
    - [File from my website, with complete explanation in IT language](https://henriksozzi.it/2021/04/impostare-lo-stato-di-unentita-in-home-assistant/)
  
**WARNING**: check that your script is the correct one as many set_state you can find on the web doesn't allow to create entities like this specific one!
 - If you want to send your camera snapshot to Pushover you'll have to create the folder /config/tmp and you'll have to ensure that the path is included in [`allowlist_external_dirs`](https://www.home-assistant.io/docs/configuration/basic/#allowlist_external_dirs)
 - If you want to send your camera snapshot to HTML5 you'll have to create the folder /config/www/cam and you'll have to ensure that the path is included in [`allowlist_external_dirs`](https://www.home-assistant.io/docs/configuration/basic/#allowlist_external_dirs)

# Installation
## New installation
If you install this package for the first time you'll have to check that you have a correctly configured packages folder as explained [here](https://www.home-assistant.io/docs/configuration/packages/#create-a-packages-folder).

Please make sure you have all needed [Dependencies](#dependencies).

Then you'll have to create the `multinotify` folder inside your `packages` folder and copy there all the files in this repo or in the release zip file.

The last step you'll have to do will be to set [parameters](#changing-package-parameters) and [create UI cards](#create-ui-cards).

A restart of the core will be the last step to get you ready to use `multinotify`.

## Updating
Updating, at the moment, is a bit tricky as some parameters are set in the `multinotify_v3.yaml` itselft, at the beginning as anchors values.
This means that you'll have to do something like this:
  - Save a copy of your actual yaml files in the packages
  - Update files in the repo or from the release zip, overwriting those in your multinotify folder
  - Set again your anchor values at the beginning of the `multinotify_v3.yaml` file and groups in the `multinotify_groups_helpers.yaml`
  - Restart your core (best!) or reload scripts, automations and eventually helpers if you're in a hurry.

# Changing package parameters
When you first install this package or after an update you have to set some parameters to fit your needs. It's a fast and easy operation, just follow next two chapters.

## Anchor parameters
In the `multinotify_v3.yaml` file you can see some anchor parameters at the beginning. This is how they looks like:
```
homeassistant:
  customize:
    package.node_anchors:
      tts_default_service: &tts_default_service "tts.cloud_say"
      default_volume: &default_volume 0.6
      iphones: &iphones ""
      iphones_critical_volume: &iphones_critical_volume 1.0
```
Here the definition of each parameters:
 - **tts_default_service**: you can set "tts.cloud_say" to use Nabu Casa wonderful TTS voices or "tts.google_translate_say" to use google free service. What you set here will be the default if you'll not specify otherwise in script parameters.
 - **default_volume**: this is the default volume for Alexa and TTS services like Google Home when you'll not specify otherwise in script parameters.
 - **iphones**: this is a list of iphone notify services separated by comma. This will be used by the script to understand if a service is an Android or iOS app, to use correct syntax for each one. If you have notifications groups of homogeneous apps (iOS only) then specify them too.
 Example value: `notify.my_iphone,notify.her_iphone,notify.all_iphones`. If you don't have iphones then simply specify an empty string `""`
 - **iphones_critical_volume**: the volume used for critical notification (iOS only) if not otherwise specified in script parameters.

## Alexa, Google Home and apps groups
After you eventually set anchor parameters you'll have to create some groups, if you want simplify sending notifications to groups of homogeneus devices.
You'll do this operation by editing the `multinotify_groups_helpers.yaml`.

Most important groups you'll have to create are Alexa speakers group you have defined in your Alexa app, in the devices tab, on the bottom of the page.
**Each Alexa speaker group you defined in the Alexa app must have a corresponding group in Home Assistant with the same name**, so that multinotify can know what devices are inside them and process them correctly.

Then, in the notify: seciont, you can create notify groups to send notifications to multiple apps.
Please don't create notify groups with heterogeneus apps (Android / iOS) but only groups of Android only or iOS only.
If you create iOS notify group please remember to add it to the [iphones anchor](#anchor-parameters), so that multinotify know that this specific group must use iOS syntax.

# Create UI cards
With this package there are two provided cards for your convenience.
First ensure that you have this module installed in your Home Assistant instance: [vertical-stack-in-card](https://github.com/ofekashery/vertical-stack-in-card).
The most easy way for doing this is by using [HACS](https://hacs.xyz)

To create provided cards in your UI please folloe this  super easy procedure. Just repeat the following steps for each file in the `UI` folder:
  - Go in the view of the dashboard where you want to put the card on
  - On the top right corner press the three dots icon, then "edit dashboard"
  - On the bottom right of the screen press the *Add card* button
  - From the card type dialog scroll to the bootom and select "manual"
  - copy and paste the content of the yml file in the UI folder into the code on the dialog, then press "SAVE" button on the bottom right.

There are two cards at the moment:

## multinotify_card.yml
A card useful to play an announcment to Alexa/TTS devices. You'll have to modify the `multinotify_ui.yaml` file according to what devices you want to see in the list.

![Multinotify card screenshot](https://raw.githubusercontent.com/energywave/multinotify/main/.github/multinotify_card.PNG?raw=true)

## multinotify_config_card.yml
A card to configure multinotify parameters. Most importantly the *Do Not Disturb* (or DND) time period.

![Multinotify config card screenshot](https://github.com/energywave/multinotify/blob/main/.github/multinotify_config_card.PNG?raw=true)

# How to use it
Let's see how to use multinotify by starting with all parameters definition and following with more details of every notification platform supported.
## Parameters syntax
You can call multinotify as a standard script with parameters like this:
```
- service: script.multinotify
  data:
    title: 'Il mio primo messaggio'
    message: 'Ciao mondo!'
    alexa_target: media_player.pian_terreno
    notify_app: |
      - notify.my_android
      - notify.his_ios
```
But there are many optional parameters you can use! Please read the following table to have a complete documentation of them.

| Param name     | Component  | Description |
| :------------- | :--------- | :---------- |
| `title`        | All        | The title of shown notifications
| `message`      | All        | The message pronounced or shown in the body. You can set the special value `"clear_notification"` to remove notification with given tag on Android, iOS and Persistent notifications. **This is the only mandatory parameter**
| `alexa_target` | Alexa      | Specify this to pronounce the message by Alexa. Please refer to [Alexa details](#alexa-details) section for possible values
| `alexa_message`| Alexa      | Specify only if you want Alexa to pronounce something different from `message` parameter, for example when using [SSML tags](https://developer.amazon.com/en-US/docs/alexa/custom-skills/speech-synthesis-markup-language-ssml-reference.html)
| `alexa_type`   | Alexa      | You can specify `announce` or `tts` to use a specific type of announce. If not specified `tts` will be used. (please refer to the [Alexa Media Player](https://github.com/custom-components/alexa_media_player/wiki/Configuration%3A-Notification-Component#functionality) documentation)
| `alexa_volume` | Alexa      | If not specified the volume of the announce will be what you defined in `default_volume` [anchor parameter](#anchor-parameters). Otherwise specify here the volume of the announce using a value between 0.1 and 10.0
| `alexa_force`  | Alexa      | If you specify `true` the announce will be played even if the time is in Do Not Disturb interval. If not specified or specified `false` the announce will be played only if time is not in Do Not Disturb time interval.
| `tts_target`   | Google / TTS | Specify this to pronounce the message by Google Home or other TTS based devices. Please refer to [Google Home and other TTS details](#google-home-and-other-tts-details) section for possible values
| `tts_message`  | Google / TTS | Specify only if you want Google Home or TTS device to pronounce something different from the `message` parameters.
| `tts_volume`  | Google / TTS | If not specified the volume of the announce will be what you defined in `default_volume` [anchor parameter](#anchor-parameters). Otherwise specify here the volume of the announce using a value between 0.1 and 10.0
| `tts_service`  | Google / TTS | If not specified the service defined in `tts_default_service` [anchor parameter](#anchor-parameters) will be used. Otherwise specify here `tts.cloud_say` if you have a [Nabu Casa](https://www.nabucasa.com/) active subscription or `tts.google_translate_say` otherwise.
| `tts_force`  | Google / TTS | If you specify `true` the announce will be played even if the time is in Do Not Disturb interval. If not specified or specified `false` the announce will be played only if time is not in Do Not Disturb time interval.
| `notify_app`  | Android / iOS App | Specify to send a notification to one or more companion app. Please refer to the [Companion app details](#companion-app-details) section for possible values.
| `notify_app_tts` | Android | If you set to true on Android app, the message will be pronounced with TTS instead of showing a notification. If you want to pronounce at max volume ignoring Do Not Disturb use with `critical: true`. |
| `notify_pushover`  | Pushover | Specify to send a notification to a specific Pushover service. *Example: `notify_pushover: "notify.pushover"`*. More info on the [Pushover details](#pushover-details) section.
| `notify_html5`  | HTML5 | Specify to send a notification to a registered HTML5 browser. PLease refer to the [HTML5 details](#html5-details) section for possible values.
| `notify_ha` | Persistent | Set to `true` if you want to send a [persistent notification](https://www.home-assistant.io/integrations/persistent_notification/) to Home Assistant.
| `icon`  | Android / HTML5 | The name of the file to use as an icon on the notification. The file must exist in `/config/www/notify_NAME.png` (where `NAME` is what you specify in this parameter). If not specified a value will be inherited, in order of precedence, by `channel`, `group` or `"info"` constant.
| `tag`  | Android / iOS / HTML5 / Persistent | If you specify a value and you send another notification with same `tag` value the previous notification will be replaced by the new. You can then remove a notification by sending a notification with special message `clear_notification` and desired `tag` to remove.
| `group`  | Android / iOS | If you specify a value all notification with same `group` value will be shown grouped on the phone. If you don't specify a value `channel` will be used for group or `"info"` if no `channel` specified.
| `channel`  | Android | On Android this parameter will identify a different notification channel. For every channel, in app settings, you can set different sound and settings. If you don't specify a value then `group` will be used or `"info"` if no `group` specified. Please read about channel and critical in [Companion App Details](#companion-app-details)
| `subtitle`  | Android / iOS | Here you can specify a subtitle on iOS and a Subject on Android
| `critical`  | Android / iOS / HTML5 | True to send a notification that will ignore the "Do Not Disturb" mode of your phone and will have maximum priority. Please read about critical and channel in [Companion app details](#companion-app-details).
| `critical_volume`  | iOS | Only on iOS you can specify a volume of `critical` notification sound. If you don't specify this and send a critical notification the iphones_critical_volume value specified in [anchor parameters](#anch]) will be used
| `url`  | Android / iOS / Pushover / HTML5 | If you specify a value when the user will click the notification this url will be opened. To open a view of your dashboard use the value `"/lovelace/view_path"` where `lovelace` is the name of your dashboard and `view_path` is the path of your view. On Android you can open an app using `"app://<package name>"`, a "more info panel" of an entity with `"entityId:<entity_id>"` or notifycation history with `settings://notification_history`. More info [here](https://companion.home-assistant.io/docs/notifications/notifications-basic#opening-a-url).
| `app_actions`  | Android / iOS / HTML5 | You can specify a list of actions that users can click and Home Assistant will receive the event. More info on the [Companion app details](#companion-app-details) section.
| `attachment`  | Android / iOS / Pushover / HTML5 | Specify a relative or absolute url to attach an image, video or audio with the notification (every platform has it's own limits). If you specify the entity id of a camera a snapshot will be attached (in the proper way for each notification platform). More info on the [Companion app details](#companion-app-details) section.

## Alexa details
Multinotify simulate an announcment volume in Alexa. To do that the sequence is the following, when you play an announce with multinotify:
- A backup of current volume and play status for every device object of the announce is done
- The announce is played
- Multinotify will wait for the time the announcment is being played
- The volume of every involved device is restored and, if it was playing music, will resume at the point it was.
For doing that it's important we know what devices are in groups you'll use. While groups has that information you can use speaker groups defined in the app that are available in Home Assistant as a media_player entity, like it is an Alexa device.
For them no devices is available, that's why you'll have to define with a group of the same name the devices involved as explained in [Alexa, Google Home and apps groups](#alexa-google-home-and-apps-groups) section.

The parameter you must set to use Alexa notification in multinotify is `alexa_target`. But you can use it in multiple ways, here all the possibilities with some examples.
### Send to a single Alexa speaker
```
service: script.multinotify
data:
  message: Hey dude, this is a test!
  alexa_target: media_player.my_alexa_speaker
```
In this example you'll play the announcment only on the `my_alexa_speaker` device.

### Send to a list of Alexa speakers
In this example we'll play the announce to the three specified devices:
```
service: script.multinotify
data:
  message: Hey dude, this is a test!
  alexa_target:
    - media_player.my_first_alexa
    - media_player.my_second_alexa
    - media_player.my_third_alexa
```

### Send to a Home Assistant defined group
In this example we'll play the announce on all devices defined in the group.
Please be aware that the announce will not be synchronized between devices this way.
```
service: script.multinotify
data:
  message: Hey dude, this is a test!
  alexa_target: group.my_alexa_devices
```

### Send to Alexa speakers group defined in the app
In this example we'll play the announce in all devices in the `ovunque` group defined in the alexa app.
The advantage of using those kind of groups is that, as long as you use `announce` (or not specify it) in `alexa_type`, the pronounciation of the announce will be synchronized between all devices.
```
service: script.multinotify
data:
  message: Hey dude, this is a test!
  alexa_target: media_player.ovunque
```

### Other information
You can use the `alexa_message` param to specify a different text that Alexa will pronounce. It can be useful in the following cases:
- Alexa isn't pronouncing something correctly. You can then write it differently only to be pronounced by Alexa.
- You want to use [SSML](https://developer.amazon.com/en-US/docs/alexa/custom-skills/speech-synthesis-markup-language-ssml-reference.html) to give some life to Alexa announcments but you don't want to read SSML tags on your app notifications, right? ;)

Let's visualize it with an example:
```
service: script.multinotify
data:
  message: The master is back home!
  alexa_target: media_player.pian_terreno
  alexa_message: <say-as interpret-as="interjection">abracadabra!</say-as> The master is back home!
  notify_app: notify.my_phone
```

You can use `alexa_type: announce` to play synchronized announcments between all involved devices, but a "ding" will be played by Alexa before the announcment.
Otherwise, if you use `alexa_type: tts` then you'll not hear anything before the announcment but involved devices will not be synchronized.

Please note that if you send a normal notification like, for example, the "washmachine is done" then it's ok that it will not be played in DND times (as defined in the [card](#create-ui-cards)). But if you play something urgent like "Flooding in the bathroom!" then you want to specify `alexa_force: true` that will let the announce to play ignoring DND times.

## Google Home and other TTS details
Multinotify will set the volume for the announcment but is not yet able to restore the previous volume after the announcment due to a limitation of the Google Home platform. I'm working on implementing it, however, by doing some magic :)
If Google devices are playing music they will stop it (that's by design, it cannot be done in a different way) but multinotify will not resume the music as it's **very** difficult and will need more code that what is present in multinotify itself. Yes I'm sorry to about that but that's a small sacrifice.

Ok, to play announcment on Google Home / TTS devices when calling multinotify you should use the `tts_target` parameter, and you can do it in the following ways:

### Send to a single Google Home / TTS device
```
service: script.multinotify
data:
  message: Hey dude, this is a test!
  tts_target: media_player.my_google_home_device
```
In this example you'll play the announcment only on the `my_google_home_device` device.

### Send to a list of Google Home / TTS speakers
In this example we'll play the announce to the three specified devices:
```
service: script.multinotify
data:
  message: Hey dude, this is a test!
  tts_target:
    - media_player.my_first_google_home
    - media_player.my_second_google_home
    - media_player.my_third_google_home
```

### Send to a Home Assistant defined group
In this example we'll play the announce on all devices defined in the group.
```
service: script.multinotify
data:
  message: Hey dude, this is a test!
  tts_target: group.my_google_home_devices
```

### Other information
As well as with Alexa you can specify a different message to be pronounces even with Google Home / TTS devices by using the `tts_message`.

Please use the correct `tts_service` by specifying the most used one in the [anchor parameters](#anchor-parameters) or by overriding it by specifying the `tts_service`.
The options you have are the folowing:
- `tts.cloud_say`: you can use that only if you have an active [Nabu Casa](https://www.nabucasa.com/) subscription. This is the best choice, in my opinion. Very natural and understandable TTS!
- `tts.google_translate_say`: this is a free service using Google Translate voice. Not bad but not as good as the previous one.

Please note that if you send a normal notification like, for example, the "washmachine is done" then it's ok that it will not be played in DND times (as defined in the [card](#create-ui-cards)). But if you play something urgent like "Flooding in the bathroom!" then you want to specify `tts_force: true` that will let the announce to play ignoring DND times.

## Companion app details
If you want to send a notification to one or more app that have installed the Android or iOS Home Assistant Companion app you have to specify the `notify_app` parameter. You can use it in these ways:

### Send to a single phone
You can send the notification to a single phone by using this syntax:
```
service: script.multinotify
data:
  title: My notification title
  message: This is the message I want you to read!
  notify_app: notify.my_phone
```

### Send to a list of phones
You can even specify a list of recipients for your notification.
You can mix Android and iOS devices, the correct syntax will be automatically used for each of them (as long as you set what are your iOS notify services in the [anchor parameters](#anchor-parameters))
```
service: script.multinotify
data:
  title: My notification title
  message: This is the message I want you to read!
  notify_app:
    - notify.my_android_phone
    - notify.his_ios_phone
```

### Send to a group of phones
If you define a group of phones in the `multinotify_groups_helpers.yaml` file then you can use it as a recipient. A notification will be sent to all members of that group.

**Please note**: each notify group must be of the same type. Don't mix Android and iOS devices in a group!
```
service: script.multinotify
data:
  title: My notification title
  message: This is the message I want you to read!
  notify_app: notify.all_phones
```

### Channel and Critical on Android: please read carefully
On Android channel settings are created when first notification arrive to the app of that channel.
So, for example, if you send the first notification of the channel `alarm` with the parameter `critical: true` the channel will be configured to ignore *Do Not Disturb* of the phone.
But if you already sent at least one notification with channel `alarm` without `critical: true` then the channel was configured to not bypass *Do Not Disturb* mode of the phone and you must set that option manually by going in android settings, app settings, Home Assistant, notifications, your channel name and then set that option.

Please note that every Android manufacturer do some customizations on the settings so precise instruction cannot be writed here.

**Android app version 2022.8 or later required**

## Pushover details
To send a notification to a [connected pushover app](https://www.home-assistant.io/integrations/pushover/) you have to set the `notify_pushover` parameter.
At the moment you can only specify the name of the pushover service to wich you want to send, like this:
```
service: script.multinotify
data:
  title: My notification title
  message: This is the message I want you to read!
  notify_pushover: notify.my_pushover
```
Multinotify will do it's best to use all the pushover features to use all specified parameters like `attachment`, `title`, `message`, `url` and `critical`.
## HTML5 details
To send notification to a configured browser by using the [HTML5 push notification service](https://www.home-assistant.io/integrations/html5/) you have to specify the `notify_html5` parameter.

I'll describe all the ways you can specify the parameter:
### Send to a single browser
You can send the notification to a single registered browser by using this syntax:
```
service: script.multinotify
data:
  title: My notification title
  message: This is the message I want you to read!
  notify_html5: notify.my_pc
```

### Send to a list of browsers
You can even specify a list of recipients for your notification.
```
service: script.multinotify
data:
  title: My notification title
  message: This is the message I want you to read!
  notify_html5:
    - notify.my_home_pc
    - notify.my_work_pc
```

### Send to a group of browsers
If you define a group of html5 browsers in the `multinotify_groups_helpers.yaml` file then you can use it as a recipient. A notification will be sent to all members of that group.

```
service: script.multinotify
data:
  title: My notification title
  message: This is the message I want you to read!
  notify_app: notify.all_my_computers
```
## URL
For notifications shown on phone or PC you can specify the `URL` parameter to open a specific website, app or home assistant page when the notification is clicked.
These the possible values:
- A relative URL to your Home Assistant instance. For example `/lovelace/my_view` to show *my_view* view in the *lovelace* dashboard.
- A full URL like `https://henriksozzi.it`
- (android only) open a specific app by using `app://<package name>`
- (android only) open the "more info panel" of an entity by using `entityId:<entity_id>`. For example: `entityId:sun.sun`
- (android only) open the notifications history by using `settings://notification_history`


## Attachments and camera snapshot
The `attachment` parameter is very powerful. You can specify an attachment of your notification in multiple ways. Multinotify will take care of the different syntax of every platform supported to achieve the result.
Here the various possibilities:
- An absolute url, for example: `https://henriksozzi.it/wp-content/uploads/2020/12/Henrik-Sozzi-small.jpg`. Not every platform supports every type of attachment. For example iOS accepts image (jpg, gif, png), video (mp4, mpg, avi), audio (aiff, wav, mp3) but Android only supports image and video and HTML5 and Pushover only supports image.
- A relative url. For example: `/local/my_file.jpg` means you have to attach a file located in `/config/www/my_file.jpg`
- A camera `entity_id`. For example `camera.living_room`. By doing that multinotify will automatically use best method for every platform involved to attach a snapshot of the camera, just as simple as that!
On iOS, when you enlarge the notification, a live preview of the camera will be shown.

## Critical notifications
Critical notifications are notification that have more importance like an anti-theft alarm notification or a water leak sensor detected.
These notifications ignore the dnd setting of the phone and will play the sound you've set, with maximum priority and importance.
On HTML5 platform the notification will not go away until you interact with it.
It's as easy as to set `critical: true`

## App actions
With app actions it's possible to set users possible response to the notification that, when pressed, can be processed with an automation on Home Assistant and do what you like.
For example you can send a notification like `You've left the home but the garage door is open. Do you want me to close it?` and let the user choose between `Yes please` and `No, thank you`.
Limitations to every platform apply, for example iOS support a maximum of 10 options while Android 3 and HTML5 2 options.
For more info please read [Companion App dedicated page](https://companion.home-assistant.io/docs/notifications/actionable-notifications) and [HTML5 Push Notification actions](https://www.home-assistant.io/integrations/html5/#actions)

This example shows what was described above:
```
service: script.multinotify
data:
  title: Security checks
  message: "You've left the home but the garage door is open. Do you want me to close it?"
  notify_app: notify.my_phone
  app_actions:
    - action: "YES"
      icon: /local/yes.png
      title: "Yes, please"
    - action: "NO"
      icon: /local/no.png
      title: "No, thank you"
```
Please note that icon will work on Android and HTML5 in this way. iOS has a limitation of SF Symbols library described [here](https://companion.home-assistant.io/docs/notifications/actionable-notifications/#icon-values).

To handle the press of an action by the user you'll have to create an automation like this (in this example the "YES" option) for Companion apps:
```
- trigger:
    - platform: event
      event_type: mobile_app_notification_action
      event_data:
        action: YES
  action:
    - service: cover.close_cover
      target:
        entity_id: cover.garage
```
And like this for HTML5:
```
- trigger:
    - platform: event
      event_type: html5_notification.clicked
      event_data:
        action: YES
    action:
      - service: cover.close_cover
        target:
          entity_id: cover.garage
```

You can combine them in one automation only:
```
- trigger:
    - platform: event
      event_type: mobile_app_notification_action
      event_data:
        action: YES
    - platform: event
      event_type: html5_notification.clicked
      event_data:
        action: YES
    action:
      - service: cover.close_cover
        target:
          entity_id: cover.garage
```
If the "NO" action will do nothing there is no need to handle it with an automation.
