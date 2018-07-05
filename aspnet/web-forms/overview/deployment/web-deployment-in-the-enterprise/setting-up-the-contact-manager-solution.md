---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Impostazione della soluzione Contact Manager | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come scaricare e configurare la soluzione Contact Manager venga eseguita in locale in una workstation di sviluppo.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 479dbb8d2edbe9fb953ea9e1312ffb8fdbd3e2fe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802131"
---
<a name="setting-up-the-contact-manager-solution"></a>Impostazione della soluzione Contact Manager
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come scaricare e configurare la soluzione Contact Manager venga eseguita in locale in una workstation di sviluppo.


## <a name="system-requirements"></a>Requisiti di sistema

Per eseguire la soluzione Contact Manager in locale ed eseguire altre attività descritte in questa esercitazione, è necessario installarvi il software della workstation dello sviluppatore:

- Visual Studio 2010 Service Pack 1, Premium o Ultimate Edition
- Internet Information Services (IIS) 7.5 Express
- SQL Server Express 2008 R2
- IIS strumento di distribuzione Web (distribuzione Web) 2.1 o versioni successive
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

Fatta eccezione per Visual Studio 2010, è possibile scaricare e installare le versioni più recenti di questi prodotti e i componenti tramite il [instalace Webové Platformy](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Scaricare ed estrarre la soluzione

È possibile scaricare l'applicazione di esempio Contact Manager da MSDN Code Gallery [qui](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Configurare ed eseguire la soluzione

Per configurare ed eseguire la soluzione Contact Manager nel computer locale, è necessario eseguire i passaggi generali:

1. Se non è già, creare un database di servizi dell'applicazione ASP.NET locale con le funzionalità di gestione di appartenenza e ruoli abilitate.
2. Modificare le stringhe di connessione nel *Web. config* in modo che puntino all'istanza locale di SQL Server Express.
3. Eseguire la soluzione da Visual Studio 2010.

Il resto di questa sezione fornisce altre indicazioni su come completare queste attività.

**Per creare il database di servizi di applicazione**

1. Aprire il prompt dei comandi di Visual Studio 2010. A tale scopo, scegliere il **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft Visual Studio 2010**, fare clic su **Visual Studio Tools**e quindi Fare clic su **prompt dei comandi di Visual Studio (2010)**.
2. Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Usare la **– C** per specificare la stringa di connessione per il server di database.
    2. Usare la **– A** per specificare funzionalità da aggiungere al database dei servizi dell'applicazione. In questo caso **m** indica che si desidera aggiungere il supporto per il provider di appartenenze e **r** indica che si desidera aggiungere il supporto per la gestione dei ruoli.
    3. Usare la **– d** per specificare un nome per il database di servizi di applicazione. Se si omette questa opzione è attivata, l'utilità creerà un database con il nome predefinito della **aspnetdb**.
3. Quando il database è stato creato correttamente, il prompt dei comandi visualizzerà un messaggio di conferma.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Per altre informazioni su aspnet\_regsql utilità, vedere [strumento di registrazione di SQL Server ASP.NET (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


Il passaggio successivo è assicurarsi che le stringhe di connessione della soluzione Contact Manager puntano all'istanza locale di SQL Server Express.

**Per aggiornare le stringhe di connessione**

1. Aprire la soluzione Contact Manager in Visual Studio 2010.
2. Nel **Esplora soluzioni** finestra, espandere il **ContactManager.Mvc** del progetto e quindi fare doppio clic il **Web. config** nodo.

    > [!NOTE]
    > Il progetto ContactManager.Mvc include due *Web. config* file. È necessario modificare il file a livello di progetto.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. Nel **connectionStrings** elemento, verificare che la stringa di connessione denominata **ApplicationServices** punti al database servizi dell'applicazione ASP.NET locale.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. Nel **Esplora soluzioni** finestra, espandere il **ContactManager.Service** del progetto e quindi fare doppio clic il **Web. config** nodo.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. Nel **connectionStrings** elemento, nella stringa di connessione denominata **ContactManagerContext**, verificare che il **Zdroj dat** viene impostata per l'istanza locale di SQL Server Express. Non devi si modifica nient'altro nella stringa di connessione.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Salvare tutti i file aperti.

A questo punto dovrebbe essere pronto per l'esecuzione della soluzione Contact Manager nel computer locale.

> [!NOTE]
> Se si seguono questi passaggi senza aver prima creato un database di servizi dell'applicazione, ASP.NET verrà creato il database la prima volta che si tenta di creare un utente. Tuttavia, creazione manuale del database offre molto più controllo all'interno del set di funzionalità di servizi dell'applicazione che si desidera supportare.


**Per eseguire la soluzione Contact Manager**

1. In Visual Studio 2010, premere F5.
2. Internet Explorer viene avviato e richiede l'URL dell'applicazione Contact Manager ASP.NET MVC 3. Per impostazione predefinita, l'applicazione viene visualizzata la **tutti i contatti** pagina.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Aggiungere alcuni contatti e quindi verificare che l'applicazione funzioni come previsto.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Passare a `http://localhost:50114/Account/Register` (se si ospita l'applicazione su una porta diversa, modificare l'URL). Aggiungere un nome utente, indirizzo di posta elettronica e password e verificare che tu sia in grado di registrare correttamente un account.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Passare a `http://localhost:50114/Account/LogOn` (se si ospita l'applicazione su una porta diversa, modificare l'URL). Verificare che tu sia in grado di accedere usando l'account che appena creato.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Chiudere Internet Explorer per arrestare il debug.

## <a name="conclusion"></a>Conclusione

A questo punto, la soluzione Contact Manager deve essere completamente configurata per l'esecuzione nel computer locale. Quando si utilizzano gli altri argomenti in questa esercitazione, è possibile usare la soluzione come riferimento.

Argomento successivo [informazioni sul File di progetto](understanding-the-project-file.md), illustra come è possibile usare i file di progetto personalizzati di Microsoft Build Engine (MSBuild) all'interno della soluzione Contact Manager per controllare il processo di distribuzione.

> [!div class="step-by-step"]
> [Precedente](the-contact-manager-solution.md)
> [Successivo](understanding-the-project-file.md)
