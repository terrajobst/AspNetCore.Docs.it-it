---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: L'interazione con la pagina Master dalla pagina contenuto (VB) | Microsoft Docs
author: rick-anderson
description: Viene illustrato come chiamare i metodi, impostare le proprietà e così via della pagina Master dal codice nella pagina di contenuto.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 59a00305cdcaf41ac0b37649382b9c3dc9ce1b0c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829221"
---
<a name="interacting-with-the-master-page-from-the-content-page-vb"></a>L'interazione con la pagina Master dalla pagina contenuto (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Viene illustrato come chiamare i metodi, impostare le proprietà e così via della pagina Master dal codice nella pagina di contenuto.


## <a name="introduction"></a>Introduzione

Nel corso delle ultime cinque esercitazioni che è stato illustrato come creare una pagina master, definire le aree del contenuto, associare le pagine ASP.NET a una pagina master e definire il contenuto specifico della pagina. Quando un visitatore richiede una particolare pagina contenuta, il contenuto e il markup di pagine master sono combinati in fase di runtime risultanti per il rendering di una gerarchia di controllo unificato. Di conseguenza, abbiamo già visto uno dei modi in cui la pagina master e una delle relative pagine di contenuto possono interagire: la pagina contenuto illustra in dettaglio il markup da transfuse in controlli ContentPlaceHolder della pagina master.

Che cos'è ancora stato da esaminare è come le pagine master e del contenuto possono interagire a livello di codice. Oltre a definire il markup per i controlli ContentPlaceHolder della pagina master, una pagina di contenuto può anche assegnare valori alle proprietà pubbliche della relativa pagina master e richiamarne i metodi pubblici. Analogamente, una pagina master può interagire con le pagine di contenuto. Sebbene sia meno frequente l'interazione tra loro i markup dichiarativi interazione a livello di codice tra una pagina master e di contenuto, esistono molti scenari in cui è necessario tale livello di programmazione.

In questa esercitazione verranno esaminati come una pagina di contenuto a livello di codice può interagire con la pagina master. Nella prossima esercitazione si esaminerà come pagina master allo stesso modo può interagire con le pagine di contenuto.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Esempi di interazione a livello di codice tra una pagina di contenuto e la relativa pagina Master

Quando una determinata area di una pagina deve essere configurato in base a una pagina per pagina, viene usato un controllo ContentPlaceHolder. Ma per quanto riguarda casi in cui la maggior parte delle pagine necessario per generare un determinato output, ma un numero ridotto di pagine necessario personalizzarlo per mostrare qualche altro? Uno di questi esempi, che abbiamo esaminati nel [ *più elementi ContentPlaceHolder e contenuto predefinito* ](multiple-contentplaceholders-and-default-content-vb.md) esercitazione comporta la visualizzazione di un'interfaccia di accesso in ogni pagina. Mentre la maggior parte delle pagine devono includere un'interfaccia di accesso, deve essere eliminata per un numero limitato di pagine, ad esempio: la pagina di accesso principali (`Login.aspx`); la pagina Crea Account; e altre pagine accessibili solo agli utenti autenticati. Il [ *più elementi ContentPlaceHolder e contenuto predefinito* ](multiple-contentplaceholders-and-default-content-vb.md) esercitazione ha illustrato come definire il contenuto predefinito per un controllo ContentPlaceHolder nella pagina master e quindi come eseguirne l'override in tali pagine in cui il il contenuto predefinito non è stato voluto.

Un'altra opzione consiste nel creare una proprietà pubblica o un metodo all'interno della pagina master che indica se mostrare o nascondere l'interfaccia di accesso. Ad esempio, la pagina master può includere una proprietà pubblica denominata `ShowLoginUI` il cui valore è stato usato per impostare il `Visible` proprietà del controllo di accesso nella pagina master. Tali pagine di contenuto in cui deve essere eliminata l'interfaccia utente di accesso quindi è possibile impostare a livello di codice le `ShowLoginUI` proprietà `False`.

