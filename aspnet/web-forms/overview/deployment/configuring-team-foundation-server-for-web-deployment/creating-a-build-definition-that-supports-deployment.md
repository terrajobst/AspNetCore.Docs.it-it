---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Creazione di una definizione di compilazione che supporta la distribuzione | Documenti Microsoft
author: jrjlee
description: "Se si desidera eseguire qualsiasi tipo di compilazione in Team Foundation Server (TFS) 2010, è necessario creare una definizione di compilazione all'interno del progetto team. Des in questo argomento..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: e5610753968328e5d0f1dba4cbbfed08480fd773
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Creazione di una definizione di compilazione che supporta la distribuzione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Se si desidera eseguire qualsiasi tipo di compilazione in Team Foundation Server (TFS) 2010, è necessario creare una definizione di compilazione all'interno del progetto team. In questo argomento viene descritto come creare una nuova definizione di compilazione in TFS e su come controllare la distribuzione web come parte del processo di compilazione in Team Build.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [soluzione responsabile contatto](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione e distribuzione è controllato da due file di progetto & #x 2014; o ne contenente le istruzioni di compilazione che si applicano a ogni ambiente di destinazione e quella contenente impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

Una definizione di compilazione è il meccanismo che consente di controllare come e quando le compilazioni vengono eseguiti per i progetti team in TFS. Ogni definizione di compilazione specifica:

- Le operazioni che si desidera compilare, ad esempio i file di soluzione di Visual Studio o file di progetto personalizzati Microsoft Build Engine (MSBuild).
- I criteri che determinano quando deve essere una build di inseriscono, ad esempio trigger manuale, integrazione continua (CI), o archiviazioni.
- Il percorso a cui Team Build devono inviare gli output di compilazione, inclusi gli elementi di distribuzione come pacchetti web e gli script di database.
- La quantità di tempo che deve essere mantenuto ogni compilazione.
- Diversi altri parametri del processo di compilazione.

> [!NOTE]
> Per ulteriori informazioni sulle definizioni di compilazione, vedere [definire il processo di compilazione](https://msdn.microsoft.com/library/ms181715.aspx).


Questo argomento viene illustrato come creare una definizione di compilazione che utilizza l'elemento di configurazione, in modo che una compilazione viene attivata quando uno sviluppatore archivia il nuovo contenuto. Se la compilazione ha esito positivo, il servizio di compilazione esegue un file di progetto personalizzati per distribuire la soluzione in un ambiente di test.

Quando si attiva una compilazione, queste azioni devono essere eseguite:

- In primo luogo, Team Build deve compilare la soluzione. Team Build come parte di questo processo, verrà richiamato Web pubblicazione Pipeline (WPP) per generare pacchetti di distribuzione web per ognuno dei progetti di applicazione web nella soluzione. Compilazione di team verrà eseguiti anche qualsiasi unit test associati alla soluzione.
- Se si verifica un errore di compilazione della soluzione, Team Build non deve intraprendere alcuna azione ulteriore. Errori di unit test devono essere considerati come un errore di compilazione.
- Se la compilazione di soluzioni ha esito positivo, Team Build deve eseguire il file di progetto personalizzato che controlla la distribuzione della soluzione. Come parte di questo processo, Team Build richiameranno lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) per installare le applicazioni in pacchetto web nei server web di destinazione e verrà richiamato l'utilità VSDBCMD.exe per eseguire la creazione del database script nei server di database di destinazione.

Ciò illustra il processo:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

Il [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) soluzione di esempio include un file di progetto MSBuild personalizzato *Publish.proj*, che è possibile eseguire da MSBuild o Team Build. Come descritto in [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), questo file di progetto definisce la logica che consente di distribuire i pacchetti web e i database in un ambiente di destinazione. Il file include la logica che consente di omettere l'edificio e il processo di creazione del pacchetto se è in esecuzione in Team Build, lasciando solo le attività di distribuzione per l'esecuzione. Questo avviene perché quando si automatizza la distribuzione in questo modo, è in genere opportuno verificare che la soluzione venga compilata correttamente e passa tutti gli unit test prima di avviare il processo di distribuzione.

