---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
title: A livello di codice impostando i valori dei parametri di ObjectDataSource (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà esaminato l'aggiunta di un metodo al nostro DAL e BLL che accetta un parametro di input e restituisce i dati. Questo parametro viene impostato nell'esempio...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 1c4588bb-255d-4088-b319-5208da756f4d
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
msc.type: authoredcontent
ms.openlocfilehash: 561b197aae925eb432a3e93d37b347a081f64a2b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827351"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-c"></a>A livello di codice impostando i valori dei parametri di ObjectDataSource (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe) o [Scarica il PDF](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)

> In questa esercitazione verrà esaminato l'aggiunta di un metodo al nostro DAL e BLL che accetta un parametro di input e restituisce i dati. Questo parametro viene impostato a livello di codice nell'esempio.


## <a name="introduction"></a>Introduzione

Come abbiamo visto nel [esercitazione precedente](declarative-parameters-cs.md), una serie di opzioni è disponibile per il passaggio in modo dichiarativo i valori dei parametri ai metodi di ObjectDataSource. Se il valore del parametro è hardcoded, proviene da un controllo Web nella pagina o in qualsiasi altra origine che è leggibile da un'origine dati `Parameter` dell'oggetto, ad esempio, di valore può essere associato al parametro di input senza scrivere una riga di codice.

Potrebbero esserci casi, tuttavia, quando il valore del parametro viene fornito da un'origine non è già stato presi in considerazione da uno dei file di dati incorporato `Parameter` oggetti. Se gli account utente è supportato il nostro sito potrebbe essere necessario impostare il parametro di base attualmente connesso nell'ID del utente. visitatore dell' Oppure potrebbe essere necessario personalizzare il valore del parametro prima dell'invio insieme al metodo dell'oggetto sottostante di ObjectDataSource.

Ogni volta che ObjectDataSource `Select` metodo viene richiamato ObjectDataSource genera prima relativi [evento Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Quindi viene richiamato il metodo dell'oggetto sottostante di ObjectDataSource. Dopo aver completato l'operazione di ObjectDataSource [selezionati eventi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) generato (figura 1 illustra questa sequenza di eventi). I valori del parametro passati nel metodo dell'oggetto sottostante di ObjectDataSource possono essere impostati o personalizzati in un gestore eventi per il `Selecting` evento.


[![Viene richiamato selezionati e il metodo selezionando gli eventi attivati prima e dopo il suo dell'oggetto sottostante di ObjectDataSource](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)

**Figura 1**: The ObjectDataSource `Selected` e `Selecting` metodo eventi attivati prima e dopo il suo sottostante dell'oggetto viene richiamato ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))


In questa esercitazione verrà esaminato l'aggiunta di un metodo al nostro DAL e BLL che accetta un parametro di input `Month`, di tipo `int` e restituisce un `EmployeesDataTable` oggetto popolato con i dipendenti che hanno la ricorrenza annuale dell'assunzione nell'oggetto specificato `Month`. Questo esempio verrà impostato questo parametro a livello di programmazione basato sul mese corrente, che mostra un elenco di "Employee ricorrenze questo mese".

Iniziamo!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Passaggio 1: Aggiunta di un metodo per`EmployeesTableAdapter`

Per questo primo esempio è necessario aggiungere un mezzo per recuperare i dipendenti il cui `HireDate` si è verificato in un mese specificato. Per fornire questa funzionalità in base alla nostra architettura è necessario innanzitutto creare un metodo in `EmployeesTableAdapter` che esegue il mapping all'istruzione SQL appropriata. A tale scopo, iniziare aprendo il set di dati tipizzato Northwind. Fare clic su di `EmployeesTableAdapter` assegnare un'etichetta e scegliere Aggiungi Query.


[![Aggiungere una nuova Query per il EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)

**Figura 2**: aggiungere una nuova Query per il `EmployeesTableAdapter` ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))


Scegliere di aggiungere un'istruzione SQL che restituisce righe. Quando si raggiunge lo specificare una `SELECT` istruzione schermata il valore predefinito `SELECT` istruzione per il `EmployeesTableAdapter` saranno già caricato. Aggiungere semplicemente nel `WHERE` clausola: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) è una funzione T-SQL che restituisce una data specifica parte di un `datetime` tipo; in questo caso usiamo `DATEPART` per tornare il mese del `HireDate` colonna.


[![Restituito solo le righe in cui HireDate Sloupec JE minore o uguale al @HiredBeforeDate parametro](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)

**Figura 3**: restituisce solo le righe in cui la `HireDate` colonna è minore o uguale al `@HiredBeforeDate` parametro ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))


Infine, modificare il `FillBy` e `GetDataBy` i nomi dei metodi per `FillByHiredDateMonth` e `GetEmployeesByHiredDateMonth`, rispettivamente.


[![Scegliere i nomi dei metodi più Appropriate rispetto alle FillBy e GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)

**Figura 4**: scegliere più appropriato metodo nomi rispetto `FillBy` e `GetDataBy` ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))


Fare clic su Fine per completare la procedura guidata e tornare all'area di progettazione del set di dati. Il `EmployeesTableAdapter` dovrebbe ora includere un nuovo set di metodi per l'accesso di dipendenti assunti in un mese specificato.


[![I nuovi metodi vengono visualizzati nell'area di progettazione del set di dati](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)

**Figura 5**: il nuovo i metodi presenti nell'area di progettazione del set di dati ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Passaggio 2: Aggiunta di`GetEmployeesByHiredDateMonth(month)`metodo a livello della logica di Business

Poiché l'Usa di architettura dell'applicazione separato dei livelli per la logica di business e dati di accesso per la logica, è necessario aggiungere un metodo per il livello BLL che le chiamate al per recuperare i dipendenti assunti prima della data specificata. Aprire il `EmployeesBLL.cs` file e aggiungere il metodo seguente:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample1.cs)]

Come con altri metodi in questa classe `GetEmployeesByHiredDateMonth(month)` chiama semplicemente verso il basso DAL e restituisce i risultati.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Passaggio 3: Visualizzazione dei dipendenti la cui anniversario assunzione è questo mese

Il passaggio finale per questo esempio consiste nella visualizzazione quei dipendenti la cui anniversario assunzione è questo mese. Iniziare aggiungendo un controllo GridView per il `ProgrammaticParams.aspx` nella pagina di `BasicReporting` cartella e aggiungere un nuovo oggetto ObjectDataSource come origine dati. Configurare ObjectDataSource per usare la `EmployeesBLL` classe con il `SelectMethod` impostato su `GetEmployeesByHiredDateMonth(month)`.


[![Usare la classe EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)

**Figura 6**: usare il `EmployeesBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))


[![Selezionare da GetEmployeesByHiredDateMonth(month) (metodo)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)

**Figura 7**: selezionare dal `GetEmployeesByHiredDateMonth(month)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))


