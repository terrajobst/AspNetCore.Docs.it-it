---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: "Novità di ASP.NET MVC 4 | Documenti Microsoft"
author: rick-anderson
description: "ASP.NET MVC 4 è un framework per la compilazione di applicazioni web scalabili e basate su standard utilizzando schemi progettuali ben definiti e le potenzialità di ASP.NET e..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 8b1bdae048afc78399ccc7b0eac7125d9b983c13
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="3bed2-103">Novità di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3bed2-103">What's New in ASP.NET MVC 4</span></span>
====================
<span data-ttu-id="3bed2-104">da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="3bed2-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="3bed2-105">Download Web categorie Kit di formazione</span><span class="sxs-lookup"><span data-stu-id="3bed2-105">Download Web Camps Training Kit</span></span>](http://www.microsoft.com/download/29843)

> <span data-ttu-id="3bed2-106">ASP.NET MVC 4 è un framework per la compilazione di applicazioni web scalabili e basate su standard utilizzando schemi progettuali ben definiti e le potenzialità di ASP.NET e .NET framework.</span><span class="sxs-lookup"><span data-stu-id="3bed2-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="3bed2-107">Questa nuova, quarta versione di framework è incentrata su come rendere più semplice lo sviluppo di applicazioni web per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>
> 
> <span data-ttu-id="3bed2-108">Per iniziare, quando si crea un nuovo progetto ASP.NET MVC 4 è ora disponibile un modello di progetto di applicazione per dispositivi mobili che è possibile utilizzare per creare un'applicazione autonoma in particolare per i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="3bed2-109">Inoltre, ASP.NET MVC 4 si integra con jQuery Mobile tramite un pacchetto NuGet jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="3bed2-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="3bed2-110">jQuery Mobile è un framework basato su HTML5 per lo sviluppo di applicazioni web compatibili con tutte le piattaforme dei dispositivi mobili più comuni, tra cui Windows Phone, iPhone, Android e così via.</span><span class="sxs-lookup"><span data-stu-id="3bed2-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="3bed2-111">Tuttavia, se è necessario specializzazione, ASP.NET MVC 4 consente inoltre di fornire visualizzazioni diverse per diversi dispositivi e specificare le ottimizzazioni specifiche del dispositivo.</span><span class="sxs-lookup"><span data-stu-id="3bed2-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>
> 
> <span data-ttu-id="3bed2-112">In questa esercitazione pratica, si inizierà con ASP.NET MVC 4 &quot;applicazione Internet&quot; modello di progetto per creare un'applicazione Raccolta foto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="3bed2-113">Si procederà al miglioramento progressivamente l'app usando jQuery Mobile e nuove funzionalità di ASP.NET MVC 4 per assicurarsi che sia compatibile con diversi dispositivi mobili e browser desktop.</span><span class="sxs-lookup"><span data-stu-id="3bed2-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="3bed2-114">Si apprenderà inoltre nuove soluzioni di codice per la generazione di codice e come ASP.NET MVC 4 rende più semplice per la scrittura di metodi di azione asincroni grazie al supporto delle attività&lt;ActionResult&gt; tipi restituiti.</span><span class="sxs-lookup"><span data-stu-id="3bed2-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>
> 
> <span data-ttu-id="3bed2-115">Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="3bed2-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="3bed2-116">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="3bed2-116">Objectives</span></span>

<span data-ttu-id="3bed2-117">In questa esercitazione pratica, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="3bed2-117">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="3bed2-118">Sfruttare i miglioramenti per i modelli, compresi progetto di MVC ASP.NET il nuovo modello di progetto di applicazione per dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="3bed2-118">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="3bed2-119">Utilizzare le query di supporto CSS e attributo viewport HTML5 per migliorare la visualizzazione nei dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="3bed2-119">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="3bed2-120">Utilizzare jQuery Mobile per l'ottimizzazione progressivi e per la compilazione di interfaccia utente web ottimizzata per il tocco</span><span class="sxs-lookup"><span data-stu-id="3bed2-120">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="3bed2-121">Creare visualizzazioni specifiche di dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="3bed2-121">Create mobile-specific views</span></span>
- <span data-ttu-id="3bed2-122">Utilizzare il componente di selezione di visualizzazione per passare tra le visualizzazioni di dispositivi mobili e desktop nell'applicazione</span><span class="sxs-lookup"><span data-stu-id="3bed2-122">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="3bed2-123">Creare i controller asincroni utilizzando il supporto delle attività</span><span class="sxs-lookup"><span data-stu-id="3bed2-123">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="3bed2-124">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3bed2-124">Prerequisites</span></span>

