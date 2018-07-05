---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
title: Creazione di un Layout a livello di sito tramite le pagine Master (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato nozioni di base di pagina master. Vale a dire, cosa sono le pagine master, qual è una creazione di una pagina master, che cosa sono i segnaposto del contenuto, come fa un cr...
ms.author: aspnetcontent
ms.date: 05/21/2008
ms.assetid: 78f8d194-03b9-44a5-8255-90e7cd1c2ee1
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: cf9264f6569fe1a5c0369b17a2358c5207cad6ad
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829279"
---
<a name="creating-a-site-wide-layout-using-master-pages-c"></a>Creazione di un Layout a livello di sito tramite le pagine Master (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_CS.pdf)

> In questa esercitazione verrà illustrato nozioni di base di pagina master. Quali sono le pagine master, come uno crei una pagina master, vale a dire, quali sono i segnaposto di contenuto, come uno crei una pagina ASP.NET che usa una pagina master, come la modifica della pagina master si ripercuote automaticamente le pagine di contenuto associate e così via.


## <a name="introduction"></a>Introduzione

Un attributo di un sito Web ben progettato è il layout coerente a livello di sito. Si consideri ad esempio il sito Web www.asp.net. Al momento della stesura di questo articolo, ogni pagina ha lo stesso contenuto nella parte superiore e inferiore della pagina. Come illustrato nella figura 1, la parte superiore di ogni pagina Visualizza una barra grigia con un elenco di Communities Microsoft. Sotto il logo del sito, ovvero l'elenco di lingue in cui è stato tradotto il sito e le sezioni principali: Home, iniziare a usare, altre, download e così via. Allo stesso modo, la parte inferiore della pagina include informazioni sulla pubblicità www.asp.net, una dichiarazione di copyright e un collegamento all'informativa sulla privacy.


[![Il sito Web www.asp.net impiega un aspetto uniforme e coerente in tutte le pagine](creating-a-site-wide-layout-using-master-pages-cs/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image1.png)

<strong>Figura 01</strong>: il sito Web www.asp.net impiega un aspetto coerente e sentirsi tra tutte le pagine ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-site-wide-layout-using-master-pages-cs/_static/image3.png))


Un altro attributo di un sito ben progettato è la facilità con cui può essere modificato l'aspetto del sito. Figura 1 mostra la home page di www.asp.net a partire da marzo 2008, ma da ora e pubblicazione dell'esercitazione, l'aspetto sia stato modificato. Ad esempio le voci di menu nella parte superiore si espanderà per includere una nuova sezione per il framework di MVC. O forse sarà unveiled una progettazione completamente nuova con diversi colori, tipi di carattere e layout. Applicare tali modifiche all'intero sito deve essere un processo rapido e semplice che non richiede la modifica di migliaia di pagine web che costituiscono il sito.

Creazione di un modello di pagina a livello di sito in ASP.NET è possibile tramite l'uso di *pagine master*. In poche parole, una pagina master è un tipo speciale di pagina ASP.NET che definisce il markup che sono comune a tutti *pagine di contenuto* , nonché le aree che possono essere personalizzate in base a una pagina di contenuto dal contenuto della pagina. (Una pagina di contenuto è una pagina ASP.NET che è associata alla pagina master). Ogni volta che viene modificata una pagina master layout o la formattazione, tutti di output relativo alla pagina contenuto viene aggiornato immediatamente in modo analogo, che rende l'applicazione delle modifiche a livello di sito aspetto semplice come l'aggiornamento e la distribuzione di un singolo file (vale a dire, la pagina master).

Questa è la prima esercitazione di una serie di esercitazioni che illustrano l'utilizzo delle pagine master. Nel corso di questa serie di esercitazioni è:

- Esaminare la creazione di pagine master e le pagine di contenuto associate,
- Illustrate un'ampia gamma di suggerimenti, trucchi e trabocchetti,
- Identificare i problemi comuni relativi pagina master ed esplorare le soluzioni alternative,
- Informazioni su come accedere alla pagina master da una pagina di contenuto e un viceversa,
- Informazioni su come specificare un contenuto della pagina master in fase di esecuzione e
- Altre avanzate argomenti relativi alla pagina master.

