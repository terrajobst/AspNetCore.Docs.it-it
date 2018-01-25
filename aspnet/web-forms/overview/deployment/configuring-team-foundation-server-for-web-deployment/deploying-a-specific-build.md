---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Distribuzione di una compilazione specifica | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come distribuire i pacchetti web e gli script di database da una compilazione precedente specifica a una nuova destinazione, ad esempio un enviro di gestione temporanea o produzione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: be1000f0cbc2f509f5014789c2bc47ce2b12fb2f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="deploying-a-specific-build"></a>Distribuzione di una compilazione specifica
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come distribuire i pacchetti web e gli script di database da una compilazione precedente specifica a una nuova destinazione, ad esempio un ambiente di gestione temporanea o produzione.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [soluzione responsabile contatto](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione e distribuzione è controllato da due file di progetto & #x 2014; o ne contenente le istruzioni di compilazione che si applicano a ogni ambiente di destinazione e quella contenente impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

Fino ad ora, negli argomenti di questa esercitazione set sono dedicata alle modalità di compilazione, del pacchetto e distribuire applicazioni web e i database come parte di un singolo passaggio o processo automatizzato. Tuttavia, in alcuni scenari comuni, è opportuno selezionare le risorse distribuite da un elenco di compilazioni in una cartella di ricezione. In altre parole, la build più recente potrebbe non essere la compilazione in cui che si desidera distribuire.

Si consideri lo scenario di integrazione continua (CI) descritto nella sezione precedente, [la creazione di una compilazione che supporta la distribuzione della definizione](creating-a-build-definition-that-supports-deployment.md). È stata creata una definizione di compilazione in Team Foundation Server (TFS) 2010. Ogni volta che uno sviluppatore esegua codice TFS, Team Build verrà compilare il codice, creare pacchetti web e gli script di database come parte del processo di compilazione, esecuzione di unit test e distribuire le risorse in un ambiente di test. A seconda dei criteri di conservazione configurato al momento della creazione della definizione di compilazione, TFS manterrà un certo numero di build precedente.

![](deploying-a-specific-build/_static/image1.png)

Si supponga ora che è stata eseguita la verifica e convalida dei test in uno di questi compilazioni nell'ambiente di test e si è pronti per distribuire l'applicazione in un ambiente di gestione temporanea. Nel frattempo, gli sviluppatori possono hanno archiviato nel nuovo codice. Non si desidera ricompilare la soluzione e distribuire l'ambiente di gestione temporanea e non si desidera distribuire la build più recente per l'ambiente di gestione temporanea. Al contrario, si desidera distribuire la compilazione specifica che è stato verificato e convalidato nel server di prova.

A tale scopo, è necessario indicare dove trovare i pacchetti web e gli script di database che ha generata una compilazione specifica di Microsoft Build Engine (MSBuild).

## <a name="overriding-the-outputroot-property"></a>L'override della proprietà OutputRoot

Nel [soluzione di esempio](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* file dichiara una proprietà denominata **OutputRoot**. Come suggerisce il nome, questa è la cartella radice che contiene tutti gli elementi che genera il processo di compilazione. Nel *Publish.proj* file, è possibile vedere che il **OutputRoot** proprietà fa riferimento al percorso radice per tutte le risorse di distribuzione.

> [!NOTE]
> **OutputRoot** è un nome di proprietà di uso comune. File di progetto Visual c# e Visual Basic anche dichiarano questa proprietà per archiviare il percorso radice per tutti gli output di compilazione.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Se si desidera che il file di progetto per distribuire pacchetti web e gli script di database da un percorso diverso & #x 2014; ad esempio l'output di un precedente compilazione TFS & #x 2014; è sufficiente eseguire l'override di **OutputRoot** proprietà. Impostare il valore della proprietà nella cartella di compilazione rilevanti nel server di Team Build. Se si stesse eseguendo MSBuild dalla riga di comando, è possibile specificare un valore per **OutputRoot** come argomento della riga di comando:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


In pratica, tuttavia, è inoltre desidera ignorare il **compilare** destinazione & #x 2014; è presente alcun punto nella creazione di una soluzione se non si prevede di utilizzare gli output di compilazione. È possibile eseguire questa operazione specificando le destinazioni da eseguire dalla riga di comando:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


Tuttavia, nella maggior parte dei casi, è opportuno creare una logica di distribuzione in una definizione di compilazione TFS. Ciò consente agli utenti con il **accodare compilazioni** dell'autorizzazione per attivare la distribuzione da qualsiasi installazione di Visual Studio con una connessione al server TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>La creazione di una definizione di compilazione per distribuire specifici compilazioni

La procedura seguente viene descritto come creare una definizione di compilazione che consente agli utenti per le distribuzioni di trigger in un ambiente di gestione temporanea con un unico comando.

In questo caso, non si desidera la definizione di compilazione per creare qualsiasi elemento & #x 2014; si desidera che venga eseguita la logica di distribuzione nel file di progetto personalizzati. Il *Publish.proj* file include la logica condizionale che ignora il **compilare** di destinazione se il file è in esecuzione in Team Build. Questa operazione viene eseguita la valutazione incorporati **BuildingInTeamBuild** proprietà, viene impostata automaticamente su **true** se si esegue il file di progetto in Team Build. Di conseguenza, è possibile saltare il processo di compilazione e l'esecuzione del file di progetto per distribuire una compilazione esistente.

**Per creare una definizione di compilazione per attivare manualmente la distribuzione**

1. In Visual Studio 2010, nel **Team Explorer** finestra, espandere il nodo del progetto team, fare doppio clic su **compilazioni**, quindi fare clic su **nuova definizione di compilazione**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Nel **generale** scheda, assegnare un nome di definizione di compilazione (ad esempio, **DeployToStaging**) e una descrizione facoltativa.
3. Nel **Trigger** , selezionare **manuale: archiviazioni non avviano una nuova compilazione**.
4. Nel **impostazioni predefinite compilazione** nella scheda il **Copia output nella seguente cartella di rilascio di compilazione** , digitare il percorso UNC Universal Naming Convention () della cartella di ricezione (ad esempio,  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Nel **processo** nella scheda il **file processo di compilazione** dall'elenco a discesa lasciare **DefaultTemplate** selezionato. Questo è uno dei modelli di processo di compilazione predefiniti che vengono aggiunte a tutti i nuovi progetti team.
6. Nel **parametri processo di compilazione** tabella, fare clic su di **elementi da compilare** riga e quindi fare clic sul **i puntini di sospensione** pulsante.

    ![](deploying-a-specific-build/_static/image4.png)
7. Nel **elementi da compilare** la finestra di dialogo, fare clic su **Aggiungi**.
8. Nel **gli elementi di tipo** elenco a discesa, seleziona **i file di progetto MSBuild**.
9. Passare al percorso del file di progetto personalizzati con cui controllano il processo di distribuzione, selezionare il file e quindi fare clic su **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. Nel **elementi da compilare** la finestra di dialogo, fare clic su **OK**.
11. Nel **parametri processo di compilazione** tabella, quindi espandere il **avanzate** sezione.
12. Nel **argomenti MSBuild** righe, specificare il percorso del file di progetto specifici dell'ambiente e aggiungere un segnaposto per il percorso della cartella di compilazione:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > È necessario eseguire l'override di **OutputRoot** valore ogni volta che si accoda una compilazione. Questa attività viene descritta la procedura descritta di seguito.
13. Fare clic su **Salva**.

Quando si attiva una compilazione, è necessario aggiornare il **OutputRoot** proprietà in modo che punti alla compilazione di cui si desidera distribuire.

**Per distribuire una compilazione specifica da una definizione di compilazione**

1. Nel **Team Explorer** finestra, fare doppio clic sulla definizione di compilazione e quindi fare clic su **Accoda nuova compilazione**.

    ![](deploying-a-specific-build/_static/image7.png)
2. Nel **Accoda compilazione** della finestra di dialogo di **parametri** scheda, espandere il **avanzate** sezione.
3. Nel **argomenti MSBuild** di riga, sostituire il valore della **OutputRoot** proprietà con il percorso della cartella di compilazione. Ad esempio:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Assicurarsi di includere una barra finale alla fine del percorso della cartella di compilazione.
4. Fare clic su **coda**.

Quando si mette in coda la compilazione, il file di progetto verrà distribuito gli script del database e web pacchetti dalla cartella di rilascio di compilazione specificato nel **OutputRoot** proprietà.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come pubblicare le risorse di distribuzione, ad esempio pacchetti web e gli script di database, si consiglia di compilazione utilizzando il modello di distribuzione split file progetto da uno specifico precedente. Illustrato come eseguire l'override di **OutputRoot** proprietà e come incorporare la logica di distribuzione in un TFS definizione di compilazione.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla creazione di definizioni di compilazione, vedere [creare una definizione di compilazione di base](https://msdn.microsoft.com/library/ms181716.aspx) e [definire il processo di compilazione](https://msdn.microsoft.com/library/ms181715.aspx). Per informazioni aggiuntive nelle build di Accodamento messaggi, vedere [accodare una compilazione](https://msdn.microsoft.com/library/ms181722.aspx).

>[!div class="step-by-step"]
[Precedente](creating-a-build-definition-that-supports-deployment.md)
[Successivo](configuring-permissions-for-team-build-deployment.md)
