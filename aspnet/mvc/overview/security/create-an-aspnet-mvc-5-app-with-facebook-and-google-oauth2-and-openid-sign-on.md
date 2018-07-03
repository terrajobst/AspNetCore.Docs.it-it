---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: MVC 5 di creare App con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come compilare un'applicazione web ASP.NET MVC 5 che consente agli utenti di accedere tramite OAuth 2.0 con le credenziali di un autenti esterni...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 8a9528f76b0166175f950543b4b8a7250bdf5100
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388766"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Creare un'App ASP.NET MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Questa esercitazione illustra come compilare un'applicazione web ASP.NET MVC 5 che consente agli utenti di accedere tramite [OAuth 2.0](http://oauth.net/2/) con le credenziali di un provider di autenticazione esterni, ad esempio Facebook, Twitter, LinkedIn, Microsoft o Google. Per semplicità, questa esercitazione è incentrata sull'uso di credenziali da Facebook e Google.
> 
> Abilitazione di queste credenziali nei siti web offre un vantaggio significativo perché milioni di utenti hanno già account con questi provider esterni. Questi utenti potrebbero essere più inclini a effettuare l'iscrizione per il sito se non dispongono creare e tenere presente un nuovo insieme di credenziali.
> 
> Vedere anche [app ASP.NET MVC 5 con SMS e posta elettronica l'autenticazione a due fattori](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> L'esercitazione illustra anche come aggiungere dati del profilo dell'utente e come usare l'API di appartenenza per aggiungere i ruoli. Questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (me, seguire in Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Introduzione

Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva. Per informazioni su Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! e altro ancora, vedere questo [progetto di esempio](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> È necessario installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva per usare Google OAuth 2 e per eseguire il debug in locale senza eventuali avvisi SSL.


Fare clic su **nuovo progetto** dalle **avviare** pagina oppure è possibile usare il menu e selezionare **File**e quindi **nuovo progetto**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Creazione della prima applicazione

Fare clic su **nuovo progetto**, quindi selezionare **Visual c#** a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET**. Denominare il progetto "MvcAuth" e quindi fare clic su **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo, fare clic su **MVC**. Se l'autenticazione non è **account utente individuali**, fare clic sui **Modifica autenticazione** e selezionare **account utente individuali**. Controllando **ospita nel cloud**, l'app sarà molto facile da ospitare in Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Se è stato selezionato **ospita nel cloud**, completare la finestra di dialogo Configura.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Usare NuGet per aggiornare il middleware OWIN più recente

Usare Gestione pacchetti NuGet per aggiornare il [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Selezionare **aggiornamenti** nel menu a sinistra. È possibile fare clic sui **Aggiorna tutte** pulsante oppure è possibile cercare solo i pacchetti OWIN (illustrati nella figura seguente):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Nell'immagine seguente, vengono visualizzati solo i pacchetti OWIN:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Dal pacchetto Manager Console (console di gestione pacchetti), è possibile immettere il `Update-Package` comando, che aggiornerà tutti i pacchetti.

Premere **F5** oppure **CTRL+F5** per eseguire l'applicazione. Nell'immagine seguente, il numero di porta è 1234. Quando si esegue l'applicazione, si noterà un numero di porta diverso.

A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sull'icona di spostamento per visualizzare il **Home**, **sulle**, **contatto**, **registrare**e **accedere** collegamenti.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Configurare SSL nel progetto

Per connettersi ai provider di autenticazione, ad esempio Google e Facebook, è necessario configurare IIS Express per utilizzare SSL. È importante continuare a usare SSL dopo l'accesso e non rilasciare nuovamente a HTTP, il cookie di accesso è semplicemente come segreto come nome utente e password sia senza utilizzare SSL inviato in testo non crittografato attraverso la rete. Inoltre, è stato così già compiuto il tempo necessario per eseguire l'handshake e proteggere il canale, ovvero la maggior parte delle caratteristiche che rendono più lento rispetto a HTTP a HTTPS, prima dell'esecuzione della pipeline MVC, pertanto, reindirizzando a HTTP dopo che si è connessi non rendere la richiesta corrente o futuro richieste è molto più veloce.

1. Nelle **Esplora soluzioni**, fare clic sui **MvcAuth** progetto.
2. Premere il tasto F4 per visualizzare le proprietà del progetto. In alternativa, scegliere il **View** menu è possibile selezionare **finestra proprietà**.
3. Change **SSL abilitato** su True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copiare l'URL SSL (che sarà `https://localhost:44300/` a meno che non creati altri progetti SSL).
5. Nella **Esplora soluzioni**, fare clic il **MvcAuth** del progetto e selezionare **proprietà**.
6. Selezionare il **Web** scheda e quindi incollare l'URL SSL nel **Url progetto** casella. Salvare il file (CTRL + S). È necessario l'URL per configurare le app di autenticazione di Facebook e Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Aggiungere il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) dell'attributo di `Home` controller in modo da richiedere tutte le richieste devono utilizzare HTTPS. Un approccio più sicuro consiste nell'aggiungere il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtro all'applicazione. Vedere la sezione &quot;proteggere l'applicazione con SSL e l'attributo autorizzare&quot; nella mio tutoral [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio App di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Di seguito è riportata una parte del controller Home.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Premere CTRL+F5 per eseguire l'applicazione. Se il certificato è stato installato in precedenza, è possibile ignorare il resto di questa sezione e passare a [creazione di un'app Google per OAuth 2 e ci si connette l'app per il progetto](#goog), in caso contrario, seguire le istruzioni per considerare attendibile autofirmato certificato IIS Express è generato.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Leggere il **avviso di sicurezza** finestra di dialogo e quindi fare clic su **Yes** se si desidera installare il certificato che rappresenta localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. Internet Explorer mostra il *Home* pagina e nessun avviso SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome, inoltre, accetta il certificato e visualizzerà contenuto HTTPS senza alcun avviso. Firefox Usa un proprio archivio certificati, in modo che verrà visualizzato un avviso. Per l'applicazione è possibile scegliere l'opzione **dichiaro di aver compreso i rischi**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Creazione di un'app Google per OAuth 2 e ci si connette l'app al progetto

> [!WARNING]
> Per istruzioni di Google OAuth corrente, vedere [configurazione di Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Passare il [Google Developers Console](https://console.developers.google.com/).
2. Se è stato ancora creato un progetto prima di, selezionare **credenziali** nella scheda a sinistra e quindi selezionare **crea**.
3. Nella scheda di sinistra, fare clic su **credenziali**.
4. Fare clic su **creare le credenziali** quindi **ID client OAuth**. 

    1. Nel **Create Client ID** finestra di dialogo, mantenere il valore predefinito **applicazione Web** per il tipo di applicazione.
    2. Impostare il **JavaScript autorizzate** origini per l'URL SSL descritti in precedenza (`https://localhost:44300/` a meno che non creati altri progetti SSL)
    3. Impostare il **Authorized redirect URI** per:  
         `https://localhost:44300/signin-google`
5. Scegliere la voce di menu schermata consenso OAuth, quindi impostare il nome di prodotto e indirizzo di posta elettronica. Dopo aver completato il modulo di fare clic su **salvare**.
6. Scegliere la voce di menu della libreria, cercare **Google + API**, fare clic su di esso, quindi premere Enable.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   L'immagine seguente mostra le API abilitate.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Da Gestione API Google API, visitare il **credenziali** pressione di tab per ottenere il **ID Client**. Download per salvare un file JSON con i segreti dell'applicazione. Copiare e incollare il **ClientId** e **ClientSecret** nel `UseGoogleAuthentication` trovato nel metodo il *Startup.Auth.cs* del file nel *App_Start* cartella. Il **ClientId** e **ClientSecret** valori riportati di seguito sono esempi e non funzionano.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Security - store mai i dati sensibili nel codice sorgente. L'account e le credenziali vengono aggiunti al codice precedente per mantenere semplice l'esempio. Visualizzare [procedure consigliate per la distribuzione delle password e altri dati sensibili in ASP.NET e servizio App di Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Premere **CTRL+F5** per compilare ed eseguire l'applicazione. Scegliere il **Accedi** collegamento.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. Sotto **usare un altro servizio per accedere**, fare clic su **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Se non è disponibile uno dei passaggi precedenti, si otterrà un errore HTTP 401. Eseguire nuovamente la procedura precedente. Se non è disponibile un'impostazione obbligatoria (ad esempio **nome del prodotto**), aggiungere l'elemento manca e salvare; può richiedere alcuni minuti per l'autenticazione funzioni.
10. Si verrà reindirizzati al sito di Google in cui vengono immesse le credenziali.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Dopo aver immesso le credenziali, verrà richiesto di concedere le autorizzazioni per l'applicazione web che appena creato:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Fare clic su **accettano**. Verrà reindirizzati al **registrare** pagina dell'applicazione MvcAuth in cui è possibile registrare l'account Google. È possibile scegliere di modificare il nome di registrazione di posta elettronica locale usato per l'account Gmail, ma in genere si vuole mantenere l'alias di posta elettronica predefinito (vale a dire quello usato per l'autenticazione). Fare clic su **Register**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Creazione dell'app in Facebook e ci si connette l'app al progetto

> [!WARNING]
> Per istruzioni di autenticazione Facebook OAuth2 corrente, vedere [l'autenticazione di configurazione di Facebook](/aspnet/core/security/authentication/social/facebook-logins)

Per l'autenticazione OAuth2 di Facebook, è necessario copiare nel progetto alcune impostazioni da un'applicazione che si crea in Facebook.

1. Nel browser, passare a [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) ed eseguire l'accesso, immettere le credenziali di Facebook.
2. Se già non sono registrati come sviluppatore Facebook, fare clic su **registrarsi come sviluppatore** e seguire le istruzioni per registrare.
3. Nel **Apps** scheda, fare clic su **Crea nuova App**.

    ![Crea nuova app](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. Immettere un **nome dell'App** e **categoria**, quindi fare clic su **crea App**.

    Il <strong>App Namespace</strong> è la parte dell'URL che l'App userà per accedere all'applicazione di Facebook per l'autenticazione (ad esempio, https\://apps.facebook.com/{App Namespace}). Se non si specifica un <strong>App Namespace</strong>, il <strong>App ID</strong> verrà usato per l'URL. Il <strong>App ID</strong> è un numero lungo generati dal sistema che verrà visualizzato nel passaggio successivo.

    ![Finestra di dialogo Nuova App crea](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. Inviare il controllo di sicurezza standard.

    ![Controllo di sicurezza](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. Selezionare **le impostazioni** per la barra di menu a sinistra![ Barra dei menu per gli sviluppatori di Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. Nel **base** sezione Impostazioni della pagina selezionare **Aggiungi piattaforma** per specificare che si sta aggiungendo un'applicazione del sito Web. ![Impostazioni di base](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. Selezionare **sito Web** tra le opzioni di piattaforma.  
  
    ![Scelte di piattaforma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. Prendere nota del **App ID** e il **segreto dell'App** in modo che sia possibile aggiungere entrambi nell'applicazione MVC più avanti in questa esercitazione. Inoltre, aggiungere l'URL del sito (`https://localhost:44300/`) per testare l'applicazione MVC. Aggiungere anche un **Contact Email**. Quindi, selezionare **Save Changes**.   

    ![Pagina dei dettagli di un'applicazione di base](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > Si noti che solo sarà in grado di eseguire l'autenticazione usando l'alias di posta elettronica che è stato registrato. Non sarà in grado di registrare altri utenti e account di test. È possibile concedere altro accesso di account di Facebook all'applicazione su Facebook **ruolo di sviluppatore** scheda.
10. In Visual Studio, aprire *App\_Start\Startup.Auth.cs*.
11. Copiare e incollare il **AppId** e **segreto dell'App** nel `UseFacebookAuthentication` (metodo). Il **AppId** e **segreto App** valori riportati di seguito sono esempi e non funzionerà.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. Fare clic su **salvare le modifiche**.
13. Premere **CTRL+F5** per eseguire l'applicazione.


Selezionare **Accedi** per visualizzare la pagina di accesso. Fare clic su **Facebook** sotto **usi un altro servizio per l'accesso.**

Immettere le credenziali di Facebook.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

Verrà richiesto di concedere l'autorizzazione per l'applicazione accedere ai profili pubblici e liste di amici.

![Dettagli dell'app per Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

Si è ora connessi. È ora possibile registrare questo account con l'applicazione.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

Quando si registra, viene aggiunta una voce per il *utenti* tabella del database delle appartenenze.

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Esaminare i dati di appartenenza

Nel **View** menu, fare clic su **Esplora Server**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Espandere **DefaultConnection (MvcAuth)**, espandere **tabelle**, fare clic destro **AspNetUsers** e fare clic su **Mostra dati tabella**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dati della tabella aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Aggiunta di dati di profilo per la classe utente

In questa sezione si aggiungeranno data di nascita e città natale ai dati utente durante la registrazione, come illustrato nell'immagine seguente.

![reg con città natale e Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Aprire il *Models\IdentityModels.cs* file e aggiungere le proprietà di Town (città) casa e data di nascita:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Aprire il *Models\AccountViewModels.cs* file e il set di proprietà Town (città) data e l'home page in nascita `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Aprire il *Controllers\AccountController.cs* del file e aggiungere il codice per town home page e data di nascita nel `ExternalLoginConfirmation` metodo di azione come mostrato:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Aggiungere la data di nascita e città natale per il *Views\Account\ExternalLoginConfirmation.cshtml* file:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

In modo che anche in questo caso è possibile registrare l'account di Facebook con l'applicazione e verificare che la nuova data di nascita e le informazioni sul profilo città Natale, è possibile aggiungere, eliminare il database di appartenenza.

Dal **Esplora soluzioni**, fare clic sui **Mostra tutti i file** icona, quindi fare clic destro *Add\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;mdf* e fare clic su **eliminare**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Dal **degli strumenti** menu, fare clic su **Gestione pacchetti NuGet**, quindi fare clic su **Package Manager Console** (PMC). Immettere i comandi seguenti nella console di gestione pacchetti.

1. Enable-Migrations
2. Add-Migration Init
3. Update-Database

Eseguire l'applicazione e usare FaceBook e Google per accedere e registrare alcuni utenti.

## <a name="examine-the-membership-data"></a>Esaminare i dati di appartenenza

Nel **View** menu, fare clic su **Esplora Server**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Fare clic destro **AspNetUsers** e fare clic su **Mostra dati tabella**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

Il `HomeTown` e `BirthDate` vengono visualizzati i campi seguenti.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>La disconnessione all'App e accedere con un altro Account

Se si accede all'App con Facebook e quindi effettuare la disconnessione e provare ad accedere nuovamente con un altro account di Facebook (usando lo stesso browser), verrà immediatamente registrato all'account di Facebook precedente che è stato usato. Per usare un altro account, è necessario passare a Facebook ed effettuare la disconnessione in Facebook. La stessa regola si applica a qualsiasi altro 3rd provider di autenticazione terze parti. In alternativa, è possibile accedere con un altro account usando un browser diverso.

## <a name="next-steps"></a>Passaggi successivi

Visualizzare [Introducing i provider di sicurezza Yahoo e OAuth di LinkedIn per OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) da Jerrie Pelser per le istruzioni di Yahoo e LinkedIn. Vedere del Jerrie insomma, pulsanti di accesso per ASP.NET MVC 5 ottenere i pulsanti per abilitare l'accesso social.

Seguire l'esercitazione [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio App di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), che continua in questa esercitazione e viene illustrato quanto segue:

1. Come distribuire l'app di Azure.
2. Come proteggere l'app con i ruoli si.
3. Come proteggere l'app con il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) e [autorizza](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtri.
4. Come usare l'API di appartenenza per aggiungere utenti e ruoli.

Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare. È anche possibile richiedere nuovi argomenti in [Mostra Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). È anche possibile richiedere e votare nuove funzionalità per essere aggiunto ad ASP.NET. Ad esempio, è possibile votare per uno strumento a [creare e gestire utenti e ruoli.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Per una valida spiegazione del funzionamento di servizi di autenticazione esterno di ASP.NET, vedere di Robert McMurray [servizi di autenticazione esterni](https://asp.net/web-api/overview/security/external-authentication-services). L'articolo di Robert anche descrive in dettaglio nell'abilitazione dell'autenticazione di Microsoft e Twitter. Tom Dykstra Kothari [esercitazione su EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) descrive l'uso con Entity Framework.
