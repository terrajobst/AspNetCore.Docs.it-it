---
title: Aree in ASP.NET Core
author: rick-anderson
description: Informazioni sulle aree, una funzionalità di ASP.NET MVC che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni).
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833527"
---
# <a name="areas-in-aspnet-core"></a>Aree in ASP.NET Core

Di [Dhananjay Kumar](https://twitter.com/debug_mode) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Le aree sono una funzionalità di ASP.NET che consente di organizzare le funzioni correlate in un gruppo come spazio dei nomi separato (per il routing) e struttura di cartelle (per le visualizzazioni). Usando le aree si crea una gerarchia per il routing aggiungendo un altro parametro di route, `area`, a `controller` e `action` o a una pagina Razor `page`.

Le aree consentono di suddividere un'app Web ASP.NET Core in gruppi funzionali più piccoli, ognuno con un proprio set di pagine Razor, controller, visualizzazioni e modelli. Un'area è in effetti una struttura all'interno di un'app. In un progetto Web ASP.NET Core i componenti logici come pagine, modello, controller e visualizzazione si trovano in cartelle diverse. Il runtime di ASP.NET Core crea una relazione tra questi componenti usando convenzioni di denominazione. Per un'app di grandi dimensioni può risultare utile suddividere l'app in aree di funzionalità di alto livello distinte. È il caso, ad esempio, di un'app di e-commerce con più business unit, ad esempio per il completamento della transazione, la fatturazione e la ricerca. Ognuna di queste business unit avrà la propria area in cui contenere visualizzazioni, controller, pagine Razor e modelli.

In un progetto è consigliabile usare le aree quando:

* L'app è costituita da più componenti funzionali di alto livello che possono rimanere logicamente separati.
* Si vuole suddividere l'app in modo da poter lavorare su ogni area funzionale in modo indipendente.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([procedura per il download](xref:index#how-to-download-a-sample)). Il download di esempio fornisce un'app di base per testare le aree.

## <a name="areas-for-controllers-with-views"></a>Aree per i controller con visualizzazioni

Una tipica app Web ASP.NET Core che usa aree, controller e visualizzazioni contiene quanto segue:

* Una [struttura di cartelle dell'area](#area-folder-structure).
* Controller decorati con l'attributo [&lbrack;Area&rbrack;](#attribute) per associare il controller all'area: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]
* La [route di area aggiunta all'avvio](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

## <a name="area-folder-structure"></a>Struttura di cartelle dell'area
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

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
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

### <a name="link-generation-with-areas"></a>Generazione di collegamenti con le aree

Il codice seguente dal [download di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) mostra la generazione di collegamenti con l'area specificata:

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

I collegamenti generati con il codice precedente sono validi ovunque nell'app.

Il download di esempio include una [visualizzazione parziale](xref:mvc/views/partial) che contiene i collegamenti precedenti e gli stessi collegamenti senza specificare l'area. La visualizzazione parziale viene referenziata nel [file di layout](), in modo che ogni pagina nell'app mostri i collegamenti generati. I collegamenti generati senza specificare l'area sono validi solo quando sono referenziati da una pagina nella stessa area e controller.

Quando l'area o il controller non sono specificati, il routing dipende dai valori di *ambiente*. I valori di route correnti della richiesta corrente sono considerati valori di ambiente per la generazione del collegamento. In molti casi per l'app di esempio, l'uso dei valori di ambiente genera collegamenti non corretti.

Per altre informazioni, vedere [Routing ad azioni del controller](xref:mvc/controllers/routing).

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a>Layout condiviso per le aree tramite il file _ViewStart.cshtml

Per condividere un layout comune per l'intera app, spostare *_ViewStart.cshtml* nella cartella radice dell'applicazione.

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a>Modificare la cartella dell'area predefinita in cui sono archiviate le visualizzazioni

Il codice seguente modifica la cartella dell'area predefinita da `"Areas"` a `"MyAreas"`:

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a>Pubblicazione di aree

Tutti i file `*.cshtml` e `wwwroot/**` vengono pubblicati per l'output quando `<Project Sdk="Microsoft.NET.Sdk.Web">` viene incluso nel file con estensione *csproj*.
