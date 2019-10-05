---
title: Errori comuni di Servizio app di Azure e IIS con ASP.NET Core
author: guardrex
description: Ottenere suggerimenti per la risoluzione degli errori comuni dell'hosting di app ASP.NET Core in Servizio app di Azure e IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 047ef23bd2f4d349d2d342d17764c7edd3e0de4a
ms.sourcegitcommit: 4649814d1ae32248419da4e8f8242850fd8679a5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/05/2019
ms.locfileid: "71975684"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="d5395-103">Errori comuni di Servizio app di Azure e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5395-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="d5395-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d5395-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d5395-105">Questo argomento descrive gli errori comuni e fornisce consigli per la risoluzione di errori specifici quando si ospitano ASP.NET Core app nel servizio app di Azure e in IIS.</span><span class="sxs-lookup"><span data-stu-id="d5395-105">This topic describes common errors and provides troubleshooting advice for specific errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="d5395-106">Per indicazioni generali sulla risoluzione dei problemi, vedere <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="d5395-106">For general troubleshooting guidance, see <xref:test/troubleshoot-azure-iis>.</span></span>

<span data-ttu-id="d5395-107">Raccogliere le seguenti informazioni:</span><span class="sxs-lookup"><span data-stu-id="d5395-107">Collect the following information:</span></span>

* <span data-ttu-id="d5395-108">Comportamento del browser (codice di stato e messaggio di errore)</span><span class="sxs-lookup"><span data-stu-id="d5395-108">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="d5395-109">Voci del log eventi dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="d5395-109">Application Event Log entries</span></span>
  * <span data-ttu-id="d5395-110">Servizio app di Azure &ndash; Vedere <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="d5395-110">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="d5395-111">IIS</span><span class="sxs-lookup"><span data-stu-id="d5395-111">IIS</span></span>
    1. <span data-ttu-id="d5395-112">Selezionare **Start** nel menu di **Windows**, digitare *Visualizzatore eventi* e premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="d5395-112">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="d5395-113">Dopo l'apertura di **Visualizzatore eventi**, espandere **Registri di Windows** > **Applicazione** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="d5395-113">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="d5395-114">Voci del log per debug e stdout di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5395-114">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="d5395-115">Servizio app di Azure &ndash; Vedere <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="d5395-115">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="d5395-116">IIS &ndash; Seguire le istruzioni nelle sezioni [Creazione e reindirizzamento dei log](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) e [Log di diagnostica avanzati](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) dell'argomento Modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5395-116">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="d5395-117">Confrontare le informazioni sugli errori con gli errori comuni seguenti.</span><span class="sxs-lookup"><span data-stu-id="d5395-117">Compare error information to the following common errors.</span></span> <span data-ttu-id="d5395-118">Se viene trovata una corrispondenza, seguire le indicazioni per la risoluzione dei problemi.</span><span class="sxs-lookup"><span data-stu-id="d5395-118">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="d5395-119">L'elenco degli errori in questo argomento non è esaustivo.</span><span class="sxs-lookup"><span data-stu-id="d5395-119">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="d5395-120">Se si verifica un errore non elencato qui, aprire un nuovo problema tramite il pulsante per l'invio di **feedback per il contenuto** nella parte inferiore di questo argomento con istruzioni dettagliate su come riprodurre l'errore.</span><span class="sxs-lookup"><span data-stu-id="d5395-120">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="d5395-121">L'aggiornamento del sistema operativo ha rimosso il modulo di ASP.NET Core a 32 bit</span><span class="sxs-lookup"><span data-stu-id="d5395-121">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="d5395-122">**Registro dell'applicazione:** Impossibile caricare la DLL del modulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**.</span><span class="sxs-lookup"><span data-stu-id="d5395-122">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="d5395-123">L'errore è nei dati.</span><span class="sxs-lookup"><span data-stu-id="d5395-123">The data is the error.</span></span>

<span data-ttu-id="d5395-124">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-124">Troubleshooting:</span></span>

