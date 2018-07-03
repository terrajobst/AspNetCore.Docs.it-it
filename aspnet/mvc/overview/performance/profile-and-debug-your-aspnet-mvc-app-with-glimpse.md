---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilatura e il debug dell'app ASP.NET MVC con Glimpse | Microsoft Docs
author: Rick-Anderson
description: Glimpse è che intraprendono e aumento delle dimensioni della famiglia di pacchetti NuGet open source che fornisce dettagliati sulle prestazioni, debug e informazioni di diagnostica per ASP.NET un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: ef65b512645d3f013ea34d3557da93254425c419
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391070"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Profilatura e il debug dell'app ASP.NET MVC con Glimpse
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Glimpse è che intraprendono e aumento delle dimensioni della famiglia di pacchetti NuGet open source che fornisce dettagliati sulle prestazioni, debug e informazioni di diagnostica per le app ASP.NET. È semplice da installare, leggero, eccezionalmente rapide e visualizza le metriche di prestazioni chiave nella parte inferiore di ogni pagina. Permette di drill-down per l'app quando è necessario scoprire cosa sta succedendo nel server. Rapida panoramica fornisce informazioni molto utili, che è consigliabile che usarla in tutto il ciclo di sviluppo, tra cui l'ambiente di test di Azure. Anche se [Fiddler](http://www.telerik.com/fiddler) e il [strumenti di sviluppo F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) forniscono un lato client, Glimpse fornisce una visualizzazione dettagliata del server. Questa esercitazione è incentrata sull'uso di Glimpse ASP.NET MVC e i pacchetti EF, ma sono disponibili molti altri pacchetti. Dove possibile collegherà appropriata [documentazione di Glimpse](http://getglimpse.com/Docs/) che consentono di gestire. Glimpse è un progetto open source, è anche possibile contribuire al codice sorgente e i documenti.


- [Installazione di Glimpse](#ig)
- [Abilitare Glimpse per localhost](#eg)
- [La scheda della sequenza temporale](#Time)
- [Associazione di modelli](#mb)
- [Route](#route)
- [Uso di Glimpse in Azure](#da)
- [Risorse aggiuntive](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Installazione di Glimpse

È possibile installare Glimpse dalla console di gestione pacchetti NuGet o dal **Gestisci pacchetti NuGet** console. Per questa dimostrazione, verrà installare i pacchetti Mvc5 e in EF6:

![installare Glimpse da NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Cercare *Glimpse.EF*

![Glimpse.EF dalla finestra di dialogo install NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Selezionando **i pacchetti installati**, è possibile visualizzare i moduli dipendenti Glimpse installati:

![Installare i pacchetti di Glimpse dalla finestra di dialogo](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

I seguenti comandi installano i moduli di Glimpse MVC5 e in EF6 dalla console di gestione pacchetti:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Abilitare Glimpse per localhost

Passare a http://localhost:&lt; porta &&gt;/glimpse.axd e fare clic sui <strong>Glimpse Turn On</strong> pulsante.

![Pagina axd glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Se hai sulla barra dei Preferiti visualizzata, è possibile trascinare e rilasciare i pulsanti di Glimpse e aggiungerli come bookmarklets:

![Internet Explorer con Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

È ora possibile passare all'app e il **testine di visualizzazione** (HUD) viene visualizzato nella parte inferiore della pagina.

![Pagina di Contact Manager con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

Il [pagina Glimpse HUD](http://getglimpse.com/Docs/Heads-up-Display) illustra in dettaglio le informazioni di temporizzazione illustrate in precedenza. Le visualizzazioni di dati HUD prestazioni unobtrusive possono inviare una notifica di un problema immediatamente - prima di procedere con il ciclo di test. Facendo clic sui &quot;g&quot; nell'angolo inferiore destro consente di visualizzare il pannello Panoramica:

![Pannello di panoramica](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Nell'immagine precedente, il [scheda esecuzione](http://getglimpse.com/Docs/Execution-Tab) è selezionata, che mostra i dettagli dell'intervallo di azioni e i filtri nella pipeline. È possibile visualizzare mio [timer filtro arrestare Watch](http://www.nuget.org/packages/StopWatch/) partono da fase 6 della pipeline. Anche se il timer leggero può fornire utili dati di profilo/temporizzazione, va perso, tutto il tempo impiegato nell'autorizzazione e il rendering della visualizzazione. Per ulteriori informazioni sul mio timer [del profilo e l'ora dell'app MVC ASP.NET completamente in Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). Il [schede](http://getglimpse.com/Docs/Tabs) pagina vengono forniti collegamenti a informazioni dettagliate su ogni scheda.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>La scheda della sequenza temporale

Ho modificato di Tom Dykstra in sospeso [esercitazione su EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) il codice seguente sostituire con il controller instructors (insegnanti):

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Il codice riportato sopra consente di passare la stringa di query (`eager`) al controllo eager o esplicita, il caricamento dei dati. Nell'immagine seguente, viene usato il caricamento esplicito e la temporizzazione pagina Mostra ogni registrazione caricati nel `Index` metodo di azione:

![caricamento esplicito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

Nel codice seguente, eager viene specificato e ogni registrazione viene recuperato dopo il `Index` vista viene definita:

![eager viene specificato](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

È possibile passare il mouse su un segmento di tempo per ottenere informazioni dettagliate sugli intervalli:

![al passaggio del mouse per visualizzare di intervallo dettagliati](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Associazione di modelli

Il [scheda modello di associazione](http://getglimpse.com/Docs/Model-Binding-Tab) offre un'ampia gamma di informazioni che consentono di comprendere come le variabili di form vengono associate e il motivo per cui alcuni non associati come si aspetterebbe. L'immagine seguente mostra il **?** icona, che è possibile selezionare per visualizzare la pagina della Guida di glimpse per tale funzionalità.

![visualizzazione modello di associazione di glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Route

 La scheda di Glimpse route può consente di eseguire il debug e comprendere il routing. Nell'immagine seguente, verrà selezionata la route di prodotto (e visualizzato in verde, una convenzione di Glimpse). ![nome del prodotto selezionato](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) vengono visualizzati anche i token di dati, aree e i vincoli di Route. Visualizzare [route Glimpse](http://getglimpse.com/Docs/Routes-Tab) e [Routing con attributi in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) per altre informazioni. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Uso di Glimpse in Azure

I criteri di sicurezza predefinito di Glimpse consentono solo dati di Glimpse deve essere visualizzato dall'host locale. È possibile modificare questo criterio di sicurezza in modo che è possibile visualizzare questi dati in un server remoto (ad esempio, un'app web in Azure). Per gli ambienti di test in Azure, aggiungere il marchio evidenziato fino a fondo le *web.confg* file per abilitare Glimpse:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Con questa modifica da solo, qualsiasi utente può visualizzare i dati di Glimpse in un sito remoto. È consigliabile aggiungere il markup precedente a un profilo di pubblicazione in modo che lo ha distribuito solo un applicato quando si usa tale profilo di pubblicazione (ad esempio, proifle i test di Azure.) Per limitare i dati di Glimpse, si aggiungerà il `canViewGlimpseData` ruolo e consentire solo agli utenti in questo ruolo per visualizzare i dati di Glimpse.

Rimuovere i commenti dal *GlimpseSecurityPolicy.cs* file e modificare le [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) chiamare da `Administrator` per il `canViewGlimpseData` ruolo:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Security - i dati dettagliati forniti da Glimpse potrebbe esporre la protezione dell'app. Microsoft non ha eseguito un controllo di sicurezza di Glimpse per l'uso nelle app di produzione.


Per informazioni sull'aggiunta di ruoli, vedere il [distribuire un'app web ASP.NET MVC 5 sicura con appartenenza, OAuth e Database SQL in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) esercitazione.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e Database SQL in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Configurazione di glimpse](http://getglimpse.com/Docs/Configuration) -pagina di documentazione su come configurare le schede, criteri di runtime, la registrazione e altro ancora.