Queste esercitazioni sono pensate per essere concise e vengono fornite istruzioni dettagliate con numerose catture di schermata che illustra il processo in modo visivo. Ogni esercitazione è disponibile nelle versioni di Visual Basic e c# e comprende un download del codice completo utilizzato.

Questa esercitazione inaugurale inizia con uno sguardo alle nozioni di base di pagina master. Abbiamo discutere funzionamento delle pagine master, esaminerà la creazione di una pagina master e pagine di contenuto associate con Visual Web Developer e vedere come le modifiche apportate a una pagina master vengono immediatamente riflessi nelle relative pagine di contenuto. Iniziamo!

## <a name="understanding-how-master-pages-work"></a>Comprendere funzionamento delle pagine Master

Creazione di un sito Web con un layout di pagina a livello di sito coerenti, è necessario che ogni pagina web emettere markup di formattazione comuni oltre il contenuto personalizzato. Ad esempio, anche se ogni post di forum o esercitazione sul www.asp.net avere il proprio contenuto univoco, ognuna di queste pagine anche eseguire il rendering di una serie di common `<div>` gli elementi che consentono di visualizzare i collegamenti di sezione di primo livello: Home, iniziare a usare, fare riferimento alle informazioni e così via.

Esistono varie tecniche per la creazione di pagine web con un aspetto uniforme e coerente. Un approccio di tipo naive è sufficiente copiare e incollare il markup di layout comuni in tutte le pagine web, ma questo approccio presenta numerosi svantaggi. Per i principianti, ogni volta che viene creata una nuova pagina, è necessario ricordarsi di copiare e incollare il contenuto condiviso nella pagina. Questo tipo copiando e incollando le operazioni sono pronte per l'errore accidentalmente si può copiare solo un subset del markup condiviso in una nuova pagina. E per primi viene, questo approccio rende sostituendo l'aspetto a livello di sito esistente con un nuovo un reale sofferenza perché ogni singola pagina del sito deve essere modificato per usare il nuovo aspetto grafico.

Prima di ASP.NET versione 2.0, gli sviluppatori spesso inseriti comuni markup nella pagina [controlli utente](https://msdn.microsoft.com/library/y6wb1a0e.aspx) e quindi aggiungere questi controlli utente per ogni singola pagina. Questo approccio è necessario che lo sviluppatore della pagina ricordarsi di aggiungere manualmente i controlli utente a ogni nuova pagina, ma è consentito per le modifiche a livello di sito più semplice perché quando si aggiorna il markup comune solo i controlli utente necessarie per essere modificato. Sfortunatamente, Visual Studio .NET 2002 e 2003 - le versioni di Visual Studio consente di creare applicazioni ASP.NET 1.x - eseguito il rendering di controlli utente nella visualizzazione progettazione come caselle di colore grigio. Di conseguenza, gli sviluppatori di pagina usando questo approccio non gode di un ambiente di progettazione WYSIWYG.

Gli svantaggi dell'uso di controlli utente venivano risolti in Visual Studio 2005 e ASP.NET versione 2.0 con l'introduzione del *pagine master*. Una pagina master è un tipo speciale di pagina ASP.NET che definisce sia il markup a livello di sito e il *aree* dove associati *pagine di contenuto* definire i tag personalizzati. Come si vedrà nel passaggio 1, queste aree vengono definite dai controlli ContentPlaceHolder. Il controllo ContentPlaceHolder indica semplicemente una posizione nella gerarchia dei controlli della pagina master in cui può essere inserito il contenuto personalizzato da una pagina di contenuto.

> [!NOTE]
> Concetti di base e funzionalità delle pagine master non è stata modificata in ASP.NET versione 2.0. Tuttavia, Visual Studio 2008 offre supporto in fase di progettazione per pagine master annidate, una funzionalità che non era disponibile in Visual Studio 2005. Si esaminerà tramite pagine master annidate in un'esercitazione futura.


Figura 2 mostra ciò che potrebbe essere simile alla pagina master www.asp.net. Si noti che la pagina master definisce il layout a livello di sito comune - il markup nella parte superiore, inferiore, a destra di ogni pagina - oltre a un controllo ContentPlaceHolder in centro a sinistra, in cui si trova il contenuto univoco per ogni pagina web.


![Una pagina Master definisce il Layout a livello di sito e le aree modificabili in base a una pagina contenuto dal contenuto della pagina](creating-a-site-wide-layout-using-master-pages-cs/_static/image4.png)

**Figura 02**: una pagina Master definisce il Layout a livello di sito e le aree modificabili in base a una pagina contenuto dal contenuto della pagina


Dopo aver definita una pagina master può essere associato a nuove pagine ASP.NET mediante i segni di graduazione di un controllo checkbox. Queste pagine ASP.NET, denominate pagine contenuto - includono un controllo contenuto per ognuno dei controlli ContentPlaceHolder della pagina master. Quando si visita la pagina di contenuto tramite un browser il motore ASP.NET crea gerarchia dei controlli della pagina master e inserisce la gerarchia dei controlli della pagina di contenuto in posizioni appropriate. Viene eseguito il rendering di questa gerarchia dei controlli combinato e viene restituito il codice HTML risultante al browser dell'utente finale. Di conseguenza, la pagina contenuto genera il markup comuni definiti nella relativa pagina master di fuori di controlli ContentPlaceHolder e il markup specifico della pagina definito all'interno di un proprio controlli contenuto. Figura 3 illustra questo concetto.


[![Il Markup della pagina richiesto è Fused nella Master Page](creating-a-site-wide-layout-using-master-pages-cs/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image5.png)

**Figura 03**: del Markup della pagina richiesta la è Fused nella Master Page ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-site-wide-layout-using-master-pages-cs/_static/image7.png))


