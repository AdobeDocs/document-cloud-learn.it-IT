---
title: Aggiornamenti importanti dei prodotti Acrobat DC per i clienti ETLA
description: Scopri le importanti modifiche alle autorizzazioni Acrobat DC incluse nelle offerte ETLA (Enterprise Term License Agreement) a partire da agosto 2020 fino al 20 novembre 2020
role: Admin
level: Intermediate
thumbnail: KT-7269.jpg
jira: KT-7269
exl-id: 1a8d3f7d-96a4-4811-b4e9-9c55287b92ea
source-git-commit: ad54f7afa78b0fbb31eccf455723a8890cb92355
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 6%

---

# Aggiornamenti importanti dei prodotti Acrobat DC per i clienti ETLA

[!DNL Adobe Sign Individual] (noto anche come Adobe Sign Pro) verrà deprestato da tutti i diritti Acrobat DC inclusi in ETLA (Enterprise Term License Agreement) a partire da agosto 2020 e continuerà fino al 20 novembre 2020. [!DNL Adobe Sign Individual] non fornisce funzionalità di livello enterprise e deve essere sostituito con i clienti Adobe Sign Enterprise for enterprise. Ciò include Acrobat DC concesso in licenza come app autonoma e Acrobat DC concesso in licenza come parte di Creative Cloud for enterprise - Tutte le applicazioni.

