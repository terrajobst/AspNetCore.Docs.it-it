---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: Configurazione delle impostazioni del livello di accesso ai dati del livello di connessione e comando (c#) | Documenti Microsoft
author: rick-anderson
description: Gli oggetti TableAdapter all'interno di un set di dati tipizzati di gestire automaticamente la connessione al database, l'emissione di comandi e la compilazione di un oggetto DataTable con i risultati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: f5f221fd792956fc21cb6eb5834299d3c5bfa80d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876111"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>Configurazione delle impostazioni del livello di accesso ai dati del livello di connessione e comando (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip) o [Scarica il PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> Gli oggetti TableAdapter all'interno di un set di dati tipizzati di gestire automaticamente la connessione al database, l'emissione di comandi e la compilazione di un oggetto DataTable con i risultati. Esistono casi, tuttavia, quando si occuperà di dettagli effettuata in questa esercitazione è imparare accedere alle impostazioni del livello di connessione e comando di database in oggetto TableAdapter.


## <a name="introduction"></a>Introduzione

In tutta la serie di esercitazioni è stata usata di dataset tipizzati per implementare il livello di accesso ai dati e oggetti business di architettura a più livelli. Come descritto nel [prima esercitazione](../introduction/creating-a-data-access-layer-cs.md), s il DataSet tipizzato DataTable fungono da archivi di dati mentre gli oggetti TableAdapter fungono da wrapper per comunicare con il database per recuperare e modificare i dati sottostanti. Gli oggetti TableAdapter incapsulare la complessità correlati all'uso con il database e ci consente di risparmiare di dover scrivere codice per la connessione al database, eseguire un comando o popolare i risultati in un oggetto DataTable.

Esistono casi, tuttavia, quando è necessario burrow all'interno dell'oggetto TableAdapter e scrivere codice che interagisce direttamente con gli oggetti ADO.NET. Nel [di wrapping delle modifiche del Database all'interno di una transazione](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) esercitazione, ad esempio, abbiamo aggiunto metodi all'oggetto TableAdapter per inizio, eseguire il commit e rollback delle transazioni di ADO.NET. Questi metodi utilizzati interna, creati manualmente `SqlTransaction` oggetto che è stato assegnato ai TableAdapter `SqlCommand` oggetti.

In questa esercitazione verrà esaminato accedere alle impostazioni del livello di connessione e comando di database in oggetto TableAdapter. In particolare, si verrà aggiunta la funzionalità per il `ProductsTableAdapter` che consente di accedere alla stringa di connessione sottostante e le impostazioni di timeout di comando.

## <a name="working-with-data-using-adonet"></a>Utilizzo di dati tramite ADO.NET

Microsoft .NET Framework contiene moltissime classi progettate specificamente per l'utilizzo con i dati. Queste classi, trovate all'interno di [ `System.Data` dello spazio dei nomi](https://msdn.microsoft.com/library/system.data.aspx), vengono definiti il *ADO.NET* classi. Alcune classi sotto l'ombrello ADO.NET sono collegati a un determinato *provider di dati*. È possibile considerare un provider di dati come un canale di comunicazione che consente di informazioni tra le classi ADO.NET e l'archivio dati sottostante. Esistono provider generalizzato, ad esempio OLE DB e ODBC, nonché provider appositamente progettati per un sistema di database specifico. Ad esempio, sebbene sia possibile connettersi a un database di Microsoft SQL Server tramite il provider OLE DB, il provider SqlClient è molto più efficiente poiché è stato progettato e ottimizzato in modo specifico per SQL Server.

Quando si accede a livello di codice ai dati, viene utilizzato in genere il modello seguente:

- Stabilire una connessione al database.
- Eseguire un comando.
- Per `SELECT` query, lavorare con i record risultanti.

Esistono classi ADO.NET separate per l'esecuzione di ognuno di questi passaggi. Per connettersi a un database tramite il provider SqlClient, ad esempio, utilizzare il [ `SqlConnection` classe](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Da rilasciare un `INSERT`, `UPDATE`, `DELETE`, o `SELECT` comando nel database, utilizzare il [ `SqlCommand` classe](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Fatta eccezione per il [di wrapping delle modifiche del Database all'interno di una transazione](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) esercitazione, non abbiamo scrivere codice ADO.NET di basso livello effettuata poiché il codice generato automaticamente gli oggetti TableAdapter include le funzionalità necessarie per connettersi al database, eseguire i comandi, recuperare i dati e popolare i dati in DataTable. Tuttavia, potrebbero esserci delle volte quando è necessario personalizzare queste impostazioni di basso livello. In passaggi successivi verrà esaminato interagire con gli oggetti ADO.NET utilizzati internamente per gli oggetti TableAdapter.

## <a name="step-1-examining-with-the-connection-property"></a>Passaggio 1: Esame con la proprietà di connessione

Ogni classe TableAdapter include un `Connection` proprietà che specifica le informazioni di connessione di database. Questo tipo di dati di proprietà s e `ConnectionString` valore dipendono dalle selezioni effettuate con la configurazione guidata TableAdapter. Tenere presente che quando si aggiunge un oggetto TableAdapter a un DataSet tipizzato questa procedura guidata chiesto di specificare il database di origine (vedere la figura 1). L'elenco di riepilogo a discesa in questo passaggio prima include i database specificati nel file di configurazione, nonché ad altri database in Esplora Server s connessioni dati. Se il database a cui che si desidera utilizzare non esiste nell'elenco a discesa, è possibile specificare una nuova connessione al database facendo clic sul pulsante nuova connessione e fornendo le informazioni di connessione necessarie.


[![Il primo passaggio della configurazione guidata TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**Figura 1**: il primo passaggio della configurazione guidata TableAdapter ([fare clic per visualizzare l'immagine ingrandita](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))


Consentono di s è opportuno esaminare il codice per i TableAdapter `Connection` proprietà. Come indicato nel [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione, è possibile visualizzare il codice del TableAdapter generate automaticamente selezionando la finestra di visualizzazione classi, drill-down la classe appropriata e quindi fare doppio clic sul nome del membro.

Passare alla finestra di visualizzazione classi dal menu Visualizza e scegliendo Visualizzazione classi (o digitando Ctrl + Maiusc + C). Dalla parte superiore della finestra di visualizzazione classi, eseguire il drill-down di `NorthwindTableAdapters` dello spazio dei nomi e selezionare il `ProductsTableAdapter` classe. Verrà visualizzata la `ProductsTableAdapter` membri s nella parte inferiore della metà della visualizzazione classi, come illustrato nella figura 2. Fare doppio clic su di `Connection` proprietà per visualizzare il codice.


![Fare doppio clic su proprietà di connessione in visualizzazione classi per visualizzare il codice generato automaticamente](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**Figura 2**: fare doppio clic su proprietà di connessione in visualizzazione classi per visualizzare il codice generato automaticamente


I TableAdapter `Connection` proprietà e altre segue di codice relativi alla connessione:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

Quando viene creata un'istanza della classe TableAdapter, la variabile membro `_connection` è uguale a `null`. Quando il `Connection` si accede a proprietà, viene controllato innanzitutto se il `_connection` è stata creata un'istanza di variabile membro. In caso contrario, il `InitConnection` metodo viene richiamato, che crea un'istanza `_connection` e imposta il relativo `ConnectionString` proprietà per il valore di stringa di connessione specificato nel passaggio prima di TableAdapter configurazione guidata s.

Il `Connection` proprietà può anche essere assegnata a un `SqlConnection` oggetto. In questo modo associa il nuovo `SqlConnection` oggetto con oggetti TableAdapter `SqlCommand` oggetti.

## <a name="step-2-exposing-connection-level-settings"></a>Passaggio 2: Esposizione di impostazioni a livello di connessione

Le informazioni di connessione devono rimanere incapsulate all'interno dell'oggetto TableAdapter e non sia accessibile agli altri livelli di architettura dell'applicazione. Tuttavia, potrebbe essere scenari quando le informazioni di connessione a livello di TableAdapter s devono essere accessibile o personalizzabile per una query, un utente o una pagina ASP.NET.

Consente di estendere s il `ProductsTableAdapter` nel `Northwind` set di dati da includere un `ConnectionString` proprietà che può essere usata dal livello di logica di Business per leggere o modificare la stringa di connessione utilizzata dal TableAdapter.

> [!NOTE]
> Oggetto *stringa di connessione* è una stringa che specifica informazioni di connessione di database, ad esempio il provider da usare, il percorso del database, le credenziali di autenticazione e altre impostazioni relative al database. Per un elenco di modelli di stringa di connessione utilizzata da un'ampia gamma di archivi dati e provider, vedere [ConnectionStrings.com](http://www.connectionstrings.com/).


Come descritto nel [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione, è possibile estendere le classi di DataSet tipizzati s generato automaticamente tramite l'utilizzo di classi parziali. Innanzitutto, creare una nuova sottocartella nel progetto denominato `ConnectionAndCommandSettings` sotto il `~/App_Code/DAL` cartella.


![Aggiungere una sottocartella denominata ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**Figura 3**: aggiungere una sottocartella denominata `ConnectionAndCommandSettings`


Aggiungere un nuovo file di classe denominato `ProductsTableAdapter.ConnectionAndCommandSettings.cs` e immettere il codice seguente:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

Questa classe parziale viene aggiunto un `public` proprietà denominata `ConnectionString` per la `ProductsTableAdapter` classe che consente a qualsiasi livello leggere o aggiornare la stringa di connessione per i TableAdapter connessione sottostante.

Con questa classe parziale creato e salvato, aprire la `ProductsBLL` classe. Passare a uno dei metodi e digitare `Adapter` e quindi premere il tasto punto per visualizzare IntelliSense. Verrà visualizzato il nuovo `ConnectionString` proprietà disponibili in IntelliSense, vale a dire che a livello di codice è possibile leggere o modificare questo valore di BLL.

## <a name="exposing-the-entire-connection-object"></a>Esposizione di oggetti l'intera connessione

Questa classe parziale espone solo una delle proprietà dell'oggetto connessione sottostante: `ConnectionString`. Se si desidera rendere disponibile oltre i confini dell'oggetto TableAdapter oggetto l'intera connessione, in alternativa è possibile modificare il `Connection` il livello di protezione di proprietà s. Il codice generato automaticamente è esaminati nel passaggio 1 ha dimostrato che i TableAdapter `Connection` proprietà è contrassegnata come `internal`, vale a dire che è possibile accedere solo dalle classi nello stesso assembly. Questo può essere modificato, tuttavia, tramite i TableAdapter `ConnectionModifier` proprietà.

Aprire il `Northwind` set di dati, fare clic su di `ProductsTableAdatper` nella finestra di progettazione e passare alla finestra Proprietà. Si verrà visualizzato il `ConnectionModifier` impostato sul valore predefinito, `Assembly`. Per rendere il `Connection` proprietà disponibile di fuori dell'assembly s DataSet tipizzato, modifica il `ConnectionModifier` proprietà `Public`.


[![Il livello di accessibilità s di proprietà di connessione può essere configurato tramite la proprietà ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**Figura 4**: il `Connection` proprietà s accessibilità livello può essere configurato tramite il `ConnectionModifier` proprietà ([fare clic per visualizzare l'immagine ingrandita](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))


Salvare il set di dati e quindi tornare al `ProductsBLL` classe. Come prima, passare a uno dei metodi e digitare `Adapter` e quindi premere il tasto punto per visualizzare IntelliSense. L'elenco deve includere un `Connection` ne consegue che è possibile ora a livello di codice di lettura o assegnare le impostazioni a livello di connessione da BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Passaggio 3: Esaminare le proprietà relative al comando

Un oggetto TableAdapter è costituito da una query principale che, per impostazione predefinita, ha generato automaticamente `INSERT`, `UPDATE`, e `DELETE` istruzioni. La query principale s `INSERT`, `UPDATE`, e `DELETE` istruzioni sono implementate nel codice s TableAdapter come un oggetto adattatore di dati ADO.NET tramite la `Adapter` proprietà. Come con il relativo `Connection` proprietà, il `Adapter` il tipo di dati di proprietà s è determinato dal provider di dati utilizzato. Poiché il provider SqlClient, non utilizzano queste esercitazioni di `Adapter` proprietà è di tipo [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

I TableAdapter `Adapter` proprietà ha tre proprietà di tipo `SqlCommand` che usa per problema di `INSERT`, `UPDATE`, e `DELETE` istruzioni:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Oggetto `SqlCommand` oggetto è responsabile per l'invio di una particolare query al database e dispone di proprietà quali: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), che contiene la stored procedure da eseguire; o l'istruzione SQL ad hoc e [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), che è una raccolta di `SqlParameter` oggetti. Come abbiamo visto nuovamente il [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione, questi comandi oggetti possono essere personalizzati tramite la finestra Proprietà.

Oltre alle query principale, il TableAdapter può includere un numero variabile di metodi che, quando richiamata, inviare un comando specificato per il database. L'oggetto comando query principale s e gli oggetti comando per tutti i metodi aggiuntivi vengono archiviati nei TableAdapter `CommandCollection` proprietà.

Consentono di s è opportuno esaminare il codice generato dal `ProductsTableAdapter` nel `Northwind` set di dati per queste due proprietà e i relativi supporto variabili membro e i metodi di supporto:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

Il codice per il `Adapter` e `CommandCollection` strettamente riproduce che di proprietà di `Connection` proprietà. Sono disponibili variabili di membro che contengono gli oggetti utilizzati dalla proprietà. Le proprietà `get` funzioni di accesso per iniziare, verifica innanzitutto se la variabile membro corrispondente è `null`. In questo caso, viene chiamato un metodo di inizializzazione che crea un'istanza della variabile membro e assegna il nucleo della proprietà relative ai comandi.

## <a name="step-4-exposing-command-level-settings"></a>Passaggio 4: Esposizione di impostazioni a livello di comando

In teoria, le informazioni a livello di comando devono rimanere incapsulate all'interno del livello di accesso ai dati. Queste informazioni devono essere necessarie in altri livelli dell'architettura, tuttavia, può essere esposta tramite una classe parziale, proprio come con le impostazioni a livello di connessione.

Poiché il TableAdapter dispone solo di un singolo `Connection` proprietà, il codice per l'esposizione di impostazioni a livello di connessione è piuttosto semplice. Gli aspetti sono un po' più complessi quando si modificano le impostazioni a livello di comando perché il TableAdapter può avere più oggetti comando - un `InsertCommand`, `UpdateCommand`, e `DeleteCommand`, insieme a un numero di oggetti del comando nella variabile di `CommandCollection` proprietà. Durante l'aggiornamento delle impostazioni a livello di comando, è necessario essere propagate a tutti gli oggetti comando queste impostazioni.

Si supponga, ad esempio, che non vi sono determinate query in TableAdapter che ha richiesto un straordinario lunghi tempi di esecuzione. Quando si utilizza il TableAdapter per eseguire uno di tali query, potrebbe essere necessario aumentare l'oggetto comando s [ `CommandTimeout` proprietà](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Questa proprietà specifica il numero di secondi di attesa per l'esecuzione del comando e il valore predefinito 30.

Per consentire il `CommandTimeout` proprietà affinché venga regolato da BLL, aggiungere il seguente `public` metodo il `ProductsDataTable` utilizzando il file di classe parziale creato nel passaggio 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`):


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

Questo metodo potrebbe essere richiamato dal livello di presentazione o BLL per impostare il timeout del comando per tutti i problemi di comandi da quell'istanza TableAdapter.

> [!NOTE]
> Il `Adapter` e `CommandCollection` proprietà vengono contrassegnate come `private`, vale a dire siano accessibili solo dal codice all'interno dell'oggetto TableAdapter. A differenza di `Connection` proprietà questi modificatori di accesso non sono configurabili. Pertanto, se si desidera esporre le proprietà a livello di comando per gli altri livelli dell'architettura è necessario utilizzare l'approccio di classe parziale in precedenza per fornire un `public` metodo o proprietà che legge o scrive il `private` oggetti comando.


## <a name="summary"></a>Riepilogo

Gli oggetti TableAdapter all'interno di un DataSet tipizzato può essere utilizzato per incapsulare dettagli di accesso ai dati e la complessità. Utilizzando gli oggetti TableAdapter, è non è necessario preoccuparsi di scrivere codice ADO.NET per connettersi al database, eseguire un comando o popolare i risultati in un oggetto DataTable. Viene tutti gestita automaticamente per noi.

Tuttavia, potrebbero esserci delle volte quando è necessario personalizzare le specifiche ADO.NET basso livello, ad esempio la modifica della stringa di connessione o valori di timeout connessione o un comando predefinito. Il TableAdapter è generato automaticamente `Connection`, `Adapter`, e `CommandCollection` proprietà, ma questi sono `internal` o `private`, per impostazione predefinita. Tali informazioni possono essere esposti mediante l'estensione di TableAdapter utilizzando classi parziali per includere `public` metodi o proprietà. In alternativa, i TableAdapter `Connection` il modificatore di accesso di proprietà può essere configurato tramite i TableAdapter `ConnectionModifier` proprietà.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Portare i revisori per questa esercitazione sono stati Burnadette Leigh, S ren Jacob Lauritsen, Teresa Murphy e Hilton Geisenow. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](working-with-computed-columns-cs.md)
> [Successivo](protecting-connection-strings-and-other-configuration-information-cs.md)
