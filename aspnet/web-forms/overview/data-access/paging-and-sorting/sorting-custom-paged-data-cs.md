---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Ordinamento con paging personalizzato dei dati (c#) | Microsoft Docs
author: rick-anderson
description: Nell'esercitazione precedente abbiamo appreso come implementare il paging personalizzato quando presentating dati in una pagina web. In questa esercitazione viene illustrato come estendere il precedente...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: cc0ca571957f29afd7e3a2657e58272f804fc6ef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825968"
---
<a name="sorting-custom-paged-data-c"></a>Ordinamento con paging personalizzato dei dati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) o [Scarica il PDF](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> Nell'esercitazione precedente abbiamo appreso come implementare il paging personalizzato quando presentating dati in una pagina web. In questa esercitazione viene illustrato come estendere l'esempio precedente in modo da supportare il paging personalizzato dell'ordinamento.


## <a name="introduction"></a>Introduzione

Rispetto al paging predefinito, il paging personalizzato può migliorare le prestazioni del paging dei dati da vari ordini di grandezza, rendendo la scelta di implementazione di paging standard de facto di paging quando il paging in grandi quantità di dati personalizzato. Implementare il paging personalizzato è più complessa rispetto all'implementazione di paging predefinito, tuttavia, in particolare quando si aggiunge l'ordinamento alla combinazione di. In questa esercitazione è possibile estendere l'esempio da quello precedente per includere il supporto per l'ordinamento *e* paging personalizzato.

> [!NOTE]
> Poiché questa esercitazione si basa su quella precedente, prima di inizio si consiglia di copiare la sintassi dichiarativa all'interno di `<asp:Content>` elemento dalla pagina web precedente esercitazione s (`EfficientPaging.aspx`) e incollarlo tra il `<asp:Content>` elemento il `SortParameter.aspx` pagina. Fare riferimento al passaggio 1 del [aggiunta di controlli di convalida per la modifica e inserimento di interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) esercitazione per una discussione più dettagliata sulle replicate le funzionalità di una pagina ASP.NET a un altro.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Passaggio 1: Riesaminati più la tecnica di Paging personalizzato

Per il paging personalizzato per il corretto funzionamento, è necessario implementare alcuni tecnica che è possibile recuperare in modo efficiente un subset di record specificato i parametri di indice di riga iniziale e massimo di righe specifico. Esistono alcune tecniche che possono essere utilizzate per raggiungere questo obiettivo. Nell'esercitazione precedente abbiamo esaminati questa operazione usando Microsoft SQL Server 2005 s nuovo `ROW_NUMBER()` funzione di rango. In breve, il `ROW_NUMBER()` funzione di rango viene assegnato un numero di riga per ogni riga restituita da una query che viene valutata da una sequenza di ordinamento specificata. Subset di record appropriato viene quindi ottenuto tramite la restituzione di una particolare sezione dei risultati numerati. La query seguente illustra come usare questa tecnica per tali prodotti numerati 11 a 20 rango i risultati ordinati in ordine alfabetico per restituire il `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Questa tecnica funziona bene per il paging mediante un ordinamento specifico (`ProductName` ordinati in ordine alfabetico, in questo caso), ma la query deve essere modificata per mostrare i risultati ordinati in base a un'espressione di ordinamento diversi. In teoria, la query precedente può essere riscritto per utilizzare un parametro nel `OVER` clausola, come illustrato di seguito:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Purtroppo, con parametri `ORDER BY` clausole non sono consentite. In alternativa, è necessario creare una stored procedure che accetta un `@sortExpression` parametro di input, ma utilizza una delle soluzioni alternative seguenti:

- Scrivere query a livello di codice per ognuna delle espressioni di ordinamento che possono essere usate; usare quindi `IF/ELSE` istruzioni T-SQL per determinare quali query da eseguire.
- Usare un `CASE` istruzione per fornire dinamica `ORDER BY` espressioni in base il `@sortExpressio` n parametro di input; vedere usato alla sezione in modo dinamico i risultati della Query di ordinamento nella [potenza di SQL `CASE` istruzioni](http://www.4guysfromrolla.com/webtech/102704-1.shtml) per altre informazioni.
- Creare la query appropriata sotto forma di stringa nella stored procedure e quindi usare [il `sp_executesql` stored procedure di sistema](https://msdn.microsoft.com/library/ms188001.aspx) per eseguire la query dinamica.

Ognuna di queste soluzioni alternative presenta alcuni svantaggi. La prima opzione non è gestibile come gli altri due perché richiede che si crea una query per ogni espressione di ordinamento possibili. Pertanto, se successivamente si decide di aggiungere campi nuovi e ordinabili per il controllo GridView è anche necessario tornare indietro e aggiornare la stored procedure. Il secondo approccio ha alcune sottigliezze che presentano problemi di prestazioni quando si ordinano le colonne del database non di tipo stringa e risentono inoltre gli stessi problemi di gestibilità del primo. E la terza scelta, che Usa istruzioni SQL dinamiche, introduce il rischio di attacchi SQL injection, se un utente malintenzionato è in grado di eseguire la stored procedure passando i valori di parametro di input di loro scelta.

Anche se nessuno di questi approcci è perfetto, ritengo che la terza opzione è la migliore dei tre. Con il relativo utilizzo di istruzioni SQL dinamiche, offre un livello di flessibilità che non li supportano altri due. Un attacco SQL injection, inoltre, può essere sfruttato soltanto se un utente malintenzionato è in grado di eseguire la stored procedure passando i parametri di input di sua scelta. Poiché il campo DAL Usa le query con parametri, ADO.NET proteggerà i parametri che vengono inviati al database tramite l'architettura, vale a dire che le vulnerabilità di attacco SQL injection esiste solo se l'autore dell'attacco può eseguire direttamente la stored procedure.

Per implementare questa funzionalità, creare una nuova stored procedure nel database Northwind denominato `GetProductsPagedAndSorted`. Questa stored procedure deve accettare tre parametri di input: `@sortExpression`, un parametro di input di tipo `nvarchar(100`) che specifica la modalità con cui i risultati devono essere ordinati e viene inserito direttamente dopo le `ORDER BY` testo nel `OVER` clausola; che `@startRowIndex` e `@maximumRows`, gli stessi parametri di input due integer dal `GetProductsPaged` stored procedure esaminate nell'esercitazione precedente. Creare il `GetProductsPagedAndSorted` stored procedure con lo script seguente:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

La stored procedure avvia, verificare che un valore per il `@sortExpression` parametro è stato specificato. Se è manca, i risultati sono classificati in base al `ProductID`. Successivamente, viene costruita la query SQL dinamica. Si noti che qui la query SQL dinamica differisce leggermente dal nostro query precedenti consente di recuperare tutte le righe dalla tabella Products. Negli esempi precedenti, abbiamo ottenuto ogni categoria di prodotto s associata a s e fornitore nomi s utilizzando una sottoquery. Questa decisione è stata presa nel [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione e avveniva anziché `JOIN` s poiché TableAdapter non può creare automaticamente l'inserimento associato, aggiornare ed eliminare i metodi per questo tipo query. Il `GetProductsPagedAndSorted` stored procedure, tuttavia, devono utilizzare `JOIN` s per i risultati ordinati in base a nomi di categoria o fornitore.

Questa query dinamica viene creato concatenando le parti di query statica e il `@sortExpression`, `@startRowIndex`, e `@maximumRows` parametri. Poiché `@startRowIndex` e `@maximumRows` sono parametri di tipo integer, devono essere convertite in nvarchars per poter essere concatenati correttamente. Dopo che è stata costruita questa query SQL dinamica, viene eseguita tramite `sp_executesql`.

Si consiglia di testare questa stored procedure con valori diversi per il `@sortExpression`, `@startRowIndex`, e `@maximumRows` parametri. Da Esplora Server, fare clic sul nome della stored procedure, quindi scegliere Esegui. Verrà visualizzata la finestra di dialogo Esegui Stored Procedure in cui è possibile immettere i parametri di input (vedere la figura 1). Per ordinare i risultati per il nome della categoria, CategoryName per usare il `@sortExpression` valore del parametro; per il nome della società supplier s per ordinare, usare CompanyName. Dopo aver specificato i valori dei parametri, fare clic su OK. I risultati vengono visualizzati nella finestra di Output. Figura 2 vengono mostrati i risultati prodotti restituzione classificate 11 a 20 ordinamento in base il `UnitPrice` in ordine decrescente.


![Provare diversi valori per le Stored Procedure s tre parametri di Input](sorting-custom-paged-data-cs/_static/image1.png)

**Figura 1**: provare diversi valori per la Stored Procedure s tre parametri di Input


[![La Stored Procedure s i risultati vengono visualizzati nella finestra di Output](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Figura 2**: la Stored Procedure s i risultati vengono visualizzati nella finestra di Output ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-custom-paged-data-cs/_static/image4.png))


> [!NOTE]
> Quando i risultati di classificazione da specificato `ORDER BY` colonna il `OVER` clausola, SQL Server deve ordinare i risultati. Questa è un'operazione rapida se è presente un indice cluster tramite le colonne vengono ordinati i risultati da o se è presente una copertura di indice, ma può essere più costoso in caso contrario. Per migliorare le prestazioni delle query sufficientemente grande, considerare l'aggiunta di un indice non cluster per la colonna mediante il quale i risultati vengono ordinati in base. Fare riferimento a [funzioni di rango e le prestazioni di SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) per altri dettagli.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Passaggio 2: Aumentare l'accesso ai dati e i livelli di logica di Business

Con la `GetProductsPagedAndSorted` stored procedure creata, il passaggio successivo consiste nel fornire un mezzo per eseguire la stored procedure tramite l'architettura dell'applicazione. Questo comporta l'aggiunta di un metodo appropriato a DAL e BLL. Let s iniziare aggiungendo un metodo per DAL. Aprire il `Northwind.xsd` DataSet tipizzato, pulsante destro del mouse su di `ProductsTableAdapter`e scegliere l'opzione di Query aggiunta dal menu di scelta rapida. Come è stato fatto nell'esercitazione precedente, si desidera configurare questo nuovo metodo per usare una stored procedure esistente - `GetProductsPagedAndSorted`, in questo caso. Per iniziare, che indica che il nuovo metodo TableAdapter utilizzare una stored procedure esistente.


![Scegliere di usare una Stored Procedure esistente](sorting-custom-paged-data-cs/_static/image5.png)

**Figura 3**: scegliere di usare una Stored Procedure esistente


Per specificare la stored procedure da usare, selezionare il `GetProductsPagedAndSorted` stored procedure nell'elenco a discesa nella schermata successiva.


![Usare il GetProductsPagedAndSorted Stored Procedure](sorting-custom-paged-data-cs/_static/image6.png)

**Figura 4**: usare il GetProductsPagedAndSorted Stored Procedure


Questa stored procedure restituisce un set di record, perché i relativi risultati, quindi, nella schermata successiva, indicano che vengono restituiti i dati tabulari.


![Indicare che la Stored Procedure restituisce dati tabulari](sorting-custom-paged-data-cs/_static/image7.png)

**Figura 5**: indica che la Stored Procedure restituisce dati tabulari


Infine, creare i metodi che usano entrambi il riempimento DAL DataTable e restituire modelli un DataTable, i metodi di denominazione `FillPagedAndSorted` e `GetProductsPagedAndSorted`, rispettivamente.


![Scegliere i nomi di metodi](sorting-custom-paged-data-cs/_static/image8.png)

**Figura 6**: scegliere i nomi di metodi


Ora che è stato esteso DAL, sono pronti per attivare per il livello BLL. Aprire il `ProductsBLL` file di classe e aggiungere un nuovo metodo `GetProductsPagedAndSorted`. Questo metodo deve accettare tre parametri di input `sortExpression`, `startRowIndex`, e `maximumRows` e deve semplicemente chiamare verso il basso in oggetti DAL `GetProductsPagedAndSorted` (metodo), come illustrato di seguito:


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Passaggio 3: Configurazione di ObjectDataSource a passata nel parametro SortExpression

Visto ampliata DAL e BLL per includere i metodi che utilizzano le `GetProductsPagedAndSorted` stored procedure, tutti che rimane consiste nel configurare ObjectDataSource nel `SortParameter.aspx` pagina da usare il nuovo metodo livello BLL e passare il `SortExpression` parametro basato sul colonna in cui l'utente ha richiesto per ordinare i risultati in base.

Iniziare modificando la s ObjectDataSource `SelectMethod` dal `GetProductsPaged` a `GetProductsPagedAndSorted`. Questa operazione può essere eseguita tramite la procedura guidata Configura origine dati, dalla finestra proprietà o direttamente tramite la sintassi dichiarativa. Successivamente, è necessario specificare un valore per s ObjectDataSource [ `SortParameterName` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Se questa proprietà è impostata, l'oggetto ObjectDataSource tenta di passare in s GridView `SortExpression` proprietà per il `SelectMethod`. In particolare, ObjectDataSource Cerca un parametro di input il cui nome è uguale al valore della `SortParameterName` proprietà. Poiché la s BLL `GetProductsPagedAndSorted` metodo contiene il parametro di input espressione ordinamento denominato `sortExpression`, impostare la s ObjectDataSource `SortExpression` proprietà sortExpression.

Dopo aver apportato queste due modifiche, la sintassi dichiarativa s ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Come con l'esercitazione precedente, assicurarsi che ObjectDataSource *non* includono i parametri di input sortExpression, startRowIndex o maximumRows nella propria raccolta SelectParameters.


Per abilitare l'ordinamento in GridView, è sufficiente selezionare la casella di controllo Abilita ordinamento nel controllo GridView s smart tag, che imposta la s GridView `AllowSorting` proprietà `true` e che causano il testo dell'intestazione per ogni colonna deve essere eseguito come un controllo LinkButton. Quando l'utente finale fa clic su uno dell'intestazione LinkButton, previsioni un postback e attendere la procedura seguente:

1. Gli aggiornamenti di GridView relativi [ `SortExpression` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) al valore della `SortExpression` del campo è stato fatto clic con il collegamento di intestazione
2. ObjectDataSource richiama s BLL `GetProductsPagedAndSorted` , passando in s GridView `SortExpression` proprietà come valore per il metodo s `sortExpression` parametro di input (insieme a appropriato `startRowIndex` e `maximumRows` i valori dei parametri di input)
3. Il livello BLL richiama s DAL `GetProductsPagedAndSorted` (metodo)
4. Viene eseguito DAL `GetProductsPagedAndSorted` stored procedure, passando il `@sortExpression` parametro (insieme al `@startRowIndex` e `@maximumRows` i valori dei parametri di input)
5. La stored procedure restituisce il subset di dati appropriato per il livello BLL, che lo restituisce a ObjectDataSource; Questi dati vengano quindi associati al GridView viene eseguito il rendering in HTML e inviati all'utente finale

Figura 7 mostra la prima pagina dei risultati quando vengono ordinati i `UnitPrice` in ordine crescente.


[![I risultati sono ordinati per l'elemento UnitPrice](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Figura 7**: il risultati sono ordinati per il prezzo unitario ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-custom-paged-data-cs/_static/image11.png))


Sebbene l'attuale implementazione può ordinare in modo corretto i risultati in base al nome del prodotto, nome della categoria, quantità per l'unità e il prezzo unitario, il tentativo di ordinare i risultati dal fornitore il nome viene importata in un'eccezione di runtime (vedere la figura 8).


![Tentativo di ordinare i risultati in base ai risultati di fornitori la seguente eccezione di Runtime](sorting-custom-paged-data-cs/_static/image12.png)

**Figura 8**: tentativo di ordinare i risultati in base ai risultati di fornitori la seguente eccezione di Runtime


Questa eccezione si verifica perché il `SortExpression` di istanze della classe GridView `SupplierName` BoundField è impostato su `SupplierName`. Tuttavia, il fornitore s nome nel `Suppliers` tabella viene effettivamente chiamata `CompanyName` è stato utilizzato un alias questo nome di colonna come `SupplierName`. Tuttavia, il `OVER` clausola utilizzata dal `ROW_NUMBER()` funzione non è possibile usare l'alias e devono usare il nome di colonna effettivi. Pertanto, modificare il `SupplierName` BoundField s `SortExpression` da SupplierName su CompanyName (vedere la figura 9). Come illustrato nella figura 10, dopo questa modifica i risultati possono essere ordinati in base al fornitore.


![Modifica SortExpression s SupplierName BoundField su CompanyName](sorting-custom-paged-data-cs/_static/image13.png)

**Figura 9**: modificare SortExpression s SupplierName BoundField in CompanyName


[![Ora i risultati possono essere ordinati dal fornitore](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Figura 10**: il risultati ora possono essere ordinate dal fornitore ([fare clic per visualizzare l'immagine con dimensioni normali](sorting-custom-paged-data-cs/_static/image16.png))


## <a name="summary"></a>Riepilogo

L'implementazione di paging personalizzata che viene esaminati nell'esercitazione precedente è necessario specificare l'ordine con cui i risultati sono da ordinare in fase di progettazione. In breve, ciò significava che l'implementazione di paging personalizzata che abbiamo implementato è stato possibile, allo stesso tempo, fornisce funzionalità di ordinamento. In questa esercitazione si consentono di superare questa limitazione estendendo la stored procedure dalla prima per includere un `@sortExpression` parametro di input mediante il quale potrebbero essere ordinati i risultati.

Dopo la creazione di questa stored procedure e la creazione di nuovi metodi nella DAL e BLL, siamo in grado di implementare un controllo GridView che offerte entrambi l'ordinamento e paging tramite la configurazione di ObjectDataSource per passare la s GridView corrente personalizzate `SortExpression` proprietà per il livello BLL `SelectMethod`.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Santos Carlos. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](efficiently-paging-through-large-amounts-of-data-cs.md)
> [Successivo](creating-a-customized-sorting-user-interface-cs.md)
