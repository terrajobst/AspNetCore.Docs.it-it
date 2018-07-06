---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Interfaccia utente ed esplorazione | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Microsoft...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: ef1b226d673d18ec333c35f193dbe6bd21e54225
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842915"
---
<a name="ui-and-navigation"></a>Interfaccia utente e la navigazione
====================
da [Erik Reitan](https://github.com/Erikre)

[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento a questa serie di esercitazioni è disponibile.


In questa esercitazione si modificherà l'interfaccia utente dell'applicazione Web predefinita per supportare le funzionalità dell'applicazione front-archivio di Wingtip Toys. Inoltre, si aggiungeranno semplice e navigazione con associazione a dati. Questa esercitazione si basa sull'esercitazione precedente "Creare il livello di accesso ai dati" e fa parte della serie di esercitazioni di Wingtip Toys.

## <a name="what-youll-learn"></a>Che cosa si apprenderà come:

- Come modificare l'interfaccia utente per supportare le funzionalità dell'applicazione front-archivio di Wingtip Toys.
- Come configurare un elemento HTML5 per includere la navigazione.
- Come creare un controllo basato sui dati a cui passare i dati di prodotto specifico.
- Come visualizzare i dati da un database creato utilizzando Code First di Entity Framework.

Web Form ASP.NET consentono di creare contenuto dinamico per l'applicazione Web. Ogni pagina Web ASP.NET viene creato in modo analogo a una pagina HTML Web statica (una pagina che non include l'elaborazione basata su server), ma la pagina Web ASP.NET include elementi aggiuntivi che ASP.NET riconosce ed elabora per generare HTML quando viene eseguita la pagina.

Con una pagina HTML statica (*htm* oppure *. HTML* file), il server soddisfa un `Web` richiesta per la lettura del file e inviarlo come-al browser. Al contrario, quando un utente richiede una pagina Web ASP.NET (*aspx* file), la pagina viene eseguito come un programma nel server Web. Durante l'esecuzione della pagina, è possibile eseguire qualsiasi attività che richiede il sito Web, tra cui il calcolo dei valori, la lettura o la scrittura delle informazioni di database o la chiamata di altri programmi. Come output, in modo dinamico la pagina genera markup (ad esempio, gli elementi in formato HTML) e invia l'output dinamico al browser.

## <a name="modifying-the-ui"></a>Modifica dell'interfaccia utente

Si continuerà a questa serie di esercitazioni modificando la *default. aspx* pagina. Si modificherà l'interfaccia utente che è già stata stabilita dal modello predefinito utilizzato per creare l'applicazione. Il tipo di modifiche che verranno eseguite sono tipiche durante la creazione di qualsiasi applicazione Web Form. Tale scopo, si sarà cambiando il titolo, parte del contenuto di sostituzione e rimozione di contenuto predefinito non necessari.

1. Apre o passa per la *default. aspx* pagina.
2. Se viene visualizzata la pagina **Design** passare alla **origine** visualizzazione.
3. Nella parte superiore della pagina nel `@Page` direttiva, modifica il `Title` dell'attributo "Attività iniziali", come evidenziato in giallo di seguito. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Nel *default. aspx* pagina, sostituire tutto il contenuto predefinito contenuto nel `<asp:Content>` contrassegnare in modo che il markup viene visualizzato come di seguito. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Salvare il *default. aspx* selezionando **salvare default. aspx** dal **File** menu.

   L'oggetto risultante *default. aspx* pagina viene visualizzata come indicato di seguito: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

Nell'esempio è stato impostato il `Title` attributo del `@Page` direttiva. Quando il codice HTML viene visualizzato in un browser, il codice del server `<%: Page.Title %>` risolve per il contenuto presente nel `Title` attributo.

La pagina di esempio include gli elementi di base che costituiscono una pagina Web ASP.NET. La pagina contiene testo statico, come si farebbe in una pagina HTML, insieme a elementi specifici di ASP.NET. Il contenuto presente nel *default. aspx* pagina verrà integrata con il contenuto della pagina master, che verrà spiegato più avanti in questa esercitazione.

### <a name="page-directive"></a>@Page (Direttiva)

Web Form ASP.NET è in genere contengono direttive che consentono di specificare informazioni sulla pagina di configurazione e proprietà per la pagina. Le direttive sono utilizzate da ASP.NET come istruzioni per l'elaborazione della pagina, ma non vengono sottoposte a rendering come parte del markup che viene inviato al browser.

La direttiva di uso più comune è la `@Page` direttiva, che consente di specificare numerose opzioni di configurazione per la pagina, inclusi i seguenti:

1. Il server di linguaggio di programmazione per il codice nella pagina, ad esempio c#.
2. Indica se la pagina è una pagina con il codice server direttamente nella pagina, che viene chiamato una pagina a file singolo, oppure se si tratta di una pagina con il codice in un file di classe separata, che viene chiamato una pagina code-behind.
3. Indica se la pagina è una pagina master associata e deve pertanto essere considerati come una pagina di contenuto.
4. Il debug e opzioni di traccia.

Se non si include un' `@Page` direttiva nella pagina o se la direttiva non include un'impostazione specifica, un'impostazione verrà ereditata dal *Web. config* file di configurazione o dal *Machine. config* file di configurazione. Il *Machine. config* file fornisce ulteriori impostazioni di configurazione a tutte le applicazioni in esecuzione in un computer.

> [!NOTE] 
> 
> Il *Machine. config* fornisce anche informazioni dettagliate su tutte le impostazioni di configurazione possibili.


### <a name="web-server-controls"></a>Controlli Server Web

Nella maggior parte delle applicazioni Web Form ASP.NET, si aggiungerà i controlli che consentono all'utente di interagire con la pagina, ad esempio pulsanti, caselle di testo, elenchi e così via. Questi controlli server Web sono simili ai pulsanti HTML e gli elementi di input. Tuttavia, questi vengono elaborati nel server, consentendo di usare il codice lato server per impostare le relative proprietà. Questi controlli inoltre generano eventi che è possibile gestire nel codice server.

Controlli server utilizzano una sintassi speciale che ASP.NET riconosce quando viene eseguita la pagina. Il nome del tag per controlli server ASP.NET inizia con un `asp:` prefisso. Ciò consente di riconoscere ed elaborare questi controlli server ASP.NET. Il prefisso potrebbe essere diverso se il controllo non fa parte di .NET Framework. Oltre al `asp:` prefisso, includano anche i controlli server ASP.NET il `runat="server"` attributo e un `ID` che è possibile usare il riferimento al controllo nel codice server.

Quando viene eseguita la pagina, ASP.NET identifica i controlli server e viene eseguito il codice che è associato a tali controlli. Molti controlli il rendering HTML o altri markup nella pagina quando viene visualizzato in un browser.

### <a name="server-code"></a>Codice lato server

La maggior parte delle applicazioni Web Form ASP.NET includono codice che viene eseguito nel server quando la pagina viene elaborata. Come indicato in precedenza, il codice lato server è utilizzabile per svolgere un'ampia gamma di elementi, ad esempio l'aggiunta di dati a un controllo ListView. ASP.NET supporta molte lingue per l'esecuzione nel server, tra cui c#, Visual Basic, j# e ad altri utenti.

ASP.NET supporta due modelli per la scrittura di codice lato server per una pagina Web. Nel modello di file singolo, il codice per la pagina è in un elemento di script in cui sono inclusi i tag di apertura di `runat="server"` attributo. In alternativa, è possibile creare il codice per la pagina in un file di classe separata, che viene considerato il modello code-behind. In questo caso, la pagina Web Form ASP.NET non contiene in genere alcun codice lato server. Al contrario, il `@Page` direttiva include informazioni che si collega il *aspx* pagina con il relativo file code-behind associato.

Il `CodeBehind` contenuto nell'attributo il `@Page` direttiva specifica il nome del file di classe separata e il `Inherits` attributo specifica il nome della classe all'interno del file code-behind che corrisponde alla pagina.

### <a name="updating-the-master-page"></a>L'aggiornamento della pagina Master

In Web Form ASP.NET, pagine master consentono di creare un layout coerente per le pagine nell'applicazione. Una singola pagina master definisce l'aspetto e il comportamento standard che desidera che per tutte le pagine (o un gruppo di pagine) nell'applicazione. È quindi possibile creare singole pagine di contenuto che contengono il contenuto che si desidera visualizzare, come descritto in precedenza. Quando gli utenti richiedono le pagine di contenuto, ASP.NET le unisce alla pagina master per generare l'output che combina il layout della pagina master con il contenuto della pagina di contenuto.

Il nuovo sito necessita un logo da visualizzare in ogni pagina singolo. Per aggiungere il logo, è possibile modificare il codice HTML della pagina master.

1. Nelle **Esplora soluzioni**individuare e aprire il **Site. master** pagina.
2. Se la pagina si trova in **Design** passare alla **origine** visualizzazione.
3. Aggiornare la pagina master dalla **modificando o aggiungendo** il markup evidenziato in giallo: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Questo codice HTML verrà visualizzata l'immagine denominata *logo* dalle *immagini* cartella dell'applicazione Web, che verrà aggiunto in un secondo momento. Quando viene visualizzata una pagina che utilizza la pagina master in un browser, verrà visualizzato il logo. Se un utente fa clic sul logo, l'utente passerà nuovamente al *default. aspx* pagina. Il tag di ancoraggio HTML `<a>` include il controllo server di immagine e consente l'immagine deve essere incluso come parte del collegamento. Il `href` per il tag di ancoraggio specifica la radice dell'attributo "`~/`" del sito Web come percorso di collegamento. Per impostazione predefinita, il *default. aspx* pagina viene visualizzata quando l'utente passa alla radice del sito Web. Il **immagine** `<asp:Image>` controllo del server include le proprietà di addizione, ad esempio `BorderStyle`, che eseguono il rendering come testo HTML se visualizzati in un browser.

### <a name="master-pages"></a>Pagine master

Una pagina master è un file con estensione master di ASP.NET (ad esempio, *Site. master*) con un layout predefinito che può includere testo statico, gli elementi HTML e controlli server. La pagina master è identificata da uno speciale `@Master` che sostituisce il `@Page` direttiva che viene utilizzata per normali *aspx* pagine.

Oltre al `@Master` direttiva della pagina master contiene anche tutti gli elementi HTML principali per una pagina, ad esempio `html`, `head`, e `form`. Ad esempio, nella pagina master aggiunto nel passaggio precedente, usare un elemento HTML `table` per il layout, un `img` (elemento) per il logo della società, testo statico e controlli server per gestire l'appartenenza comune per il sito. È possibile utilizzare qualsiasi codice HTML e gli eventuali elementi ASP.NET come parte della pagina master.

Oltre a testo statico e i controlli che verranno visualizzato in tutte le pagine, la pagina master include inoltre uno o più **ContentPlaceHolder** controlli. Questi controlli segnaposto definiscono le aree in cui verrà visualizzato il contenuto sostituibile. A sua volta, il contenuto sostituibile viene definito nelle pagine di contenuto, ad esempio *default. aspx*, usando la **contenuto** controllo server.

#### <a name="adding-image-files"></a>Aggiunta di file di immagine

L'immagine del logo che viene fatto riferimento in precedenza, insieme a tutte le immagini del prodotto, deve essere aggiunto all'applicazione Web in modo che possano essere visualizzati quando il progetto viene visualizzato in un browser.

#### <a name="download-from-msdn-samples-site"></a>Scaricare dal sito degli esempi di MSDN:

[Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

Il download include le risorse nel *WingtipToys-asset* cartella in cui vengono usati per creare l'applicazione di esempio.

1. Se non già stato fatto, scaricare i file di esempio compresso usando il collegamento sopra riportato dal sito di esempi MSDN.
2. Al termine del download, aprire il file con estensione zip e copiare il contenuto in una cartella locale nel computer.
3. Trovare e aprire il *WingtipToys-asset* cartella.
4. Trascinando e rilasciando, copiare il *Catalog* cartella dalla cartella locale nella radice del progetto di applicazione Web nelle **Esplora soluzioni** di Visual Studio. 

    ![Navigazione - copiare file e dell'interfaccia utente](ui_and_navigation/_static/image1.png)
5. Successivamente, creare una nuova cartella denominata *immagini* facendo clic con il **WingtipToys** progetto **Esplora soluzioni** e selezionando **Aggiungi**  - &gt; **Nuova cartella**.
6. Copia il *logo* del file dal *WingtipToys-asset* cartella nel **Esplora File** per il *immagini* cartella dell'applicazione Web progetto in **Esplora soluzioni** di Visual Studio.
7. Fare clic sui **Mostra tutti i file** opzione in cima **Esplora soluzioni** per aggiornare l'elenco di file se non viene visualizzato i nuovo file.  
  
    **Esplora soluzioni** Mostra ora i file di progetto aggiornato. 

    ![Interfaccia utente e la navigazione - Esplora soluzioni](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Aggiunta di pagine

Prima di aggiungere navigazione nell'applicazione Web, si aggiungerà innanzitutto due nuove pagine che verrà visualizzata. Più avanti in questa serie di esercitazioni si visualizzeranno i prodotti e i dettagli del prodotto in queste nuove pagine.

1. Nelle **Esplora soluzioni**, fare doppio clic su **WingtipToys**, fare clic su **Add**, quindi fare clic su **nuovo elemento**.   
 Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Quindi, selezionare **Web Form con pagina Master** dalla parte centrale elencare e denominarlo *ProductList. aspx*. 

    ![Interfaccia utente e la navigazione - Aggiungi finestra di dialogo Nuovo elemento](ui_and_navigation/_static/image3.png)
3. Selezionare **Site. master** collegare la pagina master per l'oggetto appena creato *aspx* pagina. 

    ![Interfaccia utente e la navigazione - Seleziona pagina Master](ui_and_navigation/_static/image4.png)
4. Aggiungere una pagina aggiuntiva denominata *ProductDetails.aspx* seguendo questa procedura stessa.

### <a name="updating-bootstrap"></a>L'aggiornamento di Bootstrap

Usano i modelli di progetto di Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), un framework di layout e temi creato da Twitter. Bootstrap Usa CSS3 per fornire progettazione reattiva, ovvero i layout possono adattarsi dinamicamente alle dimensioni di finestra diversa del browser. È possibile usare anche funzionalità temi del Bootstrap per rendere facilmente effettiva una modifica nell'aspetto dell'applicazione. Per impostazione predefinita, il modello di applicazione Web ASP.NET in Visual Studio 2013 include Bootstrap come pacchetto NuGet.

In questa esercitazione si modificherà aspetto dell'applicazione Wingtip Toys, sostituendo i file CSS Bootstrap.

1. Nelle **Esplora soluzioni**, aprire il *contenuto* cartella.
2. Fare doppio clic il *bootstrap* del file e rinominarlo *bootstrap original.css*.
3. Rinominare il *bootstrap.min.css* al *bootstrap original.min.css*.
4. Nelle **Esplora soluzioni**, fare doppio clic il *contenuto* cartella e selezionare **Apri cartella in Esplora File**.  
   Verrà visualizzato Esplora File. Si salverà un file CSS bootstrap scaricati da questa posizione.
5. Nel browser, passare a [ http://Bootswatch.com ](http://bootswatch.com/).
6. Scorrere la finestra del browser finché non viene visualizzato il tema Cerulean. 

    ![Interfaccia utente e la navigazione - Cerulean tema](ui_and_navigation/_static/image5.png)
7. Scaricare sia la *bootstrap* file e il *bootstrap.min.css* del file per il *contenuto* cartella. Usare il percorso della cartella contenuto che viene visualizzato nei **Esplora File** finestra in cui è stato aperto in precedenza.
8. Nelle **Visual Studio** in cima **Esplora soluzioni**, selezionare il **Mostra tutti i file** opzione per visualizzare i nuovi file nella cartella del contenuto. 

    ![Interfaccia utente e la navigazione - Esplora soluzioni](ui_and_navigation/_static/image6.png)

   Si noterà che i due nuovi file CSS nel **contenuto** cartella, ma si noti che l'icona accanto a ogni nome di file è disattivato. Ciò significa che il file non è ancora stato aggiunto al progetto.
9. Fare doppio clic il *bootstrap* e il *bootstrap.min.css* i file e selezionare **Includi nel progetto**.   
   Quando si esegue l'applicazione Wingtip Toys più avanti in questa esercitazione, verrà visualizzata la nuova interfaccia utente.

> [!NOTE] 
> 
> Il modello di applicazione Web ASP.NET usa la *Bundle.config* file nella radice del progetto in cui archiviare il percorso dei file CSS Bootstrap.


### <a name="modifying-the-default-navigation"></a>Modifica riquadro di spostamento predefinito

La navigazione predefinita per ogni pagina dell'applicazione può essere modificata modificando l'elemento di elenco non ordinato di navigazione che si trova nel *Site. master* pagina.

1. Nelle **Esplora soluzioni**individuare e aprire il *Site. master* pagina.
2. Aggiungere il collegamento di navigazione aggiuntivi evidenziato in giallo per l'elenco non ordinato illustrato di seguito:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Come può notare nel codice HTML riportato sopra, ogni voce è stato modificato `<li>` contenente un tag di ancoraggio `<a>` con un collegamento `href` attributo. Ogni `href` punta a una pagina nell'applicazione Web. Nel browser, quando un utente fa clic su uno di questi collegamenti (ad esempio **prodotti**), sono passerà alla pagina contenuta nel `href` (ad esempio **ProductList. aspx**). Si eseguirà l'applicazione alla fine di questa esercitazione.

> [!NOTE] 
> 
> La tilde (`~`) carattere viene utilizzato per specificare che il `href` percorso in cui inizia la radice del progetto.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Aggiunta di un controllo dei dati per visualizzare i dati di navigazione

Successivamente, si aggiungerà un controllo per visualizzare tutte le categorie dal database. Ogni categoria verrà utilizzato come un collegamento per il *ProductList. aspx* pagina. Quando un utente fa clic su un collegamento categoria nel browser, essi verranno passare alla pagina di prodotti e visualizzare solo i prodotti associati alla categoria selezionata.

Si userà una **ListView** controllo per visualizzare tutte le categorie contenute nel database. Per aggiungere un **ListView** controllo alla pagina master:

1. Nel *Site. master* pagina, aggiungere il codice seguente evidenziato `<div>` elemento **dopo** il `<div>` elemento che contiene il `id="TitleContent"` aggiunti in precedenza:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Questo codice visualizza tutte le categorie dal database. Il **ListView** consente di visualizzare ogni nome di categoria come testo del collegamento e include un collegamento al controllo il *ProductList. aspx* pagina con un valore di stringa di query contenente il `ID` della categoria. Impostando il `ItemType` proprietà nel **ListView** controllare, l'espressione di associazione dati `Item` è disponibile all'interno di `ItemTemplate` nodo e il controllo diventa fortemente tipizzati. È possibile selezionare i dettagli del `Item` dell'oggetto usando IntelliSense, ad esempio specificando le `CategoryName`. Questo codice è contenuto all'interno del contenitore `<%#: %>` che contrassegna un'espressione di associazione dati. Aggiungendo i due punti (:) alla fine del `<%#` prefisso, il risultato dell'espressione di associazione dati è codificata in formato HTML. Quando il risultato è codificata in formato HTML, l'applicazione meglio contro il tra siti (XSS) injection e attacchi intrusivi nel codice HTML di script.

> [!NOTE] 
> 
> **Suggerimento**
> 
> Quando si aggiunge codice digitando durante lo sviluppo, è possibile essere certi che un membro valido di un oggetto è stato trovato perché fortemente tipizzate controlli dati mostrano i membri disponibili basati su IntelliSense. IntelliSense offre opzioni di codice appropriato al contesto durante la digitazione di codice, ad esempio proprietà, metodi e oggetti.


Nel passaggio successivo, si implementerà il `GetCategories` metodo per recuperare i dati.

### <a name="linking-the-data-control-to-the-database"></a>Collegamento di controllo dei dati al Database

Prima che sia possibile visualizzare i dati nel controllo dei dati, è necessario collegare il controllo dei dati nel database. Per rendere il collegamento, è possibile modificare il code-behind del *Site.Master.cs* file.

1. In **Esplora soluzioni**, fare doppio clic il *Site* e quindi fare clic su **Visualizza codice**. Il *Site.Master.cs* file viene aperto nell'editor.
2. Prossimità dell'inizio del *Site.Master.cs* , aggiungere due spazi dei nomi aggiuntivi in modo che tutti gli spazi dei nomi inclusi apparire come segue:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Aggiungere evidenziata `GetCategories` metodo dopo il `Page_Load` gestore dell'evento come indicato di seguito:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Il codice sopra riportato viene eseguito quando qualsiasi pagina che utilizza la pagina master viene caricata nel browser. Il `ListView` controllo (denominati "categoryList") che è stato aggiunto in precedenza in questa esercitazione Usa l'associazione di modelli per selezionare i dati. Nel markup del `ListView` è impostare il controllo di controllo `SelectMethod` proprietà di `GetCategories` metodo illustrato in precedenza. Il `ListView` le chiamate di controllo di `GetCategories` metodo nel momento adeguato del ciclo di vita della pagina del ciclo e associa automaticamente i dati restituiti. Verranno fornite informazioni sull'associazione di dati nella prossima esercitazione.

### <a name="running-the-application-and-creating-the-database"></a>Esecuzione dell'applicazione e la creazione del Database

In precedenza in questa serie di esercitazioni si created-classe di un inizializzatore (denominata "ProductDatabaseInitializer") e specificare questa classe nella *global.asax.cs* file. Entity Framework genera il database quando l'applicazione viene eseguita la prima volta, poiché il `Application_Start` contenuta nel metodo il *global.asax.cs* file chiamerà la classe dell'inizializzatore. La classe inizializzatore userà le classi del modello (`Category` e `Product`) che aggiunto in precedenza in questa serie di esercitazioni per creare il database.

1. Nelle **Esplora soluzioni**, fare doppio clic il *default. aspx* pagina e selezionare **imposta come pagina iniziale**.
2. In Visual Studio premere **F5**.   
 Richiederà un po' di tempo per tutti gli elementi configurata durante questa prima esecuzione.   
    ![Interfaccia utente e la navigazione - Browser Windows](ui_and_navigation/_static/image7.png)  
 Quando si esegue l'applicazione, l'applicazione verrà compilata e il database denominato *wingtiptoys.mdf* verrà creato nel *App\_dati* cartella. Nel browser, verrà visualizzato un menu di navigazione categoria. Questo menu è stato generato tramite il recupero delle categorie dal database. Nella prossima esercitazione, si implementerà la navigazione.
3. Chiudere il browser per arrestare l'applicazione in esecuzione.

### <a name="reviewing-the-database"></a>Esaminare il Database

Aprire il *Web. config* file ed esaminare la sezione della stringa di connessione. È possibile notare che il `AttachDbFilename` valore nella stringa di connessione punti al `DataDirectory` per il progetto di applicazione Web. Il valore `|DataDirectory|` è un valore riservato che rappresenta il *App\_dati* cartella nel progetto. Questa cartella è in cui si trova il database che è stato creato da classi di entità.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Se il *App\_Data* cartella non è visibile o se la cartella è vuota, selezionare il **aggiornare** icona e quindi la **Mostra tutti i file** icona nella parte superiore della finestra di **Esplora soluzioni** finestra. Espandere la larghezza del **Esplora soluzioni** windows potrebbe essere necessario per visualizzare tutte le icone disponibili.


A questo punto è possibile esaminare i dati contenuti nel *wingtiptoys.mdf* file di database tramite il **Esplora Server** finestra.

1. Espandere la *App\_dati* cartella. Se il *App\_dati* cartella non è visibile, vedere la nota precedente.
2. Se il *wingtiptoys.mdf* file di database non è visibile, selezionare la **aggiornare** icona e quindi il **Mostra tutti i file** icona nella parte superiore del **Esplora soluzioni**  finestra.
3. Fare doppio clic il *wingtiptoys.mdf* file di database, quindi scegliere **Open**.  
    **Esplora server** viene visualizzato. 

    ![Interfaccia utente e la navigazione - Esplora Server](ui_and_navigation/_static/image8.png)
4. Espandere la *tabelle* cartella.
5. Fare doppio clic il **prodotti**tabelle e selezionare **Mostra dati tabella**.  
 Il **prodotti** tabella viene visualizzata. 

    ![Navigazione - tabella Products e dell'interfaccia utente](ui_and_navigation/_static/image9.png)
6. In questa vista consente di visualizzare e modificare i dati nella **prodotti** tabella manualmente.
7. Chiudi il **prodotti** finestra di tabella.
8. Nel **Esplora Server**, fare doppio clic il **prodotti** nuovamente di tabella e selezionare **Apri definizione tabella**.  
 Dati di progettazione per il **prodotti** tabella viene visualizzata. 

    ![Interfaccia utente e la navigazione - progettazione di prodotti](ui_and_navigation/_static/image10.png)
9. Nel **T-SQL** scheda verrà visualizzato l'istruzione SQL DDL che è stato usato per creare la tabella. È anche possibile usare l'interfaccia utente di **progettazione** pressione di tab per modificare lo schema.
10. Nel **Esplora Server**, fare doppio clic su **WingtipToys** del database e selezionare **3=chiusura connessione**.   
 Scollegando il database da Visual Studio, lo schema del database sarà in grado di modificare più avanti in questa serie di esercitazioni.
11. Tornare alla **Esplora soluzioni**selezionando le **Esplora soluzioni** scheda nella parte inferiore della **Esplora Server** finestra.

## <a name="summary"></a>Riepilogo

In questa esercitazione della serie è stato aggiunto alcuni interfaccia utente di base, elementi grafici, le pagine e navigazione. Inoltre, è stata eseguita l'applicazione Web, che il database creato dalle classi di dati che è stato aggiunto nell'esercitazione precedente. Anche visualizzato il contenuto del *prodotti* tabella del database visualizzando direttamente il database. Nella prossima esercitazione, verranno visualizzati gli elementi di dati e i dettagli del database.

## <a name="additional-resources"></a>Risorse aggiuntive

[Introduzione alla programmazione di ASP.NET Web Pages](https://msdn.microsoft.com/library/ms178125.aspx)   
[Cenni preliminari sui controlli Server Web ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Esercitazione CSS](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Precedente](create_the_data_access_layer.md)
> [Successivo](display_data_items_and_details.md)
