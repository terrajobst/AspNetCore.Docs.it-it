---
title: Associazione di modelli in ASP.NET Core
author: rachelappel
description: Informazioni su come l'associazione di modelli in ASP.NET Core MVC esegue il mapping dei dati dalle richieste HTTP ai parametri dei metodi di azione.
manager: wpickett
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: rachelap
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/model-binding
ms.openlocfilehash: f416bda1d7bccdfa922ba598a411ef1d150e3111
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="model-binding-in-aspnet-core"></a>Associazione di modelli in ASP.NET Core

[Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Introduzione all'associazione di modelli

L'associazione di modelli in ASP.NET Core MVC esegue il mapping dei dati dalle richieste HTTP ai parametri dei metodi di azione. I parametri possono essere di tipo semplice, ad esempio stringa, integer o float, o di tipo complesso. Questa è una funzionalità di MVC eccezionale, poiché il mapping di dati in ingresso con una controparte è uno scenario che si ripete spesso, indipendentemente dalle dimensioni o dalla complessità dei dati. MVC risolve questo problema astraendo l'associazione. Gli sviluppatori non devono quindi continuare a scrivere una versione leggermente diversa dello stesso codice in ogni app. Scrivere testo personalizzato all'interno di codice convertitore di tipi è tedioso e soggetto a errori.

## <a name="how-model-binding-works"></a>Funzionamento dell'associazione di modelli

Quando MVC riceve una richiesta HTTP, la instrada a un metodo di azione specifico di un controller. Determina il metodo di azione da eseguire in base al contenuto dei dati della route e quindi associa i valori dalla richiesta HTTP ai parametri del metodo di azione stesso. Si consideri ad esempio l'URL seguente:

`http://contoso.com/movies/edit/2`

Poiché il modello di route è simile a `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` esegue l'instradamento al controller `Movies` e al relativo metodo di azione `Edit`. Accetta anche un parametro facoltativo denominato `id`. Il codice per il metodo di azione dovrebbe essere simile al seguente:

```csharp
public IActionResult Edit(int? id)
   ```

Nota: le stringhe nella route dell'URL non fanno distinzione tra maiuscole e minuscole.

MVC tenta di associare i dati della richiesta ai parametri dell'azione in base al nome. MVC cerca i valori per ogni parametro usando il nome del parametro e i nomi delle proprietà impostabili pubbliche. Nell'esempio precedente, l'unico parametro di azione, denominato `id`, viene associato da MVC al valore con lo stesso nome nei valori di route. Oltre ai valori di route, MVC associa dati da varie parti della richiesta ed esegue l'operazione in un ordine stabilito. Di seguito è riportato un elenco delle origini dati nell'ordine in cui vengono esaminate dall'associazione di modelli:

1. `Form values`: si tratta di valori di form inseriti nella richiesta HTTP con il metodo POST, incluse le richieste POST jQuery.

2. `Route values`: set di valori di route resi disponibili dal [routing](xref:fundamentals/routing)

3. `Query strings`: parte della stringa di query dell'URI.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Nota: i valori di form, i dati di route e le stringhe di query vengono tutti archiviati come coppie nome-valore.

Poiché l'associazione di modelli ha richiesto una chiave denominata `id` e nei valori del form nessun elemento è denominato `id`, l'associazione di modelli cerca tale chiave passando ai valori della route. In questo esempio, esiste una corrispondenza. L'associazione viene eseguita e il valore viene convertito nell'integer 2. Se nella stessa richiesta fosse usata l'espressione Edit(string id), il risultato della conversione sarebbe la stringa "2".

Finora l'esempio ha usato tipi semplici. In MVC sono considerati tipi semplici tutti i tipi primitivi .NET o i tipi con un convertitore di tipo stringa. Se il parametro del metodo di azione fosse una classe, ad esempio il tipo `Movie`, contenente sia tipi semplici che tipi complessi come proprietà, l'associazione di modelli di MVC sarebbe comunque in grado di gestirlo correttamente, usando la reflection e la ricorsione per cercare corrispondenze attraverso le proprietà corrispondenti ai tipi complessi. Per associare i valori alle proprietà, l'associazione di modelli cerca il modello *nome_parametro.nome_proprietà*. Se non trova valori corrispondenti nel form, tenta di eseguire l'associazione usando solo il nome della proprietà. Per i tipi quali `Collection`, l'associazione di modelli cerca le corrispondenze in base a *nome_parametro[indice]* o semplicemente *[indice]*. L'associazione di modelli considera i tipi `Dictionary` in modo analogo, richiedendo *nome_parametro[chiave]* o semplicemente *[chiave]*, purché le chiavi siano di tipo semplice. Le chiavi supportate corrispondono ai nomi di campo HTML e agli helper tag generati per lo stesso tipo di modello. Ciò consente l'uso di valori di andata e ritorno, in modo che i campi modulo rimangano compilati con l'input dell'utente per praticità, ad esempio quando i dati associati provenienti da un'istruzione di creazione o modifica non superano la convalida.

Perché l'associazione possa verificarsi, la classe deve avere un costruttore predefinito pubblico e i membri da associare devono essere proprietà scrivibili pubbliche. Quando si verifica l'associazione di modelli, sarà possibile creare istanze della classe solo usando il costruttore predefinito pubblico. Sarà quindi possibile impostare le proprietà.

Quando un parametro viene associato, l'associazione di modelli interrompe la ricerca di valori con quel nome e passa ad associare il parametro successivo. In caso contrario, il comportamento predefinito dell'associazione di modelli imposta i parametri sui valori predefiniti di questi a seconda del loro tipo:

* `T[]`: con l'eccezione di matrici di tipo `byte[]`, l'associazione imposta i parametri di tipo `T[]` su `Array.Empty<T>()`. Le matrici di tipo `byte[]` vengono impostate su `null`.

* Tipi riferimento: l'associazione crea un'istanza di una classe con il costruttore predefinito senza impostare proprietà. L'associazione di modelli, tuttavia, imposta i parametri `string` su `null`.

* Tipi nullable: i tipi nullable vengono impostati su `null`. Nell'esempio precedente, l'associazione di modelli imposta `id` su `null`, perché è di tipo `int?`.

* Tipi: valore: i tipi valore non-nullable di tipo `T` vengono impostati su `default(T)`. L'associazione di modelli, ad esempio, imposta un parametro `int id` su 0. Anziché basarsi sui valori predefiniti, è consigliabile usare la convalida del modello o tipi nullable.

Se l'associazione non riesce, MVC non genera un errore. Tutte le azioni che accettano input utente devono controllare la proprietà `ModelState.IsValid`.

Nota: ogni voce della proprietà `ModelState` del controller è una `ModelStateEntry` contenente una proprietà `Errors`. È raro che sia necessario eseguire query in questa raccolta personalmente. In alternativa, usare `ModelState.IsValid`.

Esistono anche alcuni tipi di dati speciali che MVC deve prendere in considerazione quando esegue l'associazione di modelli:

* `IFormFile`, `IEnumerable<IFormFile>`: uno o più file caricati che fanno parte della richiesta HTTP.

* `CancellationToken`: usato per annullare l'attività nei controller asincroni.

Questi tipi possono essere associati a parametri di azione o a proprietà per un tipo classe.

Quando l'associazione di modelli è completata viene eseguita la [convalida](validation.md). L'associazione di modelli predefinita è perfetta per la grande maggioranza degli scenari di sviluppo. È anche estendibile. In caso di esigenze molto particolari, quindi, è possibile personalizzare il comportamento predefinito.

## <a name="customize-model-binding-behavior-with-attributes"></a>Personalizzare il comportamento dell'associazione di modelli con attributi

MVC contiene diversi attributi che è possibile usare per indirizzare il comportamento predefinito dell'associazione di modelli verso un'origine diversa. È ad esempio possibile specificare se per una proprietà l'associazione è obbligatoria o se non deve mai essere eseguita tramite l'attributo `[BindRequired]` o `[BindNever]`, rispettivamente. In alternativa, è possibile eseguire l'override dell'origine dati predefinita e specificare l'origine dati dello strumento di associazione di modelli. Di seguito è riportato un elenco di attributi di associazione di modelli:

* `[BindRequired]`: se non è possibile eseguire l'associazione, questo attributo aggiunge un errore dello stato del modello.

* `[BindNever]`: indica allo strumento di associazione di modelli di non eseguire mai associazioni al parametro in questione.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: usare questi attributi per specificare l'origine di associazione precisa che si vuole applicare.

* `[FromServices]`: questo attributo usa l'[inserimento di dipendenze](../../fundamentals/dependency-injection.md) per associare parametri da servizi.

* `[FromBody]`: usare i formattatori configurati per associare dati dal corpo della richiesta. Il formattatore viene selezionato in base al tipo di contenuto della richiesta.

* `[ModelBinder]`: usato per eseguire l'override dello strumento di associazione di modelli, dell'origine di associazione e del nome predefiniti.

Gli attributi sono strumenti molto utili quando è necessario eseguire l'override del comportamento predefinito dell'associazione di modelli.

## <a name="bind-formatted-data-from-the-request-body"></a>Associare dati formattati dal corpo della richiesta

I dati delle richieste possono avere un'ampia gamma di formati, ad esempio JSON, XML e molti altri. Quando si usa l'attributo [FromBody] per indicare che si vuole associare un parametro ai dati nel corpo della richiesta, MVC usa un set di formattatori configurato per gestire i dati della richiesta in base al tipo di contenuto di questa. Per impostazione predefinita, MVC include una classe `JsonInputFormatter` per la gestione dei dati JSON, ma è possibile aggiungere altri formattatori per la gestione del formato XML e di altri formati personalizzati.

> [!NOTE]
> Un solo parametro per ogni azione al massimo può essere decorato con `[FromBody]`. Il runtime di ASP.NET Core MVC delega la responsabilità della lettura del flusso di richiesta al formattatore. Dopo che il flusso di richiesta è stato letto per un parametro, non è in genere possibile leggerlo nuovamente per l'associazione di altri parametri `[FromBody]`.

> [!NOTE]
> `JsonInputFormatter`, il formattatore predefinito, si basa su [Json.NET](https://www.newtonsoft.com/json).

ASP.NET seleziona i formattatori di input in base all'intestazione [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) e al tipo del parametro, a meno che un attributo applicato ad esso non specifichi altrimenti. Se si vuole usare codice XML o in un altro formato, è necessario eseguire la configurazione corrispondente nel file *Startup.cs*, ma prima può essere necessario ottenere un riferimento a `Microsoft.AspNetCore.Mvc.Formatters.Xml` tramite NuGet. Il codice di avvio dovrebbe apparire come segue:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

Il codice nel file *Startup.cs* contiene un metodo `ConfigureServices` con un argomento `services` che è possibile usare per compilare i servizi per l'app ASP.NET. Nell'esempio viene aggiunto un formattatore XML come servizio offerto da MVC per questa app. L'argomento `options` passato al metodo `AddMvc` consente di aggiungere e gestire filtri, formattatori e altre opzioni di sistema da MVC dopo l'avvio dell'app. Applicare quindi l'attributo `Consumes` alle classi controller o ai metodi di azione per usare il formato voluto.

### <a name="custom-model-binding"></a>Associazione di modelli personalizzata

È possibile estendere l'associazione di modelli scrivendo strumenti di associazione di modelli personalizzati. Altre informazioni sull'[associazione di modelli personalizzata](../advanced/custom-model-binding.md).
