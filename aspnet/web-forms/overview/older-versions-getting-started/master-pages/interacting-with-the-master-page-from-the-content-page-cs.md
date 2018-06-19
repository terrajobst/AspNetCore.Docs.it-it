---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
title: L'interazione con la pagina Master dalla pagina contenuta (c#) | Documenti Microsoft
author: rick-anderson
description: Viene illustrato come chiamare i metodi, impostare le proprietà e così via della pagina Master dal codice nella pagina di contenuto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 32d54638-71b2-491d-81f4-f7417a13a62f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
msc.type: authoredcontent
ms.openlocfilehash: b550d8c2a64bb2ad91e1db7b2c25433f73dbd5b7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890844"
---
<a name="interacting-with-the-master-page-from-the-content-page-c"></a>L'interazione con la pagina Master dalla pagina contenuta (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_CS.pdf)

> Viene illustrato come chiamare i metodi, impostare le proprietà e così via della pagina Master dal codice nella pagina di contenuto.


## <a name="introduction"></a>Introduzione

Nel corso delle ultime cinque esercitazioni che sono state osservate come creare una pagina master, definire le aree di contenuto, associare le pagine ASP.NET a una pagina master e definire il contenuto specifico della pagina. Quando un visitatore richiede una particolare pagina contenuta, il contenuto e il markup di pagine master sono fusibile in fase di esecuzione, determinando il rendering di una gerarchia di un controllo unificato. Pertanto, abbiamo già visto uno dei modi in cui la pagina master e una delle relative pagine di contenuto possono interagire: la pagina di contenuto vengono presentate le il markup per transfuse nei controlli ContentPlaceHolder della pagina master.

Abbiamo ancora da esaminare è come le pagine master e contenuto possono interagire a livello di codice. Inoltre, per definire il markup per i controlli ContentPlaceHolder della pagina master, una pagina di contenuto può inoltre assegnare valori alle proprietà pubbliche della pagina master e richiamare i metodi pubblici. Analogamente, una pagina master può interagire con le pagine di contenuto. Durante l'interazione a livello di codice tra una pagina master e di contenuto è meno frequente l'interazione tra i markup dichiarativo, esistono molti scenari in cui è necessario tale livello di programmazione.

In questa esercitazione verranno esaminati modalità di interazione a livello di codice una pagina di contenuto con la pagina master. Nella prossima esercitazione verranno esaminati modalità di interazione allo stesso modo con le pagine di contenuto della pagina master.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Esempi di interazione a livello di codice tra una pagina di contenuto e la relativa pagina Master

Quando una determinata area di una pagina deve essere configurato in base a una pagina per pagina, utilizziamo un controllo ContentPlaceHolder. Ma cosa succede situazioni in cui la maggior parte delle pagine necessario per generare un determinato output, ma un numero ridotto di pagine necessario personalizzarlo per mostrare un altro? Un esempio, che sono state esaminate nel [ *diversi ContentPlaceHolder e il contenuto predefinito* ](multiple-contentplaceholders-and-default-content-cs.md) esercitazione comporta la visualizzazione di un'interfaccia di accesso in ogni pagina. Sebbene la maggior parte delle pagine devono includere un'interfaccia di accesso, deve essere eliminata per un numero limitato di pagine, ad esempio: pagina di accesso principale (`Login.aspx`); la pagina; creare un Account e le altre pagine che sono solo accessibili agli utenti autenticati. Il [ *diversi ContentPlaceHolder e contenuto predefinito* ](multiple-contentplaceholders-and-default-content-cs.md) esercitazione è stato illustrato come definire il contenuto predefinito per un controllo ContentPlaceHolder nella pagina master e quindi come eseguire l'override in tali pagine in cui il il contenuto predefinito non è stato necessario.

Un'altra opzione consiste nel creare una proprietà pubblica o un metodo all'interno della pagina master che indica se mostrare o nascondere l'interfaccia di accesso. Ad esempio, la pagina master potrebbe includere una proprietà pubblica denominata `ShowLoginUI` il cui valore è stato utilizzato per impostare il `Visible` proprietà del controllo di accesso nella pagina master. Queste pagine contenute in cui l'interfaccia utente di accesso deve essere eliminata può quindi impostare a livello di codice il `ShowLoginUI` proprietà `false`.

