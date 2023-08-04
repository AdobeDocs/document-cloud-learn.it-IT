---
title: Inviare notifiche utilizzando Acrobat Sign per Microsoft Dynamics 365 e Marketo
description: Scopri come inviare un messaggio di testo, un’e-mail o una notifica push per informare il firmatario che è in arrivo un accordo
feature: Integrations
role: Admin
solution: Acrobat Sign, Marketo, Document Cloud
level: Intermediate
topic: Integrations
topic-revisit: Integrations
jira: KT-7249
thumbnail: KT-7249.jpg
exl-id: 2e0de48c-70bf-4dc5-8251-88e7399f588a
source-git-commit: 452299b2b786beab9df7a5019da4f3840d9cdec9
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 1%

---

# Inviare notifiche utilizzando Acrobat Sign per Microsoft Dynamics 365 e Marketo

Scopri come inviare un messaggio di testo, un’e-mail o una notifica push per informare il firmatario che un accordo è in arrivo utilizzando Acrobat Sign, Acrobat Sign per Microsoft Dynamic, Marketo e Marketo Microsoft Dynamics Sync. Per inviare notifiche da Marketo, è innanzitutto necessario acquistare o configurare una funzionalità di gestione SMS di Marketo. In questa procedura dettagliata vengono utilizzati [Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/), ma sono disponibili altre soluzioni Marketo SMS.

## Prerequisiti

1. Installa Marketo Microsoft Dynamics Sync.

   Sono disponibili le informazioni e il plug-in più recente per Microsoft Dynamics Sync [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html)

1. Installa Acrobat Sign per Microsoft Dynamics.

   Sono disponibili informazioni su questo plug-in [qui.](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)

## Trovare l’oggetto personalizzato

Una volta completate le configurazioni di Marketo Microsoft Dynamics Sync e Acrobat Sign per Dynamics, nel Terminale di amministrazione Marketo vengono visualizzate due nuove opzioni.

![Amministrazione](assets/adminTerminal.png)

* Fai clic **[!UICONTROL Sincronizzazione delle entità Dynamics]**.

  La sincronizzazione deve essere disattivata prima di sincronizzare le entità personalizzate. Fai clic **[!UICONTROL Sincronizza schema]** se è la prima volta. In caso contrario, fare clic su **[!UICONTROL Aggiorna schema]**.

  ![Aggiorna](assets/refreshSchema.png)

## Sincronizzare l&#39;oggetto personalizzato

1. Sul lato destro, individua [!UICONTROL Lead], [!UICONTROL Contatto]e [!UICONTROL Account]oggetti personalizzati basati su.

   * **[!UICONTROL Attiva sincronizzazione]** per gli oggetti in Lead, se desideri attivare questa opzione quando un lead viene aggiunto a un accordo in Dynamics.

   * **[!UICONTROL Attiva sincronizzazione]** per gli oggetti in Contatto se desideri attivare l’attivazione quando un Contatto viene aggiunto a un accordo in Dynamics.

   * **[!UICONTROL Attiva sincronizzazione]** per gli oggetti in Account se desideri attivare quando un Account viene aggiunto a un accordo in Dynamics.

   * **Attiva sincronizzazione** per l’oggetto Accordo nell’elemento principale desiderato (Lead, Referente o Account).

   ![Oggetti personalizzati](assets/enableSyncDynamics.png)

1. Nella nuova finestra, seleziona le proprietà desiderate in Accordo.

   Abilita le caselle sotto **[!UICONTROL Vincolo]** e **[!UICONTROL Attivazione]** per esporli alle tue attività di marketing.

   ![Sincronizzazione personalizzata 1](assets/entitySync1.png)

   ![Sincronizzazione personalizzata 2](assets/entitySync2.png)

1. Riattiva la sincronizzazione dopo aver attivato la sincronizzazione sugli oggetti personalizzati.

   Torna al [!UICONTROL Terminale amministratore], quindi fare clic su **[!UICONTROL Microsoft Dynamics]**, quindi fare clic su **[!UICONTROL Attiva sincronizzazione]**.

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![Abilita globale](assets/enableGlobalDynamics.png)

## Creazione del programma

1. Ingresso [!UICONTROL Attività di marketing], clic con il pulsante destro del mouse **[!UICONTROL Attività di marketing]** sulla barra di sinistra, seleziona **[!UICONTROL Nuova cartella campagna]** e denominalo.

   ![Nuova cartella](assets/newFolder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella creata e selezionare **[!UICONTROL Nuovo programma]** e assegnagli un nome.

   Lascia tutti gli altri come predefiniti, quindi fai clic su **[!UICONTROL Crea]**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

## Configura [!DNL Twilio] SMS

Per prima cosa assicurati di avere un [!DNL Twilio] e ha acquistato le funzionalità SMS necessarie.

Configurazione di Marketo - [!DNL Twilio] Il webhook SMS richiede tre [!DNL Twilio] parametri del tuo account.

* SID account
* Token account
* Numero di telefono Twilio

Recupera questi parametri dal tuo account. Ora apri l’istanza di Marketo.

1. Fai clic **[!UICONTROL Amministratore]** in alto a destra.

   ![Amministrazione](assets/adminTab.png)

1. Fai clic **[!UICONTROL Webhook]**, quindi fare clic su **[!UICONTROL Nuovo webhook]**.

   ![Webhook](assets/webhooks.png)

1. Immetti un **[!UICONTROL Nome webhook]** e **[!UICONTROL Descrizione]**.

1. Immetti il seguente URL e assicurati di sostituire quello `ACCOUNT_SID` e `AUTH_TOKEN` con [!DNL Twilio] credenziali.

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. Seleziona **[!UICONTROL POST]** come tipo di richiesta.

1. Immetti quanto segue **Modello** e assicurati di sostituire `MY_TWILIO_NUMBER` con [!DNL Twilio] numero di telefono e `YOUR_MESSAGE` con un messaggio di tua scelta.

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. Impostare la **[!UICONTROL Codifica token di richiesta]** a *Modulo/URL*.

1. Imposta il tipo di risposta su *JSON* quindi fare clic su **[!UICONTROL Salva]**.

## Configurazione del trigger campagna avanzata

1. Nella sezione Attività di marketing fare clic con il pulsante destro del mouse sul programma creato, quindi selezionare **[!UICONTROL Nuova campagna intelligente]**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegna un nome e fai clic su **[!UICONTROL Crea]**.

   ![Smart Campaign 2](assets/smartCampaign3.png)

   Dovresti visualizzare diversi trigger disponibili per l&#39;uso nella cartella Microsoft.

1. Fai clic e trascina **[!UICONTROL Aggiunto all’accordo]** al **[!UICONTROL Elenco smart]**, quindi aggiungi eventuali vincoli che desideri avere sul trigger.

   ![Aggiunto all’accordo](assets/addedToAgreementDynamics.png)

## Impostare il flusso della campagna intelligente

1. Fare clic sul pulsante **[!UICONTROL Flusso]** nella scheda [!UICONTROL Smart Campaign].

   Cercare e trascinare **Chiama webhook** passa all’area di lavoro e seleziona il webhook creato nella sezione precedente.

   ![Chiama webhook](assets/callWebhook.png)

1. La campagna di notifica SMS per i lead aggiunti a un accordo è ora configurata.
>[!TIP]
>
>Questo tutorial fa parte del corso [Accelera i cicli di vendita con Acrobat Sign per Microsoft Dynamics e Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) questo è disponibile gratuitamente su Experience League!