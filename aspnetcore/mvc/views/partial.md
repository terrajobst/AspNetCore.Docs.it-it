---
title: Visualizzazioni parziali
author: ardalis
description: Utilizzo di visualizzazioni parziali in ASP.NET MVC di base
keywords: Parziale di ASP.NET Core, visualizzazioni parziali,
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 4be1b12c-b74e-44ff-826b-99ce86e8d464
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/partial
ms.openlocfilehash: 777c4d663f646f3bc3fbe6da0b537d651a1fb4d8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="partial-views"></a>Visualizzazioni parziali

Da [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), e [Rick Anderson](https://twitter.com/RickAndMSFT)

Componenti di base di ASP.NET MVC supporta le visualizzazioni parziali sono utili quando si dispone di parti riutilizzabili di pagine web che si desidera condividere tra diverse visualizzazioni.

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample)

## <a name="what-are-partial-views"></a>Quali sono le visualizzazioni parziali?

Una visualizzazione parziale è una vista che viene eseguito il rendering all'interno di un'altra visualizzazione. L'output HTML generato eseguendo la visualizzazione parziale viene eseguito il rendering nella visualizzazione chiamante (padre o). Ad esempio viste, utilizzano le visualizzazioni parziali di *. cshtml* estensione di file.

## <a name="when-should-i-use-partial-views"></a>Quando utilizzare le visualizzazioni parziali

Le visualizzazioni parziali sono un modo efficace di rilievo delle viste di grandi dimensioni in componenti più piccoli. Possono ridurre la duplicazione di visualizzare il contenuto e consentono di visualizzare gli elementi da riutilizzare. Elementi di layout comuni devono essere specificati nel [layout. cshtml](layout.md). Il contenuto riutilizzabile di layout non può essere incapsulato in visualizzazioni parziali.

Se si dispone di una pagina complessa composta da più parti logiche, può essere utile lavorare con ogni come visualizzazione parziale. Ogni parte della pagina può essere visualizzate separatamente dal resto della pagina e la visualizzazione per la pagina diventa molto più semplice poiché contiene solo la struttura della pagina e le chiamate al rendering di visualizzazioni parziali complessiva.

