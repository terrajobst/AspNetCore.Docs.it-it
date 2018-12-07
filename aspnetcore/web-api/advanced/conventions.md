---
title: Usare le convenzioni dell'API Web
author: pranavkm
description: Informazioni sulle convenzioni dell'API Web in ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710075"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="33620-103">Usare le convenzioni dell'API Web</span><span class="sxs-lookup"><span data-stu-id="33620-103">Use web API conventions</span></span>

<span data-ttu-id="33620-104">ASP.NET Core 2.2 introduce una modalità per l'estrazione di [documentazione dell'API](xref:tutorials/web-api-help-pages-using-swagger) comune e l'applicazione della documentazione a più azioni o controller oppure a tutti i controller inclusi in un assembly.</span><span class="sxs-lookup"><span data-stu-id="33620-104">ASP.NET Core 2.2 introduces a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="33620-105">Le convenzioni dell'API Web sostituiscono l'aggiunta di [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) alle singole azioni.</span><span class="sxs-lookup"><span data-stu-id="33620-105">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span> <span data-ttu-id="33620-106">La soluzione consente di definire i tipi restituiti "tradizionali" più comuni e i codici di stato restituiti dall'azione, con una modalità per selezionare il metodo della convenzione che si applica a un'azione.</span><span class="sxs-lookup"><span data-stu-id="33620-106">It allows you to define the most common "conventional" return types and status codes that you return from your action with a way to select the convention method that applies to an action.</span></span>

<span data-ttu-id="33620-107">Per impostazione predefinita, ASP.NET Core MVC 2.2 include il set di convenzioni predefinite `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span><span class="sxs-lookup"><span data-stu-id="33620-107">By default, ASP.NET Core MVC 2.2 ships with a set of default conventions, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="33620-108">Le convenzioni sono basate sul controller di cui ASP.NET Core esegue lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="33620-108">The conventions are based on the controller that ASP.NET Core scaffolds.</span></span> <span data-ttu-id="33620-109">Se le azioni seguono il criterio prodotto dallo scaffolding, è possibile usare le convenzioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="33620-109">If your actions follow the pattern that scaffolding produces, you should be successful using the default conventions.</span></span>

<span data-ttu-id="33620-110">In fase di runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> riconosce le convenzioni.</span><span class="sxs-lookup"><span data-stu-id="33620-110">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="33620-111">`ApiExplorer` è l'astrazione MVC per comunicare con i generatori di documenti Open API.</span><span class="sxs-lookup"><span data-stu-id="33620-111">`ApiExplorer` is MVC's abstraction to communicate with Open API document generators.</span></span> <span data-ttu-id="33620-112">Gli attributi della convenzione applicata vengono associati a un'azione e sono inclusi nella documentazione Swagger dell'azione.</span><span class="sxs-lookup"><span data-stu-id="33620-112">Attributes from the applied convention get associated with an action and are included in the action's Swagger documentation.</span></span> <span data-ttu-id="33620-113">Anche gli analizzatori di API supportano le convenzioni.</span><span class="sxs-lookup"><span data-stu-id="33620-113">API analyzers also understand conventions.</span></span> <span data-ttu-id="33620-114">Se l'azione non è convenzionale (ad esempio, se restituisce un codice di stato non documentato dalla convenzione applicata), genera un avviso richiedendo di documentarla.</span><span class="sxs-lookup"><span data-stu-id="33620-114">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), it produces a warning, encouraging you to document it.</span></span>

<span data-ttu-id="33620-115">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="33620-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="33620-116">Applicare le convenzioni dell'API Web</span><span class="sxs-lookup"><span data-stu-id="33620-116">Apply web API conventions</span></span>

<span data-ttu-id="33620-117">Esistono tre modi per applicare una convenzione.</span><span class="sxs-lookup"><span data-stu-id="33620-117">There are three ways to apply a convention.</span></span> <span data-ttu-id="33620-118">Non sono supportate convenzioni composte.</span><span class="sxs-lookup"><span data-stu-id="33620-118">Conventions don't compose.</span></span> <span data-ttu-id="33620-119">Ogni azione può essere associata a una sola convenzione.</span><span class="sxs-lookup"><span data-stu-id="33620-119">Each action may be associated with exactly one convention.</span></span> <span data-ttu-id="33620-120">Le convenzioni più specifiche (illustrate di seguito) hanno la precedenza su quelle meno specifiche.</span><span class="sxs-lookup"><span data-stu-id="33620-120">More specific conventions (detailed below) take precedence over less specific ones.</span></span> <span data-ttu-id="33620-121">Quando si applicano a un'azione due o più convenzioni con la stessa priorità, la selezione è non deterministica.</span><span class="sxs-lookup"><span data-stu-id="33620-121">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="33620-122">Per applicare una convenzione a un'azione sono disponibili le opzioni seguenti, dalla più specifica alla meno specifica:</span><span class="sxs-lookup"><span data-stu-id="33620-122">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="33620-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; È valida per azioni singole, specifica il tipo di convenzione e il metodo della convenzione applicato.</span><span class="sxs-lookup"><span data-stu-id="33620-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span> <span data-ttu-id="33620-124">Nell'esempio seguente, il metodo della convenzione `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` viene applicato all'azione `Update`:</span><span class="sxs-lookup"><span data-stu-id="33620-124">In the following sample, the convention method `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. <span data-ttu-id="33620-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applicato a un controller &mdash; Applica il tipo di convenzione a tutte le azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="33620-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the convention type to all actions on the controller.</span></span> <span data-ttu-id="33620-126">I metodi convenzione sono provvisti di hint che determinano le azioni alle quali possono essere applicati (dettagli nel contesto della creazione di convenzioni).</span><span class="sxs-lookup"><span data-stu-id="33620-126">Convention methods are decorated with hints that determine which actions it would apply to (details as part of authoring conventions).</span></span> <span data-ttu-id="33620-127">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="33620-127">For example:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. <span data-ttu-id="33620-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applicato a un an assembly &mdash; Applica il tipo di convenzione a tutti i controller dell'assembly corrente.</span><span class="sxs-lookup"><span data-stu-id="33620-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="33620-129">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="33620-129">For example:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="33620-130">Creare convenzioni dell'API Web</span><span class="sxs-lookup"><span data-stu-id="33620-130">Create web API conventions</span></span>

