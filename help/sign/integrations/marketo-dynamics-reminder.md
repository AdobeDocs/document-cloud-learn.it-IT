---
title: Inviare promemoria utilizzando Acrobat Sign per Microsoft Dynamics 365 e Marketo
description: Scopri come inviare un promemoria e-mail quando un accordo non viene firmato dopo un certo periodo di tempo
feature: Integrations
role: Admin
solution: Acrobat Sign, Marketo Engage, Document Cloud
level: Intermediate
topic: Integrations
topic-revisit: Integrations
jira: KT-7250
thumbnail: KT-7250.jpg
exl-id: 5a97fade-18a3-448a-8504-efb9e38e9187
source-git-commit: a88ec5a68aa2a02ec2f118332ec31f47d3d5d300
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 0%

---

# Inviare promemoria utilizzando Acrobat Sign per Microsoft Dynamics 365 e Marketo

Scopri come inviare un promemoria e-mail quando un accordo non viene firmato dopo un determinato periodo di tempo. Questa integrazione utilizza Acrobat Sign, Acrobat Sign per Microsoft Dynamics, Marketo e Marketo Microsoft Dynamics Sync.

## Prerequisiti

1. Installa Marketo Microsoft Dynamics Sync.

   Le informazioni e il plug-in più recente per Microsoft Dynamics Sync sono disponibili [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html)

