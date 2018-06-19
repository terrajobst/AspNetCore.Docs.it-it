---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity | Documenti Microsoft
author: HaoK
description: In questa esercitazione Mostra come configurare l'autenticazione a due fattori (2FA) tramite posta elettronica e SMS. In questo articolo è stato scritto dal Rick Anderson ( @RickAndMSFT ), PR....
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c8f628d177004a8569dde2651469ed591e48591e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876137"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Autenticazione a due fattori tramite SMS e posta elettronica con identità di ASP.NET
====================
dal [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> In questa esercitazione Mostra come configurare l'autenticazione a due fattori (2FA) tramite posta elettronica e SMS.
> 
> In questo articolo è stato scritto dal Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi. L'esempio di NuGet è stata scritta principalmente da Hao Kung.


Questo argomento vengono illustrate le operazioni seguenti:

- [Compilazione dell'esempio di identità](#build)
- [Impostare SMS per l'autenticazione a due fattori](#SMS)
- [Abilitare l'autenticazione a due fattori](#enable2)
- [Come registrare un provider di autenticazione a due fattori](#reg)
- [Combinare gli account di accesso social networking e locale](#combine)
- [Blocco degli account da attacchi di forza bruta](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Compilazione dell'esempio di identità

In questa sezione si userà NuGet per scaricare un esempio che si opererà con. Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installazione di Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva.

> [!NOTE]
> Avviso: È necessario installare Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) per completare questa esercitazione.


1. Creare un nuovo ***vuoto*** progetto Web ASP.NET.
2. Nella Console di gestione pacchetti, immettere quanto segue i comandi seguenti:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   In questa esercitazione si userà [SendGrid](http://sendgrid.com/) per inviare posta elettronica e [Twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) per collaudo sms. Il `Identity.Samples` pacchetto viene installato il codice che verrà utilizzato con.
3. Impostare il [progetto utilizzare SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Parametro facoltativo*: seguire le istruzioni in my [esercitazione conferma tramite posta elettronica](account-confirmation-and-password-recovery-with-aspnet-identity.md) collegare SendGrid e quindi eseguire l'app e registrare un account di posta elettronica.
5. * Facoltativo: * rimuovere il codice di conferma demo collegamento e-mail tratto dall'esempio (il `ViewBag.Link` codice nel controller di account. Vedere il `DisplayEmail` e `ForgotPasswordConfirmation` metodi di azione e visualizzazioni razor).
6. <em>Facoltativo: * rimuovere il `ViewBag.Status` codice verso i controller di gestione e l'Account e dal *Views\Account\VerifyCode.cshtml</em> e <em>Views\Manage\VerifyPhoneNumber.cshtml</em> visualizzazioni razor. In alternativa, è possibile mantenere il `ViewBag.Status` visualizzato per verificare come questa app funziona in locale senza dover collegare e inviare posta elettronica e messaggi SMS.

> [!NOTE]
> Avviso: Se si modifica una qualsiasi delle impostazioni di sicurezza in questo esempio, le applicazioni di produzione saranno necessario eseguire un controllo di sicurezza che chiama in modo esplicito le modifiche apportate.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Impostare SMS per l'autenticazione a due fattori

In questa esercitazione vengono fornite istruzioni per l'utilizzo di Twilio o ASPSMS ma è possibile usare qualsiasi altro provider SMS.

1. **Creazione di un Account utente con un provider SMS**  
  
   Creare un [Twilio](https://www.twilio.com/try-twilio) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.
2. **Installazione di pacchetti aggiuntivi o l'aggiunta di riferimenti al servizio**  
  
   Twilio:  
   Nella Console di gestione pacchetti, immettere il comando seguente:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   È necessario aggiungere il riferimento al servizio seguente:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Indirizzo:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Spazio dei nomi:  
    `ASPSMSX2`
3. **Capire le credenziali utente del Provider SMS**  
  
   Twilio:  
   Dal **Dashboard** scheda dell'account di Twilio, copia il **SID di Account** e **token di autorizzazione**.  
  
   ASPSMS:  
   Le impostazioni dell'account, passare alla **parametri Userkey** e copiarlo insieme il Self-definito **Password**.  
  
   Questi valori in un secondo momento verrà archiviato nella variabile di `SMSAccountIdentification` e `SMSAccountPassword` .
4. **Specifica l'ID mittente / iniziatore**  
  
   Twilio:  
   Dal **numeri** scheda, copiare il numero di telefono di Twilio.  
  
   ASPSMS:  
   All'interno di **dei creatori di sbloccare** Menu, sbloccare una o più mittenti o scegliere un creatore alfanumerico (non supportato da tutte le reti).  
  
   Questo valore in un secondo momento verrà archiviato nella variabile `SMSAccountFrom` .
5. **Il trasferimento di credenziali del provider SMS in app**  
  
   Rendere disponibile per l'app le credenziali e il numero di telefono mittente:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Sicurezza - archivio mai i dati sensibili nel codice sorgente. L'account e le credenziali vengono aggiunti al codice precedente per semplificare il codice di esempio. Vedere di Jon Atten [ASP.NET MVC: mantenere privato Out di impostazioni di controllo del codice sorgente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementazione del trasferimento dei dati al provider SMS**  
  
   Configurare il `SmsService` classe il *App\_Start\IdentityConfig.cs* file.  
  
   A seconda del provider SMS utilizzato attivare il **Twilio** o **ASPSMS** sezione: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Eseguire l'app e accedere con l'account che è stato registrato in precedenza.
8. Fare clic sull'ID utente, che attiva il `Index` metodo di azione `Manage` controller.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Fare clic su Aggiungi.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. In pochi secondi si otterrà un messaggio di testo con il codice di verifica. L'immissione e premere **Invia**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. La visualizzazione Gestione mostra che il numero di telefono è stato aggiunto.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Esaminare il codice

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

Il `Index` metodo di azione `Manage` controller imposta il messaggio di stato in base l'azione precedente e vengono forniti i collegamenti per modificare la password locale o aggiungere un account locale. Il `Index` metodo visualizza inoltre lo stato o il 2FA phone numero, l'account di accesso esterni, 2FA abilitato e ricordare 2FA metodo per il browser (illustrato più avanti). Facendo clic sull'ID utente (posta elettronica) nella barra del titolo non passare un messaggio. Facendo clic su di **numero di telefono: rimuovere** collegare passate `Message=RemovePhoneSuccess` come una stringa di query.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

Il `AddPhoneNumber` metodo di azione consente di visualizzare una finestra di dialogo per immettere un numero di telefono che può ricevere i messaggi SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Facendo clic su di **invia il codice di verifica** pulsante Invia il numero di telefono per HTTP POST `AddPhoneNumber` metodo di azione.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

Il `GenerateChangePhoneNumberTokenAsync` metodo genera il token di sicurezza che verrà impostato nel messaggio SMS. Se il servizio SMS è stato configurato, il token viene inviato come stringa &quot;il codice di sicurezza è &lt;token&gt;&quot;. Il `SmsService.SendAsync` metodo viene chiamato in modo asincrono, quindi l'app viene reindirizzato il `VerifyPhoneNumber` metodo di azione (che consente di visualizzare la finestra di dialogo seguente), in cui è possibile immettere il codice di verifica.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Dopo aver immesso il codice e fare clic su Invia, viene inserito il codice HTTP POST `VerifyPhoneNumber` metodo di azione.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

Il `ChangePhoneNumberAsync` metodo controlla il codice di sicurezza registrato. Se il codice è corretto, il numero di telefono viene aggiunto per il `PhoneNumber` campo il `AspNetUsers` tabella. Se la chiamata ha esito positivo, il `SignInAsync` metodo viene chiamato:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

Il `isPersistent` parametro determina se la sessione di autenticazione è persistente in più richieste.

Quando si modifica il profilo di sicurezza, un indicatore di sicurezza verrà generato e archiviato nel `SecurityStamp` campo il *AspNetUsers* tabella. Si noti che il `SecurityStamp` campo è diverso dal cookie di sicurezza. Il cookie di sicurezza non verrà memorizzato nel `AspNetUsers` tabella (o qualsiasi altro nel database di identità). Il token del cookie di sicurezza è autofirmato usando [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e viene creato con il `UserId, SecurityStamp` e informazioni sull'ora di scadenza.

Middleware del cookie controlla il cookie a ogni richiesta. Il `SecurityStampValidator` metodo il `Startup` classe raggiungono il database e controlla periodicamente, indicatore di sicurezza come specificato con il `validateInterval`. Questo errore si verifica solo ogni 30 minuti (in questo esempio) a meno che non si modifica il profilo di sicurezza. L'intervallo di 30 minuti è stato scelto per ridurre al minimo trip al database.

Il `SignInAsync` (metodo) deve essere chiamato quando si apportano modifiche al profilo di sicurezza. Quando viene modificato il profilo di sicurezza, il database è aggiornamenti il `SecurityStamp` campo e senza chiamare il `SignInAsync` metodo è necessario rimanere connessi a *solo* fino alla successiva esecuzione della pipeline OWIN accede al database (il `validateInterval`). È possibile testare questo modificando il `SignInAsync` per restituire immediatamente e l'impostazione del cookie `validateInterval` proprietà da 30 minuti a 5 secondi:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Con le modifiche al codice precedente, è possibile modificare il profilo di sicurezza (ad esempio, modificando lo stato di **due fattori abilitata**) e verrà eseguito 5 secondi quando il `SecurityStampValidator.OnValidateIdentity` metodo ha esito negativo. Rimuovere il ritorno a capo nel `SignInAsync` (metodo), modificare un altro profilo di sicurezza e non verranno registrate. Il `SignInAsync` metodo genera un nuovo cookie di sicurezza.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Abilitare l'autenticazione a due fattori

Nell'applicazione di esempio, è necessario utilizzare l'interfaccia utente per abilitare l'autenticazione a due fattori (2FA). Per abilitare 2FA, fare clic sull'ID utente (alias di posta elettronica) nella barra di spostamento.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Fare clic su Abilita 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Effettuare la disconnessione, quindi eseguire nuovamente l'accesso. Se è stato abilitato posta elettronica (vedere il [esercitazione precedente](account-confirmation-and-password-recovery-with-aspnet-identity.md)), è possibile selezionare il SMS o posta elettronica per 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Verrà visualizzata la pagina di verifica di codice in cui è possibile immettere il codice (da posta elettronica o SMS).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Facendo clic su di **ricordare questo browser** casella di controllo sarà esente dalla necessità di utilizzare 2FA per l'accesso con tale computer e il browser. Abilitazione 2FA e facendo clic su di **ricordare questo browser** offre una protezione avanzata 2FA da utenti malintenzionati che tentano di accedere all'account, purché non hanno accesso al computer. È possibile farlo in qualsiasi computer privati che vengono utilizzate regolarmente. Impostando **ricordare questo browser**, ottenere le funzionalità di sicurezza di 2FA dai computer non si utilizza regolarmente e ottenere il vantaggio nel non dover eseguire 2FA nei propri computer. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Come registrare un provider di autenticazione a due fattori

Quando si crea un nuovo progetto MVC, il *IdentityConfig.cs* file contiene il codice seguente per registrare un provider di autenticazione a due fattori:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Aggiungere un numero di telefono per 2FA

Il `AddPhoneNumber` metodo di azione di `Manage` controller genera un token di sicurezza ed invia a telefono numero che è stato fornito.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Dopo l'invio del token, reindirizza il `VerifyPhoneNumber` metodo di azione, in cui è possibile immettere il codice per registrare SMS per 2FA. 2FA SMS non viene utilizzato fino a quando non significa che il numero di telefono.

## <a name="enabling-2fa"></a>Abilitazione 2FA

Il `EnableTFA` 2FA consente di metodo di azione:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Si noti il `SignInAsync` deve essere chiamato perché 2FA attiva è una modifica al profilo di sicurezza. Quando 2FA è abilitata, l'utente dovrà utilizzare 2FA per l'accesso, utilizzando gli approcci 2FA registrazione (SMS e posta elettronica di esempio).

È possibile aggiungere ulteriori 2FA provider, ad esempio i generatori di codice a matrice o è possibile scrivere si è proprietari (vedere [utilizzando Google Authenticator con ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> I codici 2FA vengono generati utilizzando [basati sul tempo algoritmo Password monouso](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) e codici sono validi per sei minuti. Se si accettano più di sei minuti per immettere il codice, si otterrà un messaggio di errore del codice non valido.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Combinare gli account di accesso social networking e locale

È possibile combinare gli account locali e sociali facendo clic sul collegamento nel messaggio. Nella sequenza seguente &quot; RickAndMSFT@gmail.com &quot; viene innanzitutto creato un account di accesso locale, ma è possibile creare l'account come un accesso social prima, quindi aggiungere un account di accesso locale.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Fare clic su di **Gestisci** collegamento. Si noti il valore 0 esterno (account di accesso social) associata all'account.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Fare clic sul collegamento a un altro registro nel servizio e accettare le richieste di app. I due account sono stati combinati, sarà possibile eseguire l'accesso con uno di questi account. Si consiglia agli utenti di aggiungere gli account locali nel caso in cui il log di social networking nel servizio di autenticazione è inattivo o più probabilmente è più possibile accedere al proprio account di social networking.

Nella figura seguente, Tom è una procedura di accesso social (che si può vedere dal **account di accesso esterni: 1** visualizzato nella pagina).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Facendo clic su **Scegli una password** consente di aggiungere un registro locale in associato lo stesso account.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Blocco degli account da attacchi di forza bruta

È possibile proteggere gli account dell'app da attacchi con dizionario, consentendo di blocco dell'utente. Nell'esempio di codice il `ApplicationUserManager Create` metodo configura blocco:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Il codice precedente consente di blocco per solo l'autenticazione a due fattori. Sebbene sia possibile attivare un blocco per gli account di accesso modificando `shouldLockout` su true nel `Login` metodo del controller di account, si suggerisce di non abilitare blocco per gli account di accesso perché rende vulnerabili per l'account [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) account di accesso attacchi. Nell'esempio di codice, blocco è disabilitato per l'account amministratore creato nel `ApplicationDbInitializer Seed` metodo:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Che l'utente dispone di un account di posta elettronica convalidato

Il codice seguente richiede all'utente di disporre di un account di posta elettronica convalidato prima di poter accedere:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Come requisito 2FA controlla SignInManager

Accesso locale sia social Accedi per verificare se 2FA è abilitata. Se 2FA è abilitata, il `SignInManager` metodo di accesso restituisce `SignInStatus.RequiresVerification`, e l'utente verrà reindirizzato al `SendCode` metodo di azione, in cui è necessario immettere il codice per completare il log in sequenza. Se l'utente ha RememberMe è impostato sul cookie di utenti locale, il `SignInManager` restituirà `SignInStatus.Success` e non avranno passino 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Il codice seguente illustra il `SendCode` metodo di azione. Oggetto [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) viene creato con tutti i metodi di 2FA abilitati per l'utente. Il [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) viene passato per il [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, che consente all'utente di selezionare l'approccio 2FA (in genere di posta elettronica e SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Una volta che l'utente invia l'approccio 2FA, il `HTTP POST SendCode` viene chiamato il metodo di azione, il `SignInManager` invia il codice 2FA e l'utente viene reindirizzato al `VerifyCode` metodo di azione in cui possono inserire il codice per completare l'accesso.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>Blocco 2FA

Anche se è possibile impostare il blocco degli account per i tentativi non riusciti password di account di accesso, tale approccio rende sensibili per l'account di accesso [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) blocchi. È consigliabile che utilizzare il blocco degli account solo con 2FA. Quando il `ApplicationUserManager` viene creato, il codice di esempio imposta blocco 2FA e `MaxFailedAccessAttemptsBeforeLockout` a cinque. Una volta che un utente accede (tramite un account locale o sociali), ogni tentativo non riuscito in 2FA viene archiviato e se viene raggiunto il numero massimo di tentativi, l'utente è bloccato per cinque minuti (è possibile impostare il blocco ora con `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

- [ASP.NET Identity consigliato risorse](../getting-started/aspnet-identity-recommended-resources.md) elenco completo delle identità blog, video, esercitazioni e great così collega.
- [App di MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) viene inoltre illustrato come aggiungere informazioni sul profilo alla tabella users.
- [ASP.NET MVC e identità 2.0: nozioni di base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) da John Atten.
- [La conferma dell'account e il recupero della Password con identità di ASP.NET](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Introduzione ad ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Annuncio RTM di ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) da Pranav Rastogi.
- [Identità di ASP.NET 2.0: Impostazione di convalida dell'Account e l'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) da John Atten.
