---
title: Creazione di firme elettroniche incorporate ed esperienze basate su documenti
description: Scopri come utilizzare le API di Acrobat Sign per incorporare firme elettroniche ed esperienze documentali nelle piattaforme web e nei sistemi di gestione dei contenuti e dei documenti
role: User, Developer
level: Intermediate
topic: Integrations
thumbnail: KT-7489.jpg
kt: 7489
exl-id: db300cb9-6513-4a64-af60-eadedcd4858e
source-git-commit: f68fc211dc315a7d7ef508787c80fbe9882fcdc3
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 3%

---

# Creazione di firme elettroniche e documenti incorporati

Scopri come utilizzare le API di Acrobat Sign per incorporare firme elettroniche ed esperienze documentali nelle piattaforme web e nei sistemi di gestione dei contenuti e dei documenti. Ci sono quattro parti in questo tutorial pratico.

## Parte 1: Cosa ti serve

Nella parte 1, scopri come iniziare a usare tutto quello che ti serve per le parti 2-4. Iniziamo con l&#39;ottenimento delle credenziali API.

+++Visualizza i dettagli su come ottenere le credenziali API

* [Account per sviluppatori Acrobat Sign](https://acrobat.adobe.com/it/it/sign/developer-form.html)
* [Codice iniziale](https://github.com/benvanderberg/adobe-sign-api-tutorial)
* [VS Code (o editor a scelta)](https://code.visualstudio.com)
* Python 3.x
   * Mac - Homebrew
   * Linux - Programma di installazione integrato
   * Windows - Chocolatey
   * Tutto - https://www.python.org/downloads/

+++

## Parte 2: Codice basso/inesistente: la potenza dei moduli web

Nella parte 2, scopri l’opzione di utilizzo dei moduli Web in modalità low/no code. È sempre consigliabile verificare se è possibile evitare di scrivere il codice all&#39;inizio.

+++Visualizza i dettagli su come creare un modulo Web

1. Accedi ad Acrobat Sign con il tuo account sviluppatore.

1. Seleziona **Pubblicare un modulo Web** nella home page.

   ![Schermata della home page di Acrobat Sign](assets/embeddedesignature/embed_1.png)

1. Crea il tuo accordo.

   ![Screenshot di come creare un modulo Web](assets/embeddedesignature/embed_2.png)

1. Incorpora il tuo accordo in una pagina HTML semplice.

1. Provate ad aggiungere i parametri di query in modo dinamico.

   ![Screenshot dell&#39;aggiunta dei parametri di query](assets/embeddedesignature/embed_3.png)

+++

## Parte 3: Inviare un accordo con un modulo e unire i dati

Nella parte 3, crea accordi in modo dinamico.

+++Visualizza i dettagli su come creare accordi in modo dinamico

Innanzitutto, è necessario stabilire l&#39;accesso. Con Acrobat Sign, ci sono due modi per connettersi tramite API. Token OAuth e chiavi di integrazione. A meno che non abbiate un motivo molto specifico per utilizzare OAuth con l&#39;applicazione, è necessario esaminare innanzitutto le chiavi di integrazione.

1. Seleziona **Chiave di integrazione** sulla **Informazioni API** sotto il **Account** in Acrobat Sign.

   ![Screenshot di dove trovare la chiave di integrazione](assets/embeddedesignature/embed_4.png)

Ora che hai accesso e puoi interagire con l&#39;API, scopri cosa puoi fare con l&#39;API.

1. Passa al [Metodi API REST versione 6 di Acrobat Sign](http://adobesign.com/public/docs/restapi/v6).

   ![Screenshot della navigazione dei metodi delle API REST Acrobat Sign versione 6](assets/embeddedesignature/embed_5.png)

1. Utilizzare il token come valore &quot;al portatore&quot;.

   ![Screenshot del valore del portatore](assets/embeddedesignature/embed_6.png)

Per inviare il primo accordo, è meglio comprendere come utilizzare l’API.

1. Crea un documento transitorio e invialo.

>[!NOTE]
>
>Le chiamate di richiesta basate su JSON hanno un&#39;opzione &quot;Model&quot; e &quot;Minimal Model Schema&quot;. Questo dà le specifiche e un insieme di payload minimo.

![Screenshot della creazione di un documento transitorio](assets/embeddedesignature/embed_7.png)

Dopo aver inviato un accordo per la prima volta, puoi aggiungere la logica. È sempre consigliabile stabilire degli aiutanti per ridurre al minimo le ripetizioni. Ecco alcuni esempi:

**Convalida**

![Screenshot della logica di convalida](assets/embeddedesignature/embed_8.png)

**Intestazioni/Auth**

![Screenshot di intestazioni/logica automatica](assets/embeddedesignature/embed_9.png)

**URI base**

![Screenshot della logica URI di base](assets/embeddedesignature/embed_10.png)

Stai attento a dove atterrano i documenti transitori nell&#39;ambito del grande schema dell&#39;ecosistema di Sign.
Transiente -> Transiente accordo -> Modello -> Transiente accordo -> Widget -> Accordo

Questo esempio utilizza un modello come origine del documento. Questa è in genere la soluzione migliore, a meno che non si abbia un valido motivo per generare in modo dinamico documenti da firmare (ad esempio, codice precedente o generazione di documenti).

Il codice è abbastanza semplice; utilizza un documento libreria (modello) per l&#39;origine del documento. Il primo e il secondo firmatario vengono assegnati dinamicamente. Il `IN_PROCESS` stato significa che il documento viene inviato immediatamente. Inoltre, `mergeFieldInfo` viene utilizzato per riempire i campi in modo dinamico.

![Screenshot del codice per aggiungere firme in modo dinamico](assets/embeddedesignature/embed_11.png)

+++

## Parte 4: Incorpora esperienza di firma, reindirizzamenti e altro ancora

In molti casi, puoi consentire al partecipante che attiva l’accordo di firmare immediatamente un accordo. Questa funzione è utile per le applicazioni e i chioschi rivolti ai clienti.

+++Visualizza i dettagli su come incorporare l’esperienza di firma

Se non desideri che venga attivato il primo indirizzo e-mail di invio, puoi gestirlo facilmente modificando la chiamata API.

![Screenshot del codice per non attivare l&#39;invio dell&#39;e-mail](assets/embeddedesignature/embed_12.png)

Ecco come controllare il reindirizzamento post-firma:

![Screenshot del codice per controllare il reindirizzamento dopo la firma](assets/embeddedesignature/embed_13.png)

Dopo aver aggiornato il processo di creazione dell’accordo, il passaggio finale genera l’URL di firma. Anche questa chiamata è piuttosto semplice e genera un URL che un firmatario può utilizzare per accedere alla propria parte del processo di firma.

![Screenshot del codice per generare un URL del firmatario](assets/embeddedesignature/embed_14.png)

>[!NOTE]
>
>Tieni presente che la chiamata per la creazione dell’accordo è tecnicamente asincrona. Ciò significa che è possibile effettuare una chiamata di accordo &quot;POST&quot;, ma l’accordo non è ancora pronto. La procedura migliore è quella di stabilire un ciclo di tentativi. Utilizza un nuovo tentativo o qualsiasi altra procedura consigliata per il tuo ambiente.

![Screenshot con cui si consiglia di stabilire un ciclo di tentativi](assets/embeddedesignature/embed_15.png)

Quando tutto è messo insieme, la soluzione è abbastanza semplice. Stai creando un accordo e quindi generando un URL di firma affinché il firmatario possa fare clic su e iniziare il rituale di firma.

+++

## Altri argomenti

* [Eventi JS](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/events.md)
* Eventi webhook
   * [API REST](https://sign-acs.na1.echosign.com/public/docs/restapi/v6#!/webhooks/createWebhook)
   * [Webhook in Acrobat Sign v6](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/webhooks.md)
* [Riattiva e-mail di richiesta (con gli eventi)](https://sign-acs.na1.echosign.com/public/docs/restapi/v6#!/agreements/updateAgreement)
* [Sostituire Timeout con Riprova](https://stackoverflow.com/questions/23267409/how-to-implement-retry-mechanism-into-python-requests-library)

   <br> 
* Promemoria personalizzati
   * Con la creazione iniziale

      ![Screenshot della navigazione in Power Automate](assets/embeddedesignature/embed_16.png)

   * Oppure aggiungetene uno [in volo](https://sign-acs.na1.echosign.com/public/docs/restapi/v6#!/agreements/createReminderOnParticipant)
