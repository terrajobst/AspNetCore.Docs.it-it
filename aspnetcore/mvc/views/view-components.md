---
title: Visualizza i componenti
author: rick-anderson
description: Visualizza i componenti sono destinati in qualsiasi punto che si dispone di logica di rendering riutilizzabili.
keywords: ASP.NET Core, Visualizza i componenti, visualizzazione parziale
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: ab4705b7-59d7-4f31-bc97-ea7f292fe926
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 3bc6d3f85d8ea7fb9b72b18cfd9c5ec2d07293b0
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2017
---
# <a name="view-components"></a>Visualizza i componenti

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))

## <a name="introducing-view-components"></a>Introduzione a componenti di visualizzazione

Nuovo ad ASP.NET MVC di base, Visualizza i componenti sono simili alle visualizzazioni parziali, ma sono molto più potenti. Visualizza i componenti non utilizzare l'associazione di modelli e dipendono solo i dati forniti durante la chiamata al suo interno. Un componente di visualizzazione:

* Esegue il rendering di un blocco anziché un'intera risposta.
* Include gli stesso la separazione di problemi e i vantaggi di testabilità trovati tra un controller e una visualizzazione
* Può avere parametri e logica di business
* In genere viene richiamato da una pagina di layout

Visualizza i componenti sono destinati in qualsiasi punto che si dispone di logica di rendering riutilizzabile che è troppo complessa per una visualizzazione parziale, ad esempio:

* Menu di navigazione dinamica
* Cloud tag (in cui viene eseguita una query del database)
* Pannello di accesso
* Carrello acquisti
* Articoli pubblicati di recente
* Contenuto dell'intestazione laterale in un tipico blog
* Un pannello di accesso che verrebbe eseguito in ogni pagina e visualizza entrambi i collegamenti per effettuare la disconnessione o di log, a seconda di log nello stato dell'utente

