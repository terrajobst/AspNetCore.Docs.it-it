---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity | Microsoft Docs
author: HaoK
description: Questa esercitazione illustrerà come configurare l'autenticazione a due fattori (2FA) tramite SMS e posta elettronica. Questo articolo è stato scritto da Rick Anderson ( @RickAndMSFT ), PR....
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4b253923696e35e59c196578a232f53c11671d16
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577977"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity
====================
dal [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)

> Questa esercitazione illustrerà come configurare l'autenticazione a due fattori (2FA) tramite SMS e posta elettronica.
> 
> Questo articolo è stato scritto da Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi. L'esempio di NuGet è stato scritto principalmente da Hao Kung.


In questo argomento illustra quanto segue:

- [Compilazione dell'esempio di identità](#build)
- [Configurazione di SMS per l'autenticazione a due fattori](#SMS)
- [Abilitare l'autenticazione a due fattori](#enable2)
- [Come registrare un provider di autenticazione a due fattori](#reg)
- [Combinare gli account di accesso social e locali](#combine)
- [Blocco degli account da attacchi di forza bruta](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Compilazione dell'esempio di identità

In questa sezione si userà NuGet per scaricare un esempio che collaboreremo con. Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva.

> [!NOTE]
> Avviso: È necessario installare Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) per completare questa esercitazione.


1. Creare una nuova ***vuoto*** progetto Web ASP.NET.
2. Nella Console di gestione pacchetti immettere quanto segue i comandi seguenti:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   In questa esercitazione, useremo [SendGrid](http://sendgrid.com/) per inviare posta elettronica e [Twilio](https://www.twilio.com/) oppure [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) per sms inviato. Il `Identity.Samples` pacchetto viene installato il codice si userà.
3. Impostare il [progetto per l'uso di SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Facoltativo*: seguire le istruzioni nella mio [esercitazione conferma tramite posta elettronica](account-confirmation-and-password-recovery-with-aspnet-identity.md) collegare SendGrid e quindi eseguire l'app e registrare un account di posta elettronica.
5. * Facoltativo: * rimuovere il codice di conferma collegamento tramite posta elettronica demo tratto dall'esempio (il `ViewBag.Link` codice nel controller account. Vedere le `DisplayEmail` e `ForgotPasswordConfirmation` metodi di azione e visualizzazioni razor).
6. <em>Facoltativo: * rimuovere il `ViewBag.Status` codice verso i controller di gestione e l'Account e dal *Views\Account\VerifyCode.cshtml</em> e <em>Views\Manage\VerifyPhoneNumber.cshtml</em> visualizzazioni razor. In alternativa, è possibile mantenere il `ViewBag.Status` display per testare il funzionamento di questa app in locale senza dover associare e inviare messaggi SMS e posta elettronica.

> [!NOTE]
> Avviso: Se si modifica una qualsiasi delle impostazioni di sicurezza in questo esempio, le app di produzione saranno necessario eseguire un controllo di sicurezza che effettua chiamate in modo esplicito le modifiche apportate.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Configurazione di SMS per l'autenticazione a due fattori

Questa esercitazione offre istruzioni per l'uso di Twilio o ASPSMS ma è possibile usare qualsiasi altro provider SMS.

1. **Creazione di un Account utente con un provider SMS**  
  
   Creare un [Twilio](https://www.twilio.com/try-twilio) o un' [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.
2. **Installare pacchetti aggiuntivi o per aggiungere i riferimenti al servizio**  
  
   Twilio:  
   Nella Console di gestione pacchetti immettere il comando seguente:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   È necessario aggiungere il riferimento al servizio seguenti:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Indirizzo:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Spazio dei nomi:  
    `ASPSMSX2`
3. **Scoprire le credenziali utente del Provider SMS**  
  
   Twilio:  
   Dal **Dashboard** scheda dell'account Twilio, copiare la **SID Account** e **token di autenticazione**.  
  
   ASPSMS:  
   Da impostazioni dell'account, passare a **Userkey** e copiarlo insieme l'autodefinito **Password**.  
  
   Questi valori in un secondo momento verranno archiviati nelle variabili `SMSAccountIdentification` e `SMSAccountPassword` .
4. **Specifica l'ID mittente / creatore**  
  
   Twilio:  
   Dal **numeri** scheda, copiare il numero di telefono di Twilio.  
  
   ASPSMS:  
   All'interno di **lo sblocco dei creatori** Menu, sbloccare una o più entità di origine o scegliere un creatore alfanumerico (non supportato da tutte le reti).  
  
   Questo valore in un secondo momento verrà memorizzata nella variabile `SMSAccountFrom` .
5. **Il trasferimento di credenziali del provider SMS in app**  
  
   Rendere disponibile l'app per le credenziali e il numero di telefono mittente:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Security - store mai i dati sensibili nel codice sorgente. L'account e le credenziali vengono aggiunti al codice precedente per mantenere semplice l'esempio. Vedere di Jon Atten [ASP.NET MVC: mantenere le impostazioni Private fuori controllo del codice sorgente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementazione di trasferimento dei dati al provider SMS**  
  
   Configurare il `SmsService` classe la *App\_Start\IdentityConfig.cs* file.  
  
   A seconda del provider SMS usato attivare i **Twilio** o nella **ASPSMS** sezione: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Eseguire l'app e accedere con l'account registrato in precedenza.
8. Fare clic sul proprio ID utente, che si attiva la `Index` metodo di azione `Manage` controller.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Fare clic su Aggiungi.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. In pochi secondi si riceverà un messaggio di testo con il codice di verifica. Immetterla e premere **Submit**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. La visualizzazione di gestione mostra che il numero di telefono è stato aggiunto.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Esaminare il codice

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

Il `Index` metodo di azione `Manage` controller imposta il messaggio di stato dell'azione precedente in base e vengono forniti collegamenti per modificare la password locale o aggiungere un account locale. Il `Index` metodo visualizza inoltre lo stato o le 2FA phone numero, account di accesso esterni, abilitato 2FA e ricordare metodo 2FA per questo browser (descritto più avanti). Facendo clic sul proprio ID utente (indirizzo di posta elettronica) nella barra del titolo non passare un messaggio. Facendo clic sui **numero di telefono: rimuovere** collegare passate `Message=RemovePhoneSuccess` come una stringa di query.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

Il `AddPhoneNumber` metodo di azione consente di visualizzare una finestra di dialogo per immettere un numero di telefono che può ricevere messaggi SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Facendo clic sui **Invia codice di verifica** pulsante Invia il numero di telefono per la richiesta HTTP POST `AddPhoneNumber` metodo di azione.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

Il `GenerateChangePhoneNumberTokenAsync` metodo genera il token di sicurezza che verrà impostato nel messaggio SMS. Se il servizio SMS è stato configurato, il token viene inviato come stringa &quot;il codice diprotezione è &lt;token&gt;&quot;. Il `SmsService.SendAsync` metodo viene chiamato in modo asincrono, quindi viene reindirizzato all'app per la `VerifyPhoneNumber` metodo di azione (che consente di visualizzare la finestra di dialogo seguente), in cui è possibile immettere il codice di verifica.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Quando si immette il codice e fare clic su Invia, il codice viene pubblicato per la richiesta HTTP POST `VerifyPhoneNumber` metodo di azione.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

Il `ChangePhoneNumberAsync` metodo controlla il codice di sicurezza registrate. Se il codice sia corretto, il numero di telefono viene aggiunto per il `PhoneNumber` campo del `AspNetUsers` tabella. Se tale chiamata ha esito positivo, il `SignInAsync` viene chiamato il metodo:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

Il `isPersistent` parametro determina se la sessione di autenticazione è persistente tra più richieste.

Quando si modifica il profilo di sicurezza, un indicatore di sicurezza verrà generato e archiviato nella `SecurityStamp` campo le *AspNetUsers* tabella. Si noti che il `SecurityStamp` campo è diverso dal cookie di sicurezza. Il cookie di sicurezza non verrà memorizzato nel `AspNetUsers` tabella (o qualsiasi altro nel database di identità). Il token di cookie di sicurezza è autofirmato usando [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e viene creato con il `UserId, SecurityStamp` e informazioni sull'ora di scadenza.

Il middleware dei cookie controlla il cookie a ogni richiesta. Il `SecurityStampValidator` metodo nella `Startup` classe raggiunge il database e controlla periodicamente, indicatore di sicurezza come specificato con il `validateInterval`. Questo avviene ogni 30 minuti (in questo esempio) solo se non si modifica il profilo di sicurezza. Per ridurre al minimo trip al database è stato scelto l'intervallo di 30 minuti.

Il `SignInAsync` metodo deve essere chiamato quando si apportano modifiche al profilo di sicurezza. Quando viene modificato il profilo di sicurezza, il database sia gli aggiornamenti il `SecurityStamp` campo e senza chiamare il `SignInAsync` metodo potrebbe rimanere connessi a *solo* fino all'esecuzione successiva della pipeline OWIN acceda al database (la `validateInterval`). È possibile verificarlo modificando la `SignInAsync` per restituire immediatamente e l'impostazione del cookie `validateInterval` proprietà da 30 minuti su 5 secondi:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Con le modifiche al codice precedente, è possibile modificare il profilo di sicurezza (ad esempio, modificando lo stato del **due fattori abilitati**) e verrà disconnesso entro 5 secondi durante il `SecurityStampValidator.OnValidateIdentity` metodo ha esito negativo. Rimozione del ritorno a capo nel `SignInAsync` metodo, modificare un altro profilo di sicurezza e non verrà disconnesso. Il `SignInAsync` metodo genera un nuovo cookie di sicurezza.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Abilitare l'autenticazione a due fattori

Nell'app di esempio, è necessario usare l'interfaccia utente per abilitare l'autenticazione a due fattori (2FA). Per abilitare 2FA, fare clic sul proprio ID utente (alias di posta elettronica) nella barra di spostamento.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Fare clic su Abilita 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Effettuare la disconnessione, quindi eseguire nuovamente l'accesso. Se è stato abilitato posta elettronica (vedere la [esercitazione precedente](account-confirmation-and-password-recovery-with-aspnet-identity.md)), è possibile selezionare il SMS o posta elettronica di autenticazione 2fa.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Verrà visualizzata la pagina di verifica codice in cui è possibile immettere il codice (dall'indirizzo di posta elettronica o SMS).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Facendo clic sui **memorizzare questo browser** casella di controllo sarà esente dalla necessità di utilizzare 2FA per l'accesso con tale computer e browser. Abilitare 2FA e facendo clic sulla **memorizzare questo browser** fornirà protezione 2FA sicuro da utenti malintenzionati che tentano di accedere all'account, fino a quando non hanno accesso al computer in uso. È possibile farlo in qualsiasi computer privati che vengono utilizzate regolarmente. Impostando **memorizzare questo browser**, si ottiene le funzionalità di sicurezza di 2FA dai computer non si usa regolarmente, e si ottiene la comodità nel non dover passare attraverso 2FA nei propri computer. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Come registrare un provider di autenticazione a due fattori

Quando si crea un nuovo progetto MVC, il *IdentityConfig.cs* file contiene il codice seguente per registrare un provider di autenticazione a due fattori:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Aggiungere un numero di telefono autenticazione 2fa

Il `AddPhoneNumber` metodo di azione di `Manage` controller genera un token di sicurezza e inviati per il telefono numero che è sono specificato.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Dopo avere inviato il token, reindirizza al `VerifyPhoneNumber` metodo di azione, in cui è possibile immettere il codice per la registrazione SMS autenticazione 2fa. 2FA SMS non viene utilizzato fino a quando non è stato verificato il numero di telefono.

## <a name="enabling-2fa"></a>Abilitare 2FA

Il `EnableTFA` 2FA consente a metodo di azione:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Si noti il `SignInAsync` deve essere chiamato perché abilitare 2FA viene apportata una modifica al profilo di sicurezza. Quando è abilitato 2FA, l'utente dovrà usare 2FA per l'accesso, utilizzando gli approcci 2FA ha registrato (SMS e posta elettronica nell'esempio).

È possibile aggiungere ulteriori provider 2FA, ad esempio i generatori di codice a matrice oppure è possibile scrivere si è proprietari (vedere [tramite Google Authenticator con ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> I codici 2FA vengono generati usando [basati sul tempo algoritmo Password monouso](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) e i codici sono validi per sei minuti. Se si hanno più di sei minuti per immettere il codice, si riceverà un messaggio di errore di codice non valido.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Combinare gli account di accesso social e locali

È possibile combinare account locali e basati su social network, fare clic sul collegamento nel messaggio. Nella sequenza seguente &quot; RickAndMSFT@gmail.com &quot; viene innanzitutto creato un account di accesso locale, ma è possibile creare l'account come log basati su social network in prima, quindi aggiungere un account di accesso locale.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Fare clic sui **Gestisci** collegamento. Si noti 0 esterno (account di accesso social) associata all'account.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Fare clic sul collegamento a un altro registro nel servizio e accettare le richieste di app. I due account sono stati combinati, sarà in grado di accedere con uno di questi account. È possibile che gli utenti per aggiungere gli account locali nel caso in cui i log basati su social network in servizio di autenticazione è inattivo oppure più probabile che si è perso l'accesso al proprio account di social networking.

Nell'immagine seguente, Tom è una procedura di accesso basati su social network (che è possibile visualizzare dal **account di accesso esterni: 1** visualizzato sulla pagina).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Facendo clic su **Scegli una password** consente di aggiungere un registro locale in associati con lo stesso account.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Blocco degli account da attacchi di forza bruta

È possibile proteggere gli account nella tua app da attacchi con dizionario, consentendo di blocco dell'utente. Il codice seguente il `ApplicationUserManager Create` metodo configura blocco out:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Il codice riportato sopra consente di blocco per solo l'autenticazione a due fattori. Sebbene sia possibile attivare è stata bloccata per gli account di accesso modificando `shouldLockout` su true nel `Login` metodo del controller di account, è consigliabile non è stata bloccata per gli account di accesso è attivato perché rende vulnerabili per l'account [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) account di accesso gli attacchi. Nell'esempio di codice, blocco è disabilitato per l'account amministratore creato nel `ApplicationDbInitializer Seed` metodo:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Che l'utente dispone di un account di posta elettronica convalidato

Il codice seguente richiede un utente di un account di posta elettronica convalidato per poter accedere:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Come le verifiche per requisito 2FA SignInManager

Accesso locale sia basati su social network log di controllo per verificare se è abilitato 2FA. Se è abilitato 2FA, il `SignInManager` metodo di accesso restituisce `SignInStatus.RequiresVerification`, e l'utente verrà reindirizzato al `SendCode` metodo di azione, in cui è necessario immettere il codice per completare il log in sequenza. Se l'utente ha RememberMe è nastavit il cookie di utenti locale, il `SignInManager` restituirà `SignInStatus.Success` e non avranno passino 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Il codice seguente illustra il `SendCode` metodo di azione. Oggetto [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) viene creato con tutti i metodi di 2FA abilitati per l'utente. Il [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) viene passato per il [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, che consente all'utente di selezionare l'approccio 2FA (in genere tramite posta elettronica e SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Dopo che l'utente registra l'approccio 2FA, il `HTTP POST SendCode` viene chiamato il metodo di azione, il `SignInManager` invia il codice 2FA e l'utente viene reindirizzato al `VerifyCode` metodo di azione in cui è possibile immettere il codice per completare l'accesso.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>Blocco 2FA

Sebbene sia possibile impostare il blocco degli account in tentativi non riusciti password di account di accesso, tale approccio rende l'account di accesso possibili attacchi [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) dei blocchi. È consigliabile che usare il blocco dell'account solo con 2FA. Quando la `ApplicationUserManager` viene creato, il codice di esempio imposta blocco 2FA e `MaxFailedAccessAttemptsBeforeLockout` a cinque. Una volta che un utente accede (tramite l'account locale o un account di social networking), viene archiviato ogni tentativo non riuscito in 2FA e se viene raggiunto il numero massimo di tentativi, l'utente viene bloccato cinque minuti (è possibile impostare il blocco tempo `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

- [Risorse consigliate su ASP.NET Identity](../getting-started/aspnet-identity-recommended-resources.md) elenco completo dei blog di identità, video, esercitazioni e great quindi collega.
- [App MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) viene inoltre illustrato come aggiungere informazioni sul profilo per la tabella degli utenti.
- [ASP.NET MVC e identità 2.0: nozioni di base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) da John Atten.
- [Conferma account e recupero della Password con ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Introduzione ad ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Annuncio della versione RTM di ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) di Pranav Rastogi.
- [Identità di ASP.NET 2.0: Configurazione di convalida dell'Account e l'autorizzazione a due fattori](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) da John Atten.
