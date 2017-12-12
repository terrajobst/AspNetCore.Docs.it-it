---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: Gli URL nelle pagine Master (VB) | Documenti Microsoft
author: rick-anderson
description: Illustra come possibile interrompere l'URL della pagina master a causa di file della pagina master in un'altra directory relativa rispetto alla pagina contenuta. Esamina la riassegnazione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 603457655e2490e1685f53d2cec643cb9382a59d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="urls-in-master-pages-vb"></a>URL nelle pagine Master (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> Illustra come possibile interrompere l'URL della pagina master a causa di file della pagina master in un'altra directory relativa rispetto alla pagina contenuta. Esamina la riassegnazione degli URL tramite ~ la sintassi dichiarativa e ResolveUrl e ResolveClientUrl a livello di codice. (Si analizzino


## <a name="introduction"></a>Introduzione

In tutti gli esempi abbiamo visto finora, che le pagine master e di contenuto si trovino nella stessa cartella (cartella radice del sito Web). Ma non c'è alcun motivo per cui le pagine master e di contenuto devono essere nella stessa cartella. È certamente possibile creare pagine di contenuto nelle sottocartelle. Analogamente, è possibile creare un `~/MasterPages/` cartella in cui inserire le pagine del sito master.

Un potenziale problema con l'inserimento di pagine master e di contenuto in cartelle diverse implica URL interrotto. Se la pagina master contiene tutti gli URL in collegamenti ipertestuali, immagini o altri elementi, il collegamento sarà valido per le pagine di contenuto che si trovano in una cartella diversa. In questa esercitazione verranno esaminati l'origine di questo problema, nonché soluzioni alternative.

## <a name="the-problem-with-relative-urls"></a>Il problema relativo a tutti gli URL

Un URL in una pagina web viene definito un *URL relativo* se il percorso della risorsa a cui punta è relativo al percorso della pagina web nella struttura di cartelle del sito Web. Qualsiasi URL che non inizia con una barra iniziale (`/`) o un protocollo (ad esempio `http://`) è relativo, perché viene risolto dal browser in base alla posizione della pagina web che contiene l'URL.

Ad esempio, il sito Web è un `~/Images/` cartella con un solo file di immagine, `PoweredByASPNET.gif`. Il file pagina master `Site.master` ha un `<img>` elemento il `footerContent` area con il markup seguente:


[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

Il `src` valore nell'attributo di `<img>` elemento è un URL relativo, in quanto non venga avviato con `/` o `http://`. In breve, il `src` valore dell'attributo indica al browser di esaminare il `Images` sottocartella per un file denominato `PoweredByASPNET.gif`.

Quando si visita una pagina di contenuto, il codice precedente viene inviato direttamente al browser. È opportuno visitare `About.aspx` e visualizzare l'origine HTML inviata al browser. Si noterà che lo stesso esatto markup nella pagina master è stato inviato al browser.


[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

Se la pagina contenuta nella cartella radice (come `About.aspx`) tutto funziona come previsto perché non esiste un `Images` sottocartella nella cartella radice. Tuttavia, operazioni suddividere se la pagina contenuta in una cartella diversa da quella della pagina master. A questo scopo, creare una sottocartella denominata `Admin`. Successivamente, aggiungere una pagina contenuta denominata `Default.aspx` per il `Admin` cartella, assicurandosi di associare la nuova pagina per il `Site.master` pagina master.

> [!NOTE]
> Nel [ *che specifica il titolo, tag Meta e altre intestazioni HTML della pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) esercitazione è stata creata una classe della pagina base personalizzata denominata `BasePage` che imposta automaticamente il titolo della pagina di contenuto (se si non è stato in modo esplicito assegnato). Non dimenticare che derivano dalla classe code-behind della pagina appena creato `BasePage` in modo che possa usare questa funzionalità.


Dopo aver creato questa pagina contenuta, Esplora soluzioni simile alla figura 1.


![Una nuova cartella e una pagina ASP.NET sono stati aggiunti al progetto](urls-in-master-pages-vb/_static/image1.png)

**Figura 01**: una nuova cartella e una pagina ASP.NET sono stati aggiunti al progetto


Aggiornare quindi il `Web.sitemap` file da includere un nuovo `<siteMapNode>` voce per questa lezione. Il codice XML seguente viene illustrato l'intero `Web.sitemap` markup, che include ora l'aggiunta di un terzo `<siteMapNode>` elemento.


[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

L'oggetto appena creato `Default.aspx` pagina deve contenere quattro controlli del contenuto corrispondente per i quattro gli elementi ContentPlaceHolder in `Site.master`. Aggiungere del testo per il controllo contenuto che fa riferimento il `MainContent` ContentPlaceHolder quindi visitare la pagina tramite un browser. Come illustrato nella figura 2, il browser non è possibile trovare il `PoweredByASPNET.gif` file di immagine. Cosa accade in seguito?

Il `~/Admin/Default.aspx` pagina contenuto viene inviata lo stesso HTML per il `footerContent` area, come è stato il `About.aspx` pagina:


[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

Poiché il `<img>` dell'elemento `src` attributo è un URL relativo, il browser tenta di cercare un `Images` cartella rispetto al percorso della cartella della pagina web. In altre parole, il browser esegue la ricerca di file di immagine `Admin/Images/PoweredByASPNET.gif`.


[![Impossibile trovare il File di immagine PoweredByASPNET.gif](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**Figura 02**: il `PoweredByASPNET.gif` immagine Impossibile trovare il File ([fare clic per visualizzare l'immagine ingrandita](urls-in-master-pages-vb/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Sostituzione di tutti gli URL con URL assoluti

È l'opposto di un URL relativo un *URL assoluto*, ovvero uno che inizia con una barra rovesciata (`/`) o un protocollo, ad esempio `http://`. Poiché un URL assoluto specifica il percorso di una risorsa da un punto fisso noto, lo stesso URL assoluto è valido in qualsiasi pagina web, indipendentemente dalla posizione della pagina web nella struttura di cartelle del sito Web.

Per risolvere l'immagine interrotta illustrato nella figura 2, è necessario aggiornare il `<img>` dell'elemento `src` attributo in modo che utilizzi un URL assoluto piuttosto che relativo. Per determinare l'URL assoluto corretto, visitare una delle pagine web nel sito Web ed esaminare la barra degli indirizzi. Come illustrato nella barra degli indirizzi nella figura 2, il percorso completo per l'applicazione web è `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`. Pertanto, è possibile aggiornare il `<img>` dell'elemento `src` attribuire a uno dei due URL assoluto seguenti:

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

È opportuno aggiornare il `<img>` dell'elemento `src` dell'attributo a un URL assoluto usando uno dei formati illustrati in precedenza e quindi visitare il `~/Admin/Default.aspx` pagina tramite un browser. Questa volta il browser trovare e visualizzare correttamente il `PoweredByASPNET.gif` file di immagine (vedere la figura 3).


[![L'immagine di PoweredByASPNET.gif è ora visualizzata](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**Figura 03**: il `PoweredByASPNET.gif` immagine è ora visualizzata ([fare clic per visualizzare l'immagine ingrandita](urls-in-master-pages-vb/_static/image7.png))


Livello di codice nell'URL assoluto funziona, HTML strettamente collegato al server del sito Web e il percorso di cartella, che può cambiare. Utilizzare un URL assoluto nel formato `http://localhost:3908/...` è fragile perché il numero di porta che precede localhost viene selezionato automaticamente ogni volta che viene avviato Visual Studio Development Server Web ASP.NET predefinito. Analogamente, il `http://localhost` parte è valida solo durante il test in locale. Quando il codice viene distribuito un server di produzione, la base dell'URL passerà a un altro titolo, ad esempio `http://www.yourserver.com`. L'URL assoluto nel formato `/ASPNET_MasterPages_Tutorial_04_VB/...` risente inoltre il stessa vulnerabilità poiché spesso questo percorso dell'applicazione è diverso tra i server di sviluppo e produzione.

Buone notizie sono che ASP.NET offre un metodo per la generazione di un URL relativo valido in fase di esecuzione.

## <a name="usingandresolveclienturl"></a>Utilizzando`~`e`ResolveClientUrl`

Anziché a livello di codice un URL assoluto, ASP.NET consente agli sviluppatori di pagina da utilizzare il carattere tilde (`~`) per indicare la radice dell'applicazione web. Ad esempio, più indietro in questa esercitazione si usa la notazione `~/Admin/Default.aspx` nel testo per fare riferimento al `Default.aspx` nella pagina di `Admin` cartella. Il `~` indica che il `Admin` cartella è una sottocartella della radice dell'applicazione web.

Il `Control` della classe [ `ResolveClientUrl` metodo](https://msdn.microsoft.com/en-us/library/system.web.ui.control.resolveclienturl.aspx) acquisisce un URL e lo modifica a un URL relativo appropriato per la pagina web in cui si trova il controllo. Ad esempio, la chiamata `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` da `About.aspx` restituisce `Images/PoweredByASPNET.gif`. Viene chiamata da `~/Admin/Default.aspx`, tuttavia, viene restituito `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Poiché tutti i controlli server ASP.NET derivano dal `Control` (classe), tutti i controlli server hanno accesso al `ResolveClientUrl` metodo. Anche il `Page` deriva dalla classe di `Control` (classe), vale a dire che è possibile utilizzare questo metodo direttamente dalle classi di codice delle pagine ASP.NET.


### <a name="usingin-the-declarative-markup"></a>Utilizzando`~`nel Markup dichiarativo

Diversi controlli Web ASP.NET includono proprietà relative all'URL: il controllo collegamento ipertestuale è un `NavigateUrl` proprietà; l'immagine del controllo dispone di un `ImageUrl` proprietà; e così via. Quando sottoposto a rendering, questi controlli passano i valori delle proprietà correlate a URL `ResolveClientUrl`. Di conseguenza, se queste proprietà contengono un `~` per indicare la radice dell'applicazione web, l'URL verrà modificata in un URL relativo valido.

Tenere presente che solo i controlli server ASP.NET trasformare il `~` nelle proprietà relative all'URL. Se un `~` viene visualizzato nel markup HTML statico, ad esempio `<img src="~/Images/PoweredByASPNET.gif" />`, il motore ASP.NET invia il `~` al browser insieme al resto del contenuto HTML. Il browser si presuppone che il `~` fa parte dell'URL. Ad esempio, se il browser riceve il markup `<img src="~/Images/PoweredByASPNET.gif" />` si presuppone una sottocartella denominata `~` con una sottocartella `Images` che contiene il file di immagine `PoweredByASPNET.gif`.

Per correggere il tag di immagine in `Site.master`, sostituire `<img>` elemento con un controllo ASP.NET immagine Web. Impostare il controllo di immagine Web `ID` a `PoweredByImage`, l'oggetto `ImageUrl` proprietà `~/Images/PoweredByASPNET.gif`e il relativo `AlternateText` proprietà su "Tecnologia ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

Dopo aver apportato questa modifica della pagina master, rivedere il `~/Admin/Default.aspx` pagina nuovamente. Questa volta il `PoweredByASPNET.gif` file di immagine viene visualizzata nella pagina (vedere la figura 3). Quando il controllo immagine Web eseguito il rendering viene utilizzato il `ResolveClientUrl` metodo per risolvere il relativo `ImageUrl` valore della proprietà. In `~/Admin/Default.aspx` il `ImageUrl` viene convertito in un URL relativo appropriato, come il seguente frammento di Mostra origine HTML:


[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> Oltre a essere utilizzata nelle proprietà del controllo Web basato su URL, il `~` può anche essere utilizzati durante la chiamata di `Response.Redirect` e `Server.MapPath` tra gli altri metodi. Inoltre, il `ResolveClientUrl` metodo può essere richiamato direttamente da ASP.NET o markup dichiarativo della pagina master, se necessario, vedere [Fritz Onion](https://www.pluralsight.com/blogs/fritz/)del post di blog [Using `ResolveClientUrl` nel Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Correzione della pagina Master rimanente un URL relativo

Oltre al `<img>` elemento il `footerContent` che è appena stato risolto, la pagina master contiene un URL relativo più che richiede l'attenzione. Il `topContent` area include il collegamento "Master pagine esercitazioni," che fa riferimento al `Default.aspx`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

Poiché questo URL è relativo, verrà inviato all'utente di `Default.aspx` pagina nella cartella della pagina di contenuto visitato. Per visualizzare questo collegamento sempre puntare `Default.aspx` nella cartella radice è necessario sostituire il `<a>` elemento con un collegamento ipertestuale controllare in modo da poter utilizzare il `~` notazione.

Rimuovere il `<a>` tag di elemento e aggiungere un controllo collegamento ipertestuale al suo posto. Impostare il collegamento ipertestuale `ID` a `lnkHome`, l'oggetto `NavigateUrl` proprietà `~/Default.aspx`e il relativo `Text` proprietà su "Esercitazioni su pagine Master".


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

Questo è tutto! A questo punto tutti gli URL in questa pagina master si basano correttamente sottoposta a rendering tramite una pagina contenuto indipendentemente dal fatto le cartelle della pagina master e una pagina di contenuto si trovano.

### <a name="automatic-url-resolution-in-theheadsection"></a>Risoluzione automatica URL il`<head>`sezione

Nel [ *la creazione di un Layout a livello di sito utilizzando le pagine Master* ](creating-a-site-wide-layout-using-master-pages-vb.md) esercitazione è stato aggiunto un `<link>` per il `Styles.css` file nel `<head>` area:


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

Mentre il `<link>` dell'elemento `href` attributo è relativo, viene automaticamente convertito in un percorso appropriato in fase di esecuzione. Come descritto nel [ *che specifica il titolo, tag Meta e altre intestazioni HTML della pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) esercitazione il `<head>` area è effettivamente un controllo sul lato server, che consente di modificare il contenuto dei controlli interni quando è sottoposto a rendering.

Per effettuare questa verifica, rivedere il `~/Admin/Default.aspx` pagina e visualizzare l'origine HTML inviata al browser. Come illustrato nel frammento seguente, il `<link>` dell'elemento `href` attributo è stato modificato automaticamente a un URL relativo appropriato, `../Styles.css`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>Riepilogo

Pagine master molto spesso includono i collegamenti, immagini e altre risorse esterne che devono essere specificate tramite un URL. Perché la pagina master e pagine di contenuto potrebbero non esistere nella stessa cartella, è importante astenersi dall'utilizzo di URL relativi. Anche se è possibile usare gli URL assoluti hardcoded, in questo modo strettamente combina un URL assoluto per l'applicazione web. Se viene modificato l'URL assoluto - quanto accade spesso quando si sposta o distribuzione di un'applicazione web, è necessario ricordare di tornare indietro e aggiornare gli URL assoluti.

L'approccio ideale consiste nell'utilizzare il carattere tilde (`~`) per indicare la radice dell'applicazione. Eseguire il mapping di controlli Web ASP.NET che contengono proprietà relative all'URL di `~` alla radice dell'applicazione in fase di esecuzione. Internamente, i controlli Web di utilizzano il `Control` della classe `ResolveClientUrl` metodo per generare un URL relativo valido. Questo metodo sia pubblico e disponibili da ogni controllo del server (inclusi i `Page` classe), è possibile utilizzarlo a livello di codice da classi il code-behind, se necessario.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Pagine master in ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [La riassegnazione di URL in una pagina Master](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Utilizzando `ResolveClientUrl` nel Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 3.5 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott può essere raggiunto al [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Precedente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
[Successivo](control-id-naming-in-content-pages-vb.md)
