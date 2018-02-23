---
title: Creazione di servizi back-end per app native per dispositivi mobili
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mobile/native-mobile-backend
ms.openlocfilehash: ff09f331cff5cca7b42fa89bff55c0ed5c7d82f4
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2018
---
# <a name="creating-backend-services-for-native-mobile-applications"></a><span data-ttu-id="9db77-102">Creazione di servizi back-end per app native per dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="9db77-102">Creating Backend Services for Native Mobile Applications</span></span>

<span data-ttu-id="9db77-103">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9db77-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9db77-104">Le app per dispositivi mobili possono comunicare facilmente con i servizi back-end di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9db77-104">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="9db77-105">Visualizzare o scaricare codice di esempio di servizi back-end</span><span class="sxs-lookup"><span data-stu-id="9db77-105">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="9db77-106">App per dispositivi mobili nativa di esempio</span><span class="sxs-lookup"><span data-stu-id="9db77-106">The Sample Native Mobile App</span></span>

<span data-ttu-id="9db77-107">Questa esercitazione illustra come creare servizi back-end mediante ASP.NET Core MVC per il supporto di app per dispositivi mobili native.</span><span class="sxs-lookup"><span data-stu-id="9db77-107">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="9db77-108">Usa come client nativo l'[app ToDoRest di Xamarin.Forms](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/), che include client nativi separati per i dispositivi Android, iOS, Windows Universal e Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="9db77-108">It uses the [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="9db77-109">È possibile eseguire l'esercitazione collegata per creare l'app nativa (e installare gli strumenti Xamarin gratuiti necessari), nonché scaricare la soluzione di esempio di Xamarin.</span><span class="sxs-lookup"><span data-stu-id="9db77-109">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="9db77-110">L'esempio di Xamarin include un progetto di servizi ASP.NET Web API 2, che viene sostituito dall'app ASP.NET Core di questo articolo (senza richiedere nessuna modifica al client).</span><span class="sxs-lookup"><span data-stu-id="9db77-110">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Applicazione ToDoRest in esecuzione su uno smartphone Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="9db77-112">Funzionalità</span><span class="sxs-lookup"><span data-stu-id="9db77-112">Features</span></span>

<span data-ttu-id="9db77-113">L'app ToDoRest supporta l'aggiunta, l'eliminazione, l'aggiornamento e l'aggiunta a elenco di elementi attività.</span><span class="sxs-lookup"><span data-stu-id="9db77-113">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="9db77-114">Ogni elemento include un ID, un nome, delle note e una proprietà che indica se è stato eseguito.</span><span class="sxs-lookup"><span data-stu-id="9db77-114">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="9db77-115">La visualizzazione principale degli elementi, che appare nell'immagine in alto, contiene un elenco con il nome di ogni elemento. Un segno di spunta indica se l'elemento è stato eseguito.</span><span class="sxs-lookup"><span data-stu-id="9db77-115">The main view of the items, as shown above, lists each item's name and indicates if it's done with a checkmark.</span></span>

<span data-ttu-id="9db77-116">Se si tocca l'icona `+` viene visualizzata una finestra di dialogo per l'aggiunta di elementi:</span><span class="sxs-lookup"><span data-stu-id="9db77-116">Tapping the `+` icon opens an add item dialog:</span></span>

