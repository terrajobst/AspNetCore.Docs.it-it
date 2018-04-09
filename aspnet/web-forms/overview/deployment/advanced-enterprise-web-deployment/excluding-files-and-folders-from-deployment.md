---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Esclusione di file e cartelle da distribuzione | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come è possibile escludere file e cartelle da un pacchetto di distribuzione web quando si compila e un progetto di applicazione web del pacchetto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: c435448bf057bbef9127d66ffda24a07729f2322
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="excluding-files-and-folders-from-deployment"></a>Esclusione dalla distribuzione di file e cartelle
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come è possibile escludere file e cartelle da un pacchetto di distribuzione web quando si compila e un progetto di applicazione web del pacchetto.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [Contact Manager soluzione](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente uno istruzioni che si applicano a ogni ambiente di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifici dell'ambiente di compilazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="overview"></a>Panoramica

Quando si compila un progetto di applicazione web in Visual Studio 2010, la Pipeline di pubblicazione sul Web (WPP) consente di estendere il processo di compilazione per creare il pacchetto dell'applicazione web compilata in un pacchetto distribuibile di web. È quindi possibile utilizzare lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) per distribuire il pacchetto web a un server web IIS remoto oppure importare il pacchetto web manualmente tramite Gestione IIS. Questo processo di creazione del pacchetto è illustrato in [compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

In che modo è possibile controllare cosa ottiene incluso nel web pacchetto? Le impostazioni di progetto in Visual Studio, tramite il file di progetto sottostante, forniscono sufficiente controllo per molti scenari. Tuttavia, in alcuni casi è personalizzare il contenuto del pacchetto di web agli ambienti di destinazione specifico. Potrebbe ad esempio, si desidera includere una cartella per i file di log quando si distribuire l'applicazione in un ambiente di test ma escludere la cartella quando si distribuisce l'applicazione in un ambiente di gestione temporanea o produzione. Questo argomento viene illustrato come eseguire questa operazione.

## <a name="what-gets-included-by-default"></a>Cosa ottiene incluso per impostazione predefinita?

Quando si configurano le proprietà del progetto applicazione web in Visual Studio, il **gli elementi da distribuire** elenco il **pubblicazione/creazione pacchetto Web** pagina consente di specificare ciò che si desidera includere nella distribuzione web pacchetto. Impostazione predefinita è **solo i file necessari per eseguire questa applicazione**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Quando si sceglie **solo i file necessari per eseguire questa applicazione**, WPP tenterà di determinare quali file devono essere aggiunti al pacchetto di web. vale a dire:

- Output di una compilazione per il progetto.
- Tutti i file contrassegnati con un'azione di compilazione **contenuto**.

> [!NOTE]
> In questo file è contenuta la logica che determina i file da includere:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Esclusione di cartelle e file specifici

In alcuni casi, è opportuno utilizzare un controllo più preciso in cui vengono distribuite i file e cartelle. Se si è certi di quali file che si desidera escludere avanti di tempo e l'esclusione si applica a tutti gli ambienti di destinazione, è possibile impostare semplicemente il **azione di compilazione** di ogni file **Nessuno**.

**Per escludere i file specifici dalla distribuzione**

1. Nel **Esplora** finestra, il file e quindi fare clic su **proprietà**.
2. Nel **proprietà** finestra, nel **azione di compilazione** riga, selezionare **Nessuno**.

Tuttavia, questo approccio non è sempre pratico. Ad esempio, è possibile variare i file e cartelle sono incluse in base l'ambiente di destinazione e dall'esterno di Visual Studio. Ad esempio, nella soluzione di esempio Contact Manager, esaminare il contenuto del progetto ContactManager.Mvc:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- La cartella interna contiene alcuni script SQL che lo sviluppatore utilizza per creare, eliminare e popolare il database locali a scopo di sviluppo. Niente in questa cartella deve essere distribuiti in un ambiente di gestione temporanea o produzione.
- La cartella di script contiene più file JavaScript. Molti di questi file sono esclusivamente per supportare il debug o fornire IntelliSense in Visual Studio. Alcuni di questi file non devono essere distribuiti in ambienti di produzione o gestione temporanea. Tuttavia, desiderato per la distribuzione in un ambiente di test per sviluppatori per facilitare la risoluzione dei problemi.

Anche se è Impossibile modificare i file di progetto per escludere le cartelle e file specifici, è un modo più semplice. WPP include un meccanismo per cartelle e file esclusi dalla compilazione di elenchi di elementi denominati **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles**. È possibile estendere questo meccanismo aggiungendo elementi personalizzati a tali elenchi. A tale scopo, è necessario completare questi passaggi di alto livello:

1. Creare un file di progetto personalizzato denominato *.wpp.targets [nome progetto]* nella stessa cartella del file di progetto.

    > [!NOTE]
    > Il *. wpp.targets* file deve essere inserito nella stessa cartella file di progetto applicazione web&#x2014;, ad esempio, *ContactManager.Mvc.csproj*&#x2014;anziché nella stessa cartella come qualsiasi personalizzato file di progetto utilizzati per controllare il processo di compilazione e distribuzione.
2. Nel *. wpp.targets* file, aggiungere un **ItemGroup** elemento.
3. Nel **ItemGroup** elemento, aggiungere **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles** elementi da escludere specifici file e cartelle in base alle esigenze.

Questa è la struttura di base di questo *. wpp.targets* file:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Si noti che ogni elemento include un elemento di metadati denominato **FromTarget**. Si tratta di un valore facoltativo che non influiscono sul processo di compilazione. semplicemente serve a indicare i motivi per cui sono stati omessi cartelle o file specifici se un utente esamina i log di compilazione.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Esclusione di file e cartelle da un pacchetto Web

La procedura successiva viene illustrato come aggiungere un *. wpp.targets* file in un progetto di applicazione web e come utilizzare il file da escludere specifici file e cartelle dal pacchetto web quando si compila il progetto.

**Per escludere file e cartelle da un pacchetto di distribuzione web**

1. Aprire la soluzione in Visual Studio 2010.
2. Nel **Esplora** finestra, fare doppio clic su un nodo di progetto applicazione web (ad esempio, **ContactManager.Mvc**), scegliere **Aggiungi**e quindi fare clic su **Nuovo elemento**.
3. Nel **Aggiungi nuovo elemento** la finestra di dialogo, seleziona il **File XML** modello.
4. Nel **nome** , digitare *[nome progetto] * * *.wpp.targets** (ad esempio, **ContactManager.Mvc.wpp.targets**), quindi fare clic su **Aggiungi**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Se si aggiunge un nuovo elemento al nodo radice di un progetto, il file viene creato nella stessa cartella del file di progetto. Per verificare, aprire la cartella in Esplora risorse.
5. Nel file, aggiungere un **progetto** elemento e un **ItemGroup** elemento:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Se si desidera escludere le cartelle dal pacchetto web, aggiungere un **ExcludeFromPackageFolders** elemento per il **ItemGroup** elemento:

   1. Nel **Include** attributo, fornire un elenco delimitato da punto e virgola delle cartelle che si desidera escludere.
   2. Nel **FromTarget** elemento dei metadati, fornire un valore significativo per indicare perché le cartelle esclusi, come il nome e il *. wpp.targets* file.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Se si desidera escludere i file dal pacchetto web, aggiungere un **ExcludeFromPackageFiles** elemento per il **ItemGroup** elemento:

   1. Nel **Include** attributo, fornire un elenco delimitato da punto e virgola dei file che si desidera escludere.
   2. Nel **FromTarget** elemento dei metadati, fornire un valore significativo per indicare perché i file esclusi, come il nome e il *. wpp.targets* file.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. Il *.wpp.targets [nome progetto]* file dovrebbe ora essere simile alla seguente:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Salvare e chiudere il *.wpp.targets [nome progetto]* file.

Alla successiva esecuzione del pacchetto e di compilazione progetto applicazione web, WPP consentirà di rilevare automaticamente il *. wpp.targets* file. Tutti i file e cartelle specificate non essere inclusi nel pacchetto di web.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come escludere file e cartelle specifici quando si compila un pacchetto web, tramite la creazione di un oggetto personalizzato *. wpp.targets* file nella stessa cartella del file di progetto applicazione web.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sull'utilizzo dei file di progetto di Microsoft Build Engine (MSBuild) personalizzati per controllare il processo di distribuzione, vedere [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Per ulteriori informazioni sulla creazione di pacchetti e processo di distribuzione, vedere [compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [configurazione dei parametri per la distribuzione di pacchetto Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), e [ Distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Precedente](deploying-membership-databases-to-enterprise-environments.md)
> [Successivo](taking-web-applications-offline-with-web-deploy.md)
