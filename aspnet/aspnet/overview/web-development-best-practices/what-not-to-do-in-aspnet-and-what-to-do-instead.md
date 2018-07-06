---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Che cosa è necessario eseguire operazioni in ASP.NET e come procedere invece | Microsoft Docs
author: tfitzmac
description: Questo argomento descrive i diversi errori comuni che gli utenti apportano all'interno dei progetti web ASP.NET. Fornisce indicazioni per cosa è necessario fare per evitare questi comu...
ms.author: aspnetcontent
ms.date: 05/08/2014
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 9e9b192126995ac8a461b15bce69b60d57ca9ba1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806018"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="da401-104">Che cosa è necessario eseguire operazioni in ASP.NET e come procedere invece</span><span class="sxs-lookup"><span data-stu-id="da401-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="da401-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="da401-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="da401-106">Questo argomento descrive i diversi errori comuni che gli utenti apportano all'interno dei progetti web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="da401-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="da401-107">Fornisce indicazioni per cosa è necessario fare per evitare questi errori comuni.</span><span class="sxs-lookup"><span data-stu-id="da401-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="da401-108">Si basa su un [presentation](http://vimeo.com/68390507) dal **Damian Edwards** presso Norwegian Developers Conference.</span><span class="sxs-lookup"><span data-stu-id="da401-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="da401-109">Dichiarazione di non responsabilità</span><span class="sxs-lookup"><span data-stu-id="da401-109">Disclaimer</span></span>

<span data-ttu-id="da401-110">In questo argomento non è come una guida completa per garantire che l'applicazione è sicura ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="da401-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="da401-111">È comunque necessario seguire le procedure consigliate per la sicurezza e prestazioni non indicati in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="da401-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="da401-112">Suggerisce solo come evitare gli errori più comuni relativi a processi e le classi .NET.</span><span class="sxs-lookup"><span data-stu-id="da401-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="da401-113">Panoramica</span><span class="sxs-lookup"><span data-stu-id="da401-113">Overview</span></span>

