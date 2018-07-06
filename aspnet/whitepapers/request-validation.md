---
uid: whitepapers/request-validation
title: Richiesta di convalida - prevenzione degli attacchi tramite Script | Microsoft Docs
author: rick-anderson
description: In questo documento descrive la funzionalità di convalida richiesta di ASP.NET in cui, per impostazione predefinita, l'applicazione viene impedita l'elaborazione submitt di contenuto HTML non codificato...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0dfbfcae70792c57d530fc5e6fb73f8f96ec6e02
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809745"
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="f265d-103">Richiesta di convalida - prevenzione degli attacchi tramite Script</span><span class="sxs-lookup"><span data-stu-id="f265d-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="f265d-104">In questo documento descrive la funzionalità di ASP.NET in cui, per impostazione predefinita, l'applicazione viene impedita l'elaborazione del contenuto HTML non codificato inviato al server di convalida delle richieste.</span><span class="sxs-lookup"><span data-stu-id="f265d-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="f265d-105">Questa funzionalità di convalida richiesta può essere disabilitata quando l'applicazione è stata progettata per elaborare in modo sicuro i dati HTML.</span><span class="sxs-lookup"><span data-stu-id="f265d-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="f265d-106">Si applica a ASP.NET 1.1 e ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="f265d-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="f265d-107">Convalida delle richieste, una funzionalità di ASP.NET sin dalla versione 1.1, impedisce al server di accettare contenuto HTML non codificato che lo contiene.</span><span class="sxs-lookup"><span data-stu-id="f265d-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="f265d-108">Questa funzionalità è progettata per aiutare a evitare alcuni attacchi script injection in base al quale il codice di script client o HTML può essere inconsapevolmente inviato a un server, archiviati e quindi presentato ad altri utenti.</span><span class="sxs-lookup"><span data-stu-id="f265d-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="f265d-109">È comunque consigliabile che convalida i dati di tutti gli input e codificarli con HTML, quando appropriato.</span><span class="sxs-lookup"><span data-stu-id="f265d-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="f265d-110">Ad esempio, si crea una pagina Web che richiede l'indirizzo di posta elettronica dell'utente e quindi archivia tale indirizzo in un database di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="f265d-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="f265d-111">Se l'utente immette &lt;SCRIPT&gt;avviso ("hello from script")&lt;/script&gt; anziché un indirizzo di posta elettronica valido, quando viene visualizzato tale data, questo script può essere eseguito se il contenuto non è stato codificato correttamente.</span><span class="sxs-lookup"><span data-stu-id="f265d-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="f265d-112">La funzionalità di convalida richiesta di ASP.NET consente di evitare questo tipo di problema.</span><span class="sxs-lookup"><span data-stu-id="f265d-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="f265d-113">Perché questa funzionalità è utile</span><span class="sxs-lookup"><span data-stu-id="f265d-113">Why this feature is useful</span></span>

<span data-ttu-id="f265d-114">Molti siti non sono a conoscenza che sono aperti per attacchi intrusivi negli script semplice.</span><span class="sxs-lookup"><span data-stu-id="f265d-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="f265d-115">Se lo scopo di questi attacchi è per infettare il sito visualizzando HTML o potenzialmente eseguire lo script client a reindirizzare l'utente al sito un pirata informatico, attacchi intrusivi negli script sono un problema che devono fare i conti con gli sviluppatori Web.</span><span class="sxs-lookup"><span data-stu-id="f265d-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="f265d-116">Attacchi intrusivi negli script rappresentano un problema di tutti gli sviluppatori web, se si usa ASP.NET, ASP o altre tecnologie di sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="f265d-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="f265d-117">La funzionalità di convalida delle richieste ASP.NET in modo proattivo impedisce questi attacchi, non consentendo il contenuto HTML non codificato essere elaborati dal server, a meno che lo sviluppatore decide di consentire che il contenuto.</span><span class="sxs-lookup"><span data-stu-id="f265d-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="f265d-118">Cosa accade: pagina di errore</span><span class="sxs-lookup"><span data-stu-id="f265d-118">What to expect: Error Page</span></span>

<span data-ttu-id="f265d-119">Cattura di schermata seguente mostra alcuni esempi di codice ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="f265d-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="f265d-120">L'esecuzione di genera questo codice in una pagina semplice che consente di immettere il testo nella casella di testo, fare clic sul pulsante e visualizzare il testo nel controllo etichetta:</span><span class="sxs-lookup"><span data-stu-id="f265d-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="f265d-121">Tuttavia, era, ad esempio JavaScript, `<script>alert("hello!")</script>` immissione e inviato si otterrebbero un'eccezione:</span><span class="sxs-lookup"><span data-stu-id="f265d-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="f265d-122">Il messaggio di errore indica che un 'potenzialmente pericolosi Request. Form è stato rilevato valore' e fornisce altre informazioni, vedere la descrizione esattamente ciò che si è verificato e come modificare il comportamento.</span><span class="sxs-lookup"><span data-stu-id="f265d-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="f265d-123">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f265d-123">For example:</span></span>