Forse l'esempio più comune di contenuto e l'interazione di pagina master si verifica quando i dati visualizzati nel pagina master deve essere aggiornato dopo un'azione sia trascorso nella pagina di contenuto. Si consideri una pagina master che include un controllo GridView che visualizza i cinque più di recente aggiunti record da una tabella di database specifico e che una delle relative pagine di contenuto include un'interfaccia per l'aggiunta di nuovi record alla tabella stessa.

Quando un utente visita la pagina per aggiungere un nuovo record, vede che le cinque aggiunto più di recente record visualizzati nella pagina master. Dopo aver immesso i valori per le colonne del nuovo record, Anna invia il form. Presupponendo che il controllo GridView nella pagina master presenti relativo `EnableViewState` proprietà è impostata su True (impostazione predefinita), il relativo contenuto viene ricaricato dallo stato di visualizzazione e, di conseguenza, gli stessi cinque record vengono visualizzati anche se un record più recente è stato appena aggiunto al database. Ciò potrebbe confondere l'utente.

> [!NOTE]
> Anche se si disabilita lo stato di visualizzazione del controllo GridView in modo che esegue nuovamente l'associazione all'origine dati sottostante a ogni postback, comunque non verrà visualizzata il record aggiunto solo perché i dati sono associati a GridView in precedenza nel ciclo di vita di pagina rispetto a quando viene aggiunto il nuovo record per il datab ambiente del servizio app.


Per risolvere il problema in modo che il record appena aggiunto viene visualizzato nella pagina master del GridView durante il postback, dobbiamo indicare a GridView per riassociare alla relativa origine dati *dopo* è stato aggiunto il nuovo record nel database. Ciò richiede l'interazione tra le pagine master e il contenuto perché è l'interfaccia per l'aggiunta che del nuovo record (e i relativi gestori di eventi) sono nella pagina di contenuto, ma il controllo GridView che deve essere aggiornato nella pagina master.

Poiché viene aggiornata la visualizzazione della pagina master da un gestore eventi nella pagina di contenuto è una delle esigenze più comuni per il contenuto e l'interazione di pagina master, verranno esaminati in questo argomento in modo più dettagliato. Il download per questa esercitazione include un database di Microsoft SQL Server 2005 Express Edition denominato `NORTHWIND.MDF` del sito Web `App_Data` cartella. Il database Northwind archivia prodotto, dipendente e informazioni sulle vendite per una società fittizia, Northwind Traders.

Passaggio 1 illustra la visualizzazione di cinque più di recente aggiunta prodotti in un controllo GridView nella pagina master. Passaggio 2 crea una pagina di contenuto per l'aggiunta di nuovi prodotti. Passaggio 3 verrà illustrato come creare metodi e proprietà pubbliche nella pagina master e il passaggio 4 viene illustrato come interagire a livello di codice con queste proprietà e metodi dalla pagina contenuto.

> [!NOTE]
> Questa esercitazione non addentrarsi nell'analisi di utilizzo dei dati in ASP.NET. I passaggi per la configurazione della pagina master visualizzare i dati e la pagina contenuta per l'inserimento di dati sono state completate, ancora breezy. Per un'analisi più approfondita la visualizzazione e inserimento di dati e tramite i controlli SqlDataSource e GridView, consultare le risorse nella sezione ulteriori letture alla fine di questa esercitazione.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Passaggio 1: Visualizzare i cinque più di recente aggiunta prodotti nella pagina Master

Aprire la pagina master Site. master e aggiungere un'etichetta e un controllo GridView per il `leftContent` `<div>`. Cancellare il contenuto dell'etichetta `Text` proprietà, impostata relativo `EnableViewState` proprietà `False`e la relativa `ID` proprietà `GridMessage`; impostare il controllo GridView `ID` proprietà `RecentProducts`. Dalla finestra di progettazione, quindi espandere la smart tag di GridView e scegliere di associarlo a una nuova origine dati. Verrà avviata la configurazione guidata origine dati. Perché il database di Northwind nel `App_Data` cartella è un database di Microsoft SQL Server, scegliere di creare un SqlDataSource selezionando (vedere la figura 1), assegnare un nome SqlDataSource `RecentProductsDataSource`.


