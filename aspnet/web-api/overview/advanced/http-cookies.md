---
uid: web-api/overview/advanced/http-cookies
title: I cookie HTTP nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/17/2012
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 61e0c47efdd92a3a0b329930aeec757b446eb9b8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833430"
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="a8525-102">Cookie HTTP nell'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a8525-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="a8525-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a8525-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a8525-104">In questo argomento viene descritto come inviare e ricevere i cookie HTTP nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="a8525-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="a8525-105">Informazioni sull'utilizzo di cookie HTTP</span><span class="sxs-lookup"><span data-stu-id="a8525-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="a8525-106">Questa sezione viene fornita una breve panoramica di come vengono implementati i cookie a livello HTTP.</span><span class="sxs-lookup"><span data-stu-id="a8525-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="a8525-107">Per informazioni dettagliate, consultare [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="a8525-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="a8525-108">Un cookie è costituito da dati che invia un server nella risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a8525-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="a8525-109">Il client (facoltativamente) archivia il cookie e lo restituisce nel subsequet richieste.</span><span class="sxs-lookup"><span data-stu-id="a8525-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="a8525-110">In questo modo il client e server condividere lo stato.</span><span class="sxs-lookup"><span data-stu-id="a8525-110">This allows the client and server to share state.</span></span> <span data-ttu-id="a8525-111">Per impostare un cookie, il server include un'intestazione Set-Cookie nella risposta.</span><span class="sxs-lookup"><span data-stu-id="a8525-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="a8525-112">Il formato di un cookie è una coppia nome-valore, con gli attributi facoltativi.</span><span class="sxs-lookup"><span data-stu-id="a8525-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="a8525-113">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a8525-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="a8525-114">Di seguito è riportato un esempio con attributi:</span><span class="sxs-lookup"><span data-stu-id="a8525-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="a8525-115">Per restituire un cookie al server, il client include un'intestazione Cookie nelle richieste successive.</span><span class="sxs-lookup"><span data-stu-id="a8525-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="a8525-116">Una risposta HTTP può includere più intestazioni di Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="a8525-117">Il client restituisce più cookie con una singola intestazione Cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="a8525-118">L'ambito e la durata di un cookie sono controllate da attributi seguenti nell'intestazione Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="a8525-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="a8525-119">**Dominio**: indica al client di dominio che deve ricevere il cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="a8525-120">Ad esempio, se il dominio è "example.com", il client restituisce il cookie a ogni sottodominio di example.com.</span><span class="sxs-lookup"><span data-stu-id="a8525-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="a8525-121">Se non specificato, il dominio è il server di origine.</span><span class="sxs-lookup"><span data-stu-id="a8525-121">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="a8525-122">**Percorso**: limita il cookie nel percorso specificato all'interno del dominio.</span><span class="sxs-lookup"><span data-stu-id="a8525-122">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="a8525-123">Se non specificato, viene utilizzato il percorso dell'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="a8525-123">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="a8525-124">**Scadenza**: imposta una data di scadenza del cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-124">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="a8525-125">Il client elimina il cookie alla scadenza.</span><span class="sxs-lookup"><span data-stu-id="a8525-125">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="a8525-126">**Max-Age**: imposta la durata massima per il cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-126">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="a8525-127">Il client elimina il cookie quando raggiunge la durata massima.</span><span class="sxs-lookup"><span data-stu-id="a8525-127">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="a8525-128">Se entrambe `Expires` e `Max-Age` sono impostate, `Max-Age` ha la precedenza.</span><span class="sxs-lookup"><span data-stu-id="a8525-128">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="a8525-129">Se non è impostato, il client elimina il cookie al termine della sessione corrente.</span><span class="sxs-lookup"><span data-stu-id="a8525-129">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="a8525-130">(Il significato esatto di "sessione" è determinato dall'agente utente).</span><span class="sxs-lookup"><span data-stu-id="a8525-130">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="a8525-131">Tuttavia, tenere presente che i client potrebbero ignorare i cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-131">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="a8525-132">Ad esempio, un utente può disabilitare i cookie per motivi di privacy.</span><span class="sxs-lookup"><span data-stu-id="a8525-132">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="a8525-133">I client possono eliminare i cookie prima di scadere, o limitare il numero di cookie archiviato.</span><span class="sxs-lookup"><span data-stu-id="a8525-133">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="a8525-134">Per motivi di privacy, i client spesso rifiutare i cookie "third party", dove il dominio non corrisponde il server di origine.</span><span class="sxs-lookup"><span data-stu-id="a8525-134">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="a8525-135">In breve, il server necessario non affidarsi a ricevendo i cookie che verrà impostato.</span><span class="sxs-lookup"><span data-stu-id="a8525-135">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="a8525-136">Cookie in API Web</span><span class="sxs-lookup"><span data-stu-id="a8525-136">Cookies in Web API</span></span>

<span data-ttu-id="a8525-137">Per aggiungere un cookie a una risposta HTTP, creare un **CookieHeaderValue** istanza che rappresenta il cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-137">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="a8525-138">Chiamare quindi il **AddCookies** metodo di estensione, che viene definito nel **System.NET. http. HttpResponseHeadersExtensions** (classe), aggiungere il cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-138">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="a8525-139">Ad esempio, il codice seguente aggiunge un cookie all'interno di un'azione del controller:</span><span class="sxs-lookup"><span data-stu-id="a8525-139">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="a8525-140">Si noti che **AddCookies** accetta una matrice di **CookieHeaderValue** istanze.</span><span class="sxs-lookup"><span data-stu-id="a8525-140">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="a8525-141">Per estrarre i cookie dalla richiesta del client, chiamare il **GetCookies** metodo:</span><span class="sxs-lookup"><span data-stu-id="a8525-141">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="a8525-142">Oggetto **CookieHeaderValue** contiene una raccolta di **CookieState** istanze.</span><span class="sxs-lookup"><span data-stu-id="a8525-142">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="a8525-143">Ciascuna **CookieState** rappresenta un cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-143">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="a8525-144">Usare il metodo di un indicizzatore per ottenere un **CookieState** in base al nome, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="a8525-144">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="a8525-145">Dati dei Cookie strutturato</span><span class="sxs-lookup"><span data-stu-id="a8525-145">Structured Cookie Data</span></span>

<span data-ttu-id="a8525-146">Molti browser Limita numero di cookie che si archivierà&#8212;sia il numero totale e il numero per ogni dominio.</span><span class="sxs-lookup"><span data-stu-id="a8525-146">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="a8525-147">Pertanto, può essere utile inserire i dati strutturati in un cookie single, anziché impostare più cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-147">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="a8525-148">RFC 6265 non definisce la struttura dei dati del cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-148">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="a8525-149">Usando il **CookieHeaderValue** (classe), è possibile passare un elenco di coppie nome-valore per i dati del cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-149">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="a8525-150">Queste coppie nome-valore sono codificate come dati del form con codifica URL nell'intestazione Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="a8525-150">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="a8525-151">Il codice precedente produce l'intestazione Set-Cookie seguente:</span><span class="sxs-lookup"><span data-stu-id="a8525-151">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="a8525-152">Il **CookieState** classe fornisce un metodo di un indicizzatore per la lettura dei valori secondario da un cookie nel messaggio di richiesta:</span><span class="sxs-lookup"><span data-stu-id="a8525-152">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="a8525-153">Esempio: Impostare e recuperare i cookie in un gestore di messaggi</span><span class="sxs-lookup"><span data-stu-id="a8525-153">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="a8525-154">Negli esempi precedenti è stato illustrato come utilizzare i cookie provenienti da all'interno di un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="a8525-154">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="a8525-155">Un'altra opzione consiste nell'usare [gestori di messaggi](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="a8525-155">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="a8525-156">In precedenza nella pipeline di controller vengono richiamati i gestori di messaggi.</span><span class="sxs-lookup"><span data-stu-id="a8525-156">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="a8525-157">Un gestore di messaggi può leggere i cookie dalla richiesta prima che la richiesta raggiunga il controller o aggiungere i cookie nella risposta dopo che il controller ha generato la risposta.</span><span class="sxs-lookup"><span data-stu-id="a8525-157">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="a8525-158">Il codice seguente illustra un gestore di messaggi per la creazione degli ID sessione.</span><span class="sxs-lookup"><span data-stu-id="a8525-158">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="a8525-159">L'ID di sessione viene archiviato in un cookie.</span><span class="sxs-lookup"><span data-stu-id="a8525-159">The session ID is stored in a cookie.</span></span> <span data-ttu-id="a8525-160">Il gestore verifica la richiesta per il cookie di sessione.</span><span class="sxs-lookup"><span data-stu-id="a8525-160">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="a8525-161">Se la richiesta non include il cookie, il gestore genera un ID sessione nuovo.</span><span class="sxs-lookup"><span data-stu-id="a8525-161">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="a8525-162">In entrambi i casi, il gestore memorizza l'ID di sessione nel **HttpRequestMessage.Properties** contenitore delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="a8525-162">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="a8525-163">Aggiunge anche il cookie di sessione nella risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a8525-163">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="a8525-164">Questa implementazione non convalida che l'ID di sessione dal client sia stato effettivamente rilasciato dal server.</span><span class="sxs-lookup"><span data-stu-id="a8525-164">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="a8525-165">Non usarlo come una forma di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="a8525-165">Don't use it as a form of authentication!</span></span> <span data-ttu-id="a8525-166">Il punto dell'esempio è illustrare la gestione dei cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="a8525-166">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="a8525-167">Un controller può ottenere l'ID di sessione dal **HttpRequestMessage.Properties** contenitore delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="a8525-167">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
