---
title: Creazione di esperienze di firma elettronica e documenti incorporati
description: Informazioni sull'utilizzo delle API di Adobe Sign per incorporare la firma elettronica e documentare le esperienze nelle piattaforme Web e nei sistemi di gestione dei contenuti e dei documenti
role: User, Developer
level: Intermediate
topic: Integrations
thumbnail: KT-7489.jpg
kt: 7489
exl-id: db300cb9-6513-4a64-af60-eadedcd4858e
source-git-commit: f015bd7ea1a25772a12cd0852d452d120f205a5c
workflow-type: tm+mt
source-wordcount: '928'
ht-degree: 3%

---

# Crea una firma elettronica e esperienze documento incorporate

Informazioni sull&#39;utilizzo delle API di Adobe Sign per incorporare la firma elettronica e documentare le esperienze nelle piattaforme Web e nei sistemi di gestione dei contenuti e dei documenti. L&#39;esercitazione pratica è suddivisa in quattro parti, illustrate nei collegamenti seguenti:

<table style="table-layout:fixed">
<tr>
  <td>
    <a href="embeddedesignature.md#part1">
        <img alt="Di cosa avrai bisogno?" src="assets/embeddedesignature/EmbedPart1_thumb.png" />
    </a>
    <div>
    <a href="embeddedesignature.md#part1"><strong>Parte 1: Di cosa avrai bisogno?</strong></a>
    </div>
  </td>
  <td>
    <a href="embeddedesignature.md#part2">
        <img alt="Parte 2: Codice Basso/No — la potenza dei moduli web" src="assets/embeddedesignature/EmbedPart2_thumb.png" />
    </a>
    <div>
    <a href="embeddedesignature.md#part2"><strong>Parte 2: Codice Basso/No — la potenza dei moduli web</strong></a>
    </div>
  </td>
  <td>
   <a href="embeddedesignature.md#part3">
      <img alt="Parte 3: Invia contratto con un modulo e unisci dati" src="assets/embeddedesignature/EmbedPart3_thumb.png" />
   </a>
    <div>
    <a href="embeddedesignature.md#part3"><strong>Parte 3: Invia contratto con un modulo e unisci dati</strong></a>
    </div>
  </td>
  <td>
   <a href="embeddedesignature.md#part4">
      <img alt="Parte 4: Incorpora esperienza di firma, reindirizzamenti e altro ancora" src="assets/embeddedesignature/EmbedPart4_thumb.png" />
   </a>
    <div>
    <a href="embeddedesignature.md#part4"><strong>Parte 4: Incorpora esperienza di firma, reindirizzamenti e altro ancora</strong></a>
    </div>
  </td>
</tr>
</table>

## Parte 1: Di cosa avrai bisogno? {#part1}

Nella prima parte imparerai come iniziare con tutto quello che ti serve per le parti 2-4. Cominciamo con il recupero delle credenziali API.