Ora che abbiamo analizzato funzionamento delle pagine master, diamo un'occhiata alla creazione di una pagina master e pagine di contenuto associate con Visual Web Developer.

> [!NOTE]
> Per raggiungere un vasto pubblico, verrà creato il sito Web ASP.NET sono stati compilati in tutta questa serie di esercitazioni usando ASP.NET 3.5 con versione gratuita di Microsoft Visual Studio 2008 [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Se non sono ancora stati aggiornati a ASP.NET 3.5, non preoccuparti - i concetti illustrati in questi lavoro esercitazioni altrettanto bene con ASP.NET 2.0 e Visual Studio 2005. Tuttavia, alcune applicazioni demo possono utilizzare le nuove funzionalità .NET Framework versione 3.5; Quando vengono usate le funzionalità specifiche 3.5, includere una nota in cui viene illustrato come implementare una funzionalità simile nella versione 2.0. Tenere presente che le applicazioni demo disponibili per scaricare da ogni esercitazione destinazione .NET Framework versione 3.5, che comporta un `Web.config` file che include elementi di configurazione specifici 3.5 e i riferimenti agli spazi dei nomi specifici 3.5 di `using` istruzioni nelle classi ASP.NET alla pagina code-behind. In breve, se hai ancora a installare .NET 3.5 nel computer, l'applicazione scaricabile dal web non funzionerà senza prima rimuovere il markup specifico della 3.5 da `Web.config`. Visualizzare [Sezionando ASP.NET versione 3.5 `Web.config` File](http://www.4guysfromrolla.com/articles/121207-1.aspx) per altre informazioni su questo argomento. È necessario anche rimuovere il `using` istruzioni che fanno riferimento a spazi dei nomi specifici 3.5.


## <a name="step-1-creating-a-master-page"></a>Passaggio 1: Creazione di una pagina Master

Prima che possiamo adesso esplorare la creazione e uso delle pagine master e di contenuto, è innanzitutto necessario un sito Web ASP.NET. Iniziare creando un nuovo file in base al sistema sito Web ASP.NET. A tale scopo, avviare Visual Web Developer e quindi passare al menu File e scegliere Nuovo sito Web, visualizzare la finestra di dialogo Nuovo sito Web (vedere la figura 4). Scegliere il modello di sito Web ASP.NET, impostare l'elenco di riepilogo a discesa percorso al File System, scegliere una cartella in cui inserire il sito web e impostare il linguaggio c#. Verrà creato un nuovo sito web con un `Default.aspx` pagina ASP.NET, un' `App_Data` cartella e un `Web.config` file.

> [!NOTE]
> Visual Studio supporta due modalità di gestione del progetto: progetti di siti Web e progetti di applicazione Web. Progetti di siti Web non dispongono di un file di progetto, mentre i progetti applicazione Web imitare l'architettura di progetto in Visual Studio .NET 2002/2003: includono un file di progetto e compilare codice sorgente del progetto in un singolo assembly, che viene inserito nella `/bin` cartella. I progetti di Visual Studio 2005 inizialmente solo Web sito supportato, anche se il [modello di progetto di applicazione Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) è stato reintrodotto con Service Pack 1. Visual Studio 2008 offre entrambi i modelli di progetto. Visual Web Developer 2005 e 2008 edizioni, tuttavia, supportano solo progetti di siti Web. Usare il modello di progetto di sito Web per la demo in questa serie di esercitazioni. Se si utilizza un'edizione non Express e si vuole usare invece il modello di progetto di applicazione Web, è possibile eseguire questa operazione, ma tenere presente che potrebbero esserci alcune discrepanze tra ciò che viene visualizzato nella schermata e la procedura che è necessario eseguire e le schermate visualizzate e instructio NS forniti in queste esercitazioni.


[![Creare un nuovo sito di Web File basate sul sistema](creating-a-site-wide-layout-using-master-pages-cs/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image8.png)

**Figura 04**: creare un sito Web New File System-Based ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-site-wide-layout-using-master-pages-cs/_static/image10.png))


