---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: Visualizzazione di più record per ogni riga con il controllo DataList (VB) | Microsoft Docs
author: rick-anderson
description: In questa breve esercitazione verrà illustrato come personalizzare il layout di DataList tramite le relative proprietà RepeatColumns e RepeatDirection.
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 55e07159fd9d0f4c750a2522feb0538a1cfb4bea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831887"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Visualizzazione di più record per ogni riga con il controllo DataList (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) o [Scarica il PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> In questa breve esercitazione verrà illustrato come personalizzare il layout di DataList tramite le relative proprietà RepeatColumns e RepeatDirection.


## <a name="introduction"></a>Introduzione

Gli esempi di DataList è ve visto nelle ultime due esercitazioni hanno eseguito il rendering di ogni record dall'origine dati come una riga in una sola colonna HTML `<table>`. Si tratta del comportamento di DataList predefinito, è molto semplice personalizzare la visualizzazione di DataList in modo che gli elementi di origine dati vengono distribuiti in una tabella a più colonne, con più righe. Inoltre, è possibile disporre di tutti i dati di origine elementi visualizzati in un controllo DataList di singola riga e a più colonne.

È possibile personalizzare il layout di DataList s tramite il `RepeatColumns` e `RepeatDirection` proprietà che, rispettivamente, indicare il numero di colonne viene sottoposti a rendering e se tali elementi vengono disposti orizzontalmente o verticalmente. Figura 1, ad esempio, Mostra un controllo DataList che visualizza le informazioni sul prodotto in una tabella con tre colonne.


