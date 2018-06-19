---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Che cosa è necessario eseguire in ASP.NET e quali operazioni eseguire invece | Documenti Microsoft
author: tfitzmac
description: In questo argomento vengono descritti i diversi errori comuni, assicurarsi di persone all'interno dei progetti web ASP.NET. Fornisce indicazioni per le operazioni da effettuare per evitare questi commo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 829f3a024bc15bec8b60b91193ba9bca37b78009
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034920"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="293b4-104">Che cosa è necessario eseguire in ASP.NET e quali operazioni eseguire invece</span><span class="sxs-lookup"><span data-stu-id="293b4-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="293b4-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="293b4-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="293b4-106">In questo argomento vengono descritti i diversi errori comuni, assicurarsi di persone all'interno dei progetti web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="293b4-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="293b4-107">Fornisce indicazioni per le operazioni da effettuare per evitare questi errori comuni.</span><span class="sxs-lookup"><span data-stu-id="293b4-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="293b4-108">È basato su un [presentazione](http://vimeo.com/68390507) da **Damian Edwards** norvegese conferenza di sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="293b4-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="293b4-109">Dichiarazione di non responsabilità</span><span class="sxs-lookup"><span data-stu-id="293b4-109">Disclaimer</span></span>

<span data-ttu-id="293b4-110">In questo argomento non è come una guida completa per garantire che l'applicazione sia sicuro ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="293b4-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="293b4-111">È comunque necessario seguire le procedure consigliate per la sicurezza e delle prestazioni che non sono descritte in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="293b4-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="293b4-112">Si consiglia solo come evitare errori comuni relativi a processi e le classi .NET.</span><span class="sxs-lookup"><span data-stu-id="293b4-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="293b4-113">Panoramica</span><span class="sxs-lookup"><span data-stu-id="293b4-113">Overview</span></span>