L'esempio più comune di contenuto e l'interazione di pagina master, ad esempio si verifica quando i dati visualizzati nella pagina master deve essere aggiornato dopo un'azione sia trascorso nella pagina di contenuto. Considerare l'aggiunta di una pagina master che include un controllo GridView che visualizza più di recente i cinque record da una tabella di database specifico e che una delle relative pagine di contenuto include un'interfaccia per l'aggiunta di nuovi record alla tabella stessa.

Quando un utente visita la pagina per aggiungere un nuovo record, vede che cinque aggiunto più di recente record visualizzati nella pagina master. Dopo aver compilato i valori per le colonne del nuovo record, e invia il form. Supponendo che il controllo GridView nella pagina master è relativo `EnableViewState` proprietà è impostata su true (impostazione predefinita), il relativo contenuto viene ricaricato dallo stato di visualizzazione e, di conseguenza, gli stessi cinque record vengono visualizzati anche se un record più recente è stato appena aggiunto al database. Ciò potrebbe confondere l'utente.

> [!NOTE]
> Anche se si disabilita lo stato di visualizzazione del controllo GridView in modo che esegue nuovamente l'associazione all'origine dati sottostante a ogni postback, ancora non mostra il record appena aggiunto perché l'associazione dati a GridView nel ciclo di vita di pagina rispetto a quando il nuovo record viene aggiunto per il datab ASE.


Per risolvere questo problema in modo che il record appena aggiunto viene visualizzato nella pagina master del controllo GridView. è necessario indicare GridView riassociazione all'origine dati di postback *dopo* del nuovo record è stato aggiunto al database. È necessaria l'interazione tra il contenuto e pagine master perché l'interfaccia per l'aggiunta che del nuovo record (e i relativi gestori di eventi) sono in GridView che deve essere aggiornato, ma la pagina di contenuto è nella pagina master.

Poiché viene aggiornata la visualizzazione della pagina master da un gestore eventi nella pagina di contenuto è una delle esigenze più comuni per il contenuto e l'interazione di pagina master, vediamo in modo più dettagliato in questo argomento. Il download per questa esercitazione include un database di Microsoft SQL Server 2005 Express Edition denominato `NORTHWIND.MDF` del sito Web `App_Data` cartella. Il database Northwind archivia le informazioni sulle vendite per una società fittizia, Northwind Traders, dipendente e prodotto.

Passaggio 1 illustra la visualizzazione di cinque più di recente aggiunti prodotti in un controllo GridView nella pagina master. Passaggio 2 crea una pagina contenuto per l'aggiunta di nuovi prodotti. Passaggio 3 viene descritto come creare i metodi e proprietà pubbliche nella pagina master e il passaggio 4 di seguito viene illustrato come interfaccia a livello di codice con queste proprietà e metodi della pagina di contenuto.

> [!NOTE]
> In questa esercitazione non approfondire le specifiche di utilizzo dei dati in ASP.NET. I passaggi per l'impostazione della pagina master visualizzare i dati e la pagina contenuta per l'inserimento di dati sono state completate, ancora breezy. Per una descrizione più approfondita di visualizzazione e l'inserimento di dati e utilizzando i controlli SqlDataSource e GridView, consultare le risorse nella sezione ulteriori letture alla fine di questa esercitazione.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Passaggio 1: Visualizzare i cinque più di recente aggiunti prodotti nella pagina Master

Aprire il `Site.master` pagina master e aggiungere un'etichetta e un controllo GridView per il `leftContent` `<div>`. Cancellare il contenuto dell'etichetta `Text` impostata, il relativo `EnableViewState` la proprietà su false e il relativo `ID` proprietà `GridMessage`; impostare il controllo GridView `ID` proprietà `RecentProducts`. Dalla finestra di progettazione, quindi espandere smart tag di GridView e scegliere di associarlo a una nuova origine dati. Verrà avviata la configurazione guidata origine dati. Poiché il database Northwind di `App_Data` cartella è un database di Microsoft SQL Server, scegliere di creare un SqlDataSource selezionando (vedere Figura 1); nome SqlDataSource `RecentProductsDataSource`.


