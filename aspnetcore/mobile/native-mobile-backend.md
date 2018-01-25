---
title: Creazione di servizi back-end per applicazioni Native per dispositivi mobili
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: 2edb704ea6875e8aa70e79fe085cc0edbe0c9a55
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="creating-backend-services-for-native-mobile-applications"></a>Creazione di servizi back-end per applicazioni Native per dispositivi mobili

Da [Steve Smith](https://ardalis.com/)

App per dispositivi mobili facilmente possono comunicare con servizi di back-end ASP.NET Core.

[Consente di visualizzare o scaricare l'esempio di codice di servizi back-end](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>L'App Mobile nativa di esempio

In questa esercitazione viene illustrato come creare servizi back-end mediante ASP.NET MVC di base per supportare native per dispositivi mobili. Usa il [app Xamarin Forms ToDoRest](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) come relativo native client, che include client nativi separati per i dispositivi Android, iOS, universali di Windows e Windows Phone. È possibile seguire l'esercitazione collegato per creare l'app nativa (e installare gli strumenti di Xamarin liberi necessari), nonché download soluzione di esempio Xamarin. Nell'esempio di Xamarin è incluso un progetto di servizi di ASP.NET Web API 2, che sostituisce app ASP.NET Core di questo articolo (con nessuna modifica richiesta dal client).

![Applicazione di Rest si in esecuzione in un dispositivo Android smartphone](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>Funzionalità

L'app ToDoRest supporta la visualizzazione, aggiunta, eliminazione e aggiornamento degli elementi di attività da eseguire. Ogni elemento è un ID, un nome, le note e una proprietà che indica se è stata eseguita ancora.

La visualizzazione principale degli elementi, come illustrato in precedenza, viene elencato il nome di ogni elemento e indica se eseguita con un segno di spunta.

Toccando il `+` icona verrà visualizzata una finestra di dialogo Aggiungi elemento:

![Aggiungere l'elemento finestra di dialogo](native-mobile-backend/_static/todo-android-new-item.png)

Toccare un elemento sullo schermo principale elenco apre una finestra di dialogo di modifica in cui è possibile modificare nome, le note e completato le impostazioni dell'elemento o l'elemento può essere eliminato:

![Elemento finestra di dialogo Modifica](native-mobile-backend/_static/todo-android-edit-item.png)

In questo esempio è configurato per impostazione predefinita per utilizzare i servizi back-end ospitati developer.xamarin.com, che consentono operazioni di sola lettura. Per eseguire il test viene prima persona l'applicazione ASP.NET di base creato nella sezione successiva in esecuzione nel computer, è necessario aggiornare l'app `RestUrl` costante. Passare il `ToDoREST` del progetto e aprire il *Constants.cs* file. Sostituire il `RestUrl` con un URL che include l'IP del computer indirizzo (non localhost o 127.0.0.1, poiché questo indirizzo viene utilizzato dall'emulatore di dispositivo, non dal computer in uso). Includono anche il numero di porta (5000). Per verificare che i servizi funzionino con un dispositivo, verificare che non si dispone di un firewall attive bloccando l'accesso a questa porta.

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>Creazione del progetto ASP.NET Core

Creare una nuova applicazione Web di ASP.NET Core in Visual Studio. Scegliere il modello API Web e Nessuna autenticazione. Denominare il progetto *ToDoApi*.

![Finestra di dialogo Nuovo applicazione Web ASP.NET con il modello di progetto API Web selezionato](native-mobile-backend/_static/web-api-template.png)

L'applicazione deve rispondere a tutte le richieste effettuate alla porta 5000. Aggiornamento *Program.cs* includere `.UseUrls("http://*:5000")` per ottenere questo risultato:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> Assicurarsi di eseguire l'applicazione direttamente, anziché dietro IIS Express, che consente di ignorare le richieste non locale per impostazione predefinita. Eseguire `dotnet run` da un prompt dei comandi, oppure scegliere il profilo dell'applicazione nome nell'elenco a discesa destinazione Debug sulla barra degli strumenti di Visual Studio.

Aggiungere una classe modello che rappresenti gli elementi di attività da eseguire. Contrassegno richiesto campi utilizzando la `[Required]` attributo:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

I metodi dell'API richiedono alcune modalità di utilizzo dei dati. Utilizzare lo stesso `IToDoRepository` interfaccia gli utilizzi di esempio originali di Xamarin:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

Per questo esempio, l'implementazione utilizza solo una raccolta privata di elementi:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

Configurare l'implementazione in *Startup.cs*:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

A questo punto, si è pronti a creare il *ToDoItemsController*.

> [!TIP]
> Ulteriori informazioni sulla creazione di web API in [compilazione del primo API Web con ASP.NET MVC di base e Visual Studio](../tutorials/first-web-api.md).

## <a name="creating-the-controller"></a>Creazione del Controller

Aggiunge un nuovo controller al progetto, *ToDoItemsController*. Eredita da Microsoft.AspNetCore.Mvc.Controller. Aggiungere un `Route` attributo per indicare che il controller gestirà le richieste effettuate per i percorsi che iniziano con `api/todoitems`. Il `[controller]` token nella route viene sostituito dal nome del controller (omettendo il `Controller` suffisso) e risulta particolarmente utile per le route globale. Altre informazioni, vedere [routing](../fundamentals/routing.md).

Il controller è richiesto un `IToDoRepository` di funzione; richiedere un'istanza di questo tipo tramite il costruttore del controller. In fase di esecuzione, questa istanza è disponibile tramite il supporto del framework per [inserimento di dipendenze](../fundamentals/dependency-injection.md).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

Questa API supporta quattro verbi HTTP diversi per eseguire operazioni CRUD (Create, Read, Update, Delete) nell'origine dati. Il più semplice di questi è l'operazione di lettura, che corrisponde a una richiesta HTTP GET.

### <a name="reading-items"></a>Lettura degli elementi

Richiesta di un elenco di elementi viene eseguita con una richiesta GET per il `List` metodo. Il `[HttpGet]` attributo la `List` metodo indica che questa azione solo deve gestire le richieste GET. La route per questa azione è la route specificata nel controller. Non è necessario utilizzare il nome dell'azione come parte della route. È sufficiente garantire che ogni azione di una route univoca e non ambigua. Routing attributi possono essere applicati nel server di controller e i livelli di metodo per compilare i percorsi specifici.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

Il `List` metodo restituisce un codice di 200 risposta OK e tutti gli elementi di attività, serializzati come JSON.

È possibile testare il nuovo metodo API utilizzando un'ampia gamma di strumenti, ad esempio [Postman](https://www.getpostman.com/docs/), illustrato di seguito:

![Console di postman che mostra una richiesta GET per todoitems e il corpo della risposta che mostra il JSON per tre elementi restituiti](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>Creazione di elementi

Per convenzione, creazione di nuovi elementi di dati viene eseguito il mapping per il verbo HTTP POST. Il `Create` metodo ha un `[HttpPost]` attributo applicato e accetta un `ToDoItem` istanza. Poiché il `item` argomento verrà passato nel corpo del POST, questo parametro è decorato con il `[FromBody]` attributo.

All'interno del metodo, l'elemento è selezionato per la validità ed esistenza precedente nell'archivio dati, e se si verificano senza problemi, viene aggiunta tramite il repository. Controllo `ModelState.IsValid` esegue [la convalida del modello](../mvc/models/validation.md)e deve essere eseguita in ogni metodo dell'API che accetta l'input dell'utente.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

L'esempio utilizza un'enumerazione che contiene i codici di errore vengono passati al client per dispositivi mobili:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

Aggiunta di nuovi elementi tramite Postman scegliendo il verbo POST fornendo il nuovo oggetto in formato JSON nel corpo della richiesta di test. È inoltre necessario aggiungere un'intestazione di richiesta specificando un `Content-Type` di `application/json`.

![Console di postman che mostra una risposta e il POST](native-mobile-backend/_static/postman-post.png)

Il metodo restituisce l'elemento appena creato nella risposta.

### <a name="updating-items"></a>Aggiornamento degli elementi

La modifica di record viene eseguita utilizzando richieste PUT HTTP. Oltre a questa modifica, il `Edit` è pressoché identico al metodo `Create`. Si noti che se il record non viene trovato, il `Edit` azione restituirà un `NotFound` risposta (404).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Per eseguire test con Postman, modificare il verbo PUT. Specificare i dati dell'oggetto aggiornato nel corpo della richiesta.

![Console di postman che mostra una risposta e PUT](native-mobile-backend/_static/postman-put.png)

Questo metodo restituisce un `NoContent` risposta (204) al termine, per coerenza con l'API preesistente.

### <a name="deleting-items"></a>Eliminazione di elementi

Eliminazione di record viene eseguita, effettua le richieste di eliminazione per il servizio e passando l'ID dell'elemento da eliminare. Come con gli aggiornamenti, per gli elementi che non sono presenti richieste `NotFound` le risposte. In caso contrario, si riceverà una richiesta con esito positivo un `NoContent` risposta (204).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

Si noti che, durante il test la funzionalità di eliminazione, non è necessario nel corpo della richiesta.

![Console di postman che mostra una risposta e l'eliminazione](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Convenzioni comuni API Web

Durante lo sviluppo di servizi back-end per l'app, è possibile richiamare con un set coerente di convenzioni o i criteri per la gestione dei problemi di montaggio incrociato. Ad esempio, nel servizio illustrato in precedenza, le richieste di record specifici che sono stati trovati ha ricevuto un `NotFound` risposta, piuttosto che un `BadRequest` risposta. Analogamente, i comandi apportati a questo servizio passati tipi di modello associato sempre archiviati `ModelState.IsValid` e restituito un `BadRequest` per tipi di modello non valido.

Dopo aver identificato un criterio comune per le API, è possibile incapsulare in genere in un [filtro](../mvc/controllers/filters.md). Altre informazioni, vedere [come incapsulare i criteri di API comuni nelle applicazioni ASP.NET MVC Core](https://msdn.microsoft.com/magazine/mt767699.aspx).
