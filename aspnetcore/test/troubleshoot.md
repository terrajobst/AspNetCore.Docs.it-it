---
title: Risolvere i progetti ASP.NET Core
author: Rick-Anderson
description: Comprendere e risolvere i problemi di avvisi ed errori con i progetti ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: test/troubleshoot
ms.openlocfilehash: 8ff8ddaf4a35a02db650ff429ffbbf4e76a53ecf
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613005"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="702ad-103">Risolvere i progetti ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="702ad-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="702ad-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="702ad-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="702ad-105">I collegamenti seguenti forniscono informazioni aggiuntive sulla risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="702ad-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="702ad-106">Risolvere i problemi di ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="702ad-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="702ad-107">Risolvere i problemi di ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="702ad-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="702ad-108">Errori comuni di Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="702ad-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="702ad-109">Conferenza NDC (Londra, 2018): La diagnosi dei problemi nelle applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="702ad-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="702ad-110">ASP.NET Blog: Risoluzione dei problemi di prestazioni di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="702ad-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="702ad-111">Avvisi di .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="702ad-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="702ad-112">Entrambe le versioni a 64 bit del SDK .NET Core e a 32 bit installate</span><span class="sxs-lookup"><span data-stu-id="702ad-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="702ad-113">Nel **nuovo progetto** dialogo ASP.NET Core, si potrebbe notare l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="702ad-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="702ad-114">32 e 64 bit versioni di .NET Core SDK vengono installate.</span><span class="sxs-lookup"><span data-stu-id="702ad-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="702ad-115">Solo i modelli dalle versioni a 64 bit installate in "c:\\Program Files\\dotnet\\sdk\\' verranno visualizzati.</span><span class="sxs-lookup"><span data-stu-id="702ad-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="702ad-117">Questo avviso viene visualizzato quando (x86) 32 bit sia versioni a 64 bit (x64) di [.NET Core SDK](https://www.microsoft.com/net/download/all) installate.</span><span class="sxs-lookup"><span data-stu-id="702ad-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="702ad-118">È possibile installare entrambe le versioni di cause comuni includono:</span><span class="sxs-lookup"><span data-stu-id="702ad-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="702ad-119">Originariamente scaricato il programma di installazione di .NET Core SDK utilizzando un computer a 32 bit ma quindi copiato quest'ultimo, installato in un computer a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="702ad-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="702ad-120">il SDK di 32 bit .NET Core è stata installata mediante un'altra applicazione.</span><span class="sxs-lookup"><span data-stu-id="702ad-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="702ad-121">La versione non corretta è stata scaricata e installata.</span><span class="sxs-lookup"><span data-stu-id="702ad-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="702ad-122">Disinstallare il SDK dei componenti di base .NET a 32 bit per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="702ad-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="702ad-123">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="702ad-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="702ad-124">Se si comprende perché l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="702ad-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="702ad-125">.NET Core SDK viene installato in più posizioni</span><span class="sxs-lookup"><span data-stu-id="702ad-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="702ad-126">Nel **nuovo progetto** dialogo ASP.NET Core, si potrebbe notare l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="702ad-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="702ad-127">.NET Core SDK è installato in più posizioni.</span><span class="sxs-lookup"><span data-stu-id="702ad-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="702ad-128">Solo i modelli dal SDK installato in "c:\\Program Files\\dotnet\\sdk\\' verranno visualizzati.</span><span class="sxs-lookup"><span data-stu-id="702ad-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="702ad-130">Questo messaggio viene visualizzato quando si dispone di almeno un'installazione di .NET Core SDK in una directory all'esterno di *c:.\\file di programma\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="702ad-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="702ad-131">In genere ciò si verifica quando il SDK di .NET Core è stato distribuito in un computer mediante copia/incolla anziché il programma di installazione MSI.</span><span class="sxs-lookup"><span data-stu-id="702ad-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="702ad-132">Disinstallare il SDK dei componenti di base .NET a 32 bit per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="702ad-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="702ad-133">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="702ad-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="702ad-134">Se si comprende perché l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="702ad-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="702ad-135">Nessun SDK di .NET Core sono stato rilevato</span><span class="sxs-lookup"><span data-stu-id="702ad-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="702ad-136">Nel **nuovo progetto** dialogo ASP.NET Core, si potrebbe notare l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="702ad-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="702ad-137">Nessun SDK di .NET Core sono stato rilevato, verificare che siano incluse nella variabile di ambiente 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="702ad-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="702ad-139">Questo avviso viene visualizzato quando la variabile di ambiente `PATH` non fa riferimento a qualsiasi .NET Core SDK nel computer.</span><span class="sxs-lookup"><span data-stu-id="702ad-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="702ad-140">Per risolvere questo problema:</span><span class="sxs-lookup"><span data-stu-id="702ad-140">To resolve this problem:</span></span>

* <span data-ttu-id="702ad-141">Installare o verificare che sia installato il SDK dei componenti di base di .NET.</span><span class="sxs-lookup"><span data-stu-id="702ad-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="702ad-142">Verificare il `PATH` variabile di ambiente punta alla posizione è installato SDK.</span><span class="sxs-lookup"><span data-stu-id="702ad-142">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="702ad-143">Il programma di installazione in genere imposta la `PATH`.</span><span class="sxs-lookup"><span data-stu-id="702ad-143">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a><span data-ttu-id="702ad-144">Utilizzo di IHtmlHelper.Partial può comportare deadlock app</span><span class="sxs-lookup"><span data-stu-id="702ad-144">Use of IHtmlHelper.Partial may result in app deadlocks</span></span>

<span data-ttu-id="702ad-145">In ASP.NET Core 2.1 e versioni successive, la chiamata `Html.Partial` notificata tramite un avviso analyzer a causa del potenziale deadlock.</span><span class="sxs-lookup"><span data-stu-id="702ad-145">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="702ad-146">Il messaggio di avviso è:</span><span class="sxs-lookup"><span data-stu-id="702ad-146">The warning message is:</span></span>

> <span data-ttu-id="702ad-147">Utilizzo di IHtmlHelper.Partial può comportare deadlock di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="702ad-147">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="702ad-148">Provare a usare `<partial>` Helper di Tag o `IHtmlHelper.PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="702ad-148">Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.</span></span>

<span data-ttu-id="702ad-149">Le chiamate a `@Html.Partial` deve essere sostituito dal `@await Html.PartialAsync` o l'helper di tag parziali `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="702ad-149">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
