---
title: Helper per tag predefiniti di ASP.NET Core
author: pkellner
description: Helper per tag predefiniti di ASP.NET Core
keywords: Helper per tag di ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: dd732822a715df19c0ee4b6accad3455ad6537da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="ae4a2-104">Helper per tag predefiniti di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae4a2-104">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="ae4a2-105">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="ae4a2-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="ae4a2-106">ASP.NET Core include molti helper di tag predefiniti per incrementare la produttività.</span><span class="sxs-lookup"><span data-stu-id="ae4a2-106">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="ae4a2-107">Questa sezione include una panoramica degli helper di tag predefiniti.</span><span class="sxs-lookup"><span data-stu-id="ae4a2-107">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="ae4a2-108">Esistono helper di tag predefiniti che non vengono trattati, in quanto vengono usati internamente dal motore di visualizzazione di [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="ae4a2-108">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="ae4a2-109">Ciò include un helper di tag per il carattere ~ che si espande nel percorso radice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="ae4a2-109">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="ae4a2-110">Helper per tag incorporati di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae4a2-110">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="ae4a2-111">**[Helper per tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-111">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="ae4a2-112">**[Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-112">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="ae4a2-113">**[Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-113">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="ae4a2-114">**[Helper per tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-114">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="ae4a2-115">**[Helper per tag di modulo](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-115">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="ae4a2-116">**[Helper per tag di immagine](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-116">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="ae4a2-117">**[Helper per tag di input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-117">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="ae4a2-118">**[Helper per tag di etichetta](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-118">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="ae4a2-119">**[Helper per tag di selezione](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="ae4a2-120">**[Helper per tag area di testo](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="ae4a2-121">**[Helper per tag messaggio di convalida](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="ae4a2-122">**[Helper per tag riepilogo di convalida](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="ae4a2-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ae4a2-123">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ae4a2-123">Additional resources</span></span>

* [<span data-ttu-id="ae4a2-124">Sviluppo lato client</span><span class="sxs-lookup"><span data-stu-id="ae4a2-124">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="ae4a2-125">Helper tag</span><span class="sxs-lookup"><span data-stu-id="ae4a2-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
