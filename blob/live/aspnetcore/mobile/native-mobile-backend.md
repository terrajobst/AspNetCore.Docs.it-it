---
title: Creazione di servizi back-end per applicazioni Native per dispositivi mobili
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3b6a32f2-5af9-4ede-9b7f-17ab300526d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: be1cd9f4fe41f1a79669975cb6a89439cdd9e5c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="creating-backend-services-for-native-mobile-applications"></a><span data-ttu-id="bfd17-103">Creazione di servizi back-end per applicazioni Native per dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="bfd17-103">Creating Backend Services for Native Mobile Applications</span></span>

<span data-ttu-id="bfd17-104">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="bfd17-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="bfd17-105">App per dispositivi mobili facilmente possono comunicare con servizi di back-end ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfd17-105">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="bfd17-106">Consente di visualizzare o scaricare l'esempio di codice di servizi back-end</span><span class="sxs-lookup"><span data-stu-id="bfd17-106">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="bfd17-107">L'App Mobile nativa di esempio</span><span class="sxs-lookup"><span data-stu-id="bfd17-107">The Sample Native Mobile App</span></span>

<span data-ttu-id="bfd17-108">In questa esercitazione viene illustrato come creare servizi back-end mediante ASP.NET MVC di base per supportare native per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="bfd17-108">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="bfd17-109">Usa il [app Xamarin Forms ToDoRest](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) come relativo native client, che include client nativi separati per i dispositivi Android, iOS, universali di Windows e Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="bfd17-109">It uses the [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="bfd17-110">È possibile seguire l'esercitazione collegato per creare l'app nativa (e installare gli strumenti di Xamarin liberi necessari), nonché download soluzione di esempio Xamarin.</span><span class="sxs-lookup"><span data-stu-id="bfd17-110">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="bfd17-111">Nell'esempio di Xamarin è incluso un progetto di servizi di ASP.NET Web API 2, che sostituisce app ASP.NET Core di questo articolo (con nessuna modifica richiesta dal client).</span><span class="sxs-lookup"><span data-stu-id="bfd17-111">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Applicazione di Rest si in esecuzione in un dispositivo Android smartphone](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="bfd17-113">Funzionalità</span><span class="sxs-lookup"><span data-stu-id="bfd17-113">Features</span></span>

<span data-ttu-id="bfd17-114">L'app ToDoRest supporta la visualizzazione, aggiunta, eliminazione e aggiornamento degli elementi di attività da eseguire.</span><span class="sxs-lookup"><span data-stu-id="bfd17-114">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="bfd17-115">Ogni elemento è un ID, un nome, le note e una proprietà che indica se è stata eseguita ancora.</span><span class="sxs-lookup"><span data-stu-id="bfd17-115">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="bfd17-116">La visualizzazione principale degli elementi, come illustrato in precedenza, viene elencato il nome di ogni elemento e indica se eseguita con un segno di spunta.</span><span class="sxs-lookup"><span data-stu-id="bfd17-116">The main view of the items, as shown above, lists each item's name and indicates if it is done with a checkmark.</span></span>

<span data-ttu-id="bfd17-117">Toccando il `+` icona verrà visualizzata una finestra di dialogo Aggiungi elemento:</span><span class="sxs-lookup"><span data-stu-id="bfd17-117">Tapping the `+` icon opens an add item dialog:</span></span>

![Aggiungere l'elemento finestra di dialogo](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="bfd17-119">Toccare un elemento sullo schermo principale elenco apre una finestra di dialogo di modifica in cui è possibile modificare nome, le note e completato le impostazioni dell'elemento o l'elemento può essere eliminato:</span><span class="sxs-lookup"><span data-stu-id="bfd17-119">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Elemento finestra di dialogo Modifica](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="bfd17-121">In questo esempio è configurato per impostazione predefinita per utilizzare i servizi back-end ospitati developer.xamarin.com, che consentono operazioni di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="bfd17-121">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="bfd17-122">Per eseguire il test viene prima persona l'applicazione ASP.NET di base creato nella sezione successiva in esecuzione nel computer, è necessario aggiornare l'app `RestUrl` costante.</span><span class="sxs-lookup"><span data-stu-id="bfd17-122">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="bfd17-123">Passare il `ToDoREST` del progetto e aprire il *Constants.cs* file.</span><span class="sxs-lookup"><span data-stu-id="bfd17-123">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="bfd17-124">Sostituire il `RestUrl` con un URL che include l'IP del computer indirizzo (non localhost o 127.0.0.1, poiché questo indirizzo viene utilizzato dall'emulatore di dispositivo, non dal computer in uso).</span><span class="sxs-lookup"><span data-stu-id="bfd17-124">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="bfd17-125">Includono anche il numero di porta (5000).</span><span class="sxs-lookup"><span data-stu-id="bfd17-125">Include the port number as well (5000).</span></span> <span data-ttu-id="bfd17-126">Per verificare che i servizi funzionino con un dispositivo, verificare che non si dispone di un firewall attive bloccando l'accesso a questa porta.</span><span class="sxs-lookup"><span data-stu-id="bfd17-126">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="bfd17-127">Creazione del progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bfd17-127">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="bfd17-128">Creare una nuova applicazione Web di ASP.NET Core in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bfd17-128">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="bfd17-129">Scegliere il modello API Web e Nessuna autenticazione.</span><span class="sxs-lookup"><span data-stu-id="bfd17-129">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="bfd17-130">Denominare il progetto *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="bfd17-130">Name the project *ToDoApi*.</span></span>

![Finestra di dialogo Nuovo applicazione Web ASP.NET con il modello di progetto API Web selezionato](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="bfd17-132">L'applicazione deve rispondere a tutte le richieste effettuate alla porta 5000.</span><span class="sxs-lookup"><span data-stu-id="bfd17-132">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="bfd17-133">Aggiornamento *Program.cs* includere `.UseUrls("http://*:5000")` per ottenere questo risultato:</span><span class="sxs-lookup"><span data-stu-id="bfd17-133">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="bfd17-134">Assicurarsi di eseguire l'applicazione direttamente, anziché dietro IIS Express, che consente di ignorare le richieste non locale per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="bfd17-134">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="bfd17-135">Eseguire `dotnet run` da un prompt dei comandi, oppure scegliere il profilo dell'applicazione nome nell'elenco a discesa destinazione Debug sulla barra degli strumenti di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bfd17-135">Run `dotnet run` from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="bfd17-136">Aggiungere una classe modello che rappresenti gli elementi di attività da eseguire.</span><span class="sxs-lookup"><span data-stu-id="bfd17-136">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="bfd17-137">Contrassegno richiesto campi utilizzando la `[Required]` attributo:</span><span class="sxs-lookup"><span data-stu-id="bfd17-137">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="bfd17-138">I metodi dell'API richiedono alcune modalità di utilizzo dei dati.</span><span class="sxs-lookup"><span data-stu-id="bfd17-138">The API methods require some way to work with data.</span></span> <span data-ttu-id="bfd17-139">Utilizzare lo stesso `IToDoRepository` interfaccia gli utilizzi di esempio originali di Xamarin:</span><span class="sxs-lookup"><span data-stu-id="bfd17-139">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="bfd17-140">Per questo esempio, l'implementazione utilizza solo una raccolta privata di elementi:</span><span class="sxs-lookup"><span data-stu-id="bfd17-140">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="bfd17-141">Configurare l'implementazione in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bfd17-141">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="bfd17-142">A questo punto, si è pronti a creare il *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="bfd17-142">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="bfd17-143">Ulteriori informazioni sulla creazione di web API in [compilazione del primo API Web con ASP.NET MVC di base e Visual Studio](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bfd17-143">Learn more about creating web APIs in [Building Your First Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="bfd17-144">Creazione del Controller</span><span class="sxs-lookup"><span data-stu-id="bfd17-144">Creating the Controller</span></span>

<span data-ttu-id="bfd17-145">Aggiunge un nuovo controller al progetto, *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="bfd17-145">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="bfd17-146">Eredita da Microsoft.AspNetCore.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="bfd17-146">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="bfd17-147">Aggiungere un `Route` attributo per indicare che il controller gestirà le richieste effettuate per i percorsi che iniziano con `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="bfd17-147">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="bfd17-148">Il `[controller]` token nella route viene sostituito dal nome del controller (omettendo il `Controller` suffisso) e risulta particolarmente utile per le route globale.</span><span class="sxs-lookup"><span data-stu-id="bfd17-148">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="bfd17-149">Altre informazioni, vedere [routing](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="bfd17-149">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="bfd17-150">Il controller è richiesto un `IToDoRepository` di funzione; richiedere un'istanza di questo tipo tramite il costruttore del controller.</span><span class="sxs-lookup"><span data-stu-id="bfd17-150">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="bfd17-151">In fase di esecuzione, questa istanza è disponibile tramite il supporto del framework per [inserimento di dipendenze](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="bfd17-151">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="bfd17-152">Questa API supporta quattro verbi HTTP diversi per eseguire operazioni CRUD (Create, Read, Update, Delete) nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="bfd17-152">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="bfd17-153">Il più semplice di questi è l'operazione di lettura, che corrisponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="bfd17-153">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="bfd17-154">Lettura degli elementi</span><span class="sxs-lookup"><span data-stu-id="bfd17-154">Reading Items</span></span>

<span data-ttu-id="bfd17-155">Richiesta di un elenco di elementi viene eseguita con una richiesta GET per il `List` metodo.</span><span class="sxs-lookup"><span data-stu-id="bfd17-155">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="bfd17-156">Il `[HttpGet]` attributo la `List` metodo indica che questa azione solo deve gestire le richieste GET.</span><span class="sxs-lookup"><span data-stu-id="bfd17-156">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="bfd17-157">La route per questa azione è la route specificata nel controller.</span><span class="sxs-lookup"><span data-stu-id="bfd17-157">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="bfd17-158">Non è necessario utilizzare il nome dell'azione come parte della route.</span><span class="sxs-lookup"><span data-stu-id="bfd17-158">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="bfd17-159">È sufficiente garantire che ogni azione di una route univoca e non ambigua.</span><span class="sxs-lookup"><span data-stu-id="bfd17-159">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="bfd17-160">Routing attributi possono essere applicati nel server di controller e i livelli di metodo per compilare i percorsi specifici.</span><span class="sxs-lookup"><span data-stu-id="bfd17-160">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="bfd17-161">Il `List` metodo restituisce un codice di 200 risposta OK e tutti gli elementi di attività, serializzati come JSON.</span><span class="sxs-lookup"><span data-stu-id="bfd17-161">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="bfd17-162">È possibile testare il nuovo metodo API utilizzando un'ampia gamma di strumenti, ad esempio [Postman](https://www.getpostman.com/docs/), illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="bfd17-162">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Console di postman che mostra una richiesta GET per todoitems e il corpo della risposta che mostra il JSON per tre elementi restituiti](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="bfd17-164">Creazione di elementi</span><span class="sxs-lookup"><span data-stu-id="bfd17-164">Creating Items</span></span>

<span data-ttu-id="bfd17-165">Per convenzione, creazione di nuovi elementi di dati viene eseguito il mapping per il verbo HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="bfd17-165">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="bfd17-166">Il `Create` metodo ha un `[HttpPost]` attributo applicato e accetta un `ToDoItem` istanza.</span><span class="sxs-lookup"><span data-stu-id="bfd17-166">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="bfd17-167">Poiché il `item` argomento verrà passato nel corpo del POST, questo parametro è decorato con il `[FromBody]` attributo.</span><span class="sxs-lookup"><span data-stu-id="bfd17-167">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="bfd17-168">All'interno del metodo, l'elemento è selezionato per la validità ed esistenza precedente nell'archivio dati, e se si verificano senza problemi, viene aggiunta tramite il repository.</span><span class="sxs-lookup"><span data-stu-id="bfd17-168">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it is added using the repository.</span></span> <span data-ttu-id="bfd17-169">Controllo `ModelState.IsValid` esegue [la convalida del modello](../mvc/models/validation.md)e deve essere eseguita in ogni metodo dell'API che accetta l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="bfd17-169">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="bfd17-170">L'esempio utilizza un'enumerazione che contiene i codici di errore vengono passati al client per dispositivi mobili:</span><span class="sxs-lookup"><span data-stu-id="bfd17-170">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="bfd17-171">Aggiunta di nuovi elementi tramite Postman scegliendo il verbo POST fornendo il nuovo oggetto in formato JSON nel corpo della richiesta di test.</span><span class="sxs-lookup"><span data-stu-id="bfd17-171">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="bfd17-172">È inoltre necessario aggiungere un'intestazione di richiesta specificando un `Content-Type` di `application/json`.</span><span class="sxs-lookup"><span data-stu-id="bfd17-172">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Console di postman che mostra una risposta e il POST](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="bfd17-174">Il metodo restituisce l'elemento appena creato nella risposta.</span><span class="sxs-lookup"><span data-stu-id="bfd17-174">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="bfd17-175">Aggiornamento degli elementi</span><span class="sxs-lookup"><span data-stu-id="bfd17-175">Updating Items</span></span>

<span data-ttu-id="bfd17-176">La modifica di record viene eseguita utilizzando richieste PUT HTTP.</span><span class="sxs-lookup"><span data-stu-id="bfd17-176">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="bfd17-177">Oltre a questa modifica, il `Edit` è pressoché identico al metodo `Create`.</span><span class="sxs-lookup"><span data-stu-id="bfd17-177">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="bfd17-178">Si noti che se il record non viene trovato, il `Edit` azione restituirà un `NotFound` risposta (404).</span><span class="sxs-lookup"><span data-stu-id="bfd17-178">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="bfd17-179">Per eseguire test con Postman, modificare il verbo PUT.</span><span class="sxs-lookup"><span data-stu-id="bfd17-179">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="bfd17-180">Specificare i dati dell'oggetto aggiornato nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="bfd17-180">Specify the updated object data in the Body of the request.</span></span>

![Console di postman che mostra una risposta e PUT](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="bfd17-182">Questo metodo restituisce un `NoContent` risposta (204) al termine, per coerenza con l'API preesistente.</span><span class="sxs-lookup"><span data-stu-id="bfd17-182">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="bfd17-183">Eliminazione di elementi</span><span class="sxs-lookup"><span data-stu-id="bfd17-183">Deleting Items</span></span>

<span data-ttu-id="bfd17-184">Eliminazione di record viene eseguita, effettua le richieste di eliminazione per il servizio e passando l'ID dell'elemento da eliminare.</span><span class="sxs-lookup"><span data-stu-id="bfd17-184">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="bfd17-185">Come con gli aggiornamenti, per gli elementi che non sono presenti richieste `NotFound` le risposte.</span><span class="sxs-lookup"><span data-stu-id="bfd17-185">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="bfd17-186">In caso contrario, si riceverà una richiesta con esito positivo un `NoContent` risposta (204).</span><span class="sxs-lookup"><span data-stu-id="bfd17-186">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="bfd17-187">Si noti che, durante il test la funzionalità di eliminazione, non è necessario nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="bfd17-187">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Console di postman che mostra una risposta e l'eliminazione](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="bfd17-189">Convenzioni comuni API Web</span><span class="sxs-lookup"><span data-stu-id="bfd17-189">Common Web API Conventions</span></span>

<span data-ttu-id="bfd17-190">Durante lo sviluppo di servizi back-end per l'app, è possibile richiamare con un set coerente di convenzioni o i criteri per la gestione dei problemi di montaggio incrociato.</span><span class="sxs-lookup"><span data-stu-id="bfd17-190">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="bfd17-191">Ad esempio, nel servizio illustrato in precedenza, le richieste di record specifici che sono stati trovati ha ricevuto un `NotFound` risposta, piuttosto che un `BadRequest` risposta.</span><span class="sxs-lookup"><span data-stu-id="bfd17-191">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="bfd17-192">Analogamente, i comandi apportati a questo servizio passati tipi di modello associato sempre archiviati `ModelState.IsValid` e restituito un `BadRequest` per tipi di modello non valido.</span><span class="sxs-lookup"><span data-stu-id="bfd17-192">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="bfd17-193">Dopo aver identificato un criterio comune per le API, è possibile incapsulare in genere in un [filtro](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="bfd17-193">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="bfd17-194">Altre informazioni, vedere [come incapsulare i criteri di API comuni nelle applicazioni ASP.NET MVC Core](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="bfd17-194">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>
