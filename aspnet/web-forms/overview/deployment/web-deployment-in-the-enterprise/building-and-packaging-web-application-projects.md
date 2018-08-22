---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Compilazione e creazione del pacchetto di Web Application Projects | Microsoft Docs
author: jrjlee
description: Quando si desidera distribuire un progetto di applicazione web in un ambiente server remoto, la prima attività consiste nel compilare il progetto e generare un packa distribuzione web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: 406b8e6daf47196eb98700efe41e34c02d5682d3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830264"
---
<a name="building-and-packaging-web-application-projects"></a>Compilazione e creazione del pacchetto di Web Application Projects
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Quando si desidera distribuire un progetto di applicazione web in un ambiente server remoto, la prima attività consiste nel compilare il progetto e generare un pacchetto di distribuzione web. In questo argomento viene descritto il funzionamento del processo di compilazione per progetti di applicazione web. In particolare, la descrive:
> 
> - In che modo la Pipeline di pubblicazione sul Web (WPP) estende il processo di compilazione per includere la funzionalità di distribuzione.
> - Lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web) come attiva l'applicazione web in un pacchetto di distribuzione.
> - La modalità di compilazione e creazione di pacchetti funzionamento sul processo e sui file creati.


In Visual Studio 2010, il processo di compilazione e distribuzione per i progetti applicazione web è supportato da WPP. WPP fornisce un set di destinazioni di Microsoft Build Engine (MSBuild) che estendono la funzionalità di MSBuild e abilitarlo per l'integrazione con distribuzione Web. All'interno di Visual Studio, è possibile visualizzare questa funzionalità estesa nelle pagine delle proprietà per il progetto di applicazione web. Il **pubblicazione/creazione pacchetto Web** pagina, insieme con la **pubblicazione/creazione pacchetto SQL** pagina consente di configurare il modo in cui il progetto di applicazione web viene incluso nel pacchetto per la distribuzione di una volta completato il processo di compilazione.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Come funziona il WPP?

Se esaminiamo il file di progetto per c#-progetto di applicazione basata su web, è possibile vedere che importa due file con estensione targets.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


Il primo **importazione** istruzione è comune a tutti i progetti Visual c#. Questo file, *targets*, contiene le destinazioni e attività specifiche per Visual c#. Ad esempio, il compilatore c# (**Csc**) attività viene richiamata qui. Il *targets* file a sua volta Importa il *microsoftcommon. targets* file. Ciò consente di definire le destinazioni che sono comuni a tutti i progetti, ad esempio **compilare**, **Ricompila**, **eseguire**, **compilare**, e **Pulisci** . La seconda **importazione** istruzione è specifica per i progetti applicazione web. Il *Microsoft.WebApplication.targets* file a sua volta Importa il *Microsoft.Web.Publishing.targets* file. Il *Microsoft.Web.Publishing.targets* file essenzialmente *è* WPP. Definisce le destinazioni, ad esempio **Package** e **MSDeployPublish**, che richiamano distribuzione Web per completare varie attività di distribuzione.

Per comprendere come vengono usate questi destinazioni aggiuntive, nella soluzione di esempio Contact Manager, aprire il *Publish.proj* del file e osservare il **BuildProjects** destinazione.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Questa destinazione utilizza il **MSBuild** attività per compilare progetti diversi. Si noti che il **DeployOnBuild** e **DeployTarget** proprietà:

- Il **DeployOnBuild = true** proprietà significa essenzialmente "Voglio eseguire una destinazione aggiuntiva quando una compilazione viene completata correttamente."
- Il **DeployTarget** proprietà identifica il nome della destinazione di cui si desidera eseguire quando il **DeployOnBuild** proprietà è uguale a **true**. In questo caso, si specifica che si vuole che MSBuild per eseguire la **pacchetto** destinazione dopo aver compilato il progetto.

Il **Package** destinazione definita nel *Microsoft.Web.Publishing.targets* file. In pratica, questa destinazione accetta l'output di compilazione del progetto dell'applicazione web e lo trasforma in un pacchetto di distribuzione web che può essere pubblicato in un server web IIS.

> [!NOTE]
> Per visualizzare un file di progetto (ad esempio, <em>ContactManager.Mvc.csproj</em>) in Visual Studio 2010, è innanzitutto necessario scaricare il progetto dalla soluzione. Nel <strong>Esplora soluzioni</strong> finestra, fare doppio clic sul nodo del progetto e quindi fare clic su <strong>Scarica progetto</strong>. Fare doppio clic sul nodo del progetto anche in questo caso e quindi fare clic su <strong>Edit</strong><em>[file di progetto]</em>). Il file di progetto verrà aperto in formato XML non elaborato. Ricordarsi di ricaricare il progetto al termine.  
> Per altre informazioni sulle destinazioni di MSBuild, attività, e <strong>importazione</strong> istruzioni, vedere [informazioni sul File di progetto](understanding-the-project-file.md). Per un'introduzione più dettagliata per i file di progetto e il sito, vedere [all'interno di Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi, William Bartholomew, ISBN: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>Che cos'è un pacchetto di distribuzione Web?

