---
uid: ajax/cdn/overview
title: Rete CDN Microsoft Ajax | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 65eee9bc477fc8adf10e8d819b93375ffbb72d7b
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341839"
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="787ab-102">Rete CDN Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="787ab-102">Microsoft Ajax Content Delivery Network</span></span>
====================
> [!WARNING]
> <span data-ttu-id="787ab-103">Non è stato necessario considerare una dipendenza rigida in asset della rete CDN con le applicazioni di produzione.</span><span class="sxs-lookup"><span data-stu-id="787ab-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="787ab-104">Le applicazioni devono di test per l'asset della rete CDN fare riferimento e usare un asset di fallback quando non è disponibile la rete CDN.</span><span class="sxs-lookup"><span data-stu-id="787ab-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span> 
>
> <span data-ttu-id="787ab-105">Rete CDN Microsoft Ajax non dispone di alcun contratto di servizio in aggiunta a una rete CDN di Azure.</span><span class="sxs-lookup"><span data-stu-id="787ab-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="787ab-106">Uso [questo problema su GitHub](https://github.com/aspnet/Docs/issues/5832) per segnalare i problemi con la rete CDN Microsoft Ajax.</span><span class="sxs-lookup"><span data-stu-id="787ab-106">Use [this GitHub issue](https://github.com/aspnet/Docs/issues/5832) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="787ab-107">Sommario</span><span class="sxs-lookup"><span data-stu-id="787ab-107">Table of Contents</span></span>

