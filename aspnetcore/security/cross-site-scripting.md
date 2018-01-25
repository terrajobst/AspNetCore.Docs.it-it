---
title: Impedendo Cross-Site Scripting
author: rick-anderson
description: "Questo documento introduce Cross-Site Scripting (XSS) e le tecniche per risolvere questa vulnerabilità in un'applicazione ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: 3aaab9d4fecd3f0d0da6a0df4d83bee090b329ea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="preventing-cross-site-scripting"></a>Impedendo Cross-Site Scripting

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Cross-Site Scripting (XSS) è una vulnerabilità di sicurezza che consente a un utente malintenzionato di inserire gli script lato client (in genere JavaScript) nelle pagine web. Quando altri utenti di caricare pagine interessate verranno eseguiti gli script di utenti malintenzionati, consentendo all'utente malintenzionato di rubare i cookie e token di sessione, modificare il contenuto della pagina web mediante modifica DOM o reindirizzare il browser a un'altra pagina. Vulnerabilità XSS si verificano in genere quando si accetta l'input dell'utente e lo restituisce in una pagina senza convalida, la codifica o caratteri di escape un'applicazione.

## <a name="protecting-your-application-against-xss"></a>Proteggere l'applicazione rispetto a XSS

In una base XSS livello funziona da indurre l'applicazione in cui inserire un `<script>` tag nella pagina sottoposta a rendering o inserendo un `On*` evento in un elemento. Gli sviluppatori devono utilizzare i seguenti passaggi di prevenzione per evitare di introdurre XSS nelle proprie applicazioni.

1. Non inserire dati non attendibili l'input HTML, se non si seguono il resto dei passaggi riportati di seguito. Dati non attendibili sono tutti i dati che possono essere controllati da un utente malintenzionato, gli input di form HTML, le stringhe di query, le intestazioni HTTP, anche i dati originati da un database come un utente malintenzionato potrebbe essere in grado di violare il database, anche se essi non è possibile violare l'applicazione.

2. Prima di inserire i dati non attendibili all'interno di un elemento HTML verificare che sia codificato in formato HTML. La codifica HTML accetta caratteri, ad esempio &lt; e li converte in un formato sicuro come &amp;lt;

3. Prima di inserire i dati non attendibili in un attributo HTML verificare che sia l'attributo HTML con codificata. La codifica dell'attributo HTML è un superset di codifica HTML e consente di codificare i caratteri aggiuntivi, ad esempio "e".

4. Prima di inserire i dati non attendibili in JavaScript, inserire i dati in un elemento HTML il cui contenuto è recuperare in fase di esecuzione. Se questo non è possibile garantire che i dati viene codificato JavaScript. Codifica JavaScript accetta caratteri pericolosi per JavaScript e li sostituisce con i relativi esadecimale, ad esempio &lt; potrebbe essere codificato come `\u003C`.

5. Prima di inserire i dati non attendibili in una stringa di query URL verificare è codificato in URL.

## <a name="html-encoding-using-razor"></a>Codifica HTML utilizzando Razor

Il motore Razor utilizzato automaticamente in MVC consente di codificare tutti output originati da variabili, a meno che non si lavora effettivamente duramente per evitare che in questo modo. Viene utilizzato l'attributo HTML regole di codifica quando si usa il  *@*  direttiva. HTML codifica dell'attributo è un superset di codifica HTML che ciò significa che non è necessario preoccuparsi se è necessario utilizzare la codifica HTML o la codifica dell'attributo HTML. È necessario assicurarsi di utilizzare solo in un contesto HTML, non quando si tenta di inserire direttamente input non attendibile JavaScript. Gli helper di tag verranno inoltre codificare l'input che utilizzare nei parametri di tag.

Eseguire la seguente vista Razor;

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Questa vista restituisce il contenuto del *untrustedInput* variabile. Questa variabile include alcuni caratteri utilizzati negli attacchi XSS, vale a dire &lt;, "e &gt;. Esaminare l'origine viene codificato come output sottoposto a rendering:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> Componenti di base ASP.NET MVC fornisce un `HtmlString` classe che non è codificato automaticamente al momento di output. Non si devono mai utilizzato in combinazione con l'input non attendibile come questo esporrà una vulnerabilità XSS.

## <a name="javascript-encoding-using-razor"></a>JavaScript codifica utilizzando Razor

È possibile che si desidera inserire un valore JavaScript per l'elaborazione della visualizzazione. È possibile ottenere questo risultato in due modi. Il modo più sicuro per inserire valori semplici è di inserire il valore in un attributo di dati di un tag e recuperarli in JavaScript. Ad esempio:

