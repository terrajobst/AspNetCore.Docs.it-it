---
title: Helper per tag predefiniti di ASP.NET Core
author: pkellner
description: Helper per tag predefiniti di ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 8ffc748ec3d4eed35871543f5ceccc86aadee661
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="d47a6-103">Helper per tag predefiniti di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d47a6-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="d47a6-104">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="d47a6-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="d47a6-105">ASP.NET Core include molti helper di tag predefiniti per incrementare la produttività.</span><span class="sxs-lookup"><span data-stu-id="d47a6-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="d47a6-106">Questa sezione include una panoramica degli helper di tag predefiniti.</span><span class="sxs-lookup"><span data-stu-id="d47a6-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="d47a6-107">Esistono helper di tag predefiniti che non vengono trattati, in quanto vengono usati internamente dal motore di visualizzazione di [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="d47a6-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="d47a6-108">Ciò include un helper di tag per il carattere ~ che si espande nel percorso radice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="d47a6-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="d47a6-109">Helper per tag incorporati di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d47a6-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="d47a6-110">**[Helper per tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="d47a6-111">**[Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="d47a6-112">**[Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="d47a6-113">**[Helper per tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="d47a6-114">**[Helper per tag di modulo](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="d47a6-115">**[Helper per tag di immagine](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="d47a6-116">**[Helper per tag di input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="d47a6-117">**[Helper per tag di etichetta](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="d47a6-118">**[Helper per tag di selezione](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-118">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="d47a6-119">**[Helper per tag area di testo](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-119">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="d47a6-120">**[Helper per tag messaggio di convalida](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-120">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="d47a6-121">**[Helper per tag riepilogo di convalida](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="d47a6-121">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d47a6-122">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d47a6-122">Additional resources</span></span>

* [<span data-ttu-id="d47a6-123">Sviluppo lato client</span><span class="sxs-lookup"><span data-stu-id="d47a6-123">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="d47a6-124">Helper tag</span><span class="sxs-lookup"><span data-stu-id="d47a6-124">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
