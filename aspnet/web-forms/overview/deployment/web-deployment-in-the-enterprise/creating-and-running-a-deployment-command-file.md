---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Creazione ed esecuzione di una distribuzione di File di comando | Documenti Microsoft
author: jrjlee
description: "In questo argomento viene descritto come compilare un file di comando che consentirà di eseguire una distribuzione utilizzando il file di progetto di Microsoft Build Engine (MSBuild) come un unico passaggio, re..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: bc31bf55b29661816e0ca9a50b51b0abc3eb2c98
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
<a name="creating-and-running-a-deployment-command-file"></a>Creazione ed esecuzione di un File di comando di distribuzione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come compilare un file di comando che consentirà di eseguire una distribuzione utilizzando il file di progetto di Microsoft Build Engine (MSBuild) come un processo passo-passo e ripetibile.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [Contact Manager](the-contact-manager-solution.md) #x 2014; & soluzione per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [comprendere il processo di compilazione](understanding-the-build-process.md), in cui il processo di compilazione è controllato da due progetti #x 2014; & file contenente una compilare le istruzioni che si applicano a ogni ambiente di destinazione e quella contenente impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="process-overview"></a>Panoramica del processo

In questo argomento si apprenderà come creare ed eseguire un file di comando che utilizza questi file di progetto per eseguire una distribuzione ripetibile per l'ambiente di destinazione. In pratica, il file di comando è sufficiente contenere un comando di MSBuild che:

- Indica a MSBuild per eseguire l'indipendenti dall'ambiente *Publish.proj* file.
- Indica il *Publish.proj* il file contiene le impostazioni di progetto specifici dell'ambiente e dove trovare il file.

## <a name="create-an-msbuild-command"></a>Creare un comando di MSBuild

Come descritto in [comprendere il processo di compilazione](understanding-the-build-process.md), il file di progetto specifici dell'ambiente & #x 2014; ad esempio, *Env Dev.proj*& #x 2014; è progettato per essere importato nel indipendenti dall'ambiente *Publish.proj* file in fase di compilazione. Insieme, questi due file forniscono un set completo di istruzioni che indicano come compilare e distribuire la soluzione di MSBuild.

Il *Publish.proj* file Usa un **importare** elemento per importare il file di progetto specifici dell'ambiente.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Di conseguenza, quando si utilizza MSBuild.exe per compilare e distribuire la soluzione di gestione di contatto, è necessario:

- Eseguire MSBuild.exe nel *Publish.proj* file.
- Specificare il percorso del file di progetto specifici dell'ambiente, fornendo un parametro della riga di comando denominato **TargetEnvPropsFile**.

A tale scopo, il comando di MSBuild deve essere analogo al seguente:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


Da qui è un semplice passaggio per spostare in una distribuzione ripetibile, passo a passo. È necessario eseguire è sufficiente aggiungere il comando di MSBuild a un file con estensione cmd. Nella soluzione responsabile contatto, cartella di pubblicazione include un file denominato *pubblica Dev.cmd* che ciò esattamente.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> Il **/fl** commutatore indicato a MSBuild per creare un file di log denominato *msbuild.log* nella directory di lavoro in cui è stato richiamato MSBuild.exe.


Per distribuire o ridistribuire la soluzione di gestione di contatto, è sufficiente viene eseguito il *pubblica Dev.cmd* file. Quando si esegue il file, verrà MSBuild:

- Compilare tutti i progetti nella soluzione.
- Creazione di pacchetti web distribuibili per i progetti di applicazione web.
- Generare file dbschema e deploymanifest per i progetti di database.
- Distribuire i pacchetti web al server web.
- Distribuire il database al server di database.

## <a name="run-the-deployment"></a>Eseguire la distribuzione

Dopo aver creato un file di comando per l'ambiente di destinazione, è possibile completare l'intera distribuzione semplicemente eseguendo il file.

**Per distribuire la soluzione di gestione di contatto nell'ambiente di test**

1. Nella workstation di sviluppo, aprire Esplora risorse e quindi passare al percorso del *pubblica Dev.cmd* file.
2. Fare doppio clic sul file per l'esecuzione.
3. Se un **Apri File – avviso di sicurezza** viene visualizzata la finestra di dialogo, fare clic su **eseguire**.
4. Se le impostazioni di configurazione e un server di prova siano impostato correttamente, la finestra del prompt dei comandi verrà visualizzato un **compilazione completata** messaggio quando MSBuild ha terminato l'elaborazione dei file di progetto.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Se questa è la prima volta, aver distribuito la soluzione per questo ambiente, è necessario aggiungere l'account del computer server web di test per il **db\_datawriter** e **db\_datareader**ruoli per il **ContactManager** database. Questa procedura è descritta [configurare un Server di Database per la pubblicazione di distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > È sufficiente assegnare queste autorizzazioni, quando si crea il database. Per impostazione predefinita, il processo di compilazione non sarà possibile ricreare il database in ogni distribuzione & #x 2014; ma confrontare il database esistente per lo schema più recente e apportare solo le modifiche necessarie. Di conseguenza, è solo necessario eseguire il mapping di questi ruoli del database la prima volta che si distribuisce la soluzione.
6. Aprire Internet Explorer e passare all'URL dell'applicazione responsabile contatto (ad esempio, `http://testweb1:85/ContactManager/`).
7. Verificare che l'applicazione funzioni come previsto ed è in grado di aggiungere contatti.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Conclusione

Creazione di un file di comando che contiene le istruzioni di MSBuild fornisce un modo semplice e rapido della compilazione e distribuzione di una soluzione multiprogetto in un ambiente di destinazione specifico. Se si desidera distribuire ripetutamente la soluzione in più ambienti di destinazione, è possibile creare più file di comando. In ogni file di comando, il comando di MSBuild compilerà il file di progetto universale stesso, ma viene specificato un file di progetto specifici dell'ambiente diverso. Ad esempio, un file di comando per pubblicare uno sviluppatore o l'ambiente di test potrebbe contenere questo comando di MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Un file di comando per la pubblicazione in un ambiente di gestione temporanea può contenere questo comando di MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Per istruzioni su come personalizzare i file di progetto specifici dell'ambiente per ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


È anche possibile personalizzare il processo di compilazione per ogni ambiente si esegue l'override di proprietà o impostando vari altri parametri nel comando di MSBuild. Per ulteriori informazioni, vedere [alla riga di comando di MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

>[!div class="step-by-step"]
[Precedente](deploying-database-projects.md)
[Successivo](manually-installing-web-packages.md)
