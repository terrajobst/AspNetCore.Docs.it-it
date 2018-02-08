---
title: ASP.NET Core in Nano Server
author: shirhatti
description: Informazioni su come distribuire un'app esistente di ASP.NET Core in un'istanza di Nano Server che esegue IIS.
manager: wpickett
ms.author: riande
ms.date: 11/04/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/nano-server
ms.openlocfilehash: 4fc5f6874f86130da9f66d13778516d984ff8b46
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="3db5e-103">ASP.NET Core con IIS in Nano Server</span><span class="sxs-lookup"><span data-stu-id="3db5e-103">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="3db5e-104">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="3db5e-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="3db5e-105">In questa esercitazione un'app esistente di ASP.NET Core verrà distribuita in un'istanza di Nano Server che esegue IIS.</span><span class="sxs-lookup"><span data-stu-id="3db5e-105">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="3db5e-106">Introduzione</span><span class="sxs-lookup"><span data-stu-id="3db5e-106">Introduction</span></span>

<span data-ttu-id="3db5e-107">Nano Server è un'opzione di installazione di Windows Server 2016 caratterizzata da una superficie di piccole dimensioni, una protezione migliore e una manutenzione migliore rispetto a Server Core o al server completo.</span><span class="sxs-lookup"><span data-stu-id="3db5e-107">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="3db5e-108">Per altri dettagli e per i collegamenti per il download delle versioni di prova per 180 giorni, vedere la [documentazione di Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) ufficiale.</span><span class="sxs-lookup"><span data-stu-id="3db5e-108">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="3db5e-109">Esistono tre metodi semplici per poter provare Nano Server.</span><span class="sxs-lookup"><span data-stu-id="3db5e-109">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="3db5e-110">Quando si accede con l'account MS:</span><span class="sxs-lookup"><span data-stu-id="3db5e-110">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="3db5e-111">È possibile scaricare il file ISO di Windows Server 2016 e creare un'immagine di Nano Server.</span><span class="sxs-lookup"><span data-stu-id="3db5e-111">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="3db5e-112">Scaricare il disco rigido virtuale di Nano Server.</span><span class="sxs-lookup"><span data-stu-id="3db5e-112">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="3db5e-113">Creare una macchina virtuale in Azure tramite l'immagine di Nano Server nella raccolta di Azure.</span><span class="sxs-lookup"><span data-stu-id="3db5e-113">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="3db5e-114">È disponibile una versione di valutazione gratuita di Azure.</span><span class="sxs-lookup"><span data-stu-id="3db5e-114">A free trial of Azure is avaiable.</span></span>

<span data-ttu-id="3db5e-115">In questa esercitazione verrà usata l'opzione 2, il disco rigido virtuale di Nano Server precompilato da Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="3db5e-115">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="3db5e-116">Prima di procedere con questa esercitazione, è necessario l'[output pubblicato](xref:host-and-deploy/directory-structure) di un'applicazione ASP.NET Core esistente.</span><span class="sxs-lookup"><span data-stu-id="3db5e-116">Before proceeding with this tutorial, you will need the [published output](xref:host-and-deploy/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="3db5e-117">Verificare che l'applicazione sia compilata per l'esecuzione in un processo a **64 bit**.</span><span class="sxs-lookup"><span data-stu-id="3db5e-117">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="3db5e-118">Configurazione dell'istanza di Nano Server</span><span class="sxs-lookup"><span data-stu-id="3db5e-118">Setting up the Nano Server instance</span></span>

