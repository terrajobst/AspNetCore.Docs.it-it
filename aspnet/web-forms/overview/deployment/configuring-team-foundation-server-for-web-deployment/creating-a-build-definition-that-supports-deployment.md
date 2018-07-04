---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Creazione di una definizione di compilazione che supporta la distribuzione | Microsoft Docs
author: jrjlee
description: Se si desidera eseguire qualsiasi tipo di compilazione in Team Foundation Server (TFS) 2010, è necessario creare una definizione di compilazione all'interno del progetto team. Des in questo argomento...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: d0220ee5c7c066d107311c1235949cec5049cc27
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370290"
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Creazione di una definizione di compilazione che supporta la distribuzione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Se si desidera eseguire qualsiasi tipo di compilazione in Team Foundation Server (TFS) 2010, è necessario creare una definizione di compilazione all'interno del progetto team. Questo argomento descrive come creare una nuova definizione di compilazione in TFS e come controllare la distribuzione web come parte del processo di compilazione in Team Build.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione e distribuzione è controllato da due file di progetto&#x2014;uno contenente le istruzioni di compilazione che si applicano a tutti gli ambienti di destinazione e quella che contiene impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

Una definizione di compilazione è il meccanismo che consente di controllare come e quando si verificano le compilazioni di progetti team in TFS. Ogni definizione di compilazione specifica:

- Gli elementi da compilare, ad esempio i file della soluzione Visual Studio o i file di progetto personalizzati di Microsoft Build Engine (MSBuild).
- I criteri che determinano quando deve essere una compilazione posizionare, analogamente ai trigger manuali, integrazione continua (CI), oppure archiviazioni.
- Posizione alla quale Team Build deve inviare output di compilazione, inclusi gli elementi di distribuzione, ad esempio pacchetti web e script di database.
- La quantità di tempo che deve essere mantenuto ogni compilazione.
- Diversi altri parametri del processo di compilazione.

> [!NOTE]
> Per altre informazioni sulle definizioni di compilazione, vedere [definire il processo di compilazione](https://msdn.microsoft.com/library/ms181715.aspx).


In questo argomento illustrerà come creare una definizione di compilazione che usa integrazione continua, in modo che viene attivata una compilazione quando uno sviluppatore archivia il nuovo contenuto. Se la compilazione ha esito positivo, il servizio di compilazione esegue un file di progetto personalizzati per distribuire la soluzione in un ambiente di test.

Quando si attiva una compilazione, queste azioni devono essere eseguite:

- In primo luogo, Team Build, è necessario compilare la soluzione. Team Build come parte di questo processo, verrà richiamato la Pipeline di pubblicazione di Web (WPP) per generare i pacchetti di distribuzione web per ognuno dei progetti dell'applicazione web nella soluzione. Compilazione di team verrà eseguiti anche tutti gli unit test associati alla soluzione.
- Se ha esito negativo della compilazione della soluzione, Team Build deve intraprendere alcuna azione ulteriore. Errori negli unit test devono essere considerate come un errore di compilazione.
- Se ha esito positivo della compilazione della soluzione, è consigliabile eseguire il file di progetto personalizzato che controlla la distribuzione della soluzione Team Build. Come parte di questo processo, Team Build richiameranno lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web) per installare le applicazioni web inclusa nel pacchetto sul server web di destinazione e verrà richiamato l'utilità VSDBCMD.exe per eseguire la creazione del database script nei server di database di destinazione.

Ciò illustra il processo:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

Il [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) soluzione di esempio include un file di progetto MSBuild personalizzato *Publish.proj*, che è possibile eseguire da MSBuild o Team Build. Come descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), questo file di progetto definisce la logica che consente di distribuire i pacchetti web e i database in un ambiente di destinazione. Il file include la logica che omette la compilazione e il processo di creazione del pacchetto se è in esecuzione in Team Build, lasciando solo le attività di distribuzione da eseguire. Questo perché quando si automatizza la distribuzione in questo modo, in genere è opportuno per assicurarsi che la soluzione venga compilata correttamente e passa tutti gli unit test prima di avviare il processo di distribuzione.

