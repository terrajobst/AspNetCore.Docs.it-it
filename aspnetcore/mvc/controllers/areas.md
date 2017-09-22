---
title: Aree
author: rick-anderson
description: Viene illustrato come utilizzare le aree.
keywords: ASP.NET Core, aree, routing, le visualizzazioni
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 5e16d5e8-5696-4cb2-8ec7-d36be305c922
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: 0f388ba090ada11a0ac7937606cbcd5a89d6263e
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="areas"></a>Aree

Da [Dhananjay Kumar](https://twitter.com/debug_mode) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Le aree sono una funzionalità di ASP.NET MVC consentono di organizzare la funzionalità correlata in un gruppo come una struttura di cartelle (per le viste) e un spazio dei nomi separato (per il routing). Utilizzo di aree consente di creare una gerarchia allo scopo di routing mediante l'aggiunta di un altro parametro di route, `area`a `controller` e `action`.

Le aree consentono di suddividere un'app Web ASP.NET MVC di base di grandi dimensioni in raggruppamenti funzionali più piccoli. Un'area è in effetti una struttura MVC all'interno di un'applicazione. In un progetto MVC, vengono mantenuti i componenti logici come modello, Controller e visualizza in cartelle diverse e MVC utilizza le convenzioni di denominazione per creare la relazione tra questi componenti. Per un'app di grandi dimensioni, può risultare utile per partizionare l'app in varie aree di livello elevate di funzionalità. Ad esempio, un'applicazione di e-commerce con più unità aziendali, ad esempio l'estrazione, fatturazione e la ricerca e così via. Ognuna di queste unità sono le proprie viste componenti logici, controller e i modelli. In questo scenario, è possibile utilizzare aree per suddividere fisicamente i componenti business nello stesso progetto.

Un'area può essere definita come unità funzionale più piccole in un progetto ASP.NET MVC di base con un proprio set di modelli, visualizzazioni e controller.

È consigliabile utilizzare le aree in MVC progetto quando:

* L'applicazione è costituita da più componenti funzionali di alto livello che devono essere separati in modo logico

* Si desidera partizionare il progetto MVC in modo che ogni area funzionale possa essere elaborata in modo indipendente

Funzionalità area:

* Un'applicazione ASP.NET MVC di base può avere qualsiasi numero di aree

* Ogni area dispone di un proprio controller, modelli e visualizzazioni

* Consente di organizzare i progetti MVC di grandi dimensioni in più componenti di alto livello che possono essere svolte in modo indipendente

* Supporta più controller con lo stesso nome, purché hanno diversi *aree*

Esaminiamo un esempio per illustrare come aree vengono create e utilizzate. Si potrebbe avere una app store con due gruppi distinti di controller e visualizzazioni: prodotti e servizi. Una tipico cartella struttura che utilizza aree MVC è simile al seguente:

* Nome progetto

  * Aree

    * Prodotti

      * Controller

        * HomeController.cs

        * ManageController.cs

      * Visualizzazioni

        * Home

          * Cshtml

        * Gestisci

          * Cshtml

    * Servizi

      * Controller

        * HomeController.cs

      * Visualizzazioni

        * Home

          * Cshtml

Quando MVC tenta di eseguire il rendering di una vista in un'Area, per impostazione predefinita, tenta di ricerca nei percorsi seguenti:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

Questi sono i percorsi predefiniti che possono essere modificati tramite il `AreaViewLocationFormats` sul `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.

Ad esempio, nel seguente codice anziché il nome della cartella come 'Aree', che è stato modificato per 'Categorie'.

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

Si noti che sono la struttura del *viste* cartella è l'unico che è considerata importante qui e il contenuto della parte restante delle cartelle come *controller* e *modelli* does **non** è rilevante. Ad esempio, è necessario non è un *controller* e *modelli* cartella affatto. Questo procedimento funziona perché il contenuto di *controller* e *modelli* è solo il codice che viene compilato in una DLL in cui come contenuto del *viste* non è presente fino a quando una richiesta a quella visualizzazione è stata trovata.

Dopo aver definito la gerarchia di cartelle, è necessario che ogni controller è associata a un'area per MVC. A tale scopo, aggiungendo il nome del controller con la `[Area]` attributo.

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [4]}} -->

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

Consente di impostare una definizione della route che funziona con le aree appena create. Il [Routing alle azioni del Controller](routing.md) articolo si analizzano in dettaglio come creare definizioni di route, incluso l'utilizzo convenzionale route rispetto route di attributi. In questo esempio, si userà una route convenzionale. A tale scopo, aprire il *Startup.cs* file e modificarlo aggiungendovi il `areaRoute` denominato definizione route riportata di seguito.

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [4, 5, 6]}} -->

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(name: "areaRoute",
       template: "{area:exists}/{controller=Home}/{action=Index}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

La selezione di `http://<yourApp>/products`, `Index` il metodo di azione del `HomeController` nel `Products` area verrà richiamata.

## <a name="link-generation"></a>Generazione di collegamento

* Generazione di collegamenti da un'azione all'interno di un'area in base a un'altra azione all'interno del controller stesso controller di.

  Si supponga che il percorso della richiesta corrente è simile a`/Products/Home/Create`

  Sintassi HtmlHelper:`@Html.ActionLink("Go to Product's Home Page", "Index")`

  Sintassi di helper tag:`<a asp-action="Index">Go to Product's Home Page</a>`

  Si noti che non sia base fornire i valori "area" e "controller" di seguito sono già disponibili nel contesto della richiesta corrente. Questo tipo di valori denominati `ambient` valori.

* Generazione di collegamenti da un'azione all'interno di un'area in base a un'altra azione su un controller diverso controller di

  Si supponga che il percorso della richiesta corrente è simile a`/Products/Home/Create`

  Sintassi HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`

  Sintassi di helper tag:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  Si noti che questo ambito viene usato il valore di ambiente di un'area' ', ma è specificato in modo esplicito il valore "controller" sopra.

* Generazione di collegamenti da un'azione all'interno di un'area in base all'azione di un altro controller di un altro controller e un'area diversa.

  Si supponga che il percorso della richiesta corrente è simile a`/Products/Home/Create`

  Sintassi HtmlHelper:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`

  Sintassi di helper tag:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`

  Si noti che in questo caso in cui non vengono utilizzati valori di ambiente.

* Generazione di collegamenti da un'azione all'interno di un controller di area in base a un'altra azione su un altro controller e **non** in un'area.

  Sintassi HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`

  Sintassi di helper tag:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  Poiché si desidera generare collegamenti a un'area di non-azione del controller, è vuoto in base al valore di ambiente 'area' qui.

## <a name="publishing-areas"></a>Aree di pubblicazione

Tutti `*.cshtml` e `wwwroot/**` i file vengono pubblicati di output quando `<Project Sdk="Microsoft.NET.Sdk.Web">` è incluso nel *csproj* file.
