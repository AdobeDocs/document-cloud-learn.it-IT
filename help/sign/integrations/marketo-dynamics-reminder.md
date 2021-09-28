---
title: Invia promemoria utilizzando Adobe Sign per Microsoft Dynamics 365 e Marketo
description: Informazioni su come inviare un promemoria tramite posta elettronica quando un contratto rimane privo di firma dopo un periodo di tempo
role: Admin
product: adobe sign
solution: Adobe Sign, Marketo, Document Cloud
level: Intermediate
topic-revisit: Integrations
thumbnail: KT-7250.jpg
exl-id: 5a97fade-18a3-448a-8504-efb9e38e9187
source-git-commit: bcddb0ee106239f2786debaed908b2a2ec5ce792
workflow-type: tm+mt
source-wordcount: '911'
ht-degree: 3%

---

# Invia promemoria utilizzando Adobe Sign per Microsoft Dynamics 365 e Marketo

Informazioni su come inviare un promemoria tramite posta elettronica quando un contratto rimane privo di firma dopo un periodo di tempo. Questa integrazione utilizza Adobe Sign, Adobe Sign per Microsoft Dynamics, Marketo e Marketo Microsoft Dynamics Sync.

## Prerequisiti

1. Installare Marketo Microsoft Dynamics Sync.

   Le informazioni e il plug-in più recente per Microsoft Dynamics Sync sono disponibili [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html)

