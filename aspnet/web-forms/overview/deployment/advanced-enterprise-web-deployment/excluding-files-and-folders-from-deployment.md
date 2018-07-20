---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Esclusione file e cartelle dalla distribuzione | Microsoft Docs
author: jrjlee
description: Questo argomento descrive come è possibile escludere file e cartelle da un pacchetto di distribuzione web quando si compila e creare un pacchetto un progetto di applicazione web.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: 33eecfea86842a93eac705e7823f1eba9519670e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816678"
---
<a name="excluding-files-and-folders-from-deployment"></a>Esclusione file e cartelle dalla distribuzione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento descrive come è possibile escludere file e cartelle da un pacchetto di distribuzione web quando si compila e creare un pacchetto un progetto di applicazione web.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="overview"></a>Panoramica

Quando si compila un progetto di applicazione web in Visual Studio 2010, la Pipeline di pubblicazione sul Web (WPP) consente di estendere questo processo di compilazione, creazione del pacchetto di applicazione web compilata in un pacchetto distribuibile dal web. È quindi possibile usare lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web) per distribuire il pacchetto web a un server web IIS remoto oppure importare il pacchetto web manualmente tramite Gestione IIS. Questo processo di creazione del pacchetto è illustrato in [compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

In che modo è possibile controllare ciò che vengono inclusi nel web pacchetto? Le impostazioni del progetto in Visual Studio, tramite il file di progetto sottostante, forniscono sufficiente controllo per molti scenari. Tuttavia, in alcuni casi è possibile personalizzare il contenuto del pacchetto di web agli ambienti di destinazione specifico. Potrebbe ad esempio, si desidera includere una cartella per i file di log quando si distribuisce l'applicazione in un ambiente di test, ma escludere la cartella quando si distribuisce l'applicazione in un ambiente di gestione temporanea o produzione. In questo argomento illustrerà come eseguire questa operazione.

## <a name="what-gets-included-by-default"></a>Che cosa ottiene incluso per impostazione predefinita?

Quando si configurano le proprietà del progetto dell'applicazione web in Visual Studio, il **gli elementi da distribuire** elenco le **pubblicazione/creazione pacchetto Web** pagina consente di specificare cosa si desidera includere nella distribuzione web pacchetto. Per impostazione predefinita, questo è impostato **solo i file necessari per eseguire questa applicazione**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Quando si sceglie **solo i file necessari per eseguire questa applicazione**, WPP verrà effettuato un tentativo determinare quali file devono essere aggiunti al pacchetto di web. vale a dire:

- Tutti gli output di compilazione per il progetto.
- Tutti i file contrassegnati con un'azione di compilazione **contenuto**.

> [!NOTE]
> In questo file è contenuta la logica che determina i file da includere:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Esclusione di specifici file e cartelle

In alcuni casi, sarà necessario un controllo più preciso in cui vengono distribuite i file e cartelle. Se si conosce i file che si desidera escludere prima di tempo e l'esclusione viene applicata a tutti gli ambienti di destinazione, è possibile impostare semplicemente la **Build Action** di ogni file **None**.

**Escludere i file specifici dalla distribuzione**

1. Nel **Esplora soluzioni** finestra, fare doppio clic su file e quindi fare clic su **proprietà**.
2. Nel **delle proprietà** finestra, nel **azione di compilazione** riga, seleziona **None**.

Tuttavia, questo approccio non è sempre pratico. Ad esempio, è possibile variare quali file e cartelle sono incluse in base all'ambiente di destinazione e dall'esterno di Visual Studio. Ad esempio, nella soluzione di esempio Contact Manager, esaminare il contenuto del progetto ContactManager.Mvc:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- La cartella interna contiene alcuni script SQL che lo sviluppatore utilizza per creare, eliminare e popolare il database locale per scopi di sviluppo. Nulla in questa cartella deve essere distribuito in un ambiente di gestione temporanea o produzione.
- La cartella script contiene più file di JavaScript. Molti di questi file sono esclusivamente per supportare il debug o fornire IntelliSense in Visual Studio. Alcuni di questi file non devono essere distribuiti agli ambienti di gestione temporanea o produzione. Tuttavia, è possibile per la distribuzione in un ambiente di test per sviluppatori per facilitare la risoluzione dei problemi.

Anche se è Impossibile modificare i file di progetto per escludere le cartelle e file specifici, vi è un modo più semplice. WPP include un meccanismo per cartelle e file esclusi dalla compilazione di elenchi di elementi denominati **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles**. È possibile estendere questo meccanismo mediante l'aggiunta di elementi personalizzati a questi elenchi. A tale scopo, è necessario completare i passaggi generali:

1. Creare un file di progetto personalizzato denominato *[nome progetto].wpp.targets* nella stessa cartella del file di progetto.

    > [!NOTE]
    > Il *. WPP* file deve essere inserito nella stessa cartella del file di progetto dell'applicazione web&#x2014;, ad esempio, *ContactManager.Mvc.csproj*&#x2014;anziché nella stessa cartella personalizzati file di progetto usati per controllare il processo di compilazione e distribuzione.
2. Nel *. WPP* Aggiungi un' **ItemGroup** elemento.
3. Nel **ItemGroup** elemento, aggiungere **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles** elementi da escludere specifici file e cartelle in base alle esigenze.

Questa è la struttura di base di questo *. WPP* file:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Si noti che ogni elemento include un elemento dei metadati di elemento denominato **FromTarget**. Si tratta di un valore facoltativo che non influenza il processo di compilazione. è sufficiente serve a indicare il motivo per cui sono stati omessi cartelle o file specifici se un utente esamina i log di compilazione.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Esclusione di file e cartelle da un pacchetto Web

La procedura successiva verrà illustrato come aggiungere un *. WPP* file da un progetto di applicazione web e come usare il file da escludere specifici file e cartelle dal pacchetto web quando si compila il progetto.

**Per escludere file e cartelle da un pacchetto di distribuzione web**

1. Aprire la soluzione in Visual Studio 2010.
2. Nel **Esplora soluzioni** finestra, fare clic sul nodo del progetto di applicazione web (ad esempio, **ContactManager.Mvc**), scegliere **Add**e quindi fare clic su **Nuovo elemento**.
3. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona la **File XML** modello.
4. Nel **nome** , digitare *[nome progetto] * * *.wpp.targets** (ad esempio, **ContactManager.Mvc.wpp.targets**), quindi fare clic su **Aggiungi**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Se si aggiunge un nuovo elemento al nodo radice di un progetto, il file viene creato nella stessa cartella del file di progetto. È possibile verificarlo aprendo la cartella in Windows Explorer.
5. Nel file, aggiungere un **Project** elemento e un **ItemGroup** elemento:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Se si desidera escludere cartelle dal pacchetto web, aggiungere un' **ExcludeFromPackageFolders** elemento per il **ItemGroup** elemento:

   1. Nel **inclusione** dell'attributo, specificare un elenco delimitato da punto e virgola delle cartelle da escludere.
   2. Nel **FromTarget** elemento dei metadati, fornire un valore significativo per indicare il motivo per cui le cartelle esclusi, ad esempio il nome delle *. WPP* file.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Se si desidera escludere file dal pacchetto web, aggiungere un' **ExcludeFromPackageFiles** elemento per il **ItemGroup** elemento:

   1. Nel **inclusione** dell'attributo, specificare un elenco delimitato da punto e virgola dei file da escludere.
   2. Nel **FromTarget** elemento dei metadati, fornire un valore significativo per indicare il motivo per cui i file esclusi, ad esempio il nome delle *. WPP* file.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. Il *[nome progetto].wpp.targets* file dovrebbe ora essere simile:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Salvare e chiudere il *[nome progetto].wpp.targets* file.

La prossima volta che pacchetto e compilazione progetto di applicazione web, WPP rileva automaticamente le *. WPP* file. Eventuali file e cartelle specificati non essere incluso nel pacchetto di web.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come escludere file e cartelle specifici quando si compila un pacchetto web, tramite la creazione di una classe personalizzata *. WPP* file nella stessa cartella del file di progetto dell'applicazione web.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sull'utilizzo dei file di progetto di Microsoft Build Engine (MSBuild) personalizzati per controllare il processo di distribuzione, vedere [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Per altre informazioni sulla creazione di pacchetti e processo di distribuzione, vedere [compilazione e creazione di pacchetti Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [configurazione dei parametri per una distribuzione pacchetto Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), e [ Distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Precedente](deploying-membership-databases-to-enterprise-environments.md)
> [Successivo](taking-web-applications-offline-with-web-deploy.md)
