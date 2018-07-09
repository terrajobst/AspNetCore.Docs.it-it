---
title: Eseguire la migrazione da API Web ASP.NET ad ASP.NET Core
author: ardalis
description: Informazioni su come eseguire la migrazione di un'implementazione di API Web dall'API Web ASP.NET a ASP.NET Core MVC.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894192"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Eseguire la migrazione da API Web ASP.NET ad ASP.NET Core

[Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)

Le API Web sono servizi HTTP che soddisfano una vasta gamma di client, inclusi browser e dispositivi mobili. ASP.NET Core MVC include il supporto per le API Web, offrendo un modo unico e coerenza della compilazione di applicazioni web. In questo articolo è illustrare i passaggi necessari per eseguire la migrazione di un'implementazione di API Web dall'API Web ASP.NET ad ASP.NET Core MVC.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Progetto API Web ASP.NET di revisione

Questo articolo usa il progetto di esempio *ProductsApp*, creato nell'articolo [Introduzione a ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) come punto di partenza. Nel progetto, un progetto API Web ASP.NET semplice è configurato come indicato di seguito.

Nelle *Global.asax.cs*, viene effettuata una chiamata a `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` è definito in *App_Start*, e ha solo una riga statica `Register` metodo:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

Questa classe consente di configurare [routing con attributi](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), anche se non è effettivamente utilizzato nel progetto. Configura inoltre la tabella di routing, che viene usata da ASP.NET Web API. In questo caso, API Web ASP.NET si aspetta di ricevere gli URL in base al formato */api/ {controller} / {id}*, con *{id}* sia facoltativo.

Il *ProductsApp* progetto include un solo controller semplice, che eredita da `ApiController` ed espone due metodi:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Infine, il modello *prodotto*, usata per il *ProductsApp*, è una semplice classe:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Ora che abbiamo un semplice progetto da cui iniziare, è possibile illustrare come eseguire la migrazione di questo progetto API Web ad ASP.NET Core MVC.

## <a name="create-the-destination-project"></a>Creare il progetto di destinazione

Usa Visual Studio, creare una nuova soluzione vuota e denominarla *WebAPIMigration*. Aggiungi esistenti *ProductsApp* progetto ad esso, quindi, aggiungere un nuovo progetto di applicazione Web Core ASP.NET alla soluzione. Denominare il nuovo progetto *ProductsCore*.

![Finestra di dialogo Nuovo progetto aprire per i modelli Web](webapi/_static/add-web-project.png)

Scegliere quindi il modello di progetto API Web. Verrà eseguita la migrazione è il *ProductsApp* contenuto per questo nuovo progetto.

