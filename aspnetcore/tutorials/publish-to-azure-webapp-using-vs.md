---
title: Pubblicare un'app ASP.NET Core in Azure con Visual Studio
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/01/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 14ce45f0cd15b2de39f722767df076d2c0313787
ms.sourcegitcommit: 6ece943781d8a56784bb6160f14da85210d3fcea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio

Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) e [Rachel Appel](https://twitter.com/rachelappel)

## <a name="set-up-the-development-environment"></a>Configurare l'ambiente di sviluppo

* Installare gli [strumenti di .NET Core e Visual Studio](http://go.microsoft.com/fwlink/?LinkID=798306).

* Verificare l'[account Azure](https://portal.azure.com/). È possibile [aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/) o [attivare i vantaggi per i sottoscrittori di Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## <a name="create-a-web-app"></a>Creare un'app Web

Nella Pagina iniziale di Visual Studio selezionare **File > Nuovo > Progetto**

![File (menu)](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

Completare la finestra di dialogo **Nuovo progetto**:

* Nel riquadro a sinistra selezionare **.NET Core**.

* Nel riquadro al centro selezionare **Applicazione Web ASP.NET Core**.

* Scegliere **OK**.

![Finestra di dialogo Nuovo progetto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

Nella finestra di dialogo **Nuova applicazione Web ASP.NET Core**:

* Selezionare **Applicazione Web**.

* Selezionare **Modifica autenticazione**.

![Finestra di dialogo Nuovo progetto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

Viene visualizzata la finestra di dialogo **Modifica autenticazione**. 

* Selezionare **Account utente individuali**.

* Selezionare **OK** per tornare a **Nuova applicazione Web ASP.NET Core** e selezionare nuovamente **OK**.

![Finestra per autenticazione Nuova applicazione Web ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

Visual Studio crea la soluzione.

## <a name="run-the-app-locally"></a>Eseguire l'app in locale

* Per eseguire l'app in locale, scegliere **Debug** e **Avvia senza eseguire debug**.

* Fare clic sui collegamenti **Informazioni su** e su **Contatto** per verificare che l'applicazione Web funzioni.

![Applicazione Web aperta in Microsoft Edge su localhost](publish-to-azure-webapp-using-vs/_static/show.png)

* Selezionare **Registra** e registrare un nuovo utente. È possibile usare un indirizzo di posta elettronica fittizio. Quando si esegue l'invio, nella pagina viene visualizzato l'errore seguente:

    *"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."* (Errore interno del server: Operazione sul database non riuscita durante l'elaborazione della richiesta. Eccezione SQL: Impossibile aprire il file di database. Per risolvere il problema, applicare le migrazioni esistenti per il contesto di database dell'applicazione).

* Selezionare **Apply Migrations** (Applica migrazioni) e, quando la pagina è stata caricata, eseguire l'aggiornamento.

![Un errore interno del server indicante che un'operazione di database non è riuscita durante l'elaborazione della richiesta. Per un'eccezione SQL, non è possibile aprire il database. Per risolvere il problema, provare ad applicare le migrazioni esistenti per il contesto di database dell'applicazione.](publish-to-azure-webapp-using-vs/_static/mig.png)

L'app visualizza l'indirizzo di posta elettronica usato per registrare il nuovo utente e un collegamento **Disconnessione**.

![Applicazione Web aperta in Microsoft Edge Il collegamento Register (Registra) viene sostituito dal testo Hello email@domain.com!](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Distribuire l'app in Azure

Chiudere la pagina Web, tornare a Visual Studio e selezionare **Termina debug** dal menu **Debug**.

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica...**.

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

Nella finestra di dialogo **Pubblica** selezionare **Servizio app di Microsoft Azure** e fare clic su **Pubblica**.

![Finestra di dialogo Pubblica](publish-to-azure-webapp-using-vs/_static/maas1.png)

* Assegnare all'app un nome univoco. 

* Selezionare una sottoscrizione MSDN.

* Selezionare **Nuovo** per il gruppo di risorse e immettere un nome per il nuovo gruppo di risorse.

* Selezionare **Nuovo ** per il piano di servizio app e selezionare una località nelle proprie vicinanze. È possibile tenere il nome che viene generato per impostazione predefinita.

![Finestra di dialogo Servizio app](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* Selezionare la scheda **Servizi** per creare un nuovo database.

* Selezionare l'icona verde **+** per creare un nuovo database SQL

![Nuovo database SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* Selezionare **Nuovo** nella finestra di dialogo **Configura database SQL** per creare un nuovo database.

![Nuovo database SQL e server](publish-to-azure-webapp-using-vs/_static/conf.png)

Viene visualizzata la finestra di dialogo **Configura SQL Server**.

* Immettere nome utente e password di amministratore e selezionare **OK**. Non dimenticare il nome utente e la password creati in questo passaggio. È possibile mantenere il **Nome server** predefinito. 

* Immettere un nome per la stringa di connessione e il database.

> [!NOTE]
> La stringa "admin" non è consentita come nome utente di amministratore.

![Finestra di dialogo Configura SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* Scegliere **OK**.

Visual Studio torna alla finestra di dialogo **Crea servizio app**.

* Selezionare **Crea** nella finestra di dialogo **Crea servizio app**.

![Finestra di dialogo Configura database SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* Fare clic sul collegamento **Impostazioni** della finestra di dialogo **Pubblica**.

![Finestra di dialogo Pubblica: pannello di connessione](publish-to-azure-webapp-using-vs/_static/pubc.png)

Nella pagina **Impostazioni** della finestra di dialogo **Pubblica**:

  * Espandere **Database** e selezionare **Usa questa stringa di connessione in fase di esecuzione**.

  * Espandere **Migrazioni Entity Framework** e selezionare **Applica questa migrazione in fase di pubblicazione**.

* Selezionare **Salva**. Visual Studio torna alla finestra di dialogo **Pubblica**. 

![Finestra di dialogo Pubblica: pannello Impostazioni](publish-to-azure-webapp-using-vs/_static/pubs.png)

Fare clic su **Pubblica**. Visual Studio pubblicherà l'app in Azure e avvierà l'app cloud nel browser.

### <a name="test-your-app-in-azure"></a>Testare l'app in Azure

* Eseguire il test dei collegamenti **About** (Informazioni su) e **Contact** (Contatto).

* Registrare un nuovo utente

![Applicazione Web aperta in Microsoft Edge in Servizio app di Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>Aggiornare l'app

* Modificare la pagina Razor *Pages/About.cshtml* e modificarne il contenuto. Ad esempio, è possibile modificare il paragrafo specificando "Hello ASP.NET Core!":

    [!code-html[Informazioni su](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* Fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica**.

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

* Dopo la pubblicazione dell'app, verificare che le modifiche apportate siano disponibili in Azure.

![Verificare che l'attività sia stata completata](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>Eseguire la pulizia

Al termine del test dell'app accedere al [portale di Azure](https://portal.azure.com/) ed eliminare l'app.

* Selezionare **Gruppi di risorse** e in seguito il gruppo di risorse che è stato creato.

![Portale di Azure: Gruppi di risorse nel menu laterale](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* Nella pagina **Gruppi di risorse** selezionare **Elimina**.

![Portale di Azure: pagina Gruppi di risorse](publish-to-azure-webapp-using-vs/_static/rgd.png)

* Immettere il nome del gruppo di risorse e selezionare **Elimina**. A questo punto l'app e tutte le altre risorse create in questa esercitazione vengono eliminate da Azure.

### <a name="next-steps"></a>Passaggi successivi

* [Introduzione ad ASP.NET Core MVC e Visual Studio](first-mvc-app/start-mvc.md)

* [Introduzione a ASP.NET Core](../index.md)

* [Concetti fondamentali](../fundamentals/index.md)
