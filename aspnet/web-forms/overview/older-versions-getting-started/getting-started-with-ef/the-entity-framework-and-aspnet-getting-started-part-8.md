---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e ASP.NET 4 di Web Form - parte 8 | Documenti Microsoft
author: tdykstra
description: "L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET tramite Entity Framework. È l'applicazione di esempio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 323ee44f43f6d4081bd9ba50791755696bc9128f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e form ASP.NET Web 4 - parte 8
====================
Da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET utilizzando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Utilizzo di funzionalità di Dynamic Data per formattare e convalidare i dati

Nell'esercitazione precedente è implementato stored procedure. In questa esercitazione verrà illustrato come funzionalità di Dynamic Data possono offrire i vantaggi seguenti:

- I campi vengono formattati automaticamente per la visualizzazione in base al tipo di dati.
- I campi vengono convalidati automaticamente in base al tipo di dati.
- È possibile aggiungere metadati al modello di dati per personalizzare il comportamento di convalida e formattazione. Quando si esegue questa operazione, è possibile aggiungere le regole di convalida e formattazione in un'unica posizione e vengono automaticamente applicati ovunque si accede ai campi utilizzando i controlli Dynamic Data.

Per il funzionamento, è necessario sostituire i controlli consentono di visualizzare e modificare i campi esistenti *Students.aspx* pagina e si aggiungeranno i metadati di convalida e formattazione per i campi nome e la data del `Student` tipo di entità.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Utilizzo di DynamicField e controlli di DynamicControl

Aprire il *Students.aspx* pagina e nella `StudentsGridView` controllo Sostituisci il **nome** e **data di registrazione** `TemplateField` gli elementi con il markup seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Usa questo markup `DynamicControl` controlla al posto di `TextBox` e `Label` controlli gli studenti nome campo modello e viene utilizzato un `DynamicField` controllo per la data di registrazione. Nessuna stringa di formato viene specificata.

Aggiungere un `ValidationSummary` controllo dopo la `StudentsGridView` controllo.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

Nel `SearchGridView` controllo sostituire il markup per il **nome** e **data di registrazione** colonne come è stato eseguito nel `StudentsGridView` controllare, ad eccezione del fatto omettere il `EditItemTemplate` elemento. Il `Columns` elemento il `SearchGridView` controllo contiene ora il markup seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Aprire *Students.aspx.cs* e aggiungere le seguenti `using` istruzione:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Aggiungere un gestore per la pagina `Init` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Questo codice specifica che i dati dinamici fornirà la formattazione e convalida in questi controlli con associazione a dati per i campi del `Student` entità. Se viene visualizzato un messaggio di errore analogo al seguente quando si esegue la pagina, in genere significa che è stata dimenticata chiamare il `EnableDynamicData` metodo `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Eseguire la pagina.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

Nel **data di registrazione** colonna, insieme alla data viene visualizzata l'ora perché il tipo di proprietà `DateTime`. Che sarà possibile risolvere in un secondo momento.

Per il momento, si noti che i dati dinamici fornisce automaticamente la convalida dei dati di base. Ad esempio, fare clic su **modifica**, deselezionare il campo Data, fare clic su **aggiornamento**, noterai che Dynamic Data automaticamente rende questa un campo obbligatorio in quanto il valore non è nullable nel modello di dati. La pagina viene visualizzato un asterisco dopo il campo e un messaggio di errore nel `ValidationSummary` controllo:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

È possibile omettere il `ValidationSummary` controllare, in quanto è inoltre possibile tenere il puntatore del mouse su asterisco per visualizzare il messaggio di errore:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dati dinamici convalida inoltre che i dati immessi nel **data di registrazione** campo è una data valida:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Come si può notare, si tratta di un messaggio di errore generico. Nella sezione successiva verrà visualizzato come personalizzare i messaggi di convalida e regole di formattazione.

## <a name="adding-metadata-to-the-data-model"></a>Aggiunta di metadati al modello di dati

In genere, si desidera personalizzare la funzionalità di Dynamic Data. Ad esempio, è possibile modificare la modalità di visualizzazione dati e il contenuto dei messaggi di errore. È in genere anche personalizzare le regole di convalida dei dati per fornire ulteriori funzionalità di Dynamic Data fornisce automaticamente in base ai tipi di dati. A tale scopo, è creare classi parziali che corrispondono ai tipi di entità.

In **Esplora**, fare doppio clic su di **ContosoUniversity** progetto, selezionare **Aggiungi riferimento**e aggiungere un riferimento a `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

Nel *DAL* cartella, creare un nuovo file di classe, denominarlo *Student.cs*e sostituire il codice del modello in essa contenuti con il codice seguente.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Questo codice crea una classe parziale per il `Student` entità. Il `MetadataType` applicato a questa classe parziale che identifica la classe che si sta utilizzando per specificare i metadati. La classe di metadati può avere qualsiasi nome, ma il nome dell'entità più "Metadati" è una pratica comune.

Gli attributi applicati alle proprietà nella classe di metadati specificano la formattazione, i messaggi di errore, regole e convalida. Gli attributi riportati di seguito saranno disponibili i seguenti risultati:

- `EnrollmentDate`verrà visualizzato come una data (senza un tempo).
- Entrambi i campi nome devono essere di 25 caratteri o meno in lunghezza e un messaggio di errore personalizzato viene fornita.
- Sono necessari entrambi i campi nome e viene fornito un messaggio di errore personalizzato.

Eseguire il *Students.aspx* pagina nuovamente e si noterà che le date sono visualizzate senza tempi di:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Modifica di una riga e provare a cancellare i valori nei campi dei nomi. Gli asterischi che indica gli errori di campo vengono visualizzati non appena si lascia un campo, prima di scegliere **aggiornamento**. Quando fa clic su **aggiornamento**, nella pagina viene visualizzato il testo del messaggio di errore specificato.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Provare a immettere i nomi di più di 25 caratteri, fare clic su **aggiornamento**, e la pagina Visualizza il testo del messaggio di errore specificato.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Dopo che è stato impostato queste regole di convalida e formattazione nei metadati del modello di dati, le regole verranno applicate automaticamente in ogni pagina che visualizza o consente di apportare modifiche a questi campi, purché si utilizzi `DynamicControl` o `DynamicField` controlli. Questo riduce la quantità di codice ridondante, che è necessario scrivere, rendendo così programmazione e le operazioni di testing, e si garantisce che la convalida e la formattazione dei dati siano coerenti in tutta l'applicazione.

## <a name="more-information"></a>Altre informazioni

Si conclude così questa serie di esercitazioni sui concetti introduttivi di Entity Framework. Per altre risorse che consentono di imparare a usare Entity Framework, continuare con [la prima esercitazione della serie di esercitazioni di Entity Framework successiva](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) o i siti seguenti:

- [Domande frequenti su Entity Framework](http://www.ef-faq.org/introduction.html)
- [Blog del Team di Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework in MSDN Library](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework nel centro per sviluppatori MSDN dati](https://msdn.microsoft.com/data/ef.aspx)
- [Panoramica del controllo Server Web EntityDataSource in MSDN Library](https://msdn.microsoft.com/library/cc488502.aspx)
- [Controllo EntityDataSource API reference in MSDN Library](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Forum di Entity Framework in MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog di Julie Lerman](http://thedatafarm.com/blog/)

>[!div class="step-by-step"]
[Precedente](the-entity-framework-and-aspnet-getting-started-part-7.md)
