---
title: Distribuire un'app nel servizio App - DevOps con ASP.NET Core e Azure
author: CamSoper
description: Distribuire un'app ASP.NET Core in servizio App di Azure, il primo passaggio per DevOps con ASP.NET Core e Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: 9fe17c9e210d4dda9b74818104fc52a60d4f0077
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284539"
---
# <a name="deploy-an-app-to-app-service"></a>Distribuire un'app in servizio App

[Servizio App di Azure](/azure/app-service/) è piattaforma di hosting web di Azure. Distribuzione di un'app web in servizio App di Azure può essere eseguita manualmente o mediante un processo automatizzato. In questa sezione della Guida vengono illustrati i metodi di distribuzione che possono essere attivati manualmente o tramite script utilizzando la riga di comando oppure attivata manualmente tramite Visual Studio.

In questa sezione, è possibile eseguire le attività seguenti:

* Scaricare e compilare l'app di esempio.
* Creare un'App Web di Azure App Service usando Azure Cloud Shell.
* Distribuire l'app di esempio in Azure tramite Git.
* Distribuire una modifica all'app usando Visual Studio.
* Aggiungere uno slot di staging all'app web.
* Distribuire un aggiornamento allo slot di staging.
* Scambiare gli slot di staging e produzione.

## <a name="download-and-test-the-app"></a>Scaricare e testare l'app

