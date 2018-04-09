---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
title: L'elaborazione delle eccezioni non gestite (c#) | Documenti Microsoft
author: rick-anderson
description: Quando si verifica un errore di runtime in un'applicazione web nell'ambiente di produzione è importante per notificare a uno sviluppatore e di registrare l'errore, in modo che può essere diagnosticata in un la...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 5bc1afd5-2484-4528-b158-ab218ba150e8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: fccd529f5c489ed28f3cb01f07ffdc4452ead36d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="processing-unhandled-exceptions-c"></a>L'elaborazione delle eccezioni non gestite (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs/samples) ([procedura per il download](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Quando si verifica un errore di runtime in un'applicazione web nell'ambiente di produzione è importante per notificare a uno sviluppatore e di registrare l'errore, in modo che può essere diagnosticata in un secondo momento nel tempo. In questa esercitazione viene fornita una panoramica di come ASP.NET elabora gli errori di runtime ed esamina un modo per eseguire codice personalizzato ogni volta che un bolle di eccezione non gestita al runtime di ASP.NET.


## <a name="introduction"></a>Introduzione

Quando un'applicazione ASP.NET si verifica un'eccezione non gestita, propagata al runtime di ASP.NET, che genera il `Error` evento e viene visualizzata la pagina di errore appropriato. Esistono tre tipi diversi di pagine di errore: il Runtime errore giallo schermata di morte (YSOD); i dettagli dell'eccezione YSOD; e pagine errori personalizzati. Nel [esercitazione precedente](displaying-a-custom-error-page-cs.md) è l'applicazione configurata per utilizzare una pagina di errore personalizzata per gli utenti remoti e il YSOD Dettagli eccezione per gli utenti che visitano localmente.

Utilizzo di una pagina di errore personalizzato adatto umane corrispondente l'aspetto del sito è preferito per il valore predefinito YSOD errore di Runtime, ma la visualizzazione di una pagina di errore personalizzato è solo una parte di un errore completo la soluzione di gestione. Quando si verifica un errore in un'applicazione nell'ambiente di produzione, è importante che gli sviluppatori vengono visualizzata una notifica dell'errore in modo che possano Scopri la causa dell'eccezione e risolvere il problema. Inoltre, i dettagli dell'errore devono essere registrati in modo che l'errore può essere esaminato e la diagnosi in un secondo momento nel tempo.

In questa esercitazione viene illustrato come accedere ai dettagli di un'eccezione non gestita in modo che questi possono essere registrati e uno sviluppatore di una notifica. Le due esercitazioni seguente esplorare le librerie di registrazione errore che, dopo un po' di configurazione, vengono automaticamente notificare agli sviluppatori di errori di runtime e i relativi dettagli di log.

> [!NOTE]
> Le informazioni esaminate in questa esercitazione sono particolarmente utile se è necessario elaborare le eccezioni non gestite in modo esclusivo o personalizzato. Nei casi in cui è necessario solo per registrare l'eccezione e la notifica di uno sviluppatore, con un'errore durante la registrazione libreria è il modo per passare. Le due esercitazioni forniscono una panoramica delle due librerie di questo tipo.


## <a name="executing-code-when-theerrorevent-is-raised"></a>L'esecuzione di codice quando il`Error`evento

Eventi forniscono un oggetto di un meccanismo per segnalare che si è verificato qualcosa di interessante e eseguire codice in risposta di un altro oggetto. In qualità di sviluppatore ASP.NET si è abituati a pensare in termini di eventi. Se si desidera eseguire il codice quando il visitatore fa clic su un pulsante, si crea un gestore eventi per tale pulsante `Click` eventi e inserire il codice non esiste. Dato che il runtime ASP.NET genera relativo [ `Error` evento](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) ogni volta che si verifica un'eccezione non gestita, ne consegue che il codice per registrare i dettagli dell'errore verrà attivato in un gestore eventi. Ma come creare un gestore eventi per il `Error` evento?

Il `Error` è uno di molti eventi nel [ `HttpApplication` classe](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) che vengono generati in momenti diversi nella pipeline HTTP per la durata di una richiesta. Ad esempio, il `HttpApplication` della classe [ `BeginRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) viene generato all'inizio di ogni richiesta; relativo [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) viene generato quando un modulo di protezione ha identificato il richiedente. Questi `HttpApplication` eventi forniscono allo sviluppatore della pagina un mezzo per eseguire la logica personalizzata in vari punti della durata di una richiesta.

Gestori eventi per il `HttpApplication` eventi possono essere inseriti in un file speciale denominato `Global.asax`. Per creare questo file nel sito Web, aggiungere un nuovo elemento alla radice del sito Web utilizzando il modello di classe di applicazione globale con il nome `Global.asax`.

[![](processing-unhandled-exceptions-cs/_static/image2.png)](processing-unhandled-exceptions-cs/_static/image1.png)

**Figura 1**: aggiungere `Global.asax` all'applicazione Web  
([Fare clic per visualizzare l'immagine ingrandita](processing-unhandled-exceptions-cs/_static/image3.png))

Il contenuto e la struttura del `Global.asax` file creata da Visual Studio variano leggermente in base sull'utilizzo di un progetto di applicazione Web (WAP) o un progetto di sito Web (WSP). Con un WAP, il `Global.asax` viene implementata come due file diversi - `Global.asax` e `Global.asax.cs`. Il `Global.asax` file non contiene alcun elemento, ma un `@Application` direttiva che fa riferimento il `.cs` file; l'evento di gestori di interesse vengono definiti nel `Global.asax.cs` file. Per WSPs, viene creato un singolo file, `Global.asax`, e i gestori di eventi sono definiti in un `<script runat="server">` blocco.

Il `Global.asax` file creati in un WAP dal modello di classe di applicazione globale di Visual Studio include i gestori eventi denominati `Application_BeginRequest`, `Application_AuthenticateRequest`, e `Application_Error`, quali sono i gestori eventi per il `HttpApplication` eventi `BeginRequest`, `AuthenticateRequest`, e `Error`, rispettivamente. Sono disponibili anche i gestori eventi denominati `Application_Start`, `Session_Start`, `Application_End`, e `Session_End`, quali sono i gestori di eventi che attivano all'avvio dell'applicazione web, quando viene avviata una nuova sessione, al termine dell'applicazione e quando termina una sessione, rispettivamente. Il `Global.asax` file creati per Visual Studio in un pacchetto WSP contiene solo il `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, e `Session_End` gestori eventi.

> [!NOTE]
> Quando si distribuisce l'applicazione ASP.NET è necessario copiare il `Global.asax` file nell'ambiente di produzione. Il `Global.asax.cs` file, viene creato nel WAP, non deve essere copiato nell'ambiente di produzione, perché questo codice viene compilato in assembly del progetto.


I gestori eventi creati dal modello di classe di applicazione globale di Visual Studio non sono esaustivi. È possibile aggiungere un gestore eventi per qualsiasi `HttpApplication` evento assegnando il gestore dell'evento `Application_EventName`. Ad esempio, è possibile aggiungere il codice seguente per il `Global.asax` file per creare un gestore eventi per il [ `AuthorizeRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-cs[Main](processing-unhandled-exceptions-cs/samples/sample1.cs)]

Analogamente, è possibile rimuovere i gestori eventi creati dal modello di classe di applicazione globale che non sono necessari. Per questa esercitazione è necessario solo un gestore eventi per il `Error` eventi; contattarmi per altri gestori di eventi da rimuovere il `Global.asax` file.

> [!NOTE]
> *I moduli HTTP* offrono un altro modo per definire i gestori eventi per `HttpApplication` eventi. I moduli HTTP vengono creati come file di classe che può essere direttamente nel progetto di applicazione web o suddivisi in una libreria di classi separato. Poiché possono essere suddivisi in una libreria di classi, moduli HTTP offrono un modello più flessibile e riutilizzabile per la creazione di `HttpApplication` gestori eventi. Mentre il `Global.asax` è specifico per l'applicazione web in cui si trova, moduli HTTP possono essere compilati in assembly, a quel punto aggiungere il modulo HTTP a un sito Web è semplice come l'eliminazione di assembly il `Bin` cartella e la registrazione il Modulo `Web.config`. In questa esercitazione non corrisponde alla creazione e uso di moduli HTTP, ma le librerie di registrazione degli errori due utilizzate nelle due esercitazioni seguenti vengono implementate come moduli HTTP. Per ulteriori informazioni sui vantaggi di moduli HTTP fare riferimento a [tramite moduli e i gestori per creare componenti ASP.NET collegabili](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Il recupero delle informazioni sull'eccezione non gestita

A questo punto si dispone di un file Global. asax con un `Application_Error` gestore dell'evento. Quando si esegue questo gestore eventi è necessario inviare una notifica di uno sviluppatore dell'errore e registrare i dettagli. Per eseguire queste operazioni è necessario innanzitutto determinare i dettagli dell'eccezione che è stato generato. Utilizzare l'oggetto Server [ `GetLastError` metodo](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) per recuperare i dettagli dell'eccezione non gestita che ha causato il `Error` dell'evento da generare.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample2.cs)]

Il `GetLastError` il metodo restituisce un oggetto di tipo `Exception`, ovvero il tipo di base per tutte le eccezioni in .NET Framework. Tuttavia, nel codice precedente è cast dell'oggetto di eccezione restituito da `GetLastError` in un `HttpException` oggetto. Se il `Error` evento viene generato perché è stata generata un'eccezione durante l'elaborazione di una risorsa ASP.NET, quindi viene eseguito il wrapping di un'eccezione che è stata generata un'eccezione all'interno di un `HttpException`. Per ottenere l'eccezione che ha generato l'utilizzo di eventi di errore di `InnerException` proprietà. Se il `Error` è stato generato l'evento a causa di un'eccezione basata su HTTP, ad esempio una richiesta per una pagina non esistente, un `HttpException` viene generata un'eccezione, ma non dispone di un'eccezione interna.

Il codice seguente usa il `GetLastErrormessage` per recuperare le informazioni sull'eccezione che ha attivato il `Error` evento, l'archiviazione di `HttpException` in una variabile denominata `lastErrorWrapper`. Archivia quindi il tipo di un messaggio e traccia dello stack dell'eccezione origine nelle tre variabili di stringa, un controllo per verificare se il `lastErrorWrapper` è l'eccezione che ha attivato il `Error` eventi (nel caso di eccezioni basato su HTTP) o se è semplicemente un wrapper per un'eccezione generata durante l'elaborazione della richiesta.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample3.cs)]

