---
title: ASP.NET Core in Nano Server
author: shirhatti
description: Informazioni su come distribuire un'app esistente di ASP.NET Core in un'istanza di Nano Server che esegue IIS.
keywords: ASP.NET Core, Nano Server
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.assetid: 50922cf1-ca58-4006-9236-99b7ff2dd0cf
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: f30e911703d5c36d076872f91d4b2fafeefb91f5
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a>ASP.NET Core con IIS in Nano Server

Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)

In questa esercitazione un'app esistente di ASP.NET Core verrà distribuita in un'istanza di Nano Server che esegue IIS.

## <a name="introduction"></a>Introduzione

Nano Server è un'opzione di installazione di Windows Server 2016 caratterizzata da una superficie di piccole dimensioni, una protezione migliore e una manutenzione migliore rispetto a Server Core o al server completo. Per altri dettagli e per i collegamenti per il download delle versioni di prova per 180 giorni, vedere la [documentazione di Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) ufficiale. 

Esistono tre metodi semplici per poter provare Nano Server. Quando si accede con l'account MS:

1. È possibile scaricare il file ISO di Windows Server 2016 e creare un'immagine di Nano Server.

2. Scaricare il disco rigido virtuale di Nano Server.

3. Creare una macchina virtuale in Azure tramite l'immagine di Nano Server nella raccolta di Azure. Se non si ha un account di Azure, è possibile ottenere una versione di prova gratuita per 30 giorni.

In questa esercitazione verrà usata l'opzione 2, il disco rigido virtuale di Nano Server precompilato da Windows Server 2016.

Prima di procedere con questa esercitazione, è necessario l'[output pubblicato](xref:host-and-deploy/directory-structure) di un'applicazione ASP.NET Core esistente. Verificare che l'applicazione sia compilata per l'esecuzione in un processo a **64 bit**.

## <a name="setting-up-the-nano-server-instance"></a>Configurazione dell'istanza di Nano Server

[Creare una nuova macchina virtuale con Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) nel computer di sviluppo usando il disco rigido virtuale scaricato in precedenza. Il computer richiederà di impostare una password di amministratore prima dell'accesso. Dalla console della macchina virtuale premere F11 per impostare la password prima del primo accesso. È anche necessario controllare l'indirizzo IP della nuova macchina virtuale verificando l'IP fisso del server DHCP specificato durante il provisioning della macchina virtuale o nelle impostazioni di rete della console di ripristino di Nano Server.

> [!NOTE]
> Si supponga che la nuova macchina virtuale venga eseguita con l'indirizzo IP V4 locale 192.168.1.10.

Ora è possibile gestire Nano Server tramite la comunicazione remota di PowerShell, che è l'unico modo per amministrare interamente Nano Server.

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a>Connessione all'istanza di Nano Server tramite la comunicazione remota di PowerShell

Aprire una finestra di PowerShell con privilegi elevati per aggiungere l'istanza remota di Nano Server all'elenco `TrustedHosts`.

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> Sostituire la variabile `$nanoServerIpAddress` con l'indirizzo IP corretto.

Dopo aver aggiunto l'istanza di Nano Server a `TrustedHosts`, è possibile connettersi tramite la comunicazione remota di PowerShell.

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

Se la connessione ha esito positivo, viene visualizzato un prompt con un formato simile al seguente: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`

## <a name="creating-a-file-share"></a>Creazione di una condivisione file

Creare una condivisione file in Nano Server, in modo che sia possibile copiarvi l'applicazione pubblicata. Eseguire i comandi seguenti nella sessione remota:

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

Dopo aver eseguito i comandi precedenti, deve essere possibile accedere alla condivisione visitando `\\192.168.1.10\AspNetCoreSampleForNano` in Esplora risorse del computer host.

## <a name="open-port-in-the-firewall"></a>Aprire una porta nel firewall

Eseguire i comandi seguenti nella sessione remota per aprire una porta nel firewall per consentire a IIS di stare in ascolto del traffico TCP sulla porta TCP/8000.

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a>Installazione di IIS

Aggiungere il provider `NanoServerPackage` dalla PowerShell Gallery. Una volta installato e importato il provider, è possibile installare i pacchetti di Windows.

Eseguire i comandi seguenti nella sessione di PowerShell che è stata creata in precedenza:

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

Per verificare rapidamente se IIS è configurato correttamente, è possibile visitare l'URL `http://192.168.1.10/` dove dovrebbe essere visualizzata la pagina di benvenuto. Quando viene installato IIS, per impostazione predefinita viene creato un sito Web denominato `Default Web Site` in ascolto sulla porta 80.

## <a name="installing-the-aspnet-core-module-ancm"></a>Installazione del modulo ASP.NET Core

Il modulo ASP.NET Core è un modulo IIS 7.5+ che è responsabile della gestione dei processi di listener HTTP di ASP.NET Core e dell'inoltro delle richieste ai processi che gestisce. Al momento, il processo per installare il modulo ASP.NET Core per IIS è manuale. È necessario installare l'[aggregazione di hosting di .NET Core Windows Server](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) in un computer normale, non Nano. Dopo aver installato l'aggregazione in una macchina normale, è necessario copiare i file seguenti nella condivisione file creata in precedenza.

In un server normale, non Nano, con IIS eseguire i comandi di copia seguenti:

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

Sostituire `C:\windows\system32\inetsrv` con `C:\Program Files\IIS Express` in un computer Windows 10.

Sul lato Nano è necessario copiare i file seguenti dalla condivisione file creata in precedenza nei percorsi validi. Eseguire i comandi di copia seguenti:

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

Eseguire lo script seguente nella sessione remota:

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
> Dopo il passaggio precedente, eliminare i file *aspnetcore.dll* e *aspnetcore_schema.xml* dalla condivisione.

## <a name="installing-net-core-framework"></a>Installazione di .NET Core Framework

Se l'app è pubblicata come [distribuzione dipendente dal framework (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), nel server deve essere installato .NET Core. Usare lo [script PowerShell dotnet-install.ps1](https://dot.net/v1/dotnet-install.ps1) in una sessione remota di PowerShell per installare .NET Framework in Nano Server. Indicare la versione con interfaccia della riga di comando mediante l'opzione `-Version`:

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a>Pubblicazione dell'applicazione

Copiare l'output pubblicato dell'applicazione esistente nella radice della condivisione file.

Potrebbe essere necessario apportare modifiche a *web.config* in modo che punti a dove è stato estratto *dotnet.exe*. In alternativa, è possibile aggiungere *dotnet.exe* al percorso.

Esempio di come potrebbe essere *web.config* se *dotnet.exe* **non** è nel percorso:

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

Eseguire i comandi seguenti nella sessione remota per creare un nuovo sito in IIS per l'app pubblicata su una porta diversa da quella del sito Web predefinito. È anche necessario aprire tale porta per accedere al Web. Questo script usa `DefaultAppPool` per semplicità. Per altre considerazioni sull'esecuzione in un pool di applicazioni, vedere [Application Pools](xref:host-and-deploy/iis/index#application-pools) (Pool di applicazioni).

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a>Problema noto nell'esecuzione dell'interfaccia della riga di comando di .NET Core in Nano Server e soluzione alternativa
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a>Esecuzione dell'applicazione

L'applicazione Web pubblicata è accessibile da un browser all'indirizzo `http://192.168.1.10:8000`. Se è stata impostata la registrazione, come descritto in [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) (Creazione e reindirizzamento dei log), è possibile visualizzare i log in *C:\PublishedApps\AspNetCoreSampleForNano\logs*.