<span data-ttu-id="3bed2-125">È necessario che gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="3bed2-125">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="3bed2-126">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice B](#AppendixB) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="3bed2-126">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="3bed2-127">[ASP.NET MVC 4](../../../mvc4.md) (incluso nell'installazione di Microsoft Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="3bed2-127">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="3bed2-128">Emulatore Windows Phone (incluso nel [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="3bed2-128">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="3bed2-129">Facoltativo: [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) con **Electric Plum iPhone simulatore** estensione (solo per esercizio 3 consente di visualizzare l'applicazione web con un simulatore di iPhone)</span><span class="sxs-lookup"><span data-stu-id="3bed2-129">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="3bed2-130">Configurazione</span><span class="sxs-lookup"><span data-stu-id="3bed2-130">Setup</span></span>

<span data-ttu-id="3bed2-131">In tutto il documento lab, verrà invitati a inserire i blocchi di codice.</span><span class="sxs-lookup"><span data-stu-id="3bed2-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="3bed2-132">Per praticità, la maggior parte di tale codice viene fornito come Visual Studio frammenti di codice che è possibile utilizzare da Visual Studio per evitare di dover aggiungerla manualmente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-132">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="3bed2-133">Per installare i frammenti di codice:</span><span class="sxs-lookup"><span data-stu-id="3bed2-133">To install the code snippets:</span></span>

1. <span data-ttu-id="3bed2-134">Aprire una finestra di Esplora risorse e individuare il lab **Source\Setup** cartella.</span><span class="sxs-lookup"><span data-stu-id="3bed2-134">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="3bed2-135">Fare doppio clic su di **Setup.cmd** file in questa cartella per installare i frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3bed2-135">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="3bed2-136">Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice a: con i frammenti di codice](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="3bed2-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="3bed2-137">Esercizi</span><span class="sxs-lookup"><span data-stu-id="3bed2-137">Exercises</span></span>

<span data-ttu-id="3bed2-138">Questa esercitazione include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3bed2-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="3bed2-139">Nuovi modelli di progetto ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3bed2-139">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="3bed2-140">Creazione dell'applicazione Web Raccolta foto</span><span class="sxs-lookup"><span data-stu-id="3bed2-140">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="3bed2-141">Aggiunta del supporto per i dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="3bed2-141">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="3bed2-142">Utilizzo di un controller asincrono</span><span class="sxs-lookup"><span data-stu-id="3bed2-142">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="3bed2-143">Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio.</span><span class="sxs-lookup"><span data-stu-id="3bed2-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="3bed2-144">Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="3bed2-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="3bed2-145">Tempo stimato per completare questa esercitazione: **60 minuti**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-145">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="3bed2-146">Esercizio 1: Nuovi modelli di progetto ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3bed2-146">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="3bed2-147">In questo esercizio verranno esplorati i miglioramenti nei modelli di progetto ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3bed2-147">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="3bed2-148">Oltre al modello di applicazione Internet, è già presente in MVC 3, questa versione include ora un modello distinto per le applicazioni mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-148">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="3bed2-149">In primo luogo, esaminerà alcune funzionalità rilevanti di ognuno dei modelli.</span><span class="sxs-lookup"><span data-stu-id="3bed2-149">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="3bed2-150">Quindi, verranno usati per il rendering della pagina correttamente su piattaforme diverse utilizzando l'approccio giusto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-150">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="3bed2-151">Attività 1: esplorare il modello di applicazione Internet</span><span class="sxs-lookup"><span data-stu-id="3bed2-151">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="3bed2-152">Aprire **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-152">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="3bed2-153">Selezionare il **File | New | Progetto** comando di menu.</span><span class="sxs-lookup"><span data-stu-id="3bed2-153">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="3bed2-154">Nel **nuovo progetto** finestra di dialogo Seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere **applicazione Web ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="3bed2-154">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="3bed2-155">Denominare il progetto **PhotoGallery**, selezionare un percorso (o lasciare il valore predefinito) e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-155">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-156">Successivamente si personalizzerà la soluzione PhotoGallery ASP.NET MVC 4 che ora si sta creando.</span><span class="sxs-lookup"><span data-stu-id="3bed2-156">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="3bed2-157">![Crea un nuovo progetto](whats-new-in-aspnet-mvc-4/_static/image1.png "creando un nuovo progetto")</span><span class="sxs-lookup"><span data-stu-id="3bed2-157">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="3bed2-158">*Crea un nuovo progetto*</span><span class="sxs-lookup"><span data-stu-id="3bed2-158">*Creating a new project*</span></span>
3. <span data-ttu-id="3bed2-159">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo Seleziona il **applicazione Internet** modello di progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-159">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="3bed2-160">Verificare che sia selezionato Razor come motore di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-160">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="3bed2-161">![Crea una nuova applicazione Internet ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image2.png "crea una nuova applicazione Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="3bed2-161">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="3bed2-162">*Crea una nuova applicazione Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="3bed2-162">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-163">La sintassi Razor è stato introdotto in ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="3bed2-163">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="3bed2-164">L'obiettivo consiste nel ridurre al minimo il numero di caratteri e sequenze di tasti richieste in un file, consentendo un rapido e fluido codifica del flusso di lavoro.</span><span class="sxs-lookup"><span data-stu-id="3bed2-164">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="3bed2-165">Sfrutta Razor esistente c# / VB (o altri) linguistiche e offre una sintassi di modello che consente un straordinario HTML costruzione del flusso di lavoro.</span><span class="sxs-lookup"><span data-stu-id="3bed2-165">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="3bed2-166">Premere **F5** per eseguire la soluzione e visualizzare i modelli rinnovati.</span><span class="sxs-lookup"><span data-stu-id="3bed2-166">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="3bed2-167">È possibile controllare le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="3bed2-167">You can check out the following features:</span></span>

    <span data-ttu-id="3bed2-168">**Modelli di stile moderno**</span><span class="sxs-lookup"><span data-stu-id="3bed2-168">**Modern-style templates**</span></span>

    <span data-ttu-id="3bed2-169">I modelli sono stati rinnovati, fornendo più stili dall'aspetto moderno.</span><span class="sxs-lookup"><span data-stu-id="3bed2-169">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="3bed2-170">![Modelli ASP.NET MVC 4 modificato nello stile](whats-new-in-aspnet-mvc-4/_static/image3.png "modificato nello stile di modelli di MVC 4")</span><span class="sxs-lookup"><span data-stu-id="3bed2-170">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="3bed2-171">*Modelli ASP.NET MVC 4 modificato nello stile*</span><span class="sxs-lookup"><span data-stu-id="3bed2-171">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="3bed2-172">![Nuova pagina contatto](whats-new-in-aspnet-mvc-4/_static/image4.png "pagina nuovo contatto")</span><span class="sxs-lookup"><span data-stu-id="3bed2-172">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="3bed2-173">*Nuova pagina di contatto*</span><span class="sxs-lookup"><span data-stu-id="3bed2-173">*New Contact page*</span></span>

    <span data-ttu-id="3bed2-174">**Rendering adattivo**</span><span class="sxs-lookup"><span data-stu-id="3bed2-174">**Adaptive Rendering**</span></span>

    <span data-ttu-id="3bed2-175">Estrarre il ridimensionamento della finestra del browser e notare come il layout di pagina si adatta dinamicamente per le nuove dimensioni della finestra.</span><span class="sxs-lookup"><span data-stu-id="3bed2-175">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="3bed2-176">Questi modelli utilizzano la tecnica di rendering adattivo per eseguire correttamente il rendering in piattaforme mobili e desktop senza personalizzazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-176">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="3bed2-177">![Modello di progetto ASP.NET MVC 4 in formati diversi browser](whats-new-in-aspnet-mvc-4/_static/image5.png "il modello di progetto ASP.NET MVC 4 in formati diversi browser")</span><span class="sxs-lookup"><span data-stu-id="3bed2-177">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="3bed2-178">*Modello di progetto ASP.NET MVC 4 in formati diversi browser*</span><span class="sxs-lookup"><span data-stu-id="3bed2-178">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="3bed2-179">**Interfaccia utente più completi con JavaScript**</span><span class="sxs-lookup"><span data-stu-id="3bed2-179">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="3bed2-180">Un altro miglioramento di modelli di progetto predefiniti è l'utilizzo di JavaScript per fornire un maggior livello di interattività JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3bed2-180">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="3bed2-181">I collegamenti di accesso e registrazione utilizzati nel modello esemplificare utilizzare jQuery convalide per convalidare i campi di input dal lato client.</span><span class="sxs-lookup"><span data-stu-id="3bed2-181">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![Convalida jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="3bed2-183">*Convalida jQuery*</span><span class="sxs-lookup"><span data-stu-id="3bed2-183">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-184">Si noti che i due log nelle sezioni, nella prima sezione è possono accedere utilizzando un account registrato dal sito e nella seconda sezione che è possibile altenativelly l'accesso utilizzando un altro servizio di autenticazione come google (disattivato per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="3bed2-184">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="3bed2-185">Chiudere il browser per arrestare il debugger e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3bed2-185">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="3bed2-186">Aprire il file **AuthConfig.cs** sotto il **App\_avviare** cartella.</span><span class="sxs-lookup"><span data-stu-id="3bed2-186">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="3bed2-187">Rimuovere il commento dall'ultima riga per registrare il client di Google per *OAuth* l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-187">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="3bed2-188">Si noti che è possibile abilitare facilmente l'autenticazione utilizzando qualsiasi servizio OpenID o OAuth come Facebook, Twitter, Microsoft, e così via.</span><span class="sxs-lookup"><span data-stu-id="3bed2-188">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="3bed2-189">Premere **F5** per eseguire la soluzione e passare alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="3bed2-189">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="3bed2-190">Selezionare **Google** servizio per l'accesso.</span><span class="sxs-lookup"><span data-stu-id="3bed2-190">Select **Google** service to log in.</span></span>

    ![Selezione del log nel servizio](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="3bed2-192">*Selezione del log nel servizio*</span><span class="sxs-lookup"><span data-stu-id="3bed2-192">*Selecting the log in service*</span></span>
10. <span data-ttu-id="3bed2-193">Accedere con l'account di Google.</span><span class="sxs-lookup"><span data-stu-id="3bed2-193">Log in using your Google account.</span></span>
11. <span data-ttu-id="3bed2-194">Consentire al sito (localhost) recuperare informazioni dall'account di Google.</span><span class="sxs-lookup"><span data-stu-id="3bed2-194">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="3bed2-195">Infine, è necessario registrare il sito per associare l'account di Google.</span><span class="sxs-lookup"><span data-stu-id="3bed2-195">Finally, you will have to register in the site to associate the Google account.</span></span>

    ![Associare l'account Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

    <span data-ttu-id="3bed2-197">*Associare l'account Google*</span><span class="sxs-lookup"><span data-stu-id="3bed2-197">*Associating your Google account*</span></span>
13. <span data-ttu-id="3bed2-198">Chiudere il browser per arrestare il debugger e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3bed2-198">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="3bed2-199">Esaminare ora la soluzione di estrarre alcune altre nuove funzionalità introdotte da ASP.NET MVC 4 nel modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-199">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="3bed2-200">![Il modello di progetto applicazione ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image9.png "il modello di progetto ASP.NET MVC 4 applicazione Internet")</span><span class="sxs-lookup"><span data-stu-id="3bed2-200">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="3bed2-201">*Il modello di progetto ASP.NET MVC 4 applicazione Internet*</span><span class="sxs-lookup"><span data-stu-id="3bed2-201">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    - <span data-ttu-id="3bed2-202">**HTML 5 Markup**</span><span class="sxs-lookup"><span data-stu-id="3bed2-202">**HTML 5 Markup**</span></span>

        <span data-ttu-id="3bed2-203">Sfogliare le viste di modello per individuare il nuovo tag di tema.</span><span class="sxs-lookup"><span data-stu-id="3bed2-203">Browse template views to find out the new theme markup.</span></span>

        <span data-ttu-id="3bed2-204">![Nuovo modello, utilizzando il markup About.cshtml Razor e HTML5. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Nuovo modello, utilizzando il markup About.cshtml Razor e HTML5.")</span><span class="sxs-lookup"><span data-stu-id="3bed2-204">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

        <span data-ttu-id="3bed2-205">*Nuovo modello, utilizzando il markup Razor e HTML5 (About.cshtml).*</span><span class="sxs-lookup"><span data-stu-id="3bed2-205">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
    - <span data-ttu-id="3bed2-206">**Librerie JavaScript aggiornate**</span><span class="sxs-lookup"><span data-stu-id="3bed2-206">**Updated JavaScript libraries**</span></span>

        <span data-ttu-id="3bed2-207">Il modello predefinito di ASP.NET MVC 4 include ora Knockout.js e un framework JavaScript MVVM che consente di creare applicazioni web ad alte utilizzando JavaScript e HTML.</span><span class="sxs-lookup"><span data-stu-id="3bed2-207">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="3bed2-208">Ad esempio in MVC 3, jQuery e jQuery librerie di interfaccia utente sono incluse anche in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3bed2-208">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

        > [!NOTE]
        > <span data-ttu-id="3bed2-209">È possibile ottenere ulteriori informazioni sulla libreria Knockout.js in questo collegamento: [ [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="3bed2-209">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="3bed2-210">Inoltre, è possibile acquisire informazioni jQuery e jQuery UI in [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="3bed2-210">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="3bed2-211">Attività 2: esplorare il modello di applicazione per dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="3bed2-211">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="3bed2-212">ASP.NET MVC 4 facilita lo sviluppo di siti Web per dispositivi mobili e tablet browser.</span><span class="sxs-lookup"><span data-stu-id="3bed2-212">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="3bed2-213">Questo modello ha la stessa struttura dell'applicazione come modello di applicazione Internet (si noti che il codice del controller è praticamente identico), ma lo stile è stato modificato per eseguire il rendering correttamente i dispositivi mobili basati su tocco.</span><span class="sxs-lookup"><span data-stu-id="3bed2-213">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="3bed2-214">Selezionare il **File | New | Progetto** comando di menu.</span><span class="sxs-lookup"><span data-stu-id="3bed2-214">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="3bed2-215">Nel **nuovo progetto** finestra di dialogo Seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere il **applicazione Web ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="3bed2-215">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="3bed2-216">Denominare il progetto **PhotoGallery.Mobile**, selezionare un percorso (o lasciare l'impostazione predefinita), selezionare &quot;aggiungere alla soluzione&quot; e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-216">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="3bed2-217">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo Seleziona il **applicazione Mobile** modello di progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-217">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="3bed2-218">Verificare che sia selezionato Razor come motore di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-218">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="3bed2-219">![Crea una nuova applicazione Mobile ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image11.png "crea una nuova applicazione Mobile ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="3bed2-219">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="3bed2-220">*Crea una nuova applicazione Mobile ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="3bed2-220">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="3bed2-221">Si è ora possibile esplorare la soluzione e verificare alcune delle nuove funzionalità introdotte dal modello di soluzione ASP.NET MVC 4 per dispositivi mobili:</span><span class="sxs-lookup"><span data-stu-id="3bed2-221">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="3bed2-222">**jQuery Mobile libreria**</span><span class="sxs-lookup"><span data-stu-id="3bed2-222">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="3bed2-223">Il modello di progetto di applicazione per dispositivi mobili include la libreria jQuery Mobile, che è una libreria open source per la compatibilità browser per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-223">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="3bed2-224">jQuery Mobile applica progressivo miglioramento per il browser per dispositivi mobili che supportano CSS e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3bed2-224">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="3bed2-225">Progressivo miglioramento consente a tutti i browser visualizzare il contenuto di base di una pagina web, mentre consente solo i browser più potenti visualizzare il contenuto RTF.</span><span class="sxs-lookup"><span data-stu-id="3bed2-225">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="3bed2-226">I file JavaScript e CSS, inclusi in jQuery Mobile stile, consentono di browser per dispositivi mobili per adattare il contenuto nella schermata senza apportare modifiche nel markup della pagina.</span><span class="sxs-lookup"><span data-stu-id="3bed2-226">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="3bed2-228">*libreria di jQuery mobile inclusa nel modello*</span><span class="sxs-lookup"><span data-stu-id="3bed2-228">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="3bed2-229">**Markup basato su HTML5**</span><span class="sxs-lookup"><span data-stu-id="3bed2-229">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="3bed2-231">*Modello di applicazione per dispositivi mobili tramite markup HTML5, (cshtml e cshtml)*</span><span class="sxs-lookup"><span data-stu-id="3bed2-231">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="3bed2-232">Premere **F5** per eseguire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-232">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="3bed2-233">Aprire il **Windows Phone 7 emulatore**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-233">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="3bed2-234">Nella schermata start telefono, aprire Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="3bed2-234">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="3bed2-235">Estrarre l'URL in cui l'applicazione desktop di avvio e passare a tale URL dal telefono (ad esempio `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="3bed2-235">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="3bed2-236">È ora in grado di accedere alla pagina di accesso o estrarre le informazioni sulla pagina.</span><span class="sxs-lookup"><span data-stu-id="3bed2-236">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="3bed2-237">Si noti che lo stile del sito Web è basato sulla nuova app di Windows Store per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-237">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="3bed2-238">Il modello di progetto ASP.NET MVC 4 sia visualizzato correttamente sui dispositivi mobili, assicurarsi che tutti gli elementi della pagina sono visibili e abilitati.</span><span class="sxs-lookup"><span data-stu-id="3bed2-238">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="3bed2-239">Si noti che i collegamenti nell'intestazione sono sufficientemente grandi per essere selezionato o scelte.</span><span class="sxs-lookup"><span data-stu-id="3bed2-239">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="3bed2-240">![Progetto pagine modello in un dispositivo mobile](whats-new-in-aspnet-mvc-4/_static/image14.png "progetto pagine modello in un dispositivo mobile")</span><span class="sxs-lookup"><span data-stu-id="3bed2-240">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="3bed2-241">*Pagine modello di progetto in un dispositivo mobile*</span><span class="sxs-lookup"><span data-stu-id="3bed2-241">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="3bed2-242">Utilizza inoltre il nuovo modello di **tag meta Viewport**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-242">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="3bed2-243">Browser per più dispositivi mobili definire una larghezza di una finestra del browser virtuale o &quot;viewport&quot;, che è maggiore della larghezza effettiva del dispositivo mobile.</span><span class="sxs-lookup"><span data-stu-id="3bed2-243">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="3bed2-244">In questo modo il browser per dispositivi mobili visualizzare l'intera pagina web all'interno di visualizzazione virtuale.</span><span class="sxs-lookup"><span data-stu-id="3bed2-244">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="3bed2-245">Il **tag meta Viewport** consente agli sviluppatori web impostare la larghezza, altezza e la scala dell'area del browser per dispositivi mobili **.**</span><span class="sxs-lookup"><span data-stu-id="3bed2-245">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="3bed2-246">Il modello ASP.NET MVC 4 per applicazioni per dispositivi mobili imposta il viewport per la larghezza del dispositivo (&quot;larghezza = dispositivo larghezza&quot;) nel modello di layout (*Views\Shared\_cshtml*), in modo che tutti i le pagine verranno impostati i viewport alla larghezza dello schermo di dispositivi.</span><span class="sxs-lookup"><span data-stu-id="3bed2-246">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="3bed2-247">Si noti che il tag meta del riquadro di visualizzazione non cambierà la visualizzazione del browser predefinito.</span><span class="sxs-lookup"><span data-stu-id="3bed2-247">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="3bed2-248">Aprire  **\_cshtml**, all'interno di **viste | Condiviso** cartella e commento per il tag meta Viewport.</span><span class="sxs-lookup"><span data-stu-id="3bed2-248">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="3bed2-249">Eseguire l'applicazione, se non è già aperto ed estrarre le differenze.</span><span class="sxs-lookup"><span data-stu-id="3bed2-249">Run the application, if not already opened, and check out the differences.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

    <span data-ttu-id="3bed2-250">![Il sito dopo l'impostazione come commento il tag meta viewport](whats-new-in-aspnet-mvc-4/_static/image15.png "il sito dopo il tag meta viewport ai commenti")</span><span class="sxs-lookup"><span data-stu-id="3bed2-250">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

    <span data-ttu-id="3bed2-251">*Il sito dopo il tag meta viewport ai commenti*</span><span class="sxs-lookup"><span data-stu-id="3bed2-251">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="3bed2-252">In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-252">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="3bed2-253">Rimuovere il tag meta viewport.</span><span class="sxs-lookup"><span data-stu-id="3bed2-253">Uncomment the viewport meta tag.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="3bed2-254">Attività 3: usare il Rendering adattivo</span><span class="sxs-lookup"><span data-stu-id="3bed2-254">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="3bed2-255">In questa attività si apprenderà a un altro metodo per eseguire il rendering di una pagina Web correttamente nei dispositivi mobili e browser Web nello stesso momento senza personalizzazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-255">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="3bed2-256">Tag meta Viewport è già stato utilizzato con uno scopo simile.</span><span class="sxs-lookup"><span data-stu-id="3bed2-256">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="3bed2-257">Ora è grado di soddisfare un altro metodo potente: *rendering adattivo*.</span><span class="sxs-lookup"><span data-stu-id="3bed2-257">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="3bed2-258">Il rendering adattivo è una tecnica che utilizza **query di CSS3 media** per personalizzare lo stile applicato a una pagina.</span><span class="sxs-lookup"><span data-stu-id="3bed2-258">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="3bed2-259">Query supporti definire condizioni all'interno di un foglio di stile, il raggruppamento di stili CSS in una determinata condizione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-259">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="3bed2-260">Solo quando la condizione è true, lo stile viene applicato all'oggetto dichiarato.</span><span class="sxs-lookup"><span data-stu-id="3bed2-260">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="3bed2-261">La flessibilità offerta dalla tecnica di rendering adattivo consente qualsiasi personalizzazione per la visualizzazione del sito in dispositivi diversi.</span><span class="sxs-lookup"><span data-stu-id="3bed2-261">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="3bed2-262">È possibile definire gli stili tante desiderato in un unico foglio di stile senza scrivere codice di logica per scegliere lo stile.</span><span class="sxs-lookup"><span data-stu-id="3bed2-262">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="3bed2-263">Pertanto, è un modo molto semplice di adattamento di stili di pagina, in quanto riduce la quantità di codice duplicato e la logica per ragioni di rendering.</span><span class="sxs-lookup"><span data-stu-id="3bed2-263">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="3bed2-264">D'altra parte, il consumo di larghezza di banda aumenterebbe, come le dimensioni dei file CSS potrebbero aumentare leggermente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-264">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="3bed2-265">Utilizzando la tecnica di rendering adattivo, sarà il sito **visualizzato correttamente, indipendentemente dal browser.**</span><span class="sxs-lookup"><span data-stu-id="3bed2-265">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="3bed2-266">Tuttavia, è consigliabile se la larghezza di banda aggiuntiva carica rappresenta un problema.</span><span class="sxs-lookup"><span data-stu-id="3bed2-266">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="3bed2-267">È il formato di base di una query supporti: @media \[ambito: tutti | palmari | stampare | proiezione | schermo\] ([proprietà: valore] e... [proprietà: valore])</span><span class="sxs-lookup"><span data-stu-id="3bed2-267">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="3bed2-268">Esempi di query di supporto: &gt;  **@media tutti e (larghezza massima: 1000px) e (min-width: 700px) {}:** per tutte le risoluzioni tra 700px e 1000px.</span><span class="sxs-lookup"><span data-stu-id="3bed2-268">Examples of media queries: &gt;**@media all and (max-width: 1000px) and (min-width: 700px) {}:** For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="3bed2-269">**@mediaschermata e (min-width: 400px) e (larghezza massima: 700px) {…}:** solo per le schermate.</span><span class="sxs-lookup"><span data-stu-id="3bed2-269">**@media screen and (min-width: 400px) and (max-width: 700px) { ... }:** Only for screens.</span></span> <span data-ttu-id="3bed2-270">La risoluzione deve essere compreso tra 400 e 700px.</span><span class="sxs-lookup"><span data-stu-id="3bed2-270">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="3bed2-271">**@mediapalmari e (min-width: 20em), schermo e (min-width: 20em) {…}:** per palmari (mobili e dispositivi) e schermate iniziali.</span><span class="sxs-lookup"><span data-stu-id="3bed2-271">**@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:** For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="3bed2-272">La larghezza minima deve essere maggiore di 20em.</span><span class="sxs-lookup"><span data-stu-id="3bed2-272">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="3bed2-273">È possibile trovare ulteriori informazioni sul [sito W3C](http://www.w3.org/TR/css3-mediaqueries/).</span><span class="sxs-lookup"><span data-stu-id="3bed2-273">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="3bed2-274">Verranno ora analizzate funzionamento il rendering, migliorando la leggibilità di ASP.NET MVC 4 predefiniti modello di sito Web.</span><span class="sxs-lookup"><span data-stu-id="3bed2-274">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="3bed2-275">Aprire il **PhotoGallery.sln** soluzione è stato creato nel passaggio 1 e selezionare il **PhotoGallery** progetto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-275">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="3bed2-276">Premere **F5** per eseguire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-276">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="3bed2-277">Ridimensionare la larghezza del browser, l'impostazione di windows per la metà o meno di un quarto della dimensione originale.</span><span class="sxs-lookup"><span data-stu-id="3bed2-277">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="3bed2-278">Notare cosa accade con gli elementi nell'intestazione: alcuni elementi non saranno visualizzati nell'area visibile dell'intestazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-278">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="3bed2-279">Aprire **Site.css** file da Esplora soluzioni di Visual Studio, si trova **contenuto** cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-279">Open **Site.css** file from the Visual Studio Solution explorer, located in **Content** project folder.</span></span> <span data-ttu-id="3bed2-280">Premere **CTRL + F** aprire ricerca integrata in Visual Studio e scrivere  **@media**  per individuare il **query supporti CSS**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-280">Press **CTRL + F** to open Visual Studio integrated search, and write **@media** to locate the **CSS media query**.</span></span>

    <span data-ttu-id="3bed2-281">La condizione di query supporti definita in questo modello funziona in questo modo: quando sono di dimensioni della finestra del browser sotto **850 px**, le regole CSS applicate sono quelli definiti all'interno del blocco di supporto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-281">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="3bed2-282">![Individuare la query di supporto](whats-new-in-aspnet-mvc-4/_static/image16.png "individuare la query di supporto")</span><span class="sxs-lookup"><span data-stu-id="3bed2-282">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="3bed2-283">*Individuare la query di supporto*</span><span class="sxs-lookup"><span data-stu-id="3bed2-283">*Locating the media query*</span></span>
4. <span data-ttu-id="3bed2-284">Sostituire il valore dell'attributo larghezza massima impostato in 850 px con **10px**, per disabilitare il rendering e premere **CTRL + S** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="3bed2-284">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="3bed2-285">Tornare al browser e premere **CTRL + F5** per aggiornare la pagina con le modifiche apportate.</span><span class="sxs-lookup"><span data-stu-id="3bed2-285">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="3bed2-286">Notare le differenze in entrambe le tabelle quando si modifica la larghezza della finestra.</span><span class="sxs-lookup"><span data-stu-id="3bed2-286">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="3bed2-287">![In a sinistra, applica la pagina di @media stile, lo stile di destra viene omesso](whats-new-in-aspnet-mvc-4/_static/image17.png "In a sinistra, applica la pagina il @media stile, lo stile di destra viene omesso")</span><span class="sxs-lookup"><span data-stu-id="3bed2-287">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="3bed2-288">*Nella parte sinistra, applica la pagina di @media stile, lo stile di destra viene omesso*</span><span class="sxs-lookup"><span data-stu-id="3bed2-288">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="3bed2-289">A questo punto, analizziamo cosa accade nei dispositivi mobili:</span><span class="sxs-lookup"><span data-stu-id="3bed2-289">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="3bed2-290">![In a sinistra, applica la pagina di @media stile, lo stile di destra viene omesso](whats-new-in-aspnet-mvc-4/_static/image18.png "In a sinistra, applica la pagina il @media stile, lo stile di destra viene omesso")</span><span class="sxs-lookup"><span data-stu-id="3bed2-290">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="3bed2-291">*Nella parte sinistra, applica la pagina di @media stile, lo stile di destra viene omesso*</span><span class="sxs-lookup"><span data-stu-id="3bed2-291">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="3bed2-292">Sebbene si noterà che le modifiche quando viene eseguito il rendering della pagina in un Web browser non sono molto importanti quando si utilizza un dispositivo mobile diventano più evidenti le differenze.</span><span class="sxs-lookup"><span data-stu-id="3bed2-292">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="3bed2-293">Sul lato sinistro dell'immagine, si noterà che lo stile personalizzato migliorata la leggibilità.</span><span class="sxs-lookup"><span data-stu-id="3bed2-293">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="3bed2-294">Il rendering adattivo è utilizzabile in più scenari, rendendo più semplice applicare condizionale stile a un sito Web e risolvere problemi comuni di stile con un approccio semplice.</span><span class="sxs-lookup"><span data-stu-id="3bed2-294">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="3bed2-295">Le query di supporto CSS e tag meta Viewport non sono specifiche di ASP.NET MVC 4, in modo da poter sfruttare queste funzionalità in qualsiasi applicazione web.</span><span class="sxs-lookup"><span data-stu-id="3bed2-295">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="3bed2-296">In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-296">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="3bed2-297">Esercizio 2: Creare l'applicazione Web Raccolta foto</span><span class="sxs-lookup"><span data-stu-id="3bed2-297">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="3bed2-298">In questo esercizio, verrà utilizzata in un'applicazione Raccolta foto per visualizzare le foto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-298">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="3bed2-299">Si inizierà con il modello di progetto ASP.NET MVC 4 e quindi si aggiungerà una funzionalità per recuperare le foto da un servizio e li visualizzano nella home page.</span><span class="sxs-lookup"><span data-stu-id="3bed2-299">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="3bed2-300">Nell'esercizio seguente, si aggiornerà questa soluzione per migliorare il modo in cui che viene visualizzato nei dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-300">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="3bed2-301">Attività 1 - Creazione di un servizio di foto di simulazione</span><span class="sxs-lookup"><span data-stu-id="3bed2-301">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="3bed2-302">In questa attività si creerà una simulazione del servizio per recuperare il contenuto che verrà visualizzato nella raccolta foto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-302">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="3bed2-303">A tale scopo, si aggiungerà un nuovo controller che restituirà semplicemente un file JSON con i dati di ogni foto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-303">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="3bed2-304">Aprire **Visual Studio** se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-304">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="3bed2-305">Selezionare il **File | New | Progetto** comando di menu.</span><span class="sxs-lookup"><span data-stu-id="3bed2-305">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="3bed2-306">Nel **nuovo progetto** finestra di dialogo Seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere **applicazione Web ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="3bed2-306">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="3bed2-307">Denominare il progetto **PhotoGallery**, selezionare un percorso (o lasciare il valore predefinito) e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-307">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="3bed2-308">In alternativa, è possibile continuare a lavorare da ASP.NET MVC 4 esistente **applicazione Internet** soluzione da **esercizio 1** e ignorare il passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="3bed2-308">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="3bed2-309">Nel **nuovo progetto ASP.NET MVC 4** la finestra di dialogo, seleziona il **applicazione Internet** modello di progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-309">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="3bed2-310">Verificare che sia selezionato come il motore di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="3bed2-310">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="3bed2-311">Nel **Esplora**, fare doppio clic su di **App\_dati** cartella del progetto e selezionare **Aggiungi | Elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-311">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="3bed2-312">Individuare il **Source\Assets\App\_dati** cartella di questa esercitazione e aggiungere il **Photos.json** file.</span><span class="sxs-lookup"><span data-stu-id="3bed2-312">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="3bed2-313">Creare un nuovo controller con il nome **PhotoController**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-313">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="3bed2-314">A tale scopo, fare clic su di **controller** cartella, passa a **Aggiungi** e selezionare **Controller.**</span><span class="sxs-lookup"><span data-stu-id="3bed2-314">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="3bed2-315">Completare il nome del controller, lasciare il **controller MVC vuoto** modello e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-315">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="3bed2-316">![Aggiunta di PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "aggiungendo il PhotoController")</span><span class="sxs-lookup"><span data-stu-id="3bed2-316">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="3bed2-317">*Aggiunta di PhotoController*</span><span class="sxs-lookup"><span data-stu-id="3bed2-317">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="3bed2-318">Sostituire il **indice** (metodo) con il codice seguente **raccolta** azione e restituire il contenuto del file JSON recente sono stati aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-318">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="3bed2-319">(- Frammento di codice *ASP.NET MVC 4 Lab - Ex02 - raccolta azione*)</span><span class="sxs-lookup"><span data-stu-id="3bed2-319">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="3bed2-320">Premere **F5** per eseguire la soluzione e quindi passare all'URL seguente per testare il servizio di foto fittizie: `http://localhost:[port]/photo/gallery` (il valore [porta] dipende da porta corrente in cui è stata avviata l'applicazione).</span><span class="sxs-lookup"><span data-stu-id="3bed2-320">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="3bed2-321">La richiesta a questo URL deve recuperare il contenuto del **Photos.json** file.</span><span class="sxs-lookup"><span data-stu-id="3bed2-321">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="3bed2-322">![Test del servizio di foto simulati](whats-new-in-aspnet-mvc-4/_static/image20.png "test del servizio di foto simulati")</span><span class="sxs-lookup"><span data-stu-id="3bed2-322">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="3bed2-323">*Test del servizio di foto simulati*</span><span class="sxs-lookup"><span data-stu-id="3bed2-323">*Testing the mocked photo service*</span></span>

<span data-ttu-id="3bed2-324">In un'implementazione reale è possibile utilizzare [ASP.NET Web API](../../../../web-api/index.md) per implementare il servizio di raccolta foto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-324">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="3bed2-325">API Web ASP.NET è un framework che rende più semplice compilare servizi HTTP in grado di raggiungono una vasta gamma di client, inclusi browser e dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-325">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="3bed2-326">API Web ASP.NET è la piattaforma ideale per compilare applicazioni RESTful in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3bed2-326">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="3bed2-327">Attività 2: visualizzazione della raccolta foto</span><span class="sxs-lookup"><span data-stu-id="3bed2-327">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="3bed2-328">In questa attività si aggiornerà la Home page per visualizzare la raccolta foto tramite il servizio simulato creato nella prima attività di questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="3bed2-328">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="3bed2-329">Si aggiungere file di modello e aggiornare le visualizzazioni di raccolta.</span><span class="sxs-lookup"><span data-stu-id="3bed2-329">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="3bed2-330">In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-330">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="3bed2-331">Creare il **foto** classe il **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="3bed2-331">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="3bed2-332">A tale scopo, fare clic su di **modelli** cartella, selezionare **Aggiungi** e fare clic su **classe**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-332">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="3bed2-333">Quindi, impostare il nome su **Photo.cs** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-333">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="3bed2-334">Aggiungere i membri seguenti per il **foto** classe.</span><span class="sxs-lookup"><span data-stu-id="3bed2-334">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="3bed2-335">(- Frammento di codice *modello ASP.NET MVC 4 Lab - Ex02 - foto*)</span><span class="sxs-lookup"><span data-stu-id="3bed2-335">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="3bed2-336">Aprire il file **HomeController.cs** dalla cartella **Controller**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-336">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="3bed2-337">Aggiungere le istruzioni using seguenti.</span><span class="sxs-lookup"><span data-stu-id="3bed2-337">Add the following using statements.</span></span>

    <span data-ttu-id="3bed2-338">(- Frammento di codice *ASP.NET MVC 4 Lab - Ex02 - HomeController using*)</span><span class="sxs-lookup"><span data-stu-id="3bed2-338">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="3bed2-339">Aggiornamento di **indice** azione da utilizzare **HttpClient** per recuperare i dati di raccolta e quindi utilizzare il **JavaScriptSerializer** di deserializzazione al modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-339">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="3bed2-340">(- Frammento di codice *operazione sull'indice di ASP.NET MVC 4 Lab - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="3bed2-340">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="3bed2-341">Aprire il **cshtml** file si trova sotto il **Views\Home** cartella e sostituire tutto il contenuto con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-341">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="3bed2-342">Questo codice consente di scorrere tutte le foto recuperate dal servizio e li visualizza in un elenco non ordinato.</span><span class="sxs-lookup"><span data-stu-id="3bed2-342">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="3bed2-343">(- Frammento di codice *elenco foto di ASP.NET MVC 4 Lab - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="3bed2-343">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="3bed2-344">Nel **Esplora**, fare doppio clic su di **contenuto** cartella del progetto e selezionare **Aggiungi | Elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-344">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="3bed2-345">Individuare il **Source\Assets\Content** cartella di questa esercitazione e aggiungere il **Site** file.</span><span class="sxs-lookup"><span data-stu-id="3bed2-345">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="3bed2-346">È necessario confermare la sostituzione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-346">You will have to confirm its replacement.</span></span> <span data-ttu-id="3bed2-347">Se si dispone di **Site.css** il file è aperto, è necessario per ricaricare il file anche confermare.</span><span class="sxs-lookup"><span data-stu-id="3bed2-347">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="3bed2-348">Aprire Esplora File e copiare l'intero **foto** cartella si trova sotto il **Source\Assets** cartella di questo laboratorio nella cartella radice del progetto in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="3bed2-348">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="3bed2-349">Eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-349">Run the application.</span></span> <span data-ttu-id="3bed2-350">Viene visualizzato la home page, visualizzare le foto nella raccolta.</span><span class="sxs-lookup"><span data-stu-id="3bed2-350">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="3bed2-351">![Raccolta foto](whats-new-in-aspnet-mvc-4/_static/image21.png "raccolta foto")</span><span class="sxs-lookup"><span data-stu-id="3bed2-351">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="3bed2-352">*Raccolta foto*</span><span class="sxs-lookup"><span data-stu-id="3bed2-352">*Photo Gallery*</span></span>
11. <span data-ttu-id="3bed2-353">In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-353">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="3bed2-354">Esercizio 3: Aggiunta del supporto per i dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="3bed2-354">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="3bed2-355">Uno degli aggiornamenti di chiave in ASP.NET MVC 4 è il supporto per lo sviluppo per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-355">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="3bed2-356">In questo esercizio verranno esplorati nuove funzionalità di ASP.NET MVC 4 per applicazioni per dispositivi mobili estendendo la soluzione PhotoGallery creato nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-356">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="3bed2-357">Attività 1: installazione jQuery Mobile in un'applicazione ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3bed2-357">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="3bed2-358">Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex3-MobileSupport/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="3bed2-358">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="3bed2-359">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-359">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="3bed2-360">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="3bed2-360">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3bed2-361">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-361">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3bed2-362">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="3bed2-362">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="3bed2-363">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-363">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-364">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-364">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3bed2-365">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-365">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3bed2-366">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-366">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3bed2-367">Aprire il **Package Manager Console** facendo il **strumenti** &gt; **Gestione pacchetti libreria** &gt; **Package Manager Console** opzione di menu.</span><span class="sxs-lookup"><span data-stu-id="3bed2-367">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="3bed2-368">![Aprire la Console di gestione pacchetti NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "aprire la Console di gestione pacchetti NuGet")</span><span class="sxs-lookup"><span data-stu-id="3bed2-368">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="3bed2-369">*Aprire la Console di gestione pacchetti NuGet*</span><span class="sxs-lookup"><span data-stu-id="3bed2-369">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="3bed2-370">Nella Console di gestione pacchetti, eseguire il comando seguente per installare il **jQuery.Mobile.MVC** pacchetto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-370">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="3bed2-371">jQuery Mobile è una libreria open source per la compilazione di interfaccia utente web ottimizzata per il tocco.</span><span class="sxs-lookup"><span data-stu-id="3bed2-371">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="3bed2-372">Il pacchetto NuGet jQuery.Mobile.MVC include gli helper per l'utilizzo di jQuery Mobile con un'applicazione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3bed2-372">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-373">Eseguendo il comando seguente, si verrà download della libreria di jQuery.Mobile.MVC da Nuget.</span><span class="sxs-lookup"><span data-stu-id="3bed2-373">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="3bed2-374">PM</span><span class="sxs-lookup"><span data-stu-id="3bed2-374">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="3bed2-375">Questo comando Installa jQuery Mobile e alcuni file di supporto, inclusi i seguenti:</span><span class="sxs-lookup"><span data-stu-id="3bed2-375">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="3bed2-376">**Views/Shared/\_Layout.Mobile.cshtml**: è un jQuery Mobile layout ottimizzato per uno schermo piccolo.</span><span class="sxs-lookup"><span data-stu-id="3bed2-376">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="3bed2-377">Quando il sito Web riceve una richiesta da un browser per dispositivi mobili, che sostituirà il layout originale (\_cshtml) con questo.</span><span class="sxs-lookup"><span data-stu-id="3bed2-377">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="3bed2-378">Un componente di selezione di visualizzazione: costituito il **Views/Shared/\_ViewSwitcher.cshtml** visualizzazione parziale e **ViewSwitcherController.cs** controller.</span><span class="sxs-lookup"><span data-stu-id="3bed2-378">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="3bed2-379">Questo componente visualizzerà un collegamento nel browser per dispositivi mobili per consentire agli utenti di passare alla versione desktop della pagina.</span><span class="sxs-lookup"><span data-stu-id="3bed2-379">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="3bed2-380">![Progetto raccolta foto con supporto mobile](whats-new-in-aspnet-mvc-4/_static/image23.png "progetto raccolta foto con supporto per dispositivi mobili")</span><span class="sxs-lookup"><span data-stu-id="3bed2-380">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="3bed2-381">*Progetto di raccolta foto con supporto per dispositivi mobili*</span><span class="sxs-lookup"><span data-stu-id="3bed2-381">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="3bed2-382">Registrare i dispositivi mobili bundle.</span><span class="sxs-lookup"><span data-stu-id="3bed2-382">Register the Mobile bundles.</span></span> <span data-ttu-id="3bed2-383">A tale scopo, aprire il **Global.asax.cs** file e aggiungere la riga seguente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-383">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="3bed2-384">(- Frammento di codice *ASP.NET MVC 4 Lab - Ex03 - registro mobili bundle*)</span><span class="sxs-lookup"><span data-stu-id="3bed2-384">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="3bed2-385">Eseguire l'applicazione utilizzando un web browser desktop.</span><span class="sxs-lookup"><span data-stu-id="3bed2-385">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="3bed2-386">Aprire il **emulatore di Windows Phone 7,** nella **Menu Start | Tutti i programmi | Windows Phone SDK 7.1 | Emulatore Windows Phone.**</span><span class="sxs-lookup"><span data-stu-id="3bed2-386">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="3bed2-387">Nella schermata start telefono, aprire Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="3bed2-387">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="3bed2-388">Estrarre l'URL in cui avviare l'applicazione e passare a tale URL con il browser phone (ad esempio `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="3bed2-388">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="3bed2-389">Si noterà che l'applicazione avrà un aspetto diverso nell'emulatore Windows Phone, come il jQuery.Mobile.MVC ha creato nuove risorse nel progetto che mostrano viste ottimizzate per i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-389">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="3bed2-390">Si noti il messaggio nella parte superiore del telefono, che mostra il collegamento che consente di passare alla visualizzazione Desktop.</span><span class="sxs-lookup"><span data-stu-id="3bed2-390">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="3bed2-391">Inoltre, il  **\_Layout.Mobile.cshtml** layout in cui è stato creato il pacchetto è stato installato è incluso un layout diverso nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-391">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-392">Finora, non sussiste alcun collegamento per tornare alla visualizzazione mobile.</span><span class="sxs-lookup"><span data-stu-id="3bed2-392">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="3bed2-393">Per essere inclusa nelle versioni successive.</span><span class="sxs-lookup"><span data-stu-id="3bed2-393">It will be included in later versions.</span></span>

    <span data-ttu-id="3bed2-394">![Vista mobile della pagina principale della raccolta foto](whats-new-in-aspnet-mvc-4/_static/image24.png "mobili visualizzazione della pagina Home raccolta foto")</span><span class="sxs-lookup"><span data-stu-id="3bed2-394">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="3bed2-395">*Vista mobile della pagina principale della raccolta foto*</span><span class="sxs-lookup"><span data-stu-id="3bed2-395">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="3bed2-396">In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-396">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="3bed2-397">Attività 2: creazione di visualizzazioni per dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="3bed2-397">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="3bed2-398">In questa attività si creerà una versione per dispositivi mobili della visualizzazione dell'indice con contenuto adattato per una migliore appareance nei dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-398">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="3bed2-399">Copia il **Views\Home\Index.cshtml** consente di visualizzare e incollarlo per creare una copia, rinominare il nuovo file **Index.Mobile.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-399">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="3bed2-400">Aprire il nuovo creata **Index.Mobile.cshtml** consente di visualizzare e sostituire &lt;ul&gt; tag con questo codice.</span><span class="sxs-lookup"><span data-stu-id="3bed2-400">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="3bed2-401">In questo modo, si aggiornano i &lt;ul&gt; tag con le annotazioni dei dati per dispositivi mobili jQuery per usare i temi di jQuery mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-401">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="3bed2-402">Si noti che:</span><span class="sxs-lookup"><span data-stu-id="3bed2-402">Notice that:</span></span>
    > 
    > - <span data-ttu-id="3bed2-403">Il **ruolo dati** attributo impostato su **listview** verrà eseguito il rendering di elenco con gli stili di listview.</span><span class="sxs-lookup"><span data-stu-id="3bed2-403">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="3bed2-404">Il **dati inset** attributo impostato su true visualizzerà l'elenco con bordo arrotondato e margine.</span><span class="sxs-lookup"><span data-stu-id="3bed2-404">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="3bed2-405">Il **filtro dati** attributo impostato su **true** genererà una casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="3bed2-405">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="3bed2-406">Altre informazioni sulle convenzioni per jQuery Mobile nella documentazione di progetto: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="3bed2-406">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="3bed2-407">Premere **CTRL + S** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="3bed2-407">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="3bed2-408">Passare il **emulatore Windows Phone** e aggiornare il sito.</span><span class="sxs-lookup"><span data-stu-id="3bed2-408">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="3bed2-409">Si noti il nuovo aspetto dell'elenco della raccolta, nonché la nuova casella di ricerca si trova nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="3bed2-409">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="3bed2-410">Quindi, digitare una parola nella casella di ricerca (ad esempio, **Tulips**) per avviare una ricerca nella raccolta foto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-410">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="3bed2-411">![Raccolta con lo stile di listview con filtro](whats-new-in-aspnet-mvc-4/_static/image25.png "utilizzando lo stile di listview con filtri di raccolta")</span><span class="sxs-lookup"><span data-stu-id="3bed2-411">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="3bed2-412">*Raccolta con lo stile di listview con filtro*</span><span class="sxs-lookup"><span data-stu-id="3bed2-412">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="3bed2-413">Per riepilogare, la Guida per la visualizzazione Mobilizer hanno utilizzato per creare una copia della visualizzazione dell'indice con il &quot;mobili&quot; suffisso.</span><span class="sxs-lookup"><span data-stu-id="3bed2-413">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="3bed2-414">Questo suffisso indica ad ASP.NET MVC 4 che ogni richiesta generata da un dispositivo mobile utilizzerà questa copia dell'indice.</span><span class="sxs-lookup"><span data-stu-id="3bed2-414">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="3bed2-415">Inoltre, è stato aggiornato la versione per dispositivi mobili della visualizzazione dell'indice da utilizzare jQuery Mobile per migliorare l'aspetto del sito nei dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-415">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="3bed2-416">Tornare a Visual Studio e aprire **Site.Mobile.css** sotto il **contenuto** cartella.</span><span class="sxs-lookup"><span data-stu-id="3bed2-416">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="3bed2-417">Correggere la posizione del titolo della foto per renderlo Mostra sul lato destro dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="3bed2-417">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="3bed2-418">A tale scopo, aggiungere il codice seguente per il **Site.Mobile.css** file.</span><span class="sxs-lookup"><span data-stu-id="3bed2-418">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="3bed2-419">CSS</span><span class="sxs-lookup"><span data-stu-id="3bed2-419">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="3bed2-420">Premere **CTRL + S** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="3bed2-420">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="3bed2-421">Tornare al **emulatore Windows Phone** e aggiornare il sito.</span><span class="sxs-lookup"><span data-stu-id="3bed2-421">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="3bed2-422">Si noti che il titolo di foto viene correttamente posizionato ora.</span><span class="sxs-lookup"><span data-stu-id="3bed2-422">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="3bed2-423">![Titolo posizionato sul lato destro dell'immagine](whats-new-in-aspnet-mvc-4/_static/image26.png "titolo posizionato sul lato destro dell'immagine")</span><span class="sxs-lookup"><span data-stu-id="3bed2-423">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="3bed2-424">*Titolo posizionato sul lato destro dell'immagine*</span><span class="sxs-lookup"><span data-stu-id="3bed2-424">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="3bed2-425">Attività 3 - jQuery Mobile temi</span><span class="sxs-lookup"><span data-stu-id="3bed2-425">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="3bed2-426">Ogni layout e i widget di jQuery Mobile viene progettato attorno a un nuovo framework di CSS orientata agli oggetti che consente di applicare un tema completo unificati visual a siti e applicazioni.</span><span class="sxs-lookup"><span data-stu-id="3bed2-426">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="3bed2-427">Per impostazione predefinita jQuery Mobile tema include 5 campioni forniti lettere (, b, c, d, e) di riferimento rapido.</span><span class="sxs-lookup"><span data-stu-id="3bed2-427">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="3bed2-428">In questa attività, è possibile aggiornare il layout per dispositivi mobili per l'utilizzo di un tema diverso da quello predefinito.</span><span class="sxs-lookup"><span data-stu-id="3bed2-428">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="3bed2-429">Tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3bed2-429">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="3bed2-430">Aprire il  **\_Layout.Mobile.cshtml** file si trova in **Views\Shared**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-430">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="3bed2-431">Trovare l'elemento div con il ruolo di dati impostato su &quot;pagina&quot; e aggiornare il **dati tema** a &quot; **e**&quot;.</span><span class="sxs-lookup"><span data-stu-id="3bed2-431">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="3bed2-432">Premere **CTRL + S** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="3bed2-432">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="3bed2-433">Aggiornare il sito nel **emulatore Windows Phone** e notare che la nuova combinazione di colori.</span><span class="sxs-lookup"><span data-stu-id="3bed2-433">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="3bed2-434">![Layout per dispositivi mobili con una combinazione di colori diversi](whats-new-in-aspnet-mvc-4/_static/image27.png "layout per dispositivi mobili con una combinazione di colori diversi")</span><span class="sxs-lookup"><span data-stu-id="3bed2-434">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="3bed2-435">*Layout per dispositivi mobili con una combinazione di colori diversi*</span><span class="sxs-lookup"><span data-stu-id="3bed2-435">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="3bed2-436">Attività 4: utilizzo del componente di selezione di visualizzazione e il Browser si esegue l'override di funzioni</span><span class="sxs-lookup"><span data-stu-id="3bed2-436">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="3bed2-437">Una convenzione per dispositivi mobili con ottimizzazione per le pagine web consiste nell'aggiungere un collegamento il cui testo è un elemento come visualizzazione Desktop o modalità completa del sito che consente agli utenti di passare a una versione desktop della pagina.</span><span class="sxs-lookup"><span data-stu-id="3bed2-437">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="3bed2-438">Il pacchetto jQuery.Mobile.MVC è incluso un esempio **-Selezione tipo di visualizzazione** componente per questo scopo utilizzato nel  **\_Layout.Mobile.cshtml** visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-438">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="3bed2-439">![Collegamento per passare alla visualizzazione Desktop](whats-new-in-aspnet-mvc-4/_static/image28.png "collegamento per passare alla visualizzazione Desktop")</span><span class="sxs-lookup"><span data-stu-id="3bed2-439">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="3bed2-440">*Collegamento per passare alla visualizzazione Desktop*</span><span class="sxs-lookup"><span data-stu-id="3bed2-440">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="3bed2-441">La selezione della vista utilizza una nuova funzionalità denominata **si esegue l'override di Browser**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-441">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="3bed2-442">Questa funzionalità consente di considerare le richieste come provenienti da un altro browser (agente utente) rispetto a quella da che effettivamente provenire.</span><span class="sxs-lookup"><span data-stu-id="3bed2-442">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="3bed2-443">In questa attività si esploreranno l'implementazione di esempio di una selezione di visualizzazione aggiunta jQuery.Mobile.MVC e il nuovo browser si esegue l'override delle funzionalità in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3bed2-443">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="3bed2-444">Tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3bed2-444">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="3bed2-445">Aprire il  **\_Layout.Mobile.cshtml** visualizzazione si trova sotto il **Views\Shared** cartella e notare il componente di selezione di visualizzazione a cui fa riferimento come visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="3bed2-445">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="3bed2-446">![Layout per dispositivi mobili utilizzando il componente di visualizzazione selezione](whats-new-in-aspnet-mvc-4/_static/image29.png "layout per dispositivi mobili utilizzando il componente di selezione di visualizzazione")</span><span class="sxs-lookup"><span data-stu-id="3bed2-446">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="3bed2-447">*Layout per dispositivi mobili utilizzando il componente di selezione di visualizzazione*</span><span class="sxs-lookup"><span data-stu-id="3bed2-447">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="3bed2-448">Aprire il  **\_ViewSwitcher.cshtml** visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="3bed2-448">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="3bed2-449">La visualizzazione parziale viene utilizzato il nuovo metodo **ViewContext.HttpContext.GetOverriddenBrowser()** per determinare l'origine della richiesta web e visualizzare il collegamento corrispondente per passare uno alle visualizzazioni dei Desktop o Mobile.</span><span class="sxs-lookup"><span data-stu-id="3bed2-449">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="3bed2-450">Il **GetOverridenBrowser** metodo restituisce un **HttpBrowserCapabilitiesBase** istanza che corrisponde all'agente utente attualmente impostata per la richiesta (effettivo o sottoposto a override).</span><span class="sxs-lookup"><span data-stu-id="3bed2-450">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="3bed2-451">È possibile utilizzare questo valore per ottenere le proprietà, ad esempio **IsMobileDevice**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-451">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="3bed2-452">![Visualizzazione parziale ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "visualizzazione parziale ViewSwitcher")</span><span class="sxs-lookup"><span data-stu-id="3bed2-452">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="3bed2-453">*Visualizzazione parziale ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="3bed2-453">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="3bed2-454">Aprire il **ViewSwitcherController.cs** classe si trova nel **controller** cartella.</span><span class="sxs-lookup"><span data-stu-id="3bed2-454">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="3bed2-455">Check-out di tale azione SwitchView viene chiamato dal collegamento nel componente ViewSwitcher e notare i nuovi metodi HttpContext.</span><span class="sxs-lookup"><span data-stu-id="3bed2-455">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="3bed2-456">Il **HttpContext.ClearOverridenBrowser()** metodo rimuove qualsiasi agente utente sottoposto a override per la richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-456">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="3bed2-457">Il **HttpContext.SetOverridenBrowser()** metodo esegue l'override di valore dell'agente utente effettivo della richiesta utilizzando l'agente utente specificato.</span><span class="sxs-lookup"><span data-stu-id="3bed2-457">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="3bed2-458">![Controller ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span><span class="sxs-lookup"><span data-stu-id="3bed2-458">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="3bed2-459">*Controller ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="3bed2-459">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="3bed2-460">Sostituzione di browser è una funzionalità centrale di ASP.NET MVC 4, anch ' esso disponibile anche se non si installa il pacchetto jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="3bed2-460">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="3bed2-461">Tuttavia, questa funzionalità riguarda solo visualizzazione, layout e visualizzazione parziale e non si applica una delle funzionalità che dipendono dall'oggetto Request.</span><span class="sxs-lookup"><span data-stu-id="3bed2-461">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="3bed2-462">Attività 5 - aggiunta al commutatore della visualizzazione nella vista del Desktop</span><span class="sxs-lookup"><span data-stu-id="3bed2-462">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="3bed2-463">In questa attività, è possibile aggiornare il layout desktop per includere la selezione di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-463">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="3bed2-464">In questo modo gli utenti mobili tornare alla visualizzazione mobile durante l'esplorazione visualizzazione desktop.</span><span class="sxs-lookup"><span data-stu-id="3bed2-464">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="3bed2-465">Aggiornare il sito nel **emulatore Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-465">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="3bed2-466">Fare clic su di **visualizzazione Desktop** collegamento nella parte superiore della raccolta.</span><span class="sxs-lookup"><span data-stu-id="3bed2-466">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="3bed2-467">Si noti non esiste alcun commutatore della visualizzazione nella visualizzazione desktop consente che tornare alla visualizzazione mobile.</span><span class="sxs-lookup"><span data-stu-id="3bed2-467">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="3bed2-468">Tornare a Visual Studio e aprire il  **\_cshtml** visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-468">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="3bed2-469">Individuare la sezione di accesso e inserire una chiamata per eseguire il rendering di  **\_ViewSwitcher** visualizzazione parziale riportato di seguito il  **\_LogOnPartial** visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="3bed2-469">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="3bed2-470">Premere quindi **CTRL + S** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="3bed2-470">Then, press **CTRL + S** to save the changes.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="3bed2-471">Premere **CTRL + S** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="3bed2-471">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="3bed2-472">Aggiornare la pagina nell'emulatore Windows Phone e fare doppio clic su schermo per fare zoom avanti.</span><span class="sxs-lookup"><span data-stu-id="3bed2-472">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="3bed2-473">Si noti che verrà visualizzata la home page di **vista Mobile** collegamento che consente di passare alla visualizzazione desktop da dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-473">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="3bed2-474">![Visualizzare il rendering nella visualizzazione desktop selezione](whats-new-in-aspnet-mvc-4/_static/image32.png "Selezione tipo di visualizzazione eseguito il rendering nella visualizzazione desktop")</span><span class="sxs-lookup"><span data-stu-id="3bed2-474">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="3bed2-475">*Selezione tipo di visualizzazione eseguito il rendering nella visualizzazione desktop*</span><span class="sxs-lookup"><span data-stu-id="3bed2-475">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="3bed2-476">Passare alla visualizzazione Mobile nuovamente e passare a **su** pagina (http://localhost [porta] / Home/su).</span><span class="sxs-lookup"><span data-stu-id="3bed2-476">Switch to the Mobile view again and browse to **About** page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="3bed2-477">Si noti che, anche se è stata creata una vista About.Mobile.cshtml, informazioni su verrà visualizzata la pagina utilizzando il layout di dispositivi mobili (\_Layout.Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="3bed2-477">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="3bed2-478">![Informazioni sulla pagina](whats-new-in-aspnet-mvc-4/_static/image33.png "sulla pagina")</span><span class="sxs-lookup"><span data-stu-id="3bed2-478">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="3bed2-479">*Informazioni sulla pagina*</span><span class="sxs-lookup"><span data-stu-id="3bed2-479">*About page*</span></span>
8. <span data-ttu-id="3bed2-480">Infine, è possibile aprire il sito in un browser Web per computer desktop.</span><span class="sxs-lookup"><span data-stu-id="3bed2-480">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="3bed2-481">Si noti che nessuno dei precedenti aggiornamenti riguarda la visualizzazione desktop.</span><span class="sxs-lookup"><span data-stu-id="3bed2-481">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="3bed2-482">![Visualizzazione desktop PhotoGallery](whats-new-in-aspnet-mvc-4/_static/image34.png "visualizzazione desktop PhotoGallery")</span><span class="sxs-lookup"><span data-stu-id="3bed2-482">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="3bed2-483">*Visualizzazione desktop PhotoGallery*</span><span class="sxs-lookup"><span data-stu-id="3bed2-483">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="3bed2-484">Attività 6: creazione di nuove modalità di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="3bed2-484">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="3bed2-485">La nuova funzionalità di modalità di visualizzazione consente a un'applicazione di selezionare le viste in base al browser che genera la richiesta.</span><span class="sxs-lookup"><span data-stu-id="3bed2-485">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="3bed2-486">Ad esempio, un browser desktop richiede la Home page, l'applicazione verrà restituito il **Views\Home\Index.cshtml** modello.</span><span class="sxs-lookup"><span data-stu-id="3bed2-486">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="3bed2-487">Quindi, se un browser per dispositivi mobili richiede la Home page, l'applicazione verrà restituito il **Views\Home\Index.mobile.cshtml** modello.</span><span class="sxs-lookup"><span data-stu-id="3bed2-487">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="3bed2-488">In questa attività si creerà un layout personalizzato per i dispositivi iPhone e sarà necessario simulare le richieste provenienti da dispositivi iPhone.</span><span class="sxs-lookup"><span data-stu-id="3bed2-488">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="3bed2-489">A tale scopo, è possibile utilizzare entrambi un emulatore o simulatore di iPhone (ad esempio [Electric simulatore di dispositivi mobili](http://www.electricplum.com/)) o un browser con i componenti aggiuntivi che modificano l'agente utente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-489">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="3bed2-490">Per istruzioni su come impostare la stringa agente utente in un browser Safari emulare un iPhone, vedere [descritto come consentire Safari fingono è IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) nel blog di David Alison.</span><span class="sxs-lookup"><span data-stu-id="3bed2-490">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="3bed2-491">**Si noti che questa attività è facoltativa ed è possibile continuare del laboratorio senza eseguirla.**</span><span class="sxs-lookup"><span data-stu-id="3bed2-491">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="3bed2-492">In Visual Studio, premere **MAIUSC** + **F5** per arrestare il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-492">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="3bed2-493">Aprire **Global.asax.cs** e aggiungere la seguente istruzione using.</span><span class="sxs-lookup"><span data-stu-id="3bed2-493">Open **Global.asax.cs** and add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="3bed2-494">Aggiungere il codice evidenziato di seguito all'applicazione\_Start (metodo).</span><span class="sxs-lookup"><span data-stu-id="3bed2-494">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="3bed2-495">(- Frammento di codice *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="3bed2-495">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

    <span data-ttu-id="3bed2-496">È stato registrato un nuovo **DefaultDisplayMode denominato &quot;iPhone&quot;**, all'interno statico **DisplayModeProvider.Instance.Modes** elenco statico, che verrà confrontato con ogni richiesta in ingresso.</span><span class="sxs-lookup"><span data-stu-id="3bed2-496">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="3bed2-497">Se la richiesta in ingresso contiene la stringa &quot;iPhone&quot;, ASP.NET MVC troverà le visualizzazioni il cui nome contiene il &quot;iPhone&quot; suffisso.</span><span class="sxs-lookup"><span data-stu-id="3bed2-497">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="3bed2-498">Il parametro 0 indica specifica è la nuova modalità; ad esempio, questa vista è più specifica generale &quot;.mobile&quot; regola corrispondente richieste dai dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-498">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

    <span data-ttu-id="3bed2-499">Dopo questo codice viene eseguito quando un browser iPhone che genera una richiesta, l'applicazione utilizzerà il **Views\Shared\\_Layout.iPhone.cshtml** layout verrà creato nei passaggi successivi.</span><span class="sxs-lookup"><span data-stu-id="3bed2-499">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-500">In questo modo, il test della richiesta per iPhone è stato semplificato per scopi dimostrativi e potrebbe non funzionare come previsto per ogni stringa agente utente di iPhone (per il test di esempio viene fatta distinzione tra maiuscole e minuscole).</span><span class="sxs-lookup"><span data-stu-id="3bed2-500">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>
4. <span data-ttu-id="3bed2-501">Creare una copia del  **\_Layout.Mobile.cshtml** file nel **Views\Shared** cartella e rinominare la copia &quot;  **\_Layout.iPhone.csthml** &quot;.</span><span class="sxs-lookup"><span data-stu-id="3bed2-501">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="3bed2-502">Aprire  **\_Layout.iPhone.csthml** creato nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-502">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="3bed2-503">Trovare l'elemento div con l'attributo data-role impostato su **pagina** e modificare il **dati tema** attributo &quot; **un**&quot;.</span><span class="sxs-lookup"><span data-stu-id="3bed2-503">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

    <span data-ttu-id="3bed2-504">Ora si dispone di 3 layout nell'applicazione ASP.NET MVC 4:</span><span class="sxs-lookup"><span data-stu-id="3bed2-504">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

    1. <span data-ttu-id="3bed2-505">**\_Cshtml**: layout predefinito utilizzato per i browser desktop.</span><span class="sxs-lookup"><span data-stu-id="3bed2-505">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
    2. <span data-ttu-id="3bed2-506">**\_Layout.Mobile.cshtml**: layout predefinito utilizzato per i dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="3bed2-506">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
    3. <span data-ttu-id="3bed2-507">**\_Layout.iPhone.cshtml**: layout specifico per i dispositivi iPhone, utilizzando una combinazione di colori diversi per distinguere da \_Layout.mobile.cshtml.</span><span class="sxs-lookup"><span data-stu-id="3bed2-507">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="3bed2-508">Premere **F5** per eseguire l'applicazione e selezionare il sito di **emulatore Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-508">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="3bed2-509">Aprire un **simulatore di iPhone** (vedere [appendice C](#AppendixC) per istruzioni su come installare e configurare un simulatore di iPhone) e passare al sito troppo.</span><span class="sxs-lookup"><span data-stu-id="3bed2-509">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="3bed2-510">Si noti che ogni phone utilizza il modello specifico.</span><span class="sxs-lookup"><span data-stu-id="3bed2-510">Notice that each phone is using the specific template.</span></span>

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="3bed2-512">*Utilizzo di visualizzazioni diverse per ogni dispositivo mobile*</span><span class="sxs-lookup"><span data-stu-id="3bed2-512">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="3bed2-513">Esercizio 4: Utilizzo di controller asincroni</span><span class="sxs-lookup"><span data-stu-id="3bed2-513">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="3bed2-514">Microsoft .NET Framework 4.5 introduce nuove funzionalità del linguaggio in c# e Visual Basic per fornire una nuova base per la modalità asincrona nella programmazione .NET.</span><span class="sxs-lookup"><span data-stu-id="3bed2-514">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="3bed2-515">Questo nuovo foundation rende la programmazione asincrona simile a - e circa semplice come quello - programmazione sincrona.</span><span class="sxs-lookup"><span data-stu-id="3bed2-515">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="3bed2-516">Questo punto si è in grado di scrivere metodi di azione asincroni in ASP.NET MVC 4 con la **AsyncController** classe.</span><span class="sxs-lookup"><span data-stu-id="3bed2-516">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="3bed2-517">È possibile utilizzare i metodi di azione asincroni per l'esecuzione prolungata non associate alla CPU richieste.</span><span class="sxs-lookup"><span data-stu-id="3bed2-517">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="3bed2-518">Ciò evita che il server Web di esecuzione del lavoro durante l'elaborazione della richiesta di blocco.</span><span class="sxs-lookup"><span data-stu-id="3bed2-518">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="3bed2-519">La classe AsyncController viene in genere utilizzata per le chiamate al servizio Web con esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="3bed2-519">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="3bed2-520">Questo esercizio illustra i concetti fondamentali dell'operazione asincrona in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3bed2-520">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="3bed2-521">Se si desidera un approfondimento, è possibile leggere l'articolo seguente: [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="3bed2-521">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="3bed2-522">Attività 1: implementazione di un Controller asincrono</span><span class="sxs-lookup"><span data-stu-id="3bed2-522">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="3bed2-523">Aprire il **iniziare** soluzione che si trova in **origine/Ex4-Async/Begin/** cartella.</span><span class="sxs-lookup"><span data-stu-id="3bed2-523">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="3bed2-524">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-524">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="3bed2-525">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="3bed2-525">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3bed2-526">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-526">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3bed2-527">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="3bed2-527">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="3bed2-528">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-528">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-529">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-529">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3bed2-530">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-530">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3bed2-531">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-531">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3bed2-532">Aprire il **HomeController.cs** classe il **controller** cartella.</span><span class="sxs-lookup"><span data-stu-id="3bed2-532">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="3bed2-533">Aggiungere la seguente istruzione using.</span><span class="sxs-lookup"><span data-stu-id="3bed2-533">Add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="3bed2-534">Aggiornamento di **HomeController** classe da cui ereditare **AsyncController**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-534">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="3bed2-535">I controller che derivano da AsyncController consentono ad ASP.NET di elaborare le richieste asincrone e possono comunque metodi di azione sincroni di servizio.</span><span class="sxs-lookup"><span data-stu-id="3bed2-535">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="3bed2-536">Aggiungere il **async** (parola chiave) per il **indice** (metodo) e il tipo restituito **attività&lt;ActionResult&gt;**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-536">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="3bed2-537">Il **async** (parola chiave) è uno delle nuove parole chiave fornisce .NET Framework 4.5; indica al compilatore che questo metodo contiene codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="3bed2-537">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="3bed2-538">Oggetto **attività** oggetto rappresenta un'operazione asincrona che può essere completata in seguito a un certo punto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-538">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="3bed2-539">Sostituire il **client. GetAsync()** chiamata con la versione completa async utilizzando parola chiave await come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="3bed2-539">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="3bed2-540">(- Frammento di codice *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span><span class="sxs-lookup"><span data-stu-id="3bed2-540">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="3bed2-541">Nella versione precedente, si utilizzava il **risultato** proprietà il **attività** oggetto per bloccare il thread fino a quando il risultato viene restituito (versione di sincronizzazione).</span><span class="sxs-lookup"><span data-stu-id="3bed2-541">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="3bed2-542">Aggiunta di **await** parola chiave indica al compilatore di attendere in modo asincrono l'attività restituita dalla chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="3bed2-542">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="3bed2-543">Ciò significa che il resto del codice verrà eseguito come un callback solo dopo il metodo atteso il completamento.</span><span class="sxs-lookup"><span data-stu-id="3bed2-543">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="3bed2-544">Un altro aspetto da notare è che non è necessario modificare il blocco try-catch per ottenere questo risultato: le eccezioni che si verificano in background o in primo piano continuano a essere intercettate senza necessità di operazioni utilizzando un gestore fornito dal framework.</span><span class="sxs-lookup"><span data-stu-id="3bed2-544">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="3bed2-545">Modificare il codice per continuare con l'implementazione asincrona, sostituendo le righe con il nuovo codice, come illustrato di seguito</span><span class="sxs-lookup"><span data-stu-id="3bed2-545">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="3bed2-546">(- Frammento di codice *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span><span class="sxs-lookup"><span data-stu-id="3bed2-546">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="3bed2-547">Eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-547">Run the application.</span></span> <span data-ttu-id="3bed2-548">Non si noterà che nessuna modifica rilevante, ma il codice non verrà bloccata un thread dal pool di thread, un migliore utilizzo delle risorse del server e di migliorare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="3bed2-548">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-549">Maggiori informazioni sulle nuove funzionalità di programmazione asincrona nell'ambiente lab &quot; **la programmazione asincrona in .NET 4.5 con c# e Visual Basic** &quot; inclusi nel Kit di formazione Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3bed2-549">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="3bed2-550">Attività 2: i timeout di gestione con i token di annullamento</span><span class="sxs-lookup"><span data-stu-id="3bed2-550">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="3bed2-551">Metodi di azione asincroni che restituiscono istanze di attività possono inoltre supportare i timeout.</span><span class="sxs-lookup"><span data-stu-id="3bed2-551">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="3bed2-552">In questa attività si aggiornerà il codice del metodo di indice per la gestione di uno scenario di timeout usando un token di annullamento.</span><span class="sxs-lookup"><span data-stu-id="3bed2-552">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="3bed2-553">Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="3bed2-553">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="3bed2-554">Aggiungere la seguente istruzione using il **HomeController.cs** file.</span><span class="sxs-lookup"><span data-stu-id="3bed2-554">Add the following using statement to the **HomeController.cs** file.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="3bed2-555">Aggiornare l'operazione sull'indice per ricevere un **CancellationToken** argomento.</span><span class="sxs-lookup"><span data-stu-id="3bed2-555">Update the Index action to receive a **CancellationToken** argument.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="3bed2-556">Aggiornamento di **GetAsync** chiamata per passare il token di annullamento.</span><span class="sxs-lookup"><span data-stu-id="3bed2-556">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="3bed2-557">(- Frammento di codice *SendAsync Lab - Ex04 - ASP.NET MVC 4 con oggetto CancellationToken*)</span><span class="sxs-lookup"><span data-stu-id="3bed2-557">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="3bed2-558">Decorare il *indice* metodo con un **AsyncTimeout** attributo impostato su 500 millisecondi e **HandleError** attributo configurato per gestire  **TaskCanceledException** mediante il reindirizzamento a un **TimedOut** visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-558">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="3bed2-559">(- Frammento di codice *gli attributi di ASP.NET MVC 4 Lab - Ex04 -*)</span><span class="sxs-lookup"><span data-stu-id="3bed2-559">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="3bed2-560">Aprire il **PhotoController** classe e l'aggiornamento di **raccolta** metodo ritardare l'esecuzione 1000 millisecondi (1 secondo), per simulare un'attività a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="3bed2-560">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="3bed2-561">Aprire il **Web. config** file e abilitare gli errori personalizzati, aggiungere l'elemento seguente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-561">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>


    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="3bed2-562">Creare una nuova vista in **Views\Shared** denominato **TimedOut** e usare il layout predefinito.</span><span class="sxs-lookup"><span data-stu-id="3bed2-562">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="3bed2-563">In Esplora soluzioni fare doppio clic su di **Views\Shared** cartella e selezionare **Aggiungi | Visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-563">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="3bed2-564">![Utilizzo di visualizzazioni diverse per ogni dispositivo mobile](whats-new-in-aspnet-mvc-4/_static/image36.png "usando diverse visualizzazioni diverse per ogni dispositivo mobile")</span><span class="sxs-lookup"><span data-stu-id="3bed2-564">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="3bed2-565">*Utilizzo di visualizzazioni diverse per ogni dispositivo mobile*</span><span class="sxs-lookup"><span data-stu-id="3bed2-565">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="3bed2-566">Aggiornamento di **TimedOut** visualizzare il contenuto, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="3bed2-566">Update the **TimedOut** view content as shown below.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="3bed2-567">Eseguire l'applicazione e passare all'URL radice.</span><span class="sxs-lookup"><span data-stu-id="3bed2-567">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="3bed2-568">Come è stato aggiunto un **Sleep** di 1000 millisecondi, si otterrà un errore di timeout, generato dal **AsyncTimeout** degli attributi e intercettare per il **HandleError** attributo.</span><span class="sxs-lookup"><span data-stu-id="3bed2-568">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="3bed2-569">![Eccezione di timeout gestita](whats-new-in-aspnet-mvc-4/_static/image37.png "gestita l'eccezione di timeout")</span><span class="sxs-lookup"><span data-stu-id="3bed2-569">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="3bed2-570">*Eccezione di timeout gestita*</span><span class="sxs-lookup"><span data-stu-id="3bed2-570">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="3bed2-571">Inoltre, è possibile distribuire l'applicazione per siti Web di Azure seguenti [pubblicazione appendice d: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixD).</span><span class="sxs-lookup"><span data-stu-id="3bed2-571">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="3bed2-572">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="3bed2-572">Summary</span></span>

<span data-ttu-id="3bed2-573">In questa esercitazione pratica, è stata osservata alcune delle nuove funzionalità residenti in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3bed2-573">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="3bed2-574">Sono stati trattati i concetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="3bed2-574">The following concepts have been discussed:</span></span>

- <span data-ttu-id="3bed2-575">Sfruttare i miglioramenti per i modelli, compresi progetto di MVC ASP.NET il nuovo modello di progetto di applicazione per dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="3bed2-575">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="3bed2-576">Utilizzare le query di supporto CSS e attributo viewport HTML5 per migliorare la visualizzazione nei dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="3bed2-576">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="3bed2-577">Utilizzare jQuery Mobile per l'ottimizzazione progressivi e per la compilazione di interfaccia utente web ottimizzata per il tocco</span><span class="sxs-lookup"><span data-stu-id="3bed2-577">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="3bed2-578">Creare visualizzazioni specifiche di dispositivi mobili</span><span class="sxs-lookup"><span data-stu-id="3bed2-578">Create mobile-specific views</span></span>
- <span data-ttu-id="3bed2-579">Utilizzare il componente di selezione di visualizzazione per passare tra le visualizzazioni di dispositivi mobili e desktop nell'applicazione</span><span class="sxs-lookup"><span data-stu-id="3bed2-579">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="3bed2-580">Creare i controller asincroni utilizzando il supporto delle attività</span><span class="sxs-lookup"><span data-stu-id="3bed2-580">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="3bed2-581">Appendice a: utilizzo dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="3bed2-581">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="3bed2-582">Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="3bed2-582">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="3bed2-583">Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="3bed2-583">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="3bed2-584">![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](whats-new-in-aspnet-mvc-4/_static/image38.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="3bed2-584">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="3bed2-585">*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="3bed2-585">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="3bed2-586">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="3bed2-586">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="3bed2-587">Posizionare il cursore dove si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="3bed2-587">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="3bed2-588">Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).</span><span class="sxs-lookup"><span data-stu-id="3bed2-588">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="3bed2-589">Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="3bed2-589">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="3bed2-590">Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="3bed2-590">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="3bed2-591">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="3bed2-591">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="3bed2-592">![Iniziare a digitare il nome del frammento di codice](whats-new-in-aspnet-mvc-4/_static/image39.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="3bed2-592">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="3bed2-593">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="3bed2-593">*Start typing the snippet name*</span></span>

<span data-ttu-id="3bed2-594">![Premere Tab per selezionare il frammento di codice evidenziata](whats-new-in-aspnet-mvc-4/_static/image40.png "premere Tab per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="3bed2-594">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="3bed2-595">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="3bed2-595">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="3bed2-596">![Premere Tab nuovamente e il frammento di codice verranno espansi](whats-new-in-aspnet-mvc-4/_static/image41.png "espanderà premere Tab nuovamente e il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="3bed2-596">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="3bed2-597">*Premere Tab nuovamente e il frammento di codice verranno espansi*</span><span class="sxs-lookup"><span data-stu-id="3bed2-597">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="3bed2-598">***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)***</span><span class="sxs-lookup"><span data-stu-id="3bed2-598">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="3bed2-599">Fare clic in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="3bed2-599">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="3bed2-600">Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-600">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="3bed2-601">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="3bed2-601">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="3bed2-602">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](whats-new-in-aspnet-mvc-4/_static/image42.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="3bed2-602">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="3bed2-603">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="3bed2-603">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="3bed2-604">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](whats-new-in-aspnet-mvc-4/_static/image43.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="3bed2-604">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="3bed2-605">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="3bed2-605">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="3bed2-606">Appendice b: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="3bed2-606">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="3bed2-607">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il  **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="3bed2-607">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="3bed2-608">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="3bed2-608">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="3bed2-609">Passare a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="3bed2-609">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="3bed2-610">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *Visual Studio Express 2012 per Web con Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="3bed2-610">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="3bed2-611">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-611">Click on **Install Now**.</span></span> <span data-ttu-id="3bed2-612">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="3bed2-612">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="3bed2-613">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-613">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="3bed2-614">![Installa Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="3bed2-614">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="3bed2-615">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="3bed2-615">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="3bed2-616">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="3bed2-616">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="3bed2-618">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="3bed2-618">*Accepting the license terms*</span></span>
5. <span data-ttu-id="3bed2-619">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-619">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="3bed2-621">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="3bed2-621">*Installation progress*</span></span>
6. <span data-ttu-id="3bed2-622">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-622">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="3bed2-624">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="3bed2-624">*Installation completed*</span></span>
7. <span data-ttu-id="3bed2-625">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="3bed2-625">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="3bed2-626">Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="3bed2-626">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express per il riquadro Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="3bed2-628">*VS Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="3bed2-628">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="3bed2-629">Appendice c: installazione WebMatrix 2 e iPhone simulatore</span><span class="sxs-lookup"><span data-stu-id="3bed2-629">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="3bed2-630">Per eseguire il sito in un dispositivo simulato iPhone è possibile utilizzare l'estensione di WebMatrix &quot;Electric simulatore di dispositivi mobili per iPhone&quot;.</span><span class="sxs-lookup"><span data-stu-id="3bed2-630">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="3bed2-631">Inoltre, è possibile configurare la stessa estensione per eseguire il simulatore di Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="3bed2-631">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="3bed2-632">Attività 1: installazione di WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="3bed2-632">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="3bed2-633">Passare a [ [https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="3bed2-633">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="3bed2-634">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *WebMatrix 2*&quot;.</span><span class="sxs-lookup"><span data-stu-id="3bed2-634">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*WebMatrix 2*&quot;.</span></span>
2. <span data-ttu-id="3bed2-635">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-635">Click on **Install Now**.</span></span> <span data-ttu-id="3bed2-636">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="3bed2-636">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="3bed2-637">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-637">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="3bed2-638">![Installare WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "installare WebMatrix 2")</span><span class="sxs-lookup"><span data-stu-id="3bed2-638">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="3bed2-639">*Installare WebMatrix 2*</span><span class="sxs-lookup"><span data-stu-id="3bed2-639">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="3bed2-640">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="3bed2-640">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="3bed2-641">![Accettare le condizioni di licenza](whats-new-in-aspnet-mvc-4/_static/image50.png "accettando le condizioni di licenza")</span><span class="sxs-lookup"><span data-stu-id="3bed2-641">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="3bed2-642">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="3bed2-642">*Accepting the license terms*</span></span>
5. <span data-ttu-id="3bed2-643">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-643">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="3bed2-644">![Lo stato dell'installazione](whats-new-in-aspnet-mvc-4/_static/image51.png "l'avanzamento dell'installazione")</span><span class="sxs-lookup"><span data-stu-id="3bed2-644">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="3bed2-645">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="3bed2-645">*Installation progress*</span></span>
6. <span data-ttu-id="3bed2-646">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-646">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="3bed2-647">![Installazione completata](whats-new-in-aspnet-mvc-4/_static/image52.png "installazione completata")</span><span class="sxs-lookup"><span data-stu-id="3bed2-647">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="3bed2-648">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="3bed2-648">*Installation completed*</span></span>
7. <span data-ttu-id="3bed2-649">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="3bed2-649">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="3bed2-650">Attività 2: installare l'estensione simulatore di iPhone</span><span class="sxs-lookup"><span data-stu-id="3bed2-650">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="3bed2-651">Eseguire **WebMatrix** e aprire i siti Web esistenti o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="3bed2-651">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="3bed2-652">Fare clic su di **eseguire** pulsante il **Home** della barra multifunzione e selezionare **Aggiungi nuovo**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-652">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="3bed2-653">![Aggiunta di nuova estensione di WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "aggiunta nuova estensione di WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="3bed2-653">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="3bed2-654">*Aggiunta di nuova estensione di WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="3bed2-654">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="3bed2-655">Selezionare **iPhone simulatore** e fare clic su **installare**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-655">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="3bed2-656">![Esplorazione delle estensioni di WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "estensioni esplorazione WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="3bed2-656">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="3bed2-657">*Esplorazione delle estensioni di WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="3bed2-657">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="3bed2-658">I dettagli del pacchetto, fare clic su **installare** per continuare l'installazione dell'estensione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-658">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="3bed2-659">![iPhone estensione simulatore](whats-new-in-aspnet-mvc-4/_static/image55.png "estensione simulatore di iPhone")</span><span class="sxs-lookup"><span data-stu-id="3bed2-659">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="3bed2-660">*estensione simulatore di iPhone*</span><span class="sxs-lookup"><span data-stu-id="3bed2-660">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="3bed2-661">Leggere e accettare il contratto di licenza di estensione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-661">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="3bed2-662">![Estensione di WebMatrix EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "con l'estensione di WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="3bed2-662">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="3bed2-663">*Con l'estensione di WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="3bed2-663">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="3bed2-664">A questo punto, è possibile eseguire il sito Web da WebMatrix utilizzando l'opzione simulatore di iPhone.</span><span class="sxs-lookup"><span data-stu-id="3bed2-664">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="3bed2-665">![Eseguire utilizzando iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "eseguiti utilizzando iPhone")</span><span class="sxs-lookup"><span data-stu-id="3bed2-665">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="3bed2-666">*Eseguire utilizzando iPhone*</span><span class="sxs-lookup"><span data-stu-id="3bed2-666">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="3bed2-667">Attività 3: configurazione di Visual Studio 2012 per eseguire un simulatore di iPhone</span><span class="sxs-lookup"><span data-stu-id="3bed2-667">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="3bed2-668">Aprire **Visual Studio 2012** e aprire qualsiasi sito Web o creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-668">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="3bed2-669">Fare clic sulla freccia rivolta verso il basso sul pulsante Esegui e selezionare **Sfoglia con**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-669">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="3bed2-670">![Sfoglia con](whats-new-in-aspnet-mvc-4/_static/image58.png "Sfoglia con")</span><span class="sxs-lookup"><span data-stu-id="3bed2-670">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="3bed2-671">*Sfoglia con*</span><span class="sxs-lookup"><span data-stu-id="3bed2-671">*Browse with*</span></span>
3. <span data-ttu-id="3bed2-672">Nel &quot;Esplora con&quot; finestra di dialogo, fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-672">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="3bed2-673">Nel &quot;Aggiungi programma&quot; finestra di dialogo, utilizzare i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="3bed2-673">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

    - <span data-ttu-id="3bed2-674">**Programma**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(aggiornare di conseguenza il percorso)*</span><span class="sxs-lookup"><span data-stu-id="3bed2-674">**Program**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)*</span></span>
    - <span data-ttu-id="3bed2-675">**Argomenti**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="3bed2-675">**Arguments**: &quot;1&quot;</span></span>
    - <span data-ttu-id="3bed2-676">**Nome descrittivo**: simulatore di iPhone</span><span class="sxs-lookup"><span data-stu-id="3bed2-676">**Friendly name**: iPhone Simulator</span></span>

    <span data-ttu-id="3bed2-677">![Aggiungi programma](whats-new-in-aspnet-mvc-4/_static/image59.png "Aggiungi programma")</span><span class="sxs-lookup"><span data-stu-id="3bed2-677">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

    <span data-ttu-id="3bed2-678">*Aggiungi programma di esplorare con*</span><span class="sxs-lookup"><span data-stu-id="3bed2-678">*Add program to browse with*</span></span>
5. <span data-ttu-id="3bed2-679">Fare clic su **OK** e chiudere le finestre di dialogo.</span><span class="sxs-lookup"><span data-stu-id="3bed2-679">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="3bed2-680">Si è ora in grado di eseguire le applicazioni Web nel simulatore di iPhone da Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="3bed2-680">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="3bed2-681">![Sfoglia con iPhone simulatore](whats-new-in-aspnet-mvc-4/_static/image60.png "Sfoglia con simulatore di iPhone")</span><span class="sxs-lookup"><span data-stu-id="3bed2-681">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="3bed2-682">*Sfoglia con simulatore di iPhone*</span><span class="sxs-lookup"><span data-stu-id="3bed2-682">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="3bed2-683">Appendice d: la pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="3bed2-683">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="3bed2-684">Questa appendice mostra come creare un nuovo sito web dal portale di gestione di Windows Azure e pubblicare l'applicazione, ottenute seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3bed2-684">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="3bed2-685">Attività 1 - Creazione di un nuovo sito Web da Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3bed2-685">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="3bed2-686">Passare al [il portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-686">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-687">Con Windows Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico.</span><span class="sxs-lookup"><span data-stu-id="3bed2-687">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="3bed2-688">È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="3bed2-688">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="3bed2-689">![Accedere al portale Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "accedere al portale Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="3bed2-689">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="3bed2-690">*Accedere al portale di gestione di Azure*</span><span class="sxs-lookup"><span data-stu-id="3bed2-690">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="3bed2-691">Fare clic su **nuovo** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="3bed2-691">Click **New** on the command bar.</span></span>

    <span data-ttu-id="3bed2-692">![Creazione di un nuovo sito Web](whats-new-in-aspnet-mvc-4/_static/image62.png "creando un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="3bed2-692">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="3bed2-693">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="3bed2-693">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="3bed2-694">Fare clic su **calcolo** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-694">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="3bed2-695">Selezionare quindi **creazione rapida** opzione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-695">Then select **Quick Create** option.</span></span> <span data-ttu-id="3bed2-696">Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-696">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-697">Un sito Web di Windows Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="3bed2-697">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="3bed2-698">L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure dall'esterno al portale.</span><span class="sxs-lookup"><span data-stu-id="3bed2-698">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="3bed2-699">Non include i passaggi per configurare un database.</span><span class="sxs-lookup"><span data-stu-id="3bed2-699">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="3bed2-700">![Creazione di un nuovo sito Web utilizzando Creazione rapida](whats-new-in-aspnet-mvc-4/_static/image63.png "creando un nuovo sito Web utilizzando Creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="3bed2-700">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="3bed2-701">*Creazione di un nuovo sito Web utilizzando Creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="3bed2-701">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="3bed2-702">Attendere finché il nuovo **sito Web** viene creato.</span><span class="sxs-lookup"><span data-stu-id="3bed2-702">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="3bed2-703">Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna.</span><span class="sxs-lookup"><span data-stu-id="3bed2-703">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="3bed2-704">Verificare il funzionamento del nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="3bed2-704">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="3bed2-705">![Esplorazione per il nuovo sito web](whats-new-in-aspnet-mvc-4/_static/image64.png "esplorazione per il nuovo sito web")</span><span class="sxs-lookup"><span data-stu-id="3bed2-705">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="3bed2-706">*Esplorazione per il nuovo sito web*</span><span class="sxs-lookup"><span data-stu-id="3bed2-706">*Browsing to the new web site*</span></span>

    <span data-ttu-id="3bed2-707">![Sito Web in esecuzione](whats-new-in-aspnet-mvc-4/_static/image65.png "sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="3bed2-707">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="3bed2-708">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="3bed2-708">*Web site running*</span></span>
6. <span data-ttu-id="3bed2-709">Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-709">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="3bed2-710">![Aprire le pagine di gestione del sito web](whats-new-in-aspnet-mvc-4/_static/image66.png "apertura delle pagine di gestione del sito web")</span><span class="sxs-lookup"><span data-stu-id="3bed2-710">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="3bed2-711">*Aprire le pagine di gestione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="3bed2-711">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="3bed2-712">Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.</span><span class="sxs-lookup"><span data-stu-id="3bed2-712">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-713">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="3bed2-713">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="3bed2-714">Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-714">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="3bed2-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web per siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="3bed2-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="3bed2-716">![Profilo di pubblicazione del sito web di download](whats-new-in-aspnet-mvc-4/_static/image67.png "profilo di pubblicazione del sito web di download")</span><span class="sxs-lookup"><span data-stu-id="3bed2-716">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="3bed2-717">*Profilo di pubblicazione del sito Web di download*</span><span class="sxs-lookup"><span data-stu-id="3bed2-717">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="3bed2-718">Scaricare il file del profilo di pubblicazione in un percorso noto.</span><span class="sxs-lookup"><span data-stu-id="3bed2-718">Download the publish profile file to a known location.</span></span> <span data-ttu-id="3bed2-719">In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3bed2-719">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="3bed2-720">![Salvare il file del profilo di pubblicazione](whats-new-in-aspnet-mvc-4/_static/image68.png "il salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="3bed2-720">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="3bed2-721">*Salvare il file del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="3bed2-721">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="3bed2-722">Attività 2: configurare il Server di Database</span><span class="sxs-lookup"><span data-stu-id="3bed2-722">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="3bed2-723">Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL.</span><span class="sxs-lookup"><span data-stu-id="3bed2-723">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="3bed2-724">Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="3bed2-724">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="3bed2-725">È necessario un server di Database SQL per archiviare il database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3bed2-725">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="3bed2-726">È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-726">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="3bed2-727">Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="3bed2-727">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="3bed2-728">Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive.</span><span class="sxs-lookup"><span data-stu-id="3bed2-728">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="3bed2-729">Non creare il database, come verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="3bed2-729">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="3bed2-730">![Dashboard del Server Database SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "Dashboard del Server Database SQL")</span><span class="sxs-lookup"><span data-stu-id="3bed2-730">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="3bed2-731">*Dashboard del Server Database SQL*</span><span class="sxs-lookup"><span data-stu-id="3bed2-731">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="3bed2-732">Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-732">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="3bed2-733">A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) pulsante.</span><span class="sxs-lookup"><span data-stu-id="3bed2-733">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![Aggiunta indirizzo IP del Client](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="3bed2-735">*Aggiunta indirizzo IP del Client*</span><span class="sxs-lookup"><span data-stu-id="3bed2-735">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="3bed2-736">Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="3bed2-736">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confermare le modifiche](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="3bed2-738">*Confermare le modifiche*</span><span class="sxs-lookup"><span data-stu-id="3bed2-738">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="3bed2-739">Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="3bed2-739">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="3bed2-740">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3bed2-740">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="3bed2-741">Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-741">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="3bed2-742">![La pubblicazione dell'applicazione](whats-new-in-aspnet-mvc-4/_static/image73.png "la pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="3bed2-742">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="3bed2-743">*Pubblicazione del sito web*</span><span class="sxs-lookup"><span data-stu-id="3bed2-743">*Publishing the web site*</span></span>
2. <span data-ttu-id="3bed2-744">Importare il profilo di pubblicazione che è stato salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="3bed2-744">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="3bed2-745">![L'importazione del profilo di pubblicazione](whats-new-in-aspnet-mvc-4/_static/image74.png "l'importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="3bed2-745">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="3bed2-746">*L'importazione di profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="3bed2-746">*Importing publish profile*</span></span>
3. <span data-ttu-id="3bed2-747">Fare clic su **convalidare la connessione**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-747">Click **Validate Connection**.</span></span> <span data-ttu-id="3bed2-748">Una volta completata la convalida, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-748">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3bed2-749">Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.</span><span class="sxs-lookup"><span data-stu-id="3bed2-749">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="3bed2-750">![Convalida della connessione](whats-new-in-aspnet-mvc-4/_static/image75.png "convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="3bed2-750">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="3bed2-751">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="3bed2-751">*Validating connection*</span></span>
4. <span data-ttu-id="3bed2-752">Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="3bed2-752">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="3bed2-753">![Configurazione della distribuzione Web](whats-new-in-aspnet-mvc-4/_static/image76.png "configurazione della distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="3bed2-753">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="3bed2-754">*Configurazione della distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="3bed2-754">*Web deploy configuration*</span></span>
5. <span data-ttu-id="3bed2-755">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="3bed2-755">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="3bed2-756">Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.</span><span class="sxs-lookup"><span data-stu-id="3bed2-756">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="3bed2-757">In **nome utente** digitare il nome di accesso di amministratore di server.</span><span class="sxs-lookup"><span data-stu-id="3bed2-757">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="3bed2-758">In **Password** digitare la password dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="3bed2-758">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="3bed2-759">Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="3bed2-759">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="3bed2-760">![Configurazione di stringa di connessione di destinazione](whats-new-in-aspnet-mvc-4/_static/image77.png "configurazione stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="3bed2-760">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="3bed2-761">*Configurazione di stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="3bed2-761">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="3bed2-762">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-762">Then click **OK**.</span></span> <span data-ttu-id="3bed2-763">Quando viene richiesto di creare il database fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-763">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="3bed2-764">![Creazione del database](whats-new-in-aspnet-mvc-4/_static/image78.png "creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="3bed2-764">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="3bed2-765">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="3bed2-765">*Creating the database*</span></span>
7. <span data-ttu-id="3bed2-766">La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito.</span><span class="sxs-lookup"><span data-stu-id="3bed2-766">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="3bed2-767">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-767">Then click **Next**.</span></span>

    <span data-ttu-id="3bed2-768">![Stringa di connessione che punta al Database SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "stringa di connessione che punta al Database SQL")</span><span class="sxs-lookup"><span data-stu-id="3bed2-768">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="3bed2-769">*Stringa di connessione che punta al Database SQL*</span><span class="sxs-lookup"><span data-stu-id="3bed2-769">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="3bed2-770">Nel **anteprima** pagina, fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="3bed2-770">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="3bed2-771">![Pubblicare l'applicazione web](whats-new-in-aspnet-mvc-4/_static/image80.png "pubblicare l'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="3bed2-771">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="3bed2-772">*Pubblicare l'applicazione web*</span><span class="sxs-lookup"><span data-stu-id="3bed2-772">*Publishing the web application*</span></span>
9. <span data-ttu-id="3bed2-773">Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="3bed2-773">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="3bed2-774">![Applicazione pubblicata in Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "applicazione pubblicata in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="3bed2-774">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="3bed2-775">*Applicazione pubblicata in Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="3bed2-775">*Application published to Windows Azure*</span></span>
