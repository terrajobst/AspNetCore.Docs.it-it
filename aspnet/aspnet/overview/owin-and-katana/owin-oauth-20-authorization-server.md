---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Server di autorizzazione OAuth 2.0 OWIN | Microsoft Docs
author: hongyes
description: Questa esercitazione illustra come implementare un Server di autorizzazione OAuth 2.0 usa il middleware OWIN OAuth. Questa è un'esercitazione avanzata tale raggruppamento solo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: d2c5e984fb5866ab4bc01e10600bad4ee88bd4ed
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387338"
---
<a name="owin-oauth-20-authorization-server"></a>Server di autorizzazione OAuth 2.0 OWIN
====================
dal [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Questa esercitazione illustra come implementare un Server di autorizzazione OAuth 2.0 usa il middleware OWIN OAuth. Si tratta di un'esercitazione avanzata che illustra solo i passaggi per creare un Server di autorizzazione di OWIN OAuth 2.0. Questo non è un'esercitazione dettagliata. [Scaricare il codice di esempio](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
> 
> > [!NOTE]
> > In questa struttura dovrebbe non essere voluta da utilizzare per la creazione di un'app di produzione sicuro. Questa esercitazione intende fornire solo una struttura su come implementare un Server di autorizzazione OAuth 2.0 usa il middleware OWIN OAuth.
> 
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> | **Illustrato nell'esercitazione** | **Si integra inoltre con** |
> | --- | --- |
> | Windows 8,1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio 2013 Express per Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express). Visual Studio 2012 con l'aggiornamento più recente dovrebbe funzionare correttamente, ma l'esercitazione non è stata testata con esso e alcune selezioni di menu e finestre di dialogo sono diverse. |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarle in [Katana Project su GitHub](https://github.com/aspnet/AspNetKatana/). Per domande e commenti relativi all'esercitazione, vedere la sezione dei commenti nella parte inferiore della pagina.


Il [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) consente a un'app di terze parti ottenere l'accesso limitato a un servizio HTTP. Anziché utilizzare le credenziali del proprietario della risorsa per accedere a una risorsa protetta, il client ottenga un token di accesso (ovvero una stringa che denota un ambito specifico, durata e altri attributi di accesso). I token di accesso vengono rilasciati ai client di terze parti da un server di autorizzazione con l'approvazione del proprietario della risorsa.

Questa esercitazione illustrerà quanto segue:

- Come creare un server di autorizzazione per supportare l'autorizzazione quattro tipi di concessione di token di aggiornamento:
    - Concessione del codice di autorizzazione
    - Concessione implicita
    - Concessione delle credenziali Password proprietario risorsa
    - Concessione delle credenziali client
- Creazione di un server di risorse che è protetta da un token di accesso.
- Creazione di client di OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prerequisiti

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) o la versione gratuita [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), come indicato in **versioni Software** nella parte superiore della pagina.
- Familiarità con OWIN. Visualizzare [Introduzione al Katana Project](https://msdn.microsoft.com/magazine/dn451439.aspx) e [novità OWIN e Katana](index.md).
- Conoscenza [OAuth](http://tools.ietf.org/html/rfc6749) terminologia, tra cui [ruoli](http://tools.ietf.org/html/rfc6749#section-1.1), [Protocol Flow](http://tools.ietf.org/html/rfc6749#section-1.2), e [concessione di autorizzazione](http://tools.ietf.org/html/rfc6749#section-1.3). [Introduzione a OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) offre una buona introduzione.

## <a name="create-an-authorization-server"></a>Creare un Server di autorizzazione

In questa esercitazione, saranno all'incirca sketch su come usare [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) e ASP.NET MVC per creare un server di autorizzazione. Ci auguriamo di presto forniscono un download per l'esempio completato, in questa esercitazione include ogni passaggio. Innanzitutto, creare un'app web vuota denominata *AuthorizationServer* e installare i pacchetti seguenti:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (o qualsiasi altro account di accesso basati su social network del pacchetto, ad esempio owin)

Aggiungere un [OWIN Startup class](owin-startup-class-detection.md) nella cartella radice del progetto denominata *avvio*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Creare un *App\_avviare* cartella. Selezionare il *App\_avviare* cartella e utilizzare Maiusc + Alt + A per aggiungere la versione scaricata del *AuthorizationServer\App\_Start\Startup.Auth.cs* file.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Il codice riportato sopra consente i cookie e l'autenticazione di Google, che vengono utilizzati dal server di autorizzazione per gestire gli account di accesso dell'applicazione ed esterni.

Il `UseOAuthAuthorizationServer` metodo di estensione è quella di configurare il server di autorizzazione. Le opzioni di installazione sono:

- `AuthorizeEndpointPath`: Il percorso della richiesta in cui le applicazioni client reindirizzeranno l'agente utente per ottenere gli utenti di fornire il consenso per emettere un token o codice. Deve iniziare con una barra iniziale, ad esempio, "`/Authorize`".
- `TokenEndpointPath`: Le applicazioni client del percorso richiesta comunicano direttamente per ottenere il token di accesso. Deve iniziare con una barra iniziale, come "/token". Se il client viene emesso un [client\_segreto](http://tools.ietf.org/html/rfc6749#appendix-A.2), ma deve essere fornita a questo endpoint.
- `ApplicationCanDisplayErrors`: Impostare `true` se l'applicazione web desidera generare una pagina di errore personalizzato per gli errori di convalida client su `/Authorize` endpoint. È necessario solo per casi in cui non viene reindirizzato il browser all'applicazione client, ad esempio, quando la `client_id` o `redirect_uri` non sono corrette. Il `/Authorize` endpoint prevede che il "oauth. Errore","oauth. ErrorDescription"e"oauth. Proprietà ErrorUri"vengono aggiunte all'ambiente OWIN. 

    > [!NOTE]
    > In caso contrario true, il server di autorizzazione restituirà una pagina di errore predefinita con i dettagli dell'errore.
- `AllowInsecureHttp`: True per consentire l'autorizzazione e le richieste di token in arrivo sugli indirizzi URI HTTP e di consentire l'ingresso `redirect_uri` autorizzare indirizzi URI HTTP per i parametri di richiesta. 

    > [!WARNING]
    > Security - si tratta solo per lo sviluppo.
- `Provider`: L'oggetto fornito dall'applicazione per elaborare gli eventi generati dal middleware del Server di autorizzazione. L'applicazione deve implementare l'interfaccia completamente o è possibile creare un'istanza di `OAuthAuthorizationServerProvider` e assegnare delegati necessari per questo server supporta i flussi di OAuth.
- `AuthorizationCodeProvider`: Genera un codice di autorizzazione monouso da restituire all'applicazione client. Proteggere l'applicazione per il server OAuth **devono** fornire un'istanza di `AuthorizationCodeProvider` in cui il token prodotto dal `OnCreate/OnCreateAsync` evento viene considerato valido solo per una chiamata a `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Genera un token di aggiornamento che può essere utilizzato per produrre un nuovo token di accesso quando necessario. Se non specificato il server di autorizzazione non restituirà i token di aggiornamento dal `/Token` endpoint.

## <a name="account-management"></a>Gestione degli account

OAuth non è rilevante dove o come è gestire le informazioni sull'account utente. Dispone [ASP.NET Identity](../../../identity/index.md) che è responsabile. In questa esercitazione, si verrà semplificano il codice di gestione di account e assicurarsi che l'utente può eseguire l'accesso tramite middleware OWIN cookie. Ecco il codice di esempio semplificato per la `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` viene usato per convalidare il client con il relativo URL di reindirizzamento registrato. `ValidateClientAuthentication` Controlla l'intestazione di base dello schema e il corpo del form per ottenere le credenziali del client.

La pagina di accesso è la seguente:

![](owin-oauth-20-authorization-server/_static/image1.png)

Esaminare la IETF OAuth 2 [concessione del codice di autorizzazione](http://tools.ietf.org/html/rfc6749#section-4.1) sezione ora. 

**Provider** (nella tabella seguente) viene [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Provider, che è di tipo `OAuthAuthorizationServerProvider`, che contiene tutti gli eventi server OAuth. 

| Passaggi del flusso dalla sezione di concessione del codice di autorizzazione | Download di esempio esegue questi passaggi con: |
| --- | --- |
|  |  |
| (A) il client avvia il flusso indirizzando l'agente utente del proprietario della risorsa per l'endpoint di autorizzazione. Il client include il relativo identificatore client, ambito richiesto, lo stato locale e un URI di reindirizzamento a cui il server di autorizzazione Invia l'agente utente dopo l'accesso viene concesso (o negato). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) il server di autorizzazione autentica il proprietario della risorsa (tramite l'agente utente) e stabilisce se il proprietario della risorsa concede o Nega la richiesta di accesso del client. | **&lt;Se l'utente concede l'accesso&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) supponendo che il proprietario della risorsa concede l'accesso, il server di autorizzazione reindirizza l'agente utente al client usando l'URI fornito il reindirizzamento delle versioni precedenti (nella richiesta o durante la registrazione del client). ... |  |
|  |  |
| (D) il client richiede un token di accesso dall'endpoint di token del server di autorizzazione, includendo il codice di autorizzazione ricevuto nel passaggio precedente. Quando si effettua la richiesta, con il server di autorizzazione autentica il client. Il client include il reindirizzamento che URI usato per ottenere il codice di autorizzazione per la verifica. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Per un esempio di implementazione `AuthorizationCodeProvider.CreateAsync` e `ReceiveAsync` per controllare la creazione e la convalida del codice di autorizzazione è illustrato di seguito.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Il codice precedente Usa un dizionario simultaneo in memoria per memorizzare il ticket di codice e l'identità e il ripristino dell'identità dopo la ricezione del codice. In un'applicazione reale, questo verrà sostituito da un archivio dati permanente. L'endpoint di autorizzazione è proprietario della risorsa concedere l'accesso al client. In genere, è necessario un'interfaccia utente per consentire all'utente di fare clic su un pulsante e confermare la concessione. Middleware OWIN OAuth consente al codice dell'applicazione gestire l'endpoint di autorizzazione. Nel nostro app di esempio, utilizziamo un controller MVC chiamato `OAuthController` gestirlo. Ecco l'implementazione di esempio:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

Il `Authorize` azione controlla prima se l'utente ha effettuato l'accesso al server di autorizzazione. In caso contrario, il middleware di autenticazione al chiamante di eseguire l'autenticazione usando il cookie "Applicazione" sono le problematiche e reindirizza alla pagina di accesso. (Vedere il codice evidenziato sopra). Se l'utente ha effettuato l'accesso, eseguirà il rendering della visualizzazione Authorize, come illustrato di seguito:

![](owin-oauth-20-authorization-server/_static/image2.png)

Se il **concessione** pulsante è selezionato, il `Authorize` azione consente di creare una nuova identità "Bearer" e accedere con lo. Attiva il server di autorizzazione per generare un token di connessione e restituirlo al client con payload JSON. 

### <a name="implicit-grant"></a>Concessione implicita

Fare riferimento OAuth 2 di IETF [concessione implicita](http://tools.ietf.org/html/rfc6749#section-4.2) sezione ora.

 Il [concessione implicita](http://tools.ietf.org/html/rfc6749#section-4.2) flusso illustrato nella figura 4 viene illustrato il flusso e di mapping che OAuth OWIN segue middleware.  

| Passaggi del flusso dalla sezione di concessione implicita | Download di esempio esegue questi passaggi con: |
| --- | --- |
|  |  |
| (A) il client avvia il flusso indirizzando l'agente utente del proprietario della risorsa per l'endpoint di autorizzazione. Il client include il relativo identificatore client, ambito richiesto, lo stato locale e un URI di reindirizzamento a cui il server di autorizzazione Invia l'agente utente dopo l'accesso viene concesso (o negato). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) il server di autorizzazione autentica il proprietario della risorsa (tramite l'agente utente) e stabilisce se il proprietario della risorsa concede o Nega la richiesta di accesso del client. | **&lt;Se l'utente concede l'accesso&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) supponendo che il proprietario della risorsa concede l'accesso, il server di autorizzazione reindirizza l'agente utente al client usando l'URI fornito il reindirizzamento delle versioni precedenti (nella richiesta o durante la registrazione del client). ... |  |
|  |  |
| (D) il client richiede un token di accesso dall'endpoint di token del server di autorizzazione, includendo il codice di autorizzazione ricevuto nel passaggio precedente. Quando si effettua la richiesta, con il server di autorizzazione autentica il client. Il client include il reindirizzamento che URI usato per ottenere il codice di autorizzazione per la verifica. |  |

Poiché è già implementato l'endpoint di autorizzazione (`OAuthController.Authorize` azione) per la concessione del codice di autorizzazione, abilita automaticamente anche il flusso implicito. Nota: `Provider.ValidateClientRedirectUri` viene usato per convalidare l'ID client con l'URL di reindirizzamento, che protegge il flusso di concessione implicita di inviare l'accesso token da malintenzionati client ([attacco Man-in-the-middle](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Concessione delle credenziali Password proprietario risorsa

Fare riferimento OAuth 2 di IETF [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) sezione ora.

 Il [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3) flusso illustrato nella figura 5 viene illustrato il flusso e di mapping che OAuth OWIN segue middleware.  

| Passaggi del flusso dalla sezione Resource Owner Password Credentials Grant | Download di esempio esegue questi passaggi con: |
| --- | --- |
|  |  |
| (A) il proprietario della risorsa fornisce al client con il nome utente e la password. |  |
|  |  |
| (B) il client richiede un token di accesso dall'endpoint di token del server di autorizzazione includendo le credenziali ricevute dal proprietario della risorsa. Quando si effettua la richiesta, con il server di autorizzazione autentica il client. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) il server di autorizzazione autentica il client e convalida le credenziali del proprietario risorsa e se valido, rilascia un token di accesso. |  |

Ecco l'implementazione di esempio per `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Il codice riportato sopra è destinato per spiegare in questa sezione dell'esercitazione e non deve essere utilizzato in sicura o le app di produzione. Non controlla le credenziali di proprietari di risorse. Si presuppone ogni credenziale è valida e crea una nuova identità per tale. La nuova identità verrà usata per generare il token di accesso e token di aggiornamento. Sostituire il codice con il proprio codice di gestione di account sicuro.


### <a name="client-credentials-grant"></a>Concessione delle credenziali client

Fare riferimento OAuth 2 di IETF [concessione delle credenziali Client](http://tools.ietf.org/html/rfc6749#section-4.4) sezione ora.

 Il [concessione delle credenziali Client](http://tools.ietf.org/html/rfc6749#section-4.4) flusso illustrato nella figura 6 viene illustrato il flusso e di mapping che OAuth OWIN segue middleware.  

| Passaggi del flusso dalla sezione di concessione delle credenziali Client | Download di esempio esegue questi passaggi con: |
| --- | --- |
|  |  |
| (A) il client esegue l'autenticazione con il server di autorizzazione e richiede un token di accesso dall'endpoint di token. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) il server di autorizzazione autentica il client e, se valido, rilascia un token di accesso. |  |

Ecco l'implementazione di esempio per `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Il codice riportato sopra è destinato per spiegare in questa sezione dell'esercitazione e non deve essere utilizzato in sicura o le app di produzione. Sostituire il codice con il proprio codice di gestione client protette.


### <a name="refresh-token"></a>Token di aggiornamento

Fare riferimento OAuth 2 di IETF [Token di aggiornamento](http://tools.ietf.org/html/rfc6749#section-1.5) sezione ora.

 Il [Token di aggiornamento](http://tools.ietf.org/html/rfc6749#section-1.5) flusso illustrato nella figura 2 viene illustrato il flusso e di mapping che OAuth OWIN segue middleware.  

| Passaggi del flusso dalla sezione di concessione delle credenziali Client | Download di esempio esegue questi passaggi con: |
| --- | --- |
|  |  |
| (G) il client richiede un nuovo token di accesso per l'autenticazione con il server di autorizzazione e presentare il token di aggiornamento. I requisiti di autenticazione client sono basati sul tipo di client e in criteri del server di autorizzazione. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) il server di autorizzazione autentica il client e convalida il token di aggiornamento e se valido, genera un nuovo token di accesso (e, facoltativamente, un nuovo token di aggiornamento). |  |

Ecco l'implementazione di esempio per `Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Creare un Server di risorse che è protetta dal Token di accesso

Creare un progetto di app web vuota e installare i pacchetti nel progetto in seguito:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Creare una classe startup e configurare l'autenticazione e l'API Web. Visualizzare *AuthorizationServer\ResourceServer\Startup.cs* nel download di esempio.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Visualizzare *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* nel download di esempio.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Visualizzare *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* nel download di esempio.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` metodo consente a CORS per tutti i domini.
- `UseOAuthBearerAuthentication` metodo consente di middleware di autenticazione basata su token di connessione OAuth che consente di ricevere e convalidare i token di connessione dall'intestazione di autorizzazione nella richiesta.
- `Config.SuppressDefaultHostAuthenticaiton` evita la visualizzazione predefinita ospitare entità autenticata dall'app, pertanto saranno tutte le richieste anonime dopo questa chiamata.
- `HostAuthenticationFilter` Abilita l'autenticazione solo per il tipo di autenticazione specificato. In questo caso, è il tipo di autenticazione bearer.

Per dimostrare l'identità autenticata, creiamo un ApiController per ottenere l'output di attestazioni dell'utente corrente.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Se il server di autorizzazione e il server di risorse non sono nello stesso computer, il middleware di OAuth utilizzerà le chiavi del computer diverso per crittografare e decrittografare i token di accesso di connessione. Per condividere la stessa chiave privata tra entrambi i progetti, si aggiunge la stessa `machinekey` impostazione in entrambe *Web. config* file.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Creare i client di OAuth 2.0

 Usiamo il [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) pacchetto NuGet per semplificare il codice client.

### <a name="authorization-code-grant-client"></a>Authorization Code Grant Client

 Questo client è un'applicazione MVC. Attiva un flusso di concessione di codice di autorizzazione per ottenere il token di accesso dal back-end. Dispone di una pagina singola come illustrato di seguito:

![](owin-oauth-20-authorization-server/_static/image3.png)

- Il **Authorize** pulsante reindirizzerà browser al server di autorizzazione per notificare il proprietario della risorsa per concedere l'accesso a questo client.
- Il **Aggiorna** pulsante verrà visualizzato un nuovo token di accesso e un token di aggiornamento tramite l'aggiornamento corrente token.
- Il **API di risorsa protetta da accesso** pulsante chiamerà il server di risorse per ottenere i dati delle attestazioni dell'utente corrente e visualizzarli nella pagina.

Ecco il codice di esempio del `HomeController` del client.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` richiede SSL per impostazione predefinita. Poiché la demo Usa HTTP, è necessario aggiungere seguente impostazione nel file di configurazione:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Security - mai disabilitare SSL in un'app di produzione. Le credenziali di accesso vengono ora inviate via cavo in testo non crittografato. Il codice sopra riportato è solo per l'esplorazione e il debug di esempio locali.


### <a name="implicit-grant-client"></a>Client di concessione implicita

Questo client utilizza JavaScript per:

1. Aprire una nuova finestra e il reindirizzamento all'endpoint di autorizzazione del Server di autorizzazione.
2. Ottenere il token di accesso da frammenti di URL quando viene reindirizzato nuovamente.

L'immagine seguente illustra questo processo:

![](owin-oauth-20-authorization-server/_static/image4.png)

Il client deve disporre di due pagine: uno per la home page e l'altra per i callback. Ecco l'esempio di codice trovato in JavaScript la *index. cshtml* file:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Ecco il callback nel codice di gestione *SignIn.cshtml* file:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Una procedura consigliata è spostare il codice JavaScript in un file esterno e non incorporala con il markup Razor. Per semplificare questo esempio, sono stati combinati.


### <a name="resource-owner-password-credentials-grant-client"></a>Client concessione delle credenziali Password proprietario risorsa

Si usa un'app console per questo client della demo. Ecco il codice:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Client di concessione di credenziali client

Analogamente a Resource Owner Password Credentials Grant, ecco il codice di app console:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