[![DataList visualizzi tre prodotti per ogni riga](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Figura 1**: il DataList mostra tre prodotti per ogni riga ([fare clic per visualizzare l'immagine con dimensioni normali](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


Mostrando più elementi di origine dati per ogni riga, DataList utilizzabili in modo più efficace lo spazio orizzontale dello schermo. In questa breve esercitazione illustra queste due proprietà DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Passaggio 1: Visualizzare le informazioni sul prodotto in un controllo DataList

Prima di esaminare i `RepeatColumns` e `RepeatDirection` proprietà, s ti permettono di creare innanzitutto un controllo DataList nella pagina che elenca le informazioni di prodotto usando il layout di tabella a colonna singola, con più righe standard. In questo esempio consentono s visualizzare la s nome prodotto, categoria e prezzo con il markup seguente:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

È stato visto come associare dati a un controllo DataList negli esempi precedenti, pertanto si passerà attraverso questi passaggi rapidamente. Iniziare aprendo il `RepeatColumnAndDirection.aspx` nella pagina di `DataListRepeaterBasics` cartelle e trascinare un controllo DataList dalla casella degli strumenti nella finestra di progettazione. Nello smart tag, DataList s scegliere di creare un nuovo oggetto ObjectDataSource e configurarlo per estrarre i dati dal `ProductsBLL` classe s `GetProducts` metodo, se si sceglie (nessuno) opzione dalla procedura guidata s INSERT, UPDATE ed eliminare le schede.

Dopo la creazione e binding nuovo oggetto ObjectDataSource a DataList, Visual Studio creerà automaticamente un `ItemTemplate` che visualizza il nome e il valore per ognuno dei campi di dati del prodotto. Modificare il `ItemTemplate` direttamente tramite markup dichiarativo o dai modelli di modificare l'opzione nello smart tag DataList s in modo che utilizzi il markup illustrato in precedenza, sostituendo il *Product Name*, *nome categoria* , e *prezzo* testo con i controlli di etichetta che utilizzano la sintassi di associazione dati appropriate per assegnare valori a loro `Text` proprietà. Dopo aver aggiornato il `ItemTemplate`, markup dichiarativo pagina s dovrebbe essere simile al seguente:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Si noti che se ve incluso un identificatore di formato nel `Eval` sintassi di associazione dati per il `UnitPrice`, formattazione del valore restituito come tipo currency - `Eval("UnitPrice", "{0:C}").`

Si consiglia di visitare la pagina in un browser. Come illustrato nella figura 2, DataList esegue il rendering come una tabella a colonna singola, con più righe di prodotti.


[![Per impostazione predefinita, i renderer di DataList come una tabella a colonna singola, con più righe](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Figura 2**: per impostazione predefinita, il DataList esegue il rendering come una colonna singola, tabella con più righe ([fare clic per visualizzare l'immagine con dimensioni normali](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Passaggio 2: Modificare la direzione del Layout s DataList

Durante il comportamento predefinito per il controllo DataList consiste nel disporre gli elementi verticalmente in una tabella a colonna singola, con più righe, questo comportamento può facilmente essere modificato tramite DataList 1!s [ `RepeatDirection` proprietà](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). Il `RepeatDirection` proprietà può accettare uno dei due valori possibili: `Horizontal` o `Vertical` (predefinito).

Modificando la `RepeatDirection` proprietà dal `Vertical` a `Horizontal`, DataList esegue il rendering dei relativi record in una singola riga, la creazione di una colonna per ogni elemento dell'origine dati. Per illustrare questo effetto, fare clic su DataList nella finestra di progettazione e quindi, dalla finestra delle proprietà, modificare il `RepeatDirection` proprietà dal `Vertical` a `Horiztonal`. Immediatamente in questo modo, la finestra di progettazione consente di regolare il layout di DataList s, la creazione di un'interfaccia a riga singola, a più colonne (vedere la figura 3).


[![Gli elementi RepeatDirection proprietà determina come la direzione s DataList sono layout orizzontale](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Figura 3**: il `RepeatDirection` proprietà determina la modalità di direzione s DataList sono elementi Laid Out ([fare clic per visualizzare l'immagine con dimensioni normali](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


La visualizzazione di piccole quantità di dati, un'unica riga, a più colonne tabella potrebbe essere un modo ideale per ingrandire l'area dello schermo. Per più grandi volumi di dati, tuttavia, una singola riga richiederanno numerose colonne, che le invia gli elementi che t can rientri nello schermo a destra. Figura 4 mostra i prodotti quando viene eseguito il rendering in un controllo DataList di riga singola. Poiché esistono molti prodotti (oltre 80), l'utente deve scorrere fino all'estremità destra per visualizzare informazioni su ciascuno dei prodotti.


[![Per le origini dati sufficientemente grandi, un controllo DataList di colonna singola richiederà uno scorrimento orizzontale](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Figura 4**: per sufficientemente grandi origini dati, una singola colonna DataList verrà richiedono lo scorrimento orizzontale ([fare clic per visualizzare l'immagine con dimensioni normali](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Passaggio 3: Visualizzazione dei dati in una tabella a più colonne, con più righe

Per creare un controllo DataList a più colonne, con più righe, è necessario impostare il [ `RepeatColumns` proprietà](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) al numero di colonne da visualizzare. Per impostazione predefinita, il `RepeatColumns` viene impostata su 0, che provoca il DataList visualizzare tutti i relativi elementi in una singola riga o una colonna (in base al valore di `RepeatDirection` proprietà).

In questo esempio consentono s visualizzare tre prodotti per ogni riga della tabella. Pertanto, impostare il `RepeatColumns` proprietà a 3. Dopo aver apportato questa modifica, si consiglia di visualizzare i risultati in un browser. Come illustrato nella figura 5, i prodotti sono ora elencati in una tabella con più righe con tre colonne.


[![Per ogni riga vengono visualizzati tre prodotti](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Figura 5**: per ogni riga vengono visualizzati tre prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


Il `RepeatDirection` proprietà influisce sulla modalità di disposizione gli elementi in DataList. Figura 5 mostra i risultati con il `RepeatDirection` impostata su `Horizontal`. Si noti che i primi tre prodotti Chai, registrazione di modifiche e sciroppo di prodotto vengono disposti da sinistra a destra, dall'alto verso il basso. I tre prodotti (inizia con s Chef Anton Seasoning Cajun) vengono visualizzati in una riga di sotto le prime tre. Modifica il `RepeatDirection` proprietà verso `Vertical`, tuttavia, questi prodotti dall'alto verso il basso viene disposto, da sinistra a destra, come illustrato nella figura 6.


[![In questo caso, i prodotti sono layout orizzontale verticale](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Figura 6**: in questo caso, i prodotti sono layout orizzontale verticale ([fare clic per visualizzare l'immagine con dimensioni normali](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


Il numero di righe visualizzate nella tabella risultante dipende dal numero totale di record associato a DataList. Precisamente, si s il limite massimo del numero totale di elementi dell'origine dati viene diviso il `RepeatColumns` valore della proprietà. Poiché il `Products` tabella ha attualmente 84 prodotti, ovvero divisibile per 3, sono presenti 28 righe. Se il numero di elementi nell'origine dati e il `RepeatColumns` valore della proprietà non sono divisibili, quindi l'ultima riga o colonna avrà le celle vuote. Se il `RepeatDirection` è impostata su `Vertical`, quindi l'ultima colonna avrà le celle vuote; se `RepeatDirection` è `Horizontal`, l'ultima riga avrà le celle vuote.

## <a name="summary"></a>Riepilogo

DataList, per impostazione predefinita, vengono elencati gli elementi in una tabella a colonna singola, con più righe, che riproduce il layout di un controllo GridView con un TemplateField singolo. Mentre questo layout predefinita è accettabile, è possibile ottimizzare sullo schermo tramite la visualizzazione di più elementi di origine dati per ogni riga. Questa operazione è semplicemente una questione di impostazione DataList s `RepeatColumns` proprietà per il numero di colonne da visualizzare per ogni riga. Inoltre, il controllo DataList s `RepeatDirection` proprietà può essere utilizzata per indicare se il contenuto della tabella a più colonne, con più righe deve essere disposto in senso orizzontale da sinistra a destra e dall'alto verso il basso o verticalmente, dall'alto verso il basso, da sinistra a destra.

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata John Suru. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [Successivo](nested-data-web-controls-vb.md)
