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
ms.openlocfilehash: f69f707ba64d13fc372b7bc44718c9dcf8cec6e2
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="ffd96-102">Microsoft Ajax Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="ffd96-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="ffd96-103">Nota: La rete CDN di Microsoft Ajax non dispone di alcun contratto di servizio dopo l'utilizzo di una rete CDN di Azure.</span><span class="sxs-lookup"><span data-stu-id="ffd96-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="ffd96-104">Sommario</span><span class="sxs-lookup"><span data-stu-id="ffd96-104">Table of Contents</span></span>

<span data-ttu-id="ffd96-105">**[AJAX.microsoft.com rinominato in ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="ffd96-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="ffd96-106">**[Supporto di Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="ffd96-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="ffd96-107">**[Utilizzando ASP.NET Ajax dalla rete CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="ffd96-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="ffd96-108">**[Tramite jQuery dalla rete CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="ffd96-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="ffd96-109">**[Utilizzo di jQuery UI dalla rete CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="ffd96-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="ffd96-110">**[File di terze parti nella rete CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="ffd96-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="ffd96-111">Versioni di jQuery nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="ffd96-112">Versioni di eseguire la migrazione di jQuery nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="ffd96-113">jQuery versioni dell'interfaccia utente nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="ffd96-114">Versioni di convalida nella rete CDN jQuery</span><span class="sxs-lookup"><span data-stu-id="ffd96-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="ffd96-115">jQuery Mobile versioni nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="ffd96-116">jQuery rilasci di modelli di rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="ffd96-117">jQuery versioni ciclo nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="ffd96-118">jQuery versioni DataTable nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="ffd96-119">Versioni di Modernizr nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="ffd96-120">Versioni di JSHint nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="ffd96-121">Versioni di Knockout nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="ffd96-122">Globalizzare le versioni su una rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="ffd96-123">Rispondere versioni su una rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="ffd96-124">Versioni di bootstrap nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="ffd96-125">Versioni di bootstrap TouchCarousel nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="ffd96-126">Versioni di Hammer.js nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="ffd96-127">Web Form ASP.NET e Ajax versioni nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="ffd96-128">ASP.NET MVC rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="ffd96-129">ASP.NET SignalR rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="ffd96-130">Il Microsoft Ajax rete CDN (Content Delivery) ospita le librerie JavaScript più diffusi di terze parti, ad esempio jQuery e consente di aggiungere facilmente alle applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="ffd96-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="ffd96-131">Ad esempio, iniziare a utilizzare jQuery che è ospitato in questa rete CDN semplicemente aggiungendo un &lt;script&gt; tag a una pagina che punta a ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="ffd96-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="ffd96-132">È possibile sfruttare la rete CDN, è possibile migliorare notevolmente le prestazioni delle applicazioni Ajax.</span><span class="sxs-lookup"><span data-stu-id="ffd96-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="ffd96-133">Il contenuto della rete CDN viene memorizzati nella cache nei server presenti in tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="ffd96-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="ffd96-134">Inoltre, la rete CDN consente ai browser di riutilizzare i file JavaScript memorizzati nella cache di terze parti per siti web che si trovano in domini diversi.</span><span class="sxs-lookup"><span data-stu-id="ffd96-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="ffd96-135">La rete CDN supporta SSL (HTTPS), nel caso in cui sia necessario presentare una pagina web utilizzando Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="ffd96-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="ffd96-136">La rete CDN ospita le seguenti librerie di script di terze parti che sono state caricate e sono concessi in licenza, ai proprietari di tali raccolte:</span><span class="sxs-lookup"><span data-stu-id="ffd96-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="ffd96-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="ffd96-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="ffd96-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="ffd96-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="ffd96-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="ffd96-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="ffd96-140">jQuery convalida (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="ffd96-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="ffd96-141">jQuery ciclo (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="ffd96-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="ffd96-142">jQuery DataTable (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="ffd96-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="ffd96-143">Rete CDN di Microsoft Ajax include anche le librerie seguenti che sono state caricate da Microsoft:</span><span class="sxs-lookup"><span data-stu-id="ffd96-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="ffd96-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="ffd96-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="ffd96-145">File JavaScript di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ffd96-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="ffd96-146">File di ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="ffd96-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="ffd96-147">Microsoft non rivendica la proprietà di tutte le librerie di terze parti ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="ffd96-148">I proprietari delle librerie di copyright licenza queste librerie all'utente.</span><span class="sxs-lookup"><span data-stu-id="ffd96-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="ffd96-149">Eventuali diritti che è possibile scaricare e usare tali librerie vengono concesse unicamente dai rispettivi proprietari copyright.</span><span class="sxs-lookup"><span data-stu-id="ffd96-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="ffd96-150">Poiché questi non sono librerie di Microsoft, Microsoft non offre alcuna garanzia o licenze di diritti di proprietà intellettuale (non incluso alcun diritto di brevetto implicito) per le librerie di terze parti ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="ffd96-151">Se si desidera inviare la libreria JavaScript e la libreria è uno dei superiore estensioni/plug-in a queste librerie o librerie JavaScript (come indicato in http://trends.builtwith.com) (a) più diffusi; (b) o utili per utilizzare in ASP.NET, quindi contattare AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="ffd96-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="ffd96-152">AJAX.microsoft.com rinominato in ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="ffd96-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="ffd96-153">La rete CDN consentono di utilizzare il nome di dominio microsoft.com ed è stata modificata per utilizzare il nome di dominio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="ffd96-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="ffd96-154">Questa modifica è stata apportata per migliorare le prestazioni, perché quando un browser a cui fa riferimento il dominio microsoft.com invierebbe i cookie da quel dominio in rete con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="ffd96-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="ffd96-155">È stato rinominato in un nome di dominio diverso da microsoft.com prestazioni possono essere incrementata quanto a 25%.</span><span class="sxs-lookup"><span data-stu-id="ffd96-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="ffd96-156">Si noti ajax.microsoft.com continuerà a funzionare, ma è consigliabile ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="ffd96-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="ffd96-157">Formato precedente: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="ffd96-158">Nuovo formato: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="ffd96-159">Supporto di Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="ffd96-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="ffd96-160">Per utilizzare correttamente i file .vsdoc con Visual Studio 2008, è necessario assicurarsi di disporre di Visual Studio 2008 SP1 installato e l'installazione dell'hotfix per i file vsdoc.</span><span class="sxs-lookup"><span data-stu-id="ffd96-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="ffd96-161">È possibile ottenere questi da qui:</span><span class="sxs-lookup"><span data-stu-id="ffd96-161">You can get these from here:</span></span>

- [<span data-ttu-id="ffd96-162">Scaricare Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="ffd96-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "scaricare Visual Studio 2008 SP1")
- [<span data-ttu-id="ffd96-163">Scaricare l'hotfix .vsdoc per Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="ffd96-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "scaricare .vsdoc hotfix per Visual Studio 2008 SP1")

<span data-ttu-id="ffd96-164">Visual Studio 2010 supporta file .vsdoc senza tutte le patch aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="ffd96-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="ffd96-165">Utilizzando ASP.NET Ajax dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="ffd96-166">Quando si utilizza ASP.NET 4, è possibile reindirizzare tutte le richieste per gli script del framework ASP.NET per la rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="ffd96-167">Il recupero degli script dalla rete CDN anziché il server web locale può sostanzialmente di migliorare le prestazioni dei siti Web ASP.NET pubblico.</span><span class="sxs-lookup"><span data-stu-id="ffd96-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="ffd96-168">Utilizzare la proprietà ScriptManager EnableCDN per reindirizzare tutte le richieste di script framework ASP.NET Ajax CDN Microsoft:</span><span class="sxs-lookup"><span data-stu-id="ffd96-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="ffd96-169">Tramite jQuery dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="ffd96-170">È possibile utilizzare script jQuery ospitato nella rete CDN nell'applicazione Web aggiungendo il seguente elemento script a una pagina:</span><span class="sxs-lookup"><span data-stu-id="ffd96-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="ffd96-171">La rete CDN include anche la versione minimizzata dello script jQuery, che è possibile ottenere utilizzando l'elemento seguente:</span><span class="sxs-lookup"><span data-stu-id="ffd96-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="ffd96-172">Per consentire alla pagina di fallback per il caricamento di jQuery da un percorso locale nel proprio sito Web, se la rete CDN non disponibile, aggiungere l'elemento seguente immediatamente dopo l'elemento che fa riferimento la rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="ffd96-173">La pagina di esempio seguente usa la versione della rete CDN della libreria jQuery (con fallback a una copia locale) per visualizzare il contenuto di un elemento div quando viene premuto un pulsante.</span><span class="sxs-lookup"><span data-stu-id="ffd96-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="ffd96-174">È possibile approfondire la conoscenza di jQuery e scaricare una copia locale di jQuery visitando il [jQuery](http://jquery.com/) sito Web.</span><span class="sxs-lookup"><span data-stu-id="ffd96-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="ffd96-175">Utilizzo di jQuery UI dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="ffd96-176">La rete CDN ospita anche la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="ffd96-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="ffd96-177">La libreria dell'interfaccia utente jQuery include una vasta gamma di widget e gli effetti che è possibile utilizzare nelle applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ffd96-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="ffd96-178">Ad esempio, la pagina seguente viene illustrato come è possibile utilizzare jQuery UI Datepicker nel contesto di un'applicazione Web Form ASP.NET per visualizzare un calendario popup:</span><span class="sxs-lookup"><span data-stu-id="ffd96-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="ffd96-179">Quando si sposta lo stato attivo al controllo TextBox con la tastiera, viene visualizzato un calendario:</span><span class="sxs-lookup"><span data-stu-id="ffd96-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendario popup creato con Datepicker](overview/_static/image1.png)

<span data-ttu-id="ffd96-181">Si noti che è necessario includere tre file dalla rete CDN nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="ffd96-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="ffd96-182">La libreria jQuery &mdash; libreria jQuery UI dipende la libreria jQuery.</span><span class="sxs-lookup"><span data-stu-id="ffd96-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="ffd96-183">È necessario aggiungere la libreria jQuery alla pagina prima di aggiungere la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="ffd96-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="ffd96-184">La libreria dell'interfaccia utente jQuery &mdash; libreria jQuery UI contiene tutti gli effetti dell'interfaccia utente jQuery e widget, ad esempio il widget Datepicker utilizzate nella pagina precedente.</span><span class="sxs-lookup"><span data-stu-id="ffd96-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="ffd96-185">Un tema dell'interfaccia utente jQuery &mdash; jQuery UI supporta diversi temi.</span><span class="sxs-lookup"><span data-stu-id="ffd96-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="ffd96-186">La pagina precedente include un collegamento a un file CSS per importare il tema di Redmond.</span><span class="sxs-lookup"><span data-stu-id="ffd96-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="ffd96-187">Tutti i temi di jQuery standard dell'interfaccia utente sono ospitati nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="ffd96-188">[Visitare questa pagina](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sulla rete CDN di Microsoft Ajax") per visualizzare le anteprime per ognuno di essi.</span><span class="sxs-lookup"><span data-stu-id="ffd96-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="ffd96-189">Per ulteriori informazioni sulla libreria jQuery UI, visitare ufficiali [sito Web di jQuery UI](http://jQueryUI.com "sito Web di jQuery UI").</span><span class="sxs-lookup"><span data-stu-id="ffd96-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="ffd96-190">File di terze parti nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="ffd96-191">La rete CDN ospita alcune delle librerie di JavaScript più diffusi di terze parti.</span><span class="sxs-lookup"><span data-stu-id="ffd96-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="ffd96-192">Microsoft non rivendica la proprietà di tutte le librerie di terze parti ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="ffd96-193">I proprietari delle librerie di copyright licenza queste librerie all'utente.</span><span class="sxs-lookup"><span data-stu-id="ffd96-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="ffd96-194">Eventuali diritti che è possibile scaricare e usare tali librerie vengono concesse unicamente dai rispettivi proprietari copyright.</span><span class="sxs-lookup"><span data-stu-id="ffd96-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="ffd96-195">Poiché questi non sono librerie di Microsoft, Microsoft non offre alcuna garanzia o licenze di diritti di proprietà intellettuale (non incluso alcun diritto di brevetto implicito) per le librerie di terze parti ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="ffd96-196">Versioni di jQuery nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="ffd96-197">Le versioni seguenti di jQuery ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="ffd96-198">versione di jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-198">jQuery version 3.3.1</span></span>
- <span data-ttu-id="ffd96-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js</span></span>
- <span data-ttu-id="ffd96-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js</span></span>
- <span data-ttu-id="ffd96-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map</span></span>
- <span data-ttu-id="ffd96-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js</span></span>
- <span data-ttu-id="ffd96-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js</span></span>
- <span data-ttu-id="ffd96-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="ffd96-205">versione di jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-205">jQuery version 3.2.1</span></span>
- <span data-ttu-id="ffd96-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="ffd96-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="ffd96-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="ffd96-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="ffd96-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="ffd96-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="ffd96-212">versione di jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-212">jQuery version 3.2.0</span></span>

- <span data-ttu-id="ffd96-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="ffd96-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="ffd96-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="ffd96-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="ffd96-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="ffd96-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="ffd96-219">versione di jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-219">jQuery version 3.1.1</span></span>

- <span data-ttu-id="ffd96-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="ffd96-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="ffd96-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="ffd96-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="ffd96-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="ffd96-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="ffd96-226">jQuery versione 3.1.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-226">jQuery version 3.1.0</span></span>

- <span data-ttu-id="ffd96-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="ffd96-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="ffd96-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="ffd96-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="ffd96-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="ffd96-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="ffd96-233">versione di jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-233">jQuery version 3.0.0</span></span>

- <span data-ttu-id="ffd96-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="ffd96-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="ffd96-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="ffd96-237">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-237">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="ffd96-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="ffd96-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="ffd96-240">versione di jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-240">jQuery version 2.2.4</span></span>

- <span data-ttu-id="ffd96-241">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-241">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="ffd96-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="ffd96-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="ffd96-244">versione di jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-244">jQuery version 2.2.3</span></span>

- <span data-ttu-id="ffd96-245">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-245">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="ffd96-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="ffd96-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="ffd96-248">versione di jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-248">jQuery version 2.2.2</span></span>

- <span data-ttu-id="ffd96-249">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-249">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="ffd96-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="ffd96-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="ffd96-252">jQuery versione 2.2.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-252">jQuery version 2.2.1</span></span>

- <span data-ttu-id="ffd96-253">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-253">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="ffd96-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="ffd96-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="ffd96-256">versione di jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-256">jQuery version 2.2.0</span></span>

- <span data-ttu-id="ffd96-257">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-257">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="ffd96-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="ffd96-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="ffd96-260">versione di jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-260">jQuery version 2.1.4</span></span>

- <span data-ttu-id="ffd96-261">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-261">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="ffd96-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="ffd96-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="ffd96-264">versione di jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-264">jQuery version 2.1.3</span></span>

- <span data-ttu-id="ffd96-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="ffd96-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="ffd96-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="ffd96-268">versione di jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-268">jQuery version 2.1.2</span></span>

- <span data-ttu-id="ffd96-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="ffd96-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="ffd96-271">versione di jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-271">jQuery version 2.1.1</span></span>

- <span data-ttu-id="ffd96-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="ffd96-273">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-273">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="ffd96-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="ffd96-275">versione di jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-275">jQuery version 2.1.0</span></span>

- <span data-ttu-id="ffd96-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="ffd96-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="ffd96-278">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-278">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="ffd96-280">versione di jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-280">jQuery version 2.0.3</span></span>

- <span data-ttu-id="ffd96-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="ffd96-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="ffd96-283">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-283">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="ffd96-285">versione di jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-285">jQuery version 2.0.2</span></span>

- <span data-ttu-id="ffd96-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="ffd96-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="ffd96-288">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-288">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="ffd96-290">versione di jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-290">jQuery version 2.0.1</span></span>

- <span data-ttu-id="ffd96-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="ffd96-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="ffd96-293">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-293">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="ffd96-295">versione di jQuery 2.0.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-295">jQuery version 2.0.0</span></span>

- <span data-ttu-id="ffd96-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="ffd96-297">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-297">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="ffd96-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="ffd96-300">versione di jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-300">jQuery version 1.12.4</span></span>

- <span data-ttu-id="ffd96-301">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-301">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="ffd96-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="ffd96-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="ffd96-304">versione di jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-304">jQuery version 1.12.3</span></span>

- <span data-ttu-id="ffd96-305">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-305">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="ffd96-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="ffd96-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="ffd96-308">versione di jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-308">jQuery version 1.12.2</span></span>

- <span data-ttu-id="ffd96-309">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-309">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="ffd96-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="ffd96-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="ffd96-312">versione di jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-312">jQuery version 1.12.1</span></span>

- <span data-ttu-id="ffd96-313">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-313">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="ffd96-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="ffd96-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="ffd96-316">versione di jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-316">jQuery version 1.12.0</span></span>

- <span data-ttu-id="ffd96-317">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-317">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="ffd96-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="ffd96-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="ffd96-320">versione di jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-320">jQuery version 1.11.3</span></span>

- <span data-ttu-id="ffd96-321">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-321">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="ffd96-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="ffd96-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="ffd96-324">versione di jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-324">jQuery version 1.11.2</span></span>

- <span data-ttu-id="ffd96-325">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-325">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="ffd96-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="ffd96-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="ffd96-328">versione di jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-328">jQuery version 1.11.1</span></span>

- <span data-ttu-id="ffd96-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="ffd96-330">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-330">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="ffd96-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="ffd96-332">versione di jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-332">jQuery version 1.11.0</span></span>

- <span data-ttu-id="ffd96-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="ffd96-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="ffd96-335">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-335">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="ffd96-337">versione di jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-337">jQuery version 1.10.2</span></span>

- <span data-ttu-id="ffd96-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="ffd96-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="ffd96-340">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-340">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="ffd96-342">versione di jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-342">jQuery version 1.10.1</span></span>

- <span data-ttu-id="ffd96-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="ffd96-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="ffd96-345">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-345">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="ffd96-347">versione di jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-347">jQuery version 1.10.0</span></span>

- <span data-ttu-id="ffd96-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="ffd96-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="ffd96-350">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-350">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="ffd96-352">versione di jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-352">jQuery version 1.9.1</span></span>

- <span data-ttu-id="ffd96-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="ffd96-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="ffd96-355">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-355">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="ffd96-357">versione di jQuery alla 1.9.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-357">jQuery version 1.9.0</span></span>

- <span data-ttu-id="ffd96-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="ffd96-359">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-359">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="ffd96-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="ffd96-362">versione di jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-362">jQuery version 1.8.3</span></span>

- <span data-ttu-id="ffd96-363">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-363">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="ffd96-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="ffd96-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="ffd96-366">versione di jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-366">jQuery version 1.8.2</span></span>

- <span data-ttu-id="ffd96-367">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-367">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="ffd96-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="ffd96-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="ffd96-370">versione di jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-370">jQuery version 1.8.1</span></span>

- <span data-ttu-id="ffd96-371">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-371">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="ffd96-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="ffd96-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="ffd96-374">versione di jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-374">jQuery version 1.8.0</span></span>

- <span data-ttu-id="ffd96-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="ffd96-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="ffd96-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="ffd96-378">versione di jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-378">jQuery version 1.7.2</span></span>

- <span data-ttu-id="ffd96-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="ffd96-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="ffd96-381">versione di jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-381">jQuery version 1.7.1</span></span>

- <span data-ttu-id="ffd96-382">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-382">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="ffd96-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="ffd96-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="ffd96-385">jQuery versione 1.7</span><span class="sxs-lookup"><span data-stu-id="ffd96-385">jQuery version 1.7</span></span>

- <span data-ttu-id="ffd96-386">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-386">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="ffd96-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="ffd96-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="ffd96-389">versione di jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-389">jQuery version 1.6.4</span></span>

- <span data-ttu-id="ffd96-390">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-390">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="ffd96-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="ffd96-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="ffd96-393">versione di jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-393">jQuery version 1.6.3</span></span>

- <span data-ttu-id="ffd96-394">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-394">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="ffd96-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="ffd96-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="ffd96-397">versione di jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-397">jQuery version 1.6.2</span></span>

- <span data-ttu-id="ffd96-398">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-398">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="ffd96-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="ffd96-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="ffd96-401">versione di jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-401">jQuery version 1.6.1</span></span>

- <span data-ttu-id="ffd96-402">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-402">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="ffd96-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="ffd96-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="ffd96-405">jQuery versione 1.6</span><span class="sxs-lookup"><span data-stu-id="ffd96-405">jQuery version 1.6</span></span>

- <span data-ttu-id="ffd96-406">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-406">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="ffd96-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="ffd96-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="ffd96-409">versione di jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-409">jQuery version 1.5.2</span></span>

- <span data-ttu-id="ffd96-410">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-410">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="ffd96-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="ffd96-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="ffd96-413">versione di jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-413">jQuery version 1.5.1</span></span>

- <span data-ttu-id="ffd96-414">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-414">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="ffd96-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="ffd96-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="ffd96-417">jQuery versione 1.5</span><span class="sxs-lookup"><span data-stu-id="ffd96-417">jQuery version 1.5</span></span>

- <span data-ttu-id="ffd96-418">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-418">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="ffd96-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="ffd96-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="ffd96-421">versione di jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-421">jQuery version 1.4.4</span></span>

- <span data-ttu-id="ffd96-422">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-422">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="ffd96-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="ffd96-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="ffd96-425">versione di jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-425">jQuery version 1.4.3</span></span>

- <span data-ttu-id="ffd96-426">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-426">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="ffd96-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="ffd96-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="ffd96-429">versione di jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-429">jQuery version 1.4.2</span></span>

- <span data-ttu-id="ffd96-430">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-430">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="ffd96-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="ffd96-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="ffd96-433">versione di jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-433">jQuery version 1.4.1</span></span>

- <span data-ttu-id="ffd96-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="ffd96-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="ffd96-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="ffd96-437">jQuery versione 1.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-437">jQuery version 1.4</span></span>

- <span data-ttu-id="ffd96-438">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-438">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="ffd96-439">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-439">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="ffd96-440">versione di jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-440">jQuery version 1.3.2</span></span>

- <span data-ttu-id="ffd96-441">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-441">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="ffd96-442">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-442">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="ffd96-443">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-443">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="ffd96-444">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-444">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="ffd96-445">Versioni di eseguire la migrazione di jQuery nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-445">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="ffd96-446">Le versioni seguenti di jQuery migrazione sono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-446">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="ffd96-447">eseguire la migrazione della versione 3.0.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="ffd96-447">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="ffd96-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="ffd96-449">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-449">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="ffd96-450">Migrazione versione 1.2.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="ffd96-450">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="ffd96-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="ffd96-452">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-452">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="ffd96-453">jQuery migrazione versione 1.2.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-453">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="ffd96-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="ffd96-455">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-455">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="ffd96-456">Migrazione versione 1.1.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="ffd96-456">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="ffd96-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="ffd96-458">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-458">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="ffd96-459">Migrazione versione 1.1.0 jQuery</span><span class="sxs-lookup"><span data-stu-id="ffd96-459">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="ffd96-460">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-460">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="ffd96-461">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-461">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="ffd96-462">jQuery versione 1.0.0 di migrazione</span><span class="sxs-lookup"><span data-stu-id="ffd96-462">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="ffd96-463">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-463">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="ffd96-464">http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-464">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="ffd96-465">jQuery versioni dell'interfaccia utente nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-465">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="ffd96-466">Le versioni della libreria jQuery UI seguenti sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-466">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="ffd96-467">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="ffd96-467">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ffd96-468">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-468">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-469">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-469">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-470">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-470">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-471">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-471">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-472">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-472">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-473">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-473">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-474">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-474">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-475">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-475">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-476">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-476">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-477">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-477">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-478">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-478">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-479">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-479">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-480">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-480">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-481">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-481">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-482">jQuery UI alla 1.9.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-482">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI alla 1.9.0 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-483">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="ffd96-483">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-484">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="ffd96-484">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-485">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="ffd96-485">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-486">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="ffd96-486">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-487">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="ffd96-487">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-488">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="ffd96-488">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-489">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="ffd96-489">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-490">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="ffd96-490">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-491">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="ffd96-491">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-492">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="ffd96-492">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-493">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="ffd96-493">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-494">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="ffd96-494">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-495">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="ffd96-495">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-496">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="ffd96-496">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-497">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="ffd96-497">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-498">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="ffd96-498">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-499">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="ffd96-499">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-500">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="ffd96-500">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-501">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="ffd96-501">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 sulla rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-502">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="ffd96-502">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="ffd96-503">Versioni di convalida nella rete CDN jQuery</span><span class="sxs-lookup"><span data-stu-id="ffd96-503">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="ffd96-504">Le versioni della libreria di convalida jQuery seguenti sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-504">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="ffd96-505">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="ffd96-505">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ffd96-506">jQuery convalida 1.17.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-506">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "1.17.0 convalida jQuery")
- [<span data-ttu-id="ffd96-507">jQuery convalida 1.16.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-507">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "1.16.0 convalida jQuery")
- [<span data-ttu-id="ffd96-508">jQuery convalida 1.15.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-508">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "1.15.1 convalida jQuery")
- [<span data-ttu-id="ffd96-509">jQuery convalida 1.15.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-509">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "1.15.0 convalida jQuery")
- [<span data-ttu-id="ffd96-510">jQuery convalida 1.14.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-510">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "1.14.0 convalida jQuery")
- [<span data-ttu-id="ffd96-511">jQuery convalida 1.13.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-511">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "1.13.1 convalida jQuery")
- [<span data-ttu-id="ffd96-512">jQuery convalida 1.13.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-512">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "1.13.0 convalida jQuery")
- [<span data-ttu-id="ffd96-513">jQuery convalida 1.12.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-513">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "1.12.0 convalida jQuery")
- [<span data-ttu-id="ffd96-514">jQuery convalida 1.11.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-514">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "1.11.1 convalida jQuery")
- [<span data-ttu-id="ffd96-515">jQuery convalida 1.11.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-515">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "1.11.0 convalida jQuery")
- [<span data-ttu-id="ffd96-516">jQuery convalida 1.10.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-516">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "1.10.0 convalida jQuery")
- [<span data-ttu-id="ffd96-517">jQuery convalida 1.9</span><span class="sxs-lookup"><span data-stu-id="ffd96-517">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versione 1.9")
- [<span data-ttu-id="ffd96-518">jQuery convalida 1.8.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-518">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versione 1.8.1")
- [<span data-ttu-id="ffd96-519">jQuery convalida 1.8</span><span class="sxs-lookup"><span data-stu-id="ffd96-519">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versione 1.8")
- [<span data-ttu-id="ffd96-520">jQuery convalida 1.7</span><span class="sxs-lookup"><span data-stu-id="ffd96-520">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versione 1.7")
- [<span data-ttu-id="ffd96-521">jQuery convalida 1.6</span><span class="sxs-lookup"><span data-stu-id="ffd96-521">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery convalida 1.6")
- [<span data-ttu-id="ffd96-522">jQuery convalida 1.5.5</span><span class="sxs-lookup"><span data-stu-id="ffd96-522">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "1.5.5 convalida jQuery")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="ffd96-523">jQuery Mobile versioni nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-523">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="ffd96-524">Le seguenti versioni della libreria jQuery Mobile sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-524">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="ffd96-525">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="ffd96-525">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ffd96-526">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="ffd96-526">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-527">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-527">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-528">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-528">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-529">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-529">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-530">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-530">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-531">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-531">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-532">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-532">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-533">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-533">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-534">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-534">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-535">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-535">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-536">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-536">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-537">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="ffd96-537">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-538">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-538">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-539">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-539">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-540">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="ffd96-540">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-541">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="ffd96-541">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 nella rete CDN di Microsoft Ajax")
- [<span data-ttu-id="ffd96-542">la versione beta di jQuery Mobile 1.0 3</span><span class="sxs-lookup"><span data-stu-id="ffd96-542">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 nella rete CDN di Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="ffd96-543">jQuery rilasci di modelli di rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-543">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="ffd96-544">Le seguenti versioni di plug-in modelli jQuery sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-544">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="ffd96-545">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="ffd96-545">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ffd96-546">jQuery modelli Beta 1</span><span class="sxs-lookup"><span data-stu-id="ffd96-546">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery modelli Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="ffd96-547">jQuery versioni ciclo nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-547">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="ffd96-548">Le versioni seguenti di plug-in del ciclo di jQuery sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-548">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="ffd96-549">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="ffd96-549">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ffd96-550">jQuery 2,99 ciclo</span><span class="sxs-lookup"><span data-stu-id="ffd96-550">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 ciclo")
- [<span data-ttu-id="ffd96-551">jQuery ciclo 2.94</span><span class="sxs-lookup"><span data-stu-id="ffd96-551">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 ciclo")
- [<span data-ttu-id="ffd96-552">jQuery 2,88 ciclo</span><span class="sxs-lookup"><span data-stu-id="ffd96-552">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 ciclo")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="ffd96-553">jQuery versioni DataTable nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-553">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="ffd96-554">Le seguenti versioni di jQuery DataTable plug-in sono ospitate in questa rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-554">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="ffd96-555">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="ffd96-555">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ffd96-556">jQuery DataTable 1.10.5</span><span class="sxs-lookup"><span data-stu-id="ffd96-556">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTable 1.10.5")
- [<span data-ttu-id="ffd96-557">jQuery DataTable 1.10.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-557">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTable 1.10.4")
- [<span data-ttu-id="ffd96-558">jQuery DataTable 1.9.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-558">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTable 1.9.4")
- [<span data-ttu-id="ffd96-559">jQuery DataTable 1.9.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-559">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTable 1.9.3")
- [<span data-ttu-id="ffd96-560">jQuery DataTable 1.9.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-560">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTable 1.9.2")
- [<span data-ttu-id="ffd96-561">jQuery DataTable 1.9.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-561">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTable 1.9.1")
- [<span data-ttu-id="ffd96-562">jQuery DataTable alla 1.9.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-562">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTable alla 1.9.0")
- [<span data-ttu-id="ffd96-563">jQuery DataTable 1.8.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-563">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTable 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="ffd96-564">Versioni di Modernizr nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-564">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="ffd96-565">Le seguenti versioni di [Modernizr](http://www.modernizr.com "Modernizr") ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-565">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="ffd96-566">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-566">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="ffd96-567">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-567">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="ffd96-568">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-568">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="ffd96-569">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-569">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="ffd96-570">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-570">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="ffd96-571">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-571">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="ffd96-572">Versioni di JSHint nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-572">JSHint Releases on the CDN</span></span>

<span data-ttu-id="ffd96-573">Le seguenti versioni di [JSHint](http://www.jshint.com "JSHint") ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-573">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="ffd96-574">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-574">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="ffd96-575">Versioni di Knockout nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-575">Knockout Releases on the CDN</span></span>

<span data-ttu-id="ffd96-576">Le seguenti versioni di [Knockout](http://www.knockoutjs.com "Knockout") ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-576">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="ffd96-577">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="ffd96-578">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="ffd96-579">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="ffd96-580">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="ffd96-581">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="ffd96-582">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="ffd96-583">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="ffd96-584">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="ffd96-585">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="ffd96-586">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="ffd96-587">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="ffd96-588">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="ffd96-589">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="ffd96-590">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-590">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="ffd96-591">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-591">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="ffd96-592">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-592">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="ffd96-593">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-593">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="ffd96-594">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-594">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="ffd96-595">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-595">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="ffd96-596">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-596">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="ffd96-597">Globalizzare le versioni su una rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-597">Globalize Releases on the CDN</span></span>

<span data-ttu-id="ffd96-598">Le seguenti versioni di [Globalize](https://github.com/jquery/globalize "Globalize") ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-598">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="ffd96-599">La versione 1.0.0 di globalizzazione</span><span class="sxs-lookup"><span data-stu-id="ffd96-599">Globalize version 1.0.0</span></span>

- <span data-ttu-id="ffd96-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="ffd96-601">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/Node-Main.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-601">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="ffd96-602">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-602">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="ffd96-603">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/date.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-603">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="ffd96-604">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-604">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="ffd96-605">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-605">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="ffd96-606">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-606">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="ffd96-607">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-607">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="ffd96-608">Globalizzazione versione 0.1.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-608">Globalize version 0.1.1</span></span>

- <span data-ttu-id="ffd96-609">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-609">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="ffd96-610">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-610">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="ffd96-611">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Cultures.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-611">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="ffd96-612">tutte le impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="ffd96-612">all cultures</span></span>
- <span data-ttu-id="ffd96-613">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Culture. {codice delle impostazioni cultura} js</span><span class="sxs-lookup"><span data-stu-id="ffd96-613">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="ffd96-614">Sostituire "{codice delle impostazioni cultura}" con il codice della lingua desiderata, ad esempio, dei file in rete CDN di Microsoft globalize.culture.en GB.js== = = queste librerie sono state caricate da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ffd96-614">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="ffd96-615">Rispondere versioni su una rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-615">Respond Releases on the CDN</span></span>

<span data-ttu-id="ffd96-616">Le seguenti versioni di [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") rispondono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-616">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="ffd96-617">Rispondere versione 1.4.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-617">Respond version 1.4.2</span></span>

- <span data-ttu-id="ffd96-618">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="ffd96-619">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="ffd96-620">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-620">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="ffd96-621">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="ffd96-622">Rispondere versione 1.4.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-622">Respond version 1.4.1</span></span>

- <span data-ttu-id="ffd96-623">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="ffd96-624">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="ffd96-625">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-625">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="ffd96-626">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-626">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="ffd96-627">Rispondere versione 1.4.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-627">Respond version 1.4.0</span></span>

- <span data-ttu-id="ffd96-628">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-628">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="ffd96-629">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-629">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="ffd96-630">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-630">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="ffd96-631">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-631">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="ffd96-632">Rispondere versione 1.3.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-632">Respond version 1.3.0</span></span>

- <span data-ttu-id="ffd96-633">http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-633">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="ffd96-634">Rispondere versione 1.2.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-634">Respond version 1.2.0</span></span>

- <span data-ttu-id="ffd96-635">http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-635">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="ffd96-636">Versioni di bootstrap nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-636">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="ffd96-637">Le seguenti versioni di [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-637">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="ffd96-638">Versione bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="ffd96-638">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="ffd96-639">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-640">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-641">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-642">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ffd96-643">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-644">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-645">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-645">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ffd96-646">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-647">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-648">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-649">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-650">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="ffd96-651">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="ffd96-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="ffd96-652">Versione bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="ffd96-652">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="ffd96-653">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-654">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-655">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-656">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ffd96-657">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-658">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-659">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-659">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ffd96-660">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-661">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-662">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-663">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-664">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="ffd96-665">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="ffd96-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="ffd96-666">Bootstrap versione 3.3.5</span><span class="sxs-lookup"><span data-stu-id="ffd96-666">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="ffd96-667">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-668">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-669">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-670">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ffd96-671">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-672">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-673">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-673">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ffd96-674">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-675">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-676">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-677">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-678">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="ffd96-679">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="ffd96-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="ffd96-680">Versione bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-680">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="ffd96-681">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-682">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-683">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-684">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ffd96-685">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-686">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-687">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-687">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ffd96-688">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-689">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-690">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-691">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-692">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="ffd96-693">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="ffd96-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="ffd96-694">Versione bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-694">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="ffd96-695">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-696">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-697">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-698">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ffd96-699">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-700">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-701">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-701">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ffd96-702">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-703">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-704">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-705">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-706">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="ffd96-707">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="ffd96-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="ffd96-708">Versione bootstrap 3.3.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-708">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="ffd96-709">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-710">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-711">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-712">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ffd96-713">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-714">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-714">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-715">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ffd96-716">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-717">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-718">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-719">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-720">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="ffd96-721">Versione bootstrap 3.3.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-721">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="ffd96-722">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-723">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-724">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-725">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ffd96-726">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-727">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-727">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-728">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ffd96-729">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-730">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-731">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-732">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-733">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="ffd96-734">Versione bootstrap 3.2.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-734">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="ffd96-735">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-736">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-737">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-738">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ffd96-739">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-740">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-740">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-741">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ffd96-742">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-743">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-744">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-745">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-746">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="ffd96-747">Versione bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-747">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="ffd96-748">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-749">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-750">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-751">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ffd96-752">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-753">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-753">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-754">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ffd96-755">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-756">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-757">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-758">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-759">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="ffd96-760">Bootstrap versione 3.1.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-760">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="ffd96-761">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-762">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-763">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-764">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="ffd96-765">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-766">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-766">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-767">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="ffd96-768">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-769">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-770">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-771">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-772">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="ffd96-773">Versione bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-773">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="ffd96-774">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-775">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-776">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-777">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-777">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-778">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-779">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-780">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-781">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-782">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-783">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="ffd96-784">Versione bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-784">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="ffd96-785">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-786">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-787">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-788">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-788">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-789">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-790">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-791">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-792">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-793">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-794">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="ffd96-795">Versione bootstrap 3.0.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-795">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="ffd96-796">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-797">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-798">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-799">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-799">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-800">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-801">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-802">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-803">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-804">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-805">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="ffd96-806">Versione bootstrap 3.0.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-806">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="ffd96-807">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-808">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-809">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-810">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-810">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-811">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap-theme.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-811">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="ffd96-812">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap-theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-812">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="ffd96-813">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="ffd96-813">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="ffd96-814">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="ffd96-814">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="ffd96-815">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="ffd96-815">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="ffd96-816">http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="ffd96-816">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="ffd96-817">Bootstrap versione 2.3.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-817">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="ffd96-818">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-819">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-819">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-820">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-821">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-822">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="ffd96-823">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="ffd96-824">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/IMG/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="ffd96-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="ffd96-825">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/IMG/glyphicons-halflings-white.PNG</span><span class="sxs-lookup"><span data-stu-id="ffd96-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="ffd96-826">Bootstrap versione 2.3.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-826">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="ffd96-827">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="ffd96-828">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-828">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="ffd96-829">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-829">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="ffd96-830">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-830">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="ffd96-831">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-831">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="ffd96-832">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-832">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="ffd96-833">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/IMG/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="ffd96-833">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="ffd96-834">http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/IMG/glyphicons-halflings-white.PNG</span><span class="sxs-lookup"><span data-stu-id="ffd96-834">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="ffd96-835">Versioni di bootstrap TouchCarousel nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-835">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="ffd96-836">Le seguenti versioni di [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") TouchCarousel Bootstrap versioni sono ospitate nella rete CDN :</span><span class="sxs-lookup"><span data-stu-id="ffd96-836">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="ffd96-837">Bootstrap TouchCarousel versione 0.8.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-837">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="ffd96-838">http://AJAX.aspnetcdn.com/AJAX/bootstrap-touch-carousel/0.8.0/CSS/bootstrap-touch-carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="ffd96-838">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="ffd96-839">http://AJAX.aspnetcdn.com/AJAX/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-839">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="ffd96-840">Versioni di Hammer.js nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-840">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="ffd96-841">Le seguenti versioni di [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js versioni ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-841">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="ffd96-842">Versione Hammer.js 2.0.4</span><span class="sxs-lookup"><span data-stu-id="ffd96-842">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="ffd96-843">http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-843">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="ffd96-844">http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-844">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="ffd96-845">http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.min.Map</span><span class="sxs-lookup"><span data-stu-id="ffd96-845">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="ffd96-846">Web Form ASP.NET e Ajax versioni nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-846">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="ffd96-847">Le seguenti versioni di ASP.NET Ajax Library sono ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffd96-847">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="ffd96-848">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="ffd96-848">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ffd96-849">Web Form ASP.NET e Ajax versione 4.5.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-849">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Form ASP.NET e Ajax 4.5.2")
- [<span data-ttu-id="ffd96-850">Web Form ASP.NET e Ajax versione 4</span><span class="sxs-lookup"><span data-stu-id="ffd96-850">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Form ASP.NET e Ajax 4")
- [<span data-ttu-id="ffd96-851">ASP.NET Ajax versione 3.5</span><span class="sxs-lookup"><span data-stu-id="ffd96-851">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="ffd96-852">ASP.NET MVC rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-852">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="ffd96-853">I seguenti file JavaScript di ASP.NET MVC sono ospitati in questa rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-853">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="ffd96-854">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-854">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="ffd96-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-855">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="ffd96-856">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-856">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="ffd96-857">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-857">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="ffd96-858">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-858">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="ffd96-859">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-859">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="ffd96-860">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-860">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="ffd96-861">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-861">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="ffd96-862">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-862">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="ffd96-863">MVC ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-863">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="ffd96-864">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-864">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="ffd96-865">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-865">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="ffd96-866">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-866">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="ffd96-867">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-867">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="ffd96-868">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-868">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="ffd96-869">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-869">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="ffd96-870">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-870">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="ffd96-871">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-871">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="ffd96-872">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-872">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="ffd96-873">MVC ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-873">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="ffd96-874">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-874">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="ffd96-875">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-875">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="ffd96-876">ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="ffd96-876">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="ffd96-877">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-877">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="ffd96-878">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.debug.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-878">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="ffd96-879">ASP.NET SignalR rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="ffd96-879">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="ffd96-880">I seguenti file di ASP.NET SignalR JavaScript sono ospitati in questa rete CDN:</span><span class="sxs-lookup"><span data-stu-id="ffd96-880">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="ffd96-881">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-881">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="ffd96-882">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="ffd96-883">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-883">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="ffd96-884">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-884">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="ffd96-885">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="ffd96-886">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-886">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="ffd96-887">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-887">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="ffd96-888">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="ffd96-889">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-889">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="ffd96-890">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-890">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="ffd96-891">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="ffd96-892">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-892">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="ffd96-893">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-893">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="ffd96-894">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="ffd96-895">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-895">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="ffd96-896">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-896">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="ffd96-897">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="ffd96-898">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-898">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="ffd96-899">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-899">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="ffd96-900">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="ffd96-901">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-901">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="ffd96-902">SignalR ASP.NET 2.0.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-902">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="ffd96-903">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="ffd96-904">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-904">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="ffd96-905">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="ffd96-905">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="ffd96-906">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="ffd96-907">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-907">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="ffd96-908">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="ffd96-908">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="ffd96-909">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="ffd96-910">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-910">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="ffd96-911">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-911">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="ffd96-912">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="ffd96-913">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-913">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="ffd96-914">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="ffd96-914">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="ffd96-915">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-915">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="ffd96-916">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-916">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="ffd96-917">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="ffd96-917">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="ffd96-918">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-918">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="ffd96-919">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="ffd96-919">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="ffd96-920">Per informazioni sulle condizioni di utilizzo per la rete CDN, vedere [Microsoft Ajax CDN condizioni di utilizzo](https://www.asp.net/terms-of-use "Microsoft Ajax CDN condizioni di utilizzo").</span><span class="sxs-lookup"><span data-stu-id="ffd96-920">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
