---
title: Distribuzione continua in Azure con Visual Studio e Git
author: rick-anderson
description: Informazioni su come creare un'app Web ASP.NET Core tramite Visual Studio e distribuirla nel Servizio app di Azure usando Git per la distribuzione continua.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: c1d25b109bbf211eb476860ff77b649565960b62
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2018
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Distribuzione continua in Azure per ASP.NET Core con Visual Studio e Git

Di [Erik Reitan](https://github.com/Erikre)

In questa esercitazione viene illustrato come creare un'app web ASP.NET Core con Visual Studio e distribuirlo da Visual Studio al servizio App di Azure utilizzando la distribuzione continua.

Vedere anche [Usare VSTS per Compilare e Pubblicare un'App Web di Azure con una distribuzione continua](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), che illustra come configurare un flusso di lavoro di recapito continuo per [Servizio App di Azure](/azure/app-service/app-service-web-overview) con Visual Studio Team Services. Il recapito continuo Azure Team Services semplifica la configurazione di una pipeline di distribuzione affidabile per pubblicare gli aggiornamenti per le app ospitate in Azure App Service. È possibile configurare la pipeline dal portale di Azure per compilare, eseguire i test, distribuire uno slot di gestione temporanea e quindi distribuire nell'ambiente di produzione.

> [!NOTE]
> Per completare questa esercitazione, è necessario un account di Microsoft Azure. Per ottenere un account, [attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) o [iscriversi per una valutazione gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Prerequisiti

Questa esercitazione si presuppone che viene installato il software seguente:

* [Visual Studio](https://www.visualstudio.com)
* [.NET core SDK](https://www.microsoft.com/net/download/core) (runtime e gli strumenti)
* [Git](https://git-scm.com/downloads) per Windows

## <a name="create-an-aspnet-core-web-app"></a>Creare un'app Web ASP.NET Core

1. Avviare Visual Studio.

1. Scegliere **Nuovo** > **Progetto** dal menu **File**.

1. Selezionare il modello di progetto **Applicazione Web ASP.NET Core**. Viene visualizzato in **Modelli** > **installati** > **Visual C#** > **.NET Core**. Denominare il progetto `SampleWebAppDemo`. Selezionare l'opzione **Creare un nuovo repository Git** e fare clic su **OK**.

   ![Finestra di dialogo Nuovo progetto](azure-continuous-deployment/_static/01-new-project.png)

1. Nella finestra di dialogo **Nuovo progetto ASP.NET Core** selezionare il modello ASP.NET Core **Vuoto** e quindi fare clic su **OK**.

   ![Nuova finestra di dialogo Progetto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> La versione più recente di .NET Core è 2.0.

### <a name="running-the-web-app-locally"></a>Esecuzione dell'applicazione Web in locale

1. Una volta terminata la creazione dell'app da parte di Visual Studio, eseguire l'app selezionando **Debug** > **Avvia l'esecuzione del debug**. In alternativa, premere **F5**.

   L'inizializzazione di Visual Studio e della nuova app potrebbe richiedere tempo. Una volta completata, il browser visualizza app in esecuzione.

   ![La finestra del browser mostra l'applicazione in esecuzione che visualizza "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. Dopo aver esaminato l'app Web in esecuzione, chiudere il browser e selezionare l'icona "Interrompi debug" nella barra degli strumenti di Visual Studio per arrestare l'applicazione.

## <a name="create-a-web-app-in-the-azure-portal"></a>Creare un'app web nel portale di Azure

La procedura seguente crea un'app web nel portale di Azure:

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Selezionare **NEW** nella parte superiore sinistra dell'interfaccia del portale.

1. Selezionare **Web e dispositivi mobili** > **App Web**.

   ![Portale di Microsoft Azure: pulsante Nuovo: Web + Dispositivi mobili in Marketplace: pulsante Web App in App in primo piano](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. Nel pannello **App Web** immettere un valore univoco per il **Nome del servizio App**.

   ![Pannello App Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > Il **nome del servizio App** nome deve essere univoco. Il portale applica questa regola quando viene specificato il nome. Se fornire un valore diverso, sostituire tale valore per ogni occorrenza di **SampleWebAppDemo** in questa esercitazione.

   Anche nel pannello **App Web**, selezionare una **Posizione/piano di servizio App** o crearne uno nuovo. Se si crea un nuovo piano, selezionare il piano tariffario, percorso e altre opzioni. Per ulteriori informazioni sui piani di servizio App, vedere [piani di servizio App di Azure ad approfondimenti su](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).

1. Scegliere **Crea**. Azure sarà il provisioning e avviare l'app web.

   ![Portale di Azure: campione di pannello di Essentials Demo 01 di App Web](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Abilitare la pubblicazione di Git per la nuova app web

GIT è un sistema di controllo di versione distribuita che può essere usato per distribuire un'app web di servizio App di Azure. Codice di app Web è archiviato in un repository Git locale e il codice viene distribuito in Azure mediante il push in un repository remoto.

1. Accedere al [portale di Azure](https://portal.azure.com).

1. Selezionare **servizi App** per visualizzare un elenco dei servizi di app associato alla sottoscrizione di Azure.

1. Selezionare l'app web creata nella sezione precedente di questa esercitazione.

1. Nel pannello **Distribuzione** selezionare **Opzioni di distribuzione** > **Scegli origine** > **Repository Git locale**.

   ![Pannello delle impostazioni: pannello di origine della distribuzione: scegliere Pannello di origine](azure-continuous-deployment/_static/deployment-options.png)

1. Scegliere **OK**.

1. Se le credenziali di distribuzione per la pubblicazione di un'app web o altre app di servizio App in precedenza non è ancora state configurate, impostarli ora:

   * Selezionare **impostazioni** > **le credenziali di distribuzione**. Il **impostare le credenziali di distribuzione** pannello viene visualizzato.
   * Creare un nome utente e una password. Salvare la password per un uso successivo durante la configurazione di Git.
   * Selezionare **Salva**.

1. Nel **App Web** pannello seleziona **impostazioni** > **proprietà**. Viene visualizzato l'URL del repository Git remoto per distribuire in **URL GIT**.

1. Copiare il valore **URL GIT** per un uso successivo nell'esercitazione.

   ![Portale di Azure: pannello Proprietà dell'applicazione](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>Pubblicare l'app web in Azure App Service

In questa sezione, creare un repository Git locale, tramite Visual Studio e push da quel repository in Azure per distribuire l'app web. La procedura coinvolta include quanto segue:

* Aggiungere l'impostazione di repository remoto utilizzando il valore di URL GIT, pertanto il repository locale può essere distribuito in Azure.
* Eseguire il commit delle modifiche al progetto.
* Push delle modifiche di progetto dall'archivio locale nel repository remoto in Azure.

1. In **Esplora soluzioni** fare clic con il tasto destro del mouse su **Soluzione "SampleWebAppDemo"** e selezionare **Commit**. Il **Team Explorer** viene visualizzato.

   ![Scheda di connessione Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

1. In **Team Explorer** selezionare la **Home** (icona home) > **Impostazioni** > **Impostazioni del Repository**.

1. Nel **elementi remoti** sezione del **delle impostazioni Repository**selezionare **Aggiungi**. Il **aggiungere remoto** viene visualizzata la finestra di dialogo.

1. Impostare il **Nome** del repository remoto da **Azure SampleApp**.

1. Impostare il valore per **recuperare** per il **URL Git** che copiato da Azure precedentemente in questa esercitazione. Si noti che questo è l'URL che termina con **.git**.

   ![Modifica finestra di dialogo remota](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > In alternativa, specificare il repository remoto dal **finestra di comando** aprendo il **finestra di comando**, la modifica alla directory del progetto e immettendo il comando. Esempio:
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. Selezionare la **Home** (icona home) > **Impostazioni** > **Impostazioni globali**. Confermare che il nome e l'indirizzo e-mail siano impostati. Selezionare **aggiornamento** se necessario.

1. Selezionare **Home** > **Modifiche** da restituire alla vista **Modifiche**.

1. Immettere un messaggio di commit, ad esempio **iniziale Push #1** e selezionare **Commit**. Questa azione crea un *commit* localmente.

   ![Scheda di connessione Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > In alternativa, commit cambia dal **finestra di comando** aprendo il **finestra di comando**, la modifica alla directory del progetto e immettendo i comandi git. Esempio:
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. Selezionare **Home** > **Sincronizzazione** > **Azioni** > **Aprire il prompt dei comandi**. Consente di aprire il prompt dei comandi alla directory del progetto.

1. Immettere il comando seguente nella finestra Comando:

   `git push -u Azure-SampleApp master`

1. Immettere Azure **le credenziali di distribuzione** password creata in precedenza in Azure.

   Questo comando avvia il processo di push dei file di progetto locali in Azure. L'output del comando precedente termina con un messaggio che la distribuzione ha avuto esito positivo.

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > Collaborazione per il progetto è necessario, provare a push [GitHub](https://github.com) prima dell'inserimento di Azure.
 
### <a name="verify-the-active-deployment"></a>Verificare la distribuzione attiva

Verificare che il trasferimento di app web dall'ambiente locale a Azure ha esito positivo.

Nel [portale Azure](https://portal.azure.com), selezionare l'app web. Selezionare **distribuzione** > **opzioni di distribuzione**.

![Portale di Azure: pannello impostazioni: pannello di distribuzione che mostra la distribuzione con esito positivo](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Eseguire l'app in Azure

Ora che l'app web viene distribuito in Azure, eseguire l'app.

Questa operazione può essere eseguita in due modi:

* Nel portale di Azure, individuare il pannello app web per l'app web. Selezionare **Sfoglia** per visualizzare l'app nel browser predefinito.
* Aprire un browser e immettere l'URL per l'app web. Esempio: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>Aggiornare l'app web e pubblicare di nuovo

Dopo aver apportato modifiche al codice locale, pubblicare di nuovo:

1. In **Esplora soluzioni** di Visual Studio aprire il file *Startup.cs*.

1. Nel metodo `Configure` modificare il metodo `Response.WriteAsync` in modo che venga visualizzato nel seguente modo:

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. Salvare le modifiche in *Startup.cs*.

1. In **Esplora soluzioni** fare clic con il tasto destro del mouse su **Soluzione "SampleWebAppDemo"** e selezionare **Commit**. Il **Team Explorer** viene visualizzato.

1. Immettere un messaggio di commit, ad esempio `Update #2`.

1. Premere il pulsante **Commit** per salvare le modifiche del progetto.

1. Selezionare **Home** > **Sincronizzazione** > **Azioni** > **Push**.

> [!NOTE]
> In alternativa, le modifiche da eseguire il push di **finestra di comando** aprendo il **finestra di comando**, la modifica alla directory del progetto e immettere un comando di git. Esempio:
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Visualizzare l'app web aggiornata in Azure

Visualizzare l'app web sono aggiornate selezionando **Sfoglia** nel Pannello di app web nel portale di Azure o aprendo un browser e immettendo l'URL per l'app web. Esempio: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Risorse aggiuntive

* [Utilizzare Visual Studio Team Services per compilare e pubblicare un'App Web di Azure con la distribuzione continua](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [Kudu del progetto](https://github.com/projectkudu/kudu/wiki)
