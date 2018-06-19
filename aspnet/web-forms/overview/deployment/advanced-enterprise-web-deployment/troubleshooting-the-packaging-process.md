---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Risoluzione dei problemi il processo di creazione del pacchetto | Documenti Microsoft
author: jrjlee
description: Questo argomento viene descritto come è possibile raccogliere informazioni dettagliate sul processo di creazione di pacchetti tramite la proprietà EnablePackageProcessLoggingAndAssert di m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 816ab77c44b52c6449a139475f2ef8546bd38071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892686"
---
<a name="troubleshooting-the-packaging-process"></a>Il processo di creazione di pacchetti di risoluzione dei problemi
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento viene descritto come è possibile raccogliere informazioni dettagliate sul processo di creazione di pacchetti tramite la **EnablePackageProcessLoggingAndAssert** proprietà di Microsoft Build Engine (MSBuild).
> 
> Quando si imposta la **EnablePackageProcessLoggingAndAssert** proprietà **true**, MSBuild sarà:
> 
> - Aggiungere informazioni aggiuntive sul processo di creazione di pacchetti per i log di compilazione.
> - Log degli errori in determinate condizioni, ad esempio, se vengono trovati file duplicati nell'elenco di pacchetti.
> - Creare una directory di Log nel *ProjectName*\_cartella del pacchetto e utilizzarlo per registrare le informazioni sui file di cui si crea un pacchetto.
> 
> Se non è possibile eseguire il processo di creazione del pacchetto o i pacchetti di distribuzione web non contengono i file desiderati, è possibile utilizzare queste informazioni per risolvere il processo e il punto di blocco in cui avanzamento errato.
> 
> > [!NOTE]
> > Il **EnablePackageProcessLoggingAndAssert** proprietà funziona solo se si compila il progetto usando il **Debug** configurazione. La proprietà viene ignorata in altre configurazioni.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [Contact Manager soluzione](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente uno istruzioni che si applicano a ogni ambiente di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifici dell'ambiente di compilazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Informazioni sulla proprietà EnablePackageProcessLoggingAndAssert

[Compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) descritto come la Pipeline di pubblicazione sul Web (WPP) fornisce un set di destinazioni di MSBuild che estendono la funzionalità di MSBuild e abilitarlo per l'integrazione con Web Internet Information Services (IIS) Strumento di distribuzione (distribuzione Web). Quando si crea un progetto di applicazione web, il pacchetto si sta chiamando le destinazioni del sito.

Molte di queste destinazioni WPP includono logica condizionale che registra informazioni aggiuntive quando il **EnablePackageProcessLoggingAndAssert** è impostata su **true**. Ad esempio, se si esamina il **pacchetto** destinazione, è possibile vedere che crea una directory di log aggiuntivi e scrive un elenco di file in un file di testo se **EnablePackageProcessLoggingAndAssert** è uguale a **true**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Le destinazioni WPP sono definite nel *Microsoft.Web.Publishing.targets* file nella cartella % PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web. È possibile aprire questo file ed esaminare le destinazioni in Visual Studio 2010 o qualsiasi editor XML. Fare attenzione a non per modificare il contenuto del file.


## <a name="enabling-the-additional-logging"></a>Abilitare la registrazione aggiuntiva

È possibile fornire un valore per il **EnablePackageProcessLoggingAndAssert** proprietà in vari modi, a seconda di come si compila il progetto.

Se si compila il progetto dalla riga di comando, è possibile fornire un valore per il **EnablePackageProcessLoggingAndAssert** proprietà come argomento della riga di comando:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Se si utilizza un file di progetto personalizzati per compilare i progetti, è possibile includere il **EnablePackageProcessLoggingAndAssert** valore il **proprietà** attributo del **MSBuild**attività:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Se si utilizza una definizione di compilazione di Team Foundation Server (TFS) per compilare i progetti, è possibile fornire un valore per il **EnablePackageProcessLoggingAndAssert** proprietà il **argomenti MSBuild** riga:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Per ulteriori informazioni sulla creazione e configurazione di definizioni di compilazione, vedere [la creazione di una compilazione che supporta la distribuzione della definizione](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


In alternativa, se si desidera includere il pacchetto in ogni compilazione, è possibile modificare il file di progetto per il progetto di applicazione web impostare il **EnablePackageProcessLoggingAndAssert** proprietà **true**. È necessario aggiungere la proprietà al primo **PropertyGroup** elemento all'interno del file con estensione csproj o vbproj.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Esaminare i file di Log

Quando si compila e creare un progetto di applicazione web con il pacchetto **EnablePackageProcessLoggingAndAssert** impostato su **true**, MSBuild crea una cartella aggiuntiva denominata Accedi il *ProjectName* \_Cartella del pacchetto. La cartella dei Log contiene vari file:

![](troubleshooting-the-packaging-process/_static/image2.png)

L'elenco dei file visualizzati variano in base alle operazioni nel progetto e il processo di compilazione. Tuttavia, questi file vengono in genere usati per registrare l'elenco di file che sta raccogliendo WPP per la creazione del pacchetto, in varie fasi del processo:

- Il *PreExcludePipelineCollectFilesPhaseFileList.txt* file sono elencati i file raccolti MSBuild per creare il pacchetto prima che vengano rimossi i file specificati per l'esclusione.
- Il *AfterExcludeFilesFilesList.txt* file contiene l'elenco dei file modificati dopo la rimozione di tutti i file specificati per l'esclusione.

    > [!NOTE]
    > Per ulteriori informazioni sull'esclusione di file e cartelle dal processo di creazione di pacchetti, vedere [esclusione dei file e cartelle da distribuzione](excluding-files-and-folders-from-deployment.md).
- Il *AfterTransformWebConfig.txt* file sono elencati i file raccolti per la creazione del pacchetto dopo qualsiasi *Web. config* trasformazioni siano state eseguite. In questo elenco, qualsiasi specifiche della configurazione *Web. config* trasformare file, ad esempio *Web.Debug.config* e *Web.Release.config*, vengono escluse dall'elenco dei file per creazione di pacchetti. Una singola trasformata *Web. config* è incluso al loro posto.
- Il *PostAutoParameterizationWebConfigConnectionStrings.txt* file contiene l'elenco dei file dopo le stringhe di connessione nel *Web. config* file sono stati associati parametri. Questo è il processo che consente di sostituire le stringhe di connessione con le impostazioni corrette per l'ambiente di destinazione quando si distribuisce il pacchetto.
- Il *Prepackage.txt* file contiene l'elenco di pre-compilazione finalizzato dei file da includere nel pacchetto.

> [!NOTE]
> I nomi dei file di log aggiuntivi in genere corrispondono alle destinazioni del sito. È possibile esaminare queste destinazioni esaminando il *Microsoft.Web.Publishing.targets* file nella cartella % PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


Se il contenuto del pacchetto di web non è quello previsto, esaminare questi file può essere utile per identificare in quali punto le operazioni di processo è verificato un errore.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come utilizzare il **EnablePackageProcessLoggingAndAssert** proprietà MSBuild per risolvere il processo di creazione del pacchetto. Illustrato i diversi modi in cui è possibile specificare il valore della proprietà per il processo di compilazione e descritte informazioni aggiuntive che viene registrate quando si imposta la proprietà su **true**.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sull'utilizzo dei file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Per ulteriori informazioni su WPP e modo in cui gestisce il processo di creazione del pacchetto, vedere [compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Per istruzioni su come escludere file e cartelle specifici da pacchetti di distribuzione web, vedere [esclusione dei file e cartelle da distribuzione](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Precedente](running-windows-powershell-scripts-from-msbuild-project-files.md)