[![Associare un controllo SqlDataSource denominato RecentProductsDataSource controllo GridView.](interacting-with-the-master-page-from-the-content-page-cs/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image1.png)

**Figura 01**: associare un controllo SqlDataSource denominato di GridView `RecentProductsDataSource` ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-master-page-from-the-content-page-cs/_static/image3.png))


Il passaggio successivo viene richiesto di specificare quali database a cui connettersi. Scegliere il `NORTHWIND.MDF` database file nell'elenco a discesa e fare clic su Avanti. Poiché questa è la prima volta, abbiamo utilizzato il database, la procedura guidata propone di archiviare la stringa di connessione in `Web.config`. È di archiviare la stringa di connessione utilizzando il nome `NorthwindConnectionString`.


[![Connettersi al Database Northwind](interacting-with-the-master-page-from-the-content-page-cs/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image4.png)

**Figura 02**: la connessione al Northwind Database ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-master-page-from-the-content-page-cs/_static/image6.png))


La configurazione guidata origine dati fornisce due modalità con cui è possibile specificare la query utilizzata per recuperare i dati:

- Specificando un'istruzione SQL o stored procedure, o
- Selezione di una tabella o vista, quindi specificando le colonne da restituire

Poiché si desidera restituire che solo cinque aggiunto più di recente prodotti, è necessario specificare un'istruzione SQL personalizzata. Utilizzare la query SELECT seguente:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample1.sql)]

Il `TOP 5` (parola chiave) restituisce solo i primi cinque record della query. Il `Products` chiave primaria della tabella, `ProductID`, è un `IDENTITY` colonna che potremo verificare che ogni nuovo prodotto aggiunta alla tabella avrà un valore maggiore di quello della voce precedente. Pertanto, ordinare i risultati da `ProductID` in ordine decrescente restituisce i prodotti a partire da più di recente quelle create.


[![Restituisce i cinque prodotti aggiunti più di recente](interacting-with-the-master-page-from-the-content-page-cs/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image7.png)

**Figura 03**: restituisce i cinque più recentemente aggiunto prodotti ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-master-page-from-the-content-page-cs/_static/image9.png))


Dopo aver completato la procedura guidata, Visual Studio genera due BoundField di GridView visualizzare il `ProductName` e `UnitPrice` i campi restituiti dal database. A questo punto markup dichiarativo della pagina master deve includere codice simile al seguente:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample2.aspx)]

Come si può notare, contiene il markup: il controllo etichetta Web (`GridMessage`); GridView `RecentProducts`, con due BoundField; e un controllo SqlDataSource che restituisce i prodotti sono stati aggiunti cinque più di recente.

Con questo GridView creato e il controllo SqlDataSource configurato, visitare il sito Web tramite un browser. Come illustrato nella figura 4, si noterà una griglia in basso a sinistra che elenca i cinque più di recente aggiunti prodotti.


[![GridView Visualizza cinque prodotti aggiunti più di recente](interacting-with-the-master-page-from-the-content-page-cs/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image10.png)

**Figura 04**: GridView Visualizza cinque più recentemente aggiunto prodotti ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-master-page-from-the-content-page-cs/_static/image12.png))


> [!NOTE]
> È possibile pulire l'aspetto del controllo GridView. Alcuni suggerimenti includono la formattazione visualizzato `UnitPrice` valore come valuta e l'utilizzo di tipi di carattere e colori di sfondo per migliorare l'aspetto della griglia.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Passaggio 2: Creazione di una pagina contenuto per aggiungere nuovi prodotti

L'attività successiva consiste nel creare una pagina di contenuto da cui un utente può aggiungere un nuovo prodotto per il `Products` tabella. Aggiungere una nuova pagina contenuta per il `Admin` cartella denominata `AddProduct.aspx`, rendendo di associarlo al `Site.master` pagina master. Figura 5 Mostra Esplora soluzioni dopo l'aggiunta di questa pagina per il sito Web.


