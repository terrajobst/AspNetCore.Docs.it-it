---
title: Associazione di modelli
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899763613a97
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/model-binding
ms.openlocfilehash: 597d4058a410e0b5991b1d5a74c9fc7bfe8171b8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="model-binding"></a>Associazione di modelli

Da [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Introduzione al modello di associazione

Associazione di modelli in ASP.NET MVC di base esegue il mapping dei dati dalle richieste HTTP ai parametri di metodo di azione. I parametri possono essere tipi semplici, ad esempio stringhe, interi o valori float o possono essere tipi complessi. In questo modo una grande funzionalità di MVC mapping di dati in ingresso a un equivalente è uno scenario spesso ripetuto, indipendentemente dalla dimensione o della complessità dei dati. MVC risolve questo problema, astrarre associazione in modo gli sviluppatori non sono necessario mantenere la riscrittura di una versione leggermente diversa di tale codice stesso in tutte le applicazioni. Scrittura del testo per digitare il codice del convertitore risulta difficile e soggetta a errori.

## <a name="how-model-binding-works"></a>Funzionamento di associazione del modello

Quando MVC riceve una richiesta HTTP, viene instradato a un metodo di azione specifica di un controller. Determina il metodo di azione da eseguire in base alle quali è nei dati di route, quindi associa i valori dalla richiesta HTTP per i parametri del metodo di azione. Si consideri ad esempio l'URL seguente:

`http://contoso.com/movies/edit/2`

Poiché il modello di route è simile al seguente, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` instrada il `Movies` controller e il relativo `Edit` metodo di azione. Tale metodo accetta inoltre un parametro facoltativo denominato `id`. Il codice per il metodo di azione dovrebbe essere simile al seguente:

<!-- literal_block {"ids": [], "linenos": true, "xml:space": "preserve", "language": "csharp"} -->

```csharp
public IActionResult Edit(int? id)
   ```

Nota: Le stringhe della route dell'URL non sono tra maiuscole e minuscole.

MVC tenterà di associare i dati di richiesta per i parametri dell'azione in base al nome. MVC cercherà i valori per ogni parametro utilizzando il nome di parametro e i nomi delle proprietà impostabili pubbliche. Nell'esempio precedente, il parametro sola azione è denominato `id`, MVC associa il valore con lo stesso nome nei valori di route. Oltre ai valori di route MVC assocerà dati da varie parti della richiesta ed esegue l'operazione in un determinato ordine. Di seguito è riportato un elenco delle origini dati nell'ordine in cui li esamina l'associazione di modelli:

1. `Form values`: Si tratta di valori del form che nella richiesta HTTP con il metodo POST. (incluse le richieste POST jQuery).

2. `Route values`: Il set di valori di route disponibili in [Routing](../../fundamentals/routing.md)

3. `Query strings`: La parte di stringa di query dell'URI.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Nota: Modulo valori, i dati di route e query tutte le stringhe vengono archiviate come coppie nome-valore.

Poiché l'associazione del modello richiesto per una chiave denominata `id` Nessun elemento denominato `id` nei valori del form, spostato per i valori della route per tale chiave. In questo esempio, è una corrispondenza. Si verifichi l'associazione e il valore viene convertito all'intero 2. La stessa richiesta mediante modifica (id di stringa) potrebbe convertire la stringa "2".

Nell'esempio viene utilizzato finora tipi semplici. In MVC tipi semplici sono qualsiasi tipo primitivo .NET o con un convertitore di tipi di stringa. Se il parametro del metodo di azione fosse una classe, ad esempio il `Movie` tipo, che contiene i tipi semplici e complessi come proprietà, modello associazione continuerà del MVC gestire correttamente. Usa la reflection e la ricorsione per attraversare le proprietà di ricerca di corrispondenze di tipi complessi. Esegue la ricerca per il modello di associazione di modelli *parameter_name.property_name* per associare i valori alle proprietà. Se i valori corrispondenti del modulo non viene trovato, verrà effettuato un tentativo di eseguire il binding utilizzando solo il nome della proprietà. Per i tipi, ad esempio `Collection` , tipi di associazione del modello esegue la ricerca di corrispondenze da *parameter_name [index]* o semplicemente *[index]*. Modello di associazione considera `Dictionary` tipi in modo analogo, che richiede il *parameter_name [key]* o semplicemente *[key]*, purché le chiavi sono tipi semplici. Le chiavi supportate corrispondono ai nomi di campo HTML e gli helper di tag generati per lo stesso tipo di modello. In questo modo i valori di andata e ritorno in modo che i campi modulo rimangano riempiti con l'input dell'utente per i motivi di praticità, ad esempio, quando i dati associati dalla creazione o modifica non ha superato la convalida.

In ordine di associazione consente di eseguire la classe deve avere un costruttore predefinito pubblico e membro sia associato deve essere pubblica proprietà accessibile in scrittura. Quando si verifica l'associazione di modelli che della classe sarà possibile creare istanze solo utilizzando il costruttore predefinito pubblico, è possono impostare le proprietà.

Quando è associato un parametro, l'associazione di modelli arresta cercando valori con lo stesso nome e lo sposta associare il parametro successivo. Se l'associazione non riesce, MVC genera un errore. È possibile eseguire query per gli errori di stato modello controllando il `ModelState.IsValid` proprietà.

Nota: Ogni voce del controller `ModelState` proprietà è un `ModelStateEntry` contenente un `Errors property`. È raramente la necessità di eseguire query in questa raccolta manualmente. In alternativa, usare `ModelState.IsValid` .

Esistono inoltre alcuni tipi di dati speciale che deve prendere in considerazione quando si esegue l'associazione del modello MVC:

* `IFormFile`, `IEnumerable<IFormFile>`: Uno o più file caricati che fanno parte della richiesta HTTP.

* `CancelationToken`: Usato per annullare l'attività in un controller asincrono.

Questi tipi possono essere associati ai parametri di azione o alle proprietà a un tipo di classe.

Dopo aver completato l'associazione di modelli [convalida](validation.md) si verifica. Associazione di modelli predefinito prestazioni ottimali per la maggior parte degli scenari di sviluppo. È inoltre estendibile in modo se si hanno esigenze specifiche, è possibile personalizzare il comportamento predefinito.

## <a name="customize-model-binding-behavior-with-attributes"></a>Personalizzare il comportamento di associazione del modello con gli attributi

MVC contiene numerosi attributi che è possibile utilizzare per indirizzare il comportamento di associazione del modello predefinito per un'origine diversa. Ad esempio, è possibile specificare se l'associazione è obbligatoria per una proprietà o se non deve mai accadere in qualsiasi tramite il `[BindRequired]` o `[BindNever]` attributi. In alternativa, è possibile sostituire l'origine dati predefinita e quindi specificare l'origine dati del gestore di associazione del modello. Di seguito è un elenco di attributi di associazione del modello:

* `[BindRequired]`: Questo attributo viene aggiunto un errore di stato del modello se è Impossibile eseguire l'associazione.

* `[BindNever]`: Indica il raccoglitore di modelli per non associare a questo parametro.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Utilizzarli per specificare l'origine di associazione esatto si desidera applicare.

* `[FromServices]`: Questo attributo utilizza [inserimento di dipendenze](../../fundamentals/dependency-injection.md) per associare i parametri di servizi.

* `[FromBody]`: Utilizzare i formattatori configurati per associare i dati dal corpo della richiesta. Il formattatore è selezionato in base al tipo di contenuto della richiesta.

* `[ModelBinder]`: Usato per sostituire il gestore di associazione del modello predefinito, origine dell'associazione e il nome.

Gli attributi sono strumenti molto utili quando è necessario eseguire l'override del comportamento predefinito di associazione del modello.

## <a name="binding-formatted-data-from-the-request-body"></a>Associazione dati formattati da un corpo della richiesta

Dati della richiesta sono disponibili in un'ampia gamma di formati quali JSON, XML e molti altri. Quando si utilizza l'attributo [FromBody] per indicare che si desidera associare un parametro per i dati nel corpo della richiesta, MVC utilizza un set di formattatori di cui è stato configurato per gestire i dati della richiesta in base al tipo di contenuto. Per impostazione predefinita, MVC include un `JsonInputFormatter` classe per la gestione di dati JSON, ma è possibile aggiungere altri formattatori per la gestione di XML e altri formati personalizzati.

> [!NOTE]
> Può esistere al massimo un parametro per ogni azione decorati con `[FromBody]`. Runtime di ASP.NET MVC Core delega la responsabilità di lettura del flusso di richiesta al formattatore. Una volta il flusso di richiesta è di lettura per un parametro, non è in genere possibile leggere il flusso di richiesta nuovamente per l'associazione altri `[FromBody]` parametri.

> [!NOTE]
> Il `JsonInputFormatter` è il formattatore predefinito e si basa su [Json.NET](https://www.newtonsoft.com/json).

Formattatori di input in base a viene selezionato il [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) intestazione e il tipo del parametro, a meno che non vi è un attributo applicato alla funzione che specifica in caso contrario. Se si desidera utilizzare il codice XML o un altro formato, è necessario configurare nel *Startup.cs* file, ma è prima necessario ottenere un riferimento a `Microsoft.AspNetCore.Mvc.Formatters.Xml` tramite NuGet. Il codice di avvio dovrebbe essere simile al seguente:

<!-- literal_block {"ids": [], "linenos": true, "xml:space": "preserve", "language": "csharp"} -->

```csharp
public void ConfigureServices(IServiceCollection services)
   {
       services.AddMvc()
          .AddXmlSerializerFormatters();
   }
   ```

Codice il *Startup.cs* file contiene un `ConfigureServices` metodo con un `services` argomento è possibile utilizzare per compilare i servizi per l'applicazione ASP.NET. Nell'esempio, si aggiungono un formattatore XML come un servizio che fornisce per questa applicazione MVC. Il `options` argomento passato il `AddMvc` metodo consente di aggiungere e gestire i filtri, i formattatori e altre opzioni di sistema da MVC dopo l'avvio delle app. Quindi applicare il `Consumes` attributo alle classi controller o ai metodi di azione per lavorare con il formato desiderato.

### <a name="custom-model-binding"></a>Associazione di modelli personalizzati

Scrivere la propria raccoglitori di modelli personalizzati, è possibile estendere l'associazione di modelli. Altre informazioni, vedere [l'associazione di modelli personalizzati](../advanced/custom-model-binding.md).
