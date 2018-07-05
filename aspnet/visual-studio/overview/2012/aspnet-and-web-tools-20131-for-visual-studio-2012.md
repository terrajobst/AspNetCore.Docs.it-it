---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Note sulla versione di ASP.NET and Web Tools 2013.1 per Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Questo documento descrive la versione di ASP.NET e Web Tools 2013.1 per Visual Studio 2012.
ms.author: aspnetcontent
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 33577dec7278694f932ac7eded84359baacf65bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829101"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="ff431-103">Note sulla versione di ASP.NET and Web Tools 2013.1 per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ff431-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="ff431-104">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ff431-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ff431-105">Questo documento descrive la versione di ASP.NET e Web Tools 2013.1 per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="ff431-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="ff431-106">Sommario</span><span class="sxs-lookup"><span data-stu-id="ff431-106">Contents</span></span>

- [<span data-ttu-id="ff431-107">Note sull'installazione</span><span class="sxs-lookup"><span data-stu-id="ff431-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="ff431-108">Requisiti software</span><span class="sxs-lookup"><span data-stu-id="ff431-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="ff431-109">Nuove funzionalità di ASP.NET and Web Tools 2013.1 per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ff431-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="ff431-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="ff431-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="ff431-111">Modelli</span><span class="sxs-lookup"><span data-stu-id="ff431-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="ff431-112">Modello di ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="ff431-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="ff431-113">Modello ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="ff431-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="ff431-114">Modelli di elemento</span><span class="sxs-lookup"><span data-stu-id="ff431-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="ff431-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="ff431-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="ff431-116">Scaffolding di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ff431-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="ff431-117">Editor Razor</span><span class="sxs-lookup"><span data-stu-id="ff431-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="ff431-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="ff431-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="ff431-119">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="ff431-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="ff431-120">Scaffolding di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ff431-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="ff431-121">MVC e Scaffolding API Web - HTTP 404, errore non trovato</span><span class="sxs-lookup"><span data-stu-id="ff431-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="ff431-122">Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di un elemento di scaffolding</span><span class="sxs-lookup"><span data-stu-id="ff431-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="ff431-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="ff431-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="ff431-124">Visualizzazione di file con estensione cshtml con Esplora con o F5 provoca un errore del server</span><span class="sxs-lookup"><span data-stu-id="ff431-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="ff431-125">Tilde(~) e riscrittura Url</span><span class="sxs-lookup"><span data-stu-id="ff431-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="ff431-126">Modelli</span><span class="sxs-lookup"><span data-stu-id="ff431-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="ff431-127">Note sull'installazione</span><span class="sxs-lookup"><span data-stu-id="ff431-127">Installation Notes</span></span>

<span data-ttu-id="ff431-128">[Installare](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET e Web Tools 2013.1 per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="ff431-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="ff431-129">Requisiti software</span><span class="sxs-lookup"><span data-stu-id="ff431-129">Software Requirements</span></span>

<span data-ttu-id="ff431-130">È necessario disporre di Visual Studio 2012 o Visual Studio Express 2012 per Web.</span><span class="sxs-lookup"><span data-stu-id="ff431-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="ff431-131">Nuove funzionalità di ASP.NET and Web Tools 2013.1 per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ff431-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="ff431-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="ff431-132">Bootstrap</span></span>

<span data-ttu-id="ff431-133">Quando si eseguire lo scaffolding di controller MVC 5 e visualizzazioni, viene utilizzato il markup per le viste [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="ff431-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="ff431-134">Modelli</span><span class="sxs-lookup"><span data-stu-id="ff431-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="ff431-135">Modello di ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="ff431-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="ff431-136">È stato aggiunto un nuovo modello MVC 5.</span><span class="sxs-lookup"><span data-stu-id="ff431-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="ff431-137">Fa riferimento a pacchetti NuGet di MVC 5 più recenti ed è possibile usare lo scaffolding per aggiungere controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="ff431-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="ff431-138">Modello ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="ff431-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="ff431-139">È stato aggiunto un nuovo modello API Web 2.</span><span class="sxs-lookup"><span data-stu-id="ff431-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="ff431-140">Fa riferimento agli ultimi pacchetti NuGet di Web API 2 ed è possibile usare lo scaffolding per aggiungere controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="ff431-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="ff431-141">Modelli di elemento</span><span class="sxs-lookup"><span data-stu-id="ff431-141">Item Templates</span></span>

<span data-ttu-id="ff431-142">È stato aggiunto nuovi modelli di elemento per le visualizzazioni MVC 5, Web Pages (Razor 3) e controller API Web 2.</span><span class="sxs-lookup"><span data-stu-id="ff431-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="ff431-143">Installazione di pacchetti NuGet correlati al progetto durante l'aggiunta di nuovi elementi.</span><span class="sxs-lookup"><span data-stu-id="ff431-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="ff431-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="ff431-144">Entity Framework 6</span></span>

<span data-ttu-id="ff431-145">Quando si eseguire lo scaffolding di un controller MVC o API Web Usa Entity Framework, usiamo Framework 6.</span><span class="sxs-lookup"><span data-stu-id="ff431-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="ff431-146">Per altre informazioni su Entity Framework, vedere la [cronologia delle versioni di Entity Framework](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="ff431-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="ff431-147">È anche possibile scaricare e installare gli strumenti di Entity Framework 6 per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="ff431-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="ff431-148">Vedere le [ottenere Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="ff431-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="ff431-149">Scaffolding di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ff431-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="ff431-150">Scaffolding di ASP.NET è un framework di generazione di codice per applicazioni Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ff431-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="ff431-151">Semplifica inoltre aggiungere il codice boilerplate per il progetto che interagisce con un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="ff431-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="ff431-152">Nelle versioni precedenti di Visual Studio, lo scaffolding era limitato ai progetti ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ff431-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="ff431-153">Con questo aggiornamento, è ora possibile usare lo scaffolding per qualsiasi progetto ASP.NET, tra cui Web Form.</span><span class="sxs-lookup"><span data-stu-id="ff431-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="ff431-154">Questo aggiornamento non supporta la generazione di pagine per un progetto di Web Form, ma è comunque possibile usare lo scaffolding con Web Form mediante l'aggiunta di dipendenze MVC al progetto.</span><span class="sxs-lookup"><span data-stu-id="ff431-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="ff431-155">Verrà aggiunto il supporto per la generazione di pagine per Web Form in un aggiornamento futuro.</span><span class="sxs-lookup"><span data-stu-id="ff431-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="ff431-156">Quando si usa lo scaffolding, si assicura che tutti i necessari le dipendenze siano installate nel progetto.</span><span class="sxs-lookup"><span data-stu-id="ff431-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="ff431-157">Ad esempio, se si avvia con un progetto di Web Form ASP.NET e quindi Usa lo scaffolding per aggiungere un Controller API Web, i pacchetti NuGet necessari e i riferimenti vengono aggiunti al progetto automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ff431-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="ff431-158">Per aggiungere lo scaffolding di MVC in un progetto di Web Form, aggiungere un **nuovo elemento di scaffolding** e selezionare **dipendenze MVC 5** nella finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="ff431-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="ff431-159">Sono disponibili due opzioni per lo scaffolding di MVC; Minimal e complete.</span><span class="sxs-lookup"><span data-stu-id="ff431-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="ff431-160">Se si seleziona minima, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="ff431-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="ff431-161">Se si seleziona l'opzione completa, vengono aggiunte le dipendenze minime, nonché i necessari file di contenuto per un progetto MVC.</span><span class="sxs-lookup"><span data-stu-id="ff431-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="ff431-162">Supporto per lo scaffolding di controller asincroni Usa le nuove funzionalità asincrone da Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="ff431-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="ff431-163">Per altre informazioni ed esercitazioni, vedere [Panoramica di Scaffolding ASP.NET](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ff431-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="ff431-164">Queste esercitazioni mostrano lo scaffolding in Visual Studio 2013, ma sono applicabili anche ad ASP.NET e Web Tools 2013.1 per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="ff431-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="ff431-165">Editor Razor</span><span class="sxs-lookup"><span data-stu-id="ff431-165">Razor Editor</span></span>

<span data-ttu-id="ff431-166">Con questo aggiornamento, Visual Studio 2012 supporta ora 3 Razor degli strumenti o la modifica.</span><span class="sxs-lookup"><span data-stu-id="ff431-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="ff431-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="ff431-167">NuGet 2.7</span></span>

<span data-ttu-id="ff431-168">NuGet 2.7 include un set completo delle nuove funzionalità descritte in dettaglio all'indirizzo [note sulla versione per NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="ff431-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="ff431-169">Questa versione di NuGet Elimina la necessità per gli utenti consentire esplicitamente NuGet per ripristinare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="ff431-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="ff431-170">Quando si installa NuGet 2.7, gli utenti il consenso in modo implicito per il ripristino automatico i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="ff431-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="ff431-171">In modo esplicito, gli utenti possono rifiutare esplicitamente il ripristino di pacchetti tramite le impostazioni di NuGet in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ff431-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="ff431-172">Questa modifica semplifica come funziona il ripristino del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="ff431-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="ff431-173">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="ff431-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="ff431-174">Scaffolding di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ff431-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="ff431-175">MVC e Scaffolding API Web - HTTP 404, errore non trovato</span><span class="sxs-lookup"><span data-stu-id="ff431-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="ff431-176">Se si verifica un errore durante l'aggiunta di un elemento di scaffolding per un progetto, è possibile che il progetto verrà lasciato in uno stato incoerente.</span><span class="sxs-lookup"><span data-stu-id="ff431-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="ff431-177">Alcune delle modifiche apportate da scaffolding verrà eseguito il rollback, ma altre modifiche, ad esempio i pacchetti NuGet installati, non essere il rollback.</span><span class="sxs-lookup"><span data-stu-id="ff431-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="ff431-178">Se le modifiche alla configurazione di routing viene eseguito il rollback, gli utenti riceveranno un errore HTTP 404 quando passare a scaffolding elementi.</span><span class="sxs-lookup"><span data-stu-id="ff431-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="ff431-179">Per correggere l'errore per MVC, aggiungere un nuovo elemento di scaffolding e selezionare dipendenze MVC 5 (minima o completa).</span><span class="sxs-lookup"><span data-stu-id="ff431-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="ff431-180">Questo processo verrà aggiunto a tutte le modifiche necessarie al progetto.</span><span class="sxs-lookup"><span data-stu-id="ff431-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="ff431-181">Per correggere l'errore per l'API Web:</span><span class="sxs-lookup"><span data-stu-id="ff431-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="ff431-182">Aggiungere la classe WebApiConfig seguente al progetto.</span><span class="sxs-lookup"><span data-stu-id="ff431-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="ff431-183">Configurare webapiconfig. Register nell'applicazione\_metodo Start in Global. asax come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="ff431-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="ff431-184">Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di un elemento di scaffolding</span><span class="sxs-lookup"><span data-stu-id="ff431-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="ff431-185">Se Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di elemento di scaffolding con Entity Framework (ad esempio Controller Web API 2 con azioni, che usa Entity Framework), è possibile che Visual Studio Express non è stato possibile caricare l'immagine nativa di un assembly dipende Extensions.</span><span class="sxs-lookup"><span data-stu-id="ff431-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="ff431-186">Per risolvere questo problema, configurare Visual Studio Express per lavorare con l'immagine MSIL di Extensions:</span><span class="sxs-lookup"><span data-stu-id="ff431-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="ff431-187">Aprire il prompt dei comandi in modalità amministratore.</span><span class="sxs-lookup"><span data-stu-id="ff431-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="ff431-188">Andare a %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE o % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (per Windows a 64 bit).</span><span class="sxs-lookup"><span data-stu-id="ff431-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="ff431-189">Aprire VWDExpress.exe.config in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="ff431-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="ff431-190">Aggiungere la riga seguente sotto il &lt;configuration&gt;/&lt;runtime&gt; elemento:</span><span class="sxs-lookup"><span data-stu-id="ff431-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="ff431-191">Riavviare Visual Studio Express 2012 per Web.</span><span class="sxs-lookup"><span data-stu-id="ff431-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="ff431-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="ff431-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="ff431-193">Visualizzazione di un errore del server withBrowse file con estensione cshtml WithorF5causes</span><span class="sxs-lookup"><span data-stu-id="ff431-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="ff431-194">Quando si crea un progetto MVC 5 in Visual Studio 2012 (o aperto nel progetto di Visual Studio 2012 un MVC 5 che è stato creato in Visual Studio 2013) e si tenta di visualizzare un file con estensione cshtml usando Esplora con o F5, si riceverà un errore che indica - **errore Server nel Applicazione '/'**.</span><span class="sxs-lookup"><span data-stu-id="ff431-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="ff431-195">Il server prova a passare a `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="ff431-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="ff431-196">Per risolvere questo problema, modificare il **azione di avvio** impostazione nel progetto per **pagina specifica**.</span><span class="sxs-lookup"><span data-stu-id="ff431-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="ff431-197">Non è necessaria fornire un valore per la pagina.</span><span class="sxs-lookup"><span data-stu-id="ff431-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="ff431-198">Dopo aver apportato questa modifica, la selezione di F5 consente di passare alla radice dell'applicazione (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="ff431-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="ff431-199">Questo comportamento non è identico a quello per i progetti MVC 5 in Visual Studio 2013, in cui il **pagina corrente** impostazione consente di avviare la pagina aperta.</span><span class="sxs-lookup"><span data-stu-id="ff431-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="ff431-200">Tilde(~) e riscrittura Url</span><span class="sxs-lookup"><span data-stu-id="ff431-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="ff431-201">Dopo l'aggiornamento 3 di Razor di ASP.NET o ASP.NET MVC 5, la notazione tilde(~) potrebbe non funzionare correttamente se si utilizza l'operazione di riscrittura URL.</span><span class="sxs-lookup"><span data-stu-id="ff431-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="ff431-202">La riscrittura di URL interessa la notazione tilde(~) negli elementi HTML quali &lt;A /&gt;, &lt;SCRIPT /&gt;, &lt;collegamento /&gt;, e di conseguenza la tilde non viene eseguito il mapping alla directory radice.</span><span class="sxs-lookup"><span data-stu-id="ff431-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="ff431-203">Ad esempio, se si riscrivono le richieste per **asp.net/content** al **asp.net**, l'attributo href &lt;href = "~/content/" /&gt; si risolve in **/content/ contenuti /** invece di **/**.</span><span class="sxs-lookup"><span data-stu-id="ff431-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="ff431-204">Per eliminare questa modifica, è possibile impostare il **IIS\_WasUrlRewritten** su false in ogni pagina Web o nel contesto **applicazione\_BeginRequest** in Global. asax.</span><span class="sxs-lookup"><span data-stu-id="ff431-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="ff431-205">Modelli</span><span class="sxs-lookup"><span data-stu-id="ff431-205">Templates</span></span>

<span data-ttu-id="ff431-206">Quando si crea ASP.NET MVC i progetti con Visual Studio 2012 in Windows 8.1 o Windows Server 2012 R2, Visual Studio viene visualizzato un messaggio di errore che indica "Configurazione Web [url] per ASP.NET 4.5 non è riuscita."</span><span class="sxs-lookup"><span data-stu-id="ff431-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Errore di configurazione](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="ff431-208">Questo errore viene visualizzato perché Visual Studio 2012 non abilita la funzionalità di ASP.NET 4.5 quando viene installato in tali versioni di Windows.</span><span class="sxs-lookup"><span data-stu-id="ff431-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="ff431-209">Per abilitare ASP.NET 4.5, seguire i passaggi descritti in [o disattivazione delle funzionalità Windows attivare](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="ff431-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![attivare o disattivare le funzionalità di Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="ff431-211">In alternativa, è possibile abilitare ASP.NET 4.5 tramite la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="ff431-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="ff431-212">Aprire il prompt dei comandi in modalità amministratore.</span><span class="sxs-lookup"><span data-stu-id="ff431-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="ff431-213">Eseguire il comando seguente per abilitare ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="ff431-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
