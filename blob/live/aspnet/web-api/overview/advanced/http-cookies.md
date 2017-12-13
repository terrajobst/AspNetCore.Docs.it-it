---
uid: web-api/overview/advanced/http-cookies
title: I cookie HTTP in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: e17c51946a268aa13ec035d18dc516928c9f4419
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="c0f3c-102">Cookie HTTP in ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c0f3c-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="c0f3c-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c0f3c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c0f3c-104">In questo argomento viene descritto come inviare e ricevere i cookie HTTP nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="c0f3c-105">Informazioni sui cookie HTTP</span><span class="sxs-lookup"><span data-stu-id="c0f3c-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="c0f3c-106">Questa sezione viene fornita una breve panoramica delle modalità di implementazione dei cookie al livello HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="c0f3c-107">Per informazioni dettagliate, consultare [6265 RFC](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="c0f3c-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="c0f3c-108">Un cookie è una porzione di dati che invia un server nella risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="c0f3c-109">Il client (facoltativamente) archivia il cookie e lo restituisce nel subsequet richieste.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="c0f3c-110">In questo modo il client e server condividere lo stato.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-110">This allows the client and server to share state.</span></span> <span data-ttu-id="c0f3c-111">Per impostare un cookie, il server include un'intestazione Set-Cookie nella risposta.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="c0f3c-112">Il formato di un cookie è una coppia nome-valore, con gli attributi facoltativi.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="c0f3c-113">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c0f3c-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="c0f3c-114">Di seguito è riportato un esempio con attributi:</span><span class="sxs-lookup"><span data-stu-id="c0f3c-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="c0f3c-115">Per restituire un cookie al server, il client di contiene un'intestazione Cookie nelle richieste successive.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-115">To return a cookie to the server, the client inclues a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="c0f3c-116">Una risposta HTTP può includere più intestazioni Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="c0f3c-117">Il client restituisce più cookie con una singola intestazione Cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="c0f3c-118">L'ambito e la durata di un cookie sono controllate dagli attributi seguenti nell'intestazione Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="c0f3c-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="c0f3c-119">**Dominio**: indica al client di dominio che deve ricevere il cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="c0f3c-120">Ad esempio, se il dominio è "example.com", il client restituisce il cookie a ogni sottodominio di example.com. Se non specificato, il dominio è il server di origine.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com. If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="c0f3c-121">**Percorso**: limita il cookie al percorso specificato all'interno del dominio.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-121">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="c0f3c-122">Se non specificato, viene utilizzato il percorso dell'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-122">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="c0f3c-123">**Scadenza**: imposta una data di scadenza del cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-123">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="c0f3c-124">Dopo la scadenza, il client elimina il cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-124">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="c0f3c-125">**Max-Age**: imposta la durata massima dei cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-125">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="c0f3c-126">Quando viene raggiunta la durata massima, il client elimina il cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-126">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="c0f3c-127">Se entrambi `Expires` e `Max-Age` sono impostate, `Max-Age` ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-127">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="c0f3c-128">Se non è impostato, il client elimina il cookie al termine della sessione corrente.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-128">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="c0f3c-129">(Il significato esatto di "sessione" è determinato dall'agente utente).</span><span class="sxs-lookup"><span data-stu-id="c0f3c-129">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="c0f3c-130">Tuttavia, tenere presente che i client possono ignorare i cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-130">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="c0f3c-131">Ad esempio, un utente potrebbe disabilitare i cookie per motivi di privacy.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-131">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="c0f3c-132">I client possono eliminare i cookie prima alla scadenza oppure limitare il numero di cookie memorizzato.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-132">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="c0f3c-133">Per motivi di privacy, i client rifiutare spesso i cookie "third party", dove il dominio non corrisponde il server di origine.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-133">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="c0f3c-134">In breve, il server non deve basarsi su come ottenere nuovamente i cookie che verrà impostato.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-134">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="c0f3c-135">Cookie nel Web API</span><span class="sxs-lookup"><span data-stu-id="c0f3c-135">Cookies in Web API</span></span>

<span data-ttu-id="c0f3c-136">Per aggiungere un cookie a una risposta HTTP, creare un **CookieHeaderValue** istanza che rappresenta il cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-136">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="c0f3c-137">Chiamare quindi il **AddCookies** metodo di estensione, è definito nel **System.NET. http. HttpResponseHeadersExtensions** (classe), per aggiungere il cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-137">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="c0f3c-138">Ad esempio, il codice seguente aggiunge un cookie all'interno di un'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="c0f3c-138">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="c0f3c-139">Si noti che **AddCookies** accetta una matrice di **CookieHeaderValue** istanze.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-139">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="c0f3c-140">Per estrarre i cookie da una richiesta client, chiamare il **GetCookies** metodo:</span><span class="sxs-lookup"><span data-stu-id="c0f3c-140">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="c0f3c-141">Oggetto **CookieHeaderValue** contiene una raccolta di **CookieState** istanze.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-141">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="c0f3c-142">Ogni **CookieState** rappresenta un cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-142">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="c0f3c-143">Utilizzare il metodo dell'indicizzatore per ottenere un **CookieState** in base al nome, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-143">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="c0f3c-144">Dati strutturati Cookie</span><span class="sxs-lookup"><span data-stu-id="c0f3c-144">Structured Cookie Data</span></span>

<span data-ttu-id="c0f3c-145">Molti browser Limita numero di cookie che verrà archiviano &#8212; il numero totale sia il numero per ogni dominio.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-145">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="c0f3c-146">Pertanto, può essere utile inserire i dati strutturati in un unico cookie, anziché impostare più cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-146">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="c0f3c-147">RFC 6265 non definisce la struttura dei dati del cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-147">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="c0f3c-148">Utilizzo di **CookieHeaderValue** (classe), è possibile passare un elenco di coppie nome-valore per i dati del cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-148">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="c0f3c-149">Queste coppie nome-valore sono codificate come dati con codifica URL form nell'intestazione Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="c0f3c-149">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="c0f3c-150">Il codice precedente produce l'intestazione Set-Cookie seguente:</span><span class="sxs-lookup"><span data-stu-id="c0f3c-150">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="c0f3c-151">Il **CookieState** classe fornisce un metodo di un indicizzatore per leggere i valori secondario da un cookie nel messaggio di richiesta:</span><span class="sxs-lookup"><span data-stu-id="c0f3c-151">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="c0f3c-152">Esempio: Impostare e recuperare i cookie in un gestore di messaggi</span><span class="sxs-lookup"><span data-stu-id="c0f3c-152">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="c0f3c-153">Negli esempi precedenti è stati illustrati come utilizzare i cookie dall'interno di un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-153">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="c0f3c-154">Un'altra opzione consiste nell'utilizzare [gestori di messaggi](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="c0f3c-154">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="c0f3c-155">In precedenza nella pipeline di controller vengono richiamati i gestori di messaggi.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-155">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="c0f3c-156">Un gestore di messaggi può leggere i cookie dalla richiesta prima che la richiesta raggiunga il controller oppure aggiungere i cookie nella risposta dopo che il controller generato la risposta.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-156">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="c0f3c-157">Il codice seguente viene illustrato un gestore di messaggi per la creazione degli ID sessione.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-157">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="c0f3c-158">L'ID di sessione viene archiviato in un cookie.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-158">The session ID is stored in a cookie.</span></span> <span data-ttu-id="c0f3c-159">Il gestore verifica la richiesta per il cookie di sessione.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-159">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="c0f3c-160">Se la richiesta non include il cookie, il gestore genera un ID sessione nuovo.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-160">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="c0f3c-161">In entrambi i casi, il gestore memorizza l'ID di sessione nel **HttpRequestMessage.Properties** contenitore delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-161">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="c0f3c-162">Aggiunge inoltre il cookie di sessione nella risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-162">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="c0f3c-163">Questa implementazione non convalida che l'ID di sessione dal client sia stato effettivamente rilasciato dal server.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-163">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="c0f3c-164">Non viene usato come modulo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-164">Don't use it as a form of authentication!</span></span> <span data-ttu-id="c0f3c-165">Il punto dell'esempio è illustrare la gestione dei cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-165">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="c0f3c-166">Un controller è possibile ottenere l'ID di sessione dal **HttpRequestMessage.Properties** contenitore delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="c0f3c-166">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
