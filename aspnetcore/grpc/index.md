---
title: Introduzione a gRPC su ASP.NET Core
author: juntaoluo
description: Informazioni sui servizi gRPC con il server Kestrel e lo stack di ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
---
# <a name="introduction-to-grpc-on-aspnet-core"></a>Introduzione a gRPC su ASP.NET Core

A cura di [John Luo](https://github.com/juntaoluo)

[gRPC](https://grpc.io/docs/guides/) è un framework RPC indipendente dal linguaggio a elevate prestazioni. Per altre nozioni fondamentali su gRPC, vedere la [pagina della documentazione di gRPC](https://grpc.io/docs/).

I principali vantaggi di gRPC sono:
* Framework RPC leggero e moderno ad alte prestazioni.
* Sviluppo di API con priorità al contratto usando i buffer del protocollo per impostazione predefinita e implementazioni indipendenti dal linguaggio.
* Strumenti disponibili per molte linguaggi consentono di generare client e server fortemente tipizzati.
* Supporto per chiamate client, server e di streaming bidirezionale.
* Utilizzo di rete ridotto grazie alla serializzazione binaria Protobuf.

Questi vantaggi rendono gRPC ideale per:
* Microservizi leggeri in cui l'efficienza è fondamentale.
* Sistemi poliglotti che richiedono l'uso di più linguaggi per lo sviluppo.
* Servizi in tempo reale da punto a punto che devono gestire richieste o risposte di streaming.

Nonostante nella [pagina gRPC](https://grpc.io/docs/quickstart/csharp.html) ufficiale sia attualmente disponibile un'implementazione C#, l'implementazione corrente si basa sulla libreria nativa scritta in C ([C-core](https://grpc.io/blog/grpc-stacks) gRPC). È attualmente in corso la preparazione di una nuova implementazione completamente gestita basata sul server HTTP Kestrel e sullo stack di ASP.NET Core. I documenti seguenti contengono un'introduzione alla creazione di servizi gRPC con questa nuova implementazione.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>