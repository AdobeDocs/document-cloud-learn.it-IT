---
title: Automazione dei documenti con Acrobat Sign per Microsoft Power Platform
description: Scopri come attivare e utilizzare i connettori Acrobat Sign e Adobe PDF Tools per Microsoft Power Apps. Creazione di flussi di lavoro che automatizzano i processi di approvazione e firma aziendali in modo rapido e sicuro, senza dover ricorrere a codice
feature: Integrations, Workflow
role: User, Developer
level: Intermediate
topic: Integrations
thumbnail: KT-7488.jpg
jira: KT-7488
exl-id: 4113bc3f-293c-44a8-94ab-e1dbac74caed
source-git-commit: 452299b2b786beab9df7a5019da4f3840d9cdec9
workflow-type: tm+mt
source-wordcount: '2436'
ht-degree: 1%

---

# Automazione dei documenti con Acrobat Sign per Microsoft Power Platform

Scopri come attivare e utilizzare i connettori Acrobat Sign e Adobe PDF Tools per Microsoft Power Apps. Creazione di flussi di lavoro che automatizzano i processi di approvazione e firma aziendali in modo rapido e sicuro, senza dover ricorrere a codice. Questo tutorial pratico è suddiviso in quattro parti, descritte nei collegamenti seguenti:

<table style="table-layout:fixed">
<tr>
  <td>
    <a href="documentautomation.md#part1">
        <img alt="Parte 1: Archiviare gli accordi firmati in SharePoint con Acrobat Sign" src="assets/documentautomation/AutomationPart1_thumb.jpg" />
    </a>
    <div>
    <a href="documentautomation.md#part1"><strong>Parte 1: Archiviare gli accordi firmati in SharePoint con Acrobat Sign</strong></a>
    </div>
  </td>
  <td>
    <a href="documentautomation.md#part2">
        <img alt="Parte 2: Procedura di approvazione automatizzata per ottenere la firma elettronica con Acrobat Sign" src="assets/documentautomation/AutomationPart2_thumb.jpg" />
    </a>
    <div>
    <a href="documentautomation.md#part2"><strong>Parte 2: Procedura di approvazione automatizzata per ottenere la firma elettronica con Acrobat Sign</strong></a>
    </div>
  </td>
  <td>
   <a href="documentautomation.md#part3">
      <img alt="Parte 3: OCR automatizzato dei documenti con Adobe PDF Tools" src="assets/documentautomation/AutomationPart3_thumb.jpg" />
   </a>
    <div>
    <a href="documentautomation.md#part3"><strong>Parte 3: OCR automatizzato dei documenti con Adobe PDF Tools</strong></a>
    </div>
  </td>
  <td>
   <a href="documentautomation.md#part4">
      <img alt="Parte 4: Assemblaggio automatico di documenti con Adobe PDF Tools" src="assets/documentautomation/AutomationPart4_thumb.jpg" />
   </a>
    <div>
    <a href="documentautomation.md#part4"><strong>Parte 4: Assemblaggio automatico di documenti con Adobe PDF Tools</strong></a>
    </div>
  </td>
</tr>
</table>

## Prerequisiti

* Familiarità con Microsoft 365 e Power Automate
* Conoscenza di Acrobat Sign
* Account Microsoft 365 con accesso a SharePoint e Power Automate (Basic per Acrobat Sign, Premium per Adobe PDF Tools)
* Account per sviluppatori Acrobat Sign for enterprise o Acrobat Sign

**Esercizi 1 e 2**

* Account Acrobat Sign con accesso all’API. Un account sviluppatore o un account Enterprise.
* Sito SharePoint accessibile da Power Automate per il quale disponi delle autorizzazioni di modifica. È consigliato l&#39;accesso completo da parte dell&#39;amministratore.
* Esempio di documento per la richiesta di approvazione e firma della firma.

**Esercizi 3 e 4**

