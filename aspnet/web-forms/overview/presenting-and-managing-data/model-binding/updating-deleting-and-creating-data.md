---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: L'aggiornamento, eliminazione e creazione di dati con l'associazione di modelli e web form | Microsoft Docs
author: tfitzmac
description: Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più linee rette-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: b6cafe29d1cb46061a8743cbee62a7ffec6be990
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393876"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>L'aggiornamento, eliminazione e creazione di dati con l'associazione di modelli e web form
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta i concetti più avanzati nelle esercitazioni successive.
> 
> Questa esercitazione illustra come creare, aggiornare ed eliminare i dati con l'associazione di modelli. Si imposterà le proprietà seguenti:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Queste proprietà di ricezione il nome del metodo che gestisce l'operazione corrispondente. All'interno di tale metodo, per fornire la logica per l'interazione con i dati.
> 
> Questa esercitazione si basa sul progetto creato nel primo [parte](retrieving-data.md) della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

In questa esercitazione, sarà:

1. Aggiungere modelli di dati dinamica
2. Abilitare l'aggiornamento ed eliminazione dei dati tramite metodi di associazione di modelli
3. Applicare le regole di convalida dei dati: abilitare la creazione di un nuovo record nel database

## <a name="add-dynamic-data-templates"></a>Aggiungere modelli di dati dinamica

Per offrire la migliore esperienza utente e ridurre al minimo la ripetizione del codice, si utilizzerà i modelli di dati dinamica. È possibile integrare facilmente i modelli di dati dinamici predefiniti nel proprio sito esistente tramite l'installazione di un pacchetto NuGet.

Dal **Gestisci pacchetti NuGet**, installare il **DynamicDataTemplatesCS**.

![modelli di dati dinamici](updating-deleting-and-creating-data/_static/image1.png)

Si noti che il progetto includa ora una cartella denominata **DynamicData**. In tale cartella, sono disponibili i modelli che vengono applicati automaticamente ai controlli dinamici nei tuoi moduli di web.

![cartella dei dati dinamici](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Abilita aggiornamento ed eliminazione

Consentendo agli utenti di aggiornare ed eliminare i record nel database è molto simile a quella per il recupero dei dati. Nel **UpdateMethod** e **DeleteMethod** proprietà, si specificano i nomi dei metodi che eseguono queste operazioni. Con un controllo GridView, è anche possibile specificare la generazione automatica di modifica ed eliminare i pulsanti. Il codice evidenziato seguente illustra le aggiunte al codice di GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

Nel file code-behind, aggiungere un usando informativa **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Quindi, aggiungere il seguente aggiornamento e metodi di eliminazione.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

Il **TryUpdateModel** metodo si applica i valori associati a dati corrispondenti dal modulo web per l'elemento di dati. L'elemento di dati verrà recuperato in base al valore del parametro id.

## <a name="enforce-validation-requirements"></a>Applicare i requisiti di convalida

Gli attributi di convalida che è stato applicato alle proprietà FirstName, LastName e anno nella classe Student vengono applicati automaticamente quando l'aggiornamento dei dati. I controlli di markup di DynamicField aggiungono i validator di client e server basati su attributi di convalida. Le proprietà FirstName e LastName sono entrambi necessari. FirstName non può superare i 20 caratteri e LastName non può superare i 40 caratteri. Anno deve essere un valore valido per l'enumerazione AcademicYear.

Se l'utente viola uno dei requisiti di convalida, l'aggiornamento viene interrotta. Per visualizzare il messaggio di errore, aggiungere un controllo di controllo ValidationSummary sopra il controllo GridView. Per visualizzare gli errori di convalida dall'associazione di modelli, impostare il **ShowModelStateErrors** impostata su **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Eseguire l'applicazione web e aggiornare ed eliminare tutti i record.

![aggiornare i dati](updating-deleting-and-creating-data/_static/image3.png)

Si noti che quando la modalità di modifica il valore della proprietà anno viene automaticamente eseguito il rendering come un elenco a discesa. La proprietà Year è un valore di enumerazione e il modello di dati dinamica per un valore di enumerazione specifica un menu a discesa per la modifica. È possibile trovare tale modello aprendo il **enumerazione\_Edit. ascx** del file nei **DynamicData**/**FieldTemplates** cartella.

Se si forniscano valori validi, l'aggiornamento viene completato correttamente. In caso di violazione uno dei requisiti di convalida, l'aggiornamento viene interrotta e viene visualizzato un messaggio di errore sopra la griglia.

![messaggio di errore](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Aggiunta di nuovi record

Il controllo GridView non include il **InsertMethod** proprietà e pertanto non può essere usato per l'aggiunta di un nuovo record con l'associazione di modelli. È possibile trovare la proprietà InsertMethod nel **FormView**, **DetailsView**, o **ListView** controlli. In questa esercitazione si utilizzerà un controllo FormView per aggiungere un nuovo record.

In primo luogo, aggiungere un collegamento alla nuova pagina che verrà creato per aggiungere un nuovo record. Di sopra di controllo ValidationSummary, aggiungere:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Il nuovo collegamento verrà visualizzato nella parte superiore del contenuto della pagina Students.

![nuovo collegamento](updating-deleting-and-creating-data/_static/image5.png)

Quindi, aggiungere un nuovo web form mediante pagina master e denominarla **AddStudent**. Selezionare la pagina master Site. master.

Si forniranno i campi per l'aggiunta di un nuovo studente utilizzando un **DynamicEntity** controllo. Il controllo DynamicEntity esegue il rendering di tale proprietà modificabili nella classe specificata nella proprietà del tipo di elemento. La proprietà ID studente è stata contrassegnata con il **[ScaffoldColumn(false)]** in modo che non viene eseguito il rendering dell'attributo. Nel segnaposto MainContent della pagina AddStudent, aggiungere il codice seguente.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

Nel file code-behind (AddStudent.aspx.cs), aggiungere un **usando** istruzione per il **ContosoUniversityModelBinding.Models** dello spazio dei nomi.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Quindi, aggiungere i metodi seguenti per specificare la modalità inserire un nuovo record e un gestore eventi per il pulsante Annulla.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Salvare tutte le modifiche.

Eseguire l'applicazione web e creare un nuovo studente.

![Aggiungere nuovo studente](updating-deleting-and-creating-data/_static/image6.png)

Fare clic su **Inserisci** e notare il nuovo studente è stato creato.

![visualizzare di nuovo studente](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione, è abilitato l'aggiornamento, eliminazione e creazione di dati. Si verificato durante l'interazione con i dati vengono applicate regole di convalida.

Entro i prossimi [esercitazione](sorting-paging-and-filtering-data.md) in questa serie si abiliterà l'ordinamento, paging e filtro dei dati.

> [!div class="step-by-step"]
> [Precedente](retrieving-data.md)
> [Successivo](sorting-paging-and-filtering-data.md)