A questo punto si dispone di tutte le informazioni necessarie per scrivere codice che verrà registrati i dettagli dell'eccezione in una tabella di database. È possibile creare una tabella di database con colonne per ogni i dettagli dell'errore di interesse - il tipo, il messaggio, la traccia dello stack e così via - assieme ad altre utile di informazioni, ad esempio l'URL della pagina richiesta e il nome dell'utente attualmente connesso. Nel `Application_Error` gestore dell'evento verrà quindi connettersi al database e inserire un record nella tabella. Analogamente, è possibile aggiungere codice per uno sviluppatore dell'errore tramite posta elettronica di avviso.

Le librerie di registrazione errore esaminate nelle prossime due esercitazioni forniscono tali funzionalità predefinita, in modo non è necessario per compilare la registrazione degli errori e notifica. Tuttavia, per illustrare che il `Error` evento viene generato e che il `Application_Error` gestore eventi può essere utilizzato per registrare i dettagli dell'errore e inviare una notifica a uno sviluppatore, aggiungere codice che notifica a uno sviluppatore quando si verifica un errore.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Notifica a uno sviluppatore quando si verifica un'eccezione non gestita

Quando si verifica un'eccezione non gestita nell'ambiente di produzione è importante avvertire il team di sviluppo in modo che possano valutare l'errore e determinare cosa è necessario eseguire alcune azioni. Ad esempio, se è presente un errore durante la connessione al database, è necessario in un tipo double controllare la stringa di connessione e, forse, aprire un ticket di supporto con la società di hosting web. Se si è verificata l'eccezione a causa di un errore di programmazione, potrebbe essere necessario codice aggiuntivo o la logica di convalida da aggiungere per evitare tali errori in futuro.