La sezione successiva viene illustrato come implementare questo processo creando una nuova definizione di compilazione.

> [!NOTE]
> La procedura in & #x 2014; in cui un singolo processo automatizzato compilazioni, test e distribuisce una soluzione & #x 2014, è probabile che sia più adatto per la distribuzione in ambienti di test. Per gli ambienti di gestione temporanea e produzione si è molto più probabile che desidera distribuire il contenuto di una compilazione precedente che già verificato e convalidato in un ambiente di test. Questo approccio è descritto nell'argomento successivo, [la distribuzione di una compilazione specifica](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Che consente di eseguire questa procedura?

In genere, un amministratore TFS esegue questa procedura. In alcuni casi, un responsabile del team di sviluppo potrebbe richiedere la responsabilità per la raccolta di progetti team in TFS. Per creare una nuova definizione di compilazione, è necessario essere un membro del **Project Collection Administrators di compilare** gruppo per la raccolta di progetti team che contiene la soluzione.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Creare una definizione di compilazione per l'elemento di configurazione e distribuzione

La procedura seguente viene descritto come creare una definizione di compilazione che CI attiva. Se la compilazione ha esito positivo, la soluzione viene distribuita utilizzando la logica in un file di progetto MSBuild personalizzato.

**Per creare una definizione di compilazione per l'elemento di configurazione e distribuzione**