![Finestra di dialogo per l'aggiunta di elementi](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="9db77-118">Se si tocca un elemento sulla schermata dell'elenco principale viene visualizzata una finestra di dialogo di modifica in cui è possibile modificare le opzioni Name (Nome), Notes (Note) e Done (Fatto) dell'elemento oppure eliminare l'elemento stesso:</span><span class="sxs-lookup"><span data-stu-id="9db77-118">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Finestra di dialogo di modifica dell'elemento](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="9db77-120">Questo esempio è configurato per impostazione predefinita per l'uso dei servizi back-end ospitati in developer.xamarin.com, che consentono operazioni di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="9db77-120">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="9db77-121">Per eseguire direttamente il test nel proprio computer con l'app ASP.NET Core creata nella sezione successiva, è necessario modificare la costante `RestUrl` dell'app.</span><span class="sxs-lookup"><span data-stu-id="9db77-121">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="9db77-122">Passare al progetto `ToDoREST` e aprire il file *Constants.cs*.</span><span class="sxs-lookup"><span data-stu-id="9db77-122">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="9db77-123">Sostituire `RestUrl` con un URL che include l'indirizzo IP del computer (non localhost o 127.0.0.1, poiché questo indirizzo viene usato dall'emulatore di dispositivo e non dal computer in uso).</span><span class="sxs-lookup"><span data-stu-id="9db77-123">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="9db77-124">Includere anche il numero di porta (5000).</span><span class="sxs-lookup"><span data-stu-id="9db77-124">Include the port number as well (5000).</span></span> <span data-ttu-id="9db77-125">Per verificare che i servizi funzionino con un dispositivo, assicurarsi che l'accesso a questa porta non sia bloccato da un firewall attivo.</span><span class="sxs-lookup"><span data-stu-id="9db77-125">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="9db77-126">Creazione del progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9db77-126">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="9db77-127">Creare una nuova applicazione Web ASP.NET Core in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9db77-127">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="9db77-128">Scegliere il modello API Web e Nessuna autenticazione.</span><span class="sxs-lookup"><span data-stu-id="9db77-128">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="9db77-129">Denominare il progetto *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="9db77-129">Name the project *ToDoApi*.</span></span>

