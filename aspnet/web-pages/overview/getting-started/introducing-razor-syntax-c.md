---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introduzione alla programmazione Web ASP.NET usando la sintassi Razor (c#) | Microsoft Docs
author: tfitzmac
description: In questo capitolo offre una panoramica della programmazione con ASP.NET Web Pages con sintassi Razor. ASP.NET è una tecnologia Microsoft per l'esecuzione di indirizzo pa web dinamiche...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: b242bf52bbd63d726e6ce6ab7be01a1b81c5bf1b
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758258"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="6e1e3-104">Introduzione alla programmazione Web ASP.NET usando la sintassi Razor (c#)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>
====================
<span data-ttu-id="6e1e3-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6e1e3-106">Questo articolo offre una panoramica della programmazione con ASP.NET Web Pages con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="6e1e3-107">ASP.NET è la tecnologia Microsoft per l'esecuzione di pagine web dinamiche nei server web.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="6e1e3-108">Questo articoli è incentrata sulla usando il linguaggio di programmazione c#.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="6e1e3-109">**Si apprenderà**:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="6e1e3-110">Il principale 8 suggerimenti per l'introduzione a ASP.NET Web Pages con sintassi Razor di programmazione di programmazione.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="6e1e3-111">Concetti di programmazione di base che è necessario.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="6e1e3-112">Il codice server ASP.NET e la sintassi Razor è dedicato.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="6e1e3-113">Versioni del software</span><span class="sxs-lookup"><span data-stu-id="6e1e3-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="6e1e3-114">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="6e1e3-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="6e1e3-115">Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="6e1e3-116">8 suggerimenti di programmazione principali</span><span class="sxs-lookup"><span data-stu-id="6e1e3-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="6e1e3-117">In questa sezione sono elencati alcuni suggerimenti che è assolutamente necessario conoscere quando si inizia a scrivere codice server ASP.NET tramite la sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="6e1e3-118">La sintassi Razor è basata sul linguaggio di programmazione c#, e che è il linguaggio che viene usato spesso con ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="6e1e3-119">Tuttavia, la sintassi Razor supporta inoltre il linguaggio Visual Basic e tutto ciò che è anche possibile eseguire in Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="6e1e3-120">Per informazioni dettagliate, vedere l'appendice [linguaggio Visual Basic e la sintassi](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>


<span data-ttu-id="6e1e3-121">È possibile trovare altre informazioni sulla maggior parte di queste tecniche di programmazione più avanti nell'articolo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="6e1e3-122">1. Aggiungere codice a una pagina utilizzando il carattere @</span><span class="sxs-lookup"><span data-stu-id="6e1e3-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="6e1e3-123">Il `@` carattere avvia espressioni inline, blocchi di istruzioni singolo e blocchi a più istruzioni:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="6e1e3-124">Si tratta di queste istruzioni come appaiono quando la pagina viene eseguita in un browser:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="6e1e3-126">**La codifica HTML**</span><span class="sxs-lookup"><span data-stu-id="6e1e3-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="6e1e3-127">Quando si visualizza il contenuto in una pagina mediante la `@` carattere, come negli esempi precedenti, ASP.NET codifica in HTML di output.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="6e1e3-128">Questa impostazione sostituisce i caratteri riservati HTML (ad esempio `<` e `>` e `&`) con i codici che abilitano i caratteri da visualizzare come caratteri in una pagina web anziché essere interpretato come tag HTML o le entità.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="6e1e3-129">Senza codifica HTML, l'output dal codice server non vengano visualizzati correttamente e potrebbe esporre una pagina a rischi di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="6e1e3-130">Se l'obiettivo consiste nel markup HTML che esegue il rendering dei tag come markup di output (ad esempio `<p></p>` per un paragrafo o `<em></em>` per enfatizzare il testo), vedere la sezione [combinazione di testo, Markup e codice nei blocchi di codice](#BM_CombiningTextMarkupAndCode) più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="6e1e3-131">Altre informazioni sulla codifica HTML [utilizzo di moduli](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="6e1e3-132">2. Blocchi di codice racchiudere tra parentesi graffe</span><span class="sxs-lookup"><span data-stu-id="6e1e3-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="6e1e3-133">Oggetto *blocco di codice* include uno o più istruzioni di codice e viene racchiuso tra parentesi graffe.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="6e1e3-134">Il risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-134">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="6e1e3-136">3. All'interno di un blocco alla fine si ogni istruzione del codice con un punto e virgola</span><span class="sxs-lookup"><span data-stu-id="6e1e3-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="6e1e3-137">All'interno di un blocco di codice, ogni istruzione di codice completo deve terminare con un punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="6e1e3-138">Espressioni inline non terminano con un punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="6e1e3-139">4. Si usano le variabili per archiviare i valori</span><span class="sxs-lookup"><span data-stu-id="6e1e3-139">4. You use variables to store values</span></span>

<span data-ttu-id="6e1e3-140">È possibile archiviare i valori in una *variabile*, incluse stringhe, numeri e date, e così via. Si crea una nuova variabile usando la `var` (parola chiave).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="6e1e3-141">È possibile inserire i valori delle variabili direttamente in una pagina mediante `@`.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="6e1e3-142">Il risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-142">The result displayed in a browser:</span></span>

![Razor Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="6e1e3-144">5. Valori letterali stringa racchiuderlo tra virgolette doppie</span><span class="sxs-lookup"><span data-stu-id="6e1e3-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="6e1e3-145">Oggetto *stringa* è una sequenza di caratteri che vengono considerati come testo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="6e1e3-146">Per specificare una stringa, si racchiuderlo tra virgolette doppie:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="6e1e3-147">Se la stringa che si desidera visualizzare contiene un carattere barra rovesciata ( `\` ) o le virgolette doppie ( `"` ), utilizzare un *valore letterale stringa verbatim* che viene anteposto con il `@` operatore.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="6e1e3-148">(In c#, la \ carattere ha un significato speciale solo se si utilizza un valore letterale stringa verbatim.)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="6e1e3-149">Per incorporare le virgolette doppie, usare un valore letterale stringa verbatim e ripetere le virgolette:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="6e1e3-150">Ecco il risultato dell'uso di entrambi gli esempi in una pagina:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-150">Here's the result of using both of these examples in a page:</span></span>

![Razor Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="6e1e3-152">Si noti che la `@` carattere viene utilizzato per contrassegnare i valori letterali stringa verbatim in c# e per contrassegnare il codice nelle pagine ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>


### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="6e1e3-153">6. Codice viene fatta distinzione tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="6e1e3-153">6. Code is case sensitive</span></span>

<span data-ttu-id="6e1e3-154">In c#, le parole chiave (ad esempio `var`, `true`, e `if`) e i nomi delle variabili sono tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="6e1e3-155">Le righe di codice seguente creano due variabili diverse, `lastName` e `LastName.`</span><span class="sxs-lookup"><span data-stu-id="6e1e3-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="6e1e3-156">Se si dichiara una variabile come `var lastName = "Smith";` e, se si tenta di fare riferimento a tale variabile nella stessa `@LastName`, viene generato un errore perché `LastName` non riconosciuto.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-156">If you declare a variable as `var lastName = "Smith";` and if you try to reference that variable in your page as `@LastName`, an error results because `LastName` won't be recognized.</span></span>

> [!NOTE]
> <span data-ttu-id="6e1e3-157">In Visual Basic, parole chiave e le variabili siano *non* distinzione maiuscole / minuscole.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>


### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="6e1e3-158">7. Gran parte del codice sono necessari oggetti</span><span class="sxs-lookup"><span data-stu-id="6e1e3-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="6e1e3-159">Un' *oggetti* rappresenta un elemento che è possibile programmare con &#8212; una pagina di una casella di testo, un file, un'immagine, una richiesta web, un messaggio di posta elettronica, un record del cliente (riga del database), e così via. Gli oggetti hanno proprietà che descrivono le caratteristiche e che è possibile leggere o modificare &#8212; dispone di un oggetto casella di testo una `Text` dispone di un oggetto della richiesta di proprietà (tra gli altri), un `Url` proprietà, un messaggio di posta elettronica ha una `From` proprietà, e un oggetto customer ha una `FirstName` proprietà.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="6e1e3-160">Oggetti contengono anche i metodi che sono le &quot;verbi&quot; possono eseguire.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="6e1e3-161">Un oggetto di file sono esempi `Save` metodo, dell'oggetto image `Rotate` metodo e un oggetto messaggio di posta elettronica `Send` (metodo).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="6e1e3-162">Spesso si userà il `Request` dell'oggetto, che mostra informazioni quali i valori di caselle di testo (i campi del modulo) nella pagina, il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via. Nell'esempio seguente viene illustrato come accedere alle proprietà del `Request` oggetto e come chiamare il `MapPath` metodo il `Request` oggetto, che fornisce il percorso assoluto della pagina nel server:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="6e1e3-163">Il risultato visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-163">The result displayed in a browser:</span></span>

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="6e1e3-165">8. È possibile scrivere codice che prende decisioni</span><span class="sxs-lookup"><span data-stu-id="6e1e3-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="6e1e3-166">Una funzionalità chiave di pagine web dinamiche è che è possibile determinare quali operazioni eseguire in base alle condizioni.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="6e1e3-167">Il modo più comune per eseguire questa operazione è con il `if` istruzione (e facoltative `else` istruzione).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="6e1e3-168">L'istruzione `if(IsPost)` è un modo abbreviato per la scrittura `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="6e1e3-169">Insieme a `if` istruzioni, esistono diversi modi per verificare le condizioni, ripetere i blocchi di codice, e così via, che sono descritte più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="6e1e3-170">Il risultato visualizzato in un browser (dopo aver fatto clic **Submit**):</span><span class="sxs-lookup"><span data-stu-id="6e1e3-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="6e1e3-172">HTTP GET e i metodi POST e la proprietà istruzione IsPost</span><span class="sxs-lookup"><span data-stu-id="6e1e3-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="6e1e3-173">Il protocollo usato per le pagine web (HTTP) supporta un numero molto limitato di metodi (verbi) che consentono di effettuare richieste al server.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="6e1e3-174">Le due cause più comuni sono GET, che viene usato per leggere una pagina, e POST, che consente di inviare una pagina.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="6e1e3-175">In generale, la prima volta che un utente richiede una pagina, la pagina viene richiesta con GET.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="6e1e3-176">Se l'utente inserisce un form e quindi fa clic su un pulsante di invio, il browser invia una richiesta POST al server.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="6e1e3-177">Nella programmazione web, è spesso utile sapere se una pagina viene richiesta come un'operazione GET o un POST in modo da sapere come elaborare la pagina.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="6e1e3-178">In ASP.NET Web Pages, è possibile usare il `IsPost` proprietà per vedere se una richiesta è un'operazione GET o POST.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="6e1e3-179">Se la richiesta viene pubblicato un POST, il `IsPost` proprietà restituirà true ed è possibile eseguire operazioni come leggere i valori di caselle di testo in un form.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="6e1e3-180">Molti esempi verrà visualizzato mostrano come elaborare la pagina in modo diverso a seconda del valore di `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="6e1e3-181">Un semplice esempio di codice</span><span class="sxs-lookup"><span data-stu-id="6e1e3-181">A Simple Code Example</span></span>

<span data-ttu-id="6e1e3-182">Questa procedura illustra come creare una pagina che illustra le tecniche di programmazione di base.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="6e1e3-183">Nell'esempio, si crea una pagina che consente agli utenti di immettere due numeri e quindi li aggiunge e viene visualizzato il risultato.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="6e1e3-184">Nell'editor di creare un nuovo file e denominarlo *AddNumbers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="6e1e3-185">Copiare il codice e il markup seguente alla pagina, sostituendo qualsiasi elemento presente nella pagina.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="6e1e3-186">Ecco alcuni aspetti notare:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="6e1e3-187">Il `@` carattere viene avviato il primo blocco di codice nella pagina e precede la `totalMessage` variabile che viene incorporato nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="6e1e3-188">Il blocco nella parte superiore della pagina è racchiuso tra parentesi graffe.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="6e1e3-189">Nel blocco nella parte superiore, tutte le righe terminano con un punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="6e1e3-190">Le variabili `total`, `num1`, `num2`, e `totalMessage` archiviare diversi numeri e una stringa.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="6e1e3-191">Il valore di stringa letterale assegnato al `totalMessage` variabile è racchiuso tra virgolette doppie.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="6e1e3-192">Poiché il codice è tra maiuscole e minuscole, quando il `totalMessage` variabile viene utilizzata nella parte inferiore della pagina, il relativo nome deve corrispondere esattamente alla variabile nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="6e1e3-193">L'espressione `num1.AsInt() + num2.AsInt()` viene illustrato come lavorare con gli oggetti e metodi.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="6e1e3-194">Il `AsInt` metodo su ogni variabile Converte la stringa immessa dall'utente a un numero (integer) in modo che sia possibile eseguire operazioni aritmetiche su di esso.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="6e1e3-195">Il `<form>` tag include un `method="post"` attributo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="6e1e3-196">Specifica che quando l'utente sceglie **Add**, la pagina verrà inviata al server usando il metodo HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="6e1e3-197">Quando viene inviata la pagina, il `if(IsPost)` test restituisce true e il parametro condizionale viene eseguito, visualizzare il risultato della somma i numeri di codice.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="6e1e3-198">Salvare la pagina ed eseguirlo in un browser.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="6e1e3-199">(Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.) Immettere due numeri interi, quindi scegliere il **Add** pulsante.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="6e1e3-201">Concetti di programmazione di base</span><span class="sxs-lookup"><span data-stu-id="6e1e3-201">Basic Programming Concepts</span></span>

<span data-ttu-id="6e1e3-202">Questo articolo offre una panoramica della programmazione web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="6e1e3-203">Non è un esame esaustivo, solo una presentazione rapida tramite i concetti di programmazione che si userà più spesso.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="6e1e3-204">Anche in questo caso, viene descritto come quasi tutto ciò che occorre per iniziare a usare le pagine Web con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="6e1e3-205">Ma innanzitutto, una breve introduzione tecnica.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="6e1e3-206">La sintassi Razor, il codice lato Server e ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6e1e3-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="6e1e3-207">Sintassi Razor è una semplice sintassi di programmazione per l'incorporamento di codice basato su server in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="6e1e3-208">In una pagina web che usa la sintassi Razor, sono disponibili due tipi di contenuto: client del contenuto e codice lato server.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="6e1e3-209">Contenuto client sia le caratteristiche di cui si è abituati nelle pagine web: il markup HTML (elementi), le informazioni di stile, ad esempio CSS, forse alcuni script client, ad esempio JavaScript e testo normale.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="6e1e3-210">Sintassi Razor consente di aggiungere il codice lato server per il contenuto di client.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="6e1e3-211">Se è presente codice server nella pagina, il server esegue prima di tutto che il codice prima di inviare la pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="6e1e3-212">Tramite l'esecuzione nel server, il codice può eseguire attività che può essere molto più complessa da eseguire con i client a contenuti da solo, ad esempio l'accesso a database basati su server.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="6e1e3-213">Ancora più importante, il codice lato server può creare in modo dinamico contenuto client &#8212; può generare il markup HTML o altri contenuti in tempo reale e quindi inviarlo al browser e qualsiasi codice HTML statico che la pagina potrebbe contenere.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="6e1e3-214">Dal punto di vista del browser, client a contenuti che viene generato dal codice server non è diverso rispetto a qualsiasi altro contenuto client.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="6e1e3-215">Come abbiamo già visto, il codice lato server che ha richiesto è piuttosto semplice.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="6e1e3-216">Pagine web ASP.NET che includono la sintassi Razor hanno un'estensione di file speciale (*. cshtml* oppure *vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="6e1e3-217">Il server riconosce queste estensioni, viene eseguito il codice che è contrassegnato con sintassi Razor e invia quindi la pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="6e1e3-218">Dove è ASP.NET adatta?</span><span class="sxs-lookup"><span data-stu-id="6e1e3-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="6e1e3-219">Sintassi Razor è basata su una tecnologia di Microsoft chiamata ASP.NET, che a sua volta è basato su Microsoft .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="6e1e3-220">.NET Framework è un framework di programmazione di big data, completa di Microsoft per lo sviluppo di praticamente qualsiasi tipo di applicazione per computer.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="6e1e3-221">ASP.NET è la parte di .NET Framework che è stata appositamente progettata per la creazione di applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="6e1e3-222">Gli sviluppatori hanno usato ASP.NET per creare molti dei siti Web più grandi e più elevato traffico nel mondo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="6e1e3-223">(Qualsiasi ora noterete che l'estensione del nome file *aspx* come parte dell'URL in un sito, si saprà che il sito sia stato creato utilizzando ASP.NET.)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="6e1e3-224">La sintassi Razor ti offre tutta la potenza di ASP.NET, ma usando una sintassi semplificata che è più facile apprendere se sei un principiante e che rende più produttiva se sei un esperto.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="6e1e3-225">Anche se questa sintassi è semplice da usare, la relazione della famiglia con ASP.NET e .NET Framework significa che, man mano che diventano più sofisticati i siti Web, è necessario la potenza dei framework più grande disponibile all'utente.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="6e1e3-227">**Classi e istanze**</span><span class="sxs-lookup"><span data-stu-id="6e1e3-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="6e1e3-228">Codice server ASP.NET utilizza oggetti, che a sua volta sono basati sul concetto di classi.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="6e1e3-229">La classe è la definizione o il modello per un oggetto.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="6e1e3-230">Ad esempio, un'applicazione potrebbe contenere un `Customer` classe che definisce le proprietà e metodi necessari per qualsiasi oggetto cliente.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="6e1e3-231">Quando l'applicazione deve utilizzare informazioni reali dei clienti, crea un'istanza di (o *crea un'istanza*) un oggetto customer.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="6e1e3-232">Ogni singolo cliente è un'istanza separata del `Customer` classe.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="6e1e3-233">Ogni istanza supporta le stesse proprietà e metodi, ma i valori delle proprietà per ogni istanza sono in genere diversi, perché ogni oggetto customer è univoco.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="6e1e3-234">Nell'oggetto di un cliente, il `LastName` proprietà potrebbe essere "Smith"; in un altro oggetto customer, la `LastName` proprietà potrebbe essere "Jones".</span><span class="sxs-lookup"><span data-stu-id="6e1e3-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="6e1e3-235">In modo analogo, qualsiasi singola pagina web nel sito è un `Page` oggetto che rappresenta un'istanza di `Page` classe.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="6e1e3-236">Un pulsante nella pagina è una `Button` oggetto che rappresenta un'istanza del `Button` classe e così via.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="6e1e3-237">Ogni istanza ha caratteristiche, ma tutti si basano su quanto specificato nella definizione di classe dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>


## <a name="basic-syntax"></a><span data-ttu-id="6e1e3-238">Sintassi di base</span><span class="sxs-lookup"><span data-stu-id="6e1e3-238">Basic Syntax</span></span>

<span data-ttu-id="6e1e3-239">In precedenza è stato illustrato un esempio su come creare una pagina ASP.NET Web Pages e come è possibile aggiungere il codice lato server per il markup HTML.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="6e1e3-240">Qui si apprenderanno i concetti fondamentali di scrittura di codice server ASP.NET tramite la sintassi Razor &#8212; , ovvero le regole del linguaggio di programmazione.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="6e1e3-241">Se è esperti di programmazione (in particolare se è stato usato C, C++, c#, Visual Basic o JavaScript), gran parte delle quali è leggere qui risulteranno familiare.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="6e1e3-242">Si sarà probabilmente necessario acquisire familiarità con solo come codice lato server viene aggiunta al markup nel *cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="6e1e3-243">Combinazione di testo, Markup e codice nei blocchi di codice</span><span class="sxs-lookup"><span data-stu-id="6e1e3-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="6e1e3-244">Nei blocchi di codice server, spesso necessario output testo o markup (o entrambi) alla pagina.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="6e1e3-245">Se un blocco di codice server contiene testo che non è codice e che invece deve essere eseguito il rendering è, ASP.NET deve essere in grado di distinguere tale testo dal codice.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="6e1e3-246">Esistono diversi modi per eseguire tale operazione.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-246">There are several ways to do this.</span></span>

- <span data-ttu-id="6e1e3-247">Racchiudere il testo in un elemento HTML, ad esempio `<p></p>` o `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="6e1e3-248">L'elemento HTML può includere testo, altri elementi HTML ed espressioni di codice lato server.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="6e1e3-249">Se ASP.NET rileva il tag HTML di apertura (ad esempio, `<p>`), viene eseguito il rendering di tutti gli elementi, compreso l'elemento e il contenuto come si trova nel browser, la risoluzione di espressioni di codice lato server durante la sua esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="6e1e3-250">Usare la `@:` operatore o `<text>` elemento.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="6e1e3-251">Il `@:` restituisce una singola riga di contenuto che contengono testo normale o tag HTML senza corrispondenza; il `<text>` elemento racchiude più righe di output.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="6e1e3-252">Queste opzioni sono utili quando non si vuole eseguire il rendering di un elemento HTML come parte dell'output.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="6e1e3-253">Se si desidera restituire più righe di testo o i tag HTML non corrispondenti, è possibile far precedere a ogni riga con `@:`, oppure è possibile includere la riga in un `<text>` elemento.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="6e1e3-254">Ad esempio la `@:` operatore`<text>` i tag vengono usati da ASP.NET per identificare il contenuto di testo e mai eseguito il rendering nell'output della pagina.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="6e1e3-255">Nel primo esempio viene ripetuta nell'esempio precedente ma usa una singola coppia di `<text>` tag per racchiudere il testo per il rendering.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="6e1e3-256">Nel secondo esempio, il `<text>` e `</text>` tag racchiudono tre righe, ognuna delle quali dispone di testo non contenuto e i tag HTML non corrispondenti (`<br />`), insieme a codice lato server e i tag HTML corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="6e1e3-257">Anche in questo caso, è possibile anche far precedere a ogni riga singolarmente con il `@:` operatore; entrambi funzionano modo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e1e3-258">Quando il testo è l'output come mostrato in questa sezione &#8212; utilizzando un elemento HTML, il `@:` operatore o il `<text>` elemento &#8212; ASP.NET non di codifica HTML di output.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="6e1e3-259">(Come indicato in precedenza, ASP.NET codificare l'output di espressioni di codice server e i blocchi di codice server preceduti da `@`, ad eccezione dei casi speciali elencati in questa sezione.)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="6e1e3-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="6e1e3-260">Whitespace</span></span>

<span data-ttu-id="6e1e3-261">Gli spazi aggiuntivi in un'istruzione (e all'esterno di un valore letterale stringa) non influiscono sull'istruzione:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="6e1e3-262">Un'interruzione di riga in un'istruzione non ha alcun effetto sull'istruzione e si possono eseguire il wrapping di istruzioni per migliorare la leggibilità.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="6e1e3-263">Le istruzioni seguenti sono gli stessi:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="6e1e3-264">È, tuttavia, non è possibile eseguire il wrapping di una riga all'interno di un valore letterale stringa.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="6e1e3-265">Nell'esempio seguente non funziona:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="6e1e3-266">Per combinare una stringa lunga che esegue il wrapping su più righe, ad esempio il codice riportato sopra, sono disponibili due opzioni.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="6e1e3-267">È possibile usare l'operatore di concatenazione (`+`), si vedrà più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="6e1e3-268">È anche possibile usare il `@` carattere per creare una stringa verbatim letterale, come illustrato in precedenza in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="6e1e3-269">È possibile interrompere i valori letterali stringa verbatim su più righe:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="6e1e3-270">Codice (e il Markup) commenti</span><span class="sxs-lookup"><span data-stu-id="6e1e3-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="6e1e3-271">Commenti è possibile lasciare note per se stessi o ad altri utenti.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="6e1e3-272">Consentono inoltre di disabilitare (*commento*) una sezione di codice o markup non intendessero gestire ma si desidera mantenere nella pagina per il momento.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="6e1e3-273">È diversa impostazione come commento la sintassi per il codice Razor e per il markup HTML.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="6e1e3-274">Come con tutto il codice Razor, i commenti Razor vengono elaborati (e quindi rimosse) nel server prima che la pagina viene inviata al browser.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="6e1e3-275">Pertanto, la sintassi di commento Razor consente di inserire commenti nel codice (o anche nel markup) che è possibile visualizzare quando si modifica il file, ma che gli utenti non vedono, anche in dell'origine della pagina.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="6e1e3-276">Per i commenti Razor di ASP.NET, iniziare il commento con `@*` e terminare con `*@`.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="6e1e3-277">Il commento non può essere su una o più righe:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="6e1e3-278">Ecco un commento all'interno di un blocco di codice:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="6e1e3-279">Qui è lo stesso blocco di codice, con la riga di codice commentato in modo che non verrà eseguito:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="6e1e3-280">All'interno di un blocco di codice, come alternativa all'uso di sintassi di commento Razor, è possibile usare la sintassi del linguaggio di programmazione che si utilizza, ad esempio c# commenti:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="6e1e3-281">In c#, i commenti a riga singola preceduti dal `//` caratteri e commenti su più righe iniziano con `/*` e terminare con `*/`.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="6e1e3-282">(Come per i commenti Razor, c# commenti non viene eseguiti nel browser.)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="6e1e3-283">Per il markup, come è probabilmente noto, è possibile creare un commento HTML:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="6e1e3-284">HTML i commenti iniziano con `<!--` caratteri e terminano con `-->`.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="6e1e3-285">È possibile utilizzare i commenti HTML per racchiudere il testo non solo, ma anche eventuali markup HTML che può essere utile nella pagina, ma non si vuole eseguire il rendering.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="6e1e3-286">Questo commento HTML verrà nascondere l'intero contenuto del tag e il testo che contengono:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="6e1e3-287">A differenza dei commenti Razor, commenti HTML *sono* rendering sulla pagina e l'utente possa vederli visualizzando il sorgente della pagina.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="6e1e3-288">Razor presenta alcune limitazioni sui blocchi annidati di c#.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="6e1e3-289">Per altre informazioni vedere [variabili c# denominato e annidati blocchi genera interrotto codice](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="6e1e3-290">Variabili</span><span class="sxs-lookup"><span data-stu-id="6e1e3-290">Variables</span></span>

<span data-ttu-id="6e1e3-291">Una variabile è un oggetto denominato che viene usato per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="6e1e3-292">È possibile assegnare variabili di qualsiasi oggetto, ma il nome deve iniziare con un carattere alfabetico e non può contenere spazi vuoti o caratteri riservati.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="6e1e3-293">Le variabili e tipi di dati</span><span class="sxs-lookup"><span data-stu-id="6e1e3-293">Variables and Data Types</span></span>

<span data-ttu-id="6e1e3-294">Una variabile può avere un tipo di dati specifico, che indica il tipo di dati viene archiviato nella variabile.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="6e1e3-295">È possibile avere le variabili di stringa che archiviano i valori stringa (ad esempio &quot;Hello world&quot;), le variabili integer che archiviano valori di numeri interi (ad esempio, 3 o 79) e le variabili di date che archiviano i valori delle date in un'ampia gamma di formati (ad esempio, marzo 2009 o 4/12/2012 ).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="6e1e3-296">Ed esistono molti altri tipi di dati che è possibile usare.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="6e1e3-297">Tuttavia, in genere non è necessario specificare un tipo per una variabile.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="6e1e3-298">La maggior parte dei casi, ASP.NET può determinare il tipo di base sul modo in cui vengono usati i dati nella variabile.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="6e1e3-299">(In alcuni casi è necessario specificare un tipo; sono riportati esempi in cui questo è vero).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="6e1e3-300">Si dichiara una variabile utilizzando la `var` (parola chiave) (se non si vuole specificare un tipo) o utilizzando il nome del tipo:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="6e1e3-301">Nell'esempio seguente mostra alcuni usi tipici delle variabili in una pagina web:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="6e1e3-302">Se si combinano gli esempi precedenti in una pagina, vedere questi dati vengono visualizzati in un browser:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="6e1e3-304">La conversione e i tipi di dati di test</span><span class="sxs-lookup"><span data-stu-id="6e1e3-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="6e1e3-305">Anche se ASP.NET in genere possibile determinare automaticamente un tipo di dati, a volte questo non è possibile.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="6e1e3-306">Pertanto, è necessario supporto ASP.NET mediante l'esecuzione di una conversione esplicita.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="6e1e3-307">Anche se non è necessario convertire i tipi, a volte è utile eseguire un test per vedere quali tipi di dati si lavora con.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="6e1e3-308">Il caso più comune è che è necessario convertire una stringa in un altro tipo, ad esempio in un numero intero o Data.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="6e1e3-309">Nell'esempio seguente viene illustrato un caso tipico in cui è necessario convertire una stringa in un numero.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="6e1e3-310">Di norma, input dell'utente vengono recapitati come stringhe.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="6e1e3-311">Anche se è stato richiesto agli utenti di immettere un numero e anche se è stato immesso una cifra, quando viene inviato l'input dell'utente e di leggerlo nel codice, i dati sono in formato stringa.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="6e1e3-312">Pertanto, è necessario convertire la stringa in un numero.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="6e1e3-313">Nell'esempio, se si prova a eseguire operazioni aritmetiche sui valori senza doverle convertire in, l'errore seguente generato, perché ASP.NET non è possibile aggiungere due stringhe:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="6e1e3-314">*Impossibile convertire implicitamente il tipo 'string' a 'int'.*</span><span class="sxs-lookup"><span data-stu-id="6e1e3-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="6e1e3-315">Per convertire i valori interi, si chiama il `AsInt` (metodo).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="6e1e3-316">Se la conversione ha esito positivo, è quindi possibile aggiungere i numeri.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="6e1e3-317">La tabella seguente elenca alcuni metodi di conversione e di test comuni per le variabili.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="6e1e3-318"><strong>Metodo</strong></span><span class="sxs-lookup"><span data-stu-id="6e1e3-318"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-319"><strong>Descrizione</strong></span><span class="sxs-lookup"><span data-stu-id="6e1e3-319"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-320"><strong>Esempio</strong></span><span class="sxs-lookup"><span data-stu-id="6e1e3-320"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-321">Converte una stringa che rappresenta un numero intero (ad esempio, "593") in un numero intero.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-322">Converte una stringa like &quot;true&quot; oppure &quot;false&quot; a un tipo Boolean.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-323">Converte una stringa che contiene un valore decimale, ad esempio &quot;1.3&quot; oppure &quot;7.439&quot; su un numero a virgola mobile.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-324">Converte una stringa che contiene un valore decimale, ad esempio &quot;1.3&quot; oppure &quot;7.439&quot; in un numero decimale.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="6e1e3-325">(In ASP.NET, un numero decimale è più preciso rispetto a un numero a virgola mobile).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-326">Converte una stringa che rappresenta un valore di data e ora in ASP.NET `DateTime` tipo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-327">Converte qualsiasi altro tipo di dati in una stringa.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-327">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="6e1e3-328">Operatori</span><span class="sxs-lookup"><span data-stu-id="6e1e3-328">Operators</span></span>

<span data-ttu-id="6e1e3-329">Un operatore è una parola chiave o un carattere che indica ad ASP.NET che tipo di comando da eseguire in un'espressione.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="6e1e3-330">Il linguaggio c# (e la sintassi Razor che si basa su di esso) supporta molti operatori, ma è sufficiente riconoscere alcune per iniziare.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="6e1e3-331">Nella tabella seguente sono riepilogati gli operatori più comuni.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-331">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
    <span data-ttu-id="6e1e3-332"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="6e1e3-332"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-333"><strong>Descrizione</strong></span><span class="sxs-lookup"><span data-stu-id="6e1e3-333"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-334"><strong>Esempi</strong></span><span class="sxs-lookup"><span data-stu-id="6e1e3-334"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-335">Operatori matematici di utilizzati nelle espressioni numeriche.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-335">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-336">Assegnazione.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-336">Assignment.</span></span> <span data-ttu-id="6e1e3-337">Assegna il valore sul lato destro di un'istruzione per l'oggetto sul lato sinistro.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-337">Assigns the value on the right side of a statement to the object on the left side.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-338">Uguaglianza.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-338">Equality.</span></span> <span data-ttu-id="6e1e3-339">Restituisce `true` se i valori sono uguali.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-339">Returns `true` if the values are equal.</span></span> <span data-ttu-id="6e1e3-340">(Si noti che la distinzione tra i `=` operatore e il `==` operator.)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-340">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-341">Disuguaglianza.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-341">Inequality.</span></span> <span data-ttu-id="6e1e3-342">Restituisce `true` se i valori non sono uguali.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-342">Returns `true` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-343">Meno-di, maggiore-di, minore di-than-or-equal e maggiore-than-or-equal.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-343">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-344">Concatenazione, che consente di unire le stringhe.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-344">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="6e1e3-345">ASP.NET riconosce la differenza tra questo operatore e operatore dell'addizione in base al tipo di dati dell'espressione.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-345">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-346">Gli operatori di incremento e decremento, quali addizioni e sottrazioni 1 (rispettivamente) da una variabile.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-346">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-347">Punto.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-347">Dot.</span></span> <span data-ttu-id="6e1e3-348">Utilizzato per distinguere gli oggetti e le relative proprietà e metodi.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-348">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-349">Parentesi.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-349">Parentheses.</span></span> <span data-ttu-id="6e1e3-350">Utilizzato per le espressioni di raggruppamento e passare i parametri a metodi.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-350">Used to group expressions and to pass parameters to methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-351">Le parentesi quadre.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-351">Brackets.</span></span> <span data-ttu-id="6e1e3-352">Utilizzato per l'accesso ai valori nelle matrici o raccolte.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-352">Used for accessing values in arrays or collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-353">No.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-353">Not.</span></span> <span data-ttu-id="6e1e3-354">Inverte una `true` valore `false` e viceversa.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-354">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="6e1e3-355">In genere utilizzato come un modo abbreviato per testare `false` (vale a dire, per non `true`).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-355">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&&` <code>&#124;&#124;</code>
    :::column-end:::
    :::column:::
    <span data-ttu-id="6e1e3-356">AND logico e o, che vengono utilizzati per collegare le condizioni.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-356">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="6e1e3-357">Utilizzo di File e i percorsi delle cartelle nel codice</span><span class="sxs-lookup"><span data-stu-id="6e1e3-357">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="6e1e3-358">Sarà spesso lavorano con i percorsi di file e cartelle nel codice.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-358">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="6e1e3-359">Ecco un esempio di struttura di cartelle fisico per un sito Web come potrebbe apparire nel computer di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-359">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="6e1e3-360">Ecco alcuni dettagli essenziali sui percorsi e gli URL:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-360">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="6e1e3-361">Un URL inizia con un nome di dominio (`http://www.example.com`) o un nome di server (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-361">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="6e1e3-362">Un URL corrisponde a un percorso fisico in un computer host.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-362">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="6e1e3-363">Ad esempio, `http://myserver` possono corrispondere alla cartella *C:\websites\mywebsite* nel server.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-363">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="6e1e3-364">Un percorso virtuale è a sintassi abbreviata per rappresentare i percorsi nel codice senza la necessità di specificare il percorso completo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-364">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="6e1e3-365">Include la parte di un URL che segue il nome di dominio o server.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-365">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="6e1e3-366">Quando si usano i percorsi virtuali, è possibile spostare il codice a un dominio diverso o un server senza la necessità di aggiornare i percorsi.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-366">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="6e1e3-367">Ecco un esempio che consentono di comprendere le differenze:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-367">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="6e1e3-368">URL completo</span><span class="sxs-lookup"><span data-stu-id="6e1e3-368">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="6e1e3-369">Nome del server</span><span class="sxs-lookup"><span data-stu-id="6e1e3-369">Server name</span></span> | <span data-ttu-id="6e1e3-370">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="6e1e3-370">*mycompanyserver*</span></span> |
| <span data-ttu-id="6e1e3-371">Percorso virtuale</span><span class="sxs-lookup"><span data-stu-id="6e1e3-371">Virtual path</span></span> | <span data-ttu-id="6e1e3-372">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="6e1e3-372">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="6e1e3-373">Percorso fisico</span><span class="sxs-lookup"><span data-stu-id="6e1e3-373">Physical path</span></span> | <span data-ttu-id="6e1e3-374">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="6e1e3-374">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="6e1e3-375">È la radice virtuale /, esattamente come la radice dell'unità c: è unità \.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-375">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="6e1e3-376">(I percorsi delle cartelle virtuali sempre usano le barre). Il percorso virtuale di una cartella non deve necessariamente avere lo stesso nome della cartella fisica; può trattarsi di un alias.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-376">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="6e1e3-377">(Nei server di produzione, il percorso virtuale raramente corrisponde a un percorso fisico esatto)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-377">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="6e1e3-378">Quando si lavora con i file e cartelle nel codice, in alcuni casi è necessario fare riferimento al percorso fisico e in alcuni casi un percorso virtuale, a seconda di quali oggetti si lavora con.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-378">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="6e1e3-379">ASP.NET fornisce questi strumenti per l'uso di percorsi di file e cartelle nel codice: il `Server.MapPath` metodo e il `~` operatore e `Href` (metodo).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-379">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="6e1e3-380">La conversione dei percorsi virtuali a fisici: il metodo server. MapPath</span><span class="sxs-lookup"><span data-stu-id="6e1e3-380">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="6e1e3-381">Il `Server.MapPath` metodo converte un percorso virtuale (ad esempio */default.cshtml*) in un percorso fisico assoluto (ad esempio *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-381">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="6e1e3-382">Utilizzare questo metodo ogni volta che è necessario un percorso fisico completo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-382">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="6e1e3-383">Un esempio tipico è quando si esegue la lettura o la scrittura di un file di testo o file di immagine nel server web.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-383">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="6e1e3-384">In genere non conosce il percorso fisico assoluto del sito nel server del sito di hosting, in modo che questo metodo consente di convertire il percorso si conosce, ovvero il percorso virtuale, ovvero per il percorso corrispondente nel server per l'utente.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-384">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="6e1e3-385">Si passa il percorso virtuale in un file o cartella in cui il metodo e restituisce il percorso fisico:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-385">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="6e1e3-386">La radice virtuale di riferimento: il ~ operatore e il metodo di Href</span><span class="sxs-lookup"><span data-stu-id="6e1e3-386">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="6e1e3-387">In un *. cshtml* oppure *vbhtml* file, è possibile fare riferimento nel percorso radice virtuale usando il `~` operatore.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-387">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="6e1e3-388">Ciò è molto utile perché pagine è possibile spostarsi in un sito e tutti i collegamenti che ad altre pagine contengono non saranno interrotti.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-388">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="6e1e3-389">È anche utile nel caso in cui si sposta mai il sito Web in un percorso diverso.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-389">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="6e1e3-390">Ecco alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-390">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="6e1e3-391">Se il sito Web è `http://myserver/myapp`, ecco come ASP.NET tratterà questi percorsi quando viene eseguita la pagina:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-391">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="6e1e3-392">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="6e1e3-392">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="6e1e3-393">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="6e1e3-393">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="6e1e3-394">(Non viene effettivamente visualizzato questi percorsi come i valori della variabile, ma ASP.NET considererà i percorsi come se questo è ciò che fossero).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-394">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="6e1e3-395">È possibile usare il `~` operatore nel codice server (come sopra) sia nel markup, simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-395">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="6e1e3-396">Nel markup, si utilizza il `~` operatore per creare percorsi di risorse, ad esempio i file di immagine, altre pagine web e file CSS.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-396">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="6e1e3-397">Quando viene eseguita la pagina, ASP.NET cerca tramite la pagina (codice e markup) e risolve tutti i `~` riferimenti al percorso appropriato.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-397">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="6e1e3-398">I cicli e la logica condizionale</span><span class="sxs-lookup"><span data-stu-id="6e1e3-398">Conditional Logic and Loops</span></span>

<span data-ttu-id="6e1e3-399">Codice server ASP.NET consente di eseguire attività in base alle condizioni e scrivere codice che si ripete istruzioni un numero specifico di volte in cui (vale a dire, codice che esegue un ciclo).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-399">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="6e1e3-400">Le condizioni di test</span><span class="sxs-lookup"><span data-stu-id="6e1e3-400">Testing Conditions</span></span>

<span data-ttu-id="6e1e3-401">Per testare una condizione semplice è usare il `if` istruzione che restituisce true o false in base a un test è specificare:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-401">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="6e1e3-402">Il `if` (parola chiave) viene avviato un blocco.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-402">The `if` keyword starts a block.</span></span> <span data-ttu-id="6e1e3-403">Il test effettivo (condizione) è racchiuso tra parentesi e restituisce true o false.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-403">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="6e1e3-404">Le istruzioni eseguite se il test è true sono racchiusi tra parentesi graffe.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-404">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="6e1e3-405">Un' `if` istruzione può includere un `else` blocco che consente di specificare istruzioni da eseguire se la condizione è false:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-405">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="6e1e3-406">È possibile aggiungere più condizioni usando un `else if` blocco:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-406">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="6e1e3-407">In questo esempio, se la prima condizione nella se non è impostato su true, blocca il `else if` condizione viene confrontata.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-407">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="6e1e3-408">Se tale condizione viene soddisfatta, le istruzioni di `else if` blocco vengono eseguite.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-408">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="6e1e3-409">Se nessuna delle condizioni viene soddisfatta, le istruzioni di `else` blocco vengono eseguite.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-409">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="6e1e3-410">È possibile aggiungere un numero qualsiasi di se invece si blocca e quindi chiudere con un `else` bloccare come il &quot;tutto il resto&quot; condizione.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-410">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="6e1e3-411">Per testare un numero elevato di condizioni, usare un `switch` blocco:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-411">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="6e1e3-412">Valore da testare è racchiuso tra parentesi (nell'esempio di `weekday` variabile).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-412">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="6e1e3-413">Ogni singolo test viene utilizzato un `case` istruzione che termina con due punti (:).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-413">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="6e1e3-414">Se il valore di un `case` istruzione corrisponde al valore di test, viene eseguito il codice in tale blocco case.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-414">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="6e1e3-415">Si chiude ogni istruzione case con un `break` istruzione.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-415">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="6e1e3-416">(Se si dimentica di includere l'interruzione in ognuno `case` blocca, il codice dal successivo `case` istruzione eseguirà inoltre.) Oggetto `switch` blocco ha spesso un `default` istruzione come ultimo caso per un &quot;tutto il resto&quot; opzione che viene eseguito se viene soddisfatta nessuna di altri casi.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-416">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="6e1e3-417">Il risultato di ultimi due blocchi condizionali visualizzati in un browser:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-417">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="6e1e3-419">Codice di ciclo</span><span class="sxs-lookup"><span data-stu-id="6e1e3-419">Looping Code</span></span>

<span data-ttu-id="6e1e3-420">È spesso necessario eseguire ripetutamente le stesse istruzioni.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-420">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="6e1e3-421">Eseguire questa operazione dal ciclo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-421">You do this by looping.</span></span> <span data-ttu-id="6e1e3-422">Ad esempio, si esegue spesso le stesse istruzioni per ogni elemento in una raccolta di dati.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-422">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="6e1e3-423">Se si conosce esattamente quante volte si desidera eseguire un ciclo, è possibile usare un `for` ciclo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-423">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="6e1e3-424">Questo tipo di ciclo è particolarmente utile per il conteggio dei o il conteggio verso il basso:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-424">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="6e1e3-425">Il ciclo inizia con la `for` (parola chiave), seguita da tre istruzioni racchiuso tra parentesi, ognuna terminato con un punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-425">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="6e1e3-426">All'interno delle parentesi, la prima istruzione (`var i=10;`) viene creato un contatore e lo inizializza per 10.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-426">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="6e1e3-427">Non è necessario assegnare un nome del contatore `i` &#8212; è possibile utilizzare qualsiasi variabile.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-427">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="6e1e3-428">Quando il `for` ciclo viene eseguito, il contatore viene incrementato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-428">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="6e1e3-429">La seconda istruzione (`i < 21;`) imposta la condizione per fino a quando si desidera contare.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-429">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="6e1e3-430">In questo caso, si desidera che consente di passare a un massimo di 20 (vale a dire, continua a usare anche se il contatore è minore di 21).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-430">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="6e1e3-431">La terza istruzione (`i++` ) usa un operatore di incremento, che specifica semplicemente che il contatore dovrebbe avere 1 aggiunto ad esso ogni volta che viene eseguito il ciclo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-431">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="6e1e3-432">All'interno delle parentesi graffe è il codice che verrà eseguito in ogni iterazione del ciclo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-432">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="6e1e3-433">Il codice crea un nuovo paragrafo (`<p>` elemento) ogni volta e si aggiunge una riga all'output, la visualizzazione del valore di `i` (il contatore).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-433">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="6e1e3-434">Quando si esegue questa pagina, l'esempio crea 11 righe visualizzando l'output, con il testo in ogni riga che indica il numero dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-434">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="6e1e3-436">Se si lavora con una raccolta o una matrice, usano spesso un `foreach` ciclo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-436">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="6e1e3-437">Una raccolta è un gruppo di oggetti simili e il `foreach` ciclo consente eseguire un'attività su ogni elemento nella raccolta.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-437">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="6e1e3-438">Questo tipo di ciclo è utile per le raccolte, perché a differenza di un `for` ciclo, non è necessario incrementare il contatore o impostare un limite.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-438">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="6e1e3-439">Al contrario, il `foreach` codice del ciclo procede semplicemente tramite la raccolta venga completata.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-439">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="6e1e3-440">Ad esempio, il codice seguente restituisce gli elementi di `Request.ServerVariables` raccolta, che è un oggetto che contiene informazioni relative al server web.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-440">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="6e1e3-441">Usa un' `foreac` ciclo h per visualizzare il nome di ogni elemento creando un nuovo `<li>` elemento in un elenco puntato di HTML.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-441">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="6e1e3-442">Il `foreach` parola chiave è seguita dalle parentesi in cui si dichiara una variabile che rappresenta un singolo elemento della raccolta (nell'esempio `var item`), seguito dal `in` (parola chiave), la raccolta che si desidera scorrere in ciclo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-442">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="6e1e3-443">Nel corpo del `foreach` ciclo, è possibile accedere all'elemento corrente usando la variabile dichiarata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-443">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="6e1e3-445">Per creare un ciclo più generico, usare il `while` istruzione:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-445">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="6e1e3-446">Oggetto `while` ciclo inizia con il `while` (parola chiave), seguita dalle parentesi, in cui si specifica quanto tempo il ciclo continua (qui, come, purché `countNum` è minore di 50), quindi il blocco da ripetere.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-446">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="6e1e3-447">In genere incrementare i cicli (aggiungere) o di decremento (sottrarre) una variabile o un oggetto utilizzato per il conteggio.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-447">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="6e1e3-448">Nell'esempio, il `+=` operatore aggiunge 1 a `countNum` ogni volta che viene eseguito il ciclo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-448">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="6e1e3-449">(Per decrementano una variabile in un ciclo che conta verso il basso, si utilizzerebbe l'operatore di decremento `-=`).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-449">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="6e1e3-450">Oggetti e raccolte</span><span class="sxs-lookup"><span data-stu-id="6e1e3-450">Objects and Collections</span></span>

<span data-ttu-id="6e1e3-451">Quasi tutti gli elementi in un sito Web ASP.NET è un oggetto, tra cui la pagina web stessa.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-451">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="6e1e3-452">Questa sezione illustra alcuni oggetti importante che si utilizzerà spesso nel codice.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-452">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="6e1e3-453">Oggetti della pagina</span><span class="sxs-lookup"><span data-stu-id="6e1e3-453">Page Objects</span></span>

<span data-ttu-id="6e1e3-454">L'oggetto di base in ASP.NET è la pagina.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-454">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="6e1e3-455">Si può accedere alle proprietà dell'oggetto pagina direttamente, senza alcun oggetto qualificato.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-455">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="6e1e3-456">Il codice seguente ottiene il percorso di file della pagina, tramite il `Request` oggetto della pagina:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-456">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="6e1e3-457">Per rendere chiaro che si sta che fanno riferimento a proprietà e metodi sull'oggetto pagina corrente, è anche possibile usare la parola chiave `this` per rappresentare l'oggetto pagina nel codice.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-457">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="6e1e3-458">Ecco un esempio di codice precedente, con `this` aggiunto per rappresentare la pagina:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-458">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="6e1e3-459">È possibile usare le proprietà del `Page` oggetto da ottenere molte informazioni, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-459">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="6e1e3-460">`Request`.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-460">`Request`.</span></span> <span data-ttu-id="6e1e3-461">Come abbiamo già visto, questa è una raccolta di informazioni sulla richiesta corrente, tra cui il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-461">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="6e1e3-462">`Response`.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-462">`Response`.</span></span> <span data-ttu-id="6e1e3-463">Si tratta di una raccolta di informazioni sulla risposta che verrà inviata al browser al termine dell'esecuzione il codice del server (pagina).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-463">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="6e1e3-464">Ad esempio, è possibile usare questa proprietà per scrivere informazioni nella risposta.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-464">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="6e1e3-465">Oggetti della raccolta (matrici e dizionari)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-465">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="6e1e3-466">Oggetto *collection* è un gruppo di oggetti dello stesso tipo, ad esempio una raccolta di `Customer` oggetti da un database.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-466">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="6e1e3-467">ASP.NET contiene molte raccolte predefinite, ad esempio il `Request.Files` raccolta.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-467">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="6e1e3-468">Sarà spesso lavorano con i dati nelle raccolte.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-468">You'll often work with data in collections.</span></span> <span data-ttu-id="6e1e3-469">Due tipi di raccolte comuni sono le *matrice* e il *dizionario*.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-469">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="6e1e3-470">Una matrice è utile quando si vuole archiviare un insieme di elementi simili, ma non si vuole creare una variabile separata per contenere ciascun elemento:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-470">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="6e1e3-471">Con le matrici, ad esempio si dichiara un tipo di dati specifico `string`, `int`, o `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-471">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="6e1e3-472">Per indicare che la variabile può contenere una matrice, è aggiungere le parentesi alla dichiarazione (ad esempio `string[]` o `int[]`).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-472">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="6e1e3-473">È possibile accedere agli elementi in una matrice usando la loro posizione (indice) o tramite il `foreach` istruzione.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-473">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="6e1e3-474">Gli indici di matrice sono in base zero in &#8212; , ovvero il primo elemento è nella posizione 0, il secondo elemento alla posizione 1 e così via.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-474">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="6e1e3-475">È possibile determinare il numero di elementi in una matrice tramite il recupero relativo `Length` proprietà.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-475">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="6e1e3-476">Per ottenere la posizione di un elemento specifico nella matrice (la ricerca nella matrice), usare il `Array.IndexOf` (metodo).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-476">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="6e1e3-477">È anche possibile eseguire operazioni come reverse il contenuto di una matrice (il `Array.Reverse` metodo) oppure ordinare il contenuto (il `Array.Sort` (metodo)).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-477">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="6e1e3-478">L'output del codice di matrice di stringa visualizzato in un browser:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-478">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="6e1e3-480">Un dizionario è una raccolta di coppie chiave/valore, in cui è fornire la chiave (o nome) per impostare o recuperare il valore corrispondente:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-480">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="6e1e3-481">Per creare un dizionario, usare il `new` (parola chiave) per indicare che si sta creando un nuovo oggetto dizionario.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-481">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="6e1e3-482">È possibile assegnare un dizionario a una variabile utilizzando la `var` (parola chiave).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-482">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="6e1e3-483">Si indicano i tipi di dati degli elementi nel dizionario tramite parentesi angolari ( `< >` ).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-483">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="6e1e3-484">Alla fine della dichiarazione, è necessario aggiungere una coppia di parentesi, perché si tratta in realtà un metodo che crea un nuovo dizionario.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-484">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="6e1e3-485">Per aggiungere elementi al dizionario, è possibile chiamare il `Add` metodo per la variabile del dizionario (`myScores` in questo caso), quindi specificare una chiave e un valore.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-485">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="6e1e3-486">In alternativa, è possibile utilizzare le parentesi quadre indicano la chiave ed eseguire una semplice assegnazione, come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-486">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="6e1e3-487">Per ottenere un valore dal dizionario, si specifica la chiave tra parentesi quadre:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-487">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="6e1e3-488">Chiamata di metodi con parametri</span><span class="sxs-lookup"><span data-stu-id="6e1e3-488">Calling Methods with Parameters</span></span>

<span data-ttu-id="6e1e3-489">Come leggere più indietro in questo articolo, gli oggetti che si programma con possono presentare metodi.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-489">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="6e1e3-490">Ad esempio, un `Database` oggetto potrebbe avere un `Database.Connect` (metodo).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-490">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="6e1e3-491">Molti metodi dispongono anche di uno o più parametri.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-491">Many methods also have one or more parameters.</span></span> <span data-ttu-id="6e1e3-492">Oggetto *parametro* è un valore che si passa a un metodo per abilitare il metodo completare l'attività.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-492">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="6e1e3-493">Ad esempio, esaminata una dichiarazione per il `Request.MapPath` metodo che accetta tre parametri:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-493">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="6e1e3-494">(La riga è stato eseguito il wrapping per renderlo più leggibile.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-494">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="6e1e3-495">Tenere presente che è possibile inserire interruzioni di riga quasi in qualsiasi luogo ad eccezione del fatto all'interno di stringhe che sono racchiusi tra virgolette doppie.)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-495">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="6e1e3-496">Questo metodo restituisce il percorso fisico sul server che corrisponde a un percorso virtuale specificato.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-496">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="6e1e3-497">I tre parametri per il metodo vengono `virtualPath`, `baseVirtualDir`, e `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-497">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="6e1e3-498">Si noti che nella dichiarazione, i parametri sono elencati con i tipi di dati dei dati che verranno accettano. Quando si chiama questo metodo, è necessario fornire valori per tutti i tre parametri.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-498">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="6e1e3-499">La sintassi Razor offre due opzioni per il passaggio di parametri a un metodo: *parametri posizionali* e *parametri denominati*.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-499">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="6e1e3-500">Per chiamare un metodo usando i parametri posizionali, passare i parametri in un ordine fisso specificato nella dichiarazione del metodo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-500">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="6e1e3-501">(È in genere saprebbe quest'ordine leggendo la documentazione relativa al metodo.) È necessario seguire l'ordine e non è possibile ignorare i parametri &#8212; se necessario, si passa una stringa vuota (`""`) o `null` per un parametro posizionale che non si dispone di un valore per.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-501">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="6e1e3-502">Nell'esempio seguente si presuppone una cartella denominata *script* nel tuo sito Web.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-502">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="6e1e3-503">Il codice chiama il `Request.MapPath` metodo e passa i valori per i tre parametri nell'ordine corretto.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-503">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="6e1e3-504">Viene quindi visualizzato il percorso risulta mappato.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-504">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="6e1e3-505">Quando un metodo ha molti parametri, è possibile mantenere il codice più leggibile utilizzando parametri denominati.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-505">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="6e1e3-506">Per chiamare un metodo utilizzando parametri denominati, si specifica il nome del parametro seguito da due punti (:) e quindi il valore.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-506">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="6e1e3-507">Il vantaggio di parametri denominati è che è possibile passarli in qualsiasi ordine desiderato.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-507">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="6e1e3-508">(Uno svantaggio è che la chiamata al metodo non è compresso).</span><span class="sxs-lookup"><span data-stu-id="6e1e3-508">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="6e1e3-509">Nell'esempio seguente chiama il metodo di stesso come illustrato in precedenza, ma utilizza parametri di fornire i valori denominati:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-509">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="6e1e3-510">Come può notare, i parametri vengono passati in un ordine diverso.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-510">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="6e1e3-511">Tuttavia, se si esegue l'esempio precedente e in questo esempio, verrà restituiscono lo stesso valore.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-511">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="6e1e3-512">Gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="6e1e3-512">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="6e1e3-513">Istruzioni Try-Catch</span><span class="sxs-lookup"><span data-stu-id="6e1e3-513">Try-Catch Statements</span></span>

<span data-ttu-id="6e1e3-514">È necessario spesso istruzioni nel codice che potrebbe non riuscire per motivi di all'esterno del controllo.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-514">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="6e1e3-515">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-515">For example:</span></span>

- <span data-ttu-id="6e1e3-516">Se il codice tenta di creare o accedere a un file, tutti i tipi di errori potrebbero verificarsi.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-516">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="6e1e3-517">Il file desiderato potrebbe non esistere, potrebbe essere stato bloccato, il codice potrebbe non disporre delle autorizzazioni e così via.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-517">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="6e1e3-518">Analogamente, se il codice tenta di aggiornare i record in un database, possono essere presenti problemi relativi alle autorizzazioni, la connessione al database potrebbe essere eliminata, i dati da salvare potrebbero essere non valido e così via.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-518">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="6e1e3-519">In termini di programmazione, queste situazioni sono denominate *eccezioni*.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-519">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="6e1e3-520">Se il codice rileva un'eccezione, viene generato (genera) un messaggio di errore di, nella migliore delle ipotesi, indesiderate agli utenti:</span><span class="sxs-lookup"><span data-stu-id="6e1e3-520">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="6e1e3-522">In situazioni in cui il codice che si verifichino eccezioni e per evitare messaggi di errore di questo tipo, è possibile usare `try/catch` istruzioni.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-522">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="6e1e3-523">Nel `try` istruzione, eseguire il codice che si sta archiviando.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-523">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="6e1e3-524">In uno o più `catch` (istruzioni), è possibile cercare specifici errori (tipi specifici di eccezioni) che sono stati generati.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-524">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="6e1e3-525">È possibile includere un numero `catch` istruzioni è necessario individuare errori che si prevede di usare.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-525">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="6e1e3-526">È consigliabile evitare di utilizzare il `Response.Redirect` metodo `try/catch` (istruzioni), poiché potrebbe causare un'eccezione nella pagina.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-526">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="6e1e3-527">Nell'esempio seguente mostra una pagina che consente di creare un file di testo per la prima richiesta e quindi visualizza un pulsante che consente all'utente di aprire il file.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-527">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="6e1e3-528">L'esempio Usa un nome file errato deliberatamente in modo che lo genererà un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-528">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="6e1e3-529">Il codice riporta `catch` istruzioni per due possibili eccezioni: `FileNotFoundException`, che si verifica se il nome del file non è corretto, e `DirectoryNotFoundException`, che si verifica se ASP.NET non è possibile anche trovare la cartella.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-529">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="6e1e3-530">(È possibile rimuovere il commento nell'esempio, un'istruzione per verificarne l'esecuzione quando tutto funziona correttamente.)</span><span class="sxs-lookup"><span data-stu-id="6e1e3-530">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="6e1e3-531">Se il codice non gestisce l'eccezione, si vedrà una pagina di errore, ad esempio lo screenshot precedente.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-531">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="6e1e3-532">Tuttavia, il `try/catch` sezione aiuta a impedire all'utente di visualizzare questi tipi di errori.</span><span class="sxs-lookup"><span data-stu-id="6e1e3-532">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="6e1e3-533">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6e1e3-533">Additional Resources</span></span>

<span data-ttu-id="6e1e3-534">**Programmazione con Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="6e1e3-534">**Programming with Visual Basic**</span></span>


[<span data-ttu-id="6e1e3-535">Appendice: linguaggio Visual Basic e sintassi</span><span class="sxs-lookup"><span data-stu-id="6e1e3-535">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)


<span data-ttu-id="6e1e3-536">**Documentazione di riferimento**</span><span class="sxs-lookup"><span data-stu-id="6e1e3-536">**Reference Documentation**</span></span>


[<span data-ttu-id="6e1e3-537">ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="6e1e3-537">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="6e1e3-538">Linguaggio c#</span><span class="sxs-lookup"><span data-stu-id="6e1e3-538">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
