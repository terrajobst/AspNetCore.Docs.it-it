---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Lo Scaffolding di ASP.NET in Visual Studio 2013 | Documenti Microsoft
author: tfitzmac
description: Lo Scaffolding di ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506730"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="c7200-103">Scaffolding di ASP.NET in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c7200-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="c7200-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c7200-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c7200-105">Lo Scaffolding di ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c7200-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="c7200-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c7200-106">Overview</span></span>

<span data-ttu-id="c7200-107">Lo Scaffolding di ASP.NET è un framework di generazione di codice per applicazioni Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c7200-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="c7200-108">Visual Studio 2013 sono inclusi generatori di codice pre-installato per i progetti MVC e Web API.</span><span class="sxs-lookup"><span data-stu-id="c7200-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="c7200-109">Aggiungere lo scaffolding al progetto quando si desidera aggiungere rapidamente il codice che interagisce con i modelli di dati.</span><span class="sxs-lookup"><span data-stu-id="c7200-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="c7200-110">Utilizzare lo scaffolding, è possibile ridurre la quantità di tempo per sviluppare le operazioni standard per i dati nel progetto.</span><span class="sxs-lookup"><span data-stu-id="c7200-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="c7200-111">Per impostazione predefinita, Visual Studio 2013 non supporta la generazione di codice per un progetto di Web Form, ma è possibile usare lo scaffolding con Web Form aggiungendo le dipendenze MVC al progetto o l'installazione di un'estensione.</span><span class="sxs-lookup"><span data-stu-id="c7200-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="c7200-112">Entrambi gli approcci sono illustrati di seguito.</span><span class="sxs-lookup"><span data-stu-id="c7200-112">Both approaches are shown below.</span></span>

<span data-ttu-id="c7200-113">Visual Studio 2013 Update 2 (attualmente RC) offre la possibilità di estendere lo Scaffolding di ASP.NET per soddisfare i requisiti dello scenario.</span><span class="sxs-lookup"><span data-stu-id="c7200-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="c7200-114">Con questa funzionalità, è possibile creare un modello di scaffolding personalizzato e aggiungerlo alla finestra di dialogo Aggiungi nuova pagina.</span><span class="sxs-lookup"><span data-stu-id="c7200-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="c7200-115">All'interno del modello personalizzato, specificare il codice che viene generato quando si aggiunge un elemento di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="c7200-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="c7200-116">Per ulteriori informazioni, vedere [la creazione di un Scaffolder personalizzato per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="c7200-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7200-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c7200-117">Prerequisites</span></span>

