---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: La registrazione utilizzando siti esterni in un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: Questo articolo illustra come eseguire l'accesso al sito di ASP.NET Web Pages (Razor) usando Facebook, Google, Twitter, Yahoo e ad altri siti, vale a dire, come supportare...
ms.author: aspnetcontent
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: e6c5b578b4c74fd04136d31751d51b59ba9197ba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825437"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>La registrazione utilizzando siti esterni in un sito di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come eseguire l'accesso al sito di ASP.NET Web Pages (Razor) usando Facebook, Google, Twitter, Yahoo e ad altri siti, vale a dire, sul supporto di OAuth e OpenID all'interno del sito.
> 
> Che cosa si apprenderà come:
> 
> - Come abilitare l'accesso da altri siti quando si usa il modello di sito Starter WebMatrix.
> 
> Si tratta della funzionalità ASP.NET introdotta nell'articolo:
> 
> - Il `OAuthWebSecurity` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3

Include il supporto per ASP.NET Web Pages [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) provider. Con questi provider, è possibile consentire agli utenti di accedere al sito usando le proprie credenziali da Facebook, Twitter, Microsoft e Google. Ad esempio, per accedere con un account di Facebook, gli utenti possono semplicemente scegliere un'icona di Facebook, che li reindirizza alla pagina di accesso di Facebook in cui immettere le informazioni utente. È quindi possibile associare l'account di accesso di Facebook con il proprio account nel sito. Un miglioramento correlato alle funzionalità di appartenenza a pagine Web è che gli utenti possono associare più account di accesso (inclusi gli accessi dai siti di social networking) con un singolo account nel sito Web.

La seguente immagine illustra la pagina di accesso dal **Starter Site** modello, in cui un utente può scegliere un'icona di Facebook, Twitter, Google o Microsoft per abilitare la registrazione con un account esterno:

![provider esterni](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

È possibile abilitare l'appartenenza a OAuth e OpenID rimuovendo poche righe di codice nel **Starter Site** modello. I metodi e proprietà si usano per lavorare con OAuth e OpenID provider è nel `WebMatrix.Security.OAuthWebSecurity` classe. Il **Starter Site** modello include un'infrastruttura completa dell'appartenenza, completa di una pagina di accesso, un database di appartenenza e tutto il codice necessario consentire agli utenti di accedere al sito usando le credenziali locali o quelle di un altro sito .

In questa sezione fornisce un esempio di come consentire agli utenti di accedere da siti esterni a un sito di base di **Starter Site** modello. Dopo aver creato un sito di base, tale scopo (dettagli):

- Per i siti che usano un provider OAuth (Facebook, Twitter e Microsoft), si crea un'applicazione nel sito esterno. In questo modo le chiavi dell'applicazione che è necessario per richiamare la funzionalità di accesso per tali siti.
- Per i siti che usano un provider di OpenID (Google), non è necessario creare un'applicazione. Per tutti questi siti, è necessario un account per accedere e creare applicazioni per sviluppatori.

    > [!NOTE]
    > Le applicazioni Microsoft accettano solo un URL in tempo reale per un sito Web funzionante, non è possibile utilizzare un URL del sito Web locale per testare gli account di accesso.
- Modificare alcuni file nel sito Web per specificare il provider di autenticazione appropriato e per l'invio di un account di accesso al sito da usare.

Questo articolo fornisce istruzioni separate per le attività seguenti:

- [Abilitare gli account di accesso di Google](#To_enable_Google_logins)
- [Abilitare gli account di accesso di Facebook](#To_enable_Facebook_logins)
- [Abilitare gli account di accesso di Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Abilitare gli account di accesso di Google

1. Creare o aprire un sito di ASP.NET Web Pages è basato sul modello di sito Starter WebMatrix.
2. Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento dalla riga di codice seguente. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Account di accesso di Google test

1. Eseguire la *default. cshtml* page del sito e scegliere il **Accedi** pulsante.
2. Nel *account di accesso* nella pagina il **usi un altro servizio per l'accesso** , quindi scegliere il **Google** o **Yahoo** pulsante di invio. Questo esempio Usa l'account di accesso di Google. 

    La pagina web reindirizza la richiesta alla pagina di accesso di Google.

    ![Accesso Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Immettere le credenziali per un account Google esistente.
4. Se Google viene chiesto se si vuole consentire *Localhost* per usare le informazioni dall'account, fare clic su **Consenti**.

    Il codice Usa il token di Google per l'autenticazione dell'utente e quindi restituisce a questa pagina nel sito Web. Questa pagina consente agli utenti di associare i loro account di accesso di Google con un account esistente nel sito Web, o possono registrare un nuovo account nel sito per associare l'account di accesso esterno con.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Scegliere il **associare** pulsante. Il browser torna alla home page dell'applicazione.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Abilitare gli account di accesso di Facebook

1. Andare alla [sito degli sviluppatori Facebook](https://developers.facebook.com/apps) (accesso, se non è già stato effettuato).
2. Scegliere il **Create New App** pulsante e quindi seguire le istruzioni per assegnare un nome e creare la nuova applicazione.
3. Nella sezione **selezionare come si integrerà l'app con Facebook**, scegliere il **sito Web** sezione.
4. Immettere il **URL sito** campo con l'URL del sito (ad esempio, `http://www.example.com`). Il **Domain** campo è facoltativo; è possibile utilizzare questo per fornire l'autenticazione per un intero dominio (ad esempio *example.com*). 

    > [!NOTE]
    > Se si esegue un sito nel computer locale con un URL come `http://localhost:12345` (dove il numero è un numero di porta locale), è possibile aggiungere questo valore per il **URL del sito** campo per il test del sito. Tuttavia, ogni volta che il numero di porta delle modifiche al sito locale, dovrai aggiornare il **URL sito** campo dell'applicazione.
5. Scegliere il **Save Changes** pulsante.
6. Scegliere il **app** scheda nuovo e quindi visualizzare la pagina iniziale per l'applicazione.
7. Copia il **App ID** e **segreto App** i valori per l'applicazione e incollarli in un file di testo temporaneo. Si passerà questi valori per il provider di Facebook nel codice del sito Web.
8. Chiudere il sito per sviluppatori di Facebook.

A questo punto è apportare modifiche alle due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account di Facebook.

1. Creare o aprire un sito di ASP.NET Web Pages è basato sul modello di sito Starter WebMatrix.
2. Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Facebook. Il blocco di codice senza commenti è simile al seguente: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Copia il **App ID** valore dall'applicazione Facebook come valore del `appId` parametro (all'interno delle virgolette).
4. Copia **segreto App** valore dall'applicazione Facebook come il `appSecret` valore del parametro.
5. Salvare e chiudere il file.

### <a name="testing-facebook-login"></a>Test di accesso di Facebook

1. Esecuzione del sito *default. cshtml* pagina, quindi scegliere il **Login** pulsante.
2. Nel *account di accesso* nella pagina il **usi un altro servizio per l'accesso** keychains il **Facebook** icona. 

    La pagina web reindirizza la richiesta alla pagina di accesso di Facebook.

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Accedere a un account di Facebook. 

    Il codice Usa il token di Facebook per l'autenticazione dell'utente e quindi torna in una pagina in cui è possibile associare l'account di accesso di Facebook con account di accesso del sito. L'indirizzo di posta elettronica o nome utente viene compilato nel **messaggio di posta elettronica** campo nel form.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Scegliere il **associare** pulsante. 

    Il browser torna alla home page e si è connessi.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Abilitare gli account di accesso di Twitter

1. Selezionare il [sito agli sviluppatori di Twitter](https://dev.twitter.com/).
2. Scegliere il **creare un'App** collegamento e quindi accedere al sito.
3. Nel **creare un'applicazione** formano, compilare il **Name** e **descrizione** campi.
4. Nel **sito Web** immettere l'URL del sito (ad esempio, `http://www.example.com`). 

    > [!NOTE]
    > Se si sta verificando il sito in locale (tramite un URL come `http://localhost:12345`), Twitter potrebbero non accettare l'URL. Tuttavia, potrebbe essere possibile usare l'indirizzo IP di loopback locale (ad esempio `http://127.0.0.1:12345`). Ciò semplifica il processo di test dell'applicazione in locale. Tuttavia, ogni volta che viene modificato il numero di porta del sito locale, è necessario aggiornare il **sito Web** campo dell'applicazione.
5. Nel **URL di Callback** immettere un URL per la pagina nel sito Web che si desidera che gli utenti a cui tornare dopo la registrazione in Twitter. Ad esempio, per inviare gli utenti alla home page del sito Starter (che riconoscerà il relativo stato connesso), immettere lo stesso URL immesso nella **sito Web** campo.
6. Accettare le condizioni e scegliere il **Crea applicazione Twitter** pulsante.
7. Nel **applicazioni personali** pagina di destinazione scegliere l'applicazione creata.
8. Nel **informazioni dettagliate** scheda, scorrere verso il basso e scegliere il **Create My Access Token** pulsante.
9. Nel **dettagli** scheda, copiare la **Consumer Key** e **segreto Consumer** i valori per l'applicazione e incollarli in un file di testo temporaneo. Questi valori verranno passate al provider di Twitter nel codice del sito Web.
10. Uscire dal sito di Twitter.

A questo punto è apportare modifiche alle due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account Twitter.

1. Creare o aprire un sito di ASP.NET Web Pages è basato sul modello di sito Starter WebMatrix.
2. Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Twitter. Il blocco di codice senza commenti è simile alla seguente: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Copia il **Consumer Key** valore dell'applicazione Twitter come valore del `consumerKey` parametro (all'interno delle virgolette).
4. Copia il **Consumer Secret** valore dell'applicazione Twitter come valore del `consumerSecret` parametro.
5. Salvare e chiudere il file.

### <a name="testing-twitter-login"></a>Test di accesso a Twitter

1. Eseguire la *default. cshtml* page del sito e scegliere il **Login** pulsante.
2. Nel *account di accesso* nella pagina il **usi un altro servizio per l'accesso** keychains il **Twitter** icona. 

    La pagina web reindirizza la richiesta a una pagina di accesso di Twitter per l'applicazione creata.

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Accedere a un account Twitter.
4. Il codice Usa il token di Twitter per autenticare l'utente e quindi torna a una pagina in cui è possibile associare l'account di accesso con l'account del sito Web. L'indirizzo di posta elettronica o nome viene compilato nel **messaggio di posta elettronica** campo nel form.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Scegliere il **associare** pulsante. 

    Il browser torna alla home page e si è connessi.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


- [Personalizzazione del comportamento a livello di sito](https://go.microsoft.com/fwlink/?LinkId=202906)
- [L'aggiunta della protezione e l'appartenenza a un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202904)