![Finestra di dialogo Nuova applicazione Web ASP.NET Core con il modello di progetto API Web selezionato](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="9db77-131">L'applicazione deve rispondere a tutte le richieste effettuate sulla porta 5000.</span><span class="sxs-lookup"><span data-stu-id="9db77-131">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="9db77-132">A tale scopo aggiornare *Program.cs* in modo che includa `.UseUrls("http://*:5000")`:</span><span class="sxs-lookup"><span data-stu-id="9db77-132">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="9db77-133">Assicurarsi di eseguire l'applicazione direttamente anziché dietro IIS Express, che per impostazione predefinita ignora le richieste non locali.</span><span class="sxs-lookup"><span data-stu-id="9db77-133">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="9db77-134">Eseguire `dotnet run` al prompt dei comandi oppure scegliere il profilo del nome dell'applicazione nell'elenco a discesa Destinazione di debug sulla barra degli strumenti di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9db77-134">Run `dotnet run` from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="9db77-135">Aggiungere una classe modello che rappresenti gli elementi attività.</span><span class="sxs-lookup"><span data-stu-id="9db77-135">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="9db77-136">Contrassegnare i campi obbligatori con l'attributo `[Required]`:</span><span class="sxs-lookup"><span data-stu-id="9db77-136">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="9db77-137">I metodi dell'API richiedono un approccio per l'uso dei dati.</span><span class="sxs-lookup"><span data-stu-id="9db77-137">The API methods require some way to work with data.</span></span> <span data-ttu-id="9db77-138">Usare la stessa interfaccia `IToDoRepository` usata nell'esempio originale di Xamarin:</span><span class="sxs-lookup"><span data-stu-id="9db77-138">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="9db77-139">Per questo esempio, l'implementazione usa solo una raccolta privata di elementi:</span><span class="sxs-lookup"><span data-stu-id="9db77-139">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="9db77-140">Configurare l'implementazione in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9db77-140">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="9db77-141">A questo punto si è pronti per creare *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="9db77-141">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="9db77-142">Per altre informazioni sulla creazione di API Web, vedere [Building Your First Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md) (Compilazione di un'API Web con ASP.NET Core MVC e Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="9db77-142">Learn more about creating web APIs in [Building Your First Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="9db77-143">Creazione del controller</span><span class="sxs-lookup"><span data-stu-id="9db77-143">Creating the Controller</span></span>

<span data-ttu-id="9db77-144">Aggiungere al progetto un nuovo controller, *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="9db77-144">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="9db77-145">Il controller deve ereditare da Microsoft.AspNetCore.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="9db77-145">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="9db77-146">Aggiungere un attributo `Route` per indicare che il controller gestirà le richieste inoltrate ai percorsi che iniziano con `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="9db77-146">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="9db77-147">Il token `[controller]` nella route viene sostituito dal nome del controller (omettendo il suffisso `Controller`) ed è particolarmente utile per le route globali.</span><span class="sxs-lookup"><span data-stu-id="9db77-147">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="9db77-148">Altre informazioni sul [routing](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="9db77-148">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="9db77-149">Per funzionare, il controller richiede un `IToDoRepository`. Richiedere un'istanza di questo tipo tramite il costruttore del controller.</span><span class="sxs-lookup"><span data-stu-id="9db77-149">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="9db77-150">In fase di esecuzione, questa istanza viene resa disponibile tramite il supporto dell'[inserimento di dipendenze](../fundamentals/dependency-injection.md) specificato dal framework.</span><span class="sxs-lookup"><span data-stu-id="9db77-150">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="9db77-151">Questa API supporta quattro verbi HTTP diversi per eseguire operazioni CRUD (Create, Read, Update, Delete - Creazione, Lettura, Aggiornamento, Eliminazione) nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="9db77-151">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="9db77-152">L'operazione più semplice tra queste è Read, che corrisponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="9db77-152">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="9db77-153">Lettura degli elementi</span><span class="sxs-lookup"><span data-stu-id="9db77-153">Reading Items</span></span>

<span data-ttu-id="9db77-154">La richiesta di un elenco di elementi viene eseguita con una richiesta GET al metodo `List`.</span><span class="sxs-lookup"><span data-stu-id="9db77-154">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="9db77-155">L'attributo `[HttpGet]` del metodo `List` indica che questa azione deve gestire solo le richieste GET.</span><span class="sxs-lookup"><span data-stu-id="9db77-155">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="9db77-156">La route di questa azione è la route specificata nel controller.</span><span class="sxs-lookup"><span data-stu-id="9db77-156">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="9db77-157">Non è necessario usare il nome dell'azione come parte della route.</span><span class="sxs-lookup"><span data-stu-id="9db77-157">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="9db77-158">È sufficiente garantire che ogni azione disponga di una route univoca e non ambigua.</span><span class="sxs-lookup"><span data-stu-id="9db77-158">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="9db77-159">Gli attributi di routing possono essere applicati sia a livello di controller che di metodo per definire route specifiche.</span><span class="sxs-lookup"><span data-stu-id="9db77-159">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="9db77-160">Il metodo `List` restituisce un codice di risposta 200 (OK) e tutti gli elementi di attività (ToDo), serializzati con formato JSON.</span><span class="sxs-lookup"><span data-stu-id="9db77-160">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="9db77-161">È possibile eseguire il test del nuovo metodo API usando un'ampia gamma di strumenti, ad esempio [Postman](https://www.getpostman.com/docs/), visualizzato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9db77-161">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Console di Postman con una richiesta GET per elementi di attività (todoitems) e il corpo della risposta con JSON per tre elementi restituiti](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="9db77-163">Creazione di elementi</span><span class="sxs-lookup"><span data-stu-id="9db77-163">Creating Items</span></span>

<span data-ttu-id="9db77-164">Per convenzione, la creazione di nuovi elementi di dati viene associata al verbo HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="9db77-164">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="9db77-165">Il metodo `Create` ha un attributo `[HttpPost]` applicato e accetta un'istanza `ToDoItem`.</span><span class="sxs-lookup"><span data-stu-id="9db77-165">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="9db77-166">Dato che l'argomento `item` viene passato nel corpo di POST, questo parametro è decorato con l'attributo `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="9db77-166">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="9db77-167">All'interno del metodo, l'elemento viene verificato per validità ed esistenza precedente nell'archivio dati. Se non si verificano problemi, l'elemento viene aggiunto tramite il repository.</span><span class="sxs-lookup"><span data-stu-id="9db77-167">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it's added using the repository.</span></span> <span data-ttu-id="9db77-168">La verifica `ModelState.IsValid` esegue la [convalida del modello](../mvc/models/validation.md) e deve essere eseguita in ogni metodo dell'API che accetta input utente.</span><span class="sxs-lookup"><span data-stu-id="9db77-168">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="9db77-169">L'esempio usa un'enumerazione contenente codici di errore che vengono passati al client per dispositivi mobili:</span><span class="sxs-lookup"><span data-stu-id="9db77-169">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="9db77-170">Eseguire il test dell'aggiunta di nuovi elementi con Postman scegliendo il verbo POST che rende disponibile il nuovo oggetto in formato JSON nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9db77-170">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="9db77-171">È anche necessario aggiungere un'intestazione di richiesta specificando come `Content-Type` la voce `application/json`.</span><span class="sxs-lookup"><span data-stu-id="9db77-171">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Console di Postman che visualizzano un POST e una risposta](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="9db77-173">Il metodo restituisce l'elemento appena creato nella risposta.</span><span class="sxs-lookup"><span data-stu-id="9db77-173">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="9db77-174">Aggiornamento degli elementi</span><span class="sxs-lookup"><span data-stu-id="9db77-174">Updating Items</span></span>

<span data-ttu-id="9db77-175">La modifica dei record viene eseguita mediante richieste PUT HTTP.</span><span class="sxs-lookup"><span data-stu-id="9db77-175">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="9db77-176">Fatta eccezione per questa modifica, il metodo `Edit` è quasi identico a `Create`.</span><span class="sxs-lookup"><span data-stu-id="9db77-176">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="9db77-177">Si noti che se il record non viene trovato, l'azione `Edit` restituisce una risposta `NotFound` (404).</span><span class="sxs-lookup"><span data-stu-id="9db77-177">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="9db77-178">Per eseguire test con Postman, impostare il verbo su PUT.</span><span class="sxs-lookup"><span data-stu-id="9db77-178">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="9db77-179">Specificare i dati dell'oggetto aggiornati nel Corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9db77-179">Specify the updated object data in the Body of the request.</span></span>

![Console di Postman con un elemento PUT e una risposta](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="9db77-181">Se va a buon fine, questo metodo restituisce una risposta `NoContent` (204) per coerenza con l'API preesistente.</span><span class="sxs-lookup"><span data-stu-id="9db77-181">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="9db77-182">Eliminazione di elementi</span><span class="sxs-lookup"><span data-stu-id="9db77-182">Deleting Items</span></span>

<span data-ttu-id="9db77-183">L'eliminazione di record si ottiene inviando richieste DELETE al servizio e passando l'ID dell'elemento da eliminare.</span><span class="sxs-lookup"><span data-stu-id="9db77-183">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="9db77-184">Come con gli aggiornamenti, le richieste relative a elementi che non esistono ricevono risposte `NotFound`.</span><span class="sxs-lookup"><span data-stu-id="9db77-184">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="9db77-185">Una richiesta con esito positivo riceve una risposta `NoContent` (204).</span><span class="sxs-lookup"><span data-stu-id="9db77-185">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="9db77-186">Si noti che durante il test della funzionalità di eliminazione non è necessario alcun elemento nella sezione Corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9db77-186">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Console di Postman con un'operazione DELETE e una risposta](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="9db77-188">Convenzioni comuni dell'API Web</span><span class="sxs-lookup"><span data-stu-id="9db77-188">Common Web API Conventions</span></span>

<span data-ttu-id="9db77-189">Durante lo sviluppo dei servizi back-end per l'app è necessario creare un set di convenzioni o criteri coerenti per la gestione dei problemi di montaggio incrociato.</span><span class="sxs-lookup"><span data-stu-id="9db77-189">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="9db77-190">Ad esempio nel servizio precedente le richieste di record specifici che non sono stati trovati ricevono una risposta `NotFound` anziché una risposta `BadRequest`.</span><span class="sxs-lookup"><span data-stu-id="9db77-190">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="9db77-191">Analogamente, i comandi inoltrati a questo servizio che trasmettono tipi associati al modello verificano sempre `ModelState.IsValid` e restituiscono `BadRequest` per i tipi di modello non validi.</span><span class="sxs-lookup"><span data-stu-id="9db77-191">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="9db77-192">Dopo aver identificato criteri comuni per le API è in genere possibile incapsulare tali criteri in un [filtro](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="9db77-192">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="9db77-193">Altre informazioni su [come incapsulare criteri comuni per le API nelle applicazioni ASP.NET Core MVC](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="9db77-193">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>
