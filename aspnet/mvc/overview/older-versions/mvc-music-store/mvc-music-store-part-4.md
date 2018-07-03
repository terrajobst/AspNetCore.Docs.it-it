---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: Accesso ai dati e modelli | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 4 illustra i modelli e accesso ai dati.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: ea8fe623a1b59b80fd7f087036b9ed716eafadbe
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402038"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="d3c75-104">Parte 4: Modelli e accesso ai dati</span><span class="sxs-lookup"><span data-stu-id="d3c75-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="d3c75-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="d3c75-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="d3c75-106">Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="d3c75-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="d3c75-107">La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="d3c75-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="d3c75-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="d3c75-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="d3c75-109">Parte 4 illustra i modelli e accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="d3c75-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="d3c75-110">Finora, abbiamo abbiamo stato passando solo "dati fittizi" verso i controller per i modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d3c75-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="d3c75-111">È ora possibile collegare un database effettivo.</span><span class="sxs-lookup"><span data-stu-id="d3c75-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="d3c75-112">In questa esercitazione si parlerà come usare SQL Server Compact Edition (spesso chiamati SQL CE) come il motore di database.</span><span class="sxs-lookup"><span data-stu-id="d3c75-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="d3c75-113">SQL CE è un database gratuito incorporato, file basati su che non richiede alcuna installazione o configurazione, che rende molto semplice per lo sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="d3c75-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="d3c75-114">Accesso al database con Entity Framework Code First</span><span class="sxs-lookup"><span data-stu-id="d3c75-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="d3c75-115">Si userà il supporto di Entity Framework (EF) che è incluso in progetti ASP.NET MVC 3 per eseguire query e aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="d3c75-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="d3c75-116">Entity Framework è un oggetto flessibile relazionale mapping (ORM) dei dati API che consente agli sviluppatori di eseguire query e aggiornare i dati archiviati in un database in una modalità orientata agli oggetti.</span><span class="sxs-lookup"><span data-stu-id="d3c75-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="d3c75-117">Entity Framework versione 4 supporta un paradigma di sviluppo denominato code first.</span><span class="sxs-lookup"><span data-stu-id="d3c75-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="d3c75-118">Code first consente di creare l'oggetto modello mediante la scrittura di classi semplice (noto anche come POCO da "plain-old" CLR Object) e può anche creare il database in tempo reale dalle classi.</span><span class="sxs-lookup"><span data-stu-id="d3c75-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="d3c75-119">Modifiche per le classi di modello</span><span class="sxs-lookup"><span data-stu-id="d3c75-119">Changes to our Model Classes</span></span>

<span data-ttu-id="d3c75-120">La funzionalità per la creazione del database in Entity Framework verrà usato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d3c75-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="d3c75-121">Prima di procedere, tuttavia, creiamo lievi modifiche per le classi di modello di componente aggiuntivo di alcuni aspetti che verrà usato in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="d3c75-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="d3c75-122">Aggiunta di classi del modello artista</span><span class="sxs-lookup"><span data-stu-id="d3c75-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="d3c75-123">Nostro album verranno associati alle creazioni degli artisti, quindi si aggiungerà una classe di modello semplice per descrivere un artista.</span><span class="sxs-lookup"><span data-stu-id="d3c75-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="d3c75-124">Aggiungere una nuova classe nella cartella Models denominato Artist.cs usando il codice illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d3c75-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="d3c75-125">Aggiornare le classi di modello</span><span class="sxs-lookup"><span data-stu-id="d3c75-125">Updating our Model Classes</span></span>

<span data-ttu-id="d3c75-126">Aggiornare la classe di Album come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d3c75-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="d3c75-127">Successivamente, apportare le modifiche seguenti alla classe del genere.</span><span class="sxs-lookup"><span data-stu-id="d3c75-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="d3c75-128">Aggiunta dell'App\_cartella di dati</span><span class="sxs-lookup"><span data-stu-id="d3c75-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="d3c75-129">Si aggiungerà un'App\_directory dei dati per il progetto per contenere i file di database SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="d3c75-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="d3c75-130">App\_dati sono una directory speciale in ASP.NET che ha già le autorizzazioni di accesso di sicurezza corrette per l'accesso al database.</span><span class="sxs-lookup"><span data-stu-id="d3c75-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="d3c75-131">Dal menu progetto, selezionare Aggiungi cartella ASP.NET, quindi App\_dei dati.</span><span class="sxs-lookup"><span data-stu-id="d3c75-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="d3c75-132">Creazione di una stringa di connessione nel file Web. config</span><span class="sxs-lookup"><span data-stu-id="d3c75-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="d3c75-133">Si aggiungerà alcune righe al file di configurazione del sito Web in modo che Entity Framework sia in grado di connettersi al database.</span><span class="sxs-lookup"><span data-stu-id="d3c75-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="d3c75-134">Fare doppio clic sul file Web. config situato nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="d3c75-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="d3c75-135">Scorrere fino alla fine di questo file e aggiungere una &lt;connectionStrings&gt; sezione immediatamente sopra l'ultima riga, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d3c75-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="d3c75-136">Aggiunta di una classe di contesto</span><span class="sxs-lookup"><span data-stu-id="d3c75-136">Adding a Context Class</span></span>

