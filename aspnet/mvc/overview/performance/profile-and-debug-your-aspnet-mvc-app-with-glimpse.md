---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilatura e il debug dell'app ASP.NET MVC con Glimpse | Microsoft Docs
author: Rick-Anderson
description: Glimpse è che intraprendono e aumento delle dimensioni della famiglia di pacchetti NuGet open source che fornisce dettagliati sulle prestazioni, debug e informazioni di diagnostica per ASP.NET un...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: f5d174ff6823d654a24dcb2c90f10a3cbd24f1e7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829293"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="4cfd3-103">Profilatura e il debug dell'app ASP.NET MVC con Glimpse</span><span class="sxs-lookup"><span data-stu-id="4cfd3-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="4cfd3-104">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="4cfd3-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="4cfd3-105">Glimpse è che intraprendono e aumento delle dimensioni della famiglia di pacchetti NuGet open source che fornisce dettagliati sulle prestazioni, debug e informazioni di diagnostica per le app ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="4cfd3-106">È semplice da installare, leggero, eccezionalmente rapide e visualizza le metriche di prestazioni chiave nella parte inferiore di ogni pagina.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="4cfd3-107">Permette di drill-down per l'app quando è necessario scoprire cosa sta succedendo nel server.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="4cfd3-108">Rapida panoramica fornisce informazioni molto utili, che è consigliabile che usarla in tutto il ciclo di sviluppo, tra cui l'ambiente di test di Azure.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="4cfd3-109">Anche se [Fiddler](http://www.telerik.com/fiddler) e il [strumenti di sviluppo F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) forniscono un lato client, Glimpse fornisce una visualizzazione dettagliata del server.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="4cfd3-110">Questa esercitazione è incentrata sull'uso di Glimpse ASP.NET MVC e i pacchetti EF, ma sono disponibili molti altri pacchetti.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="4cfd3-111">Dove possibile collegherà appropriata [documentazione di Glimpse](http://getglimpse.com/Docs/) che consentono di gestire.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="4cfd3-112">Glimpse è un progetto open source, è anche possibile contribuire al codice sorgente e i documenti.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="4cfd3-113">Installazione di Glimpse</span><span class="sxs-lookup"><span data-stu-id="4cfd3-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="4cfd3-114">Abilitare Glimpse per localhost</span><span class="sxs-lookup"><span data-stu-id="4cfd3-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="4cfd3-115">La scheda della sequenza temporale</span><span class="sxs-lookup"><span data-stu-id="4cfd3-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="4cfd3-116">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="4cfd3-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="4cfd3-117">Route</span><span class="sxs-lookup"><span data-stu-id="4cfd3-117">Routes</span></span>](#route)
- [<span data-ttu-id="4cfd3-118">Uso di Glimpse in Azure</span><span class="sxs-lookup"><span data-stu-id="4cfd3-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="4cfd3-119">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4cfd3-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="4cfd3-120">Installazione di Glimpse</span><span class="sxs-lookup"><span data-stu-id="4cfd3-120">Installing Glimpse</span></span>

<span data-ttu-id="4cfd3-121">È possibile installare Glimpse dalla console di gestione pacchetti NuGet o dal **Gestisci pacchetti NuGet** console.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="4cfd3-122">Per questa dimostrazione, verrà installare i pacchetti Mvc5 e in EF6:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![installare Glimpse da NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="4cfd3-124">Cercare *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="4cfd3-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF dalla finestra di dialogo install NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="4cfd3-126">Selezionando **i pacchetti installati**, è possibile visualizzare i moduli dipendenti Glimpse installati:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Installare i pacchetti di Glimpse dalla finestra di dialogo](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="4cfd3-128">I seguenti comandi installano i moduli di Glimpse MVC5 e in EF6 dalla console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="4cfd3-129">Abilitare Glimpse per localhost</span><span class="sxs-lookup"><span data-stu-id="4cfd3-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="4cfd3-130">Passare a http://localhost:&lt; porta &&gt;/glimpse.axd e fare clic sui <strong>Glimpse Turn On</strong> pulsante.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Pagina axd glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="4cfd3-132">Se hai sulla barra dei Preferiti visualizzata, è possibile trascinare e rilasciare i pulsanti di Glimpse e aggiungerli come bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Internet Explorer con Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="4cfd3-134">È ora possibile passare all'app e il **testine di visualizzazione** (HUD) viene visualizzato nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Pagina di Contact Manager con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="4cfd3-136">Il [pagina Glimpse HUD](http://getglimpse.com/Docs/Heads-up-Display) illustra in dettaglio le informazioni di temporizzazione illustrate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="4cfd3-137">Le visualizzazioni di dati HUD prestazioni unobtrusive possono inviare una notifica di un problema immediatamente - prima di procedere con il ciclo di test.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="4cfd3-138">Facendo clic sui &quot;g&quot; nell'angolo inferiore destro consente di visualizzare il pannello Panoramica:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Pannello di panoramica](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="4cfd3-140">Nell'immagine precedente, il [scheda esecuzione](http://getglimpse.com/Docs/Execution-Tab) è selezionata, che mostra i dettagli dell'intervallo di azioni e i filtri nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="4cfd3-141">È possibile visualizzare mio [timer filtro arrestare Watch](http://www.nuget.org/packages/StopWatch/) partono da fase 6 della pipeline.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="4cfd3-142">Anche se il timer leggero può fornire utili dati di profilo/temporizzazione, va perso, tutto il tempo impiegato nell'autorizzazione e il rendering della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="4cfd3-143">Per ulteriori informazioni sul mio timer [del profilo e l'ora dell'app MVC ASP.NET completamente in Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="4cfd3-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="4cfd3-144">Il [schede](http://getglimpse.com/Docs/Tabs) pagina vengono forniti collegamenti a informazioni dettagliate su ogni scheda.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="4cfd3-145">La scheda della sequenza temporale</span><span class="sxs-lookup"><span data-stu-id="4cfd3-145">The Timeline tab</span></span>

<span data-ttu-id="4cfd3-146">Ho modificato di Tom Dykstra in sospeso [esercitazione su EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) il codice seguente sostituire con il controller instructors (insegnanti):</span><span class="sxs-lookup"><span data-stu-id="4cfd3-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="4cfd3-147">Il codice riportato sopra consente di passare la stringa di query (`eager`) al controllo eager o esplicita, il caricamento dei dati.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="4cfd3-148">Nell'immagine seguente, viene usato il caricamento esplicito e la temporizzazione pagina Mostra ogni registrazione caricati nel `Index` metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![caricamento esplicito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="4cfd3-150">Nel codice seguente, eager viene specificato e ogni registrazione viene recuperato dopo il `Index` vista viene definita:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![eager viene specificato](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="4cfd3-152">È possibile passare il mouse su un segmento di tempo per ottenere informazioni dettagliate sugli intervalli:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-152">You can hover over a time segment to get detailed timing information:</span></span>

![al passaggio del mouse per visualizzare di intervallo dettagliati](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="4cfd3-154">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="4cfd3-154">Model Binding</span></span>

<span data-ttu-id="4cfd3-155">Il [scheda modello di associazione](http://getglimpse.com/Docs/Model-Binding-Tab) offre un'ampia gamma di informazioni che consentono di comprendere come le variabili di form vengono associate e il motivo per cui alcuni non associati come si aspetterebbe.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="4cfd3-156">L'immagine seguente mostra il **?**</span><span class="sxs-lookup"><span data-stu-id="4cfd3-156">The image below shows the **?**</span></span> <span data-ttu-id="4cfd3-157">icona, che è possibile selezionare per visualizzare la pagina della Guida di glimpse per tale funzionalità.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![visualizzazione modello di associazione di glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="4cfd3-159">Route</span><span class="sxs-lookup"><span data-stu-id="4cfd3-159">Routes</span></span>

 <span data-ttu-id="4cfd3-160">La scheda di Glimpse route può consente di eseguire il debug e comprendere il routing.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="4cfd3-161">Nell'immagine seguente, verrà selezionata la route di prodotto (e visualizzato in verde, una convenzione di Glimpse).</span><span class="sxs-lookup"><span data-stu-id="4cfd3-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="4cfd3-162">![nome del prodotto selezionato](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) vengono visualizzati anche i token di dati, aree e i vincoli di Route.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="4cfd3-163">Visualizzare [route Glimpse](http://getglimpse.com/Docs/Routes-Tab) e [Routing con attributi in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="4cfd3-164">Uso di Glimpse in Azure</span><span class="sxs-lookup"><span data-stu-id="4cfd3-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="4cfd3-165">I criteri di sicurezza predefinito di Glimpse consentono solo dati di Glimpse deve essere visualizzato dall'host locale.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="4cfd3-166">È possibile modificare questo criterio di sicurezza in modo che è possibile visualizzare questi dati in un server remoto (ad esempio, un'app web in Azure).</span><span class="sxs-lookup"><span data-stu-id="4cfd3-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="4cfd3-167">Per gli ambienti di test in Azure, aggiungere il marchio evidenziato fino a fondo le *web.confg* file per abilitare Glimpse:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="4cfd3-168">Con questa modifica da solo, qualsiasi utente può visualizzare i dati di Glimpse in un sito remoto.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="4cfd3-169">È consigliabile aggiungere il markup precedente a un profilo di pubblicazione in modo che lo ha distribuito solo un applicato quando si usa tale profilo di pubblicazione (ad esempio, proifle i test di Azure.) Per limitare i dati di Glimpse, si aggiungerà il `canViewGlimpseData` ruolo e consentire solo agli utenti in questo ruolo per visualizzare i dati di Glimpse.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="4cfd3-170">Rimuovere i commenti dal *GlimpseSecurityPolicy.cs* file e modificare le [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) chiamare da `Administrator` per il `canViewGlimpseData` ruolo:</span><span class="sxs-lookup"><span data-stu-id="4cfd3-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="4cfd3-171">Security - i dati dettagliati forniti da Glimpse potrebbe esporre la protezione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="4cfd3-172">Microsoft non ha eseguito un controllo di sicurezza di Glimpse per l'uso nelle app di produzione.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="4cfd3-173">Per informazioni sull'aggiunta di ruoli, vedere il [distribuire un'app web ASP.NET MVC 5 sicura con appartenenza, OAuth e Database SQL in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="4cfd3-174">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4cfd3-174">Additional Resources</span></span>

- [<span data-ttu-id="4cfd3-175">Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e Database SQL in Azure</span><span class="sxs-lookup"><span data-stu-id="4cfd3-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="4cfd3-176">[Configurazione di glimpse](http://getglimpse.com/Docs/Configuration) -pagina di documentazione su come configurare le schede, criteri di runtime, la registrazione e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="4cfd3-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