![Nuova finestra di dialogo dell'applicazione Web con il modello di progetto API Web selezionato nell'elenco dei modelli ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Eliminare il `Project_Readme.html` file dal nuovo progetto. La soluzione dovrebbe ora essere simile al seguente:

![Apertura della soluzione dell'applicazione in Esplora soluzioni con file e cartelle dei progetti ProductsApp e ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>La migrazione della configurazione

ASP.NET Core non vengono più usati *Global. asax*, *Web. config*, o *App_Start* cartelle. Al contrario, tutte le attività di avvio vengono eseguite nel *Startup.cs* nella radice del progetto (vedere [avvio dell'applicazione](../fundamentals/startup.md)). In ASP.NET Core MVC, il routing basato su attributi è ora inclusa per impostazione predefinita quando `UseMvc()` viene chiamato; e, questa è l'approccio consigliato per configurare le route di API Web (ed è il modo in cui il progetto iniziale di API Web gestisce il routing).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Supponendo che si desidera utilizzare il routing con attributi nel progetto in futuro, è necessaria alcuna configurazione aggiuntiva. È sufficiente applicare gli attributi in base alle esigenze per i controller e azioni, come avviene nell'esempio `ValuesController` classe che viene incluso nel progetto API Web starter:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Si noti la presenza di *[controller]* sulla riga 8. Routing basato su attributo ora supporta alcuni token, ad esempio *[controller]* e *[action]*. Questi token vengono sostituiti in fase di esecuzione con il nome del controller o azione, rispettivamente, a cui è stato applicato l'attributo. Questa opzione serve a ridurre il numero di "stringhe magiche" nel progetto e assicura che le route rimarrà sincronizzate con i corrispondenti controller e azioni quando il refactoring di ridenominazione automatica viene applicato.

Per eseguire la migrazione il controller API di prodotti, è prima necessario copiare *ProductsController* al nuovo progetto. Quindi includere semplicemente l'attributo della route nel controller:

```csharp
[Route("api/[controller]")]
```

È inoltre necessario aggiungere il `[HttpGet]` attributo per i due metodi, poiché entrambi deve essere chiamati tramite HTTP Get. Includere la prospettiva di un parametro "id" nell'attributo per `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

A questo punto, il routing è configurato correttamente. Tuttavia, abbiamo non possiamo ancora eseguirne il test. Prima è necessario apportare modifiche aggiuntive *ProductsController* verrà compilato.

## <a name="migrate-models-and-controllers"></a>Eseguire la migrazione di modelli e controller

L'ultimo passaggio del processo di migrazione per questo progetto API Web semplice consiste nel copiare tramite il controller e gli eventuali modelli usano. In questo caso, è sufficiente copiare *Controllers/ProductsController.cs* dal progetto originale a quello nuovo. Quindi, copiare l'intera cartella Modelli dal progetto originale a quello nuovo. Modificare gli spazi dei nomi in modo che corrisponda il nuovo nome del progetto (*ProductsCore*).  A questo punto, è possibile compilare l'applicazione e si noterà un numero di errori di compilazione. Questi devono in genere rientrano nelle categorie seguenti:

* *ApiController* non esiste

* *System.Web.Http* dello spazio dei nomi non esiste

* *IHttpActionResult* non esiste

Fortunatamente, questi sono tutti molto facili da risolvere:

* Change *ApiController* al *Controller* (potrebbe essere necessario aggiungere *usando Microsoft.AspNetCore.Mvc*)

* Eliminare eventuali che fa riferimento all'istruzione using *System.Web.Http*

* Modificare qualsiasi metodo restituzione *IHttpActionResult* per restituire un *IActionResult*

Dopo che queste modifiche sono state effettuate e inutilizzato istruzioni using rimossi, dopo la migrazione *ProductsController* abbia un aspetto simile al seguente:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

A questo punto dovrebbe essere in grado di eseguire il progetto migrato e passare a */api/prodotti*; e, si dovrebbe visualizzare l'elenco completo dei 3 prodotti. Passare a */api/products/1* dovrebbe essere il primo prodotto.

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a>Shim di compatibilità ASP.NET 4.x API Web 2

Uno strumento utile quando i progetti di migrazione ASP.NET Web API ad ASP.NET Core è il [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) libreria. Lo shim di compatibilità si estende a ASP.NET Core per consentire un numero di diverse convenzioni di Web API 2 da usare. L'esempio trasferita in precedenza in questo documento è abbastanza semplice che lo shim di compatibilità non è necessario. Per progetti di grandi dimensioni usano lo shim di compatibilità può essere utile per temporaneamente colmare la lacuna API tra ASP.NET Core e ASP.NET Web API 2.

Lo shim di compatibilità delle API Web deve essere usata come misura temporanea per facilitare la migrazione dei grandi progetti API Web ad ASP.NET Core. Nel corso del tempo, i progetti devono essere aggiornati per usare i modelli ASP.NET Core anziché basarsi sullo shim di compatibilità.

Le funzionalità di compatibilità inclusi in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` includono:

* Aggiunge un `ApiController` tipo in modo che i tipi di base di controller non devono essere aggiornati.
* Consente l'associazione del modello basato su Web API. ASP.NET Core MVC associazione funzioni del modello in modo analogo a MVC 5, per impostazione predefinita. Le modifiche di shim di compatibilità del modello dell'associazione per essere più simile alle convenzioni dell'associazione del modello API Web 2. Ad esempio, i tipi complessi vengono associati automaticamente dal corpo della richiesta.
* Estende l'associazione di modelli in modo che le azioni del controller possono accettare parametri di tipo `HttpRequestMessage`.
* Aggiunge i formattatori dei messaggi che consente le azioni per restituire i risultati di tipo `HttpResponseMessage`.
* Aggiunge i metodi di risposta aggiuntivi che le azioni di API Web 2 potrebbero essere utilizzati per gestire le risposte:
  * Generatori HttpResponseMessage:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Metodi di azione risultati:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Aggiunge un'istanza di `IContentNegotiator` al contenitore di inserimento delle dipendenze dell'app e rende il contenuto tipi correlati alla negoziazione dal [ASPNET](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) disponibili. Sono inclusi i tipi, ad esempio `DefaultContentNegotiator`, `MediaTypeFormatter`e così via.

Per usare lo shim di compatibilità, è necessario:

* Installare il [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pacchetto NuGet.
* Registrazione servizi dello shim di compatibilità con contenitore di inserimento delle dipendenze dell'app tramite la chiamata `services.AddMvc().AddWebApiConventions()` dell'app `Startup.ConfigureServices` (metodo).
* Definire le route specifici dell'API Web usando `MapWebApiRoute` nella `IRouteBuilder` dell'app `IApplicationBuilder.UseMvc` chiamare.

## <a name="summary"></a>Riepilogo

La migrazione di un semplice progetto API Web ASP.NET a ASP.NET Core MVC è piuttosto semplice, grazie al supporto predefinito per le API Web in ASP.NET Core MVC. I componenti principali, che saranno necessario eseguire la migrazione di tutti i progetti API Web ASP.NET sono le route, controller e modelli, insieme agli aggiornamenti per i tipi utilizzati dal controller e azioni.
