---
uid: web-pages/readme/overview
title: File Leggimi di WebMatrix | Microsoft Docs
author: rick-anderson
description: WebMatrix e ASP.NET Web Pages (Razor) versione 1.0 Leggimi
ms.author: aspnetcontent
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 8aa0eedb3156ca33905c106b72d5fa553a917b16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822862"
---
<a name="webmatrix-readme"></a><span data-ttu-id="0c9da-103">File Leggimi di WebMatrix</span><span class="sxs-lookup"><span data-stu-id="0c9da-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="0c9da-104">13 gennaio 2011</span><span class="sxs-lookup"><span data-stu-id="0c9da-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="0c9da-105">Sommario</span><span class="sxs-lookup"><span data-stu-id="0c9da-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="0c9da-106">Si applica questo file Leggimi per la 1.0 versione di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="0c9da-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="0c9da-107">Panoramica</span><span class="sxs-lookup"><span data-stu-id="0c9da-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="0c9da-108">Installazione</span><span class="sxs-lookup"><span data-stu-id="0c9da-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="0c9da-109">Come pubblicare applicazioni</span><span class="sxs-lookup"><span data-stu-id="0c9da-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="0c9da-110">Le modifiche e problemi</span><span class="sxs-lookup"><span data-stu-id="0c9da-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="0c9da-111">Installazione di WebMatrix 1.0</span><span class="sxs-lookup"><span data-stu-id="0c9da-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="0c9da-112">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="0c9da-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="0c9da-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="0c9da-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="0c9da-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="0c9da-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="0c9da-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="0c9da-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="0c9da-116">Installazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="0c9da-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="0c9da-117">Pubblicazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="0c9da-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="0c9da-118">Per altre informazioni</span><span class="sxs-lookup"><span data-stu-id="0c9da-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="0c9da-119">Panoramica</span><span class="sxs-lookup"><span data-stu-id="0c9da-119">Overview</span></span>

> <span data-ttu-id="0c9da-120">Microsoft WebMatrix 1.0 è uno stack di sviluppo web gratuito che consente di installare in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="0c9da-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="0c9da-121">Si integra un server web con il database e Framework per creare un'esperienza unica e integrata di programmazione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="0c9da-122">È possibile usare WebMatrix per ottimizzare le attività di codice, testare e pubblicare il proprio sito Web ASP.NET o PHP, oppure è possibile usare WebMatrix per avviare un nuovo sito Web con più comuni App open source quali DotNetNuke, Umbraco, WordPress o Joomla.</span><span class="sxs-lookup"><span data-stu-id="0c9da-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="0c9da-123">WebMatrix utilizza sullo stesso server web potente, motore di database e ambiente di Framework che verrà eseguito il sito Web su internet, che semplifica il passaggio dallo sviluppo alla produzione diretto e immediato.</span><span class="sxs-lookup"><span data-stu-id="0c9da-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="0c9da-124">Installazione</span><span class="sxs-lookup"><span data-stu-id="0c9da-124">Installation</span></span>