[![Aggiungere una nuova pagina ASP.NET per la cartella Admin](interacting-with-the-master-page-from-the-content-page-cs/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image13.png)

**Figura 05**: aggiungere una nuova pagina ASP.NET per il `Admin` cartella ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-master-page-from-the-content-page-cs/_static/image15.png))


Tenere presente che nel [ *che specifica il titolo, tag Meta e altre intestazioni HTML della pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) esercitazione è stata creata una classe della pagina base personalizzata denominata `BasePage` che ha generato il titolo della pagina se si trattasse di impostare in modo non esplicito. Passare al `AddProduct.aspx` codice della pagina classe e fare in modo che derivano da `BasePage` (anziché da `System.Web.UI.Page`).

Infine, aggiornare il `Web.sitemap` file da includere una voce per questa lezione. Aggiungere il markup seguente sotto il `<siteMapNode>` della lezione problemi denominazione ID di controllo:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample3.xml)]

Come illustrato nella figura 6, l'aggiunta di questo `<siteMapNode>` elemento si riflette nell'elenco lezioni.

Tornare al `AddProduct.aspx`. Nel controllo del contenuto per il `MainContent` ContentPlaceHolder, aggiungere un controllo DetailsView e denominarla `NewProduct`. Associare il controllo DetailsView a un nuovo controllo SqlDataSource denominato `NewProductDataSource`. Come con SqlDataSource nel passaggio 1, configurare la procedura guidata in modo che utilizzi il database Northwind e scegliere di specificare un'istruzione SQL personalizzata. Poiché il controllo DetailsView verrà utilizzato per aggiungere elementi al database, è necessario specificare sia un `SELECT` istruzione e un `INSERT` istruzione. Utilizzare la seguente `SELECT` query:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample4.sql)]

Dalla scheda Inserisci, aggiungere il seguente `INSERT` istruzione:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample5.sql)]

Dopo aver completato la procedura guidata, passare a smart tag del controllo DetailsView e selezionare la casella di controllo "Abilita inserimento". Questo comando aggiunge un CommandField al controllo DetailsView con relativo `ShowInsertButton` proprietà è impostata su true. Poiché questo controllo DetailsView verrà utilizzato esclusivamente per l'inserimento di dati, impostare di DetailsView `DefaultMode` proprietà `Insert`.

Questo è tutto qui! Proviamo questa pagina. Visitare `AddProduct.aspx` tramite un browser, immettere un nome e il prezzo (vedere Figura 6).


[![Aggiungere un nuovo prodotto al Database](interacting-with-the-master-page-from-the-content-page-cs/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image16.png)

**Figura 06**: aggiungere un nuovo prodotto al Database ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-master-page-from-the-content-page-cs/_static/image18.png))


Dopo aver digitato il nome e il prezzo del prodotto di nuovo, fare clic sul pulsante Inserisci. In questo modo il form per i postback. Durante del postback, il controllo SqlDataSource `INSERT` istruzione; i due parametri vengono popolati con i valori immessi dall'utente in due controlli di casella di testo di DetailsView. Sfortunatamente, non vi è alcun feedback visivo che si è verificato un inserimento. Sarebbe utile disporre di un messaggio visualizzato per confermare che sia stato aggiunto un nuovo record. Lasciare questo campo come esercizio per il lettore. Inoltre, dopo aver aggiunto un nuovo record dal controllo DetailsView GridView nella pagina master ancora Mostra gli stessi cinque record come prima; non include il record appena aggiunto. Come risolvere questo problema nei prossimi passaggi verranno presi in esame.

