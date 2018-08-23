---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Pagine master | Microsoft Docs
author: microsoft
description: Uno dei componenti principali di un sito Web corretto è un aspetto uniforme e coerente. In ASP.NET 1.x, gli sviluppatori utilizzavano controlli utente replicare comuni elem. di pagina...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: f40eb338a1b6b8eebb6578dd7938e96a05b1617f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827634"
---
<a name="master-pages"></a>Pagine master
====================
da [Microsoft](https://github.com/microsoft)

> Uno dei componenti principali di un sito Web corretto è un aspetto uniforme e coerente. In ASP.NET 1.x, gli sviluppatori utilizzavano controlli utente per la replica di elementi comuni delle pagine in un'applicazione Web. Anche se si tratta di una soluzione utilizzabile per ottenere, tramite i controlli utente presenta tuttavia alcuni svantaggi. Ad esempio, una modifica della posizione di un controllo utente richiede una modifica a più pagine in un sito. Controlli utente non viene eseguiti anche nella visualizzazione Progettazione dopo l'inserimento in una pagina.


Uno dei componenti principali di un sito Web corretto è un aspetto uniforme e coerente. In ASP.NET 1.x, gli sviluppatori utilizzavano controlli utente per la replica di elementi comuni delle pagine in un'applicazione Web. Anche se si tratta di una soluzione utilizzabile per ottenere, tramite i controlli utente presenta tuttavia alcuni svantaggi. Ad esempio, una modifica della posizione di un controllo utente richiede una modifica a più pagine in un sito. Controlli utente non viene eseguiti anche nella visualizzazione Progettazione dopo l'inserimento in una pagina.

ASP.NET 2.0 introduce Master le pagine come modo di mantenere un aspetto uniforme e coerente e come si scoprirà presto, Master pagine rappresentano un significativo miglioramento rispetto al metodo di controllo utente.

## <a name="why-master-pages"></a>Pagine Master perché?

Si potrebbe chiedere perché erano necessari pagine master in ASP.NET 2.0. Dopotutto, gli sviluppatori di siti Web usano già i controlli utente in ASP.NET 1.x a condividere le aree del contenuto tra le pagine. Esistono varie ragioni per cui controlli utente sono una soluzione non ottimale per la creazione di un layout comune.

Controlli utente realtà non definiscono il layout di pagina. Al contrario, definire il layout e le funzionalità per una parte di una pagina. La differenza tra queste due classi è importante perché rende molto più difficile la facilità di gestione di una soluzione di controllo utente. Ad esempio, quando si desidera modificare la posizione di un controllo utente della pagina, è necessario modificare la pagina effettiva in cui viene visualizzato il controllo utente. Memorizzate correttamente se si dispone solo di poche pagine, ma in siti di grandi dimensioni, diventa rapidamente un incubo di gestione del sito.

Un altro svantaggio dell'uso di controlli utente per definire un layout comune è rooted nell'architettura di ASP.NET. Se viene modificato alcun membro pubblico di un controllo utente, è necessario ricompilare tutte le pagine che utilizzano il controllo utente. ASP.NET verrà a sua volta, accedere a pagine quando vengono prima kompilaci JIT. Questa operazione, ancora una volta, genera un'architettura non scalabile e un problema di gestione del sito per siti di grandi dimensioni.

Entrambi questi problemi (e molto altro ancora) vengono indirizzate da pagine master in ASP.NET 2.0.

## <a name="how-master-pages-work"></a>Funzionamento delle pagine Master

Una pagina master è analoga a un modello per le altre pagine. Gli elementi della pagina che devono essere condiviso da altre pagine (vale a dire i menu, i bordi e così via) vengono aggiunti alla pagina master. Quando vengono aggiunte nuove pagine al sito, è possibile associare a una pagina master. Una pagina associata a una pagina master viene definita un' **pagina contenuto**. Per impostazione predefinita, una pagina di contenuto assume l'aspetto della pagina master. Tuttavia, quando si crea una pagina master, è possibile definire parti della pagina che può sostituire la pagina di contenuto con il proprio contenuto. Queste parti vengono definiti utilizzando un nuovo controllo introdotto in ASP.NET 2.0. il **ContentPlaceHolder** controllo.

Una pagina master può contenere qualsiasi numero di controlli ContentPlaceHolder (o nessuno.) Nella pagina contenuto, viene visualizzato il contenuto dai controlli ContentPlaceHolder all'interno di **contenuto** controlli, un altro nuovo controllo in ASP.NET 2.0. Per impostazione predefinita, le pagine di contenuto che controlla il contenuto sono vuote, in modo che sia possibile fornire il proprio contenuto. Se si desidera utilizzare il contenuto della pagina master all'interno di controlli contenuto, non è così come si vedrà più avanti in questo modulo. Il controllo contenuto viene mappato al controllo ContentPlaceHolder tramite l'attributo ContentPlaceHolderID del controllo contenuto. Il codice seguente esegue il mapping un controllo contenuto a un controllo ContentPlaceHolder chiamato mainBody su una pagina master.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Spesso si farà persone vengono descritte le pagine master come una classe di base per le altre pagine. Memorizzate in realtà non è true. La relazione tra le pagine master e le pagine di contenuto non fa parte di ereditarietà.


**Figura 1** Mostra una pagina master e da una pagina di contenuto associata come appaiono in Visual Studio 2005. È possibile visualizzare il controllo ContentPlaceHolder nella pagina master e il corrispondente nella pagina di contenuto controllo contenuto. Si noti che il contenuto di pagine master di fuori di ContentPlaceHolder sia visibile ma grigio nella pagina di contenuto. Solo il contenuto all'interno di ContentPlaceHolder può essere sostituito dalla pagina contenuto. Tutti gli altri contenuti da cui deriva la pagina master non sono modificabili.


![Una pagina master e la relativa pagina di contenuto associata](master-pages/_static/image1.jpg)

**Figura 1**: una pagina master e la relativa pagina di contenuto associata


## <a name="creating-a-master-page"></a>Creazione di una pagina Master

Per creare una nuova pagina master:

1. Aprire Visual Studio 2005 e creare un nuovo sito Web.
2. Fare clic sul File, nuovo, il File.
3. Scegliere Master File dalla finestra di dialogo Aggiungi nuovo elemento, come illustrato nella **figura 2**.
4. Fare clic su Aggiungi.


![Creazione di una nuova pagina Master](master-pages/_static/image2.jpg)

**Figura 2**: creazione di una nuova pagina Master


Si noti che è l'estensione di file per una pagina master <em>master</em>. Questo è uno dei modi in cui una pagina master è diverso da una pagina normale. La principale differenza è che anziché un @Page direttiva della pagina master contiene un @Master direttiva. Passare alla visualizzazione origine per il servizio master pagina è stata appena creata ed esaminare il codice.

Una nuova pagina master avrà un controllo ContentPlaceHolder per impostazione predefinita. Nella maggior parte dei casi, è più opportuno creare prima gli elementi comuni della pagina e quindi inserire i controlli ContentPlaceHolder in cui si desidera contenuto personalizzato. In questi casi, gli sviluppatori dovranno eliminare il controllo ContentPlaceHolder predefinito e inserire nuovi record quando viene sviluppata la pagina. Controlli ContentPlaceHolder non sono possibile ridimensionare nonostante il fatto che essi vengono visualizzati i quadratini di ridimensionamento. Le dimensioni del controllo ContentPlaceHolder automaticamente in base al contenuto in esso contenuti con un'eccezione; Se si inserisce un controllo ContentPlaceHolder all'interno di un elemento di blocco, ad esempio una cella della tabella, verrà ridimensionato in base alle dimensioni dell'elemento.

## <a name="lab-1-working-with-master-pages"></a>Laboratorio 1 utilizzo delle pagine Master

In questo laboratorio, si verrà creare una nuova pagina master e definire tre controlli ContentPlaceHolder. Si verrà quindi creare una nuova pagina di contenuto e sostituire il contenuto da almeno uno dei controlli ContentPlaceHolder.

1. Creare una pagina master e inserire i controlli ContentPlaceHolder. 

    1. Creare una nuova pagina master, come descritto in precedenza.
    2. Eliminare il controllo ContentPlaceHolder predefinito.
    3. Selezionare il controllo ContentPlaceHolder facendo clic sul bordo ombreggiato superiore del controllo e quindi eliminarla premendo il tasto CANC sulla tastiera.
    4. Inserire una nuova tabella tramite il *intestazione e il lato* modello come illustrato nella figura 3. Modificare la larghezza e altezza per il 90% ogni in modo che l'intera tabella è visibile nella finestra di progettazione.


![](master-pages/_static/image3.jpg)

**Figura 3**


1. Posizionare il cursore in ogni cella della tabella e impostare il *valign* proprietà *top*.
2. Dalla casella degli strumenti, inserire un controllo ContentPlaceHolder nella prima cella della tabella (la cella di intestazione.)
3. Quando si inserisce questo controllo ContentPlaceHolder, si noterà che l'altezza della riga verrà occupano quasi tutta la pagina come illustrato nella figura 4. Deve preoccuparsi che a questo punto.


![Lo spazio vuoto è nella stessa cella come il controllo ContentPlaceHolder](master-pages/_static/image1.gif)

**Figura 4**: lo spazio vuoto è nella stessa cella come il controllo ContentPlaceHolder


1. Inserire un controllo ContentPlaceHolder le altre due celle. Dopo che sono stati inseriti gli altri controlli ContentPlaceHolder, le dimensioni delle celle della tabella devono essere come previsto. La pagina deve ora apparire come illustrata nella pagina **figura 5**.


![Il Master con tutti i controlli ContentPlaceHolder. Si noti che l'altezza della cella per cella di intestazione è ciò che deve essere](master-pages/_static/image2.gif)

**Figura 5**: Master The con tutti i controlli ContentPlaceHolder. Si noti che l'altezza della cella per cella di intestazione è ciò che deve essere


1. Immettere il testo di propria scelta in ognuna delle tre controlli ContentPlaceHolder.
2. Salvare la pagina master come exercise1.master.
3. Creare un nuovo modulo Web e associarla alla pagina master exercise1.master.
4. Selezionare File, nuovo, il File in Visual Studio 2005.
5. Selezionare **Web Form** nella finestra di dialogo Aggiungi nuovo elemento.
6. Assicurarsi che sia selezionata la casella di controllo Seleziona pagina master come illustrato nella figura 6.


![Aggiunta di una nuova pagina contenuto](master-pages/_static/image3.gif)

**Figura 6**: aggiunta di una nuova pagina contenuto


1. Fare clic su Aggiungi.
2. Selezionare lo exercise1.master selezionare una finestra di dialogo pagina master come illustrato nella figura 7.
3. Fare clic su OK per aggiungere la nuova pagina contenuta.

La nuova pagina di contenuto viene visualizzato in Visual Studio con un controllo contenuto che per ogni controllo ContentPlaceHolder nella pagina master. Per impostazione predefinita, i controlli contenuto sono vuoti, in modo che sia possibile aggiungere il proprio contenuto. Se avessero, per poter usare il contenuto dal controllo ContentPlaceHolder nella pagina master, è sufficiente fare clic sul simbolo di smart tag (la piccola freccia nera nell'angolo superiore destro del controllo) e scegliere *Masters contenuto predefinito* nello smart tag come illustrato nella **figura 8**. Quando in questo caso, la voce di menu viene modificato in *creare contenuto personalizzato*. Clic su di esso a questo punto rimuove il contenuto della pagina master che consente di definire il contenuto personalizzato per tale controllo contenuto.


![L'impostazione di un controllo contenuto sul valore predefinito per il contenuto di pagine Master](master-pages/_static/image4.gif)

**Figura 7**: impostazione di un controllo contenuto per impostazione predefinita per il contenuto di pagine Master


## <a name="connecting-master-page-and-content-pages"></a>Pagina Master che esegue la connessione e le pagine di contenuto

L'associazione tra una pagina master e una pagina di contenuto può essere configurato in uno dei quattro modi diversi:

- Il <strong>MasterPageFile</strong> attributo il @Page (direttiva)
- Impostando il **Page.MasterPageFile** proprietà nel codice.
- Il **&lt;pagine&gt;** elemento nel file di configurazione dell'applicazione (Web. config nella cartella radice dell'applicazione)
- Il **&lt;pagine&gt;** elemento in un file di configurazione di sottocartelle (Web. config in una sottocartella)

## <a name="masterpagefile-attribute"></a>MasterPageFile (attributo)

L'attributo MasterPageFile semplifica applicare una pagina master per una particolare pagina ASP.NET. È anche il metodo utilizzato per applicare la pagina master quando si seleziona il **Seleziona pagina Master** dopo averla selezionata come nell'esercizio 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Impostazione Page.MasterPageFile nel codice

Impostando la proprietà MasterPageFile nel codice, è possibile applicare una particolare pagina master per il contenuto in fase di esecuzione. Ciò è utile nei casi in cui potrebbe essere necessario applicare una pagina master specifica basata su un ruolo di utenti o altri criteri. La proprietà MasterPageFile deve essere impostata nel metodo PreInit. Se viene impostata dopo il metodo PreInit, verrà generata un'eccezione InvalidOperationException. La pagina in cui viene impostata questa proprietà deve avere anche un contenuto controllo come controllo di primo livello per la pagina. In caso contrario verrà generata eccezione HttpException quando è impostata la proprietà MasterPageFile.

## <a name="using-the-ltpagesgt-element"></a>Usando il &lt;pagine&gt; elemento

È possibile configurare una pagina master per pagine impostando l'attributo masterPageFile &lt;pagine&gt; elemento del file Web. config. Quando si usa questo metodo, tenere presente che i file Web. config più bassi nella struttura dell'applicazione è possono sostituire questa impostazione. Qualsiasi set di attributi MasterPageFile un @Page direttiva sostituiranno anche questa impostazione. Usando il &lt;pagine&gt; elemento rende più semplice creare un <em>master</em> pagina master che può essere sottoposto a override se necessario in determinate cartelle o file.

## <a name="properties-in-master-pages"></a>Proprietà nelle pagine Master

Una pagina master può esporre proprietà eseguendo semplicemente le proprietà pubbliche all'interno della pagina master. Ad esempio, il codice seguente definisce una proprietà denominata SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Per accedere alla proprietà SomeProperty dalla pagina di contenuto, è necessario utilizzare lo schema proprietà come segue:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Pagine Master di annidamento

Pagine master sono la soluzione ideale per garantire un aspetto comune e coerente tra un'applicazione Web di grandi dimensioni. Non è, tuttavia, spesso di avere determinate parti di un grande sito condividono un'interfaccia comune, mentre altre parti condividono un'interfaccia diversa. Per soddisfare questa esigenza, diverse pagine master sono la soluzione perfetta. Tuttavia, ancora non si occupa del fatto che un'applicazione di grandi dimensioni può avere alcuni componenti (ad esempio un menu, ad esempio) che vengono condivise tra tutte le pagine e altri componenti che vengono condivisi solo tra determinate sezioni del sito. Per tale tipo di situazione, pagine master annidate riempire la necessità perfettamente. Come si è visto, una normale pagina master è costituita da una pagina master e una pagina di contenuto. In una situazione di pagina master annidata, esistono due pagine master. uno schema padre e un schema figlio. La pagina master figlio è anche una pagina di contenuto e principale è la pagina master padre.

Ecco il codice per una pagina master tipico:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

In uno scenario master annidato, questo sarebbe il master padre. Un'altra pagina master sarebbe utilizzare questa pagina come pagina master e che il codice sarà analogo al seguente:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Si noti che in questo scenario, il master figlio anche una pagina di contenuto per il servizio master padre. Tutti i contenuti del master figlio viene visualizzato all'interno di un controllo contenuto che ottiene il contenuto dal controllo ContentPlaceHolder dell'elemento padre.

> [!NOTE]
> Supporto della finestra di progettazione non è disponibile per le pagine master annidate. Quando si sviluppa usando master nidificati, è necessario utilizzare la visualizzazione origine.


In questo video viene illustrata una procedura dettagliata dell'uso di pagine master annidate.


![](master-pages/_static/image1.png)


[Aprirlo Video a schermo intero](master-pages/_static/nested1.wmv)


![Selezione di una pagina Master](master-pages/_static/image4.jpg)

**Figura 8**: selezione di una pagina Master
