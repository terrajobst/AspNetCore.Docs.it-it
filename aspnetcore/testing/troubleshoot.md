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
uid: testing/troubleshoot
ms.openlocfilehash: a75dc666621600e1e2fe36c29acbe7484bae9229
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="19520-103">Risolvere i progetti ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19520-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="19520-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="19520-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="19520-105">I collegamenti seguenti forniscono informazioni aggiuntive sulla risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="19520-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="19520-106">Risolvere i problemi di ASP.NET Core in Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="19520-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="19520-107">Risolvere i problemi di ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="19520-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="19520-108">Errori comuni di Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19520-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="19520-109">YouTube: La diagnosi dei problemi nelle applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19520-109">YouTube: Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="19520-110">Avvisi di .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="19520-110">.NET Core SDK warnings</span></span>

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="19520-111">Entrambi 32 e 64 bit di .NET Core SDK siano installate le versioni</span><span class="sxs-lookup"><span data-stu-id="19520-111">Both the 32 and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="19520-112">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si potrebbe vedere il seguente avviso vengono visualizzati nella parte superiore:</span><span class="sxs-lookup"><span data-stu-id="19520-112">In the **New Project** dialog for ASP.NET Core, you may see the following warning appear at the top:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="19520-114">Questo avviso viene visualizzato quando (x86) 32 bit sia versioni a 64 bit (x64) di [.NET Core SDK](https://www.microsoft.com/net/download/all) installate.</span><span class="sxs-lookup"><span data-stu-id="19520-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="19520-115">È possono installare entrambe le versioni di cause comuni includono:</span><span class="sxs-lookup"><span data-stu-id="19520-115">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="19520-116">Originariamente scaricato il programma di installazione di .NET Core SDK utilizzando un computer a 32 bit, ma quindi copiato quest'ultimo, installato in un computer a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="19520-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="19520-117">il SDK di 32 bit .NET Core è stata installata mediante un'altra applicazione.</span><span class="sxs-lookup"><span data-stu-id="19520-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="19520-118">La versione non corretta è stata scaricata e installata.</span><span class="sxs-lookup"><span data-stu-id="19520-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="19520-119">Disinstallare il SDK dei componenti di base .NET a 32 bit per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="19520-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="19520-120">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="19520-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="19520-121">Se si comprende perché l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="19520-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="19520-122">.NET Core SDK viene installato in più posizioni</span><span class="sxs-lookup"><span data-stu-id="19520-122">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="19520-123">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core venga visualizzato il seguente messaggio di avviso vengono visualizzati nella parte superiore:</span><span class="sxs-lookup"><span data-stu-id="19520-123">In the **New Project** dialog for ASP.NET Core you may see the following warning appear at the top:</span></span> 

 <span data-ttu-id="19520-124">.NET Core SDK è installato in più posizioni.</span><span class="sxs-lookup"><span data-stu-id="19520-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="19520-125">Solo i modelli dal SDK installato in "c:\Programmi\Microsoft Files\dotnet\sdk\' verranno visualizzati.</span><span class="sxs-lookup"><span data-stu-id="19520-125">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="19520-127">Questo messaggio viene visualizzato perché sono presenti almeno un'installazione di .NET Core SDK in una directory all'esterno di * c:\Programmi\Microsoft Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="19520-127">You are seeing this message because you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="19520-128">In genere che si verifica quando il SDK di .NET Core è stato distribuito in una macchina mediante copia/incolla anziché il programma di installazione MSI.</span><span class="sxs-lookup"><span data-stu-id="19520-128">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="19520-129">Disinstallare il SDK dei componenti di base .NET a 32 bit per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="19520-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="19520-130">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="19520-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="19520-131">Se si comprende perché l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="19520-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>
