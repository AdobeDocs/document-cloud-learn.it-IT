---
title: Inviare notifiche utilizzando Adobe Sign per Microsoft Dynamics 365 e Marketo
description: Scopri come inviare un messaggio di testo, un’e-mail o una notifica push per informare il firmatario che un accordo è in arrivo
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

# Inviare notifiche utilizzando Adobe Sign per Microsoft Dynamics 365 e Marketo

Scopri come inviare un messaggio di testo, un’e-mail o una notifica push per comunicare al firmatario che un accordo è in arrivo tramite Adobe Sign, Adobe Sign per Microsoft Dynamic, Marketo e Marketo Microsoft Dynamics Sync. Per inviare notifiche da Marketo, è necessario innanzitutto acquistare o configurare una funzione di gestione degli SMS Marketo. Questa procedura guidata utilizza [Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/), ma sono disponibili altre soluzioni Marketo SMS.

## Prerequisiti

1. Installa Marketo Microsoft Dynamics Sync.

   Sono disponibili informazioni e il plug-in più recente per Microsoft Dynamics Sync [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html)

1. Installa Adobe Sign per Microsoft Dynamics.

   Informazioni su questo plug-in sono disponibili [qui.](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)

## Trovare l’oggetto personalizzato

Al termine delle configurazioni Marketo Microsoft Dynamics Sync e Adobe Sign for Dynamics, nel Terminale di amministrazione di Marketo vengono visualizzate due nuove opzioni.

![Ammin.](assets/adminTerminal.png)

* Fai clic su **[!UICONTROL Sincronizzazione delle entità Dynamics]**.

   La sincronizzazione deve essere disattivata prima di sincronizzare le entità personalizzate. Fai clic su **[!UICONTROL Sincronizza schema]** se questa è la tua prima volta. Altrimenti, fai clic su **[!UICONTROL Aggiorna schema]**.

   ![Aggiorna](assets/refreshSchema.png)

## Sincronizzare l’oggetto personalizzato

1. A destra, individua [!UICONTROL Piombo], [!UICONTROL Contatti]e [!UICONTROL Account]oggetti personalizzati basati su -.

   * **[!UICONTROL Attiva sincronizzazione]** per gli oggetti in Lead se desideri attivarli quando un lead viene aggiunto a un accordo in Dynamics.

   * **[!UICONTROL Attiva sincronizzazione]** per gli oggetti in Referente se desideri attivarli quando un Referente viene aggiunto a un accordo in Dynamics.

   * **[!UICONTROL Attiva sincronizzazione]** per gli oggetti in Account se desideri attivarli quando un account viene aggiunto a un accordo in Dynamics.

   * **Attiva sincronizzazione** per l’oggetto Accordo nell’area superiore desiderata (Lead, Referente o Account).

   ![Oggetti personalizzati](assets/enableSyncDynamics.png)

1. Nella nuova finestra, seleziona le proprietà desiderate in Accordo.

   Attivare le caselle in **[!UICONTROL Vincolo]** e **[!UICONTROL Trigger]** per esporli alle tue attività di marketing.

   ![Sincronizzazione personalizzata 1](assets/entitySync1.png)

   ![Sincronizzazione personalizzata 2](assets/entitySync2.png)

1. Riattiva la sincronizzazione dopo aver attivato la sincronizzazione sugli oggetti personalizzati.

   Torna alla pagina [!UICONTROL Terminale di amministrazione], quindi fai clic su **[!UICONTROL Microsoft Dynamics]**, quindi fai clic su **[!UICONTROL Attiva sincronizzazione]**.

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![Abilita globale](assets/enableGlobalDynamics.png)

## Creare il programma

1. In [!UICONTROL Attività di marketing], clic con il pulsante destro del mouse **[!UICONTROL Attività di marketing]** sulla barra a sinistra, seleziona **[!UICONTROL Nuova cartella delle campagne]** e denominarlo.

   ![Nuova cartella](assets/newFolder.png)

1. Fai clic con il pulsante destro del mouse sulla cartella creata, seleziona **[!UICONTROL Nuovo programma]** e assegnargli un nome.

   Lascia tutto il resto come predefinito, quindi fai clic su **[!UICONTROL Crea]**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

## Configurazione [!DNL Twilio] SMS

Assicurati innanzitutto di avere un [!DNL Twilio] e ha acquistato le funzionalità SMS necessarie.

Configurazione del Marketo - [!DNL Twilio] Il webhook SMS richiede tre [!DNL Twilio] dell&#39;account.

* ID account
* Token account
* Numero di telefono Twilio

Recuperate questi parametri dal vostro account, ora aprite l&#39;istanza di Marketo.

1. Fai clic su **[!UICONTROL Amministratore]** in alto a destra.

   ![Ammin.](assets/adminTab.png)

1. Fai clic su **[!UICONTROL Webhook]**, quindi fai clic su **[!UICONTROL Nuovo webhook]**.

   ![Webhook](assets/webhooks.png)

1. Immettere un valore **[!UICONTROL Nome webhook]** e **[!UICONTROL Descrizione]**.

1. Immettere l&#39;URL seguente e sostituire il `ACCOUNT_SID` e `AUTH_TOKEN` con [!DNL Twilio] credenziali.

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. Seleziona **[!UICONTROL POST]** come tipo di richiesta.

1. Immetti quanto segue **Modello** e assicurarsi di sostituire `MY_TWILIO_NUMBER` con [!DNL Twilio] numero di telefono e `YOUR_MESSAGE` con un messaggio di tua scelta.

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. Impostare la proprietà **[!UICONTROL Codifica token di richiesta]** a *Modulo/URL*.

1. Imposta il tipo di risposta su *JSON* quindi fai clic su **[!UICONTROL Salva]**.

## Impostare l’attivatore Campagna avanzata

1. Nella sezione Attività di marketing, fai clic con il pulsante destro del mouse sul programma che hai creato, quindi seleziona **[!UICONTROL Nuova campagna intelligente]**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegnare un nome, quindi fare clic **[!UICONTROL Crea]**.

   ![Smart Campaign 2](assets/smartCampaign3.png)

   Nella cartella Microsoft devono essere visualizzati diversi attivatori disponibili per l’uso.

1. Fare clic e trascinare **[!UICONTROL Aggiunta all’accordo]** alla proprietà **[!UICONTROL Elenco avanzato]** quindi aggiungi eventuali vincoli che desideri avere sul trigger.

   ![Aggiunta all’accordo](assets/addedToAgreementDynamics.png)

## Impostare il flusso delle campagne avanzate

1. Fate clic sul **[!UICONTROL Flusso]** nella [!UICONTROL Campagna intelligente].

   Cercare e trascinare il **Chiama webhook** scorri sull’area di lavoro e seleziona il webhook creato nella sezione precedente.

   ![Chiama webhook](assets/callWebhook.png)

1. È ora configurata la campagna di notifica tramite SMS per i contatti qualificati che sono stati aggiunti a un accordo.
>[!TIP]
>
>Questo tutorial fa parte del corso [Accelera i cicli di vendita con Adobe Sign per Microsoft Dynamics e Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) disponibile gratuitamente ad Experience League!