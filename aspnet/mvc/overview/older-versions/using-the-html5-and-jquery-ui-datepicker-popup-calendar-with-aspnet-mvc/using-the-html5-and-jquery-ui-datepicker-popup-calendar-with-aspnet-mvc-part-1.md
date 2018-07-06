---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Uso di HTML5 e jQuery UI Datepicker Popup Calendar con ASP.NET MVC - parte 1 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base di come usare i modelli nell'editor di modelli di visualizzazione e il calendario jQuery UI datepicker popup in MV un ASP.NET...
ms.author: aspnetcontent
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: dccebd7fb53f947bf782a486f5643bf4c05542d1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841435"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Uso di HTML5 e jQuery UI Datepicker Popup Calendar con ASP.NET MVC - parte 1
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Questa esercitazione insegnerà le nozioni di base di come usare i modelli nell'editor di modelli di visualizzazione e il calendario di jQuery UI datepicker popup in un'applicazione Web ASP.NET MVC.


Questa esercitazione insegnerà le nozioni di base di come usare i modelli nell'editor di modelli di visualizzazione e il componente aggiuntivo jQuery [calendario UI datepicker popup](http://plugins.jquery.com/project/datepicker) in un'applicazione Web ASP.NET MVC. Per questa esercitazione, è possibile usare Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;in Visual Web Developer&quot;), che è una versione gratuita di Microsoft Visual Studio o se si dispone già di questo, è possibile usare Visual Studio 2010 SP1.

Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i software necessari usando i collegamenti seguenti:

- [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)

Se si usa Visual Studio 2010 anziché Visual Web Developer, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Questa esercitazione si presuppone di aver completato la [Introduzione a MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) esercitazione o che si abbia familiarità con lo sviluppo di ASP.NET MVC. Questa esercitazione inizia con il progetto completato il [Introduzione a MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) esercitazione.

Questa esercitazione Mostra codice in c#. Tuttavia, il [progetto iniziale](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) e progetto completato sono anche disponibili in Visual Basic.

Un progetto di Visual Studio con il codice sorgente c# e Visual Basic è disponibile a complemento di questo argomento: [scaricare](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Scopo dell'esercitazione

Si aggiungeranno i modelli (in particolare, modificare e visualizzare i modelli) all'applicazione dall'elenco dei filmati semplice che è stato creato nel [Introduzione a MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) esercitazione. Si aggiungerà anche un [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendario popup per semplificare il processo di immissione di date. Lo screenshot seguente mostra l'applicazione modificata con jQuery UI datepicker popup calendario visualizzato.

![jQuery completato](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Competenze

Ecco cosa si apprenderà:

- Come usare gli attributi dal [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi per controllare il formato dei dati quando viene visualizzato e quando è in modalità di modifica.
- Come creare modelli (modificare e visualizzare i modelli) per controllare la formattazione dei dati.
- Come aggiungere il [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) come un modo per immettere i campi Data.

### <a name="getting-started"></a>Introduzione

Se si ha già l'applicazione dall'elenco dei filmati dal progetto di partenza, scaricarlo usando il collegamento seguente: [scaricare](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800). Quindi, in Windows Explorer, fare doppio clic il *MvcMovie.zip* del file e selezionare **proprietà**. Nel **proprietà MvcMovie.zip** finestra di dialogo **Unblock**. (Lo sblocco impedisce a un avviso di sicurezza che si verifica quando si prova a usare una *zip* file scaricato dal web.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Fare doppio clic il *MvcMovie.zip* del file e selezionare **Estrai tutto** per decomprimere il file. In Visual Studio 2010 o Visual Web Developer aprire la *MvcMovieCS\_TU.sln* file.

Nelle **Esplora soluzioni**, fare doppio clic il *Views\Shared\\layout. cshtml* per aprirlo. Modifica il `H1` intestazione dalla **App MVC Movie** al **film jQuery**. Premere CTRL+F5 per eseguire l'applicazione, quindi scegliere il **Home** nella scheda, si viene indirizzati al `Index` metodo del controller di film. Per provare l'applicazione, selezionare la **Edit** collegamento e il **dettagli** collegamento per uno dei film. Si noti che nelle visualizzazioni di indice, Edit e i dettagli, la data di rilascio e il prezzo viene formattati in modo corretto:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

La formattazione per la data e il prezzo è il risultato dell'uso il [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) nelle proprietà dell'attributo il `Movie` classe.

Aprire il *Movie.cs* del file e impostare come commento il `DisplayFormat` dell'attributo sul `ReleaseDate` e `Price` proprietà. L'oggetto risultante `Movie` abbia un aspetto simile al seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Premere CTRL+F5 per eseguire l'applicazione e selezionare il **Home** pressione di tab per visualizzare l'elenco di film. Questa volta la data di rilascio Mostra la data e ora e il campo del prezzo non viene più visualizzato il simbolo di valuta. La modifica nel `Movie` classe ha annullato la formattazione interessante illustrato in precedenza, ma si correggerà che a breve.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Utilizzo dell'attributo DataAnnotations DataType per specificare il tipo di dati

Sostituire il commento `DisplayFormat` dell'attributo per il `ReleaseDate` proprietà con il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo, mediante il `Date` enumerazione. Sostituire il `DisplayFormat` dell'attributo per il `Price` proprietà con il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo anche in questo caso, questa volta usando il `Currency` enumerazione. Si tratta di come appare il codice completo:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Eseguire l'applicazione. Ora la data di rilascio e le proprietà di prezzo sono formattate correttamente (vale a dire usando formati di data e valuta appropriati). Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo fornisce metadati del tipo per l'incorporato di ASP.NET MVC i modelli in modo che i campi di eseguire il rendering nel formato corretto. Usando il `DataType` attributo è preferibile all'uso di `DisplayFormat` attributo originariamente nel codice, poiché il `DataType` attributo rende il modello più pulito e flessibile per scopi, ad esempio internazionalizzazione.

Nella sezione successiva verrà illustrato come creare modelli personalizzati per visualizzare i campi Data.

> [!div class="step-by-step"]
> [avanti](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
