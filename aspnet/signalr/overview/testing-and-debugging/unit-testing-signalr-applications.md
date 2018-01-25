---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Unit test delle applicazioni SignalR | Documenti Microsoft
author: pfletcher
description: "In questo articolo viene descritto come utilizzare le funzionalità di Unit test di SignalR 2.0."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: d767e1a9d27670387133e5a48a8f92f5bdd39d9e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="88023-103">Unit test delle applicazioni SignalR</span><span class="sxs-lookup"><span data-stu-id="88023-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="88023-104">da [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="88023-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="88023-105">Questo articolo descrive l'uso delle funzionalità di Unit test di SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="88023-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="88023-106">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="88023-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="88023-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="88023-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="88023-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="88023-108">.NET 4.5</span></span>
> - <span data-ttu-id="88023-109">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="88023-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="88023-110">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="88023-110">Questions and comments</span></span>
> 
> <span data-ttu-id="88023-111">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="88023-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="88023-112">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="88023-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="88023-113">Unit test delle applicazioni SignalR</span><span class="sxs-lookup"><span data-stu-id="88023-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="88023-114">È possibile utilizzare le funzionalità di unit test in SignalR 2 per creare unit test per l'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="88023-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="88023-115">SignalR 2 include i [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interfaccia, che può essere utilizzato per creare un oggetto fittizio per simulare i metodi dell'hub di test.</span><span class="sxs-lookup"><span data-stu-id="88023-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="88023-116">In questa sezione aggiungerai unit test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando [XUnit.net](https://github.com/xunit/xunit) e [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="88023-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="88023-117">XUnit.net verrà utilizzato per controllare il test. Moq verrà usato per creare un [simulare](http://en.wikipedia.org/wiki/Mock_object) oggetto per il test.</span><span class="sxs-lookup"><span data-stu-id="88023-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="88023-118">Se si desidera; è possibile utilizzare altri framework di simulazione [NSubstitute](http://nsubstitute.github.io/) è anche una buona scelta.</span><span class="sxs-lookup"><span data-stu-id="88023-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="88023-119">Questa esercitazione viene illustrato come impostare l'oggetto fittizio in due modi: in primo luogo, usando un `dynamic` oggetto (introdotto in .NET Framework 4) e i secondi, utilizzando un'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="88023-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="88023-120">Sommario</span><span class="sxs-lookup"><span data-stu-id="88023-120">Contents</span></span>

<span data-ttu-id="88023-121">In questa esercitazione include le sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="88023-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="88023-122">Unit test con dinamico</span><span class="sxs-lookup"><span data-stu-id="88023-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="88023-123">Unit test dal tipo</span><span class="sxs-lookup"><span data-stu-id="88023-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="88023-124">Unit test con dinamico</span><span class="sxs-lookup"><span data-stu-id="88023-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="88023-125">In questa sezione si aggiungerà uno unit test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando un oggetto dinamico.</span><span class="sxs-lookup"><span data-stu-id="88023-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="88023-126">Installare il [estensione XUnit Runner](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) per Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="88023-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="88023-127">Completare il [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md), o scaricare l'applicazione completata dal [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="88023-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="88023-128">Se si utilizza la versione di download dell'applicazione Guida introduttiva, aprire **Package Manager Console** e fare clic su **ripristinare** per aggiungere il pacchetto di SignalR per il progetto.</span><span class="sxs-lookup"><span data-stu-id="88023-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Ripristino dei pacchetti](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="88023-130">Aggiungere un progetto alla soluzione per lo unit test.</span><span class="sxs-lookup"><span data-stu-id="88023-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="88023-131">Fare clic sulla soluzione in **Esplora** e selezionare **Aggiungi**, **nuovo progetto...** . Sotto il **c#** nodo, seleziona il **Windows** nodo.</span><span class="sxs-lookup"><span data-stu-id="88023-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="88023-132">Selezionare **libreria di classi**.</span><span class="sxs-lookup"><span data-stu-id="88023-132">Select **Class Library**.</span></span> <span data-ttu-id="88023-133">Denominare il nuovo progetto **TestLibrary** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="88023-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Creare una libreria di Test](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="88023-135">Aggiungere un riferimento nel progetto libreria di test al progetto SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="88023-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="88023-136">Fare doppio clic su di **TestLibrary** del progetto e selezionare **Aggiungi**, **riferimento...** . Selezionare il **progetti** nodo sotto il **soluzione** nodo e controllare **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="88023-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="88023-137">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="88023-137">Click **OK**.</span></span>

    ![Aggiungi riferimento al progetto](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="88023-139">Aggiungere i pacchetti SignalR, Moq e XUnit il **TestLibrary** progetto.</span><span class="sxs-lookup"><span data-stu-id="88023-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="88023-140">Nel **Package Manager Console**, impostare il **progetto predefinito** elenco a discesa per **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="88023-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="88023-141">Nella finestra della console, eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="88023-141">Run the following commands in the console window:</span></span>

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![Installare i pacchetti](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="88023-143">Creare il file di test.</span><span class="sxs-lookup"><span data-stu-id="88023-143">Create the test file.</span></span> <span data-ttu-id="88023-144">Fare doppio clic su di **TestLibrary** sul progetto e scegliere **Aggiungi...** , **Classe**.</span><span class="sxs-lookup"><span data-stu-id="88023-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="88023-145">Denominare la nuova classe **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="88023-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="88023-146">Sostituire il contenuto di Tests.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="88023-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="88023-147">Nel codice precedente, viene creato un client di prova utilizzando il `Mock` dall'oggetto di [Moq](https://github.com/Moq/moq4) libreria, di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1, assegnare `dynamic` per il tipo parametro). Il `IHubCallerConnectionContext` interfaccia è l'oggetto proxy con cui per richiamare metodi sul client.</span><span class="sxs-lookup"><span data-stu-id="88023-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="88023-148">Il `broadcastMessage` funzione viene quindi definita per il client fittizio in modo che può essere chiamato dalla `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="88023-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="88023-149">Chiama quindi il motore dei test di `Send` metodo il `ChatHub` (classe), che a sua volta chiama il simulati `broadcastMessage` (funzione).</span><span class="sxs-lookup"><span data-stu-id="88023-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="88023-150">Compilare la soluzione premendo **F6**.</span><span class="sxs-lookup"><span data-stu-id="88023-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="88023-151">Eseguire lo unit test.</span><span class="sxs-lookup"><span data-stu-id="88023-151">Run the unit test.</span></span> <span data-ttu-id="88023-152">In Visual Studio, selezionare **Test**, **Windows**, **Esplora Test**.</span><span class="sxs-lookup"><span data-stu-id="88023-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="88023-153">Nella finestra Esplora Test, fare doppio clic su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.</span><span class="sxs-lookup"><span data-stu-id="88023-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Esplora test](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="88023-155">Verificare che il test superato controllando riquadro in basso nella finestra Esplora Test.</span><span class="sxs-lookup"><span data-stu-id="88023-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="88023-156">Nella finestra verrà visualizzato che il test è stato superato.</span><span class="sxs-lookup"><span data-stu-id="88023-156">The window will show that the test passed.</span></span>

    ![Test superato](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="88023-158">Unit test dal tipo</span><span class="sxs-lookup"><span data-stu-id="88023-158">Unit testing by type</span></span>

<span data-ttu-id="88023-159">In questa sezione si aggiungerà un test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando un'interfaccia che contiene il metodo da testare.</span><span class="sxs-lookup"><span data-stu-id="88023-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="88023-160">Completare i passaggi 1-7 nel [Unit test con Dynamic](#dynamic) esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="88023-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="88023-161">Sostituire il contenuto di Tests.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="88023-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="88023-162">Nel codice precedente, viene creata un'interfaccia definisce la firma del `broadcastMessage` metodo per il quale il motore di test verrà creato un client fittizio.</span><span class="sxs-lookup"><span data-stu-id="88023-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="88023-163">Un client fittizio viene quindi creato utilizzando il `Mock` , oggetto di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1, assegnare `dynamic` per il parametro di tipo.) Il `IHubCallerConnectionContext` interfaccia è l'oggetto proxy con cui per richiamare metodi sul client.</span><span class="sxs-lookup"><span data-stu-id="88023-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="88023-164">Il test quindi crea un'istanza di `ChatHub`e quindi crea una versione fittizia del `broadcastMessage` metodo, che a sua volta viene richiamata chiamando il `Send` metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="88023-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="88023-165">Compilare la soluzione premendo **F6**.</span><span class="sxs-lookup"><span data-stu-id="88023-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="88023-166">Eseguire lo unit test.</span><span class="sxs-lookup"><span data-stu-id="88023-166">Run the unit test.</span></span> <span data-ttu-id="88023-167">In Visual Studio, selezionare **Test**, **Windows**, **Esplora Test**.</span><span class="sxs-lookup"><span data-stu-id="88023-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="88023-168">Nella finestra Esplora Test, fare doppio clic su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.</span><span class="sxs-lookup"><span data-stu-id="88023-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Esplora test](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="88023-170">Verificare che il test superato controllando riquadro in basso nella finestra Esplora Test.</span><span class="sxs-lookup"><span data-stu-id="88023-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="88023-171">Nella finestra verrà visualizzato che il test è stato superato.</span><span class="sxs-lookup"><span data-stu-id="88023-171">The window will show that the test passed.</span></span>

    ![Test superato](unit-testing-signalr-applications/_static/image8.png)
