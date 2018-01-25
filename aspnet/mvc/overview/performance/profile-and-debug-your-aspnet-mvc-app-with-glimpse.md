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
ms.openlocfilehash: 9cfdced21251b482ca527dda9c3a698de77cc8ca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="60680-103">Profilo ed eseguire il debug dell'app ASP.NET MVC con sguardo rapido</span><span class="sxs-lookup"><span data-stu-id="60680-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="60680-104">Da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="60680-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="60680-105">Panoramica di è un che intraprendono e in continuo aumento famiglia di pacchetti NuGet open source che offre prestazioni dettagliati, debug e informazioni di diagnostica per App ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="60680-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="60680-106">È semplice da installare, leggere, ultra-veloce e visualizza le metriche di prestazioni chiave nella parte inferiore di ogni pagina.</span><span class="sxs-lookup"><span data-stu-id="60680-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="60680-107">Consente di eseguire il drill down in app quando è necessario sapere cosa accade nel server.</span><span class="sxs-lookup"><span data-stu-id="60680-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="60680-108">Sguardo rapido fornisce informazioni molto utili, che si consiglia di che usare in tutto il ciclo di sviluppo, tra cui l'ambiente di testing di Azure.</span><span class="sxs-lookup"><span data-stu-id="60680-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="60680-109">Mentre [Fiddler](http://www.telerik.com/fiddler) e [gli strumenti di sviluppo F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) forniscono un lato client, sguardo rapido fornisce una visualizzazione dettagliata dal server.</span><span class="sxs-lookup"><span data-stu-id="60680-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="60680-110">In questa esercitazione sarà incentrata sull'uso di panoramica di ASP.NET MVC e dei pacchetti di Entity Framework, ma sono disponibili molti altri pacchetti.</span><span class="sxs-lookup"><span data-stu-id="60680-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="60680-111">Dove possibile verrà collegare appropriati [osservare docs](http://getglimpse.com/Docs/) che consentono di gestire.</span><span class="sxs-lookup"><span data-stu-id="60680-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="60680-112">Panoramica di è un progetto open source, è troppo possono contribuire al codice sorgente e i documenti.</span><span class="sxs-lookup"><span data-stu-id="60680-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="60680-113">Panoramica di installazione</span><span class="sxs-lookup"><span data-stu-id="60680-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="60680-114">Abilitare sguardo rapido per localhost</span><span class="sxs-lookup"><span data-stu-id="60680-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="60680-115">La scheda Cronologia</span><span class="sxs-lookup"><span data-stu-id="60680-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="60680-116">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="60680-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="60680-117">Route</span><span class="sxs-lookup"><span data-stu-id="60680-117">Routes</span></span>](#route)
- [<span data-ttu-id="60680-118">Uso di collegamenti in Azure</span><span class="sxs-lookup"><span data-stu-id="60680-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="60680-119">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="60680-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="60680-120">Panoramica di installazione</span><span class="sxs-lookup"><span data-stu-id="60680-120">Installing Glimpse</span></span>

<span data-ttu-id="60680-121">È possibile installare sguardo rapido dalla console di gestione di pacchetto NuGet o dal **Gestisci pacchetti NuGet** console.</span><span class="sxs-lookup"><span data-stu-id="60680-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="60680-122">Per questa dimostrazione, installano i pacchetti Mvc5 ed EF6:</span><span class="sxs-lookup"><span data-stu-id="60680-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![installare sguardo rapido da NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="60680-124">Cercare *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="60680-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF dalla finestra di dialogo Installazione NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="60680-126">Selezionando **i pacchetti installati**, è possibile visualizzare i moduli dipendenti sguardo rapido installati:</span><span class="sxs-lookup"><span data-stu-id="60680-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Installare pacchetti sguardo rapido dalla finestra di dialogo](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="60680-128">I seguenti comandi installano moduli MVC5 sguardo rapido ed EF6 dalla console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="60680-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="60680-129">Abilitare sguardo rapido per localhost</span><span class="sxs-lookup"><span data-stu-id="60680-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="60680-130">Passare a http://localhost:&lt;n. porta&gt;/glimpse.axd e fare clic su di **attiva sguardo rapido** pulsante.</span><span class="sxs-lookup"><span data-stu-id="60680-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the **Turn Glimpse On** button.</span></span>

![Pagina di panoramica axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="60680-132">Se è visualizzata la barra di Preferiti, è possibile trascinare e rilasciare i pulsanti di panoramica e aggiungerli come bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="60680-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Internet Explorer con boookmarklets sguardo rapido](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="60680-134">È ora possibile passare all'app e **testine di visualizzazione** (HUD) viene visualizzato nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="60680-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Pagina di gestione di contatto con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="60680-136">Il [pagina Panoramica HUD](http://getglimpse.com/Docs/Heads-up-Display) illustra in dettaglio le informazioni di temporizzazione illustrate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="60680-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="60680-137">Le visualizzazioni dati HUD prestazioni non intrusivo possono inviare una notifica di un problema - immediatamente prima di arrivare al ciclo di test.</span><span class="sxs-lookup"><span data-stu-id="60680-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="60680-138">Facendo clic su di &quot;g&quot; nell'angolo inferiore destro consente di visualizzare il pannello Panoramica:</span><span class="sxs-lookup"><span data-stu-id="60680-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Pannello Panoramica](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="60680-140">Nell'immagine precedente, il [scheda esecuzione](http://getglimpse.com/Docs/Execution-Tab) è selezionata, che mostra i dettagli dell'intervallo di azioni e i filtri nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="60680-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="60680-141">È possibile visualizzare il [timer filtro espressioni di controllo arrestare](http://www.nuget.org/packages/StopWatch/) avviare nella fase 6 della pipeline.</span><span class="sxs-lookup"><span data-stu-id="60680-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="60680-142">Mentre il timer leggero può fornire utili profilo/intervallo di dati, non vengono visualizzati tutti il tempo impiegato per l'autorizzazione e il rendering della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="60680-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="60680-143">Per ulteriori informazioni sul personale timer in [profilo e l'ora di applicazione MVC ASP.NET in Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="60680-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="60680-144">Il [schede](http://getglimpse.com/Docs/Tabs) pagina fornisce collegamenti a informazioni dettagliate su ogni scheda.</span><span class="sxs-lookup"><span data-stu-id="60680-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="60680-145">La scheda Cronologia</span><span class="sxs-lookup"><span data-stu-id="60680-145">The Timeline tab</span></span>

<span data-ttu-id="60680-146">Ho modificato di Tom Dykstra in attesa [esercitazione 6/MVC 5 EF](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) con il codice seguente modifica al controller istruttori:</span><span class="sxs-lookup"><span data-stu-id="60680-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="60680-147">Il codice precedente consente di passare la stringa di query (`eager`) al controllo eager o esplicito, il caricamento dei dati.</span><span class="sxs-lookup"><span data-stu-id="60680-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="60680-148">Nell'immagine seguente, viene utilizzato il caricamento esplicito e la pagina di temporizzazione Mostra ogni registrazione caricata nel `Index` metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="60680-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![caricamento esplicito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="60680-150">Nel codice seguente, viene specificato eager e ogni registrazione viene recuperata dopo il `Index` vista viene definita:</span><span class="sxs-lookup"><span data-stu-id="60680-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![eager specificato](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="60680-152">È possibile passare il mouse su un segmento di tempo per ottenere informazioni di intervallo dettagliati:</span><span class="sxs-lookup"><span data-stu-id="60680-152">You can hover over a time segment to get detailed timing information:</span></span>

![al passaggio del mouse per visualizzare di intervallo dettagliati](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="60680-154">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="60680-154">Model Binding</span></span>

<span data-ttu-id="60680-155">Il [scheda modello di associazione](http://getglimpse.com/Docs/Model-Binding-Tab) un'ampia gamma di informazioni che consentono di comprendere la modalità in cui sono associate le variabili di form e perché alcuni non associati come previsto.</span><span class="sxs-lookup"><span data-stu-id="60680-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="60680-156">L'immagine seguente viene illustrato il **?**</span><span class="sxs-lookup"><span data-stu-id="60680-156">The image below shows the **?**</span></span> <span data-ttu-id="60680-157">icona, è possibile fare clic per visualizzare la pagina di panoramica della Guida per la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="60680-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![osservare il visualizzazione modello di associazione](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="60680-159">Route</span><span class="sxs-lookup"><span data-stu-id="60680-159">Routes</span></span>

 <span data-ttu-id="60680-160">Scheda Panoramica route verrà consentono di eseguire il debug e comprendere il routing.</span><span class="sxs-lookup"><span data-stu-id="60680-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="60680-161">Nell'immagine seguente, verrà selezionata la route di prodotto e visualizzato in verde, una convenzione sguardo rapido.</span><span class="sxs-lookup"><span data-stu-id="60680-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="60680-162">![Nome prodotto selezionato](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) vengono visualizzati anche i token di dati, aree e i vincoli della Route.</span><span class="sxs-lookup"><span data-stu-id="60680-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="60680-163">Vedere [sguardo rapido route](http://getglimpse.com/Docs/Routes-Tab) e [attributo Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="60680-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="60680-164">Uso di collegamenti in Azure</span><span class="sxs-lookup"><span data-stu-id="60680-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="60680-165">I criteri di sicurezza predefinito di panoramica consentono solo dati sguardo rapido deve essere visualizzato dall'host locale.</span><span class="sxs-lookup"><span data-stu-id="60680-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="60680-166">È possibile modificare il criterio di protezione, quindi è possibile visualizzare questi dati in un server remoto (ad esempio un'app web in Azure).</span><span class="sxs-lookup"><span data-stu-id="60680-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="60680-167">Negli ambienti di test in Azure, aggiungere il contrassegno evidenziato fino a fondo il *confg* file per abilitare sguardo rapido:</span><span class="sxs-lookup"><span data-stu-id="60680-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="60680-168">Con questa modifica da solo, qualsiasi utente può visualizzare i dati di sguardo rapido in un sito remoto.</span><span class="sxs-lookup"><span data-stu-id="60680-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="60680-169">È possibile aggiungere il markup in precedenza per un profilo di pubblicazione in modo che ha distribuito solo un applicato quando si utilizza il profilo di pubblicazione (ad esempio, proifle il testing di Azure.) Per limitare i dati di panoramica, verrà aggiunto il `canViewGlimpseData` ruolo e consentire solo a utenti in questo ruolo per visualizzare i dati di panoramica.</span><span class="sxs-lookup"><span data-stu-id="60680-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="60680-170">Rimuovere i commenti dal *GlimpseSecurityPolicy.cs* file e modificare il [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) chiamare da `Administrator` per il `canViewGlimpseData` ruolo:</span><span class="sxs-lookup"><span data-stu-id="60680-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="60680-171">Sicurezza - i dati dettagliati forniti da sguardo rapido potrebbe esporre la sicurezza dell'app.</span><span class="sxs-lookup"><span data-stu-id="60680-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="60680-172">Microsoft non ha eseguito un controllo di sicurezza di Glimpse per l'utilizzo in applicazioni di produzione.</span><span class="sxs-lookup"><span data-stu-id="60680-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="60680-173">Per informazioni sull'aggiunta di ruoli, vedere il [distribuire un'app web Secure ASP.NET MVC 5 con appartenenza, OAuth e il Database SQL in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="60680-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="60680-174">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="60680-174">Additional Resources</span></span>

- [<span data-ttu-id="60680-175">Distribuire un'app protetta ASP.NET MVC 5 con appartenenza, OAuth e il Database SQL in Azure</span><span class="sxs-lookup"><span data-stu-id="60680-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="60680-176">[Osservare il configurazione](http://getglimpse.com/Docs/Configuration) -pagina Doc sulla configurazione delle schede, i criteri di runtime, registrazione e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="60680-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
