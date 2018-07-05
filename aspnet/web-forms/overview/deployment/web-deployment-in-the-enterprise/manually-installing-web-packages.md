---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Installazione manuale dei pacchetti Web | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come importare manualmente un pacchetto di distribuzione web in Internet Information Services (IIS). La compilazione di argomento e l'applicazione Web di creazione dei pacchetti...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 7fd8060104ca4e02919a3fbac135edb3e9396c64
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833005"
---
<a name="manually-installing-web-packages"></a>Installazione manuale dei pacchetti Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come importare manualmente un pacchetto di distribuzione web in Internet Information Services (IIS).
> 
> L'argomento [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md) descritto come IIS strumento di distribuzione Web (distribuzione Web), insieme a Microsoft Build Engine (MSBuild) e Web pubblicazione Pipeline (WPP), consente creare un pacchetto di progetti applicazione Web in un unico file zip. Questo file, comunemente noto come un pacchetto di distribuzione web (o semplicemente un pacchetto di distribuzione), contiene tutte le informazioni di configurazione e il contenuto che è necessario per creare nuovamente l'applicazione web in un server web IIS.
> 
> Dopo aver creato un pacchetto di distribuzione web, è possibile pubblicarlo in un server IIS in vari modi. In molti scenari, è opportuno sfruttare i vantaggi dei punti di integrazione tra MSBuild, WPP e distribuzione Web per creare e installare i pacchetti web in modalità remota come parte di un processo di compilazione e distribuzione automatizzato o passo a passo. Questo processo è descritto nella [distribuzione di pacchetti Web](deploying-web-packages.md). Tuttavia, questo non è sempre possibile. Si supponga che si vuole distribuire un'applicazione web in un ambiente di produzione con connessione Internet. Per motivi di sicurezza, un ambiente di produzione è in molto meno probabile trovarsi dietro un firewall in una subnet separata dal server di compilazione, in una rete perimetrale (detta anche DMZ e subnet schermata). In molti casi, l'ambiente di produzione sarà in un dominio separato o in una rete isolata fisicamente.
> 
> In questi scenari, l'unica opzione possibile trasferire il pacchetto web nel server di destinazione e importarlo manualmente in IIS. Sebbene questo approccio impedisce la distribuzione automatica, è comunque una tecnica molto efficace per la pubblicazione di un'applicazione web&#x2014;sufficiente copiare un unico file zip nel server web e consentono di eseguire il processo di importazione usando una procedura guidata.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

## <a name="task-overview"></a>Panoramica di Task

È necessario completare queste attività di alto livello per importare un pacchetto di distribuzione web in IIS:

- Creare un pacchetto di distribuzione web usando il MSBuild della riga di comando, Team Build o Visual Studio 2010.
- Copiare il pacchetto web al server web di destinazione.
- Utilizzare l'importazione guidata pacchetto di applicazione in Gestione IIS per installare il pacchetto web e fornire valori per le variabili, ad esempio le stringhe di connessione e gli endpoint di servizio.

In questo argomento illustrerà come eseguire queste procedure. Le attività e procedure dettagliate in questo argomento presuppongono che si abbia già familiarità con i concetti alla base dei pacchetti web, distribuzione Web e il sito. Per altre informazioni, vedere [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).

