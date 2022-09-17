# CHANGELOG
Please read carefully when you upgrade what's new and if there is some usage change.
Please note that before [3.6.1](#ver-361---07092022) version italian language was used.

## Ver. 1.0 - 30/03/2021
 - Prima versione pubblicata qui: https://henriksozzi.it/2021/03/script-con-parametri-opzionali-in-home-assistant/

## Ver. 2.0 - 25/11/2021
 - Resa parametrica la lista dei dispositivi da processare per il volume (costato tanto sonno)
 - Aggiunto Pausa / Play della musica (idea di Massimiliano Albani, grazie!!!) per evitare
   che l'aumento del volume comporti un breve aumento anche della musica poco piacevole.
 - Aggiunto delay calcolato in base alla frase così che il ripristino del volume ed eventuali
   azioni seguenti al richiamo dello script avvengano dopo il termine dell'annuncio
   (ad esempio è ora possibile far dire ad Alexa "...ora ti alzo le tapparelle" e alzarle)
 - Impostato attributo volume_level forzatamente ad ogni cambio, così che quando capita
   che il websocket di Alexa Media Player venga perso il volume sia comunque corretto
   Ref: https://github.com/custom-components/alexa_media_player/issues/1429

## Ver. 2.0.1 - 10/12/2021
 - Modificata modalità in queued max 10 per permettere l'accodamento di più messaggi

## Ver. 3.0 - 02/02/2022
 - Integrazione Google Home (non riprende la musica dopo tts)

## Ver. 3.1 - 27/05/2022
 - Supportate notifiche ad iphone (sperimentale). Gli iphone vanno specificati nell'apposita variabile anchor
 - Aggiunto supporto a Pushover

## Ver. 3.2 - 04/08/2022 - Grazie a Francesco Calamita per le idee ed i test su iphone!!!
 - Migliorate notifiche app, in particolare su iphone. Il volume delle notifiche critiche su iphone
   è definibile dal parametro anchor iphones_critical_volume. Su Android le notifiche critiche
   usano ora il canale alarm_stream in override a quel che è stato eventualmente specificato e
   impostano il canale come alta importanza, colore rosso e led notifiche rosso.
 - ATTENZIONE! Il parametro critical di multinotify non ha più il significato precedente ma ora, se impostato a true,
   specifica una notifica che ignora la modalità silenziosa del telefono (sia Android che iPhone). Default = false.

## Ver. 3.3 - 09/08/2022
 - Aggiunti parametri critical_volume (solo iOS), subtitle (Android/iOS) e url (Android/iOS),
   per le notifiche all'app companion (credit: Francesco Calamita, GRAZIE!)
 - Nel calcolo della durata dell'annuncio Alexa implementata RegEx che rimuove tutti i TAG SSML eventualmente presenti.
   In questo modo il calcolo della durata è ora più fedele alla frase pronunciata anche in presenza di tag SSML.
 - Aggiunti input_datetime per orario inizio e fine DND (Do Not Disturb) ovvero gli orari in cui Alexa
   e TTS non parlano più. ATTENZIONE: partono senza un default (per renderli permanenti) quindi sarà
   necessario assegnarli un valore! Io uso 9:00 - 23:00.
 - Aggiunti input_text contenenti gli ultimi annunci Alexa e TTS, usabili da UI (stato) o da
   riprodurre su Alexa e TTS (attributo message). La differenza tra i due è l'eventuale SSML, filtrato nello stato.

## Ver. 3.4 - 11/08/2022
 - Aggiunto parametro alexa_type che permette di scegliere se fare gli annunci con "announce" (con din don e sincronizzati tra più Echo)
   oppure "tts" (senza din don ma non sincronizzati tra più Echo)
 - Aggiunto supporto ad url e critical su PushOver (usa i parametri che già ci sono)
 - Aggiunto parametro app_actions. Il contenuto è yaml e va formattato come si formatterebbe per il servizio di notifica app companion.
 - Aggiunto parametro attachment. Si può specificare un file in www usando "/local/file.jpg" oppure in media usando "/media/local/video.mp4"
   o ancora su un sito web pubblico usando "https://henriksozzi.it/wp-content/uploads/2020/12/Henrik-Sozzi-small.jpg". Il tipo di allegato
   (immagine, video o audio) è automaticamente determinato dall'estensione del file.
   Ma soprattutto è possibile specificare l'entity_id di una propria telecamera per allegare automaticamente uno snapshot alla notifica
   su Android (usa il proxy) ed uno snapshot che quando espanso mostra lo streaming MPEG della cam su iOS.

## Ver. 3.4.1 - 11/08/2022
 - bugfix orari DND
 - Gli orari DND, se non impostati, vengono inizializzati a 23:00 - 09:00 (come prima di averli parametrici)
 - Card invio messaggi manuali tolta da multinotify_ui.yaml ed inserita in nuovo file multinotify_card.yml
 - Aggiunta card (dal design spartano ma funzionale) multinotify_card_config.yml utile in una plancia di configurazione

## Ver. 3.5 - 12/08/2022 - Grazie a Vito Schiavone per i test su iOS!
 - Ora notify_app può essere passato in due modi. Come prima ovvero `notify_app: notify.mio_telefono` oppure specificando più servizi
   anche misti tra android ed iOS nel modo seguente:
```
   notify_app:
     - notify.mio_android
     - notify.suo_ios
```
   Per ogni invio verrà valutato se è un iOS o no controllando in iphones (da impostare negli anchor più in basso)
   Si possono ancora usare i gruppi ma SOLO se contengono dispositivi omogenei più smartphone android o più smartphone iOS
   ma NON entrambi i tipi nello stesso gruppo. In tal caso specificare singolarmente i servizi come nell'esempio sopra.
 - bugfix se non passati allegati ad iOS

## Ver. 3.5.1 - 12/08/2022 - Grazie ancora a Vito Schiavone
 - Bugfixes notifiche ad iOS

## Ver. 3.6 - 12/08/2022
 - Implementate notifiche HTML5! Supporta servizi multipli (browser diversi), icone, tag, critical, url, azioni e allegati
   (immagini o entity_id di una telecamera: fa snapshot e lo invia!)
 - Reso funzionante attachment su pushover quando specificata una immagine o un'entity_id di una telecamera (fa snapshot e lo invia!)
 - Definitivamente risolti i problemi su iOS.

## Ver. 3.6.1 - 07/09/2022
**ENGLISH WILL BE USED FROM NOW ON!!!**
 - The all_alexa group is now renamed to the same name you have in "speaker group" in your Alexa app. For Italian Alexa users it's "Ovunque". This is not anymore a special case but is treated as a normal speaker group.
 - Renamed tmp entities by removing "tts_" as a prefix (as multinotify v2 did)
 - Changed comments and documentation
 
## Ver. 3.6.2 - 12/09/2022 - (BREAKING CHANGE)
 - **BREAKING CHANGE**: from this release multinotify is not anymore named `multinotify3` but `multinotify` as it had a different name in the beta phase to be available side to side with the stable version. Now with this version the code is considered stable so you cannot anymore place it side to side with older versions but you have to remove the older 2.0 version.
 - Used media_stream instead of channel for Android app critical notification to ignore dnd phone settings. This was a change in the app usage since 2022.8 version.
 
 ## Ver. 3.7.0 - 17/09/2022
  - Added parameter `notify_app_tts`. If you set it to true on Android phones the message will be pronounced instead of showing a notification. If you want to ignore the *Do Not Disturb* mode of the phone and play at full volume use with `critical: true`.
  - Added parameter `notify_persistent`. If you set it to `true` then a persistent notification will be created in Home Assistant (shown in the UI at the row *notifications* in the bottom of the menu)
  - You can now specify `message: "clear_notification"` by specifying a tag of a previous sent notification you want to remove. It will be removed on Android, iOS, Persistent notification.
  - Fix for critical notifications on Android. You need to specify `critical: true` and a `channel` that you can then assign a sound of your like.
  The channel will be configured as to bypass "Do Not Disturb" settings of the phone at first notification of the channel. If you already send notifications with this channel (without `critical: true`) then you have to set to bypass Do Not Disturb manually in notifications settings of the app, in Android settings. Now it uses `alarm_stream_max` instead of `alarm_stream` to play at highest volume possible. **Needs App ver 2022.8 or later**.