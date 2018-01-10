---
title: Panoramica di ASP.NET MVC
author: ardalis
description: "Informazioni su come Core di ASP.NET MVC è un framework completo per la compilazione di applicazioni web e modello di progettazione utilizzando Model-View-Controller API."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 01/08/2018
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 33c293e15c0a7f18bbace9dc564fe11d93a7d509
ms.sourcegitcommit: df2157ae9aeea0075772719c29784425c783e82a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/10/2018
---
# <a name="overview-of-aspnet-core-mvc"></a>Panoramica di ASP.NET MVC

Da [Steve Smith](https://ardalis.com/)

Componenti di base di ASP.NET MVC è un framework completo per la compilazione di applicazioni web e modello di progettazione utilizzando Model-View-Controller API.

## <a name="what-is-the-mvc-pattern"></a>Che cos'è il modello MVC?

Il modello architetturale Model-View-Controller (MVC) suddivide un'applicazione in tre gruppi principali di componenti: modelli, visualizzazioni e controller. Questo modello consente di ottenere [separazione delle problematiche](http://deviq.com/separation-of-concerns/). Usando questo modello, le richieste degli utenti vengono indirizzate a un Controller che è responsabile per l'utilizzo del modello per eseguire le azioni dell'utente e/o recuperare i risultati delle query. Il Controller sceglie visualizzazione da mostrare all'utente e fornisce i dati del modello che richiede.

Il diagramma seguente illustra i tre componenti principali e quelle che fanno riferimento gli altri:

![Modello MVC](overview/_static/mvc.png)

Questo limite delle responsabilità consente di scalare l'applicazione in termini di complessità, perché è più facile da codice, eseguire il debug e testare un elemento (modello, visualizzazione o controller) con un singolo processo (e segue il [singolo principio di responsabilità ](http://deviq.com/single-responsibility-principle/)). È più difficile da aggiornare, test e il debug del codice con dipendenze distribuiti in due o più di queste tre aree. Ad esempio, logica dell'interfaccia utente tende a cambiano più frequentemente logica di business. Se la logica di presentazione di codice e di business viene combinata in un singolo oggetto, è necessario modificare un oggetto che contiene la logica di business, ogni volta che si modifica l'interfaccia utente. Questo problema può causare errori e richiede la riesecuzione del test di tutte le regole di business dopo ogni interfaccia utente minima modificato.

> [!NOTE]
> Sia la visualizzazione e il controller dipendono dal modello. Tuttavia, il modello dipende la vista né il controller. Questo è uno dei principali vantaggi di separazione. Questa separazione consente la presentazione visiva indipendente da compilare e testare il modello.

### <a name="model-responsibilities"></a>Responsabilità di modello

Il modello in un'applicazione MVC rappresenta lo stato dell'applicazione e qualsiasi logica di business o di operazioni che devono essere eseguite da esso. Logica di business deve essere incapsulata nel modello, insieme a qualsiasi logica di implementazione per la persistenza dello stato dell'applicazione. Visualizzazioni fortemente tipizzate utilizzano in genere i tipi di ViewModel progettati per contenere i dati da visualizzare per tale vista. Il controller crea e popola le istanze di ViewModel dal modello.

> [!NOTE]
> Esistono diversi modi per organizzare il modello in un'applicazione che utilizza il modello architetturale MVC. Altre informazioni su alcune [diversi tipi di modello](http://deviq.com/kinds-of-models/).

### <a name="view-responsibilities"></a>Visualizza responsabilità

Le visualizzazioni sono responsabili per la presentazione di contenuto tramite l'interfaccia utente. Usano il [motore di visualizzazione Razor](#razor-view-engine) per incorporare il codice .NET nel markup HTML. Dovrebbe esserci minimo logica all'interno di viste e qualsiasi logica in essi contenuti devono essere correlati alla presentazione di contenuto. Se si rileva la necessità di eseguire una notevole della logica di visualizzazione di file per visualizzare i dati da un modello complesso, è consigliabile utilizzare un [del componente di visualizzazione](views/view-components.md), ViewModel, o del modello di visualizzazione per semplificare la visualizzazione.

### <a name="controller-responsibilities"></a>Responsabilità controller

I controller sono i componenti che gestiscono l'interazione dell'utente, utilizzano il modello e selezionano infine una visualizzazione per il rendering. In un'applicazione MVC, Visualizza solo informazioni; il controller gestisce e risponde all'input dell'utente e l'interazione. Nel modello MVC, è il punto di ingresso iniziale, il controller ed è responsabile per selezionare il modello di tipi da utilizzare e la visualizzazione per il rendering (pertanto il relativo nome - controlla la modalità dell'app risponde a una determinata richiesta).

> [!NOTE]
> Controller dovrebbero non essere eccessivamente complessa da un numero eccessivo di responsabilità. Per mantenere la logica di controller di diventare eccessivamente complessa, utilizzare il [singolo responsabilità principio](http://deviq.com/single-responsibility-principle/) alla logica di business di push fuori il controller e nel modello di dominio.

>[!TIP]
> Se si ritiene che le azioni del controller eseguono frequentemente gli stessi tipi di azioni, è possibile seguire il [non ripetere manualmente principio](http://deviq.com/don-t-repeat-yourself/) spostando queste azioni comuni in [filtri](#filters).

## <a name="what-is-aspnet-core-mvc"></a>Che cos'è ASP.NET MVC di base

Il framework ASP.NET MVC di base è un semplice open source, il framework di presentazione testabili altamente ottimizzato per l'utilizzo con ASP.NET Core.

Componenti di base di ASP.NET MVC fornisce basate su modelli consentono di creare siti Web dinamici che consente una netta separazione delle problematiche. Fornisce un controllo completo sul markup, supporta basto e utilizza gli standard web più recenti.

## <a name="features"></a>Funzionalità

Componenti di base di ASP.NET MVC include quanto segue:

* [Routing](#routing)
* [Associazione di modelli](#model-binding)
* [Convalida modello](#model-validation)
* [Inserimento di dipendenze](../fundamentals/dependency-injection.md)
* [Filtri](#filters)
* [Aree](#areas)
* [API Web](#web-apis)
* [Testabilità](#testability)
* [Motore di visualizzazione Razor](#razor-view-engine)
* [Visualizzazioni fortemente tipizzate](#strongly-typed-views)
* [Helper tag](#tag-helpers)
* [Visualizza i componenti](#view-components)

### <a name="routing"></a>Routing

ASP.NET MVC di base viene compilato in cima [routing ASP.NET Core](../fundamentals/routing.md), un componente di mapping di URL potente che consente di compilare applicazioni con URL comprensibili e disponibile per la ricerca. In questo modo è possibile definire URL criteri di denominazione dell'applicazione che funzionano correttamente per l'ottimizzazione del motore di ricerca (SEO) e per la generazione di collegamento, senza tener conto di come vengono organizzati i file sul server web. È possibile definire i percorsi utilizzando una sintassi di modello di route pratico che supporta i vincoli di valore di route, i valori predefiniti e i valori facoltativi.

*Basato sulle convenzioni di routing* i formati di globale definiscono l'URL che consente l'applicazione accetta e ciascuna di queste modalità di formattazione viene eseguito il mapping a un metodo di azione specifica in specificato controller. Quando viene ricevuta una richiesta in ingresso, il motore di routing analizza l'URL corrisponde a uno dei formati di URL definiti e quindi chiama il metodo di azione del controller associato.

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*Attributo routing* consente di specificare le informazioni di routing dichiarando i controller e azioni con attributi che definiscono la route dell'applicazione. Ciò significa che le definizioni route vengono posizionate accanto al controller e l'azione con cui si associato.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a>Associazione di modelli

Componenti di base di ASP.NET MVC [associazione del modello](models/model-binding.md) converte i dati di richiesta del client (i valori del form, dati della route, i parametri di stringa di query, le intestazioni HTTP) in oggetti in grado di gestire il controller. Di conseguenza, la logica del controller non è necessaria l'operazione di capire i dati della richiesta in ingresso; è semplicemente i dati come parametri per i metodi di azione.

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>Convalida modello

Componenti di base ASP.NET MVC supporta [convalida](models/validation.md) dichiarando l'oggetto modello con gli attributi di convalida di dati dell'annotazione. Gli attributi di convalida vengono controllate sul lato client prima che i valori vengono inviati al server, nonché nel server prima di azione del controller viene chiamato.

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

Un'azione del controller:

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

Il framework gestisce convalida dei dati richiesta sul client e nel server. La logica di convalida specificata sui tipi di modello viene aggiunto alle viste sottoposto a rendering come annotazioni non intrusivi e viene applicata nei browser con [convalida jQuery](https://jqueryvalidation.org/).

### <a name="dependency-injection"></a>Inserimento di dipendenze

ASP.NET di base include il supporto incorporato per [inserimento di dipendenze (DI)](../fundamentals/dependency-injection.md). In ASP.NET MVC di base, [controller](controllers/dependency-injection.md) possibile richiesta necessari servizi tramite i relativi costruttori, consentendo loro di seguire il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/).

Inoltre è possibile utilizzare l'app [inserimento di dipendenze di file nella visualizzazione](views/dependency-injection.md), usando il `@inject` direttiva:

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a>Filtri

[I filtri](controllers/filters.md) consentono agli sviluppatori di incapsulare i problemi di montaggio incrociato, ad esempio la gestione delle eccezioni o autorizzazione. Filtri di abilitare la logica personalizzata in esecuzione di pre e post-elaborazione per i metodi di azione e possono essere configurati per l'esecuzione in determinati punti all'interno della pipeline di esecuzione per una determinata richiesta. Filtri possono essere applicati ai controller o azioni come attributi (o possono essere eseguiti a livello globale). Alcuni filtri (ad esempio `Authorize`) sono inclusi in framework.


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a>Aree

[Aree](controllers/areas.md) consentono di suddividere un'app Web ASP.NET MVC di base di grandi dimensioni in raggruppamenti funzionali più piccoli. Un'area è una struttura MVC all'interno di un'applicazione. In un progetto MVC, vengono mantenuti i componenti logici come modello, Controller e visualizza in cartelle diverse e MVC utilizza le convenzioni di denominazione per creare la relazione tra questi componenti. Per un'app di grandi dimensioni, può risultare utile per partizionare l'app in varie aree di livello elevate di funzionalità. Ad esempio, un'applicazione di e-commerce con più unità aziendali, ad esempio l'estrazione, fatturazione e la ricerca e così via. Ognuna di queste unità sono le proprie viste componenti logici, controller e i modelli.

### <a name="web-apis"></a>API Web

Oltre a essere una piattaforma ideale per la compilazione di siti web, ASP.NET MVC di base dispone di supporto ottimale per la compilazione di API Web. È possibile compilare servizi raggiungono una vasta gamma di client, inclusi browser e dispositivi mobili.

Il framework include il supporto per la negoziazione del contenuto HTTP con supporto incorporato per [la formattazione dei dati](models/formatting.md) come JSON o XML. Scrivere [formattatori personalizzati](advanced/custom-formatters.md) per aggiungere supporto per formati personalizzati.

Utilizzare la generazione di collegamenti per abilitare il supporto per hypermedia. Attivare con facilità il supporto per [risorse multiorigine (CORS) di condivisione](http://www.w3.org/TR/cors/) in modo che le API Web può essere condiviso tra più applicazioni Web.

### <a name="testability"></a>Testabilità

Utilizzo del framework di interfacce e inserimento di dipendenze renderlo particolarmente adatto agli unit test e il framework include funzionalità (ad esempio, un provider TestHost e InMemory per Entity Framework) che rendono [test di integrazione](../testing/integration-testing.md) rapido semplice e oltre. Altre informazioni, vedere [logica del controller di test](controllers/testing.md).

### <a name="razor-view-engine"></a>Motore di visualizzazione Razor

[ASP.NET MVC Core views](views/overview.md) utilizzare il [motore di visualizzazione Razor](views/razor.md) per il rendering di visualizzazioni. Razor è un linguaggio di markup modello compact, espressivo e fluida per la definizione di viste usando codice c# incorporato. Razor viene utilizzato per generare dinamicamente il contenuto web nel server. Normalmente, è possibile combinare codice lato server con il codice e contenuto sul lato client.

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

Tramite il motore di visualizzazione Razor è possibile definire [layout](views/layout.md), [visualizzazioni parziali](views/partial.md) e sezioni sostituibili.

### <a name="strongly-typed-views"></a>Visualizzazioni fortemente tipizzate

Visualizzazioni Razor in MVC possono essere fortemente tipizzate in base al modello. Controller è possono passare un modello fortemente tipizzato per abilitare le viste di controllo dei tipi e IntelliSense supporta le viste.

Ad esempio, la vista seguente esegue il rendering di un modello di tipo `IEnumerable<Product>`:

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>Helper di tag

[Gli helper di tag](views/tag-helpers/intro.md) consentire al codice lato server partecipare alla creazione e il rendering di elementi HTML in file Razor. È possibile utilizzare gli helper di tag per definire i tag personalizzati (ad esempio, `<environment>`) o per modificare il comportamento dei tag esistenti (ad esempio, `<label>`). Eseguire il binding gli helper di tag a elementi specifici in base al nome di elemento e i relativi attributi. Offrono i vantaggi del rendering lato server mantenendo al tempo stesso un'esperienza di modifica HTML.

Vi sono molti gli helper di Tag predefiniti per le attività comuni, ad esempio la creazione di moduli, collegamenti, asset durante il caricamento e più - e ancora più disponibili nel repository GitHub pubblici e come NuGet pacchetti. Vengono creati gli helper di tag in c# e di destinazione gli elementi HTML in base al nome di elemento, il nome dell'attributo o tag padre. Ad esempio, l'oggetto incorporato LinkTagHelper può essere utilizzato per creare un collegamento al `Login` azione del `AccountsController`:

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

Il `EnvironmentTagHelper` può essere utilizzato per includere script differenti nelle visualizzazioni (ad esempio, non elaborato o minimizzata) in base all'ambiente di runtime, ad esempio sviluppo, gestione temporanea o produzione:

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

Gli helper di tag forniscono un'esperienza di sviluppo HTML descrittivo e un ambiente completo di IntelliSense per la creazione di markup HTML e Razor. La maggior parte degli helper Tag predefiniti come destinazione gli elementi HTML esistenti e fornire attributi sul lato server per l'elemento.

### <a name="view-components"></a>Visualizza i componenti

[Visualizzare i componenti](views/view-components.md) consentono di creare un pacchetto la logica di rendering e riutilizzarla in tutta l'applicazione. Ma sono simili a [visualizzazioni parziali](views/partial.md), ma con logica associata.
