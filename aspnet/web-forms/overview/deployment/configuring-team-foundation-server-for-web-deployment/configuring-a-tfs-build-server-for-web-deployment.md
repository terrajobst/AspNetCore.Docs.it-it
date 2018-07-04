---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Configurazione di un TFS Server di compilazione per la distribuzione Web | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come preparare un server di compilazione di Team Foundation Server (TFS) per compilare e distribuire le soluzioni con Team Build e la Internet Informaz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 12a18f601a75b607c97c46ecb7f68947ca3a342b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382533"
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Configurazione di un Server di compilazione TFS per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento descrive come preparare un server di compilazione di Team Foundation Server (TFS) per compilare e distribuire le soluzioni con Team Build e lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web).


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

Per preparare un server di compilazione per compilare e distribuire le soluzioni, è necessario:

- Installare e configurare il servizio di compilazione TFS.
- Installare Visual Studio 2010.
- Installare eventuali prodotti o i componenti necessari per compilare la soluzione, come le versioni di .NET Framework o di ASP.NET MVC.
- Installare la funzionalità distribuzione Web 2.0 o versione successiva.

In questo argomento illustrerà come eseguire queste procedure o puntano ad altre risorse in cui sono presenti. Le attività e procedure dettagliate in questo argomento presuppongono che:

