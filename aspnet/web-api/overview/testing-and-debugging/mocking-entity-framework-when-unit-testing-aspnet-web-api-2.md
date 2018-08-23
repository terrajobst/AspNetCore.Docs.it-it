---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Comportamento fittizio di Entity Framework quando gli Unit test ASP.NET Web API 2 | Microsoft Docs
author: tfitzmac
description: Questo materiale sussidiario e applicazione illustrano come creare unit test per l'applicazione API Web 2 che usa Entity Framework. Viene illustrato come modificare il...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 0bc5ab59583a2be3f889ba05d26c6cda4589057d
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838718"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Comportamento fittizio di Entity Framework quando gli Unit test ASP.NET Web API 2
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

[Download progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Questo materiale sussidiario e applicazione illustrano come creare unit test per l'applicazione API Web 2 che usa Entity Framework. Mostra come modificare il controller di scaffolding per abilitare il passaggio di un oggetto di contesto per il testing e come creare oggetti di test che funzionano con Entity Framework.
> 
> Per un'introduzione agli unit test con l'API Web ASP.NET, vedere [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
> 
> Questa esercitazione presuppone che si abbia familiarità con i concetti di base dell'API Web ASP.NET. Per un'esercitazione introduttiva, vedere [Introduzione a ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - API Web 2


## <a name="in-this-topic"></a>Contenuto dell'argomento

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Prerequisiti](#prereqs)
- [Scaricare il codice](#download)
- [Creare applicazioni con progetto unit test](#appwithunittest)
- [Creare la classe modello](#modelclass)
- [Aggiungere il controller](#controller)
- [Aggiungere l'inserimento delle dipendenze](#dependency)
- [Installare i pacchetti NuGet nel progetto di test](#testpackages)
- [Creare il contesto di test](#testcontext)
- [Creare test](#tests)
- [Eseguire i test](#runtests)

Se sono già state completate la procedura descritta in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), è possibile passare alla sezione [aggiungere il controller](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Prerequisiti

Visual Studio 2017 Community, Professional o Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Scaricare il codice

Scaricare il [progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Il progetto scaricabile include codice degli unit test per questo argomento e per il [Unit test ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) argomento.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Creare applicazioni con progetto unit test

È possibile creare un progetto unit test durante la creazione dell'applicazione o aggiungere un progetto unit test per un'applicazione esistente. Questa esercitazione illustra la creazione di un progetto unit test quando si crea l'applicazione.

Creare una nuova applicazione Web ASP.NET denominato **StoreApp**.

Nelle finestre del nuovo progetto ASP.NET, selezionare la **vuoto** modello e aggiungere le cartelle e riferimenti di base per l'API Web. Selezionare il **aggiungere gli unit test** opzione. Progetto di unit test viene automaticamente denominato **StoreApp.Tests**. È possibile mantenere questo nome.

![creare progetto unit test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Dopo aver creato l'applicazione, si vedrà che contiene due progetti: **StoreApp** e **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Creare la classe modello

Nel progetto StoreApp, aggiungere un file di classe per il **modelli** cartella denominata **Product.cs**. Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Compilare la soluzione.

<a id="controller"></a>
## <a name="add-the-controller"></a>Aggiungere il controller

Fare clic sulla cartella controller e selezionare **Add** e **nuovo elemento di scaffolding**. Selezionare Controller Web API 2 con azioni, che usa Entity Framework.

![Aggiungere nuovo controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Impostare i seguenti valori:

- Nome del controller: **ProductController**
- Classe modello: **prodotto**
- Classe del contesto dati: [selezionare **nuovo contesto dati** pulsante immette i valori visualizzati sotto]

![specificare controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Fare clic su **Add** per creare il controller con il codice generato automaticamente. Il codice include metodi per la creazione, recupero, aggiornamento ed eliminazione di istanze della classe del prodotto. Il codice seguente illustra il metodo per aggiungere un prodotto. Si noti che il metodo restituisce un'istanza di **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult è una delle nuove funzionalità di Web API 2 e semplifica lo sviluppo di unit test.

Nella sezione successiva, si personalizzerà il codice generato per facilitare il passaggio di oggetti di test al controller.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Aggiungere l'inserimento delle dipendenze

Attualmente, la classe ProductController è hardcoded per utilizzare un'istanza della classe StoreAppContext. Si userà un modello denominato inserimento delle dipendenze per modificare l'applicazione e rimuovere tale dipendenza hardcoded. Interrompendo la dipendenza, è possibile passare in un oggetto fittizio durante il test.

Fare doppio clic sui **modelli** cartella, quindi aggiungere una nuova interfaccia denominata **IStoreAppContext**.

Sostituire il codice con il seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Aprire il file StoreAppContext.cs e apportare le modifiche evidenziate di seguito. Le modifiche importanti da notare sono:

- Classe StoreAppContext implementa interfaccia IStoreAppContext
- Metodo MarkAsModified è implementato


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Aprire il file ProductController.cs. Modificare il codice esistente in modo che corrisponda il codice evidenziato. Queste modifiche, interrompere la dipendenza su StoreAppContext e consentono alle altre classi passare un oggetto diverso per la classe del contesto. Questa modifica consentirà di passare in un contesto di test durante gli unit test.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

È presente un'altra modifica da apportare nel ProductController. Nel **PutProduct** (metodo), sostituire la riga che imposta lo stato dell'entità modificata con una chiamata al metodo MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Compilare la soluzione.

A questo punto si è pronti configurare il progetto di test.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installare i pacchetti NuGet nel progetto di test

Quando si usa il modello vuoto per creare un'applicazione, il progetto di unit test (StoreApp.Tests) non include i pacchetti NuGet installati. Altri modelli, ad esempio il modello API Web, includono alcuni pacchetti NuGet nel progetto di unit test. Per questa esercitazione, è necessario includere il pacchetto di Entity Framework e il pacchetto di Microsoft ASP.NET Web API 2 Core per il progetto di test.

Fare clic sul progetto StoreApp.Tests e selezionare **Gestisci pacchetti NuGet**. È necessario selezionare il progetto StoreApp.Tests per aggiungere i pacchetti al progetto.

![gestire i pacchetti](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Dai pacchetti Online, trovare e installare il pacchetto di Entity Framework (versione 6.0 o versione successiva). Se risulta che il pacchetto di Entity Framework è già installato, si potrebbe avere selezionato il progetto StoreApp invece che al progetto StoreApp.Tests.

![aggiungere Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Trovare e installare il pacchetto di Microsoft ASP.NET Web API 2 Core.

![installare il pacchetto principale di api web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Chiudere la finestra Gestisci pacchetti NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Creare il contesto di test

Aggiungere una classe denominata **TestDbSet** al progetto di test. Questa classe funge da classe base per il set di dati di test. Sostituire il codice con il seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Aggiungere una classe denominata **TestProductDbSet** al progetto di test che contiene il codice seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Aggiungere una classe denominata **TestStoreAppContext** e sostituire il codice esistente con il codice seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Creare test

Per impostazione predefinita, il progetto di test include un file di test vuoto denominato **UnitTest1.cs**. Questo file indica gli attributi da utilizzare per creare metodi di test. Per questa esercitazione, è possibile eliminare questo file consente di aggiungere una nuova classe di test.

Aggiungere una classe denominata **TestProductController** al progetto di test. Sostituire il codice con il seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Esegui test

A questo punto si è pronti eseguire i test. Tutti il metodo che sono contrassegnati con il **TestMethod** attributo verrà testato. Dal **Test** voce di menu, eseguire i test.

![eseguire test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Aprire il **Esplora Test** finestra e osservare i risultati dei test.

![risultati test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
