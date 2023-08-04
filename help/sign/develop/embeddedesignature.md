---
title: Creare esperienze di firma elettronica e documenti incorporate
description: Scopri come utilizzare le API di Acrobat Sign per incorporare esperienze di firma elettronica e di gestione dei documenti in piattaforme Web e sistemi di gestione di contenuti e documenti
feature: Integrations, Workflow
role: Developer
level: Intermediate
topic: Integrations
jira: KT-7489
thumbnail: KT-7489.jpg
kt: 7489
exl-id: db300cb9-6513-4a64-af60-eadedcd4858e
source-git-commit: 452299b2b786beab9df7a5019da4f3840d9cdec9
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 2%

---

# Creare esperienze incorporate di firma elettronica e di creazione di documenti

Scopri come utilizzare le API di Acrobat Sign per incorporare esperienze di firma elettronica e di gestione dei documenti nelle piattaforme Web e nei sistemi di gestione dei contenuti e dei documenti. Questo tutorial pratico si articola in quattro parti.

## Parte 1: Cosa serve

Nella parte 1, scopri come iniziare con tutto ciò di cui hai bisogno per le parti 2-4. Iniziamo con il recupero delle credenziali API.

+++Visualizza dettagli su come ottenere le credenziali API

* [Account per sviluppatori Acrobat Sign](https://acrobat.adobe.com/it/it/sign/developer-form.html)
* [Codice Starter](https://github.com/benvanderberg/adobe-sign-api-tutorial)
* [Codice VS (o editor di tua scelta)](https://code.visualstudio.com)
* Python 3.x
   * Mac - Homebrew
   * Linux - Programma di installazione integrato
   * Windows - Chocolatey
   * Tutti - https://www.python.org/downloads/

+++

## Parte 2: Codice basso/nessun — la potenza dei moduli web

Nella parte 2, esplora l’opzione &quot;low/no code&quot; per l’utilizzo dei moduli web. È sempre una buona idea vedere se è possibile evitare di scrivere codice all&#39;inizio.

+++Visualizzare i dettagli sulla creazione di un modulo Web

1. Accedi ad Acrobat Sign con il tuo account sviluppatore.

1. Seleziona **Pubblicare un modulo Web** nella home page.

   ![Schermata iniziale di Acrobat Sign](assets/embeddedesignature/embed_1.png)

1. Crea l’accordo.

   ![Schermata per la creazione di un modulo Web](assets/embeddedesignature/embed_2.png)

1. Incorpora l’accordo in una pagina HTML piatta.

1. Sperimentare l&#39;aggiunta dinamica dei parametri di query.

   ![Schermata per l’aggiunta di parametri di query](assets/embeddedesignature/embed_3.png)

+++

## Parte 3: Inviare un accordo con un modulo e unire i dati

Nella parte 3, crea dinamicamente gli accordi.

+++Visualizza dettagli su come creare dinamicamente gli accordi

In primo luogo, è necessario stabilire l&#39;accesso. Con Acrobat Sign, sono disponibili due modi per connettersi tramite API. Token OAuth e chiavi di integrazione. A meno che tu non abbia un motivo molto specifico per utilizzare OAuth con la tua applicazione, dovresti prima esplorare le Chiavi di integrazione.

1. Seleziona **Chiave di integrazione** sul **Informazioni API** menu sotto **Account** in Acrobat Sign.

   ![Schermata indicante dove trovare la chiave di integrazione](assets/embeddedesignature/embed_4.png)

Ora che hai accesso e puoi interagire con l’API, scopri cosa puoi fare con l’API.

1. Passare alla [Metodi API REST versione 6 per Acrobat Sign](http://adobesign.com/public/docs/restapi/v6).

   ![Schermata di navigazione Metodi API REST versione 6 di Acrobat Sign](assets/embeddedesignature/embed_5.png)

1. Utilizza il token come valore &quot;al portatore&quot;.

   ![Schermata del valore al portatore](assets/embeddedesignature/embed_6.png)

Per inviare il primo accordo, è meglio comprendere come utilizzare l’API.

1. Crea un documento transitorio e invialo.

>[!NOTE]
>
>Le chiamate di richiesta basate su JSON dispongono di un&#39;opzione &quot;Modello&quot; e di uno &quot;Schema modello minimo&quot;. Questo fornisce le specifiche e un set di payload minimo.

![Schermata della creazione di un documento transitorio](assets/embeddedesignature/embed_7.png)

Dopo aver inviato un accordo per la prima volta, puoi aggiungere la logica. È sempre una buona idea stabilire degli assistenti per ridurre al minimo le ripetizioni. Di seguito sono riportati alcuni esempi:

**Convalida**

![Schermata della logica di convalida](assets/embeddedesignature/embed_8.png)

**Intestazioni/Autenticazione**

![Schermata delle intestazioni/logica di autenticazione](assets/embeddedesignature/embed_9.png)

**URI di base**

![Schermata della logica dell&#39;URI di base](assets/embeddedesignature/embed_10.png)

Tieni presente dove atterrano i documenti transitori all’interno del sistema complessivo dell’ecosistema Sign.
Transitorio -> Accordo Transitorio -> Modello -> Accordo Transitorio -> Widget -> Accordo

In questo esempio viene utilizzato un modello come origine del documento. Questa è in genere la procedura migliore, a meno che non si abbia una ragione valida per generare dinamicamente i documenti per la firma (ad esempio, per generare codice legacy o documenti).

Il codice è abbastanza semplice; utilizza un documento libreria (modello) per l&#39;origine del documento. Il primo e il secondo firmatario vengono assegnati in modo dinamico. La `IN_PROCESS` state indica che il documento viene inviato immediatamente. Inoltre, `mergeFieldInfo` viene utilizzato per compilare dinamicamente i campi.

![Schermata del codice per aggiungere le firme in modo dinamico](assets/embeddedesignature/embed_11.png)

+++

## Parte 4: incorpora esperienza di firma, reindirizzamenti e altro ancora

In molti casi può essere utile consentire al partecipante che esegue l’attivazione di firmare immediatamente un accordo. Questa funzione è utile per le applicazioni e i chioschi rivolti ai clienti.

+++Visualizza i dettagli su come incorporare l’esperienza di firma

Se non desideri che venga attivato il primo invio di e-mail, un modo semplice consiste nel gestire il comportamento modificando la chiamata API.

![Schermata del codice per non attivare l’invio di e-mail](assets/embeddedesignature/embed_12.png)

Ecco come controllare il reindirizzamento post-firma:

![Schermata del codice per controllare il reindirizzamento post-firma](assets/embeddedesignature/embed_13.png)

Dopo aver aggiornato il processo di creazione dell’accordo, il passaggio finale genera l’URL di firma. Anche questa chiamata è piuttosto semplice e genera un URL che un firmatario può utilizzare per accedere alla propria parte del processo di firma.

![Schermata di codice per generare un URL firmatario](assets/embeddedesignature/embed_14.png)

>[!NOTE]
>
>Tieni presente che la chiamata per la creazione dell’accordo è tecnicamente asincrona. Ciò significa che è possibile effettuare una chiamata all’accordo &quot;POST&quot;, ma l’accordo non è ancora pronto. È consigliabile stabilire un ciclo di tentativi. Utilizza un nuovo tentativo o qualsiasi altra procedura consigliata per il tuo ambiente.

![Screenshot che indica che è consigliabile stabilire un ciclo di tentativi](assets/embeddedesignature/embed_15.png)

Quando si assembla tutto, la soluzione è piuttosto semplice. Stai creando un accordo e quindi generando un URL di firma in cui il firmatario può fare clic per iniziare il rituale di firma.

+++

## Argomenti aggiuntivi

* [Eventi JS](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/events.md)
* Eventi webhook
   * [API REST](https://sign-acs.na1.echosign.com/public/docs/restapi/v6#!/webhooks/createWebhook)
   * [Webhook in Acrobat Sign v6](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/webhooks.md)
* [Riattiva e-mail di richiesta (con eventi)](https://sign-acs.na1.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreement)
* [Sostituisci timeout con un nuovo tentativo](https://stackoverflow.com/questions/23267409/how-to-implement-retry-mechanism-into-python-requests-library)
* Promemoria personalizzati
   * Con la creazione iniziale

     ![Schermata di navigazione a Power Automate](assets/embeddedesignature/embed_16.png)

   * Oppure aggiungetene uno [in volo](https://sign-acs.na1.echosign.com/public/docs/restapi/v6#!/agreements/createReminderOnParticipant)
