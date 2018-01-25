---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages Guida alla risoluzione dei problemi (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo articolo vengono descritti i problemi che possono verificarsi quando si utilizzano pagine Web ASP.NET (Razor) e alcune soluzioni suggerite. Versioni del software pagina Web ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: 72c49ed8b76b5fb5eb15bf01f57980b382607496
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>Pagine Web ASP.NET (Razor) Troubleshooting Guide
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo vengono descritti i problemi che possono verificarsi quando si utilizzano pagine Web ASP.NET (Razor) e alcune soluzioni suggerite.
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2 e pagine Web ASP.NET 1.0.


Di seguito sono elencate le diverse sezioni di questo argomento:

- [Problemi con l'esecuzione delle pagine](#Issues_Running_.cshtml_Pages)
- [Problemi di codice Razor](#IssuesWithRazorCode)
- [Problemi relativi alla sicurezza e l'appartenenza](#membership)
- [Problemi con l'invio di posta elettronica](#email)
- [Risorse aggiuntive](#AdditionalResources)

Per domande di carattere generale, vedere [pagine Web ASP.NET (Razor) domande frequenti su](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problemi con l'esecuzione delle pagine

Un'ampia gamma di problemi può impedire *. cshtml* e *. vbhtml* pagine da eseguito correttamente. In questa sezione elenca i messaggi di errore comuni e cause probabili.

### <a name="http-error-403---forbidden-access-is-denied"></a>Errore HTTP 403 - accesso negato: Accesso negato

*Non si dispone dell'autorizzazione per visualizzare la directory o la pagina utilizzando le credenziali fornite.*

Questo errore può verificarsi se il server non è in esecuzione la versione corretta di .NET Framework. Assicurarsi che il computer che esegue il server (locale o remota) disponga almeno .NET Framework 4. Assicurarsi inoltre che l'applicazione è configurato per eseguire la versione corretta.

Se si riscontra questo problema in locale mentre si lavora in WebMatrix, fare clic su di **sito** area di lavoro e nella visualizzazione albero fare clic su **impostazioni**. Nel **selezionare .NET Framework versione** selezionare **.NET 4 (modalità integrata)**. Se questa versione è già impostata, provare a eseguire WebMatrix come amministratore.

Assicurarsi che la radice del sito Web disponga di almeno uno *. cshtml* file in essa contenuti.

Se viene visualizzato questo errore quando il server web in un server remoto, contattare l'amministratore del server. Assicurarsi che il server dispone di .NET Framework 4 o successiva. Assicurarsi inoltre che l'applicazione è in esecuzione in un pool di applicazioni configurato per utilizzare tale versione di.NET Framework.

Se si dispone di controllo del server, assicurarsi che la versione corretta di .NET Framework è in esecuzione. È anche possibile provare a ripristinare l'installazione eseguendo il `aspnet_regiis -iru` comando. (Ad esempio, se si installa IIS dopo l'installazione di .NET Framework, IIS verrà non essere configurato correttamente per eseguire le pagine ASP.NET.) Per ulteriori informazioni, vedere [strumento di registrazione IIS ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Errore HTTP 403.14 - accesso negato

*Il server Web è configurato per non visualizzare il contenuto di questa directory.*

Questo errore può verificarsi se la richiesta di una risorsa protetta (ad esempio il *Web. config* file) o che si trova in una cartella protetta (ad esempio *App\_dati* o *App\_Codice*).

### <a name="http-error-40417---not-found"></a>Errore HTTP 404.17 - non trovato

*Il contenuto richiesto sembra essere uno script e non verrà fornito dal gestore di file statici.*

Questo errore può verificarsi se il server non è configurato correttamente per l'utilizzo di .NET Framework 4 o versione successiva e pertanto non riconosce il codice in `@{ }` blocchi. Vedere la descrizione precedente *HTTP Errore 403 - accesso negato: accesso negato*.

### <a name="http-error-4047---not-found"></a>Errore HTTP 404.7 - non trovato

*Il modulo filtro delle richieste è configurato per negare l'estensione di file*

Questo errore può verificarsi se *. cshtml* o *. vbhtml* estensioni sono state bloccate in modo esplicito nel server. Un sintomo del problema è che gli URL lavoro quando non includono l'estensione, ma gli URL che includono *. cshtml* o *. vbhtml* non funzionano. Una soluzione possibile consiste nell'abilitare nuovamente le estensioni del sito *Web. config* file. Nell'esempio seguente viene illustrato come abilitare il *. cshtml* estensione.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Errore HTTP 404.8 - non trovato

*Il modulo filtro delle richieste è configurato per negare un percorso nell'URL che contiene una sezione hiddenSegment.*

Questo errore può verificarsi se la richiesta di una risorsa protetta (ad esempio il *Web. config* file) o che si trova in una cartella protetta (ad esempio *App\_dati* o *App\_Codice*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Questo tipo di pagina non è disponibile (errore Server nell'applicazione '/')

Vedere la descrizione in precedenza per HTTP 404.17 di errore.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problemi di codice Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Il nome '*classe*' non esiste nel contesto corrente

Un motivo viene visualizzato questo errore è spesso che `class` riferimenti a un file di supporto, ma l'helper non è installato. Ad esempio, se si tenta di utilizzare un file di supporto, ma non è stato installato il pacchetto da NuGet, verrà visualizzato questo errore. Utilizzare la raccolta in WebMatrix per trovare e installare il supporto.

Se è installato il supporto, ma la pagina ancora non lo riconosce, provare ad aggiungere aggiungere un `using` istruzione al codice. Nel `using` istruzione, lo spazio dei nomi che include il supporto di riferimento. Ad esempio, gli helper di base sono incluse nel package di ASP.NET Web Helpers presenti il `System.Web.Helpers` dello spazio dei nomi. Nella parte superiore della pagina in cui si desidera utilizzare l'helper, aggiungere questa riga:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problemi relativi alla sicurezza e l'appartenenza

Se si utilizza il sistema di sicurezza incorporati (membership) nelle pagine Web ASP.NET (Razor), possono verificarsi i problemi seguenti.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Per chiamare questo metodo, la proprietà "Membership.Provider" deve essere un'istanza di "ExtendedMembershipProvider"

Questo errore può indicare che non `AspNetSqlMembershipProvider` classe è configurata. (Un sintomo è che il sito funziona correttamente in locale, ma genera questo errore quando si pubblica il server del provider di hosting). Una correzione per questo problema consiste nell'abilitare in modo esplicito l'appartenenza semplice aggiungendo quanto segue per il sito *Web. config* file:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problemi con l'invio di posta elettronica

Problemi relativi all'invio di posta elettronica possono essere difficile eseguire il debug. Può essere un problema iniziale che non è possibile connettersi al server SMTP. Se la connessione ha esito positivo, ASP.NET invia il messaggio al server SMTP. Tuttavia, si possono verificare problemi con il messaggio stesso che impedisce l'invio al server SMTP.

Se l'applicazione non inviare posta elettronica, procedere come segue:

- Il nome del server SMTP è spesso simile `smtp.provider.com` o `smtp.provider.net`. Tuttavia, se si pubblica il sito per un provider di hosting, il nome del server SMTP a questo punto potrebbe essere `localhost`. Questa situazione si verifica perché dopo avere pubblicato e il sito è in esecuzione nel server del provider, il server SMTP potrebbe essere locale dal punto di vista dell'applicazione. Questa modifica nei nomi di server potrebbe indicare che è necessario modificare il nome del server SMTP come parte del processo di pubblicazione.
- In genere, il numero di porta è 25. Tuttavia, alcuni provider richiedono l'utilizzo della porta 587 o alcune altre porte. Contattare il proprietario del server SMTP del numero di porta prevedono di utilizzare.
- Assicurarsi di utilizzare le credenziali corrette. Se il sito è stato pubblicato in un provider di hosting, utilizzare le credenziali che il provider ha specificamente indicato per la posta elettronica. Queste credenziali possono essere diverse dalle credenziali che utilizzare per la pubblicazione.
- In alcuni casi non è necessario credenziali affatto. Se si invia tramite posta elettronica utilizzando l'account personale, provider di posta elettronica potrebbe essere già conoscere le credenziali. Dopo la pubblicazione, potrebbe essere necessario utilizzare credenziali diverse rispetto a quando si testa il computer locale.
- Se il provider di posta elettronica utilizza la crittografia, impostare `WebMail.EnableSsl` a `true`.

Se si verifica un errore di invio di posta elettronica, è visualizzato un messaggio di errore ASP.NET standard, che è simile al seguente:

![Messaggio di errore ASP.NET quando si è verificato un problema con messaggio di posta elettronica](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

È anche possibile eseguire il debug problemi relativi all'invio di posta elettronica utilizzando un `try-catch` blocco, come nell'esempio seguente. Quando si utilizza un `try-catch` blocco, ASP.NET non vengono visualizzati i messaggi di errore standard. In alternativa, è possibile acquisire l'errore nel `catch` parte del blocco.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Sostituire i valori appropriati per `your-SMTP-server-name`e così via. Alcuni dei messaggi di errore che potrebbe essere visualizzato in questo modo i seguenti:

- *Errore durante l'invio di posta elettronica.*

    oppure

    *Un tentativo di connessione non riuscita perché l'entità connesso non ha risposto correttamente dopo un periodo di tempo o stabilire una connessione non è riuscita perché l'host connesso non ha risposto*

    Questo errore indica in genere che l'applicazione non riuscito a connettersi al server SMTP. Controllare il nome del server e il numero di porta.
- *Cassetta postale non disponibile. Risposta del server: 5.1.0 &lt; someuser@invaliddomain &gt; mittente rifiutato: il dominio del mittente non valido*

    Questo messaggio può indicare che il `From` indirizzo non è corretto o mancante.
- *La stringa specificata non è nel formato richiesto per un indirizzo di posta elettronica.*

    Questo errore potrebbe indicare che il valore di `To` o `From` proprietà non sono riconosciute come indirizzi di posta elettronica. (ASP.NET non è possibile verificare che l'indirizzo di posta elettronica sia valido, ma solo la 's nel formato corretto, ad esempio  *name@domain.com* .)

> [!NOTE]
> Rimuovere il markup che viene visualizzato l'errore (`@errorMessage`) prima di pubblicare la pagina in un sito in tempo reale. Non è consigliabile per consentire agli utenti di vedere i messaggi di errore che si ottiene da un server.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Domande frequenti sulle pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix e ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum sul sito Web ASP.NET
