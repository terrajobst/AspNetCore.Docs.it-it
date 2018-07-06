---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: Principali differenze tra IIS e il Server di sviluppo ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Quando si testa in locale un'applicazione ASP.NET, è probabile che si usa Server Web di sviluppo ASP.NET. Tuttavia, il sito Web di produzione è più probabile pow...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 586bc45a44e773c3097de0959411a2d27098459a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821090"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>Differenze principali tra IIS e il Server di sviluppo ASP.NET (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> Quando si testa in locale un'applicazione ASP.NET, è probabile che si usa Server Web di sviluppo ASP.NET. Tuttavia, il sito Web di produzione è più probabile IIS spenta. Esistono alcune differenze tra la modalità di gestire le richieste di questi server web, e queste differenze possono avere conseguenze importanti. Questa esercitazione illustra alcune delle differenze più pertinente.


## <a name="introduction"></a>Introduzione

Ogni volta che un utente visita un'applicazione ASP.NET browser dell'utente invia una richiesta al sito Web. Tale richiesta viene prelevata dal software di server web, che coordina con il runtime ASP.NET per generare e restituire il contenuto per la risorsa richiesta. Il[**ho** qdn **posso** nformation **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) sono una suite di servizi che forniscono funzionalità comuni basati su Internet per Server di Windows. IIS è il server web più diffuso per le applicazioni ASP.NET in ambienti di produzione; è molto probabile che il software del server web sta utilizzando il provider di hosting web a servire l'applicazione ASP.NET. IIS può inoltre costituire il software del server web nell'ambiente di sviluppo, anche se ciò comporta l'installazione di IIS e configurarlo in modo corretto.


Il Server di sviluppo ASP.NET è un'opzione di server web alternativi per l'ambiente di sviluppo. incluso in ed è integrato in Visual Studio. A meno che l'applicazione web è stato configurato per usare IIS, ASP.NET Development Server viene automaticamente avviato e utilizzato come server web la prima volta che si visita una pagina web da Visual Studio. Le applicazioni web demo creati nuovamente nel [ *determinare quali file devono essere distribuiti* ](determining-what-files-need-to-be-deployed-vb.md) esercitazione sono state entrambe le applicazioni web basate sul sistema di file che non sono state configurate per utilizzare IIS. Pertanto, quando si visita uno di questi siti Web da Visual Studio viene usato il Server di sviluppo ASP.NET.

In un mondo perfetto sarebbe identici di ambienti di sviluppo e produzione. Tuttavia, come illustrato nell'esercitazione precedente non è insolito per gli ambienti di avere impostazioni di configurazione diverse. L'uso di software del server web diversi in ambienti aggiunge un'altra variabile che deve essere presi in considerazione quando si distribuisce un'applicazione. Questa esercitazione illustra le differenze principali tra IIS e ASP.NET Development Server. A causa di queste differenze esistono scenari in cui il codice eseguito funzioneranno nell'ambiente di sviluppo genera un'eccezione o ha un comportamento differente quando eseguito nell'ambiente di produzione.

## <a name="security-context-differences"></a>Differenze di contesto di sicurezza

Ogni volta che il software del server web gestisce una richiesta in ingresso che la richiesta associa a un contesto di sicurezza specifico. Queste informazioni di contesto di sicurezza vengano usate dal sistema operativo per determinare quali azioni sono consentite dalla richiesta. Ad esempio, una pagina ASP.NET può includere codice che registra un messaggio in un file su disco. Affinché questa pagina ASP.NET viene eseguita senza errori, il contesto di sicurezza deve avere le appropriate autorizzazioni a livello di sistema per i file, vale a dire le autorizzazioni su tale file di scrittura.

Il Server di sviluppo ASP.NET associa le richieste in ingresso con il contesto di sicurezza dell'utente attualmente connesso. Se è connessi al desktop con privilegi di amministratore, le pagine ASP.NET gestite tramite ASP.NET Development Server avrà gli stessi diritti di accesso come amministratore. Tuttavia, ASP.NET le richieste gestite da IIS sono associate a un account del computer specifico. Per impostazione predefinita, l'account del computer del servizio di rete viene usato da IIS versione 6 e 7, anche se il provider di hosting web potrebbe stato configurato un account univoco per ogni cliente. Inoltre, il provider di hosting web probabilmente ha assegnato autorizzazioni limitate per l'account computer. Il risultato finale è che è possibile le pagine web che vengono eseguite senza errori nell'ambiente di sviluppo, ma generano eccezioni correlate alle autorizzazioni quando viene ospitato nell'ambiente di produzione.

