---
title: Distribuzione continua in Azure con Visual Studio e Git
author: rick-anderson
description: Informazioni su come creare un'app web ASP.NET Core tramite Visual Studio e distribuirla nel Servizio app di Azure, usando Git per la distribuzione continua.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: a9efad38b1c75bd3a186b4ec85861357ecf744b9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Distribuzione continua in Azure per ASP.NET Core con Visual Studio e Git

Di [Erik Reitan](https://github.com/Erikre)

Questa esercitazione mostra come creare un'app web ASP.NET Core tramite Visual Studio e distribuirla da Visual Studio nel Servizio app di Azure, usando Git la distribuzione continua. 

Vedere anche [Usare VSTS per Compilare e Pubblicare un'App Web di Azure con una distribuzione continua](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), che illustra come configurare un flusso di lavoro di recapito continuo per [Servizio App di Azure](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) con Visual Studio Team Services. Il recapito continuo di Azure in Team Services semplifica la configurazione di una pipeline di distribuzione solida per pubblicare gli aggiornamenti per l'app cel servizio App di Azure. È possibile configurare la pipeline dal portale di Azure per compilare, eseguire i test, distribuire in uno slot di staging e quindi distribuire nell'ambiente di produzione.

> [!NOTE]
> Per completare questa esercitazione, è necessario un account di Microsoft Azure. Se non si dispone di un account, è possibile [attivare i benefici per il sottoscrittore MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) o [iscriversi per una versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Prerequisiti

Questa esercitazione presuppone che è già stato installato quanto segue:

* [Visual Studio](https://www.visualstudio.com)

* [ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (runtime e strumenti)

* [Git](https://git-scm.com/downloads) per Windows

## <a name="create-an-aspnet-core-web-app"></a>Creare un'app Web ASP.NET Core

1. Avviare Visual Studio.

2. Scegliere **Nuovo** > **Progetto** dal menu **File**.

3. Selezionare il modello di progetto **Applicazione Web ASP.NET**. Viene visualizzata in **Modelli** > **Installati** > **Visual C#** > **Web**. Denominare il progetto `SampleWebAppDemo`. Selezionare l'opzione **Creare un nuovo repository Git** e fare clic su **OK**.

   ![Finestra di dialogo Nuovo progetto](azure-continuous-deployment/_static/01-new-project.png)

4. Nella finestra di dialogo **Nuovo progetto ASP.NET** selezionare il modello ASP.NET Core **Vuoto**, quindi fare clic su **OK**.

   ![Nuova finestra di dialogo Progetto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a>Esecuzione dell'applicazione Web in locale

1. Una volta terminata la creazione dell'app da parte di Visual Studio, eseguire l'app selezionando **Debug** -> **Avvia l'esecuzione del debug**. In alternativa è possibile premere **F5**.

   L'inizializzazione di Visual Studio e della nuova app potrebbe richiedere tempo. Una volta completata, il browser visualizzerà l'app in esecuzione.

   ![La finestra del browser mostra l'applicazione in esecuzione che visualizza "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

2. Dopo aver revisionato l'app Web in esecuzione, chiudere il browser e fare clic sull'icona "Interrompi l'esecuzione del debug" nella barra degli strumenti di Visual Studio per arrestare l'app.

## <a name="create-a-web-app-in-the-azure-portal"></a>Creare un'app web nel portale di Azure

La procedura seguente guiderà nella creazione di un'app web nel portale di Azure.

1. Registrarsi al [Portale di Azure](https://portal.azure.com)
2. TOOCARE **NUOVO** nella parte superiore sinistra del Portale
3. TOCCARE **Web + Dispositivi mobili** > **App Web**

    ![Portale di Microsoft Azure: pulsante Nuovo: Web + Dispositivi mobili in Marketplace: pulsante Web App in App in primo piano](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  Nel pannello **App Web** immettere un valore univoco per il **Nome del servizio App**.

    ![Pannello App Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    >Il nome **Nome del servizio App** deve essere univoco. Quando si tenta di immettere il nome, il portale applicherà questa regola. Dopo avere immesso un valore diverso, è necessario sostituire tale valore per ogni occorrenza di **SampleWebAppDemo** visualizzata in questa esercitazione.

    &nbsp;
    
    Anche nel pannello **App Web**, selezionare una **Posizione/piano di servizio App** o crearne uno nuovo. Se si crea un nuovo piano, selezionare il piano tariffario, la posizione e altre opzioni. Per altre informazioni sui piani del servizio App, [Panoramica di approfondimento dei piani del Servizio App di Azure](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).

5.  Scegliere **Crea**. Azure eseguirà il provisioning e avvierà l'app web.

    ![Portale di Azure: campione di pannello di Essentials Demo 01 di App Web](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Abilitare la pubblicazione di Git per la nuova app web

GIT è un sistema di controllo della versione distribuita che è possibile usare per distribuire l'app web del servizio App di Azure. Si archivierà il codice scritto per l'app web in un repository Git locale e il codice verrà distribuito in Azure eseguendo il push in un repository remoto.

1. Accedere al [portale di Azure](https://portal.azure.com), se non si è già connessi.

2. Fare clic su **Sfoglia**, posizionato nella parte inferiore del riquadro di spostamento.

3. Fare clic su **App Web** per visualizzare un elenco delle App web associate alla sottoscrizione di Azure.

4. Selezionare l'app web che è stata creata nella sezione precedente di questa esercitazione.

5. Se il pannello **Impostazioni** non viene visualizzato, selezionare **Impostazioni** nel pannello **App Web**.

6. Nel pannello **Impostazioni** selezionare **Origine distribuzione** > **Scegli origine** > **Repository Git locale**.

   ![Pannello delle impostazioni: pannello di origine della distribuzione: scegliere Pannello di origine](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. Fare clic su **OK**.

8. Se le credenziali di distribuzione per la pubblicazione di un'app web o di altri servizi app web non sono state precedentemente configurate, impostarle ora:

   * Fare clic su **impostazioni** > **Credenziali di distribuzione**. Il pannello **Impostare le credenziali di distribuzione** verrà visualizzato.

   * Creare un nome utente e una password.  Questa password sarà necessaria successivamente quando si configurerà Git.

   * Fare clic su **Salva**.

9. Nel pannello **App Web** fare clic su **Impostazioni** > **Proprietà**. L'URL del repository Git remoto che verrà distribuita è visualizzata in **URL GIT**.

10. Copiare il valore **URL GIT** per un uso successivo nell'esercitazione.

   ![Portale di Azure: pannello Proprietà dell'applicazione](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>Pubblicare l'app Web nel servizio app di Azure

In questa sezione si creerà un repository Git locale, tramite Visual Studio, e si eseguirà il push da quel repository in Azure per distribuire l'app web. La procedura coinvolta include quanto segue:

   * Aggiungere l'impostazione del repository remoto tramite il valore di URL GIT, in modo da poter distribuire il repository locale in Azure.

   * Eseguire il commit delle modifiche del progetto.

   * Effettuare il push delle modifiche di progetto dal repository locale al repository remoto in Azure.

&nbsp;
   
1.  In **Esplora soluzioni** fare clic con il tasto destro del mouse su **Soluzione "SampleWebAppDemo"** e selezionare **Commit**. Il **Team Explorer** verrà visualizzato.

    ![Scheda di connessione Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

2.  In **Team Explorer** selezionare la **Home** (icona home) > **Impostazioni** > **Impostazioni del Repository**.

3.  Nella sezione **Remoti** delle **Impostazioni del Repository** selezionare **Aggiungi**. La finestra di dialogo **Aggiungi remoto** verrà visualizzata.

4.  Impostare il **Nome** del repository remoto da **Azure SampleApp**.

5.  Impostare il valore per **Recupero** per l'**URL Git** precedentemente copiato da Azure in questa esercitazione. Si noti che questo è l'URL che termina con **.git**.

    ![Modifica finestra di dialogo remota](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    >In alternativa è possibile specificare il repository remoto dalla **Finestra di comando** aprendo la **Finestra di comando**, modificando la directory del progetto e immettendo il comando. Ad esempio:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

6.  Selezionare la **Home** (icona home) > **Impostazioni** > **Impostazioni globali**. Accertarsi di disporre del nome e dell'indirizzo di posta elettronica impostato. È anche necessario selezionare **Aggiornamento**.

7.  Selezionare **Home** > **Modifiche** da restituire alla vista **Modifiche**.

8.  Immettere un messaggio di commit, ad esempio **Push iniziale #1** e fare clic su **Commit**. Questa azione consente di creare un *Commit* a livello locale. Successivamente, è necessaria la *sincronizzazione* con Azure.

    ![Scheda di connessione Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    >In alternativa è possibile eseguire il commit delle modifiche dalla **Finestra di comando** aprendo la **Finestra di comando**, modificando la directory del progetto e immettendo i comandi git. Ad esempio:
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  Selezionare **Home** > **Sincronizzazione** > **Azioni** > **Aprire il prompt dei comandi**. Il prompt dei comandi aprirà la directory del progetto.

10.  Immettere il comando seguente nella finestra Comando:

    `git push -u Azure-SampleApp master`

11.  Immettere la password delle **le credenziali di distribuzione** di Azure creata precedentemente in Azure.

    >[!NOTE]
    >La password non sarà visibile mentre viene immessa.

    Questo comando avvierà il processo di esecuzione del push dei file di progetto locali in Azure. L'output del comando precedente termina con un messaggio che indica che la distribuzione ha avuto esito positivo.
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > Se è necessario collaborare ad un progetto, è necessario considerare di eseguire il push su [GitHub](https://github.com) tra l'inserimento in Azure.
 
### <a name="verify-the-active-deployment"></a>Verificare la distribuzione attiva

È possibile verificare di aver trasferito con esito positivo l'app web dall'ambiente locale in Azure. Verrà visualizzata la corretta distribuzione elencata.

1. Nel [portale Azure](https://portal.azure.com) selezionare l'app web. Selezionare quindi **Impostazioni** > **Distribuzione continua**.

   ![Portale di Azure: pannello impostazioni: pannello di distribuzione che mostra la distribuzione con esito positivo](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Eseguire l'app in Azure

Ora che è stato distribuita l'app web in Azure, è possibile eseguire l'app.

Questa operazione può essere eseguita nei due modi seguenti.

* Nel portale di Azure posizionare il pannello dell'app web per l'app web e fare clic su **Sfoglia** per visualizzare l'app nel browser predefinito.

* Aprire un browser e immettere l'URL per l'app web. Ad esempio:

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a>Aggiornare l'app web e pubblicare di nuovo

Dopo aver apportato delle modifiche al codice locale, è possibile pubblicare di nuovo.

1.  In **Esplora soluzioni** di Visual Studio aprire il file *Startup.cs*.

2.  Nel metodo `Configure` modificare il metodo `Response.WriteAsync` in modo che venga visualizzato nel seguente modo:

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  Salvare le modifiche a *Startup.cs*.

4.  In **Esplora soluzioni** fare clic con il tasto destro del mouse su **Soluzione "SampleWebAppDemo"** e selezionare **Commit**. Il **Team Explorer** verrà visualizzato.

5.  Immettere un messaggio per il commit, ad esempio:

    ```none
    Update #2
    ```

6.  Premere il pulsante **Commit** per salvare le modifiche del progetto.

7.  Selezionare **Home** > **Sincronizzazione** > **Azioni** > **Push**.

>[!NOTE]
>In alternativa è possibile eseguire il push delle modifiche dalla **Finestra di comando** aprendo la **Finestra di comando**, modificando la directory del progetto e immettendo un comando git. Ad esempio:
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Visualizzare l'app web aggiornata in Azure

Visualizzare l'app web aggiornata selezionando **Sfoglia** dal pannello dell'app web nel Portale di Azure o aprendo un browser e immettendo l'URL per l'app web. Ad esempio:

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Risorse aggiuntive

* [Pubblicazione e Distribuzione](index.md)

* [Kudu del progetto](https://github.com/projectkudu/kudu/wiki)
