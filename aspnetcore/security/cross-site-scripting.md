---
title: Evitare il Cross-Site Scripting (XSS) in ASP.NET Core
author: rick-anderson
description: Informazioni su Cross-Site Scripting (XSS) e le tecniche per risolvere questa vulnerabilità in un'app ASP.NET Core.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 1e9e988be68313cfd493832519c1be89335d6e48
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815214"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="ca39d-103">Evitare il Cross-Site Scripting (XSS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca39d-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="ca39d-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ca39d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ca39d-105">Cross-Site Scripting (XSS) è una vulnerabilità di sicurezza che consente a un utente malintenzionato di inserire gli script sul lato client (in genere JavaScript) in pagine web.</span><span class="sxs-lookup"><span data-stu-id="ca39d-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="ca39d-106">Quando altri utenti caricare pagine interessate verranno eseguiti script non autorizzato, consentendo all'utente malintenzionato di impadronirsi di cookie e token di sessione, modificare il contenuto della pagina web mediante modifica DOM o reindirizzare il browser a un'altra pagina.</span><span class="sxs-lookup"><span data-stu-id="ca39d-106">When other users load affected pages the attacker's scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="ca39d-107">Le vulnerabilità XSS, in genere, vengono eseguiti quando un'applicazione accetta input dell'utente e lo invia a una pagina senza la convalida, la codifica o facendola.</span><span class="sxs-lookup"><span data-stu-id="ca39d-107">XSS vulnerabilities generally occur when an application takes user input and outputs it to a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="ca39d-108">Proteggere l'applicazione da XSS</span><span class="sxs-lookup"><span data-stu-id="ca39d-108">Protecting your application against XSS</span></span>

<span data-ttu-id="ca39d-109">AT un XSS a livello di base funziona inducendo dell'applicazione nell'inserimento di un `<script>` tag nella pagina sottoposta a rendering, o inserendo un `On*` evento in un elemento.</span><span class="sxs-lookup"><span data-stu-id="ca39d-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="ca39d-110">Gli sviluppatori utilizzino la seguente procedura di prevenzione per evitare l'introduzione di XSS nelle proprie applicazioni.</span><span class="sxs-lookup"><span data-stu-id="ca39d-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="ca39d-111">Non inserire mai i dati non attendibili nell'input HTML, a meno che non si seguono il resto dei passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="ca39d-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="ca39d-112">Dati non attendibili sono tutti i dati che possono essere controllati da un utente malintenzionato, gli input dei moduli HTML, le stringhe di query, intestazioni HTTP, persino i dati originati da un database come un utente malintenzionato potrebbe essere in grado di violare il database anche se essi non è una violazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ca39d-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="ca39d-113">Prima di inserire i dati non attendibili all'interno di un elemento HTML verificare che sia codificato in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="ca39d-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="ca39d-114">La codifica HTML accetta caratteri, ad esempio &lt; e li converte in un formato sicuro, ad esempio &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="ca39d-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="ca39d-115">Prima di inserire i dati non attendibili in un attributo HTML verificare che è codificato in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="ca39d-115">Before putting untrusted data into an HTML attribute ensure it's HTML encoded.</span></span> <span data-ttu-id="ca39d-116">Codifica dell'attributo HTML è un superset di codifica HTML e codifica i caratteri aggiuntivi, ad esempio "e".</span><span class="sxs-lookup"><span data-stu-id="ca39d-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="ca39d-117">Prima di inserire i dati non attendibili in JavaScript inserire i dati in un elemento HTML il cui contenuto è recuperare in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ca39d-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="ca39d-118">Se questo non è possibile, quindi verificare che i dati sono con codificata JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ca39d-118">If this isn't possible, then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="ca39d-119">Codifica JavaScript accetta caratteri pericolosi per JavaScript e li sostituisce con loro esadecimale, ad esempio &lt; potrebbe essere codificato come `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="ca39d-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="ca39d-120">Prima di inserire i dati non attendibili in una stringa di query URL assicurarsi che è codificato in URL.</span><span class="sxs-lookup"><span data-stu-id="ca39d-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="ca39d-121">Codifica HTML con Razor</span><span class="sxs-lookup"><span data-stu-id="ca39d-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="ca39d-122">Il motore di Razor usato automaticamente in MVC consente di codificare tutte output originate dalle variabili, a meno che non lavorare sodo per evitare che in questo modo.</span><span class="sxs-lookup"><span data-stu-id="ca39d-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="ca39d-123">Usa le regole di codifica attributi HTML quando si usa la *@* direttiva.</span><span class="sxs-lookup"><span data-stu-id="ca39d-123">It uses HTML attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="ca39d-124">HTML codifica dell'attributo è un superset di codifica HTML ciò significa che non è necessario preoccuparsi se sia necessario utilizzare la codifica HTML o codifica dell'attributo HTML.</span><span class="sxs-lookup"><span data-stu-id="ca39d-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="ca39d-125">È necessario assicurarsi di utilizzare solo in un contesto HTML, non quando si tenta di inserire input non attendibile direttamente in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ca39d-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="ca39d-126">Gli helper tag verranno inoltre codificare l'input che utilizzare nei parametri di tag.</span><span class="sxs-lookup"><span data-stu-id="ca39d-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="ca39d-127">Richiedere la visualizzazione Razor seguente:</span><span class="sxs-lookup"><span data-stu-id="ca39d-127">Take the following Razor view:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="ca39d-128">In questa vista restituisce il contenuto della *untrustedInput* variabile.</span><span class="sxs-lookup"><span data-stu-id="ca39d-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="ca39d-129">Questa variabile include alcuni caratteri che vengono utilizzati per gli attacchi XSS, vale a dire &lt;, "e &gt;.</span><span class="sxs-lookup"><span data-stu-id="ca39d-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="ca39d-130">Esaminare l'origine Mostra l'output sottoposto a rendering codificato come:</span><span class="sxs-lookup"><span data-stu-id="ca39d-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="ca39d-131">ASP.NET Core MVC offre un `HtmlString` classe che non è con codifica automatica al momento di output.</span><span class="sxs-lookup"><span data-stu-id="ca39d-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="ca39d-132">Ciò non deve mai essere usato in combinazione con l'input non attendibile come si esporrà una vulnerabilità XSS.</span><span class="sxs-lookup"><span data-stu-id="ca39d-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="ca39d-133">Codifica di JavaScript con Razor</span><span class="sxs-lookup"><span data-stu-id="ca39d-133">JavaScript Encoding using Razor</span></span>

