---
title: Helper tag di ancoraggio in ASP.NET Core
author: pkellner
description: Descrive l'uso dell'helper tag di ancoraggio
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a>Helper tag di ancoraggio

Di [Peter Kellner](http://peterkellner.net) 

L'helper tag di ancoraggio migliora il tag di ancoraggio HTML (`<a ... ></a>`) con l'aggiunta di nuovi attributi. Il collegamento generato (sul tag `href`) viene creato usando i nuovi attributi. Tale URL può includere un protocollo facoltativo, ad esempio https.

Il controller altoparlante seguente viene usato negli esempi di questo documento.

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Attributi dell'helper tag di ancoraggio

### <a name="asp-controller"></a>asp-controller

`asp-controller` viene usato per associare il controller che verrà usato per generare l'URL. I controller specificati devono esistere nel progetto corrente. Il codice seguente elenca tutti gli altoparlanti: 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

Il markup generato è il seguente:

```html
<a href="/Speaker">All Speakers</a>
```

Se `asp-controller` è specificato e `asp-action` non è specificato, l'elemento `asp-action` predefinito è il metodo controller predefinito della visualizzazione attualmente in esecuzione. Nell'esempio precedente, se `asp-action` viene escluso e questo helper tag di ancoraggio viene generato dalla vista `Index` di *HomeController* (**/Home**), il markup generato sarà il seguente:

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>asp-action

`asp-action` è il nome del metodo di azione nel controller che verrà incluso nell'elemento `href` generato. Ad esempio, il codice seguente imposta l'elemento `href` generato in modo che faccia riferimento alla pagina dei dettagli dell'altoparlante:

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

Il markup generato è il seguente:

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Se non è specificato nessun attributo `asp-controller` viene usato il controller predefinito per chiamare la visualizzazione che esegue la visualizzazione corrente.  
 
Se l'attributo `asp-action` è `Index` non viene accodata nessuna azione all'URL e di conseguenza viene chiamato il metodo `Index` predefinito. L'azione specificata (o usata per impostazione predefinita) deve esistere nel controller a cui si fa riferimento in `asp-controller`.

### <a name="asp-page"></a>asp-page

Usare l'attributo `asp-page` in un tag di ancoraggio per impostarne l'URL in modo che indichi una pagina specifica. L'URL viene creato facendo precedere al nome della pagina un carattere barra "/". L'URL dell'esempio seguente fa riferimento alla pagina "Speaker" nella directory corrente.

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

L'attributo `asp-page` nell'esempio di codice precedente esegue il rendering dell'output HTML nella visualizzazione, con risultati simili al frammento seguente:

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

L'attributo `asp-page` e gli attributi `asp-route`, `asp-controller` e `asp-action` si escludono a vicenda. `asp-page` può tuttavia essere usato con `asp-route-id` per il controllo del routing, come visualizzato nell'esempio di codice seguente:

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

`asp-route-id` produce l'output seguente:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> Per l'uso dell'attributo `asp-page` in Razor Pages gli URL devono essere percorsi relativi, ad esempio `"./Speaker"`. I percorsi relativi nell'attributo `asp-page` non sono disponibili nelle visualizzazioni MVC. Per le visualizzazioni MVC usare la sintassi "/".

### <a name="asp-route-value"></a>asp-route-{value}

`asp-route-` è un prefisso di route con caratteri jolly. Qualsiasi valore inserito dopo il trattino finale viene interpretato come un potenziale parametro di route. Se non viene trovata una route predefinita, il prefisso di route viene accodato come parametro di richiesta e relativo valore all'elemento href generato. In caso contrario viene sostituito nel modello di route.

Se ad esempio è presente un metodo del controller definito come segue:

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

E il modello di route predefinito in *Startup.cs* è definito come segue:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

Il file con estensione **cshtml** contenente l'helper tag di ancoraggio necessario per usare il parametro modello **speaker** passato dal controller alla vista ha la struttura seguente:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Il codice HTML generato risultante sarà il seguente, perché nella route predefinita è stato rilevato **id**.

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Se il prefisso di route non è incluso nel modello di routing rilevato, come ad esempio nel file con estensione **cshtml** seguente:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Il codice HTML generato risultante sarà il seguente, perché **speakerid** non è stato rilevato nella route corrispondente:

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Se `asp-controller` o `asp-action` non vengono specificati, viene eseguita la stessa elaborazione predefinita usata nell'attributo `asp-route`.

### <a name="asp-route"></a>asp-route

`asp-route` consente di creare un URL che esegue il collegamento diretto a una route con nome. Mediante gli attributi di routing è possibile assegnare un nome a una route come visualizzato in `SpeakerController` e usarla nel metodo `Evaluations` corrispondente.

`Name = "speakerevals"` richiede all'helper tag di ancoraggio di generare una route diretta a tale metodo del controller usando l'URL `/Speaker/Evaluations`. Se è specificato `asp-controller` o `asp-action` in aggiunta a `asp-route`, la route generata potrebbe non essere quella prevista. Evitare l'uso di `asp-route` con `asp-controller` o `asp-action` per non creare un conflitto di route.

### <a name="asp-all-route-data"></a>asp-all-route-data

`asp-all-route-data` consente la creazione di un dizionario di coppie di valori chiave in cui la chiave è il nome del parametro e il valore è il valore associato a tale chiave.

Come illustrato nell'esempio seguente, viene creato un dizionario inline e i dati vengono passati alla visualizzazione Razor. In alternativa è possibile passare i dati con il modello.

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

Il codice precedente genera l'URL seguente: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

Quando si fa clic sul collegamento viene chiamato il metodo del controller `EvaluationsCurrent`. Il metodo viene chiamato perché il controller presenta due parametri stringa che corrispondono a ciò che è stato creato dal dizionario `asp-all-route-data`.

Se una o più chiavi nel dizionario corrispondono a parametri della route, tali valori vengono sostituiti nella route come previsto e gli altri valori non corrispondenti vengono generati come parametri di richiesta.

### <a name="asp-fragment"></a>asp-fragment

`asp-fragment` definisce un frammento di URL da accodare all'URL. L'helper tag di ancoraggio aggiunge il carattere hash (#). Se si crea un tag:

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

L'URL generato è: http://localhost/Speaker/Evaluations#SpeakerEvaluations

I tag hash sono utili per la compilazione di applicazioni lato client. Ad esempio possono essere usati per semplificare l'uso di contrassegni e la ricerca in JavaScript.

### <a name="asp-area"></a>asp-area

`asp-area` imposta il nome di area usato da ASP.NET Core per impostare la route appropriata. Gli esempi che seguono visualizzano come l'attributo di area determina la modifica del mapping delle route. Se si imposta `asp-area` su Blogs, la directory `Areas/Blogs` viene aggiunta come prefisso alle route dei controller e delle visualizzazioni associate di questo tag di ancoraggio.

* Nome progetto
  * wwwroot
  * Aree
    * Blog
      * Controller
        * HomeController.cs
      * Visualizzazioni
        * Home
          * Index.cshtml
          * AboutBlog.cshtml
  * Controller

Se viene specificato un tag di area valido, ad esempio ```area="Blogs"```, per il riferimento al file ```AboutBlog.cshtml``` si ottiene un risultato simile al seguente usando l'helper tag di ancoraggio.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

Il codice HTML generato include il segmento delle aree e ha l'aspetto seguente:

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Per garantire il funzionamento delle aree MVC in un'applicazione Web, il modello di route deve includere un riferimento all'area, se esistente. Tale modello, che è il secondo parametro della chiamata del metodo `routes.MapRoute`, viene visualizzato come segue: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>asp-protocol

`asp-protocol` consente di specificare un protocollo (ad esempio `https`) nell'URL. Un esempio di helper tag di ancoraggio che include il protocollo è il seguente:

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

e genera il codice HTML seguente:

```<a href="https://localhost/Home/About">About</a>```

Nell'esempio il dominio è localhost, ma l'helper tag di ancoraggio usa il dominio pubblico del sito Web per generare l'URL.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Aree](xref:mvc/controllers/areas)
