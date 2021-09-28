---
title: Invia notifiche utilizzando Adobe Sign per Microsoft Dynamics 365 e Marketo
description: Informazioni su come inviare un messaggio di testo, un'e-mail o una notifica push per far sapere al firmatario che è in corso un accordo
role: Admin
product: adobe sign
solution: Adobe Sign, Marketo, Document Cloud
level: Intermediate
topic-revisit: Integrations
thumbnail: KT-7249.jpg
exl-id: 2e0de48c-70bf-4dc5-8251-88e7399f588a
source-git-commit: bcddb0ee106239f2786debaed908b2a2ec5ce792
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 1%

---

# Invia notifiche utilizzando Adobe Sign per Microsoft Dynamics 365 e Marketo

Informazioni su come inviare un messaggio di testo, un&#39;e-mail o una notifica push per informare il firmatario che un accordo è in corso utilizzando Adobe Sign, Adobe Sign per Microsoft Dynamic, Marketo e Marketo Microsoft Dynamics Sync. Per inviare le notifiche da Marketo, è innanzitutto necessario acquistare o configurare una funzionalità di gestione di Marketo SMS. Questa procedura dettagliata utilizza [Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/), ma sono disponibili altre soluzioni Marketo SMS.

## Prerequisiti

1. Installare Marketo Microsoft Dynamics Sync.

   Le informazioni e il plug-in più recente per Microsoft Dynamics Sync sono disponibili [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html)

1. Installare Adobe Sign per Microsoft Dynamics.

   Le informazioni sul plug-in sono disponibili [qui.](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)

## Trova l&#39;oggetto personalizzato

Una volta completate le configurazioni Marketo Microsoft Dynamics Sync e Adobe Sign for Dynamics, vengono visualizzate due nuove opzioni nel terminale Amministrazione Marketo.

![Ammin.](assets/adminTerminal.png)

* Fare clic su **[!UICONTROL Sincronizza entità Dynamics]**.

   La sincronizzazione deve essere disattivata prima di sincronizzare le entità personalizzate. Fare clic su **[!UICONTROL Sincronizza schema]** se questa è la prima volta. In caso contrario, fare clic su **[!UICONTROL Aggiorna schema]**.

   ![Aggiorna](assets/refreshSchema.png)

## Sincronizza l&#39;oggetto personalizzato

1. Sul lato destro, individuare gli oggetti personalizzati basati su [!UICONTROL Lead], [!UICONTROL Contact] e [!UICONTROL Account].

   * **[!UICONTROL Abilitare]** Syncfor per gli oggetti in Lead se si desidera attivare quando un lead viene aggiunto a un accordo in Dynamics.

   * **[!UICONTROL Attivare]** Syncfor per gli oggetti in Contact se si desidera attivare quando un contatto viene aggiunto a un accordo in Dynamics.

   * **[!UICONTROL Attivare]** Syncfor per gli oggetti in Account se si desidera attivare quando un Account viene aggiunto a un accordo in Dynamics.

   * **Abilitare** Syncfor per l&#39;oggetto Agreement sotto il padre desiderato (Lead, contatto o Account).

   ![Oggetti personalizzati](assets/enableSyncDynamics.png)

1. Nella nuova finestra selezionare le proprietà desiderate in Contratto.

   Abilitare le caselle sotto **[!UICONTROL Vincoli]** e **[!UICONTROL Trigger]** per esporle alle attività di marketing.

   ![Sincronizzazione personalizzata 1](assets/entitySync1.png)

   ![Sincronizzazione personalizzata 2](assets/entitySync2.png)

1. Riattiva la sincronizzazione dopo aver attivato la sincronizzazione sugli oggetti personalizzati.

   Tornare al [!UICONTROL Admin Terminal], quindi fare clic su **[!UICONTROL Microsoft Dynamics]**, quindi fare clic su **[!UICONTROL Abilita sincronizzazione]**.

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![Abilita globale](assets/enableGlobalDynamics.png)

## Crea il programma

1. In [!UICONTROL Attività di marketing], fare clic con il pulsante destro del mouse su **[!UICONTROL Attività di marketing]** sulla barra a sinistra, selezionare **[!UICONTROL Nuova cartella campagna]** e denominarla.

   ![Nuova cartella](assets/newFolder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella creata, selezionare **[!UICONTROL Nuovo programma]** e assegnargli un nome.

   Lasciare tutto il resto come predefinito, quindi fare clic su **[!UICONTROL Crea]**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

## Imposta [!DNL Twilio] SMS

Assicurarsi innanzitutto di disporre di un account [!DNL Twilio] attivo e di aver acquistato le funzionalità SMS richieste.

L&#39;impostazione del webhook Marketo - [!DNL Twilio] SMS richiede tre [!DNL Twilio] parametri dall&#39;account.

* SID account
* Token account
* Numero di telefono Twilio

Recuperate questi parametri dall&#39;account e aprite l&#39;istanza Marketo.

1. Fare clic su **[!UICONTROL Admin]** in alto a destra.

   ![Ammin.](assets/adminTab.png)

1. Fare clic su **[!UICONTROL Webhooks]**, quindi su **[!UICONTROL Nuovo webhook]**.

   ![Webhook](assets/webhooks.png)

1. Immettere un **[!UICONTROL Nome webhook]** e **[!UICONTROL Descrizione]**.

1. Immettere l&#39;URL seguente e assicurarsi di sostituire le credenziali `ACCOUNT_SID` e `AUTH_TOKEN` con le credenziali [!DNL Twilio].

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. Selezionare **[!UICONTROL POST]** come tipo di richiesta.

1. Immettere il seguente **Modello** e assicurarsi di sostituire `MY_TWILIO_NUMBER` con il numero di telefono [!DNL Twilio] e `YOUR_MESSAGE` con un messaggio scelto.

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. Impostare la **[!UICONTROL Codifica token richiesta]** su *Maschera/URL*.

1. Impostare il tipo di risposta su *JSON*, quindi fare clic su **[!UICONTROL Salva]**.

## Imposta il trigger Smart Campaign

1. Nella sezione Attività di marketing fare clic con il pulsante destro del mouse sul programma creato, quindi selezionare **[!UICONTROL Nuova campagna]**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegnare un nome, quindi fare clic su **[!UICONTROL Crea]**.

   ![Smart Campaign 2](assets/smartCampaign3.png)

   È necessario visualizzare diversi trigger disponibili per l&#39;utilizzo nella cartella Microsoft.

1. Fare clic e trascinare **[!UICONTROL Aggiunta a Contratto]** all&#39;elenco **[!UICONTROL Smart List]**, quindi aggiungere i vincoli che si desidera avere sul trigger.

   ![Aggiunta a contratto](assets/addedToAgreementDynamics.png)

## Imposta il flusso di Smart Campaign

1. Fare clic sulla scheda **[!UICONTROL Flusso]** nella [!UICONTROL Campagna intelligente].

   Cercare e trascinare il flusso **Call Webhook** nell&#39;area di lavoro e selezionare il webhook creato nella sezione precedente.

   ![Chiama Webhook](assets/callWebhook.png)

1. La campagna di notifica via SMS per i lead che vengono aggiunti a un accordo è ora impostata.
>[!TIP]
>
>Questa esercitazione fa parte del corso [Accelerare i cicli di vendita con Adobe Sign per Microsoft Dynamics e Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) disponibile gratuitamente su Experience League!