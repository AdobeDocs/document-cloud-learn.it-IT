---
title: Inviare promemoria utilizzando la Guida alla configurazione di Acrobat Sign for Salesforce e Marketo
description: Scopri come inviare un promemoria via e-mail da Marketo quando un accordo non viene firmato dopo un certo periodo di tempo
feature: Integrations
role: Admin
solution: Acrobat Sign, Marketo Engage, Document Cloud
level: Intermediate
topic: Integrations
jira: KT-7248
topic-revisit: Integrations
thumbnail: KT-7248.jpg
exl-id: 33aca2e0-2f27-4100-a16f-85ba652c17a3
source-git-commit: a88ec5a68aa2a02ec2f118332ec31f47d3d5d300
workflow-type: tm+mt
source-wordcount: '921'
ht-degree: 0%

---

# Inviare promemoria utilizzando la Guida alla configurazione di Acrobat Sign for Salesforce e Marketo

Scopri come inviare un promemoria via e-mail da Marketo quando un accordo non viene firmato dopo un determinato periodo di tempo. Questa integrazione utilizza Acrobat Sign, Acrobat Sign per Salesforce, Marketo e Marketo e Salesforce Sync.

## Prerequisiti

1. Installa Marketo Salesforce Sync.

   Le informazioni e il plug-in più recente per Salesforce Sync sono disponibili [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/understanding-the-salesforce-sync.html)

