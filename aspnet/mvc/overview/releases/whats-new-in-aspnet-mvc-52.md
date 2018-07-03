---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: What ' s New in ASP.NET MVC 5.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: bcffaa9fddfb13db0b7bd203029e850903a13a54
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389961"
---
<a name="whats-new-in-aspnet-mvc-52"></a>What ' s New in ASP.NET MVC 5.2
====================
da [Microsoft](https://github.com/microsoft)

In questo argomento descrive cosa sono le novità di ASP.NET MVC 5.2, [ASPNET 5.2.2](#52) e [Beta di ASP.NET MVC 5.2.3](#mvc523Beta)

- [Requisiti software](#softRequire)
- [Download](#download)
- [Documentazione](#documentation)
- [Nuove funzionalità in ASP.NET MVC 5.2](#new-features)

    - [Miglioramenti di routing dell'attributo](#attributerouting)
- [Problemi noti e modifiche di rilievo](#knownbreakingchanges)
- [Correzioni di bug](#bug-fixes)
- [ASPNET 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Requisiti software

- Visual Studio 2012: Download [ASP.NET e Web Tools 2013.1 per Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Download [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) o versione successiva. Questo aggiornamento è necessaria per modificare le visualizzazioni Razor di ASP.NET MVC 5.2.

<a id="download"></a>
## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in NuGet gallery. Seguono tutti i pacchetti di runtime di [Versionamento semantico](http://semver.org/) specifica. Il pacchetto di ASP.NET MVC 5.2 più recente è la seguente versione: "5.2.0". È possibile installare o aggiornare i pacchetti attraverso [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.

È possibile installare o aggiornare ai pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:

Install-Package ASPNET-versione 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET MVC 5.2 sono disponibili dal sito web ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Nuove funzionalità in ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Miglioramenti di routing dell'attributo

A questo punto il Routing con attributi offre un punto di estensibilità chiamato IDirectRouteProvider, che consente il controllo completo su come le route con attributi vengono individuate e configurate. Un IDirectRouteProvider è responsabile di fornire un elenco di azioni e controller insieme alle informazioni sulle route associata a specificare esattamente quali configurazione di routing è desiderato per le azioni. Quando si chiama MapAttributes/MapHttpAttributeRoutes si può specificare un'implementazione IDirectRouteProvider.

Personalizzazione IDirectRouteProvider sarà più semplice estendendo l'implementazione predefinita, DefaultDirectRouteProvider. Questa classe fornisce metodi virtuali separati sottoponibile a override per modificare la logica per l'individuazione di attributi, la creazione di voci della route e l'individuazione di prefisso di route e il prefisso dell'area.

Con il nuovo attributo routing estensibilità di IDirectRouteProvider, un utente è stato possibile eseguire le operazioni seguenti:

1. Supporta l'ereditarietà di route con attributi. Ad esempio, nello scenario seguente post di Blog e Store controller Usa una convenzione di route di attributo definito dal BaseController. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Generare automaticamente i nomi di route per le route con attributi. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Modificare i prefissi di route in un'unica posizione centrale prima che le route vengono aggiunti alla tabella di route.
4. Escludere i controller in cui si desidera il routing con attributi da cercare. Ci auguriamo di blog su 3 e 4 a breve.

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook correzioni per la superficie dell'API modificate

Il pacchetto di Facebook MVC [è stata interrotta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) a causa di alcune API le modifiche apportate da Facebook. Verranno anche rilasciati un nuovo pacchetto di Facebook (Microsoft.AspNet.Facebook 1.0.0) per risolvere questi problemi.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Lo scaffolding MVC/API Web in un progetto con 5.2.0 risultati i pacchetti in 5.1.2 di pacchetti per quelle che non esistono già nel progetto

Aggiornamento di pacchetti NuGet per MVC ASP.NET 5.2.0 non aggiorna il modello di progetto applicazione Web ASP.NET o gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET. Usano la versione precedente dei pacchetti di runtime ASP.NET (ad esempio 5.1.2 nell'aggiornamento 2). Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (ad esempio 5.1.2 nell'aggiornamento 2) dei pacchetti necessari, se non sono già disponibili nei progetti. Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti più recenti nei progetti. Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento dei pacchetti dei progetti Web API 2.2 o ASP.NET MVC 5.2, assicurarsi che le versioni dell'API Web e ASP.NET MVC sono coerenti.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Installazione del pacchetto Microsoft.jQuery.Unobtrusive.Validation NuGet ha esito negativo perché non è in grado di trovare una versione di Microsoft.jQuery.Unobtrusive.Validation compatibile con jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation richiede jQuery &gt;= 1.8 e jQuery.Validation &gt;= 1.8. But,jQuery.Validation (1,8) richiede jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). Per questo motivo, quando NuGet installa il JQuery 1.8 e jQuery.Validation 1.8 nello stesso momento, non riesce. Quando si verifica questo problema, è possibile aggiornare semplicemente la versione di a jQuery.Validation &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) che contiene il delimitatore jQuery corretto in primo luogo, dovrebbe essere possibile installare Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Il componente aggiuntivo jquery. Versione del pacchetto nuget convalida 1.13.0 non riconosce alcuni indirizzi di posta elettronica internazionale

versione del pacchetto nuget jQuery.Validation 1.11.1 è l'ultima versione nota che riconosce seguendo gli indirizzi di posta elettronica valido. Le versioni successive potrebbero non essere in grado di riconoscerli. Ad esempio:

Standard di internazionalizzazione indirizzo (EAI) messaggio di posta elettronica (ad esempio, [ &#29992; &#25143; @domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + Internationalized Resource Identifier (IRI) (ad es., [ &#29992; &#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088; &#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Il problema viene segnalato in [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>L'evidenziazione della sintassi per le visualizzazioni Razor in Visual Studio 2013

Se l'aggiornamento a ASP.NET MVC 5.2 senza aggiornare Visual Studio 2013, non si otterrà supporto all'editor di Visual Studio per l'evidenziazione della sintassi durante la modifica le visualizzazioni Razor. È necessario aggiornare Visual Studio 2013, per ottenere questo supporto.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correzioni di bug e gli aggiornamenti delle funzionalità secondarie

Questa versione include anche diverse correzioni di bug e la funzionalità secondaria degli aggiornamenti. È possibile trovare l'elenco completo di seguito:

- [pacchetto 5.2](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>ASPNET 5.2.2

Questa versione non include nuove funzionalità o correzioni di bug in MVC. Sono state apportate una [modifica nelle pagine Web](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) per offrire un miglioramento significativo delle prestazioni e successivamente aggiornare tutti gli altri pacchetti dipendenti da questa nuova versione delle pagine Web.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>Versione Beta di ASP.NET MVC 5.2.3

È possibile leggere informazioni sul rilascio [qui](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Questa versione include correzioni di bug solo. È possibile usare [questa query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) per visualizzare l'elenco dei problemi risolti in questa versione.
