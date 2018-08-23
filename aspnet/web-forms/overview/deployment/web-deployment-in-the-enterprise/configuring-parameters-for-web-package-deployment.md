---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Configurazione dei parametri per una distribuzione pacchetto Web | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come impostare i valori di parametro, come i nomi delle applicazioni web Internet Information Services (IIS), le stringhe di connessione e gli endpoint di servizio...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 6660ad52ce68932be63e2874a5f6acb34336e575
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835591"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Configurazione dei parametri per una distribuzione pacchetto Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come impostare i valori dei parametri, come i nomi delle applicazioni web Internet Information Services (IIS), le stringhe di connessione e gli endpoint di servizio, quando si distribuisce un pacchetto web a un server web IIS remoto.


Quando si compila un progetto di applicazione web, la compilazione e il processo di creazione dei pacchetti vengono generati tre file principali:

- Oggetto *ZIP [nome progetto]* file. Questo è il pacchetto di distribuzione web per il progetto di applicazione web. Questo pacchetto contiene tutti i assembly, file, script di database e le risorse necessarie per ricreare l'applicazione web in un server web IIS remoto.
- Oggetto *[nome progetto].deploy.cmd* file. Contiene una serie di comandi con parametri (MSDeploy.exe) distribuzione Web che pubblica il pacchetto di distribuzione web a un server web IIS remoto.
- Oggetto *[nome progetto]. SetParameters* file. Ciò fornisce un set di valori di parametro al comando MSDeploy.exe. È possibile aggiornare i valori in questo file e passarlo a distribuzione Web come un parametro della riga di comando quando si distribuisce il pacchetto di web.

> [!NOTE]
> Per altre informazioni sulla compilazione e il processo di creazione dei pacchetti, vedere [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).


Il *SetParameters* file viene generato in modo dinamico dal file di progetto applicazione web e i file di configurazione all'interno del progetto. Quando si compila e creare un pacchetto del progetto, la Pipeline di pubblicazione sul Web (WPP) rileva automaticamente un numero elevato di variabili che potrebbero essere diversi tra ambienti di distribuzione, ad esempio la destinazione dell'applicazione web IIS e tutte le stringhe di connessione di database. Questi valori con parametri nel pacchetto di distribuzione web e aggiunto a automaticamente le *SetParameters* file. Ad esempio, se si aggiunge una stringa di connessione per il *Web. config* file nel progetto di applicazione web, il processo di compilazione rileverà questa modifica e aggiungerà una voce per il *SetParameters* file di conseguenza.

In molti casi, sarà sufficiente la parametrizzazione automatica. Tuttavia, se gli utenti devono modificare altre impostazioni tra ambienti di distribuzione, ad esempio le impostazioni dell'applicazione o gli URL dell'endpoint servizio, è necessario indicare WPP per impostare i parametri per questi valori nel pacchetto di distribuzione e aggiungere le voci corrispondenti per il *SetParameters* file. Le sezioni che seguono illustrano come eseguire questa operazione.

### <a name="automatic-parameterization"></a>Parametrizzazione automatica

Quando si compila e assemblare un'applicazione web, WPP verrà parametrizzare automaticamente queste operazioni:

- La destinazione IIS web nome e percorso dell'applicazione.
- Qualsiasi connessione stringhe nel *Web. config* file.
- Le stringhe di connessione per tutti i database aggiunti per il **pubblicazione/creazione pacchetto SQL** scheda nelle pagine delle proprietà di progetto.

Ad esempio, se si intende compilare e assemblare il [Contact Manager](the-contact-manager-solution.md) genererà questa soluzione di esempio senza modificare il processo di parametrizzazione in qualsiasi modo, WPP *ContactManager.Mvc.SetParameters.xml* file:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


In questo caso:

- Il **/<nome applicazione Web IIS** parametro è il percorso IIS in cui si desidera distribuire l'applicazione web. Il valore predefinito proviene dal **pubblicazione/creazione pacchetto Web** pagina nelle pagine delle proprietà di progetto.
- Il **stringa di connessione ApplicationServices-Web. config** parametro è stato generato da un **connectionStrings/aggiungere** elemento il *Web. config* file. Rappresenta la stringa di connessione che l'applicazione deve usare per contattare il database di appartenenza. Il valore specificato qui verranno sostituiti nella distribuito *Web. config* file. Il valore predefinito proviene dalla distribuzione non definitiva *Web. config* file.

