---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Usare ViewData e implementare classi ViewModel | Microsoft Docs
author: microsoft
description: Passaggio 6 viene mostrato come Abilita il supporto per scenari di modifica di moduli più completo sono illustrate due approcci utilizzabili per passare i dati dai controller alle visualizzazioni:...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 70b264def11b55fc08165dda307ff9e82fd4be7f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826959"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Usare ViewData e implementare classi ViewModel
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 6 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 6 viene mostrato come Abilita il supporto per scenari di modifica di moduli più completo sono illustrate due approcci utilizzabili per passare i dati dai controller alle visualizzazioni: ViewData e ViewModel.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner passaggio 6: ViewData e ViewModel

Abbiamo trattati numerosi scenari di post dei form ed è stato illustrato come implementare creare, aggiornare ed eliminare il supporto (CRUD). Verrà ora migliorano ulteriormente la nostra implementazione DinnersController e abilitare il supporto per gli scenari di modifica di moduli più avanzati. Durante questa operazione si parlerà due approcci utilizzabili per passare i dati dai controller alle visualizzazioni: ViewData e ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Passaggio di dati dai controller ai modelli di visualizzazione

Uno delle caratteristiche di definizione dello schema MVC è di tipo strict "separazione delle problematiche" consente di applicare tra i diversi componenti di un'applicazione. I modelli, controller e visualizzazioni ognuno sono ben definiti i ruoli e responsabilità e comunicano tra loro in modalità ben definite. Ciò consente di alzare di livello la testabilità e riutilizzo del codice.

Quando una classe Controller decide di eseguire il rendering di una risposta HTML a un client, è responsabile per il passaggio in modo esplicito per il modello di vista tutti i dati necessari per eseguire il rendering nella risposta. I modelli di vista evitare di eseguire qualsiasi logica di applicazione o il recupero dei dati: ed è invece opportuno limitare autonomamente solo con codice di rendering che si basa il modello/dati passato dal controller.

Ora i dati del modello passato dal nostro DinnersController classe per i modelli di visualizzazione è semplice e molto semplice: un elenco di oggetti Dinner nel caso di Index () e un singolo Dinner degli oggetti nel caso Details(), Edit (), create () e Delete (). Quando si aggiungono altre funzionalità dell'interfaccia utente all'applicazione, spesso verrà necessario passare più della semplice i dati per eseguire il rendering delle risposte HTML all'interno dei nostri modelli di visualizzazione. È ad esempio, potrebbe essere opportuno modificare il campo "Paese" entro la modifica e creazione di visualizzazioni da in corso di una casella di testo HTML per un controllo dropdownlist. Piuttosto che come hardcoded l'elenco a discesa dei nomi di paese nel modello di visualizzazione, potrebbe essere necessario generarlo da un elenco di paesi supportati che è compilare in modo dinamico. È necessario un modo per passare l'oggetto Dinner *e* l'elenco di paesi supportati da controller per i modelli di visualizzazione.

Esaminiamo due modi, che è possibile eseguire questa operazione.

### <a name="using-the-viewdata-dictionary"></a>Utilizzando il dizionario ViewData

La classe di base Controller espone una proprietà di dizionario "ViewData" che può essere usata per passare gli elementi dati aggiuntivi dai controller alle visualizzazioni.

Per supportare lo scenario in cui si desidera la casella di testo "Paese" entro la visualizzazione di modifica viene modificato da una casella di testo HTML per un controllo dropdownlist, ad esempio, è possibile aggiornare il metodo di azione Edit () per passare (oltre a un oggetto Dinner) un oggetto SelectList che può essere utilizzato come il valore m odello di un controllo dropdownlist paesi.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Il costruttore del SelectList precedente è che accetta un elenco di provincie per popolare il rilascio-downlist con, nonché il valore attualmente selezionato.

È quindi possibile aggiornare il modello di vista Edit. aspx per usare il metodo helper Html.DropDownList() anziché il metodo helper Html.TextBox() usati in precedenza:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Il metodo helper Html.DropDownList() precedente accetta due parametri. Il primo è il nome dell'elemento form HTML di output. Il secondo è il modello "SelectList" viene passati tramite il dizionario ViewData. Utilizziamo il codice c# "come" parola chiave per il cast del tipo all'interno del dizionario come un SelectList.

E quando si esegue l'accesso alle applicazioni e il */Dinners/Edit/1* URL all'interno di browser si vedrà che la modifica dell'interfaccia utente è stato aggiornato per visualizzare un controllo dropdownlist dei paesi invece di una casella di testo:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Poiché si esegue il rendering anche il modello di visualizzazione di modifica dal metodo HTTP-POST modifica (in scenari quando si verificano errori), è necessario assicurarsi che abbiamo anche aggiornare questo metodo per aggiungere il SelectList a ViewData quando il modello di vista di cui viene eseguito il rendering in scenari di errore:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

E questo scenario di modifica DinnersController supporta ora un controllo DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Uso di un modello ViewModel

