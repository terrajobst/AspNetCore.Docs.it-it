---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Il processo di creazione di pacchetti di risoluzione dei problemi | Microsoft Docs
author: jrjlee
description: Questo argomento viene descritto come è possibile raccogliere informazioni dettagliate sul processo di creazione del pacchetto usando la proprietà EnablePackageProcessLoggingAndAssert nel valore M...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 22086d3c154214457fe35794998accdf6109471c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384144"
---
<a name="troubleshooting-the-packaging-process"></a>Il processo di creazione di pacchetti di risoluzione dei problemi
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento viene descritto come è possibile raccogliere informazioni dettagliate sul processo di creazione di pacchetti tramite il **EnablePackageProcessLoggingAndAssert** proprietà di Microsoft Build Engine (MSBuild).
> 
> Quando si imposta la **EnablePackageProcessLoggingAndAssert** proprietà **true**, MSBuild sarà:
> 
> - Aggiungere informazioni aggiuntive sul processo di creazione di pacchetti per i log di compilazione.
> - Registrare gli errori in determinate condizioni, ad esempio, se i file duplicati vengono trovati nell'elenco di pacchetti.
> - Creare una directory di Log nel *ProjectName*\_cartella del pacchetto e usarla per registrare le informazioni sui file di creazione del pacchetto.
> 
> Se il processo di creazione del pacchetto ha esito negativo o pacchetti di distribuzione web non contengono i file previsti, è possibile utilizzare queste informazioni per risolvere i problemi del processo e pinpoint in cui lo stato di avanzamento non corretto.
> 
> > [!NOTE]
> > Il **EnablePackageProcessLoggingAndAssert** proprietà funziona solo se si compila il progetto usando la **Debug** configurazione. La proprietà viene ignorata in altre configurazioni.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Informazioni sulla proprietà EnablePackageProcessLoggingAndAssert

[Compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) descritto come la Pipeline di pubblicazione sul Web (WPP) fornisce un set di destinazioni di MSBuild che estendono la funzionalità di MSBuild e abilitarlo per l'integrazione con Web Internet Information Services (IIS) Strumento di distribuzione (distribuzione Web). Quando si comprime un progetto di applicazione web, si richiama le destinazioni WPP.

Molte di queste destinazioni WPP includono logica condizionale che consente di registrare informazioni aggiuntive quando la **EnablePackageProcessLoggingAndAssert** è impostata su **true**. Ad esempio, se si esamina il **Package** destinazione, è possibile vedere che crea una directory di log aggiuntivi e scrive un elenco di file in un file di testo se **EnablePackageProcessLoggingAndAssert** è uguale a **true**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Le destinazioni WPP sono definite nel *Microsoft.Web.Publishing.targets* file nella cartella %\MSBuild\Microsoft\VisualStudio\v10.0\Web % programmi (x86). È possibile aprire questo file ed esaminare le destinazioni in Visual Studio 2010 o qualsiasi editor XML. Prestare attenzione a non per modificare il contenuto del file.


## <a name="enabling-the-additional-logging"></a>Abilitare la registrazione aggiuntiva

È possibile fornire un valore per il **EnablePackageProcessLoggingAndAssert** proprietà in vari modi, a seconda del modo in cui si compila il progetto.

Se si compila il progetto dalla riga di comando, è possibile fornire un valore per il **EnablePackageProcessLoggingAndAssert** proprietà come un argomento della riga di comando:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Se si usa un file di progetto personalizzati per compilare i progetti, è possibile includere il **EnablePackageProcessLoggingAndAssert** valore nel **delle proprietà** attributo del **MSBuild**attività:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Se si usa una definizione di compilazione di Team Foundation Server (TFS) per compilare i progetti, è possibile fornire un valore per il **EnablePackageProcessLoggingAndAssert** proprietà di **argomenti MSBuild** riga:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Per altre informazioni sulla creazione e configurazione di definizioni di compilazione, vedere [creazione di una compilazione che supporta la distribuzione della definizione](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


In alternativa, se si desidera includere il pacchetto in ogni build, è possibile modificare il file di progetto per il progetto di applicazione web impostare il **EnablePackageProcessLoggingAndAssert** proprietà **true**. È necessario aggiungere la proprietà per il primo **PropertyGroup** elemento all'interno del file con estensione csproj o vbproj.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Esaminare i file di Log

Quando si compila e creare un progetto di applicazione web con il pacchetto **EnablePackageProcessLoggingAndAssert** impostata su **true**, MSBuild crea una cartella aggiuntiva denominata Log nel *NomeProgetto* \_Cartella del pacchetto. La cartella dei Log contiene vari file:

![](troubleshooting-the-packaging-process/_static/image2.png)

L'elenco dei file visualizzati variano in base alle operazioni nel progetto e il processo di compilazione. Tuttavia, questi file vengono in genere utilizzati per registrare l'elenco di file che si stanno raccogliendo WPP per la creazione del pacchetto, in varie fasi del processo:

- Il *PreExcludePipelineCollectFilesPhaseFileList.txt* file sono elencati i file raccolti di MSBuild per creare il pacchetto prima che vengano rimossi tutti i file vengono specificati per l'esclusione.
- Il *AfterExcludeFilesFilesList.txt* file contiene l'elenco di file modificate dopo la rimozione di tutti i file vengono specificati per l'esclusione.

    > [!NOTE]
    > Per altre informazioni sull'esclusione di file e cartelle dal processo di creazione dei pacchetti, vedere [esclusione file e cartelle dalla distribuzione](excluding-files-and-folders-from-deployment.md).
- Il *AfterTransformWebConfig.txt* file sono elencati i file raccolti in un pacchetto dopo qualsiasi *Web. config* trasformazioni sono state eseguite. In questo elenco, qualsiasi specifici della configurazione *Web. config* trasformare file, ad esempio *v* e *Release*, vengono esclusi dall'elenco dei file per creazione del pacchetto. Un singolo trasformato *Web. config* è incluso nella loro posizione.
- Il *PostAutoParameterizationWebConfigConnectionStrings.txt* file contiene l'elenco dei file dopo le stringhe di connessione nel *Web. config* file sono stati associati parametri. Questo è il processo che consente di sostituire le stringhe di connessione con le impostazioni corrette per l'ambiente di destinazione quando si distribuisce il pacchetto.
- Il *Prepackage.txt* file contiene l'elenco di pre-compilazione finalizzato dei file da includere nel pacchetto.

> [!NOTE]
> I nomi dei file di log aggiuntivi in genere corrispondono a destinazioni WPP. È possibile esaminare queste destinazioni esaminando il *Microsoft.Web.Publishing.targets* file nella cartella %\MSBuild\Microsoft\VisualStudio\v10.0\Web % programmi (x86).


Se il contenuto del pacchetto di web non è quelli previsti, esaminare questi file può essere utile per identificare a quale punto le operazioni di processo causa dell'errore.

## <a name="conclusion"></a>Conclusione

In questo argomento illustrato come è possibile usare la **EnablePackageProcessLoggingAndAssert** proprietà in MSBuild per risolvere i problemi il processo di creazione del pacchetto. Spiega i diversi modi in cui è possibile specificare il valore della proprietà per il processo di compilazione e fornendo le informazioni aggiuntive che viene registrate quando si imposta la proprietà su **true**.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sull'utilizzo dei file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Per altre informazioni su WPP e come gestisce il processo di creazione dei pacchetti, vedere [compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Per indicazioni su come escludere file e cartelle specifici dai pacchetti di distribuzione web, vedere [esclusione file e cartelle dalla distribuzione](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Precedente](running-windows-powershell-scripts-from-msbuild-project-files.md)
