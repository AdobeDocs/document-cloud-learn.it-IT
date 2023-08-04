---
title: Inviare promemoria utilizzando Acrobat Sign per Microsoft Dynamics 365 e Marketo
description: Scopri come inviare un promemoria e-mail quando un accordo non viene firmato dopo un certo periodo di tempo
feature: Integrations
role: Admin
solution: Acrobat Sign, Marketo, Document Cloud
level: Intermediate
topic: Integrations
topic-revisit: Integrations
jira: KT-7250
thumbnail: KT-7250.jpg
exl-id: 5a97fade-18a3-448a-8504-efb9e38e9187
source-git-commit: 452299b2b786beab9df7a5019da4f3840d9cdec9
workflow-type: tm+mt
source-wordcount: '911'
ht-degree: 2%

---

# Inviare promemoria utilizzando Acrobat Sign per Microsoft Dynamics 365 e Marketo

Scopri come inviare un promemoria e-mail quando un accordo non viene firmato dopo un determinato periodo di tempo. Questa integrazione utilizza Acrobat Sign, Acrobat Sign per Microsoft Dynamics, Marketo e Marketo Microsoft Dynamics Sync.

## Prerequisiti

1. Installa Marketo Microsoft Dynamics Sync.

   Sono disponibili le informazioni e il plug-in più recente per Microsoft Dynamics Sync [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html)

