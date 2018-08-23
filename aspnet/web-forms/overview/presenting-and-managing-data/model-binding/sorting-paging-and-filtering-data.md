---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: L'ordinamento, paging e filtro dei dati con l'associazione di modelli e web form | Microsoft Docs
author: tfitzmac
description: Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più linee rette-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 86ddedb68b8d18057cd2f7d68e795efda33734b1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836852"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>L'ordinamento, paging e filtro dei dati con l'associazione di modelli e web form
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta i concetti più avanzati nelle esercitazioni successive.
> 
> Questa esercitazione illustra come aggiungere l'ordinamento, paging e filtro dei dati tramite l'associazione di modelli.
> 
> Questa esercitazione si basa sul progetto creato nel primo [parte](retrieving-data.md) della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

In questa esercitazione, sarà:

1. Abilitare l'ordinamento e paging dei dati
2. Abilitare il filtro dei dati basata su una selezione dall'utente

## <a name="add-sorting"></a>Aggiungere l'ordinamento

Abilitazione di un ordinamento nel controllo GridView è molto semplice. Nel file Student.aspx, è sufficiente impostare **AllowSorting** al **true** in GridView. Non è necessario impostare una **SortExpression** valore DataField viene usato automaticamente per ogni colonna. Il controllo GridView consente di modificare la query per includere i dati di ordinamento dal valore selezionato. Il codice evidenziato di seguito viene illustrata l'aggiunta che è necessario apportare abilitare l'ordinamento.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Eseguire l'applicazione web e i record di ordinamento per studenti per testare i valori in colonne diverse.

![studenti di ordinamento](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Aggiunta di impaginazione

L'abilitazione di paging è anche molto semplice. In GridView, impostare il **AllowPaging** proprietà **true** e impostare il **PageSize** proprietà per il numero di record da visualizzare in ogni pagina. In questa esercitazione, è possibile impostarlo su 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Eseguire l'applicazione web e notare che ora i record sono suddivisi su più pagine con non più di 4 record visualizzati in una singola pagina.

![Aggiunta di impaginazione](sorting-paging-and-filtering-data/_static/image4.png)

Esecuzione di query posticipata migliora l'efficienza dell'applicazione. Invece di recuperare l'intero set di dati, il controllo GridView consente di modificare la query per recuperare solo i record per la pagina corrente.

## <a name="filter-records-by-user-selection"></a>Filtrare i record in base a selezione utente

Associazione di modelli consente di aggiungere diversi attributi che consentono di designare come impostare il valore per un parametro in un metodo di associazione del modello. Questi attributi sono le **System.Web.ModelBinding** dello spazio dei nomi. e comprendono:

- Control
- Cookie
- Form
- Profilo
- QueryString
- RouteData
- Sessione
- UserProfile
- ViewState

In questa esercitazione si userà il valore di un controllo per filtrare i record visualizzati in GridView. Si aggiungerà il **controllo** attributo al metodo di query è stato creato in precedenza. In un [in un secondo momento](using-query-string-values-to-retrieve-data.md) esercitazione, si applicherà il **QueryString** attributo a un parametro per specificare che il valore del parametro provenga da un valore di stringa di query.

In primo luogo, di sopra di controllo ValidationSummary, aggiungere un menu a discesa per il filtro che gli studenti vengono visualizzati.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

Nel file code-behind, modificare il metodo select per ricevere un valore dal controllo e impostare il nome del parametro per il nome del controllo che fornisce il valore.

È necessario aggiungere un **usando** istruzione per il **System.Web.ModelBinding** dello spazio dei nomi per risolvere l'attributo del controllo.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Il codice seguente illustra il metodo select nuovamente ha lavorato per filtrare i dati restituiti in base al valore di elenco a discesa. Aggiunta di un attributo del controllo prima di un parametro specifica che il valore per questo parametro provenga da un controllo con lo stesso nome.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Eseguire l'applicazione web e selezionare valori diversi nell'elenco a discesa elenco per filtrare l'elenco degli studenti.

![studenti di filtro](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione, è abilitato l'ordinamento e paging dei dati. È anche abilitato il filtro dei dati dal valore di un controllo.

Entro i prossimi [esercitazione](integrating-jquery-ui.md) si procederà al miglioramento dell'interfaccia utente tramite l'integrazione di un widget di JQuery UI nel modello di dati dinamica.

> [!div class="step-by-step"]
> [Precedente](updating-deleting-and-creating-data.md)
> [Successivo](integrating-jquery-ui.md)