Successivamente, aggiungere una pagina master al sito nella directory radice facendo clic sul nome del progetto, scegliendo Aggiungi nuovo elemento e selezionando il modello di pagina Master. Si noti che le pagine master terminano con l'estensione `.master`. Denominare questa nuova pagina master `Site.master` e fare clic su Aggiungi.


[![Aggiungere una pagina Master denominato Site. master al sito Web](creating-a-site-wide-layout-using-master-pages-cs/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image11.png)

**Figura 05**: aggiungere un nome di pagina Master `Site.master` nel sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-site-wide-layout-using-master-pages-cs/_static/image13.png))


Aggiunge un nuovo file pagina master tramite Visual Web Developer viene creata una pagina master con il seguente markup dichiarativo:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample1.aspx)]

La prima riga nel markup dichiarativo è il [ `@Master` direttiva](https://msdn.microsoft.com/library/ms228176.aspx). Il `@Master` direttiva è simile al [ `@Page` direttiva](https://msdn.microsoft.com/library/ydy4x04a.aspx) che appare nelle pagine ASP.NET. Definisce il server-side delle lingue (c#) e le informazioni relative alla posizione e l'ereditarietà di classe code-behind della pagina master.

Il `DOCTYPE` e il markup della pagina dichiarativo visualizzato sotto il `@Master` direttiva. La pagina include codice HTML statico con quattro controlli sul lato server:

- **Un modulo Web (il `<form runat="server">`)** , perché tutte le pagine ASP.NET, in genere dispone di un modulo Web - e poiché la pagina master può includere i controlli Web che devono essere visualizzati all'interno di un Web Form - assicurarsi di aggiungere il Web Form alla pagina master (anziché l'aggiunta di un modulo Web per e ACH contenuto pagina).
- **Un controllo ContentPlaceHolder denominato `ContentPlaceHolder1`**  -questo controllo ContentPlaceHolder viene visualizzato all'interno del modulo Web e viene usata come area per l'interfaccia utente della pagina di contenuto.
- **Un server-side `<head>` elemento** : il `<head>` elemento ha la `runat="server"` attributo, rendendo accessibili tramite codice lato server. Il `<head>` elemento viene implementato in questo modo, in modo che il titolo della pagina e altri `<head>`-correlati markup può essere aggiunte o modificato a livello di codice. Ad esempio, impostando una pagina ASP.NET `Title` le modifiche alle proprietà di `<title>` elemento sottoposto a rendering dal `<head>` controllo server.
- **Un controllo ContentPlaceHolder denominato `head`**  -questo controllo ContentPlaceHolder viene visualizzato all'interno di `<head>` server di controllo e può essere utilizzato in modo dichiarativo aggiungere contenuti al `<head>` elemento.

