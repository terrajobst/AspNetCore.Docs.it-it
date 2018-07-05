---
uid: web-pages/readme/beta3
title: WebMatrix e ASP.NET Web Pages (Razor) Beta 3 Release Leggimi | Microsoft Docs
author: rick-anderson
description: WebMatrix e ASP.NET Web Pages (Razor) Beta 3 Release Leggimi
ms.author: aspnetcontent
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 16b324e555b5450fc1e05c63e7e19985a2d02b89
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831832"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="46c8a-103">WebMatrix e ASP.NET Web Pages (Razor) Beta 3 Release Leggimi</span><span class="sxs-lookup"><span data-stu-id="46c8a-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="46c8a-104">WebMatrix e ASP.NET Web Pages (Razor) Beta 3 Release Leggimi</span><span class="sxs-lookup"><span data-stu-id="46c8a-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="46c8a-105">9 novembre 2010</span><span class="sxs-lookup"><span data-stu-id="46c8a-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="46c8a-106">Sommario</span><span class="sxs-lookup"><span data-stu-id="46c8a-106">Contents</span></span>

- [<span data-ttu-id="46c8a-107">Panoramica</span><span class="sxs-lookup"><span data-stu-id="46c8a-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="46c8a-108">Installazione</span><span class="sxs-lookup"><span data-stu-id="46c8a-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="46c8a-109">Nuove funzionalità, le modifiche e problemi noti nella versione Beta 3</span><span class="sxs-lookup"><span data-stu-id="46c8a-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="46c8a-110">Problemi di installazione di WebMatrix</span><span class="sxs-lookup"><span data-stu-id="46c8a-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="46c8a-111">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="46c8a-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="46c8a-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="46c8a-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="46c8a-113">Installazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="46c8a-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="46c8a-114">Pubblicazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="46c8a-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="46c8a-115">Altri problemi</span><span class="sxs-lookup"><span data-stu-id="46c8a-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="46c8a-116">Per altre informazioni</span><span class="sxs-lookup"><span data-stu-id="46c8a-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="46c8a-117">Panoramica</span><span class="sxs-lookup"><span data-stu-id="46c8a-117">Overview</span></span>

> <span data-ttu-id="46c8a-118">Microsoft WebMatrix Beta è uno stack di sviluppo web gratuito che consente di installare in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="46c8a-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="46c8a-119">Si integra un server web con il database e Framework per creare un'esperienza unica e integrata di programmazione.</span><span class="sxs-lookup"><span data-stu-id="46c8a-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="46c8a-120">È possibile usare Beta di WebMatrix per ottimizzare le attività di codice, testare e pubblicare il proprio sito Web ASP.NET o PHP oppure è possibile usare una versione Beta di WebMatrix per avviare un nuovo sito Web con più comuni App open source quali DotNetNuke, Umbraco, WordPress o Joomla.</span><span class="sxs-lookup"><span data-stu-id="46c8a-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="46c8a-121">Versione Beta di WebMatrix utilizza sullo stesso server web potente, motore di database e ambiente di Framework che verrà eseguito il sito Web su internet, che semplifica il passaggio dallo sviluppo alla produzione diretto e immediato.</span><span class="sxs-lookup"><span data-stu-id="46c8a-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="46c8a-122">Installazione</span><span class="sxs-lookup"><span data-stu-id="46c8a-122">Installation</span></span>

> <span data-ttu-id="46c8a-123">Per installare WebMatrix Beta 3, usare [programma di installazione di piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="46c8a-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="46c8a-124">Dopo aver installato l'installazione guidata piattaforma Web, è possibile usarlo per installare WebMatrix Beta 3.</span><span class="sxs-lookup"><span data-stu-id="46c8a-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="46c8a-125">Se si verificano problemi durante l'installazione, fare riferimento a [risoluzione dei problemi con Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="46c8a-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="46c8a-126">Istruzioni per la pubblicazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="46c8a-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="46c8a-127">Vedere [istruzioni dettagliate per la pubblicazione di applicazioni](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="46c8a-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="46c8a-128">Nuove funzionalità, le modifiche, andKnown problemi</span><span class="sxs-lookup"><span data-stu-id="46c8a-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="46c8a-129">Installazione della versione Beta di WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="46c8a-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="46c8a-130">Problema: WebMatrix Beta 3 è disponibile solo sulle piattaforme che supportano Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="46c8a-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="46c8a-131">.NET Framework versione 4 è obbligatorio per la versione Beta di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="46c8a-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="46c8a-132">In alcuni casi, il programma di installazione di WebMatrix Beta consente di provare a installare su una piattaforma che non fa parte del set di configurazione supportata.</span><span class="sxs-lookup"><span data-stu-id="46c8a-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="46c8a-133">In particolare, Windows Vista senza l'aggiornamento SP1 consentirà di avviare l'installazione della versione Beta di WebMatrix, ma il componente di .NET Framework 4 avrà esito negativo e bloccare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="46c8a-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="46c8a-134">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-134">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-135">Installare in una piattaforma supportata, che include:</span><span class="sxs-lookup"><span data-stu-id="46c8a-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="46c8a-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="46c8a-136">Windows 7</span></span>
> - <span data-ttu-id="46c8a-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="46c8a-137">Windows Server 2008</span></span>
> - <span data-ttu-id="46c8a-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="46c8a-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="46c8a-139">Windows Vista SP1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="46c8a-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="46c8a-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="46c8a-140">Windows XP SP3</span></span>
> - <span data-ttu-id="46c8a-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="46c8a-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="46c8a-142">Problema: Non è possibile installare WebMatrix Beta 3 se Microsoft Visual Studio 2008 viene installato senza Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="46c8a-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="46c8a-143">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-143">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-144">Installare [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) dall'area download Microsoft.</span><span class="sxs-lookup"><span data-stu-id="46c8a-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="46c8a-145">Problema: Alcuni assembly per SQL Server Compact 4.0 non sono installati nella Global Assembly Cache</span><span class="sxs-lookup"><span data-stu-id="46c8a-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="46c8a-146">Gli assembly gestiti per SQL Server Compact 4.0 non vengono inseriti in global assembly cache (GAC) quando si installa SQL Server Compact 4.0 in un computer a 64 bit e il computer ha solo .NET Framework 3.5 SP1 Client installato il profilo.</span><span class="sxs-lookup"><span data-stu-id="46c8a-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="46c8a-147">Gli assembly gestiti che non sono installati nella Global Assembly Cache sono:</span><span class="sxs-lookup"><span data-stu-id="46c8a-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="46c8a-148">*SqlServerCe. dll* (provider ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="46c8a-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="46c8a-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="46c8a-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="46c8a-150">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-150">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-151">Disinstallare SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="46c8a-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="46c8a-152">Scaricare e installare la versione completa di .NET Framework 3.5 SP1 dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="46c8a-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="46c8a-153">Microsoft .NET Framework 3.5 Service pack 1 (pacchetto completo)</span><span class="sxs-lookup"><span data-stu-id="46c8a-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="46c8a-154">Reinstallare SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="46c8a-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="46c8a-155">Problema: Non è possibile disinstallare SQL Server Compact mediante la riga di comando</span><span class="sxs-lookup"><span data-stu-id="46c8a-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="46c8a-156">La disinstallazione di SQL Server Compact mediante le opzioni della riga di comando non funziona in questa versione.</span><span class="sxs-lookup"><span data-stu-id="46c8a-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="46c8a-157">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-157">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-158">Uso *programmi e funzionalità* nel Pannello di controllo di Windows per disinstallare Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="46c8a-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="46c8a-159">Pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="46c8a-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="46c8a-160">In questa sezione del documento vengono descritte le nuove funzionalità, le modifiche e problemi noti con la versione Beta 3 di ASP.NET Web Pages con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="46c8a-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="46c8a-161">Nuove funzionalità</span><span class="sxs-lookup"><span data-stu-id="46c8a-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="46c8a-162">Modifiche</span><span class="sxs-lookup"><span data-stu-id="46c8a-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="46c8a-163">Problemi</span><span class="sxs-lookup"><span data-stu-id="46c8a-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="46c8a-164">Nuove funzionalità in versione Beta 3 per ASP.NET Web Pages con sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="46c8a-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="46c8a-165">Novità: Il metodo "HTML. RAW" esegue il rendering di markup non codificato</span><span class="sxs-lookup"><span data-stu-id="46c8a-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="46c8a-166">Il nuovo `Html.Raw` metodo consente di eseguire il rendering di markup HTML come markup anziché il rendering di output codificata.</span><span class="sxs-lookup"><span data-stu-id="46c8a-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="46c8a-167">(Per impostazione predefinita, ASP.NET Razor consente di codificare le stringhe prima eseguito il rendering.) La sintassi è:</span><span class="sxs-lookup"><span data-stu-id="46c8a-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="46c8a-168">Nell'esempio riportato di seguito viene illustrato come usare `Html.Raw`:</span><span class="sxs-lookup"><span data-stu-id="46c8a-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="46c8a-169">Modifiche nella versione Beta 3 per ASP.NET Web Pages con sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="46c8a-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="46c8a-170">Modifica: Il metodo "HrefAttribute" rimosso</span><span class="sxs-lookup"><span data-stu-id="46c8a-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="46c8a-171">Il `HrefAttribute` metodo di `WebPage` classe è stata rimossa.</span><span class="sxs-lookup"><span data-stu-id="46c8a-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="46c8a-172">Questo supporto è stato usato per codificare i caratteri non sicuri nell'URL.</span><span class="sxs-lookup"><span data-stu-id="46c8a-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="46c8a-173">Non è più necessario perché ASP.NET Razor codifica automaticamente stringhe.</span><span class="sxs-lookup"><span data-stu-id="46c8a-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="46c8a-174">(Usare il nuovo `Html.Raw` metodo per eseguire il rendering delle stringhe non codificate.)</span><span class="sxs-lookup"><span data-stu-id="46c8a-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="46c8a-175">Modifica: Sintassi dichiarativa "@helper" helper modificato</span><span class="sxs-lookup"><span data-stu-id="46c8a-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="46c8a-176">Nella versione Beta 3, ASP.NET viene modificato come analizza gli helper che vengono creati utilizzando il `@helper` sintassi.</span><span class="sxs-lookup"><span data-stu-id="46c8a-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="46c8a-177">In sostanza, il `@helper` sintassi ora viene analizzata come un blocco di codice invece che come blocco di markup che può includere il codice.</span><span class="sxs-lookup"><span data-stu-id="46c8a-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="46c8a-178">Pertanto, il codice all'interno dell'helper non dovrà essere racchiuso tra parentesi `@{ }` blocchi.</span><span class="sxs-lookup"><span data-stu-id="46c8a-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="46c8a-179">Al contrario, markup all'interno dell'helper deve essere incluso in modo esplicito gli elementi HTML o ASP.NET Razor `<text></text>` tag.</span><span class="sxs-lookup"><span data-stu-id="46c8a-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="46c8a-180">Ad esempio, il seguente `@helper` sintassi funziona in versione Beta 3:</span><span class="sxs-lookup"><span data-stu-id="46c8a-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="46c8a-181">Nella versione Beta 3, questo supporto deve essere modificato per essere simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="46c8a-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="46c8a-182">Si noti che il `@{ }` caratteri intorno al codice iniziale nell'helper non viene più utilizzata.</span><span class="sxs-lookup"><span data-stu-id="46c8a-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="46c8a-183">Questo avviene perché il contenuto degli helper viene considerato come un blocco di codice per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="46c8a-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="46c8a-184">L'helper di rendering del markup, che inizia con l'apertura `<a>` tag.</span><span class="sxs-lookup"><span data-stu-id="46c8a-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="46c8a-185">Se è necessario eseguire il rendering dell'helper tag che non includono un tag di chiusura o testo normale (ad esempio, `<meta>` tag), il rendering del contenuto deve essere `<text></text>` tag.</span><span class="sxs-lookup"><span data-stu-id="46c8a-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="46c8a-186">Change: "WebPageContext.HttpContext" removed</span><span class="sxs-lookup"><span data-stu-id="46c8a-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="46c8a-187">Il `WebPageContext.HttpContext` proprietà è stata rimossa.</span><span class="sxs-lookup"><span data-stu-id="46c8a-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="46c8a-188">In alternativa, usare `HttpContext.Current`.</span><span class="sxs-lookup"><span data-stu-id="46c8a-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="46c8a-189">(Il `WebPageContext.HttpContext` proprietà semplicemente eseguito il wrapping in questo.)</span><span class="sxs-lookup"><span data-stu-id="46c8a-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="46c8a-190">Modifica: Helper "Facebook" spostato al nuovo pacchetto</span><span class="sxs-lookup"><span data-stu-id="46c8a-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="46c8a-191">Il `Facebook` helper è stata spostata nel *Facebook.Helper* libreria, che include il `Facebook` helper e funzionalità aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="46c8a-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="46c8a-192">È necessario installare questa libreria come pacchetto separato, come descritto in "Installare helper con Gestione pacchetti" nell'esercitazione [Introduzione alle pagine ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="46c8a-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="46c8a-193">Modifica: L'appartenenza al ruolo e sicurezza tipi passa al nuovo assembly</span><span class="sxs-lookup"><span data-stu-id="46c8a-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="46c8a-194">I tipi seguenti sono stati spostati le `WebMatrix.WebData` assembly:</span><span class="sxs-lookup"><span data-stu-id="46c8a-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="46c8a-195">Modifica: Classe "TagBuilder" Sposta System.Web.WebPages.dll assembly</span><span class="sxs-lookup"><span data-stu-id="46c8a-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="46c8a-196">Il `TagBuilde` classe r è state spostate in assembly System.Web.WebPages.dll.</span><span class="sxs-lookup"><span data-stu-id="46c8a-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="46c8a-197">In precedenza, questo è stato in un assembly che fa parte di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="46c8a-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="46c8a-198">Questa modifica significa che è necessario non installare ASP.NET MVC per usare il `TagBuilder` classe.</span><span class="sxs-lookup"><span data-stu-id="46c8a-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="46c8a-199">Tuttavia, la classe è ancora nel `System.Web.Mvc` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="46c8a-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="46c8a-200">Per usare la `TagBuilder` classe (ad esempio, in un helper ASP.NET Razor personalizzato), è necessario fare riferimento a spazio dei nomi (ad esempio, aggiungendo `@using System.Web.Mvc` al codice).</span><span class="sxs-lookup"><span data-stu-id="46c8a-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="46c8a-201">Modifica: Stata modificata; la sintassi di convalida richiesta Classe di "Convalida" rimosso</span><span class="sxs-lookup"><span data-stu-id="46c8a-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="46c8a-202">Nella versione Beta 3, per disabilitare la convalida per un singolo campo o un set di campi, è possibile chiamare il `Validation.Exclude` , passando il nome o nomi dei campi da escludere dalla convalida.</span><span class="sxs-lookup"><span data-stu-id="46c8a-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="46c8a-203">Una nuova sintassi è disponibile nella versione Beta 3 per la convalida verrà ignorata.</span><span class="sxs-lookup"><span data-stu-id="46c8a-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="46c8a-204">Il `Validation` metodo utilizzato nella versione Beta 3 è stata rimossa.</span><span class="sxs-lookup"><span data-stu-id="46c8a-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="46c8a-205">Se non si disabilita la convalida della richiesta, se gli utenti provano a caricare il markup HTML (ad esempio, usando un editor di testo RTF su una pagina), il sito Web verrà segnalato un errore simile *è stato rilevato un valore Request. Form potenzialmente pericoloso dal client*e non è stato accettato l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="46c8a-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="46c8a-206">Se si disabilita la convalida della richiesta, è necessario archiviare manualmente input dell'utente per assicurarsi che non contengono il markup potenzialmente pericoloso o creare script usando uno strumento come il [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="46c8a-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="46c8a-207">Per disabilitare la convalida delle richieste automatica, chiamare il `Request.Unvalidated` passandogli il nome del campo o un altro oggetto post che si desidera ignorare la convalida delle richieste per.</span><span class="sxs-lookup"><span data-stu-id="46c8a-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="46c8a-208">È possibile usare questo metodo per ignorare la convalida per tutti gli elementi nel `Form`, `QueryString`, `Cookies`, e `ServerVariables` raccolte.</span><span class="sxs-lookup"><span data-stu-id="46c8a-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="46c8a-209">Gli esempi seguenti illustrano come usare il `Unvalidated` metodo:</span><span class="sxs-lookup"><span data-stu-id="46c8a-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="46c8a-210">Problemi noti per ASP.NET Web Pages con sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="46c8a-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="46c8a-211">Problema: Un comportamento imprevisto quando si usa una tabella utente personalizzato per l'appartenenza</span><span class="sxs-lookup"><span data-stu-id="46c8a-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="46c8a-212">Per inizializzare il provider di appartenenze per un sito Web ASP.NET Razor, si chiama il `WebSecurity.InitializeDatabaseConnection` (metodo).</span><span class="sxs-lookup"><span data-stu-id="46c8a-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="46c8a-213">(In WebMatrix, il modello Starter Site include una chiamata al metodo nel  *\_AppStart.cshtml* file.) Se il `autoCreateTables` parametro di questo metodo è impostato su true (per impostazione predefinita, viene impostata su true nel modello Starter Site), e se un nome di tabella non riconosciuto viene passato al metodo (il secondo parametro), il metodo non genera un errore.</span><span class="sxs-lookup"><span data-stu-id="46c8a-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="46c8a-214">Al contrario, viene creato automaticamente nella tabella.</span><span class="sxs-lookup"><span data-stu-id="46c8a-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="46c8a-215">Può trattarsi di un problema, se si prevede di usare una tabella utente personalizzata per l'appartenenza, ma passare il nome della tabella non corretto per il `WebSecurity.InitializeDatabaseConnection` (metodo).</span><span class="sxs-lookup"><span data-stu-id="46c8a-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="46c8a-216">Poiché il metodo non per impostazione predefinita genera un errore se la tabella specificata non esiste e perché crea invece una nuova tabella, è possibile l'applicazione sembra funzionare.</span><span class="sxs-lookup"><span data-stu-id="46c8a-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="46c8a-217">Tuttavia, il codice dell'applicazione che si basa sulla tabella utente personalizzata (e sui campi in essa) può avere esito negativo con errori imprevisti.</span><span class="sxs-lookup"><span data-stu-id="46c8a-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="46c8a-218">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-218">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-219">Assicurarsi che il nome passato il `InitializeDatabaseConnection` corrispondenze di metodo del profilo utente di tabella nel database di appartenenza o assicurarsi che il `autoCreateTables` parametro è impostato su false.</span><span class="sxs-lookup"><span data-stu-id="46c8a-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="46c8a-220">Problema: errore "Non riuscita a generare un'istanza utente di SQL Server"</span><span class="sxs-lookup"><span data-stu-id="46c8a-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="46c8a-221">Se un'applicazione Web di WebMatrix utilizza SQL Server Express ed è in esecuzione IIS 7.5 in Windows 7 o Windows Server 2008 R2, si potrebbe essere visualizzato un errore che indica che SQL Server non è possibile recuperare il percorso di applicazioni locali dell'utente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="46c8a-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="46c8a-222">**Soluzione alternativa** assicurarsi che l'account di Windows che l'applicazione viene eseguita con (in genere un servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio *App\_Data*.</span><span class="sxs-lookup"><span data-stu-id="46c8a-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="46c8a-223">Informazioni più dettagliate sono disponibili nell'articolo della Knowledge Base [i problemi delle istanze utente di SQL Server Express e ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="46c8a-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="46c8a-224">Problema: In Visual Studio, gli spazi dei nomi per gli assembly personalizzati (DLL) non vengono importati automaticamente</span><span class="sxs-lookup"><span data-stu-id="46c8a-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="46c8a-225">Se si usano gli assembly personalizzati in un progetto in Visual Studio, gli spazi dei nomi dichiarati in tali assembly non vengono importati automaticamente in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="46c8a-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="46c8a-226">Di conseguenza, i riferimenti a tipi personalizzati potrebbero non essere riconosciuti in fase di progettazione e sono contrassegnati come non riconosciuti in Visual Studio (con una sottolineatura di"").</span><span class="sxs-lookup"><span data-stu-id="46c8a-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="46c8a-227">Questo problema si verifica solo in fase di progettazione in Visual Studio. l'applicazione venga eseguita correttamente.</span><span class="sxs-lookup"><span data-stu-id="46c8a-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="46c8a-228">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-228">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-229">Includere un `using` istruzione (`imports` in Visual Basic) che fa riferimento l'entità che non sono riconosciute in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="46c8a-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="46c8a-230">Problema: Visual Studio IntelliSense e modelli di progetto disponibili solo in ASP.NET MVC versione 3</span><span class="sxs-lookup"><span data-stu-id="46c8a-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="46c8a-231">Installazione di ASP.NET Web Pages anche installare gli strumenti per Visual Studio, ad esempio i modelli di progetto e IntelliSense per le applicazioni ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="46c8a-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="46c8a-232">**Soluzione alternativa** per usare i modelli di progetto e IntelliSense per le applicazioni ASP.NET Web Pages in Visual Studio, installare ASP.NET MVC 3 RC tramite l'installazione guidata piattaforma Web o il [programma di installazione autonomo](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="46c8a-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="46c8a-233">Problema: "&lt;helper&gt; Impossibile trovare la classe" errore</span><span class="sxs-lookup"><span data-stu-id="46c8a-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="46c8a-234">Dopo l'aggiornamento alla versione Beta 3, si potrebbe essere visualizzato un errore che una classe helper (ad esempio, il `Facebook` classe) non è stato nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="46c8a-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="46c8a-235">A partire da Beta 2 e continuando nella versione Beta 3, gli helper sono stati spostati ai pacchetti che è necessario installare in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="46c8a-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="46c8a-236">I siti esistenti non vengono aggiornati in modo da includere questi pacchetti; inclusi i siti nel *\My Documents\IISExpress* oppure *\My Documents\My Web* cartelle.</span><span class="sxs-lookup"><span data-stu-id="46c8a-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="46c8a-237">In particolare, verrà visualizzato questo errore se si usa il sito predefinito in *siti personali* (WebSite1), che include un riferimento al `Twitter` helper.</span><span class="sxs-lookup"><span data-stu-id="46c8a-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="46c8a-238">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-238">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-239">Rimuovere i commenti per le chiamate a tutti gli helper del sito, eseguire la  *\_Admin* pagina e installare i pacchetti che includono gli helper che si desidera utilizzare.</span><span class="sxs-lookup"><span data-stu-id="46c8a-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="46c8a-240">Dopo aver installato il pacchetto, è possibile rimuovere il commento le righe che fanno riferimento a funzioni di supporto.</span><span class="sxs-lookup"><span data-stu-id="46c8a-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="46c8a-241">Problema: La distribuzione di Beta 3 ASP.NET Razor assembly nella cartella Bin potrebbe non funzionare in siti di hosting</span><span class="sxs-lookup"><span data-stu-id="46c8a-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="46c8a-242">Se si distribuisce un sito Web di ASP.NET Web Pages in un sito di hosting, e se si distribuiscono gli assembly di ASP.NET Razor Beta 3 per il sito *Bin* cartella, si potrebbero riscontrare gli errori, inclusi i seguenti:</span><span class="sxs-lookup"><span data-stu-id="46c8a-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="46c8a-243">Questa situazione può verificarsi se il provider di hosting ha installato l'assembly di ASP.NET Web Pages versione Beta 1 nella cache di applicazione globale del server (GAC).</span><span class="sxs-lookup"><span data-stu-id="46c8a-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="46c8a-244">Gli assembly nella Global Assembly Cache ottengono la precedenza su assembly installati in locale nel *Bin* cartella.</span><span class="sxs-lookup"><span data-stu-id="46c8a-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="46c8a-245">**Soluzione alternativa** contattare il provider di hosting per confermare che gli errori vengono visualizzati sono a causa di un conflitto tra le versioni del provider degli assembly e quelle in uso.</span><span class="sxs-lookup"><span data-stu-id="46c8a-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="46c8a-246">In questo caso, chiedere al provider di hosting di aggiornare gli assembly nella Global Assembly Cache del server.</span><span class="sxs-lookup"><span data-stu-id="46c8a-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="46c8a-247">Problema: I feed di lettura o altri dati esterni tramite un server proxy</span><span class="sxs-lookup"><span data-stu-id="46c8a-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="46c8a-248">Se il server che esegue il sito si trova dietro un server proxy, si potrebbe essere necessario configurare le informazioni sul proxy nel *Web. config* file per poter essere in grado di leggere le informazioni provenienti dall'esterno del sito.</span><span class="sxs-lookup"><span data-stu-id="46c8a-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="46c8a-249">Ad esempio, se si usa il `ReCaptcha` helper, l'helper comunica con il servizio reCAPTCHA, ma potrebbe essere bloccato dal server proxy.</span><span class="sxs-lookup"><span data-stu-id="46c8a-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="46c8a-250">Feed utilizzati nelle pagine Web ASP.NET, ad esempio il feed usato da Gestione pacchetti in modo analogo, potrebbero richiedere la configurazione del proxy.</span><span class="sxs-lookup"><span data-stu-id="46c8a-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="46c8a-251">Se si verificano problemi nell'uso di un servizio esterno o l'utilizzo di feed di pacchetti, inserire gli elementi seguenti nella radice dell'applicazione *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="46c8a-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="46c8a-252">Per altre informazioni sulla configurazione di un server proxy, vedere [ &lt;proxy&gt; (impostazioni di rete)](https://msdn.microsoft.com/library/sa91de1e.aspx) sul sito Web MSDN.</span><span class="sxs-lookup"><span data-stu-id="46c8a-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="46c8a-253">Problema: Errore "Impossibile caricare Microsoft.Web.Infrastructure.dll"</span><span class="sxs-lookup"><span data-stu-id="46c8a-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="46c8a-254">Se già installata la versione Beta 1 di ASP.NET Web Pages con sintassi Razor e quindi installare la versione Beta 3, tutti gli assembly appropriati sono installati nella Global Assembly Cache, ad eccezione *Microsoft.Web.Infrastructure.dll*.</span><span class="sxs-lookup"><span data-stu-id="46c8a-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="46c8a-255">Di conseguenza, quando si esegue ASP.NET Razor pages, viene visualizzato un errore che indica che *Microsoft.Web.Infrastructure.dll* non può essere caricato.</span><span class="sxs-lookup"><span data-stu-id="46c8a-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="46c8a-256">Questo problema si verifica se è stato caricato la versione Beta 3 in un computer pulito.</span><span class="sxs-lookup"><span data-stu-id="46c8a-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="46c8a-257">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-257">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-258">Nel Pannello di controllo disinstallare ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="46c8a-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="46c8a-259">Reinstallare la versione Beta 3.</span><span class="sxs-lookup"><span data-stu-id="46c8a-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="46c8a-260">Problema: Disinstallazione di .NET Framework versione 4 Disabilita ASP.NET Web Pages con sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="46c8a-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="46c8a-261">Se si disinstalla .NET Framework versione 4 e quindi reinstallarlo, ASP.NET Web Pages con sintassi Razor è disabilitato.</span><span class="sxs-lookup"><span data-stu-id="46c8a-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="46c8a-262">Le pagine con le *cshtml* estensioni non vengono eseguite correttamente.</span><span class="sxs-lookup"><span data-stu-id="46c8a-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="46c8a-263">ASP.NET Web Pages registra un assembly nella directory radice macchina *Web. config* file e rimozione di .NET Framework consente di rimuovere tale file.</span><span class="sxs-lookup"><span data-stu-id="46c8a-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="46c8a-264">Reinstallare .NET Framework viene installata una versione nuova del file di configurazione, ma non aggiunge il riferimento per l'assembly di ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="46c8a-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="46c8a-265">**Soluzione alternativa** dopo la reinstallazione di .NET Framework, reinstallare ASP.NET Web Pages con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="46c8a-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="46c8a-266">Verrà aggiunto l'elemento seguente per il *Web. config* file nella radice del computer, ovvero in genere nel percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="46c8a-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="46c8a-267">Problema: Le applicazioni distribuite in precedenza con ASP.NET gli assembly nella cartella Bin del verificano di errori</span><span class="sxs-lookup"><span data-stu-id="46c8a-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="46c8a-268">Durante la distribuzione, copie degli assembly di ASP.NET Web Pages (ad esempio, *Microsoft.WebPages.dll*) per il *Bin* cartella del sito Web nel server.</span><span class="sxs-lookup"><span data-stu-id="46c8a-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="46c8a-269">(Ciò si sono verificati automaticamente durante la distribuzione o perché lo sviluppatore copiati in modo esplicito gli assembly.) Tuttavia, quando viene installata la versione Beta 3, gli errori si verifica, ad esempio gli errori che non è possibile trovare alcuni tipi.</span><span class="sxs-lookup"><span data-stu-id="46c8a-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="46c8a-270">Ciò si verifica perché un numero di tipi di pagine Web ASP.NET sono stato spostato in diversi spazi dei nomi per la versione Beta 3.</span><span class="sxs-lookup"><span data-stu-id="46c8a-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="46c8a-271">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="46c8a-271">**Workaround** </span></span>  
> <span data-ttu-id="46c8a-272">Cancella il *Bin* cartella dell'applicazione distribuita, copiare i nuovi assembly nella cartella (o ridistribuire l'applicazione), quindi riavviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="46c8a-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="46c8a-273">Problema: Gli URL senza estensioni non si trova il file.cshtml/.vbhtml in IIS 7 o IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="46c8a-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="46c8a-274">In IIS 7 o IIS 7.5, le richieste con un URL analogo al seguente non sono in grado di trovare le pagine che hanno le *. cshtml* oppure *vbhtml* estensione:</span><span class="sxs-lookup"><span data-stu-id="46c8a-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="46c8a-275">Il problema si verifica perché la riscrittura degli URL non è abilitato per impostazione predefinita per IIS 7 o IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="46c8a-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="46c8a-276">Lo scenario probabile che è che il problema non viene visualizzato durante il test in locale tramite IIS Express, ma si verificano quando si distribuisce il sito Web in un sito Web di hosting.</span><span class="sxs-lookup"><span data-stu-id="46c8a-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="46c8a-277">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="46c8a-278">Se si dispone di controllo sul computer del server, nel computer del server, installare l'aggiornamento descritto nel [è disponibile che consente di determinati gestori di IIS 7.0 o IIS 7.5 di gestire le richieste il cui URL non terminano con un periodo di un aggiornamento](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="46c8a-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="46c8a-279">Se non è un controllo sul computer server (ad esempio, si distribuisce in un sito Web di hosting), aggiungere il codice seguente nel sito Web *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="46c8a-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="46c8a-280">Problema: Utilizzando le pagine Web ASP.NET e ASP.NET MVC o Web Application Project nella stessa applicazione</span><span class="sxs-lookup"><span data-stu-id="46c8a-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="46c8a-281">Se si usa ASP.NET Web Pages in un'applicazione ASP.NET MVC o il progetto di applicazione Web, si potrebbe essere visualizzato un errore che *WebPageHttpApplication* nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="46c8a-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="46c8a-282">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-282">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-283">Se si verifica questo errore, modificare la classe di base da cui deriva l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="46c8a-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="46c8a-284">Nel *Global. asax* file, modificare la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="46c8a-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="46c8a-285">A questo scopo:</span><span class="sxs-lookup"><span data-stu-id="46c8a-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="46c8a-286">Ciò in effetti Annulla una modifica che è stata introdotta per la versione Beta 1 di ASP.NET Web Pages con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="46c8a-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="46c8a-287">Problema: Distribuire un'applicazione in un computer che non dispone di SQL Server Compact è installato</span><span class="sxs-lookup"><span data-stu-id="46c8a-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="46c8a-288">Le applicazioni che includono i database di SQL Server Compact possono eseguire in un computer in cui SQL Server Compact non è installato.</span><span class="sxs-lookup"><span data-stu-id="46c8a-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="46c8a-289">Microsoft WebMatrix Beta 3 automaticamente consente di copiare tali file binari per l'utente ed esegue l'appropriato *Web. config* trasformazioni di file.</span><span class="sxs-lookup"><span data-stu-id="46c8a-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="46c8a-290">**Soluzione alternativa** se è necessario copiare tali file e apportare le *Web. config* modifiche al file manualmente, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="46c8a-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="46c8a-291">Copiare gli assembly del motore di database per il *Bin* cartella (e sottocartelle) dell'applicazione nel computer di destinazione:</span><span class="sxs-lookup"><span data-stu-id="46c8a-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="46c8a-292">Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **al** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="46c8a-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="46c8a-293">Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **alla** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="46c8a-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="46c8a-294">Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **alla** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="46c8a-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="46c8a-295">Nella cartella radice del sito Web, creare o aprire una *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="46c8a-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="46c8a-296">(In WebMatrix Beta 3, questo tipo di file è disponibile se si fa clic **tutte** nel **scegliere un tipo di File** nella finestra di dialogo.)</span><span class="sxs-lookup"><span data-stu-id="46c8a-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="46c8a-297">Aggiungere l'elemento seguente come elemento figlio il **&lt;configuration&gt;** elemento (non all'interno di **&lt;System. Web&gt;** elemento):</span><span class="sxs-lookup"><span data-stu-id="46c8a-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="46c8a-298">Problema: Database e WebGrid helper non funzionano in Medium Trust in Visual Basic</span><span class="sxs-lookup"><span data-stu-id="46c8a-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="46c8a-299">Se si usa Visual Basic (creando *vbhtml* file), il `Database` e `WebGrid` helper non funzionerà se l'applicazione è impostata per usare Medium Trust.</span><span class="sxs-lookup"><span data-stu-id="46c8a-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="46c8a-300">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-300">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-301">Impostare temporaneamente l'applicazione per usare l'attendibilità.</span><span class="sxs-lookup"><span data-stu-id="46c8a-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="46c8a-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="46c8a-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="46c8a-303">Problema: La proprietà "Encrypt" non è riconosciuta</span><span class="sxs-lookup"><span data-stu-id="46c8a-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="46c8a-304">SQL Server Compact 4.0 non riconosce i `Encrypt` proprietà del `SqlCeConnection` classe.</span><span class="sxs-lookup"><span data-stu-id="46c8a-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="46c8a-305">Utilizzare questa proprietà non crittografare i file di database.</span><span class="sxs-lookup"><span data-stu-id="46c8a-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="46c8a-306">Il `Encrypt` proprietà è stata deprecata nella versione 3.5 di SQL Server Compact ed è stata mantenuta solo per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="46c8a-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="46c8a-307">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-307">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-308">Usare la `Encryption Mode` proprietà del `SqlCeConnection` classe per crittografare i file di database di SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="46c8a-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="46c8a-309">Nell'esempio seguente viene illustrato come creare un database di SQL Server Compact 4.0 crittografate tramite il `Encryption Mode` proprietà:</span><span class="sxs-lookup"><span data-stu-id="46c8a-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="46c8a-310">Per modificare la modalità di crittografia di un database di SQL Server Compact 4.0 esistente, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="46c8a-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="46c8a-311">Per crittografare un database di SQL Server Compact 4.0 non crittografato, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="46c8a-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="46c8a-312">Problema: Sono necessarie le librerie di runtime di Microsoft Visual C++ 2008</span><span class="sxs-lookup"><span data-stu-id="46c8a-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="46c8a-313">Il nativa le DLL di SQL Server Compact 4.0 necessario di Microsoft Visual C++ 2008 librerie di Runtime (x86, IA64 e x64), Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="46c8a-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="46c8a-314">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-314">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-315">Installare .NET Framework 3.5 SP1.</span><span class="sxs-lookup"><span data-stu-id="46c8a-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="46c8a-316">Viene inoltre installato Visual C++ 2008 Runtime librerie SP1.</span><span class="sxs-lookup"><span data-stu-id="46c8a-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="46c8a-317">È possibile scaricare le librerie dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="46c8a-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="46c8a-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span><span class="sxs-lookup"><span data-stu-id="46c8a-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="46c8a-319">Si noti che l'installazione di .NET Framework 2.0, 3.0, o 4 li *non* installare Visual C++ 2008 Runtime librerie SP1.</span><span class="sxs-lookup"><span data-stu-id="46c8a-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="46c8a-320">Problema: Se è installato SQL Server Compact prima di installare .NET Framework nel computer, il nome invariante del provider non è registrato nel file Machine. config di .NET Framework</span><span class="sxs-lookup"><span data-stu-id="46c8a-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="46c8a-321">SQL Server Compact può essere installato in un computer che non dispone di .NET Framework installata perché SQL Server Compact richiede .NET framework.</span><span class="sxs-lookup"><span data-stu-id="46c8a-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="46c8a-322">Se .NET Framework versione 3.5 né 4 è installato prima di installare SQL Server Compact, l'installazione di SQL Server Compact non registra il nome invariante del provider nel *Machine. config* file.</span><span class="sxs-lookup"><span data-stu-id="46c8a-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="46c8a-323">Qualsiasi applicazione che si basa sulla voce in SQL Server Compact il *Machine. config* file avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="46c8a-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="46c8a-324">La voce di registrazione nome invariante nelle *Machine. config* aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="46c8a-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="46c8a-325">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-325">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-326">Disinstallare SQL Server Compact 4.0 CTP1.</span><span class="sxs-lookup"><span data-stu-id="46c8a-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="46c8a-327">Scaricare e installare le versioni complete di .NET Framework dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="46c8a-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="46c8a-328">Microsoft .NET Framework 3.5 Service pack 1 (pacchetto completo)</span><span class="sxs-lookup"><span data-stu-id="46c8a-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="46c8a-329">Versione di Microsoft .NET Framework 4.0 (pacchetto completo)</span><span class="sxs-lookup"><span data-stu-id="46c8a-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="46c8a-330">Quindi si reinstalla [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="46c8a-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="46c8a-331">Installazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="46c8a-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="46c8a-332">Problema: Installazione di un'applicazione può richiedere molto tempo se cartella documenti dell'utente viene reindirizzata a una condivisione di rete</span><span class="sxs-lookup"><span data-stu-id="46c8a-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="46c8a-333">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-333">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-334">Nessuno.</span><span class="sxs-lookup"><span data-stu-id="46c8a-334">None.</span></span> <span data-ttu-id="46c8a-335">L'applicazione potrebbe richiedere del tempo per l'installazione, ma verrà installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="46c8a-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="46c8a-336">Pubblicazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="46c8a-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="46c8a-337">Problema: Sito potrebbe non funzionare dopo la pubblicazione se il campo "URL di destinazione" non è il prefisso http:// o https://</span><span class="sxs-lookup"><span data-stu-id="46c8a-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="46c8a-338">Nel **impostazioni di pubblicazione** finestra di dialogo, se l'URL di destinazione non inizia con `http://` o `https://`, il sito potrebbe non funzionare dopo la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="46c8a-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="46c8a-339">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-339">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-340">Assicurarsi che prima di pubblicare un sito, l'URL di destinazione nel **impostazioni di pubblicazione** finestra di dialogo inizia con `http://` o `https://`.</span><span class="sxs-lookup"><span data-stu-id="46c8a-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="46c8a-341">Problema: La pubblicazione di un database MySQL non riesce con l'errore "non è stato possibile pubblicare il database.</span><span class="sxs-lookup"><span data-stu-id="46c8a-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="46c8a-342">Questa situazione può verificarsi se il database remoto non è possibile eseguire lo script."</span><span class="sxs-lookup"><span data-stu-id="46c8a-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="46c8a-343">L'errore può verificarsi per diversi motivi.</span><span class="sxs-lookup"><span data-stu-id="46c8a-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="46c8a-344">Un motivo per cui che è possibile visualizzare questo errore è se lo script di database contiene un carattere di virgoletta singola (') e set di caratteri predefinito del database MySQL di destinazione non è presente in UTF-8.</span><span class="sxs-lookup"><span data-stu-id="46c8a-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="46c8a-345">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-345">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-346">Impostare il carattere predefinito impostato per il database MySQL remoto su UTF-8.</span><span class="sxs-lookup"><span data-stu-id="46c8a-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="46c8a-347">Altri problemi</span><span class="sxs-lookup"><span data-stu-id="46c8a-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="46c8a-348">Problema: / Filtro di ricerca non funziona nei report di Group By: tipo di problema</span><span class="sxs-lookup"><span data-stu-id="46c8a-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="46c8a-349">Quando si esegue un report per un sito, se si immette testo nella *Filtra per URL* casella e fare clic su *ricerca*, non accade nulla.</span><span class="sxs-lookup"><span data-stu-id="46c8a-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="46c8a-350">Infatti, questo controllo non funziona durante la *Group By* stato del report è impostato su *tipo di problema*, ovvero l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="46c8a-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="46c8a-351">**Soluzione alternativa** nella *Group By* della scheda della barra multifunzione, fare clic su *URL* per raggruppare le voci per i rispettivi URL di origine.</span><span class="sxs-lookup"><span data-stu-id="46c8a-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="46c8a-352">La casella di testo e il pulsante per filtrare le voci sono funzionale durante questo stato.</span><span class="sxs-lookup"><span data-stu-id="46c8a-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="46c8a-353">Problema: Le applicazioni WCF non è possibile eseguire con IIS Express</span><span class="sxs-lookup"><span data-stu-id="46c8a-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="46c8a-354">Passare a un'applicazione WCF genera un errore simile a quello seguente:</span><span class="sxs-lookup"><span data-stu-id="46c8a-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="46c8a-355">*Impossibile caricare il file o l'assembly ' Microsoft.Web.Administration, versione = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una delle relative dipendenze. Il sistema non riesce a trovare il file specificato.*</span><span class="sxs-lookup"><span data-stu-id="46c8a-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="46c8a-356">Ciò si verifica perché WCF non supporta il versione Beta di IIS Express per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="46c8a-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="46c8a-357">**Soluzione alternativa** usare una qualsiasi delle seguenti soluzioni alternative (soluzione alternativa 2 # richiede Microsoft Windows Vista o versioni successive):</span><span class="sxs-lookup"><span data-stu-id="46c8a-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="46c8a-358">Copia il *Microsoft.Web.dll* e *Administration* assembly dal percorso di installazione di WebMatrix per il *bin* directory di WCF applicazione.</span><span class="sxs-lookup"><span data-stu-id="46c8a-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="46c8a-359">Per impostazione predefinita, WebMatrix viene installato nel *Microsoft WebMatrix* sottocartella del sistema *i file di programma* cartella.</span><span class="sxs-lookup"><span data-stu-id="46c8a-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="46c8a-360">In Microsoft Windows Vista o versione successiva, creare un collegamento simbolico per gli assembly di *bin* directory usando i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="46c8a-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="46c8a-361">(Questo approccio ha il vantaggio che non crea una copia degli assembly).</span><span class="sxs-lookup"><span data-stu-id="46c8a-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="46c8a-362">Installare i due assembly nella Global Assembly Cache.</span><span class="sxs-lookup"><span data-stu-id="46c8a-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="46c8a-363">Da un prompt dei comandi con privilegi elevati, eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="46c8a-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="46c8a-364">Problema: nella versione WebMatrix Beta 3 non riesce a eseguire determinate attività che richiedono l'elevazione dei privilegi</span><span class="sxs-lookup"><span data-stu-id="46c8a-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="46c8a-365">WebMatrix Beta 3 è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi, ad esempio l'installazione di componenti aggiuntivi nelle situazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="46c8a-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="46c8a-366">In Windows Vista o Windows 7, si è connessi con un account che dispone di privilegi amministrativi e controllo Account utente (UAC) è disabilitato.</span><span class="sxs-lookup"><span data-stu-id="46c8a-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="46c8a-367">Si utilizza Microsoft Windows XP o Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="46c8a-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="46c8a-368">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-368">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-369">La maggior parte delle attività nella versione Beta 3 di WebMatrix non richiedono autorizzazioni amministrative.</span><span class="sxs-lookup"><span data-stu-id="46c8a-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="46c8a-370">Per questi, è possibile eseguire l'operazione perché un amministratore o seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="46c8a-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="46c8a-371">In Windows Vista o Windows 7, abilitare la UAC.</span><span class="sxs-lookup"><span data-stu-id="46c8a-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="46c8a-372">In Windows XP, aggiungere l'utente al gruppo di sicurezza Administrators.</span><span class="sxs-lookup"><span data-stu-id="46c8a-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="46c8a-373">Problema: "Del sito da Web Gallery" è disabilitato</span><span class="sxs-lookup"><span data-stu-id="46c8a-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="46c8a-374">Il **sito da Web Gallery** opzione è disabilitata se l'installazione guidata piattaforma Web 3.0 non è installato.</span><span class="sxs-lookup"><span data-stu-id="46c8a-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="46c8a-375">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-375">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-376">Installare il [installazione guidata piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="46c8a-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="46c8a-377">Problema: In Windows Server 2003, IIS Express non viene avviato per un utente senza privilegi di amministratore</span><span class="sxs-lookup"><span data-stu-id="46c8a-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="46c8a-378">In Windows Server 2003, quando si avvia una pagina o l'avvio di IIS Express, IIS Express non viene avviato.</span><span class="sxs-lookup"><span data-stu-id="46c8a-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="46c8a-379">Per le pagine Web, viene visualizzato un errore che indica che l'applicazione sia stata avviata da un utente senza privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="46c8a-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="46c8a-380">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-380">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-381">Avviare WebMatrix Beta 3 come un utente amministratore.</span><span class="sxs-lookup"><span data-stu-id="46c8a-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="46c8a-382">Per altre informazioni, vedere l'articolo della Knowledge Base seguente:</span><span class="sxs-lookup"><span data-stu-id="46c8a-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="46c8a-383">Può restare in attesa di un'applicazione che viene avviata da un utente senza privilegi di amministratore per il traffico HTTP del computer in cui l'applicazione è in esecuzione in Windows Vista, Windows Server 2003 o Windows XP.</span><span class="sxs-lookup"><span data-stu-id="46c8a-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="46c8a-384">Problema: Google Chrome non è disponibile come opzione di esecuzione</span><span class="sxs-lookup"><span data-stu-id="46c8a-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="46c8a-385">Google Chrome non viene visualizzata nell'elenco dei browser nel **eseguiti** nel **Home** scheda.</span><span class="sxs-lookup"><span data-stu-id="46c8a-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="46c8a-386">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-386">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-387">Alcune versioni di Google Chrome non si registrano correttamente con la funzionalità programmi predefiniti in Windows.</span><span class="sxs-lookup"><span data-stu-id="46c8a-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="46c8a-388">Risolvere il problema, avviare Google Chrome, scegliere il *personalizzare e controllo Google Chrome* menu, fare clic su *opzioni*e quindi fare clic su *marca Google Chrome il browser predefinito installato*.</span><span class="sxs-lookup"><span data-stu-id="46c8a-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="46c8a-389">Problema: La finestra di dialogo "Foreign Key" non consente l'immissione di una chiave primaria</span><span class="sxs-lookup"><span data-stu-id="46c8a-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="46c8a-390">Il **Foreign Key** nella finestra di dialogo non consente di immettere il nome della chiave primario della tabella di chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="46c8a-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="46c8a-391">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-391">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-392">Ciò è intenzionale.</span><span class="sxs-lookup"><span data-stu-id="46c8a-392">This is intentional.</span></span> <span data-ttu-id="46c8a-393">Non devi immettere il nome della chiave primaria della tabella della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="46c8a-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="46c8a-394">Problema: Il pulsante "Relazioni" è disabilitato</span><span class="sxs-lookup"><span data-stu-id="46c8a-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="46c8a-395">Il **relazioni** pulsante sotto il **tabella** scheda il **database** dell'area di lavoro è disabilitato per i database di SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="46c8a-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="46c8a-396">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-396">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-397">Nessuno.</span><span class="sxs-lookup"><span data-stu-id="46c8a-397">None.</span></span> <span data-ttu-id="46c8a-398">SQL Server Compact non supporta le relazioni tra tabelle.</span><span class="sxs-lookup"><span data-stu-id="46c8a-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="46c8a-399">Problema: Le query SQL con parametri vengono generate eccezioni</span><span class="sxs-lookup"><span data-stu-id="46c8a-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="46c8a-400">In SQL Server Compact 4.0, se non si specifica un tipo di dati, ad esempio `SqlDbType` o `DbType` per i parametri nelle query con parametri, viene generata un'eccezione quando viene eseguita la query.</span><span class="sxs-lookup"><span data-stu-id="46c8a-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="46c8a-401">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="46c8a-401">**Workaround**</span></span>  
> <span data-ttu-id="46c8a-402">Impostare in modo esplicito il tipo di dati per i parametri, ad esempio `SqlDbType` o `DbType`.</span><span class="sxs-lookup"><span data-stu-id="46c8a-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="46c8a-403">Questo aspetto è fondamentale nel caso di tipi di dati BLOB (`image` e `ntext`).</span><span class="sxs-lookup"><span data-stu-id="46c8a-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="46c8a-404">Usare codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="46c8a-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="46c8a-405">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="46c8a-405">For More Information</span></span>

<span data-ttu-id="46c8a-406">Per ulteriori informazioni su WebMatrix Beta 3, vedere i siti Web seguenti:</span><span class="sxs-lookup"><span data-stu-id="46c8a-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="46c8a-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="46c8a-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="46c8a-408">ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="46c8a-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="46c8a-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="46c8a-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="46c8a-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="46c8a-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="46c8a-411">Tutti i diritti riservati.</span><span class="sxs-lookup"><span data-stu-id="46c8a-411">All Rights Reserved.</span></span> <span data-ttu-id="46c8a-412">[Le condizioni d'uso](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="46c8a-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
