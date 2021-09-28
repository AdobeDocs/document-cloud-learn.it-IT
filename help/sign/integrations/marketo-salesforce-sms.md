---
title: Invia notifiche utilizzando Adobe Sign per Salesforce e Marketo
description: Informazioni su come inviare un messaggio di testo, un'e-mail o una notifica push per far sapere al firmatario che è in corso un accordo
role: Admin
product: adobe sign
solution: Adobe Sign, Marketo, Document Cloud
level: Intermediate
topic-revisit: Integrations
thumbnail: KT-7248.jpg
exl-id: ac3334ec-b65f-4ce4-b323-884948f5e0a6
source-git-commit: bc79bbde966c99bdf32231ff073b49cfc759d928
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 1%

---

# Invia notifiche utilizzando Adobe Sign per Salesforce e Marketo

Informazioni su come inviare un messaggio di testo, un&#39;e-mail o una notifica push per far sapere al firmatario che un accordo è in arrivo utilizzando Adobe Sign, Adobe Sign per Salesforce, Marketo e Marketo Salesforce Sync. Per inviare le notifiche da Marketo, è innanzitutto necessario acquistare o configurare una funzionalità di gestione di Marketo SMS. Questa procedura dettagliata utilizza [Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/), ma sono disponibili altre soluzioni Marketo SMS.

## Prerequisiti

1. Installare Marketo Salesforce Sync.

   Le informazioni e il plug-in più recente per Salesforce Sync sono disponibili [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/understanding-the-salesforce-sync.html)

1. Installare Adobe Sign per Salesforce.

   Le informazioni sul plug-in sono disponibili [qui.](https://helpx.adobe.com/ca/sign/using/salesforce-integration-installation-guide.html)

## Trova l&#39;oggetto personalizzato

Una volta completate le configurazioni Marketo Salesforce Sync e Adobe Sign for Salesforce, vengono visualizzate diverse nuove opzioni nel Terminale Amministrazione Marketo.

![Ammin.](assets/adminTab.png)

![Sincronizzazione oggetti](assets/salesforceAdmin.png)

1. Fare clic su **Sincronizza schema** se questa è la prima volta. In caso contrario, fare clic su **Aggiorna schema**.

   ![Aggiorna](assets/refreshSchema1.png)

1. Se la sincronizzazione globale è in esecuzione, disattivare facendo clic su **Disattiva sincronizzazione globale**.

   ![Disabilita](assets/disableGlobal.png)

1. Fare clic su **Aggiorna schema**.

   ![Aggiorna 2](assets/refreshSchema2.png)

## Sincronizzare gli oggetti personalizzati

Sul lato destro, vedere Oggetti personalizzati basati su Lead, Contact e Account.

**Abilitare** Syncfor per gli oggetti in Lead se si desidera attivare quando un lead viene aggiunto a un accordo in Salesforce.

**Abilitare** Syncfor per gli oggetti sotto Contatto se si desidera attivare quando un contatto viene aggiunto a un accordo in Salesforce.

**Abilitare** Syncfor per gli oggetti in Account se si desidera attivare quando un Account viene aggiunto a un accordo in Salesforce.

1. **Attiva** Syncfor per gli oggetti personalizzati visualizzati sotto l&#39;oggetto principale desiderato (Lead, contatto o Account).

   ![Oggetti personalizzati](assets/customObjects.png)

1. Nelle seguenti risorse viene illustrato come attivare **Sincronizzazione**.

   ![Sincronizzazione personalizzata 1](assets/customObjectSync1.png)

   ![Sincronizzazione personalizzata 2](assets/customObjectSync2.png)

1. Al termine dell&#39;abilitazione della sincronizzazione sugli oggetti personalizzati, riattivare la sincronizzazione.

   ![Abilita globale](assets/enableGlobal.png)

## Crea il programma

1. Nella sezione Attività di marketing di Marketo, fare clic con il pulsante destro del mouse su **Attività di marketing** sulla barra a sinistra, selezionare **Nuova cartella campagna** e dargli un nome.

   ![Nuova cartella](assets/newFolder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella creata, selezionare **Nuovo programma** e assegnargli un nome. Lasciare tutto il resto come predefinito, quindi fare clic su **Crea**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

## Imposta Twilio SMS

Prima assicurati di avere un account Twilio attivo e di aver acquistato le funzionalità SMS richieste.

L&#39;impostazione del webhook SMS Marketo - Twilio richiede tre parametri Twilio dal tuo account.

- SID account
- Token account
- Numero di telefono Twilio

Recuperare questi parametri dall&#39;account, quindi aprire l&#39;istanza Marketo.

1. Fare clic su **Admin** in alto a destra.

   ![Ammin.](assets/adminTab.png)

1. Fare clic su **Webhooks**, quindi **Nuovo Webhook**.

   ![Webhook](assets/webhooks.png)

1. Immettere un **Nome webhook** e **Descrizione**.

1. Immettere l&#39;URL seguente e assicurarsi di sostituire **[ACCOUNT_SID]** e **[AUTH_TOKEN]** con le credenziali Twilio.

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. Selezionare **POST** come tipo di richiesta.

1. Immettere il seguente **Modello** e assicurarsi di sostituire **[MY_TWILIO_NUMBER]** con il numero di telefono Twilio e **[TUO_MESSAGE]** con un messaggio scelto.

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. Impostare Codifica token richiesta su Maschera/URL.

1. Impostare il tipo di risposta su JSON, quindi fare clic su **Salva**.

## Imposta il trigger Smart Campaign

1. Nella sezione Attività di marketing fare clic con il pulsante destro del mouse sul programma creato, quindi selezionare **Nuova campagna**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegnare un nome, quindi fare clic su **Crea**.

   ![Smart Campaign 2](assets/smartCampaign3.png)

   Se la configurazione per la sincronizzazione personalizzata degli oggetti è stata eseguita correttamente, è necessario visualizzare i seguenti trigger disponibili per l&#39;utilizzo nella cartella Salesforce.

1. Fare clic su e trascinare Added to Agreement to Smart List. Aggiungere i vincoli che si desidera avere sul trigger.

   ![Aggiunta all&#39;accordo 2](assets/addedToAgreement2.png)

## Imposta il flusso di Smart Campaign

1. Fare clic sulla scheda **Flusso** nella campagna Smart. Cercare e trascinare il flusso **Call Webhook** nell&#39;area di lavoro e selezionare il webhook creato nella sezione precedente.

   ![Chiama Webhook](assets/callWebhook.png)

1. La campagna di notifica via SMS per i lead che vengono aggiunti a un accordo è ora impostata.

>[!TIP]
>
>Questa esercitazione fa parte del corso [Accelerare i cicli di vendita con Adobe Sign for Salesforce e Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1), disponibile gratuitamente con Experience League!