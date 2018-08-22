---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: URL nelle pagine Master (VB) | Microsoft Docs
author: rick-anderson
description: Illustra come gli URL nella pagina master possono interrompersi a causa di un file della pagina master in un'altra directory relativa rispetto alla pagina contenuta. Esamina la riassegnazione...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 881debeeaa98a7f2be7ccadb501c019e698b22f2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833323"
---
<a name="urls-in-master-pages-vb"></a>URL nelle pagine Master (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> Illustra come gli URL nella pagina master possono interrompersi a causa di un file della pagina master in un'altra directory relativa rispetto alla pagina contenuta. Esamina la riassegnazione degli URL tramite ~ la sintassi dichiarativa e l'uso ResolveUrl e ResolveClientUrl a livello di codice. (Vedere anche


## <a name="introduction"></a>Introduzione

In tutti gli esempi abbiamo visto finora, che le pagine master e di contenuto hanno si trovino nella stessa cartella (cartella radice del sito Web). Ma non c'è nessun motivo per cui le pagine master e di contenuto devono essere nella stessa cartella. È certamente possibile creare pagine di contenuto nelle sottocartelle. Analogamente, è possibile creare un `~/MasterPages/` cartella in cui posizionare le pagine master del sito.

Un possibile problema con l'inserimento di pagine master e di contenuto in cartelle diverse implica URL interrotto. Se la pagina master contiene tutti gli URL dei collegamenti ipertestuali, immagini o altri elementi, il collegamento sarà valido per le pagine di contenuto che si trovano in una cartella diversa. In questa esercitazione verranno esaminati l'origine di questo problema, nonché le soluzioni alternative.

## <a name="the-problem-with-relative-urls"></a>Il problema con gli URL relativi

Un URL in una pagina web viene definito un' *URL relativo* se il percorso della risorsa cui punta è relativo alla posizione della pagina web nella struttura di cartelle del sito Web. Qualsiasi URL che non inizia con una barra rovesciata iniziale (`/`) o un protocollo (ad esempio `http://`) è relativo perché viene risolto dal browser in base alla posizione della pagina web che contiene l'URL.

Ad esempio, il nostro sito Web ha un `~/Images/` cartella con un unico file di immagine, `PoweredByASPNET.gif`. Il file pagina master `Site.master` ha un `<img>` elemento il `footerContent` area con il markup seguente:


[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

Il `src` nel valore dell'attributo di `<img>` elemento è un URL relativo, dal momento che non iniziano con `/` o `http://`. In breve, il `src` valore dell'attributo indica al browser di esaminare il `Images` sottocartella per un file denominato `PoweredByASPNET.gif`.

Quando si visita una pagina di contenuto, il markup riportato sopra viene inviato direttamente al browser. Si consiglia di visitare `About.aspx` e visualizzare l'origine HTML inviato al browser. Si noterà che lo stesso markup esatto nella pagina master è stato inviato al browser.


[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

Se la pagina di contenuto si trova nella cartella radice (essendo `About.aspx`) tutto funzioni come previsto poiché non esiste un `Images` sottocartella relativo alla cartella radice. Tuttavia, le cose suddividere se la pagina di contenuto si trova in una cartella diversa da quella della pagina master. Per illustrare questo scenario, creare una sottocartella denominata `Admin`. Successivamente, aggiungere una pagina di contenuto denominata `Default.aspx` per il `Admin` cartella, assicurandosi di associare la nuova pagina per il `Site.master` pagina master.

> [!NOTE]
> Nel [ *specificazione di titolo, metatag e altre intestazioni HTML nella pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) esercitazione è stata creata una classe di pagina base personalizzata denominata `BasePage` che impostato automaticamente il contenuto del titolo pagina (se si non è stato esplicitamente assegnato). Non dimenticare di derivano dalla classe code-behind della pagina appena creata `BasePage` in modo che possa usare questa funzionalità.


Dopo aver creato questa pagina di contenuto, Esplora soluzioni sarà simile alla figura 1.


![Una nuova cartella e una pagina ASP.NET sono stati aggiunti al progetto](urls-in-master-pages-vb/_static/image1.png)

**Figura 01**: una nuova cartella e una pagina ASP.NET sono stati aggiunti al progetto


A questo punto, aggiornare il `Web.sitemap` file per includere un nuovo `<siteMapNode>` voce per questa lezione. Il codice XML seguente viene illustrato l'intero `Web.sitemap` markup, che ora include l'aggiunta di un terzo `<siteMapNode>` elemento.


[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

L'oggetto appena creato `Default.aspx` pagina dovrebbe disporre di quattro controlli contenuto corrispondente per i quattro elementi ContentPlaceHolder in `Site.master`. Aggiungere il testo per il controllo contenuto che fa riferimento il `MainContent` ContentPlaceHolder e quindi accedere alla pagina tramite un browser. Come illustrato nella figura 2, il browser non è stato trovato il `PoweredByASPNET.gif` file di immagine. Che cosa succede?

Il `~/Admin/Default.aspx` pagina di contenuto viene inviato il codice HTML stesso per il `footerContent` area, come è stato il `About.aspx` pagina:


[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

Poiché il `<img>` dell'elemento `src` attributo è un URL relativo, il Visualizzatore prova a cercare un `Images` cartella al percorso di cartella della pagina web. In altre parole, il browser sta cercando il file di immagine `Admin/Images/PoweredByASPNET.gif`.


[![Impossibile trovare il File di immagine PoweredByASPNET.gif](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**Figura 02**: il `PoweredByASPNET.gif` immagine del File non trovato ([fare clic per visualizzare l'immagine con dimensioni normali](urls-in-master-pages-vb/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Sostituire gli URL relativi con gli URL assoluti

È l'opposto di un URL relativo un' *URL assoluto*, ovvero una che inizia con una barra rovesciata (`/`) o un protocollo, ad esempio `http://`. Poiché un URL assoluto specifica il percorso di una risorsa da un punto fisso noto, lo stesso URL assoluto è valido in qualsiasi pagina web, indipendentemente dalla posizione della pagina web nella struttura di cartelle del sito Web.

Per risolvere l'immagine interrotta illustrato nella figura 2, è necessario aggiornare il `<img>` dell'elemento `src` attributo in modo che utilizzi un URL assoluto anziché uno relativo. Per determinare l'URL assoluto corretto, visitare una delle pagine web nel sito Web ed esaminare la barra degli indirizzi. Come illustrato nella barra degli indirizzi nella figura 2, il percorso completo per l'applicazione web è `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`. Pertanto, è possibile aggiornare il `<img>` dell'elemento `src` attribuire a uno dei due URL assoluto seguente:

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

Si consiglia di aggiornare il `<img>` dell'elemento `src` dell'attributo a un URL assoluto usando uno dei formati illustrati in precedenza e quindi visitare la `~/Admin/Default.aspx` pagina tramite un browser. Questa volta il browser trovare e visualizzare correttamente il `PoweredByASPNET.gif` file di immagine (vedere la figura 3).


[![L'immagine PoweredByASPNET.gif viene ora visualizzata](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**Figura 03**: il `PoweredByASPNET.gif` immagine è ora visualizzata ([fare clic per visualizzare l'immagine con dimensioni normali](urls-in-master-pages-vb/_static/image7.png))


Anche se codificare in URL assoluto funziona, strettamente collegato del codice HTML al server del sito Web e percorso della cartella, ma potrebbe cambiare. Usando un URL assoluto del form `http://localhost:3908/...` è instabile perché il numero di porta che precede localhost viene selezionato automaticamente ogni volta che viene avviato Visual Studio Development Server Web ASP.NET predefinito. Analogamente, il `http://localhost` parte è valida solo durante il test locale. Una volta il codice viene distribuito in un server di produzione, la base URL cambierà per qualcos'altro, ad esempio `http://www.yourserver.com`. L'URL assoluto sotto forma `/ASPNET_MasterPages_Tutorial_04_VB/...` risente inoltre inconvenienti stesso, poiché spesso questo percorso dell'applicazione è diverso tra i server di sviluppo e produzione.

La buona notizia è che ASP.NET offre un metodo per la generazione di un URL relativo valido in fase di esecuzione.

## <a name="usingandresolveclienturl"></a>Usando`~`e`ResolveClientUrl`

Anziché a livello di codice un URL assoluto, ASP.NET consente agli sviluppatori di pagina da utilizzare il carattere tilde (`~`) per indicare la radice dell'applicazione web. Ad esempio, più indietro in questa esercitazione ho utilizzato la notazione `~/Admin/Default.aspx` nel testo per fare riferimento al `Default.aspx` nella pagina di `Admin` cartella. Il `~` indica che il `Admin` cartella è una sottocartella della directory principale dell'applicazione web.

Il `Control` della classe [ `ResolveClientUrl` metodo](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) accetta un URL e lo modifica a un URL relativo per la pagina web in cui risiede il controllo appropriato. Ad esempio, chiamando `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` dal `About.aspx` restituisce `Images/PoweredByASPNET.gif`. Viene chiamata da `~/Admin/Default.aspx`, tuttavia, viene restituito `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Poiché tutti i controlli server ASP.NET derivano dal `Control` (classe), tutti i controlli server hanno accesso al `ResolveClientUrl` (metodo). Anche il `Page` deriva dalla classe di `Control` (classe), vale a dire che è possibile usare questo metodo direttamente da classi di code-behind di ASP.NET alla pagina.


### <a name="usingin-the-declarative-markup"></a>Usando`~`nel Markup dichiarativo

Diversi controlli Web ASP.NET includono proprietà relative alla URL: il controllo collegamento ipertestuale è un `NavigateUrl` proprietà; l'immagine del controllo ha un `ImageUrl` proprietà; e così via. Quando sottoposto a rendering, questi controlli passano dei relativi valori di proprietà relative a URL `ResolveClientUrl`. Di conseguenza, se queste proprietà contengono una `~` per indicare la radice dell'applicazione web, l'URL verrà modificato in un URL relativo valido.

Tenere presente che solo i controlli server ASP.NET trasformano il `~` delle proprietà dei controlli relativi URL. Se un `~` viene visualizzata nel markup HTML statico, ad esempio `<img src="~/Images/PoweredByASPNET.gif" />`, il motore ASP.NET invia il `~` insieme al resto del contenuto HTML nel browser. Il browser si presuppone che il `~` fa parte dell'URL. Ad esempio, se il browser riceve il markup `<img src="~/Images/PoweredByASPNET.gif" />` presuppone che è presente una sottocartella denominata `~` con una sottocartella `Images` che contiene il file di immagine `PoweredByASPNET.gif`.

Per correggere il tag di immagine nel `Site.master`, sostituire il `<img>` elemento con un controllo Web immagine ASP.NET. Impostare il controllo Web Image `ID` a `PoweredByImage`, la relativa `ImageUrl` proprietà `~/Images/PoweredByASPNET.gif`e il relativo `AlternateText` proprietà su "Basata su ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

Dopo aver apportato questa modifica della pagina master, visitare di nuovo il `~/Admin/Default.aspx` pagina nuovamente. Questa volta il `PoweredByASPNET.gif` file di immagine viene visualizzata nella pagina (vedere la figura 3). Quando il controllo immagine Web viene eseguito il rendering Usa il `ResolveClientUrl` metodo per risolvere i relativi `ImageUrl` valore della proprietà. Nelle `~/Admin/Default.aspx` il `ImageUrl` viene convertito in un URL relativo appropriato, come il seguente frammento di codice HTML sorgente Mostra:


[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> Oltre a essere usata nelle proprietà del controllo Web basato su URL, il `~` può essere utilizzato anche quando si chiama il `Response.Redirect` e `Server.MapPath` metodi, tra gli altri. Inoltre, il `ResolveClientUrl` metodo può essere richiamato direttamente da un ASP.NET o markup dichiarativo della pagina master, se necessario, vedere [Fritz Onion](https://www.pluralsight.com/blogs/fritz/)del post di blog [Using `ResolveClientUrl` nel Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Correzione della pagina Master rimanenti gli URL relativi

Oltre al `<img>` elemento il `footerContent` che è appena stato risolto, la pagina master contiene un URL più relativo che richiede l'attenzione. Il `topContent` area include il collegamento "Master pagine delle esercitazioni," che fa riferimento al `Default.aspx`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

Poiché questo URL è relativo, verrà inviato all'utente di `Default.aspx` pagina nella cartella della pagina contenuto visitato. Per visualizzare questo collegamento sempre puntare `Default.aspx` nella cartella radice è necessario sostituire il `<a>` elemento con un collegamento ipertestuale Web controllare in modo che è possibile usare il `~` notation.

Rimuovere il `<a>` markup dell'elemento e aggiungere un controllo collegamento ipertestuale al suo posto. Impostare il collegamento ipertestuale `ID` al `lnkHome`, la relativa `NavigateUrl` proprietà `~/Default.aspx`e il relativo `Text` proprietà su "Esercitazioni di pagine Master".


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

La procedura è terminata. A questo punto tutti gli URL nella nostra pagina master si basano in modo corretto quando viene eseguito il rendering da una pagina di contenuto indipendentemente dal fatto che le cartelle, le pagine master e del contenuto si trovano.

### <a name="automatic-url-resolution-in-theheadsection"></a>Risoluzione degli URL automatica nel`<head>`sezione

Nel [ *creazione di un Layout a livello di sito tramite le pagine Master* ](creating-a-site-wide-layout-using-master-pages-vb.md) esercitazione è stata aggiunta una `<link>` per il `Styles.css` del file nel `<head>` area:


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

Mentre il `<link>` dell'elemento `href` attributo è relativo, viene automaticamente convertito in un percorso appropriato in fase di esecuzione. Come illustrato nella [ *specificazione di titolo, metatag e altre intestazioni HTML nella pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) esercitazione il `<head>` area è in realtà un controllo lato server, che consente di modificare il contenuto dei relativi controlli interni quando ne viene eseguito il rendering.

Per effettuare questa verifica, rivedere il `~/Admin/Default.aspx` pagina e visualizzare l'origine HTML inviato al browser. Come illustrato nel seguente frammento di codice seguente, il `<link>` dell'elemento `href` attributo modificato automaticamente un URL relativo appropriato, `../Styles.css`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>Riepilogo

Pagine master molto spesso includono i collegamenti, immagini e altre risorse esterne che devono essere specificati tramite un URL. Perché la pagina master e le pagine di contenuto potrebbero non esistere nella stessa cartella, è importante astengono usando gli URL relativi. Sebbene sia possibile usare URL assoluti hardcoded, in questo modo strettamente associa l'URL assoluto per l'applicazione web. Se viene modificato l'URL assoluto - quanto accade spesso quando si sposta o si distribuisce un'applicazione web - è necessario ricordarsi di tornare indietro e aggiornare gli URL assoluti.

L'approccio ideale consiste nell'usare il carattere tilde (`~`) per indicare la radice dell'applicazione. Eseguire il mapping di controlli Web ASP.NET che contengono proprietà relative a URL di `~` alla radice dell'applicazione in fase di esecuzione. Internamente, i controlli Web di usano la `Control` della classe `ResolveClientUrl` metodo per generare un URL relativo valido. Questo metodo è pubblica e disponibili da ogni controllo del server (inclusi i `Page` classe), pertanto è possibile usarlo a livello di codice dalle classi code-behind, se necessario.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Pagine master in ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [La riassegnazione URL in una pagina Master](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Usando `ResolveClientUrl` nel Markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla.com, ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach manualmente ASP.NET 3.5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
> [Successivo](control-id-naming-in-content-pages-vb.md)
