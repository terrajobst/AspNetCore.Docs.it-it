---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
title: "La visualizzazione di più record per ogni riga con il controllo DataList (c#) | Documenti Microsoft"
author: rick-anderson
description: "In questa breve esercitazione verrà illustrato come personalizzare il layout del controllo DataList tramite le relative proprietà RepeatColumns e RepeatDirection."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: cf5acaf5-d4f6-4957-badc-b89956b285f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e9f04089afdbeb1b13725536c9fe97951ee8ca5c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-c"></a>La visualizzazione di più record per ogni riga con il controllo DataList (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_CS.exe) o [Scarica il PDF](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/datatutorial31cs1.pdf)

> In questa breve esercitazione verrà illustrato come personalizzare il layout del controllo DataList tramite le relative proprietà RepeatColumns e RepeatDirection.


## <a name="introduction"></a>Introduzione

Gli esempi di DataList è visualizzata nelle ultime due esercitazioni ve è eseguito il rendering di ogni record dall'origine dati come una riga in HTML una sola colonna `<table>`. Benché questo sia il comportamento di DataList predefinito, è molto semplice personalizzare la visualizzazione del controllo DataList in modo che gli elementi di origine dati sono suddivisi in una tabella a più colonne, con più righe. Inoltre, è s possibile disporre di tutti i dati di origine gli elementi visualizzati in un singola riga e a più colonne di DataList.

È possibile personalizzare il layout di DataList s tramite il relativo `RepeatColumns` e `RepeatDirection` proprietà, che, a indicare rispettivamente il numero di colonne viene sottoposti a rendering e se tali elementi sono disposti orizzontalmente o verticalmente. Figura 1, ad esempio, Mostra un controllo DataList che visualizza informazioni sul prodotto in una tabella con tre colonne.


