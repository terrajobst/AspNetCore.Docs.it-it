---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Utilizzo di HTML5 e jQuery UI Datepicker Popup Calendar with ASP.NET MVC - parte 1 | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione verrà fornite le nozioni di base lavorare con modelli di editor, modelli di visualizzazione e il calendario di jQuery UI datepicker popup in una macchina virtuale di ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 4b5507021af47d96c29809c9830d0558f5501f87
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Utilizzo di HTML5 e jQuery UI Datepicker Popup Calendar with ASP.NET MVC - parte 1
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base lavorare con modelli di editor, modelli di visualizzazione e il calendario di jQuery UI datepicker popup in un'applicazione Web ASP.NET MVC.


Questa esercitazione viene illustrato come utilizzare i modelli di editor, modelli di visualizzazione e di jQuery le nozioni fondamentali sul [calendario UI datepicker popup](http://plugins.jquery.com/project/datepicker) in un'applicazione Web ASP.NET MVC. Per questa esercitazione, è possibile utilizzare Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), che è una versione gratuita di Microsoft Visual Studio o se si dispone già di, è possibile utilizzare Visual Studio 2010 SP1.

Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente il software richiesto usando i collegamenti seguenti:

- [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)

Se si utilizza Visual Studio 2010 anziché Visual Web Developer, è possibile installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

In questa esercitazione si presuppone di aver completato il [Guida introduttiva a MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) esercitazione o che si ha familiarità con lo sviluppo di ASP.NET MVC. In questa esercitazione inizia con il progetto completato il [Guida introduttiva a MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) esercitazione.

In questa esercitazione viene illustrato codice in c#. Tuttavia, il [progetto iniziale](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) e progetto completato sono disponibili anche in Visual Basic.

Un progetto di Visual Studio con codice sorgente c# e Visual Basic è disponibile a complemento di questo argomento: [scaricare](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Cosa Compilerai

È possibile aggiungere modelli (in particolare, modificare e visualizzare i modelli) per l'applicazione di elenco di film semplice che è stato creato nel [Guida introduttiva a MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) esercitazione. Verrà inoltre aggiunto un [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendario popup per semplificare il processo di immissione di date. Nella schermata seguente viene illustrata l'applicazione modificato con jQuery UI datepicker popup calendario visualizzato.

![termine jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Si apprenderà competenze

Ecco cosa si apprenderà:

- Utilizzo degli attributi dal [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi per controllare il formato dei dati quando viene visualizzato e quando è in modalità di modifica.
- Come creare modelli (modificare e visualizzare i modelli) per controllare la formattazione dei dati.
- Come aggiungere il [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) come un modo per inserire i campi di Data.

### <a name="getting-started"></a>Introduzione

Se si dispone già l'applicazione di elenco di film dal progetto di partenza, scaricare i file utilizzando il collegamento seguente: [scaricare](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800). In Esplora risorse, quindi fare clic il *MvcMovie.zip* file e selezionare **proprietà**. Nel **MvcMovie.zip proprietà** nella finestra di dialogo **Unblock**. (Lo sblocco impedisce a un avviso di sicurezza che si verifica quando si tenta di utilizzare un *zip* file scaricato dal web.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Fare doppio clic su di *MvcMovie.zip* file e selezionare **Estrai tutto** per decomprimere il file. In Visual Studio 2010 o Visual Web Developer aprire la *MvcMovieCS\_TU.sln* file.

In **Esplora**, fare doppio clic su di *Views\Shared\\layout. cshtml* per aprirlo. Modifica il `H1` intestazione dalla **App cinematografica MVC** a **jQuery film**. Premere CTRL + F5 per eseguire l'applicazione e fare clic su di **Home** scheda, che consente di passare al `Index` metodo del controller di film. Per provare l'applicazione, selezionare il **modifica** collegamento e **dettagli** collegamento per uno dei filmati. Si noti che nelle visualizzazioni di indice, Edit e dettagli, la data di rilascio e il prezzo sono correttamente formattati:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

La formattazione per la data e il prezzo è il risultato dell'utilizzo di [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo delle proprietà del `Movie` classe.

Aprire il *Movie.cs* file e impostare come commento il `DisplayFormat` attributo la `ReleaseDate` e `Price` proprietà. Il valore risultante `Movie` classe è simile al seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Premere CTRL + F5 per eseguire l'applicazione e selezionare il **Home** scheda per visualizzare l'elenco di film. La data di rilascio Mostra la data e l'ora e il campo prezzo non viene più visualizzato il simbolo di valuta. La modifica di `Movie` classe annullata la formattazione nice illustrato in precedenza, ma che potrà essere corretto in proposito.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Utilizzo dell'attributo DataAnnotations DataType per specificare il tipo di dati

Sostituire il commento `DisplayFormat` attributo per il `ReleaseDate` proprietà con il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo, mediante il `Date` enumerazione. Sostituire il `DisplayFormat` attributo per il `Price` proprietà con il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo nuovamente, questa volta usando il `Currency` enumerazione. Questo è il codice completato l'aspetto seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Eseguire l'applicazione. Ora la data di rilascio e le proprietà di prezzo sono formattate correttamente (ovvero con formati di data e valuta appropriati). Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo fornisce metadati del tipo per MVC ASP.NET predefinita modelli in modo che i campi di cui è stato eseguito il rendering in formato corretto. Utilizzando il `DataType` attributo è preferibile usare il `DisplayFormat` attributo originariamente nel codice, poiché il `DataType` attributo rende il modello più semplice e più flessibile per scopi di internazionalizzazione.

Nella sezione successiva verrà visualizzato come creare modelli personalizzati per visualizzare i campi Data.

>[!div class="step-by-step"]
[avanti](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