Questo markup dichiarativo di pagina master predefinita funge da punto di partenza per la progettazione di pagine master. È possibile modificare il codice HTML o per aggiungere altri controlli Web o elementi ContentPlaceHolder della pagina master.

> [!NOTE]
> Quando si progetta una pagina master assicurarsi che la pagina master contiene un modulo Web e che almeno un controllo ContentPlaceHolder viene visualizzato in questo Web Form.


### <a name="creating-a-simple-site-layout"></a>Creazione di un Layout semplice sito

Espandiamo `Site.master`del markup dichiarativo predefinito per creare un layout del sito in cui condividono tutte le pagine: un'intestazione comune, una colonna a sinistra con navigazione, notizie e altro contenuto a livello di sito; e un piè di pagina che viene visualizzata l'icona "Con tecnologia per Microsoft ASP.NET". Figura 6 illustra il risultato finale della pagina master quando una delle relative pagine di contenuto viene visualizzata tramite un browser. L'area in un cerchio rosso nella figura 6 è specifico per la pagina visitata (`Default.aspx`); gli altri contenuti sono definito nella pagina master e di conseguenza coerente in tutte le pagine di contenuto.


[![La pagina Master definisce il Markup per la parte superiore sinistra e parti inferiore](creating-a-site-wide-layout-using-master-pages-cs/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image14.png)

**Figura 06**: la pagina Master definisce il Markup per la parte superiore sinistra e parti inferiore ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-site-wide-layout-using-master-pages-cs/_static/image16.png))


Per ottenere il layout del sito illustrato nella figura 6, iniziare aggiornando il `Site.master` pagina master in modo che contenga il seguente markup dichiarativo:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample2.aspx)]

Layout della pagina master viene definito mediante una serie di `<div>` elementi HTML. Il `topContent` `<div>` contiene il markup che viene visualizzato nella parte superiore di ogni pagina, mentre le `mainContent`, `leftContent`, e `footerContent` `<div>` s vengono utilizzati per visualizzare il contenuto della pagina, la colonna a sinistra e il "con tecnologia da Microsoft Icona ASP.NET", rispettivamente. Oltre ad aggiungere queste `<div>` elementi, sono inoltre stati rinominati i `ID` proprietà del controllo ContentPlaceHolder primario dal `ContentPlaceHolder1` a `MainContent`.

Le regole di formattazione e layout per queste diverse `<div>` elementi è descritto nel [foglio di stile CSS (Cascading)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) file `Styles.css`, che viene specificato tramite una &lt;collegamento&gt; elemento di pagina master &lt;head&gt; elemento. Queste diverse regole definiscono l'aspetto della ognuno `<div>` elemento indicato in precedenza. Ad esempio, il `topContent` `<div>` elemento, che consente di visualizzare il testo "Esercitazioni su pagine Master" e il collegamento, dispone di proprie regole di formattazione specificate nella `Styles.css` come indicato di seguito:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample3.css)]

Se si segue in prossimità del computer, è necessario scaricare codice di accompagnamento dell'esercitazione e aggiungere il `Styles.css` file al progetto. Analogamente, è necessario anche creare una cartella denominata immagini e copiare l'icona "Con tecnologia per Microsoft ASP.NET" dal sito Web demo scaricato al progetto.

> [!NOTE]
> Una descrizione delle CSS e formattazione delle pagine web esula dall'ambito di questo articolo. Per altre informazioni su CSS, consultare il [CSS esercitazioni](http://www.w3schools.com/css/default.asp) alla [W3Schools.com](http://www.w3schools.com/). Invitiamo anche a scaricare il codice associato dell'esercitazione e provare a usare le impostazioni CSS in `Styles.css` per osservare gli effetti delle diverse regole di formattazione.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Creazione di una pagina Master utilizzando un modello di progettazione esistente

