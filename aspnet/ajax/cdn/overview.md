---
uid: ajax/cdn/overview
title: Rete CDN Microsoft Ajax | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 3ec70bb0746d18e453b6e5728b6ba90b2a3e87f3
ms.sourcegitcommit: 19cbda409bdbbe42553dc385ea72d2a8e246509c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992935"
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="3f318-102">Rete CDN Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="3f318-102">Microsoft Ajax Content Delivery Network</span></span>
====================
> [!WARNING]
> <span data-ttu-id="3f318-103">Non è stato necessario considerare una dipendenza rigida in asset della rete CDN con le applicazioni di produzione.</span><span class="sxs-lookup"><span data-stu-id="3f318-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="3f318-104">Le applicazioni devono di test per l'asset della rete CDN fare riferimento e usare un asset di fallback quando non è disponibile la rete CDN.</span><span class="sxs-lookup"><span data-stu-id="3f318-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span> 
>
> <span data-ttu-id="3f318-105">Rete CDN Microsoft Ajax non dispone di alcun contratto di servizio in aggiunta a una rete CDN di Azure.</span><span class="sxs-lookup"><span data-stu-id="3f318-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="3f318-106">Uso [questo problema su GitHub](https://github.com/aspnet/Docs/issues/5832) per segnalare i problemi con la rete CDN Microsoft Ajax.</span><span class="sxs-lookup"><span data-stu-id="3f318-106">Use [this GitHub issue](https://github.com/aspnet/Docs/issues/5832) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="3f318-107">Sommario</span><span class="sxs-lookup"><span data-stu-id="3f318-107">Table of Contents</span></span>

