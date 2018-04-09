---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: Principali differenze tra IIS e il Server di sviluppo ASP.NET (VB) | Documenti Microsoft
author: rick-anderson
description: Durante il test di un'applicazione ASP.NET localmente, probabilità sono che si utilizza il Server Web di sviluppo ASP.NET. Tuttavia, il sito Web di produzione è probabilmente pow...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 47b1959f9b92d161da0476b274c8154333ad80dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>Differenze principali tra IIS e il Server di sviluppo ASP.NET (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> Durante il test di un'applicazione ASP.NET localmente, probabilità sono che si utilizza il Server Web di sviluppo ASP.NET. Tuttavia, il sito Web di produzione è probabilmente IIS spenta. Esistono alcune differenze tra la modalità di gestire le richieste di questi server web e queste differenze possono avere conseguenze. In questa esercitazione illustra alcune delle differenze più pertinenti.


## <a name="introduction"></a>Introduzione

Ogni volta che un utente accede a un'applicazione ASP.NET il browser invia una richiesta per il sito Web. Tale richiesta viene prelevata dal software di server web, che interagisce con il runtime di ASP.NET per generare e restituire il contenuto per la risorsa richiesta. Il[**si** nternet **si** nformation **S** linea (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) sono una suite di servizi che forniscono funzionalità comuni basati su Internet per Server di Windows. IIS è il server web di uso più comune per le applicazioni ASP.NET in ambienti di produzione; è probabile che il software del server web utilizzato dal provider host web per servire l'applicazione ASP.NET. IIS consente inoltre come il software del server web nell'ambiente di sviluppo, anche se ciò comporta l'installazione di IIS e configurarlo correttamente.


Il Server di sviluppo ASP.NET è un'opzione di server web alternativo per l'ambiente di sviluppo. incluso in ed è integrato in Visual Studio. A meno che l'applicazione web è stata configurata per utilizzare IIS, il Server di sviluppo ASP.NET viene automaticamente avviato e utilizzato come server web la prima volta che si visita una pagina web da Visual Studio. Le applicazioni web demo creato nuovamente il [ *determinare quali file devono essere distribuiti* ](determining-what-files-need-to-be-deployed-vb.md) esercitazione sono state entrambe le applicazioni web basate sul sistema di file che non sono state configurate per utilizzare IIS. Pertanto, durante la visita di questi siti Web da Visual Studio viene utilizzato il Server di sviluppo ASP.NET.

In una situazione ideale sarebbe identico gli ambienti di sviluppo e produzione. Tuttavia, come descritto nell'esercitazione precedente non è raro che gli ambienti per le impostazioni di configurazione diversi. Utilizzo di software del server web diverso negli ambienti aggiunge un'altra variabile che deve essere prese in considerazione quando si distribuisce un'applicazione. In questa esercitazione vengono illustrate le differenze principali tra IIS e il Server di sviluppo ASP.NET. A causa di queste differenze esistono scenari in cui il codice eseguito completamente nell'ambiente di sviluppo genera un'eccezione o si comporta in modo diverso quando eseguito nell'ambiente di produzione.

## <a name="security-context-differences"></a>Differenze di contesto di sicurezza

Ogni volta che il software del server web gestisce una richiesta in ingresso associa la richiesta a un contesto di sicurezza specifico. Queste informazioni di contesto di sicurezza utilizzate dal sistema operativo per determinare quali azioni sono consentite dalla richiesta. Ad esempio, una pagina ASP.NET potrebbe includere codice che registra un messaggio in un file su disco. Affinché questa pagina ASP.NET viene eseguita senza errori, il contesto di sicurezza necessario appropriate autorizzazioni a livello di sistema del file, vale a dire scrittura autorizzazioni su tale file.

Il Server di sviluppo ASP.NET associa le richieste in ingresso con il contesto di sicurezza dell'utente attualmente connesso. Se è stato effettuato l'accesso al desktop come amministratore, le pagine ASP.NET servite dal Server di sviluppo ASP.NET avranno gli stessi diritti di accesso come amministratore. Tuttavia, le richieste ASP.NET gestite da IIS sono associate a un account di computer specifico. Per impostazione predefinita, l'account servizio di rete del computer viene utilizzato da IIS versione 6 e 7, anche se il provider di hosting web potrebbe aver configurato un account univoco per ogni cliente. Inoltre, il provider di hosting web sono probabilmente assegnate autorizzazioni limitate per l'account computer. Il risultato è che è possibile pagine web che vengono eseguite senza errori nell'ambiente di sviluppo, ma genera eccezioni correlate alle autorizzazioni quando sono ospitati nell'ambiente di produzione.

