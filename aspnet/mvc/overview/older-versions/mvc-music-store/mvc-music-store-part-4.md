---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: Accesso ai dati e i modelli | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 4 riguarda l'accesso ai dati e modelli.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="32e15-104">Parte 4: Accesso ai dati e i modelli</span><span class="sxs-lookup"><span data-stu-id="32e15-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="32e15-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="32e15-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="32e15-106">L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="32e15-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="32e15-107">L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="32e15-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="32e15-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio.</span><span class="sxs-lookup"><span data-stu-id="32e15-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="32e15-109">Parte 4 riguarda l'accesso ai dati e modelli.</span><span class="sxs-lookup"><span data-stu-id="32e15-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="32e15-110">Fino a questo punto, è stato stato passando solo "dati fittizi" dai nostri controller per i modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="32e15-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="32e15-111">Ora si è pronti per collegare un database reale.</span><span class="sxs-lookup"><span data-stu-id="32e15-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="32e15-112">In questa esercitazione parleremo illustrato come utilizzare SQL Server Compact Edition (spesso chiamato SQL CE) come il motore di database.</span><span class="sxs-lookup"><span data-stu-id="32e15-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="32e15-113">SQL CE è un database gratuito incorporato, i file in base che non richiede una configurazione, che rende molto semplice per lo sviluppo locale o l'installazione.</span><span class="sxs-lookup"><span data-stu-id="32e15-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="32e15-114">Accesso al database con Entity Framework Code-First</span><span class="sxs-lookup"><span data-stu-id="32e15-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="32e15-115">Verrà usato il supporto di Entity Framework (EF) incluso in progetti ASP.NET MVC 3 per eseguire query e aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="32e15-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="32e15-116">Entity Framework è un oggetto flessibile relazionale mapping API che consente agli sviluppatori di query e aggiornare i dati archiviati in un database in modalità orientata agli oggetti di dati (ORM).</span><span class="sxs-lookup"><span data-stu-id="32e15-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="32e15-117">Entity Framework versione 4 supporta un paradigma di sviluppo chiamato prima di codice.</span><span class="sxs-lookup"><span data-stu-id="32e15-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="32e15-118">Prima di codice consente di creare l'oggetto modello mediante la scrittura di classi semplice (noto anche come POCO da oggetti CLR "plain-old") e può anche creare il database al momento dalle classi.</span><span class="sxs-lookup"><span data-stu-id="32e15-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="32e15-119">Modifiche per il modello di classi</span><span class="sxs-lookup"><span data-stu-id="32e15-119">Changes to our Model Classes</span></span>

<span data-ttu-id="32e15-120">In questa esercitazione, si verrà sfruttando la funzionalità di creazione del database in Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="32e15-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="32e15-121">Prima di procedere è tuttavia verifichiamo alcune lievi modifiche per il nostro classi del modello per aggiungere alcuni elementi che verrà usato in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="32e15-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="32e15-122">Aggiunta di classi del modello artista</span><span class="sxs-lookup"><span data-stu-id="32e15-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="32e15-123">Il nostro album verrà associato a artisti, pertanto verrà aggiunto una classe di modello semplice per descrivere un artista.</span><span class="sxs-lookup"><span data-stu-id="32e15-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="32e15-124">Aggiungere una nuova classe nella cartella Models denominato Artist.cs utilizzando il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="32e15-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="32e15-125">Aggiornare il nostro classi modello</span><span class="sxs-lookup"><span data-stu-id="32e15-125">Updating our Model Classes</span></span>

<span data-ttu-id="32e15-126">Aggiornare la classe Album come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="32e15-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="32e15-127">Successivamente, effettuare gli aggiornamenti seguenti alla classe genere.</span><span class="sxs-lookup"><span data-stu-id="32e15-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="32e15-128">Aggiunta dell'App\_cartella dati</span><span class="sxs-lookup"><span data-stu-id="32e15-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="32e15-129">Verrà aggiunto una App\_directory dei dati al progetto per l'archiviazione dei file di database SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="32e15-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="32e15-130">App\_dati sono una directory speciale in ASP.NET che ha già le autorizzazioni di accesso di sicurezza corrette per l'accesso al database.</span><span class="sxs-lookup"><span data-stu-id="32e15-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="32e15-131">Dal menu progetto, selezionare Aggiungi cartella ASP.NET, quindi App\_dati.</span><span class="sxs-lookup"><span data-stu-id="32e15-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="32e15-132">Creazione di una stringa di connessione nel file Web. config</span><span class="sxs-lookup"><span data-stu-id="32e15-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="32e15-133">Si aggiungerà alcune righe al file di configurazione del sito Web in modo che Entity Framework sia in grado di connettersi al database.</span><span class="sxs-lookup"><span data-stu-id="32e15-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="32e15-134">Fare doppio clic sul file Web. config situato nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="32e15-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="32e15-135">Scorrere fino alla fine di questo file e aggiungere un &lt;connectionStrings&gt; sezione direttamente sopra l'ultima riga, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="32e15-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="32e15-136">Aggiunta di una classe di contesto</span><span class="sxs-lookup"><span data-stu-id="32e15-136">Adding a Context Class</span></span>

