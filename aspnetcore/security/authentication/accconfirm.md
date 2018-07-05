---
title: Conferma account e recupero della password in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803272"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Conferma account e recupero della password in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)

Questa esercitazione illustra come compilare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password. Questa esercitazione viene **non** un argomento di inizio. È necessario avere familiarità con:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticazione](xref:security/authentication/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Visualizzare [questo file PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) per le versioni 2.x e ASP.NET Core MVC 1.1.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Creare un nuovo progetto ASP.NET Core con .NET Core CLI

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

* `--auth Individual` Specifica il modello di progetto di account utente individuali.
* In Windows, aggiungere il `-uld` opzione. Specifica che deve essere usato Local DB invece di SQLite.
* Eseguire `new mvc --help` per ottenere assistenza su questo comando.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se si usa il comando o SQLite, eseguire il comando seguente in una finestra di comando:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Specifica il modello di progetto di account utente individuali.
* In Windows, aggiungere il `-uld` opzione. Specifica che deve essere usato Local DB invece di SQLite.
* Eseguire `new mvc --help` per ottenere assistenza su questo comando.

---

In alternativa, è possibile creare un nuovo progetto ASP.NET Core con Visual Studio:

* In Visual Studio, creare una nuova **applicazione Web** progetto.
* Selezionare **ASP.NET Core 2.0**. **.NET core** sia selezionata nell'immagine seguente, ma è possibile selezionare **.NET Framework**.
* Selezionare **Modifica autenticazione** e impostare **singoli account utente di**.
* Mantenere il valore predefinito **nell'app dell'account utente di Store**.

![Finestra Nuovo progetto, che mostra "Radio di account utente individuali" selezionata](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Registrazione di nuovi utenti di test

Eseguire l'app, selezionare la **registrare** collegare e registrare un utente. Seguire le istruzioni per eseguire le migrazioni di Entity Framework Core. A questo punto, l'unica convalida il messaggio di posta elettronica è con il [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attributo. Dopo aver inviato la registrazione, si è connessi all'app. Più avanti nell'esercitazione, il codice venga aggiornato in modo che i nuovi utenti non è possibile accedere fino alla posta elettronica è stata convalidata.

## <a name="view-the-identity-database"></a>Visualizzare il database di identità

Visualizzare [utilizzare SQLite in un progetto ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) per istruzioni su come visualizzare i database SQLite.

Per Visual Studio:

* Dal **View** dal menu **Esplora oggetti di SQL Server** (SSOX).
* Passare a **(localdb) MSSQLLocalDB (Server SQL 13)**. Fare clic su **dbo. AspNetUsers** > **consente di visualizzare dati**:

![Menu di scelta rapida nella tabella AspNetUsers in Esplora oggetti di SQL Server](accconfirm/_static/ssox.png)

Si noti la tabella `EmailConfirmed` campo è `False`.

Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma. Fare doppio clic sulla riga e selezionare **Elimina**. L'eliminazione di alias di posta elettronica rende più semplice nella procedura seguente.

---

## <a name="require-https"></a>La richiesta di HTTPS

Visualizzare [Richiedi HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Richiedere conferma tramite posta elettronica

È consigliabile verificare che il messaggio di posta elettronica di una nuova registrazione utente. Inviare tramite posta elettronica di conferma consente di verificare che non sta rappresentando qualcun altro (vale a dire non hanno registrato con un altro messaggio di posta elettronica). Si supponga che si ha un forum di discussione e si desidera evitare che "yli@example.com"da registrare come"nolivetto@contoso.com". Senza conferma tramite posta elettronica, "nolivetto@contoso.com" può ricevere e-mail indesiderate dalla propria app. Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non l'aveste notato l'errore di ortografia di "yli". Essi non sarebbe in grado di utilizzare il recupero della password perché l'app non dispone di posta elettronica corretta. Conferma tramite posta elettronica fornisce solo una protezione limitata dal BOT. Conferma tramite posta elettronica non fornisce protezione da utenti malintenzionati con numero di account di posta elettronica.

In genere si desidera impedire ai nuovi utenti dalla registrazione di tutti i dati al sito web prima che abbiano un messaggio di posta elettronica confermato.

Aggiornamento `ConfigureServices` per richiedere un indirizzo di posta elettronica confermato:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` impedisce l'accesso fino a quando non viene confermata la posta elettronica agli utenti registrati.

### <a name="configure-email-provider"></a>Configurare il provider di posta elettronica

In questa esercitazione, SendGrid consente di inviare posta elettronica. È necessario un account SendGrid e una chiave per l'invio di posta elettronica. È possibile usare altri provider di posta elettronica. ASP.NET Core 2.x include `System.Net.Mail`, pertanto è possibile inviare tramite posta elettronica dall'app. È consigliabile che usare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica. SMTP sono difficili da proteggere e configurato correttamente.

Il [modello di opzioni](xref:fundamentals/configuration/options) viene utilizzato per accedere alle impostazioni account e la chiave dell'utente. Per altre informazioni, vedere [configurazione](xref:fundamentals/configuration/index).

Creare una classe per recuperare la chiave di protezione della posta elettronica. Per questo esempio, il `AuthMessageSenderOptions` classe viene creata nel *Services/AuthMessageSenderOptions.cs* file:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Impostare il `SendGridUser` e `SendGridKey` con il [strumento secret manager](xref:security/app-secrets). Ad esempio:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

In Windows, Secret Manager archivia le coppie di chiavi/valore in una *Secrets* del file nei `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.

Il contenuto del *Secrets* file non vengono crittografati. Il *Secrets* file è illustrato di seguito (il `SendGridKey` valore è stato rimosso.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Configurare l'avvio per usare AuthMessageSenderOptions

Aggiungere `AuthMessageSenderOptions` al contenitore del servizio alla fine del `ConfigureServices` metodo nella *Startup.cs* file:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Configurare la classe AuthMessageSender

Questa esercitazione illustra come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.

Installare il `SendGrid` pacchetto NuGet:

* Dalla riga di comando:

    `dotnet add package SendGrid`

* Dalla Console di gestione pacchetti, immettere il comando seguente:

  `Install-Package SendGrid`

Visualizzare [inizia gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account SendGrid gratuito.

#### <a name="configure-sendgrid"></a>Configurare SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Per configurare SendGrid, aggiungere codice simile al seguente nella *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Aggiungere il codice nel *Services/MessageServices.cs* simile al seguente per configurare SendGrid:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Abilitare il ripristino di conferma e la password account

Il modello presenta il codice per il ripristino di conferma e la password di account. Trovare il `OnPostAsync` nel metodo *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Impedire che gli utenti appena registrati viene connesso automaticamente impostando come commento la riga seguente:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Il metodo completo viene visualizzato con la riga modificata evidenziata:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Per abilitare la conferma dell'account, rimuovere il commento nel codice seguente:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Nota:** il codice è impedire che un utente appena registrato viene connesso automaticamente impostando come commento la riga seguente:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Abilita ripristino password rimuovendo il codice nel `ForgotPassword` azione di *AccountController*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Rimuovere il commento form *Views/Account/ForgotPassword.cshtml*. Si potrebbe voler rimuovere il `<p> For more information on how to enable reset password ... </p>` elemento che contiene un collegamento a questo articolo.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Registrare, messaggio di posta elettronica di conferma e reimpostazione della password

Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.

* Eseguire l'app e registrare un nuovo utente

  ![Visualizzazione Account registrazione dell'applicazione Web](accconfirm/_static/loginaccconfirm1.png)

* Controllare la posta elettronica per il collegamento di conferma di account. Visualizzare [eseguire il Debug tramite posta elettronica](#debug) se non si riceve il messaggio di posta elettronica.
* Fare clic sul collegamento per confermare l'indirizzo di posta elettronica.
* Accedere con l'indirizzo di posta elettronica e la password.
* Esegue la disconnessione.

### <a name="view-the-manage-page"></a>Visualizzare la pagina di gestione

Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)

Potrebbe essere necessario espandere la barra di spostamento per visualizzare il nome utente.

![Nella barra di spostamento](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

La pagina di gestione viene visualizzata con il **profilo** selezionato della scheda. Il **messaggio di posta elettronica** Mostra una casella di controllo che indica il messaggio di posta elettronica è stata confermata.

![pagina di gestione](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ciò viene indicato più avanti nell'esercitazione.
![pagina Gestisci](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Reimpostazione della password di test

* Se si è connessi, selezionare **Logout**.
* Selezionare il **accedere** collegamento e selezionare il **password dimenticata?** collegamento.
* Immettere l'indirizzo di posta elettronica usato per registrare l'account.
* Viene inviato un messaggio di posta elettronica con un collegamento di reimpostazione della password. Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password. Dopo che la password è stata reimpostata, è possibile accedere con l'indirizzo di posta elettronica e la nuova password.

<a name="debug"></a>

### <a name="debug-email"></a>Eseguire il debug tramite posta elettronica

Se non è possibile ottenere l'indirizzo di posta elettronica funzionante:

* Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Rivedere le [attività di posta elettronica](https://sendgrid.com/docs/User_Guide/email_activity.html) pagina.
* Controllare la cartella della posta indesiderata.
* Provare un altro alias di posta elettronica in un provider di posta elettronica diverso (Microsoft, Yahoo, Gmail e così via)
* Provare a inviare agli account di posta elettronica diverso.

**Una procedura consigliata** consiste nel **non** usare i segreti di produzione nel test e sviluppo. Se si pubblica l'app in Azure, è possibile impostare i segreti di SendGrid come impostazioni dell'applicazione nel portale di App Web di Azure. Il sistema di configurazione è configurato per leggere le chiavi da variabili di ambiente.

## <a name="combine-social-and-local-login-accounts"></a>Combinare gli account di accesso social e locali

Per completare questa sezione, è innanzitutto necessario abilitare un provider di autenticazione esterno. Visualizzare [Facebook, Google e l'autenticazione esterna provider](xref:security/authentication/social/index).

È possibile combinare account locali e basati su social network, fare clic sul collegamento nel messaggio. Nella sequenza seguente, "RickAndMSFT@gmail.com" viene prima creato un account di accesso locale; tuttavia, è possibile creare l'account come account di accesso basati su social network, prima di tutto, quindi aggiungere un account di accesso locale.

![Applicazione Web: RickAndMSFT@gmail.com utente autenticato](accconfirm/_static/rick.png)

Fare clic sui **Gestisci** collegamento. Si noti 0 esterno (account di accesso social) associata all'account.

![Gestione visualizzazione](accconfirm/_static/manage.png)

Fare clic sul collegamento a un altro servizio di accesso e accettare le richieste di app. Nell'immagine seguente, Facebook è il provider di autenticazione esterno:

![Gestire la visualizzazione di account di accesso esterni listato Facebook](accconfirm/_static/fb.png)

I due account sono stati combinati. Si è in grado di accedere con uno di questi account. È possibile che gli utenti per aggiungere gli account locali nel caso in cui il servizio di autenticazione account di accesso basati su social network è inattivo oppure più probabile che abbia perso l'accesso al proprio account di social networking.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Abilitare la conferma dell'account dopo un sito ha utenti

Abilitare la conferma dell'account in un sito con gli utenti per bloccare tutti gli utenti esistenti. Gli utenti esistenti sono bloccati perché i propri account non confermato. Per evitare il blocco degli utente esistente, usare uno degli approcci seguenti:

* Aggiornare il database per contrassegnare tutti gli utenti esistenti come viene confermata.
* Verificare che gli utenti in fase di chiusura. Ad esempio, batch-trasmissione messaggi di posta elettronica con collegamenti di conferma.
