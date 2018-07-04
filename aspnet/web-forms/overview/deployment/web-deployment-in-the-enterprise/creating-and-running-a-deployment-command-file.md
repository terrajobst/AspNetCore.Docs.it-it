---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Creazione ed esecuzione di una distribuzione di File di comando | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come creare un file di comando che consentirà di eseguire una distribuzione usando i file di progetto di Microsoft Build Engine (MSBuild) come un unico passaggio, re...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: fc59920feb5eb48bc8150606b58a1ed4ba60ee92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395373"
---
<a name="creating-and-running-a-deployment-command-file"></a>Creazione ed esecuzione di un File di comando di distribuzione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento descrive come creare un file di comando che consentirà di eseguire una distribuzione con i file di progetto di Microsoft Build Engine (MSBuild) con un processo ripetibile, passo a passo.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [Contact Manager](the-contact-manager-solution.md) soluzione&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul processo di compilazione](understanding-the-build-process.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="process-overview"></a>Panoramica del processo

In questo argomento si apprenderà come creare ed eseguire un file di comando che usa questi file di progetto per eseguire una distribuzione ripetibile per l'ambiente di destinazione. In pratica, il file di comando semplicemente deve contenere un comando di MSBuild che:

- Indica a MSBuild per eseguire l'ambiente indipendente *Publish.proj* file.
- Indica la *Publish.proj* file quale file contiene le impostazioni di progetto specifici dell'ambiente e dove reperirla.

## <a name="create-an-msbuild-command"></a>Creare un comando di MSBuild

Come descritto in [informazioni sul processo di compilazione](understanding-the-build-process.md), il file di progetto specifici dell'ambiente&#x2014;, ad esempio, *Env-Dev.proj*&#x2014;è progettato per essere importato in ambiente indipendente *Publish.proj* file in fase di compilazione. Insieme, questi due file forniscono un set completo di istruzioni che indicano come compilare e distribuire la soluzione di MSBuild.

Il *Publish.proj* file usa una **importare** elemento per importare il file di progetto specifici dell'ambiente.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Di conseguenza, quando si usa MSBuild.exe per compilare e distribuire la soluzione Contact Manager, è necessario:

- Eseguire MSBuild.exe nel *Publish.proj* file.
- Specificare il percorso del file di progetto specifici dell'ambiente, fornendo un parametro della riga di comando denominato **TargetEnvPropsFile**.

A tale scopo, il comando di MSBuild sarà analogo al seguente:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


A questo punto, è un semplice passaggio per spostare in una distribuzione ripetibile, passo a passo. È necessario eseguire è sufficiente aggiungere il comando di MSBuild in un file con estensione cmd. Nella soluzione Contact Manager, la cartella di pubblicazione include un file denominato *Publish-Dev.cmd* che esegue esattamente questa.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> Il **/fl** indica a MSBuild per creare un file di log denominato *MSBuild* nella directory di lavoro in cui è stata richiamata MSBuild.exe.


Per distribuire o ridistribuire la soluzione Contact Manager, è sufficiente per eseguire operazioni viene eseguito il *Publish-Dev.cmd* file. Quando si esegue il file, MSBuild sarà:

- Compilare tutti i progetti nella soluzione.
- Generazione di pacchetti web distribuibili per i progetti di applicazione web.
- Generare file dbschema e deploymanifest per i progetti di database.
- Distribuire i pacchetti web al server web.
- Distribuire il database al server di database.

## <a name="run-the-deployment"></a>Eseguire la distribuzione

Dopo aver creato un file di comando per l'ambiente di destinazione, sarà possibile completare l'intera distribuzione eseguendo semplicemente il file.

**Per distribuire la soluzione Contact Manager nell'ambiente di test**

1. Nella workstation per gli sviluppatori, aprire Windows Explorer e quindi passare al percorso dei *Publish-Dev.cmd* file.
2. Fare doppio clic sul file per l'esecuzione.
3. Se un' **Apri File – avviso di sicurezza** verrà visualizzata la finestra di dialogo, fare clic su **eseguire**.
4. Se le impostazioni di configurazione e il server di test sono configurati correttamente, la finestra del prompt dei comandi visualizzerà un **compilazione ha avuto esito positivo** dei messaggi quando MSBuild ha terminato l'elaborazione di file di progetto.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Se questa è la prima volta che è stata distribuita la soluzione in questo ambiente, è necessario aggiungere l'account del computer server web di test per il **db\_datawriter** e **db\_datareader**ruoli su di **ContactManager** database. Questa procedura è descritta in [configurare un Server di Database per la pubblicazione di distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Devi solo assegnare queste autorizzazioni quando si crea il database. Per impostazione predefinita, il processo di compilazione non verrà ricreato il database in ogni distribuzione&#x2014;, invece verrà confrontare il database esistente per lo schema più recente e assegnare solo le modifiche necessarie. Di conseguenza, è solo necessario eseguire il mapping di questi ruoli del database alla prima che distribuzione della soluzione.
6. Aprire Internet Explorer e passare all'URL dell'applicazione Contact Manager (ad esempio, `http://testweb1:85/ContactManager/`).
7. Verificare che l'applicazione funzioni come previsto e che sia possibile aggiungere contatti.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Conclusione

Creazione di un file di comando che contiene le istruzioni di MSBuild offre un modo facile e veloce di creazione e distribuzione di una soluzione multiprogetto in un ambiente di destinazione specifico. Se si desidera distribuire ripetutamente la soluzione in più ambienti di destinazione, è possibile creare più file di comando. In ogni file di comando, il comando di MSBuild compilerà il file di progetto universale stesso, ma viene specificato un file di progetto specifici dell'ambiente diverso. Ad esempio, un file di comando per la pubblicazione a uno sviluppatore o ambiente di test potrebbe contenere questo comando di MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Un file di comando per la pubblicazione in un ambiente di staging potrebbe contenere questo comando di MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Per indicazioni su come personalizzare i file di progetto specifici dell'ambiente per ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


È anche possibile personalizzare il processo di compilazione per ogni ambiente mediante l'override delle proprietà o l'impostazione di varie altre opzioni nel comando di MSBuild. Per altre informazioni, vedere [riferimenti alla riga di comando di MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Precedente](deploying-database-projects.md)
> [Successivo](manually-installing-web-packages.md)