<span data-ttu-id="33620-131">Una convenzione è un tipo statico con metodi.</span><span class="sxs-lookup"><span data-stu-id="33620-131">A convention is a static type with methods.</span></span> <span data-ttu-id="33620-132">Questi metodi vengono annotati con attributi `[ProducesResponseType]` o `[ProducesDefaultResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="33620-132">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span>

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

<span data-ttu-id="33620-133">Se si applica questa convenzione a un assembly, il metodo della convenzione viene applicato a qualsiasi azione con nome `Find` che ha esattamente un parametro con nome `id` e non ha altri attributi di metadati più specifici.</span><span class="sxs-lookup"><span data-stu-id="33620-133">Applying this convention to an assembly results in the convention method applying to any action with the name `Find` and having exactly one parameter named `id`, as long as they don't have other more specific metadata attributes.</span></span>

<span data-ttu-id="33620-134">Oltre a `[ProducesResponseType]` e `[ProducesDefaultResponseType]`, è possibile applicare `[ApiConventionNameMatch]` e `[ApiConventionTypeMatch]` al metodo convenzione che determina i metodi ai quali vengono applicati.</span><span class="sxs-lookup"><span data-stu-id="33620-134">In addition to `[ProducesResponseType]` and `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` can be applied to the convention method that determines the methods they apply to.</span></span> <span data-ttu-id="33620-135">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="33620-135">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* <span data-ttu-id="33620-136">L'opzione `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` applicata al metodo indica che la convenzione può corrispondere a qualsiasi azione con prefisso "Find".</span><span class="sxs-lookup"><span data-stu-id="33620-136">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method, indicates that the convention can match any action as long as it's prefixed with "Find".</span></span> <span data-ttu-id="33620-137">Sono inclusi metodi come `Find`, `FindPet` o `FindById`.</span><span class="sxs-lookup"><span data-stu-id="33620-137">This includes methods such as `Find`, `FindPet`, or `FindById`.</span></span>
* <span data-ttu-id="33620-138">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applicato al parametro indica che la convenzione può corrispondere a metodi che hanno esattamente un parametro che termina con l'identificatore del suffisso.</span><span class="sxs-lookup"><span data-stu-id="33620-138">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention can match methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="33620-139">Sono inclusi parametri come `id` o `petId`.</span><span class="sxs-lookup"><span data-stu-id="33620-139">This includes parameters such as `id` or `petId`.</span></span> <span data-ttu-id="33620-140">`ApiConventionTypeMatch` può essere applicato in modo analogo ai tipi, per vincolare il tipo del parametro.</span><span class="sxs-lookup"><span data-stu-id="33620-140">`ApiConventionTypeMatch` can be similarly applied to types to constrain the type of the parameter.</span></span> <span data-ttu-id="33620-141">È possibile usare un argomento `params[]` per indicare i parametri rimanenti, per i quali non è necessario che venga rilevata una corrispondenza esplicita.</span><span class="sxs-lookup"><span data-stu-id="33620-141">A `params[]` argument can be used to indicate remaining parameters that don't need to be explicitly matched.</span></span>