<span data-ttu-id="293b4-114">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="293b4-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="293b4-115">Conformità agli standard</span><span class="sxs-lookup"><span data-stu-id="293b4-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="293b4-116">Adattatori di controllo</span><span class="sxs-lookup"><span data-stu-id="293b4-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="293b4-117">Proprietà di stile per controlli</span><span class="sxs-lookup"><span data-stu-id="293b4-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="293b4-118">Pagina e i callback di controllo</span><span class="sxs-lookup"><span data-stu-id="293b4-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="293b4-119">Rilevamento di funzionalità del browser</span><span class="sxs-lookup"><span data-stu-id="293b4-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="293b4-120">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="293b4-120">Security</span></span>](#security)

    - [<span data-ttu-id="293b4-121">Convalida della richiesta</span><span class="sxs-lookup"><span data-stu-id="293b4-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="293b4-122">Sessione e l'autenticazione basata su form senza cookie</span><span class="sxs-lookup"><span data-stu-id="293b4-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="293b4-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="293b4-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="293b4-124">Attendibilità media</span><span class="sxs-lookup"><span data-stu-id="293b4-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="293b4-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="293b4-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="293b4-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="293b4-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="293b4-127">Affidabilità e prestazioni</span><span class="sxs-lookup"><span data-stu-id="293b4-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="293b4-128">PreSendRequestHeaders e PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="293b4-128">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="293b4-129">Eventi di pagina asincrona con Web Form</span><span class="sxs-lookup"><span data-stu-id="293b4-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="293b4-130">Lavoro Fire-and-Forget</span><span class="sxs-lookup"><span data-stu-id="293b4-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="293b4-131">Il corpo entità della richiesta</span><span class="sxs-lookup"><span data-stu-id="293b4-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="293b4-132">Response. Redirect e Response</span><span class="sxs-lookup"><span data-stu-id="293b4-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="293b4-133">EnableViewState e ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="293b4-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="293b4-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="293b4-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="293b4-135">Richieste in esecuzione lunghe (> 110 secondi)</span><span class="sxs-lookup"><span data-stu-id="293b4-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="293b4-136">Conformità agli standard</span><span class="sxs-lookup"><span data-stu-id="293b4-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="293b4-137">Adattatori di controllo</span><span class="sxs-lookup"><span data-stu-id="293b4-137">Control Adapters</span></span>

<span data-ttu-id="293b4-138">Consiglio: Interrompere l'utilizzo di adattatori di controllo per il rendering adattivo e usare le query di supporto CSS e HTML conforme agli standard.</span><span class="sxs-lookup"><span data-stu-id="293b4-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="293b4-139">Gli adattatori di controlli sono stati introdotti in .NET 2.0 per il rendering di codice di presentazione che è stato personalizzato per gli ambienti e dispositivi diversi.</span><span class="sxs-lookup"><span data-stu-id="293b4-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="293b4-140">A questo punto, il rendering adattivo può essere eseguito con CSS e HTML.</span><span class="sxs-lookup"><span data-stu-id="293b4-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="293b4-141">Si deve interrompere l'utilizzo di adattatori di controllo e convertire tutti gli adapter esistenti in CSS e HTML.</span><span class="sxs-lookup"><span data-stu-id="293b4-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="293b4-142">Per ulteriori informazioni, vedere [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) e [procedura: aggiungere pagine Mobile Web Form di ASP.NET del / applicazione MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="293b4-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="293b4-143">Proprietà di stile per controlli</span><span class="sxs-lookup"><span data-stu-id="293b4-143">Style Properties on Controls</span></span>

<span data-ttu-id="293b4-144">Consiglio: Arrestare l'impostazione dei valori di stile nel markup del controllo e invece di impostare valori di formattazione nei fogli di stile CSS.</span><span class="sxs-lookup"><span data-stu-id="293b4-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="293b4-145">I controlli server Web contengono decine di proprietà che può essere utilizzata per impostare le proprietà di stile in linea.</span><span class="sxs-lookup"><span data-stu-id="293b4-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="293b4-146">Ad esempio, la proprietà ForeColor imposta il colore del testo per un controllo.</span><span class="sxs-lookup"><span data-stu-id="293b4-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="293b4-147">È possibile ottenere questo risultato stesso in modo più efficiente, tramite fogli di stile CSS.</span><span class="sxs-lookup"><span data-stu-id="293b4-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="293b4-148">Fogli di stile consentono di centralizzare i valori di stile e di evitare di impostare questi valori in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="293b4-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="293b4-149">L'esempio seguente mostra una classe CSS set testo rosso.</span><span class="sxs-lookup"><span data-stu-id="293b4-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="293b4-150">Nell'esempio seguente viene illustrato come applicare in modo dinamico la classe CSS.</span><span class="sxs-lookup"><span data-stu-id="293b4-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="293b4-151">Pagina e i callback di controllo</span><span class="sxs-lookup"><span data-stu-id="293b4-151">Page and Control Callbacks</span></span>

<span data-ttu-id="293b4-152">Consiglio: Interrompere l'utilizzo di callback di pagina e di controllo e quindi usare i seguenti: AJAX, UpdatePanel, metodi di azione MVC, API Web o SignalR.</span><span class="sxs-lookup"><span data-stu-id="293b4-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="293b4-153">Nelle versioni precedenti di ASP.NET, metodi di callback di pagina e il controllo abilitato, è possibile aggiornare parte della pagina web senza aggiornare un'intera pagina.</span><span class="sxs-lookup"><span data-stu-id="293b4-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="293b4-154">È ora possibile eseguire aggiornamenti parziali della pagina tramite [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="293b4-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="293b4-155">È consigliabile arrestare utilizzando metodi di callback, perché possono causare problemi con gli URL brevi e di routing.</span><span class="sxs-lookup"><span data-stu-id="293b4-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="293b4-156">Per impostazione predefinita, i controlli non abilitare metodi di callback, ma se è abilitata questa funzionalità in un controllo, è consigliabile disabilitarla.</span><span class="sxs-lookup"><span data-stu-id="293b4-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="293b4-157">Rilevamento di funzionalità del browser</span><span class="sxs-lookup"><span data-stu-id="293b4-157">Browser Capability Detection</span></span>

<span data-ttu-id="293b4-158">Consiglio: Interrompere l'uso di rilevamento di funzionalità browser statica e quindi usare rilevamento funzionalità dinamica.</span><span class="sxs-lookup"><span data-stu-id="293b4-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="293b4-159">Nelle versioni precedenti di ASP.NET, le funzionalità supportate per ogni browser vengono memorizzate in un file XML.</span><span class="sxs-lookup"><span data-stu-id="293b4-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="293b4-160">Supporto delle funzionalità rilevamento tramite una ricerca statica non è l'approccio migliore.</span><span class="sxs-lookup"><span data-stu-id="293b4-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="293b4-161">A questo punto, è possibile rilevare in modo dinamico un browser tutte le funzionalità supportate tramite un framework di rilevamento di funzionalità, ad esempio [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="293b4-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="293b4-162">Funzionalità rilevamento determina il supporto durante il tentativo di utilizzare un metodo o proprietà e quindi un controllo per verificare se il browser ha prodotto il risultato desiderato.</span><span class="sxs-lookup"><span data-stu-id="293b4-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="293b4-163">Per impostazione predefinita, Modernizr è incluso nei modelli di applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="293b4-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="293b4-164">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="293b4-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="293b4-165">Convalida della richiesta</span><span class="sxs-lookup"><span data-stu-id="293b4-165">Request Validation</span></span>

<span data-ttu-id="293b4-166">Consiglio: Convalidare l'input dell'utente e la codifica dell'output da utenti.</span><span class="sxs-lookup"><span data-stu-id="293b4-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="293b4-167">Convalida della richiesta è una funzionalità di ASP.NET che esamina ogni richiesta e interrompe la richiesta se viene trovata una minaccia.</span><span class="sxs-lookup"><span data-stu-id="293b4-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="293b4-168">Non basarsi sulla convalida richiesta per la protezione dell'applicazione da attacchi di script tra siti.</span><span class="sxs-lookup"><span data-stu-id="293b4-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="293b4-169">Al contrario, convalidare tutti gli input dagli utenti e codificare l'output.</span><span class="sxs-lookup"><span data-stu-id="293b4-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="293b4-170">In alcuni casi, è possibile utilizzare espressioni regolari per convalidare l'input, ma nei casi più complessi, che è necessario convalidare input dell'utente usando le classi .NET che determinano se il valore corrisponde a valori consentiti.</span><span class="sxs-lookup"><span data-stu-id="293b4-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="293b4-171">Nell'esempio seguente viene illustrato come utilizzare un metodo statico nella classe Uri per determinare se l'Uri fornito da un utente è valido.</span><span class="sxs-lookup"><span data-stu-id="293b4-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="293b4-172">Tuttavia, per verificare sufficientemente l'Uri, anche controllare per verificare che venga specificato `http` o `https`.</span><span class="sxs-lookup"><span data-stu-id="293b4-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="293b4-173">L'esempio seguente usa i metodi di istanza per verificare che l'Uri sia valido.</span><span class="sxs-lookup"><span data-stu-id="293b4-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="293b4-174">Prima di eseguire il rendering di input dell'utente in formato HTML o tra l'input dell'utente in una query SQL, codificare i valori per evitare che sia incluso codice dannoso.</span><span class="sxs-lookup"><span data-stu-id="293b4-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="293b4-175">È possibile HTML codifica il valore nel markup con la &lt;%: %&gt; sintassi, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="293b4-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="293b4-176">In alternativa, nella sintassi Razor, è possibile HTML codifica con @, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="293b4-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="293b4-177">L'esempio seguente mostra come HTML codifica un valore nel code-behind.</span><span class="sxs-lookup"><span data-stu-id="293b4-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="293b4-178">Per codificare in modo sicuro un valore per i comandi SQL, utilizzare i parametri del comando, ad esempio il [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="293b4-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="293b4-179">Sessione e l'autenticazione basata su form senza cookie</span><span class="sxs-lookup"><span data-stu-id="293b4-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="293b4-180">Consiglio: Richiedono i cookie.</span><span class="sxs-lookup"><span data-stu-id="293b4-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="293b4-181">Il passaggio di informazioni di autenticazione nella stringa di query non è protetto.</span><span class="sxs-lookup"><span data-stu-id="293b4-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="293b4-182">Quando l'applicazione include l'autenticazione, pertanto richiede i cookie.</span><span class="sxs-lookup"><span data-stu-id="293b4-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="293b4-183">Se i cookie vengono archiviate informazioni riservate, è consigliabile richiedere SSL per il cookie.</span><span class="sxs-lookup"><span data-stu-id="293b4-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="293b4-184">Nell'esempio seguente viene illustrato come specificare il file Web. config che l'autenticazione basata su form richiede un cookie viene trasmesso tramite SSL.</span><span class="sxs-lookup"><span data-stu-id="293b4-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="293b4-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="293b4-185">EnableViewStateMac</span></span>

<span data-ttu-id="293b4-186">Consiglio: Mai impostata su false.</span><span class="sxs-lookup"><span data-stu-id="293b4-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="293b4-187">Per impostazione predefinita, EnbableViewStateMac è impostata su true.</span><span class="sxs-lookup"><span data-stu-id="293b4-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="293b4-188">Anche se l'applicazione non utilizza lo stato di visualizzazione, non impostare EnableViewStateMac su false.</span><span class="sxs-lookup"><span data-stu-id="293b4-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="293b4-189">Impostare questo valore su false verranno rendere l'applicazione vulnerabile agli script tra siti.</span><span class="sxs-lookup"><span data-stu-id="293b4-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="293b4-190">A partire da ASP.NET 4.5.2, il runtime applica **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="293b4-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="293b4-191">Anche se si imposta su false, il runtime ignora questo valore e procede con il valore impostato su true.</span><span class="sxs-lookup"><span data-stu-id="293b4-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="293b4-192">Per ulteriori informazioni, vedere [ASP.NET 4.5.2 ed EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="293b4-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="293b4-193">Nell'esempio seguente viene illustrato come impostare EnableViewStateMac su true.</span><span class="sxs-lookup"><span data-stu-id="293b4-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="293b4-194">Non è necessario impostare effettivamente questo valore su true perché è true per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="293b4-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="293b4-195">Tuttavia, se si have impostato su false in qualsiasi pagina dell'applicazione, è necessario correggere subito questo valore.</span><span class="sxs-lookup"><span data-stu-id="293b4-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="293b4-196">Attendibilità media</span><span class="sxs-lookup"><span data-stu-id="293b4-196">Medium Trust</span></span>

<span data-ttu-id="293b4-197">Consiglio: Non dipendono attendibilità media (o qualsiasi altro livello di attendibilità) come limite di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="293b4-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="293b4-198">Attendibilità parziale non protegge in modo adeguato l'applicazione e non deve essere utilizzata.</span><span class="sxs-lookup"><span data-stu-id="293b4-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="293b4-199">In alternativa, usare l'attendibilità totale e isolare le applicazioni non attendibili nel pool di applicazioni separati.</span><span class="sxs-lookup"><span data-stu-id="293b4-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="293b4-200">Inoltre, eseguire ogni pool di applicazioni con un'identità univoca.</span><span class="sxs-lookup"><span data-stu-id="293b4-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="293b4-201">Per ulteriori informazioni, vedere [attendibilità parziale di ASP.NET non garantisce l'isolamento delle applicazioni](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="293b4-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="293b4-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="293b4-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="293b4-203">Consiglio: Non disattivare le impostazioni di sicurezza &lt;appSettings&gt; elemento.</span><span class="sxs-lookup"><span data-stu-id="293b4-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="293b4-204">L'elemento appSettings contiene molti valori che sono necessari per gli aggiornamenti di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="293b4-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="293b4-205">Non si deve modificare o disabilitare questi valori.</span><span class="sxs-lookup"><span data-stu-id="293b4-205">You should not change or disable these values.</span></span> <span data-ttu-id="293b4-206">Se è necessario disabilitare questi valori quando si distribuisce un aggiornamento, immediatamente abilitare di nuovo dopo aver completato la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="293b4-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="293b4-207">Per informazioni dettagliate, vedere [elemento appSettings ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="293b4-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="293b4-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="293b4-208">UrlPathEncode</span></span>

<span data-ttu-id="293b4-209">Suggerimento: Usare [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) invece.</span><span class="sxs-lookup"><span data-stu-id="293b4-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="293b4-210">Il metodo UrlPathEncode è stato aggiunto a .NET Framework per risolvere un problema di compatibilità del browser molto specifiche.</span><span class="sxs-lookup"><span data-stu-id="293b4-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="293b4-211">Non codificare in modo adeguato un URL e non impedisce l'applicazione di script tra siti.</span><span class="sxs-lookup"><span data-stu-id="293b4-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="293b4-212">Non deve mai utilizzato nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="293b4-212">You should never use it in your application.</span></span> <span data-ttu-id="293b4-213">Utilizzare invece [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="293b4-213">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="293b4-214">Nell'esempio seguente viene illustrato come passare un URL codificato come parametro di stringa di query per un controllo hyperlink.</span><span class="sxs-lookup"><span data-stu-id="293b4-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="293b4-215">Affidabilità e prestazioni</span><span class="sxs-lookup"><span data-stu-id="293b4-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="293b4-216">PreSendRequestHeaders e PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="293b4-216">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="293b4-217">Consiglio: Non utilizzare questi eventi con i moduli gestiti.</span><span class="sxs-lookup"><span data-stu-id="293b4-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="293b4-218">Invece di scrivere un modulo nativo di IIS per eseguire l'operazione necessaria.</span><span class="sxs-lookup"><span data-stu-id="293b4-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="293b4-219">Vedere [la creazione di moduli di codice nativo HTTP](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="293b4-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="293b4-220">È possibile utilizzare il [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) e [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) gli eventi con i moduli nativi di IIS.</span><span class="sxs-lookup"><span data-stu-id="293b4-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="293b4-221">Non utilizzare `PreSendRequestHeaders` e `PreSendRequestContent` con i moduli gestiti che implementano `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="293b4-221">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="293b4-222">Impostazione di queste proprietà possono causare problemi durante le richieste asincrone.</span><span class="sxs-lookup"><span data-stu-id="293b4-222">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="293b4-223">La combinazione di applicazione richiesto Routing (ARR) e WebSocket potrebbe portare a eccezioni di violazione di accesso che possono causare w3wp arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="293b4-223">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="293b4-224">Ad esempio, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 in iiscore.dll ha causato un'eccezione di violazione di accesso (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="293b4-224">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="293b4-225">Eventi di pagina asincrona con Web Form</span><span class="sxs-lookup"><span data-stu-id="293b4-225">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="293b4-226">Consiglio: In Web Form, evitare di scrivere async void metodi per gli eventi del ciclo di vita della pagina e quindi usare [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) per codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="293b4-226">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="293b4-227">Se si contrassegna un evento di pagina con **async** e **void**, non è possibile determinare quando termina il codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="293b4-227">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="293b4-228">Utilizzare invece Page.RegisterAsyncTask per eseguire il codice asincrono in modo che consente di tenere traccia del relativo completamento.</span><span class="sxs-lookup"><span data-stu-id="293b4-228">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="293b4-229">Nell'esempio seguente un pulsante di fare clic su gestore contenente codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="293b4-229">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="293b4-230">Questo esempio è inclusa la lettura di un valore stringa in modo asincrono, che viene fornito solo come un esempio semplificato di un'attività asincrona e non come una procedura consigliata.</span><span class="sxs-lookup"><span data-stu-id="293b4-230">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="293b4-231">Se si usano le attività asincrone, impostare il framework di destinazione del runtime Http 4.5 nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="293b4-231">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="293b4-232">Impostare il framework di destinazione 4.5 attiva nel contesto di sincronizzazione nuovo che è stato aggiunto in .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="293b4-232">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="293b4-233">Questo valore è impostato per impostazione predefinita nei nuovi progetti in Visual Studio 2012, ma è non possibile impostare se si lavora con un progetto esistente.</span><span class="sxs-lookup"><span data-stu-id="293b4-233">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="293b4-234">Lavoro Fire-and-Forget</span><span class="sxs-lookup"><span data-stu-id="293b4-234">Fire-and-Forget Work</span></span>

<span data-ttu-id="293b4-235">Suggerimento: Quando si gestisce una richiesta all'interno di ASP.NET, evitare di avvio lavoro fire-and-forget (tale chiamata al metodo QueueUserWorkItem o la creazione di un timer che chiama ripetutamente un delegato).</span><span class="sxs-lookup"><span data-stu-id="293b4-235">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="293b4-236">Se l'applicazione presenta lavoro fire-and-forget in esecuzione in ASP.NET, l'applicazione può ottenere sincronizzato. In qualsiasi momento, può essere eliminato il dominio dell'applicazione che si intende che il processo in corso potrebbe non corrispondere lo stato corrente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="293b4-236">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="293b4-237">È consigliabile spostare questo tipo di lavoro all'esterno di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="293b4-237">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="293b4-238">È possibile utilizzare un processi Web, servizio di Windows o un ruolo di lavoro in Azure per eseguire il lavoro in corso ed eseguire il codice da un altro processo.</span><span class="sxs-lookup"><span data-stu-id="293b4-238">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="293b4-239">Se è necessario eseguire questa operazione all'interno di ASP.NET, è possibile aggiungere il pacchetto Nuget chiamato [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) per eseguire il codice.</span><span class="sxs-lookup"><span data-stu-id="293b4-239">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="293b4-240">Il corpo entità della richiesta</span><span class="sxs-lookup"><span data-stu-id="293b4-240">Request Entity Body</span></span>

<span data-ttu-id="293b4-241">Consiglio: Impedire la lettura di Request. Form o Request.InputStream prima dell'esecuzione del gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="293b4-241">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="293b4-242">La prima è necessario leggere da Request. Form o Request.InputStream è il gestore evento eseguito.</span><span class="sxs-lookup"><span data-stu-id="293b4-242">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="293b4-243">In MVC, il Controller è il gestore e l'Esegui evento quando viene eseguito il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="293b4-243">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="293b4-244">In Web Form, la pagina è il gestore e l'Esegui evento quando viene generato l'evento Page. Init.</span><span class="sxs-lookup"><span data-stu-id="293b4-244">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="293b4-245">Se in precedenza l'evento Esegui leggere il corpo entità della richiesta, interferire con l'elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="293b4-245">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="293b4-246">Se è necessario leggere il corpo di entità di richiesta prima dell'evento di esecuzione, utilizzare uno [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) o [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="293b4-246">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="293b4-247">Quando si utilizza GetBufferlessInputStream, ottenere il flusso non elaborato della richiesta e assumere la responsabilità per l'elaborazione della richiesta di intera.</span><span class="sxs-lookup"><span data-stu-id="293b4-247">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="293b4-248">Dopo aver chiamato GetBufferlessInputStream, Request. Form e Request.InputStream non sono disponibili perché non sono stati compilati da ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="293b4-248">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="293b4-249">Quando si utilizza GetBufferedInputStream, si ottiene una copia del flusso della richiesta.</span><span class="sxs-lookup"><span data-stu-id="293b4-249">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="293b4-250">Request. Form Request.InputStream rimangono comunque disponibili in un secondo momento nella richiesta poiché ASP.NET compila a altra copia.</span><span class="sxs-lookup"><span data-stu-id="293b4-250">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="293b4-251">Response. Redirect e Response</span><span class="sxs-lookup"><span data-stu-id="293b4-251">Response.Redirect and Response.End</span></span>

<span data-ttu-id="293b4-252">Consiglio: Tenere presente le differenze nella modalità di gestione di thread dopo la chiamata [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="293b4-252">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="293b4-253">Il [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) metodo chiama il metodo Response.</span><span class="sxs-lookup"><span data-stu-id="293b4-253">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="293b4-254">In un processo sincrono, chiamare Request.Redirect fa sì che il thread corrente viene interrotta immediatamente.</span><span class="sxs-lookup"><span data-stu-id="293b4-254">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="293b4-255">Tuttavia, in un processo asincrono, chiamare Response. Redirect non interrompe il thread corrente, in modo continua l'esecuzione del codice per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="293b4-255">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="293b4-256">In un processo asincrono, è necessario restituire l'attività da parte del metodo per interrompere l'esecuzione di codice.</span><span class="sxs-lookup"><span data-stu-id="293b4-256">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="293b4-257">In un progetto MVC, è necessario non chiamare Response. Redirect.</span><span class="sxs-lookup"><span data-stu-id="293b4-257">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="293b4-258">Al contrario, restituire un RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="293b4-258">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="293b4-259">EnableViewState e ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="293b4-259">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="293b4-260">Suggerimento: Usare ViewStateMode anziché EnableViewState, per fornire un controllo granulare quali controlli utilizzare lo stato di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="293b4-260">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="293b4-261">Quando si imposta EnableViewState su false nella direttiva della pagina, lo stato di visualizzazione è disabilitato per tutti i controlli all'interno della pagina e non può essere abilitato.</span><span class="sxs-lookup"><span data-stu-id="293b4-261">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="293b4-262">Se si desidera abilitare lo stato di visualizzazione solo per alcuni controlli della pagina, impostare ViewStateMode su disabilitato per la pagina.</span><span class="sxs-lookup"><span data-stu-id="293b4-262">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="293b4-263">Quindi, impostare ViewStateMode su abilitato, solo i controlli che devono effettivamente lo stato di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="293b4-263">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="293b4-264">Abilita lo stato di visualizzazione per solo i controlli che ne hanno necessità, è possibile ridurre le dimensioni dello stato di visualizzazione per le pagine web.</span><span class="sxs-lookup"><span data-stu-id="293b4-264">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="293b4-265">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="293b4-265">SqlMembershipProvider</span></span>

<span data-ttu-id="293b4-266">Suggerimento: Usare provider Universal.</span><span class="sxs-lookup"><span data-stu-id="293b4-266">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="293b4-267">Nei modelli di progetto corrente, SqlMembershipProvider è stata sostituita da [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), che è disponibile come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="293b4-267">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="293b4-268">Se si utilizza SqlMembershipProvider in un progetto che è stato compilato con una versione precedente dei modelli, è necessario passare a Universal Providers.</span><span class="sxs-lookup"><span data-stu-id="293b4-268">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="293b4-269">I provider Universal funziona con tutti i database che sono supportati da Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="293b4-269">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="293b4-270">Per ulteriori informazioni, vedere [Introduzione a ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="293b4-270">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="293b4-271">Le richieste a esecuzione prolungata (> 110 secondi)</span><span class="sxs-lookup"><span data-stu-id="293b4-271">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="293b4-272">Suggerimento: Usare [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) o [SignalR](../../../signalr/index.md) per i client connessi e utilizzare le operazioni dei / o asincrone.</span><span class="sxs-lookup"><span data-stu-id="293b4-272">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="293b4-273">Richieste a esecuzione prolungata possono causare risultati imprevisti e una riduzione delle prestazioni nell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="293b4-273">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="293b4-274">L'impostazione di timeout predefinito per una richiesta è 110 secondi.</span><span class="sxs-lookup"><span data-stu-id="293b4-274">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="293b4-275">Se si utilizza lo stato della sessione con una richiesta a esecuzione prolungata, ASP.NET verrà rilasciare il blocco sull'oggetto sessione dopo 110 secondi.</span><span class="sxs-lookup"><span data-stu-id="293b4-275">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="293b4-276">Tuttavia, l'applicazione potrebbe essere all'interno di un'operazione sull'oggetto di sessione quando viene rilasciato il blocco e l'operazione potrebbe non essere completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="293b4-276">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="293b4-277">Se una seconda richiesta da parte dell'utente viene bloccata durante l'esecuzione della prima richiesta, la seconda richiesta può accedere all'oggetto di sessione in uno stato incoerente.</span><span class="sxs-lookup"><span data-stu-id="293b4-277">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="293b4-278">Se l'applicazione include operazioni dei / o di blocco (o sincrone), l'applicazione sarà non risponda.</span><span class="sxs-lookup"><span data-stu-id="293b4-278">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="293b4-279">Per migliorare le prestazioni, utilizzare le operazioni dei / o asincrone in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="293b4-279">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="293b4-280">Inoltre, è possibile usare WebSocket o SignalR per la connessione client al server.</span><span class="sxs-lookup"><span data-stu-id="293b4-280">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="293b4-281">Queste funzionalità sono progettate per gestire in modo efficiente le richieste a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="293b4-281">These features are designed to efficiently handle long-running requests.</span></span>
