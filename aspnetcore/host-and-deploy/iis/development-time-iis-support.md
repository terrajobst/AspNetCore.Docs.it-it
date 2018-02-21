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
ms.openlocfilehash: a8bdf4c0c0399c62666e6e61e70c0298a42c2c12
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="fc0a3-103">Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc0a3-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="fc0a3-104">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="fc0a3-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="fc0a3-105">Questo articolo descrive [Visual Studio](https://www.visualstudio.com/vs/) supporto per il debug di App di ASP.NET Core in esecuzione dietro IIS su Windows Server.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="fc0a3-106">Questo argomento vengono illustrati l'abilitazione di questa funzionalità e l'impostazione di un progetto.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fc0a3-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="fc0a3-107">Prerequisites</span></span>

* <span data-ttu-id="fc0a3-108">Visual Studio 2017 15.3 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="fc0a3-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="fc0a3-109">ASP.NET e il carico di lavoro Sviluppo Web *o* il carico di lavoro Sviluppo multipiattaforma .NET Core</span><span class="sxs-lookup"><span data-stu-id="fc0a3-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="fc0a3-110">Abilitare IIS</span><span class="sxs-lookup"><span data-stu-id="fc0a3-110">Enable IIS</span></span>

<span data-ttu-id="fc0a3-111">Abilitare IIS.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-111">Enable IIS.</span></span> <span data-ttu-id="fc0a3-112">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="fc0a3-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="fc0a3-113">Selezionare la casella di controllo **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-113">Select the **Internet Information Services** checkbox.</span></span>

![Funzionalità di Windows con la casella di controllo Internet Information Services selezionata come quadrato nero (non con un segno di spunta) ad indicare che alcune funzionalità di IIS sono abilitate](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="fc0a3-115">Se l'installazione di IIS richiede un riavvio, riavviare il sistema.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-115">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="fc0a3-116">Abilitare il supporto IIS in fase di sviluppo</span><span class="sxs-lookup"><span data-stu-id="fc0a3-116">Enable development-time IIS support</span></span>

<span data-ttu-id="fc0a3-117">Avviare il programma di installazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-117">Launch the Visual Studio installer.</span></span> <span data-ttu-id="fc0a3-118">Selezionare il **fase di sviluppo IIS supporta** componente.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-118">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="fc0a3-119">Il componente è elencato come facoltativi nel **riepilogo** pannello per il **sviluppo web ASP.NET e** carico di lavoro.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-119">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="fc0a3-120">Questo modo vengono installati il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module), ovvero un modulo nativo di IIS necessario per eseguire app di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="fc0a3-124">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="fc0a3-124">Configure the project</span></span>

<span data-ttu-id="fc0a3-125">Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="fc0a3-126">In **Esplora soluzioni** di Visual Studio fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="fc0a3-127">Selezionare la scheda **Debug**. Selezionare **IIS** nell'elenco a discesa **Avvia**.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="fc0a3-128">Verificare che la funzionalità **Avvia browser** sia abilitata con l'URL corretto.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Finestra delle proprietà del progetto con la scheda Debug selezionata.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="fc0a3-133">In alternativa, aggiungere manualmente un profilo di avvio per il [launchSettings.json](http://json.schemastore.org/launchsettings) file nell'app:</span><span class="sxs-lookup"><span data-stu-id="fc0a3-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="fc0a3-134">Visual Studio potrebbe richiedere un riavvio, se non è in esecuzione come amministratore.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="fc0a3-135">In tal caso riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc0a3-135">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fc0a3-136">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fc0a3-136">Additional resources</span></span>

* [<span data-ttu-id="fc0a3-137">Host ASP.NET Core in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="fc0a3-137">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="fc0a3-138">Introduzione al modulo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc0a3-138">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="fc0a3-139">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc0a3-139">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
