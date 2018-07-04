---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Scaffolding di ASP.NET in Visual Studio 2013 | Microsoft Docs
author: tfitzmac
description: Scaffolding di ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: f014ef1439376349f49f10f1f4063945ab81e7c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369092"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="10b7d-103">Scaffolding di ASP.NET in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="10b7d-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="10b7d-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="10b7d-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="10b7d-105">Scaffolding di ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="10b7d-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="10b7d-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="10b7d-106">Overview</span></span>

<span data-ttu-id="10b7d-107">Scaffolding di ASP.NET è un framework di generazione di codice per applicazioni Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="10b7d-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="10b7d-108">Visual Studio 2013 include generatori di codice pre-installato per i progetti MVC e API Web.</span><span class="sxs-lookup"><span data-stu-id="10b7d-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="10b7d-109">Aggiungere lo scaffolding al progetto quando si desidera aggiungere rapidamente il codice che interagisce con i modelli di dati.</span><span class="sxs-lookup"><span data-stu-id="10b7d-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="10b7d-110">Usare lo scaffolding, è possibile ridurre la quantità di tempo per lo sviluppo di operazioni di dati standard nel progetto.</span><span class="sxs-lookup"><span data-stu-id="10b7d-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="10b7d-111">Per impostazione predefinita, Visual Studio 2013 non supporta la generazione di codice per un progetto di Web Form, ma è possibile usare lo scaffolding con Web Form mediante l'aggiunta di dipendenze MVC al progetto o installare un'estensione.</span><span class="sxs-lookup"><span data-stu-id="10b7d-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="10b7d-112">Entrambi gli approcci sono illustrati di seguito.</span><span class="sxs-lookup"><span data-stu-id="10b7d-112">Both approaches are shown below.</span></span>

<span data-ttu-id="10b7d-113">Visual Studio 2013 Update 2 (attualmente RC) offre la possibilità di estendere lo Scaffolding di ASP.NET per soddisfare i requisiti dello scenario.</span><span class="sxs-lookup"><span data-stu-id="10b7d-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="10b7d-114">Con questa funzionalità, è possibile creare un modello di scaffolding personalizzato e aggiungerlo alla finestra di dialogo Add Scaffold nuovo.</span><span class="sxs-lookup"><span data-stu-id="10b7d-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="10b7d-115">All'interno del modello personalizzato, specificare il codice che viene generato quando si aggiunge un elemento di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="10b7d-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="10b7d-116">Per altre informazioni, vedere [creazione di un'utilità di scaffolding personalizzati per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="10b7d-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10b7d-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="10b7d-117">Prerequisites</span></span>

