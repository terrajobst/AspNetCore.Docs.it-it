---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: I controlli origine dati | Documenti Microsoft
author: microsoft
description: Il controllo DataGrid in ASP.NET 1. x contrassegnato un notevole miglioramento di accesso ai dati nelle applicazioni Web. Tuttavia, non è stato semplice da utilizzare come potrebbe...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: b1ac7fb62767d61c97fe00338bc0f5087f4863b5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885895"
---
<a name="data-source-controls"></a>Controlli origine dati
====================
by [Microsoft](https://github.com/microsoft)

> Il controllo DataGrid in ASP.NET 1. x contrassegnato un notevole miglioramento di accesso ai dati nelle applicazioni Web. Tuttavia, non è stato semplice da utilizzare come potrebbe. È necessario comunque una notevole quantità di codice per ottenere funzionalità più utili da esso. Ad esempio è il modello in tutti i dati l'accesso a tal fine in 1. x.


Il controllo DataGrid in ASP.NET 1. x contrassegnato un notevole miglioramento di accesso ai dati nelle applicazioni Web. Tuttavia, non è stato semplice da utilizzare come potrebbe. È necessario comunque una notevole quantità di codice per ottenere funzionalità più utili da esso. Ad esempio è il modello in tutti i dati l'accesso a tal fine in 1. x.

ASP.NET 2.0 gli indirizzi con parzialmente con i controlli origine dati. I controlli origine dati in ASP.NET 2.0 forniscono agli sviluppatori un modello dichiarativo per il recupero dei dati, la visualizzazione dei dati e la modifica dei dati. Lo scopo dei controlli origine dati è fornire una rappresentazione coerenza dei dati a controlli con associazione a dati indipendentemente dall'origine di tali dati. Il fulcro di controlli origine dati in ASP.NET 2.0 è la classe astratta del controllo origine dati. La classe del controllo origine dati fornisce un'implementazione di base dell'interfaccia IDataSource e l'interfaccia IListSource, il secondo consente di assegnare il controllo origine dati come origine dati di un controllo con associazione a dati (tramite la nuova proprietà DataSourceId illustrato più avanti) ed esporre i dati ivi sotto forma di elenco. Ogni elenco di dati da un controllo origine dati viene esposto come un oggetto DataSourceView. Viene fornito accesso alle istanze DataSourceView dall'interfaccia IDataSource. Ad esempio, il metodo GetViewNames restituisce ICollection che consente di enumerare il DataSourceViews associata a un controllo origine dati specifica e il metodo GetView consente di accedere a una particolare istanza DataSourceView in base al nome.

I controlli origine dati non dispongono di alcuna interfaccia utente. Vengono implementati come controlli server in modo che possono supportare la sintassi dichiarativa e in modo che dispongono di accesso per lo stato della pagina se si desidera. I controlli origine dati non eseguire il rendering di markup HTML al client.

> [!NOTE]
> Come si vedrà in un secondo momento, vi sono inoltre la memorizzazione nella cache vantaggi ottenuti utilizzando i controlli origine dati.


## <a name="storing-connection-strings"></a>L'archiviazione delle stringhe di connessione

Prima di passare a esaminando come configurare i controlli origine dati, si dovrebbe coprire una nuova funzionalità di ASP.NET 2.0 relative stringhe di connessione. ASP.NET 2.0 introduce una nuova sezione nel file di configurazione che consente di memorizzare facilmente le stringhe di connessione che possono essere letti in modo dinamico in fase di esecuzione. Il &lt;connectionStrings&gt; sezione rende più semplice archiviare le stringhe di connessione.

Nel frammento seguente aggiunge una nuova stringa di connessione.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Come il &lt;appSettings&gt; sezione, il &lt;connectionStrings&gt; sezione viene visualizzata di fuori del &lt;System. Web&gt; sezione nel file di configurazione.


Per utilizzare questa stringa di connessione, è possibile utilizzare la sintassi seguente quando si imposta l'attributo ConnectionString di un controllo server.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

Il &lt;connectionStrings&gt; sezione può anche essere crittografata in modo che non sono esposta informazioni riservate. Tale possibilità verrà trattata in un modulo successivo.

## <a name="caching-data-sources"></a>La memorizzazione nella cache di origini dati

Ogni controllo origine dati fornisce quattro proprietà per configurare la memorizzazione nella cache; EnableCaching CacheDuration, CacheExpirationPolicy e CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching è una proprietà booleana che determina se la memorizzazione nella cache è abilitata per il controllo origine dati.

## <a name="cacheduration-property"></a>Proprietà CacheDuration

La proprietà CacheDuration imposta il numero di secondi durante i quali la cache rimane valida. Impostando questa proprietà su **0** la cache rimane valido fino a quando non invalidato in modo esplicito.

## <a name="cacheexpirationpolicy-property"></a>Proprietà CacheExpirationPolicy

La proprietà CacheExpirationPolicy può essere impostata su **assoluto** o **estendibile**. Verrà impostata su assoluto indica che la quantità massima di tempo in cui verranno memorizzati nella cache di dati è il numero di secondi specificato dalla proprietà CacheDuration. Impostandola su estendibile, l'ora di scadenza viene reimpostato quando viene eseguita ogni operazione.

## <a name="cachekeydependency-property"></a>Proprietà CacheKeyDependency

Se viene specificato un valore di stringa per la proprietà CacheKeyDependency, ASP.NET imposterà una nuova dipendenza della cache basata su quella stringa. Ciò consente di invalidare in modo esplicito la cache semplicemente modificando o rimuovendo il CacheKeyDependency.

**Importante**: se la rappresentazione è abilitata e l'accesso all'origine dati e/o contenuto dei dati è basato su identità del client, è consigliabile che la memorizzazione nella cache disabilitata impostando EnableCaching su False. Se la memorizzazione nella cache è abilitata in questo scenario, un utente diverso da quello che ha richiesto originariamente i dati genera una richiesta di autorizzazione per l'origine dati non viene applicata. I dati verranno semplicemente serviti dalla cache.

## <a name="the-sqldatasource-control"></a>Il controllo SqlDataSource

Il controllo SqlDataSource consente agli sviluppatori di accedere ai dati archiviati in qualsiasi database relazionale che supporta ADO.NET. È possibile utilizzare il provider SqlClient per accedere a un database di SQL Server, il provider OLEDB, il provider ODBC o il provider OracleClient per accedere a Oracle. Pertanto, SqlDataSource è certamente non utilizzate solo per l'accesso ai dati in un database di SQL Server.

Per utilizzare SqlDataSource, sufficiente fornire un valore per la proprietà ConnectionString e specificare un comando SQL o stored procedure. Il controllo SqlDataSource si occupa dell'utilizzo con l'architettura ADO.NET sottostante. Apre la connessione, esegue una query origine dati o esegue la stored procedure, restituisce i dati e quindi chiude la connessione per l'utente.

> [!NOTE]
> Perché la classe del controllo origine dati chiude automaticamente la connessione per l'utente, è necessario ridurre il numero di chiamate dei clienti generati da perdita di connessioni al database.


Nel frammento di codice seguente associa un controllo DropDownList a un controllo SqlDataSource usando la stringa di connessione che viene archiviata nel file di configurazione, come illustrato in precedenza.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Come illustrato sopra, la proprietà DataSourceMode del SqlDataSource specifica la modalità per l'origine dati. Nell'esempio precedente, DataSourceMode è impostato su DataReader. In tal caso, SqlDataSource restituirà un oggetto IDataReader utilizzando un cursore forward-only e di sola lettura. Il tipo specificato di oggetto restituito è controllato dal provider utilizzato. In questo caso, si utilizza il provider SqlClient come specificato nella &lt;connectionStrings&gt; sezione del file Web. config. Pertanto, l'oggetto restituito sarà di tipo SqlDataReader. Specificando un valore DataSourceMode del set di dati, i dati possono essere archiviati in un set di dati nel server. Questa modalità consente di aggiungere funzionalità come l'ordinamento, paging e così via. Se avevo stato associazione SqlDataSource a un controllo GridView, verrà scelto la modalità di set di dati. Tuttavia, nel caso di un controllo DropDownList, la modalità di DataReader è la scelta corretta.

> [!NOTE]
> Quando la memorizzazione nella cache un SqlDataSource o un AccessDataSource, è necessario impostare la proprietà DataSourceMode a set di dati. Se si abilita la memorizzazione nella cache con un oggetto DataReader di DataSourceMode, si verificherà un'eccezione.


## <a name="sqldatasource-properties"></a>Proprietà SqlDataSource

Di seguito è riportate alcune delle proprietà del controllo SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Valore booleano che specifica se un comando select viene annullato se uno dei parametri è null. True per impostazione predefinita.

### <a name="conflictdetection"></a>ConflictDetection

In una situazione in cui più utenti possono aggiornare un'origine dati allo stesso tempo, la proprietà ConflictDetection determina il comportamento del controllo SqlDataSource. Questa proprietà restituisce uno dei valori dell'enumerazione ConflictOptions. Tali valori sono **CompareAllValues** e **OverwriteChanges**. Se impostato su OverwriteChanges, ultima persona per scrivere i dati all'origine dei dati sovrascrive le modifiche precedenti. Tuttavia, impostando la proprietà ConflictDetection su CompareAllValues, i parametri vengono creati per le colonne restituite da SelectCommand e i parametri vengono creati anche per ognuna di queste colonne consentendo SqlDataSource per contenere i valori originali determinare se i valori sono stati modificati in quanto è stato eseguito il SelectCommand.

### <a name="deletecommand"></a>DeleteCommand

Imposta o ottiene la stringa SQL utilizzata per l'eliminazione delle righe dal database. Questo può essere una query SQL o un nome della stored procedure.

### <a name="deletecommandtype"></a>DeleteCommandType

Imposta o ottiene il tipo di comando di eliminazione, che può essere una query SQL (testo) o una stored procedure (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Restituisce i parametri usati da DeleteCommand dell'oggetto SqlDataSourceView associato al controllo SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Questa proprietà viene utilizzata per specificare il formato dei parametri con valore originale nei casi in cui è impostata la proprietà ConflictDetection per CompareAllValues. Il valore predefinito è {0} il che significa che i parametri di valore originale avrà lo stesso nome di parametro originale. In altre parole, se il nome del campo è EmployeeID, il parametro del valore originale sarà @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Imposta o ottiene la stringa SQL che viene utilizzata per recuperare dati dal database. Questo può essere una query SQL o un nome della stored procedure.

### <a name="selectcommandtype"></a>SelectCommandType

Imposta o ottiene il tipo di comando select, che può essere una query SQL (testo) o una stored procedure (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Restituisce i parametri utilizzati per la proprietà SelectCommand dell'oggetto SqlDataSourceView associato al controllo SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Ottiene o imposta il nome di un parametro di stored procedure che viene utilizzato quando l'ordinamento dei dati recuperati dal controllo origine dati. Valido solo quando SelectCommandType è impostata su StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Stringa che specifica il database e le tabelle utilizzate in una dipendenza della cache di SQL Server è delimitata da un punto e virgola. (Verranno descritte le dipendenze della cache SQL in un modulo successivo).

### <a name="updatecommand"></a>UpdateCommand

Imposta o ottiene la stringa SQL utilizzata durante l'aggiornamento dei dati nel database. Questo può essere una query SQL o un nome della stored procedure.

### <a name="updatecommandtype"></a>UpdateCommandType

Imposta o ottiene il tipo di comando di aggiornamento, che può essere una query SQL (testo) o una stored procedure (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Restituisce i parametri usati da UpdateCommand dell'oggetto SqlDataSourceView associato al controllo SqlDataSource.

## <a name="the-accessdatasource-control"></a>Il controllo AccessDataSource

Il controllo AccessDataSource deriva dalla classe SqlDataSource e viene utilizzato per associare dati a un database Microsoft Access. La proprietà ConnectionString per il controllo AccessDataSource è una proprietà di sola lettura. Anziché utilizzare la proprietà ConnectionString, la proprietà DataFile è utilizzata in modo che punti al Database di Access, come illustrato di seguito.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

L'AccessDataSource imposterà sempre ProviderName di base SqlDataSource OleDb e si connette al database utilizzando il provider OLE DB Microsoft.Jet.OLEDB.4.0. È possibile utilizzare il controllo AccessDataSource per connettersi a un database di Access protetti da password. Se è necessario connettersi a un database protetto da password, è necessario utilizzare il controllo SqlDataSource.

> [!NOTE]
> I database di Access archiviati all'interno del sito Web devono essere inseriti nell'App\_directory dei dati. ASP.NET non consente i file in questa directory da visualizzare. È necessario concedere all'account del processo delle autorizzazioni di lettura e scrittura per l'App\_directory dei dati quando si utilizzano i database di Access.


## <a name="the-xmldatasource-control"></a>Il controllo XmlDataSource

L'oggetto XmlDataSource viene utilizzato per associare dati XML ai dati a controlli con associazione a dati. È possibile associare a un file XML utilizzando la proprietà DataFile o è possibile associare a una stringa XML utilizzando la proprietà di dati. L'oggetto XmlDataSource espone gli attributi XML come campi associabili. Nei casi in cui è necessario associare i valori che non sono rappresentati come attributi, è necessario utilizzare una trasformazione XSL. È inoltre possibile utilizzare espressioni XPath per filtrare i dati XML.

Si consideri il file XML seguente:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Si noti che l'oggetto XmlDataSource utilizza una proprietà XPath di *persone/persona* per filtrare solo i &lt;persona&gt; nodi. DropDownList quindi associa dati per l'attributo LastName utilizzando la proprietà DataTextField.

Mentre il controllo XmlDataSource viene utilizzato principalmente per associare dati ai dati XML di sola lettura, è possibile modificare il file di dati XML. Si noti che in questi casi, dall'inserimento automatico, aggiornamento ed eliminazione delle informazioni nel file XML non viene eseguita automaticamente a quanto accade con altri controlli origine dati. In alternativa, è necessario scrivere codice per modificare manualmente i dati del controllo XmlDataSource nei modi seguenti.

### <a name="getxmldocument"></a>GetXmlDocument

Recupera un oggetto XmlDocument contenente il codice XML recuperato dal controllo XmlDataSource.

### <a name="save"></a>Salva

Salva l'elemento XmlDocument in memoria fino all'origine dati.

È importante tenere presente che il metodo Save funziona solo quando vengono soddisfatte le due condizioni seguenti:

1. L'oggetto XmlDataSource utilizza la proprietà DataFile da associare a un file XML anziché la proprietà di dati da associare ai dati XML in memoria.
2. Tramite la proprietà di trasformazione o TransformFile viene specificata alcuna trasformazione.

Si noti inoltre che il metodo Save può restituire risultati imprevisti quando viene chiamato da più utenti contemporaneamente.

## <a name="the-objectdatasource-control"></a>Il controllo ObjectDataSource

I controlli origine dati che sono stati illustrati fino a questo punto sono scelte eccellente per applicazioni a due livelli in cui il controllo origine dati comunica direttamente con l'archivio dati. Tuttavia, molte applicazioni reali sono applicazioni multilivello in cui un controllo origine dati potrebbe essere necessario comunicare con un oggetto business che, a sua volta comunica con il livello dati. In queste situazioni, ObjectDataSource riempie la fattura correttamente. ObjectDataSource funziona in combinazione con un oggetto di origine. Il controllo ObjectDataSource creerà un'istanza dell'oggetto di origine, chiamare il metodo specificato e dispose dell'istanza dell'oggetto nell'ambito di una singola richiesta, se l'oggetto dispone di metodi di istanza anziché i metodi statici (Shared in Visual Basic). Pertanto, l'oggetto deve essere senza stato. Ovvero, l'oggetto deve acquisire e rilasciare tutte le risorse necessarie all'interno di una singola richiesta. È possibile controllare la modalità in cui viene creato l'oggetto di origine gestendo l'evento ObjectCreating del controllo ObjectDataSource. È possibile creare un'istanza dell'oggetto di origine e quindi impostare la proprietà ObjectInstance della classe ObjectDataSourceEventArgs a tale istanza. Il controllo ObjectDataSource utilizzerà l'istanza viene creata nell'evento ObjectCreating anziché creare un'istanza.

Se l'oggetto di origine per un controllo ObjectDataSource espone i metodi statici pubblici (Shared in Visual Basic) che possono essere chiamati per recuperare e modificare i dati, un controllo ObjectDataSource chiamerà tali metodi direttamente. Se un controllo ObjectDataSource deve creare un'istanza dell'oggetto di origine per effettuare chiamate al metodo, l'oggetto deve includere un costruttore pubblico che non accetta parametri. Il controllo ObjectDataSource chiamerà questo costruttore quando crea una nuova istanza dell'oggetto di origine.

Se l'oggetto di origine non contiene un costruttore pubblico senza parametri, è possibile creare un'istanza dell'oggetto di origine che verrà utilizzato dal controllo ObjectDataSource nell'evento ObjectCreating.

## <a name="specifying-object-methods"></a>Specifica dei metodi di oggetto

L'oggetto di origine per un controllo ObjectDataSource può contenere qualsiasi numero di metodi che consentono di selezionare, inserire, aggiornare o eliminare dati. Questi metodi vengono chiamati dal controllo ObjectDataSource in base al nome del metodo, come identificato dalla proprietà SelectMethod, InsertMethod, UpdateMethod o di DeleteMethod del controllo ObjectDataSource. L'oggetto di origine può includere anche un metodo SelectCount facoltativo, che è identificato dal controllo ObjectDataSource utilizzando la proprietà SelectCountMethod, che restituisce il conteggio del numero totale di oggetti nell'origine dati. Controllo ObjectDataSource chiamerà il metodo SelectCount dopo la chiamata a un metodo Select per recuperare il numero totale di record nell'origine dati per l'utilizzo durante la suddivisione in.

## <a name="lab-using-data-source-controls"></a>Lab usando i controlli origine dati

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Esercizio 1: visualizzazione di dati con il controllo SqlDataSource

Nell'esercizio seguente utilizza il controllo SqlDataSource per la connessione al database Northwind. Si presuppone di avere accesso al database Northwind in un'istanza di SQL Server 2000.

1. Creare un nuovo sito Web ASP.NET.
2. Aggiungere un nuovo file Web. config.

    1. Pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
    2. Scegliere i File di configurazione Web dall'elenco dei modelli e fare clic su Aggiungi.
3. Modificare il &lt;connectionStrings&gt; sezione come indicato di seguito: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Passare alla visualizzazione codice e aggiungere un attributo ConnectionString e SelectCommand un attributo di &lt;asp: SqlDataSource&gt; controllare nel modo seguente: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Dalla visualizzazione progettazione, aggiungere un nuovo controllo GridView.
6. Nell'elenco a discesa Scegli origine dati in menu attività di GridView, scegliere SqlDataSource1.
7. Fare clic su Default.aspx e scegliere Visualizza nel Browser dal menu. Fare clic su Sì quando viene richiesto di salvare.
8. GridView Visualizza i dati dalla tabella Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Esercizio 2: modifica dei dati con il controllo SqlDataSource

Nell'esercizio seguente viene illustrato a cui associare i dati un DropDownList controllo utilizzando la sintassi dichiarativa e consente di modificare i dati presentati nel controllo DropDownList.

1. In visualizzazione progettazione, eliminare il controllo GridView dalla Default.aspx. 

    **Importante**: il controllo SqlDataSource nella pagina.
2. Aggiungere un controllo DropDownList a Default.aspx.
3. Passare alla visualizzazione origine.
4. Aggiungere un attributo DataSourceId DataTextField e DataValueField il &lt;asp: DropDownList&gt; controllare nel modo seguente: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Salvare Default.aspx e visualizzarlo nel browser. Si noti che DropDownList contiene tutti i prodotti dal database Northwind.
6. Chiudere il browser.
7. Nella vista di origine di Default.aspx, aggiungere un nuovo controllo casella di testo sotto il controllo DropDownList. Modificare la proprietà ID della casella di testo per txtProductName.
8. Sotto il controllo casella di testo, aggiungere un nuovo controllo pulsante. Modificare la proprietà ID del pulsante btnUpdate e la proprietà Text su **aggiornamento Product Name**.
9. Nella vista di origine di Default.aspx, aggiungere una proprietà UpdateCommand e due UpdateParameters di nuovo al tag SqlDataSource come segue: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Si noti che sono presenti che due parametri (ProductName e ProductID) aggiunti in questo codice di aggiornamento. Questi parametri sono mappati per la proprietà Text del txtProductName casella di testo e la proprietà SelectedValue di ddlProducts DropDownList.
10. Passare alla visualizzazione progettazione e fare doppio clic sul pulsante per aggiungere un gestore eventi.
11. Aggiungere il codice seguente il btnUpdate\_scegliere codice: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Fare clic su Default.aspx e scegliere di visualizzarla nel browser. Quando viene richiesto di salvare tutte le modifiche, fare clic su Sì.
13. ASP.NET 2.0 le classi parziali consentono per la compilazione in fase di esecuzione. Non è necessario compilare un'applicazione per vedere le modifiche al codice applicate.
14. Selezionare un prodotto DropDownList.
15. Immettere un nuovo nome per il prodotto selezionato nella casella di testo e quindi fare clic sul pulsante di aggiornamento.
16. Il nome del prodotto viene aggiornato nel database.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Esercizio 3 mediante il controllo ObjectDataSource

Questa esercitazione verrà illustrato come utilizzare il controllo ObjectDataSource e un oggetto di origine per interagire con il database Northwind.

1. Pulsante destro del mouse sul progetto in Esplora soluzioni e fare clic su Aggiungi nuovo elemento.
2. Selezionare il modulo Web nell'elenco di modelli. Modificare il nome in object.aspx e fare clic su Aggiungi.
3. Pulsante destro del mouse sul progetto in Esplora soluzioni e fare clic su Aggiungi nuovo elemento.
4. Selezionare una classe nell'elenco di modelli. Modificare il nome della classe in NorthwindData.cs e fare clic su Aggiungi.
5. Fare clic su Sì quando viene richiesto di aggiungere la classe per l'App\_cartella del codice.
6. Aggiungere il codice seguente al file NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Aggiungere il codice seguente per la vista di origine di object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Salvare tutti i file e individuare object.aspx.
9. Interagire con l'interfaccia di visualizzazione dei dettagli, la modifica di dipendenti, dipendenti aggiunta ed eliminazione dipendenti.
