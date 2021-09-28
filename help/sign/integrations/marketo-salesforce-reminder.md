---
title: Invia promemoria utilizzando Adobe Sign for Salesforce e Marketo Configuration Guide
description: Informazioni su come inviare un promemoria tramite posta elettronica da Marketo quando un accordo rimane privo di firma dopo un periodo di tempo
role: Admin
product: adobe sign
solution: Adobe Sign, Marketo, Document Cloud
level: Intermediate
topic-revisit: Integrations
thumbnail: KT-7248.jpg
exl-id: 33aca2e0-2f27-4100-a16f-85ba652c17a3
source-git-commit: bc79bbde966c99bdf32231ff073b49cfc759d928
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 1%

---

# Invia promemoria utilizzando Adobe Sign for Salesforce e Marketo Configuration Guide

Informazioni su come inviare un promemoria via e-mail da Marketo quando un accordo rimane privo di firma dopo un periodo di tempo. Questa integrazione utilizza Adobe Sign, Adobe Sign per Salesforce, Marketo e Marketo e Salesforce Sync.

## Prerequisiti

1. Installare Marketo Salesforce Sync.

   Le informazioni e il plug-in più recente per Salesforce Sync sono disponibili [qui.](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/understanding-the-salesforce-sync.html)

1. Installare Adobe Sign per Salesforce.

   Le informazioni sul plug-in sono disponibili [qui.](https://helpx.adobe.com/ca/sign/using/salesforce-integration-installation-guide.html)

## Trova l&#39;oggetto personalizzato

Al termine delle configurazioni Marketo Salesforce Sync e Adobe Sign for Salesforce, vengono visualizzate diverse nuove opzioni nel terminale Marketo Admin.

![Ammin.](assets/adminTab.png)

![Sincronizzazione oggetti](assets/salesforceAdmin.png)

1. Fare clic su **Sincronizza schema** se questa è la prima volta. In caso contrario, fare clic su **Aggiorna schema**.

   ![Aggiorna](assets/refreshSchema1.png)

1. Se la sincronizzazione globale è in esecuzione, disattivare facendo clic su **Disattiva sincronizzazione globale**.

   ![Disabilita](assets/disableGlobal.png)

1. Fare clic su **Aggiorna schema**.

   ![Aggiorna 2](assets/refreshSchema2.png)

## Sincronizza l&#39;oggetto personalizzato

Sul lato destro, vedere Oggetti personalizzati basati su Lead, Contact e Account.

**Abilitare** Syncfor per gli oggetti in Lead se si desidera inviare un promemoria quando un lead non ha firmato un contratto in Salesforce.

**Abilitare** Syncfor per gli oggetti in Contatto se si desidera inviare un promemoria quando un contatto non ha firmato un accordo in Salesforce.

**Abilitare** Syncfor per gli oggetti in Account se si desidera inviare un promemoria quando un Account non ha firmato un contratto in Salesforce.

1. **Abilitare** Syncfor per l&#39;oggetto  **** Agreement visualizzato nell&#39;elemento padre desiderato (Lead, Contact o Account). Eseguire questa operazione per qualsiasi altro oggetto personalizzato che si desidera sincronizzare.

   ![Oggetto contratto](assets/agreementObject.png)

1. Nelle seguenti risorse viene illustrato come attivare **Sincronizzazione**.

   ![Sincronizzazione personalizzata 1](assets/customObjectSync1.png)

   ![Sincronizzazione personalizzata 2](assets/customObjectSync2.png)

## Esporre i campi dell&#39;oggetto personalizzato ai trigger

1. Mentre la Sincronizzazione globale è disattivata, selezionare l&#39;oggetto personalizzato Contratto per il quale è stata abilitata la sincronizzazione, quindi **Modifica campi visibili**.

1. Selezionare il campo &quot;Nome contratto&quot; nella colonna di trigger per esporlo ai trigger di azione campagna. Selezionare tutti gli altri campi per i quali si desidera filtrare, quindi **Salva**.

   ![Modifica campi visibili 1](assets/editVisible1.png)

   ![Modifica campi visibili 2](assets/editVisible2.png)

1. Al termine dell&#39;abilitazione della sincronizzazione sugli oggetti personalizzati e dell&#39;esposizione dei valori di trigger, ricordatevi di riattivare la sincronizzazione:

   ![Abilita globale](assets/enableGlobal.png)

## Crea programma e token

1. Nella sezione Attività di marketing di Marketo, fare clic con il pulsante destro del mouse su **Attività di marketing** sulla barra a sinistra, selezionare **Nuova cartella campagna** e dargli un nome.

   ![Nuova cartella](assets/newFolder.png)

1. Fare clic con il pulsante destro del mouse sulla cartella creata, selezionare **Nuovo programma** e assegnargli un nome. Lasciare tutto il resto come predefinito, quindi fare clic su **Crea**.

   ![Nuovo programma 1](assets/newProgram1.png)

   ![Nuovo programma 2](assets/newProgram2.png)

1. Fare clic su **My Tokens**, quindi trascinare **Email Script** nell&#39;area di lavoro.

   ![Script di posta elettronica](assets/emailScript.png)

1. Assegnare un nome, quindi fare clic su **Fare clic per modificare**.

   ![Nome e modifica](assets/nameAndSave.png)

1. Espandere **Oggetti personalizzati** sulla destra, quindi espandere l&#39;oggetto **Agreement**. Trova e trascina l&#39;URL Nome contratto, Stato contratto, Data firma e Firma nell&#39;area di lavoro.

1. Scrivere uno script Velocity utilizzando i token seguenti per visualizzare l&#39;URL del contratto di un contratto che non viene firmato per una settimana. Di seguito è riportato un esempio che confronta la data corrente con la data di invio:

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

1. Fare clic su **Salva**.

## Crea promemoria e aggiungi personalizzazione

Esempi di personalizzazione: il nome del firmatario, il nome dell&#39;accordo, un collegamento all&#39;accordo, ecc.

1. Fare clic con il pulsante destro del mouse sul programma creato e scegliere **Nuovo asset locale**, quindi selezionare **Email**.

   ![Nuovo messaggio di posta elettronica](assets/createNewEmail.png)

1. Nella nuova scheda immettere un **Nome** e **Descrizione** per l&#39;e-mail e selezionare un modello dal selettore modelli. Fare clic su **Crea**.

   ![Selezione modello](assets/templatePicker.png)

1. Impostare **Da nome** e **Da indirizzo**.

   ![Promemoria e-mail](assets/reminderEmail.png)

1. Fare clic sul corpo del messaggio per attivare l&#39;editor. Fare clic sul pulsante **Inserisci token**, trovare il token URL del contratto personalizzato creato, quindi fare clic su **Inserisci**. Completate la personalizzazione dei messaggi e-mail e fate clic su **Salva**.

   ![Inserisci token](assets/insertToken.png)

1. Visualizza l&#39;anteprima utilizzando un profilo a cui è stato assegnato un contratto. È necessario visualizzare un collegamento all&#39;URL con il nome del contratto come etichetta.

   ![Invia collegamento per e-mail](assets/emailLink.png)

## Imposta filtro Smart Campaign

1. Fare clic con il pulsante destro del mouse sul programma creato, quindi scegliere **Nuova campagna**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. Assegnare un nome alla scelta, quindi fare clic su **Crea**.

   ![Smart Campaign 2](assets/smartCampaign2.png)

1. Cerca, quindi fai clic e trascina **Contratto** nell&#39;elenco smart.

   ![Ha stipulato un accordo](assets/hasAgreement.png)

1. I campi esposti al trigger devono ora essere disponibili in **Aggiungi vincolo**. Selezionare **Stato contratto** e tutti gli altri campi per i quali si desidera filtrare. Per ogni campo aggiunto, definire i valori per cui filtrare. In questo caso, attiverà solo quando **Stato contratto** è in attesa di firma e **Data di invio** è in precedenza prima di 7 giorni.

   ![Stato dell’accordo](assets/agreementStatus.png)

   >[!NOTE]
   >
   > d un identificatore univoco dei vincoli, ad esempio **Nome contratto**, se si desidera che la campagna venga eseguita solo per determinati accordi.

1. Confermare il pubblico della campagna e vedere chi si qualificherà nella scheda Pianificazione.

   ![Qualificatori](assets/qualifiers.png)

## Imposta il flusso di Smart Campaign

Poiché è stato utilizzato il filtro campagna **Giorni non firmati**, è possibile utilizzare una ricorrenza pianificata per la campagna.

1. Fare clic sulla scheda **Flusso** nella campagna Smart. Cercare e trascinare il flusso **Invia e-mail** nell&#39;area di lavoro e selezionare il promemoria creato nella sezione precedente.

   ![Invia messaggio di posta elettronica](assets/sendEmail.png)

1. Fare clic sulla scheda **Pianificazione** nella campagna Smart. Verificare che il flusso della campagna sia limitato all&#39;esecuzione di una sola volta per persona nelle **Impostazioni campagna Smart**. Quindi, fare clic sulla scheda **Programma ricorrenza**.

   ![Scheda Pianificazione](assets/scheduleTab.png)

1. Impostare **Schedule** su Daily, scegliere un giorno e un&#39;ora di inizio e una data di fine per la campagna, se necessario.

   ![Impostazioni pianificazione](assets/scheduleSettings.png)

>[!TIP]
>
>Questa esercitazione fa parte del corso [Accelerare i cicli di vendita con Adobe Sign for Salesforce e Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1), disponibile gratuitamente con Experience League!