<span data-ttu-id="3db5e-119">[Creare una nuova macchina virtuale con Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) nel computer di sviluppo usando il disco rigido virtuale scaricato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3db5e-119">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="3db5e-120">Il computer richiederà di impostare una password di amministratore prima dell'accesso.</span><span class="sxs-lookup"><span data-stu-id="3db5e-120">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="3db5e-121">Dalla console della macchina virtuale premere F11 per impostare la password prima del primo accesso.</span><span class="sxs-lookup"><span data-stu-id="3db5e-121">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="3db5e-122">È anche necessario controllare l'indirizzo IP della nuova macchina virtuale verificando l'IP fisso del server DHCP specificato durante il provisioning della macchina virtuale o nelle impostazioni di rete della console di ripristino di Nano Server.</span><span class="sxs-lookup"><span data-stu-id="3db5e-122">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="3db5e-123">Si supponga che la nuova macchina virtuale venga eseguita con l'indirizzo IP V4 locale 192.168.1.10.</span><span class="sxs-lookup"><span data-stu-id="3db5e-123">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="3db5e-124">Ora è possibile gestire Nano Server tramite la comunicazione remota di PowerShell, che è l'unico modo per amministrare interamente Nano Server.</span><span class="sxs-lookup"><span data-stu-id="3db5e-124">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="3db5e-125">Connessione all'istanza di Nano Server tramite la comunicazione remota di PowerShell</span><span class="sxs-lookup"><span data-stu-id="3db5e-125">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="3db5e-126">Aprire una finestra di PowerShell con privilegi elevati per aggiungere l'istanza remota di Nano Server all'elenco `TrustedHosts`.</span><span class="sxs-lookup"><span data-stu-id="3db5e-126">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="3db5e-127">Sostituire la variabile `$nanoServerIpAddress` con l'indirizzo IP corretto.</span><span class="sxs-lookup"><span data-stu-id="3db5e-127">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="3db5e-128">Dopo aver aggiunto l'istanza di Nano Server a `TrustedHosts`, è possibile connettersi tramite la comunicazione remota di PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3db5e-128">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="3db5e-129">Se la connessione ha esito positivo, viene visualizzato un prompt con un formato simile al seguente: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="3db5e-129">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="3db5e-130">Creazione di una condivisione file</span><span class="sxs-lookup"><span data-stu-id="3db5e-130">Creating a file share</span></span>

<span data-ttu-id="3db5e-131">Creare una condivisione file in Nano Server, in modo che sia possibile copiarvi l'applicazione pubblicata.</span><span class="sxs-lookup"><span data-stu-id="3db5e-131">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="3db5e-132">Eseguire i comandi seguenti nella sessione remota:</span><span class="sxs-lookup"><span data-stu-id="3db5e-132">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="3db5e-133">Dopo aver eseguito i comandi precedenti, deve essere possibile accedere alla condivisione visitando `\\192.168.1.10\AspNetCoreSampleForNano` in Esplora risorse del computer host.</span><span class="sxs-lookup"><span data-stu-id="3db5e-133">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="3db5e-134">Aprire una porta nel firewall</span><span class="sxs-lookup"><span data-stu-id="3db5e-134">Open port in the firewall</span></span>

<span data-ttu-id="3db5e-135">Eseguire i comandi seguenti nella sessione remota per aprire una porta nel firewall per consentire a IIS di stare in ascolto del traffico TCP sulla porta TCP/8000.</span><span class="sxs-lookup"><span data-stu-id="3db5e-135">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="3db5e-136">Installazione di IIS</span><span class="sxs-lookup"><span data-stu-id="3db5e-136">Installing IIS</span></span>

<span data-ttu-id="3db5e-137">Aggiungere il provider `NanoServerPackage` dalla PowerShell Gallery.</span><span class="sxs-lookup"><span data-stu-id="3db5e-137">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="3db5e-138">Una volta installato e importato il provider, è possibile installare i pacchetti di Windows.</span><span class="sxs-lookup"><span data-stu-id="3db5e-138">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="3db5e-139">Eseguire i comandi seguenti nella sessione di PowerShell che è stata creata in precedenza:</span><span class="sxs-lookup"><span data-stu-id="3db5e-139">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="3db5e-140">Per verificare rapidamente se IIS è configurato correttamente, è possibile visitare l'URL `http://192.168.1.10/` dove dovrebbe essere visualizzata la pagina di benvenuto.</span><span class="sxs-lookup"><span data-stu-id="3db5e-140">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="3db5e-141">Quando viene installato IIS, per impostazione predefinita viene creato un sito Web denominato `Default Web Site` in ascolto sulla porta 80.</span><span class="sxs-lookup"><span data-stu-id="3db5e-141">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="installing-the-aspnet-core-module-ancm"></a><span data-ttu-id="3db5e-142">Installazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3db5e-142">Installing the ASP.NET Core Module (ANCM)</span></span>

