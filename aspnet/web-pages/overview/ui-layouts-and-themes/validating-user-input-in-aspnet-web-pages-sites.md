---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Convalida dell'Input utente in ASP.NET Web Pages siti (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo articolo viene descritto come convalidare le informazioni ricevute dagli utenti &mdash; , ovvero per assicurarsi che gli utenti immettano valido informazioni in formato HTML basata su form in un sistema autonomo....
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Convalida dell'Input utente in siti Web di ASP.NET di pagine (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene descritto come convalidare le informazioni ricevute dagli utenti &mdash; , ovvero per assicurarsi che gli utenti immettano valido informazioni in formato HTML basata su form in un sito di pagine Web ASP.NET (Razor).
> 
> Illustra quanto segue:
> 
> - Come controllare l'input che un utente corrisponde ai criteri di convalida che definiscono.
> - Come determinare se sono stati superati tutti i test di convalida.
> - Come visualizzare gli errori di convalida e come formattarle.
> - Come convalidare i dati che non provengano direttamente dagli utenti.
> 
> Programmazione di concetti introdotti nell'articolo ASP.NET sono:
> 
> - Il `Validation` helper.
> - Il `Html.ValidationSummary` e `Html.ValidationMessage` metodi.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


In questo articolo sono contenute le sezioni seguenti:

- [Panoramica di convalida dell'Input utente](#Overview_of_User_Input_Validation)
- [Convalida dell'Input utente](#Validating_User_Input)
- [Aggiunta della convalida lato Client](#Adding_Client-Side_Validation)
- [Gli errori di convalida di formattazione](#Formatting_Validation_Errors)
- [Convalida dei dati che non provengano direttamente dagli utenti](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Panoramica di convalida dell'Input utente

Se chiedere agli utenti di immettere le informazioni in una pagina, ad esempio, in un form, è importante assicurarsi che i valori immessi siano validi. Ad esempio, non si desidera elaborare un modulo privo di informazioni critiche.

Quando gli utenti immettono i valori in un form HTML, i valori immessi sono stringhe. In molti casi, i valori che necessari sono alcuni altri tipi di dati, ad esempio numeri interi o date. Pertanto, è inoltre necessario assicurarsi che i valori che gli utenti immettono possono essere convertiti correttamente ai tipi di dati appropriato.

Può inoltre essere alcune limitazioni sui valori. Anche se gli utenti immettono correttamente un numero intero, ad esempio, si potrebbe essere necessario per assicurarsi che il valore è compreso in un determinato intervallo.

![Errori di convalida che usano le classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Importante** convalida dell'input utente è anche importante per la sicurezza. Quando si limitano i valori che possono essere immesse nel form, si riduce la probabilità che un utente può immettere un valore che può compromettere la sicurezza del sito.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Convalida dell'Input utente

In ASP.NET Web Pages 2, è possibile utilizzare il `Validator` helper per verificare l'input dell'utente. L'approccio di base consiste nell'eseguire le operazioni seguenti:

1. Determinare quali input elementi (campi) che si desidera convalidare.

    In genere convalidare i valori in `<input>` elementi in un form. Tuttavia, è buona norma convalidare tutti gli input, anche l'input proveniente da un elemento vincolato come un `<select>` elenco. Ciò consente di verificare che gli utenti non ignorare i controlli in una pagina e inviare un modulo.
2. Nel codice della pagina, aggiungere i controlli di convalida singoli per ogni elemento di input utilizzando metodi del `Validation` helper.

    Per verificare i campi obbligatori, utilizzare `Validation.RequireField(field, [error message])` (per un singolo campo) o `Validation.RequireFields(field1, field2, ...))` (per un elenco di campi). Per altri tipi di convalida, utilizzare `Validation.Add(field, ValidationType)`. Per `ValidationType`, è possibile utilizzare queste opzioni:

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
3. Quando viene inviata la pagina, controllare se la convalida ha superato controllando `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Se sono presenti errori di convalida, ignorando l'elaborazione della pagina normale. Ad esempio, se lo scopo della pagina consiste nell'aggiornare un database, è non farlo fino a quando non sono stati risolti tutti gli errori di convalida.
4. Se sono presenti errori di convalida, visualizzare i messaggi di errore nel markup della pagina utilizzando `Html.ValidationSummary` o `Html.ValidationMessage`, o entrambi.

L'esempio seguente mostra una pagina in cui vengono illustrati tali passaggi.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Per visualizzare la modalità di convalida, eseguire questa pagina e deliberatamente commettere errori. Ad esempio, di seguito come viene visualizzata la pagina se si dimentica di immettere un nome di linea, se si immette un, e se si immette una data non valida:

![Errori di convalida nella pagina sottoposta a rendering](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Aggiunta della convalida lato Client

Per impostazione predefinita, viene convalidata l'input dell'utente dopo l'invio della pagina, vale a dire, la convalida viene eseguita nel codice server. Uno svantaggio di questo approccio è che gli utenti non sanno che sono state apportate errore fino a dopo invio della pagina. Se un form è lunga o complessa, la segnalazione degli errori solo dopo l'invio della pagina può risultare poco pratica per l'utente.

È possibile aggiungere il supporto per eseguire la convalida negli script client. In tal caso, la convalida viene eseguita con gli utenti del lavoro nel browser. Si supponga, ad esempio, che specificare che un valore deve essere un numero intero. Se un utente immette un valore non intero, viene segnalato l'errore non appena l'utente lascia il campo immissione. Gli utenti ricevono un riscontro immediato, è opportuno. Convalida basata su client può anche ridurre il numero di volte in cui l'utente deve inviare il modulo per correggere gli errori più.

> [!NOTE]
> Anche se si utilizza la convalida lato client, la convalida viene eseguita sempre anche nel codice server. Esegue la convalida nel codice server è una misura di sicurezza, nel caso in cui gli utenti di ignorare la convalida basata su client.


1. Registrare le librerie JavaScript seguente nella pagina:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Due delle librerie sono può essere caricato da una rete di distribuzione di contenuti (CDN), non è necessariamente in modo che nel computer o server. Tuttavia, è necessario disporre di una copia locale di *jquery.validate.unobtrusive.js*. Se sta non ancora lavorando con un modello di WebMatrix (ad esempio **Starter Site** ) che include la libreria, creare un sito di pagine Web che si basa sul **Starter Site**. Quindi copiare la *. js* file al sito corrente.
2. Nel markup di ogni elemento che sta effettuando la convalida, aggiungere una chiamata a `Validation.For(field)`. Questo metodo genera gli attributi utilizzati per la convalida lato client. (Anziché la creazione di codice JavaScript, il metodo genera attributi quali `data-val-...`. Questi attributi supportano la convalida client non intrusiva che utilizza jQuery per eseguire il lavoro.)

La pagina seguente mostra come aggiungere funzionalità di convalida client all'esempio illustrato in precedenza.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Non tutti i controlli di convalida eseguiti sul client. In particolare, la convalida di tipo di dati (integer, data e così via) non vengono eseguiti nel client. I seguenti controlli di lavoro nel client e server:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

In questo esempio, il test per una data valida non funzionerà nel codice client. Tuttavia, verrà eseguito il test nel codice server.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Gli errori di convalida di formattazione

È possibile controllare la modalità di visualizzazione degli errori di convalida la definizione delle classi CSS che hanno nomi riservati seguenti:

- `field-validation-error`. Definisce l'output del `Html.ValidationMessage` metodo quando viene visualizzato un errore.
- `field-validation-valid`. Definisce l'output del `Html.ValidationMessage` metodo quando non si verifica alcun errore.
- `input-validation-error`. Definisce come `<input>` gli elementi vengono visualizzati quando si verifica un errore. (Ad esempio, è possibile utilizzare questa classe per impostare il colore di sfondo di un &lt;input&gt; elemento da un colore diverso se il relativo valore non è valido.) Questa classe CSS viene utilizzata solo durante la convalida del client (in ASP.NET Web Pages 2).
- `input-validation-valid`. Definisce l'aspetto di `<input>` elementi quando non si è verificato alcun errore.
- `validation-summary-errors`. Definisce l'output del `Html.ValidationSummary` metodo visualizzato un elenco di errori.
- `validation-summary-valid`. Definisce l'output del `Html.ValidationSummary` metodo quando non si verifica alcun errore.

Nell'esempio `<style>` vengono mostrate le regole per le condizioni di errore.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Se si include questo stile di blocco nelle pagine di esempio da in questo articolo, la visualizzazione dell'errore sarà simile al seguente:

![Errori di convalida che usano le classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Se non si utilizza la convalida del client in ASP.NET Web Pages 2, le classi CSS per il `<input>` elementi (`input-validation-error` e `input-validation-valid` non ha alcun effetto.


### <a name="static-and-dynamic-error-display"></a>Visualizzazione degli errori statici e dinamici

Le regole CSS entrano nelle coppie, ad esempio `validation-summary-errors` e `validation-summary-valid`. Queste coppie consentono di definire le regole per entrambe le condizioni: una condizione di errore e una condizione (non degli errori) "normale". È importante comprendere che il markup per la visualizzazione degli errori viene sempre eseguito, anche se non sono presenti errori. Ad esempio, se una pagina contiene un `Html.ValidationSummary` metodo nel codice, l'origine della pagina conterrà il markup seguente anche quando la pagina viene richiesta per la prima volta:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

In altre parole, il `Html.ValidationSummary` metodo esegue il rendering sempre un `<div>` elemento e un elenco, anche se l'elenco degli errori è vuoto. Analogamente, il `Html.ValidationMessage` metodo esegue il rendering sempre un `<span>` elemento come segnaposto per un errore di singoli campi, anche se non si verificano errori.

In alcuni casi, la visualizzazione di un messaggio di errore possono utilizzare la pagina per ridisporre e può causare gli elementi nella pagina per spostarsi all'interno. Le regole CSS che terminano con `-valid` consentono di definire un layout che consente di evitare questo problema. Ad esempio, è possibile definire `field-validation-error` e `field-validation-valid` sia hanno la stessa dimensione fissa. In questo modo, l'area di visualizzazione per il campo è statico e non modifica il flusso di pagina, se viene visualizzato un messaggio di errore.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Convalida dei dati che non provengano direttamente dagli utenti

A volte è necessario convalidare le informazioni che non provengano direttamente da un form HTML. Un esempio tipico è una pagina in cui viene passato un valore in una stringa di query, come nell'esempio seguente:

`http://server/myapp/EditClassInformation?classid=1022`

In questo caso, si desidera assicurarsi che il valore passato alla pagina (qui, 1022 per il valore di `classid`) è valido. È possibile utilizzare direttamente la `Validation` helper per eseguire la convalida. Tuttavia, è possibile utilizzare altre funzionalità del sistema di convalida, ad esempio la possibilità di visualizzare i messaggi di errore di convalida.

> [!NOTE] 
> 
> **Importante** convalidare sempre i valori che vengono recuperate dal *qualsiasi* origine, inclusi i valori del campo del form, i valori di stringa di query e valori dei cookie. È facile per gli utenti di modificare questi valori (ad esempio, per finalità dannose). È pertanto necessario verificare questi valori per proteggere l'applicazione.


Nell'esempio seguente viene illustrato come convalidare un valore che viene passato una stringa di query. Il codice verifica che il valore non sia vuoto e che sia un numero intero.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Si noti che il test viene eseguito quando la richiesta non è un invio del modulo (`if(!IsPost)`). La prima volta che viene richiesta la pagina passa questo test, ma non quando la richiesta è di invio di un form.

Per visualizzare questo errore, è possibile aggiungere l'errore all'elenco di errori di convalida chiamando `Validation.AddFormError("message")`. Se la pagina contiene una chiamata al `Html.ValidationSummary` (metodo), l'errore viene visualizzato, come un errore di convalida dell'input dell'utente.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Utilizzo di form HTML nei siti di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202892)
