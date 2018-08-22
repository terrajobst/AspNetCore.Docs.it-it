---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: L'elaborazione di eccezioni non gestite (VB) | Microsoft Docs
author: rick-anderson
description: Quando si verifica un errore di runtime in un'applicazione web nell'ambiente di produzione è importante per notificare a uno sviluppatore e di registrare l'errore, in modo che potrebbe essere diagnosticato in un oggetto la...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 29ea7f376f61c242ab93cfb71e1a7b435c575482
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823882"
---
<a name="processing-unhandled-exceptions-vb"></a>L'elaborazione di eccezioni non gestite (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples) ([procedura per il download](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Quando si verifica un errore di runtime in un'applicazione web nell'ambiente di produzione è importante per notificare a uno sviluppatore e di registrare l'errore, in modo che potrebbe essere diagnosticato in un secondo momento nel tempo. Questa esercitazione offre una panoramica su come ASP.NET elabora gli errori in fase di esecuzione ed esamina un modo per eseguire codice personalizzato ogni volta che un bolle di eccezione non gestita al runtime di ASP.NET.


## <a name="introduction"></a>Introduzione

Quando si verifica un'eccezione non gestita in un'applicazione ASP.NET, viene propagato al runtime di ASP.NET, che genera il `Error` evento e viene visualizzata la pagina di errore appropriato. Esistono tre tipi diversi di pagine di errore: il Runtime errore giallo schermata di morte (YSOD); i dettagli dell'eccezione YSOD; e pagine errori personalizzati. Nel [esercitazione precedente](displaying-a-custom-error-page-vb.md) è stata configurata l'applicazione per usare una pagina di errore personalizzato per gli utenti remoti e il YSOD Dettagli eccezione per gli utenti che visitano in locale.

Utilizzo di una pagina di errore personalizzato Converto corrispondente l'aspetto del sito è preferito per il valore predefinito YSOD di errore di Runtime, ma la visualizzazione di una pagina di errore personalizzata è solo una parte di una soluzione di gestione degli errori completo. Quando si verifica un errore in un'applicazione nell'ambiente di produzione, è importante che gli sviluppatori ricevono una notifica dell'errore, in modo che possano scoprirle la causa dell'eccezione e risolvere il problema. Inoltre, i dettagli dell'errore devono essere registrati in modo che l'errore può essere esaminato e diagnosticato in un secondo momento nel tempo.

Questa esercitazione illustra come accedere ai dettagli di un'eccezione non gestita in modo che essi possono essere registrate e uno sviluppatore di una notifica. Le due esercitazioni seguendo questo esplorare librerie di registrazione errore che, dopo un po' di configurazione, verranno automaticamente notificare agli sviluppatori di errori di runtime e i relativi dettagli di log.

> [!NOTE]
> Le informazioni esaminate in questa esercitazione sono particolarmente utile se è necessario elaborare le eccezioni non gestite in modo esclusivo o personalizzato. Nei casi in cui è solo necessario registrare l'eccezione e invia una notifica di uno sviluppatore, con un'errore durante la registrazione libreria è la soluzione. Le due esercitazioni offrono una panoramica delle due librerie di questo tipo.


## <a name="executing-code-when-theerrorevent-is-raised"></a>L'esecuzione di codice quando il`Error`viene generato l'evento

Eventi forniscono un oggetto di un meccanismo per segnalare che si è verificato qualcosa di interessante e per un altro oggetto eseguire il codice nella risposta. Uno sviluppatore ASP.NET si è abituati a pensare in termini di eventi. Se si desidera eseguire il codice quando il visitatore sceglie un pulsante particolare, si crea un gestore eventi per tale pulsante `Click` eventi e inserire il codice non esiste. Dato che il runtime ASP.NET genera relativi [ `Error` evento](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) ogni volta che si verifica un'eccezione non gestita, ne consegue che il codice per la registrazione dei dettagli dell'errore verrà inserito in un gestore eventi. Ma come si crea un gestore eventi per il `Error` evento?