Nel corso degli anni ho creato un numero di applicazioni web ASP.NET per le aziende di piccole e medie. Alcuni client ha un layout del sito esistente che volevano usare; altri assunzione di una finestra di progettazione grafica competente. Alcuni affidata me per progettare il layout del sito Web. Come si può vedere nella figura 6, tasking del programmatore per progettare layout di un sito Web è in genere come buona norma come se avessero la finanziario eseguire open-heart surgery mentre il dottore esegue le imposte.

Per fortuna, esistono innumerous siti Web che offrono i modelli di progettazione HTML gratuiti - Google ha restituito risultati oltre sei milioni per il termine di ricerca "modelli di sito Web gratuito." Uno di quelli personali preferite [OpenDesigns.org](http://opendesigns.org/). Dopo aver trovato un modello di sito Web desiderato, aggiungere il file CSS e immagini al progetto di sito Web e l'integrazione HTML del modello nella pagina master.

> [!NOTE]
> Microsoft offre anche numerose [ASP.NET progettazione avvia Kit di modelli gratuiti](https://msdn.microsoft.com/asp.net/aa336613.aspx) integrate nella finestra di dialogo Nuovo sito Web in Visual Studio.


## <a name="step-2-creating-associated-content-pages"></a>Passaggio 2: Creazione di associate le pagine di contenuto

Con la pagina master creata, siamo pronti iniziare a creare pagine ASP.NET che sono associate alla pagina master. Tali pagine sono dette *pagine di contenuto*.

È possibile aggiungere una nuova pagina ASP.NET al progetto e associarlo al `Site.master` pagina master. Fare doppio clic sul nome del progetto in Esplora soluzioni e scegliere l'opzione Aggiungi nuovo elemento. Selezionare il modello Web Form, immettere il nome `About.aspx`e quindi selezionare la casella di controllo "Seleziona pagina master", come illustrato nella figura 7. In questo modo, l'istruzione Select verrà visualizzato una finestra di dialogo pagina Master casella (vedere la figura 8) da cui è possibile scegliere la pagina master da usare.

> [!NOTE]
> Se è stato creato un sito Web ASP.NET usando il modello di progetto di applicazione Web anziché il modello di progetto di sito Web non si vedranno la casella di controllo "Seleziona pagina master" nella finestra di dialogo Aggiungi nuovo elemento illustrato nella figura 7. Per creare un contenuto pagina quando si utilizza il progetto di applicazione Web del modello è necessario scegliere il modello Web Form di contenuto anziché il modello Web Form. Dopo aver selezionato il modello di Form di contenuto Web e facendo clic su Aggiungi, lo stesso Seleziona pagina Master verrà visualizzata la finestra di dialogo illustrata nella figura 8.


[![Aggiungere una nuova pagina contenuta](creating-a-site-wide-layout-using-master-pages-cs/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image17.png)

**Figura 07**: aggiungere una nuova pagina contenuto ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-site-wide-layout-using-master-pages-cs/_static/image19.png))


[![Selezionare la pagina Master Site. master](creating-a-site-wide-layout-using-master-pages-cs/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image20.png)

**Figura 08**: selezionare il `Site.master` pagina Master ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-site-wide-layout-using-master-pages-cs/_static/image22.png))


Come il seguente markup dichiarativo, una nuova pagina di contenuto contiene una `@Page` direttiva che punta al relativo schema pagina e un controllo contenuto per ognuno dei controlli ContentPlaceHolder della pagina master.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample4.aspx)]

> [!NOTE]
> Nella sezione "Creazione di un semplice sito Layout" nel passaggio 1 sono stati rinominati `ContentPlaceHolder1` a `MainContent`. Se non è stato rinominato in questo controllo ContentPlaceHolder `ID` nello stesso modo, il contenuto del markup della pagina dichiarativo sarà leggermente diversa dal markup illustrato in precedenza. Vale a dire, secondo del controllo del contenuto `ContentPlaceHolderID` rifletteranno il `ID` di corrispondente ContentPlaceHolder controllare nella pagina master.


Durante il rendering di una pagina di contenuto, il motore ASP.NET deve fuse della pagina con controlli ContentPlaceHolder relativa della pagina master controlli del contenuto. Il motore ASP.NET determina la pagina master alla pagina di contenuto dal `@Page` della direttiva `MasterPageFile` attributo. Come mostra il markup riportato sopra, questa pagina di contenuto è associata a `~/Site.master`.

