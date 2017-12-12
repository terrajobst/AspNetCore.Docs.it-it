---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Installare manualmente i pacchetti Web | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come importare manualmente un pacchetto di distribuzione web in Internet Information Services (IIS). L'edificio di argomento e il pacchetto Web il...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 0ab0b4c24c1771a21c45bac011b5f156cb15d28a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="manually-installing-web-packages"></a>Installare manualmente i pacchetti Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come importare manualmente un pacchetto di distribuzione web in Internet Information Services (IIS).
> 
> L'argomento [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md) descritto come IIS strumento di distribuzione Web (distribuzione Web), in combinazione con Microsoft Build Engine (MSBuild) e Web pubblicazione Pipeline (WPP), consente di comprimere il progetti di applicazione Web in un unico file zip. Questo file, comunemente noto come un pacchetto di distribuzione web (o semplicemente un pacchetto di distribuzione) contiene tutte le informazioni di configurazione e il contenuto che è necessario per creare nuovamente l'applicazione web in un server web IIS.
> 
> Dopo aver creato un pacchetto di distribuzione web, è possibile pubblicarlo in un server IIS in vari modi. In molti scenari, sarà possibile sfruttare i punti di integrazione tra MSBuild, WPP e distribuzione Web per creare e installare i pacchetti web in modalità remota come parte di un processo di compilazione e distribuzione automatico o singolo passaggio. Questo processo è descritto in [distribuzione di pacchetti Web](deploying-web-packages.md). Tuttavia, ciò non è sempre possibile. Si supponga che si desidera distribuire un'applicazione web in un ambiente di produzione con connessione Internet. Per motivi di sicurezza, in almeno molto probabilmente da un firewall in una subnet separata dal server di compilazione, in una rete perimetrale (detta anche DMZ e subnet schermata) è un ambiente di produzione. In molti casi, l'ambiente di produzione sarà in un dominio diverso o in una rete isolata fisicamente.
> 
> In questi scenari, l'unica opzione possibile trasferire il pacchetto web nel server di destinazione e importarlo manualmente in IIS. Sebbene questo approccio impedisce la distribuzione automatica, è ancora una tecnica estremamente efficiente per la pubblicazione di un'applicazione web & #x 2014, è sufficiente copiare un unico file zip per il server web e consentono di eseguire il processo di importazione utilizzando una procedura guidata.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [soluzione responsabile contatto](the-contact-manager-solution.md)& #x 2014; per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

## <a name="task-overview"></a>Panoramica di Task

È necessario completare queste attività di alto livello per importare un pacchetto di distribuzione web in IIS:

- Creare un pacchetto di distribuzione web mediante il MSBuild della riga di comando, Team Build o Visual Studio 2010.
- Copiare il pacchetto web al server web di destinazione.
- Utilizzare l'importazione guidata pacchetto di applicazione in Gestione IIS per installare il pacchetto web e fornire valori per le variabili come stringhe di connessione e gli endpoint del servizio.

Questo argomento viene illustrato come eseguire queste procedure. Le attività e procedure dettagliate in questo argomento si presuppongono che si ha già familiarità con i concetti di base WPP, distribuzione Web e i pacchetti web. Per ulteriori informazioni, vedere [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).

> [!NOTE]
> In questo argomento viene utilizzato meglio in combinazione con [configurare un Server Web per distribuire pubblicazione sul Web (non in linea distribuzione)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), che illustra come installare i componenti necessari e preparare un sito Web IIS per l'importazione del pacchetto.


## <a name="create-a-web-deployment-package"></a>Creare un pacchetto di distribuzione Web

La prima attività consiste nel creare un pacchetto di distribuzione web per il progetto di applicazione web da distribuire. È possibile creare pacchetti web in diversi modi.

**Approccio 1: Creare un pacchetto come parte del processo di compilazione con Visual Studio**

È possibile configurare il progetto di applicazione web per creare un pacchetto di distribuzione web dopo ogni compilazione tramite la **pubblicazione/creazione pacchetto Web** scheda nelle pagine delle proprietà di progetto. Questo processo è descritto in [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).

**Approccio 2: Creare un pacchetto come parte del processo di compilazione con MSBuild**

Se si compila il progetto di applicazione web tramite MSBuild direttamente tramite un file di progetto MSBuild personalizzato oppure dalla riga di comando, è possibile creare un pacchetto di distribuzione web come parte del processo di compilazione, includendo il **DeployOnBuild = true** e **DeployTarget = pacchetto** proprietà nel comando. Questo processo è descritto in [comprendere il processo di compilazione](understanding-the-build-process.md).

**Approccio 3: Creare un pacchetto su richiesta in Visual Studio**

È possibile creare un pacchetto di distribuzione web per un progetto di applicazione web in qualsiasi momento in Visual Studio 2010. A tale scopo, nel **Esplora** , mouse sul progetto di applicazione web e quindi fare clic su **compila pacchetto di distribuzione**.

![](manually-installing-web-packages/_static/image1.png)

**Approccio 4: Creare un pacchetto su richiesta dalla riga di comando**

È possibile creare un pacchetto di distribuzione web dalla riga di comando richiamando il **pacchetto** destinazione del progetto di applicazione web tramite MSBuild. Il comando sarà analogo al seguente:


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