Scarica materiali [qui](https://github.com/benvanderberg/adobe-sign-pdftools-powerautomate-tutorial)

## Parte 1: Archiviare gli accordi firmati in SharePoint con Acrobat Sign {#part1}

Nella prima parte, utilizzerai un modello di flusso di lavoro Power Automate per impostare un flusso di lavoro automatizzato in cui verranno salvati tutti gli accordi firmati nel tuo sito SharePoint.

1. Seleziona Power Automate.
1. Cerca Acrobat Sign.

   ![Schermata di navigazione a Power Automate](assets/documentautomation/automation_1.png)

1. Scegli **Salvare un accordo Acrobat Sign completato nella libreria SharePoint**.

   ![Schermata dell’azione Salva un accordo Acrobat Sign completato nella libreria SharePoint](assets/documentautomation/automation_2.png)

1. Esaminare la schermata e configurare eventuali connessioni necessarie. Abilita la connessione Acrobat Sign.
1. Fai clic sul pulsante blu `+` simbolo.

   ![Schermata della connessione di flusso Acrobat Sign e SharePoint](assets/documentautomation/automation_3.png)

1. Immetti l’indirizzo e-mail del tuo account Acrobat Sign e fai clic sul campo Password nella nuova finestra.

   ![Schermata della schermata di accesso di Acrobat Sign](assets/documentautomation/automation_4.png)

   Attendi un momento per Adobe per controllare il tuo account.

   >[!NOTE]
   >
   >Questo controllo ti indirizzerà all&#39;accesso appropriato se stai utilizzando un Adobe ID o il nostro SSO aziendale.

1. Completa l&#39;accesso.
1. Fai clic **Continua** per passare alla schermata Modifica flusso.
1. Assegna un nome al trigger.

   ![Schermata per l&#39;assegnazione del nome al trigger](assets/documentautomation/automation_5.png)

1. Configura le impostazioni di SharePoint.

   ![Schermata per la configurazione delle impostazioni di SharePoint](assets/documentautomation/automation_6.png)

   **Indirizzo sito:** Sito SharePoint
   **Percorso cartella:** Percorso dei documenti condivisi che si desidera utilizzare
   **Nome file:** Accettate le impostazioni predefinite
   **Contenuto file:** Accettate le impostazioni predefinite

1. Salvare il flusso.

   ![Schermata dell’icona Salva](assets/documentautomation/automation_7.png)

1. Passare alla schermata di panoramica del flusso con la freccia posteriore blu. Verificherete questo flusso nella parte 2.

   ![Schermata della schermata di panoramica del flusso](assets/documentautomation/automation_8.png)

Questo flusso verrà testato nella parte successiva.

## Parte 2: Procedura di approvazione automatizzata per ottenere la firma elettronica con Acrobat Sign {#part2}

Nella seconda parte, costruiamo la prima parte con un Flusso più robusto e testiamo entrambi i Flussi per vederli in azione.

1. Seleziona **Modelli** sul lato sinistro dall’interfaccia di Power Automate.

   ![Schermata di selezione dei modelli](assets/documentautomation/automation_9.png)

1. Cerca &quot;approvazione del manager&quot;.
1. Seleziona **Richiesta di approvazione del manager per un file selezionato**.

   ![Schermata di selezione dell&#39;approvazione di Request Manager per un file selezionato](assets/documentautomation/automation_10.png)

   Verifica le connessioni e aggiungi quelle mancanti.

   >[!NOTE]
   >
   >Se si tratta del primo flusso eseguito con le approvazioni, queste verranno configurate completamente durante l&#39;esecuzione del flusso.

1. Fai clic **Continua** per passare alla schermata flow editing.

   Questo flusso include molti passaggi preconfigurati, tra cui il controllo degli errori e i passaggi condizionali nidificati.

1. Configura **Per un file selezionato** come segue:
   **Indirizzo sito:** Il tuo sito SharePoint
   **Nome libreria:** Archivio Documenti
1. Aggiungi un input nel modo seguente:
   **Tipo**: E-mail
   **Nome**: E-mail firmatario

   ![Schermata di configurazione del flusso](assets/documentautomation/automation_11.png)

1. Configura **Ottieni proprietà file:** come segue:
   **Indirizzo sito:** Il tuo sito SharePoint
   **Nome libreria:** Archivio Documenti

1. Scorri verso il basso e cerca **In caso affermativo**.

   ![Schermata della configurazione If yes](assets/documentautomation/automation_12.png)

1. Fai clic **Aggiungere un’azione** nella **In caso affermativo** (non nella parte inferiore) per aggiungere i passaggi da inviare per la firma.

   ![Schermata per l’aggiunta di un’azione nella casella Se sì](assets/documentautomation/automation_13.png)

1. Cerca **Ottieni contenuto file di SharePoint** e scegli **Ottieni contenuto file**.

   ![Schermata della casella di ricerca](assets/documentautomation/automation_14.png)

1. Configurare **Ottieni contenuto file** come segue:

   ![Schermata della configurazione Ottieni contenuto file](assets/documentautomation/automation_15.png)

   **Indirizzo sito:** Sito SharePoint.
   **Identificatore file:** Cercare &quot;identifier&quot; e scegliere Identifier dal menu **Ottieni proprietà file** passaggio.
1. Cerca &quot;Adobe&quot; e scegli **Acrobat Sign** per aggiungere un’altra azione.

   ![Schermata del menu di ricerca](assets/documentautomation/automation_16.png)

1. Immetti &quot;upload&quot; nella casella di ricerca di Acrobat Sign e seleziona **Carica un documento e ottieni l’ID del documento**.
1. Cercare la variabile dinamica **Nome** per ottenere il nome dell&#39;elemento o del documento selezionato nel trigger in **Nome file**.
1. Fai clic **Espressione** nell&#39;assistente variabili sotto **Contenuto del file**.

   ![Schermata di Caricamento di un documento e recupero della schermata ID documento](assets/documentautomation/automation_17.png)

1. Aggiungi un singolo apostrofo, quindi fai di nuovo clic per **Contenuto dinamico**, elimina l’apostrofo, seleziona **Contenuto del file** e quindi fare clic su **OK**.

   Assicurati che non siano presenti apostrofi aggiuntivi e che il risultato sia simile a quello illustrato nell’esempio seguente.

   ![Schermata dell&#39;aspetto della schermata Dynamic Content](assets/documentautomation/automation_18.png)

1. Nell’area di ricerca di Acrobat Sign, cerca &quot;crea&quot; per aggiungere un’altra azione Acrobat Sign.
1. Seleziona **Crea e accetta da un documento caricato e invialo per la firma**.

   ![Schermata della ricerca di Crea](assets/documentautomation/automation_19.png)

1. Configura le informazioni richieste: scegli **Nome** dall&#39;assistente variabile dinamico in **Nome accordo**.
Scegli **ID documento** dall&#39;assistente variabile dinamico in **ID documento**.
Scegli **E-mail firmatario** dall&#39;assistente variabile dinamico in **E-mail partecipante**.
Immettere &quot;1&quot; in **Ordine partecipanti**.
Scegli **Firmatario** dal menu a discesa **Ruolo partecipante**.

   ![Schermata delle informazioni richieste](assets/documentautomation/automation_20.png)

1. **Salva** il flusso.

### Prova del flusso

Accedere al repository dei documenti del sito SharePoint per verificarne il funzionamento.

1. Seleziona il documento e scegli **Automatizza** e **Flusso** hai appena creato.

   ![Schermata per la selezione del menu e del flusso Automatizza](assets/documentautomation/automation_21.png)

1. Avviare il flusso per convalidare le connessioni (solo prima esecuzione del flusso).
1. Immettere un messaggio valido per l&#39;approvatore in **Messaggio**.
1. Immetti l’indirizzo e-mail del firmatario del documento **E-mail firmatario**.
1. Fai clic **Esegui flusso**.

L’approvatore configurato per l’utente che avvia il flusso riceverà una richiesta di approvazione. Puoi approvare tramite e-mail o tramite il menu delle azioni di Power Automate.
Una volta approvato, firma il documento. A seconda dell’utente e del fatto che abbia effettuato l’accesso a Sign, potrebbe essere necessario aprire le finestre di firma in un browser privato.

![Schermata di apertura della finestra del browser privato](assets/documentautomation/automation_22.png)

Completa la procedura di firma, quindi riprova nella cartella SharePoint.

![Schermata della cartella SharePoint](assets/documentautomation/automation_23.png)

## Parte 3: OCR automatizzato dei documenti con Adobe PDF Tools {#part3}

Nella terza parte imparerai come automatizzare l’OCR nei PDF quando vengono importati in Microsoft SharePoint. Questo risolve un problema che si verifica con i documenti PDF scansionati che non sono ricercabili in SharePoint.

![Schermata del documento PDF nel browser](assets/documentautomation/automation_24.png)

### Impostare una cartella in SharePoint

Accedete a Microsoft SharePoint in cui desiderate archiviare i documenti.

1. Fai clic **+ Nuovo** per creare una nuova cartella denominata &quot;Contratti elaborati&quot;.
1. Fai clic **+ Nuovo** per creare una nuova cartella denominata &quot;Vecchi contratti&quot;.

   ![Schermata delle nuove cartelle](assets/documentautomation/automation_25.png)

A queste cartelle viene ora fatto riferimento come parte del flusso di Power Automate.

### Creare un flusso da un modello

1. Accedi a https://flow.microsoft.com.
1. Fai clic **Modelli** nella barra laterale.

   ![Schermata di selezione dei modelli](assets/documentautomation/automation_26.png)

1. Seleziona **Convertire i file aggiunti di recente in PDF per la ricerca di testo in SharePoint**.
1. Fare clic sul pulsante **+** accanto a Strumenti di Adobe PDF.

   ![Schermata per la selezione del simbolo +](assets/documentautomation/automation_27.png)

1. Passa a https://www.adobe.com/go/powerautomate_getstarted in una nuova scheda.
1. Fai clic su **Guida introduttiva**.

   ![Schermata del pulsante Inizia](assets/documentautomation/automation_28.png)

1. Accedi con l’Adobe ID.

   ![Schermata della schermata di accesso](assets/documentautomation/automation_29.png)

1. Immetti il nome e la descrizione delle credenziali e fai clic su **Crea credenziali**.

   ![Schermata della schermata Crea credenziali](assets/documentautomation/automation_30.png)

   Mantenere aperta la finestra con le credenziali. Sarà necessario inserirli in Microsoft Power Automate.

   ![Schermata della scheda del browser da mantenere aperta](assets/documentautomation/automation_31.png)

1. Immetti le credenziali e fai clic su **Crea in Microsoft Power Automate**.

   ![Schermata di immissione delle credenziali di PDF Tools](assets/documentautomation/automation_32.png)

1. Fai clic su **Continua**.

   ![Schermata da dove fare clic su Continua](assets/documentautomation/automation_33.png)

   Ora puoi vedere una vista del flusso di lavoro e dovrai configurarlo per il tuo ambiente.

1. Seleziona il campo Indirizzo sito e scegli il sito SharePoint che stai utilizzando sotto il trigger chiamato **Quando si crea un file in una cartella**.

   ![Schermata di selezione Quando viene creato un file in un trigger di cartella](assets/documentautomation/automation_34.png)

1. Fare clic sull&#39;icona della cartella per passare alla cartella Contratti precedenti che si trova in ID cartella.

   ![Schermata di selezione della cartella Contratti precedenti](assets/documentautomation/automation_35.png)

1. Modifica **Crea file** azione nella parte inferiore del flusso:

   Modifica **Indirizzo sito** all&#39;indirizzo del sito.
Specificare il percorso della cartella Contratti elaborati nel percorso della cartella.

1. Fai clic **Salva** nell&#39;angolo in alto a destra.
1. Fai clic **Test**.
1. Seleziona **Manualmente**.
1. Fai clic **Test**.

   ![Schermata del flusso di prova](assets/documentautomation/automation_36.png)

### Prova il nuovo flusso

1. Passa alla cartella Contratti precedenti in SharePoint.
1. Accedere a E03/Old Contracts nei file di esercizi scaricati.
1. Copiare i file ReleaseFormXX.pdf nella cartella Vecchi contratti in SharePoint.

   ![Schermata di copia dei file](assets/documentautomation/automation_37.png)

Ora, se si passa alla cartella Contratti elaborati, è possibile visualizzare i PDF disponibili dopo che il flusso è stato eseguito. Se apri i PDF, puoi vedere che il testo è selezionabile.
Inoltre, SharePoint indicizza il documento, consentendovi di effettuare una ricerca nel contenuto dei documenti dalla barra di ricerca in SharePoint.

## Parte 4: Assemblaggio automatico di documenti con Adobe PDF Tools {#part4}

Nella quarta parte imparerai come unire più documenti in base alle informazioni fornite durante la selezione e l’avvio di un flusso da Microsoft SharePoint. In questo scenario, il flusso:

* Chiedi informazioni per scegliere cosa includere in un pacchetto per un cliente.
* In base alle informazioni fornite, unisce molti documenti. Questi documenti includono una copertina e white paper opzionali.
* Il documento di unione viene salvato in SharePoint.

### Importare file di esercizi in SharePoint

1. Aprire la cartella E04 nei file dell&#39;esercizio.
1. Importa in SharePoint le cartelle Proposta, Modelli e Documenti generati.

   ![Schermata di importazione delle cartelle](assets/documentautomation/automation_38.png)

Queste cartelle verranno utilizzate come riferimento. In particolare, per la proposta verrà utilizzato il file Proposal.docx.

Nella cartella Modelli è presente una cartella Copertine che include progettazioni di frontespizi per diverse città. È inoltre disponibile una cartella Whitepapers che contiene white paper aggiuntivi opzionali che, se selezionati, verranno allegati alla fine.

### Importare il flusso in Microsoft Power Automate

1. Accedi a Microsoft Power Automate (https://flow.microsoft.com).
1. Fai clic **I miei flussi**.

   ![Schermata da dove selezionare I miei flussi](assets/documentautomation/automation_39.png)

1. Fai clic su **Importa**.

   ![Schermata di importazione](assets/documentautomation/automation_40.png)

1. Fai clic **Carica** e scegli la cartella GenerateProposal_20210311231623.zip in E04/Flows/.

   ![Schermata di selezione della cartella](assets/documentautomation/automation_41.png)

1. Fai clic su **Importa**.

1. Fai clic sull’icona a forma di chiave inglese sotto Azione accanto a **Invia proposta al cliente**.

   ![Schermata dell’icona della chiave inglese](assets/documentautomation/automation_42.png)

1. Seleziona **Crea come nuovo** in Configurazione.
1. Impostare il nome del flusso in Nome risorsa.
1. Fai clic su **Salva**.

   Ripeti questa operazione per le altre risorse correlate e seleziona la connessione.

   ![Schermata di salvataggio del file](assets/documentautomation/automation_43.png)

1. Fai clic **Importa** dopo aver effettuato tutti i collegamenti.

### Imposta per un file selezionato

Dopo aver creato il flusso, effettua le operazioni riportate di seguito:

1. Fate clic su **Modifica**.

   ![Schermata da cui selezionare la modifica](assets/documentautomation/automation_44.png)

1. Seleziona il trigger **Per un file selezionato**.

   Aggiungi il tuo sito SharePoint all&#39;indirizzo del sito.
Aggiungi la tua libreria nella libreria.

   ![Schermata del trigger completato](assets/documentautomation/automation_45.png)

### Imposta templateFolderPath

1. Fare clic sulla variabile templateFolderPath.
1. Impostate il percorso della cartella Modelli all&#39;interno del sito di SharePoint importato.

### Imposta Copertina Ottieni Contenuto File

1. Fai clic **Copertina** che espande l&#39;ambito.
1. Espandi **Copertina: Ottieni contenuto file**.

   Imposta l&#39;indirizzo del sito sul sito SharePoint.

   ![Schermata della copertina espansa](assets/documentautomation/automation_46.png)



### Imposta file selezionato

1. Espandere **File selezionato** azione ambito.

   Modificare l&#39;indirizzo del sito e il nome della libreria rispettivamente in Sito e Libreria di SharePoint in **Ottieni proprietà file**.
Cambia l&#39;indirizzo del sito nel sito SharePoint in **Ottieni contenuto file**.

   ![Schermata dell’azione espansa File selezionato](assets/documentautomation/automation_47.png)

### Imposta white paper

1. Fai clic **White paper** azione.
1. Espandi **Condizione: Aggiungi white paper**.

   ![Schermata della condizione espansa Aggiungi white paper](assets/documentautomation/automation_48.png)

1. Espandi **White paper 1: Ottenere il contenuto del file utilizzando il percorso**.
Modifica l&#39;indirizzo del sito nel sito SharePoint specificato.

Ripeti gli stessi passaggi per **Condizione: aggiungere white paper 2**.

### Imposta Crea file

1. Espandi **Crea file**.

   Modificare l&#39;indirizzo del sito e il percorso della cartella nel sito SharePoint e nel percorso in cui si trova la cartella Documenti generati.

1. Fai clic su **Salva**.

### Verifica del flusso

1. Passare alla cartella Proposta in SharePoint.
1. Selezionare la cartella Proposta.docx.

   ![Schermata di selezione della cartella della proposta](assets/documentautomation/automation_49.png)

1. Seleziona il flusso sotto **Automatizza** menu.

   ![Schermata di selezione del menu Automatizza](assets/documentautomation/automation_50.png)

1. Fai clic **Continua** per iniziare il flusso.

   ![Schermata di selezione del pulsante Continua](assets/documentautomation/automation_51.png)

1. Scegli Copertina e i white paper che desideri allegare.
1. Fai clic **Esegui flusso**.

   ![Schermata del pulsante Esegui flusso](assets/documentautomation/automation_52.png)

Passa alla cartella Genera documenti. Ora dovresti visualizzare il file PDF generato.

![Schermata della directory di SharePoint con il nuovo file PDF](assets/documentautomation/automation_53.png)

### Aggiunta di Protect e altre azioni al flusso

Dopo aver creato correttamente un flusso, si modificherà il flusso per crittografare il documento PDF con una password. Viene inoltre descritto come utilizzare altre azioni.

1. Torna alla fine del flusso.
1. Fare clic sul pulsante **+** simbolo tra **Unisci PDF** e **Crea file**.

   ![Schermata in cui selezionare il simbolo +](assets/documentautomation/automation_54.png)

1. Seleziona **Aggiungere un’azione**.
1. Cercate &quot;Adobe PDF Tools&quot;.

   ![Schermata di ricerca di Adobe PDF](assets/documentautomation/automation_55.png)

1. Seleziona **Protect PDF dalla visualizzazione**.
1. Utilizzare Contenuto dinamico per impostare il campo Nome file su **Nome file PDF da Merge PDF**.

   ![Schermata del contenuto dinamico](assets/documentautomation/automation_56.png)

   Nel trigger è presente un campo Password che fa parte del modulo di avvio. Possiamo usarlo qui.

1. Cerca **Campo Password** utilizzando il contenuto dinamico e inserirlo nel campo Password.

   ![Schermata di ricerca della password](assets/documentautomation/automation_57.png)

1. Usa contenuto dinamico per impostarlo su **Contenuto file PDF da PDF di unione** nel campo Contenuto file.
1. Modificare il **Crea file** per ottenere il contenuto del file da Protect PDF anziché da Merge PDF.
1. Espandi **Crea file**.
1. Cancella il campo Contenuto file.
1. Usa contenuto dinamico per inserire **Contenuto file PDF** da **Protect PDF dalla visualizzazione**.

### Verifica del flusso

1. Passare alla cartella Proposta in SharePoint.
1. Selezionare Proposta.docx.

   ![Schermata di selezione della cartella Proposta](assets/documentautomation/automation_58.png)

1. Seleziona **Automatizza** per scegliere il flusso.

   ![Schermata di selezione dell’opzione Automatizza dal menu](assets/documentautomation/automation_59.png)

1. Fai clic **Continua** per iniziare il flusso.

   ![Schermata della selezione di Continua](assets/documentautomation/automation_60.png)

1. Scegliere la copertina e i white paper da allegare.
1. Impostare il campo Password sulla Password che si desidera impostare.
1. Fai clic **Esegui flusso**.

   ![Schermata dei file selezionati e pulsante Esegui flusso](assets/documentautomation/automation_61.png)

1. Passa alla cartella Genera documenti.
Dovresti visualizzare il file PDF generato. Apri il file PDF e viene richiesto di immettere la password PDF.

   ![Schermata di PDF generato nella directory di SharePoint](assets/documentautomation/automation_62.png)