Le classi .NET Framework nel [ `System.Net.Mail` dello spazio dei nomi](https://msdn.microsoft.com/library/system.net.mail.aspx) semplificano inviare un messaggio di posta elettronica. Il [ `MailMessage` classe](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) rappresenta un messaggio di posta elettronica e dispone di proprietà quali `To`, `From`, `Subject`, `Body`, e `Attachments`. Il `SmtpClass` viene utilizzato per inviare un `MailMessage` oggetto utilizza un server SMTP specificato; le impostazioni del server SMTP possono essere specificate a livello di programmazione o in modo dichiarativo nel [ `<system.net>` elemento](https://msdn.microsoft.com/library/6484zdc1.aspx) nel `Web.config file`. Per ulteriori informazioni sull'invio di posta elettronica i messaggi in un'applicazione ASP.NET vedere l'articolo, [l'invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)e [domande frequenti su System.Net.Mail](http://systemnetmail.com/).

> [!NOTE]
> Il `<system.net>` elemento contiene le impostazioni del server SMTP utilizzate dalla `SmtpClient` classe quando si invia un messaggio di posta elettronica. Probabilmente società di hosting web dispone di un server SMTP che è possibile utilizzare per inviare posta elettronica dall'applicazione. Per informazioni sulle impostazioni del server SMTP, che è consigliabile utilizzare nell'applicazione web, consultare la sezione di supporto dell'host web.


Aggiungere il codice seguente per il `Application_Error` gestore eventi per l'invio di uno sviluppatore di un messaggio di posta elettronica quando si verifica un errore:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample4.cs)]

Mentre il codice sopra riportato è piuttosto lungo, la maggior parte di esso crea il codice HTML visualizzato il messaggio di posta elettronica per gli sviluppatori. Il codice di avvio facendo riferimento al `HttpException` restituito dal `GetLastError` metodo (`lastErrorWrapper`). L'eccezione che è stato generato dalla richiesta è stato recuperato tramite `lastErrorWrapper.InnerException` e viene assegnato alla variabile `lastError`. Il tipo di un messaggio e stack di informazioni di traccia vengono recuperate dal `lastError` e archiviati in tre variabili stringa.

Successivamente, un `MailMessage` oggetto denominato `mm` viene creato. Il corpo del messaggio di posta elettronica è in formato HTML e visualizza l'URL della pagina richiesta, il nome dell'utente attualmente connesso e informazioni sull'eccezione (il tipo di messaggio e traccia dello stack). Una delle caratteristiche più interessanti di `HttpException` classe è che è possibile generare il codice HTML utilizzato per creare l'eccezione dettagli giallo schermata di morte (YSOD) chiamando il [GetHtmlErrorMessage metodo](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Questo metodo è usato per recuperare il codice di eccezione dettagli YSOD e aggiungerlo al messaggio di posta elettronica come allegato. Attenzione: se l'eccezione che ha attivato il `Error` evento è verificata un'eccezione basata su HTTP (ad esempio una richiesta per una pagina inesistente) il `GetHtmlErrorMessage` metodo restituirà `null`.

Il passaggio finale consiste nella trasmissione di `MailMessage`. Questa operazione viene eseguita creando un nuovo `SmtpClient` metodo e chiamando il relativo `Send` metodo.

> [!NOTE]
> Prima di utilizzare questo codice nell'applicazione web è opportuno modificare i valori di `ToAddress` e `FromAddress` costanti support@example.com per posta elettronica indirizzo di posta elettronica di notifica di errore devono essere inviati a e provengono da. È inoltre necessario specificare le impostazioni del server SMTP nel `<system.net>` sezione `Web.config`. Consultare il provider di hosting web per determinare le impostazioni del server SMTP da utilizzare.


Con questo codice nella posizione ogni volta che si verifica un errore lo sviluppatore viene inviato un messaggio di posta elettronica che riepiloga l'errore e include il YSOD. Nell'esercitazione precedente è illustrato un errore di runtime visita Genre.aspx e passando un oggetto non valido `ID` valore tramite la stringa di query, ad esempio `Genre.aspx?ID=foo`. Visitare la pagina con il `Global.asax` file sul posto genera la stessa esperienza utente, come nell'esercitazione precedente - nell'ambiente di sviluppo si procederà vedere l'eccezione dettagli giallo schermata di morte, mentre nell'ambiente di produzione, sarà necessario vedere la pagina di errore personalizzato. Oltre a questo comportamento esistente, lo sviluppatore viene inviato un messaggio di posta elettronica.

**Figura 2** Mostra messaggio di posta elettronica ricevuto durante la visita `Genre.aspx?ID=foo`. Il corpo del messaggio di posta elettronica vengono riepilogate le informazioni sull'eccezione, mentre il `YSOD.htm` allegato viene visualizzato il contenuto visualizzato nel YSOD di dettagli di eccezione (vedere **figura 3**).

[![](processing-unhandled-exceptions-cs/_static/image5.png)](processing-unhandled-exceptions-cs/_static/image4.png)

**Figura 2**: lo sviluppatore viene inviato una notifica di posta elettronica ogni volta che si verifica un'eccezione non gestita  
([Fare clic per visualizzare l'immagine ingrandita](processing-unhandled-exceptions-cs/_static/image6.png))

[![](processing-unhandled-exceptions-cs/_static/image8.png)](processing-unhandled-exceptions-cs/_static/image7.png)

**Figura 3**: la notifica di posta elettronica include i dettagli dell'eccezione YSOD come allegato  
([Fare clic per visualizzare l'immagine ingrandita](processing-unhandled-exceptions-cs/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Cosa succede utilizzando la pagina di errore personalizzati?

In questa esercitazione viene illustrato come utilizzare `Global.asax` e `Application_Error` gestore dell'evento per eseguire codice quando si verifica un'eccezione non gestita. In particolare, è utilizzato il gestore eventi per notificare a uno sviluppatore di un errore. è possibile estendere in modo da registrare anche i dettagli dell'errore in un database. La presenza del `Application_Error` gestore dell'evento non influisce sulla esperienza dell'utente finale. Sono ancora vedere la pagina di errore configurata, sia essa YSOD di dettagli di errore, il YSOD di errore di Runtime o la pagina di errore personalizzato.

È naturale chiedersi se la `Global.asax` file e `Application_Error` eventi sono necessario quando si utilizza una pagina di errore personalizzato. Quando si verifica un errore all'utente viene visualizzata la pagina di errore personalizzati così perché non è stato inserito il codice per notificare lo sviluppatore e i dettagli dell'errore di log nella classe code-behind della pagina di errore personalizzati? Mentre è certamente possibile aggiungere codice alla classe code-behind della pagina di errore personalizzato non si dispone dell'accesso per i dettagli dell'eccezione che ha attivato il `Error` evento quando si usa la tecnica, è esplorare nell'esercitazione precedente. La chiamata di `GetLastError` metodo dalla pagina di errore personalizzato restituisce `Nothing`.

Il motivo per questo comportamento è raggiunto la pagina di errore personalizzato tramite un reindirizzamento. Quando un'eccezione non gestita raggiunge il runtime di ASP.NET il motore ASP.NET genera relativo `Error` eventi (che esegue il `Application_Error` gestore eventi) e quindi *reindirizza* l'utente alla pagina di errore personalizzato eseguendo un `Response.Redirect(customErrorPageUrl)`. Il `Response.Redirect` metodo invia una risposta al client con un codice di stato HTTP 302 che indicano il browser per richiedere un nuovo URL, vale a dire la pagina di errore personalizzato. Il browser richiede quindi automaticamente la nuova pagina. È possibile indicare che la pagina di errore personalizzato è stato richiesto separatamente dalla pagina in cui l'errore ha origine dal momento che cambia la barra degli indirizzi del browser all'URL di pagina di errore personalizzato (vedere **figura 4**).

[![](processing-unhandled-exceptions-cs/_static/image11.png)](processing-unhandled-exceptions-cs/_static/image10.png)

**Figura 4**: quando si verifica un errore il Browser viene reindirizzato all'URL di pagina di errore personalizzati  
([Fare clic per visualizzare l'immagine ingrandita](processing-unhandled-exceptions-cs/_static/image12.png))

L'effetto finale è che la richiesta in cui si è verificata l'eccezione non gestita termina quando il server risponde con il reindirizzamento HTTP 302. La richiesta successiva alla pagina di errore personalizzata è una nuova richiesta. a questo punto ASP.NET motore ha eliminato le informazioni sull'errore e, inoltre, non è possibile associare l'eccezione non gestita nella richiesta precedente con la nuova richiesta per la pagina di errore personalizzato. Questo è il motivo `GetLastError` restituisce `null` quando viene chiamato dalla pagina di errore personalizzato.

Tuttavia, è possibile avere una pagina di errore personalizzata eseguita durante la stessa richiesta che ha causato l'errore. Il [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) metodo trasferisce l'esecuzione all'URL specificato e lo elabora all'interno della stessa richiesta. È possibile spostare il codice di `Application_Error` gestore eventi per classe code-behind della pagina di errore personalizzata, sostituirla in `Global.asax` con il codice seguente:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample5.cs)]

A questo punto quando si verifica un'eccezione non gestita di `Application_Error` gestore trasferisce il controllo alla pagina di errore personalizzato appropriato in base al codice di stato HTTP. Poiché il controllo è stato trasferito, pagina di errore personalizzata può accedere alle informazioni sull'eccezione non gestita tramite `Server.GetLastError` e inviare una notifica a uno sviluppatore dell'errore e registrare i dettagli. Il `Server.Transfer` chiamata Arresta il motore ASP.NET da reindirizzamento dell'utente alla pagina di errore personalizzata. Contenuto della pagina di errore personalizzato, invece, viene restituito come risposta alla pagina che ha generato l'errore.

## <a name="summary"></a>Riepilogo

Quando si verifica un'eccezione non gestita in un'applicazione web ASP.NET il runtime ASP.NET genera il `Error` evento e viene visualizzata la pagina di errore configurate. È possibile ricevere lo sviluppatore dell'errore, effettuare i dettagli o elaborarlo in qualche modo, tramite la creazione di un gestore eventi per l'evento di errore. Esistono due modi per creare un gestore eventi per `HttpApplication` eventi come `Error`: nel `Global.asax` file o da un modulo HTTP. In questa esercitazione viene illustrato come creare un `Error` nel gestore dell'evento di `Global.asax` file che gli sviluppatori di un errore di notifica tramite un messaggio di posta elettronica.

Creazione di un `Error` gestore eventi è utile se è necessario elaborare le eccezioni non gestite in modo esclusivo o personalizzato. Tuttavia, la creazione di propri `Error` gestore eventi per registrare l'eccezione o per notificare a uno sviluppatore non è l'utilizzo più efficiente del tempo perché esistono già le librerie di registrazione errore gratuito e facile da utilizzare che sono possibile configurare in pochi minuti. Le due esercitazioni esaminare due tali librerie.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Moduli ASP.NET e Cenni preliminari sui gestori HTTP](https://support.microsoft.com/kb/307985)
- [Risponde correttamente alle eccezioni non gestite - l'elaborazione delle eccezioni non gestite](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` Classe e l'oggetto applicazione ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [I gestori HTTP e moduli HTTP in ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [L'invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [La comprensione di `Global.asax` File](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Utilizzo di gestori e i moduli HTTP per creare componenti ASP.NET collegabili](https://msdn.microsoft.com/library/aa479332.aspx)
- [Utilizzo di ASP.NET `Global.asax` File](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Utilizzo di `HttpApplication` istanze](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Precedente](displaying-a-custom-error-page-cs.md)
> [Successivo](logging-error-details-with-asp-net-health-monitoring-cs.md)
