---
title: DevOps con ASP.NET Core e Azure | Integrazione continua e distribuzione
author: CamSoper
description: Questa guida include informazioni complete sulla creazione di una pipeline DevOps per un'app ASP.NET Core ospitata in Azure.
ms.author: scaddie
ms.date: 08/17/2018
uid: azure/devops/cicd
ms.openlocfilehash: 0bfe1545da4c0778055d7c81c1588d3267d2e711
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340108"
---
# <a name="continuous-integration-and-deployment"></a>Integrazione continua e distribuzione

Nel capitolo precedente, è stato creato un repository Git locale per l'app semplice lettore di Feed. In questo capitolo verrà pubblicare tale codice in un repository GitHub e creare una pipeline di servizi di Azure DevOps con le pipeline di Azure. La pipeline consente le compilazioni continue e le distribuzioni dell'app. Qualsiasi commit al repository GitHub attiva una compilazione e una distribuzione in slot di staging dell'App Web di Azure.

In questa sezione, si completeranno le attività seguenti:

* Pubblicare il codice dell'app in GitHub
* Disconnettere la distribuzione Git locale
* Creazione di un'organizzazione di Azure DevOps
* Creare un progetto team in servizi di Azure DevOps
* Creazione di una definizione di compilazione
* Creare una pipeline di rilascio
* Eseguire il commit delle modifiche a GitHub e distribuire automaticamente in Azure
* Esaminare la pipeline della pipeline di Azure

## <a name="publish-the-apps-code-to-github"></a>Pubblicare il codice dell'app in GitHub

1. Aprire una finestra del browser e passare a `https://github.com`.
1. Scegliere il **+** elenco a discesa nell'intestazione e selezionare **nuovo repository**:

    ![Opzione nuovo Repository GitHub](media/cicd/github-new-repo.png)

1. Selezionare il proprio account nel **proprietario** elenco a discesa e immettere *lettore di feed semplice* nel **nome del Repository** nella casella di testo.
1. Scegliere il **crea repository** pulsante.
1. Aprire shell dei comandi del computer locale. Passare alla directory in cui il *lettore di feed semplice* è archiviato il repository Git.
1. Rinominare l'oggetto esistente *origin* remota a *upstream*. Eseguire il seguente comando:
    ```console
    git remote rename origin upstream
    ```
1. Aggiungere un nuovo *origin* remota che puntano alla copia locale del repository in GitHub. Eseguire il seguente comando:
    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```
1. Pubblicare il repository Git locale al repository GitHub appena creato. Eseguire il seguente comando:
    ```console
    git push -u origin master
    ```
1. Aprire una finestra del browser e passare a `https://github.com/<GitHub_username>/simple-feed-reader/`. Convalida che viene visualizzato il codice nel repository GitHub.

## <a name="disconnect-local-git-deployment"></a>Disconnettere la distribuzione Git locale

Rimuovere la distribuzione Git locale con i passaggi seguenti. Le pipeline di Azure, un servizio di Azure DevOps, sia sostituisce e aggiunge tale funzionalità.

