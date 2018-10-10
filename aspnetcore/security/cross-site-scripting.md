---
title: Evitare il Cross-Site Scripting (XSS) in ASP.NET Core
author: rick-anderson
description: Informazioni su Cross-Site Scripting (XSS) e le tecniche per risolvere questa vulnerabilità in un'app ASP.NET Core.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 50f0211a2c64708d9b788dd10ce9064e66014d55
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910525"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>Evitare il Cross-Site Scripting (XSS) in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Cross-Site Scripting (XSS) è una vulnerabilità di sicurezza che consente a un utente malintenzionato di inserire gli script sul lato client (in genere JavaScript) in pagine web. Quando altri utenti caricare pagine interessate verranno eseguiti script non autorizzato, consentendo all'utente malintenzionato di impadronirsi di cookie e token di sessione, modificare il contenuto della pagina web mediante modifica DOM o reindirizzare il browser a un'altra pagina. Le vulnerabilità XSS, in genere, vengono eseguiti quando un'applicazione accetta input dell'utente e lo invia a una pagina senza la convalida, la codifica o facendola.

## <a name="protecting-your-application-against-xss"></a>Proteggere l'applicazione da XSS

AT un XSS a livello di base funziona inducendo dell'applicazione nell'inserimento di un `<script>` tag nella pagina sottoposta a rendering, o inserendo un `On*` evento in un elemento. Gli sviluppatori utilizzino la seguente procedura di prevenzione per evitare l'introduzione di XSS nelle proprie applicazioni.

1. Non inserire mai i dati non attendibili nell'input HTML, a meno che non si seguono il resto dei passaggi seguenti. Dati non attendibili sono tutti i dati che possono essere controllati da un utente malintenzionato, gli input dei moduli HTML, le stringhe di query, intestazioni HTTP, persino i dati originati da un database come un utente malintenzionato potrebbe essere in grado di violare il database anche se essi non è una violazione dell'applicazione.

2. Prima di inserire i dati non attendibili all'interno di un elemento HTML verificare che sia codificato in formato HTML. La codifica HTML accetta caratteri, ad esempio &lt; e li converte in un formato sicuro, ad esempio &amp;lt;

3. Prima di inserire i dati non attendibili in un attributo HTML verificare che è codificato in formato HTML. Codifica dell'attributo HTML è un superset di codifica HTML e codifica i caratteri aggiuntivi, ad esempio "e".

4. Prima di inserire i dati non attendibili in JavaScript inserire i dati in un elemento HTML il cui contenuto è recuperare in fase di esecuzione. Se questo non è possibile, quindi verificare che i dati sono con codificata JavaScript. Codifica JavaScript accetta caratteri pericolosi per JavaScript e li sostituisce con loro esadecimale, ad esempio &lt; potrebbe essere codificato come `\u003C`.

5. Prima di inserire i dati non attendibili in una stringa di query URL assicurarsi che è codificato in URL.

## <a name="html-encoding-using-razor"></a>Codifica HTML con Razor

Il motore di Razor usato automaticamente in MVC consente di codificare tutte output originate dalle variabili, a meno che non lavorare sodo per evitare che in questo modo. Usa le regole di codifica attributi HTML quando si usa la *@* direttiva. HTML codifica dell'attributo è un superset di codifica HTML ciò significa che non è necessario preoccuparsi se sia necessario utilizzare la codifica HTML o codifica dell'attributo HTML. È necessario assicurarsi di utilizzare solo in un contesto HTML, non quando si tenta di inserire input non attendibile direttamente in JavaScript. Gli helper tag verranno inoltre codificare l'input che utilizzare nei parametri di tag.

Richiedere la visualizzazione Razor seguente:

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

In questa vista restituisce il contenuto della *untrustedInput* variabile. Questa variabile include alcuni caratteri che vengono utilizzati per gli attacchi XSS, vale a dire &lt;, "e &gt;. Esaminare l'origine Mostra l'output sottoposto a rendering codificato come:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC offre un `HtmlString` classe che non è con codifica automatica al momento di output. Ciò non deve mai essere usato in combinazione con l'input non attendibile come si esporrà una vulnerabilità XSS.

## <a name="javascript-encoding-using-razor"></a>Codifica di JavaScript con Razor

È possibile che si desidera inserire un valore in JavaScript per l'elaborazione nella propria visualizzazione. È possibile ottenere questo risultato in due modi. Il modo più sicuro per inserire i valori è di inserire il valore in un attributo di dati di un tag e recuperarla in JavaScript. Ad esempio:

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

Questo modo verrà generato il codice HTML seguente

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

Che, quando è in esecuzione, verrà visualizzato quanto segue:

```none
<"123">
   <"123">
   ```

È inoltre possibile chiamare direttamente il codificatore di JavaScript:

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

Questo verrà eseguito il rendering nel browser come indicato di seguito:

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> Non concatenare l'input non attendibile in JavaScript per creare gli elementi DOM. È consigliabile usare `createElement()` e assegnare i valori delle proprietà in modo appropriato, ad esempio `node.TextContent=`, oppure utilizzare `element.SetAttribute()` / `element[attribute]=` in caso contrario, si espone al XSS in base al modello DOM.

