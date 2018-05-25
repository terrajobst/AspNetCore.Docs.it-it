---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Creare MVC 5 App con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#) | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione viene illustrato come compilare un'applicazione web ASP.NET MVC 5 che consente agli utenti di accedere con OAuth 2.0 con le credenziali di un authenti esterni...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: c289c209b50f0c2c1f2d8b15a3aedeaebf671d0b
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/21/2018
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Crea un'applicazione ASP.NET MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione viene illustrato come compilare un'applicazione web ASP.NET MVC 5 che consente agli utenti di accedere utilizzando [OAuth 2.0](http://oauth.net/2/) con le credenziali di un provider di autenticazione esterna, ad esempio Facebook, Twitter, LinkedIn, Microsoft o Google. Per semplicità, in questa esercitazione è incentrata sull'utilizzo di credenziali Facebook e Google.
> 
> Abilitazione di queste credenziali in siti web offre un vantaggio significativo perché milioni di utenti dispongono già di account con tali provider esterni. Questi utenti potrebbero essere più inclini a effettuare l'iscrizione per il sito se non dispone di creare e ricordare un nuovo insieme di credenziali.
> 
> Vedere anche [app ASP.NET MVC 5 con SMS e l'autenticazione a due fattori di posta elettronica](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Anche l'esercitazione viene illustrato come aggiungere dati di profilo per l'utente e come utilizzare l'API di appartenenza per aggiungere ruoli. In questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (me seguire su Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Introduzione

Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installazione di Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva. Per informazioni su Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, il flusso, Stack di Exchange, Tripit, Twitch, Twitter, Yahoo! e altro ancora, vedere questo [progetto di esempio](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> È necessario installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva per usare Google OAuth 2 e per eseguire il debug in locale senza avvisi SSL.


Fare clic su **nuovo progetto** dal **avviare** pagina oppure è possibile utilizzare il menu e selezionare **File**e quindi **nuovo progetto**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Creazione di un'applicazione

Fare clic su **nuovo progetto**, quindi selezionare **Visual c#** a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET**. Denominare il progetto "MvcAuth" e quindi fare clic su **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo, fare clic su **MVC**. Se l'autenticazione non **singoli account utente di**, fare clic su di **Modifica autenticazione** e selezionare **singoli account utente di**. Controllando **Host nel cloud**, l'app sarà molto semplice ospitare in Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Se si seleziona **Host nel cloud**, completare la finestra di dialogo Configura.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Utilizzare NuGet per aggiornare il middleware OWIN più recente

Usare Gestione pacchetti NuGet per aggiornare il [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Selezionare **aggiornamenti** nel menu a sinistra. È possibile fare clic su di **Aggiorna tutto** pulsante oppure è possibile cercare solo i pacchetti OWIN (illustrati nella figura seguente):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Nell'immagine seguente, vengono visualizzati solo i pacchetti OWIN:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Dal Package Manager Console (PMC), è possibile immettere il `Update-Package` comando, che aggiornerà tutti i pacchetti.

Premere **F5** o **Ctrl + F5** per eseguire l'applicazione. Nell'immagine seguente, il numero di porta è 1234. Quando si esegue l'applicazione, verrà visualizzato un numero di porta diverso.

A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sull'icona di spostamento per visualizzare il **Home**, **su**, **contatto**, **registrare**e **Accedi** collegamenti.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Impostazione di SSL nel progetto

Per connettersi al provider di autenticazione come Google e Facebook, è necessario configurare IIS Express per utilizzare SSL. È importante mantenere l'utilizzo di SSL dopo l'accesso e drop non HTTP, il cookie di accesso come segreto come nome utente e password e senza l'utilizzo di SSL che si sta inviando il messaggio in testo non crittografato attraverso la rete. Inoltre, è già stata acquisita il tempo necessario per eseguire l'handshake e proteggere il canale (ovvero la maggior parte di ciò che rende più lenta rispetto all'HTTP a HTTPS) prima dell'esecuzione della pipeline MVC, pertanto reindirizzamento su HTTP dopo che si è connessi non effettuare la richiesta corrente o futuro richieste è molto più veloce.

1. In **Esplora**, fare clic su di **MvcAuth** progetto.
2. Premere il tasto F4 per visualizzare le proprietà del progetto. In alternativa, dal **vista** menu è possibile selezionare **finestra proprietà**.
3. Modifica **SSL abilitato** su True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copiare l'URL SSL (che sarà `https://localhost:44300/` a meno che non è stato creato altri progetti SSL).
5. In **Esplora soluzioni**, fare clic destro la **MvcAuth** progetto e selezionare **proprietà**.
6. Selezionare il **Web** scheda e quindi incollare l'URL SSL nella **Url progetto** casella. Salvare il file (CTRL + S). È necessario l'URL per configurare le app di autenticazione di Facebook e Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Aggiungere il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attributo la `Home` controller in modo da richiedere tutte le richieste devono utilizzare HTTPS. Un approccio più sicuro consiste nell'aggiungere il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtro all'applicazione. Vedere la sezione &quot;proteggere l'applicazione con SSL e l'attributo autorizzare&quot; in my tutoral [creare un'applicazione MVC ASP.NET con autenticazione e il database di SQL Server e distribuire in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Di seguito è riportata una parte del controller Home.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Premere CTRL+F5 per eseguire l'applicazione. Se il certificato è stato installato in precedenza, è possibile ignorare il resto di questa sezione e passare a [creando un'app di Google per OAuth 2 e l'applicazione di connessione al progetto](#goog), in caso contrario, seguire le istruzioni per considerare attendibile autofirmato certificato che ha generato IIS Express.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Lettura di **avviso di sicurezza** finestra di dialogo e quindi fare clic su **Sì** se si desidera installare il certificato che rappresenta localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. Internet Explorer mostra il *Home* pagina e nessun avviso SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome, inoltre, accetta il certificato e visualizzerà contenuto HTTPS senza un messaggio di avviso. Firefox Usa il proprio archivio certificati, quindi verrà visualizzato un avviso. Per l'applicazione è possibile scegliere l'opzione **comprenderne i rischi**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Creazione di un'app di Google per OAuth 2 e l'applicazione di connessione al progetto

> [!WARNING]
> Per istruzioni di Google OAuth corrente, vedere [l'autenticazione di Google di configurazione in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Passare il [Google Developers Console](https://console.developers.google.com/).
2. Se è ancora stato creato un progetto, selezionare **credenziali** nel riquadro a sinistra e quindi selezionare **crea**.
3. Nella scheda di sinistra, fare clic su **credenziali**.
4. Fare clic su **creare credenziali** quindi **ID client OAuth**. 

    1. Nel **Create Client ID** finestra di dialogo, mantenere il valore predefinito **applicazione Web** per il tipo di applicazione.
    2. Impostare il **autorizzato JavaScript** le origini per l'URL SSL utilizzato in precedenza (`https://localhost:44300/` a meno che non è stato creato altri progetti SSL)
    3. Impostare il **URI di reindirizzamento autorizzato** per:  
         `https://localhost:44300/signin-google`
5. Scegliere la voce di menu consenso OAuth schermata, quindi impostare il nome di prodotto e l'indirizzo di posta elettronica. Dopo aver completato il modulo di fare clic su **salvare**.
6. Scegliere la voce di menu della raccolta, cercare **Google + API**, fare clic su di esso, quindi premere attiva.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   L'immagine seguente mostra le API abilitate.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Da Gestione API Google API, visitare il **credenziali** tab per ottenere il **ID Client**. Download per salvare un file JSON con segreti dell'applicazione. Copiare e incollare il **ClientId** e **ClientSecret** nel `UseGoogleAuthentication` trovato nel metodo il *Startup.Auth.cs* file nel *App_Start* cartella. Il **ClientId** e **ClientSecret** valori riportati di seguito sono riportati alcuni esempi e non funzionano.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Sicurezza - archivio mai i dati sensibili nel codice sorgente. L'account e le credenziali vengono aggiunti al codice precedente per semplificare il codice di esempio. Vedere [procedure consigliate per la distribuzione delle password e altri dati sensibili per ASP.NET e servizio App di Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Premere **CTRL + F5** per compilare ed eseguire l'applicazione. Fare clic su di **Accedi** collegamento.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. In **utilizzare un altro servizio per l'accesso**, fare clic su **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Se non è disponibile uno dei passaggi precedenti, si otterrà un errore HTTP 401. Controlla di nuovo i passaggi precedenti. Se non è disponibile un'impostazione obbligatoria (ad esempio **nome del prodotto**), aggiungere l'elemento mancante e salvare; può richiedere alcuni minuti per l'uso dell'autenticazione.
10. Si verrà reindirizzati al sito di Google dove inserire le credenziali.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Dopo aver immesso le credenziali, verrà richiesto di concedere le autorizzazioni per l'applicazione web che appena creato:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Fare clic su **accettare**. Si verrà reindirizzati al **registrare** pagina dell'applicazione MvcAuth in cui è possibile registrare l'account Google. È possibile scegliere di modificare il nome di registrazione di posta elettronica locale utilizzato per l'account Gmail, ma in genere si desidera mantenere l'alias di posta elettronica predefinito (vale a dire quello utilizzato per l'autenticazione). Fare clic su **Register**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Creazione di app in Facebook e l'applicazione di connessione al progetto

> [!WARNING]
> Per istruzioni di autenticazione Facebook OAuth2 corrente, vedere [l'autenticazione di configurazione di Facebook](/aspnet/core/security/authentication/social/facebook-logins)

Per l'autenticazione Facebook OAuth2, è necessario copiare nel progetto alcune impostazioni da un'applicazione creata con Facebook.

1. Nel browser, passare a [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) e accedere immettendo le credenziali di Facebook.
2. Se non sono già registrati come uno sviluppatore di Facebook, fare clic su **registrare gli sviluppatori** e seguire le istruzioni per registrare.
3. Nel **app** scheda, fare clic su **Create New App**.

    ![Crea nuova app](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. Immettere un **nome App** e **categoria**, quindi fare clic su **creare App**.

    Deve essere univoco in Facebook. Il <strong>App Namespace</strong> è la parte dell'URL utilizzato per accedere all'applicazione di Facebook per l'autenticazione dell'App (ad esempio, https://apps.facebook.com/{App Namespace}). Se non si specifica un <strong>Namespace di App</strong>, <strong>ID App</strong> verrà utilizzato per l'URL. Il <strong>ID App</strong> è un numero lungo generato dal sistema che verrà visualizzato nel passaggio successivo.

    ![Creare una finestra di dialogo Nuova App](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. Inviare il controllo di sicurezza standard.

    ![Controllo di sicurezza](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. Selezionare **impostazioni** per la barra dei menu a sinistra.![ Barra dei menu per gli sviluppatori di Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. Nel **base** sezione delle impostazioni della pagina selezionare **aggiungere piattaforma** per specificare che si sta aggiungendo un'applicazione del sito Web. ![Impostazioni di base](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. Selezionare **sito Web** tra le opzioni di piattaforma.  
  
    ![Scelte di piattaforma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. Annotare il **ID App** e **segreto dell'applicazione** in modo che è possibile aggiungere entrambe nell'applicazione MVC più avanti in questa esercitazione. Inoltre, aggiungere l'URL del sito (`https://localhost:44300/`) per testare l'applicazione MVC. Inoltre, aggiungere un **Contact Email**. Selezionare quindi **Salva modifiche**.   

    ![Pagina dei dettagli di un'applicazione di base](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > Si noti che solo sarà in grado di eseguire l'autenticazione utilizzando l'alias di posta elettronica che è stato registrato. Non sarà in grado di registrare altri utenti e account di prova. È possibile concedere l'accesso agli account altri Facebook per l'applicazione su Facebook **ruolo di sviluppatore** scheda.
10. In Visual Studio, aprire *App\_Start\Startup.Auth.cs*.
11. Copiare e incollare il **AppId** e **segreto dell'App** nel `UseFacebookAuthentication` metodo. Il **AppId** e **segreto dell'App** valori riportati di seguito sono riportati alcuni esempi e non funzionerà.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. Fare clic su **salvare modifiche**.
13. Premere **CTRL + F5** per eseguire l'applicazione.


Selezionare **Accedi** per visualizzare la pagina di accesso. Fare clic su **Facebook** in **utilizzare un altro servizio per l'accesso.**

Immettere le credenziali di Facebook.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

Verrà richiesto di concedere l'autorizzazione per l'applicazione accedere al profilo pubblico e un elenco di tipo friend.

![Dettagli dell'app Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

Ora connessi. È ora possibile registrare questo account con l'applicazione.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

Quando si registra, viene aggiunta una voce per il *utenti* tabella del database delle appartenenze.

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Esaminare i dati di appartenenza

Nel **vista** menu, fare clic su **Esplora Server**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Espandere **DefaultConnection (MvcAuth)**, espandere **tabelle**, fare clic destro **AspNetUsers** e fare clic su **Mostra dati tabella**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dati della tabella aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Aggiunta di dati di profilo per la classe utente

In questa sezione si aggiungerà la data di nascita e città natale ai dati utente durante la registrazione, come illustrato nella figura seguente.

![reg con città natale e Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Aprire il *Models\IdentityModels.cs* file e aggiungere le proprietà di città home page e data di nascita:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Aprire il *Models\AccountViewModels.cs* file e il set di date e home proprietà città di nascita `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Aprire il *Controllers\AccountController.cs* file e aggiungere il codice per città home page e data di nascita nel `ExternalLoginConfirmation` metodo di azione, come illustrato:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Aggiungere la data di nascita e città natale per il *Views\Account\ExternalLoginConfirmation.cshtml* file:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Eliminare il database di appartenenza che consente di registrare l'account di Facebook con l'applicazione e verificare che è possibile aggiungere la nuova data di nascita e le informazioni sul profilo città natale nuovamente.

Da **Esplora**, fare clic su di **Mostra tutti i file** icona, quindi fare clic destro *Aggiungi\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;fileconestensionemdf* e fare clic su **eliminare**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Dal **strumenti** menu, fare clic su **Gestione pacchetti NuGet**, quindi fare clic su **Package Manager Console** (PMC). Immettere i comandi seguenti in PMC.

1. Enable-Migrations
2. Migrazione aggiungere Init
3. Update-Database

Eseguire l'applicazione e utilizzare FaceBook e Google per accedere e registrare alcuni utenti.

## <a name="examine-the-membership-data"></a>Esaminare i dati di appartenenza

Nel **vista** menu, fare clic su **Esplora Server**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Fare clic destro **AspNetUsers** e fare clic su **Mostra dati tabella**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

Il `HomeTown` e `BirthDate` campi sono illustrati di seguito.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>La disconnessione dell'App e accedere con un altro Account

Se si accede all'App con Facebook e quindi disconnettersi e riprovare ad accedere nuovamente con un account Facebook diverso (utilizzando lo stesso browser), verrà immediatamente eseguito per l'account Facebook precedente che è stato utilizzato. Per utilizzare un altro account, è necessario passare a Facebook e la disconnessione a Facebook. La stessa regola si applica a qualsiasi altro 3rd provider di autenticazione entità. In alternativa, è possibile accedere con un altro account usando un browser diverso.

## <a name="next-steps"></a>Passaggi successivi

Vedere [introdurre i provider di sicurezza Yahoo e OAuth di LinkedIn per OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) da Jerrie Pelser per le istruzioni di Yahoo e LinkedIn. Vedere del Jerrie piuttosto pulsanti di accesso social per ASP.NET MVC 5 ottenere l'account di accesso social abilitano i pulsanti.

Seguire l'esercitazione [creare un'applicazione MVC ASP.NET con autenticazione e il database di SQL Server e distribuire in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), che continua a questa esercitazione e Mostra le operazioni seguenti:

1. Come distribuire l'app in Azure.
2. Come proteggere app con i ruoli.
3. Come proteggere le app con il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) e [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtri.
4. Come utilizzare l'API di appartenenza per aggiungere utenti e ruoli.

Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione ed è stato possibile apportare miglioramenti. È inoltre possibile richiedere nuovi argomenti in [Mostra Me come con codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). È anche possibile chiedere e votare nuove caratteristiche per essere aggiunto ad ASP.NET. Ad esempio, è possibile votare per uno strumento per [creare e gestire utenti e ruoli.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Per una spiegazione chiara del funzionamento di servizi di autenticazione esterno di ASP.NET, vedere di Robert McMurray [servizi di autenticazione esterni](https://asp.net/web-api/overview/security/external-authentication-services). Articolo Paolo inseriti anche nel dettaglio abilitare l'autenticazione di Microsoft e Twitter. Tom Dykstra dell'eccellente [esercitazione EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) viene illustrato l'utilizzo con Entity Framework.
