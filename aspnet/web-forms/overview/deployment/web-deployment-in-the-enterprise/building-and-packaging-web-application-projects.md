---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Compilazione e l'assemblaggio di progetti di applicazione Web | Documenti Microsoft
author: jrjlee
description: "Quando si desidera distribuire un progetto di applicazione web in un ambiente server remoto, la prima attività consiste nel compilare il progetto per generare un packa distribuzione web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: c05f725c9e6b493a6af8f5b5d20dbc9ff73a1ef8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="building-and-packaging-web-application-projects"></a>Compilazione e l'assemblaggio di progetti di applicazione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Quando si desidera distribuire un progetto di applicazione web in un ambiente server remoto, la prima attività è per compilare il progetto e generare un pacchetto di distribuzione web. In questo argomento viene descritto il funzionamento del processo di compilazione per progetti di applicazione web. In particolare, viene spiegato:
> 
> - Come la Pipeline di pubblicazione sul Web (WPP) estende il processo di compilazione per includere funzionalità di distribuzione.
> - Lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) attiva la modalità dell'applicazione web in un pacchetto di distribuzione.
> - La creazione di pacchetti e alla build funzionamento sul processo e i file creati.


In Visual Studio 2010, il processo di compilazione e distribuzione per progetti di applicazione web è supportato in WPP. WPP fornisce un set di destinazioni di Microsoft Build Engine (MSBuild) che estendono la funzionalità di MSBuild e abilitarlo per l'integrazione con distribuzione Web. All'interno di Visual Studio, è possibile visualizzare questa funzionalità estesa nelle pagine delle proprietà per il progetto di applicazione web. Il **pubblicazione/creazione pacchetto Web** pagina, insieme con il **pubblicazione/creazione pacchetto SQL** pagina consente di configurare il progetto di applicazione web viene creato un pacchetto per la distribuzione una volta completato il processo di compilazione.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Come funziona WPP?

Se esaminare il file di progetto per c#-progetto di applicazione basata su web, è possibile vedere che importa due file con estensione targets.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


Il primo **importazione** istruzione è comune a tutti i progetti Visual c#. Questo file, *Microsoft.CSharp.targets*, contiene le destinazioni e attività specifiche di Visual c#. Ad esempio, il compilatore c# (**Csc**) viene richiamata attività qui. Il *Microsoft.CSharp.targets* file a sua volta Importa il *targets* file. Ciò consente di definire le destinazioni che sono comuni a tutti i progetti, ad esempio **compilare**, **ricompilare**, **eseguire**, **compilare**, e **Pulisci** . Il secondo **importazione** istruzione è specifica dei progetti di applicazione web. Il *WebApplication* file a sua volta Importa il *Microsoft.Web.Publishing.targets* file. Il *Microsoft.Web.Publishing.targets* file essenzialmente *è* WPP. Definisce le destinazioni, ad esempio **pacchetto** e **MSDeployPublish**, che richiama la distribuzione Web per completare diverse attività di distribuzione.

Per comprendere come vengono utilizzate queste destinazioni aggiuntive, nella soluzione di esempio Contact Manager, aprire il *Publish.proj* file e guardare il **BuildProjects** destinazione.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Questa destinazione utilizza il **MSBuild** attività per compilare progetti diversi. Si noti il **DeployOnBuild** e **DeployTarget** proprietà:

- Il **DeployOnBuild = true** proprietà significa essenzialmente che "Si desidera eseguire una destinazione aggiuntiva quando compilazione viene completata correttamente."
- Il **DeployTarget** proprietà identifica il nome della destinazione di cui si desidera eseguire quando il **DeployOnBuild** proprietà è uguale a **true**. In questo caso, si specifica che si desidera di MSBuild per eseguire il **pacchetto** destinazione dopo aver compilato il progetto.

Il **pacchetto** definita nella destinazione di *Microsoft.Web.Publishing.targets* file. In pratica, la destinazione accetta l'output di compilazione del progetto di applicazione web e lo trasforma in un pacchetto di distribuzione web che può essere pubblicato in un server web IIS.

> [!NOTE]
> Per visualizzare un file di progetto (ad esempio, *ContactManager.Mvc.csproj*) in Visual Studio 2010, è necessario prima scaricare il progetto dalla soluzione. Nel **Esplora** finestra, fare doppio clic sul nodo del progetto e quindi fare clic su **Scarica progetto**. Nuovo pulsante destro del mouse sul nodo del progetto e quindi fare clic su **modifica***[file di progetto]*). Il file di progetto verrà aperto in formato XML non elaborato. Ricordarsi di ricaricare il progetto, al termine.  
> Per ulteriori informazioni sulle destinazioni di MSBuild, attività, e **importazione** istruzioni, vedere [informazioni sui File di progetto](understanding-the-project-file.md). Per un'introduzione più dettagliata a WPP e file di progetto, vedere [all'interno di Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi e William Bartholomew, ISBN: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>Che cos'è un pacchetto di distribuzione Web?

