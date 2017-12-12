---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: L'aggiornamento, eliminazione e creazione di dati di associazione del modello e web form | Documenti Microsoft
author: tfitzmac
description: "Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 18c065b44524e7738c048b5908fa50c592188064
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>L'aggiornamento, eliminazione e creazione di dati di associazione del modello e web form
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta concetti più avanzati nelle esercitazioni successive.
> 
> In questa esercitazione viene illustrato come creare, aggiornare ed eliminare i dati con l'associazione di modelli. Impostare le proprietà seguenti:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Queste proprietà è visualizzato il nome del metodo che gestisce l'operazione corrispondente. All'interno di tale metodo, fornire la logica per l'interazione con i dati.
> 
> Questa esercitazione si basa sul progetto creato nel primo [parte](retrieving-data.md) della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.


## <a name="what-youll-build"></a>Occorre invece compilare

In questa esercitazione, effettuare le operazioni seguenti:

1. Aggiungere modelli di dati dinamici
2. Abilitare l'aggiornamento ed eliminazione dei dati tramite i metodi di associazione del modello
3. Applicare le regole di convalida di dati: consentono di creare un nuovo record nel database

## <a name="add-dynamic-data-templates"></a>Aggiungere modelli di dati dinamici

Per offrire la migliore esperienza utente e ridurre al minimo ripetizioni nel codice, si utilizzeranno i modelli di dati dinamica. È possibile integrare facilmente i modelli di dati dinamica preesistente nel sito esistente mediante l'installazione di un pacchetto NuGet.

Dal **Gestisci pacchetti NuGet**, installare il **DynamicDataTemplatesCS**.

![modelli di dati dinamici](updating-deleting-and-creating-data/_static/image1.png)

Si noti che il progetto include ora una cartella denominata **DynamicData**. In questa cartella sono disponibili i modelli vengono applicati automaticamente ai controlli dinamici nei moduli web.

![cartella Dynamic data](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Abilita aggiornamento ed eliminazione

Consentire agli utenti di aggiornare ed eliminare i record nel database è molto simile al processo per il recupero dei dati. Nel **UpdateMethod** e **DeleteMethod** proprietà, specificare i nomi dei metodi che eseguono tali operazioni. Un controllo GridView, è anche possibile specificare la generazione automatica di modifica ed eliminare i pulsanti. Il codice evidenziato di seguito mostra le aggiunte al codice GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

Nel file code-behind, aggiungere un tramite l'istruzione per **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Quindi, aggiungere il seguente aggiornamento ed eliminazione di metodi.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

Il **TryUpdateModel** metodo si applica l'associazione a dati i valori corrispondenti dal web form per l'elemento di dati. L'elemento di dati verrà recuperato in base al valore del parametro id.

## <a name="enforce-validation-requirements"></a>Applicare i requisiti di convalida

Gli attributi di convalida che applica alle proprietà FirstName, LastName e l'anno nella classe Student vengono applicati automaticamente quando l'aggiornamento dei dati. I controlli di DynamicField aggiungono validator client e server in base agli attributi di convalida. Le proprietà FirstName e LastName sono entrambi necessari. Non può superare i 20 caratteri FirstName e LastName non può superare i 40 caratteri. Anno deve essere un valore valido per l'enumerazione AcademicYear.

Se l'utente viola uno dei requisiti di convalida, l'aggiornamento non verrà eseguito. Per visualizzare il messaggio di errore, aggiungere un controllo ValidationSummary sopra il controllo GridView. Per visualizzare gli errori di convalida dell'associazione modello, impostare il **ShowModelStateErrors** proprietà impostata su **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Eseguire l'applicazione web e aggiornare ed eliminare tutti i record.

![Aggiornare i dati](updating-deleting-and-creating-data/_static/image3.png)

Si noti che quando la modalità di modifica il valore per la proprietà Year viene automaticamente eseguito il rendering come un elenco a discesa. La proprietà Year è un valore di enumerazione e il modello di dati dinamica per un valore di enumerazione specifica un menu a discesa per la modifica. È possibile trovare tale modello aprendo il **enumerazione\_Edit. ascx** file nel **DynamicData**/**FieldTemplates** cartella.

Se si forniscono i valori validi, l'aggiornamento viene completato correttamente. In caso di violazione di uno dei requisiti di convalida, l'aggiornamento non verrà eseguito e un messaggio di errore viene visualizzato sopra la griglia.

![Messaggio di errore](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Aggiunta di nuovi record

Il controllo GridView non include il **InsertMethod** proprietà e pertanto non può essere utilizzato per l'aggiunta di un nuovo record con l'associazione di modelli. È possibile trovare la proprietà InsertMethod nel **FormView**, **DetailsView**, o **ListView** controlli. In questa esercitazione si utilizzerà un controllo FormView per aggiungere un nuovo record.

In primo luogo, aggiungere un collegamento alla nuova pagina che viene creata per l'aggiunta di un nuovo record. Sopra il controllo ValidationSummary, aggiungere:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Il nuovo collegamento verrà visualizzato nella parte superiore del contenuto per la pagina di studenti.

![nuovo collegamento](updating-deleting-and-creating-data/_static/image5.png)

Quindi, aggiungere un nuovo web form mediante pagina master e denominarlo **AddStudent**. Selezionare la pagina master Site. master.

Verrà eseguito il rendering dell'oggetto i campi per l'aggiunta di un nuovo studente utilizzando un **DynamicEntity** controllo. Il controllo DynamicEntity esegue il rendering di tale proprietà modificabili nella classe specificata nella proprietà ItemType. La proprietà StudentID è stata contrassegnata con il **[ScaffoldColumn(false)]** attributo in modo che non viene eseguito il rendering. Segnaposto della pagina AddStudent MainContent, aggiungere il codice seguente.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

Nel file code-behind (AddStudent.aspx.cs), aggiungere un **utilizzando** istruzione per il **ContosoUniversityModelBinding.Models** dello spazio dei nomi.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Aggiungere quindi i metodi seguenti per specificare la modalità di inserimento di un nuovo record e un gestore eventi per il pulsante Annulla.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Salvare tutte le modifiche.

Eseguire l'applicazione web e creare un nuovo studente.

![Aggiungere nuovo studente](updating-deleting-and-creating-data/_static/image6.png)

Fare clic su **inserire** e notare che è stato creato il nuovo studente.

![visualizzare di nuovo studente](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione è abilitato l'aggiornamento, eliminazione e creazione di dati. Si verificato durante l'interazione con i dati vengono applicate regole di convalida.

Nella prossima [esercitazione](sorting-paging-and-filtering-data.md) in questa serie, si abiliterà ordinamento, paging e filtro dei dati.

>[!div class="step-by-step"]
[Precedente](retrieving-data.md)
[Successivo](sorting-paging-and-filtering-data.md)
