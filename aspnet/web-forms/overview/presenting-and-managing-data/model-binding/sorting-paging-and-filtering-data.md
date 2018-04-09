---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Ordinamento, paging e filtro dei dati con l'associazione di modelli e web form | Documenti Microsoft
author: tfitzmac
description: Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: d63ebecadd392877e4cb1d1dffe9db2d1d231190
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Ordinamento, paging e filtro dei dati con l'associazione di modelli e web form
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta concetti più avanzati nelle esercitazioni successive.
> 
> In questa esercitazione viene illustrato come aggiungere l'ordinamento, paging e filtro dei dati tramite l'associazione di modelli.
> 
> Questa esercitazione si basa sul progetto creato nel primo [parte](retrieving-data.md) della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.


## <a name="what-youll-build"></a>Occorre invece compilare

In questa esercitazione, effettuare le operazioni seguenti:

1. Abilitare l'ordinamento e paging dei dati
2. Abilitare il filtro dei dati basati su una selezione dall'utente

## <a name="add-sorting"></a>Aggiungere l'ordinamento

Abilitazione di un ordinamento in GridView è molto semplice. Nel file Student.aspx, è sufficiente impostare **AllowSorting** a **true** in GridView. Non è necessario impostare un **SortExpression** come DataField viene usato automaticamente il valore per ogni colonna. GridView modifica la query per includere l'ordinamento dei dati dal valore selezionato. Il codice evidenziato di seguito viene illustrata l'aggiunta che è necessario apportare abilitare l'ordinamento.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Eseguire l'applicazione web e test record studente ordinamento per i valori in colonne diverse.

![studenti di ordinamento](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>aggiunta di impaginazione

L'abilitazione di paging è anche molto semplice. In GridView, impostare il **AllowPaging** proprietà **true** e impostare il **PageSize** proprietà per il numero di record che si desidera visualizzare in ogni pagina. In questa esercitazione, è possibile impostarla su 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Eseguire l'applicazione web e notare che ora i record sono suddivisi su più pagine con non più di 4 record visualizzati in una singola pagina.

![aggiunta di impaginazione](sorting-paging-and-filtering-data/_static/image4.png)

Esecuzione di query posticipata migliora l'efficienza dell'applicazione. Invece di recuperare l'intero set di dati, il controllo GridView modifica la query per recuperare solo i record per la pagina corrente.

## <a name="filter-records-by-user-selection"></a>Filtrare i record per la selezione utente

Associazione di modelli aggiunge diversi attributi che consentono di designare come impostare il valore per un parametro in un metodo di associazione del modello. Questi attributi sono di **System.Web.ModelBinding** dello spazio dei nomi. e comprendono:

- Control
- Cookie
- Form
- Profilo
- QueryString
- RouteData
- Sessione
- UserProfile
- ViewState

In questa esercitazione si utilizzerà il valore di un controllo per filtrare i record visualizzati in GridView. Si aggiungerà il **controllo** attributo al metodo di query è stato creato in precedenza. In un [in un secondo momento](using-query-string-values-to-retrieve-data.md) esercitazione, si applicherà il **QueryString** attributo a un parametro per specificare che il valore del parametro proviene da un valore di stringa di query.

In primo luogo, sopra il controllo ValidationSummary, aggiungere un menu a discesa per il filtro vengono visualizzati quali studenti.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

Nel file code-behind, modificare il metodo select per ricevere un valore dal controllo e impostare il nome del parametro sul nome del controllo che fornisce il valore.

È necessario aggiungere un **utilizzando** istruzione per il **System.Web.ModelBinding** dello spazio dei nomi per risolvere l'attributo del controllo.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Il codice seguente illustra il metodo select nuovamente lavorato per filtrare i dati restituiti in base al valore dell'elenco a discesa. Aggiunta di un attributo di controllo prima di un parametro specifica che il valore per questo parametro viene fornito da un controllo con lo stesso nome.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Eseguire l'applicazione web e selezionare valori diversi nell'elenco a discesa elenco per filtrare l'elenco di studenti.

![studenti di filtro](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione è abilitato ordinamento e paging dei dati. È attivato il filtro dei dati dal valore di un controllo.

Nella prossima [esercitazione](integrating-jquery-ui.md) si procederà al miglioramento dell'interfaccia utente tramite l'integrazione di un widget di JQuery UI nel modello di dati dinamica.

> [!div class="step-by-step"]
> [Precedente](updating-deleting-and-creating-data.md)
> [Successivo](integrating-jquery-ui.md)
