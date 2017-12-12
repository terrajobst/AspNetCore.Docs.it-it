---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Unit test ASP.NET Web API 2 | Documenti Microsoft
author: tfitzmac
description: Questa Guida e l'applicazione viene illustrato come creare semplici unit test per l'applicazione Web API 2. In questa esercitazione viene illustrato come includere un proj di unit test...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 13211ee4543e17a4bfb2f83495f4041880f37df2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-aspnet-web-api-2"></a>Unit test ASP.NET Web API 2
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Questa Guida e l'applicazione viene illustrato come creare semplici unit test per l'applicazione Web API 2. In questa esercitazione viene illustrato come includere un progetto di unit test nella soluzione e scrivere metodi di test che consentono di controllare i valori restituiti da un metodo del controller.
> 
> In questa esercitazione si presuppone che si ha familiarità con i concetti di base dell'API Web ASP.NET. Per un'esercitazione introduttiva, vedere [Introduzione a ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> Gli unit test in questo argomento sono intenzionalmente limitati agli scenari di dati semplice. Per gli unit test di scenari di dati più avanzati, vedere [tali Entity Framework quando Unit test ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
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

    - [Aggiungi progetto di unit test quando si crea l'applicazione](#whencreate)
    - [Aggiungi progetto di unit test per un'applicazione esistente](#addtoexisting)
- [Configurare l'applicazione Web API 2](#setupproject)
- [Installare i pacchetti NuGet nel progetto di test](#testpackages)
- [Creazione di test](#tests)
- [Eseguire i test](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Prerequisiti

Edizione di Visual Studio 2017 Community, Professional o Enterprise

<a id="download"></a>
## <a name="download-code"></a>Scaricare il codice

Scaricare il [progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Il progetto scaricabile include codice unit test per questo argomento e per il [tali Entity Framework quando Unit test ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) argomento.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Creare l'applicazione con progetto di unit test

È possibile creare un progetto di unit test quando si crea l'applicazione o aggiungere un progetto di unit test per un'applicazione esistente. Questa esercitazione illustra entrambi i metodi per la creazione di un progetto di unit test. Per eseguire questa esercitazione, è possibile utilizzare entrambi gli approcci.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Aggiungi progetto di unit test quando si crea l'applicazione

Creare una nuova applicazione Web ASP.NET denominata **StoreApp**.

![Crea progetto](unit-testing-with-aspnet-web-api/_static/image1.png)

Nelle finestre del nuovo progetto ASP.NET, selezionare il **vuoto** modello e aggiungere cartelle e i riferimenti di base per l'API Web. Selezionare il **aggiungere unit test** opzione. Progetto di unit test viene automaticamente denominato **StoreApp.Tests**. È possibile mantenere questo nome.

![creare progetto unit test](unit-testing-with-aspnet-web-api/_static/image2.png)

Dopo aver creato l'applicazione, si noterà che contiene due progetti.

![due progetti](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Aggiungi progetto di unit test per un'applicazione esistente

Se non è stato creato il progetto di unit test quando viene creata l'applicazione, è possibile aggiungere uno in qualsiasi momento. Si supponga, ad esempio, si dispone già di un'applicazione denominata StoreApp, e si desidera aggiungere unit test. Per aggiungere un progetto di unit test, la soluzione e scegliere **Aggiungi** e **nuovo progetto**.

![aggiungere un nuovo progetto alla soluzione](unit-testing-with-aspnet-web-api/_static/image4.png)

Selezionare **Test** nel riquadro sinistro e scegliere **progetto di Unit Test** per il tipo di progetto. Denominare il progetto **StoreApp.Tests**.

![Aggiungi progetto di unit test](unit-testing-with-aspnet-web-api/_static/image5.png)

Verrà visualizzato il progetto di unit test nella soluzione.

Nel progetto di unit test, aggiungere un riferimento progetto a progetto originale.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Configurare l'applicazione Web API 2

Nel progetto StoreApp, aggiungere un file di classe per il **modelli** cartella denominata **Product.cs**. Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Compilare la soluzione.

Fare clic sulla cartella controller e selezionare **Aggiungi** e **nuovo elemento di scaffolding**. Selezionare **API Web 2 Controller - vuoto**.

![Aggiungere nuovo controller](unit-testing-with-aspnet-web-api/_static/image6.png)

Impostare il nome del controller **SimpleProductController**, fare clic su **Aggiungi**.

![specificare controller](unit-testing-with-aspnet-web-api/_static/image7.png)

Sostituire il codice esistente con quello seguente. Per semplificare questo esempio, i dati vengono archiviati in un elenco anziché in un database. L'elenco definito in questa classe rappresenta i dati di produzione. Si noti che il controller include un costruttore che accetta come parametro un elenco di oggetti di prodotto. Questo costruttore consente di passare i dati di test quando gli unit test. Il controller sono inclusi anche due **async** metodi per illustrare gli unit test di metodi asincroni. Questi metodi asincroni sono stati implementati chiamando **Task.FromResult** per ridurre al minimo estranei codice, ma in genere i metodi includono operazioni a elevato utilizzo di risorse.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Il metodo GetProduct restituisce un'istanza di **IHttpActionResult** interfaccia. IHttpActionResult è una delle nuove funzionalità di Web API 2 e semplifica lo sviluppo di unit test. Classi che implementano l'interfaccia IHttpActionResult sono disponibili nel [Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) dello spazio dei nomi. Queste classi rappresentano le possibili risposte da una richiesta di azione e che corrispondano ai codici di stato HTTP.

Compilare la soluzione.

A questo punto si è pronti configurare il progetto di test.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installare i pacchetti NuGet nel progetto di test

Quando si utilizza il modello vuoto per creare un'applicazione, il progetto di unit test (StoreApp.Tests) non include tutti i pacchetti NuGet installati. Altri modelli, ad esempio il modello API Web, includono alcuni pacchetti NuGet nel progetto di unit test. Per questa esercitazione, è necessario includere il pacchetto Microsoft ASP.NET Web API 2 Core per il progetto di test.

Fare clic sul progetto StoreApp.Tests e selezionare **Gestisci pacchetti NuGet**. È necessario selezionare il progetto StoreApp.Tests per aggiungere i pacchetti al progetto.

![gestire i pacchetti](unit-testing-with-aspnet-web-api/_static/image8.png)

Trovare e installare il pacchetto Microsoft ASP.NET Web API 2 Core.

![installare il pacchetto di web api core](unit-testing-with-aspnet-web-api/_static/image9.png)

Chiudere la finestra Gestisci pacchetti NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Creare test

Per impostazione predefinita, il progetto di test include un file di test a vuoto denominato UnitTest1. cs. Questo file indica gli attributi da utilizzare per creare metodi di test. Per gli unit test, è possibile utilizzare questo file o creare un file.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

Per questa esercitazione si creerà la propria classe di test. È possibile eliminare il file UnitTest1.cs. Aggiungere una classe denominata **TestSimpleProductController.cs**e sostituire il codice con il codice seguente.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Esegui test

A questo punto si è pronti eseguire i test. Tutti il metodo che sono contrassegnati con il **TestMethod** attributo verrà eseguito il test. Dal **Test** voce di menu, eseguire i test.

![eseguire test](unit-testing-with-aspnet-web-api/_static/image11.png)

Aprire il **Esplora Test** finestra e notare che i risultati dei test.

![risultati test](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Riepilogo

Questa esercitazione è stata completata. I dati in questa esercitazione è stato intenzionalmente semplificati di concentrarsi sull'esecuzione di unit test di condizioni. Per gli unit test di scenari di dati più avanzati, vedere [tali Entity Framework quando Unit test ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