Quando si compila e distribuisce un progetto di applicazione web, usando Visual Studio 2010 o tramite MSBuild direttamente, il risultato finale è in genere un *pacchetto di distribuzione web*. Il pacchetto di distribuzione web è un file con estensione zip. Contiene tutti gli elementi che IIS e distribuzione Web per poter ricreare l'applicazione web, tra cui:

- L'output compilato dell'applicazione web, tra cui il contenuto, i file di risorse, i file di configurazione, JavaScript e CSS risorse dei fogli di stile e così via.
- Fare riferimento ad assembly per il progetto di applicazione web e per tutti i progetti nella soluzione.
- Script SQL per generare tutti i database che si distribuisce l'applicazione web.

Una volta il pacchetto di distribuzione web è stato generato, è possibile pubblicarlo in un server web IIS in vari modi. Ad esempio, è possibile distribuirlo in remoto come destinazione il servizio Web Deploy Remote Agent o del gestore distribuzione Web nel server web di destinazione, oppure è possibile usare Gestione IIS per importare manualmente il pacchetto nel server web di destinazione. Per altre informazioni su questi approcci alla distribuzione, vedere [scelta dell'approccio a destra per la distribuzione Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Come funziona il processo di compilazione?

Viene illustrato cosa accade quando si compila e creare un pacchetto un progetto di applicazione web:

![](building-and-packaging-web-application-projects/_static/image2.png)

Quando si compila un progetto di applicazione web, il processo di compilazione genera un file denominato *[nome progetto]. SourceManifest. XML*. Insieme ai file di progetto e l'output di compilazione, questo *. SourceManifest. XML* file indica a distribuzione Web è necessario includere nel pacchetto di distribuzione web. I dati di input, distribuzione Web genera un pacchetto di distribuzione web denominato *ZIP [nome progetto]*.

Insieme al pacchetto di distribuzione web, il processo di compilazione genera due file che possono aiutarti a usare il pacchetto:

- Il *. deploy. cmd* file include un set di comandi con parametri (MSDeploy.exe) distribuzione Web che pubblica il pacchetto di distribuzione web a un server web IIS remoto. In esecuzione la *. deploy. cmd* file, con i parametri appropriati, in genere fornisce un modo più rapido e semplice alternativa alla costruzione manuale di MSDeploy.exe comandi manualmente.
- Il *SetParameters* file fornisce un set di valori di parametro al comando MSDeploy.exe. Questi valori includono proprietà quali il nome dell'applicazione web IIS a cui si vuole distribuire il pacchetto, i valori di qualsiasi endpoint di servizio e le stringhe di connessione definita nel *Web. config* file e qualsiasi proprietà di distribuzione valori definiti nelle pagine delle proprietà del progetto.

Il *SetParameters* file è fondamentale per la gestione del processo di distribuzione. Questo file viene generato in modo dinamico in base al contenuto del progetto dell'applicazione web. Ad esempio, se si aggiunge una stringa di connessione per il *Web. config* file, il processo di compilazione verrà automaticamente rilevare la stringa di connessione, impostare i parametri per la distribuzione di conseguenza e creare una voce nel  *SetParameters* file consente di modificare la stringa di connessione come parte del processo di distribuzione. Argomento successivo [configurazione dei parametri per una distribuzione pacchetto Web](configuring-parameters-for-web-package-deployment.md), viene illustrato il ruolo di questo file in modo più dettagliato e vengono descritti i diversi modi in cui è possibile modificarlo durante la compilazione e distribuzione.

> [!NOTE]
> In Visual Studio 2010, WPP non supporta la precompilazione le pagine in un'applicazione web prima della creazione del pacchetto. La prossima versione di Visual Studio e WPP includerà la possibilità di precompilare un'applicazione web come opzione di creazione dei pacchetti.


## <a name="conclusion"></a>Conclusione

In questo argomento fornito una panoramica del processo di creazione di pacchetti e di compilazione per progetti di applicazione web in Visual Studio 2010. Descritto come WPP consente di richiamare i comandi di distribuzione Web da MSBuild e spiegato come la compilazione e creazione di pacchetti funzionamento sul processo.

Dopo aver creato un pacchetto di distribuzione web, il passaggio successivo è eseguire la distribuzione. Per altre informazioni, vedere [configurazione dei parametri per una distribuzione pacchetto Web](configuring-parameters-for-web-package-deployment.md) e [distribuzione di pacchetti Web](deploying-web-packages.md).

## <a name="further-reading"></a>Ulteriori informazioni

Avanti negli argomenti di questa esercitazione [configurazione dei parametri per una distribuzione pacchetto Web](configuring-parameters-for-web-package-deployment.md) e [distribuzione di pacchetti Web](deploying-web-packages.md), fornire indicazioni su come usare il pacchetto web è stata creata. L'ultima esercitazione di questa serie [avanzata di distribuzione Web aziendale](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), fornisce indicazioni su come personalizzare e risolvere i problemi il processo di creazione del pacchetto.

Per un'introduzione più dettagliata per i file di progetto e il sito, vedere [all'interno di Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi, William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Precedente](understanding-the-build-process.md)
> [Successivo](configuring-parameters-for-web-package-deployment.md)
