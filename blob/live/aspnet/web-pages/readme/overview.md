---
uid: web-pages/readme/overview
title: File Leggimi di WebMatrix | Documenti Microsoft
author: rick-anderson
description: WebMatrix e il file Leggimi ASP.NET Web Pages (Razor) versione 1.0
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 90f24550d2bb50147bab6be545be63c1838f312a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="webmatrix-readme"></a><span data-ttu-id="920f6-103">File Leggimi di WebMatrix</span><span class="sxs-lookup"><span data-stu-id="920f6-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="920f6-104">13 gennaio 2011</span><span class="sxs-lookup"><span data-stu-id="920f6-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="920f6-105">Contenuto</span><span class="sxs-lookup"><span data-stu-id="920f6-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="920f6-106">Questo file Leggimi si applica alla 1.0 versione di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="920f6-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="920f6-107">Panoramica</span><span class="sxs-lookup"><span data-stu-id="920f6-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="920f6-108">Installazione</span><span class="sxs-lookup"><span data-stu-id="920f6-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="920f6-109">Come pubblicare applicazioni</span><span class="sxs-lookup"><span data-stu-id="920f6-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="920f6-110">Le modifiche e problemi</span><span class="sxs-lookup"><span data-stu-id="920f6-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="920f6-111">Installazione di WebMatrix 1.0</span><span class="sxs-lookup"><span data-stu-id="920f6-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="920f6-112">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="920f6-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="920f6-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="920f6-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="920f6-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="920f6-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="920f6-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="920f6-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="920f6-116">L'installazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="920f6-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="920f6-117">Pubblicazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="920f6-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="920f6-118">Per ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="920f6-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="920f6-119">Panoramica</span><span class="sxs-lookup"><span data-stu-id="920f6-119">Overview</span></span>

> <span data-ttu-id="920f6-120">Microsoft WebMatrix 1.0 è uno stack di sviluppo web gratuito che installa in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="920f6-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="920f6-121">Si integra un server web con il database e programmazione Framework per creare un'esperienza unica e integrata.</span><span class="sxs-lookup"><span data-stu-id="920f6-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="920f6-122">È possibile utilizzare WebMatrix per ottimizzare le attività di codice, testare e pubblicare il proprio sito Web ASP.NET o PHP oppure è possibile utilizzare WebMatrix per avviare un nuovo sito Web utilizzando più comuni App open source come DotNetNuke, Umbraco, WordPress o Joomla.</span><span class="sxs-lookup"><span data-stu-id="920f6-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="920f6-123">WebMatrix utilizza stesso server web potente, motore di database e ambiente di Framework che verrà eseguito il sito Web in internet, che effettua la transizione dallo sviluppo alla produzione modo diretto e immediato.</span><span class="sxs-lookup"><span data-stu-id="920f6-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="920f6-124">Installazione</span><span class="sxs-lookup"><span data-stu-id="920f6-124">Installation</span></span>