## <a name="accessing-encoders-in-code"></a>L'accesso ai codificatori nel codice

I codificatori HTML, JavaScript e l'URL sono disponibili per il codice in due modi, è possibile inserire tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection) oppure è possibile usare i codificatori predefinito contenuti nel `System.Text.Encodings.Web` dello spazio dei nomi. Se si usano i codificatori predefinita qualsiasi è applicata a intervalli di caratteri per essere considerato sicuro non avranno effetto: i codificatori predefinito usano le regole di codifica più sicure possibile.

Usare i codificatori configurabili tramite l'inserimento delle dipendenze di costruttori devono accettare un *HtmlEncoder*, *JavaScriptEncoder* e *UrlEncoder* parametro come appropriato. Ad esempio;

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

## <a name="encoding-url-parameters"></a>Codifica dei parametri URL

Se si desidera compilare una stringa di query URL con input non attendibile come utilizzare un valore di `UrlEncoder` per codificare il valore. Ad esempio,

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Dopo la codifica di encodedValue variabile conterrà `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Gli spazi, offerte, segni di punteggiatura e altri caratteri non sicuri sarà percentuale con codifica per il loro valore esadecimale, ad esempio un carattere di spazio diventerà % 20.

>[!WARNING]
> Non usare l'input non attendibile come parte di un percorso URL. Passare sempre l'input non attendibile come valore di stringa di query.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Personalizzazione dei codificatori

Per impostazione predefinita i codificatori usano un elenco elementi attendibili limitato all'intervallo Unicode latino di base e codificare tutti i caratteri esterni all'intervallo come gli equivalenti di codice carattere. Questo comportamento influisce anche sul rendering di helper tag Razor e HtmlHelper come userà i codificatori per le stringhe di output.

Infatti sono per la protezione da bug del browser sconosciuto o futuro (bug del browser precedenti hanno perché entrano in gioco l'analisi basata sull'elaborazione dei caratteri non inglesi). Se il sito web abbia un uso massiccio di caratteri non latini, ad esempio il cinese, alfabeto cirillico o da altri utenti non si tratta probabilmente il comportamento desiderato.

È possibile personalizzare gli elenchi sicuri codificatore per includere gli intervalli appropriati per l'applicazione durante l'avvio, in Unicode `ConfigureServices()`.

Ad esempio, tramite la configurazione predefinita è possibile usare un HtmlHelper Razor in questo modo;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Quando si visualizza l'origine della pagina web verrà visualizzato che al rendering come indicato di seguito, con il testo in cinese codificato.

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Per ampliare i caratteri considerati sicuri dal codificatore si potrebbe inserire la riga seguente nel `ConfigureServices()` metodo `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

In questo esempio può ampliarsi nel tipo nell'elenco per includere il CjkUnifiedIdeographs intervallo Unicode. Ora diventa l'output sottoposto a rendering

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Gli intervalli di elenco elementi attendibili vengono specificati come tabelle di codici Unicode, non i linguaggi. Il [standard Unicode](http://unicode.org/) contiene un elenco delle [codice grafici](http://www.unicode.org/charts/index.html) è possibile usare per trovare il grafico contenente i caratteri. Ogni codificatore, Html, JavaScript e Url, deve essere configurato separatamente.

> [!NOTE]
> Personalizzazione dell'elenco elementi attendibili influisce solo sui codificatori originati tramite l'inserimento delle dipendenze. Se si accede direttamente a un codificatore tramite `System.Text.Encodings.Web.*Encoder.Default` quindi l'impostazione predefinita, latino di base verranno utilizzato solo dell'elenco indirizzi attendibili.

## <a name="where-should-encoding-take-place"></a>Dove dovrebbe avvenire la codifica?

La pratica generalmente accettata è che la codifica avvenga nel punto di output e che i valori codificati non devono mai essere archiviati in un database. La codifica nel punto di output consente di modificare l'utilizzo dei dati, ad esempio da HTML a un valore query string. Consente inoltre di cercare facilmente i dati senza dover codificare i valori prima di eseguire ricerca e di sfruttare i vantaggi di eventuali modifiche o correzioni di errori apportate agli encoder.

## <a name="validation-as-an-xss-prevention-technique"></a>Convalida di una tecnica di prevenzione di XSS

La convalida può essere uno strumento utile per limitare gli attacchi XSS. Ad esempio, una stringa numerica che contiene solo i caratteri 0-9 non attiva un attacco XSS. Convalida diventa più complessa quando si accettano HTML nell'input dell'utente. L'analisi di input HTML è difficile, se non impossibile. Markdown, insieme a un parser che estrae con HTML incorporati, è un'opzione più sicura per accettare gli input avanzati. Non basarsi sulla convalida da solo. Codificare sempre l'input non attendibile prima di output, indipendentemente da quali la convalida o la purificazione dei processi è stata eseguita.