<span data-ttu-id="d5395-125">I file non appartenenti al sistema operativo presenti nella directory **C:\Windows\SysWOW64\inetsrv** non vengono mantenuti durante un aggiornamento del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="d5395-125">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="d5395-126">Se il modulo ASP.NET Core viene installato prima di un aggiornamento del sistema operativo e quindi si esegue qualsiasi pool di app in modalità a 32 bit dopo l'aggiornamento, si verifica questo problema.</span><span class="sxs-lookup"><span data-stu-id="d5395-126">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="d5395-127">Dopo un aggiornamento del sistema operativo, ripristinare il Modulo di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5395-127">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="d5395-128">Vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="d5395-128">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="d5395-129">Selezionare **Ripara** quando viene eseguito il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="d5395-129">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="d5395-130">Estensione del sito mancante, estensioni del sito a 32 bit (x86) e a 64 bit (x64) installate o numero di bit per il processo impostato errato</span><span class="sxs-lookup"><span data-stu-id="d5395-130">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="d5395-131">*Si applica alle app ospitate da Servizi app di Azure.*</span><span class="sxs-lookup"><span data-stu-id="d5395-131">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="d5395-132">**Browser:** Errore HTTP 500.0 - Errore di caricamento gestore In-Process ANCM</span><span class="sxs-lookup"><span data-stu-id="d5395-132">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="d5395-133">**Registro dell'applicazione:** Chiamata di hostfxr per trovare il gestore delle richieste In-Process non riuscita senza trovare dipendenze native.</span><span class="sxs-lookup"><span data-stu-id="d5395-133">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="d5395-134">Non è stato possibile trovare il gestore delle richieste In-Process.</span><span class="sxs-lookup"><span data-stu-id="d5395-134">Could not find inprocess request handler.</span></span> <span data-ttu-id="d5395-135">Output acquisito dalla chiamata di hostfxr: Non è stato possibile trovare qualsiasi versione del framework compatibile.</span><span class="sxs-lookup"><span data-stu-id="d5395-135">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="d5395-136">Impossibile trovare la versione del framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' specificata.</span><span class="sxs-lookup"><span data-stu-id="d5395-136">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="d5395-137">Impossibile avviare l'applicazione '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span><span class="sxs-lookup"><span data-stu-id="d5395-137">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="d5395-138">**Log stdout del modulo ASP.NET Core:** Non è stato possibile trovare qualsiasi versione del framework compatibile.</span><span class="sxs-lookup"><span data-stu-id="d5395-138">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="d5395-139">Impossibile trovare la versione del framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' specificata.</span><span class="sxs-lookup"><span data-stu-id="d5395-139">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d5395-140">**Log di debug del modulo ASP.NET Core:** Chiamata di hostfxr per trovare il gestore delle richieste In-Process non riuscita senza trovare dipendenze native.</span><span class="sxs-lookup"><span data-stu-id="d5395-140">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="d5395-141">Questo errore probabilmente significa che l'app non è configurata correttamente. Controllare le versioni di Microsoft.NetCore.App e Microsoft.AspNetCore.App specificate come destinazione dall'applicazione e installate nel computer.</span><span class="sxs-lookup"><span data-stu-id="d5395-141">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="d5395-142">Restituito HRESULT non riuscito: 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="d5395-142">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="d5395-143">Non è stato possibile trovare il gestore delle richieste In-Process.</span><span class="sxs-lookup"><span data-stu-id="d5395-143">Could not find inprocess request handler.</span></span> <span data-ttu-id="d5395-144">Non è stato possibile trovare qualsiasi versione del framework compatibile.</span><span class="sxs-lookup"><span data-stu-id="d5395-144">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="d5395-145">Impossibile trovare la versione del framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' specificata.</span><span class="sxs-lookup"><span data-stu-id="d5395-145">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker-end

<span data-ttu-id="d5395-146">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-146">Troubleshooting:</span></span>

