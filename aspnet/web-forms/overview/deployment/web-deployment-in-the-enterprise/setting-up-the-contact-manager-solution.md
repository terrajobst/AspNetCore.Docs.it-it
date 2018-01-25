---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Impostazione della soluzione di gestione di contatto | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come scaricare e configurare la soluzione di gestione contatto per l'esecuzione in locale in una workstation di sviluppo.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: b8176b3b8622e21187a91647323322e55582373c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="setting-up-the-contact-manager-solution"></a>Impostazione della soluzione di gestione di contatto
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come scaricare e configurare la soluzione di gestione contatto per l'esecuzione in locale in una workstation di sviluppo.


## <a name="system-requirements"></a>Requisiti di sistema

Per eseguire la soluzione di gestione di contatto in locale e per eseguire altre attività descritte in questa esercitazione, è necessario installare il software nella workstation di sviluppo:

- Visual Studio 2010 Service Pack 1, Premium o Ultimate Edition
- Internet Information Services (IIS) 7.5 Express
- SQL Server Express 2008 R2
- IIS strumento di distribuzione Web (distribuzione Web) 2.1 o versioni successive
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

Fatta eccezione per Visual Studio 2010, è possibile scaricare e installare le versioni più recenti di tutti questi prodotti e componenti tramite il [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Scaricare ed estrarre la soluzione

È possibile scaricare l'applicazione di esempio di gestione di contatto da MSDN Code Gallery [qui](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Configurare ed eseguire la soluzione

Per configurare ed eseguire la soluzione di gestione di contatto nel computer locale, è necessario eseguire questi passaggi di alto livello:

1. Se non hai uno già, creare un database di servizi dell'applicazione ASP.NET locale con le funzionalità di gestione di ruoli e appartenenze abilitate.
2. Modificare le stringhe di connessione nel *Web. config* file in modo che punti all'istanza locale di SQL Server Express.
3. Eseguire la soluzione da Visual Studio 2010.

Nella parte restante di questa sezione fornisce informazioni aggiuntive sulla modalità di completamento di ognuna di queste attività.

**Per creare il database di servizi di applicazione**

1. Aprire il prompt dei comandi di Visual Studio 2010. A tale scopo, scegliere il **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft Visual Studio 2010**, fare clic su **Visual Studio Tools**e quindi Fare clic su **prompt dei comandi di Visual Studio (2010)**.
2. Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Utilizzare il **-C** per specificare la stringa di connessione per il server di database.
    2. Utilizzare il **:** per specificare l'applicazione Servizi funzionalità che si desidera aggiungere al database. In questo caso, **m** indica che si desidera aggiungere il supporto per il provider di appartenenze e **r** indica che si desidera aggiungere il supporto per la gestione dei ruoli.
    3. Utilizzare il **– d** per specificare un nome per il database di servizi dell'applicazione. Se si omette questa opzione è attivata, l'utilità verrà creato un database con il nome predefinito di **aspnetdb**.
3. Quando il database è stato creato correttamente, il prompt dei comandi verrà visualizzato un messaggio di conferma.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Per ulteriori informazioni su aspnet\_regsql utilità, vedere [strumento di registrazione di SQL Server ASP.NET (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


Il passaggio successivo è per assicurarsi che le stringhe di connessione della soluzione di gestione di contatto del punto per l'istanza locale di SQL Server Express.

**Per aggiornare le stringhe di connessione**

1. Aprire la soluzione di gestione di contatto in Visual Studio 2010.
2. Nel **Esplora** finestra, espandere il **ContactManager.Mvc** dei progetti e quindi fare doppio clic sul **Web. config** nodo.

    > [!NOTE]
    > Il progetto ContactManager.Mvc include due *Web. config* file. È necessario modificare il file a livello di progetto.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. Nel **connectionStrings** elemento, verificare che la stringa di connessione denominato **ApplicationServices** punta al database di servizi dell'applicazione ASP.NET locale.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. Nel **Esplora** finestra, espandere il **ContactManager.Service** dei progetti e quindi fare doppio clic sul **Web. config** nodo.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. Nel **connectionStrings** elemento, nella stringa di connessione denominata **ContactManagerContext**, verificare che il **origine dati** proprietà è impostata per l'istanza locale di SQL Server Express. Non è necessario modificare qualsiasi altro elemento nella stringa di connessione.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Salvare tutti i file aperti.

È ora pronto per eseguire la soluzione di gestione di contatto nel computer locale.

> [!NOTE]
> Se questa procedura senza prima creare un database di servizi dell'applicazione, ASP.NET verrà creato il database la prima volta che si tenta di creare un utente. Tuttavia, creare manualmente il database offre molte più controllo sul set di funzionalità di servizi di applicazione che si desidera supportare.


**Per eseguire la soluzione di gestione di contatto**

1. In Visual Studio 2010, premere F5.
2. Internet Explorer viene avviato e richiede l'URL dell'applicazione Contact Manager ASP.NET MVC 3. Per impostazione predefinita, l'applicazione visualizza la **tutti i contatti** pagina.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Aggiungere alcuni contatti e quindi verificare che l'applicazione funzioni come previsto.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Passare a `http://localhost:50114/Account/Register` (se si ospita l'applicazione su una porta diversa, modificare l'URL). Aggiungere un nome utente, un indirizzo di posta elettronica e una password e verificare che tu sia in grado di registrare correttamente un account.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Passare a `http://localhost:50114/Account/LogOn` (se si ospita l'applicazione su una porta diversa, modificare l'URL). Verificare che si è in grado di accedere utilizzando l'account che appena creato.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Chiudere Internet Explorer per arrestare il debug.

## <a name="conclusion"></a>Conclusione

A questo punto, la soluzione di gestione di contatto deve essere completamente configurata per l'esecuzione nel computer locale. Quando si lavora con gli altri argomenti in questa esercitazione, è possibile utilizzare la soluzione come riferimento.

L'argomento successivo, [informazioni sui File di progetto](understanding-the-project-file.md), viene illustrato come è possibile utilizzare i file di progetto personalizzati di Microsoft Build Engine (MSBuild) all'interno della soluzione di gestione di contatto per controllare il processo di distribuzione.

>[!div class="step-by-step"]
[Precedente](the-contact-manager-solution.md)
[Successivo](understanding-the-project-file.md)
