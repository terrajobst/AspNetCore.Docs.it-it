---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: A livello di codice impostando i valori dei parametri di ObjectDataSource (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione verrà esaminato l'aggiunta di un metodo per il nostro DAL e BLL che accetta un solo parametro di input e restituisce i dati. Nell'esempio verrà imposta questo parametro...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: ac53d651601829b6e7d2ce312a084618a8afbb61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>A livello di codice impostando i valori dei parametri di ObjectDataSource (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) o [Scarica il PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> In questa esercitazione verrà esaminato l'aggiunta di un metodo per il nostro DAL e BLL che accetta un solo parametro di input e restituisce i dati. Nell'esempio verrà impostata a livello di codice questo parametro.


## <a name="introduction"></a>Introduzione

Come illustrato nel [esercitazione precedente](declarative-parameters-vb.md), alcune opzioni è disponibili per il passaggio in modo dichiarativo i valori dei parametri ai metodi di ObjectDataSource. Se il valore del parametro è hardcoded, di un controllo Web nella pagina, o in qualsiasi altra origine che è leggibile da un'origine dati `Parameter` dell'oggetto, ad esempio, di valore può essere associato al parametro di input senza scrivere una riga di codice.

È possibile che, tuttavia, quando il valore del parametro proviene da un'origine non è già presi in considerazione da uno dei file di dati incorporato `Parameter` oggetti. Se gli account utente è supportato il sito potrebbe essere opportuno impostare il parametro in base nell'ID utente dell'utente attualmente connesso Oppure è possibile che sia necessario personalizzare il valore del parametro prima dell'invio insieme al metodo dell'oggetto sottostante di ObjectDataSource.

Ogni volta che ObjectDataSource `Select` metodo viene richiamato prima genera ObjectDataSource relativo [evento Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Metodo dell'oggetto sottostante di ObjectDataSource viene quindi richiamato. Una volta completato l'operazione di ObjectDataSource [evento selezionati](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) generato (figura 1 illustra questa sequenza di eventi). I valori del parametro passati nel metodo dell'oggetto sottostante di ObjectDataSource possono essere impostati o in un gestore eventi per personalizzare il `Selecting` evento.


[![Viene richiamato il ObjectDataSource selezionato e metodo selezionando gli eventi vengono attivati prima e dopo il suo sottostante dell'oggetto](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Figura 1**: il ObjectDataSource `Selected` e `Selecting` gli eventi vengono attivati prima e dopo il suo sottostante dell'oggetto viene richiamato ([fare clic per visualizzare l'immagine ingrandita](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


In questa esercitazione verrà esaminato l'aggiunta di un metodo a livello logico di business che accetta un singolo parametro di input e DAL `Month`, di tipo `Integer` e restituisce un `EmployeesDataTable` oggetto popolato con i dipendenti che hanno le assunzione anniversario nell'oggetto specificato `Month`. Questo esempio verrà impostato il parametro a livello di programmazione basato sul mese corrente, che mostra un elenco di "Employee ricorrenze del mese".

Iniziamo!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Passaggio 1: Aggiunta di un metodo per`EmployeesTableAdapter`

Per il primo esempio è necessario aggiungere un modo per recuperare i dipendenti il cui `HireDate` si è verificato in un mese specificato. Per fornire questa funzionalità in conformità con l'architettura è necessario innanzitutto creare un metodo in `EmployeesTableAdapter` che esegue il mapping all'istruzione SQL appropriato. A tale scopo, innanzitutto, aprire il DataSet tipizzato Northwind. Fare clic su di `EmployeesTableAdapter` etichetta e scegliere Aggiungi Query.


[![Aggiungere una nuova Query il EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Figura 2**: aggiungere una nuova Query per il `EmployeesTableAdapter` ([fare clic per visualizzare l'immagine ingrandita](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Scegliere di aggiungere un'istruzione SQL che restituisce righe. Quando si raggiunge la specifica un `SELECT` istruzione schermata il valore predefinito `SELECT` istruzione per il `EmployeesTableAdapter` saranno già caricato. Aggiungere semplicemente nel `WHERE` clausola: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) è una funzione di T-SQL che restituisce una parte relativa alla data particolare di un `datetime` tipo; in questo caso verrà usato `DATEPART` per restituire il mese del `HireDate` colonna.


[![Restituito solo le righe in cui HireDate Column è minore o uguale al @HiredBeforeDate parametro](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Figura 3**: restituisce solo le righe in cui la `HireDate` colonna è minore o uguale al `@HiredBeforeDate` parametro ([fare clic per visualizzare l'immagine ingrandita](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Infine, modificare il `FillBy` e `GetDataBy` i nomi dei metodi per `FillByHiredDateMonth` e `GetEmployeesByHiredDateMonth`, rispettivamente.


[![Scegliere nomi di metodo più appropriati anziché FillBy e GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Figura 4**: scegliere più appropriato metodo nomi rispetto `FillBy` e `GetDataBy` ([fare clic per visualizzare l'immagine ingrandita](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Fare clic su Fine per completare la procedura guidata e tornare all'area di progettazione del set di dati. Il `EmployeesTableAdapter` dovrebbero ora includere un nuovo set di metodi per l'accesso ai dipendenti assunti in un mese specificato.


[![I nuovi metodi vengono visualizzati nell'area di progettazione del set di dati](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Figura 5**: il nuovo metodi presenti nell'area di progettazione del set di dati ([fare clic per visualizzare l'immagine ingrandita](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Passaggio 2: Aggiunta di`GetEmployeesByHiredDateMonth(month)`al livello di logica di Business (metodo)

Poiché l'applicazione architettura vengono usati livello separato per la logica di business e i dati di accesso logica, è necessario aggiungere un metodo per il livello Business LOGIC che le chiamate al per recuperare i dipendenti assunti prima di una data specificata. Aprire il `EmployeesBLL.vb` file e aggiungere il metodo seguente:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Come con il nostro altri metodi in questa classe `GetEmployeesByHiredDateMonth(month)` chiama semplicemente verso il basso DAL e restituisce i risultati.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Passaggio 3: Visualizzazione dei dipendenti la cui anniversario assunzione è il mese corrente

Il passaggio finale per questo esempio è per visualizzare i dipendenti il cui anniversario assunzione è il mese corrente. Per iniziare, aggiungere un controllo GridView per il `ProgrammaticParams.aspx` nella pagina di `BasicReporting` cartella e aggiungere un nuovo oggetto ObjectDataSource come origine dati. Configurare ObjectDataSource per utilizzare il `EmployeesBLL` classe con il `SelectMethod` impostato su `GetEmployeesByHiredDateMonth(month)`.


[![Utilizzare la classe EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Figura 6**: usare il `EmployeesBLL` classe ([fare clic per visualizzare l'immagine ingrandita](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![Selezionare da GetEmployeesByHiredDateMonth(month) (metodo)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Figura 7**: Select From il `GetEmployeesByHiredDateMonth(month)` metodo ([fare clic per visualizzare l'immagine ingrandita](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


Nella schermata finale viene richiesto di fornire il `month` origine del valore del parametro. Poiché questo valore verrà impostata a livello di programmazione, lasciare l'origine del parametro impostato sul valore predefinito None opzione e fare clic su Fine.


[![Lasciare il parametro origine impostato su Nessuno](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Figura 8**: lasciare l'origine parametro impostato su None ([fare clic per visualizzare l'immagine ingrandita](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Verrà creato un `Parameter` oggetto ObjectDataSource `SelectParameters` raccolta che non dispone di un valore specificato.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Per impostare questo valore a livello di codice, è necessario creare un gestore eventi per ObjectDataSource `Selecting` evento. A tale scopo, passare alla visualizzazione progettazione e fare doppio clic su ObjectDataSource. In alternativa, selezionare ObjectDataSource, passare alla finestra proprietà e fare clic sull'icona del lampo. Successivamente, di fare doppio clic nella casella di testo accanto al `Selecting` evento o digitare il nome che si desidera utilizzare il gestore dell'evento. Come una terza opzione, è possibile creare il gestore dell'evento selezionando ObjectDataSource e il relativo `Selecting` evento due elenchi di riepilogo a discesa nella parte superiore della classe code-behind della pagina.


![Scegliere l'icona del fulmine nella finestra proprietà per visualizzare l'elenco di eventi di un controllo Web](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Figura 9**: scegliere l'icona del fulmine nella finestra proprietà per elencare gli eventi di un controllo Web


Tutti e tre gli approcci di aggiungere un nuovo gestore eventi per ObjectDataSource `Selecting` classe code-behind della pagina dell'evento. In questo gestore eventi è possibile leggere e scrivere i valori di parametro utilizzando `e.InputParameters(parameterName)`, dove *`parameterName`* è il valore della `Name` attributo la `<asp:Parameter>` tag (il `InputParameters` insieme può anche essere indicizzata ordinale, come in `e.InputParameters(index)`). Per impostare il `month` parametro per il mese corrente, aggiungere il comando seguente per il `Selecting` gestore eventi:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Quando si visita questa pagina tramite un browser possiamo vedere che un solo dipendente assunzione di questo mese (marzo) Laura Callahan, che ha lavorato nell'azienda dal 1994.


[![I dipendenti il cui ricorrenze questo mese vengono visualizzate](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Figura 10**: I dipendenti la cui ricorrenze questo mese vengono visualizzate ([fare clic per visualizzare l'immagine ingrandita](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Riepilogo

Mentre i valori dei parametri di ObjectDataSource sono in genere possibile impostare in modo dichiarativo, senza richiedere una riga di codice, è facile impostare i valori dei parametri a livello di codice. È sufficiente per eseguire la creazione di un gestore eventi per ObjectDataSource `Selecting` evento, che viene generato prima che venga richiamato il metodo dell'oggetto sottostante e impostare manualmente i valori per uno o più parametri tramite il `InputParameters` insieme.

Questa esercitazione si conclude la sezione Reporting di base. Il [esercitazione successiva](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) avvia la sezione di operazioni di filtro e scenari Master-Details, in cui viene analizzate le tecniche per consentire il visitatore per filtrare i dati e drill-down da un report master in un report dettagli.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione Hilton Giesenow. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](declarative-parameters-vb.md)
