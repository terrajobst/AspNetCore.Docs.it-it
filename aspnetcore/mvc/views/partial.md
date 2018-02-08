---
title: Visualizzazioni parziali
author: ardalis
description: Uso delle visualizzazioni parziali in ASP.NET Core MVC
manager: wpickett
ms.author: riande
ms.date: 03/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/partial
ms.openlocfilehash: 169948e5d7dc8068463ed61114666148b785b217
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="partial-views"></a>Visualizzazioni parziali

Di [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend) e [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC supporta le visualizzazioni parziali, utili quando si dispone di parti riutilizzabili di pagine web che si desidera condividere tra diverse visualizzazioni.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Che cosa sono le visualizzazioni parziali?

Una visualizzazione parziale è una visualizzazione resa in un'altra visualizzazione. L'output HTML generato tramite l'esecuzione della visualizzazione parziale viene reso nella visualizzazione di chiamata (o padre). Analogamente alle visualizzazioni, le visualizzazioni parziali usano l'estensione *.cshtml*.

## <a name="when-should-i-use-partial-views"></a>Quando usare le visualizzazioni parziali

Le visualizzazioni parziali sono un modo efficace per suddividere le visualizzazioni di grandi dimensioni in componenti più piccoli. Possono ridurre la duplicazione del contenuto della visualizzazione e consentire di riutilizzare elementi della visualizzazione. Gli elementi comuni del layout devono essere specificati in [_Layout.cshtml](layout.md). Il contenuto riutilizzabile non appartenente al layout può essere incapsulato in visualizzazioni parziali.

Nel caso di una pagina complessa composta da più parti logiche, può essere utile lavorare con ogni parte come visualizzazione parziale. Ogni parte della pagina può essere visualizzata separatamente dal resto della pagina e la visualizzazione della pagina diventa molto più semplice perché contiene solo la struttura generale della pagina e le chiamate per il rendering delle visualizzazioni parziali.

Suggerimento: nelle visualizzazioni seguire il [principio Don't Repeat Yourself](http://deviq.com/don-t-repeat-yourself/).

## <a name="declaring-partial-views"></a>Dichiarazione delle visualizzazioni parziali

Le visualizzazioni parziali vengono create come qualsiasi altra visualizzazione: creare un file *.cshtml* all'interno della cartella *Visualizzazioni*. Non vi è differenza semantica tra una visualizzazione parziale e una visualizzazione normale: viene soltanto eseguito il rendering in modo diverso. È possibile creare una visualizzazione che viene restituita direttamente da un controller `ViewResult` e la stessa visualizzazione può essere usata come visualizzazione parziale. La differenza principale tra le modalità di rendering di una visualizzazione e di una visualizzazione parziale è che le visualizzazioni parziali non eseguono *_ViewStart.cshtml* (mentre le visualizzazioni lo eseguono - per altre informazioni su *_ViewStart.cshtml* vedere [Layout ](layout.md)).

## <a name="referencing-a-partial-view"></a>Riferimento a una visualizzazione parziale

All'interno di una pagina di visualizzazione esistono diversi modi in cui è possibile eseguire il rendering di una visualizzazione parziale. Il più semplice consiste nell'uso di `Html.Partial`, che restituisce `IHtmlString` e a cui è possibile fare riferimento facendo precedere la chiamata da `@`:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

Il metodo `PartialAsync` è disponibile per le visualizzazioni parziali contenenti codice asincrono (anche se in genere è sconsigliato il codice nelle visualizzazioni):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

È possibile eseguire il rendering di una visualizzazione parziale con `RenderPartial`. Questo metodo non restituisce un risultato, ma trasmette l'output sottoposto a rendering direttamente alla risposta. Poiché non restituisce un risultato, deve essere chiamato all'interno di un blocco di codice Razor (è possibile anche chiamare `RenderPartialAsync` se necessario):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

Poiché trasmette il risultato direttamente, `RenderPartial` e `RenderPartialAsync` possono offrire prestazioni superiori in alcuni scenari. Tuttavia, nella maggior parte dei casi, si consiglia di usare `Partial` e `PartialAsync`.

> [!NOTE]
> Se le visualizzazioni richiedono l'esecuzione di codice, il criterio consigliato consiste nell'uso di un [componente di visualizzazione](view-components.md) anziché di una visualizzazione parziale.

### <a name="partial-view-discovery"></a>Individuazione delle visualizzazioni parziali

Quando si fa riferimento a una visualizzazione parziale, è possibile fare riferimento alla sua posizione in diversi modi:

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

È possibile avere visualizzazioni parziali diverse con lo stesso nome in cartelle di visualizzazione diverse. Quando si fa riferimento alle visualizzazioni in base al nome (senza estensione di file), le visualizzazioni in ogni cartella usano la visualizzazione parziale nella stessa cartella. È inoltre possibile specificare una visualizzazione parziale predefinita da usare, collocandola nella cartella *Condiviso*. La visualizzazione parziale condivisa verrà usata dalle visualizzazioni che non dispongono di una propria versione della visualizzazione parziale. È possibile creare una visualizzazione parziale predefinita (in *Condiviso*), che viene sottoposta a override da una visualizzazione parziale con lo stesso nome nella stessa cartella della visualizzazione padre.

Le visualizzazioni parziali possono essere *concatenate*. Ovvero una visualizzazione parziale può chiamare un'altra visualizzazione parziale (a condizione di non creare un ciclo). All'interno di ogni visualizzazione o visualizzazione parziale, i percorsi relativi sono sempre relativi a tale visualizzazione, non alla visualizzazione radice o padre.

> [!NOTE]
> Se si dichiara una `section` [Razor](razor.md) in una visualizzazione parziale, questa non sarà visibile alla/e relativa/e visualizzazione/i padre, ma sarà limitata alla visualizzazione parziale.

## <a name="accessing-data-from-partial-views"></a>Accesso ai dati da visualizzazioni parziali

Quando viene creata un'istanza di una visualizzazione parziale, le viene associata una copia del dizionario `ViewData` della visualizzazione padre. Gli aggiornamenti apportati ai dati all'interno della visualizzazione parziale non vengono mantenuti per la visualizzazione padre. Una modifica a `ViewData` in una visualizzazione parziale viene persa quando viene restituita la visualizzazione parziale.

È possibile trasmettere un'istanza di `ViewDataDictionary` alla visualizzazione parziale:

```csharp
@Html.Partial("PartialName", customViewData)
   ```

È inoltre possibile trasmettere un modello in una visualizzazione parziale. Può trattarsi del modello di visualizzazione della pagina, di una parte di esso o di un oggetto personalizzato. È possibile trasmettere un modello a `Partial`,`PartialAsync`, `RenderPartial` o `RenderPartialAsync`:

```csharp
@Html.Partial("PartialName", viewModel)
   ```

È possibile trasmettere un'istanza di `ViewDataDictionary` e un modello di visualizzazione a una visualizzazione parziale:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

Il markup seguente mostra la visualizzazione *Views/Articles/Read.cshtml* contenente due visualizzazioni parziali. La seconda visualizzazione parziale viene trasmessa a un modello e `ViewData` viene trasmesso alla visualizzazione parziale. È possibile trasmettere nuovi dizionari `ViewData` mantenendo `ViewData` esistente se si usa l'overload del costrutto di `ViewDataDictionary` evidenziato di seguito:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Views/Shared/AuthorPartial*:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

*ArticleSection* parziale:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

In fase di esecuzione, i parziali vengono sottoposti a rendering nella visualizzazione padre, di cui viene eseguito il rendering all'interno del *_Layout.cshtml* condiviso

![output di visualizzazione parziale](partial/_static/output.png)
