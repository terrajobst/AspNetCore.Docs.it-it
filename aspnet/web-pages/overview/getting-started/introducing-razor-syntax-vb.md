---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introduzione alla programmazione Web ASP.NET usando la sintassi Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: In questa appendice offre una panoramica della programmazione con pagine Web ASP.NET in Visual Basic usando la sintassi Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 17a3a4925766b74446955a8e3a6fddbf9d29a721
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021703"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="d4b6a-103">Introduzione alla programmazione Web ASP.NET usando la sintassi Razor (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="d4b6a-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>
====================
<span data-ttu-id="d4b6a-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d4b6a-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d4b6a-105">Questo articolo offre una panoramica della programmazione con ASP.NET Web Pages con sintassi Razor e Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="d4b6a-106">ASP.NET è la tecnologia Microsoft per l'esecuzione di pagine web dinamiche nei server web.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="d4b6a-107">**Si apprenderà**:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="d4b6a-108">Il principale 8 suggerimenti per l'introduzione a ASP.NET Web Pages con sintassi Razor di programmazione di programmazione.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="d4b6a-109">Concetti di programmazione di base che è necessario.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="d4b6a-110">Il codice server ASP.NET e la sintassi Razor è dedicato.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="d4b6a-111">Versioni del software</span><span class="sxs-lookup"><span data-stu-id="d4b6a-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="d4b6a-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d4b6a-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d4b6a-113">Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="d4b6a-114">La maggior parte degli esempi dell'uso di ASP.NET Web Pages con sintassi Razor usano c#.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="d4b6a-115">Ma la sintassi Razor supporta anche Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="d4b6a-116">Per programmare una pagina web ASP.NET in Visual Basic, si crea una pagina web con un *vbhtml* estensione nome file, quindi aggiungere il codice Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="d4b6a-117">Questo articolo offre una panoramica dell'utilizzo con la sintassi per creare pagine Web ASP.NET e il linguaggio Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="d4b6a-118">I modelli di sito Web predefinito di Microsoft WebMatrix (**panificio**, **raccolta foto**, e **Starter Site**e così via) sono disponibili nelle versioni c# e Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="d4b6a-119">È possibile installare i modelli di Visual Basic da come pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="d4b6a-120">Modelli di siti Web vengono installati nella cartella radice del sito in una cartella denominata *Templates Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="d4b6a-121">8 suggerimenti di programmazione principali</span><span class="sxs-lookup"><span data-stu-id="d4b6a-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="d4b6a-122">In questa sezione sono elencati alcuni suggerimenti che è assolutamente necessario conoscere quando si inizia a scrivere codice server ASP.NET tramite la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="d4b6a-123">1. Aggiungere codice a una pagina utilizzando il carattere @</span><span class="sxs-lookup"><span data-stu-id="d4b6a-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="d4b6a-124">Il `@` carattere avvia espressioni inline, i blocchi a istruzione singola e blocchi a più istruzioni:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="d4b6a-125">Il risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-125">The result displayed in a browser:</span></span>