* <span data-ttu-id="d5395-147">Se l'app è in esecuzione in un runtime di anteprima, installare l'estensione del sito a 32 bit (x86) **o** a 64 bit (x64) corrispondente al numero di bit dell'app e alla versione del runtime dell'app.</span><span class="sxs-lookup"><span data-stu-id="d5395-147">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="d5395-148">**Non installare entrambe le estensioni o più versioni di runtime dell'estensione.**</span><span class="sxs-lookup"><span data-stu-id="d5395-148">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="d5395-149">Runtime ASP.NET Core {RUNTIME VERSION} (x86)</span><span class="sxs-lookup"><span data-stu-id="d5395-149">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="d5395-150">Runtime ASP.NET Core {RUNTIME VERSION} (x64)</span><span class="sxs-lookup"><span data-stu-id="d5395-150">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="d5395-151">Riavviare l'app.</span><span class="sxs-lookup"><span data-stu-id="d5395-151">Restart the app.</span></span> <span data-ttu-id="d5395-152">Attendere alcuni secondi per il riavvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="d5395-152">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="d5395-153">Se l'app è in esecuzione in un runtime di anteprima e sono installate entrambe le [estensioni del sito](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) a 32 bit (x86) e a 64 bit (x64), disinstallare l'estensione del sito che non corrisponde al numero di bit dell'app.</span><span class="sxs-lookup"><span data-stu-id="d5395-153">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="d5395-154">Dopo aver rimosso l'estensione del sito, riavviare l'app.</span><span class="sxs-lookup"><span data-stu-id="d5395-154">After removing the site extension, restart the app.</span></span> <span data-ttu-id="d5395-155">Attendere alcuni secondi per il riavvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="d5395-155">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="d5395-156">Se l'app è in esecuzione in un runtime di anteprima e il numero di bit dell'estensione del sito corrisponde a quello dell'app, verificare che la *versione del runtime* dell'estensione del sito di anteprima corrisponda alla versione del runtime dell'app.</span><span class="sxs-lookup"><span data-stu-id="d5395-156">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="d5395-157">Verificare che la **piattaforma** dell'app in **Impostazioni applicazione** corrisponda al numero di bit dell'app.</span><span class="sxs-lookup"><span data-stu-id="d5395-157">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="d5395-158">Per altre informazioni, vedere <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span><span class="sxs-lookup"><span data-stu-id="d5395-158">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="d5395-159">Un'app x86 viene distribuita, ma il pool di app non è abilitato per le app a 32 bit</span><span class="sxs-lookup"><span data-stu-id="d5395-159">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="d5395-160">**Browser:** Errore HTTP 500.30 - Errore di avvio In-Process ANCM</span><span class="sxs-lookup"><span data-stu-id="d5395-160">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="d5395-161">**Registro dell'applicazione:** Eccezione gestita imprevista per l'applicazione '/LM/W3SVC/5/ROOT' con radice fisica '{PATH}' Codice eccezione = '0xe0434352'.</span><span class="sxs-lookup"><span data-stu-id="d5395-161">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="d5395-162">Controllare i log stderr per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="d5395-162">Please check the stderr logs for more information.</span></span> <span data-ttu-id="d5395-163">Applicazione '/LM/W3SVC/5/ROOT' con radice fisica '{PATH}'. Impossibile caricare clr e applicazione gestita.</span><span class="sxs-lookup"><span data-stu-id="d5395-163">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="d5395-164">Chiusura prematura del thread di lavoro CLR</span><span class="sxs-lookup"><span data-stu-id="d5395-164">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="d5395-165">**Log stdout del modulo ASP.NET Core:** il file di log viene creato ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="d5395-165">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d5395-166">**Log di debug del modulo ASP.NET Core:** Restituito HRESULT non riuscito: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="d5395-166">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="d5395-167">Questo scenario viene intercettato dall'SDK durante la pubblicazione di un'app autonoma.</span><span class="sxs-lookup"><span data-stu-id="d5395-167">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="d5395-168">L'SDK genera un errore se il RID non corrisponde alla piattaforma di destinazione (ad esempio, RID `win10-x64` con `<PlatformTarget>x86</PlatformTarget>` nel file di progetto).</span><span class="sxs-lookup"><span data-stu-id="d5395-168">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="d5395-169">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-169">Troubleshooting:</span></span>

<span data-ttu-id="d5395-170">Per una distribuzione dipendente dal framework x86 (`<PlatformTarget>x86</PlatformTarget>`), abilitare il pool di app IIS per le app a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="d5395-170">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="d5395-171">In Gestione IIS aprire **Impostazioni avanzate** per il pool di app e impostare **Attiva applicazioni a 32 bit** su **True**.</span><span class="sxs-lookup"><span data-stu-id="d5395-171">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="d5395-172">La piattaforma è in conflitto con RID</span><span class="sxs-lookup"><span data-stu-id="d5395-172">Platform conflicts with RID</span></span>

* <span data-ttu-id="d5395-173">**Browser:** errore HTTP 502.5 - Errore del processo</span><span class="sxs-lookup"><span data-stu-id="d5395-173">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="d5395-174">**Registro dell'applicazione:** L'applicazione 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con radice fisica 'C:\{PATH}\' non è riuscita ad avviare il processo con la riga di comando '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', Codice errore = '0x80004005 : ff.</span><span class="sxs-lookup"><span data-stu-id="d5395-174">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="d5395-175">**Log stdout del modulo ASP.NET Core:** Eccezione non gestita: System.BadImageFormatException: Impossibile caricare il file o l'assembly '{ASSEMBLY}.dll'.</span><span class="sxs-lookup"><span data-stu-id="d5395-175">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="d5395-176">Tentativo di caricare un programma con un formato non corretto.</span><span class="sxs-lookup"><span data-stu-id="d5395-176">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="d5395-177">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-177">Troubleshooting:</span></span>

* <span data-ttu-id="d5395-178">Confermare che l'app sia eseguita in locale su Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d5395-178">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="d5395-179">Un errore del processo può essere causato da un problema interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d5395-179">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="d5395-180">Per altre informazioni, vedere <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="d5395-180">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="d5395-181">Se questa eccezione si verifica per una distribuzione di App di Azure durante l'aggiornamento di un'app e la distribuzione di assembly più recenti, eliminare manualmente tutti i file dalla distribuzione precedente.</span><span class="sxs-lookup"><span data-stu-id="d5395-181">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="d5395-182">Le assembly incompatibili con il tempo di ritardo possono causare una eccezione `System.BadImageFormatException` quando si distribuisce un'app aggiornata.</span><span class="sxs-lookup"><span data-stu-id="d5395-182">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="d5395-183">Endpoint dell'URI non corretto o sito web arrestato</span><span class="sxs-lookup"><span data-stu-id="d5395-183">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="d5395-184">**Browser:** ERR_CONNECTION_REFUSED **--oppure--** La connessione non è riuscita</span><span class="sxs-lookup"><span data-stu-id="d5395-184">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="d5395-185">**Registro dell'applicazione:** nessuna voce</span><span class="sxs-lookup"><span data-stu-id="d5395-185">**Application Log:** No entry</span></span>

