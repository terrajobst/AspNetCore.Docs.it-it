---
title: Risoluzione dei problemi per ASP.NET Core
author: Rick-Anderson
description: Comprendere e risolvere i problemi di avvisi ed errori con i progetti ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: test/troubleshoot
ms.openlocfilehash: 64a353a9bdf0753c63f676f9d07f42ba45acdcab
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "35217651"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="aea90-103">Risolvere i progetti ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aea90-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="aea90-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aea90-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aea90-105">I collegamenti seguenti forniscono informazioni aggiuntive sulla risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="aea90-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="aea90-106">Risolvere i problemi di ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="aea90-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="aea90-107">Risolvere i problemi di ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="aea90-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="aea90-108">Errori comuni di Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aea90-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* <span data-ttu-id="aea90-109">[YouTube: Diagnosing issues in ASP.NET Core Applications](https://www.youtube.com/watch?v=RYI0DHoIVaA) (Problemi di diagnosi nelle applicazioni ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="aea90-109">[YouTube: Diagnosing issues in ASP.NET Core Applications](https://www.youtube.com/watch?v=RYI0DHoIVaA)</span></span>

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="aea90-110">Avvisi di .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="aea90-110">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="aea90-111">Entrambe le versioni a 64 bit del SDK .NET Core e a 32 bit installate</span><span class="sxs-lookup"><span data-stu-id="aea90-111">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="aea90-112">Nel **nuovo progetto** dialogo ASP.NET Core, si potrebbe notare l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="aea90-112">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="aea90-114">Questo avviso viene visualizzato quando (x86) 32 bit sia versioni a 64 bit (x64) di [.NET Core SDK](https://www.microsoft.com/net/download/all) installate.</span><span class="sxs-lookup"><span data-stu-id="aea90-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="aea90-115">È possono installare entrambe le versioni di cause comuni includono:</span><span class="sxs-lookup"><span data-stu-id="aea90-115">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="aea90-116">Originariamente scaricato il programma di installazione di .NET Core SDK utilizzando un computer a 32 bit, ma quindi copiato quest'ultimo, installato in un computer a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="aea90-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="aea90-117">il SDK di 32 bit .NET Core è stata installata mediante un'altra applicazione.</span><span class="sxs-lookup"><span data-stu-id="aea90-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="aea90-118">La versione non corretta è stata scaricata e installata.</span><span class="sxs-lookup"><span data-stu-id="aea90-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="aea90-119">Disinstallare il SDK dei componenti di base .NET a 32 bit per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="aea90-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="aea90-120">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="aea90-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="aea90-121">Se si comprende perché l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="aea90-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="aea90-122">.NET Core SDK viene installato in più posizioni</span><span class="sxs-lookup"><span data-stu-id="aea90-122">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="aea90-123">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core venga visualizzato l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="aea90-123">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

 <span data-ttu-id="aea90-124">.NET Core SDK è installato in più posizioni.</span><span class="sxs-lookup"><span data-stu-id="aea90-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="aea90-125">Solo i modelli dal SDK installato in "c:\Programmi\Microsoft Files\dotnet\sdk\' verranno visualizzati.</span><span class="sxs-lookup"><span data-stu-id="aea90-125">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="aea90-127">Questo messaggio viene visualizzato quando si dispone di almeno un'installazione di .NET Core SDK in una directory all'esterno di * c:\Programmi\Microsoft Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="aea90-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="aea90-128">In genere che si verifica quando il SDK di .NET Core è stato distribuito in una macchina mediante copia/incolla anziché il programma di installazione MSI.</span><span class="sxs-lookup"><span data-stu-id="aea90-128">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="aea90-129">Disinstallare il SDK dei componenti di base .NET a 32 bit per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="aea90-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="aea90-130">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="aea90-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="aea90-131">Se si comprende perché l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="aea90-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="aea90-132">Nessun SDK di .NET Core sono stato rilevato</span><span class="sxs-lookup"><span data-stu-id="aea90-132">No .NET Core SDKs were detected</span></span>
<span data-ttu-id="aea90-133">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core venga visualizzato l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="aea90-133">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

<span data-ttu-id="aea90-134">**Nessun SDK di .NET Core sono stato rilevato, verificare che siano incluse nella variabile di ambiente 'PATH'**</span><span class="sxs-lookup"><span data-stu-id="aea90-134">**No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'**</span></span>

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="aea90-136">Questo avviso viene visualizzato quando la variabile di ambiente `PATH` non fa riferimento a qualsiasi .NET Core SDK nel computer.</span><span class="sxs-lookup"><span data-stu-id="aea90-136">This warning appears when the environment variable `PATH` doesn’t point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="aea90-137">Per risolvere questo problema:</span><span class="sxs-lookup"><span data-stu-id="aea90-137">To resolve this problem:</span></span>

* <span data-ttu-id="aea90-138">Installare o verificare che sia installato il SDK dei componenti di base di .NET.</span><span class="sxs-lookup"><span data-stu-id="aea90-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="aea90-139">Verificare il `PATH` variabile di ambiente punta alla posizione è installato SDK.</span><span class="sxs-lookup"><span data-stu-id="aea90-139">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="aea90-140">Il programma di installazione in genere imposta la `PATH`.</span><span class="sxs-lookup"><span data-stu-id="aea90-140">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-application-deadlocks"></a><span data-ttu-id="aea90-141">Utilizzo di IHtmlHelper.Partial può comportare deadlock di applicazioni</span><span class="sxs-lookup"><span data-stu-id="aea90-141">Use of IHtmlHelper.Partial may result in application deadlocks</span></span>

<span data-ttu-id="aea90-142">In ASP.NET Core 2.1 e versioni successive, la chiamata `Html.Partial` notificata tramite un avviso analyzer a causa del potenziale deadlock.</span><span class="sxs-lookup"><span data-stu-id="aea90-142">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="aea90-143">Il messaggio di avviso è:</span><span class="sxs-lookup"><span data-stu-id="aea90-143">The warning message is:</span></span>

<span data-ttu-id="aea90-144">*Utilizzo di IHtmlHelper.Partial può comportare deadlock di applicazioni. Provare a usare `<partial>` Helper di Tag o `IHtmlHelper.PartialAsync`.*</span><span class="sxs-lookup"><span data-stu-id="aea90-144">*Use of IHtmlHelper.Partial may result in application deadlocks. Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.*</span></span>

<span data-ttu-id="aea90-145">Le chiamate a `@Html.Partial` deve essere sostituito dal `@await Html.PartialAsync` o l'helper di tag parziali `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="aea90-145">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