WPP Parametrizza anche queste proprietà nel pacchetto di distribuzione che genera l'errore. È possibile fornire valori per queste proprietà quando si installa il pacchetto di distribuzione. Se si installa il pacchetto manualmente tramite Gestione IIS, come descritto in [manualmente l'installazione dei pacchetti Web](manually-installing-web-packages.md), l'installazione guidata viene richiesto di fornire valori per i parametri. Se si installa il pacchetto in modalità remota usando la *. deploy. cmd* nel file, come descritto in [distribuzione di pacchetti Web](deploying-web-packages.md), distribuzione Web avrà un aspetto a questo *SetParameters* file fornire i valori dei parametri. È possibile modificare i valori di *SetParameters* file manualmente oppure è possibile personalizzare il file come parte di un processo automatizzato di compilazione e distribuzione. Questo processo è descritto più dettagliatamente più avanti in questo argomento.

### <a name="custom-parameterization"></a>Parametrizzazione personalizzata

Negli scenari di distribuzione più complessi, è spesso opportuno di parametrizzare le proprietà aggiuntive prima di distribuire il progetto. In generale, si deve impostare i parametri per qualsiasi proprietà e le impostazioni variano tra ambienti di destinazione. Questi possono includere:

- Endpoint di servizio di *Web. config* file.
- Le impostazioni dell'applicazione di *Web. config* file.
- Qualsiasi altra proprietà dichiarativa che si desidera richiesta agli utenti di specificare.

Per impostare i parametri per queste proprietà in modo semplice consiste nell'aggiungere un *Parameters. XML* file nella cartella radice del progetto dell'applicazione web. Nella soluzione Contact Manager, ad esempio, il progetto ContactManager.Mvc include un' *Parameters. XML* file nella cartella radice.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Se si apre questo file, si noterà che contiene un singolo **parametro** voce. La voce viene utilizzata una query XML Path Language (XPath) consente di individuare e impostare i parametri per l'URL dell'endpoint del servizio ContactService Windows Communication Foundation (WCF) nei *Web. config* file.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Oltre alla definizione di parametri per l'URL dell'endpoint nel pacchetto di distribuzione, WPP aggiunge inoltre una voce corrispondente per il *SetParameters* file generato insieme al pacchetto di distribuzione.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Se si installa manualmente il pacchetto di distribuzione, Gestione IIS richiederà per l'indirizzo dell'endpoint servizio con le proprietà che sono state parametrizzate automaticamente. Se si installa il pacchetto di distribuzione eseguendo il *. deploy. cmd* file, è possibile modificare il *SetParameters* file per fornire un valore per l'indirizzo dell'endpoint servizio insieme ai valori per il proprietà che sono state parametrizzate automaticamente.

