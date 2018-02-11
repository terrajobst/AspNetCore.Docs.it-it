---
title: Helper tag di ancoraggio
author: pkellner
description: Informazioni sugli attributi dell'helper tag di ancoraggio ASP.NET Core e sul ruolo di ogni attributo per l'estensione del comportamento del tag di ancoraggio HTML.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: f3b704174c3287edda12725b7973a2464e485bac
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2018
---
# <a name="anchor-tag-helper"></a>Helper tag di ancoraggio

Di [Peter Kellner](http://peterkellner.net) e [Scott Addie](https://github.com/scottaddie)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

L'[helper tag](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) di ancoraggio migliora il tag di ancoraggio HTML standard (`<a ... ></a>`) con l'aggiunta di nuovi attributi. Per convenzione, i nomi di attributo hanno il prefisso `asp-`. Il valore dell'attributo `href` dell'elemento di ancoraggio visualizzato dipende dai valori degli attributi `asp-`.

*SpeakerController* viene usato negli esempi in tutto il documento:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

Segue un inventario degli attributi `asp-`.

## <a name="asp-controller"></a>asp-controller

L'attributo [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) assegna il controller usato per generare l'URL. Il markup seguente elenca tutti gli altoparlanti:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspController)]

Codice HTML generato:

```html
<a href="/Speaker">All Speakers</a>
```

Se si specifica l'attributo `asp-controller` e l'attributo `asp-action` viene omesso, il valore `asp-action` predefinito è l'azione del controller associata alla visualizzazione di esecuzione corrente. Se `asp-action` viene omesso dal markup precedente e viene usato l'helper tag di ancoraggio nella visualizzazione *Index* di *HomeController* (*/Home*), il codice HTML generato è:

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>asp-action

Il valore dell'attributo [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) rappresenta il nome dell'azione del controller incluso nell'attributo `href` generato. Il markup seguente imposta il valore dell'attributo `href` generato sulla pagina delle valutazioni degli altoparlanti:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAction)]

Codice HTML generato:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Se non viene specificato alcun attributo `asp-controller` viene usato il controller predefinito per chiamare la visualizzazione che esegue la visualizzazione corrente.

Se il valore dell'attributo `asp-action` è `Index` non viene accodata alcuna azione all'URL, con conseguente chiamata dell'azione `Index` predefinita. L'azione specificata (o usata per impostazione predefinita) deve esistere nel controller a cui si fa riferimento in `asp-controller`.

## <a name="asp-route-value"></a>asp-route-{value}

L'attributo [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) abilita un prefisso di route con caratteri jolly. Qualsiasi valore che sostituisce il segnaposto `{value}` viene interpretato come potenziale parametro di route. Se non viene trovata una route predefinita, il prefisso di route viene accodato come parametro di richiesta e relativo valore all'attributo `href` generato. In caso contrario viene sostituito nel modello di route.

Esaminare l'azione del controller seguente:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

Con un modello di route predefinito definito in *Startup.Configure*:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

La visualizzazione MVC usa il modello, fornito dall'azione, come segue:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

È stata trovata una corrispondenza per il segnaposto `{id?}` della route predefinita. Codice HTML generato:

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Si supponga che il prefisso di route non faccia parte del modello di routing corrispondente, come nel caso della visualizzazione MVC seguente:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

Il codice HTML seguente viene generato perché `speakerid` non è stato trovato nella route corrispondente:

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

Se `asp-controller` o `asp-action` non vengono specificati, viene eseguita la stessa elaborazione predefinita usata nell'attributo `asp-route`.

## <a name="asp-route"></a>asp-route

L'attributo [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) viene usato per la creazione di un URL collegato direttamente a una route denominata. Mediante gli [attributi di routing](xref:mvc/controllers/routing#attribute-routing) è possibile assegnare un nome a una route come mostrato in `SpeakerController` e usarla nell'azione `Evaluations` corrispondente:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=22-24)]

Nel markup seguente l'attributo `asp-route` fa riferimento alla route denominata:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspRoute)]

L'helper tag di ancoraggio genera una route direttamente per tale azione del controller usando l'URL */Speaker/Evaluations*. Codice HTML generato:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Se è specificato `asp-controller` o `asp-action` in aggiunta a `asp-route`, la route generata potrebbe non essere quella prevista. Per evitare un conflitto di route, è consigliabile non usare `asp-route` con gli attributi `asp-controller` o `asp-action`.