<span data-ttu-id="ca39d-134">È possibile che si desidera inserire un valore in JavaScript per l'elaborazione nella propria visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ca39d-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="ca39d-135">È possibile ottenere questo risultato in due modi.</span><span class="sxs-lookup"><span data-stu-id="ca39d-135">There are two ways to do this.</span></span> <span data-ttu-id="ca39d-136">Il modo più sicuro per inserire i valori è di inserire il valore in un attributo di dati di un tag e recuperarla in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ca39d-136">The safest way to insert values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="ca39d-137">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ca39d-137">For example:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="ca39d-138">Questo modo verrà generato il codice HTML seguente</span><span class="sxs-lookup"><span data-stu-id="ca39d-138">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="ca39d-139">Che, quando è in esecuzione, verrà visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="ca39d-139">Which, when it runs, will render the following:</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="ca39d-140">È inoltre possibile chiamare direttamente il codificatore di JavaScript:</span><span class="sxs-lookup"><span data-stu-id="ca39d-140">You can also call the JavaScript encoder directly:</span></span>

```cshtml
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="ca39d-141">Questo verrà eseguito il rendering nel browser come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="ca39d-141">This will render in the browser as follows:</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="ca39d-142">Non concatenare l'input non attendibile in JavaScript per creare gli elementi DOM.</span><span class="sxs-lookup"><span data-stu-id="ca39d-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="ca39d-143">È consigliabile usare `createElement()` e assegnare i valori delle proprietà in modo appropriato, ad esempio `node.TextContent=`, oppure utilizzare `element.SetAttribute()` / `element[attribute]=` in caso contrario, si espone al XSS in base al modello DOM.</span><span class="sxs-lookup"><span data-stu-id="ca39d-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="ca39d-144">L'accesso ai codificatori nel codice</span><span class="sxs-lookup"><span data-stu-id="ca39d-144">Accessing encoders in code</span></span>

<span data-ttu-id="ca39d-145">I codificatori HTML, JavaScript e l'URL sono disponibili per il codice in due modi, è possibile inserire tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection) oppure è possibile usare i codificatori predefinito contenuti nel `System.Text.Encodings.Web` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="ca39d-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="ca39d-146">Se si usano i codificatori predefinita qualsiasi è applicata a intervalli di caratteri per essere considerato sicuro non avranno effetto: i codificatori predefinito usano le regole di codifica più sicure possibile.</span><span class="sxs-lookup"><span data-stu-id="ca39d-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="ca39d-147">Usare i codificatori configurabili tramite l'inserimento delle dipendenze di costruttori devono accettare un *HtmlEncoder*, *JavaScriptEncoder* e *UrlEncoder* parametro come appropriato.</span><span class="sxs-lookup"><span data-stu-id="ca39d-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="ca39d-148">Ad esempio;</span><span class="sxs-lookup"><span data-stu-id="ca39d-148">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="ca39d-149">Codifica dei parametri URL</span><span class="sxs-lookup"><span data-stu-id="ca39d-149">Encoding URL Parameters</span></span>

<span data-ttu-id="ca39d-150">Se si desidera compilare una stringa di query URL con input non attendibile come utilizzare un valore di `UrlEncoder` per codificare il valore.</span><span class="sxs-lookup"><span data-stu-id="ca39d-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="ca39d-151">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="ca39d-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="ca39d-152">Dopo la codifica di encodedValue variabile conterrà `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="ca39d-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="ca39d-153">Gli spazi, offerte, segni di punteggiatura e altri caratteri non sicuri sarà percentuale con codifica per il loro valore esadecimale, ad esempio un carattere di spazio diventerà % 20.</span><span class="sxs-lookup"><span data-stu-id="ca39d-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="ca39d-154">Non usare l'input non attendibile come parte di un percorso URL.</span><span class="sxs-lookup"><span data-stu-id="ca39d-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="ca39d-155">Passare sempre l'input non attendibile come valore di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="ca39d-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="ca39d-156">Personalizzazione dei codificatori</span><span class="sxs-lookup"><span data-stu-id="ca39d-156">Customizing the Encoders</span></span>

