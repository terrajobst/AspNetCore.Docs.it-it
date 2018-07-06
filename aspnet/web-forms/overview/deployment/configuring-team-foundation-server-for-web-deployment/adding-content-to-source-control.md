---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Aggiunta di contenuto al controllo del codice sorgente | Microsoft Docs
author: jrjlee
description: In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010. Viene descritto come aggiungere soluzioni e progetti a un progetto di un team...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 9fdb2e37f2925273b457157b634865d93e865098
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838371"
---
<a name="adding-content-to-source-control"></a>Aggiunta di contenuto a controllo del codice sorgente
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010. Viene descritto come aggiungere soluzioni e progetti a un progetto team in TFS e viene spiegato come aggiungere le dipendenze esterne, ad esempio Framework o assembly al controllo del codice sorgente.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

## <a name="task-overview"></a>Panoramica di Task

Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere contenuto al controllo del codice sorgente. Per aggiungere una soluzione a controllo del codice sorgente in TFS, è necessario completare i passaggi generali:

- Connettersi a un progetto team.
- Mappare la struttura di cartelle del progetto team sul server a una struttura di cartelle nel computer locale.
- Aggiungere la soluzione e il relativo contenuto al controllo del codice sorgente.
- Aggiungere le dipendenze esterne a controllo del codice sorgente.

In questo argomento illustrerà come eseguire queste procedure.