- Si inizia con una compilazione pulita server che esegue Windows Server 2008 R2 Service Pack 1.
- Il server è a un dominio con un indirizzo IP statico.
- Aver installato il livello applicazione TFS in un server separato, come descritto in [distribuzione Web aziendale: panoramica dello Scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Che consente di eseguire queste procedure?

Nella maggior parte dei casi, un amministratore di TFS sarà responsabile per la configurazione server di compilazione. In alcuni casi, il team di sviluppo può assumere la proprietà del server di compilazione specifici.

## <a name="install-and-configure-the-tfs-build-service"></a>Installare e configurare il servizio di compilazione TFS

Quando si configura un server di compilazione, è la prima attività per installare e configurare il servizio di compilazione TFS. Come parte di questo processo, è necessario:

- Il servizio di compilazione TFS di installare e configurare un account del servizio. Qualsiasi attività di compilazione, tra cui la distribuzione, verrà eseguito usando l'identità dell'account del servizio di compilazione.
- Creare un *controller di compilazione* e uno o più *gli agenti di compilazione*. Ogni controller di compilazione gestisce un set di agenti di compilazione. Quando si accodare una compilazione, il controller di compilazione assegna l'attività di compilazione a un agente di compilazione disponibili. Ogni raccolta di progetti team in TFS viene mappato a un singolo controller di compilazione.
- Configurare una cartella di ricezione per l'output di compilazione. Si tratta di una condivisione di rete. Qualsiasi output, ad esempio i pacchetti di distribuzione web di compilazione, vengono inviati per la cartella di ricezione.

Il [amministrazione di Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) capitolo su MSDN contiene tutte le risorse necessarie per eseguire queste attività:

- Per una panoramica concettuale di Team Foundation Build, tra cui il servizio di compilazione, i controller di compilazione e agenti di compilazione, vedere [comprendere un sistema di Team Foundation Build](https://msdn.microsoft.com/library/dd793166.aspx).
- Per informazioni sull'installazione e configurazione del servizio di compilazione, vedere [configurare un computer di compilazione](https://msdn.microsoft.com/library/ms181712.aspx).
- Per informazioni sulla creazione di controller di compilazione, vedere [creare e usare un Controller di compilazione](https://msdn.microsoft.com/library/ee330987.aspx).
- Per informazioni sulla creazione di agenti di compilazione, vedere [creare e usare gli agenti di compilazione](https://msdn.microsoft.com/library/bb399135.aspx).
- Per informazioni sulla creazione e configurazione di cartelle di ricezione, vedere [configurare cartelle di ricezione](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Installare i componenti e i prodotti necessari

Per abilitare il server di compilazione compilare soluzioni, è necessario installare tutti i prodotti, componenti o gli assembly che la soluzione richiede. Prima di installare eventuali componenti della piattaforma web, è necessario installare Visual Studio 2010 (qualsiasi versione) nel server di compilazione. Ciò garantisce che i file di destinazione principale Microsoft Build Engine (MSBuild) e i file di destinazione Web pubblicazione Pipeline (WPP) siano disponibili per il servizio di compilazione. Il programma di installazione di Visual Studio deve inoltre installare distribuzione Web, che è necessario se si prevede di distribuire i pacchetti web come parte del processo di compilazione.

Il modo migliore per installare i componenti della piattaforma web comune è usare il [instalace Webové Platformy](https://go.microsoft.com/?linkid=9805118). Ciò garantisce che si sta installando la versione più recente di ogni prodotto e automaticamente anche rileva e installa tutti i prerequisiti per ogni prodotto. Nel caso del [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) soluzione, si deve usare l'installazione guidata piattaforma Web per installare questi prodotti e componenti:

- **.NET Framework 4.0**. Ciò è necessario eseguire le applicazioni che sono state create in questa versione di .NET Framework.
- **Web Deployment Tool 2.1 o versioni successive**. Ciò consente di installare distribuzione Web (e il relativo file eseguibile sottostante, MSDeploy.exe) nel server. Come parte di questo processo, installa e avvia il servizio agente di distribuzione Web. Questo servizio ti permette di distribuire i pacchetti web da un computer remoto.
- **ASP.NET MVC 3**. Ciò consente di installare gli assembly che è necessario eseguire le applicazioni ASP.NET MVC 3.

**Per installare i componenti e prodotti necessari**

1. Installare Visual Studio 2010. Quando viene richiesto di selezionare le funzionalità da installare, è necessario includere:

    1. Alcun linguaggio di programmazione che è necessario eseguire la compilazione.
    2. Visual Web Developer. Ciò garantisce che le destinazioni WPP vengano aggiunti al server di compilazione.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Una volta completata l'installazione di Visual Studio 2010, scaricare e installare [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (se non è già incluso nel supporto di installazione).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 risolve un bug che può impedire l'individuazione di MSDeploy eseguibile di MSBuild.
3. Scaricare e avviare il [instalace Webové Platformy](https://go.microsoft.com/?linkid=9805118).
4. In cima il **installazione guidata piattaforma Web 3.0** finestra, fare clic su **prodotti**.
5. Sul lato sinistro della finestra, nel riquadro di spostamento, fare clic su **Frameworks**.
6. Nel **Microsoft .NET Framework 4** righe, se .NET Framework non è già installato, fare clic su **Add**.

    > [!NOTE]
    > Si potrebbe avere già installato .NET Framework 4.0 tramite Windows Update. Se è già installato un prodotto o componente, l'installazione guidata piattaforma Web indicherà questo sostituendo il **Add** sul pulsante con il testo **installato**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. Nel **ASP.NET MVC 3 (Visual Studio 2010)** riga, fare clic su **Add**.
8. Nel riquadro di spostamento, fare clic su **Server**.
9. Nel **2.1 dello strumento di distribuzione Web** riga, fare clic su **Add**.
10. Fare clic su **Installa**. L'installazione guidata piattaforma Web verrà visualizzato un elenco di prodotti&#x2014;con le dipendenze associate&#x2014;per essere installata e verrà richiesto di accettare le condizioni di licenza.
11. Esaminare le condizioni di licenza e se l'utente accetti le condizioni, fare clic su **accetto**.
12. Una volta completata l'installazione, fare clic su **Finish**, quindi chiudere il **installazione guidata piattaforma Web 3.0** finestra.

> [!NOTE]
> Se il processo di distribuzione prevede l'uso degli strumenti, ad esempio VSDBCMD.exe o SQLCMD.exe, è necessario assicurarsi che questi siano installati nel server di compilazione. VSDBCMD.exe è uno strumento di Visual Studio e viene in genere aggiunto al server quando si installa Team Foundation Build. SQLCMD.exe è uno strumento di SQL Server. È possibile scaricare una versione autonoma di SQLCMD.exe dal [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) pagina.


## <a name="conclusion"></a>Conclusione

A questo punto, il server di compilazione è pronto per iniziare a compilare e distribuire i progetti di applicazione web. Argomento successivo [creazione di una compilazione che supporta la distribuzione della definizione](creating-a-build-definition-that-supports-deployment.md), viene descritto come creare e configurare una definizione di compilazione per controllare quando e come compilare e distribuire i progetti.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni generali sull'uso di Team Build, vedere [amministrazione di Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Precedente](adding-content-to-source-control.md)
> [Successivo](creating-a-build-definition-that-supports-deployment.md)
