---
title: Helper di Tag di ancoraggio | Documenti Microsoft
author: pkellner
description: Di seguito viene illustrato l'utilizzo di Helper di Tag di ancoraggio
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 74609b515936ec7da8bfc133c27cb69f51311924
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="anchor-tag-helper"></a>Helper di Tag di ancoraggio

Di [Peter Kellner](http://peterkellner.net) 

L'Helper di Tag di ancoraggio migliora il punto di ancoraggio HTML (`<a ... ></a>`) tag aggiungendo nuovi attributi. Il collegamento generato (sul `href` tag) viene creato utilizzando i nuovi attributi. Tale URL può includere un protocollo facoltativi, ad esempio https.

Negli esempi in questo documento viene usato il controller di voce riportata di seguito.

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Attributi Helper di Tag di ancoraggio

### <a name="asp-controller"></a>ASP controller

`asp-controller`viene utilizzato per associare il controller verrà utilizzato per generare l'URL. Il controller specificato deve essere presente nel progetto corrente. Il codice seguente vengono elencati tutti gli altoparlanti: 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

Il markup generato sarà:

```html
<a href="/Speaker">All Speakers</a>
```

Se il `asp-controller` specificato e `asp-action` non è, il valore predefinito `asp-action` sarà metodo controller predefinito per la visualizzazione attualmente in esecuzione. Che nell'esempio precedente, se viene `asp-action` viene lasciata fuori, e viene generato questo Helper di Tag di ancoraggio da *HomeController*del `Index` vista (**/Home**), il markup generato sarà:

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>azione di ASP

`asp-action`è il nome del metodo di azione nel controller che verranno inclusi nell'oggetto generato `href`. Ad esempio, il codice seguente imposta generato `href` in modo che punti alla pagina di dettaglio del relatore:

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

Il markup generato sarà:

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Se non `asp-controller` attributo viene specificato, verrà utilizzato il controller predefinito chiamando la visualizzazione di cui eseguire la vista corrente.  
 
Se l'attributo `asp-action` è `Index`, nessuna azione viene aggiunto all'URL, determinando il valore predefinito `Index` metodo chiamato. L'azione specificata (o per impostazione predefinita), deve esistere nel controller a cui fa riferimento `asp-controller`.

### <a name="asp-page"></a>pagina ASP

Utilizzare il `asp-page` attributo in un tag di ancoraggio per impostare il relativo URL in modo che punti a una pagina specifica. Prefisso per il nome di pagina con una barra "/" crea l'URL. Nell'esempio seguente l'URL punta alla pagina "Voce" nella directory corrente.

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

Il `asp-page` attributo nell'esempio di codice precedente viene eseguito il rendering di output HTML nella visualizzazione è simile al frammento riportato di seguito:

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

Il `asp-page` attributo si escludono a vicenda con la `asp-route`, `asp-controller`, e `asp-action` gli attributi. Tuttavia, `asp-page` può essere utilizzato con `asp-route-id` per controllare il routing, come illustrato nell'esempio di codice seguente:

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

Il `asp-route-id` produce il seguente output:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> Utilizzare il `asp-page` attributo nelle pagine Razor, l'URL deve essere un percorso relativo, ad esempio `"./Speaker"`. I percorsi relativi nel `asp-page` attributo non sono disponibili nelle visualizzazioni MVC. Utilizzare la sintassi "/" per le visualizzazioni MVC.

### <a name="asp-route-value"></a>asp-route-{value}

`asp-route-`è un prefisso di route con caratteri jolly. Qualsiasi valore inserito dopo il trattino finale verrà interpretato come un parametro di route possibili. Se non viene trovata una route predefinita, per href generato come valore di parametro di richiesta e verrà aggiunto il prefisso della route. In caso contrario verrà sostituito nel modello di route.

Presupponendo che sia un metodo del controller definiti come segue:

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

E che il modello di route predefiniti definito nel *Startup.cs* come indicato di seguito:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

Il **cshtml** file che contiene l'Helper di Tag di ancoraggio necessarie per l'utilizzo di **altoparlante** parametro di modello passato dal controller per la visualizzazione è indicato di seguito:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Il codice HTML generato verrà quindi modificato come segue perché **id** è stato trovato nella route predefinita.

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Se il prefisso di route non fa parte del modello di routing trovato, che si verifica con i seguenti **cshtml** file:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Il codice HTML generato verrà quindi modificato come segue perché **speakerid** non è stato trovato nella route corrispondenza:

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Se il valore `asp-controller` o `asp-action` non vengono specificati, viene seguito l'elaborazione predefinita stesso in cui è nel `asp-route` attributo.

### <a name="asp-route"></a>route di ASP

`asp-route`fornisce un modo per creare un URL che colleghi direttamente a una route denominata. Gli attributi di routing una route può essere denominata come illustrato nel `SpeakerController` e utilizzato nel relativo `Evaluations` metodo.

`Name = "speakerevals"`indica l'Helper di Tag di ancoraggio per generare una route direttamente a tale metodo controller utilizzando l'URL `/Speaker/Evaluations`. Se `asp-controller` o `asp-action` è specificato in aggiunta al `asp-route`, la route generata potrebbe non essere quello previsto. `asp-route`non devono essere usate con uno degli attributi `asp-controller` o `asp-action` per evitare un conflitto di route.

### <a name="asp-all-route-data"></a>asp-all-route-data

`asp-all-route-data`Consente di creare un dizionario di coppie chiave-valore in cui la chiave è il nome del parametro e il valore è il valore associato a tale chiave.

Come nell'esempio seguente viene illustrato, viene creato un dizionario inline e i dati vengono passati alla visualizzazione razor. In alternativa, i dati potrebbero essere anche passati con il modello.

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

Il codice precedente genera il seguente URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

Quando viene selezionato il collegamento, il metodo controller `EvaluationsCurrent` viene chiamato. Viene chiamato perché tale controller presenta due parametri di stringa che corrispondono a ciò che è stato creato dal `asp-all-route-data` dizionario.

Se le chiavi nella corrispondenza dizionario parametri della route, tali valori saranno sostituiti nella route come appropriato e gli altri valori non corrispondenti verranno generati come parametri di richiesta.

### <a name="asp-fragment"></a>frammento di ASP

`asp-fragment`definisce un frammento di URL da aggiungere all'URL. L'Helper di Tag di ancoraggio aggiungerà il cancelletto (#). Se si crea un tag:

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

L'URL generato sarà: http://localhost/Speaker/Evaluations#SpeakerEvaluations

Tag di hash sono utili quando si compilano applicazioni sul lato client. Possono essere utilizzati per contrassegnare e la ricerca in JavaScript, ad esempio semplice.

### <a name="asp-area"></a>asp-area

`asp-area`Imposta il nome dell'area che ASP.NET Core viene utilizzato per impostare la route appropriata. Di seguito sono riportati esempi di come l'attributo area causa una modifica del mapping delle route. Impostazione `asp-area` ai blog prefissi di directory `Areas/Blogs` a tutte le route delle controller associato e le viste per il tag di ancoraggio.

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

Specificare un tag area valido, ad esempio ```area="Blogs"``` quando si fa riferimento il ```AboutBlog.cshtml``` file sarà simile al seguente utilizzando l'Helper di Tag di ancoraggio.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

Il codice HTML generato includerà il segmento di aree e verrà modificato come segue:

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Per le aree MVC funzionare in un'applicazione web, il modello di route deve includere un riferimento all'area, se esiste. Modello, che è il secondo parametro del `routes.MapRoute` chiamata al metodo, verrà visualizzato come:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>protocollo di ASP

Il `asp-protocol` consente di specificare un protocollo (ad esempio `https`) nell'indirizzo URL. Un esempio di Helper di Tag di ancoraggio che include il protocollo apparirà come segue:

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

e genererà HTML come indicato di seguito:

```<a href="https://localhost/Home/About">About</a>```

Nell'esempio il dominio è localhost, ma l'Helper di Tag di ancoraggio utilizza dominio pubblico del sito Web durante la generazione dell'URL.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Aree](xref:mvc/controllers/areas)
