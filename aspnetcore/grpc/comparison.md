---
title: Confronto tra servizi gRPC e API HTTP
author: jamesnk
description: Informazioni su come gRPC viene confrontato con HTTP APIs e ciò che è consigliabile sono gli scenari.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 03/31/2019
uid: grpc/comparison
ms.openlocfilehash: 655c921788deb30f3c0f3b47f4440dc8701c0f59
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874930"
---
# <a name="comparing-grpc-services-with-http-apis"></a>Confronto tra servizi gRPC e API HTTP

Da [James metodo Newton-King](https://twitter.com/jamesnk)

Questo articolo illustra come [services gRPC](https://grpc.io/docs/guides/) confrontare a APIs HTTP (tra cui ASP.NET Core [le API Web](xref:web-api/index)). La tecnologia usata per fornire un'API per l'app è una scelta importante e gRPC offre vantaggi esclusivi rispetto a APIs HTTP. Questo articolo illustra i punti di forza e punti deboli di gRPC e propone scenari di utilizzo gRPC su altre tecnologie.

#### <a name="overview"></a>Panoramica

|    Funzionalità             |    gRPC                                                 |    API HTTP con JSON                       |
|------------------------|---------------------------------------------------------|----------------------------------------------|
|    Contratto            |    Obbligatorio (`*.proto`)                                 |    Facoltativo (OpenAPI)                        |
|    Trasporto           |    HTTP/2                                               |    HTTP                                      |
|    Payload             |    [Protobuf (small, binaria)](#performance)             |    JSON (grandi, leggibili)              |
|    Prescriptiveness    |    [Specifica del tipo Strict](#strict-specification)        |    File separati. Qualsiasi HTTP è valida                  |
|    Flusso           |    [Client, server, bidirezionali](#streaming)         |    Il client e server                            |
|    Supporto browser     |    [No (richiede grpc web)](#limited-browser-support)   |    Yes                                       |
|    Sicurezza            |    Transport (HTTPS)                                    |    Transport (HTTPS)                         |
|    Generazione di codice client     |    [Sì](#code-generation)                              |    Strumenti di terze parti e OpenAPI             |

## <a name="grpc-strengths"></a>punti di forza gRPC

### <a name="performance"></a>Prestazioni

i messaggi gRPC vengono serializzati usando la [Protobuf](https://developers.google.com/protocol-buffers/docs/overview), un formato di messaggi binario efficiente. Protobuf serializza molto rapidamente nel server e client. I risultati di serializzazione Protobuf nel payload di messaggi di piccole dimensioni, importante negli scenari di larghezza di banda limitata, ad esempio App per dispositivi mobili.

gRPC è progettato per HTTP/2, una revisione principale del protocollo HTTP che offre vantaggi significativi delle prestazioni su HTTP 1.x:

* Frame binario e compressione. Protocollo HTTP/2 è compatto ed efficiente sia in invio e ricezione.
* Multiplexing di più chiamate HTTP/2 su una singola connessione TCP. Elimina il multiplexing [blocco head-of-line](https://en.wikipedia.org/wiki/Head-of-line_blocking).

### <a name="code-generation"></a>Generazione del codice

Tutti i framework gRPC forniscono un supporto eccellente per la generazione di codice. Un file di base per lo sviluppo gRPC è il [ `*.proto` file](https://developers.google.com/protocol-buffers/docs/proto3), che definisce il contratto dei messaggi e servizi gRPC. Da questo Framework gRPC file codice genererà una classe di base del servizio, messaggi e un client completo.

Condividendo le `*.proto` file tra server e client, i messaggi e il codice client può essere generato dal finale alla fine. Generazione del codice del client consente di eliminare la duplicazione dei messaggi nel client e server e crea un client fortemente tipizzato per l'utente. Non dover scrivere un client consente di risparmiare tempo significativo sullo sviluppo di applicazioni con molti servizi.

### <a name="strict-specification"></a>Specifica del tipo Strict

Non esiste una specifica formale per le API di HTTP con JSON. Gli sviluppatori perdurano il migliore formato degli URL, verbi HTTP e i codici di risposta.

Il [gRPC specifica](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) rigida sul formato di un servizio gRPC deve seguire. gRPC Elimina dibattito e consente di risparmiare tempo per gli sviluppatori poiché gPRC coerenza finale tra più piattaforme e le implementazioni.

### <a name="streaming"></a>Flusso

HTTP/2 offre una base per i flussi di comunicazione di lunga durata, in tempo reale. gRPC fornisce un supporto eccellente per lo streaming tramite HTTP/2.

Un servizio gRPC supporta tutte le combinazioni di streaming:

* Unario (nessun streaming)
* Server a client di streaming
* Client di streaming server
* Bidirezionale di streaming

### <a name="deadlinetimeouts-and-cancellation"></a>Scadenza/i di timeout e l'annullamento

gRPC consente ai client di specificare quanto tempo sono disposti ad attendere perché una chiamata RPC per il completamento. Il [scadenza](https://grpc.io/blog/deadlines) viene inviato al server, e il server può decidere l'azione da intraprendere se supera la data di scadenza. Ad esempio, il server può annullare le richieste di gRPC/HTTP/database in corso in caso di timeout.

La propagazione di scadenza e l'annullamento tramite figlio gRPC chiamate consente di applicare limiti di utilizzo delle risorse.

## <a name="grpc-recommended-scenarios"></a>gRPC consigliati gli scenari

gRPC è particolarmente adatto agli scenari seguenti:

* **I Microservizi** &ndash; gRPC è progettato per una latenza bassa e comunicazioni a velocità elevata. gRPC è ideale per i microservizi leggeri in cui l'efficienza è fondamentale.
* **Comunicazione in tempo reale da punto a punto** &ndash; gRPC ha un supporto eccellente per la trasmissione bidirezionali. gRPC servizi possono push in tempo reale senza il polling dei messaggi.
* **Gli ambienti Polygot** &ndash; gRPC strumenti supporta tutti i linguaggi di sviluppo più diffusi, rendendo la scelta ideale per ambienti multilingue gRPC.
* **Rete ambienti vincolati** &ndash; gRPC messaggi serializzati con Protobuf, un formato di messaggi leggero. Un messaggio gRPC è sempre minore di un equivalente messaggio JSON.

## <a name="grpc-weaknesses"></a>punti di debolezza gRPC

### <a name="limited-browser-support"></a>Supporto browser limitato

Non è possibile chiamare direttamente un servizio gRPC da un browser oggi stesso. gRPC utilizza ampiamente le funzionalità HTTP/2 e Nessun browser fornisce il livello di controllo indispensabili durante le richieste web per supportare un client gRPC. Ad esempio, i browser non consentono un chiamante richiede l'utilizzo di HTTP/2 o forniscono l'accesso a frame HTTP/2 sottostanti.

[gRPC Web](https://grpc.io/docs/tutorials/basic/web.html) è una tecnologia aggiuntiva dal team di gRPC che offre un supporto limitato gRPC nel browser. gRPC Web è costituita da due parti: un client JavaScript che supporta tutti i browser moderni e un proxy Web gRPC nel server. Il client gRPC Web chiama il proxy e inoltrerà il proxy per le richieste gRPC al server gRPC.

Non tutte le funzionalità del gRPC sono supportate da gRPC-Web. È disponibile un supporto limitato per lo streaming server e client e il flusso bidirezionale non è supportato.

### <a name="not-human-readable"></a>Non leggibili

Le richieste di API HTTP vengono inviate come testo e possono essere letto e create dagli utenti.

gRPC messaggi vengono codificati con Protobuf per impostazione predefinita. Sebbene Protobuf è efficace per inviare e ricevere, formato binario non umano leggibile. Protobuf richiede specificato nella descrizione dell'interfaccia del messaggio di `*.proto` file da deserializzare correttamente. Strumenti aggiuntivi sono necessario per analizzare Protobuf payload in transito e comporre le richieste a mano.

Funzionalità, ad esempio [reflection server](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) e il [strumento da riga di comando gRPC](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md) esiste per facilitare i messaggi binari Protobuf. Inoltre, i messaggi di supporto Protobuf [conversione in e da JSON](https://developers.google.com/protocol-buffers/docs/proto3#json). La conversione JSON incorporata fornisce un modo efficiente per convertire i messaggi Protobuf da e verso forma leggibile durante il debug.

## <a name="alternative-framework-scenarios"></a>Scenari alternativi framework

Sono consigliati altri Framework su gRPC negli scenari seguenti:

* **Browser API accessibili** &ndash; gRPC non è completamente supportato nel browser. gRPC Web può offrire supporto del browser, ma ha limitazioni e introduce un proxy server.
* **Trasmissione di comunicazione in tempo reale** &ndash; gRPC supporta le comunicazioni in tempo reale tramite streaming, ma non esiste il concetto di trasmettere un messaggio in uscita per connessioni registrate. Ad esempio in uno scenario di chat room dove inviare nuovi messaggi di chat a tutti i client nella chat room, ogni chiamata gRPC deve trasmettere singolarmente i messaggi di chat di nuovo al client. [SignalR](xref:signalr/introduction) è un framework utile per questo scenario. SignalR è presente il concetto di connessioni persistenti e il supporto incorporato per la trasmissione di messaggi.
* **Comunicazione tra processi** &ndash; un processo deve ospitare un server HTTP/2 per accettare le chiamate in ingresso gRPC. Per Windows, comunicazione tra processi [pipe](/dotnet/standard/io/pipe-operations) è un metodo veloce e leggero di comunicazione.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