Per informazioni dettagliate su come creare un *Parameters. XML* del file, vedere [procedura: utilizzare i parametri per configurare le impostazioni quando un pacchetto di distribuzione è installato](https://msdn.microsoft.com/library/ff398068.aspx). La procedura denominata **da usare i parametri di distribuzione per le impostazioni del file Web. config** vengono fornite istruzioni dettagliate.

## <a name="modifying-the-setparametersxml-file"></a>Modifica del File SetParameters

Se si prevede di distribuire manualmente il pacchetto di applicazione web&#x2014;eseguendo il *. deploy. cmd* file o eseguendo MSDeploy.exe dalla riga di comando&#x2014;non c'è niente per impedire all'utente di modificare manualmente il  *SetParameters* file prima della distribuzione. Tuttavia, se Usa una soluzione su scala aziendale, potrebbe essere necessario distribuire un pacchetto di applicazione web come parte di un processo di compilazione e distribuzione più grande e automatizzato. In questo scenario, è necessario Microsoft Build Engine (MSBuild) per modificare la *SetParameters* file automaticamente. È possibile farlo usando MSBuild **XmlPoke** attività.

Il [soluzione di esempio Contact Manager](the-contact-manager-solution.md) viene illustrato questo processo. Gli esempi di codice che seguono sono stati modificati per mostrare solo i dettagli rilevanti per questo esempio.

> [!NOTE]
> Per una panoramica più ampia del modello di file di progetto nella soluzione di esempio e un'introduzione ai file di progetto personalizzato in generale, vedere [informazioni sul File di progetto](understanding-the-project-file.md) e [informazioni sul processo di compilazione](understanding-the-build-process.md).


In primo luogo, i valori dei parametri di interesse sono definiti come proprietà nel file di progetto specifici dell'ambiente (ad esempio, *Env-Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Per indicazioni su come personalizzare i file di progetto specifici dell'ambiente per ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Successivamente, il *Publish.proj* file Importa queste proprietà. Poiché ogni *SetParameters* file è associato un *. deploy. cmd* file e si desidera in definitiva il file di progetto per richiamare ogni *. deploy. cmd* file, il progetto file crea un MSBuild *articoli* per ogni *. deploy. cmd* del file e definisce le proprietà di interesse come *metadati dell'elemento*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


In questo caso:

- Il **ParametersXml** metadati valore indica la posizione del *SetParameters* file.
- Il **IisWebAppName** valore è il percorso IIS in cui si desidera distribuire l'applicazione web.
- Il **MembershipDBConnectionString** valore è la stringa di connessione per il database di appartenenza e il **MembershipDBConnectionName** valore è la **nome** attributo del parametro corrispondente nel *SetParameters* file.
- Il **ServiceEndpointValue** valore è l'indirizzo dell'endpoint del servizio WCF nel server di destinazione e il **ServiceEndpointParamName** valore è l'attributo del nome del parametro corrispondente in il *SetParameters* file.

Infine, nella *Publish.proj* file, il **PublishWebPackages** viene utilizzato come destinazione il **XmlPoke** attività per modificare questi valori nel *SetParameters* file.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Si noterà che ogni **XmlPoke** attività specifica quattro valori di attributo:

- Il **XmlInputPath** attributo indica l'attività dove trovare il file che si desidera modificare.
- Il **Query** attributo è una query XPath che identifica il nodo XML che si desidera modificare.
- Il **valore** attributo è il nuovo valore si desidera inserire il nodo XML selezionato.
- Il **condizione** attributo è i criteri in cui l'attività deve eseguire o meno. In questi casi, la condizione garantisce che non si tenti di inserire un valore null o vuoto nel *SetParameters* file.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto il ruolo del *SetParameters* file e illustrato come viene generato quando si compila un progetto di applicazione web. Spiegato come è possibile parametrizzare le impostazioni aggiuntive tramite l'aggiunta di un *Parameters. XML* file al progetto. Spiega anche come è possibile modificare il *SetParameters* file come parte di un processo di compilazione automatizzata, più grande, tramite il **XmlPoke** attività nei file di progetto.

L'argomento successivo, [distribuzione di pacchetti Web](deploying-web-packages.md), viene descritto come distribuire un pacchetto web eseguendo la *. deploy. cmd* file o utilizzando MSDeploy.exe comandi direttamente. In entrambi i casi, è possibile specificare il *SetParameters* file come parametro di distribuzione.

## <a name="further-reading"></a>Ulteriori informazioni

Per informazioni su come creare pacchetti web, vedere [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md). Per indicazioni su come distribuire un pacchetto web, vedere [distribuzione di pacchetti Web](deploying-web-packages.md). Per una procedura dettagliata su come creare un *Parameters. XML* del file, vedere [procedura: utilizzare i parametri per configurare le impostazioni quando un pacchetto di distribuzione è installato](https://msdn.microsoft.com/library/ff398068.aspx).

Per altre informazioni generali sulla parametrizzazione nella distribuzione Web, vedere [parametrizzazione distribuzione Web in azione](https://go.microsoft.com/?linkid=9805119) (post di blog).

> [!div class="step-by-step"]
> [Precedente](building-and-packaging-web-application-projects.md)
> [Successivo](deploying-web-packages.md)
