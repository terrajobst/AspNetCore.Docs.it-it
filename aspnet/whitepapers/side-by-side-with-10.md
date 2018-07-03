---
uid: whitepapers/side-by-side-with-10
title: L'esecuzione di ASP.NET Side-by-Side di .NET Framework 1.0 e 1.1 | Microsoft Docs
author: rick-anderson
description: Questo white paper viene descritto come installare sia .NET 1.0 e 1.1 di .NET nel computer, consentendo un'applicazione Web ASP.NET per l'esecuzione in entrambe le versioni di con i fotogrammi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1b8bcebd59a900e54c759509fd4fc5ad1f4f8576
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391160"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="2fbcd-103">Esecuzione di ASP.NET Side-by-Side di .NET Framework 1.0 e 1.1</span><span class="sxs-lookup"><span data-stu-id="2fbcd-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="2fbcd-104">Questo white paper viene descritto come installare sia .NET 1.0 e 1.1 di .NET nel computer, consentendo un'applicazione Web ASP.NET per l'esecuzione in entrambe le versioni di framework.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="2fbcd-105">Si applica a ASP.NET 1.0 e ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="2fbcd-106">In ASP.NET, le applicazioni si parla di essere in esecuzione side-by-side quando vengono installati nello stesso computer, ma usare versioni diverse di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="2fbcd-107">L'argomento seguente descrive come configurare le applicazioni ASP.NET per l'esecuzione side-by-side e fornisce istruzioni dettagliate per:</span><span class="sxs-lookup"><span data-stu-id="2fbcd-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="2fbcd-108">Gestire mapping dell'applicazione Web a .NET Framework versione 1.0 durante l'installazione</span><span class="sxs-lookup"><span data-stu-id="2fbcd-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="2fbcd-109">Eseguire il mapping di un'applicazione Web a una versione specifica di .NET Framework</span><span class="sxs-lookup"><span data-stu-id="2fbcd-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="2fbcd-110">Trovare la versione di .NET Framework che utilizza un sito Web</span><span class="sxs-lookup"><span data-stu-id="2fbcd-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="2fbcd-111">In genere, quando un componente o un'applicazione viene aggiornata in un computer, la versione meno recente viene rimosso e sostituita con la versione più recente.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="2fbcd-112">Se la nuova versione non è compatibile con la versione precedente, questa operazione interrompe in genere altre applicazioni che usano il componente o applicazione.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="2fbcd-113">.NET Framework fornisce supporto per l'esecuzione side-by-side, che consente a più versioni di un assembly o applicazione da installare nello stesso computer contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="2fbcd-114">Poiché è possibile installare più versioni contemporaneamente, le applicazioni gestite possono selezionare la versione da usare senza influire sulle applicazioni che usano una versione diversa.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="2fbcd-115">Per impostazione predefinita, durante l'installazione di .NET Framework versione 1.1, tutte le applicazioni ASP.NET esistenti vengono riconfigurate automaticamente per usare la versione più recente di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="2fbcd-116">Se si preferisce che non le applicazioni ASP.NET per .NET Framework 1.1 per impostazione predefinita, fare clic su [qui](#1) per informazioni su come evitare questo problema durante l'installazione.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="2fbcd-117">Se si aggiorna il server Web per .NET Framework 1.1 e si desidera che uno o più applicazioni Web per l'esecuzione di .NET Framework 1.0, è necessario aggiornare il mapping di Script di Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="2fbcd-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="2fbcd-118">Il mapping di script è il meccanismo per eseguire il mapping di estensione di file con estensione aspx per un'applicazione Web specifica a una versione di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="2fbcd-119">Fare clic su [qui](#2) per imparare a eseguire il mapping di un'applicazione Web a una versione specifica di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="2fbcd-120">È possibile usare la gestione di informazioni Internet o lo strumento di registrazione ASP.NET IIS (Aspnet\_regiis.exe) per trovare la versione di .NET Framework in esecuzione una particolare applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="2fbcd-121">Fare clic su [qui](#3) per imparare a trovare la versione di .NET Framework che utilizza un sito Web.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="2fbcd-122">Un'importazione considerazione durante la migrazione a .NET Framework 1.1 è che ogni versione di .NET Framework Usa un proprio file Machine. config.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="2fbcd-123">Di conseguenza, se un amministratore IT ha apportato modifiche al file Machine. config, tali modifiche devono essere migrate nel file Machine. config di .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="2fbcd-124">Gestione di mapping dell'applicazione Web per .NET Framework 1.0 durante l'installazione</span><span class="sxs-lookup"><span data-stu-id="2fbcd-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="2fbcd-125">Per impostazione predefinita, tutte le applicazioni ASP.NET esistenti vengono automaticamente riconfigurate durante l'installazione per utilizzare la versione più recente di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="2fbcd-126">Usa la versione più recente di .NET Framework, le applicazioni possono usufruire dei miglioramenti e nuove funzionalità incluse nella nuova versione.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="2fbcd-127">Allo stesso tempo, l'amministratore Web, che potrebbe essere necessario un controllo granulare delle applicazioni che vengono aggiornati, può impedire la riassociazione automatica di tutte le applicazioni ASP.NET esistenti durante l'installazione di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="2fbcd-128">Per evitare la riassociazione automatica dell'intera applicazione ASP.NET per la versione più recente di .NET Framework, l'amministratore Web può usare l'opzione della riga di comando /noaspupgrade Dotnetfx.exe programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="2fbcd-129">**Per impedire la modifica del totale dell'applicazione di ASP.NET alla versione più recente**</span><span class="sxs-lookup"><span data-stu-id="2fbcd-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="2fbcd-130">Passare a **avviare**.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-130">Go to **Start**.</span></span>
2. <span data-ttu-id="2fbcd-131">Fare clic su **eseguiti**.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-131">Click on **run**.</span></span>
3. <span data-ttu-id="2fbcd-132">Digitare **cmd**.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-132">Type **cmd**.</span></span>
4. <span data-ttu-id="2fbcd-133">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="2fbcd-134">Dal prompt dei comandi, digitare la riga seguente per avviare l'installazione di .NET Framework: **Dotnetfx.exe /c: "installare /noaspupgrade?**.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="2fbcd-135">Fare clic su **Sì** nell'installazione di Microsoft .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="2fbcd-136">Questo verrà avviato il processo di installazione di .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="2fbcd-137">Eseguire il mapping di un'applicazione Web a una versione specifica di .NET Framework</span><span class="sxs-lookup"><span data-stu-id="2fbcd-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="2fbcd-138">Ogni versione di .NET Framework include una versione dello strumento di registrazione ASP.NET IIS (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="2fbcd-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="2fbcd-139">Questo strumento consente agli amministratori di specificare che un'applicazione Web deve essere eseguito in una particolare versione di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="2fbcd-140">Ciò viene detto mapping di un'applicazione Web a una versione di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="2fbcd-141">Gli amministratori devono selezionare Aspnet\_regiis.exe che corrisponde alla versione di .NET Framework che verrà associato all'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="2fbcd-142">Ad esempio, un amministratore che si desidera specificare che un sito Web usare .NET Framework 1.1 debba usare Aspnet\_regiis.exe fornito con .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="2fbcd-143">Aspnet\_regiis.exe per la versione 1.0 si trova in:</span><span class="sxs-lookup"><span data-stu-id="2fbcd-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="2fbcd-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="2fbcd-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="2fbcd-145">Aspnet\_regiis.exe per versione 1,1 si trova in:</span><span class="sxs-lookup"><span data-stu-id="2fbcd-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="2fbcd-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="2fbcd-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="2fbcd-147">Aspnet\_regiis.exe fornisce due opzioni per il mapping di un'applicazione Web di script:</span><span class="sxs-lookup"><span data-stu-id="2fbcd-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="2fbcd-148">**-s** imposta il mapping di script nel percorso e nel relativo elemento figlio le directory.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="2fbcd-149">**-sn** imposta il mapping di script solo del percorso.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="2fbcd-150">Il percorso definisce il percorso dell'applicazione Web IIS metadati, che è definito sotto forma di W3SVC/ROOT / {WebSiteNumber} / {applicazione\_nome}.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="2fbcd-151">Ad esempio, per un'applicazione Web denominata portale che si trova nel sito Web predefinito, il percorso della metabase è W3SVC/1/ROOT/il portale.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="2fbcd-152">Tenere presente che è anche possibile usare uno strumento denominato Editor Metabase per ottenere il percorso della metabase.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="2fbcd-153">È possibile scaricare questo strumento dal sito di supporto tecnico Microsoft all'indirizzo [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="2fbcd-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="2fbcd-154">Eseguire Aspnet\_regiis.exe -s W3SVC/1/ROOT/il portale per aggiornare il portale IIS mappa e relativo subapplication script.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="2fbcd-155">Eseguire Aspnet\_regiis.exe sn - W3SVC/1/ROOT/il portale per aggiornare lo script IIS portale esegue il mapping, senza influire sulle applicazioni nel portale? le sottodirectory s.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="2fbcd-156">Trovare la versione di .NET Framework che usa un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="2fbcd-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="2fbcd-157">Un amministratore può utilizzare Gestione servizi Internet per trovare la versione di .NET Framework esegue un sito Web.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="2fbcd-158">Versioni diverse del sistema operativo avviare Gestione servizi Internet in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="2fbcd-159">Per avviare il gestore del servizio, seguire i passaggi elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="2fbcd-160">**Avviare Internet Service Manager**</span><span class="sxs-lookup"><span data-stu-id="2fbcd-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="2fbcd-161">Passare a **avviare**.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-161">Go to **Start**.</span></span>
2. <span data-ttu-id="2fbcd-162">Fare clic su **eseguiti**.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-162">Click on **run**.</span></span>
3. <span data-ttu-id="2fbcd-163">Tipo di **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="2fbcd-164">Da Gestione servizi Internet, selezionare l'applicazione Web la cui versione di .NET Framework che si desidera conoscere.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="2fbcd-165">Fare doppio clic sull'applicazione Web e fare clic su **proprietà.**</span><span class="sxs-lookup"><span data-stu-id="2fbcd-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="2fbcd-166">Nella finestra Proprietà, selezionare **configurazione.**</span><span class="sxs-lookup"><span data-stu-id="2fbcd-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="2fbcd-167">La tabella di mapping dell'applicazione, selezionare **. aspx**, fare clic su **modificare**.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="2fbcd-168">Dal **eseguibile** casella di testo, esaminare la directory della versione mediante lo scorrimento.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="2fbcd-169">Se la directory della versione è v.1.1.4322, l'applicazione è mappata a .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="2fbcd-170">Viceversa, se la directory della versione è v1.0.3705, l'applicazione è mappata a .NET Framework 1.0.</span><span class="sxs-lookup"><span data-stu-id="2fbcd-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
