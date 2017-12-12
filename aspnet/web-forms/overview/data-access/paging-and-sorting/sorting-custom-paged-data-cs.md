---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Ordinamento personalizzato di dati (c#) di paging | Documenti Microsoft
author: rick-anderson
description: "Nell'esercitazione precedente è stato descritto come implementare il paging personalizzato quando presentating dati in una pagina web. In questa esercitazione viene illustrato come estendere il precedente..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: f171929da3610f70f3641030d9a5fdb88f610f7f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="sorting-custom-paged-data-c"></a>Ordinamento personalizzato paging dei dati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) o [Scarica il PDF](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> Nell'esercitazione precedente è stato descritto come implementare il paging personalizzato quando presentating dati in una pagina web. In questa esercitazione viene illustrato come estendere l'esempio precedente in modo da supportare il paging personalizzato dell'ordinamento.


## <a name="introduction"></a>Introduzione

Rispetto al paging predefinito, il paging personalizzato può migliorare le prestazioni del paging dei dati per più ordini di grandezza, rendendo la scelta di implementazione di paging fatto di paging quando il paging di grandi quantità di dati personalizzato. Implementare il paging personalizzato è più complessa dell'implementazione predefinita paging, tuttavia, in particolare durante l'aggiunta di ordinamento alla combinazione di. In questa esercitazione è possibile estendere l'esempio da quella precedente per includere il supporto per l'ordinamento *e* il paging personalizzato.

> [!NOTE]
> Poiché in questa esercitazione si basa su quello precedente, prima di inizio dedicare alcuni minuti per copiare la sintassi dichiarativa all'interno di `<asp:Content>` elemento dalla pagina web precedente esercitazione s (`EfficientPaging.aspx`) e incollarlo tra il `<asp:Content>` elemento il `SortParameter.aspx` pagina. Fare riferimento al passaggio 1 del [aggiunta di controlli di convalida per la modifica e inserimento di interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) esercitazione per una descrizione più dettagliata in cui vengono replicate le funzionalità di una pagina ASP.NET a un altro.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>Passaggio 1: Riesaminare la tecnica di Paging personalizzata

Per il paging personalizzato per il corretto funzionamento, è necessario implementare una tecnica che è possibile acquisire in modo efficiente un particolare subset di record di base ai parametri di indice di riga iniziale e massimo di righe. Esistono alcune tecniche che può essere utilizzato per raggiungere questo obiettivo. Nell'esercitazione precedente è stato esaminato il completamento di questa utilizzando Microsoft SQL Server 2005 s nuovo `ROW_NUMBER()` funzione di rango. In breve, il `ROW_NUMBER()` funzione di rango assegnato un numero di riga per ogni riga restituita da una query che è classificata in base a un criterio di ordinamento specificato. Il subset di record appropriato viene quindi ottenuto tramite la restituzione di una particolare sezione dei risultati numerati. La query seguente viene illustrato come utilizzare questa tecnica per restituire i prodotti numerati 11 a 20 classificazione i risultati ordinati in ordine alfabetico per il `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Questa tecnica funziona anche per il paging mediante un ordinamento specifico (`ProductName` in ordine alfabetico, in questo caso), ma la query deve essere modificata per visualizzare i risultati ordinati in base a un'espressione di ordinamento diverso. Idealmente, la query precedente potrebbe essere riscritto per utilizzare un parametro nel `OVER` clausola, come illustrato di seguito:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Purtroppo, con parametri `ORDER BY` clausole non sono consentite. In alternativa, è necessario creare una stored procedure che accetta un `@sortExpression` parametro di input, ma utilizza una delle operazioni seguenti:

- Scrivere query hardcoded per ciascuna delle espressioni di ordinamento che possono essere usate; Utilizzare quindi `IF/ELSE` istruzioni T-SQL per determinare la query da eseguire.
- Utilizzare un `CASE` istruzione per fornire dinamica `ORDER BY` espressioni in base il `@sortExpressio` n, utilizzato per la sezione in modo dinamico i risultati di Query dell'ordinamento in come parametro di input [Power di SQL `CASE` istruzioni](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Per ulteriori informazioni.
- Creare la query appropriata sotto forma di stringa nella stored procedure e quindi utilizzare [il `sp_executesql` stored procedure di sistema](https://msdn.microsoft.com/en-us/library/ms188001.aspx) per eseguire la query dinamica.

Ognuna di queste soluzioni alternative presenta alcuni svantaggi. La prima opzione non è gestibile come gli altri due perché richiede la creazione di una query per ogni espressione di ordinamento possibili. Pertanto, se successivamente si decide di aggiungere i campi di nuovo, ordinabili a GridView è anche necessario tornare indietro e aggiornare la stored procedure. Il secondo approccio ha alcune sfumature che presentano problemi di prestazioni quando si ordinano le colonne del database non di tipo stringa e risente inoltre del primo agli stessi problemi di gestibilità. E la terza, che Usa istruzioni SQL dinamiche, introduce il rischio di attacco SQL injection, se un utente malintenzionato è in grado di eseguire la stored procedure passando i valori di parametro di input di propria scelta.

Anche se nessuno di questi approcci è perfetto, ritengo che la terza opzione è la più di tre. Con il relativo utilizzo di SQL dinamico, offre un livello di flessibilità, che non gli altri due. Un attacco SQL injection, inoltre, può essere sfruttato solo se un utente malintenzionato è in grado di eseguire la stored procedure passando i parametri di input di sua scelta. Poiché DAL utilizza le query con parametri, ADO.NET proteggerà i parametri che vengono inviati al database tramite l'architettura, vale a dire che la vulnerabilità di attacco SQL injection esiste solo se l'utente malintenzionato può eseguire direttamente la stored procedure.

Per implementare questa funzionalità, creare una nuova stored procedure nel database di Northwind denominato `GetProductsPagedAndSorted`. Questa stored procedure deve accettare tre parametri di input: `@sortExpression`, un parametro di input di tipo `nvarchar(100`) che specifica la modalità con cui i risultati devono essere ordinati e viene inserito direttamente dopo il `ORDER BY` testo il `OVER` clausola; e `@startRowIndex` e `@maximumRows`, gli stessi parametri di input due integer dal `GetProductsPaged` stored procedure esaminate nell'esercitazione precedente. Creare il `GetProductsPagedAndSorted` stored procedure utilizzando lo script seguente:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

Avvia la stored procedure, verificare che un valore per il `@sortExpression` parametro è stato specificato. Se è presente, i risultati vengono classificati in base `ProductID`. Successivamente, viene costruita la query SQL dinamica. Si noti che la query SQL dinamica qui differisce leggermente dal nostro query precedenti consente di recuperare tutte le righe dalla tabella Products. Negli esempi precedenti, sono stati ottenuti ogni categoria di prodotto s associata a s e fornitore nomi s utilizzando una sottoquery. Questa decisione nuovamente il [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione ed è stato effettuato anziché `JOIN` s perché il TableAdapter non è possibile creare automaticamente associata insert, update e delete metodi per tale query. Il `GetProductsPagedAndSorted` deve tuttavia utilizzare stored procedure `JOIN` s per i risultati devono essere ordinati in base ai nomi di categoria o il fornitore.

Questa query dinamica viene creata concatenando le parti di query statica e `@sortExpression`, `@startRowIndex`, e `@maximumRows` parametri. Poiché `@startRowIndex` e `@maximumRows` sono parametri di tipo integer, devono essere convertite in nvarchars per poter essere concatenati correttamente. Una volta la query SQL dinamica è stata creata, viene eseguita tramite `sp_executesql`.

È opportuno testare questa stored procedure con valori diversi per il `@sortExpression`, `@startRowIndex`, e `@maximumRows` parametri. Da Esplora Server, fare clic sul nome della stored procedure, quindi scegliere Esegui. Verrà visualizzata la finestra di dialogo Esegui Stored Procedure in cui è possibile immettere i parametri di input (vedere la figura 1). Per ordinare i risultati per il nome della categoria, utilizzare CategoryName per i `@sortExpression` valore del parametro; per ordinare in base al nome della società fornitore s, usare CompanyName. Dopo aver specificato i valori dei parametri, fare clic su OK. I risultati vengono visualizzati nella finestra di Output. La figura 2 mostra i risultati quando la restituzione di prodotti classificati 11 a 20 ordinamento in base il `UnitPrice` in ordine decrescente.


![Provare valori diversi per il Stored Procedure s tre parametri di Input](sorting-custom-paged-data-cs/_static/image1.png)

**Figura 1**: provare diversi valori per la Stored Procedure s tre parametri di Input.


[![I dispositivi di Stored Procedure i risultati vengono visualizzati nella finestra di Output](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Figura 2**: s la Stored Procedure i risultati vengono visualizzati nella finestra di Output ([fare clic per visualizzare l'immagine ingrandita](sorting-custom-paged-data-cs/_static/image4.png))


> [!NOTE]
> Quando i risultati di classificazione da specificato `ORDER BY` colonna il `OVER` clausola, SQL Server è necessario ordinare i risultati. Questa è un'operazione rapida se è un indice cluster per le colonne dai risultati sono ordinati o se esiste una copertura di indice, ma può essere più costosi da risolvere in caso contrario. Per migliorare le prestazioni delle query sufficientemente grande, considerare l'aggiunta di un indice non cluster per la colonna da cui i risultati vengono ordinati in base. Fare riferimento a [funzioni di rango e le prestazioni in SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) per altri dettagli.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Passaggio 2: Aumento l'accesso ai dati e il livello di logica di Business

Con il `GetProductsPagedAndSorted` stored procedure creata nel passaggio successivo consiste nella consentono di eseguire la stored procedure tramite l'architettura dell'applicazione. Questo implica l'aggiunta di un metodo appropriato a DAL e BLL. Consente di s per iniziare, aggiungere un metodo per DAL. Aprire il `Northwind.xsd` DataSet tipizzato, pulsante destro del mouse su di `ProductsTableAdapter`e scegliere l'opzione Aggiungi Query dal menu di scelta rapida. Come è stato fatto nell'esercitazione precedente, si desidera configurare questo nuovo metodo per l'utilizzo di una stored procedure esistente - `GetProductsPagedAndSorted`, in questo caso. Per iniziare, che indica che si desidera far usare una stored procedure esistente con il nuovo metodo di TableAdapter.


![Scegliere di utilizzare una Stored Procedure esistente](sorting-custom-paged-data-cs/_static/image5.png)

**Figura 3**: scegliere di utilizzare una Stored Procedure esistente


Per specificare la stored procedure da utilizzare, selezionare il `GetProductsPagedAndSorted` stored procedure dall'elenco a discesa nella schermata successiva.


![Utilizzare il GetProductsPagedAndSorted Stored Procedure](sorting-custom-paged-data-cs/_static/image6.png)

**Figura 4**: utilizzare il GetProductsPagedAndSorted Stored Procedure


Questa stored procedure restituisce un set di record, come i risultati in tal caso, nella schermata successiva, indicano che vengono restituiti i dati tabulari.


![Indicare che la Stored Procedure restituisce dati in formato tabulare](sorting-custom-paged-data-cs/_static/image7.png)

**Figura 5**: indicare che la Stored Procedure restituisce dati in formato tabulare


Infine, creare i metodi che usano entrambi il riempimento DAL DataTable e restituire modelli un DataTable, i metodi di denominazione `FillPagedAndSorted` e `GetProductsPagedAndSorted`, rispettivamente.


![Scegliere i nomi di metodi](sorting-custom-paged-data-cs/_static/image8.png)

**Figura 6**: scegliere i nomi di metodi


Ora che si va esteso DAL, è nuovamente pronto per attivare il livello Business Logic. Aprire il `ProductsBLL` file di classe e aggiungere un nuovo metodo `GetProductsPagedAndSorted`. Questo metodo deve accettare tre parametri di input `sortExpression`, `startRowIndex`, e `maximumRows` e deve semplicemente chiamare verso il basso in oggetti DAL `GetProductsPagedAndSorted` (metodo), come illustrato di seguito:


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Passaggio 3: Configurazione ObjectDataSource da passare nel parametro SortExpression

Con ampliate DAL e BLL per includere metodi che utilizzano il `GetProductsPagedAndSorted` stored procedure, tutte che rimane consiste nel configurare ObjectDataSource nel `SortParameter.aspx` pagina da utilizzare il nuovo metodo BLL e passare il `SortExpression` parametro basato sul colonna in cui l'utente ha richiesto per ordinare i risultati da.

Iniziare modificando il s ObjectDataSource `SelectMethod` da `GetProductsPaged` a `GetProductsPagedAndSorted`. Questa operazione può essere eseguita tramite la configurazione guidata origine dati, la finestra proprietà o direttamente tramite la sintassi dichiarativa. Successivamente, è necessario fornire un valore per ObjectDataSource s [ `SortParameterName` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Se questa proprietà è impostata, ObjectDataSource tenta di passare in GridView s `SortExpression` proprietà per il `SelectMethod`. In particolare, un parametro di input il cui nome è uguale al valore di ricerca ObjectDataSource il `SortParameterName` proprietà. Poiché i dispositivi BLL `GetProductsPagedAndSorted` metodo ha il parametro di input espressione ordinamento denominato `sortExpression`, impostare s ObjectDataSource `SortExpression` proprietà sortExpression.

Dopo aver apportato queste due modifiche, la sintassi dichiarativa s ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Nell'esercitazione precedente, verificare che non è ObjectDataSource *non* includono i parametri di input sortExpression, startRowIndex o maximumRows nella propria raccolta SelectParameters.


Per attivare l'ordinamento in GridView, semplicemente la casella di controllo Abilita ordinamento nel controllo GridView s smart tag, che imposta il s GridView `AllowSorting` proprietà `true` e causando il testo dell'intestazione per ogni colonna deve essere eseguito come LinkButton. Quando l'utente finale fa clic su una delle intestazioni LinkButton, viene utilizzata un postback e attendere la procedura seguente:

1. Gli aggiornamenti di GridView relativo [ `SortExpression` proprietà](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) il valore di `SortExpression` del campo è stato fatto clic con il collegamento di intestazione
2. ObjectDataSource richiama s BLL `GetProductsPagedAndSorted` , passando in GridView s `SortExpression` proprietà come valore per il metodo s `sortExpression` parametro di input (insieme appropriato `startRowIndex` e `maximumRows` i valori dei parametri di input)
3. Il livello Business LOGIC richiama i dispositivi DAL `GetProductsPagedAndSorted` (metodo)
4. Viene eseguito DAL `GetProductsPagedAndSorted` stored procedure, passando il `@sortExpression` parametro (insieme al `@startRowIndex` e `@maximumRows` i valori dei parametri di input)
5. La stored procedure restituisce il subset di dati appropriato per il livello Business LOGIC, che restituisce al ObjectDataSource; Questi dati sono quindi associati a GridView, il rendering in HTML e inviati all'utente finale

Figura 7 illustra la prima pagina di risultati quando vengono ordinati i `UnitPrice` in ordine crescente.


[![I risultati vengono ordinati per il prezzo unitario](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Figura 7**: dei risultati vengono ordinati in base UnitPrice ([fare clic per visualizzare l'immagine ingrandita](sorting-custom-paged-data-cs/_static/image11.png))


Durante l'attuale implementazione può ordinare correttamente i risultati in base al nome del prodotto, nome di categoria, quantità per l'unità e il prezzo unitario, il tentativo di ordinare i risultati dal fornitore dei risultati di nome in un'eccezione di runtime (vedere la figura 8).


![Il tentativo di ordinare i risultati dal fornitore dà la seguente eccezione di Runtime](sorting-custom-paged-data-cs/_static/image12.png)

**Figura 8**: tentativo di ordinare i risultati dal fornitore dà la seguente eccezione di Runtime


Questa eccezione si verifica perché il `SortExpression` di GridView s `SupplierName` BoundField è impostato su `SupplierName`. Tuttavia, il fornitore s nome nel `Suppliers` tabella viene effettivamente chiamata `CompanyName` siamo rimasti alias il nome della colonna come `SupplierName`. Tuttavia, il `OVER` clausola utilizzata per il `ROW_NUMBER()` funzione non è possibile utilizzare l'alias e devono usare il nome di colonna effettivi. Di conseguenza, modificare il `SupplierName` BoundField s `SortExpression` da NomeFornitore su CompanyName (vedere Figura 9). Come illustrato nella figura 10, dopo questa modifica che i risultati possono essere ordinati in base al fornitore.


![Modificare SortExpression s NomeFornitore BoundField CompanyName](sorting-custom-paged-data-cs/_static/image13.png)

**Figura 9**: modificare SortExpression s NomeFornitore BoundField CompanyName


[![I risultati possono essere ordinati in base al fornitore](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Figura 10**: il risultati possono essere ordinati in base fornitore ([fare clic per visualizzare l'immagine ingrandita](sorting-custom-paged-data-cs/_static/image16.png))


## <a name="summary"></a>Riepilogo

L'implementazione di paging personalizzata che sono state esaminate nell'esercitazione precedente è necessario specificare l'ordine con cui i risultati sono stati da ordinare in fase di progettazione. In breve, in questo modo che l'implementazione di paging personalizzata che sono implementati Impossibile, allo stesso tempo, fornire funzionalità di ordinamento. In questa esercitazione è consentono di superare questa limitazione estendendo la stored procedure dalla prima per includere un `@sortExpression` parametro di input mediante il quale è possano ordinare i risultati.

Dopo la creazione di questa stored procedure e creazione di nuovi metodi in DAL e BLL, siamo in grado di implementare un controllo GridView che offerti entrambi ordinamento e paging configurando ObjectDataSource per passare in s GridView corrente personalizzato `SortExpression` proprietà al livello Business Logic `SelectMethod`.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Carlos Santos. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](efficiently-paging-through-large-amounts-of-data-cs.md)
[Successivo](creating-a-customized-sorting-user-interface-cs.md)
