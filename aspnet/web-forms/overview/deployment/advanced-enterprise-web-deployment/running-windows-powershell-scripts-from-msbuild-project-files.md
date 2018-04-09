---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Esecuzione di script di Windows PowerShell dai file di progetto MSBuild | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come eseguire uno script di Windows PowerShell come parte di un processo di compilazione e distribuzione. È possibile eseguire uno script in locale (in altre parole, di b....
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: c8ef22cfbba7b3b85944ea4c49f3183e5a6aafbb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Esecuzione di script di Windows PowerShell dai file di progetto MSBuild
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come eseguire uno script di Windows PowerShell come parte di un processo di compilazione e distribuzione. È possibile eseguire uno script in locale (in altre parole, nel server di compilazione) o in modalità remota, ad esempio in un server di database o un server web di destinazione.
> 
> Esistono molti motivi per cui si decide di eseguire uno script di Windows PowerShell di post-distribuzione. Può, ad esempio, essere necessario:
> 
> - Aggiungere un'origine evento personalizzato nel Registro di sistema.
> - Generare una directory del file system per il caricamento.
> - Eseguire la pulizia delle directory di compilazione.
> - Scrivere voci in un file di log personalizzato.
> - Inviare messaggi di posta elettronica invitare gli utenti a un'applicazione web appena sottoposti a provisioning.
> - Creare account utente con le autorizzazioni appropriate.
> - Configurare la replica tra istanze di SQL Server.
> 
> Questo argomento viene illustrato come eseguire script di Windows PowerShell, sia localmente che in modalità remota da una destinazione personalizzata in un file di progetto di Microsoft Build Engine (MSBuild).


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [Contact Manager soluzione](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente uno istruzioni che si applicano a ogni ambiente di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifici dell'ambiente di compilazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

Per eseguire uno script di Windows PowerShell come parte di un processo di distribuzione automatica o passo a passo, è necessario completare queste attività di alto livello:

- Aggiungere lo script di Windows PowerShell per la soluzione e controllo del codice sorgente.
- Creare un comando che richiama script di Windows PowerShell.
- Escape di caratteri XML riservati nel comando.
- Creare una destinazione nel file di progetto MSBuild personalizzato e utilizzare il **Exec** attività per eseguire il comando.

Questo argomento viene illustrato come eseguire queste procedure. Le attività e procedure dettagliate in questo argomento si presuppongono che si ha già familiarità con le proprietà e le destinazioni di MSBuild, e di comprendere come utilizzare un file di progetto MSBuild personalizzato per unità di un processo di compilazione e distribuzione. Per ulteriori informazioni, vedere [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Creazione e aggiunta di script di Windows PowerShell

Le attività in questo argomento utilizzano uno script di Windows PowerShell di esempio denominato **LogDeploy.ps1** per illustrare come eseguire gli script da MSBuild. Il **LogDeploy.ps1** script contiene una funzione semplice che scrive una voce di riga singola di un file di log:


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


Il **LogDeploy.ps1** script accetta due parametri. Il primo parametro rappresenta il percorso completo al file di log a cui si desidera aggiungere una voce e il secondo parametro rappresenta la destinazione di distribuzione che si desidera registrare nel file di log. Quando si esegue lo script, aggiunge una riga nel file di log nel formato seguente:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Per rendere il **LogDeploy.ps1** script disponibili per MSBuild, è necessario:

- Aggiungere lo script al controllo del codice sorgente.
- Aggiungere lo script per la soluzione in Visual Studio 2010.

Non è necessario distribuire lo script con il contenuto della soluzione, indipendentemente dal fatto se si prevede di eseguire lo script nel server di compilazione o in un computer remoto. Un'opzione consiste nell'aggiungere lo script per una cartella della soluzione. Nell'esempio Contact Manager perché si desidera utilizzare lo script di Windows PowerShell come parte del processo di distribuzione, è consigliabile aggiungere lo script nella cartella di soluzione di pubblicazione.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Il contenuto di cartelle della soluzione viene copiato per server di compilazione come materiale di origine. Non formano, tuttavia, nessuna parte di alcun output di progetto.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>L'esecuzione di uno Script di Windows PowerShell nel Server di compilazione

In alcuni scenari, si desidera eseguire gli script di Windows PowerShell nel computer che compila i progetti. Ad esempio, utilizzare uno script di Windows PowerShell per eseguire la pulizia delle cartelle di compilazione o di scrivere voci in un file di log personalizzato.

In termini di sintassi, esecuzione di uno script di Windows PowerShell da un file di progetto MSBuild è equivale all'esecuzione di uno script di Windows PowerShell da un prompt dei comandi normale. È necessario richiamare l'eseguibile powershell.exe e utilizzare il **-comando** commutatore per fornire i comandi desiderati Windows PowerShell per l'esecuzione. (In Windows PowerShell v2, è inoltre possibile utilizzare il **: file** switch). Il comando deve accettare questo formato:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Ad esempio:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Se il percorso per lo script include spazi, è necessario racchiudere il percorso di file tra virgolette singole precedute da una e commerciale. È possibile utilizzare le virgolette doppie, perché è già stata usata per includere il comando:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


Quando si richiama questo comando da MSBuild, esistono alcune considerazioni aggiuntive. In primo luogo, è necessario includere il **– NonInteractive** flag per verificare che lo script viene eseguito in modalità non interattiva. Successivamente, è necessario includere il **– ExecutionPolicy** flag con un valore dell'argomento appropriato. Specifica i criteri di esecuzione che Windows PowerShell verranno applicate a script e consente di ignorare i criteri di esecuzione predefiniti, che potrebbero impedire l'esecuzione dello script. È possibile scegliere tra questi valori degli argomenti:

- Il valore **Unrestricted** consentirà di Windows PowerShell eseguire lo script, indipendentemente dal fatto che lo script viene eseguito l'accesso.
- Il valore **RemoteSigned** consentirà di Windows PowerShell eseguire gli script non firmati creati nel computer locale. Tuttavia, gli script creati in un' posizione devono essere firmati. (In pratica, l'utente è molto improbabile che hanno creato uno script di Windows PowerShell in locale in un server di compilazione).
- Il valore **AllSigned** consentirà di Windows PowerShell eseguire solo script firmati.

I criteri di esecuzione predefinito sono **Restricted**, che impedisce l'esecuzione di qualsiasi file di script di Windows PowerShell.

Infine, è necessario utilizzare caratteri XML riservati che si verificano nel comando di Windows PowerShell di escape:

- Sostituire le virgolette singole con  **&amp;apos;**
- Sostituire le virgolette doppie con  **&amp;quot;**
- Sostituire le e commerciali con  **&amp;amp;**

- Quando si apportano queste modifiche, il comando sarà simile alla seguente:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


All'interno del file di progetto MSBuild personalizzato, è possibile creare una nuova destinazione e utilizzare il **Exec** attività per eseguire questo comando:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


In questo esempio, si noti che:

- Tutte le variabili, ad esempio i valori dei parametri e il percorso dell'eseguibile di Windows PowerShell, vengono dichiarate come proprietà di MSBuild.
- Le condizioni sono inclusi per consentire agli utenti di eseguire l'override di questi valori dalla riga di comando.
- Il **MSDeployComputerName** proprietà è dichiarata in un' posizione nel file di progetto.

Quando si esegue questa destinazione come parte del processo di compilazione, Windows PowerShell verrà eseguito il comando e scrivere una voce di log per il file specificato.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>L'esecuzione di uno Script di Windows PowerShell in un Computer remoto

Windows PowerShell è in grado di eseguire gli script nei computer remoti tramite [gestione remota Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). A tale scopo, è necessario utilizzare il [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet. Ciò consente di eseguire lo script su uno o più computer remoti senza dover copiare lo script per i computer remoti. I risultati vengono restituiti al computer locale da cui è stato eseguito lo script.

> [!NOTE]
> Prima di utilizzare il **Invoke-Command** cmdlet di Windows PowerShell di eseguire script in un computer remoto, è necessario configurare un listener WinRM per accettare i messaggi remoti. È possibile farlo eseguendo il comando **winrm quickconfig** nel computer remoto. Per ulteriori informazioni, vedere [installazione e configurazione per gestione remota Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


Da una finestra di Windows PowerShell, utilizzare questa sintassi per l'esecuzione di **LogDeploy.ps1** script in un computer remoto:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Esistono diverse altre modalità di utilizzo **Invoke-Command** per eseguire uno script di file, ma questo approccio è la più semplice quando è necessario fornire i valori dei parametri e gestire i percorsi contenenti spazi.


Quando si esegue questo codice da un prompt dei comandi, è necessario richiamare l'eseguibile di Windows PowerShell e utilizzare il **-comando** parametro per fornire le istruzioni:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Come prima, è necessario specificare alcune opzioni aggiuntive ed escape di caratteri XML riservati quando si esegue il comando da MSBuild:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Infine, come in precedenza, è possibile utilizzare il **Exec** attività all'interno di una destinazione di MSBuild personalizzata per eseguire il comando:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Quando si esegue questa destinazione come parte del processo di compilazione, Windows PowerShell verrà eseguito lo script nel computer specificato nella **– computername** argomento.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come eseguire uno script di Windows PowerShell da un file di progetto MSBuild. È possibile utilizzare questo approccio per eseguire uno script di Windows PowerShell, localmente o in un computer remoto, come parte di un processo di compilazione e distribuzione automatico o passo-passo.

## <a name="further-reading"></a>Ulteriori informazioni

Per informazioni sulla firma degli script di Windows PowerShell e la gestione dei criteri di esecuzione, vedere [esecuzione di script di Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Per informazioni sull'esecuzione di comandi di Windows PowerShell da un computer remoto, vedere [esecuzione di comandi remoti](https://technet.microsoft.com/library/dd819505.aspx).

Per ulteriori informazioni sull'utilizzo dei file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Precedente](taking-web-applications-offline-with-web-deploy.md)
> [Successivo](troubleshooting-the-packaging-process.md)