<span data-ttu-id="ca39d-157">Per impostazione predefinita i codificatori usano un elenco elementi attendibili limitato all'intervallo Unicode latino di base e codificare tutti i caratteri esterni all'intervallo come gli equivalenti di codice carattere.</span><span class="sxs-lookup"><span data-stu-id="ca39d-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="ca39d-158">Questo comportamento influisce anche sul rendering di helper tag Razor e HtmlHelper come userà i codificatori per le stringhe di output.</span><span class="sxs-lookup"><span data-stu-id="ca39d-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="ca39d-159">Infatti sono per la protezione da bug del browser sconosciuto o futuro (bug del browser precedenti hanno perché entrano in gioco l'analisi basata sull'elaborazione dei caratteri non inglesi).</span><span class="sxs-lookup"><span data-stu-id="ca39d-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="ca39d-160">Se il sito web abbia un uso massiccio di caratteri non latini, ad esempio il cinese, alfabeto cirillico o da altri utenti non si tratta probabilmente il comportamento desiderato.</span><span class="sxs-lookup"><span data-stu-id="ca39d-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="ca39d-161">È possibile personalizzare gli elenchi sicuri codificatore per includere gli intervalli appropriati per l'applicazione durante l'avvio, in Unicode `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="ca39d-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="ca39d-162">Ad esempio, tramite la configurazione predefinita è possibile usare un HtmlHelper Razor in questo modo;</span><span class="sxs-lookup"><span data-stu-id="ca39d-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="ca39d-163">Quando si visualizza l'origine della pagina web verrà visualizzato che al rendering come indicato di seguito, con il testo in cinese codificato.</span><span class="sxs-lookup"><span data-stu-id="ca39d-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="ca39d-164">Per ampliare i caratteri considerati sicuri dal codificatore si potrebbe inserire la riga seguente nel `ConfigureServices()` metodo `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="ca39d-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="ca39d-165">In questo esempio può ampliarsi nel tipo nell'elenco per includere il CjkUnifiedIdeographs intervallo Unicode.</span><span class="sxs-lookup"><span data-stu-id="ca39d-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="ca39d-166">Ora diventa l'output sottoposto a rendering</span><span class="sxs-lookup"><span data-stu-id="ca39d-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="ca39d-167">Gli intervalli di elenco elementi attendibili vengono specificati come tabelle di codici Unicode, non i linguaggi.</span><span class="sxs-lookup"><span data-stu-id="ca39d-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="ca39d-168">Il [standard Unicode](https://unicode.org/) contiene un elenco delle [codice grafici](https://www.unicode.org/charts/index.html) è possibile usare per trovare il grafico contenente i caratteri.</span><span class="sxs-lookup"><span data-stu-id="ca39d-168">The [Unicode standard](https://unicode.org/) has a list of [code charts](https://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="ca39d-169">Ogni codificatore, Html, JavaScript e Url, deve essere configurato separatamente.</span><span class="sxs-lookup"><span data-stu-id="ca39d-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="ca39d-170">Personalizzazione dell'elenco elementi attendibili influisce solo sui codificatori originati tramite l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="ca39d-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="ca39d-171">Se si accede direttamente a un codificatore tramite `System.Text.Encodings.Web.*Encoder.Default` quindi l'impostazione predefinita, latino di base verranno utilizzato solo dell'elenco indirizzi attendibili.</span><span class="sxs-lookup"><span data-stu-id="ca39d-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="ca39d-172">Dove dovrebbe avvenire la codifica?</span><span class="sxs-lookup"><span data-stu-id="ca39d-172">Where should encoding take place?</span></span>

