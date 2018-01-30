---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Visualizzazione di una pagina di errore personalizzati (c#) | Documenti Microsoft
author: rick-anderson
description: "Ciò che viene visualizzato quando si verifica un errore di runtime in un'applicazione web ASP.NET? La risposta dipende dalla modalità del sito Web &lt;customErrors&gt; configurazione..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d68dedfc1f606cc6f0381bcbdb3f65c1ea3b2e5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
<a name="displaying-a-custom-error-page-c"></a>Visualizzazione di una pagina di errore personalizzati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Ciò che viene visualizzato quando si verifica un errore di runtime in un'applicazione web ASP.NET? La risposta dipende dalla modalità del sito Web &lt;customErrors&gt; configurazione. Per impostazione predefinita, viene visualizzata una schermata di colore gialla antiestetici proclaiming che si è verificato un errore di runtime. In questa esercitazione viene illustrato come personalizzare queste impostazioni alla pagina di errore personalizzata di visualizzazione di un oggetto piacevole dell'aspetto del sito corrispondente.


## <a name="introduction"></a>Introduzione

In una situazione ideale non sarebbe possibile errori in fase di esecuzione. I programmatori di scrivere il codice propria un bug e con la convalida dell'input utente affidabili ed esterni risorse come server di database e server di posta elettronica non venga portato offline. Naturalmente, in realtà sono inevitabili gli errori. Le classi in .NET Framework segnalano un errore generando un'eccezione. Ad esempio, la chiamata a un oggetto SqlConnection metodo Open dell'oggetto consente di stabilire una connessione al database specificato da una stringa di connessione. Tuttavia, se il database è attivo o se le credenziali nella stringa di connessione sono valide quindi il metodo Open genera un `SqlException`. Le eccezioni possono essere gestite dall'uso di `try/catch/finally` blocchi. Se il codice all'interno un `try` blocco genera un'eccezione, il controllo viene trasferito al blocco catch appropriato in cui lo sviluppatore può tentare la correzione dell'errore. Se non si verifica alcun blocco catch corrispondente, o se il codice che ha generato l'eccezione non è presente in un blocco try, l'eccezione percolates lo stack di chiamate di search `try/catch/finally` blocchi.

Se l'eccezione viene propagata tutti fino al runtime ASP.NET senza essere stato gestito il [ `HttpApplication` classe](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)del [ `Error` evento](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) viene generato e l'applicazione configurata *pagina di errore*  viene visualizzato. Per impostazione predefinita, ASP.NET consente di visualizzare una pagina di errore che viene viene definita il [giallo schermata di morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Esistono due versioni di YSOD: uno Mostra i dettagli dell'eccezione, una traccia dello stack e altre informazioni utili agli sviluppatori di debug dell'applicazione (vedere **figura 1**); l'altro indica semplicemente che si è verificato un errore in fase di esecuzione (vedere  **Figura 2**).

I dettagli dell'eccezione YSOD è molto utile per gli sviluppatori di debug dell'applicazione, ma la visualizzazione di un YSOD agli utenti finali è tacky e poco professionale. In alternativa, è opportuno tenere gli utenti finali a una pagina di errore che gestisce l'aspetto del sito con prose intuitiva che descrive la situazione. Buone notizie sono che la creazione di una pagina di errore personalizzato è molto semplice. In questa esercitazione inizia con un'occhiata ASP. Pagine di errore diversi della rete. Viene quindi illustrato come configurare l'applicazione web per visualizzare gli utenti di una pagina di errore personalizzati in caso di errore.

### <a name="examining-the-three-types-of-error-pages"></a>Esaminare i tre tipi di pagine di errore

Viene visualizzato quando un'eccezione non gestita si verifica in un'applicazione ASP.NET a uno dei tre tipi di pagine di errore:

- La pagina di errore di eccezione dettagli giallo schermata di morte
- La pagina di errore di Runtime errore giallo schermata di morte o
- Una pagina di errore personalizzati

Gli sviluppatori di pagine di errore sono più familiare è la YSOD Dettagli eccezione. Per impostazione predefinita, questa pagina viene visualizzata agli utenti che sono visita localmente e pertanto è la pagina che viene visualizzato quando si verifica un errore durante il test del sito nell'ambiente di sviluppo. Come suggerisce il nome, il YSOD Dettagli eccezione fornisce informazioni dettagliate sull'eccezione - il tipo, il messaggio e la traccia dello stack. Inoltre, se l'eccezione è stata generata dal codice nella classe code-behind della pagina ASP.NET e se l'applicazione è configurata per il debug quindi il YSOD Dettagli eccezione verrà visualizzata anche questa riga di codice (e alcune righe di codice riportato sopra e sotto).

**Figura 1** viene mostrata la pagina YSOD Dettagli eccezione. Prendere nota dell'URL nella finestra di indirizzi del browser: `http://localhost:62275/Genre.aspx?ID=foo`. Tenere presente che il `Genre.aspx` pagina sono elencate le recensioni in un particolare genere. È necessario che `GenreId` valore (un `uniqueidentifier`) essere passato tramite il parametro querystring; ad esempio, l'URL appropriato per visualizzare le recensioni fittizio è `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Se non`uniqueidentifier` valore viene passato in tramite la stringa di query (ad esempio "foo") viene generata un'eccezione.

> [!NOTE]
> Per riprodurre l'errore nell'applicazione web demo disponibile per il download è possibile visitare `Genre.aspx?ID=foo` direttamente o fare clic sul collegamento "Genera un errore di Runtime" `Default.aspx`.


Tenere presente le informazioni sull'eccezione presentati in **figura 1**. Il messaggio di eccezione, "conversione non riuscita durante la conversione da una stringa di caratteri a uniqueidentifier" è presente nella parte superiore della pagina. Il tipo di eccezione, `System.Data.SqlClient.SqlException`, è elencato. È inoltre disponibile la traccia dello stack.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Figura 1**: YSOD i dettagli dell'eccezione include informazioni sull'eccezione  
 ([Fare clic per visualizzare l'immagine ingrandita](displaying-a-custom-error-page-cs/_static/image3.png))

L'altro tipo di YSOD è YSOD di errore di Runtime e viene visualizzato **figura 2**. YSOD di errore di Runtime informa il visitatore che si è verificato un errore di run-time, ma non include le informazioni sull'eccezione generata. (, Tuttavia, vengono fornite istruzioni su come rendere visibili i dettagli dell'errore modificando il `Web.config` file, che fa parte di ciò che rende tale YSOD un aspetto poco professionale.)

Per impostazione predefinita, il YSOD di errore di Runtime verrà visualizzato agli utenti di visitare in modalità remota (tramite http://www.yoursite.com), come evidenziato dall'URL nella barra degli indirizzi del browser in **figura 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Le due schermate YSOD diverse esistono perché gli sviluppatori sono interessati a conoscere i dettagli dell'errore, ma tali informazioni non devono essere visualizzate in un sito in tempo reale come può rivelare potenziali vulnerabilità di sicurezza o altre informazioni riservate a chiunque visiti la sito.

> [!NOTE]
> Se si segue e si utilizza DiscountASP.NET come host web, si noterà che il YSOD di errore di Runtime non viene visualizzata quando si visita il sito in tempo reale. In questo modo DiscountASP.NET dispone di propri server configurato per mostrare il YSOD Dettagli eccezione per impostazione predefinita. Buone notizie sono che è possibile eseguire l'override di questo comportamento predefinito aggiungendo un `<customErrors>` sezione per la `Web.config` file. La sezione "Configurazione di cui errore pagina viene visualizzato" esamina il `<customErrors>` sezione in modo dettagliato.


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Figura 2**: errore di Runtime YSOD non Include tutti i dettagli dell'errore  
 ([Fare clic per visualizzare l'immagine ingrandita](displaying-a-custom-error-page-cs/_static/image6.png))

Il terzo tipo di pagina di errore è la pagina di errore personalizzata, è una pagina web creata. Il vantaggio di una pagina di errore personalizzato consiste nel disporre di un controllo completo sulle informazioni che viene visualizzate all'utente con l'aspetto della pagina; pagina di errore personalizzata è possibile utilizzare la stessa pagina master e gli stili come le altre pagine. La sezione "Utilizzando una pagina di errore personalizzati" vengono illustrati la creazione di una pagina di errore personalizzato e configurarlo per visualizzare in caso di un'eccezione non gestita. **Figura 3** offre provare questa pagina di errore personalizzato. Come si può notare, l'aspetto della pagina di errore è molto più professionale rispetto a uno di colore giallo schermate di morte illustrato nelle figure 1 e 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Figura 3**: una pagina di errore personalizzato offre un aspetto più personalizzata  
 ([Fare clic per visualizzare l'immagine ingrandita](displaying-a-custom-error-page-cs/_static/image9.png))

È opportuno controllare la barra degli indirizzi del browser in **figura 3**. Si noti che la barra degli indirizzi Mostra l'URL della pagina di errore personalizzato (`/ErrorPages/Oops.aspx`). Nelle figure 1 e 2 schermate giallo morte vengono visualizzata nella stessa pagina che ha avuto origine l'errore (`Genre.aspx`). Pagina di errore personalizzata viene passata l'URL della pagina in cui si è verificato l'errore tramite il `aspxerrorpath` parametro querystring.

## <a name="configuring-which-error-page-is-displayed"></a>Configurazione di pagina di errore di cui viene visualizzata.

Delle tre pagine di errore visualizzato è basato su due variabili:

- Le informazioni di configurazione di `<customErrors>` sezione, e
- Se l'utente visita il sito locale o remota.

Il [ `<customErrors>` sezione](https://msdn.microsoft.com/library/h0hfz6fc.aspx) in `Web.config` dispone di due attributi che influiscono sulla viene visualizzata la pagina di errore: `defaultRedirect` e `mode`. L'attributo `defaultRedirect` è facoltativo. Se fornito, specifica l'URL della pagina di errore personalizzato e indica che deve essere visualizzata la pagina di errore personalizzato anziché il YSOD di errore di Runtime. Il `mode` attributo è obbligatorio e può accettare solo uno dei tre valori: `On`, `Off`, o `RemoteOnly`. Questi valori non hanno il seguente comportamento:

- `On`-indica che viene visualizzata la pagina di errore personalizzate o YSOD di errore di Runtime a tutti i visitatori, indipendentemente dal fatto che siano locale o remoto.
- `Off`-Specifica che il YSOD Dettagli eccezione viene visualizzata per tutti i visitatori, indipendentemente dal fatto che siano locale o remoto.
- `RemoteOnly`-indica che la pagina di errore personalizzate o YSOD di errore di Runtime viene visualizzata visitatori remoto, mentre viene visualizzato il YSOD Dettagli eccezione visitatori locali.

Se non diversamente specificato, ASP.NET si comporta come se è stato impostato l'attributo mode su `RemoteOnly` e non è stato specificato un `defaultRedirect` valore. In altre parole, il comportamento predefinito è che viene visualizzato il YSOD Dettagli eccezione visitatori locali mentre visitatori remoti viene visualizzato il YSOD di errore di Runtime. È possibile eseguire l'override di questo comportamento predefinito aggiungendo un `<customErrors>` sezione all'applicazione web`Web.config file.`

## <a name="using-a-custom-error-page"></a>Utilizzo di una pagina di errore personalizzati

Ogni applicazione web deve avere una pagina di errore personalizzato. Fornisce un'alternativa più professionale YSOD di errore di Runtime, è possibile creare facilmente e configurazione dell'applicazione per utilizzare la pagina di errore personalizzato accetta solo alcuni minuti. Il primo passaggio è creare la pagina di errore personalizzato. Ho aggiunto una nuova cartella per l'applicazione recensioni denominata `ErrorPages` e aggiunto a che una nuova pagina ASP.NET denominato `Oops.aspx`. Impostare la pagina stessa pagina master come il resto delle pagine del sito in modo che erediti automaticamente lo stesso aspetto e comportamento.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Figura 4**: creare una pagina di errore personalizzati

Successivamente, dedicare alcuni minuti il contenuto per la pagina di errore di creazione. Ho creato una pagina di errore personalizzato piuttosto semplice con un messaggio che indica che si è verificato un errore imprevisto e un collegamento alla home page del sito.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Figura 5**: progettare la pagina di errore personalizzati  
 ([Fare clic per visualizzare l'immagine ingrandita](displaying-a-custom-error-page-cs/_static/image14.png))

Con la pagina di errore completata, configurare l'applicazione web per utilizzare la pagina di errore personalizzato anziché il YSOD di errore di Runtime. Questa operazione viene eseguita specificando l'URL della pagina di errore nel `<customErrors>` della sezione `defaultRedirect` attributo. Aggiungere il markup seguente all'applicazione `Web.config` file:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Il codice precedente consente di configurare l'applicazione per mostrare il YSOD Dettagli eccezione per gli utenti che visitano localmente, utilizzando la pagina di errore personalizzato Oops.aspx per gli utenti che visitano in modalità remota. Per vedere un esempio pratico, distribuire il sito Web nell'ambiente di produzione e quindi visitare la pagina Genre.aspx nel sito in tempo reale con un valore di stringa di query non valido. Si verrà visualizzata la pagina di errore personalizzato (fare riferimento al **figura 3**).

Per verificare che la pagina di errore personalizzato viene visualizzata solo per gli utenti remoti, visitare il `Genre.aspx` pagina con un valore querystring non valido dall'ambiente di sviluppo. Si noterà comunque il YSOD Dettagli eccezione (fare riferimento al **figura 1**). Il `RemoteOnly` impostazione assicura che gli utenti che visitano il sito nell'ambiente di produzione vedono la pagina di errore personalizzato, mentre gli sviluppatori che lavorano in locale visualizzare i dettagli dell'eccezione.

## <a name="notifying-developers-and-logging-error-details"></a>Notificare agli sviluppatori e i dettagli dell'errore di registrazione

Gli errori che si verificano nell'ambiente di sviluppo sono stati causati dallo sviluppatore del proprio computer locale. Mary è visualizzata informazioni dell'eccezione nel YSOD Dettagli eccezione e i passaggi che stava eseguendo quando si è verificato l'errore e lo sa. Tuttavia, quando si verifica un errore in produzione, lo sviluppatore non ha alcuna conoscenza che si è verificato un errore a meno che l'utente finale, visitare il sito richiede il tempo per segnalare l'errore. E anche se l'utente abbandona il suo modo per informare il team di sviluppo che si è verificato un errore, senza conoscere il tipo di eccezione, messaggio e traccia dello stack può essere difficile da diagnosticare la causa dell'errore, lasciandoli soli a risolvere il problema.

Per questi motivi che è fondamentale che sia stato registrato un errore nell'ambiente di produzione per un archivio permanente (ad esempio un database) e che gli sviluppatori vengono visualizzati avvisi di questo errore. Pagina di errore personalizzata può sembrare un ottimo strumento per eseguire la registrazione e notifica. Sfortunatamente, la pagina di errore personalizzato non dispone dell'accesso per i dettagli dell'errore e non può essere utilizzata per accedere a queste informazioni. Buone notizie sono che esistono diversi modi per intercettare i dettagli dell'errore e per accedere a essi e le esercitazioni successive tre esplorare in dettaglio in questo argomento.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Utilizzando le pagine di errore personalizzato diverso per gli stati di errore HTTP diverso

Quando un'eccezione viene generata da una pagina ASP.NET e non viene gestita l'eccezione percolates fino al runtime di ASP.NET, che consente di visualizzare la pagina di errore configurata. Se una richiesta viene nel motore di ASP.NET, ma non può essere elaborata per qualche motivo, ad esempio il file richiesto non è disponibile o leggere le autorizzazioni sono state disabilitate per il file, il motore di ASP.NET genera un `HttpException`. Questa eccezione, ad esempio le eccezioni generate dalle pagine ASP.NET, viene propagata al runtime, causando la pagina di errore appropriato da visualizzare.

Ciò significa che per l'applicazione web nell'ambiente di produzione è che se un utente richiede una pagina che non viene trovata, quindi che verrà visualizzata la pagina di errore personalizzato. **Figura 6** Mostra un esempio. Perché la richiesta è per una pagina inesistente (`NoSuchPage.aspx`), un `HttpException` viene generata un'eccezione e viene visualizzata la pagina di errore personalizzato (si noti il riferimento a `NoSuchPage.aspx` nel `aspxerrorpath` parametro querystring).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Figura 6**: il Runtime ASP.NET Visualizza configurato errore pagina In risposta a una richiesta non valida ([fare clic per visualizzare l'immagine ingrandita](displaying-a-custom-error-page-cs/_static/image17.png))

Per impostazione predefinita, tutti i tipi di errori di determinare la stessa pagina di errore personalizzato da visualizzare. Tuttavia, è possibile specificare una pagina di errore personalizzato diverso per un utilizzo specifico di HTTP stato codice `<error>` elementi figlio all'interno di `<customErrors>` sezione. Ad esempio, una pagina di errore diversi visualizzata in caso di una pagina di errore, che dispone di un codice di stato HTTP di 404 non trovata per aggiornare il `<customErrors>` sezione per includere il markup seguente:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Con questa modifica sul posto, ogni volta che un utente visita in modalità remota richiede una risorsa di ASP.NET che non esiste, verrà reindirizzato al `404.aspx` pagina di errore personalizzata anziché `Oops.aspx`. Come **figura 7** illustrato, il `404.aspx` pagina può includere un messaggio più specifico rispetto alla pagina di errore personalizzato generale.

> [!NOTE]
> Estrarre [pagine di errore 404, una volta più](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) per istruzioni sulla creazione di pagine di errore 404 effettivo.


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**Figura 7**: la pagina di errore 404 personalizzate Visualizza un messaggio più mirato rispetto a`Oops.aspx`  
 ([Fare clic per visualizzare l'immagine ingrandita](displaying-a-custom-error-page-cs/_static/image20.png)) 

Poiché si è certi che il `404.aspx` pagina viene raggiunto solo quando l'utente effettua una richiesta per una pagina che non è stata trovata, è possibile migliorare la pagina di errore personalizzata per includere funzionalità per consentire all'utente di risolvere questo tipo di errore specifico. Ad esempio, è possibile compilare una tabella di database che esegue il mapping nota URL non valido a un URL valido e quindi chiedere di `404.aspx` pagina di errore personalizzata eseguire una query di tabella e suggerire l'utente tenta di raggiungere le pagine.

> [!NOTE]
> Pagina di errore personalizzata viene visualizzata solo quando viene effettuata una richiesta a una risorsa gestita dal motore di ASP.NET. Come accennato nel [principali differenze tra IIS e il Server di sviluppo ASP.NET](core-differences-between-iis-and-the-asp-net-development-server-cs.md) dell'esercitazione, il server web può gestire alcune richieste stessi. Per impostazione predefinita, IIS web server elabora le richieste di contenuto statico, ad esempio immagini e file HTML senza richiamare il motore ASP.NET. Di conseguenza, se l'utente richiede un file di immagine non esistente che verrà visualizzato il messaggio di errore 404 predefinito di IIS anziché ASP. Pagina di errore configurata della rete.


## <a name="summary"></a>Riepilogo

Quando un'applicazione ASP.NET si verifica un'eccezione non gestita, l'utente viene visualizzato uno dei tre pagine di errore: eccezione dettagli giallo schermata di morte; Errore di Runtime giallo schermata; o una pagina di errore personalizzato. Viene visualizzata la pagina di errore varia a seconda dell'applicazione `<customErrors>` configurazione e indica se l'utente visita localmente o in remoto. Il comportamento predefinito è mostrare il YSOD Dettagli eccezione ai visitatori locali e il YSOD di errore di Runtime per i visitatori remoti.

Mentre il YSOD di errore di Runtime consente di nascondere informazioni potenzialmente riservate da parte dell'utente, visitare il sito, interruzioni dall'aspetto del sito e lo rende l'applicazione cerca difettoso. Un approccio migliore consiste nell'utilizzare una pagina di errore personalizzati, che comporta la creazione e progettazione della pagina di errore personalizzato e specificare l'URL nel `<customErrors>` della sezione `defaultRedirect` attributo. È anche possibile avere più pagine di errore personalizzati per i diversi stati di errore HTTP.

Pagina di errore personalizzata è il primo passaggio di un errore completo strategia per un sito Web nell'ambiente di produzione di gestione. Lo sviluppatore dell'errore di avvisi e i dettagli di registrazione sono anche importanti passaggi. Le esercitazioni successive tre esplorare tecniche per la notifica degli errori e registrazione.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Pagine di errore, ancora una volta](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Linee guida di progettazione delle eccezioni](https://msdn.microsoft.com/library/ms229014.aspx)
- [Pagine di errore descrittivo](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Gestione e generazione di eccezioni](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Correttamente utilizzando le pagine di errore personalizzato in ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

>[!div class="step-by-step"]
[Precedente](strategies-for-database-development-and-deployment-cs.md)
[Successivo](processing-unhandled-exceptions-cs.md)
