---
title: Convalida del modello in ASP.NET MVC di base
author: rachelappel
description: Informazioni sulla convalida del modello in ASP.NET MVC di base.
keywords: Convalida di ASP.NET Core, MVC,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3a8676dd-7ed8-4a05-bca2-44e288ab99ee
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: efbc68e898cadd06d61fa69914fe08f3a12ba802
ms.sourcegitcommit: 8b5733f1cd5d2c2b6d432bf82fcd4be2d2d6b2a3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2017
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

Non nullable [i tipi di valore](/dotnet/csharp/language-reference/keywords/value-types) (ad esempio `decimal`, `int`, `float`, e `DateTime`) sono intrinsecamente necessari e non è necessaria la `Required` attributo. L'applicazione non esegue alcun controllo di convalida sul lato server per i tipi non contrassegnati `Required`.

Associazione di modelli MVC, che non è attualmente preoccupata per convalida e gli attributi di convalida, rifiuta l'invio di un campo di form che contiene un valore mancante o spazi vuoti per un tipo non nullable. In assenza di un `BindRequired` attributo sulla proprietà di destinazione, l'associazione di modelli ignora i dati mancanti per i tipi che non ammette valori null, in cui il campo del form è assente da dati del form in ingresso.

Il [BindRequired attributo](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (vedere anche [personalizzare il comportamento di associazione del modello con attributi](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) è utile per verificare i dati del form sono stati completati. Se applicato a una proprietà, il sistema di associazione del modello richiede un valore per tale proprietà. Quando applicato a un tipo, il sistema di associazione del modello richiede valori per tutte le proprietà di quel tipo.

Quando si utilizza un [Nullable\<T > tipo](/dotnet/csharp/programming-guide/nullable-types/) (ad esempio, `decimal?` o `System.Nullable<decimal>`) e contrassegnarla `Required`, viene eseguito un controllo di convalida sul lato server come se la proprietà fosse un tipo nullable standard (per l'esempio, un `string`).

La convalida lato client richiede un valore per un campo modulo che corrisponde a una proprietà del modello che è stata contrassegnata `Required` e per una proprietà di tipo non nullable non sono stati contrassegnati `Required`. `Required`può essere utilizzato per controllare il messaggio di errore di convalida lato client.

## <a name="model-state"></a>Stato del modello

Stato del modello rappresenta gli errori di convalida nei valori di form HTML inviati.

MVC continuerà convalida dei campi finché non raggiunge il numero massimo di errori (200 per impostazione predefinita). È possibile configurare questo numero, inserendo il codice seguente nel `ConfigureServices` metodo il *Startup.cs* file:

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a>Lo stato del modello di gestione degli errori

Si verifica la convalida del modello prima di ogni azione del controller richiamato ed è responsabilità del metodo di azione per controllare `ModelState.IsValid` e rispondere nel modo appropriato. In molti casi, la reazione appropriata è di restituire un tipo di risposta di errore, in teoria che riporta in dettaglio il motivo per cui non è riuscita la convalida del modello.

Alcune applicazioni è possibile scegliere di seguire una convenzione standard per la gestione di errori di convalida del modello, nel qual caso un filtro può essere un luogo adatto per implementare criteri di. È consigliabile testare il comportamento delle azioni con gli stati di modello valido e non validi.

## <a name="manual-validation"></a>Convalida manuale

Una volta completata la convalida e l'associazione di modelli, si desidera ripetere le parti. Ad esempio, un utente sia stato immesso testo in un campo a cui è previsto un numero intero o potrebbe essere necessario calcolare un valore per proprietà di un modello.

Potrebbe essere necessario eseguire manualmente la convalida. A tale scopo, chiamare il `TryValidateModel` (metodo), come illustrato di seguito:

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a>Convalida personalizzata

Gli attributi di convalida di lavoro per la maggior parte delle esigenze di convalida. Tuttavia, alcune regole di convalida sono specifici per l'azienda, come non si appena è necessaria la convalida di dati generici, ad esempio garantire un campo o la conformità a un intervallo di valori. Per questi scenari, gli attributi di convalida personalizzata sono un'ottima soluzione. La creazione di attributi di convalida personalizzata in MVC è semplice. Solo ereditare il `ValidationAttribute`ed eseguire l'override di `IsValid` metodo. Il `IsValid` metodo accetta due parametri, il primo è un oggetto denominato *valore* e il secondo è un `ValidationContext` oggetto denominato *validationContext*. *Valore* fa riferimento al valore effettivo del campo che convalida il validator personalizzato.

Nell'esempio seguente indica che gli utenti non possono impostare il genere una regola business *classico* per un filmato rilasciato dopo 1960. Il `[ClassicMovie]` attributo controlla per primo il genere e se è un classico, quindi controlla la data di rilascio per verificare che è successiva a quella 1960. Se viene rilasciato dopo 1960, la convalida non riesce. L'attributo accetta un parametro integer che rappresenta l'anno che è possibile utilizzare per convalidare i dati. È possibile acquisire il valore del parametro nel costruttore dell'attributo, come illustrato di seguito:

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-28)]

Il `movie` variabile sopra rappresenta un `Movie` oggetto che contiene i dati per convalidare l'invio del modulo. In questo caso, il codice di convalida controlla la data e un paese di `IsValid` metodo il `ClassicMovieAttribute` classe in base alle regole. Dopo la convalida `IsValid` restituisce un `ValidationResult.Success` codice, e quando la convalida non riesce, un `ValidationResult` con un messaggio di errore. Quando un utente modifica il `Genre` campo e invia il form, il `IsValid` metodo il `ClassicMovieAttribute` verificherà se il film sia un classico. Ad esempio un attributo predefinito, applicare il `ClassicMovieAttribute` a una proprietà, ad esempio `ReleaseDate` per garantire che convalida viene eseguita, come illustrato nell'esempio di codice precedente. Poiché l'esempio funziona solo con `Movie` tipi, un'opzione migliore consiste nell'utilizzare `IValidatableObject` come illustrato nel paragrafo seguente.

In alternativa, questo codice stesso può essere inserito nel modello mediante l'implementazione di `Validate` metodo il `IValidatableObject` interfaccia. Mentre gli attributi di convalida personalizzata funziona anche per la convalida delle proprietà singole, implementazione `IValidatableObject` può essere utilizzato per implementare la convalida a livello di classe come illustrato di seguito.

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=33-41)]