<span data-ttu-id="32e15-137">Fare clic sulla cartella di modelli e aggiungere una nuova classe denominata MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="32e15-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="32e15-138">Questa classe verrà rappresenta il contesto di database di Entity Framework ed eseguiranno gestire la creazione, leggere, aggiornare ed eliminare operazioni per Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32e15-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="32e15-139">Il codice per questa classe è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="32e15-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="32e15-140">Questo è tutto, non c'è alcun altro configurazione speciali interfacce, e così via. Estendendo la classe di base DbContext, la classe MusicStoreEntities è in grado di gestire le nostre operazioni di database per noi.</span><span class="sxs-lookup"><span data-stu-id="32e15-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="32e15-141">Ora che abbiamo che agganciato, aggiungere alcune proprietà aggiuntive per il nostro classi del modello per sfruttare alcune delle informazioni aggiuntive nel database.</span><span class="sxs-lookup"><span data-stu-id="32e15-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="32e15-142">Aggiungere i dati del catalogo di archivio</span><span class="sxs-lookup"><span data-stu-id="32e15-142">Adding our store catalog data</span></span>

<span data-ttu-id="32e15-143">Si sarà possibile avvalersi di una funzionalità in Entity Framework e aggiunge i dati di "valore di inizializzazione" a un database appena creato.</span><span class="sxs-lookup"><span data-stu-id="32e15-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="32e15-144">Questo verrà pre-popolare il catalogo di archivio con un elenco di generi, artisti e album.</span><span class="sxs-lookup"><span data-stu-id="32e15-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="32e15-145">Il download MvcMusicStore Assets.zip - che include i file di progettazione del sito utilizzati in precedenza in questa esercitazione, con un file di classe con dati questo valore di inizializzazione, che si trova in una cartella denominata codice.</span><span class="sxs-lookup"><span data-stu-id="32e15-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="32e15-146">All'interno del codice / cartella Models, individuare il file SampleData.cs e rilasciarla nella cartella modelli del progetto, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="32e15-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="32e15-147">È necessario aggiungere una riga di codice per informare tale classe SampleData di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="32e15-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="32e15-148">Fare doppio clic sul file Global. asax nella radice del progetto per aprirlo e aggiungere la riga seguente all'inizio dell'applicazione\_Start (metodo).</span><span class="sxs-lookup"><span data-stu-id="32e15-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="32e15-149">A questo punto, abbiamo completato il lavoro necessario per configurare Entity Framework per il progetto.</span><span class="sxs-lookup"><span data-stu-id="32e15-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="32e15-150">Esecuzione di query sul database</span><span class="sxs-lookup"><span data-stu-id="32e15-150">Querying the Database</span></span>

