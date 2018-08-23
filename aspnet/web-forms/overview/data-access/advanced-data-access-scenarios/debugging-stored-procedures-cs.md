---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: Debug di Stored procedure (c#) | Microsoft Docs
author: rick-anderson
description: Versioni di Visual Studio Professional e Team System Edition consentono di impostare punti di interruzione e passaggio alle stored procedure all'interno di SQL Server, effettua il debug archiviate...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: a377dde0c4ed25aed549ca1f3b8eeeecece79517
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832398"
---
<a name="debugging-stored-procedures-c"></a>Debug di Stored procedure (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip) o [Scarica il PDF](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Versioni di Visual Studio Professional e Team System Edition consentono di impostare punti di interruzione e passaggio alle stored procedure all'interno di SQL Server, effettua il debug di stored procedure facile come il debug di codice dell'applicazione. Questa esercitazione illustra il debug diretto di un database e il debug dell'applicazione delle stored procedure.


## <a name="introduction"></a>Introduzione

Visual Studio offre un'esperienza di debug avanzata. Con poche sequenze di tasti o del clic del mouse, è possibile usare i punti di interruzione per arrestare l'esecuzione di un programma ed esaminare il flusso di controllo e lo stato. Insieme a debug il codice dell'applicazione, Visual Studio offre supporto per il debug di stored procedure da SQL Server. Proprio come i punti di interruzione può essere impostate all'interno del codice di una classe code-behind ASP.NET o classe di livello per la logica di Business, in modo eccessivo può essere inseriti all'interno di stored procedure.

In questa esercitazione si esaminerà l'esecuzione di istruzioni nella stored procedure da Esplora Server all'interno di Visual Studio anche come impostare punti di interruzione raggiunti quando la stored procedure viene chiamata dall'applicazione ASP.NET in esecuzione.

> [!NOTE]
> Sfortunatamente, stored procedure possono solo essere eseguite l'istruzione e il debug tramite le versioni di sistemi Professional e Team di Visual Studio. Se si usa la versione standard di Visual Studio o Visual Web Developer, siete invitati a leggere lungo perché illustra i passaggi necessari per eseguire il debug di stored procedure, ma non sarà in grado di replicare questi passaggi nel computer.


## <a name="sql-server-debugging-concepts"></a>Concetti di debug di SQL Server

Microsoft SQL Server 2005 è stato progettato per fornire l'integrazione con il [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), ovvero il runtime usato da tutti gli assembly .NET. Di conseguenza, SQL Server 2005 supporta gli oggetti di database gestito. Vale a dire, è possibile creare oggetti di database, ad esempio stored procedure e funzioni definite dall'utente (UDF) come metodi in una classe c#. In questo modo le stored procedure e funzioni definite dall'utente per usare la funzionalità in .NET Framework e da proprie classi personalizzate. Naturalmente, SQL Server 2005 fornisce inoltre supporto per gli oggetti di database T-SQL.

SQL Server 2005 offre supporto per il debug di T-SQL sia gli oggetti di database gestito. Tuttavia, questi oggetti possono solo eseguire il debug nelle diverse edizioni di Visual Studio 2005 Professional e Team di sistemi. In questa esercitazione verranno esaminate debug gli oggetti di database T-SQL. L'esercitazione successiva illustra il debug di oggetti di database gestiti.

Il [Panoramica di T-SQL e il debug di CLR in SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) post di blog dalle [team di integrazione CLR di SQL Server 2005](https://blogs.msdn.com/sqlclr/default.aspx) evidenzia i tre modi per eseguire il debug di oggetti di SQL Server 2005 da Visual Studio:

- **Indirizzare il debug di Database (DDD)** - da Esplora Server è possibile eseguire l'istruzione qualsiasi oggetto di database T-SQL, ad esempio stored procedure e funzioni definite dall'utente. Verranno esaminate DDD nel passaggio 1.
- **Debug dell'applicazione** -è possibile impostare punti di interruzione all'interno di un oggetto di database e quindi eseguire l'applicazione ASP.NET. Quando viene eseguito l'oggetto di database, il punto di interruzione verrà raggiunto e attivato il controllo al debugger. Si noti che con il debug dell'applicazione è possibile passare in un oggetto di database dal codice dell'applicazione. È necessario impostare punti di interruzione in modo esplicito in tali stored procedure o funzioni definite dall'utente in cui si desidera che il debugger per interrompere. Debug dell'applicazione viene esaminato avvio nel passaggio 2.
- **Il debug da un progetto di SQL Server** -edizioni di Visual Studio Professional e sistemi Team includono un tipo di progetto di SQL Server che viene comunemente usato per creare oggetti di database gestiti. Verranno esaminate tramite progetti di SQL Server e il debug i relativi contenuti nella prossima esercitazione.

Visual Studio può eseguire il debug di stored procedure in istanze di SQL Server locali e remote. Un'istanza di SQL Server locale è ne sono installati nello stesso computer di Visual Studio. Se non si trova il database di SQL Server in uso nel computer di sviluppo, quindi viene considerato un'istanza remota. Per queste esercitazioni usati istanze locali di SQL Server. Debug di stored procedure in un'istanza remota di SQL server richiede ulteriori passaggi di configurazione rispetto a quando il debug di stored procedure in un'istanza locale.

Se si usa un'istanza di SQL Server locale, è possibile iniziare con il passaggio 1 e funzionamento di questa esercitazione alla fine. Se si usa un'istanza remota di SQL Server, tuttavia, sarà prima necessario assicurarsi che durante il debug è effettuato l'accesso al computer di sviluppo con un account utente di Windows che dispone di un account di accesso di SQL Server nell'istanza remota. Moveover, sia l'account di accesso di database e l'accesso al database utilizzata per connettersi al database dell'applicazione ASP.NET in esecuzione devono essere membri del `sysadmin` ruolo. Alla fine di questa esercitazione per altre informazioni sulla configurazione di Visual Studio e SQL Server per eseguire il debug di un'istanza remota, vedere gli oggetti di Database di debug di T-SQL nella sezione istanze Remote.

Infine, comprendere che il supporto per oggetti di database T-SQL del debug non è come funzionalità avanzate come supporto per le applicazioni .NET per il debug. Ad esempio, le condizioni punto di interruzione e i filtri non sono supportati, solo un subset delle finestre di debug sono disponibili, è possibile usare modifica e continuazione, la finestra controllo immediato viene eseguito il rendering inutile e così via. Visualizzare [limitazioni sui comandi del Debugger e le funzionalità](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) per altre informazioni.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Passaggio 1: Eseguire istruzioni direttamente in una Stored Procedure

Visual Studio è facile eseguire direttamente il debug di un oggetto di database. Let s illustrato come usare la funzionalità di debug di Database diretto (DDD) per eseguire l'istruzione di `Products_SelectByCategoryID` stored procedure nel database Northwind. Come suggerisce il nome, `Products_SelectByCategoryID` restituisce le informazioni per una determinata categoria; è stata creata nel [utilizzando Stored procedure esistenti per DataSet tipizzata s TableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) esercitazione. Per iniziare, passare a Esplora Server ed espandere il nodo del database Northwind. Successivamente, eseguire il drill-nella cartella di Stored procedure, fare clic su di `Products_SelectByCategoryID` stored procedure e scegliere l'opzione Esegui Stored Procedure dal menu di scelta rapida. Verrà avviato il debugger.

Poiché il `Products_SelectByCategoryID` stored procedure si aspetta un `@CategoryID` parametro di input, ci viene richiesto di fornire questo valore. Immettere 1, che restituisce informazioni sulle bibite.


![Usare il valore 1 per il @CategoryID parametro](debugging-stored-procedures-cs/_static/image1.png)

**Figura 1**: usare il valore 1 per il `@CategoryID` parametro


Dopo aver fornito il valore per il `@CategoryID` parametro, la stored procedure viene eseguita. Tuttavia, anziché tramite l'esecuzione fino al completamento, il debugger interrompe l'esecuzione della prima istruzione. Si noti la freccia gialla nel margine, che indica la posizione corrente nella stored procedure. È possibile visualizzare e modificare i valori dei parametri tramite la finestra Espressioni di controllo o posizionando il puntatore sul nome del parametro nella stored procedure.


[![Il Debugger ha interrotto la prima istruzione della Stored Procedure](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**Figura 2**: il Debugger ha interrotto la prima istruzione della Stored Procedure ([fare clic per visualizzare l'immagine con dimensioni normali](debugging-stored-procedures-cs/_static/image4.png))


Per esaminare la stored procedure un'istruzione alla volta, fare clic sul pulsante Esegui istruzione/routine nella barra degli strumenti o premere il tasto F10. Il `Products_SelectByCategoryID` stored procedure contiene un singolo `SELECT` istruzione, in modo da raggiungere F10 verrà Esegui istruzione/routine di singola istruzione e completare l'esecuzione della stored procedure. Dopo il completamento della stored procedure, l'output verrà visualizzato nella finestra di Output e il debugger verrà terminato.

> [!NOTE]
> Debug di T-SQL si verifica a livello di istruzione. è possibile eseguire istruzioni un `SELECT` istruzione.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Passaggio 2: Configurazione del sito Web per il debug dell'applicazione

Durante il debug di una stored procedure direttamente da Esplora Server è utile, in molti scenari siamo più interessati a stored procedure di debug quando viene chiamata dall'applicazione ASP.NET. È possibile aggiungere i punti di interruzione a una stored procedure dall'interno di Visual Studio e quindi avviare il debug dell'applicazione ASP.NET. Quando una stored procedure con i punti di interruzione viene richiamata dall'applicazione, l'esecuzione si arresta nel punto di interruzione ed è possibile visualizzare e modificare i valori dei parametri di stored procedure s e scorrere le relative istruzioni, come abbiamo fatto nel passaggio 1.

Prima di poter iniziare il debug di stored procedure chiamate dall'applicazione, è necessario indicare all'applicazione web ASP.NET per l'integrazione con il debugger di SQL Server. Avviare facendo clic sul nome del sito Web in Esplora soluzioni (`ASPNET_Data_Tutorial_74_CS`). Scegliere l'opzione di pagine delle proprietà dal menu di scelta rapida, selezionare l'elemento di opzioni di avvio a sinistra e selezionare la casella di controllo di SQL Server nella sezione debugger (vedere la figura 3).


[![Casella di controllo di SQL Server nelle pagine delle proprietà dell'applicazione s](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**Figura 3**: casella di controllo di SQL Server nell'applicazione s pagine delle proprietà ([fare clic per visualizzare l'immagine con dimensioni normali](debugging-stored-procedures-cs/_static/image7.png))


Inoltre, è necessario aggiornare la stringa di connessione di database usata dall'applicazione in modo che il pool di connessioni è disabilitato. Quando viene chiusa una connessione a un database, il corrispondente `SqlConnection` oggetto viene posizionato in un pool di connessioni disponibili. Quando si stabilisce una connessione a un database, un oggetto di connessione disponibili possono essere recuperati da questo pool anziché dover creare e stabilire una nuova connessione. Questo pool di oggetti di connessione è un miglioramento delle prestazioni e viene abilitato per impostazione predefinita. Tuttavia, durante il debug si vuole disattivare il pool di connessioni, poiché l'infrastruttura di debug non viene ristabilita correttamente quando si lavora con una connessione che è stata estratta dal pool.

Per il pool di connessioni disattivato, aggiornare il `NORTHWNDConnectionString` nelle `Web.config` in modo che includa l'impostazione `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> Dopo aver completato il debug di SQL Server tramite l'applicazione ASP.NET assicurarsi di riattivare il pool di connessioni tramite la rimozione di `Pooling` impostazione dalla stringa di connessione (o impostandolo su `Pooling=true` ).


A questo punto l'applicazione ASP.NET è stata configurata per consentire a Visual Studio eseguire il debug di oggetti di database di SQL Server quando viene richiamato tramite l'applicazione web. L'ultima ora consiste nell'aggiungere un punto di interruzione a una stored procedure e avviare il debug.

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Passaggio 3: Aggiunta di un punto di interruzione e il debug

Aprire il `Products_SelectByCategoryID` stored procedure e impostare un punto di interruzione all'inizio del `SELECT` istruzione, fare clic sul margine nella posizione appropriata o posizionando il cursore all'inizio del `SELECT` istruzione e premendo F9. Come illustrato nella figura 4, il punto di interruzione viene visualizzato come un cerchio rosso sul margine.


[![Impostare un punto di interruzione il Products_SelectByCategoryID Stored Procedure](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**Figura 4**: impostare un punto di interruzione il `Products_SelectByCategoryID` Stored Procedure ([fare clic per visualizzare l'immagine con dimensioni normali](debugging-stored-procedures-cs/_static/image10.png))


Affinché un oggetto di database SQL da sottoporre a debug attraverso un'applicazione client, è fondamentale che il database deve essere configurato per supportare il debug dell'applicazione. Quando si imposta un punto di interruzione, questa impostazione deve essere cambiata automaticamente, ma è consigliabile controllare nuovamente. Fare clic su di `NORTHWND.MDF` nodo in Esplora Server. Menu di scelta rapida deve includere un menu di selezione elemento denominato il debug dell'applicazione.


![Assicurarsi che sia abilitata l'opzione di debug dell'applicazione](debugging-stored-procedures-cs/_static/image11.png)

**Figura 5**: assicurarsi che sia abilitata l'opzione di debug dell'applicazione


Con il set di punti di interruzione e l'opzione di debug dell'applicazione abilitata, siamo pronti eseguire il debug di stored procedure quando viene chiamato dall'applicazione ASP.NET. Avviare il debugger, passare al menu Debug e scegliendo Avvia debug, premendo F5 o facendo clic su verde icona Riproduci sulla barra degli strumenti. Verrà avvia il debugger e avviare il sito Web.

Il `Products_SelectByCategoryID` stored procedure è stata creata nel [utilizzando Stored procedure esistenti per DataSet tipizzata s TableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) esercitazione. La pagina web corrispondente (`~/AdvancedDAL/ExistingSprocs.aspx`) contiene GridView che visualizza i risultati restituiti dalla stored procedure. Visitare questa pagina tramite il browser. Al raggiungimento di pagina, il punto di interruzione il `Products_SelectByCategoryID` verranno raggiunti stored procedure e il controllo restituito a Visual Studio. Proprio come nel passaggio 1, è possibile esaminare le istruzioni s stored procedure e la visualizzazione e modificare i valori dei parametri.


[![La pagina ExistingSprocs.aspx Visualizza inizialmente le bibite](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**Figura 6**: il `ExistingSprocs.aspx` pagina Visualizza inizialmente le bibite ([fare clic per visualizzare l'immagine con dimensioni normali](debugging-stored-procedures-cs/_static/image14.png))


[![La Stored Procedure s è stato raggiunto punto di interruzione](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**Figura 7**: la Stored Procedure s è stato raggiunto punto di interruzione ([fare clic per visualizzare l'immagine con dimensioni normali](debugging-stored-procedures-cs/_static/image17.png))


Come illustrato nella figura 7, il valore di finestra Espressioni di controllo di `@CategoryID` parametro è 1. Infatti, il `ExistingSprocs.aspx` pagina Visualizza inizialmente i prodotti della categoria beverages, che ha un `CategoryID` valore pari a 1. Scegliere una categoria diversa nell'elenco a discesa. Questa operazione causa un postback ed esegue nuovamente il `Products_SelectByCategoryID` stored procedure. Il punto di interruzione viene raggiunto, ma questa volta il `@CategoryID` valore del parametro s riflette l'elemento di elenco selezionato di elenco a discesa s `CategoryID`.


[![Scegliere una categoria diversa nell'elenco a discesa](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**Figura 8**: scegliere una categoria diversa dall'elenco a discesa scegliere ([fare clic per visualizzare l'immagine con dimensioni normali](debugging-stored-procedures-cs/_static/image20.png))


[![Il @CategoryID parametro riflette la categoria selezionata dalla pagina Web](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**Figura 9**: il `@CategoryID` parametro riflette la categoria selezionata dalla pagina Web ([fare clic per visualizzare l'immagine con dimensioni normali](debugging-stored-procedures-cs/_static/image23.png))


> [!NOTE]
> Se il punto di interruzione nel `Products_SelectByCategoryID` stored procedure non viene raggiunto durante la visita di `ExistingSprocs.aspx` pagina, assicurarsi che la casella di controllo di SQL Server è stata selezionata nella sezione dell'applicazione ASP.NET s pagina delle proprietà del debugger, che è stato pool di connessioni disabilitato o che il database s opzione di debug dell'applicazione è abilitato. Se viene nuovamente ancora problemi, riavviare Visual Studio e riprovare.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Debug di oggetti di Database T-SQL su istanze Remote

Debug di oggetti di database tramite Visual Studio è piuttosto semplice, quando l'istanza di database di SQL Server è nello stesso computer di Visual Studio. Tuttavia, se SQL Server e Visual Studio si trovano in computer diversi quindi alcune operazioni di configurazione attenta deve assicurarsi che tutto funzioni correttamente. Esistono due attività principali che vengono affrontate con:

- Assicurarsi che l'account di accesso utilizzato per la connessione al database tramite ADO.NET appartenga al `sysadmin` ruolo.
- Assicurarsi che l'account utente Windows usato da Visual Studio nel computer di sviluppo sia un account di accesso valido di SQL Server a cui appartiene il `sysadmin` ruolo.

Il primo passaggio è relativamente semplice. Prima di tutto identificare l'account utente usato per connettersi al database dall'applicazione ASP.NET e quindi, aggiungere tale account di accesso di SQL Server Management Studio il `sysadmin` ruolo.

La seconda attività richiede che l'account utente Windows usato per eseguire il debug dell'applicazione sia un account di accesso valido nel database remoto. Tuttavia, probabile che l'account di Windows per l'accesso alla workstation con non è un account di accesso valido nel Server SQL. Anziché aggiungere l'account di accesso specifico a SQL Server, una scelta migliore sarebbe quello di designare un account utente di Windows come account di debug di SQL Server. Quindi, per eseguire il debug di oggetti di database di un'istanza remota di SQL Server, eseguire Visual Studio usando le credenziali dell'account s tale account di accesso Windows.

Ad esempio, dovrebbe chiarire le cose. Si supponga che vi sia un account di Windows denominato `SQLDebug` all'interno del dominio di Windows. Questo account dovranno essere aggiunti all'istanza remota di SQL Server come account di accesso valido e come membro del `sysadmin` ruolo. Quindi, per eseguire il debug di istanza di SQL Server remoto da Visual Studio, è necessario eseguire Visual Studio come le `SQLDebug` utente. Questa operazione potrebbe essere effettuata la disconnessione dal nostro workstation, accedere come `SQLDebug`, e quindi avviare Visual Studio, ma un approccio più semplice, è possibile accedere al nostro workstation usando le proprie credenziali e quindi usare `runas.exe` per avviare Visual Studio come il `SQLDebug` utente. `runas.exe` consente a una determinata applicazione deve essere eseguito sotto forma di un account utente diverso. Per avviare Visual Studio come `SQLDebug`, è possibile immettere l'istruzione seguente nella riga di comando:


[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

Per una spiegazione più dettagliata su questo processo, vedere [Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker ' s Guide to Visual Studio e SQL Server, settima edizione* nonché [How To: Set di autorizzazioni di SQL Server Per eseguire il debug](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Se il computer di sviluppo è in esecuzione Windows XP Service Pack 2 è necessario configurare il Firewall connessione Internet per consentire il debug remoto. [La procedura per: attivare il debug di SQL Server 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) articolo mette in rilievo che questa operazione comporta due passaggi: (a) nel computer host Visual Studio, è necessario aggiungere `Devenv.exe` per l'elenco delle eccezioni e aprire la porta TCP 135; e (b) nel computer remoto (SQL), è necessario aprire TCP 135 porta e aggiungere `sqlservr.exe` all'elenco delle eccezioni. Se i criteri del dominio richiedono la comunicazione di rete vengano eseguite tramite IPSec, è necessario aprire le porte UDP 4500 e UDP 500.


## <a name="summary"></a>Riepilogo

Oltre a fornire il supporto del debug per il codice dell'applicazione .NET, Visual Studio offre anche un'ampia gamma di opzioni di debug per SQL Server 2005. In questa esercitazione abbiamo esaminato due di queste opzioni: debug diretto di Database e il debug dell'applicazione. Per eseguire direttamente il debug di un oggetto di database T-SQL, individuare l'oggetto tramite Esplora Server, quindi fare clic su di esso e scegliere Esegui istruzione. Verrà avviato il debugger e il processo si interromperà la prima istruzione nell'oggetto di database, a questo punto è possibile esaminare le istruzioni s oggetto e la visualizzazione e modificare i valori dei parametri. Nel passaggio 1 viene usato questo approccio per eseguire l'istruzione di `Products_SelectByCategoryID` stored procedure.

Debug dell'applicazione consente di punto di interruzione direttamente all'interno di oggetti di database. Quando un oggetto di database con i punti di interruzione viene richiamato da un'applicazione client (ad esempio, un'applicazione web ASP.NET), il programma si interrompe quando subentra il debugger. Debug dell'applicazione è utile perché illustra in modo più chiaro quale azione di applicazione fa sì che un oggetto di database specifico da richiamare. Tuttavia, è necessario un po' più configurazione e installazione di debug diretto di Database.

Gli oggetti di database possono anche eseguire il debug tramite progetti di SQL Server. Verrà esaminato tramite progetti di SQL Server e come usarli per creare ed eseguire il debug di oggetti di database gestiti nella prossima esercitazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](protecting-connection-strings-and-other-configuration-information-cs.md)
> [Successivo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
