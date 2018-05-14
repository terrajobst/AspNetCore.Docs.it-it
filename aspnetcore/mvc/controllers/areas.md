---
title: Aree in ASP.NET Core
author: rick-anderson
description: Informazioni sulle aree, una funzionalità di ASP.NET MVC che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni).
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/areas
ms.openlocfilehash: 61527eb350b5aba9cb37b1de5acdeae1287bf073
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="areas-in-aspnet-core"></a>Aree in ASP.NET Core

Di [Dhananjay Kumar](https://twitter.com/debug_mode) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Le aree sono una funzionalità di ASP.NET MVC che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni). Usando le aree si crea una gerarchia per il routing aggiungendo un altro parametro di route, `area`, a `controller` e `action`.

Le aree consentono di suddividere un'app Web ASP.NET Core MVC di grandi dimensioni in raggruppamenti funzionali più piccoli. Un'area è in effetti una struttura MVC all'interno di un'applicazione. In un progetto MVC i componenti logici come modello, controller e visualizzazione si trovano in cartelle diverse e MVC crea una relazione tra questi componenti tramite convenzioni di denominazione. Per un'app di grandi dimensioni può risultare utile suddividere l'app in aree di funzionalità di alto livello distinte. È il caso, ad esempio, di un'app di e-commerce con più business unit, ad esempio per il completamento della transazione, la fatturazione, la ricerca e così via. Ognuna di queste business unit avrà le proprie visualizzazioni componenti, i propri controller e i propri modelli logici. In questo scenario, è possibile usare le aree per suddividere fisicamente i componenti business all'interno dello stesso progetto.

Un'area può essere definita come un insieme di unità funzionali più piccole in un progetto ASP.NET Core MVC con un proprio set di controller, visualizzazioni e modelli.

In un progetto MVC è consigliabile usare le aree quando:

* L'applicazione è costituita da più componenti funzionali di alto livello che devono rimanere logicamente separati

* Si vuole suddividere il progetto MVC in modo da poter lavorare su ogni area funzionale in modo indipendente

Caratteristiche delle aree:

* Un'app ASP.NET Core MVC può avere un numero qualsiasi di aree

* Ogni area ha un proprio controller, nonché modelli e visualizzazioni propri

* Consente di organizzare progetti MVC di grandi dimensioni in più componenti di alto livello che è possibile usare in modo indipendente

* Supporta più controller con lo stesso nome, purché abbiano *aree* diverse

Di seguito viene presentato un esempio di creazione e di uso delle aree. Si supponga di avere un'app di vendita con due raggruppamenti distinti di controller e visualizzazioni: Products e Services. Una struttura di cartelle tipica per questo tipo di uso delle MVC è la seguente:

* Nome progetto

  * Aree

    * Prodotti

      * Controllers

        * HomeController.cs

        * ManageController.cs

      * Visualizzazioni

        * Home

          * Index.cshtml

        * Gestisci

          * Index.cshtml

    * Servizi

      * Controllers

        * HomeController.cs

      * Visualizzazioni

        * Home

          * Index.cshtml

Quando MVC tenta di eseguire il rendering di una visualizzazione in un'area, per impostazione predefinita cerca nei percorsi seguenti:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

Questi sono i percorsi predefiniti, che possono essere modificati tramite `AreaViewLocationFormats` in `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.

Nel codice seguente, ad esempio, il nome della cartella 'Areas' è stato modificato in 'Categories'.

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

Si noti che in questo esempio la struttura della cartella *Views* è l'unica considerata importante. Il contenuto delle altre cartelle, ad esempio *Controllers* e *Models* **non** è rilevante. Non è assolutamente necessario, ad esempio, che esistano le cartelle *Controllers* e *Models*. Questo procedimento funziona perché il contenuto di *Controllers* e *Models* è solo codice che viene compilato in un file con estensione dll, mentre il contenuto di *Views* non lo è finché non viene effettuata una richiesta a una visualizzazione.

Dopo aver definito la gerarchia di cartelle, è necessario informare MVC che ogni controller è associato a un'area. A tale scopo, si decora il nome del controller con l'attributo `[Area]`.

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

Impostare la definizione di una route che funzioni con le aree appena create. L'articolo [Route ad azioni del controller](routing.md) descrive in dettaglio come creare definizioni di route, nonché come usare route convenzionali rispetto a route di attributi. In questo esempio verrà usata una route convenzionale. A tale scopo, aprire il file *Startup.cs* e modificarlo aggiungendo la definizione di route denominata `areaRoute` riportata di seguito.

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

Se si passa a `http://<yourApp>/products`, viene richiamato il metodo di azione `Index` di `HomeController` nell'area `Products`.

## <a name="link-generation"></a>Generazione di collegamenti

* Generazione di collegamenti da un'azione all'interno di un controller basato su un'area a un'altra azione all'interno dello stesso controller.

  Si supponga che il percorso della richiesta corrente sia `/Products/Home/Create`

  Sintassi di HtmlHelper: `@Html.ActionLink("Go to Product's Home Page", "Index")`

  Sintassi di TagHelper: `<a asp-action="Index">Go to Product's Home Page</a>`

  Si noti che non è necessario specificare qui i valori 'area' e 'controller', perché sono già disponibili nel contesto della richiesta corrente. Valori di questo tipo sono detti valori `ambient`.

* Generazione di collegamenti da un'azione all'interno di un controller basato su un'area a un'altra azione in un controller diverso

  Si supponga che il percorso della richiesta corrente sia `/Products/Home/Create`

  Sintassi di HtmlHelper: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`

  Sintassi di TagHelper: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Si noti che qui è usato un valore ambient 'area', mentre il valore 'controller' è specificato in modo esplicito.

* Generazione di collegamenti da un'azione all'interno di un controller basato su un'area a un'altra azione in un altro controller e in un'altra area.

  Si supponga che il percorso della richiesta corrente sia `/Products/Home/Create`

  Sintassi di HtmlHelper: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`

  Sintassi di TagHelper: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`

  Si noti che in questo caso non vengono usati valori ambient.

* Generazione di collegamenti da un'azione all'interno di un controller basato su un'area a un'altra azione in un altro controller e **non** in un'area.

  Sintassi di HtmlHelper: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`

  Sintassi di TagHelper: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Poiché si vogliono generare collegamenti a un'azione del controller non basato su un'area, in questo caso il valore ambient di 'area' viene svuotato.

## <a name="publishing-areas"></a>Pubblicazione di aree

Tutti i file `*.cshtml` e `wwwroot/**` vengono pubblicati per l'output quando `<Project Sdk="Microsoft.NET.Sdk.Web">` viene incluso nel file con estensione *csproj*.
