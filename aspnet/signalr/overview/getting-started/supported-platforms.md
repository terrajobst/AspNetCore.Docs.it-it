---
uid: signalr/overview/getting-started/supported-platforms
title: Piattaforme supportate | Microsoft Docs
author: pfletcher
description: Questo articolo descrive quali i client e server sono supportati da SignalR.
ms.author: riande
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: d522602c3523d97a12c74b2d901391bd00d4f2b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828097"
---
<a name="supported-platforms"></a>Piattaforme supportate
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> Questo articolo descrive quali i client e server sono supportati da SignalR. 
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


SignalR è supportato in un'ampia gamma di configurazioni di client e server. Inoltre, ogni opzione di trasporto dispone di un set di requisiti propri; Se i requisiti di sistema per un trasporto non sono disponibili, SignalR effettuerà correttamente il failover altri trasporti. Per altre informazioni su trasporti che SignalR supporta, vedere [trasporti e i fallback](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Requisiti di sistema di server

Il componente server di SignalR può essere ospitato in un'ampia gamma di configurazioni del server. Questa sezione descrive le versioni supportate di sistemi operativi .NET framework, Internet Information Server e altri componenti.

### <a name="supported-server-operating-systems"></a>Sistemi operativi server supportati

Il componente server di SignalR può essere ospitato nei seguenti sistemi operativi client o server. Si noti che per SignalR usare i WebSocket, Windows Server 2012, Windows Server 2016 o Windows 8 è obbligatorio (WebSocket può essere usato nei siti Web di Microsoft Azure, purché versione del sito di .NET framework è impostata su 4.5 e Web socket è abilitata del sito Pagina di configurazione).

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Windows Azure

### <a name="supported-server-net-framework-version"></a>Versione di .NET Framework server supportati