> [!NOTE]
> Oltre ad aggiungere una forma di feedback visivo che l'inserimento ha avuto esito positivo, è consigliabile aggiornare anche l'interfaccia di inserimento del controllo DetailsView per includere la convalida. Attualmente, non vengono convalidati. Se un utente immette un valore valido per il `UnitPrice` campo, ad esempio "troppo costoso," verrà generata un'eccezione durante il postback quando il sistema tenta di convertire la stringa in un numero decimale. Per ulteriori informazioni sulla personalizzazione di inserimento dell'interfaccia, vedere il [ *personalizzazione dell'interfaccia di modifica dati* esercitazione](../../data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) da my [utilizzo di una serie di esercitazioni dati](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Passaggio 3: Creazione di proprietà e metodi pubblici della pagina Master

Nel passaggio 1 è stato aggiunto un controllo etichetta Web denominato `GridMessage` sopra GridView nella pagina master. Questa etichetta deve facoltativamente viene visualizzato un messaggio. Ad esempio, dopo l'aggiunta di un nuovo record per il `Products` tabella, potrebbe essere opportuno visualizzare il messaggio: "*ProductName* è stato aggiunto al database." Anziché a livello di codice il testo per l'etichetta nella pagina master, potrebbe essere opportuno personalizzabile per la pagina di contenuto del messaggio.

Poiché il controllo etichetta viene implementato come una variabile membro protetto all'interno della pagina master che non è possibile accedervi direttamente dalle pagine di contenuto. Per poter funzionare con l'etichetta all'interno di una pagina master dalla pagina contenuto (o, d'altra parte, qualsiasi controllo Web nella pagina master) è necessario creare una proprietà pubblica nella pagina master che espone il controllo Web o funge da proxy per cui una delle relative proprietà può essere  accedere al. Aggiungere la sintassi seguente alla classe code-behind della pagina master per esporre l'etichetta `Text` proprietà:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample6.cs)]

Quando viene aggiunto un nuovo record per il `Products` tabella da una pagina contenuto la `RecentProducts` GridView nella pagina master è necessario associare nuovamente all'origine dati sottostante. Per riassociare la chiamata di GridView relativo `DataBind` metodo. Poiché GridView nella pagina master non è accessibile a livello di programmazione per le pagine di contenuto, è necessario creare un metodo pubblico nella pagina master che, quando viene chiamato, nuovamente l'associazione dati a GridView. Aggiungere il metodo seguente alla classe code-behind della pagina master:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample7.cs)]

Con il `GridMessageText` proprietà e `RefreshRecentProductsGrid` metodo sul posto, qualsiasi pagina di contenuto a livello di codice impostare o leggere il valore della `GridMessage` dell'etichetta `Text` proprietà o la riassociazione dei dati per il `RecentProducts` GridView. Passaggio 4 viene illustrato come accedere alla pagina master proprietà e metodi pubblici da una pagina contenuta.

> [!NOTE]
> Non dimenticare di contrassegnare le proprietà e metodi come la pagina master `public`. Se si non indicare in modo esplicito queste proprietà e metodi come `public`, non sarà accessibile dalla pagina contenuto.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Passaggio 4: Chiamata di membri pubblici della pagina Master da una pagina contenuto

Ora che la pagina master include le proprietà pubbliche necessarie e i metodi, si è pronti per richiamare queste proprietà e metodi di `AddProduct.aspx` pagina contenuto. In particolare, è necessario impostare la pagina master `GridMessageText` proprietà e chiamare il relativo `RefreshRecentProductsGrid` metodo dopo il nuovo prodotto è stato aggiunto al database. Tutti i controlli Web dati ASP.NET generano eventi immediatamente prima e dopo il completamento delle varie attività, che rendono più facile per gli sviluppatori di pagine intraprendere le azioni a livello di codice prima o dopo l'attività. Ad esempio, quando l'utente finale fa clic sul pulsante Inserisci di DetailsView, nel postback genera DetailsView relativo `ItemInserting` evento prima di iniziare l'inserimento del flusso di lavoro. Quindi inserisce il record nel database. Successivamente, genera DetailsView relativo `ItemInserted` evento. Pertanto, per poter funzionare con la pagina master dopo aver aggiunto il nuovo prodotto, creare un gestore eventi in DetailsView `ItemInserted` evento.

Esistono due modi che una pagina di contenuto a livello di codice può interfacciarsi con pagina master:

- Utilizzo di `Page.Master` proprietà, che restituisce un riferimento alla pagina master fortemente tipizzato, o
- Specificare il percorso della pagina pagina master tipo o di file tramite un `@MasterType` direttiva; una proprietà fortemente tipizzate viene aggiunta automaticamente alla pagina denominata `Master`.

