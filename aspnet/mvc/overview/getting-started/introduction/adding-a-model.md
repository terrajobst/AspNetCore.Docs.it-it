---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Aggiunta di un modello | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 2d99a9c553ba79922cb0c5e852709ab97ea2e22b
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38211054"
---
<a name="adding-a-model"></a><span data-ttu-id="95882-102">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="95882-102">Adding a Model</span></span>
====================
<span data-ttu-id="95882-103">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="95882-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="95882-104">In questa sezione si aggiungeranno alcune classi per la gestione di film in un database.</span><span class="sxs-lookup"><span data-stu-id="95882-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="95882-105">Queste classi saranno la &quot;modello&quot; fa parte dell'app ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="95882-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="95882-106">Si userà una tecnologia di accesso ai dati di .NET Framework nota come il [Entity Framework](https://docs.microsoft.com/ef/) per definire e usare queste classi del modello.</span><span class="sxs-lookup"><span data-stu-id="95882-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="95882-107">Entity Framework (noto anche come Entity Framework) supporta un paradigma di sviluppo denominato *Code First*.</span><span class="sxs-lookup"><span data-stu-id="95882-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="95882-108">Prima di tutto i codice consente di creare gli oggetti del modello mediante la scrittura di classi semplici.</span><span class="sxs-lookup"><span data-stu-id="95882-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="95882-109">(Queste sono anche note come classi POCO, dalla &quot;plain-old CLR Object.&quot;) È quindi possibile avere il database creato in tempo reale dalle classi, che consente a un flusso di lavoro molto pulito e rapida evoluzione.</span><span class="sxs-lookup"><span data-stu-id="95882-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="95882-110">Se viene richiesto di creare il database prima di tutto, è comunque possibile seguire questa esercitazione per informazioni sullo sviluppo di app MVC ed Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="95882-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="95882-111">È quindi possibile seguire Tom Fizmakens [Scaffolding di ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) esercitazione che illustra il primo approccio di database.</span><span class="sxs-lookup"><span data-stu-id="95882-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="95882-112">Aggiunta di classi di modello</span><span class="sxs-lookup"><span data-stu-id="95882-112">Adding Model Classes</span></span>

<span data-ttu-id="95882-113">Nella **Esplora soluzioni**, fare clic il *modelli* cartella, selezionare **Add**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="95882-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="95882-114">Immettere il *classe* name &quot;film&quot;.</span><span class="sxs-lookup"><span data-stu-id="95882-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="95882-115">Aggiungere le seguenti cinque proprietà per il `Movie` classe:</span><span class="sxs-lookup"><span data-stu-id="95882-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="95882-116">Si userà il `Movie` classe per rappresentare i film in un database.</span><span class="sxs-lookup"><span data-stu-id="95882-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="95882-117">Ogni istanza di un `Movie` oggetto corrisponderà a una riga all'interno di una tabella di database e ogni proprietà del `Movie` classe verrà eseguito il mapping a una colonna nella tabella.</span><span class="sxs-lookup"><span data-stu-id="95882-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="95882-118">Nota: Per usare Data. Entity e la classe correlata, è necessario installare il [pacchetto NuGet di Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="95882-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="95882-119">Fare clic sul collegamento per ulteriori istruzioni.</span><span class="sxs-lookup"><span data-stu-id="95882-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="95882-120">Nello stesso file, aggiungere il codice seguente `MovieDBContext` classe:</span><span class="sxs-lookup"><span data-stu-id="95882-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="95882-121">Il `MovieDBContext` classe rappresenta il contesto di database di film Entity Framework, che gestisce il recupero, l'archiviazione e aggiornando `Movie` classe istanze in un database.</span><span class="sxs-lookup"><span data-stu-id="95882-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="95882-122">Il `MovieDBContext` deriva dal `DbContext` classe fornita da Entity Framework di base.</span><span class="sxs-lookup"><span data-stu-id="95882-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="95882-123">Per poter fare riferimento a `DbContext` e `DbSet`, è necessario aggiungere il codice seguente `using` informativa nella parte superiore del file:</span><span class="sxs-lookup"><span data-stu-id="95882-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="95882-124">Tale scopo, è possibile aggiungere manualmente l'utilizzo istruzione oppure è possibile passare il mouse sulle righe ondulate rosse, fare clic su `Show potential fixes` e fare clic su `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="95882-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="95882-125">Nota: Alcuni inutilizzati `using` istruzioni sono state rimosse.</span><span class="sxs-lookup"><span data-stu-id="95882-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="95882-126">Visual Studio mostrerà le dipendenze non usate in grigio.</span><span class="sxs-lookup"><span data-stu-id="95882-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="95882-127">È possibile rimuovere le dipendenze inutilizzate passandovi sopra le dipendenze grigio, fare clic su `Show potential fixes` e fare clic su **Rimuovi using inutilizzate.**</span><span class="sxs-lookup"><span data-stu-id="95882-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="95882-128">È stato infine aggiunto un modello (il valore M in MVC).</span><span class="sxs-lookup"><span data-stu-id="95882-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="95882-129">Nella sezione successiva si utilizzerà la stringa di connessione di database.</span><span class="sxs-lookup"><span data-stu-id="95882-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="95882-130">[Precedente](adding-a-view.md)
> [Successivo](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="95882-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
