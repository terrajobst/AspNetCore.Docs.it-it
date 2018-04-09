---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Creare oggetti di trasferimento dati DTO | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="bce3a-102">Creare oggetti di trasferimento dati (DTO)</span><span class="sxs-lookup"><span data-stu-id="bce3a-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="bce3a-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bce3a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="bce3a-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="bce3a-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="bce3a-105">Diritto a questo punto, l'API web espone le entità di database per il client.</span><span class="sxs-lookup"><span data-stu-id="bce3a-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="bce3a-106">Il client riceve i dati che è mappata direttamente a tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="bce3a-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="bce3a-107">Tuttavia, che non è sempre una buona idea.</span><span class="sxs-lookup"><span data-stu-id="bce3a-107">However, that's not always a good idea.</span></span> <span data-ttu-id="bce3a-108">Talvolta si desidera modificare la forma dei dati inviati al client.</span><span class="sxs-lookup"><span data-stu-id="bce3a-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="bce3a-109">Può, ad esempio, essere necessario:</span><span class="sxs-lookup"><span data-stu-id="bce3a-109">For example, you might want to:</span></span>

- <span data-ttu-id="bce3a-110">Rimuovere i riferimenti circolari (vedere la sezione precedente).</span><span class="sxs-lookup"><span data-stu-id="bce3a-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="bce3a-111">Nascondere determinate proprietà che non dovrebbero per visualizzare i client.</span><span class="sxs-lookup"><span data-stu-id="bce3a-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="bce3a-112">Omettere alcune proprietà per ridurre le dimensioni di payload.</span><span class="sxs-lookup"><span data-stu-id="bce3a-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="bce3a-113">Convertire oggetti grafici che contengono gli oggetti annidati, in modo da renderli più utile per i client.</span><span class="sxs-lookup"><span data-stu-id="bce3a-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="bce3a-114">Evitare di "eccesso di registrazione" vulnerabilità.</span><span class="sxs-lookup"><span data-stu-id="bce3a-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="bce3a-115">(Vedere [la convalida del modello](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) per una descrizione della registrazione in eccesso.)</span><span class="sxs-lookup"><span data-stu-id="bce3a-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="bce3a-116">Separare il livello di servizio dal livello del database.</span><span class="sxs-lookup"><span data-stu-id="bce3a-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="bce3a-117">A tale scopo, è possibile definire un *oggetto di trasferimento dati* (DTO).</span><span class="sxs-lookup"><span data-stu-id="bce3a-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="bce3a-118">Un DTO è un oggetto che definisce come i dati verranno inviati in rete.</span><span class="sxs-lookup"><span data-stu-id="bce3a-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="bce3a-119">Di seguito viene illustrato il funzionamento con l'entità Book.</span><span class="sxs-lookup"><span data-stu-id="bce3a-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="bce3a-120">Nella cartella Models, aggiungere due classi DTO:</span><span class="sxs-lookup"><span data-stu-id="bce3a-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="bce3a-121">Il `BookDetailDTO` classe sono incluse tutte le proprietà del modello di libro, con la differenza che `AuthorName` è una stringa che conterrà il nome dell'autore.</span><span class="sxs-lookup"><span data-stu-id="bce3a-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="bce3a-122">Il `BookDTO` classe contiene un subset di proprietà da `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="bce3a-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="bce3a-123">Successivamente, sostituire i due metodi GET di `BooksController` (classe), con le versioni che restituiscono DTO.</span><span class="sxs-lookup"><span data-stu-id="bce3a-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="bce3a-124">Si userà il LINQ **selezionare** istruzione da convertire in DTO provenienti da entità Book.</span><span class="sxs-lookup"><span data-stu-id="bce3a-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="bce3a-125">Ecco il codice SQL generato dalla nuova `GetBooks` metodo.</span><span class="sxs-lookup"><span data-stu-id="bce3a-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="bce3a-126">È possibile vedere che Entity Framework traduce LINQ **selezionare** in un'istruzione SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="bce3a-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="bce3a-127">Infine, modificare il `PostBook` per restituire un DTO.</span><span class="sxs-lookup"><span data-stu-id="bce3a-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="bce3a-128">In questa esercitazione viene convertito in DTO manualmente nel codice.</span><span class="sxs-lookup"><span data-stu-id="bce3a-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="bce3a-129">Un'altra opzione consiste nell'utilizzare una libreria come [AutoMapper](http://automapper.org/) che gestisce automaticamente la conversione.</span><span class="sxs-lookup"><span data-stu-id="bce3a-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="bce3a-130">[Precedente](part-4.md)
> [Successivo](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="bce3a-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
