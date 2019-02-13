---
title: Conferma account e recupero della password in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password.
ms.author: riande
ms.date: 2/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 77d7b209d57f9ee44f158798ff780ce85c87aaf2
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56159408"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Conferma account e recupero della password in ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

Visualizzare [questo file PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) per ASP.NET Core 1.1 e versione 2.1.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)

Questa esercitazione illustra come compilare un'app ASP.NET Core con messaggio di posta elettronica conferma e reimpostazione della password. Questa esercitazione viene **non** un argomento di inizio. È necessario avere familiarità con:

* [ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)
* [Autenticazione](xref:security/authentication/identity)
* [Entity Framework Core](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a>Creare un'app web ed eseguire lo scaffolding di identità

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

* In Visual Studio, creare una nuova **applicazione Web** progetto denominato **WebPWrecover**.
* Selezionare **ASP.NET Core 2.1**.
* Mantenere il valore predefinito **Authentication** impostata su **Nessuna autenticazione**. L'autenticazione viene aggiunto nel passaggio successivo.

Nel passaggio successivo:

* Impostare la pagina di layout *~/Pages/Shared/_Layout.cshtml*
* Selezionare *Account/Register*
* Creare un nuovo **alla classe contesto dati**

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

Eseguire `dotnet aspnet-codegenerator identity --help` per ottenere informazioni sullo strumento di scaffolding.

------

Seguire le istruzioni in [abilitare l'autenticazione](xref:security/authentication/scaffold-identity#useauthentication):

* Aggiungere `app.UseAuthentication();` a `Startup.Configure`
* Aggiungere `<partial name="_LoginPartial" />` nel file di layout.

## <a name="test-new-user-registration"></a>Registrazione di nuovi utenti di test

Eseguire l'app, selezionare la **registrare** collegare e registrare un utente. A questo punto, l'unica convalida il messaggio di posta elettronica è con il [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attributo. Dopo aver inviato la registrazione, si è connessi all'app. Più avanti nell'esercitazione, il codice venga aggiornato in modo che i nuovi utenti non riesci ad accedere fino a quando non viene convalidata la posta elettronica.

[!INCLUDE[](~/includes/view-identity-db.md)]

Si noti la tabella `EmailConfirmed` campo è `False`.

Si potrebbe voler utilizzare nuovamente questo messaggio di posta elettronica nel passaggio successivo, quando l'app invia un messaggio di posta elettronica di conferma. Fare doppio clic sulla riga e selezionare **Elimina**. L'eliminazione di alias di posta elettronica rende più semplice nella procedura seguente.

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Richiedere conferma tramite posta elettronica

È consigliabile verificare che il messaggio di posta elettronica di una nuova registrazione utente. Inviare tramite posta elettronica di conferma consente di verificare che non sta rappresentando qualcun altro (vale a dire non hanno registrato con un altro messaggio di posta elettronica). Si supponga che si ha un forum di discussione e si desidera evitare che "yli@example.com"da registrare come"nolivetto@contoso.com". Senza conferma tramite posta elettronica, "nolivetto@contoso.com" può ricevere e-mail indesiderate dalla propria app. Si supponga che l'utente registrato accidentalmente come "ylo@example.com" e non l'aveste notato l'errore di ortografia di "yli". Essi non sarebbe in grado di utilizzare il recupero della password perché l'app non dispone di posta elettronica corretta. Conferma tramite posta elettronica fornisce una protezione limitata dal BOT. Conferma tramite posta elettronica non fornisce protezione da utenti malintenzionati con numero di account di posta elettronica.

In genere si desidera impedire ai nuovi utenti dalla registrazione di tutti i dati al sito web prima che abbiano un messaggio di posta elettronica confermato.

Update *Areas/Identity/IdentityHostingStartup.cs* per richiedere un indirizzo di posta elettronica confermato:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

`config.SignIn.RequireConfirmedEmail = true;` impedisce l'accesso fino a quando non viene confermata la posta elettronica agli utenti registrati.

### <a name="configure-email-provider"></a>Configurare il provider di posta elettronica

In questa esercitazione [SendGrid](https://sendgrid.com) viene usato per inviare posta elettronica. È necessario un account SendGrid e una chiave per l'invio di posta elettronica. È possibile usare altri provider di posta elettronica. ASP.NET Core 2.x include `System.Net.Mail`, pertanto è possibile inviare tramite posta elettronica dall'app. È consigliabile che usare SendGrid o un altro servizio di posta elettronica per inviare posta elettronica. SMTP sono difficili da proteggere e configurato correttamente.

Creare una classe per recuperare la chiave di protezione della posta elettronica. Per questo esempio, creare *Services/AuthMessageSenderOptions.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a>Configurare i segreti utente di SendGrid

Aggiungere un valore univoco `<UserSecretsId>` valore per il `<PropertyGroup>` elemento del file di progetto:

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

Impostare il `SendGridUser` e `SendGridKey` con il [strumento secret manager](xref:security/app-secrets). Ad esempio:

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
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
 
Per altre informazioni, vedere la [modello di opzioni](xref:fundamentals/configuration/options) e [configurazione](xref:fundamentals/configuration/index).

### <a name="install-sendgrid"></a>Installare SendGrid

Questa esercitazione illustra come aggiungere le notifiche di posta elettronica tramite [SendGrid](https://sendgrid.com/), ma è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi.

Installare il `SendGrid` pacchetto NuGet:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Dalla Console di gestione pacchetti, immettere il comando seguente:

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Dalla console, immettere il comando seguente:

```cli
dotnet add package SendGrid
```

------

Visualizzare [inizia gratuitamente con SendGrid](https://sendgrid.com/free/) per registrare un account SendGrid gratuito.
### <a name="implement-iemailsender"></a>Implementare IEmailSender

L'implementazione `IEmailSender`, creare *Services/EmailSender.cs* con codice simile al seguente:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a>Configurare l'avvio per il supporto tramite posta elettronica

Aggiungere il codice seguente per il `ConfigureServices` metodo nella *Startup.cs* file:

* Aggiungere `EmailSender` come un temporaneo del servizio.
* Registrare il `AuthMessageSenderOptions` istanza di configurazione.

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a>Abilitare il ripristino di conferma e la password account

Il modello presenta il codice per il ripristino di conferma e la password di account. Trovare il `OnPostAsync` nel metodo *Areas/Identity/Pages/Account/Register.cshtml.cs*.

Impedire che gli utenti appena registrati viene connesso automaticamente impostando come commento la riga seguente:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Il metodo completo viene visualizzato con la riga modificata evidenziata:

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a>Registrare, messaggio di posta elettronica di conferma e reimpostazione della password

Eseguire l'app web e testare la conferma dell'account e il flusso di ripristino password.

* Eseguire l'app e registrare un nuovo utente

  ![Visualizzazione Account registrazione dell'applicazione Web](accconfirm/_static/loginaccconfirm1.png)

* Controllare la posta elettronica per il collegamento di conferma di account. Visualizzare [eseguire il Debug tramite posta elettronica](#debug) se non si riceve il messaggio di posta elettronica.
* Fare clic sul collegamento per confermare l'indirizzo di posta elettronica.
* Accedere con l'indirizzo di posta elettronica e la password.
* Disconnettersi.

### <a name="view-the-manage-page"></a>Visualizzare la pagina di gestione

Selezionare il nome utente nel browser: ![finestra del browser con il nome utente](accconfirm/_static/un.png)

Potrebbe essere necessario espandere la barra di spostamento per visualizzare il nome utente.

![Nella barra di spostamento](accconfirm/_static/x.png)

La pagina di gestione viene visualizzata con il **profilo** selezionato della scheda. Il **messaggio di posta elettronica** Mostra una casella di controllo che indica il messaggio di posta elettronica è stata confermata.

### <a name="test-password-reset"></a>Reimpostazione della password di test

* Se si è connessi, selezionare **Logout**.
* Selezionare il **accedere** collegamento e selezionare il **password dimenticata?** collegamento.
* Immettere l'indirizzo di posta elettronica usato per registrare l'account.
* Viene inviato un messaggio di posta elettronica con un collegamento di reimpostazione della password. Controllare la posta elettronica e fare clic sul collegamento per reimpostare la password. Dopo che la password è stata reimpostata, è possibile accedere con l'indirizzo di posta elettronica e la nuova password.

<a name="debug"></a>

### <a name="debug-email"></a>Eseguire il debug tramite posta elettronica

Se non è possibile ottenere l'indirizzo di posta elettronica funzionante:

* Impostare un punto di interruzione `EmailSender.Execute` per verificare `SendGridClient.SendEmailAsync` viene chiamato.
* Creare un [app console per inviare posta elettronica](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) usando codice simile a `EmailSender.Execute`.
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
* Verificare che gli utenti esistenti. Ad esempio, batch-trasmissione messaggi di posta elettronica con collegamenti di conferma.

::: moniker-end
