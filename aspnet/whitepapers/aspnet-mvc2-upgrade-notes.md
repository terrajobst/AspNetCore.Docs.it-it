---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: L'aggiornamento di un'applicazione ASP.NET MVC 1.0 ad ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive entrambi come aggiornare una procedura guidata e manualmente un'applicazione ASP.NET MVC 1.0 per ASP.NET MVC 2. Questo documento è disponibile anche per d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 3de69df7e80037de35c2609232f4574bc9d03c80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834453"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="2255c-104">L'aggiornamento di un'applicazione ASP.NET MVC 1.0 ad ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="2255c-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="2255c-105">Questo documento descrive entrambi come aggiornare una procedura guidata e manualmente un'applicazione ASP.NET MVC 1.0 per ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="2255c-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="2255c-106">Questo documento è disponibile anche per [scaricare](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="2255c-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="2255c-107">Introduzione</span><span class="sxs-lookup"><span data-stu-id="2255c-107">Introduction</span></span>

<span data-ttu-id="2255c-108">ASP.NET MVC 2 può essere installato affiancato con ASP.NET MVC 1.0 nello stesso server.</span><span class="sxs-lookup"><span data-stu-id="2255c-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="2255c-109">Questo offre applicazioni sviluppatori flessibilità nella scelta del momento aggiornare un'applicazione ASP.NET MVC 1.0 ad ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="2255c-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="2255c-110">Visual Studio 2010 include una procedura guidata che gli aggiornamenti di progetti ASP.NET MVC 1.0 esistenti compilati con Visual Studio 2008 per ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="2255c-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="2255c-111">Viene avviato aggiornamento guidato, aprire un progetto ASP.NET MVC 1.0 in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2255c-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="2255c-112">Aggiornamento guidato per ASP.NET MVC 1.0 in Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="2255c-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="2255c-113">Per aggiornare un'applicazione ASP.NET MVC 1.0 ad ASP.NET MVC 2 in Visual Studio 2008 SP1, usare l'applicazione MvcAppConverter (non supportato).</span><span class="sxs-lookup"><span data-stu-id="2255c-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="2255c-114">È possibile scaricare l'applicazione dall'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="2255c-114">You can download this application from the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="2255c-115">Aggiornare manualmente un progetto ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="2255c-115">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="2255c-116">Per aggiornare manualmente un'applicazione ASP.NET MVC 1.0 esistente alla versione 2, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="2255c-116">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="2255c-117">Eseguire il backup del progetto esistente.</span><span class="sxs-lookup"><span data-stu-id="2255c-117">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="2255c-118">In un editor di testo aprire il file di progetto (file con estensione csproj o vbproj) e trovare l'elemento Generators\projecttypeguid.</span><span class="sxs-lookup"><span data-stu-id="2255c-118">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="2255c-119">Il valore di quell'elemento, sostituire il GUID {603c0e0b-db56-11dc-be95-000d561079b0} con {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="2255c-119">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="2255c-120">Al termine, il valore di tale elemento deve essere come segue:</span><span class="sxs-lookup"><span data-stu-id="2255c-120">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="2255c-121">Nella cartella radice dell'applicazione Web, modificare il file Web. config.</span><span class="sxs-lookup"><span data-stu-id="2255c-121">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="2255c-122">Cercare System, versione = 1.0.0.0 e sostituire tutte le istanze con System, versione = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="2255c-122">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="2255c-123">Ripetere il passaggio precedente per il file Web. config che si trova nella cartella Views.</span><span class="sxs-lookup"><span data-stu-id="2255c-123">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="2255c-124">Aprire il progetto con Visual Studio e nella **Esplora soluzioni**, espandere il **riferimenti** nodo.</span><span class="sxs-lookup"><span data-stu-id="2255c-124">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="2255c-125">Eliminare il riferimento a System (che fa riferimento all'assembly versione 1.0).</span><span class="sxs-lookup"><span data-stu-id="2255c-125">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="2255c-126">Aggiungere un riferimento a System (v2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="2255c-126">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="2255c-127">Aggiungere l'elemento bindingRedirect seguente al file Web. config nella radice dell'applicazione sotto la sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="2255c-127">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="2255c-128">Creare una nuova applicazione ASP.NET MVC 2 vuota.</span><span class="sxs-lookup"><span data-stu-id="2255c-128">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="2255c-129">Copiare i file dalla cartella degli script della nuova applicazione nella cartella degli script dell'applicazione esistente.</span><span class="sxs-lookup"><span data-stu-id="2255c-129">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="2255c-130">Aggiornare il € dell'applicazione esistente™ file CSS s con le definizioni di stile CSS nel file Site CSS.</span><span class="sxs-lookup"><span data-stu-id="2255c-130">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="2255c-131">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="2255c-131">Compile the application and run it.</span></span> <span data-ttu-id="2255c-132">Se si verificano errori, fare riferimento alla sezione delle modifiche di rilievo nel [What ' s New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) pagina.</span><span class="sxs-lookup"><span data-stu-id="2255c-132">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
