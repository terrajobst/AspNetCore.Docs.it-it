---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Interfaccia utente e la navigazione | Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET tramite ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per abbiamo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: 7f1d8a1a473820a7c8da4c8086904cc41c86fd2a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="ui-and-navigation"></a>Interfaccia utente e la navigazione
====================
Da [Erik Reitan](https://github.com/Erikre)

[Scarica progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con il codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento questa serie di esercitazioni è disponibile.


In questa esercitazione si modificherà l'interfaccia utente dell'applicazione Web predefinita per supportare le funzionalità dell'applicazione front-archivio Wingtip Toys. Inoltre, si aggiungeranno semplice e navigazione con associazione a dati. In questa esercitazione si basa sull'esercitazione precedente "Creare il livello di accesso ai dati" e fa parte della serie di esercitazioni Wingtip Toys.

## <a name="what-youll-learn"></a>Illustra quanto segue:

- Come modificare l'interfaccia utente per supportare le funzionalità dell'applicazione front-archivio Wingtip Toys.
- Come configurare un elemento HTML5 per includere la navigazione.
- Come creare un controllo basato sui dati a cui passare i dati di prodotto specifico.
- Come visualizzare i dati da un database creato utilizzando Code First di Entity Framework.

Web Form ASP.NET consente di creare contenuto dinamico per l'applicazione Web. Ogni pagina Web ASP.NET viene creato in modo analogo a una pagina HTML Web statica (una pagina che non include l'elaborazione basata su server), ma la pagina Web ASP.NET include elementi aggiuntivi che ASP.NET riconosce ed elabora per generare HTML quando viene eseguita la pagina.

Con una pagina HTML statica (*htm* o *HTML* file), il server soddisfa un `Web` richiesta per la lettura del file e inviarlo come-al browser. Al contrario, quando un utente richiede una pagina Web ASP.NET (*aspx* file), la pagina viene eseguito come un programma nel server Web. Durante l'esecuzione della pagina, è possibile eseguire qualsiasi attività che richiede il sito Web, inclusi il calcolo dei valori, di lettura o scrittura delle informazioni di database o la chiamata di altri programmi. Come output, in modo dinamico la pagina genera markup (ad esempio gli elementi in formato HTML) e invia l'output dinamico nel browser.

## <a name="modifying-the-ui"></a>Modifica l'interfaccia utente

Si continuerà a questa serie di esercitazioni modificando il *Default.aspx* pagina. Si modificherà l'interfaccia utente già definiti nel modello predefinito utilizzato per creare l'applicazione. I tipi di modifiche da eseguire sono tipiche durante la creazione di qualsiasi applicazione Web Form. È possibile farlo cambiando il titolo, parte del contenuto di sostituzione e rimozione di contenuto predefinito non necessari.

1. Aprire o passare il *Default.aspx* pagina.
2. Se viene visualizzata la pagina **progettazione** passare alla **origine** visualizzazione.
3. Nella parte superiore della pagina di `@Page` direttiva, modifica il `Title` attributo "Iniziale", come evidenziato in giallo sotto. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Anche nel *Default.aspx* pagina, sostituire tutto il contenuto predefinito contenuto nel `<asp:Content>` tag in modo che il markup viene visualizzato come sotto. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Salvare il *Default.aspx* selezionando **salvare Default.aspx** dal **File** menu.

 Il valore risultante *Default.aspx* pagina viene visualizzata come indicato di seguito: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

Nell'esempio, impostare il `Title` attributo del `@Page` direttiva. Quando il codice HTML viene visualizzato in un browser, il codice server `<%: Page.Title %>` risolve il contenuto di `Title` attributo.

La pagina di esempio include gli elementi di base che costituiscono una pagina Web ASP.NET. La pagina contiene testo statico, potrebbe essere in una pagina HTML, insieme a elementi specifici di ASP.NET. Il contenuto di *Default.aspx* pagina verrà integrata con il contenuto della pagina master, che verrà spiegato più avanti in questa esercitazione.

### <a name="page-directive"></a>@Page(Direttiva)

Web Form ASP.NET è in genere contengono direttive che consentono di specificare informazioni della pagina proprietà e la configurazione per la pagina. Le direttive sono utilizzate da ASP.NET come istruzioni sull'elaborazione della pagina, ma non vengono sottoposte a rendering come parte del codice che viene inviato al browser.

La direttiva utilizzata più di frequente è il `@Page` direttiva, che consente di specificare numerose opzioni di configurazione per la pagina, inclusi i seguenti:

1. Il server di linguaggio di programmazione per il codice nella pagina, ad esempio c#.
2. Se la pagina è una pagina con il codice server direttamente nella pagina, che viene chiamata una pagina a file singolo, o se è una pagina con il codice in un file di classe separata, che viene chiamato una pagina code-behind.
3. Se la pagina associata a una pagina master associata e pertanto deve essere considerata come una pagina di contenuto.
4. Il debug e le opzioni di traccia.

Se non si include un `@Page` direttiva nella pagina o se la direttiva non include un'impostazione specifica, verrà ereditata da un'impostazione di *Web. config* file di configurazione o dal *Machine. config* file di configurazione. Il *Machine. config* file fornisce ulteriori impostazioni di configurazione a tutte le applicazioni in esecuzione in un computer.

> [!NOTE] 
> 
> Il *Machine. config* fornisce anche informazioni dettagliate su tutte le impostazioni di configurazione.


### <a name="web-server-controls"></a>Controlli Server Web

Nella maggior parte delle applicazioni Web Form ASP.NET, si aggiungerà i controlli che consentono all'utente di interagire con la pagina, ad esempio pulsanti, caselle di testo, elenchi e così via. I controlli server Web sono simili ai pulsanti HTML e gli elementi di input. Tuttavia, vengono elaborate sul server, consentendo di utilizzare il codice lato server per impostare le relative proprietà. Questi controlli inoltre generano eventi che è possibile gestire nel codice server.

I controlli server utilizzano una sintassi speciale che ASP.NET riconosce durante l'esecuzione della pagina. Il nome di tag per i controlli server ASP.NET inizia con un `asp:` prefisso. Ciò consente di riconoscere ed elaborare questi controlli server ASP.NET. Il prefisso può essere diverso se il controllo non fa parte di .NET Framework. Oltre al `asp:` prefisso, controlli server ASP.NET includono inoltre il `runat="server"` attributo e un `ID` che è possibile utilizzare per fare riferimento al controllo nel codice server.

Quando la pagina viene eseguita, ASP.NET identifica i controlli server e viene eseguito il codice che è associato a tali controlli. Molti controlli eseguire il rendering di HTML o altro markup nella pagina quando viene visualizzato in un browser.

### <a name="server-code"></a>Codice lato server

La maggior parte delle applicazioni Web Form ASP.NET includono codice che viene eseguito nel server durante l'elaborazione della pagina. Come indicato in precedenza, è possibile utilizzare il codice lato server per svolgere un'ampia gamma di elementi, ad esempio l'aggiunta di dati a un controllo ListView. ASP.NET supporta molte lingue per l'esecuzione nel server, inclusi c#, Visual Basic, j# e altri utenti.

ASP.NET supporta due modelli per la scrittura di codice server per una pagina Web. Nel modello di file singolo, il codice per la pagina è in un elemento di script in cui sono inclusi il tag di apertura di `runat="server"` attributo. In alternativa, è possibile creare il codice per la pagina in un file di classe separata, che viene definito il modello code-behind. In questo caso, la pagina Web Form ASP.NET non contiene in genere alcun codice server. Al contrario, il `@Page` direttiva include informazioni che si collega il *aspx* pagina con il file code-behind associato.

Il `CodeBehind` contenuto nell'attributo la `@Page` direttiva specifica il nome del file di classi separato e `Inherits` attributo specifica il nome della classe all'interno del file di codice che corrisponde alla pagina.

### <a name="updating-the-master-page"></a>Aggiornamento della pagina Master

Web Form ASP.NET, pagine master consentono di creare un layout coerente per le pagine dell'applicazione. Una singola pagina master definisce l'aspetto e il comportamento standard che desidera che per tutte le pagine (o un gruppo di pagine) nell'applicazione. È quindi possibile creare singole pagine di contenuto che includono il contenuto che si desidera visualizzare, come descritto in precedenza. Quando gli utenti richiedono le pagine di contenuto, ASP.NET unisce con la pagina master per produrre l'output che combina il layout della pagina master con il contenuto della pagina di contenuto.

Il nuovo sito è necessario un singolo logo da visualizzare in ogni pagina. Per aggiungere il logo, è possibile modificare il codice HTML della pagina master.

1. In **Esplora**, trovare e aprire il **Site. master** pagina.
2. Se la pagina è **progettazione** passare alla **origine** visualizzazione.
3. Aggiornare la pagina master da **modificando o aggiungendo** il markup evidenziato in giallo: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Questo codice HTML verrà visualizzata l'immagine denominata *logo* dal *immagini* cartella dell'applicazione Web, che verrà aggiunto in un secondo momento. Quando viene visualizzata una pagina che utilizza la pagina master in un browser, verrà visualizzato il logo. Se un utente fa clic sul logo, l'utente verrà visualizzata nuovamente la *Default.aspx* pagina. Il tag di ancoraggio HTML `<a>` include il controllo server di immagine e consente l'immagine deve essere incluso come parte del collegamento. Il `href` per il tag di ancoraggio specifica la radice dell'attributo "`~/`" del sito Web come percorso di collegamento. Per impostazione predefinita, il *Default.aspx* pagina viene visualizzata quando l'utente passa alla radice del sito Web. Il **immagine** `<asp:Image>` controllo server include le proprietà di aggiunta, ad esempio `BorderStyle`, che eseguono il rendering in formato HTML quando visualizzata in un browser.

### <a name="master-pages"></a>Pagine master

Una pagina master è un file con estensione master di ASP.NET (ad esempio, *Site. master*) con un layout predefinito che può includere testo statico, gli elementi HTML e controlli server. La pagina master è identificata da una speciale `@Master` che sostituisce il `@Page` istruzione utilizzata per ordinario *aspx* pagine.

Oltre al `@Master` direttiva, la pagina master contiene anche tutti gli elementi HTML di livello superiore per una pagina, ad esempio `html`, `head`, e `form`. Ad esempio, nella pagina master è stato aggiunto in precedenza, utilizzare un elemento HTML `table` per il layout, un `img` elemento per il logo della società, testo statico e controlli di server per gestire l'appartenenza comune per il sito. È possibile utilizzare qualsiasi codice HTML e ASP.NET come parte della pagina master.

Oltre ai controlli che verranno visualizzato in tutte le pagine e testo statico, la pagina master include anche uno o più **ContentPlaceHolder** controlli. Questi controlli segnaposto definiscono le aree in cui verrà visualizzato il contenuto sostituibile. A sua volta, il contenuto sostituibile è definito nelle pagine di contenuto, ad esempio *Default.aspx*, usando il **contenuto** controllo server.

#### <a name="adding-image-files"></a>Aggiunta di file di immagine

L'immagine del logo che viene fatto riferimento in precedenza, insieme a tutte le immagini di prodotto, deve essere aggiunto all'applicazione Web in modo che possano essere visualizzati quando il progetto viene visualizzato in un browser.

#### <a name="download-from-msdn-samples-site"></a>Scaricare dal sito di esempi MSDN:

[Guida introduttiva a 4.5 Web Form ASP.NET e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

Il download include le risorse di *WingtipToys asset* cartella in cui vengono utilizzati per creare l'applicazione di esempio.

1. Se non già stato fatto, scaricare i file di esempio compressi utilizzando il collegamento precedente dal sito di esempi MSDN.
2. Una volta scaricata, aprire il file con estensione zip e copiare il contenuto in una cartella locale nel computer.
3. Trovare e aprire il *WingtipToys asset* cartella.
4. Trascinando e rilasciando, copiare il *catalogo* cartella dalla cartella locale nella radice del progetto di applicazione Web nel **Esplora** di Visual Studio. 

    ![Interfaccia utente e la navigazione - copia di file](ui_and_navigation/_static/image1.png)
5. Successivamente, creare una nuova cartella denominata *immagini* facendo clic con il **WingtipToys** nel progetto **Esplora** e selezionando **Aggiungi**  - &gt; **Nuova cartella**.
6. Copia il *logo* file dal *WingtipToys asset* cartella **Esplora File** per il *immagini* cartella dell'applicazione Web nel progetto **Esplora** di Visual Studio.
7. Fare clic su di **Mostra tutti i file** opzione nella parte superiore di **Esplora** per aggiornare l'elenco dei file se non viene visualizzato i nuovo file.  
  
    **Esplora soluzioni** ora vengono mostrati i file di progetto aggiornato. 

    ![Interfaccia utente e la navigazione - Esplora soluzioni](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Aggiunta di pagine

Prima di aggiungere navigazione per l'applicazione Web, aggiungerai due nuove pagine che verrà visualizzata. Più avanti in questa serie di esercitazioni, si visualizzeranno i prodotti e i relativi dettagli su queste nuove pagine.

1. In **Esplora**, fare doppio clic su **WingtipToys**, fare clic su **Aggiungi**, quindi fare clic su **nuovo elemento**.   
 Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Selezionare quindi **Web Form con pagina Master** dal centro elenco e denominarlo *ProductList.aspx*. 

    ![Interfaccia utente e la navigazione - Aggiungi finestra di dialogo Nuovo elemento](ui_and_navigation/_static/image3.png)
3. Selezionare **Site. master** per collegare la pagina master per l'oggetto appena creato *aspx* pagina. 

    ![Interfaccia utente e la navigazione - Seleziona pagina Master](ui_and_navigation/_static/image4.png)
4. Aggiungere una pagina aggiuntiva denominata *ProductDetails.aspx* seguendo la stessa procedura.

### <a name="updating-bootstrap"></a>L'aggiornamento di Bootstrap

Utilizzano i modelli di progetto di Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), un framework di layout e i temi creato da Twitter. Bootstrap utilizza CSS3 per fornire una progettazione reattiva, ovvero layout può adattarsi dinamicamente alle dimensioni della finestra del browser diverse. Inoltre, è possibile utilizzare funzionalità dei temi del Bootstrap facilmente attuare una modifica nell'aspetto dell'applicazione. Per impostazione predefinita, il modello di applicazione Web ASP.NET in Visual Studio 2013 include Bootstrap come pacchetto NuGet.

In questa esercitazione si modificherà aspetto dell'applicazione Wingtip Toys sostituendo i file CSS di Bootstrap.

1. In **Esplora**, aprire il *contenuto* cartella.
2. Fare doppio clic su di *bootstrap.css* file e rinominarlo *bootstrap original.css*.
3. Rinominare il *bootstrap.min.css* a *bootstrap original.min.css*.
4. In **Esplora**, fare doppio clic su di *contenuto* cartella e selezionare **Apri cartella in Esplora File**.  
 Verrà visualizzato Esplora File. Si salverà un file CSS bootstrap scaricati in questa posizione.
5. Nel browser, passare a [http://Bootswatch.com](http://bootswatch.com/).
6. Scorrere la finestra del browser finché non viene visualizzato il tema Cerulean. 

    ![Interfaccia utente e la navigazione - Cerulean tema](ui_and_navigation/_static/image5.png)
7. Scaricare entrambi il *bootstrap.css* file e *bootstrap.min.css* file per il *contenuto* cartella. Utilizzare il percorso per la cartella del contenuto che viene visualizzata nel **Esplora File** finestra aperta in precedenza.
8. In **Visual Studio** nella parte superiore di **Esplora**, selezionare il **Mostra tutti i file** opzione per visualizzare i nuovi file nella cartella del contenuto. 

    ![Interfaccia utente e la navigazione - Esplora soluzioni](ui_and_navigation/_static/image6.png)

 Si noterà che i due nuovi file CSS nel **contenuto** cartella, ma si noti che l'icona accanto a ogni nome di file non è disponibile. Ciò significa che il file non è ancora stato aggiunto al progetto.
9. Fare doppio clic su di *bootstrap.css* e *bootstrap.min.css* file e selezionare **Includi nel progetto**.   
 Quando si esegue l'applicazione Wingtip Toys più avanti in questa esercitazione, verrà visualizzata la nuova interfaccia utente.

> [!NOTE] 
> 
> Il modello di applicazione Web ASP.NET utilizza il *Bundle.config* file alla radice del progetto per archiviare il percorso dei file CSS di Bootstrap.


### <a name="modifying-the-default-navigation"></a>Modifica di navigazione predefinito

La navigazione predefinito per ogni pagina nell'applicazione può essere modificata da modificare l'elemento di elenco non ordinato di navigazione nel *Site. master* pagina.

1. In **Esplora**, individuare e aprire il *Site. master* pagina.
2. Aggiungere il collegamento di navigazione aggiuntivi evidenziato in giallo per l'elenco non ordinato illustrato di seguito:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Come si può notare nel codice HTML sopra, ogni voce è stato modificato `<li>` contenente un tag di ancoraggio `<a>` con un collegamento `href` attributo. Ogni `href` punta a una pagina nell'applicazione Web. Nel browser, quando un utente fa clic su uno di questi collegamenti (ad esempio **prodotti**), verrà visualizzata la pagina contenuta nel `href` (ad esempio **ProductList.aspx**). Alla fine di questa esercitazione verrà eseguita l'applicazione.

> [!NOTE] 
> 
> La tilde (`~`) carattere viene utilizzato per specificare che il `href` percorso inizia alla radice del progetto.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Aggiunta di un controllo di dati per visualizzare i dati di navigazione

Successivamente, aggiungere un controllo per visualizzare tutte le categorie dal database. Ogni categoria fungerà da un collegamento per il *ProductList.aspx* pagina. Quando un utente fa clic su un collegamento categoria nel browser, essi verranno passare alla pagina dei prodotti e vedere solo i prodotti associati alla categoria selezionata.

Si userà un **ListView** controllo per visualizzare tutte le categorie contenute nel database. Per aggiungere un **ListView** controllo della pagina master:

1. Nel *Site. master* pagina, aggiungere le seguenti evidenziate `<div>` elemento **dopo** il `<div>` elemento che contiene il `id="TitleContent"` aggiunta in precedenza:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Questo codice visualizza tutte le categorie dal database. Il **ListView** controllo Visualizza il nome di ogni categoria come testo del collegamento e include un collegamento per il *ProductList* pagina con un valore di stringa di query contenente il `ID` della categoria. Impostando il `ItemType` proprietà nel **ListView** controllo, espressione di associazione dati `Item` è disponibile all'interno di `ItemTemplate` nodo e il controllo diventa fortemente tipizzati. È possibile selezionare i dettagli del `Item` oggetto usando IntelliSense, ad esempio specificando il `CategoryName`. Questo codice è contenuto all'interno del contenitore `<%#: %>` che contrassegna un'espressione di associazione dati. Aggiungendo (:) alla fine del `<%#` prefisso, il risultato dell'espressione di associazione di dati è codificata in formato HTML. Quando il risultato è codificata in formato HTML, l'applicazione migliore protezione contro intersito script injection (XSS) e attacchi intrusivi nel codice HTML.

> [!NOTE] 
> 
> **Suggerimento**
> 
> Quando si aggiunge codice digitando durante lo sviluppo, si può verificare che un membro valido di un oggetto viene trovato poiché fortemente tipizzato controlli dati mostrano i membri disponibili in base a IntelliSense. IntelliSense consente di scegliere di codice appropriato al contesto durante la digitazione di codice, ad esempio proprietà, metodi e oggetti.


Nel passaggio successivo, verrà implementata la `GetCategories` metodo per recuperare i dati.

### <a name="linking-the-data-control-to-the-database"></a>Collegamento del controllo dati al Database

Prima di poter visualizzare i dati nel controllo dei dati, è necessario collegare il controllo dei dati al database. Per rendere il collegamento, è possibile modificare il codice sottostante del *Site.Master.cs* file.

1. In **Esplora**, fare doppio clic su di *Site. master* pagina e quindi fare clic su **Visualizza codice**. Il *Site.Master.cs* file viene aperto nell'editor.
2. Inizio della parte di *Site.Master.cs* file, aggiungere due spazi dei nomi aggiuntivi in modo che gli spazi dei nomi inclusi come indicato:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Aggiungere evidenziata `GetCategories` metodo dopo il `Page_Load` gestore dell'evento come indicato di seguito:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Il codice precedente viene eseguito quando qualsiasi pagina che utilizza la pagina master viene caricata nel browser. Il `ListView` controllo (denominata "categoryList") che è stato aggiunto in precedenza in questa esercitazione Usa l'associazione di modelli per selezionare i dati. Nel markup del `ListView` controllo è impostare il controllo `SelectMethod` proprietà per il `GetCategories` metodo illustrato in precedenza. Il `ListView` chiamate di controllo di `GetCategories` metodo nel momento adeguato del ciclo di vita della pagina del ciclo e associa automaticamente i dati restituiti. Verranno fornite informazioni sull'associazione di dati nella prossima esercitazione.

### <a name="running-the-application-and-creating-the-database"></a>Esecuzione dell'applicazione e la creazione del Database

In precedenza in questa serie di esercitazioni creato un inizializzatore di classe (denominato "ProductDatabaseInitializer") e specificato a questa classe nella *global.asax.cs* file. Entity Framework genera il database quando l'applicazione viene eseguita la prima volta, poiché il `Application_Start` contenuta nel metodo il *global.asax.cs* file chiamerà la classe di inizializzatore. La classe di inizializzatore utilizzerà le classi di modello (`Category` e `Product`) aggiunto in precedenza in questa serie di esercitazioni per creare il database.

1. In **Esplora**, fare doppio clic su di *Default.aspx* pagina e selezionare **imposta come pagina iniziale**.
2. Stampa di Visual Studio **F5**.   
 Richiederà un po' di tempo per impostare tutti gli elementi durante la prima esecuzione.   
    ![Interfaccia utente e la navigazione - finestre del Browser](ui_and_navigation/_static/image7.png)  
 Quando si esegue l'applicazione, verrà compilata l'applicazione e il database denominato *wingtiptoys.mdf* verrà creato nel *App\_dati* cartella. Nel browser, verrà visualizzato un menu di navigazione categoria. Questo menu è stato generato mediante il recupero delle categorie dal database. Nella prossima esercitazione, verrà implementata la navigazione.
3. Chiudere il browser per arrestare l'applicazione in esecuzione.

### <a name="reviewing-the-database"></a>Verifica del Database

Aprire il *Web. config* file ed esaminare la sezione della stringa di connessione. È possibile vedere che il `AttachDbFilename` punta al valore nella stringa di connessione di `DataDirectory` per il progetto di applicazione Web. Il valore `|DataDirectory|` è un valore riservato che rappresenta il *App\_dati* cartella nel progetto. Questa cartella è in cui si trova il database creato da classi di entità.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Se il *App\_dati* cartella non è visibile o se la cartella è vuota, selezionare il **aggiornamento** icona e quindi la **Mostra tutti i file** in alto del **Esplora** finestra. Espandere la larghezza del **Esplora** windows potrebbe essere necessario visualizzare tutte le icone disponibili.


Ora è possibile controllare i dati contenuti nel *wingtiptoys.mdf* file di database utilizzando il **Esplora Server** finestra.

1. Espandere il *App\_dati* cartella. Se il *App\_dati* cartella non è visibile, vedere la nota precedente.
2. Se il *wingtiptoys.mdf* file di database non è visibile, selezionare il **aggiornamento** icona e quindi la **Mostra tutti i file** in alto del **Esplora soluzioni**  finestra.
3. Fare doppio clic su di *wingtiptoys.mdf* file di database e selezionare **aprire**.  
    **Esplora server** viene visualizzato. 

    ![Interfaccia utente e la navigazione - Esplora Server](ui_and_navigation/_static/image8.png)
4. Espandere il *tabelle* cartella.
5. Fare doppio clic su di **prodotti**tabella e selezionare **Mostra dati tabella**.  
 Il **prodotti** tabella viene visualizzata. 

    ![Interfaccia utente e la navigazione - tabella di prodotti](ui_and_navigation/_static/image9.png)
6. Questa visualizzazione consente di visualizzare e modificare i dati di **prodotti** tabella manualmente.
7. Chiudi il **prodotti** finestra della tabella.
8. Nel **Esplora Server**, fare doppio clic su di **prodotti** tabella nuovo e selezionare **Apri definizione tabella**.  
 I dati di progettazione per il **prodotti** tabella viene visualizzata. 

    ![Interfaccia utente e la navigazione - progettazione di prodotti](ui_and_navigation/_static/image10.png)
9. Nel **T-SQL** scheda verrà visualizzato l'istruzione SQL DDL che è stato utilizzato per creare la tabella. È inoltre possibile utilizzare l'interfaccia utente di **progettazione** scheda per modificare lo schema.
10. Nel **Esplora Server**, fare doppio clic su **WingtipToys** del database e selezionare **chiusura connessione**.   
 Scollegare il database da Visual Studio, lo schema del database sarà in grado di modificare più avanti in questa serie di esercitazioni.
11. Tornare alla **Esplora**selezionando il **Esplora** scheda nella parte inferiore del **Esplora Server** finestra.

## <a name="summary"></a>Riepilogo

In questa esercitazione della serie è stato aggiunto alcuni interfaccia utente di base, grafica, pagine e navigazione. Inoltre, è stata eseguita l'applicazione Web, che il database creato dalle classi di dati che è stato aggiunto nell'esercitazione precedente. Inoltre visualizzare il contenuto del *prodotti* tabella del database visualizzando direttamente il database. Nella prossima esercitazione, si visualizzeranno gli elementi di dati e i dettagli del database.

## <a name="additional-resources"></a>Risorse aggiuntive

[Introduzione alla programmazione di pagine Web ASP.NET](https://msdn.microsoft.com/library/ms178125.aspx)   
[Cenni preliminari sui controlli Server Web ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Esercitazione CSS](http://www.w3schools.com/css/default.asp)

>[!div class="step-by-step"]
[Precedente](create_the_data_access_layer.md)
[Successivo](display_data_items_and_details.md)
