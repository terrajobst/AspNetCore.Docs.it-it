---
uid: mvc/overview/releases/mvc51-release-notes
title: What ' s New in ASP.NET MVC 5.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 6cdf5ae42457ab10e30693a1b77b4aee65570be5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831394"
---
<a name="whats-new-in-aspnet-mvc-51"></a>What ' s New in ASP.NET MVC 5.1
====================
da [Microsoft](https://github.com/microsoft)

Questo argomento descrive cosa sono le novità di ASP.NET Web MVC 5.1.

- [Requisiti software](#SoftwareRequirements)
- [Download](#download)
- [Documentazione](#documentation)
- [Nuove funzionalità in ASP.NET MVC 5.1](#new-features)

    - [Miglioramenti di routing dell'attributo](#AttributeRouting)
    - [Supporto di bootstrap per i modelli di editor](#Bootstrap)
    - [Supporto di enumerazione in visualizzazioni](#Enum)
    - [Convalida discreta per gli attributi di MinLength/MaxLength](#Unobtrusive)
    - [Supporto di contesto 'this' in Ajax Unobtrusive](#thisContext)
- [Problemi noti e modifiche di rilievo](#KnownBreakingChanges)- [correzioni di Bug](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Requisiti software

- Visual Studio 2012: Download [ASP.NET e Web Tools 2013.1 per Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064). Questo aggiornamento è necessaria per modificare le visualizzazioni Razor di ASP.NET MVC 5.1.

<a id="download"></a>
## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in NuGet gallery. Seguono tutti i pacchetti di runtime di [Versionamento semantico](http://semver.org/) specifica. Il pacchetto di ASP.NET MVC 5.1 RTM più recente è la seguente versione: "5.1.2". È possibile installare o aggiornare i pacchetti attraverso [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.

È possibile installare o aggiornare ai pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET MVC 5.1 RTM sono disponibili dal sito web ASP.NET ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Nuove funzionalità in ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Miglioramenti di routing dell'attributo

 Selezione delle route basata su attributi routing ora supporta i vincoli, abilitare il controllo delle versioni e l'intestazione. Molti aspetti della route con attributi sono ora personalizzabili tramite il `IDirectRouteFactory` interfaccia e `RouteFactoryAttribute` classe. Il prefisso della route è ora estendibile tramite le `IRoutePrefix` interfaccia e `RoutePrefixAttribute` classe. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Supporto di enumerazione in visualizzazioni

1. Nuovo `@Html.EnumDropDownListFor()` metodi helper. Devono essere usati come la maggior parte degli helper HTML precisando che l'espressione deve restituire un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo o un' [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) dove `T` è un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo. Usare `EnumHelper.IsValidForEnumHelper()` di verificare questi requisiti.
2. Nuove `EnumHelper.GetSelectList()` metodi che restituiscono un `IList<SelectListItem>`. Ciò è utile quando occorre modificare prima della chiamata, ad esempio, un elenco di selezione `@Html.DropDownListFor()`, o quando si vogliono visualizzare i nomi di cui `@Html.EnumDropDownListFor()` Mostra.

Il codice seguente illustra queste API.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

È possibile vedere un esempio completo [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Supporto di bootstrap per i modelli di editor

È ora consentito il passaggio negli attributi HTML nella [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) come un' [oggetto anonimo](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Ad esempio:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Convalida discreta per MinLengthAttribute e MaxLengthAttribute

La convalida lato client per i tipi stringa e matrice sarà ora supportata per le proprietà decorata con il [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) e [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributi.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Supporto di contesto 'this' in Ajax Unobtrusive

Le funzioni di callback (`OnBegin, OnComplete, OnFailure, OnSuccess`) a questo punto sarà in grado di individuare l'elemento chiamante tramite la `this` contesto. Ad esempio:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

### <a name="attribute-routing"></a>Routing con attributi

Ambiguità corrisponda al routing attributo ora segnalerà un errore anziché scelta la prima corrispondenza.

Le route con attributi non possono utilizzare il `{controller}` parametro e dall'uso di `{action}` parametro sulle route inserito in azioni. Usa questi parametri molto probabile che potrebbe causare ambiguità. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Lo scaffolding di MVC o Web API in un progetto con risultati pacchetti 5.1 nei 5.0 pacchetti per quelle che non esistono già nel progetto

Aggiornamento di pacchetti NuGet per ASP.NET MVC 5.1 RTM non aggiorna il modello di progetto applicazione Web ASP.NET o gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET. Usano la versione precedente dei pacchetti di runtime ASP.NET (versione=5.0.0.0). Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (versione=5.0.0.0) dei pacchetti necessari, se non sono già disponibili nei progetti. Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti più recenti nei progetti. Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento dei pacchetti dei progetti Web API 2.1 o ASP.NET MVC 5.1, assicurarsi che le versioni dell'API Web e ASP.NET MVC sono coerenti. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>L'evidenziazione della sintassi per le visualizzazioni Razor in Visual Studio 2013

Se si aggiorna alla versione RTM di ASP.NET MVC 5.1 senza aggiornare Visual Studio 2013, non si otterrà supporto all'editor di Visual Studio per l'evidenziazione della sintassi durante la modifica le visualizzazioni Razor. È necessario aggiornare Visual Studio 2013, per ottenere questo supporto. 

### <a name="type-renames"></a>Operazioni di ridenominazione di tipo

Alcuni dei tipi usati per l'estensibilità di routing di attributi sono stati rinominati in 5.1 RTM.

| **Vecchio nome del tipo (5.1 RC)** | **Nuovo tipo di nome (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correzioni di bug

Questa versione include anche diverse correzioni di bug. È possibile trovare l'elenco completo di seguito:

- [5.1.0 pacchetto](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pacchetto](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 il pacchetto contiene gli aggiornamenti di IntelliSense, ma non le correzioni di bug.
