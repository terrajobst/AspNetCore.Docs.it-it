---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Acquisire le applicazioni Web non in linea con Web distribuire | Documenti Microsoft
author: jrjlee
description: "In questo argomento viene descritto come eseguire un'applicazione web in modalità offline per tutta la durata della distribuzione automatica usando l'Avvis Web Internet Information Services (IIS)..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: a0c59245eedbf53f367949e12dd83e2611f44fc4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Creazione di applicazioni Web Offline con Web distribuire
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come eseguire un'applicazione web in modalità offline per tutta la durata della distribuzione automatica tramite lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web). Gli utenti che esplorano all'applicazione web vengono reindirizzati a un *App\_offline.htm* file fino al completamento della distribuzione.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [soluzione responsabile contatto](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due progetti #x 2014; & file contenente una compilare le istruzioni che si applicano a ogni ambiente di destinazione e quella contenente impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

In molti scenari, si desidera disconnettere un'applicazione web mentre si apportano modifiche ai componenti correlati, ad esempio database o i servizi web. In genere, in IIS e ASP.NET, tale scopo, inserire un file denominato *App\_offline.htm* nella cartella radice dell'applicazione web o sito Web IIS. Il *App\_offline.htm* file è un file HTML standard e in genere contiene un semplice messaggio che informa l'utente che il sito è temporaneamente non disponibile per interventi di manutenzione. Mentre il *App\_offline.htm* file esistente nella cartella radice del sito Web, IIS verrà reindirizzata automaticamente tutte le richieste per il file. Una volta terminata l'esecuzione di aggiornamenti, si rimuove il *App\_offline.htm* file e il sito Web riprende normalmente le risposte alle richieste.

Quando si utilizza distribuzione Web per eseguire le distribuzioni automatica o passo-passo per un ambiente di destinazione, si desidera incorporare l'aggiunta e rimozione di *App\_offline.htm* file nel processo di distribuzione. A tale scopo, è necessario completare queste attività di alto livello:

- Nel file di progetto di Microsoft Build Engine (MSBuild) utilizzato per controllare il processo di distribuzione, creare una destinazione di MSBuild che viene copiato un *App\_offline.htm* file nel server di destinazione prima di qualsiasi attività di distribuzione iniziare.
- Aggiungere un'altra destinazione di MSBuild che rimuove il *App\_offline.htm* file dal server di destinazione una volta completate tutte le attività di distribuzione.
- Nel progetto di applicazione web, creare un *. wpp.targets* file che assicura che un *App\_offline.htm* file viene aggiunto al pacchetto di distribuzione quando viene richiamata distribuzione Web.

Questo argomento viene illustrato come eseguire queste procedure. Le attività e procedure dettagliate in questo argomento si presuppongono di aver già creato una soluzione che contiene almeno un progetto di applicazione web e l'uso di un file di progetto personalizzati per controllare il processo di distribuzione come descritto in [distribuzione Web nel Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). In alternativa, è possibile utilizzare il [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) soluzione per seguire gli esempi nella sezione di esempio.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Aggiunta di un'App\_File Offline per un progetto di applicazione Web

È necessario completare la prima attività consiste nell'aggiungere un *App\_offline* file al progetto di applicazione web:

- Per impedire che il file di interferire con il processo di sviluppo (non si desidera impostare in modo permanente non in linea dell'applicazione), è necessario chiamare questo metodo un elemento diverso da *App\_offline.htm*. Ad esempio, è possibile specificare il nome del file *App\_offline di Microsoft Dynamics*.
- Per impedire che il file verrà distribuito come-è, è necessario impostare l'azione di compilazione su **Nessuno**.

**Per aggiungere un'App\_file offline per un progetto di applicazione web**

1. Aprire la soluzione in Visual Studio 2010.
2. Nel **Esplora** finestra del mouse sul progetto di applicazione web, scegliere **Aggiungi**, quindi fare clic su **nuovo elemento**.
3. Nel **Aggiungi nuovo elemento** nella finestra di dialogo **pagina HTML**.
4. Nel **nome** digitare **App\_offline di Microsoft Dynamics**, quindi fare clic su **Aggiungi**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Aggiungere il codice HTML semplice per informare gli utenti che non è disponibile l'applicazione e quindi salvare il file. Non includere tag qualsiasi lato server (ad esempio, i tag che sono precedute dal prefisso "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. Nel **Esplora** finestra, fare doppio clic su nuovo file e quindi fare clic su **proprietà**.
7. Nel **proprietà** finestra, nel **azione di compilazione** riga, selezionare **Nessuno**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Distribuzione e l'eliminazione di un'App\_File Offline

Il passaggio successivo consiste nella modifica di una logica di distribuzione per copiare il file nel server di destinazione all'inizio del processo di distribuzione e rimuoverlo al termine.

> [!NOTE]
> La procedura seguente si presuppone che si usi un file di progetto MSBuild personalizzato per controllare il processo di distribuzione, come descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Se si distribuisce direttamente da Visual Studio, è necessario usare un approccio diverso. Sayed Ibrahim Hashimi viene descritto un approccio in [come eseguire l'App non in linea durante la pubblicazione sul Web](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Per distribuire un *App\_offline* file a un sito Web IIS di destinazione, è necessario richiamare MSDeploy.exe mediante il [distribuzione Web **contentPath** provider](https://technet.microsoft.com/en-us/library/dd569034(WS.10).aspx). Il **contentPath** provider supporta entrambi i percorsi di directory fisica e i percorsi del sito Web o un'applicazione IIS, che rende la soluzione ideale per la sincronizzazione di un file in una cartella di progetto di Visual Studio e un'applicazione web IIS. Per distribuire il file, il comando di MSDeploy sarà analogo al seguente:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Per rimuovere il file dal sito di destinazione alla fine del processo di distribuzione, il comando di MSDeploy sarà analogo al seguente:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Per automatizzare questi comandi come parte di un processo di compilazione e distribuzione, è necessario integrarli in file di progetto MSBuild personalizzato. La procedura seguente viene descritto come eseguire questa operazione.

**Per distribuire ed eliminare un'App\_file offline**

1. In Visual Studio 2010, aprire il file di progetto MSBuild che controlla il processo di distribuzione. Nel [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) soluzione di esempio, si tratta di *Publish.proj* file.
2. Nella radice **progetto** elemento, creare un nuovo **PropertyGroup** elemento da archiviare le variabili per il *App\_offline* distribuzione:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. Il **SourceRoot** proprietà definita altrove nel *Publish.proj* file. Indica il percorso della cartella radice per il relativo il percorso corrente & #x 2014; in altre parole, relativo alla posizione del contenuto di origine di *Publish.proj* file.
4. Il **contentPath** provider non accetterà i percorsi di file relativo, pertanto è necessario ottenere un percorso assoluto al file di origine prima di poterla distribuire. È possibile utilizzare il [ConvertToAbsolutePath](https://msdn.microsoft.com/en-us/library/bb882668.aspx) attività per eseguire questa operazione.
5. Aggiungere un nuovo **destinazione** elemento denominato **GetAppOfflineAbsolutePath**. All'interno di questa destinazione, utilizzare il **ConvertToAbsolutePath** attività per ottenere un percorso assoluto per il *App\_offline modello* file nella cartella del progetto.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. La destinazione accetta il percorso relativo di *App\_offline modello* file nella cartella del progetto e lo salva in una nuova proprietà come un percorso assoluto. Il **BeforeTargets** attributo specifica che la destinazione da eseguire prima il **DeployAppOffline** destinazione, verrà creata nel passaggio successivo.
7. Aggiungere una nuova destinazione denominata **DeployAppOffline**. All'interno di questa destinazione, richiamare il comando di MSDeploy.exe che distribuisce il *App\_offline* file al server web di destinazione.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. In questo esempio, il **ContactManagerIisPath** proprietà definita altrove nel file di progetto. Si tratta semplicemente un percorso applicazione IIS, nel formato *[nome sito Web IIS] / [nome applicazione]*. L'inclusione di una condizione nella destinazione consente agli utenti di passare il *App\_offline* distribuzione o disattivare la modifica di un valore della proprietà o fornendo un parametro della riga di comando.
9. Aggiungere una nuova destinazione denominata **DeleteAppOffline**. All'interno di questa destinazione, richiamare il comando di MSDeploy.exe che rimuove le *App\_offline* file dal server web di destinazione.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. L'attività finale consiste nel richiamare queste nuove destinazioni in punti appropriati durante l'esecuzione del file di progetto. È possibile farlo in vari modi. Ad esempio, nel *Publish.proj* file, il **FullPublishDependsOn** proprietà specifica un elenco di destinazioni che devono essere eseguiti in ordine quando il **FullPublish** predefinito viene richiamata la destinazione.
11. Modificare il file di progetto MSBuild per richiamare il **DeployAppOffline** e **DeleteAppOffline** destinazioni in punti appropriati nel processo di pubblicazione.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Quando si esegue il file di progetto MSBuild personalizzata, il *App\_offline* file verrà distribuito al server immediatamente dopo una compilazione corretta. Verrà quindi eliminato dal server dopo aver complete tutte le attività di distribuzione.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Aggiunta di un'App\_File Offline ai pacchetti di distribuzione

A seconda della modalità di configurazione della distribuzione, qualsiasi contenuto esistente di un'applicazione web IIS di destinazione & #x 2014; ad esempio il *App\_offline.htm* #x 2014; & file potrebbero essere eliminati automaticamente quando si distribuisce un sito web pacchetto per la destinazione. Per garantire che il *App\_offline.htm* file rimane attivo per la durata della distribuzione, è necessario includere il file all'interno del pacchetto di distribuzione web stesso inoltre per il file direttamente all'inizio della distribuzione il processo di distribuzione.

- Se è stato seguito le attività precedenti in questo argomento, verranno aggiunti il *App\_offline.htm* file al progetto di applicazione web in un nome file diverso (utilizzassimo *App\_ non in linea di Microsoft Dynamics*) e verrà installata l'operazione di compilazione **Nessuno**. Queste modifiche sono necessari per evitare che il file da interferenze con lo sviluppo e debug. Di conseguenza, è necessario personalizzare il processo di creazione di pacchetti per assicurarsi che il *App\_offline.htm* file è incluso nel pacchetto di distribuzione web.

La Pipeline di pubblicazione sul Web (WPP) usa un elenco di elementi denominato **FilesForPackagingFromProject** per compilare un elenco di file che devono essere incluse nel pacchetto di distribuzione web. È possibile personalizzare il contenuto dei pacchetti web tramite l'aggiunta di elementi personalizzati a questo elenco. A tale scopo, è necessario completare questi passaggi di alto livello:

1. Creare un file di progetto personalizzato denominato *.wpp.targets [nome progetto]* nella stessa cartella del file di progetto.

    > [!NOTE]
    > Il *. wpp.targets* file deve essere inserito nella stessa cartella del file di progetto applicazione web & #x 2014; ad esempio, *ContactManager.Mvc.csproj*& #x 2014; anziché nella stessa cartella file di progetto personalizzato utilizzati per il controllo della compilazione e il processo di distribuzione.
2. Nel *. wpp.targets* file, creare una nuova destinazione di MSBuild che esegue *prima* il **CopyAllFilesToSingleFolderForPackage** destinazione. Questa è la destinazione del sito che compila un elenco di elementi da includere nel pacchetto.
3. Nella nuova destinazione, creare un **ItemGroup** elemento.
4. Nel **ItemGroup** elemento, aggiungere un **FilesForPackagingFromProject** elemento e specificare il *App\_offline.htm* file.

Il *. wpp.targets* file dovrebbe essere simile alla seguente:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Questi sono i punti chiave della nota in questo esempio:

- Il **BeforeTargets** attributo inserisce la destinazione in WPP specificando che deve essere eseguito immediatamente prima di **CopyAllFilesToSingleFolderForPackage** destinazione.
- Il **FilesForPackagingFromProject** elemento viene utilizzato il **DestinationRelativePath** valore dei metadati per rinominare il file da *App\_offline di Microsoft Dynamics* per *App\_offline.htm* quando vengono aggiunti all'elenco.

La procedura successiva viene illustrato come aggiungere questo *. wpp.targets* file a un progetto di applicazione web.

**Per aggiungere un. wpp.targets file a un pacchetto di distribuzione web**

1. Aprire la soluzione in Visual Studio 2010.
2. Nel **Esplora** finestra, fare doppio clic su un nodo di progetto applicazione web (ad esempio, **ContactManager.Mvc**), scegliere **Aggiungi**e quindi fare clic su **Nuovo elemento**.
3. Nel **Aggiungi nuovo elemento** la finestra di dialogo, seleziona il **File XML** modello.
4. Nel **nome** digitare *[nome progetto]***. wpp.targets** (ad esempio, **ContactManager.Mvc.wpp.targets**), quindi fare clic su  **Aggiungere**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Se si aggiunge un nuovo elemento al nodo radice di un progetto, il file viene creato nella stessa cartella del file di progetto. Per verificare, aprire la cartella in Esplora risorse.
5. Nel file, aggiungere il markup di MSBuild descritto in precedenza.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Salvare e chiudere il *.wpp.targets [nome progetto]* file.

Alla successiva esecuzione del pacchetto e di compilazione progetto applicazione web, WPP consentirà di rilevare automaticamente il *. wpp.targets* file. Il *App\_offline di Microsoft Dynamics* file verrà incluso nel pacchetto di distribuzione web risultante come *App\_offline.htm*.

> [!NOTE]
> Se la distribuzione non riesce, il *App\_offline.htm* rimarrà file sul posto e l'applicazione rimarrà non in linea. Si tratta in genere il comportamento desiderato. Per attivare l'applicazione torna online, è possibile eliminare il *App\_offline.htm* file dal server web. In alternativa, se si correggere eventuali errori, esecuzione una corretta distribuzione, il *App\_offline.htm* file verrà rimosso.


## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come eseguire un'applicazione web in modalità offline per tutta la durata di una distribuzione, pubblicazione un *App\_offline.htm* file nel server di destinazione all'inizio del processo di distribuzione e alla rimozione di fine. Inoltre descritto come includere un *App\_offline.htm* file in un pacchetto di distribuzione web.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla creazione di pacchetti e processo di distribuzione, vedere [compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [configurazione dei parametri per la distribuzione di pacchetto Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), e [ Distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Se la pubblicazione di applicazioni web direttamente da Visual Studio, piuttosto che utilizza l'approccio di file progetto MSBuild personalizzato descritto in queste esercitazioni, è necessario utilizzare un approccio leggermente diverso per portare l'applicazione durante la pubblicazione processo. Per ulteriori informazioni, vedere [come portare offline l'app web durante la pubblicazione](https://go.microsoft.com/?linkid=9805135) (post di blog).

>[!div class="step-by-step"]
[Precedente](excluding-files-and-folders-from-deployment.md)
[Successivo](running-windows-powershell-scripts-from-msbuild-project-files.md)
