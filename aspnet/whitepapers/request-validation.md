---
uid: whitepapers/request-validation
title: Richiedere la convalida - prevenzione degli attacchi Script | Documenti Microsoft
author: rick-anderson
description: "In questo documento descrive la funzionalità di convalida richiesta di ASP.NET in cui, per impostazione predefinita, l'applicazione viene impedito di elaborazione submitt di contenuto HTML non codificato..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="3eac1-103">Richiedere la convalida - prevenzione degli attacchi di Script</span><span class="sxs-lookup"><span data-stu-id="3eac1-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="3eac1-104">Questo documento descrive la funzionalità di ASP.NET in cui, per impostazione predefinita, l'applicazione viene impedito di elaborazione del contenuto HTML non codificato di inviata al server di convalida delle richieste.</span><span class="sxs-lookup"><span data-stu-id="3eac1-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="3eac1-105">Questa funzionalità di convalida richiesta può essere disabilitata quando l'applicazione è stato progettato per elaborare in modo sicuro dati HTML.</span><span class="sxs-lookup"><span data-stu-id="3eac1-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="3eac1-106">Si applica a ASP.NET 1.1 e 2.0 di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3eac1-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="3eac1-107">Convalida della richiesta, una funzionalità di ASP.NET versione 1.1, impedisce il server di accettare contenuto HTML non codificato che lo contiene.</span><span class="sxs-lookup"><span data-stu-id="3eac1-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="3eac1-108">Questa funzionalità è progettata per aiutare a evitare alcuni attacchi script injection, in base al quale il codice di script client o HTML può essere involontario inviati a un server, archiviati e quindi presentato ad altri utenti.</span><span class="sxs-lookup"><span data-stu-id="3eac1-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="3eac1-109">È comunque consigliabile che la convalida dei dati di tutti gli input e la codifica HTML quando appropriato.</span><span class="sxs-lookup"><span data-stu-id="3eac1-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="3eac1-110">Ad esempio, si crea una pagina Web che richiede l'indirizzo di posta elettronica dell'utente e, successivamente, archiviarlo che posta elettronica in un database.</span><span class="sxs-lookup"><span data-stu-id="3eac1-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="3eac1-111">Se l'utente immette &lt;SCRIPT&gt;avviso ("hello dallo script")&lt;/SCRIPT&gt; anziché un indirizzo di posta elettronica valido, quando i dati viene presentati, questo script può essere eseguito se il contenuto non è stato codificato correttamente.</span><span class="sxs-lookup"><span data-stu-id="3eac1-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="3eac1-112">La funzionalità di convalida richiesta di ASP.NET consente di evitare questo inconveniente.</span><span class="sxs-lookup"><span data-stu-id="3eac1-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="3eac1-113">Perché questa funzionalità è utile</span><span class="sxs-lookup"><span data-stu-id="3eac1-113">Why this feature is useful</span></span>

<span data-ttu-id="3eac1-114">Molti siti non sono in grado di riconoscere che sono vulnerabili agli attacchi intrusivi nel codice script semplice.</span><span class="sxs-lookup"><span data-stu-id="3eac1-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="3eac1-115">Se lo scopo di tali attacchi è per infettare il sito visualizzando HTML o potenzialmente eseguire lo script client per reindirizzare l'utente al sito un pirata informatico, attacchi intrusivi nel codice di script sono un problema che gli sviluppatori Web devono competere con.</span><span class="sxs-lookup"><span data-stu-id="3eac1-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="3eac1-116">Attacchi intrusivi nel codice script rappresentano un problema di tutti gli sviluppatori web, se si utilizza ASP.NET, ASP o altre tecnologie di sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="3eac1-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="3eac1-117">La funzionalità di convalida delle richieste ASP.NET in modo proattivo impedisce questi attacchi, non consentendo contenuto HTML non codificato essere elaborati dal server, a meno che lo sviluppatore decide di consentire che il contenuto.</span><span class="sxs-lookup"><span data-stu-id="3eac1-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="3eac1-118">Cosa accade: pagina di errore</span><span class="sxs-lookup"><span data-stu-id="3eac1-118">What to expect: Error Page</span></span>

<span data-ttu-id="3eac1-119">Cattura di schermata seguente mostra alcuni esempi di codice ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="3eac1-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="3eac1-120">L'esecuzione di genera questo codice in una pagina semplice che consente di immettere del testo nella casella di testo, fare clic sul pulsante e visualizzare il testo nel controllo etichetta:</span><span class="sxs-lookup"><span data-stu-id="3eac1-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="3eac1-121">Tuttavia, sono ad esempio JavaScript, `<script>alert("hello!")</script>` per essere immessi e inviati si otterrebbe un'eccezione:</span><span class="sxs-lookup"><span data-stu-id="3eac1-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="3eac1-122">Il messaggio di errore indica che un 'potenzialmente pericolosi Request. Form è stato rilevato valore' e fornisce altre informazioni, vedere la descrizione esattamente ciò che si è verificato e come modificare il comportamento.</span><span class="sxs-lookup"><span data-stu-id="3eac1-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="3eac1-123">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3eac1-123">For example:</span></span>

