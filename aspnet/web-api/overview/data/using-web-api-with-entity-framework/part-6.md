---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Creare il Client JavaScript | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b397c5a413ae213c9b79da1c0e0626efe21c7e21
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="create-the-javascript-client"></a><span data-ttu-id="1475e-102">Creare il Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="1475e-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="1475e-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1475e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1475e-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="1475e-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="1475e-105">In questa sezione si creerà il client per l'applicazione, in formato HTML, JavaScript e [Knockout.js](http://knockoutjs.com/) libreria.</span><span class="sxs-lookup"><span data-stu-id="1475e-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="1475e-106">Verrà creata l'app client in varie fasi:</span><span class="sxs-lookup"><span data-stu-id="1475e-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="1475e-107">Visualizzazione di un elenco di libri.</span><span class="sxs-lookup"><span data-stu-id="1475e-107">Showing a list of books.</span></span>
- <span data-ttu-id="1475e-108">Visualizzazione dei dettagli un libro.</span><span class="sxs-lookup"><span data-stu-id="1475e-108">Showing a book detail.</span></span>
- <span data-ttu-id="1475e-109">Aggiunta di un libro di nuovo.</span><span class="sxs-lookup"><span data-stu-id="1475e-109">Adding a new book.</span></span>

<span data-ttu-id="1475e-110">La libreria Knockout utilizza il modello Model-View-ViewModel (MVVM):</span><span class="sxs-lookup"><span data-stu-id="1475e-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="1475e-111">Il **modello** è la rappresentazione sul lato server dei dati del dominio di business (in questo caso documentazione e gli autori).</span><span class="sxs-lookup"><span data-stu-id="1475e-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="1475e-112">Il **visualizzazione** è il livello di presentazione (HTML).</span><span class="sxs-lookup"><span data-stu-id="1475e-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="1475e-113">Il **modello di visualizzazione** è un oggetto JavaScript che contiene i modelli.</span><span class="sxs-lookup"><span data-stu-id="1475e-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="1475e-114">Il modello di visualizzazione è un'astrazione del codice dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="1475e-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="1475e-115">Ha alcuna conoscenza della rappresentazione HTML.</span><span class="sxs-lookup"><span data-stu-id="1475e-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="1475e-116">Rappresenta invece astratta funzionalità della visualizzazione, ad esempio &quot;un elenco di libri&quot;.</span><span class="sxs-lookup"><span data-stu-id="1475e-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="1475e-117">La vista con associazione dati per il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1475e-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="1475e-118">Gli aggiornamenti al modello di visualizzazione vengono applicati automaticamente nella vista.</span><span class="sxs-lookup"><span data-stu-id="1475e-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="1475e-119">Il modello di visualizzazione ottiene anche gli eventi dalla visualizzazione, ad esempio, fa clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="1475e-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="1475e-120">Questo approccio rende facile modificare il layout e l'interfaccia utente dell'applicazione, poiché è possibile modificare le associazioni, senza riscrivere il codice.</span><span class="sxs-lookup"><span data-stu-id="1475e-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="1475e-121">Ad esempio, si potrebbe visualizzare un elenco di elementi come un `<ul>`, quindi cambiarlo in seguito a una tabella.</span><span class="sxs-lookup"><span data-stu-id="1475e-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="1475e-122">Aggiungere la libreria di ritaglio</span><span class="sxs-lookup"><span data-stu-id="1475e-122">Add the Knockout Library</span></span>

<span data-ttu-id="1475e-123">In Visual Studio, dal **strumenti** dal menu **Gestione pacchetti libreria**.</span><span class="sxs-lookup"><span data-stu-id="1475e-123">In Visual Studio, from the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="1475e-124">Selezionare quindi **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="1475e-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="1475e-125">Nella finestra della Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1475e-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="1475e-126">Questo comando aggiunge i file Knockout alla cartella degli script.</span><span class="sxs-lookup"><span data-stu-id="1475e-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="1475e-127">Creare il modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="1475e-127">Create the View Model</span></span>

<span data-ttu-id="1475e-128">Aggiungere un file JavaScript denominato app.js alla cartella degli script.</span><span class="sxs-lookup"><span data-stu-id="1475e-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="1475e-129">(In Esplora soluzioni fare doppio clic sulla cartella script, selezionare **Aggiungi**, quindi selezionare **JavaScript File**.) Incollare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1475e-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="1475e-130">In, Knockout la `observable` classe consente l'associazione dati.</span><span class="sxs-lookup"><span data-stu-id="1475e-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="1475e-131">Quando cambia il contenuto di un oggetto observable, observable notifica a tutti i controlli con associazione a dati, in modo che possano aggiornare autonomamente.</span><span class="sxs-lookup"><span data-stu-id="1475e-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="1475e-132">(Il `observableArray` classe è la versione della matrice di *observable*.) Per iniziare, il modello di visualizzazione dispone di due oggetti osservabili:</span><span class="sxs-lookup"><span data-stu-id="1475e-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="1475e-133">`books`contiene l'elenco di libri.</span><span class="sxs-lookup"><span data-stu-id="1475e-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="1475e-134">`error`contiene un messaggio di errore se ha esito negativo di una chiamata AJAX.</span><span class="sxs-lookup"><span data-stu-id="1475e-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="1475e-135">Il `getAllBooks` metodo effettua una chiamata AJAX per ottenere l'elenco di libri.</span><span class="sxs-lookup"><span data-stu-id="1475e-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="1475e-136">Quindi inserisce il risultato nel `books` matrice.</span><span class="sxs-lookup"><span data-stu-id="1475e-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="1475e-137">Il `ko.applyBindings` metodo fa parte della libreria di ritaglio.</span><span class="sxs-lookup"><span data-stu-id="1475e-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="1475e-138">Accetta come parametro, il modello di visualizzazione e impostare l'associazione dati.</span><span class="sxs-lookup"><span data-stu-id="1475e-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="1475e-139">Aggiungere un Bundle di Script</span><span class="sxs-lookup"><span data-stu-id="1475e-139">Add a Script Bundle</span></span>

<span data-ttu-id="1475e-140">Creazione di bundle è una funzionalità in ASP.NET 4.5 che rende più semplice combinare o aggregare più file in un singolo file.</span><span class="sxs-lookup"><span data-stu-id="1475e-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="1475e-141">Creazione di bundle riduce il numero di richieste al server, che consente di migliorare il tempo di caricamento pagina.</span><span class="sxs-lookup"><span data-stu-id="1475e-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="1475e-142">Aprire il file App\_Start/BundleConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="1475e-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="1475e-143">Aggiungere il codice seguente al metodo RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="1475e-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

>[!div class="step-by-step"]
<span data-ttu-id="1475e-144">[Precedente](part-5.md)
[Successivo](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="1475e-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
