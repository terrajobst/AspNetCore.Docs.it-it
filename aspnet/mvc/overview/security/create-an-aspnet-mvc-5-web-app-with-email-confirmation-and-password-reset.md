---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Creare un'app web di ASP.NET MVC 5 sicura con l'accesso, inviare tramite posta elettronica di conferma e reimpostazione della password (c#) | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione viene illustrato come compilare un'app web ASP.NET MVC 5 con conferma tramite posta elettronica e la password reimpostata utilizzando il sistema di appartenenze di ASP.NET Identity. Si autorità di certificazione..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 5689031015279484cc616090a767a8c25eefa3c1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Creare un'app web di ASP.NET MVC 5 sicura con l'accesso, inviare tramite posta elettronica di conferma e reimpostazione della password (c#)
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione viene illustrato come compilare un'app web ASP.NET MVC 5 con conferma tramite posta elettronica e la password reimpostata utilizzando il sistema di appartenenze di ASP.NET Identity. È possibile scaricare l'applicazione completata [qui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Il download contiene funzioni di supporto di debug che consentono di testare conferma tramite posta elettronica e SMS senza dover configurare un messaggio di posta elettronica o il provider SMS.
> 
> In questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Creare un'applicazione MVC ASP.NET

Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.

> [!NOTE]
> Avviso: È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.


1. Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC. Web Form supporta anche ASP.NET Identity, pertanto è possibile attenersi alla procedura simile in un'app di web form.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Lasciare l'autenticazione predefinita come **singoli account utente di**. Se si desidera ospitare l'applicazione in Azure, lasciare selezionata la casella di controllo. Più avanti nell'esercitazione si distribuirà in Azure. È possibile [aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Impostare il [progetto utilizzare SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Eseguire l'app, fare clic su di **registrare** collegare e registrare un utente. A questo punto, la convalida sola nel messaggio di posta elettronica è con il [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attributo.
5. In Esplora Server, passare a **dati Connections\DefaultConnection\Tables\AspNetUsers**fare clic destro del mouse e selezionare **Apri definizione tabella**.

    La figura seguente mostra il `AspNetUsers` dello schema:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Fare clic con il pulsante destro sul **AspNetUsers** tabella e selezionare **Mostra dati tabella**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 A questo punto il messaggio di posta elettronica non è stato confermato.
7. Fare clic sulla riga e selezionare Elimina. Verranno aggiungere di nuovo questo messaggio di posta elettronica nel passaggio successivo e inviare un messaggio di posta elettronica di conferma.

## <a name="email-confirmation"></a>Conferma tramite posta elettronica

È consigliabile verificare il messaggio di posta elettronica di una nuova registrazione utente per verificare che non rappresentano un altro utente (vale a dire, è ancora stato registrato con un altro messaggio di posta elettronica). Si supponga di un forum di discussione, si desidera impedire `"bob@example.com"` tramite la registrazione come `"joe@contoso.com"`. Senza conferma tramite posta elettronica, `"joe@contoso.com"` Impossibile ricevere la posta elettronica indesiderato dall'app. Si supponga che Bob accidentalmente registrato come `"bib@example.com"` e non veniva notare, ha non sarebbe in grado di utilizzare password Ripristina perché l'app non ha il suo messaggio di posta elettronica corretto. Conferma tramite posta elettronica fornisce a livello di protezione limitato da BOT e non offre protezione da spamming determinato, hanno molti alias di posta elettronica di lavoro che possono utilizzare per registrare.

In genere si desidera impedire agli utenti di nuovo di registrazione dei dati del sito web prima che siano stati confermati tramite posta elettronica, un SMS o un altro meccanismo. <a id="build"></a>Nelle sezioni riportate di seguito, si consentirà conferma tramite posta elettronica e modificare il codice per impedire l'accesso fino a quando non è stato confermato l'accesso alla posta elettronica di utenti appena registrati.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Collegare SendGrid

Sebbene questa esercitazione viene illustrato solo come aggiungere una notifica tramite posta elettronica tramite [SendGrid](http://sendgrid.com/), è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi (vedere [risorse aggiuntive](#addRes)).

1. Nella Console di gestione pacchetti, immettere quanto segue il comando seguente: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Passare al [pagina di iscrizione Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) e registrare gratuitamente account SendGrid. Aggiungere codice analogo al seguente per configurare SendGrid:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

È necessario aggiungere che include quanto segue:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Per semplificare questo esempio, si saranno archiviare le impostazioni di app nel *Web. config* file:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Sicurezza - archivio mai i dati sensibili nel codice sorgente. L'account e le credenziali vengono archiviate in appSetting. In Azure, è possibile archiviare questi valori in modo sicuro nel  **[configura](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  scheda nel portale di Azure. Vedere [procedure consigliate per la distribuzione delle password e altri dati sensibili su ASP.NET e in Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Abilitare la conferma tramite posta elettronica nel controller di Account

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Verificare il *Views\Account\ConfirmEmail.cshtml* file presenta la sintassi razor corretto. (Il @ carattere nella prima riga potrebbe essere manca. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Eseguire l'app e fare clic sul collegamento di registro. Dopo aver inviato il modulo di registrazione, si è connessi.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Verificare l'account di posta elettronica e fare clic sul collegamento per confermare la posta elettronica.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Richiedi conferma tramite posta elettronica prima dell'accesso in

Attualmente una volta che un utente ha completato il modulo di registrazione, vengono registrate. In genere, è necessario confermare la posta elettronica prima di registrarle. Nella sezione seguente, si modificherà il codice per richiedere nuovi utenti per disporre di un messaggio di posta elettronica di conferma vengono registrate nel (autenticato). Aggiornamento di `HttpPost Register` metodo con le seguenti modifiche evidenziate:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Commentando il `SignInAsync` (metodo), l'utente non sarà firmato per la registrazione. Il `TempData["ViewBagLink"] = callbackUrl;` riga può essere utilizzata per [debug dell'app](#dbg) e registrazione di test senza l'invio di posta elettronica. `ViewBag.Message`viene utilizzato per visualizzare le istruzioni di conferma. Il [Scarica esempio](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contiene il codice per test conferma tramite posta elettronica senza configurare posta elettronica e consente inoltre il debug dell'applicazione.

Creare un `Views\Shared\Info.cshtml` file e aggiungere il markup razor seguente:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Aggiungere il [attributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) per il `Contact` il metodo di azione del controller Home. È possibile utilizzare fare clic su di **contatto** collegamento per verificare gli utenti anonimi non hanno accesso e gli utenti autenticati dispone dell'accesso.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

È necessario aggiornare anche il `HttpPost Login` metodo di azione:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Aggiornamento di *Views\Shared\Error.cshtml* visualizzazione per visualizzare il messaggio di errore:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Eliminare gli account nel **AspNetUsers** la tabella contenente l'alias di posta elettronica che si desidera eseguire il test con. Eseguire l'app e verificare che non è possibile accedere fino a quando non è confermato l'indirizzo di posta elettronica. Dopo aver confermato l'indirizzo di posta elettronica, fare clic su di **contatto** collegamento.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Ripristino o la reimpostazione della password

Rimuovere i caratteri di commento dal `HttpPost ForgotPassword` il metodo di azione nel controller di account:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Rimuovere i caratteri di commento dal `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) nel *Views\Account\Login.cshtml* file di visualizzazione razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Nella pagina di accesso verrà visualizzati un collegamento per reimpostare la password.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Inviare di nuovo il collegamento di conferma tramite posta elettronica

Una volta che un utente crea un nuovo account locale, essi vengono inviati tramite posta elettronica un collegamento di conferma che devono utilizzare prima di poter accedere. Se l'utente accidentalmente Elimina il messaggio di posta elettronica di conferma o messaggio di posta elettronica non arriva mai, sarà necessario il collegamento di conferma inviato nuovamente. Le modifiche al codice seguente viene illustrato come abilitare questa opzione.

Aggiungere il seguente metodo helper a fondo il *Controllers\AccountController.cs* file:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Il metodo Register per utilizzare l'helper di nuovo l'aggiornamento:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Aggiornare il metodo di accesso per inviare nuovamente la password quando se non è stato confermato l'account utente:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combinare gli account di accesso social networking e locale

È possibile combinare gli account locali e sociali facendo clic sul collegamento nel messaggio. Nella sequenza seguente  **RickAndMSFT@gmail.com**  viene innanzitutto creato un account di accesso locale, ma è possibile creare l'account come un accesso social prima, quindi aggiungere un account di accesso locale.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Fare clic su di **Gestisci** collegamento. Si noti il valore 0 esterno (account di accesso social) associata all'account.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Fare clic sul collegamento a un altro registro nel servizio e accettare le richieste di app. I due account sono stati combinati, sarà possibile eseguire l'accesso con uno di questi account. Si consiglia agli utenti di aggiungere gli account locali nel caso in cui il log di social networking nel servizio di autenticazione è inattivo o più probabilmente è più possibile accedere al proprio account di social networking.

Nella figura seguente, Tom è una procedura di accesso social (che si può vedere dal **account di accesso esterni: 1** visualizzato nella pagina).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Facendo clic su **Scegli una password** consente di aggiungere un registro locale in associato lo stesso account.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Conferma tramite posta elettronica in modo più approfondito

L'esercitazione [la conferma dell'Account e Password di ripristino con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entra in questo argomento con maggiori dettagli.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debug dell'applicazione

Se non si ottiene un messaggio di posta elettronica contenente il collegamento:

- Controllare la cartella della posta indesiderata o posta indesiderata.
- Accedere al proprio account di SendGrid e fare clic su di [collegamento dell'attività di posta elettronica](https://sendgrid.com/logs/index).

Per verificare il collegamento di verifica senza messaggio di posta elettronica, scaricare il [esempio completo](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Nella pagina verranno visualizzati il collegamento di conferma e codici di conferma.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Collegamenti a ASP.NET Identity consigliato risorse](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Account di conferma e il recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) vengono descritti in dettaglio in Conferma password di ripristino e account.
- [App di MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) in questa esercitazione viene illustrato come scrivere un'applicazione ASP.NET MVC 5 con autorizzazione di Facebook e Google OAuth 2. Viene inoltre illustrato come aggiungere ulteriori dati al database di identità.
- [Distribuire un'app protetta ASP.NET MVC con appartenenza, OAuth e il Database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). In questa esercitazione aggiunge distribuzione di Azure, come proteggere le app con i ruoli, come utilizzare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.
- [Creazione di un'app di Google per OAuth 2 e l'applicazione di connessione al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Creazione di app in Facebook e l'applicazione di connessione al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Impostazione di SSL nel progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
