---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Esercitazione: Introduzione a SignalR 1. x | Documenti Microsoft'
author: pfletcher
description: Utilizzare ASP.NET SignalR per compilare un'applicazione di chat in tempo reale in una pagina HTML.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: c61be6f7a64c000c8d9489f35eea520fd0bb32dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="191ec-103">Esercitazione: Introduzione a SignalR 1. x</span><span class="sxs-lookup"><span data-stu-id="191ec-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="191ec-104">da [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="191ec-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="191ec-105">In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="191ec-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="191ec-106">Verrà aggiunta SignalR a un'applicazione web ASP.NET vuota e creare una pagina HTML per l'invio e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="191ec-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="191ec-107">Panoramica</span><span class="sxs-lookup"><span data-stu-id="191ec-107">Overview</span></span>

<span data-ttu-id="191ec-108">Questa esercitazione viene illustrato lo sviluppo di SignalR viene mostrato come compilare un'applicazione di una chat semplice basato su browser.</span><span class="sxs-lookup"><span data-stu-id="191ec-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="191ec-109">Si verrà aggiungere la libreria SignalR a un'applicazione web ASP.NET vuota, creare una classe di hub per l'invio di messaggi al client e creare una pagina HTML che consente agli utenti di inviare e ricevere messaggi di chat.</span><span class="sxs-lookup"><span data-stu-id="191ec-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="191ec-110">Per un'esercitazione simile che illustra come creare un'applicazione di chat in MVC 4 con una visualizzazione MVC, vedere [Guida introduttiva a SignalR e MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="191ec-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="191ec-111">Questa esercitazione Usa la versione (1. x) di SignalR.</span><span class="sxs-lookup"><span data-stu-id="191ec-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="191ec-112">Per informazioni dettagliate sulle modifiche tra SignalR 1. x e 2.0, vedere [SignalR aggiornamento 1. x progetti](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="191ec-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="191ec-113">SignalR è una libreria .NET open source per la compilazione di applicazioni web che richiedono l'intervento dell'utente in tempo reale o gli aggiornamenti dei dati in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="191ec-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="191ec-114">Sono esempi di applicazioni social networking, giochi multiutente, meteo di collaborazione e notizie, business o finanziari aggiornamento delle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="191ec-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="191ec-115">Spesso si tratta di applicazioni in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="191ec-115">These are often called real-time applications.</span></span>

<span data-ttu-id="191ec-116">SignalR semplifica il processo di compilazione di applicazioni in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="191ec-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="191ec-117">Include una libreria del server ASP.NET e una libreria client JavaScript per renderne più semplice gestire le connessioni client-server e inviare gli aggiornamenti del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="191ec-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="191ec-118">È possibile aggiungere la libreria SignalR a un'applicazione ASP.NET esistente per ottenere le funzionalità in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="191ec-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="191ec-119">Nell'esercitazione vengono illustrate le seguenti attività di sviluppo SignalR:</span><span class="sxs-lookup"><span data-stu-id="191ec-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="191ec-120">Aggiunta della libreria di SignalR a un'applicazione web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="191ec-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="191ec-121">Creazione di una classe di hub per eseguire il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="191ec-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="191ec-122">Utilizzando la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.</span><span class="sxs-lookup"><span data-stu-id="191ec-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="191ec-123">La schermata seguente viene illustrata l'applicazione di chat in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="191ec-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="191ec-124">Ogni nuovo utente è possibile inviare commenti e vedere i commenti aggiunti dopo che l'utente viene aggiunto alla chat.</span><span class="sxs-lookup"><span data-stu-id="191ec-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Istanze di chat](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="191ec-126">Nelle sezioni:</span><span class="sxs-lookup"><span data-stu-id="191ec-126">Sections:</span></span>