<span data-ttu-id="787ab-108">**[AJAX.microsoft.com rinominata ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="787ab-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="787ab-109">**[Supporto di Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="787ab-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="787ab-110">**[Utilizzo di ASP.NET Ajax dalla rete CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="787ab-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="787ab-111">**[Uso di jQuery dalla rete CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="787ab-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="787ab-112">**[Usando l'interfaccia utente dalla rete CDN di jQuery](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="787ab-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="787ab-113">**[File di terze parti sulla rete CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="787ab-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="787ab-114">Versioni di jQuery per la rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="787ab-115">Versioni di eseguire la migrazione di jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="787ab-116">Versioni dell'interfaccia utente per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="787ab-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="787ab-117">Versioni di convalida per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="787ab-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="787ab-118">jQuery Mobile versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="787ab-119">Versioni di modelli per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="787ab-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="787ab-120">Ciclo di versioni per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="787ab-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="787ab-121">jQuery DataTables versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="787ab-122">Versioni di Modernizr per la rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="787ab-123">Versioni JSHint sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="787ab-124">Versioni Knockout sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="787ab-125">Globalizzare versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="787ab-126">Rispondere versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="787ab-127">Versioni bootstrap sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="787ab-128">Versioni TouchCarousel bootstrap sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="787ab-129">Versioni Hammer.js sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="787ab-130">Web Form ASP.NET e Ajax versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="787ab-131">ASP.NET MVC rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="787ab-132">ASP.NET SignalR rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="787ab-133">Il Content Delivery Network (rete CDN Microsoft Ajax) ospita le librerie JavaScript di terze parti più diffusi, ad esempio jQuery e consente di aggiungere facilmente alle applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="787ab-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="787ab-134">Ad esempio, è possibile iniziare a usare jQuery ospitato nella rete CDN in questo semplicemente aggiungendo un &lt;script&gt; tag a una pagina che punta a ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="787ab-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="787ab-135">Grazie all'uso della rete CDN, è possibile migliorare significativamente le prestazioni delle applicazioni Ajax.</span><span class="sxs-lookup"><span data-stu-id="787ab-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="787ab-136">Il contenuto della rete CDN viene memorizzati nella cache nei server di tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="787ab-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="787ab-137">Inoltre, la rete CDN consente ai browser di riutilizzare i file JavaScript di terze parti memorizzata nella cache per i siti web che si trovano in domini diversi.</span><span class="sxs-lookup"><span data-stu-id="787ab-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="787ab-138">La rete CDN supporta SSL (HTTPS) nel caso in cui è necessario presentare una pagina web utilizzando Secure Sockets Layer.</span><span class="sxs-lookup"><span data-stu-id="787ab-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="787ab-139">La rete CDN ospita le seguenti librerie di script di terze parti che sono state caricate e sono concessi in licenza all'utente, dai proprietari di queste librerie:</span><span class="sxs-lookup"><span data-stu-id="787ab-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="787ab-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="787ab-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="787ab-141">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="787ab-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="787ab-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="787ab-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="787ab-143">jQuery Validation (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="787ab-143">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="787ab-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="787ab-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="787ab-145">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="787ab-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="787ab-146">Rete CDN Microsoft Ajax include anche le librerie seguenti che sono state caricate da Microsoft:</span><span class="sxs-lookup"><span data-stu-id="787ab-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="787ab-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="787ab-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="787ab-148">File JavaScript di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="787ab-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="787ab-149">File ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="787ab-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="787ab-150">Microsoft non rivendica la proprietà di tutte le librerie di terze parti ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="787ab-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="787ab-151">I proprietari del copyright delle librerie sono licenze queste librerie all'utente.</span><span class="sxs-lookup"><span data-stu-id="787ab-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="787ab-152">Eventuali diritti che potrebbe essere necessario scaricare e usare tali librerie vengono concessi esclusivamente per i rispettivi proprietari del copyright.</span><span class="sxs-lookup"><span data-stu-id="787ab-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="787ab-153">Poiché questi non sono librerie Microsoft, Microsoft non offre alcuna garanzia né licenze di diritti di proprietà intellettuale (non incluso alcun diritto di brevetto implicito) per le librerie di terze parti ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="787ab-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="787ab-154">Se si vuole inviare la libreria JavaScript e la libreria è una delle librerie JavaScript superiore (come indicato nella http://trends.builtwith.com) o le estensioni plug-in o da queste librerie sono (a) più diffusi; o (b) utili per l'utilizzo in ASP.NET quindi contattare AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="787ab-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="787ab-155">AJAX.microsoft.com rinominata ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="787ab-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="787ab-156">La rete CDN consente di usare il nome di dominio microsoft.com ed è stata modificata per usare il nome di dominio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="787ab-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="787ab-157">Questa modifica è stata apportata per migliorare le prestazioni poiché quando un browser viene fatto riferimento il dominio microsoft.com lo invierà tutti i cookie da quel dominio attraverso la rete con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="787ab-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="787ab-158">Rinominando con un nome di dominio diverso da microsoft.com prestazioni possono essere incrementata di gran parte al 25%.</span><span class="sxs-lookup"><span data-stu-id="787ab-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="787ab-159">Si noti ajax.microsoft.com continueranno a funzionare, ma è consigliabile ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="787ab-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="787ab-160">Vecchio formato: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="787ab-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="787ab-161">Nuovo formato: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="787ab-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="787ab-162">Supporto di Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="787ab-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="787ab-163">Per usare correttamente i file .vsdoc con Visual Studio 2008, è necessario assicurarsi che sia installato Visual Studio 2008 SP1 installato e installato l'hotfix per i file di vsdoc.</span><span class="sxs-lookup"><span data-stu-id="787ab-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="787ab-164">È possibile ottenerli da qui:</span><span class="sxs-lookup"><span data-stu-id="787ab-164">You can get these from here:</span></span>

- [<span data-ttu-id="787ab-165">Download Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="787ab-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Download Visual Studio 2008 SP1")
- [<span data-ttu-id="787ab-166">Download dell'hotfix .vsdoc per Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="787ab-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "scaricare .vsdoc hotfix per Visual Studio 2008 SP1")

<span data-ttu-id="787ab-167">Visual Studio 2010 supporta file .vsdoc senza tutte le patch aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="787ab-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="787ab-168">Utilizzo di ASP.NET Ajax dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="787ab-169">Quando si usa ASP.NET 4, è possibile reindirizzare tutte le richieste per gli script del framework ASP.NET per la rete CDN.</span><span class="sxs-lookup"><span data-stu-id="787ab-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="787ab-170">Il recupero degli script dalla rete CDN anziché il server web locale può migliorare notevolmente le prestazioni di siti Web pubblici di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="787ab-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="787ab-171">Usare la proprietà di ScriptManager EnableCDN per reindirizzare tutte le richieste di script framework ASP.NET per la rete CDN Microsoft Ajax:</span><span class="sxs-lookup"><span data-stu-id="787ab-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="787ab-172">Uso di jQuery dalla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="787ab-173">È possibile usare gli script jQuery ospitati nella rete CDN nell'applicazione Web aggiungendo il seguente elemento script a una pagina:</span><span class="sxs-lookup"><span data-stu-id="787ab-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="787ab-174">La rete CDN include anche la versione minimizzata dello script jQuery, che è possibile ottenere usando l'elemento seguente:</span><span class="sxs-lookup"><span data-stu-id="787ab-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="787ab-175">Per consentire alla pagina di fallback per il caricamento di jQuery da un percorso locale sul proprio sito Web in caso di indisponibilità della rete CDN, aggiungere l'elemento seguente immediatamente dopo l'elemento che fa riferimento la rete CDN:</span><span class="sxs-lookup"><span data-stu-id="787ab-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="787ab-176">La pagina di esempio seguente usa la versione della rete CDN di jQuery libreria (con il fallback per una copia locale) per visualizzare il contenuto di un elemento div quando viene selezionato un pulsante.</span><span class="sxs-lookup"><span data-stu-id="787ab-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="787ab-177">È possibile approfondire la conoscenza di jQuery e scaricare una copia locale di jQuery, visitare il [jQuery](http://jquery.com/) sito Web.</span><span class="sxs-lookup"><span data-stu-id="787ab-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="787ab-178">Usando l'interfaccia utente dalla rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="787ab-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="787ab-179">La rete CDN ospita anche la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="787ab-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="787ab-180">La libreria dell'interfaccia utente di jQuery include un set completo di widget e gli effetti che è possibile usare nelle applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="787ab-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="787ab-181">Ad esempio, la pagina seguente viene illustrato come è possibile usare il componente aggiuntivo jQuery UI Datepicker nel contesto di un'applicazione Web Form ASP.NET per visualizzare un calendario popup:</span><span class="sxs-lookup"><span data-stu-id="787ab-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="787ab-182">Quando si sposta lo stato attivo alla casella di testo usando la tastiera, viene visualizzato un calendario:</span><span class="sxs-lookup"><span data-stu-id="787ab-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendario popup per la creazione con selezione data](overview/_static/image1.png)

<span data-ttu-id="787ab-184">Si noti che è necessario includere tre file dalla rete CDN nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="787ab-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="787ab-185">La libreria jQuery &mdash; la libreria dell'interfaccia utente di jQuery dipende dalla libreria jQuery.</span><span class="sxs-lookup"><span data-stu-id="787ab-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="787ab-186">È necessario aggiungere la libreria jQuery per la pagina prima di aggiungere la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="787ab-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="787ab-187">La libreria dell'interfaccia utente di jQuery &mdash; la libreria dell'interfaccia utente di jQuery contiene tutti gli effetti dell'interfaccia utente di jQuery e widget, ad esempio il widget Datepicker usato nella pagina precedente.</span><span class="sxs-lookup"><span data-stu-id="787ab-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="787ab-188">Un tema dell'interfaccia utente di jQuery &mdash; l'interfaccia utente di jQuery supporta diversi temi disponibili.</span><span class="sxs-lookup"><span data-stu-id="787ab-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="787ab-189">La pagina precedente include un collegamento a un file CSS per importare il tema di Redmond.</span><span class="sxs-lookup"><span data-stu-id="787ab-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="787ab-190">Tutti i temi di interfaccia utente di jQuery standard sono ospitati nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="787ab-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="787ab-191">[Visitare questa pagina](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sulla rete CDN Microsoft Ajax") per visualizzare le anteprime per ogni tema.</span><span class="sxs-lookup"><span data-stu-id="787ab-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="787ab-192">Per altre informazioni sulla libreria dell'interfaccia utente di jQuery, visitare ufficiale [sito Web dell'interfaccia utente di jQuery](http://jQueryUI.com "sito Web dell'interfaccia utente di jQuery").</span><span class="sxs-lookup"><span data-stu-id="787ab-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="787ab-193">File di terze parti sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="787ab-194">La rete CDN ospita alcune delle più diffuse librerie JavaScript di terze parti.</span><span class="sxs-lookup"><span data-stu-id="787ab-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="787ab-195">Microsoft non rivendica la proprietà di tutte le librerie di terze parti ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="787ab-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="787ab-196">I proprietari del copyright delle librerie sono licenze queste librerie all'utente.</span><span class="sxs-lookup"><span data-stu-id="787ab-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="787ab-197">Eventuali diritti che potrebbe essere necessario scaricare e usare tali librerie vengono concessi esclusivamente per i rispettivi proprietari del copyright.</span><span class="sxs-lookup"><span data-stu-id="787ab-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="787ab-198">Poiché questi non sono librerie Microsoft, Microsoft non offre alcuna garanzia né licenze di diritti di proprietà intellettuale (non incluso alcun diritto di brevetto implicito) per le librerie di terze parti ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="787ab-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="787ab-199">Versioni di jQuery per la rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="787ab-200">Le versioni seguenti di jQuery sono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="787ab-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="787ab-201">jQuery version 3.3.1</span><span class="sxs-lookup"><span data-stu-id="787ab-201">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="787ab-202">jQuery version 3.2.1</span><span class="sxs-lookup"><span data-stu-id="787ab-202">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="787ab-203">jQuery version 3.2.0</span><span class="sxs-lookup"><span data-stu-id="787ab-203">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="787ab-204">jQuery version 3.1.1</span><span class="sxs-lookup"><span data-stu-id="787ab-204">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="787ab-205">jQuery version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="787ab-205">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="787ab-206">jQuery version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="787ab-206">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="787ab-207">versione di jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="787ab-207">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="787ab-208">versione 2.2.3 di jQuery</span><span class="sxs-lookup"><span data-stu-id="787ab-208">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="787ab-209">versione di jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="787ab-209">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="787ab-210">versione di jQuery 2.2.1</span><span class="sxs-lookup"><span data-stu-id="787ab-210">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="787ab-211">jQuery version 2.2.0</span><span class="sxs-lookup"><span data-stu-id="787ab-211">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="787ab-212">versione di jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="787ab-212">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="787ab-213">versione di jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="787ab-213">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="787ab-214">versione di jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="787ab-214">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="787ab-215">versione di jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="787ab-215">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="787ab-216">jQuery version 2.1.0</span><span class="sxs-lookup"><span data-stu-id="787ab-216">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="787ab-217">versione di jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="787ab-217">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="787ab-218">versione di jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="787ab-218">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="787ab-219">jQuery version 2.0.1</span><span class="sxs-lookup"><span data-stu-id="787ab-219">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="787ab-220">jQuery version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="787ab-220">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="787ab-221">versione di jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="787ab-221">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="787ab-222">jQuery version 1.12.3</span><span class="sxs-lookup"><span data-stu-id="787ab-222">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="787ab-223">jQuery version 1.12.2</span><span class="sxs-lookup"><span data-stu-id="787ab-223">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="787ab-224">jQuery version 1.12.1</span><span class="sxs-lookup"><span data-stu-id="787ab-224">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="787ab-225">jQuery version 1.12.0</span><span class="sxs-lookup"><span data-stu-id="787ab-225">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="787ab-226">jQuery version 1.11.3</span><span class="sxs-lookup"><span data-stu-id="787ab-226">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="787ab-227">jQuery version 1.11.2</span><span class="sxs-lookup"><span data-stu-id="787ab-227">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="787ab-228">jQuery version 1.11.1</span><span class="sxs-lookup"><span data-stu-id="787ab-228">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="787ab-229">jQuery version 1.11.0</span><span class="sxs-lookup"><span data-stu-id="787ab-229">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="787ab-230">versione di jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="787ab-230">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="787ab-231">versione di jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="787ab-231">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="787ab-232">jQuery version 1.10.0</span><span class="sxs-lookup"><span data-stu-id="787ab-232">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="787ab-233">jQuery version 1.9.1</span><span class="sxs-lookup"><span data-stu-id="787ab-233">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="787ab-234">jQuery version 1.9.0</span><span class="sxs-lookup"><span data-stu-id="787ab-234">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="787ab-235">jQuery version 1.8.3</span><span class="sxs-lookup"><span data-stu-id="787ab-235">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="787ab-236">jQuery version 1.8.2</span><span class="sxs-lookup"><span data-stu-id="787ab-236">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="787ab-237">jQuery version 1.8.1</span><span class="sxs-lookup"><span data-stu-id="787ab-237">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="787ab-238">jQuery version 1.8.0</span><span class="sxs-lookup"><span data-stu-id="787ab-238">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="787ab-239">jQuery version 1.7.2</span><span class="sxs-lookup"><span data-stu-id="787ab-239">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="787ab-240">jQuery version 1.7.1</span><span class="sxs-lookup"><span data-stu-id="787ab-240">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="787ab-241">versione di jQuery 1.7</span><span class="sxs-lookup"><span data-stu-id="787ab-241">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="787ab-242">jQuery version 1.6.4</span><span class="sxs-lookup"><span data-stu-id="787ab-242">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="787ab-243">jQuery version 1.6.3</span><span class="sxs-lookup"><span data-stu-id="787ab-243">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="787ab-244">versione di jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="787ab-244">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="787ab-245">jQuery version 1.6.1</span><span class="sxs-lookup"><span data-stu-id="787ab-245">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="787ab-246">versione di jQuery 1.6</span><span class="sxs-lookup"><span data-stu-id="787ab-246">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="787ab-247">jQuery version 1.5.2</span><span class="sxs-lookup"><span data-stu-id="787ab-247">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="787ab-248">jQuery version 1.5.1</span><span class="sxs-lookup"><span data-stu-id="787ab-248">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="787ab-249">jQuery version 1.5</span><span class="sxs-lookup"><span data-stu-id="787ab-249">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="787ab-250">jQuery version 1.4.4</span><span class="sxs-lookup"><span data-stu-id="787ab-250">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="787ab-251">jQuery version 1.4.3</span><span class="sxs-lookup"><span data-stu-id="787ab-251">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="787ab-252">jQuery version 1.4.2</span><span class="sxs-lookup"><span data-stu-id="787ab-252">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="787ab-253">jQuery version 1.4.1</span><span class="sxs-lookup"><span data-stu-id="787ab-253">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="787ab-254">versione di jQuery 1.4</span><span class="sxs-lookup"><span data-stu-id="787ab-254">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="787ab-255">jQuery version 1.3.2</span><span class="sxs-lookup"><span data-stu-id="787ab-255">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="787ab-256">Versioni di eseguire la migrazione di jQuery sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-256">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="787ab-257">Le versioni seguenti di jQuery Migrate sono ospitate nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="787ab-257">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="787ab-258">jQuery Migrate version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="787ab-258">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="787ab-259">jQuery Migrate version 1.2.1</span><span class="sxs-lookup"><span data-stu-id="787ab-259">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="787ab-260">jQuery Migrate version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="787ab-260">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="787ab-261">jQuery Migrate version 1.1.1</span><span class="sxs-lookup"><span data-stu-id="787ab-261">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="787ab-262">jQuery Migrate version 1.1.0</span><span class="sxs-lookup"><span data-stu-id="787ab-262">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="787ab-263">jQuery Migrate version 1.0.0</span><span class="sxs-lookup"><span data-stu-id="787ab-263">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="787ab-264">Versioni dell'interfaccia utente per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="787ab-264">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="787ab-265">Le versioni seguenti della libreria dell'interfaccia utente di jQuery sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="787ab-265">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="787ab-266">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="787ab-266">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="787ab-267">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="787ab-267">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-268">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="787ab-268">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-269">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="787ab-269">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-270">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="787ab-270">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-271">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="787ab-271">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-272">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="787ab-272">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-273">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="787ab-273">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-274">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="787ab-274">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-275">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="787ab-275">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-276">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="787ab-276">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-277">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="787ab-277">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-278">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="787ab-278">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-279">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="787ab-279">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-280">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="787ab-280">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-281">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="787ab-281">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-282">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="787ab-282">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-283">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="787ab-283">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-284">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="787ab-284">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-285">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="787ab-285">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-286">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="787ab-286">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-287">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="787ab-287">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-288">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="787ab-288">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-289">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="787ab-289">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-290">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="787ab-290">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-291">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="787ab-291">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-292">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="787ab-292">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-293">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="787ab-293">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-294">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="787ab-294">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-295">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="787ab-295">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-296">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="787ab-296">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-297">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="787ab-297">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-298">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="787ab-298">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-299">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="787ab-299">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-300">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="787ab-300">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-301">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="787ab-301">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="787ab-302">Versioni di convalida per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="787ab-302">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="787ab-303">Le versioni seguenti della libreria di convalida di jQuery sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="787ab-303">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="787ab-304">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="787ab-304">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="787ab-305">jQuery Validate 1.19.0</span><span class="sxs-lookup"><span data-stu-id="787ab-305">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "jQuery Validation 1.19.0")
- [<span data-ttu-id="787ab-306">jQuery Validate 1.17.0</span><span class="sxs-lookup"><span data-stu-id="787ab-306">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery Validation 1.17.0")
- [<span data-ttu-id="787ab-307">jQuery Validate 1.16.0</span><span class="sxs-lookup"><span data-stu-id="787ab-307">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="787ab-308">jQuery Validate 1.15.1</span><span class="sxs-lookup"><span data-stu-id="787ab-308">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validation 1.15.1")
- [<span data-ttu-id="787ab-309">jQuery Validate 1.15.0</span><span class="sxs-lookup"><span data-stu-id="787ab-309">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validation 1.15.0")
- [<span data-ttu-id="787ab-310">jQuery Validate 1.14.0</span><span class="sxs-lookup"><span data-stu-id="787ab-310">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validation 1.14.0")
- [<span data-ttu-id="787ab-311">jQuery Validate 1.13.1</span><span class="sxs-lookup"><span data-stu-id="787ab-311">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validation 1.13.1")
- [<span data-ttu-id="787ab-312">jQuery Validate 1.13.0</span><span class="sxs-lookup"><span data-stu-id="787ab-312">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery Validation 1.13.0")
- [<span data-ttu-id="787ab-313">jQuery Validate 1.12.0</span><span class="sxs-lookup"><span data-stu-id="787ab-313">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery Validation 1.12.0")
- [<span data-ttu-id="787ab-314">jQuery Validate 1.11.1</span><span class="sxs-lookup"><span data-stu-id="787ab-314">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery Validation 1.11.1")
- [<span data-ttu-id="787ab-315">jQuery Validate 1.11.0</span><span class="sxs-lookup"><span data-stu-id="787ab-315">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="787ab-316">jQuery Validate 1.10.0</span><span class="sxs-lookup"><span data-stu-id="787ab-316">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery Validation 1.10.0")
- [<span data-ttu-id="787ab-317">jQuery Validate 1.9</span><span class="sxs-lookup"><span data-stu-id="787ab-317">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate version 1.9")
- [<span data-ttu-id="787ab-318">jQuery Validate 1.8.1</span><span class="sxs-lookup"><span data-stu-id="787ab-318">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate version 1.8.1")
- [<span data-ttu-id="787ab-319">jQuery Validate 1.8</span><span class="sxs-lookup"><span data-stu-id="787ab-319">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate version 1.8")
- [<span data-ttu-id="787ab-320">jQuery Validate 1.7</span><span class="sxs-lookup"><span data-stu-id="787ab-320">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate version 1.7")
- [<span data-ttu-id="787ab-321">jQuery Validate 1.6</span><span class="sxs-lookup"><span data-stu-id="787ab-321">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery Validate 1.6")
- [<span data-ttu-id="787ab-322">jQuery Validate 1.5.5</span><span class="sxs-lookup"><span data-stu-id="787ab-322">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery Validate 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="787ab-323">jQuery Mobile versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-323">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="787ab-324">Le versioni seguenti di libreria jQuery per dispositivi mobili sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="787ab-324">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="787ab-325">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="787ab-325">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="787ab-326">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="787ab-326">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-327">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="787ab-327">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-328">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="787ab-328">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-329">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="787ab-329">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-330">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="787ab-330">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-331">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="787ab-331">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-332">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="787ab-332">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-333">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="787ab-333">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-334">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="787ab-334">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-335">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="787ab-335">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-336">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="787ab-336">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-337">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="787ab-337">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-338">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="787ab-338">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-339">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="787ab-339">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-340">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="787ab-340">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-341">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="787ab-341">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 sulla rete CDN Microsoft Ajax")
- [<span data-ttu-id="787ab-342">Nella versione beta di jQuery Mobile 1.0 3</span><span class="sxs-lookup"><span data-stu-id="787ab-342">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 sulla rete CDN Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="787ab-343">Versioni di modelli per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="787ab-343">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="787ab-344">Le versioni seguenti del plug-in modelli di jQuery sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="787ab-344">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="787ab-345">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="787ab-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="787ab-346">jQuery Templates Beta 1</span><span class="sxs-lookup"><span data-stu-id="787ab-346">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery Templates Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="787ab-347">Ciclo di versioni per la rete CDN di jQuery</span><span class="sxs-lookup"><span data-stu-id="787ab-347">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="787ab-348">Le versioni seguenti del plug-in del ciclo di jQuery sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="787ab-348">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="787ab-349">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="787ab-349">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="787ab-350">jQuery Cycle 2.99</span><span class="sxs-lookup"><span data-stu-id="787ab-350">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="787ab-351">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="787ab-351">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="787ab-352">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="787ab-352">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="787ab-353">jQuery DataTables versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-353">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="787ab-354">Le versioni seguenti del plug-in di jQuery DataTables sono ospitate nella rete CDN in questo.</span><span class="sxs-lookup"><span data-stu-id="787ab-354">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="787ab-355">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="787ab-355">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="787ab-356">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="787ab-356">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="787ab-357">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="787ab-357">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery 1.10.4 DataTables")
- [<span data-ttu-id="787ab-358">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="787ab-358">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="787ab-359">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="787ab-359">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="787ab-360">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="787ab-360">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="787ab-361">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="787ab-361">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="787ab-362">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="787ab-362">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="787ab-363">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="787ab-363">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="787ab-364">Versioni di Modernizr per la rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-364">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="787ab-365">Le seguenti versioni di [Modernizr](http://www.modernizr.com "Modernizr") sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="787ab-365">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="787ab-366">Versioni JSHint sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-366">JSHint Releases on the CDN</span></span>

<span data-ttu-id="787ab-367">Le seguenti versioni di [JSHint](http://www.jshint.com "JSHint") sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="787ab-367">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="787ab-368">Versioni Knockout sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-368">Knockout Releases on the CDN</span></span>

<span data-ttu-id="787ab-369">Le seguenti versioni di [Knockout](http://www.knockoutjs.com "Knockout") sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="787ab-369">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

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

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="787ab-370">Globalizzare versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-370">Globalize Releases on the CDN</span></span>

<span data-ttu-id="787ab-371">Le seguenti versioni di [Globalize](https://github.com/jquery/globalize "Globalize") sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="787ab-371">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="787ab-372">Globalizzare versione 1.0.0</span><span class="sxs-lookup"><span data-stu-id="787ab-372">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="787ab-373">Globalizzare versione 0.1.1</span><span class="sxs-lookup"><span data-stu-id="787ab-373">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="787ab-374">tutte le impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="787ab-374">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="787ab-375">Sostituire "{codice delle impostazioni cultura}" con il codice della lingua desiderata, ad esempio Microsoft globalize.culture.en GB.js== file sulla rete CDN = = queste librerie sono state caricate da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="787ab-375">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="787ab-376">Rispondere versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-376">Respond Releases on the CDN</span></span>

<span data-ttu-id="787ab-377">Le seguenti versioni di [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") rispondono sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="787ab-377">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="787ab-378">Rispondere versione 1.4.2</span><span class="sxs-lookup"><span data-stu-id="787ab-378">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="787ab-379">Rispondere versione 1.4.1</span><span class="sxs-lookup"><span data-stu-id="787ab-379">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="787ab-380">Rispondere versione 1.4.0</span><span class="sxs-lookup"><span data-stu-id="787ab-380">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="787ab-381">La versione 1.3.0 rispondere</span><span class="sxs-lookup"><span data-stu-id="787ab-381">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="787ab-382">Rispondere versione 1.2.0</span><span class="sxs-lookup"><span data-stu-id="787ab-382">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="787ab-383">Versioni bootstrap sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-383">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="787ab-384">Le seguenti versioni di [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="787ab-384">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-421"></a><span data-ttu-id="787ab-385">Versione bootstrap 4.2.1</span><span class="sxs-lookup"><span data-stu-id="787ab-385">Bootstrap version 4.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-411"></a><span data-ttu-id="787ab-386">Versione bootstrap 4.1.1</span><span class="sxs-lookup"><span data-stu-id="787ab-386">Bootstrap version 4.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a><span data-ttu-id="787ab-387">Eseguire il bootstrap versione 4.0.0</span><span class="sxs-lookup"><span data-stu-id="787ab-387">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-340"></a><span data-ttu-id="787ab-388">Eseguire il bootstrap versione 3.4.0</span><span class="sxs-lookup"><span data-stu-id="787ab-388">Bootstrap version 3.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="787ab-389">Versione bootstrap 3.3.7</span><span class="sxs-lookup"><span data-stu-id="787ab-389">Bootstrap version 3.3.7</span></span>

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

#### <a name="bootstrap-version-336"></a><span data-ttu-id="787ab-390">Versione bootstrap 3.3.6</span><span class="sxs-lookup"><span data-stu-id="787ab-390">Bootstrap version 3.3.6</span></span>

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

#### <a name="bootstrap-version-335"></a><span data-ttu-id="787ab-391">Eseguire il bootstrap versione 3.3.5</span><span class="sxs-lookup"><span data-stu-id="787ab-391">Bootstrap version 3.3.5</span></span>

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

#### <a name="bootstrap-version-334"></a><span data-ttu-id="787ab-392">Versione bootstrap 3.3.4</span><span class="sxs-lookup"><span data-stu-id="787ab-392">Bootstrap version 3.3.4</span></span>

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

#### <a name="bootstrap-version-332"></a><span data-ttu-id="787ab-393">Versione bootstrap 3.3.2</span><span class="sxs-lookup"><span data-stu-id="787ab-393">Bootstrap version 3.3.2</span></span>

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

#### <a name="bootstrap-version-331"></a><span data-ttu-id="787ab-394">Eseguire il bootstrap versione 3.3.1</span><span class="sxs-lookup"><span data-stu-id="787ab-394">Bootstrap version 3.3.1</span></span>

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

#### <a name="bootstrap-version-330"></a><span data-ttu-id="787ab-395">Eseguire il bootstrap versione 3.3.0</span><span class="sxs-lookup"><span data-stu-id="787ab-395">Bootstrap version 3.3.0</span></span>

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

#### <a name="bootstrap-version-320"></a><span data-ttu-id="787ab-396">Eseguire il bootstrap versione 3.2.0</span><span class="sxs-lookup"><span data-stu-id="787ab-396">Bootstrap version 3.2.0</span></span>

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

#### <a name="bootstrap-version-311"></a><span data-ttu-id="787ab-397">Versione bootstrap 3.1.1</span><span class="sxs-lookup"><span data-stu-id="787ab-397">Bootstrap version 3.1.1</span></span>

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

#### <a name="bootstrap-version-310"></a><span data-ttu-id="787ab-398">Eseguire il bootstrap versione 3.1.0</span><span class="sxs-lookup"><span data-stu-id="787ab-398">Bootstrap version 3.1.0</span></span>

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

#### <a name="bootstrap-version-303"></a><span data-ttu-id="787ab-399">Versione bootstrap 3.0.3</span><span class="sxs-lookup"><span data-stu-id="787ab-399">Bootstrap version 3.0.3</span></span>

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

#### <a name="bootstrap-version-302"></a><span data-ttu-id="787ab-400">Versione bootstrap 3.0.2</span><span class="sxs-lookup"><span data-stu-id="787ab-400">Bootstrap version 3.0.2</span></span>

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

#### <a name="bootstrap-version-301"></a><span data-ttu-id="787ab-401">Eseguire il bootstrap versione 3.0.1</span><span class="sxs-lookup"><span data-stu-id="787ab-401">Bootstrap version 3.0.1</span></span>

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

#### <a name="bootstrap-version-300"></a><span data-ttu-id="787ab-402">Eseguire il bootstrap della versione 3.0.0</span><span class="sxs-lookup"><span data-stu-id="787ab-402">Bootstrap version 3.0.0</span></span>

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

#### <a name="bootstrap-version-232"></a><span data-ttu-id="787ab-403">Eseguire il bootstrap versione 2.3.2</span><span class="sxs-lookup"><span data-stu-id="787ab-403">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="787ab-404">Eseguire il bootstrap versione 2.3.1</span><span class="sxs-lookup"><span data-stu-id="787ab-404">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="787ab-405">Versioni TouchCarousel bootstrap sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-405">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="787ab-406">Le seguenti versioni di [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel rilasci sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="787ab-406">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="787ab-407">Bootstrap TouchCarousel versione 0.8.0</span><span class="sxs-lookup"><span data-stu-id="787ab-407">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="787ab-408">Versioni Hammer.js sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-408">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="787ab-409">Le seguenti versioni di [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js rilasci sono ospitati nella rete CDN:</span><span class="sxs-lookup"><span data-stu-id="787ab-409">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="787ab-410">Hammer.js versione 2.0.4</span><span class="sxs-lookup"><span data-stu-id="787ab-410">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="787ab-411">Web Form ASP.NET e Ajax versioni sulla rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-411">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="787ab-412">Le versioni seguenti di libreria ASP.NET Ajax sono ospitate nella rete CDN.</span><span class="sxs-lookup"><span data-stu-id="787ab-412">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="787ab-413">Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.</span><span class="sxs-lookup"><span data-stu-id="787ab-413">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="787ab-414">Versione di Web Form ASP.NET e Ajax 4.5.2</span><span class="sxs-lookup"><span data-stu-id="787ab-414">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Form ASP.NET e Ajax 4.5.2")
- [<span data-ttu-id="787ab-415">Versione di Web Form ASP.NET e Ajax 4</span><span class="sxs-lookup"><span data-stu-id="787ab-415">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Form ASP.NET e Ajax 4")
- [<span data-ttu-id="787ab-416">ASP.NET Ajax versione 3.5</span><span class="sxs-lookup"><span data-stu-id="787ab-416">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax di ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="787ab-417">ASP.NET MVC rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-417">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="787ab-418">I seguenti file JavaScript di MVC ASP.NET sono ospitati nella rete CDN in questo:</span><span class="sxs-lookup"><span data-stu-id="787ab-418">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="787ab-419">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="787ab-419">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="787ab-420">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="787ab-420">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="787ab-421">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="787ab-421">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="787ab-422">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="787ab-422">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="787ab-423">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="787ab-423">ASP.NET MVC 3.0</span></span>

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

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="787ab-424">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="787ab-424">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="787ab-425">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="787ab-425">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="787ab-426">ASP.NET SignalR rilascia nella rete CDN</span><span class="sxs-lookup"><span data-stu-id="787ab-426">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="787ab-427">I seguenti file ASP.NET SignalR JavaScript sono ospitati nella rete CDN in questo:</span><span class="sxs-lookup"><span data-stu-id="787ab-427">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="787ab-428">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="787ab-428">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="787ab-429">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="787ab-429">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="787ab-430">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="787ab-430">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="787ab-431">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="787ab-431">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="787ab-432">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="787ab-432">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="787ab-433">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="787ab-433">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="787ab-434">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="787ab-434">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="787ab-435">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="787ab-435">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="787ab-436">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="787ab-436">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="787ab-437">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="787ab-437">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="787ab-438">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="787ab-438">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="787ab-439">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="787ab-439">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="787ab-440">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="787ab-440">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="787ab-441">Per informazioni sulle condizioni di utilizzo per la rete CDN, vedere [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span><span class="sxs-lookup"><span data-stu-id="787ab-441">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