<span data-ttu-id="32e15-151">Ora si aggiorna il nostro StoreController in modo che anziché "dati fittizio" chiami invece al database per eseguire una query tutte le informazioni di.</span><span class="sxs-lookup"><span data-stu-id="32e15-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="32e15-152">Inizieremo con la dichiarazione di un campo nel **StoreController** per contenere un'istanza della classe MusicStoreEntities, denominata storeDB:</span><span class="sxs-lookup"><span data-stu-id="32e15-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="32e15-153">Aggiornamento dell'indice di archivio per eseguire query sul database</span><span class="sxs-lookup"><span data-stu-id="32e15-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="32e15-154">La classe MusicStoreEntities viene mantenuta da Entity Framework ed espone una proprietà di raccolta per ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="32e15-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="32e15-155">Si aggiorna azione di indice del nostro StoreController per recuperare tutti i generi nel database.</span><span class="sxs-lookup"><span data-stu-id="32e15-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="32e15-156">In precedenza è eseguita questa operazione a livello di codice i dati di stringa.</span><span class="sxs-lookup"><span data-stu-id="32e15-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="32e15-157">Ora è possibile invece utilizzare il contesto di Entity Framework Generes raccolta:</span><span class="sxs-lookup"><span data-stu-id="32e15-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="32e15-158">Nessuna modifica desidera essere eseguite per il modello di visualizzazione poiché il StoreIndexViewModel stesso è restituiti prima - viene semplicemente restituito dati in tempo reale dal database ora viene comunque restituito.</span><span class="sxs-lookup"><span data-stu-id="32e15-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="32e15-159">Quando si esegue nuovamente il progetto e visitare l'URL "e delle archiviazioni", è ora verrà visualizzato un elenco di tutti i generi nel database:</span><span class="sxs-lookup"><span data-stu-id="32e15-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="32e15-160">Aggiornamento di archivio Sfoglia e i dettagli per utilizzare i dati in tempo reale</span><span class="sxs-lookup"><span data-stu-id="32e15-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="32e15-161">Con l'archivio/Sfoglia? genre =*[alcune genre]* metodo di azione, si sta cercando un genere in base al nome.</span><span class="sxs-lookup"><span data-stu-id="32e15-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="32e15-162">È previsto solo un risultato, poiché non dovrebbero essere sempre due voci per lo stesso nome Genre e pertanto è possibile utilizzare il. Estensione Single() nelle query LINQ per eseguire query per l'oggetto appropriato genere simile al seguente (non digitare quanto segue ancora):</span><span class="sxs-lookup"><span data-stu-id="32e15-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="32e15-163">L'unico metodo accetta un'espressione Lambda come un parametro, che indica che si desidera che un singolo oggetto Genre in modo che il nome corrisponde al valore, viene definito.</span><span class="sxs-lookup"><span data-stu-id="32e15-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="32e15-164">Nel caso precedente, ci stiamo durante il caricamento di un singolo oggetto Genre con un valore di nome corrispondente a Disco.</span><span class="sxs-lookup"><span data-stu-id="32e15-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="32e15-165">L'utente verrà reindirizzato a una funzionalità di Entity Framework che consente di indicare altre entità correlate desideriamo caricare anche quando l'oggetto Genre viene recuperato.</span><span class="sxs-lookup"><span data-stu-id="32e15-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="32e15-166">Questa funzionalità viene chiamata forma del risultato di Query e consente di ridurre il numero di volte in cui che è necessario accedere al database per recuperare tutte le informazioni che necessarie.</span><span class="sxs-lookup"><span data-stu-id="32e15-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="32e15-167">Si desidera recuperare preventivamente gli album per genere che è possibile recuperare, pertanto verrà aggiornata la query per includere da Genres.Include("Albums") per indicare che si desidera anche album correlati.</span><span class="sxs-lookup"><span data-stu-id="32e15-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="32e15-168">Si tratta di una più efficiente, poiché consente di recuperare i dati del genere e di Album nella richiesta singolo database.</span><span class="sxs-lookup"><span data-stu-id="32e15-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="32e15-169">Con spiegazioni dall'area di lavoro, ecco come apparirà la l'azione del controller Sfoglia aggiornata:</span><span class="sxs-lookup"><span data-stu-id="32e15-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="32e15-170">È ora possibile aggiornare l'archivio di esplorazione della vista per visualizzare album disponibili in ogni genere.</span><span class="sxs-lookup"><span data-stu-id="32e15-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="32e15-171">Aprire il modello di visualizzazione (presente in /Views/Store/Browse.cshtml) e aggiungere un elenco puntato di album, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="32e15-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="32e15-172">In esecuzione l'applicazione e la selezione di archivio/Sfoglia? genre = Mostra Jazz che i risultati sono ora rimosse dal database, la visualizzazione di tutti gli album questo genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="32e15-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="32e15-173">Verrà effettuata la stessa modifica per il nostro /Store/dettagli / [id] URL e sostituire i dati fittizi con una query sul database che carica un Album il cui ID corrisponde al valore di parametro.</span><span class="sxs-lookup"><span data-stu-id="32e15-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="32e15-174">In esecuzione l'applicazione e la selezione di /Store/Details/1 mostra che i risultati sono ora rimosse dal database.</span><span class="sxs-lookup"><span data-stu-id="32e15-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="32e15-175">Dopo che la pagina Dettagli archivio è impostata per visualizzare un album dall'ID di Album, si aggiorna il **Sfoglia** Visualizza il collegamento alla visualizzazione dettagli.</span><span class="sxs-lookup"><span data-stu-id="32e15-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="32e15-176">Si utilizzerà HTML. ActionLink, esattamente come è stato fatto per creare un collegamento dall'indice di archivio per archivio passare alla fine della sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="32e15-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="32e15-177">Di seguito è riportata l'origine completa per la vista di esplorazione.</span><span class="sxs-lookup"><span data-stu-id="32e15-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="32e15-178">A questo punto siamo in grado di individuare la pagina archivio una pagina del genere, che elenca gli album di disponibili, e facendo clic su un album è possibile visualizzare i dettagli per tale album.</span><span class="sxs-lookup"><span data-stu-id="32e15-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="32e15-179">[Precedente](mvc-music-store-part-3.md)
> [Successivo](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="32e15-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
