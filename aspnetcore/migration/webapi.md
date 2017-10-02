---
title: La migrazione da ASP.NET Web API
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4f0564b4-ed4e-4e1e-9755-c1144d21a0ef
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: 4acb7ccf7f944df5d08ac7faa342f0c72cf9d1a7
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2017
---
# <a name="migrating-from-aspnet-web-api"></a>La migrazione da ASP.NET Web API

Da [Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)

API Web sono servizi HTTP in grado di raggiungono una vasta gamma di client, inclusi browser e dispositivi mobili. ASP.NET MVC di base include il supporto per la compilazione di API Web che fornisce un singolo modo coerente, di compilazione di applicazioni web. In questo articolo viene descritto come i passaggi necessari per eseguire la migrazione di un'implementazione di API Web di API Web ASP.NET ad ASP.NET MVC di base.

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Progetto API Web ASP.NET di revisione

Questo articolo viene utilizzato il progetto di esempio, *ProductsApp*, creato nell'articolo [Introduzione a ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) come punto di partenza. In tale progetto, un progetto di API Web ASP.NET semplice viene configurato come indicato di seguito.

In *Global.asax.cs*, viene effettuata una chiamata a `WebApiConfig.Register`:

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig`è definito in *App_Start*, e ha solo una riga statica `Register` metodo:

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


Questa classe consente di configurare [routing degli attributi](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), anche se non è effettivamente utilizzato nel progetto. Configura inoltre la tabella di routing viene utilizzato da ASP.NET Web API. In questo caso, si otterranno gli URL in base al formato ASP.NET Web API */api/ {controller} / {id}*, con *{id}* sia facoltativo.

Il *ProductsApp* progetto include un solo controller semplice, che eredita da `ApiController` ed espone due metodi:

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Infine, il modello di *prodotto*, utilizzato per il *ProductsApp*, una classe semplice:

[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]

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

[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

Supponendo che si desidera utilizzare il routing degli attributi nel progetto in futuro, è necessaria alcuna configurazione aggiuntiva. È sufficiente applicare gli attributi in base alle esigenze per i controller e azioni, come avviene nell'esempio `ValuesController` classe inclusa nel progetto di avvio di Web API:

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

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

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

A questo punto debba essere in grado di eseguire il progetto di migrazione e passare a */api/prodotti*; e, è necessario visualizzare l'elenco completo dei 3 prodotti. Passare a */api/products/1* dovrebbe essere visualizzata prima del prodotto.

## <a name="summary"></a>Riepilogo

La migrazione di un progetto ASP.NET Web API semplice per ASP.NET MVC di base è piuttosto semplice, grazie al supporto incorporato per l'API Web di ASP.NET MVC di base. Le parti principali, che saranno necessario eseguire la migrazione di tutti i progetti ASP.NET Web API sono route, controller e i modelli, gli aggiornamenti per i tipi utilizzati dai controller e azioni.