> <span data-ttu-id="0c9da-125">Per installare WebMatrix 1.0, è necessario installare innanzitutto le [programma di installazione di piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="0c9da-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="0c9da-126">Dopo aver installato l'installazione guidata piattaforma Web, è possibile usarlo per installare WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="0c9da-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="0c9da-127">Se si verificano problemi durante l'installazione, fare riferimento a [risoluzione dei problemi con Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="0c9da-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="0c9da-128">Come pubblicare applicazioni</span><span class="sxs-lookup"><span data-stu-id="0c9da-128">How to Publish Applications</span></span>

> <span data-ttu-id="0c9da-129">Vedere [istruzioni dettagliate per la pubblicazione di applicazioni](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="0c9da-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="0c9da-130">Le modifiche e problemi</span><span class="sxs-lookup"><span data-stu-id="0c9da-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="0c9da-131">Problemi di installazione 1.0 WebMatrix</span><span class="sxs-lookup"><span data-stu-id="0c9da-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="0c9da-132">Problema: WebMatrix 1.0 è disponibile solo nelle piattaforme che supportano Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="0c9da-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="0c9da-133">.NET Framework versione 4 è obbligatorio per WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="0c9da-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="0c9da-134">In alcuni casi, il programma di installazione di WebMatrix 1.0 consente di provare a installare su una piattaforma che non fa parte del set di configurazione supportata.</span><span class="sxs-lookup"><span data-stu-id="0c9da-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="0c9da-135">In particolare, Windows Vista senza l'aggiornamento SP1 consentirà di avviare l'installazione di WebMatrix, ma il componente di .NET Framework 4 avrà esito negativo e bloccare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="0c9da-136">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-136">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-137">Installare in una piattaforma supportata, che include:</span><span class="sxs-lookup"><span data-stu-id="0c9da-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="0c9da-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="0c9da-138">Windows 7</span></span>
> - <span data-ttu-id="0c9da-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="0c9da-139">Windows Server 2008</span></span>
> - <span data-ttu-id="0c9da-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="0c9da-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="0c9da-141">Windows Vista SP1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="0c9da-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="0c9da-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="0c9da-142">Windows XP SP3</span></span>
> - <span data-ttu-id="0c9da-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="0c9da-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="0c9da-144">Problema: Non è possibile installare WebMatrix 1.0 se Microsoft Visual Studio 2008 viene installato senza Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="0c9da-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="0c9da-145">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-145">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-146">Installare [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) dall'area download Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0c9da-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="0c9da-147">Problema: Alcuni assembly per SQL Server Compact 4.0 non sono installati nella Global Assembly Cache</span><span class="sxs-lookup"><span data-stu-id="0c9da-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="0c9da-148">Gli assembly gestiti per SQL Server Compact 4.0 non vengono inseriti in global assembly cache (GAC) quando si installa SQL Server Compact 4.0 in un computer a 64 bit e il computer ha solo .NET Framework 3.5 SP1 Client installato il profilo.</span><span class="sxs-lookup"><span data-stu-id="0c9da-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="0c9da-149">Gli assembly gestiti che non sono installati nella Global Assembly Cache sono:</span><span class="sxs-lookup"><span data-stu-id="0c9da-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="0c9da-150">*SqlServerCe. dll* (provider ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="0c9da-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="0c9da-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="0c9da-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="0c9da-152">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-152">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-153">Disinstallare SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="0c9da-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="0c9da-154">Scaricare e installare la versione completa di .NET Framework 3.5 SP1 dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="0c9da-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="0c9da-155">Microsoft .NET Framework 3.5 Service pack 1 (pacchetto completo)</span><span class="sxs-lookup"><span data-stu-id="0c9da-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="0c9da-156">Reinstallare SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="0c9da-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="0c9da-157">Problema: Non è possibile disinstallare SQL Server Compact mediante la riga di comando</span><span class="sxs-lookup"><span data-stu-id="0c9da-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="0c9da-158">La disinstallazione di SQL Server Compact mediante le opzioni della riga di comando non funziona in questa versione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="0c9da-159">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-159">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-160">Uso *programmi e funzionalità* nel Pannello di controllo di Windows per disinstallare Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="0c9da-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="0c9da-161">Pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0c9da-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="0c9da-162">In questa sezione del documento vengono descritte le nuove funzionalità, le modifiche e problemi noti con la 1.0 versione di ASP.NET Web Pages con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="0c9da-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="0c9da-163">Nuove funzionalità</span><span class="sxs-lookup"><span data-stu-id="0c9da-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="0c9da-164">Modifiche</span><span class="sxs-lookup"><span data-stu-id="0c9da-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="0c9da-165">Problemi</span><span class="sxs-lookup"><span data-stu-id="0c9da-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a>  <span data-ttu-id="0c9da-166">Nuove funzionalità</span><span class="sxs-lookup"><span data-stu-id="0c9da-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="0c9da-167">Novità: Impostazione di configurazione aggiunte per disabilitare la gestione dei pacchetti</span><span class="sxs-lookup"><span data-stu-id="0c9da-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="0c9da-168">Una nuova `asp:AdminManagerEnabled` chiave è disponibile per il `<appSettings>` elemento le *Web. config* file, che consente di disabilitare completamente la gestione dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="0c9da-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="0c9da-169">Il valore predefinito per questo elemento è impostato su true, il che significa che se non è incluso nel *Web. config* file, la gestione dei pacchetti è abilitato.</span><span class="sxs-lookup"><span data-stu-id="0c9da-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="0c9da-170">Per disabilitare la gestione dei pacchetti, aggiungere il seguente elemento per il *Web. config* file nella radice del sito Web:</span><span class="sxs-lookup"><span data-stu-id="0c9da-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  <span data-ttu-id="0c9da-171">Modifiche</span><span class="sxs-lookup"><span data-stu-id="0c9da-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="0c9da-172">: Modifica della chiave "webPages:AdminFolderVirtualPath" rinominato in "asp: AdminFolderVirtualPath"</span><span class="sxs-lookup"><span data-stu-id="0c9da-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="0c9da-173">Il `webPages:AdminFolderVirtualPath` chiave che può essere aggiunti al *Web. config* ridenominazione di file per specificare il percorso di gestione pacchetti per usare i `asp:` dello spazio dei nomi anziché il `webPages` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="0c9da-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="0c9da-174">Se si usa questo elemento, è necessario rinominarlo nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a>  <span data-ttu-id="0c9da-175">Problemi noti</span><span class="sxs-lookup"><span data-stu-id="0c9da-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="0c9da-176">Problema: Le password degli utenti di appartenenza non è più riconosciuti</span><span class="sxs-lookup"><span data-stu-id="0c9da-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="0c9da-177">L'algoritmo per la creazione e l'archiviazione delle password di appartenenza (account di accesso) è stato modificato per essere più sicuro.</span><span class="sxs-lookup"><span data-stu-id="0c9da-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="0c9da-178">Di conseguenza, le password archiviate per i membri (utenti) creati nelle versioni Beta di ASP.NET Razor non verrà riconosciute.</span><span class="sxs-lookup"><span data-stu-id="0c9da-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="0c9da-179">**Soluzione alternativa** se il sito non è ancora stato inserito nell'ambiente di produzione, rimuovere i record utente dal database di appartenenza.</span><span class="sxs-lookup"><span data-stu-id="0c9da-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="0c9da-180">Se il database si trova in tempo reale, a livello di programmazione rigenerare le password esistenti nel database delle appartenenze.</span><span class="sxs-lookup"><span data-stu-id="0c9da-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="0c9da-181">Problema: Un comportamento imprevisto quando si usa una tabella utente personalizzato per l'appartenenza</span><span class="sxs-lookup"><span data-stu-id="0c9da-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="0c9da-182">Per inizializzare il provider di appartenenze per un sito Web ASP.NET Razor, si chiama il `WebSecurity.InitializeDatabaseConnection` (metodo).</span><span class="sxs-lookup"><span data-stu-id="0c9da-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="0c9da-183">(In WebMatrix, il modello Starter Site include una chiamata al metodo nel  *\_AppStart.cshtml* file.) Se il `autoCreateTables` parametro di questo metodo è impostato su true (per impostazione predefinita, viene impostata su true nel modello Starter Site), e se un nome di tabella non riconosciuto viene passato al metodo (il secondo parametro), il metodo non genera un errore.</span><span class="sxs-lookup"><span data-stu-id="0c9da-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="0c9da-184">Al contrario, viene creato automaticamente nella tabella.</span><span class="sxs-lookup"><span data-stu-id="0c9da-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="0c9da-185">Può trattarsi di un problema, se si prevede di usare una tabella utente personalizzata per l'appartenenza, ma passare il nome della tabella non corretto per il `WebSecurity.InitializeDatabaseConnection` (metodo).</span><span class="sxs-lookup"><span data-stu-id="0c9da-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="0c9da-186">Poiché il metodo non per impostazione predefinita genera un errore se la tabella specificata non esiste e perché crea invece una nuova tabella, è possibile l'applicazione sembra funzionare.</span><span class="sxs-lookup"><span data-stu-id="0c9da-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="0c9da-187">Tuttavia, il codice dell'applicazione che si basa sulla tabella utente personalizzata (e sui campi in essa) può avere esito negativo con errori imprevisti.</span><span class="sxs-lookup"><span data-stu-id="0c9da-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="0c9da-188">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-188">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-189">Assicurarsi che il nome passato il `InitializeDatabaseConnection` corrispondenze di metodo del profilo utente di tabella nel database di appartenenza o assicurarsi che il `autoCreateTables` parametro è impostato su false.</span><span class="sxs-lookup"><span data-stu-id="0c9da-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="0c9da-190">Problema: Messaggio errore "il modulo di amministrazione richiede l'accesso a ~/App\_Data"</span><span class="sxs-lookup"><span data-stu-id="0c9da-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="0c9da-191">In alcune circostanze, il tentativo di creare utenti o in caso contrario, utilizzare il sistema di appartenenze ASP.NET può causare la pagina visualizzare l'errore *il modulo di amministrazione richiede l'accesso a ~/App\_dati*.</span><span class="sxs-lookup"><span data-stu-id="0c9da-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="0c9da-192">Ciò si verifica se l'account che sta utilizzando IIS o IIS Express non dispone di autorizzazioni per creare e scrivere il *App\_dati* cartella sotto la radice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="0c9da-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="0c9da-193">**Soluzione alternativa** creare manualmente un' *App\_dati* cartella per il sito Web.</span><span class="sxs-lookup"><span data-stu-id="0c9da-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="0c9da-194">Assicurarsi che l'account di Windows che l'applicazione viene eseguita con (in genere un servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio App\_dei dati.</span><span class="sxs-lookup"><span data-stu-id="0c9da-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="0c9da-195">Informazioni più dettagliate sono disponibili nell'articolo della Knowledge Base [i problemi delle istanze utente di SQL Server Express e ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="0c9da-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="0c9da-196">Problema: errore "Non riuscita a generare un'istanza utente di SQL Server"</span><span class="sxs-lookup"><span data-stu-id="0c9da-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="0c9da-197">Se un'applicazione Web di WebMatrix utilizza SQL Server Express ed è in esecuzione IIS 7.5 in Windows 7 o Windows Server 2008 R2, si potrebbe essere visualizzato un errore che indica che SQL Server non è possibile recuperare il percorso di applicazioni locali dell'utente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="0c9da-198">**Soluzione alternativa** assicurarsi che l'account di Windows che l'applicazione viene eseguita con (in genere un servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio *App\_Data*.</span><span class="sxs-lookup"><span data-stu-id="0c9da-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="0c9da-199">Informazioni più dettagliate sono disponibili nell'articolo della Knowledge Base [i problemi delle istanze utente di SQL Server Express e ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="0c9da-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="0c9da-200">Problema: I file che contiene le risorse di gestione pacchetti o le password di gestione pacchetti sono utilizzabili in IIS 6.0 e versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="0c9da-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="0c9da-201">Se si distribuisce un'applicazione ASP.NET Web Pages (Razor) che è stata creata usando la versione RC2, e se l'applicazione contiene un *password.txt* oppure *packagesources.txt* file */App\_ Dati/admin*, IIS 6.0 servirà il file se richiesto, può esporre le password per l'istanza di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="0c9da-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="0c9da-202">**Soluzione alternativa** rinominare il *password.txt* oppure *packagesources.txt* file *password.config* o *packagesources.config*. Per impostazione predefinita, IIS 6.0 non fornirà i file con il *config* estensione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="0c9da-203">(In IIS 7, nessun file nei *App\_dati* cartella vengono gestite, in modo che non è necessario rinominare i file.)</span><span class="sxs-lookup"><span data-stu-id="0c9da-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="0c9da-204">Problema: I pacchetti installati con la versione Beta 3 di disinstallazione non rimuovere completamente i componenti del pacchetto</span><span class="sxs-lookup"><span data-stu-id="0c9da-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="0c9da-205">Se è installato un pacchetto utilizzando la gestione dei pacchetti in versione Beta 3 e quindi provare a disinstallare l'estensione Usa la versione corrente, il pacchetto non viene disinstallato completamente.</span><span class="sxs-lookup"><span data-stu-id="0c9da-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="0c9da-206">Tramite la gestione di pacchetti **disinstallazione** pulsante consente di rimuovere alcuni componenti, ma lascia il codice di libreria del pacchetto e non aggiorna il *package* file.</span><span class="sxs-lookup"><span data-stu-id="0c9da-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="0c9da-207">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="0c9da-207">**Workaround** </span></span>  
> <span data-ttu-id="0c9da-208">Seguire questi passaggi:</span><span class="sxs-lookup"><span data-stu-id="0c9da-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="0c9da-209">Eliminare il *App\_Data\packages* cartella.</span><span class="sxs-lookup"><span data-stu-id="0c9da-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="0c9da-210">Questa operazione rimuove tutti i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="0c9da-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="0c9da-211">Eliminare il *Packages. config* file nella radice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="0c9da-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="0c9da-212">Problema: In Visual Studio, richiamare la gestione dei pacchetti basata sul web viene portata offline l'applicazione</span><span class="sxs-lookup"><span data-stu-id="0c9da-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="0c9da-213">Se si lavora in Visual Studio (non WebMatrix) e usare la  *\_admin* funzionalità per la gestione di pacchetti, Visual Studio di avviare l'applicazione viene portata offline e registra il *app\_ offline.htm* nella radice del sito, viene ostacolata la possibilità di usare Gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="0c9da-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="0c9da-214">Sebbene questo comportamento si vedrebbe principalmente quando si usa l'interfaccia di gestione pacchetti basato su web, lo stesso comportamento si verifica se si aggiungere, rimuovere o modificare tutti i file nei *App\_dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="0c9da-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="0c9da-215">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="0c9da-215">**Workaround** </span></span>  
> <span data-ttu-id="0c9da-216">Per utilizzare i pacchetti in Visual Studio, usare l'estensione NuGet anziché la gestione pacchetti basato su web.</span><span class="sxs-lookup"><span data-stu-id="0c9da-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="0c9da-217">Per informazioni, vedere la [documentazione di NuGet](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="0c9da-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="0c9da-218">Se si lavora con gli altri file nei *App\_dati* cartella, si consiglia di mantenere i file in un' posizione per evitare questo problema.</span><span class="sxs-lookup"><span data-stu-id="0c9da-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="0c9da-219">Se non è pratico, eliminare il *app\_offline.htm* file manualmente o attendere fino a quando il sito verrà ripristinato automaticamente (per impostazione predefinita dopo 30 secondi).</span><span class="sxs-lookup"><span data-stu-id="0c9da-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="0c9da-220">Problema: Visual Studio IntelliSense e modelli di progetto disponibili solo in ASP.NET MVC versione 3</span><span class="sxs-lookup"><span data-stu-id="0c9da-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="0c9da-221">Installazione di ASP.NET Web Pages anche installare gli strumenti per Visual Studio, ad esempio i modelli di progetto e IntelliSense per le applicazioni ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="0c9da-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="0c9da-222">**Soluzione alternativa** per usare i modelli di progetto e IntelliSense per le applicazioni ASP.NET Web Pages in Visual Studio, installare ASP.NET MVC 3 RC tramite l'installazione guidata piattaforma Web o il [programma di installazione autonomo](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="0c9da-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="0c9da-223">Problema: I feed di lettura o altri dati esterni tramite un server proxy</span><span class="sxs-lookup"><span data-stu-id="0c9da-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="0c9da-224">Se il server che esegue il sito si trova dietro un server proxy, si potrebbe essere necessario configurare le informazioni sul proxy nel *Web. config* file per poter essere in grado di leggere le informazioni provenienti dall'esterno del sito.</span><span class="sxs-lookup"><span data-stu-id="0c9da-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="0c9da-225">Ad esempio, se si usa il `ReCaptcha` helper, l'helper comunica con il servizio reCAPTCHA, ma potrebbe essere bloccato dal server proxy.</span><span class="sxs-lookup"><span data-stu-id="0c9da-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="0c9da-226">Feed utilizzati nelle pagine Web ASP.NET, ad esempio il feed usato da Gestione pacchetti in modo analogo, potrebbero richiedere la configurazione del proxy.</span><span class="sxs-lookup"><span data-stu-id="0c9da-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="0c9da-227">Se si verificano problemi nell'uso di un servizio esterno o l'utilizzo di feed di pacchetti, inserire gli elementi seguenti nella radice dell'applicazione *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="0c9da-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="0c9da-228">Per altre informazioni sulla configurazione di un server proxy, vedere [ &lt;proxy&gt; (impostazioni di rete)](https://msdn.microsoft.com/library/sa91de1e.aspx) sul sito Web MSDN.</span><span class="sxs-lookup"><span data-stu-id="0c9da-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="0c9da-229">Problema: Disinstallazione di .NET Framework versione 4 Disabilita ASP.NET Web Pages con sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="0c9da-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="0c9da-230">Se si disinstalla .NET Framework versione 4 e quindi reinstallarlo, ASP.NET Web Pages con sintassi Razor è disabilitato.</span><span class="sxs-lookup"><span data-stu-id="0c9da-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="0c9da-231">Le pagine con le *cshtml* estensioni non vengono eseguite correttamente.</span><span class="sxs-lookup"><span data-stu-id="0c9da-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="0c9da-232">ASP.NET Web Pages registra un assembly nella directory radice macchina *Web. config* file e rimozione di .NET Framework consente di rimuovere tale file.</span><span class="sxs-lookup"><span data-stu-id="0c9da-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="0c9da-233">Reinstallare .NET Framework viene installata una versione nuova del file di configurazione, ma non aggiunge il riferimento per l'assembly di ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="0c9da-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="0c9da-234">**Soluzione alternativa** dopo la reinstallazione di .NET Framework, reinstallare ASP.NET Web Pages con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="0c9da-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="0c9da-235">Verrà aggiunto l'elemento seguente per il *Web. config* file nella radice del computer, ovvero in genere nel percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="0c9da-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="0c9da-236">Problema: Gli URL senza estensioni non si trova il file.cshtml/.vbhtml in IIS 7 o IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="0c9da-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="0c9da-237">In IIS 7 o IIS 7.5, le richieste con un URL analogo al seguente non sono in grado di trovare le pagine che hanno le *. cshtml* oppure *vbhtml* estensione:</span><span class="sxs-lookup"><span data-stu-id="0c9da-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="0c9da-238">Il problema si verifica perché la riscrittura degli URL non è abilitato per impostazione predefinita per IIS 7 o IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="0c9da-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="0c9da-239">Lo scenario probabile che è che il problema non viene visualizzato durante il test in locale tramite IIS Express, ma si verificano quando si distribuisce il sito Web in un sito Web di hosting.</span><span class="sxs-lookup"><span data-stu-id="0c9da-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="0c9da-240">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="0c9da-241">Se si dispone di controllo sul computer del server, nel computer del server, installare l'aggiornamento descritto nel [è disponibile che consente di determinati gestori di IIS 7.0 o IIS 7.5 di gestire le richieste il cui URL non terminano con un periodo di un aggiornamento](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="0c9da-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="0c9da-242">Se non è un controllo sul computer server (ad esempio, si distribuisce in un sito Web di hosting), aggiungere il codice seguente nel sito Web *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="0c9da-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="0c9da-243">Problema: Distribuire un'applicazione in un computer che non dispone di SQL Server Compact è installato</span><span class="sxs-lookup"><span data-stu-id="0c9da-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="0c9da-244">Le applicazioni che includono i database di SQL Server Compact possono eseguire in un computer in cui SQL Server Compact non è installato.</span><span class="sxs-lookup"><span data-stu-id="0c9da-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="0c9da-245">Microsoft WebMatrix 1.0 automaticamente consente di copiare tali file binari per l'utente ed esegue l'appropriato *Web. config* trasformazioni di file.</span><span class="sxs-lookup"><span data-stu-id="0c9da-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="0c9da-246">**Soluzione alternativa** se è necessario copiare tali file e apportare le *Web. config* modifiche al file manualmente, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0c9da-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="0c9da-247">Copiare gli assembly del motore di database per il *Bin* cartella (e sottocartelle) dell'applicazione nel computer di destinazione:</span><span class="sxs-lookup"><span data-stu-id="0c9da-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="0c9da-248">Copia *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="0c9da-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>        <span data-ttu-id="0c9da-249">**to** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="0c9da-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="0c9da-250">Copia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>alla</em></strong>\Bin\x86\*</span><span class="sxs-lookup"><span data-stu-id="0c9da-250">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86\*</span></span>
>    - <span data-ttu-id="0c9da-251">Copia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>alla</strong><em>\Bin\amd64</em></span><span class="sxs-lookup"><span data-stu-id="0c9da-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span></span>
> 
> 2. <span data-ttu-id="0c9da-252">Nella cartella radice del sito Web, creare o aprire una *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="0c9da-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="0c9da-253">(In WebMatrix 1.0, questo tipo di file è disponibile se si fa clic **tutte** nel **scegliere un tipo di File** nella finestra di dialogo.)</span><span class="sxs-lookup"><span data-stu-id="0c9da-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="0c9da-254">Aggiungere l'elemento seguente come elemento figlio di `<configuration>` elemento (non all'interno di `<system.web>` elemento):</span><span class="sxs-lookup"><span data-stu-id="0c9da-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="0c9da-255">Problema: "Database" e "WebGrid" helper non funzionano in Medium Trust in Visual Basic</span><span class="sxs-lookup"><span data-stu-id="0c9da-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="0c9da-256">Se si usa Visual Basic (creando *vbhtml* file), il `Database` e `WebGrid` helper non funzionerà se l'applicazione è impostata per usare Medium Trust.</span><span class="sxs-lookup"><span data-stu-id="0c9da-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="0c9da-257">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-257">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-258">Se si usa Visual Studio 2010, è possibile risolvere il problema installando la versione Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="0c9da-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="0c9da-259">Fino a quando non è disponibile la versione finale della versione SP1, è possibile scaricare la versione Beta di SP1 dal [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) pagina nel Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="0c9da-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="0c9da-260">Se questo non è pratico, o se non si utilizza Visual Studio 2010, è possibile temporaneamente impostare l'applicazione per usare l'attendibilità.</span><span class="sxs-lookup"><span data-stu-id="0c9da-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="0c9da-261">Problema: "ApplicationPart" le risorse siano accessibili esternamente</span><span class="sxs-lookup"><span data-stu-id="0c9da-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="0c9da-262">Se un assembly contiene gli oggetti da cui deriva il `ApplicationPart` che le risorse dell'assembly vengono esposte dalla classe di `ResourceRouteHandler` classe.</span><span class="sxs-lookup"><span data-stu-id="0c9da-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="0c9da-263">Si consideri ad esempio l'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="0c9da-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="0c9da-264">Questa richiesta di download di tutte le stringhe di risorse nel *System.Web.WebPages.Administration.dll* assembly.</span><span class="sxs-lookup"><span data-stu-id="0c9da-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="0c9da-265">Tutte le risorse incorporate, anche quelli che non sono destinati a essere gestite come contenuto statico, vengono scaricate.</span><span class="sxs-lookup"><span data-stu-id="0c9da-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="0c9da-266">Se le risorse incorporate contengono informazioni riservate, ciò può rappresentare un rischio per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="0c9da-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="0c9da-267">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="0c9da-267">**Workaround** </span></span>  
> <span data-ttu-id="0c9da-268">Se si crea un' **ApplicationPart** dell'oggetto, assicurarsi che le risorse incorporate associati **ApplicationPart** assembly dell'oggetto non contengono informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="0c9da-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="0c9da-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="0c9da-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="0c9da-270">Per informazioni sui problemi di installazione per WebMatrix, vedere [problemi di installazione di WebMatrix](#Known_Issues_Installation) più indietro in questo documento.</span><span class="sxs-lookup"><span data-stu-id="0c9da-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="0c9da-271">In questa sezione del documento vengono descritti problemi noti per l'ambiente di sviluppo WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="0c9da-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="0c9da-272">Problema: Il nome utente o password di una stringa di connessione di database in un file Web. config non vengono riflesse nell'area di lavoro di database</span><span class="sxs-lookup"><span data-stu-id="0c9da-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="0c9da-273">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="0c9da-274">Nel *Web. config* file, modificare il nome del database nella stringa di connessione (ad esempio, aggiungere "1" a esso).</span><span class="sxs-lookup"><span data-stu-id="0c9da-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="0c9da-275">Salvare il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="0c9da-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="0c9da-276">Fare clic su **database** e aggiornare.</span><span class="sxs-lookup"><span data-stu-id="0c9da-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="0c9da-277">Modificare il nome del database nella stringa di connessione nel *Web. config* file il nome del database originale.</span><span class="sxs-lookup"><span data-stu-id="0c9da-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="0c9da-278">Salvare il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="0c9da-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="0c9da-279">Fare clic su **database** e aggiornare.</span><span class="sxs-lookup"><span data-stu-id="0c9da-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="0c9da-280">Problema: Non è possibile eliminare le cartelle create da WebMatrix</span><span class="sxs-lookup"><span data-stu-id="0c9da-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="0c9da-281">Se WebMatrix viene eseguito con autorizzazioni elevate (vale a dire, si inizia a utilizzare WebMatrix il **Esegui come amministratore** opzione in Windows), non è possibile eliminare le cartelle che vengono create da WebMatrix utilizza Windows Explorer.</span><span class="sxs-lookup"><span data-stu-id="0c9da-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="0c9da-282">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-282">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-283">Eseguire Esplora Windows con autorizzazioni elevate.</span><span class="sxs-lookup"><span data-stu-id="0c9da-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="0c9da-284">Attenersi ai passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="0c9da-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="0c9da-285">In Windows, fare clic su **avviare**.</span><span class="sxs-lookup"><span data-stu-id="0c9da-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="0c9da-286">Immettere "Windows Explorer" e fare clic sulla voce relativa **Windows Explorer**.</span><span class="sxs-lookup"><span data-stu-id="0c9da-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="0c9da-287">Fare clic su **Esegui come amministratore**.</span><span class="sxs-lookup"><span data-stu-id="0c9da-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="0c9da-288">È quindi possibile eliminare le cartelle.</span><span class="sxs-lookup"><span data-stu-id="0c9da-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="0c9da-289">Problema: WebMatrix 1.0 non riesce a eseguire determinate attività che richiedono l'elevazione dei privilegi</span><span class="sxs-lookup"><span data-stu-id="0c9da-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="0c9da-290">WebMatrix 1.0 non riesce a eseguire determinate attività che richiedono l'elevazione dei privilegi, ad esempio l'installazione di componenti aggiuntivi nelle situazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0c9da-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="0c9da-291">In Windows Vista o Windows 7, si è connessi con un account che dispone di privilegi amministrativi e controllo Account utente (UAC) è disabilitato.</span><span class="sxs-lookup"><span data-stu-id="0c9da-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="0c9da-292">Si utilizza Microsoft Windows XP o Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="0c9da-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="0c9da-293">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-293">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-294">La maggior parte delle attività in WebMatrix 1.0 non richiedono autorizzazioni amministrative.</span><span class="sxs-lookup"><span data-stu-id="0c9da-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="0c9da-295">Per questi, è possibile eseguire l'operazione perché un amministratore o seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="0c9da-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="0c9da-296">In Windows Vista o Windows 7, abilitare la UAC.</span><span class="sxs-lookup"><span data-stu-id="0c9da-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="0c9da-297">In Windows XP, aggiungere l'utente al gruppo di sicurezza Administrators.</span><span class="sxs-lookup"><span data-stu-id="0c9da-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="0c9da-298">Problema: "Del sito da Web Gallery" è disabilitato</span><span class="sxs-lookup"><span data-stu-id="0c9da-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="0c9da-299">Il **sito da Web Gallery** opzione è disabilitata se l'installazione guidata piattaforma Web 3.0 non è installato.</span><span class="sxs-lookup"><span data-stu-id="0c9da-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="0c9da-300">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-300">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-301">Installare il [installazione guidata piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="0c9da-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="0c9da-302">Problema: Google Chrome non è disponibile come opzione di esecuzione</span><span class="sxs-lookup"><span data-stu-id="0c9da-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="0c9da-303">Google Chrome non viene visualizzata nell'elenco dei browser nel **eseguiti** nel **Home** scheda.</span><span class="sxs-lookup"><span data-stu-id="0c9da-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="0c9da-304">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-304">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-305">Alcune versioni di Google Chrome non si registrano correttamente con la funzionalità programmi predefiniti in Windows.</span><span class="sxs-lookup"><span data-stu-id="0c9da-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="0c9da-306">Risolvere il problema, avviare Google Chrome, scegliere il *personalizzare e controllo Google Chrome* menu, fare clic su *opzioni*e quindi fare clic su *marca Google Chrome il browser predefinito installato*.</span><span class="sxs-lookup"><span data-stu-id="0c9da-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="0c9da-307">Problema: La finestra di dialogo "Foreign Key" non consente l'immissione di una chiave primaria</span><span class="sxs-lookup"><span data-stu-id="0c9da-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="0c9da-308">Il **Foreign Key** nella finestra di dialogo non consente di immettere il nome della chiave primario della tabella di chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="0c9da-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="0c9da-309">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-309">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-310">Ciò è intenzionale.</span><span class="sxs-lookup"><span data-stu-id="0c9da-310">This is intentional.</span></span> <span data-ttu-id="0c9da-311">Non devi immettere il nome della chiave primaria della tabella della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="0c9da-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="0c9da-312">Problema: IntelliSense non è disponibile in WebMatrix per Razor sintassi, c# o Visual Basic</span><span class="sxs-lookup"><span data-stu-id="0c9da-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="0c9da-313">IntelliSense è supportata in WebMatrix per HTML e CSS.</span><span class="sxs-lookup"><span data-stu-id="0c9da-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="0c9da-314">Non è tuttavia disponibile per altri linguaggi.</span><span class="sxs-lookup"><span data-stu-id="0c9da-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="0c9da-315">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="0c9da-315">**Workaround** </span></span>  
> <span data-ttu-id="0c9da-316">Nessuno.</span><span class="sxs-lookup"><span data-stu-id="0c9da-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="0c9da-317">Problema: IntelliSense per HTML e CSS suggerisce gli elementi che non sono appropriati in base al contesto</span><span class="sxs-lookup"><span data-stu-id="0c9da-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="0c9da-318">IntelliSense per il markup in WebMatrix supporta HTML usando il [schema XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) e l'utilizzo di CSS il [dello schema CSS 2.1](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="0c9da-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="0c9da-319">Poiché IntelliSense si basa su questi schemi specifici, alcuni tag, gli attributi o proprietà potrebbero proposto che non sono appropriate per la definizione di stile o di pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="0c9da-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="0c9da-320">Per HTML, può anche causare suggerimenti imprevisti nel contenuto che potrebbero essere interpretati come XHTML in formato non valido (ad esempio, quando i tag non vengono chiuse).</span><span class="sxs-lookup"><span data-stu-id="0c9da-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="0c9da-321">Questo problema potrebbe essere più evidente se il punto di inserimento si trova all'interno di un tag incompleto; In tal caso, IntelliSense potrebbe suggerire nuovi tag di apertura o offrire altri suggerimenti non corretti.</span><span class="sxs-lookup"><span data-stu-id="0c9da-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="0c9da-322">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="0c9da-322">**Workaround** </span></span>  
> <span data-ttu-id="0c9da-323">Per HTML, assicurarsi di lavorare all'interno di una pagina XHTML completata in formato corretto.</span><span class="sxs-lookup"><span data-stu-id="0c9da-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="0c9da-324">Per CSS, non è disponibile alcuna soluzione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="0c9da-325">Problema: IntelliSense non viene richiamato durante la digitazione</span><span class="sxs-lookup"><span data-stu-id="0c9da-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="0c9da-326">In alcuni casi, IntelliSense potrebbe non essere richiamato come HTML o CSS venga immesso nell'editor di.</span><span class="sxs-lookup"><span data-stu-id="0c9da-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="0c9da-327">In particolare, questo può verificarsi quando il punto di inserimento è direttamente accanto a un altro elemento o alla fine di un file.</span><span class="sxs-lookup"><span data-stu-id="0c9da-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="0c9da-328">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="0c9da-328">**Workaround** </span></span>  
> <span data-ttu-id="0c9da-329">Assicurarsi che vi sia uno spazio vuoto attorno al punto di inserimento e che il punto di inserimento non è alla fine di un file.</span><span class="sxs-lookup"><span data-stu-id="0c9da-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="0c9da-330">È anche possibile richiamare IntelliSense manualmente premendo Ctrl + barra spaziatrice.</span><span class="sxs-lookup"><span data-stu-id="0c9da-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="0c9da-331">Problema: Nessuna interfaccia utente è disponibile per la disabilitazione di IntelliSense</span><span class="sxs-lookup"><span data-stu-id="0c9da-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="0c9da-332">WebMatrix 1.0 non fornisce alcuna interfaccia utente o il movimento per disabilitare IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="0c9da-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="0c9da-333">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="0c9da-333">**Workaround** </span></span>  
> <span data-ttu-id="0c9da-334">Iniziare a WebMatrix mediante il comando seguente, che include un'opzione che disabilita IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="0c9da-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="0c9da-335">IIS Express</span><span class="sxs-lookup"><span data-stu-id="0c9da-335">IIS Express</span></span>

<span data-ttu-id="0c9da-336">IIS Express ha un proprio file Leggimi, che è disponibile all'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="0c9da-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="0c9da-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="0c9da-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="0c9da-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="0c9da-338">SQL Server Compact</span></span>

<span data-ttu-id="0c9da-339">SQL Server Compact ha un proprio file Leggimi, che è disponibile all'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="0c9da-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="0c9da-340">Per informazioni sui problemi che interessano l'installazione di SQL Server Compact come parte di WebMatrix, vedere [problemi di installazione di WebMatrix](#Known_Issues_Installation) più indietro in questo documento.</span><span class="sxs-lookup"><span data-stu-id="0c9da-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a>  <span data-ttu-id="0c9da-341">Installazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="0c9da-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="0c9da-342">Problema: Installazione di un'applicazione può richiedere molto tempo se cartella documenti dell'utente viene reindirizzata a una condivisione di rete</span><span class="sxs-lookup"><span data-stu-id="0c9da-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="0c9da-343">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-343">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-344">Nessuno.</span><span class="sxs-lookup"><span data-stu-id="0c9da-344">None.</span></span> <span data-ttu-id="0c9da-345">L'applicazione potrebbe richiedere del tempo per l'installazione, ma verrà installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="0c9da-345">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a>  <span data-ttu-id="0c9da-346">Pubblicazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="0c9da-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="0c9da-347">Problema: "richiesto non è possibile acquisire le autorizzazioni" Errore durante la pubblicazione di un Database di SQL Compact</span><span class="sxs-lookup"><span data-stu-id="0c9da-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="0c9da-348">WebMatrix non supporta completamente la distribuzione di file binari di supporti per SQL Server Compact a un server che esegue .NET Framework versione 3.5 con una configurazione a livello di attendibilità medio.</span><span class="sxs-lookup"><span data-stu-id="0c9da-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="0c9da-349">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-349">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-350">La soluzione migliore consiste nell'installare .NET Framework 4 nel server.</span><span class="sxs-lookup"><span data-stu-id="0c9da-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="0c9da-351">In alternativa, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0c9da-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="0c9da-352">Aggiungere i seguenti elementi per il `SecurityClasses` nella sezione *Web\_MediumTrust.config* file:</span><span class="sxs-lookup"><span data-stu-id="0c9da-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="0c9da-353">Creare un nuovo set di autorizzazioni nel *Web\_MediumTrust.config* file con le autorizzazioni necessarie seguenti:</span><span class="sxs-lookup"><span data-stu-id="0c9da-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="0c9da-354">Applicare l'autorizzazione impostata su SQL Server Compact inserendo gli elementi seguenti nel *Web\_MediumTrust.config* file:</span><span class="sxs-lookup"><span data-stu-id="0c9da-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="0c9da-355">Problema: Le applicazioni web della raccolta e PhpBB visualizzano un errore "Servizio non disponibile" dopo la pubblicazione</span><span class="sxs-lookup"><span data-stu-id="0c9da-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="0c9da-356">In alcune circostanze, la pubblicazione di un'applicazione causa un errore "servizio non disponibile".</span><span class="sxs-lookup"><span data-stu-id="0c9da-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="0c9da-357">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-357">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-358">In WebMatrix, aggiungere una barra rovesciata (\) alla fine del nome del server nel **impostazioni di pubblicazione** finestra e quindi pubblicare nuovamente l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="0c9da-359">Problema: I collegamenti e il layout di sito Web di Moodle vengono interrotti dopo la pubblicazione</span><span class="sxs-lookup"><span data-stu-id="0c9da-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="0c9da-360">Dopo aver pubblicato un'applicazione di Moodle, l'applicazione non funziona correttamente.</span><span class="sxs-lookup"><span data-stu-id="0c9da-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="0c9da-361">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-361">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-362">In WebMatrix, aggiungere una barra (/) alla fine del **nome del sito** campo le **impostazioni di pubblicazione** finestra e quindi pubblicare nuovamente l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="0c9da-363">Problema: La pubblicazione di nopCommerce ha esito negativo con un errore di database</span><span class="sxs-lookup"><span data-stu-id="0c9da-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="0c9da-364">NopCommerce pubblicazione ha esito negativo e viene segnalato un errore di database, ad esempio "inserire il nop\_tabella del log non è riuscita."</span><span class="sxs-lookup"><span data-stu-id="0c9da-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="0c9da-365">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="0c9da-366">In WebMatrix, fare clic su **eseguire** avviare nopCommerce in locale.</span><span class="sxs-lookup"><span data-stu-id="0c9da-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="0c9da-367">Accedere alla pagina Amministrazione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="0c9da-368">Scegliere il **sistema** menu.</span><span class="sxs-lookup"><span data-stu-id="0c9da-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="0c9da-369">Scegliere il **Log** opzione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="0c9da-370">Scegliere il **Cancella Log** pulsante.</span><span class="sxs-lookup"><span data-stu-id="0c9da-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="0c9da-371">Pubblicare di nuovo nopCommerce.</span><span class="sxs-lookup"><span data-stu-id="0c9da-371">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="0c9da-372">Problema: Silverstripe CMS Visualizza un "errore HTTP 500 PHP FCGI" quando si scarica un sito pubblicato</span><span class="sxs-lookup"><span data-stu-id="0c9da-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="0c9da-373">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-373">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-374">Dopo aver fatto clic **Download pubblicati site**, ignorare `silverstripe-cache/manifest_main` nelle **Publish Preview**.</span><span class="sxs-lookup"><span data-stu-id="0c9da-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="0c9da-375">Questo file viene usato per la memorizzazione nella cache ed è specifico per ogni computer.</span><span class="sxs-lookup"><span data-stu-id="0c9da-375">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="0c9da-376">Problema: Subtext Visualizza "Errore Server nell'applicazione '/'" quando si scarica un sito pubblicato</span><span class="sxs-lookup"><span data-stu-id="0c9da-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="0c9da-377">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-377">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-378">Aprire il sito *Web. config* file e sostituire l'ID utente e password nella stringa di connessione del database con le credenziali di amministratore di SQL Server (le credenziali "sa").</span><span class="sxs-lookup"><span data-stu-id="0c9da-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="0c9da-379">In alternativa, seguire questi passaggi per concedere all'account utente si è connessi con `db_owner` autorizzazioni:</span><span class="sxs-lookup"><span data-stu-id="0c9da-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="0c9da-380">Installare SQL Server Management Studio utilizzando l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="0c9da-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="0c9da-381">Connettersi all'istanza di SQL Server Express locale (per impostazione predefinita, `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="0c9da-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="0c9da-382">Fare clic su **database** &gt; *[localSubtextDatabase]* &gt; **sicurezza** &gt; **utenti** &gt; *[localSubtextUser*] (valore predefinito è `subtextuser`], pulsante destro del mouse e scegliere **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="0c9da-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="0c9da-383">Selezionare **db\_proprietario** nella sezione l'appartenenza al ruolo.</span><span class="sxs-lookup"><span data-stu-id="0c9da-383">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="0c9da-384">Problema: Sito potrebbe non funzionare dopo la pubblicazione se il campo "URL di destinazione" non è il prefisso http:// o https://</span><span class="sxs-lookup"><span data-stu-id="0c9da-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="0c9da-385">Nel **impostazioni di pubblicazione** finestra di dialogo, se l'URL di destinazione non inizia con `http://` o `https://`, il sito potrebbe non funzionare dopo la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="0c9da-386">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-386">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-387">Assicurarsi che prima di pubblicare un sito, l'URL di destinazione nel **impostazioni di pubblicazione** finestra di dialogo inizia con `http://` o `https://`.</span><span class="sxs-lookup"><span data-stu-id="0c9da-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="0c9da-388">Problema: La pubblicazione di un database MySQL non riesce con l'errore "non è stato possibile pubblicare il database.</span><span class="sxs-lookup"><span data-stu-id="0c9da-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="0c9da-389">Questa situazione può verificarsi se il database remoto non è possibile eseguire lo script."</span><span class="sxs-lookup"><span data-stu-id="0c9da-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="0c9da-390">L'errore può verificarsi per diversi motivi.</span><span class="sxs-lookup"><span data-stu-id="0c9da-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="0c9da-391">Un motivo per cui che è possibile visualizzare questo errore è se lo script di database contiene un carattere di virgoletta singola (') e set di caratteri predefinito del database MySQL di destinazione non è presente in UTF-8.</span><span class="sxs-lookup"><span data-stu-id="0c9da-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="0c9da-392">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-392">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-393">Impostare il carattere predefinito impostato per il database MySQL remoto su UTF-8.</span><span class="sxs-lookup"><span data-stu-id="0c9da-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="0c9da-394">Problema: Alcuni collegamenti non sono visibili in DotNetNuke dopo la pubblicazione o il download del sito</span><span class="sxs-lookup"><span data-stu-id="0c9da-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="0c9da-395">Se si pubblica o un sito di DotNetNuke di download, si potrebbe essere necessario cancellare la cache per ottenere i nuovi collegamenti vengano visualizzati nel sito.</span><span class="sxs-lookup"><span data-stu-id="0c9da-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="0c9da-396">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="0c9da-397">Accedere come "Host".</span><span class="sxs-lookup"><span data-stu-id="0c9da-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="0c9da-398">Passare al menu di host e selezionare **delle impostazioni dell'Host**.</span><span class="sxs-lookup"><span data-stu-id="0c9da-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="0c9da-399">Scorrere verso il basso e sotto **impostazioni avanzate**, espandere **le impostazioni delle prestazioni**.</span><span class="sxs-lookup"><span data-stu-id="0c9da-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="0c9da-400">Scegliere il **Cancella Cache** collegamento per le pagine.</span><span class="sxs-lookup"><span data-stu-id="0c9da-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="0c9da-401">Passare alla parte inferiore della pagina e riavviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-401">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="0c9da-402">Problema: Alcuni collegamenti in AtomSite vengono interrotte dopo il download di un sito pubblicato</span><span class="sxs-lookup"><span data-stu-id="0c9da-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="0c9da-403">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-403">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-404">Nel *service.config* file, *users.config* file e tutti i *. XML* file, sostituire la stringa URL (ad esempio, `http://myhost.com/atomsite`) con quello locale (ad esempio, `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="0c9da-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="0c9da-405">Problema: Le applicazioni basate su MySQL, ad esempio WordPress non è possibile pubblicare e segnalano un errore di database</span><span class="sxs-lookup"><span data-stu-id="0c9da-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="0c9da-406">Per impostazione predefinita, WebMatrix installa MySQL con il set di caratteri UTF-8.</span><span class="sxs-lookup"><span data-stu-id="0c9da-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="0c9da-407">Se si installa MySQL per conto proprio, e il set di caratteri non è UTF-8 (ad esempio, è Latin1), il processo di pubblicazione per i database potrebbe non riuscire.</span><span class="sxs-lookup"><span data-stu-id="0c9da-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="0c9da-408">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="0c9da-409">Modificare il set di caratteri per MySQL in UTF-8.</span><span class="sxs-lookup"><span data-stu-id="0c9da-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="0c9da-410">(Per informazioni dettagliate, vedere [regole di confronto e Set di caratteri Server](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) sul sito Web di MySQL.)</span><span class="sxs-lookup"><span data-stu-id="0c9da-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="0c9da-411">Reinstallare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="0c9da-412">Ripubblicare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0c9da-412">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="0c9da-413">Problema: "Download pubblicati sito" ha esito negativo per le applicazioni con il programma di installazione basata su browser</span><span class="sxs-lookup"><span data-stu-id="0c9da-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="0c9da-414">Alcune applicazioni (ad esempio, Kentico CMS) è necessario avviarli in del browser per eseguire il programma di installazione post-installazione, ad esempio la creazione di un database.</span><span class="sxs-lookup"><span data-stu-id="0c9da-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="0c9da-415">Se si pubblica un'applicazione simile al seguente senza completare l'installazione basata su browser, il download dello stesso sito da un server remoto non riuscirà.</span><span class="sxs-lookup"><span data-stu-id="0c9da-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="0c9da-416">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-416">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-417">Completare la configurazione basata su browser prima di pubblicare il sito.</span><span class="sxs-lookup"><span data-stu-id="0c9da-417">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="0c9da-418">Problema: "Download pubblicati sito" ha esito negativo con un errore di database per DotNetNuke e Kooboo CMS</span><span class="sxs-lookup"><span data-stu-id="0c9da-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="0c9da-419">Se si prova a scaricare un'applicazione da un server e si dispone di credenziali di amministratore nella stringa di connessione del database nel **impostazioni di pubblicazione** finestra di dialogo, si potrebbe essere visualizzato l'errore seguente nel log di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="0c9da-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="0c9da-420">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="0c9da-420">**Workaround**</span></span>  
> <span data-ttu-id="0c9da-421">Se è più pratico, ripubblicare il sito (oppure che venga pubblicato) usando credenziali senza privilegi di amministratore per il database.</span><span class="sxs-lookup"><span data-stu-id="0c9da-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="0c9da-422">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="0c9da-422">For More Information</span></span>

<span data-ttu-id="0c9da-423">Per ulteriori informazioni su WebMatrix 1.0, vedere i siti Web seguenti:</span><span class="sxs-lookup"><span data-stu-id="0c9da-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="0c9da-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="0c9da-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="0c9da-425">ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="0c9da-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="0c9da-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="0c9da-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="0c9da-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="0c9da-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="0c9da-428">Tutti i diritti riservati.</span><span class="sxs-lookup"><span data-stu-id="0c9da-428">All Rights Reserved.</span></span> <span data-ttu-id="0c9da-429">[Le condizioni d'uso](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="0c9da-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