Il `Error` eventi sono uno dei molti eventi di [ `HttpApplication` classe](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) che vengono generati in determinate fasi della pipeline HTTP nel corso della durata di una richiesta. Ad esempio, il `HttpApplication` della classe [ `BeginRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) viene generato all'inizio di ogni richiesta; relativo [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) viene generato quando un modulo di protezione ha identificato il richiedente. Questi `HttpApplication` eventi offrono allo sviluppatore un mezzo per eseguire la logica personalizzata in vari momenti nel corso della durata di una richiesta.

I gestori eventi per il `HttpApplication` eventi possono essere inseriti in un file speciale denominato `Global.asax`. Per creare questo file nel sito Web, aggiungere un nuovo elemento alla radice del proprio sito Web usando il modello di classe di applicazione globale con il nome `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Figura 1**: aggiungere `Global.asax` all'applicazione Web  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](processing-unhandled-exceptions-vb/_static/image3.png))

Il contenuto e la struttura del `Global.asax` file creato da Visual Studio differisce leggermente in base al fatto che si usa un progetto di applicazione Web (WAP) o un progetto sito Web (WSP). Con un WAF, le `Global.asax` viene implementata come due file diversi - `Global.asax` e `Global.asax.vb`. Il `Global.asax` file non contiene alcun elemento, ma un `@Application` direttiva che fa riferimento il `.vb` file; l'evento in cui sono stati definiti gestori di interesse il `Global.asax.vb` file. Per WSPs, viene creato solo un singolo file, `Global.asax`, e i gestori di eventi sono definiti in un `<script runat="server">` blocco.

Il `Global.asax` file creato in un WAF dal modello di classe di applicazione globale di Visual Studio include i gestori di eventi denominati `Application_BeginRequest`, `Application_AuthenticateRequest`, e `Application_Error`, che sono i gestori eventi per il `HttpApplication` eventi `BeginRequest`, `AuthenticateRequest`, e `Error`, rispettivamente. Sono disponibili anche i gestori eventi denominati `Application_Start`, `Session_Start`, `Application_End`, e `Session_End`, quali sono i gestori di eventi che vengono generati all'avvio dell'applicazione web, quando viene avviata una nuova sessione, al termine dell'applicazione e quando termina una sessione, rispettivamente. Il `Global.asax` file creato da Visual Studio in un WSP contiene solo le `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, e `Session_End` gestori eventi.

> [!NOTE]
> Quando si distribuisce l'applicazione ASP.NET è necessario copiare il `Global.asax` file nell'ambiente di produzione. Il `Global.asax.vb` file, che viene creato in WAP, non dovrà essere copiati nell'ambiente di produzione perché questo codice viene compilato nell'assembly del progetto.


I gestori di eventi creati dal modello di classe di applicazione globale di Visual Studio non sono esaustivi. È possibile aggiungere un gestore eventi per qualsiasi `HttpApplication` evento assegnando un gestore dell'evento `Application_EventName`. Ad esempio, è possibile aggiungere il codice seguente per il `Global.asax` file per creare un gestore eventi per il [ `AuthorizeRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

Analogamente, è possibile rimuovere i gestori degli eventi creati dal modello di classe di applicazione globale non sono necessari. Per questa esercitazione ti chiediamo solo un gestore eventi per il `Error` eventi; è possibile rimuovere altri gestori di eventi dal `Global.asax` file.

> [!NOTE]
> *Moduli HTTP* offrono un altro modo per definire i gestori eventi per `HttpApplication` gli eventi. Moduli HTTP vengono creati come un file di classe che può essere inserito direttamente all'interno del progetto di applicazione web o suddivisi in una libreria di classi separata. Poiché essi possono essere suddivisi tra una libreria di classi, moduli HTTP offrono un modello più flessibile e riutilizzabile per la creazione di `HttpApplication` gestori eventi. Mentre il `Global.asax` è specifico all'applicazione web in cui risiedono, moduli HTTP possono essere compilati in assembly, a questo punto aggiungere il modulo HTTP a un sito Web è semplice come l'eliminazione di assembly `Bin` cartella e la registrazione di Modulo in `Web.config`. Questa esercitazione non ha l'aspetto nella creazione e uso di moduli HTTP, ma le librerie di registrazione degli errori due usate le esercitazioni seguenti mostrano due vengono implementate come moduli HTTP. Per altre informazioni sui vantaggi di moduli HTTP si intende [usando moduli e gestori HTTP per creare componenti ASP.NET collegabili](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Il recupero delle informazioni relative all'eccezione non gestita

A questo punto si dispone di un file Global. asax con un `Application_Error` gestore dell'evento. Quando si esegue questo gestore eventi è necessario inviare una notifica di uno sviluppatore dell'errore e i dettagli di log. Per eseguire queste operazioni è necessario innanzitutto determinare i dettagli dell'eccezione generata. Usare l'oggetto di Server [ `GetLastError` metodo](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) per recuperare i dettagli dell'eccezione non gestita che ha causato il `Error` dell'evento da generare.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

Il `GetLastError` metodo restituisce un oggetto di tipo `Exception`, ovvero il tipo di base per tutte le eccezioni in .NET Framework. Tuttavia, nel codice precedente sono cast l'oggetto di eccezione restituito da `GetLastError` in un `HttpException` oggetto. Se il `Error` evento viene generato perché è stata generata un'eccezione durante l'elaborazione di una risorsa di ASP.NET, quindi viene eseguito il wrapping l'eccezione generata all'interno di un `HttpException`. Per ottenere l'eccezione che ha causato l'utilizzo di eventi di errore di `InnerException` proprietà. Se il `Error` è stato generato l'evento a causa di un'eccezione basata su HTTP, ad esempio una richiesta per una pagina inesistente, un `HttpException` viene generata un'eccezione, ma non dispone di un'eccezione interna.

Il codice seguente usa il `GetLastErrormessage` per recuperare le informazioni relative all'eccezione che ha attivato il `Error` evento, l'archiviazione di `HttpException` in una variabile denominata `lastErrorWrapper`. Quindi archivia il tipo di messaggio e analisi dello stack dell'eccezione origine in tre variabili stringa, un controllo per verificare se il `lastErrorWrapper` è l'eccezione che ha attivato il `Error` eventi (nel caso di eccezioni basato su HTTP) o se è semplicemente un wrapper per un'eccezione generata durante l'elaborazione della richiesta.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

A questo punto si dispongano di tutte le informazioni necessarie per scrivere codice che verrà registrati i dettagli dell'eccezione in una tabella di database. È possibile creare una tabella di database con colonne per ognuno dei dettagli dell'errore di interesse - il tipo, il messaggio, l'analisi dello stack e così via - assieme ad altre utili informazioni, ad esempio l'URL della pagina richiesta e il nome dell'utente attualmente connesso. Nel `Application_Error` gestore eventi verrebbe quindi connettersi al database e inserire un record nella tabella. Analogamente, è possibile aggiungere codice per uno sviluppatore dell'errore tramite posta elettronica di avviso.

Le librerie di registrazione errore esaminate nelle esercitazioni seguenti due forniscono tale funzionalità per impostazione predefinita, in modo che non è necessario creare questa registrazione degli errori e notifica. Tuttavia, per illustrare che il `Error` evento viene generato e che il `Application_Error` gestore eventi può essere utilizzato per registrare i dettagli dell'errore e inviare una notifica di uno sviluppatore, aggiungere codice che invia una notifica di uno sviluppatore quando si verifica un errore.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Avvisare gli sviluppatori quando si verifica un'eccezione non gestita

Quando si verifica un'eccezione non gestita nell'ambiente di produzione è importante avvisare il team di sviluppo in modo da poter valutare l'errore e determinare cosa è necessario eseguire alcune azioni. Ad esempio, se è presente un errore durante la connessione al database, è necessario in double controllare la stringa di connessione e, forse, aprire un ticket di supporto con le società di hosting web. Se si è verificata l'eccezione a causa di un errore di programmazione, potrebbe essere necessario codice aggiuntivo o la logica di convalida da aggiungere per evitare tali errori in futuro.

Le classi .NET Framework nel [ `System.Net.Mail` dello spazio dei nomi](https://msdn.microsoft.com/library/system.net.mail.aspx) rendono semplice inviare un messaggio di posta elettronica. Il [ `MailMessage` classe](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) rappresenta un messaggio di posta elettronica e dispone di proprietà quali `To`, `From`, `Subject`, `Body`, e `Attachments`. Il `SmtpClass` viene usato per inviare un `MailMessage` utilizza un server SMTP specificato dell'oggetto; le impostazioni del server SMTP possono essere specificate a livello di programmazione o in modo dichiarativo nel [ `<system.net>` elemento](https://msdn.microsoft.com/library/6484zdc1.aspx) nel `Web.config file`. Per altre informazioni sull'invio di posta elettronica dei messaggi in un'applicazione ASP.NET consultare il mio articolo [l'invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)e il [domande frequenti su System.NET. Mail](http://systemnetmail.com/).

> [!NOTE]
> Il `<system.net>` elemento contiene le impostazioni del server SMTP utilizzate dal `SmtpClient` classe quando si invia un messaggio di posta elettronica. Le società di web hosting probabilmente ha un server SMTP che è possibile usare per inviare posta elettronica dall'applicazione. Per informazioni sulle impostazioni di server SMTP, che è consigliabile usare nell'applicazione web, vedere sezione relativa al supporto dell'host web.


Aggiungere il codice seguente per il `Application_Error` gestore eventi per inviare uno sviluppatore di un messaggio di posta elettronica quando si verifica un errore:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Mentre il codice sopra riportato è piuttosto lungo, la maggior parte di esso crea il codice HTML visualizzato nel messaggio di posta elettronica inviato agli sviluppatori. Inizia il codice facendo riferimento il `HttpException` restituito dal `GetLastError` metodo (`lastErrorWrapper`). L'eccezione che è stato generato dalla richiesta viene recuperato tramite `lastErrorWrapper.InnerException` e viene assegnato alla variabile `lastError`. Il tipo di messaggio e stack le informazioni di traccia viene recuperate da `lastError` e archiviati in tre variabili di stringa.

Successivamente, una `MailMessage` oggetto denominato `mm` viene creato. Il corpo del messaggio di posta elettronica è HTML formattato e visualizza l'URL della pagina richiesta, il nome dell'utente attualmente connesso e informazioni sull'eccezione (il tipo di messaggio e analisi dello stack). Uno degli aspetti interessanti le `HttpException` classe è che è possibile generare il codice HTML utilizzato per creare l'eccezione dettagli giallo schermata di morte (YSOD) chiamando il [metodo GetHtmlErrorMessage](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Questo metodo viene usato qui per recuperare il markup YSOD Dettagli eccezione e aggiungerlo al messaggio di posta elettronica come allegato. Attenzione: se l'eccezione che ha attivato la `Error` evento è verificata un'eccezione basata su HTTP (ad esempio una richiesta per una pagina inesistente) il `GetHtmlErrorMessage` metodo restituirà `null`.

Il passaggio finale consiste nell'inviare il `MailMessage`. Questa operazione viene eseguita creando un nuovo `SmtpClient` metodo e chiamando il metodo relativo `Send` (metodo).

> [!NOTE]
> Prima di usare questo codice nell'applicazione web è opportuno modificare i valori nel `ToAddress` e `FromAddress` costanti da support@example.com a qualsiasi messaggio di posta elettronica indirizzo l'e-mail di notifica di errore devono essere inviati a e hanno origine da. È anche necessario specificare le impostazioni server SMTP nel `<system.net>` sezione `Web.config`. Consultare il provider di hosting web per determinare le impostazioni del server SMTP da utilizzare.


Con questo codice attivo ogni volta che si verifica un errore allo sviluppatore viene inviato un messaggio di posta elettronica che riepiloga l'errore e include il YSOD. Nell'esercitazione precedente è stato illustrato un errore di runtime che visitano Genre.aspx e passando un valore non valido `ID` valore tramite la stringa di query, ad esempio `Genre.aspx?ID=foo`. Visita la pagina con il `Global.asax` file sul posto genera la stessa esperienza utente, come nell'esercitazione precedente, nell'ambiente di sviluppo si continuerà a vedere l'eccezione dettagli giallo schermata di morte, mentre nell'ambiente di produzione sarà vedere la pagina di errore personalizzato. Oltre a questo comportamento esistente, lo sviluppatore viene inviato un messaggio di posta elettronica.

**Figura 2** Mostra il messaggio di posta elettronica ricevuto quando si visita `Genre.aspx?ID=foo`. Il corpo del messaggio di posta elettronica sono riepilogate le informazioni sull'eccezione, mentre il `YSOD.htm` allegato viene visualizzato il contenuto disponibile nella finestra di YSOD Dettagli eccezione (vedere **figura 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Figura 2**: lo sviluppatore viene inviato una notifica di posta elettronica ogni volta che si verifica un'eccezione non gestita  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Figura 3**: notifica di posta elettronica include i dettagli dell'eccezione YSOD come allegato  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Che cosa succede su come utilizzare la pagina di errore personalizzato?

Questa esercitazione ha illustrato come usare `Global.asax` e il `Application_Error` gestore eventi per eseguire codice quando si verifica un'eccezione non gestita. In particolare, è stato usato questo gestore dell'evento per notificare lo sviluppatore di un errore. può essere esteso in modo da registrare anche i dettagli dell'errore in un database. La presenza del `Application_Error` gestore eventi non influisce sul esperienza dell'utente finale. Sono ancora visualizzata la pagina degli errori configurato, sia esso YSOD di dettagli di errore, YSOD l'errore di Runtime o la pagina di errore personalizzato.

È naturale chiedersi se il `Global.asax` file e `Application_Error` eventi sono necessario quando si usa una pagina di errore personalizzato. Quando si verifica un errore all'utente viene visualizzata la pagina di errore personalizzato perché non possiamo è inseriamo il codice per notificare lo sviluppatore e registrare i dettagli dell'errore nella classe code-behind della pagina di errore personalizzato? Mentre è certamente possibile aggiungere codice alla classe code-behind della pagina errori personalizzati non si sono disponibili per i dettagli dell'eccezione che ha attivato il `Error` eventi quando si usa la tecnica sono stati presentati nell'esercitazione precedente. Chiama il `GetLastError` metodo dalla pagina di errore personalizzato restituisce `Nothing`.

Il motivo per questo comportamento è che la pagina di errore personalizzato viene raggiunta tramite un reindirizzamento. Quando un'eccezione non gestita raggiunge il runtime ASP.NET il motore ASP.NET genera relativi `Error` eventi (che viene eseguito il `Application_Error` gestore eventi) e quindi *reindirizza* l'utente alla pagina di errore personalizzato tramite l'esecuzione di una `Response.Redirect(customErrorPageUrl)`. Il `Response.Redirect` metodo invia una risposta al client con un codice di stato HTTP 302, che indicano il browser per richiedere un nuovo URL, vale a dire la pagina di errore personalizzato. Il browser richiede quindi automaticamente la nuova pagina. È possibile indicare che la pagina di errore personalizzato è stato richiesto separatamente dalla pagina in cui è verificato l'errore poiché viene modificata la barra degli indirizzi del browser all'URL pagina di errore personalizzata (vedere **figura 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Figura 4**: quando si verifica un errore nel Browser viene reindirizzato all'URL pagina di errore personalizzato  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](processing-unhandled-exceptions-vb/_static/image12.png))

Il risultato finale è che la richiesta in cui si è verificata un'eccezione non gestita termina quando il server risponde con il reindirizzamento HTTP 302. La richiesta successiva alla pagina di errore personalizzato è una nuova richiesta. a questo punto ASP.NET motore ha eliminato le informazioni sull'errore e, inoltre, non è possibile associare all'eccezione non gestita nella richiesta precedente con la nuova richiesta per la pagina di errore personalizzato. È per questo motivo `GetLastError` restituisce `null` quando viene chiamato dalla pagina di errore personalizzato.

Tuttavia, è possibile inserire la pagina di errore personalizzato eseguita durante la stessa richiesta che ha causato l'errore. Il [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) metodo trasferisce l'esecuzione all'URL specificato e lo elabora all'interno della stessa richiesta. È possibile spostare il codice `Application_Error` gestore eventi per classe code-behind della pagina di errore personalizzata, sostituirla in `Global.asax` con il codice seguente:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

A questo punto quando si verifica un'eccezione non gestita di `Application_Error` gestore dell'evento trasferisce il controllo alla pagina di errore personalizzato appropriato in base al codice di stato HTTP. Poiché il controllo è stato trasferito, la pagina di errore personalizzato ha accesso alle informazioni relative all'eccezione non gestita tramite `Server.GetLastError` e possono notificare a uno sviluppatore dell'errore e i dettagli di log. Il `Server.Transfer` chiamata si arresta il motore ASP.NET da reindirizzare l'utente alla pagina di errore personalizzato. Contenuto della pagina errori personalizzati, invece, viene restituito come la risposta alla pagina che ha generato l'errore.

## <a name="summary"></a>Riepilogo

Quando si verifica un'eccezione non gestita in un'applicazione web ASP.NET il runtime ASP.NET genera il `Error` eventi e consente di visualizzare la pagina di errore configurata. Si riceverà lo sviluppatore dell'errore, i dettagli di log o elaborarli in qualche altro modo, tramite la creazione di un gestore eventi per l'evento di errore. Esistono due modi per creare un gestore eventi per `HttpApplication` eventi come `Error`: nel `Global.asax` file o da un modulo HTTP. Questa esercitazione ha illustrato come creare un `Error` gestore dell'evento nel `Global.asax` file che invia una notifica agli sviluppatori di un errore per mezzo di un messaggio di posta elettronica.

Creazione di un `Error` gestore eventi è utile se è necessario elaborare le eccezioni non gestite in modo esclusivo o personalizzato. Tuttavia, crearne una propria `Error` gestore dell'evento per registrare l'eccezione o per inviare una notifica di uno sviluppatore non è ottimizzare l'uso del tuo tempo perché esistono già librerie di registrazione errore gratuito e facile da usare che possono essere configurata in pochi minuti. Le due esercitazioni esaminare due librerie di questo tipo.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Cenni preliminari sui gestori HTTP e moduli HTTP ASP.NET](https://support.microsoft.com/kb/307985)
- [Risponde correttamente alle eccezioni non gestite - l'elaborazione di eccezioni non gestite](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` Classe e l'oggetto di applicazione ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [I gestori HTTP e moduli HTTP in ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [L'invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [La comprensione di `Global.asax` File](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Uso di moduli e gestori HTTP per creare componenti ASP.NET collegabili](https://msdn.microsoft.com/library/aa479332.aspx)
- [Utilizzo di ASP.NET `Global.asax` File](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Utilizzo di `HttpApplication` istanze](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Precedente](displaying-a-custom-error-page-vb.md)
> [Successivo](logging-error-details-with-asp-net-health-monitoring-vb.md)