## <a name="asp-all-route-data"></a>asp-all-route-data

L'attributo [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) supporta la creazione di un dizionario di coppie chiave-valore. La chiave è il nome del parametro e il valore è il valore del parametro.

Nell'esempio seguente viene inizializzato un dizionario che viene poi passato a una visualizzazione Razor. In alternativa è possibile passare i dati con il modello.

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Il codice precedente genera il codice HTML seguente:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

Il dizionario `asp-all-route-data` viene reso flat per produrre una stringa di query corrispondente ai requisiti dell'azione `Evaluations` in overload:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=26-30)]

In presenza di chiavi nel dizionario corrispondenti ai parametri di route, questi valori vengono sostituiti nella route come appropriato. Gli altri valori non corrispondenti vengono generati come parametri di richiesta.

## <a name="asp-fragment"></a>asp-fragment

L'attributo [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) definisce un frammento di URL da accodare all'URL. L'helper tag di ancoraggio aggiunge il carattere hash (#). Considerare il markup seguente:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Codice HTML generato:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

I tag hash sono utili per la compilazione di app sul lato client. Ad esempio possono essere usati per semplificare l'uso di contrassegni e la ricerca in JavaScript.

## <a name="asp-area"></a>asp-area

L'attributo [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) imposta il nome dell'area usato per impostare la route appropriata. L'esempio seguente illustra come l'attributo di area determina la modifica del mapping delle route. Se si imposta `asp-area` su "Blogs", la directory *Areas/Blogs* viene aggiunta come prefisso alle route dei controller e delle visualizzazioni associati per questo tag di ancoraggio.

* **<Nome progetto\>**
  * **wwwroot**
  * **Aree**
    * **Blog**
      * **Controller**
        * *HomeController.cs*
      * **Visualizzazioni**
        * **Home**
          * *AboutBlog.cshtml*
          * *Index.cshtml*
        * *_ViewStart.cshtml*
  * **Controller**

Data la gerarchia di directory precedente, il markup per fare riferimento al file *AboutBlog.cshtml* è:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspArea)]

Codice HTML generato:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Per garantire il funzionamento delle aree in un'app MVC, il modello di route deve includere un riferimento all'area, se esistente. Tale modello è rappresentato dal secondo parametro della chiamata del metodo `routes.MapRoute` in *Startup.Configure*:[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>asp-protocol

L'attributo [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) consente di specificare un protocollo (ad esempio `https`) nell'URL. Ad esempio:

[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]

Codice HTML generato:

```html
<a href="https://localhost/Home/About">About</a>
```

Il nome host nell'esempio è localhost, ma l'helper tag di ancoraggio usa il dominio pubblico del sito Web per generare l'URL.

## <a name="asp-host"></a>asp-host

L'attributo [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) consente di specificare un nome host nell'URL. Ad esempio:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspHost)]

Codice HTML generato:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>asp-page

L'attributo [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) viene usato con le pagine Razor. Usarlo per impostare il valore dell'attributo `href` per un tag di ancoraggio su una pagina specifica. L'URL viene creato anteponendo una barra ("/") al nome della pagina.

L'esempio seguente fa riferimento alla pagina Razor Attendee:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPage)]

Codice HTML generato:

```html
<a href="/Attendee">All Attendees</a>
```

L'attributo `asp-page` e gli attributi `asp-route`, `asp-controller` e `asp-action` si escludono a vicenda. `asp-page` può tuttavia essere usato con `asp-route-{value}` per il controllo del routing, come dimostra il markup seguente:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

Codice HTML generato:

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a>asp-page-handler

L'attributo [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) viene usato con le pagine Razor. È progettato per il collegamento di gestori di pagine specifici.

Si consideri il gestore di pagine seguente:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Il markup associato del modello di pagina si collega al gestore di pagina `OnGetProfile`. Si noti che il prefisso `On<Verb>` del nome di metodo del gestore di pagina viene omesso nel valore dell'attributo `asp-page-handler`. Il suffisso `Async` verrebbe omesso anche nel caso di un metodo asincrono.

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Codice HTML generato:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Aree](xref:mvc/controllers/areas)
* [Introduzione a Razor Pages](xref:mvc/razor-pages/index)
