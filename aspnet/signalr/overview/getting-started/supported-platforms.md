---
uid: signalr/overview/getting-started/supported-platforms
title: Piattaforme supportate | Documenti Microsoft
author: pfletcher
description: In questo articolo descrive i client e server supportate da SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 1379b9fb638f67896d88d7aa4312d95280ef7318
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
<a name="supported-platforms"></a>Piattaforme supportate
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> In questo articolo descrive i client e server supportate da SignalR. 
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


SignalR è supportato in diverse configurazioni di client e server. Inoltre, ogni opzione di trasporto ha un set di requisiti specifici; Se non sono disponibili i requisiti di sistema per un trasporto, SignalR verrà normalmente failover in altri trasporti. Per ulteriori informazioni sui trasporti supportati SignalR, vedere [trasporti e fallback](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Requisiti di sistema di server

Il componente server di SignalR può essere ospitato su una vasta gamma di configurazioni del server. In questa sezione vengono descritte le versioni supportate di sistemi operativi, .NET framework, Internet Information Server e altri componenti.

### <a name="supported-server-operating-systems"></a>Sistemi operativi server supportati

Il componente server di SignalR può essere ospitato nei seguenti sistemi operativi client o server. Si noti che per SignalR usare i WebSocket, Windows Server 2012 o Windows 8 è necessario (WebSocket può essere utilizzato nei siti Web di Windows Azure, purché la versione del sito di .NET framework è impostata su 4.5 e WebSocket è abilitata nella pagina di configurazione del sito).

- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Windows Azure

### <a name="supported-server-net-framework-version"></a>Versione di .NET Framework server supportati

SignalR 2 è supportato solo in .NET Framework 4.5. Vedere il [degli aggiornamenti consigliati](#updates) sezione per gli aggiornamenti che migliorano l'affidabilità, compatibilità, stabilità e prestazioni.

### <a name="supported-server-iis-versions"></a>Versioni di IIS server supportati

SignalR è ospitato in IIS, sono supportate le versioni seguenti. Si noti che se viene utilizzato un sistema operativo client, come per lo sviluppo (Windows 8 o Windows 7), le versioni complete di IIS o Cassini non devono essere utilizzate, poiché sarà presente un limite di 10 connessioni simultanee imposte, di cui verrà raggiunto molto velocemente poiché le connessioni temporaneo, spesso ristabilita e sono non vengono eliminati immediatamente al momento non è più in uso. IIS Express deve essere utilizzato nei sistemi operativi client.

Si noti inoltre che per SignalR utilizzare WebSocket, IIS 8 o IIS Express 8 devono essere utilizzate, deve essere utilizzata dal server Windows 8, Windows Server 2012 o versioni successive, e WebSocket deve essere abilitato in IIS. Per informazioni su come abilitare i WebSocket in IIS, vedere [supporto del protocollo WebSocket IIS 8.0](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 o 8 IIS Express.
- IIS 7 e 7.5. Supporto per [URL senza estensione](https://support.microsoft.com/kb/980368) è obbligatorio.
- IIS deve essere in esecuzione in modalità integrata. non è supportata in modalità classica. Messaggio di fino a 30 secondi possono verificare ritardi se IIS viene eseguito in modalità classica utilizza il trasporto Server-Sent eventi.
- L'applicazione host deve essere in esecuzione in modalità di attendibilità totale.

## <a name="client-system-requirements"></a>Requisiti del sistema client

SignalR può essere utilizzato in una vasta gamma di piattaforme client. In questa sezione vengono descritti i requisiti di sistema per l'utilizzo di SignalR in web browser, applicazioni desktop di Windows, le applicazioni Silverlight e dispositivi mobili.

### <a name="web-browsers"></a>Web browser

SignalR può essere utilizzato in una vasta gamma di browser, ma in genere, sono supportate solo le ultime due versioni.

Le applicazioni che utilizzano SignalR nei browser occorre utilizzare jQuery 1.6.4 o versioni successive principali (ad esempio 1.7.2 1.8.2 o 1.9.1).

SignalR è utilizzabile nei browser seguenti:

- Versioni di Microsoft Internet Explorer 8, 9, 10 e 11. Moderno, Desktop e mobili versioni sono supportati.
- Mozilla Firefox: versione corrente - 1, versioni di Windows e Mac.
- Google Chrome: versione corrente - 1, versioni di Windows e Mac.
- Safari: versione corrente - 1, versioni di iOS e Mac.
- Opera: versione corrente - 1, solo Windows.
- Browser Android

Oltre ai che richiedono alcuni browser, i trasporti diversi utilizzati SignalR hanno requisiti dei propri. I trasporti seguenti sono supportati con le seguenti configurazioni:

<a id="browser"></a>

**Requisiti del Web Browser trasporto**

| Trasporto | Internet Explorer | Chrome (Windows o iOS) | Firefox | Safari (OSX o iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| Oggetti WebSocket | 10+ | corrente - 1 | corrente - 1 | corrente - 1 | N/D |
| Eventi inviati al server | N/D | corrente - 1 | corrente - 1 | corrente - 1 | N/D |
| ForeverFrame | 8+ | N/D | N/D | N/D | 4.1 |
| Polling lungo | 8+ | corrente - 1 | corrente - 1 | corrente - 1 | 4.1 |

\*: 6 + richiesta per la funzionalità completa.

#### <a name="unsupported-browsers"></a>Browser non supportati

Mentre SignalR *potrebbe* eseguite senza problemi principali nelle versioni precedenti di browser, non attivamente testati SignalR in essi contenuti e non è in genere correggere i bug che possono essere visualizzati in essi contenuti.

### <a name="windows-desktop-and-silverlight-applications"></a>Windows Desktop e applicazioni di Silverlight

Oltre a eseguire in un web browser, SignalR possono essere ospitati in client Windows autonomo o applicazioni di Silverlight. Applicazioni Desktop di Windows e Silverlight SignalR presentano i requisiti di sistema seguenti.

- Applicazioni con .NET 4 sono supportate in Windows XP SP3 o versioni successive.
- Applicazioni che usano .NET Framework 4.5 sono supportate in Windows Vista o versioni successive.

Oltre del sistema operativo e i requisiti di .NET framework, i trasporti disponibili per SignalR hanno requisiti dei propri. I trasporti seguenti sono supportati con le seguenti configurazioni:

**Requisiti di trasporto di Silverlight e Windows Desktop**

| Trasporto | Applicazione .NET | Silverlight |
| --- | --- | --- |
| Web socket | Windows 8 e versioni successive e .NET 4.5 + | N/D |
| Forever Frame | N/D | N/D |
| Eventi inviati al server | .NET 4+ | 5+ |
| Polling lungo | .NET 4+ | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows Store e le applicazioni Windows Phone

SignalR può essere utilizzato nelle applicazioni Windows Store e Windows Phone 8. I trasporti seguenti sono supportati con le seguenti configurazioni:

**Windows Store e requisiti di trasporto di Windows Phone**

| Trasporto | Windows Store/ .NET | Windows Store / JavaScript | Windows Phone / IE | Windows Phone/ .NET |
| --- | --- | --- | --- | --- |
| Oggetti WebSocket | N/D | Win8+ | 8+ | N/D |
| Forever Frame | N/D | Win8+ | 7.5+ | N/D |
| Eventi inviati al server | Win8+ | N/D | N/D | 8+ |
| Polling lungo | Win8+ | Win8+ | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Aggiornamenti consigliati

Gli aggiornamenti seguenti sono consigliati per i server di SignalR:

- È disponibile un aggiornamento per .NET Framework 4.5 [qui](https://support.microsoft.com/kb/2750149).
- Microsoft rilascerà periodicamente QFE per ASP.NET. Questi devono essere applicati come disponibili.
