---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: shirhatti
description: Informazioni sul supporto del debug di applicazioni ASP.NET Core durante l'esecuzione dietro IIS in Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a5f727dd21ac0c6702691df2215c42f4adc0ec27
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="774dc-103">Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="774dc-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="774dc-104">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="774dc-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="774dc-105">L'articolo descrive il supporto del debug di applicazioni ASP.NET Core in [Visual Studio](https://www.visualstudio.com/vs/) durante l'esecuzione dietro IIS in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="774dc-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="774dc-106">Questo argomento vengono illustrati l'abilitazione di questa funzionalità e l'impostazione di un progetto.</span><span class="sxs-lookup"><span data-stu-id="774dc-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="774dc-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="774dc-107">Prerequisites</span></span>

* <span data-ttu-id="774dc-108">Visual Studio 2017 15.3 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="774dc-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="774dc-109">ASP.NET e il carico di lavoro Sviluppo Web *o* il carico di lavoro Sviluppo multipiattaforma .NET Core</span><span class="sxs-lookup"><span data-stu-id="774dc-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="774dc-110">Abilitare IIS</span><span class="sxs-lookup"><span data-stu-id="774dc-110">Enable IIS</span></span>

<span data-ttu-id="774dc-111">Abilitare IIS.</span><span class="sxs-lookup"><span data-stu-id="774dc-111">Enable IIS.</span></span> <span data-ttu-id="774dc-112">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="774dc-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="774dc-113">Selezionare la casella di controllo **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="774dc-113">Select the **Internet Information Services** checkbox.</span></span>

![Funzionalità di Windows con la casella di controllo Internet Information Services selezionata come quadrato nero (non con un segno di spunta) ad indicare che alcune funzionalità di IIS sono abilitate](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="774dc-115">Se l'installazione di IIS richiede un riavvio, riavviare il sistema.</span><span class="sxs-lookup"><span data-stu-id="774dc-115">If the IIS installation requires a reboot, reboot the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="774dc-116">Abilitare il supporto IIS in fase di sviluppo</span><span class="sxs-lookup"><span data-stu-id="774dc-116">Enable development-time IIS support</span></span>

<span data-ttu-id="774dc-117">Dopo aver installato IIS, è possibile avviare il programma di installazione di Visual Studio per modificare l'installazione esistente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="774dc-117">Once IIS is installed, launch the Visual Studio installer to modify the existing Visual Studio installation.</span></span> <span data-ttu-id="774dc-118">Nel programma di installazione selezionare il componente **Supporto IIS in fase di sviluppo** .</span><span class="sxs-lookup"><span data-stu-id="774dc-118">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="774dc-119">Il componente è elencato come componente facoltativo nel pannello **Riepilogo** del carico di lavoro **Sviluppo ASP.NET e Web**.</span><span class="sxs-lookup"><span data-stu-id="774dc-119">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="774dc-120">Viene installato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), un modulo IIS nativo necessario per l'esecuzione delle applicazioni ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="774dc-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="774dc-124">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="774dc-124">Configure the project</span></span>

<span data-ttu-id="774dc-125">Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="774dc-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="774dc-126">In **Esplora soluzioni** di Visual Studio fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="774dc-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="774dc-127">Selezionare la scheda **Debug**. Selezionare **IIS** nell'elenco a discesa **Avvia**.</span><span class="sxs-lookup"><span data-stu-id="774dc-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="774dc-128">Verificare che la funzionalità **Avvia browser** sia abilitata con l'URL corretto.</span><span class="sxs-lookup"><span data-stu-id="774dc-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Finestra delle proprietà del progetto con la scheda Debug selezionata.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="774dc-133">In alternativa, aggiungere manualmente un profilo di avvio per il [launchSettings.json](http://json.schemastore.org/launchsettings) file nell'app:</span><span class="sxs-lookup"><span data-stu-id="774dc-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="774dc-134">Visual Studio potrebbe richiedere un riavvio, se non è in esecuzione come amministratore.</span><span class="sxs-lookup"><span data-stu-id="774dc-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="774dc-135">In tal caso riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="774dc-135">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="774dc-136">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="774dc-136">Congratulations!</span></span> <span data-ttu-id="774dc-137">A questo punto, il progetto è configurato per il supporto in fase di sviluppo IIS.</span><span class="sxs-lookup"><span data-stu-id="774dc-137">At this point, the project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="774dc-138">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="774dc-138">Additional resources</span></span>

* [<span data-ttu-id="774dc-139">Host ASP.NET Core in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="774dc-139">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="774dc-140">Introduzione al modulo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="774dc-140">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="774dc-141">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="774dc-141">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
