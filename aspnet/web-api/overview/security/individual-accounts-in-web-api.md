---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Proteggere un'API Web con singoli account e un account di accesso locale in API Web ASP.NET 2.2 | Microsoft Docs
author: MikeWasson
description: Questo argomento illustra come proteggere un'API web usando OAuth2 per l'autenticazione con un database di appartenenza. Versioni del software utilizzate nell'esercitazione 201 Studio Visual...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: f37cb206b78fd46835bb59bede5920f8e5acd3fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399938"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Proteggere un'API Web con singoli account e un account di accesso locale in API Web ASP.NET 2.2
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare l'App di esempio](https://github.com/MikeWasson/LocalAccountsApp)

> Questo argomento illustra come proteggere un'API web usando OAuth2 per l'autenticazione con un database di appartenenza.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [API Web 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [Identità ASP.NET 2.1](../../../identity/index.md)


In Visual Studio 2013, il modello di progetto API Web offre tre opzioni per l'autenticazione:

- **Account individuali.** L'app Usa un database di appartenenza.
- **Account aziendali.** Gli utenti accedono con le Azure Active Directory, Office 365 o le credenziali di Active Directory locale.
- **Autenticazione di Windows.** Questa opzione è per le applicazioni Intranet e Usa il modulo IIS di autenticazione di Windows.

Per altre informazioni su queste opzioni, vedere [creazione di progetti Web ASP.NET in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Singoli account forniscono un utente di accedere due modi:

- **Account di accesso locale**. L'utente registra nel sito, immettere un nome utente e password. L'app archivia l'hash della password nel database delle appartenenze. Quando l'utente accede, il sistema di identità di ASP.NET viene verificata la password.
- **Account di accesso social**. L'utente accede con un servizio esterno, ad esempio Facebook, Microsoft o Google. L'app ancora crea una voce per l'utente nel database di appartenenza, ma non archivia le credenziali. L'utente viene autenticato effettuando l'accesso al servizio esterno.

Questo articolo viene descritto lo scenario di account di accesso locale. Account di accesso locali e basati su social network, API Web Usa OAuth2 per autenticare le richieste. Tuttavia, i flussi di credenziali sono diversi per account di accesso locali e basati su social network.

In questo articolo, illustrerò una semplice app che consente all'utente di accedere e inviare chiamate AJAX autenticate a un'API web. È possibile scaricare il codice di esempio [qui](https://github.com/MikeWasson/LocalAccountsApp). Il file Leggimi viene descritto come creare il codice di esempio da zero in Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

L'app di esempio Usa Knockout. js per l'associazione dati e jQuery per l'invio di richieste AJAX. Mi concentrerò sulle chiamate AJAX, in modo non occorre conoscere Knockout. js per questo articolo.

Corso della trattazione, descriverò:

- Ciò che l'app esegue sul lato client.
- Ciò che accade sul server.
- Il traffico HTTP al centro.

In primo luogo, è necessario definire la terminologia OAuth2.

- *Risorsa*. Una parte dei dati che possono essere protetti.
- *Server di risorse*. Il server che ospita la risorsa.
- *Proprietario della risorsa*. L'entità che può concedere l'autorizzazione per accedere a una risorsa. (In genere l'utente.)
- *Client*: app che richiede l'accesso alla risorsa. In questo articolo, il client è un web browser.
- *Token di accesso*. Token che concede l'accesso a una risorsa.
- *Token di connessione*. Un particolare tipo di token di accesso, con la proprietà che chiunque può usare il token. In altre parole, un client non è necessario una chiave di crittografia o altri segreto per usare un token di connessione. Per questo motivo, i token di connessione devono essere usati solo su un HTTPS e devono avere tempi di scadenza relativamente breve.
- *Server di autorizzazione*. Un server che fornisce token di accesso.

Un'applicazione può fungere da server di autorizzazione e server di risorse. Il modello di progetto API Web segue questo modello.

## <a name="local-login-credential-flow"></a>Flusso di credenziali di account di accesso locale

Per account di accesso locale, API Web Usa il [flusso di password proprietario risorsa](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) definito in OAuth2.

1. L'utente immette un nome e la password del client.
2. Il client invia le credenziali al server di autorizzazione.
3. Il server di autorizzazione autentica le credenziali e restituisce un token di accesso.
4. Per accedere a una risorsa protetta, il client include il token di accesso nell'intestazione dell'autorizzazione della richiesta HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Quando si seleziona **singoli account** nel modello di progetto API Web, il progetto include un server di autorizzazione che convalida le credenziali utente e rilascia i token. Il diagramma seguente mostra lo stesso flusso di credenziali in termini di componenti di API Web.

![](individual-accounts-in-web-api/_static/image4.png)

In questo scenario, i controller API Web fungono da server di risorse. Un filtro di autenticazione convalida i token di accesso e il **[Authorize]** attributo viene usato per proteggere una risorsa. Quando un controller o azione ha il **[Authorize]** dell'attributo, tutte le richieste a tale controller o azione deve essere autenticata. In caso contrario, l'autorizzazione viene negata e API Web restituisce un errore 401 (non autorizzato).

Il server di autorizzazione e il filtro di autenticazione sia chiamare in un' [middleware OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) componente che gestisce i dettagli di OAuth2. Descriverò la progettazione in modo più dettagliato più avanti in questa esercitazione.

## <a name="sending-an-unauthorized-request"></a>L'invio di una richiesta non autorizzata

Per iniziare, eseguire l'app, quindi scegliere il **Call API** pulsante. Al completamento della richiesta, si dovrebbe vedere un messaggio di errore nel **risultato** casella. Ciò avviene perché la richiesta non contiene un token di accesso, in modo che la richiesta non è autorizzata.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

Il **Call API** pulsante Invia una richiesta AJAX ~/api/valori, che richiama un'azione del controller API Web. Di seguito è riportata la sezione di codice JavaScript che invia la richiesta AJAX. Nell'app di esempio, tutto il codice di app JavaScript si trova nel file Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Fino a quando l'utente accede, non vi è alcun token di connessione e quindi nessuna intestazione di autorizzazione nella richiesta. In questo modo la richiesta restituire l'errore 401.

Ecco la richiesta HTTP. (È usata [Fiddler](http://www.telerik.com/fiddler) per acquisire il traffico HTTP.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Si noti che la risposta include un'intestazione Www-Authenticate con la richiesta di verifica impostato su Bearer. Valore che indica che il server si aspetta un token di connessione.

## <a name="register-a-user"></a>Registrazione di un utente

Nel **registrare** sezione dell'app, immettere un indirizzo di posta elettronica e una password, quindi scegliere il **registrare** pulsante.

Non è necessario usare un indirizzo di posta elettronica valido per questo esempio, ma una vera app potrebbe verificare l'indirizzo. (Vedere [creare un'app web ASP.NET MVC 5 sicura con accesso, messaggio di posta elettronica conferma e reimpostazione della password](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Per la password, usare qualcosa come "Password1!", con una lettera maiuscola, lettera minuscola, numero e il carattere non alfanumerico. Per mantenere semplice l'app, avevo lasciato la convalida lato client, in modo che se si è verificato un problema con il formato della password, si otterrà un errore 400 (richiesta non valida).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

Il **registrare** pulsante Invia una richiesta POST a ~/api/Account/Register /. Il corpo della richiesta è un oggetto JSON che contiene il nome e la password. Ecco il codice JavaScript che invia la richiesta:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Richiesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Questa richiesta è gestita dal `AccountController` classe. Internamente, `AccountController` Usa ASP.NET Identity per gestire il database di appartenenza.

Se si esegue localmente l'app da Visual Studio, gli account utente vengono archiviati nel database locale nella tabella AspNetUsers. Per visualizzare le tabelle in Visual Studio, scegliere il **visualizzazione** dal menu **Esplora Server**, quindi espandere **connessioni dati**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Ottenere un accesso Token

Fino ad ora non è stato eseguito alcun OAuth, ma ora si vedrà il server di autorizzazione OAuth in azione, quando si richiede un token di accesso. Nel **Log In** area dell'app di esempio, immettere l'indirizzo di posta elettronica e la password e fare clic su **Accedi**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

Il **Log In** pulsante Invia una richiesta all'endpoint del token. Il corpo della richiesta contiene i dati codificati negli url di form seguenti:

- concedere\_tipo: "password"
- nome utente: &lt;messaggio di posta elettronica dell'utente&gt;
- Password: &lt;password&gt;

Ecco il codice JavaScript che invia la richiesta AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Se la richiesta ha esito positivo, il server di autorizzazione restituisce un token di accesso nel corpo della risposta. Si noti che il token archiviati in archiviazione di sessione, usare in un secondo momento quando si inviano richieste all'API. A differenza di alcune forme di autenticazione (ad esempio l'autenticazione basata su cookie), il browser non includerà automaticamente il token di accesso nelle richieste successive. L'applicazione deve farlo in modo esplicito. Che è positivo, in quanto limita [vulnerabilità CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Richiesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

È possibile notare che la richiesta contiene le credenziali dell'utente. Si *necessario* usare HTTPS per fornire protezione a livello di trasporto.

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Per migliorare la leggibilità, applicato un rientro di JSON e troncato il token di accesso, che è piuttosto lungo.

Il `access_token`, `token_type`, e `expires_in` proprietà sono definite dalla specifica OAuth2. Le altre proprietà (`userName`, `.issued`, e `.expires`) sono solo a scopo informativo. È possibile trovare il codice che aggiunge le proprietà aggiuntive nel `TokenEndpoint` metodo, nel file /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Inviare una richiesta autenticata

Ora che abbiamo un token di connessione, possiamo apportare una richiesta autenticata all'API. Questa operazione viene eseguita impostando l'intestazione di autorizzazione nella richiesta. Scegliere il **Call API** pulsante nuovo per visualizzare il risultato.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Richiesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Effettuare la disconnessione

Poiché il browser non memorizza nella cache le credenziali o il token di accesso, la disconnessione è semplicemente una questione di "Se si dimentica" il token, rimuovendolo dall'archivio di sessione:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Comprendere il modello di progetto di singoli account

Quando si seleziona **singoli account** nel modello di progetto applicazione Web ASP.NET, il progetto include:

- Un server di autorizzazione di OAuth2.
- Un endpoint Web API per la gestione degli account utente
- Un modello di Entity Framework per l'archiviazione di account utente.

Di seguito sono le classi dell'applicazione principale che implementano queste funzionalità:

- `AccountController`. Fornisce un endpoint Web API per la gestione degli account utente. Il `Register` azione è l'unico che è stato usato in questa esercitazione. Altri metodi nella classe supportano la reimpostazione della password, gli account di accesso basati su social network e altre funzionalità.
- `ApplicationUser`, definito in /Models/IdentityModels.cs. Questa classe è il modello di Entity Framework per gli account utente nel database delle appartenenze.
- `ApplicationUserManager`, definito in /App\_Start/IdentityConfig.cs questa classe deriva da [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) esegue le operazioni sugli account utente, ad esempio la creazione di un nuovo utente, la verifica delle password e così via e rende automaticamente persistente modifiche al database.
- `ApplicationOAuthProvider`. Questo oggetto viene inserito nel middleware OWIN ed elabora gli eventi generati dal middleware. Deriva da [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Configurazione del Server di autorizzazione

In StartupAuth.cs, il codice seguente consente di configurare il server di autorizzazione di OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

Il `TokenEndpointPath` proprietà rappresenta il percorso URL dell'endpoint server di autorizzazione. Che rappresenta l'URL che l'app Usa per ottenere i token di connessione.

Il `Provider` proprietà consente di specificare un provider che si collega il middleware OWIN ed elabora gli eventi generati dal middleware.

Ecco il flusso di base quando si vuole che l'app ottenere un token:

1. Per ottenere un token di accesso, l'app invia una richiesta di ~ / Token.
2. Le chiamate di middleware OAuth `GrantResourceOwnerCredentials` sul provider.
3. Il provider chiami la `ApplicationUserManager` per convalidare le credenziali e creare un'identità basata sulle attestazioni.
4. Se l'operazione riesce, il provider crea un ticket di autenticazione, che viene usato per generare il token.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

Il middleware di OAuth non conoscenza gli account utente. Il provider comunica con il middleware e ASP.NET Identity. Per altre informazioni sull'implementazione del server di autorizzazione, vedere [OWIN OAuth 2.0 Authorization Server](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Configurazione di API Web per usare i token di connessione

Nel `WebApiConfig.Register` metodo, il codice seguente configura l'autenticazione per la pipeline di Web API:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

Il **HostAuthenticationFilter** classe consente l'autenticazione tramite token di connessione.

Il **SuppressDefaultHostAuthentication** metodo indica a Web API per ignorare qualsiasi tipo di autenticazione che si verifica prima che la richiesta raggiunga la pipeline di API Web, IIS oppure dal middleware OWIN. In questo modo, è possibile limitare l'API Web per autenticare solo usando token di connessione.

> [!NOTE]
> In particolare, la parte MVC dell'app potrebbe usare autenticazione basata su form, che archivia le credenziali in un cookie. L'autenticazione basata su cookie richiede l'uso dei token antifalsificazione, per impedire attacchi CSRF. Questo è un problema per le API web perché non vi è alcun modo pratico per l'API web per inviare il token antifalsificazione al client. (Per altre informazioni generali su questo problema, vedere [impedisce gli attacchi CSRF nell'API Web](preventing-cross-site-request-forgery-csrf-attacks.md).) La chiamata **SuppressDefaultHostAuthentication** garantisce che l'API Web non è vulnerabile agli attacchi CSRF da credenziali archiviate nei cookie.


Quando il client richiede una risorsa protetta, ecco cosa accade nella pipeline di Web API:

1. Il **HostAuthentication** filtro chiama il middleware di OAuth per convalidare il token.
2. Il middleware converte il token in un oggetto claimsidentity.
3. A questo punto, la richiesta è *autenticato* ma non *autorizzato*.
4. Il filtro di autorizzazione esamina l'identità delle attestazioni. Se le attestazioni di autorizzano dell'utente per tale risorsa, la richiesta è autorizzata. Per impostazione predefinita, il **[Authorize]** attributo autorizza qualsiasi richiesta che è stato autenticato. Tuttavia, è possibile autorizzare ruoli o altre attestazioni. Per altre informazioni, vedere [autenticazione e autorizzazione nell'API Web](authentication-and-authorization-in-aspnet-web-api.md).
5. Se i passaggi precedenti hanno esito positivo, il controller restituisce la risorsa protetta. In caso contrario, il client riceve un errore 401 (non autorizzato).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Risorse aggiuntive

- [ASP.NET Identity](../../../identity/index.md)
- [Informazioni sulle funzionalità di sicurezza nel modello di applicazione a singola pagina per VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). Post di blog MSDN da Hongye Sun.
- [Analisi dettagliata di Web API singoli account modello-parte 2: gli account locali](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Post di blog di Dominick Baier.
- [Ospitare l'autenticazione e l'API Web con OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Una spiegazione esauriente relativamente `SuppressDefaultHostAuthentication` e `HostAuthenticationFilter` da Brock Allen.
- [Personalizzazione delle informazioni di profilo in ASP.NET Identity nei modelli di Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Post di blog MSDN di Pranav Rastogi.
- [Per la gestione della durata richiesta per la classe UserManager in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Post di blog MSDN di Suhas Joshi, con una spiegazione esauriente relativamente il `UserManager` classe.