<span data-ttu-id="3db5e-143">Il modulo ASP.NET Core è un modulo IIS 7.5+ che è responsabile della gestione dei processi di listener HTTP di ASP.NET Core e dell'inoltro delle richieste ai processi che gestisce.</span><span class="sxs-lookup"><span data-stu-id="3db5e-143">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="3db5e-144">Al momento, il processo per installare il modulo ASP.NET Core per IIS è manuale.</span><span class="sxs-lookup"><span data-stu-id="3db5e-144">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="3db5e-145">È necessario installare l'[aggregazione di hosting di .NET Core Windows Server](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) in un computer normale, non Nano.</span><span class="sxs-lookup"><span data-stu-id="3db5e-145">You will need to install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) on a regular (not Nano) machine.</span></span> <span data-ttu-id="3db5e-146">Dopo aver installato l'aggregazione in una macchina normale, è necessario copiare i file seguenti nella condivisione file creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3db5e-146">After installing the bundle on a regular machine, you will need to copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="3db5e-147">In un server normale, non Nano, con IIS eseguire i comandi di copia seguenti:</span><span class="sxs-lookup"><span data-stu-id="3db5e-147">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="3db5e-148">Sostituire `C:\windows\system32\inetsrv` con `C:\Program Files\IIS Express` in un computer Windows 10.</span><span class="sxs-lookup"><span data-stu-id="3db5e-148">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="3db5e-149">Sul lato Nano è necessario copiare i file seguenti dalla condivisione file creata in precedenza nei percorsi validi.</span><span class="sxs-lookup"><span data-stu-id="3db5e-149">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="3db5e-150">Eseguire i comandi di copia seguenti:</span><span class="sxs-lookup"><span data-stu-id="3db5e-150">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="3db5e-151">Eseguire lo script seguente nella sessione remota:</span><span class="sxs-lookup"><span data-stu-id="3db5e-151">Run the following script in the remote session:</span></span>

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> <span data-ttu-id="3db5e-152">Dopo il passaggio precedente, eliminare i file *aspnetcore.dll* e *aspnetcore_schema.xml* dalla condivisione.</span><span class="sxs-lookup"><span data-stu-id="3db5e-152">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="3db5e-153">Installazione di .NET Core Framework</span><span class="sxs-lookup"><span data-stu-id="3db5e-153">Installing .NET Core Framework</span></span>

<span data-ttu-id="3db5e-154">Se l'app è pubblicata come [distribuzione dipendente dal framework (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), nel server deve essere installato .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3db5e-154">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="3db5e-155">Usare lo [script PowerShell dotnet-install.ps1](https://dot.net/v1/dotnet-install.ps1) in una sessione remota di PowerShell per installare .NET Framework in Nano Server.</span><span class="sxs-lookup"><span data-stu-id="3db5e-155">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="3db5e-156">Indicare la versione con interfaccia della riga di comando mediante l'opzione `-Version`:</span><span class="sxs-lookup"><span data-stu-id="3db5e-156">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="3db5e-157">Pubblicazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="3db5e-157">Publishing the application</span></span>

<span data-ttu-id="3db5e-158">Copiare l'output pubblicato dell'applicazione esistente nella radice della condivisione file.</span><span class="sxs-lookup"><span data-stu-id="3db5e-158">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="3db5e-159">Potrebbe essere necessario apportare modifiche a *web.config* in modo che punti a dove è stato estratto *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="3db5e-159">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="3db5e-160">In alternativa, è possibile aggiungere *dotnet.exe* al percorso.</span><span class="sxs-lookup"><span data-stu-id="3db5e-160">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="3db5e-161">Esempio di come potrebbe essere *web.config* se *dotnet.exe* **non** è nel percorso:</span><span class="sxs-lookup"><span data-stu-id="3db5e-161">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="3db5e-162">Eseguire i comandi seguenti nella sessione remota per creare un nuovo sito in IIS per l'app pubblicata su una porta diversa da quella del sito Web predefinito.</span><span class="sxs-lookup"><span data-stu-id="3db5e-162">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="3db5e-163">È anche necessario aprire tale porta per accedere al Web.</span><span class="sxs-lookup"><span data-stu-id="3db5e-163">You also need to open that port to access the web.</span></span> <span data-ttu-id="3db5e-164">Questo script usa `DefaultAppPool` per semplicità.</span><span class="sxs-lookup"><span data-stu-id="3db5e-164">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="3db5e-165">Per altre considerazioni sull'esecuzione in un pool di applicazioni, vedere [Application Pools](xref:host-and-deploy/iis/index#application-pools) (Pool di applicazioni).</span><span class="sxs-lookup"><span data-stu-id="3db5e-165">For more considerations on running under an application pool, see [Application Pools](xref:host-and-deploy/iis/index#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="3db5e-166">Problema noto nell'esecuzione dell'interfaccia della riga di comando di .NET Core in Nano Server e soluzione alternativa</span><span class="sxs-lookup"><span data-stu-id="3db5e-166">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="3db5e-167">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="3db5e-167">Running the application</span></span>

<span data-ttu-id="3db5e-168">L'applicazione Web pubblicata è accessibile da un browser all'indirizzo `http://192.168.1.10:8000`.</span><span class="sxs-lookup"><span data-stu-id="3db5e-168">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="3db5e-169">Se è stata impostata la registrazione, come descritto in [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) (Creazione e reindirizzamento dei log), è possibile visualizzare i log in *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span><span class="sxs-lookup"><span data-stu-id="3db5e-169">If you've set up logging as described in [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>
