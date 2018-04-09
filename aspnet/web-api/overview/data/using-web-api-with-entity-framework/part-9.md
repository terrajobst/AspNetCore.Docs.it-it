---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Aggiungere un nuovo elemento al Database | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 5845c092c4d7aee12b33b3f0a49c0e944c0fb9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="cef48-102">Aggiungere un nuovo elemento al Database</span><span class="sxs-lookup"><span data-stu-id="cef48-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="cef48-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cef48-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="cef48-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="cef48-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="cef48-105">In questa sezione si aggiungerà la possibilità agli utenti di creare una nuova rubrica.</span><span class="sxs-lookup"><span data-stu-id="cef48-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="cef48-106">In app.js, aggiungere il codice seguente per il modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="cef48-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="cef48-107">In cshtml, sostituire il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="cef48-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="cef48-108">con:</span><span class="sxs-lookup"><span data-stu-id="cef48-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="cef48-109">Questo codice crea un modulo per l'invio di un nuovo autore.</span><span class="sxs-lookup"><span data-stu-id="cef48-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="cef48-110">I valori per l'elenco di riepilogo a discesa di autore vengono associati a dati per il `authors` observable nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="cef48-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="cef48-111">Per gli altri form, i valori sono associati a dati per il `newBook` proprietà del modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="cef48-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="cef48-112">Il gestore di invio del form è associato il `addBook` funzione:</span><span class="sxs-lookup"><span data-stu-id="cef48-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="cef48-113">Il `addBook` funzione legge i valori correnti degli input form associato a dati per creare un oggetto JSON.</span><span class="sxs-lookup"><span data-stu-id="cef48-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="cef48-114">Quindi invia l'oggetto JSON per `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="cef48-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cef48-115">[Precedente](part-8.md)
> [Successivo](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="cef48-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
