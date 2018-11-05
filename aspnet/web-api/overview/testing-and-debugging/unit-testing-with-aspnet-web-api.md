---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Gli unit test ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: Questo materiale sussidiario e applicazione illustrano come creare semplici unit test per l'applicazione API Web 2. Questa esercitazione Mostra come includere un proj di unit test...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 915610e6646ebe86dd8f16f290ecabd36bf7f48d
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020832"
---
<a name="unit-testing-aspnet-web-api-2"></a>Unit test ASP.NET Web API 2
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

[Download progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Questo materiale sussidiario e applicazione illustrano come creare semplici unit test per l'applicazione API Web 2. Questa esercitazione illustra come includere un progetto unit test nella soluzione e scrivere i metodi di test che consentono di controllare i valori restituiti da un metodo del controller.
>
> Questa esercitazione presuppone che si abbia familiarità con i concetti di base dell'API Web ASP.NET. Per un'esercitazione introduttiva, vedere [Introduzione a ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> Gli unit test in questo argomento sono intenzionalmente limitato a scenari di dati semplici. Per gli unit test di scenari di dati più avanzati, vedere [comportamento fittizio di Entity Framework quando Unit test ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2

## <a name="in-this-topic"></a>Contenuto dell'argomento

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Prerequisiti](#prereqs)
- [Scaricare il codice](#download)
- [Creare applicazioni con progetto unit test](#appwithunittest)
    - [Aggiungi progetto di unit test, la creazione dell'applicazione](#whencreate)
    - [Aggiungi progetto di unit test per un'applicazione esistente](#addtoexisting)
- [Configurare l'applicazione API Web 2](#setupproject)
- [Installare i pacchetti NuGet nel progetto di test](#testpackages)
- [Creare test](#tests)
- [Eseguire i test](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Scaricare il codice

Scaricare il [progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Il progetto scaricabile include codice degli unit test per questo argomento e per il [comportamento fittizio di Entity Framework quando Unit test ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) argomento.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Creare applicazioni con progetto unit test

È possibile creare un progetto unit test durante la creazione dell'applicazione o aggiungere un progetto unit test per un'applicazione esistente. Questa esercitazione illustra entrambi i metodi per la creazione di un progetto unit test. Per seguire questa esercitazione, è possibile usare entrambi gli approcci.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Aggiungi progetto di unit test, la creazione dell'applicazione

Creare una nuova applicazione Web ASP.NET denominato **StoreApp**.

![Crea progetto](unit-testing-with-aspnet-web-api/_static/image1.png)

Nelle finestre del nuovo progetto ASP.NET, selezionare la **vuoto** modello e aggiungere le cartelle e riferimenti di base per l'API Web. Selezionare il **aggiungere gli unit test** opzione. Progetto di unit test viene automaticamente denominato **StoreApp.Tests**. È possibile mantenere questo nome.

![creare progetto unit test](unit-testing-with-aspnet-web-api/_static/image2.png)

Dopo aver creato l'applicazione, si noterà che contiene due progetti.

![due progetti](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Aggiungi progetto di unit test per un'applicazione esistente

Se non è stato creato il progetto di unit test quando è stata creata l'applicazione, è possibile aggiungerne uno in qualsiasi momento. Si supponga, ad esempio, si dispone già di un'applicazione denominata StoreApp e si desidera aggiungere gli unit test. Per aggiungere un progetto unit test, la soluzione e scegliere **Add** e **nuovo progetto**.

![Aggiungi nuovo progetto alla soluzione](unit-testing-with-aspnet-web-api/_static/image4.png)

Selezionare **Test** nel riquadro a sinistra e scegliere **progetto di Unit Test** per il tipo di progetto. Denominare il progetto **StoreApp.Tests**.

![Aggiungi progetto di unit test](unit-testing-with-aspnet-web-api/_static/image5.png)

Si noterà il progetto di unit test nella soluzione.

Nel progetto di unit test, aggiungere un riferimento progetto a progetto originale.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Configurare l'applicazione API Web 2

Nel progetto StoreApp, aggiungere un file di classe per il **modelli** cartella denominata **Product.cs**. Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Compilare la soluzione.

Fare clic sulla cartella controller e selezionare **Add** e **nuovo elemento di scaffolding**. Selezionare **Web API 2 Controller - vuoto**.

![Aggiungere nuovo controller](unit-testing-with-aspnet-web-api/_static/image6.png)

Impostare il nome del controller **SimpleProductController**, fare clic su **Add**.

![specificare controller](unit-testing-with-aspnet-web-api/_static/image7.png)

Sostituire il codice esistente con quello seguente. Per semplificare questo esempio, i dati vengono archiviati in un elenco anziché un database. L'elenco definito in questa classe rappresenta i dati di produzione. Si noti che il controller include un costruttore che accetta come parametro un elenco di oggetti Product. Questo costruttore consente di passare dati di test quando gli unit test. Il controller include anche due **async** metodi per illustrare gli unit test di metodi asincroni. Questi metodi asincroni sono stati implementati chiamando **Task.FromResult** per ridurre al minimo il codice di estraneo, ma in genere i metodi includono operazioni a elevato utilizzo di risorse.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Il metodo GetProduct restituisce un'istanza di **IHttpActionResult** interfaccia. IHttpActionResult è una delle nuove funzionalità di Web API 2 e semplifica lo sviluppo di unit test. Classi che implementano l'interfaccia IHttpActionResult si trovano nel [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) dello spazio dei nomi. Queste classi rappresentano le possibili risposte da una richiesta di azione e corrispondono ai codici di stato HTTP.

Compilare la soluzione.

A questo punto si è pronti configurare il progetto di test.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installare i pacchetti NuGet nel progetto di test

Quando si usa il modello vuoto per creare un'applicazione, il progetto di unit test (StoreApp.Tests) non include i pacchetti NuGet installati. Altri modelli, ad esempio il modello API Web, includono alcuni pacchetti NuGet nel progetto di unit test. Per questa esercitazione, è necessario includere il pacchetto di Microsoft ASP.NET Web API 2 Core per il progetto di test.

Fare clic sul progetto StoreApp.Tests e selezionare **Gestisci pacchetti NuGet**. È necessario selezionare il progetto StoreApp.Tests per aggiungere i pacchetti al progetto.

![gestire i pacchetti](unit-testing-with-aspnet-web-api/_static/image8.png)

Trovare e installare il pacchetto di Microsoft ASP.NET Web API 2 Core.

![installare il pacchetto principale di api web](unit-testing-with-aspnet-web-api/_static/image9.png)

Chiudere la finestra Gestisci pacchetti NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Creare test

Per impostazione predefinita, il progetto di test include un file di test vuoto denominato UnitTest1.cs. Questo file indica gli attributi da utilizzare per creare metodi di test. Per gli unit test, è possibile usare questo file o creare un file.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

Per questa esercitazione si creerà la propria classe di test. È possibile eliminare il file UnitTest1.cs. Aggiungere una classe denominata **TestSimpleProductController.cs**e sostituire il codice con il codice seguente.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Esegui test

A questo punto si è pronti eseguire i test. Tutti il metodo che sono contrassegnati con il **TestMethod** attributo verrà testato. Dal **Test** voce di menu, eseguire i test.

![eseguire test](unit-testing-with-aspnet-web-api/_static/image11.png)

Aprire il **Esplora Test** finestra e osservare i risultati dei test.

![risultati test](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Riepilogo

Questa esercitazione è stata completata. I dati in questa esercitazione è stati semplificati intenzionalmente focalizzare l'attenzione sulle condizioni di testing unità. Per gli unit test di scenari di dati più avanzati, vedere [comportamento fittizio di Entity Framework quando Unit test ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
