---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: shirhatti
description: Informazioni sul supporto del debug di applicazioni ASP.NET Core durante l'esecuzione dietro IIS in Windows Server.
keywords: ASP.NET Core,internet information services,iis,windows server,modulo asp.net core,debug
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="9e678-104">Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e678-104">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="9e678-105">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="9e678-105">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="9e678-106">L'articolo descrive il supporto del debug di applicazioni ASP.NET Core in [Visual Studio](https://www.visualstudio.com/vs/) durante l'esecuzione dietro IIS in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="9e678-106">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="9e678-107">Questo argomento illustra nel dettaglio l'abilitazione della funzionalità e la configurazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="9e678-107">This topic walks you through enabling this feature and setting up your project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e678-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="9e678-108">Prerequisites</span></span>

* <span data-ttu-id="9e678-109">Visual Studio 2017 15.3 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="9e678-109">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="9e678-110">ASP.NET e il carico di lavoro Sviluppo Web *o* il carico di lavoro Sviluppo multipiattaforma .NET Core</span><span class="sxs-lookup"><span data-stu-id="9e678-110">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="9e678-111">Abilitare IIS</span><span class="sxs-lookup"><span data-stu-id="9e678-111">Enable IIS</span></span>

<span data-ttu-id="9e678-112">Abilitare IIS nel sistema.</span><span class="sxs-lookup"><span data-stu-id="9e678-112">Enable IIS on your system.</span></span> <span data-ttu-id="9e678-113">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="9e678-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="9e678-114">Selezionare la casella di controllo **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="9e678-114">Select the **Internet Information Services** checkbox.</span></span>

![Funzionalità di Windows con la casella di controllo Internet Information Services selezionata come quadrato nero (non con un segno di spunta) ad indicare che alcune funzionalità di IIS sono abilitate](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="9e678-116">Se l'installazione di IIS richiede un riavvio, riavviare il sistema.</span><span class="sxs-lookup"><span data-stu-id="9e678-116">If your IIS installation requires a reboot, reboot your system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="9e678-117">Abilitare il supporto IIS in fase di sviluppo</span><span class="sxs-lookup"><span data-stu-id="9e678-117">Enable development-time IIS support</span></span>

<span data-ttu-id="9e678-118">Dopo aver installato IIS avviare il programma di installazione di Visual Studio per modificare l'installazione esistente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9e678-118">Once you've installed IIS, launch the Visual Studio installer to modify your existing Visual Studio installation.</span></span> <span data-ttu-id="9e678-119">Nel programma di installazione selezionare il componente **Supporto IIS in fase di sviluppo **.</span><span class="sxs-lookup"><span data-stu-id="9e678-119">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="9e678-120">Il componente è elencato come componente facoltativo nel pannello **Riepilogo** del carico di lavoro **Sviluppo ASP.NET e Web**.</span><span class="sxs-lookup"><span data-stu-id="9e678-120">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="9e678-121">Viene installato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), un modulo IIS nativo necessario per l'esecuzione delle applicazioni ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9e678-121">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="9e678-125">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="9e678-125">Configure the project</span></span>

<span data-ttu-id="9e678-126">Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="9e678-126">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="9e678-127">In **Esplora soluzioni** di Visual Studio fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="9e678-127">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="9e678-128">Selezionare la scheda **Debug**. Selezionare **IIS** nell'elenco a discesa **Avvia**.</span><span class="sxs-lookup"><span data-stu-id="9e678-128">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="9e678-129">Verificare che la funzionalità **Avvia browser** sia abilitata con l'URL corretto.</span><span class="sxs-lookup"><span data-stu-id="9e678-129">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Finestra delle proprietà del progetto con la scheda Debug selezionata.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="9e678-134">In alternativa è possibile aggiungere manualmente un profilo di avvio per il file [launchSettings.json](http://json.schemastore.org/launchsettings) nell'app:</span><span class="sxs-lookup"><span data-stu-id="9e678-134">Alternatively, you can manually add a launch profile to your [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

<span data-ttu-id="9e678-135">Se non si sta eseguendo Visual Studio come amministratore è possibile che venga richiesto di riavviare il programma.</span><span class="sxs-lookup"><span data-stu-id="9e678-135">You may be prompted to restart Visual Studio if you weren't running as an administrator.</span></span> <span data-ttu-id="9e678-136">In tal caso riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9e678-136">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="9e678-137">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="9e678-137">Congratulations!</span></span> <span data-ttu-id="9e678-138">A questo punto il progetto è configurato per il supporto IIS in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="9e678-138">At this point, your project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9e678-139">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9e678-139">Additional resources</span></span>

* [<span data-ttu-id="9e678-140">Host ASP.NET Core in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="9e678-140">Host ASP.NET Core on Windows with IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="9e678-141">Introduzione al modulo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e678-141">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="9e678-142">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e678-142">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