<span data-ttu-id="f265d-124">Convalida delle richieste ha rilevato un valore di input potenzialmente pericolosa client e l'elaborazione della richiesta è stata interrotta.</span><span class="sxs-lookup"><span data-stu-id="f265d-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="f265d-125">Questo valore potrebbe indicare un tentativo di compromettere la sicurezza dell'applicazione, ad esempio un attacco di scripting intersito.</span><span class="sxs-lookup"><span data-stu-id="f265d-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="f265d-126">È possibile disabilitare la convalida della richiesta impostando `validateRequest=false` nella direttiva di pagina o nella sezione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f265d-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="f265d-127">Tuttavia, è consigliabile che l'applicazione in modo esplicito controllare tutti gli input in questo caso.</span><span class="sxs-lookup"><span data-stu-id="f265d-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="f265d-128">La disabilitazione della convalida richiesta in una pagina</span><span class="sxs-lookup"><span data-stu-id="f265d-128">Disabling request validation on a page</span></span>

<span data-ttu-id="f265d-129">Per disabilitare la convalida delle richieste in una pagina è necessario impostare il `validateRequest` attributo della direttiva della pagina per `false`:</span><span class="sxs-lookup"><span data-stu-id="f265d-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="f265d-130">Quando la convalida delle richieste è disabilitata, il contenuto può essere inviato a una pagina. è responsabilità dello sviluppatore della pagina per verificare che il contenuto viene codificata o elaborate correttamente.</span><span class="sxs-lookup"><span data-stu-id="f265d-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="f265d-131">La disabilitazione della convalida richiesta per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="f265d-131">Disabling request validation for your application</span></span>

<span data-ttu-id="f265d-132">Per disabilitare la convalida della richiesta per l'applicazione, è necessario modificare o creare un file Web. config per l'applicazione e impostare l'attributo validateRequest del `<pages />` sezione `false`:</span><span class="sxs-lookup"><span data-stu-id="f265d-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="f265d-133">Se si vuole disabilitare la convalida delle richieste per tutte le applicazioni nel server, è possibile apportare questa modifica al file Machine. config.</span><span class="sxs-lookup"><span data-stu-id="f265d-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="f265d-134">Quando la convalida delle richieste è disabilitata, è possibile inviare contenuto dell'applicazione. è responsabilità dello sviluppatore dell'applicazione per verificare che il contenuto viene codificata o elaborate correttamente.</span><span class="sxs-lookup"><span data-stu-id="f265d-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="f265d-135">Il codice seguente viene modificato per disattivare la convalida delle richieste:</span><span class="sxs-lookup"><span data-stu-id="f265d-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="f265d-136">Ora, se il codice JavaScript seguente è stata immessa nella casella di testo `<script>alert("hello!")</script>` il risultato sarà:</span><span class="sxs-lookup"><span data-stu-id="f265d-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="f265d-137">Per evitare questa situazione, con la convalida delle richieste disabilitata, è necessario in formato HTML codificare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="f265d-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="f265d-138">Come codificare il contenuto in formato HTML</span><span class="sxs-lookup"><span data-stu-id="f265d-138">How to HTML encode content</span></span>

<span data-ttu-id="f265d-139">Se è stata disabilitata la convalida della richiesta, è buona norma codifica HTML del contenuto che verrà archiviato per un uso futuro.</span><span class="sxs-lookup"><span data-stu-id="f265d-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="f265d-140">La codifica HTML sostituirà automaticamente qualsiasi '&lt;'o'&gt;' (insieme a diversi altri simboli) con il codice HTML corrispondente rappresentazione con codifica.</span><span class="sxs-lookup"><span data-stu-id="f265d-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="f265d-141">Ad esempio, '&lt;'viene sostituito da'&amp;lt;' e '&gt;'viene sostituito da'&amp;gt;'.</span><span class="sxs-lookup"><span data-stu-id="f265d-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="f265d-142">I browser usano questi codici speciali per la visualizzazione di '&lt;'o'&gt;' nel browser.</span><span class="sxs-lookup"><span data-stu-id="f265d-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="f265d-143">Il contenuto può essere facilmente codificata in formato HTML sul server utilizzando il `Server.HtmlEncode(string)` API.</span><span class="sxs-lookup"><span data-stu-id="f265d-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="f265d-144">Contenuto può anche essere facilmente-decodificato in HTML, vale a dire, ripristinato l'utilizzo di codice HTML standard di `Server.HtmlDecode(string)` (metodo).</span><span class="sxs-lookup"><span data-stu-id="f265d-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="f265d-145">Risultante:</span><span class="sxs-lookup"><span data-stu-id="f265d-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
