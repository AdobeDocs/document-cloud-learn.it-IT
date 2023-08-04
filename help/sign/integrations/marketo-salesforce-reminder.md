---
title: Inviare promemoria utilizzando la Guida alla configurazione di Acrobat Sign for Salesforce e Marketo
description: Scopri come inviare un promemoria via e-mail da Marketo quando un accordo non viene firmato dopo un certo periodo di tempo
feature: Integrations
role: Admin
solution: Acrobat Sign, Marketo, Document Cloud
level: Intermediate
topic: Integrations
jira: KT-7248
topic-revisit: Integrations
thumbnail: KT-7248.jpg
exl-id: 33aca2e0-2f27-4100-a16f-85ba652c17a3
source-git-commit: 452299b2b786beab9df7a5019da4f3840d9cdec9
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 1%

---

# Inviare promemoria utilizzando la Guida alla configurazione di Acrobat Sign for Salesforce e Marketo

Scopri come inviare un promemoria via e-mail da Marketo quando un accordo non viene firmato dopo un determinato periodo di tempo. Questa integrazione utilizza Acrobat Sign, Acrobat Sign per Salesforce, Marketo e Marketo e Salesforce Sync.

## Prerequisiti

1. Installa Marketo Salesforce Sync.

   Sono disponibili le informazioni e il plug-in più recente per Salesforce Sync [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/understanding-the-salesforce-sync.html)