[![Associare il controllo GridView a un controllo SqlDataSource denominato RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Figura 01**: eseguire l'associazione a un controllo SqlDataSource denominato GridView `RecentProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))


Il passaggio successivo viene chiesto di specificare quali database a cui connettersi. Scegliere il `NORTHWIND.MDF` file dall'elenco a discesa scegliere nel database e fare clic su Avanti. Poiché questa è la prima volta che abbiamo usato in questo database, offrirà la procedura guidata archiviare la stringa di connessione in `Web.config`. È di archiviare la stringa di connessione usando il nome `NorthwindConnectionString`.


[![Connettersi al Database Northwind](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Figura 02**: connettersi al Northwind Database ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))


La procedura guidata Configura origine dati offre due modi per cui è possibile specificare la query usata per recuperare i dati:

- Specificando un'istruzione SQL personalizzata o una stored procedure, o
- Selezione di una tabella o vista e specificando le colonne da restituire

Poiché si vuole restituire che solo le cinque aggiunto più di recente prodotti, è necessario specificare un'istruzione SQL personalizzata. Usare il comando seguente `SELECT` query:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

Il `TOP 5` (parola chiave) restituisce solo i primi cinque record della query. Il `Products` chiave primaria della tabella, `ProductID`, è un `IDENTITY` colonna, che potremo verificare che ogni nuovo prodotto aggiunto alla tabella avrà un valore superiore a quello della voce precedente. Pertanto, ordinare i risultati da `ProductID` restituisce i prodotti a partire da più di recente quelle create in ordine decrescente.


[![Restituisce i cinque prodotti aggiunti più di recente](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Figura 03**: restituisce i cinque più recentemente aggiunto prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))


Dopo aver completato la procedura guidata, Visual Studio genera due i BoundField di GridView visualizzare il `ProductName` e `UnitPrice` campi restituiti dal database. A questo punto markup dichiarativo della pagina master deve includere markup simile al seguente:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Come si può notare, contiene il markup: il controllo Web Label (`GridMessage`); GridView `RecentProducts`, con due BoundField; e un controllo SqlDataSource che restituisce i cinque più di recente sono stati aggiunti i prodotti.

Con questo GridView creato e il relativo controllo SqlDataSource è configurato, visitare il sito Web tramite un browser. Come illustrato nella figura 4, è possibile vedere una griglia in basso a sinistra che elenca i cinque più di recente aggiunta prodotti.


[![GridView Visualizza i cinque prodotti aggiunti più di recente](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Figura 04**: GridView Visualizza i cinque più recentemente aggiunto prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))


> [!NOTE]
> È possibile pulire l'aspetto di GridView. Alcuni suggerimenti includono la formattazione visualizzati `UnitPrice` valore come una valuta e l'utilizzo di tipi di carattere e colori di sfondo per migliorare l'aspetto della griglia.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Passaggio 2: Creazione di una pagina di contenuto per aggiungere nuovi prodotti

L'attività successiva consiste nel creare una pagina di contenuto da cui un utente può aggiungere un nuovo prodotto per il `Products` tabella. Aggiungere una nuova pagina contenuta per il `Admin` cartella denominata `AddProduct.aspx`, assicurandosi di associarlo al `Site.master` pagina master. Figura 5 Mostra Esplora soluzioni dopo l'aggiunta di questa pagina per il sito Web.


[![Aggiungere una nuova pagina di ASP.NET per la cartella Admin](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Figura 05**: aggiungere una nuova pagina ASP.NET per il `Admin` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))


Si tenga presente che nel [ *specificazione di titolo, metatag e altre intestazioni HTML nella pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) esercitazione è stata creata una classe di pagina base personalizzata denominata `BasePage` che ha generato il titolo della pagina se si tratta di impostare in modo non esplicito. Andare alla `AddProduct.aspx` code-behind della pagina classe e fare in modo che derivano da `BasePage` (anziché da `System.Web.UI.Page`).

Aggiornare infine il `Web.sitemap` file da includere una voce per questa lezione. Aggiungere il markup seguente sotto il `<siteMapNode>` per la lezione di problemi di denominazione ID controllo:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Come illustrato nella figura 6, l'aggiunta di questo `<siteMapNode>` elemento si riflette nell'elenco di lezioni.

Tornare al `AddProduct.aspx`. Nel controllo del contenuto per il `MainContent` ContentPlaceHolder, aggiungere un controllo DetailsView e denominarlo `NewProduct`. Eseguire l'associazione a un nuovo controllo SqlDataSource denominato DetailsView `NewProductDataSource`. Come con SqlDataSource nel passaggio 1, configurare la procedura guidata in modo che utilizzi il database Northwind e scegliere di specificare un'istruzione SQL personalizzata. Poiché DetailsView verrà usato per aggiungere elementi al database, è necessario specificare sia un `SELECT` istruzione e una `INSERT` istruzione. Usare il comando seguente `SELECT` query:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

Quindi, aggiungere quanto segue dalla scheda Inserisci, `INSERT` istruzione:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Dopo aver completato la procedura guidata passa al tag di DetailsView intelligente e selezionare la casella di controllo "Abilita inserimento". Questo modo viene aggiunta una CommandField DetailsView con relativo `ShowInsertButton` proprietà è impostata su True. Poiché questo controllo DetailsView verrà usato esclusivamente per l'inserimento di dati, impostare DetailsView `DefaultMode` proprietà `Insert`.

Questo è tutto qui! È possibile testare questa pagina. Visitare `AddProduct.aspx` tramite un browser, immettere un nome e il prezzo (vedere la figura 6).


[![Aggiungere un nuovo prodotto al Database](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Figura 06**: aggiungere un nuovo prodotto al Database ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))


Dopo aver digitato il nome e il prezzo per il nuovo prodotto, fare clic sul pulsante di inserimento. In questo modo il modulo di postback. Durante il del postback, il controllo SqlDataSource `INSERT` istruzione; i due parametri vengono popolati con i valori immessi dall'utente in due controlli TextBox di DetailsView. Sfortunatamente, non vi è alcun feedback visivo che si è verificata un'operazione di inserimento. Sarebbe utile avere un messaggio visualizzato, che conferma che è stato aggiunto un nuovo record. Lasciare come esercizio per il lettore. Inoltre, dopo aver aggiunto un nuovo record dal DetailsView GridView nella pagina master viene ancora visualizzato gli stessi cinque record precedente; non include il record appena aggiunti. Verrà esaminato come risolvere il problema nei prossimi passaggi.

> [!NOTE]
> Oltre ad aggiungere una forma di feedback visivo che l'inserimento ha avuto esito positivo, è consigliabile aggiornare anche l'interfaccia di inserimento di DetailsView per includere la convalida. Attualmente, non è prevista alcuna convalida. Se un utente immette un valore valido per il `UnitPrice` campo, ad esempio "troppo costoso," verrà generata un'eccezione durante il postback quando il sistema tenta di convertire tale stringa in un valore decimale. Per altre informazioni sulla personalizzazione di inserimento dell'interfaccia, vedere la [ *personalizzazione dell'interfaccia di modifica dei dati* esercitazione](https://asp.net/learn/data-access/tutorial-20-vb.aspx) dal mio [utilizzo di serie di esercitazioni dati](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Passaggio 3: Creazione di metodi e proprietà pubbliche nella pagina Master

Nel passaggio 1 è stato aggiunto un controllo etichetta Web denominato `GridMessage` sopra il controllo GridView nella pagina master. Questa etichetta consente, facoltativamente, visualizzare un messaggio. Ad esempio, dopo aver aggiunto un nuovo record per il `Products` tabella, potrebbe essere necessario visualizzare un messaggio che legge: "*ProductName* è stato aggiunto al database." Piuttosto che come hardcoded il testo per l'etichetta nella pagina master, si potrebbe essere opportuno personalizzabile per la pagina di contenuto del messaggio.

Poiché il controllo etichetta viene implementato come una variabile membro protetto all'interno della pagina master che non è possibile accedervi direttamente dalle pagine di contenuto. Per poter funzionare con l'etichetta all'interno di una pagina master dalla pagina contenuto (o parimenti, qualsiasi controllo Web nella pagina master) è necessario creare una proprietà pubblica nella pagina master che espone il controllo Web o funge da proxy che può essere una delle relative proprietà  si accede. Aggiungere la sintassi seguente alla classe code-behind della pagina master per esporre l'etichetta `Text` proprietà:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Quando viene aggiunto un nuovo record per il `Products` tabella da una pagina contenuto la `RecentProducts` GridView nella pagina master deve ripetere l'associazione all'origine dati sottostante. Per riassociare la chiamata di GridView relativo `DataBind` (metodo). Poiché il controllo GridView nella pagina master non è accessibile a livello di codice alle pagine di contenuto, è necessario creare un metodo pubblico nella pagina master che, quando viene chiamato, nuovamente l'associazione dati a GridView. Aggiungere il metodo seguente alla classe code-behind della pagina master:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Con il `GridMessageText` proprietà e `RefreshRecentProductsGrid` metodo posto, qualsiasi pagina di contenuto possa a livello di codice impostare o leggere il valore della `GridMessage` dell'etichetta `Text` proprietà o la riassociazione dei dati per il `RecentProducts` GridView. Passaggio 4 viene illustrato come accedere a metodi e proprietà pubbliche della pagina master da una pagina di contenuto.

> [!NOTE]
> Non dimenticare di contrassegnare le proprietà e metodi come la pagina master `Public`. Se si non indicare in modo esplicito queste proprietà e metodi come `Public`, non sarà accessibile dalla pagina contenuto.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Passaggio 4: Chiamata a membri pubblici della pagina Master da una pagina di contenuto

Ora che la pagina master contiene metodi e proprietà pubbliche necessari, siamo pronti per richiamare le proprietà e metodi dal `AddProduct.aspx` pagina contenuto. In particolare, è necessario impostare la pagina master `GridMessageText` proprietà e chiamare relativo `RefreshRecentProductsGrid` metodo dopo che il nuovo prodotto è stato aggiunto al database. Tutti i controlli Web dei dati ASP.NET vengono attivati gli eventi immediatamente prima e dopo il completamento di varie attività, che rendono più semplice per gli sviluppatori di pagina eseguire un'azione a livello di codice prima o dopo l'attività. Ad esempio, quando l'utente finale fa clic sul pulsante Inserisci DetailsView, sul postback il controllo DetailsView genera relativo `ItemInserting` eventi prima di iniziare l'inserimento del flusso di lavoro. Vengono quindi inseriti i record nel database. In seguito, genera DetailsView relativo `ItemInserted` evento. Pertanto, per usare la pagina master dopo che è stato aggiunto il nuovo prodotto, creare un gestore eventi per DetailsView `ItemInserted` evento.

Esistono due modi in cui una pagina di contenuto a livello di programmazione può interfacciarsi con relativa pagina master:

- Uso di `Page.Master` proprietà, che restituisce un riferimento alla pagina master non fortemente tipizzato, o
- Specificare il percorso della pagina pagina master tipo o il file tramite un `@MasterType` direttiva; ciò aggiunge automaticamente una proprietà fortemente tipizzate per la pagina dal titolo `Master`.

Verrà ora esaminato entrambi gli approcci.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Usando il non fortemente tipizzate`Page.Master`proprietà

Tutte le pagine web ASP.NET deve derivare dal `Page` (classe), che si trova nella `System.Web.UI` dello spazio dei nomi. Il `Page` classe include una [ `Master` proprietà](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) che restituisce un riferimento alla pagina principale della pagina. Se la pagina non ha una pagina master `Master` restituisce `Nothing`.

Il `Master` proprietà restituisce un oggetto di tipo [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (che si trova nella `System.Web.UI` dello spazio dei nomi) che è il tipo di base da cui derivano da tutte le pagine master. Pertanto, per usare proprietà o metodi pubblici definiti nella pagina master del nostro sito Web è necessario eseguire il cast di `MasterPage` oggetto restituito dal `Master` proprietà nel tipo appropriato. Poiché il file pagina master denominato `Site.master`, la classe code-behind è stata denominata `Site`. Pertanto, il codice seguente esegue il cast di `Page.Master` proprietà a un'istanza del `Site` classe.


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Ora che abbiamo eseguito il cast di non fortemente tipizzate `Page.Master` proprietà per il tipo di sito è possibile fare riferimento a proprietà e metodi specifici al sito. Come illustrato nella figura 7, la proprietà pubblica `GridMessageText` viene visualizzato nell'elenco a discesa IntelliSense.


[![In IntelliSense vengono mostrate le proprietà pubbliche e i metodi di pagina Master](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Figura 07**: in IntelliSense vengono mostrate le proprietà pubbliche e i metodi di pagina Master ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))


> [!NOTE]
> Se il file pagina master denominata `MasterPage.master` nome della classe code-behind della pagina master è `MasterPage`. Questo può causare ambiguo codice quando si esegue il cast dal tipo `System.Web.UI.MasterPage` per il `MasterPage` classe. In breve, è necessario qualificare completamente il tipo di cui che si esegue il cast a, che può essere un po' complesso quando si usa il modello di progetto di sito Web. Mio suggerimento sarà possibile assicurarsi che quando si crea la pagina master si assegna un nome diverso da `MasterPage.master` o, meglio ancora, creare un riferimento fortemente tipizzato per la pagina master.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Creazione di un riferimento fortemente tipizzato con la`@MasterType`(direttiva)

Se si osserva attentamente, si noterà che classe code-behind della pagina ASP.NET è una classe parziale (nota di `Partial` parola chiave nella definizione della classe). Classi parziali introdotte in c# e Visual Basic con.NET Framework 2.0 e, in breve, consentano ai membri della classe di essere definito in più file. Il file di classe code-behind - `AddProduct.aspx.vb`, ad esempio, contiene il codice, lo sviluppatore della pagina, creiamo. Oltre a nostro codice, il motore ASP.NET crea automaticamente un file di classi separate con le proprietà e gestori eventi in cui traducono il markup dichiarativo in gerarchia delle classi della pagina.

La generazione automatica di codice che si verifica ogni volta che viene visitata una pagina ASP.NET la strada per alcune possibilità piuttosto interessanti e utili. Nel caso di pagine master, se si indicano al motore ASP.NET quale pagina master sta utilizzando la pagina contenuta viene generato l'oggetto fortemente tipizzato `Master` proprietà per noi.

Usare la [ `@MasterType` direttiva](https://msdn.microsoft.com/library/ms228274.aspx) per comunicare al motore ASP.NET del tipo di pagina master alla pagina contenuto. Il `@MasterType` direttiva può accettare il nome di tipo della pagina master o al percorso del file. Per specificare che il `AddProduct.aspx` pagina utilizzi `Site.master` come pagina master, aggiungere la seguente direttiva all'inizio del `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Questa direttiva indica al motore di ASP.NET per aggiungere un riferimento fortemente tipizzato per la pagina master tramite una proprietà denominata `Master`. Con il `@MasterType` direttiva posto, è possibile chiamare il `Site.master` master della pagina proprietà e metodi pubblici direttamente tramiti il `Master` proprietà senza cast.

> [!NOTE]
> Se si omette il `@MasterType` direttiva, la sintassi `Page.Master` e `Master` restituire lo stesso risultato: un oggetto non fortemente tipizzato alla pagina principale della pagina. Se si include il `@MasterType` direttiva quindi `Master` restituisce un riferimento fortemente tipizzato per la pagina master specificata. `Page.Master`, tuttavia, restituisce comunque un riferimento non fortemente tipizzato. Per un'analisi più completa perché questo è il caso e il comportamento della `Master` proprietà viene creata quando il `@MasterType` direttiva è inclusa, vedere [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)del post di blog [ `@MasterType` in ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>L'aggiornamento della pagina Master dopo l'aggiunta di un nuovo prodotto

Ora che si sa come richiamare le proprietà pubbliche e i metodi di una pagina di contenuto di una pagina master, siamo pronti per aggiornare il `AddProduct.aspx` pagina in modo che la pagina master viene aggiornata dopo l'aggiunta di un nuovo prodotto. All'inizio del passaggio 4 è stato creato un gestore eventi per il controllo DetailsView `ItemInserting` evento, che viene eseguita immediatamente dopo il nuovo prodotto è stato aggiunto al database. Aggiungere il codice seguente al gestore eventi:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

Il codice riportato sopra Usa entrambi i non fortemente tipizzate `Page.Master` proprietà e fortemente tipizzata `Master` proprietà. Si noti che il `GridMessageText` è impostata su "*ProductName* aggiunte alla griglia..." I valori del prodotto appena aggiunti sono accessibili tramite il `e.Values` raccolta, come si può notare, l'appena aggiunte dal `ProductName` valore sono accessibili tramite `e.Values("ProductName")`.

Figura 8 mostra la `AddProduct.aspx` pagina immediatamente dopo un nuovo prodotto - di Scott Soda - è stato aggiunto al database. Si noti che il nome del prodotto appena aggiunte è indicato nell'etichetta della pagina master e che il controllo GridView è stato aggiornato per includere il prodotto e il prezzo.


[![Etichetta della pagina Master e GridView mostrano il prodotto appena aggiunta](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Figura 08**: la pagina Master etichetta e GridView viene illustrato il prodotto Just-Added ([fare clic per visualizzare l'immagine con dimensioni normali](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))


## <a name="summary"></a>Riepilogo

In teoria, una pagina master e le pagine contenuto sono completamente separate rispetto a altro e non richiedono alcun livello di interazione. Mentre le pagine master e le pagine di contenuto devono essere progettate tenendo presente questo obiettivo, esistono una serie di scenari comuni in cui una pagina di contenuto deve interfacciarsi con relativa pagina master. Uno dei motivi più comuni coinvolge l'aggiornamento di una parte specifica della visualizzazione della pagina master basata su un'azione che si sono verificati nella pagina di contenuto.

La buona notizia è che è relativamente semplice avere una pagina di contenuto a livello di programmazione interagire con la pagina master. Iniziare creando metodi o proprietà pubbliche nella pagina master che incapsulano la funzionalità che deve essere richiamato da una pagina di contenuto. Quindi, nella pagina di contenuto, accedere nella pagina master le proprietà e metodi attraverso il non fortemente tipizzate `Page.Master` proprietà oppure usare il `@MasterType` direttiva per creare un riferimento fortemente tipizzato per la pagina master.

Nella prossima esercitazione verranno esaminati con la pagina master interagire a livello di codice con una delle relative pagine di contenuto.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [L'accesso e aggiornamento dei dati in ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Pagine Master ASP.NET: Suggerimenti, trucchi e trabocchetti](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` in ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Il passaggio di informazioni tra il contenuto e le pagine Master](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Utilizzo dei dati nelle esercitazioni ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla.com, ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach manualmente ASP.NET 3.5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). È possibile contattarlo Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Zack Jones. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami un messaggio nella [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](control-id-naming-in-content-pages-vb.md)
> [Successivo](interacting-with-the-content-page-from-the-master-page-vb.md)
