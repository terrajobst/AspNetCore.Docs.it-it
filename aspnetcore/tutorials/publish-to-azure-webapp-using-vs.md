---
title: Pubblicare un'app ASP.NET Core in Azure con Visual Studio
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Cesar Blum Silveira](https://github.com/cesarbs)

## <a name="set-up-the-development-environment"></a>Configurare l'ambiente di sviluppo

* Installare la versione più recente di [Azure SDK per Visual Studio](https://www.visualstudio.com/features/azure-tools-vs). L'SDK installa Visual Studio, se non è già disponibile.

> [!NOTE]
> L'installazione dell'SDK può richiedere più di 30 minuti se il computer non dispone di molte dipendenze.

* Installare gli [strumenti .NET Core e Visual Studio](http://go.microsoft.com/fwlink/?LinkID=798306)

* Verificare l'[account Azure](https://portal.azure.com/). È possibile [aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/) o [attivare i vantaggi per i sottoscrittori di Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## <a name="create-a-web-app"></a>Creare un'app Web

Nella pagina iniziale di Visual Studio toccare **Nuovo progetto...**.

![Pagina iniziale](publish-to-azure-webapp-using-vs/_static/new_project.png)

In alternativa è possibile usare i menu per creare un nuovo progetto. Toccare **File > Nuovo > Progetto**.

![File (menu)](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

Completare la finestra di dialogo **Nuovo progetto**:

* Nel riquadro a sinistra toccare **Web**

* Nel riquadro al centro toccare **Applicazione Web ASP.NET Core (.NET Core)**

* Toccare **OK**.

![Finestra di dialogo Nuovo progetto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core (.NET Core)**:

* Toccare **Applicazione Web**.

* Verificare che **Autenticazione** sia impostato su **Account utente individuali**

* Verificare che **Ospita nel cloud** non **sia** selezionata

* Toccare **OK**.

![Finestra di dialogo Nuova Applicazione Web ASP.NET Core (.NET Core)](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a>Eseguire il test dell'app in locale

* Premere **Ctrl-F5** per eseguire l'app in locale

* Toccare i collegamenti **About** (Informazioni su) e **Contact** (Contatto). A seconda delle dimensioni del dispositivo, può essere necessario toccare l'icona di spostamento per visualizzare i collegamenti.

![Applicazione Web aperta in Microsoft Edge su localhost](publish-to-azure-webapp-using-vs/_static/show.png)

* Toccare **Registra** e registrare un nuovo utente. È possibile usare un indirizzo di posta elettronica fittizio. Quando si esegue l'invio, verrà visualizzato un messaggio come il seguente:

![Un errore interno del server indicante che un'operazione di database non è riuscita durante l'elaborazione della richiesta. Per un'eccezione SQL, non è possibile aprire il database. Per risolvere il problema, provare ad applicare le migrazioni esistenti per il contesto di database dell'applicazione.](publish-to-azure-webapp-using-vs/_static/mig.png)

È possibile risolvere il problema in due modi:

* Toccare **Apply Migrations** (Applica migrazioni) e dopo che la pagina è stata aggiornata eseguire il refresh della pagina oppure

* Eseguire il comando seguente da un prompt dei comandi nella directory del progetto:

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

L'app visualizza il messaggio di posta elettronica usato per registrare il nuovo utente e un collegamento **Disconnetti**.

![Applicazione Web aperta in Microsoft Edge Il collegamento Register (Registra) viene sostituito dal testo Hello abc@example.com!](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Distribuire l'app in Azure

Verificare che l'app pubblicata per la distribuzione non sia in esecuzione. I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app. La distribuzione non viene eseguita perché non è possibile copiare i file bloccati.

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e selezionare **Pubblica...**.

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

Nella finestra di dialogo **Pubblica** selezionare **Servizio app di Microsoft Azure**.

![Finestra di dialogo Pubblica](publish-to-azure-webapp-using-vs/_static/maas1.png)

Toccare **Nuovo...** per crea un nuovo gruppo di risorse. La creazione di un nuovo gruppo di risorse renderà più facile eliminare tutte le risorse di Azure create in questa esercitazione.

![Finestra di dialogo Servizio app](publish-to-azure-webapp-using-vs/_static/newrg1.png)

Creare un nuovo gruppo di risorse e piano del servizio app:

* Toccare **Nuovo...** per il gruppo di risorse e immettere un nome per il nuovo gruppo di risorse

* Toccare **Nuovo...**  per il piano di servizio app e selezionare una località nelle proprie vicinanze. È possibile mantenere il nome generato predefinito

* Toccare **Esplora altri servizi di Azure** per creare un nuovo database

![Finestra di dialogo Nuovo gruppo di risorse: pannello Hosting](publish-to-azure-webapp-using-vs/_static/cas.png)

* Toccare l'icona  **+**  verde per creare un nuovo database SQL

![Finestra di dialogo Nuovo gruppo di risorse: pannello Servizi](publish-to-azure-webapp-using-vs/_static/sql.png)

* Toccare **Nuovo...** nella finestra di dialogo **Configura database SQL** per creare un nuovo server di database.

![Finestra di dialogo Configura database SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

* Immettere nome utente e password di dell'amministratore e quindi toccare **OK**. Non dimenticare il nome utente e la password creati in questo passaggio. È possibile mantenere il **nome server** predefinito.

![Finestra di dialogo Configura SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> La stringa "admin" non è consentita come nome utente di amministratore.

* Toccare **OK** nella finestra di dialogo **Configura database SQL**.

![Finestra di dialogo Configura database SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* Toccare **Crea** nella finestra di dialogo **Crea servizio app**.

![Finestra di dialogo Crea servizio app](publish-to-azure-webapp-using-vs/_static/create_as.png)

* Toccare **Successivo** nella finestra di dialogo **Pubblica**.

![Finestra di dialogo Pubblica: pannello di connessione](publish-to-azure-webapp-using-vs/_static/pubc.png)

* Nel pannello **Impostazioni** della finestra di dialogo **Pubblica**:

  * Espandere **Database** e selezionare **Usa questa stringa di connessione in fase di esecuzione**

  * Espandere **Migrazioni Entity Framework** e controllare **Applica questa migrazione in fase di pubblicazione**

* Toccare **Pubblica** e attendere fino al completamento dell'app da parte di Visual Studio

![Finestra di dialogo Pubblica: pannello Impostazioni](publish-to-azure-webapp-using-vs/_static/pubs.png)

Visual Studio pubblicherà l'app in Azure e avvierà l'app cloud nel browser.

### <a name="test-your-app-in-azure"></a>Testare l'app in Azure

* Eseguire il test dei collegamenti **About** (Informazioni su) e **Contact** (Contatto).

* Registrare un nuovo utente

![Applicazione Web aperta in Microsoft Edge in Servizio app di Azure](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a>Aggiornare l'app

* Modificare il file di vista Razor `Views/Home/About.cshtml` e cambiarne il contenuto. Ad esempio:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* Fare clic con il pulsante destro del mouse sul progetto e toccare di nuovo **Pubblica**.

![Menu di scelta rapida con il collegamento per la pubblicazione evidenziato](publish-to-azure-webapp-using-vs/_static/pub.png)

* Dopo la pubblicazione dell'app, verificare che le modifiche apportate siano disponibili in Azure

### <a name="clean-up"></a>Eseguire la pulizia

Al termine del test dell'app accedere al [portale di Azure](https://portal.azure.com/) ed eliminare l'app.

* Selezionare **Gruppi di risorse**, quindi toccare il gruppo di risorse che è stato creato

![Portale di Azure: Gruppi di risorse nel menu laterale](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* Nel pannello **Gruppo di risorse** toccare **Elimina**

![Portale di Azure: pannello Gruppi di risorse](publish-to-azure-webapp-using-vs/_static/rgd.png)

* Immettere il nome del gruppo di risorse e toccare **Elimina**. A questo punto l'app e tutte le altre risorse create in questa esercitazione vengono eliminate da Azure

### <a name="next-steps"></a>Passaggi successivi

* [Introduzione ad ASP.NET Core MVC e Visual Studio](first-mvc-app/start-mvc.md)

* [Introduzione a ASP.NET Core](../index.md)

* [Concetti fondamentali](../fundamentals/index.md)
