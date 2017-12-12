---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilo ed eseguire il debug dell'app ASP.NET MVC con panoramica | Documenti Microsoft
author: Rick-Anderson
description: "Panoramica è che intraprendono e famiglia di pacchetti NuGet open source che offre prestazioni dettagliati di crescita, debug e informazioni di diagnostica per ASP.NET un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 98b21a54ba00a8c82c3be7ba4e39d44041ed42c6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Profilo ed eseguire il debug dell'app ASP.NET MVC con sguardo rapido
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> Panoramica di è un che intraprendono e in continuo aumento famiglia di pacchetti NuGet open source che offre prestazioni dettagliati, debug e informazioni di diagnostica per App ASP.NET. È semplice da installare, leggere, ultra-veloce e visualizza le metriche di prestazioni chiave nella parte inferiore di ogni pagina. Consente di eseguire il drill down in app quando è necessario sapere cosa accade nel server. Sguardo rapido fornisce informazioni molto utili, che si consiglia di che usare in tutto il ciclo di sviluppo, tra cui l'ambiente di testing di Azure. Mentre [Fiddler](http://www.telerik.com/fiddler) e [gli strumenti di sviluppo F-12](https://msdn.microsoft.com/en-us/library/ie/gg589512(v=vs.85).aspx) forniscono un lato client, sguardo rapido fornisce una visualizzazione dettagliata dal server. In questa esercitazione sarà incentrata sull'uso di panoramica di ASP.NET MVC e dei pacchetti di Entity Framework, ma sono disponibili molti altri pacchetti. Dove possibile verrà collegare appropriati [osservare docs](http://getglimpse.com/Docs/) che consentono di gestire. Panoramica di è un progetto open source, è troppo possono contribuire al codice sorgente e i documenti.


- [Panoramica di installazione](#ig)
- [Abilitare sguardo rapido per localhost](#eg)
- [La scheda Cronologia](#Time)
- [Associazione di modelli](#mb)
- [Route](#route)
- [Uso di collegamenti in Azure](#da)
- [Risorse aggiuntive](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Panoramica di installazione

È possibile installare sguardo rapido dalla console di gestione di pacchetto NuGet o dal **Gestisci pacchetti NuGet** console. Per questa dimostrazione, installano i pacchetti Mvc5 ed EF6:

![installare sguardo rapido da NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Cercare *Glimpse.EF*

![Glimpse.EF dalla finestra di dialogo Installazione NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Selezionando **i pacchetti installati**, è possibile visualizzare i moduli dipendenti sguardo rapido installati:

![Installare pacchetti sguardo rapido dalla finestra di dialogo](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

I seguenti comandi installano moduli MVC5 sguardo rapido ed EF6 dalla console di gestione pacchetti:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Abilitare sguardo rapido per localhost

Passare a http://localhost:&lt;n. porta&gt;/glimpse.axd e fare clic su di **attiva sguardo rapido** pulsante.

![Pagina di panoramica axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Se è visualizzata la barra di Preferiti, è possibile trascinare e rilasciare i pulsanti di panoramica e aggiungerli come bookmarklets:

![Internet Explorer con boookmarklets sguardo rapido](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

È ora possibile passare all'app e **testine di visualizzazione** (HUD) viene visualizzato nella parte inferiore della pagina.

![Pagina di gestione di contatto con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

Il [pagina Panoramica HUD](http://getglimpse.com/Docs/Heads-up-Display) illustra in dettaglio le informazioni di temporizzazione illustrate in precedenza. Le visualizzazioni dati HUD prestazioni non intrusivo possono inviare una notifica di un problema - immediatamente prima di arrivare al ciclo di test. Facendo clic su di &quot;g&quot; nell'angolo inferiore destro consente di visualizzare il pannello Panoramica:

![Pannello Panoramica](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Nell'immagine precedente, il [scheda esecuzione](http://getglimpse.com/Docs/Execution-Tab) è selezionata, che mostra i dettagli dell'intervallo di azioni e i filtri nella pipeline. È possibile visualizzare il [timer filtro espressioni di controllo arrestare](http://www.nuget.org/packages/StopWatch/) avviare nella fase 6 della pipeline. Mentre il timer leggero può fornire utili profilo/intervallo di dati, non vengono visualizzati tutti il tempo impiegato per l'autorizzazione e il rendering della visualizzazione. Per ulteriori informazioni sul personale timer in [profilo e l'ora di applicazione MVC ASP.NET in Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). Il [schede](http://getglimpse.com/Docs/Tabs) pagina fornisce collegamenti a informazioni dettagliate su ogni scheda.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>La scheda Cronologia

Ho modificato di Tom Dykstra in attesa [esercitazione 6/MVC 5 EF](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) con il codice seguente modifica al controller istruttori:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Il codice precedente consente di passare la stringa di query (`eager`) al controllo eager o esplicito, il caricamento dei dati. Nell'immagine seguente, viene utilizzato il caricamento esplicito e la pagina di temporizzazione Mostra ogni registrazione caricata nel `Index` metodo di azione:

![caricamento esplicito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

Nel codice seguente, viene specificato eager e ogni registrazione viene recuperata dopo il `Index` vista viene definita:

![eager specificato](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

È possibile passare il mouse su un segmento di tempo per ottenere informazioni di intervallo dettagliati:

![al passaggio del mouse per visualizzare di intervallo dettagliati](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Associazione di modelli

Il [scheda modello di associazione](http://getglimpse.com/Docs/Model-Binding-Tab) un'ampia gamma di informazioni che consentono di comprendere la modalità in cui sono associate le variabili di form e perché alcuni non associati come previsto. L'immagine seguente viene illustrato il **?** icona, è possibile fare clic per visualizzare la pagina di panoramica della Guida per la funzionalità.

![osservare il visualizzazione modello di associazione](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Route

 Scheda Panoramica route verrà consentono di eseguire il debug e comprendere il routing. Nell'immagine seguente, verrà selezionata la route di prodotto e visualizzato in verde, una convenzione sguardo rapido. ![Nome prodotto selezionato](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) vengono visualizzati anche i token di dati, aree e i vincoli della Route. Vedere [sguardo rapido route](http://getglimpse.com/Docs/Routes-Tab) e [attributo Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) per ulteriori informazioni. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Uso di collegamenti in Azure

I criteri di sicurezza predefinito di panoramica consentono solo dati sguardo rapido deve essere visualizzato dall'host locale. È possibile modificare il criterio di protezione, quindi è possibile visualizzare questi dati in un server remoto (ad esempio un'app web in Azure). Negli ambienti di test in Azure, aggiungere il contrassegno evidenziato fino a fondo il *confg* file per abilitare sguardo rapido:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Con questa modifica da solo, qualsiasi utente può visualizzare i dati di sguardo rapido in un sito remoto. È possibile aggiungere il markup in precedenza per un profilo di pubblicazione in modo che ha distribuito solo un applicato quando si utilizza il profilo di pubblicazione (ad esempio, proifle il testing di Azure.) Per limitare i dati di panoramica, verrà aggiunto il `canViewGlimpseData` ruolo e consentire solo a utenti in questo ruolo per visualizzare i dati di panoramica.

Rimuovere i commenti dal *GlimpseSecurityPolicy.cs* file e modificare il [IsInRole](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) chiamare da `Administrator` per il `canViewGlimpseData` ruolo:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Sicurezza - i dati dettagliati forniti da sguardo rapido potrebbe esporre la sicurezza dell'app. Microsoft non ha eseguito un controllo di sicurezza di Glimpse per l'utilizzo in applicazioni di produzione.


Per informazioni sull'aggiunta di ruoli, vedere il [distribuire un'app web Secure ASP.NET MVC 5 con appartenenza, OAuth e il Database SQL in Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) esercitazione.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Distribuire un'app protetta ASP.NET MVC 5 con appartenenza, OAuth e il Database SQL in Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Osservare il configurazione](http://getglimpse.com/Docs/Configuration) -pagina Doc sulla configurazione delle schede, i criteri di runtime, registrazione e altro ancora.
