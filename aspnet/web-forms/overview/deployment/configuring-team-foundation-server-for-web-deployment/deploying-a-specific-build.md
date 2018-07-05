---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Distribuzione di una compilazione specifica | Microsoft Docs
author: jrjlee
description: In questo argomento descrive come distribuire i pacchetti web e script di database da una specifica build precedente a una nuova destinazione, ad esempio un enviro gestione temporanea o produzione...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 312cd6f42463722b76996a3a848102946e0ca265
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832468"
---
<a name="deploying-a-specific-build"></a>Distribuzione di una compilazione specifica
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento descrive come distribuire i pacchetti web e script di database da una specifica build precedente a una nuova destinazione, ad esempio un ambiente di gestione temporanea o produzione.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione e distribuzione è controllato da due file di progetto&#x2014;uno contenente le istruzioni di compilazione che si applicano a tutti gli ambienti di destinazione e quella che contiene impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

Fino ad ora, negli argomenti di questa serie di esercitazioni sono incentrate sulla compilazione del pacchetto e distribuire applicazioni web e i database come parte di un singolo passaggio o processo automatizzato. In alcuni scenari comuni, tuttavia, sarà necessario selezionare le risorse distribuite da un elenco di compilazioni in una cartella di ricezione. In altre parole, la build più recente potrebbe non essere la compilazione da distribuire.

Si consideri lo scenario di integrazione continua (CI) descritto nell'argomento precedente [creazione di una compilazione che supporta la distribuzione della definizione](creating-a-build-definition-that-supports-deployment.md). È stata creata una definizione di compilazione in Team Foundation Server (TFS) 2010. Ogni volta che uno sviluppatore archivia codice in TFS, Team Build verrà compila il codice, creare pacchetti web e script di database come parte del processo di compilazione, eseguire tutti gli unit test e distribuire le risorse in un ambiente di test. A seconda dei criteri di conservazione configurato al momento della creazione della definizione di compilazione, TFS manterrà un certo numero di compilazioni precedenti.

![](deploying-a-specific-build/_static/image1.png)

A questo punto, si supponga aver eseguito la verifica si basa su uno di questi test di convalida nell'ambiente di test e si è pronti per distribuire l'applicazione in un ambiente di staging. Nel frattempo, gli sviluppatori possono avere controllati nel nuovo codice. Non si vuole ricompilare la soluzione e distribuire l'ambiente di gestione temporanea e non si desidera distribuire la build più recente per l'ambiente di gestione temporanea. Al contrario, si vuole distribuire la compilazione specifica che è stato verificato e convalidato nel server di prova.

A tale scopo, è necessario indicare dove trovare i pacchetti web e gli script di database che ha generata una compilazione specifica di Microsoft Build Engine (MSBuild).

## <a name="overriding-the-outputroot-property"></a>L'override della proprietà OutputRoot

Nel [soluzione di esempio](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), il *Publish.proj* file dichiara una proprietà denominata **OutputRoot**. Come suggerisce il nome, questa è la cartella radice che contiene tutto ciò che il processo di compilazione che genera l'errore. Nel *Publish.proj* nel file, è possibile vedere che il **OutputRoot** proprietà fa riferimento al percorso radice per tutte le risorse di distribuzione.

> [!NOTE]
> **OutputRoot** è un nome di proprietà di uso comune. File di progetto Visual c# e Visual Basic anche dichiarano questa proprietà per archiviare il percorso radice per tutti gli output di compilazione.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Se si desidera che il file di progetto per la distribuzione dei pacchetti web e script da una posizione diversa del database&#x2014;desidera che gli output di una compilazione TFS precedente&#x2014;è sufficiente eseguire l'override di **OutputRoot** proprietà. È necessario impostare il valore della proprietà nella cartella di compilazione rilevanti nel server di Team Build. Se si eseguiva MSBuild dalla riga di comando, è possibile specificare un valore per **OutputRoot** come argomento della riga di comando:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


In pratica, tuttavia, anche si ignora la **compilare** destinazione&#x2014;non ha senso nel creare la tua soluzione se non si prevede di usare gli output di compilazione. È possibile eseguire questa operazione specificando le destinazioni da eseguire dalla riga di comando:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


Tuttavia, nella maggior parte dei casi, è opportuno creare una logica di distribuzione in una definizione di compilazione TFS. Ciò consente agli utenti con il **accodare compilazioni** autorizzati ad attivare la distribuzione da qualsiasi installazione di Visual Studio con una connessione al server TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>La creazione di una definizione di compilazione per distribuire specifici compilazioni

