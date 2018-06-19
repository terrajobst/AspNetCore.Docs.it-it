---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Account di conferma e il recupero della Password con identità di ASP.NET (c#) | Documenti Microsoft
author: HaoK
description: Prima di eseguire questa esercitazione che è necessario completare prima creare un'app web di ASP.NET MVC 5 sicura con l'accesso, la reimpostazione di posta elettronica di conferma e la password. In questa esercitazione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0167388cf6b488b72ca36f583a7794690dbf9900
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876033"
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>La conferma dell'account e Password di ripristino con identità di ASP.NET (c#)
====================
dal [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Prima di eseguire questa esercitazione è necessario completare [creare un'app web di ASP.NET MVC 5 sicura con l'accesso, la reimpostazione di posta elettronica di conferma e la password](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). In questa esercitazione contiene ulteriori dettagli e viene illustrato come impostare la posta elettronica per la conferma dell'account locale e consentire agli utenti di reimpostare la password dimenticata in ASP.NET Identity. In questo articolo è stato scritto dal Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi. L'esempio di NuGet è stata scritta principalmente da Hao Kung.


Un account utente locale richiede all'utente di creare una password per l'account e password verrà memorizzata () nell'app web. Identità di ASP.NET supporta anche gli account di social networking, che non richiedono all'utente di creare una password per l'app. [Gli account di social networking](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) utilizzare una terza parte (ad esempio Google, Twitter, Facebook o Microsoft) per autenticare gli utenti. Questo argomento vengono illustrate le operazioni seguenti:

- [Creare un'applicazione MVC ASP.NET](#createMvc) ed esplorare le funzionalità di ASP.NET Identity.
- [Compilazione dell'esempio di identità](#build)
- [Impostare la conferma tramite posta elettronica](#email)

Nuovi utenti di registrare i relativi alias di posta elettronica, che consente di creare un account locale.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Fare clic sul pulsante registro invia un messaggio di posta elettronica di conferma contenente un token di convalida per l'indirizzo di posta elettronica.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

L'utente viene inviato un messaggio di posta elettronica con un token di conferma per il proprio account.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Facendo clic sul collegamento di conferma dell'account.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Ripristino o la reimpostazione della password

Gli utenti locali che dimenticano la password possono avere un token di sicurezza inviato al proprio account di posta elettronica, che consente di reimpostare la password.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
L'utente riceverà presto un messaggio di posta elettronica con un collegamento che consente loro di reimpostazione della password.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Facendo clic sul collegamento giungeranno alla pagina di reimpostazione.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Fare clic su di **reimpostare** pulsante confermare la password è stata reimpostata.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Creare un'app Web ASP.NET

Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installazione di Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva.

> [!NOTE]
> Avviso: È necessario installare Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) per completare questa esercitazione.


1. Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC. Web Form supporta anche ASP.NET Identity, pertanto è possibile attenersi alla procedura simile in un'app di web form.
2. Lasciare l'autenticazione predefinita come **singoli account utente di**.
3. Eseguire l'app, fare clic su di **registrare** collegare e registrare un utente. A questo punto, la convalida sola nel messaggio di posta elettronica è con il [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attributo.
4. In Esplora Server, passare a **dati Connections\DefaultConnection\Tables\AspNetUsers**fare clic destro del mouse e selezionare **Apri definizione tabella**.

    La figura seguente mostra il `AspNetUsers` dello schema:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Fare clic con il pulsante destro sul **AspNetUsers** tabella e selezionare **Mostra dati tabella**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   A questo punto il messaggio di posta elettronica non è stato confermato.

L'archivio di dati predefinito per ASP.NET Identity è Entity Framework, ma è possibile configurarlo per utilizzare altri archivi dati e per aggiungere altri campi. Vedere [risorse aggiuntive](#addRes) sezione alla fine di questa esercitazione.

Il [classe di avvio OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) viene chiamato quando l'app viene avviata e richiama il `ConfigureAuth` metodo *App\_Start\Startup.Auth.cs*, Configura la pipeline OWIN che inizializza ASP.NET Identity. Esaminare il `ConfigureAuth` metodo. Ogni `CreatePerOwinContext` chiamata registra un callback (salvato nel `OwinContext`) che verrà chiamato una volta per ogni richiesta per creare un'istanza del tipo specificato. È possibile impostare un punto di interruzione nel costruttore e `Create` il metodo di ogni tipo (`ApplicationDbContext, ApplicationUserManager`) e verificare che vengono chiamati su ogni richiesta. Un'istanza di `ApplicationDbContext` e `ApplicationUserManager` viene archiviato nel contesto di OWIN, è possibile accedere in tutta l'applicazione. ASP.NET Identity hook nella pipeline OWIN tramite il middleware di cookie. Per ulteriori informazioni, vedere [Per la gestione della durata richiesta per la classe UserManager in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Quando si modifica il profilo di sicurezza, un indicatore di sicurezza verrà generato e archiviato nel `SecurityStamp` campo il *AspNetUsers* tabella. Si noti che il `SecurityStamp` campo è diverso dal cookie di sicurezza. Il cookie di sicurezza non verrà memorizzato nel `AspNetUsers` tabella (o qualsiasi altro nel database di identità). Il token del cookie di sicurezza è autofirmato usando [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e viene creato con il `UserId, SecurityStamp` e informazioni sull'ora di scadenza.

Middleware del cookie controlla il cookie a ogni richiesta. Il `SecurityStampValidator` metodo il `Startup` classe raggiungono il database e controlla periodicamente, indicatore di sicurezza come specificato con il `validateInterval`. Questo errore si verifica solo ogni 30 minuti (in questo esempio) a meno che non si modifica il profilo di sicurezza. L'intervallo di 30 minuti è stato scelto per ridurre al minimo trip al database. Vedere il [esercitazione di autenticazione a due fattori](index.md) per altri dettagli.

Per i commenti nel codice, il `UseCookieAuthentication` metodo supporta l'autenticazione dei cookie. Il `SecurityStamp` codice associato e viene fornisce un ulteriore livello di sicurezza per l'app, quando si modifica la password, verrà registrato del browser eseguire l'accesso. Il `SecurityStampValidator.OnValidateIdentity` metodo consente all'app convalidare il token di sicurezza quando l'utente effettua l'accesso, che viene utilizzato quando si modifica una password o l'account di accesso esterno. Questo è necessario per garantire che tutti i token (cookie) generati con la vecchia password vengono invalidati. Nel progetto di esempio, se si modifica la password di utenti, quindi un nuovo token viene generata per l'utente, vengono invalidati tutti i token precedenti e `SecurityStamp` campo viene aggiornato.

Il sistema di identità consentono di configurare l'app, quando viene modificato il profilo di sicurezza agli utenti (ad esempio, quando l'utente modifica la password o modifiche associata account di accesso (ad esempio da Facebook, Google, account Microsoft, e così via), l'utente è connesso da tutti istanze del browser. Ad esempio, l'immagine seguente viene illustrato il [esempio Single Sign-Out](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) app, che consente all'utente di disconnettersi da tutte le istanze del browser (in questo caso, Internet Explorer, Firefox e Chrome) facendo clic su un pulsante. In alternativa, l'esempio consente di disconnettersi solo un'istanza specifica del browser.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

Il [esempio Single Sign-Out](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) app Mostra come identità di ASP.NET consente di rigenerare il token di sicurezza. Questo è necessario per garantire che tutti i token (cookie) generati con la vecchia password vengono invalidati. Questa funzionalità fornisce un ulteriore livello di sicurezza per l'applicazione. Quando si modifica la password, verrà registrato il quale si accede a questa applicazione.

Il *App\_Start\IdentityConfig.cs* file contiene il `ApplicationUserManager`, `EmailService` e `SmsService` classi. Il `EmailService` e `SmsService` classi ogni implementano la `IIdentityMessageService` interfaccia, in modo che si dispone di metodi comuni in ogni classe per configurare posta elettronica e SMS. Sebbene questa esercitazione viene illustrato solo come aggiungere una notifica tramite posta elettronica tramite [SendGrid](http://sendgrid.com/), è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.

Il `Startup` classe contiene inoltre una presentazione per la stampa per aggiungere gli account di accesso social (Facebook, Twitter e così via), vedere l'esercitazione [MVC 5 App con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) per altre informazioni.

Esaminare il `ApplicationUserManager` (classe), che contiene le informazioni sull'identità di utenti e configura le funzionalità seguenti:

- Requisiti di complessità delle password.
- Blocco utente (tentativi e l'ora).
- Autenticazione a due fattori (2FA). Descritti 2FA e SMS in un'altra esercitazione.
- Associazione di posta e dei servizi SMS. (Un'altra esercitazione verrà descritta SMS).

Il `ApplicationUserManager` classe deriva dal modello generico `UserManager<ApplicationUser>` classe. `ApplicationUser` deriva da [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` deriva da generica `IdentityUser` classe:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Le proprietà precedente coincidano con le proprietà di `AspNetUsers` tabella, illustrata in precedenza.

Gli argomenti generici in `IUser` consentono di derivare una classe di utilizzo di tipi diversi per la chiave primaria. Vedere il [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) esempio in cui viene illustrato come modificare la chiave primaria da stringa a int o GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) è definito in *Models\IdentityModels.cs* come:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Genera il codice evidenziato sopra un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity e OWIN Cookie di autenticazione basata sulle attestazioni, pertanto il framework richiede che l'app per generare un `ClaimsIdentity` per l'utente. `ClaimsIdentity` contiene informazioni su tutte le attestazioni per l'utente, ad esempio il nome dell'utente, l'età e i ruoli utente appartiene. È anche possibile aggiungere ulteriori attestazioni per l'utente in questa fase.

OWIN `AuthenticationManager.SignIn` metodo passa il `ClaimsIdentity` e accede l'utente:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[App di MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) viene illustrato come è possibile aggiungere proprietà aggiuntive per la `ApplicationUser` classe.

## <a name="email-confirmation"></a>Conferma tramite posta elettronica

È consigliabile verificare il messaggio di posta elettronica un nuovo utente registrarsi per verificare che non rappresentano un altro utente (vale a dire, è ancora stato registrato con un altro messaggio di posta elettronica). Si supponga di un forum di discussione, si desidera impedire `"bob@example.com"` tramite la registrazione come `"joe@contoso.com"`. Senza conferma tramite posta elettronica, `"joe@contoso.com"` Impossibile ricevere la posta elettronica indesiderato dall'app. Si supponga che Bob accidentalmente registrato come `"bib@example.com"` e non veniva notare, ha non sarebbe in grado di utilizzare password Ripristina perché l'app non ha il suo messaggio di posta elettronica corretto. Conferma tramite posta elettronica fornisce a livello di protezione limitato da BOT e non offre protezione da spamming determinato, hanno molti alias di posta elettronica di lavoro che possono utilizzare per registrare. Nell'esempio seguente, l'utente non sarà possibile cambiare la password, fino a quando non è stato confermato l'account (da esse facendo clic su un collegamento di conferma ricevuto in siano registrati con l'account di posta elettronica.) È possibile applicare questo flusso di lavoro in altri scenari, ad esempio l'invio di un collegamento per confermare e reimpostare la password al nuovo account creato dall'amministratore, l'invio all'utente un messaggio di posta elettronica quando vengono modificati il proprio profilo e così via. In genere si desidera impedire agli utenti di nuovo di registrazione dei dati del sito web prima che siano stati confermati tramite posta elettronica, un SMS o un altro meccanismo. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>La creazione di un esempio più completo

In questa sezione si userà NuGet per scaricare un esempio più completo, che si opererà con.

1. Creare un nuovo ***vuoto*** progetto Web ASP.NET.
2. Nella Console di gestione pacchetti, immettere quanto segue i comandi seguenti: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   In questa esercitazione si userà [SendGrid](http://sendgrid.com/) per inviare posta elettronica. Il `Identity.Samples` pacchetto viene installato il codice che verrà utilizzato con.
3. Impostare il [progetto utilizzare SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Creazione di account locali di test eseguendo l'app, fare clic di **registrare** il collegamento e registrazione del modulo di registrazione.
5. Scegliere il collegamento di posta elettronica, demo che simula una conferma tramite posta elettronica.
6. Rimuovere il codice di conferma demo collegamento e-mail tratto dall'esempio (il `ViewBag.Link` codice nel controller di account. Vedere il `DisplayEmail` e `ForgotPasswordConfirmation` metodi di azione e visualizzazioni razor).

> [!NOTE]
> Avviso: Se si modifica una qualsiasi delle impostazioni di sicurezza in questo esempio, le applicazioni di produzione saranno necessario eseguire un controllo di sicurezza che chiama in modo esplicito le modifiche apportate.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Esaminare il codice nell'App\_Start\IdentityConfig.cs

L'esempio viene illustrato come creare un account e aggiungerlo al *Admin* ruolo. Messaggio di posta elettronica nell'esempio è necessario sostituire con il messaggio di posta elettronica che verrà utilizzato per l'account amministratore. Il modo più semplice subito a creare un account amministratore è a livello di codice nel `Seed` metodo. Ci auguriamo che in futuro uno strumento che consente di creare e amministrare utenti e ruoli. Nell'esempio di codice consentono di creare e gestire utenti e ruoli, ma è necessario innanzitutto un account degli amministratori per eseguire i ruoli e le pagine di amministrazione di utente. In questo esempio, l'account amministratore viene creato quando viene effettuato il seeding del database.

Modificare la password e modificare il nome a un account in cui è possibile ricevere notifiche tramite posta elettronica.

> [!WARNING]
> Sicurezza - archivio mai i dati sensibili nel codice sorgente.

Come accennato in precedenza, il `app.CreatePerOwinContext` chiamata nella classe di avvio aggiunge i callback per la `Create` metodo le app DB contenuto, gestione e il ruolo Gestione classi utente. Chiamate di pipeline OWIN il `Create` metodo su queste classi per ogni richiesta e archivia il contesto per ogni classe. Il controller di account espone il gestore utenti dal contesto HTTP (che contiene il contesto OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Quando un utente registra un account locale, il `HTTP Post Register` metodo viene chiamato:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Il codice precedente utilizza i dati del modello per creare un nuovo account utente utilizzando il messaggio di posta elettronica e la password immessa. Se l'alias di posta elettronica si trova nell'archivio dati, la creazione di account non riesce e viene visualizzato nuovamente il modulo. Il `GenerateEmailConfirmationTokenAsync` metodo crea un token di conferma sicura e viene memorizzato nell'archivio dati di ASP.NET Identity. Il [Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) metodo crea un collegamento contenente il `UserId` e token di conferma. Questo collegamento viene quindi inviato tramite posta elettronica dell'utente, l'utente può fare clic sul collegamento nella propria app di posta elettronica per confermare il proprio account.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Impostare la conferma tramite posta elettronica

Passare al [pagina di iscrizione Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) e registrare l'account gratuito. Aggiungere codice analogo al seguente per configurare SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Client di posta elettronica è spesso accettare solo messaggi di testo (non HTML). È necessario fornire il messaggio in testo e HTML. Nell'esempio SendGrid, questa operazione viene eseguita con il `myMessage.Text` e `myMessage.Html` nel codice sopra riportato.


Il codice seguente viene illustrato come inviare tramite posta elettronica di [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) classe where `message.Body` restituisce solo il collegamento.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Sicurezza - archivio mai i dati sensibili nel codice sorgente. L'account e le credenziali vengono archiviate in appSetting. In Azure, è possibile archiviare questi valori in modo sicuro nel **[configura](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** scheda nel portale di Azure. Vedere [procedure consigliate per la distribuzione delle password e altri dati sensibili su ASP.NET e in Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Immettere le credenziali SendGrid, eseguire l'app, registrazione con un alias di posta elettronica può fare clic sul collegamento di conferma nel messaggio di posta elettronica. Per informazioni su come eseguire questa operazione con il [Outlook.com](http://outlook.com) account di posta elettronica, vedere di John Atten [c# configurazione SMTP per l'Host SMTP Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) e il suo[identità ASP.NET 2.0: convalida dell'impostazione di Account e l'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) post.

Una volta che un utente fa clic il **registrare** pulsante al relativo indirizzo di posta elettronica viene inviato un messaggio di conferma contenente un token di convalida.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

L'utente viene inviato un messaggio di posta elettronica con un token di conferma per il proprio account.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Esaminare il codice

Nel codice seguente viene illustrato il metodo `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Il metodo non riesce senza avvisare se il messaggio di posta elettronica utente non è stato confermato. Se è stato registrato un errore per un indirizzo di posta elettronica non valido, gli utenti malintenzionati è possibile utilizzare tali informazioni per trovare l'ID utente valido (alias di posta elettronica) agli attacchi.

Il codice seguente illustra il `ConfirmEmail` metodo nel controller di account che viene chiamato quando l'utente fa clic sul collegamento di conferma nel messaggio di posta elettronica inviato a essi:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Dopo che è stato utilizzato un token di password dimenticata, viene invalidato. La modifica di codice seguente il `Create` (metodo) (nel *App\_Start\IdentityConfig.cs* file) imposta il token scadrà fra 3 ore.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Con il codice precedente, la password dimenticata e i token di conferma tramite posta elettronica scadrà in 3 ore. Il valore predefinito `TokenLifespan` è un giorno.

Il codice seguente viene illustrato il metodo di conferma tramite posta elettronica:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Per rendere la tua app più sicura, ASP.NET Identity supporta autenticazione a due fattori (2FA). Vedere [identità di ASP.NET 2.0: impostazione di convalida dell'Account e l'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) da John Atten. Anche se è possibile impostare il blocco degli account per i tentativi non riusciti password di account di accesso, tale approccio rende sensibili per l'account di accesso [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) blocchi. È consigliabile che utilizzare il blocco degli account solo con 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

- [Panoramica dei provider di archiviazione personalizzati per ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [App di MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) viene inoltre illustrato come aggiungere informazioni sul profilo alla tabella users.
- [ASP.NET MVC e identità 2.0: nozioni di base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) da John Atten.
- [Introduzione ad ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Annuncio RTM di ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) da Pranav Rastogi.