* [Account per sviluppatori Adobe Sign](https://acrobat.adobe.com/it/it/sign/developer-form.html)
* [Codice Starter](https://github.com/benvanderberg/adobe-sign-api-tutorial)
* [Codice VS (o editor di tua scelta)](https://code.visualstudio.com)
* Python 3.x
   * Mac — Ebraico
   * Linux — Installatore integrato
   * Finestre — Cioccolatino
   * Tutti — https://www.python.org/downloads/

## Parte 2: Codice Basso/No — la potenza dei moduli web {#part2}

Nella parte 2, si esplorerà l&#39;opzione low/no code quando si utilizzano i webform. È sempre una buona idea vedere se all&#39;inizio puoi evitare di scrivere codice.

1. Accedi a Adobe Sign con il tuo account sviluppatore.
1. Fare clic su **Pubblica un modulo Web** nella home page.

   ![Schermata della home page di Adobe Sign](assets/embeddedesignature/embed_1.png)

1. Crea il contratto.

   ![Screenshot di come creare un modulo Web](assets/embeddedesignature/embed_2.png)

1. Incorpora il contratto in una pagina HTML flat.
1. Esperimento con aggiunta dinamica di parametri di query.

   ![Screenshot dell&#39;aggiunta di parametri di query](assets/embeddedesignature/embed_3.png)

## Parte 3: Invia contratto con un modulo e unisci dati {#part3}

Nella terza parte, creerete in modo dinamico accordi.

Per prima cosa, dovrete stabilire l&#39;accesso. Con Adobe Sign, esistono due modi per connettersi tramite API. Token OAuth e chiavi di integrazione. A meno che non si disponga di un motivo specifico per utilizzare OAuth con l&#39;applicazione, è necessario esplorare prima le chiavi di integrazione.

1. Selezionare **Tasto di integrazione** nel menu **Informazioni API** nella scheda **Account** di Adobe Sign.

   ![Screenshot di dove trovare la chiave di integrazione](assets/embeddedesignature/embed_4.png)

Ora che hai accesso e puoi interagire con l&#39;API, vedi cosa puoi fare con l&#39;API.

1. Passare all&#39;API [Adobe Sign REST API versione 6 Methods](http://adobesign.com/public/docs/restapi/v6).

   ![Screenshot dei metodi API REST di Adobe Sign Versione 6](assets/embeddedesignature/embed_5.png)

1. Utilizzare il token come valore &quot;Bearer&quot;.

   ![Screenshot del valore del portatore](assets/embeddedesignature/embed_6.png)

Per inviare il primo contratto è meglio capire come utilizzare l&#39;API.

1. Creare un documento temporaneo e inviarlo.

>[!NOTE]
>
>Le chiamate di richiesta basate su JSON dispongono di un&#39;opzione &quot;Model&quot; e &quot;Minimal Model Schema&quot;. Questo fornisce le specifiche e un set di payload minimo.

![Screenshot della creazione di un documento provvisorio](assets/embeddedesignature/embed_7.png)

Dopo aver inviato un accordo per la prima volta, sei pronto ad aggiungere la logica. È sempre una buona idea stabilire degli aiutanti per minimizzare le ripetizioni. Ecco alcuni esempi:

**Convalida**

![Screenshot della logica di convalida](assets/embeddedesignature/embed_8.png)

**Intestazioni/Auth**

![Screenshot delle intestazioni/logica di autenticazione](assets/embeddedesignature/embed_9.png)

**URI di base**

![Screenshot della logica dell&#39;URI di base](assets/embeddedesignature/embed_10.png)

Attenzione a dove atterrano i documenti di Transient nel grande schema dell&#39;ecosistema dei segni.
Transizione -> Contratto
Transizione -> Modello -> Contratto
Transizione -> Widget -> Contratto

In questo esempio viene utilizzato un modello come origine del documento. Questa è in genere la route migliore, a meno che non si disponga di un valido motivo per generare in modo dinamico documenti per la firma (ad esempio, generazione di codice legacy o di documenti).

Il codice è abbastanza semplice; utilizza un documento di libreria (modello) per l&#39;origine del documento. Il primo e il secondo firmatario vengono assegnati in modo dinamico. Lo stato `IN_PROCESS` indica che il documento viene inviato immediatamente. Inoltre, `mergeFieldInfo` viene utilizzato per riempire in modo dinamico i campi.

![Screenshot di codice per aggiungere dinamicamente le firme](assets/embeddedesignature/embed_11.png)

## Parte 4: Incorpora esperienza di firma, reindirizzamenti e altro ancora {#part4}

In molti casi, è possibile consentire al partecipante che esegue il trigger di firmare immediatamente un accordo. Questa funzione è utile per applicazioni e chioschi rivolti ai clienti.

Se non si desidera che il primo messaggio di posta elettronica di invio venga attivato, è facile gestire il comportamento con una modifica alla chiamata API.

![Screenshot di codice per non attivare l&#39;invio di messaggi di posta elettronica](assets/embeddedesignature/embed_12.png)

Ecco come controllare il reindirizzamento post-firma:

![Screenshot di codice per controllare il reindirizzamento post-firma](assets/embeddedesignature/embed_13.png)

Dopo aver aggiornato il processo di creazione del contratto, il passaggio finale genera l&#39;URL di firma. Questa chiamata è piuttosto semplice e genera un URL che un firmatario può utilizzare per accedere alla propria parte del processo di firma.

![Screenshot di codice per generare un URL del firmatario](assets/embeddedesignature/embed_14.png)

>[!NOTE]
>
>Si noti che la chiamata di creazione del contratto è tecnicamente asincrona. Ciò significa che si può fare una chiamata per un accordo &quot;POST&quot;, ma l&#39;accordo non è ancora pronto. La procedura migliore è stabilire un ciclo di tentativi. Utilizzare un nuovo tentativo o qualsiasi altra procedura appropriata per l&#39;ambiente.

![Screenshot con cui si dice che è meglio stabilire un ciclo di tentativi](assets/embeddedesignature/embed_15.png)

Quando tutto viene messo insieme, la soluzione è piuttosto semplice. Stai stipulando un accordo e quindi stai generando un URL di firma per il firmatario che clicca su e inizia il rituale di firma.

### Ulteriori argomenti

* [Eventi JS](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/events.md)
* Eventi webhook
   * [API REST](https://sign-acs.na1.echosign.com/public/docs/restapi/v6#!/webhooks/createWebhook)
   * [Webhook in Adobe Sign 6](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/webhooks.md)
* [Riattiva messaggi di posta elettronica richiesta (con eventi)](https://sign-acs.na1.echosign.com/public/docs/restapi/v6#!/accordi/aggiornamentoContratto)
* [Sostituisci timeout con un tentativo](https://stackoverflow.com/questions/23267409/how-to-implement-retry-mechanism-into-python-requests-library)

   <br> 
* Promemoria personalizzata
   * Con la creazione iniziale

      ![Screenshot della guida di Power Automate](assets/embeddedesignature/embed_16.png)

   * Oppure aggiungetene uno [in volo](https://sign-acs.na1.echosign.com/public/docs/restapi/v6#!/agreements/createPromemoriaOnParticipant)

## Risorse aggiuntive

http://bit.ly/Summit21-T126

Comprende:
* Account per sviluppatori Adobe Sign
* Docs API di Adobe Sign
* Codice esempio
* Codice di Visual Studio
* Python
