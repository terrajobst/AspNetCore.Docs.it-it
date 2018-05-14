---
title: Associazione di modelli personalizzata in ASP.NET Core
author: ardalis
description: Informazioni su come l'associazione di modelli consente alle azioni del controller di funzionare direttamente con i tipi di modello in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/10/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: a687753083d3b11898e9ff35828780a5ad240854
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="custom-model-binding-in-aspnet-core"></a>Associazione di modelli personalizzata in ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

Mediante l'associazione di modelli le azioni dei controller possono operare direttamente con i tipi di modello (passati come argomenti dei metodi), anziché con le richieste HTTP. Il mapping tra i dati delle richieste in ingresso e i modelli applicativi è gestito dagli strumenti di associazione di modelli. Gli sviluppatori possono estendere la funzionalità di associazione di modelli predefinita implementando strumenti di associazione di modelli personalizzati (anche se in genere non è necessario creare un provider personalizzato).

[Visualizzare o scaricare un esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Limiti degli strumenti di associazione di modelli predefiniti

Gli strumenti di associazione di modelli predefiniti supportano la maggior parte dei tipi di dati .NET Core comuni e soddisfano le esigenze della maggior parte degli sviluppatori. Tali strumenti associano direttamente l'input di testo della richiesta ai tipi di modello. Può essere necessario trasformare l'input prima dell'associazione. Un esempio è il caso in cui è presente una chiave che può essere usata per cercare i dati del modello. È possibile usare uno strumento di associazione di modelli personalizzato per il recupero di dati in base alla chiave.

## <a name="model-binding-review"></a>Analisi dell'associazione di modelli

L'associazione di modelli usa definizioni specifiche per i tipi sui quali opera. Un *tipo semplice* viene convertito da una stringa singola dell'input. Un *tipo complesso* viene convertito da più valori di input. Il framework determina la differenza in base all'esistenza di un elemento `TypeConverter`. È consigliabile creare un convertitore di tipi se è presente un mapping `string` -> `SomeType` semplice che non richiede risorse esterne.

Prima di creare uno strumento di associazione di modelli personalizzato, è utile esaminare le modalità di implementazione degli strumenti di associazione di modelli esistenti. Considerare [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder), che consente di convertire le stringhe con codifica base64 in matrici di byte. Le matrici di byte vengono spesso archiviate come file o campi BLOB del database.

### <a name="working-with-the-bytearraymodelbinder"></a>Uso di ByteArrayModelBinder

Le stringhe con codifica Base64 possono essere usate per rappresentare i dati binari. Ad esempio l'immagine seguente può essere codificata come stringa.

![bot dotnet](custom-model-binding/images/bot.png "bot dotnet")

Una piccola parte della stringa codificata è visualizzata nella figura seguente:

![bot dotnet codificato](custom-model-binding/images/encoded-bot.png "bot dotnet codificato")

Seguire le istruzioni nel [file Leggimi dell'esempio](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) per convertire la stringa con codifica base64 in un file.

ASP.NET Core MVC può accettare una stringa con codifica base64 e usare un `ByteArrayModelBinder` per convertirla in una matrice di byte. L'elemento [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) che implementa [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) esegue il mapping degli argomenti `byte[]` a `ByteArrayModelBinder`:

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

Quando si crea uno strumento di associazione di modelli personalizzato è possibile implementare il proprio tipo `IModelBinderProvider` o usare [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).

L'esempio indica come usare `ByteArrayModelBinder` per convertire una stringa con codifica base64 in un `byte[]` e salvare il risultato in un file:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

È possibile pubblicare (POST) una stringa con codifica base64 in questo metodo API usando uno strumento come [Postman](https://www.getpostman.com/):

![postman](custom-model-binding/images/postman.png "postman")

Se lo strumento di associazione riesce ad associare i dati della richiesta a proprietà o argomenti denominati in modo appropriato, l'associazione di modelli ha esito positivo. L'esempio seguente indica come usare `ByteArrayModelBinder` con un modello di visualizzazione:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Esempio di strumento di associazione di modelli personalizzato

In questa sezione viene implementato uno strumento di associazione di modelli personalizzato che:

- Converte i dati della richiesta in ingresso in argomenti chiave fortemente tipizzati.
- Usa Entity Framework Core per recuperare l'entità associata.
- Passa l'entità associata come argomento al metodo di azione.

L'esempio seguente usa l'attributo `ModelBinder` nel modello `Author`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

Nel codice precedente l'attributo `ModelBinder` specifica il tipo di `IModelBinder` da usare per associare i parametri di azione `Author`. 

`AuthorEntityBinder` viene usato per associare un parametro `Author` recuperando l'entità da un'origine dati tramite Entity Framework Core e un `authorId`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

L'esempio di codice seguente indica come usare `AuthorEntityBinder` in un metodo di azione:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

L'attributo `ModelBinder` può essere usato per applicare gli elementi `AuthorEntityBinder` ai parametri che non usano convenzioni predefinite:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

In questo esempio, dato che il nome dell'argomento non è il valore `authorId` predefinito, viene specificato nel parametro usando l'attributo `ModelBinder`. Si noti che sia controller sia il metodo di azione risultano più semplici rispetto alla ricerca dell'entità nel metodo di azione. La logica per recuperare l'autore usando Entity Framework Core viene spostata nello strumento di associazione di modelli. Ciò può rappresentare una semplificazione notevole quando vari metodi eseguono l'associazione al modello Author e favorisce l'osservanza del [principio DRY](http://deviq.com/don-t-repeat-yourself/) (Do not Repeat Yourself, Non ripeterti).

È possibile applicare l'attributo `ModelBinder` a singole proprietà del modello (ad esempio a ViewModel) o a parametri del metodo di azione per specificare un determinato strumento di associazione di modelli o nome di modello solo per quel determinato tipo o azione.

### <a name="implementing-a-modelbinderprovider"></a>Implementazione di un elemento ModelBinderProvider

Anziché applicare un attributo, è possibile implementare `IModelBinderProvider`. Gli strumenti di associazione del framework incorporati vengono implementati in questo modo. Quando si specifica il tipo sul quale opera lo strumento di associazione, si indica il tipo di argomento prodotto dallo strumento, **non** l'input accettato dallo strumento. Il provider strumento di associazione seguente funziona con l'elemento `AuthorEntityBinder`. Quando viene aggiunto alla raccolta di provider di MVC non è necessario usare l'attributo `ModelBinder` sui parametri tipizzati `Author` o `Author`.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Nota: il codice precedente restituisce un elemento `BinderTypeModelBinder`. `BinderTypeModelBinder` funziona come factory per gli strumenti di associazione di modelli e implementa la funzionalità DI (Dependency Injection, Inserimento di dipendenze). `AuthorEntityBinder` richiede DI (Dependency Injection, Inserimento di dipendenze) per l'accesso a EF Core. Usare `BinderTypeModelBinder` se lo strumento di associazione di modelli richiede servizi da DI (Dependency Injection, Inserimento di dipendenze).

Per usare un provider dello strumento di associazione di modelli personalizzato, aggiungerlo in `ConfigureServices`:

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Durante la valutazione degli strumenti di associazione di modelli, la raccolta di provider viene esaminata in ordine. Viene usato il primo provider che restituisce uno strumento di associazione.

La figura seguente visualizza gli strumenti di associazione di modelli predefiniti dal debugger.

![strumenti di associazione di modelli predefiniti](custom-model-binding/images/default-model-binders.png "strumenti di associazione di modelli predefiniti")

Se il provider personalizzato viene aggiunto alla fine della raccolta, è possibile che uno strumento di associazione di modelli incorporato venga chiamato prima dello strumento di associazione di modelli personalizzato. In questo esempio il provider personalizzato viene aggiunto all'inizio della raccolta, per garantire che venga usato per gli argomenti dell'azione `Author`.

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>Suggerimenti e procedure ottimali

Gli strumenti di associazione di modelli personalizzati:
- Non devono provare a impostare codici di stato o restituire risultati (ad esempio 404 Non trovato). Se si verifica un errore nell'associazione di modelli, l'errore deve essere gestito da un [filtro azioni](xref:mvc/controllers/filters) o da logica inclusa nel metodo di azione.
- Sono particolarmente utili per eliminare codice ripetitivo e problemi di montaggio incrociato dai metodi di azione.
- In genere non devono essere usati per convertire una stringa in un tipo personalizzato. Un elemento [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) rappresenta solitamente una scelta migliore.
