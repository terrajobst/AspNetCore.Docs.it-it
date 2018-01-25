---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Server di autorizzazione OAuth 2.0 OWIN | Documenti Microsoft
author: hongyes
description: "In questa esercitazione fornisce le istruzioni su come implementare un Server di autorizzazione OAuth 2.0 tramite il middleware OWIN OAuth. Questo è il raggruppamento solo di un'esercitazione avanzata..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e5968f8d19191c3f44e9bd58f8e22a39d8d8faff
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="owin-oauth-20-authorization-server"></a>Server di autorizzazione OAuth 2.0 OWIN
====================
da [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione fornisce le istruzioni su come implementare un Server di autorizzazione OAuth 2.0 tramite il middleware OWIN OAuth. Si tratta di un'esercitazione avanzata solo vengono delineati i passaggi per creare un Server di autorizzazione OWIN OAuth 2.0. Non si tratta di un'esercitazione dettagliata. [Scaricare il codice di esempio](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
> 
> > [!NOTE]
> > In questa struttura dovrebbe non deve essere utilizzato per la creazione di un'app di produzione sicuro. Questa esercitazione è destinata a fornire solo una struttura su come implementare un Server di autorizzazione OAuth 2.0 tramite il middleware OWIN OAuth.
> 
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> | **Nell'esercitazione** | **Funziona anche con** |
> | --- | --- |
> | Windows 8,1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio 2013 Express per Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express). Visual Studio 2012, con l'aggiornamento più recente dovrebbero funzionare, ma l'esercitazione non è stata testata con esso e alcune selezioni di menu e finestre di dialogo sono diverse. |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli in [Katana progetto in GitHub](https://github.com/aspnet/AspNetKatana/). Per domande e commenti in merito alla esercitazione stesso, vedere la sezione commenti nella parte inferiore della pagina.


Il [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) consente un'app di terze parti ottenere l'accesso limitato a un servizio HTTP. Anziché utilizzare le credenziali del proprietario della risorsa per accedere a una risorsa protetta, il client ottiene un token di accesso (ovvero una stringa che indica un ambito specifico, durata e altri attributi di accesso). Vengono rilasciati token di accesso ai client di terze parti con l'approvazione del proprietario della risorsa da un server di autorizzazione.

In questa esercitazione illustra:

- Come creare un server di autorizzazione per supportare l'autorizzazione di quattro tipi di concessione e token di aggiornamento:
    - Concessione del codice di autorizzazione
    - Concessione implicita
    - Concessione delle credenziali di Password del proprietario della risorsa
    - Concessione delle credenziali client
- Creazione di un server delle risorse che è protetta da un token di accesso.
- Creazione di client di OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prerequisiti

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) o la versione gratuita [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), come indicato **versioni del Software** nella parte superiore della pagina.
- Familiarità con OWIN. Vedere [introduzione con il progetto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) e [novità di OWIN e Katana](index.md).
- Una certa familiarità con [OAuth](http://tools.ietf.org/html/rfc6749) terminologia, tra cui [ruoli](http://tools.ietf.org/html/rfc6749#section-1.1), [protocollo flusso](http://tools.ietf.org/html/rfc6749#section-1.2), e [autorizzazione Grant](http://tools.ietf.org/html/rfc6749#section-1.3). [Introduzione di OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) fornisce una buona introduzione.

## <a name="create-an-authorization-server"></a>Creare un Server di autorizzazione

In questa esercitazione verrà approssimativamente sketch su come usare [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) e MVC ASP.NET per creare un server di autorizzazione. Ci auguriamo che fornire più presto un download per l'esempio completo, come in questa esercitazione non include ogni passaggio. Innanzitutto, creare un'app web vuota denominata *AuthorizationServer* e installare i pacchetti seguenti:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Italiano (o qualsiasi altro account di accesso social del pacchetto, ad esempio l'italiano)

Aggiungere un [classe di avvio di OWIN](owin-startup-class-detection.md) nella cartella radice del progetto denominata *avvio*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Creare un *App\_avviare* cartella. Selezionare il *App\_avviare* cartella e utilizzare Maiusc + Alt + A per aggiungere la versione di scaricata la *AuthorizationServer\App\_Start\Startup.Auth.cs* file.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Il codice precedente consente i cookie e l'autenticazione di Google, vengono utilizzati dal server di autorizzazione stesso per gestire gli account di accesso applicazione/esterna.

Il `UseOAuthAuthorizationServer` è il metodo di estensione per il server di autorizzazione del programma di installazione. Le opzioni di installazione sono:

- `AuthorizeEndpointPath`: Il percorso della richiesta in cui le applicazioni client reindirizzeranno l'agente utente per gli utenti di ottenere il consenso per emettere un token o codice. Deve iniziare con una barra iniziale, ad esempio, "`/Authorize`".
- `TokenEndpointPath`: Le applicazioni client del percorso richiesta comunicano direttamente per ottenere il token di accesso. Deve iniziare con una barra iniziale, come "/token". Se il client viene emesso un [client\_secret](http://tools.ietf.org/html/rfc6749#appendix-A.2), deve essere fornito a questo endpoint.
- `ApplicationCanDisplayErrors`: Impostare su `true` se l'applicazione web richiede la generazione di una pagina di errore personalizzato per gli errori di convalida del client in `/Authorize` endpoint. Questo è necessario solo per i casi in cui non viene reindirizzato il browser all'applicazione client, ad esempio, quando il `client_id` o `redirect_uri` non sono corrette. Il `/Authorize` endpoint prevede che "oauth. Errore","oauth. ErrorDescription"e"oauth. Proprietà ErrorUri"vengono aggiunti all'ambiente OWIN. 

    > [!NOTE]
    > Se non è true, il server di autorizzazione restituirà una pagina di errore predefinita con i dettagli dell'errore.
- `AllowInsecureHttp`: True per consentire di autorizzare e le richieste di token per arrivare agli indirizzi URI HTTP e per consentire in ingresso `redirect_uri` autorizzare indirizzi URI HTTP per i parametri di richiesta. 

    > [!WARNING]
    > Sicurezza - si tratta per solo per lo sviluppo.
- `Provider`: L'oggetto fornito dall'applicazione per elaborare gli eventi generati dal middleware del Server di autorizzazione. L'applicazione può implementare completamente l'interfaccia o è possibile creare un'istanza di `OAuthAuthorizationServerProvider` e assegnare delegati necessari per questo server supporta i flussi di OAuth.
- `AuthorizationCodeProvider`: Genera un codice di autorizzazione monouso da restituire all'applicazione client. Per il server di OAuth proteggere l'applicazione **deve** fornire un'istanza per `AuthorizationCodeProvider` in cui il token prodotto dal `OnCreate/OnCreateAsync` evento viene considerato valido solo per una chiamata a `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Genera un token di aggiornamento che può essere utilizzato per produrre un nuovo token di accesso quando necessario. Se non è stato specificato il server di autorizzazione non restituirà i token di aggiornamento dal `/Token` endpoint.

## <a name="account-management"></a>Gestione degli account

OAuth non è rilevante dove o come gestire le informazioni sull'account utente. Ha [ASP.NET Identity](../../../identity/index.md) che è responsabile. In questa esercitazione è semplificano il codice di gestione di account e assicurarsi che l'utente può eseguire l'accesso tramite il middleware di cookie OWIN. Ecco il codice di esempio semplificato per la `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri`viene utilizzato per convalidare il client con il relativo URL di reindirizzamento registrato. `ValidateClientAuthentication`Controlla l'intestazione di schema di base e il corpo del form per ottenere le credenziali del client.

Pagina di accesso è illustrata di seguito:

![](owin-oauth-20-authorization-server/_static/image1.png)

Esaminare IETF OAuth 2 [concessione del codice di autorizzazione](http://tools.ietf.org/html/rfc6749#section-4.1) sezione ora. 

**Provider** (nella tabella riportata di seguito) è [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Provider, che è di tipo `OAuthAuthorizationServerProvider`, che contiene tutti gli eventi server OAuth. 

| Passaggi del flusso dalla sezione di concessione del codice di autorizzazione | Download di esempio esegue le operazioni con: |
| --- | --- |
|  |  |
| (A) il client avvia il flusso indirizzando agente del proprietario della risorsa utente all'endpoint di autorizzazione. Il client include un URI di reindirizzamento a cui il server di autorizzazione verrà inviato l'agente utente nuovamente una volta completato l'accesso concesso (o negato), ambito richiesto, lo stato locale e il relativo identificatore client. | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) il server di autorizzazione autentica il proprietario della risorsa (tramite l'agente utente) e stabilisce se il proprietario della risorsa concede o Nega la richiesta di accesso del client. | **&lt;Se l'utente concede l'accesso&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) supponendo che il proprietario della risorsa concede l'accesso, il server di autorizzazione reindirizza l'agente utente al client utilizzando l'URI fornito il reindirizzamento delle versioni precedenti (nella richiesta o durante la registrazione del client). ... |  |
|  |  |
| (D) il client richiede un token di accesso dall'endpoint del token del server di autorizzazione, includendo il codice di autorizzazione ricevuto nel passaggio precedente. Quando si effettua la richiesta, il client autentica con il server di autorizzazione. Il client include il reindirizzamento che URI utilizzato per ottenere il codice di autorizzazione per la verifica. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Un'implementazione di esempio per `AuthorizationCodeProvider.CreateAsync` e `ReceiveAsync` per controllare la creazione e la convalida del codice di autorizzazione è illustrato di seguito.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Il codice precedente Usa un dizionario simultaneo in memoria per archiviare il ticket dell'identità e di codice e il ripristino dell'identità dopo aver ricevuto il codice. In un'applicazione reale, verrà sostituito da un archivio dati persistente. L'endpoint di autorizzazione è per il proprietario della risorsa concedere l'accesso al client. In genere, è necessario che un'interfaccia utente per consentire all'utente di fare clic su un pulsante e confermare la concessione. Middleware OWIN OAuth consente al codice dell'applicazione gestire l'endpoint di autorizzazione. Nella nostra app di esempio, si utilizzano controller MVC chiamato `OAuthController` da gestire. Di seguito è riportata l'implementazione di esempio:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

Il `Authorize` azione verificherà innanzitutto se l'utente è connesso al server di autorizzazione. In caso contrario, il middleware di autenticazione richiede al chiamante di eseguire l'autenticazione utilizzando il cookie "Application" e reindirizza alla pagina di accesso. (Vedere il codice evidenziato sopra). Se l'utente ha effettuato l'accesso, esegue il rendering della visualizzazione autorizza, come illustrato di seguito:

![](owin-oauth-20-authorization-server/_static/image2.png)

Se il **Grant** pulsante è selezionato, il `Authorize` azione consente di creare una nuova identità "Bearer" e accedere con il. Attiva il server di autorizzazione per generare un token di connessione e restituirlo al client con payload JSON. 

### <a name="implicit-grant"></a>Concessione implicita

Fare riferimento alla IETF OAuth 2 [concessione implicita](http://tools.ietf.org/html/rfc6749#section-4.2) sezione ora.

 Il [concessione implicita](http://tools.ietf.org/html/rfc6749#section-4.2) flusso illustrato nella figura 4 viene illustrato il flusso e mapping di OAuth OWIN che segue middleware.  

| Passaggi del flusso dalla sezione di concessione implicita | Download di esempio esegue le operazioni con: |
| --- | --- |
|  |  |
| (A) il client avvia il flusso indirizzando agente del proprietario della risorsa utente all'endpoint di autorizzazione. Il client include un URI di reindirizzamento a cui il server di autorizzazione verrà inviato l'agente utente nuovamente una volta completato l'accesso concesso (o negato), ambito richiesto, lo stato locale e il relativo identificatore client. | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) il server di autorizzazione autentica il proprietario della risorsa (tramite l'agente utente) e stabilisce se il proprietario della risorsa concede o Nega la richiesta di accesso del client. | **&lt;Se l'utente concede l'accesso&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) supponendo che il proprietario della risorsa concede l'accesso, il server di autorizzazione reindirizza l'agente utente al client utilizzando l'URI fornito il reindirizzamento delle versioni precedenti (nella richiesta o durante la registrazione del client). ... |  |
|  |  |
| (D) il client richiede un token di accesso dall'endpoint del token del server di autorizzazione, includendo il codice di autorizzazione ricevuto nel passaggio precedente. Quando si effettua la richiesta, il client autentica con il server di autorizzazione. Il client include il reindirizzamento che URI utilizzato per ottenere il codice di autorizzazione per la verifica. |  |

Poiché è già implementato l'endpoint di autorizzazione (`OAuthController.Authorize` azione) per la concessione del codice di autorizzazione, verrà automaticamente attivato anche flusso implicito. Nota: `Provider.ValidateClientRedirectUri` viene utilizzato per convalidare l'ID client con il relativo URL di reindirizzamento, che consente di proteggere il flusso di concessione implicita di inviare l'accesso client di token dannosi ([attacco Man-in-the-middle](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Concessione delle credenziali di Password del proprietario della risorsa

Fare riferimento alla IETF OAuth 2 [concessione di credenziali di Password proprietario risorsa](http://tools.ietf.org/html/rfc6749#section-4.3) sezione ora.

 Il [concessione di credenziali di Password proprietario risorsa](http://tools.ietf.org/html/rfc6749#section-4.3) flusso illustrato nella figura 5 viene illustrato il flusso e mapping di OAuth OWIN che segue middleware.  

| Passaggi del flusso dalla sezione concessione credenziali Password del proprietario della risorsa | Download di esempio esegue le operazioni con: |
| --- | --- |
|  |  |
| (A) il proprietario della risorsa fornisce al client con il nome utente e la password. |  |
|  |  |
| (B) il client richiede un token di accesso dall'endpoint del token del server di autorizzazione includendo le credenziali ricevute dal proprietario della risorsa. Quando si effettua la richiesta, il client autentica con il server di autorizzazione. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) il server di autorizzazione autentica il client e convalida le credenziali del proprietario di risorse e se è valido, emette un token di accesso. |  |

Di seguito è l'implementazione di esempio per `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Il codice sopra riportato è destinato a illustrare in questa sezione dell'esercitazione e non deve essere utilizzato in sicura o app di produzione. Non controlla le credenziali di proprietari della risorsa. Si presuppone di ogni tipo di credenziali sia valido e crea una nuova identità per tale. La nuova identità da utilizzare per generare il token di accesso e token di aggiornamento. Sostituire il codice con il proprio codice di gestione di account sicuro.


### <a name="client-credentials-grant"></a>Concessione delle credenziali client

Fare riferimento alla IETF OAuth 2 [concessione di credenziali Client](http://tools.ietf.org/html/rfc6749#section-4.4) sezione ora.

 Il [concessione di credenziali Client](http://tools.ietf.org/html/rfc6749#section-4.4) flusso illustrato nella figura 6 viene illustrato il flusso e mapping di OAuth OWIN che segue middleware.  

| Passaggi del flusso dalla sezione di concessione di credenziali Client | Download di esempio esegue le operazioni con: |
| --- | --- |
|  |  |
| (A) il client autentica con il server di autorizzazione e richiede un token di accesso dall'endpoint del token. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) il server di autorizzazione autentica il client e, se valido, genera un token di accesso. |  |

Di seguito è l'implementazione di esempio per `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Il codice sopra riportato è destinato a illustrare in questa sezione dell'esercitazione e non deve essere utilizzato in sicura o app di produzione. Sostituire il codice con il proprio codice di gestione client protette.


### <a name="refresh-token"></a>Token di aggiornamento

Fare riferimento alla IETF OAuth 2 [Token di aggiornamento](http://tools.ietf.org/html/rfc6749#section-1.5) sezione ora.

 Il [Token di aggiornamento](http://tools.ietf.org/html/rfc6749#section-1.5) flusso illustrato nella figura 2 viene illustrato il flusso e mapping di OAuth OWIN che segue middleware.  

| Passaggi del flusso dalla sezione di concessione di credenziali Client | Download di esempio esegue le operazioni con: |
| --- | --- |
|  |  |
| (G) il client richiede un nuovo token di accesso per l'autenticazione con il server di autorizzazione e presentare il token di aggiornamento. I requisiti di autenticazione client sono basati sul tipo di client e i criteri del server di autorizzazione. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) il server di autorizzazione autentica il client convalida il token di aggiornamento e se è valido, genera un nuovo token di accesso (e, facoltativamente, un nuovo token di aggiornamento). |  |

Di seguito è l'implementazione di esempio per `Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Creare un Server delle risorse che è protetta da Token di accesso

Creare un progetto di app web vuota e installare i seguenti pacchetti nel progetto:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Creare una classe di avvio e configurare l'autenticazione e l'API Web. Vedere *AuthorizationServer\ResourceServer\Startup.cs* nel download di esempio.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Vedere *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* nel download di esempio.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Vedere *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* nel download di esempio.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors`metodo consente la condivisione CORS per tutti i domini.
- `UseOAuthBearerAuthentication`metodo consente il middleware di autenticazione del token di connessione OAuth che consente di ricevere e convalidare i token di connessione dall'intestazione di autorizzazione nella richiesta.
- `Config.SuppressDefaultHostAuthenticaiton`evita la visualizzazione predefinita host entità autenticata dall'app, pertanto sarà tutte le richieste anonime dopo questa chiamata.
- `HostAuthenticationFilter`Abilita l'autenticazione solo per il tipo di autenticazione specificato. In questo caso, è il tipo di autenticazione bearer.

Per dimostrare l'identità autenticata, verrà creato un ApiController per restituire le attestazioni dell'utente corrente.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Se il server di autorizzazione e il server di risorse non sono presenti nello stesso computer, il middleware OAuth utilizzerà le chiavi del computer diverso per crittografare e decrittografare i token di accesso di connessione. Per condividere la stessa chiave privata tra entrambi i progetti, è lo stesso aggiungere `machinekey` impostazione sia in *Web. config* file.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Creare i client di OAuth 2.0

 Utilizziamo la [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) pacchetto NuGet per semplificare il codice client.

### <a name="authorization-code-grant-client"></a>Client di concessione del codice autorizzazione

 Questo client è un'applicazione MVC. Attiverà un flusso di concessione di codice di autorizzazione per ottenere il token di accesso dal back-end. Contiene una singola pagina, come illustrato di seguito:

![](owin-oauth-20-authorization-server/_static/image3.png)

- Il **Authorize** pulsante reindirizzerà browser al server di autorizzazione per comunicare al proprietario della risorsa per concedere l'accesso a questo client.
- Il **aggiornamento** pulsante verrà visualizzato un nuovo token di accesso e un token di aggiornamento tramite l'aggiornamento corrente token.
- Il **accesso protetto risorsa API** pulsante chiamerà il server di risorse per ottenere i dati delle attestazioni dell'utente corrente e li visualizza nella pagina.

Ecco il codice di esempio di `HomeController` del client.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth`Per impostazione predefinita, richiede SSL. Poiché demo sta usando HTTP, è necessario aggiungere l'impostazione nel file di configurazione seguente:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Sicurezza - mai disabilitare SSL in un'app di produzione. Le credenziali di accesso vengono ora inviate attraverso la rete in testo non crittografato. Il codice sopra riportato è solo per il debug di esempio locale e l'esplorazione.


### <a name="implicit-grant-client"></a>Client di concessione implicita

Il client utilizza JavaScript per:

1. Aprire una nuova finestra e il reindirizzamento all'endpoint authorize del Server di autorizzazione.
2. Ottenere il token di accesso da frammenti URL durante il reindirizzamento nuovamente.

Nella figura seguente viene illustrato questo processo:

![](owin-oauth-20-authorization-server/_static/image4.png)

Il client deve disporre di due pagine: uno per la home page e l'altro per i callback. Ecco l'esempio di codice disponibile in JavaScript il *cshtml* file:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Ecco il callback, la gestione del codice in *SignIn.cshtml* file:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Una procedura consigliata è di spostare il codice JavaScript in un file esterno e non incorporarlo con il markup Razor. Per semplificare questo esempio, che sono state combinate.


### <a name="resource-owner-password-credentials-grant-client"></a>Client di concedere le credenziali di Password del proprietario della risorsa

Si utilizza un'applicazione console per demo questo client. Ecco il codice:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Client di concessione di credenziali client

Come per la concessione di credenziali Password proprietario risorsa, ecco il codice di app console:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
