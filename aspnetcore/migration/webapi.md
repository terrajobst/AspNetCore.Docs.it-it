---
title: Eseguire la migrazione da ASP.NET Web API per ASP.NET Core
author: ardalis
description: Informazioni su come eseguire la migrazione di un'implementazione di API Web di ASP.NET Web API a ASP.NET MVC di base.
manager: wpickett
ms.author: riande
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/webapi
ms.openlocfilehash: 8d842877e49e317323d453e71ebb3302245f388d
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2018
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Eseguire la migrazione da ASP.NET Web API per ASP.NET Core

[Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)

API Web sono servizi HTTP in grado di raggiungono una vasta gamma di client, inclusi browser e dispositivi mobili. ASP.NET MVC di base include il supporto per la compilazione di API Web che fornisce un singolo modo coerente, di compilazione di applicazioni web. In questo articolo viene descritto come i passaggi necessari per eseguire la migrazione di un'implementazione di API Web di API Web ASP.NET ad ASP.NET MVC di base.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Progetto API Web ASP.NET di revisione

Questo articolo viene utilizzato il progetto di esempio, *ProductsApp*, creato nell'articolo [Introduzione a ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) come punto di partenza. In tale progetto, un progetto di API Web ASP.NET semplice viene configurato come indicato di seguito.

In *Global.asax.cs*, viene effettuata una chiamata a `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` è definito in *App_Start*, e ha solo una riga statica `Register` metodo:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


Questa classe consente di configurare [routing degli attributi](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), anche se non è effettivamente utilizzato nel progetto. Viene inoltre configurata la tabella di routing, viene utilizzata da ASP.NET Web API. In questo caso, si otterranno gli URL in base al formato ASP.NET Web API */api/ {controller} / {id}*, con *{id}* sia facoltativo.

Il *ProductsApp* progetto include un solo controller semplice, che eredita da `ApiController` ed espone due metodi:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Infine, il modello di *prodotto*, utilizzato per il *ProductsApp*, una classe semplice:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Dopo aver creato un progetto semplice da cui iniziare, è possibile illustrare come eseguire la migrazione di questo progetto API Web per ASP.NET MVC di base.

## <a name="create-the-destination-project"></a>Creare il progetto di destinazione

Utilizzando Visual Studio, creare una nuova soluzione vuota e denominarla *WebAPIMigration*. Aggiungere esistente *ProductsApp* progetto a esso, quindi, aggiungere un nuovo progetto applicazione ASP.NET Core Web alla soluzione. Denominare il nuovo progetto *ProductsCore*.

![Finestra di dialogo Nuovo progetto aprire modelli Web](webapi/_static/add-web-project.png)

Successivamente, scegliere il modello di progetto API Web. Verrà eseguita la migrazione è il *ProductsApp* contenuto al nuovo progetto.

