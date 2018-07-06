---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Controlli origine dati | Microsoft Docs
author: microsoft
description: Il controllo DataGrid in ASP.NET 1.x contrassegnato un notevole miglioramento nell'accesso ai dati nelle applicazioni Web. Tuttavia, non è stato semplice da utilizzare come avrebbe potuto...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: 15a09e8ac7da6d23216a92863ae7ce6db7ecd57a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809354"
---
<a name="data-source-controls"></a>Controlli origine dati
====================
da [Microsoft](https://github.com/microsoft)

> Il controllo DataGrid in ASP.NET 1.x contrassegnato un notevole miglioramento nell'accesso ai dati nelle applicazioni Web. Tuttavia, non è stato semplice da utilizzare come avrebbe potuto. Comunque necessaria una notevole quantità di codice per ottenere molte funzionalità utili da quest'ultimo. Ad esempio è il modello in tutte le tue attività di accesso di dati nella versione 1.x.


Il controllo DataGrid in ASP.NET 1.x contrassegnato un notevole miglioramento nell'accesso ai dati nelle applicazioni Web. Tuttavia, non è stato semplice da utilizzare come avrebbe potuto. Comunque necessaria una notevole quantità di codice per ottenere molte funzionalità utili da quest'ultimo. Ad esempio è il modello in tutte le tue attività di accesso di dati nella versione 1.x.

ASP.NET 2.0 risolve questo problema con parzialmente con i controlli origine dati. I controlli origine dati in ASP.NET 2.0 offrono agli sviluppatori un modello dichiarativo per il recupero dei dati, la visualizzazione dei dati e la modifica dei dati. Lo scopo dei controlli origine dati è fornire una rappresentazione coerente dei dati ai controlli con associazione a dati indipendentemente dall'origine di tali dati. Il fulcro di controlli origine dati in ASP.NET 2.0 è la classe astratta controllo origine dati. La classe DataSourceControl fornisce un'implementazione di base dell'interfaccia IDataSource e l'interfaccia IListSource, quest'ultimo dei quali consente di assegnare al controllo origine dati come origine dati di un controllo con associazione a dati (tramite la nuova proprietà di un elemento DataSourceId illustrato più avanti) e di esporre i dati al suo interno come elenco. Ogni elenco di dati da un controllo origine dati viene esposto come un oggetto DataSourceView. Accesso alle istanze di DataSourceView avviene tramite l'interfaccia IDataSource. Ad esempio, il metodo GetViewNames restituisce un oggetto ICollection che consente di enumerare i DataSourceViews associata a un controllo origine dati specifica e il metodo GetView consente di accedere a una particolare istanza di DataSourceView in base al nome.

Controlli origine dati non dispongono di alcuna interfaccia utente. Essi vengono implementate come controlli server in modo che possono supportare la sintassi dichiarativa e in modo che abbiano accesso allo stato di pagina se si desidera. Controlli origine dati non vengono visualizzati eventuali markup HTML per il client.

> [!NOTE]
> Come si vedrà in seguito, vi sono anche la memorizzazione nella cache i vantaggi ottenuti utilizzando i controlli origine dati.


## <a name="storing-connection-strings"></a>L'archiviazione delle stringhe di connessione

Prima di addentrarsi esaminando come configurare i controlli origine dati, si dovrebbe coprire una nuova funzionalità in ASP.NET 2.0 riguardanti le stringhe di connessione. ASP.NET 2.0 viene introdotta una nuova sezione nel file di configurazione che consente di archiviare agevolmente le stringhe di connessione che possono essere letti in modo dinamico in fase di esecuzione. Il &lt;connectionStrings&gt; sezione rende più semplice archiviare le stringhe di connessione.

Il frammento di codice seguente aggiunge una nuova stringa di connessione.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Come le &lt;appSettings&gt; sezione, il &lt;connectionStrings&gt; verrà visualizzata la sezione all'esterno del &lt;System. Web&gt; sezione nel file di configurazione.


Per usare questa stringa di connessione, è possibile usare la sintassi seguente quando si imposta l'attributo ConnectionString di un controllo server.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

Il &lt;connectionStrings&gt; sezione può anche essere crittografata in modo che le informazioni riservate non viene esposto. Queste funzionalità verranno trattate in un modulo successivo.

## <a name="caching-data-sources"></a>La memorizzazione nella cache le origini dati

Ogni controllo origine dati fornisce quattro proprietà per configurare la memorizzazione nella cache; EnableCaching CacheDuration, CacheExpirationPolicy e CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching è una proprietà booleana che determina se la memorizzazione nella cache è abilitata per il controllo origine dati.

## <a name="cacheduration-property"></a>Proprietà CacheDuration

La proprietà CacheDuration imposta il numero di secondi durante i quali la cache rimane valida. Impostando questa proprietà su **0** fa sì che la cache deve rimanere valido fino a quando non invalidato in modo esplicito.

## <a name="cacheexpirationpolicy-property"></a>Proprietà CacheExpirationPolicy

La proprietà CacheExpirationPolicy può essere impostata su **assoluto** oppure **Sliding**. Impostandola su assoluto significa che la quantità massima di tempo in cui i dati nella cache è il numero di secondi specificato dalla proprietà CacheDuration. Impostandola su estendibile, l'ora di scadenza viene reimpostata quando viene eseguita ogni operazione.

## <a name="cachekeydependency-property"></a>Proprietà CacheKeyDependency

Se viene specificato un valore stringa per la proprietà CacheKeyDependency, ASP.NET configurerà una nuova dipendenza della cache in base a tale stringa. Ciò consente in modo esplicito invalidare la cache semplicemente modificando o rimuovendo il CacheKeyDependency.

**Importante**: se la rappresentazione è abilitata e di accesso per l'origine dati e/o il contenuto dei dati si basata sulle identità del client, è consigliabile che la memorizzazione nella cache disabilitata impostando EnableCaching su False. Se la memorizzazione nella cache è abilitata in questo scenario, un utente diverso dall'utente che ha richiesto originariamente i dati genera una richiesta di autorizzazione per l'origine dati non viene applicata. I dati verranno semplicemente serviti dalla cache.

## <a name="the-sqldatasource-control"></a>Il controllo SqlDataSource

Il controllo SqlDataSource consente agli sviluppatori di accedere ai dati archiviati in qualsiasi database relazionale che supporta ADO.NET. È possibile utilizzare il provider SqlClient per accedere a un database di SQL Server, il provider OLEDB, il provider System.Data.Odbc o il provider System.Data.OracleClient per accedere a Oracle. Pertanto, SqlDataSource certamente non viene usato solo per l'accesso ai dati in un database di SQL Server.

Per poter utilizzare SqlDataSource, sufficiente fornire un valore per la proprietà ConnectionString e specificare un comando SQL o stored procedure. Il controllo SqlDataSource si occupa dell'utilizzo con l'architettura ADO.NET sottostante. Apre la connessione, esegue query sull'origine dati o esegue la stored procedure, restituisce i dati e quindi chiude la connessione per l'utente.

> [!NOTE]
> Poiché la classe DataSourceControl chiude automaticamente la connessione, dovrebbe ridurre il numero di chiamate dei clienti generato dalla perdita di connessioni di database.


Il frammento di codice seguente associa un controllo DropDownList per un controllo SqlDataSource usando la stringa di connessione archiviata nel file di configurazione, come illustrato in precedenza.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Come illustrato in precedenza, la proprietà DataSourceMode di SqlDataSource specifica la modalità per l'origine dati. Nell'esempio precedente, DataSourceMode è impostato su DataReader. In tal caso, SqlDataSource restituirà un oggetto di interfaccia IDataReader utilizzando un cursore forward-only e di sola lettura. Il tipo specificato di oggetto restituito è controllato dal provider utilizzato. In questo caso, ho scelto il provider SqlClient come specificato nella &lt;connectionStrings&gt; sezione del file Web. config. Pertanto, l'oggetto restituito sarà di tipo oggetto SqlDataReader. Specificando un valore DataSourceMode del set di dati, i dati possono essere archiviati in un set di dati nel server. Questa modalità consente di aggiungere funzionalità come l'ordinamento, paging e così via. Se avessi have i been data binding SqlDataSource a un controllo GridView, si sarebbe scelto la modalità di set di dati. Tuttavia, nel caso di un controllo DropDownList, la modalità di DataReader è la scelta corretta.

> [!NOTE]
> Quando la memorizzazione nella cache un SqlDataSource o un AccessDataSource, la proprietà DataSourceMode deve essere impostata a set di dati. Se si abilita la memorizzazione nella cache con DataSourceMode del DataReader, si verificherà un'eccezione.


## <a name="sqldatasource-properties"></a>Proprietà SqlDataSource

Di seguito sono alcune delle proprietà del controllo SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Valore booleano che specifica se un comando select viene annullato se uno dei parametri è null. True per impostazione predefinita.

### <a name="conflictdetection"></a>ConflictDetection

In una situazione in cui più utenti potrebbero trattarsi di aggiornare un'origine dati allo stesso tempo, la proprietà ConflictDetection determina il comportamento del controllo SqlDataSource. Questa proprietà restituisce uno dei valori dell'enumerazione ConflictOptions. Tali valori sono **CompareAllValues** e **OverwriteChanges**. Se impostato su OverwriteChanges, ultima persona che scrivere i dati all'origine dati sovrascrive le modifiche precedenti. Tuttavia, se la proprietà ConflictDetection è impostata su CompareAllValues, i parametri vengono creati per le colonne restituite da SelectCommand e i parametri vengono anche creati per contenere i valori originali in ognuna di queste colonne consentendo SqlDataSource per determinare se i valori sono state modificate poiché è stato eseguito la proprietà SelectCommand.

### <a name="deletecommand"></a>DeleteCommand

Imposta o ottiene la stringa SQL utilizzata quando l'eliminazione delle righe dal database. Ciò può essere una query SQL o un nome di stored procedure.

### <a name="deletecommandtype"></a>DeleteCommandType

Imposta o ottiene il tipo di comando di eliminazione, che può essere una query SQL (testo) o una stored procedure (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Restituisce i parametri usati da DeleteCommand dell'oggetto SqlDataSourceView associato al controllo SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Questa proprietà viene utilizzata per specificare il formato dei parametri di valore originale in casi in cui la proprietà ConflictDetection è impostata su CompareAllValues. Il valore predefinito è {0} che significa che i parametri di valore originale avranno lo stesso nome del parametro originale. In altre parole, se il nome del campo è EmployeeID, il parametro del valore originale sarà @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Imposta o ottiene la stringa SQL utilizzata per recuperare i dati dal database. Ciò può essere una query SQL o un nome di stored procedure.

### <a name="selectcommandtype"></a>SelectCommandType

Imposta o ottiene il tipo di comando select, che può essere una query SQL (testo) o una stored procedure (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Restituisce i parametri utilizzati per la proprietà SelectCommand dell'oggetto SqlDataSourceView associato al controllo SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Ottiene o imposta il nome di un parametro di stored procedure che viene usato quando l'ordinamento dei dati recuperati dal controllo origine dati. Valido solo quando SelectCommandType è impostato su StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Stringa delimitata da punto e virgola che specifica il database e le tabelle utilizzate in una dipendenza della cache di SQL Server. (Verranno discusso dipendenze della cache SQL in un modulo più avanti).

### <a name="updatecommand"></a>UpdateCommand

Imposta o ottiene la stringa SQL utilizzata durante l'aggiornamento dei dati nel database. Ciò può essere una query SQL o un nome di stored procedure.

### <a name="updatecommandtype"></a>UpdateCommandType

Imposta o ottiene il tipo di comando di aggiornamento, che può essere una query SQL (testo) o una stored procedure (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Restituisce i parametri usati da UpdateCommand dell'oggetto SqlDataSourceView associato al controllo SqlDataSource.

## <a name="the-accessdatasource-control"></a>Il controllo AccessDataSource

Il controllo AccessDataSource deriva dalla classe SqlDataSource e viene utilizzato per associare dati a un database Microsoft Access. La proprietà ConnectionString per il controllo AccessDataSource è una proprietà di sola lettura. Invece di usare la proprietà ConnectionString, la proprietà file di dati viene utilizzata in modo che punti al Database di Access come illustrato di seguito.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

Il AccessDataSource imposterà sempre il ProviderName di base SqlDataSource OleDb e si connette al database utilizzando il provider Microsoft.Jet.OLEDB.4.0 OLE DB. È possibile utilizzare il controllo AccessDataSource per connettersi a un database di Access protetti da password. Se è necessario connettersi a un database protetto da password, è consigliabile usare il controllo SqlDataSource.

> [!NOTE]
> I database di Access archiviati all'interno del sito Web devono essere inseriti nell'App\_directory dei dati. ASP.NET non supporta i file in questa directory da visualizzare. È necessario concedere le autorizzazioni di lettura e scrittura per l'App dell'account del processo\_directory dei dati quando si usa database di Access.


## <a name="the-xmldatasource-control"></a>Il controllo XmlDataSource

XmlDataSource viene utilizzata per associare dati XML ai dati a controlli con associazione a dati. È possibile associare a un file XML usando la proprietà file di dati oppure è possibile associare a una stringa XML utilizzando la proprietà di dati. XmlDataSource espone attributi XML come campi associabili. Nei casi in cui è necessario per associare ai valori che non sono rappresentati come attributi, è necessario utilizzare una trasformazione XSL. È anche possibile usare le espressioni XPath per filtrare i dati XML.

Si consideri il seguente file XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Si noti che l'oggetto XmlDataSource Usa una proprietà XPath di *persone/persona* per filtrare solo il &lt;persona&gt; nodi. DropDownList quindi associa dati per l'attributo LastName utilizzando la proprietà DataTextField.

Mentre il controllo XmlDataSource viene principalmente usato per associare i dati di sola lettura dei dati XML, è possibile modificare il file di dati XML. Si noti che in questi casi, dall'inserimento automatico, aggiornamento ed eliminazione delle informazioni nel file XML non viene eseguito automaticamente a quanto accade con altri controlli origine dati. In alternativa, è necessario scrivere codice per modificare manualmente i dati usando i metodi seguenti del controllo XmlDataSource.

### <a name="getxmldocument"></a>Metodi GetXmlDocument

Recupera un oggetto XmlDocument contenente il codice XML recuperato dal controllo XmlDataSource.

### <a name="save"></a>Salva

Salva il modello in memoria XmlDocument nuovamente all'origine dati.

È importante tenere presente che il metodo Save funziona solo quando vengono soddisfatte le due condizioni seguenti:

1. XmlDataSource Usa la proprietà file di dati da associare a un file XML anziché la proprietà di dati da associare ai dati XML in memoria.
2. Tramite la proprietà di trasformazione o TransformFile viene specificata alcuna trasformazione.

Si noti inoltre che il metodo Save può generare risultati imprevisti quando viene chiamato da più utenti contemporaneamente.

## <a name="the-objectdatasource-control"></a>Il controllo ObjectDataSource

I controlli origine dati che sono stati descritti fino a questo punto rappresentano una scelta eccellente per le applicazioni a due livelli in cui il controllo origine dati comunica direttamente con l'archivio dati. Tuttavia, molte applicazioni reali sono applicazioni multilivello in cui un controllo origine dati potrebbe essere necessario comunicare con un oggetto business che, a sua volta comunica con il livello dati. In queste situazioni, ObjectDataSource riempie la fattura perfettamente. ObjectDataSource funziona in combinazione con un oggetto di origine. Il controllo ObjectDataSource creerà un'istanza dell'oggetto di origine, chiamare il metodo specificato e dispose dell'istanza dell'oggetto nell'ambito di una singola richiesta, se l'oggetto dispone di metodi di istanza anziché i metodi statici (Shared in Visual Basic). Pertanto, l'oggetto deve essere senza stato. Vale a dire l'oggetto deve acquisire e rilasciare tutte le risorse necessarie all'interno dell'intervallo di una singola richiesta. È possibile controllare come viene creato l'oggetto di origine gestendo l'evento ObjectCreating del controllo ObjectDataSource. È possibile creare un'istanza dell'oggetto di origine e quindi impostare la proprietà ObjectInstance della classe ObjectDataSourceEventArgs a quell'istanza. Il controllo ObjectDataSource userà l'istanza viene creata nell'evento ObjectCreating anziché creare un'istanza di per sé.

Se l'oggetto di origine per un controllo ObjectDataSource espone i metodi statici pubblici (Shared in Visual Basic) che possono essere chiamati per recuperare e modificare i dati, un controllo ObjectDataSource chiamerà tali metodi direttamente. Se un controllo ObjectDataSource deve creare un'istanza dell'oggetto di origine per effettuare chiamate al metodo, l'oggetto deve includere un costruttore pubblico che non accetta parametri. Il controllo ObjectDataSource chiamerà questo costruttore quando crea una nuova istanza dell'oggetto di origine.

Se l'oggetto di origine non contiene un costruttore pubblico senza parametri, è possibile creare un'istanza dell'oggetto di origine che verrà utilizzato dal controllo ObjectDataSource nell'evento ObjectCreating.

## <a name="specifying-object-methods"></a>Specifica dei metodi di oggetto

L'oggetto di origine per un controllo ObjectDataSource può contenere qualsiasi numero di metodi che consentono di selezionare, inserire, aggiornare o eliminare dati. Questi metodi vengono chiamati dal controllo ObjectDataSource in base al nome del metodo, come identificato dalla proprietà SelectMethod, InsertMethod, UpdateMethod o DeleteMethod del controllo ObjectDataSource. L'oggetto di origine può anche includere un metodo SelectCount facoltativo, che è identificato dal controllo ObjectDataSource usando la proprietà SelectCountMethod, che restituisce il conteggio del numero totale di oggetti nell'origine dati. Il controllo ObjectDataSource chiamerà il metodo SelectCount dopo aver chiamato un metodo Select per recuperare il numero totale di record nell'origine dati per l'uso durante la suddivisione in.

## <a name="lab-using-data-source-controls"></a>Lab usando i controlli origine dati

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Esercizio 1: la visualizzazione dei dati con il controllo SqlDataSource

L'esercizio seguente usa il controllo SqlDataSource per la connessione al database Northwind. Si presuppone di avere accesso al database Northwind in un'istanza di SQL Server 2000.

1. Creare un nuovo sito Web ASP.NET.
2. Aggiungere un nuovo file Web. config.

    1. Pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
    2. Scegliere i File di configurazione Web dall'elenco dei modelli e fare clic su Aggiungi.
3. Modificare il &lt;connectionStrings&gt; sezione come indicato di seguito: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Passare alla visualizzazione del codice e aggiungere un attributo ConnectionString e un attributo di SelectCommand per il &lt;asp: SqlDataSource&gt; controllano come indicato di seguito: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Dalla visualizzazione di progettazione, aggiungere un nuovo controllo GridView.
6. Nell'elenco a discesa Scegli origine dati nel menu attività di GridView, scegliere SqlDataSource1.
7. Fare clic su default. aspx e scegliere Visualizza nel Browser dal menu di scelta. Fare clic su Sì quando viene richiesto di salvare.
8. GridView Visualizza i dati dalla tabella Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Esercizio 2: modifica dei dati con il controllo SqlDataSource

L'esercizio seguente viene illustrato come associare i dati un controllo DropDownList controllare utilizzando la sintassi dichiarativa e consente di modificare i dati presentati nel controllo DropDownList.

1. Nella visualizzazione progettazione, eliminare il controllo GridView da default. aspx. 

    **Importante**: lasciare il controllo SqlDataSource nella pagina.
2. Aggiungere un controllo DropDownList a default. aspx.
3. Passare alla visualizzazione origine.
4. Aggiungere un attributo DataSourceId DataTextField e DataValueField per il &lt;asp: DropDownList&gt; controllano come indicato di seguito: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Salvare default. aspx e visualizzarlo nel browser. Si noti che il controllo DropDownList contiene tutti i prodotti dal database Northwind.
6. Chiudere il browser.
7. Nella vista di origine di default. aspx, aggiungere un nuovo controllo casella di testo sotto il controllo DropDownList. Modificare la proprietà ID della casella di testo a txtProductName.
8. Sotto il controllo casella di testo, aggiungere un nuovo controllo pulsante. Modificare la proprietà ID del pulsante in btnUpdate e la proprietà Text in **aggiornamento Product Name**.
9. Nella vista di origine di default. aspx, aggiungere una proprietà UpdateCommand e due nuove UpdateParameters nel tag SqlDataSource come indicato di seguito: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Si noti che esistono che due parametri (ProductName e ProductID) aggiunti in questo codice di aggiornamento. Vengono eseguito il mapping di questi parametri per la proprietà Text del txtProductName nella casella di testo e la proprietà SelectedValue di ddlProducts DropDownList.
10. Passare alla visualizzazione progettazione e fare doppio clic sul controllo pulsante per aggiungere un gestore eventi.
11. Aggiungere il codice seguente per il btnUpdate\_fare clic sul codice: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Fare clic su default. aspx e scegliere di visualizzarla nel browser. Quando viene richiesto di salvare tutte le modifiche, fare clic su Sì.
13. ASP.NET 2.0 le classi parziali consentono per la compilazione in fase di esecuzione. Non è necessario compilare un'applicazione per visualizzare le modifiche al codice applicate.
14. Selezionare un prodotto da DropDownList.
15. Immettere un nuovo nome per il prodotto selezionato nella casella di testo e quindi fare clic sul pulsante di aggiornamento.
16. Il nome del prodotto viene aggiornato nel database.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Esercizio 3 mediante il controllo ObjectDataSource

Questo esercizio illustra come usare il controllo ObjectDataSource e un oggetto di origine per interagire con il database Northwind.

1. Pulsante destro del mouse sul progetto in Esplora soluzioni e fare clic su Aggiungi nuovo elemento.
2. Selezionare Web Form nell'elenco dei modelli. Modificare il nome in object.aspx e fare clic su Aggiungi.
3. Pulsante destro del mouse sul progetto in Esplora soluzioni e fare clic su Aggiungi nuovo elemento.
4. Selezionare classe nell'elenco dei modelli. Modificare il nome della classe in NorthwindData.cs e fare clic su Aggiungi.
5. Fare clic su Sì, quando viene richiesto di aggiungere la classe per l'App\_cartella del codice.
6. Aggiungere il codice seguente al file NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Aggiungere il codice seguente alla visualizzazione origine della object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Salvare tutti i file e passare object.aspx.
9. Interagire con l'interfaccia di visualizzazione dei dettagli, modifica i dipendenti, aggiungendo i dipendenti e l'eliminazione dei dipendenti.
