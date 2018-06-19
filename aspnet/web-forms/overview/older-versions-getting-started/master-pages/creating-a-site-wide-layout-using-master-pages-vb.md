---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: Creazione di un Layout a livello di sito utilizzando le pagine Master (VB) | Documenti Microsoft
author: rick-anderson
description: Questa esercitazione verrà mostrato nozioni fondamentali sulla pagina master. In particolare, quali sono le pagine master, come esegue una creazione di una pagina master, quali sono i segnaposto del contenuto, funzionamento un cr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: d18993af7159de552db0c622fbef58e814e36ebb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890879"
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Creazione di un Layout a livello di sito utilizzando le pagine Master (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> Questa esercitazione verrà mostrato nozioni fondamentali sulla pagina master. In particolare, quali sono le pagine master, come uno crei una pagina master, quali sono i segnaposto di contenuto, come uno crei una pagina ASP.NET che utilizza una pagina master, come la modifica della pagina master viene riflessa automaticamente nei relativi pagine contenuto associate e così via.


## <a name="introduction"></a>Introduzione

Un attributo di un sito Web ben progettato è un layout coerente a livello di sito. Il sito Web www.asp.net, si consideri ad esempio. Al momento della redazione del presente documento, ogni pagina contiene lo stesso contenuto nella parte superiore e inferiore della pagina. Come illustrato nella figura 1, all'inizio di ogni pagina Visualizza una barra grigia con un elenco di Microsoft Communities. Di sotto di che rappresenta il logo del sito, l'elenco delle lingue in cui è stato tradotto il sito e le sezioni principali: Home, introduzione, informazioni, download e così via. Analogamente, la parte inferiore della pagina include informazioni su pubblicità www.asp.net, informazioni di copyright e un collegamento all'informativa sulla privacy.


[![Il sito Web www.asp.net impiega un aspetto coerente in tutte le pagine](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>Figura 01</strong>: il sito Web www.asp.net impiega un aspetto coerente e, è possibile tra tutte le pagine ([fare clic per visualizzare l'immagine ingrandita](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


Un altro attributo di un sito ben progettato consiste nella facilità con cui può essere modificato l'aspetto del sito. Figura 1 mostra la home page www.asp.net a partire da marzo 2008, ma tra il momento e la pubblicazione di questa esercitazione, sia stato modificato l'aspetto. Ad esempio le voci di menu nella parte superiore verranno espanso per includere una nuova sezione per il framework di MVC. O forse una completamente nuova progettazione con diversi colori, tipi di carattere e layout verrà presentata. L'applicazione di tali modifiche all'intero sito deve essere un processo semplice e veloce che non è necessario modificare le migliaia di pagine web che costituiscono il sito.

La creazione di un modello di pagina a livello di sito in ASP.NET è possibile mediante l'utilizzo di *pagine master*. In breve, una pagina master è un tipo speciale di pagina ASP.NET che definisce il markup che sono comune a tutti *il contenuto di pagine* nonché le aree che possono essere personalizzate in base a una pagina di contenuto dal contenuto della pagina. (Una pagina di contenuto è una pagina ASP.NET che è associata alla pagina master). Ogni volta che viene modificata una pagina master layout o la formattazione, tutti output le pagine contenuto viene aggiornato immediatamente in modo analogo, rendendo l'applicazione delle modifiche a livello di sito aspetto semplice come l'aggiornamento e la distribuzione di un singolo file (in particolare, la pagina master).

Questa è la prima esercitazione di una serie di esercitazioni che esplorano utilizzando le pagine master. Nel corso di questa serie di esercitazioni è:

- Esaminare la creazione di pagine master e le pagine di contenuto associate,
- Discutere una serie di suggerimenti, consigli e trap,
- Identificare i problemi di pagina master comuni e soluzioni alternative, esplorare
- Informazioni su come accedere alla pagina master da una pagina contenuto e un viceversa,
- Informazioni su come specificare un contenuto della pagina master in fase di esecuzione e
- Altri avanzata pagina master.

Queste esercitazioni sono pensate per essere conciso e vengono fornite istruzioni dettagliate con una notevole quantità di schermate guidare il processo in modo visivo. Ogni esercitazione è disponibile in c# e Visual Basic versioni e include il download di codice completo utilizzato.

In questa esercitazione primo inizia con un'occhiata nozioni fondamentali sulla pagina master. Si descrive funzionamento delle pagine master, esaminare la creazione di una pagina master e pagine di contenuto associate tramite Visual Web Developer e vedere come le modifiche a una pagina master vengono applicate immediatamente le pagine di contenuto. Iniziamo!

## <a name="understanding-how-master-pages-work"></a>La comprensione di funzionamento delle pagine Master

Creazione di un sito Web con un layout di pagina a livello di sito coerente, è necessario che ogni pagina web creare tag di formattazione comuni oltre il contenuto personalizzato. Ad esempio, mentre ogni post del forum o esercitazione www.asp.net hanno i propri contenuti univoco, ognuna di queste pagine anche eseguire il rendering di una serie di comune `<div>` gli elementi che consentono di visualizzare i collegamenti della sezione di primo livello: Home, iniziare, fare riferimento alle informazioni e così via.

Esistono diverse tecniche per la creazione di pagine web con un aspetto coerente. Un approccio naïve è semplicemente copiare e incollare il markup di layout comuni a tutte le pagine web, ma questo approccio presenta numerosi svantaggi. Per iniziare, ogni volta che viene creata una nuova pagina, è necessario ricordarsi di copiare e incollare il contenuto condiviso nella pagina. Tali copiando e incollando le operazioni sono particolarmente adatto per errore accidentalmente, è possibile copiare solo un subset del markup condiviso in una nuova pagina. E per primi, questo approccio rende sostituendo l'aspetto a livello di sito esistente con una nuova istanza di una reale sofferenza perché ogni singola pagina del sito deve essere modificata per utilizzare il nuovo aspetto.

Prima di ASP.NET versione 2.0, pagina sviluppatori spesso inseriti markup comuni in [controlli utente](https://msdn.microsoft.com/library/y6wb1a0e.aspx) e quindi aggiungere questi controlli utente a ogni pagina. Questo approccio è necessario che lo sviluppatore della pagina è necessario aggiungere manualmente i controlli utente a ogni nuova pagina, ma è consentita per le modifiche a livello di sito più semplice perché quando si aggiorna il markup comune solo i controlli utente devono essere modificate. Purtroppo, Visual Studio .NET 2002 e 2003 - le versioni di Visual Studio utilizzato per creare le applicazioni ASP.NET 1. x - eseguito il rendering di controlli utente nella visualizzazione progettazione come caselle di colore grigio. Di conseguenza, gli sviluppatori di pagina utilizzando questo approccio non gode di un ambiente in fase di progettazione WYSIWYG.

Gli svantaggi dell'uso di controlli utente sono stati risolti in ASP.NET versione 2.0 e Visual Studio 2005 con l'introduzione di *pagine master*. Una pagina master è un tipo speciale di pagina ASP.NET che definisce i markup a livello di sito e *aree* dove associata *il contenuto di pagine* definire i tag personalizzati. Come verrà illustrato nel passaggio 1, queste aree sono definite dai controlli ContentPlaceHolder. Il controllo ContentPlaceHolder indica semplicemente una posizione nella gerarchia dei controlli della pagina master in cui il contenuto personalizzato può essere inserito da una pagina contenuta.

> [!NOTE]
> I principali concetti e le funzionalità delle pagine master non è cambiato dall'ASP.NET versione 2.0. Tuttavia, Visual Studio 2008 offre supporto in fase di progettazione per le pagine master annidate, una funzionalità che non era disponibile in Visual Studio 2005. Verranno esaminati utilizzando pagine master annidate in un'esercitazione future.


Figura 2 mostra l'aspetto della pagina master per www.asp.net. Si noti che la pagina master definisce il layout di a livello di sito comune - markup superiore, inferiore e destro di ogni pagina, nonché un ContentPlaceHolder al centro a sinistra, in cui si trova il contenuto univoco per ogni pagina web.


![Una pagina Master definisce il Layout a livello di sito e le aree modificabili in base a una pagina contenuto dalla pagina contenuto](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Figura 02**: una pagina Master definisce il Layout a livello di sito e le aree modificabili in base a una pagina contenuto pagina in base al contenuto


Dopo avere definita una pagina master può essere associato alle nuove pagine ASP.NET tramite il segno di graduazione di una casella di controllo. Queste pagine ASP.NET - chiamate pagine di contenuto, includono un controllo contenuto per ognuno dei controlli ContentPlaceHolder della pagina master. Quando si visita la pagina di contenuto tramite un browser il motore ASP.NET crea gerarchia dei controlli della pagina master e inserisce la gerarchia dei controlli della pagina di contenuto nelle posizioni appropriate. Questa gerarchia del controllo combinato viene eseguito il rendering e il codice HTML risultante viene restituito al browser dell'utente finale. Di conseguenza, la pagina contenuta genera il markup comune definiti nella relativa pagina master di fuori dei controlli ContentPlaceHolder sia il markup specifico della pagina definito all'interno di controlli contenuti. Figura 3 viene illustrato questo concetto.


[![Il Markup della pagina richiesta è fusibile nella pagina Master](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Figura 03**: Markup della pagina richiesta la è fusibile nella pagina Master ([fare clic per visualizzare l'immagine ingrandita](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


Ora che sono stati illustrati funzionamento delle pagine master, diamo un'occhiata alla creazione di una pagina master e pagine di contenuto associate tramite Visual Web Developer.

> [!NOTE]
> Per raggiungere un vasto pubblico, verrà creato il sito Web ASP.NET, creata in questa serie di esercitazioni con ASP.NET 3.5 versione gratuita di Microsoft di Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Se non sono ancora stati aggiornati a ASP.NET 3.5, non occorre preoccuparsi - i concetti illustrati in queste esercitazioni di lavoro altrettanto bene con ASP.NET 2.0 e Visual Studio 2005. Tuttavia, alcune applicazioni demo possono utilizzare le nuove funzionalità di .NET Framework versione 3.5; Quando si utilizzano le funzionalità specifiche 3.5, includere una nota in cui viene illustrato come implementare una funzionalità simile nella versione 2.0. Tenere presente che le applicazioni demo disponibili per scaricare da ogni esercitazione destinazione .NET Framework versione 3.5, che comporta un `Web.config` file che include elementi di configurazione specifici 3.5. In breve, se si dispone ancora di installare .NET 3.5 nel computer in uso, quindi l'applicazione web scaricabile non funzionerà senza prima rimuovere il markup specifico della 3.5 da `Web.config`. Vedere [Sezionando ASP.NET versione 3.5 `Web.config` File](http://www.4guysfromrolla.com/articles/121207-1.aspx) per ulteriori informazioni su questo argomento.


## <a name="step-1-creating-a-master-page"></a>Passaggio 1: Creazione di una pagina Master

Prima di è possibile esplorare la creazione e utilizzo delle pagine master e di contenuto, è necessario innanzitutto un sito Web ASP.NET. Iniziare creando un nuovo file system basato su sito Web ASP.NET. A tale scopo, avviare Visual Web Developer e quindi passare al menu File e scegliere Nuovo sito Web, visualizzare la finestra di dialogo Nuovo sito Web (vedere la figura 4). Scegliere il modello di sito Web ASP.NET, impostare l'elenco di riepilogo a discesa percorso al File System, scegliere una cartella in cui inserire il sito web e impostare la lingua di Visual Basic. Verrà creato un nuovo sito web con un `Default.aspx` pagina ASP.NET, un `App_Data` cartella e un `Web.config` file.

> [!NOTE]
> Visual Studio supporta due modalità di gestione dei progetti: progetti di siti Web e progetti di applicazione Web. Progetti di sito Web non dispongono di un file di progetto, mentre i progetti di applicazione Web simulare l'architettura di progetto in Visual Studio .NET 2002/2003 - includono un file di progetto e compilare codice sorgente del progetto in un singolo assembly, che viene inserito nella `/bin` cartella. Visual Studio 2005 inizialmente solo supportati progetti di sito Web, sebbene il modello di progetto di applicazione Web è stato reintrodotto con Service Pack 1. Visual Studio 2008 offre entrambi i modelli di progetto. Visual Web Developer 2005 e 2008 edizioni, tuttavia, supportano solo progetti di siti Web. Usare il modello di progetto di sito Web per la demo in questa serie di esercitazioni. Se si utilizza un'edizione non Express e si desidera utilizzare il [modello di progetto di applicazione Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) , invece, è possibile eseguire questa operazione, ma tenere presente che potrebbero esistere alcune discrepanze tra sullo schermo e i passaggi da eseguire e il le schermate visualizzate e alle istruzioni fornite in queste esercitazioni.


[![Creare un nuovo sistema basato su sito Web di File](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Figura 04**: creare un sito Web New File System-Based ([fare clic per visualizzare l'immagine ingrandita](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


Successivamente, aggiungere una pagina master per il sito nella directory radice facendo clic sul nome del progetto, scegliere Aggiungi nuovo elemento e selezionando il modello di pagina Master. Si noti che le pagine master deve terminare con l'estensione `.master`. Denominare questa nuova pagina master `Site.master` e fare clic su Aggiungi.


[![Aggiungere una pagina Master denominata Site. master per il sito Web](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Figura 05**: aggiungere un nome di pagina Master `Site.master` al sito Web ([fare clic per visualizzare l'immagine ingrandita](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


Aggiunta di un nuovo file pagina master tramite Visual Web Developer crea una pagina master con markup dichiarativo seguente:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

È la prima riga nel markup dichiarativo di [ `@Master` direttiva](https://msdn.microsoft.com/library/ms228176.aspx). Il `@Master` è simile alla direttiva di [ `@Page` direttiva](https://msdn.microsoft.com/library/ydy4x04a.aspx) che viene visualizzato nelle pagine ASP.NET. Definisce il linguaggio sul lato server (VB) e le informazioni relative alla posizione e l'ereditarietà di classe code-behind della pagina master.

Il `DOCTYPE` e markup dichiarativo della pagina verrà visualizzata sotto il `@Master` direttiva. La pagina include HTML statico con quattro controlli sul lato server:

- **Un Web Form (il `<form runat="server">`)** , perché tutte le pagine ASP.NET in genere presentano una forma di Web - e perché la pagina master può includere i controlli Web che devono figurare all'interno di un Web Form, assicurarsi di aggiungere il Web Form per la pagina master (anziché aggiungere un Web Form e abbia la priorità contenuto pagina).
- **Un controllo ContentPlaceHolder denominato `ContentPlaceHolder1`**  -questo controllo ContentPlaceHolder viene visualizzato all'interno del Form Web e viene utilizzata come area per l'interfaccia utente della pagina di contenuto.
- **Un server-side `<head>` elemento** : il `<head>` elemento ha il `runat="server"` attributo, rendendo accessibile tramite codice lato server. Il `<head>` elemento viene implementato in questo modo, in modo che il titolo della pagina e altri `<head>`-correlati di markup può essere aggiunto o modificato a livello di codice. Ad esempio, l'impostazione della pagina ASP.NET `Title` le modifiche alle proprietà di `<title>` il rendering dell'elemento dal `<head>` controllo server.
- **Un controllo ContentPlaceHolder denominato `head`**  -questo controllo ContentPlaceHolder viene visualizzato all'interno di `<head>` server di controllo e può essere utilizzato in modo dichiarativo aggiungere contenuto al `<head>` elemento.

In questo markup dichiarativo di pagina master predefinita funge da punto di partenza per la progettazione di pagine master. È possibile modificare il codice HTML o per aggiungere altri controlli Web o gli elementi ContentPlaceHolder della pagina master.

> [!NOTE]
> Quando si progetta una pagina master per verificare che la pagina master contiene un Web Form e che almeno un controllo ContentPlaceHolder viene visualizzato all'interno di questo Web Form.


### <a name="creating-a-simple-site-layout"></a>Creazione di un Layout semplice del sito

Consente di espandere `Site.master`del markup dichiarativo predefinito per creare un layout del sito in cui tutte le pagine condividono: un'intestazione comune, una colonna a sinistra con navigazione, notizie e altro contenuto; a livello di sito e un piè di pagina che viene visualizzata l'icona "Attivato da Microsoft ASP.NET". Figura 6 mostra il risultato finale della pagina master quando una delle pagine contenute viene visualizzata tramite un browser. L'area in un cerchio rosso nella figura 6 è specifico per la pagina visitata (`Default.aspx`); altro contenuto è definito nella pagina master e pertanto coerente in tutte le pagine di contenuto.


[![La pagina Master definisce il Markup per la parte superiore sinistra e inferiore](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Figura 06**: la pagina Master definisce il Markup per la parte superiore sinistra e inferiore ([fare clic per visualizzare l'immagine ingrandita](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))


Per ottenere il layout del sito illustrato nella figura 6, iniziare aggiornando il `Site.master` pagina master, in modo che contenga il seguente markup dichiarativo:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

Layout della pagina master viene definito mediante una serie di `<div>` elementi HTML. Il `topContent` `<div>` contiene il markup che viene visualizzato nella parte superiore di ogni pagina, mentre il `mainContent`, `leftContent`, e `footerContent` `<div>` s vengono utilizzati per visualizzare il contenuto della pagina, la colonna a sinistra e "attivato da Microsoft Icona ASP.NET", rispettivamente. Oltre ad aggiungere queste `<div>` elementi, rinominato anche il `ID` proprietà del controllo ContentPlaceHolder primario da `ContentPlaceHolder1` a `MainContent`.

Le regole di formattazione e layout per queste diverse `<div>` elementi viene dichiarato nel [foglio di stile CSS (Cascading)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) file `Styles.css`, che viene specificato mediante un `<link>` elemento della pagina master `<head>`elemento. Queste diverse regole definiscono l'aspetto di ogni `<div>` elemento indicato in precedenza. Ad esempio, il `topContent` `<div>` elemento, che consente di visualizzare il testo "Esercitazioni su pagine Master" e il collegamento, dispone di proprie regole di formattazione specificate `Styles.css` come indicato di seguito:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Se si segue il computer, sarà necessario scaricare codice associato dell'esercitazione e aggiungere il `Styles.css` file al progetto. Analogamente, è inoltre necessario creare una cartella denominata `Images` e copiare l'icona "Attivato da Microsoft ASP.NET" dal sito Web demo scaricato al progetto.

> [!NOTE]
> Una discussione di CSS e la formattazione della pagina web non rientra nell'ambito di questo articolo. Per ulteriori informazioni su CSS, consultare il [CSS esercitazioni](http://www.w3schools.com/css/default.asp) in [W3Schools.com](http://www.w3schools.com/). Si consiglia di scaricare codice associato dell'esercitazione e riprodurre con le impostazioni di CSS in anche `Styles.css` per osservare gli effetti delle diverse regole di formattazione.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Creazione di una pagina Master utilizzando un modello di progettazione esistente

Nel corso degli anni ho creato un numero di applicazioni web ASP.NET per le aziende di piccole e medie. Alcuni client ha un layout del sito esistente che volevano usare; altri assunto una finestra di progettazione grafica competente. Alcuni affidati me per progettare il layout del sito Web. Come si può vedere dalla figura 6, tasking del programmatore per progettare il layout di un sito Web è in genere come buona norma come se avessero il contabile eseguire open-heart surgery mentre il tuo medico non le imposte.

Per fortuna, esistono innumerous siti Web che offrono modelli di progettazione HTML gratuiti - Google restituito più di sei milioni di risultati per il termine di ricerca "modelli di siti Web disponibile". Uno di quelli personali Preferiti è [OpenDesigns.org](http://opendesigns.org/). Dopo aver individuato un modello di sito Web che si desidera, aggiungere il file CSS e le immagini al progetto di sito Web e integrazione HTML del modello nella pagina master.

> [!NOTE]
> Microsoft offre inoltre una serie di [ASP.NET progettazione avvia Kit di modelli gratuiti](https://msdn.microsoft.com/asp.net/aa336613.aspx) che integrare la finestra di dialogo Nuovo sito Web in Visual Studio.


## <a name="step-2-creating-associated-content-pages"></a>Passaggio 2: Creazione associata pagine di contenuto

Con la pagina master creata, pronti a iniziare a creare pagine ASP.NET che sono associate a una pagina master. Tali pagine sono dette *il contenuto di pagine*.

Consente di aggiungere una nuova pagina ASP.NET al progetto e associarlo al `Site.master` pagina master. Fare clic sul nome del progetto in Esplora soluzioni e scegliere l'opzione Aggiungi nuovo elemento. Selezionare il modello Web Form, immettere il nome `About.aspx`, quindi selezionare la casella di controllo "Seleziona pagina master", come illustrato nella figura 7. In questo modo, l'istruzione Select verrà visualizzata una finestra di dialogo pagina Master casella (vedere la figura 8) da cui è possibile scegliere la pagina master da utilizzare.

> [!NOTE]
> Se è stato creato il sito Web ASP.NET utilizzando il modello di progetto di applicazione Web anziché il modello di progetto di sito Web non si vedranno la casella di controllo "Seleziona pagina master" nella finestra di dialogo Aggiungi nuovo elemento illustrato nella figura 7. Per creare un contenuto pagina quando si utilizza il progetto di applicazione Web del modello è necessario scegliere il modello di contenuto Web Form anziché il modello Web Form. Dopo aver selezionato il modello di modulo di contenuto Web e fare clic su Aggiungi, lo stesso Seleziona pagina Master che verrà visualizzata la finestra di dialogo illustrata nella figura 8.


[![Aggiungere una nuova pagina contenuta](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Figura 07**: aggiungere una nuova pagina contenuto ([fare clic per visualizzare l'immagine ingrandita](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![Selezionare la pagina Master Site. master](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Figura 08**: selezionare il `Site.master` pagina Master ([fare clic per visualizzare l'immagine ingrandita](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


Come mostra il markup dichiarativo seguente, una nuova pagina di contenuto contiene un `@Page` direttiva che punta al relativo schema pagina e un controllo contenuto per ognuno dei controlli ContentPlaceHolder della pagina master.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> Nella sezione "Creazione di un semplice sito Layout" nel passaggio 1 sono stati rinominati `ContentPlaceHolder1` a `MainContent`. Se non è stato rinominato del controllo ContentPlaceHolder `ID` nello stesso modo, markup dichiarativo della pagina contenuto sarà leggermente diversa dal markup illustrato in precedenza. In particolare, il secondo del controllo del contenuto `ContentPlaceHolderID` rifletterà il `ID` del ContentPlaceHolder corrispondente controllare nella pagina master.


Quando si esegue il rendering di una pagina di contenuto, il motore ASP.NET è necessario associare il la pagina contenuto controlli con controlli ContentPlaceHolder della pagina master. Il motore ASP.NET determina il contenuto della pagina master dal `@Page` della direttiva `MasterPageFile` attributo. Come mostra il codice precedente, questa pagina di contenuto è associata a `~/Site.master`.

Poiché la pagina master contiene due controlli ContentPlaceHolder - `head` e `MainContent` -Visual Web Developer generati due controlli contenuto. Ogni controllo contenuto fa riferimento a un particolare ContentPlaceHolder tramite il relativo `ContentPlaceHolderID` proprietà.

Pagine master lucentezza tramite tecniche a livello di sito modello precedente dove è con il relativo supporto in fase di progettazione. Figura 9 è illustrato il `About.aspx` pagina contenuto quando è visualizzato in visualizzazione progettazione del Visual Web Developer. Si noti che mentre il contenuto della pagina master è visibile, non è disponibile e non può essere modificato. I controlli del contenuto ContentPlaceHolder della pagina master corrispondente sono, tuttavia, modificabili. E, come con qualsiasi altra pagina ASP.NET, è possibile creare il contenuto dell'interfaccia pagina aggiungendo i controlli Web tramite le viste di origine o di progettazione.


[![Visualizzazione di progettazione della pagina di contenuto vengono visualizzati sia il contenuto pagina Master e specifici della pagina.](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Figura 09**: il contenuto della pagina Progettazione Vista consente di visualizzare sia le specifiche delle pagine e contenuto della pagina Master ([fare clic per visualizzare l'immagine ingrandita](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Aggiunta di controlli Web e Markup alla pagina di contenuto

È opportuno creare contenuto per il `About.aspx` pagina. Come si vede nella figura 10, immettere un titolo "Informazioni sull'autore" e un paio di paragrafi di testo, ma è possibile aggiungere controlli Web, troppo. Dopo aver creato questa interfaccia, visitare il `About.aspx` pagina tramite un browser.


[![Visitare la pagina About attraverso un Browser](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Figura 10**: visitare il `About.aspx` pagina tramite un Browser ([fare clic per visualizzare l'immagine ingrandita](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


È importante tenere presente che la pagina richiesta di contenuto e la pagina master associata sono fusibile e sottoposto a rendering come un intero interamente sul server web. Il browser dell'utente finale viene quindi inviato il codice HTML risultante, fusibili. Per verificare questa condizione, consente di visualizzare il codice HTML ricevuto dal browser passare al menu Visualizza e scegliendo di origine. Si noti che non esistono alcun frame o qualsiasi altre tecniche specializzate per la visualizzazione delle pagine web diverse in un'unica finestra.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Associazione di una pagina Master a una pagina ASP.NET esistente

Come è stato illustrato in questo passaggio, l'aggiunta di che una nuova pagina contenuta per un'applicazione web ASP.NET è semplice come "Seleziona pagina master" la casella di controllo e il prelievo della pagina master. Sfortunatamente, la conversione di una pagina ASP.NET esistente a una pagina master non è più semplice.

Per associare una pagina master a una pagina ASP.NET esistente è necessario eseguire la procedura seguente:

1. Aggiungere il `MasterPageFile` attributo della pagina ASP.NET `@Page` direttiva, di modo che punti alla pagina principale appropriata.
2. Aggiungere controlli contenuto per ogni ContentPlaceHolder nella pagina master.
3. Tagliare e incollare il contenuto della pagina ASP.NET esistente nei controlli del contenuto appropriati in modo selettivo. Ad esempio "in modo selettivo" qui perché ASP.NET pagina probabilmente contiene markup che è già espressa dalla pagina master, ad esempio il `DOCTYPE`, il `<html>` elemento e il Web Form.

Per istruzioni dettagliate su questo processo insieme catture di schermata, estrarre [Scott Guthrie](https://weblogs.asp.net/scottgu/)del [utilizzo delle pagine Master e esplorazione del sito](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) esercitazione. Il "aggiornamento `Default.aspx` e `DataSample.aspx` per utilizzare la pagina Master" sezione descrive questi passaggi.

Poiché è molto più semplice creare nuove pagine di contenuto anziché per convertire le pagine ASP.NET esistenti in pagine di contenuto, è consigliabile che ogni volta che crea un nuovo sito Web ASP.NET aggiungere una pagina master per il sito. Associare tutte le nuove pagine ASP.NET per la pagina master. Non occorre preoccuparsi se la pagina master iniziale è molto semplice o normale; è possibile aggiornare la pagina master in un secondo momento.

> [!NOTE]
> Quando si crea una nuova applicazione ASP.NET, Visual Web Developer viene aggiunta una `Default.aspx` pagina che non è associato a una pagina master. Se si desidera pratica la conversione di una pagina ASP.NET esistente in una pagina di contenuto, proseguire e farlo con `Default.aspx`. In alternativa, è possibile eliminare `Default.aspx` e quindi aggiungere di nuovo, ma questo momento la casella di controllo "Seleziona pagina master".


## <a name="step-3-updating-the-master-pages-markup"></a>Passaggio 3: Aggiornamento Markup della pagina Master

Uno dei principali vantaggi delle pagine master è una singola pagina master può essere utilizzata per definire il layout generale per numerose pagine nel sito. Pertanto, l'aspetto del sito di aggiornamento richiede l'aggiornamento di un singolo file - la pagina master.

Per illustrare questo comportamento, consente di aggiornare la pagina master per includere la data corrente nella parte superiore della colonna a sinistra. Aggiungere un'etichetta denominata `DateDisplay` per il `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

Creare quindi un `Page_Load` gestore eventi per il master per la pagina e aggiungere il codice seguente:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

Questo codice imposta l'etichetta `Text` proprietà per la data e ora correnti formattato come il giorno della settimana, il nome del mese e giorno a due cifre (vedere Figura 11). Con questa modifica, rivedere una delle pagine. Come mostrato nella figura 11, il tag risulta viene immediatamente aggiornato per includere la modifica della pagina master.


[![Le modifiche alla pagina Master vengono applicate quando si apre la pagina contenuto](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Figura 11**: di modifiche alla pagina Master vengono applicate quando si apre la pagina contenuto ([fare clic per visualizzare l'immagine ingrandita](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> Come illustrato in questo esempio, pagine master possono contenere controlli Web lato server, codice e i gestori di eventi.


## <a name="summary"></a>Riepilogo

Pagine master consentono agli sviluppatori ASP.NET progettare un layout coerenza a livello di sito facilmente aggiornabile. Creazione di pagine master e le relative pagine di contenuto associate è semplice come la creazione di pagine ASP.NET standard, come Visual Web Developer offre supporto avanzato in fase di progettazione.

L'esempio di pagina master creati in questa esercitazione contiene due controlli ContentPlaceHolder, head e MainContent. È specificato solo il markup per il controllo MainContent ContentPlaceHolder nella pagina contenuta, tuttavia. Nella prossima esercitazione verranno esaminate tramite il contenuto di più controlli nella pagina di contenuto. È inoltre possibile vedere come definire l'impostazione predefinita il markup per il contenuto controlli all'interno della pagina master, nonché come definito per alternare utilizzando il valore predefinito nello schema markup pagina e fornire di markup personalizzata della pagina di contenuto.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [ASP.NET per le finestre di progettazione: liberare i modelli di progettazione e informazioni aggiuntive sulla compilazione di siti Web ASP.NET utilizzando gli standard Web](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Cenni preliminari sulle pagine Master ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Propagazione esercitazioni di fogli di stile (CSS)](http://www.w3schools.com/css/default.asp)
- [Impostazione in modo dinamico il titolo della pagina](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Pagine master in ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Esercitazioni delle Guide rapide delle pagine master](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft fin dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 3.5 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott può essere raggiunto al [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](nested-master-pages-cs.md)
> [Successivo](multiple-contentplaceholders-and-default-content-vb.md)
