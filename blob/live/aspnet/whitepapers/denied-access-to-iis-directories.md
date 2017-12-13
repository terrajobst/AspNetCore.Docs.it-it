---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET negato l'accesso a directory IIS | Documenti Microsoft
author: rick-anderson
description: "Questo white paper vengono descritte le operazioni da eseguire se una richiesta all'applicazione ASP.NET restituisce l'errore \"accesso negato alla directory nomedirectory. Non è riuscito a s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 64118ac7a5f280775106d2dc7636923b08f28d89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="29f93-104">ASP.NET negato l'accesso a directory IIS</span><span class="sxs-lookup"><span data-stu-id="29f93-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="29f93-105">Questo white paper vengono descritte le operazioni da eseguire se una richiesta all'applicazione ASP.NET restituisce l'errore "accesso negato a *nomedirectory* directory.</span><span class="sxs-lookup"><span data-stu-id="29f93-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="29f93-106">Impossibile avviare il monitoraggio delle directory chaanges."</span><span class="sxs-lookup"><span data-stu-id="29f93-106">Failed to start monitoring directory chaanges."</span></span>
> 
> <span data-ttu-id="29f93-107">Si applica a ASP.NET 1.0 e 1.1 di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="29f93-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="29f93-108">Versione RTM di ASP.NET V1 viene ora eseguito con un minore di account di windows - registrato come account "ASPNET" in un computer locale con privilegi.</span><span class="sxs-lookup"><span data-stu-id="29f93-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="29f93-109">In alcune bloccati a sistemi, questo account potrebbe non per impostazione predefinita di lettura accesso di sicurezza directory di contenuto di un sito Web, directory radice dell'applicazione o la directory radice del sito web.</span><span class="sxs-lookup"><span data-stu-id="29f93-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="29f93-110">In questo caso verrà visualizzato l'errore seguente quando viene richiesto di pagine da una determinata applicazione web:</span><span class="sxs-lookup"><span data-stu-id="29f93-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="29f93-111">Per risolvere questo problema, è necessario modificare le autorizzazioni di sicurezza nelle directory appropriate.</span><span class="sxs-lookup"><span data-stu-id="29f93-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="29f93-112">In particolare, ASP.NET richiede la lettura, esecuzione e l'elenco di accesso per l'account ASPNET per la radice del sito web (ad esempio: c:\inetpub\wwwroot o in qualsiasi directory sito alternativo configurate in IIS), la directory del contenuto e delle directory radice dell'applicazione per monitorare le modifiche di file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="29f93-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="29f93-113">La radice dell'applicazione corrisponde al percorso della cartella associato alla directory virtuale dell'applicazione nello strumento di amministrazione di IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="29f93-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="29f93-114">Ad esempio, si consideri la seguente gerarchia di applicazione nella cartella wwwroot.</span><span class="sxs-lookup"><span data-stu-id="29f93-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="29f93-115">Per questo esempio, l'account ASPNET richiede le autorizzazioni di lettura definite in precedenza per il contenuto di myapp sia la directory wwwroot.</span><span class="sxs-lookup"><span data-stu-id="29f93-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="29f93-116">Un singolo ACL ereditato per la cartella radice, facoltativamente, nonché per entrambe le directory se si sta annidati.</span><span class="sxs-lookup"><span data-stu-id="29f93-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="29f93-117">Per aggiungere autorizzazioni a una directory, eseguire la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="29f93-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="29f93-118">Utilizzando Esplora risorse, passare alla directory</span><span class="sxs-lookup"><span data-stu-id="29f93-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="29f93-119">Fare clic con il pulsante destro sulla cartella directory, quindi scegliere "Proprietà"</span><span class="sxs-lookup"><span data-stu-id="29f93-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="29f93-120">Passare alla scheda "Sicurezza" nella finestra di dialogo proprietà</span><span class="sxs-lookup"><span data-stu-id="29f93-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="29f93-121">Fare clic sul pulsante "Aggiungi" e immettere il nome del computer seguito dal nome dell'account ASPNET.</span><span class="sxs-lookup"><span data-stu-id="29f93-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="29f93-122">Ad esempio, in un computer denominato "webdev", si immettere webdev\ASPNET e fare clic su "OK".</span><span class="sxs-lookup"><span data-stu-id="29f93-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="29f93-123">Verificare che l'account ASPNET disponga di "lettura &amp; Execute", "Visualizzazione contenuto cartella" e "Lettura" caselle di controllo selezionata.</span><span class="sxs-lookup"><span data-stu-id="29f93-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="29f93-124">Fare clic su OK per chiudere la finestra di dialogo e salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="29f93-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="29f93-125">Se si desidera, queste modifiche possono essere automatizzate tramite script o lo strumento "cacls.exe" fornito con Windows.</span><span class="sxs-lookup"><span data-stu-id="29f93-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="29f93-126">Per ulteriori informazioni sull'account ASPNET, vedere il [documento domande frequenti su](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="29f93-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="29f93-127">Se una determinata applicazione web si basa sulla disponibilità di scrittura o modificare le autorizzazioni per un file o cartella particolare, questo può essere concessa seguendo la stessa procedura e selezionando le caselle di controllo "Scrittura" e/o "Modifica".</span><span class="sxs-lookup"><span data-stu-id="29f93-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="29f93-128">Nei computer che consentono a tutti gli utenti o l'accesso in lettura gruppo utenti di queste directory (ovvero la configurazione predefinita), non si verificheranno problemi e i passaggi precedenti non saranno più necessari.</span><span class="sxs-lookup"><span data-stu-id="29f93-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
