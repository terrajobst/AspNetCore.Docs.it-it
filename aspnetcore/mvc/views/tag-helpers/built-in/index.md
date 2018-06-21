---
title: Helper per tag predefiniti di ASP.NET Core
author: pkellner
description: Informazioni su come gli helper per i tag predefiniti di ASP.NET Core sono utili per incrementare la produttività.
ms.author: riande
ms.date: 09/13/2017
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 4f2ebf1600f42847db1c1f9517787b020d2e86c9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279168"
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="cc854-103">Helper per tag predefiniti di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc854-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="cc854-104">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="cc854-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="cc854-105">ASP.NET Core include molti helper di tag predefiniti per incrementare la produttività.</span><span class="sxs-lookup"><span data-stu-id="cc854-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="cc854-106">Questa sezione include una panoramica degli helper di tag predefiniti.</span><span class="sxs-lookup"><span data-stu-id="cc854-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="cc854-107">Esistono helper di tag predefiniti che non vengono trattati, in quanto vengono usati internamente dal motore di visualizzazione di [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="cc854-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="cc854-108">Ciò include un helper di tag per il carattere ~ che si espande nel percorso radice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="cc854-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="cc854-109">Helper per tag incorporati di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc854-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="cc854-110">**[Helper per tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="cc854-111">**[Helper per tag di cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="cc854-112">**[Helper per tag di cache distribuita](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="cc854-113">**[Helper per tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="cc854-114">**[Helper per tag di modulo](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="cc854-115">**[Helper per tag di immagine](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="cc854-116">**[Helper per tag di input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="cc854-117">**[Helper per tag di etichetta](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="cc854-118">**[Helper per tag parziale](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="cc854-119">**[Helper per tag di selezione](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="cc854-120">**[Helper per tag area di testo](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="cc854-121">**[Helper per tag messaggio di convalida](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="cc854-122">**[Helper per tag riepilogo di convalida](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="cc854-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc854-123">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="cc854-123">Additional resources</span></span>

* [<span data-ttu-id="cc854-124">Sviluppo sul lato client</span><span class="sxs-lookup"><span data-stu-id="cc854-124">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="cc854-125">Helper tag</span><span class="sxs-lookup"><span data-stu-id="cc854-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