Poiché la pagina master contiene due controlli ContentPlaceHolder - `head` e `MainContent` -Visual Web Developer generati due controlli contenuto. Ogni controllo contenuto fa riferimento a un particolare controllo ContentPlaceHolder tramite relativo `ContentPlaceHolderID` proprietà.

In cui le pagine master caratterizzano su tecniche del modello a livello di sito precedente consiste nell'usare il relativo supporto in fase di progettazione. Figura 9 mostra la `About.aspx` pagina contenuto quando viene visualizzato in visualizzazione di progettazione di Visual Web per gli sviluppatori. Si noti che mentre il contenuto della pagina master è visibile, è in grigio e non può essere modificato. I controlli contenuto corrispondenti a elementi ContentPlaceHolder della pagina master sono, tuttavia, modificabili. E proprio come con qualsiasi altra pagina ASP.NET, è possibile creare contenuto dell'interfaccia della pagina aggiungendo i controlli Web tramite le viste di origine o di progettazione.


[![Visualizzazione di progettazione della pagina di contenuto vengono visualizzati sia il contenuto della pagina Master e specifico della pagina](creating-a-site-wide-layout-using-master-pages-cs/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image23.png)

**Figura 09**: il contenuto della pagina Progettazione Vista consente di visualizzare sia le specifiche delle pagine e contenuto della pagina Master ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-site-wide-layout-using-master-pages-cs/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Aggiunta di controlli Web e Markup alla pagina di contenuto

Si consiglia di creare contenuto per il `About.aspx` pagina. Come può vedere nella figura 10, che dopo avere immesso un titolo "Sull'autore" e un paio di paragrafi di testo, ma è possibile aggiungere controlli Web, troppo. Dopo aver creato questa interfaccia, visitare il `About.aspx` pagina tramite un browser.


[![Visita la pagina About tramite un Browser](creating-a-site-wide-layout-using-master-pages-cs/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image26.png)

**Figura 10**: visitare la `About.aspx` tramite un Browser a pagina ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-site-wide-layout-using-master-pages-cs/_static/image28.png))


È importante comprendere che la pagina di contenuto richiesta e la pagina master associata sono fused e viene eseguito il rendering nel suo complesso interamente sul server web. Browser dell'utente finale viene inviato il codice HTML risultante, congiunta. Per verificarlo, visualizzare il codice HTML ricevuto dal browser passando al menu Visualizza e scelta di origine. Si noti che non esistono Nessun frame o qualsiasi altre tecniche specializzate per la visualizzazione delle pagine web diverse in una singola finestra.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Associazione di una pagina Master a una pagina ASP.NET esistente

Come abbiamo visto in questo passaggio, l'aggiunta di che una nuova pagina contenuta per un'applicazione web ASP.NET è sufficiente selezionare la casella di controllo "Seleziona pagina master" e il prelievo della pagina master. Sfortunatamente, la conversione di una pagina ASP.NET esistente in una pagina master non è altrettanto semplice.

Per associare una pagina master per una pagina ASP.NET esistente è necessario eseguire la procedura seguente:

1. Aggiungere il `MasterPageFile` dell'attributo della pagina ASP.NET `@Page` direttiva, di modo che punti alla pagina master appropriata.
2. Aggiungere controlli contenuto per ognuna delle ContentPlaceHolder nella pagina master.
3. In modo selettivo tagliare e incollare il contenuto esistente della pagina ASP.NET nei controlli del contenuto appropriati. Dico "in modo selettivo" qui perché probabilmente di pagina ASP.NET contenente il markup che è già espressa dalla pagina master, ad esempio la `DOCTYPE`, il `<html>` elemento e il Web Form.