<span data-ttu-id="3f318-108">**[AJAX.microsoft.com rinominata ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="3f318-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="3f318-109">**[Supporto di Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="3f318-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="3f318-110">**[Utilizzo di ASP.NET Ajax dalla rete CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="3f318-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="3f318-111">**[Uso di jQuery dalla rete CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="3f318-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="3f318-112">**[Usando l'interfaccia utente dalla rete CDN di jQuery](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="3f318-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="3f318-113">**[File di terze parti sulla rete CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="3f318-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="3f318-114">Versioni di jQuery per la rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="3f318-115">Versioni di eseguire la migrazione di jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="3f318-116">Versioni dell'interfaccia utente per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="3f318-117">Versioni di convalida per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="3f318-118">jQuery Mobile versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="3f318-119">Versioni di modelli per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="3f318-120">Ciclo di versioni per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="3f318-121">jQuery DataTables versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="3f318-122">Versioni di Modernizr per la rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="3f318-123">Versioni JSHint sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="3f318-124">Versioni Knockout sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="3f318-125">Globalizzare versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="3f318-126">Rispondere versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="3f318-127">Versioni bootstrap sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="3f318-128">Versioni TouchCarousel bootstrap sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="3f318-129">Versioni Hammer.js sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="3f318-130">Web Form ASP.NET e Ajax versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="3f318-131">ASP.NET MVC rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="3f318-132">ASP.NET SignalR rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="3f318-133">Il Content Delivery Network (rete CDN Microsoft Ajax) ospita le librerie JavaScript di terze parti più diffusi, ad esempio jQuery e consente di aggiungere facilmente alle applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="3f318-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="3f318-134">Ad esempio, è possibile iniziare a usare jQuery ospitato nella rete CDN in questo semplicemente aggiungendo un &lt;script&gt; tag a una pagina che punta a ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="3f318-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="3f318-135">Grazie all'uso della rete CDN, è possibile migliorare significativamente le prestazioni delle applicazioni Ajax.</span><span class="sxs-lookup"><span data-stu-id="3f318-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="3f318-136">Il contenuto della rete CDN viene memorizzati nella cache nei server di tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="3f318-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="3f318-137">Inoltre, la rete CDN consente ai browser di riutilizzare i file JavaScript di terze parti memorizzata nella cache per i siti web che si trovano in domini diversi.</span><span class="sxs-lookup"><span data-stu-id="3f318-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="3f318-138">La rete CDN supporta SSL (HTTPS) nel caso in cui è necessario presentare una pagina web utilizzando Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="3f318-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="3f318-139">La rete CDN ospita le seguenti librerie di script di terze parti che sono state caricate e sono concessi in licenza all'utente, dai proprietari di queste librerie:</span><span class="sxs-lookup"><span data-stu-id="3f318-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="3f318-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="3f318-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="3f318-141">interfaccia utente (www.jqueryui.com) di jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="3f318-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="3f318-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="3f318-143">jQuery Validation (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="3f318-143">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="3f318-144">jQuery ciclo (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="3f318-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="3f318-145">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="3f318-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="3f318-146">Rete CDN Microsoft Ajax include anche le librerie seguenti che sono state caricate da Microsoft:</span><span class="sxs-lookup"><span data-stu-id="3f318-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="3f318-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="3f318-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="3f318-148">File JavaScript di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3f318-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="3f318-149">File ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="3f318-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="3f318-150">Microsoft non rivendica la proprietà di tutte le librerie di terze parti ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="3f318-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="3f318-151">I proprietari del copyright delle librerie sono licenze queste librerie all'utente.</span><span class="sxs-lookup"><span data-stu-id="3f318-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="3f318-152">Eventuali diritti che potrebbe essere necessario scaricare e usare tali librerie vengono concessi esclusivamente per i rispettivi proprietari del copyright.</span><span class="sxs-lookup"><span data-stu-id="3f318-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="3f318-153">Poiché questi non sono librerie Microsoft, Microsoft non offre alcuna garanzia né licenze di diritti di proprietà intellettuale (non incluso alcun diritto di brevetto implicito) per le librerie di terze parti ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="3f318-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="3f318-154">Se si vuole inviare la libreria JavaScript e la libreria è una delle librerie JavaScript superiore (come indicato nella http://trends.builtwith.com) o le estensioni plug-in o da queste librerie sono (a) più diffusi; o (b) utili per l'utilizzo in ASP.NET quindi contattare AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="3f318-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="3f318-155">AJAX.microsoft.com rinominata ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="3f318-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="3f318-156">La rete CDN consente di usare il nome di dominio microsoft.com ed è stata modificata per usare il nome di dominio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="3f318-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="3f318-157">Questa modifica è stata apportata per migliorare le prestazioni poiché quando un browser viene fatto riferimento il dominio microsoft.com lo invierà tutti i cookie da quel dominio attraverso la rete con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="3f318-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="3f318-158">Rinominando con un nome di dominio diverso da microsoft.com prestazioni possono essere incrementata di gran parte al 25%.</span><span class="sxs-lookup"><span data-stu-id="3f318-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="3f318-159">Si noti ajax.microsoft.com continueranno a funzionare, ma è consigliabile ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="3f318-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="3f318-160">Vecchio formato: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="3f318-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="3f318-161">Nuovo formato: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="3f318-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="3f318-162">Supporto di Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="3f318-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="3f318-163">Per usare correttamente i file .vsdoc con Visual Studio 2008, è necessario assicurarsi che sia installato Visual Studio 2008 SP1 installato e installato l'hotfix per i file di vsdoc.</span><span class="sxs-lookup"><span data-stu-id="3f318-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="3f318-164">È possibile ottenerli da qui:</span><span class="sxs-lookup"><span data-stu-id="3f318-164">You can get these from here:</span></span>

- [<span data-ttu-id="3f318-165">Download di Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="3f318-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "scaricare Visual Studio 2008 SP1")
- [<span data-ttu-id="3f318-166">Download dell'hotfix .vsdoc per Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="3f318-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "scaricare .vsdoc hotfix per Visual Studio 2008 SP1")

<span data-ttu-id="3f318-167">Visual Studio 2010 supporta file .vsdoc senza tutte le patch aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="3f318-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="3f318-168">Utilizzo di ASP.NET Ajax dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="3f318-169">Quando si usa ASP.NET 4, è possibile reindirizzare tutte le richieste per gli script del framework ASP.NET per la rete CDN.</span><span class="sxs-lookup"><span data-stu-id="3f318-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="3f318-170">Il recupero degli script dalla rete CDN anziché il server web locale può migliorare notevolmente le prestazioni di siti Web pubblici di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3f318-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="3f318-171">Usare la proprietà di ScriptManager EnableCDN per reindirizzare tutte le richieste di script framework ASP.NET per la rete CDN Microsoft Ajax:</span><span class="sxs-lookup"><span data-stu-id="3f318-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="3f318-172">Uso di jQuery dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="3f318-173">È possibile usare gli script jQuery ospitati nella rete CDN nell'applicazione Web aggiungendo il seguente elemento script a una pagina:</span><span class="sxs-lookup"><span data-stu-id="3f318-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="3f318-174">La rete CDN include anche la versione minimizzata dello script jQuery, che è possibile ottenere usando l'elemento seguente:</span><span class="sxs-lookup"><span data-stu-id="3f318-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="3f318-175">Per consentire alla pagina di fallback per il caricamento di jQuery da un percorso locale sul proprio sito Web in caso di indisponibilità della rete CDN, aggiungere l'elemento seguente immediatamente dopo l'elemento che fa riferimento la rete CDN:</span><span class="sxs-lookup"><span data-stu-id="3f318-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="3f318-176">La pagina di esempio seguente usa la versione della rete CDN di jQuery libreria (con il fallback per una copia locale) per visualizzare il contenuto di un elemento div quando viene selezionato un pulsante.</span><span class="sxs-lookup"><span data-stu-id="3f318-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="3f318-177">È possibile approfondire la conoscenza di jQuery e scaricare una copia locale di jQuery, visitare il [jQuery](http://jquery.com/) sito Web.</span><span class="sxs-lookup"><span data-stu-id="3f318-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="3f318-178">Usando l'interfaccia utente dalla rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="3f318-179">La rete CDN ospita anche la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="3f318-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="3f318-180">La libreria dell'interfaccia utente di jQuery include un set completo di widget e gli effetti che è possibile usare nelle applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3f318-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="3f318-181">Ad esempio, la pagina seguente viene illustrato come è possibile usare il componente aggiuntivo jQuery UI Datepicker nel contesto di un'applicazione Web Form ASP.NET per visualizzare un calendario popup:</span><span class="sxs-lookup"><span data-stu-id="3f318-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="3f318-182">Quando si sposta lo stato attivo alla casella di testo usando la tastiera, viene visualizzato un calendario:</span><span class="sxs-lookup"><span data-stu-id="3f318-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendario popup per la creazione con selezione data](overview/_static/image1.png)

<span data-ttu-id="3f318-184">Si noti che è necessario includere tre file dalla rete CDN nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="3f318-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="3f318-185">La libreria jQuery &mdash; la libreria dell'interfaccia utente di jQuery dipende dalla libreria jQuery.</span><span class="sxs-lookup"><span data-stu-id="3f318-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="3f318-186">È necessario aggiungere la libreria jQuery per la pagina prima di aggiungere la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="3f318-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="3f318-187">La libreria dell'interfaccia utente di jQuery &mdash; la libreria dell'interfaccia utente di jQuery contiene tutti gli effetti dell'interfaccia utente di jQuery e widget, ad esempio il widget Datepicker usato nella pagina precedente.</span><span class="sxs-lookup"><span data-stu-id="3f318-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="3f318-188">Un tema dell'interfaccia utente di jQuery &mdash; l'interfaccia utente di jQuery supporta diversi temi disponibili.</span><span class="sxs-lookup"><span data-stu-id="3f318-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="3f318-189">La pagina precedente include un collegamento a un file CSS per importare il tema di Redmond.</span><span class="sxs-lookup"><span data-stu-id="3f318-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="3f318-190">Tutti i temi di interfaccia utente di jQuery standard sono ospitati nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="3f318-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="3f318-191">[Visitare questa pagina](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sulla rete CDN Microsoft Ajax") per visualizzare le anteprime per ogni tema.</span><span class="sxs-lookup"><span data-stu-id="3f318-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="3f318-192">Per altre informazioni sulla libreria dell'interfaccia utente di jQuery, visitare ufficiale [sito Web dell'interfaccia utente di jQuery](http://jQueryUI.com "sito Web dell'interfaccia utente di jQuery").</span><span class="sxs-lookup"><span data-stu-id="3f318-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="3f318-193">File di terze parti sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="3f318-194">La rete CDN ospita alcune delle più diffuse librerie JavaScript di terze parti.</span><span class="sxs-lookup"><span data-stu-id="3f318-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="3f318-195">Microsoft non rivendica la proprietà di tutte le librerie di terze parti ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="3f318-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="3f318-196">I proprietari del copyright delle librerie sono licenze queste librerie all'utente.</span><span class="sxs-lookup"><span data-stu-id="3f318-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="3f318-197">Eventuali diritti che potrebbe essere necessario scaricare e usare tali librerie vengono concessi esclusivamente per i rispettivi proprietari del copyright.</span><span class="sxs-lookup"><span data-stu-id="3f318-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="3f318-198">Poiché questi non sono librerie Microsoft, Microsoft non offre alcuna garanzia né licenze di diritti di proprietà intellettuale (non incluso alcun diritto di brevetto implicito) per le librerie di terze parti ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="3f318-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="3f318-199">Versioni di jQuery per la rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="3f318-200">Le versioni seguenti di jQuery sono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="3f318-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="3f318-201">versione di jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="3f318-201">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="3f318-202">versione di jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="3f318-202">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="3f318-203">versione di jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="3f318-203">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="3f318-204">versione di jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="3f318-204">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="3f318-205">versione di jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="3f318-205">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="3f318-206">versione di jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="3f318-206">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="3f318-207">versione di jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="3f318-207">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="3f318-208">versione 2.2.3 di jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-208">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="3f318-209">versione di jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="3f318-209">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="3f318-210">versione di jQuery 2.2.1</span><span class="sxs-lookup"><span data-stu-id="3f318-210">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="3f318-211">versione di jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="3f318-211">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="3f318-212">versione di jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="3f318-212">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="3f318-213">versione di jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="3f318-213">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="3f318-214">versione di jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="3f318-214">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="3f318-215">versione di jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="3f318-215">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="3f318-216">versione di jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="3f318-216">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="3f318-217">versione di jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="3f318-217">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="3f318-218">versione di jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="3f318-218">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="3f318-219">versione di jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="3f318-219">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="3f318-220">versione di jQuery 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3f318-220">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="3f318-221">versione di jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="3f318-221">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="3f318-222">versione di jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="3f318-222">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="3f318-223">versione di jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="3f318-223">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="3f318-224">versione di jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="3f318-224">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="3f318-225">versione di jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="3f318-225">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="3f318-226">versione di jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="3f318-226">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="3f318-227">versione di jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="3f318-227">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="3f318-228">versione di jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="3f318-228">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="3f318-229">versione di jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="3f318-229">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="3f318-230">versione di jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="3f318-230">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="3f318-231">versione di jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="3f318-231">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="3f318-232">versione di jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="3f318-232">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="3f318-233">versione di jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="3f318-233">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="3f318-234">versione di jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="3f318-234">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="3f318-235">versione di jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="3f318-235">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="3f318-236">versione di jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="3f318-236">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="3f318-237">versione di jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="3f318-237">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="3f318-238">versione di jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="3f318-238">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="3f318-239">versione di jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="3f318-239">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="3f318-240">versione di jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="3f318-240">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="3f318-241">versione di jQuery 1.7</span><span class="sxs-lookup"><span data-stu-id="3f318-241">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="3f318-242">versione di jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="3f318-242">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="3f318-243">versione di jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="3f318-243">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="3f318-244">versione di jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="3f318-244">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="3f318-245">versione di jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="3f318-245">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="3f318-246">versione di jQuery 1.6</span><span class="sxs-lookup"><span data-stu-id="3f318-246">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="3f318-247">versione di jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="3f318-247">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="3f318-248">versione di jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="3f318-248">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="3f318-249">versione di jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="3f318-249">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="3f318-250">versione di jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="3f318-250">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="3f318-251">versione di jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="3f318-251">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="3f318-252">versione di jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="3f318-252">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="3f318-253">versione di jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="3f318-253">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="3f318-254">versione di jQuery 1.4</span><span class="sxs-lookup"><span data-stu-id="3f318-254">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="3f318-255">versione di jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="3f318-255">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="3f318-256">Versioni di eseguire la migrazione di jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-256">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="3f318-257">Le versioni seguenti di jQuery Migrate sono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="3f318-257">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="3f318-258">jQuery Migrate versione 3.0.0</span><span class="sxs-lookup"><span data-stu-id="3f318-258">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="3f318-259">jQuery Migrate versione 1.2.1</span><span class="sxs-lookup"><span data-stu-id="3f318-259">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="3f318-260">jQuery Migrate versione 1.2.0</span><span class="sxs-lookup"><span data-stu-id="3f318-260">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="3f318-261">eseguire la migrazione versione 1.1.1 jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-261">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="3f318-262">jQuery Migrate versione 1.1.0</span><span class="sxs-lookup"><span data-stu-id="3f318-262">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="3f318-263">jQuery Migrate versione 1.0.0</span><span class="sxs-lookup"><span data-stu-id="3f318-263">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="3f318-264">Versioni dell'interfaccia utente per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-264">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="3f318-265">Le versioni seguenti della libreria dell'interfaccia utente di jQuery sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="3f318-265">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="3f318-266">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="3f318-266">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="3f318-267">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="3f318-267">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-268">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="3f318-268">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-269">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="3f318-269">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-270">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="3f318-270">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-271">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="3f318-271">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-272">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="3f318-272">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-273">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="3f318-273">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-274">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="3f318-274">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-275">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="3f318-275">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-276">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="3f318-276">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-277">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="3f318-277">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-278">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="3f318-278">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-279">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="3f318-279">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-280">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="3f318-280">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-281">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="3f318-281">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-282">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="3f318-282">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-283">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="3f318-283">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-284">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="3f318-284">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-285">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="3f318-285">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-286">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="3f318-286">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-287">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="3f318-287">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-288">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="3f318-288">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-289">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="3f318-289">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-290">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="3f318-290">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-291">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="3f318-291">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-292">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="3f318-292">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-293">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="3f318-293">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-294">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="3f318-294">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-295">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="3f318-295">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-296">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="3f318-296">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-297">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="3f318-297">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-298">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="3f318-298">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-299">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="3f318-299">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-300">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="3f318-300">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-301">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="3f318-301">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="3f318-302">Versioni di convalida per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-302">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="3f318-303">Le versioni seguenti della libreria di convalida di jQuery sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="3f318-303">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="3f318-304">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="3f318-304">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="3f318-305">jQuery Validate 1.17.0</span><span class="sxs-lookup"><span data-stu-id="3f318-305">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery Validation 1.17.0")
- [<span data-ttu-id="3f318-306">jQuery. Validate versione 1.16.0</span><span class="sxs-lookup"><span data-stu-id="3f318-306">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "convalida di jQuery 1.16.0")
- [<span data-ttu-id="3f318-307">jQuery Validate 1.15.1</span><span class="sxs-lookup"><span data-stu-id="3f318-307">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "convalida di jQuery 1.15.1")
- [<span data-ttu-id="3f318-308">jQuery Validate 1.15.0</span><span class="sxs-lookup"><span data-stu-id="3f318-308">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "convalida di jQuery 1.15.0")
- [<span data-ttu-id="3f318-309">jQuery Validate 1.14.0</span><span class="sxs-lookup"><span data-stu-id="3f318-309">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "convalida di jQuery 1.14.0")
- [<span data-ttu-id="3f318-310">jQuery Validate 1.13.1</span><span class="sxs-lookup"><span data-stu-id="3f318-310">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "convalida di jQuery 1.13.1")
- [<span data-ttu-id="3f318-311">jQuery Validate 1.13.0</span><span class="sxs-lookup"><span data-stu-id="3f318-311">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "convalida di jQuery 1.13.0")
- [<span data-ttu-id="3f318-312">jQuery Validate 1.12.0</span><span class="sxs-lookup"><span data-stu-id="3f318-312">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "convalida di jQuery 1.12.0")
- [<span data-ttu-id="3f318-313">jQuery Validate 1.11.1</span><span class="sxs-lookup"><span data-stu-id="3f318-313">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "convalida di jQuery 1.11.1")
- [<span data-ttu-id="3f318-314">jQuery Validate 1.11.0</span><span class="sxs-lookup"><span data-stu-id="3f318-314">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "convalida di jQuery 1.11.0")
- [<span data-ttu-id="3f318-315">jQuery Validate 1.10.0</span><span class="sxs-lookup"><span data-stu-id="3f318-315">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "convalida di jQuery 1.10.0")
- [<span data-ttu-id="3f318-316">jQuery Validate 1.9</span><span class="sxs-lookup"><span data-stu-id="3f318-316">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jQuery. Validate versione 1.9")
- [<span data-ttu-id="3f318-317">jQuery Validate 1.8.1</span><span class="sxs-lookup"><span data-stu-id="3f318-317">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jQuery. Validate versione 1.8.1")
- [<span data-ttu-id="3f318-318">jQuery Validate 1.8</span><span class="sxs-lookup"><span data-stu-id="3f318-318">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jQuery. Validate versione 1.8")
- [<span data-ttu-id="3f318-319">jQuery Validate 1.7</span><span class="sxs-lookup"><span data-stu-id="3f318-319">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jQuery. Validate versione 1.7")
- [<span data-ttu-id="3f318-320">Convalida di jQuery 1.6</span><span class="sxs-lookup"><span data-stu-id="3f318-320">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "convalida di jQuery 1.6")
- [<span data-ttu-id="3f318-321">Convalida di jQuery 1.5.5</span><span class="sxs-lookup"><span data-stu-id="3f318-321">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "convalida di jQuery 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="3f318-322">jQuery Mobile versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-322">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="3f318-323">Le versioni seguenti di libreria jQuery per dispositivi mobili sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="3f318-323">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="3f318-324">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="3f318-324">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="3f318-325">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="3f318-325">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-326">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="3f318-326">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-327">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="3f318-327">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-328">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="3f318-328">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-329">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="3f318-329">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-330">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="3f318-330">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-331">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="3f318-331">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-332">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="3f318-332">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-333">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="3f318-333">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-334">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="3f318-334">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-335">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="3f318-335">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-336">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="3f318-336">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-337">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="3f318-337">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-338">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="3f318-338">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-339">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="3f318-339">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-340">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="3f318-340">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="3f318-341">Nella versione beta di jQuery Mobile 1.0 3</span><span class="sxs-lookup"><span data-stu-id="3f318-341">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 sulla rete CDN Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="3f318-342">Versioni di modelli per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-342">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="3f318-343">Le versioni seguenti del plug-in modelli di jQuery sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="3f318-343">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="3f318-344">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="3f318-344">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="3f318-345">jQuery Beta 1 di modelli</span><span class="sxs-lookup"><span data-stu-id="3f318-345">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery Beta 1 di modelli")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="3f318-346">Ciclo di versioni per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="3f318-346">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="3f318-347">Le versioni seguenti del plug-in del ciclo di jQuery sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="3f318-347">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="3f318-348">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="3f318-348">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="3f318-349">jQuery ciclo 2,99</span><span class="sxs-lookup"><span data-stu-id="3f318-349">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 ciclo")
- [<span data-ttu-id="3f318-350">jQuery ciclo 2.94</span><span class="sxs-lookup"><span data-stu-id="3f318-350">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 ciclo")
- [<span data-ttu-id="3f318-351">jQuery ciclo 2,88</span><span class="sxs-lookup"><span data-stu-id="3f318-351">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 ciclo")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="3f318-352">jQuery DataTables versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-352">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="3f318-353">Le versioni seguenti del plug-in di jQuery DataTables sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="3f318-353">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="3f318-354">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="3f318-354">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="3f318-355">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="3f318-355">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="3f318-356">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="3f318-356">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery 1.10.4 DataTables")
- [<span data-ttu-id="3f318-357">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="3f318-357">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="3f318-358">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="3f318-358">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="3f318-359">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="3f318-359">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="3f318-360">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="3f318-360">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery 1.9.1 DataTables")
- [<span data-ttu-id="3f318-361">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="3f318-361">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery 1.9.0 DataTables")
- [<span data-ttu-id="3f318-362">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="3f318-362">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="3f318-363">Versioni di Modernizr per la rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-363">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="3f318-364">Le seguenti versioni di [Modernizr](http://www.modernizr.com "Modernizr") sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="3f318-364">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="3f318-365">Versioni JSHint sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-365">JSHint Releases on the CDN</span></span>

<span data-ttu-id="3f318-366">Le seguenti versioni di [JSHint](http://www.jshint.com "JSHint") sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="3f318-366">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="3f318-367">Versioni Knockout sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-367">Knockout Releases on the CDN</span></span>

<span data-ttu-id="3f318-368">Le seguenti versioni di [Knockout](http://www.knockoutjs.com "Knockout") sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="3f318-368">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="3f318-369">Globalizzare versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-369">Globalize Releases on the CDN</span></span>

<span data-ttu-id="3f318-370">Le seguenti versioni di [Globalize](https://github.com/jquery/globalize "Globalize") sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="3f318-370">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="3f318-371">Globalizzare versione 1.0.0</span><span class="sxs-lookup"><span data-stu-id="3f318-371">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="3f318-372">Globalizzare versione 0.1.1</span><span class="sxs-lookup"><span data-stu-id="3f318-372">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="3f318-373">tutte le impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="3f318-373">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="3f318-374">Sostituire "{codice delle impostazioni cultura}" con il codice della lingua desiderata, ad esempio Microsoft globalize.culture.en GB.js== file sulla rete CDN = = queste librerie sono state caricate da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3f318-374">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="3f318-375">Rispondere versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-375">Respond Releases on the CDN</span></span>

<span data-ttu-id="3f318-376">Le seguenti versioni di [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") rispondono sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="3f318-376">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="3f318-377">Rispondere versione 1.4.2</span><span class="sxs-lookup"><span data-stu-id="3f318-377">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="3f318-378">Rispondere versione 1.4.1</span><span class="sxs-lookup"><span data-stu-id="3f318-378">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="3f318-379">Rispondere versione 1.4.0</span><span class="sxs-lookup"><span data-stu-id="3f318-379">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="3f318-380">La versione 1.3.0 rispondere</span><span class="sxs-lookup"><span data-stu-id="3f318-380">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="3f318-381">Rispondere versione 1.2.0</span><span class="sxs-lookup"><span data-stu-id="3f318-381">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="3f318-382">Versioni bootstrap sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-382">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="3f318-383">Le seguenti versioni di [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="3f318-383">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-411"></a><span data-ttu-id="3f318-384">Versione bootstrap 4.1.1</span><span class="sxs-lookup"><span data-stu-id="3f318-384">Bootstrap version 4.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a><span data-ttu-id="3f318-385">Eseguire il bootstrap versione 4.0.0</span><span class="sxs-lookup"><span data-stu-id="3f318-385">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-337"></a><span data-ttu-id="3f318-386">Versione bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="3f318-386">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="3f318-387">Versione bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="3f318-387">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="3f318-388">Eseguire il bootstrap versione 3.3.5</span><span class="sxs-lookup"><span data-stu-id="3f318-388">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="3f318-389">Versione bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="3f318-389">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="3f318-390">Versione bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="3f318-390">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="3f318-391">Eseguire il bootstrap versione 3.3.1</span><span class="sxs-lookup"><span data-stu-id="3f318-391">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="3f318-392">Eseguire il bootstrap versione 3.3.0</span><span class="sxs-lookup"><span data-stu-id="3f318-392">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="3f318-393">Eseguire il bootstrap versione 3.2.0</span><span class="sxs-lookup"><span data-stu-id="3f318-393">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="3f318-394">Versione bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="3f318-394">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="3f318-395">Eseguire il bootstrap versione 3.1.0</span><span class="sxs-lookup"><span data-stu-id="3f318-395">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="3f318-396">Versione bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="3f318-396">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="3f318-397">Versione bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="3f318-397">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="3f318-398">Eseguire il bootstrap versione 3.0.1</span><span class="sxs-lookup"><span data-stu-id="3f318-398">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="3f318-399">Eseguire il bootstrap della versione 3.0.0</span><span class="sxs-lookup"><span data-stu-id="3f318-399">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="3f318-400">Eseguire il bootstrap versione 2.3.2</span><span class="sxs-lookup"><span data-stu-id="3f318-400">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="3f318-401">Eseguire il bootstrap versione 2.3.1</span><span class="sxs-lookup"><span data-stu-id="3f318-401">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="3f318-402">Versioni TouchCarousel bootstrap sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-402">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="3f318-403">Le seguenti versioni di [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel rilasci sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="3f318-403">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="3f318-404">Bootstrap TouchCarousel versione 0.8.0</span><span class="sxs-lookup"><span data-stu-id="3f318-404">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="3f318-405">Versioni Hammer.js sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-405">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="3f318-406">Le seguenti versioni di [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js rilasci sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="3f318-406">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="3f318-407">Hammer.js versione 2.0.4</span><span class="sxs-lookup"><span data-stu-id="3f318-407">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="3f318-408">Web Form ASP.NET e Ajax versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-408">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="3f318-409">Le versioni seguenti di libreria ASP.NET Ajax sono ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="3f318-409">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="3f318-410">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="3f318-410">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="3f318-411">Versione di Web Form ASP.NET e Ajax 4.5.2</span><span class="sxs-lookup"><span data-stu-id="3f318-411">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Form ASP.NET e Ajax 4.5.2")
- [<span data-ttu-id="3f318-412">Versione di Web Form ASP.NET e Ajax 4</span><span class="sxs-lookup"><span data-stu-id="3f318-412">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Form ASP.NET e Ajax 4")
- [<span data-ttu-id="3f318-413">ASP.NET Ajax versione 3.5</span><span class="sxs-lookup"><span data-stu-id="3f318-413">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax di ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="3f318-414">ASP.NET MVC rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-414">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="3f318-415">I seguenti file JavaScript di MVC ASP.NET sono ospitati nella rete CDN in questo:</span><span class="sxs-lookup"><span data-stu-id="3f318-415">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="3f318-416">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="3f318-416">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="3f318-417">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="3f318-417">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="3f318-418">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="3f318-418">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="3f318-419">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="3f318-419">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="3f318-420">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="3f318-420">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.min.js 
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.js 
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="3f318-421">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="3f318-421">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="3f318-422">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="3f318-422">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="3f318-423">ASP.NET SignalR rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="3f318-423">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="3f318-424">I seguenti file ASP.NET SignalR JavaScript sono ospitati nella rete CDN in questo:</span><span class="sxs-lookup"><span data-stu-id="3f318-424">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="3f318-425">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="3f318-425">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="3f318-426">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="3f318-426">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="3f318-427">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="3f318-427">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="3f318-428">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="3f318-428">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="3f318-429">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="3f318-429">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="3f318-430">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="3f318-430">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="3f318-431">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="3f318-431">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="3f318-432">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3f318-432">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="3f318-433">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="3f318-433">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="3f318-434">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="3f318-434">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="3f318-435">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="3f318-435">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="3f318-436">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="3f318-436">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="3f318-437">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="3f318-437">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="3f318-438">Per informazioni sulle condizioni di utilizzo per la rete CDN, vedere [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span><span class="sxs-lookup"><span data-stu-id="3f318-438">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