1. Installa [Acrobat Sign per Microsoft Dynamics](https://appsource.microsoft.com/it-it/product/dynamics-365/adobesign.f3b856fc-a427-4d47-ad4b-d5d1baba6f86).

   Le informazioni su questo plug-in sono disponibili [qui.](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)

## Trovare l’oggetto personalizzato

Una volta completate le configurazioni di Marketo Microsoft Dynamics Sync e Acrobat Sign per Dynamics, nel Terminale di amministrazione Marketo vengono visualizzate due nuove opzioni.

![Amministratore](assets/adminTerminal.png)

1. Fare clic su **[!UICONTROL Dynamics Entities Sync]**.

   La sincronizzazione deve essere disattivata prima di sincronizzare le entità personalizzate. Se è la prima volta, fai clic su **Sincronizza schema**. In caso contrario, fare clic su **Aggiorna schema**.

   ![Aggiorna](assets/refreshSchema.png)

## Sincronizzare l&#39;oggetto personalizzato

1. Sul lato destro, individua gli oggetti personalizzati basati su [!UICONTROL Lead], [!UICONTROL Contact] e [!UICONTROL Account].

   * **Abilitare la sincronizzazione** per gli oggetti in **[!UICONTROL Lead]** se si desidera inviare un promemoria quando un [!UICONTROL Lead] non ha firmato un accordo in Dynamics.

   * **Abilitare la sincronizzazione** per gli oggetti in **[!UICONTROL Contatto]** se si desidera inviare un promemoria quando un [!UICONTROL Contatto] non ha firmato un accordo in Dynamics.

   * **Abilitare la sincronizzazione** per gli oggetti in **[!UICONTROL Account]** se si desidera inviare un promemoria quando un [!UICONTROL Account] non ha firmato un accordo in Dynamics.

   * **Abilita sincronizzazione** per l’oggetto accordo nel **[!UICONTROL principale]** ([!UICONTROL lead], [!UICONTROL contatto] o [!UICONTROL account]) desiderato.

   ![Oggetti personalizzati](assets/enableSyncDynamics.png)

1. Nella nuova finestra seleziona le proprietà desiderate in Accordo, quindi abilita le caselle in **Vincolo** e **Attivazione** per esporle alle tue attività di marketing.

   ![Sincronizzazione personalizzata 1](assets/entitySync1.png)

   ![Sincronizzazione personalizzata 2](assets/entitySync2.png)

1. Riattiva la sincronizzazione dopo aver attivato la sincronizzazione sugli oggetti personalizzati.

   Torna al Terminale di amministrazione, fai clic su **Microsoft Dynamics**, quindi su **Abilita sincronizzazione**.

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![Abilita globale](assets/enableGlobalDynamics.png)

## Crea il programma e il token

1. Nella sezione Attività di marketing di Marketo, fai clic con il pulsante destro del mouse su **Attività di marketing** nella barra di sinistra.

   Selezionare **Nuova cartella campagna** e assegnargli un nome.

   ![Nuova cartella](assets/newFolder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella creata, selezionare **Nuovo programma** e assegnare un nome.

   Lasciare tutto il resto come predefinito, quindi fare clic su **Crea**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

1. Fai clic su **I miei token**, quindi trascina **Script e-mail** nell&#39;area di lavoro.

   ![Script e-mail](assets/emailScript.png)

1. Assegna un nome, quindi fai clic su **Fai clic per modificare**.

   ![Nome e modifica](assets/nameAndSave.png)

1. Espandere **[!UICONTROL Oggetti personalizzati]** sul lato destro, quindi espandere l&#39;oggetto **[!UICONTROL Accordo]**.

   Trova e trascina [!UICONTROL Nome], Stato accordo, Inviato il e URL firmatario corrente nell&#39;area di lavoro.

1. Scrivi uno script Velocity utilizzando questi token per visualizzare l’URL dell’accordo di un accordo che non viene firmato per una settimana. Di seguito è riportato un esempio che confronta la data corrente con quella di Inviato il:

   ```
   #foreach($agreement in $adobe_agreementList)
       #if($agreement.adobe_esagreementstatus == "Out for Signature")
           #set($todayCalObj = $date.toCalendar($date.toDate("yyyy-MM-dd",$date.get('yyyy-MM-dd'))) )
           #set($dateSentCalObj = $date.toCalendar($date.toDate("yyyy-MM-dd",$agreement.adobe_datesent)) )
           #set($dateDiff = ($todayCalObj.getTimeInMillis() - $dateSentCalObj.getTimeInMillis()) / 86400000 )
   
           #if($dateDiff >= 7)
               #set($agreementName = $agreement.adobe_name)
               #set($agreementURL = $agreement.adobe_currentsignerurl.substring(8))
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

1. Fai clic su **[!UICONTROL Salva]**.

## Crea il promemoria e aggiungi personalizzazione

Esempi di personalizzazione includono: il nome del firmatario, il nome dell’accordo, un collegamento all’accordo e così via.

1. Fai clic con il pulsante destro del mouse sul programma creato e fai clic su **[!UICONTROL Nuova risorsa locale]**, quindi seleziona **[!UICONTROL E-mail]**.

   ![Nuovo messaggio di posta elettronica](assets/createNewEmail.png)

1. Nella nuova scheda, immettete un **[!UICONTROL Nome]** e una **[!UICONTROL Descrizione]** per l&#39;e-mail e selezionate un modello dal selettore di modelli.

   ![Selettore modello](assets/templatePicker.png)

1. Fate clic su **[!UICONTROL Crea]**.

1. Impostare **[!UICONTROL Da nome]** e **[!UICONTROL Da indirizzo]**.

   ![E-mail di promemoria](assets/reminderEmail.png)

1. Fare clic sul corpo del messaggio per attivare l&#39;editor.

   Fai clic sul pulsante **[!UICONTROL Inserisci token]**, trova il token URL dell’accordo personalizzato creato, quindi fai clic su **[!UICONTROL Inserisci]**. Completa la personalizzazione dell&#39;e-mail e fai clic su **[!UICONTROL Salva]**.

   ![Inserisci token](assets/insertToken.png)

1. Visualizza l’anteprima utilizzando un profilo a cui è assegnato un accordo.

   Dovresti visualizzare un collegamento all’URL con il Nome accordo come etichetta.

   ![Collegamento e-mail](assets/emailLink.png)

## Impostare il filtro Smart Campaign

1. Fare clic con il pulsante destro del mouse sul programma creato, quindi scegliere **[!UICONTROL Nuova campagna avanzata]**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegna un nome a tua scelta, quindi fai clic su **[!UICONTROL Crea]**.

   ![Smart Campaign 2](assets/smartCampaign2.png)

1. Cerca, quindi fai clic e trascina **[!UICONTROL Has Agreement]** (Ha accordo) nell&#39;elenco smart.

   ![Ha Un Accordo](assets/hasAgreementDynamics1.png)

   I campi esposti al trigger dovrebbero essere disponibili in **[!UICONTROL Aggiungi vincolo]**.

1. Seleziona **[!UICONTROL Stato accordo]** ed eventuali altri campi in base ai quali desideri filtrare l’accordo.

   Per ogni campo aggiunto, definisci i valori in base ai quali filtrare. In questo caso, viene attivato solo quando **[!UICONTROL Stato accordo]** è *Inviato per firma* e **[!UICONTROL Inviato il]** è *passato prima di 1 settimana*.

   ![Stato accordo](assets/hasAgreementDynaSentOn.png)

   >[!NOTE]
   >
   > Aggiungi un identificatore univoco ai vincoli, ad esempio **Nome**, se desideri che questa campagna venga eseguita solo per determinati accordi.

1. Confermare il pubblico della campagna e vedere chi sarà idoneo nella scheda Pianificazione.

   ![Qualificatori](assets/qualifiers.png)

## Impostare il flusso della campagna intelligente

Poiché è stato utilizzato il filtro campagna **Giorni alla scadenza**, è possibile utilizzare una ricorrenza pianificata per la campagna.

1. Fare clic sulla scheda **[!UICONTROL Flusso]** nella [!UICONTROL Smart Campaign].

   Cerca e trascina il flusso **Invia e-mail** nell&#39;area di lavoro e seleziona l&#39;e-mail di promemoria creata nella sezione precedente.

   ![Invia messaggio di posta elettronica](assets/sendEmail.png)

1. Fare clic sulla scheda **[!UICONTROL Pianificazione]** in Smart Campaign. Assicurati che il flusso della campagna sia limitato all&#39;esecuzione una sola volta a persona nelle **Impostazioni campagna avanzata**. Fai quindi clic sulla scheda **Pianifica ricorrenza**.

   ![Scheda Pianificazione](assets/scheduleTab.png)

1. Imposta **Pianificazione** su _Giornaliera_. Scegliere un giorno di inizio, un&#39;ora e una data di fine per la campagna, se necessario.

   ![Impostazioni pianificazione](assets/scheduleSettings.png)
