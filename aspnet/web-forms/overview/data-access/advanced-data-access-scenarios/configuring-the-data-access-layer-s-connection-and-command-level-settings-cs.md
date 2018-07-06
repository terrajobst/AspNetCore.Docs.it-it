---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: Configurazione delle impostazioni a livello di connessione e di comando del livello di accesso ai dati (c#) | Microsoft Docs
author: rick-anderson
description: Gli oggetti TableAdapter in un DataSet tipizzato eseguono automaticamente la connessione al database, invio di comandi e popolamento di un oggetto DataTable con i risultati...
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: 835f17c35d85658a855072bf6d9355fdae9f63ab
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837837"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>Configurazione delle impostazioni a livello di connessione e di comando del livello di accesso ai dati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip) o [Scarica il PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> Gli oggetti TableAdapter in un DataSet tipizzato eseguono automaticamente la connessione al database, invio di comandi e popolamento di un oggetto DataTable con i risultati. Esistono alcuni casi tuttavia, quando si vogliono occuparsi di questi dettagli noi stessi in questa esercitazione viene illustrato come accedere alle impostazioni a livello di connessione e di comando di database in oggetto TableAdapter.


## <a name="introduction"></a>Introduzione

In tutta la serie di esercitazioni è stato utilizzato di dataset tipizzati per implementare il livello di accesso ai dati e oggetti business della nostra architettura a più livelli. Come descritto nel [prima esercitazione](../introduction/creating-a-data-access-layer-cs.md), s DataSet tipizzata DataTable fungono da archivi di dati mentre gli oggetti TableAdapter fungono da wrapper per comunicare con il database per recuperare e modificare i dati sottostanti. Gli oggetti TableAdapter incapsulano la complessità correlati all'uso con il database e ci risparmia di dover scrivere codice per connettersi al database, eseguire un comando o popolare i risultati in un oggetto DataTable.

Esistono casi, tuttavia, quando occorre burrow nelle profondità dell'oggetto TableAdapter e scrivere codice che interagisce direttamente con gli oggetti ADO.NET. Nel [di wrapping delle modifiche al Database in una transazione](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) esercitazione, ad esempio, abbiamo aggiunto metodi all'oggetto TableAdapter per l'inizio, eseguire il commit e rollback delle transazioni di ADO.NET. Utilizzare un interno, questi metodi creati manualmente `SqlTransaction` oggetto a cui è stato assegnato ai TableAdapter `SqlCommand` oggetti.

In questa esercitazione verrà esaminato come accedere alle impostazioni del database a livello di connessione e di comando in TableAdapter. In particolare, si aggiungeranno funzionalità per il `ProductsTableAdapter` che consente di accedere alla stringa di connessione sottostante e le impostazioni del timeout di comando.

## <a name="working-with-data-using-adonet"></a>Utilizzo di dati tramite ADO.NET

Microsoft .NET Framework include una vasta gamma di classi progettata specificamente per funzionare con i dati. Queste classi, trovate all'interno di [ `System.Data` dello spazio dei nomi](https://msdn.microsoft.com/library/system.data.aspx), sono detti il *ADO.NET* classi. Alcune delle classi in quello ADO.NET sono legate a un determinato *provider di dati*. È possibile considerare un provider di dati come un canale di comunicazione che consente le informazioni vengano trasmesse tra le classi ADO.NET e l'archivio dati sottostante. Sono disponibili provider generalizzato, ad esempio OleDb e ODBC, nonché provider appositamente progettati per un sistema di database specifico. Ad esempio, sebbene sia possibile connettersi a un database di Microsoft SQL Server utilizzando il provider OLE DB, il provider SqlClient è molto più efficiente perché è stato progettato e ottimizzato in modo specifico per SQL Server.

Quando si accede a livello di programmazione dei dati, viene usato in genere il modello seguente:

- Stabilire una connessione al database.
- Eseguire un comando.
- Per `SELECT` query, lavorare con i record risultanti.

Esistono classi ADO.NET separate per l'esecuzione di ognuno di questi passaggi. Per connettersi a un database tramite il provider SqlClient, ad esempio, usare il [ `SqlConnection` classe](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Da rilasciare un `INSERT`, `UPDATE`, `DELETE`, o `SELECT` comando per il database, usare il [ `SqlCommand` classe](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Fatta eccezione per il [di wrapping delle modifiche al Database in una transazione](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) esercitazione abbiamo non abbiamo scritto uno ADO.NET di basso livello di codice noi perché il codice generato automaticamente gli oggetti TableAdapter include la funzionalità necessaria per connettersi al database, eseguire i comandi, recuperare i dati e popolare i dati in DataTable. Tuttavia, potrebbero esserci casi quando è necessario personalizzare queste impostazioni di basso livello. Tramite i passaggi successivi verranno esaminate come sfrutta gli oggetti ADO.NET utilizzati internamente per gli oggetti TableAdapter.

## <a name="step-1-examining-with-the-connection-property"></a>Passaggio 1: Esame con la proprietà di connessione

Ogni classe TableAdapter include un `Connection` proprietà che specifica le informazioni di connessione di database. Questo tipo di dati di proprietà s e `ConnectionString` valore dipendono dalle selezioni effettuate nella configurazione guidata TableAdapter. È importante ricordare che quando si aggiunta prima di tutto un oggetto TableAdapter a un DataSet tipizzato questa procedura guidata chiede us per il database di origine (vedere la figura 1). L'elenco a discesa in questo primo passaggio include i database specificati nel file di configurazione, nonché ad altri database in Esplora Server s connessioni dati. Se non esiste nell'elenco a discesa scegliere il database da utilizzare, è possibile specificare una nuova connessione al database facendo clic sul pulsante nuova connessione e fornendo le informazioni di connessione necessarie.


[![Il primo passaggio della configurazione guidata TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**Figura 1**: il primo passaggio della configurazione guidata TableAdapter ([fare clic per visualizzare l'immagine con dimensioni normali](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))


Let s si consiglia di esaminare il codice per i TableAdapter `Connection` proprietà. Come indicato nella [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione, è possibile visualizzare il codice del TableAdapter generate automaticamente passando alla finestra di visualizzazione classi, eseguire il drill-down la classe appropriata e quindi fare doppio clic sul nome del membro.

Passare alla finestra di visualizzazione classi, selezionando il menu di visualizzazione e scegliere Visualizzazione classi (oppure digitando Ctrl + Maiusc + C). Dalla metà superiore della finestra Visualizzazione classi, eseguire il drill-down per il `NorthwindTableAdapters` dello spazio dei nomi e selezionare il `ProductsTableAdapter` classe. Verrà visualizzata la `ProductsTableAdapter` membri s nella parte inferiore della metà di visualizzazione classi, come illustrato nella figura 2. Fare doppio clic il `Connection` proprietà per visualizzare il relativo codice.


![Fare doppio clic su proprietà di connessione in Class View per visualizzare il codice generato automaticamente](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**Figura 2**: fare doppio clic su proprietà di connessione in Class View per visualizzare il codice generato automaticamente


I TableAdapter `Connection` proprietà e altri segue di codice correlati alla connessione:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

Quando viene creata un'istanza della classe TableAdapter, la variabile membro `_connection` è uguale a `null`. Quando la `Connection` si accede alla proprietà, viene innanzitutto verificato se il `_connection` variabile membro è stata creata l'istanza. In caso contrario, il `InitConnection` metodo viene richiamato, che crea un'istanza `_connection` e imposta il `ConnectionString` proprietà per il valore di stringa di connessione specificato dal primo passaggio TableAdapter configurazione guidata s.

Il `Connection` proprietà può anche essere assegnata a un `SqlConnection` oggetto. In questo modo consente di associare il nuovo `SqlConnection` oggetto con ciascuno dei TableAdapter `SqlCommand` oggetti.

## <a name="step-2-exposing-connection-level-settings"></a>Passaggio 2: Esposizione di impostazioni a livello di connessione

Le informazioni di connessione devono restare incapsulate all'interno del TableAdapter e non sia accessibile ad altri livelli dell'architettura dell'applicazione. Tuttavia, potrebbero esserci scenari quando le informazioni di connessione a livello di TableAdapter s devono essere accessibile o è personalizzabile per una query, un utente o una pagina ASP.NET.

Let s estendere la `ProductsTableAdapter` nella `Northwind` set di dati da includere un `ConnectionString` proprietà che può essere usata dal livello della logica di Business per leggere o modificare la stringa di connessione utilizzata dal TableAdapter.

> [!NOTE]
> Oggetto *stringa di connessione* è una stringa che specifica informazioni di connessione di database, ad esempio il provider da usare, il percorso del database, le credenziali di autenticazione e altre impostazioni relative al database. Per un elenco dei modelli di stringa di connessione utilizzata da un'ampia gamma di archivi dati e provider, vedere [ConnectionStrings.com](http://www.connectionstrings.com/).


Come descritto nel [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione, le classi generate automaticamente di DataSet tipizzato s possono essere esteso tramite l'uso di classi parziali. In primo luogo, creare una nuova sottocartella nel progetto denominato `ConnectionAndCommandSettings` sotto il `~/App_Code/DAL` cartella.


![Aggiungere una sottocartella denominata ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**Figura 3**: aggiungere una sottocartella denominata `ConnectionAndCommandSettings`


Aggiungere un nuovo file di classe denominato `ProductsTableAdapter.ConnectionAndCommandSettings.cs` e immettere il codice seguente:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

Questa classe parziale consente di aggiungere un `public` proprietà denominata `ConnectionString` per il `ProductsTableAdapter` classe che consente a qualsiasi livello leggere o aggiornare la stringa di connessione per i TableAdapter connessione sottostante.

Con questa classe parziale creato e salvato, aprire il `ProductsBLL` classe. Passare a uno dei metodi esistenti e digitare `Adapter` e quindi premere il tasto punto per visualizzare IntelliSense. Verrà visualizzato il nuovo `ConnectionString` proprietà disponibili in IntelliSense, il che significa che a livello di codice è possibile leggere o modificare questo valore dalla classe BLL.

## <a name="exposing-the-entire-connection-object"></a>Che espone l'oggetto connessione intero

Questa classe parziale espone solo una delle proprietà dell'oggetto connessione sottostante: `ConnectionString`. Se si desidera rendere disponibile oltre i confini dell'oggetto TableAdapter l'oggetto intera connessione, in alternativa è possibile modificare il `Connection` a livello di protezione proprietà s. Il codice generato automaticamente abbiamo esaminati nel passaggio 1 ha dimostrato che i TableAdapter `Connection` proprietà è contrassegnata come `internal`, vale a dire che essere accessibile solo dalle classi nello stesso assembly. Ciò può essere modificata, tuttavia, tramite i TableAdapter `ConnectionModifier` proprietà.

Aprire il `Northwind` set di dati, fare clic su di `ProductsTableAdatper` nella finestra di progettazione e passare alla finestra Proprietà. Si vedrà il `ConnectionModifier` impostata sul valore predefinito, `Assembly`. Per rendere il `Connection` proprietà disponibile di fuori dell'assembly s DataSet tipizzato, modificare il `ConnectionModifier` proprietà `Public`.


[![Il livello di accessibilità s di proprietà di connessione può essere configurato tramite la proprietà ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**Figura 4**: il `Connection` proprietà s accessibilità livello può essere configurato tramite il `ConnectionModifier` proprietà ([fare clic per visualizzare l'immagine con dimensioni normali](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))


Salvare il set di dati e quindi tornare al `ProductsBLL` classe. Come prima, andare in uno dei metodi esistenti e digitare `Adapter` e quindi premere il tasto punto per visualizzare IntelliSense. L'elenco dovrebbe includere un `Connection` proprietà, vale a dire che è possibile ora a livello di codice di lettura o assegnare le impostazioni a livello di connessione da BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Passaggio 3: Esaminare le proprietà relative ai comandi

Un oggetto TableAdapter è costituito da una query principale che, per impostazione predefinita, ha generato automaticamente `INSERT`, `UPDATE`, e `DELETE` istruzioni. Questa query principale 1!s `INSERT`, `UPDATE`, e `DELETE` le istruzioni vengono implementate nel codice s TableAdapter come un oggetto adattatore di dati ADO.NET tramite la `Adapter` proprietà. Come con relativi `Connection` proprietà, il `Adapter` proprietà s tipo di dati dipende dal provider di dati usato. Poiché queste esercitazioni usano il provider SqlClient, il `Adapter` proprietà è di tipo [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

I TableAdapter `Adapter` proprietà dispone di tre proprietà di tipo `SqlCommand` che usa per problema il `INSERT`, `UPDATE`, e `DELETE` istruzioni:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Oggetto `SqlCommand` oggetto è responsabile per l'invio di una particolare query al database e dispone di proprietà, ad esempio: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), che contiene l'istruzione SQL ad hoc o stored procedure da eseguire; e [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), che è una raccolta di `SqlParameter` oggetti. Come abbiamo visto nella [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione, questi comandi di oggetti possono essere personalizzati tramite la finestra Proprietà.

Oltre alle query principali, TableAdapter può includere un numero variabile di metodi che, quando richiamata, inviare un comando specificato nel database. L'oggetto comando query principale s e gli oggetti comando per tutti i metodi aggiuntivi vengono archiviati in ai TableAdapter `CommandCollection` proprietà.

Let s si consiglia di esaminare il codice generato dal `ProductsTableAdapter` nella `Northwind` set di dati per queste due proprietà e le variabili membro supporto e metodi helper:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

Il codice per il `Adapter` e `CommandCollection` proprietà riproduce fedelmente del `Connection` proprietà. Sono presenti variabili membro che contengono gli oggetti utilizzati dalla proprietà. La proprietà `get` funzioni di accesso per iniziare, verifica innanzitutto se la variabile membro corrispondente è `null`. In questo caso, viene chiamato un metodo di inizializzazione che crea un'istanza della variabile membro e assegna il core le proprietà relative ai comandi.

## <a name="step-4-exposing-command-level-settings"></a>Passaggio 4: Esposizione di impostazioni a livello di comando

In teoria, le informazioni a livello di comando devono rimanere incapsulate all'interno di livello di accesso ai dati. Queste informazioni devono essere necessarie in altri livelli dell'architettura, tuttavia, può essere esposti tramite una classe parziale, proprio come con le impostazioni a livello di connessione.

Poiché il TableAdapter ha solo un singolo `Connection` proprietà, il codice per l'esposizione di impostazioni a livello di connessione è piuttosto semplice. Le cose sono un po' più complesse quando si modificano le impostazioni a livello di comando perché il TableAdapter può avere più oggetti comando - un' `InsertCommand`, `UpdateCommand`, e `DeleteCommand`, insieme a un numero variabile di oggetti comando nel `CommandCollection` proprietà. Quando si aggiornano le impostazioni a livello di comando, dovrà essere propagate a tutti gli oggetti comando queste impostazioni.

Si supponga, ad esempio, che non vi sono alcune query in oggetto TableAdapter che ha richiesto un straordinario lungo tempo di esecuzione. Quando si usa l'oggetto TableAdapter per eseguire una di queste query, potrebbe essere necessario aumentare l'oggetto comando 1!s [ `CommandTimeout` proprietà](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Questa proprietà specifica il numero di secondi di attesa per il comando da eseguire e valore predefinito è 30.

Per consentire la `CommandTimeout` proprietà affinché venga regolato per il livello BLL, aggiungere quanto segue `public` metodo per il `ProductsDataTable` utilizzando il file di classe parziale creato nel passaggio 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`):


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

Questo metodo può essere richiamato dal livello BLL o livello di presentazione per impostare il timeout del comando per tutti i problemi di comandi da quell'istanza TableAdapter.

> [!NOTE]
> Il `Adapter` e `CommandCollection` le proprietà sono contrassegnate come `private`, vale a dire che è accessibile solo dal codice all'interno del TableAdapter. A differenza di `Connection` proprietà, questi modificatori di accesso non sono configurabili. Pertanto, se si desidera esporre le proprietà a livello di comando per gli altri livelli dell'architettura è necessario usare l'approccio della classe parziale in precedenza per fornire una `public` metodo o proprietà che legge o scrive il `private` oggetti command.


## <a name="summary"></a>Riepilogo

Gli oggetti TableAdapter all'interno di un set di dati tipizzati consentono di incapsulare dettagli accesso ai dati e la complessità. Usa gli oggetti TableAdapter, non abbiamo preoccuparsi di scrivere codice ADO.NET per connettersi al database, eseguire un comando o popolare i risultati in un oggetto DataTable. Viene tutto gestito automaticamente per noi.

Tuttavia, potrebbero esserci volte quando è necessario personalizzare le specifiche ADO.NET a basso livello, ad esempio la modifica della stringa di connessione o i valori di timeout connessione o un comando predefinito. Il TableAdapter ha generato automaticamente `Connection`, `Adapter`, e `CommandCollection` delle proprietà, ma questi sono entrambi `internal` o `private`, per impostazione predefinita. Queste informazioni interne possono essere esposti mediante l'estensione di TableAdapter utilizzando classi parziali per includere `public` metodi o proprietà. In alternativa, gli oggetti TableAdapter `Connection` modificatore di accesso di proprietà può essere configurati tramite i TableAdapter `ConnectionModifier` proprietà.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Burnadette Leigh, S ren Jacob Lauritsen, Teresa Murphy e Hilton Geisenow. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](working-with-computed-columns-cs.md)
> [Successivo](protecting-connection-strings-and-other-configuration-information-cs.md)
