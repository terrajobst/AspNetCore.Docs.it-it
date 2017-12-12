---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Configurazione dei parametri per la distribuzione di pacchetto Web | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come impostare i valori dei parametri, ad esempio i nomi delle applicazioni web di Internet Information Services (IIS), le stringhe di connessione e gli endpoint del servizio,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: dd1ae266740ea4728c0624b1833a98ac262e0e5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="configuring-parameters-for-web-package-deployment"></a>Configurazione dei parametri per la distribuzione di pacchetto Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come impostare i valori dei parametri, ad esempio i nomi delle applicazioni web di Internet Information Services (IIS), le stringhe di connessione e gli endpoint del servizio, quando si distribuisce un pacchetto web in un server web IIS remoto.


Quando si compila un progetto di applicazione web, la compilazione e il processo di creazione del pacchetto genera tre file di chiave:

- Oggetto *[nome progetto] zip* file. Questo è il pacchetto di distribuzione web per il progetto di applicazione web. Il pacchetto contiene tutti i assembly, i file, script di database e le risorse necessarie per ricreare l'applicazione web in un server web IIS remoto.
- Oggetto *.deploy.cmd [nome progetto]* file. Contiene una serie di comandi con parametri distribuzione Web (MSDeploy.exe) di pubblicare il pacchetto di distribuzione web in un server web IIS remoto.
- Oggetto *[nome progetto]. SetParameters* file. Ciò fornisce un set di valori di parametro per il comando di MSDeploy.exe. È possibile aggiornare i valori in questo file e passarlo a distribuzione Web come un parametro della riga di comando quando si distribuisce il pacchetto di web.

> [!NOTE]
> Per ulteriori informazioni sulla compilazione e il processo di creazione di pacchetti, vedere [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).


Il *SetParameters* file viene generato in modo dinamico dal file di progetto applicazione web e i file di configurazione all'interno del progetto. Quando si compila e creare un pacchetto del progetto, la Pipeline di pubblicazione sul Web (WPP) rileva automaticamente un numero elevato di variabili che possono cambiare tra ambienti di distribuzione, come applicazione web IIS di destinazione e tutte le stringhe di connessione di database. Questi valori con parametri nel pacchetto di distribuzione web e aggiunto a automaticamente le *SetParameters* file. Ad esempio, se si aggiunge una stringa di connessione per il *Web. config* file nel progetto di applicazione web rileverà questa modifica il processo di compilazione e aggiunge una voce per il *SetParameters* file di conseguenza.

In molti casi, la parametrizzazione automatica saranno sufficiente. Tuttavia, se gli utenti devono modificare altre impostazioni tra ambienti di distribuzione, ad esempio le impostazioni dell'applicazione o l'URL dell'endpoint del servizio, è necessario indicare WPP parametrizzare questi valori nel pacchetto di distribuzione e aggiungere le voci corrispondenti per il *SetParameters* file. Le sezioni che seguono illustrano come eseguire questa operazione.

### <a name="automatic-parameterization"></a>Parametrizzazione automatica

Quando si compila e pacchetto di un'applicazione web, WPP verrà parametrizzare automaticamente queste operazioni:

- La destinazione IIS web nome e percorso dell'applicazione.
- Stringhe di connessione a tutti i *Web. config* file.
- Le stringhe di connessione per i database a cui aggiungere il **pubblicazione/creazione pacchetto SQL** scheda nelle pagine delle proprietà di progetto.

Ad esempio, se fosse necessario compilare e assemblare il [Contact Manager](the-contact-manager-solution.md) genera questa soluzione di esempio senza toccare il processo di parametrizzazione in alcun modo, WPP *ContactManager.Mvc.SetParameters.xml* file:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


In questo caso:

- Il **nome dell'applicazione Web IIS** parametro è il percorso di IIS in cui si desidera distribuire l'applicazione web. Il valore predefinito è ricavato il **pubblicazione/creazione pacchetto Web** pagina nelle pagine delle proprietà di progetto.
- Il **stringa di connessione ApplicationServices config** parametro è stato generato da un **connectionStrings/Aggiungi** elemento il *Web. config* file. Rappresenta la stringa di connessione che l'applicazione deve utilizzare per contattare il database delle appartenenze. Il valore fornito in questo caso sarà sostituito in distribuito *Web. config* file. Il valore predefinito è indicato da pre-distribuzione *Web. config* file.

