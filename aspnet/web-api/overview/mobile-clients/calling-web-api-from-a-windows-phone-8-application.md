---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Chiamata di API Web da un Windows Phone 8 applicazione (c#) | Documenti Microsoft
author: rmcmurray
description: Creare uno scenario end-to-end completato, costituita da un'applicazione ASP.NET Web API che fornisce un catalogo di libri da un'applicazione Windows Phone 8.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 7d0486b4cab85ffe77fda87d4b34dd3ec0a9e8fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="5ac85-103">Chiamata di API Web da un'applicazione di Windows Phone 8 (c#)</span><span class="sxs-lookup"><span data-stu-id="5ac85-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="5ac85-104">da [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="5ac85-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="5ac85-105">In questa esercitazione si apprenderà come creare uno scenario end-to-end completato, costituita da un'applicazione ASP.NET Web API che fornisce un catalogo di libri da un'applicazione Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="5ac85-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="5ac85-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="5ac85-106">Overview</span></span>

<span data-ttu-id="5ac85-107">Servizi rESTful come ASP.NET Web API semplificano la creazione di applicazioni basate su HTTP per gli sviluppatori mediante l'astrazione l'architettura per applicazioni sul lato server e lato client.</span><span class="sxs-lookup"><span data-stu-id="5ac85-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="5ac85-108">Anziché creare un protocollo basato sui socket proprietario per la comunicazione, gli sviluppatori di Web API semplicemente necessario pubblicare i metodi HTTP necessari per la propria applicazione, (ad esempio: GET, POST, PUT, DELETE), e solo gli sviluppatori di applicazioni client dovranno utilizzare i metodi HTTP che sono necessari per la propria applicazione.</span><span class="sxs-lookup"><span data-stu-id="5ac85-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="5ac85-109">In questa esercitazione end-to-end, si apprenderà come usare l'API Web per creare i progetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="5ac85-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="5ac85-110">Nel [prima parte di questa esercitazione](#STEP1), si creerà un'applicazione API Web ASP.NET che supporta tutte le operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) per gestire un catalogo di libri.</span><span class="sxs-lookup"><span data-stu-id="5ac85-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="5ac85-111">L'applicazione utilizzerà il [File XML di esempio (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) da MSDN.</span><span class="sxs-lookup"><span data-stu-id="5ac85-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="5ac85-112">Nel [seconda parte di questa esercitazione](#STEP2), si creerà un'applicazione Windows Phone 8 interattiva che consente di recuperare i dati dall'applicazione API Web.</span><span class="sxs-lookup"><span data-stu-id="5ac85-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="5ac85-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="5ac85-113">Prerequisites</span></span>

- <span data-ttu-id="5ac85-114">Visual Studio 2013 con Windows Phone 8 SDK installato</span><span class="sxs-lookup"><span data-stu-id="5ac85-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="5ac85-115">Windows 8 o in un secondo momento con Hyper-V sia installato un sistema a 64 bit</span><span class="sxs-lookup"><span data-stu-id="5ac85-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="5ac85-116">Per un elenco di requisiti aggiuntivi, vedere il *requisiti di sistema* sezione la [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) pagina di download.</span><span class="sxs-lookup"><span data-stu-id="5ac85-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="5ac85-117">Se si desidera testare la connettività tra API Web e i progetti Windows Phone 8 nel sistema locale, è necessario seguire le istruzioni di *[connessione l'emulatore di Windows Phone 8 per le applicazioni API Web in un locale Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* articolo per configurare l'ambiente di test.</span><span class="sxs-lookup"><span data-stu-id="5ac85-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="5ac85-118">Passaggio 1: Creazione del progetto di libreria di API Web</span><span class="sxs-lookup"><span data-stu-id="5ac85-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="5ac85-119">Il primo passaggio di questa esercitazione end-to-end consiste nel creare un progetto di API Web che supporta tutte le operazioni CRUD. Si noti che si aggiunge il progetto di applicazione Windows Phone per questa soluzione in [passaggio 2](#STEP2) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5ac85-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="5ac85-120">Aprire **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="5ac85-121">Fare clic su **File**, quindi **nuova**e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="5ac85-122">Quando il **nuovo progetto** è visualizzata la finestra di dialogo, espandere **installato**, quindi **modelli**, quindi **Visual c#**e quindi **Web**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="5ac85-123">Fare clic per espandere immagine</span><span class="sxs-lookup"><span data-stu-id="5ac85-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="5ac85-124">Evidenziare **applicazione Web ASP.NET**, immettere **BookStore** per il nome del progetto e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="5ac85-125">Quando il **nuovo progetto ASP.NET** è visualizzata la finestra di dialogo, selezionare il **API Web** modello e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="5ac85-126">Fare clic per espandere immagine</span><span class="sxs-lookup"><span data-stu-id="5ac85-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="5ac85-127">Quando si apre il progetto API Web, è possibile rimuovere il controller di esempio dal progetto:</span><span class="sxs-lookup"><span data-stu-id="5ac85-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="5ac85-128">Espandere il **controller** cartella in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="5ac85-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="5ac85-129">Fare doppio clic su di **ValuesController.cs** file e quindi fare clic su **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="5ac85-130">Fare clic su **OK** quando viene richiesto di confermare l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="5ac85-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="5ac85-131">Aggiungere un file di dati XML al progetto API Web. Questo file contiene il contenuto del catalogo della libreria:</span><span class="sxs-lookup"><span data-stu-id="5ac85-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="5ac85-132">Fare doppio clic su di **App\_dati** cartella in Esplora soluzioni, quindi fare clic su **Aggiungi**, quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="5ac85-133">Quando il **Aggiungi nuovo elemento** è visualizzata la finestra di dialogo, evidenziare il **File XML** modello.</span><span class="sxs-lookup"><span data-stu-id="5ac85-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="5ac85-134">Nome del file **Books.xml**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="5ac85-135">Quando il **Books.xml** file è aperto, sostituire il codice nel file con il codice XML dall'esempio **books.xml** file su MSDN:</span><span class="sxs-lookup"><span data-stu-id="5ac85-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="5ac85-136">Salvare e chiudere il file XML.</span><span class="sxs-lookup"><span data-stu-id="5ac85-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="5ac85-137">Aggiungere il modello di libreria al progetto API Web. Questo modello contiene la logica di creazione, lettura, aggiornamento ed eliminazione (CRUD) per l'applicazione della libreria:</span><span class="sxs-lookup"><span data-stu-id="5ac85-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="5ac85-138">Fare doppio clic su di **modelli** cartella in Esplora soluzioni, quindi fare clic su **Aggiungi**, quindi fare clic su **classe**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="5ac85-139">Quando il **Aggiungi nuovo elemento** è visualizzata la finestra di dialogo, denominare il file di classe **BookDetails.cs**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="5ac85-140">Quando il **BookDetails.cs** file è aperto, sostituire il codice nel file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5ac85-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="5ac85-141">Salvare e chiudere il **BookDetails.cs** file.</span><span class="sxs-lookup"><span data-stu-id="5ac85-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="5ac85-142">Aggiungere il controller della libreria per il progetto API Web:</span><span class="sxs-lookup"><span data-stu-id="5ac85-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="5ac85-143">Fare doppio clic sul **controller** cartella in Esplora soluzioni, quindi fare clic su **Aggiungi**, quindi fare clic su **Controller**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="5ac85-144">Quando il **aggiungere lo scaffolding** è visualizzata la finestra di dialogo, evidenziare **Web API 2 Controller - vuoto**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="5ac85-145">Quando il **Aggiungi Controller** è visualizzata la finestra di dialogo, denominare il controller **BooksController**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="5ac85-146">Quando il **BooksController.cs** file è aperto, sostituire il codice nel file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5ac85-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="5ac85-147">Salvare e chiudere il **BooksController.cs** file.</span><span class="sxs-lookup"><span data-stu-id="5ac85-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="5ac85-148">Compilare l'applicazione API Web per controllare gli errori.</span><span class="sxs-lookup"><span data-stu-id="5ac85-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="5ac85-149">Passaggio 2: Aggiunta del progetto di catalogo della libreria di Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="5ac85-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="5ac85-150">Il passaggio successivo di questo scenario end-to-end consiste nel creare l'applicazione di catalogo per Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="5ac85-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="5ac85-151">L'applicazione utilizzerà il *Windows Phone Databound App* modello per l'interfaccia utente predefinita e si utilizzerà l'applicazione API Web che è stato creato in [passaggio 1](#STEP1) di questa esercitazione come origine dati.</span><span class="sxs-lookup"><span data-stu-id="5ac85-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="5ac85-152">Fare doppio clic su di **BookStore** soluzione nel in Esplora soluzioni, quindi fare clic su **Aggiungi**e quindi **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="5ac85-153">Quando il **nuovo progetto** è visualizzata la finestra di dialogo, espandere **installato**, quindi **Visual c#**e quindi **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="5ac85-154">Evidenziare **Windows Phone Databound App**, immettere **BookCatalog** per il nome e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="5ac85-155">Aggiungere il pacchetto NuGet di Json.NET per la **BookCatalog** progetto:</span><span class="sxs-lookup"><span data-stu-id="5ac85-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="5ac85-156">Fare doppio clic su **riferimenti** per il **BookCatalog** progetto in Esplora soluzioni e quindi fare clic su **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5ac85-157">Quando il **Gestisci pacchetti NuGet** è visualizzata la finestra di dialogo, espandere il **Online** sezione ed evidenziare **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="5ac85-158">Immettere **Json.NET** nella ricerca e fare clic sull'icona di ricerca.</span><span class="sxs-lookup"><span data-stu-id="5ac85-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="5ac85-159">Evidenziare **Json.NET** nei risultati di ricerca e quindi fare clic su **installare**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="5ac85-160">Al termine dell'installazione, fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="5ac85-161">Aggiungere il **BookDetails** modello per il **BookCatalog** progetto; contiene un modello generico della classe della libreria:</span><span class="sxs-lookup"><span data-stu-id="5ac85-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="5ac85-162">Fare doppio clic su di **BookCatalog** progetto in Esplora soluzioni, quindi fare clic su **Aggiungi**, quindi fare clic su **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="5ac85-163">Denominare la nuova cartella **modelli**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="5ac85-164">Fare doppio clic su di **modelli** cartella in Esplora soluzioni, quindi fare clic su **Aggiungi**, quindi fare clic su **classe**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="5ac85-165">Quando il **Aggiungi nuovo elemento** è visualizzata la finestra di dialogo, denominare il file di classe **BookDetails.cs**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="5ac85-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="5ac85-166">Quando il **BookDetails.cs** file è aperto, sostituire il codice nel file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5ac85-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="5ac85-167">Salvare e chiudere il **BookDetails.cs** file.</span><span class="sxs-lookup"><span data-stu-id="5ac85-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="5ac85-168">Aggiornamento di **MainViewModel.cs** classe per includere la funzionalità per comunicare con l'applicazione API Web della libreria:</span><span class="sxs-lookup"><span data-stu-id="5ac85-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="5ac85-169">Espandere il **ViewModel** cartella in Esplora soluzioni e quindi fare doppio clic il **MainViewModel.cs** file.</span><span class="sxs-lookup"><span data-stu-id="5ac85-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="5ac85-170">Quando il **MainViewModel.cs** file è aperto, sostituire il codice nel file con il codice seguente, si noti che è necessario aggiornare il valore del `apiUrl` costante con l'URL effettivo dell'API Web:</span><span class="sxs-lookup"><span data-stu-id="5ac85-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="5ac85-171">Salvare e chiudere il **MainViewModel.cs** file.</span><span class="sxs-lookup"><span data-stu-id="5ac85-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="5ac85-172">Aggiornamento di **MainPage. XAML** file per personalizzare il nome dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="5ac85-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="5ac85-173">Fare doppio clic su di **MainPage. XAML** file in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="5ac85-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="5ac85-174">Quando il **MainPage. XAML** file è aperto, individuare le righe di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5ac85-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="5ac85-175">Sostituire le righe con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5ac85-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="5ac85-176">Salvare e chiudere il **MainPage. XAML** file.</span><span class="sxs-lookup"><span data-stu-id="5ac85-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="5ac85-177">Aggiornamento di **DetailsPage.xaml** file per personalizzare gli elementi visualizzati:</span><span class="sxs-lookup"><span data-stu-id="5ac85-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="5ac85-178">Fare doppio clic su di **DetailsPage.xaml** file in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="5ac85-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="5ac85-179">Quando il **DetailsPage.xaml** file è aperto, individuare le righe di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5ac85-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="5ac85-180">Sostituire le righe con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5ac85-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="5ac85-181">Salvare e chiudere il **DetailsPage.xaml** file.</span><span class="sxs-lookup"><span data-stu-id="5ac85-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="5ac85-182">Compilare l'applicazione Windows Phone per controllare gli errori.</span><span class="sxs-lookup"><span data-stu-id="5ac85-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="5ac85-183">Passaggio 3: Test della soluzione End-to-End</span><span class="sxs-lookup"><span data-stu-id="5ac85-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="5ac85-184">Come indicato nella *prerequisiti* sezione di questa esercitazione, quando si testa la connettività tra API Web e Windows Phone 8 progetti nel sistema locale, è necessario seguire le istruzioni di *[ Connessione dell'emulatore di Windows Phone 8 per le applicazioni API Web in un Computer locale](https://go.microsoft.com/fwlink/?LinkId=324014)* articolo per configurare l'ambiente di test.</span><span class="sxs-lookup"><span data-stu-id="5ac85-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="5ac85-185">Dopo aver configurato l'ambiente di testing, è necessario impostare l'applicazione Windows Phone come progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="5ac85-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="5ac85-186">A tale scopo, evidenziare il **BookCatalog** applicazione in Esplora soluzioni e quindi fare clic su **imposta come progetto di avvio**:</span><span class="sxs-lookup"><span data-stu-id="5ac85-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="5ac85-187">Fare clic per espandere immagine</span><span class="sxs-lookup"><span data-stu-id="5ac85-187">Click image to expand</span></span> |

<span data-ttu-id="5ac85-188">Quando si preme F5, Visual Studio verrà avviato sia di Windows Phone emulatore, che verrà visualizzato un &quot;attendere&quot; messaggio mentre i dati dell'applicazione viene recuperati dall'API Web:</span><span class="sxs-lookup"><span data-stu-id="5ac85-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="5ac85-189">Fare clic per espandere immagine</span><span class="sxs-lookup"><span data-stu-id="5ac85-189">Click image to expand</span></span> |

<span data-ttu-id="5ac85-190">Se tutti gli elementi ha esito positivo, verrà visualizzato il catalogo visualizzato:</span><span class="sxs-lookup"><span data-stu-id="5ac85-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="5ac85-191">Fare clic per espandere immagine</span><span class="sxs-lookup"><span data-stu-id="5ac85-191">Click image to expand</span></span> |

<span data-ttu-id="5ac85-192">Se si tocca su qualsiasi titolo del libro, l'applicazione verrà visualizzata la descrizione del libro:</span><span class="sxs-lookup"><span data-stu-id="5ac85-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="5ac85-193">Fare clic per espandere immagine</span><span class="sxs-lookup"><span data-stu-id="5ac85-193">Click image to expand</span></span> |

<span data-ttu-id="5ac85-194">Se l'applicazione non può comunicare con l'API Web, verrà visualizzato un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="5ac85-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="5ac85-195">Fare clic per espandere immagine</span><span class="sxs-lookup"><span data-stu-id="5ac85-195">Click image to expand</span></span> |

<span data-ttu-id="5ac85-196">Se si tocca sul messaggio di errore, verranno visualizzati eventuali dettagli aggiuntivi sull'errore:</span><span class="sxs-lookup"><span data-stu-id="5ac85-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="5ac85-197">Fare clic per espandere immagine</span><span class="sxs-lookup"><span data-stu-id="5ac85-197">Click image to expand</span></span>                                                                 |

