---
title: Stato di sessioni e app in ASP.NET Core
author: rick-anderson
description: Individuare gli approcci per mantenere lo stato di sessioni e app tra le richieste.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: a510e4f49e158203dd7c5e1e0bd28472541f7925
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836337"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="39361-103">Stato di sessioni e app in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39361-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="39361-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="39361-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="39361-105">HTTP è un protocollo senza stato.</span><span class="sxs-lookup"><span data-stu-id="39361-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="39361-106">Senza eseguire ulteriori passaggi, le richieste HTTP sono messaggi indipendenti che non mantengono i valori dell'utente o lo stato delle app.</span><span class="sxs-lookup"><span data-stu-id="39361-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="39361-107">In questo articolo vengono descritti diversi approcci per mantenere lo stato di app e dati utente tra le richieste.</span><span class="sxs-lookup"><span data-stu-id="39361-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="39361-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="39361-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="39361-109">Gestione dello stato</span><span class="sxs-lookup"><span data-stu-id="39361-109">State management</span></span>

<span data-ttu-id="39361-110">Lo stato può essere archiviato usando diversi approcci.</span><span class="sxs-lookup"><span data-stu-id="39361-110">State can be stored using several approaches.</span></span> <span data-ttu-id="39361-111">Ogni approccio è descritto di seguito in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="39361-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="39361-112">Approccio con risorsa di archiviazione</span><span class="sxs-lookup"><span data-stu-id="39361-112">Storage approach</span></span> | <span data-ttu-id="39361-113">Meccanismo di archiviazione</span><span class="sxs-lookup"><span data-stu-id="39361-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="39361-114">Cookie</span><span class="sxs-lookup"><span data-stu-id="39361-114">Cookies</span></span>](#cookies) | <span data-ttu-id="39361-115">Cookie HTTP (possono includere dati archiviati usando il codice app lato server)</span><span class="sxs-lookup"><span data-stu-id="39361-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="39361-116">Stato della sessione</span><span class="sxs-lookup"><span data-stu-id="39361-116">Session state</span></span>](#session-state) | <span data-ttu-id="39361-117">Cookie HTTP e codice app lato server</span><span class="sxs-lookup"><span data-stu-id="39361-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="39361-118">TempData</span><span class="sxs-lookup"><span data-stu-id="39361-118">TempData</span></span>](#tempdata) | <span data-ttu-id="39361-119">Cookie HTTP o stato della sessione</span><span class="sxs-lookup"><span data-stu-id="39361-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="39361-120">Stringhe di query</span><span class="sxs-lookup"><span data-stu-id="39361-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="39361-121">Stringhe di query HTTP</span><span class="sxs-lookup"><span data-stu-id="39361-121">HTTP query strings</span></span> |
| [<span data-ttu-id="39361-122">Campi nascosti</span><span class="sxs-lookup"><span data-stu-id="39361-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="39361-123">Campi dei form HTTP</span><span class="sxs-lookup"><span data-stu-id="39361-123">HTTP form fields</span></span> |
| [<span data-ttu-id="39361-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="39361-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="39361-125">Codice app lato server</span><span class="sxs-lookup"><span data-stu-id="39361-125">Server-side app code</span></span> |
| [<span data-ttu-id="39361-126">Cache</span><span class="sxs-lookup"><span data-stu-id="39361-126">Cache</span></span>](#cache) | <span data-ttu-id="39361-127">Codice app lato server</span><span class="sxs-lookup"><span data-stu-id="39361-127">Server-side app code</span></span> |
| [<span data-ttu-id="39361-128">Inserimento dipendenze</span><span class="sxs-lookup"><span data-stu-id="39361-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="39361-129">Codice app lato server</span><span class="sxs-lookup"><span data-stu-id="39361-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="39361-130">Cookie</span><span class="sxs-lookup"><span data-stu-id="39361-130">Cookies</span></span>

<span data-ttu-id="39361-131">I cookie archiviano i dati tra le richieste.</span><span class="sxs-lookup"><span data-stu-id="39361-131">Cookies store data across requests.</span></span> <span data-ttu-id="39361-132">Poiché i cookie vengono inviati con ogni richiesta, la loro dimensione deve essere mantenuta al minimo.</span><span class="sxs-lookup"><span data-stu-id="39361-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="39361-133">Idealmente, in un cookie deve essere archiviato solo un identificatore con i dati archiviati dall'app.</span><span class="sxs-lookup"><span data-stu-id="39361-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="39361-134">La maggior parte dei browser limita la dimensione dei cookie a 4096 byte.</span><span class="sxs-lookup"><span data-stu-id="39361-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="39361-135">Per ogni dominio è disponibile solo un numero limitato di cookie.</span><span class="sxs-lookup"><span data-stu-id="39361-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="39361-136">Poiché i cookie possono essere manomessi, devono essere convalidati dall'app.</span><span class="sxs-lookup"><span data-stu-id="39361-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="39361-137">I cookie possono essere eliminati dagli utenti e scadono nei client.</span><span class="sxs-lookup"><span data-stu-id="39361-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="39361-138">Tuttavia, i cookie sono in genere la forma più durevole di salvataggio permanente dei dati nel client.</span><span class="sxs-lookup"><span data-stu-id="39361-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="39361-139">I cookie vengono spesso usati per la personalizzazione, ovvero il contenuto viene personalizzato per un utente noto.</span><span class="sxs-lookup"><span data-stu-id="39361-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="39361-140">L'utente viene solo identificato e non autenticato nella maggior parte dei casi.</span><span class="sxs-lookup"><span data-stu-id="39361-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="39361-141">Nel cookie può essere memorizzato il nome dell'utente, il nome dell'account o l'ID utente univoco, ad esempio un GUID.</span><span class="sxs-lookup"><span data-stu-id="39361-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="39361-142">È quindi possibile usare il cookie per accedere alle impostazioni personalizzate dell'utente, ad esempio il colore di sfondo preferito per il sito Web.</span><span class="sxs-lookup"><span data-stu-id="39361-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="39361-143">Tenere presente il [Regolamento generale sulla protezione dei dati dell'Unione Europea](https://ec.europa.eu/info/law/law-topic/data-protection) quando si emettono i cookie e si gestisce la protezione dei dati.</span><span class="sxs-lookup"><span data-stu-id="39361-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="39361-144">Per altre informazioni, vedere il [supporto per il Regolamento generale sulla protezione dei dati in ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="39361-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="39361-145">Stato sessione</span><span class="sxs-lookup"><span data-stu-id="39361-145">Session state</span></span>

<span data-ttu-id="39361-146">Lo stato della sessione è uno scenario di ASP.NET Core per l'archiviazione dei dati utente mentre l'utente visualizza un'app Web.</span><span class="sxs-lookup"><span data-stu-id="39361-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="39361-147">Lo stato della sessione usa un archivio gestito dall'app per rendere persistenti i dati tra le richieste provenienti da un client.</span><span class="sxs-lookup"><span data-stu-id="39361-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="39361-148">I dati della sessione sono supportati da una cache e considerati dati temporanei: il sito deve continuare a funzionare senza i dati della sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span> <span data-ttu-id="39361-149">I dati critici dell'applicazione devono essere archiviati nel database utente e memorizzati nella cache della sessione solo al fine di ottimizzare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="39361-149">Critical application data should be stored in the user database and cached in session only as a performance optimization.</span></span>

> [!NOTE]
> <span data-ttu-id="39361-150">La sessione non è supportata nelle app [SignalR](xref:signalr/index) poiché un [hub SignalR](xref:signalr/hubs) può essere eseguito indipendentemente da un contesto HTTP.</span><span class="sxs-lookup"><span data-stu-id="39361-150">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="39361-151">Ad esempio, ciò può verificarsi quando una lunga richiesta di polling viene mantenuta aperta da un hub oltre la durata del contesto HTTP della richiesta.</span><span class="sxs-lookup"><span data-stu-id="39361-151">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="39361-152">ASP.NET Core mantiene lo stato della sessione assegnando un cookie al client che contiene un ID sessione. Tale ID viene inviato all'app con ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="39361-152">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="39361-153">L'app usa l'ID sessione per recuperare i dati della sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-153">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="39361-154">Lo stato della sessione presenta i comportamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="39361-154">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="39361-155">Dato che il cookie di sessione è specifico per il browser, le sessioni non vengono condivise tra i browser.</span><span class="sxs-lookup"><span data-stu-id="39361-155">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="39361-156">I cookie di sessione vengono eliminati al termine della sessione del browser.</span><span class="sxs-lookup"><span data-stu-id="39361-156">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="39361-157">Se viene ricevuto un cookie per una sessione scaduta, viene creata una nuova sessione che usa lo stesso cookie di sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-157">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="39361-158">Le sessioni vuote non vengono conservate: nella sessione deve essere impostato almeno un valore perché la sessione diventi permanente tra le richieste.</span><span class="sxs-lookup"><span data-stu-id="39361-158">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="39361-159">Se una sessione non viene conservata, viene generato un nuovo ID sessione per ogni nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="39361-159">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="39361-160">L'app conserva una sessione per un periodo di tempo limitato dopo l'ultima richiesta.</span><span class="sxs-lookup"><span data-stu-id="39361-160">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="39361-161">L'app imposta il timeout della sessione o usa il valore predefinito, pari a 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="39361-161">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="39361-162">Lo stato sessione è ideale per archiviare i dati utente specifici per una determinata sessione, ma nel caso in cui i dati non richiedano un'archiviazione permanente tra le sessioni.</span><span class="sxs-lookup"><span data-stu-id="39361-162">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="39361-163">I dati della sessione vengono eliminati quando viene chiamata l'implementazione [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) o alla scadenza della sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-163">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="39361-164">Non esiste un meccanismo predefinito per indicare al codice app che un browser client è stato chiuso o che il cookie di sessione viene eliminato o è scaduto nel client.</span><span class="sxs-lookup"><span data-stu-id="39361-164">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>
<span data-ttu-id="39361-165">I modelli di pagina ASP.NET Core MVC e Razor includono il supporto del [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="39361-165">The ASP.NET Core MVC and Razor pages templates include support for [General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span> <span data-ttu-id="39361-166">[I cookie di stato sessione non sono indispensabili](xref:security/gdpr#tempdata-provider-and-session-state-cookies-are-not-essential), lo stato sessione non è attivo quando la verifica è disabilitata.</span><span class="sxs-lookup"><span data-stu-id="39361-166">[Session state cookies aren't essential](xref:security/gdpr#tempdata-provider-and-session-state-cookies-are-not-essential), session state isn't functional when tracking is disabled.</span></span>

> [!WARNING]
> <span data-ttu-id="39361-167">Evitare di archiviare dati sensibili nello stato della sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-167">Don't store sensitive data in session state.</span></span> <span data-ttu-id="39361-168">L'utente potrebbe non chiudere il browser e cancellare il cookie di sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-168">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="39361-169">Alcuni browser mantengono i cookie di sessione validi tra diverse finestre del browser.</span><span class="sxs-lookup"><span data-stu-id="39361-169">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="39361-170">Una sessione può non essere limitata a un solo utente: in tal caso l'utente successivo potrebbe continuare a sfogliare l'app usando lo stesso cookie di sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-170">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="39361-171">Il provider di cache in memoria archivia i dati della sessione nella memoria del server in cui si trova l'app.</span><span class="sxs-lookup"><span data-stu-id="39361-171">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="39361-172">In uno scenario di server farm:</span><span class="sxs-lookup"><span data-stu-id="39361-172">In a server farm scenario:</span></span>

* <span data-ttu-id="39361-173">Usare *sessioni permanenti* per associare ogni sessione a un'istanza specifica dell'app in un singolo server.</span><span class="sxs-lookup"><span data-stu-id="39361-173">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="39361-174">Il [Servizio app di Azure](https://azure.microsoft.com/services/app-service/) usa il modulo [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) per applicare le sessioni permanenti per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="39361-174">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="39361-175">Tuttavia le sessioni permanenti possono compromettere la scalabilità e complicare gli aggiornamenti delle app Web.</span><span class="sxs-lookup"><span data-stu-id="39361-175">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="39361-176">Un approccio migliore è usare la cache distribuita Redis o SQL Server, che non richiede sessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="39361-176">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="39361-177">Per ulteriori informazioni, vedere <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="39361-177">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="39361-178">Il cookie di sessione viene crittografato con [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span><span class="sxs-lookup"><span data-stu-id="39361-178">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="39361-179">La protezione dati deve essere configurata correttamente in modo da leggere i cookie di sessione in ogni computer.</span><span class="sxs-lookup"><span data-stu-id="39361-179">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="39361-180">Per altre informazioni, vedere <xref:security/data-protection/introduction> e [Provider di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="39361-180">For more information, see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="39361-181">Configurare lo stato della sessione</span><span class="sxs-lookup"><span data-stu-id="39361-181">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="39361-182">Con il pacchetto [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/), incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), è disponibile il middleware per la gestione dello stato della sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-182">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="39361-183">Per abilitare il middleware della sessione, `Startup` deve contenere:</span><span class="sxs-lookup"><span data-stu-id="39361-183">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="39361-184">Il pacchetto [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) offre il middleware per la gestione dello stato della sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-184">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="39361-185">Per abilitare il middleware della sessione, `Startup` deve contenere:</span><span class="sxs-lookup"><span data-stu-id="39361-185">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="39361-186">Una delle cache di memoria [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache).</span><span class="sxs-lookup"><span data-stu-id="39361-186">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="39361-187">L'implementazione `IDistributedCache` viene usata come archivio di backup per la sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-187">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="39361-188">Per ulteriori informazioni, vedere <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="39361-188">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="39361-189">Una chiamata ad [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="39361-189">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="39361-190">Una chiamata a [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span><span class="sxs-lookup"><span data-stu-id="39361-190">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="39361-191">Il codice seguente indica come configurare il provider della sessione in memoria con un'implementazione in memoria predefinita di `IDistributedCache`:</span><span class="sxs-lookup"><span data-stu-id="39361-191">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

<span data-ttu-id="39361-192">L'ordine del middleware è importante.</span><span class="sxs-lookup"><span data-stu-id="39361-192">The order of middleware is important.</span></span> <span data-ttu-id="39361-193">Nell'esempio precedente si verifica un'eccezione `InvalidOperationException` se `UseSession` viene chiamata dopo `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="39361-193">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="39361-194">Per altre informazioni, vedere la sezione relativa all'[ordine del middleware](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="39361-194">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

<span data-ttu-id="39361-195">[HttpContext. Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) è disponibile dopo che è stato configurato lo stato della sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-195">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="39361-196">Non è possibile accedere a `HttpContext.Session` prima di chiamare `UseSession`.</span><span class="sxs-lookup"><span data-stu-id="39361-196">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="39361-197">Non è possibile creare un nuovo cookie di sessione dopo che l'app ha iniziato la scrittura nel flusso di risposte.</span><span class="sxs-lookup"><span data-stu-id="39361-197">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="39361-198">L'eccezione viene registrata nel log del server Web e non è visualizzata nel browser.</span><span class="sxs-lookup"><span data-stu-id="39361-198">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="39361-199">Caricare lo stato della sessione in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="39361-199">Load session state asynchronously</span></span>

<span data-ttu-id="39361-200">Il provider di sessione predefinito in ASP.NET Core carica in modo asincrono i record di sessione dall'archivio di backup [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) sottostante solo se il metodo [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) viene chiamato in modo esplicito prima dei metodi [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set) o [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove).</span><span class="sxs-lookup"><span data-stu-id="39361-200">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="39361-201">Se `LoadAsync` non viene chiamato per primo, il record di sessione sottostante viene caricato in modo sincrono e ciò può compromettere le prestazioni su larga scala.</span><span class="sxs-lookup"><span data-stu-id="39361-201">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="39361-202">Per fare in modo che le app implementino questo criterio, eseguire il wrapping delle implementazioni [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) e [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) con versioni che generano un'eccezione se il metodo `LoadAsync` non viene chiamato prima di `TryGetValue`, `Set` o `Remove`.</span><span class="sxs-lookup"><span data-stu-id="39361-202">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="39361-203">Registrare le versioni con wrapping nel contenitore di servizi.</span><span class="sxs-lookup"><span data-stu-id="39361-203">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="39361-204">Opzioni di sessione</span><span class="sxs-lookup"><span data-stu-id="39361-204">Session options</span></span>

<span data-ttu-id="39361-205">Per eseguire l'override delle impostazioni predefinite di sessione, usare [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span><span class="sxs-lookup"><span data-stu-id="39361-205">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="39361-206">Opzione</span><span class="sxs-lookup"><span data-stu-id="39361-206">Option</span></span> | <span data-ttu-id="39361-207">Description</span><span class="sxs-lookup"><span data-stu-id="39361-207">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="39361-208">Cookie</span><span class="sxs-lookup"><span data-stu-id="39361-208">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="39361-209">Determina le impostazioni usate per creare il cookie.</span><span class="sxs-lookup"><span data-stu-id="39361-209">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="39361-210">Per impostazione predefinita, [Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) è [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="39361-210">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="39361-211">Per impostazione predefinita, [Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) è [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="39361-211">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="39361-212">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) per impostazione predefinita è [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span><span class="sxs-lookup"><span data-stu-id="39361-212">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="39361-213">Per [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="39361-213">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="39361-214">Per [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) il valore predefinito è `false`.</span><span class="sxs-lookup"><span data-stu-id="39361-214">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="39361-215">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="39361-215">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="39361-216">Il valore `IdleTimeout` indica quanto tempo la sessione può rimanere inattiva prima che il relativo contenuto venga abbandonato.</span><span class="sxs-lookup"><span data-stu-id="39361-216">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="39361-217">Ogni accesso alla sessione reimposta il timeout.</span><span class="sxs-lookup"><span data-stu-id="39361-217">Each session access resets the timeout.</span></span> <span data-ttu-id="39361-218">Si noti che questa impostazione vale solo per il contenuto della sessione, non per il cookie.</span><span class="sxs-lookup"><span data-stu-id="39361-218">This setting only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="39361-219">Il valore predefinito è 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="39361-219">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="39361-220">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="39361-220">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="39361-221">L'intervallo di tempo massimo consentito per caricare una sessione dall'archivio o per eseguirne il commit nell'archivio.</span><span class="sxs-lookup"><span data-stu-id="39361-221">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="39361-222">Questa impostazione può essere applicabile solo alle operazioni asincrone.</span><span class="sxs-lookup"><span data-stu-id="39361-222">This setting may only apply to asynchronous operations.</span></span> <span data-ttu-id="39361-223">Questo timeout può essere disattivato usando [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span><span class="sxs-lookup"><span data-stu-id="39361-223">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="39361-224">Il valore predefinito è 1 minuto.</span><span class="sxs-lookup"><span data-stu-id="39361-224">The default is 1 minute.</span></span> |

<span data-ttu-id="39361-225">Session usa un cookie per tenere traccia e identificare le richieste provenienti da un solo browser.</span><span class="sxs-lookup"><span data-stu-id="39361-225">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="39361-226">Per impostazione predefinita, questo cookie è denominato `.AspNetCore.Session` e usa il percorso `/`.</span><span class="sxs-lookup"><span data-stu-id="39361-226">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="39361-227">Poiché il cookie predefinito non specifica un dominio, non viene reso disponibile allo script lato client nella pagina (perché il valore predefinito di [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) è `true`).</span><span class="sxs-lookup"><span data-stu-id="39361-227">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="39361-228">Opzione</span><span class="sxs-lookup"><span data-stu-id="39361-228">Option</span></span> | <span data-ttu-id="39361-229">Description</span><span class="sxs-lookup"><span data-stu-id="39361-229">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="39361-230">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="39361-230">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="39361-231">Determina il dominio usato per creare il cookie.</span><span class="sxs-lookup"><span data-stu-id="39361-231">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="39361-232">`CookieDomain` non è impostato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="39361-232">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="39361-233">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="39361-233">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="39361-234">Determina se il browser deve consentire l'accesso al cookie a JavaScript sul lato client.</span><span class="sxs-lookup"><span data-stu-id="39361-234">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="39361-235">Il valore predefinito è `true`, ovvero il cookie viene passato solo alle richieste HTTP e non è disponibile per lo script nella pagina.</span><span class="sxs-lookup"><span data-stu-id="39361-235">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="39361-236">CookieName</span><span class="sxs-lookup"><span data-stu-id="39361-236">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="39361-237">Determina il nome del cookie usato per salvare in modo permanente l'ID di sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-237">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="39361-238">Il valore predefinito è [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="39361-238">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="39361-239">CookiePath</span><span class="sxs-lookup"><span data-stu-id="39361-239">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="39361-240">Determina il percorso usato per creare il cookie.</span><span class="sxs-lookup"><span data-stu-id="39361-240">Determines the path used to create the cookie.</span></span> <span data-ttu-id="39361-241">Il valore predefinito è [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="39361-241">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="39361-242">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="39361-242">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="39361-243">Determina se il cookie deve essere trasmesso solo alle richieste HTTPS.</span><span class="sxs-lookup"><span data-stu-id="39361-243">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="39361-244">Il valore predefinito è [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span><span class="sxs-lookup"><span data-stu-id="39361-244">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="39361-245">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="39361-245">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="39361-246">Il valore `IdleTimeout` indica quanto tempo la sessione può rimanere inattiva prima che il relativo contenuto venga abbandonato.</span><span class="sxs-lookup"><span data-stu-id="39361-246">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="39361-247">Ogni accesso alla sessione reimposta il timeout.</span><span class="sxs-lookup"><span data-stu-id="39361-247">Each session access resets the timeout.</span></span> <span data-ttu-id="39361-248">Si noti che questo vale solo per il contenuto della sessione, non per il cookie.</span><span class="sxs-lookup"><span data-stu-id="39361-248">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="39361-249">Il valore predefinito è 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="39361-249">The default is 20 minutes.</span></span> |

<span data-ttu-id="39361-250">Session usa un cookie per tenere traccia e identificare le richieste provenienti da un solo browser.</span><span class="sxs-lookup"><span data-stu-id="39361-250">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="39361-251">Per impostazione predefinita, questo cookie è denominato `.AspNet.Session` e usa il percorso `/`.</span><span class="sxs-lookup"><span data-stu-id="39361-251">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="39361-252">Per eseguire l'override delle impostazioni predefinite della sessione con cookie, usare `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="39361-252">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

<span data-ttu-id="39361-253">L'app usa la proprietà [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) per determinare quanto tempo una sessione può rimanere inattiva prima che il relativo contenuto nella cache del server venga abbandonato.</span><span class="sxs-lookup"><span data-stu-id="39361-253">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="39361-254">Questa proprietà è indipendente dalla scadenza del cookie.</span><span class="sxs-lookup"><span data-stu-id="39361-254">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="39361-255">Ogni richiesta che passa attraverso il [middleware di sessione](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) reimposta il timeout.</span><span class="sxs-lookup"><span data-stu-id="39361-255">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="39361-256">Lo stato della sessione è *non di blocco*.</span><span class="sxs-lookup"><span data-stu-id="39361-256">Session state is *non-locking*.</span></span> <span data-ttu-id="39361-257">Se due richieste tentano simultaneamente di modificare il contenuto di una sessione, l'ultima richiesta sostituisce la prima.</span><span class="sxs-lookup"><span data-stu-id="39361-257">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="39361-258">`Session` viene implementata come *sessione coerente*, ovvero tutto il contenuto viene archiviato insieme.</span><span class="sxs-lookup"><span data-stu-id="39361-258">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="39361-259">Quando due richieste tentano di modificare diversi valori della sessione, l'ultima richiesta può sostituire le modifiche apportate alla sessione dalla prima.</span><span class="sxs-lookup"><span data-stu-id="39361-259">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="39361-260">Impostare e ottenere i valori della sessione</span><span class="sxs-lookup"><span data-stu-id="39361-260">Set and get Session values</span></span>

<span data-ttu-id="39361-261">Lo stato della sessione è accessibile da una classe [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) di Razor Pages o una classe [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) di MVC con [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span><span class="sxs-lookup"><span data-stu-id="39361-261">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="39361-262">Questa proprietà è un'implementazione di [ISession](/dotnet/api/microsoft.aspnetcore.http.isession).</span><span class="sxs-lookup"><span data-stu-id="39361-262">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="39361-263">L'implementazione `ISession` offre diversi metodi di estensione per impostare e recuperare i valori interi e stringa.</span><span class="sxs-lookup"><span data-stu-id="39361-263">The `ISession` implementation provides several extension methods to set and retrieve integer and string values.</span></span> <span data-ttu-id="39361-264">I metodi di estensione si trovano nello spazio dei nomi [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (aggiungere un'istruzione `using Microsoft.AspNetCore.Http;` per ottenere l'accesso ai metodi di estensione) quando al pacchetto [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) fa riferimento il progetto.</span><span class="sxs-lookup"><span data-stu-id="39361-264">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="39361-265">Entrambi i pacchetti sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="39361-265">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="39361-266">L'implementazione `ISession` offre diversi metodi di estensione per impostare e recuperare i valori interi e stringa.</span><span class="sxs-lookup"><span data-stu-id="39361-266">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="39361-267">I metodi di estensione si trovano nello spazio dei nomi [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (aggiungere un'istruzione `using Microsoft.AspNetCore.Http;` per ottenere l'accesso ai metodi di estensione) quando al pacchetto [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) fa riferimento il progetto.</span><span class="sxs-lookup"><span data-stu-id="39361-267">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="39361-268">Metodi di estensione `ISession`:</span><span class="sxs-lookup"><span data-stu-id="39361-268">`ISession` extension methods:</span></span>

* [<span data-ttu-id="39361-269">Get(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="39361-269">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="39361-270">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="39361-270">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="39361-271">GetString(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="39361-271">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="39361-272">SetInt32(ISession, String, Int32)</span><span class="sxs-lookup"><span data-stu-id="39361-272">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="39361-273">SetString(ISession, String, String)</span><span class="sxs-lookup"><span data-stu-id="39361-273">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="39361-274">Nell'esempio seguente viene recuperato il valore di sessione per la chiave `IndexModel.SessionKeyName` (`_Name` nell'app di esempio) in una pagina di Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="39361-274">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="39361-275">L'esempio seguente illustra come impostare e ottenere un intero e una stringa:</span><span class="sxs-lookup"><span data-stu-id="39361-275">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

<span data-ttu-id="39361-276">Tutti i dati della sessione devono essere serializzati per abilitare uno scenario di cache distribuita, anche quando si usa la cache in memoria.</span><span class="sxs-lookup"><span data-stu-id="39361-276">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="39361-277">Sono disponibili serializzatori di stringa e numero (vedere i metodi e i metodi di estensione di [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span><span class="sxs-lookup"><span data-stu-id="39361-277">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="39361-278">I tipi complessi devono essere serializzati dall'utente usando un altro meccanismo, ad esempio JSON.</span><span class="sxs-lookup"><span data-stu-id="39361-278">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="39361-279">Aggiungere i seguenti metodi di estensione per impostare e ottenere oggetti serializzabili:</span><span class="sxs-lookup"><span data-stu-id="39361-279">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="39361-280">L'esempio seguente illustra come impostare e ottenere un oggetto serializzabile con i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="39361-280">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="39361-281">TempData</span><span class="sxs-lookup"><span data-stu-id="39361-281">TempData</span></span>

<span data-ttu-id="39361-282">ASP.NET Core espone la [proprietà TempData di un modello di pagina di Razor Pages](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) o la proprietà [TempData di un controller MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span><span class="sxs-lookup"><span data-stu-id="39361-282">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="39361-283">Questa proprietà archivia i dati finché non viene letta.</span><span class="sxs-lookup"><span data-stu-id="39361-283">This property stores data until it's read.</span></span> <span data-ttu-id="39361-284">I metodi [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) e [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) possono essere usati per esaminare i dati senza eliminazione.</span><span class="sxs-lookup"><span data-stu-id="39361-284">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="39361-285">TempData è particolarmente utile per il reindirizzamento quando i dati sono necessari per più di una richiesta singola.</span><span class="sxs-lookup"><span data-stu-id="39361-285">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="39361-286">La proprietà TempData viene implementata dai provider TempData usando i cookie o lo stato sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-286">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="39361-287">Provider TempData</span><span class="sxs-lookup"><span data-stu-id="39361-287">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="39361-288">In ASP.NET Core 2.0 o versioni successive il provider TempData basato su cookie viene usato per impostazione predefinita per memorizzare TempData nei cookie.</span><span class="sxs-lookup"><span data-stu-id="39361-288">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="39361-289">I dati del cookie vengono crittografati tramite [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), codificati con [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder) e quindi suddivisi in blocchi.</span><span class="sxs-lookup"><span data-stu-id="39361-289">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="39361-290">Dato che il cookie è suddiviso in blocchi, il limite di dimensione per un singolo cookie di ASP.NET Core 1.x non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="39361-290">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="39361-291">I dati del cookie non vengono compressi perché la compressione di dati crittografati può comportare problemi di sicurezza, ad esempio con attacchi [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)).</span><span class="sxs-lookup"><span data-stu-id="39361-291">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="39361-292">Per altre informazioni sul provider TempData basato sui cookie, vedere [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span><span class="sxs-lookup"><span data-stu-id="39361-292">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="39361-293">In ASP.NET Core 1.0 e 1.1 il provider TempData con stato sessione è il provider predefinito.</span><span class="sxs-lookup"><span data-stu-id="39361-293">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="39361-294">Scegliere un provider TempData</span><span class="sxs-lookup"><span data-stu-id="39361-294">Choose a TempData provider</span></span>

<span data-ttu-id="39361-295">La scelta di un provider TempData implica diverse considerazioni, tra cui:</span><span class="sxs-lookup"><span data-stu-id="39361-295">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="39361-296">L'app utilizza già lo stato sessione?</span><span class="sxs-lookup"><span data-stu-id="39361-296">Does the app already use session state?</span></span> <span data-ttu-id="39361-297">Se sì, l'uso del provider TempData con stato sessione non comporta un carico aggiuntivo per l'app (indipendentemente dalla dimensione dei dati).</span><span class="sxs-lookup"><span data-stu-id="39361-297">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="39361-298">L'app usa TempData solo quando necessario e per quantità di dati relativamente piccole (fino a 500 byte)?</span><span class="sxs-lookup"><span data-stu-id="39361-298">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="39361-299">Se sì, il provider TempData con cookie aggiunge un piccolo carico di lavoro a ogni richiesta che include TempData.</span><span class="sxs-lookup"><span data-stu-id="39361-299">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="39361-300">Se no, il provider TempData con stato sessione può essere utile per evitare sequenze di andata e ritorno di grandi quantità di dati in ogni richiesta fino a quando il contenuto TempData non viene consumato.</span><span class="sxs-lookup"><span data-stu-id="39361-300">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="39361-301">L'app viene eseguita in un server farm in più server?</span><span class="sxs-lookup"><span data-stu-id="39361-301">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="39361-302">Se sì, non è necessaria un'ulteriore configurazione per usare il provider TempData con cookie di fuori della protezione dei dati (vedere <xref:security/data-protection/introduction> e [Provider di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers)).</span><span class="sxs-lookup"><span data-stu-id="39361-302">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="39361-303">La maggior parte dei client Web (ad esempio i Web browser) impone limiti per la dimensione massima di ogni cookie, il numero totale di cookie o entrambe le impostazioni.</span><span class="sxs-lookup"><span data-stu-id="39361-303">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="39361-304">Se si usa il provider TempData con cookie, verificare che l'app non superi tali limiti.</span><span class="sxs-lookup"><span data-stu-id="39361-304">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="39361-305">Considerare la dimensione totale dei dati.</span><span class="sxs-lookup"><span data-stu-id="39361-305">Consider the total size of the data.</span></span> <span data-ttu-id="39361-306">Considerare l'aumento delle dimensioni del cookie dovuto a crittografia e suddivisione in blocchi.</span><span class="sxs-lookup"><span data-stu-id="39361-306">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="39361-307">Configurare il provider TempData</span><span class="sxs-lookup"><span data-stu-id="39361-307">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="39361-308">Il provider TempData basato su cookie è abilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="39361-308">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="39361-309">Per abilitare il provider TempData basato sulla sessione, usare il metodo di estensione [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider):</span><span class="sxs-lookup"><span data-stu-id="39361-309">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="39361-310">Il codice classe `Startup` seguente configura il provider TempData basato sulla sessione:</span><span class="sxs-lookup"><span data-stu-id="39361-310">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

<span data-ttu-id="39361-311">L'ordine del middleware è importante.</span><span class="sxs-lookup"><span data-stu-id="39361-311">The order of middleware is important.</span></span> <span data-ttu-id="39361-312">Nell'esempio precedente si verifica un'eccezione `InvalidOperationException` se `UseSession` viene chiamata dopo `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="39361-312">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="39361-313">Per altre informazioni, vedere la sezione relativa all'[ordine del middleware](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="39361-313">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="39361-314">Se la destinazione è .NET Framework e si usa il provider TempData basato sulla sessione, aggiungere il pacchetto [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) al progetto.</span><span class="sxs-lookup"><span data-stu-id="39361-314">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="39361-315">Stringhe di query</span><span class="sxs-lookup"><span data-stu-id="39361-315">Query strings</span></span>

<span data-ttu-id="39361-316">È possibile passare una quantità limitata di dati da una richiesta a un'altra aggiungendo i dati alla stringa di query della nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="39361-316">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="39361-317">Questo è utile per l'acquisizione dello stato con una modalità persistente, che consente la condivisione dei collegamenti con stato incorporato tramite posta elettronica o social network.</span><span class="sxs-lookup"><span data-stu-id="39361-317">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="39361-318">Poiché le stringhe di query dell'URL sono pubbliche, non usare mai le stringhe di query per i dati sensibili.</span><span class="sxs-lookup"><span data-stu-id="39361-318">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="39361-319">OItre alle condivisioni involontarie, i dati presenti nelle stringhe di query possono essere oggetto di attacchi di [falsificazione della richiesta tra siti (CSRF, Cross-Site Request Forgery)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), che ingannano gli utenti autenticati invitandoli a visitare siti pericolosi.</span><span class="sxs-lookup"><span data-stu-id="39361-319">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="39361-320">Gli utenti malintenzionati possono quindi sottrarre dati utente dall'app o eseguire azioni dannose per conto dell'utente.</span><span class="sxs-lookup"><span data-stu-id="39361-320">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="39361-321">Qualsiasi stato dell'app o della sessione mantenuto deve garantire la protezione dagli attacchi CSRF.</span><span class="sxs-lookup"><span data-stu-id="39361-321">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="39361-322">Per altre informazioni, vedere [Prevenire attacchi tramite richieste intersito false (XSRF/CSRF)](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="39361-322">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="39361-323">Campi nascosti</span><span class="sxs-lookup"><span data-stu-id="39361-323">Hidden fields</span></span>

<span data-ttu-id="39361-324">I dati possono essere salvati in campi modulo nascosti e pubblicati di nuovo nella richiesta successiva.</span><span class="sxs-lookup"><span data-stu-id="39361-324">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="39361-325">Questo si verifica spesso nei moduli a più pagine.</span><span class="sxs-lookup"><span data-stu-id="39361-325">This is common in multi-page forms.</span></span> <span data-ttu-id="39361-326">Poiché il client potenzialmente può alterare i dati, l'app deve sempre ripetere la convalida dei dati archiviati nei campi nascosti.</span><span class="sxs-lookup"><span data-stu-id="39361-326">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="39361-327">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="39361-327">HttpContext.Items</span></span>

<span data-ttu-id="39361-328">La raccolta [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) viene usata per archiviare i dati durante l'elaborazione di una singola richiesta.</span><span class="sxs-lookup"><span data-stu-id="39361-328">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="39361-329">Il contenuto della raccolta viene rimosso al termine dell'elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="39361-329">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="39361-330">La raccolta `Items` spesso viene usata per consentire ai componenti o al middleware di comunicare quando operano in momenti diversi durante una richiesta e non è disponibile un metodo diretto per passare i parametri.</span><span class="sxs-lookup"><span data-stu-id="39361-330">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="39361-331">Nell'esempio seguente [middleware](xref:fundamentals/middleware/index) aggiunge `isVerified` alla raccolta `Items`.</span><span class="sxs-lookup"><span data-stu-id="39361-331">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="39361-332">In punto successivo della pipeline un altro middleware può accedere al valore di `isVerified`:</span><span class="sxs-lookup"><span data-stu-id="39361-332">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="39361-333">Per il middleware che viene usato da un'unica app sono accettabili le chiavi `string`.</span><span class="sxs-lookup"><span data-stu-id="39361-333">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="39361-334">Il middleware condiviso tra le istanze dell'app deve usare chiavi oggetto univoche per evitare conflitti di chiavi.</span><span class="sxs-lookup"><span data-stu-id="39361-334">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="39361-335">L'esempio seguente illustra come usare una chiave oggetto univoca definita in una classe middleware:</span><span class="sxs-lookup"><span data-stu-id="39361-335">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

<span data-ttu-id="39361-336">Altri elementi di codice possono accedere al valore archiviato in `HttpContext.Items` usando la chiave esposta dalla classe middleware:</span><span class="sxs-lookup"><span data-stu-id="39361-336">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="39361-337">Questo approccio ha anche il vantaggio di eliminare l'uso di stringhe chiave nel codice.</span><span class="sxs-lookup"><span data-stu-id="39361-337">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="39361-338">Cache</span><span class="sxs-lookup"><span data-stu-id="39361-338">Cache</span></span>

<span data-ttu-id="39361-339">La memorizzazione nella cache è un modo efficiente per archiviare e recuperare dati.</span><span class="sxs-lookup"><span data-stu-id="39361-339">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="39361-340">L'app può controllare la durata degli elementi della cache.</span><span class="sxs-lookup"><span data-stu-id="39361-340">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="39361-341">I dati memorizzati nella cache non sono associati a una richiesta, un utente o una sessione specifici.</span><span class="sxs-lookup"><span data-stu-id="39361-341">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="39361-342">**Prestare attenzione a non memorizzare nella cache i dati specifici dell'utente che possono essere recuperati dalle richieste di altri utenti.**</span><span class="sxs-lookup"><span data-stu-id="39361-342">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="39361-343">Per ulteriori informazioni, vedere <xref:performance/caching/response>.</span><span class="sxs-lookup"><span data-stu-id="39361-343">For more information, see <xref:performance/caching/response>.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="39361-344">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="39361-344">Dependency Injection</span></span>

<span data-ttu-id="39361-345">Usare [Dependency Injection](xref:fundamentals/dependency-injection) (Inserimento di dipendenze) per rendere i dati disponibili a tutti gli utenti:</span><span class="sxs-lookup"><span data-stu-id="39361-345">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="39361-346">Definire un servizio che contiene i dati.</span><span class="sxs-lookup"><span data-stu-id="39361-346">Define a service containing the data.</span></span> <span data-ttu-id="39361-347">Ad esempio, una classe denominata `MyAppData` è definita come segue:</span><span class="sxs-lookup"><span data-stu-id="39361-347">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="39361-348">Aggiungere la classe del servizio a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="39361-348">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="39361-349">Usare la classe del servizio dati:</span><span class="sxs-lookup"><span data-stu-id="39361-349">Consume the data service class:</span></span>

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a><span data-ttu-id="39361-350">Errori comuni</span><span class="sxs-lookup"><span data-stu-id="39361-350">Common errors</span></span>

* <span data-ttu-id="39361-351">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'." (Risoluzione servizio non riuscita per il tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' durante il tentativo di attivazione di 'Microsoft.AspNetCore.Session.DistributedSessionStore').</span><span class="sxs-lookup"><span data-stu-id="39361-351">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="39361-352">Questo errore dipende in genere dal fatto che almeno un'implementazione `IDistributedCache` non è stata configurata.</span><span class="sxs-lookup"><span data-stu-id="39361-352">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="39361-353">Per altre informazioni, vedere <xref:performance/caching/distributed> e <xref:performance/caching/memory>.</span><span class="sxs-lookup"><span data-stu-id="39361-353">For more information, see <xref:performance/caching/distributed> and <xref:performance/caching/memory>.</span></span>

* <span data-ttu-id="39361-354">Se il middleware di sessione non riesce a rendere permanente una sessione (ad esempio se l'archivio di backup non è disponibile), registra l'eccezione e la richiesta continua normalmente.</span><span class="sxs-lookup"><span data-stu-id="39361-354">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="39361-355">Ciò provoca un comportamento imprevedibile.</span><span class="sxs-lookup"><span data-stu-id="39361-355">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="39361-356">Ad esempio, un utente archivia un carrello acquisti nella sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-356">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="39361-357">L'utente aggiunge un articolo al carrello ma il commit ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="39361-357">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="39361-358">L'app non riconosce l'errore e segnala all'utente che l'articolo è stato aggiunto al carrello anche se non è vero.</span><span class="sxs-lookup"><span data-stu-id="39361-358">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="39361-359">L'approccio consigliato per verificare la presenza di errori è chiamare `await feature.Session.CommitAsync();` dal codice dell'app quando l'app termina di scrivere nella sessione.</span><span class="sxs-lookup"><span data-stu-id="39361-359">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="39361-360">`CommitAsync` genera un'eccezione se l'archivio di backup non è disponibile.</span><span class="sxs-lookup"><span data-stu-id="39361-360">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="39361-361">Se `CommitAsync` ha esito negativo, l'app è in grado di elaborare l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="39361-361">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="39361-362">`LoadAsync` viene generata nelle stesse condizioni in cui l'archivio dati non è disponibile.</span><span class="sxs-lookup"><span data-stu-id="39361-362">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39361-363">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="39361-363">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
