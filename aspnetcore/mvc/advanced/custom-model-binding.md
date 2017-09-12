---
title: Associazione di modelli personalizzati
author: ardalis
description: Personalizzazione di associazione del modello in ASP.NET MVC di base.
keywords: ASP.NET Core, l'associazione di modelli, Raccoglitore di modelli personalizzati
ms.author: riande
manager: wpickett
ms.date: 4/10/2017
ms.topic: article
ms.assetid: ebd98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 2b95073bc0972908d0c0b2158a036e4374c7df4d
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="custom-model-binding"></a>Associazione di modelli personalizzati

Da [Steve Smith](https://ardalis.com/)

Associazione di modelli consente di azioni del controller lavorare direttamente con i tipi di modello (passati come argomenti dei metodi), invece di richieste HTTP. Mapping tra i modelli di data e l'applicazione richiesta in ingresso sono gestite da gestori di associazione del modello. Gli sviluppatori possono estendere la funzionalità di associazione di modelli predefiniti implementando raccoglitori di modelli personalizzati (ma in genere, non è necessario scrivere un provider personalizzato).

[Consente di visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Limitazioni di gestore di associazione del modello predefinito

I raccoglitori di modelli predefiniti supportano la maggior parte dei comuni tipi di dati .NET Core e devono soddisfare le esigenze di maggior parte degli sviluppatori. È prevista l'associazione basata su testo di input dalla richiesta direttamente a tipi di modello. Potrebbe essere necessario trasformare l'input prima dell'associazione. Ad esempio, quando si dispone di una chiave che può essere usata per cercare i dati del modello. È possibile utilizzare un raccoglitore di modelli personalizzati per recuperare i dati in base alla chiave.

## <a name="model-binding-review"></a>Esame del modello di associazione

Associazione di modelli utilizza le definizioni specifiche per i tipi su che opera. Oggetto *tipo semplice* viene convertito da una singola stringa di input. Oggetto *tipo complesso* viene convertito da più valori di input. Il framework determina la differenza in base alla presenza di un `TypeConverter`. È consigliabile creare un convertitore di tipi se si dispone di una semplice `string`  ->  `SomeType` mapping che non richiedono risorse esterne.

Prima di creare il propria Raccoglitore di modelli personalizzati, è opportuno esaminare i risultati del modello esistente come vengono implementati i gestori di associazione. Si consideri il [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) che consente di convertire le stringhe con codifica base64 in matrici di byte. Le matrici di byte vengono spesso archiviate come file o i campi BLOB del database.

### <a name="working-with-the-bytearraymodelbinder"></a>Utilizzo di ByteArrayModelBinder

Per rappresentare i dati binari, è possono utilizzare le stringhe con codifica Base64. Nell'immagine seguente, ad esempio, può essere codificato come stringa.

![dotnet bot](custom-model-binding/images/bot.png "bot dotnet")

Una piccola parte della stringa con codifica è illustrata nella figura seguente:

![bot dotnet codificato](custom-model-binding/images/encoded-bot.png "bot dotnet con codificato")

Seguire le istruzioni di [Leggimi dell'esempio](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) per convertire la stringa con codifica base64 in un file.

ASP.NET MVC di base può accettare una stringa con codifica base64 e utilizzare un `ByteArrayModelBinder` per convertirlo in una matrice di byte. Il [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) che implementa l'interfaccia [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mappe `byte[]` argomenti `ByteArrayModelBinder`:

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

Quando si crea il propria Raccoglitore di modelli personalizzati, è possibile implementare la propria `IModelBinderProvider` digitare oppure utilizzare il [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).

Nell'esempio seguente viene illustrato come utilizzare `ByteArrayModelBinder` per convertire una stringa con codifica base64 in un `byte[]` e salvare il risultato in un file:

[!code-csharp[Principale](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

È possibile inviare una stringa con codifica base64 a questo metodo api usando uno strumento come [Postman](https://www.getpostman.com/):

![postman](custom-model-binding/images/postman.png "postman")

Fino a quando il gestore di associazione è possibile associare dati di richiesta alle proprietà denominate in modo appropriato o argomenti, l'associazione di modelli avrà esito positivo. Nell'esempio seguente viene illustrato come utilizzare `ByteArrayModelBinder` con un modello di visualizzazione:

[!code-csharp[Principale](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Esempio di Raccoglitore di modelli personalizzati

In questa sezione verrà implementato un raccoglitore di modelli personalizzati che:

- Converte i dati della richiesta in ingresso in argomenti chiavi fortemente tipizzati.
- Usa Entity Framework Core per recuperare l'entità associata.
- Passa l'entità associata come argomento al metodo di azione.

L'esempio seguente usa il `ModelBinder` attributo di `Author` modello:

[!code-csharp[Principale](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

Nel codice precedente, il `ModelBinder` attributo specifica il tipo di `IModelBinder` che deve essere utilizzata per associare `Author` parametri dell'azione. 

Il `AuthorEntityBinder` viene utilizzata per associare un `Author` parametro mediante il recupero dell'entità da un'origine dati tramite Entity Framework Core e un `authorId`:

[!code-csharp[Principale](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

Il codice seguente viene illustrato come utilizzare il `AuthorEntityBinder` in un metodo di azione:

[!code-csharp[Principale](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

Il `ModelBinder` attributo può essere utilizzato per applicare il `AuthorEntityBinder` ai parametri che non utilizzano convenzioni predefinite:

[!code-csharp[Principale](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

In questo esempio, poiché il nome dell'argomento non è il valore predefinito `authorId`, viene specificato nel parametro usando `ModelBinder` attributo. Si noti che il controller e l'azione metodo sono semplificati rispetto alla ricerca di entità nel metodo di azione. La logica per recuperare l'autore usando Entity Framework Core viene spostata lo strumento di associazione del modello. Può trattarsi di una notevole semplificazione, quando si dispongono di diversi metodi che associare al modello di autore e può essere utile per seguire il [principio secca](http://deviq.com/don-t-repeat-yourself/).

È possibile applicare il `ModelBinder` attributo alle proprietà del modello singolo (ad esempio in un elemento viewmodel) o ai parametri di metodo di azione per specificare un determinato raccoglitore di modelli o nome del modello per solo quel tipo o l'azione.

### <a name="implementing-a-modelbinderprovider"></a>Implementazione di un ModelBinderProvider

Anziché applicare un attributo, è possibile implementare `IModelBinderProvider`. Questo è l'implementazione dei gestori di associazione del framework incorporato. Quando si specifica il tipo del gestore di associazione opera su, specificare il tipo di argomento, viene generato, **non** accetta il gestore di associazione di input. Il provider del gestore di associazione seguente funziona con il `AuthorEntityBinder`. Quando viene aggiunto alla raccolta di MVC di provider, non è necessario utilizzare il `ModelBinder` attributo `Author` o `Author` parametri tipizzati.

[!code-csharp[Principale](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Nota: Il codice precedente restituisce un `BinderTypeModelBinder`. `BinderTypeModelBinder`funge da una factory per raccoglitori di modelli e fornisce l'inserimento di dipendenze (DI). Il `AuthorEntityBinder` richiede DI accedere EF Core. Utilizzare `BinderTypeModelBinder` se il gestore di associazione del modello richiede servizi DI.

Per utilizzare un provider di strumento di associazione del modello personalizzato, aggiungerlo in `ConfigureServices`:

[!code-csharp[Principale](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Quando si valuta raccoglitori di modelli, la raccolta di provider viene esaminata in ordine. Viene utilizzato il primo provider che restituisce uno strumento di associazione.

La figura seguente mostra il valore predefinito raccoglitori dal debugger.

![raccoglitori di modelli predefinito](custom-model-binding/images/default-model-binders.png "predefinito raccoglitori di modelli")

Aggiunta alla fine della raccolta di provider può comportare un raccoglitore di modelli predefiniti viene chiamato prima che il gestore di associazione personalizzato con una possibilità. In questo esempio, il provider personalizzato viene aggiunto all'inizio della raccolta per assicurare venga utilizzato per `Author` argomenti dell'azione.

[!code-csharp[Principale](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>Indicazioni e procedure consigliate

Raccoglitori di modelli personalizzati:
- Non tentare di impostare i codici di stato o restituire risultati (ad esempio 404 non trovato). Se si verifica un errore di associazione del modello, un [filtro azione](xref:mvc/controllers/filters) o logica all'interno del metodo di azione stesso deve gestire l'errore.
- Sono particolarmente utili per eliminare codice ripetitivo e problemi di montaggio incrociato da metodi di azione.
- In genere non devono essere utilizzate per convertire una stringa in un tipo personalizzato, un [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) in genere è un'opzione migliore.