Per istruzioni dettagliate su questo processo con schermate, consultare [Scott Guthrie](https://weblogs.asp.net/scottgu/)del [tramite pagine Master e spostamento nel sito](http://webproject.scottgu.com/CSharp/MasterPages/MasterPages.aspx) esercitazione. Il "Update `Default.aspx` e `DataSample.aspx` utilizzare la pagina Master" sezione vengono illustrati tali passaggi.

Poiché è molto più facile creare nuove pagine di contenuto anziché convertire pagine ASP.NET esistenti in pagine di contenuto, è consigliabile che ogni volta che crea un nuovo sito Web ASP.NET aggiungere una pagina master al sito. Associare tutte le nuove pagine ASP.NET a questa pagina master. Non occorre preoccuparsi se la pagina master iniziale è molto semplice o normale; è possibile aggiornare la pagina master in un secondo momento.

> [!NOTE]
> Quando si crea una nuova applicazione ASP.NET, Visual Web Developer viene aggiunto un `Default.aspx` pagina in cui non è associato a una pagina master. Se si desidera far pratica la conversione di una pagina ASP.NET esistente in una pagina di contenuto, proseguire e farlo con `Default.aspx`. In alternativa, è possibile eliminare `Default.aspx` e quindi aggiungere di nuovo, ma questa volta selezionando la casella di controllo "Seleziona pagina master".


## <a name="step-3-updating-the-master-pages-markup"></a>Passaggio 3: Aggiornamento di Markup della pagina Master

Uno dei principali vantaggi delle pagine master è una singola pagina master può essere utilizzata per definire il layout generale per numerose pagine nel sito. Pertanto, aggiorna l'aspetto del sito, è necessario aggiornare un singolo file - pagina master.

Per illustrare questo comportamento, è possibile aggiornare la pagina master per includere la data corrente nella parte superiore della colonna a sinistra. Aggiungere un'etichetta denominata `DateDisplay` per il `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample5.aspx)]

Creare quindi un `Page_Load` gestore dell'evento per il servizio master di pagina e aggiungere il codice seguente:

[!code-csharp[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample6.cs)]

Il codice precedente imposta l'etichetta `Text` proprietà per la data corrente e l'ora è formattata come il giorno della settimana, il nome del mese e giorno a due cifre (vedere la figura 11). Con questa modifica, visitare di nuovo una delle pagine di contenuto. Come illustrato nella figura 11, il codice risulta viene immediatamente aggiornato per includere la modifica della pagina master.


[![Le modifiche alla pagina Master vengono applicate quando la visualizzazione di una pagina di contenuto](creating-a-site-wide-layout-using-master-pages-cs/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image29.png)

**Figura 11**: la modifiche alla pagina Master vengono applicate quando la visualizzazione di una pagina di contenuto ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-site-wide-layout-using-master-pages-cs/_static/image31.png))


> [!NOTE]
> Come illustrato in questo esempio, le pagine master possono contenere i controlli Web lato server, codice e i gestori eventi.


## <a name="summary"></a>Riepilogo

Pagine master consentono agli sviluppatori ASP.NET progettare un layout a livello di sito coerente che può essere facilmente aggiornato. Creazione di pagine master e le pagine di contenuto associate è semplice come la creazione di pagine ASP.NET standard, come Visual Web Developer offre supporto avanzato in fase di progettazione.

Nell'esempio di pagina master è stato creato in questa esercitazione ha due controlli ContentPlaceHolder `head` e `MainContent`. Viene specificata solo markup per il `MainContent` controllo ContentPlaceHolder nella nostra pagina contenuta, tuttavia. Nella prossima esercitazione spiega come usare il contenuto più controlli nella pagina di contenuto. Viene inoltre illustrato come definire predefinito markup per il contenuto controlla all'interno della pagina master, nonché come definito per alternare usando il valore predefinito nel master markup pagina e fornire codice personalizzato dalla pagina contenuto.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [ASP.NET per le finestre di progettazione: liberare i modelli di progettazione e linee guida sulla creazione di siti Web ASP.NET usando gli standard Web](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Cenni preliminari sulle pagine Master ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Esercitazioni di fogli di stile (CSS) a catena](http://www.w3schools.com/css/default.asp)
- [Impostazione in modo dinamico il titolo della pagina](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Pagine master in ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Esercitazioni introduttive di pagine master](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla.com, ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach manualmente ASP.NET 3.5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [avanti](multiple-contentplaceholders-and-default-content-cs.md)
