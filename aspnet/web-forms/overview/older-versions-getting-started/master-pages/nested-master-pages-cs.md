---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
title: Annidato pagine Master (c#) | Documenti Microsoft
author: rick-anderson
description: Viene illustrato come annidare una pagina master all'interno di altra.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 32b7fb6e-d74b-4048-91f8-70631b2523ee
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: c9497659e0b8ff8164f122e6e3cb382ac0355a32
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="nested-master-pages-c"></a>Pagine Master annidate (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.pdf)

> Viene illustrato come annidare una pagina master all'interno di altra.


## <a name="introduction"></a>Introduzione

Nel corso delle nove esercitazioni precedenti abbiamo visto come implementare un layout a livello di sito con pagine master. In breve, pagine master consentono, lo sviluppatore della pagina, per definire comuni markup nella pagina insieme aree specifiche che possono essere personalizzati in base a una pagina di contenuto dal contenuto della pagina master. I controlli ContentPlaceHolder in una pagina master indicano le aree personalizzabili; tag personalizzati per i controlli ContentPlaceHolder sono definiti nella pagina di contenuto tramite i controlli del contenuto.

Le tecniche di pagina master che abbiamo esplorato finora sono ideali se si dispone di un singolo layout utilizzato in tutto il sito. Molti siti Web di grandi dimensioni, tuttavia, presentano un layout di sito personalizzato in diverse sezioni. Si consideri ad esempio un'applicazione sanitaria usata dal personale ospedale per gestire la fatturazione, attività e le informazioni sui pazienti. Potrebbero essere presenti tre tipi di pagine web in questa applicazione:

- Pagine specifiche di membro del personale in cui i membri del personale possono aggiornare la disponibilità, visualizzare le pianificazioni o richiesta di ferie.
- Pagine di paziente specifiche in cui i membri del personale consente di visualizzare o modificare le informazioni per un paziente specifico.
- Pagine di fatturazione specifiche in cui esaminare corrente contabili attestazione gli Stati e report finanziari.

Tutte le pagine potrebbero condividere un layout comune, ad esempio un menu in alto e una serie di collegamenti usati di frequente nella parte inferiore. Tuttavia, il personale, paziente e pagine specifiche di fatturazione potrebbe essere necessario personalizzare il layout generico. Ad esempio, ad esempio tutte le pagine specifiche del personale devono includere un elenco di calendario e attività che mostra la disponibilità dell'utente attualmente connesso e pianificazione giornaliera. Tutte le pagine specifiche paziente eventualmente necessario visualizzare il nome, indirizzo e l'assicurazione informazioni relative al paziente le cui informazioni viene modificate.

È possibile creare questo tipo di layout personalizzato tramite *pagine master annidate*. Per implementare lo scenario precedente, si inizierebbe creando una pagina master che è definito il contenuto di layout, i menu e i piè di pagina a livello di sito, con gli elementi ContentPlaceHolder definire le aree personalizzabili. È quindi necessario creare tre pagine di master annidate, uno per ogni tipo di pagina web. Ogni pagina master annidata è possibile definire il contenuto tra il tipo di contenuto delle pagine che utilizzano la pagina master. In altre parole, la pagina master annidata per le pagine specifiche paziente includerebbe markup e logica a livello di codice per la visualizzazione delle informazioni relative al paziente in fase di modifica. Quando si crea una nuova pagina paziente specifici, è necessario associarlo a questa pagina master annidata.

In questa esercitazione viene avviato per illustrare i vantaggi delle pagine master annidate. Viene quindi illustrato come creare e utilizzare pagine master annidate.

> [!NOTE]
> Pagine master annidate sono stati possibili a partire dalla versione 2.0 di .NET Framework. Tuttavia, Visual Studio 2005 non includono il supporto in fase di progettazione per le pagine master annidate. Buone notizie sono che Visual Studio 2008 offre esperienze in fase di progettazione per le pagine master annidate. Se si desidera utilizzare pagine master annidate, ma utilizzano ancora Visual Studio 2005, consultare [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog, [suggerimenti per le pagine Master annidate in fase di progettazione di Visual Studio 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>I vantaggi di pagine Master annidate

Molti siti Web hanno una concetto la struttura del sito nonché più personalizzate progettazioni specifiche di determinati tipi di pagine. Ad esempio, nell'applicazione web demo è stata creata una sezione Amministrazione rudimentale (le pagine di `~/Admin` cartella). Attualmente le pagine web di `~/Admin` cartella Usa la stessa pagina master come tali pagine non nella sezione di amministrazione (vale a dire, `Site.master` o `Alternate.master`, a seconda della selezione dell'utente).

> [!NOTE]
> Per il momento, si supponga che il sito ha una sola pagina master, `Site.master`. Prenderemo utilizzando le pagine master annidate con pagine master due (o più) a partire da "Utilizzando un nidificata Master per l'amministrazione sezione della pagina" più avanti in questa esercitazione.


Si supponga che si stava richiesto per personalizzare il layout delle pagine per includere informazioni aggiuntive o i collegamenti che altrimenti non sarebbe presenti in altre pagine nel sito di amministrazione. Sono disponibili quattro tecniche per implementare questo requisito:

1. Aggiungere manualmente le informazioni specifiche di amministrazione e i collegamenti a ogni pagina di contenuto di `~/Admin` cartella.
2. Aggiornamento di `Site.master` pagina master per includere le informazioni specifiche di sezione di amministrazione e i collegamenti e quindi aggiungere codice della pagina master per mostrare o nascondere queste sezioni in base a una delle pagine di amministrazione se viene visitata.
3. Creare una nuova pagina master in modo specifico per la sezione di amministrazione, copiare il markup di `Site.master`, aggiungere le informazioni specifiche di sezione di amministrazione e i collegamenti e quindi aggiornare le pagine di contenuto di `~/Admin` cartella da utilizzare il nuovo master pagina.
4. Creare una pagina master annidata che associa `Site.master` e dispone di pagine di contenuto nel `~/Admin` utilizzo cartella questa nuova nidificate pagina master. Questa pagina master annidata includono solo le informazioni aggiuntive e i collegamenti specifici per le pagine di amministrazione e la necessità di ripetere il markup già definito in `Site.master`.

La prima opzione è la meno appetibile. Lo scopo di utilizzo delle pagine master consiste nello spostare dalla necessità di copiare e incollare il markup comune per le nuove pagine ASP.NET manualmente. La seconda opzione è accettabile, ma rende meno gestibili come ausiliari le pagine master con il tag viene visualizzato solo occasionalmente e richiede la modifica della pagina master per aggirare questo markup e ricordare, l'applicazione esattamente, determinati tag viene visualizzato e nascosto. Questo approccio potrebbe essere meno tenable come personalizzazioni da più tipi di pagine web deve essere gestite da questa pagina master.

La terza opzione rimuove il disordine e complessità i problemi di esposto con la seconda opzione. Tuttavia, svantaggio principale opzione del tre è che richiede la presenza di copiare e incollare il layout comune da `Site.master` nella nuova pagina master specifica sezione di amministrazione. Se si decide successivamente di modificare il layout a livello di sito è necessario ricordarsi di modificare le impostazioni in due posizioni.

La quarta opzione, pagine master nidificate, offrono il meglio delle opzioni di secondo e terzo. Le informazioni di layout a livello di sito verranno mantenute in un unico file - pagina master - mentre il contenuto specifico per le aree particolare separato in file diversi.

In questa esercitazione inizia con un'occhiata a creazione e utilizzo di una semplice pagina master annidata. Si crea una nuova pagina master livello superiore, due pagine master annidate e due pagine di contenuto. A partire da "Utilizzando un annidati Master per l'amministrazione sezione della pagina", si esamina l'architettura di pagina master esistente per includere l'utilizzo di pagine master annidate di aggiornamento. In particolare, si crea una pagina master annidata e usarlo per includere il contenuto personalizzato aggiuntivo per le pagine di contenuto di `~/Admin` cartella.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Passaggio 1: Creazione di una semplice pagina Master di primo livello

Creazione di un master annidate basato su uno dei master esistente pagine e quindi l'aggiornamento di una pagina di contenuto esistente per l'utilizzo di questa nuova pagina master annidata anziché la pagina master principale comporta un livello di complessità perché già prevedono alcune pagine di contenuto esistente Controlli ContentPlaceHolder definiti nella pagina master. Pertanto, la pagina master annidata deve anche includere gli stessi controlli ContentPlaceHolder con gli stessi nomi. Inoltre, l'applicazione demo particolare ha due pagine master (`Site.master` e `Alternate.master`) che vengono assegnate dinamicamente a una pagina contenuto in base alle preferenze dell'utente, che aggiunge a questa complessità. Verranno esaminati l'aggiornamento dell'applicazione esistente da utilizzare pagine master annidate più avanti in questa esercitazione, ma si prima dello stato attivo su un semplice annidate esempio pagine master.

Creare una nuova cartella denominata `NestedMasterPages` e quindi aggiungere un nuovo file pagina master in tale cartella denominata `Simple.master`. (Vedere Figura 1 per una schermata di Esplora soluzioni dopo l'aggiunta di questa cartella e file). Trascinare il `AlternateStyles.css` file da Esplora soluzioni nella finestra di progettazione del foglio di stile. Aggiunge un `<link>` elemento per il file del foglio di stile nel `<head>` elemento, dopo il quale la pagina master `<head>` markup dell'elemento dovrebbe essere simile:


[!code-aspx[Main](nested-master-pages-cs/samples/sample1.aspx)]

Successivamente, aggiungere il markup seguente all'interno del modulo Web di `Simple.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample2.aspx)]

Questo codice consente di visualizzare un collegamento denominato "Pagine Master annidate (semplice)" nella parte superiore della pagina in caratteri grandi bianco su sfondo blu. Sotto questo è il `MainContent` ContentPlaceHolder. Figura 1 mostra la `Simple.master` pagina master quando caricato nella progettazione di Visual Studio.


[![La pagina Master annidata definisce contenuto specifico per le pagine nella sezione Amministrazione](nested-master-pages-cs/_static/image2.png)](nested-master-pages-cs/_static/image1.png)

**Figura 01**: il annidati Master pagina definisce contenuto specifico per le pagine nella sezione Amministrazione ([fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Passaggio 2: Creazione di una pagina Master annidata semplice

`Simple.master` contiene due controlli ContentPlaceHolder: il `MainContent` ContentPlaceHolder è aggiunto all'interno del Form Web insieme al `head` ContentPlaceHolder nel `<head>` elemento. Se si crea una pagina contenuta e associarlo al `Simple.master` pagina contenuto avrebbe due controlli contenuto che fa riferimento il due ContentPlaceHolder. Analogamente, se si crea una pagina master annidata e associarlo al `Simple.master` la pagina master annidata avrà due controlli contenuto.

Aggiungere una nuova pagina master annidata per il `NestedMasterPages` cartella denominata `SimpleNested.master`. Fare clic su di `NestedMasterPages` cartella e scegliere Aggiungi nuovo elemento. Verrà visualizzata la finestra di dialogo Aggiungi nuovo elemento mostrata nella figura 2. Selezionare il tipo di modello di pagina Master e digitare il nome della nuova pagina master. Per indicare che la nuova pagina master deve essere una pagina master annidata, selezionare la casella di controllo "Seleziona pagina master".

Successivamente, fare clic sul pulsante Aggiungi. L'istruzione Select stesso verrà visualizzata una finestra di dialogo pagina Master, viene visualizzato quando l'associazione di una pagina di contenuto a una pagina master (vedere la figura 3). Scegliere il `Simple.master` pagina master nel `NestedMasterPages` cartella e fare clic su OK.

> [!NOTE]
> Se è stato creato il sito Web ASP.NET utilizzando il modello di progetto di applicazione Web anziché il modello di progetto di sito Web non si vedranno la casella di controllo "Seleziona pagina master" nella finestra di dialogo Aggiungi nuovo elemento mostrato nella figura 2. Per creare una pagina master annidata quando si utilizza il modello di progetto di applicazione Web è necessario scegliere il modello di pagina Master annidata (anziché il modello di pagina Master). Dopo aver selezionato il modello annidato pagina Master e fare clic su Aggiungi, lo stesso Seleziona pagina Master che verrà visualizzata la finestra di dialogo illustrata nella figura 3.


[![Controllare la &quot;Seleziona pagina master&quot; casella di controllo per aggiungere una pagina Master annidate](nested-master-pages-cs/_static/image5.png)](nested-master-pages-cs/_static/image4.png)

**Figura 02**: selezionare la casella di controllo "Seleziona pagina master" per aggiungere una pagina Master annidate ([fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image6.png))


[![Associare la pagina Master annidata alla pagina Master Simple.master](nested-master-pages-cs/_static/image8.png)](nested-master-pages-cs/_static/image7.png)

**Figura 03**: pagina Master annidate per associare il `Simple.master` pagina Master ([fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image9.png))


Markup dichiarativo della pagina master annidata, illustrato di seguito, contiene due controlli contenuto di riferimento ai controlli ContentPlaceHolder due di livello superiore della pagina master.


[!code-aspx[Main](nested-master-pages-cs/samples/sample3.aspx)]

Fatta eccezione per il `<%@ Master %>` direttiva, markup dichiarativo iniziale della pagina master annidata è identico al markup generato inizialmente durante l'associazione di una pagina di contenuto nella stessa pagina di primo livello master. Ad esempio una pagina contenuto `<%@ Page %>` direttiva, il `<%@ Master %>` qui direttiva include un `MasterPageFile` attributo che specifica una pagina master padre della pagina master annidata. La differenza principale tra la pagina master annidata e una pagina di contenuto associato alla stessa pagina master principale è che la pagina master annidata può contenere controlli ContentPlaceHolder. I controlli della pagina master annidata ContentPlaceHolder definiscono le aree in cui le pagine di contenuto possono personalizzare il markup.

Aggiornare questa pagina master annidata in modo che venga visualizzato il testo "Hello, da SimpleNested!" nel controllo contenuto che corrisponde alla `MainContent` controllo ContentPlaceHolder.


[!code-aspx[Main](nested-master-pages-cs/samples/sample4.aspx)]

Dopo aver apportato questa aggiunta, salvare la pagina master annidata e quindi aggiungere una nuova pagina contenuta per il `NestedMasterPages` cartella denominata `Default.aspx`e associarlo al `SimpleNested.master` pagina master. Dopo l'aggiunta di questa pagina potrebbe essere strano che non contiene alcun controllo contenuto (vedere la figura 4). Una pagina di contenuto può accedere solo ai relativi *padre* gli elementi ContentPlaceHolder della pagina master. `SimpleNested.master` non contiene tutti i controlli ContentPlaceHolder; Pertanto, qualsiasi pagina di contenuto associato a questa pagina master non può contenere tutti i controlli contenuti.


[![La nuova pagina di contenuto non contiene contenuti controlli](nested-master-pages-cs/_static/image11.png)](nested-master-pages-cs/_static/image10.png)

**Figura 04**: il nuovo contenuto pagina contiene No controlli contenuto ([fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image12.png))


È necessario eseguire è aggiornare la pagina master annidata (`SimpleNested.master`) per includere i controlli ContentPlaceHolder. In genere è opportuno che le pagine master annidate per includere un ContentPlaceHolder per ogni ContentPlaceHolder definiti dalla relativa pagina master padre, consentendo la pagina master figlio o una pagina contenuto e utilizzare uno qualsiasi di ContentPlaceHolder di livello superiore della pagina master controlli.

Aggiornamento di `SimpleNested.master` pagina master per includere un ContentPlaceHolder nei due controlli contenuti. Assegnare i controlli ContentPlaceHolder lo stesso nome del controllo ContentPlaceHolder che fa riferimento il controllo contenuto. In altre parole, aggiungere un controllo ContentPlaceHolder denominato `MainContent` al contenuto del controllo `SimpleNested.master` che fa riferimento il `MainContent` ContentPlaceHolder in `Simple.master`. Eseguire la stessa operazione nel controllo contenuto che fa riferimento il `head` ContentPlaceHolder.

> [!NOTE]
> Si consiglia di assegnare i controlli ContentPlaceHolder nella pagina master nidificata lo stesso come ContentPlaceHolder nella pagina master di primo livello, la denominazione simmetria non è più necessario. È possibile assegnare ai controlli ContentPlaceHolder nella pagina master nidificata qualsiasi nome desiderato. Tuttavia, facile ricordare cosa ContentPlaceHolder corrispondono ai quali aree della pagina, se il primo livello pagina master e pagine master annidate utilizzano gli stessi nomi.


Dopo aver apportato queste aggiunte le `SimpleNested.master` markup dichiarativo della pagina master dovrebbe essere simile al seguente:


[!code-aspx[Main](nested-master-pages-cs/samples/sample5.aspx)]

Eliminare il `Default.aspx` il contenuto pagina appena creato e quindi aggiungerlo nuovamente, associarlo al `SimpleNested.master` pagina master. Questa volta Visual Studio aggiunge due controlli del contenuto per il `Default.aspx`, che fa riferimento il ContentPlaceHolder ora definita in `SimpleNested.master` (vedere Figura 6). Aggiungere il testo "Hello, da Default.aspx!" nel contenuto del controllo a cui fa riferimento `MainContent`.

La figura 5 mostra tre entità coinvolte, `Simple.master`, `SimpleNested.master`, e `Default.aspx` - e come interagiscono tra loro. Come illustrato nel diagramma, la pagina master annidata implementa i controlli del contenuto per ContentPlaceHolder del padre. Se queste aree devono poter accedere alla pagina di contenuto, la pagina master annidata è necessario aggiungere il proprio ContentPlaceHolder ai controlli del contenuto.


[![Le pagine Master di primo livello e nidificati, definiscono il contenuto del Layout della pagina](nested-master-pages-cs/_static/image14.png)](nested-master-pages-cs/_static/image13.png)

**Figura 05**: le pagine Master di primo livello e nidificati, definiscono il contenuto del Layout della pagina ([fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image15.png))


Questo comportamento viene illustrato come una pagina contenuto o una pagina master è solo consapevoli della pagina master padre. Questo comportamento è indicato anche dalla progettazione di Visual Studio. La figura 6 mostra la finestra di progettazione `Default.aspx`. Mentre la finestra di progettazione mostra chiaramente le aree sono modificabili della pagina di contenuto e quali non sono parti, non risolvere le ambiguità nei quali sono le aree non modificabili della pagina master nidificata e quali sono le aree della pagina master di primo livello.


[![Il contenuto di pagina ora include controlli contenuto per gli elementi ContentPlaceHolder della pagina Master annidata](nested-master-pages-cs/_static/image17.png)](nested-master-pages-cs/_static/image16.png)

**Figura 06**: il contenuto pagina ora include controlli contenuto per gli elementi ContentPlaceHolder della pagina Master annidate ([fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Passaggio 3: Aggiunta di una seconda pagina Master nidificata semplice

Il vantaggio di pagine master annidate è più evidente quando sono presenti più pagine master annidate. Per illustrare questo vantaggio, creare un'altra pagina master annidata nel `NestedMasterPages` pagina master annidate questo nuovo nome della cartella; `SimpleNestedAlternate.master` e associarlo al `Simple.master` pagina master. Aggiungere controlli ContentPlaceHolder in due controlli contenuto della pagina master annidata, come abbiamo visto nel passaggio 2. Aggiungere anche il testo "Hello, da SimpleNestedAlternate!" nel controllo contenuto che corrisponde al primo livello pagina master `MainContent` ContentPlaceHolder. Dopo aver apportato queste modifiche dichiarativo per della nuova pagina master annidata dovrebbe essere simile al seguente:


[!code-aspx[Main](nested-master-pages-cs/samples/sample6.aspx)]

Creare una pagina contenuta denominata `Alternate.aspx` nel `NestedMasterPages` cartella e associarlo al `SimpleNestedAlternate.master` pagina master annidata. Aggiungere il testo "Hello, da alternativo!" nel controllo contenuto che corrisponde a `MainContent`. Figura 7 illustra `Alternate.aspx` quando viene visualizzato tramite la progettazione di Visual Studio.


[![Alternate.aspx è associato alla pagina Master SimpleNestedAlternate.master](nested-master-pages-cs/_static/image20.png)](nested-master-pages-cs/_static/image19.png)

**Figura 07**: `Alternate.aspx` è associato il `SimpleNestedAlternate.master` pagina Master ([fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image21.png))


Confrontare la finestra di progettazione nella figura 7 nella finestra di progettazione nella figura 6. Entrambe le pagine di contenuto condividono lo stesso layout definite nella pagina master principale (`Simple.master`), vale a dire il titolo "Nested Master pagine esercitazione (semplice)". Sia ancora contenuto distinto definito nelle pagine master padre, il testo "Hello, da SimpleNested!" Nella figura 6 e "Hello, da SimpleNestedAlternate!" Nella figura 7. Concesse, qui queste differenze sono semplici, ma è possibile estendere questo esempio per includere le differenze più significative. Ad esempio, il `SimpleNested.master` pagina può includere un menu con le opzioni specifiche per le pagine di contenuto, mentre `SimpleNestedAlternate.master` potrebbe contenere informazioni pertinenti per le pagine di contenuto a esso associati.

Si supponga ora che è necessario apportare una modifica al layout generale del sito. Ad esempio, si supponga di voler aggiungere un elenco di collegamenti comuni a tutte le pagine di contenuto. A tale scopo è aggiornare la pagina master di primo livello, `Simple.master`. Sono tutte le modifiche vengono immediatamente riflessi nel relativo pagine master annidate e, con l'estensione, le pagine di contenuto.

Per dimostrare la facilità con cui è possibile modificare il layout del sito onnicomprensiva, aprire il `Simple.master` pagina master e aggiungere il markup seguente tra i `topContent` e `mainContent` `<div>` elementi:


[!code-aspx[Main](nested-master-pages-cs/samples/sample7.aspx)]

Consente di aggiungere due collegamenti nella parte superiore di ogni pagina che associa `Simple.master`, `SimpleNested.master`, o `SimpleNestedAlternate.master`; le modifiche si applicano a pagine master annidate tutti e le relative pagine contenute immediatamente. La figura 8 mostra `Alternate.aspx` quando viene visualizzato tramite un browser. Si noti l'aggiunta dei collegamenti nella parte superiore della pagina (rispetto alla figura 7).


[![Modificato per la pagina Master di livello superiore vengono immediatamente riflessi nel relativo pagine Master annidate e le pagine di contenuto Their](nested-master-pages-cs/_static/image23.png)](nested-master-pages-cs/_static/image22.png)

**Figura 08**: modificato per la pagina Master di livello superiore vengono immediatamente riflessi nel relativo pagine Master annidate e le pagine di contenuto Their ([fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Utilizzo di una pagina Master annidata per la sezione di amministrazione

A questo punto è siano presenti i vantaggi del master annidate pagine e si è appreso come crearli e utilizzarli in un'applicazione ASP.NET. Negli esempi di passaggi 1, 2 e 3, coinvolti, tuttavia, la creazione di una nuova pagina master di primo livello, nuove pagine master annidate e nuove pagine di contenuto. Cosa succede aggiungendo una nuova pagina master annidata in un sito Web con una pagina di primo livello master esistente e pagine di contenuto?

Integrazione di una pagina master annidata in un sito Web esistente e la relativa associazione alle pagine di contenuto esistente richiede uno sforzo maggiore rispetto a partire da zero. I passaggi 4, 5, 6 e 7 esplorare queste sfide come forniamo applicazione demo per includere una nuova pagina master annidata denominata `AdminNested.master` che contiene le istruzioni per l'amministratore e viene utilizzato per le pagine ASP.NET il `~/Admin` cartella.

L'integrazione di una pagina master annidata nell'applicazione demo sono stati introdotti i problemi legati alla seguenti:

- Il contenuto esistente di pagine nel `~/Admin` cartella hanno alcune aspettative dalla pagina master. Per iniziare, si aspettano determinati controlli ContentPlaceHolder deve essere presente. Inoltre, il `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` pagine chiamare pubblico della pagina master `RefreshRecentProductsGrid` (metodo), impostare il relativo `GridMessageText` proprietà, o dispone di un gestore eventi per relativo `PricesDoubled` evento. Di conseguenza, la pagina master annidata deve fornire gli elementi ContentPlaceHolder stesso e i membri pubblici.
- Nell'esercitazione precedente è stato migliorato il `BasePage` classe per impostare dinamicamente il `Page` dell'oggetto `MasterPageFile` proprietà in base a una variabile di sessione. Come per un sostegno pagine master dinamiche quando si utilizzano pagine master annidate?

Questi due problemi esporrà compilare la pagina master annidata e usarlo dalle pagine di contenuto esistente. Si sarà analizzare e vincono questi problemi quando si verificano.

## <a name="step-4-creating-the-nested-master-page"></a>Passaggio 4: Creazione di una pagina Master nidificata

La prima attività consiste nel creare la pagina master annidata da utilizzare per le pagine nella sezione Amministrazione. Come illustrato nel passaggio 2, quando l'aggiunta di un nuovo annidato pagina master è necessario specificare una pagina master padre della pagina master annidata. Ma sono disponibili due pagine master principale: `Site.master` e `Alternate.master`. Tenere presente che è stato creato `Alternate.master` nell'esercitazione precedente e scritto codice `BasePage` classe imposta l'oggetto di pagina `MasterPageFile` proprietà in fase di esecuzione per una `Site.master` o `Alternate.master` in base al valore il `MyMasterPage` Variabile di sessione.

Come si configura la pagina master annidata in modo che utilizzi la pagina master principale appropriata? Sono disponibili due opzioni:

- Creare due pagine master annidate, `AdminNestedSite.master` e `AdminNestedAlternate.master`e associarle alle pagine master principale `Site.master` e `Alternate.master`, rispettivamente. In `BasePage`, quindi, è necessario impostare il `Page` dell'oggetto `MasterPageFile` alla pagina master nidificata appropriata.
- Creare una singola pagina master annidata e le pagine di contenuto utilizzare questa pagina master. Quindi, in fase di esecuzione, è necessario impostare la pagina master annidata `MasterPageFile` proprietà della pagina master principale appropriata in fase di esecuzione. (Come è possibile che hanno realizzato a questo punto, le pagine master è inoltre disponibile un `MasterPageFile` proprietà.)

Consente di usare la seconda opzione. Creare un file di pagina master a nidificata singolo nel `~/Admin` cartella denominata `AdminNested.master`. Poiché entrambi `Site.master` e `Alternate.master` hanno lo stesso set di controlli ContentPlaceHolder, non è importante indicare la pagina master è necessario associarli a, anche se si consiglia di associarlo al `Site.master` per i migliori risultati della verifica di coerenza.


[![Aggiungere una pagina Master annidata nella cartella ~/Admin.](nested-master-pages-cs/_static/image26.png)](nested-master-pages-cs/_static/image25.png)

**Figura 09**: aggiungere una pagina Master annidata per il `~/Admin` cartella. ([Fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image27.png))


Poiché la pagina master annidata è associata a una pagina master con quattro controlli ContentPlaceHolder, Visual Studio vengono aggiunti quattro controlli al markup dell'elemento iniziale del nuovo file pagina master annidata contenuto. Come abbiamo visto nei passaggi 2 e 3, aggiungere un controllo ContentPlaceHolder in ogni controllo contenuto, assegnargli lo stesso nome controllo ContentPlaceHolder di livello superiore della pagina master. Aggiungere anche il markup seguente per il controllo contenuto che corrisponde alla `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-cs/samples/sample8.html)]

Successivamente, definire il `instructions` della classe CSS nel `Styles.css` e `AlternateStyles.css` file CSS. Le regole CSS seguenti che gli elementi HTML con lo stile di `instructions` classe da visualizzare con il colore di sfondo giallo e un bordo nero a tinta unita:


[!code-css[Main](nested-master-pages-cs/samples/sample9.css)]

Poiché questo markup è stato aggiunto alla pagina master nidificata, verrà visualizzata solo in tali pagine che utilizzano questa pagina master annidata (in particolare, le pagine nella sezione Amministrazione).

Dopo aver apportato queste aggiunte per la pagina master annidata, il markup dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](nested-master-pages-cs/samples/sample10.aspx)]

Notare che ogni controllo contenuto dispone di un controllo ContentPlaceHolder e che i controlli di ContentPlaceHolder `ID` proprietà vengono assegnati gli stessi valori dei controlli ContentPlaceHolder corrispondente nella pagina master di primo livello. Inoltre, è presente il markup specifico della sezione di amministrazione di `MainContent` ContentPlaceHolder.

Figura 10 è illustrata la `AdminNested.master` pagina master annidata quando viene visualizzato tramite Progettazione di Visual Studio. È possibile visualizzare le istruzioni nella casella di colore gialla nella parte superiore del `MainContent` controllo contenuto.


[![La pagina Master annidata estende il primo livello pagina Master per includere le istruzioni per l'amministratore.](nested-master-pages-cs/_static/image29.png)](nested-master-pages-cs/_static/image28.png)

**Figura 10**: la pagina Master annidata estende il primo livello pagina Master per includere le istruzioni per l'amministratore. ([Fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Passaggio 5: Aggiornamento delle pagine di contenuto esistenti per utilizzare la nuova pagina Master nidificata

In qualsiasi momento si aggiunga una nuova pagina contenuta per la sezione di amministrazione, è necessario associarlo al `AdminNested.master` pagina master appena creato. Ma cosa succede esistenti pagine di contenuto? Attualmente, tutte le pagine di contenuto nel sito di derivano il `BasePage` (classe), che a livello di codice imposta il contenuto della pagina master in fase di esecuzione. Questa operazione è il comportamento desiderato per le pagine di contenuto della sezione di amministrazione. Si desidera invece che queste pagine contenute utilizzare sempre il `AdminNested.master` pagina. Sarà la responsabilità della pagina master nidificata per scegliere la pagina di contenuto a destra di primo livello in fase di esecuzione.

Per il modo migliore per ottenere questo desiderata comportamento consiste nel creare una nuova classe di pagina base personalizzata denominata `AdminBasePage` che estende la `BasePage` classe. `AdminBasePage` quindi è possibile eseguire l'override di `SetMasterPageFile` e impostare il `Page` dell'oggetto `MasterPageFile` sul valore hardcoded "~ / Admin/AdminNested.master". In questo modo, qualsiasi pagina che deriva da `AdminBasePage` utilizzerà `AdminNested.master`, mentre qualsiasi pagina che deriva da `BasePage` avrà relativo `MasterPageFile` impostata in modo dinamico su "~ / Site. master" o "~ / Alternate.master" in base al valore del `MyMasterPage` Variabile di sessione.

Per iniziare, aggiungere un nuovo file di classe e il `App_Code` cartella denominata `AdminBasePage.cs`. Avere `AdminBasePage` estendere `BasePage` e quindi eseguire l'override di `SetMasterPageFile` metodo. In tale metodo, assegnare il `MasterPageFile` il valore "~ / Admin/AdminNested.master". Dopo aver apportato queste modifiche classe file dovrebbe essere simile al seguente:


[!code-csharp[Main](nested-master-pages-cs/samples/sample11.cs)]

È ora necessario avere le pagine di contenuto esistente nella gestione di sezione derivano da `AdminBasePage` anziché `BasePage`. Passare per il file di classe code-behind per ogni pagina contenuto la `~/Admin` cartella e apportare questa modifica. Ad esempio, in `~/Admin/Default.aspx` sarebbe stato necessario modificare la dichiarazione di classe di codice da:


[!code-csharp[Main](nested-master-pages-cs/samples/sample12.cs)]

A:


[!code-csharp[Main](nested-master-pages-cs/samples/sample13.cs)]

Figura 11 illustra come il primo livello pagina master (`Site.master` o `Alternate.master`), la pagina master annidata (`AdminNested.master`), e le pagine di contenuto di sezione di amministrazione sono correlati tra loro.


[![La pagina Master annidata definisce contenuto specifico per le pagine nella sezione Amministrazione](nested-master-pages-cs/_static/image32.png)](nested-master-pages-cs/_static/image31.png)

**Figura 11**: il annidati Master pagina definisce contenuto specifico per le pagine nella sezione Amministrazione ([fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Passaggio 6: Mirroring le proprietà e metodi pubblici della pagina Master

Tenere presente che il `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` pagine interagiscono a livello di codice con la pagina master: `~/Admin/AddProduct.aspx` chiamate del pubblica la pagina master `RefreshRecentProductsGrid` (metodo) e imposta il relativo `GridMessageText` proprietà; `~/Admin/Products.aspx` dispone di un gestore eventi per il `PricesDoubled` evento. Nell'esercitazione precedente è stata creata una classe astratta `BaseMasterPage` classe definiti questi membri pubblici.

Il `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` pagine si presuppongono che la pagina master deriva il `BaseMasterPage` classe. Il `AdminNested.master` pagina, tuttavia, attualmente estende la `System.Web.UI.MasterPage` classe. Di conseguenza, durante la visita `~/Admin/Products.aspx` un `InvalidCastException` viene generata un'eccezione con il messaggio: "Impossibile eseguire il cast dell'oggetto di tipo ' ASP.admin\_adminnested\_master' nel tipo 'BaseMasterPage'."

Per risolvere il problema è necessario che il `AdminNested.master` classe code-behind estendere `BaseMasterPage`. Aggiornare la dichiarazione di classe codice della pagina master annidata dal:


[!code-csharp[Main](nested-master-pages-cs/samples/sample14.cs)]

A:


[!code-csharp[Main](nested-master-pages-cs/samples/sample15.cs)]

Non è stata ancora completata. Poiché il `BaseMasterPage` classe è astratta, è necessario eseguire l'override di `abstract` membri, `RefreshRecentProductsGrid` e `GridMessageText`. Questi membri vengono utilizzati per le pagine master di primo livello per aggiornare le relative interfacce utente. (In realtà, solo il `Site.master` pagina master utilizza questi metodi, anche se entrambe le pagine master principale implementano questi metodi, poiché entrambi estendere `BaseMasterPage`.)

Anche se è necessario implementare questi membri di `AdminNested.master`, sufficiente queste implementazioni è sufficiente chiamare lo stesso membro nella pagina master principale utilizzata dalla pagina master nidificata. Ad esempio, quando una pagina contenuto nella sezione Amministrazione chiama la pagina master annidata `RefreshRecentProductsGrid` (metodo), è necessario eseguire la pagina master annidata è, a sua volta, chiamare `Site.master` o `Alternate.master`del `RefreshRecentProductsGrid` metodo.

A tale scopo, iniziare ad aggiungere le seguenti `@MasterType` direttiva all'inizio del `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample16.aspx)]

Tenere presente che il `@MasterType` direttiva aggiunge una proprietà fortemente tipizzata per la classe code-behind denominata `Master`. Quindi eseguire l'override di `RefreshRecentProductsGrid` e `GridMessageText` membri e il delegato semplicemente la chiamata al `Master`corrispondente del metodo:


[!code-csharp[Main](nested-master-pages-cs/samples/sample17.cs)]

Con questo codice, deve essere in grado di vedere e utilizzare le pagine di contenuto della sezione di amministrazione. Figura 12 illustra il `~/Admin/Products.aspx` pagina quando viene visualizzato tramite un browser. Come si può notare, la pagina include la casella di istruzioni di amministrazione in cui è definita nella pagina master nidificata.


[![Le pagine di contenuto della sezione amministrazione comprendono delle istruzioni nella parte superiore di ogni pagina](nested-master-pages-cs/_static/image35.png)](nested-master-pages-cs/_static/image34.png)

**Figura 12**: le pagine contenuto nelle amministrazione sezione includono istruzioni nella parte superiore di ogni pagina di ([fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Passaggio 7: Utilizzo della pagina Master di primo livello appropriato in fase di esecuzione

Mentre tutte le pagine di contenuto nella sezione amministrazione sono pienamente funzionali, tutte utilizzano la stessa pagina di primo livello master e ignorare la pagina master selezionata dall'utente in `ChooseMasterPage.aspx`. Questo comportamento è dovuto al fatto che la pagina master annidata è relativo `MasterPageFile` proprietà è impostata in modo statico su `Site.master` nel relativo `<%@ Master %>` direttiva.

Utilizzare la pagina di primo livello master selezionata dall'utente finale, è necessario impostare il `AdminNested.master`del `MasterPageFile` proprietà sul valore di `MyMasterPage` variabile di sessione. Poiché viene impostato in modo le pagine contenute `MasterPageFile` proprietà in `BasePage`, si potrebbe pensare che è sufficiente impostare la pagina master annidata `MasterPageFile` proprietà in `BaseMasterPage` o nel `AdminNested.master`della classe code-behind. Questo funzionerà, tuttavia, in quanto è necessario aver impostato la `MasterPageFile` proprietà entro la fine della fase PreInit. La prima ora che è possibile toccare a livello di codice nel ciclo di vita di pagina da una pagina master è la fase di inizializzazione (che si verifica dopo la fase PreInit).

Pertanto, è necessario impostare la pagina master annidata `MasterPageFile` proprietà dalle pagine di contenuto. Il contenuto solo le pagine che utilizzano il `AdminNested.master` pagina master derivano da `AdminBasePage`. Pertanto, è possibile inserire questa logica sono. Nel passaggio 5 è overrode il `SetMasterPageFile` (metodo), l'impostazione di `Page` dell'oggetto `MasterPageFile` proprietà su "~ / Admin/AdminNested.master". Aggiornamento `SetMasterPageFile` impostare anche la pagina master `MasterPageFile` proprietà per il risultato archiviato nella sessione:


[!code-csharp[Main](nested-master-pages-cs/samples/sample18.cs)]

Il `GetMasterPageFileFromSession` metodo, che è stato aggiunto alla `BasePage` classe nell'esercitazione precedente, restituisce il percorso del file pagina master appropriato in base al valore di variabile di sessione.

Con questa modifica sul posto, la selezione dell'utente pagina master viene mantenuta in sezione Amministrazione. Figura 13 illustra la stessa pagina, come nella figura 12, ma dopo che l'utente ha modificato la selezione della pagina master per `Alternate.master`.


[![La pagina di amministrazione nidificata utilizza la pagina Master principale selezionato dall'utente](nested-master-pages-cs/_static/image38.png)](nested-master-pages-cs/_static/image37.png)

**Figura 13**: la pagina di amministrazione Nested utilizza la pagina Master di primo livello selezionato dall'utente ([fare clic per visualizzare l'immagine ingrandita](nested-master-pages-cs/_static/image39.png))


## <a name="summary"></a>Riepilogo

Quantità pagine di contenuto come like è possono associare a una pagina master, è possibile creare nidificata pagine master con una pagina master figlio associano a una pagina master padre. La pagina master figlio può definire i controlli del contenuto per ogni ContentPlaceHolder del padre; è quindi possibile aggiungere controlli propri ContentPlaceHolder (così come altri markup) per i controlli contenuti. Pagine master annidate sono molto utili nelle applicazioni web di grandi dimensioni in cui tutte le pagine di condividono un aspetto unificato, ma determinate sezioni del sito devono essere univoche personalizzate.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Pagine Master ASP.NET annidate](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Suggerimenti per la fase di progettazione di Visual Studio 2005 e pagine Master annidate](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Visual Studio 2008 annidati supporto delle pagine Master](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft fin dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 3.5 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott può essere raggiunto al [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](specifying-the-master-page-programmatically-cs.md)
> [Successivo](creating-a-site-wide-layout-using-master-pages-vb.md)
