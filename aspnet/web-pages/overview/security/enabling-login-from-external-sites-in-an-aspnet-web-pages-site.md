---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Accesso tramite siti esterni in un Web ASP.NET di pagine del sito (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo articolo viene illustrato come accedere al sito pagine Web ASP.NET (Razor) utilizzando Google, Facebook, Twitter, Yahoo e altri siti, vale a dire il supporto di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530170"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Registrazione tramite siti esterni in un sito Web di ASP.NET di pagine (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come accedere al sito pagine Web ASP.NET (Razor) utilizzando Google, Facebook, Twitter, Yahoo e altri siti, vale a dire il supporto di OAuth e OpenID nel sito.
> 
> Illustra quanto segue:
> 
> - Come abilitare l'account di accesso da altri siti quando si utilizza il modello Starter Site di WebMatrix.
> 
> Si tratta della funzionalità ASP.NET introdotta nell'articolo:
> 
> - Il `OAuthWebSecurity` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 3

Include il supporto per ASP.NET Web Pages [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) provider. Utilizzando questi provider, è possibile consentire agli utenti l'accesso al sito utilizzando le credenziali esistenti da Facebook, Twitter, Microsoft e Google. Ad esempio, per accedere con un account Facebook, gli utenti possono solo scegliere un'icona di Facebook, che li reindirizza alla pagina di accesso di Facebook in cui inserire le informazioni utente. È quindi possibile associare l'account di accesso di Facebook con il proprio account nel sito. Una funzionalità avanzata correlata alle funzionalità di appartenenza a pagine Web è che gli utenti possono associare più account di accesso (inclusi gli accessi dai siti di social networking) con un singolo account nel sito Web.

La seguente immagine illustra la pagina account di accesso di **Starter Site** modello, in cui un utente può scegliere un'icona di Facebook, Twitter, Google o Microsoft per abilitare l'accesso con un account esterno:

![provider esterni](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

È possibile abilitare l'appartenenza OAuth e OpenID rimuovendo poche righe di codice il **Starter Site** modello. I metodi e proprietà consentono di lavorare con OAuth e i provider OpenID sono nella `WebMatrix.Security.OAuthWebSecurity` classe. Il **Starter Site** modello include un'infrastruttura completa dell'appartenenza, completo di una pagina di accesso, un database di appartenenza e tutto il codice necessario consentire agli utenti l'accesso al sito usando le credenziali locali o da un altro sito .

In questa sezione viene fornito un esempio di come consentire agli utenti di accedere da siti esterni a un sito che si basa sul **Starter Site** modello. Dopo aver creato un sito di partenza, tale scopo (dettagli):

- Per i siti che usano un provider OAuth (Facebook, Twitter e Microsoft), si crea un'applicazione nel sito esterno. In questo modo le chiavi dell'applicazione che è necessario per richiamare la funzionalità di accesso per tali siti.
- Per i siti che usano un provider OpenID (Google), non si dispone creare un'applicazione. Per tutti i siti, è necessario disporre di un account per accedere e creare applicazioni dello sviluppatore.

    > [!NOTE]
    > Applicazioni Microsoft accettano solo un URL in tempo reale per un sito Web di lavoro, non è possibile utilizzare un URL del sito Web locale per il test degli account di accesso.
- Modifica di alcuni file nel sito Web per specificare il provider di autenticazione appropriato e per l'invio di un account di accesso al sito che si desidera utilizzare.

Questo articolo fornisce istruzioni separate per le attività seguenti:

- [Abilitare gli account di accesso di Google](#To_enable_Google_logins)
- [Abilitare gli account di accesso di Facebook](#To_enable_Facebook_logins)
- [Abilitazione di account di accesso Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Abilitare gli account di accesso di Google

1. Creare o aprire un sito ASP.NET Web Pages basato sul modello Starter Site di WebMatrix.
2. Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento la riga di codice seguente. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Account di accesso Google di test

1. Eseguire il *cshtml* pagina del sito e scegliere il **Accedi** pulsante.
2. Nel *account di accesso* nella pagina di **utilizzare un altro servizio per l'accesso** sezione, scegliere il **Google** o **Yahoo** pulsante di invio. Questo esempio viene utilizzato l'account di accesso di Google. 

    La pagina web reindirizza la richiesta alla pagina di accesso di Google.

    ![Accesso di Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Immettere le credenziali per un account esistente di Google.
4. Se Google viene chiesto se si desidera consentire *Localhost* per utilizzare le informazioni dell'account, fare clic su **Consenti**.

    Il codice Usa il token di Google per autenticare l'utente e torna a questa pagina nel sito Web. Questa pagina consente agli utenti di associare i relativi account di accesso di Google a un account esistente nel sito Web o cui possono registrare un nuovo account del sito per associare l'account di accesso esterno con.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Scegliere il **associare** pulsante. Il browser restituisce alla pagina iniziale dell'applicazione.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Abilitare gli account di accesso di Facebook

1. Passare al [sito agli sviluppatori di Facebook](https://developers.facebook.com/apps) (accedere se non è già connesso).
2. Scegliere il **Create New App** pulsante e quindi seguire le istruzioni per assegnare un nome e creare la nuova applicazione.
3. Nella sezione **selezionare come permetterà di combinare l'app con Facebook**, scegliere il **sito Web** sezione.
4. Compilare il **URL del sito** campo con l'URL del sito (ad esempio, `http://www.example.com`). Il **dominio** campo è facoltativo; è possibile utilizzare questo metodo per fornire l'autenticazione per un intero dominio (ad esempio *example.com*). 

    > [!NOTE]
    > Se si esegue un sito nel computer locale con un URL come `http://localhost:12345` (dove il numero è un numero di porta locale), è possibile aggiungere questo valore per il **URL del sito** campo per il test del sito. Tuttavia, ogni volta che il numero di porta delle modifiche del sito locale, sarà necessario aggiornare il **URL del sito** campo dell'applicazione.
5. Scegliere il **Salva modifiche** pulsante.
6. Scegliere il **app** scheda nuovo e quindi visualizzare la pagina iniziale per l'applicazione.
7. Copia il **ID App** e **segreto dell'App** valori per l'applicazione e incollarli in un file di testo temporaneo. Questi valori verrà passato al provider di Facebook nel codice del sito Web.
8. Chiudere il sito per sviluppatori di Facebook.

Ora è apportare modifiche a due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account Facebook.

1. Creare o aprire un sito ASP.NET Web Pages basato sul modello Starter Site di WebMatrix.
2. Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Facebook. Il blocco di codice senza commenti è simile al seguente: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Copia il **ID App** valore dall'applicazione Facebook come il valore di `appId` parametro (racchiuso tra virgolette).
4. Copia **segreto dell'App** valore dall'applicazione Facebook come il `appSecret` valore del parametro.
5. Salvare e chiudere il file.

### <a name="testing-facebook-login"></a>Test di accesso di Facebook

1. Eseguire il sito *cshtml* pagina e selezionare il **accesso** pulsante.
2. Nel *account di accesso* nella pagina di **utilizzare un altro servizio per l'accesso** , scegliere il **Facebook** icona. 

    La pagina web reindirizza la richiesta alla pagina di accesso Facebook.

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Accedere a un account Facebook. 

    Il codice Usa il token di Facebook per autenticare l'utente e quindi restituisce a una pagina in cui è possibile associare l'account di accesso di Facebook con account di accesso del sito. L'indirizzo di posta elettronica o nome utente verrà inserita nel **posta elettronica** campo nel form.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Scegliere il **associare** pulsante. 

    Il browser restituisce alla home page e si è connessi.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Abilitazione di account di accesso Twitter

1. Individuare il [sito agli sviluppatori di Twitter](https://dev.twitter.com/).
2. Scegliere il **creare un'App** collegamento e quindi accedere al sito.
3. Nel **creare un'applicazione** modulo, compilare il **nome** e **descrizione** campi.
4. Nel **sito Web** immettere l'URL del sito (ad esempio, `http://www.example.com`). 

    > [!NOTE]
    > Se si sta testando il sito locale (tramite un URL come `http://localhost:12345`), Twitter potrebbero non accettare l'URL. Tuttavia, potrebbe essere in grado di utilizzare l'indirizzo IP di loopback locale (ad esempio `http://127.0.0.1:12345`). Questa operazione semplifica il processo di test dell'applicazione in locale. Tuttavia, ogni volta che viene modificato il numero di porta del sito locale, è necessario aggiornare il **sito Web** campo dell'applicazione.
5. Nel **URL Callback** immettere un URL per la pagina nel sito Web che si desidera che gli utenti restituire dopo la registrazione in Twitter. Ad esempio, per inviare gli utenti alla home page del sito Starter (che verrà riconosciuto loro stato di accesso), immettere lo stesso URL immesso nel **sito Web** campo.
6. Accettare le condizioni e scegliere il **creare un'applicazione Twitter** pulsante.
7. Nel **applicazioni personali** pagina di destinazione scegliere l'applicazione creata.
8. Nel **dettagli** scheda, scorrere verso il basso e scegliere il **creare risorse del Token di accesso** pulsante.
9. Nel **dettagli** scheda, copiare il **chiave Consumer** e **segreto del cliente** i valori per l'applicazione e incollarli in un file di testo temporaneo. Questi valori verrà passato al provider di Twitter nel codice del sito Web.
10. Chiudere il sito di Twitter.

Ora è apportare modifiche a due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account Twitter.

1. Creare o aprire un sito ASP.NET Web Pages basato sul modello Starter Site di WebMatrix.
2. Aprire il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Twitter. Il blocco di codice senza commenti è simile al seguente: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Copia il **chiave Consumer** valore dall'applicazione Twitter come valore della `consumerKey` parametro (racchiuso tra virgolette).
4. Copia il **segreto del cliente** l'applicazione di Twitter come valore del valore di `consumerSecret` parametro.
5. Salvare e chiudere il file.

### <a name="testing-twitter-login"></a>Test di account di accesso Twitter

1. Eseguire il *cshtml* pagina del sito e scegliere il **accesso** pulsante.
2. Nel *account di accesso* nella pagina di **utilizzare un altro servizio per l'accesso** , scegliere il **Twitter** icona. 

    La pagina web reindirizza la richiesta a una pagina di accesso Twitter per l'applicazione creata.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Accedere a un account Twitter.
4. Il codice Usa il token di Twitter per autenticare l'utente e quindi torna a una pagina in cui è possibile associare l'account di accesso con l'account del sito Web. Il nome o indirizzo di posta verrà inserita nel **posta elettronica** campo nel form.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Scegliere il **associare** pulsante. 

    Il browser restituisce alla home page e si è connessi.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


- [Personalizzazione del comportamento a livello di sito](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Aggiunta della protezione e l'appartenenza a un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202904)