1. Installare [Adobe Sign per Microsoft Dynamics](https://appsource.microsoft.com/it-it/product/dynamics-365/adobesign.f3b856fc-a427-4d47-ad4b-d5d1baba6f86).

   Le informazioni sul plug-in sono disponibili [qui.](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)

## Trova l&#39;oggetto personalizzato

Una volta completate le configurazioni Marketo Microsoft Dynamics Sync e Adobe Sign for Dynamics, vengono visualizzate due nuove opzioni nel terminale Amministrazione Marketo.

![Ammin.](assets/adminTerminal.png)

1. Fare clic su **[!UICONTROL Sincronizza entità Dynamics]**.

   La sincronizzazione deve essere disattivata prima di sincronizzare le entità personalizzate. Fare clic su **Sincronizza schema** se questa è la prima volta. In caso contrario, fare clic su **Aggiorna schema**.

   ![Aggiorna](assets/refreshSchema.png)

## Sincronizza l&#39;oggetto personalizzato

1. Sul lato destro, individuare gli oggetti personalizzati basati su [!UICONTROL Lead], [!UICONTROL Contact] e [!UICONTROL Account].

   * **Attivare** Syncfor per gli oggetti sotto  **** Leadif se si desidera inviare un promemoria quando un   Leadha non ha firmato un accordo in Dynamics.

   * **Attivare** Syncfor per gli oggetti in  **** Contatto se si desidera inviare un promemoria quando un   Contacthas non ha firmato un accordo in Dynamics.

   * **Attivare** Syncfor per gli oggetti in  **** Account per inviare un promemoria quando un   Account non ha firmato un accordo in Dynamics.

   * **Abilitare** Syncfor per l&#39;oggetto contratto in corrispondenza dell&#39;oggetto  **[!UICONTROL Parent]**  desiderato ([!UICONTROL Lead],  [!UICONTROL Contact] o  [!UICONTROL Account]).

   ![Oggetti personalizzati](assets/enableSyncDynamics.png)

1. Nella nuova finestra, selezionare le proprietà desiderate nell&#39;ambito del contratto, quindi abilitare le caselle in **Vincolo** e **Trigger** per esporle alle attività di marketing.

   ![Sincronizzazione personalizzata 1](assets/entitySync1.png)

   ![Sincronizzazione personalizzata 2](assets/entitySync2.png)

1. Riattiva la sincronizzazione dopo aver attivato la sincronizzazione sugli oggetti personalizzati.

   Tornare al terminale di amministrazione, fare clic su **Microsoft Dynamics**, quindi fare clic su **Abilita sincronizzazione**.

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![Abilita globale](assets/enableGlobalDynamics.png)

## Crea programma e token

1. Nella sezione Attività di marketing di Marketo fare clic con il pulsante destro del mouse su **Attività di marketing** sulla barra a sinistra.

   Selezionare **Nuova cartella campagna** e assegnargli un nome.

   ![Nuova cartella](assets/newFolder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella creata, selezionare **Nuovo programma** e assegnargli un nome.

   Lasciare tutto il resto come predefinito, quindi fare clic su **Crea**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

1. Fare clic su **My Tokens**, quindi trascinare **Email Script** nell&#39;area di lavoro.

   ![Script di posta elettronica](assets/emailScript.png)

1. Assegnare un nome, quindi fare clic su **Fare clic per modificare**.

   ![Nome e modifica](assets/nameAndSave.png)

1. Espandere **[!UICONTROL Oggetti personalizzati]** sulla destra, quindi espandere l&#39;oggetto **[!UICONTROL Agreement]**.

   Trova e trascina [!UICONTROL Nome], Stato contratto, Inviato e URL firmatario corrente sull&#39;area di lavoro.

1. Scrivere uno script Velocity utilizzando i token seguenti per visualizzare l&#39;URL del contratto di un contratto che non viene firmato per una settimana. Di seguito è riportato un esempio che confronta la data corrente con Inviato On:

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

1. Fare clic su **[!UICONTROL Salva]**.

## Crea promemoria e aggiungi personalizzazione

Esempi di personalizzazione: il nome del firmatario, il nome dell&#39;accordo, un collegamento all&#39;accordo, ecc.

1. Fare clic con il pulsante destro del mouse sul programma creato e scegliere **[!UICONTROL Nuovo asset locale]**, quindi selezionare **[!UICONTROL Email]**.

   ![Nuovo messaggio di posta elettronica](assets/createNewEmail.png)

1. Nella nuova scheda immettere un **[!UICONTROL Nome]** e **[!UICONTROL Descrizione]** per l&#39;e-mail e selezionare un modello dal selettore modelli.

   ![Selezione modello](assets/templatePicker.png)

1. Fare clic su **[!UICONTROL Crea]**.

1. Impostare **[!UICONTROL Da nome]** e **[!UICONTROL Da indirizzo]**.

   ![Promemoria e-mail](assets/reminderEmail.png)

1. Fare clic sul corpo del messaggio per attivare l&#39;editor.

   Fare clic sul pulsante **[!UICONTROL Inserisci token]**, trovare il token URL del contratto personalizzato creato, quindi fare clic su **[!UICONTROL Inserisci]**. Completate la personalizzazione dei messaggi e-mail e fate clic su **[!UICONTROL Salva]**.

   ![Inserisci token](assets/insertToken.png)

1. Visualizza l&#39;anteprima utilizzando un profilo a cui è stato assegnato un contratto.

   È necessario visualizzare un collegamento all&#39;URL con il nome del contratto come etichetta.

   ![Invia collegamento per e-mail](assets/emailLink.png)

## Imposta filtro Smart Campaign

1. Fare clic con il pulsante destro del mouse sul programma creato, quindi scegliere **[!UICONTROL Nuova campagna]**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegnare un nome alla scelta, quindi fare clic su **[!UICONTROL Crea]**.

   ![Smart Campaign 2](assets/smartCampaign2.png)

1. Cerca, quindi fai clic e trascina **[!UICONTROL Contratto]** nell&#39;elenco smart.

   ![Ha stipulato un accordo](assets/hasAgreementDynamics1.png)

   I campi esposti al trigger devono essere disponibili in **[!UICONTROL Aggiungi vincolo]**.

1. Selezionare **[!UICONTROL Stato contratto]** e tutti gli altri campi per i quali si desidera filtrare.

   Per ogni campo aggiunto, definire i valori per cui filtrare. In questo caso, si attiva solo quando **[!UICONTROL Stato contratto]** è *Fuori per firma* e **[!UICONTROL Inviato On]** è *passato prima di una settimana*.

   ![Stato dell’accordo](assets/hasAgreementDynaSentOn.png)

   >[!NOTE]
   >
   > Aggiungere un identificatore univoco ai vincoli, ad esempio **Nome**, se si desidera che la campagna venga eseguita solo per determinati accordi.

1. Confermare il pubblico della campagna e vedere chi si qualificherà nella scheda Pianificazione.

   ![Qualificatori](assets/qualifiers.png)

## Imposta il flusso di Smart Campaign

Poiché è stato utilizzato il filtro campagna **Giorni fino a scadenza**, è possibile utilizzare una ricorrenza pianificata per la campagna.

1. Fare clic sulla scheda **[!UICONTROL Flusso]** nella [!UICONTROL Campagna intelligente].

   Cercare e trascinare il flusso **Invia e-mail** nell&#39;area di lavoro e selezionare il promemoria creato nella sezione precedente.

   ![Invia messaggio di posta elettronica](assets/sendEmail.png)

1. Fare clic sulla scheda **[!UICONTROL Pianificazione]** nella Smart Campaign. Verificare che il flusso della campagna sia limitato all&#39;esecuzione di una sola volta per persona nelle **Impostazioni campagna Smart**. Quindi, fare clic sulla scheda **Programma ricorrenza**.

   ![Scheda Pianificazione](assets/scheduleTab.png)

1. Impostare **Schedule** su _Daily_. Scegliere un giorno e un&#39;ora di inizio e una data di fine per la campagna, se necessario.

   ![Impostazioni pianificazione](assets/scheduleSettings.png)

>[!TIP]
>
>Questa esercitazione fa parte del corso [Accelerare i cicli di vendita con Adobe Sign per Microsoft Dynamics e Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) disponibile gratuitamente su Experience League!