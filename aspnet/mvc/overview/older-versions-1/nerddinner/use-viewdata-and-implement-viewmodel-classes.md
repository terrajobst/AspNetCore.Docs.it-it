---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Utilizzare ViewData e implementare ViewModel classi | Documenti Microsoft
author: microsoft
description: "Passaggio 6 mostra come Abilita il supporto per più form modifica di scenari e viene inoltre due approcci che possono essere utilizzati per passare i dati dai controller alle visualizzazioni:..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 36b9e87cc24f74f7f2cc592afb5102709b598f74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Utilizzare ViewData e implementare ViewModel classi
====================
da [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 6 di una liberazione [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 6 mostra come Abilita il supporto per più form modifica di scenari e viene inoltre due approcci che possono essere utilizzati per passare i dati dai controller alle visualizzazioni: ViewData e ViewModel.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner passaggio 6: ViewData e ViewModel

Abbiamo interessato un numero di scenari di post dei form e illustrato come implementare creazione, aggiornamento ed eliminazione di supporto (CRUD). Viene ora eseguire l'implementazione di DinnersController ulteriori e abilitare il supporto per gli scenari di modifica di moduli più dettagliato. Durante questa operazione si parlerà due approcci che possono essere utilizzati per passare i dati dai controller alle visualizzazioni: ViewData e ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Passaggio di dati da controller a modelli di visualizzazione

Tra le caratteristiche di definizione del modello MVC è di tipo strict "separazione delle problematiche" consente di applicare tra i diversi componenti di un'applicazione. I modelli, controller e visualizzazioni ogni ben definiti ruoli e responsabilità e comunicano tra loro in modo ben definito. Questo consente di alzare di livello testabilità e riutilizzo del codice.

Quando una classe Controller decide di eseguire il rendering di una risposta HTML a un client, è responsabile per il passaggio in modo esplicito per il modello di visualizzazione di tutti i dati necessari per eseguire il rendering nella risposta. Modelli di visualizzazione non dovrebbero mai eseguire qualsiasi logica di applicazione o di recupero dati: e devono invece limitarsi per dispone solo del codice per il rendering viene gestito di fuori di modello/dati passati dal controller.

Ora i dati del modello passato per il nostro DinnersController classe per i modelli di visualizzazione è semplice e semplice: un elenco di oggetti Dinner nel caso di Index () e un singolo Dinner degli oggetti nel caso di Details(), Edit(), metodo di creazione e Delete (). Man mano che si aggiungono altre funzionalità dell'interfaccia utente per l'applicazione, spesso verrà necessario passare più dati per eseguire il rendering delle risposte HTML all'interno i nostri modelli di visualizzazione. Ad esempio, potrebbe essere opportuno modificare il campo "Paese" all'interno di questo tipo di modifica e creare visualizzazioni da in una casella di testo HTML per un controllo dropdownlist. Anziché a livello di codice l'elenco a discesa dei nomi di paese nel modello di visualizzazione, potrebbe essere opportuno per generarlo da un elenco di paesi supportati che è compilare in modo dinamico. È necessario un modo per passare l'oggetto Dinner *e* l'elenco di paesi supportati da questo controller per i modelli di visualizzazione.

Situazione in due modi, che è possibile eseguire questa operazione.

### <a name="using-the-viewdata-dictionary"></a>Utilizzando il dizionario ViewData

La classe di base Controller espone una proprietà del dizionario "ViewData" che può essere usata per passare gli elementi di dati aggiuntivi da controller alle visualizzazioni.

Per supportare lo scenario in cui si desidera la casella di testo "Paese" entro la visualizzazione di modifica viene modificato da una casella di testo HTML per un controllo dropdownlist, ad esempio, è possibile aggiornare il metodo di azione Edit() per passare (oltre a un oggetto Dinner) un oggetto SelectList che può essere utilizzato come la m enti di un controllo dropdownlist paesi.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Il costruttore di SelectList di cui sopra accetta un elenco di regioni per popolare il downlist rilascio con il valore attualmente selezionato.

È quindi possibile aggiornare il modello di visualizzazione Edit per utilizzare il metodo di supporto Html.DropDownList() anziché il metodo helper Html.TextBox() che è utilizzate in precedenza:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Il metodo di supporto Html.DropDownList() precedente accetta due parametri. Il primo è il nome dell'elemento di form HTML per l'output. Il secondo è il modello "SelectList" è passati tramite il dizionario ViewData. Si sta usando c# "as" parola chiave per il cast del tipo all'interno del dizionario come una SelectList.

E ora quando si esegue l'accesso alle applicazioni e il */Dinners/Edit/1* URL nel browser possiamo vedere che la modifica dell'interfaccia utente è stato aggiornato per visualizzare un dropdownlist dei paesi anziché una casella di testo:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Dal momento che si esegue il rendering anche il modello di visualizzazione di modifica dal metodo HTTP-POST modifica (in scenari quando si verificano errori), è opportuno per assicurarsi che abbiamo anche aggiornare questo metodo per aggiungere il SelectList ViewData quando il modello di visualizzazione viene eseguito il rendering in scenari di errore:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

E questo scenario di modifica DinnersController supporta ora un DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Uso di un modello ViewModel

L'approccio di dizionario ViewData ha il vantaggio di essere piuttosto veloce e facile da implementare. Alcuni sviluppatori non piace utilizzando i dizionari basato su stringa, tuttavia, poiché gli errori che non saranno rilevati in fase di compilazione può causare errori di digitazione. Il dizionario ViewData non tipizzato richiede anche utilizzando l'operatore "come" o eseguire il cast quando si usa un linguaggio fortemente tipizzato come c# in un modello di visualizzazione.

Un approccio alternativo che è possibile usare è noto anche come il modello "View". Quando si utilizza questo modello che è creare classi fortemente tipizzate che sono ottimizzati per gli scenari di visualizzazione ed ed espongono le proprietà per i valori o contenuto dinamico necessari per i modelli di visualizzazione. La classi controller quindi compilare e passare queste classi con ottimizzazione per la visualizzazione per il modello di visualizzazione da utilizzare. In questo modo l'indipendenza dai tipi, il controllo in fase di compilazione e intellisense editor all'interno di modelli di visualizzazione.

Ad esempio, per consentire la cena modulo modificando scenari è possibile creare un "DinnerFormViewModel" classe come di seguito che espone due proprietà fortemente tipizzate: un oggetto Dinner e il modello SelectList necessari per popolare dropdownlist paesi:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

È possibile aggiornare quindi il metodo di azione Edit() per creare il DinnerFormViewModel utilizzando l'oggetto Dinner che viene recuperato da questo repository e quindi passare a questo modello di visualizzazione:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Ti invieremo un quindi aggiorna il modello di visualizzazione in modo che prevede "DinnerFormViewModel" anziché "La cena" oggetto modificando l'attributo "inherits" nella parte superiore della pagina Edit come illustrato di seguito:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Quando prepariamo queste intellisense della proprietà "Modello" all'interno di modelli di visualizzazione verrà aggiornata per riflettere il modello a oggetti del tipo DinnerFormViewModel che è passato:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

È quindi possibile aggiornare il codice di visualizzazione di lavoro viene. Avviso seguito modo non si intende modificare i nomi degli elementi di input è in corso la creazione (gli elementi del modulo verranno comunque denominati "Title", "Paese"), ma stiamo aggiornando i metodi HTML Helper per recuperare i valori utilizzando la classe DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Verrà inoltre aggiornata metodo post di modifica per utilizzare la classe DinnerFormViewModel quando degli errori di rendering:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

È possibile aggiornare i metodi di azione del metodo di creazione di riutilizzare l'esatto stesso *DinnerFormViewModel* classe per consentire i paesi DropDownList all'interno di tali anche. Di seguito è riportata l'implementazione HTTP-GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Di seguito è riportata l'implementazione del metodo HTTP-POST creare:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

E nostre schermate modifica e crea supportano ora elenchi di rilascio per prelevare il paese.

### <a name="custom-shaped-viewmodel-classes"></a>Classi ViewModel forma personalizzata

Nello scenario precedente, la classe DinnerFormViewModel espone direttamente l'oggetto modello Dinner come una proprietà, insieme a una proprietà del modello SelectList supporto. Questo approccio funziona per gli scenari in cui l'interfaccia utente HTML si intende creare all'interno di modelli di visualizzazione corrisponde relativamente strettamente per gli oggetti modello di dominio.

Per gli scenari in cui questo non è il caso, un'opzione che è possibile utilizzare consiste nel creare una classe ViewModel forma personalizzata il cui modello a oggetti più è ottimizzato per l'utilizzo da parte della vista – e che può essere completamente diverso dall'oggetto del modello di dominio sottostante. Ad esempio, potrebbe potenzialmente esporre i nomi di proprietà diversi e/o le proprietà di aggregazione raccolte da più oggetti modello.

Le classi ViewModel forma personalizzati possono essere entrambi utilizzato per passare i dati dai controller alle visualizzazioni per il rendering, nonché per agevolare la gestione di dati del form rinviati al metodo di azione di un controller. Per questo scenario successivo, aggiornare l'oggetto ViewModel con i dati di modulo registrato e quindi utilizzare l'istanza ViewModel per eseguire il mapping o recuperare un oggetto del modello di dominio effettivo potrebbe essere il metodo di azione.

Classi di ViewModel a forma di personalizzato possono fornire una notevole flessibilità e sono occorre esaminare ogni volta che si trova il codice per il rendering all'interno dei modelli di visualizzazione o il codice di registrazione di modulo nei metodi di azione a partire da ottenere troppo complesso. Questo è spesso un segno che i modelli di dominio non corrispondono correttamente all'interfaccia utente a cui si sta generando e che può aiutare a una classe intermedia ViewModel a forma di personalizzati.

### <a name="next-step"></a>Passo successivo

Ora esaminiamo come è possibile utilizzare parziali e pagine master consente di riutilizzare e condividere dell'interfaccia utente tra l'applicazione.

>[!div class="step-by-step"]
[Precedente](provide-crud-create-read-update-delete-data-form-entry-support.md)
[Successivo](re-use-ui-using-master-pages-and-partials.md)
