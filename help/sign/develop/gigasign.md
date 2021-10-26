---
title: Raccolta di documenti ad alto volume con GigaSign
description: Gigasign consente di inviare, raccogliere e monitorare i documenti da firmare a migliaia di persone contemporaneamente
role: User, Admin
product: adobe sign
level: Intermediate
topic-revisit: Integrations
thumbnail: 328113.jpg
exl-id: a59eab61-fe61-45c6-8137-f074e1f2b3ed
source-git-commit: 7d82422e442cbbed9420050c30ca70821e9a2cdd
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 5%

---

# Raccolta di documenti ad alto volume con GigaSign

Gigasign consente di inviare, raccogliere e monitorare i documenti da firmare a migliaia di persone contemporaneamente. È progettato per comunicazioni ad elevata capacità con dipendenti e clienti, supportando fino a 2.500 destinatari per ogni invio in blocco. GigaSign utilizza l&#39;API Adobe Sign per fornire la stessa funzionalità di MegaSign e include il supporto per più firmatari, gruppi di destinatari, ruoli dei destinatari, nomi degli accordi, copia delle informazioni e altro ancora.

>[!VIDEO](https://video.tv.adobe.com/v/328113?hidetitle=true)

## Scaricare e installare l’app GigaSign

[Scarica il file zip di GigaSign](https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:8975dbca-98d5-4e66-9164-d21163c91c7f)

[Collegamento per il download di Java 1.8 (solo se necessario)](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html) {target=&quot;_blank&quot;}

[Indirizzi IP alla whitelist (solo se necessario)](https://helpx.adobe.com/it/sign/system-requirements.html#IPs){target=&quot;_blank&quot;}

## Istruzioni di configurazione di base

1. Accedi al tuo account Adobe Sign.

1. Fai clic su **[!UICONTROL Gruppo]** o **[!UICONTROL Account]**, a seconda di quale sia la parte superiore.

1. Digita &quot;Token di accesso&quot; nel campo di ricerca sul lato sinistro dello schermo.

1. Premete l’icona &quot;+&quot; sul lato destro.

1. Crea una chiave con gli ambiti necessari (User_Read, Agreement_Read, Agreement_Write, Agreement_Send, Library_Read).

1. Fai doppio clic sulla chiave che hai creato e copia il testo COMPLETO (si sposta a destra, in modo da ottenere tutto).

1. Apri GigaSign.

1. Fate clic sul **[!UICONTROL Impostazioni]** in alto a destra.

1. Incolla la chiave di integrazione nella prima riga.

1. Immetti l’indirizzo e-mail dell’account utilizzato per creare la chiave nella seconda riga.

1. Fai clic su **[!UICONTROL Invia]**.
