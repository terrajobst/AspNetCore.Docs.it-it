---
title: Visualizzazioni parziali in ASP.NET Core
author: ardalis
description: Informazioni sulle visualizzazioni parziali, visualizzazioni il cui rendering viene eseguito all'interno di un'altra visualizzazione, e su quando devono essere usate nelle app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/partial
ms.openlocfilehash: 983f3caae34b21b46d8f556e70673cf3c97abbd3
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938459"
---
# <a name="partial-views-in-aspnet-core"></a>Visualizzazioni parziali in ASP.NET Core

Di [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT) e [Scott Sauber](https://twitter.com/scottsauber)

ASP.NET Core MVC supporta le visualizzazioni parziali, utili per la condivisione di parti riutilizzabili di pagine Web tra diverse visualizzazioni.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Che cosa sono le visualizzazioni parziali

Una visualizzazione parziale è una visualizzazione resa in un'altra visualizzazione. L'output HTML generato tramite l'esecuzione della visualizzazione parziale viene reso nella visualizzazione di chiamata (o padre). Analogamente alle visualizzazioni, le visualizzazioni parziali usano l'estensione *.cshtml*.

Ad esempio, il modello di progetto **applicazione Web** ASP.NET Core 2.1 include una visualizzazione parziale *_CookieConsentPartial.cshtml*. La visualizzazione parziale viene caricata da *_Layout.cshtml*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_Layout.cshtml?name=snippet_CookieConsentPartial)]

## <a name="when-to-use-partial-views"></a>Quando usare le visualizzazioni parziali

Le visualizzazioni parziali sono un modo efficace per suddividere le visualizzazioni di grandi dimensioni in componenti più piccoli. Possono ridurre la duplicazione del contenuto della visualizzazione e consentire di riutilizzare elementi della visualizzazione. Gli elementi comuni del layout devono essere specificati in [_Layout.cshtml](xref:mvc/views/layout). Il contenuto riutilizzabile non appartenente al layout può essere incapsulato in visualizzazioni parziali.

In una pagina complessa composta da più parti logiche, è utile lavorare con ogni parte come visualizzazione parziale. Ogni parte della pagina può essere visualizzata in modo isolato dal resto della pagina. La visualizzazione della pagina diventa più semplice, poiché contiene solo la struttura generale della pagina e chiamate per il rendering delle visualizzazioni parziali.