![Finestra di dialogo Nuovo applicazione Web con il modello di progetto API Web selezionato nell'elenco di modelli ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Eliminare il `Project_Readme.html` file dal nuovo progetto. La soluzione dovrebbe risultare simile al seguente:

![Apri in Esplora soluzioni con file e cartelle dei progetti ProductsApp e ProductsCore soluzione dell'applicazione](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>La migrazione della configurazione

ASP.NET di base non vengono più utilizzati *Global. asax*, *Web. config*, o *App_Start* cartelle. Al contrario, tutte le attività di avvio vengono eseguite in *Startup.cs* nella radice del progetto (vedere [avvio dell'applicazione](../fundamentals/startup.md)). In ASP.NET MVC di base, il routing basato su attributi è ora inclusa per impostazione predefinita quando `UseMvc()` viene chiamato e questo è l'approccio consigliato per la configurazione delle route di API Web (e il modo in cui il progetto iniziale di API Web gestisce routing).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Supponendo che si desidera utilizzare il routing degli attributi nel progetto in futuro, è necessaria alcuna configurazione aggiuntiva. È sufficiente applicare gli attributi in base alle esigenze per i controller e azioni, come avviene nell'esempio `ValuesController` classe inclusa nel progetto di avvio di Web API:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Si noti la presenza di *[controller]* sulla riga 8. Routing basato su attributo ora supporta determinati token, ad esempio *[controller]* e *[azione]*. Questi token vengono sostituiti in fase di esecuzione con il nome del controller o azione, rispettivamente, a cui è stato applicato l'attributo. Questa opzione serve a ridurre il numero di stringhe chiave nel progetto e garantisce che le route rimarrà sincronizzate con i corrispondenti controller e le azioni quando viene applicato il refactoring di ridenominazione automatica.

Per eseguire la migrazione il controller API di prodotti, è necessario copiare *ProductsController* al nuovo progetto. Quindi includere semplicemente l'attributo della route sul controller:

```csharp
[Route("api/[controller]")]
```

È inoltre necessario aggiungere il `[HttpGet]` attributo per i due metodi, poiché entrambi deve essere chiamati tramite HTTP Get. Includere la prospettiva di un parametro "id" nell'attributo `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

A questo punto, il routing è configurato correttamente. Tuttavia, è Impossibile ancora eseguirne il test. Altre modifiche devono essere eseguite prima *ProductsController* verrà compilato.

## <a name="migrate-models-and-controllers"></a>Eseguire la migrazione di modelli e controller

L'ultimo passaggio del processo di migrazione per questo progetto API Web semplice consiste nel copiare i controller e tutti i modelli utilizzano. In questo caso, è sufficiente copiare *Controllers/ProductsController.cs* dal progetto originale a quello nuovo. Copiare l'intera cartella modelli del progetto originale a quello nuovo. Modificare gli spazi dei nomi per corrispondere al nuovo nome di progetto (*ProductsCore*).  A questo punto, è possibile compilare l'applicazione e si troverà un numero di errori di compilazione. In genere, questi devono rientrare nelle categorie seguenti:

* *ApiController* non esiste

* *System.Web.Http* dello spazio dei nomi non esiste.

* *IHttpActionResult* non esiste

Fortunatamente, questi sono molto semplici correggere:

* Modifica *ApiController* a *Controller* (potrebbe essere necessario aggiungere *utilizzando Microsoft.AspNetCore.Mvc*)

* Eliminare qualsiasi istruzione che fa riferimento a *System.Web.Http*

* Modificare qualsiasi metodo restituzione *IHttpActionResult* per restituire un *IActionResult*

Una volta queste modifiche sono state effettuate e non utilizzati tramite istruzioni rimossi, dopo la migrazione *ProductsController* classe è simile al seguente:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

A questo punto debba essere in grado di eseguire il progetto di migrazione e passare a */api/prodotti*; e, è necessario visualizzare l'elenco completo dei 3 prodotti. Passare a */api/products/1* dovrebbe essere visualizzata prima del prodotto.

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a>Microsoft.AspNetCore.Mvc.WebApiCompatShim

È uno strumento utile durante la migrazione ASP.NET Web API progetti ASP.NET Core di [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) libreria. Lo shim di compatibilità estende ASP.NET Core per consentire un numero di diverse convenzioni di Web API 2 da utilizzare. Nell'esempio vengono trasferita in precedenza in questo documento è abbastanza semplice che lo shim di compatibilità non è necessario. Per progetti di grandi dimensioni, può essere utile per temporaneamente per colmare il divario API tra ASP.NET Core e ASP.NET Web API 2 usano lo shim di compatibilità.

Lo shim di compatibilità di Web API deve essere utilizzata come una misura temporanea per facilitare la migrazione dei progetti di grandi dimensioni API Web per ASP.NET Core. Nel corso del tempo, i progetti devono essere aggiornati per l'utilizzo di modelli ASP.NET Core invece di basarsi sullo shim di compatibilità. 

Le funzionalità di compatibilità incluse in Microsoft.AspNetCore.Mvc.WebApiCompatShim includono:

* Aggiunge un `ApiController` tipo in modo che i tipi di base dei controller non dovranno essere aggiornati.
* Consente l'associazione di modelli di stile di API Web. ASP.NET MVC modello associazione funzioni di base in modo simile a MVC 5, per impostazione predefinita. Le modifiche di shim di compatibilità del modello di associazione per essere più simile alle convenzioni di associazione modello API Web 2. Ad esempio, i tipi complessi vengono associati automaticamente dal corpo della richiesta.
* Estende l'associazione di modelli in modo che le azioni del controller possono accettare parametri di tipo `HttpRequestMessage`.
* Aggiunge i formattatori dei messaggi che consente di azioni per restituire i risultati di tipo `HttpResponseMessage`.
* Aggiunge metodi di risposta aggiuntivi che le azioni di Web API 2 potrebbero essere usata per servire risposte:
    * Generatori HttpResponseMessage:
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * Metodi di azione risultati:
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* Aggiunge un'istanza di `IContentNegotiator` al contenitore dell'app e rende contenuti tipi correlati alla negoziazione dal [webapi](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) disponibili. Sono inclusi tipi, ad esempio `DefaultContentNegotiator`, `MediaTypeFormatter`e così via.

Per usare lo shim di compatibilità, è necessario:

* Riferimento di [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pacchetto NuGet.
* Registrare i servizi del shim di compatibilità con contenitore dell'app chiamando `services.AddWebApiConventions()` dell'applicazione `Startup.ConfigureServices` metodo.
* Definire le route di Web API specifiche utilizzando `MapWebApiRoute` nella `IRouteBuilder` dell'applicazione `IApplicationBuilder.UseMvc` chiamare.

## <a name="summary"></a>Riepilogo

La migrazione di un progetto ASP.NET Web API semplice per ASP.NET MVC di base è piuttosto semplice, grazie al supporto incorporato per l'API Web di ASP.NET MVC di base. Le parti principali, che saranno necessario eseguire la migrazione di tutti i progetti ASP.NET Web API sono route, controller e i modelli, gli aggiornamenti per i tipi utilizzati dai controller e azioni.
