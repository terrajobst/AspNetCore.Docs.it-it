---
title: Stato di sessione e dell'applicazione in ASP.NET Core
author: rick-anderson
description: Approcci per la conservazione di applicazione e lo stato utente (sessione) tra le richieste.
keywords: Registrare ASP.NET Core, lo stato dell'applicazione, lo stato della sessione, stringa di query,
ms.author: riande
manager: wpickett
ms.date: 10/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d4d10ef45d562f34c3f8b5ce025abaf763c862d3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="40c80-104">Introduzione allo stato sessione e dell'applicazione ASP.NET di base</span><span class="sxs-lookup"><span data-stu-id="40c80-104">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="40c80-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), e [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="40c80-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="40c80-106">HTTP è un protocollo senza stato.</span><span class="sxs-lookup"><span data-stu-id="40c80-106">HTTP is a stateless protocol.</span></span> <span data-ttu-id="40c80-107">Un server web considera ogni richiesta HTTP come indipendente e non mantiene i valori utente delle richieste precedenti.</span><span class="sxs-lookup"><span data-stu-id="40c80-107">A web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="40c80-108">In questo articolo vengono illustrati diversi modi per mantenere l'applicazione e lo stato della sessione tra le richieste.</span><span class="sxs-lookup"><span data-stu-id="40c80-108">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="40c80-109">Stato della sessione</span><span class="sxs-lookup"><span data-stu-id="40c80-109">Session state</span></span>

<span data-ttu-id="40c80-110">Lo stato della sessione è una funzionalità di ASP.NET Core che è possibile usare per salvare e archiviare i dati utente mentre l'utente visualizza l'app Web.</span><span class="sxs-lookup"><span data-stu-id="40c80-110">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="40c80-111">È costituito da una tabella hash o dizionario sul server, lo stato della sessione mantiene i dati in tutte le richieste da un browser.</span><span class="sxs-lookup"><span data-stu-id="40c80-111">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="40c80-112">I dati della sessione sono supportati da una cache.</span><span class="sxs-lookup"><span data-stu-id="40c80-112">The session data is backed by a cache.</span></span>

<span data-ttu-id="40c80-113">ASP.NET Core mantiene lo stato della sessione, assegnando al client un cookie che contiene l'ID di sessione, che viene inviato al server con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="40c80-113">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="40c80-114">Il server utilizza l'ID di sessione per recuperare i dati di sessione.</span><span class="sxs-lookup"><span data-stu-id="40c80-114">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="40c80-115">Poiché il cookie di sessione specifico del browser, è possibile condividere le sessioni tra browser.</span><span class="sxs-lookup"><span data-stu-id="40c80-115">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="40c80-116">I cookie di sessione vengono eliminati solo al termine della sessione del browser.</span><span class="sxs-lookup"><span data-stu-id="40c80-116">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="40c80-117">Se un cookie viene ricevuto per una sessione scaduta, viene creata una nuova sessione che usa lo stesso cookie di sessione.</span><span class="sxs-lookup"><span data-stu-id="40c80-117">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="40c80-118">Il server mantiene una sessione per un periodo di tempo limitato dopo l'ultima richiesta.</span><span class="sxs-lookup"><span data-stu-id="40c80-118">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="40c80-119">È possibile impostare il timeout della sessione o usare il valore predefinito di 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="40c80-119">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="40c80-120">Lo stato della sessione è ideale per archiviare i dati utente che sono specifico per una determinata sessione, ma non devono essere mantenute in modo permanente.</span><span class="sxs-lookup"><span data-stu-id="40c80-120">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="40c80-121">Dati vengono eliminati dall'archivio di backup, sia quando si chiama `Session.Clear` o scadenza della sessione nell'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="40c80-121">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="40c80-122">Il server non riconosce quando il browser viene chiuso o quando viene eliminato il cookie di sessione.</span><span class="sxs-lookup"><span data-stu-id="40c80-122">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="40c80-123">Non archiviare i dati sensibili nella sessione.</span><span class="sxs-lookup"><span data-stu-id="40c80-123">Do not store sensitive data in session.</span></span> <span data-ttu-id="40c80-124">Il client potrebbe non chiudere il browser e deselezionare il cookie di sessione (e alcuni browser mantenere attivi i cookie di sessione in windows).</span><span class="sxs-lookup"><span data-stu-id="40c80-124">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="40c80-125">Inoltre, una sessione potrebbe non essere limitata a un utente singolo; l'utente successivo potrebbe continuare con la stessa sessione.</span><span class="sxs-lookup"><span data-stu-id="40c80-125">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="40c80-126">Il provider della sessione in memoria archivia i dati di sessione nel server locale.</span><span class="sxs-lookup"><span data-stu-id="40c80-126">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="40c80-127">Se si prevede di eseguire l'app web in una server farm, è necessario utilizzare le sessioni permanenti per associare ogni sessione in un server specifico.</span><span class="sxs-lookup"><span data-stu-id="40c80-127">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="40c80-128">La piattaforma di siti Web di Azure per impostazione predefinita le sessioni permanenti (Application Request Routing o ARR).</span><span class="sxs-lookup"><span data-stu-id="40c80-128">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="40c80-129">Tuttavia, le sessioni permanenti possono influire sulla scalabilità e complicare gli aggiornamenti delle app web.</span><span class="sxs-lookup"><span data-stu-id="40c80-129">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="40c80-130">Un'opzione migliore consiste nell'usare Redis o distribuite di SQL Server memorizza nella cache, che non richiede le sessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="40c80-130">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="40c80-131">Per ulteriori informazioni, vedere [utilizzano una Cache distribuita](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="40c80-131">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="40c80-132">Per informazioni dettagliate sulla configurazione di provider di servizi, vedere [la configurazione di sessione](#configuring-session) più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="40c80-132">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>


