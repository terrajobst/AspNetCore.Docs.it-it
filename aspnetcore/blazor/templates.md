---
title: Modelli di Blazor ASP.NET Core
author: guardrex
description: Informazioni su ASP.NET Core Blazor modelli di app e Blazor struttura del progetto.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: 2a95b986450471b474d93ead252255f2bd9d4918
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160119"
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

* *Program.cs* &ndash; punto di ingresso dell'app che configura l' [host](xref:fundamentals/host/generic-host)ASP.NET Core. Il codice in questo file è comune a tutte le app ASP.NET Core generate dai modelli ASP.NET Core.

* *Startup.cs* &ndash; contiene la logica di avvio dell'app. La classe `Startup` definisce due metodi:

  * `ConfigureServices` &ndash; configura i servizi [di inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app. Nelle app Blazor server i servizi vengono aggiunti chiamando <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>e il `WeatherForecastService` viene aggiunto al contenitore del servizio per l'uso da parte del componente `FetchData` di esempio.
  * `Configure` &ndash; configura la pipeline di gestione delle richieste dell'app:
    * Blazor webassembly &ndash; aggiunge il componente `App` (specificato come `app` elemento DOM al metodo `AddComponent`), che è il componente radice dell'app.
    * Server Blazor
      * <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> viene chiamato per configurare un endpoint per la connessione in tempo reale con il browser. La connessione viene creata con [SignalR](xref:signalr/introduction), un Framework per l'aggiunta di funzionalità Web in tempo reale alle app.
      * [MapFallbackToPage ("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) viene chiamato per configurare la pagina radice dell'app (*pages/_Host. cshtml*) e abilitare la navigazione.

* *wwwroot/index.html* (Blazor webassembly) &ndash; pagina radice dell'app implementata come pagina HTML:
  * Quando viene inizialmente richiesta una pagina dell'app, il rendering della pagina viene eseguito e restituito nella risposta.
  * La pagina specifica la posizione in cui viene eseguito il rendering del componente `App` radice. Il componente `App` (*app. Razor*) viene specificato come elemento `app` Dom al metodo `AddComponent` in `Startup.Configure`.
  * Viene caricato il file `_framework/blazor.webassembly.js` JavaScript, che:
    * Scarica il Runtime .NET, l'app e le dipendenze dell'app.
    * Inizializza il runtime per l'esecuzione dell'app.

* *Pages/_Host. cshtml* (Blazor Server) &ndash; la pagina radice dell'app implementata come pagina Razor:
  * Quando viene inizialmente richiesta una pagina dell'app, il rendering della pagina viene eseguito e restituito nella risposta.
  * Viene caricato il file `_framework/blazor.server.js` JavaScript, che configura la connessione SignalR in tempo reale tra il browser e il server.
  * Nella pagina host viene specificata la posizione in cui viene eseguito il rendering del componente `App` radice (*app. Razor*).

* *App. razor* &ndash; il componente radice dell'app che configura il routing sul lato client usando il componente <xref:Microsoft.AspNetCore.Components.Routing.Router>. Il componente `Router` intercetta la navigazione del browser ed esegue il rendering della pagina corrispondente all'indirizzo richiesto.

* Cartella *pages* &ndash; contiene i componenti e le pagine instradabili (*Razor*) che costituiscono l'app Blazor. La route per ogni pagina viene specificata utilizzando la direttiva [`@page`](xref:mvc/views/razor#page) . Il modello include i componenti seguenti:
  * `Index` (*index. Razor*) &ndash; implementa la Home page.
  * `Counter` (*Counter. Razor*) &ndash; implementa la pagina del contatore.
  * `Error` (*Error. Razor*, Blazor solo app Server) &ndash; sottoposto a rendering quando si verifica un'eccezione non gestita nell'app.
  * `FetchData` (*fetchData. Razor*) &ndash; implementa la pagina Recupera dati.

* Cartella *condivisa* &ndash; contiene altri componenti dell'interfaccia utente (*Razor*) usati dall'app:
  * `MainLayout` (*MainLayout. Razor*) &ndash; componente layout dell'app.
  * `NavMenu` (*NavMenu. Razor*) &ndash; implementa la navigazione nell'intestazione laterale. Include il [componente NavLink](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), che esegue il rendering dei collegamenti di navigazione ad altri componenti Razor. Il componente `NavLink` indica automaticamente uno stato selezionato quando viene caricato il componente, che consente all'utente di comprendere il componente attualmente visualizzato.

* *_Imports. razor* &ndash; include direttive Razor comuni da includere nei componenti dell'app (*Razor*), ad esempio [`@using`](xref:mvc/views/razor#using) direttive per gli spazi dei nomi.

* Cartella *dati* (serverBlazor) &ndash; contiene la classe `WeatherForecast` e l'implementazione del `WeatherForecastService` che forniscono dati meteorologici di esempio al componente `FetchData` dell'app.

* *wwwroot* &ndash; la cartella [radice Web](xref:fundamentals/index#web-root) per l'app contenente le risorse statiche pubbliche dell'app.

* *appSettings. JSON* (serverBlazor) &ndash; le impostazioni di configurazione per l'app.
