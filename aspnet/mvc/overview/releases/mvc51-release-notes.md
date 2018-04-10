---
uid: mvc/overview/releases/mvc51-release-notes
title: Novità di ASP.NET MVC 5.1 | Documenti Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
---
<a name="whats-new-in-aspnet-mvc-51"></a>Novità di ASP.NET MVC 5.1
====================
by [Microsoft](https://github.com/microsoft)

In questo argomento vengono descritte le novità di ASP.NET Web MVC 5.1.

- [Requisiti software](#SoftwareRequirements)
- [Download](#download)
- [Documentazione](#documentation)
- [Nuove funzionalità in ASP.NET MVC 5.1](#new-features)

    - [Attributo di miglioramenti di routing](#AttributeRouting)
    - [Bootstrap supporto per i modelli di editor](#Bootstrap)
    - [Supporto di enum nelle viste](#Enum)
    - [Convalida non intrusiva per gli attributi di MinLength/MaxLength](#Unobtrusive)
    - [Supporto di contesto 'this' in non intrusivo Ajax](#thisContext)
- [Problemi noti e modifiche di rilievo](#KnownBreakingChanges)- [correzioni di Bug](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Requisiti software

- Visual Studio 2012: Download [2013.1 per Visual Studio 2012 degli strumenti Web ASP.NET e](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064). Questo aggiornamento è necessario per la modifica delle visualizzazioni Razor di ASP.NET MVC 5.1.

<a id="download"></a>
## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in Raccolta NuGet. Seguono tutti i pacchetti di runtime di [controllo delle versioni semantico](http://semver.org/) specifica. Il pacchetto di ASP.NET MVC 5.1 RTM più recente è la seguente versione: "5.1.2". È possibile installare o aggiornare i pacchetti tramite [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.

È possibile installare o aggiornare i pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET MVC 5.1 RTM sono disponibili dal sito web ASP.NET ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Nuove funzionalità in ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Attributo di miglioramenti di routing

 Selezione delle route basata su attributo routing ora supporta i vincoli, abilitando il controllo delle versioni e intestazione. Molti aspetti delle route di attributo sono ora personalizzabili tramite il `IDirectRouteFactory` interfaccia e `RouteFactoryAttribute` classe. Il prefisso della route è estensibile tramite il `IRoutePrefix` interfaccia e `RoutePrefixAttribute` classe. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Supporto di enum nelle viste

1. Nuovo `@Html.EnumDropDownListFor()` metodi di supporto. Che dovrebbero essere utilizzate come la maggior parte degli helper HTML, tenendo però che l'espressione deve restituire un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo o un [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) dove `T` è un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo. Utilizzare `EnumHelper.IsValidForEnumHelper()` per verificare i requisiti seguenti.
2. Nuovo `EnumHelper.GetSelectList()` metodi che restituiscono un `IList<SelectListItem>`. Ciò è utile quando è necessario modificare un elenco di selezione prima di chiamare, ad esempio, `@Html.DropDownListFor()`, o quando si desidera visualizzare i nomi che `@Html.EnumDropDownListFor()` Mostra.

Il codice seguente illustra queste API.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

È possibile visualizzare un esempio completo [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Bootstrap supporto per i modelli di editor

È ora consente il passaggio negli attributi HTML nel [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) come un [oggetto anonimo](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Ad esempio:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Convalida non intrusiva per MinLengthAttribute e MaxLengthAttribute

Convalida lato client per i tipi stringa e matrice sarà ora supportata per le proprietà decorata con il [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) e [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributi.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Supporto di contesto 'this' in non intrusivo Ajax

Le funzioni di callback (`OnBegin, OnComplete, OnFailure, OnSuccess`) saranno in grado di individuare l'elemento chiamata tramite il `this` contesto. Ad esempio:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

### <a name="attribute-routing"></a>Routing degli attributi

Ambiguità di corrispondenze di routing attributo ora segnalerà un errore anziché scelta la prima corrispondenza.

Route di attributi vietate l'utilizzo di `{controller}` parametro e dall'utilizzo di `{action}` parametro route inserito in azioni. Utilizzi di questi parametri molto probabilmente potrebbe causare ambiguità. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Lo scaffolding di API Web MVC e in un progetto con risultati pacchetti 5.1 nei 5.0 pacchetti per quelle che non esistono già nel progetto

L'aggiornamento di pacchetti NuGet per ASP.NET MVC 5.1 RTM non aggiornare gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET o il modello di progetto applicazione Web ASP.NET. Usano la versione precedente dei pacchetti di runtime ASP.NET (5.0.0.0). Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (5.0.0.0) dei pacchetti richiesti, se non sono già disponibili nei progetti. Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti nei progetti più recenti. Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento di pacchetti dei progetti Web API 2.1 o ASP.NET MVC 5.1, assicurarsi che le versioni di API Web e ASP.NET MVC sono coerenti. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Evidenziazione della sintassi per le visualizzazioni Razor in Visual Studio 2013

Se l'aggiornamento alla versione RTM di ASP.NET MVC 5.1 senza l'aggiornamento di Visual Studio 2013, non si otterrà il supporto di editor di Visual Studio per l'evidenziazione della sintassi durante la modifica di visualizzazioni Razor. È necessario aggiornare Visual Studio 2013 per ottenere questo supporto. 

### <a name="type-renames"></a>Ridenominazione di tipo

Alcuni dei tipi utilizzati per l'estensibilità di routing di attributo vengono rinominati in RTM 5.1.

| **Nome di tipo precedente (5.1 RC)** | **Nuovo tipo di nome (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correzioni di bug

Questa versione include inoltre diverse correzioni di bug. È possibile trovare l'elenco completo di seguito:

- [5.1.0 pacchetto](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pacchetto](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

Il 5.1.2 pacchetto contiene gli aggiornamenti di IntelliSense ma non correzioni di bug.
