---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "L'invio di dati di Form HTML nell'API Web ASP.NET: dati Form-urlencoded | Microsoft Docs"
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/15/2012
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2d01212cc408f8bb66fa3103464c9a1f7a1e21c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823811"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="cedeb-102">L'invio di dati di Form HTML nell'API Web ASP.NET: dati Form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="cedeb-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="cedeb-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cedeb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="cedeb-104">Parte 1: Dati di Form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="cedeb-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="cedeb-105">Questo articolo illustra come pubblicare dati form-urlencoded a un controller API Web.</span><span class="sxs-lookup"><span data-stu-id="cedeb-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="cedeb-106">Panoramica dei moduli HTML</span><span class="sxs-lookup"><span data-stu-id="cedeb-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="cedeb-107">L'invio di tipi complessi</span><span class="sxs-lookup"><span data-stu-id="cedeb-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="cedeb-108">L'invio di dati del Form tramite AJAX</span><span class="sxs-lookup"><span data-stu-id="cedeb-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="cedeb-109">L'invio di tipi semplici</span><span class="sxs-lookup"><span data-stu-id="cedeb-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="cedeb-110">[Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="cedeb-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="cedeb-111">Panoramica dei moduli HTML</span><span class="sxs-lookup"><span data-stu-id="cedeb-111">Overview of HTML Forms</span></span>

<span data-ttu-id="cedeb-112">Utilizzo di form HTML a GET o POST per inviare dati al server.</span><span class="sxs-lookup"><span data-stu-id="cedeb-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="cedeb-113">Il **metodo** attributo delle **form** elemento fornisce il metodo HTTP:</span><span class="sxs-lookup"><span data-stu-id="cedeb-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="cedeb-114">Il metodo predefinito è GET.</span><span class="sxs-lookup"><span data-stu-id="cedeb-114">The default method is GET.</span></span> <span data-ttu-id="cedeb-115">Se il form Usa GET, il formato dati sono codificati nell'URI come stringa di query.</span><span class="sxs-lookup"><span data-stu-id="cedeb-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="cedeb-116">Se il form Usa POST, i dati del modulo viene inseriti nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="cedeb-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="cedeb-117">Per i dati registrati, la **enctype** attributo specifica il formato del corpo della richiesta:</span><span class="sxs-lookup"><span data-stu-id="cedeb-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="cedeb-118">Enctype</span><span class="sxs-lookup"><span data-stu-id="cedeb-118">enctype</span></span> | <span data-ttu-id="cedeb-119">Descrizione</span><span class="sxs-lookup"><span data-stu-id="cedeb-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cedeb-120">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="cedeb-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="cedeb-121">Dati del form sono codificati come coppie nome/valore, simile a una stringa di query URI.</span><span class="sxs-lookup"><span data-stu-id="cedeb-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="cedeb-122">Questo è il formato predefinito per POST.</span><span class="sxs-lookup"><span data-stu-id="cedeb-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="cedeb-123">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="cedeb-123">multipart/form-data</span></span> | <span data-ttu-id="cedeb-124">Dati del form sono codificati come un messaggio MIME multiparte.</span><span class="sxs-lookup"><span data-stu-id="cedeb-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="cedeb-125">Utilizzare questo formato, se si sta caricando un file al server.</span><span class="sxs-lookup"><span data-stu-id="cedeb-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="cedeb-126">Parte 1 di questo articolo vengono esaminati formato x-www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="cedeb-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="cedeb-127">[Parte 2](sending-html-form-data-part-2.md) descrive multipart MIME.</span><span class="sxs-lookup"><span data-stu-id="cedeb-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="cedeb-128">L'invio di tipi complessi</span><span class="sxs-lookup"><span data-stu-id="cedeb-128">Sending Complex Types</span></span>

<span data-ttu-id="cedeb-129">In genere, si invierà un tipo complesso, composto da valori ricavati da vari controlli del form.</span><span class="sxs-lookup"><span data-stu-id="cedeb-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="cedeb-130">Si consideri il modello seguente rappresenta un aggiornamento di stato:</span><span class="sxs-lookup"><span data-stu-id="cedeb-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="cedeb-131">Di seguito è un controller API Web che accetta un `Update` oggetto tramite POST.</span><span class="sxs-lookup"><span data-stu-id="cedeb-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="cedeb-132">Questo controller Usa [basate su azioni routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), quindi il modello di route viene &quot;api / {controller} / {action} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="cedeb-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="cedeb-133">Il client invierà i dati da &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="cedeb-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="cedeb-134">Ora passiamo alla scrittura di un form HTML per gli utenti di inviare un aggiornamento di stato.</span><span class="sxs-lookup"><span data-stu-id="cedeb-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="cedeb-135">Si noti che il **azione** attributo nel form è l'URI del nostro azione del controller.</span><span class="sxs-lookup"><span data-stu-id="cedeb-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="cedeb-136">Ecco il modulo con alcuni valori immessi:</span><span class="sxs-lookup"><span data-stu-id="cedeb-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="cedeb-137">Quando l'utente sceglie invio, il browser invia una richiesta HTTP simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="cedeb-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="cedeb-138">Si noti che il corpo della richiesta contiene i dati del modulo, formattati come coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="cedeb-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="cedeb-139">API Web, le coppie nome/valore viene convertito automaticamente in un'istanza di `Update` classe.</span><span class="sxs-lookup"><span data-stu-id="cedeb-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="cedeb-140">L'invio di dati del Form tramite AJAX</span><span class="sxs-lookup"><span data-stu-id="cedeb-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="cedeb-141">Quando un utente invia un form, il browser si sposta dalla pagina corrente ed esegue il rendering il corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="cedeb-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="cedeb-142">È normale quando la risposta è una pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="cedeb-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="cedeb-143">Con un'API web, tuttavia, il corpo della risposta è in genere sia vuota o contiene i dati strutturati, ad esempio JSON.</span><span class="sxs-lookup"><span data-stu-id="cedeb-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="cedeb-144">In tal caso, è più opportuno inviare i dati del modulo usando AJAX richiedono, in modo che la pagina è possibile elaborare la risposta.</span><span class="sxs-lookup"><span data-stu-id="cedeb-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="cedeb-145">Il codice seguente viene illustrato come pubblicare i dati del modulo tramite jQuery.</span><span class="sxs-lookup"><span data-stu-id="cedeb-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="cedeb-146">Il componente aggiuntivo jQuery **inviare** funzione sostituisce le azioni form con una nuova funzione.</span><span class="sxs-lookup"><span data-stu-id="cedeb-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="cedeb-147">Sostituisce il comportamento predefinito del pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="cedeb-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="cedeb-148">Il **serializzare** funzione serializza i dati del modulo in coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="cedeb-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="cedeb-149">Per inviare i dati del form al server, chiamare `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="cedeb-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="cedeb-150">Al completamento della richiesta, il `.success()` o `.error()` gestore viene visualizzato un messaggio appropriato all'utente.</span><span class="sxs-lookup"><span data-stu-id="cedeb-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="cedeb-151">L'invio di tipi semplici</span><span class="sxs-lookup"><span data-stu-id="cedeb-151">Sending Simple Types</span></span>

<span data-ttu-id="cedeb-152">Nelle sezioni precedenti, abbiamo inviato un tipo complesso, API Web deserializzata in un'istanza di una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="cedeb-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="cedeb-153">È anche possibile inviare i tipi semplici, ad esempio una stringa.</span><span class="sxs-lookup"><span data-stu-id="cedeb-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="cedeb-154">Prima di inviare un tipo semplice, prendere in considerazione il wrapping invece il valore in un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="cedeb-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="cedeb-155">Ciò offre i vantaggi di convalida del modello sul lato server e rende più semplice estendere il modello, se necessario.</span><span class="sxs-lookup"><span data-stu-id="cedeb-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="cedeb-156">I passaggi di base per l'invio di un tipo semplice sono uguali, ma vi sono due differenze meno evidenti.</span><span class="sxs-lookup"><span data-stu-id="cedeb-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="cedeb-157">Nel controller, in primo luogo, è necessario decorare il nome del parametro con il **FromBody** attributo.</span><span class="sxs-lookup"><span data-stu-id="cedeb-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="cedeb-158">Per impostazione predefinita, API Web prova a ottenere i tipi semplici dall'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="cedeb-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="cedeb-159">Il **FromBody** attributo indica a Web API per leggere il valore dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="cedeb-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="cedeb-160">API Web legge il corpo della risposta in modo che solo un parametro di un'azione può provenire dal corpo della richiesta più di una volta.</span><span class="sxs-lookup"><span data-stu-id="cedeb-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="cedeb-161">Se è necessario ottenere valori più dal corpo della richiesta, definire un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="cedeb-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="cedeb-162">In secondo luogo, il client deve inviare il valore con il formato seguente:</span><span class="sxs-lookup"><span data-stu-id="cedeb-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="cedeb-163">In particolare, la parte relativa al nome della coppia nome/valore deve essere vuoto per un tipo semplice.</span><span class="sxs-lookup"><span data-stu-id="cedeb-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="cedeb-164">Non tutti i browser supportano ad form HTML, ma questo formato è creare nello script come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="cedeb-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="cedeb-165">Ecco un form di esempio:</span><span class="sxs-lookup"><span data-stu-id="cedeb-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="cedeb-166">Ed ecco lo script per inviare il valore di modulo.</span><span class="sxs-lookup"><span data-stu-id="cedeb-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="cedeb-167">L'unica differenza da script precedente è l'argomento passato il **post** (funzione).</span><span class="sxs-lookup"><span data-stu-id="cedeb-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="cedeb-168">È possibile usare lo stesso approccio per l'invio di una matrice di tipi semplici:</span><span class="sxs-lookup"><span data-stu-id="cedeb-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="cedeb-169">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="cedeb-169">Additional Resources</span></span>

[<span data-ttu-id="cedeb-170">Parte 2: Caricamento di File e MIME Multipart</span><span class="sxs-lookup"><span data-stu-id="cedeb-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