WPP Parametrizza anche queste proprietà nel pacchetto di distribuzione che genera l'errore. Quando si installa il pacchetto di distribuzione, è possibile fornire valori per queste proprietà. Se si installa il pacchetto manualmente tramite Gestione IIS, come descritto in [manualmente l'installazione di pacchetti di Web](manually-installing-web-packages.md), l'installazione guidata viene richiesto di fornire valori per eventuali parametri. Se si installa il pacchetto in modalità remota utilizzando la *. deploy* file, come descritto in [distribuzione di pacchetti Web](deploying-web-packages.md), distribuzione Web sarà a questo *SetParameters* file fornire i valori di parametro. È possibile modificare i valori di *SetParameters* file manualmente oppure è possibile personalizzare il file come parte di un processo di compilazione e distribuzione automatizzato. Questo processo è descritto in dettaglio più avanti in questo argomento.

### <a name="custom-parameterization"></a>Parametrizzazione personalizzata

Negli scenari di distribuzione più complessi, sarà spesso possibile parametrizzare le proprietà aggiuntive prima di distribuire il progetto. In generale, è consigliabile parametrizzare qualsiasi proprietà e le impostazioni variano tra ambienti di destinazione. Questi possono includere:

- Endpoint del servizio di *Web. config* file.
- Le impostazioni dell'applicazione di *Web. config* file.
- Altre proprietà dichiarativo che si desidera richiedere agli utenti di specificare.

Per parametrizzare queste proprietà in modo semplice consiste nell'aggiungere un *Parameters. XML* file nella cartella radice del progetto di applicazione web. Nella soluzione responsabile contatto, ad esempio, il progetto ContactManager.Mvc include un *Parameters. XML* file nella cartella radice.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Se si apre questo file, si noterà che contiene un singolo **parametro** voce. La voce utilizza una query XML Path Language (XPath) per individuare e parametrizzare l'URL dell'endpoint del servizio Windows Communication Foundation (WCF) ContactService il *Web. config* file.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Oltre a parametrizzazione l'URL dell'endpoint nel pacchetto di distribuzione, WPP aggiunge inoltre una voce corrispondente per il *SetParameters* file che viene generato con il pacchetto di distribuzione.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Se si installa manualmente il pacchetto di distribuzione, Gestione IIS verrà richiesto per l'indirizzo dell'endpoint servizio con le proprietà che sono stati sottoposta a parametrizzazione automatica. Se si installa il pacchetto di distribuzione eseguendo il *. deploy* file, è possibile modificare il *SetParameters* file per fornire un valore per l'indirizzo dell'endpoint servizio con i valori per il proprietà che sono stati sottoposta a parametrizzazione automatica.