> [!TIP]
> Nelle visualizzazioni seguire il [principio Don't Repeat Yourself](https://deviq.com/don-t-repeat-yourself/).

## <a name="declare-partial-views"></a>Dichiarare le visualizzazioni parziali

Le visualizzazioni parziali vengono create come una visualizzazione normale&mdash; creando un file con estensione *cshtml* all'interno della cartella *Visualizzazioni*. Non vi è differenza semantica tra una visualizzazione parziale e una visualizzazione normale, tuttavia il rendering viene eseguito in modo diverso. È possibile creare una visualizzazione che viene restituita direttamente dall'elemento [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult) di un controller e la stessa visualizzazione può essere usata come visualizzazione parziale. La differenza principale tra le modalità di rendering di una visualizzazione e di una visualizzazione parziale è che le visualizzazioni parziali non eseguono *_ViewStart.cshtml*. Le visualizzazioni normali eseguono *_ViewStart.cshtml*. Per altre informazioni su *_ViewStart.cshtml*, vedere [Layout](xref:mvc/views/layout).

Per convenzione, i nomi dei file di visualizzazione parziale iniziano spesso con `_`. Questa convenzione di denominazione non è un requisito, ma è utile per differenziare visivamente le visualizzazioni parziali da quelle normali.

## <a name="reference-a-partial-view"></a>Riferimento a una visualizzazione parziale

All'interno di una pagina di visualizzazione esistono diversi modi per eseguire il rendering di una visualizzazione parziale. La procedura consigliata consiste nell'usare il rendering asincrono.

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Helper tag Partial

L'helper tag Partial richiede ASP.NET Core 2.1 o versioni successive. Esegue il rendering in modo asincrono e usa una sintassi simile a HTML:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialTagHelper)]

Per ulteriori informazioni, vedere <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.

::: moniker-end

### <a name="asynchronous-html-helper"></a>Helper HTML asincrono

Quando si usa un helper HTML, la procedura consigliata consiste nell'usare [PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync#Microsoft_AspNetCore_Mvc_Rendering_HtmlHelperPartialExtensions_PartialAsync_Microsoft_AspNetCore_Mvc_Rendering_IHtmlHelper_System_String_). Restituisce un tipo [IHtmlContent](/dotnet/api/microsoft.aspnetcore.html.ihtmlcontent) di cui è stato eseguito il wrapping in un `Task`. Il riferimento al metodo avviene facendo precedere la chiamata con `@`:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialAsync)]

In alternativa è possibile eseguire il rendering di una visualizzazione parziale con [RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync). Questo metodo non restituisce un risultato. Trasmette l'output sottoposto a rendering direttamente alla risposta. Poiché il metodo non restituisce un risultato, deve essere chiamato all'interno di un blocco di codice Razor:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

Poiché trasmette il risultato direttamente, `RenderPartialAsync` può offrire prestazioni superiori in alcuni scenari. È tuttavia consigliabile usare `PartialAsync`.

### <a name="synchronous-html-helper"></a>Helper HTML sincrono

[Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial) e [RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial) sono gli equivalenti sincroni rispettivamente di `PartialAsync` e `RenderPartialAsync`. L'uso degli equivalenti sincroni non è consigliabile perché in alcuni scenari creano un deadlock. Le versioni future non conterranno i metodi sincroni.

> [!IMPORTANT]
> Se le visualizzazioni richiedono l'esecuzione di codice, usare un [componente di visualizzazione](xref:mvc/views/view-components) anziché una visualizzazione parziale.

::: moniker range=">= aspnetcore-2.1"

In ASP.NET Core 2.1 o versioni successive, la chiamata a `Partial` o `RenderPartial` genera un avviso di analizzatore. Ad esempio, l'uso di `Partial` produce il messaggio di avviso seguente:

> L'uso di IHtmlHelper.Partial potrebbe determinare deadlock dell'applicazione. Provare a usare l'helper tag `<partial>` o `IHtmlHelper.PartialAsync`.

Sostituire le chiamate a `@Html.Partial` con `@await Html.PartialAsync` o l'helper tag Partial. Per altre informazioni sulla migrazione dell'helper tag Partial, vedere [Eseguire la migrazione da un helper HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).

::: moniker-end

## <a name="partial-view-discovery"></a>Individuazione delle visualizzazioni parziali

Quando si fa riferimento a una visualizzazione parziale, è possibile fare riferimento alla sua posizione in diversi modi. Ad esempio:

::: moniker range=">= aspnetcore-2.1"

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
<partial name="_ViewName" />

// A view with this name must be in the same folder
<partial name="_ViewName.cshtml" />

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
<partial name="~/Views/Folder/_ViewName.cshtml" />
<partial name="/Views/Folder/_ViewName.cshtml" />

// Locate the view using a relative path
<partial name="../Account/_LoginPartial.cshtml" />
```

L'esempio precedente usa l'helper tag Partial, che richiede ASP.NET Core 2.1 o versioni successive. L'esempio seguente usa gli helper HTML asincroni per eseguire la stessa attività.

::: moniker-end

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
@await Html.PartialAsync("_ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("_ViewName.cshtml")

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
@await Html.PartialAsync("~/Views/Folder/_ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/_ViewName.cshtml")

// Locate the view using a relative path
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

È possibile avere visualizzazioni parziali diverse con lo stesso nome file in cartelle di visualizzazione diverse. Quando si fa riferimento alle visualizzazioni in base al nome (senza estensione di file), le visualizzazioni in ogni cartella usano la visualizzazione parziale nella stessa cartella. È inoltre possibile specificare una visualizzazione parziale predefinita da usare, collocandola nella cartella *Condiviso*. La visualizzazione parziale condivisa viene usata dalle visualizzazioni che non hanno una propria versione della visualizzazione parziale. È possibile creare una visualizzazione parziale predefinita (in *Condiviso*), che viene sottoposta a override da una visualizzazione parziale con lo stesso nome nella stessa cartella della visualizzazione padre.

Le visualizzazioni parziali possono essere *concatenate*, ovvero una visualizzazione parziale può chiamare un'altra visualizzazione parziale (a condizione di non creare un ciclo). All'interno di ogni visualizzazione o visualizzazione parziale, i percorsi relativi sono sempre relativi a tale visualizzazione, non alla visualizzazione radice o padre.

> [!NOTE]
> Un oggetto [Razor](xref:mvc/views/razor) `section`definito in una visualizzazione parziale non è visibile alle visualizzazioni di elementi padre. L'oggetto `section` è visibile solo per la visualizzazione parziale in cui è definito.

## <a name="access-data-from-partial-views"></a>Accesso ai dati da visualizzazioni parziali

Quando viene creata un'istanza di una visualizzazione parziale, le viene associata una copia del dizionario `ViewData` della visualizzazione padre. Gli aggiornamenti apportati ai dati all'interno della visualizzazione parziale non vengono mantenuti per la visualizzazione padre. Le modifiche a `ViewData` in una visualizzazione parziale vengono perse quando viene restituita la visualizzazione parziale.

È possibile trasmettere un'istanza di [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) alla visualizzazione parziale:

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

È possibile trasmettere un modello in una visualizzazione parziale. Può trattarsi del modello di visualizzazione della pagina o di un oggetto personalizzato. È possibile trasmettere un modello a `PartialAsync` o a `RenderPartialAsync`:

```cshtml
@await Html.PartialAsync("_PartialName", viewModel)
```

È possibile trasmettere un'istanza di `ViewDataDictionary` e un modello di visualizzazione a una visualizzazione parziale:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_PartialAsync)]

Il markup seguente illustra la visualizzazione *Views/Articles/Read.cshtml* contenente due visualizzazioni parziali. La seconda visualizzazione parziale viene trasmessa a un modello e `ViewData` viene trasmesso alla visualizzazione parziale. Usare l'overload del costrutto `ViewDataDictionary` evidenziato per passare un nuovo dizionario `ViewData`mantenendo il dizionario `ViewData` esistente.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=17-20)]

*Views/Shared/_AuthorPartial*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

L'elemento *_ArticleSection* parziale:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

In fase di esecuzione, i parziali vengono sottoposti a rendering nella visualizzazione padre, di cui viene eseguito il rendering all'interno dell'elemento *_Layout.cshtml* condiviso.

![output di visualizzazione parziale](partial/_static/output.png)

## <a name="additional-resources"></a>Risorse aggiuntive

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>

::: moniker-end
