---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Configurazione di un TFS Server di compilazione per la distribuzione Web | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come preparare un server di compilazione di Team Foundation Server (TFS) per compilare e distribuire le soluzioni con Team Build e le informazioni di Internet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 505cca303b5569b2f676adab767d742cb5cd21a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Configurazione di un Server di compilazione TFS per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come preparare un server di compilazione di Team Foundation Server (TFS) per compilare e distribuire le soluzioni con Team Build e lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web).


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [soluzione responsabile contatto](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due progetti #x 2014; & file contenente una compilare le istruzioni che si applicano a ogni ambiente di destinazione e quella contenente impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

Per preparare un server di compilazione per compilare e distribuire le soluzioni, è necessario:

- Installare e configurare il servizio di compilazione TFS.
- Installare Visual Studio 2010.
- Installare eventuali prodotti o i componenti richiesti per compilare la soluzione, come le versioni di .NET Framework o di ASP.NET MVC.
- Installare distribuzione Web 2.0 o versione successiva.

Questo argomento viene illustrato come eseguire queste procedure o puntino ad altre risorse, qualora esistano. Le attività e procedure dettagliate in questo argomento si presuppongono che:

- Si inizia con una compilazione pulita server che esegue Windows Server 2008 R2 Service Pack 1.
- Il server è aggiunto al dominio con un indirizzo IP statico.
- È installato il livello di applicazione di TFS in un server separato, come descritto in [distribuzione Web aziendale: panoramica dello Scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Che consente di eseguire queste procedure?

Nella maggior parte dei casi, un amministratore TFS sarà responsabile per la configurazione del server di compilazione. In alcuni casi, il team di sviluppo può assumere la proprietà del server di compilazione specifica.

## <a name="install-and-configure-the-tfs-build-service"></a>Installare e configurare il servizio di compilazione TFS

Quando si configura un server di compilazione, la prima attività è di installare e configurare il servizio di compilazione TFS. Come parte di questo processo, è necessario:

- Installare il servizio di compilazione TFS e configurare un account del servizio. Le attività di compilazione, inclusa la distribuzione, verranno eseguito utilizzando l'identità dell'account del servizio di compilazione.
- Creare un *controller di compilazione* e uno o più *gli agenti di compilazione*. Ogni controller di compilazione gestisce un set di agenti di compilazione. Quando si accoda una compilazione, il controller di compilazione assegna le attività di compilazione per un agente di compilazione disponibili. Ogni raccolta di progetti team in TFS è mappato a un solo controller di compilazione.
- Configurare una cartella di ricezione per l'output di compilazione. Si tratta di una condivisione di rete. Qualsiasi compilazione output, ad esempio pacchetti di distribuzione web, vengono inviati alla cartella di ricezione.

Il [amministrazione di Team Foundation Build](https://msdn.microsoft.com/en-us/library/ms252495.aspx) capitolo in MSDN contiene tutte le risorse necessarie per eseguire queste attività:

- Per una panoramica concettuale di Team Foundation Build, tra cui il servizio di compilazione, controller di compilazione e agenti di compilazione, vedere [la comprensione di Team Foundation Build System](https://msdn.microsoft.com/en-us/library/dd793166.aspx).
- Per informazioni sull'installazione e configurazione del servizio di compilazione, vedere [configurare un computer di compilazione](https://msdn.microsoft.com/en-us/library/ms181712.aspx).
- Per informazioni sulla creazione di controller di compilazione, vedere [creare e usare un Controller di compilazione](https://msdn.microsoft.com/en-us/library/ee330987.aspx).
- Per informazioni sulla creazione di agenti di compilazione, vedere [creare e usare agenti di compilazione](https://msdn.microsoft.com/en-us/library/bb399135.aspx).
- Per informazioni sulla creazione e configurazione di cartelle di ricezione, vedere [configurare cartelle di ricezione](https://msdn.microsoft.com/en-us/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Installare i componenti e i prodotti necessari

Per abilitare il server di compilazione compilare le soluzioni, è necessario installare tutti i prodotti, componenti o gli assembly che la soluzione richiede. Prima di installare i componenti della piattaforma web, è necessario installare Visual Studio 2010 (qualsiasi versione) nel server di compilazione. In questo modo si garantisce che i file di destinazione di base Microsoft Build Engine (MSBuild) e i file di destinazione Pipeline pubblicazione sul Web (WPP) sono disponibili per il servizio di compilazione. Il programma di installazione di Visual Studio è necessario installare anche distribuzione Web, è necessario se si prevede di distribuire i pacchetti web come parte del processo di compilazione.

Il modo migliore per installare i componenti della piattaforma web comune è utilizzare il [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9805118). Ciò garantisce che si sta installando la versione più recente di ogni prodotto e inoltre rileva e installa automaticamente tutti i prerequisiti per ogni prodotto. In caso del [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) soluzione, utilizzare l'installazione guidata piattaforma Web per installare questi prodotti e componenti:

- **.NET framework 4.0**. Ciò è necessario per eseguire applicazioni che sono state compilate in questa versione di .NET Framework.
- **2.1 o successivo dello strumento di distribuzione Web**. Questo modo vengono installati sul server di distribuzione Web (e il relativo file eseguibile sottostante, MSDeploy.exe). Come parte di questo processo, installa e avvia il servizio agente di distribuzione Web. Questo servizio consente di distribuire i pacchetti web da un computer remoto.
- **ASP.NET MVC 3**. Consente di installare gli assembly che necessari per eseguire applicazioni ASP.NET MVC 3.

**Per installare i componenti e prodotti necessari**

1. Installare Visual Studio 2010. Quando viene richiesto di selezionare le funzionalità da installare, è necessario includere:

    1. Qualsiasi linguaggi di programmazione che si devono compilare.
    2. Visual Web Developer. In questo modo si garantisce che le destinazioni del sito vengono aggiunti al server di compilazione.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Una volta completata l'installazione di Visual Studio 2010, scaricare e installare [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (se è non è già incluso nel supporto di installazione).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 risolve un bug che può impedire l'individuazione di MSDeploy eseguibile di MSBuild.
3. Scaricare e avviare il [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9805118).
4. Nella parte superiore del **installazione guidata piattaforma Web 3.0** finestra, fare clic su **prodotti**.
5. Sul lato sinistro della finestra, nel riquadro di spostamento, fare clic su **Framework**.
6. Nel **Microsoft .NET Framework 4** riga se .NET Framework non è già installato, fare clic su **Aggiungi**.

    > [!NOTE]
    > Si è già installato .NET Framework 4.0 in Windows Update. Se è già installato un prodotto o componente, l'installazione guidata piattaforma Web indicherà questo sostituendo il **Aggiungi** pulsante con il testo **installato**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. Nel **ASP.NET MVC 3 (Visual Studio 2010)** di riga, fare clic su **Aggiungi**.
8. Nel riquadro di spostamento, fare clic su **Server**.
9. Nel **2.1 dello strumento di distribuzione Web** di riga, fare clic su **Aggiungi**.
10. Fare clic su **Installa**. L'installazione guidata piattaforma Web verrà visualizzato un elenco di prodotti & #x 2014; insieme a tutte le dipendenze associate & #x 2014; sia installato e verrà richiesto di accettare le condizioni di licenza.
11. Esaminare le condizioni di licenza e, se accettano le condizioni, fare clic su **accetto**.
12. Una volta completato l'installazione, fare clic su **fine**, quindi chiudere il **installazione guidata piattaforma Web 3.0** finestra.

> [!NOTE]
> Se il processo di distribuzione include l'utilizzo di strumenti come VSDBCMD.exe o SQLCMD.exe, è necessario assicurarsi che questi siano installati nel server di compilazione. VSDBCMD.exe è uno strumento di Visual Studio e viene in genere aggiunto al server durante l'installazione di Team Foundation Build. SQLCMD.exe è uno strumento di SQL Server. È possibile scaricare una versione autonoma di SQLCMD.exe dal [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) pagina.


## <a name="conclusion"></a>Conclusione

A questo punto, il server di compilazione è possibile avviare la creazione e la distribuzione di progetti di applicazione web. L'argomento successivo, [la creazione di una compilazione che supporta la distribuzione della definizione](creating-a-build-definition-that-supports-deployment.md), viene descritto come creare e configurare una definizione di compilazione per controllare quando e come i progetti sono compilati e distribuiti.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni generali sull'uso di Team Build, vedere [amministrazione di Team Foundation Build](https://msdn.microsoft.com/en-us/library/ms252495.aspx).

>[!div class="step-by-step"]
[Precedente](adding-content-to-source-control.md)
[Successivo](creating-a-build-definition-that-supports-deployment.md)
