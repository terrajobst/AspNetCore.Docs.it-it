---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Gestione delle relazioni tra entità | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: fcfddb3d56d0be641d2df9d92c334776975621be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826157"
---
<a name="handling-entity-relations"></a><span data-ttu-id="bf027-102">Relazioni tra entità di gestione</span><span class="sxs-lookup"><span data-stu-id="bf027-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="bf027-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bf027-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="bf027-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="bf027-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="bf027-105">Questa sezione descrive alcuni dettagli di come Entity Framework carica entità correlate e come gestire le proprietà di navigazione circolare nelle classi di modelli.</span><span class="sxs-lookup"><span data-stu-id="bf027-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="bf027-106">(Questa sezione fornisce informazioni di background e non è necessario per completare l'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="bf027-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="bf027-107">Se si preferisce, andare al [parte 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="bf027-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="bf027-108">Caricamento e il caricamento Lazy eager</span><span class="sxs-lookup"><span data-stu-id="bf027-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="bf027-109">Quando si usa Entity Framework con un database relazionale, è importante comprendere la modalità di caricamento dei dati correlati in Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bf027-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="bf027-110">È anche utile visualizzare le query SQL generato da EF.</span><span class="sxs-lookup"><span data-stu-id="bf027-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="bf027-111">Per tracciare il codice SQL, aggiungere la seguente riga di codice per il `BookServiceContext` costruttore:</span><span class="sxs-lookup"><span data-stu-id="bf027-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="bf027-112">Se si invia una richiesta GET a /api/books, restituisce JSON simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="bf027-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="bf027-113">Si noterà che la proprietà dell'autore è null, anche se il libro contiene un AuthorId valido.</span><span class="sxs-lookup"><span data-stu-id="bf027-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="bf027-114">Ciò avviene perché EF non stia caricando le entità autore correlate.</span><span class="sxs-lookup"><span data-stu-id="bf027-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="bf027-115">Il log di traccia della query SQL conferma questo:</span><span class="sxs-lookup"><span data-stu-id="bf027-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="bf027-116">L'istruzione SELECT accetta dalla tabella dei libri e non fa riferimento la tabella di autore.</span><span class="sxs-lookup"><span data-stu-id="bf027-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="bf027-117">Per riferimento, ecco il metodo nel `BooksController` classe che restituisce l'elenco di libri.</span><span class="sxs-lookup"><span data-stu-id="bf027-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="bf027-118">Vediamo come possiamo restituire l'autore come parte dei dati JSON.</span><span class="sxs-lookup"><span data-stu-id="bf027-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="bf027-119">Esistono tre modi per caricare i dati correlati in Entity Framework: il caricamento eager, il caricamento lazy e il caricamento esplicito.</span><span class="sxs-lookup"><span data-stu-id="bf027-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="bf027-120">Esistono vantaggi e svantaggi con ogni tecnica, pertanto è importante comprenderne il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="bf027-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="bf027-121">Caricamento eager</span><span class="sxs-lookup"><span data-stu-id="bf027-121">Eager Loading</span></span>

<span data-ttu-id="bf027-122">Con *caricamento eager*, Entity Framework carica entità correlate come parte della query del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="bf027-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="bf027-123">Per eseguire il caricamento eager, usare il **System.Data.Entity.Include** metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="bf027-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="bf027-124">Indica a Entity Framework per includere i dati dell'autore della query.</span><span class="sxs-lookup"><span data-stu-id="bf027-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="bf027-125">Se si apporta questa modifica e si esegue l'app, i dati JSON sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="bf027-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="bf027-126">Il log di traccia indica che EF eseguita un join sulle tabelle di libro e autore.</span><span class="sxs-lookup"><span data-stu-id="bf027-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="bf027-127">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="bf027-127">Lazy Loading</span></span>

<span data-ttu-id="bf027-128">Con il caricamento lazy, Entity Framework carica automaticamente un'entità correlata quando la proprietà di navigazione per l'entità è dereferenziata.</span><span class="sxs-lookup"><span data-stu-id="bf027-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="bf027-129">Per abilitare il caricamento lazy, verificare la proprietà di navigazione virtuale.</span><span class="sxs-lookup"><span data-stu-id="bf027-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="bf027-130">Ad esempio, nella classe Book:</span><span class="sxs-lookup"><span data-stu-id="bf027-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="bf027-131">Si consideri ora il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="bf027-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="bf027-132">Quando il caricamento lazy è abilitato, l'accesso alla `Author` proprietà `books[0]` fa in modo che Entity Framework eseguire query sul database per l'autore.</span><span class="sxs-lookup"><span data-stu-id="bf027-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="bf027-133">Il caricamento lazy richiede più trip di database, perché Entity Framework invia una query ogni volta che recupera un'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="bf027-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="bf027-134">In generale, è consigliabile il caricamento lazy disabilitato per gli oggetti da serializzare.</span><span class="sxs-lookup"><span data-stu-id="bf027-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="bf027-135">Il serializzatore deve leggere tutte le proprietà sul modello, che attiva il caricamento di entità correlate.</span><span class="sxs-lookup"><span data-stu-id="bf027-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="bf027-136">Ad esempio, ecco le query SQL quando Entity Framework serializza l'elenco di libri con caricamento lazy abilitata.</span><span class="sxs-lookup"><span data-stu-id="bf027-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="bf027-137">È possibile vedere che Entity Framework esegue tre query separate per gli autori di tre.</span><span class="sxs-lookup"><span data-stu-id="bf027-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="bf027-138">Esistono ancora volte quando si potrebbe voler usare il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="bf027-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="bf027-139">Il caricamento eager può causare Entity Framework generare un join molto complesso.</span><span class="sxs-lookup"><span data-stu-id="bf027-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="bf027-140">O potrebbe essere necessario entità correlate per un piccolo subset di dati e il caricamento lazy risulterebbe sicuramente più efficace.</span><span class="sxs-lookup"><span data-stu-id="bf027-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="bf027-141">Un modo per evitare problemi di serializzazione deve serializzare gli oggetti di trasferimento di dati (DTO) invece di oggetti entità.</span><span class="sxs-lookup"><span data-stu-id="bf027-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="bf027-142">Questo approccio verrà illustrato più avanti nell'articolo.</span><span class="sxs-lookup"><span data-stu-id="bf027-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="bf027-143">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="bf027-143">Explicit Loading</span></span>

<span data-ttu-id="bf027-144">Il caricamento esplicito è simile a il caricamento lazy, ad eccezione del fatto che i dati correlati di ottenere in modo esplicito nel codice. non avviene automaticamente quando si accede a una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="bf027-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="bf027-145">Il caricamento esplicito offre un maggiore controllo sul momento in cui caricare i dati correlati, ma richiede codice aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="bf027-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="bf027-146">Per ulteriori informazioni sul caricamento esplicito, vedere [caricamento di entità correlate](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="bf027-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="bf027-147">Le proprietà di navigazione e i riferimenti circolari</span><span class="sxs-lookup"><span data-stu-id="bf027-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="bf027-148">Quando definito i modelli di libro e autore, ho definito una proprietà di navigazione nel `Book` classe per la relazione autore del libro, ma non definiscono una proprietà di navigazione in altra direzione.</span><span class="sxs-lookup"><span data-stu-id="bf027-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="bf027-149">Cosa accade se si aggiunge la proprietà di navigazione corrispondente per il `Author` classe?</span><span class="sxs-lookup"><span data-stu-id="bf027-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="bf027-150">Sfortunatamente, questo costituisce un problema durante la serializzazione dei modelli.</span><span class="sxs-lookup"><span data-stu-id="bf027-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="bf027-151">Se si caricano i dati correlati, viene creato un grafico circolare dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="bf027-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="bf027-152">Quando il formattatore JSON o XML tenta di serializzare il grafico, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="bf027-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="bf027-153">I due formattatori generano messaggi di eccezione diverso.</span><span class="sxs-lookup"><span data-stu-id="bf027-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="bf027-154">Di seguito è riportato un esempio per il formattatore JSON:</span><span class="sxs-lookup"><span data-stu-id="bf027-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="bf027-155">Ecco il formattatore XML:</span><span class="sxs-lookup"><span data-stu-id="bf027-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="bf027-156">Un'unica soluzione consiste nell'usare gli oggetti DTO, che ho descritto nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="bf027-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="bf027-157">In alternativa, è possibile configurare i formattatori XML e JSON per gestire i cicli di grafico.</span><span class="sxs-lookup"><span data-stu-id="bf027-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="bf027-158">Per altre informazioni, vedere [gestisce i riferimenti all'oggetto circolare](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="bf027-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="bf027-159">Per questa esercitazione, non è necessario il `Author.Book` proprietà di navigazione, pertanto è possibile tralasciare.</span><span class="sxs-lookup"><span data-stu-id="bf027-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bf027-160">[Precedente](part-3.md)
> [Successivo](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="bf027-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