```none
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

Questa sintassi produrrà il seguente codice HTML

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

Che, quando viene eseguito, verrà eseguito il rendering la procedura seguente.

```none
<"123">
   <"123">
   ```

È inoltre possibile chiamare direttamente, il codificatore di JavaScript

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

Questo verrà eseguito il rendering nel browser come segue:

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> Non concatenare l'input non attendibile in JavaScript per creare gli elementi DOM. È consigliabile utilizzare `createElement()` e assegnare i valori delle proprietà in modo appropriato, ad esempio `node.TextContent=`, oppure utilizzare `element.SetAttribute()` / `element[attribute]=` in caso contrario si espone al XSS basato su DOM.

## <a name="accessing-encoders-in-code"></a>L'accesso a codificatori nel codice

I codificatori HTML, JavaScript e URL sono disponibili al codice in due modi diversi, è possibile inserire tramite [inserimento di dipendenze](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) o è possibile usare i codificatori predefinito contenuti nel `System.Text.Encodings.Web` dello spazio dei nomi. Se si utilizzano i codificatori predefinito qualsiasi applicati a intervalli di caratteri per essere considerato sicuro non avranno effetto: i codificatori predefinito utilizzano le regole di codifica più sicure possibile.

Per usare i codificatori configurabili tramite devono richiedere i costruttori DI una *HtmlEncoder*, *JavaScriptEncoder* e *UrlEncoder* parametro come appropriato. Ad esempio;

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

## <a name="encoding-url-parameters"></a>Codifica i parametri URL

Se si desidera compilare una stringa di query URL con input non attendibile come utilizzare un valore di `UrlEncoder` per codificare il valore. Ad esempio,

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Dopo la codifica di encodedValue variabile conterrà `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Gli spazi, virgolette, segni di punteggiatura e altri caratteri non sicuri sarà percentuale con codifica in base al valore esadecimale, ad esempio un carattere di spazio diventerà % 20.

>[!WARNING]
> Non utilizzare l'input non attendibile come parte di un percorso URL. Passare sempre l'input non attendibile come valore di stringa di query.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>I codificatori di personalizzazione

Per impostazione predefinita i codificatori usano un elenco provvisoria limitato all'intervallo Unicode latino di base e codificare tutti i caratteri di fuori di tale intervallo, come gli equivalenti di codice di carattere. Questo comportamento influisce anche sulle helper di tag Razor HtmlHelper il rendering come i codificatori verrà utilizzato per le stringhe di output.

La giustificazione è evitare i bug browser sconosciuto o futuro (bug browser precedenti hanno attivato backup l'analisi basata sull'elaborazione dei caratteri non inglesi). Se il sito web abbia un uso massiccio di caratteri non latini, ad esempio il cinese, alfabeto cirillico o altri utenti non si tratta probabilmente il comportamento desiderato.

È possibile personalizzare gli elenchi di sicuro codificatore per includere gli intervalli appropriati per l'applicazione durante l'avvio, in Unicode `ConfigureServices()`.

Ad esempio, utilizzando la configurazione predefinita, è possibile utilizzare un HtmlHelper Razor come illustrato di seguito;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Quando si visualizza l'origine della pagina web verrà visualizzato che è stato eseguito il rendering come indicato di seguito, con il testo cinese codificato.

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Per ampliare i caratteri considerati attendibili dal codificatore si inserisce la riga seguente nel `ConfigureServices()` metodo `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

In questo esempio viene convertito nell'elenco per includere il CjkUnifiedIdeographs intervallo Unicode. Output sottoposto a rendering diventerebbe ora

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Come grafici di codice Unicode, le lingue non vengono specificati gli intervalli di attendibili. Il [standard Unicode](http://unicode.org/) include un elenco di [codice grafici](http://www.unicode.org/charts/index.html) è possibile utilizzare per trovare il grafico contenente i caratteri. Ogni codificatore, Html, JavaScript e Url, deve essere configurato separatamente.

> [!NOTE]
> Personalizzazione dell'elenco influisce solo sui codificatori originati tramite DI. Se si accede direttamente a un codificatore tramite `System.Text.Encodings.Web.*Encoder.Default` quindi il valore predefinito, latino di base da utilizzare solo dell'elenco indirizzi attendibili.

## <a name="where-should-encoding-take-place"></a>In cui devono essere inseriti take codifica?

Generale accettato pratica è che la codifica viene eseguita al momento di output e valori codificati non devono essere mai archiviati in un database. Codifica nel punto di output consente di modificare l'utilizzo di dati, ad esempio, da HTML a un valore di stringa di query. Inoltre, consente di cercare facilmente i dati senza la necessità di codificare i valori prima di eseguire ricerche e consente di sfruttare i vantaggi di eventuali modifiche apportate ai codificatori di correzioni di bug.

## <a name="validation-as-an-xss-prevention-technique"></a>Convalida come tecnica di prevenzione XSS

La convalida può essere uno strumento utile per limitare gli attacchi XSS. Ad esempio, una semplice stringa numerica contenente solo i caratteri 0-9 non verrà generato un attacco XSS. Convalida diventa più complessa se lo si desidera accettare HTML nell'input dell'utente, l'analisi di input HTML è difficile se non impossibile. MarkDown e altri formati di testo sarebbe un'opzione più sicura per l'input RTF. Mai affidamento sulla convalida da solo. Codificare sempre l'input non attendibile prima di output, indipendentemente dal tipo di convalida è stata eseguita.