1. Installa Acrobat Sign per Salesforce.

   Sono disponibili informazioni su questo plug-in [qui.](https://helpx.adobe.com/ca/sign/using/salesforce-integration-installation-guide.html)

## Trovare l’oggetto personalizzato

Al termine delle configurazioni di Marketo Salesforce Sync e Acrobat Sign for Salesforce, nel Terminale di amministrazione di Marketo verranno visualizzate diverse nuove opzioni.

![Amministrazione](assets/adminTab.png)

![Sincronizzazione oggetti](assets/salesforceAdmin.png)

1. Fai clic **Sincronizza schema** se è la prima volta. In caso contrario, fare clic su **Aggiorna schema**.

   ![Aggiorna](assets/refreshSchema1.png)

1. Se la sincronizzazione globale è in esecuzione, disattivarla facendo clic su **Disattiva sincronizzazione globale**.

   ![Disabilita](assets/disableGlobal.png)

1. Fai clic **Aggiorna schema**.

   ![Aggiorna 2](assets/refreshSchema2.png)

## Sincronizzare l&#39;oggetto personalizzato

Sul lato destro, vedere Oggetti personalizzati basati su lead, contatto e account.

**Attiva sincronizzazione** per gli oggetti in Lead, se desideri inviare un promemoria quando un lead non ha firmato un accordo in Salesforce.

**Attiva sincronizzazione** per gli oggetti in Referente, se desideri inviare un promemoria quando un Referente non ha firmato un accordo in Salesforce.

**Attiva sincronizzazione** per gli oggetti in Account, se desideri inviare un promemoria quando un account non ha firmato un accordo in Salesforce.

1. **Attiva sincronizzazione** per **Accordo** oggetto visualizzato sotto l&#39;elemento padre desiderato (lead, contatto o account). Esegui questa operazione per qualsiasi altro oggetto personalizzato che desideri sincronizzare.

   ![Oggetto Accordo](assets/agreementObject.png)

1. Le seguenti risorse mostrano come **Attiva sincronizzazione**.

   ![Sincronizzazione personalizzata 1](assets/customObjectSync1.png)

   ![Sincronizzazione personalizzata 2](assets/customObjectSync2.png)

## Esporre i campi oggetto personalizzati ai trigger

1. Mentre la sincronizzazione globale è disattivata, seleziona l’oggetto personalizzato dell’accordo per il quale hai abilitato la sincronizzazione, quindi **Modifica campi visibili**.

1. Controlla il campo &quot;Nome accordo&quot; nella colonna del trigger per esporlo ai trigger dell&#39;azione della campagna. Seleziona eventuali altri campi in base ai quali filtrare, quindi **Salva**.

   ![Modifica campi visibili 1](assets/editVisible1.png)

   ![Modifica campi visibili 2](assets/editVisible2.png)

1. Al termine dell&#39;attivazione della sincronizzazione sugli oggetti personalizzati e dell&#39;esposizione dei valori di attivazione, ricordati di riattivare la sincronizzazione:

   ![Abilita globale](assets/enableGlobal.png)

## Crea il programma e il token

1. Nella sezione Attività di marketing di Marketo, fare clic con il pulsante destro del mouse su **Attività di marketing** sulla barra di sinistra, seleziona **Nuova cartella campagna** e assegnagli un nome.

   ![Nuova cartella](assets/newFolder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella creata e selezionare **Nuovo programma** e assegnagli un nome. Lascia tutti gli altri come predefiniti, quindi fai clic su **Crea**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

1. Fai clic su **I miei token**, quindi trascina  **Script e-mail** nell&#39;area di lavoro.

   ![Script e-mail](assets/emailScript.png)

1. Assegnare un nome, quindi fare clic su **Fai clic per modificare**.

   ![Nome e modifica](assets/nameAndSave.png)

1. Espandi **Oggetti personalizzati** sul lato destro, quindi espandere **Accordo** oggetto. Trova e trascina Nome accordo, Stato accordo, Data firma e URL firma nell’area di lavoro.

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

1. Fare clic con il pulsante destro del mouse sul programma creato e scegliere **Nuova risorsa locale**, quindi seleziona **E-mail**.

   ![Nuovo indirizzo e-mail](assets/createNewEmail.png)

1. Nella nuova scheda, immettete un **Nome** e **Descrizione** per l’e-mail e seleziona un modello dal selettore modelli. Fai clic su **Crea**.

   ![Selettore modello](assets/templatePicker.png)

1. Impostare la **Nome mittente** e **Indirizzo mittente**.

   ![E-mail promemoria](assets/reminderEmail.png)

1. Fare clic sul corpo del messaggio per attivare l&#39;editor. Fare clic sul pulsante **Inserisci token** trova il token URL dell’accordo personalizzato che hai creato, quindi fai clic su **Inserisci**. Completa la personalizzazione dell’e-mail e fai clic su **Salva**.

   ![Inserisci token](assets/insertToken.png)

1. Visualizza l’anteprima utilizzando un profilo a cui è assegnato un accordo. Dovresti visualizzare un collegamento all’URL con il Nome accordo come etichetta.

   ![Collegamento e-mail](assets/emailLink.png)

## Impostare il filtro Smart Campaign

1. Fare clic con il pulsante destro del mouse sul programma creato, quindi scegliere **Nuova campagna intelligente**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegna un nome a tua scelta, quindi fai clic su **Crea**.

   ![Smart Campaign 2](assets/smartCampaign2.png)

1. Cerca, quindi fai clic e trascina **Con accordo** all&#39;elenco smart.

   ![Con accordo](assets/hasAgreement.png)

1. I campi esposti al trigger dovrebbero ora essere disponibili in **Aggiungi vincolo**. Seleziona **Stato accordo** e qualsiasi altro campo in base al quale si desidera filtrare i dati. Per ogni campo aggiunto, definisci i valori in base ai quali filtrare. In questo caso, verrà attivato solo quando **Stato accordo** è stato inviato per la firma e **Data invio** è trascorso prima dei 7 giorni.

   ![Stato dell’accordo](assets/agreementStatus.png)

   >[!NOTE]
   >
   > d un identificatore univoco per i vincoli, ad esempio **Nome accordo**, se desideri che questa campagna venga eseguita solo per determinati accordi.

1. Confermare il pubblico della campagna e vedere chi sarà idoneo nella scheda Pianificazione.

   ![Qualificatori](assets/qualifiers.png)

## Impostare il flusso della campagna intelligente

Perché il filtro della campagna **Giorni senza firma** è stata utilizzata, è possibile utilizzare una ricorrenza pianificata per la campagna.

1. Fare clic sul pulsante **Flusso** nella scheda Smart Campaign. Cercare e trascinare **Invia e-mail** passa all’area di lavoro e seleziona l’e-mail di promemoria creata nella sezione precedente.

   ![Invia e-mail](assets/sendEmail.png)

1. Fare clic sul pulsante **Pianificazione** nella scheda Smart Campaign. Assicurati che il flusso della campagna sia limitato all&#39;esecuzione una sola volta a persona nel **Impostazioni Smart Campaign**. Fai quindi clic sul pulsante **Pianifica ricorrenza** scheda.

   ![Scheda Pianificazione](assets/scheduleTab.png)

1. Impostare la **Pianificazione** in Giornaliero, scegliere un giorno e un&#39;ora di inizio e una data di fine della campagna, se necessario.

   ![Impostazioni pianificazione](assets/scheduleSettings.png)

>[!TIP]
>
>Questo tutorial fa parte del corso [Accelera i cicli di vendita con Acrobat Sign per Salesforce e Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) questo è disponibile gratuitamente su Experience League!
