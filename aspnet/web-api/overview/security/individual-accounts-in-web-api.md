---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Proteggere un'API Web con singoli account e un account di accesso locale in ASP.NET Web API 2.2 | Documenti Microsoft
author: MikeWasson
description: In questo argomento viene illustrato come proteggere un'API web usando OAuth2 per l'autenticazione per un database di appartenenza. Versioni del software utilizzate con l'esercitazione 201 Studio Visual...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e2056e769edf972cba830b31cf37f6418148ca73
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Proteggere un'API Web con singoli account e un account di accesso locale in ASP.NET Web API 2.2
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare App di esempio](https://github.com/MikeWasson/LocalAccountsApp)

> In questo argomento viene illustrato come proteggere un'API web usando OAuth2 per l'autenticazione per un database di appartenenza.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [2.2 API Web](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


In Visual Studio 2013, il modello di progetto API Web fornisce tre opzioni per l'autenticazione:

- **Singoli account.** L'app Usa un database di appartenenza.
- **Account aziendali.** Gli utenti accedono con i relativi Azure Active Directory, Office 365 o credenziali di Active Directory locale.
- **Autenticazione di Windows.** Questa opzione è solo per applicazioni Intranet e Usa il modulo IIS di autenticazione di Windows.

Per ulteriori informazioni su queste opzioni, vedere [creazione di progetti Web ASP.NET in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Singoli account forniscono due modi per un utente di accesso:

- **Account di accesso locale**. L'utente registra nel sito, immettere un nome utente e password. L'app consente di archiviare l'hash della password nel database delle appartenenze. Quando l'utente effettua l'accesso, il sistema di identità di ASP.NET viene verificata la password.
- **Account di accesso social**. L'utente accede con un servizio esterno, ad esempio Facebook, Microsoft o Google. Ancora l'app viene creata una voce per l'utente nel database delle appartenenze, ma non archivia le credenziali. L'utente viene autenticato mediante l'accesso al servizio esterno.

In questo articolo esamina lo scenario di account di accesso locale. Per account di accesso locali e sociali, API Web utilizzi OAuth2 per autenticare le richieste. Tuttavia, i flussi di credenziali sono diversi per account di accesso locale e sociale.

In questo articolo viene illustrato una semplice app che consente all'utente di accedere e inviare chiamate AJAX autenticate da un'API web. È possibile scaricare il codice di esempio [qui](https://github.com/MikeWasson/LocalAccountsApp). Il file Leggimi viene descritto come creare l'esempio da zero in Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

L'app di esempio utilizza Knockout.js per associazione a dati e jQuery per inviare le richieste AJAX. Sarà possibile concentrarsi sulle chiamate AJAX, in modo che non è necessario conoscere Knockout.js per questo articolo.

Inoltre, verranno descritti:

- L'app operazioni eseguite sul lato client.
- Qual è il problema nel server.
- Il traffico HTTP al centro.

In primo luogo, è necessario definire la terminologia OAuth2.

- *Risorsa*. Una parte dei dati che possono essere protetti.
- *Server di risorse*. Il server che ospita la risorsa.
- *Proprietario della risorsa*. L'entità che è possibile concedere l'autorizzazione per accedere a una risorsa. (In genere l'utente.)
- *Client*: app che richiede l'accesso alla risorsa. In questo articolo, il client è un web browser.
- *Token di accesso*. Token che concede l'accesso a una risorsa.
- *Token di connessione*. Un particolare tipo di token di accesso, con la proprietà che chiunque può utilizzare il token. In altre parole, un client non deve necessariamente una chiave di crittografia o un altro segreto per utilizzare un token di connessione. Per questo motivo, i token di connessione devono essere utilizzati solo su un HTTPS e devono avere date di scadenza relativamente breve.
- *Server di autorizzazione*. Un server che fornisce i token di accesso.

Un'applicazione può fungere da server di autorizzazione sia server di risorse. Il modello di progetto API Web segue questo modello.

## <a name="local-login-credential-flow"></a>Flusso delle credenziali di account di accesso locale

Per l'account di accesso locale, l'API Web utilizza il [flusso di risorse proprietario password](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) definito in OAuth2.

1. L'utente immette un nome e una password nel client.
2. Il client invia le credenziali al server di autorizzazione.
3. Il server di autorizzazione autentica le credenziali e restituisce un token di accesso.
4. Per accedere a una risorsa protetta, il client include il token di accesso nell'intestazione di autorizzazione della richiesta HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Quando si seleziona **singoli account** nel modello di progetto API Web, il progetto include un server di autorizzazione che convalida le credenziali utente e rilascia i token. Il diagramma seguente mostra lo stesso flusso di credenziali in termini di componenti dell'API Web.

![](individual-accounts-in-web-api/_static/image4.png)

In questo scenario, controller API Web fungono da server di risorse. Un filtro di autenticazione convalida i token di accesso e **[Authorize]** attributo viene utilizzato per proteggere una risorsa. Quando un controller o un'azione ha il **[Authorize]** attributo, tutte le richieste in tale controller o l'azione deve essere autenticata. In caso contrario, l'autorizzazione viene negata e l'API Web restituisce un errore 401 (non autorizzato).

Il server di autorizzazione e il filtro di autenticazione sia chiamerà un [middleware OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) componente che gestisce i dettagli di OAuth2. Verrà descritta la progettazione in modo più dettagliato più avanti in questa esercitazione.

## <a name="sending-an-unauthorized-request"></a>L'invio di una richiesta non autorizzata

Per iniziare, eseguire l'app e fare clic su di **chiamare API** pulsante. Al completamento della richiesta, si dovrebbe vedere un messaggio di errore nel **risultato** casella. Ciò avviene perché la richiesta non contiene un token di accesso, la richiesta non è autorizzata.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

Il **chiamare API** pulsante Invia una richiesta AJAX ~/api/valori, che richiama un'azione del controller API Web. Di seguito è riportata la sezione di codice JavaScript che invia la richiesta AJAX. Nell'applicazione di esempio, tutto il codice di app JavaScript si trova nel file Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Fino a quando l'utente si connette, non vi è alcun token di connessione e pertanto alcuna intestazione di autorizzazione nella richiesta. In questo modo la richiesta restituire un errore 401.

Di seguito è riportata la richiesta HTTP. (Utilizzato [Fiddler](http://www.telerik.com/fiddler) per acquisire il traffico HTTP.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Si noti che la risposta include un'intestazione Www-Authenticate con la richiesta di verifica impostato su una connessione. Che indica che il server è previsto un token di connessione.

## <a name="register-a-user"></a>Registrazione di un utente

Nel **registrare** sezione dell'app, immettere un messaggio di posta elettronica e una password, quindi scegliere il **registrare** pulsante.

Non è necessario utilizzare un indirizzo di posta elettronica valido per questo esempio, ma un'applicazione reale si conferma l'indirizzo. (Vedere [creare un'app web di ASP.NET MVC 5 sicura con l'accesso, la reimpostazione di posta elettronica di conferma e la password](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Per la password, utilizzare simile al seguente "Password1!", con una lettera maiuscola, lettere minuscole, numero e i caratteri non alfanumerici. Per semplificare l'app, tralasciato convalida lato client, pertanto se si verifica un problema con il formato della password, si otterrà un errore 400 (richiesta non valida).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

Il **registrare** pulsante Invia una richiesta POST per ~/api/Account/Register /. Il corpo della richiesta è un oggetto JSON che contiene il nome e la password. Ecco il codice JavaScript che invia la richiesta:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Richiesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Questa richiesta viene gestita dalla `AccountController` classe. Internamente, `AccountController` Usa identità ASP.NET per gestire il database delle appartenenze.

Se si esegue l'app localmente da Visual Studio, gli account utente vengono archiviati nel database locale nella tabella AspNetUsers. Per visualizzare le tabelle in Visual Studio, scegliere il **vista** dal menu **Esplora Server**, quindi espandere **connessioni dati**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Ottenere un accesso Token

Finora che abbiamo realizzato non qualsiasi OAuth, ma il server di autorizzazione OAuth in azione, ora è visualizzato quando si richiede un token di accesso. Nel **Accedi** area dell'applicazione di esempio, immettere il messaggio di posta elettronica e la password e fare clic su **Accedi**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

Il **Accedi** pulsante Invia una richiesta all'endpoint token. Il corpo della richiesta contiene i dati codificati negli url di form seguenti:

- concedere\_tipo: "password"
- nome utente: &lt;posta elettronica dell'utente&gt;
- password: &lt;password&gt;

Ecco il codice JavaScript che invia la richiesta AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Se la richiesta ha esito positivo, il server di autorizzazione restituisce un token di accesso nel corpo della risposta. Si noti che, il token viene memorizzato in archiviazione di sessione, da utilizzare in un secondo momento durante l'invio di richieste all'API. A differenza di alcuni moduli di autenticazione (ad esempio l'autenticazione basata su cookie), il browser non includerà automaticamente il token di accesso nelle richieste successive. L'applicazione deve essere eseguita in modo esplicito. Che è utile poiché limita [vulnerabilità CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Richiesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

È possibile vedere che la richiesta contiene le credenziali dell'utente. Si *deve* utilizzare HTTPS per garantire la sicurezza di livello di trasporto.

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Per migliorare la leggibilità, è rientrato JSON e troncato il token di accesso, è piuttosto lungo.

Il `access_token`, `token_type`, e `expires_in` le proprietà sono definite nella specifica OAuth2. Le altre proprietà (`userName`, `.issued`, e `.expires`) sono solo a scopo informativo. È possibile trovare il codice che aggiunge proprietà aggiuntive di `TokenEndpoint` (metodo), nel file /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Inviare una richiesta autenticata

Ora che è disponibile un token di connessione, è possibile rendere una richiesta autenticata all'API. Questa operazione viene eseguita impostando l'intestazione di autorizzazione nella richiesta. Fare clic su di **chiamare API** pulsante nuovo per visualizzare il risultato.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Richiesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Risposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Effettuare la disconnessione

Poiché il browser non memorizza nella cache le credenziali o il token di accesso, la disconnessione è semplicemente una "dimenticare" il token, rimuovendolo dall'archivio di sessione:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Comprendere il modello di progetto di singoli account

Quando si seleziona **singoli account** nel modello di progetto applicazione Web ASP.NET, il progetto include:

- Un server di autorizzazione OAuth2.
- Un endpoint Web API per la gestione degli account utente
- Un modello di Entity Framework per l'archiviazione di account utente.

Di seguito sono le classi dell'applicazione principale che implementano queste funzionalità:

- `AccountController`. Fornisce un endpoint Web API per la gestione degli account utente. Il `Register` azione è l'unica che è stato usato in questa esercitazione. Altri metodi sulla classe supportano la reimpostazione della password, gli account di accesso social networking e altre funzionalità.
- `ApplicationUser`, definito in /Models/IdentityModels.cs. Questa classe è il modello di Entity Framework per gli account utente nel database delle appartenenze.
- `ApplicationUserManager`, definito in /App\_Start/IdentityConfig.cs tale classe deriva dalla [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) e vengono eseguite operazioni sugli account utente, ad esempio la creazione di un nuovo utente, la verifica delle password e così via e rende automaticamente persistente modifiche al database.
- `ApplicationOAuthProvider`. L'oggetto si collega il middleware OWIN ed elabora gli eventi generati dal middleware. Deriva da [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Configurazione del Server di autorizzazione

In StartupAuth.cs, il codice seguente consente di configurare il server di autorizzazione OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

Il `TokenEndpointPath` proprietà è il percorso URL endpoint del server di autorizzazione. Che rappresenta l'URL viene utilizzato tale app per ottenere il token di connessione.

Il `Provider` proprietà specifica un provider che si collega il middleware OWIN ed elabora gli eventi generati dal middleware.

Ecco il flusso di base quando si desidera ottenere un token all'app:

1. Per ottenere un token di accesso, l'app invia una richiesta di ~ / Token.
2. Le chiamate di middleware OAuth `GrantResourceOwnerCredentials` nel provider.
3. Il provider chiama il `ApplicationUserManager` per convalidare le credenziali e creare un'identità basata sulle attestazioni.
4. Se l'operazione riesce, il provider crea un ticket di autenticazione, viene utilizzato per generare il token.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

Il middleware di OAuth non conosce gli account utente. Il provider gestisce la comunicazione tra il middleware e l'identità di ASP.NET. Per ulteriori informazioni sull'implementazione di server di autorizzazione, vedere [OWIN OAuth 2.0 autorizzazione Server](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Configurazione Web API, utilizzare i token di connessione

Nel `WebApiConfig.Register` metodo, il codice seguente imposta l'autenticazione per la pipeline di Web API:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

Il **HostAuthenticationFilter** classe consente l'autenticazione tramite i token di connessione.

Il **SuppressDefaultHostAuthentication** metodo indica l'API Web per ignorare qualsiasi autenticazione che si verifica prima che la richiesta raggiunga la pipeline di Web API, da IIS o dal middleware OWIN. In questo modo, è possibile limitare l'API Web per l'autenticazione solo tramite i token di connessione.

> [!NOTE]
> In particolare, la parte MVC dell'app può utilizzare autenticazione basata su form, che archivia le credenziali in un cookie. L'autenticazione basata su cookie richiede l'utilizzo di token antifalsificazione, per impedire attacchi CSRF. Questo è un problema per il web API, poiché non esiste alcun modo pratico per l'API web per inviare il token antifalsificazione al client. (Per ulteriori informazioni su questo problema, vedere [prevenzione degli attacchi CSRF nell'API Web](preventing-cross-site-request-forgery-csrf-attacks.md).) La chiamata **SuppressDefaultHostAuthentication** garantisce che l'API Web non è vulnerabile ad attacchi CSRF da credenziali archiviate nei cookie.


Quando il client richiede una risorsa protetta, ecco cosa accade nella pipeline di Web API:

1. Il **HostAuthentication** filtro chiama il middleware di OAuth per convalidare il token.
2. Il middleware converte il token in un'identità basata sulle attestazioni.
3. A questo punto, la richiesta è *autenticato* ma non *autorizzato*.
4. Il filtro di autorizzazione esamina l'identità basata sulle attestazioni. Se le attestazioni di autorizzano dell'utente per tale risorsa, la richiesta è autorizzata. Per impostazione predefinita, il **[Authorize]** attributo autorizzare qualsiasi richiesta che viene autenticato. Tuttavia, è possibile autorizzare dal ruolo o da altre richieste. Per ulteriori informazioni, vedere [autenticazione e autorizzazione in Web API](authentication-and-authorization-in-aspnet-web-api.md).
5. Se i passaggi precedenti hanno esito positivo, il controller restituisce alla risorsa protetta. In caso contrario, il client riceve un errore 401 (non autorizzato).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Risorse aggiuntive

- [Identità di ASP.NET](../../../identity/index.md)
- [Informazioni sulle funzionalità di sicurezza nel modello di SPA per VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). Post di blog MSDN da Hongye Sun.
- [Sezionando Web API singoli account modello-parte 2: gli account locali](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Post di blog di Dominick Baier.
- [Host di autenticazione e l'API Web con OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Una spiegazione chiara della `SuppressDefaultHostAuthentication` e `HostAuthenticationFilter` da Brock Allen.
- [Personalizzazione delle informazioni del profilo in ASP.NET Identity nei modelli di Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Post di blog MSDN Pranav Rastogi.
- [Per la gestione della durata richiesta per la classe UserManager in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Post di blog MSDN di Suhas Joshi, con una spiegazione chiara della `UserManager` classe.
