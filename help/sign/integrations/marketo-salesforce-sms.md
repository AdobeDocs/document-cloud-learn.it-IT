---
title: Inviare notifiche utilizzando Acrobat Sign per Salesforce e Marketo
description: Scopri come inviare un messaggio di testo, un’e-mail o una notifica push per informare il firmatario che un accordo è in arrivo
role: Admin
product: adobe sign
solution: Acrobat Sign, Marketo, Document Cloud
level: Intermediate
topic-revisit: Integrations
thumbnail: KT-7248.jpg
exl-id: ac3334ec-b65f-4ce4-b323-884948f5e0a6
source-git-commit: e02b1250de94ec781e7984c6c146dbae993f5d31
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 1%

---

# Inviare notifiche utilizzando Acrobat Sign per Salesforce e Marketo

Scopri come inviare un messaggio di testo, un’e-mail o una notifica push per comunicare al firmatario che un accordo è in arrivo utilizzando Acrobat Sign, Acrobat Sign per Salesforce, Marketo e Marketo Salesforce Sync. Per inviare notifiche da Marketo, è necessario innanzitutto acquistare o configurare una funzione di gestione degli SMS Marketo. Questa procedura guidata utilizza [Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/), ma sono disponibili altre soluzioni Marketo SMS.

## Prerequisiti

1. Installa Marketo Salesforce Sync.

   Sono disponibili informazioni e l’ultimo plug-in per Salesforce Sync [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/understanding-the-salesforce-sync.html)

1. Installa Acrobat Sign per Salesforce.

   Informazioni su questo plug-in sono disponibili [qui.](https://helpx.adobe.com/ca/sign/using/salesforce-integration-installation-guide.html)

## Trovare l’oggetto personalizzato

Al termine delle configurazioni Marketo Salesforce Sync e Acrobat Sign per Salesforce, nel Terminale di amministrazione di Marketo vengono visualizzate diverse nuove opzioni.

![Ammin.](assets/adminTab.png)

![Sincronizzazione oggetto](assets/salesforceAdmin.png)

1. Fai clic su **Sincronizza schema** se questa è la tua prima volta. Altrimenti, fai clic su **Aggiorna schema**.

   ![Aggiorna](assets/refreshSchema1.png)

1. Se è in esecuzione la sincronizzazione globale, disattiva facendo clic su **Disattiva sincronizzazione globale**.

   ![Disabilita](assets/disableGlobal.png)

1. Fai clic su **Aggiorna schema**.

   ![Refresh 2](assets/refreshSchema2.png)

## Sincronizzare gli oggetti personalizzati

Sul lato destro, consultate Oggetti personalizzati Lead, Contact e Account.

**Attiva sincronizzazione** per gli oggetti in Lead se desideri attivarli quando un lead viene aggiunto a un accordo in Salesforce.

**Attiva sincronizzazione** per gli oggetti in Referente se desideri attivarli quando un Referente viene aggiunto a un accordo in Salesforce.

**Attiva sincronizzazione** per gli oggetti in Account se desideri attivarli quando un account viene aggiunto a un accordo in Salesforce.

1. **Attiva sincronizzazione** per gli oggetti personalizzati visualizzati sotto l’elemento principale desiderato (Lead, Referente o Account).

   ![Oggetti personalizzati](assets/customObjects.png)

1. Le seguenti risorse mostrano come **Attiva sincronizzazione**.

   ![Sincronizzazione personalizzata 1](assets/customObjectSync1.png)

   ![Sincronizzazione personalizzata 2](assets/customObjectSync2.png)

1. Al termine dell’attivazione della sincronizzazione sugli oggetti personalizzati, riattiva la sincronizzazione.

   ![Abilita globale](assets/enableGlobal.png)

## Creare il programma

1. Nella sezione Attività di marketing di Marketo, fai clic con il pulsante destro del mouse su **Attività di marketing** sulla barra a sinistra, seleziona **Nuova cartella delle campagne** e assegnargli un nome.

   ![Nuova cartella](assets/newFolder.png)

1. Fai clic con il pulsante destro del mouse sulla cartella creata, seleziona **Nuovo programma** e assegnargli un nome. Lascia tutto il resto come predefinito, quindi fai clic su **Crea**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

## Impostare Twilio SMS

Assicurati innanzitutto di avere un account Twilio attivo e di acquistare le funzionalità SMS necessarie.

La configurazione del webhook Marketo - Twilio SMS richiede tre parametri Twilio dal tuo account.

- ID account
- Token account
- Numero di telefono Twilio

Recuperate questi parametri dal vostro account, ora aprite l&#39;istanza di Marketo.

1. Fai clic su **Amministratore** in alto a destra.

   ![Ammin.](assets/adminTab.png)

1. Fai clic su **Webhook**, quindi **Nuovo webhook**.

   ![Webhook](assets/webhooks.png)

1. Immettere un valore **Nome webhook** e **Descrizione**.

1. Immettere l&#39;URL seguente e sostituire il **[ACCOUNT_SID]** e **[AUTH_TOKEN]** con le tue credenziali Twilio.

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. Seleziona **POST** come tipo di richiesta.

1. Immetti quanto segue **Modello** e assicurarsi di sostituire **[MY_TWILIO_NUMBER]** con il numero di telefono Twilio e **[YOUR_MESSAGE]** con un messaggio di tua scelta.

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. Impostare la codifica del token di richiesta su Form/URL.

1. Imposta il tipo di risposta su JSON, quindi fai clic su **Salva**.

## Impostare l’attivatore Campagna avanzata

1. Nella sezione Attività di marketing, fai clic con il pulsante destro del mouse sul programma che hai creato, quindi seleziona **Nuova campagna intelligente**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegnare un nome, quindi fare clic **Crea**.

   ![Smart Campaign 2](assets/smartCampaign3.png)

   Se la configurazione per la sincronizzazione personalizzata degli oggetti è stata eseguita correttamente, i seguenti attivatori sono disponibili per l’uso nella cartella Salesforce.

1. Fai clic e trascina Aggiungi all’accordo nell’elenco avanzato. Aggiungi eventuali vincoli che desideri applicare al trigger.

   ![Aggiunta all&#39;accordo 2](assets/addedToAgreement2.png)

## Impostare il flusso delle campagne avanzate

1. Fai clic sul **Flusso** nella Campagna avanzata. Cercare e trascinare il **Chiama webhook** scorri sull’area di lavoro e seleziona il webhook creato nella sezione precedente.

   ![Chiama webhook](assets/callWebhook.png)

1. È ora configurata la campagna di notifica tramite SMS per i contatti qualificati che sono stati aggiunti a un accordo.

>[!TIP]
>
>Questo tutorial fa parte del corso [Accelera i cicli di vendita con Acrobat Sign per Salesforce e Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) disponibile gratuitamente ad Experience League!