1. Installa [Acrobat Sign per Microsoft Dynamics](https://appsource.microsoft.com/it-it/product/dynamics-365/adobesign.f3b856fc-a427-4d47-ad4b-d5d1baba6f86).

   Sono disponibili informazioni su questo plug-in [qui.](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)

## Trovare l’oggetto personalizzato

Una volta completate le configurazioni di Marketo Microsoft Dynamics Sync e Acrobat Sign per Dynamics, nel Terminale di amministrazione Marketo vengono visualizzate due nuove opzioni.

![Amministrazione](assets/adminTerminal.png)

1. Fai clic **[!UICONTROL Sincronizzazione delle entità Dynamics]**.

   La sincronizzazione deve essere disattivata prima di sincronizzare le entità personalizzate. Fai clic **Sincronizza schema** se è la prima volta. In caso contrario, fare clic su **Aggiorna schema**.

   ![Aggiorna](assets/refreshSchema.png)

## Sincronizzare l&#39;oggetto personalizzato

1. Sul lato destro, individua [!UICONTROL Lead], [!UICONTROL Contatto]e [!UICONTROL Account]oggetti personalizzati basati su.

   * **Attiva sincronizzazione** per gli oggetti sotto **[!UICONTROL Lead]** se desideri inviare un promemoria quando un [!UICONTROL Lead] non ha firmato un accordo in Dynamics.

   * **Attiva sincronizzazione** per gli oggetti sotto **[!UICONTROL Contatto]** se desideri inviare un promemoria quando un [!UICONTROL Contatto] non ha firmato un accordo in Dynamics.

   * **Attiva sincronizzazione** per gli oggetti sotto **[!UICONTROL Account]** se desideri inviare un promemoria quando un [!UICONTROL Account] non ha firmato un accordo in Dynamics.

   * **Attiva sincronizzazione** per l’oggetto accordo nel campo desiderato **[!UICONTROL Elemento padre]** ([!UICONTROL Lead], [!UICONTROL Contatto]o [!UICONTROL Account]).

   ![Oggetti personalizzati](assets/enableSyncDynamics.png)

1. Nella nuova finestra seleziona le proprietà desiderate in Accordo, quindi abilita le caselle sotto **Vincolo** e **Attivazione** per esporli alle tue attività di marketing.

   ![Sincronizzazione personalizzata 1](assets/entitySync1.png)

   ![Sincronizzazione personalizzata 2](assets/entitySync2.png)

1. Riattiva la sincronizzazione dopo aver attivato la sincronizzazione sugli oggetti personalizzati.

   Torna al Terminale di amministrazione, fai clic su **Microsoft Dynamics**, quindi fare clic su **Attiva sincronizzazione**.

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![Abilita globale](assets/enableGlobalDynamics.png)

## Crea il programma e il token

1. Nella sezione Attività di marketing di Marketo, fare clic con il pulsante destro del mouse su **Attività di marketing** sulla barra sinistra.

   Seleziona **Nuova cartella campagna** e assegnagli un nome.

   ![Nuova cartella](assets/newFolder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella creata e selezionare **Nuovo programma** e assegnagli un nome.

   Lascia tutti gli altri come predefiniti, quindi fai clic su **Crea**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

1. Fai clic su **I miei token**, quindi trascina **Script e-mail** nell&#39;area di lavoro.

   ![Script e-mail](assets/emailScript.png)

1. Assegnare un nome, quindi fare clic su **Fai clic per modificare**.

   ![Nome e modifica](assets/nameAndSave.png)

1. Espandi **[!UICONTROL Oggetti personalizzati]** sul lato destro, quindi espandere **[!UICONTROL Accordo]** oggetto.

   Trova e trascina [!UICONTROL Nome], Stato accordo, Inviato il e URL firmatario corrente nell’area di lavoro.

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

1. Fare clic con il pulsante destro del mouse sul programma creato e scegliere **[!UICONTROL Nuova risorsa locale]**, quindi seleziona **[!UICONTROL E-mail]**.

   ![Nuovo indirizzo e-mail](assets/createNewEmail.png)

1. Nella nuova scheda, immettete un **[!UICONTROL Nome]** e **[!UICONTROL Descrizione]** per l’e-mail e seleziona un modello dal selettore modelli.

   ![Selettore modello](assets/templatePicker.png)

1. Fai clic su **[!UICONTROL Crea]**.

1. Impostare la **[!UICONTROL Nome mittente]** e **[!UICONTROL Indirizzo mittente]**.

   ![E-mail promemoria](assets/reminderEmail.png)

1. Fare clic sul corpo del messaggio per attivare l&#39;editor.

   Fare clic sul pulsante **[!UICONTROL Inserisci token]** trova il token URL dell’accordo personalizzato che hai creato, quindi fai clic su **[!UICONTROL Inserisci]**. Completa la personalizzazione dell’e-mail e fai clic su **[!UICONTROL Salva]**.

   ![Inserisci token](assets/insertToken.png)

1. Visualizza l’anteprima utilizzando un profilo a cui è assegnato un accordo.

   Dovresti visualizzare un collegamento all’URL con il Nome accordo come etichetta.

   ![Collegamento e-mail](assets/emailLink.png)

## Impostare il filtro Smart Campaign

1. Fare clic con il pulsante destro del mouse sul programma creato, quindi scegliere **[!UICONTROL Nuova campagna intelligente]**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegna un nome a tua scelta, quindi fai clic su **[!UICONTROL Crea]**.

   ![Smart Campaign 2](assets/smartCampaign2.png)

1. Cerca, quindi fai clic e trascina **[!UICONTROL Con accordo]** all&#39;elenco smart.

   ![Con accordo](assets/hasAgreementDynamics1.png)

   I campi esposti al trigger dovrebbero essere disponibili in **[!UICONTROL Aggiungi vincolo]**.

1. Seleziona **[!UICONTROL Stato accordo]** e qualsiasi altro campo in base al quale si desidera filtrare i dati.

   Per ogni campo aggiunto, definisci i valori in base ai quali filtrare. In questo caso, viene attivato solo quando **[!UICONTROL Stato accordo]** è *Inviato per firma* e **[!UICONTROL Data invio]** è *in passato prima di 1 settimana*.

   ![Stato dell’accordo](assets/hasAgreementDynaSentOn.png)

   >[!NOTE]
   >
   > Aggiungi un identificatore univoco ai vincoli, ad esempio **Nome**, se desideri che questa campagna venga eseguita solo per determinati accordi.

1. Confermare il pubblico della campagna e vedere chi sarà idoneo nella scheda Pianificazione.

   ![Qualificatori](assets/qualifiers.png)

## Impostare il flusso della campagna intelligente

Perché il filtro della campagna **Giorni alla scadenza** è stata utilizzata, è possibile utilizzare una ricorrenza pianificata per la campagna.

1. Fare clic sul pulsante **[!UICONTROL Flusso]** nella scheda [!UICONTROL Smart Campaign].

   Cercare e trascinare **Invia e-mail** passa all’area di lavoro e seleziona l’e-mail di promemoria creata nella sezione precedente.

   ![Invia e-mail](assets/sendEmail.png)

1. Fare clic sul pulsante **[!UICONTROL Pianificazione]** nella scheda Smart Campaign. Assicurati che il flusso della campagna sia limitato all&#39;esecuzione una sola volta a persona nel **Impostazioni Smart Campaign**. Fai quindi clic sul pulsante **Pianifica ricorrenza** scheda.

   ![Scheda Pianificazione](assets/scheduleTab.png)

1. Impostare la **Pianificazione** a _Giornaliera_. Scegliere un giorno di inizio, un&#39;ora e una data di fine per la campagna, se necessario.

   ![Impostazioni pianificazione](assets/scheduleSettings.png)

>[!TIP]
>
>Questo tutorial fa parte del corso [Accelera i cicli di vendita con Acrobat Sign per Microsoft Dynamics e Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) questo è disponibile gratuitamente su Experience League!