Esaminiamo entrambi gli approcci.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Utilizzando il fortemente tipizzato`Page.Master`proprietà

Tutte le pagine web ASP.NET deve derivare dal `Page` (classe), che si trova nel `System.Web.UI` dello spazio dei nomi. Il `Page` classe include un [ `Master` proprietà](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) che restituisce un riferimento alla pagina principale della pagina. Se la pagina non dispone di una pagina master `Master` restituisce `null`.

Il `Master` proprietà restituisce un oggetto di tipo [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (situato nel `System.Web.UI` dello spazio dei nomi) che è il tipo di base da cui derivare tutte le pagine master. Pertanto, per usare proprietà o metodi pubblici definiti nella pagina master del sito è necessario eseguire il cast di `MasterPage` oggetto restituito dal `Master` proprietà nel tipo appropriato. Poiché il file pagina master è denominata `Site.master`, la classe code-behind è denominata `Site`. Pertanto, nell'esempio di codice i cast di `Page.Master` proprietà a un'istanza della classe del sito.


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample8.cs)]

Ora che abbiamo eseguito il cast di fortemente tipizzato `Page.Master` proprietà per il `Site` tipo si può fare riferimento le proprietà e metodi specifici del sito. Come illustrato nella figura 7, la proprietà pubblica `GridMessageText` viene visualizzato nell'elenco a discesa IntelliSense.


[![In IntelliSense vengono mostrate la pagina Master proprietà e metodi pubblici](interacting-with-the-master-page-from-the-content-page-cs/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image19.png)

**Figura 07**: in IntelliSense vengono mostrate le proprietà pubbliche e i metodi di nostra pagina Master ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-master-page-from-the-content-page-cs/_static/image21.png))


> [!NOTE]
> Se il file pagina master denominata `MasterPage.master` nome classe di codice della pagina master è `MasterPage`. Questo può causare ambiguo codice quando si esegue il cast dal tipo `System.Web.UI.MasterPage` per la `MasterPage` classe. In breve, è necessario qualificare completamente il tipo a cui che si esegue il cast, che può essere un po' complesso quando si utilizza il modello di progetto di sito Web. Il suggerimento, è possibile assicurarsi che quando si crea la pagina master denominarlo diverso che `MasterPage.master` o, ancora meglio, creare un riferimento fortemente tipizzata della pagina master.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Creazione di un riferimento fortemente tipizzato con il`@MasterType`(direttiva)

Se si osserva attentamente, si noterà che classe code-behind della pagina ASP.NET è una classe parziale (si noti il `partial` parola chiave nella definizione di classe). Classi parziali sono stati introdotti in c# e Visual Basic con di.NET Framework 2.0 e, in pratica, consentono di membri di una classe da definire per più file. Il file di classe code-behind - `AddProduct.aspx.cs`, ad esempio, contiene il codice, lo sviluppatore della pagina, creata. Oltre a questo codice, il motore ASP.NET crea automaticamente un file di classe separata con le proprietà e gestori eventi in cui convertire il markup dichiarativo nella gerarchia delle classi della pagina.

La generazione automatica di codice che si verifica ogni volta che viene visitata una pagina ASP.NET apre la strada a alcune possibilità piuttosto interessanti e utili. Nel caso di pagine master, se si indicano al motore di ASP.NET, quali pagina master viene utilizzata per la pagina contenuta viene generato un fortemente tipizzato `Master` proprietà riservata.