Le attività e procedure dettagliate in questo argomento si presuppongono di avere già creato un nuovo progetto team TFS per gestire il contenuto. Per altre informazioni sulla creazione di un nuovo progetto team, vedere [la creazione di un progetto Team in TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Che consente di eseguire queste procedure?

Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere e modificare il contenuto all'interno dei progetti team specifico.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Connettersi a un progetto Team e creare un Mapping della cartella

Prima di aggiungere contenuto al controllo del codice sorgente, è necessario connettersi a un progetto team e creare un mapping tra la struttura di cartelle nel server e il file system nel computer locale.

**Per connettersi a un progetto team ed eseguire il mapping di un percorso locale**

1. Nella workstation per gli sviluppatori, aprire Visual Studio 2010.
2. In Visual Studio sul **Team** menu, fare clic su **Connetti a Team Foundation Server**.

    > [!NOTE]
    > Se già stata configurata una connessione a un server TFS, è possibile omettere i passaggi da 3 a 6.
3. Nel **connessione al progetto Team** finestra di dialogo, fare clic su **server**.
4. Nel **Aggiungi/Rimuovi Team Foundation Server** finestra di dialogo, fare clic su **Add**.
5. Nel **Aggiungi Team Foundation Server** finestra di dialogo, fornire i dettagli dell'istanza di TFS e quindi fare clic su **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. Nel **Aggiungi/Rimuovi Team Foundation Server** finestra di dialogo, fare clic su **Chiudi**.
7. Nel **Connetti a progetto Team** finestra di dialogo, selezionare l'istanza di TFS si desidera connettersi, per selezionare il team di progetto selezionare il progetto team da aggiungere alla raccolta e quindi fare clic su **Connect**.

    ![](adding-content-to-source-control/_static/image2.png)
8. Nel **Team Explorer** finestra, espandere il progetto team e quindi fare doppio clic su **controllo del codice sorgente**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Nel **Esplora controllo codice sorgente** scheda, fare clic su **non mappato**.

    ![](adding-content-to-source-control/_static/image4.png)
10. Nel **mappa** nella finestra di dialogo il **cartella locale** casella, selezionare (o creare) una cartella locale per agire come cartella radice per il progetto team e quindi fare clic su **mappa**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Quando viene chiesto di scaricare i file di origine, fare clic su **Sì**.

    ![](adding-content-to-source-control/_static/image6.png)

A questo punto, è stato eseguito il mapping di cartella sul lato server per il progetto team in una cartella locale nella workstation di sviluppo. Inoltre scaricato qualsiasi contenuto esistente dal progetto team per la struttura di cartelle locali. È ora possibile iniziare ad aggiungere il proprio contenuto al controllo del codice sorgente.

## <a name="add-projects-and-solutions-to-source-control"></a>Aggiungere soluzioni e progetti al controllo del codice sorgente

Per aggiungere soluzioni e progetti al controllo del codice sorgente, è innanzitutto necessario per spostarli nella cartella mappata per il progetto team nel computer locale. È quindi possibile archiviare il contenuto da sincronizzare le aggiunte con il server.

**Aggiungere progetti al controllo del codice sorgente**

1. Nella workstation per gli sviluppatori, spostare i progetti e soluzioni in una posizione appropriata all'interno della struttura di cartella mappata per il progetto team.

    > [!NOTE]
    > Molte organizzazioni presentano un approccio preferito per la modalità progetti e soluzioni devono essere organizzate in controllo del codice sorgente. Per indicazioni su come le cartelle di struttura, vedere [How To: struttura del controllo cartelle del codice sorgente in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Aprire la soluzione in Visual Studio 2010.
3. Nel **Esplora soluzioni** finestra, fare doppio clic la soluzione e quindi fare clic su **Aggiungi soluzione al controllo del codice sorgente**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > In alcuni casi, a seconda del modo in cui l'organizzazione mi piace al contenuto della struttura in TFS, devi aggiungere progetti al controllo del codice sorgente singolarmente per fornire un controllo più accurato sul modo in cui è organizzato il codice sorgente.
4. Verificare che il **Esplora controllo codice sorgente** scheda viene visualizzato il contenuto è stato aggiunto all'interno della struttura di cartelle del server per il progetto team.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > Il **Esplora controllo codice sorgente** scheda consente di visualizzare i contenuti con la richiesta alcuna ulteriore perché è stata aggiunta la soluzione in una cartella mappata nel file system locale. Se la soluzione in una posizione non mappata, sarebbe chiesto di specificare percorsi di cartelle in TFS e nel file system locale.
5. Nel **Esplora controllo codice sorgente** nella scheda il **cartelle** riquadro, fare clic sul progetto team (ad esempio, **ContactManager**) e quindi fare clic su **Check-In Le modifiche in sospeso**.
6. Nel **Check-In-file di origine** della finestra di dialogo digitare un commento e quindi fare clic su **Archivia**.

    ![](adding-content-to-source-control/_static/image9.png)

A questo punto è stato aggiunto la soluzione al controllo del codice sorgente in TFS.

## <a name="add-external-dependencies-to-source-control"></a>Aggiungere le dipendenze esterne a controllo del codice sorgente

Quando si aggiunge un progetto o una soluzione al controllo del codice sorgente, eventuali file e cartelle all'interno del progetto o soluzione verranno inoltre aggiunto. Tuttavia, in molti casi, progetti e soluzioni anche basano su dipendenze esterne, ad esempio assembly locali, per funzionare correttamente. È necessario aggiungere tutte le risorse di controllo del codice sorgente per consentire a Team Build e altri membri del team di sviluppo compilare il codice completato.

Ad esempio, la struttura di cartelle per la soluzione di esempio Contact Manager include una cartella denominata dei pacchetti. Contiene l'assembly e varie risorse di supporto per ADO.NET Entity Framework 4.1. La cartella dei pacchetti non fa parte della soluzione Contact Manager, ma la soluzione non verrà compilato correttamente senza di essa. Per abilitare il Team Build compilare la soluzione, è necessario aggiungere la cartella di pacchetti al controllo del codice sorgente.

> [!NOTE]
> L'inclusione di una cartella di pacchetti è tipico di ciò che accade quando si aggiunge di Entity Framework, o risorse simili, alla soluzione mediante l'estensione NuGet per Visual Studio 2010.


**Per aggiungere contenuto non di progetto al controllo del codice sorgente**

1. Assicurarsi che gli elementi da aggiungere (ad esempio, la cartella dei pacchetti) siano in una posizione appropriata all'interno di una cartella mappata nel file system locale.
2. In Visual Studio 2010, nelle **Team Explorer** finestra, espandere il progetto team e quindi fare doppio clic su **controllo del codice sorgente**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Nel **Esplora controllo codice sorgente** nella scheda il **cartelle** riquadro, selezionare la cartella che contiene l'elemento o gli elementi si desidera aggiungere.
4. Scegliere il **Aggiungi elementi alla cartella** pulsante.

    ![](adding-content-to-source-control/_static/image11.png)
5. Nel **aggiungere al controllo del codice sorgente** finestra di dialogo, selezionare la cartella o elementi che si desidera aggiungere e quindi fare clic su **successivo**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Nel **esclusi gli elementi** scheda, selezionare gli elementi necessari sono stati esclusi automaticamente (ad esempio, gli assembly) e quindi fare clic su **Includi elementi**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Nel **elementi da aggiungere** scheda, verificare che siano elencati tutti i file da includere e quindi fare clic su **fine**.

    ![](adding-content-to-source-control/_static/image14.png)
8. Nel **Esplora controllo codice sorgente** finestra, fare clic sui **Archivia** pulsante.

    ![](adding-content-to-source-control/_static/image15.png)
9. Nel **Check-In-file di origine** della finestra di dialogo digitare un commento e quindi fare clic su **Archivia**.

A questo punto, si sono aggiunte le dipendenze esterne per la soluzione al controllo del codice sorgente.

## <a name="conclusion"></a>Conclusione

In questo argomento descrive come connettersi a un progetto team, eseguire il mapping di una struttura di cartelle e aggiungere contenuto al controllo del codice sorgente. Per altre informazioni su come lavorare con gli elementi nel controllo del codice sorgente, vedere [tramite controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).

Argomento successivo [configurazione di un Server di compilazione TFS per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md), viene descritto come preparare un server TFS Team Build per compilare e distribuire la soluzione.

## <a name="further-reading"></a>Ulteriori informazioni

Per informazioni più complete sull'uso di controllo del codice sorgente in TFS, vedere [tramite controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Precedente](creating-a-team-project-in-tfs.md)
> [Successivo](configuring-a-tfs-build-server-for-web-deployment.md)
