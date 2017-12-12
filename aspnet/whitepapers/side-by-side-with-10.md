---
uid: whitepapers/side-by-side-with-10
title: Esecuzione di ASP.NET Side-by-Side di .NET Framework 1.0 e 1.1 | Documenti Microsoft
author: rick-anderson
description: Questo white paper viene descritto come installare sia .NET 1.0 e 1.1 di .NET nel computer, consentendo di un'applicazione Web ASP.NET per l'esecuzione su entrambe le versioni di con i fotogrammi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="520dc-103">Esecuzione di ASP.NET Side-by-Side di .NET Framework 1.0 e 1.1</span><span class="sxs-lookup"><span data-stu-id="520dc-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="520dc-104">Questo white paper viene descritto come installare sia .NET 1.0 e 1.1 di .NET nel computer, consentendo di un'applicazione Web ASP.NET per l'esecuzione su entrambe le versioni di framework.</span><span class="sxs-lookup"><span data-stu-id="520dc-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="520dc-105">Si applica a ASP.NET 1.0 e 1.1 di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="520dc-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="520dc-106">In ASP.NET, applicazioni si parla di essere in esecuzione side-by-side quando vengono installati nello stesso computer, ma utilizzare versioni diverse di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="520dc-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="520dc-107">L'argomento seguente viene descritto come configurare le applicazioni ASP.NET per l'esecuzione side-by-side e vengono fornite procedure dettagliate per:</span><span class="sxs-lookup"><span data-stu-id="520dc-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="520dc-108">Gestire i mapping dell'applicazione Web durante l'installazione di .NET Framework versione 1.0</span><span class="sxs-lookup"><span data-stu-id="520dc-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="520dc-109">Eseguire il mapping di un'applicazione Web in una versione specifica di .NET Framework</span><span class="sxs-lookup"><span data-stu-id="520dc-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="520dc-110">Trovare la versione di .NET Framework che utilizza un sito Web</span><span class="sxs-lookup"><span data-stu-id="520dc-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="520dc-111">In genere, quando un componente o un'applicazione viene aggiornata in un computer, la versione precedente è rimossa e sostituita con la versione più recente.</span><span class="sxs-lookup"><span data-stu-id="520dc-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="520dc-112">Se la nuova versione non è compatibile con la versione precedente, viene in genere interrotta altre applicazioni che utilizzano il componente o applicazione.</span><span class="sxs-lookup"><span data-stu-id="520dc-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="520dc-113">.NET Framework fornisce il supporto per l'esecuzione side-by-side, che consente più versioni di un assembly o applicazione venga installata nello stesso computer contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="520dc-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="520dc-114">Poiché è possibile installare più versioni contemporaneamente, le applicazioni gestite possono selezionare la versione da utilizzare senza influire sulle applicazioni che utilizzano una versione diversa.</span><span class="sxs-lookup"><span data-stu-id="520dc-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="520dc-115">Per impostazione predefinita, durante l'installazione di .NET Framework versione 1.1, tutte le applicazioni ASP.NET esistenti vengono riconfigurate automaticamente per usare la versione più recente di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="520dc-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="520dc-116">Se si preferisce non nelle applicazioni ASP.NET in .NET Framework 1.1 per impostazione predefinita, fare clic su [qui](#1) per informazioni su come evitare questo problema durante l'installazione.</span><span class="sxs-lookup"><span data-stu-id="520dc-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="520dc-117">Se si aggiorna il server Web a .NET Framework 1.1 e si desidera una o più applicazioni Web per l'esecuzione di .NET Framework 1.0, è necessario aggiornare il mapping di Script di Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="520dc-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="520dc-118">Il mapping di script è il meccanismo per eseguire il mapping di estensione di file con estensione aspx per un'applicazione Web specifica a una versione di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="520dc-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="520dc-119">Fare clic su [qui](#2) per informazioni su come eseguire il mapping di un'applicazione Web in una versione specifica di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="520dc-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="520dc-120">È possibile utilizzare la gestione di informazioni Internet o lo strumento di registrazione IIS ASP.NET (Aspnet\_regiis.exe) per trovare la versione di .NET Framework in esecuzione una particolare applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="520dc-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="520dc-121">Fare clic su [qui](#3) per informazioni su come trovare la versione di .NET Framework che utilizza un sito Web.</span><span class="sxs-lookup"><span data-stu-id="520dc-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="520dc-122">Una considerazione di importazione per la migrazione a .NET Framework 1.1 è che ogni versione di .NET Framework Usa il proprio file Machine. config.</span><span class="sxs-lookup"><span data-stu-id="520dc-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="520dc-123">Di conseguenza, se un amministratore Web ha apportato modifiche al file Machine. config, tali modifiche devono essere migrate nel file Machine. config di .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="520dc-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="520dc-124">Gestione di mapping dell'applicazione Web durante l'installazione di .NET Framework 1.0</span><span class="sxs-lookup"><span data-stu-id="520dc-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="520dc-125">Per impostazione predefinita, tutte le applicazioni ASP.NET esistenti vengono automaticamente riconfigurate durante l'installazione di utilizzare la versione più recente di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="520dc-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="520dc-126">Usa la versione più recente di .NET Framework, le applicazioni possono usufruire dei miglioramenti e nuove funzionalità incluse nella nuova versione.</span><span class="sxs-lookup"><span data-stu-id="520dc-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="520dc-127">Allo stesso tempo, l'amministratore Web, che potrebbe essere necessario un controllo granulare delle applicazioni che vengono aggiornati, può impedire la riassociazione automatica di tutte le applicazioni ASP.NET esistenti durante l'installazione di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="520dc-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="520dc-128">Per evitare la riassociazione automatica dell'intera applicazione ASP.NET per la versione più recente di .NET Framework, l'amministratore Web è possibile utilizzare l'opzione della riga di comando /noaspupgrade con il programma di installazione Dotnetfx.exe.</span><span class="sxs-lookup"><span data-stu-id="520dc-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="520dc-129">**Per impedire la modifica del totale dell'applicazione ASP.NET alla versione più recente**</span><span class="sxs-lookup"><span data-stu-id="520dc-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="520dc-130">Passare a **avviare**.</span><span class="sxs-lookup"><span data-stu-id="520dc-130">Go to **Start**.</span></span>
2. <span data-ttu-id="520dc-131">Fare clic su **eseguire**.</span><span class="sxs-lookup"><span data-stu-id="520dc-131">Click on **run**.</span></span>
3. <span data-ttu-id="520dc-132">Digitare **cmd**.</span><span class="sxs-lookup"><span data-stu-id="520dc-132">Type **cmd**.</span></span>
4. <span data-ttu-id="520dc-133">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="520dc-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="520dc-134">Dal prompt dei comandi, digitare quanto segue per avviare l'installazione di .NET Framework: **/c: Dotnetfx.exe "installare /noaspupgrade?**.</span><span class="sxs-lookup"><span data-stu-id="520dc-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="520dc-135">Fare clic su **Sì** nell'installazione di Microsoft .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="520dc-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="520dc-136">Questo verrà avviato il processo di installazione di .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="520dc-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="520dc-137">Eseguire il mapping di un'applicazione Web in una versione specifica di .NET Framework</span><span class="sxs-lookup"><span data-stu-id="520dc-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="520dc-138">Ogni versione di .NET Framework include una versione dello strumento di registrazione IIS ASP.NET (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="520dc-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="520dc-139">Questo strumento consente agli amministratori di specificare che un'applicazione Web deve essere eseguito in una determinata versione di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="520dc-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="520dc-140">Questo viene considerato il mapping di un'applicazione Web a una versione di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="520dc-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="520dc-141">Gli amministratori devono selezionare Aspnet\_regiis.exe che corrisponde alla versione di .NET Framework che verrà associato all'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="520dc-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="520dc-142">Ad esempio, un amministratore che si desidera specificare un sito Web viene utilizzato .NET Framework 1.1 debba utilizzare Aspnet\_regiis.exe fornito con .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="520dc-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="520dc-143">Aspnet\_regiis.exe per la versione 1.0 si trova in:</span><span class="sxs-lookup"><span data-stu-id="520dc-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="520dc-144">C:\WINDOWS\Microsoft.NET\Framework\**v 1.0.3705** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="520dc-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="520dc-145">Aspnet\_regiis.exe per versione 1,1 si trova in:</span><span class="sxs-lookup"><span data-stu-id="520dc-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="520dc-146">C:\WINDOWS\Microsoft.NET\Framework\**v 1.1.4322** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="520dc-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="520dc-147">Aspnet\_regiis.exe sono disponibili due opzioni per il mapping di un'applicazione Web di script:</span><span class="sxs-lookup"><span data-stu-id="520dc-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="520dc-148">**-s** imposta il mapping di script nel percorso e il relativo elemento figlio directory.</span><span class="sxs-lookup"><span data-stu-id="520dc-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="520dc-149">**sn -** imposta il mapping di script solo del percorso.</span><span class="sxs-lookup"><span data-stu-id="520dc-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="520dc-150">Il percorso di definire il percorso dell'applicazione Web IIS metadati, che è definito sotto forma di W3SVC/ROOT / {WebSiteNumber} / {applicazione\_nome}.</span><span class="sxs-lookup"><span data-stu-id="520dc-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="520dc-151">Per un'applicazione Web, definita portale si trova nel sito Web predefinito, ad esempio, il percorso della metabase è W3SVC/1/ROOT/portale.</span><span class="sxs-lookup"><span data-stu-id="520dc-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="520dc-152">Si noti che inoltre, è possibile utilizzare uno strumento denominato Editor Metabase per ottenere il percorso della metabase.</span><span class="sxs-lookup"><span data-stu-id="520dc-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="520dc-153">È possibile scaricare questo strumento dal sito Microsoft Support [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="520dc-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="520dc-154">Eseguire Aspnet\_regiis.exe -s W3SVC/1/ROOT/portale per aggiornare il portale IIS script mappa e il relativo subapplication.</span><span class="sxs-lookup"><span data-stu-id="520dc-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="520dc-155">Eseguire Aspnet\_regiis.exe sn - W3SVC/1/ROOT/portale per aggiornare lo script IIS portale esegue il mapping, senza influire sulle applicazioni nel portale? sottodirectory s.</span><span class="sxs-lookup"><span data-stu-id="520dc-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="520dc-156">Trovare la versione di .NET Framework che utilizza un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="520dc-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="520dc-157">Un amministratore può utilizzare Gestione servizio Internet per trovare la versione di .NET Framework viene eseguito un sito Web.</span><span class="sxs-lookup"><span data-stu-id="520dc-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="520dc-158">Versioni diverse del sistema operativo avvia Gestione servizi Internet in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="520dc-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="520dc-159">Per avviare il gestore del servizio, attenersi alla procedura riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="520dc-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="520dc-160">**Avviare Internet Service Manager**</span><span class="sxs-lookup"><span data-stu-id="520dc-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="520dc-161">Passare a **avviare**.</span><span class="sxs-lookup"><span data-stu-id="520dc-161">Go to **Start**.</span></span>
2. <span data-ttu-id="520dc-162">Fare clic su **eseguire**.</span><span class="sxs-lookup"><span data-stu-id="520dc-162">Click on **run**.</span></span>
3. <span data-ttu-id="520dc-163">Tipo **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="520dc-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="520dc-164">Da Gestione servizi Internet, selezionare l'applicazione Web, la cui versione di .NET Framework che si desidera conoscere.</span><span class="sxs-lookup"><span data-stu-id="520dc-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="520dc-165">Pulsante destro del mouse sull'applicazione Web e fare clic su **proprietà.**</span><span class="sxs-lookup"><span data-stu-id="520dc-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="520dc-166">Dalla finestra Proprietà selezionare **configurazione.**</span><span class="sxs-lookup"><span data-stu-id="520dc-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="520dc-167">La tabella di mapping dell'applicazione, selezionare **aspx**, fare clic su **modifica**.</span><span class="sxs-lookup"><span data-stu-id="520dc-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="520dc-168">Dal **eseguibile** casella di testo, esaminare la directory della versione mediante lo scorrimento.</span><span class="sxs-lookup"><span data-stu-id="520dc-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="520dc-169">Se la directory della versione è v.1.1.4322, l'applicazione è mappata a .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="520dc-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="520dc-170">Viceversa, se la directory versione v 1.0.3705, l'applicazione è mappata a .NET Framework 1.0.</span><span class="sxs-lookup"><span data-stu-id="520dc-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
