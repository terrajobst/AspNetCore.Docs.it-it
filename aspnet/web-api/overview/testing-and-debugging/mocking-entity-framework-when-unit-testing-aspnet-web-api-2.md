---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Tali Entity Framework quando gli Unit test ASP.NET Web API 2 | Documenti Microsoft
author: tfitzmac
description: Questa Guida e l'applicazione viene illustrato come creare unit test per l'applicazione Web API 2 che utilizza Entity Framework. Viene illustrato come modificare il...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: abfde7edec85812de3560f4edefb110c3e374580
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152865"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Tali Entity Framework quando gli Unit test ASP.NET Web API 2
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Questa Guida e l'applicazione viene illustrato come creare unit test per l'applicazione Web API 2 che utilizza Entity Framework. Viene illustrato come modificare il controller di scaffolding per abilitare il passaggio di un oggetto di contesto per il testing e come creare gli oggetti di test che funzionano con Entity Framework.
> 
> Per un'introduzione agli unit test con ASP.NET Web API, vedere [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
> 
> In questa esercitazione si presuppone che si ha familiarità con i concetti di base dell'API Web ASP.NET. Per un'esercitazione introduttiva, vedere [Introduzione a ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2


## <a name="in-this-topic"></a>Contenuto dell'argomento

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Prerequisiti](#prereqs)
- [Scaricare il codice](#download)
- [Creare l'applicazione con progetto di unit test](#appwithunittest)
- [Creare la classe modello](#modelclass)
- [Aggiungere il controller](#controller)
- [Aggiungere l'inserimento di dipendenze](#dependency)
- [Installare i pacchetti NuGet nel progetto di test](#testpackages)
- [Creare il contesto del test](#testcontext)
- [Creazione di test](#tests)
- [Eseguire i test](#runtests)

Se sono stati già completati i passaggi descritti in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), è possibile passare alla sezione [aggiungere il controller](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Prerequisiti

Edizione di Visual Studio 2017 Community, Professional o Enterprise

<a id="download"></a>
## <a name="download-code"></a>Scaricare il codice

Scaricare il [progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Il progetto scaricabile include codice unit test per questo argomento e per il [Unit test ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) argomento.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Creare l'applicazione con progetto di unit test

È possibile creare un progetto di unit test quando si crea l'applicazione o aggiungere un progetto di unit test per un'applicazione esistente. In questa esercitazione viene illustrato come creare un progetto di unit test quando si crea l'applicazione.

Creare una nuova applicazione Web ASP.NET denominata **StoreApp**.

Nelle finestre del nuovo progetto ASP.NET, selezionare il **vuoto** modello e aggiungere cartelle e i riferimenti di base per l'API Web. Selezionare il **aggiungere unit test** opzione. Progetto di unit test viene automaticamente denominato **StoreApp.Tests**. È possibile mantenere questo nome.

![creare progetto unit test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Dopo aver creato l'applicazione, si vedrà che contiene due progetti: **StoreApp** e **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Creare la classe modello

Nel progetto StoreApp, aggiungere un file di classe per il **modelli** cartella denominata **Product.cs**. Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Compilare la soluzione.

<a id="controller"></a>
## <a name="add-the-controller"></a>Aggiungere il controller

Fare clic sulla cartella controller e selezionare **Aggiungi** e **nuovo elemento di scaffolding**. Selezionare i Controller di Web API 2 con azioni, mediante Entity Framework.

![Aggiungere nuovo controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Impostare i seguenti valori:

- Nome del controller: **ProductController**
- Classe del modello: **prodotto**
- Classe del contesto dati: [selezionare **nuovo contesto dati** pulsante immette i valori illustrati di seguito]

![specificare controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Fare clic su **Aggiungi** per creare il controller con il codice generato automaticamente. Il codice include metodi per la creazione, recupero, l'aggiornamento e l'eliminazione di istanze della classe del prodotto. Nel codice seguente viene illustrato il metodo per aggiungere un prodotto. Si noti che il metodo restituisce un'istanza di **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult è una delle nuove funzionalità di Web API 2 e semplifica lo sviluppo di unit test.

Nella sezione successiva, si personalizzerà il codice generato per facilitare il passaggio di oggetti di test al controller.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Aggiungere l'inserimento di dipendenze

Attualmente, la classe ProductController è hardcoded per utilizzare un'istanza della classe StoreAppContext. Utilizzare un modello di inserimento di dipendenze per modificare l'applicazione e rimuovere tale dipendenza hardcoded. Interrompendo la dipendenza, è possibile passare un oggetto fittizio durante il test.

Fare doppio clic su di **modelli** cartella e aggiungere una nuova interfaccia denominata **IStoreAppContext**.

Sostituire il codice con il seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Aprire il file StoreAppContext.cs e apportare le seguenti modifiche evidenziate. Le modifiche importanti da notare sono:

- Classe StoreAppContext implementa interfaccia IStoreAppContext
- Metodo MarkAsModified implementato


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Aprire il file ProductController.cs. Modificare il codice esistente in modo che corrisponda il codice evidenziato. Queste modifiche, interrompere la dipendenza su StoreAppContext e consentono alle altre classi passare un oggetto diverso per la classe del contesto. Questa modifica consentirà di passare un contesto di test durante gli unit test.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

È presente una modifica di altre che è necessario apportare ProductController. Nel **PutProduct** (metodo), sostituire con una chiamata al metodo MarkAsModified è modificata la riga che imposta lo stato dell'entità.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Compilare la soluzione.

A questo punto si è pronti configurare il progetto di test.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installare i pacchetti NuGet nel progetto di test

Quando si utilizza il modello vuoto per creare un'applicazione, il progetto di unit test (StoreApp.Tests) non include tutti i pacchetti NuGet installati. Altri modelli, ad esempio il modello API Web, includono alcuni pacchetti NuGet nel progetto di unit test. Per questa esercitazione, è necessario includere il packge Entity Framework e il pacchetto Microsoft ASP.NET Web API 2 Core per il progetto di test.

Fare clic sul progetto StoreApp.Tests e selezionare **Gestisci pacchetti NuGet**. È necessario selezionare il progetto StoreApp.Tests per aggiungere i pacchetti al progetto.

![gestire i pacchetti](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Dai pacchetti Online, trovare e installare il pacchetto EntityFramework (versione 6.0 o versione successiva). Se risulta che il pacchetto EntityFramework sia già installato, potrebbe avere selezionato il progetto StoreApp anziché il progetto StoreApp.Tests.

![aggiunta di Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Trovare e installare il pacchetto Microsoft ASP.NET Web API 2 Core.

![installare il pacchetto di web api core](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Chiudere la finestra Gestisci pacchetti NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Creare il contesto del test

Aggiungere una classe denominata **TestDbSet** al progetto di test. Questa classe funge da classe base per il set di dati di test. Sostituire il codice con il seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Aggiungere una classe denominata **TestProductDbSet** al progetto di test che contiene il codice seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Aggiungere una classe denominata **TestStoreAppContext** e sostituire il codice esistente con il codice seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Creare test

Per impostazione predefinita, il progetto di test include un file di test a vuoto denominato **UnitTest1.cs**. Questo file indica gli attributi da utilizzare per creare metodi di test. Per questa esercitazione, è possibile eliminare questo file consente di aggiungere una nuova classe di test.

Aggiungere una classe denominata **TestProductController** al progetto di test. Sostituire il codice con il seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Esegui test

A questo punto si è pronti eseguire i test. Tutti il metodo che sono contrassegnati con il **TestMethod** attributo verrà eseguito il test. Dal **Test** voce di menu, eseguire i test.

![eseguire test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Aprire il **Esplora Test** finestra e notare che i risultati dei test.

![risultati test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
