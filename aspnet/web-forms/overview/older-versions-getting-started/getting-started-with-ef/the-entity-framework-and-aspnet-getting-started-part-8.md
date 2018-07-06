---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 8 | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET utilizzando Entity Framework. L'applicazione di esempio è...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 545438a48c57bed00530f3d4cc143542d8999581
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826098"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 8
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Usando la funzionalità di Dynamic Data per formattare e convalidare i dati

Nell'esercitazione precedente è implementato le stored procedure. Questa esercitazione verrà illustrato come la funzionalità di Dynamic Data può offrire i vantaggi seguenti:

- I campi vengono formattati automaticamente per la visualizzazione in base al tipo di dati.
- I campi vengono convalidati automaticamente in base al tipo di dati.
- È possibile aggiungere metadati al modello di dati per personalizzare il comportamento di formattazione e convalida. Quando si esegue questa operazione, è possibile aggiungere le regole di formattazione e convalida un'unica posizione e vengono automaticamente applicati ovunque si accede ai campi utilizzando i controlli Dynamic Data.

Per visualizzarne il funzionamento, sarà necessario modificare i controlli da usare per visualizzare e modificare campi esistenti *Students.aspx* pagina e si aggiungerà la formattazione e convalida i metadati per i campi nome e la data del `Student` tipo di entità.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Utilizzo di markup di DynamicField e controlli di DynamicControl

Aprire il *Students.aspx* pagina e nella `StudentsGridView` controllo Sostituisci il **nome** e **data di registrazione** `TemplateField` elementi con il markup seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Usa questo markup `DynamicControl` controlla invece di `TextBox` e `Label` controlli in studente assegnare un nome campo modello e viene utilizzato un `DynamicField` controllo per la data di registrazione. Nessuna stringa di formato viene specificata.

Aggiungere un `ValidationSummary` controllo dopo il `StudentsGridView` controllo.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

Nel `SearchGridView` controllo sostituire il markup per il **Name** e **data di registrazione** colonne come è stato fatto nel `StudentsGridView` controllare, ad eccezione del fatto omettere il `EditItemTemplate` elemento. Il `Columns` dell'elemento di `SearchGridView` controllo ora contiene il markup seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Aprire *Students.aspx.cs* e aggiungere quanto segue `using` istruzione:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Aggiungere un gestore per la pagina `Init` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Questo codice specifica che i dati dinamici fornirà la formattazione e convalida in questi controlli con associazione a dati per i campi del `Student` entità. Se viene visualizzato un messaggio di errore simile al seguente quando si esegue la pagina, in genere significa che ha dimenticato di chiamare il `EnableDynamicData` metodo `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Eseguire la pagina.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

Nel **data di registrazione** colonna, insieme alla data viene visualizzata l'ora poiché il tipo di proprietà `DateTime`. Correggerai che in un secondo momento.

Per ora, si noti che i dati dinamici offre automaticamente la convalida dei dati di base. Ad esempio, fare clic su **Edit**, deselezionare il campo della data, fare clic su **Update**, e vedrai che i dati dinamici automaticamente rende questa un campo obbligatorio in quanto il valore non ammette valori null nel modello di dati. Nella pagina viene visualizzato un asterisco dopo il campo e un messaggio di errore nel `ValidationSummary` controllo:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

È possibile omettere il `ValidationSummary` controllare, in quanto è anche possibile tenere premuto il puntatore del mouse sull'asterisco per visualizzare il messaggio di errore:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

I dati dinamici convalida inoltre che i dati immessi nel **data di registrazione** campo è una data valida:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Come si può notare, si tratta di un messaggio di errore generico. Nella sezione successiva verrà illustrato come personalizzare i messaggi, nonché la convalida e regole di formattazione.

## <a name="adding-metadata-to-the-data-model"></a>Aggiunta di metadati nel modello di dati

In genere, si desidera personalizzare la funzionalità fornita da Dynamic Data. Ad esempio, è possibile modificare la modalità di visualizzazione dei dati e il contenuto dei messaggi di errore. È in genere anche personalizzare le regole di convalida dei dati per offrire più funzionalità rispetto a ciò che fornisce i dati dinamici automaticamente in base i tipi di dati. A tale scopo è creare classi parziali che corrispondono ai tipi di entità.

Nella **Esplora soluzioni**, fare doppio clic il **ContosoUniversity** progetto, selezionare **Aggiungi riferimento**e aggiungere un riferimento a `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

Nel *DAL* cartella, creare un nuovo file di classe, denominarlo *Student.cs*e sostituire il codice del modello in essa con il codice seguente.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Questo codice crea una classe parziale per il `Student` entità. Il `MetadataType` attributo applicato a questa classe parziale identifica la classe che si sta usando per specificare i metadati. La classe di metadati può avere qualsiasi nome, ma usando il nome dell'entità più "Metadati" è una pratica comune.

Gli attributi applicati alle proprietà nella classe di metadati specificano la formattazione, convalida, regole e messaggi di errore. Gli attributi riportati di seguito saranno disponibili i seguenti risultati:

- `EnrollmentDate` verrà visualizzato come una data (senza un'ora).
- Entrambi i campi nome devono essere 25 caratteri o meno in lunghezza, mentre un messaggio di errore personalizzato viene fornito.
- Sono necessari entrambi i campi nome e viene fornito un messaggio di errore personalizzato.

Eseguire la *Students.aspx* pagina nuovamente e noterete che le date sono ora visualizzate senza tempi di:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Modificare una riga e provare a cancellare i valori nei campi dei nomi. Gli asterischi che indica gli errori di campo vengono visualizzati non appena si lascia un campo, prima di fare clic **Update**. Quando fa clic su **Update**, la pagina Visualizza il testo del messaggio di errore specificato.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Provare a immettere nomi più lunghi di 25 caratteri, fare clic su **Update**, e la pagina Visualizza il testo del messaggio di errore specificato.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Ora che è stato configurato queste regole di convalida e formattazione nei metadati del modello di dati, le regole verranno applicate automaticamente in ogni pagina che consente di visualizzare o consente di apportare modifiche a tali campi, in modo purché utilizzino `DynamicControl` o `DynamicField` controlli. Ciò riduce la quantità di codice ridondante, è necessario scrivere, che rende la programmazione e le operazioni di testing, e assicura che siano coerenti in tutta l'applicazione convalida e formattazione dei dati.

## <a name="more-information"></a>Altre informazioni

Si conclude così questa serie di esercitazioni sui concetti introduttivi di Entity Framework. Per altre risorse che consentono di imparare a usare Entity Framework, continuare [la prima esercitazione della serie di esercitazioni di Entity Framework successiva](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) o i siti seguenti:

- [Domande frequenti su Entity Framework](http://www.ef-faq.org/introduction.html)
- [Blog del Team di Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework in MSDN Library](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework in MSDN Data Developer Center](https://msdn.microsoft.com/data/ef.aspx)
- [Cenni preliminari sul controllo Server Web EntityDataSource in MSDN Library](https://msdn.microsoft.com/library/cc488502.aspx)
- [Controllo EntityDataSource API reference in MSDN Library](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Entity Framework forum su MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog di Julie](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-7.md)
