---
title: Inviare notifiche utilizzando Acrobat Sign per Salesforce e Marketo
description: Scopri come inviare un messaggio di testo, un’e-mail o una notifica push per informare il firmatario che è in arrivo un accordo
feature: Integrations
role: Admin
solution: Acrobat Sign, Marketo Engage, Document Cloud
level: Intermediate
jira: KT-7247
topic: Integrations
topic-revisit: Integrations
thumbnail: KT-7248.jpg
exl-id: ac3334ec-b65f-4ce4-b323-884948f5e0a6
source-git-commit: a88ec5a68aa2a02ec2f118332ec31f47d3d5d300
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Invia notifiche utilizzando Acrobat Sign per [!DNL Salesforce] e [!DNL Marketo]

Scopri come inviare un messaggio di testo, un’e-mail o una notifica push per avvisare il firmatario che è in arrivo un accordo utilizzando Acrobat Sign, Acrobat Sign per Salesforce, Marketo e Marketo Salesforce Sync. Per inviare notifiche da Marketo, è innanzitutto necessario acquistare o configurare una funzionalità di gestione SMS di Marketo. In questa procedura dettagliata vengono utilizzati [Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/), ma sono disponibili altre soluzioni Marketo SMS.

## Prerequisiti

1. Installa Marketo Salesforce Sync.

   Le informazioni e il plug-in più recente per Salesforce Sync sono disponibili [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/understanding-the-salesforce-sync.html?lang=it)

1. Installa Acrobat Sign per Salesforce.

   Le informazioni su questo plug-in sono disponibili [qui.](https://helpx.adobe.com/ca/sign/using/salesforce-integration-installation-guide.html)

## Trovare l’oggetto personalizzato

Una volta completate la sincronizzazione di Marketo Salesforce e le configurazioni di Acrobat Sign per Salesforce, nel terminale di amministrazione di Marketo vengono visualizzate diverse nuove opzioni.

![Amministratore](assets/adminTab.png)

![Sincronizzazione oggetti](assets/salesforceAdmin.png)

1. Se è la prima volta, fai clic su **Sincronizza schema**. In caso contrario, fare clic su **Aggiorna schema**.

   ![Aggiorna](assets/refreshSchema1.png)

1. Se la sincronizzazione globale è in esecuzione, disattivarla facendo clic su **Disattiva sincronizzazione globale**.

   ![Disabilita](assets/disableGlobal.png)

1. Fare clic su **Aggiorna schema**.

   ![Aggiorna 2](assets/refreshSchema2.png)

## Sincronizzare gli oggetti personalizzati

Sul lato destro, vedere Oggetti personalizzati basati su lead, contatto e account.

**Abilita la sincronizzazione** per gli oggetti in Lead se desideri attivarla quando un lead viene aggiunto a un accordo in Salesforce.

**Abilita la sincronizzazione** per gli oggetti in Contatto se desideri attivare l’attivazione quando un Contatto viene aggiunto a un accordo in Salesforce.

**Abilita la sincronizzazione** per gli oggetti in Account se desideri attivarla quando un account viene aggiunto a un accordo in Salesforce.

1. **Abilita sincronizzazione** per gli oggetti personalizzati visualizzati sotto l&#39;elemento padre desiderato (lead, contatto o account).

   ![Oggetti personalizzati](assets/customObjects.png)

1. Le risorse seguenti mostrano come **Abilitare la sincronizzazione**.

   ![Sincronizzazione personalizzata 1](assets/customObjectSync1.png)

   ![Sincronizzazione personalizzata 2](assets/customObjectSync2.png)

1. Al termine dell&#39;attivazione della sincronizzazione sugli oggetti personalizzati, riattiva la sincronizzazione.

   ![Abilita globale](assets/enableGlobal.png)

## Creazione del programma

1. Nella sezione Attività di marketing di Marketo, fare clic con il pulsante destro del mouse su **Attività di marketing** sulla barra di sinistra, selezionare **Nuova cartella campagna** e assegnargli un nome.

   ![Nuova cartella](assets/newFolder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella creata, selezionare **Nuovo programma** e assegnare un nome. Lasciare tutto il resto come predefinito, quindi fare clic su **Crea**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

## Configura Twilio SMS

Assicurati innanzitutto di disporre di un account Twilio attivo e di aver acquistato le funzioni SMS necessarie.

La configurazione del webhook Marketo - Twilio SMS richiede tre parametri Twilio dal tuo account.

- SID account
- Token account
- Numero di telefono Twilio

Recupera questi parametri dal tuo account: apri la tua istanza di Marketo.

1. Fai clic su **Amministratore** in alto a destra.

   ![Amministratore](assets/adminTab.png)

1. Fai clic su **Webhook**, quindi su **Nuovo webhook**.

   ![Webhook](assets/webhooks.png)

1. Immetti un **Nome webhook** e una **Descrizione**.

1. Immetti il seguente URL e assicurati di sostituire **[ACCOUNT_SID]** e **[AUTH_TOKEN]** con le tue credenziali Twilio.

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. Seleziona **POST** come tipo di richiesta.

1. Immetti il seguente **modello** e assicurati di sostituire **[MY_TWILIO_NUMBER]** con il tuo numero di telefono Twilio e **[YOUR_MESSAGE]** con un messaggio di tua scelta.

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. Imposta la codifica del token di richiesta su Form/URL.

1. Imposta il tipo di risposta su JSON, quindi fai clic su **Salva**.

## Configurazione del trigger campagna avanzata

1. Nella sezione Attività di marketing fare clic con il pulsante destro del mouse sul programma creato, quindi selezionare **Nuova campagna avanzata**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegnale un nome, quindi fai clic su **Crea**.

   ![Smart Campaign 2](assets/smartCampaign3.png)

   Se la configurazione della sincronizzazione degli oggetti personalizzati è stata eseguita correttamente, dovresti visualizzare i seguenti trigger disponibili per l’uso nella cartella Salesforce.

1. Fare clic e trascinare il pulsante Aggiunto all&#39;accordo nell&#39;elenco smart. Aggiungi eventuali vincoli che desideri avere sul trigger.

   ![Aggiunto all&#39;accordo 2](assets/addedToAgreement2.png)

## Impostare il flusso della campagna intelligente

1. Fare clic sulla scheda **Flusso** in Smart Campaign. Cerca e trascina il flusso **Webhook di chiamata** nell&#39;area di lavoro e seleziona il webhook creato nella sezione precedente.

   ![Chiama Webhook](assets/callWebhook.png)

1. La campagna di notifica SMS per i lead aggiunti a un accordo è ora configurata.
