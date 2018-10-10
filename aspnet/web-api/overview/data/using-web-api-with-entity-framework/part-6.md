---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Creare il Client JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b5cb4d93c30ef80a48da48ffc51dd51411b1d0d0
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912661"
---
<a name="create-the-javascript-client"></a><span data-ttu-id="b01c6-102">Creare il Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b01c6-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="b01c6-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b01c6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b01c6-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="b01c6-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="b01c6-105">In questa sezione si creerà il client per l'applicazione, usando HTML, JavaScript e il [Knockout. js](http://knockoutjs.com/) libreria.</span><span class="sxs-lookup"><span data-stu-id="b01c6-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="b01c6-106">Verrà compilata l'app client in più fasi:</span><span class="sxs-lookup"><span data-stu-id="b01c6-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="b01c6-107">Che mostra un elenco di libri.</span><span class="sxs-lookup"><span data-stu-id="b01c6-107">Showing a list of books.</span></span>
- <span data-ttu-id="b01c6-108">Visualizzazione dei dettagli un libro.</span><span class="sxs-lookup"><span data-stu-id="b01c6-108">Showing a book detail.</span></span>
- <span data-ttu-id="b01c6-109">Aggiunta di un nuovo libro.</span><span class="sxs-lookup"><span data-stu-id="b01c6-109">Adding a new book.</span></span>

<span data-ttu-id="b01c6-110">La libreria Knockout Usa il modello Model-View-ViewModel (MVVM):</span><span class="sxs-lookup"><span data-stu-id="b01c6-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="b01c6-111">Il **modello** è la rappresentazione lato server dei dati nel dominio dell'azienda (nel nostro caso, libri e autori).</span><span class="sxs-lookup"><span data-stu-id="b01c6-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="b01c6-112">Il **vista** è il livello di presentazione (HTML).</span><span class="sxs-lookup"><span data-stu-id="b01c6-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="b01c6-113">Il **modello di visualizzazione** è un oggetto JavaScript che contiene i modelli.</span><span class="sxs-lookup"><span data-stu-id="b01c6-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="b01c6-114">Il modello di visualizzazione è un'astrazione di codice dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="b01c6-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="b01c6-115">Dispone di informazioni relative alla relativa rappresentazione in HTML.</span><span class="sxs-lookup"><span data-stu-id="b01c6-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="b01c6-116">Rappresenta invece uno funzionalità astratta della visualizzazione, ad esempio &quot;un elenco di libri&quot;.</span><span class="sxs-lookup"><span data-stu-id="b01c6-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="b01c6-117">La vista è associato a dati al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b01c6-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="b01c6-118">Gli aggiornamenti al modello di visualizzazione vengono automaticamente riflesse nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b01c6-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="b01c6-119">Il modello di visualizzazione ottiene anche gli eventi dalla visualizzazione, ad esempio un clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="b01c6-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="b01c6-120">Questo approccio semplifica modificare il layout e interfaccia utente dell'app, poiché è possibile modificare le associazioni, senza riscrivere il codice.</span><span class="sxs-lookup"><span data-stu-id="b01c6-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="b01c6-121">Ad esempio, si potrebbe visualizzare un elenco di elementi come un `<ul>`, quindi modificarlo in seguito a una tabella.</span><span class="sxs-lookup"><span data-stu-id="b01c6-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="b01c6-122">Aggiungere la libreria Knockout</span><span class="sxs-lookup"><span data-stu-id="b01c6-122">Add the Knockout Library</span></span>

<span data-ttu-id="b01c6-123">In Visual Studio dal **degli strumenti** dal menu **Gestione pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b01c6-123">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="b01c6-124">Quindi selezionare **Console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="b01c6-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="b01c6-125">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b01c6-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="b01c6-126">Questo comando aggiunge i file Knockout nella cartella degli script.</span><span class="sxs-lookup"><span data-stu-id="b01c6-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="b01c6-127">Creare il modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="b01c6-127">Create the View Model</span></span>

<span data-ttu-id="b01c6-128">Aggiungere un file JavaScript denominato app. js nella cartella degli script.</span><span class="sxs-lookup"><span data-stu-id="b01c6-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="b01c6-129">(In Esplora soluzioni fare doppio clic su cartella Scripts, selezionare **Add**, quindi selezionare **JavaScript File**.) Incollare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b01c6-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="b01c6-130">In Knockout, il `observable` classe consente l'associazione dati.</span><span class="sxs-lookup"><span data-stu-id="b01c6-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="b01c6-131">Quando si modifica il contenuto di un oggetto osservabile, la sequenza observable notifica a tutti i controlli con associazione a dati, in modo che possano aggiornare autonomamente.</span><span class="sxs-lookup"><span data-stu-id="b01c6-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="b01c6-132">(Il `observableArray` classe è la versione di matrice di *observable*.) Per iniziare, il nostro modello di visualizzazione dispone di due oggetti osservabili:</span><span class="sxs-lookup"><span data-stu-id="b01c6-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="b01c6-133">`books` contiene l'elenco di libri.</span><span class="sxs-lookup"><span data-stu-id="b01c6-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="b01c6-134">`error` contiene un messaggio di errore se ha esito negativo di una chiamata AJAX.</span><span class="sxs-lookup"><span data-stu-id="b01c6-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="b01c6-135">Il `getAllBooks` metodo effettua una chiamata AJAX per ottenere l'elenco di libri.</span><span class="sxs-lookup"><span data-stu-id="b01c6-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="b01c6-136">Quindi inserisce il risultato nel `books` matrice.</span><span class="sxs-lookup"><span data-stu-id="b01c6-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="b01c6-137">Il `ko.applyBindings` metodo fa parte della libreria Knockout.</span><span class="sxs-lookup"><span data-stu-id="b01c6-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="b01c6-138">Accetta il modello di visualizzazione come parametro e imposta il data binding.</span><span class="sxs-lookup"><span data-stu-id="b01c6-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="b01c6-139">Aggiungere un Bundle di Script</span><span class="sxs-lookup"><span data-stu-id="b01c6-139">Add a Script Bundle</span></span>

<span data-ttu-id="b01c6-140">Creazione di bundle è una funzionalità in ASP.NET 4.5 che rende più semplice combinare o più file del bundle in un unico file.</span><span class="sxs-lookup"><span data-stu-id="b01c6-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="b01c6-141">Creazione di bundle consente di ridurre il numero di richieste al server, che consente di migliorare il tempo di caricamento pagina.</span><span class="sxs-lookup"><span data-stu-id="b01c6-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="b01c6-142">Aprire il file App\_Start/BundleConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="b01c6-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="b01c6-143">Aggiungere il codice seguente al metodo RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="b01c6-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="b01c6-144">[Precedente](part-5.md)
> [Successivo](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="b01c6-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