Quando si compila e distribuisce un progetto di applicazione web, tramite Visual Studio 2010 o tramite MSBuild direttamente, il risultato finale è in genere un *pacchetto di distribuzione web*. Il pacchetto di distribuzione web è un file con estensione zip. Contiene tutti gli elementi che IIS e distribuzione Web necessarie per ricreare l'applicazione web, tra cui:

- L'output compilato dell'applicazione web, inclusi il contenuto, i file di risorse, i file di configurazione, JavaScript e CSS risorse fogli di stile e così via.
- Assembly per il progetto di applicazione web e per qualsiasi riferimento a progetti nella soluzione.
- Script SQL per generare tutti i database che si distribuisce l'applicazione web.

Una volta il pacchetto di distribuzione web è stato generato, è possibile pubblicarlo in un server web IIS in vari modi. Ad esempio, è possibile distribuirlo in modalità remota specificando come destinazione il servizio agente remoto di distribuzione Web o il gestore distribuzione Web nel server web di destinazione oppure è possibile utilizzare Gestione IIS per importare manualmente il pacchetto nel server web di destinazione. Per ulteriori informazioni su questi approcci alla distribuzione, vedere [scelta dell'approccio di destra per distribuzione Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Come funziona il processo di compilazione?

Viene illustrato cosa accade quando si compila e un progetto di applicazione web del pacchetto:

![](building-and-packaging-web-application-projects/_static/image2.png)

Quando si compila un progetto di applicazione web, il processo di compilazione genera un file denominato *[nome progetto]. SourceManifest. XML*. Insieme ai file di progetto e l'output di compilazione, questo *. SourceManifest. XML* file indica a distribuzione Web è necessario includere nel pacchetto di distribuzione web. Utilizzando i dati di input, distribuzione Web consente di generare un pacchetto di distribuzione web denominato *[nome progetto] zip*.

Insieme al pacchetto di distribuzione web, il processo di compilazione genera due file che possono aiutarti a usare il pacchetto:

- Il *. deploy* file include un set di comandi con parametri distribuzione Web (MSDeploy.exe) di pubblicare il pacchetto di distribuzione web in un server web IIS remoto. Esegue il *. deploy* file, con i parametri appropriati, in genere fornisce un modo più rapido e semplice alternativa alla creazione manuale di MSDeploy.exe comandi manualmente.
- Il *SetParameters* file fornisce un set di valori di parametro per il comando di MSDeploy.exe. Questi valori includono le proprietà come il nome dell'applicazione web IIS a cui si desidera distribuire il pacchetto, i valori di endpoint del servizio e le stringhe di connessione definite nel *Web. config* file e qualsiasi proprietà di distribuzione valori definiti nelle pagine delle proprietà di progetto.

Il *SetParameters* file è fondamentale per la gestione del processo di distribuzione. Questo file viene generato in modo dinamico in base al contenuto del progetto di applicazione web. Ad esempio, se si aggiunge una stringa di connessione per il *Web. config* file, il processo di compilazione sarà la stringa di connessione, parametrizzare la distribuzione, di conseguenza, crea e rileva automaticamente una voce di  *SetParameters* file consente di modificare la stringa di connessione come parte del processo di distribuzione. L'argomento successivo, [configurazione dei parametri per la distribuzione di pacchetto Web](configuring-parameters-for-web-package-deployment.md), viene illustrato il ruolo di questo file in modo più dettagliato e descrive i diversi modi in cui è possibile modificarla in fase di compilazione e distribuzione.

> [!NOTE]
> In Visual Studio 2010, WPP non supporta la precompilazione di pagine in un'applicazione web prima della creazione del pacchetto. La versione successiva di Visual Studio e WPP includerà il possibilità per la precompilazione di un'applicazione web come opzione di creazione del pacchetto.


## <a name="conclusion"></a>Conclusione

In questo argomento viene fornita una panoramica del processo di creazione di pacchetti e di compilazione per i progetti di applicazione web in Visual Studio 2010. Descritto come WPP consente di richiamare i comandi di distribuzione Web da MSBuild e spiegato come la creazione di pacchetti e alla build funzionamento sul processo.

Dopo aver creato un pacchetto di distribuzione web, il passaggio successivo consiste nella distribuzione. Per ulteriori informazioni, vedere [configurazione dei parametri per la distribuzione di pacchetto Web](configuring-parameters-for-web-package-deployment.md) e [distribuzione di pacchetti Web](deploying-web-packages.md).

## <a name="further-reading"></a>Ulteriori informazioni

Negli argomenti successivi di questa esercitazione, [configurazione dei parametri per la distribuzione di pacchetto Web](configuring-parameters-for-web-package-deployment.md) e [distribuzione di pacchetti Web](deploying-web-packages.md), fornire indicazioni su come usare il pacchetto web è stato creato. L'esercitazione finale di questa serie, [avanzate di distribuzione Web aziendale](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), vengono fornite indicazioni su come personalizzare e risolvere il processo di creazione del pacchetto.

Per un'introduzione più dettagliata a WPP e file di progetto, vedere [all'interno di Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi e William Bartholomew, ISBN: 978-0-7356-4524-0.

>[!div class="step-by-step"]
[Precedente](understanding-the-build-process.md)
[Successivo](configuring-parameters-for-web-package-deployment.md)