Accesso a [!DNL Adobe Sign Individual] è disponibile in Acrobat tramite **Adobe Sign** o il metodo **Fill &amp; Sign** strumento ([Richiedi firme](https://www.adobe.com/it/acrobat/online/request-signature.html){target="_blank"}).

![[!DNL Adobe Sign Individual] accesso in Acrobat DC](../assets/Deploy_SignEntitle1.png)

Se non avete aggiornato Acrobat DC alla versione più recente, lo strumento potrebbe essere etichettato &quot;Send for Signature&quot;.

## Perché stiamo disinstallando questo?

[A ottobre 2018, abbiamo rilasciato un nuovo Acrobat DC](https://news.adobe.com/news/news-details/2018/Adobe-Redefines-What-Is-Possible-With-PDF-With-All-New-Acrobat-DC). Questa ultima versione include nuovi strumenti e funzionalità per lavorare meglio con i PDF su dispositivi mobili, Web e desktop, oltre a nuovi strumenti di collaborazione. In qualità di abbonato ad Acrobat DC, dovresti già avere a disposizione queste straordinarie funzionalità. Un altro importante aggiornamento che abbiamo rilasciato riguarda la nostra soluzione di firma elettronica Adobe Sign.

Prima della versione di ottobre 2018, gli utenti di Acrobat DC sono stati in grado di inviare i documenti per la firma elettronica utilizzando strumenti in Acrobat con etichetta &quot;Fill &amp; Sign&quot; (o &quot;Adobe Sign&quot; o &quot;Send for Signature&quot;) con cui è stato effettuato il provisioning [!DNL Adobe Sign Individual] diritto.

Anche se questa opzione offre un ottimo modo per acquisire le firme elettroniche, stiamo disinstallando il provisioning [!DNL Adobe Sign Individual] poiché non fornisce la funzionalità di livello Enterprise disponibile tramite Adobe Sign Enterprise, ad esempio:

* Possibilità di gestire centralmente gli utenti che dispongono delle autorizzazioni per inviare accordi o firmare
* Consentire agli amministratori di gestire gli accordi inviati e utilizzati nell’organizzazione
* Fornire controlli dettagliati per gestire le firme elettroniche all&#39;interno dell&#39;organizzazione

Inoltre, Adobe Sign Enterprise offre più funzionalità rispetto a quelle disponibili nel [!DNL Adobe Sign Individual] diritto, compreso, a titolo esemplificativo:

* Amministrazione
   * Single Sign-on
   * Delega account
* Integrazioni
   * Integrazioni aziendali predefinite con Dropbox, Salesforce, Workday, ecc.
   * Adobe Sign è la soluzione di firma elettronica preferita in tutti gli [Microsoft](https://acrobat.adobe.com/us/en/business/integrations/microsoft.html) portfolio enterprise, tra cui Office 365, SharePoint, Dynamics, Teams e Flow
* Personalizzazione e ottimizzazione
   * Autenticazione avanzata delle firme elettroniche, verifica avanzata dell’identità dei firmatari basata su ID, designer del flusso di lavoro, supporto linguistico avanzato, ecc.

Adobe Sign è la soluzione leader di settore riconosciuta a livello globale per l&#39;acquisizione di firme conformi agli standard legali. Adobe Sign è stato progettato fin da subito per soddisfare le esigenze di firma elettronica della tua organizzazione, con strumenti IT admin-friendly per garantire che tu e i tuoi utenti utilizziate firme elettroniche che rispettano pienamente le varie normative locali e di settore in materia di firme elettroniche. Visita il sito [qui](https://helpx.adobe.com/it/enterprise/using/adobe-sign-for-enterprise.html) per ulteriori informazioni sulla gestione di Sign-through [Adobe Admin Console](https://helpx.adobe.com/it/enterprise/using/admin-console.html).

Contatta il tuo contatto di Adobe per scoprire come puoi continuare a fornire le funzionalità di firma elettronica della tua organizzazione tramite la nostra piattaforma più ampia per la gestione dei documenti digitali, che include Acrobat DC e Adobe Sign Enterprise.

## Accesso agli accordi esistenti

Gli utenti potranno comunque accedere agli accordi inviati prima di questa azione tramite Adobe Document Cloud accedendo con il proprio Adobe ID all’indirizzo https://documentcloud.adobe.com. Se l’utente è pianificato per la migrazione a Sign Enterprise, dovrà attenersi a queste [disposizioni](https://helpx.adobe.com/it/sign/kb/how-to-download-signed-documents---adobe-sign.html).

## Acrobat DC senza [!DNL Sign Individual] autorizzazione

Gli utenti che dispongono di autorizzazioni Adobe Sign Enterprise potranno inviare accordi in Acrobat utilizzando l’Adobe Sign o [!UICONTROL Fill &amp; Sign] (Richiedi firme) strumento.
Gli utenti che non dispongono delle autorizzazioni Adobe Sign Enterprise non potranno inviare nuovi accordi e riceveranno un messaggio di errore. L&#39;immagine seguente delinea i possibili risultati.

![Messaggio di errore per l’esperienza Acrobat DC](../assets/Deploy_SignEntitle2.png)

## Esperienza Web di Adobe Document Cloud senza autorizzazione Firma individuale

Gli utenti potranno accedere a https://documentcloud.adobe.com/ per accedere e scaricare gli accordi inviati prima di deprovisioning delle autorizzazioni per Adobe Sign Individual.

![Messaggio di errore per l’esperienza Web di Document Cloud](../assets/Deploy_SignEntitle3.png)

## Per ulteriori informazioni, visitare le pagine seguenti:

* [Accesso ad Adobe Document Cloud](https://helpx.adobe.com/document-cloud/help/sign-in.html)
* [Gestione dei file (dove sono i file?)](https://helpx.adobe.com/document-cloud/help/manage-files.html)
* [Utilizzo [!UICONTROL Customization Wizard Acrobat] per la configurazione](https://www.adobe.com/devnet-docs/acrobatetk/tools/Wizard/WizardDC/index.html)
* [Panoramica di [!UICONTROL Admin Console]](https://helpx.adobe.com/it/enterprise/using/admin-console.html)
* [Gestione di Adobe Sign sul [!UICONTROL Admin Console]](https://helpx.adobe.com/it/enterprise/using/adobe-sign-for-enterprise.html)

**Revisioni** 20 maggio 2020; post originale - agosto 2019
