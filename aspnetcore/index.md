---
title: Introduzione a ASP.NET Core
author: rick-anderson
description: Offre un'introduzione ad ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: 7d5afd5871316509a385d14bbfe886bfe55d1ff4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-aspnet-core"></a>Introduzione a ASP.NET Core

Di [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core è un framework multipiattaforma, ad alte prestazioni, [open source](https://github.com/aspnet/home) per la compilazione di moderne applicazioni basate sul cloud, connesse a Internet. Con ASP.NET Core, è possibile:

* Compilare app web e servizi, app [IoT](https://www.microsoft.com/internet-of-things/) e back-end per dispositivi mobili.
* Usare gli strumenti di sviluppo preferiti in Windows, macOS e Linux.
* Distribuire nel cloud o in locale.
* Eseguire in [.NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Perché usare ASP.NET Core?

Milioni di sviluppatori hanno usato, e continuano a usare, [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) per creare app Web. ASP.NET Core è una riprogettazione di ASP.NET 4.x, con modifiche a livello di architettura che comportano un framework più efficiente e modulare.

ASP.NET Core offre i vantaggi seguenti:

* Una storia unificata per la compilazione dell'interfaccia utente web e delle API web.
* Integrazione di [moderni framework lato client](xref:client-side/index) e di flussi di lavoro di sviluppo.
* Un [sistema di configurazione](xref:fundamentals/configuration/index) basato sull'ambiente, pronto per il cloud.
* [Inserimento delle dipendenze](xref:fundamentals/dependency-injection) incorporato.
* Una pipeline di richieste HTTP leggera, [ad alte prestazioni](https://github.com/aspnet/benchmarks) e modulare.
* Possibilità di gestire l'hosting in [IIS](xref:host-and-deploy/iis/index), [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) e [Docker](xref:host-and-deploy/docker/index) o di testare internamente i processi personalizzati.
* Controllo delle versioni delle app affiancato quando la destinazione è [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
* Gli strumenti che semplificano lo sviluppo del web moderno.
* Possibilità di compilare ed eseguire in Windows, macOS e Linux.
* Open-source e [incentrato sulle community](https://live.asp.net/).

ASP.NET Core viene fornito esclusivamente come pacchetti [NuGet](https://www.nuget.org/). Ciò consente di ottimizzare l'app per includere solo i pacchetti NuGet necessari. In effetti, le app ASP.NET Core 2.x destinate a NET Core richiedono un [singolo pacchetto NuGet](xref:fundamentals/metapackage). Una riduzione della superficie occupata dall'app offre anche diversi vantaggi, tra cui una maggiore sicurezza, una riduzione delle esigenze di assistenza e un miglioramento delle prestazioni.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Compilare API web e interfaccia utente web tramite ASP.NET Core MVC

ASP.NET Core MVC offre funzionalità per la compilazione di [API Web](xref:tutorials/index#build-web-apis) e [app Web](xref:tutorials/index#build-web-apps):

* Il [Model-View-Controller (MVC)](xref:mvc/overview) consente di rendere le API web e le app web [testabili](testing/index.md).
* [Le pagine Razor](xref:mvc/razor-pages/index) (una novità di ASP.NET Core 2.0) sono un modello di programmazione basato su pagine che rende la creazione di un'interfaccia utente Web più semplice ed efficace.
* [Il markup Razor](xref:mvc/views/razor) offre una sintassi produttiva per le [pagine Razor](xref:mvc/razor-pages/index) e le [visualizzazioni MVC](xref:mvc/views/overview).
* Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.
* Il supporto incorporato per [più formati di dati e per la negoziazione del contenuto](mvc/models/formatting.md) consente alle API web di raggiungere una vasta gamma di client, inclusi i browser e i dispositivi mobili.
* L'[associazione di modelli](xref:mvc/models/model-binding) esegue automaticamente il mapping dei dati dalle richieste HTTP ai parametri del metodo di azione.
* La [convalida dei modelli](xref:mvc/models/validation) esegue automaticamente la convalida sul lato server e sul lato client.

## <a name="client-side-development"></a>Sviluppo lato client

ASP.NET Core si integra perfettamente con popolari framework e librerie sul lato client, inclusi [Angular](xref:spa/angular), [React](xref:spa/react) e [Bootstrap](xref:client-side/bootstrap). Vedere [Sviluppo lato client](xref:client-side/index) per altri dettagli.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* [Esercitazioni di ASP.NET Core](xref:tutorials/index)
* [Nozioni fondamentali su ASP.NET Core](xref:fundamentals/index)
* [La trasmissione settimanale della community di ASP.NET](https://live.asp.net/) offre una presentazione dei progressi e dei piani del team. Vengono segnalati nuovi blog e software di terze parti.