Schermata finale viene chiesto di specificare per fornire la `month` origine del valore del parametro. Poiché questo valore verrà impostata a livello di codice, lasciare il parametro source impostati sul valore predefinito None opzione e fare clic su Fine.


[![Lasciare il parametro Source impostato su Nessuno](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)

**Figura 8**: lasciare l'origine di parametro impostato su None ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))


Verrà creato un `Parameter` oggetto nella finestra di ObjectDataSource `SelectParameters` raccolta che non ha un valore specificato.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample2.aspx)]

Per impostare questo valore a livello di codice, è necessario creare un gestore eventi per ObjectDataSource `Selecting` evento. A tale scopo, passare alla visualizzazione progettazione e fare doppio clic su ObjectDataSource. In alternativa, selezionare ObjectDataSource, passare alla finestra delle proprietà e scegliere l'icona a forma di fulmine. Successivamente, fare doppio clic nella casella di testo accanto al `Selecting` evento o digitare il nome da usare il gestore dell'evento.


![Fare clic sull'icona a forma di fulmine nella finestra proprietà per visualizzare l'elenco degli eventi di un controllo Web](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image25.png)

**Figura 9**: fare clic sull'icona a forma di fulmine nella finestra proprietà per visualizzare l'elenco degli eventi di un controllo Web


Entrambi gli approcci aggiungere un nuovo gestore eventi per ObjectDataSource `Selecting` classe code-behind della pagina dell'evento. In questo gestore eventi è possibile leggere e scrivere i valori di parametro utilizzando `e.InputParameters[parameterName]`, dove *`parameterName`* è il valore della `Name` attributo il `<asp:Parameter>` tag (il `InputParameters` insieme può anche essere indicizzata ordinale, come in `e.InputParameters[index]`). Per impostare il `month` parametro per il mese corrente, aggiungere il codice seguente il `Selecting` gestore dell'evento:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample3.cs)]

Quando si visita questa pagina tramite un browser è possibile osservare che un solo dipendente è stato assunto questo mese (marzo) Laura Callahan, chi ha lavorato nell'azienda dal 1994.


[![I dipendenti il cui ricorrenze questo mese vengono visualizzati](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)

**Figura 10**: I dipendenti la cui ricorrenze questo mese vengono visualizzati ([fare clic per visualizzare l'immagine con dimensioni normali](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))


## <a name="summary"></a>Riepilogo

Mentre i parametri di ObjectDataSource in genere possibile impostare i valori in modo dichiarativo, senza richiedere una riga di codice, è facile impostare i valori dei parametri a livello di codice. Tutto dobbiamo fare è creare un gestore eventi per ObjectDataSource `Selecting` evento, che viene attivato prima che venga richiamato il metodo dell'oggetto sottostante e impostare manualmente i valori per uno o più parametri tramite la `InputParameters` raccolta.

Questa esercitazione si conclude la sezione Basic Reporting. Il [prossima esercitazione](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) la sezione di operazioni di filtro e gli scenari di Master-Details, in cui abbiamo verranno analizzate le tecniche per consentire il visitatore per filtrare i dati e il drill-down da un report master in un report dettagli viene avviato.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Hilton Giesenow, revisore per questa esercitazione. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](declarative-parameters-cs.md)
> [Successivo](displaying-data-with-the-objectdatasource-vb.md)
