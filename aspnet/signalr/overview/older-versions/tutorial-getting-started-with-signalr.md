---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Esercitazione: Introduzione a SignalR 1.x | Microsoft Docs'
author: pfletcher
description: Usare SignalR ASP.NET per compilare un'applicazione di chat in tempo reale in una pagina HTML.
ms.author: riande
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 720a4879f5fbe3c0c2b4c7809cb94c22547329c3
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287363"
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="02e64-103">Esercitazione: Introduzione a SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="02e64-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="02e64-104">dal [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="02e64-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="02e64-105">In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="02e64-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="02e64-106">Verrà aggiunta SignalR a un'applicazione web ASP.NET vuota e creare una pagina HTML per l'invio e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="02e64-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="02e64-107">Panoramica</span><span class="sxs-lookup"><span data-stu-id="02e64-107">Overview</span></span>

<span data-ttu-id="02e64-108">Questa esercitazione introduce lo sviluppo di SignalR, che illustrano come creare un'applicazione di chat basata su browser semplice.</span><span class="sxs-lookup"><span data-stu-id="02e64-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="02e64-109">Si verrà aggiungere la libreria di SignalR a un'applicazione web ASP.NET vuota, creare una classe di hub per l'invio di messaggi per i client e creare una pagina HTML che consente agli utenti di inviare e ricevere i messaggi di chat.</span><span class="sxs-lookup"><span data-stu-id="02e64-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="02e64-110">Per un'esercitazione simile che mostra come creare un'applicazione di chat in MVC 4 con una visualizzazione MVC, vedere [Introduzione a SignalR e MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="02e64-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="02e64-111">Questa esercitazione Usa la versione finale (1.x) di SignalR.</span><span class="sxs-lookup"><span data-stu-id="02e64-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="02e64-112">Per informazioni dettagliate sulle modifiche apportate tra SignalR 1.x e 2.0, vedere [l'aggiornamento di SignalR 1.x progetti](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="02e64-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="02e64-113">SignalR è una libreria .NET open source per la compilazione di applicazioni web che richiedono l'interazione dell'utente in tempo reale o gli aggiornamenti dei dati in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="02e64-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="02e64-114">Ad esempio le applicazioni basati su social network, giochi multiutente, meteo, collaborazione e nelle novità, business o finanziari aggiornare le applicazioni.</span><span class="sxs-lookup"><span data-stu-id="02e64-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="02e64-115">Spesso si tratta di applicazioni in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="02e64-115">These are often called real-time applications.</span></span>

<span data-ttu-id="02e64-116">SignalR semplifica il processo di compilazione di applicazioni in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="02e64-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="02e64-117">Include una raccolta di server ASP.NET e una libreria client JavaScript per renderne più semplice gestire le connessioni client a server e il push degli aggiornamenti del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="02e64-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="02e64-118">È possibile aggiungere la libreria di SignalR a un'applicazione ASP.NET esistente per ottenere funzionalità in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="02e64-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="02e64-119">L'esercitazione illustra le attività di sviluppo SignalR seguenti:</span><span class="sxs-lookup"><span data-stu-id="02e64-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="02e64-120">Aggiunta della libreria di SignalR a un'applicazione web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="02e64-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="02e64-121">Creazione di una classe di hub per eseguire il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="02e64-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="02e64-122">Usa la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.</span><span class="sxs-lookup"><span data-stu-id="02e64-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="02e64-123">Lo screenshot seguente mostra l'applicazione di chat in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="02e64-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="02e64-124">Ogni nuovo utente è possibile inviare commenti e vedere i commenti aggiunti dopo che l'utente viene aggiunto alla chat.</span><span class="sxs-lookup"><span data-stu-id="02e64-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Istanze di chat](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="02e64-126">Nelle sezioni:</span><span class="sxs-lookup"><span data-stu-id="02e64-126">Sections:</span></span>

- [<span data-ttu-id="02e64-127">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="02e64-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="02e64-128">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="02e64-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="02e64-129">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="02e64-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="02e64-130">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="02e64-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="02e64-131">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="02e64-131">Set up the Project</span></span>

<span data-ttu-id="02e64-132">In questa sezione viene illustrato come creare un'applicazione web ASP.NET vuota, aggiungere SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="02e64-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="02e64-133">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="02e64-133">Prerequisites:</span></span>

- <span data-ttu-id="02e64-134">Visual Studio 2010 SP1 o 2012.</span><span class="sxs-lookup"><span data-stu-id="02e64-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="02e64-135">Se non hai Visual Studio, vedere [download ASP.NET](https://www.asp.net/downloads) per ottenere il Visual Studio 2012 Express strumento di sviluppo gratuito.</span><span class="sxs-lookup"><span data-stu-id="02e64-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="02e64-136">[Microsoft ASP.NET e Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="02e64-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="02e64-137">Per Visual Studio 2012, il programma di installazione aggiunge nuove funzionalità ASP.NET, inclusi i modelli di SignalR per Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="02e64-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="02e64-138">Per Visual Studio 2010 SP1, non è disponibile un programma di installazione, ma è possibile completare l'esercitazione installando il pacchetto NuGet di SignalR, come descritto nei passaggi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="02e64-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="02e64-139">La procedura seguente Usa Visual Studio 2012 per creare un'applicazione Web ASP.NET vuota e aggiungere la libreria di SignalR:</span><span class="sxs-lookup"><span data-stu-id="02e64-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="02e64-140">In Visual Studio creare un'applicazione Web ASP.NET vuota.</span><span class="sxs-lookup"><span data-stu-id="02e64-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Creazione di web vuoto](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="02e64-142">Aprire il **Console di gestione pacchetti** selezionando **strumenti | Gestione pacchetti NuGet | Console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="02e64-142">Open the **Package Manager Console** by selecting **Tools | NuGet Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="02e64-143">Immettere il comando seguente nella finestra della console:</span><span class="sxs-lookup"><span data-stu-id="02e64-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="02e64-144">Questo comando installa la versione più recente di SignalR 1.x.</span><span class="sxs-lookup"><span data-stu-id="02e64-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="02e64-145">Nelle **Esplora soluzioni**, fare clic sul progetto, selezionare **Add | Classe**.</span><span class="sxs-lookup"><span data-stu-id="02e64-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="02e64-146">Denominare la nuova classe **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="02e64-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="02e64-147">Nelle **Esplora soluzioni** espandere il nodo di script.</span><span class="sxs-lookup"><span data-stu-id="02e64-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="02e64-148">Librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="02e64-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Riferimenti alla libreria](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="02e64-150">Sostituire il codice nel **ChatHub** classe con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="02e64-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="02e64-151">Nelle **Esplora soluzioni**, fare clic sul progetto, quindi fare clic su **Add | Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="02e64-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="02e64-152">Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona **classe di applicazione globale** e fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="02e64-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Aggiungere globale](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="02e64-154">Aggiungere il codice seguente `using` istruzioni dopo l'oggetto fornito `using` le istruzioni nella classe Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="02e64-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="02e64-155">Aggiungere la riga di codice seguente il `Application_Start` metodo della classe globale per registrare la route predefinita per gli hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="02e64-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="02e64-156">Nelle **Esplora soluzioni**, fare clic sul progetto, quindi fare clic su **Add | Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="02e64-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="02e64-157">Nel **Aggiungi nuovo elemento** finestra di dialogo, selezionare la pagina Html e fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="02e64-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="02e64-158">Nelle **Esplora soluzioni**, fare doppio clic su pagina HTML appena creato e fare clic su **imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="02e64-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="02e64-159">Sostituire il codice predefinito nella pagina HTML con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="02e64-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="02e64-160">**Salva tutto** per il progetto.</span><span class="sxs-lookup"><span data-stu-id="02e64-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="02e64-161">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="02e64-161">Run the Sample</span></span>

1. <span data-ttu-id="02e64-162">Premere F5 per eseguire il progetto in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="02e64-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="02e64-163">La pagina HTML viene caricata in un'istanza del browser e richiede un nome utente.</span><span class="sxs-lookup"><span data-stu-id="02e64-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="02e64-165">Immettere un nome utente.</span><span class="sxs-lookup"><span data-stu-id="02e64-165">Enter a user name.</span></span>
3. <span data-ttu-id="02e64-166">Copiare l'URL dalla riga dell'indirizzo del browser e usarlo per aprire due altre istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="02e64-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="02e64-167">In ogni istanza del browser immettere un nome utente univoco.</span><span class="sxs-lookup"><span data-stu-id="02e64-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="02e64-168">In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**.</span><span class="sxs-lookup"><span data-stu-id="02e64-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="02e64-169">I commenti deve essere visualizzato in tutte le istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="02e64-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="02e64-170">Questa applicazione di chat semplice non gestisce il contesto di discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="02e64-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="02e64-171">L'hub trasmette i commenti per tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="02e64-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="02e64-172">Gli utenti che partecipano alla chat in un secondo momento visualizzeranno messaggi aggiunti dal momento in cui che questi vengono aggiunti.</span><span class="sxs-lookup"><span data-stu-id="02e64-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="02e64-173">Lo screenshot seguente mostra l'applicazione di chat in esecuzione in tre istanze del browser, ognuno dei quali vengono aggiornate quando un'istanza di invia un messaggio:</span><span class="sxs-lookup"><span data-stu-id="02e64-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Browser di chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="02e64-175">Nelle **Esplora soluzioni**, esaminare le **documenti Script** nodo per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="02e64-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="02e64-176">È presente un file di script denominato **hub** che la libreria di SignalR genera in modo dinamico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="02e64-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="02e64-177">Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.</span><span class="sxs-lookup"><span data-stu-id="02e64-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Script generato hub](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="02e64-179">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="02e64-179">Examine the Code</span></span>

<span data-ttu-id="02e64-180">L'applicazione di chat SignalR illustra due attività di sviluppo di SignalR base: creazione di un hub come oggetto di coordinamento principale nel server e usando la libreria jQuery SignalR per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="02e64-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="02e64-181">Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="02e64-181">SignalR Hubs</span></span>

<span data-ttu-id="02e64-182">Nell'esempio di codice le **ChatHub** deriva dalla classe la **Microsoft.AspNet.SignalR.Hub** classe.</span><span class="sxs-lookup"><span data-stu-id="02e64-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="02e64-183">Derivazione dal **Hub** classe è un modo utile per compilare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="02e64-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="02e64-184">È possibile creare metodi pubblici nella classe dell'hub e quindi accedere a tali metodi chiamandole dagli script jQuery in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="02e64-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="02e64-185">Nel codice di chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="02e64-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="02e64-186">L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="02e64-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="02e64-187">Il **inviare** metodo illustra alcuni concetti di hub:</span><span class="sxs-lookup"><span data-stu-id="02e64-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="02e64-188">Dichiarare i metodi pubblici in un hub in modo che i client possono chiamare li.</span><span class="sxs-lookup"><span data-stu-id="02e64-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="02e64-189">Usare la **Microsoft.AspNet.SignalR.Hub.Clients** proprietà dinamiche per accedere a tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="02e64-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="02e64-190">Chiamare una funzione di jQuery nel client (ad esempio il `broadcastMessage` (funzione)) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="02e64-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="02e64-191">SignalR e jQuery</span><span class="sxs-lookup"><span data-stu-id="02e64-191">SignalR and jQuery</span></span>

<span data-ttu-id="02e64-192">La pagina HTML nel codice di esempio mostra come usare la libreria jQuery SignalR per comunicare con un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="02e64-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="02e64-193">Le attività di base nel codice siano dichiarando un proxy per l'hub, dichiara una funzione che il server può chiamare per contenuto push ai client e l'avvio di una connessione per inviare messaggi all'hub di riferimento.</span><span class="sxs-lookup"><span data-stu-id="02e64-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="02e64-194">Il codice seguente dichiara un proxy per un hub.</span><span class="sxs-lookup"><span data-stu-id="02e64-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="02e64-195">In jQuery il riferimento alla classe server e i relativi membri è in maiuscole/minuscole camel.</span><span class="sxs-lookup"><span data-stu-id="02e64-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="02e64-196">Nell'esempio di codice fa riferimento il codice c# **ChatHub** classe in jQuery come **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="02e64-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="02e64-197">Il codice seguente è come si crea una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="02e64-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="02e64-198">La classe dell'hub sul server chiama questa funzione per eseguire il push degli aggiornamenti di contenuto a ogni client.</span><span class="sxs-lookup"><span data-stu-id="02e64-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="02e64-199">Le due righe che HTML codificare il contenuto prima di visualizzarla sono facoltative e mostrano un modo semplice per impedire attacchi script injection.</span><span class="sxs-lookup"><span data-stu-id="02e64-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="02e64-200">Il codice seguente viene illustrato come aprire una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="02e64-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="02e64-201">Il codice avvia la connessione e quindi lo si passa una funzione per gestire l'evento click sulle **inviare** pulsante nella pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="02e64-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="02e64-202">Questo approccio assicura che la connessione viene stabilita prima esecuzione del gestore dell'evento.</span><span class="sxs-lookup"><span data-stu-id="02e64-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="02e64-203">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="02e64-203">Next Steps</span></span>

<span data-ttu-id="02e64-204">Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="02e64-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="02e64-205">Si è appreso anche diverse attività di sviluppo di SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.</span><span class="sxs-lookup"><span data-stu-id="02e64-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="02e64-206">È possibile rendere l'applicazione di esempio in questa esercitazione o altre applicazioni SignalR disponibile tramite Internet per la distribuzione in un provider di hosting.</span><span class="sxs-lookup"><span data-stu-id="02e64-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="02e64-207">Microsoft offre l'hosting web gratuito per fino a 10 siti web in una liberazione [account di valutazione di Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="02e64-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="02e64-208">Per una procedura dettagliata su come distribuire l'applicazione di esempio SignalR, vedere [pubblicare i SignalR esempio della Guida introduttiva come un sito Web di Microsoft Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="02e64-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="02e64-209">Per informazioni dettagliate su come distribuire un progetto web Visual Studio per un sito Web di Microsoft Azure, vedere [distribuzione di un'applicazione ASP.NET a un sito Web di Microsoft Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="02e64-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="02e64-210">(Nota: Il trasporto WebSocket non è attualmente supportato per siti Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="02e64-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="02e64-211">Il trasporto WebSocket quando non è disponibile, SignalR utilizza altri trasporti disponibili come descritto nella sezione dei trasporti del [Introduzione a SignalR argomento](index.md).)</span><span class="sxs-lookup"><span data-stu-id="02e64-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="02e64-212">Per informazioni su concetti più avanzati sviluppi in materia di SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:</span><span class="sxs-lookup"><span data-stu-id="02e64-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="02e64-213">Progetto di SignalR</span><span class="sxs-lookup"><span data-stu-id="02e64-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="02e64-214">SignalR Github ed esempi</span><span class="sxs-lookup"><span data-stu-id="02e64-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="02e64-215">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="02e64-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