[![DataList mostra tre prodotti per ogni riga](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image1.png)

**Figura 1**: il DataList mostra tre prodotti per ogni riga ([fare clic per visualizzare l'immagine ingrandita](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image3.png))


Tramite la visualizzazione di più elementi di origine dati per ogni riga, DataList utilizzabili in modo più efficace lo spazio orizzontale dello schermo. In questa breve esercitazione si esamineranno queste due proprietà DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Passaggio 1: Visualizzazione di informazioni sul prodotto in un controllo DataList

Prima di esaminare il `RepeatColumns` e `RepeatDirection` proprietà, s consentono di creare innanzitutto un controllo DataList in nostra pagina che elenca le informazioni prodotto utilizzando il layout di tabella a colonna singola, con più righe standard. Per questo esempio, consentire s visualizzare il nome del prodotto s, categoria e prezzo utilizzando il markup seguente:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample1.html)]

Si viene illustrato come associare dati a un controllo DataList negli esempi precedenti, pertanto sarà spostare rapidamente questi passaggi. Aprire il `RepeatColumnAndDirection.aspx` nella pagina di `DataListRepeaterBasics` cartelle e trascinare un controllo DataList dalla casella degli strumenti nella finestra di progettazione. Smart tag DataList s, scegliere di creare un nuovo oggetto ObjectDataSource e configurarlo per estrarre i dati di `ProductsBLL` classe s `GetProducts` metodo, se si sceglie (nessuno) l'opzione dalla procedura guidata s INSERT, UPDATE ed eliminare le schede.

Dopo la creazione e associazione ObjectDataSource nuovo a DataList, Visual Studio crea automaticamente un `ItemTemplate` che visualizza il nome e il valore per ognuno dei campi dati di prodotto. Regolare il `ItemTemplate` direttamente tramite markup dichiarativo o dai modelli di modificare l'opzione nello smart tag DataList s in modo che utilizzi il codice illustrato in precedenza, sostituendo il *Product Name*, *nome categoria* , e *prezzo* testo con controlli etichetta che utilizzano la sintassi di associazione di dati appropriato per assegnare valori a loro `Text` proprietà. Dopo aver aggiornato il `ItemTemplate`, markup dichiarativo pagina s dovrebbe essere simile al seguente:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample2.aspx)]

Si noti che se viene incluso un identificatore di formato nel `Eval` sintassi di associazione dati per il `UnitPrice`, la formattazione del valore restituito come valuta -`Eval("UnitPrice", "{0:C}").`

Richiedere qualche istante per visitare la pagina in un browser. Come illustrato nella figura 2, DataList esegue il rendering come una tabella a colonna singola, con più righe di prodotti.


[![Per impostazione predefinita, il rendering di DataList come una tabella a colonna singola, con più righe](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image4.png)

**Figura 2**: per impostazione predefinita, DataList esegue il rendering come una colonna singola, tabella con più righe ([fare clic per visualizzare l'immagine ingrandita](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Passaggio 2: Modificare la direzione del Layout s DataList

Durante il comportamento predefinito per il controllo DataList è per disporre gli elementi verticalmente in una tabella a colonna singola, con più righe, questo comportamento può facilmente essere modificato tramite DataList s [ `RepeatDirection` proprietà](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). Il `RepeatDirection` proprietà può accettare uno dei due valori possibili: `Horizontal` o `Vertical` (impostazione predefinita).

Modificando il `RepeatDirection` proprietà `Vertical` a `Horizontal`, DataList esegue il rendering i record in una singola riga, la creazione di una colonna per ogni elemento dell'origine dati. Per illustrare questo effetto, fare clic sul controllo DataList nella finestra di progettazione e quindi, dalla finestra delle proprietà, modificare il `RepeatDirection` proprietà `Vertical` a `Horiztonal`. Immediatamente in questo caso, la finestra di progettazione adatta DataList s, creazione di un'interfaccia a riga singola, a più colonne (vedere la figura 3).


[![Gli elementi RepeatDirection determina come la direzione per la proprietà DataList s sono posizionati Out](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image7.png)

**Figura 3**: il `RepeatDirection` proprietà determina come gli elementi di direzione s DataList sono posizionati Out ([fare clic per visualizzare l'immagine ingrandita](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image9.png))


La visualizzazione di piccole quantità di dati, una singola riga, a più colonne tabella potrebbe essere una soluzione ideale per ottimizzare l'area dello schermo. Per i volumi di dati di grandi dimensioni, tuttavia, una singola riga richiederà numerose colonne, quali inserimenti, gli elementi che t può rientri nello schermo a destra. La figura 4 mostra i prodotti quando sottoposto a rendering in un singola riga di DataList. Poiché sono presenti molti prodotti (superato l'80), l'utente deve scorrere fino all'estremità destra per visualizzare informazioni su ciascuno dei prodotti.


[![Per le origini dati sufficientemente grande DataList una singola colonna richiederà lo scorrimento orizzontale](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image10.png)

**Figura 4**: per sufficientemente grande le origini dati, una singola colonna DataList verrà richiedono lo scorrimento orizzontale ([fare clic per visualizzare l'immagine ingrandita](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Passaggio 3: La visualizzazione dei dati in una tabella a più colonne, con più righe

Per creare un controllo DataList a più colonne, con più righe, è necessario impostare il [ `RepeatColumns` proprietà](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) al numero di colonne da visualizzare. Per impostazione predefinita, il `RepeatColumns` è impostata su 0, che provoca il DataList visualizzare tutti gli elementi in una singola riga o una colonna (in base al valore di `RepeatDirection` proprietà).

Per questo esempio, consentire s visualizzare tre prodotti per ogni riga della tabella. Pertanto, impostare il `RepeatColumns` proprietà su 3. Dopo aver apportato questa modifica, richiedere qualche istante per visualizzare i risultati in un browser. Come illustrato nella figura 5, i prodotti sono ora elencati in una tabella con più righe con tre colonne.


[![Vengono visualizzati tre prodotti per ogni riga](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image13.png)

**Figura 5**: tre prodotti vengono visualizzati per ogni riga ([fare clic per visualizzare l'immagine ingrandita](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image15.png))


Il `RepeatDirection` proprietà influisce sulla disposizione degli elementi nella DataList. Figura 5 mostra i risultati con il `RepeatDirection` proprietà impostata su `Horizontal`. Si noti che i primi tre prodotti Chai, registrazione e sciroppo di prodotto sono posizionati da sinistra a destra e dall'alto verso il basso. I tre prodotti (a partire da Chef Anton s Cajun Seasoning) vengono visualizzati in una riga di sotto le prime tre. Modifica il `RepeatDirection` proprietà nuovamente a `Vertical`, tuttavia, viene disposto questi prodotti dall'alto verso il basso, da sinistra a destra, come illustrato nella figura 6.


[![In questo caso, i prodotti sono posizionati Out verticale](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image16.png)

**Figura 6**: in questo caso, i prodotti sono posizionati Out verticalmente ([fare clic per visualizzare l'immagine ingrandita](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image18.png))


Il numero di righe visualizzate nella tabella risultante dipende dal numero totale di record associato a DataList. La precisione è s diviso il limite massimo del numero totale di elementi dell'origine dati il `RepeatColumns` valore della proprietà. Poiché il `Products` table presenta attualmente 84 prodotti, che è divisibile per 3, sono presenti 28 righe. Se il numero di elementi nell'origine dati e `RepeatColumns` valore della proprietà non è divisibile, quindi l'ultima riga o colonna avrà le celle vuote. Se il `RepeatDirection` è impostato su `Vertical`, l'ultima colonna avrà le celle vuote; se `RepeatDirection` è `Horizontal`, l'ultima riga avrà le celle vuote.

## <a name="summary"></a>Riepilogo

DataList, per impostazione predefinita, vengono elencati gli elementi in una tabella a colonna singola, con più righe, che riproduce il layout di un controllo GridView con un singolo TemplateField. Durante il layout predefinito è accettabile, è possibile ingrandire area dello schermo mediante la visualizzazione di più elementi di origine dati per ogni riga. Tale scopo è sufficiente impostare DataList s `RepeatColumns` proprietà per il numero di colonne da visualizzare per ogni riga. Inoltre, il controllo DataList s `RepeatDirection` proprietà può essere utilizzata per indicare se il contenuto della tabella a più colonne, con più righe debba essere disposti orizzontalmente da sinistra a destra e dall'alto verso il basso o verticalmente, dall'alto verso il basso, da sinistra a destra.

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata John Suru. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
[Successivo](nested-data-web-controls-cs.md)
