---
title: Inviare notifiche utilizzando Acrobat Sign per Microsoft Dynamics 365 e Marketo
description: Scopri come inviare un messaggio di testo, un’e-mail o una notifica push per informare il firmatario che è in arrivo un accordo
feature: Integrations
role: Admin
solution: Acrobat Sign, Marketo Engage, Document Cloud
level: Intermediate
topic: Integrations
topic-revisit: Integrations
jira: KT-7249
thumbnail: KT-7249.jpg
exl-id: 2e0de48c-70bf-4dc5-8251-88e7399f588a
source-git-commit: a88ec5a68aa2a02ec2f118332ec31f47d3d5d300
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 0%

---

# Inviare notifiche utilizzando Acrobat Sign per Microsoft Dynamics 365 e Marketo

Scopri come inviare un messaggio di testo, un’e-mail o una notifica push per informare il firmatario che un accordo è in arrivo utilizzando Acrobat Sign, Acrobat Sign per Microsoft Dynamic, Marketo e Marketo Microsoft Dynamics Sync. Per inviare notifiche da Marketo, è innanzitutto necessario acquistare o configurare una funzionalità di gestione SMS di Marketo. In questa procedura dettagliata vengono utilizzati [Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/), ma sono disponibili altre soluzioni Marketo SMS.

## Prerequisiti

1. Installa Marketo Microsoft Dynamics Sync.

   Le informazioni e il plug-in più recente per Microsoft Dynamics Sync sono disponibili [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html?lang=it)

1. Installa Acrobat Sign per Microsoft Dynamics.

   Le informazioni su questo plug-in sono disponibili [qui.](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)

## Trovare l’oggetto personalizzato

Una volta completate le configurazioni di Marketo Microsoft Dynamics Sync e Acrobat Sign per Dynamics, nel Terminale di amministrazione Marketo vengono visualizzate due nuove opzioni.

![Amministratore](assets/adminTerminal.png)

* Fare clic su **[!UICONTROL Dynamics Entities Sync]**.

  La sincronizzazione deve essere disattivata prima di sincronizzare le entità personalizzate. Se è la prima volta, fai clic su **[!UICONTROL Sincronizza schema]**. In caso contrario, fare clic su **[!UICONTROL Aggiorna schema]**.

  ![Aggiorna](assets/refreshSchema.png)

## Sincronizzare l&#39;oggetto personalizzato

1. Sul lato destro, individua gli oggetti personalizzati basati su [!UICONTROL Lead], [!UICONTROL Contact] e [!UICONTROL Account].

   * **[!UICONTROL Abilita la sincronizzazione]** per gli oggetti in Lead se desideri attivarla quando un lead viene aggiunto a un accordo in Dynamics.

   * **[!UICONTROL Abilita la sincronizzazione]** per gli oggetti in Contatto se desideri attivare l’attivazione quando un Contatto viene aggiunto a un accordo in Dynamics.

   * **[!UICONTROL Abilita la sincronizzazione]** per gli oggetti in Account se desideri attivarla quando un account viene aggiunto a un accordo in Dynamics.

   * **Abilita sincronizzazione** per l’oggetto accordo nel principale desiderato (lead, contatto o account).

   ![Oggetti personalizzati](assets/enableSyncDynamics.png)

1. Nella nuova finestra, seleziona le proprietà desiderate in Accordo.

   Abilita le caselle in **[!UICONTROL Vincolo]** e **[!UICONTROL Attivazione]** per esporle alle tue attività di marketing.

   ![Sincronizzazione personalizzata 1](assets/entitySync1.png)

   ![Sincronizzazione personalizzata 2](assets/entitySync2.png)

1. Riattiva la sincronizzazione dopo aver attivato la sincronizzazione sugli oggetti personalizzati.

   Torna al [!UICONTROL Terminale amministratore], fai clic su **[!UICONTROL Microsoft Dynamics]**, quindi su **[!UICONTROL Attiva sincronizzazione]**.

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![Abilita globale](assets/enableGlobalDynamics.png)

## Creazione del programma

1. In [!UICONTROL Attività di marketing], fare clic con il pulsante destro del mouse su **[!UICONTROL Attività di marketing]** sulla barra di sinistra, selezionare **[!UICONTROL Nuova cartella campagna]** e denominarla.

   ![Nuova cartella](assets/newFolder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella creata, selezionare **[!UICONTROL Nuovo programma]** e assegnare un nome.

   Lasciare tutto il resto come predefinito, quindi fare clic su **[!UICONTROL Crea]**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

## Configura [!DNL Twilio] SMS

Assicurati innanzitutto di disporre di un account [!DNL Twilio] attivo e di aver acquistato le funzionalità SMS necessarie.

La configurazione del webhook Marketo - [!DNL Twilio] SMS richiede tre [!DNL Twilio] parametri dal tuo account.

* SID account
* Token account
* Numero di telefono Twilio

Recupera questi parametri dal tuo account. Ora apri l’istanza di Marketo.

1. Fai clic su **[!UICONTROL Amministratore]** in alto a destra.

   ![Amministratore](assets/adminTab.png)

1. Fai clic su **[!UICONTROL Webhook]**, quindi su **[!UICONTROL Nuovo webhook]**.

   ![Webhook](assets/webhooks.png)

1. Immetti un **[!UICONTROL Nome webhook]** e una **[!UICONTROL Descrizione]**.

1. Immetti il seguente URL e assicurati di sostituire `ACCOUNT_SID` e `AUTH_TOKEN` con le tue credenziali [!DNL Twilio].

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. Seleziona **[!UICONTROL POST]** come tipo di richiesta.

1. Immetti il seguente **modello** e assicurati di sostituire `MY_TWILIO_NUMBER` con il tuo numero di telefono [!DNL Twilio] e `YOUR_MESSAGE` con un messaggio di tua scelta.

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. Imposta **[!UICONTROL Codifica token di richiesta]** su *Modulo/URL*.

1. Imposta il tipo di risposta su *JSON*, quindi fai clic su **[!UICONTROL Salva]**.

## Configurazione del trigger campagna avanzata

1. Nella sezione Attività di marketing fare clic con il pulsante destro del mouse sul programma creato, quindi selezionare **[!UICONTROL Nuova campagna avanzata]**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegnale un nome, quindi fai clic su **[!UICONTROL Crea]**.

   ![Smart Campaign 2](assets/smartCampaign3.png)

   Dovresti visualizzare diversi trigger disponibili per l&#39;uso nella cartella Microsoft.

1. Fai clic e trascina **[!UICONTROL Aggiunto all&#39;accordo]** nell&#39;**[!UICONTROL elenco smart]**, quindi aggiungi eventuali vincoli che desideri avere sul trigger.

   ![Aggiunto all&#39;accordo](assets/addedToAgreementDynamics.png)

## Impostare il flusso della campagna intelligente

1. Fare clic sulla scheda **[!UICONTROL Flusso]** nella [!UICONTROL Smart Campaign].

   Cerca e trascina il flusso **Webhook di chiamata** nell&#39;area di lavoro e seleziona il webhook creato nella sezione precedente.

   ![Chiama Webhook](assets/callWebhook.png)

1. La campagna di notifica SMS per i lead aggiunti a un accordo è ora configurata.