- [<span data-ttu-id="191ec-127">Impostare il progetto</span><span class="sxs-lookup"><span data-stu-id="191ec-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="191ec-128">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="191ec-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="191ec-129">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="191ec-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="191ec-130">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="191ec-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="191ec-131">Impostare il progetto</span><span class="sxs-lookup"><span data-stu-id="191ec-131">Set up the Project</span></span>

<span data-ttu-id="191ec-132">In questa sezione viene illustrato come creare applicazioni web ASP.NET vuota aggiungere SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="191ec-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="191ec-133">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="191ec-133">Prerequisites:</span></span>

- <span data-ttu-id="191ec-134">Visual Studio 2010 SP1 o 2012.</span><span class="sxs-lookup"><span data-stu-id="191ec-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="191ec-135">Se non si dispone di Visual Studio, vedere [Scarica ASP.NET](https://www.asp.net/downloads) per ottenere il libero Visual Studio 2012 Express strumento di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="191ec-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="191ec-136">[Microsoft Web ASP.NET e strumenti 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="191ec-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="191ec-137">Per Visual Studio 2012, il programma di installazione aggiunge nuove funzionalità ASP.NET, inclusi i modelli di SignalR per Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="191ec-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="191ec-138">Per Visual Studio 2010 SP1, un programma di installazione non è disponibile, ma è possibile completare l'esercitazione installando il pacchetto SignalR NuGet, come descritto nei passaggi di installazione.</span><span class="sxs-lookup"><span data-stu-id="191ec-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="191ec-139">La procedura seguente utilizza Visual Studio 2012 per creare un'applicazione Web vuota ASP.NET e aggiungere la libreria SignalR:</span><span class="sxs-lookup"><span data-stu-id="191ec-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="191ec-140">In Visual Studio, creare un'applicazione Web ASP.NET vuota.</span><span class="sxs-lookup"><span data-stu-id="191ec-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Creare web vuoto](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="191ec-142">Aprire il **Package Manager Console** selezionando **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="191ec-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="191ec-143">Immettere il comando seguente nella finestra della console:</span><span class="sxs-lookup"><span data-stu-id="191ec-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="191ec-144">Questo comando consente di installare la versione più recente di SignalR 1. x.</span><span class="sxs-lookup"><span data-stu-id="191ec-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="191ec-145">In **Esplora**, fare clic sul progetto, selezionare **Aggiungi | Classe**.</span><span class="sxs-lookup"><span data-stu-id="191ec-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="191ec-146">Denominare la nuova classe **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="191ec-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="191ec-147">In **Esplora** espandere il nodo di script.</span><span class="sxs-lookup"><span data-stu-id="191ec-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="191ec-148">Librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="191ec-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Riferimenti alla libreria](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="191ec-150">Sostituire il codice di **ChatHub** classe con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="191ec-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="191ec-151">In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="191ec-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="191ec-152">Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona **classe di applicazione globale** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="191ec-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Aggiungere globale](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="191ec-154">Aggiungere il seguente `using` istruzioni dopo forniti `using` istruzioni nella classe Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="191ec-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="191ec-155">Aggiungere la riga di codice seguente il `Application_Start` metodo della classe globale per registrare la route predefinita per gli hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="191ec-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="191ec-156">In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="191ec-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="191ec-157">Nel **Aggiungi nuovo elemento** finestra di dialogo, selezionare la pagina Html e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="191ec-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="191ec-158">In **Esplora**, fare clic sulla pagina HTML appena creato e fare clic su **imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="191ec-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="191ec-159">Sostituire il codice predefinito nella pagina HTML con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="191ec-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="191ec-160">**Salvare tutti** per il progetto.</span><span class="sxs-lookup"><span data-stu-id="191ec-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="191ec-161">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="191ec-161">Run the Sample</span></span>

1. <span data-ttu-id="191ec-162">Premere F5 per eseguire il progetto in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="191ec-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="191ec-163">La pagina HTML viene caricato in un'istanza del browser e richiede un nome utente.</span><span class="sxs-lookup"><span data-stu-id="191ec-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="191ec-165">Immettere un nome utente.</span><span class="sxs-lookup"><span data-stu-id="191ec-165">Enter a user name.</span></span>
3. <span data-ttu-id="191ec-166">Copiare l'URL dalla riga dell'indirizzo del browser e utilizzarla per aprire le due altre istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="191ec-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="191ec-167">In ogni istanza del browser, immettere un nome utente univoco.</span><span class="sxs-lookup"><span data-stu-id="191ec-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="191ec-168">In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**.</span><span class="sxs-lookup"><span data-stu-id="191ec-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="191ec-169">I commenti deve essere visualizzato in tutte le istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="191ec-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="191ec-170">Questa applicazione di una chat semplice non gestisce il contesto della discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="191ec-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="191ec-171">L'hub trasmette i commenti per tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="191ec-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="191ec-172">Partecipare alla chat in un secondo momento gli utenti visualizzeranno messaggi aggiunti dal momento in cui che si connettono.</span><span class="sxs-lookup"><span data-stu-id="191ec-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="191ec-173">La schermata seguente viene illustrata l'applicazione di chat in esecuzione in tre istanze del browser, ognuno dei quali vengono aggiornati quando un'istanza invia un messaggio:</span><span class="sxs-lookup"><span data-stu-id="191ec-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Browser chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="191ec-175">In **Esplora**, controllare il **documenti Script** nodo per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="191ec-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="191ec-176">È presente un file script denominato **hub** che la libreria SignalR genera in modo dinamico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="191ec-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="191ec-177">Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.</span><span class="sxs-lookup"><span data-stu-id="191ec-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Script generato hub](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="191ec-179">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="191ec-179">Examine the Code</span></span>

<span data-ttu-id="191ec-180">L'applicazione di chat SignalR illustra due attività di sviluppo SignalR base: creazione di un hub come oggetto di coordinamento principale sul server e utilizza la libreria di jQuery SignalR per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="191ec-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="191ec-181">Hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="191ec-181">SignalR Hubs</span></span>

<span data-ttu-id="191ec-182">Nell'esempio di codice il **ChatHub** deriva dalla classe di **Microsoft.AspNet.SignalR.Hub** classe.</span><span class="sxs-lookup"><span data-stu-id="191ec-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="191ec-183">Derivazione da di **Hub** classe è utile per compilare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="191ec-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="191ec-184">È possibile creare metodi pubblici nella classe hub e quindi accedere a tali metodi mediante chiamata dagli script jQuery in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="191ec-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="191ec-185">Nel codice chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="191ec-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="191ec-186">L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="191ec-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="191ec-187">Il **inviare** metodo illustra i diversi concetti di hub:</span><span class="sxs-lookup"><span data-stu-id="191ec-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="191ec-188">Dichiarare i metodi pubblici in un hub in modo che i client possono essere richiamati.</span><span class="sxs-lookup"><span data-stu-id="191ec-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="191ec-189">Utilizzare il **Microsoft.AspNet.SignalR.Hub.Clients** proprietà dinamiche per accedere a tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="191ec-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="191ec-190">Chiamare una funzione di jQuery sul client (ad esempio il `broadcastMessage` (funzione)) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="191ec-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="191ec-191">SignalR e jQuery</span><span class="sxs-lookup"><span data-stu-id="191ec-191">SignalR and jQuery</span></span>

<span data-ttu-id="191ec-192">La pagina HTML nell'esempio di codice viene illustrato come utilizzare la libreria jQuery SignalR per comunicare con un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="191ec-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="191ec-193">Attività essenziali nel codice sta dichiarando un proxy per fare riferimento all'hub, dichiarazione di una funzione che il server è possibile chiamare per contenuto push ai client e all'avvio di una connessione per inviare messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="191ec-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="191ec-194">Il codice seguente dichiara un proxy per un hub.</span><span class="sxs-lookup"><span data-stu-id="191ec-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="191ec-195">In jQuery il riferimento alla classe del server e i relativi membri è in maiuscole-minuscole camel.</span><span class="sxs-lookup"><span data-stu-id="191ec-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="191ec-196">L'esempio di codice fa riferimento a c# **ChatHub** classe jQuery come **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="191ec-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="191ec-197">Il codice seguente è come si crea una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="191ec-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="191ec-198">Classe dell'hub sul server chiama questa funzione per inviare gli aggiornamenti del contenuto di ogni client.</span><span class="sxs-lookup"><span data-stu-id="191ec-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="191ec-199">Le due righe che HTML codificare il contenuto prima di visualizzarlo sono facoltative e mostrano un modo semplice per impedire attacchi script injection.</span><span class="sxs-lookup"><span data-stu-id="191ec-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="191ec-200">Il codice seguente viene illustrato come aprire una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="191ec-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="191ec-201">Il codice avvia la connessione e passa a una funzione per gestire l'evento click nel **inviare** nella pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="191ec-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="191ec-202">Questo approccio assicura che la connessione viene stabilita prima dell'esecuzione il gestore dell'evento.</span><span class="sxs-lookup"><span data-stu-id="191ec-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="191ec-203">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="191ec-203">Next Steps</span></span>

<span data-ttu-id="191ec-204">Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="191ec-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="191ec-205">È stato inoltre diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.</span><span class="sxs-lookup"><span data-stu-id="191ec-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="191ec-206">È possibile rendere l'applicazione di esempio in questa esercitazione o altre applicazioni SignalR disponibili su Internet distribuendole da un provider di hosting.</span><span class="sxs-lookup"><span data-stu-id="191ec-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="191ec-207">Microsoft offre l'hosting web gratuito per fino a 10 siti web gratuito [account di valutazione di Windows Azure](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="191ec-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="191ec-208">Per una procedura dettagliata su come distribuire l'applicazione di esempio SignalR, vedere [pubblicare il SignalR esempio della Guida introduttiva di un sito Web di Microsoft Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="191ec-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="191ec-209">Per informazioni dettagliate su come distribuire un progetto web di Visual Studio a un sito Web di Windows Azure, vedere [si distribuisce un'applicazione ASP.NET a un sito Web di Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="191ec-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="191ec-210">(Nota: il trasporto WebSocket non è attualmente supportato per siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="191ec-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="191ec-211">Trasporto WebSocket quando non è disponibile, SignalR utilizza gli altri trasporti disponibili come descritto nella sezione di trasporti di [Introduzione all'argomento SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="191ec-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="191ec-212">Per informazioni su concetti più avanzati di sviluppi SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:</span><span class="sxs-lookup"><span data-stu-id="191ec-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="191ec-213">Progetto SignalR</span><span class="sxs-lookup"><span data-stu-id="191ec-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="191ec-214">SignalR Github ed esempi</span><span class="sxs-lookup"><span data-stu-id="191ec-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="191ec-215">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="191ec-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