Per visualizzare questo tipo di errore nell'azione ho creato una pagina nel sito Web che crea un file su disco che archivia la data e l'ora più recente di un utente recensioni, visualizzare il *insegna manualmente ASP.NET 3.5 nelle 24 ore* esaminare. Per proseguire, aprire il `~/Tech/TYASP35.aspx` pagina e aggiungere il codice seguente per il `Page_Load` gestore eventi:

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> Il [ `File.WriteAllText` metodo](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) crea un nuovo file se non esiste e quindi scrive il contenuto specificato. Se il file esiste già, il contenuto esistente viene sovrascritto.


Successivamente, visitare il *insegna manualmente ASP.NET 3.5 nelle 24 ore* pagina verifica book in ambiente di sviluppo utilizzando il Server di sviluppo ASP.NET. Supponendo che si è connessi al computer con un account che disponga di autorizzazioni adeguate per creare e modificare un file di testo nel sito web viene visualizzato directory radice dell'applicazione la revisione del libro identico al precedente, ma ogni volta che la pagina visitata la data e ora e dell'utente  Indirizzo IP viene archiviato nel `LastTYASP35Access.txt` file. Puntare il browser in questo file. si verrà visualizzato un messaggio simile a quello illustrato nella figura 1.


[![Il File di testo contiene la data e ora ultima revisione del libro è stata visitata&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**Figura 1**: il File di testo contiene la data e ora ultima revisione del libro è stata visitata ([fare clic per visualizzare l'immagine ingrandita](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))


Distribuire l'applicazione web nell'ambiente di produzione e quindi visitare ospitato *insegna manualmente ASP.NET 3.5 nelle 24 ore* pagina verifica manuale. A questo punto la pagina di revisione del libro è visualizzato come normale o il messaggio di errore illustrato nella figura 2. Alcuni provider host web concedere autorizzazioni di scrittura per l'account del computer ASP.NET anonimo, in cui la pagina di case funzionerà senza errori. Se, tuttavia, il provider di hosting web impedisce l'accesso in scrittura per l'account anonimo una [ `UnauthorizedAccessException` eccezione](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) viene generato quando il `TYASP35.aspx` pagina tenta di scrivere la data e ora correnti per il `LastTYASP35Access.txt` file.


[![L'Account del computer predefinito usato da IIS non dispone di autorizzazioni per scrivere nel File System](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**Figura 2**: il predefinito macchina Account utilizzato da IIS Does non dispone delle autorizzazioni di scrittura nel File System ([fare clic per visualizzare l'immagine ingrandita](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))


Buone notizie sono un tipo di strumento di autorizzazioni che consente di specificare le autorizzazioni del file nel sito Web che la maggior parte dei provider di host web. Concedere l'accesso in scrittura account ASP.NET anonimo alla directory radice e quindi nuovamente la pagina di revisione della Rubrica. (Se necessario, contattare il provider di hosting web per ottenere assistenza su come concedere autorizzazioni di scrittura per l'account ASP.NET predefinito.) Questa volta la pagina devono essere caricati senza errori e `LastTYASP35Access.txt` file deve essere creato correttamente.

La distanza qui è che, poiché il Server di sviluppo ASP.NET opera in un contesto di sicurezza diverso rispetto a IIS, è possibile che le pagine ASP.NET di leggere o scrivere nel file System, leggere o scrivere nel registro eventi di Windows, o leggere o scrivere a Windows  Registro di sistema verrà funziona come previsto in sviluppo ma generano eccezioni quando è in produzione. Quando si compila un'applicazione web che sarà distribuito in un ambiente di hosting web condivisa, leggere o scrivere nel registro eventi o il Registro di sistema di Windows. Inoltre, prendere nota di tutte le pagine ASP.NET di leggere o scrivere nel file System come potrebbe essere necessario concedono autorizzazioni di lettura e scrittura dei privilegi nelle cartelle appropriate nell'ambiente di produzione.

## <a name="differences-on-serving-static-content"></a>Differenze nella gestione contenuto statico

Un'altra differenza principale tra IIS e il Server di sviluppo ASP.NET è una modalità di gestione di richieste di contenuto statico. Ogni richiesta che diventa il Server di sviluppo ASP.NET per una pagina ASP.NET, un'immagine o un file JavaScript, viene elaborato dal runtime di ASP.NET. Per impostazione predefinita, IIS richiama solo dal runtime ASP.NET quando arriva una richiesta per una risorsa ASP.NET, ad esempio una pagina web ASP.NET, un servizio Web e così via. Richieste di contenuto statico - immagini, file CSS, file JavaScript, i file PDF, i file ZIP e così via - vengono recuperate da IIS senza richiedere l'intervento del runtime di ASP.NET. (È possibile indicare IIS per funzionare con il runtime ASP.NET per la gestione di contenuto statico; in questa esercitazione per ulteriori informazioni, consultare la sezione "Autenticazione URL nel file statici con IIS 7 e l'autenticazione basata su form di esecuzione")

Il runtime ASP.NET esegue una serie di passaggi per generare il contenuto richiesto, tra cui (che identifica il richiedente) di autenticazione e autorizzazione (consente di determinare se il richiedente dispone dell'autorizzazione per visualizzare il contenuto richiesto). È una forma comune di autenticazione *l'autenticazione basata su form*, in cui un utente è identificato da immettere le proprie credenziali, in genere un nome utente e password - nelle caselle di testo in una pagina web. Durante la convalida le credenziali, il sito Web memorizza un *ticket di autenticazione* cookie nel browser dell'utente, che viene inviato con ogni richiesta successiva per il sito Web e viene usata per autenticare l'utente. Inoltre, è possibile specificare *autorizzazione URL* regole che determinano quali utenti possono o non è possibile accedere a una cartella particolare. Molti siti Web ASP.NET di utilizzare l'autenticazione basata su form e l'autorizzazione URL per supportare gli account utente e definire le parti del sito che sono accessibili agli utenti autenticati o utenti che appartengono a un determinato ruolo solo.

> [!NOTE]
> Per un esame esauriente di ASP. Del NET autenticazione basata su form, autorizzazione URL e altre funzionalità relative all'account utente, assicurarsi di estrarre il [esercitazioni di sicurezza del sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Si consideri un sito Web che supporta gli account utente tramite autorizzazione basata su form e dispone di una cartella, usare l'autorizzazione URL, è configurata per consentire agli utenti autenticati. Si supponga che la cartella contiene pagine ASP.NET e i file PDF e che il motivo è che solo gli utenti autenticati possono visualizzare questi file PDF.

Cosa accade se un visitatore tenta di visualizzare uno di questi file PDF immettendo l'URL direttamente nella barra degli indirizzi del browser dell'utente. Per individuare, si crea una nuova cartella nel sito recensioni, aggiungere alcuni file PDF e configurare il sito per utilizzare l'autorizzazione di URL per impedire a utenti anonimi di visitare questa cartella. Se si scarica applicazione demo, si noterà che ho creato una cartella denominata `PrivateDocs` e aggiunta di un file PDF da my [esercitazioni di sicurezza del sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (come appropriato!). Il `PrivateDocs` cartella contiene inoltre un `Web.config` file che specifica le regole di autorizzazione URL per negare agli utenti anonimi:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

Infine, è stata configurata l'applicazione web per utilizzare l'autenticazione basata su form aggiornando il file Web. config nella directory radice, la sostituzione:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

con:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

Utilizzo del Server di sviluppo ASP.NET, visitare il sito e immettere l'URL diretto per uno dei file PDF nella barra degli indirizzi del browser. Se hai scaricato il sito Web associato a questa esercitazione che l'URL dovrebbe essere simile: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Immettere l'URL nella barra degli indirizzi determina il browser inviare una richiesta al Server di sviluppo ASP.NET per il file. I Server di sviluppo ASP.NET trasferisce la richiesta al runtime di ASP.NET per l'elaborazione. Poiché non è stato ancora effettuato e il `Web.config` nel `PrivateDocs` cartella è configurata per negare l'accesso anonimo, il runtime di ASP.NET ci reindirizza automaticamente alla pagina di accesso, `Login.aspx` (vedere la figura 3). Quando il reindirizzamento dell'utente alla pagina di accesso, ASP.NET include un `ReturnUrl` parametro querystring che indica la pagina a cui ha tentato di visualizzare l'utente. Dopo aver completato l'accesso l'utente può essere restituito da questa pagina.


[![Gli utenti non autorizzati vengono automaticamente reindirizzati alla pagina di accesso](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**Figura 3**: gli utenti non autorizzati vengono automaticamente reindirizzati alla pagina di accesso ([fare clic per visualizzare l'immagine ingrandita](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))


Ora di seguito viene illustrato questo comportamento in produzione. Distribuire l'applicazione e immettere l'URL diretto a uno dei PDF nel `PrivateDocs` cartella nell'ambiente di produzione. Verrà chiesto di specificare il browser per inviare una richiesta di IIS per il file. Poiché è richiesto un file statico, IIS recupera e restituisce il file senza richiamare il runtime di ASP.NET. Di conseguenza, si è verificato alcun controllo di autorizzazione URL eseguita. il contenuto del documento PDF apparentemente private è accessibile a qualsiasi utente che conosce l'URL diretto al file.


[![Gli utenti anonimi scaricare i file PDF privato immettendo l'URL diretto al File](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**Figura 4**: anonimo gli utenti possono scaricare il privata PDF file da immettendo l'URL diretto al File ([fare clic per visualizzare l'immagine ingrandita](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Eseguire l'autenticazione basata su form e l'autenticazione di URL nel file statici con IIS 7

Esistono un paio di tecniche che è possibile utilizzare per proteggere il contenuto statico da utenti non autorizzati. IIS 7 è stato introdotto il *pipeline integrata*, che unisce del flusso di lavoro di IIS con flusso di lavoro del runtime ASP.NET. In breve, è possibile indicare IIS per richiamare l'autenticazione del runtime ASP.NET e i moduli di autorizzazione tutte le richieste in ingresso (incluso il contenuto statico come file PDF). Contattare il provider di hosting web per informazioni su come configurare il sito Web per l'utilizzo di pipeline integrata.

Quando IIS è stato configurato per l'utilizzo di pipeline integrata di aggiungere il markup seguente per il `Web.config` file nella directory radice:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

Questo codice consente di configurare IIS 7, utilizzare i moduli di autenticazione e autorizzazione basate su ASP.NET. Distribuire nuovamente l'applicazione, quindi accedere nuovamente il file PDF. Questa volta quando IIS gestisce la richiesta fornisce la logica di autenticazione e autorizzazione del runtime ASP.NET consente di controllare la richiesta. Poiché solo gli utenti autenticati sono autorizzati a visualizzare il contenuto di `PrivateDocs` cartella, il visitatore anonimo viene automaticamente reindirizzato alla pagina di accesso (vedere la figura 3).

> [!NOTE]
> Se il provider di hosting web utilizza ancora IIS 6 è possibile utilizzare la funzionalità di pipeline integrata. Una soluzione alternativa consiste nell'inserire i documenti privati in una cartella che impedisce l'accesso HTTP (ad esempio `App_Data`) e quindi creare una pagina per gestire questi documenti. Questa pagina è possibile che venga chiamata `GetPDF.aspx`e viene passato il nome del file PDF tramite un parametro di stringa di query. Il `GetPDF.aspx` pagina sarebbe verificare innanzitutto che l'utente dispone dell'autorizzazione per visualizzare il file e, in tal caso, utilizzare il [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) metodo per inviare il contenuto del file PDF richiesto al client richiedente. Questa tecnica viene utilizzata anche per IIS 7 Se non necessario abilitare la pipeline integrata.


## <a name="summary"></a>Riepilogo

Applicazioni Web in un ambiente di produzione sono ospitate tramite software del server web IIS Microsoft. Nell'ambiente di sviluppo, tuttavia, l'applicazione può essere ospitato con IIS o il Server di sviluppo ASP.NET. In teoria, il software del server web stesso deve essere utilizzato in entrambi gli ambienti perché utilizzando software diverso aggiunge un'altra variabile nella combinazione. Tuttavia, la facilità di utilizzo del Server di sviluppo ASP.NET rende un'opzione utile nell'ambiente di sviluppo. Buone notizie sono che esistono solo alcune differenze fondamentali tra IIS e il Server di sviluppo ASP.NET, e se si è conoscenza di queste differenze è possibile eseguire i passaggi per garantire che l'applicazione funziona e funziona nello stesso modo indipendentemente la ambiente.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Integrazione di ASP.NET con IIS 7.0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Utilizzo dell'autenticazione forum ASP.NET con tutti i tipi di contenuto in IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (Video)
- [Server Web in Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Precedente](common-configuration-differences-between-development-and-production-vb.md)
> [Successivo](deploying-a-database-vb.md)
