---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: Novità di ASP.NET MVC 5.2 | Documenti Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26504100"
---
<a name="whats-new-in-aspnet-mvc-52"></a>Novità di ASP.NET MVC 5.2
====================
da [Microsoft](https://github.com/microsoft)

In questo argomento vengono descritte le nuove per ASP.NET MVC 5.2, [italiano 5.2.2](#52) e [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [Requisiti software](#softRequire)
- [Download](#download)
- [Documentazione](#documentation)
- [Nuove funzionalità in ASP.NET MVC 5.2](#new-features)

    - [Attributo di miglioramenti di routing](#attributerouting)
- [Problemi noti e modifiche di rilievo](#knownbreakingchanges)
- [Correzioni di bug](#bug-fixes)
- [Italiano 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Requisiti software

- Visual Studio 2012: Download [2013.1 per Visual Studio 2012 degli strumenti Web ASP.NET e](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Download [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) o versione successiva. Questo aggiornamento è necessario per la modifica delle visualizzazioni Razor di ASP.NET MVC 5.2.

<a id="download"></a>
## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in Raccolta NuGet. Seguono tutti i pacchetti di runtime di [controllo delle versioni semantico](http://semver.org/) specifica. Il pacchetto di ASP.NET MVC 5.2 più recente è la seguente versione: "5.2.0". È possibile installare o aggiornare i pacchetti tramite [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.

È possibile installare o aggiornare i pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:

Italiano Install-Package-versione 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET MVC 5.2 sono disponibili dal sito web ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Nuove funzionalità in ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Attributo di miglioramenti di routing

Attributo Routing ora fornisce un punto di estensibilità chiamato IDirectRouteProvider, che consente il controllo completo sulla modalità di route di attributi vengono individuate e configurate. Un IDirectRouteProvider è responsabile di fornire un elenco di azioni e controller insieme alle informazioni relative all'itinerario associato per specificare esattamente quale configurazione di routing è desiderate per tali azioni. Un'implementazione IDirectRouteProvider può essere specificata quando si chiama MapAttributes/MapHttpAttributeRoutes.

Personalizzazione IDirectRouteProvider sarà più semplice estendendo l'implementazione predefinita, DefaultDirectRouteProvider. Questa classe fornisce metodi virtuali separati sottoponibile a override per modificare la logica per individuare gli attributi, la creazione di voci della route e individuare il prefisso di route e il prefisso dell'area.

Con il nuovo attributo routing estendibilità del IDirectRouteProvider, un utente può eseguire le operazioni seguenti:

1. Supportano l'ereditarietà delle route di attributo. Ad esempio, nello scenario seguente controller Blog e archivio utilizza una convenzione di route di attributo definito dal BaseController. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Genera automaticamente i nomi di route per la route di attributi. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Modificare i prefissi di route in un'unica posizione centrale prima che le route richiedere di essere aggiunti alla tabella di route.
4. Escludere i controller su cui eseguire il routing di attributo per cercare. Non appena verrà probabilmente blog su 3 e 4.

### <a name="facebook-fixes-for-changed-api-surface"></a>Correzioni di Facebook per modificate superficie dell'API

Il pacchetto di Facebook MVC [è stata interrotta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) a causa di alcune API modifiche apportate da Facebook. Verrà inoltre rilasciato un nuovo pacchetto di Facebook (italiano 1.0.0) per risolvere questi problemi.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Lo scaffolding di API Web MVC e in un progetto con 5.2.0 risultati di pacchetti in 5.1.2 pacchetti per quelle che non esistono già nel progetto

Aggiornamento dei pacchetti NuGet per MVC ASP.NET 5.2.0 non aggiornare gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET o il modello di progetto applicazione Web ASP.NET. Usano la versione precedente dei pacchetti di runtime ASP.NET (ad esempio 5.1.2 in Update 2). Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (ad esempio 5.1.2 in Update 2) di pacchetti richiesti, se non sono già disponibili nei progetti. Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti nei progetti più recenti. Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento di pacchetti dei progetti Web API 2.2 o ASP.NET MVC 5.2, assicurarsi che le versioni di API Web e ASP.NET MVC sono coerenti.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Installazione del pacchetto Microsoft.jQuery.Unobtrusive.Validation NuGet non riesce perché non è in grado di trovare una versione di Microsoft.jQuery.Unobtrusive.Validation compatibile per jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation richiede jQuery &gt;= 1.8 e jQuery.Validation &gt;= 1.8. JQuery è necessario but,jQuery.Validation (1.8) (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). Per questo motivo, quando si installa di NuGet di JQuery 1.8 e jQuery.Validation 1.8 allo stesso tempo, l'esito negativo. Se si verifica questo problema, è possibile limitarsi ad aggiornare la versione di jQuery.Validation per &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) che ha il limite di jQuery fissa prima di tutto, deve essere in grado di installare Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Il jquery. Versione del pacchetto nuget convalida 1.13.0 non riconosce alcuni indirizzi di posta elettronica internazionali

versione del pacchetto nuget jQuery.Validation 1.11.1 è l'ultima versione nota che riconosce i seguenti indirizzi di posta elettronica valido. Le versioni successive potrebbero non essere in grado di riconoscerle. Ad esempio:

Standard di internazionalizzazione di indirizzo (EAI) di posta elettronica (ad esempio, [&#29992; &#25143;@domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + Internationalized IRI (Resource Identifier) (ad es., [&#29992; &#25143; @&#1076; &#1086; &#1084; &#1077; &#1085;. &#1088; &#1092; ](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Il problema viene segnalato in [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Evidenziazione della sintassi per le visualizzazioni Razor in Visual Studio 2013

Se l'aggiornamento a ASP.NET MVC 5.2 senza l'aggiornamento di Visual Studio 2013, non si otterrà il supporto di editor di Visual Studio per l'evidenziazione della sintassi durante la modifica di visualizzazioni Razor. È necessario aggiornare Visual Studio 2013 per ottenere questo supporto.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correzioni di bug e aggiornamenti delle funzionalità secondarie

Questa versione include inoltre diverse correzioni di bug e le funzionalità secondarie gli aggiornamenti. È possibile trovare l'elenco completo di seguito:

- [pacchetto 5.2](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Italiano 5.2.2

Questa versione non contiene nuove funzionalità o correzioni di bug in MVC. Sono stati apportati un [modificare nelle pagine Web](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) per un miglioramento significativo delle prestazioni e successivamente aggiornare tutti gli altri pacchetti dipendente è proprietario per dipendere questa nuova versione di pagine Web.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

È possibile leggere informazioni sulle versioni [qui](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Questa versione include solo correzioni di bug. È possibile utilizzare [questa query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) per visualizzare l'elenco dei problemi risolti in questa versione.
