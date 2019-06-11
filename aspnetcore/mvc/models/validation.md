---
title: Convalida del modello in ASP.NET Core MVC
author: tdykstra
description: Informazioni sulla convalida del modello in ASP.NET Core MVC e in Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
monikerRange: '>= aspnetcore-2.1'
uid: mvc/models/validation
ms.openlocfilehash: 9737e45729b4e5abd9a33824c4d6610ca21681c0
ms.sourcegitcommit: c5339594101d30b189f61761275b7d310e80d18a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/02/2019
ms.locfileid: "66458482"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a>Convalida del modello in ASP.NET Core MVC e in Razor Pages

Questo articolo illustra come convalidare l'input utente in un'app ASP.NET Core MVC o Razor Pages.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).

## <a name="model-state"></a>Stato del modello

Lo stato del modello rappresenta gli errori che provengono da due sottosistemi: associazione di modelli e convalida del modello. Gli errori provenienti dall'[associazione di modelli](model-binding.md) sono in genere errori di conversione dei dati, ad esempio l'immissione di una "x" in un campo in cui è previsto un intero. La convalida del modello è un processo successivo all'associazione di modelli e segnala gli errori in caso di dati non conformi alle regole di business, ad esempio l'immissione del valore 0 in un campo in cui è previsto un valore compreso tra 1 e 5.

Sia l'associazione che la convalida di modelli avviene prima di eseguire un'azione del controller o un metodo gestore di Razor Pages. Per le app Web, è responsabilità dell'app esaminare `ModelState.IsValid` e rispondere nel modo appropriato. Le app Web in genere visualizzare di nuovo la pagina con un messaggio di errore:

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

I controller API Web non devono controllare `ModelState.IsValid` se hanno l'attributo `[ApiController]`. In questo caso, se lo stato del modello non è valido, viene restituita una risposta HTTP 400 automatica contenente i dettagli del problema. Per altre informazioni, vedere [Risposte HTTP 400 automatiche](xref:web-api/index#automatic-http-400-responses).

## <a name="rerun-validation"></a>Rieseguire la convalida

La convalida è automatica, ma potrebbe essere necessario ripeterla manualmente. Ad esempio, è possibile che si calcoli un valore per una proprietà e che si voglia rieseguire la convalida dopo aver impostato la proprietà sul valore calcolato. Per rieseguire la convalida, chiamare il metodo `TryValidateModel` come illustrato di seguito:

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a>Attributi di convalida

Gli attributi di convalida consentono di specificare le regole di convalida per le proprietà del modello. Nell'esempio seguente dell'[app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) viene illustrata una classe di modello annotata con attributi di convalida. `[ClassicMovie]` è un attributo di convalida personalizzato, mentre gli altri sono attributi predefiniti. Non viene visualizzato l'attributo `[ClassicMovie2]`, che rappresenta un modo alternativo di implementare un attributo personalizzato.

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a>Attributi predefiniti

Di seguito sono elencati alcuni degli attributi di convalida predefiniti:

* `[CreditCard]`: convalida che la proprietà abbia un formato di carta di credito.
* `[Compare]`: convalida che due proprietà abbiano corrispondenza in un modello.
* `[EmailAddress]`: convalida che la proprietà abbia un formato di indirizzo di posta elettronica.
* `[Phone]`: convalida che la proprietà abbia un formato di numero di telefono.
* `[Range]`: convalida che il valore della proprietà rientri in un intervallo specificato.
* `[RegularExpression]`: convalida che il valore della proprietà corrisponda a un'espressione regolare specificata.
* `[Required]`: convalida che il campo non sia Null. Vedere l'[attributo [Required]](#required-attribute) per informazioni dettagliate sul comportamento di questo attributo.
* `[StringLength]`: convalida che un valore della proprietà della stringa non superi un limite di lunghezza specificata.
* `[Url]`: convalida che la proprietà abbia un formato di URL.
* `[Remote]`: convalida l'input nel client chiamando un metodo di azione nel server. Vedere l'[attributo [Remote]](#remote-attribute) per informazioni dettagliate sul comportamento di questo attributo.

Nello spazio dei nomi [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) è possibile trovare un elenco completo degli attributi di convalida.

### <a name="error-messages"></a>Messaggi di errore

Gli attributi di convalida consentono di specificare il messaggio di errore da visualizzare in caso di input non valido. Ad esempio:

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

Internamente gli attributi chiamano `String.Format` con un segnaposto per il nome campo e talvolta con segnaposto aggiuntivi. Ad esempio:

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

Quando applicato a una proprietà `Name`, il messaggio di errore creato con il codice precedente sarà "La lunghezza del nome utente deve essere compresa tra 6 e 8".

Per scoprire i parametri passati a `String.Format` per un messaggio di errore dell'attributo specifico, vedere il [codice sorgente DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).

## <a name="required-attribute"></a>Attributo [Required]

Per impostazione predefinita, il sistema di convalida considera i parametri non nullable o le proprietà come se avessero un attributo `[Required]`. I [tipi di valore](/dotnet/csharp/language-reference/keywords/value-types) `decimal` e `int` sono parametri non nullable.

### <a name="required-validation-on-the-server"></a>Convalida dell'attributo [Required] nel server

Nel server un valore obbligatorio viene considerato mancante se la proprietà è Null. Un campo non nullable è sempre valido e il messaggio di errore dell'attributo [Required] non viene mai visualizzato.

Può però succedere che l'associazione di modelli per una proprietà non nullable abbia esito negativo, generando un messaggio di errore, ad esempio `The value '' is invalid`. Per specificare un messaggio di errore personalizzato per la convalida lato server di tipi non nullable, sono disponibili le opzioni seguenti:

* Il campo deve ammettere i valori Null, ad esempio `decimal?` anziché `decimal`. I tipi di valore [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) vengono considerati tipi nullable standard.
* Specificare il messaggio di errore predefinito che l'associazione di modelli deve usare, come illustrato nell'esempio seguente:

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  Per altre informazioni sugli errori dell'associazione di modelli che possono essere impostati come messaggi predefiniti, vedere <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.

### <a name="required-validation-on-the-client"></a>Convalida dell'attributo [Required] nel client

Le stringhe e i tipi non nullable sono gestiti in modo diverso nel client rispetto a come vengono gestiti nel server. Nel client:

* Un valore viene considerato presente solo viene immesso un input per tale valore. La convalida lato client gestisce quindi i tipi non nullable come i tipi nullable.
* Lo spazio vuoto in un campo stringa viene considerato un input valido per il metodo [required](https://jqueryvalidation.org/required-method/) della convalida di jQuery. La convalida lato server considera un campo stringa obbligatorio non valido solo se viene immesso uno spazio vuoto.

Come indicato in precedenza, i tipi non nullable vengono trattati come se avessero un attributo `[Required]`. Quindi viene eseguita la convalida lato client anche se non si applica l'attributo `[Required]`. Se invece non si usa l'attributo, viene visualizzato un messaggio di errore predefinito. Per specificare un messaggio di errore personalizzato, usare l'attributo.

## <a name="remote-attribute"></a>Attributo [Remote]

L'attributo `[Remote]` implementa la convalida lato client che richiede la chiamata a un metodo nel server per determinare se l'input del campo è valido. Ad esempio, l'app potrebbe dover verificare se un nome utente è già in uso.

Per implementare la convalida remota:

1. Creare un metodo di azione per JavaScript da chiamare.  Il metodo [remote](https://jqueryvalidation.org/remote-method/) jQuery Validate prevede una risposta JSON:

   * `"true"` indica che i dati di input sono validi.
   * `"false"`, `undefined` o `null` indica che l'input non è valido.  Visualizzare il messaggio di errore predefinito.
   * Qualsiasi altra stringa indica che l'input non è valido. Visualizzare la stringa come messaggio di errore personalizzato.

   Di seguito è riportato un esempio di un metodo di azione che restituisce un messaggio di errore personalizzato:

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. Nella classe del modello annotare la proprietà con un attributo `[Remote]` che punta al metodo di azione di convalida, come illustrato nell'esempio seguente:

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   L'attributo `[Remote]` si trova nello spazio dei nomi `Microsoft.AspNetCore.Mvc`. Installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) se non si usa il metapacchetto `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`.
   
### <a name="additional-fields"></a>Campi aggiuntivi

La proprietà `[Remote]` dell'attributo `AdditionalFields` consente di convalidare combinazioni di campi rispetto ai dati nel server. Ad esempio, se il modello `User` avesse le proprietà `FirstName` e `LastName`, potrebbe essere necessario controllare che non siano già esistenti utenti con la stessa coppia di nomi. Nell'esempio riportato di seguito viene illustrato come usare `AdditionalFields`:

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

È stato possibile impostare in modo esplicito `AdditionalFields` sulle stringhe `"FirstName"` e `"LastName"`, ma usando l'operatore [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) il successivo refactoring risulta semplificato. Il metodo di azione per la convalida deve accettare gli argomenti sia per il nome e sia per il cognome:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

Quando si immette un nome o un cognome, JavaScript esegue una chiamata remota per verificare se tale coppia di nomi è in uso.

Per convalidare due o più campi aggiuntivi, specificarli sotto forma di elenco delimitato da virgole. Ad esempio, per aggiungere una proprietà `MiddleName` al modello, impostare l'attributo `[Remote]` come illustrato nell'esempio seguente:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, come tutti gli argomenti dell'attributo, deve essere un'espressione costante. Non usare quindi una [stringa interpolata](/dotnet/csharp/language-reference/keywords/interpolated-strings) oppure chiamare <xref:System.String.Join*> per inizializzare `AdditionalFields`.

## <a name="alternatives-to-built-in-attributes"></a>Alternative agli attributi predefiniti

Se si vuole usare un tipo di convalida non definita da attributi predefiniti, è possibile eseguire le operazioni seguenti:

* [Creare attributi personalizzati](#custom-attributes).
* [Implementare IValidatableObject](#ivalidatableobject).

## <a name="custom-attributes"></a>Attributi personalizzati

Per gli scenari non gestiti dagli attributi di convalida predefiniti, è possibile creare attributi di convalida personalizzati. Creare una classe che eredita da <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> ed eseguire l'override del metodo <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.

Il metodo `IsValid` accetta un oggetto denominato *value*, ovvero l'input da convalidare. Un overload accetta anche un oggetto `ValidationContext`, che contiene informazioni aggiuntive, ad esempio l'istanza del modello creato dall'associazione di modelli.

Nell'esempio seguente si convalida che la data di uscita di un film di genere *Classic* non sia successiva a un anno specifico. L'attributo `[ClassicMovie2]` prima controlla il genere, poi prosegue solo se il genere è *Classic*. Per i film identificati come classici, controlla la data di uscita per verificare che non sia successiva al limite passato al costruttore dell'attributo.

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

La variabile `movie` nell'esempio precedente rappresenta un oggetto `Movie` contenente i dati dell'invio del modulo. Il metodo `IsValid` controlla la data e il genere. Se la convalida ha esito positivo, `IsValid` restituisce un codice `ValidationResult.Success`. Se la convalida ha esito negativo, viene restituito un codice `ValidationResult` con un messaggio di errore.

## <a name="ivalidatableobject"></a>IValidatableObject

L'esempio precedente usa solo tipi `Movie`. Un'altra opzione per la convalida a livello di classe consiste nell'implementare `IValidatableObject` nella classe del modello, come illustrato nell'esempio seguente:

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a>Convalida del nodo di primo livello

I nodi di primo livello includono:

* Parametri di azione
* Proprietà del controller
* Parametri del gestore di pagina
* Proprietà del modello di pagina

Vengono convalidati i nodi di primo livello associati al modello, oltre a convalidare le proprietà del modello. Nell'esempio seguente dell'app di esempio il metodo `VerifyPhone` usa <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> per convalidare il parametro di azione `phone`:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

I nodi di primo livello possono usare <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> con gli attributi di convalida. Nell'esempio seguente dall'app di esempio, il metodo `CheckAge` specifica che il parametro `age` deve essere associato dalla stringa di query quando viene inviato il modulo:

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

La pagina Check Age (*CheckAge.cshtml*), include due moduli. Il primo modulo invia un valore `Age` `99` come stringa di query: `https://localhost:5001/Users/CheckAge?Age=99`.

Quando viene inviato un parametro correttamente formattato `age` dalla stringa di query, il modulo viene convalidato.

Il secondo modulo nella pagina Check Age invia il valore `Age` nel corpo della richiesta e la convalida ha esito negativo. L'associazione non riesce perché il parametro `age` deve provenire da una stringa di query.

Quando viene eseguita con `CompatibilityVersion.Version_2_1` o versione successiva, la convalida del nodo di primo livello è abilitata per impostazione predefinita. In caso contrario, la convalida del nodo di primo livello è disabilitata. L'opzione predefinita può essere sovrascritta impostando la proprietà <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> in (`Startup.ConfigureServices`), come illustrato di seguito:

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a>Numero massimo di errori

La convalida si interrompe quando viene raggiunto il numero di errori (200 per impostazione predefinita). È possibile configurare questo numero con il codice seguente in `Startup.ConfigureServices`:

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a>Numero massimo di ricorsioni

<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> attraversare l'oggetto grafico del modello che deve essere convalidato. Per i modelli molti profondi o ricorsivi all'infinito, la convalida può generare un overflow dello stack. [MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) consente di arrestare tempestivamente la convalida se la ricorsione del visitatore supera la profondità configurata. Il valore predefinito di `MvcOptions.MaxValidationDepth` è 200 quando la convalida viene eseguita con `CompatibilityVersion.Version_2_2` o versione successiva. Per le versioni precedenti, il valore è Null, vale a dire che non esistono limiti di profondità.

## <a name="automatic-short-circuit"></a>Corto circuito automatico

La convalida viene ignorata automaticamente (corto circuito) se l'oggetto grafico non richiede la convalida. Gli oggetti per cui viene ignorata la convalida sono le raccolte di primitive (ad esempio `byte[]`, `string[]`, `Dictionary<string, string>`) e gli oggetti grafici complessi che non hanno validator.

## <a name="disable-validation"></a>Disabilitare la convalida

Per disabilitare la convalida:

1. Creare un'implementazione di `IObjectModelValidator` che non contrassegna i campi come non validi.

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. Aggiungere il codice seguente a `Startup.ConfigureServices` per sostituire l'impostazione `IObjectModelValidator` predefinita nel contenitore di inserimento delle dipendenze.

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

Potrebbero essere visualizzati errori di stato del modello generati dall'associazione di modelli.

## <a name="client-side-validation"></a>Convalida lato client

La convalida lato client non consente l'invio del modulo finché non è ritenuto valido. Tramite il pulsante Invia viene eseguito JavaScript, che invia il modulo oppure visualizza i messaggi di errore.

La convalida lato client evitare un inutile round trip al server in caso di errori di input in un modulo. I riferimenti dello script seguenti in *_Layout.cshtml* e *_ValidationScriptsPartial.cshtml* supportano la convalida lato client:

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

Lo script [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) è una libreria front-end Microsoft personalizzata che si basa sul noto plug-in [jQuery Validate](https://jqueryvalidation.org/). Senza Query Unobtrusive Validation, sarebbe necessario scrivere il codice della stessa logica di convalida in due posizioni, vale a dire negli attributi di convalida lato server nelle proprietà del modello e nuovamente negli script lato client. Invece, gli [helper tag](xref:mvc/views/tag-helpers/intro)e agli [helper HTML](xref:mvc/views/overview) usano gli attributi di convalida e i metadati di tipo delle proprietà del modello per eseguire il rendering degli attributi `data-` HTML 5 negli elementi del modulo che devono essere convalidati. jQuery Unobtrusive Validation analizza gli attributi `data-` e passa la logica a jQuery Validate, "copiando" in modo efficace la logica di convalida lato server nel client. È possibile visualizzare gli errori di convalida nel client tramite gli helper tag, come illustrato di seguito:

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

Gli helper tag precedenti eseguono il rendering del codice HTML seguente.

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="">
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

Si noti che gli attributi `data-` nell'output HTML corrispondono agli attributi di convalida per la proprietà `ReleaseDate`. L'attributo `data-val-required` contiene un messaggio di errore che viene visualizzato se l'utente non compila il campo relativo alla data di uscita. Lo script jQuery Unobtrusive Validation passa questo valore al metodo [`required()`](https://jqueryvalidation.org/required-method/) jQuery Validate, che visualizza il messaggio nell'elemento **\<span>** di accompagnamento.

La convalida del tipo di dati si basa sul tipo .NET di una proprietà, a meno che sia sostituito dall'attributo `[DataType]`. I browser generano messaggi di errore predefiniti propri che il pacchetto jQuery Validation Unobtrusive Validation può comunque sostituire. Gli attributi `[DataType]` e le sottoclassi, ad esempio `[EmailAddress]`, consentono di specificare il messaggio di errore.

### <a name="add-validation-to-dynamic-forms"></a>Aggiungere la convalida a moduli dinamici

Quando la pagina viene caricata, jQuery Unobtrusive Validation passa la logica e i parametri di convalida a jQuery Validate. La convalida non viene quindi eseguita automaticamente nei moduli generati in modo dinamico. Per abilitare la convalida, chiedere a jQuery Unobtrusive Validation di analizzare il modulo dinamico immediatamente dopo essere stato creato. Il codice riportato di seguito configura ad esempio la convalida lato client in un modulo che è stato aggiunto tramite AJAX.

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

Il metodo `$.validator.unobtrusive.parse()` accetta un selettore jQuery per ogni argomento. Questo metodo indica a jQuery Unobtrusive Validation di analizzare gli attributi `data-` dei moduli all'interno di tale tipo di selettore. I valori di tali attributi vengono quindi passati al plug-in jQuery Validate.

### <a name="add-validation-to-dynamic-controls"></a>Aggiungere la convalida a controlli dinamici

Il metodo `$.validator.unobtrusive.parse()` funziona su un intero modulo e non sui singoli controlli generati dinamicamente, ad esempio `<input>` e `<select/>`. Per eseguire il reparse del modulo, rimuovere i dati di convalida aggiunti quando il modulo è stato precedentemente analizzato, come illustrato nell'esempio seguente:

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a>Convalida lato client personalizzata

La convalida lato client personalizzata viene eseguita generando attributi `data-` HTML che vengono usati con un adapter jQuery Validate personalizzato. Il codice dell'adapter di esempio seguente è stato scritto per gli attributi `ClassicMovie` e `ClassicMovie2` presentati precedentemente nell'articolo:

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

Per informazioni sulla scrittura degli adapter, vedere la [documentazione di jQuery Validate](http://jqueryvalidation.org/documentation/).

L'uso di un adapter per un determinato campo viene attivato dagli attributi `data-`, i quali eseguono le operazioni seguenti:

* Contrassegnano il campo come campo da sottoporre a convalida (`data-val="true"`).
* Identificano un nome della regola di convalida e un testo del messaggio di errore, ad esempio `data-val-rulename="Error message."`.
* Specificare i parametri aggiuntivi necessari al validator, ad esempio `data-val-rulename-parm1="value"`.

L'esempio seguente contiene gli attributi `data-` per l'attributo `ClassicMovie` dell'app di esempio:

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

Come già accennato in precedenza, gli [helper tag](xref:mvc/views/tag-helpers/intro) e gli [helper HTML](xref:mvc/views/overview) usano le informazioni degli attributi di convalida per eseguire il rendering degli attributi `data-`. Per scrivere il codice che crea attributi HTML `data-` personalizzati, sono disponibili due opzioni:

* Creare una classe che deriva da `AttributeAdapterBase<TAttribute>` e una classe che implementa `IValidationAttributeAdapterProvider` e registrare l'attributo e il relativo adapter nell'inserimento delle dipendenze. Questo metodo segue il [principio di singola responsabilità](https://wikipedia.org/wiki/Single_responsibility_principle) in quanto il codice di convalida relativo a server e client si trova in classi separate. L'adapter offre anche un altro vantaggio. Essendo registrato nell'inserimento delle dipendenze, può usare gli altri servizi disponibili nell'inserimento delle dipendenze se necessario.
* Implementare `IClientModelValidator` nella classe `ValidationAttribute`. Questo metodo potrebbe essere appropriato se l'attributo non esegue la convalida lato server e non richiede altri servizi dell'inserimento delle dipendenze.

### <a name="attributeadapter-for-client-side-validation"></a>AttributeAdapter per la convalida lato client

Questo metodo di rendering degli attributi `data-` in HTML viene usato dall'attributo `ClassicMovie` nell'app di esempio. Per aggiungere la convalida lato client usando questo metodo:

1. Creare una classe di adapter dell'attributo per l'attributo di convalida personalizzata. Derivare la classe da [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2). Creare un metodo `AddValidation` che aggiunge gli attributi `data-` all'output sottoposto a rendering, come illustrato in questo esempio:

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. Creare una classe di provider dell'adapter che implementa <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>. Nel metodo `GetAttributeAdapter` passare l'attributo personalizzato al costruttore dell'adapter, come illustrato in questo esempio:

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. Registrare il provider dell'adapter per l'inserimento delle dipendenze in `Startup.ConfigureServices`:

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a>IClientModelValidator per la convalida lato client

Questo metodo di rendering degli attributi `data-` in HTML viene usato dall'attributo `ClassicMovie2` nell'app di esempio. Per aggiungere la convalida lato client usando questo metodo:

* Nell'attributo di convalida personalizzato implementare l'interfaccia `IClientModelValidator` e creare un metodo `AddValidation`. Nel metodo `AddValidation` aggiungere gli attributi `data-` per la convalida, come illustrato nell'esempio seguente:

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a>Disabilitare la convalida lato client

Il codice seguente disabilita la convalida lato client nelle visualizzazioni MVC:

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

E in Razor Pages:

[!code-csharp[](validation/sample_snapshot/Startup3.cs?name=snippet_DisableClientValidation)]

Un'altra opzione per disabilitare la convalida lato client consiste nell'impostare come commento il riferimento a `_ValidationScriptsPartial` nel file *.cshtml*.

## <a name="additional-resources"></a>Risorse aggiuntive

* [System.ComponentModel.DataAnnotations namespace](xref:System.ComponentModel.DataAnnotations)
* [Associazione di modelli](model-binding.md)