Per visualizzare questo tipo di errore nell'azione ho creato una pagina nel sito Web delle recensioni dei libri che crea un file su disco che archivia la data e l'ora più recente di un utente visualizzato le *insegnare manualmente ASP.NET 3.5 in 24 ore* esaminare. Per seguire la procedura, aprire il `~/Tech/TYASP35.aspx` pagina e aggiungere il codice seguente per il `Page_Load` gestore dell'evento:

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> Il [ `File.WriteAllText` metodo](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) crea un nuovo file se non esiste e quindi scrive il contenuto specificato. Se il file esiste già, il contenuto esistente viene sovrascritto.


Successivamente, visitare il *insegnare manualmente ASP.NET 3.5 in 24 ore* pagina revisione del libro nell'ambiente di sviluppo utilizzando ASP.NET Development Server. Supponendo che si è connessi al computer con un account che disponga di autorizzazioni adeguate per creare e modificare un file di testo all'interno del web directory radice dell'applicazione recensione di un libro sembra identico al precedente, ma ogni volta che la pagina visitata la data e ora e dell'utente  Indirizzo IP viene archiviato nel `LastTYASP35Access.txt` file. Puntare il browser in questo file. si dovrebbe vedere un messaggio simile a quello illustrato nella figura 1.


[![Il File di testo contiene la data e ora ultima recensione di un libro è stato visitato&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**Figura 1**: il File di testo contiene la data e ora ultima recensione di un libro è stato visitato ([fare clic per visualizzare l'immagine con dimensioni normali](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))


Distribuire l'applicazione web nell'ambiente di produzione e quindi visitare di hosting *insegnare manualmente ASP.NET 3.5 in 24 ore* pagina revisione del libro. A questo punto si dovrebbe vedere sia la pagina di revisione del libro come normale o il messaggio di errore riportato nella figura 2. Alcuni provider host web concedere autorizzazioni di scrittura per l'account del computer ASP.NET anonimo, in cui i casi la pagina funzionerà senza errori. Se, tuttavia, il provider di hosting web non consente l'accesso in scrittura per l'account anonimo un' [ `UnauthorizedAccessException` eccezioni](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) viene generato quando il `TYASP35.aspx` pagina tenta di scrivere la data e ora correnti per il `LastTYASP35Access.txt` file.


[![L'Account del computer predefinito usato da IIS non dispone delle autorizzazioni per scrivere nel File System](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**Figura 2**: The Default macchina Account usato da IIS viene non dispone delle autorizzazioni in scrittura nel File System ([fare clic per visualizzare l'immagine con dimensioni normali](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))


La buona notizia è che la maggior parte dei provider di host web sono una sorta di strumento di autorizzazioni che è possibile specificare le autorizzazioni del file nel sito Web. Concedere l'accesso in scrittura account ASP.NET anonimo alla directory radice e quindi visitare di nuovo la pagina di revisione del libro. (Se necessario, contattare il provider di hosting web per ottenere assistenza su come concedere autorizzazioni di scrittura per l'account ASP.NET predefinito.) Questa volta deve caricare la pagina senza errori e `LastTYASP35Access.txt` file deve essere creata correttamente.

Il take della questione è che poiché il Server di sviluppo ASP.NET opera in un contesto di sicurezza diversi da IIS, è possibile che le pagine ASP.NET di leggere o scrivere nel file System, leggere o scrivere al registro eventi di Windows, o leggere o scrivere in di Windows  Registro di sistema verrà funziona come previsto in sviluppo ma generano eccezioni quando in produzione. Quando si compila un'applicazione web che verrà distribuita in un ambiente di hosting web condivisa, non leggere o scrivere nel registro eventi o il Registro di sistema di Windows. Prendere anche nota di tutte le pagine ASP.NET che leggono da o scrivere nel file System come potrebbe essere necessario concedono autorizzazioni di lettura e scrittura dei privilegi nelle cartelle appropriate nell'ambiente di produzione.

## <a name="differences-on-serving-static-content"></a>Differenze nella gestione del contenuto statico

Un'altra differenza principale tra IIS e il Server di sviluppo ASP.NET è modo in cui gestiscono le richieste per il contenuto statico. Ogni richiesta in arrivo nel Server di sviluppo ASP.NET, ad esempio per una pagina ASP.NET, un'immagine o un file JavaScript, viene elaborato dal runtime di ASP.NET. Per impostazione predefinita, IIS chiama solo il runtime ASP.NET quando viene ricevuta una richiesta per una risorsa ASP.NET, ad esempio una pagina web ASP.NET, un servizio Web e così via. Le richieste per il contenuto statico - immagini, file CSS, i file JavaScript, i file PDF, file ZIP e simili - vengono recuperate da IIS senza il coinvolgimento del runtime di ASP.NET. (È possibile indicare a IIS di funzionare con il runtime ASP.NET durante l'utilizzo di contenuto statico; in questa esercitazione per altre informazioni, consultare la sezione "Eseguire l'autenticazione basata su form e l'autenticazione di URL nei file statici con IIS 7").

Il runtime ASP.NET esegue una serie di passaggi per generare il contenuto richiesto, tra cui (identificare il richiedente) di autenticazione e autorizzazione (che determina se il richiedente è autorizzato a visualizzare il contenuto richiesto). È un tipo di autenticazione più diffusi *l'autenticazione basata su form*, in cui un utente viene identificato immettendo le proprie credenziali, in genere un nome utente e password - nelle caselle di testo in una pagina web. Dopo la convalida le proprie credenziali, il sito Web archivia un' *ticket di autenticazione* cookie nel browser dell'utente, che viene inviato con ogni richiesta successiva per il sito Web e viene usata per autenticare l'utente. Inoltre, è possibile specificare *autorizzazione URL* le regole che determinano quali utenti possono o non è possibile accedere a una particolare cartella. Molti siti Web ASP.NET usano l'autenticazione basata su form e l'autorizzazione URL per supportare gli account utente e definire le parti del sito accessibili solo agli utenti autenticati o agli utenti che appartengono a un determinato ruolo.

> [!NOTE]
> Per un esame approfondito di ASP. L'autenticazione basata su form di NET, l'autorizzazione URL e altre funzionalità relative all'account utente, assicurarsi di consultare mio [sito Web di sicurezza esercitazioni](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Prendere in considerazione un sito Web che supporta gli account utente con autorizzazione basata su form e dispone di una cartella che, utilizzando l'autorizzazione URL, è configurata per consentire solo agli utenti autenticati. Si supponga che questa cartella contiene pagine ASP.NET e i file PDF e che il motivo è che solo gli utenti autenticati possono visualizzare questi file PDF.

Cosa accade se un visitatore tenta di visualizzare uno di questi file PDF immettendo l'URL direttamente nella barra degli indirizzi del suo browser? Per individuare, è possibile creare una nuova cartella nel sito le recensioni dei libri, aggiungere alcuni file PDF e configurare il sito per usare l'autorizzazione URL impedire a utenti anonimi che visitano questa cartella. Se si scarica l'applicazione demo qui noterete che creato una cartella denominata `PrivateDocs` e aggiungere un file PDF da my [sito Web di sicurezza esercitazioni](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (come adattano!). Il `PrivateDocs` cartella contiene inoltre un `Web.config` file che specifica le regole di autorizzazione URL per negare agli utenti anonimi:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

Infine, è stata configurata l'applicazione web per usare l'autenticazione basata su form aggiornando il file Web. config nella directory radice, la sostituzione:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

con:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

Utilizzo del Server di sviluppo ASP.NET, visitare il sito e immettere l'URL diretto per uno dei file PDF nella barra degli indirizzi del browser. Se è stato scaricato il sito Web associato a questa esercitazione l'URL dovrebbe essere simile a: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Inserendo l'URL nella barra degli indirizzi determina il browser inviare una richiesta al Server di sviluppo ASP.NET per il file. Il Server di sviluppo ASP.NET inoltra la richiesta al runtime di ASP.NET per l'elaborazione. Poiché è stata non ancora eseguito l'accesso e il `Web.config` nella `PrivateDocs` cartella è configurata per negare l'accesso anonimo, il runtime ASP.NET us reindirizza automaticamente alla pagina di accesso, `Login.aspx` (vedere la figura 3). Quando si reindirizza l'utente alla pagina di accesso, ASP.NET include un `ReturnUrl` parametro querystring che indica che la pagina utente tentava di visualizzazione. Dopo aver completato l'accesso l'utente può essere restituito da questa pagina.


[![Gli utenti non autorizzati vengono automaticamente reindirizzati alla pagina di accesso](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**Figura 3**: gli utenti non autorizzati vengono automaticamente reindirizzati alla pagina di accesso ([fare clic per visualizzare l'immagine con dimensioni normali](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))


Ora vediamo come il comportamento è in produzione. Distribuire l'applicazione e immettere l'URL diretto tra i file PDF nel `PrivateDocs` cartella nell'ambiente di produzione. Verrà chiesto di specificare il browser per inviare una richiesta di IIS per il file. Poiché viene richiesto un file statico, IIS recupera e restituisce il file senza richiamare il runtime ASP.NET. Di conseguenza, si è verificato alcun controllo di autorizzazione URL eseguita. il contenuto del file PDF presumibilmente privati è accessibile a chiunque ne conosca l'URL diretto al file.


[![Gli utenti anonimi possono scaricare i file di formato PDF privato immettendo l'URL diretto al File](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**Figura 4**: anonimo gli utenti possono scaricare il privata PDF i file da immettendo l'URL diretto al File ([fare clic per visualizzare l'immagine con dimensioni normali](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Eseguire l'autenticazione basata su form e URL nei file statici con IIS 7

Esistono due tecniche che è possibile utilizzare per proteggere il contenuto statico da utenti non autorizzati. IIS 7 introdotto il *pipeline integrata*, che subirebbero del flusso di lavoro di IIS con flusso di lavoro del runtime ASP.NET. In breve, è possibile indicare a IIS per richiamare l'autenticazione del runtime ASP.NET e moduli di autorizzazione tutte le richieste in ingresso (tra cui il contenuto statico, ad esempio i file PDF). Contattare il provider di hosting web per scoprire come configurare il sito Web per l'uso della pipeline integrata.

Dopo aver configurato IIS per l'uso della pipeline integrata aggiungere il markup seguente per il `Web.config` file nella directory radice:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

Questo codice indica a IIS 7 per usare i moduli di autenticazione e autorizzazione basate su ASP.NET. Ridistribuire l'applicazione e quindi visitare di nuovo il file con estensione PDF. Questa volta quando IIS gestisce la richiesta offre la logica di autenticazione e autorizzazione del runtime ASP.NET un'opportunità per esaminare la richiesta. Poiché solo gli utenti autenticati sono autorizzati a visualizzare il contenuto nel `PrivateDocs` cartella, il visitatore anonimo viene automaticamente reindirizzato alla pagina di accesso (vedere la figura 3).

> [!NOTE]
> Se il provider di hosting web Usa ancora IIS 6 è possibile usare la funzionalità di pipeline integrata. Una soluzione alternativa consiste nell'inserire i documenti privati in una cartella che impedisce l'accesso HTTP (ad esempio `App_Data`) e quindi creare una pagina per gestire tali documenti. Questa pagina può essere chiamata `GetPDF.aspx`e viene passato il nome del file PDF tramite un parametro di stringa di query. Il `GetPDF.aspx` pagina eseguite prima di tutto verificare che l'utente dispone dell'autorizzazione per visualizzare il file e, se in questo caso, usare il [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) metodo per inviare il contenuto del file PDF richiesto al client richiedente. Questa tecnica funzionerebbe anche per IIS 7 Se non necessario abilitare la pipeline integrata.


## <a name="summary"></a>Riepilogo

Applicazioni Web in un ambiente di produzione sono ospitate usando software del server web IIS Microsoft. Nell'ambiente di sviluppo, tuttavia, l'applicazione può essere ospitata utilizzando IIS o il Server di sviluppo ASP.NET. In teoria, lo stesso software di server web deve essere utilizzato in entrambi gli ambienti perché usi un software diverso aggiunge un'altra variabile nella combinazione di. Tuttavia, la semplicità d'uso del Server di sviluppo ASP.NET rende interessante la scelta nell'ambiente di sviluppo. La buona notizia è che sono presenti solo poche differenze fondamentali tra il Server di sviluppo ASP.NET e IIS, e se è a conoscenza di queste differenze è possibile intervenire per garantire che l'applicazione funziona e comporta nello stesso modo indipendentemente del ambiente.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Integrazione di ASP.NET con IIS 7.0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Uso dell'autenticazione forum ASP.NET con tutti i tipi di contenuto in IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (Video)
- [Server Web in Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Precedente](common-configuration-differences-between-development-and-production-vb.md)
> [Successivo](deploying-a-database-vb.md)