<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="40c80-133">TempData</span><span class="sxs-lookup"><span data-stu-id="40c80-133">TempData</span></span>

<span data-ttu-id="40c80-134">ASP.NET MVC di base espone il [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) proprietà in un [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="40c80-134">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="40c80-135">Questa proprietà archivia i dati finché non viene letta.</span><span class="sxs-lookup"><span data-stu-id="40c80-135">This property stores data until it is read.</span></span> <span data-ttu-id="40c80-136">I metodi `Keep` e `Peek` possono essere usati per esaminare i dati senza eliminazione.</span><span class="sxs-lookup"><span data-stu-id="40c80-136">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="40c80-137">`TempData`è particolarmente utile per il reindirizzamento, quando sono necessari dati per più di una singola richiesta.</span><span class="sxs-lookup"><span data-stu-id="40c80-137">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="40c80-138">`TempData`è implementato dal provider TempData, ad esempio, utilizza i cookie o lo stato della sessione.</span><span class="sxs-lookup"><span data-stu-id="40c80-138">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="40c80-139">TempData provider</span><span class="sxs-lookup"><span data-stu-id="40c80-139">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40c80-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40c80-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="40c80-141">In ASP.NET Core 2.0 e versioni successive, per impostazione predefinita viene utilizzato il provider di TempData basato su cookie per memorizzare TempData nei cookie.</span><span class="sxs-lookup"><span data-stu-id="40c80-141">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="40c80-142">I dati del cookie sono codificati con la [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="40c80-142">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="40c80-143">Perché il cookie viene crittografato e in blocchi, il cookie single dimensione limite di ASP.NET Core 1. x non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="40c80-143">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="40c80-144">I dati del cookie non viene compresso perché la compressione dei dati encryped può comportare problemi di sicurezza, ad esempio il [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violazione](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacchi.</span><span class="sxs-lookup"><span data-stu-id="40c80-144">The cookie data is not compressed because compressing encryped data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="40c80-145">Per ulteriori informazioni sul provider TempData basato su cookie, vedere [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="40c80-145">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40c80-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40c80-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="40c80-147">In ASP.NET Core 1.0 e 1.1, il provider di TempData dello stato sessione è il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="40c80-147">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="40c80-148">Scelta di un provider TempData</span><span class="sxs-lookup"><span data-stu-id="40c80-148">Choosing a TempData provider</span></span>

<span data-ttu-id="40c80-149">Scelta di un provider TempData prevede alcune considerazioni, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="40c80-149">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="40c80-150">L'applicazione ha già utilizza lo stato della sessione per altri scopi?</span><span class="sxs-lookup"><span data-stu-id="40c80-150">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="40c80-151">In questo caso, tramite il provider di TempData dello stato sessione non è senza alcun costo aggiuntivo per l'applicazione (a parte la dimensione dei dati).</span><span class="sxs-lookup"><span data-stu-id="40c80-151">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="40c80-152">L'applicazione utilizza TempData solo quando strettamente necessario, per relativamente piccole quantità di dati (fino a 500 byte)?</span><span class="sxs-lookup"><span data-stu-id="40c80-152">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="40c80-153">Se in tal caso, il provider di TempData cookie aggiungerà un costo di piccole dimensioni a ogni richiesta che trasmette TempData.</span><span class="sxs-lookup"><span data-stu-id="40c80-153">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="40c80-154">In caso contrario, il provider di TempData dello stato di sessione può essere utile evitare round trip una grande quantità di dati in ogni richiesta fino a quando non viene utilizzato il TempData.</span><span class="sxs-lookup"><span data-stu-id="40c80-154">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="40c80-155">L'applicazione viene eseguita in una web farm (più server)?</span><span class="sxs-lookup"><span data-stu-id="40c80-155">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="40c80-156">In questo caso, non vi è alcuna configurazione aggiuntiva per utilizzare il provider di TempData cookie.</span><span class="sxs-lookup"><span data-stu-id="40c80-156">If so, there is no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="40c80-157">La maggior parte dei client web (ad esempio i browser web) imporre limiti alla dimensione massima di ciascun cookie, il numero totale di cookie o entrambi.</span><span class="sxs-lookup"><span data-stu-id="40c80-157">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="40c80-158">Pertanto, quando si utilizza il provider di TempData cookie, verificare che l'app non supera questi limiti.</span><span class="sxs-lookup"><span data-stu-id="40c80-158">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="40c80-159">Prendere in considerazione le dimensioni totali dei dati, tenendo conto dei sovraccarichi di crittografia e la suddivisione in blocchi.</span><span class="sxs-lookup"><span data-stu-id="40c80-159">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<span data-ttu-id="40c80-160">Per configurare il provider TempData per un'applicazione, registrare un'implementazione del provider TempData in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="40c80-160">To configure the TempData provider for an application, register a TempData provider implementation in `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddMvc()
        .AddSessionStateTempDataProvider();

    // The Session State TempData Provider requires adding the session state service
    services.AddSession();
}
```

## <a name="query-strings"></a><span data-ttu-id="40c80-161">Stringhe di query</span><span class="sxs-lookup"><span data-stu-id="40c80-161">Query strings</span></span>

<span data-ttu-id="40c80-162">È possibile passare una quantità limitata di dati da una richiesta a un altro, aggiungerlo alla stringa di query della nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="40c80-162">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="40c80-163">Ciò è utile per l'acquisizione dello stato in modo persistente che consente i collegamenti con stato incorporato da condividere tramite posta elettronica o social network.</span><span class="sxs-lookup"><span data-stu-id="40c80-163">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="40c80-164">Tuttavia, per questo motivo, non utilizzare le stringhe di query per i dati sensibili.</span><span class="sxs-lookup"><span data-stu-id="40c80-164">However, for this reason,  you should never use query strings for sensitive data.</span></span> <span data-ttu-id="40c80-165">Oltre a facilmente condiviso, inclusi i dati nelle stringhe di query possono creare le opportunità di [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacchi, che possono indurre gli utenti a siti dannosi mentre autenticato.</span><span class="sxs-lookup"><span data-stu-id="40c80-165">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="40c80-166">Gli utenti malintenzionati possono quindi intercettare i dati utente dall'app o richiedere azioni dannose per conto dell'utente.</span><span class="sxs-lookup"><span data-stu-id="40c80-166">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="40c80-167">Qualsiasi stato mantenuto application o session necessario proteggere da attacchi CSRF.</span><span class="sxs-lookup"><span data-stu-id="40c80-167">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="40c80-168">Per ulteriori informazioni sugli attacchi CSRF, vedere [attacchi di prevenzione Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="40c80-168">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="40c80-169">Dati post e campi nascosti</span><span class="sxs-lookup"><span data-stu-id="40c80-169">Post data and hidden fields</span></span>

<span data-ttu-id="40c80-170">Dati possono essere salvati nei campi del form nascosto e inviati nuovamente la richiesta successiva.</span><span class="sxs-lookup"><span data-stu-id="40c80-170">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="40c80-171">Questo è comune in un form con più pagine.</span><span class="sxs-lookup"><span data-stu-id="40c80-171">This is common in multipage forms.</span></span> <span data-ttu-id="40c80-172">Tuttavia, poiché il client può potenzialmente manomettere i dati, il server deve sempre sottoporlo.</span><span class="sxs-lookup"><span data-stu-id="40c80-172">However, because the  client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="40c80-173">Cookie</span><span class="sxs-lookup"><span data-stu-id="40c80-173">Cookies</span></span>

<span data-ttu-id="40c80-174">I cookie consentono di archiviare dati specifici dell'utente nelle applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="40c80-174">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="40c80-175">Poiché i cookie vengono inviati con ogni richiesta, la dimensione deve essere mantenuta al minimo.</span><span class="sxs-lookup"><span data-stu-id="40c80-175">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="40c80-176">Idealmente, solo un identificatore deve essere archiviato in un cookie con i dati effettivi archiviati nel server.</span><span class="sxs-lookup"><span data-stu-id="40c80-176">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="40c80-177">La maggior parte dei browser limitare i cookie a 4096 byte.</span><span class="sxs-lookup"><span data-stu-id="40c80-177">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="40c80-178">Inoltre, solo un numero limitato di cookie è disponibile per ogni dominio.</span><span class="sxs-lookup"><span data-stu-id="40c80-178">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="40c80-179">Poiché i cookie sono soggette alla manomissione, devono essere convalidati nel server.</span><span class="sxs-lookup"><span data-stu-id="40c80-179">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="40c80-180">Sebbene la durata del cookie in un client è soggetto a scadenza e l'intervento dell'utente, sono in genere la forma più durevole di persistenza dei dati nel client.</span><span class="sxs-lookup"><span data-stu-id="40c80-180">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="40c80-181">I cookie vengono spesso utilizzati per la personalizzazione, in cui il contenuto viene personalizzato per un utente noto.</span><span class="sxs-lookup"><span data-stu-id="40c80-181">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="40c80-182">Poiché l'utente viene identificato solo e non è stato autenticato nella maggior parte dei casi, è possibile proteggere in genere un cookie archiviando il nome utente, nome dell'account o un ID utente univoco (ad esempio un GUID) nel cookie.</span><span class="sxs-lookup"><span data-stu-id="40c80-182">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="40c80-183">È quindi possibile utilizzare il cookie di accesso all'infrastruttura di personalizzazione utente di un sito.</span><span class="sxs-lookup"><span data-stu-id="40c80-183">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="40c80-184">HttpContext. Items</span><span class="sxs-lookup"><span data-stu-id="40c80-184">HttpContext.Items</span></span>

<span data-ttu-id="40c80-185">Il `Items` raccolta è una buona posizione in cui archiviare i dati necessari solo durante l'elaborazione una particolare richiesta.</span><span class="sxs-lookup"><span data-stu-id="40c80-185">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="40c80-186">Contenuto della raccolta viene eliminato dopo ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="40c80-186">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="40c80-187">Il `Items` insieme è particolarmente utile come un modo per componenti o middleware per comunicare quando operano in momenti diversi durante una richiesta e in alcun modo diretto per passare i parametri.</span><span class="sxs-lookup"><span data-stu-id="40c80-187">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="40c80-188">Per ulteriori informazioni, vedere [utilizzo HttpContext. Items](#working-with-httpcontextitems), più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="40c80-188">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="40c80-189">Cache</span><span class="sxs-lookup"><span data-stu-id="40c80-189">Cache</span></span>

<span data-ttu-id="40c80-190">La memorizzazione nella cache è un modo efficiente per archiviare e recuperare dati.</span><span class="sxs-lookup"><span data-stu-id="40c80-190">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="40c80-191">È possibile controllare la durata di elementi memorizzati nella cache in base al tempo e altre considerazioni.</span><span class="sxs-lookup"><span data-stu-id="40c80-191">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="40c80-192">Altre informazioni, vedere [la memorizzazione nella cache](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="40c80-192">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="40c80-193">Utilizzo con lo stato della sessione</span><span class="sxs-lookup"><span data-stu-id="40c80-193">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="40c80-194">Configurazione della sessione</span><span class="sxs-lookup"><span data-stu-id="40c80-194">Configuring Session</span></span>

<span data-ttu-id="40c80-195">Il `Microsoft.AspNetCore.Session` pacchetto fornisce il middleware per la gestione dello stato sessione.</span><span class="sxs-lookup"><span data-stu-id="40c80-195">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="40c80-196">Per abilitare il middleware di sessione, `Startup`deve contenere:</span><span class="sxs-lookup"><span data-stu-id="40c80-196">To enable the session middleware, `Startup`must contain:</span></span>

- <span data-ttu-id="40c80-197">Uno del [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) cache in memoria.</span><span class="sxs-lookup"><span data-stu-id="40c80-197">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="40c80-198">Il `IDistributedCache` implementazione viene utilizzata come archivio di backup per sessione.</span><span class="sxs-lookup"><span data-stu-id="40c80-198">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="40c80-199">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) chiama, che richiede il pacchetto NuGet "Microsoft.AspNetCore.Session".</span><span class="sxs-lookup"><span data-stu-id="40c80-199">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="40c80-200">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) chiamare.</span><span class="sxs-lookup"><span data-stu-id="40c80-200">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="40c80-201">Il codice seguente viene illustrato come configurare il provider della sessione in memoria.</span><span class="sxs-lookup"><span data-stu-id="40c80-201">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40c80-202">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40c80-202">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40c80-203">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40c80-203">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="40c80-204">È possibile fare riferimento a una sessione da `HttpContext` una volta installato e configurato.</span><span class="sxs-lookup"><span data-stu-id="40c80-204">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="40c80-205">Se si tenta di accedere a `Session` prima `UseSession` è stato chiamato, l'eccezione `InvalidOperationException: Session has not been configured for this application or request` viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="40c80-205">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="40c80-206">Se si tenta di creare un nuovo `Session` (vale a dire alcun cookie di sessione non creato) dopo che si è già iniziato la scrittura di `Response` flusso, l'eccezione `InvalidOperationException: The session cannot be established after the response has started` viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="40c80-206">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="40c80-207">L'eccezione è reperibile nel log di server web. non verrà visualizzata nel browser.</span><span class="sxs-lookup"><span data-stu-id="40c80-207">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="40c80-208">Il caricamento asincrono di sessione</span><span class="sxs-lookup"><span data-stu-id="40c80-208">Loading Session asynchronously</span></span> 

<span data-ttu-id="40c80-209">Il provider di sessione predefinito in ASP.NET Core carica il record di sessione sottostante [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store in modo asincrono solo se il [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) metodo viene chiamato in modo esplicito prima  il `TryGetValue`, `Set`, o `Remove` metodi.</span><span class="sxs-lookup"><span data-stu-id="40c80-209">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="40c80-210">Se `LoadAsync` non viene chiamato prima di tutto, sottostante il record di sessione viene caricato in modo sincrono, che possono incidere sul possibilità dell'app di scala.</span><span class="sxs-lookup"><span data-stu-id="40c80-210">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="40c80-211">Per applicare questo modello applicazioni, eseguire il wrapping di [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) e [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementazioni con le versioni che generano un'eccezione se il `LoadAsync` metodo non è chiamato prima `TryGetValue`, `Set`, o `Remove`.</span><span class="sxs-lookup"><span data-stu-id="40c80-211">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="40c80-212">Registrare le versioni di cui effettua il wrapping nel contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="40c80-212">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="40c80-213">Dettagli di implementazione</span><span class="sxs-lookup"><span data-stu-id="40c80-213">Implementation Details</span></span>

<span data-ttu-id="40c80-214">Sessione utilizza un cookie per tenere traccia e identificare le richieste provenienti da un solo browser.</span><span class="sxs-lookup"><span data-stu-id="40c80-214">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="40c80-215">Per impostazione predefinita, questo cookie denominato ". AspNet.Session"e utilizza un percorso di"/".</span><span class="sxs-lookup"><span data-stu-id="40c80-215">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="40c80-216">Poiché il valore predefinito di cookie non viene specificato un dominio, non renderlo disponibile per lo script sul lato client nella pagina (perché `CookieHttpOnly` per impostazione predefinita `true`).</span><span class="sxs-lookup"><span data-stu-id="40c80-216">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="40c80-217">Per eseguire l'override delle impostazioni predefinite di sessione, utilizzare `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="40c80-217">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40c80-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40c80-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40c80-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40c80-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="40c80-220">Il server utilizza il `IdleTimeout` proprietà per determinare quanto tempo una sessione può rimanere inattiva prima che il relativo contenuto viene abbandonato.</span><span class="sxs-lookup"><span data-stu-id="40c80-220">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="40c80-221">Questa proprietà è indipendente dalla scadenza del cookie.</span><span class="sxs-lookup"><span data-stu-id="40c80-221">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="40c80-222">Ogni richiesta che passa attraverso il middleware di sessione (leggere o scrivere) Reimposta il timeout.</span><span class="sxs-lookup"><span data-stu-id="40c80-222">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="40c80-223">Poiché `Session` è *non blocca il thread*, se due richieste tentano di modificare il contenuto della sessione, l'ultima prevale sulla prima.</span><span class="sxs-lookup"><span data-stu-id="40c80-223">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="40c80-224">`Session`viene implementato come un *sessione coerente*, il che significa che tutto il contenuto viene archiviato insieme.</span><span class="sxs-lookup"><span data-stu-id="40c80-224">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="40c80-225">Due richieste che modificano parti diverse della sessione (chiavi diverse) potrebbero comunque avere un'influenza reciproca.</span><span class="sxs-lookup"><span data-stu-id="40c80-225">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="40c80-226">Impostazione e recupero di valori di sessione</span><span class="sxs-lookup"><span data-stu-id="40c80-226">Setting and getting Session values</span></span>

<span data-ttu-id="40c80-227">Sessione avviene tramite il `Session` proprietà `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="40c80-227">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="40c80-228">Questa proprietà è un [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementazione.</span><span class="sxs-lookup"><span data-stu-id="40c80-228">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="40c80-229">L'esempio seguente mostra l'impostazione e recupero di un tipo int e una stringa:</span><span class="sxs-lookup"><span data-stu-id="40c80-229">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="40c80-230">Se si aggiungono i seguenti metodi di estensione, è possibile impostare e ottenere gli oggetti serializzabili alla sessione:</span><span class="sxs-lookup"><span data-stu-id="40c80-230">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="40c80-231">Nell'esempio seguente viene illustrato come impostare e ottenere un oggetto serializzabile:</span><span class="sxs-lookup"><span data-stu-id="40c80-231">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="40c80-232">Utilizzo di HttpContext. Items</span><span class="sxs-lookup"><span data-stu-id="40c80-232">Working with HttpContext.Items</span></span>

<span data-ttu-id="40c80-233">Il `HttpContext` astrazione fornisce supporto per una raccolta di dizionario di tipo `IDictionary<object, object>`, denominato `Items`.</span><span class="sxs-lookup"><span data-stu-id="40c80-233">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="40c80-234">Questa raccolta è disponibile dall'inizio di un *HttpRequest* e vengono eliminati alla fine di ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="40c80-234">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="40c80-235">È possibile accedervi tramite l'assegnazione di un valore a una voce con chiave, o richiedendo il valore per una chiave specifica.</span><span class="sxs-lookup"><span data-stu-id="40c80-235">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="40c80-236">Nell'esempio seguente, [Middleware](middleware.md) aggiunge `isVerified` per il `Items` insieme.</span><span class="sxs-lookup"><span data-stu-id="40c80-236">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="40c80-237">In un secondo momento nella pipeline, un altro middleware Impossibile accedervi:</span><span class="sxs-lookup"><span data-stu-id="40c80-237">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="40c80-238">Per middleware che verrà utilizzato solo da una singola app `string` chiavi sono accettabili.</span><span class="sxs-lookup"><span data-stu-id="40c80-238">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="40c80-239">Tuttavia, middleware che verrà condivisi tra le applicazioni debba utilizzare chiavi dell'oggetto univoco per evitare qualsiasi rischio di conflitti di chiave.</span><span class="sxs-lookup"><span data-stu-id="40c80-239">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="40c80-240">Se si sta sviluppando middleware che devono essere utilizzati in più applicazioni, utilizzare una chiave oggetto univoco definita nella classe middleware come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="40c80-240">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

<span data-ttu-id="40c80-241">Altro codice può accedere al valore archiviato in `HttpContext.Items` utilizzando la chiave esposta dalla classe middleware:</span><span class="sxs-lookup"><span data-stu-id="40c80-241">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="40c80-242">Questo approccio presenta inoltre il vantaggio di eliminare la ripetizione di stringhe"magiche" in più posizioni nel codice.</span><span class="sxs-lookup"><span data-stu-id="40c80-242">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="40c80-243">Dati di stato dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="40c80-243">Application state data</span></span>

<span data-ttu-id="40c80-244">Utilizzare [Dependency Injection](xref:fundamentals/dependency-injection) per rendere i dati disponibili a tutti gli utenti:</span><span class="sxs-lookup"><span data-stu-id="40c80-244">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="40c80-245">Definire un servizio che contiene i dati (ad esempio, una classe denominata `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="40c80-245">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="40c80-246">Aggiungere la classe di servizio per `ConfigureServices` (ad esempio `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="40c80-246">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="40c80-247">Utilizzare la classe di servizio dati in ogni controller:</span><span class="sxs-lookup"><span data-stu-id="40c80-247">Consume the data service class in each controller:</span></span>

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

### <a name="common-errors-when-working-with-session"></a><span data-ttu-id="40c80-248">Errori comuni quando si lavora con sessione</span><span class="sxs-lookup"><span data-stu-id="40c80-248">Common errors when working with session</span></span>

* <span data-ttu-id="40c80-249">"Impossibile risolvere il servizio per il tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' durante il tentativo di attivazione 'Microsoft.AspNetCore.Session.DistributedSessionStore'".</span><span class="sxs-lookup"><span data-stu-id="40c80-249">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="40c80-250">Questo è generalmente causato dal mancato configurare almeno un `IDistributedCache` implementazione.</span><span class="sxs-lookup"><span data-stu-id="40c80-250">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="40c80-251">Per ulteriori informazioni, vedere [utilizzano una Cache distribuita](xref:performance/caching/distributed) e [In memoria la memorizzazione nella cache](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="40c80-251">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

### <a name="additional-resources"></a><span data-ttu-id="40c80-252">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="40c80-252">Additional Resources</span></span>


* [<span data-ttu-id="40c80-253">ASP.NET Core 1. x: esempio di codice usato in questo documento</span><span class="sxs-lookup"><span data-stu-id="40c80-253">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="40c80-254">ASP.NET Core 2. x: esempio di codice usato in questo documento</span><span class="sxs-lookup"><span data-stu-id="40c80-254">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
