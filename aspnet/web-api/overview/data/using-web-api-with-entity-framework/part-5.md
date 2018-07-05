---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Creare oggetti di trasferimento dati (DTO) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: a1be1b72559aaffdf530402313f58d6c2e25b189
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828163"
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="46b38-102">Creare oggetti di trasferimento dati (DTO)</span><span class="sxs-lookup"><span data-stu-id="46b38-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="46b38-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="46b38-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="46b38-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="46b38-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="46b38-105">Diritto a questo punto, l'API web espone le entità del database al client.</span><span class="sxs-lookup"><span data-stu-id="46b38-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="46b38-106">Il client riceve i dati che esegue il mapping direttamente alle tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="46b38-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="46b38-107">Tuttavia, che non è sempre una buona idea.</span><span class="sxs-lookup"><span data-stu-id="46b38-107">However, that's not always a good idea.</span></span> <span data-ttu-id="46b38-108">Talvolta si desidera modificare la forma dei dati inviati al client.</span><span class="sxs-lookup"><span data-stu-id="46b38-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="46b38-109">Può, ad esempio, essere necessario:</span><span class="sxs-lookup"><span data-stu-id="46b38-109">For example, you might want to:</span></span>

- <span data-ttu-id="46b38-110">Rimuovere i riferimenti circolari (vedere la sezione precedente).</span><span class="sxs-lookup"><span data-stu-id="46b38-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="46b38-111">Nascondi proprietà specifiche che non dovrebbero per visualizzare i client.</span><span class="sxs-lookup"><span data-stu-id="46b38-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="46b38-112">Omettere alcune proprietà per ridurre le dimensioni del payload.</span><span class="sxs-lookup"><span data-stu-id="46b38-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="46b38-113">Convertire oggetti grafici contenenti oggetti annidati, in modo da renderli più pratico per i client.</span><span class="sxs-lookup"><span data-stu-id="46b38-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="46b38-114">Evitare di "" eventuali vulnerabilità all'overposting.</span><span class="sxs-lookup"><span data-stu-id="46b38-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="46b38-115">(Vedere [convalida dei modelli](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) per una discussione di evitare l'overposting.)</span><span class="sxs-lookup"><span data-stu-id="46b38-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="46b38-116">Disaccoppiare il livello di servizio dal livello di database.</span><span class="sxs-lookup"><span data-stu-id="46b38-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="46b38-117">A tale scopo, è possibile definire un *oggetto di trasferimento dati* (DTO).</span><span class="sxs-lookup"><span data-stu-id="46b38-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="46b38-118">Un oggetto DTO è un oggetto che definisce come i dati verranno inviati in rete.</span><span class="sxs-lookup"><span data-stu-id="46b38-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="46b38-119">Vediamo come funziona con l'entità Book.</span><span class="sxs-lookup"><span data-stu-id="46b38-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="46b38-120">Nella cartella Models, aggiungere due classi DTO:</span><span class="sxs-lookup"><span data-stu-id="46b38-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="46b38-121">Il `BookDetailDTO` classe include tutte le proprietà del modello di libro, con la differenza che `AuthorName` è una stringa che conterrà il nome dell'autore.</span><span class="sxs-lookup"><span data-stu-id="46b38-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="46b38-122">Il `BookDTO` classe contiene un subset delle proprietà da `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="46b38-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="46b38-123">Successivamente, sostituire i due metodi GET nel `BooksController` (classe), con le versioni che restituiscono oggetti DTO.</span><span class="sxs-lookup"><span data-stu-id="46b38-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="46b38-124">Si userà il LINQ **seleziona** istruzione da convertire in oggetti DTO provenienti da entità Book.</span><span class="sxs-lookup"><span data-stu-id="46b38-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="46b38-125">Ecco il codice SQL generato dal nuovo `GetBooks` (metodo).</span><span class="sxs-lookup"><span data-stu-id="46b38-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="46b38-126">È possibile vedere che Entity Framework traduce LINQ **seleziona** in un'istruzione SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="46b38-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="46b38-127">Modificare infine la `PostBook` per restituire un oggetto DTO.</span><span class="sxs-lookup"><span data-stu-id="46b38-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="46b38-128">In questa esercitazione, abbiamo deciso di convertire agli oggetti DTO manualmente nel codice.</span><span class="sxs-lookup"><span data-stu-id="46b38-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="46b38-129">Un'altra opzione consiste nell'usare una libreria, ad esempio [AutoMapper](http://automapper.org/) che gestisce automaticamente la conversione.</span><span class="sxs-lookup"><span data-stu-id="46b38-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="46b38-130">[Precedente](part-4.md)
> [Successivo](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="46b38-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
