---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Creare un'app web ASP.NET MVC 5 sicura con accesso, inviare tramite posta elettronica di conferma e reimpostazione della password (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come compilare un'app web ASP.NET MVC 5 con conferma tramite posta elettronica e reimpostazione della password usando il sistema di appartenenze ASP.NET Identity. È ca...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 56c1a5c414fdcece8d827d1187144b4948d8eb93
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387043"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Creare un'app web ASP.NET MVC 5 sicura con accesso, inviare tramite posta elettronica di conferma e reimpostazione della password (c#)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Questa esercitazione illustra come compilare un'app web ASP.NET MVC 5 con conferma tramite posta elettronica e reimpostazione della password usando il sistema di appartenenze ASP.NET Identity. È possibile scaricare l'applicazione completata [qui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Il download contiene gli helper di debug che consentono di testare conferma tramite posta elettronica e SMS senza dover configurare un indirizzo di posta elettronica o il provider SMS.
> 
> Questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Creare un'app ASP.NET MVC

Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.

> [!NOTE]
> Avviso: È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.


1. Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC. Web Form supporta inoltre ASP.NET Identity, pertanto è possibile seguire la procedura in un'applicazione web form.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Lasciare l'autenticazione predefinita come **singoli account utente di**. Se vuoi ospitare l'app in Azure, lasciare selezionata la casella di controllo. Più avanti nell'esercitazione verrà distribuita in Azure. È possibile [aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Impostare il [progetto per l'uso di SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Eseguire l'app, scegliere il **registrare** collegare e registrare un utente. A questo punto, l'unica convalida il messaggio di posta elettronica è con il [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attributo.
5. In Esplora Server, passare a **dati Connections\DefaultConnection\Tables\AspNetUsers**, fare clic e selezionare **Apri definizione tabella**.

    La figura seguente mostra la `AspNetUsers` dello schema:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Fare clic con il pulsante destro sul **AspNetUsers** tabelle e selezionare **Mostra dati tabella**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 A questo punto non è stato confermato l'indirizzo di posta elettronica.
7. Fare clic sulla riga e selezionare Elimina. Si sarà aggiungere di nuovo questo messaggio di posta elettronica nel passaggio successivo e inviare un messaggio di posta elettronica di conferma.

## <a name="email-confirmation"></a>Conferma tramite posta elettronica

È consigliabile verificare che il messaggio di posta elettronica di una nuova registrazione utente per verificare che non rappresentano un altro utente (vale a dire non hanno registrato con un altro messaggio di posta elettronica). Si supponga di un forum di discussione, si desidera impedire `"bob@example.com"` tramite la registrazione come `"joe@contoso.com"`. Senza conferma tramite posta elettronica, `"joe@contoso.com"` è stato possibile ottenere l'indirizzo di posta elettronica indesiderato dalla propria app. Si supponga che Bob accidentalmente registrato come `"bib@example.com"` e non l'aveste notato, egli non sarebbe in grado di utilizzare ripristino password perché l'app non ha il suo indirizzo di posta elettronica corretto. Conferma tramite posta elettronica fornisce solo una protezione limitata dai Bot e non fornisce protezione da spammer determinato, hanno molti alias di posta elettronica di lavoro che possono usare per registrare.

In genere si desidera impedire ai nuovi utenti dalla registrazione di tutti i dati al sito web prima che sono state confermate tramite posta elettronica, un messaggio SMS o un altro meccanismo. <a id="build"></a>Nelle sezioni seguenti, verrà Abilita conferma tramite posta elettronica e modificare il codice per impedire l'accesso fino a quando non è stata confermata la posta elettronica gli utenti appena registrati.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Associare SendGrid

Sebbene in questa esercitazione illustra solo come aggiungere notifica tramite posta elettronica tramite [SendGrid](http://sendgrid.com/), è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi (vedere [risorse aggiuntive](#addRes)).

1. Nella Console di gestione pacchetti immettere il comando seguente: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Andare alla [pagina di iscrizione Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) e registrare un account SendGrid gratuito. Configurare SendGrid aggiungendo codice analogo al seguente nella *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

È necessario aggiungere che include quanto segue:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Per semplificare questo esempio, le impostazioni dell'app in verrà archiviato il *Web. config* file:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Security - store mai i dati sensibili nel codice sorgente. L'account e le credenziali vengono archiviate in appSetting. In Azure, è possibile archiviare in modo sicuro questi valori sul **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** scheda nel portale di Azure. Visualizzare [procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Abilitare la conferma tramite posta elettronica nel controller Account

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Verificare i *Views\Account\ConfirmEmail.cshtml* file ha la sintassi razor corretto. (Il @ carattere nella prima riga potrebbe essere manca. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Eseguire l'app e fare clic sul collegamento di registrazione. Dopo aver inviato il modulo di registrazione, si è connessi.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Verificare l'account di posta elettronica e fare clic sul collegamento per confermare l'indirizzo di posta elettronica.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Richiedere conferma tramite posta elettronica prima dell'accesso in

Attualmente una volta che un utente ha completato il modulo di registrazione, vengono registrate. In genere si vuole verificare la posta elettronica prima archiviandoli. Nella sezione seguente, si modificherà il codice per richiedere agli utenti di nuovo per disporre di un messaggio di posta elettronica confermato prima vengono registrate nel (autenticato). Aggiornamento di `HttpPost Register` metodo con le modifiche evidenziate di seguito:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Impostando come commento il `SignInAsync` metodo, l'utente non essere firmata in per la registrazione. Il `TempData["ViewBagLink"] = callbackUrl;` riga può essere usata per [il debug dell'app](#dbg) e registrazione di test senza l'invio di posta elettronica. `ViewBag.Message` Consente di visualizzare le istruzioni di conferma. Il [Scarica esempio](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contiene il codice per test conferma tramite posta elettronica senza configurare posta elettronica e consente inoltre il debug dell'applicazione.

Creare un `Views\Shared\Info.cshtml` file e aggiungere il markup razor seguente:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Aggiungere il [attributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) per il `Contact` metodo di azione del controller Home. È possibile fare clic sui **contatto** collegamento per verificare gli utenti anonimi non hanno accesso e si dispongono dell'accesso agli utenti autenticati.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

È necessario aggiornare anche il `HttpPost Login` metodo di azione:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Aggiorna il *Views\Shared\Error.cshtml* visualizzazione per visualizzare il messaggio di errore:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Eliminare gli account nel **AspNetUsers** tabella che contiene l'alias di posta elettronica si desidera eseguire il test. Eseguire l'app e verificare che non è possibile accedere fino a quando non si è verificato l'indirizzo di posta elettronica. Dopo aver verificato l'indirizzo di posta elettronica, scegliere il **contatto** collegamento.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Ripristino/reimpostazione della password

Rimuovere i caratteri di commento dal `HttpPost ForgotPassword` metodo di azione nel controller account:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Rimuovere i caratteri di commento dal `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) nel *Views\Account\Login.cshtml* file di visualizzazione razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Nella pagina di accesso mostrerà ora un link per reimpostare la password.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Inviare di nuovo collegamento di conferma tramite posta elettronica

Una volta che un utente crea un nuovo account locale, essi vengono inviati tramite posta elettronica un collegamento di conferma che viene richiesto di usare prima di poter accedere. Se l'utente accidentalmente Elimina il messaggio di posta elettronica di conferma o non arrivi mai il messaggio di posta elettronica, è necessario il collegamento di conferma inviato nuovamente. Le modifiche al codice seguente viene illustrato come abilitare questa opzione.

Aggiungere il seguente metodo helper a fondo le *Controllers\AccountController.cs* file:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Aggiornare il metodo Register per usare il nuovo helper:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Aggiornare il metodo di accesso per inviare di nuovo la password se non è stato confermato l'account utente:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combinare gli account di accesso social e locali

È possibile combinare account locali e basati su social network, fare clic sul collegamento nel messaggio. Nella sequenza seguente **RickAndMSFT@gmail.com** viene innanzitutto creato un account di accesso locale, ma è possibile creare l'account come log basati su social network in prima, quindi aggiungere un account di accesso locale.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Fare clic sui **Gestisci** collegamento. Si noti il **account di accesso esterni: 0** associate all'account.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Fare clic sul collegamento a un altro registro nel servizio e accettare le richieste di app. I due account sono stati combinati, sarà in grado di accedere con uno di questi account. È possibile che gli utenti per aggiungere gli account locali nel caso in cui i log basati su social network in servizio di autenticazione è inattivo oppure più probabile che si è perso l'accesso al proprio account di social networking.

Nell'immagine seguente, Tom è una procedura di accesso basati su social network (che è possibile visualizzare dal **account di accesso esterni: 1** visualizzato sulla pagina).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Facendo clic su **Scegli una password** consente di aggiungere un registro locale in associati con lo stesso account.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Conferma tramite posta elettronica in modo più approfondito

L'esercitazione [conferma Account e recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entra in questo argomento con altri dettagli.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debug dell'app

Se non si riceve un messaggio di posta elettronica che contiene il collegamento:

- Controllare la cartella della posta indesiderata o antispam.
- Accedere all'account SendGrid e fare clic sui [collegamento di attività di posta elettronica](https://sendgrid.com/logs/index).

Per testare il collegamento per la verifica senza messaggio di posta elettronica, scaricare il [esempio completato](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Nella pagina verranno visualizzati il collegamento di conferma e codici di conferma.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Risorse consigliate su collegamenti ad ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Conferma dell'account e recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) descrivono in maggiore dettaglio nella conferma account e di ripristino della password.
- [App MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) questa esercitazione illustra come scrivere un'app ASP.NET MVC 5 con Facebook e Google OAuth 2 l'autorizzazione. Viene inoltre illustrato come aggiungere dati aggiuntivi al database di identità.
- [Distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e Database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Questa esercitazione aggiunge una distribuzione di Azure, come proteggere l'app con i ruoli, come usare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.
- [Creazione di un'app Google per OAuth 2 e ci si connette l'app al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Creazione dell'app in Facebook e ci si connette l'app al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configurare SSL nel progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