<span data-ttu-id="ca39d-173">La pratica generalmente accettata è che la codifica avvenga nel punto di output e che i valori codificati non devono mai essere archiviati in un database.</span><span class="sxs-lookup"><span data-stu-id="ca39d-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="ca39d-174">La codifica nel punto di output consente di modificare l'utilizzo dei dati, ad esempio da HTML a un valore query string.</span><span class="sxs-lookup"><span data-stu-id="ca39d-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="ca39d-175">Consente inoltre di cercare facilmente i dati senza dover codificare i valori prima di eseguire ricerca e di sfruttare i vantaggi di eventuali modifiche o correzioni di errori apportate agli encoder.</span><span class="sxs-lookup"><span data-stu-id="ca39d-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="ca39d-176">Convalida di una tecnica di prevenzione di XSS</span><span class="sxs-lookup"><span data-stu-id="ca39d-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="ca39d-177">La convalida può essere uno strumento utile per limitare gli attacchi XSS.</span><span class="sxs-lookup"><span data-stu-id="ca39d-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="ca39d-178">Ad esempio, una stringa numerica che contiene solo i caratteri 0-9 non attiva un attacco XSS.</span><span class="sxs-lookup"><span data-stu-id="ca39d-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="ca39d-179">Convalida diventa più complessa quando si accettano HTML nell'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="ca39d-179">Validation becomes more complicated when accepting HTML in user input.</span></span> <span data-ttu-id="ca39d-180">L'analisi di input HTML è difficile, se non impossibile.</span><span class="sxs-lookup"><span data-stu-id="ca39d-180">Parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="ca39d-181">Markdown, insieme a un parser che estrae con HTML incorporati, è un'opzione più sicura per accettare gli input avanzati.</span><span class="sxs-lookup"><span data-stu-id="ca39d-181">Markdown, coupled with a parser that strips embedded HTML, is a safer option for accepting rich input.</span></span> <span data-ttu-id="ca39d-182">Non basarsi sulla convalida da solo.</span><span class="sxs-lookup"><span data-stu-id="ca39d-182">Never rely on validation alone.</span></span> <span data-ttu-id="ca39d-183">Codificare sempre l'input non attendibile prima di output, indipendentemente da quali la convalida o la purificazione dei processi è stata eseguita.</span><span class="sxs-lookup"><span data-stu-id="ca39d-183">Always encode untrusted input before output, no matter what validation or sanitization has been performed.</span></span>
