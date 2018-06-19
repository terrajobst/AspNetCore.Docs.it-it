---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Pagine master | Documenti Microsoft
author: microsoft
description: Uno dei componenti principali di un sito Web ha esito positivo è un aspetto coerente. In ASP.NET 1. x, gli sviluppatori di usare controlli utente per replicare comuni elem. di pagina...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: f45dd9704f665244d2a48ec000326f6e98984e4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885113"
---
<a name="master-pages"></a>Pagine master
====================
by [Microsoft](https://github.com/microsoft)

> Uno dei componenti principali di un sito Web ha esito positivo è un aspetto coerente. In ASP.NET 1. x, gli sviluppatori di controlli utente utilizzavano per replicare i comuni elementi della pagina in un'applicazione Web. Mentre è certamente una soluzione pratica, l'uso di controlli utente presenta alcuni svantaggi. Ad esempio, una modifica della posizione di un controllo utente richiede una modifica a più pagine in un sito. Controlli utente non viene eseguiti anche nella visualizzazione Progettazione dopo l'inserimento in una pagina.


Uno dei componenti principali di un sito Web ha esito positivo è un aspetto coerente. In ASP.NET 1. x, gli sviluppatori di controlli utente utilizzavano per replicare i comuni elementi della pagina in un'applicazione Web. Mentre è certamente una soluzione pratica, l'uso di controlli utente presenta alcuni svantaggi. Ad esempio, una modifica della posizione di un controllo utente richiede una modifica a più pagine in un sito. Controlli utente non viene eseguiti anche nella visualizzazione Progettazione dopo l'inserimento in una pagina.

ASP.NET 2.0 introduce Master come un modo per gestire un aspetto coerente e come si le pagine appena vedrà, Master pagine rappresentano un significativo miglioramento rispetto al metodo di controllo utente.

## <a name="why-master-pages"></a>Pagine Master perché?

Si potrebbe chiedere perché necessario pagine master in ASP.NET 2.0. Dopo tutto, gli sviluppatori di siti Web usano già i controlli utente ASP.NET 1. x per condividere le aree di contenuto tra le pagine. Esistono varie ragioni perché i controlli utente sono una soluzione ottimale per la creazione di un layout comune.

Controlli utente in realtà non definiscono il layout di pagina. Al contrario, definire il layout e le funzionalità di una parte di una pagina. La differenza tra questi due è importante perché rende molto più difficile la gestione di una soluzione di controllo utente. Ad esempio, quando si desidera modificare la posizione di un controllo utente della pagina, è necessario modificare la pagina effettiva in cui viene visualizzato il controllo utente. Se si dispone solo di alcune pagine, ma in siti di grandi dimensioni, diventa rapidamente un incubo di gestione del sito, un preciso memorizzate!

Un altro svantaggio dell'uso di controlli utente per la definizione di un layout comune è una radice nell'architettura di ASP.NET se stesso. Se qualsiasi membro pubblico di un controllo utente viene modificato, avrà bisogno di ricompilare tutte le pagine che utilizzano il controllo utente. A sua volta, ASP.NET verrà quindi accedere di re-JIT pagine quando vengono prima. Questa operazione, ancora una volta, produce un'architettura non scalabile e un problema di gestione del sito per i siti di grandi dimensioni.

Entrambi questi problemi (e molto altro ancora) ben vengono indirizzate mediante pagine master in ASP.NET 2.0.

## <a name="how-master-pages-work"></a>Funzionamento delle pagine Master

Una pagina master è analoga a un modello per le altre pagine. Elementi della pagina che devono essere condivise tra le altre pagine (ad esempio menu, i bordi, e così via) vengono aggiunti alla pagina master. Quando vengono aggiunte nuove pagine del sito, è possibile associare a una pagina master. Una pagina in cui è associata a una pagina master viene chiamata un **pagina contenuto**. Per impostazione predefinita, una pagina contenuto assume l'aspetto della pagina master. Tuttavia, quando si crea una pagina master, è possibile definire le parti della pagina che la pagina di contenuto può sostituire con il proprio contenuto. Queste parti vengono definiti utilizzando un nuovo controllo introdotto in ASP.NET 2.0. il **ContentPlaceHolder** controllo.

Una pagina master può contenere qualsiasi numero di controlli ContentPlaceHolder (o nessuno.) Nella pagina contenuto, viene visualizzato il contenuto dai controlli ContentPlaceHolder all'interno di **contenuto** controlli, un altro nuovo controllo di ASP.NET 2.0. Per impostazione predefinita, le pagine di contenuto di che controlli del contenuto sono vuote in modo che è possibile fornire i propri dati. Se si desidera utilizzare il contenuto della pagina master all'interno dei controlli contenuto, non è così come è possibile osservare più avanti in questo modulo. Il controllo contenuto viene eseguito il mapping al controllo ContentPlaceHolder tramite l'attributo ContentPlaceHolderID del controllo del contenuto. Il codice seguente esegue il mapping un controllo contenuto a un controllo ContentPlaceHolder chiamato mainBody in una pagina master.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Spesso si farà persone vengono descritte le pagine master come una classe di base per le altre pagine. Memorizzate in realtà non true. La relazione tra le pagine master e contenuto non è uno di ereditarietà.


**Figura 1** Mostra una pagina master e una pagina di contenuto associata come appaiono in Visual Studio 2005. È possibile visualizzare il controllo ContentPlaceHolder nella pagina master e il corrispondente nella pagina contenuto controllo contenuto. Si noti che il contenuto delle pagine master di fuori di ContentPlaceHolder è visibile ma grigio nella pagina di contenuto. Solo il contenuto all'interno di ContentPlaceHolder può essere sostituito dalla pagina di contenuto. Tutti gli altri contenuti da cui deriva la pagina master non sono modificabili.


![Una pagina master e la relativa pagina di contenuto associata](master-pages/_static/image1.jpg)

**Figura 1**: una pagina master e la relativa pagina di contenuto associata


## <a name="creating-a-master-page"></a>Creazione di una pagina Master

Per creare una nuova pagina master:

1. Aprire Visual Studio 2005 e creare un nuovo sito Web.
2. Fare clic su File, nuovo, File.
3. Scegli File principale dalla finestra di dialogo Aggiungi nuovo elemento, come mostrato nel **figura 2**.
4. Fare clic su Aggiungi.


![Creazione di una nuova pagina Master](master-pages/_static/image2.jpg)

**Figura 2**: creazione di una nuova pagina Master


Si noti che l'estensione di file per una pagina master è <em>master</em>. Questo è uno dei modi che una pagina master è diverso da una pagina normale. La principale differenza è che anziché un @Page direttiva, la pagina master contiene un @Master direttiva. Passare alla visualizzazione origine per il master per la pagina appena creato, esaminare il codice.

Per impostazione predefinita, una nuova pagina master sarà necessario un controllo ContentPlaceHolder. Nella maggior parte dei casi, preferibile per creare gli elementi comuni di pagina prima e quindi inserire i controlli ContentPlaceHolder in cui si desidera contenuto personalizzato. In questi casi, gli sviluppatori dovranno eliminare il controllo ContentPlaceHolder predefinito e inserire nuovi record, come la pagina è stata sviluppata. Controlli ContentPlaceHolder non sono ridimensionabili nonostante il fatto che visualizzano quadratini di ridimensionamento. Le dimensioni del controllo ContentPlaceHolder automaticamente in base al contenuto contenente con un'eccezione; Se si inserisce un controllo ContentPlaceHolder all'interno di un elemento di blocco, ad esempio una cella della tabella, verrà ridimensionato in base alle dimensioni dell'elemento.

## <a name="lab-1-working-with-master-pages"></a>Laboratorio 1 utilizzo delle pagine Master

In questo laboratorio verrà di creare una nuova pagina master e definire tre controlli ContentPlaceHolder. Si verrà quindi creare una nuova pagina contenuta e sostituire il contenuto da almeno uno dei controlli ContentPlaceHolder.

1. Creare una pagina master e inserire i controlli ContentPlaceHolder. 

    1. Creare una nuova pagina master, come descritto in precedenza.
    2. Eliminare il controllo ContentPlaceHolder predefinito.
    3. Selezionare il controllo ContentPlaceHolder facendo clic sul bordo superiore ombreggiato del controllo e quindi eliminarla premendo il tasto CANC sulla tastiera.
    4. Inserire una nuova tabella tramite il *intestazione e il lato* modello come mostrato nella figura 3. Modificare la larghezza e altezza al 90% ogni in modo che l'intera tabella è visibile nella finestra di progettazione.


![](master-pages/_static/image3.jpg)

**Figura 3**


1. Posizionare il cursore in ogni cella della tabella e impostare il *valign* proprietà *top*.
2. Dalla casella degli strumenti, inserire un controllo ContentPlaceHolder nella prima cella della tabella (cella di intestazione).
3. Quando si inserisce il controllo ContentPlaceHolder, si noterà che l'altezza della riga avrà quasi la pagina intera come illustrato nella figura 4. Preoccuparsi che a questo punto.


![Lo spazio vuoto è nella stessa cella come ContentPlaceHolder](master-pages/_static/image1.gif)

**Figura 4**: lo spazio vuoto è nella stessa cella come ContentPlaceHolder


1. Inserire un controllo ContentPlaceHolder in due celle. Dopo l'inserimento di altri controlli ContentPlaceHolder, le dimensioni delle celle della tabella devono essere come previsto. La pagina dovrebbe essere simile alla pagina visualizzata **figura 5**.


![Il Master per tutti i controlli ContentPlaceHolder. Si noti che l'altezza della cella per cella di intestazione è ora quale deve essere](master-pages/_static/image2.gif)

**Figura 5**: il Master per tutti i controlli ContentPlaceHolder. Si noti che l'altezza della cella per cella di intestazione è ora quale deve essere


1. In ognuno dei tre controlli ContentPlaceHolder, immettere del testo di propria scelta.
2. Salvare la pagina master come exercise1.master.
3. Creare un nuovo Web Form e associarlo a una pagina master exercise1.master.
4. Selezionare il File, nuovo, File in Visual Studio 2005.
5. Selezionare **Web Form** nella finestra di dialogo Aggiungi nuovo elemento.
6. Assicurarsi che sia selezionata la casella di controllo Seleziona pagina master come illustrato nella figura 6.


![Aggiunta di una nuova pagina contenuto](master-pages/_static/image3.gif)

**Figura 6**: aggiunta di una nuova pagina contenuto


1. Fare clic su Aggiungi.
2. Selezionare lo exercise1.master selezionare una finestra di dialogo pagina master come illustrato nella figura 7.
3. Fare clic su OK per aggiungere la nuova pagina contenuta.

La nuova pagina di contenuto viene visualizzato in Visual Studio con un controllo contenuto per ogni controllo ContentPlaceHolder nella pagina master. Per impostazione predefinita, i controlli del contenuto sono vuoti, in modo che è possibile aggiungere i propri dati. Se youd per poter utilizzare il contenuto dal controllo ContentPlaceHolder nella pagina master, è sufficiente fare clic sul simbolo smart tag (nero freccia nell'angolo superiore sinistro del controllo) e scegliere *contenuto master predefinito* smart tag come illustrato in **figura 8**. Quando si esegue questa operazione, la voce di menu diventa *Crea contenuto personalizzato*. Facendo clic a questo punto rimuove il contenuto della pagina master che consente di definire il contenuto personalizzato per tale controllo contenuto.


![Impostazione di un controllo contenuto sul valore predefinito per il contenuto di pagine Master](master-pages/_static/image4.gif)

**Figura 7**: impostazione di un controllo contenuto sul valore predefinito per il contenuto di pagine Master


## <a name="connecting-master-page-and-content-pages"></a>Connessione pagina Master e pagine di contenuto

L'associazione tra una pagina master e di una pagina di contenuto può essere configurata in uno dei quattro modi diversi:

- Il <strong>MasterPageFile</strong> attributo del @Page (direttiva)
- L'impostazione di **Page.MasterPageFile** proprietà nel codice.
- Il **&lt;pagine&gt;** elemento nel file di configurazione dell'applicazione (Web. config nella cartella radice dell'applicazione)
- Il **&lt;pagine&gt;** elemento in un file di configurazione (Web. config in una sottocartella) le sottocartelle

## <a name="masterpagefile-attribute"></a>MasterPageFile Attribute

L'attributo MasterPageFile rende più facile applicare una pagina master per una determinata pagina ASP.NET. È anche il metodo utilizzato per applicare la pagina master quando si archivia il **Seleziona pagina Master** dopo averla selezionata come nell'esercizio 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Impostazione Page.MasterPageFile nel codice

Impostando la proprietà MasterPageFile nel codice, è possibile applicare una particolare pagina master per il contenuto in fase di esecuzione. Ciò è utile nei casi in cui potrebbe essere necessario applicare una pagina master specifica in base al ruolo di utenti o ad altri criteri. Nel metodo PreInit, è necessario impostare la proprietà MasterPageFile. Se viene impostato dopo il metodo PreInit, verrà generata un'eccezione InvalidOperationException. La pagina in cui viene impostata questa proprietà deve avere anche un contenuto controllo come controllo di primo livello per la pagina. In caso contrario verrà generata un'HttpException quando è impostata la proprietà MasterPageFile.

## <a name="using-the-ltpagesgt-element"></a>Utilizzo di &lt;pagine&gt; elemento

È possibile configurare una pagina master per le pagine impostando l'attributo masterPageFile &lt;pagine&gt; elemento del file Web. config. Quando si utilizza questo metodo, tenere presente che il file Web. config nella struttura dell'applicazione è possono sostituire questa impostazione. Qualsiasi set di attributi MasterPageFile un @Page direttiva sostituiranno anche questa impostazione. Utilizzo di &lt;pagine&gt; elemento rende più semplice per creare un <em>master</em> pagina master che può essere sottoposto a override se necessario in determinate cartelle o file.

## <a name="properties-in-master-pages"></a>Proprietà nelle pagine Master

Una pagina master può esporre proprietà eseguendo semplicemente le proprietà pubbliche all'interno della pagina master. Ad esempio, il codice seguente definisce una proprietà denominata SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Per accedere alla proprietà SomeProperty della pagina di contenuto, è necessario utilizzare lo schema proprietà come illustrato di seguito:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Pagine Master di annidamento

Pagine master sono la soluzione ideale per garantire un aspetto comune tra un'applicazione Web di grandi dimensioni. Tuttavia, non è insolito usare determinate parti di una condivisione di grandi dimensioni di sito un'interfaccia comune, mentre altre parti condividono un'interfaccia diversa. Per soddisfare tale esigenza, più pagine master sono la soluzione ideale. Tuttavia, che ancora non risolve il fatto che un'applicazione di grandi dimensioni potrebbe avere alcuni componenti (ad esempio, un menu, ad esempio) che vengono condivisi tra tutte le pagine e altri componenti che sono condivise solo determinate sezioni del sito. Per tale tipo di situazione, pagine master annidate compilare correttamente la necessità. Come si è visto, una pagina master normale è costituito da una pagina master e di una pagina di contenuto. In una situazione di pagina master annidata, esistono due pagine master. uno schema padre e un schema di figlio. La pagina master figlio è anche una pagina di contenuto e principale è la pagina master padre.

Di seguito è riportato il codice per una pagina master tipico:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

In uno scenario master annidate, potrebbe essere il master padre. Un'altra pagina master è necessario utilizzare questa pagina come pagina master e che il codice è analogo al seguente:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Si noti che in questo scenario, il master figlio è anche una pagina contenuto per lo schema padre. Tutto il contenuto del master figlio viene visualizzato all'interno di un controllo contenuto che ottiene il contenuto dal controllo ContentPlaceHolder dell'elemento padre.

> [!NOTE]
> Supporto della finestra di progettazione non è disponibile per le pagine master annidate. Quando si sviluppa utilizzando schemi annidati, è necessario utilizzare la visualizzazione origine.


Questo video illustra una procedura dettagliata dell'utilizzo di pagine master annidate.


![](master-pages/_static/image1.png)


[Aprirlo Video a schermo intero](master-pages/_static/nested1.wmv)


![Selezione di una pagina Master](master-pages/_static/image4.jpg)

**Figura 8**: selezione di una pagina Master