L'app usata in questa guida è un'app ASP.NET Core preesistente [semplice lettore di Feed](https://github.com/Azure-Samples/simple-feed-reader/). Si tratta di un'app di pagine Razor che usa il `Microsoft.SyndicationFeed.ReaderWriter` API per recuperare un feed RSS/Atom e visualizzare gli articoli di notizie in un elenco.

È possibile esaminare il codice, ma è importante comprendere che non ci sono particolari sull'app. È sufficiente una semplice app ASP.NET Core a scopo illustrativo.

Da una shell dei comandi, scaricare il codice, compilare il progetto ed eseguirlo come indicato di seguito.

> *Nota: Gli utenti Linux/macOS devono apportare le modifiche appropriate per i percorsi, ad esempio, con barra rovesciata (`/`) anziché una barra rovesciata (`\`).*

1. Clonare il codice in una cartella nel computer locale.

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. Modificare la cartella di lavoro per le *lettore di feed semplice* cartella in cui è stato creato.

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. Ripristinare i pacchetti e compilare la soluzione.

    ```console
    dotnet build
    ```

4. Eseguire l'app.

    ```console
    dotnet run
    ```

    ![Il comando dotnet run ha esito positivo](./media/deploying-to-app-service/dotnet-run.png)

5. Aprire un browser e passare a `http://localhost:5000`. L'app consente di digitare o incollare un URL del feed di diffusione e visualizzare un elenco delle notizie.

     ![L'app Visualizza il contenuto di un feed RSS](./media/deploying-to-app-service/app-in-browser.png)

6. Dopo aver completato l'app funziona correttamente, arrestarlo premendo **Ctrl**+**C** nella shell dei comandi.

## <a name="create-the-azure-app-service-web-app"></a>Creare l'App Web di servizio App di Azure

Per distribuire l'app, è necessario creare un servizio App [App Web](/azure/app-service/app-service-web-overview). Dopo la creazione dell'App Web, verrà distribuita a esso dal computer locale tramite Git.

1. Accedi per il [Azure Cloud Shell](https://shell.azure.com/bash). Nota: Quando si accede per la prima volta, Cloud Shell chiede di creare un account di archiviazione per i file di configurazione. Accettare le impostazioni predefinite oppure specificare un nome univoco.

2. Usare Cloud Shell per i passaggi seguenti.

    a. Dichiarare una variabile per archiviare il nome dell'app web. Il nome deve essere univoco da usare nell'URL predefinito. Usando il `$RANDOM` funzione Bash per costruire il nome garantisce l'univocità e i risultati nel formato `webappname99999`.

    ```console
    webappname=mywebapp$RANDOM
    ```

    b. Creare un gruppo di risorse. Gruppi di risorse forniscono un mezzo per aggregare le risorse di Azure da gestire come gruppo.

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    Il `az` comando richiama il [CLI Azure](/cli/azure/). L'interfaccia della riga di comando può essere eseguito in locale, ma l'utilizzo in Cloud Shell consente di risparmiare tempo e configurazione.

    c. Creare un piano di servizio App nel livello S1. Un piano di servizio App è un raggruppamento di App web che condividono lo stesso livello di prezzo. Il piano S1 non è disponibile, ma è obbligatorio per la funzionalità degli slot di staging.

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    d. Creare la risorsa dell'app web usando il piano di servizio App nello stesso gruppo di risorse.

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    e. Impostare le credenziali di distribuzione. Queste credenziali per la distribuzione si applicano a tutte le app web nella sottoscrizione. Non usare caratteri speciali nel nome utente.

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    f. Configurare l'app web per accettare le distribuzioni da Git locale e la visualizzazione di *URL di distribuzione Git*. **Si noti l'URL per un secondo momento**.

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    g. Visualizzare il *URL dell'app web*. Passare a questo URL per visualizzare l'app web vuota. **Si noti l'URL per un secondo momento**.

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. Usando una shell dei comandi nel computer locale, passare alla cartella del progetto dell'app web (ad esempio, `.\simple-feed-reader\SimpleFeedReader`). Eseguire i comandi seguenti per configurare Git per effettuare il push all'URL di distribuzione:

    a. Aggiungere l'URL remoto nel repository locale.

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    b. Push locale *master* branch per il *azure-produzione* del remoto *master* ramo.

    ```console
    git push azure-prod master
    ```

    Verrà richiesto per le credenziali di distribuzione creata in precedenza. Osservare l'output nella shell dei comandi. Azure crea l'app ASP.NET Core in remoto.

4. In un browser, passare al *URL dell'app Web* e prendere nota dell'app è stato compilato e distribuito. Modifiche aggiuntive possono essere eseguito il commit nel repository Git locale con `git commit`. Queste modifiche vengono inserite in Azure con il precedente `git push` comando.

## <a name="deployment-with-visual-studio"></a>Distribuzione con Visual Studio

> *Nota: In questa sezione si applica solo a Windows. Gli utenti di Linux e macOS è necessario apportare la modifica descritta nel passaggio 2 di seguito. Salvare il file ed eseguire il commit della modifica nel repository locale con `git commit`. Infine, il push della modifica con `git push`, come nella prima sezione.*

L'app è già stata distribuita dalla shell dei comandi. Utilizziamo gli strumenti integrati di Visual Studio per distribuire un aggiornamento all'app. Dietro le quinte, Visual Studio esegue la stessa operazione come la riga di comando degli strumenti, ma all'interno dell'interfaccia utente familiare di Visual Studio.

1. Aprire *SimpleFeedReader.sln* in Visual Studio.
2. In Esplora soluzioni, aprire *Pages\Index.cshtml*. Change `<h2>Simple Feed Reader</h2>` a `<h2>Simple Feed Reader - V2</h2>`.
3. Premere **Ctrl**+**MAIUSC**+**B** per compilare l'app.
4. In Esplora soluzioni fare doppio clic sul progetto e fare clic su **pubblica**.

    ![Screenshot che Mostra pulsante destro del mouse, pubblicazione](./media/deploying-to-app-service/publish.png)
5. Visual Studio possa creare una nuova risorsa di servizio App, ma questo aggiornamento verrà pubblicato tramite la distribuzione esistente. Nel **selezionare una destinazione di pubblicazione** finestra di dialogo, seleziona **servizio App** dall'elenco a sinistra e quindi selezionare **seleziona esistente**. Fare clic su **Pubblica**.
6. Nel **servizio App** finestra di dialogo, verificare che l'account Microsoft o aziendale utilizzato per creare la sottoscrizione di Azure venga visualizzato in alto a destra. In caso contrario, fare clic sul menu a discesa e aggiungerlo.
7. Verificare che Azure corretto **sottoscrizione** sia selezionata. Per la **View**, selezionare **gruppo di risorse**. Espandere la **AzureTutorial** gruppo di risorse e quindi selezionare l'app web esistente. Fare clic su **OK**.

    ![Finestra di dialogo di screenshot che illustra la pubblicazione del servizio App](./media/deploying-to-app-service/publish-dialog.png)

Visual Studio compila e distribuisce l'app in Azure. Passare all'URL dell'app web. Verificare che il `<h2>` Modifica elemento è in tempo reale.

![L'app con la modifica del titolo](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a>Slot di distribuzione

Gli slot di distribuzione supportano la gestione temporanea delle modifiche senza conseguenze per le app in esecuzione nell'ambiente di produzione. Dopo aver convalidata da un team di garanzia di qualità, la versione dell'app per le fasi di produzione e di slot di staging è possibile scambiare. L'app in gestione temporanea viene promosso alla produzione in questo modo. La procedura seguente crea uno slot di staging, distribuisce alcune modifiche a esso e scambia lo slot di staging alla produzione dopo la verifica.

1. Accedi per il [Azure Cloud Shell](https://shell.azure.com/bash), se non già effettuato l'accesso.
2. Creare lo slot di staging.

    a. Creare uno slot di distribuzione con il nome *staging*.

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    b. Configurare lo slot di staging per usare la distribuzione locale di Git e ottenere il **staging** URL di distribuzione. **Si noti l'URL per un secondo momento**.

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    c. Visualizzazione URL dello slot di staging. Passare all'URL per visualizzare lo slot di staging vuoto. **Si noti l'URL per un secondo momento**.

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. In un editor di testo o Visual Studio, modificare *cshtml* nuovamente in modo che le `<h2>` elemento legge `<h2>Simple Feed Reader - V3</h2>` e salvare il file.

4. Eseguire il commit del file al repository Git locale, usando il **modifiche** pagina in Visual Studio *Team Explorer* scheda o immettendo quanto segue utilizzando shell dei comandi del computer locale:

    ```console
    git commit -a -m "upgraded to V3"
    ```
5. Utilizzando shell dei comandi del computer locale, aggiungere l'URL di distribuzione di staging come un Git remoto e il push il commit delle modifiche:

    a. Aggiungere l'URL remoto per la gestione temporanea per il repository Git locale.

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    b. Push locale *master* branch per il *azure-gestione temporanea* del remoto *master* ramo.

    ```console
    git push azure-staging master
    ```

    Attendere che Azure crea e distribuisce l'app.

6. Per verificare che sia stato distribuito V3 allo slot di staging, aprire due finestre del browser. In una finestra, passare all'URL dell'app web originale. In altra finestra, passare all'URL di app web di staging. L'URL di produzione serve V2 dell'app. L'URL di gestione temporanea serve V3 dell'app.

    ![Schermata di confronto tra le finestre del browser](./media/deploying-to-app-service/ready-to-swap.png)

7. In Cloud Shell, trasferire lo slot di gestione temporanea verificata/preparata-up nell'ambiente di produzione.

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. Verificare che lo scambio si è verificato aggiornando le finestre del due browser.

    ![Confronto tra le finestre del browser al termine dello scambio](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a>Riepilogo

In questa sezione sono state completate le attività seguenti:

* Scaricata e compilata l'app di esempio.
* Creare un'App Web di Azure App Service usando Azure Cloud Shell.
* Distribuire l'app di esempio in Azure con Git.
* Distribuire una modifica all'app usando Visual Studio.
* Aggiungere uno slot di staging all'app web.
* Distribuzione di un aggiornamento allo slot di staging.
* Scambiare gli slot di staging e produzione.

Nella sezione successiva, si apprenderà come creare una pipeline di DevOps con le pipeline di Azure.

## <a name="additional-reading"></a>Altre informazioni

* [Panoramica delle App Web](/azure/app-service/app-service-web-overview)
* [Creare un'app web .NET Core e Database SQL di Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [Configurare le credenziali di distribuzione per il servizio App di Azure](/azure/app-service/app-service-deployment-credentials)
* [Configurare ambienti di servizio App di Azure di staging](/azure/app-service/web-sites-staged-publishing)