1. Aprire il [portale di Azure](https://portal.azure.com/)e passare al *di gestione temporanea (mywebapp\<unique_number\>/staging)* App Web. L'App Web è possibile trovare rapidamente immettendo *staging* nella casella di ricerca del portale:

    ![il termine di ricerca di App Web di gestione temporanea](media/cicd/portal-search-box.png)

1. Fare clic su **opzioni di distribuzione**. Viene visualizzato un nuovo pannello. Fare clic su **Disconnect** per rimuovere la configurazione di controllo codice sorgente Git locale di che è stato aggiunto nel capitolo precedente. Confermare l'operazione di rimozione facendo il **Sì** pulsante.
1. Passare il *mywebapp < unique_number >* servizio App. Come promemoria, finestra di ricerca del portale è utilizzabile per individuare rapidamente il servizio App.
1. Fare clic su **opzioni di distribuzione**. Viene visualizzato un nuovo pannello. Fare clic su **Disconnect** per rimuovere la configurazione di controllo codice sorgente Git locale di che è stato aggiunto nel capitolo precedente. Confermare l'operazione di rimozione facendo il **Sì** pulsante.

## <a name="create-an-azure-devops-organization"></a>Creazione di un'organizzazione di Azure DevOps

1. Aprire un browser e passare per il [pagina di creazione dell'organizzazione di Azure DevOps](https://go.microsoft.com/fwlink/?LinkId=307137).
1. Digitare un nome univoco nel **scegliere un nome facile da ricordare** nella casella di testo per comporre l'URL per l'accesso dell'organizzazione di Azure DevOps.
1. Selezionare il **Git** pulsante di opzione, poiché il codice è ospitato in un repository GitHub.
1. Fare clic sul pulsante **Continue** (Continua). Dopo una breve attesa, un account e un progetto team, denominato *MyFirstProject*, vengono creati.

    ![Pagina di creazione di Azure DevOps dell'organizzazione](media/cicd/vsts-account-creation.png)

1. Aprire il messaggio di posta elettronica di conferma che indica che l'organizzazione di Azure DevOps e progetto siano pronte per l'uso. Scegliere il **avvia il tuo progetto** pulsante:

    ![Il pulsante di progetto di avvio](media/cicd/vsts-start-project.png)

1. Viene aperto un browser  *\<account_name\>. visualstudio.com*. Scegliere il *MyFirstProject* collegamento per avviare la configurazione della pipeline di DevOps del progetto.

## <a name="configure-the-azure-pipelines-pipeline"></a>Configurare la pipeline della pipeline di Azure

Esistono tre passaggi distinti per il completamento. Completare i passaggi nei risultati di tre sezioni seguenti in una pipeline DevOps operativa.

### <a name="grant-azure-devops-access-to-the-github-repository"></a>DevOps di Azure di concedere l'accesso al repository GitHub

1. Espandere la **o compilare il codice da un repository esterno** accordion. Scegliere il **compilare il programma di installazione** pulsante:

    ![Pulsante di compilazione il programma di installazione](media/cicd/vsts-setup-build.png)

1. Selezionare il **GitHub** opzione il **consente di selezionare un'origine** sezione:

    ![Selezionare un'origine - GitHub](media/cicd/vsts-select-source.png)

1. È necessaria l'autorizzazione prima di DevOps di Azure possono accedere al repository GitHub. Immettere *< GitHub_username > connessione GitHub* nel **nome connessione** nella casella di testo. Ad esempio:

    ![Nome della connessione GitHub](media/cicd/vsts-repo-authz.png)

1. Se l'autenticazione a due fattori è abilitata nell'account GitHub, è necessario un token di accesso personale. In tal caso, scegliere il **Authorize con un token di accesso personale GitHub** collegamento. Vedere le [istruzioni di creazione del token di accesso personale GitHub ufficiali](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) per assistenza. Solo le *repository* ambito delle autorizzazioni è necessaria. In caso contrario, scegliere il **autorizza con OAuth** pulsante.
1. Quando richiesto, accedere al proprio account GitHub. Selezionare quindi autorizza per concedere l'accesso all'organizzazione di Azure DevOps. Se ha esito positivo, viene creato un nuovo endpoint del servizio.
1. Fare clic sui puntini di sospensione accanto al **Repository** pulsante. Selezionare il *< GitHub_username > / lettore di feed semplice* repository dall'elenco. Scegliere il **seleziona** pulsante.
1. Selezionare il *master* creare un ramo dalle **ramo predefinito per le compilazioni manuale e pianificate** elenco a discesa. Fare clic sul pulsante **Continue** (Continua). Viene visualizzata la pagina di selezione del modello.

### <a name="create-the-build-definition"></a>Creare la definizione di compilazione

1. Nella pagina di selezione del modello, immettere *ASP.NET Core* nella casella di ricerca:

    ![Ricerca di ASP.NET Core sulla pagina del modello](media/cicd/vsts-template-selection.png)

1. Vengono visualizzati i risultati della ricerca del modello. Passare il mouse sul **ASP.NET Core** e scegliere il **applica** pulsante.
1. Il **attività** verrà visualizzata la scheda della definizione di compilazione. Fare clic sulla scheda **Trigger** .
1. Verificare i **abilitare l'integrazione continua** casella. Sotto il **filtri** sezione, verificare che il **tipo** elenco a discesa è impostata su *inclusione*. Impostare il **specifica rami** elenco a discesa per *master*.

    ![Abilitare le impostazioni di integrazione continua](media/cicd/vsts-enable-ci.png)

    Queste impostazioni causano una compilazione da attivare quando viene eseguito il push di qualsiasi modifica apportata per la *master* ramo del repository GitHub. Integrazione continua è stato testato nel [confermare le modifiche a GitHub e distribuire automaticamente in Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) sezione.

1. Scegliere il **Salva e accoda** e selezionare il **salvare** opzione:

    ![Pulsante Salva](media/cicd/vsts-save-build.png)

1. Viene visualizzata la finestra di dialogo modale seguente:

    ![Salva definizione di compilazione - finestra di dialogo modale](media/cicd/vsts-save-modal.png)

    Usare la cartella predefinita del *\\*, fare clic sui **salvare** pulsante.

### <a name="create-the-release-pipeline"></a>Creare la pipeline di rilascio

1. Scegliere il **rilasci** scheda del progetto team. Scegliere il **nuova pipeline** pulsante.

    ![Scheda versioni - pulsante per la nuova definizione](media/cicd/vsts-new-release-definition.png)

    Viene visualizzato il riquadro di selezione del modello.

1. Nella pagina di selezione del modello, immettere *servizio App* nella casella di ricerca:

    ![Casella di ricerca modello pipeline di rilascio](media/cicd/vsts-release-template-search.png)

1. Vengono visualizzati i risultati della ricerca del modello. Passare il mouse sul **distribuzione servizio App di Azure con Slot** e scegliere il **applica** pulsante. Il **Pipeline** verrà visualizzata la scheda della pipeline di rilascio.

    ![Pipeline di rilascio della scheda della Pipeline](media/cicd/vsts-release-definition-pipeline.png)

1. Fare clic sui **Add** pulsante nel **elementi** casella. Il **Aggiungi artefatto** pannello viene visualizzato:

    ![Pipeline di rilascio, aggiungere il pannello dell'artefatto](media/cicd/vsts-release-add-artifact.png)

1. Selezionare il **compilare** dal riquadro le **tipo di origine** sezione. Questo tipo consente il collegamento della pipeline di rilascio per la definizione di compilazione.
1. Selezionare *MyFirstProject* dalle **progetto** elenco a discesa.
1. Selezionare il nome della definizione di compilazione *MyFirstProject-ASP.NET Core-CI*, dalle **origine (definizione di compilazione)** elenco a discesa.
1. Selezionare *più recente* dalle **versione predefinita** elenco a discesa. Questa opzione Crea gli elementi generati dall'esecuzione più recente della definizione di compilazione.
1. Sostituire il testo di **alias di origine** nella casella di testo con *Drop*.
1. Fare clic sul pulsante **Aggiungi**. Il **artefatti** sezione viene aggiornato per visualizzare le modifiche.
1. Scegliere l'icona a forma di fulmine per abilitare la distribuzione continua:

    ![Pipeline di rilascio elementi - sull'icona saetta](media/cicd/vsts-artifacts-lightning-bolt.png)

    Con questa opzione è abilitata, una distribuzione si verifica ogni volta che è disponibile una nuova compilazione.
1. Oggetto **trigger di distribuzione continua** pannello viene visualizzato a destra. Fare clic sull'interruttore per abilitare la funzionalità. Non è necessario abilitare la **trigger di richiesta Pull**.
1. Fare clic sui **Add** elenco a discesa nel **creare filtri per rami** sezione. Scegliere il **ramo predefinito della definizione di compilazione** opzione. Questo filtro causa il rilascio attivare solo per una compilazione del repository di GitHub *master* ramo.
1. Fare clic sul pulsante **Salva**. Fare clic sui **OK** pulsante risultante **salvare** finestra di dialogo modale.
1. Scegliere il **ambiente 1** casella. Un' **ambiente** pannello viene visualizzato a destra. Modifica il *ambiente 1* testo nel **nome ambiente** casella di testo *produzione*.

   ![Pipeline di rilascio - casella di testo Nome ambiente](media/cicd/vsts-environment-name-textbox.png)

1. Fare clic sui **1 fase, 2 attività** clic sul collegamento nella **produzione** casella:

    ![Pipeline di rilascio - link.png ambiente di produzione](media/cicd/vsts-production-link.png)

    Il **attività** verrà visualizzata la scheda dell'ambiente.
1. Scegliere il **distribuzione servizio App di Azure per lo Slot** attività. Le impostazioni vengono visualizzate in un pannello a destra.
1. Selezionare la sottoscrizione di Azure associata al servizio App dal **sottoscrizione di Azure** elenco a discesa. Una volta selezionato, scegliere il **Authorize** pulsante.
1. Selezionare *App Web* dalle **tipo di App** elenco a discesa.
1. Selezionare *mywebapp / < unique_number / >* dalle **nome del servizio App** elenco a discesa.
1. Selezionare *AzureTutorial* dalle **gruppo di risorse** elenco a discesa.
1. Selezionare *di gestione temporanea* dalle **Slot** elenco a discesa.
1. Fare clic sul pulsante **Salva**.
1. Passare il mouse sul nome della pipeline di versione predefinito. Fare clic sull'icona della matita per modificarlo. Uso *MyFirstProject-ASP.NET Core-CD* come nome.

    ![Nome della pipeline di rilascio](media/cicd/vsts-release-definition-name.png)

1. Fare clic sul pulsante **Salva**.

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Eseguire il commit delle modifiche a GitHub e distribuire automaticamente in Azure

1. Aprire *SimpleFeedReader.sln* in Visual Studio.
1. In Esplora soluzioni, aprire *Pages\Index.cshtml*. Change `<h2>Simple Feed Reader - V3</h2>` a `<h2>Simple Feed Reader - V4</h2>`.
1. Premere **Ctrl**+**MAIUSC**+**B** per compilare l'app.
1. Eseguire il commit di file al repository GitHub. Usare la **modifiche** pagina in Visual Studio *Team Explorer* scheda o eseguire le operazioni seguenti utilizzando shell dei comandi del computer locale:

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. Il push della modifica *master* branch per il *origin* remota del repository GitHub:

    ```console
    git push origin master
    ```

    Viene visualizzato il commit nel repository di GitHub *master* ramo:

    ![Commit GitHub nel ramo master](media/cicd/github-commit.png)

    Viene attivata la compilazione, perché nella definizione di compilazione è abilitata l'integrazione continua **trigger** scheda:

    ![abilitare l'integrazione continua](media/cicd/enable-ci.png)

1. Passare al **in coda** scheda della finestra di **le pipeline di Azure** > **Compila** pagina in servizi di Azure DevOps. La compilazione in coda Mostra i rami e commit che ha attivato la compilazione:

    ![compilazione in coda](media/cicd/build-queued.png)

1. Al termine della compilazione, si verifica una distribuzione in Azure. Passare all'app nel browser. Si noti che il testo "V4" viene visualizzato nell'intestazione di:

    ![app aggiornata](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a>Esaminare la pipeline della pipeline di Azure

### <a name="build-definition"></a>Definizione di compilazione

Una definizione di compilazione è stata creata con il nome *MyFirstProject-ASP.NET Core-CI*. Al termine, la compilazione produce un *zip* file tra cui gli asset da pubblicare. La pipeline di rilascio consente di distribuire tali risorse in Azure.

La definizione di compilazione **attività** scheda vengono elencati i singoli passaggi in uso. Esistono cinque attività di compilazione.

![attività definizioni di compilazione](media/cicd/build-definition-tasks.png)

1. **Ripristinare** &mdash; esegue il `dotnet restore` comando per ripristinare i pacchetti NuGet dell'app. Il pacchetto predefinito feed usato è nuget.org.
1. **Compilare** &mdash; esegue il `dotnet build --configuration release` comando per compilare il codice dell'app. Ciò `--configuration` opzione viene utilizzata per produrre una versione ottimizzata del codice, vale a dire appropriato per la distribuzione in un ambiente di produzione. Modificare il *BuildConfiguration* variabile sulla definizione di compilazione **variabili** scheda se, ad esempio, non è necessaria una configurazione di debug.
1. **Test** &mdash; esegue il `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` comando per eseguire gli unit test dell'app. Gli unit test vengono eseguiti all'interno di qualsiasi c# progetto corrispondente il `**/*Tests/*.csproj` criterio glob. I risultati del test vengono salvati un *trx* file nel percorso specificato per il `--results-directory` opzione. Se i test hanno esito negativo, la compilazione ha esito negativo e non è stata distribuita.

    > [!NOTE]
    > Per verificare l'unità di lavoro di test, modificare *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* intenzionalmente interrompere uno dei test. Ad esempio, modificare `Assert.True(result.Count > 0);` al `Assert.False(result.Count > 0);` nel `Returns_News_Stories_Given_Valid_Uri` (metodo). Eseguire il commit e push della modifica in GitHub. La compilazione viene attivata e avrà esito negativo. Lo stato della pipeline di compilazione diventa **non è stato possibile**. Ripristinare le modifiche, commit e push. La compilazione ha esito positivo.

1. **Pubblicare** &mdash; esegue il `dotnet publish --configuration release --output <local_path_on_build_agent>` comando per produrre una *zip* file con gli elementi da distribuire. Il `--output` opzione consente di specificare il percorso di pubblicazione le *zip* file. Che l'indirizzo è specificato passando una [variabile predefinita](https://docs.microsoft.com/vsts/pipelines/build/variables) denominata `$(build.artifactstagingdirectory)`. Tale variabile si espande in un percorso locale, ad esempio *c:\agent\_work\1\a*, dell'agente di compilazione.
1. **Pubblica artefatto** &mdash; Publishes le *zip* file prodotto dal **Publish** attività. L'attività accetta il *zip* percorso come un parametro, ovvero la variabile predefinita dei file `$(build.artifactstagingdirectory)`. Il *zip* i file vengono pubblicati come una cartella denominata *drop*.

Fare clic sulla definizione di compilazione **riepilogo** collegamento per visualizzare una cronologia delle compilazioni con la definizione di:

![cronologia della definizione di compilazione](media/cicd/build-definition-summary.png)

Nella pagina risulta, fare clic sul collegamento corrispondente al numero di build univoco:

![pagina Riepilogo definizione di compilazione](media/cicd/build-definition-completed.png)

Viene visualizzato un riepilogo di questa compilazione specifica. Fare clic sui **artefatti** scheda e notare il *drop* prodotto dalla compilazione cartella verrà elencata:

![creare elementi di definizione - cartella di ricezione](media/cicd/build-definition-artifacts.png)

Usare la **scaricare** e **Explore** collegamenti per controllare gli elementi pubblicati.

### <a name="release-pipeline"></a>Pipeline di rilascio

Una pipeline di rilascio è stata creata con il nome *MyFirstProject-ASP.NET Core-CD*:

![Panoramica della pipeline di rilascio](media/cicd/release-definition-overview.png)

I due componenti principali della pipeline di rilascio sono le **artefatti** e il **ambienti**. Facendo clic sulla casella nella **artefatti** sezione mostra il pannello seguente:

![elementi della pipeline di rilascio](media/cicd/release-definition-artifacts.png)

Il **origine (definizione di compilazione)** valore rappresenta la definizione di compilazione a cui è collegata questa pipeline di rilascio. Il *zip* file prodotto da un'esecuzione riuscita della definizione di compilazione viene fornito per il *produzione* ambiente per la distribuzione in Azure. Fare clic sui *1 fase, 2 attività* clic sul collegamento nella *produzione* ambiente (environment) per visualizzare le attività della pipeline di rilascio:

![attività della pipeline di rilascio](media/cicd/release-definition-tasks.png)

La pipeline di rilascio è costituito da due attività: *distribuzione servizio App di Azure per lo Slot* e *gestire servizio App di Azure - di scambio Slot*. Facendo clic la prima attività, vengono visualizzate la configurazione delle operazioni seguenti:

![pipeline di rilascio delle attività di distribuzione](media/cicd/release-definition-task1.png)

La sottoscrizione di Azure, tipo di servizio, nome dell'app web, gruppo di risorse e lo slot di distribuzione sono definiti nell'attività di distribuzione. Il **pacchetto o cartella** nella casella di testo contiene il *zip* percorso del file da estrarre e distribuita nel *staging* slot del *mywebapp\<univoco numero\>*  app web.

Fare clic sull'attività di scambio di slot rivela la configurazione delle operazioni seguenti:

![attività di scambio dello slot di pipeline di rilascio](media/cicd/release-definition-task2.png)

La sottoscrizione, gruppo di risorse, tipo di servizio, nome dell'app web e i dettagli di uno slot di distribuzione vengono forniti. Il **scambia con produzione** casella di controllo è selezionata. Di conseguenza, i bit distribuiti per la *staging* slot vengono scambiate nell'ambiente di produzione.

## <a name="additional-reading"></a>Altre informazioni

* [Creare la prima pipeline con Azure Pipelines](/azure/devops/pipelines/get-started-yaml)
* [Progetto di compilazione e .NET Core](/azure/devops/pipelines/languages/dotnet-core)
* [Distribuire un'app web con le pipeline di Azure](/azure/devops/pipelines/targets/webapp)
