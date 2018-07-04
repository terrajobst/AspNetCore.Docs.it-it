---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Convalida dell'Input utente in ASP.NET Web Pages (Razor) siti | Microsoft Docs
author: tfitzmac
description: Questo articolo viene illustrato come convalidare le informazioni ricevute dagli utenti &mdash; , ovvero per assicurarsi che gli utenti immettano valido informazioni in formato HTML basata su form in un sistema autonomo....
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 2a35e9895c5b711d5c6c5544987f99fe7e2e0085
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368227"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Convalida dell'Input utente in ASP.NET Web Pages (Razor) Sites
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo viene illustrato come convalidare le informazioni ricevute dagli utenti &mdash; , ovvero per assicurarsi che gli utenti immettano valido informazioni in formato HTML basata su form in un sito di ASP.NET Web Pages (Razor).
> 
> Che cosa si apprenderà come:
> 
> - Come verificare che un utente di input corrisponde ai criteri di convalida definite.
> - Come determinare se sono stati superati tutti i test di convalida.
> - Come visualizzare gli errori di convalida (e come formattarle).
> - Come convalidare i dati che non provengono direttamente dagli utenti.
> 
> Questi sono i concetti introdotti nell'articolo di programmazione ASP.NET:
> 
> - Il `Validation` helper.
> - Il `Html.ValidationSummary` e `Html.ValidationMessage` metodi.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


In questo articolo sono contenute le sezioni seguenti:

- [Panoramica della convalida dell'Input utente](#Overview_of_User_Input_Validation)
- [Convalida dell'Input utente](#Validating_User_Input)
- [Aggiunta della convalida lato Client](#Adding_Client-Side_Validation)
- [Formattazione degli errori di convalida](#Formatting_Validation_Errors)
- [La convalida dei dati che non provengono direttamente da utenti](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Panoramica della convalida dell'Input utente

Se si chiede all'utente di immettere le informazioni in una pagina, ad esempio, in un form, è importante assicurarsi che i valori immessi siano validi. Ad esempio, non si desidera elaborare un form in cui mancanza informazioni critiche.

Quando gli utenti immettono valori in un form HTML, i valori immessi sono stringhe. In molti casi, i valori che necessari sono alcuni altri tipi di dati, ad esempio date o numeri interi. Pertanto, è anche necessario assicurarsi che i valori immessi dall'utente possono essere convertiti in modo corretto per i tipi di dati appropriato.

Potrebbe anche essere determinate restrizioni in base ai valori. Anche se gli utenti immettono correttamente un numero intero, ad esempio, è necessario assicurarsi che il valore è compreso in un determinato intervallo.

![Errori di convalida che utilizzano le classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Importante** convalida dell'input utente è importante anche per la sicurezza. Quando si limitano i valori che possono essere immessi nel form, si riduce la probabilità che un utente può immettere un valore che può compromettere la sicurezza del sito.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Convalida dell'Input utente

In ASP.NET Web Pages 2, è possibile usare il `Validator` helper per testare l'input dell'utente. L'approccio di base consiste nell'eseguire le operazioni seguenti:

1. Determinare quali elementi (campi) che si desidera convalidare di input.

    In genere convalidare i valori in `<input>` elementi in un form. Tuttavia, è consigliabile convalidare tutti gli input, anche input proveniente da un elemento vincolato, ad esempio un `<select>` elenco. Ciò consente di assicurarsi che gli utenti non ignorare i controlli in una pagina e inviare un modulo.
2. Nel codice della pagina, aggiungere i controlli di convalida singoli per ogni elemento di input usando i metodi del `Validation` helper.

    Per verificare i campi obbligatori, usare `Validation.RequireField(field, [error message])` (per un singolo campo) o `Validation.RequireFields(field1, field2, ...))` (per un elenco di campi). Per altri tipi di convalida, usare `Validation.Add(field, ValidationType)`. Per `ValidationType`, è possibile usare queste opzioni:

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Quando viene inviata la pagina, verificare se ha superato la convalida controllando `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Se sono presenti errori di convalida, si ignora l'elaborazione delle pagine normale. Ad esempio, se lo scopo della pagina si aggiorna un database, è non farlo fino a quando non sono stati risolti tutti gli errori di convalida.
4. Se sono presenti errori di convalida, visualizzare i messaggi di errore nel markup della pagina mediante `Html.ValidationSummary` o `Html.ValidationMessage`, o entrambi.

Nell'esempio seguente mostra una pagina che illustra questi passaggi.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Per informazioni sul funzionamento della convalida, eseguire questa pagina e deliberatamente commettono errori. Ad esempio, ecco ciò che la pagina è simile se si dimentica di immettere il nome del corso, se si immette un, e se si immette una data non valida:

![Errori di convalida nella pagina sottoposta a rendering](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Aggiunta della convalida lato Client

Per impostazione predefinita, viene convalidato input utente dopo l'invio della pagina, vale a dire, la convalida viene eseguita nel codice server. Uno svantaggio di questo approccio è che gli utenti non sappiano che sono state apportate un errore fino a dopo la pagina Invia. Se un modulo è lunga o complessa, la segnalazione degli errori solo dopo l'invio della pagina può risultare poco pratica all'utente.

È possibile aggiungere il supporto per eseguire la convalida in script client. In tal caso, la convalida viene eseguita con gli utenti del lavoro nel browser. Ad esempio, si supponga di che specificare che un valore deve essere un numero intero. Se un utente immette un valore diverso da integer, viene segnalato l'errore, non appena l'utente lascia il campo di immissione. Gli utenti ricevono un riscontro immediato, che è facilmente. Convalida basata su client può anche ridurre il numero di volte in cui l'utente deve inviare il modulo per correggere gli errori di più.

> [!NOTE]
> Anche se si usa la convalida lato client, la convalida viene eseguita sempre anche nel codice server. Esegue la convalida nel codice del server è una misura di sicurezza, nel caso in cui gli utenti di ignorare la convalida basata su client.


1. Registra le librerie JavaScript seguente nella pagina:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Due delle librerie sono può essere caricato da una rete di distribuzione di contenuti (CDN), quindi non necessariamente è necessario in modo che vengano nel computer o server. Tuttavia, è necessario disporre una copia locale del *jquery.validate.unobtrusive.js*. Se sta non ancora lavorando con un modello di WebMatrix (ad esempio **Starter Site** ) che include la libreria, creare un sito Web Pages base **Starter Site**. Copiare quindi il *js* file al sito corrente.
2. Nel markup, per ogni elemento che sta effettuando la convalida, aggiungere una chiamata a `Validation.For(field)`. Questo metodo genera gli attributi utilizzati per la convalida lato client. (Piuttosto che genera il codice JavaScript effettivo, il metodo genera attributi quali `data-val-...`. Questi attributi supportano la convalida del client non intrusivi che usa jQuery per svolgere il lavoro).

La pagina seguente viene illustrato come aggiungere funzionalità di convalida client all'esempio illustrato in precedenza.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Non tutti i controlli di convalida eseguiti sul client. In particolare, la convalida del tipo di dati (integer, data e così via) non vengono eseguiti nel client. I controlli seguenti funzionano sia il client di server:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

In questo esempio, il test per una data valida non funzionerà nel codice client. Tuttavia, verrà eseguito il test nel codice server.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Formattazione degli errori di convalida

È possibile controllare la modalità di visualizzazione degli errori di convalida la definizione delle classi CSS che hanno nomi riservati seguenti:

- `field-validation-error`. Definisce l'output del `Html.ValidationMessage` metodo quando visualizza un errore.
- `field-validation-valid`. Definisce l'output del `Html.ValidationMessage` metodo quando si verifica alcun errore.
- `input-validation-error`. Definisce come `<input>` rendering degli elementi quando si verifica un errore. (Ad esempio, è possibile utilizzare questa classe per impostare il colore di sfondo di un' &lt;input&gt; elemento da un colore diverso se il relativo valore non è valido.) Questa classe CSS viene usata solo durante la convalida del client (in ASP.NET Web Pages 2).
- `input-validation-valid`. Definisce l'aspetto del `<input>` elementi quando non si è verificato alcun errore.
- `validation-summary-errors`. Definisce l'output del `Html.ValidationSummary` metodo viene visualizzato un elenco di errori.
- `validation-summary-valid`. Definisce l'output del `Html.ValidationSummary` metodo quando si verifica alcun errore.

Nell'esempio `<style>` vengono mostrate le regole per condizioni di errore.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Se si include questo blocco di stile nelle pagine di esempio da sopra nell'articolo, la visualizzazione dell'errore sarà simile al seguente:

![Errori di convalida che utilizzano le classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Se non si usa la convalida del client in ASP.NET Web Pages 2, le classi CSS per il `<input>` elementi (`input-validation-error` e `input-validation-valid` non hanno alcun effetto.


### <a name="static-and-dynamic-error-display"></a>Visualizzazione degli errori statici e dinamici

Le regole CSS sono disponibili in coppie, ad esempio `validation-summary-errors` e `validation-summary-valid`. Queste coppie consentono di definire le regole per entrambe le condizioni: una condizione di errore e una condizione (non degli errori) "normale". È importante comprendere che il markup per la visualizzazione degli errori viene sempre eseguito, anche se non sono presenti errori. Ad esempio, se una pagina presenta un `Html.ValidationSummary` metodo nel markup, l'origine della pagina conterrà il markup seguente anche quando viene richiesta la pagina per la prima volta:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

In altre parole, il `Html.ValidationSummary` metodo esegue il rendering sempre un `<div>` elemento e un elenco, anche se l'elenco di errori è vuota. Allo stesso modo, il `Html.ValidationMessage` metodo esegue il rendering sempre un `<span>` elemento come segnaposto per un errore di singoli campi, anche se è presente alcun errore.

In alcune situazioni, la visualizzazione di un messaggio di errore possono utilizzare la pagina per ridisporre e può causare gli elementi nella pagina per spostarsi all'interno. Le regole CSS che terminano in `-valid` consentono di definire un layout che consentono di evitare questo problema. Ad esempio, è possibile definire `field-validation-error` e `field-validation-valid` a entrambi hanno lo stesso a dimensione fissa. In questo modo, l'area di visualizzazione per il campo è statico e non verrà modificato il flusso della pagina, se viene visualizzato un messaggio di errore.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>La convalida dei dati che non provengono direttamente da utenti

A volte è necessario convalidare le informazioni che non provengono direttamente da un form HTML. Un esempio tipico è una pagina in cui viene passato un valore in una stringa di query, come nell'esempio seguente:

`http://server/myapp/EditClassInformation?classid=1022`

In questo caso, si desidera assicurarsi che il valore passato alla pagina (a questo punto, per il valore di 1022 `classid`) è valido. È possibile utilizzare direttamente la `Validation` helper per eseguire la convalida. Tuttavia, è possibile usare altre funzionalità del sistema di convalida, come la possibilità di visualizzare i messaggi di errore di convalida.

> [!NOTE] 
> 
> **Importanti** convalidare sempre i valori che si ottiene mediante *qualsiasi* origine, inclusi i valori del campo del form, i valori di stringa di query e i valori dei cookie. È facile agli utenti di modificare questi valori (ad esempio per scopi dannosi). Pertanto, è necessario verificare questi valori per proteggere l'applicazione.


Nell'esempio seguente viene illustrato come è possibile convalidare un valore che viene passato una stringa di query. Il codice verifica che il valore non vuoto e che sia un numero intero.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Si noti che il test viene eseguito quando la richiesta non è l'invio di un form (`if(!IsPost)`). La prima volta che viene richiesta la pagina passa questo test, ma non quando la richiesta è l'invio di un form.

Per visualizzare questo errore, è possibile aggiungere l'errore all'elenco di errori di convalida tramite una chiamata `Validation.AddFormError("message")`. Se la pagina contiene una chiamata al `Html.ValidationSummary` metodo, l'errore viene visualizzato, proprio come un errore di convalida dell'input dell'utente.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Utilizzo di moduli HTML in siti con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202892)
