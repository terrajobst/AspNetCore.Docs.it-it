---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Aggiunta di un modello | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b3ef871c4d7627a03c8f0fd8cce9d3e97fc1a4ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867450"
---
<a name="adding-a-model"></a><span data-ttu-id="0a484-102">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="0a484-102">Adding a Model</span></span>
====================
<span data-ttu-id="0a484-103">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0a484-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="0a484-104">In questa sezione si aggiungeranno alcune classi per la gestione di filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="0a484-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="0a484-105">Queste classi sarà il &quot;modello&quot; fa parte dell'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0a484-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="0a484-106">Si utilizzerà una tecnologia di accesso ai dati di .NET Framework nota come il [Entity Framework](https://docs.microsoft.com/ef/) per definire e utilizzare queste classi di modello.</span><span class="sxs-lookup"><span data-stu-id="0a484-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="0a484-107">Entity Framework (noto anche come EF) supporta un paradigma di sviluppo denominato *Code First*.</span><span class="sxs-lookup"><span data-stu-id="0a484-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="0a484-108">Codice, innanzitutto, consente di creare gli oggetti del modello mediante la scrittura di classi semplici.</span><span class="sxs-lookup"><span data-stu-id="0a484-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="0a484-109">(Sono anche noti come classi POCO, da &quot;oggetti CLR normale precedente.&quot;) È il database creato al momento dalle classi, che consente a un flusso di lavoro di sviluppo molto pulito e rapido.</span><span class="sxs-lookup"><span data-stu-id="0a484-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="0a484-110">Se è necessario creare innanzitutto il database, è comunque possibile eseguire questa esercitazione per apprendere lo sviluppo di app MVC ed Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0a484-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="0a484-111">È quindi possibile seguire Tom Fizmakens [lo Scaffolding di ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) esercitazione che illustra il primo approccio di database.</span><span class="sxs-lookup"><span data-stu-id="0a484-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="0a484-112">Aggiunta di classi modello</span><span class="sxs-lookup"><span data-stu-id="0a484-112">Adding Model Classes</span></span>

<span data-ttu-id="0a484-113">In **Esplora soluzioni**, fare clic destro la *modelli* cartella, selezionare **Aggiungi**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="0a484-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="0a484-114">Immettere il *classe* nome &quot;film&quot;.</span><span class="sxs-lookup"><span data-stu-id="0a484-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="0a484-115">Aggiungere le seguenti cinque proprietà per il `Movie` classe:</span><span class="sxs-lookup"><span data-stu-id="0a484-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="0a484-116">Si userà la `Movie` classe per rappresentare filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="0a484-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="0a484-117">Ogni istanza di un `Movie` corrisponde a una riga all'interno di una tabella di database e ogni proprietà dell'oggetto di `Movie` classe verrà eseguito il mapping a una colonna nella tabella.</span><span class="sxs-lookup"><span data-stu-id="0a484-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="0a484-118">Nota: Per utilizzare System.Data.Entity e la classe correlata, è necessario installare il [pacchetto NuGet di Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="0a484-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="0a484-119">Fare clic sul collegamento per ulteriori istruzioni.</span><span class="sxs-lookup"><span data-stu-id="0a484-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="0a484-120">Nello stesso file, aggiungere le seguenti `MovieDBContext` classe:</span><span class="sxs-lookup"><span data-stu-id="0a484-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="0a484-121">Il `MovieDBContext` classe rappresenta il contesto del database film Entity Framework, che gestisce il recupero, l'archiviazione e l'aggiornamento `Movie` classe istanze in un database.</span><span class="sxs-lookup"><span data-stu-id="0a484-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="0a484-122">Il `MovieDBContext` deriva il `DbContext` classe forniti da Entity Framework di base.</span><span class="sxs-lookup"><span data-stu-id="0a484-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="0a484-123">Per poter fare riferimento a `DbContext` e `DbSet`, è necessario aggiungere il seguente `using` istruzione all'inizio del file:</span><span class="sxs-lookup"><span data-stu-id="0a484-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="0a484-124">È possibile farlo aggiungendo manualmente utilizzando l'istruzione oppure è possibile passare il mouse sulle righe ondulate rosse, fare clic su `Show potential fixes` e fare clic su `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="0a484-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="0a484-125">Nota: Alcuni inutilizzati `using` istruzioni sono state rimosse.</span><span class="sxs-lookup"><span data-stu-id="0a484-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="0a484-126">Visual Studio Visualizza dipendenze non utilizzate in grigio.</span><span class="sxs-lookup"><span data-stu-id="0a484-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="0a484-127">È possibile rimuovere le dipendenze non utilizzate passandovi sopra le dipendenze grigio, fare clic su `Show potential fixes` e fare clic su **Rimuovi using inutilizzate.**</span><span class="sxs-lookup"><span data-stu-id="0a484-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="0a484-128">Infine, è stato aggiunto un modello (M in MVC).</span><span class="sxs-lookup"><span data-stu-id="0a484-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="0a484-129">Nella sezione successiva si utilizzerà la stringa di connessione di database.</span><span class="sxs-lookup"><span data-stu-id="0a484-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0a484-130">[Precedente](adding-a-view.md)
> [Successivo](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="0a484-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
