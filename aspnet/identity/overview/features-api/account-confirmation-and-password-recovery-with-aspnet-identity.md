---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Conferma dell'account e recupero della Password con ASP.NET Identity (c#) | Microsoft Docs
author: HaoK
description: Prima di eseguire questa esercitazione che è consigliabile completare prima creare un'app web ASP.NET MVC 5 sicura con accesso, messaggio di posta elettronica conferma e reimpostazione della password. Questa esercitazione...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 77a3e9d5e8b2698d2464e33520d779febd4533bd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838068"
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Conferma account e recupero della Password con ASP.NET Identity (c#)
====================
dal [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Prima di eseguire questa esercitazione è consigliabile completare prima [creare un'app web ASP.NET MVC 5 sicura con accesso, messaggio di posta elettronica conferma e reimpostazione della password](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Questa esercitazione contiene altri dettagli e spiega come configurare posta elettronica di conferma dell'account locale e consentire agli utenti di reimpostare la password dimenticata in ASP.NET Identity. Questo articolo è stato scritto da Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi. L'esempio di NuGet è stato scritto principalmente da Hao Kung.


Un account utente locale richiede all'utente di creare una password per l'account e password verrà memorizzata () nell'app web. ASP.NET Identity supporta inoltre gli account di social networking, che non richiedono all'utente di creare una password per l'app. [Account di social networking](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) utilizzare una terza parte (ad esempio, Google, Twitter, Facebook o Microsoft) per autenticare gli utenti. In questo argomento illustra quanto segue:

- [Creare un'app ASP.NET MVC](#createMvc) ed esplorare le funzionalità di ASP.NET Identity.
- [Compilazione dell'esempio di identità](#build)
- [Configurare conferma tramite posta elettronica](#email)

Nuovi utenti di registrare relativi alias di posta elettronica, che consente di creare un account locale.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Facendo clic sul pulsante Registra invia un messaggio di posta elettronica di conferma contenente un token di convalida al relativo indirizzo di posta elettronica.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

L'utente viene inviato un messaggio di posta elettronica con un token di conferma per il proprio account.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Facendo clic sul collegamento di conferma dell'account.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Ripristino/reimpostazione della password

Gli utenti locali che dimenticano la password possono avere un token di sicurezza inviato a un account di posta elettronica, consentendo loro di reimpostare la password.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
L'utente riceverà a breve un messaggio di posta elettronica con un collegamento che consente loro di reimpostare la password.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Facendo clic sul collegamento giungeranno alla pagina di reimpostazione.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Facendo clic sui **reimpostare** pulsante viene visualizzata la conferma della password è stata reimpostata.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Creare un'app Web ASP.NET

Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva.

> [!NOTE]
> Avviso: È necessario installare Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) per completare questa esercitazione.


1. Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC. Web Form supporta inoltre ASP.NET Identity, pertanto è possibile seguire la procedura in un'applicazione web form.
2. Lasciare l'autenticazione predefinita come **singoli account utente di**.
3. Eseguire l'app, scegliere il **registrare** collegare e registrare un utente. A questo punto, l'unica convalida il messaggio di posta elettronica è con il [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attributo.
4. In Esplora Server, passare a **dati Connections\DefaultConnection\Tables\AspNetUsers**, fare clic e selezionare **Apri definizione tabella**.

    La figura seguente mostra la `AspNetUsers` dello schema:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Fare clic con il pulsante destro sul **AspNetUsers** tabelle e selezionare **Mostra dati tabella**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   A questo punto non è stato confermato l'indirizzo di posta elettronica.

L'archivio di dati predefinito per ASP.NET Identity è Entity Framework, ma è possibile configurarlo per usare altri archivi dati e per aggiungere altri campi. Visualizzare [risorse aggiuntive](#addRes) sezione alla fine di questa esercitazione.

Il [classe di avvio OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) viene chiamato quando l'app viene avviata e richiama il `ConfigureAuth` metodo *App\_Start\Startup.Auth.cs*, Configura la pipeline OWIN che inizializza ASP.NET Identity. Esaminare il metodo `ConfigureAuth`. Ciascuna `CreatePerOwinContext` chiamata registra un callback (salvato nel `OwinContext`) che verrà chiamato una volta per ogni richiesta per creare un'istanza del tipo specificato. È possibile impostare un punto di interruzione nel costruttore e `Create` metodo di ogni tipo (`ApplicationDbContext, ApplicationUserManager`) e verificare che vengono chiamati su ogni richiesta. Un'istanza di `ApplicationDbContext` e `ApplicationUserManager` viene archiviato nel contesto OWIN, che è possibile accedere in tutta l'applicazione. ASP.NET Identity associa alla pipeline OWIN tramite middleware dei cookie. Per altre informazioni, vedere [Per la gestione della durata richiesta per la classe UserManager in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Quando si modifica il profilo di sicurezza, un indicatore di sicurezza verrà generato e archiviato nella `SecurityStamp` campo le *AspNetUsers* tabella. Si noti che il `SecurityStamp` campo è diverso dal cookie di sicurezza. Il cookie di sicurezza non verrà memorizzato nel `AspNetUsers` tabella (o qualsiasi altro nel database di identità). Il token di cookie di sicurezza è autofirmato usando [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e viene creato con il `UserId, SecurityStamp` e informazioni sull'ora di scadenza.

Il middleware dei cookie controlla il cookie a ogni richiesta. Il `SecurityStampValidator` metodo nella `Startup` classe raggiunge il database e controlla periodicamente, indicatore di sicurezza come specificato con il `validateInterval`. Questo avviene ogni 30 minuti (in questo esempio) solo se non si modifica il profilo di sicurezza. Per ridurre al minimo trip al database è stato scelto l'intervallo di 30 minuti. Vedere il [esercitazione sull'autenticazione a due fattori](index.md) per altri dettagli.

Per i commenti nel codice, il `UseCookieAuthentication` metodo supporta l'autenticazione dei cookie. Il `SecurityStamp` codice associato e viene fornisce un livello aggiuntivo di sicurezza all'App, quando si modifica la password, verrà eseguito all'esterno del browser eseguire l'accesso. Il `SecurityStampValidator.OnValidateIdentity` metodo consente all'app di convalidare il token di sicurezza quando l'utente effettua l'accesso, che viene usato quando si cambia una password o l'account di accesso esterno. Ciò è necessario per garantire che qualsiasi token (cookie) generati con la vecchia password non sono più validi. Nel progetto di esempio, se si modifica la password degli utenti, quindi un nuovo token viene generata per l'utente, eventuali token precedente non sono più validi e `SecurityStamp` campo viene aggiornato.

Il sistema di identità consentono di configurare l'app, quando viene modificato il profilo di sicurezza agli utenti (ad esempio, quando l'utente modifica la propria password o le modifiche associata account di accesso (ad esempio da Facebook, Google, account Microsoft, e così via), l'utente è connesso all'esterno di tutti istanze del browser. Ad esempio, l'immagine seguente mostra la [esempio di Single Sign-Out](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) app, consentendo all'utente di disconnettersi da tutte le istanze del browser (in questo caso, Internet Explorer, Firefox e Chrome) facendo clic su un pulsante. In alternativa, il codice di esempio consente di accedere solo all'esterno di un'istanza specifica del browser.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

Il [esempio di Single Sign-Out](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) app Mostra come ASP.NET Identity consente di rigenerare il token di sicurezza. Ciò è necessario per garantire che qualsiasi token (cookie) generati con la vecchia password non sono più validi. Questa funzionalità fornisce un livello aggiuntivo di sicurezza per l'applicazione. Quando si modifica la password, verrà eseguito il quale si accede a questa applicazione.

Il *App\_Start\IdentityConfig.cs* file contiene il `ApplicationUserManager`, `EmailService` e `SmsService` classi. Il `EmailService` e `SmsService` classi ogni implementano il `IIdentityMessageService` interfaccia, in modo che si dispone di metodi più comuni in ogni classe per configurare posta elettronica e SMS. Sebbene in questa esercitazione illustra solo come aggiungere notifica tramite posta elettronica tramite [SendGrid](http://sendgrid.com/), è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.

Il `Startup` classe contiene inoltre standard per aggiungere gli account di accesso social (Facebook, Twitter e così via), vedere l'esercitazione [App MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) per altre informazioni.

Esaminare il `ApplicationUserManager` (classe), che contiene le informazioni sull'identità degli utenti e configura le funzionalità seguenti:

- Requisiti di complessità.
- Utente è stata bloccata (tentativi e l'ora).
- Autenticazione a due fattori (2FA). Verrà analizzato 2FA ed SMS in un'altra esercitazione.
- Eseguire l'hook del messaggio di posta elettronica e servizi SMS. (Verrà analizzato SMS in un'altra esercitazione).

Il `ApplicationUserManager` classe deriva da generica `UserManager<ApplicationUser>` classe. `ApplicationUser` deriva da [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` deriva da generica `IdentityUser` classe:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Le proprietà precedente coincidono con le proprietà nel `AspNetUsers` tabella, illustrata in precedenza.

Argomenti generici in `IUser` consentono di derivare una classe con tipi diversi per la chiave primaria. Vedere le [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) esempio che illustra come modificare la chiave primaria da stringa a int o GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) definito in *Models\IdentityModels.cs* come:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Il codice evidenziato precedente genera un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity e l'autenticazione tramite Cookie OWIN sono basate sulle attestazioni, pertanto il framework richiede che l'app per generare un `ClaimsIdentity` per l'utente. `ClaimsIdentity` contiene informazioni su tutte le attestazioni dell'utente, ad esempio il nome dell'utente, l'età e i ruoli utente appartiene. È anche possibile aggiungere altre attestazioni dell'utente in questa fase.

OWIN `AuthenticationManager.SignIn` metodo passa il `ClaimsIdentity` viene effettuato l'accesso utente:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[App MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) viene illustrato come aggiungere proprietà aggiuntive per il `ApplicationUser` classe.

## <a name="email-confirmation"></a>Conferma tramite posta elettronica

È una buona idea per confermare l'indirizzo di posta elettronica registrare un nuovo utente con per verificare che non rappresentano un altro utente (vale a dire non hanno registrato con un altro messaggio di posta elettronica). Si supponga di un forum di discussione, si desidera impedire `"bob@example.com"` tramite la registrazione come `"joe@contoso.com"`. Senza conferma tramite posta elettronica, `"joe@contoso.com"` è stato possibile ottenere l'indirizzo di posta elettronica indesiderato dalla propria app. Si supponga che Bob accidentalmente registrato come `"bib@example.com"` e non l'aveste notato, egli non sarebbe in grado di utilizzare ripristino password perché l'app non ha il suo indirizzo di posta elettronica corretto. Conferma tramite posta elettronica fornisce solo una protezione limitata dai Bot e non fornisce protezione da spammer determinato, hanno molti alias di posta elettronica di lavoro che possono usare per registrare. Nell'esempio seguente, l'utente non sarà in grado di modificare la password, fino a quando non è stato confermato il proprio account (da esse facendo clic su un collegamento di conferma ricevuto nell'account di posta elettronica registrato con.) È possibile applicare questo flusso di lavoro ad altri scenari, ad esempio l'invio di un collegamento per confermare e reimpostare la password nei nuovi account creati dall'amministratore, l'invio all'utente un messaggio di posta elettronica quando vengono modificati il proprio profilo e così via. In genere si desidera impedire ai nuovi utenti dalla registrazione di tutti i dati al sito web prima che sono state confermate tramite posta elettronica, un messaggio SMS o un altro meccanismo. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>Creazione di un esempio più completo

In questa sezione si userà NuGet per scaricare un esempio più esaustivo che collaboreremo con.

1. Creare una nuova ***vuoto*** progetto Web ASP.NET.
2. Nella Console di gestione pacchetti immettere quanto segue i comandi seguenti: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   In questa esercitazione, useremo [SendGrid](http://sendgrid.com/) per inviare posta elettronica. Il `Identity.Samples` pacchetto viene installato il codice si userà.
3. Impostare il [progetto per l'uso di SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Per testare la creazione dell'account locale che esegue l'app, fare clic sui **registrare** il collegamento e la registrazione del modulo di registrazione.
5. Fare clic sul collegamento di posta elettronica demo, che simula una conferma tramite posta elettronica.
6. Rimuovere il codice di conferma collegamento tramite posta elettronica demo tratto dall'esempio (il `ViewBag.Link` codice nel controller account. Vedere le `DisplayEmail` e `ForgotPasswordConfirmation` metodi di azione e visualizzazioni razor).

> [!NOTE]
> Avviso: Se si modifica una qualsiasi delle impostazioni di sicurezza in questo esempio, le app di produzione saranno necessario eseguire un controllo di sicurezza che chiama in modo esplicito le modifiche apportate.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Esaminare il codice nell'App\_Start\IdentityConfig.cs

L'esempio illustra come creare un account e aggiungerlo al *Admin* ruolo. È necessario sostituire l'indirizzo di posta elettronica nell'esempio con il messaggio di posta elettronica che si utilizzerà per l'account amministratore. Il modo più semplice subito a creare un account amministratore è a livello di codice nel `Seed` (metodo). Ci auguriamo di disporre in futuro uno strumento che ti permetterà di creare e amministrare utenti e ruoli. Il codice di esempio consentono di creare e gestire utenti e ruoli, ma è necessario disporre prima di tutto un account degli amministratori per eseguire i ruoli e le pagine di amministrazione di utente. In questo esempio, l'account amministratore viene creato quando il database viene inizializzato.

Modificare la password e modificare il nome a un account in cui è possibile ricevere notifiche tramite posta elettronica.

> [!WARNING]
> Security - store mai i dati sensibili nel codice sorgente.

Come accennato in precedenza, il `app.CreatePerOwinContext` chiamata nella classe startup aggiunge ai callback di `Create` metodo per le app DB contenuto, manager e il ruolo Gestione classi utente. Le chiamate di pipeline OWIN il `Create` metodo su queste classi per ogni richiesta e archivia il contesto per ogni classe. Il controller account espone il gestore utenti dal contesto, che contiene il contesto OWIN, HTTP:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Quando un utente si registra un account locale, il `HTTP Post Register` viene chiamato il metodo:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Il codice precedente Usa i dati del modello per creare un nuovo account utente utilizzando il messaggio di posta elettronica e la password immessa. Se l'alias di posta elettronica si trova nell'archivio dati, la creazione dell'account non riesce e viene visualizzato nuovamente il modulo. Il `GenerateEmailConfirmationTokenAsync` metodo crea un token di conferma sicuro e lo archivia nell'archivio dati di ASP.NET Identity. Il [Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) metodo crea un collegamento che contiene il `UserId` e token di conferma. Viene quindi inviato tramite posta elettronica il collegamento all'utente, l'utente può fare clic sul collegamento nella propria app di posta elettronica per confermare l'account.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Configurare conferma tramite posta elettronica

Andare alla [pagina di iscrizione Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) e registrare l'account gratuito. Aggiungere codice simile al seguente per configurare SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Client di posta elettronica è spesso accettare solo i messaggi di testo (non HTML). È necessario fornire il messaggio in testo e HTML. Nell'esempio SendGrid precedente, questa operazione viene eseguita con il `myMessage.Text` e `myMessage.Html` codice sopra riportato.


Il codice seguente illustra come inviare messaggio di posta elettronica tramite il [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) classe where `message.Body` restituisce solo il collegamento.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Security - store mai i dati sensibili nel codice sorgente. L'account e le credenziali vengono archiviate in appSetting. In Azure, è possibile archiviare in modo sicuro questi valori sul **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** scheda nel portale di Azure. Visualizzare [procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Immettere le credenziali di SendGrid, eseguire l'app, la registrazione con un alias di posta elettronica può fare clic sul collegamento di conferma nel messaggio di posta elettronica. Per informazioni su come eseguire questa operazione con il [Outlook.com](http://outlook.com) account di posta elettronica, vedere di John Atten [c# configurazione SMTP per l'Host SMTP Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) e il suo[identità ASP.NET 2.0: convalida dell'impostazione di Account e l'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) post.

Una volta che un utente sceglie il **registrare** pulsante al relativo indirizzo di posta elettronica viene inviato un messaggio di conferma contenente un token di convalida.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

L'utente viene inviato un messaggio di posta elettronica con un token di conferma per il proprio account.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Esaminare il codice

Nel codice seguente viene illustrato il metodo `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

Se non è stato confermato l'indirizzo di posta elettronica utente, il metodo non riesce senza avvisare. Se è stato inserito un errore per un indirizzo di posta elettronica non valido, gli utenti malintenzionati è stato possibile usare tali informazioni per trovare l'ID utente valido (alias di posta elettronica) agli attacchi.

Il codice seguente illustra il `ConfirmEmail` metodo nel controller account che viene chiamato quando l'utente fa clic sul collegamento di conferma nel messaggio di posta elettronica inviato ad essi:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Dopo che è stato usato un token di password dimenticata, che venga invalidata. La modifica di codice seguente nel `Create` metodo (nelle *App\_Start\IdentityConfig.cs* file) imposta il token scada in 3 ore.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Con il codice precedente, i token di conferma tramite posta elettronica e la password dimenticata scadrà in 3 ore. Il valore predefinito `TokenLifespan` è un giorno.

Il codice seguente illustra il metodo di conferma tramite posta elettronica:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Per rendere l'app più sicura, ASP.NET Identity supporta l'autenticazione a due fattori (2FA). Visualizzare [identità di ASP.NET 2.0: configurazione di convalida dell'Account e l'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) da John Atten. Sebbene sia possibile impostare il blocco degli account in tentativi non riusciti password di account di accesso, tale approccio rende l'account di accesso possibili attacchi [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) dei blocchi. È consigliabile che usare il blocco dell'account solo con 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

- [Panoramica dei provider di archiviazione personalizzati per ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [App MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) viene inoltre illustrato come aggiungere informazioni sul profilo per la tabella degli utenti.
- [ASP.NET MVC e identità 2.0: nozioni di base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) da John Atten.
- [Introduzione ad ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Annuncio della versione RTM di ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) di Pranav Rastogi.