* <span data-ttu-id="d5395-186">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-186">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d5395-187">**Log di debug del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-187">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d5395-188">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-188">Troubleshooting:</span></span>

* <span data-ttu-id="d5395-189">Verificare che sia in uso l'endpoint URI corretto per l'app.</span><span class="sxs-lookup"><span data-stu-id="d5395-189">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="d5395-190">Controllare i binding.</span><span class="sxs-lookup"><span data-stu-id="d5395-190">Check the bindings.</span></span>

* <span data-ttu-id="d5395-191">Verificare che il sito Web IIS non sia nello stato *In arresto*.</span><span class="sxs-lookup"><span data-stu-id="d5395-191">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="d5395-192">Le funzionalità del server CoreWebEngine o W3SVC sono disabilitate</span><span class="sxs-lookup"><span data-stu-id="d5395-192">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="d5395-193">**Eccezione del sistema operativo:** per usare il modulo ASP.NET Core è necessario installare le funzionalità di IIS 7.0 CoreWebEngine e W3SVC.</span><span class="sxs-lookup"><span data-stu-id="d5395-193">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="d5395-194">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-194">Troubleshooting:</span></span>

<span data-ttu-id="d5395-195">Verificare che il ruolo e le funzionalità appropriati siano abilitati.</span><span class="sxs-lookup"><span data-stu-id="d5395-195">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="d5395-196">Vedere [Configurazione di IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="d5395-196">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="d5395-197">Percorso fisico del sito Web non corretto o app mancante</span><span class="sxs-lookup"><span data-stu-id="d5395-197">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="d5395-198">**Browser:** 403 Non consentito: accesso negato **--oppure--** 403.14 Non consentito - Il server Web è configurato per non consentire la visualizzazione del contenuto dalla directory.</span><span class="sxs-lookup"><span data-stu-id="d5395-198">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="d5395-199">**Registro dell'applicazione:** nessuna voce</span><span class="sxs-lookup"><span data-stu-id="d5395-199">**Application Log:** No entry</span></span>

* <span data-ttu-id="d5395-200">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-200">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d5395-201">**Log di debug del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-201">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d5395-202">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-202">Troubleshooting:</span></span>

<span data-ttu-id="d5395-203">Controllare il sito Web IIS **Impostazioni di base** e la cartella dell'app fisica.</span><span class="sxs-lookup"><span data-stu-id="d5395-203">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="d5395-204">Verificare che l'app si trovi nella cartella nel sito Web IIS **Percorso fisico**.</span><span class="sxs-lookup"><span data-stu-id="d5395-204">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="d5395-205">Ruolo non corretto, modulo ASP.NET Core non installato o autorizzazioni non corrette</span><span class="sxs-lookup"><span data-stu-id="d5395-205">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="d5395-206">**Browser:** 500.19 Errore interno al server - Non è possibile accedere alla pagina richiesta in quanto i dati di configurazione correlati alla pagina non sono validi.</span><span class="sxs-lookup"><span data-stu-id="d5395-206">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="d5395-207">**--Oppure--** Non è possibile visualizzare questa pagina</span><span class="sxs-lookup"><span data-stu-id="d5395-207">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="d5395-208">**Registro dell'applicazione:** nessuna voce</span><span class="sxs-lookup"><span data-stu-id="d5395-208">**Application Log:** No entry</span></span>

* <span data-ttu-id="d5395-209">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-209">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d5395-210">**Log di debug del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-210">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d5395-211">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-211">Troubleshooting:</span></span>

* <span data-ttu-id="d5395-212">Verificare che il ruolo appropriato sia abilitato.</span><span class="sxs-lookup"><span data-stu-id="d5395-212">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="d5395-213">Vedere [Configurazione di IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="d5395-213">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="d5395-214">Aprire **Programmi e funzionalità** oppure **App e funzionalità** e verificare che sia installato **Windows Server Hosting**.</span><span class="sxs-lookup"><span data-stu-id="d5395-214">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="d5395-215">Se **Windows Server Hosting** non è presente nell'elenco dei programmi installati, scaricare e installare il bundle di hosting .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5395-215">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="d5395-216">Programma di installazione del bundle di hosting .NET Core corrente (download diretto)</span><span class="sxs-lookup"><span data-stu-id="d5395-216">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="d5395-217">Per altre informazioni, vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="d5395-217">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="d5395-218">Assicurarsi che **Pool di applicazioni** > **Modello di processo** > **Identità** sia impostato su **ApplicationPoolIdentity** o che l'identità personalizzata disponga delle autorizzazioni appropriate per accedere alla cartella di distribuzione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d5395-218">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="d5395-219">Se il bundle di hosting ASP.NET Core è stato disinstallato e quindi è stata installata una versione del bundle di hosting precedente, il file *applicationHost.config* non include una sezione per il modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5395-219">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="d5395-220">Aprire *applicationHost.config* in *%windir%/System32/inetsrv/config* e trovare il gruppo di sezioni `<configuration><configSections><sectionGroup name="system.webServer">`.</span><span class="sxs-lookup"><span data-stu-id="d5395-220">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="d5395-221">Se la sezione per il modulo ASP.NET Core non è presente nel gruppo di sezioni, aggiungere l'elemento della sezione:</span><span class="sxs-lookup"><span data-stu-id="d5395-221">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="d5395-222">In alternativa installare la versione più recente del bundle di hosting ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5395-222">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="d5395-223">La versione più recente è compatibile con le app ASP.NET Core supportate.</span><span class="sxs-lookup"><span data-stu-id="d5395-223">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="d5395-224">ProcessPath non corretto, variabile di percorso mancante, aggregazione di hosting non installata, sistema/IIS non riavviato, VC Redistributable non installato o violazione dell'accesso a dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="d5395-224">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d5395-225">**Browser:** Errore HTTP 500.0 - Errore di caricamento gestore In-Process ANCM</span><span class="sxs-lookup"><span data-stu-id="d5395-225">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="d5395-226">**Registro dell'applicazione:** L'applicazione 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con radice fisica 'C:\{PATH}\' non è riuscita ad avviare il processo con la riga di comando "{...}"</span><span class="sxs-lookup"><span data-stu-id="d5395-226">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="d5395-227">', ErrorCode = '0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="d5395-227">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="d5395-228">Impossibile avviare l'applicazione '{PATH}'.</span><span class="sxs-lookup"><span data-stu-id="d5395-228">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="d5395-229">Eseguibile non trovato in '{PATH}'.</span><span class="sxs-lookup"><span data-stu-id="d5395-229">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="d5395-230">Impossibile avviare l'applicazione '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span><span class="sxs-lookup"><span data-stu-id="d5395-230">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="d5395-231">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-231">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="d5395-232">**Log di debug del modulo ASP.NET Core:** Registro eventi: 'Avvio dell'applicazione '{PATH}' non riuscito.</span><span class="sxs-lookup"><span data-stu-id="d5395-232">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="d5395-233">Eseguibile non trovato in '{PATH}'.</span><span class="sxs-lookup"><span data-stu-id="d5395-233">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="d5395-234">Restituito HRESULT non riuscito: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="d5395-234">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d5395-235">**Browser:** errore HTTP 502.5 - Errore del processo</span><span class="sxs-lookup"><span data-stu-id="d5395-235">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="d5395-236">**Registro dell'applicazione:** L'applicazione 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' con radice fisica 'C:\{PATH}\' non è riuscita ad avviare il processo con la riga di comando "{...}"</span><span class="sxs-lookup"><span data-stu-id="d5395-236">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="d5395-237">', ErrorCode = '0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="d5395-237">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="d5395-238">**Log stdout del modulo ASP.NET Core:** il file di log viene creato ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="d5395-238">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="d5395-239">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-239">Troubleshooting:</span></span>

* <span data-ttu-id="d5395-240">Confermare che l'app sia eseguita in locale su Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d5395-240">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="d5395-241">Un errore del processo può essere causato da un problema interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d5395-241">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="d5395-242">Per altre informazioni, vedere <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="d5395-242">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="d5395-243">Verificare che l'attributo *processPath* dell'elemento `<aspNetCore>` in *web.config* sia `dotnet` per una distribuzione dipendente da framework (FDD, Framework-Dependent Deployment) o `.\{ASSEMBLY}.exe` per una [distribuzione autonoma (SCD, Self-Contained Deployment)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="d5395-243">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="d5395-244">Per una distribuzione dipendente dal framework, *dotnet.exe* potrebbe non essere accessibile tramite le impostazioni del percorso.</span><span class="sxs-lookup"><span data-stu-id="d5395-244">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="d5395-245">Confermare che *C:\Programmi\dotnet\\* esiste nelle impostazioni PATH di sistema.</span><span class="sxs-lookup"><span data-stu-id="d5395-245">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="d5395-246">Per una distribuzione dipendente da framework, *dotnet.exe* potrebbe non essere accessibile per l'identità utente del pool di app.</span><span class="sxs-lookup"><span data-stu-id="d5395-246">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="d5395-247">Confermare che l'identità dell'utente del pool di app abbia accesso alla directory *C:\Programmi\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="d5395-247">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="d5395-248">Verificare che non siano presenti regole di negazione configurate per l'identità dell'utente del pool di app in *C:\Programmi\dotnet* e nelle directory dell'app.</span><span class="sxs-lookup"><span data-stu-id="d5395-248">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="d5395-249">È possibile che sia stata eseguita una distribuzione FDD e che sia stato installato .NET Core senza riavviare IIS.</span><span class="sxs-lookup"><span data-stu-id="d5395-249">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="d5395-250">Riavviare il server o riavviare IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="d5395-250">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="d5395-251">È possibile che sia stata eseguita una distribuzione FDD senza installare il runtime .NET Core nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="d5395-251">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="d5395-252">Se il runtime .NET Core non è stato installato, eseguire il **programma di installazione del bundle di hosting .NET Core** nel sistema.</span><span class="sxs-lookup"><span data-stu-id="d5395-252">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="d5395-253">Programma di installazione del bundle di hosting .NET Core corrente (download diretto)</span><span class="sxs-lookup"><span data-stu-id="d5395-253">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="d5395-254">Per altre informazioni, vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="d5395-254">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="d5395-255">Se è necessario un runtime specifico, scaricare il runtime dagli [archivi di download .NET](https://dotnet.microsoft.com/download/archives) e installarlo nel sistema.</span><span class="sxs-lookup"><span data-stu-id="d5395-255">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="d5395-256">Completare l'installazione riavviando il sistema o riavviando IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="d5395-256">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="d5395-257">Argomenti non corretti dell'elemento \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="d5395-257">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d5395-258">**Browser:** Errore HTTP 500.0 - Errore di caricamento gestore In-Process ANCM</span><span class="sxs-lookup"><span data-stu-id="d5395-258">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="d5395-259">**Registro dell'applicazione:** Chiamata di hostfxr per trovare il gestore delle richieste In-Process non riuscita senza trovare dipendenze native.</span><span class="sxs-lookup"><span data-stu-id="d5395-259">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="d5395-260">Questo errore probabilmente significa che l'app non è configurata correttamente. Controllare le versioni di Microsoft.NetCore.App e Microsoft.AspNetCore.App specificate come destinazione dall'applicazione e installate nel computer.</span><span class="sxs-lookup"><span data-stu-id="d5395-260">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="d5395-261">Non è stato possibile trovare il gestore delle richieste In-Process.</span><span class="sxs-lookup"><span data-stu-id="d5395-261">Could not find inprocess request handler.</span></span> <span data-ttu-id="d5395-262">Output acquisito dalla chiamata di hostfxr: Si intendeva eseguire comandi di dotnet SDK?</span><span class="sxs-lookup"><span data-stu-id="d5395-262">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="d5395-263">Installare dotnet SDK da: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Impossibile avviare l'applicazione '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span><span class="sxs-lookup"><span data-stu-id="d5395-263">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="d5395-264">**Log stdout del modulo ASP.NET Core:** Si intendeva eseguire comandi di dotnet SDK?</span><span class="sxs-lookup"><span data-stu-id="d5395-264">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="d5395-265">Installare dotnet SDK da: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="d5395-265">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="d5395-266">**Log di debug del modulo ASP.NET Core:** Chiamata di hostfxr per trovare il gestore delle richieste In-Process non riuscita senza trovare dipendenze native.</span><span class="sxs-lookup"><span data-stu-id="d5395-266">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="d5395-267">Questo errore probabilmente significa che l'app non è configurata correttamente. Controllare le versioni di Microsoft.NetCore.App e Microsoft.AspNetCore.App specificate come destinazione dall'applicazione e installate nel computer.</span><span class="sxs-lookup"><span data-stu-id="d5395-267">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="d5395-268">Restituito HRESULT non riuscito: 0x8000ffff Non è stato possibile trovare il gestore delle richieste In-Process.</span><span class="sxs-lookup"><span data-stu-id="d5395-268">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="d5395-269">Output acquisito dalla chiamata di hostfxr: Si intendeva eseguire comandi di dotnet SDK?</span><span class="sxs-lookup"><span data-stu-id="d5395-269">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="d5395-270">Installare dotnet SDK da: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Restituito HRESULT non riuscito: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="d5395-270">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d5395-271">**Browser:** errore HTTP 502.5 - Errore del processo</span><span class="sxs-lookup"><span data-stu-id="d5395-271">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="d5395-272">**Registro dell'applicazione:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span><span class="sxs-lookup"><span data-stu-id="d5395-272">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="d5395-273">**Log stdout del modulo ASP.NET Core:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span><span class="sxs-lookup"><span data-stu-id="d5395-273">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="d5395-274">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-274">Troubleshooting:</span></span>

* <span data-ttu-id="d5395-275">Confermare che l'app sia eseguita in locale su Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d5395-275">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="d5395-276">Un errore del processo può essere causato da un problema interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d5395-276">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="d5395-277">Per altre informazioni, vedere <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="d5395-277">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="d5395-278">Verificare che l'attributo *arguments* dell'elemento `<aspNetCore>` in *web.config* sia (a) `.\{ASSEMBLY}.dll` per una distribuzione dipendente da framework (FDD) o (b) non disponibile, una stringa vuota (`arguments=""`) o un elenco degli argomenti dell'app (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) per una distribuzione autonoma (SCD).</span><span class="sxs-lookup"><span data-stu-id="d5395-278">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="d5395-279">Framework condiviso di .NET Core mancante</span><span class="sxs-lookup"><span data-stu-id="d5395-279">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="d5395-280">**Browser:** Errore HTTP 500.0 - Errore di caricamento gestore In-Process ANCM</span><span class="sxs-lookup"><span data-stu-id="d5395-280">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="d5395-281">**Registro dell'applicazione:** Chiamata di hostfxr per trovare il gestore delle richieste In-Process non riuscita senza trovare dipendenze native.</span><span class="sxs-lookup"><span data-stu-id="d5395-281">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="d5395-282">Questo errore probabilmente significa che l'app non è configurata correttamente. Controllare le versioni di Microsoft.NetCore.App e Microsoft.AspNetCore.App specificate come destinazione dall'applicazione e installate nel computer.</span><span class="sxs-lookup"><span data-stu-id="d5395-282">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="d5395-283">Non è stato possibile trovare il gestore delle richieste In-Process.</span><span class="sxs-lookup"><span data-stu-id="d5395-283">Could not find inprocess request handler.</span></span> <span data-ttu-id="d5395-284">Output acquisito dalla chiamata di hostfxr: Non è stato possibile trovare qualsiasi versione del framework compatibile.</span><span class="sxs-lookup"><span data-stu-id="d5395-284">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="d5395-285">Impossibile trovare la versione del framework 'Microsoft.AspNetCore.App' specificata '{VERSION}'.</span><span class="sxs-lookup"><span data-stu-id="d5395-285">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="d5395-286">Impossibile avviare l'applicazione '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span><span class="sxs-lookup"><span data-stu-id="d5395-286">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="d5395-287">**Log stdout del modulo ASP.NET Core:** Non è stato possibile trovare qualsiasi versione del framework compatibile.</span><span class="sxs-lookup"><span data-stu-id="d5395-287">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="d5395-288">Impossibile trovare la versione del framework 'Microsoft.AspNetCore.App' specificata '{VERSION}'.</span><span class="sxs-lookup"><span data-stu-id="d5395-288">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="d5395-289">**Log di debug del modulo ASP.NET Core:** Restituito HRESULT non riuscito: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="d5395-289">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="d5395-290">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-290">Troubleshooting:</span></span>

<span data-ttu-id="d5395-291">Per una distribuzione dipendente da framework (FDD), verificare che nel sistema sia installato il runtime corretto.</span><span class="sxs-lookup"><span data-stu-id="d5395-291">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="d5395-292">Pool di applicazioni arrestato</span><span class="sxs-lookup"><span data-stu-id="d5395-292">Stopped Application Pool</span></span>

* <span data-ttu-id="d5395-293">**Browser:** 503 Servizio non disponibile</span><span class="sxs-lookup"><span data-stu-id="d5395-293">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="d5395-294">**Registro dell'applicazione:** nessuna voce</span><span class="sxs-lookup"><span data-stu-id="d5395-294">**Application Log:** No entry</span></span>

* <span data-ttu-id="d5395-295">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-295">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d5395-296">**Log di debug del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-296">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d5395-297">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-297">Troubleshooting:</span></span>

<span data-ttu-id="d5395-298">Confermare che il pool di applicazioni non sia nello stato *Arrestato*.</span><span class="sxs-lookup"><span data-stu-id="d5395-298">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="d5395-299">L'app secondaria include una sezione \<handlers></span><span class="sxs-lookup"><span data-stu-id="d5395-299">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="d5395-300">**Browser:** errore HTTP 500.19 - Errore del server interno</span><span class="sxs-lookup"><span data-stu-id="d5395-300">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="d5395-301">**Registro dell'applicazione:** nessuna voce</span><span class="sxs-lookup"><span data-stu-id="d5395-301">**Application Log:** No entry</span></span>

* <span data-ttu-id="d5395-302">**Log stdout del modulo ASP.NET Core:** il file di log dell'app radice viene creato e mostra un funzionamento normale.</span><span class="sxs-lookup"><span data-stu-id="d5395-302">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="d5395-303">Il file di log dell'app secondaria non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-303">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d5395-304">**Log di debug del modulo ASP.NET Core:** il file di log dell'app radice viene creato e mostra un funzionamento normale.</span><span class="sxs-lookup"><span data-stu-id="d5395-304">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="d5395-305">Il file di log dell'app secondaria non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-305">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d5395-306">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-306">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d5395-307">Verificare che il file *web.config* dell'app secondaria non includa una sezione `<handlers>` o che l'app secondaria non erediti i gestori dell'app padre.</span><span class="sxs-lookup"><span data-stu-id="d5395-307">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="d5395-308">La sezione `<system.webServer>` dell'app padre di *web.config* viene inserita all'interno di un elemento `<location>`.</span><span class="sxs-lookup"><span data-stu-id="d5395-308">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="d5395-309">La proprietà <xref:System.Configuration.SectionInformation.InheritInChildApplications*> è impostata su `false` per indicare che le impostazioni specificate nell'elemento [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) non sono ereditate da app che risiedono in una sottodirectory dell'app padre.</span><span class="sxs-lookup"><span data-stu-id="d5395-309">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="d5395-310">Per altre informazioni, vedere <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="d5395-310">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d5395-311">Verificare che il file *web.config* della sotto-applicazione non includa una sezione `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="d5395-311">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="d5395-312">percorso errato del log stdout</span><span class="sxs-lookup"><span data-stu-id="d5395-312">stdout log path incorrect</span></span>

* <span data-ttu-id="d5395-313">**Browser:** l'app risponde normalmente.</span><span class="sxs-lookup"><span data-stu-id="d5395-313">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d5395-314">**Registro dell'applicazione:** non è possibile avviare il reindirizzamento di stdout in C:\Programmi\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="d5395-314">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="d5395-315">Messaggio di eccezione: HRESULT 0x80070005 restituito in {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="d5395-315">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="d5395-316">Non è possibile arrestare il reindirizzamento di stdout in C:\Programmi\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="d5395-316">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="d5395-317">Messaggio di eccezione: HRESULT 0x80070002 restituito in {PATH}.</span><span class="sxs-lookup"><span data-stu-id="d5395-317">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="d5395-318">Non è possibile avviare il reindirizzamento di stdout in {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="d5395-318">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="d5395-319">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-319">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="d5395-320">**Log di debug del modulo ASP.NET Core:** non è possibile avviare il reindirizzamento di stdout in C:\Programmi\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="d5395-320">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="d5395-321">Messaggio di eccezione: HRESULT 0x80070005 restituito in {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="d5395-321">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="d5395-322">Non è possibile arrestare il reindirizzamento di stdout in C:\Programmi\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="d5395-322">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="d5395-323">Messaggio di eccezione: HRESULT 0x80070002 restituito in {PATH}.</span><span class="sxs-lookup"><span data-stu-id="d5395-323">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="d5395-324">Non è possibile avviare il reindirizzamento di stdout in {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="d5395-324">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d5395-325">**Registro dell'applicazione:** Avviso: Impossibile creare stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -214702489.</span><span class="sxs-lookup"><span data-stu-id="d5395-325">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="d5395-326">**Log stdout del modulo ASP.NET Core:** il file di log non viene creato.</span><span class="sxs-lookup"><span data-stu-id="d5395-326">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="d5395-327">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-327">Troubleshooting:</span></span>

* <span data-ttu-id="d5395-328">Il percorso `stdoutLogFile` specificato nell'elemento `<aspNetCore>` di *web.config* non esiste.</span><span class="sxs-lookup"><span data-stu-id="d5395-328">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="d5395-329">Per altre informazioni, vedere [Modulo ASP.NET Core: Creazione e reindirizzamento dei log](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="d5395-329">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="d5395-330">L'utente del pool di app non ha accesso in scrittura al percorso del log di stdout.</span><span class="sxs-lookup"><span data-stu-id="d5395-330">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="d5395-331">Problema generale della configurazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="d5395-331">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d5395-332">**Browser:** Errore HTTP 500.0 - Errore di caricamento gestore In-Process ANCM **--oppure--** Errore HTTP 500.30 - Errore di avvio In-Process ANCM</span><span class="sxs-lookup"><span data-stu-id="d5395-332">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="d5395-333">**Registro dell'applicazione:** Variabile</span><span class="sxs-lookup"><span data-stu-id="d5395-333">**Application Log:** Variable</span></span>

* <span data-ttu-id="d5395-334">**Log stdout del modulo ASP.NET Core:** il file di log viene creato, ma vuoto o con voci normali fino al punto di errore dell'app.</span><span class="sxs-lookup"><span data-stu-id="d5395-334">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="d5395-335">**Log di debug del modulo ASP.NET Core:** Variabile</span><span class="sxs-lookup"><span data-stu-id="d5395-335">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d5395-336">**Browser:** errore HTTP 502.5 - Errore del processo</span><span class="sxs-lookup"><span data-stu-id="d5395-336">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="d5395-337">**Registro dell'applicazione:** L'applicazione "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con radice fisica "C:\{PATH}\' ha creato un processo con la riga di comando" "C:\{PATH}\{ASSEMBLY}.{exe|dll}" ma ha subito un arresto anomalo o non ha risposto o non era in ascolto sulla porta specificata "{PORT}", ErrorCode = '{ERROR CODE}'</span><span class="sxs-lookup"><span data-stu-id="d5395-337">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="d5395-338">**Log stdout del modulo ASP.NET Core:** il file di log viene creato ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="d5395-338">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="d5395-339">Risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d5395-339">Troubleshooting:</span></span>

<span data-ttu-id="d5395-340">non è stato possibile avviare il processo, molto probabilmente a causa di un problema di configurazione dell'app o di programmazione.</span><span class="sxs-lookup"><span data-stu-id="d5395-340">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="d5395-341">Per altre informazioni, vedere i seguenti argomenti:</span><span class="sxs-lookup"><span data-stu-id="d5395-341">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>