<span data-ttu-id="d3c75-137">Fare doppio clic su cartella Models e aggiungere una nuova classe denominata MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="d3c75-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="d3c75-138">Questa classe verrà rappresentano il contesto del database, Entity Framework e verrà gestire la creazione, lettura, operazioni aggiornamento ed eliminazione per noi.</span><span class="sxs-lookup"><span data-stu-id="d3c75-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="d3c75-139">Seguito è riportato il codice per questa classe.</span><span class="sxs-lookup"><span data-stu-id="d3c75-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="d3c75-140">Questo è tutto, non vi è alcun altro configurazione speciale interfacce, e così via. Con l'estensione della classe base DbContext, la classe MusicStoreEntities è in grado di gestire le operazioni di database per noi.</span><span class="sxs-lookup"><span data-stu-id="d3c75-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="d3c75-141">A questo punto, abbiamo che collegato, è possibile aggiungere alcune altre proprietà per le classi di modello possa sfruttare i vantaggi di alcune informazioni aggiuntive nel nostro database.</span><span class="sxs-lookup"><span data-stu-id="d3c75-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="d3c75-142">Aggiungere i dati di catalogo di archivio</span><span class="sxs-lookup"><span data-stu-id="d3c75-142">Adding our store catalog data</span></span>

<span data-ttu-id="d3c75-143">Si sarà possibile avvalersi di una funzionalità in Entity Framework che consente di aggiungere dati di "valore di inizializzazione" a un database appena creato.</span><span class="sxs-lookup"><span data-stu-id="d3c75-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="d3c75-144">Ciò popola preventivamente il nostro catalogo di archivio con un elenco di generi e gli artisti album.</span><span class="sxs-lookup"><span data-stu-id="d3c75-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="d3c75-145">Il download MvcMusicStore Assets.zip - che comprendeva i file di progettazione del sito usati in precedenza in questa esercitazione - dispone di un file di classe con questi dati di valore di inizializzazione, che si trova in una cartella denominata codice.</span><span class="sxs-lookup"><span data-stu-id="d3c75-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="d3c75-146">All'interno del codice / cartella Models, individuare il file SampleData.cs e rilasciarlo nella cartella Models nel progetto, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d3c75-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="d3c75-147">A questo punto è necessario aggiungere una riga di codice per indicare a Entity Framework su tale classe SampleData.</span><span class="sxs-lookup"><span data-stu-id="d3c75-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="d3c75-148">Fare doppio clic sul file Global. asax nella radice del progetto per aprirlo e aggiungere la riga seguente all'inizio dell'applicazione\_Start (metodo).</span><span class="sxs-lookup"><span data-stu-id="d3c75-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="d3c75-149">A questo punto, abbiamo completato le operazioni necessarie per configurare Entity Framework per il progetto.</span><span class="sxs-lookup"><span data-stu-id="d3c75-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="d3c75-150">Esecuzione di query sul database</span><span class="sxs-lookup"><span data-stu-id="d3c75-150">Querying the Database</span></span>

