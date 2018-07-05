---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Aggiungere un nuovo elemento al Database | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 36251ba907a6f580b63f0fded0591c26b6ff879e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818508"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="0de6f-102">Aggiungere un nuovo elemento al Database</span><span class="sxs-lookup"><span data-stu-id="0de6f-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="0de6f-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0de6f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0de6f-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="0de6f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="0de6f-105">In questa sezione si aggiungerà la possibilità per gli utenti creare un nuovo libro.</span><span class="sxs-lookup"><span data-stu-id="0de6f-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="0de6f-106">Nell'app. js, aggiungere il codice seguente al modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="0de6f-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="0de6f-107">In Index. cshtml, sostituire il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="0de6f-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="0de6f-108">con:</span><span class="sxs-lookup"><span data-stu-id="0de6f-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="0de6f-109">Questo codice crea un form per l'invio di un autore di nuovo.</span><span class="sxs-lookup"><span data-stu-id="0de6f-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="0de6f-110">I valori per l'elenco di riepilogo a discesa di autore sono associati a dati per il `authors` osservabile in modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="0de6f-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="0de6f-111">Per gli altri input di modulo, i valori vengono associati per il `newBook` proprietà del modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="0de6f-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="0de6f-112">Il gestore di invio del form è associato ai `addBook` funzione:</span><span class="sxs-lookup"><span data-stu-id="0de6f-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="0de6f-113">Il `addBook` funzione legge i valori correnti degli input form associato a dati per creare un oggetto JSON.</span><span class="sxs-lookup"><span data-stu-id="0de6f-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="0de6f-114">Quindi esegue il postback l'oggetto JSON da `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="0de6f-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0de6f-115">[Precedente](part-8.md)
> [Successivo](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="0de6f-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
