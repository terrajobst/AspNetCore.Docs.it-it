---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Creazione di un progetto Team in TFS | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come creare un nuovo progetto team in Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: ef8cddb7733c1f8cacd24c5cf341a42741d25a95
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-team-project-in-tfs"></a>Creazione di un progetto Team in TFS
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come creare un nuovo progetto team in Team Foundation Server (TFS) 2010.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [soluzione responsabile contatto](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

## <a name="task-overview"></a>Panoramica di Task

Per eseguire il provisioning e usare un nuovo progetto team in TFS, è necessario completare questi passaggi di alto livello:

- Concedere le autorizzazioni all'utente che ha creato il nuovo progetto team.
- Creare il progetto team.
- Concedere le autorizzazioni ai membri del team che utilizzeranno il progetto.
- Verificare nella parte del contenuto.

Questo argomento viene illustrato come eseguire queste procedure e verrà identificato gli utenti e ruoli che possono essere responsabile di ogni procedura. Tenere presente che, a seconda della struttura dell'organizzazione, ognuna di queste attività può essere la responsabilità di un'altra persona.

Le attività e procedure dettagliate in questo argomento si presuppongono di aver installato e configurato TFS e di aver creato una raccolta di progetti team come parte del processo di configurazione. Per ulteriori informazioni su questi presupposti e per le informazioni più generali sullo scenario, vedere [configurare un Server di compilazione TFS per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Concedere le autorizzazioni per l'autore del progetto Team

Per creare un nuovo progetto team, è necessario queste autorizzazioni:

- È necessario disporre di **creare nuovi progetti** autorizzazione nel livello applicazione TFS. L'autorizzazione viene concessa in genere l'aggiunta di utenti per il **Project Collection Administrators** gruppo TFS. Il **Team Foundation Administrators** gruppo globale include anche l'autorizzazione.
- È necessario disporre dell'autorizzazione per creare nuovi siti del team della raccolta siti di SharePoint corrispondente alla raccolta di progetti team TFS. L'autorizzazione viene concessa in genere aggiungendo l'utente a un gruppo di SharePoint con **controllo completo** raccolta siti di diritti in SharePoint.
- Se si utilizza la funzionalità di SQL Server Reporting Services, è necessario essere un membro del **Gestione contenuto Team Foundation** ruolo in Reporting Services.

### <a name="who-performs-these-procedures"></a>Che consente di eseguire queste procedure?

In genere, la persona o il gruppo che amministra la distribuzione di TFS esegue queste procedure.

Poiché si tratta di un set di autorizzazioni con privilegi elevati, i nuovi progetti team vengono solitamente creati da un piccolo sottoinsieme di utenti con responsabilità per l'amministrazione di una distribuzione di TFS. Gli sviluppatori verranno non in genere essere concesse le autorizzazioni necessarie per creare nuovi progetti team.

### <a name="grant-permissions-in-tfs"></a>Concedere le autorizzazioni in TFS

Se si desidera consentire agli utenti di creare nuovi progetti team, la prima attività di alto livello consiste nell'aggiungere l'utente per il **Project Collection Administrators** gruppo per la raccolta di progetti team.

**Per aggiungere un utente al gruppo Project Collection Administrators**

1. Nel server TFS, nel **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft Team Foundation Server 2010**, quindi fare clic su **Team Foundation Console di amministrazione**.
2. Nella visualizzazione albero di navigazione, espandere **livello applicazione**, quindi fare clic su **raccolte di progetti Team**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. Nel **raccolte di progetti Team** riquadro, selezionare il progetto team di raccolta che si desidera gestire.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Nel **generale** scheda, fare clic su **l'appartenenza al gruppo**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. Nel **gruppi globali** la finestra di dialogo, seleziona il **Project Collection Administrators** gruppo e quindi fare clic su **proprietà**.
6. Nel **proprietà gruppo Team Foundation Server** nella finestra di dialogo **utente o gruppo Windows**, quindi fare clic su **Aggiungi**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. Nel **Seleziona utenti, computer o gruppi** la finestra di dialogo digitare il nome utente dell'utente che si desidera essere in grado di creare nuovi progetti team, fare clic su **Controlla nomi**, quindi fare clic su **OK** .

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. Nel **proprietà gruppo Team Foundation Server** la finestra di dialogo, fare clic su **OK**.
9. Nel **gruppi globali** la finestra di dialogo, fare clic su **Chiudi**.

### <a name="grant-permissions-in-sharepoint-services"></a>Concedere le autorizzazioni in SharePoint Services

Successivamente, è necessario concedere all'utente l'autorizzazione per creare nuovi siti del team della raccolta siti di SharePoint corrispondente alla raccolta di progetti team TFS.

**Per concedere autorizzazioni controllo completo per la raccolta siti di SharePoint**

1. Nella Console di amministrazione Team Foundation Server sul **raccolte di progetti Team** pagina, selezionare la raccolta di progetti team che si desidera gestire.
2. Nel **sito di SharePoint** scheda, prendere nota del valore del **percorso corrente sito predefinito** URL.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Aprire Internet Explorer e quindi passare all'URL annotato nel passaggio 2.

    > [!NOTE]
    > Se non accede a Windows dell'utente che ha creato la raccolta di progetti team, è necessario accedere a SharePoint con questo account utente per continuare.
4. Nel **Azioni sito** menu, fare clic su **Impostazioni sito**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Nel **Impostazioni sito** pagina **utenti e autorizzazioni**, fare clic su **utenti e gruppi**.
6. Nel Pannello di navigazione a sinistra, fare clic su **gruppi**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Nel **utenti e gruppi: tutti i gruppi di** pagina, fare clic su **Imposta gruppi per questo sito**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

    > [!NOTE]
    > È possibile che venga visualizzato un **HTTP 404 non trovato** errore dovuto a un bug di codifica HTTP doppio. In questo caso, è possibile sostituire l'URL con questo:   
    > [*URL della raccolta siti*] /\_layouts/permsetup.aspx  
    > Ad esempio:  
    > http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx
8. Nel **Imposta gruppi per questo sito** pagina, aggiungere l'utente che ha creato i progetti team di **proprietari** gruppo e quindi fare clic su **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Per ulteriori informazioni sull'abilitazione agli utenti di creare nuovi progetti team all'interno di una raccolta di progetti team, vedere [impostare autorizzazioni di amministratore per raccolte di progetti Team](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Creare un nuovo progetto Team e aggiungere utenti

Quando si dispone delle autorizzazioni necessarie, è possibile utilizzare il **Team Explorer** finestra in Visual Studio 2010 per creare un nuovo progetto team. Questo approccio offre una procedura guidata che consente di raccogliere tutte le informazioni necessarie ed esegue le attività necessarie in TFS, SharePoint e SQL Server Reporting Services. È inoltre necessario concedere autorizzazioni per il nuovo progetto team per i membri del team di sviluppatori, per consentire loro di aggiungere e modificare il contenuto.

### <a name="who-performs-these-procedures"></a>Che consente di eseguire queste procedure?

In genere un amministratore TFS o un responsabile del team di sviluppo consente di eseguire queste procedure.

### <a name="create-a-new-team-project"></a>Creare un nuovo progetto Team

La procedura seguente viene descritto come creare un nuovo progetto team in TFS 2010.

**Per creare un nuovo progetto team**

1. Nel **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft Visual Studio 2010**, fare doppio clic su **Microsoft Visual Studio 2010**, e quindi fare clic su **Esegui come amministratore**.

    > [!NOTE]
    > Se non viene eseguito Visual Studio 2010 come amministratore, la creazione guidata nuovo progetto Team avrà esito negativo dell'ultimo passaggio.
2. Se il **controllo dell'Account utente** viene visualizzata la finestra di dialogo, fare clic su **Sì**.
3. In Visual Studio, sul **Team** menu, fare clic su **Connetti a Team Foundation Server**.

    > [!NOTE]
    > Se già stata configurata una connessione a un server TFS, è possibile omettere i passaggi da 4 a 7.
4. Nel **connessione al progetto Team** la finestra di dialogo, fare clic su **server**.
5. Nel **Aggiungi/Rimuovi Team Foundation Server** la finestra di dialogo, fare clic su **Aggiungi**.
6. Nel **Aggiungi Team Foundation Server** la finestra di dialogo, fornire i dettagli dell'istanza di TFS, quindi fare clic su **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. Nel **Aggiungi/Rimuovi Team Foundation Server** la finestra di dialogo, fare clic su **Chiudi**.
8. Nel **Connetti a progetto Team** la finestra di dialogo, seleziona l'istanza TFS si desidera connettersi, per selezionare il team di progetto raccolta che si desidera aggiungere e quindi fare clic su **Connetti**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. Nel **Team Explorer** finestra, rapida, raccolta di progetti team e quindi fare clic su **nuovo progetto Team**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. Nel **nuovo progetto Team** la finestra di dialogo, specificare un nome e una descrizione per il progetto team, quindi fare clic su **Avanti**.

    > [!NOTE]
    > Se il progetto team include spazi, potrebbero verificarsi alcuni problemi quando si accede a utilizzare lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) per distribuire i pacchetti dal percorso di output. Spazi nel percorso di possono rendere molto più difficile eseguire i comandi di distribuzione Web.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Nel **selezionare un modello di processo** pagina, selezionare il modello di processo che si desidera utilizzare per gestire il processo di sviluppo e quindi fare clic su **Avanti**.

    > [!NOTE]
    > Per ulteriori informazioni sui modelli di processo per TFS, vedere [strumenti e modelli di processo](https://msdn.microsoft.com/vstudio/aa718795).
12. Nel **le impostazioni del sito del Team** pagina, lasciare le impostazioni predefinite invariate e quindi fare clic su **Avanti**.
13. Questa impostazione crea o identifica, un sito del team di SharePoint che viene associato al progetto team TFS. Il team di sviluppo può utilizzare questo sito per gestire la documentazione, fanno parte di thread di discussione, creare pagine wiki e varie altre attività non correlate al codice. Per ulteriori informazioni, vedere [le interazioni tra i prodotti SharePoint e Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Nel **specifica le impostazioni del controllo origine** pagina, lasciare le impostazioni predefinite invariate e quindi fare clic su **Avanti**.
15. Questa impostazione identifica o crea il percorso nella gerarchia di cartelle TFS che fungerà da una cartella radice per il contenuto.
16. Nel **Conferma impostazioni progetto Team** pagina, fare clic su **fine**.
17. Quando il nuovo progetto team è stato creato, nel **progetto Team creato** pagina, fare clic su **Chiudi**.

### <a name="add-users-to-a-team-project"></a>Aggiungere utenti a un progetto Team

Dopo aver creato il nuovo progetto team, è possibile concedere autorizzazioni agli utenti per poter avviare l'aggiunta e la collaborazione sul contenuto.

**Per aggiungere utenti a un progetto team**

1. In Visual Studio 2010, nel **Team Explorer** finestra, fare clic sul progetto team, scegliere **impostazioni progetto Team**, quindi fare clic su **l'appartenenza al gruppo**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Per consentire agli utenti di aggiungere, modificare e rimuovere codice sottoposto a controllo del codice sorgente, è necessario aggiungere il contatto per il **collaboratori** gruppo.
3. Nel **gruppi progetto** la finestra di dialogo, seleziona il **collaboratori** gruppo e quindi fare clic su **proprietà**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. Nel **proprietà gruppo Team Foundation Server** nella finestra di dialogo **utente o gruppo Windows**, quindi fare clic su **Aggiungi**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. Nel **Seleziona utenti, computer o gruppi** la finestra di dialogo digitare il nome utente dell'utente a cui si desidera aggiungere al progetto team, fare clic su **Controlla nomi**, quindi fare clic su **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. Nel **proprietà gruppo Team Foundation Server** la finestra di dialogo, fare clic su **OK**.
7. Nel **gruppi progetto** la finestra di dialogo, fare clic su **Chiudi**.

## <a name="conclusion"></a>Conclusione

A questo punto, è possibile utilizzare il nuovo progetto team e il team di sviluppo è possibile avviare l'aggiunta di contenuto e la collaborazione nel processo di sviluppo.

L'argomento successivo, [contenuto aggiunta al controllo del codice sorgente](adding-content-to-source-control.md), viene descritto come aggiungere contenuto al controllo del codice sorgente.

## <a name="further-reading"></a>Ulteriori informazioni

Per una più ampia informazioni aggiuntive sulla creazione di progetti team in TFS, vedere [creare un progetto Team](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Per ulteriori informazioni sull'abilitazione agli utenti di creare nuovi progetti team all'interno di una raccolta di progetti team, vedere [impostare autorizzazioni di amministratore per raccolte di progetti Team](https://msdn.microsoft.com/library/dd547204.aspx). Per ulteriori informazioni sull'aggiunta di utenti ai progetti team, vedere [aggiungere utenti ai progetti Team](https://msdn.microsoft.com/library/bb558971.aspx).

>[!div class="step-by-step"]
[Precedente](configuring-team-foundation-server-for-web-deployment.md)
[Successivo](adding-content-to-source-control.md)
