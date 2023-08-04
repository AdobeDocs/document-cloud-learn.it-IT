---
title: Raccogliere documenti con grandi volumi con GigaSign
description: Gigasign consente di inviare, raccogliere e tenere traccia dei documenti da firmare a migliaia di persone allo stesso tempo
feature: Workflow
role: Developer
level: Intermediate
jira: KT-6626
topic-revisit: Integrations
thumbnail: 328113.jpg
exl-id: a59eab61-fe61-45c6-8137-f074e1f2b3ed
source-git-commit: 452299b2b786beab9df7a5019da4f3840d9cdec9
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 2%

---

# Raccogliere documenti con grandi volumi con GigaSign

Gigasign consente di inviare, raccogliere e tenere traccia dei documenti da firmare a migliaia di persone allo stesso tempo. È progettato per comunicazioni di grandi volumi con dipendenti e clienti, supportando fino a 2.500 destinatari per ogni invio in modalità collettiva. GigaSign utilizza l’API di Acrobat Sign per fornire le stesse funzionalità di MegaSign e include il supporto per più firmatari, gruppi di destinatari, ruoli dei destinatari, nomi degli accordi, copie per conoscenza e altro ancora.

>[!VIDEO](https://video.tv.adobe.com/v/328113?quality=12&learn=on&hidetitle=true)

## Scaricare e installare l’app GigaSign

[Scarica il file ZIP di GigaSign](https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:8975dbca-98d5-4e66-9164-d21163c91c7f)

[Collegamento per il download a Java 1.8 (solo se necessario)](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html) {target="_blank"}

[Indirizzi IP alla whitelist (solo se necessario)](https://helpx.adobe.com/it/sign/system-requirements.html#IPs){target="_blank"}

## Istruzioni di configurazione di base

1. Accedi al tuo account Acrobat Sign.

1. Fai clic **[!UICONTROL Raggruppamento]** oppure **[!UICONTROL Account]**, qualsiasi cosa veda in alto.

1. Digita &quot;Token di accesso&quot; nel campo di ricerca sul lato sinistro dello schermo.

1. Premete l’icona &quot;+&quot; a destra.

1. Crea una chiave con gli ambiti necessari (User_Read, Agreement_Read, Agreement_Write, Agreement_Send, Library_Read).

1. Fare doppio clic sul tasto creato e copiare il testo COMPLETO (il tasto non viene visualizzato sullo schermo a destra, quindi assicurarsi di avere tutto).

1. Apri GigaSign.

1. Fare clic sul pulsante **[!UICONTROL Impostazioni]** icona in alto a destra.

1. Incolla la chiave di integrazione nella prima riga.

1. Immetti l’indirizzo e-mail dell’account utilizzato per creare quella chiave nella seconda riga.

1. Fai clic su **[!UICONTROL Invia]**.
