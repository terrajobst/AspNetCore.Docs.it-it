---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Impostazione delle applicazioni Web non in linea con Web distribuisce | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come eseguire un'applicazione web in modalità offline per la durata di una distribuzione automatizzata utilizzando l'Avvis Web Internet Information Services (IIS)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 1e9e5e9fef99a1179c1b830a42df6e6cb6b0e69d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382513"
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Impostazione delle applicazioni Web non in linea con Web distribuire
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come eseguire un'applicazione web in modalità offline per la durata di una distribuzione automatizzata tramite lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web). Gli utenti che esplorano per l'applicazione web vengono reindirizzati a un *App\_offline.htm* fino al completamento della distribuzione di file.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

In molti scenari, è opportuno eseguire un'applicazione web non in linea mentre si apportano modifiche ai componenti correlati, ad esempio database o i servizi web. In genere, in IIS e ASP.NET, tale scopo, inserire un file denominato *App\_offline.htm* nella cartella radice dell'applicazione web o sito Web IIS. Il *App\_offline.htm* file è un file HTML standard e in genere contiene un semplice messaggio che informa l'utente che il sito è temporaneamente non disponibile a causa di manutenzione. Mentre il *App\_offline.htm* file esistente nella cartella radice del sito Web, IIS viene reindirizzato automaticamente tutte le richieste per il file. Una volta terminata l'esecuzione di aggiornamenti, si rimuove il *App\_offline.htm* file e il sito Web viene ripresa l'elaborazione delle richieste come di consueto.

Quando si usa distribuzione Web per eseguire distribuzioni automatizzate o passo-passo per un ambiente di destinazione, è possibile incorporare aggiungendo e rimuovendo il *App\_offline.htm* file nel processo di distribuzione. A tale scopo, è necessario completare queste attività di alto livello:

- Nel file di progetto Microsoft Build Engine (MSBuild) che consente di controllare il processo di distribuzione, creare una destinazione di MSBuild che consente di copiare un' *App\_offline.htm* file al server di destinazione prima di qualsiasi attività di distribuzione iniziare.
- Aggiungere un'altra destinazione di MSBuild che rimuove i *App\_offline.htm* file dal server di destinazione dopo aver complete tutte le attività di distribuzione.
- Nel progetto di applicazione web, creare un *. WPP* file che assicura che un' *App\_offline.htm* file viene aggiunto al pacchetto di distribuzione quando la distribuzione Web viene richiamata.

In questo argomento illustrerà come eseguire queste procedure. Le attività e procedure dettagliate in questo argomento si presuppongono di avere già creato una soluzione che contiene almeno un progetto di applicazione web e l'uso di un file di progetto personalizzati per controllare il processo di distribuzione come descritto in [distribuzione Web di Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). In alternativa, è possibile usare la [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) soluzione per seguire gli esempi illustrati nell'argomento di esempio.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Aggiunta di un'App\_File Offline per un progetto di applicazione Web

È necessario completare la prima attività consiste nell'aggiungere un' *App\_offline* file al progetto di applicazione web:

- Per evitare che il file interferisca con il processo di sviluppo (non si desidera l'applicazione sia definitivamente offline), è consigliabile assegnare un nome diverso da *App\_offline.htm*. Ad esempio, è possibile denominare il file *App\_offline-Microsoft Dynamics*.
- Per impedire che i file vengano distribuiti come-è, è necessario impostare l'azione di compilazione su **None**.

**Aggiungere un'App\_file offline per un progetto di applicazione web**

1. Aprire la soluzione in Visual Studio 2010.
2. Nel **Esplora soluzioni** finestra, fare clic sul progetto di applicazione web, scegliere **Add**, quindi fare clic su **nuovo elemento**.
3. Nel **Aggiungi nuovo elemento** finestra di dialogo **pagina HTML**.
4. Nel **nome** , digitare **App\_offline-Microsoft Dynamics**, quindi fare clic su **Aggiungi**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Aggiungere semplice codice HTML per informare gli utenti che l'applicazione non è disponibile e quindi salvare il file. Non includono eventuali tag sul lato server (ad esempio, eventuali tag che hanno il prefisso "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. Nel **Esplora soluzioni** finestra, fare doppio clic su nuovo file e quindi fare clic su **proprietà**.
7. Nel **delle proprietà** finestra, nel **azione di compilazione** riga, seleziona **None**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Distribuzione e l'eliminazione di un'App\_File Offline

Il passaggio successivo consiste nel modificare la logica di distribuzione per copiare il file nel server di destinazione all'inizio del processo di distribuzione e rimuoverlo al termine.

> [!NOTE]
> La procedura seguente si presuppone che si sta usando un file di progetto MSBuild personalizzato per controllare il processo di distribuzione, come descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Se si distribuisce direttamente da Visual Studio, è necessario usare un approccio diverso. Sayed Ibrahim Hashimi descrive un approccio in [richiedere Your Offline durante la pubblicazione di App Web come](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Per distribuire un' *App\_offline* file a un sito Web IIS di destinazione, è necessario richiamare MSDeploy.exe utilizzando la [distribuzione Web **contentPath** provider](https://technet.microsoft.com/library/dd569034(WS.10).aspx). Il **contentPath** provider supporta entrambi i percorsi di directory fisica e i percorsi di applicazione o sito Web IIS, che rende la scelta ideale per la sincronizzazione di un file tra una cartella di progetto di Visual Studio e un'applicazione web IIS. Per distribuire il file, il comando di MSDeploy sarà analogo al seguente:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Per rimuovere il file dal sito di destinazione al termine del processo di distribuzione, il comando di MSDeploy sarà analogo al seguente:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Per automatizzare i comandi seguenti come parte di un processo di compilazione e distribuzione, è necessario integrarli nel file di progetto MSBuild personalizzato. La procedura seguente descrive come eseguire questa operazione.

**Per distribuire ed eliminare un'App\_file offline**

1. In Visual Studio 2010, aprire il file di progetto MSBuild che controlla il processo di distribuzione. Nel [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) soluzione di esempio, si tratta di *Publish.proj* file.
2. Nella radice **Project** elemento, creare un nuovo **PropertyGroup** elemento per archiviare le variabili per il *App\_offline* distribuzione:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. Il **SourceRoot** proprietà è definita in un punto nel *Publish.proj* file. Indica il percorso della cartella radice per il contenuto di origine rispetto al percorso corrente&#x2014;in altre parole, relativo al percorso del *Publish.proj* file.
4. Il **contentPath** provider non accetta percorsi file relativi, pertanto è necessario ottenere un percorso assoluto al file di origine prima di poterla distribuire. È possibile usare la [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) attività per eseguire questa operazione.
5. Aggiungere un nuovo **destinazione** elemento denominato **GetAppOfflineAbsolutePath**. All'interno di questa destinazione, usare il **ConvertToAbsolutePath** attività per ottenere un percorso assoluto per il *App\_offline-template* file nella cartella del progetto.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Questa destinazione richiede il percorso relativo per il *App\_offline-template* file nella cartella del progetto e lo salva in una nuova proprietà come percorso assoluto del file. Il **BeforeTargets** attributo specifica che si desidera che questa destinazione da eseguire prima il **DeployAppOffline** target, che potrà essere creata nel passaggio successivo.
7. Aggiungere una nuova destinazione denominata **DeployAppOffline**. All'interno di questa destinazione, richiamare il comando MSDeploy.exe che distribuisce i *App\_offline* file al server web di destinazione.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. In questo esempio, il **ContactManagerIisPath** proprietà è definita in un' posizione nel file di progetto. Si tratta semplicemente un percorso applicazione IIS, nel formato *[nome sito Web IIS] / [nome applicazione]*. Inclusione di una condizione nel database di destinazione consente agli utenti di passare il *App\_offline* distribuzione attiva o disattiva la modifica di un valore della proprietà o fornendo un parametro della riga di comando.
9. Aggiungere una nuova destinazione denominata **DeleteAppOffline**. All'interno di questa destinazione, richiamare il comando MSDeploy.exe che rimuove i *App\_offline* file dal server web di destinazione.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. L'attività finale consiste nel richiamare queste nuove destinazioni in momenti appropriati durante l'esecuzione del file di progetto. È possibile farlo in vari modi. Ad esempio, nel *Publish.proj* file, il **FullPublishDependsOn** proprietà specifica un elenco di destinazioni che devono essere eseguite in ordine quando la **FullPublish** predefinito viene richiamata la destinazione.
11. Modificare il file di progetto MSBuild per richiamare il **DeployAppOffline** e **DeleteAppOffline** destinazioni nei punti appropriati nel processo di pubblicazione.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Quando si esegue il file di progetto MSBuild personalizzate, il *App\_offline* file verrà distribuito al server immediatamente dopo una compilazione corretta. Si verrà quindi essere eliminato dal server dopo aver complete tutte le attività di distribuzione.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Aggiunta di un'App\_File Offline per i pacchetti di distribuzione

A seconda della modalità di configurazione della distribuzione, qualsiasi il contenuto esistente nella destinazione IIS web dell'applicazione&#x2014;, ad esempio il *App\_offline.htm* file&#x2014;possono essere eliminati automaticamente quando si distribuisce un sito web pacchetto per la destinazione. Per garantire che il *App\_offline.htm* file rimane sul posto per la durata della distribuzione, è necessario includere il file all'interno del pacchetto di distribuzione web stesso inoltre per il file direttamente all'inizio della distribuzione il processo di distribuzione.

- Se sono stati eseguiti le attività precedenti in questo argomento, verranno aggiunti i *App\_offline.htm* file al progetto di applicazione web in un nome file diverso (usassimo *App\_ offline-Microsoft Dynamics*) e l'azione di compilazione è sarà stato impostato su **None**. Queste modifiche sono necessarie per evitare che il file da interferiscano con lo sviluppo e debug. Di conseguenza, è necessario personalizzare il processo di creazione del pacchetto per verificare che il *App\_offline.htm* file è incluso nel pacchetto di distribuzione web.

La Pipeline di pubblicazione sul Web (WPP) usa un elenco di elementi denominato **FilesForPackagingFromProject** per compilare un elenco di file che devono essere incluse nel pacchetto di distribuzione web. È possibile personalizzare il contenuto dei pacchetti web aggiungendo gli elementi di questo elenco. A tale scopo, è necessario completare i passaggi generali:

1. Creare un file di progetto personalizzato denominato *[nome progetto].wpp.targets* nella stessa cartella del file di progetto.

    > [!NOTE]
    > Il *. WPP* file deve essere inserito nella stessa cartella del file di progetto dell'applicazione web&#x2014;, ad esempio, *ContactManager.Mvc.csproj*&#x2014;anziché nella stessa cartella personalizzati file di progetto usati per controllare il processo di compilazione e distribuzione.
2. Nel *. WPP* del file, creare una nuova destinazione di MSBuild che esegue *prima* il **CopyAllFilesToSingleFolderForPackage** destinazione. Questa è la destinazione WPP che compila un elenco di elementi da includere nel pacchetto.
3. Nella destinazione di nuovo, creare un **ItemGroup** elemento.
4. Nel **ItemGroup** elemento, aggiungere un **FilesForPackagingFromProject** elemento e specificare la *App\_offline.htm* file.

Il *. WPP* file sarà analogo al seguente:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Questi sono i punti chiave della nota in questo esempio:

- Il **BeforeTargets** attributo inserisce questa destinazione in WPP specificando che deve essere eseguito immediatamente prima di **CopyAllFilesToSingleFolderForPackage** destinazione.
- Il **FilesForPackagingFromProject** elemento Usa le **DestinationRelativePath** valore dei metadati per rinominare il file da *App\_offline-Microsoft Dynamics* per *App\_offline.htm* quando questo viene aggiunto all'elenco.

La procedura seguente viene illustrato come aggiungere quanto segue *. WPP* file a un progetto di applicazione web.

**Per aggiungere un. WPP file a un pacchetto di distribuzione web**

1. Aprire la soluzione in Visual Studio 2010.
2. Nel **Esplora soluzioni** finestra, fare clic sul nodo del progetto di applicazione web (ad esempio, **ContactManager.Mvc**), scegliere **Add**e quindi fare clic su **Nuovo elemento**.
3. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona la **File XML** modello.
4. Nel **nome** , digitare *[nome progetto] * * *.wpp.targets** (ad esempio, **ContactManager.Mvc.wpp.targets**), quindi fare clic su **Aggiungi**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Se si aggiunge un nuovo elemento al nodo radice di un progetto, il file viene creato nella stessa cartella del file di progetto. È possibile verificarlo aprendo la cartella in Windows Explorer.
5. Nel file, aggiungere il markup di MSBuild descritto in precedenza.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Salvare e chiudere il *[nome progetto].wpp.targets* file.

La prossima volta che pacchetto e compilazione progetto di applicazione web, WPP rileva automaticamente le *. WPP* file. Il *App\_offline-Microsoft Dynamics* file verrà incluso nel pacchetto di distribuzione web risultante come *App\_offline.htm*.

> [!NOTE]
> Se la distribuzione non riesce, il *App\_offline.htm* file rimarranno nella posizione e l'applicazione rimarrà offline. In genere si tratta del comportamento desiderato. Per riportare l'applicazione torna online, è possibile eliminare il *App\_offline.htm* file dal server web. In alternativa, se si correggere eventuali errori, esecuzione una corretta distribuzione, il *App\_offline.htm* file verrà rimosso.


## <a name="conclusion"></a>Conclusione

Questo argomento descrive come creare un'applicazione web in modalità offline per la durata di una distribuzione, pubblicando un *App\_offline.htm* file al server di destinazione all'inizio del processo di distribuzione e la rimozione, vedere il fine. È stato anche descritto come includere un *App\_offline.htm* file in un pacchetto di distribuzione web.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sulla creazione di pacchetti e processo di distribuzione, vedere [compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [configurazione dei parametri per una distribuzione pacchetto Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), e [ Distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Se si pubblicano le applicazioni web direttamente da Visual Studio, piuttosto che usa l'approccio di file progetto MSBuild personalizzata descritto in queste esercitazioni, è necessario usare un approccio leggermente diverso per portare offline l'applicazione durante la pubblicazione processo. Per altre informazioni, vedere [come eseguire l'app web in modalità offline durante la pubblicazione](https://go.microsoft.com/?linkid=9805135) (post di blog).

> [!div class="step-by-step"]
> [Precedente](excluding-files-and-folders-from-deployment.md)
> [Successivo](running-windows-powershell-scripts-from-msbuild-project-files.md)