SignalR 2 è supportato solo in .NET Framework 4.5. Vedere le [degli aggiornamenti consigliati](#updates) sezione per gli aggiornamenti che migliorano l'affidabilità, compatibilità, stabilità e prestazioni.

### <a name="supported-server-iis-versions"></a>Versioni di IIS server supportati

Quando si SignalR è ospitato in IIS, sono supportate le versioni seguenti. Si noti che se viene usato un sistema operativo client, ad esempio per lo sviluppo di Windows 8 o Windows 7, le versioni complete di IIS o Cassini non usare, perché sarà presente un limite di 10 connessioni simultanee imposte, di cui verrà raggiunto molto velocemente poiché le connessioni temporanei, spesso ristabilita e sono non vengono eliminati immediatamente al momento non è più in uso. IIS Express deve essere utilizzato nei sistemi operativi client.

Si noti inoltre che per SignalR usare WebSocket, 8, IIS o IIS Express 8 deve essere usato, il server deve utilizzare Windows 8, Windows Server 2012 o versioni successive, e WebSocket deve essere abilitato in IIS. Per informazioni su come abilitare i WebSocket in IIS, vedere [supporto del protocollo WebSocket IIS 8.0](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 o 8 IIS Express.
- IIS 7 e 7.5. Supporto per [URL senza estensione](https://support.microsoft.com/kb/980368) è obbligatorio.
- IIS deve essere in esecuzione in modalità integrata. non è supportata in modalità classica. Messaggio di fino a 30 secondi possono verificare ritardi se IIS viene eseguito in modalità classica mediante il trasporto di eventi Server-Sent.
- L'applicazione host deve essere in esecuzione in modalità di attendibilità totale.

## <a name="client-system-requirements"></a>Requisiti di sistema client

SignalR è utilizzabile in un'ampia gamma di piattaforme client. In questa sezione vengono descritti i requisiti di sistema per l'uso di SignalR in web browser, applicazioni desktop di Windows, le applicazioni Silverlight e dispositivi mobili.

### <a name="web-browsers"></a>Web browser

SignalR è utilizzabile in un'ampia gamma di browser web, ma in genere, sono supportate solo le ultime due versioni.

Le applicazioni che usano SignalR nei browser devono usare la versione di jQuery 1.6.4 o versioni successive principali (ad esempio 1.7.2, 1.8.2 o 1.9.1).

SignalR è utilizzabile nei browser seguenti:

- Versioni di Microsoft Internet Explorer 8, 9, 10 e 11. Moderno, Desktop e le versioni mobili sono supportati.
- Mozilla Firefox: versione corrente - 1, versioni di Windows e Mac.
- Google Chrome: versione corrente - 1, versioni di Windows e Mac.
- Safari: versione corrente - 1, versioni di iOS e Mac.
- Opera: versione corrente - 1, solo Windows.
- Browser Android

Oltre a richiedere determinati browser, i vari trasporti che usa SignalR hanno requisiti di proprie. I trasporti seguenti sono supportati con le configurazioni seguenti:

<a id="browser"></a>

**Requisiti del Web Browser trasporto**

| Trasporto | Internet Explorer | Chrome (Windows o iOS) | Firefox | Safari (OSX o iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| Oggetti WebSocket | 10+ | Current - 1 | Current - 1 | Current - 1 | N/D |
| Eventi inviati al server | N/D | Current - 1 | Current - 1 | Current - 1 | N/D |
| ForeverFrame | 8+ | N/D | N/D | N/D | 4.1 |
| Polling di lunga durata | 8+ | Current - 1 | Current - 1 | Current - 1 | 4.1 |

\*: 6 + richiesto per la funzionalità completa.

#### <a name="unsupported-browsers"></a>Browser non supportati

Mentre SignalR *potrebbe* eseguire senza problemi di rilievo nelle versioni di browser precedenti, abbiamo non testare attivamente SignalR in essi contenuti e non è in genere correggere i bug che possono essere visualizzati in essi contenuti.

### <a name="windows-desktop-and-silverlight-applications"></a>Windows Desktop e applicazioni di Silverlight

Oltre a essere eseguito in un web browser, SignalR possono essere ospitati in client di Windows autonomo o le applicazioni Silverlight. Le applicazioni Windows Desktop e Silverlight SignalR hanno i seguenti requisiti di sistema.

- Applicazioni che utilizzano .NET 4 sono supportati in Windows XP SP3 o versioni successive.
- Applicazioni che utilizzano .NET Framework 4.5 sono supportati in Windows Vista o versioni successive.

Oltre a sistema operativo e requisiti di .NET framework, i trasporti disponibili per SignalR hanno i propri requisiti. I trasporti seguenti sono supportati con le configurazioni seguenti:

**Requisiti di trasporto di Silverlight e Desktop Windows**

| Trasporto | Applicazione .NET | Silverlight |
| --- | --- | --- |
| Web socket | Windows 8 e versioni successive e .NET 4.5 e versioni successive | N/D |
| Forever Frame | N/D | N/D |
| Eventi inviati al server | .NET 4+ | 5+ |
| Polling di lunga durata | .NET 4+ | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Applicazioni di Windows Phone e Windows Store

SignalR è utilizzabile in applicazioni Windows Store e Windows Phone 8. I trasporti seguenti sono supportati con le configurazioni seguenti:

**Requisiti di trasporto di Windows Phone e Windows Store**

| Trasporto | Windows Store / .NET | Windows Store / JavaScript | Windows Phone Internet Explorer | Windows Phone / .NET |
| --- | --- | --- | --- | --- |
| Oggetti WebSocket | N/D | Win8 + | 8+ | N/D |
| Forever Frame | N/D | Win8 + | 7.5+ | N/D |
| Eventi inviati al server | Win8 + | N/D | N/D | 8+ |
| Polling di lunga durata | Win8 + | Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Aggiornamenti consigliati

Gli aggiornamenti seguenti sono consigliati per i server di SignalR:

- È disponibile un aggiornamento per .NET Framework 4.5 [qui](https://support.microsoft.com/kb/2750149).
- Microsoft rilascerà periodicamente QFE per ASP.NET. Questi devono essere applicati come disponibili.
