---
title: Creazione di efficaci e reattive siti con Bootstrap
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bootstrap
ms.openlocfilehash: aee3304515686fc8e45e8e2aafb79d957219f94a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="building-beautiful-responsive-sites-with-bootstrap"></a>Creazione di efficaci e reattive siti con Bootstrap

<a name="bootstrap-index"></a>

Da [Steve Smith](https://ardalis.com/)

Bootstrap è attualmente il framework di web più diffuso per lo sviluppo di applicazioni web reattiva. Offre un numero di funzionalità e i vantaggi che possono migliorare l'esperienza degli utenti con il sito web, se più esperti nella progettazione front-end e di sviluppo o di un esperto. Bootstrap viene distribuito come un set di file CSS e JavaScript ed è progettato per la scala del sito Web o applicazione in modo efficiente dai telefoni a Tablet ai desktop.

## <a name="getting-started"></a>Per iniziare

Esistono diversi modi per iniziare con Bootstrap. Se si inizia una nuova applicazione web in Visual Studio, è possibile scegliere il modello di avvio predefinito per ASP.NET di base, in cui verrà vengono preinstallati Bootstrap case:

![Avvio automatico di visualizzazione di Esplora modello starter](bootstrap/_static/bootstrap-in-starter-template.png)

Aggiunta di Bootstrap per un ASP.NET Core progetto è sufficiente aggiungerli alla *bower. JSON* come dipendenza:

[!code-json[Main](../common/samples/WebApplication1/bower.json?highlight=5)]

Questo è il modo consigliato per aggiungere un progetto ASP.NET Core di Bootstrap.

È inoltre possibile installare mediante uno dei diversi gestori di pacchetti, ad esempio Bower, npm o NuGet bootstrap. In ogni caso, il processo è essenzialmente lo stesso:

### <a name="bower"></a>Bower

```console
bower install bootstrap
```

### <a name="npm"></a>npm

```console
npm install bootstrap
```

### <a name="nuget"></a>NuGet

```console
Install-Package bootstrap
```

> [!NOTE]
> Il metodo consigliato per installare le dipendenze del client come Bootstrap di ASP.NET Core tramite Bower (utilizzando *bower. JSON*, come illustrato in precedenza). L'utilizzo di npm/NuGet vengono visualizzati per illustrare come Bootstrap può essere facilmente aggiunte altri tipi di applicazioni web, incluse le versioni precedenti di ASP.NET.

Se viene fatto riferimento proprie versioni locali del Bootstrap, è necessario farvi riferimento in tutte le pagine che verranno utilizzato. Nell'ambiente di produzione è necessario fare riferimento tramite una rete CDN bootstrap. Nel modello di sito ASP.NET predefinito, il *layout. cshtml* file così come segue:

[!code-html[Main](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Se prevede di utilizzare uno qualsiasi dei plug-in di jQuery del Bootstrap, è necessario inoltre fare riferimento a jQuery.

## <a name="basic-templates-and-features"></a>Funzionalità e i modelli di base

Il modello di Bootstrap di base è molto simile a quello di *layout. cshtml* file illustrato sopra, in modo semplice e include un menu di base per la navigazione e una posizione in cui eseguire il rendering il resto della pagina.

### <a name="basic-navigation"></a>Navigazione di base

Il modello predefinito utilizza un set di `<div>` elementi per il rendering di una barra di spostamento superiore e il corpo principale della pagina. Se si utilizza HTML5, è possibile sostituire il primo `<div>` tag con un `<nav>` tag per ottenere lo stesso effetto, ma con una semantica più precisa. All'interno di questo primo `<div>` è possibile visualizzare esistono molti altri. Innanzitutto, un `<div>` con una classe "container", quindi all'interno di tale tipo, più due `<div>` elementi: "navbar-header" e "barra di spostamento e compressione". Tag div barra di spostamento intestazione include un pulsante che verrà visualizzato quando la schermata è di sotto di una determinata larghezza minima, mostrare 3 linee orizzontali (una cosiddetta "icona pulsante"). L'icona viene eseguito il rendering con pure HTML e CSS. Nessuna immagine è obbligatorio. Questo è il codice che viene visualizzata l'icona, con ogni il <span> tag per il rendering di una delle barre bianche:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Include inoltre il nome dell'applicazione, che viene visualizzata nella parte superiore sinistra. Il menu di navigazione principale viene eseguito il rendering per il `<ul>` elemento all'interno del tag div, secondo e i collegamenti per circa, Home e contattare. Collegamenti aggiuntivi per l'account di accesso e di registro vengono aggiunti per la riga loginpartial nella riga 29. Di sotto di spostamento, il corpo principale di ogni pagina viene eseguito il rendering in un altro `<div>`, contrassegnata con le classi "container" e "contenuto del corpo". Nel file layout predefinito semplice illustrato di seguito, il contenuto della pagina di cui esegue il rendering specifica visualizzazione associata con la pagina e quindi una semplice `<footer>` viene aggiunto alla fine del `<div>` elemento. È possibile visualizzare come predefinito sulla pagina viene visualizzata utilizzando questo modello:

![Informazioni sulla pagina](bootstrap/_static/about-page-wide.png)

La barra di spostamento compressa, con il pulsante "pulsante" in alto a destra, viene visualizzata quando la finestra scende di sotto di una determinata larghezza:

![informazioni sulla pagina con menu tre linee](bootstrap/_static/about-page-hamburger.png)

Facendo clic sull'icona rivela le voci di menu in un cassetto verticale che scorre verso il basso dalla parte superiore della pagina:

![informazioni sulla pagina con tre linee menu espanso](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Tipografia e collegamenti

Bootstrap Imposta base tipografia, colori e la formattazione nel file CSS di collegamento del sito. Questo file CSS include gli stili predefiniti per le tabelle, i pulsanti, gli elementi form, immagini e altro ancora ([ulteriori](http://getbootstrap.com/css/)). Una funzionalità particolarmente utile è il sistema di layout di griglia, coperto accanto.

### <a name="grids"></a>Griglie

Una delle funzionalità più comuni di Bootstrap è un sistema di layout di griglia. Applicazioni web moderne consigliabile evitare di utilizzare il `<table>` tag per il layout, invece di limitare l'utilizzo di questo elemento ai dati tabulari effettivi. Invece, colonne e righe possono essere disposti utilizzando una serie di `<div>` elementi e le classi CSS appropriate. Esistono diversi vantaggi di questo approccio, inclusa la possibilità di modificare il layout di griglia per visualizzare in verticale nelle schermate narrow, ad esempio nei telefoni.

[Sistema di layout di griglia del bootstrap](http://getbootstrap.com/css/#grid) si basa su dodici colonne. Questo numero è stato scelto perché possono essere suddivisi in modo uniforme in 1, 2, 3 o 4 colonne e la larghezza delle colonne può variare all'interno di 1/12 della larghezza verticale dello schermo. Per iniziare a utilizzare il sistema di layout di griglia, è consigliabile iniziare con un contenitore `<div>` e quindi aggiungere una riga `<div>`, come illustrato di seguito:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Successivamente, aggiungere ulteriori `<div>` elementi per ogni colonna e specificare il numero di colonne che `<div>` deve occupare (fuori 12) come parte di una classe CSS a partire da "col - md-". Ad esempio, se si desidera semplicemente contenere due colonne della stessa dimensione, utilizzare una classe di "col-md-6" per ciascuno di essi. In questo caso "md" è l'abbreviazione per "medio" e fa riferimento alle dimensioni del display dimensioni standard computer desktop. Sono disponibili quattro opzioni diverse, che è possibile scegliere tra e ognuno verrà utilizzato per larghezza superiore a meno che non viene sottoposto a override (in modo se si desidera il layout da correggere indipendentemente dalla larghezza dello schermo, è possibile specificare solo classi xs).

Prefisso di classe CSS | Livello di dispositivo | Larghezza
:---: | :---: | :---:
col-xs- | Telefoni | < 768px
col-sm - | Tablet | >= 768px
col-md- | Desktop | >= 992px
col-lg - | Consente di visualizzare Desktop più grande | >= 1200px

Quando si specificano due colonne entrambi con "6-md-col" layout risultante sarà due colonne con risoluzioni desktop, ma queste due colonne verranno disposti in verticale quando sottoposto a rendering in dispositivi di piccole dimensioni (o una finestra del browser più ristretta su un computer desktop), consentendo agli utenti di visualizzare facilmente contenuto senza dover scorrere orizzontalmente.

Bootstrap verrà sempre per impostazione predefinita un layout a colonna singola, pertanto è necessario specificare le colonne quando si desidera più di una colonna. L'unica volta si desidera specificare in modo esplicito che un `<div>` occupato tutte le colonne di 12, è possibile ignorare il comportamento di un maggiore livello di dispositivo. Quando si specificano più classi di livello di dispositivo, potrebbe essere necessario reimpostare il rendering delle colonne in determinati momenti. Aggiunta di un elemento div clearfix che è visibile all'interno di un determinato riquadro di visualizzazione solo possibile eseguire questa operazione, come illustrato di seguito:

![griglia viewport "narrow" e "wide"](bootstrap/_static/narrow-and-wide-viewport-grid.png)

Nell'esempio precedente, uno e due condividere una riga nel layout "md", mentre due e tre condividono una riga nel layout "xs". Senza il clearfix `<div>`, due e tre non vengono visualizzati correttamente nella vista "xs" (si noti che vengono visualizzati solo uno, quattro e cinque):

![griglia senza utilizzare clearfix](bootstrap/_static/grid-without-clearfix.png)

In questo esempio, una sola riga `<div>` è stato usato, e ancora Bootstrap ha principalmente giusto per quanto riguarda il layout e la sovrapposizione delle colonne. In genere, è necessario specificare una riga `<div>` per ogni riga orizzontale richiede il layout e naturalmente è possibile annidare Bootstrap griglie dello stesso. Quando si esegue l'operazione, ogni griglia nidificata occupano 100% della larghezza dell'elemento in cui è inserito, che può essere suddiviso utilizzando le classi di colonna.

### <a name="jumbotron"></a>Jumbotron

Se si utilizza i modelli MVC ASP.NET predefiniti in Visual Studio 2012 o 2013, si sarà notato Jumbotron nell'azione. Fa riferimento a una grande quantità di larghezza massima di una pagina che può essere utilizzata per visualizzare un'immagine di sfondo di grandi dimensioni, una chiamata a azione, un ciclo o elementi simili. Per aggiungere un jumbotron a una pagina, aggiungere semplicemente un `<div>` e assegnargli una classe di "jumbotron", quindi posizionare un contenitore `<div>` all'interno e aggiungere il contenuto. È possibile regolare facilmente lo standard sulla pagina da utilizzare un jumbotron per le intestazioni principale che visualizza:

![esempio Jumbotron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Pulsanti

Le classi del pulsante predefinito e i colori vengono visualizzati nella figura riportata di seguito.

![pulsanti con tema](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Notifiche

Badge, consultare le didascalie di piccole dimensioni, in genere numeriche accanto a un elemento di navigazione. È possibile indicare un numero di messaggi o notifiche in attesa o la presenza di aggiornamenti. Specifica di tale badge è semplice come l'aggiunta di un <span> contenente il testo, con una classe di "notifica":

![badge con tema](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Avvisi

Potrebbe essere necessario visualizzare un tipo di notifica, avviso o messaggio di errore per gli utenti dell'applicazione. Ovvero in cui le classi di avviso standard sono utili. Sono disponibili quattro diversi livelli di gravità con combinazioni di colori associato:

![avvisi con tema](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Menu e barre di spostamento

Il layout include già una barra di spostamento standard, ma il tema Bootstrap supporta opzioni di stile aggiuntive. È anche possibile scegliere di visualizzare la barra di spostamento in verticale anziché in orizzontale se è preferibile che, navigazione secondarie nonché come aggiungere gli elementi nel menu a comparsa. Menu di spostamento semplice, ad esempio elenchi di schede, vengono compilati in cima <ul> elementi. È possibile crearli in poche parole fornendo solo le classi CSS "nav" e "nav schede":

![elenchi di schede con tema](bootstrap/_static/theme-tabstrips.png)

Barre di spostamento vengono compilate in modo analogo, ma sono un po' più complessi. Iniziano con un `<nav>` o `<div>` con una classe di "barra di spostamento", in cui un elemento div contenitore contiene il resto degli elementi. La pagina include già una barra di spostamento nella relativa intestazione – quello riportato di seguito si espande semplicemente su questo, aggiunta del supporto per un menu a discesa:

![barre di spostamento con tema](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Elementi aggiuntivi

Il tema predefinito può anche essere utilizzato per presentare tabelle HTML in uno stile formattato in modo appropriato, incluso il supporto per le viste con striping. Sono presenti etichette con stili che sono simili a quelle dei pulsanti. È possibile creare menu a discesa personalizzati che supportano le opzioni di stile aggiuntive oltre il codice HTML standard `<select>` elemento nonché attraenti simile a quello di sito di avvio predefinito è già in uso. Se è necessario un indicatore di stato, sono disponibili diversi stili da scegliere, nonché elencare i gruppi e pannelli che includono un titolo e il contenuto. Esplorare le opzioni aggiuntive di seguito il tema Bootstrap standard:

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Più temi

È possibile estendere il tema di Bootstrap standard eseguendo l'override di alcuni o tutti i relativi CSS, modificare i colori e stili in base alle esigenze dell'applicazione. Se si desidera iniziare da un tema predefinito, sono disponibili diverse raccolte di tema disponibile online che specializzare Bootstrap dei temi, ad esempio WrapBootstrap.com (che include una varietà di temi commerciali) e Bootswatch.com (che offre i temi disponibili). Alcuni dei modelli di pagamento disponibili forniscono numerose funzionalità disponibile per il tema di Bootstrap base, ad esempio un supporto avanzato per amministrativi menu e i dashboard con i misuratori e grafici avanzati. Un esempio di un modello di pagamento comune è Inspinia, attualmente in vendita per $18, che include un modello di ASP.NET MVC5 oltre AngularJS e alle versioni HTML statiche. Di seguito è riportata una schermata di esempio.

![Esempio tema inspinia](bootstrap/_static/theme-inspinia.png)

Se si desidera modificare il tema Bootstrap, inserire il *bootstrap.css* file per il tema desiderato di **wwwroot/css** cartella e modificare i riferimenti in *layout. cshtml* per puntare. Modificare i collegamenti per tutti gli ambienti:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Se si desidera creare un dashboard personalizzato, è possibile avviare dell'esempio gratuita disponibile qui: [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Componenti

Oltre a tali elementi già descritti, Bootstrap include il supporto per un'ampia gamma di [componenti dell'interfaccia utente predefiniti](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Bootstrap include set di icone da Glyphicons ([http://glyphicons.com](http://glyphicons.com)), con oltre 200 icone disponibile gratuitamente per l'utilizzo all'interno dell'applicazione web abilitata Bootstrap. Di seguito è riportato un esempio di piccole dimensioni:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>gruppi di input

Gruppi di input consentono l'aggiunta di testo aggiuntivi o i pulsanti con un elemento di input, fornisce all'utente un'esperienza più intuitiva:

![gruppi di input](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Controlli di navigazione

Controlli di navigazione sono un componente dell'interfaccia utente comune utilizzato per illustrare all'utente loro cronologia recente o la profondità della gerarchia di navigazione di un sito. Aggiungere facilmente applicando la classe "navigazione" a qualsiasi `<ol>` elemento elenco. Include il supporto incorporato per la paginazione tramite la classe "paginazione" su un `<ul>` elemento all'interno di un `<nav>`. Aggiungere presentazioni incorporato reattiva e video usando `<iframe>`, `<embed>`, `<video>`, o `<object>` elementi, Bootstrap verrà stile automaticamente. Specificare un particolare proporzioni utilizzando classi specifiche come "incorporare-reattiva-16by9".

## <a name="javascript-support"></a>Supporto di JavaScript

Libreria JavaScript del bootstrap include il supporto di API per i componenti inclusi, che consente di controllare il comportamento a livello di codice all'interno dell'applicazione. Inoltre, *bootstrap.js* include decine plug-in personalizzato di jQuery, fornendo funzionalità aggiuntive come transizioni, finestre di dialogo modale, scorrere verso il rilevamento (aggiornamento degli stili in base a cui l'utente è stata fatta scorrere nel documento), comportamento di compressione, nastri e apposizione menu nella finestra in modo che non è possibile scorrere lo schermo. Non vi sia spazio sufficiente per coprire tutti i componenti aggiuntivi JavaScript incorporati Bootstrap: per ulteriori informazioni, visitare [http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Riepilogo

Bootstrap fornisce un framework web che può essere utilizzato per produttivo e rapidamente il layout e applicare uno stile di una vasta gamma di applicazioni e siti Web. La funzionalità tipografiche di base e gli stili forniscono un piacevole aspetto che possono essere modificato facilmente tramite supporto dei temi personalizzati, che è possibile prevedere o acquistato in commercio. Supporta una serie di componenti web che in precedenza richiede controlli di terze parti costosi da eseguire, supportando gli standard web moderno e aprire.
