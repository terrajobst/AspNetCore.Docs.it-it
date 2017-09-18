---
title: Introduzione a ASP.NET Core
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: 10831719630bc638ce2f7424f53548518868d433
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-aspnet-core"></a>Introduzione a ASP.NET Core

Di [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core è un framework multipiattaforma, ad alte prestazioni, [open source](https://github.com/aspnet/home) per la compilazione di moderne applicazioni basate sul cloud, connesse a Internet. Con ASP.NET Core, è possibile:

* Compilare app web e servizi, app IoT e back-end per dispositivi mobili.
* Usare gli strumenti di sviluppo preferiti in Windows, macOS e Linux.
* Distribuisci sul cloud o in locale
* Eseguire in [.NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Perché usare ASP.NET Core?

Milioni di sviluppatori hanno usato ASP.NET (e continuano a usarlo) per creare app web. ASP.NET Core è una riprogettazione di ASP.NET, con modifiche a livello di architettura che comportano un framework più efficiente e modulare.

ASP.NET Core offre i vantaggi seguenti:

* Una storia unificata per la compilazione dell'interfaccia utente web e delle API web.
* Integrazione di [moderni framework lato client](xref:client-side/index) e di flussi di lavoro di sviluppo.
* Un [sistema di configurazione](xref:fundamentals/configuration) basato sull'ambiente, pronto per il cloud.
* [Inserimento delle dipendenze](xref:fundamentals/dependency-injection) incorporato.
* Una pipeline di richiesta HTTP leggera, ad alte prestazioni e modulare.
* Capacità di eseguire l'host in IIS o il self-host nel processo dell'utente.
* È possibile eseguire in [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), che supporta la versione del'app side-by-side impostata su true.
* Gli strumenti che semplificano lo sviluppo del web moderno.
* Possibilità di compilare ed eseguire in Windows, macOS e Linux.
* Focalizzati su risorse open source e sulle community.

ASP.NET Core viene fornito esclusivamente come pacchetti [NuGet](https://nuget.org). Ciò consente di ottimizzare l'app per includere i pacchetti NuGet che sono necessari. Una riduzione della superficie occupata dall'app offre anche diversi vantaggi, tra cui una maggiore sicurezza, una riduzione delle esigenze di assistenza e un miglioramento delle prestazioni.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Compilare API web e interfaccia utente web tramite ASP.NET Core MVC

ASP.NET Core MVC fornisce delle funzionalità che consentono di compilare [le API web](xref:tutorials/index#building-web-apis) e [le app web](xref:tutorials/index#building-web-applications):

* Il [Model-View-Controller (MVC)](xref:mvc/overview) consente di rendere le API web e le app web [testabili](testing/index.md).
* [Le pagine Razor](xref:mvc/razor-pages/index) (nuove in 2.0) un modello di programmazione basato su pagine che rende la creazione di un'interfaccia utente Web più semplice ed efficace.
* [La sintassi Razor](xref:mvc/views/razor) fornisce un linguaggio produttivo per le [pagine Razor](xref:mvc/razor-pages/index) e le [visualizzazioni MVC](xref:mvc/views/overview).
* Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.
* Il supporto incorporato per [più formati di dati e per la negoziazione del contenuto](mvc/models/formatting.md) consente alle API web di raggiungere una vasta gamma di client, inclusi i browser e i dispositivi mobili.
* [Associazione di modelli](xref:mvc/models/model-binding) esegue automaticamente il mapping dei dati dalle richieste HTTP ai parametri del metodo di azione.
* [Convalida modello](xref:mvc/models/validation) esegue automaticamente la convalida del lato server e del lato client.

## <a name="client-side-development"></a>Sviluppo lato client

ASP.NET Core è progettato per integrarsi perfettamente con un'ampia gamma di Framework, sul lato client, ad esempio [AngularJS](xref:client-side/angular), [Knockout.js](xref:client-side/knockout), e [Bootstrap](xref:client-side/bootstrap). Vedere [Sviluppo lato client](client-side/index.md) per altri dettagli.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* [Esercitazioni di ASP.NET Core](xref:tutorials/index)
* [Nozioni fondamentali su ASP.NET Core](xref:fundamentals/index)
* [L'impostazione settimanale della community di ASP.NET](https://live.asp.net/) copre lo stato di avanzamento del team e i pianifica e caratterizza i nuovi blog e i software di terze parti.
