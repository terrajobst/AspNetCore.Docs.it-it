---
title: Inserimento di dipendenze nelle viste
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: cade61b1ebdb2b845b07117384475638c0227f7f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="7cb44-102">Inserimento di dipendenze nelle viste</span><span class="sxs-lookup"><span data-stu-id="7cb44-102">Dependency injection into views</span></span>

<span data-ttu-id="7cb44-103">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7cb44-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7cb44-104">Supporta ASP.NET Core [inserimento di dipendenze](xref:fundamentals/dependency-injection) nelle viste.</span><span class="sxs-lookup"><span data-stu-id="7cb44-104">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="7cb44-105">Questo può essere utile per i servizi di visualizzazione specifica, ad esempio localizzazione o dati necessari solo per il popolamento di elementi di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="7cb44-105">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="7cb44-106">È consigliabile mantenere [separazione delle problematiche](http://deviq.com/separation-of-concerns/) tra il controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="7cb44-106">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="7cb44-107">La maggior parte dei dati di che visualizzano le visualizzazioni devono essere passata dal controller.</span><span class="sxs-lookup"><span data-stu-id="7cb44-107">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="7cb44-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7cb44-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="7cb44-109">Un esempio semplice</span><span class="sxs-lookup"><span data-stu-id="7cb44-109">A Simple Example</span></span>

<span data-ttu-id="7cb44-110">È possibile inserire un servizio in una visualizzazione utilizzando il `@inject` direttiva.</span><span class="sxs-lookup"><span data-stu-id="7cb44-110">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="7cb44-111">È possibile considerare `@inject` come aggiunta di una proprietà per la visualizzazione e la proprietà utilizzando DI popolamento.</span><span class="sxs-lookup"><span data-stu-id="7cb44-111">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="7cb44-112">La sintassi per `@inject`:`@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="7cb44-112">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="7cb44-113">Un esempio di `@inject` in azione:</span><span class="sxs-lookup"><span data-stu-id="7cb44-113">An example of `@inject` in action:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="7cb44-114">Consente di visualizzare un elenco di `ToDoItem` istanze, insieme a un riepilogo che mostra statistiche generali.</span><span class="sxs-lookup"><span data-stu-id="7cb44-114">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="7cb44-115">Il riepilogo è popolato da inserita `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="7cb44-115">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="7cb44-116">Questo servizio è registrato per l'inserimento di dipendenze in `ConfigureServices` in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7cb44-116">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="7cb44-117">Il `StatisticsService` esegue i calcoli sul set di `ToDoItem` istanze, a cui accede tramite un repository:</span><span class="sxs-lookup"><span data-stu-id="7cb44-117">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

<span data-ttu-id="7cb44-118">Il repository di esempio utilizza una raccolta in memoria.</span><span class="sxs-lookup"><span data-stu-id="7cb44-118">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="7cb44-119">L'implementazione illustrato in precedenza (che agisce su tutti i dati in memoria) non è consigliata per i set di dati di grandi dimensioni, a cui si accede in remoto.</span><span class="sxs-lookup"><span data-stu-id="7cb44-119">The implementation shown above (which operates on all of the data in memory) is not recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="7cb44-120">Nell'esempio vengono visualizzati i dati del modello associato alla vista e il servizio inserito nella vista:</span><span class="sxs-lookup"><span data-stu-id="7cb44-120">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Per visualizzare l'elenco di elementi totali, completare gli elementi, la priorità Media e un elenco di attività con i relativi livelli di priorità e i valori booleani che indica il completamento.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="7cb44-122">Inserimento dei dati di ricerca</span><span class="sxs-lookup"><span data-stu-id="7cb44-122">Populating Lookup Data</span></span>