A seconda di quale approccio si utilizza, il risultato finale è lo stesso. WPP crea un pacchetto di distribuzione web come file zip, insieme alle varie risorse di supporto, nella cartella di output per il progetto di applicazione web.

![](manually-installing-web-packages/_static/image2.png)

Quando si prevede di importare manualmente il pacchetto web, è necessario solo il file zip. Copiare questo file nel server web di destinazione ed è possibile iniziare il processo di importazione.

## <a name="import-a-web-package-into-iis"></a>Importare un pacchetto Web in IIS

È possibile utilizzare la procedura seguente per importare un pacchetto di distribuzione web dal file system locale in un sito Web IIS. Prima di eseguire questa procedura, assicurarsi di disporre di:

- Copiare il pacchetto di distribuzione web al server web.
- Configurare un server web IIS per ospitare l'applicazione.

Per ulteriori informazioni sulla configurazione di un server web IIS per supportare i pacchetti di distribuzione web, vedere [configurare un Server Web per distribuire pubblicazione sul Web (non in linea distribuzione)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**Per importare un pacchetto di distribuzione web utilizzando Gestione IIS**

1. In Gestione IIS, nel **connessioni** riquadro, il pulsante destro del sito Web IIS, scegliere **Distribuisci**, quindi fare clic su **Importa applicazione**.

    ![](manually-installing-web-packages/_static/image3.png)
2. Nella procedura guidata Importa applicazione del pacchetto, nel **selezionare il pacchetto** pagina, selezionare il percorso del pacchetto di distribuzione web e quindi fare clic su **Avanti**.
3. Nel **selezionare il contenuto del pacchetto** deselezionare qualsiasi contenuto che non necessarie e quindi fare clic su **Avanti**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > In molti casi, potrebbe non essere desidera importare tutto ciò che viene fornito con un pacchetto di distribuzione web. Ad esempio, si potrebbe non si desidera consentire la distribuzione Web sostituire il database associato.  
    > Il **concedere le autorizzazioni** voci impostare le autorizzazioni per file system di destinazione per assicurarsi che l'identità del pool di applicazioni possono accedere alla cartella fisica che archivia il contenuto del sito Web. Inoltre, l'utente di autenticazione anonima è concesse autorizzazioni di lettura alla cartella per consentire l'applicazione di file di tipo MIME Multipurpose Internet Mail Extensions (). Se si preferisce, è possibile eliminare queste voci e configurare le autorizzazioni manualmente.
4. Nel **Immetti informazioni sul pacchetto di applicazione** pagina, fornire le informazioni richieste.

    ![](manually-installing-web-packages/_static/image5.png)
5. Quando si crea un pacchetto web, WPP analizza il file di configurazione per l'applicazione e rileva eventuali variabili, ad esempio le stringhe di connessione e gli endpoint del servizio. In questo caso:

    1. **Percorso dell'applicazione** è il percorso di IIS in cui si desidera installare l'applicazione. Questa impostazione è comune a tutti i pacchetti di distribuzione che crea WPP.
    2. **Indirizzo dell'Endpoint servizio ContactService** è l'indirizzo che l'applicazione deve utilizzare per comunicare con il servizio WCF distribuito. Questa impostazione corrisponde a una voce di *Web. config* file.
    3. Il primo **stringa di connessione** impostazione è la stringa di connessione che distribuzione Web deve utilizzare per distribuire il database associato all'applicazione (in questo caso un database delle appartenenze ASP.NET). Questa impostazione corrisponde all'impostazione sul **pubblicazione/creazione pacchetto SQL** scheda in Visual Studio.
    4. Il secondo **stringa di connessione** impostazione è la stringa di connessione che l'applicazione verrà effettivamente utilizzata per comunicare con il database quando è attivo e in esecuzione. Corrisponde a una stringa di connessione nel *Web. config* file.

        > [!NOTE]
        > Per ulteriori informazioni su dove provengono questi parametri, vedere [configurazione dei parametri per la distribuzione di pacchetto Web](configuring-parameters-for-web-package-deployment.md).
6. Scegliere **Avanti**.
7. Se questo non è la prima volta che è stato distribuito l'applicazione con il sito Web, verrà chiesto di specificare se si desidera eliminare tutto il contenuto esistente prima dell'installazione. Scegliere l'opzione più adatta alle proprie esigenze e quindi fare clic su **Avanti**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Al termine dell'installazione del pacchetto di IIS, fare clic su **fine**.

    ![](manually-installing-web-packages/_static/image7.png)

A questo punto, è stato pubblicato correttamente l'applicazione web in IIS.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come importare un pacchetto di distribuzione web in un sito Web IIS mediante Gestione IIS. Questo approccio per la pubblicazione delle applicazioni web è appropriato quando i vincoli di sicurezza o infrastruttura di distribuzione remota Impossibile o indesiderata.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni su come configurare un server web IIS per supportare l'importazione manuale di un pacchetto web, vedere [configurare un Server Web per distribuire pubblicazione sul Web (non in linea distribuzione)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Per istruzioni generali sulla distribuzione di pacchetti web, vedere [procedura dettagliata: distribuzione di un progetto di applicazione Web utilizzando un pacchetto di distribuzione Web (parte 1 di 4)](https://msdn.microsoft.com/en-us/library/dd483479.aspx).

>[!div class="step-by-step"]
[Precedente](creating-and-running-a-deployment-command-file.md)
