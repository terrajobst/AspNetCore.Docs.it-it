---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: "Gestione delle relazioni di entità | Documenti Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 9294da7cd5b7a362d4ade9d1bf7e7747e20ee1a8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="handling-entity-relations"></a><span data-ttu-id="ffbde-102">Gestione delle relazioni di entità</span><span class="sxs-lookup"><span data-stu-id="ffbde-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="ffbde-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ffbde-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ffbde-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="ffbde-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ffbde-105">Questa sezione descrive alcuni dettagli del modo in cui EF carica entità correlate e come gestire le proprietà di navigazione circolare nelle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="ffbde-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="ffbde-106">(Questa sezione fornisce informazioni di background e non è necessario per completare l'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ffbde-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="ffbde-107">Se si preferisce, andare al [parte 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="ffbde-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="ffbde-108">Caricamento e il caricamento Lazy eager</span><span class="sxs-lookup"><span data-stu-id="ffbde-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="ffbde-109">Quando si utilizza Entity Framework con un database relazionale, è importante comprendere la modalità di caricamento dati correlati in Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ffbde-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="ffbde-110">È inoltre utile visualizzare le query SQL che genera l'errore Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ffbde-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="ffbde-111">Per tracciare SQL, aggiungere la seguente riga di codice per il `BookServiceContext` costruttore:</span><span class="sxs-lookup"><span data-stu-id="ffbde-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="ffbde-112">Se si invia una richiesta GET a /api/books, restituisce JSON simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ffbde-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="ffbde-113">Si noterà che nella proprietà Author è null, anche se il runbook contiene un AuthorId valido.</span><span class="sxs-lookup"><span data-stu-id="ffbde-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="ffbde-114">Ciò avviene perché EF non è possibile caricare le entità correlate di autore.</span><span class="sxs-lookup"><span data-stu-id="ffbde-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="ffbde-115">Questo conferma che il log di traccia della query SQL:</span><span class="sxs-lookup"><span data-stu-id="ffbde-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="ffbde-116">L'istruzione SELECT accetta dalla tabella di documentazione e non fanno riferimento alla tabella di autore.</span><span class="sxs-lookup"><span data-stu-id="ffbde-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="ffbde-117">Per riferimento, di seguito è il metodo di `BooksController` classe che restituisce l'elenco di libri.</span><span class="sxs-lookup"><span data-stu-id="ffbde-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="ffbde-118">Di seguito viene illustrato come è possibile tornare all'autore come parte dei dati JSON.</span><span class="sxs-lookup"><span data-stu-id="ffbde-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="ffbde-119">Esistono tre modi per caricare i dati correlati in Entity Framework: il caricamento non differito, il caricamento lazy e caricamento esplicito.</span><span class="sxs-lookup"><span data-stu-id="ffbde-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="ffbde-120">Esistono vantaggi e svantaggi con ogni tecnica, è importante comprenderne il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="ffbde-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="ffbde-121">Caricamento eager</span><span class="sxs-lookup"><span data-stu-id="ffbde-121">Eager Loading</span></span>

<span data-ttu-id="ffbde-122">Con *caricamento eager*, EF carica entità correlate come parte della query del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="ffbde-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="ffbde-123">Per eseguire il caricamento non differito, utilizzare il **System.Data.Entity.Include** metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="ffbde-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="ffbde-124">In questo modo EF per includere i dati di autore della query.</span><span class="sxs-lookup"><span data-stu-id="ffbde-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="ffbde-125">Se si apporta questa modifica e di eseguire l'app, ora i dati JSON sono simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ffbde-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="ffbde-126">Il log di traccia Mostra di EF eseguito un join sulle tabelle di libro e autore.</span><span class="sxs-lookup"><span data-stu-id="ffbde-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="ffbde-127">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="ffbde-127">Lazy Loading</span></span>

<span data-ttu-id="ffbde-128">Con il caricamento differito, EF carica automaticamente un'entità correlata quando la proprietà di navigazione per l'entità viene dereferenziata.</span><span class="sxs-lookup"><span data-stu-id="ffbde-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="ffbde-129">Per abilitare il caricamento differito, impostare la proprietà di navigazione virtuale.</span><span class="sxs-lookup"><span data-stu-id="ffbde-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="ffbde-130">Ad esempio, nella classe Book:</span><span class="sxs-lookup"><span data-stu-id="ffbde-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="ffbde-131">Si consideri ora il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ffbde-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="ffbde-132">Quando il caricamento lazy è abilitato, l'accesso di `Author` proprietà `books[0]` fa sì che Entity Framework eseguire query sul database per l'autore.</span><span class="sxs-lookup"><span data-stu-id="ffbde-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="ffbde-133">Caricamento lazy richiede più trip di database, in quanto EF invia una query ogni volta che viene recuperata un'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="ffbde-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="ffbde-134">In genere, si desidera il caricamento lazy disabilitato per gli oggetti da serializzare.</span><span class="sxs-lookup"><span data-stu-id="ffbde-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="ffbde-135">Il serializzatore deve leggere tutte le proprietà sul modello, che attiva il caricamento delle entità correlate.</span><span class="sxs-lookup"><span data-stu-id="ffbde-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="ffbde-136">Ad esempio, ecco le query SQL EF serializza l'elenco di libri con abilitato del caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="ffbde-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="ffbde-137">È possibile vedere che EF rende tre query separate per tre autori.</span><span class="sxs-lookup"><span data-stu-id="ffbde-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="ffbde-138">Sono ancora presenti volte quando si potrebbe desiderare di utilizzare il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="ffbde-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="ffbde-139">Caricamento eager può causare EF generare un join molto complesso.</span><span class="sxs-lookup"><span data-stu-id="ffbde-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="ffbde-140">O potrebbe essere necessario entità correlate per un piccolo subset di dati e il caricamento lazy sarebbe più efficiente.</span><span class="sxs-lookup"><span data-stu-id="ffbde-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="ffbde-141">È di un modo per evitare problemi di serializzazione per serializzare oggetti di trasferimento di dati DTO anziché oggetti entità.</span><span class="sxs-lookup"><span data-stu-id="ffbde-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="ffbde-142">Questo approccio vi mostrerò avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="ffbde-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="ffbde-143">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="ffbde-143">Explicit Loading</span></span>

<span data-ttu-id="ffbde-144">Caricamento esplicito è simile al caricamento lazy, ad eccezione del fatto che sia possibile ottenere in modo esplicito i dati correlati nel codice. non avviene automaticamente quando si accede a una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="ffbde-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="ffbde-145">Caricamento esplicito offre maggiore controllo sul momento in cui caricare i dati correlati, ma richiede codice aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="ffbde-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="ffbde-146">Per ulteriori informazioni sul caricamento esplicito, vedere [durante il caricamento delle entità correlate](https://msdn.microsoft.com/en-us/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="ffbde-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/en-us/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="ffbde-147">Le proprietà di navigazione e i riferimenti circolari</span><span class="sxs-lookup"><span data-stu-id="ffbde-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="ffbde-148">Definendo i modelli di libro e l'autore, sono state definite proprietà di navigazione nel `Book` classe per la relazione libro autore, ma non definita una proprietà di navigazione in altra direzione.</span><span class="sxs-lookup"><span data-stu-id="ffbde-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="ffbde-149">Cosa accade se si aggiunge la proprietà di navigazione corrispondente per la `Author` classe?</span><span class="sxs-lookup"><span data-stu-id="ffbde-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="ffbde-150">Sfortunatamente, questo costituisce un problema durante la serializzazione dei modelli.</span><span class="sxs-lookup"><span data-stu-id="ffbde-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="ffbde-151">Se si caricano i dati correlati, viene creato un grafico circolare dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="ffbde-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="ffbde-152">Quando il formattatore JSON o XML tenta di serializzare il grafico, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="ffbde-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="ffbde-153">I due formattatori generano messaggi di eccezione diverso.</span><span class="sxs-lookup"><span data-stu-id="ffbde-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="ffbde-154">Di seguito è riportato un esempio per il formattatore JSON:</span><span class="sxs-lookup"><span data-stu-id="ffbde-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="ffbde-155">Di seguito è riportato il formattatore XML:</span><span class="sxs-lookup"><span data-stu-id="ffbde-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="ffbde-156">Una soluzione consiste nell'utilizzare DTO, cui è descritta nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="ffbde-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="ffbde-157">In alternativa, è possibile configurare i formattatori JSON e XML per la gestione di cicli di grafico.</span><span class="sxs-lookup"><span data-stu-id="ffbde-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="ffbde-158">Per ulteriori informazioni, vedere [la gestione di riferimenti circolari oggetto](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="ffbde-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="ffbde-159">Per questa esercitazione, non è necessario il `Author.Book` proprietà di navigazione, pertanto è possibile tralasciare.</span><span class="sxs-lookup"><span data-stu-id="ffbde-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ffbde-160">[Precedente](part-3.md)
[Successivo](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="ffbde-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