Suggerimento: Seguire la [non ripetere manualmente principio](http://deviq.com/don-t-repeat-yourself/) nelle visualizzazioni.

## <a name="declaring-partial-views"></a>Dichiarazione di visualizzazioni parziali

Visualizzazioni parziali vengono create come qualsiasi altra visualizzazione: si crea un *. cshtml* file all'interno di *viste* cartella. Non c'è alcuna differenza semantica tra una visualizzazione parziale e una visualizzazione normale - vengono semplicemente visualizzati in modo diverso. È possibile creare una visualizzazione che viene restituita direttamente da un controller `ViewResult`, e la stessa vista può essere utilizzata come una visualizzazione parziale. La differenza principale tra le modalità di rendering di una vista e una visualizzazione parziale è che le visualizzazioni parziali non eseguano *viewstart* (mentre viste - altre informazioni, vedere *viewstart* in [Layout ](layout.md)).

## <a name="referencing-a-partial-view"></a>Riferimento a una visualizzazione parziale

All'interno di una pagina di visualizzazione, esistono diversi modi in cui è possibile eseguire il rendering di una visualizzazione parziale. La più semplice consiste nell'utilizzare `Html.Partial`, che restituisce un `IHtmlString` e a cui fa riferimento, facendolo precedere la chiamata con `@`:

[!code-html[Principale](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

Il `PartialAsync` metodo è disponibile per parziale viste contenenti codice asincrono (anche se è in genere sconsigliata codice nelle viste):

[!code-html[Principale](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

È possibile eseguire il rendering di una visualizzazione parziale con `RenderPartial`. Questo metodo non restituisce un risultato. il flusso di output del rendering direttamente alla risposta. Perché non restituisce un risultato, deve essere chiamato all'interno di un blocco di codice Razor (è anche possibile chiamare `RenderPartialAsync` se necessario):

[!code-html[Principale](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

Poiché il flusso direttamente, il risultato `RenderPartial` e `RenderPartialAsync` può offrire prestazioni superiori in alcuni scenari. Tuttavia, nella maggior parte dei casi, si consiglia di utilizzare `Partial` e `PartialAsync`.

> [!NOTE]
> Se le visualizzazioni necessarie eseguire il codice, il modello consigliato consiste nell'utilizzare un [del componente di visualizzazione](view-components.md) anziché una visualizzazione parziale.

### <a name="partial-view-discovery"></a>Individuazione di visualizzazione parziale

Quando si fa riferimento a una visualizzazione parziale, è possibile fare riferimento alla posizione in diversi modi:

```text
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@Html.Partial("ViewName")

// A view with this name must be in the same folder
@Html.Partial("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@Html.Partial("~/Views/Folder/ViewName.cshtml")
@Html.Partial("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@Html.Partial("../Account/LoginPartial.cshtml")
```

Nelle cartelle di visualizzazione diversa, è possibile avere diverse visualizzazioni parziali con lo stesso nome. Quando si fa riferimento le viste in base al nome (senza estensione file), le visualizzazioni in ogni cartella utilizzerà la visualizzazione parziale nella stessa cartella con essi. È inoltre possibile specificare una visualizzazione parziale predefinito da usare, collocandola nel *Shared* cartella. La visualizzazione parziale condivisa da utilizzare per le viste che non dispongono di una propria versione della visualizzazione parziale. È possibile creare una visualizzazione parziale predefinito (in *Shared*), che viene sottoposto a override da una visualizzazione parziale con lo stesso nome nella stessa cartella di visualizzazione padre.

Le visualizzazioni parziali possono essere *concatenate*. Ovvero una visualizzazione parziale può richiamare un'altra visualizzazione parziale (fino a quando non è possibile creare un ciclo). All'interno di ogni vista o una visualizzazione parziale, i percorsi relativi sono sempre relativi alla visualizzazione visualizzazione, non il principale o padre.

> [!NOTE]
> Se si dichiara un [Razor](razor.md) `section` in una visualizzazione parziale, non sarà visibile per i relativi elementi padre, è limitata alla visualizzazione parziale.

## <a name="accessing-data-from-partial-views"></a>Accesso ai dati da visualizzazioni parziali

Quando viene creata un'istanza di una visualizzazione parziale, ottiene una copia della visualizzazione padre `ViewData` dizionario. Gli aggiornamenti apportati ai dati all'interno della visualizzazione parziale non vengono mantenuti per la visualizzazione padre. `ViewData`modificato in un elemento parziale vista viene persa quando restituisce la visualizzazione parziale.

È possibile passare un'istanza di `ViewDataDictionary` della visualizzazione parziale:

```csharp
@Html.Partial("PartialName", customViewData)
   ```

È inoltre possibile passare un modello in una visualizzazione parziale. Può trattarsi di modello di visualizzazione della pagina, o una parte di esso o un oggetto personalizzato. È possibile passare un modello per `Partial`,`PartialAsync`, `RenderPartial`, o `RenderPartialAsync`:

```csharp
@Html.Partial("PartialName", viewModel)
   ```

È possibile passare un'istanza di `ViewDataDictionary` e un modello di visualizzazione per una visualizzazione parziale:

[!code-html[Principale](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

Il markup seguente viene illustrato il *Views/Articles/Read.cshtml* vista che contiene due visualizzazioni parziali. La visualizzazione parziale secondo passa in un modello e `ViewData` della visualizzazione parziale. È possibile passare a nuovi `ViewData` dizionario mantenendo esistente `ViewData` se si utilizza l'overload del costruttore del `ViewDataDictionary` evidenziato di seguito:

[!code-html[Principale](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Viste, condivisi/AuthorPartial*:

[!code-html[Principale](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

Il *ArticleSection* parziale:

[!code-html[Principale](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

In fase di esecuzione, i parziali vengono sottoposti a rendering nella visualizzazione padre, che a sua volta viene eseguito il rendering all'interno di condiviso *layout. cshtml*

![output di visualizzazione parziale](partial/_static/output.png)
