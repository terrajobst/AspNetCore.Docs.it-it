---
title: Layout in ASP.NET Core
author: ardalis
description: Informazioni su come usare layout comuni, condividere direttive ed eseguire codice comune prima di eseguire il rendering delle visualizzazioni in un'applicazione ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 8e89c8e6cf18c47abb6bf432cdc6bb6b97e8aeb0
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
ms.locfileid: "29904751"
---
# <a name="layout-in-aspnet-core"></a>Layout in ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

Le visualizzazioni condividono spesso elementi visivi e a livello di codice. In questo articolo si apprenderà come usare layout comuni, condividere direttive ed eseguire codice comune prima di eseguire il rendering delle visualizzazioni nell'applicazione ASP.NET.

## <a name="what-is-a-layout"></a>Che cos'è il layout?

La maggior parte delle applicazioni web presenta un layout comune che fornisce all'utente un'esperienza omogenea, nel passare da una pagina a un'altra. In genere, il layout comprende elementi dell'interfaccia utente comune, ad esempio l'intestazione dell'app, la navigazione o elementi di menu e piè di pagina.

![Esempio di layout di pagina](layout/_static/page-layout.png)

Molte pagine all'interno di un'app utilizzano anche strutture HTML comuni, come script e fogli di stile. Tutti questi elementi condivisi possono essere definiti in un file di *layout*, cui è possibile fare riferimento da qualsiasi visualizzazione utilizzata all'interno dell'app. I layout riducono il codice duplicato nelle visualizzazioni, consentendo di seguire il [principio Don't Repeat Yourself (DRY)](http://deviq.com/don-t-repeat-yourself/).

Per convenzione, il layout predefinito per un'app ASP.NET è denominato `_Layout.cshtml`. Il modello di progetto di Visual Studio ASP.NET Core MVC include questo file di layout nella cartella `Views/Shared`:

![cartella visualizzazioni in Esplora soluzioni](layout/_static/web-project-views.png)

Questo layout definisce un modello di livello superiore per le visualizzazioni incluse nell'app. Le app non richiedono un layout e possono definire più di un layout, con diverse visualizzazioni che specificano layout diversi.

Esempio di `_Layout.cshtml`:

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Definizione di un layout

Le visualizzazioni Razor hanno una proprietà `Layout`. Le visualizzazioni singole specificano un layout impostando la seguente proprietà:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Il layout specificato può utilizzare un percorso completo (esempio: `/Views/Shared/_Layout.cshtml`) o un nome parziale (esempio: `_Layout`). Quando viene fornito un nome parziale, il motore di visualizzazione Razor cerca il file di layout tramite il processo di individuazione standard. Viene innanzitutto cercata la cartella associata al controller, seguita dalla cartella `Shared`. Questo processo di individuazione è identico a quello utilizzato per individuare le [visualizzazioni parziali](partial.md).

Per impostazione predefinita, ogni layout deve chiamare `RenderBody`. Quando viene eseguita la chiamata a `RenderBody`, viene eseguito il rendering del contenuto della visualizzazione.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Sezioni

Un layout può facoltativamente fare riferimento a una o più *sezioni*, chiamando `RenderSection`. Le sezioni forniscono un modo per organizzare la posizione in cui devono essere inseriti determinati elementi di pagina. Ogni chiamata a `RenderSection` può specificare se tale sezione è obbligatoria o facoltativa. Se una sezione richiesta non viene trovata, verrà generata un'eccezione. Le viste singole specificano il contenuto da sottoporre a rendering all'interno di una sezione tramite la sintassi Razor `@section`. Se una visualizzazione definisce una sezione, è necessario eseguirne il rendering (o si verifica un errore).

Esempio di definizione `@section` in una visualizzazione:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

Nel codice precedente vengono aggiunti script di convalida alla sezione `scripts` in una visualizzazione che include un modulo. Altre visualizzazioni nella stessa applicazione potrebbero non richiedere altri script, pertanto non sarà necessario definire una sezione di script.

Le sezioni definite in una visualizzazione sono disponibili solo nella relativa pagina di layout immediato. Non è possibile farvi riferimento da righe parzialmente eseguite, componenti di visualizzazione o altre parti del sistema di visualizzazione.

### <a name="ignoring-sections"></a>Esclusione di sezioni

Per impostazione predefinita, la pagina di layout deve eseguire il rendering della parte principale e di tutte le sezioni di una pagina di contenuto. Il motore di visualizzazione Razor impone questa operazione verificando se è stato eseguito il rendering della parte principale e di ogni sezione.

Per indicare al motore di visualizzazione di escludere la parte principale o le sezioni, chiamare i metodi `IgnoreBody` e `IgnoreSection`.

Deve essere eseguito il rendering della parte principale e di tutte le sezioni di una pagina Razor oppure devono essere escluse.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Importazione delle direttive condivise

Le visualizzazioni possono utilizzare le direttive Razor per eseguire molte operazioni, ad esempio l'importazione di spazi dei nomi o l'esecuzione dell'[inserimento di dipendenze](dependency-injection.md). Le direttive condivise da numerose visualizzazioni possono essere specificate in un file `_ViewImports.cshtml` comune. Il file `_ViewImports` supporta le direttive seguenti:

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

Il file non supporta altre funzionalità di Razor, come le funzioni e le definizioni di sezione.

Esempio di file `_ViewImports.cshtml`:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

Il file `_ViewImports.cshtml` per un'applicazione ASP.NET Core MVC viene generalmente posizionato nella cartella `Views`. È possibile collocare un file `_ViewImports.cshtml` in qualsiasi cartella, nel qual caso verrà applicato unicamente alle visualizzazioni all'interno di tale cartella e nelle relative sottocartelle. I file `_ViewImports` vengono elaborati a partire dal livello radice e successivamente per ogni cartella collegata al percorso della visualizzazione stessa, pertanto le impostazioni specificate al livello radice possono essere sottoposte a override a livello di cartella.

Ad esempio, se un file `_ViewImports.cshtml` a livello radice specifica `@model` e `@addTagHelper`, e un altro file `_ViewImports.cshtml` nella cartella associata al controller della visualizzazione specifica un file `@model` diverso e aggiunge un altro `@addTagHelper`, la visualizzazione avrà accesso a entrambi gli helper di tag e userà il secondo `@model`.

Se vengono eseguiti più file `_ViewImports.cshtml` per una visualizzazione, il comportamento combinato delle direttive incluse nei file `ViewImports.cshtml` sarà:

* `@addTagHelper`, `@removeTagHelper`: eseguiti nell'ordine

* `@tagHelperPrefix`: il file più vicino alla visualizzazione esegue l'override di tutti gli altri

* `@model`: il file più vicino alla visualizzazione esegue l'override di tutti gli altri

* `@inherits`: il file più vicino alla visualizzazione esegue l'override di tutti gli altri

* `@using`: vengono inclusi tutti; i duplicati vengono esclusi

* `@inject`: per ogni proprietà, quella più vicina alla visualizzazione esegue l'override di tutte le altre con lo stesso nome di proprietà

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Esecuzione di codice prima di ogni visualizzazione

Se si dispone di un codice che è necessario eseguire prima di ogni visualizzazione, è necessario inserirlo nel file `_ViewStart.cshtml`. Per convenzione, il file `_ViewStart.cshtml` si trova nella cartella `Views`. Le istruzioni elencate in `_ViewStart.cshtml` vengono eseguite prima di ogni visualizzazione completa (non dei layout e delle visualizzazioni parziali). Come [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` è di tipo gerarchico. Se un file `_ViewStart.cshtml` è definito nella cartella di visualizzazione associata al controller, verrà eseguito dopo quello definito nella radice della cartella `Views` (se presente).

Esempio di file `_ViewStart.cshtml`:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Il file precedente specifica che tutte le visualizzazioni useranno il layout `_Layout.cshtml`.

> [!NOTE]
> Né `_ViewStart.cshtml` né `_ViewImports.cshtml` vengono generalmente posizionati nella cartella `/Views/Shared`. Le versioni a livello di app di questi file devono essere inserite direttamente nella cartella `/Views`.