<span data-ttu-id="d3c75-151">A questo punto è possibile aggiornare il StoreController in modo che invece di usare "dati fittizio" chiama invece nel database per eseguire query di tutte le informazioni.</span><span class="sxs-lookup"><span data-stu-id="d3c75-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="d3c75-152">Si inizierà dichiarando un campo nel **StoreController** per conservare un'istanza della classe MusicStoreEntities, denominata storeDB:</span><span class="sxs-lookup"><span data-stu-id="d3c75-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="d3c75-153">Aggiornamento dell'indice Store per eseguire query sul database</span><span class="sxs-lookup"><span data-stu-id="d3c75-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="d3c75-154">La classe MusicStoreEntities è gestita da Entity Framework ed espone una proprietà di raccolta per ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="d3c75-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="d3c75-155">Aggiorniamo azione Index del nostro StoreController per recuperare tutti i generi dal database.</span><span class="sxs-lookup"><span data-stu-id="d3c75-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="d3c75-156">In precedenza abbiamo ottenuto questo come hardcoded i dati di stringa.</span><span class="sxs-lookup"><span data-stu-id="d3c75-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="d3c75-157">A questo punto è possibile invece usare semplicemente il contesto di Entity Framework Generes raccolta:</span><span class="sxs-lookup"><span data-stu-id="d3c75-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="d3c75-158">Nessuna modifica desidera essere eseguiti per il modello di vista poiché viene comunque restituito il StoreIndexViewModel stesso viene restituite prima - viene semplicemente restituito dei dati in tempo reale dal database ora.</span><span class="sxs-lookup"><span data-stu-id="d3c75-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="d3c75-159">Quando si esegue nuovamente il progetto e visitare l'URL "/ Store", si vedrà ora un elenco di tutti i generi dal database:</span><span class="sxs-lookup"><span data-stu-id="d3c75-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="d3c75-160">L'aggiornamento Store individuare e informazioni dettagliate per usare i dati in tempo reale</span><span class="sxs-lookup"><span data-stu-id="d3c75-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="d3c75-161">Con/Store/Sfoglia? genre =*[some-genre]* metodo di azione, viene eseguita la ricerca per genere in base al nome.</span><span class="sxs-lookup"><span data-stu-id="d3c75-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="d3c75-162">È previsto solo un risultato, poiché sono sempre non dovrebbero essere presenti due voci per lo stesso nome Genre e quindi è possibile usare il. Estensione Single () nelle query LINQ per eseguire query per l'oggetto Genre appropriato simile al seguente (non digitare questo ancora):</span><span class="sxs-lookup"><span data-stu-id="d3c75-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="d3c75-163">Il metodo Single accetta un'espressione Lambda come un parametro che specifica che si desidera un singolo oggetto Genre tale che il relativo nome corrisponde al valore che abbiamo definito.</span><span class="sxs-lookup"><span data-stu-id="d3c75-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="d3c75-164">Nel caso precedente, è corso il caricamento un singolo oggetto Genre con un valore di nome corrispondente Disco.</span><span class="sxs-lookup"><span data-stu-id="d3c75-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="d3c75-165">Articolo verrà fornita una funzionalità di Entity Framework che consente di indicare altre entità correlate che si desidera caricare anche quando l'oggetto Genre viene recuperato.</span><span class="sxs-lookup"><span data-stu-id="d3c75-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="d3c75-166">Questa funzionalità è denominata forma del risultato di Query e ci permette di ridurre il numero di volte in cui che è necessario accedere al database per recuperare tutte le informazioni che necessarie.</span><span class="sxs-lookup"><span data-stu-id="d3c75-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="d3c75-167">Si vuole recuperare preventivamente gli album per Genre vengono recuperati, in modo che la query da includere da Genres.Include("Albums") per indicare che si desidera anche album correlati verrà aggiornata.</span><span class="sxs-lookup"><span data-stu-id="d3c75-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="d3c75-168">Questo è più efficiente, poiché consente di recuperare i nostri Genre Album dati e nella richiesta singolo database.</span><span class="sxs-lookup"><span data-stu-id="d3c75-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="d3c75-169">Con le spiegazioni disponibili dall'area di lavoro, ecco come appare l'azione del controller Sfoglia aggiornata:</span><span class="sxs-lookup"><span data-stu-id="d3c75-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="d3c75-170">È ora possibile aggiornare la Store esplorazione della vista per visualizzare album che sono disponibili in ogni genere.</span><span class="sxs-lookup"><span data-stu-id="d3c75-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="d3c75-171">Aprire il modello di visualizzazione (trovato nel /Views/Store/Browse.cshtml) e aggiungere un elenco puntato di album come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d3c75-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="d3c75-172">Esecuzione dell'applicazione e passare a/Store/Sfoglia? genre = Mostra Jazz che i risultati sono ora rimosse dal database, visualizzazione di tutti gli album nel genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="d3c75-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="d3c75-173">Si renderà la stessa modifica al nostro /Store/dettagli / [id] URL e sostituire i dati fittizi con una query sul database che carica un Album il cui ID corrisponde al valore di parametro.</span><span class="sxs-lookup"><span data-stu-id="d3c75-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="d3c75-174">Esecuzione dell'applicazione e scegliendo /Store/Details/1 mostra che i risultati sono ora rimosse dal database.</span><span class="sxs-lookup"><span data-stu-id="d3c75-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="d3c75-175">Ora che la pagina dettagli Store è configurata per la visualizzazione di un album dall'ID di Album, aggiorniamo il **esplorare** Visualizza il collegamento alla visualizzazione dettagli.</span><span class="sxs-lookup"><span data-stu-id="d3c75-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="d3c75-176">Si userà HTML. ActionLink, esattamente come è stato fatto per creare un collegamento dall'indice Store per Store passare alla fine della sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="d3c75-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="d3c75-177">Di seguito è riportata l'origine completa per la visualizzazione di esplorazione.</span><span class="sxs-lookup"><span data-stu-id="d3c75-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="d3c75-178">A questo punto siamo in grado di passare dalla nostra pagina Store a una pagina Genre, che elenca gli album disponibili, e facendo clic su un album è possibile visualizzare i dettagli per tale album.</span><span class="sxs-lookup"><span data-stu-id="d3c75-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="d3c75-179">[Precedente](mvc-music-store-part-3.md)
> [Successivo](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="d3c75-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