> <span data-ttu-id="920f6-125">Per installare WebMatrix 1.0, è necessario installare il [programma di installazione di piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="920f6-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="920f6-126">Dopo aver installato l'installazione guidata piattaforma Web, è possibile utilizzare, per installare WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="920f6-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="920f6-127">Se si verificano problemi durante l'installazione, fare riferimento a [risoluzione dei problemi con l'installazione guidata piattaforma Web di Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="920f6-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="920f6-128">Come pubblicare applicazioni</span><span class="sxs-lookup"><span data-stu-id="920f6-128">How to Publish Applications</span></span>

> <span data-ttu-id="920f6-129">Vedere [istruzioni dettagliate per la pubblicazione di applicazioni](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="920f6-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="920f6-130">Le modifiche e problemi</span><span class="sxs-lookup"><span data-stu-id="920f6-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="920f6-131">Problemi di installazione 1.0 di WebMatrix</span><span class="sxs-lookup"><span data-stu-id="920f6-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="920f6-132">Problema: WebMatrix 1.0 è disponibile solo su piattaforme che supportano Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="920f6-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="920f6-133">.NET Framework versione 4 è obbligatorio per WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="920f6-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="920f6-134">In alcuni casi, il programma di installazione di WebMatrix 1.0 consente di provare a installare su una piattaforma che non fa parte del set di configurazione supportata.</span><span class="sxs-lookup"><span data-stu-id="920f6-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="920f6-135">In particolare, Windows Vista senza l'aggiornamento SP1 consentono di iniziare l'installazione di WebMatrix, ma il componente di .NET Framework 4 avrà esito negativo e bloccare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="920f6-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="920f6-136">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-136">**Workaround**</span></span>  
> <span data-ttu-id="920f6-137">Installare in una piattaforma supportata, che include:</span><span class="sxs-lookup"><span data-stu-id="920f6-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="920f6-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="920f6-138">Windows 7</span></span>
> - <span data-ttu-id="920f6-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="920f6-139">Windows Server 2008</span></span>
> - <span data-ttu-id="920f6-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="920f6-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="920f6-141">Windows Vista SP1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="920f6-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="920f6-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="920f6-142">Windows XP SP3</span></span>
> - <span data-ttu-id="920f6-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="920f6-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="920f6-144">Problema: Impossibile installare WebMatrix 1.0 se è installato Microsoft Visual Studio 2008 senza Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="920f6-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="920f6-145">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-145">**Workaround**</span></span>  
> <span data-ttu-id="920f6-146">Installare [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) dall'area Download Microsoft.</span><span class="sxs-lookup"><span data-stu-id="920f6-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="920f6-147">Problema: Alcuni assembly di SQL Server Compact 4.0 non sono installati nella Global Assembly Cache</span><span class="sxs-lookup"><span data-stu-id="920f6-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="920f6-148">Quando si installa SQL Server Compact 4.0 in un computer a 64 bit e il computer dispone solo di .NET Framework 3.5 SP1 Client Profile installato, gli assembly gestiti per SQL Server Compact 4.0 non vengono inseriti in global assembly cache (GAC).</span><span class="sxs-lookup"><span data-stu-id="920f6-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="920f6-149">Gli assembly gestiti che non sono installati nella Global Assembly Cache sono:</span><span class="sxs-lookup"><span data-stu-id="920f6-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="920f6-150">*SqlServerCe* (provider ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="920f6-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="920f6-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="920f6-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="920f6-152">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-152">**Workaround**</span></span>  
> <span data-ttu-id="920f6-153">Disinstallare SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="920f6-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="920f6-154">Scaricare e installare la versione completa di .NET Framework 3.5 SP1 dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="920f6-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="920f6-155">Microsoft .NET Framework 3.5 Service pack 1 (pacchetto completo)</span><span class="sxs-lookup"><span data-stu-id="920f6-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="920f6-156">Reinstallare SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="920f6-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="920f6-157">Problema: Impossibile disinstallare SQL Server Compact mediante la riga di comando</span><span class="sxs-lookup"><span data-stu-id="920f6-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="920f6-158">La disinstallazione di SQL Server Compact mediante le opzioni della riga di comando non funziona in questa versione.</span><span class="sxs-lookup"><span data-stu-id="920f6-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="920f6-159">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-159">**Workaround**</span></span>  
> <span data-ttu-id="920f6-160">Utilizzare *programmi e funzionalità* nel Pannello di controllo per disinstallare Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="920f6-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="920f6-161">Pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="920f6-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="920f6-162">In questa sezione del documento vengono descritte le nuove funzionalità, le modifiche e problemi noti con la 1.0 versione di ASP.NET Web Pages con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="920f6-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="920f6-163">Nuove funzionalità</span><span class="sxs-lookup"><span data-stu-id="920f6-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="920f6-164">Modifiche</span><span class="sxs-lookup"><span data-stu-id="920f6-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="920f6-165">Problemi</span><span class="sxs-lookup"><span data-stu-id="920f6-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a><span data-ttu-id="920f6-166">Nuove funzionalità</span><span class="sxs-lookup"><span data-stu-id="920f6-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="920f6-167">Novità: Impostazione di configurazione aggiunta per disabilitare la gestione dei pacchetti</span><span class="sxs-lookup"><span data-stu-id="920f6-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="920f6-168">Un nuovo `asp:AdminManagerEnabled` chiave è disponibile per il `<appSettings>` elemento il *Web. config* file, che consente di disabilitare completamente la gestione dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="920f6-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="920f6-169">Il valore predefinito per questo elemento è true, il che significa che se non è incluso nel *Web. config* file, la gestione dei pacchetti è abilitato.</span><span class="sxs-lookup"><span data-stu-id="920f6-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="920f6-170">Per disabilitare la gestione dei pacchetti, aggiungere il seguente elemento di *Web. config* file nella radice del sito Web:</span><span class="sxs-lookup"><span data-stu-id="920f6-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a><span data-ttu-id="920f6-171">Modifiche</span><span class="sxs-lookup"><span data-stu-id="920f6-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="920f6-172">Modifica: tasto "webPages:AdminFolderVirtualPath" rinominato in "asp: AdminFolderVirtualPath"</span><span class="sxs-lookup"><span data-stu-id="920f6-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="920f6-173">Il `webPages:AdminFolderVirtualPath` chiave che può essere aggiunti al *Web. config* file per specificare il percorso di package manager è stato rinominato per utilizzare il `asp:` dello spazio dei nomi anziché i `webPages` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="920f6-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="920f6-174">Se si utilizza questo elemento, è necessario rinominare la cartella nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="920f6-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a><span data-ttu-id="920f6-175">Problemi noti</span><span class="sxs-lookup"><span data-stu-id="920f6-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="920f6-176">Problema: Le password degli utenti di appartenenza non riconosciuti.</span><span class="sxs-lookup"><span data-stu-id="920f6-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="920f6-177">L'algoritmo per la creazione e l'archiviazione delle password di appartenenza (accesso) è stato modificato per essere più sicura.</span><span class="sxs-lookup"><span data-stu-id="920f6-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="920f6-178">Di conseguenza, le password archiviate per i membri (utenti) creati in versioni Beta di ASP.NET Razor non verrà riconosciute.</span><span class="sxs-lookup"><span data-stu-id="920f6-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="920f6-179">**Soluzione alternativa** se il sito non è ancora stato inserito nell'ambiente di produzione, è possibile rimuovere i record utente dal database di appartenenza.</span><span class="sxs-lookup"><span data-stu-id="920f6-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="920f6-180">Se il database è in tempo reale, è necessario rigenerare a livello di codice le password esistenti nel database delle appartenenze.</span><span class="sxs-lookup"><span data-stu-id="920f6-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="920f6-181">Problema: Un comportamento imprevisto quando si utilizza una tabella utente personalizzato per l'appartenenza</span><span class="sxs-lookup"><span data-stu-id="920f6-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="920f6-182">Per inizializzare il provider di appartenenze per un sito Web ASP.NET Razor, si chiama il `WebSecurity.InitializeDatabaseConnection` metodo.</span><span class="sxs-lookup"><span data-stu-id="920f6-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="920f6-183">(In WebMatrix, il modello Starter Site include una chiamata al metodo nel  *\_AppStart.cshtml* file.) Se il `autoCreateTables` parametro di questo metodo è impostato su true (per impostazione predefinita, viene impostata su true nel modello Starter Site), e se un nome di tabella non riconosciuto viene passato al metodo (il secondo parametro), il metodo non genera un errore.</span><span class="sxs-lookup"><span data-stu-id="920f6-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="920f6-184">Al contrario, viene creato automaticamente nella tabella.</span><span class="sxs-lookup"><span data-stu-id="920f6-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="920f6-185">Può trattarsi di un problema, se si prevede di utilizzare una tabella utente personalizzata per l'appartenenza ma passare il nome della tabella non corretto per il `WebSecurity.InitializeDatabaseConnection` metodo.</span><span class="sxs-lookup"><span data-stu-id="920f6-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="920f6-186">Poiché il metodo per impostazione predefinita genera un errore se la tabella specificata non esiste, e invece creata una nuova tabella, può sembrare che l'applicazione si sta lavorando.</span><span class="sxs-lookup"><span data-stu-id="920f6-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="920f6-187">Tuttavia, il codice dell'applicazione che si basa sulla tabella utente personalizzata (e sui campi in essa contenuti) può avere esito negativo con errori imprevisti.</span><span class="sxs-lookup"><span data-stu-id="920f6-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="920f6-188">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-188">**Workaround**</span></span>  
> <span data-ttu-id="920f6-189">Assicurarsi che il nome passato il `InitializeDatabaseConnection` corrispondenze metodo tabella nel database delle appartenenze, il profilo utente o assicurarsi che il `autoCreateTables` parametro è impostato su false.</span><span class="sxs-lookup"><span data-stu-id="920f6-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="920f6-190">: Problema Messaggio di errore "il modulo di amministrazione richiede l'accesso a ~/App\_dati"</span><span class="sxs-lookup"><span data-stu-id="920f6-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="920f6-191">In alcuni casi, durante la creazione di utenti o in altro modo il sistema di appartenenze ASP.NET può determinare la pagina visualizzare l'errore *il modulo di amministrazione richiede l'accesso a ~/App\_dati*.</span><span class="sxs-lookup"><span data-stu-id="920f6-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="920f6-192">Questo errore si verifica se è in esecuzione IIS o IIS Express con l'account non dispone delle autorizzazioni per creare e scrivere il *App\_dati* cartella nella radice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="920f6-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="920f6-193">**Soluzione alternativa** creare manualmente un *App\_dati* cartella per il sito Web.</span><span class="sxs-lookup"><span data-stu-id="920f6-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="920f6-194">Assicurarsi che l'account di Windows a cui viene eseguito l'applicazione (in genere un servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio App\_dati.</span><span class="sxs-lookup"><span data-stu-id="920f6-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="920f6-195">Informazioni più dettagliate sono disponibili nell'articolo della Knowledge Base [problemi istanze utente di SQL Server Express e progetti di applicazione Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="920f6-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="920f6-196">Errore: problema "Impossibile generare un'istanza utente di SQL Server"</span><span class="sxs-lookup"><span data-stu-id="920f6-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="920f6-197">Se un'applicazione Web di WebMatrix utilizza SQL Server Express è in esecuzione IIS 7.5 in Windows 7 o Windows Server 2008 R2, si potrebbe essere visualizzato un errore che indica che SQL Server non è possibile recuperare il percorso dell'applicazione locale dell'utente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="920f6-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="920f6-198">**Soluzione alternativa** assicurarsi che l'account di Windows a cui viene eseguito l'applicazione (in genere un servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio *App\_dati*.</span><span class="sxs-lookup"><span data-stu-id="920f6-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="920f6-199">Informazioni più dettagliate sono disponibili nell'articolo della Knowledge Base [problemi istanze utente di SQL Server Express e progetti di applicazione Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="920f6-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="920f6-200">Problema: I file che contiene le risorse di gestione di pacchetti o le password di gestione di pacchetti sono servable in IIS 6.0 e versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="920f6-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="920f6-201">Se si distribuisce un'applicazione ASP.NET Web Pages (Razor) che è stata compilata utilizzando la versione RC2, e se l'applicazione contiene un *password.txt* o *packagesources.txt* file */App\_ Dati/admin*, IIS 6.0 verrà utilizzato il file, se richiesto, può esporre le password per l'istanza di gestione del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="920f6-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="920f6-202">**Soluzione alternativa** rinominare il *password.txt* o *packagesources.txt* file *password.config* o *packagesources.config*. Per impostazione predefinita, IIS 6.0 non serve i file con il *config* estensione.</span><span class="sxs-lookup"><span data-stu-id="920f6-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="920f6-203">(In IIS 7, nessun file di *App\_dati* cartella vengono soddisfatte, pertanto non è necessario rinominare i file.)</span><span class="sxs-lookup"><span data-stu-id="920f6-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="920f6-204">Problema: I pacchetti installati utilizzando la versione Beta 3 di disinstallazione non rimuovere completamente i componenti del pacchetto</span><span class="sxs-lookup"><span data-stu-id="920f6-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="920f6-205">Se è installato un pacchetto tramite Gestione pacchetti nella versione Beta 3 e quindi provare a disinstallare utilizzando la versione corrente, il pacchetto non è completamente disinstallato.</span><span class="sxs-lookup"><span data-stu-id="920f6-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="920f6-206">Utilizzando la gestione di pacchetti **Disinstalla** pulsante consente di rimuovere alcuni componenti, ma lascia il codice di libreria del pacchetto e non aggiorna il *package.config* file.</span><span class="sxs-lookup"><span data-stu-id="920f6-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="920f6-207">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="920f6-207">**Workaround** </span></span>  
> <span data-ttu-id="920f6-208">Eseguire questi passaggi:</span><span class="sxs-lookup"><span data-stu-id="920f6-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="920f6-209">Eliminare il *App\_Data\packages* cartella.</span><span class="sxs-lookup"><span data-stu-id="920f6-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="920f6-210">Questa operazione rimuove tutti i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="920f6-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="920f6-211">Eliminare il *Packages* file nella radice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="920f6-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="920f6-212">Problema: In Visual Studio, richiamare la gestione dei pacchetti basata sul web richiede l'applicazione non in linea</span><span class="sxs-lookup"><span data-stu-id="920f6-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="920f6-213">Se si lavora in Visual Studio (non WebMatrix) e utilizzare il  *\_admin* funzionalità per la gestione di pacchetti, Visual Studio di avviare l'applicazione viene portata offline e registra il *app\_ offline.htm* nella radice del sito Web, che interferisce con la possibilità di utilizzare la gestione dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="920f6-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="920f6-214">Sebbene questo comportamento si vedrà più di frequente quando si utilizza l'interfaccia di gestione basato su web pacchetto, lo stesso comportamento si verifica se aggiungere, rimuovere o modificare i file di *App\_dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="920f6-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="920f6-215">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="920f6-215">**Workaround** </span></span>  
> <span data-ttu-id="920f6-216">Per utilizzare i pacchetti in Visual Studio, utilizzare l'estensione NuGet anziché la gestione dei pacchetti basata sul web.</span><span class="sxs-lookup"><span data-stu-id="920f6-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="920f6-217">Per informazioni, vedere il [documentazione di NuGet](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="920f6-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="920f6-218">Se si lavora con altri file di *App\_dati* cartella, si consiglia di conservare i file in un' posizione per evitare questo problema.</span><span class="sxs-lookup"><span data-stu-id="920f6-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="920f6-219">Se non è pratico, eliminare il *app\_offline.htm* file manualmente o attendere che il sito torna online automaticamente (impostazione predefinita, dopo 30 secondi).</span><span class="sxs-lookup"><span data-stu-id="920f6-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="920f6-220">Problema: Visual Studio IntelliSense e progetto modelli disponibili solo in ASP.NET MVC versione 3</span><span class="sxs-lookup"><span data-stu-id="920f6-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="920f6-221">L'installazione di ASP.NET Web Pages non installare anche gli strumenti per Visual Studio, ad esempio i modelli di progetto e IntelliSense per le applicazioni ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="920f6-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="920f6-222">**Soluzione alternativa** per utilizzare i modelli di progetto e IntelliSense per le applicazioni di pagine Web ASP.NET in Visual Studio, installare ASP.NET MVC 3 RC tramite l'installazione guidata piattaforma Web o [programma di installazione autonomo](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="920f6-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="920f6-223">Problema: Lettura feed o altri dati esterni tramite un server proxy</span><span class="sxs-lookup"><span data-stu-id="920f6-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="920f6-224">Se il server che esegue il sito è un server proxy, potrebbe essere necessario configurare le informazioni sul proxy nel *Web. config* file per poter essere in grado di leggere informazioni proveniente dall'esterno del sito.</span><span class="sxs-lookup"><span data-stu-id="920f6-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="920f6-225">Ad esempio, se si utilizza il `ReCaptcha` helper, il supporto comunica con il servizio reCAPTCHA, ma potrebbe essere bloccato da server proxy.</span><span class="sxs-lookup"><span data-stu-id="920f6-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="920f6-226">Analogamente, feed che vengono utilizzati in ASP.NET Web Pages, ad esempio il feed utilizzato dal gestore del pacchetto, potrebbe richiedere la configurazione del proxy.</span><span class="sxs-lookup"><span data-stu-id="920f6-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="920f6-227">Se si verificano problemi nell'utilizzo di un servizio esterno o l'utilizzo con il feed del pacchetto, è possibile inserire gli elementi seguenti nella directory principale dell'applicazione *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="920f6-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="920f6-228">Per ulteriori informazioni sulla configurazione di un server proxy, vedere [ &lt;proxy&gt; elemento (impostazioni di rete)](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) nel sito Web MSDN.</span><span class="sxs-lookup"><span data-stu-id="920f6-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="920f6-229">Problema: Disinstallazione di .NET Framework versione 4 Disabilita ASP.NET Web Pages con sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="920f6-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="920f6-230">Se si disinstalla .NET Framework versione 4 e quindi reinstallarlo, ASP.NET Web Pages con sintassi Razor è disabilitato.</span><span class="sxs-lookup"><span data-stu-id="920f6-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="920f6-231">Pagine con il *. cshtml* estensione non vengono eseguite correttamente.</span><span class="sxs-lookup"><span data-stu-id="920f6-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="920f6-232">ASP.NET Web Pages registra un assembly nella directory principale macchina *Web. config* file e la rimozione di .NET Framework consente di rimuovere tale file.</span><span class="sxs-lookup"><span data-stu-id="920f6-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="920f6-233">Reinstallare .NET Framework viene installata una nuova versione del file di configurazione, ma non aggiunge il riferimento per l'assembly di ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="920f6-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="920f6-234">**Soluzione alternativa** dopo la reinstallazione di .NET Framework, reinstallare ASP.NET Web Pages con sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="920f6-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="920f6-235">Aggiunge l'elemento seguente per il *Web. config* file nella radice della macchina, ovvero in genere nel percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="920f6-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="920f6-236">Problema: URL senza estensione non si trova il file.cshtml/.vbhtml in IIS 7 o IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="920f6-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="920f6-237">In IIS 7 o IIS 7.5, le richieste con un URL simile al seguente non sono in grado di trovare le pagine che hanno il *. cshtml* o *. vbhtml* estensione:</span><span class="sxs-lookup"><span data-stu-id="920f6-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
>   
> `http://www.example.com/ExampleSite/ExampleFile`  
>   
> <span data-ttu-id="920f6-238">Il problema si verifica perché la riscrittura URL non è abilitata per impostazione predefinita per IIS 7 o IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="920f6-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="920f6-239">Lo scenario probabile che è che il problema non viene visualizzato durante il test in locale utilizzando IIS Express, ma si verificano quando si distribuisce il sito Web in un sito Web di hosting.</span><span class="sxs-lookup"><span data-stu-id="920f6-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="920f6-240">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="920f6-241">Se si dispone di un controllo sul computer server, nel computer del server, installare l'aggiornamento descritto in [è disponibile che consente di determinati gestori di IIS 7.0 o IIS 7.5 di gestire le richieste il cui URL non terminano con un periodo di un aggiornamento](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="920f6-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="920f6-242">Se non è un controllo sul computer server (ad esempio, si distribuisce in un sito di hosting Web), aggiungere quanto segue per il sito Web *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="920f6-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="920f6-243">Problema: Distribuire un'applicazione in un computer che non dispone di SQL Server Compact è installato</span><span class="sxs-lookup"><span data-stu-id="920f6-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="920f6-244">Le applicazioni che includono database di SQL Server Compact è possono eseguire in un computer in cui SQL Server Compact non è installato.</span><span class="sxs-lookup"><span data-stu-id="920f6-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="920f6-245">Microsoft WebMatrix 1.0 automaticamente copia i file binari per l'utente ed esegue appropriata *Web. config* file trasformazioni.</span><span class="sxs-lookup"><span data-stu-id="920f6-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="920f6-246">**Soluzione alternativa** se è necessario copiare questi file, assicurarsi di *Web. config* modifiche al file manualmente, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="920f6-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="920f6-247">Copiare gli assembly del motore di database per il *Bin* (cartelle e sottocartelle) dell'applicazione nel computer di destinazione:</span><span class="sxs-lookup"><span data-stu-id="920f6-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>     - <span data-ttu-id="920f6-248">Copia *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="920f6-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>         <span data-ttu-id="920f6-249">**per** *\bin.*</span><span class="sxs-lookup"><span data-stu-id="920f6-249">**to** *\Bin*</span></span>
>     - <span data-ttu-id="920f6-250">Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\* * * per***\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="920f6-250">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\****to***\Bin\x86*</span></span>
>     - <span data-ttu-id="920f6-251">Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **a***\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="920f6-251">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **to***\Bin\amd64*</span></span>
> 2. <span data-ttu-id="920f6-252">Nella cartella radice del sito Web, creare o aprire un *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="920f6-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="920f6-253">(In WebMatrix 1.0, è disponibile se si fa clic su questo tipo di file **tutti** nel **scegliere un tipo di File** la finestra di dialogo.)</span><span class="sxs-lookup"><span data-stu-id="920f6-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="920f6-254">Aggiungere il seguente elemento come figlio del `<configuration>` elemento (non all'interno di `<system.web>` elemento):</span><span class="sxs-lookup"><span data-stu-id="920f6-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="920f6-255">Problema: "Database" e "WebGrid" helper non funzionano in attendibilità media in Visual Basic</span><span class="sxs-lookup"><span data-stu-id="920f6-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="920f6-256">Se si utilizza Visual Basic (creazione *. vbhtml* file), il `Database` e `WebGrid` helper non funzionerà se l'applicazione è impostato per utilizzare l'attendibilità Media.</span><span class="sxs-lookup"><span data-stu-id="920f6-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="920f6-257">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-257">**Workaround**</span></span>  
> <span data-ttu-id="920f6-258">Se si utilizza Visual Studio 2010, è possibile risolvere il problema installando la versione del Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="920f6-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="920f6-259">Fino a quando non è disponibile la versione finale della versione SP1, è possibile scaricare la versione Beta di SP1 dal [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) pagina Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="920f6-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="920f6-260">Se questo non è pratico, o se non si utilizza Visual Studio 2010, è possibile temporaneamente impostare l'applicazione di usare l'attendibilità totale.</span><span class="sxs-lookup"><span data-stu-id="920f6-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="920f6-261">Problema: "ApplicationPart" risorse sono accessibili dall'esterno</span><span class="sxs-lookup"><span data-stu-id="920f6-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="920f6-262">Se un assembly contiene gli oggetti da cui deriva il `ApplicationPart` che le risorse dell'assembly vengono esposti dalla classe di `ResourceRouteHandler` classe.</span><span class="sxs-lookup"><span data-stu-id="920f6-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="920f6-263">Si consideri ad esempio l'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="920f6-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="920f6-264">La richiesta di download di tutte le stringhe di risorsa nel *System.Web.WebPages.Administration.dll* assembly.</span><span class="sxs-lookup"><span data-stu-id="920f6-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="920f6-265">Tutte le risorse incorporate, anche quelli che non sono destinati a essere utilizzato come contenuto statico, vengono scaricate.</span><span class="sxs-lookup"><span data-stu-id="920f6-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="920f6-266">Se le risorse incorporate contengono informazioni riservate, questo può rappresentare un rischio per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="920f6-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="920f6-267">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="920f6-267">**Workaround** </span></span>  
> <span data-ttu-id="920f6-268">Se si crea un **ApplicationPart** dell'oggetto, assicurarsi che le risorse incorporate associato che **ApplicationPart** assembly dell'oggetto non contengono informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="920f6-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="920f6-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="920f6-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="920f6-270">Per informazioni sui problemi di installazione per WebMatrix, vedere [problemi di installazione di WebMatrix](#Known_Issues_Installation) più indietro in questo documento.</span><span class="sxs-lookup"><span data-stu-id="920f6-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="920f6-271">In questa sezione del documento vengono descritti i problemi noti per l'ambiente di sviluppo WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="920f6-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="920f6-272">Problema: Il nome utente o password di una stringa di connessione di database in un file Web. config non vengono riflesse nell'area di lavoro database</span><span class="sxs-lookup"><span data-stu-id="920f6-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="920f6-273">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="920f6-274">Nel *Web. config* file, modificare il nome del database nella stringa di connessione (ad esempio, aggiungervi "1").</span><span class="sxs-lookup"><span data-stu-id="920f6-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="920f6-275">Salvare il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="920f6-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="920f6-276">Fare clic su **database** e aggiornare.</span><span class="sxs-lookup"><span data-stu-id="920f6-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="920f6-277">Modificare il nome del database nella stringa di connessione nel *Web. config* file il nome del database originale.</span><span class="sxs-lookup"><span data-stu-id="920f6-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="920f6-278">Salvare il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="920f6-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="920f6-279">Fare clic su **database** e aggiornare.</span><span class="sxs-lookup"><span data-stu-id="920f6-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="920f6-280">Problema: Impossibile eliminare le cartelle create da WebMatrix</span><span class="sxs-lookup"><span data-stu-id="920f6-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="920f6-281">Se WebMatrix è in esecuzione con autorizzazioni elevate (vale a dire, iniziare a utilizzare WebMatrix il **Esegui come amministratore** opzione in Windows), Impossibile eliminare le cartelle create da WebMatrix utilizzando Esplora risorse.</span><span class="sxs-lookup"><span data-stu-id="920f6-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="920f6-282">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-282">**Workaround**</span></span>  
> <span data-ttu-id="920f6-283">Eseguire Esplora risorse di Windows con autorizzazioni elevate.</span><span class="sxs-lookup"><span data-stu-id="920f6-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="920f6-284">Attenersi ai passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="920f6-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="920f6-285">In Windows, fare clic su **avviare**.</span><span class="sxs-lookup"><span data-stu-id="920f6-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="920f6-286">Immettere "Esplora" e fare clic sulla voce per **Esplora**.</span><span class="sxs-lookup"><span data-stu-id="920f6-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="920f6-287">Fare clic su **Esegui come amministratore**.</span><span class="sxs-lookup"><span data-stu-id="920f6-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="920f6-288">È quindi possibile eliminare le cartelle.</span><span class="sxs-lookup"><span data-stu-id="920f6-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="920f6-289">Problema: WebMatrix 1.0 non è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi</span><span class="sxs-lookup"><span data-stu-id="920f6-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="920f6-290">WebMatrix 1.0 non è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi, ad esempio l'installazione di componenti aggiuntivi nelle situazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="920f6-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="920f6-291">In Windows Vista o Windows 7, si è connessi con un account che dispone di privilegi amministrativi e il controllo dell'Account utente (UAC) è disabilitata.</span><span class="sxs-lookup"><span data-stu-id="920f6-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="920f6-292">Si utilizza Microsoft Windows XP o Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="920f6-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="920f6-293">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-293">**Workaround**</span></span>  
> <span data-ttu-id="920f6-294">La maggior parte delle attività in WebMatrix 1.0 non richiedono autorizzazioni amministrative.</span><span class="sxs-lookup"><span data-stu-id="920f6-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="920f6-295">Se sono presenti, è possibile eseguire l'operazione come amministratore o eseguire la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="920f6-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="920f6-296">In Windows Vista o Windows 7, abilitare il controllo dell'account utente.</span><span class="sxs-lookup"><span data-stu-id="920f6-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="920f6-297">In Windows XP, aggiungere l'utente al gruppo di sicurezza Administrators.</span><span class="sxs-lookup"><span data-stu-id="920f6-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="920f6-298">Problema: "Del sito dalla raccolta di Web" è disabilitata</span><span class="sxs-lookup"><span data-stu-id="920f6-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="920f6-299">Il **sito dalla raccolta Web** opzione è disabilitata se l'installazione guidata piattaforma Web 3.0 non è installato.</span><span class="sxs-lookup"><span data-stu-id="920f6-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="920f6-300">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-300">**Workaround**</span></span>  
> <span data-ttu-id="920f6-301">Installare il [installazione guidata piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="920f6-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="920f6-302">Problema: Google Chrome non è disponibile come opzione di esecuzione</span><span class="sxs-lookup"><span data-stu-id="920f6-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="920f6-303">Google Chrome non viene visualizzata nell'elenco dei browser in **eseguire** sul **Home** scheda.</span><span class="sxs-lookup"><span data-stu-id="920f6-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="920f6-304">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-304">**Workaround**</span></span>  
> <span data-ttu-id="920f6-305">Alcune versioni di Google Chrome non effettuano la registrazione in modo corretto con la funzionalità di programmi predefiniti di Windows.</span><span class="sxs-lookup"><span data-stu-id="920f6-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="920f6-306">In alternativa, avviare Google Chrome, scegliere il *personalizzare e controllo Google Chrome* menu, fare clic su *opzioni*e quindi fare clic su *verificare Google Chrome il browser predefinito*.</span><span class="sxs-lookup"><span data-stu-id="920f6-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="920f6-307">Problema: La finestra di dialogo "Foreign Key" non consente l'immissione di una chiave primaria</span><span class="sxs-lookup"><span data-stu-id="920f6-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="920f6-308">Il **Foreign Key** la finestra di dialogo non consente di immettere il nome della chiave primario della tabella di chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="920f6-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="920f6-309">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-309">**Workaround**</span></span>  
> <span data-ttu-id="920f6-310">Ciò è intenzionale.</span><span class="sxs-lookup"><span data-stu-id="920f6-310">This is intentional.</span></span> <span data-ttu-id="920f6-311">Non è necessario immettere il nome della chiave primaria della tabella della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="920f6-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="920f6-312">Problema: IntelliSense non è disponibile in WebMatrix per Razor sintassi c# o Visual Basic</span><span class="sxs-lookup"><span data-stu-id="920f6-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="920f6-313">IntelliSense è supportato in WebMatrix per HTML e CSS.</span><span class="sxs-lookup"><span data-stu-id="920f6-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="920f6-314">Non è tuttavia disponibile per altri linguaggi.</span><span class="sxs-lookup"><span data-stu-id="920f6-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="920f6-315">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="920f6-315">**Workaround** </span></span>  
> <span data-ttu-id="920f6-316">Nessuno.</span><span class="sxs-lookup"><span data-stu-id="920f6-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="920f6-317">Problema: IntelliSense per HTML e CSS suggerisce gli elementi che non sono appropriati in base al contesto</span><span class="sxs-lookup"><span data-stu-id="920f6-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="920f6-318">IntelliSense per il markup in WebMatrix supporta l'HTML usando il [schema XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) e l'utilizzo di CSS di [dello schema CSS 2.1](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="920f6-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="920f6-319">Perché IntelliSense è basato su tali schemi specifici, determinati tag, attributi o proprietà potrebbero suggerite che non sono appropriate per la definizione di stile o di pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="920f6-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="920f6-320">Per HTML, può anche provocare ai suggerimenti imprevisti nel contenuto che potrebbe essere interpretata come XHTML in formato non valido (ad esempio, quando i tag non chiusi).</span><span class="sxs-lookup"><span data-stu-id="920f6-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="920f6-321">Questo problema potrebbe essere più evidente se il punto di inserimento si trova all'interno di un tag incompleto; In tal caso, IntelliSense potrebbe suggerire nuovi tag di apertura o altri suggerimenti non corretto.</span><span class="sxs-lookup"><span data-stu-id="920f6-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="920f6-322">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="920f6-322">**Workaround** </span></span>  
> <span data-ttu-id="920f6-323">Per HTML, assicurarsi che l'utilizzo all'interno di una pagina XHTML completezza in formato corretto.</span><span class="sxs-lookup"><span data-stu-id="920f6-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="920f6-324">Per CSS, è disponibile alcuna soluzione.</span><span class="sxs-lookup"><span data-stu-id="920f6-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="920f6-325">Problema: IntelliSense non è stato richiamato durante la digitazione</span><span class="sxs-lookup"><span data-stu-id="920f6-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="920f6-326">In alcuni casi, IntelliSense potrebbe non richiamato come HTML o CSS vengano immessi nell'editor.</span><span class="sxs-lookup"><span data-stu-id="920f6-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="920f6-327">In particolare, questo può verificarsi quando il punto di inserimento è direttamente accanto a un altro elemento o alla fine di un file.</span><span class="sxs-lookup"><span data-stu-id="920f6-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="920f6-328">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="920f6-328">**Workaround** </span></span>  
> <span data-ttu-id="920f6-329">Assicurarsi che vi sia uno spazio vuoto attorno al punto di inserimento e che il punto di inserimento non è alla fine di un file.</span><span class="sxs-lookup"><span data-stu-id="920f6-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="920f6-330">È anche possibile richiamare IntelliSense manualmente premendo Ctrl + barra spaziatrice.</span><span class="sxs-lookup"><span data-stu-id="920f6-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="920f6-331">Problema: Alcuna interfaccia utente non è disponibile per la disabilitazione di IntelliSense</span><span class="sxs-lookup"><span data-stu-id="920f6-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="920f6-332">WebMatrix 1.0 non fornisce alcuna interfaccia utente o movimento per la disabilitazione di IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="920f6-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="920f6-333">**Soluzione alternativa** </span><span class="sxs-lookup"><span data-stu-id="920f6-333">**Workaround** </span></span>  
> <span data-ttu-id="920f6-334">Avviare WebMatrix mediante il comando seguente, che include un'opzione che disabilita IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="920f6-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="920f6-335">IIS Express</span><span class="sxs-lookup"><span data-stu-id="920f6-335">IIS Express</span></span>

<span data-ttu-id="920f6-336">IIS Express è il proprio file Leggimi, che è disponibile all'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="920f6-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="920f6-337">https://go.microsoft.com/fwlink/?LinkId=207675&amp;clcid = 0x409</span><span class="sxs-lookup"><span data-stu-id="920f6-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="920f6-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="920f6-338">SQL Server Compact</span></span>

<span data-ttu-id="920f6-339">SQL Server Compact è il proprio file Leggimi, che è disponibile all'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="920f6-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="920f6-340">https://go.microsoft.com/fwlink/?LinkId=208545</span><span class="sxs-lookup"><span data-stu-id="920f6-340">https://go.microsoft.com/fwlink/?LinkID=208545</span></span>](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="920f6-341">Per informazioni sui problemi che comportano l'installazione di SQL Server Compact come parte di WebMatrix, vedere [problemi di installazione di WebMatrix](#Known_Issues_Installation) più indietro in questo documento.</span><span class="sxs-lookup"><span data-stu-id="920f6-341">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a><span data-ttu-id="920f6-342">L'installazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="920f6-342">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="920f6-343">Problema: L'installazione di un'applicazione può richiedere molto tempo se cartella documenti dell'utente viene reindirizzata a una condivisione di rete</span><span class="sxs-lookup"><span data-stu-id="920f6-343">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="920f6-344">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-344">**Workaround**</span></span>  
> <span data-ttu-id="920f6-345">Nessuno.</span><span class="sxs-lookup"><span data-stu-id="920f6-345">None.</span></span> <span data-ttu-id="920f6-346">L'applicazione potrebbe richiedere qualche istante per l'installazione, ma verrà installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="920f6-346">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a><span data-ttu-id="920f6-347">Pubblicazione di applicazioni</span><span class="sxs-lookup"><span data-stu-id="920f6-347">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="920f6-348">Problema: "richiesto non è possibile acquisire le autorizzazioni" Errore durante la pubblicazione di un Database di SQL Compact</span><span class="sxs-lookup"><span data-stu-id="920f6-348">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="920f6-349">WebMatrix non supporta completamente la distribuzione di file binari del supporto per SQL Server Compact a un server che esegue .NET Framework versione 3.5 con una configurazione a livello di attendibilità medio.</span><span class="sxs-lookup"><span data-stu-id="920f6-349">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="920f6-350">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-350">**Workaround**</span></span>  
> <span data-ttu-id="920f6-351">La soluzione migliore consiste nell'installare .NET Framework 4 nel server.</span><span class="sxs-lookup"><span data-stu-id="920f6-351">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="920f6-352">In alternativa, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="920f6-352">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="920f6-353">Aggiungere i seguenti elementi per il `SecurityClasses` sezione *Web\_MediumTrust.config* file:</span><span class="sxs-lookup"><span data-stu-id="920f6-353">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="920f6-354">Creare un nuovo set di autorizzazioni nel *Web\_MediumTrust.config* file con le autorizzazioni necessarie seguenti:</span><span class="sxs-lookup"><span data-stu-id="920f6-354">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="920f6-355">Applicare l'autorizzazione impostata su SQL Server Compact inserisce gli elementi seguenti nel *Web\_MediumTrust.config* file:</span><span class="sxs-lookup"><span data-stu-id="920f6-355">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="920f6-356">Problema: Le applicazioni web della raccolta e PhpBB visualizzano un errore "Servizio non è disponibile" dopo la pubblicazione</span><span class="sxs-lookup"><span data-stu-id="920f6-356">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="920f6-357">In alcune circostanze, la pubblicazione di un'applicazione genera un errore di "servizio non è disponibile".</span><span class="sxs-lookup"><span data-stu-id="920f6-357">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="920f6-358">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-358">**Workaround**</span></span>  
> <span data-ttu-id="920f6-359">In WebMatrix, aggiungere una barra rovesciata (\) alla fine del nome del server nel **impostazioni di pubblicazione** finestra e quindi pubblicare nuovamente l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="920f6-359">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="920f6-360">Problema: I collegamenti e il layout del sito Web di Moodle vengono interrotti dopo la pubblicazione</span><span class="sxs-lookup"><span data-stu-id="920f6-360">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="920f6-361">Dopo la pubblicazione di un'applicazione di Moodle, l'applicazione non funziona correttamente.</span><span class="sxs-lookup"><span data-stu-id="920f6-361">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="920f6-362">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-362">**Workaround**</span></span>  
> <span data-ttu-id="920f6-363">In WebMatrix, aggiungere una barra (/) alla fine del **nome sito** campo il **impostazioni di pubblicazione** finestra e quindi pubblicare nuovamente l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="920f6-363">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="920f6-364">Problema: La pubblicazione nopCommerce viene generato un errore di database</span><span class="sxs-lookup"><span data-stu-id="920f6-364">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="920f6-365">NopCommerce pubblicazione ha esito negativo e restituisce un errore di database come "inserire la nop\_tabella del log non è riuscita."</span><span class="sxs-lookup"><span data-stu-id="920f6-365">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="920f6-366">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-366">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="920f6-367">In WebMatrix, fare clic su **eseguire** per avviare nopCommerce localmente.</span><span class="sxs-lookup"><span data-stu-id="920f6-367">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="920f6-368">Accedere alla pagina Amministrazione.</span><span class="sxs-lookup"><span data-stu-id="920f6-368">Log into the administration page.</span></span>
> 3. <span data-ttu-id="920f6-369">Fare clic su di **sistema** menu.</span><span class="sxs-lookup"><span data-stu-id="920f6-369">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="920f6-370">Fare clic su di **Log** opzione.</span><span class="sxs-lookup"><span data-stu-id="920f6-370">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="920f6-371">Fare clic su di **Cancella Log** pulsante.</span><span class="sxs-lookup"><span data-stu-id="920f6-371">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="920f6-372">Pubblicare nuovamente nopCommerce.</span><span class="sxs-lookup"><span data-stu-id="920f6-372">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="920f6-373">Problema: Silverstripe CMS viene visualizzato un "500 PHP FCGI errore HTTP" quando si scarica un sito pubblicato</span><span class="sxs-lookup"><span data-stu-id="920f6-373">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="920f6-374">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-374">**Workaround**</span></span>  
> <span data-ttu-id="920f6-375">Dopo aver fatto clic **Download pubblicazione sito**, ignorare `silverstripe-cache/manifest_main` in **Anteprima pubblicazione**.</span><span class="sxs-lookup"><span data-stu-id="920f6-375">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="920f6-376">Questo file viene utilizzato per la memorizzazione ed è specifico per ogni computer.</span><span class="sxs-lookup"><span data-stu-id="920f6-376">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="920f6-377">Problema: Subtext viene visualizzato "Errore Server nell'applicazione '/'" quando si scarica un sito pubblicato</span><span class="sxs-lookup"><span data-stu-id="920f6-377">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="920f6-378">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-378">**Workaround**</span></span>  
> <span data-ttu-id="920f6-379">Aprire il sito *Web. config* file e sostituire l'ID utente e password nella stringa di connessione del database con le credenziali di amministratore di SQL Server (le credenziali "sa").</span><span class="sxs-lookup"><span data-stu-id="920f6-379">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="920f6-380">In alternativa, eseguire questi passaggi per concedere all'account utente si è connessi con `db_owner` autorizzazioni:</span><span class="sxs-lookup"><span data-stu-id="920f6-380">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="920f6-381">Installare SQL Server Management Studio utilizzando l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="920f6-381">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="920f6-382">Connettersi all'istanza locale di SQL Server Express (per impostazione predefinita, `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="920f6-382">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="920f6-383">Fare clic su **database** &gt; *[localSubtextDatabase]* &gt; **sicurezza** &gt; **utenti** &gt; *[localSubtextUser*] (valore predefinito è `subtextuser`], mouse e scegliere **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="920f6-383">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="920f6-384">Selezionare **db\_proprietario** nella sezione l'appartenenza al ruolo.</span><span class="sxs-lookup"><span data-stu-id="920f6-384">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="920f6-385">Problema: Sito potrebbe non funzionare dopo la pubblicazione se il campo "URL di destinazione" non è preceduto da http:// o https://</span><span class="sxs-lookup"><span data-stu-id="920f6-385">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="920f6-386">Nel **le impostazioni di pubblicazione** la finestra di dialogo, se l'URL di destinazione non inizia con `http://` o `https://`, il sito potrebbe non funzionare dopo la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="920f6-386">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="920f6-387">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-387">**Workaround**</span></span>  
> <span data-ttu-id="920f6-388">Verificare che prima di pubblicare un sito, l'URL di destinazione nel **impostazioni di pubblicazione** la finestra di dialogo inizia con `http://` o `https://`.</span><span class="sxs-lookup"><span data-stu-id="920f6-388">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="920f6-389">Problema: La pubblicazione di un database MySQL non riesce con l'errore "Impossibile pubblicare il database.</span><span class="sxs-lookup"><span data-stu-id="920f6-389">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="920f6-390">Questa situazione può verificarsi se il database remoto non è possibile eseguire lo script."</span><span class="sxs-lookup"><span data-stu-id="920f6-390">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="920f6-391">L'errore può verificarsi per diversi motivi.</span><span class="sxs-lookup"><span data-stu-id="920f6-391">The error can occur for a number of reasons.</span></span> <span data-ttu-id="920f6-392">Un motivo per cui che è possibile visualizzare questo errore è se lo script del database contiene un carattere di virgoletta singola (') e il set di caratteri predefinito del database MySQL di destinazione non è presente in UTF-8.</span><span class="sxs-lookup"><span data-stu-id="920f6-392">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="920f6-393">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-393">**Workaround**</span></span>  
> <span data-ttu-id="920f6-394">Impostare il carattere predefinito impostato per il database MySQL remoto su UTF-8.</span><span class="sxs-lookup"><span data-stu-id="920f6-394">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="920f6-395">Problema: Alcuni collegamenti non sono visibili in DotNetNuke dopo la pubblicazione o il sito di download</span><span class="sxs-lookup"><span data-stu-id="920f6-395">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="920f6-396">Se si pubblica o scarica un sito di DotNetNuke, si potrebbe essere necessario cancellare la cache per ottenere i nuovi collegamenti vengano visualizzati nel sito.</span><span class="sxs-lookup"><span data-stu-id="920f6-396">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="920f6-397">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-397">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="920f6-398">Accedere come "Host".</span><span class="sxs-lookup"><span data-stu-id="920f6-398">Log in as "Host".</span></span>
> 2. <span data-ttu-id="920f6-399">Passare al menu di host e selezionare **impostazioni Host**.</span><span class="sxs-lookup"><span data-stu-id="920f6-399">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="920f6-400">Scorrere verso il basso e in **impostazioni avanzate**, espandere **le impostazioni delle prestazioni**.</span><span class="sxs-lookup"><span data-stu-id="920f6-400">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="920f6-401">Fare clic su di **Cancella Cache** collegamento delle pagine.</span><span class="sxs-lookup"><span data-stu-id="920f6-401">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="920f6-402">Passare alla parte inferiore della pagina e riavviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="920f6-402">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="920f6-403">Problema: Alcuni collegamenti in AtomSite vengono interrotti dopo il download di un sito pubblicato</span><span class="sxs-lookup"><span data-stu-id="920f6-403">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="920f6-404">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-404">**Workaround**</span></span>  
> <span data-ttu-id="920f6-405">Nel *service.config* file, *users.config* file e tutti *XML* file, sostituire la stringa URL (ad esempio, `http://myhost.com/atomsite`) con quello locale (ad esempio, `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="920f6-405">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="920f6-406">Problema: Le applicazioni basate su MySQL WordPress non è possibile pubblicare e segnalare un errore di database</span><span class="sxs-lookup"><span data-stu-id="920f6-406">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="920f6-407">Per impostazione predefinita, WebMatrix installa MySQL con il set di caratteri UTF-8.</span><span class="sxs-lookup"><span data-stu-id="920f6-407">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="920f6-408">Se è possibile installare MySQL per conto proprio e il set di caratteri non UTF-8 (ad esempio, è Latin1), il processo di pubblicazione per i database potrebbe non riuscire.</span><span class="sxs-lookup"><span data-stu-id="920f6-408">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="920f6-409">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-409">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="920f6-410">Modificare il set di caratteri per MySQL UTF-8.</span><span class="sxs-lookup"><span data-stu-id="920f6-410">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="920f6-411">(Per informazioni dettagliate, vedere [Server Set di caratteri e le regole di confronto](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) sul sito Web di MySQL.)</span><span class="sxs-lookup"><span data-stu-id="920f6-411">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="920f6-412">Reinstallare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="920f6-412">Reinstall the application.</span></span>
> 3. <span data-ttu-id="920f6-413">Ripubblicare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="920f6-413">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="920f6-414">Problema: "Sito di Download pubblicato" non riesce per le applicazioni che dispongono di un'installazione basata su browser</span><span class="sxs-lookup"><span data-stu-id="920f6-414">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="920f6-415">Alcune applicazioni (ad esempio, Kentico CMS) è necessario avviarli nel browser per eseguire il programma di installazione di post-installazione, ad esempio la creazione di un database.</span><span class="sxs-lookup"><span data-stu-id="920f6-415">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="920f6-416">Se si pubblica un'applicazione simile al seguente senza completare l'installazione basata su browser, verrà eseguito il tentativo di scaricare lo stesso sito da un server remoto.</span><span class="sxs-lookup"><span data-stu-id="920f6-416">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="920f6-417">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-417">**Workaround**</span></span>  
> <span data-ttu-id="920f6-418">Completare l'installazione basata su browser prima di pubblicare il sito.</span><span class="sxs-lookup"><span data-stu-id="920f6-418">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="920f6-419">Problema: "Sito di Download pubblicato" viene generato un errore di database per DotNetNuke e Kooboo CMS</span><span class="sxs-lookup"><span data-stu-id="920f6-419">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="920f6-420">Se si tenta di scaricare un'applicazione da un server e si dispone delle credenziali nella stringa di connessione del database nel **impostazioni di pubblicazione** finestra di dialogo, si potrebbe vedere il seguente errore nel Registro di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="920f6-420">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="920f6-421">**Soluzione alternativa**</span><span class="sxs-lookup"><span data-stu-id="920f6-421">**Workaround**</span></span>  
> <span data-ttu-id="920f6-422">Se possibile, eseguire di nuovo il sito (o pubblicarlo) usando credenziali senza privilegi di amministratore per il database.</span><span class="sxs-lookup"><span data-stu-id="920f6-422">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="920f6-423">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="920f6-423">For More Information</span></span>

<span data-ttu-id="920f6-424">Per ulteriori informazioni su WebMatrix 1.0, vedere i siti Web seguenti:</span><span class="sxs-lookup"><span data-stu-id="920f6-424">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="920f6-425">IIS.net</span><span class="sxs-lookup"><span data-stu-id="920f6-425">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="920f6-426">ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="920f6-426">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="920f6-427">Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="920f6-427">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="920f6-428">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="920f6-428">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="920f6-429">Tutti i diritti riservati.</span><span class="sxs-lookup"><span data-stu-id="920f6-429">All Rights Reserved.</span></span> <span data-ttu-id="920f6-430">[Condizioni di utilizzo](https://msdn.microsoft.com/en-us/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="920f6-430">[Terms of Use](https://msdn.microsoft.com/en-us/cc300389.aspx).</span></span>
