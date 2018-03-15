---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Aggiunta di contenuto al controllo del codice sorgente | Documenti Microsoft
author: jrjlee
description: In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010. Viene descritto come aggiungere soluzioni e progetti per un progetto team...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: d46e2697d10ca27f8e08533350a6e7f2354b4a43
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
<a name="adding-content-to-source-control"></a>Aggiunta di contenuto al controllo del codice sorgente
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010. Viene descritto come aggiungere soluzioni e progetti in un progetto team in TFS e viene descritto come aggiungere dipendenze esterne come Framework o assembly di controllo del codice sorgente.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [soluzione responsabile contatto](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

## <a name="task-overview"></a>Panoramica di Task

Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere contenuto al controllo del codice sorgente. Per aggiungere una soluzione al controllo del codice sorgente in TFS, è necessario completare questi passaggi di alto livello:

- Connettersi a un progetto team.
- Mappare la struttura di cartelle del progetto team sul server a una struttura di cartelle nel computer locale.
- Controllo del codice sorgente, aggiungere la soluzione e il relativo contenuto.
- Aggiungere le eventuali dipendenze esterne a controllo del codice sorgente.

Questo argomento viene illustrato come eseguire queste procedure.

Le attività e procedure dettagliate in questo argomento si presuppongono che è già stato creato un nuovo progetto team TFS per gestire il contenuto. Per ulteriori informazioni sulla creazione di un nuovo progetto team, vedere [la creazione di un progetto Team in TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Che consente di eseguire queste procedure?

Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere e modificare il contenuto all'interno dei progetti team specifico.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Connettersi a un progetto Team e creare un Mapping della cartella

Prima di aggiungere contenuto al controllo del codice sorgente, è necessario connettersi a un progetto team e creare un mapping tra la struttura di cartelle nel server e il file system nel computer locale.

**Per connettersi a un progetto team ed eseguire il mapping di un percorso locale**

1. Nella workstation di sviluppo, aprire Visual Studio 2010.
2. In Visual Studio, sul **Team** menu, fare clic su **Connetti a Team Foundation Server**.

    > [!NOTE]
    > Se già stata configurata una connessione a un server TFS, è possibile omettere i passaggi da 3 a 6.
3. Nel **connessione al progetto Team** la finestra di dialogo, fare clic su **server**.
4. Nel **Aggiungi/Rimuovi Team Foundation Server** la finestra di dialogo, fare clic su **Aggiungi**.
5. Nel **Aggiungi Team Foundation Server** la finestra di dialogo, fornire i dettagli dell'istanza di TFS, quindi fare clic su **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. Nel **Aggiungi/Rimuovi Team Foundation Server** la finestra di dialogo, fare clic su **Chiudi**.
7. Nel **Connetti a progetto Team** nella finestra di dialogo selezionare l'istanza TFS si desidera connettersi, per selezionare il team di progetto di raccolta, selezionare il progetto team si desidera aggiungere e quindi fare clic su **Connetti**.

    ![](adding-content-to-source-control/_static/image2.png)
8. Nel **Team Explorer** finestra, espandere il progetto team e quindi fare doppio clic su **controllo del codice sorgente**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Nel **Esplora controllo codice sorgente** scheda, fare clic su **non mappato**.

    ![](adding-content-to-source-control/_static/image4.png)
10. Nel **mappa** della finestra di dialogo di **cartella locale** casella, selezionare (o creare) una cartella locale per agire come la cartella radice per il progetto team e quindi fare clic su **mappa**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Quando viene chiesto di scaricare i file di origine, fare clic su **Sì**.

    ![](adding-content-to-source-control/_static/image6.png)

A questo punto, è stato eseguito il mapping di cartella sul lato server per il progetto team in una cartella locale nella workstation di sviluppo. Una volta anche scaricato qualsiasi contenuto esistente dal progetto team per la struttura di cartelle locali. È ora possibile iniziare a aggiungere i propri dati al controllo del codice sorgente.

## <a name="add-projects-and-solutions-to-source-control"></a>Aggiungere soluzioni e progetti al controllo del codice sorgente

Per aggiungere soluzioni e progetti al controllo del codice sorgente, è innanzitutto necessario spostarli nella cartella per il progetto team mappato nel computer locale. È quindi possibile archiviare il contenuto da sincronizzare le aggiunte con il server.

**Per aggiungere progetti al controllo del codice sorgente**

1. Nella workstation di sviluppo, spostare i progetti e soluzioni in una posizione appropriata all'interno della struttura di cartella mappata per il progetto team.

    > [!NOTE]
    > Molte organizzazioni disporrà di un approccio consigliato per come progetti e soluzioni devono essere organizzate nel controllo del codice sorgente. Per istruzioni su come strutturare delle cartelle, vedere [How To: struttura del controllo cartelle del codice sorgente in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Aprire la soluzione in Visual Studio 2010.
3. Nel **Esplora** finestra rapida della soluzione e quindi fare clic su **Aggiungi soluzione al controllo del codice sorgente**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > In alcuni casi, a seconda di come l'organizzazione ama contenuto della struttura in TFS, devi aggiungere progetti al controllo del codice sorgente singolarmente per fornire un controllo più accurato tramite la modalità di organizzazione del codice sorgente.
4. Verificare che il **Esplora controllo codice sorgente** scheda consente di visualizzare il contenuto è stato aggiunto all'interno della struttura di cartelle del server per il progetto team.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > Il **Esplora controllo codice sorgente** scheda consente di visualizzare il contenuto di una richiesta di conferma senza ulteriore poiché la soluzione è stato aggiunto a una cartella mappata nel file system locale. Se la soluzione in una posizione non mappata, verrà chiesto di specificare i percorsi delle cartelle in TFS e file system locale.
5. Nel **Esplora controllo codice sorgente** nella scheda il **cartelle** riquadro, fare clic sul progetto team (ad esempio, **ContactManager**) e quindi fare clic su **archivia Le modifiche in sospeso**.
6. Nel **Check-In-file di origine** nella finestra di dialogo digitare un commento e quindi fare clic su **Archivia**.

    ![](adding-content-to-source-control/_static/image9.png)

A questo punto è stato aggiunto la soluzione al controllo del codice sorgente in TFS.

## <a name="add-external-dependencies-to-source-control"></a>Aggiungere le dipendenze esterne a controllo del codice sorgente

Quando si aggiunge un progetto o una soluzione a controllo del codice sorgente, tutti i file e cartelle all'interno del progetto o soluzione verranno inoltre aggiunto. Tuttavia, in molti casi, progetti e soluzioni anche basano su dipendenze esterne, ad esempio assembly locale, per funzionare correttamente. È necessario aggiungere tutte le risorse di controllo del codice sorgente per consentire di Team Build e gli altri membri del team di sviluppatori di compilare il codice correttamente.

Ad esempio, la struttura di cartelle per la soluzione di esempio Contact Manager include una cartella denominata pacchetti. Contiene l'assembly e varie risorse di supporto per ADO.NET Entity Framework 4.1. La cartella di pacchetti non è parte della soluzione di gestione di contatto, ma la soluzione non verrà compilato correttamente senza di esso. Per abilitare Team Build compilare la soluzione, è necessario aggiungere la cartella di pacchetti al controllo del codice sorgente.

> [!NOTE]
> L'inclusione di una cartella di pacchetti è tipico di ciò che accade quando si aggiunge la Entity Framework, o le risorse simile, alla soluzione mediante l'estensione NuGet per Visual Studio 2010.


**Per aggiungere contenuto non di progetto al controllo del codice sorgente**

1. Verificare che gli elementi che si desidera aggiungere (ad esempio, la cartella pacchetti) siano in una posizione appropriata all'interno di una cartella mappata nel file system locale.
2. In Visual Studio 2010, nel **Team Explorer** finestra, espandere il progetto team e quindi fare doppio clic su **controllo del codice sorgente**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Nel **Esplora controllo codice sorgente** nella scheda il **cartelle** riquadro, selezionare la cartella che contiene l'elemento o gli elementi si desidera aggiungere.
4. Fare clic su di **Aggiungi elementi alla cartella** pulsante.

    ![](adding-content-to-source-control/_static/image11.png)
5. Nel **aggiungere al controllo del codice sorgente** finestra di dialogo, selezionare la cartella o elementi che si desidera aggiungere e quindi fare clic su **Avanti**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Nel **tutti gli elementi** scheda, selezionare gli elementi necessari sono stati esclusi automaticamente (ad esempio, assembly) e quindi fare clic su **includono elementi**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Nel **elementi da aggiungere** scheda, verificare che sono elencati tutti i file che si desidera includere e quindi fare clic su **fine**.

    ![](adding-content-to-source-control/_static/image14.png)
8. Nel **Esplora controllo codice sorgente** finestra, fare clic su di **Archivia** pulsante.

    ![](adding-content-to-source-control/_static/image15.png)
9. Nel **Check-In-file di origine** nella finestra di dialogo digitare un commento e quindi fare clic su **Archivia**.

A questo punto, è stato aggiunto le dipendenze esterne per la soluzione al controllo del codice sorgente.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come connettersi a un progetto team, eseguire il mapping di una struttura di cartelle e aggiungere contenuto al controllo del codice sorgente. Per ulteriori informazioni su come lavorare con gli elementi nel controllo del codice sorgente, vedere [uso del controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).

L'argomento successivo, [configurazione di un Server di compilazione TFS per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md), viene descritto come preparare un server TFS Team Build per compilare e distribuire la soluzione.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sull'uso di controllo del codice sorgente in TFS, vedere [uso del controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).

>[!div class="step-by-step"]
[Precedente](creating-a-team-project-in-tfs.md)
[Successivo](configuring-a-tfs-build-server-for-web-deployment.md)
