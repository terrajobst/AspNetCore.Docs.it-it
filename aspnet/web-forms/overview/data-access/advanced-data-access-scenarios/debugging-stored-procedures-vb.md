---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: Debug di Stored procedure (VB) | Documenti Microsoft
author: rick-anderson
description: Edizioni di Visual Studio Professional e Team System consentono di impostare punti di interruzione ed eseguire un'istruzione stored procedure all'interno di SQL Server, rendendo il debug archiviate...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: 3391a78eaeb0add46e75048069a614ba00628f67
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876449"
---
<a name="debugging-stored-procedures-vb"></a>Debug di Stored procedure (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip) o [Scarica il PDF](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> Edizioni di Visual Studio Professional e Team System consentono di impostare punti di interruzione ed eseguire un'istruzione stored procedure all'interno di SQL Server, rendendo il debug di stored procedure semplice come il debug di codice dell'applicazione. Questa esercitazione viene illustrato il debug diretto di un database e il debug dell'applicazione di stored procedure.


## <a name="introduction"></a>Introduzione

Visual Studio offre un'esperienza di debug. Con alcuni tasti o di clic del mouse, è possibile utilizzare i punti di interruzione per arrestare l'esecuzione di un programma ed esaminare il flusso di controllo e stato s. Con il debug di codice dell'applicazione, Visual Studio offre supporto per il debug di stored procedure da SQL Server. Esattamente come è possibile impostare punti di interruzione all'interno del codice di una classe code-behind ASP.NET o di una classe di livello di logica di Business, in modo eccessivo può essere inseriti all'interno di stored procedure.

In questa esercitazione verranno esaminati l'esecuzione di istruzioni nella stored procedure da Esplora Server all'interno di Visual Studio e come impostare punti di interruzione raggiunti quando la stored procedure viene chiamata dall'applicazione ASP.NET in esecuzione.

> [!NOTE]
> Sfortunatamente, stored procedure possono solo essere eseguito l'accesso e il debug tramite le versioni di sistemi Professional e Team di Visual Studio. Se si utilizza la versione standard di Visual Studio o Visual Web Developer, sono formulati come esaminare i passaggi necessari per eseguire il debug di stored procedure, ma non sarà in grado di replicare questi passaggi nel computer.


## <a name="sql-server-debugging-concepts"></a>Concetti di debug di SQL Server

Microsoft SQL Server 2005 è stato progettato per fornire l'integrazione con il [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), ovvero il runtime utilizzato da tutti gli assembly .NET. Di conseguenza, SQL Server 2005 supporta oggetti di database gestiti. Ovvero, è possibile creare oggetti di database quali stored procedure e funzioni definite dall'utente (UDF) come metodi in una classe di Visual Basic. In questo modo le stored procedure e funzioni definite dall'utente per usare la funzionalità di .NET Framework e dalle classi personalizzate. Naturalmente, SQL Server 2005 fornisce anche supporto per gli oggetti di database di T-SQL.

SQL Server 2005 offre il supporto del debug per T-SQL e oggetti di database gestiti. Tuttavia, questi oggetti possono solo eseguire il debug nelle diverse edizioni di Visual Studio 2005 Professional e Team sistemi. In questa esercitazione verrà esaminato gli oggetti di database di debug T-SQL. L'esercitazione successiva esamina il debug di oggetti di database gestiti.

Il [Panoramica di T-SQL e il debug CLR in SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) dal post di blog di [team integrazione con CLR di SQL Server 2005](https://blogs.msdn.com/sqlclr/default.aspx) evidenzia i tre modi per eseguire il debug di oggetti di SQL Server 2005 da Visual Studio:

- **Indirizzare il debug di Database (DDD)** - da Esplora Server, è possibile eseguire qualsiasi oggetto di database di T-SQL, ad esempio stored procedure e funzioni definite dall'utente. Esamineremo DDD nel passaggio 1.
- **Debug di applicazioni** -è possibile impostare i punti di interruzione all'interno di un oggetto di database e quindi eseguire l'applicazione ASP.NET. Quando viene eseguito l'oggetto di database, il punto di interruzione verrà raggiunto e attivato il controllo al debugger. Si noti che con il debug dell'applicazione è possibile eseguire in un oggetto di database dal codice dell'applicazione. È necessario impostare i punti di interruzione in modo esplicito in tali stored procedure o funzioni definite dall'utente in cui il debugger per interrompere. Il debug dell'applicazione viene esaminato avvio nel passaggio 2.
- **Il debug da un progetto SQL Server** -le edizioni di Visual Studio Professional e Team sistemi sono un tipo di progetto di SQL Server che viene comunemente utilizzato per creare oggetti di database gestiti. Verranno presi in esame usando i progetti di SQL Server e il debug dei contenuti nella prossima esercitazione.

Visual Studio può eseguire il debug di stored procedure in istanze di SQL Server locali e remote. Un'istanza locale di SQL Server è installato nello stesso computer di Visual Studio. Se non si trova il database di SQL Server in uso nel computer di sviluppo, viene considerato un'istanza remota. Per queste esercitazioni utilizziamo istanze di SQL Server locale. Debug di stored procedure in un'istanza remota di SQL server richiede ulteriori passaggi di configurazione rispetto a quando il debug di stored procedure in un'istanza locale.

Se si utilizza un'istanza di SQL Server locale, è possibile iniziare con il passaggio 1 e questa esercitazione alla fine. Se si utilizza un'istanza remota di SQL Server, tuttavia, sarà innanzitutto necessario accertarsi che durante il debug è connessi al computer di sviluppo con un account utente di Windows che dispone di un account di accesso di SQL Server nell'istanza remota. Moveover, sia l'accesso al database e l'accesso al database utilizzata per connettersi al database dell'applicazione ASP.NET in esecuzione devono essere membri del `sysadmin` ruolo. Alla fine di questa esercitazione per ulteriori informazioni sulla configurazione di Visual Studio e SQL Server per eseguire il debug di un'istanza remota, vedere gli oggetti di Database di debug T-SQL nella sezione istanze Remote.

Infine, comprendere che il debug di supporto per gli oggetti di database di T-SQL non è come funzionalità avanzata come supporto per le applicazioni .NET per il debug. Ad esempio, le condizioni di punto di interruzione e i filtri non sono supportati, solo un subset di finestre di debug sono disponibili, è possibile usare modifica e continuazione, viene eseguito il rendering di finestra di controllo immediato inutile e così via. Vedere [limitazioni sui comandi del Debugger e le funzionalità](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) per ulteriori informazioni.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Passaggio 1: Esecuzione di istruzioni direttamente in una Stored Procedure

Visual Studio è facile eseguire direttamente il debug di un oggetto di database. Consente di esaminare come utilizzare la funzionalità di debug di Database diretto (DDD) per eseguire l'istruzione s il `Products_SelectByCategoryID` stored procedure nel database Northwind. Come suggerisce il nome, `Products_SelectByCategoryID` restituisce informazioni sul prodotto per una particolare categoria; è stata creata nel [utilizzando Stored procedure esistenti per gli oggetti TableAdapter s DataSet tipizzato](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) esercitazione. Avviare, passare a Esplora Server espandere il nodo del database Northwind. Successivamente, drill-down della cartella Stored Procedures, fare clic su di `Products_SelectByCategoryID` stored procedure e scegliere l'opzione Esegui Stored Procedure dal menu di scelta rapida. Verrà avviato il debugger.

Poiché il `Products_SelectByCategoryID` stored procedure prevede che un `@CategoryID` parametro di input, viene chiesto di fornire questo valore. Immettere 1, che restituirà informazioni di bibite.


![Utilizzare il valore 1 per il @CategoryID parametro](debugging-stored-procedures-vb/_static/image1.png)

**Figura 1**: usare il valore 1 per il `@CategoryID` parametro


Dopo aver fornito il valore per il `@CategoryID` parametro, la stored procedure viene eseguita. Anziché tramite l'esecuzione fino al completamento, tuttavia, il debugger interrompe l'esecuzione della prima istruzione. Si noti la freccia gialla nel margine, che indica la posizione corrente nella stored procedure. È possibile visualizzare e modificare i valori dei parametri tramite la finestra Espressioni di controllo o spostando il puntatore sul nome del parametro nella stored procedure.


[![Il Debugger ha interrotto la prima istruzione di Stored Procedure](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**Figura 2**: il Debugger ha interrotto la prima istruzione di Stored Procedure ([fare clic per visualizzare l'immagine ingrandita](debugging-stored-procedures-vb/_static/image4.png))


Per esaminare la stored procedure di un'istruzione alla volta, fare clic sul pulsante Esegui istruzione/routine nella barra degli strumenti o premere il tasto F10. Il `Products_SelectByCategoryID` stored procedure contiene un singolo `SELECT` istruzione, in modo da raggiungere F10 passare all'istruzione singola e verrà completata l'esecuzione della stored procedure. Una volta completata la stored procedure, l'output verrà visualizzato nella finestra di Output e il debugger verrà terminato.

> [!NOTE]
> Il debug T-SQL si verifica a livello di istruzione. è possibile eseguirne un `SELECT` istruzione.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Passaggio 2: Configurazione del sito Web per il debug dell'applicazione

Durante il debug di una stored procedure direttamente da Esplora Server è utile, in molti scenari si intende più eseguire il debug di stored procedure quando viene chiamato dall'applicazione ASP.NET. È possibile aggiungere i punti di interruzione a una stored procedure dall'interno di Visual Studio e quindi avviare il debug dell'applicazione ASP.NET. Quando una stored procedure con i punti di interruzione viene richiamata dall'applicazione, verrà interrotta l'esecuzione nel punto di interruzione ed è possibile visualizzare e modificare i valori dei parametri di stored procedure s e scorrere le relative istruzioni, proprio come abbiamo fatto nel passaggio 1.

È possibile avviare il debug di stored procedure chiamate dall'applicazione, è necessario indicare all'applicazione web ASP.NET per l'integrazione con il debugger di SQL Server. Per iniziare, facendo clic sul nome del sito Web in Esplora soluzioni (`ASPNET_Data_Tutorial_74_VB`). Scegliere l'opzione di pagine delle proprietà dal menu di scelta rapida, selezionare l'elemento di opzioni di avvio a sinistra e selezionare la casella di controllo di SQL Server nella sezione debugger (vedere la figura 3).


[![Casella di controllo di SQL Server nelle pagine delle proprietà dell'applicazione s](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**Figura 3**: casella di controllo di SQL Server nell'applicazione s pagine delle proprietà ([fare clic per visualizzare l'immagine ingrandita](debugging-stored-procedures-vb/_static/image7.png))


Inoltre, è necessario aggiornare la stringa di connessione di database utilizzata dall'applicazione in modo che il pool di connessioni è disabilitato. Quando una connessione a un database viene chiuso, il corrispondente `SqlConnection` oggetto viene posizionato in un pool di connessioni disponibili. Quando viene stabilita una connessione a un database, un oggetto di connessioni disponibili possono essere recuperati da questo pool anziché dover creare e definire una nuova connessione. Questo pool di oggetti di connessione è un miglioramento delle prestazioni ed è abilitato per impostazione predefinita. Tuttavia, durante il debug si desidera disattivare il pool di connessioni, in quanto l'infrastruttura di debug non viene ristabilita correttamente quando si lavora con una connessione che è stata estratta dal pool.

Al pool di connessioni disattivato, aggiornare il `NORTHWNDConnectionString` in `Web.config` in modo che includa l'impostazione `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> Dopo aver completato il debug di SQL Server tramite l'applicazione ASP.NET assicurarsi di ripristinare rimuovendo il pool di connessioni di `Pooling` impostazione dalla stringa di connessione (o impostandola su `Pooling=true` ).


A questo punto l'applicazione ASP.NET è stato configurato per consentire a Visual Studio eseguire il debug di oggetti di database di SQL Server quando viene richiamato tramite l'applicazione web. Resta consiste nell'aggiungere un punto di interruzione a una stored procedure e avviare il debug!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Passaggio 3: Aggiunta di un punto di interruzione e il debug

Aprire il `Products_SelectByCategoryID` stored procedure e impostare un punto di interruzione all'inizio del `SELECT` istruzione facendo clic sul margine nella posizione appropriata o posizionando il cursore all'inizio del `SELECT` istruzione e premendo F9. Come illustrato nella figura 4, il punto di interruzione viene visualizzato come un cerchio rosso nel margine.


[![Impostare un punto di interruzione nel Products_SelectByCategoryID Stored Procedure](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**Figura 4**: impostare un punto di interruzione nel `Products_SelectByCategoryID` Stored Procedure ([fare clic per visualizzare l'immagine ingrandita](debugging-stored-procedures-vb/_static/image10.png))


Affinché un oggetto di database SQL da sottoporre a debug tramite un'applicazione client, è fondamentale che il database deve essere configurato per supportare il debug dell'applicazione. Quando si imposta un punto di interruzione, questa impostazione deve attivata automaticamente, ma è consigliabile controllare. Fare clic su di `NORTHWND.MDF` nodo in Esplora Server. Menu di scelta rapida deve includere un menu selezionato elemento denominato il debug dell'applicazione.


![Assicurarsi che sia abilitata l'opzione di debug dell'applicazione](debugging-stored-procedures-vb/_static/image11.png)

**Figura 5**: assicurarsi che sia abilitata l'opzione di debug dell'applicazione


Con il set di punti di interruzione e l'opzione di debug dell'applicazione abilitata, sono pronti eseguire il debug di stored procedure quando viene chiamato dall'applicazione ASP.NET. Avviare il debugger, passare al menu Debug e scegliendo Avvia debug premendo F5 o facendo clic su verde riprodurre l'icona nella barra degli strumenti. Verrà avvia il debugger e avviare il sito Web.

Il `Products_SelectByCategoryID` stored procedure è stata creata nel [utilizzando Stored procedure esistenti per gli oggetti TableAdapter s DataSet tipizzato](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) esercitazione. La pagina web corrispondente (`~/AdvancedDAL/ExistingSprocs.aspx`) contiene GridView che visualizza i risultati restituiti dalla stored procedure. Visitare questa pagina tramite il browser. Al raggiungimento di pagina, il punto di interruzione di `Products_SelectByCategoryID` riscontrerà stored procedure e il controllo restituito a Visual Studio. Come nel passaggio 1, è possibile esaminare le istruzioni di s stored procedure e la visualizzazione e modificare i valori dei parametri.


[![La pagina ExistingSprocs.aspx vengono inizialmente visualizzate sia le bibite](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**Figura 6**: il `ExistingSprocs.aspx` pagina vengono inizialmente visualizzate sia le bibite ([fare clic per visualizzare l'immagine ingrandita](debugging-stored-procedures-vb/_static/image14.png))


[![La Stored Procedure s punto di interruzione viene raggiunto](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**Figura 7**: la Stored Procedure s è stato raggiunto punto di interruzione ([fare clic per visualizzare l'immagine ingrandita](debugging-stored-procedures-vb/_static/image17.png))


Come illustrato nella figura 7, il valore della finestra Espressioni di controllo di `@CategoryID` parametro è 1. In questo modo il `ExistingSprocs.aspx` pagina inizialmente Visualizza i prodotti della categoria beverages, che ha un `CategoryID` valore pari a 1. Scegliere una categoria diversa dall'elenco a discesa. Questa operazione provoca un postback ed esegue nuovamente il `Products_SelectByCategoryID` stored procedure. Il punto di interruzione viene raggiunto, ma questa volta il `@CategoryID` valore del parametro s riflette l'elemento di elenco a discesa selezionato s `CategoryID`.


[![Scegliere una categoria diversa dall'elenco a discesa](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**Figura 8**: scegliere una categoria diversa dall'elenco a discesa ([fare clic per visualizzare l'immagine ingrandita](debugging-stored-procedures-vb/_static/image20.png))


[![Il @CategoryID parametro riflette la categoria selezionata dalla pagina Web](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**Figura 9**: il `@CategoryID` parametro riflette la categoria selezionata dalla pagina Web ([fare clic per visualizzare l'immagine ingrandita](debugging-stored-procedures-vb/_static/image23.png))


> [!NOTE]
> Se il punto di interruzione di `Products_SelectByCategoryID` stored procedure non viene raggiunto durante la visita di `ExistingSprocs.aspx` pagina, assicurarsi che la casella di controllo di SQL Server è stata controllata nella sezione dell'applicazione ASP.NET s pagina delle proprietà debugger, che il pool di connessioni è stato disabilitato e che il database s. opzione di debug dell'applicazione è abilitato. Se si stanno ancora problemi, riavviare Visual Studio e riprovare.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Il debug T-SQL oggetti di Database su istanze Remote

Debug di oggetti di database tramite Visual Studio è abbastanza semplice quando l'istanza di database di SQL Server è nello stesso computer di Visual Studio. Tuttavia, se SQL Server e Visual Studio si trovano in computer diversi alcuni la configurazione è necessario ottenere tutto funziona correttamente. Esistono due attività principali che stiamo affrontate con:

- Verificare che l'account di accesso utilizzato per la connessione al database tramite ADO.NET appartenga al `sysadmin` ruolo.
- Verificare che l'account utente di Windows utilizzato da Visual Studio nel computer di sviluppo sia un account di accesso valido di SQL Server a cui appartiene il `sysadmin` ruolo.

Il primo passaggio è relativamente semplice. Innanzitutto, identificare l'account utente utilizzato per connettersi al database dall'applicazione ASP.NET e quindi aggiungere tale account di accesso da SQL Server Management Studio, il `sysadmin` ruolo.

La seconda attività richiede che l'account utente di Windows utilizzati per il debug dell'applicazione sia un account di accesso valido nel database remoto. Tuttavia, probabilità sono connessi alla propria workstation con l'account di Windows non è un account di accesso valido in SQL Server. Invece di aggiungere l'account di accesso specifico a SQL Server, una scelta migliore, è possibile designare un account utente di Windows come account di debug di SQL Server. Quindi, per eseguire il debug di un'istanza remota di SQL Server gli oggetti di database, eseguire Visual Studio con credenziali dell'account s tale account di accesso Windows.

Un esempio, dovrebbe chiarire aspetti. Si supponga che vi sia un account di Windows denominato `SQLDebug` all'interno del dominio di Windows. Dovrà essere aggiunto all'istanza remota di SQL Server come account di accesso valido e come membro di questo account il `sysadmin` ruolo. Quindi, per eseguire il debug all'istanza di SQL Server remoto da Visual Studio, si dovrebbe eseguire Visual Studio come il `SQLDebug` utente. Questa operazione eseguendo l'accesso fuori la nostra workstation, accedere come `SQLDebug`, e quindi avviare Visual Studio, ma un approccio più semplice, è possibile eseguire l'accesso per la workstation con le proprie credenziali e quindi utilizzare `runas.exe` per avviare Visual Studio come il `SQLDebug` utente. `runas.exe` consente a una particolare applicazione deve essere eseguito sotto forma di un account utente diverso. Per avviare Visual Studio come `SQLDebug`, è possibile immettere l'istruzione seguente nella riga di comando:


[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

Per informazioni più dettagliate su questo processo, vedere [Vaughn](http://betav.com/BLOG/billva/) s *pagina Web contenente s Guida di Visual Studio e SQL Server, edizione settimo* nonché [procedura: impostare le autorizzazioni di SQL Server per il debug](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Se il computer di sviluppo è in esecuzione Windows XP Service Pack 2, è necessario configurare il Firewall connessione Internet per consentire il debug remoto. [La procedura per: attivare il debug di SQL Server 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) articolo note che questa operazione comporta due passaggi: (a) nel computer host di Visual Studio, è necessario aggiungere `Devenv.exe` per l'elenco delle eccezioni e aprire la porta TCP 135; e (b) del computer remoto (SQL), è necessario aprire TCP 135 porta e aggiungere `sqlservr.exe` all'elenco delle eccezioni. Se i criteri del dominio richiedono la comunicazione di rete per essere eseguite tramite IPSec, è necessario aprire le porte UDP 4500 e UDP 500.


## <a name="summary"></a>Riepilogo

Oltre a fornire il supporto del debug per il codice dell'applicazione .NET, Visual Studio fornisce anche un'ampia gamma di opzioni di debug per SQL Server 2005. In questa esercitazione è stato esaminato due di esse: il debug diretto di Database e il debug dell'applicazione. Per eseguire direttamente il debug di un oggetto di database di T-SQL, individuare l'oggetto tramite Esplora Server, quindi fare doppio clic su di esso e scegliere Esegui. Questo viene avviato il debugger si interrompe la prima istruzione nell'oggetto di database, a quel punto è possibile esaminare le istruzioni di s oggetto e la visualizzazione e modificare i valori dei parametri. Nel passaggio 1 è utilizzato questo approccio per eseguire l'istruzione di `Products_SelectByCategoryID` stored procedure.

Il debug dell'applicazione consente di punto di interruzione direttamente all'interno di oggetti di database. Quando un oggetto di database con i punti di interruzione viene richiamato da un'applicazione client (ad esempio, un'applicazione web ASP.NET), il programma si interrompe come assume il debugger. Il debug dell'applicazione è utile in quanto Mostra più chiaramente l'azione di applicazione comporta un oggetto di database specifico da richiamare. Tuttavia, è necessario installare e configurare il debug diretto di Database rispetto a un po' più.

Possono inoltre eseguire il debug di oggetti di database tramite progetti di SQL Server. Verranno esaminati utilizzando progetti di SQL Server e come utilizzarle per creare ed eseguire il debug di oggetti di database gestiti nella prossima esercitazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](protecting-connection-strings-and-other-configuration-information-vb.md)
> [Successivo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)
