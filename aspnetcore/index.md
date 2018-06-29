---
title: Introduzione a ASP.NET Core
author: rick-anderson
description: Introduzione ad ASP.NET Core, un framework multipiattaforma, ad alte prestazioni, open source per la compilazione di applicazioni moderne basate sul cloud, connesse a Internet.
ms.author: riande
ms.date: 02/28/2018
uid: index
ms.openlocfilehash: 6de7f1bc8229c5de519e4064dda0a7061cf8b9c6
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077712"
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

ASP.NET Core viene fornito esclusivamente come pacchetti [NuGet](https://www.nuget.org/). L'uso di pacchetto NuGet consente di ottimizzare l'app includendo solo le dipendenze necessarie. In effetti, le app ASP.NET Core 2.x destinate a NET Core richiedono un [singolo pacchetto NuGet](xref:fundamentals/metapackage). Una riduzione della superficie occupata dall'app offre anche diversi vantaggi, tra cui una maggiore sicurezza, una riduzione delle esigenze di assistenza e un miglioramento delle prestazioni.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Compilare API web e interfaccia utente web tramite ASP.NET Core MVC

ASP.NET Core MVC offre funzionalità per la compilazione di [API Web](xref:tutorials/index#build-web-apis) e [app Web](xref:tutorials/index#build-web-apps):

* Il [Model-View-Controller (MVC)](xref:mvc/overview) consente di rendere le API web e le app web [testabili](xref:test/index).
* [Le pagine Razor](xref:razor-pages/index) (una novità di ASP.NET Core 2.0) sono un modello di programmazione basato su pagine che rende la creazione di un'interfaccia utente Web più semplice ed efficace.
* [Il markup Razor](xref:mvc/views/razor) offre una sintassi produttiva per le [pagine Razor](xref:razor-pages/index) e le [visualizzazioni MVC](xref:mvc/views/overview).
* Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.
* Il supporto incorporato per [più formati di dati e per la negoziazione del contenuto](xref:web-api/advanced/formatting) consente alle API web di raggiungere una vasta gamma di client, inclusi i browser e i dispositivi mobili.
* L'[associazione di modelli](xref:mvc/models/model-binding) esegue automaticamente il mapping dei dati dalle richieste HTTP ai parametri del metodo di azione.
* La [convalida dei modelli](xref:mvc/models/validation) esegue automaticamente la convalida sul lato server e sul lato client.

## <a name="client-side-development"></a>Sviluppo lato client

ASP.NET Core si integra perfettamente con popolari framework e librerie sul lato client, inclusi [Angular](xref:spa/angular), [React](xref:spa/react) e [Bootstrap](xref:client-side/bootstrap). Per altre informazioni, vedere [Sviluppo lato client](xref:client-side/index).

## <a name="aspnet-core-targeting-net-framework"></a>ASP.NET Core per .NET Framework

ASP.NET Core può avere come destinazione .NET Core o .NET Framework. Le app ASP.NET Core destinate a .NET Framework non sono multipiattaforma, ma funzionano solo in Windows. Non è prevista la rimozione del supporto di ASP.NET Core per .NET Framework. ASP.NET Core è in genere costituito da librerie [.NET Standard](/dotnet/standard/net-standard). Le app scritte con .NET 2.0 Standard funzionano ovunque sia supportato .NET Standard 2.0.

Usare .NET Core come destinazione offre diversi vantaggi, che aumentano con ogni versione. Alcuni vantaggi di .NET Core in .NET Framework sono:

* Funzionamento multipiattaforma. Esecuzione con macOS, Linux e Windows.
* Miglioramento delle prestazioni
* Controllo delle versioni side-by-side
* Nuove API
* Open source

È in corso un'intensa attività volta a colmare il divario da .NET Framework a .NET Core relativo alle API. [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) ha reso disponibili in .NET Core migliaia di API solo per Windows. Queste API non erano disponibili in .NET Core 1. x.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Esercitazioni di ASP.NET Core](xref:tutorials/index)
* [Nozioni fondamentali su ASP.NET Core](xref:fundamentals/index)
* [La trasmissione settimanale della community di ASP.NET](https://live.asp.net/) offre una presentazione dei progressi e dei piani del team. Vengono segnalati nuovi blog e software di terze parti.
