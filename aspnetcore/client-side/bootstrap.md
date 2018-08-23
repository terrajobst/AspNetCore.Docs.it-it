---
title: Compilazione di siti efficienti e reattivi con Bootstrap e ASP.NET Core
author: ardalis
description: Informazioni su come usare Bootstrap per lo sviluppo di App web reattive con ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: client-side/bootstrap
ms.openlocfilehash: 1ccf33b299739f5aa963a53feb70b44b290443ca
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835878"
---
# <a name="build-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>Compilazione di siti efficienti e reattivi con Bootstrap e ASP.NET Core

<a name="bootstrap-index"></a>

Di [Steve Smith](https://ardalis.com/)

Bootstrap è attualmente il framework web più diffuso per lo sviluppo di applicazioni web reattiva. Offre numerose funzionalità e sui vantaggi che consentono di migliorare l'esperienza degli utenti con il sito web, se sei un principiante alla progettazione front-end e lo sviluppo e gli esperti. Bootstrap viene distribuito come un set di file CSS e JavaScript ed è progettato per il sito Web o un'applicazione di scalabilità in modo efficiente da telefoni a Tablet ai desktop.

## <a name="get-started"></a>Introduzione

Esistono diversi modi per iniziare a usare Bootstrap. Se si inizia una nuova applicazione web in Visual Studio, è possibile scegliere il modello di avvio predefinito per ASP.NET Core, in cui maiuscole Bootstrap proverranno pre-installato:

![Eseguire il bootstrap in visualizzazione soluzione modello starter](bootstrap/_static/bootstrap-in-starter-template.png)

Aggiunta di Bootstrap per un ASP.NET Core progetto è sufficiente aggiungerla alla *bower. JSON* come dipendenza:

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

Questo è il modo consigliato per aggiungere Bootstrap per un progetto ASP.NET Core.

È anche possibile installare usando uno dei diversi strumenti di gestione di pacchetti, ad esempio Bower, npm e NuGet di bootstrap. In ogni caso, il processo è essenzialmente lo stesso:

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
> Il metodo consigliato per installare le dipendenze dal lato client come Bootstrap in ASP.NET Core è tramite Bower (mediante *bower. JSON*, come illustrato in precedenza). L'uso di NuGet o npm vengono visualizzati per dimostrare con quanta facilità Bootstrap possono essere aggiunti ad altri tipi di applicazioni web, incluse le versioni precedenti di ASP.NET.

Se viene fatto riferimento proprie versioni locali di Bootstrap, è necessario farvi riferimento in tutte le pagine che verranno usato. Nell'ambiente di produzione si deve fare riferimento a bootstrap con una rete CDN. Nel modello di sito di ASP.NET Core predefinito, il *layout. cshtml* file in modo simile al seguente:

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Se si intende usare uno qualsiasi dei plug-in jQuery di Bootstrap, è necessario anche fare riferimento a jQuery.

## <a name="basic-templates-and-features"></a>Funzionalità e i modelli di base

Il modello più semplice di Bootstrap è molto simile il *layout. cshtml* file illustrato sopra, nonché semplicemente include un menu di base per la navigazione e una posizione da cui eseguire il rendering il resto della pagina.

### <a name="basic-navigation"></a>Navigazione di base

Il modello predefinito Usa un set di `<div>` elementi per il rendering di una barra di spostamento superiore e il corpo principale della pagina. Se si usa HTML5, è possibile sostituire il primo `<div>` contrassegnare con un `<nav>` tag per ottenere lo stesso effetto, ma con una semantica più precisa. All'interno di questo primo `<div>` noterete esistono molti altri. Prima di tutto una `<div>` con una classe di "container", quindi entro tale, altri due `<div>` elementi: "navbar-header" e "navbar-collapse". Il tag div navbar-intestazione include un pulsante che verrà visualizzato quando lo schermo è di sotto di una determinata larghezza minima, con 3 linee orizzontali (un cosiddetto "icona di hamburger"). L'icona viene eseguito il rendering mediante puro codice HTML e CSS. non è necessaria alcuna immagine. Questo è il codice che viene visualizzata l'icona, con ognuna del <span> tag per il rendering di una delle barre bianche:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Contiene inoltre il nome dell'applicazione, che viene visualizzata in alto a sinistra. Il menu di navigazione principale viene eseguito il rendering per il `<ul>` elemento all'interno di div secondo e include collegamenti a circa, Home e contatto. Di sotto di spostamento, il corpo principale di ogni pagina viene eseguito il rendering in un altro `<div>`, contrassegnata con le classi "container" e "contenuto del corpo". Nella semplice predefinita \_riportati di seguito, il contenuto della pagina viene sottoposti a rendering dalla visualizzazione specifica associata con la pagina e quindi un semplice file di Layout `<footer>` viene aggiunto alla fine della `<div>` elemento. È possibile visualizzare come l'elemento predefinito sulla pagina viene visualizzata usando questo modello:

![Informazioni sulla pagina](bootstrap/_static/about-page-wide.png)

La barra di spostamento compressa, con il pulsante "hamburger" in alto a destra, viene visualizzato quando la finestra scende sotto una determinata larghezza:

![pagina About con il menu hamburger](bootstrap/_static/about-page-hamburger.png)

Facendo clic sull'icona, vengono visualizzate le voci di menu in un drawer verticale che scorre verso il basso dalla parte superiore della pagina:

![pagina About con il menu hamburger espanso](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Tipografia e collegamenti

Bootstrap Configura funzionalità tipografiche di base del sito, colori e collegamento di formattazione nel file CSS. Questo file CSS include gli stili predefiniti per le tabelle, i pulsanti, elementi del form, immagini e altro ancora ([altre informazioni](http://getbootstrap.com/css/)). Una funzionalità particolarmente utile è il sistema di layout griglia, illustrato di seguito.

### <a name="grids"></a>Griglie

Una delle funzionalità più note di Bootstrap è il sistema di layout di griglia. Applicazioni web moderne consigliabile evitare di usare il `<table>` tag per il layout, invece limitando l'uso di questo elemento di dati effettivi in formato tabulare. Al contrario, righe e colonne possono essere disposti usando una serie di `<div>` elementi e le classi CSS appropriate. Esistono diversi vantaggi di questo approccio, inclusa la possibilità di adattare il layout delle griglie in visualizzazione verticale negli schermi stretti, come nei telefoni.

[Sistema di layout di griglia di bootstrap](http://getbootstrap.com/css/#grid) si basa su dodici colonne. Questo numero è stato scelto perché possono essere suddivisi in modo uniforme in 1, 2, 3 o 4 colonne, e può variare la larghezza delle colonne all'interno di 1/12th della larghezza verticale dello schermo. Per iniziare a usare il sistema di layout di griglia, è consigliabile iniziare con un contenitore `<div>` e quindi aggiungere una riga `<div>`, come illustrato di seguito:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Successivamente, aggiungere ulteriori `<div>` elementi per ogni colonna e specificare il numero di colonne che `<div>` deve occupare (su 12) come parte di una classe CSS inizia con "col - md-". Ad esempio, se si desidera semplicemente avere due colonne di dimensioni uguali, si potrebbe usare una classe di "col-md-6" per ognuno di essi. In questo caso "md" è l'acronimo di "medium" e fa riferimento a dimensioni del display dimensioni standard computer desktop. Sono disponibili quattro opzioni diverse, che è possibile scegliere tra e ognuno verrà usato per la larghezza delle versioni successive a meno che non viene sottoposto a override (in modo che se si desidera che il layout risolvibili indipendentemente dalla larghezza dello schermo, è possibile specificare solo classi xs).

Prefisso di classe CSS | Livello di dispositivo | Larghezza
:---: | :---: | :---:
col-xs: | Telefoni | < 768px
col: sm - | Tablet | >= 768px
col-md - | Computer desktop | >= 992px
col-generatore di carico: | Consente di visualizzare Desktop più grande | >= 1200px

Quando si specificano due colonne entrambe con "col-md-6" il layout risulta sarà di due colonne con risoluzioni desktop, ma queste due colonne vengono impilate in verticale quando viene eseguito il rendering in dispositivi di piccole dimensioni o una finestra del browser più ristretta in un computer desktop, consentendo agli utenti di visualizzare facilmente contenuto senza la necessità di scorrere orizzontalmente.

Bootstrap verrà sempre per impostazione predefinita un layout di colonna singola, pertanto è necessario solo specificare le colonne quando si desidera che più di una colonna. L'unica volta è consigliabile specificare in modo esplicito che un `<div>` take backup di tutti i 12 colonne, è possibile ignorare il comportamento di un maggiore livello di dispositivo. Quando si specificano più classi di livello di dispositivo, potrebbe essere necessario reimpostare il rendering delle colonne in determinati momenti. Aggiunta di un elemento div clearfix che è visibile all'interno di un determinato riquadro di visualizzazione solo possibile ottenere questo risultato, come illustrato di seguito:

![griglia viewport Narrow e "wide"](bootstrap/_static/narrow-and-wide-viewport-grid.png)

Nell'esempio precedente, uno e due condividono una riga nel layout "md", mentre due e tre condividono una riga nel layout "xs". Senza il clearfix `<div>`, due e tre non vengono visualizzati correttamente nella visualizzazione di "xs" (si noti che vengono visualizzati solo uno, quattro e cinque):

![griglia senza usare clearfix](bootstrap/_static/grid-without-clearfix.png)

In questo esempio, solo una singola riga `<div>` è stato usato, e mantenere comunque Bootstrap ha principalmente la cosa giusta per quanto riguarda il layout e la sovrapposizione delle colonne. In genere, è necessario specificare una riga `<div>` per ogni riga orizzontale richiede il layout e naturalmente è possibile annidare Bootstrap griglie dello stesso. Quando esegue questa operazione, ogni griglia annidato occuperà 100% della larghezza dell'elemento in cui si trova, che può essere suddivise usando le classi di colonna.

### <a name="jumbotron"></a>Jumbotron

Se hai già usato i modelli di ASP.NET Core MVC predefinita in Visual Studio 2012 o 2013, si sarà notato Jumbotron in azione. Fa riferimento a una sezione a larghezza intera di grandi dimensioni di una pagina che può essere utilizzata per visualizzare un'immagine di sfondo di grandi dimensioni, una chiamata di azione, una rotazione degli elementi o elementi simili. Per aggiungere un jumbotron a una pagina, è sufficiente aggiungere una `<div>` e assegnargli una classe di "jumbotron", quindi posizionare un contenitore `<div>` all'interno e Aggiungi il contenuto. È possibile regolare facilmente lo standard sulla tabella da utilizzare un jumbotron per le intestazioni principale che visualizza:

![esempio Jumbotron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Pulsanti

Le classi del pulsante predefinito e i colori vengono visualizzati nella figura seguente.

![pulsanti con tema](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Notifiche

Notifiche di vedere i callout di piccole dimensioni, in genere numerici accanto a un elemento di navigazione. È possibile indicare un numero di messaggi o le notifiche in attesa o la presenza di aggiornamenti. Specifica di tali medaglie è semplice come aggiungere un `<span>` contenente il testo, con una classe di "badge":

![notifiche con tema](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Avvisi

Potrebbe essere necessario visualizzare un tipo di notifica, avviso o messaggio di errore per gli utenti dell'applicazione. Ovvero in cui le classi standard di avviso sono utili. Esistono quattro diversi livelli di gravità con gli schemi di colore associato:

![avvisi con tema](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Menu e barre di spostamento

Il layout include già una barra di spostamento standard, ma il tema Bootstrap supporta opzioni di stile aggiuntive. Inoltre facilmente, è possibile scegliere di visualizzare la barra di spostamento in senso verticale anziché orizzontalmente se che ha preferito, navigazione secondarie anche l'aggiunta di elementi nel menu a comparsa. Menu di spostamento semplice, ad esempio elenchi di schede, vengono compilati su `<ul>` elementi. È possibile creare tali molto semplicemente fornendo semplicemente li con le classi CSS "spostamento" e "barra di spostamento a schede":

![elenchi di schede con tema](bootstrap/_static/theme-tabstrips.png)

Barre di spostamento vengono compilate in modo analogo, ma sono un po' più complessi. Inizino con un `<nav>` o `<div>` con una classe di "nella barra di spostamento", all'interno del quale un elemento div contenitore contiene il resto degli elementi. La pagina include già una barra di spostamento nella relativa intestazione, quello riportato di seguito si espande semplicemente a disegnarvi sopra, aggiunta del supporto per un menu a discesa:

![eventuali temi delle barre di spostamento](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Elementi aggiuntivi

Il tema predefinito è anche utilizzabile per presentare le tabelle HTML in uno stile ben formattato, incluso il supporto per le viste con striping. Sono presenti etichette con gli stili che sono simili a quelle dei pulsanti. È possibile creare menu a discesa personalizzati che supportano le opzioni di stile aggiuntive oltre l'HTML standard `<select>` elemento nonché attraenti simile a quello sta già usando il sito iniziale predefinito. Se è necessario un indicatore di stato, esistono diversi stili tra cui scegliere, nonché elencare i gruppi e i pannelli che includono un titolo e il contenuto. Esplora le opzioni aggiuntive in questo caso il tema Bootstrap standard:

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Più temi

È possibile estendere il tema Bootstrap standard eseguendo l'override di alcuni o tutti i file CSS relativo, regolare i colori e stili in base alle esigenze della propria applicazione. Se si desidera iniziare da un tema pronte all'uso, sono disponibili diverse raccolte di tema disponibile online che specializzano nei temi Bootstrap, ad esempio WrapBootstrap.com (che presenta numerosi temi commerciali) e Bootswatch.com (che offre i temi gratuiti). Alcuni dei modelli a pagamento disponibili offrono una notevole di funzionalità disponibile per il tema Bootstrap base, ad esempio supporto avanzato per i menu amministrativi e i dashboard con i misuratori e grafici avanzati. Un esempio di un modello di pagamento più diffusi è Inspinia, mostrata nello screenshot seguente:

![Esempio tema inspinia](bootstrap/_static/theme-inspinia.png)

Se si desidera modificare il tema Bootstrap, inserire il *bootstrap* file per il tema desiderato nella **wwwroot/css** cartella e modificare i riferimenti nelle *layout. cshtml* per puntare. Modificare i collegamenti per tutti gli ambienti:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Se si desidera creare un dashboard personalizzato, è possibile iniziare dall'esempio gratuito disponibile qui: [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Componenti

Oltre a tali elementi già illustrati, Bootstrap include il supporto per svariati [componenti dell'interfaccia utente incorporati](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Bootstrap include set di icone da Glyphicons ([http://glyphicons.com](http://glyphicons.com)), con oltre 200 icone disponibili gratuitamente per l'uso all'interno dell'applicazione web compatibili con Bootstrap. Qui è solo un piccolo campione:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>Gruppi di input

Gruppi di input consentono l'aggregazione di testo aggiuntivi o i pulsanti con un elemento di input, consentendo così all'utente un'esperienza più intuitiva:

![gruppi di input](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Percorsi di navigazione

Percorsi di navigazione sono un componente dell'interfaccia utente comune usato per illustrare all'utente la cronologia recente o profondità della gerarchia di navigazione di un sito. Aggiungi con facilità applicando la classe "breadcrumb" a qualsiasi `<ol>` dell'elemento di elenco. Include il supporto incorporato per la paginazione tramite la classe "paginazione" in un `<ul>` elemento all'interno di un `<nav>`. Aggiungere video e presentazioni embedded reattiva usando `<iframe>`, `<embed>`, `<video>`, o `<object>` elementi, che verrà Bootstrap di applicare uno stile automaticamente. Specificare un particolare proporzioni usando classi specifiche, ad esempio "incorporare-velocità di risposta-16by9".

## <a name="javascript-support"></a>Supporto per JavaScript

Libreria JavaScript di bootstrap include il supporto API per i componenti inclusi, consentendo di controllare il comportamento a livello di codice all'interno dell'applicazione. È inoltre *bootstrap. js* include più di una dozzina plug-in jQuery personalizzati, che fornisce funzionalità aggiuntive come le transizioni, finestre di dialogo modale, scorrere verso il rilevamento (aggiornamento gli stili di base in cui l'utente ha fatto scorrere nel documento), comportamento di compressione, sui nastri e menu fissaggio alla finestra in modo da non escono dalla schermata. Non c'è spazio sufficiente per coprire tutti i componenti aggiuntivi JavaScript integrati in Bootstrap – per altre informazioni, visitare [ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Riepilogo

Bootstrap fornisce un framework web che può essere utilizzato per rapidamente e in modo produttivo il layout e applicare uno stile a un'ampia gamma di applicazioni e siti Web. La funzionalità tipografiche di base e gli stili forniscono un piacevole aspetto che può essere modificato facilmente grazie al supporto di tema personalizzato, che può essere creato manualmente o acquistato in commercio. Supporta una serie di componenti web che in precedenza sarebbe stato necessario costosa controlli di terze parti per portare a termine, supportando gli standard web moderni e open.
