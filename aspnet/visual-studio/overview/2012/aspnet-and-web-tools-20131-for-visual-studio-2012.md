---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Note sulla versione di ASP.NET e strumenti Web 2013.1 per Visual Studio 2012 | Documenti Microsoft
author: microsoft
description: Questo documento descrive la versione di ASP.NET e 2013.1 strumenti Web per Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036427"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="2d907-103">Note sulla versione di ASP.NET e strumenti Web 2013.1 per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2d907-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="2d907-104">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2d907-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="2d907-105">Questo documento descrive la versione di ASP.NET e 2013.1 strumenti Web per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="2d907-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="2d907-106">Sommario</span><span class="sxs-lookup"><span data-stu-id="2d907-106">Contents</span></span>

- [<span data-ttu-id="2d907-107">Note sull'installazione</span><span class="sxs-lookup"><span data-stu-id="2d907-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="2d907-108">Requisiti software</span><span class="sxs-lookup"><span data-stu-id="2d907-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="2d907-109">Nuove funzionalità di ASP.NET e strumenti Web 2013.1 per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2d907-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="2d907-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="2d907-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="2d907-111">Modelli</span><span class="sxs-lookup"><span data-stu-id="2d907-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="2d907-112">Modello di MVC ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="2d907-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="2d907-113">Modello ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="2d907-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="2d907-114">Modelli di elemento</span><span class="sxs-lookup"><span data-stu-id="2d907-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="2d907-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2d907-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="2d907-116">Scaffolding di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2d907-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="2d907-117">Editor Razor</span><span class="sxs-lookup"><span data-stu-id="2d907-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="2d907-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="2d907-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="2d907-119">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="2d907-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="2d907-120">Scaffolding di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2d907-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="2d907-121">MVC e Web API Scaffolding - HTTP 404, errore non trovato</span><span class="sxs-lookup"><span data-stu-id="2d907-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="2d907-122">Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di un elemento di scaffolding</span><span class="sxs-lookup"><span data-stu-id="2d907-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="2d907-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="2d907-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="2d907-124">Visualizzazione file cshtml con Esplora con o F5 provoca un errore del server</span><span class="sxs-lookup"><span data-stu-id="2d907-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="2d907-125">Tilde(~) e riscrittura Url</span><span class="sxs-lookup"><span data-stu-id="2d907-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="2d907-126">Modelli</span><span class="sxs-lookup"><span data-stu-id="2d907-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="2d907-127">Note sull'installazione</span><span class="sxs-lookup"><span data-stu-id="2d907-127">Installation Notes</span></span>

<span data-ttu-id="2d907-128">[Installare](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) Web ASP.NET e strumenti 2013.1 per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="2d907-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="2d907-129">Requisiti software</span><span class="sxs-lookup"><span data-stu-id="2d907-129">Software Requirements</span></span>

<span data-ttu-id="2d907-130">È necessario disporre di Visual Studio 2012 o Visual Studio Express 2012 per Web.</span><span class="sxs-lookup"><span data-stu-id="2d907-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="2d907-131">Nuove funzionalità di ASP.NET e strumenti Web 2013.1 per Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2d907-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="2d907-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="2d907-132">Bootstrap</span></span>

<span data-ttu-id="2d907-133">Durante lo scaffolding controller MVC 5 e visualizzazioni, viene utilizzato il markup per le viste [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="2d907-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="2d907-134">Modelli</span><span class="sxs-lookup"><span data-stu-id="2d907-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="2d907-135">Modello di MVC ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="2d907-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="2d907-136">È stato aggiunto un nuovo modello MVC 5.</span><span class="sxs-lookup"><span data-stu-id="2d907-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="2d907-137">Fa riferimento a pacchetti MVC 5 NuGet più recenti e lo scaffolding consente di aggiungere controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="2d907-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="2d907-138">Modello ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="2d907-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="2d907-139">È stato aggiunto un nuovo modello di API Web 2.</span><span class="sxs-lookup"><span data-stu-id="2d907-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="2d907-140">Fa riferimento a pacchetti Web API 2 NuGet più recenti e lo scaffolding consente di aggiungere controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="2d907-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="2d907-141">Modelli di elemento</span><span class="sxs-lookup"><span data-stu-id="2d907-141">Item Templates</span></span>

<span data-ttu-id="2d907-142">Nuovi modelli di elemento di visualizzazioni MVC 5, pagine Web (Razor 3) e controller Web API 2 è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="2d907-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="2d907-143">L'installazione dei pacchetti NuGet correlati al progetto durante l'aggiunta di nuovi elementi.</span><span class="sxs-lookup"><span data-stu-id="2d907-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="2d907-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2d907-144">Entity Framework 6</span></span>

<span data-ttu-id="2d907-145">Durante lo scaffolding un controller MVC o Web API Usa Entity Framework, utilizziamo Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2d907-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="2d907-146">Per ulteriori informazioni su Entity Framework, vedere il [cronologia delle versioni di Entity Framework](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="2d907-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="2d907-147">È anche possibile scaricare e installare gli strumenti di Entity Framework 6 per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="2d907-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="2d907-148">Vedere il [ottenere Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="2d907-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="2d907-149">Scaffolding di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2d907-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="2d907-150">Lo Scaffolding di ASP.NET è un framework di generazione di codice per applicazioni Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2d907-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="2d907-151">Che consente di aggiungere il codice di boilerplate al progetto che interagisce con un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="2d907-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="2d907-152">Nelle versioni precedenti di Visual Studio, lo scaffolding è limitato ai progetti ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2d907-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="2d907-153">Con questo aggiornamento, è ora possibile usare lo scaffolding per qualsiasi progetto ASP.NET, inclusi Web Form.</span><span class="sxs-lookup"><span data-stu-id="2d907-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="2d907-154">Questo aggiornamento non supporta la generazione di pagine per un progetto di Web Form, ma è comunque possibile utilizzare lo scaffolding con Web Form aggiungendo le dipendenze MVC al progetto.</span><span class="sxs-lookup"><span data-stu-id="2d907-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="2d907-155">Verrà aggiunto il supporto per la generazione di pagine Web Form in un aggiornamento futuro.</span><span class="sxs-lookup"><span data-stu-id="2d907-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="2d907-156">Quando si usa lo scaffolding, è assicurare che tutti obbligatori le dipendenze siano installate nel progetto.</span><span class="sxs-lookup"><span data-stu-id="2d907-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="2d907-157">Ad esempio, se si avvia con un progetto di Web Form ASP.NET e quindi Usa lo scaffolding per aggiungere un Controller API Web, i pacchetti NuGet necessari e i riferimenti vengono aggiunti al progetto automaticamente.</span><span class="sxs-lookup"><span data-stu-id="2d907-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="2d907-158">Per aggiungere lo scaffolding di MVC a un progetto di Web Form, aggiungere un **nuovo elemento di scaffolding** e selezionare **MVC 5 dipendenze** nella finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="2d907-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="2d907-159">Sono disponibili due opzioni per lo scaffolding MVC; Minimo e completo.</span><span class="sxs-lookup"><span data-stu-id="2d907-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="2d907-160">Se si seleziona minima, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="2d907-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="2d907-161">Se si seleziona l'opzione completo, vengono aggiunte le dipendenze minime, nonché i file di contenuto richiesti per un progetto MVC.</span><span class="sxs-lookup"><span data-stu-id="2d907-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="2d907-162">Supporto per lo scaffolding controller asincrono utilizza le nuove funzionalità asincrone di Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2d907-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="2d907-163">Per ulteriori informazioni ed esercitazioni, vedere [Panoramica lo Scaffolding di ASP.NET](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2d907-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="2d907-164">Queste esercitazioni viene illustrato lo scaffolding con Visual Studio 2013, ma sono inoltre applicabili ad ASP.NET e 2013.1 strumenti Web per Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="2d907-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="2d907-165">Editor Razor</span><span class="sxs-lookup"><span data-stu-id="2d907-165">Razor Editor</span></span>

<span data-ttu-id="2d907-166">Con questo aggiornamento, Visual Studio 2012 supporta ora gli strumenti o modifica 3 Razor.</span><span class="sxs-lookup"><span data-stu-id="2d907-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="2d907-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="2d907-167">NuGet 2.7</span></span>

<span data-ttu-id="2d907-168">2.7 NuGet include un set completo delle nuove funzionalità descritte in dettaglio [note sulla versione 2.7 di NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="2d907-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="2d907-169">Questa versione di NuGet Elimina la necessità per gli utenti consentire in modo esplicito NuGet per ripristinare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="2d907-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="2d907-170">Il consenso per ripristinare automaticamente i pacchetti mancanti durante l'installazione di NuGet 2.7, gli utenti in modo implicito.</span><span class="sxs-lookup"><span data-stu-id="2d907-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="2d907-171">Gli utenti possono escludere in modo esplicito di ripristino del pacchetto tramite le impostazioni di NuGet in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d907-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="2d907-172">Questa modifica semplifica il funzionamento di ripristino del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="2d907-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="2d907-173">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="2d907-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="2d907-174">Scaffolding di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2d907-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="2d907-175">MVC e Web API Scaffolding - HTTP 404, errore non trovato</span><span class="sxs-lookup"><span data-stu-id="2d907-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="2d907-176">Se si verifica un errore durante l'aggiunta di un elemento di scaffolding per un progetto, è possibile che il progetto verrà lasciato in uno stato incoerente.</span><span class="sxs-lookup"><span data-stu-id="2d907-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="2d907-177">Alcune delle modifiche apportate lo scaffolding di essere eseguito il rollback, ma altre modifiche, ad esempio i pacchetti NuGet installati, non sarà possibile eseguire il rollback.</span><span class="sxs-lookup"><span data-stu-id="2d907-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="2d907-178">Se le modifiche alla configurazione di routing rollback, gli utenti riceveranno un errore HTTP 404 quando passando a elementi di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="2d907-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="2d907-179">Per correggere l'errore per MVC, aggiungere un nuovo elemento di scaffolding e selezionare le dipendenze di MVC 5 (minima o completa).</span><span class="sxs-lookup"><span data-stu-id="2d907-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="2d907-180">Questo processo verrà aggiungere tutte le modifiche necessarie al progetto.</span><span class="sxs-lookup"><span data-stu-id="2d907-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="2d907-181">Per correggere l'errore per l'API Web:</span><span class="sxs-lookup"><span data-stu-id="2d907-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="2d907-182">Aggiungere alla classe WebApiConfig seguente al progetto.</span><span class="sxs-lookup"><span data-stu-id="2d907-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="2d907-183">Configurare WebApiConfig.Register nell'applicazione\_Start (metodo) in Global. asax, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="2d907-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="2d907-184">Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di un elemento di scaffolding</span><span class="sxs-lookup"><span data-stu-id="2d907-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="2d907-185">Se Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta dell'elemento di scaffolding con Entity Framework (ad esempio Web 2 Controller API con azioni, mediante Entity Framework), è possibile che Visual Studio Express non è stato possibile caricare l'immagine nativa di un assembly dipende Extensions.</span><span class="sxs-lookup"><span data-stu-id="2d907-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="2d907-186">Per risolvere il problema, configurare Visual Studio Express per funzionare con l'immagine MSIL di Extensions:</span><span class="sxs-lookup"><span data-stu-id="2d907-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="2d907-187">Aprire il prompt dei comandi in modalità amministratore.</span><span class="sxs-lookup"><span data-stu-id="2d907-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="2d907-188">Passare a %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE o % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (per Windows a 64 bit).</span><span class="sxs-lookup"><span data-stu-id="2d907-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="2d907-189">Aprire VWDExpress.exe.config in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="2d907-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="2d907-190">Aggiungere la riga seguente sotto il &lt;configurazione&gt;/&lt;runtime&gt; elemento:</span><span class="sxs-lookup"><span data-stu-id="2d907-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="2d907-191">Riavviare Visual Studio Express 2012 per Web.</span><span class="sxs-lookup"><span data-stu-id="2d907-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="2d907-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="2d907-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="2d907-193">Visualizzazione di un errore del server withBrowse file cshtml WithorF5causes</span><span class="sxs-lookup"><span data-stu-id="2d907-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="2d907-194">Quando si crea un progetto MVC 5 in Visual Studio 2012 (o aprire il progetto di Visual Studio 2012 un MVC 5 che è stato creato in Visual Studio 2013) e si tenta di visualizzare un file cshtml utilizzando Sfoglia con o F5, si riceverà un errore che indica - **errore Server nel Applicazione '/'**.</span><span class="sxs-lookup"><span data-stu-id="2d907-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="2d907-195">Il server tenta di passare a `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="2d907-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="2d907-196">Per risolvere questo problema, modificare il **azione di avvio** impostazione nel progetto per **pagina specifica**.</span><span class="sxs-lookup"><span data-stu-id="2d907-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="2d907-197">Non è necessario fornire un valore per la pagina.</span><span class="sxs-lookup"><span data-stu-id="2d907-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="2d907-198">Dopo aver apportato questa modifica, la selezione di F5 consente di passare alla radice dell'applicazione (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="2d907-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="2d907-199">Questo comportamento non è lo stesso come il comportamento per i progetti MVC 5 in Visual Studio 2013, in cui il **pagina corrente** impostazione consente di avviare la pagina aperta.</span><span class="sxs-lookup"><span data-stu-id="2d907-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="2d907-200">Tilde(~) e riscrittura Url</span><span class="sxs-lookup"><span data-stu-id="2d907-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="2d907-201">Dopo l'aggiornamento a 3 Razor di ASP.NET o ASP.NET MVC 5, la notazione tilde(~) potrebbe non funzionare correttamente se si utilizza la riscrittura URL.</span><span class="sxs-lookup"><span data-stu-id="2d907-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="2d907-202">URL rewrite interessa la notazione tilde(~) negli elementi HTML quali &lt;A /&gt;, &lt;SCRIPT /&gt;, &lt;collegamento /&gt;, e di conseguenza la tilde non viene eseguito il mapping alla directory radice.</span><span class="sxs-lookup"><span data-stu-id="2d907-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="2d907-203">Ad esempio, se si riscrivono le richieste per **asp.net/content** a **asp.net**, l'attributo href nel &lt;href = "~/content/" /&gt; si risolve in **/content/ contenuto /** anziché **/**.</span><span class="sxs-lookup"><span data-stu-id="2d907-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="2d907-204">Per eliminare questa modifica, è possibile impostare il **IIS\_WasUrlRewritten** contesto su false in ogni pagina Web o in **applicazione\_BeginRequest** in Global. asax.</span><span class="sxs-lookup"><span data-stu-id="2d907-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="2d907-205">Modelli</span><span class="sxs-lookup"><span data-stu-id="2d907-205">Templates</span></span>

<span data-ttu-id="2d907-206">Quando ASP.NET MVC progetti creati con Visual Studio 2012, Windows 8.1 o Windows Server 2012 R2, Visual Studio viene visualizzato un messaggio di errore che indica "Configurazione Web [url] per ASP.NET 4.5 non è riuscita."</span><span class="sxs-lookup"><span data-stu-id="2d907-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Errore di configurazione](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="2d907-208">Questo errore viene visualizzato perché Visual Studio 2012 non abilita la funzionalità ASP.NET 4.5 quando viene installato in tali versioni di Windows.</span><span class="sxs-lookup"><span data-stu-id="2d907-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="2d907-209">Per abilitare ASP.NET 4.5, eseguire i passaggi descritti in [o disattivazione delle funzionalità Windows attivare](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="2d907-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![attivare o disattivare le funzionalità di Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="2d907-211">In alternativa, è possibile abilitare ASP.NET 4.5 tramite la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="2d907-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="2d907-212">Aprire il prompt dei comandi in modalità amministratore.</span><span class="sxs-lookup"><span data-stu-id="2d907-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="2d907-213">Eseguire il comando seguente per abilitare ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="2d907-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
