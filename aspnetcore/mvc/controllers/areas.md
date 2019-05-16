---
title: Aree in ASP.NET Core
author: rick-anderson
description: Informazioni sulle aree, una funzionalità di ASP.NET MVC che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni).
ms.author: riande
ms.date: 05/06/2019
uid: mvc/controllers/areas
ms.openlocfilehash: 35c7682861f7392b0bcda7326e4d7f5ccc356bda
ms.sourcegitcommit: b508b115107e0f8d7f62b25cfcc8ad45e1373459
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/07/2019
ms.locfileid: "65212597"
---
# <a name="areas-in-aspnet-core"></a>Aree in ASP.NET Core

Di [Dhananjay Kumar](https://twitter.com/debug_mode) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Le aree sono una funzionalità di ASP.NET che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni). Usando le aree si crea una gerarchia per il routing aggiungendo un altro parametro di route, `area`, a `controller` e `action` o a una pagina Razor `page`.

Le aree consentono di suddividere un'app Web ASP.NET Core in gruppi funzionali più piccoli, ognuno con un proprio set di pagine Razor, controller, visualizzazioni e modelli. Un'area è in effetti una struttura all'interno di un'app. In un progetto Web ASP.NET Core i componenti logici come pagine, modello, controller e visualizzazione si trovano in cartelle diverse. Il runtime di ASP.NET Core crea una relazione tra questi componenti usando convenzioni di denominazione. Per un'app di grandi dimensioni può risultare utile suddividere l'app in aree di funzionalità di alto livello distinte. È il caso, ad esempio, di un'app di e-commerce con più business unit, ad esempio per il completamento della transazione, la fatturazione e la ricerca. Ognuna di queste business unit avrà la propria area in cui contenere visualizzazioni, controller, pagine Razor e modelli.

In un progetto è consigliabile usare le aree quando:

* L'app è costituita da più componenti funzionali di alto livello che possono rimanere logicamente separati.
* Si vuole suddividere l'app in modo da poter lavorare su ogni area funzionale in modo indipendente.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([procedura per il download](xref:index#how-to-download-a-sample)). Il download di esempio fornisce un'app di base per testare le aree.

Se si usa Razor Pages, vedere [Aree con pagine Razor](#areas-with-razor-pages) in questo documento.

## <a name="areas-for-controllers-with-views"></a>Aree per i controller con visualizzazioni

Una tipica app Web ASP.NET Core che usa aree, controller e visualizzazioni contiene quanto segue:

* Una [struttura di cartelle dell'area](#area-folder-structure).
* I controller contrassegnati con l'attributo [&lbrack;Area&rbrack;](#attribute) per associare il controller all'area:

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* La [route di area aggiunta all'avvio](#add-area-route):

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a>Struttura di cartelle dell'area

Si consideri un'applicazione che ha due gruppi logici, *Prodotti* e *Servizi*. Usando le aree, la struttura delle cartelle sarebbe simile alla seguente:

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
          * About.cshtml
    * Servizi
      * Controllers
        * HomeController.cs
      * Visualizzazioni
        * Home
          * Index.cshtml

Anche se il layout precedente è tipico quando si usano le aree, per usare questa struttura di cartelle sono necessari solo i file delle visualizzazioni. L'individuazione delle visualizzazioni cercherà un file di visualizzazione area corrispondente in quest'ordine:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

Il percorso delle cartelle non di visualizzazioni, come *Controller* e *Modelli* **non** è rilevante. Ad esempio, le cartelle *Controller* e *Modelli* non sono necessarie. Il contenuto di *Controller* e *Modelli* è codice che viene compilato in un file DLL. Il contenuto di *Visualizzazioni* non viene compilato finché non viene effettuata una richiesta a tale visualizzazione.

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a>Associare il controller a un'area

I controller di area sono identificati dall'attributo [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute):

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a>Aggiungere una route di area

Le route di area usano in genere il routing convenzionale anziché il routing con attributi. Il routing convenzionale dipende dall'ordinamento. In generale, le route con aree devono essere posizionate prima delle altre nella tabella di route poiché sono più specifiche rispetto alle route senza un'area.

`{area:...}` può essere usato come token nei modelli di route se lo spazio URL è uniforme in tutte le aree:

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

Nel codice precedente, `exists` applica il vincolo che la route deve corrispondere a un'area. L'uso di `{area:...}` è il meccanismo meno complesso per l'aggiunta del routing alle aree.

Il codice seguente usa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> per creare due route di area denominate:

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

Quando si usa `MapAreaRoute` con ASP.NET Core 2.2, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore/issues/7772).

Per altre informazioni, vedere [Aree](xref:mvc/controllers/routing#areas).

### <a name="link-generation-with-mvc-areas"></a>Generazione di collegamenti con le aree MVC

Il codice seguente dal [download di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) mostra la generazione di collegamenti con l'area specificata:

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

I collegamenti generati con il codice precedente sono validi ovunque nell'app.

Il download di esempio include una [visualizzazione parziale](xref:mvc/views/partial) che contiene i collegamenti precedenti e gli stessi collegamenti senza specificare l'area. La visualizzazione parziale viene referenziata nel [file di layout](xref:mvc/views/layout), in modo che ogni pagina nell'app mostri i collegamenti generati. I collegamenti generati senza specificare l'area sono validi solo quando sono referenziati da una pagina nella stessa area e controller.

Quando l'area o il controller non sono specificati, il routing dipende dai valori di *ambiente*. I valori di route correnti della richiesta corrente sono considerati valori di ambiente per la generazione del collegamento. In molti casi per l'app di esempio, l'uso dei valori di ambiente genera collegamenti non corretti.

Per altre informazioni, vedere [Routing ad azioni del controller](xref:mvc/controllers/routing).

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a>Layout condiviso per le aree tramite il file _ViewStart.cshtml

Per condividere un layout comune per l'intera app, spostare *_ViewStart.cshtml* nella cartella radice dell'applicazione.

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a>Modificare la cartella dell'area predefinita in cui sono archiviate le visualizzazioni

Il codice seguente modifica la cartella dell'area predefinita da `"Areas"` a `"MyAreas"`:

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a>Aree con pagine Razor

Per le aree con pagine Razor è richiesta una cartella *Areas/&lt;nome area&gt;/Pages* nella radice dell'app. La struttura di cartelle seguente viene usata con il [download di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)

* Nome progetto
  * Aree
    * Prodotti
      * Pages
        * _ViewImports
        * Informazioni su
        * Indice
    * Servizi
      * Pages
        * Gestisci
          * Informazioni su
          * Indice

### <a name="link-generation-with-razor-pages-and-areas"></a>Generazione del collegamento con pagine Razor e aree

Il codice seguente dal [download di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) mostra la generazione di collegamenti con l'area specificata (ad esempio, `asp-area="Products"`):

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

I collegamenti generati con il codice precedente sono validi ovunque nell'app.

Il download di esempio include una [visualizzazione parziale](xref:mvc/views/partial) che contiene i collegamenti precedenti e gli stessi collegamenti senza specificare l'area. La visualizzazione parziale viene referenziata nel [file di layout](xref:mvc/views/layout), in modo che ogni pagina nell'app mostri i collegamenti generati. I collegamenti generati senza specificare l'area sono validi solo in caso di riferimento da una pagina nella stessa area.

Quando l'area non è specificata, il routing dipende dai valori di *ambiente*. I valori di route correnti della richiesta corrente sono considerati valori di ambiente per la generazione del collegamento. In molti casi per l'app di esempio, l'uso dei valori di ambiente genera collegamenti non corretti. Si considerino, ad esempio, i collegamenti generati dal codice seguente:

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

Per il codice precedente:

* Il collegamento generato da `<a asp-page="/Manage/About">` è corretto solo quando l'ultima richiesta riguarda una pagina nell'area `Services`. Ad esempio, `/Services/Manage/`, `/Services/Manage/Index` o `/Services/Manage/About`.
* Il collegamento generato da `<a asp-page="/About">` è corretto solo quando l'ultima richiesta riguarda una pagina in `/Home`.
* Il codice deriva dal [download di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).

### <a name="import-namespace-and-tag-helpers-with-viewimports-file"></a>Importare lo spazio dei nomi e gli helper tag con il file _ViewImports

È possibile aggiungere un file *_ViewImports* nella cartella *Pages* di ogni area per importare lo spazio dei nomi e gli helper tag in ogni pagina Razor nella cartella.

Prendere in considerazione l'area *Services* del codice di esempio, che non contiene un file *_ViewImports*. Il markup seguente mostra la pagina Razor */Services/Manage/About*:

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

Nel markup precedente:

* Il nome di dominio completo deve essere usato per specificare il modello (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).
* [Gli helper tag](xref:mvc/views/tag-helpers/intro) sono abilitati da `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`

Nel download di esempio, l'area Products contiene il file *_ViewImports* seguente:

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

Il markup seguente mostra la pagina Razor */Products/About*:

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

Nel file precedente lo spazio dei nomi e la direttiva `@addTagHelper` vengono importati dal file *Areas/Products/Pages/_ViewImports.cshtml*.

Per altre informazioni, vedere [Gestione dell'ambito dell'helper tag](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) e [Importazione delle direttive condivise](xref:mvc/views/layout#importing-shared-directives).

### <a name="shared-layout-for-razor-pages-areas"></a>Layout condiviso per le aree di pagine Razor

Per condividere un layout comune per l'intera app, spostare *_ViewStart.cshtml* nella cartella radice dell'applicazione.

### <a name="publishing-areas"></a>Pubblicazione di aree

Tutti i file `*.cshtml` e `wwwroot/**` vengono pubblicati per l'output quando `<Project Sdk="Microsoft.NET.Sdk.Web">` viene incluso nel file con estensione csproj*.