<span data-ttu-id="da401-114">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="da401-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="da401-115">Conformità agli standard</span><span class="sxs-lookup"><span data-stu-id="da401-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="da401-116">Adattatori di controllo</span><span class="sxs-lookup"><span data-stu-id="da401-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="da401-117">Proprietà di stile per controlli</span><span class="sxs-lookup"><span data-stu-id="da401-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="da401-118">Pagina e i callback di controllo</span><span class="sxs-lookup"><span data-stu-id="da401-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="da401-119">Rilevamento di funzionalità del browser</span><span class="sxs-lookup"><span data-stu-id="da401-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="da401-120">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="da401-120">Security</span></span>](#security)

    - [<span data-ttu-id="da401-121">Convalida delle richieste</span><span class="sxs-lookup"><span data-stu-id="da401-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="da401-122">Sessione e autenticazione basata su form senza cookie</span><span class="sxs-lookup"><span data-stu-id="da401-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="da401-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="da401-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="da401-124">Livello di attendibilità medio</span><span class="sxs-lookup"><span data-stu-id="da401-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="da401-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="da401-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="da401-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="da401-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="da401-127">Affidabilità e prestazioni</span><span class="sxs-lookup"><span data-stu-id="da401-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="da401-128">PreSendRequestHeaders e PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="da401-128">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="da401-129">Eventi di pagina asincrona con i Web Form</span><span class="sxs-lookup"><span data-stu-id="da401-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="da401-130">Lavoro Fire-and-Forget</span><span class="sxs-lookup"><span data-stu-id="da401-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="da401-131">Corpo entità richiedente</span><span class="sxs-lookup"><span data-stu-id="da401-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="da401-132">Response. Redirect e Response</span><span class="sxs-lookup"><span data-stu-id="da401-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="da401-133">EnableViewState e ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="da401-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="da401-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="da401-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="da401-135">Richieste di lunga durata esecuzione (> 110 secondi)</span><span class="sxs-lookup"><span data-stu-id="da401-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="da401-136">Conformità agli standard</span><span class="sxs-lookup"><span data-stu-id="da401-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="da401-137">Adattatori di controllo</span><span class="sxs-lookup"><span data-stu-id="da401-137">Control Adapters</span></span>

<span data-ttu-id="da401-138">Raccomandazione: Interrompere l'uso di adattatori di controllo per il rendering adattivo e usare invece le query supporti CSS e HTML conforme agli standard.</span><span class="sxs-lookup"><span data-stu-id="da401-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="da401-139">Gli adattatori di controlli sono stati introdotti in .NET 2.0 per il rendering di codice di presentazione che è stato personalizzato per gli ambienti e dispositivi diversi.</span><span class="sxs-lookup"><span data-stu-id="da401-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="da401-140">A questo punto, il rendering adattivo può essere eseguito con HTML e CSS.</span><span class="sxs-lookup"><span data-stu-id="da401-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="da401-141">È necessario interrompere l'uso di adattatori di controllo e convertire tutti gli adapter esistenti CSS e HTML.</span><span class="sxs-lookup"><span data-stu-id="da401-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="da401-142">Per altre informazioni, vedere [query sui supporti](http://www.w3.org/TR/css3-mediaqueries/) e [procedura: aggiungere pagine per dispositivi mobili per il Web Form ASP.NET / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="da401-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="da401-143">Proprietà di stile per controlli</span><span class="sxs-lookup"><span data-stu-id="da401-143">Style Properties on Controls</span></span>

<span data-ttu-id="da401-144">Raccomandazione: Arrestare l'impostazione dei valori di stile nel markup del controllo e invece impostare valori di formattazione in fogli di stile CSS.</span><span class="sxs-lookup"><span data-stu-id="da401-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="da401-145">I controlli server Web contengono decine di proprietà che può essere usata per impostare le proprietà di stile in linea.</span><span class="sxs-lookup"><span data-stu-id="da401-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="da401-146">Ad esempio, la proprietà ForeColor imposta il colore del testo per un controllo.</span><span class="sxs-lookup"><span data-stu-id="da401-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="da401-147">È possibile ottenere questo risultato stesso in modo più efficiente tramite fogli di stile CSS.</span><span class="sxs-lookup"><span data-stu-id="da401-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="da401-148">Fogli di stile consentono di centralizzare i valori di stile e non impostare questi valori in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="da401-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="da401-149">Nell'esempio seguente mostra una classe CSS il testo di set e impostato su rosso.</span><span class="sxs-lookup"><span data-stu-id="da401-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="da401-150">L'esempio seguente viene illustrato come applicare in modo dinamico la classe CSS.</span><span class="sxs-lookup"><span data-stu-id="da401-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="da401-151">Pagina e i callback di controllo</span><span class="sxs-lookup"><span data-stu-id="da401-151">Page and Control Callbacks</span></span>

<span data-ttu-id="da401-152">Raccomandazione: Interrompere l'uso di callback di pagina e controllo e usare invece le seguenti operazioni: AJAX, UpdatePanel, i metodi di azione MVC, API Web o SignalR.</span><span class="sxs-lookup"><span data-stu-id="da401-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="da401-153">Nelle versioni precedenti di ASP.NET, i metodi di callback di pagina e il controllo consentono di aggiornare una parte della pagina web senza l'aggiornamento di un'intera pagina.</span><span class="sxs-lookup"><span data-stu-id="da401-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="da401-154">È ora possibile eseguire aggiornamenti parziali della pagina tramite [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="da401-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="da401-155">È consigliabile arrestare usando i metodi di callback perché possono causare problemi con gli URL brevi e routing.</span><span class="sxs-lookup"><span data-stu-id="da401-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="da401-156">Per impostazione predefinita, i controlli non abilitare i metodi di callback, ma se è abilitata questa funzionalità in un controllo, è consigliabile disabilitarla.</span><span class="sxs-lookup"><span data-stu-id="da401-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="da401-157">Rilevamento di funzionalità del browser</span><span class="sxs-lookup"><span data-stu-id="da401-157">Browser Capability Detection</span></span>

<span data-ttu-id="da401-158">Raccomandazione: Interrompere l'uso di rilevamento di funzionalità del browser statico e usare invece il rilevamento di funzionalità dinamiche.</span><span class="sxs-lookup"><span data-stu-id="da401-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="da401-159">Nelle versioni precedenti di ASP.NET, le funzionalità supportate per ogni browser sono state archiviate in un file XML.</span><span class="sxs-lookup"><span data-stu-id="da401-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="da401-160">Supporto della funzionalità rilevamento tramite una ricerca statico non è l'approccio migliore.</span><span class="sxs-lookup"><span data-stu-id="da401-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="da401-161">A questo punto, è possibile rilevare in modo dinamico un browser le funzionalità supportate usando un framework di rilevamento di funzionalità, ad esempio [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="da401-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="da401-162">Rilevamento delle funzionalità determina il supporto tenta di utilizzare un metodo o proprietà e quindi un controllo per verificare se il browser ha prodotto il risultato desiderato.</span><span class="sxs-lookup"><span data-stu-id="da401-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="da401-163">Per impostazione predefinita, Modernizr è incluso nei modelli di applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="da401-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="da401-164">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="da401-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="da401-165">Convalida delle richieste</span><span class="sxs-lookup"><span data-stu-id="da401-165">Request Validation</span></span>

<span data-ttu-id="da401-166">Raccomandazione: Convalida dell'input utente e la codifica dell'output dagli utenti.</span><span class="sxs-lookup"><span data-stu-id="da401-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="da401-167">Convalida della richiesta è una funzionalità di ASP.NET che esamina ogni richiesta e interrompe la richiesta se viene rilevata una minaccia percepita.</span><span class="sxs-lookup"><span data-stu-id="da401-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="da401-168">Non basarsi sul convalida delle richieste per la protezione dell'applicazione da attacchi di scripting intersito.</span><span class="sxs-lookup"><span data-stu-id="da401-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="da401-169">Al contrario, convalidare tutti gli input dagli utenti e codificare l'output.</span><span class="sxs-lookup"><span data-stu-id="da401-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="da401-170">In alcuni casi, è possibile usare espressioni regolari per convalidare l'input, ma nei casi più complessi, che è necessario convalidare i valori consentiti di input dell'utente tramite le classi .NET che determinano se il valore corrisponde.</span><span class="sxs-lookup"><span data-stu-id="da401-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="da401-171">Nell'esempio seguente viene illustrato come usare un metodo statico nella classe Uri per determinare se l'Uri fornito da un utente è valido.</span><span class="sxs-lookup"><span data-stu-id="da401-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="da401-172">Tuttavia, per sufficientemente verificare l'Uri, dovrebbe anche controllare per verificare che venga specificato `http` o `https`.</span><span class="sxs-lookup"><span data-stu-id="da401-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="da401-173">Nell'esempio seguente usa i metodi di istanza per verificare che l'Uri sia valido.</span><span class="sxs-lookup"><span data-stu-id="da401-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="da401-174">Prima di eseguire il rendering di input dell'utente in formato HTML o inclusi input dell'utente in una query SQL, codificare i valori per assicurarsi che non è incluso codice dannoso.</span><span class="sxs-lookup"><span data-stu-id="da401-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="da401-175">Si può HTML codifica il valore nel markup con la &lt;%: %&gt; sintassi, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="da401-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="da401-176">O, nella sintassi Razor, si può HTML codificare con @, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="da401-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="da401-177">L'esempio seguente mostra come HTML codifica un valore nel code-behind.</span><span class="sxs-lookup"><span data-stu-id="da401-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="da401-178">Per codificare in modo sicuro un valore per i comandi SQL, usare i parametri di comando, ad esempio la [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="da401-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="da401-179">Sessione e autenticazione basata su form senza cookie</span><span class="sxs-lookup"><span data-stu-id="da401-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="da401-180">Raccomandazione: Richiede i cookie.</span><span class="sxs-lookup"><span data-stu-id="da401-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="da401-181">Il passaggio di informazioni di autenticazione nella stringa di query non è protetto.</span><span class="sxs-lookup"><span data-stu-id="da401-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="da401-182">Pertanto, richiede i cookie quando l'applicazione include l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="da401-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="da401-183">Se i cookie vengono archiviate le informazioni riservate, è consigliabile richiedere SSL per il cookie.</span><span class="sxs-lookup"><span data-stu-id="da401-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="da401-184">Nell'esempio seguente viene illustrato come specificare il file Web. config che un cookie che viene trasmesso tramite SSL richiede l'autenticazione basata su form.</span><span class="sxs-lookup"><span data-stu-id="da401-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="da401-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="da401-185">EnableViewStateMac</span></span>

<span data-ttu-id="da401-186">Raccomandazione: Mai impostata su false.</span><span class="sxs-lookup"><span data-stu-id="da401-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="da401-187">Per impostazione predefinita, EnbableViewStateMac è impostato su true.</span><span class="sxs-lookup"><span data-stu-id="da401-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="da401-188">Anche se l'applicazione non usa lo stato di visualizzazione, non impostare EnableViewStateMac su false.</span><span class="sxs-lookup"><span data-stu-id="da401-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="da401-189">Impostando questo valore su false verrà rendere l'applicazione vulnerabile a XSS.</span><span class="sxs-lookup"><span data-stu-id="da401-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="da401-190">A partire da ASP.NET 4.5.2, il runtime applica **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="da401-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="da401-191">Anche se si impostato su false, il runtime ignora questo valore e procede con il valore impostato su true.</span><span class="sxs-lookup"><span data-stu-id="da401-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="da401-192">Per altre informazioni, vedere [ASP.NET 4.5.2 ed EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="da401-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="da401-193">Nell'esempio seguente viene illustrato come impostare EnableViewStateMac su true.</span><span class="sxs-lookup"><span data-stu-id="da401-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="da401-194">Non devi effettivamente impostare questo valore su true perché è true per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="da401-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="da401-195">Tuttavia, se si hanno impostata su false per qualsiasi pagina nell'applicazione, è necessario correggere subito questo valore.</span><span class="sxs-lookup"><span data-stu-id="da401-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="da401-196">Livello di attendibilità medio</span><span class="sxs-lookup"><span data-stu-id="da401-196">Medium Trust</span></span>

<span data-ttu-id="da401-197">Raccomandazione: Non dipendono attendibilità media (o qualsiasi altro livello di attendibilità) come limite di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="da401-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="da401-198">Attendibilità parziale non protegge in modo adeguato l'applicazione e non deve essere utilizzata.</span><span class="sxs-lookup"><span data-stu-id="da401-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="da401-199">In alternativa, usare l'attendibilità e isolare le applicazioni non attendibili nel pool di applicazioni separati.</span><span class="sxs-lookup"><span data-stu-id="da401-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="da401-200">Inoltre, eseguire ogni pool di applicazioni in un'identità univoca.</span><span class="sxs-lookup"><span data-stu-id="da401-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="da401-201">Per altre informazioni, vedere [ASP.NET parzialmente attendibile non garantisce l'isolamento applicazione](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="da401-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="da401-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="da401-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="da401-203">Raccomandazione: Non disattivare le impostazioni di sicurezza &lt;appSettings&gt; elemento.</span><span class="sxs-lookup"><span data-stu-id="da401-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="da401-204">L'elemento appSettings contiene molti valori necessari per gli aggiornamenti della sicurezza.</span><span class="sxs-lookup"><span data-stu-id="da401-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="da401-205">Non devi modificare o disabilitare tali valori.</span><span class="sxs-lookup"><span data-stu-id="da401-205">You should not change or disable these values.</span></span> <span data-ttu-id="da401-206">Se è necessario disabilitare questi valori quando si distribuisce un aggiornamento, immediatamente abilitare di nuovo dopo il completamento della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="da401-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="da401-207">Per informazioni dettagliate, vedere [elemento appSettings ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="da401-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="da401-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="da401-208">UrlPathEncode</span></span>

<span data-ttu-id="da401-209">Consiglio: Usare [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) invece.</span><span class="sxs-lookup"><span data-stu-id="da401-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="da401-210">Il metodo UrlPathEncode è stato aggiunto a .NET Framework per risolvere un problema di compatibilità browser molto specifico.</span><span class="sxs-lookup"><span data-stu-id="da401-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="da401-211">Può non codificare in modo adeguato un URL e non protegge l'applicazione di scripting intersito.</span><span class="sxs-lookup"><span data-stu-id="da401-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="da401-212">Non deve mai utilizzato nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="da401-212">You should never use it in your application.</span></span> <span data-ttu-id="da401-213">Usare invece [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="da401-213">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="da401-214">Nell'esempio seguente viene illustrato come passare un URL codificato come parametro di stringa di query per un controllo collegamento ipertestuale.</span><span class="sxs-lookup"><span data-stu-id="da401-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="da401-215">Affidabilità e prestazioni</span><span class="sxs-lookup"><span data-stu-id="da401-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="da401-216">PreSendRequestHeaders e PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="da401-216">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="da401-217">Raccomandazione: Non usare questi eventi con moduli gestiti.</span><span class="sxs-lookup"><span data-stu-id="da401-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="da401-218">Invece di scrivere un modulo IIS nativo per eseguire l'operazione necessaria.</span><span class="sxs-lookup"><span data-stu-id="da401-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="da401-219">Visualizzare [creazione di moduli HTTP di codice nativo](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="da401-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="da401-220">È possibile usare la [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) e [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) gli eventi con i moduli nativi IIS.</span><span class="sxs-lookup"><span data-stu-id="da401-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="da401-221">Non utilizzare `PreSendRequestHeaders` e `PreSendRequestContent` con i moduli gestiti che implementano `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="da401-221">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="da401-222">Impostazione di queste proprietà possono causare problemi con le richieste asincrone.</span><span class="sxs-lookup"><span data-stu-id="da401-222">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="da401-223">La combinazione di applicazioni ha richiesto il Routing (ARR) e websockets potrebbe causare eccezioni di violazione di accesso che possono causare w3wp arresto anomalo del sistema.</span><span class="sxs-lookup"><span data-stu-id="da401-223">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="da401-224">Ad esempio, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 in iiscore.dll ha generato un'eccezione di violazione di accesso (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="da401-224">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="da401-225">Eventi di pagina asincrona con i Web Form</span><span class="sxs-lookup"><span data-stu-id="da401-225">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="da401-226">Raccomandazione: In Web Form, evitare di scrivere async void metodi per gli eventi del ciclo di vita di pagina e usare invece [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) per il codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="da401-226">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="da401-227">Quando si contrassegna un evento di pagina con **async** e **void**, non è possibile determinare quando ha terminato il codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="da401-227">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="da401-228">Usare invece Page.RegisterAsyncTask per eseguire il codice asincrono in modo che consente di tenere traccia del relativo completamento.</span><span class="sxs-lookup"><span data-stu-id="da401-228">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="da401-229">L'esempio seguente mostra un pulsante di fare clic sul gestore che contiene il codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="da401-229">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="da401-230">Questo esempio include la lettura di un valore stringa in modo asincrono, che viene fornito solo come un esempio semplificato di un'attività asincrona e non come una procedura consigliata.</span><span class="sxs-lookup"><span data-stu-id="da401-230">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="da401-231">Se si usa le attività asincrone, impostare il framework di destinazione di runtime Http alla versione 4.5 nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="da401-231">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="da401-232">Impostazione del framework di destinazione a 4.5 turns nel nuovo contesto di sincronizzazione che è stata aggiunta in .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="da401-232">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="da401-233">Questo valore è impostato per impostazione predefinita nei nuovi progetti in Visual Studio 2012, ma è non possibile impostare se si lavora con un progetto esistente.</span><span class="sxs-lookup"><span data-stu-id="da401-233">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="da401-234">Lavoro Fire-and-Forget</span><span class="sxs-lookup"><span data-stu-id="da401-234">Fire-and-Forget Work</span></span>

<span data-ttu-id="da401-235">Raccomandazione: Quando si gestisce una richiesta all'interno di ASP.NET, evitare l'avvio di lavoro fire-and-forget (tale chiamata al metodo ThreadPool. QueueUserWorkItem o la creazione di un timer che chiama ripetutamente un delegato).</span><span class="sxs-lookup"><span data-stu-id="da401-235">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="da401-236">Se l'applicazione presenta il lavoro fire-and-forget eseguito all'interno di ASP.NET, l'applicazione può ottenere sincronizzato. In qualsiasi momento, può essere eliminato il dominio dell'applicazione che si intende che il processo in corso potrebbe non corrispondere lo stato corrente dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="da401-236">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="da401-237">È consigliabile spostare questo tipo di lavoro all'esterno di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="da401-237">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="da401-238">È possibile utilizzare un processo Web, servizio di Windows o un ruolo di lavoro in Azure per eseguire il lavoro in corso ed eseguire il codice da un altro processo.</span><span class="sxs-lookup"><span data-stu-id="da401-238">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="da401-239">Se è necessario eseguire questa operazione all'interno di ASP.NET, è possibile aggiungere il pacchetto Nuget chiamato [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) per eseguire il codice.</span><span class="sxs-lookup"><span data-stu-id="da401-239">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="da401-240">Corpo entità richiedente</span><span class="sxs-lookup"><span data-stu-id="da401-240">Request Entity Body</span></span>

<span data-ttu-id="da401-241">Raccomandazione: Evitare la lettura di Request. Form o Request.InputStream prima del gestore evento eseguito.</span><span class="sxs-lookup"><span data-stu-id="da401-241">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="da401-242">Non appena possibile è consigliabile leggere da Request. Form o Request.InputStream è durante il gestore evento eseguito.</span><span class="sxs-lookup"><span data-stu-id="da401-242">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="da401-243">In MVC, il Controller è il gestore e l'evento execute è quando viene eseguito il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="da401-243">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="da401-244">In Web Form, la pagina è il gestore ed evento execute è quando viene generato l'evento Page. Init.</span><span class="sxs-lookup"><span data-stu-id="da401-244">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="da401-245">Se si legge il corpo entità della richiesta precedente a evento execute, interferire con l'elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="da401-245">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="da401-246">Se è necessario leggere il corpo di entità di richiesta prima dell'evento execute, utilizzare [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) oppure [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="da401-246">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="da401-247">Quando si usa GetBufferlessInputStream, ottenere il flusso non elaborato dalla richiesta e si assume alcuna responsabilità per l'elaborazione della richiesta intera.</span><span class="sxs-lookup"><span data-stu-id="da401-247">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="da401-248">Dopo aver chiamato GetBufferlessInputStream, Request. Form e Request.InputStream non sono disponibili perché non sono stati compilati da ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="da401-248">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="da401-249">Quando si usa GetBufferedInputStream, ottenere una copia del flusso dalla richiesta.</span><span class="sxs-lookup"><span data-stu-id="da401-249">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="da401-250">Request. Form Request.InputStream rimangono comunque disponibili in un secondo momento nella richiesta poiché ASP.NET consente di popolare a altra copia.</span><span class="sxs-lookup"><span data-stu-id="da401-250">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="da401-251">Response. Redirect e Response</span><span class="sxs-lookup"><span data-stu-id="da401-251">Response.Redirect and Response.End</span></span>

<span data-ttu-id="da401-252">Raccomandazione: Essere consapevoli delle differenze nella modalità di gestione di thread dopo la chiamata [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="da401-252">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="da401-253">Il [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) metodo chiama il metodo Response.</span><span class="sxs-lookup"><span data-stu-id="da401-253">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="da401-254">In un processo sincrono, chiamare Request.Redirect fa sì che il thread corrente viene interrotta immediatamente.</span><span class="sxs-lookup"><span data-stu-id="da401-254">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="da401-255">Tuttavia, in un processo asincrono, la chiamata a Response. Redirect non interrompe il thread corrente, quindi continua l'esecuzione di codice per la richiesta.</span><span class="sxs-lookup"><span data-stu-id="da401-255">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="da401-256">In un processo asincrono, è necessario restituire l'attività dal metodo che deve interrompere l'esecuzione del codice.</span><span class="sxs-lookup"><span data-stu-id="da401-256">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="da401-257">In un progetto MVC, è necessario chiamare non Response. Redirect.</span><span class="sxs-lookup"><span data-stu-id="da401-257">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="da401-258">Al contrario, restituisce un RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="da401-258">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="da401-259">EnableViewState e ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="da401-259">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="da401-260">Consiglio: Usare ViewStateMode, anziché EnableViewState, per fornire un controllo granulare su quali controlli utilizzano lo stato di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="da401-260">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="da401-261">Quando si imposta EnableViewState su false nella direttiva di pagina, lo stato di visualizzazione è disabilitato per tutti i controlli all'interno della pagina e non può essere abilitato.</span><span class="sxs-lookup"><span data-stu-id="da401-261">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="da401-262">Se si vuole attivare lo stato di visualizzazione per solo determinati controlli nella pagina, impostato su disabilitato ViewStateMode per la pagina.</span><span class="sxs-lookup"><span data-stu-id="da401-262">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="da401-263">Quindi, impostare ViewStateMode su abilitato, solo i controlli che devono effettivamente lo stato di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="da401-263">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="da401-264">Abilitando lo stato di visualizzazione per solo i controlli che ne hanno necessità, è possibile ridurre le dimensioni dello stato di visualizzazione per le pagine web.</span><span class="sxs-lookup"><span data-stu-id="da401-264">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="da401-265">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="da401-265">SqlMembershipProvider</span></span>

<span data-ttu-id="da401-266">Consiglio: Usare i provider universali.</span><span class="sxs-lookup"><span data-stu-id="da401-266">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="da401-267">Nei modelli di progetto corrente, SqlMembershipProvider è stato sostituito da [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), disponibile come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="da401-267">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="da401-268">Se si usa SqlMembershipProvider in un progetto che è stato compilato con una versione precedente dei modelli, è necessario passare a Universal Providers.</span><span class="sxs-lookup"><span data-stu-id="da401-268">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="da401-269">I provider universali funziona con tutti i database che sono supportati da Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="da401-269">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="da401-270">Per altre informazioni, vedere [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="da401-270">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="da401-271">Le richieste a esecuzione prolungata (> 110 secondi)</span><span class="sxs-lookup"><span data-stu-id="da401-271">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="da401-272">Consiglio: Usare [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) oppure [SignalR](../../../signalr/index.md) per i client connessi e utilizzare le operazioni dei / o asincrone.</span><span class="sxs-lookup"><span data-stu-id="da401-272">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="da401-273">Le richieste a esecuzione prolungata possono causare risultati imprevedibili e una riduzione delle prestazioni nell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="da401-273">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="da401-274">L'impostazione di timeout predefinito per una richiesta è 110 secondi.</span><span class="sxs-lookup"><span data-stu-id="da401-274">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="da401-275">Se si usa lo stato della sessione con una richiesta a esecuzione prolungata, ASP.NET rilascerà il blocco sull'oggetto sessione dopo 110 secondi.</span><span class="sxs-lookup"><span data-stu-id="da401-275">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="da401-276">Tuttavia, l'applicazione potrebbe essere un'operazione sull'oggetto sessione in corso quando viene rilasciato il blocco e l'operazione potrebbe non essere completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="da401-276">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="da401-277">Se una seconda richiesta da parte dell'utente viene bloccata durante la prima richiesta è in esecuzione, la seconda richiesta può accedere all'oggetto di sessione in uno stato incoerente.</span><span class="sxs-lookup"><span data-stu-id="da401-277">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="da401-278">Se l'applicazione include operazioni dei / o di blocco (o sincrone), l'applicazione sarà non risponda.</span><span class="sxs-lookup"><span data-stu-id="da401-278">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="da401-279">Per migliorare le prestazioni, usare le operazioni dei / o asincrone in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="da401-279">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="da401-280">Inoltre, è possibile usare WebSocket o SignalR per la connessione client al server.</span><span class="sxs-lookup"><span data-stu-id="da401-280">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="da401-281">Queste funzionalità sono progettate per gestire in modo efficiente le richieste a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="da401-281">These features are designed to efficiently handle long-running requests.</span></span>