<span data-ttu-id="c7200-118">Per usare lo Scaffolding di ASP.NET, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="c7200-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="c7200-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c7200-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="c7200-120">Strumenti di sviluppo Web (parte dell'installazione predefinita di Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="c7200-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="c7200-121">Framework Web ASP.NET e strumenti 2013 (parte dell'installazione predefinita di Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="c7200-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="c7200-122">Aggiungere un elemento di scaffolding MVC o Web API</span><span class="sxs-lookup"><span data-stu-id="c7200-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="c7200-123">Per aggiungere lo scaffolding, il progetto o una cartella all'interno del progetto e scegliere **Aggiungi** – **nuovo elemento di scaffolding**, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="c7200-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Aggiungere lo scaffolding elemento](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="c7200-125">Dal **aggiungere lo scaffolding** finestra, selezionare il tipo di pagina da aggiungere.</span><span class="sxs-lookup"><span data-stu-id="c7200-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Selezionare il tipo di pagina](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="c7200-127">Il **Aggiungi Controller** finestra consente di selezionare le opzioni per la generazione di controller, ad esempio se si desidera utilizzare le nuove funzionalità asincrone di Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="c7200-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Aggiungi controller](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="c7200-129">Le relative classi e le pagine vengono create per il proprio scenario.</span><span class="sxs-lookup"><span data-stu-id="c7200-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="c7200-130">Ad esempio, l'immagine seguente mostra le controller MVC e le viste che sono state create tramite lo scaffolding per una classe di modello denominato film.</span><span class="sxs-lookup"><span data-stu-id="c7200-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![I file creati](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="c7200-132">Aggiungere un elemento di scaffolding al Web Form</span><span class="sxs-lookup"><span data-stu-id="c7200-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="c7200-133">Per aggiungere lo scaffolding che genera il codice di Web Form, è necessario installare un'estensione di Visual Studio o aggiungere le dipendenze MVC.</span><span class="sxs-lookup"><span data-stu-id="c7200-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="c7200-134">Entrambi gli approcci sono illustrati di seguito, ma è necessario procedere in uno di questi approcci.</span><span class="sxs-lookup"><span data-stu-id="c7200-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="c7200-135">Web Form lo Scaffolding di estensione</span><span class="sxs-lookup"><span data-stu-id="c7200-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="c7200-136">È possibile installare un'estensione di Visual Studio che consentono di usare lo scaffolding con un progetto di Web Form.</span><span class="sxs-lookup"><span data-stu-id="c7200-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="c7200-137">In Visual Studio, selezionare **strumenti** e quindi **estensioni e aggiornamenti**.</span><span class="sxs-lookup"><span data-stu-id="c7200-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="c7200-138">Da questa finestra di dialogo di Visual Studio Gallery per ricerca **lo Scaffolding di Web Form**.</span><span class="sxs-lookup"><span data-stu-id="c7200-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![installare lo scaffolding di web form](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="c7200-140">Per ulteriori informazioni, vedere [lo Scaffolding di Web Form](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="c7200-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="c7200-141">Dipendenze MVC</span><span class="sxs-lookup"><span data-stu-id="c7200-141">MVC Dependencies</span></span>

<span data-ttu-id="c7200-142">Per aggiungere le dipendenze MVC, selezionare **Aggiungi** - **nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="c7200-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="c7200-143">Nella finestra, aggiungere lo scaffolding selezionare **le dipendenze MVC**, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c7200-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![aggiungere le dipendenze MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="c7200-145">Sono disponibili due opzioni per lo scaffolding MVC; Minimo e completo.</span><span class="sxs-lookup"><span data-stu-id="c7200-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="c7200-146">Se si seleziona minima, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="c7200-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="c7200-147">Se si seleziona l'opzione completo, vengono aggiunte le dipendenze minime, nonché i file di contenuto richiesti per un progetto MVC.</span><span class="sxs-lookup"><span data-stu-id="c7200-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="c7200-148">Per utilizzare facilmente lo scaffolding, selezionare le dipendenze complete.</span><span class="sxs-lookup"><span data-stu-id="c7200-148">To easily use scaffolding, select Full dependencies.</span></span>

![Selezionare le dipendenze complete](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="c7200-150">Dopo aver aggiunto le dipendenze, verrà visualizzato un **readme.txt** file.</span><span class="sxs-lookup"><span data-stu-id="c7200-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="c7200-151">Seguire attentamente le istruzioni in questo file per garantire il corretto funzionamento del progetto.</span><span class="sxs-lookup"><span data-stu-id="c7200-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="c7200-152">Quando si hanno completato i passaggi nel file Readme. txt, è possibile aggiungere un nuovo elemento di scaffolding come illustrato nella sezione precedente relativa MVC e Web API.</span><span class="sxs-lookup"><span data-stu-id="c7200-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="c7200-153">Generato automaticamente visualizzazioni e controller funzionerà correttamente all'interno del progetto.</span><span class="sxs-lookup"><span data-stu-id="c7200-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="c7200-154">Esercitazioni</span><span class="sxs-lookup"><span data-stu-id="c7200-154">Tutorials</span></span>

<span data-ttu-id="c7200-155">Per creare un scaffolder personalizzato, vedere [la creazione di un Scaffolder personalizzato per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="c7200-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="c7200-156">Per personalizzare i file generati, vedere [come personalizzare i file generati dalla finestra di dialogo Nuovo elemento di scaffolding](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="c7200-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="c7200-157">Per un esempio dell'utilizzo di scaffolding con **lo sviluppo del Database First**, vedere [Entity Framework Database First con ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="c7200-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="c7200-158">Per un esempio di utilizzo delle pagine di supporto temporaneo in un **MVC** del progetto, vedere [Introduzione a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="c7200-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="c7200-159">Per un esempio di utilizzo delle pagine di supporto temporaneo in un **API Web** del progetto, vedere [creare un'API REST con il Routing degli attributi in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="c7200-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
