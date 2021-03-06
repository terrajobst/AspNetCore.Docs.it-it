---
title: Modelli di Blazor ASP.NET Core
author: guardrex
description: Informazioni su ASP.NET Core Blazor modelli di app e Blazor struttura del progetto.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: 71a9d9eee8637dda0b3cecac82ff96a0c3bfedb5
ms.sourcegitcommit: f3b1bcfd108e5d53f73abc0bf2555890869d953b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80320985"
---
# <a name="aspnet-core-opno-locblazor-templates"></a>Modelli di Blazor ASP.NET Core

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

In Blazor Framework sono disponibili modelli per lo sviluppo di app per ognuno dei modelli di hosting Blazor:

* Blazor webassembly (`blazorwasm`)
* Server Blazor (`blazorserver`)

Per ulteriori informazioni sui modelli di hosting di Blazor, vedere <xref:blazor/hosting-models>.

Per istruzioni dettagliate sulla creazione di un'app Blazor da un modello, vedere <xref:blazor/get-started>.

## <a name="opno-locblazor-project-structure"></a>struttura del progetto Blazor

I file e le cartelle seguenti costituiscono un'app Blazor generata da un modello di Blazor:

* *Program.cs* &ndash; punto di ingresso dell'app che configura:

  * [Host](xref:fundamentals/host/generic-host) ASP.NET Core (serverBlazor)
  * L'host webassembly (Blazor webassembly) &ndash; il codice in questo file è univoco per le app create dal modello Blazor webassembly (`blazorwasm`).
    * Il componente `App`, che è il componente radice dell'app, viene specificato come elemento `app` DOM al metodo `Add`.
    * I servizi possono essere configurati con il metodo `ConfigureServices` nel generatore host, ad esempio `builder.Services.AddSingleton<IMyDependency, MyDependency>();`.
    * La configurazione può essere fornita tramite il generatore host (`builder.Configuration`).

* *Startup.cs* (serverBlazor) &ndash; contiene la logica di avvio dell'app. La classe `Startup` definisce due metodi:

  * `ConfigureServices` &ndash; configura i servizi [di inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app. Nelle app Blazor server i servizi vengono aggiunti chiamando <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>e il `WeatherForecastService` viene aggiunto al contenitore del servizio per l'uso da parte del componente `FetchData` di esempio.
  * `Configure` &ndash; configura la pipeline di gestione delle richieste dell'app:
    * <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> viene chiamato per configurare un endpoint per la connessione in tempo reale con il browser. La connessione viene creata con [SignalR](xref:signalr/introduction), un Framework per l'aggiunta di funzionalità Web in tempo reale alle app.
    * [MapFallbackToPage ("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) viene chiamato per configurare la pagina radice dell'app (*pages/_Host. cshtml*) e abilitare la navigazione.

* *wwwroot/index.html* (Blazor webassembly) &ndash; pagina radice dell'app implementata come pagina HTML:
  * Quando viene inizialmente richiesta una pagina dell'app, il rendering della pagina viene eseguito e restituito nella risposta.
  * La pagina specifica la posizione in cui viene eseguito il rendering del componente `App` radice. Il componente `App` (*app. Razor*) viene specificato come elemento `app` Dom al metodo `AddComponent` in `Startup.Configure`.
  * Viene caricato il file `_framework/blazor.webassembly.js` JavaScript, che:
    * Scarica il Runtime .NET, l'app e le dipendenze dell'app.
    * Inizializza il runtime per l'esecuzione dell'app.

* *App. razor* &ndash; il componente radice dell'app che configura il routing sul lato client usando il componente <xref:Microsoft.AspNetCore.Components.Routing.Router>. Il componente `Router` intercetta la navigazione del browser ed esegue il rendering della pagina corrispondente all'indirizzo richiesto.

* Cartella *pages* &ndash; contiene i componenti e le pagine instradabili (*Razor*) che costituiscono l'app Blazor e la pagina Razor radice di un'app Blazor server. La route per ogni pagina viene specificata utilizzando la direttiva [`@page`](xref:mvc/views/razor#page) . Il modello include quanto segue:
  * *_Host. cshtml* (Blazor Server) &ndash; pagina radice dell'app implementata come pagina Razor:
    * Quando viene inizialmente richiesta una pagina dell'app, il rendering della pagina viene eseguito e restituito nella risposta.
    * Viene caricato il file `_framework/blazor.server.js` JavaScript, che configura la connessione SignalR in tempo reale tra il browser e il server.
    * Nella pagina host viene specificata la posizione in cui viene eseguito il rendering del componente `App` radice (*app. Razor*).
  * `Counter` (*Counter. Razor*) &ndash; implementa la pagina del contatore.
  * `Error` (*Error. Razor*, Blazor solo app Server) &ndash; sottoposto a rendering quando si verifica un'eccezione non gestita nell'app.
  * `FetchData` (*fetchData. Razor*) &ndash; implementa la pagina Recupera dati.
  * `Index` (*index. Razor*) &ndash; implementa la Home page.

* Cartella *condivisa* &ndash; contiene altri componenti dell'interfaccia utente (*Razor*) usati dall'app:
  * `MainLayout` (*MainLayout. Razor*) &ndash; componente layout dell'app.
  * `NavMenu` (*NavMenu. Razor*) &ndash; implementa la navigazione nell'intestazione laterale. Include il [componente NavLink](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), che esegue il rendering dei collegamenti di navigazione ad altri componenti Razor. Il componente `NavLink` indica automaticamente uno stato selezionato quando viene caricato il componente, che consente all'utente di comprendere il componente attualmente visualizzato.

* *_Imports. razor* &ndash; include direttive Razor comuni da includere nei componenti dell'app (*Razor*), ad esempio [`@using`](xref:mvc/views/razor#using) direttive per gli spazi dei nomi.

* Cartella *dati* (serverBlazor) &ndash; contiene la classe `WeatherForecast` e l'implementazione del `WeatherForecastService` che forniscono dati meteorologici di esempio al componente `FetchData` dell'app.

* *wwwroot* &ndash; la cartella [radice Web](xref:fundamentals/index#web-root) per l'app contenente le risorse statiche pubbliche dell'app.

* *appSettings. JSON* (serverBlazor) &ndash; le impostazioni di configurazione per l'app.