<span data-ttu-id="3eac1-124">Convalida della richiesta ha rilevato un valore di input potenzialmente pericoloso client e l'elaborazione della richiesta è stata interrotta.</span><span class="sxs-lookup"><span data-stu-id="3eac1-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="3eac1-125">Questo valore potrebbe indicare un tentativo di compromettere la sicurezza dell'applicazione, ad esempio un attacco di script tra siti.</span><span class="sxs-lookup"><span data-stu-id="3eac1-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="3eac1-126">È possibile disabilitare la convalida della richiesta impostando `validateRequest=false` nell'istruzione Page o nella sezione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="3eac1-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="3eac1-127">Tuttavia, è consigliabile che l'applicazione in modo esplicito verificare tutti gli input in questo caso.</span><span class="sxs-lookup"><span data-stu-id="3eac1-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="3eac1-128">Disabilitare la convalida della richiesta in una pagina</span><span class="sxs-lookup"><span data-stu-id="3eac1-128">Disabling request validation on a page</span></span>

<span data-ttu-id="3eac1-129">Per disabilitare la convalida della richiesta in una pagina è necessario impostare il `validateRequest` attributo della direttiva della pagina per `false`:</span><span class="sxs-lookup"><span data-stu-id="3eac1-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="3eac1-130">Quando la convalida della richiesta è disabilitata, il contenuto può essere inviato a una pagina. è responsabilità dello sviluppatore della pagina per verificare che il contenuto con codifica o elaborate correttamente.</span><span class="sxs-lookup"><span data-stu-id="3eac1-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="3eac1-131">Sulla disabilitazione della convalida richiesta per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="3eac1-131">Disabling request validation for your application</span></span>

<span data-ttu-id="3eac1-132">Per disabilitare la convalida della richiesta per l'applicazione, è necessario modificare o creare un file Web. config per l'applicazione e impostare l'attributo validateRequest del `<pages />` sezione a `false`:</span><span class="sxs-lookup"><span data-stu-id="3eac1-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="3eac1-133">Se si desidera disabilitare la convalida della richiesta per tutte le applicazioni nel server, è possibile apportare questa modifica al file Machine. config.</span><span class="sxs-lookup"><span data-stu-id="3eac1-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="3eac1-134">Quando la convalida della richiesta è disabilitata, è possibile inviare contenuto dell'applicazione. è responsabilità dello sviluppatore dell'applicazione per assicurarsi che il contenuto con codifica o elaborate correttamente.</span><span class="sxs-lookup"><span data-stu-id="3eac1-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="3eac1-135">Il codice seguente viene modificato per disattivare la convalida della richiesta:</span><span class="sxs-lookup"><span data-stu-id="3eac1-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="3eac1-136">Se è stato immesso il codice JavaScript seguente nella casella di testo `<script>alert("hello!")</script>` il risultato sarebbe:</span><span class="sxs-lookup"><span data-stu-id="3eac1-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="3eac1-137">Per evitare questa situazione, con la convalida delle richieste disattivata, abbiamo base in formato HTML codificare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="3eac1-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="3eac1-138">Come codificare contenuto in formato HTML</span><span class="sxs-lookup"><span data-stu-id="3eac1-138">How to HTML encode content</span></span>

<span data-ttu-id="3eac1-139">Se è stata disabilitata la convalida della richiesta, è buona norma contenuto con codifica HTML che verrà archiviato per un utilizzo futuro.</span><span class="sxs-lookup"><span data-stu-id="3eac1-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="3eac1-140">La codifica HTML verrà automaticamente sostituito qualsiasi '&lt;'o'&gt;' (insieme a diversi altri simboli) con codice HTML corrispondente rappresentazione con codifica.</span><span class="sxs-lookup"><span data-stu-id="3eac1-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="3eac1-141">Ad esempio, '&lt;'viene sostituito da'&amp;lt;' e '&gt;'viene sostituito da'&amp;gt;'.</span><span class="sxs-lookup"><span data-stu-id="3eac1-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="3eac1-142">I browser usano questi codici speciali per visualizzare il '&lt;'o'&gt;' nel browser.</span><span class="sxs-lookup"><span data-stu-id="3eac1-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="3eac1-143">Il contenuto può essere facilmente codificata in formato HTML sul server utilizzando il `Server.HtmlEncode(string)` API.</span><span class="sxs-lookup"><span data-stu-id="3eac1-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="3eac1-144">Contenuto può inoltre essere facilmente HTML decodifica, vale a dire ripristinato l'utilizzo di HTML standard di `Server.HtmlDecode(string)` metodo.</span><span class="sxs-lookup"><span data-stu-id="3eac1-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="3eac1-145">Risultante:</span><span class="sxs-lookup"><span data-stu-id="3eac1-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
