---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: shirhatti
description: Individuare il supporto per il debug di applicazioni ASP.NET Core durante l'esecuzione di base IIS in Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="47418-103">Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47418-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="47418-104">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="47418-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="47418-105">Questo articolo descrive [Visual Studio](https://www.visualstudio.com/vs/) supporto per il debug di App di ASP.NET Core in esecuzione dietro IIS su Windows Server.</span><span class="sxs-lookup"><span data-stu-id="47418-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="47418-106">Questo argomento vengono illustrati l'abilitazione di questa funzionalità e l'impostazione di un progetto.</span><span class="sxs-lookup"><span data-stu-id="47418-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="47418-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="47418-107">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="47418-108">Abilitare IIS</span><span class="sxs-lookup"><span data-stu-id="47418-108">Enable IIS</span></span>

<span data-ttu-id="47418-109">Abilitare IIS.</span><span class="sxs-lookup"><span data-stu-id="47418-109">Enable IIS.</span></span> <span data-ttu-id="47418-110">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="47418-110">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="47418-111">Selezionare la casella di controllo **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="47418-111">Select the **Internet Information Services** checkbox.</span></span>

![Funzionalità di Windows con la casella di controllo Internet Information Services selezionata come quadrato nero (non con un segno di spunta) ad indicare che alcune funzionalità di IIS sono abilitate](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="47418-113">Se l'installazione di IIS richiede un riavvio, riavviare il sistema.</span><span class="sxs-lookup"><span data-stu-id="47418-113">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="47418-114">Abilitare il supporto IIS in fase di sviluppo</span><span class="sxs-lookup"><span data-stu-id="47418-114">Enable development-time IIS support</span></span>

<span data-ttu-id="47418-115">Avviare il programma di installazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="47418-115">Launch the Visual Studio installer.</span></span> <span data-ttu-id="47418-116">Selezionare il **fase di sviluppo IIS supporta** componente.</span><span class="sxs-lookup"><span data-stu-id="47418-116">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="47418-117">Il componente è elencato come facoltativi nel **riepilogo** pannello per il **sviluppo web ASP.NET e** carico di lavoro.</span><span class="sxs-lookup"><span data-stu-id="47418-117">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="47418-118">Questo modo vengono installati il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module), ovvero un modulo nativo di IIS necessario per eseguire app di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="47418-118">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="47418-122">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="47418-122">Configure the project</span></span>

<span data-ttu-id="47418-123">Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="47418-123">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="47418-124">In **Esplora soluzioni** di Visual Studio fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="47418-124">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="47418-125">Selezionare la scheda **Debug**. Selezionare **IIS** nell'elenco a discesa **Avvia**.</span><span class="sxs-lookup"><span data-stu-id="47418-125">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="47418-126">Verificare che la funzionalità **Avvia browser** sia abilitata con l'URL corretto.</span><span class="sxs-lookup"><span data-stu-id="47418-126">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Finestra delle proprietà del progetto con la scheda Debug selezionata.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="47418-131">In alternativa, aggiungere manualmente un profilo di avvio per il [launchSettings.json](http://json.schemastore.org/launchsettings) file nell'app:</span><span class="sxs-lookup"><span data-stu-id="47418-131">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="47418-132">Visual Studio potrebbe richiedere un riavvio, se non è in esecuzione come amministratore.</span><span class="sxs-lookup"><span data-stu-id="47418-132">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="47418-133">In tal caso riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="47418-133">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="47418-134">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="47418-134">Additional resources</span></span>

* [<span data-ttu-id="47418-135">Host ASP.NET Core in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="47418-135">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="47418-136">Introduzione al modulo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47418-136">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="47418-137">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47418-137">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
