---
uid: ajax/cdn/overview
title: Microsoft Ajax Content Delivery Network | Documenti Microsoft
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: f1225f06e5218d893e3f49b2ccc67d56365b30e5
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="cc550-102">Microsoft Ajax Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="cc550-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="cc550-103">Nota: La rete CDN di Microsoft Ajax non dispone di alcun contratto di servizio dopo l'utilizzo di una rete CDN di Azure.</span><span class="sxs-lookup"><span data-stu-id="cc550-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="cc550-104">Sommario</span><span class="sxs-lookup"><span data-stu-id="cc550-104">Table of Contents</span></span>

<span data-ttu-id="cc550-105">**[AJAX.microsoft.com rinominato in ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="cc550-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="cc550-106">**[Supporto di Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="cc550-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="cc550-107">**[Utilizzando ASP.NET Ajax dalla rete CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="cc550-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="cc550-108">**[Tramite jQuery dalla rete CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="cc550-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="cc550-109">**[Utilizzo di jQuery UI dalla rete CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="cc550-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="cc550-110">**[File di terze parti nella rete CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="cc550-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="cc550-111">Versioni di jQuery nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="cc550-112">Versioni di eseguire la migrazione di jQuery nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="cc550-113">jQuery versioni dell'interfaccia utente nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="cc550-114">Versioni di convalida nella rete CDN jQuery</span><span class="sxs-lookup"><span data-stu-id="cc550-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="cc550-115">jQuery Mobile versioni nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="cc550-116">jQuery rilasci di modelli di rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="cc550-117">jQuery versioni ciclo nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="cc550-118">jQuery versioni DataTable nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="cc550-119">Versioni di Modernizr nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="cc550-120">Versioni di JSHint nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="cc550-121">Versioni di Knockout nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="cc550-122">Globalizzare le versioni su una rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="cc550-123">Rispondere versioni su una rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="cc550-124">Versioni di bootstrap nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="cc550-125">Versioni di bootstrap TouchCarousel nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="cc550-126">Versioni di Hammer.js nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="cc550-127">Web Form ASP.NET e Ajax versioni nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="cc550-128">ASP.NET MVC rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="cc550-129">ASP.NET SignalR rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="cc550-130">Il Microsoft Ajax rete CDN (Content Delivery) ospita le librerie JavaScript più diffusi di terze parti, ad esempio jQuery e consente di aggiungere facilmente alle applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="cc550-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="cc550-131">Ad esempio, iniziare a utilizzare jQuery che è ospitato in questa rete CDN semplicemente aggiungendo un &lt;script&gt; tag a una pagina che punta a ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="cc550-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="cc550-132">È possibile sfruttare la rete CDN, è possibile migliorare notevolmente le prestazioni delle applicazioni Ajax.</span><span class="sxs-lookup"><span data-stu-id="cc550-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="cc550-133">Il contenuto della rete CDN viene memorizzati nella cache nei server presenti in tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="cc550-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="cc550-134">Inoltre, la rete CDN consente ai browser di riutilizzare i file JavaScript memorizzati nella cache di terze parti per siti web che si trovano in domini diversi.</span><span class="sxs-lookup"><span data-stu-id="cc550-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="cc550-135">La rete CDN supporta SSL (HTTPS), nel caso in cui sia necessario presentare una pagina web utilizzando Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="cc550-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="cc550-136">La rete CDN ospita le seguenti librerie di script di terze parti che sono state caricate e sono concessi in licenza, ai proprietari di tali raccolte:</span><span class="sxs-lookup"><span data-stu-id="cc550-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="cc550-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="cc550-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="cc550-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="cc550-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="cc550-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="cc550-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="cc550-140">jQuery convalida (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="cc550-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="cc550-141">jQuery ciclo (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="cc550-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="cc550-142">DataTable (jQueryhttp://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="cc550-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="cc550-143">Rete CDN di Microsoft Ajax include anche le librerie seguenti che sono state caricate da Microsoft:</span><span class="sxs-lookup"><span data-stu-id="cc550-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="cc550-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="cc550-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="cc550-145">File JavaScript di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="cc550-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="cc550-146">File di ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="cc550-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="cc550-147">Microsoft non rivendica la proprietà di tutte le librerie di terze parti ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="cc550-148">I proprietari delle librerie di copyright licenza queste librerie all'utente.</span><span class="sxs-lookup"><span data-stu-id="cc550-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="cc550-149">Eventuali diritti che è possibile scaricare e usare tali librerie vengono concesse unicamente dai rispettivi proprietari copyright.</span><span class="sxs-lookup"><span data-stu-id="cc550-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="cc550-150">Poiché questi non sono librerie di Microsoft, Microsoft non offre alcuna garanzia o licenze di diritti di proprietà intellettuale (non incluso alcun diritto di brevetto implicito) per le librerie di terze parti ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="cc550-151">Se si desidera inviare la libreria JavaScript e la libreria è una delle librerie JavaScript superiore (come indicato in http://trends.builtwith.com) o estensioni/plug-in da queste librerie che sono (a) più diffusi; o (b) utili per l'utilizzo in ASP.NET, quindi contattare AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="cc550-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="cc550-152">AJAX.microsoft.com rinominato in ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="cc550-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="cc550-153">La rete CDN consentono di utilizzare il nome di dominio microsoft.com ed è stata modificata per utilizzare il nome di dominio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="cc550-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="cc550-154">Questa modifica è stata apportata per migliorare le prestazioni, perché quando un browser a cui fa riferimento il dominio microsoft.com invierebbe i cookie da quel dominio in rete con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc550-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="cc550-155">È stato rinominato in un nome di dominio diverso da microsoft.com prestazioni possono essere incrementata quanto a 25%.</span><span class="sxs-lookup"><span data-stu-id="cc550-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="cc550-156">Si noti ajax.microsoft.com continuerà a funzionare, ma è consigliabile ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="cc550-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="cc550-157">Formato precedente: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="cc550-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="cc550-158">Nuovo formato: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="cc550-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="cc550-159">Supporto di Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="cc550-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="cc550-160">Per utilizzare correttamente i file .vsdoc con Visual Studio 2008, è necessario assicurarsi di disporre di Visual Studio 2008 SP1 installato e l'installazione dell'hotfix per i file vsdoc.</span><span class="sxs-lookup"><span data-stu-id="cc550-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="cc550-161">È possibile ottenere questi da qui:</span><span class="sxs-lookup"><span data-stu-id="cc550-161">You can get these from here:</span></span>

- [<span data-ttu-id="cc550-162">Scaricare Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="cc550-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "scaricare Visual Studio 2008 SP1")
- [<span data-ttu-id="cc550-163">Scaricare l'hotfix .vsdoc per Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="cc550-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "scaricare .vsdoc hotfix per Visual Studio 2008 SP1")

<span data-ttu-id="cc550-164">Visual Studio 2010 supporta file .vsdoc senza tutte le patch aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="cc550-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="cc550-165">Utilizzando ASP.NET Ajax dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="cc550-166">Quando si utilizza ASP.NET 4, è possibile reindirizzare tutte le richieste per gli script del framework ASP.NET per la rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="cc550-167">Il recupero degli script dalla rete CDN anziché il server web locale può sostanzialmente di migliorare le prestazioni dei siti Web ASP.NET pubblico.</span><span class="sxs-lookup"><span data-stu-id="cc550-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="cc550-168">Utilizzare la proprietà ScriptManager EnableCDN per reindirizzare tutte le richieste di script framework ASP.NET Ajax CDN Microsoft:</span><span class="sxs-lookup"><span data-stu-id="cc550-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="cc550-169">Tramite jQuery dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="cc550-170">È possibile utilizzare script jQuery ospitato nella rete CDN nell'applicazione Web aggiungendo il seguente elemento script a una pagina:</span><span class="sxs-lookup"><span data-stu-id="cc550-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="cc550-171">La rete CDN include anche la versione minimizzata dello script jQuery, che è possibile ottenere utilizzando l'elemento seguente:</span><span class="sxs-lookup"><span data-stu-id="cc550-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="cc550-172">Per consentire alla pagina di fallback per il caricamento di jQuery da un percorso locale nel proprio sito Web, se la rete CDN non disponibile, aggiungere l'elemento seguente immediatamente dopo l'elemento che fa riferimento la rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="cc550-173">La pagina di esempio seguente usa la versione della rete CDN della libreria jQuery (con fallback a una copia locale) per visualizzare il contenuto di un elemento div quando viene premuto un pulsante.</span><span class="sxs-lookup"><span data-stu-id="cc550-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="cc550-174">È possibile approfondire la conoscenza di jQuery e scaricare una copia locale di jQuery visitando il [jQuery](http://jquery.com/) sito Web.</span><span class="sxs-lookup"><span data-stu-id="cc550-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="cc550-175">Utilizzo di jQuery UI dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="cc550-176">La rete CDN ospita anche la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="cc550-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="cc550-177">La libreria dell'interfaccia utente jQuery include una vasta gamma di widget e gli effetti che è possibile utilizzare nelle applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cc550-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="cc550-178">Ad esempio, la pagina seguente viene illustrato come è possibile utilizzare jQuery UI Datepicker nel contesto di un'applicazione Web Form ASP.NET per visualizzare un calendario popup:</span><span class="sxs-lookup"><span data-stu-id="cc550-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="cc550-179">Quando si sposta lo stato attivo al controllo TextBox con la tastiera, viene visualizzato un calendario:</span><span class="sxs-lookup"><span data-stu-id="cc550-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendario popup creato con Datepicker](overview/_static/image1.png)

<span data-ttu-id="cc550-181">Si noti che è necessario includere tre file dalla rete CDN nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="cc550-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="cc550-182">La libreria jQuery &mdash; libreria jQuery UI dipende la libreria jQuery.</span><span class="sxs-lookup"><span data-stu-id="cc550-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="cc550-183">È necessario aggiungere la libreria jQuery alla pagina prima di aggiungere la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="cc550-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="cc550-184">La libreria dell'interfaccia utente jQuery &mdash; libreria jQuery UI contiene tutti gli effetti dell'interfaccia utente jQuery e widget, ad esempio il widget Datepicker utilizzate nella pagina precedente.</span><span class="sxs-lookup"><span data-stu-id="cc550-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="cc550-185">Un tema dell'interfaccia utente jQuery &mdash; jQuery UI supporta diversi temi.</span><span class="sxs-lookup"><span data-stu-id="cc550-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="cc550-186">La pagina precedente include un collegamento a un file CSS per importare il tema di Redmond.</span><span class="sxs-lookup"><span data-stu-id="cc550-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="cc550-187">Tutti i temi di jQuery standard dell'interfaccia utente sono ospitati nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="cc550-188">[Visitare questa pagina](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sulla rete CDN di Microsoft Ajax") per visualizzare le anteprime per ognuno di essi.</span><span class="sxs-lookup"><span data-stu-id="cc550-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="cc550-189">Per ulteriori informazioni sulla libreria jQuery UI, visitare ufficiali [sito Web di jQuery UI](http://jQueryUI.com "sito Web di jQuery UI").</span><span class="sxs-lookup"><span data-stu-id="cc550-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="cc550-190">File di terze parti nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="cc550-191">La rete CDN ospita alcune delle librerie di JavaScript più diffusi di terze parti.</span><span class="sxs-lookup"><span data-stu-id="cc550-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="cc550-192">Microsoft non rivendica la proprietà di tutte le librerie di terze parti ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="cc550-193">I proprietari delle librerie di copyright licenza queste librerie all'utente.</span><span class="sxs-lookup"><span data-stu-id="cc550-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="cc550-194">Eventuali diritti che è possibile scaricare e usare tali librerie vengono concesse unicamente dai rispettivi proprietari copyright.</span><span class="sxs-lookup"><span data-stu-id="cc550-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="cc550-195">Poiché questi non sono librerie di Microsoft, Microsoft non offre alcuna garanzia o licenze di diritti di proprietà intellettuale (non incluso alcun diritto di brevetto implicito) per le librerie di terze parti ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="cc550-196">Versioni di jQuery nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="cc550-197">Le versioni seguenti di jQuery ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="cc550-198">versione di jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="cc550-198">jQuery version 3.3.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="cc550-199">versione di jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="cc550-199">jQuery version 3.2.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="cc550-200">versione di jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="cc550-200">jQuery version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="cc550-201">versione di jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="cc550-201">jQuery version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="cc550-202">jQuery versione 3.1.0</span><span class="sxs-lookup"><span data-stu-id="cc550-202">jQuery version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="cc550-203">versione di jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="cc550-203">jQuery version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="cc550-204">versione di jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="cc550-204">jQuery version 2.2.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="cc550-205">versione di jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="cc550-205">jQuery version 2.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="cc550-206">versione di jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="cc550-206">jQuery version 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="cc550-207">jQuery versione 2.2.1</span><span class="sxs-lookup"><span data-stu-id="cc550-207">jQuery version 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="cc550-208">versione di jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="cc550-208">jQuery version 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="cc550-209">versione di jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="cc550-209">jQuery version 2.1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="cc550-210">versione di jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="cc550-210">jQuery version 2.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="cc550-211">versione di jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="cc550-211">jQuery version 2.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="cc550-212">versione di jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="cc550-212">jQuery version 2.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="cc550-213">versione di jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="cc550-213">jQuery version 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="cc550-214">versione di jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="cc550-214">jQuery version 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="cc550-215">versione di jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="cc550-215">jQuery version 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="cc550-216">versione di jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="cc550-216">jQuery version 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="cc550-217">versione di jQuery 2.0.0</span><span class="sxs-lookup"><span data-stu-id="cc550-217">jQuery version 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="cc550-218">versione di jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="cc550-218">jQuery version 1.12.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="cc550-219">versione di jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="cc550-219">jQuery version 1.12.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="cc550-220">versione di jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="cc550-220">jQuery version 1.12.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="cc550-221">versione di jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="cc550-221">jQuery version 1.12.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="cc550-222">versione di jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="cc550-222">jQuery version 1.12.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="cc550-223">versione di jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="cc550-223">jQuery version 1.11.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="cc550-224">versione di jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="cc550-224">jQuery version 1.11.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="cc550-225">versione di jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="cc550-225">jQuery version 1.11.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="cc550-226">versione di jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="cc550-226">jQuery version 1.11.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="cc550-227">versione di jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="cc550-227">jQuery version 1.10.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="cc550-228">versione di jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="cc550-228">jQuery version 1.10.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="cc550-229">versione di jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="cc550-229">jQuery version 1.10.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="cc550-230">versione di jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="cc550-230">jQuery version 1.9.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="cc550-231">versione di jQuery alla 1.9.0</span><span class="sxs-lookup"><span data-stu-id="cc550-231">jQuery version 1.9.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="cc550-232">versione di jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="cc550-232">jQuery version 1.8.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="cc550-233">versione di jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="cc550-233">jQuery version 1.8.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="cc550-234">versione di jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="cc550-234">jQuery version 1.8.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="cc550-235">versione di jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="cc550-235">jQuery version 1.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="cc550-236">versione di jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="cc550-236">jQuery version 1.7.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="cc550-237">versione di jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="cc550-237">jQuery version 1.7.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="cc550-238">jQuery versione 1.7</span><span class="sxs-lookup"><span data-stu-id="cc550-238">jQuery version 1.7</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="cc550-239">versione di jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="cc550-239">jQuery version 1.6.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="cc550-240">versione di jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="cc550-240">jQuery version 1.6.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="cc550-241">versione di jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="cc550-241">jQuery version 1.6.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="cc550-242">versione di jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="cc550-242">jQuery version 1.6.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="cc550-243">jQuery versione 1.6</span><span class="sxs-lookup"><span data-stu-id="cc550-243">jQuery version 1.6</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="cc550-244">versione di jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="cc550-244">jQuery version 1.5.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="cc550-245">versione di jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="cc550-245">jQuery version 1.5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="cc550-246">jQuery versione 1.5</span><span class="sxs-lookup"><span data-stu-id="cc550-246">jQuery version 1.5</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="cc550-247">versione di jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="cc550-247">jQuery version 1.4.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="cc550-248">versione di jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="cc550-248">jQuery version 1.4.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="cc550-249">versione di jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="cc550-249">jQuery version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="cc550-250">versione di jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="cc550-250">jQuery version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="cc550-251">jQuery versione 1.4</span><span class="sxs-lookup"><span data-stu-id="cc550-251">jQuery version 1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="cc550-252">versione di jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="cc550-252">jQuery version 1.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="cc550-253">Versioni di eseguire la migrazione di jQuery nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-253">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="cc550-254">Le versioni seguenti di jQuery migrazione sono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-254">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="cc550-255">eseguire la migrazione della versione 3.0.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="cc550-255">jQuery Migrate version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="cc550-256">Migrazione versione 1.2.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="cc550-256">jQuery Migrate version 1.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="cc550-257">jQuery migrazione versione 1.2.0</span><span class="sxs-lookup"><span data-stu-id="cc550-257">jQuery Migrate version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="cc550-258">Migrazione versione 1.1.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="cc550-258">jQuery Migrate version 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="cc550-259">Migrazione versione 1.1.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="cc550-259">jQuery Migrate version 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="cc550-260">jQuery versione 1.0.0 di migrazione</span><span class="sxs-lookup"><span data-stu-id="cc550-260">jQuery Migrate version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="cc550-261">jQuery versioni dell'interfaccia utente nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-261">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="cc550-262">Le versioni della libreria jQuery UI seguenti sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-262">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="cc550-263">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="cc550-263">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="cc550-264">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="cc550-264">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-265">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="cc550-265">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-266">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="cc550-266">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-267">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="cc550-267">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-268">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="cc550-268">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-269">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="cc550-269">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-270">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="cc550-270">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-271">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="cc550-271">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-272">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="cc550-272">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-273">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="cc550-273">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-274">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="cc550-274">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-275">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="cc550-275">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-276">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="cc550-276">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-277">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="cc550-277">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-278">jQuery UI alla 1.9.0</span><span class="sxs-lookup"><span data-stu-id="cc550-278">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI alla 1.9.0 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-279">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="cc550-279">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-280">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="cc550-280">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-281">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="cc550-281">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-282">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="cc550-282">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-283">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="cc550-283">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-284">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="cc550-284">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-285">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="cc550-285">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-286">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="cc550-286">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-287">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="cc550-287">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-288">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="cc550-288">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-289">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="cc550-289">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-290">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="cc550-290">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-291">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="cc550-291">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-292">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="cc550-292">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-293">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="cc550-293">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-294">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="cc550-294">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-295">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="cc550-295">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-296">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="cc550-296">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-297">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="cc550-297">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-298">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="cc550-298">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="cc550-299">Versioni di convalida nella rete CDN jQuery</span><span class="sxs-lookup"><span data-stu-id="cc550-299">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="cc550-300">Le versioni della libreria di convalida jQuery seguenti sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-300">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="cc550-301">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="cc550-301">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="cc550-302">jQuery convalida 1.17.0</span><span class="sxs-lookup"><span data-stu-id="cc550-302">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "1.17.0 convalida jQuery")
- [<span data-ttu-id="cc550-303">jQuery convalida 1.16.0</span><span class="sxs-lookup"><span data-stu-id="cc550-303">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "1.16.0 convalida jQuery")
- [<span data-ttu-id="cc550-304">jQuery convalida 1.15.1</span><span class="sxs-lookup"><span data-stu-id="cc550-304">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "1.15.1 convalida jQuery")
- [<span data-ttu-id="cc550-305">jQuery convalida 1.15.0</span><span class="sxs-lookup"><span data-stu-id="cc550-305">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "1.15.0 convalida jQuery")
- [<span data-ttu-id="cc550-306">jQuery convalida 1.14.0</span><span class="sxs-lookup"><span data-stu-id="cc550-306">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "1.14.0 convalida jQuery")
- [<span data-ttu-id="cc550-307">jQuery convalida 1.13.1</span><span class="sxs-lookup"><span data-stu-id="cc550-307">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "1.13.1 convalida jQuery")
- [<span data-ttu-id="cc550-308">jQuery convalida 1.13.0</span><span class="sxs-lookup"><span data-stu-id="cc550-308">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "1.13.0 convalida jQuery")
- [<span data-ttu-id="cc550-309">jQuery convalida 1.12.0</span><span class="sxs-lookup"><span data-stu-id="cc550-309">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "1.12.0 convalida jQuery")
- [<span data-ttu-id="cc550-310">jQuery convalida 1.11.1</span><span class="sxs-lookup"><span data-stu-id="cc550-310">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "1.11.1 convalida jQuery")
- [<span data-ttu-id="cc550-311">jQuery convalida 1.11.0</span><span class="sxs-lookup"><span data-stu-id="cc550-311">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "1.11.0 convalida jQuery")
- [<span data-ttu-id="cc550-312">jQuery convalida 1.10.0</span><span class="sxs-lookup"><span data-stu-id="cc550-312">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "1.10.0 convalida jQuery")
- [<span data-ttu-id="cc550-313">jQuery convalida 1.9</span><span class="sxs-lookup"><span data-stu-id="cc550-313">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versione 1.9")
- [<span data-ttu-id="cc550-314">jQuery convalida 1.8.1</span><span class="sxs-lookup"><span data-stu-id="cc550-314">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versione 1.8.1")
- [<span data-ttu-id="cc550-315">jQuery convalida 1.8</span><span class="sxs-lookup"><span data-stu-id="cc550-315">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versione 1.8")
- [<span data-ttu-id="cc550-316">jQuery convalida 1.7</span><span class="sxs-lookup"><span data-stu-id="cc550-316">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versione 1.7")
- [<span data-ttu-id="cc550-317">jQuery convalida 1.6</span><span class="sxs-lookup"><span data-stu-id="cc550-317">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery convalida 1.6")
- [<span data-ttu-id="cc550-318">jQuery convalida 1.5.5</span><span class="sxs-lookup"><span data-stu-id="cc550-318">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "1.5.5 convalida jQuery")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="cc550-319">jQuery Mobile versioni nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-319">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="cc550-320">Le seguenti versioni della libreria jQuery Mobile sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-320">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="cc550-321">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="cc550-321">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="cc550-322">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="cc550-322">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-323">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="cc550-323">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-324">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="cc550-324">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-325">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="cc550-325">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-326">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="cc550-326">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-327">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="cc550-327">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-328">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="cc550-328">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-329">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="cc550-329">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-330">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="cc550-330">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-331">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="cc550-331">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-332">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="cc550-332">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-333">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="cc550-333">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-334">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="cc550-334">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-335">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="cc550-335">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-336">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="cc550-336">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-337">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="cc550-337">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="cc550-338">la versione beta di jQuery Mobile 1.0 3</span><span class="sxs-lookup"><span data-stu-id="cc550-338">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 nella rete CDN di Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="cc550-339">jQuery rilasci di modelli di rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-339">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="cc550-340">Le seguenti versioni di plug-in modelli jQuery sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-340">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="cc550-341">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="cc550-341">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="cc550-342">jQuery modelli Beta 1</span><span class="sxs-lookup"><span data-stu-id="cc550-342">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery modelli Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="cc550-343">jQuery versioni ciclo nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-343">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="cc550-344">Le versioni seguenti di plug-in del ciclo di jQuery sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-344">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="cc550-345">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="cc550-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="cc550-346">jQuery 2,99 ciclo</span><span class="sxs-lookup"><span data-stu-id="cc550-346">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 ciclo")
- [<span data-ttu-id="cc550-347">jQuery ciclo 2.94</span><span class="sxs-lookup"><span data-stu-id="cc550-347">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 ciclo")
- [<span data-ttu-id="cc550-348">jQuery 2,88 ciclo</span><span class="sxs-lookup"><span data-stu-id="cc550-348">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 ciclo")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="cc550-349">jQuery versioni DataTable nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-349">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="cc550-350">Le seguenti versioni di jQuery DataTable plug-in sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-350">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="cc550-351">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="cc550-351">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="cc550-352">jQuery DataTable 1.10.5</span><span class="sxs-lookup"><span data-stu-id="cc550-352">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTable 1.10.5")
- [<span data-ttu-id="cc550-353">jQuery DataTable 1.10.4</span><span class="sxs-lookup"><span data-stu-id="cc550-353">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTable 1.10.4")
- [<span data-ttu-id="cc550-354">jQuery DataTable 1.9.4</span><span class="sxs-lookup"><span data-stu-id="cc550-354">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTable 1.9.4")
- [<span data-ttu-id="cc550-355">jQuery DataTable 1.9.3</span><span class="sxs-lookup"><span data-stu-id="cc550-355">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTable 1.9.3")
- [<span data-ttu-id="cc550-356">jQuery DataTable 1.9.2</span><span class="sxs-lookup"><span data-stu-id="cc550-356">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTable 1.9.2")
- [<span data-ttu-id="cc550-357">jQuery DataTable 1.9.1</span><span class="sxs-lookup"><span data-stu-id="cc550-357">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTable 1.9.1")
- [<span data-ttu-id="cc550-358">jQuery DataTable alla 1.9.0</span><span class="sxs-lookup"><span data-stu-id="cc550-358">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTable alla 1.9.0")
- [<span data-ttu-id="cc550-359">jQuery DataTable 1.8.2</span><span class="sxs-lookup"><span data-stu-id="cc550-359">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTable 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="cc550-360">Versioni di Modernizr nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-360">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="cc550-361">Le seguenti versioni di [Modernizr](http://www.modernizr.com "Modernizr") ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-361">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="cc550-362">Versioni di JSHint nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-362">JSHint Releases on the CDN</span></span>

<span data-ttu-id="cc550-363">Le seguenti versioni di [JSHint](http://www.jshint.com "JSHint") ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-363">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="cc550-364">Versioni di Knockout nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-364">Knockout Releases on the CDN</span></span>

<span data-ttu-id="cc550-365">Le seguenti versioni di [Knockout](http://www.knockoutjs.com "Knockout") ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-365">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="cc550-366">Globalizzare le versioni su una rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-366">Globalize Releases on the CDN</span></span>

<span data-ttu-id="cc550-367">Le seguenti versioni di [Globalize](https://github.com/jquery/globalize "Globalize") ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-367">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="cc550-368">La versione 1.0.0 di globalizzazione</span><span class="sxs-lookup"><span data-stu-id="cc550-368">Globalize version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="cc550-369">Globalizzazione versione 0.1.1</span><span class="sxs-lookup"><span data-stu-id="cc550-369">Globalize version 0.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="cc550-370">tutte le impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="cc550-370">all cultures</span></span>
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="cc550-371">Sostituire "{codice delle impostazioni cultura}" con il codice della lingua desiderata, ad esempio, dei file in rete CDN di Microsoft globalize.culture.en GB.js== = = queste librerie sono state caricate da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cc550-371">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="cc550-372">Rispondere versioni su una rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-372">Respond Releases on the CDN</span></span>

<span data-ttu-id="cc550-373">Le seguenti versioni di [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") rispondono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-373">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="cc550-374">Rispondere versione 1.4.2</span><span class="sxs-lookup"><span data-stu-id="cc550-374">Respond version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="cc550-375">Rispondere versione 1.4.1</span><span class="sxs-lookup"><span data-stu-id="cc550-375">Respond version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="cc550-376">Rispondere versione 1.4.0</span><span class="sxs-lookup"><span data-stu-id="cc550-376">Respond version 1.4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="cc550-377">Rispondere versione 1.3.0</span><span class="sxs-lookup"><span data-stu-id="cc550-377">Respond version 1.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="cc550-378">Rispondere versione 1.2.0</span><span class="sxs-lookup"><span data-stu-id="cc550-378">Respond version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="cc550-379">Versioni di bootstrap nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-379">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="cc550-380">Le seguenti versioni di [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-380">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-400"></a><span data-ttu-id="cc550-381">Versione bootstrap 4.0.0</span><span class="sxs-lookup"><span data-stu-id="cc550-381">Bootstrap version 4.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="cc550-382">Versione bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="cc550-382">Bootstrap version 3.3.7</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="cc550-383">Versione bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="cc550-383">Bootstrap version 3.3.6</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="cc550-384">Bootstrap versione 3.3.5</span><span class="sxs-lookup"><span data-stu-id="cc550-384">Bootstrap version 3.3.5</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="cc550-385">Versione bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="cc550-385">Bootstrap version 3.3.4</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="cc550-386">Versione bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="cc550-386">Bootstrap version 3.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="cc550-387">Versione bootstrap 3.3.1</span><span class="sxs-lookup"><span data-stu-id="cc550-387">Bootstrap version 3.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="cc550-388">Versione bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="cc550-388">Bootstrap version 3.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="cc550-389">Versione bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="cc550-389">Bootstrap version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="cc550-390">Versione bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="cc550-390">Bootstrap version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="cc550-391">Bootstrap versione 3.1.0</span><span class="sxs-lookup"><span data-stu-id="cc550-391">Bootstrap version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="cc550-392">Versione bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="cc550-392">Bootstrap version 3.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="cc550-393">Versione bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="cc550-393">Bootstrap version 3.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="cc550-394">Versione bootstrap 3.0.1</span><span class="sxs-lookup"><span data-stu-id="cc550-394">Bootstrap version 3.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="cc550-395">Versione bootstrap 3.0.0</span><span class="sxs-lookup"><span data-stu-id="cc550-395">Bootstrap version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="cc550-396">Bootstrap versione 2.3.2</span><span class="sxs-lookup"><span data-stu-id="cc550-396">Bootstrap version 2.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="cc550-397">Bootstrap versione 2.3.1</span><span class="sxs-lookup"><span data-stu-id="cc550-397">Bootstrap version 2.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="cc550-398">Versioni di bootstrap TouchCarousel nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-398">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="cc550-399">Le seguenti versioni di [ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") TouchCarousel Bootstrap versioni sono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-399">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="cc550-400">Bootstrap TouchCarousel versione 0.8.0</span><span class="sxs-lookup"><span data-stu-id="cc550-400">Bootstrap TouchCarousel version 0.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="cc550-401">Versioni di Hammer.js nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-401">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="cc550-402">Le seguenti versioni di [ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js versioni ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-402">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="cc550-403">Versione Hammer.js 2.0.4</span><span class="sxs-lookup"><span data-stu-id="cc550-403">Hammer.js version 2.0.4</span></span>

- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="cc550-404">Web Form ASP.NET e Ajax versioni nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-404">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="cc550-405">Le seguenti versioni di ASP.NET Ajax Library sono ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="cc550-405">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="cc550-406">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="cc550-406">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="cc550-407">Web Form ASP.NET e Ajax versione 4.5.2</span><span class="sxs-lookup"><span data-stu-id="cc550-407">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Form ASP.NET e Ajax 4.5.2")
- [<span data-ttu-id="cc550-408">Web Form ASP.NET e Ajax versione 4</span><span class="sxs-lookup"><span data-stu-id="cc550-408">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Form ASP.NET e Ajax 4")
- [<span data-ttu-id="cc550-409">ASP.NET Ajax versione 3.5</span><span class="sxs-lookup"><span data-stu-id="cc550-409">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="cc550-410">ASP.NET MVC rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-410">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="cc550-411">I seguenti file JavaScript di ASP.NET MVC sono ospitati in questa rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-411">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="cc550-412">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="cc550-412">ASP.NET MVC 5.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="cc550-413">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="cc550-413">ASP.NET MVC 5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="cc550-414">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="cc550-414">ASP.NET MVC 5.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="cc550-415">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="cc550-415">ASP.NET MVC 4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="cc550-416">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="cc550-416">ASP.NET MVC 3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="cc550-417">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="cc550-417">ASP.NET MVC 2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="cc550-418">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="cc550-418">ASP.NET MVC 1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="cc550-419">ASP.NET SignalR rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="cc550-419">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="cc550-420">I seguenti file di ASP.NET SignalR JavaScript sono ospitati in questa rete CDN:</span><span class="sxs-lookup"><span data-stu-id="cc550-420">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="cc550-421">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="cc550-421">ASP.NET SignalR 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="cc550-422">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="cc550-422">ASP.NET SignalR 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="cc550-423">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="cc550-423">ASP.NET SignalR 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="cc550-424">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="cc550-424">ASP.NET SignalR 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="cc550-425">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="cc550-425">ASP.NET SignalR 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="cc550-426">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="cc550-426">ASP.NET SignalR 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="cc550-427">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="cc550-427">ASP.NET SignalR 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="cc550-428">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="cc550-428">ASP.NET SignalR 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="cc550-429">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="cc550-429">ASP.NET SignalR 1.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="cc550-430">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="cc550-430">ASP.NET SignalR 1.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="cc550-431">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="cc550-431">ASP.NET SignalR 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="cc550-432">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="cc550-432">ASP.NET SignalR 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="cc550-433">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="cc550-433">ASP.NET SignalR 1.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="cc550-434">Per informazioni sulle condizioni di utilizzo per la rete CDN, vedere [Microsoft Ajax CDN condizioni di utilizzo](https://www.asp.net/terms-of-use "Microsoft Ajax CDN condizioni di utilizzo").</span><span class="sxs-lookup"><span data-stu-id="cc550-434">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
