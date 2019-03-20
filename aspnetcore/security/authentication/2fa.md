---
title: Autenticazione a due fattori con SMS in ASP.NET Core
author: rick-anderson
description: Informazioni su come configurare l'autenticazione a due fattori (2FA) con un'app ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 116249a7cd4faebd0c899e383d86f5c5c3c7146a
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265244"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Autenticazione a due fattori con SMS in ASP.NET Core

Dal [Rick Anderson](https://twitter.com/RickAndMSFT) e [Svizzera sviluppatori](https://github.com/Swiss-Devs)

>[!WARNING]
> Due factor authentication (2FA) le app di autenticazione, usando un basati sul tempo monouso Password algoritmo (TOTP), sono il settore approccio autenticazione 2fa consigliato. 2FA usando TOTP è preferibile a SMS 2FA. Per altre informazioni, vedere [generazione di abilitare la scansione del codice per le app di autenticazione TOTP in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) per ASP.NET Core 2.0 e versioni successive.

Questa esercitazione illustra come configurare l'autenticazione a due fattori (2FA) tramite SMS. Le istruzioni sono puramente [twilio](https://www.twilio.com/) e [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ma è possibile usare qualsiasi altro provider SMS. È consigliabile svolgere [conferma Account e recupero Password](xref:security/authentication/accconfirm) prima di iniziare questa esercitazione.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Come scaricare](xref:index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Creare un nuovo progetto ASP.NET Core

Creare una nuova app web ASP.NET Core denominata `Web2FA` con singoli account utente. Seguire le istruzioni in <xref:security/enforcing-ssl> per configurare e richiedere HTTPS.

### <a name="create-an-sms-account"></a>Creare un account SMS

Creare un account SMS, ad esempio, dal [twilio](https://www.twilio.com/) oppure [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Registrare le credenziali di autenticazione (per twilio: accountSid e authToken, per ASPSMS: UserKey e Password).

#### <a name="figuring-out-sms-provider-credentials"></a>Scoprire le credenziali del Provider SMS

**Twilio:**

Dalla scheda Dashboard dell'account Twilio, copiare il **SID dell'Account** e **token di autenticazione**.

**ASPSMS:**

Da impostazioni dell'account, passare a **Userkey** e copiarlo in combinazione con i **Password**.

In un secondo momento verranno archiviati questi valori con lo strumento secret manager tra quelle `SMSAccountIdentification` e `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Specifica l'ID mittente / creatore

**Twilio:** Dalla scheda numeri, copiare di Twilio **numero di telefono**.

**ASPSMS:** All'interno del Menu dei creatori sbloccare, sbloccare una o più entità di origine o scegliere un creatore alfanumerico (non supportato da tutte le reti).

Questo valore con lo strumento secret manager all'interno della chiave in un secondo momento verranno archiviati `SMSAccountFrom`.

### <a name="provide-credentials-for-the-sms-service"></a>Fornire le credenziali per il servizio SMS

Si userà il [modello di opzioni](xref:fundamentals/configuration/options) per accedere alle impostazioni e chiave dell'account utente.

* Creare una classe per recuperare la chiave sicura di SMS. Per questo esempio, il `SMSoptions` classe viene creata nel *Services/SMSoptions.cs* file.

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Impostare il `SMSAccountIdentification`, `SMSAccountPassword` e `SMSAccountFrom` con il [strumento secret manager](xref:security/app-secrets). Ad esempio:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* Aggiungere il pacchetto NuGet per il provider SMS. Dal Manager Console pacchetti eseguiti:

**Twilio:**

`Install-Package Twilio`

**ASPSMS:**

`Install-Package ASPSMS`

* Aggiungere il codice nel *Services/MessageServices.cs* file per consentire il SMS. Usare la sezione ASPSMS o Twilio:

**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Configurare l'avvio da usare `SMSoptions`

Aggiungere `SMSoptions` al contenitore del servizio nel `ConfigureServices` metodo nella *Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Abilitare l'autenticazione a due fattori

Aprire il *Views/Manage/Index.cshtml* file di visualizzazione Razor e rimuovere il commento caratteri (in modo non markup viene impostata come commento).

## <a name="log-in-with-two-factor-authentication"></a>Accedere con l'autenticazione a due fattori

* Eseguire l'app e registrare un nuovo utente

![Visualizzazione Register dell'applicazione Web aperta in Microsoft Edge](2fa/_static/login2fa1.png)

* Toccare il nome utente, che attiva il `Index` metodo di azione nel controller di gestione. Toccare il numero di telefono **Add** collegamento.

![Gestione visualizzazione - toccare il collegamento "Aggiungi"](2fa/_static/login2fa2.png)

* Aggiungere un numero di telefono che verrà visualizzato il codice di verifica e toccare **Invia codice di verifica**.

![Aggiungi pagina il numero di telefono](2fa/_static/login2fa3.png)

* Si riceverà un messaggio di testo con il codice di verifica. Immetterla e toccare **Submit**

![Numero di telefono pagina verifica](2fa/_static/login2fa4.png)

Se non si riceve un messaggio di testo, vedere pagina log twilio.

* La visualizzazione di gestione mostra che il numero di telefono è stato aggiunto correttamente.

![Gestione visualizzazione: numero di telefono è stato aggiunto](2fa/_static/login2fa5.png)

* Toccare **abilitare** per abilitare l'autenticazione a due fattori.

![Gestione visualizzazione - abilitare l'autenticazione a due fattori](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Test di autenticazione a due fattori

* Esegue la disconnessione.

* Accedi.

* L'account utente è abilitata l'autenticazione a due fattori, pertanto è necessario fornire il secondo fattore di autenticazione. In questa esercitazione è stata abilitata la verifica telefonica. I modelli predefiniti consentono inoltre di impostare la posta elettronica come secondo fattore. È possibile impostare altri fattori secondo per l'autenticazione, ad esempio i codici a matrice. Toccare **inviare**.

![Invio di visualizzazione del codice di verifica](2fa/_static/login2fa7.png)

* Immettere il codice che si ottiene il messaggio SMS.

* Facendo clic sui **memorizzare questo browser** casella di controllo sarà esente dalla necessità di utilizzare 2FA per l'accesso quando si usa lo stesso dispositivo e browser. Abilitare 2FA e facendo clic su **memorizzare questo browser** fornirà protezione 2FA sicuro da utenti malintenzionati che tentano di accedere all'account, fino a quando non hanno accesso al dispositivo. È possibile farlo in qualsiasi dispositivo privata che vengono utilizzate regolarmente. Impostando **memorizzare questo browser**, si ottiene le funzionalità di sicurezza di 2FA dai dispositivi non si usa regolarmente, e si ottiene la comodità nel non dover passare attraverso 2FA nei propri dispositivi.

![Verificare la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Blocco degli account per la protezione da attacchi di forza bruta

Blocco degli account è consigliabile 2fa. Una volta che un utente accede tramite un account locale o un account di social networking, viene archiviato ogni tentativo non riuscito in 2FA. Se i tentativi di accesso non riusciti massima viene raggiunta, l'utente è bloccato (impostazione predefinita: 5 al minuto blocco dopo 5 tentativi di accesso non riusciti). Un'autenticazione riuscita Reimposta il numero di tentativi di accesso non riuscito e reimposta l'orologio. Il numero massimo tentativi di accesso e tempo di blocco può essere impostata con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) e [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). Di seguito consente di configurare il blocco degli account per 10 minuti dopo 10 tentativi di accesso non riusciti:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Verificare che [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) imposta `lockoutOnFailure` a `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
