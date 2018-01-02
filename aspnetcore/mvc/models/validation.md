---
title: Convalida del modello in ASP.NET MVC di base
author: rachelappel
description: Informazioni sulla convalida del modello in ASP.NET MVC di base.
keywords: Convalida di ASP.NET Core, MVC,
ms.author: riande
manager: wpickett
ms.date: 12/18/2016
ms.topic: article
ms.assetid: 3a8676dd-7ed8-4a05-bca2-44e288ab99ee
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.openlocfilehash: 7f641c247cb672934e76fa13bc7b7beb3990dd82
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/19/2017
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a>Introduzione alla convalida del modello in ASP.NET MVC di base

Da [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-validation"></a>Introduzione alla convalida del modello

Prima di un'app archivia i dati in un database, l'applicazione deve convalidare i dati. Dati devono essere verificati potenziali minacce alla sicurezza, verificate che viene formattato in modo appropriato per tipo e dimensione, e deve essere conforme alle regole. Sebbene possa essere ridondante e difficile da implementare, è necessaria la convalida. In MVC, la convalida viene eseguita nel client e server.

Fortunatamente, .NET è astratto convalida in attributi di convalida. Questi attributi contengono codice di convalida, riducendo la quantità di codice da scrivere.

[Consente di visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).

## <a name="validation-attributes"></a>Attributi di convalida

Gli attributi di convalida sono un modo per configurare la convalida del modello in modo simile a livello concettuale convalida nei campi nelle tabelle di database. Sono inclusi i vincoli, ad esempio l'assegnazione di tipi di dati o i campi obbligatori. Altri tipi di convalida includono l'applicazione di modelli di dati per applicare le regole di business, ad esempio una carta di credito, il numero di telefono o un indirizzo di posta elettronica. Gli attributi di convalida assicurarsi di applicare questi requisiti molto più semplici e facile da utilizzare.

Di seguito è riportato un annotazioni `Movie` modello da un'app archivia informazioni sui film e programmi TV. La maggior parte delle proprietà sono necessario e diverse proprietà di stringa sono i requisiti di lunghezza. Inoltre, non c'è una restrizione di intervallo numerico in atto per la `Price` proprietà da 0 a $999,99, insieme a un attributo di convalida personalizzata.

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

Semplicemente leggendo il modello vengono visualizzate le regole relative ai dati per questa app, semplificando la gestione del codice. Di seguito sono diversi attributi di convalida incorporate comuni:

* `[CreditCard]`: Consente di convalidare la proprietà ha un formato di carta di credito.

* `[Compare]`: Convalida due proprietà in una corrispondenza del modello.

* `[EmailAddress]`: Consente di convalidare la proprietà ha un formato di posta elettronica.

* `[Phone]`: Consente di convalidare la proprietà ha un formato telefonico.

* `[Range]`: Consente di convalidare il valore di proprietà rientra l'intervallo specificato.

* `[RegularExpression]`: Verifica che i dati corrispondano all'espressione regolare specificata.

* `[Required]`: Rende una proprietà necessaria.

* `[StringLength]`: Consente di convalidare che una proprietà di stringa ha più la lunghezza massima specificata.

* `[Url]`: Consente di convalidare la proprietà ha un formato di URL.

MVC supporta qualsiasi attributo che deriva da `ValidationAttribute` ai fini della convalida. Numero di attributi di convalida utile è reperibile nel [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) dello spazio dei nomi.

Potrebbero essere presenti istanze in cui è necessario più funzionalità rispetto a forniscono attributi predefiniti. Tutte le volte, è possibile creare gli attributi di convalida personalizzata mediante derivazione da `ValidationAttribute` o la modifica del modello per implementare `IValidatableObject`.

## <a name="notes-on-the-use-of-the-required-attribute"></a>Note sull'utilizzo dell'attributo obbligatorio

I [tipi valore](/dotnet/csharp/language-reference/keywords/value-types) non nullable (ad esempio `decimal`, `int`, `float` e `DateTime`) sono intrinsecamente necessari e non richiedono l'attributo `Required`. L'applicazione non esegue alcun controllo di convalida sul lato server per i tipi non contrassegnati `Required`.

Associazione di modelli MVC, che non è attualmente preoccupata per convalida e gli attributi di convalida, rifiuta l'invio di un campo di form che contiene un valore mancante o spazi vuoti per un tipo non nullable. In assenza di un `BindRequired` attributo sulla proprietà di destinazione, l'associazione di modelli ignora i dati mancanti per i tipi che non ammette valori null, in cui il campo del form è assente da dati del form in ingresso.

Il [BindRequired attributo](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (vedere anche [personalizzare il comportamento di associazione del modello con attributi](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) è utile per verificare i dati del form sono stati completati. Se applicato a una proprietà, il sistema di associazione del modello richiede un valore per tale proprietà. Quando applicato a un tipo, il sistema di associazione del modello richiede valori per tutte le proprietà di quel tipo.

Quando si utilizza un [Nullable\<T > tipo](/dotnet/csharp/programming-guide/nullable-types/) (ad esempio, `decimal?` o `System.Nullable<decimal>`) e contrassegnarla `Required`, viene eseguito un controllo di convalida sul lato server come se la proprietà fosse un tipo nullable standard (per l'esempio, un `string`).

La convalida lato client richiede un valore per un campo modulo che corrisponde a una proprietà del modello che è stata contrassegnata `Required` e per una proprietà di tipo non nullable non sono stati contrassegnati `Required`. `Required`può essere utilizzato per controllare il messaggio di errore di convalida lato client.

## <a name="model-state"></a>Stato del modello

Stato del modello rappresenta gli errori di convalida nei valori di form HTML inviati.

MVC continuerà convalida dei campi finché non raggiunge il numero massimo di errori (200 per impostazione predefinita). È possibile configurare questo numero, inserendo il codice seguente nel `ConfigureServices` metodo il *Startup.cs* file:

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>Lo stato del modello di gestione degli errori

Si verifica la convalida del modello prima di ogni azione del controller richiamato ed è responsabilità del metodo di azione per controllare `ModelState.IsValid` e rispondere nel modo appropriato. In molti casi, la reazione appropriata è di restituire una risposta di errore, in teoria che riporta in dettaglio il motivo per cui non è riuscita la convalida del modello.

Alcune applicazioni è possibile scegliere di seguire una convenzione standard per la gestione di errori di convalida del modello, nel qual caso un filtro può essere un luogo adatto per implementare criteri di. È consigliabile testare il comportamento delle azioni con gli stati di modello valido e non validi.

## <a name="manual-validation"></a>Convalida manuale

Una volta completata la convalida e l'associazione di modelli, si desidera ripetere le parti. Ad esempio, un utente sia stato immesso testo in un campo a cui è previsto un numero intero o potrebbe essere necessario calcolare un valore per proprietà di un modello.

Potrebbe essere necessario eseguire manualmente la convalida. A tale scopo, chiamare il `TryValidateModel` (metodo), come illustrato di seguito:

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Convalida personalizzata

Gli attributi di convalida di lavoro per la maggior parte delle esigenze di convalida. Tuttavia, alcune regole di convalida sono specifici per l'azienda. Le regole potrebbero non essere più comuni tecniche di convalida di dati, ad esempio, è necessario garantire un campo o che sia conforme a un intervallo di valori. Per questi scenari, gli attributi di convalida personalizzata sono un'ottima soluzione. La creazione di attributi di convalida personalizzata in MVC è semplice. Solo ereditare il `ValidationAttribute`ed eseguire l'override di `IsValid` metodo. Il `IsValid` metodo accetta due parametri, il primo è un oggetto denominato *valore* e il secondo è un `ValidationContext` oggetto denominato *validationContext*. *Valore* fa riferimento al valore effettivo del campo che convalida il validator personalizzato.

Nell'esempio seguente indica che gli utenti non possono impostare il genere una regola business *classico* per un filmato rilasciato dopo 1960. Il `[ClassicMovie]` attributo controlla per primo il genere e se è un classico, quindi controlla la data di rilascio per verificare che è successiva a quella 1960. Se viene rilasciato dopo 1960, la convalida non riesce. L'attributo accetta un parametro integer che rappresenta l'anno che è possibile utilizzare per convalidare i dati. È possibile acquisire il valore del parametro nel costruttore dell'attributo, come illustrato di seguito:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

Il `movie` variabile sopra rappresenta un `Movie` oggetto che contiene i dati per convalidare l'invio del modulo. In questo caso, il codice di convalida controlla la data e un paese di `IsValid` metodo il `ClassicMovieAttribute` classe in base alle regole. Dopo la convalida `IsValid` restituisce un `ValidationResult.Success` codice, e quando la convalida non riesce, un `ValidationResult` con un messaggio di errore. Quando un utente modifica il `Genre` campo e invia il form, il `IsValid` metodo il `ClassicMovieAttribute` verificherà se il film sia un classico. Ad esempio un attributo predefinito, applicare il `ClassicMovieAttribute` a una proprietà, ad esempio `ReleaseDate` per garantire che convalida viene eseguita, come illustrato nell'esempio di codice precedente. Poiché l'esempio funziona solo con `Movie` tipi, un'opzione migliore consiste nell'utilizzare `IValidatableObject` come illustrato nel paragrafo seguente.

In alternativa, questo codice stesso può essere inserito nel modello mediante l'implementazione di `Validate` metodo il `IValidatableObject` interfaccia. Mentre gli attributi di convalida personalizzata funziona anche per la convalida delle proprietà singole, implementazione `IValidatableObject` può essere utilizzato per implementare la convalida a livello di classe come illustrato di seguito.

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a>Convalida lato client

La convalida lato client è una maggiore praticità per gli utenti. Consente di risparmiare tempo in caso contrario spendere in attesa di un round trip al server. In termini di business, anche alcuni le frazioni di secondo moltiplicato centinaia di volte al giorno consente di aggiungere fino a una quantità elevata di tempo, expense e frustrazione. Convalida semplice e immediata consente agli utenti di lavorare in modo più efficiente e ottenere una migliore qualità di input e output.

È necessario disporre una vista con i riferimenti a script JavaScript appropriate sul posto per la convalida lato client funzionare come seguito.

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

Il [jQuery non intrusiva convalida](https://github.com/aspnet/jquery-validation-unobtrusive) script è una libreria di front-end Microsoft personalizzata che si basa sulla famosa [convalida jQuery](https://jqueryvalidation.org/) plug-in. Senza jQuery non intrusiva convalida, è necessario codice la stessa logica di convalida in due posizioni: una volta negli attributi di convalida sul lato server nelle proprietà del modello, quindi nuovamente in script sul lato client (gli esempi per jQuery convalida [ `validate()` ](https://jqueryvalidation.org/validate/) metodo mostra come complesso questo potrebbe diventare). Invece di MVC [gli helper di Tag](xref:mvc/views/tag-helpers/intro) e [helper HTML](xref:mvc/views/overview) sono in grado di utilizzare gli attributi di convalida e metadati dalle proprietà dei modelli per eseguire il rendering HTML 5 [attributi dati](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) in gli elementi di formato che devono essere convalidate. MVC genera il `data-` gli attributi per gli attributi incorporati e personalizzati. jQuery non intrusiva convalida quindi analizzata thes `data-` attributi e passa la logica di convalida, in modo efficace "copia" la logica di convalida sul lato server al client jQuery. È possibile visualizzare gli errori di convalida sul client utilizzando gli helper di tag rilevanti, come illustrato di seguito:

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Gli helper di tag precedenti in formato HTML riportato di seguito. Si noti che il `data-` attributi nel codice HTML di output corrispondono agli attributi di convalida per il `ReleaseDate` proprietà. Il `data-val-required` attributo seguente contiene un messaggio di errore da visualizzare se l'utente non compila il campo Data di rilascio. jQuery non intrusiva convalida tale valore viene passato per la convalida jQuery [ `required()` ](https://jqueryvalidation.org/required-method/) (metodo), che consente di visualizzare il messaggio di accompagnamento  **\<span >** elemento.

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
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

La convalida lato client impedisce l'invio fino a quando il form è valido. Pulsante Invia esegue JavaScript che invia il form o Visualizza messaggi di errore.

MVC determina i valori di attributo di tipo in base al tipo di dati .NET di una proprietà, probabilmente sottoposto a override utilizzando `[DataType]` attributi. La base `[DataType]` attributo non esegue alcuna convalida reale lato server. Browser scegliere i propri messaggi di errore e visualizzare gli errori, ma vogliono, tuttavia il pacchetto non intrusivo convalida jQuery è possibile ignorare i messaggi e visualizzarli in modo coerente con altri utenti. Questo errore si verifica più ovviamente quando gli utenti applicare `[DataType]` sottoclassi, ad esempio `[EmailAddress]`.

### <a name="add-validation-to-dynamic-forms"></a>Aggiungere la convalida di moduli dinamici

Poiché jQuery non intrusiva convalida passa la logica di convalida e i parametri di convalida jQuery al primo caricamento della pagina, form generato in modo dinamico non assumerà automaticamente la convalida. In alternativa, è necessario indicare jQuery non intrusiva convalida per analizzare il modulo dinamico immediatamente dopo averlo creato. Ad esempio, il codice riportato di seguito viene illustrato come è possibile impostare la convalida lato client in un form aggiunto tramite AJAX.

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
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

Il `$.validator.unobtrusive.parse()` metodo accetta un selettore di jQuery per un argomento. Questo metodo indica jQuery non intrusiva convalida per analizzare il `data-` gli attributi dei moduli all'interno di tale tipo di selettore. I valori di questi attributi vengono quindi passati per il plug-in di convalida jQuery in modo che il modulo presenta le regole di convalida sul lato client desiderato.

### <a name="add-validation-to-dynamic-controls"></a>Aggiungere la convalida per i controlli dinamici

È inoltre possibile aggiornare le regole di convalida in un form quando singoli controlli, ad esempio `<input/>`s e `<select/>`s, vengono generati in modo dinamico. Non è possibile passare i selettori per questi elementi per il `parse()` metodo direttamente perché circostanti analisi è già stata completata e non verrà aggiornata.  In alternativa, è innanzitutto rimuovere i dati di convalida esistenti, quindi reparse l'intero form, come illustrato di seguito:

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        form.removeData("validator")    // Added by the raw jQuery Validate
            .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a>IClientModelValidator

È possibile creare logica sul lato client per l'attributo personalizzato, e [convalida non intrusiva](http://jqueryvalidation.org/documentation/) verrà eseguito sul client per l'utente automaticamente come parte della convalida. Il primo passaggio consiste nel controllare gli attributi di dati vengono aggiunti mediante l'implementazione di `IClientModelValidator` interfaccia come illustrato di seguito:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

Gli attributi che implementano questa interfaccia è possono aggiungere campi generati di attributi HTML. Esaminare l'output per il `ReleaseDate` elemento rivela HTML che è simile all'esempio precedente, ma ora è un `data-val-classicmovie` attributo definita in precedenza il `AddValidation` metodo `IClientModelValidator`.

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

La convalida non intrusiva utilizza i dati di `data-` gli attributi da visualizzare i messaggi di errore. Tuttavia, jQuery non riconosce le regole di messaggi o finché non vengono aggiunti del jQuery `validator` oggetto. Come illustrato nell'esempio riportato di seguito che aggiunge un metodo denominato `classicmovie` contenente codice di convalida personalizzata per il jQuery `validator` oggetto.

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

JQuery dispone ora le informazioni per eseguire la convalida personalizzata di JavaScript, nonché il messaggio di errore da visualizzare se il codice di convalida restituisce false.

## <a name="remote-validation"></a>Convalida remota

La convalida remota è una caratteristica fondamentale da utilizzare quando è necessario convalidare i dati nel client rispetto ai dati nel server. Ad esempio, l'app potrebbe essere necessario verificare se un nome utente o di posta elettronica è già in uso e deve eseguire la query una grande quantità di dati a tale scopo. Download di grandi set di dati per la convalida di uno o alcuni campi utilizza troppe risorse. Può anche esporre informazioni riservate. In alternativa è possibile effettuare una richiesta di andata e ritorno per convalidare un campo.

È possibile implementare la convalida remota in un processo in due fasi. In primo luogo, è necessario annotare il modello con il `[Remote]` attributo. Il `[Remote]` attributo accetta più overload, è possibile utilizzare per indirizzare JavaScript sul lato client per il codice appropriato da chiamare. Nell'esempio seguente fa riferimento al `VerifyEmail` il metodo di azione del `Users` controller.

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

Il secondo passaggio è l'inserimento di codice di convalida nel metodo di azione corrispondente come definito nel `[Remote]` attributo. Secondo la convalida jQuery [ `remote()` ](https://jqueryvalidation.org/remote-method/) documentazione relativa al metodo:

> La risposta di serverside deve essere una stringa JSON che deve essere `"true"` per elementi validi e può essere `"false"`, `undefined`, o `null` per elementi non validi, utilizzando il messaggio di errore predefinito. Se la risposta serverside è una stringa, ad esempio. `"That name is already taken, try peter123 instead"`, questa stringa verrà visualizzata come un messaggio di errore personalizzato al posto del valore predefinito.

La definizione di `VerifyEmail()` metodo segue queste regole, come illustrato di seguito. Restituisce un errore di convalida del messaggio se non viene eseguita, il messaggio di posta elettronica o `true` se il messaggio di posta elettronica è gratuito e include il risultato in un `JsonResult` oggetto. Sul lato client può quindi utilizzare il valore restituito per continuare o visualizzare l'errore, se necessario.

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

Quando gli utenti immettono un messaggio di posta elettronica, JavaScript nella visualizzazione diventerà una chiamata remota per verificare se tale messaggio di posta elettronica è stato eseguito e, in caso affermativo, viene visualizzato il messaggio di errore. In caso contrario, l'utente può inviare il modulo come di consueto.

Il `AdditionalFields` proprietà del `[Remote]` attributo è utile per la convalida delle combinazioni di campi sui dati nel server.  Ad esempio, se il `User` modello precedente ha due proprietà aggiuntive chiamata `FirstName` e `LastName`, si consiglia di verificare che nessun utente esistente dispongano di tale coppia di nomi.  Per definire le proprietà di nuovo, come illustrato nel codice seguente:

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

`AdditionalFields`avrebbero potuto essere impostate in modo esplicito per le stringhe `"FirstName"` e `"LastName"`, ma tramite il [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) operatore questo semplifica il refactoring in un secondo momento.  Il metodo di azione per eseguire la convalida deve quindi accetta due argomenti, uno per il valore di `FirstName` e uno per il valore di `LastName`.


[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

A questo punto quando gli utenti immettere un nome e cognome, JavaScript:

* Effettua una chiamata remota per vedere se è stato eseguito tale coppia di nomi.
* Se la coppia è stata eseguita, viene visualizzato un messaggio di errore. 
* Se non è stato eseguito, l'utente può inviare il modulo.

Se si desidera convalidare uno o più campi aggiuntivi con il `[Remote]` attributo, fornito come un elenco delimitato da virgole.  Ad esempio, per aggiungere un `MiddleName` impostata per il modello, il `[Remote]` attributo come illustrato nel codice seguente:

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`, come tutti gli argomenti di attributo, deve essere un'espressione costante.  Pertanto, non è necessario utilizzare un [interpolati stringa](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interpolated-strings) o chiamare [ `string.Join()` ](https://msdn.microsoft.com/en-us/library/system.string.join(v=vs.110).aspx) inizializzare `AdditionalFields`. Per ogni campo aggiuntivo che si aggiunge al `[Remote]` attributo, è necessario aggiungere un altro argomento al metodo di azione del controller corrispondente.
