---
title: Layout
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 29f12d1f-9734-48bd-bf1a-cee53a8ab700
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: 064621d8756b007c5b8859111bf3a03a0d7dda81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="layout"></a>Layout

Da [Steve Smith](https://ardalis.com/)

Viste spesso condividono elementi visivi e a livello di codice. In questo articolo si apprenderà come usare layout comuni, condividere direttive ed eseguire codice comune prima di rendering delle visualizzazioni nell'applicazione ASP.NET.

## <a name="what-is-a-layout"></a>Che cos'è un Layout

La maggior parte delle applicazioni web presentano un layout comune che fornisce all'utente un'esperienza coerente, come si spostano da una pagina. In genere, il layout include elementi dell'interfaccia utente comune, ad esempio l'intestazione di app, la navigazione o elementi di menu e piè di pagina.

![Esempio di Layout di pagina](layout/_static/page-layout.png)

Strutture HTML comuni, ad esempio script e fogli di stile vengono spesso utilizzate anche dal numero di pagine all'interno di un'app. Tutti questi elementi di lavoro condivisa può essere definiti un *layout* file, è possibile fare riferimento da qualsiasi visualizzazione utilizzata all'interno dell'app. Layout di ridurre il codice duplicato in viste, consentendo loro seguire il [principio non ripetere manualmente (sorgente)](http://deviq.com/don-t-repeat-yourself/).

Per convenzione, il layout predefinito per un'app ASP.NET denominato `_Layout.cshtml`. Il modello di progetto di Visual Studio ASP.NET Core MVC include il file di layout nel `Views/Shared` cartella:

![cartella Views in Esplora soluzioni](layout/_static/web-project-views.png)

Questo layout definisce un modello di livello superiore per le viste nell'app. App non richiedono un layout e le applicazioni è possono definire più di un layout, con diverse visualizzazioni specificare layout diversi.

Un esempio `_Layout.cshtml`:

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Specificare un Layout

Visualizzazioni Razor hanno un `Layout` proprietà. Singole visualizzazioni per specificare un layout, impostazione di questa proprietà:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Il layout specificato è possibile utilizzare un percorso completo (esempio: `/Views/Shared/_Layout.cshtml`) o un nome parziale (esempio: `_Layout`). Quando viene fornito un nome parziale, il motore di visualizzazione Razor cercherà il file di layout tramite il processo di individuazione standard. La cartella associata al controller viene eseguita la ricerca in primo luogo, aggiungendo il `Shared` cartella. Questo processo di individuazione è identico a quello utilizzato per individuare [visualizzazioni parziali](partial.md).

Per impostazione predefinita, è necessario chiamare ogni layout `RenderBody`. Quando la chiamata a `RenderBody` è inserito, verrà visualizzato il contenuto della visualizzazione.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Sezioni

Un layout può fare riferimento facoltativamente uno o più *sezioni*, chiamando `RenderSection`. Che forniscono un modo per organizzare in cui determinati elementi di pagina devono essere inseriti. Ogni chiamata a `RenderSection` possibile specificare se tale sezione è obbligatoria o facoltativa. Se una sezione richiesta non viene trovata, verrà generata un'eccezione. Viste singole specificare il contenuto da sottoporre a rendering all'interno di una sezione con il `@section` sintassi Razor. Se una vista definisce una sezione, è necessario eseguire il rendering o si verifica un errore.

Un esempio `@section` definizione in una vista:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

Nel codice precedente, vengono aggiunti gli script di convalida per il `scripts` sezione in una vista che include un modulo. Altre visualizzazioni nella stessa applicazione possono non richiedere eventuali altri script e pertanto non sarà necessario definire una sezione di script.

Nelle sezioni definite in una vista sono disponibili solo nella relativa pagina di layout immediato. È possibile farvi riferimento da parziali, Visualizza i componenti o da altre parti del sistema di visualizzazione.

### <a name="ignoring-sections"></a>Ignorare le sezioni

Per impostazione predefinita, il corpo e tutte le sezioni in una pagina di contenuto devono tutti essere rendering pagina di layout. Il motore di visualizzazione Razor questo comportamento viene imposto da verificare se il corpo e ogni sezione è stato eseguito il rendering.

Per indicare al motore di visualizzazione per ignorare il corpo o sezioni, chiamare il `IgnoreBody` e `IgnoreSection` metodi.

Il corpo e tutte le sezioni in una pagina Razor deve essere eseguito il rendering o ignorati.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>L'importazione delle direttive condivise

Viste è possono utilizzare le direttive Razor per eseguire molte operazioni, ad esempio l'importazione di spazi dei nomi o l'esecuzione di [inserimento di dipendenze](dependency-injection.md). Direttive condivise da numerose visualizzazioni possono essere specificate in una comune `_ViewImports.cshtml` file. Il `_ViewImports` file supporta le direttive seguenti:

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

Il file non supporta altre funzionalità di Razor, ad esempio le funzioni e le definizioni di sezione.

Un esempio `_ViewImports.cshtml` file:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

Il `_ViewImports.cshtml` file per un'applicazione ASP.NET MVC di base in genere viene posizionata nel `Views` cartella. Oggetto `_ViewImports.cshtml` file può essere posizionato in qualsiasi cartella, nel quale caso verranno applicata solo a viste all'interno di tale cartella e nelle relative sottocartelle. `_ViewImports`i file vengono elaborati a partire a livello di radice, e quindi per ogni cartella iniziali fino al percorso della visualizzazione stessa, che le impostazioni specificate a livello di radice può essere sottoposto a override a livello di cartella.

Ad esempio, se un livello di radice `_ViewImports.cshtml` file specifica `@model` e `@addTagHelper`e un altro `_ViewImports.cshtml` specifica un altro file nella cartella della visualizzazione associata al controller `@model` e aggiunge un altro `@addTagHelper`, la visualizzazione sarà possibile accedere a entrambi gli helper di tag e utilizzerà il secondo `@model`.

Se più `_ViewImports.cshtml` i file vengono eseguiti per una vista, combinati comportamento delle direttive di cui è incluso nel `ViewImports.cshtml` file saranno come indicato di seguito:

* `@addTagHelper`, `@removeTagHelper`: eseguiti nell'ordine

* `@tagHelperPrefix`: il più vicino alla vista esegue l'override di tutte le altre

* `@model`: il più vicino alla vista esegue l'override di tutte le altre

* `@inherits`: il più vicino alla vista esegue l'override di tutte le altre

* `@using`: tutti sono inclusi. i duplicati vengono ignorati

* `@inject`: per ogni proprietà, il più vicino alla vista esegue l'override di tutti gli altri con lo stesso nome di proprietà

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Esecuzione di codice prima di ogni visualizzazione.

Se si dispone di codice è necessario eseguire prima di ogni visualizzazione, questo deve essere inserito nel `_ViewStart.cshtml` file. Per convenzione, il `_ViewStart.cshtml` file si trova nella `Views` cartella. Le istruzioni nella `_ViewStart.cshtml` vengono eseguiti prima di ogni visualizzazione completa (non di layout e le visualizzazioni parziali non). Ad esempio [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` è di tipo gerarchico. Se un `_ViewStart.cshtml` file è definito nella cartella di visualizzazione associata al controller, verrà eseguito dopo quello definito nella radice di `Views` cartella (se presente).

Un esempio `_ViewStart.cshtml` file:

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Il file precedente specifica che tutte le viste useranno il `_Layout.cshtml` layout.

> [!NOTE]
> Né `_ViewStart.cshtml` né `_ViewImports.cshtml` vengono generalmente posizionati il `/Views/Shared` cartella. Le versioni a livello di applicazione di questi file devono essere inserite direttamente nella `/Views` cartella.
