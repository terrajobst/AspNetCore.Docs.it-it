---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Pagine Web ASP.NET (Razor) rapida API riferimento | Documenti Microsoft
author: tfitzmac
description: "Questa pagina contiene un elenco con brevi esempi di oggetti utilizzati più di frequente, le proprietà e metodi per la programmazione di ASP.NET Web Pages con sintassi Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 35f91f4dbea4881d9dabc4ab7c6b96dbb6a01ea2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="a6d0e-103">ASP.NET Web Pages riferimento API rapida (Razor)</span><span class="sxs-lookup"><span data-stu-id="a6d0e-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>
====================
<span data-ttu-id="a6d0e-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a6d0e-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a6d0e-105">Questa pagina contiene un elenco con brevi esempi di oggetti utilizzati più di frequente, le proprietà e metodi per la programmazione di ASP.NET Web Pages con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="a6d0e-106">Le descrizioni contrassegnate con "(v2)" sono stati introdotti in ASP.NET Web Pages versione 2.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="a6d0e-107">Per la documentazione di riferimento API, vedere il [la documentazione di riferimento di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=208659) su MSDN.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="a6d0e-108">Versioni del software</span><span class="sxs-lookup"><span data-stu-id="a6d0e-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="a6d0e-109">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a6d0e-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a6d0e-110">In questa esercitazione funziona anche con ASP.NET Web Pages 2 e pagine Web ASP.NET 1.0 (ad eccezione delle funzionalità contrassegnate v2).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>