1. Installa Acrobat Sign per Salesforce.

   Le informazioni su questo plug-in sono disponibili [qui.](https://helpx.adobe.com/ca/sign/using/salesforce-integration-installation-guide.html)

## Trovare l’oggetto personalizzato

Al termine delle configurazioni di Marketo Salesforce Sync e Acrobat Sign for Salesforce, nel Terminale di amministrazione di Marketo verranno visualizzate diverse nuove opzioni.

![Amministratore](assets/adminTab.png)

![Sincronizzazione oggetti](assets/salesforceAdmin.png)

1. Se è la prima volta, fai clic su **Sincronizza schema**. In caso contrario, fare clic su **Aggiorna schema**.

   ![Aggiorna](assets/refreshSchema1.png)

1. Se la sincronizzazione globale è in esecuzione, disattivarla facendo clic su **Disattiva sincronizzazione globale**.

   ![Disabilita](assets/disableGlobal.png)

1. Fare clic su **Aggiorna schema**.

   ![Aggiorna 2](assets/refreshSchema2.png)

## Sincronizzare l&#39;oggetto personalizzato

Sul lato destro, vedere Oggetti personalizzati basati su lead, contatto e account.

**Abilita la sincronizzazione** per gli oggetti in Lead se desideri inviare un promemoria quando un lead non ha firmato un accordo in Salesforce.

**Abilita la sincronizzazione** per gli oggetti in Referente se desideri inviare un promemoria quando un Referente non ha firmato un accordo in Salesforce.

**Abilita la sincronizzazione** per gli oggetti in Account se desideri inviare un promemoria quando un account non ha firmato un accordo in Salesforce.

1. **Abilita sincronizzazione** per l&#39;oggetto **Accordo** visualizzato sotto l&#39;elemento padre desiderato (lead, contatto o account). Esegui questa operazione per qualsiasi altro oggetto personalizzato che desideri sincronizzare.

   ![Oggetto accordo](assets/agreementObject.png)

1. Le risorse seguenti mostrano come **Abilitare la sincronizzazione**.

   ![Sincronizzazione personalizzata 1](assets/customObjectSync1.png)

   ![Sincronizzazione personalizzata 2](assets/customObjectSync2.png)

## Esporre i campi oggetto personalizzati ai trigger

1. Mentre la sincronizzazione globale è disattivata, seleziona l’oggetto personalizzato dell’accordo per il quale hai abilitato la sincronizzazione, quindi **Modifica campi visibili**.

1. Controlla il campo &quot;Nome accordo&quot; nella colonna del trigger per esporlo ai trigger dell&#39;azione della campagna. Controlla eventuali altri campi in base ai quali vuoi filtrare, quindi **Salva**.

   ![Modifica campi visibili 1](assets/editVisible1.png)

   ![Modifica campi visibili 2](assets/editVisible2.png)

1. Al termine dell&#39;attivazione della sincronizzazione sugli oggetti personalizzati e dell&#39;esposizione dei valori di attivazione, ricordati di riattivare la sincronizzazione:

   ![Abilita globale](assets/enableGlobal.png)

## Crea il programma e il token

1. Nella sezione Attività di marketing di Marketo, fare clic con il pulsante destro del mouse su **Attività di marketing** sulla barra di sinistra, selezionare **Nuova cartella campagna** e assegnargli un nome.

   ![Nuova cartella](assets/newFolder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella creata, selezionare **Nuovo programma** e assegnare un nome. Lasciare tutto il resto come predefinito, quindi fare clic su **Crea**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

1. Fai clic su **I miei token**, quindi trascina **Script e-mail** nell&#39;area di lavoro.

   ![Script e-mail](assets/emailScript.png)

1. Assegna un nome, quindi fai clic su **Fai clic per modificare**.

   ![Nome e modifica](assets/nameAndSave.png)

1. Espandere **Oggetti personalizzati** sul lato destro, quindi espandere l&#39;oggetto **Accordo**. Trova e trascina Nome accordo, Stato accordo, Data firma e URL firma nell’area di lavoro.

1. Scrivi uno script Velocity utilizzando questi token per visualizzare l’URL dell’accordo di un accordo che non viene firmato per una settimana. Di seguito è riportato un esempio che confronta la data corrente con la data Inviata:

   ```
   #foreach($agreement in $echosign_dev1__SIGN_Agreement__cList)
       #if($agreement.echosign_dev1__Status__c == "Out for Signature")
           #set($todayCalObj = $date.toCalendar($date.toDate("yyyy-MM-dd",$date.get('yyyy-MM-dd'))) )
           #set($dateSentCalObj = $date.toCalendar($date.toDate("yyyy-MM-dd",$agreement.echosign_dev1__DateSent__c)) )
           #set($dateDiff = ($todayCalObj.getTimeInMillis() - $dateSentCalObj.getTimeInMillis()) / 86400000 )
   
           #if($dateDiff >= 7)
               #set($agreementName = $agreement.Name)
               #set($agreementURL = $agreement.echosign_dev1__Signing_URL__c.substring(8))
               #break
           #else
           #end
       #else
       #end
   #end
   
   #if(${agreementName})
       <a href="https://${agreementURL}">${agreementName}</a>
   #else
       Please contact us. 
   #end
   ```

1. Fai clic su **Salva**.

## Crea il promemoria e aggiungi personalizzazione

Esempi di personalizzazione includono: il nome del firmatario, il nome dell’accordo, un collegamento all’accordo e così via.

1. Fai clic con il pulsante destro del mouse sul programma creato e fai clic su **Nuova risorsa locale**, quindi seleziona **E-mail**.

   ![Nuovo messaggio di posta elettronica](assets/createNewEmail.png)

1. Nella nuova scheda, immettete un **Nome** e una **Descrizione** per l&#39;e-mail e selezionate un modello dal selettore di modelli. Fate clic su **Crea**.

   ![Selettore modello](assets/templatePicker.png)

1. Impostare **Da nome** e **Da indirizzo**.

   ![E-mail di promemoria](assets/reminderEmail.png)

1. Fare clic sul corpo del messaggio per attivare l&#39;editor. Fai clic sul pulsante **Inserisci token**, trova il token URL dell’accordo personalizzato creato, quindi fai clic su **Inserisci**. Completa la personalizzazione dell&#39;e-mail e fai clic su **Salva**.

   ![Inserisci token](assets/insertToken.png)

1. Visualizza l’anteprima utilizzando un profilo a cui è assegnato un accordo. Dovresti visualizzare un collegamento all’URL con il Nome accordo come etichetta.

   ![Collegamento e-mail](assets/emailLink.png)

## Impostare il filtro Smart Campaign

1. Fare clic con il pulsante destro del mouse sul programma creato, quindi scegliere **Nuova campagna avanzata**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegna un nome a tua scelta, quindi fai clic su **Crea**.

   ![Smart Campaign 2](assets/smartCampaign2.png)

1. Cerca, quindi fai clic e trascina **Has Agreement** (Ha accordo) nell&#39;elenco smart.

   ![Ha Un Accordo](assets/hasAgreement.png)

1. I campi esposti al trigger dovrebbero ora essere disponibili in **Aggiungi vincolo**. Seleziona **Stato accordo** ed eventuali altri campi in base ai quali desideri filtrare l’accordo. Per ogni campo aggiunto, definisci i valori in base ai quali filtrare. In questo caso, verrà attivato solo quando **Stato accordo** è Inviato per firma e **Data di invio** è anteriore a 7 giorni.

   ![Stato accordo](assets/agreementStatus.png)

   >[!NOTE]
   >
   > Aggiungi un identificatore univoco ai vincoli, ad esempio **Nome accordo**, se desideri che questa campagna venga eseguita solo per determinati accordi.

1. Confermare il pubblico della campagna e vedere chi sarà idoneo nella scheda Pianificazione.

   ![Qualificatori](assets/qualifiers.png)

## Impostare il flusso della campagna intelligente

Poiché è stato utilizzato il filtro campagna **Giorni senza firma**, è possibile utilizzare una ricorrenza pianificata per la campagna.

1. Fare clic sulla scheda **Flusso** in Smart Campaign. Cerca e trascina il flusso **Invia e-mail** nell&#39;area di lavoro e seleziona l&#39;e-mail di promemoria creata nella sezione precedente.

   ![Invia messaggio di posta elettronica](assets/sendEmail.png)

1. Fare clic sulla scheda **Pianificazione** in Smart Campaign. Assicurati che il flusso della campagna sia limitato all&#39;esecuzione una sola volta a persona nelle **Impostazioni campagna avanzata**. Fai quindi clic sulla scheda **Pianifica ricorrenza**.

   ![Scheda Pianificazione](assets/scheduleTab.png)

1. Impostare **Pianificazione** su Giornaliera, scegliere il giorno e l&#39;ora di inizio e la data di fine della campagna, se necessario.

   ![Impostazioni pianificazione](assets/scheduleSettings.png)