> [!NOTE]
> In questo argomento è particolarmente utile in combinazione con [configurare un Server Web per la pubblicazione distribuzione Web (distribuzione Offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), che illustra come installare i componenti necessari e preparare un sito Web IIS per l'importazione del pacchetto.


## <a name="create-a-web-deployment-package"></a>Creare un pacchetto di distribuzione Web

La prima attività consiste nel creare un pacchetto di distribuzione web per il progetto di applicazione web da distribuire. È possibile creare pacchetti web in diversi modi.

**Approccio 1: Creare un pacchetto come parte del processo di compilazione con Visual Studio**

È possibile configurare il progetto di applicazione web per creare un pacchetto di distribuzione web dopo ogni compilazione tramite il **pubblicazione/creazione pacchetto Web** scheda nelle pagine delle proprietà di progetto. Questo processo è descritto nella [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).

**Approccio 2: Creare un pacchetto come parte del processo di compilazione con MSBuild**

Se si compila il progetto di applicazione web usando MSBuild direttamente, tramite un file di progetto MSBuild personalizzato o dalla riga di comando, è possibile creare un pacchetto di distribuzione web come parte del processo di compilazione includendo la **DeployOnBuild = true** e **DeployTarget = pacchetto** proprietà nel comando. Questo processo è descritto nella [informazioni sul processo di compilazione](understanding-the-build-process.md).

**Approccio 3: Creare un pacchetto su richiesta in Visual Studio**

È possibile creare un pacchetto di distribuzione web per un progetto di applicazione web in qualsiasi momento in Visual Studio 2010. A questo scopo, nella **Esplora soluzioni** finestra, fare clic sul progetto di applicazione web e quindi fare clic su **genera pacchetto di distribuzione**.

![](manually-installing-web-packages/_static/image1.png)

**Approccio 4: Creare un pacchetto su richiesta dalla riga di comando**

È possibile creare un pacchetto di distribuzione web dalla riga di comando quando si richiama il **pacchetto** destinazione nel progetto di applicazione web tramite MSBuild. Il comando sarà analogo al seguente:


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


A seconda di quale approccio si usa, il risultato finale è lo stesso. WPP crea un pacchetto di distribuzione web come un file con estensione zip, insieme a varie risorse di supporto, nella cartella di output per il progetto di applicazione web.

![](manually-installing-web-packages/_static/image2.png)

Quando si prevede di importare il pacchetto web manualmente, è necessario solo il file con estensione zip. Copiare questo file nel server web di destinazione ed è possibile iniziare il processo di importazione.

## <a name="import-a-web-package-into-iis"></a>Importare un pacchetto Web in IIS

È possibile utilizzare la procedura seguente per importare un pacchetto di distribuzione web dal file system locale in un sito Web IIS. Prima di eseguire questa procedura, assicurarsi di avere:

- Copiare il pacchetto di distribuzione web al server web.
- Configurare un server web IIS per ospitare l'applicazione.

Per altre informazioni su come configurare un server web IIS per supportare i pacchetti di distribuzione web, vedere [configurare un Server Web per la pubblicazione distribuzione Web (distribuzione Offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**Per importare un pacchetto di distribuzione web mediante Gestione IIS**

1. In Gestione IIS nel **connessioni** riquadro, fare clic sul sito Web IIS, scegliere **Distribuisci**, quindi fare clic su **Importa applicazione**.

    ![](manually-installing-web-packages/_static/image3.png)
2. Nella creazione guidata pacchetto di applicazione di importazione, sul **selezionare il pacchetto** pagina, selezionare il percorso del pacchetto di distribuzione web e quindi fare clic su **successivo**.
3. Nel **selezionare il contenuto del pacchetto** pagina, deselezionare qualsiasi contenuto che non richiedono e quindi fare clic su **successivo**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > In molti casi, non si desideri importare tutto ciò che viene fornito con un pacchetto di distribuzione web. Ad esempio, non è possibile consentire a distribuzione Web sostituire il database associato.  
    > Il **concedere le autorizzazioni** voci impostare le autorizzazioni per file system di destinazione per assicurarsi che l'identità del pool di applicazioni possono accedere alla cartella fisica che archivia il contenuto del sito Web. Inoltre, l'utente di autenticazione anonima è concesse autorizzazioni di lettura alla cartella per consentire all'applicazione di usare i file di tipo MIME Multipurpose Internet Mail Extensions (). Se si preferisce, è possibile rimuovere queste voci e configurare manualmente le autorizzazioni.
4. Nel **Immetti informazioni sul pacchetto di applicazione** pagina, fornire le informazioni richieste.

    ![](manually-installing-web-packages/_static/image5.png)
5. Quando si crea un pacchetto web, WPP analizza il file di configurazione per l'applicazione e rileva eventuali variabili, ad esempio le stringhe di connessione e gli endpoint di servizio. In questo caso:

    1. **Percorso applicazione** è il percorso IIS in cui si desidera installare l'applicazione. Questa impostazione è comune a tutti i pacchetti di distribuzione che crea il sito.
    2. **Indirizzo dell'Endpoint servizio ContactService** è l'indirizzo che l'applicazione deve usare per comunicare con il servizio WCF distribuito. Questa impostazione corrisponde a una voce nella *Web. config* file.
    3. Il primo **stringa di connessione** impostazione è la stringa di connessione che distribuzione Web deve usare per distribuire il database associato all'applicazione (in questo caso un database di appartenenza ASP.NET). Questa impostazione corrisponde all'impostazione sul **pubblicazione/creazione pacchetto SQL** scheda in Visual Studio.
    4. La seconda **stringa di connessione** impostazione è la stringa di connessione che verrà effettivamente usata per comunicare con il database quando è attivo e in esecuzione. Ciò corrisponde a una voce della stringa di connessione nel *Web. config* file.

        > [!NOTE]
        > Per ulteriori informazioni sulla provenienza di questi parametri, vedere [configurazione dei parametri per una distribuzione pacchetto Web](configuring-parameters-for-web-package-deployment.md).
6. Scegliere **Avanti**.
7. Se non si tratta la prima volta che è stato distribuito l'applicazione con il sito Web, verrà richiesto di specificare se si desidera eliminare tutto il contenuto esistente prima dell'installazione. Scegliere l'opzione più adatta alle proprie esigenze e quindi fare clic su **successivo**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Al termine dell'installazione del pacchetto IIS, fare clic su **fine**.

    ![](manually-installing-web-packages/_static/image7.png)

A questo punto, è stato pubblicato l'applicazione web in IIS.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come importare un pacchetto di distribuzione web in un sito Web IIS mediante Gestione IIS. Questo approccio per la pubblicazione di applicazioni web è appropriato quando i vincoli di sicurezza o di infrastruttura rendere distribuzione remota Impossibile o indesiderata.

## <a name="further-reading"></a>Ulteriori informazioni

Per indicazioni su come configurare un server web IIS per supportare importazione manuale di un pacchetto web, vedere [configurare un Server Web per la pubblicazione distribuzione Web (distribuzione Offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Per istruzioni generali sulla distribuzione dei pacchetti web, vedere [procedura dettagliata: distribuzione di un progetto di applicazione Web usando un pacchetto di distribuzione Web (parte 1 di 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Precedente](creating-and-running-a-deployment-command-file.md)