Per informazioni dettagliate su come creare un *Parameters. XML* file, vedere [procedura: utilizzare i parametri per configurare le impostazioni quando un pacchetto di distribuzione è installato](https://msdn.microsoft.com/en-us/library/ff398068.aspx). La procedura denominata **come utilizzare i parametri di distribuzione per le impostazioni del file Web. config** vengono fornite istruzioni dettagliate.

## <a name="modifying-the-setparametersxml-file"></a>Modificare il File SetParameters

Se si intende distribuire manualmente il pacchetto dell'applicazione web & #x 2014; eseguendo il *. deploy* file o tramite l'esecuzione di MSDeploy.exe dalla riga di comando & #x 2014; non è necessario eseguire per impedire all'utente di modificare manualmente il *SetParameters* file prima della distribuzione. Tuttavia, se si lavora in una soluzione aziendale, potrebbe essere necessario distribuire un pacchetto di applicazione web come parte di un processo di compilazione e distribuzione più grande e automatizzato. In questo scenario, è necessario Microsoft Build Engine (MSBuild) per modificare il *SetParameters* file automaticamente. È possibile farlo tramite il MSBuild **XmlPoke** attività.

Il [soluzione di esempio Contact Manager](the-contact-manager-solution.md) viene illustrato questo processo. Gli esempi di codice che seguono sono stati modificati per mostrare solo i dettagli che sono rilevanti per questo esempio.

> [!NOTE]
> Per una panoramica più ampia del modello di file di progetto nella soluzione di esempio e un'introduzione ai file di progetto personalizzati in generale, vedere [informazioni sui File di progetto](understanding-the-project-file.md) e [comprendere il processo di compilazione](understanding-the-build-process.md).


Innanzitutto, i valori dei parametri di interesse sono definiti come proprietà nel file di progetto specifici dell'ambiente (ad esempio, *Env Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Per istruzioni su come personalizzare i file di progetto specifici dell'ambiente per ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Successivamente, il *Publish.proj* file Importa queste proprietà. Poiché ogni *SetParameters* file è associato un *. deploy* file e si desidera infine il file di progetto per richiamare ogni *. deploy* file, il progetto file crea un MSBuild *elemento* per ogni *. deploy* file e definisce le proprietà di interesse come *metadati dell'elemento*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


In questo caso:

- Il **ParametersXml** valore dei metadati indica la posizione del *SetParameters* file.
- Il **IisWebAppName** valore è il percorso di IIS a cui si desidera distribuire l'applicazione web.
- Il **MembershipDBConnectionString** valore è la stringa di connessione per il database di appartenenza e **MembershipDBConnectionName** valore è il **nome** attributo del parametro corrispondente nella *SetParameters* file.
- Il **ServiceEndpointValue** valore è l'indirizzo dell'endpoint del servizio WCF nel server di destinazione e **ServiceEndpointParamName** valore è l'attributo nome del parametro corrispondente in il *SetParameters* file.

Infine, nel *Publish.proj* file, il **PublishWebPackages** utilizza come destinazione il **XmlPoke** attività per modificare questi valori nel *SetParameters* file.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Si noterà che ogni **XmlPoke** attività specifica quattro valori di attributo:

- Il **XmlInputPath** attributo indica l'attività dove trovare il file che si desidera modificare.
- Il **Query** attributo è una query XPath che identifica il nodo XML che si desidera modificare.
- Il **valore** attributo è il nuovo valore che si desidera inserire il nodo XML selezionato.
- Il **condizione** attributo è i criteri in cui l'attività deve eseguire o meno. In questi casi, la condizione garantisce che non tenti di inserire un valore null o vuoto nel *SetParameters* file.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto il ruolo del *SetParameters* illustrato come viene generato quando si compila un progetto di applicazione web e dei file. Illustrato come è possibile parametrizzare le impostazioni aggiuntive aggiungendo un *Parameters. XML* file al progetto. Descritto inoltre come è possibile modificare il *SetParameters* file come parte di un processo di compilazione automatizzato, più grande, tramite il **XmlPoke** attività nei file di progetto.

L'argomento successivo, [distribuzione di pacchetti Web](deploying-web-packages.md), viene descritto come è possibile distribuire un pacchetto web eseguendo il *. deploy* file o i comandi direttamente tramite MSDeploy.exe. In entrambi i casi, è possibile specificare il *SetParameters* file come un parametro di distribuzione.

## <a name="further-reading"></a>Ulteriori informazioni

Per informazioni su come creare pacchetti web, vedere [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md). Per istruzioni su come distribuire un pacchetto web, vedere [distribuzione di pacchetti Web](deploying-web-packages.md). Per istruzioni dettagliate su come creare un *Parameters. XML* file, vedere [procedura: utilizzare i parametri per configurare le impostazioni quando un pacchetto di distribuzione è installato](https://msdn.microsoft.com/en-us/library/ff398068.aspx).

Per ulteriori informazioni generali su parametrizzazione nella distribuzione Web, vedere [Web distribuire parametrizzazione nell'azione](https://go.microsoft.com/?linkid=9805119) (post di blog).

>[!div class="step-by-step"]
[Precedente](building-and-packaging-web-application-projects.md)
[Successivo](deploying-web-packages.md)