<span data-ttu-id="a6d0e-111">Questa pagina contiene informazioni di riferimento per le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a6d0e-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="a6d0e-112">Classi</span><span class="sxs-lookup"><span data-stu-id="a6d0e-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="a6d0e-113">Dati</span><span class="sxs-lookup"><span data-stu-id="a6d0e-113">Data</span></span>](#Data)
- [<span data-ttu-id="a6d0e-114">Helper</span><span class="sxs-lookup"><span data-stu-id="a6d0e-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="a6d0e-115">Convalida</span><span class="sxs-lookup"><span data-stu-id="a6d0e-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="a6d0e-116">Classi</span><span class="sxs-lookup"><span data-stu-id="a6d0e-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="a6d0e-117">Contiene dati che possono essere condiviso da tutte le pagine dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="a6d0e-118">È possibile utilizzare dinamica `App` proprietà per accedere agli stessi dati, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="a6d0e-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="a6d0e-119">Converte un valore stringa in un valore booleano (true/false).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="a6d0e-120">Restituisce false o il valore specificato se la stringa non rappresentano true o false.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="a6d0e-121">Converte un valore di stringa a data/ora.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-121">Converts a string value to date/time.</span></span> <span data-ttu-id="a6d0e-122">Restituisce `DateTime.MinValue` o il valore specificato se la stringa non rappresenta una data/ora.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="a6d0e-123">Converte un valore stringa in un valore decimale.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="a6d0e-124">Restituisce 0,0 o il valore specificato se la stringa non rappresenta un valore decimale.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="a6d0e-125">Converte un valore stringa in un valore float.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-125">Converts a string value to a float.</span></span> <span data-ttu-id="a6d0e-126">Restituisce 0,0 o il valore specificato se la stringa non rappresenta un valore decimale.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="a6d0e-127">Converte un valore stringa in un intero.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-127">Converts a string value to an integer.</span></span> <span data-ttu-id="a6d0e-128">Restituisce 0 o il valore specificato se la stringa non rappresenta un numero intero.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="a6d0e-129">Crea un URL compatibile con browser da un percorso file locale, con le parti di percorso aggiuntivo facoltativo.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="a6d0e-130">Esegue il rendering *valore* come markup HTML anziché eseguirne il rendering con codifica HTML come output.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="a6d0e-131">Restituisce true se il valore può essere convertito da una stringa nel tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="a6d0e-132">Restituisce true se l'oggetto o una variabile non ha alcun valore.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="a6d0e-133">Restituisce true se la richiesta è una richiesta POST.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="a6d0e-134">(Le richieste iniziali sono in genere è un'operazione GET).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="a6d0e-135">Specifica il percorso di una pagina di layout da applicare a questa pagina.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="a6d0e-136">Contiene dati condivisi tra la pagina, pagine di layout e pagine parziali nella richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="a6d0e-137">È possibile utilizzare dinamica `Page` proprietà per accedere agli stessi dati, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="a6d0e-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="a6d0e-138">(Le pagine di layout) Visualizza il contenuto di una pagina di contenuto che non sia in qualsiasi sezioni denominate.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="a6d0e-139">Esegue il rendering di una pagina di contenuto con il percorso specificato e i dati aggiuntivi facoltativi.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="a6d0e-140">È possibile ottenere i valori dei parametri aggiuntivi da `PageData` dalla posizione (ad esempio, 1) o alla chiave (ad esempio, 2).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="a6d0e-141">(Le pagine di layout) Esegue il rendering di una sezione di contenuto con un nome.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="a6d0e-142">Impostare *obbligatorio* su false per rendere una sezione facoltativa.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="a6d0e-143">Ottiene o imposta il valore di un cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="a6d0e-144">Ottiene i file che sono stati caricati nella richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="a6d0e-145">Ottiene i dati che è stati registrati in un form (come stringhe).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="a6d0e-146">`Request[key]`controlla sia il `Request.Form` e `Request.QueryString` raccolte.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="a6d0e-147">Ottiene i dati che è stati specificati nella stringa di query dell'URL.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="a6d0e-148">`Request[key]`controlla sia il `Request.Form` e `Request.QueryString` raccolte.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="a6d0e-149">Convalida per un elemento del form, il valore di stringa di query, dal cookie o valore dell'intestazione della richiesta in modo selettivo disabilita.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="a6d0e-150">Convalida della richiesta è abilitata per impostazione predefinita e impedisce agli utenti di markup o altro contenuto potenzialmente pericoloso di registrazione.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="a6d0e-151">Aggiunge un'intestazione di server HTTP alla risposta.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="a6d0e-152">Memorizza nella cache l'output delle pagine per un tempo specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="a6d0e-153">Facoltativamente, impostare *scorrevole* per reimpostare il timeout per ogni accesso alla pagina e *varyByParams* per memorizzare nella cache diverse versioni della pagina per ogni stringa di query diversi nella richiesta della pagina.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="a6d0e-154">Reindirizza la richiesta del browser in un nuovo percorso.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="a6d0e-155">Imposta il codice di stato HTTP inviato al browser.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="a6d0e-156">Scrive il contenuto di *dati* alla risposta con un tipo MIME facoltativo.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="a6d0e-157">Scrive il contenuto di un file di risposta.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="a6d0e-158">(Le pagine di layout) Definisce una sezione di contenuto con un nome.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="a6d0e-159">Decodifica una stringa che viene codificato in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="a6d0e-160">Codifica una stringa per il rendering nel markup HTML.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="a6d0e-161">Restituisce il percorso fisico del server per il percorso virtuale specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="a6d0e-162">Decodifica il testo da un URL.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="a6d0e-163">Codifica testo da inserire in un URL.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="a6d0e-164">Ottiene o imposta un valore che non esiste finché l'utente chiude il browser.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="a6d0e-165">Visualizza una rappresentazione di stringa del valore dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="a6d0e-166">Ottiene i dati aggiuntivi da URL (ad esempio, *MyPage/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="a6d0e-167">Modifica la password per l'utente specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="a6d0e-168">Conferma di un account con il token di conferma di account.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="a6d0e-169">Crea un nuovo account utente con il nome utente specificato e la password.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="a6d0e-170">Per richiedere un token di conferma, passare true per *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="a6d0e-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="a6d0e-171">Ottiene l'identificatore di tipo integer per l'utente attualmente connesso.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="a6d0e-172">Ottiene il nome per l'utente attualmente connesso.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="a6d0e-173">Genera un token di reimpostazione della password che può essere inviato tramite posta elettronica a un utente in modo che l'utente è possibile reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="a6d0e-174">Restituisce l'ID utente del nome utente.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="a6d0e-175">Restituisce true se l'utente corrente è connesso.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="a6d0e-176">Restituisce true se l'utente è stata confermata (ad esempio, tramite un messaggio di conferma).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="a6d0e-177">Restituisce true se il nome dell'utente corrente corrisponde al nome utente specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="a6d0e-178">Consente all'utente in impostando un token di autenticazione nel cookie.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="a6d0e-179">Registra la disconnessione dell'utente rimuovendo il cookie di token di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="a6d0e-180">Se l'utente non è autenticato, imposta lo stato HTTP su 401 (non autorizzato).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="a6d0e-181">Se l'utente corrente non è un membro di uno dei ruoli specificati, imposta lo stato HTTP su 401 (non autorizzato).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="a6d0e-182">Se l'utente corrente non è l'utente specificato da *username*, imposta lo stato HTTP su 401 (non autorizzato).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="a6d0e-183">Se il token di reimpostazione della password è valido, cambia la password dell'utente per la nuova password.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="a6d0e-184">Dati</span><span class="sxs-lookup"><span data-stu-id="a6d0e-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="a6d0e-185">Esegue *SQLstatement* (con parametri facoltativi), ad esempio INSERT, DELETE o UPDATE e restituisce il numero di record interessati.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="a6d0e-186">Restituisce la colonna identity della riga inserita più di recente.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="a6d0e-187">Apre il file di database specificato o il database specificato utilizzando una stringa di connessione denominata dal *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="a6d0e-188">Apre un database utilizzando la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-188">Opens a database using the connection string.</span></span> <span data-ttu-id="a6d0e-189">(Questo contrasta con `Database.Open`, che utilizza un nome di stringa di connessione.)</span><span class="sxs-lookup"><span data-stu-id="a6d0e-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="a6d0e-190">Il database utilizzando una query *SQLstatement* (passando facoltativamente i parametri) e restituisce i risultati sotto forma di raccolta.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="a6d0e-191">Esegue *SQLstatement* (con parametri facoltativi) e restituisce un singolo record.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="a6d0e-192">Esegue *SQLstatement* (con parametri facoltativi) e restituisce un valore singolo.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="a6d0e-193">Helper</span><span class="sxs-lookup"><span data-stu-id="a6d0e-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="a6d0e-194">Esegue il rendering del codice JavaScript Analitica Google per l'ID specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="a6d0e-195">Esegue il rendering il codice JavaScript Analitica StatCounter per il progetto specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="a6d0e-196">Esegue il rendering il codice JavaScript Analitica Yahoo per l'account specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="a6d0e-197">Passa una ricerca in Bing.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-197">Passes a search to Bing.</span></span> <span data-ttu-id="a6d0e-198">Per specificare il sito alla ricerca e un titolo per la casella di ricerca, è possibile impostare il `Bing.SiteUrl` e `Bing.SiteTitle` proprietà.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="a6d0e-199">In genere si impostano queste proprietà  *\_AppStart* pagina.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="a6d0e-200">Inizializza un grafico.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="a6d0e-201">Aggiunge una legenda a un grafico.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="a6d0e-202">Aggiunge una serie di valori al grafico.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="a6d0e-203">Restituisce un hash per i dati specificati.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="a6d0e-204">L'algoritmo predefinito è `sha256`.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="a6d0e-205">Consente agli utenti di Facebook di stabilire una connessione a pagine.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="a6d0e-206">Esegue il rendering dell'interfaccia utente per il caricamento dei file.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="a6d0e-207">Esegue il rendering del tag giocatore Xbox specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="a6d0e-208">Esegue il rendering dell'immagine Gravatar per l'indirizzo di posta elettronica specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="a6d0e-209">Converte un oggetto dati in una stringa in formato JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="a6d0e-210">Converte una stringa di input con codifica JSON in un oggetto dati che è possibile scorrere o inserire in un database.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="a6d0e-211">Esegue il rendering di collegamenti di rete sociali tramite il titolo specificato e l'URL facoltativo.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="a6d0e-212">Associa un messaggio di errore a un campo del form.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-212">Associates an error message with a form field.</span></span> <span data-ttu-id="a6d0e-213">Utilizzare il `ModelState` helper per l'accesso al membro.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="a6d0e-214">Associa un messaggio di errore a un form.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-214">Associates an error message with a form.</span></span> <span data-ttu-id="a6d0e-215">Utilizzare il `ModelState` helper per l'accesso al membro.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="a6d0e-216">Restituisce true se non sono presenti errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="a6d0e-217">Utilizzare il `ModelState` helper per l'accesso al membro.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="a6d0e-218">Esegue il rendering le proprietà e i valori di elementi figlio e un oggetto oggetti.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="a6d0e-219">Esegue il rendering il test di verifica reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="a6d0e-220">Imposta le chiavi pubbliche e private per il servizio di reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="a6d0e-221">In genere si impostano queste proprietà  *\_AppStart* pagina.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="a6d0e-222">Restituisce il risultato del test di reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="a6d0e-223">Esegue il rendering delle informazioni sullo stato sulle pagine Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="a6d0e-224">Esegue il rendering di un flusso di Twitter per l'utente specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="a6d0e-225">Esegue il rendering di un flusso di Twitter per il testo di ricerca specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="a6d0e-226">Esegue il rendering di un lettore Flash video per il file specificato con l'altezza e larghezza facoltativa.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="a6d0e-227">Esegue il rendering di un lettore multimediale di Windows per il file specificato con l'altezza e larghezza facoltativa.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="a6d0e-228">Esegue il rendering di un lettore Silverlight per l'oggetto specificato *XAP* file con l'altezza e larghezza richiesta.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="a6d0e-229">Restituisce l'oggetto specificato da *chiave*, oppure null se l'oggetto non è stato trovato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="a6d0e-230">Rimuove l'oggetto specificato da *chiave* dalla cache.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="a6d0e-231">Inserisce *valore* nella cache con il nome specificato da *chiave*.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="a6d0e-232">Crea un nuovo `WebGrid` utilizzando i dati di una query dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="a6d0e-233">Esegue il rendering di markup per la visualizzazione di dati in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="a6d0e-234">Esegue il rendering di un cercapersone per il `WebGrid` oggetto.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="a6d0e-235">Carica un'immagine dal percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="a6d0e-236">Aggiunge l'immagine specificata come filigrana.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="a6d0e-237">Aggiunge il testo specificato all'immagine.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="a6d0e-238">Consente di capovolgere orizzontalmente o verticalmente l'immagine.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="a6d0e-239">Carica un'immagine quando viene registrata un'immagine a una pagina durante il caricamento di un file.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="a6d0e-240">Ridimensiona un'immagine.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="a6d0e-241">Ruota l'immagine a sinistra o a destra.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="a6d0e-242">Salva l'immagine nel percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="a6d0e-243">Imposta la password per il server SMTP.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="a6d0e-244">In genere si imposta questa proprietà  *\_AppStart* pagina.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="a6d0e-245">Invia un messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="a6d0e-246">Imposta il nome del server SMTP.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-246">Sets the SMTP server name.</span></span> <span data-ttu-id="a6d0e-247">In genere si imposta questa proprietà*\_AppStart* pagina.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-247">Normally you set this property in the*\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="a6d0e-248">Imposta il nome utente per il server SMTP.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="a6d0e-249">In genere è necessario impostare questa proprietà  *\_AppStart* pagina.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="a6d0e-250">Convalida</span><span class="sxs-lookup"><span data-stu-id="a6d0e-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="a6d0e-251">(v2) Esegue il rendering di un messaggio di errore di convalida per il campo specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="a6d0e-252">(v2) Visualizza un elenco di tutti gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="a6d0e-253">(v2) Registra un elemento di input utente per il tipo di convalida specificato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="a6d0e-254">(v2) In modo dinamico esegue il rendering di attributi della classe CSS per la convalida lato client in modo che è possibile formattare i messaggi di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="a6d0e-255">(Richiede che fare riferimento alle librerie di script client appropriati e definire le classi CSS).</span><span class="sxs-lookup"><span data-stu-id="a6d0e-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="a6d0e-256">(v2) Abilita la convalida lato client per il campo di input utente.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="a6d0e-257">È necessario che fare riferimento alle librerie di script client appropriato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="a6d0e-258">(v2) Restituisce true se tutti gli elementi input utente che vengono registrate per la convalida contengono valori validi.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="a6d0e-259">(v2) Specifica che gli utenti devono fornire un valore per l'elemento di input utente.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="a6d0e-260">(v2) Specifica che gli utenti devono fornire valori per ognuno degli elementi di input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="a6d0e-261">Questo metodo non consente di specificare un messaggio di errore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-261">This method does not let you specify a custom error message.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

<span data-ttu-id="a6d0e-262">(v2) Specifica un test di convalida quando si utilizza il `Validation.Add` metodo.</span><span class="sxs-lookup"><span data-stu-id="a6d0e-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
