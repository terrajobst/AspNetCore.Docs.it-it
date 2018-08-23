---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: Annidati pagine Master (VB) | Microsoft Docs
author: rick-anderson
description: Illustra come annidare un'unica pagina master all'interno di altra.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 86507c92b531898db1c9d29ba2ac28cc35d5d284
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837088"
---
<a name="nested-master-pages-vb"></a>Pagine Master annidate (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Illustra come annidare un'unica pagina master all'interno di altra.


## <a name="introduction"></a>Introduzione

Nel corso delle ultime nove esercitazioni è stato descritto come implementare un layout a livello di sito con pagine master. In breve, le pagine master consentono, lo sviluppatore della pagina, per definire tag comuni nella pagina master con aree specifiche che possono essere personalizzati in base a una pagina di contenuto dal contenuto della pagina. I controlli ContentPlaceHolder in una pagina master indicano le aree personalizzabili; tag personalizzati per i controlli ContentPlaceHolder definiti nella pagina di contenuto tramite i controlli del contenuto.

Le tecniche di pagina master che abbiamo esplorato finora sono molto utili se si dispone di un singolo layout usato nell'intero sito. Tuttavia, molti siti Web di grandi dimensioni hanno un layout del sito personalizzato tra diverse sezioni. Ad esempio, prendere in considerazione un'applicazione sanitaria usata dal personale ospedale per gestire la fatturazione, le attività e le informazioni sui pazienti. Potrebbero essere presenti tre tipi di pagine web in questa applicazione:

- Pagine specifiche del membro del personale in cui i membri del personale possono aggiornare la disponibilità, visualizzare le pianificazioni o richiedere ferie.
- Pagine specifiche del paziente in cui i membri del personale consente di visualizzare o modificare le informazioni per un incontro del paziente specifico.
- Le pagine di fatturazione specifico in cui accountants esaminare corrente attestazione gli Stati e report finanziari.

Tutte le pagine potrebbero condividere un layout comune, ad esempio un menu in alto e una serie di collegamenti usati di frequente nella parte inferiore. Ma il personale, paziente e pagine di fatturazione specifico potrebbe essere necessario personalizzare il layout generico. Ad esempio, ad esempio tutte le pagine specifiche del personale devono includere un elenco di calendari e attività che mostra la disponibilità e una pianificazione giornaliera dell'utente attualmente connesso. Forse tutte le pagine specifiche del paziente necessarie visualizzare il nome, indirizzo e assicurazioni informazioni relative al paziente viene modificati cui informazioni.

È possibile creare questo tipo di layout personalizzati con *pagine master nidificate*. Per implementare lo scenario precedente, si può iniziare creando una pagina master che è definito il contenuto di layout, i menu e piè di pagina a livello di sito, con elementi ContentPlaceHolder che definiscono le aree personalizzabili. È quindi necessario creare tre pagine di master annidate, uno per ogni tipo di pagina web. Ogni pagina master annidata sarebbe definire il contenuto tra il tipo di contenuto delle pagine che utilizzano la pagina master. In altre parole, la pagina master annidata per pagine di contenuto specifici dei pazienti includerà markup e a livello di codice per la logica per la visualizzazione delle informazioni relative al paziente in fase di modifica. Quando si crea una nuova pagina dei pazienti specifici è associarlo a questa pagina master annidata.

Questa esercitazione inizia evidenziando i vantaggi di pagine master annidate. Viene quindi illustrato come creare e usare pagine master annidate.

> [!NOTE]
> Pagine master annidate sono state possibili a partire dalla versione 2.0 di .NET Framework. Visual Studio 2005, tuttavia, non include il supporto in fase di progettazione per pagine master annidate. La buona notizia è che Visual Studio 2008 offre un'esperienza avanzata in fase di progettazione per pagine master annidate. Se si è interessati a usare pagine master annidate ma utilizza ancora Visual Studio 2005, consultare [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog [suggerimenti per le pagine Master annidate in fase di progettazione di Visual Studio 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>I vantaggi di pagine Master annidate

Molti siti Web include una struttura globale del sito, nonché più personalizzate progettazioni specifiche di determinati tipi di pagine. Ad esempio, nella nostra applicazione web demo è stata creata una sezione di amministrazione rudimentale (le pagine di `~/Admin` cartella). Attualmente le pagine web nel `~/Admin` cartella usano la stessa pagina master come tali pagine non nella sezione di amministrazione (vale a dire `Site.master` o `Alternate.master`, a seconda della selezione dell'utente).

> [!NOTE]
> Per ora, fingendo che il sito ha una sola pagina master, `Site.master`. Intendiamo soddisfare tramite pagine master annidate con pagine master due (o più) inizia con "Usando un nidificata Master per l'amministrazione sezione della pagina" più avanti in questa esercitazione.


Si supponga che è stato richiesto di personalizzazione del layout delle pagine di amministrazione per includere informazioni aggiuntive o i collegamenti che altrimenti non sarebbero presenti in altre pagine nel sito. Esistono quattro tecniche per implementare questo requisito:

1. Aggiungere manualmente le informazioni specifiche di amministrazione e i collegamenti per ogni pagina contenuto nel `~/Admin` cartella.
2. Aggiornamento di `Site.master` pagina master per includere le informazioni specifiche delle sezione Amministrazione e i collegamenti e viene aggiunto codice alla pagina master per mostrare o nascondere queste sezioni basata sul fatto che una delle pagine di amministrazione da visitare.
3. Creare una nuova pagina master in modo specifico per la sezione di amministrazione, copiare il markup da `Site.master`, aggiungere le informazioni specifiche delle sezione Amministrazione e i collegamenti e quindi aggiornare le pagine di contenuto di `~/Admin` cartella da usare questo nuovo master pagina.
4. Creare una pagina master annidata che associa `Site.master` e dispone di pagine di contenuto nel `~/Admin` cartella usare questo nuovo annidate pagina master. Questa pagina master annidata includerà solo le informazioni aggiuntive e collegamenti specifici per le pagine di amministrazione e non è necessario ripetere il markup già definito in `Site.master`.

La prima opzione è la meno gradevole. Lo scopo di utilizzo delle pagine master è a non dover copiare e incollare il markup più comune per le nuove pagine ASP.NET manualmente. La seconda opzione è accettabile, ma rende meno gestibili come lo ausiliari le pagine master con il markup che viene visualizzato solo occasionalmente e richiede agli sviluppatori la modifica della pagina master per aggirare questo markup e di dover ricordare, l'applicazione esattamente, determinati markup viene visualizzato e nascosto. Questo approccio potrebbe essere meno tenable come personalizzazioni da più tipi di pagine web doveva essere gestite da questa pagina master.

La terza opzione rimuove il disordine e complessità rilascia l'esposto con la seconda opzione. Tuttavia, principale svantaggio di opzione di tre è che bisogna copiare e incollare il layout comune da `Site.master` nella nuova pagina master specifica sezione di amministrazione. Se si decide successivamente di modificare il layout a livello di sito che è necessario ricordarsi di modificarla in due posizioni.

La quarta opzione, pagine master nidificate e inviaci il meglio delle opzioni di secondo e terzo. Le informazioni sul layout a livello di sito verranno mantenuti in un unico file - la pagina master di primo livello: mentre il contenuto specifico per aree specifiche viene separato in file diversi.

Questa esercitazione inizia con uno sguardo alla creazione e utilizzo di una semplice pagina master annidata. Creiamo una nuova pagina master livello superiore, due pagine master annidate e due pagine di contenuto. A partire da "Usando un annidati Master per l'amministrazione sezione della pagina", vengono esaminati l'architettura di pagina master esistente per includere l'uso di pagine master annidate di aggiornamento. In particolare, si crea una pagina master annidata e usarlo per includere contenuti personalizzati aggiuntivi per le pagine di contenuto di `~/Admin` cartella.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Passaggio 1: Creazione di una semplice pagina Master di primo livello

Creazione di un master annidato basata su uno dei master esistente pagine e quindi l'aggiornamento di una pagina di contenuto esistente per usare questa pagina master annidata anziché la pagina master di primo livello implica alcune complessità perché già prevedono alcune pagine di contenuto esistente Controlli ContentPlaceHolder definiti nella pagina master di primo livello. Pertanto, la pagina master annidata deve includere anche gli stessi controlli ContentPlaceHolder con gli stessi nomi. Inoltre, l'applicazione demo particolare ha due pagine master (`Site.master` e `Alternate.master`) che vengono assegnate dinamicamente a una pagina contenuto in base alle preferenze dell'utente, che aggiunge a questa complessità. Si esaminerà l'aggiornamento dell'applicazione esistente da utilizzare pagine master annidate più avanti in questa esercitazione, ma è possibile dedicarsi prima un semplice annidati esempio pagine master.

Creare una nuova cartella denominata `NestedMasterPages` e quindi aggiungere un nuovo file pagina master per quella cartella denominato `Simple.master`. (Vedere la figura 1 per una schermata di Esplora soluzioni dopo l'aggiunta di questa cartella e file.) Trascinare il `AlternateStyles.css` file del foglio di stile da Esplora soluzioni nella finestra di progettazione. Verrà aggiunto un `<link>` elemento del file di foglio di stile nel `<head>` elemento, dopo il quale la pagina master `<head>` markup dell'elemento simile a:


[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

Successivamente, aggiungere il markup seguente all'interno di Web Form di `Simple.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Questo markup consente di visualizzare un collegamento denominato "Pagine Master annidate (semplice)" nella parte superiore della pagina in caratteri grandi bianco su sfondo blu. Sotto questo è il `MainContent` ContentPlaceHolder. La figura 1 mostra il `Simple.master` pagina master quando caricati nella progettazione di Visual Studio.


[![La pagina Master annidata definisce contenuto specifico per le pagine nella sezione di amministrazione](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Figura 01**: The annidati Master pagina definisce contenuto specifico per le pagine nella sezione di amministrazione ([fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Passaggio 2: Creazione di una semplice pagina Master annidata

`Simple.master` contiene due controlli ContentPlaceHolder: il `MainContent` ContentPlaceHolder è aggiunto all'interno del modulo Web insieme al `head` ContentPlaceHolder nel `<head>` elemento. Se si dovesse creare una pagina di contenuto e associarlo al `Simple.master` nella pagina di contenuto avrà due controlli contenuto di riferimento di due elementi ContentPlaceHolder. Analogamente, se si crea una pagina master annidata e associarlo al `Simple.master` quindi la pagina master annidata avrà due controlli contenuto.

È possibile aggiungere una nuova pagina master annidata per il `NestedMasterPages` cartella denominata `SimpleNested.master`. Fare clic su di `NestedMasterPages` cartella e scegliere Aggiungi nuovo elemento. Verrà visualizzata la finestra di dialogo Aggiungi nuovo elemento illustrato nella figura 2. Selezionare il tipo di modello di pagina Master e digitare il nome della nuova pagina master. Per indicare che la nuova pagina master deve essere una pagina master annidata, selezionare la casella di controllo "Seleziona pagina master".

Successivamente, fare clic sul pulsante Aggiungi. Questo visualizzerà la stessa selezionare una finestra di dialogo della pagina Master viene visualizzato quando si associa una pagina di contenuto a una pagina master (vedere la figura 3). Scegliere il `Simple.master` pagina master nel `NestedMasterPages` cartella e fare clic su OK.

> [!NOTE]
> Se è stato creato un sito Web ASP.NET usando il modello di progetto di applicazione Web anziché il modello di progetto di sito Web non si vedranno la casella di controllo "Seleziona pagina master" nella finestra di dialogo Aggiungi nuovo elemento illustrato nella figura 2. Per creare una pagina master annidata quando si usa il modello di progetto di applicazione Web è necessario scegliere il modello di pagina Master annidata (anziché il modello di pagina Master). Dopo aver selezionato il modello di pagina Master annidata e fai clic su Aggiungi, lo stesso Seleziona pagina Master verrà visualizzata la finestra di dialogo illustrata nella figura 3.


[![Verificare i &quot;Seleziona pagina master&quot; casella di controllo per aggiungere una pagina Master annidata](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Figura 02**: selezionare la casella di controllo "Seleziona pagina master" per aggiungere una pagina Master annidata ([fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image6.png))


[![Associare la pagina Master annidata alla pagina Master Simple.master](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Figura 03**: eseguire l'associazione della pagina Master annidata per il `Simple.master` pagina Master ([fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image9.png))


Markup dichiarativo della pagina master annidata, illustrato di seguito contiene due controlli contenuto che fanno riferimento a controlli ContentPlaceHolder due di livello superiore della pagina master.


[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

Fatta eccezione per il `<%@ Master %>` direttiva, markup dichiarativo iniziale della pagina master annidata è identico al markup che viene generato inizialmente quando si associa una pagina di contenuto nella stessa pagina master di primo livello. Ad esempio una pagina di contenuto `<%@ Page %>` direttiva, il `<%@ Master %>` qui direttiva include un `MasterPageFile` attributo che specifica una pagina master padre della pagina master annidata. La differenza principale tra la pagina master annidata e una pagina di contenuto associato alla stessa pagina master principale è che la pagina master annidata può includere controlli ContentPlaceHolder. Controlli ContentPlaceHolder della pagina master annidata definiscono le aree in cui le pagine di contenuto possono personalizzare il markup.

Aggiornare questa pagina master annidata in modo che venga visualizzato il testo "Hello, from SimpleNested!" nel controllo contenuto che corrisponde alla `MainContent` controllo ContentPlaceHolder.


[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Dopo aver apportato questa aggiunta, salvare la pagina master annidata e quindi aggiungere una nuova pagina contenuta per il `NestedMasterPages` cartella denominata `Default.aspx`e associarlo al `SimpleNested.master` pagina master. Quando viene aggiunta in questa pagina potrebbe essere sorprendente verificare che non contiene alcun controllo contenuto (vedere la figura 4). Una pagina di contenuto sono accessibili soltanto relativi *padre* elementi ContentPlaceHolder della pagina master. `SimpleNested.master` non contiene tutti i controlli ContentPlaceHolder; Pertanto, qualsiasi pagina di contenuto associato a questa pagina master non può contenere tutti i controlli contenuti.


[![La nuova pagina di contenuto non contiene contenuti controlli](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Figura 04**: il nuovo contenuto pagina contiene No controlli contenuto ([fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image12.png))


Che cosa dobbiamo fare è aggiornare la pagina master annidata (`SimpleNested.master`) per includere i controlli ContentPlaceHolder. In genere è opportuno le pagine master annidate per includere un controllo ContentPlaceHolder per ogni controllo ContentPlaceHolder definiti dalla relativa pagina master padre, consentendo la relativa pagina master figlio o pagina contenuta per lavorare con uno qualsiasi dei ContentPlaceHolder di livello superiore della pagina master controlli.

Aggiornamento di `SimpleNested.master` pagina master per includere un controllo ContentPlaceHolder nei suoi due controlli contenuti. Assegnare lo stesso nome del controllo ContentPlaceHolder a che proprio controllo contenuto fa riferimento controlli ContentPlaceHolder. Vale a dire, aggiungere un controllo ContentPlaceHolder denominato `MainContent` al contenuto del controllo `SimpleNested.master` che fa riferimento il `MainContent` ContentPlaceHolder in `Simple.master`. Eseguire la stessa operazione nel controllo contenuto che fa riferimento il `head` ContentPlaceHolder.

> [!NOTE]
> Mentre consigliabile assegnare i controlli ContentPlaceHolder nella pagina master annidata uguale di elementi ContentPlaceHolder nella pagina master di primo livello, la simmetria di denominazione non è necessaria. È possibile assegnare i controlli ContentPlaceHolder nella pagina master annidata qualsiasi nome desiderato. Tuttavia, se è più semplice da ricordare cosa elementi ContentPlaceHolder corrispondenti ai quali aree della pagina se la pagina master di primo livello e pagine master annidate utilizzano gli stessi nomi.


Dopo aver apportato queste aggiunte di `SimpleNested.master` markup dichiarativo della pagina master dovrebbe essere simile al seguente:


[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Eliminare il `Default.aspx` il contenuto pagina appena creato e quindi aggiungerlo nuovamente, associarla al `SimpleNested.master` pagina master. Questa volta Visual Studio aggiunge due controlli contenuto per il `Default.aspx`, che fa riferimento l'elementi ContentPlaceHolder ora definita in `SimpleNested.master` (vedere la figura 6). Aggiungere il testo "Hello, from default. aspx!" nel contenuto del controllo a cui fa riferimento `MainContent`.

La figura 5 illustra le tre entità qui - coinvolte `Simple.master`, `SimpleNested.master`, e `Default.aspx` - e come interagiscono tra loro. Come illustrato nella figura, la pagina master annidata implementa i controlli del contenuto per ContentPlaceHolder del padre. Se queste aree devono essere accessibili per la pagina contenuto, la pagina master annidata deve aggiungere il proprio elementi ContentPlaceHolder ai controlli contenuto.


[![Le pagine Master di primo livello e nidificati dettare un Layout della pagina di contenuto](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Figura 05**: le pagine Master di primo livello e nidificati dettare un Layout della pagina contenuto ([fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image15.png))


Questo comportamento viene illustrato come una pagina di contenuto o la pagina master è solo cognizant della relativa pagina master padre. Questo comportamento è indicato anche dalla progettazione di Visual Studio. Figura 6 mostra la finestra di progettazione `Default.aspx`. Mentre la finestra di progettazione mostra chiaramente quali aree geografiche possono essere modificate dalla pagina di contenuto e ciò che non sono parti, non risolvere l'ambiguità che cosa sono le aree non modificabile dalla pagina master annidata e quali aree sono dalla pagina master di primo livello.


[![L'ora di pagina contenuto include controlli contenuto per gli elementi ContentPlaceHolder della pagina Master annidata](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Figura 06**: il contenuto pagina ora include controlli contenuto per gli elementi ContentPlaceHolder della pagina Master annidata ([fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Passaggio 3: Aggiunta di una seconda pagina Master annidata semplice

Il vantaggio di pagine master annidate è più evidente quando sono presenti più pagine master annidate. Per illustrare questo vantaggio, creare un'altra pagina master annidata nel `NestedMasterPages` cartella; pagina master denominare questa nuova annidati `SimpleNestedAlternate.master` e associarlo al `Simple.master` pagina master. Aggiungere controlli ContentPlaceHolder in due controlli di contenuto della pagina master annidata, come abbiamo fatto nel passaggio 2. Aggiungere anche il testo "Hello, from SimpleNestedAlternate!" nel controllo contenuto che corrisponde a di livello superiore della pagina master `MainContent` ContentPlaceHolder. Dopo aver apportato queste modifiche markup dichiarativo di nuovo nidificata della pagina master dovrebbe essere simile al seguente:


[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Creare una pagina di contenuto denominata `Alternate.aspx` nella `NestedMasterPages` cartella e associarlo al `SimpleNestedAlternate.master` pagina master annidata. Aggiungere il testo "Hello, from alternativo!" nel controllo contenuto che corrisponde a `MainContent`. Figura 7 mostra `Alternate.aspx` quando viene visualizzato tramite la progettazione di Visual Studio.


[![Alternate.aspx è associato alla pagina Master SimpleNestedAlternate.master](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Figura 07**: `Alternate.aspx` è associato il `SimpleNestedAlternate.master` pagina Master ([fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image21.png))


Confrontare la finestra di progettazione nella figura 7 nella finestra di progettazione nella figura 6. Entrambe le pagine di contenuto condividono lo stesso layout definite nella pagina master di primo livello (`Simple.master`), vale a dire il titolo "Nested Master esercitazione sulle pagine (semplice)". Entrambi contengono ancora contenuto distinto definito nelle pagine master padre - il testo "Hello, from SimpleNested!" nelle Figure 6 e "Hello, from SimpleNestedAlternate!" Nella figura 7. Concesso, queste differenze qui sono semplici, ma è possibile estendere questo esempio per includere le differenze più significative. Ad esempio, il `SimpleNested.master` pagina potrebbe includere un menu con le opzioni specifiche per le pagine di contenuto, mentre `SimpleNestedAlternate.master` potrebbe avere informazioni relative alle pagine di contenuto a esso associati.

A questo punto, si supponga che è necessario apportare modifiche al layout del sito globale. Ad esempio, si supponga di voler aggiungere un elenco di collegamenti comuni a tutte le pagine di contenuto. A tale scopo si aggiorna la pagina master di primo livello, `Simple.master`. Eventuali modifiche vengono immediatamente riflessi nel relativo pagine master annidate e, di conseguenza, le pagine di contenuto.

Per illustrare la semplicità con cui è possibile modificare il layout del sito unificati, aprire il `Simple.master` pagina master e aggiungere il markup seguente tra il `topContent` e `mainContent` `<div>` elementi:


[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Questo consente di aggiungere due collegamenti nella parte superiore di ogni pagina in cui si associa `Simple.master`, `SimpleNested.master`, o `SimpleNestedAlternate.master`; queste modifiche si applicano le pagine di contenuto e pagine master annidate tutto immediatamente. Figura 8 mostra `Alternate.aspx` quando viene visualizzato tramite un browser. Si noti l'aggiunta di collegamenti nella parte superiore della pagina (rispetto alla figura 7).


[![Stato modificato per la pagina Master di livello superiore vengono immediatamente riflessi nelle pagine Master nidificate e pagine di contenuto Their](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Figura 08**: stato modificato per la pagina Master di livello superiore vengono immediatamente riflessi nelle pagine Master nidificate e pagine di contenuto Their ([fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Uso di una pagina Master annidata per la sezione di amministrazione

A questo punto sono stati analizzati i vantaggi del master annidata pagine e come creare e usarli in un'applicazione ASP.NET. Negli esempi di passaggi da 1, 2 e 3, tuttavia, implica la creazione di una nuova pagina master di primo livello, le nuove pagine master nidificate e nuove pagine di contenuto. Per quanto riguarda l'aggiunta una nuova pagina master annidata in un sito Web con una pagina master di primo livello esistente e le pagine di contenuto?

L'integrazione di una pagina master annidata in un sito Web esistente e la relativa associazione alle pagine di contenuto esistenti richiede un po' più complessa rispetto a partire da zero. I passaggi 4, 5, 6 e 7 esplorare queste sfide come ti forniamo altre applicazione demo per includere una nuova pagina master annidata denominata `AdminNested.master` che contiene istruzioni per l'amministratore e viene usato per le pagine ASP.NET nella `~/Admin` cartella.

L'integrazione di una pagina master annidata nell'applicazione demo introduce gli ostacoli seguenti:

- Il contenuto esistente di pagine nel `~/Admin` cartella hanno alcune aspettative da loro pagina master. Prima di tutto, si aspettano determinati controlli ContentPlaceHolder sia presente. Inoltre, il `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` pagine chiamare pubblico della pagina master `RefreshRecentProductsGrid` metodo, impostare relativo `GridMessageText` proprietà, o dispone di un gestore eventi per relativo `PricesDoubled` evento. Di conseguenza, questa pagina master annidata deve fornire la stesso elementi ContentPlaceHolder e membri pubblici.
- Nell'esercitazione precedente sono stati migliorati i `BasePage` classe per impostare dinamicamente le `Page` dell'oggetto `MasterPageFile` proprietà basato su una variabile di sessione. Come per supportiamo le pagine master dinamiche quando si usano le pagine master nidificate?

Questi due sfide esporrà come compilazione della pagina master annidata e usarlo dalle pagine di contenuto esistente. Illustreremo analizzare e vincono questi problemi quando si verificano.

## <a name="step-4-creating-the-nested-master-page"></a>Passaggio 4: Creazione di una pagina Master annidata

La prima attività consiste nel creare la pagina master annidata da utilizzare per le pagine nella sezione di amministrazione. Come abbiamo visto nel passaggio 2, quando si aggiunge un nuovo annidati pagina master è necessario specificare una pagina master padre della pagina master annidata. Ma abbiamo due pagine master di livello superiore: `Site.master` e `Alternate.master`. Tenere presente che è stata creata `Alternate.master` nell'esercitazione precedente e ha scritto codice `BasePage` classe tale set il `Page` dell'oggetto `MasterPageFile` proprietà in fase di esecuzione a una delle due `Site.master` o `Alternate.master` in base al valore di `MyMasterPage` Variabile di sessione.

Come si configura questa pagina master annidata in modo che utilizzi la pagina master principale appropriata? Sono disponibili due opzioni:

- Creare due pagine master nidificate `AdminNestedSite.master` e `AdminNestedAlternate.master`e associarle alle pagine master di primo livello `Site.master` e `Alternate.master`, rispettivamente. Nelle `BasePage`, quindi, è sufficiente impostare la `Page` dell'oggetto `MasterPageFile` alla pagina master annidata appropriata.
- Creare una singola pagina master annidata e hanno pagine di contenuto di usare questa pagina master. Quindi, in fase di esecuzione, è necessario impostare la pagina master annidata `MasterPageFile` proprietà della pagina master primo livello appropriato in fase di esecuzione. (Come si potrebbe avere intuito a questo punto, le pagine master hanno anche una `MasterPageFile` proprietà.)

È possibile usare la seconda opzione. Creare un unico file del pagina master annidata, nelle `~/Admin` cartella denominata `AdminNested.master`. Poiché entrambe `Site.master` e `Alternate.master` hanno lo stesso set di controlli ContentPlaceHolder, non importa quale pagina master è necessario associarli a, sebbene io Consiglio di associarlo al `Site.master` per i migliori risultati della coerenza.


[![Aggiungere una pagina Master annidata alla cartella ~/Admin.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Figura 09**: aggiungere una pagina Master annidata per il `~/Admin` cartella. ([Fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image27.png))


Poiché la pagina master annidata è associata a una pagina master con quattro controlli ContentPlaceHolder, Visual Studio aggiunge quattro controlli dei contenuti per tag iniziale del nuovo file pagina master annidata. Come abbiamo fatto nei passaggi 2 e 3, aggiungere un controllo ContentPlaceHolder in ogni controllo contenuto, assegnandogli lo stesso nome del controllo ContentPlaceHolder di livello superiore della pagina master. Aggiungere anche il markup seguente per il controllo contenuto che corrisponde alla `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

Successivamente, definire le `instructions` della classe CSS nel `Styles.css` e `AlternateStyles.css` file CSS. Le regole CSS seguenti causano uno stile definiti con gli elementi HTML di `instructions` classe deve essere visualizzato con un colore di sfondo giallo chiaro e un bordo nero a tinta unita:


[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Poiché questo markup è stato aggiunto alla pagina master annidata, verrà visualizzata solo in quelle pagine che utilizzano questa pagina master annidata (vale a dire, le pagine nella sezione di amministrazione).

Dopo aver apportato queste aggiunte a una pagina master annidata, il relativo markup dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Si noti che ogni controllo contenuto include un controllo ContentPlaceHolder e che i controlli ContentPlaceHolder `ID` proprietà vengono assegnate gli stessi valori di controlli ContentPlaceHolder corrispondenti nella pagina master di primo livello. Inoltre, viene visualizzato il markup specifico della sezione di amministrazione nel `MainContent` ContentPlaceHolder.

Figura 10 è illustrata la `AdminNested.master` pagina master annidata quando viene visualizzato in Progettazione di Visual Studio. È possibile visualizzare le istruzioni riportate nella casella di colore gialla nella parte superiore del `MainContent` controllo contenuto.


[![La pagina Master annidata estende la pagina Master di primo livello per includere le istruzioni per l'amministratore.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Figura 10**: la pagina Master annidata estende la pagina Master di primo livello per includere le istruzioni per l'amministratore. ([Fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Passaggio 5: Aggiornamento delle pagine di contenuto esistenti per usare la nuova pagina Master annidata

Ogni volta che è aggiungere una nuova pagina contenuta per la sezione di amministrazione è necessario associarla al `AdminNested.master` pagina master appena creato. Ma per quanto riguarda esistente il contenuto di pagine? Attualmente, tutte le pagine di contenuto nel sito di derivano il `BasePage` (classe), che a livello di codice imposta il contenuto della pagina master in fase di esecuzione. Ciò non è il comportamento desiderato per le pagine di contenuto nella sezione di amministrazione. Si vuole invece di queste pagine contenute per usare sempre la `AdminNested.master` pagina. Sarà la responsabilità della pagina master annidata per scegliere la pagina di contenuto a destra di primo livello in fase di esecuzione.

Per il modo migliore per raggiungere questo desired comportamento consiste nel creare una nuova classe di pagina base personalizzata denominata `AdminBasePage` che estende il `BasePage` classe. `AdminBasePage` quindi è possibile eseguire l'override di `SetMasterPageFile` e impostare il `Page` dell'oggetto `MasterPageFile` sul valore hardcoded "~ / Admin/AdminNested.master". In questo modo, qualsiasi pagina che deriva da `AdminBasePage` userà `AdminNested.master`, mentre qualsiasi pagina che deriva da `BasePage` avranno relativo `MasterPageFile` proprietà è impostata in modo dinamico né "~ / Site. master" o "~ / Alternate.master" in base al valore della `MyMasterPage` Variabile di sessione.

Iniziare aggiungendo un nuovo file di classe per il `App_Code` cartella denominata `AdminBasePage.vb`. Disporre `AdminBasePage` estendere `BasePage` e quindi eseguire l'override di `SetMasterPageFile` (metodo). In tale metodo assegnare i `MasterPageFile` il valore "~ / Admin/AdminNested.master". Dopo aver apportato queste modifiche la classe file dovrebbe essere simile al seguente:


[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

È ora necessario avere le pagine di contenuto esistenti nell'amministrazione sezione derivano da `AdminBasePage` invece di `BasePage`. Passare al file della classe code-behind per ogni pagina nel contenuto di `~/Admin` cartella e apportare questa modifica. Ad esempio, in `~/Admin/Default.aspx` è modificherebbe la dichiarazione di classe code-behind da:


[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

A:


[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

Figura 11 viene illustrata la modalità della pagina master di primo livello (`Site.master` o `Alternate.master`), la pagina master annidata (`AdminNested.master`), e le pagine di contenuto di sezione di amministrazione sono correlate tra loro.


[![La pagina Master annidata definisce contenuto specifico per le pagine nella sezione di amministrazione](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Figura 11**: The annidati Master pagina definisce contenuto specifico per le pagine nella sezione di amministrazione ([fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Passaggio 6: Mirroring metodi pubblici e le proprietà della pagina Master

Si tenga presente che il `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` pagine interagiscono a livello di codice con la pagina master: `~/Admin/AddProduct.aspx` chiamate della pagina master del pubblica `RefreshRecentProductsGrid` metodo e i set relativo `GridMessageText` proprietà; `~/Admin/Products.aspx` dispone di un gestore eventi per il `PricesDoubled` evento. Nell'esercitazione precedente abbiamo creato un `MustInherit` `BaseMasterPage` classe definita questi membri pubblici.

Il `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` pagine si presuppongono che la pagina master deriva dal `BaseMasterPage` classe. Il `AdminNested.master` pagina, tuttavia, attualmente estende il `System.Web.UI.MasterPage` classe. Di conseguenza, quando si visita `~/Admin/Products.aspx` un' `InvalidCastException` generata con il messaggio: "Impossibile eseguire il cast dell'oggetto di tipo ' ASP.admin\_adminnested\_master' nel tipo 'BaseMasterPage'."

Per risolvere il problema è necessario avere il `AdminNested.master` Estendi classe code-behind `BaseMasterPage`. Aggiornare una dichiarazione di classe code-behind della pagina master annidata da:


[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

A:


[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Non abbiamo ancora ancora finito. È necessario eseguire l'override di membri contrassegnati come `MustOverride`, vale a dire `RefreshRecentProductsGrid` e `GridMessageText`. Questi membri vengono utilizzati per le pagine master di primo livello per aggiornare le proprie interfacce utente. (In realtà, solo il `Site.master` pagina master Usa questi metodi, anche se entrambe le pagine master di primo livello implementano questi metodi, poiché entrambi estendono `BaseMasterPage`.)

Anche se è necessario implementare questi membri in `AdminNested.master`, tutte queste implementazioni devono eseguire è chiamare semplicemente il membro stesso nella pagina master primo livello utilizzata dalla pagina master annidata. Ad esempio, quando una pagina di contenuto nella sezione di amministrazione chiama della pagina master annidata `RefreshRecentProductsGrid` metodo, la pagina master annidata deve fare è, a sua volta, chiamare `Site.master` oppure `Alternate.master`del `RefreshRecentProductsGrid` (metodo).

A tale scopo, iniziare aggiungendo il codice seguente `@MasterType` direttiva all'inizio del `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Si tenga presente che il `@MasterType` direttiva aggiunge una proprietà fortemente tipizzata per la classe code-behind denominata `Master`. Quindi eseguire l'override di `RefreshRecentProductsGrid` e `GridMessageText` membri e semplicemente delega la chiamata al `Master`corrispondente del metodo:


[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Con questo codice, dovrebbe essere possibile visitare e usare le pagine di contenuto nella sezione di amministrazione. Figura 12 vengono illustrate le `~/Admin/Products.aspx` pagina quando viene visualizzato tramite un browser. Come può notare, la pagina include la casella di istruzioni di amministrazione in cui è definita nella pagina master annidata.


[![Pagine di contenuto nella sezione di amministrazione includono le istruzioni nella parte superiore di ogni pagina](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Figura 12**: le pagine contenuto nelle istruzioni di amministrazione sezione includono la parte superiore di ogni pagina ([fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Passaggio 7: Uso della pagina Master di primo livello appropriato in fase di esecuzione

Mentre tutte le pagine di contenuto nella sezione di amministrazione sono pienamente funzionali, tutte utilizzano la stessa pagina master di primo livello e ignorare la pagina master selezionata dall'utente in `ChooseMasterPage.aspx`. Questo comportamento è dovuto al fatto che la pagina master annidata ha relativo `MasterPageFile` staticamente impostata su `Site.master` nel relativo `<%@ Master %>` direttiva.

Utilizzare la pagina master principale selezionata dall'utente finale, dobbiamo impostare il `AdminNested.master`del `MasterPageFile` sul valore nel `MyMasterPage` variabile di sessione. Perché lo abbiamo impostato il contenuto delle pagine `MasterPageFile` le proprietà in `BasePage`, si potrebbe pensare che è sufficiente impostare la pagina master annidata `MasterPageFile` proprietà in `BaseMasterPage` o nel `AdminNested.master`della classe code-behind. Questa operazione non funziona, tuttavia, poiché è necessario aver impostato la `MasterPageFile` proprietà entro la fine della fase PreInit. Ora la prima volta che è possibile attingere a livello di codice il ciclo di vita di pagina da una pagina master è la fase di inizializzazione (che si verifica dopo la fase di PreInit).

Pertanto, è necessario impostare la pagina master annidata `MasterPageFile` proprietà dalle pagine di contenuto. Il contenuto solo le pagine che utilizzano le `AdminNested.master` pagina master derivano da `AdminBasePage`. È quindi possiamo inserire questa logica non esiste. Nel passaggio 5 è stato eseguito l'override di `SetMasterPageFile` metodo, l'impostazione dell'oggetto pagina `MasterPageFile` proprietà su "~ / Admin/AdminNested.master". Update `SetMasterPageFile` inoltre impostare la pagina master `MasterPageFile` proprietà per il risultato archiviato nella sessione:


[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

Il `GetMasterPageFileFromSession` metodo, che è stato aggiunto al `BasePage` classe nell'esercitazione precedente, restituisce il percorso del file pagina master appropriata basato sul valore della variabile di sessione.

Con questa modifica, la selezione dell'utente pagina master è ancora presente nella sezione di amministrazione. Figura 13 mostra la pagina stessa, come nella figura 12, ma dopo che l'utente ha modificato la selezione di pagina master per `Alternate.master`.


[![La pagina di amministrazione annidati Usa la pagina Master di primo livello selezionato dall'utente](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Figura 13**: la pagina di amministrazione Nested utilizza la pagina Master di primo livello selezionato dall'utente ([fare clic per visualizzare l'immagine con dimensioni normali](nested-master-pages-vb/_static/image39.png))


## <a name="summary"></a>Riepilogo

Molte pagine di contenuto come like possono associare a una pagina master, è possibile creare nidificata pagine master con una pagina master figlio associano a una pagina master padre. La pagina master figlio può definire i controlli del contenuto per ognuna delle elementi ContentPlaceHolder del controllo padre; quindi possibile aggiungere i proprio controlli ContentPlaceHolder (nonché altro markup) a questi controlli contenuto. Pagine master annidate sono molto utili nelle applicazioni web di grandi dimensioni in cui tutte le pagine condividono un aspetto globale, ma determinate sezioni del sito richiedono personalizzazioni univoche.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Pagine Master ASP.NET annidate](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Suggerimenti per la fase di progettazione di Visual Studio 2005 e pagine Master annidate](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Visual Studio 2008 annidati supporto delle pagine Master](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla.com, ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach manualmente ASP.NET 3.5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). È possibile contattarlo Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami un messaggio nella [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](specifying-the-master-page-programmatically-vb.md)
