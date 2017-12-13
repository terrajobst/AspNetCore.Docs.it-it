---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: L'aggiornamento di un'applicazione MVC ASP.NET 1.0 per ASP.NET MVC 2 | Documenti Microsoft
author: rick-anderson
description: "Questo documento descritta come eseguire l'aggiornamento di una procedura guidata e manualmente un'applicazione ASP.NET MVC 1.0 per ASP.NET MVC 2. Questo documento è disponibile anche per d..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="d45e5-104">L'aggiornamento di un'applicazione MVC ASP.NET 1.0 per ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="d45e5-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="d45e5-105">Questo documento descritta come eseguire l'aggiornamento di una procedura guidata e manualmente un'applicazione ASP.NET MVC 1.0 per ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="d45e5-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="d45e5-106">Questo documento è disponibile anche per [scaricare](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="d45e5-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="d45e5-107">Introduzione</span><span class="sxs-lookup"><span data-stu-id="d45e5-107">Introduction</span></span>

<span data-ttu-id="d45e5-108">ASP.NET MVC 2 può essere installato in modo affiancato con ASP.NET MVC 1.0 nello stesso server.</span><span class="sxs-lookup"><span data-stu-id="d45e5-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="d45e5-109">Questo offre flessibilità agli sviluppatori dell'applicazione scegliere quando eseguire l'aggiornamento di un'applicazione MVC ASP.NET 1.0 per ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="d45e5-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="d45e5-110">Visual Studio 2010 include una procedura guidata che gli aggiornamenti di progetti ASP.NET MVC 1.0 esistenti compilati con Visual Studio 2008 per ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="d45e5-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="d45e5-111">L'aggiornamento guidato è stato avviato, aprire un progetto ASP.NET MVC 1.0 in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="d45e5-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="d45e5-112">Aggiornamento guidato per ASP.NET MVC 1.0 in Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="d45e5-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="d45e5-113">Per aggiornare un'applicazione MVC ASP.NET 1.0 per ASP.NET MVC 2 in Visual Studio 2008 SP1, utilizzare l'applicazione MvcAppConverter (non supportato).</span><span class="sxs-lookup"><span data-stu-id="d45e5-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="d45e5-114">È possibile scaricare l'applicazione dall'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="d45e5-114">You can download this application from the following URL:</span></span>

[<span data-ttu-id="d45e5-115">https://go.microsoft.com/fwlink/?LinkId=185351</span><span class="sxs-lookup"><span data-stu-id="d45e5-115">https://go.microsoft.com/fwlink/?LinkID=185351</span></span>](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="d45e5-116">Aggiornamento manuale di un progetto MVC ASP.NET 1.0</span><span class="sxs-lookup"><span data-stu-id="d45e5-116">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="d45e5-117">Per aggiornare manualmente un'applicazione MVC ASP.NET 1.0 alla versione 2, seguire questi passaggi:</span><span class="sxs-lookup"><span data-stu-id="d45e5-117">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="d45e5-118">Eseguire un backup del progetto esistente.</span><span class="sxs-lookup"><span data-stu-id="d45e5-118">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="d45e5-119">In un editor di testo aprire il file di progetto (file con estensione csproj o vbproj) e trovare l'elemento ProjectTypeGuid.</span><span class="sxs-lookup"><span data-stu-id="d45e5-119">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="d45e5-120">Se il valore di tale elemento, sostituire il GUID {603c0e0b-db56-11dc-be95-000d561079b0} con {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="d45e5-120">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="d45e5-121">Al termine, il valore di tale elemento deve essere come segue:</span><span class="sxs-lookup"><span data-stu-id="d45e5-121">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="d45e5-122">Nella cartella radice dell'applicazione Web, modificare il file Web. config.</span><span class="sxs-lookup"><span data-stu-id="d45e5-122">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="d45e5-123">Ricerca per System, versione = 1.0.0.0 e sostituire tutte le istanze con System, versione = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="d45e5-123">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="d45e5-124">Ripetere il passaggio precedente per il file Web. config che si trova nella cartella Views.</span><span class="sxs-lookup"><span data-stu-id="d45e5-124">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="d45e5-125">Aprire il progetto con Visual Studio e in **Esplora**, espandere il **riferimenti** nodo.</span><span class="sxs-lookup"><span data-stu-id="d45e5-125">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="d45e5-126">Eliminare il riferimento a System (che fa riferimento all'assembly versione 1.0).</span><span class="sxs-lookup"><span data-stu-id="d45e5-126">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="d45e5-127">Aggiungere un riferimento a System (v2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="d45e5-127">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="d45e5-128">Aggiungere l'elemento bindingRedirect seguente al file Web. config nella radice dell'applicazione nella sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d45e5-128">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="d45e5-129">Creare una nuova applicazione ASP.NET MVC 2 vuota.</span><span class="sxs-lookup"><span data-stu-id="d45e5-129">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="d45e5-130">Copiare i file dalla cartella Scripts della nuova applicazione nella cartella degli script dell'applicazione esistente.</span><span class="sxs-lookup"><span data-stu-id="d45e5-130">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="d45e5-131">Aggiornare il € applicazioni esistenti™ file CSS s con le definizioni di stile CSS nel file Site.</span><span class="sxs-lookup"><span data-stu-id="d45e5-131">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="d45e5-132">Compilare l'applicazione ed eseguirlo.</span><span class="sxs-lookup"><span data-stu-id="d45e5-132">Compile the application and run it.</span></span> <span data-ttu-id="d45e5-133">Se si verificano errori, vedere la sezione di modifiche di rilievo del [novità di ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) pagina.</span><span class="sxs-lookup"><span data-stu-id="d45e5-133">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