## <a name="client-side-validation"></a>Convalida lato client

La convalida lato client è una maggiore praticità per gli utenti. Consente di risparmiare tempo in caso contrario spendere in attesa di un round trip al server. In termini di business, anche alcuni le frazioni di secondo moltiplicato centinaia di volte al giorno consente di aggiungere fino a una quantità elevata di tempo, expense e frustrazione. Convalida semplice e immediata consente agli utenti di lavorare in modo più efficiente e ottenere una migliore qualità di input e output.

È necessario disporre una vista con i riferimenti a script JavaScript appropriate sul posto per la convalida lato client funzionare come seguito.

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

Per convalidare i dati e visualizzare eventuali messaggi di errore utilizzando JavaScript, MVC Usa attributi di convalida oltre ai metadati del tipo di proprietà del modello. Quando si utilizza MVC per il rendering degli elementi di formato da un modello utilizzando [gli helper di Tag](xref:mvc/views/tag-helpers/intro) o [helper HTML](xref:mvc/views/overview) verranno aggiunti HTML 5 [attributi dati](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) negli elementi del form che devono essere convalidate, come di seguito riportato. MVC genera il `data-` gli attributi per gli attributi incorporati e personalizzati. È possibile visualizzare gli errori di convalida sul client utilizzando gli helper di tag rilevanti, come illustrato di seguito:

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

Gli helper di tag precedenti in formato HTML riportato di seguito. Si noti che il `data-` attributi nel codice HTML di output corrispondono agli attributi di convalida per il `ReleaseDate` proprietà. Il `data-val-required` attributo seguente contiene un messaggio di errore da visualizzare se l'utente non compila il campo Data di rilascio e consente di visualizzare che il messaggio di accompagnamento  **\<span >** elemento.

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

È possibile implementare la convalida remota in un processo in due fasi. In primo luogo, è necessario annotare il modello con il `[Remote]` attributo. Il `[Remote]` attributo accetta più overload, è possibile utilizzare per indirizzare JavaScript sul lato client per il codice appropriato da chiamare. Nell'esempio punta al `VerifyEmail` il metodo di azione del `Users` controller.

[!code-csharp[Main](validation/sample/User.cs?range=5-9)]

Il secondo passaggio è l'inserimento di codice di convalida nel metodo di azione corrispondente come definito nel `[Remote]` attributo. Restituisce un `JsonResult` che sul lato client consente di continuare o sospendere e visualizzare un errore, se necessario.

[!code-none[Main](validation/sample/UsersController.cs?range=19-28)]

Ora quando gli utenti immettono un messaggio di posta elettronica, JavaScript nella visualizzazione effettua una chiamata remota per vedere se è stato eseguito il messaggio di posta elettronica e in tal caso, viene visualizzato il messaggio di errore. In caso contrario, l'utente può inviare il modulo come di consueto.