![Razor Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="d4b6a-127">**La codifica HTML**</span><span class="sxs-lookup"><span data-stu-id="d4b6a-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="d4b6a-128">Quando si visualizza il contenuto in una pagina mediante la `@` carattere, come negli esempi precedenti, ASP.NET codifica in HTML di output.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="d4b6a-129">Questa impostazione sostituisce i caratteri riservati HTML (ad esempio `<` e `>` e `&`) con i codici che abilitano i caratteri da visualizzare come caratteri in una pagina web anziché essere interpretato come tag HTML o le entità.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="d4b6a-130">Senza codifica HTML, l'output dal codice server non vengano visualizzati correttamente e potrebbe esporre una pagina a rischi di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="d4b6a-131">Se l'obiettivo consiste nel markup HTML che esegue il rendering dei tag come markup di output (ad esempio `<p></p>` per un paragrafo o `<em></em>` per enfatizzare il testo), vedere la sezione [combinazione di testo, Markup e codice nei blocchi di codice](#BM_CombiningTextMarkupAndCode) più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="d4b6a-132">Altre informazioni sulla codifica HTML [utilizzo di form HTML nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="d4b6a-133">2. Si racchiudere i blocchi di codice con il codice... Codice di fine</span><span class="sxs-lookup"><span data-stu-id="d4b6a-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="d4b6a-134">Un blocco di codice include una o più istruzioni di codice e racchiusa tra le parole chiave `Code` e `End Code`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="d4b6a-135">Posizionare l'apertura `Code` parola chiave immediatamente dopo il `@` carattere &#8212; tra di essi non possono essere presenti spazi vuoti.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="d4b6a-136">Il risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-136">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="d4b6a-138">3. All'interno di un blocco alla fine si ogni istruzione del codice con un'interruzione di riga</span><span class="sxs-lookup"><span data-stu-id="d4b6a-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="d4b6a-139">In un blocco di codice Visual Basic, ogni istruzione viene terminata con un'interruzione di riga.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="d4b6a-140">(Più avanti nell'articolo si noterà un modo per eseguire il wrapping di un'istruzione di codice lunghi in più righe se necessario.)</span><span class="sxs-lookup"><span data-stu-id="d4b6a-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="d4b6a-141">4. Si usano le variabili per archiviare i valori</span><span class="sxs-lookup"><span data-stu-id="d4b6a-141">4. You use variables to store values</span></span>

<span data-ttu-id="d4b6a-142">È possibile archiviare i valori in una *variabile*, incluse stringhe, numeri e date, e così via. Si crea una nuova variabile usando la `Dim` (parola chiave).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="d4b6a-143">È possibile inserire i valori delle variabili direttamente in una pagina mediante `@`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="d4b6a-144">Il risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-144">The result displayed in a browser:</span></span>

![Razor Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="d4b6a-146">5. Valori letterali stringa racchiuderlo tra virgolette doppie</span><span class="sxs-lookup"><span data-stu-id="d4b6a-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="d4b6a-147">Oggetto *stringa* è una sequenza di caratteri che vengono considerati come testo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="d4b6a-148">Per specificare una stringa, si racchiuderlo tra virgolette doppie:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="d4b6a-149">Per incorporare le virgolette doppie all'interno di un valore stringa, inserire due caratteri di virgoletta doppia.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="d4b6a-150">Se si desidera che il carattere di virgoletta doppia presente una volta nell'output della pagina, immettere il valore `""` all'interno di virgolette di stringhe e se si desidera venga visualizzato due volte, immetterlo come `""""` all'interno della stringa tra virgolette.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="d4b6a-151">Il risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-151">The result displayed in a browser:</span></span>

![Razor Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="d4b6a-153">6. Codice Visual Basic non fa distinzione maiuscole / minuscole</span><span class="sxs-lookup"><span data-stu-id="d4b6a-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="d4b6a-154">Il linguaggio Visual Basic non distinzione maiuscole / minuscole.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="d4b6a-155">Parole chiave di programmazione (ad esempio `Dim`, `If`, e `True`) e i nomi delle variabili (come `myString`, o `subTotal`) possono essere scritti in ogni caso.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="d4b6a-156">Le seguenti righe di codice assegna un valore alla variabile `lastname` usando una minuscola assegnare un nome e quindi restituito il valore della variabile di pagina utilizzando un nome di lettere maiuscole.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="d4b6a-157">Il risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="d4b6a-159">7. Gran parte della codifica implica l'utilizzo di oggetti</span><span class="sxs-lookup"><span data-stu-id="d4b6a-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="d4b6a-160">Un oggetto rappresenta una cosa che è possibile programmare con &#8212; una pagina di una casella di testo, un file, un'immagine, una richiesta web, un messaggio di posta elettronica, un record del cliente (riga del database), e così via. Gli oggetti hanno proprietà che descrivono le caratteristiche di &#8212; dispone di un oggetto casella di testo una `Text` proprietà, un oggetto della richiesta ha un `Url` proprietà, un messaggio di posta elettronica ha una `From` proprietà e un oggetto customer ha una `FirstName` proprietà.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="d4b6a-161">Oggetti contengono anche i metodi che sono le &quot;verbi&quot; possono eseguire.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="d4b6a-162">Un oggetto di file sono esempi `Save` metodo, dell'oggetto image `Rotate` metodo e un oggetto messaggio di posta elettronica `Send` (metodo).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="d4b6a-163">Spesso si userà il `Request` i campi oggetto, che fornisce informazioni quali i valori del form nella pagina (caselle di testo, e così via), il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via. In questo esempio viene illustrato come accedere alle proprietà del `Request` oggetto e come chiamare il `MapPath` metodo il `Request` oggetto, che fornisce il percorso assoluto della pagina nel server:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="d4b6a-164">Il risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-164">The result displayed in a browser:</span></span>

![Razor Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="d4b6a-166">8. È possibile scrivere codice che prende decisioni</span><span class="sxs-lookup"><span data-stu-id="d4b6a-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="d4b6a-167">Una funzionalità chiave di pagine web dinamiche è che è possibile determinare quali operazioni eseguire in base alle condizioni.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="d4b6a-168">Il modo più comune per eseguire questa operazione è con il `If` istruzione (e facoltative `Else` istruzione).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="d4b6a-169">L'istruzione `If IsPost` è un modo abbreviato per la scrittura `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="d4b6a-170">Insieme a `If` istruzioni, esistono diversi modi per verificare le condizioni, ripetere i blocchi di codice, e così via, che sono descritte più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="d4b6a-171">Il risultato visualizzato in un browser (dopo aver fatto clic **Submit**):</span><span class="sxs-lookup"><span data-stu-id="d4b6a-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="d4b6a-173">**HTTP GET e i metodi POST e la proprietà istruzione IsPost**</span><span class="sxs-lookup"><span data-stu-id="d4b6a-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="d4b6a-174">Il protocollo usato per le pagine web (HTTP) supporta un numero molto limitato di metodi (&quot;verbi&quot;) che consentono di effettuare richieste al server.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="d4b6a-175">Le due cause più comuni sono GET, che viene usato per leggere una pagina, e POST, che consente di inviare una pagina.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="d4b6a-176">In generale, la prima volta che un utente richiede una pagina, la pagina viene richiesta con GET.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="d4b6a-177">Se l'utente inserisce in un form e quindi fa clic su **Submit**, il browser invia una richiesta POST al server.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="d4b6a-178">Nella programmazione web, è spesso utile sapere se una pagina viene richiesta come un'operazione GET o un POST in modo da sapere come elaborare la pagina.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="d4b6a-179">In ASP.NET Web Pages, è possibile usare il `IsPost` proprietà per vedere se una richiesta è un'operazione GET o POST.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="d4b6a-180">Se la richiesta viene pubblicato un POST, il `IsPost` proprietà restituirà true ed è possibile eseguire operazioni come leggere i valori di caselle di testo in un form.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="d4b6a-181">Molti esempi verrà visualizzato mostrano come elaborare la pagina in modo diverso a seconda del valore di `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="d4b6a-182">Un semplice esempio di codice</span><span class="sxs-lookup"><span data-stu-id="d4b6a-182">A Simple Code Example</span></span>

<span data-ttu-id="d4b6a-183">Questa procedura illustra come creare una pagina che illustra le tecniche di programmazione di base.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="d4b6a-184">Nell'esempio, si crea una pagina che consente agli utenti di immettere due numeri e quindi li aggiunge e viene visualizzato il risultato.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="d4b6a-185">Nell'editor di creare un nuovo file e denominarlo *AddNumbers.vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="d4b6a-186">Copiare il codice e il markup seguente alla pagina, sostituendo qualsiasi elemento presente nella pagina.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="d4b6a-187">Ecco alcuni aspetti notare:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="d4b6a-188">Il `@` carattere viene avviato il primo blocco di codice nella pagina e precede la `totalMessage` variabile incorporato nella parte inferiore.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="d4b6a-189">Il blocco nella parte superiore della pagina è racchiuso tra parentesi `Code...End Code`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="d4b6a-190">Le variabili `total`, `num1`, `num2`, e `totalMessage` archiviare diversi numeri e una stringa.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="d4b6a-191">Il valore di stringa letterale assegnato al `totalMessage` variabile è racchiuso tra virgolette doppie.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="d4b6a-192">Poiché il codice Visual Basic viene fatta distinzione tra maiuscole e minuscole, non quando il `totalMessage` variabile viene utilizzata nella parte inferiore della pagina, il relativo nome deve solo in modo che corrisponda l'ortografici di dichiarazione di variabile nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="d4b6a-193">Le maiuscole e minuscole è irrilevante.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="d4b6a-194">L'espressione `num1.AsInt()`  +  `num2.AsInt()` viene illustrato come lavorare con gli oggetti e metodi.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="d4b6a-195">Il `AsInt` metodo su ogni variabile Converte la stringa immessa dall'utente a un numero intero (integer) che possono essere aggiunti.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="d4b6a-196">Il `<form>` tag include un `method="post"` attributo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="d4b6a-197">Specifica che quando l'utente sceglie **Add**, la pagina verrà inviata al server usando il metodo HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="d4b6a-198">Quando l'invio della pagina, il codice `If IsPost` restituisce true, il parametro condizionale viene eseguito, visualizzare il risultato della somma i numeri di codice.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="d4b6a-199">Salvare la pagina ed eseguirlo in un browser.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="d4b6a-200">(Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.) Immettere due numeri interi, quindi scegliere il **Add** pulsante.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="d4b6a-202">La sintassi e linguaggio Visual Basic</span><span class="sxs-lookup"><span data-stu-id="d4b6a-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="d4b6a-203">In precedenza è stato illustrato un esempio su come creare una pagina web ASP.NET e come è possibile aggiungere il codice lato server per il markup HTML.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="d4b6a-204">Qui si apprenderanno i concetti fondamentali dell'utilizzo di Visual Basic per scrivere il codice server ASP.NET tramite la sintassi Razor &#8212; , ovvero le regole del linguaggio di programmazione.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="d4b6a-205">Se è esperti di programmazione (in particolare se è stato usato C, C++, c#, Visual Basic o JavaScript), gran parte delle quali è leggere qui risulteranno familiare.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="d4b6a-206">Si sarà probabilmente necessario acquisire familiarità con solo come markup in viene aggiunto codice WebMatrix *vbhtml* file.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a>  <span data-ttu-id="d4b6a-207">Combinazione di testo, markup e codice nei blocchi di codice</span><span class="sxs-lookup"><span data-stu-id="d4b6a-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="d4b6a-208">Nei blocchi di codice server, è spesso opportuno per l'output di testo e markup alla pagina.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="d4b6a-209">Se un blocco di codice server contiene testo che non è codice e che invece deve essere eseguito il rendering è, ASP.NET deve essere in grado di distinguere tale testo dal codice.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="d4b6a-210">Esistono diversi modi per eseguire tale operazione.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-210">There are several ways to do this.</span></span>

- <span data-ttu-id="d4b6a-211">Racchiudere il testo in un blocco di HTML, ad esempio `<p></p>` o `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="d4b6a-212">L'elemento HTML può includere testo, altri elementi HTML ed espressioni di codice lato server.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="d4b6a-213">Se ASP.NET rileva il tag HTML di apertura (ad esempio, `<p>`), esegue il rendering di tutti gli elementi dell'elemento e il proprio contenuto come consiste nel browser (e risolve le espressioni di codice lato server).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="d4b6a-214">Usare la `@:` operatore o `<text>` elemento.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="d4b6a-215">Il `@:` restituisce una singola riga di contenuto che contengono testo normale o tag HTML senza corrispondenza; il `<text>` elemento racchiude più righe di output.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="d4b6a-216">Queste opzioni sono utili quando non si vuole eseguire il rendering di un elemento HTML come parte dell'output.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="d4b6a-217">Nell'esempio seguente ripete l'esempio precedente ma usa una singola coppia di `<text>` tag per racchiudere il testo per il rendering.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="d4b6a-218">Nell'esempio seguente, il `<text>` e `</text>` tag racchiudono tre righe, ognuna delle quali dispone di testo non contenuto e i tag HTML non corrispondenti (`<br />`), insieme a codice lato server e i tag HTML corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="d4b6a-219">Anche in questo caso, è possibile anche far precedere a ogni riga singolarmente con il `@:` operatore; entrambi funzionano modo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="d4b6a-220">Quando il testo è l'output come mostrato in questa sezione &#8212; utilizzando un elemento HTML, il `@:` operatore o il `<text>` elemento &#8212; ASP.NET non di codifica HTML di output.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="d4b6a-221">(Come indicato in precedenza, ASP.NET codificare l'output di espressioni di codice server e i blocchi di codice server preceduti da `@`, ad eccezione dei casi speciali elencati in questa sezione.)</span><span class="sxs-lookup"><span data-stu-id="d4b6a-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="d4b6a-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="d4b6a-222">Whitespace</span></span>

<span data-ttu-id="d4b6a-223">Gli spazi aggiuntivi in un'istruzione (e all'esterno di un valore letterale stringa) non influiscono sull'istruzione:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="d4b6a-224">Suddividere lunghe istruzioni in più righe</span><span class="sxs-lookup"><span data-stu-id="d4b6a-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="d4b6a-225">È possibile suddividere un'istruzione di codice lunghi in più righe utilizzando il carattere di sottolineatura `_` (che in Visual Basic viene chiamato il *carattere di continuazione*) dopo ogni riga di codice.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="d4b6a-226">Per interrompere un'istruzione alla riga successiva, aggiungere uno spazio e quindi il carattere di continuazione alla fine della riga.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="d4b6a-227">Continuare l'istruzione nella riga successiva.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-227">Continue the statement on the next line.</span></span> <span data-ttu-id="d4b6a-228">È possibile eseguire il wrapping di istruzioni in tutte le righe in base alle esigenze migliorare la leggibilità.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="d4b6a-229">Le istruzioni seguenti sono gli stessi:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="d4b6a-230">È, tuttavia, non è possibile eseguire il wrapping di una riga all'interno di un valore letterale stringa.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="d4b6a-231">Nell'esempio seguente non funziona:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="d4b6a-232">Per combinare una stringa lunga che esegue il wrapping su più righe, ad esempio il codice sopra riportato, è necessario utilizzare il *operatore di concatenazione* (`&`), si vedrà più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="d4b6a-233">Commenti del codice</span><span class="sxs-lookup"><span data-stu-id="d4b6a-233">Code comments</span></span>

<span data-ttu-id="d4b6a-234">Commenti è possibile lasciare note per se stessi o ad altri utenti.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="d4b6a-235">I commenti di sintassi Razor sono preceduti `@*` e terminare con `*@`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="d4b6a-236">All'interno di blocchi di codice è possibile usare i commenti di sintassi Razor, o è possibile usare il carattere ordinario Visual Basic commento, ovvero una virgoletta singola (`'`) come prefisso a ogni riga.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="d4b6a-237">Variabili</span><span class="sxs-lookup"><span data-stu-id="d4b6a-237">Variables</span></span>

<span data-ttu-id="d4b6a-238">Una variabile è un oggetto denominato che viene usato per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="d4b6a-239">È possibile assegnare variabili di qualsiasi oggetto, ma il nome deve iniziare con un carattere alfabetico e non può contenere spazi vuoti o caratteri riservati.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="d4b6a-240">In Visual Basic, come illustrato in precedenza, non è rilevante nel caso delle lettere nel nome di una variabile.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="d4b6a-241">Le variabili e tipi di dati</span><span class="sxs-lookup"><span data-stu-id="d4b6a-241">Variables and data types</span></span>

<span data-ttu-id="d4b6a-242">Una variabile può avere un tipo di dati specifico, che indica il tipo di dati viene archiviato nella variabile.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="d4b6a-243">È possibile avere le variabili di stringa che archiviano i valori stringa (ad esempio &quot;Hello world&quot;), le variabili integer che archiviano valori di numeri interi (ad esempio, 3 o 79) e le variabili di date che archiviano i valori delle date in un'ampia gamma di formati (ad esempio, marzo 2009 o 4/12/2012 ).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="d4b6a-244">Ed esistono molti altri tipi di dati che è possibile usare.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="d4b6a-245">Tuttavia, non devi specificare un tipo per una variabile.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="d4b6a-246">Nella maggior parte dei casi ASP.NET può determinare il tipo di base sul modo in cui vengono usati i dati nella variabile.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="d4b6a-247">(In alcuni casi è necessario specificare un tipo; sono riportati esempi in cui questo è vero).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="d4b6a-248">Per dichiarare una variabile senza specificare un tipo, usare `Dim` più il nome di variabile (ad esempio, `Dim myVar`).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="d4b6a-249">Per dichiarare una variabile con un tipo, usare `Dim` più il nome della variabile, seguito da `As` e quindi il nome del tipo (ad esempio, `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="d4b6a-250">Nell'esempio seguente mostra alcune espressioni inline che usano le variabili in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="d4b6a-251">Il risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-251">The result displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="d4b6a-253">La conversione e i tipi di dati di test</span><span class="sxs-lookup"><span data-stu-id="d4b6a-253">Converting and testing data types</span></span>

<span data-ttu-id="d4b6a-254">Anche se ASP.NET in genere possibile determinare automaticamente un tipo di dati, a volte questo non è possibile.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="d4b6a-255">Pertanto, è necessario supporto ASP.NET mediante l'esecuzione di una conversione esplicita.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="d4b6a-256">Anche se non è necessario convertire i tipi, a volte è utile eseguire un test per vedere quali tipi di dati si lavora con.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="d4b6a-257">Il caso più comune è che è necessario convertire una stringa in un altro tipo, ad esempio in un numero intero o Data.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="d4b6a-258">Nell'esempio seguente viene illustrato un caso tipico in cui è necessario convertire una stringa in un numero.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="d4b6a-259">Di norma, input dell'utente vengono recapitati come stringhe.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="d4b6a-260">Anche se è stato richiesto all'utente di immettere un numero e anche se è stato immesso una cifra, quando viene inviato l'input dell'utente e di leggerlo nel codice, i dati sono in formato stringa.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="d4b6a-261">Pertanto, è necessario convertire la stringa in un numero.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="d4b6a-262">Nell'esempio, se si prova a eseguire operazioni aritmetiche sui valori senza doverle convertire in, l'errore seguente generato, perché ASP.NET non è possibile aggiungere due stringhe:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="d4b6a-263">Per convertire i valori interi, si chiama il `AsInt` (metodo).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="d4b6a-264">Se la conversione ha esito positivo, è quindi possibile aggiungere i numeri.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="d4b6a-265">La tabella seguente elenca alcuni metodi di conversione e di test comuni per le variabili.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-265">The following table lists some common conversion and test methods for variables.</span></span>


:::row:::
    :::column:::
        <strong>Method</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Example</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Converts a string that represents a whole number (like &quot;593&quot;) to an integer.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number. (In ASP.NET, a decimal number is more precise than a floating-point number.)
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Converts a string that represents a date and time value to the ASP.NET `DateTime` type.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Converts any other data type to a string.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::


## <a name="operators"></a><span data-ttu-id="d4b6a-266">Operatori</span><span class="sxs-lookup"><span data-stu-id="d4b6a-266">Operators</span></span>

<span data-ttu-id="d4b6a-267">Un operatore è una parola chiave o un carattere che indica ad ASP.NET che tipo di comando da eseguire in un'espressione.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-267">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="d4b6a-268">Visual Basic supporta molti operatori, ma è sufficiente riconoscere alcune per iniziare a sviluppare le pagine web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-268">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="d4b6a-269">Nella tabella seguente sono riepilogati gli operatori più comuni.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-269">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Examples</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        Math operators used in numerical expressions.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Assignment and equality. Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Inequality. Returns `True` if the values are not equal.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Less than, greater than, less than or equal, and greater than or equal.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Concatenation, which is used to join strings.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        The increment and decrement operators, which add and subtract 1 (respectively) from a variable.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Dot. Used to distinguish objects and their properties and methods.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parentheses. Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Not. Reverses a true value to false and vice versa. Typically used as a shorthand way to test for `False` (that is, for not `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        Logical AND and OR, which are used to link conditions together.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="d4b6a-270">Utilizzo di File e i percorsi delle cartelle nel codice</span><span class="sxs-lookup"><span data-stu-id="d4b6a-270">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="d4b6a-271">Sarà spesso lavorano con i percorsi di file e cartelle nel codice.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-271">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="d4b6a-272">Ecco un esempio di struttura di cartelle fisico per un sito Web come potrebbe apparire nel computer di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-272">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="d4b6a-273">Ecco alcuni dettagli essenziali sui percorsi e gli URL:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-273">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="d4b6a-274">Un URL inizia con un nome di dominio (`http://www.example.com`) o un nome di server (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-274">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="d4b6a-275">Un URL corrisponde a un percorso fisico in un computer host.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-275">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="d4b6a-276">Ad esempio, `http://myserver` possono corrispondere alla cartella *C:\websites\mywebsite* nel server.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-276">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="d4b6a-277">Un percorso virtuale è a sintassi abbreviata per rappresentare i percorsi nel codice senza la necessità di specificare il percorso completo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-277">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="d4b6a-278">Include la parte di un URL che segue il nome di dominio o server.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-278">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="d4b6a-279">Quando si usano i percorsi virtuali, è possibile spostare il codice a un dominio diverso o un server senza la necessità di aggiornare i percorsi.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-279">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="d4b6a-280">Ecco un esempio che consentono di comprendere le differenze:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-280">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="d4b6a-281">URL completo</span><span class="sxs-lookup"><span data-stu-id="d4b6a-281">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="d4b6a-282">Nome del server</span><span class="sxs-lookup"><span data-stu-id="d4b6a-282">Server name</span></span> | <span data-ttu-id="d4b6a-283">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="d4b6a-283">*mycompanyserver*</span></span> |
| <span data-ttu-id="d4b6a-284">Percorso virtuale</span><span class="sxs-lookup"><span data-stu-id="d4b6a-284">Virtual path</span></span> | <span data-ttu-id="d4b6a-285">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="d4b6a-285">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="d4b6a-286">Percorso fisico</span><span class="sxs-lookup"><span data-stu-id="d4b6a-286">Physical path</span></span> | <span data-ttu-id="d4b6a-287">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="d4b6a-287">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="d4b6a-288">È la radice virtuale /, esattamente come la radice dell'unità c: è unità \.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-288">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="d4b6a-289">(I percorsi delle cartelle virtuali sempre usano le barre). Il percorso virtuale di una cartella non deve necessariamente avere lo stesso nome della cartella fisica; può trattarsi di un alias.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-289">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="d4b6a-290">(Nei server di produzione, il percorso virtuale raramente corrisponde a un percorso fisico esatto)</span><span class="sxs-lookup"><span data-stu-id="d4b6a-290">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="d4b6a-291">Quando si lavora con i file e cartelle nel codice, in alcuni casi è necessario fare riferimento al percorso fisico e in alcuni casi un percorso virtuale, a seconda di quali oggetti si lavora con.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-291">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="d4b6a-292">ASP.NET fornisce questi strumenti per l'uso di percorsi di file e cartelle nel codice: il `Server.MapPath` metodo e il `~` operatore e `Href` (metodo).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-292">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="d4b6a-293">La conversione dei percorsi virtuali a fisici: il metodo server. MapPath</span><span class="sxs-lookup"><span data-stu-id="d4b6a-293">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="d4b6a-294">Il `Server.MapPath` metodo converte un percorso virtuale (ad esempio */default.cshtml*) in un percorso fisico assoluto (ad esempio *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-294">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="d4b6a-295">Utilizzare questo metodo ogni volta che è necessario un percorso fisico completo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-295">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="d4b6a-296">Un esempio tipico è quando si esegue la lettura o la scrittura di un file di testo o file di immagine nel server web.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-296">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="d4b6a-297">In genere non conosce il percorso fisico assoluto del sito nel server del sito di hosting, in modo che questo metodo consente di convertire il percorso si conosce, ovvero il percorso virtuale, ovvero per il percorso corrispondente nel server per l'utente.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-297">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="d4b6a-298">Si passa il percorso virtuale in un file o cartella in cui il metodo e restituisce il percorso fisico:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-298">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="d4b6a-299">La radice virtuale di riferimento: il ~ operatore e il metodo di Href</span><span class="sxs-lookup"><span data-stu-id="d4b6a-299">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="d4b6a-300">In un *. cshtml* oppure *vbhtml* file, è possibile fare riferimento nel percorso radice virtuale usando il `~` operatore.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-300">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="d4b6a-301">Ciò è molto utile perché pagine è possibile spostarsi in un sito e tutti i collegamenti che ad altre pagine contengono non saranno interrotti.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-301">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="d4b6a-302">È anche utile nel caso in cui si sposta mai il sito Web in un percorso diverso.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-302">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="d4b6a-303">Ecco alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-303">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="d4b6a-304">Se il sito Web è `http://myserver/myapp`, ecco come ASP.NET tratterà questi percorsi quando viene eseguita la pagina:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-304">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="d4b6a-305">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="d4b6a-305">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="d4b6a-306">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="d4b6a-306">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="d4b6a-307">(Non viene effettivamente visualizzato questi percorsi come i valori della variabile, ma ASP.NET considererà i percorsi come se questo è ciò che fossero).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-307">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="d4b6a-308">È possibile usare il `~` operatore nel codice server (come sopra) sia nel markup, simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-308">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="d4b6a-309">Nel markup, si utilizza il `~` operatore per creare percorsi di risorse, ad esempio i file di immagine, altre pagine web e file CSS.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-309">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="d4b6a-310">Quando viene eseguita la pagina, ASP.NET cerca tramite la pagina (codice e markup) e risolve tutti i `~` riferimenti al percorso appropriato.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-310">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="d4b6a-311">I cicli e la logica condizionale</span><span class="sxs-lookup"><span data-stu-id="d4b6a-311">Conditional Logic and Loops</span></span>

<span data-ttu-id="d4b6a-312">Codice server ASP.NET consente di eseguire attività in base a condizioni e scrivere il codice che si ripete le istruzioni di codice, ovvero un numero specifico di volte che viene eseguito un ciclo).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-312">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="d4b6a-313">Le condizioni di test</span><span class="sxs-lookup"><span data-stu-id="d4b6a-313">Testing conditions</span></span>

<span data-ttu-id="d4b6a-314">Per testare una condizione semplice è usare il `If...Then` istruzione che restituisce `True` o `False` basato su un test specificato:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-314">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="d4b6a-315">Il `If` (parola chiave) viene avviato un blocco.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-315">The `If` keyword starts a block.</span></span> <span data-ttu-id="d4b6a-316">Il test effettivo (condizione) segue il `If` (parola chiave) e restituisce true o false.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-316">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="d4b6a-317">Il `If` termina con l'istruzione `Then`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-317">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="d4b6a-318">Le istruzioni che verranno eseguita se il test è true siano racchiusi `If` e `End If`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-318">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="d4b6a-319">Un' `If` istruzione può includere un `Else` blocco che consente di specificare istruzioni da eseguire se la condizione è false:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-319">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="d4b6a-320">Se un' `If` istruzione avvia un blocco di codice, non è necessario usare la normale `Code...End Code` istruzioni per includere i blocchi.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-320">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="d4b6a-321">È possibile aggiungere semplicemente `@` al blocco, e il corretto funzionamento.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-321">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="d4b6a-322">Questo approccio funziona con `If` , nonché altre parole chiave che sono seguite da blocchi di codice, tra cui di programmazione Visual Basic `For`, `For Each`, `Do While`e così via.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-322">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="d4b6a-323">È possibile aggiungere più condizioni usando uno o più `ElseIf` blocchi:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-323">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="d4b6a-324">In questo esempio, se la prima condizione nella `If` blocco non è impostato su true, il `ElseIf` condizione viene confrontata.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-324">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="d4b6a-325">Se tale condizione viene soddisfatta, le istruzioni di `ElseIf` blocco vengono eseguite.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-325">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="d4b6a-326">Se nessuna delle condizioni viene soddisfatta, le istruzioni di `Else` blocco vengono eseguite.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-326">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="d4b6a-327">È possibile aggiungere un numero qualsiasi di `ElseIf` blocca e quindi chiudere con un' `Else` block come le &quot;tutto il resto&quot; condizione.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-327">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="d4b6a-328">Per testare un numero elevato di condizioni, usare un `Select Case` blocco:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-328">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="d4b6a-329">Valore da testare è racchiuso tra parentesi (nell'esempio, la variabile del giorno della settimana).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-329">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="d4b6a-330">Ogni singolo test viene utilizzato un `Case` istruzione contenente un valore.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-330">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="d4b6a-331">Se il valore di una `Case` istruzione corrisponde al valore di test, il codice che `Case` blocco viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-331">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="d4b6a-332">Il risultato di ultimi due blocchi condizionali visualizzati in un browser:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-332">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="d4b6a-334">Codice di ciclo</span><span class="sxs-lookup"><span data-stu-id="d4b6a-334">Looping code</span></span>

<span data-ttu-id="d4b6a-335">È spesso necessario eseguire ripetutamente le stesse istruzioni.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-335">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="d4b6a-336">Eseguire questa operazione dal ciclo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-336">You do this by looping.</span></span> <span data-ttu-id="d4b6a-337">Ad esempio, si esegue spesso le stesse istruzioni per ogni elemento in una raccolta di dati.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-337">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="d4b6a-338">Se si conosce esattamente quante volte si desidera eseguire un ciclo, è possibile usare un `For` ciclo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-338">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="d4b6a-339">Questo tipo di ciclo è particolarmente utile per il conteggio dei o il conteggio verso il basso:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-339">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="d4b6a-340">Il ciclo inizia con la `For` (parola chiave), seguita da tre elementi:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-340">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="d4b6a-341">Immediatamente dopo il `For` istruzione, si dichiara una variabile contatore (non è necessario usare `Dim`) e quindi indicare l'intervallo, come in `i = 10 to 20`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-341">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="d4b6a-342">Ciò significa che la variabile `i` verrà avviare il conteggio da 10 e continuare fino a raggiungere 20 (inclusivo).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-342">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="d4b6a-343">Tra i `For` e `Next` istruzioni è il contenuto del blocco.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-343">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="d4b6a-344">Può contenere uno o più istruzioni di codice che eseguono con ogni ciclo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-344">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="d4b6a-345">Il `Next i` istruzione termina il ciclo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-345">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="d4b6a-346">Incrementa il contatore e inizia l'iterazione successiva del ciclo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-346">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="d4b6a-347">La riga di codice tra il `For` e `Next` righe contiene il codice eseguito per ogni iterazione del ciclo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-347">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="d4b6a-348">Il codice crea un nuovo paragrafo (`<p>` elemento) ogni volta e si aggiunge una riga all'output, visualizzando il valore dei (il contatore).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-348">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="d4b6a-349">Quando si esegue questa pagina, l'esempio crea 11 righe visualizzando l'output, con il testo in ogni riga che indica il numero dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-349">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="d4b6a-351">Se si lavora con una raccolta o una matrice, usano spesso un `For Each` ciclo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-351">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="d4b6a-352">Una raccolta è un gruppo di oggetti simili e il `For Each` ciclo consente eseguire un'attività su ogni elemento nella raccolta.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-352">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="d4b6a-353">Questo tipo di ciclo è utile per le raccolte, perché a differenza di un `For` ciclo, non è necessario incrementare il contatore o impostare un limite.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-353">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="d4b6a-354">Al contrario, il `For Each` codice del ciclo procede semplicemente tramite la raccolta venga completata.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-354">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="d4b6a-355">Gli elementi in questo esempio vengono restituite le `Request.ServerVariables` insieme (che contiene informazioni relative al server web).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-355">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="d4b6a-356">Usa un' `For Each` per visualizzare il nome di ogni elemento creando un nuovo ciclo `<li>` elemento in un elenco puntato di HTML.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-356">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="d4b6a-357">Il `For Each` parola chiave è seguita da una variabile che rappresenta un singolo elemento della raccolta (nell'esempio `myItem`), seguito dal `In` (parola chiave), la raccolta che si desidera scorrere in ciclo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-357">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="d4b6a-358">Nel corpo del `For Each` ciclo, è possibile accedere all'elemento corrente usando la variabile dichiarata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-358">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="d4b6a-360">Per creare un ciclo più generico, usare il `Do While` istruzione:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-360">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="d4b6a-361">Questo ciclo inizia con la `Do While` (parola chiave), seguita da una condizione, seguita dal blocco di ripetizione.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-361">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="d4b6a-362">In genere incrementare i cicli (aggiungere) o di decremento (sottrarre) una variabile o un oggetto utilizzato per il conteggio.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-362">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="d4b6a-363">Nell'esempio di `+=` operatore aggiunge 1 al valore di una variabile ogni volta che viene eseguito il ciclo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-363">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="d4b6a-364">(Per decrementano una variabile in un ciclo che conta verso il basso, si utilizzerebbe l'operatore di decremento `-=`.)</span><span class="sxs-lookup"><span data-stu-id="d4b6a-364">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="d4b6a-365">Oggetti e raccolte</span><span class="sxs-lookup"><span data-stu-id="d4b6a-365">Objects and Collections</span></span>

<span data-ttu-id="d4b6a-366">Quasi tutti gli elementi in un sito Web ASP.NET è un oggetto, tra cui la pagina web stessa.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-366">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="d4b6a-367">Questa sezione illustra alcuni oggetti importante che si utilizzerà spesso nel codice.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-367">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="d4b6a-368">Oggetti della pagina</span><span class="sxs-lookup"><span data-stu-id="d4b6a-368">Page objects</span></span>

<span data-ttu-id="d4b6a-369">L'oggetto di base in ASP.NET è la pagina.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-369">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="d4b6a-370">Si può accedere alle proprietà dell'oggetto pagina direttamente, senza alcun oggetto qualificato.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-370">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="d4b6a-371">Il codice seguente ottiene il percorso di file della pagina, tramite il `Request` oggetto della pagina:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-371">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="d4b6a-372">È possibile usare le proprietà del `Page` oggetto da ottenere molte informazioni, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-372">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="d4b6a-373">`Request`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-373">`Request`.</span></span> <span data-ttu-id="d4b6a-374">Come abbiamo già visto, questa è una raccolta di informazioni sulla richiesta corrente, tra cui il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-374">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="d4b6a-375">`Response`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-375">`Response`.</span></span> <span data-ttu-id="d4b6a-376">Si tratta di una raccolta di informazioni sulla risposta che verrà inviata al browser al termine dell'esecuzione il codice del server (pagina).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-376">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="d4b6a-377">Ad esempio, è possibile usare questa proprietà per scrivere informazioni nella risposta.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-377">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="d4b6a-378">Oggetti della raccolta (matrici e dizionari)</span><span class="sxs-lookup"><span data-stu-id="d4b6a-378">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="d4b6a-379">Una raccolta è un gruppo di oggetti dello stesso tipo, ad esempio una raccolta di `Customer` oggetti da un database.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-379">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="d4b6a-380">ASP.NET contiene molte raccolte predefinite, ad esempio il `Request.Files` raccolta.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-380">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="d4b6a-381">Sarà spesso lavorano con i dati nelle raccolte.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-381">You'll often work with data in collections.</span></span> <span data-ttu-id="d4b6a-382">Due tipi di raccolte comuni sono le *matrice* e il *dizionario*.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-382">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="d4b6a-383">Una matrice è utile quando si vuole archiviare un insieme di elementi simili, ma non si vuole creare una variabile separata per contenere ciascun elemento:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-383">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="d4b6a-384">Con le matrici, ad esempio si dichiara un tipo di dati specifico `String`, `Integer`, o `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-384">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="d4b6a-385">Per indicare che la variabile può contenere una matrice, aggiungere le parentesi per il nome della variabile nella dichiarazione (ad esempio `Dim myVar() As String`).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-385">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="d4b6a-386">È possibile accedere agli elementi in una matrice usando la loro posizione (indice) o tramite il `For Each` istruzione.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-386">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="d4b6a-387">Gli indici di matrice sono in base zero in &#8212; , ovvero il primo elemento è nella posizione 0, il secondo elemento alla posizione 1 e così via.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-387">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="d4b6a-388">È possibile determinare il numero di elementi in una matrice tramite il recupero relativo `Length` proprietà.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-388">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="d4b6a-389">Per ottenere la posizione di un elemento specifico nella matrice (vale a dire, la ricerca nella matrice), usare il `Array.IndexOf` (metodo).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-389">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="d4b6a-390">È anche possibile eseguire operazioni come reverse il contenuto di una matrice (il `Array.Reverse` metodo) oppure ordinare il contenuto (il `Array.Sort` (metodo)).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-390">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="d4b6a-391">L'output del codice di matrice di stringa visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-391">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="d4b6a-393">Un dizionario è una raccolta di coppie chiave/valore, in cui è fornire la chiave (o nome) per impostare o recuperare il valore corrispondente:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-393">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="d4b6a-394">Per creare un dizionario, usare il `New` parola chiave per indicare che si sta creando un nuovo `Dictionary` oggetto.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-394">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="d4b6a-395">È possibile assegnare un dizionario a una variabile utilizzando la `Dim` (parola chiave).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-395">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="d4b6a-396">Si indicano i tipi di dati degli elementi nel dizionario usando le parentesi ( `( )` ).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-396">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="d4b6a-397">Alla fine della dichiarazione, è necessario aggiungere un'altra coppia di parentesi, perché si tratta in realtà un metodo che crea un nuovo dizionario.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-397">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="d4b6a-398">Per aggiungere elementi al dizionario, è possibile chiamare il `Add` metodo per la variabile del dizionario (`myScores` in questo caso), quindi specificare una chiave e un valore.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-398">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="d4b6a-399">In alternativa, è possibile utilizzare le parentesi per indicare la chiave ed eseguire una semplice assegnazione, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-399">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="d4b6a-400">Per ottenere un valore dal dizionario, si specifica la chiave in parentesi:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-400">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="d4b6a-401">Chiamata di metodi con parametri</span><span class="sxs-lookup"><span data-stu-id="d4b6a-401">Calling Methods with Parameters</span></span>

<span data-ttu-id="d4b6a-402">Come illustrato in precedenza in questo articolo, gli oggetti che si programma con dispongono di metodi.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-402">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="d4b6a-403">Ad esempio, un `Database` oggetto potrebbe avere un `Database.Connect` (metodo).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-403">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="d4b6a-404">Molti metodi dispongono anche di uno o più parametri.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-404">Many methods also have one or more parameters.</span></span> <span data-ttu-id="d4b6a-405">Oggetto *parametro* è un valore che si passa a un metodo per abilitare il metodo completare l'attività.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-405">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="d4b6a-406">Ad esempio, esaminata una dichiarazione per il `Request.MapPath` metodo che accetta tre parametri:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-406">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="d4b6a-407">Questo metodo restituisce il percorso fisico sul server che corrisponde a un percorso virtuale specificato.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-407">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="d4b6a-408">I tre parametri per il metodo vengono `virtualPath`, `baseVirtualDir`, e `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-408">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="d4b6a-409">Si noti che nella dichiarazione, i parametri sono elencati con i tipi di dati dei dati che verranno accettano. Quando si chiama questo metodo, è necessario fornire valori per tutti i tre parametri.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-409">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="d4b6a-410">Quando si usa Visual Basic con sintassi Razor, sono disponibili due opzioni per passare parametri a un metodo: *parametri posizionali* oppure *parametri denominati*.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-410">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="d4b6a-411">Per chiamare un metodo usando i parametri posizionali, passare i parametri in un ordine fisso specificato nella dichiarazione del metodo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-411">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="d4b6a-412">(È in genere saprebbe quest'ordine leggendo la documentazione relativa al metodo.) È necessario seguire l'ordine e non è possibile ignorare i parametri &#8212; se necessario, si passa una stringa vuota (`""`) o null per un parametro posizionale che non si dispone di un valore per.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-412">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="d4b6a-413">Nell'esempio seguente si presuppone una cartella denominata *script* nel tuo sito Web.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-413">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="d4b6a-414">Il codice chiama il `Request.MapPath` metodo e passa i valori per i tre parametri nell'ordine corretto.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-414">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="d4b6a-415">Viene quindi visualizzato il percorso risulta mappato.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-415">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="d4b6a-416">Quando sono presenti numerosi parametri per un metodo, è possibile mantenere il codice più lineare e più leggibili utilizzando parametri denominati.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-416">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="d4b6a-417">Per chiamare un metodo utilizzando parametri denominati, specificare il nome del parametro seguito da `:=` e quindi specificare il valore.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-417">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="d4b6a-418">Un vantaggio dei parametri denominati è che è possibile aggiungerli in qualsiasi ordine desiderato.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-418">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="d4b6a-419">(Uno svantaggio è che la chiamata al metodo non è compresso).</span><span class="sxs-lookup"><span data-stu-id="d4b6a-419">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="d4b6a-420">Nell'esempio seguente chiama il metodo di stesso come illustrato in precedenza, ma utilizza parametri di fornire i valori denominati:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-420">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="d4b6a-421">Come può notare, i parametri vengono passati in un ordine diverso.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-421">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="d4b6a-422">Tuttavia, se si esegue l'esempio precedente e in questo esempio, verrà restituiscono lo stesso valore.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-422">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="d4b6a-423">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="d4b6a-423">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="d4b6a-424">Istruzioni Try-Catch</span><span class="sxs-lookup"><span data-stu-id="d4b6a-424">Try-Catch statements</span></span>

<span data-ttu-id="d4b6a-425">È necessario spesso istruzioni nel codice che potrebbe non riuscire per motivi di all'esterno del controllo.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-425">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="d4b6a-426">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d4b6a-426">For example:</span></span>

- <span data-ttu-id="d4b6a-427">Se il codice tenta di aprire, creare, leggere o scrivere un file, tutti i tipi di errori potrebbero verificarsi.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-427">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="d4b6a-428">Il file desiderato potrebbe non esistere, potrebbe essere stato bloccato, il codice potrebbe non disporre delle autorizzazioni e così via.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-428">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="d4b6a-429">Analogamente, se il codice tenta di aggiornare i record in un database, possono essere presenti problemi relativi alle autorizzazioni, la connessione al database potrebbe essere eliminata, i dati da salvare potrebbero essere non valido e così via.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-429">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="d4b6a-430">In termini di programmazione, queste situazioni sono denominate *eccezioni*.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-430">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="d4b6a-431">Se il codice rileva un'eccezione, viene generato (genera) un messaggio di errore nella migliore delle ipotesi, ovvero indesiderate agli utenti.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-431">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="d4b6a-433">In situazioni in cui il codice che si verifichino eccezioni e per evitare messaggi di errore di questo tipo, è possibile usare `Try/Catch` istruzioni.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-433">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="d4b6a-434">Nel `Try` istruzione, eseguire il codice che si sta archiviando.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-434">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="d4b6a-435">In uno o più `Catch` (istruzioni), è possibile cercare specifici errori (tipi specifici di eccezioni) che sono stati generati.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-435">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="d4b6a-436">È possibile includere un numero `Catch` istruzioni è necessario individuare errori che sta previsione.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-436">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="d4b6a-437">È consigliabile evitare di utilizzare il `Response.Redirect` metodo `Try/Catch` (istruzioni), poiché potrebbe causare un'eccezione nella pagina.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-437">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="d4b6a-438">Nell'esempio seguente mostra una pagina che consente di creare un file di testo per la prima richiesta e quindi visualizza un pulsante che consente all'utente di aprire il file.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-438">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="d4b6a-439">L'esempio Usa un nome file errato deliberatamente in modo che lo genererà un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-439">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="d4b6a-440">Il codice riporta `Catch` istruzioni per due possibili eccezioni: `FileNotFoundException`, che si verifica se il nome del file non è corretto, e `DirectoryNotFoundException`, che si verifica se ASP.NET non è possibile anche trovare la cartella.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-440">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="d4b6a-441">(È possibile rimuovere il commento nell'esempio, un'istruzione per verificarne l'esecuzione quando tutto funziona correttamente.)</span><span class="sxs-lookup"><span data-stu-id="d4b6a-441">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="d4b6a-442">Se il codice non gestisce l'eccezione, si vedrà una pagina di errore, ad esempio lo screenshot precedente.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-442">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="d4b6a-443">Tuttavia, il `Try/Catch` sezione aiuta a impedire all'utente di visualizzare questi tipi di errori.</span><span class="sxs-lookup"><span data-stu-id="d4b6a-443">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="d4b6a-444">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d4b6a-444">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="d4b6a-445">Documentazione di riferimento</span><span class="sxs-lookup"><span data-stu-id="d4b6a-445">Reference Documentation</span></span>

- [<span data-ttu-id="d4b6a-446">ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="d4b6a-446">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="d4b6a-447">Linguaggio Visual Basic</span><span class="sxs-lookup"><span data-stu-id="d4b6a-447">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
