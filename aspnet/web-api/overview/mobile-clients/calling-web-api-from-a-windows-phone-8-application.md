---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Chiamata di API Web da un Windows Phone 8 applicazione (c#) | Microsoft Docs
author: rmcmurray
description: Crea uno scenario end-to-end completato costituita da un'applicazione API Web ASP.NET che fornisce un catalogo di libri a un'applicazione Windows Phone 8.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 40be2935c52e7dcab9e682d4d15e9e75c0260223
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388422"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="ebd20-103">Chiamata di API Web da un'applicazione di 8 Windows Phone (c#)</span><span class="sxs-lookup"><span data-stu-id="ebd20-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="ebd20-104">da [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="ebd20-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="ebd20-105">In questa esercitazione si apprenderà come creare uno scenario end-to-end completato costituita da un'applicazione API Web ASP.NET che fornisce un catalogo di libri a un'applicazione Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="ebd20-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="ebd20-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="ebd20-106">Overview</span></span>

<span data-ttu-id="ebd20-107">Servizi rESTful, ad esempio API Web ASP.NET semplificano la creazione di applicazioni basate su HTTP per gli sviluppatori tramite l'astrazione l'architettura per le applicazioni lato client e lato server.</span><span class="sxs-lookup"><span data-stu-id="ebd20-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="ebd20-108">Anziché creare un protocollo basato sui socket proprietario per la comunicazione, gli sviluppatori di API Web devono semplicemente pubblicare i metodi HTTP necessari per la propria applicazione (ad esempio: GET, POST, PUT, DELETE), e gli sviluppatori di applicazioni client solo bisogno di utilizzarlo i metodi HTTP che sono necessari per la propria applicazione.</span><span class="sxs-lookup"><span data-stu-id="ebd20-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="ebd20-109">In questa esercitazione end-to-end, si apprenderà come usare l'API Web per creare i progetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="ebd20-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="ebd20-110">Nel [prima parte di questa esercitazione](#STEP1), si creerà un'applicazione API Web ASP.NET che supporta tutte le operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) per gestire un catalogo di libro.</span><span class="sxs-lookup"><span data-stu-id="ebd20-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="ebd20-111">L'applicazione utilizzerà il [esempio di File XML (books. XML)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) da MSDN.</span><span class="sxs-lookup"><span data-stu-id="ebd20-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="ebd20-112">Nel [seconda parte di questa esercitazione](#STEP2), si creerà un'applicazione Windows Phone 8 interattiva che consente di recuperare i dati dall'applicazione API Web.</span><span class="sxs-lookup"><span data-stu-id="ebd20-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="ebd20-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ebd20-113">Prerequisites</span></span>

- <span data-ttu-id="ebd20-114">Visual Studio 2013 con Windows Phone 8 SDK installato</span><span class="sxs-lookup"><span data-stu-id="ebd20-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="ebd20-115">Windows 8 o versioni successive in un sistema a 64 bit con installato Hyper-V</span><span class="sxs-lookup"><span data-stu-id="ebd20-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="ebd20-116">Per un elenco di requisiti aggiuntivi, vedere la *requisiti di sistema* sezione il [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) pagina di download.</span><span class="sxs-lookup"><span data-stu-id="ebd20-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="ebd20-117">Se si desidera testare la connettività tra API Web e i progetti Windows Phone 8 nel sistema locale, è necessario seguire le istruzioni di *[ci si connette all'emulatore di Windows Phone 8 alle applicazioni API Web in un locale Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* articolo per configurare l'ambiente di test.</span><span class="sxs-lookup"><span data-stu-id="ebd20-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="ebd20-118">Passaggio 1: Creazione del progetto della libreria di API Web</span><span class="sxs-lookup"><span data-stu-id="ebd20-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="ebd20-119">Il primo passaggio di questa esercitazione end-to-end consiste nel creare un progetto API Web che supporta tutte le operazioni CRUD. Si noti che si aggiunge il progetto di applicazione Windows Phone alla soluzione in [passaggio 2](#STEP2) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ebd20-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="ebd20-120">Aprire **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="ebd20-121">Fare clic su **File**, quindi **nuove**e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="ebd20-122">Quando la **nuovo progetto** verrà visualizzata la finestra di dialogo, espandere **installati**, quindi **modelli**, quindi **Visual c#** e quindi **Web**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="ebd20-123">Fare clic sull'immagine per espandere</span><span class="sxs-lookup"><span data-stu-id="ebd20-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="ebd20-124">Evidenziare **applicazione Web ASP.NET**, immettere **BookStore** per il nome del progetto e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="ebd20-125">Quando la **nuovo progetto ASP.NET** verrà visualizzata la finestra di dialogo, selezionare la **API Web** modello e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="ebd20-126">Fare clic sull'immagine per espandere</span><span class="sxs-lookup"><span data-stu-id="ebd20-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="ebd20-127">Quando si apre il progetto API Web, rimuovere il controller di esempio dal progetto:</span><span class="sxs-lookup"><span data-stu-id="ebd20-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="ebd20-128">Espandere la **controller** cartella in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="ebd20-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="ebd20-129">Fare doppio clic il **ValuesController.cs** del file e quindi fare clic su **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="ebd20-130">Fare clic su **OK** quando viene richiesto di confermare l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="ebd20-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="ebd20-131">Aggiungere un file di dati XML al progetto API Web. Questo file contiene il contenuto del catalogo della libreria:</span><span class="sxs-lookup"><span data-stu-id="ebd20-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="ebd20-132">Fare doppio clic sui **App\_dati** cartella in Esplora soluzioni, quindi fare clic su **Add**, quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="ebd20-133">Quando la **Aggiungi nuovo elemento** verrà visualizzata la finestra di dialogo, evidenziare il **File XML** modello.</span><span class="sxs-lookup"><span data-stu-id="ebd20-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="ebd20-134">Denominare il file **books. XML**, quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="ebd20-135">Quando la **books. XML** apertura file, sostituire il codice nel file con il codice XML di esempio **books. XML** file su MSDN:</span><span class="sxs-lookup"><span data-stu-id="ebd20-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="ebd20-136">Salvare e chiudere il file XML.</span><span class="sxs-lookup"><span data-stu-id="ebd20-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="ebd20-137">Aggiungere il modello della libreria al progetto API Web. Questo modello contiene la logica di creazione, lettura, aggiornamento ed eliminazione (CRUD) per l'applicazione della libreria:</span><span class="sxs-lookup"><span data-stu-id="ebd20-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="ebd20-138">Fare doppio clic sui **modelli** cartella in Esplora soluzioni, quindi fare clic su **Add**, quindi fare clic su **classe**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="ebd20-139">Quando la **Aggiungi nuovo elemento** verrà visualizzata la finestra di dialogo, denominare il file di classe **BookDetails.cs**, quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="ebd20-140">Quando la **BookDetails.cs** file viene aperto, sostituire il codice nel file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ebd20-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="ebd20-141">Salvare e chiudere il **BookDetails.cs** file.</span><span class="sxs-lookup"><span data-stu-id="ebd20-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="ebd20-142">Aggiungere il controller della libreria al progetto API Web:</span><span class="sxs-lookup"><span data-stu-id="ebd20-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="ebd20-143">Fare doppio clic sul **controller** cartella in Esplora soluzioni, quindi fare clic su **Add**, quindi fare clic su **Controller**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="ebd20-144">Quando la **Add Scaffold** verrà visualizzata la finestra di dialogo, evidenziare **Web API 2 Controller - Empty**, quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="ebd20-145">Quando la **Aggiungi Controller** verrà visualizzata la finestra di dialogo, denominare il controller **BooksController**, quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="ebd20-146">Quando la **BooksController.cs** file viene aperto, sostituire il codice nel file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ebd20-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="ebd20-147">Salvare e chiudere il **BooksController.cs** file.</span><span class="sxs-lookup"><span data-stu-id="ebd20-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="ebd20-148">Compilare l'applicazione API Web per verificare la presenza di errori.</span><span class="sxs-lookup"><span data-stu-id="ebd20-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="ebd20-149">Passaggio 2: Aggiungere il progetto di catalogo della libreria di Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="ebd20-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="ebd20-150">Il passaggio successivo di questo scenario end-to-end consiste nel creare l'applicazione di catalogo per Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="ebd20-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="ebd20-151">L'applicazione utilizzerà il *Windows Phone Databound App* modello per l'interfaccia utente predefinita che utilizzerà l'applicazione API Web creato nel [passaggio 1](#STEP1) di questa esercitazione come origine dati.</span><span class="sxs-lookup"><span data-stu-id="ebd20-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="ebd20-152">Fare doppio clic sul **BookStore** soluzione nel in Esplora soluzioni, quindi fare clic su **Add**e quindi **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="ebd20-153">Quando la **nuovo progetto** verrà visualizzata la finestra di dialogo, espandere **installati**, quindi **Visual c#** e quindi **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="ebd20-154">Evidenziare **Windows Phone Databound App**, immettere **BookCatalog** per il nome e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="ebd20-155">Quando si preme F5, Visual Studio avvierà sia il Windows Phone emulatore, che verrà visualizzato un **attendere** del messaggio mentre i dati dell'applicazione vengono recuperati dall'API Web:</span><span class="sxs-lookup"><span data-stu-id="ebd20-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="ebd20-156">Se tutto ciò che ha esito positivo, verrà visualizzato il catalogo visualizzato:</span><span class="sxs-lookup"><span data-stu-id="ebd20-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="ebd20-157">Se si tocca su qualsiasi titolo del libro, l'applicazione visualizzerà la descrizione del libro:</span><span class="sxs-lookup"><span data-stu-id="ebd20-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="ebd20-158">Se l'applicazione non può comunicare con l'API Web, verrà visualizzato un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="ebd20-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="ebd20-159">Se si tocca sul messaggio di errore, verranno visualizzati eventuali dettagli aggiuntivi sull'errore:</span><span class="sxs-lookup"><span data-stu-id="ebd20-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="ebd20-160">Al termine dell'installazione, fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="ebd20-161">Aggiungere il **BookDetails** del modello per il **BookCatalog** progetto; contiene un modello generico della classe della libreria:</span><span class="sxs-lookup"><span data-stu-id="ebd20-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="ebd20-162">Fare doppio clic sui **BookCatalog** del progetto in Esplora soluzioni, quindi fare clic su **Add**, quindi fare clic su **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="ebd20-163">Denominare la nuova cartella **modelli**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="ebd20-164">Fare doppio clic sui **modelli** cartella in Esplora soluzioni, quindi fare clic su **Add**, quindi fare clic su **classe**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="ebd20-165">Quando la **Aggiungi nuovo elemento** verrà visualizzata la finestra di dialogo, denominare il file di classe **BookDetails.cs**, quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="ebd20-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="ebd20-166">Quando la **BookDetails.cs** file viene aperto, sostituire il codice nel file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ebd20-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="ebd20-167">Salvare e chiudere il **BookDetails.cs** file.</span><span class="sxs-lookup"><span data-stu-id="ebd20-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="ebd20-168">Aggiorna il **MainViewModel.cs** classe per includere la funzionalità per comunicare con l'applicazione API Web della libreria:</span><span class="sxs-lookup"><span data-stu-id="ebd20-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="ebd20-169">Espandere la **ViewModel** cartella in Esplora soluzioni e quindi fare doppio clic il **MainViewModel.cs** file.</span><span class="sxs-lookup"><span data-stu-id="ebd20-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="ebd20-170">Quando la **MainViewModel.cs** file viene aperto, sostituire il codice nel file con il codice seguente; si noti che sarà necessario aggiornare il valore della `apiUrl` costanti con l'URL effettivo dell'API Web:</span><span class="sxs-lookup"><span data-stu-id="ebd20-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="ebd20-171">Salvare e chiudere il **MainViewModel.cs** file.</span><span class="sxs-lookup"><span data-stu-id="ebd20-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="ebd20-172">Aggiorna il **MainPage. XAML** . ini per personalizzare il nome dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="ebd20-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="ebd20-173">Fare doppio clic il **MainPage. XAML** file in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="ebd20-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="ebd20-174">Quando la **MainPage. XAML** file viene aperto, individuare le righe di codice seguenti:</span><span class="sxs-lookup"><span data-stu-id="ebd20-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="ebd20-175">Sostituire le righe con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="ebd20-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="ebd20-176">Salvare e chiudere il **MainPage. XAML** file.</span><span class="sxs-lookup"><span data-stu-id="ebd20-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="ebd20-177">Aggiorna il **DetailsPage.xaml** . ini per personalizzare gli elementi visualizzati:</span><span class="sxs-lookup"><span data-stu-id="ebd20-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="ebd20-178">Fare doppio clic il **DetailsPage.xaml** file in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="ebd20-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="ebd20-179">Quando la **DetailsPage.xaml** file viene aperto, individuare le righe di codice seguenti:</span><span class="sxs-lookup"><span data-stu-id="ebd20-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="ebd20-180">Sostituire le righe con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="ebd20-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="ebd20-181">Salvare e chiudere il **DetailsPage.xaml** file.</span><span class="sxs-lookup"><span data-stu-id="ebd20-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="ebd20-182">Compilare l'applicazione Windows Phone per individuare eventuali errori.</span><span class="sxs-lookup"><span data-stu-id="ebd20-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="ebd20-183">Passaggio 3: Test della soluzione End-to-End</span><span class="sxs-lookup"><span data-stu-id="ebd20-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="ebd20-184">Come accennato nel *prerequisiti* sezione di questa esercitazione, quando si testa la connettività tra l'API Web e Windows Phone 8 i progetti nel sistema locale, sarà necessario seguire le istruzioni riportate nel *[ Ci si connette all'emulatore di Windows Phone 8 alle applicazioni API Web in un Computer locale](https://go.microsoft.com/fwlink/?LinkId=324014)* articolo per configurare l'ambiente di test.</span><span class="sxs-lookup"><span data-stu-id="ebd20-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="ebd20-185">Dopo aver configurato l'ambiente di testing, è necessario impostare l'applicazione Windows Phone come progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="ebd20-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="ebd20-186">A tale scopo, evidenziare il **BookCatalog** dell'applicazione in Esplora soluzioni e quindi fare clic su **imposta come progetto di avvio**:</span><span class="sxs-lookup"><span data-stu-id="ebd20-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="ebd20-187">Fare clic sull'immagine per espandere</span><span class="sxs-lookup"><span data-stu-id="ebd20-187">Click image to expand</span></span> |

<span data-ttu-id="ebd20-188">Quando si preme F5, Visual Studio avvierà sia il Windows Phone emulatore, che verrà visualizzato un &quot;attendere&quot; del messaggio mentre i dati dell'applicazione vengono recuperati dall'API Web:</span><span class="sxs-lookup"><span data-stu-id="ebd20-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="ebd20-189">Fare clic sull'immagine per espandere</span><span class="sxs-lookup"><span data-stu-id="ebd20-189">Click image to expand</span></span> |

<span data-ttu-id="ebd20-190">Se tutto ciò che ha esito positivo, verrà visualizzato il catalogo visualizzato:</span><span class="sxs-lookup"><span data-stu-id="ebd20-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="ebd20-191">Fare clic sull'immagine per espandere</span><span class="sxs-lookup"><span data-stu-id="ebd20-191">Click image to expand</span></span> |

<span data-ttu-id="ebd20-192">Se si tocca su qualsiasi titolo del libro, l'applicazione visualizzerà la descrizione del libro:</span><span class="sxs-lookup"><span data-stu-id="ebd20-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="ebd20-193">Fare clic sull'immagine per espandere</span><span class="sxs-lookup"><span data-stu-id="ebd20-193">Click image to expand</span></span> |

<span data-ttu-id="ebd20-194">Se l'applicazione non può comunicare con l'API Web, verrà visualizzato un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="ebd20-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="ebd20-195">Fare clic sull'immagine per espandere</span><span class="sxs-lookup"><span data-stu-id="ebd20-195">Click image to expand</span></span> |

<span data-ttu-id="ebd20-196">Se si tocca sul messaggio di errore, verranno visualizzati eventuali dettagli aggiuntivi sull'errore:</span><span class="sxs-lookup"><span data-stu-id="ebd20-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="ebd20-197">Fare clic sull'immagine per espandere</span><span class="sxs-lookup"><span data-stu-id="ebd20-197">Click image to expand</span></span>                                                                 |