Utilizzare il [ `@MasterType` direttiva](https://msdn.microsoft.com/library/ms228274.aspx) per comunicare al motore ASP.NET del tipo di contenuto della pagina pagina master. Il `@MasterType` direttiva può accettare il nome di tipo della pagina master o il percorso del file. Per specificare che il `AddProduct.aspx` pagina utilizza `Site.master` come pagina master, aggiungere la seguente direttiva all'inizio del `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample9.aspx)]

Questa direttiva indica al motore ASP.NET per aggiungere un riferimento fortemente tipizzata per la pagina master tramite una proprietà denominata `Master`. Con il `@MasterType` direttiva sul posto, è possibile chiamare il `Site.master` generale della pagina proprietà e metodi pubblici direttamente tramiti il `Master` proprietà senza cast.

> [!NOTE]
> Se si omette il `@MasterType` direttiva, la sintassi `Page.Master` e `Master` restituiscono la stessa cosa: un oggetto fortemente tipizzato alla pagina principale della pagina. Se si include il `@MasterType` direttiva quindi `Master` restituisce un riferimento fortemente tipizzata per la pagina master specificata. `Page.Master`, tuttavia, ancora restituisce un riferimento non fortemente tipizzato. Per esaminare più completa perché questo è il caso e come `Master` proprietà viene creata quando il `@MasterType` direttiva è inclusa, vedere [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)del post di blog [ `@MasterType` in ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Aggiornamento della pagina Master dopo l'aggiunta di un nuovo prodotto

Ora che si sa come richiamare i metodi da una pagina contenuto e le proprietà pubbliche di una pagina master, si è pronti per aggiornare il `AddProduct.aspx` pagina in modo che la pagina master viene aggiornata dopo l'aggiunta di un nuovo prodotto. All'inizio del passaggio 4 è stato creato un gestore eventi per il controllo DetailsView `ItemInserting` evento, che viene eseguito immediatamente dopo il nuovo prodotto è stato aggiunto al database. Aggiungere il codice seguente al gestore eventi:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample10.cs)]

Il codice precedente utilizza entrambi i fortemente tipizzato `Page.Master` proprietà e fortemente tipizzata `Master` proprietà. Si noti che il `GridMessageText` è impostata su "*ProductName* aggiunte alla griglia..." I valori del prodotto appena aggiunte sono accessibili tramite il `e.Values` insieme, come si può notare, l'appena aggiunti `ProductName` valore sono accessibili tramite `e.Values["ProductName"]`.

Figura 8 viene illustrata la `AddProduct.aspx` pagina immediatamente dopo un nuovo prodotto - Scott Soda - è stato aggiunto al database. Si noti che il nome di prodotto appena aggiunto è indicato nell'etichetta della pagina master e che il controllo GridView è stato aggiornato per includere il prodotto e prezzo corrispondente.


[![Etichetta della pagina Master e GridView mostrano il prodotto Just-aggiunta](interacting-with-the-master-page-from-the-content-page-cs/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image22.png)

**Figura 08**: etichetta della pagina Master e GridView mostrano il prodotto Just-Added ([fare clic per visualizzare l'immagine ingrandita](interacting-with-the-master-page-from-the-content-page-cs/_static/image24.png))


## <a name="summary"></a>Riepilogo

Idealmente, una pagina master e le pagine di contenuto sono completamente distinti, uno da altro e nessun livello di interazione. Mentre le pagine master e contenuto devono essere progettate tenendo presente questo obiettivo, esistono alcuni scenari comuni in cui una pagina di contenuto deve interfacciarsi con pagina master. Uno dei motivi più comuni coinvolge l'aggiornamento di una parte specifica di visualizzazione della pagina master in base a un'azione che si sono verificati nella pagina di contenuto.

Buone notizie sono che è relativamente semplice di una pagina di contenuto a livello di codice tenendo conto della pagina master. Iniziare creando proprietà o metodi pubblici della pagina master che incapsulano la funzionalità che deve essere richiamato da una pagina di contenuto. Quindi, nella pagina di contenuto, accedere a proprietà e metodi della pagina master tramite il fortemente tipizzato `Page.Master` oppure utilizzare il `@MasterType` direttiva per creare un riferimento fortemente tipizzata della pagina master.

Nella prossima esercitazione verranno esaminati come disporre di interagire a livello di codice con una delle pagine del contenuto della pagina master.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [L'accesso e l'aggiornamento dei dati in ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Pagine Master ASP.NET: Suggerimenti, consigli e trap](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` in ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Il passaggio di informazioni tra il contenuto e pagine Master](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Utilizzo di dati nelle esercitazioni ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft fin dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 3.5 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott può essere raggiunto al [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Zack Jones. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](control-id-naming-in-content-pages-cs.md)
> [Successivo](interacting-with-the-content-page-from-the-master-page-cs.md)
