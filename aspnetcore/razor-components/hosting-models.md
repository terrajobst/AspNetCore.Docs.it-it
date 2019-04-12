---
title: Componenti di Razor modelli di hosting
author: guardrex
description: Comprendere Blazor lato client e lato server ASP.NET Core Razor componenti modelli di hosting.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ed70117c94bf1a3e4c208f70310bbf0473bae44
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515527"
---
# <a name="razor-components-hosting-models"></a>Componenti di Razor modelli di hosting

Da [Daniel Roth](https://github.com/danroth27)

Componenti Razor è un framework web progettato per l'esecuzione del client nel browser in un [WebAssembly](http://webassembly.org/)-basati su runtime di .NET (*Blazor*) o sul lato server in ASP.NET Core (*Razor di ASP.NET Core Componenti*). Indipendentemente dal fatto i modelli di modello, l'app e il componente di hosting *rimangono invariate*. Questo articolo illustra i modelli di hosting disponibili:

* [Blazor sul lato client](#client-side-hosting-model)
* [Razor Components ASP.NET Core sul lato server](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a>Modello di hosting sul lato client

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Modello di hosting dell'entità per Blazor è sul lato client in esecuzione nel browser sul WebAssembly. L'app Blazor, le relative dipendenze e il runtime .NET vengono scaricati nel browser. L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser. Aggiornamenti dell'interfaccia utente e la gestione degli eventi si verificano all'interno dello stesso processo. Gli asset dell'app vengono distribuiti come file statici da un server web o servizio in grado di servire contenuti statici per i client.

![Blazor lato client: L'app Blazor viene eseguita su un thread dell'interfaccia utente all'interno del browser.](hosting-models/_static/client-side.png)

Per creare un'app Blazor usando il modello di hosting lato client, utilizzare uno dei modelli seguenti:

* **Blazor** ([blazor nuovo dotnet](/dotnet/core/tools/dotnet-new)) &ndash; distribuito come un set di file statici.
* **(ASP.NET Core ospitate) Blazor** ([blazorhosted nuovo dotnet](/dotnet/core/tools/dotnet-new)) &ndash; ospitate da un server di ASP.NET Core. L'app ASP.NET Core viene usato l'app Blazor ai client. L'app Blazor lato client può interagire con il server in rete con chiamate all'API web o [SignalR](xref:signalr/introduction).

I modelli includono le *components.webassembly.js* script che gestisce:

* Scaricare il runtime di .NET, l'app e le dipendenze dell'app.
* Inizializzazione del runtime per eseguire l'app.

Il modello di hosting lato client offre diversi vantaggi. Blazor lato client:

* Non dispone di alcuna dipendenza lato server .NET.
* Utilizza le funzionalità e le risorse del client.
* Gli Offload di lavoro dal server al client.
* Supporta scenari non in linea.

Degli svantaggi all'hosting lato client. Blazor lato client:

* Limita l'app per le funzionalità del browser.
* Richiede un client in grado di supportare hardware e software (ad esempio, WebAssembly supporto).
* Ha una maggiore dimensione del download e l'app più il tempo di caricamento.
* È inferiore a maturare runtime .NET e supporto degli strumenti (ad esempio, le limitazioni nel [.NET Standard](/dotnet/standard/net-standard) supporto e il debug).

## <a name="server-side-hosting-model"></a>Modello di hosting sul lato server

Con il modello di hosting ASP.NET Core Razor componenti lato server, l'app viene eseguita nel server da all'interno di un'app ASP.NET Core. Aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite un [SignalR](xref:signalr/introduction) connessione.

![ASP.NET Core Razor componenti lato server: Il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) sul server tramite una connessione SignalR.](hosting-models/_static/server-side.png)

Per creare un'app Razor componenti usando il modello di hosting sul lato server, usare ASP.NET Core **componenti Razor** modello ([dotnet nuovo razorcomponents](/dotnet/core/tools/dotnet-new)). L'app ASP.NET Core ospita l'app sul lato server per i componenti di Razor e configura l'endpoint di SignalR in cui i client si connettono. L'app ASP.NET Core fa riferimento l'app `Startup` classe da aggiungere:

* Servizi di Razor componenti lato server.
* L'app per la gestione della pipeline delle richieste.

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

Il *components.server.js* script&dagger; stabilisce la connessione client. È responsabilità dell'app per mantenere e ripristinare lo stato di app in base alle esigenze (ad esempio, in caso di una connessione di rete persa).

Il modello di hosting di server-side offre diversi vantaggi. Componenti lato server Razor:

* Hanno dimensioni app notevolmente inferiori rispetto a un'app Blazor lato client e caricare più velocemente.
* Possono sfruttare le funzionalità server, incluso l'uso di qualsiasi API compatibili con .NET Core.
* Eseguire in .NET Core nel server, quindi .NET esistenti degli strumenti, ad esempio il debug, funziona come previsto.
* Funziona con i thin client (ad esempio, i browser che non supportano WebAssembly e risorse vincolate dispositivi).
* .NET /C# base di codice, incluso il codice di componente dell'app, non viene servita al client.

Esistono svantaggi lato server di hosting. Componenti lato server Razor:

* Hanno una latenza maggiore: Ogni interazione dell'utente prevede un hop di rete.
* Non offrono alcun supporto offline: Se la connessione del client non riesce, l'app smette di funzionare.
* La scalabilità ridotta: Il server deve gestire più connessioni di client e la gestione dello stato del client.
* Richiede un server di ASP.NET Core per rendere disponibile l'app. Distribuzione senza un server (ad esempio, da una rete CDN) non è possibile.

&dagger;Il *components.server.js* lo script viene pubblicato nel seguente percorso: *bin / {Debug | Versione} / {FRAMEWORK di destinazione} /publish/ {nome applicazione}. Dist/App/Framework*.
