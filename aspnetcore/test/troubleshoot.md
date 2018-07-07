---
title: Risolvere i problemi di progetti ASP.NET Core
author: Rick-Anderson
description: Riconoscere e risolvere i problemi di avvisi ed errori con i progetti ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: b4d90f541a4cda2d41b49101b7ea39af87a4dcb4
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889012"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="1a472-103">Risolvere i problemi di progetti ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a472-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="1a472-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1a472-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1a472-105">I collegamenti seguenti forniscono indicazioni sulla risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="1a472-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="1a472-106">Risolvere i problemi di ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="1a472-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="1a472-107">Risolvere i problemi di ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="1a472-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="1a472-108">Errori comuni di Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a472-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="1a472-109">NDC Conference (Londra, 2018): La diagnosi dei problemi nelle applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a472-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="1a472-110">Blog su ASP.NET: Risoluzione dei problemi di prestazioni di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a472-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="1a472-111">Avvisi di .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="1a472-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="1a472-112">Entrambe le versioni a 64 bit di .NET Core SDK e a 32 bit installate</span><span class="sxs-lookup"><span data-stu-id="1a472-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="1a472-113">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="1a472-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="1a472-114">Vengono installate sia 32 e 64 versioni di bit di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="1a472-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="1a472-115">Solo i modelli dalle versioni a 64 bit installate in ' c:\\Program Files\\dotnet\\sdk\\' verranno visualizzati.</span><span class="sxs-lookup"><span data-stu-id="1a472-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="1a472-117">Questo avviso viene visualizzato quando (x86) 32 bit sia versioni a 64 bit (x64) del [.NET Core SDK](https://www.microsoft.com/net/download/all) siano installati.</span><span class="sxs-lookup"><span data-stu-id="1a472-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="1a472-118">Cause più comuni che è possibile installare entrambe le versioni includono:</span><span class="sxs-lookup"><span data-stu-id="1a472-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="1a472-119">Si originariamente scaricato il programma di installazione di .NET Core SDK Usa un computer a 32 bit, ma quindi copiata quest'ultimo e viene installato in un computer a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="1a472-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="1a472-120">32 bit .NET Core SDK è stato installato da un'altra applicazione.</span><span class="sxs-lookup"><span data-stu-id="1a472-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="1a472-121">La versione errata è stata scaricata e installata.</span><span class="sxs-lookup"><span data-stu-id="1a472-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="1a472-122">Disinstallare il SDK di .NET Core a 32 bit per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="1a472-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="1a472-123">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="1a472-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="1a472-124">Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="1a472-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="1a472-125">.NET Core SDK è installato in più posizioni</span><span class="sxs-lookup"><span data-stu-id="1a472-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="1a472-126">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="1a472-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="1a472-127">.NET Core SDK è installato in più posizioni.</span><span class="sxs-lookup"><span data-stu-id="1a472-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="1a472-128">Solo i modelli dagli SDK installati in ' c:\\Program Files\\dotnet\\sdk\\' verranno visualizzati.</span><span class="sxs-lookup"><span data-stu-id="1a472-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="1a472-130">Viene visualizzato questo messaggio quando si dispone di almeno un'installazione di .NET Core SDK in una directory fuori *c:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="1a472-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="1a472-131">In genere ciò si verifica quando .NET Core SDK è stato distribuito in un computer tramite Copia/Incolla anziché il programma di installazione MSI.</span><span class="sxs-lookup"><span data-stu-id="1a472-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="1a472-132">Disinstallare il SDK di .NET Core a 32 bit per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="1a472-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="1a472-133">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="1a472-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="1a472-134">Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="1a472-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="1a472-135">Nessun SDK per .NET Core sono stati rilevati</span><span class="sxs-lookup"><span data-stu-id="1a472-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="1a472-136">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="1a472-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="1a472-137">Nessun SDK per .NET Core sono stati rilevati, assicurarsi che siano inclusi nella variabile di ambiente 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="1a472-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="1a472-139">Questo avviso viene visualizzato quando la variabile di ambiente `PATH` non punta a qualsiasi SDK per .NET Core nel computer.</span><span class="sxs-lookup"><span data-stu-id="1a472-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="1a472-140">Per risolvere questo problema:</span><span class="sxs-lookup"><span data-stu-id="1a472-140">To resolve this problem:</span></span>

* <span data-ttu-id="1a472-141">Installare o verificare che sia installato .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="1a472-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="1a472-142">Verificare che il `PATH` variabile di ambiente punta alla posizione in cui è installato il SDK.</span><span class="sxs-lookup"><span data-stu-id="1a472-142">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="1a472-143">Il programma di installazione è imposta in genere il `PATH`.</span><span class="sxs-lookup"><span data-stu-id="1a472-143">The installer normally sets the `PATH`.</span></span>