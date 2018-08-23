---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Visualizzazione di una pagina di errore personalizzati (c#) | Microsoft Docs
author: rick-anderson
description: Ciò che viene visualizzato quando si verifica un errore di runtime in un'applicazione web ASP.NET? La risposta dipende dalla modalità del sito Web &lt;customErrors&gt; configurazione...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 449b8eb26f3f6018fdd6c6dcc1de1c8d58214ac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827625"
---
<a name="displaying-a-custom-error-page-c"></a>Visualizzazione di una pagina di errore personalizzati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Ciò che viene visualizzato quando si verifica un errore di runtime in un'applicazione web ASP.NET? La risposta dipende dalla modalità del sito Web &lt;customErrors&gt; configurazione. Per impostazione predefinita, gli utenti vengono visualizzati una schermata di gialla poco eleganti proclaiming che si è verificato un errore di runtime. Questa esercitazione illustra come personalizzare queste impostazioni alla pagina di errore personalizzato visualizzato un esteticamente piacevole corrispondente aspetto del sito.


## <a name="introduction"></a>Introduzione

In un mondo perfetto non sarebbe possibile errori in fase di esecuzione. I programmatori di scrivere il codice propria un bug e con la convalida dell'input utente affidabili ed esterni delle risorse, ad esempio i server di database e server di posta elettronica sarebbero mai offline. Naturalmente, in realtà gli errori sono inevitabili. Le classi in .NET Framework segnalano un errore generando un'eccezione. Ad esempio, la chiamata a un oggetto SqlConnection metodo Open dell'oggetto stabilisce una connessione al database specificato da una stringa di connessione. Tuttavia, se il database è attivo o se le credenziali nella stringa di connessione sono valide quindi il metodo Open genera un `SqlException`. Le eccezioni possono essere gestite mediante l'utilizzo di `try/catch/finally` blocchi. Se il codice all'interno un `try` blocco genera un'eccezione, il controllo viene trasferito al blocco catch appropriato in cui lo sviluppatore può tentare la correzione dell'errore. Se non esiste alcun blocco catch corrispondente oppure se il codice che ha generato l'eccezione non è presente in un blocco try, l'eccezione percolates lo stack di chiamate in search di `try/catch/finally` blocchi.

Se l'eccezione viene propagata fino all'ASP.NET runtime senza essere stato gestito, il [ `HttpApplication` classe](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)del [ `Error` evento](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) viene generato e configurato *pagina di errore*  viene visualizzato. Per impostazione predefinita, ASP.NET consente di visualizzare una pagina di errore affettuoso definito come il [giallo schermata di morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Esistono due versioni del YSOD: Mostra i dettagli dell'eccezione, un'analisi dello stack e altre informazioni utili agli sviluppatori di debug dell'applicazione (vedere **figura 1**); l'altro indica semplicemente che si è verificato un errore di run-time (vedere  **Figura 2**).

I dettagli dell'eccezione YSOD è particolarmente utile per gli sviluppatori di debug dell'applicazione, ma che mostra un YSOD agli utenti finali è tacky e poco professionale. Al contrario, attenzione agli utenti finali a una pagina di errore che gestisce l'aspetto del sito con più semplici da usare prose descrivendo la situazione. La buona notizia è che la creazione di tale pagina errori personalizzati è piuttosto semplice. Questa esercitazione inizia con uno sguardo ad ASP. Pagine di errore diversi di NET. Viene quindi illustrato come configurare l'applicazione web da visualizzare agli utenti una pagina di errore personalizzati in caso di un errore.

### <a name="examining-the-three-types-of-error-pages"></a>Esaminare i tre tipi di pagine di errore

Quando un'eccezione non gestita si verifica in un'applicazione ASP.NET uno dei tre tipi di pagine di errore viene visualizzato:

- La pagina di errore di eccezione dettagli giallo schermata di morte,
- La pagina di errore di Runtime errore giallo schermata di morte, o
- Pagina di errore personalizzata

Gli sviluppatori di pagina di errore più familiare con è la YSOD Dettagli eccezione. Per impostazione predefinita, questa pagina viene visualizzata agli utenti che vengono visitate in locale e pertanto è la pagina che viene visualizzato quando si verifica un errore durante il test del sito nell'ambiente di sviluppo. Come suggerisce il nome, il YSOD Dettagli eccezione fornisce informazioni dettagliate sull'eccezione - il tipo, il messaggio e l'analisi dello stack. Inoltre, se è stata generata l'eccezione dal codice nella classe code-behind della pagina ASP.NET e se l'applicazione è configurata per il debug quindi il YSOD Dettagli eccezione illustra anche la seguente riga di codice (e alcune righe di codice riportato sopra e sotto di essa).

**Figura 1** Mostra la pagina YSOD Dettagli eccezione. Prendere nota dell'URL nella finestra di indirizzi del browser: `http://localhost:62275/Genre.aspx?ID=foo`. Si tenga presente che il `Genre.aspx` pagina vengono elencati i recensioni dei libri in genere particolare. Si presuppone che `GenreId` valore (una `uniqueidentifier`) essere passato tramite la stringa di query; ad esempio, l'URL appropriato per visualizzare le recensioni fittizio è `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Se non`uniqueidentifier` valore viene passato in tramite la stringa di query (ad esempio, "foo") viene generata un'eccezione.

> [!NOTE]
> Per riprodurre l'errore nell'applicazione demo web disponibile per il download è possibile visitare `Genre.aspx?ID=foo` direttamente oppure fare clic sul collegamento "Genera un errore di Runtime" `Default.aspx`.


Tenere presente le informazioni sull'eccezione presentati nella **figura 1**. Il messaggio di eccezione, "conversione non riuscita durante la conversione da una stringa di caratteri a uniqueidentifier" è presente nella parte superiore della pagina. Il tipo dell'eccezione, `System.Data.SqlClient.SqlException`, è elencato. È inoltre disponibile l'analisi dello stack.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Figura 1**: I dettagli dell'eccezione YSOD include informazioni sull'eccezione  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-custom-error-page-cs/_static/image3.png))

L'altro tipo di YSOD è YSOD l'errore di Runtime e viene visualizzato nel **figura 2**. YSOD l'errore di Runtime informa il visitatore che si è verificato un errore di run-time, ma non include eventuali informazioni relative all'eccezione generata. (, Tuttavia, forniscono istruzioni su come poter visualizzare i dettagli dell'errore modificando il `Web.config` file, che fa parte di ciò che rende tale YSOD un aspetto poco professionale.)

Per impostazione predefinita, il YSOD di errore di Runtime verrà visualizzato agli utenti che visitano in modalità remota (tramite http://www.yoursite.com), come evidenziato dall'URL nella barra degli indirizzi del browser nel **Figura2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Le due schermate YSOD diverse esistono perché gli sviluppatori sono interessati a conoscere i dettagli dell'errore, ma tali informazioni non devono essere visualizzate in un sito live come può rivelare potenziali vulnerabilità della sicurezza o altre informazioni riservate a chiunque visiti il sito.

> [!NOTE]
> Se si seguono e Usa DiscountASP.NET dell'host web, è possibile notare che il YSOD di errore di Runtime non vengono visualizzate quando si visita il sito in tempo reale. Questo avviene perché DiscountASP.NET dispone di propri server configurato per mostrare il YSOD Dettagli eccezione per impostazione predefinita. La buona notizia è che è possibile eseguire l'override di questo comportamento predefinito tramite l'aggiunta di un `<customErrors>` sezione il `Web.config` file. La sezione "Configurazione quale pagina viene visualizzato errore" esamina le `<customErrors>` sezione in modo dettagliato.


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Figura 2**: errore di Runtime YSOD non Include tutti i dettagli dell'errore  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-custom-error-page-cs/_static/image6.png))

Il terzo tipo di pagina di errore è la pagina errori personalizzati, che è una pagina web che si crea. Il vantaggio di una pagina di errore personalizzata è di avere il controllo completo sulle informazioni che viene visualizzate all'utente con l'aspetto della pagina; la pagina di errore personalizzata può utilizzare la stessa pagina master e gli stili come le altre pagine. La sezione "Utilizzo di una tabella errore personalizzata" illustra la creazione di una pagina di errore personalizzato e configurarlo per visualizzare in caso di un'eccezione non gestita. **Figura 3** offre provare questa pagina di errore personalizzato. Come si può notare, l'aspetto della pagina di errore è molto più professionale rispetto a uno di giallo le schermate di morte illustrato nelle figure 1 e 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Figura 3**: una pagina di errore personalizzato offre un aspetto più personalizzata  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-custom-error-page-cs/_static/image9.png))

Si consiglia di controllare la barra degli indirizzi del browser nelle **figura 3**. Si noti che la barra degli indirizzi Mostra l'URL della pagina di errore personalizzata (`/ErrorPages/Oops.aspx`). Nelle figure 1 e 2 vengono visualizzata le schermate di morte giallo nella stessa pagina da cui ha origine l'errore (`Genre.aspx`). Pagina di errore personalizzata viene passata l'URL della pagina in cui si è verificato l'errore tramite il `aspxerrorpath` parametro querystring.

## <a name="configuring-which-error-page-is-displayed"></a>Configurazione di pagina di errore di cui viene visualizzata

Quale delle tre pagine di possibile errore viene visualizzato si basa su due variabili:

- Le informazioni di configurazione nel `<customErrors>` sezione, e
- Indica se l'utente sta visitando il sito locale o remota.

Il [ `<customErrors>` sezione](https://msdn.microsoft.com/library/h0hfz6fc.aspx) nelle `Web.config` include due attributi che influiscono sulla quale pagina di errore viene visualizzato: `defaultRedirect` e `mode`. L'attributo `defaultRedirect` è facoltativo. Se fornito, specifica l'URL della pagina di errore personalizzato e indica che deve essere visualizzata la pagina di errore personalizzato anziché il YSOD di errore di Runtime. Il `mode` attributo è obbligatorio e può accettare solo uno dei tre valori: `On`, `Off`, o `RemoteOnly`. Questi valori hanno il comportamento seguente:

- `On` -indica che viene visualizzata la pagina di errore personalizzate o YSOD l'errore di Runtime per tutti i visitatori, indipendentemente dal fatto che siano locali o remoti.
- `Off` -Specifica che il YSOD Dettagli eccezione viene visualizzata per tutti i visitatori, indipendentemente dal fatto che siano locali o remoti.
- `RemoteOnly` -indica che la pagina di errore personalizzate o YSOD l'errore di Runtime viene visualizzata per i visitatori remoti, mentre il YSOD Dettagli eccezione viene visualizzata per i visitatori locali.

Se non diversamente specificato, ASP.NET si comporta come se è stato impostato l'attributo mode su `RemoteOnly` e non fosse stato specificato un `defaultRedirect` valore. In altre parole, il comportamento predefinito è viene visualizzato il YSOD Dettagli eccezione ai visitatori locali, mentre YSOD l'errore di Runtime viene visualizzata per i visitatori del remoti. È possibile eseguire l'override di questo comportamento predefinito aggiungendo un `<customErrors>` sezione dell'applicazione web `Web.config file.`

## <a name="using-a-custom-error-page"></a>Utilizzo di una pagina di errore personalizzato

Ogni applicazione web deve avere una pagina di errore personalizzato. Fornisce un'alternativa più dall'aspetto professionale al YSOD errore di Runtime, è possibile creare con facilità e configurazione dell'applicazione per utilizzare la pagina di errore personalizzato richiede solo alcuni istanti. Il primo passaggio è creare la pagina di errore personalizzato. Ho aggiunto una nuova cartella per l'applicazione le recensioni dei libri denominata `ErrorPages` e quindi aggiungervi che una nuova pagina ASP.NET denominato `Oops.aspx`. Disponibile la pagina Usa la stessa pagina master come il resto delle pagine del sito in modo che erediti automaticamente lo stesso aspetto e comportamento.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Figura 4**: creare una pagina di errore personalizzato

Successivamente, dedicare alcuni minuti il contenuto per la pagina di errore di creazione. Ho creato una pagina di errore personalizzato piuttosto semplice con un messaggio che indica che si è verificato un errore imprevisto e un collegamento alla home page del sito.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Figura 5**: progettare la pagina di errore personalizzato  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-custom-error-page-cs/_static/image14.png))

Con la pagina di errore completata, configurare l'applicazione web per utilizzare la pagina di errore personalizzato anziché il YSOD di errore di Runtime. Questa operazione viene eseguita specificando l'URL della pagina di errore nel `<customErrors>` della sezione `defaultRedirect` attributo. Aggiungere il markup seguente all'applicazione `Web.config` file:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Il markup precedente configura l'applicazione per mostrare il YSOD Dettagli eccezione per gli utenti che visitano in locale, quando si usa la pagina di errore personalizzata Oops.aspx per gli utenti che visitano in modalità remota. Per visualizzare questa azione, distribuire il sito Web nell'ambiente di produzione e quindi accedere alla pagina Genre.aspx sul sito in tempo reale con un valore di stringa di query non è valido. Verrà visualizzata la pagina degli errori personalizzati (fare riferimento a **figura 3**).

Per verificare che la pagina di errore personalizzato viene visualizzata solo agli utenti remoti, visitare il `Genre.aspx` pagina con una stringa di query non valida dall'ambiente di sviluppo. Noterete ancora la YSOD Dettagli eccezione (fare riferimento a **figura 1**). Il `RemoteOnly` impostazione assicura che gli utenti che visitano il sito nell'ambiente di produzione vedono la pagina di errore personalizzato mentre gli sviluppatori che lavorano in locale continuano a visualizzare i dettagli dell'eccezione.

## <a name="notifying-developers-and-logging-error-details"></a>Avvisare gli sviluppatori e i dettagli dell'errore di registrazione

Gli errori che si verificano nell'ambiente di sviluppo sono stati causati dallo sviluppatore seduti a suo computer. Anna è racchiusa informazioni dell'eccezione di YSOD Dettagli eccezione e mia madre sa quali passaggi Anna stava eseguendo quando si è verificato l'errore. Tuttavia, quando si verifica un errore in produzione, lo sviluppatore non ha una conoscenza che si è verificato un errore, a meno che l'utente finale che visitano il sito accetta il tempo necessario per segnalare l'errore. E anche se l'utente abbandona il suo modo per avvisare il team di sviluppo che si è verificato un errore, senza conoscere il tipo di eccezione, messaggio e analisi dello stack può essere difficile da diagnosticare la causa dell'errore, tantomeno risolverlo.

Per questi motivi che è fondamentale che eventuali errori nell'ambiente di produzione viene registrata a un archivio permanente (ad esempio un database) e che gli sviluppatori vengono visualizzati avvisi di questo errore. Pagina di errore personalizzata può sembrare un buon punto di eseguire queste operazioni la registrazione e la notifica. Sfortunatamente, la pagina di errore personalizzata non dispone dell'accesso per i dettagli dell'errore e pertanto non può essere utilizzata per registrare queste informazioni. La buona notizia è che esistono diversi modi per intercettare i dettagli dell'errore e per registrarli e successive tre esercitazioni esplorare in dettaglio in questo argomento.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Uso delle pagine di errore personalizzato diverso per gli stati di errore HTTP diverso

Quando un'eccezione generata da una pagina ASP.NET e non gestita, l'eccezione percolates fino al runtime di ASP.NET, che consente di visualizzare la pagina di errore configurata. Se una richiesta viene fornito nel motore ASP.NET ma non può essere elaborata per qualche motivo, probabilmente il file richiesto non è trovato o leggere le autorizzazioni sono state disabilitate per il file, il motore di ASP.NET genera un `HttpException`. Questa eccezione, ad esempio le eccezioni generate da pagine ASP.NET, che viene propagato al runtime, causando la pagina di errore appropriato da visualizzare.

Cosa significa questo per l'applicazione web nell'ambiente di produzione è che se un utente richiede una pagina che non viene trovata allora che verrà visualizzata la pagina di errore personalizzato. **Figura 6** Mostra un esempio di questo tipo. Perché la richiesta riguarda una pagina inesistente (`NoSuchPage.aspx`), un' `HttpException` viene generata un'eccezione e viene visualizzata la pagina di errore personalizzata (si noti il riferimento al `NoSuchPage.aspx` nel `aspxerrorpath` parametro querystring).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Figura 6**: il Runtime ASP.NET visualizza la configurato errore pagina In risposta a una richiesta non valida ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-custom-error-page-cs/_static/image17.png))

Per impostazione predefinita, tutti i tipi di errori di causano la stessa pagina di errore personalizzato da visualizzare. Tuttavia, è possibile specificare una pagina di errore personalizzato diverso per una specifica HTTP stato codice usando `<error>` gli elementi figlio all'interno di `<customErrors>` sezione. Ad esempio, per ottenere una pagina di errore diversi visualizzata in caso di una pagina non trovata errore che ha un codice di stato HTTP di 404, aggiornare il `<customErrors>` sezione da includere il markup seguente:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Con questa modifica, ogni volta che un utente potrebbe visitare in modalità remota richiede una risorsa di ASP.NET che non esiste, vengono reindirizzati per il `404.aspx` pagina errori personalizzati anziché `Oops.aspx`. Come **figura 7** illustrato, la `404.aspx` pagina può includere un messaggio più specifico rispetto alla pagina di errore personalizzato generale.

> [!NOTE]
> Consulta [pagine di errore 404, una volta più](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) per indicazioni sulla creazione di pagine di errore 404 efficace.


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**Figura 7**: la pagina di errore 404 personalizzata viene visualizzato un messaggio più mirato rispetto a `Oops.aspx`  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-custom-error-page-cs/_static/image20.png)) 

Poiché è certo che il `404.aspx` pagina viene raggiunta solo quando l'utente effettua una richiesta per una pagina che non è stata trovata, è possibile migliorare questa pagina di errore personalizzato per includere funzionalità per consentire all'utente di risolvere questo tipo specifico di errore. Ad esempio, è possibile creare una tabella di database che esegue il mapping nota URL non valido a un URL valido e quindi chiedere il `404.aspx` personalizzato pagina di errore eseguire una query sulla tabella e suggerire l'utente stia tentando di raggiungere le pagine.

> [!NOTE]
> Pagina di errore personalizzata viene visualizzata solo quando viene effettuata una richiesta a una risorsa gestita dal modulo di ASP.NET. Come illustrato nella [differenze principali tra IIS e ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-cs.md) dell'esercitazione, il server web potrebbe gestire determinate richieste se stesso. Per impostazione predefinita, IIS web server elabora le richieste per il contenuto statico come immagini e file HTML senza richiamare il motore di ASP.NET. Di conseguenza, se l'utente richiede un file di immagine non esistenti otterranno nuovo messaggio di errore 404 predefinita di IIS anziché ASP. Pagina di errore configurata di NET.


## <a name="summary"></a>Riepilogo

Quando si verifica un'eccezione non gestita in un'applicazione ASP.NET, all'utente viene visualizzato uno dei tre pagine di errore: eccezione dettagli giallo schermata di morte; Errore di Runtime giallo schermata; o una pagina di errore personalizzato. Viene visualizzata la pagina di errore varia a seconda dell'applicazione `<customErrors>` configuration e indica se l'utente sta visitando localmente o in remoto. Il comportamento predefinito è per mostrare il YSOD Dettagli eccezione ai visitatori locali e YSOD l'errore di Runtime per i visitatori del remoti.

Mentre il YSOD di errore di Runtime nasconde informazioni sugli errori potrebbero contenere informazioni riservate da parte dell'utente che visitano il sito, si interrompe dall'aspetto del sito e rende l'applicazione cerca difettoso. Un approccio migliore consiste nell'utilizzare una pagina errori personalizzati, che comporta la creazione e progettazione della pagina di errore personalizzato e specificando il relativo URL nel `<customErrors>` della sezione `defaultRedirect` attributo. È anche possibile avere più pagine di errore personalizzati per diversi stati di errore HTTP.

Pagina di errore personalizzata è il primo passaggio in una strategia per un sito Web nell'ambiente di produzione di gestione degli errori completo. Lo sviluppatore dell'errore di avviso e i dettagli di registrazione sono anche importanti passaggi. Successive tre esercitazioni di esplorare tecniche di notifica di errore e la registrazione.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Pagine di errore, ancora una volta](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Linee guida di progettazione delle eccezioni](https://msdn.microsoft.com/library/ms229014.aspx)
- [Pagine di errore descrittivo](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Gestione e generazione di eccezioni](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [In modo corretto utilizzando pagine errori personalizzati in ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Precedente](strategies-for-database-development-and-deployment-cs.md)
> [Successivo](processing-unhandled-exceptions-cs.md)