Un componente di visualizzazione è costituito da due parti: la classe (in genere derivato da [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) e il risultato restituisce (in genere una vista). Ad esempio controller, un componente di visualizzazione può essere un POCO, ma desidera sfruttare i metodi e le proprietà disponibili derivandolo dalla maggior parte degli sviluppatori `ViewComponent`.

## <a name="creating-a-view-component"></a>Creazione di un componente di visualizzazione

In questa sezione contiene i requisiti di alto livello per creare un componente di visualizzazione. In un secondo momento nell'articolo, viene esaminare in dettaglio ogni passaggio e creare un componente di visualizzazione.

### <a name="the-view-component-class"></a>La classe di componente di visualizzazione

È possibile creare una classe di componente di visualizzazione da una delle seguenti:

* Derivazione da *ViewComponent*
* La decorazione di una classe con il `[ViewComponent]` attributo o deriva da una classe con il `[ViewComponent]` attributo
* Creazione di una classe il cui nome termina con il suffisso *ViewComponent*

Ad esempio controller, Visualizza i componenti devono essere classi di pubbliche, non annidata e non astratta. Il nome del componente di visualizzazione è il nome di classe con il suffisso "ViewComponent" rimosso. È possibile inoltre specificare in modo esplicito utilizzando il `ViewComponentAttribute.Name` proprietà.

Una classe di componente di visualizzazione:

* Supporta completamente costruttore [inserimento di dipendenze](../../fundamentals/dependency-injection.md)

* Non prendere parte del ciclo di vita di controller, ovvero non è possibile utilizzare [filtri](../controllers/filters.md) in un componente di visualizzazione

### <a name="view-component-methods"></a>Metodi del componente di visualizzazione

Un componente di visualizzazione definisce la logica in un `InvokeAsync` metodo che restituisce un `IViewComponentResult`. I parametri che provengano direttamente dalla chiamata di un componente di visualizzazione, non dall'associazione del modello. Un componente di visualizzazione mai direttamente gestisce una richiesta. In genere, Inizializza un modello e la passa a una visualizzazione chiamando un componente di visualizzazione di `View` metodo. In sintesi, visualizzare i metodi del componente:

* Definire un `InvokeAsync` metodo che restituisce un`IViewComponentResult`
* Inizializza un modello e la passa a una vista mediante la chiamata in genere il `ViewComponent` `View` (metodo)
* Parametri provengono dal metodo di chiamata, non HTTP, non è disponibile alcuna associazione di modelli
* Sono non raggiungibile direttamente come un endpoint HTTP, vengono richiamati dal codice (in genere in una vista). Un componente di visualizzazione mai gestisce una richiesta
* La firma, anziché i dettagli della richiesta HTTP corrente sono sottoposti a overload

### <a name="view-search-path"></a>Percorso di ricerca di visualizzazione

Il runtime esegue la ricerca per la visualizzazione nei percorsi seguenti:

   * Viste /\<controller_name > /Components/\<view_component_name > /\<view_name >
   * Componenti/Shared/visualizzazioni/\<view_component_name > /\<view_name >

Il nome di visualizzazione predefinito per un componente di visualizzazione è *predefinito*, ovvero il file di visualizzazione in genere è denominato *cshtml*. È possibile specificare un nome di visualizzazione diverso quando si crea il risultato di componente di visualizzazione o quando si chiama il `View` metodo.

Si consiglia denominare il file di visualizzazione *cshtml* e utilizzare il *componenti/Shared/visualizzazioni/\<view_component_name > /\<view_name >* percorso. Il `PriorityList` del componente di visualizzazione utilizzato in questo esempio Usa *Views/Shared/Components/PriorityList/Default.cshtml* per la vista del componente di visualizzazione.

## <a name="invoking-a-view-component"></a>Richiamo di un componente di visualizzazione

Per utilizzare il componente di visualizzazione, chiamare le operazioni seguenti all'interno di una vista:

```html
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

I parametri verranno passati al `InvokeAsync` metodo. Il `PriorityList` sviluppato nell'articolo del componente di visualizzazione viene richiamato dal *Views/Todo/Index.cshtml* file della vista. Nell'esempio seguente, il `InvokeAsync` metodo viene chiamato con due parametri:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Richiamo di un componente di visualizzazione come un Helper Tag

Per ASP.NET Core 1.1 e versioni successive, è possibile richiamare un componente di visualizzazione come una [Helper di Tag](xref:mvc/views/tag-helpers/intro):

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Base alla convezione Pascal classe e metodo parametri per gli helper di Tag vengono convertiti in loro [inferiore case kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Utilizza l'Helper di Tag per richiamare un componente di visualizzazione di `<vc></vc>` elemento. Il componente di visualizzazione viene specificato come segue:

```html
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Nota: Per utilizzare un componente di visualizzazione come un Helper Tag, è necessario registrare l'assembly contenente il componente di visualizzazione utilizzando il `@addTagHelper` direttiva. Ad esempio, se il componente di visualizzazione si trova in un assembly denominato "MyWebApp", aggiungere la seguente direttiva per il `_ViewImports.cshtml` file:

```csharp
@addTagHelper *, MyWebApp
```

È possibile registrare un componente di visualizzazione come un Helper Tag a qualsiasi file che fa riferimento il componente di visualizzazione. Vedere [Gestione ambito Helper di Tag](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) per ulteriori informazioni su come registrare gli helper di Tag.

Il `InvokeAsync` metodo utilizzato in questa esercitazione:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Nel markup di Helper di Tag:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Nell'esempio precedente, il `PriorityList` del componente di visualizzazione diventa `priority-list`. I parametri per il componente di visualizzazione vengono passati come attributi in lettere minuscole kebab.

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Richiamo di un componente di visualizzazione direttamente da un controller

Visualizza i componenti vengono in genere richiamati da una vista, ma è possibile richiamare tali direttamente da un metodo del controller. Mentre i componenti di visualizzazione non definiscono endpoint come controller, è possibile implementare un'azione del controller che restituisce il contenuto di un `ViewComponentResult`.

In questo esempio, il componente di visualizzazione viene chiamato direttamente dal controller:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>Procedura dettagliata: Creazione di un componente di visualizzazione semplice

[Scaricare](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compilare e testare il codice di avvio. Si tratta di un progetto semplice con un `Todo` controller che visualizza un elenco di *Todo* elementi.

![Elenco di ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>Aggiungere una classe ViewComponent

Creare un *ViewComponents* cartella e aggiungere le seguenti `PriorityListViewComponent` classe:

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Note sul codice:

* Visualizzazione classi di componenti possono essere contenute in **qualsiasi** cartella nel progetto.
* Poiché il nome della classe PriorityList**ViewComponent** termina con il suffisso **ViewComponent**, il runtime utilizza la stringa "PriorityList" quando si fa riferimento il componente della classe da una vista. Verrà illustrato che in modo più dettagliato più avanti.
* Il `[ViewComponent]` attributo è possibile modificare il nome utilizzato per fare riferimento a un componente di visualizzazione. Ad esempio, è possibile avere denominato la classe `XYZ` e applicati i `ViewComponent` attributo:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* Il `[ViewComponent]` attributo indica il selettore di componente di visualizzazione per utilizzare il nome `PriorityList` durante la ricerca per le visualizzazioni associate, il componente e di utilizzare la stringa "PriorityList" quando si fa riferimento il componente della classe da una vista. Verrà illustrato che in modo più dettagliato più avanti.
* Il componente utilizza [inserimento di dipendenze](../../fundamentals/dependency-injection.md) a disposizione il contesto dei dati.
* `InvokeAsync`espone un metodo che può essere chiamato da una vista e può accettare un numero arbitrario di argomenti.
* Il `InvokeAsync` il metodo restituisce il set di `ToDo` gli elementi che soddisfano il `isDone` e `maxPriority` parametri.

### <a name="create-the-view-component-razor-view"></a>Creare la visualizzazione Razor componente di visualizzazione

* Creare il *componenti/Shared/visualizzazioni* cartella. Questa cartella **deve** denominato *componenti*.

* Creare il *viste/Shared/componenti/PriorityList* cartella. Il nome della cartella deve corrispondere il nome della classe di componente di visualizzazione, o il nome della classe meno il suffisso (se è seguito convenzione e utilizzata la *ViewComponent* suffisso nel nome della classe). Se è stata utilizzata la `ViewComponent` attributo, il nome della classe dovrà corrispondere la designazione di attributo.

* Creare un *Views/Shared/Components/PriorityList/Default.cshtml* visualizzazione Razor:[!code-html[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   La visualizzazione Razor accetta un elenco di `TodoItem` e li visualizza. Se il componente di visualizzazione `InvokeAsync` metodo non passa il nome della visualizzazione (come in questo esempio), *predefinito* per convenzione viene utilizzato per il nome della vista. Più avanti nell'esercitazione verrà illustrato come passare il nome della vista. Per ignorare lo stile predefinito per un controller specifico, aggiungere una visualizzazione nella cartella di visualizzazione controller specifico (ad esempio *Views/Todo/Components/PriorityList/Default.cshtml)*.
    
    Se il componente di visualizzazione è specifici dei controller, è possibile aggiungere alla cartella controller specifico (*Views/Todo/Components/PriorityList/Default.cshtml*).

* Aggiungere un `div` che contiene una chiamata per il componente alla fine dell'elenco di priorità il *Views/Todo/index.cshtml* file:

    [!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

Il markup `@await Component.InvokeAsync` viene illustrata la sintassi per chiamare i componenti di visualizzazione. Il primo argomento è il nome del componente di cui che si desidera richiamare o chiamare. I parametri successivi vengono passati al componente. `InvokeAsync`può richiedere un numero arbitrario di argomenti.

Testare l'app. La figura seguente mostra l'elenco di attività e gli elementi con priorità:

![elementi di elenco e priorità attività](view-components/_static/pi.png)

È inoltre possibile chiamare il componente di visualizzazione direttamente dal controller:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![elementi con priorità dall'azione IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Specificare un nome di visualizzazione

Un componente di visualizzazione complesso potrebbe essere necessario specificare una vista non predefinita in alcune condizioni. Il codice seguente viene illustrato come specificare la vista "PVC" dal `InvokeAsync` metodo. Aggiornamento di `InvokeAsync` metodo la `PriorityListViewComponent` classe.

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Copia il *Views/Shared/Components/PriorityList/Default.cshtml* file da una vista denominata *Views/Shared/Components/PriorityList/PVC.cshtml*. Aggiungere un'intestazione per indicare che la vista PVC è in uso.

[!code-html[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Aggiornamento *Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Eseguire l'app e verificare la visualizzazione PVC.

![Priorità del componente di visualizzazione](view-components/_static/pvc.png)

Se non viene visualizzata la vista PVC, verificare che si sta chiamando il componente di visualizzazione con una priorità pari a 4 o versione successiva.

### <a name="examine-the-view-path"></a>Esaminare il percorso di visualizzazione

* Modificare il parametro di priorità a tre o meno in modo non viene restituita la visualizzazione priority.
* Rinominare temporaneamente il *Views/Todo/Components/PriorityList/Default.cshtml* a *1Default.cshtml*.
* Testare l'app, si otterrà l'errore seguente:

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Copia *Views/Todo/Components/PriorityList/1Default.cshtml* a *Views/Shared/Components/PriorityList/Default.cshtml*.
* Aggiungere codice per il *Shared* Todo visualizzazione componente per indicare la vista è dal *Shared* cartella.
* Test di **Shared** vista del componente.

![Output di attività con vista del componente condiviso](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>Evitare stringhe chiave

Se si desidera compilare la sicurezza di tempo, è possibile sostituire il nome del componente di visualizzazione a livello di codice con il nome della classe. Creare il componente di visualizzazione senza il suffisso "ViewComponent":

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Aggiungere un `using` istruzione per il Razor consente di visualizzare file e utilizzare il `nameof` operatore:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Inserimento di dipendenze in visualizzazioni](dependency-injection.md)
