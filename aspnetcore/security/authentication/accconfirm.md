---
title: La conferma dell'account e Password di ripristino in ASP.NET Core
author: rick-anderson
description: Viene illustrato come compilare un'app di ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
manager: wpickett
ms.author: riande
ms.date: 12/1/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 14c7fdfc1ed8b87aac8ca937298c7da6373bf06d
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>La conferma dell'account e il recupero della password in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette) 

In questa esercitazione viene illustrato come compilare un'app di ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.

## <a name="create-a-new-aspnet-core-project"></a>Creare un nuovo progetto ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Questo passaggio si applica a Visual Studio in Windows. Vedere la sezione successiva per le istruzioni di CLI.

L'esercitazione è necessario Visual Studio 2017 Preview 2 o versione successiva.

* In Visual Studio, creare un nuovo progetto applicazione Web.
* Selezionare **ASP.NET Core 2.0**. La figura seguente mostra **.NET Core** selezionata, ma è possibile selezionare **.NET Framework**.
* Selezionare **Modifica autenticazione** e impostato su **singoli account utente di**.
* Mantenere il valore predefinito **in-app dell'account utente di archivio**.

![Finestra Nuovo progetto con "Singoli account utente opzione"](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

L'esercitazione è necessario Visual Studio 2017 o versione successiva.

* In Visual Studio, creare un nuovo progetto applicazione Web.
* Selezionare **Modifica autenticazione** e impostato su **singoli account utente di**.

![Finestra Nuovo progetto con "Singoli account utente opzione"](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a>Creazione del progetto CLI di .NET core per macOS e Linux

Se si utilizza l'interfaccia CLI o SQLite, eseguire le operazioni seguenti in una finestra di comando:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual`Specifica il modello di singoli account utente.
* In Windows, aggiungere il `-uld` opzione. Il `-uld` opzione Crea una stringa di connessione del database locale anziché un database SQLite.
* Eseguire `new mvc --help` per ottenere assistenza su questo comando.

## <a name="test-new-user-registration"></a>Verifica registrazione nuovo utente

Eseguire l'app, selezionare il **registrare** collegare e registrare un utente. Seguire le istruzioni per eseguire le migrazioni di Entity Framework Core. A questo punto, la convalida sola nel messaggio di posta elettronica è con il [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attributo. Dopo aver inviato la registrazione, si è connessi all'app. Più avanti nell'esercitazione, verranno modificate in questo modo nuovi utenti non possono accedere finché non viene convalidata la posta elettronica.

## <a name="view-the-identity-database"></a>Visualizzare il database di identità

# <a name="sql-servertabsql-server"></a>[SQL Server](#tab/sql-server)

* Dal **vista** dal menu **Esplora oggetti di SQL Server** (sillaba SSOX). 
* Passare a **(localdb) MSSQLLocalDB (SQL Server 13)**. Fare clic su **dbo. AspNetUsers** > **visualizzare dati**:

![Menu di scelta rapida per la tabella AspNetUsers in Esplora oggetti di SQL Server](accconfirm/_static/ssox.png)

Si noti il `EmailConfirmed` campo `False`.

Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma. Fare doppio clic sulla riga e selezionare **eliminare**. Eliminare il messaggio di posta elettronica alias ora renderà più facile nei passaggi seguenti.

# <a name="sqlitetabsqlite"></a>[SQLite](#tab/sqlite)

Vedere [utilizzo di SQLite in un progetto ASP.NET MVC Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) per istruzioni su come visualizzare il database di SQLite. 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a>Richiedere SSL e l'installazione di IIS Express per SSL

Vedere [applicazione SSL](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Richiedi conferma tramite posta elettronica

È consigliabile verificare il messaggio di posta elettronica di una nuova registrazione utente per verificare che non viene rappresentata un'altra persona (vale a dire, è ancora stato registrato con un altro messaggio di posta elettronica). Si supponga che si dispone di un forum di discussione e si desidera impedire "yli@example.com"tramite la registrazione come"nolivetto@contoso.com." Senza conferma tramite posta elettronica, "nolivetto@contoso.com" Impossibile ricevere la posta elettronica indesiderato dall'app. Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non veniva rilevato l'errore di ortografia di "yli", sarebbe possibile usare il recupero della password, perché l'app non dispone di posta elettronica corretta. Conferma tramite posta elettronica fornisce a livello di protezione limitato da BOT e non offre protezione da spamming determinato che dispongono di molti alias di posta elettronica di lavoro che possono utilizzare per registrare.

In genere si desidera impedire agli utenti di nuovo di tutti i dati al sito web di registrazione prima che sia una conferma tramite posta elettronica. 

Aggiornamento `ConfigureServices` per richiedere una conferma tramite posta elettronica:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
La riga precedente impedisce registrati in corso la registrazione fino a quando non viene confermata la posta elettronica. Tuttavia, tale riga non impedisca nuovi utenti viene effettuato l'accesso dopo cui registrare. Il codice predefinito l'accesso utente dopo che la registrazione. Dopo l'accesso, non sarà in grado di accedere di nuovo fino a quando non vengono registrati. Più avanti in questa esercitazione verranno modificate sono l'utente di codice in modo appena registrato **non** effettuato l'accesso.

### <a name="configure-email-provider"></a>Configurare i provider di posta elettronica

In questa esercitazione, SendGrid viene utilizzato per inviare posta elettronica. È necessario un account SendGrid e una chiave per l'invio di posta elettronica. È possibile utilizzare altri provider di posta elettronica. ASP.NET Core 2. x include `System.Net.Mail`, che consente di inviare posta elettronica dalla tua app. È consigliabile che utilizzare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica. SMTP è difficile da proteggere e configurato correttamente.

Il [modello opzioni](xref:fundamentals/configuration/options) viene utilizzato per accedere alle impostazioni di account e la chiave utente. Per ulteriori informazioni, vedere [configurazione](xref:fundamentals/configuration/index).

Creare una classe per recuperare la chiave di proteggere la posta elettronica. Per questo esempio, il `AuthMessageSenderOptions` classe il *Services/AuthMessageSenderOptions.cs* file.

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Impostare il `SendGridUser` e `SendGridKey` con il [strumento segreto manager](../app-secrets.md). Ad esempio:

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

In Windows, gestione Secret archivia le coppie di chiavi/valore in un *secrets.json* file nella directory %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId >.

Il contenuto del *secrets.json* file non vengono crittografati. Il *secrets.json* file è illustrato di seguito (il `SendGridKey` valore è stato rimosso.)

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Configurare l'avvio per l'utilizzo di AuthMessageSenderOptions

Aggiungere `AuthMessageSenderOptions` al contenitore del servizio alla fine del `ConfigureServices` metodo il *Startup.cs* file:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Configurare la classe AuthMessageSender

In questa esercitazione viene illustrato come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.

* Installare il `SendGrid` pacchetto NuGet. Dalla Console di gestione pacchetti, immettere quanto segue il comando seguente:

  `Install-Package SendGrid`

* Vedere [iniziare gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account di SendGrid gratuito.

#### <a name="configure-sendgrid"></a>Configurare SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* Aggiungere il codice nel *Services/EmailSender.cs* simile al seguente per configurare SendGrid:

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* Aggiungere il codice nel *Services/MessageServices.cs* simile al seguente per configurare SendGrid:

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Abilitare il ripristino di conferma e la password di account

Il modello presenta il codice per il ripristino di conferma e la password di account. Trovare il `[HttpPost] Register` metodo il *AccountController.cs* file.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Impedire che i nuovi utenti registrati automaticamente l'accesso da impostare come commento la riga seguente:

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

Il metodo completo viene visualizzato con la riga modificata evidenziata:

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

Nota: Il codice precedente avrà esito negativo se si implementa `IEmailSender` e inviare un messaggio di testo normale. Vedere [questo problema](https://github.com/aspnet/Home/issues/2152) per ulteriori informazioni e una soluzione alternativa.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Rimuovere il commento il codice per consentire la conferma dell'account.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

Nota: È stiamo anche impedire un utente appena registrato automaticamente l'accesso da impostare come commento la riga seguente:

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Abilita ripristino password rimuovendo il codice di `ForgotPassword` azione nel *Controllers/AccountController.cs* file.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Rimuovere il commento modulo *Views/Account/ForgotPassword.cshtml*. È possibile rimuovere il `<p> For more information on how to enable reset password ... </p>` elemento che contiene un collegamento a questo articolo.

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Registrare, posta elettronica di conferma e reimpostare la password

Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.

* Eseguire l'app e registrare un nuovo utente

 ![Visualizzazione di registrare Account dell'applicazione Web](accconfirm/_static/loginaccconfirm1.png)

* Controllare la posta elettronica per il collegamento di conferma di account. Vedere [Debug posta elettronica](#debug) se non si ottiene il messaggio di posta elettronica.
* Fare clic sul collegamento per confermare la posta elettronica.
* Accedere con il messaggio di posta elettronica e la password.
* Esegue la disconnessione.

### <a name="view-the-manage-page"></a>Visualizzare la pagina di gestione

Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)

Potrebbe essere necessario espandere la barra di spostamento per visualizzare il nome utente.

![barra di spostamento](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Verrà visualizzata la pagina di gestione con il **profilo** scheda selezionata. Il **posta elettronica** Mostra una casella di controllo che indica il messaggio di posta elettronica è stata confermata. 

![pagina Gestisci](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Su questa pagina verrà descritto più avanti nell'esercitazione.
![pagina Gestisci](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Reimpostazione della password di test

* Se si è connessi, selezionare **Logout**.  
* Selezionare il **Accedi** collegamento e selezionare il **password dimenticata?** collegamento.
* Immettere il messaggio di posta elettronica che è utilizzato per registrare l'account.
* Verrà inviato un messaggio di posta elettronica con un collegamento per reimpostare la password. Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password.  Dopo che la password è stata reimpostata, può eseguire l'accesso con il messaggio di posta elettronica e la nuova password.

<a name="debug"></a>

### <a name="debug-email"></a>Eseguire il debug di posta elettronica

Se non è possibile ottenere l'utilizzo di posta elettronica:

* Esaminare il [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.
* Controllare la cartella della posta indesiderata.
* Provare un altro alias di posta elettronica su un provider diverso posta elettronica (Microsoft, Yahoo, Gmail, ecc.)
* Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Provare a inviare agli account di posta elettronica diverso.

**Nota:** è una procedura consigliata di non utilizzare i segreti di produzione nel test e sviluppo. Se si pubblica l'app in Azure, è possibile impostare i segreti SendGrid come impostazioni dell'applicazione nel portale di Azure Web App. Il sistema di configurazione è configurato per la lettura delle chiavi dalle variabili di ambiente.

## <a name="prevent-login-at-registration"></a>Impedire l'accesso al momento della registrazione

Con i modelli di correnti, quando un utente ha completato il modulo di registrazione, si è connessi (autenticato). In genere, è necessario confermare la posta elettronica prima di registrarle. Nella sezione seguente si modificherà il codice in modo da richiedere nuovi utenti dispongano di un messaggio di posta elettronica di conferma prima che si è connessi. Aggiornamento di `[HttpPost] Login` azione nel *AccountController.cs* file con le seguenti modifiche evidenziate.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

**Nota:** è una procedura consigliata di non utilizzare i segreti di produzione nel test e sviluppo. Se si pubblica l'app in Azure, è possibile impostare i segreti SendGrid come impostazioni dell'applicazione nel portale di Azure Web App. Il sistema di configurazione è configurato per la lettura delle chiavi dalle variabili di ambiente.


## <a name="combine-social-and-local-login-accounts"></a>Combinare gli account di accesso social networking e locale

Nota: In questa sezione si applica solo a ASP.NET di base 1. x. Per ASP.NET Core 2. x, vedere [questo](https://github.com/aspnet/Docs/issues/3753) problema.

Per completare questa sezione, è innanzitutto necessario abilitare i provider di autenticazione esterno. Vedere [Abilitazione autenticazione con Facebook, Google e altri provider esterni](social/index.md).

È possibile combinare gli account locali e sociali facendo clic sul collegamento nel messaggio. Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso social prima, quindi aggiungere un account di accesso locale.

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

Fare clic su di **Gestisci** collegamento. Si noti il valore 0 esterno (account di accesso social) associata all'account.

![Gestione visualizzazione](accconfirm/_static/manage.png)

Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app. Nell'immagine seguente, Facebook è il provider di autenticazione esterni:

![Gestire l'account di accesso esterni di visualizzazione elenco Facebook](accconfirm/_static/fb.png)

I due account sono stati combinati. Sarà in grado di accedere con uno di questi account. Si consiglia agli utenti di aggiungere gli account locali nel caso in cui il log di social networking nel servizio di autenticazione non è attivo, o più probabile hai più accesso al proprio account di social networking.