Nella sezione successiva illustra come implementare questo processo creando una nuova definizione di compilazione.

> [!NOTE]
> Questa procedura&#x2014;in cui automatizzata un singolo processo di build, test e distribuisce una soluzione&#x2014;è probabile che sia più adatto alla distribuzione in ambienti di test. Per gli ambienti di staging e produzione si è molto più probabile che vuole distribuire il contenuto da una compilazione precedente che già verificato e convalidato in un ambiente di test. Questo approccio è descritto nell'argomento successivo [distribuzione di una compilazione specifica](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Chi esegue questa procedura?

In genere, un amministratore di TFS esegue questa procedura. In alcuni casi, il responsabile del team di sviluppo può assumersi la responsabilità per la raccolta di progetti team in TFS. Per creare una nuova definizione di compilazione, è necessario essere un membro del **Project Collection Build Administrators** gruppo per la raccolta di progetti team che contiene la soluzione.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Creare una definizione di compilazione per integrazione continua e distribuzione

La procedura seguente descrive come creare una definizione di compilazione integrazione continua attiva. Se la compilazione ha esito positivo, la soluzione viene distribuita utilizzando la logica in un file di progetto MSBuild personalizzato.

**Per creare una definizione di compilazione per integrazione continua e distribuzione**

1. In Visual Studio 2010, nelle **Team Explorer** finestra, espandere il nodo del progetto team, fare doppio clic su **Compila**, quindi fare clic su **nuova definizione di compilazione**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Nel **generali** scheda, assegnare un nome definizione di compilazione (ad esempio **DeployToTest**) e una descrizione facoltativa.
3. Nel **Trigger** scheda, selezionare i criteri in cui si vuole attivare una nuova compilazione. Ad esempio, se si desidera compilare la soluzione e distribuire nell'ambiente di test ogni volta che uno sviluppatore archivia nuovo codice, selezionare **Continuous Integration**.
4. Nel **impostazioni predefinite compilazione** nella scheda il **copia la cartella di rilascio seguente output di compilazione** , digitare il percorso Universal Naming Convention (UNC) della cartella di ricezione (ad esempio,  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Questo percorso di rilascio archivia diverse compilazioni, a seconda dei criteri di conservazione che è configurare. Quando si desidera pubblicare gli elementi di distribuzione da una compilazione specifica in un ambiente di gestione temporanea o produzione, si tratta in cui è possibile trovarli.
5. Nel **processo** nella scheda il **file di processo di compilazione** lasciare nell'elenco a discesa **DefaultTemplate. XAML** selezionato. Questo è uno dei modelli di processo di compilazione predefiniti che vengono aggiunte a tutti i nuovi progetti team.
6. Nel **parametri processo di compilazione** , fare clic nel **elementi da compilare** di riga e quindi fare clic sui **puntini di sospensione** pulsante.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. Nel **elementi da compilare** finestra di dialogo, fare clic su **Add**.
8. Passare al percorso del file di soluzione e quindi fare clic su **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. Nel **elementi da compilare** finestra di dialogo, fare clic su **Add**.
10. Nel **elementi di tipo** elenco a discesa, seleziona **i file di progetto MSBuild**.
11. Passare al percorso del file di progetto personalizzati con cui controllano il processo di distribuzione, selezionare il file e quindi fare clic su **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. Il **elementi da compilare** nella finestra di dialogo vengono visualizzati due elementi. Fare clic su **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Nel **processo** nella scheda il **parametri processo di compilazione** tabella, quindi espandere il **avanzate** sezione.
14. Nel **argomenti MSBuild** riga, aggiungere eventuali argomenti della riga di comando di MSBuild che *entrambi* gli elementi di compilazione richiede. Nello scenario di soluzione Contact Manager, sono necessari questi argomenti:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. In questo esempio:

    1. Il **DeployOnBuild = true** e **DeployTarget = pacchetto** quando si compila la soluzione Contact Manager, sono necessari argomenti. Indica a MSBuild per creare web i pacchetti di distribuzione dopo la compilazione di ogni progetto di applicazione web, come descritto in [compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. Il **TargetEnvPropsFile** argomento è obbligatorio quando si compila il *Publish.proj* file. Questa proprietà indica il percorso del file di configurazione specifiche dell'ambiente, come descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Nel **criteri di conservazione** , configurare il numero di compilazioni di ogni tipo da conservare in base alle esigenze.
17. Fare clic su **Salva**.

## <a name="queue-a-build"></a>Accodare una compilazione

A questo punto, si have creato almeno una nuova definizione di compilazione. Il processo di compilazione che è definita ora verrà eseguito in base a trigger specificato nella definizione di compilazione.

Se è stata configurata la definizione di compilazione per l'uso degli elementi di configurazione, è possibile testare la definizione di compilazione in due modi:

- Archiviare il progetto team per attivare una compilazione automatica di alcuni contenuti.
- Accodare manualmente una compilazione.

**Per accodare manualmente una compilazione**

1. Nel **Team Explorer** finestra, fare doppio clic la definizione di compilazione e quindi fare clic su **Accoda nuova compilazione**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. Nel **Accoda compilazione** della finestra di dialogo esaminare le proprietà di compilazione e quindi fare clic su **coda**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Per esaminare lo stato di avanzamento e il risultato di una compilazione&#x2014;indipendentemente dal fatto che è stato attivato manualmente o automaticamente&#x2014;fare doppio clic sulla definizione di compilazione nel **Team Explorer** finestra. Verrà aperta una **Build Explorer** scheda.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

A questo punto, è possibile risolvere errori di compilazione. Se si fa doppio clic su una singola build, è possibile visualizzare le informazioni di riepilogo e fare clic per visualizzare file di log dettagliati.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

È possibile utilizzare queste informazioni per risolvere i problemi di errori di compilazione e risolvere eventuali problemi prima di tentare un'altra compilazione.

> [!NOTE]
> Le compilazioni che eseguono la logica di distribuzione sono potrebbe non riuscire fino a quando non si hanno concesso le autorizzazioni necessarie nell'ambiente di destinazione a server di compilazione. Per altre informazioni, vedere [configurazione delle autorizzazioni per la distribuzione di Team Build](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Monitorare il processo di compilazione

TFS fornisce un'ampia gamma di funzionalità che consentono di monitorare il processo di compilazione. Ad esempio, TFS può inviare un messaggio di posta elettronica o visualizzare gli avvisi nell'area di notifica della barra delle applicazioni quando è stata completata una compilazione. Per altre informazioni, vedere [eseguire e monitorare le compilazioni](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Conclusione

In questo argomento descrive come creare una definizione di compilazione in TFS. La definizione di compilazione è configurata per l'integrazione continua, in modo che il processo di compilazione viene eseguita ogni volta che uno sviluppatore archivia nel contenuto per il progetto team. La definizione di compilazione esegue un file di progetto MSBuild personalizzato per distribuire i pacchetti web e gli script di database in un ambiente di server di destinazione.

Affinché una distribuzione automatizzata abbia esito positivo come parte di un processo di compilazione, è necessario concedere le autorizzazioni appropriate all'account del servizio di compilazione nel server web di destinazione e il server di database di destinazione. L'argomento finale in questa esercitazione [configurazione delle autorizzazioni per la distribuzione di Team Build](configuring-permissions-for-team-build-deployment.md), viene descritto come identificare e configurare le autorizzazioni necessarie per la distribuzione automatizzata da un server Team Build.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sulla creazione di definizioni di compilazione, vedere [creare una definizione di compilazione di base](https://msdn.microsoft.com/library/ms181716.aspx) e [definiscono il processo di compilazione](https://msdn.microsoft.com/library/ms181715.aspx). Per altre informazioni sull'accodamento di compilazioni, vedere [accodare una compilazione](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Precedente](configuring-a-tfs-build-server-for-web-deployment.md)
> [Successivo](deploying-a-specific-build.md)