L'approccio di dizionario ViewData presenta il vantaggio di essere piuttosto veloce e facile da implementare. Alcuni sviluppatori preferiamo non utilizzare i dizionari basato su stringa, tuttavia, poiché gli errori che non verrà rilevati in fase di compilazione può causare errori di digitazione. Il dizionario ViewData non tipizzato richiede anche usando l'operatore "come" o eseguire il cast quando si usa un linguaggio fortemente tipizzato, ad esempio c# in un modello di vista.

È possibile usare un approccio alternativo è uno noto anche come il modello "ViewModel". Quando si usa questo modello è creare classi fortemente tipizzate che sono ottimizzati per gli scenari di visualizzazione specifici ed ed espongono le proprietà per i valori/contenuto dinamico necessari per i modelli di visualizzazione. Le classi controller possono quindi popolare e passare queste classi ottimizzate per la visualizzazione al nostro modello di visualizzazione da utilizzare. In questo modo l'indipendenza dai tipi, controllo della fase di compilazione e intellisense dell'editor nei modelli di visualizzazione.

Ad esempio, per consentire la cena modulo gli scenari di modifica è possibile creare un "DinnerFormViewModel" classe, come di seguito che espone due proprietà fortemente tipizzate: un oggetto Dinner e il modello SelectList necessari per popolare il controllo dropdownlist paesi:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

È possibile aggiornare quindi il metodo di azione Edit () per creare il DinnerFormViewModel utilizzando l'oggetto Dinner recuperato da repository e quindi passarlo al nostro modello di vista:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Verrà quindi aggiorna il modello di visualizzazione in modo che si prevede che un "DinnerFormViewModel" invece di un "Dinner" dell'oggetto modificando l'attributo "inherits" nella parte superiore della pagina Edit. aspx come segue:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Dopo che è eseguire questa operazione, gli elementi intellisense della proprietà "Modello" entro il modello di visualizzazione verrà aggiornata per riflettere il modello a oggetti del tipo DinnerFormViewModel che si passa il:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

È quindi possibile aggiornare il codice di visualizzazione per lavorare all'esterno di esso. Si noti che sotto il modo in cui non si stanno modificando i nomi degli elementi di input è in corso la creazione (gli elementi del modulo verranno comunque denominati "Title", "Country"), ma stiamo aggiornando i metodi HTML Helper per recuperare i valori usando la classe DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Verrà inoltre aggiornato il metodo post di modifica per usare la classe DinnerFormViewModel quando degli errori di rendering:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

È possibile aggiornare i metodi di azione create () di riutilizzare esattamente uguali *DinnerFormViewModel* classe per abilitare i paesi DropDownList all'interno di tali anche. Di seguito è riportata l'implementazione HTTP-GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Di seguito è riportata l'implementazione del metodo HTTP-POST creare:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

E ora entrambe le nostre schermate di creazione e modifica supportano drop elenchi per scegliere il paese.

### <a name="custom-shaped-viewmodel-classes"></a>Classi ViewModel a forma di personalizzato

Nello scenario precedente, la classe DinnerFormViewModel espone direttamente l'oggetto modello Dinner come proprietà, insieme a una proprietà del modello SelectList supporta. Questo approccio funziona bene per scenari in cui l'interfaccia utente HTML si creeranno entro il modello di visualizzazione corrisponde relativamente strettamente a nostro gli oggetti del modello di dominio.

Per gli scenari in cui non è il caso, un'opzione che è possibile usare consiste nel creare una classe di ViewModel a forma di personalizzata il cui modello a oggetti è più adatte per l'utilizzo da parte della vista: e che potrebbe essere visualizzato completamente diverso dall'oggetto modello di dominio sottostante. Ad esempio, potrebbe potenzialmente esporre i nomi di proprietà diversi e/o aggregata proprietà raccolte da più oggetti del modello.

Le classi ViewModel a forma di personalizzato possono essere usato sia per passare i dati dai controller alle visualizzazioni per il rendering, nonché per agevolare la gestione di dati del modulo inviati al metodo di azione del controller. Per questo scenario successivo, potrebbe essere il metodo di azione aggiornare un oggetto ViewModel con i dati di modulo inviato e quindi usare l'istanza del ViewModel per eseguire il mapping o recuperare un oggetto modello di dominio effettivi.

Classi ViewModel a forma di personalizzato possono fornire una notevole flessibilità e sono un aspetto da esaminare qualsiasi momento è trovare il codice per il rendering all'interno dei modelli di visualizzazione o il codice per la registrazione in forma all'interno di metodi di azione iniziando a diventare troppo complicato. Si tratta spesso di un segno che i modelli di dominio non corrispondono correttamente nell'interfaccia utente vengano generati e che può aiutare a una classe intermedia ViewModel a forma di personalizzati.

### <a name="next-step"></a>Passo successivo

Ora esaminiamo come possiamo usare righe parzialmente eseguite e le pagine master per riutilizzare e condividere dell'interfaccia utente nell'applicazione.

> [!div class="step-by-step"]
> [Precedente](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Successivo](re-use-ui-using-master-pages-and-partials.md)