<span data-ttu-id="10b7d-118">Per usare lo Scaffolding di ASP.NET, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="10b7d-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="10b7d-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="10b7d-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="10b7d-120">Strumenti di sviluppo Web (parte dell'installazione predefinita di Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="10b7d-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="10b7d-121">ASP.NET Web Frameworks e Tools 2013 (parte dell'installazione predefinita di Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="10b7d-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="10b7d-122">Aggiungere un elemento di scaffolding di MVC o API Web</span><span class="sxs-lookup"><span data-stu-id="10b7d-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="10b7d-123">Per aggiungere lo scaffolding, il progetto o una cartella all'interno del progetto e scegliere **Add** – **nuovo elemento di scaffolding**, come illustrato nell'immagine seguente.</span><span class="sxs-lookup"><span data-stu-id="10b7d-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Aggiungi elemento di scaffolding](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="10b7d-125">Dal **Add Scaffold** finestra, selezionare il tipo di scaffolding da aggiungere.</span><span class="sxs-lookup"><span data-stu-id="10b7d-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Selezionare il tipo di scaffolding](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="10b7d-127">Il **Aggiungi Controller** finestra ti offre la possibilità di selezionare le opzioni per la generazione di controller, ad esempio se si desidera utilizzare le nuove funzionalità asincrone da Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="10b7d-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Aggiungi controller](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="10b7d-129">Le classi pertinenti e le pagine vengono create per il proprio scenario.</span><span class="sxs-lookup"><span data-stu-id="10b7d-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="10b7d-130">Ad esempio, l'immagine seguente mostra il controller MVC e le viste che sono state create tramite scaffolding di una classe di modello denominata film.</span><span class="sxs-lookup"><span data-stu-id="10b7d-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![I file creati](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="10b7d-132">Aggiungere un elemento di scaffolding per Web Form</span><span class="sxs-lookup"><span data-stu-id="10b7d-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="10b7d-133">Per aggiungere lo scaffolding che genera il codice di Web Form, è necessario installare un'estensione per Visual Studio o Aggiungi dipendenze MVC.</span><span class="sxs-lookup"><span data-stu-id="10b7d-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="10b7d-134">Entrambi gli approcci sono illustrati di seguito, ma è sufficiente eseguire uno di questi approcci.</span><span class="sxs-lookup"><span data-stu-id="10b7d-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="10b7d-135">Web Forms Scaffolding estensione</span><span class="sxs-lookup"><span data-stu-id="10b7d-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="10b7d-136">È possibile installare un'estensione di Visual Studio che consentono di usare lo scaffolding con un progetto di Web Form.</span><span class="sxs-lookup"><span data-stu-id="10b7d-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="10b7d-137">In Visual Studio, selezionare **degli strumenti** e quindi **estensioni e aggiornamenti**.</span><span class="sxs-lookup"><span data-stu-id="10b7d-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="10b7d-138">Questa finestra di dialogo di ricerca per Visual Studio Gallery **estensione Web Forms Scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="10b7d-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![installare lo scaffolding di web form](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="10b7d-140">Per altre informazioni, vedere [estensione Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="10b7d-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="10b7d-141">Dipendenze MVC</span><span class="sxs-lookup"><span data-stu-id="10b7d-141">MVC Dependencies</span></span>

<span data-ttu-id="10b7d-142">Per aggiungere dipendenze MVC, selezionare **Add** - **nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="10b7d-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="10b7d-143">Nella finestra Aggiungi scaffolding, selezionare **dipendenze MVC**, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="10b7d-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![Aggiungi dipendenze MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="10b7d-145">Sono disponibili due opzioni per lo scaffolding di MVC; Minimal e complete.</span><span class="sxs-lookup"><span data-stu-id="10b7d-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="10b7d-146">Se si seleziona minima, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="10b7d-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="10b7d-147">Se si seleziona l'opzione completa, vengono aggiunte le dipendenze minime, nonché i necessari file di contenuto per un progetto MVC.</span><span class="sxs-lookup"><span data-stu-id="10b7d-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="10b7d-148">Per utilizzare facilmente lo scaffolding, selezionare dipendenze complete.</span><span class="sxs-lookup"><span data-stu-id="10b7d-148">To easily use scaffolding, select Full dependencies.</span></span>

![Selezionare le dipendenze complete](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="10b7d-150">Dopo aver aggiunto le dipendenze, si noterà una **Readme. txt** file.</span><span class="sxs-lookup"><span data-stu-id="10b7d-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="10b7d-151">Seguire attentamente le istruzioni in questo file per garantire che il progetto funziona correttamente.</span><span class="sxs-lookup"><span data-stu-id="10b7d-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="10b7d-152">Dopo aver completato i passaggi nel file Readme. txt, è possibile aggiungere un nuovo elemento di scaffolding, come illustrato nella sezione precedente su MVC e API Web.</span><span class="sxs-lookup"><span data-stu-id="10b7d-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="10b7d-153">Generato automaticamente visualizzazioni e controller funzionerà correttamente all'interno del progetto.</span><span class="sxs-lookup"><span data-stu-id="10b7d-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="10b7d-154">Esercitazioni</span><span class="sxs-lookup"><span data-stu-id="10b7d-154">Tutorials</span></span>

<span data-ttu-id="10b7d-155">Per creare un'utilità di scaffolding personalizzato, vedere [creazione di un'utilità di scaffolding personalizzati per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="10b7d-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="10b7d-156">Per personalizzare i file generati, vedere [come personalizzare i file generati dalla finestra di dialogo Nuovo elemento di scaffolding](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="10b7d-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="10b7d-157">Per un esempio d'uso con scaffolding **lo sviluppo di Database First**, vedere [Entity Framework Database First con ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="10b7d-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="10b7d-158">Per un esempio dell'uso di scaffolding in un' **MVC** del progetto, vedere [Introduzione a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="10b7d-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="10b7d-159">Per un esempio dell'uso di scaffolding in una **API Web** del progetto, vedere [creare un'API REST con il Routing con attributi nell'API Web 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="10b7d-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