<span data-ttu-id="7cb44-123">Attacco intrusivo nel codice di visualizzazione può essere utile per popolare le opzioni di elementi dell'interfaccia utente, ad esempio elenchi a discesa.</span><span class="sxs-lookup"><span data-stu-id="7cb44-123">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="7cb44-124">Si consideri un modulo di profilo utente che include opzioni per la specifica relativa al sesso, stato e altre preferenze.</span><span class="sxs-lookup"><span data-stu-id="7cb44-124">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="7cb44-125">Rendering di questo tipo un form utilizzando un approccio MVC standard richiederebbe il controller per richiedere dati di servizi di accesso per ognuno di questi set di opzioni e quindi compilare un modello o `ViewBag` con ogni set di opzioni da associare.</span><span class="sxs-lookup"><span data-stu-id="7cb44-125">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="7cb44-126">Un approccio alternativo inserisce i servizi direttamente nella visualizzazione per ottenere le opzioni.</span><span class="sxs-lookup"><span data-stu-id="7cb44-126">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="7cb44-127">In questo modo viene ridotta la quantità di codice necessario per il controller, spostare la logica di costruzione di questo elemento di visualizzazione nella vista stessa.</span><span class="sxs-lookup"><span data-stu-id="7cb44-127">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="7cb44-128">Azione del controller per visualizzare un form di modifica del profilo è sufficiente passare il form l'istanza di profilo:</span><span class="sxs-lookup"><span data-stu-id="7cb44-128">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="7cb44-129">Il form HTML utilizzato per aggiornare queste preferenze include elenchi a discesa per tre delle proprietà:</span><span class="sxs-lookup"><span data-stu-id="7cb44-129">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Aggiornare la vista profilo con un modulo che consente l'immissione di nome, sesso, stato e il colore preferito.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="7cb44-131">Questi elenchi vengono popolati da un servizio che è stato inserito nella visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="7cb44-131">These lists are populated by a service that has been injected into the view:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="7cb44-132">Il `ProfileOptionsService` è un servizio a livello di interfaccia utente progettato per fornire solo i dati necessari per questo form:</span><span class="sxs-lookup"><span data-stu-id="7cb44-132">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> <span data-ttu-id="7cb44-133">Non dimenticare di registrare tipi richiesto tramite l'inserimento di dipendenze nel `ConfigureServices` metodo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7cb44-133">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="7cb44-134">Si esegue l'override di servizi</span><span class="sxs-lookup"><span data-stu-id="7cb44-134">Overriding Services</span></span>

<span data-ttu-id="7cb44-135">Oltre l'inserimento di nuovi servizi, questa tecnica può essere usata anche per eseguire l'override di servizi precedentemente inseriti in una pagina.</span><span class="sxs-lookup"><span data-stu-id="7cb44-135">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="7cb44-136">La figura seguente mostra tutti i campi disponibili nella pagina utilizzato nel primo esempio:</span><span class="sxs-lookup"><span data-stu-id="7cb44-136">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Menu di scelta rapida di IntelliSense in un oggetto tipizzato elenco dei campi Html, componente, StatsService e Url simbolo @](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="7cb44-138">Come si può notare, i campi predefiniti includono `Html`, `Component`, e `Url` (così come il `StatsService` che abbiamo inserito).</span><span class="sxs-lookup"><span data-stu-id="7cb44-138">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="7cb44-139">Se ad esempio si desidera sostituire l'helper HTML predefinita con la propria, è possibile farlo facilmente con `@inject`:</span><span class="sxs-lookup"><span data-stu-id="7cb44-139">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="7cb44-140">Se si desidera estendere i servizi esistenti, è possibile utilizzare questa tecnica semplicemente durante ereditare o disposizione testo per l'implementazione esistente con il proprio.</span><span class="sxs-lookup"><span data-stu-id="7cb44-140">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="7cb44-141">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="7cb44-141">See Also</span></span>

* <span data-ttu-id="7cb44-142">Blog di Simon Timms: [recupero dei dati di ricerca nella visualizzazione](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="7cb44-142">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