La procedura seguente descrive come creare una definizione di compilazione che consente agli utenti di attivare le distribuzioni in un ambiente di staging con un unico comando.

In questo caso, non si desidera la definizione di compilazione per compilare effettivamente qualsiasi elemento&#x2014;è sufficiente per eseguire la logica di distribuzione nel file di progetto personalizzato. Il *Publish.proj* file include la logica condizionale che ignora la **compilazione** di destinazione se il file è in esecuzione in Team Build. A tale scopo, la valutazione integrato **BuildingInTeamBuild** proprietà, che viene impostata automaticamente **true** se si esegue il file di progetto in Team Build. Di conseguenza, è possibile ignorare il processo di compilazione e l'esecuzione del file di progetto per distribuire una compilazione esistente.

**Per creare una definizione di compilazione per attivare manualmente la distribuzione**

1. In Visual Studio 2010, nelle **Team Explorer** finestra, espandere il nodo del progetto team, fare doppio clic su **Compila**, quindi fare clic su **nuova definizione di compilazione**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Nel **generali** scheda, assegnare un nome definizione di compilazione (ad esempio **DeployToStaging**) e una descrizione facoltativa.
3. Nel **Trigger** scheda, seleziona **manuale: le archiviazioni non avviano una nuova compilazione**.
4. Nel **impostazioni predefinite compilazione** nella scheda il **copia la cartella di rilascio seguente output di compilazione** , digitare il percorso Universal Naming Convention (UNC) della cartella di ricezione (ad esempio,  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Nel **processo** nella scheda il **file di processo di compilazione** lasciare nell'elenco a discesa **DefaultTemplate. XAML** selezionato. Questo è uno dei modelli di processo di compilazione predefiniti che vengono aggiunte a tutti i nuovi progetti team.
6. Nel **parametri processo di compilazione** , fare clic nel **elementi da compilare** di riga e quindi fare clic sui **puntini di sospensione** pulsante.

    ![](deploying-a-specific-build/_static/image4.png)
7. Nel **elementi da compilare** finestra di dialogo, fare clic su **Add**.
8. Nel **elementi di tipo** elenco a discesa, seleziona **i file di progetto MSBuild**.
9. Passare al percorso del file di progetto personalizzati con cui controllano il processo di distribuzione, selezionare il file e quindi fare clic su **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. Nel **elementi da compilare** finestra di dialogo, fare clic su **OK**.
11. Nel **parametri processo di compilazione** tabella, quindi espandere il **avanzate** sezione.
12. Nel **argomenti MSBuild** righe, specificare il percorso del file di progetto specifici dell'ambiente e aggiungere un segnaposto per il percorso della cartella di compilazione:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > È necessario eseguire l'override di **OutputRoot** valore ogni volta che si accodare una compilazione. Questo aspetto è illustrato nella procedura successiva.
13. Fare clic su **Salva**.

Quando si attiva una compilazione, è necessario aggiornare il **OutputRoot** proprietà in modo da puntare alla compilazione da distribuire.

**Per distribuire una compilazione specifica da una definizione di compilazione**

1. Nel **Team Explorer** finestra, fare doppio clic la definizione di compilazione e quindi fare clic su **Accoda nuova compilazione**.

    ![](deploying-a-specific-build/_static/image7.png)
2. Nel **Accoda compilazione** finestra di dialogo il **parametri** scheda, quindi espandere il **avanzate** sezione.
3. Nel **argomenti MSBuild** righe, sostituire il valore della **OutputRoot** proprietà con il percorso della cartella di compilazione. Ad esempio:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Assicurarsi di includere la barra finale alla fine del percorso di cartella di compilazione.
4. Fare clic su **coda**.

Quando si accodare la compilazione, il file di progetto verrà distribuito gli script del database e web i pacchetti nella cartella di ricezione di compilazione è specificato nella **OutputRoot** proprietà.

## <a name="conclusion"></a>Conclusione

In questo argomento descrive come risorse di distribuzione, ad esempio pacchetti web e script di database di pubblicazione, da uno specifico precedente compilazione usando il modello di distribuzione file split. Illustrato come eseguire l'override di **OutputRoot** proprietà e come incorporare la logica di distribuzione in un TFS definizione di compilazione.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sulla creazione di definizioni di compilazione, vedere [creare una definizione di compilazione di base](https://msdn.microsoft.com/library/ms181716.aspx) e [definiscono il processo di compilazione](https://msdn.microsoft.com/library/ms181715.aspx). Per altre informazioni sull'accodamento di compilazioni, vedere [accodare una compilazione](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Precedente](creating-a-build-definition-that-supports-deployment.md)
> [Successivo](configuring-permissions-for-team-build-deployment.md)