1. In Visual Studio 2010, nel **Team Explorer** finestra, espandere il nodo del progetto team, fare doppio clic su **compilazioni**, quindi fare clic su **nuova definizione di compilazione**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Nel **generale** scheda, assegnare un nome di definizione di compilazione (ad esempio, **DeployToTest**) e una descrizione facoltativa.
3. Nel **Trigger** scheda, selezionare i criteri in cui si vuole attivare una nuova compilazione. Ad esempio, se si desidera compilare la soluzione e distribuire l'ambiente di test ogni volta che uno sviluppatore archivia nel nuovo codice, selezionare **integrazione continua**.
4. Nel **impostazioni predefinite compilazione** nella scheda il **Copia output nella seguente cartella di rilascio di compilazione** , digitare il percorso UNC Universal Naming Convention () della cartella di ricezione (ad esempio,  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Questo percorso di rilascio archivia le compilazioni diverse, a seconda dei criteri di conservazione configurate. Quando si desidera pubblicare gli elementi di distribuzione da una compilazione specifica in un ambiente di produzione o gestione temporanea, viene visualizzato in cui è possibile trovarli.
5. Nel **processo** nella scheda il **file processo di compilazione** dall'elenco a discesa lasciare **DefaultTemplate** selezionato. Questo è uno dei modelli di processo di compilazione predefiniti che vengono aggiunte a tutti i nuovi progetti team.
6. Nel **parametri processo di compilazione** tabella, fare clic su di **elementi da compilare** riga e quindi fare clic sul **i puntini di sospensione** pulsante.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. Nel **elementi da compilare** la finestra di dialogo, fare clic su **Aggiungi**.
8. Passare al percorso del file di soluzione e quindi fare clic su **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. Nel **elementi da compilare** la finestra di dialogo, fare clic su **Aggiungi**.
10. Nel **gli elementi di tipo** elenco a discesa, seleziona **i file di progetto MSBuild**.
11. Passare al percorso del file di progetto personalizzati con cui controllano il processo di distribuzione, selezionare il file e quindi fare clic su **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. Il **elementi da compilare** la finestra di dialogo vengono visualizzati due elementi. Fare clic su **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Nel **processo** nella scheda il **parametri processo di compilazione** tabella, quindi espandere il **avanzate** sezione.
14. Nel **argomenti MSBuild** di riga, aggiungere tutti gli argomenti della riga di comando di MSBuild che *entrambi* degli elementi di compilazione richiede. Nello scenario di soluzione Contact Manager, sono necessari questi argomenti:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. In questo esempio:

    1. Il **DeployOnBuild = true** e **DeployTarget = pacchetto** sono necessari argomenti quando si compila la soluzione di gestione di contatto. Ciò indica a MSBuild per creare web pacchetti di distribuzione dopo la compilazione di ogni progetto di applicazione web, come descritto in [compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. Il **TargetEnvPropsFile** argomento è obbligatorio quando si compila il *Publish.proj* file. Questa proprietà indica il percorso del file di configurazione specifici dell'ambiente, come descritto in [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Nel **criteri di conservazione** , configurare il numero di compilazioni di ogni tipo a cui si desidera mantenere come richiesto.
17. Fare clic su **Salva**.

## <a name="queue-a-build"></a>Accodare una compilazione

A questo punto, creazione almeno una nuova definizione di compilazione. Verrà eseguito il processo di compilazione che è definito in base a trigger specificato nella definizione di compilazione.

Se hai configurato la definizione di compilazione per utilizzare l'elemento di configurazione, è possibile testare la definizione di compilazione in due modi:

- Archiviare il progetto team per attivare una compilazione automatica di alcuni contenuti.
- Accodare una compilazione manuale.

**Per accodare una compilazione manuale**

1. Nel **Team Explorer** finestra, fare doppio clic sulla definizione di compilazione e quindi fare clic su **Accoda nuova compilazione**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. Nel **Accoda compilazione** nella finestra di dialogo esaminare le proprietà di compilazione e quindi fare clic su **coda**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Per esaminare lo stato di avanzamento e il risultato di una compilazione & #x 2014; indipendentemente dal fatto che è stata attivata manualmente o automaticamente & #x 2014; fare doppio clic sulla definizione di compilazione nel **Team Explorer** finestra. Verrà aperta una **Build Explorer** scheda.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Da qui è possibile risolvere errori di compilazione. Se si fa doppio clic su una singola compilazione, è possibile visualizzare le informazioni di riepilogo e fare clic per file di log dettagliati.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

È possibile utilizzare queste informazioni per risolvere i problemi relativi a errori di compilazione e risolvere eventuali problemi prima di tentare un'altra compilazione.

> [!NOTE]
> Le compilazioni che eseguono la logica di distribuzione sono probabilmente esito negativo fino a quando non si dispone del server di compilazione tutte le autorizzazioni necessarie nell'ambiente di destinazione. Per ulteriori informazioni, vedere [configurazione delle autorizzazioni per la distribuzione di compilazione Team](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Monitorare il processo di compilazione

TFS fornisce un'ampia gamma di funzionalità che consentono di monitorare il processo di compilazione. Ad esempio, TFS può inviare un messaggio di posta elettronica o visualizzare gli avvisi nell'area di notifica della barra delle applicazioni quando è stata completata una compilazione. Per ulteriori informazioni, vedere [eseguire e monitorare le compilazioni](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come creare una definizione di compilazione in TFS. La definizione di compilazione è configurata per l'integrazione continua, il processo di compilazione con cui viene eseguito ogni volta che uno sviluppatore archivia nel contenuto al progetto team. La definizione di compilazione esegue un file di progetto MSBuild personalizzato per distribuire pacchetti web e gli script di database in un ambiente server di destinazione.

Affinché una distribuzione automatica abbia esito positivo come parte di un processo di compilazione, è necessario concedere le autorizzazioni appropriate all'account del servizio di compilazione nel server web di destinazione e il server di database di destinazione. L'argomento finale in questa esercitazione, [configurazione delle autorizzazioni per la distribuzione di compilazione Team](configuring-permissions-for-team-build-deployment.md), viene descritto come identificare e configurare le autorizzazioni necessarie per la distribuzione automatica da un server Team Build.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla creazione di definizioni di compilazione, vedere [creare una definizione di compilazione di base](https://msdn.microsoft.com/library/ms181716.aspx) e [definire il processo di compilazione](https://msdn.microsoft.com/library/ms181715.aspx). Per informazioni aggiuntive nelle build di Accodamento messaggi, vedere [accodare una compilazione](https://msdn.microsoft.com/library/ms181722.aspx).

>[!div class="step-by-step"]
[Precedente](configuring-a-tfs-build-server-for-web-deployment.md)
[Successivo](deploying-a-specific-build.md)
