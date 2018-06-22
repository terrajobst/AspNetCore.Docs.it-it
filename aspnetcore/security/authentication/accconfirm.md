---
title: La conferma dell'account e il recupero della password in ASP.NET Core
author: rick-anderson
description: Informazioni su come compilare un'app di ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: db41dd47518fa8b35c006b3291068e7724cf6cca
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275080"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>La conferma dell'account e il recupero della password in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)

In questa esercitazione viene illustrato come compilare un'app di ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password. Questa esercitazione è **non** un argomento di inizio. È necessario avere familiarità con:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticazione](xref:security/authentication/index)
* [Conferma account e recupero password](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Vedere [questo file PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) per le versioni 1.1 di MVC ASP.NET Core e 2. x.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Creare un nuovo progetto ASP.NET Core con l'interfaccia CLI di .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* `--auth Individual` Specifica il modello di progetto di singoli account utente.
* In Windows, aggiungere il `-uld` opzione. Specifica di che utilizzare LocalDB anziché SQLite.
* Eseguire `new mvc --help` per ottenere assistenza su questo comando.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se si utilizza l'interfaccia CLI o SQLite, eseguire le operazioni seguenti in una finestra di comando:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Specifica il modello di progetto di singoli account utente.
* In Windows, aggiungere il `-uld` opzione. Specifica di che utilizzare LocalDB anziché SQLite.
* Eseguire `new mvc --help` per ottenere assistenza su questo comando.

---

In alternativa, è possibile creare un nuovo progetto ASP.NET Core con Visual Studio:

* In Visual Studio, creare un nuovo **applicazione Web** progetto.
* Selezionare **ASP.NET Core 2.0**. **.NET core** sia selezionato nell'immagine seguente, ma è possibile selezionare **.NET Framework**.
* Selezionare **Modifica autenticazione** e impostato su **singoli account utente di**.
* Mantenere il valore predefinito **in-app dell'account utente di archivio**.

![Finestra Nuovo progetto con "Singoli account utente opzione"](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Verifica registrazione nuovo utente

Eseguire l'app, selezionare il **registrare** collegare e registrare un utente. Seguire le istruzioni per eseguire le migrazioni di Entity Framework Core. A questo punto, la convalida sola nel messaggio di posta elettronica è con il [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attributo. Dopo aver inviato la registrazione, si è connessi all'app. Più avanti nell'esercitazione, il codice viene aggiornato in modo non possono accedere nuovi utenti fino a quando non è stata convalidata alla posta elettronica.

## <a name="view-the-identity-database"></a>Visualizzare il database di identità

Vedere [utilizzare SQLite in un progetto MVC ASP.NET Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) per istruzioni su come visualizzare il database di SQLite.

Per Visual Studio:

* Dal **vista** dal menu **Esplora oggetti di SQL Server** (sillaba SSOX).
* Passare a **(localdb) MSSQLLocalDB (SQL Server 13)**. Fare clic su **dbo. AspNetUsers** > **visualizzare dati**:

![Menu di scelta rapida per la tabella AspNetUsers in Esplora oggetti di SQL Server](accconfirm/_static/ssox.png)

Si noti la tabella `EmailConfirmed` campo `False`.

Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma. Fare doppio clic sulla riga e selezionare **eliminare**. Eliminare l'alias di posta elettronica rende più semplice nei passaggi seguenti.

---

## <a name="require-https"></a>Utilizzo di HTTPS

Vedere [richiedono HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Richiedi conferma tramite posta elettronica

È consigliabile verificare il messaggio di posta elettronica di una nuova registrazione utente. Posta elettronica di conferma è utile per verificare che non viene rappresentata un'altra persona (vale a dire, è ancora stato registrato con un altro messaggio di posta elettronica). Si supponga che si dispone di un forum di discussione e si desidera impedire "yli@example.com"tramite la registrazione come"nolivetto@contoso.com". Senza conferma tramite posta elettronica, "nolivetto@contoso.com" Impossibile ricevere il messaggio di posta elettronica indesiderato dall'app. Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non veniva rilevato l'errore di ortografia di "yli". Essi non potranno utilizzare il recupero della password in quanto l'app non dispone di posta elettronica corretta. Conferma tramite posta elettronica fornisce solo protezione limitata da BOT. Conferma tramite posta elettronica non offre protezione da utenti malintenzionati con numero di account di posta elettronica.

In genere si desidera impedire agli utenti di nuovo di tutti i dati al sito web di registrazione prima che sia una conferma tramite posta elettronica.

Aggiornamento `ConfigureServices` per richiedere una conferma tramite posta elettronica:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` impedisce l'accesso fino a quando non viene confermata la posta elettronica agli utenti registrati.

### <a name="configure-email-provider"></a>Configurare i provider di posta elettronica

In questa esercitazione, SendGrid viene utilizzato per inviare posta elettronica. È necessario un account SendGrid e una chiave per l'invio di posta elettronica. È possibile utilizzare altri provider di posta elettronica. ASP.NET Core 2. x include `System.Net.Mail`, che consente di inviare posta elettronica dalla tua app. È consigliabile che utilizzare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica. SMTP è difficile da proteggere e configurato correttamente.

Il [modello opzioni](xref:fundamentals/configuration/options) viene utilizzato per accedere alle impostazioni di account e la chiave utente. Per ulteriori informazioni, vedere [configurazione](xref:fundamentals/configuration/index).

Creare una classe per recuperare la chiave di proteggere la posta elettronica. Per questo esempio, il `AuthMessageSenderOptions` classe il *Services/AuthMessageSenderOptions.cs* file:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Impostare il `SendGridUser` e `SendGridKey` con il [strumento segreto manager](xref:security/app-secrets). Ad esempio:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

In Windows, gestione Secret archivia coppie di chiavi/valore in un *secrets.json* file nel `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.

Il contenuto del *secrets.json* file non vengono crittografati. Il *secrets.json* file è illustrato di seguito (il `SendGridKey` valore è stato rimosso.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Configurare l'avvio per l'utilizzo di AuthMessageSenderOptions

Aggiungere `AuthMessageSenderOptions` al contenitore del servizio alla fine del `ConfigureServices` metodo il *Startup.cs* file:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Configurare la classe AuthMessageSender

In questa esercitazione viene illustrato come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.

Installare il `SendGrid` pacchetto NuGet:

* Dalla riga di comando:

    `dotnet add package SendGrid`

* Dalla Console di gestione pacchetti, immettere il comando seguente:

  `Install-Package SendGrid`

Vedere [iniziare gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account di SendGrid gratuito.

#### <a name="configure-sendgrid"></a>Configurare SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Per configurare SendGrid, aggiungere codice analogo al seguente nella *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Aggiungere il codice nel *Services/MessageServices.cs* simile al seguente per configurare SendGrid:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Abilitare il ripristino di conferma e la password di account

Il modello presenta il codice per il ripristino di conferma e la password di account. Trovare il `OnPostAsync` metodo *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Impedire che i nuovi utenti registrati automaticamente l'accesso da impostare come commento la riga seguente:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Il metodo completo viene visualizzato con la riga modificata evidenziata:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Per abilitare la conferma dell'account, rimuovere il commento nel codice seguente:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Nota:** il codice è impedire che un utente appena registrato viene connesso automaticamente da impostare come commento la riga seguente:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Abilita ripristino password rimuovendo il codice di `ForgotPassword` azione di *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Rimuovere il commento modulo *Views/Account/ForgotPassword.cshtml*. È possibile rimuovere il `<p> For more information on how to enable reset password ... </p>` elemento che contiene un collegamento a questo articolo.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

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

Questo è riportato più avanti nell'esercitazione.
![pagina Gestisci](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Reimpostazione della password di test

* Se si è connessi, selezionare **Logout**.
* Selezionare il **Accedi** collegamento e selezionare il **password dimenticata?** collegamento.
* Immettere il messaggio di posta elettronica che è utilizzato per registrare l'account.
* Viene inviato un messaggio di posta elettronica con un collegamento per reimpostare la password. Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password. Dopo che la password è stata reimpostata correttamente, è possibile accedere con il messaggio di posta elettronica e la nuova password.

<a name="debug"></a>

### <a name="debug-email"></a>Eseguire il debug di posta elettronica

Se non è possibile ottenere l'utilizzo di posta elettronica:

* Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Esaminare il [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.
* Controllare la cartella della posta indesiderata.
* Provare un altro alias di posta elettronica su un provider diverso posta elettronica (Microsoft, Yahoo, Gmail, ecc.)
* Provare a inviare agli account di posta elettronica diverso.

**Una procedura consigliata** è **non** Usa i segreti di produzione nel test e sviluppo. Se si pubblica l'app in Azure, è possibile impostare i segreti SendGrid come impostazioni dell'applicazione nel portale di Azure Web App. Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.

## <a name="combine-social-and-local-login-accounts"></a>Combinare gli account di accesso social networking e locale

Per completare questa sezione, è innanzitutto necessario abilitare i provider di autenticazione esterno. Vedere [Facebook, Google, autenticazione basata su provider esterni e](xref:security/authentication/social/index).

È possibile combinare gli account locali e sociali facendo clic sul collegamento nel messaggio. Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso social prima, quindi aggiungere un account di accesso locale.

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

Fare clic su di **Gestisci** collegamento. Si noti il valore 0 esterno (account di accesso social) associata all'account.

![Gestione visualizzazione](accconfirm/_static/manage.png)

Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app. Nella figura seguente, Facebook è il provider di autenticazione esterni:

![Gestire l'account di accesso esterni di visualizzazione elenco Facebook](accconfirm/_static/fb.png)

I due account sono stati combinati. Si è in grado di accedere con uno di questi account. Si consiglia agli utenti di aggiungere gli account locali nel caso in cui il servizio di autenticazione account di accesso social è inattivo o più probabile hai più accesso al proprio account di social networking.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Abilitare la conferma dell'account dopo che un sito con gli utenti

Consentire la conferma dell'account in un sito con utenti Blocca tutti gli utenti esistenti. Gli utenti esistenti vengono bloccati perché gli account non sono confermati. Per risolvere blocco utente esistente, utilizzare uno degli approcci seguenti:

* Aggiornare il database per contrassegnare tutti gli utenti esistenti come viene confermata.
* Verificare che gli utenti esistenti. Ad esempio, batch-trasmissione messaggi di posta elettronica con i collegamenti di conferma.
