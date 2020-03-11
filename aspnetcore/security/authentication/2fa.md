---
title: Autenticazione a due fattori con SMS in ASP.NET Core
author: rick-anderson
description: Informazioni su come configurare l'autenticazione a due fattori (2FA) con un'app ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 424d21e446de02b39daa7685205a00931361b326
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661455"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Autenticazione a due fattori con SMS in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Swiss-sviluppatori](https://github.com/Swiss-Devs)

>[!WARNING]
> Due factor authentication (2FA) le app di autenticazione, usando un basati sul tempo monouso Password algoritmo (TOTP), sono il settore approccio autenticazione 2fa consigliato. 2FA usando TOTP è preferibile a SMS 2FA. Per altre informazioni, vedere [abilitare la generazione di codice a matrice per le app TOTP Authenticator in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) per ASP.NET Core 2,0 e versioni successive.

Questa esercitazione illustra come configurare l'autenticazione a due fattori (2FA) tramite SMS. Vengono fornite istruzioni per [Twilio](https://www.twilio.com/) e [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ma è possibile usare qualsiasi altro provider SMS. Prima di iniziare questa esercitazione, è consigliabile completare la [conferma dell'account e il recupero della password](xref:security/authentication/accconfirm) .

[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Come eseguire il download](xref:index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Creare un nuovo progetto ASP.NET Core

Creare una nuova app Web ASP.NET Core denominata `Web2FA` con singoli account utente. Per configurare e richiedere HTTPS, seguire le istruzioni riportate in <xref:security/enforcing-ssl>.

### <a name="create-an-sms-account"></a>Creare un account SMS

Creare un account SMS, ad esempio, da [Twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Registrare le credenziali di autenticazione (per twilio: accountSid e authToken, per ASPSMS: Userkey e una Password).

#### <a name="figuring-out-sms-provider-credentials"></a>Scoprire le credenziali del Provider SMS

**Twilio**

Dalla scheda Dashboard dell'account Twilio copiare il **SID dell'account** e il **token di autenticazione**.

**ASPSMS:**

Dalle impostazioni dell'account passare a **userKey** e copiarlo insieme alla **password**.

Questi valori vengono archiviati in un secondo momento con lo strumento di gestione dei segreti all'interno delle chiavi `SMSAccountIdentification` e `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Specifica l'ID mittente / creatore

**Twilio:** Dalla scheda numeri copiare il **numero di telefono**Twilio.

**ASPSMS:** Nel menu Sblocca autori sbloccare uno o più autori o scegliere un originatore alfanumerico (non supportato da tutte le reti).

Questo valore verrà archiviato in un secondo momento con lo strumento di gestione del segreto all'interno della chiave `SMSAccountFrom`.

### <a name="provide-credentials-for-the-sms-service"></a>Fornire le credenziali per il servizio SMS

Per accedere all'account utente e alle impostazioni della chiave verrà usato il [modello di opzioni](xref:fundamentals/configuration/options) .

* Creare una classe per recuperare la chiave sicura di SMS. Per questo esempio, la classe `SMSoptions` viene creata nel file *Services/SMSoptions. cs* .

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Impostare le `SMSAccountIdentification`, `SMSAccountPassword` e `SMSAccountFrom` con lo [strumento di gestione dei segreti](xref:security/app-secrets). Ad esempio:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* Aggiungere il pacchetto NuGet per il provider SMS. Dal Manager Console pacchetti eseguiti:

**Twilio**

`Install-Package Twilio`

**ASPSMS:**

`Install-Package ASPSMS`

* Aggiungere il codice nel file *Services/MessageServices. cs* per abilitare SMS. Usare la sezione ASPSMS o Twilio:

**Twilio**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Configurare l'avvio per l'uso di `SMSoptions`

Aggiungere `SMSoptions` al contenitore dei servizi nel metodo `ConfigureServices` in *Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Abilitare l'autenticazione a due fattori

Aprire il file di visualizzazione Razor *views/Manage/index. cshtml* e rimuovere i caratteri di commento, in modo che nessun markup sia impostato come commento.

## <a name="log-in-with-two-factor-authentication"></a>Accedere con l'autenticazione a due fattori

* Eseguire l'app e registrare un nuovo utente

![Visualizzazione Register dell'applicazione Web aperta in Microsoft Edge](2fa/_static/login2fa1.png)

* Toccare il nome utente, che attiva il metodo di azione `Index` in Gestisci controller. Toccare quindi il collegamento **Aggiungi** numero di telefono.

![Gestione visualizzazione - toccare il collegamento "Aggiungi"](2fa/_static/login2fa2.png)

* Aggiungere un numero di telefono che riceverà il codice di verifica e toccare **Invia codice di verifica**.

![Aggiungi pagina il numero di telefono](2fa/_static/login2fa3.png)

* Si riceverà un messaggio di testo con il codice di verifica. Immettilo e tocca **Invia**

![Numero di telefono pagina verifica](2fa/_static/login2fa4.png)

Se non si riceve un messaggio di testo, vedere pagina log twilio.

* La visualizzazione di gestione mostra che il numero di telefono è stato aggiunto correttamente.

![Gestione visualizzazione: numero di telefono è stato aggiunto](2fa/_static/login2fa5.png)

* Toccare **Abilita** per abilitare l'autenticazione a due fattori.

![Gestione visualizzazione - abilitare l'autenticazione a due fattori](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Test di autenticazione a due fattori

* Esegue la disconnessione.

* Eseguire l'accesso.

* L'account utente è abilitata l'autenticazione a due fattori, pertanto è necessario fornire il secondo fattore di autenticazione. In questa esercitazione è stata abilitata la verifica telefonica. I modelli predefiniti consentono inoltre di impostare la posta elettronica come secondo fattore. È possibile impostare altri fattori secondo per l'autenticazione, ad esempio i codici a matrice. Toccare **Invia**.

![Invio di visualizzazione del codice di verifica](2fa/_static/login2fa7.png)

* Immettere il codice che si ottiene il messaggio SMS.

* Quando si fa clic sulla casella di controllo **memorizza questo browser** , non è necessario usare 2FA per accedere quando si usa lo stesso dispositivo e browser. Abilitazione di 2FA e facendo clic su **ricorda questo browser** fornirà una protezione 2FA avanzata da utenti malintenzionati che tentano di accedere all'account, purché non dispongano dell'accesso al dispositivo. È possibile farlo in qualsiasi dispositivo privata che vengono utilizzate regolarmente. Impostando **ricorda questo browser**, si ottiene la maggiore sicurezza di 2FA dai dispositivi che non vengono usati regolarmente e si ottiene la convenienza di non dover passare attraverso 2FA nei propri dispositivi.

![Verificare la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Blocco degli account per la protezione da attacchi di forza bruta

Blocco degli account è consigliabile 2fa. Una volta che un utente accede tramite un account locale o un account di social networking, viene archiviato ogni tentativo non riuscito in 2FA. Se i tentativi di accesso non riusciti massima viene raggiunta, l'utente è bloccato (impostazione predefinita: blocco di 5 minuti dopo 5 tentativi di accesso non riusciti). Un'autenticazione riuscita Reimposta il numero di tentativi di accesso non riuscito e reimposta l'orologio. Il numero massimo di tentativi di accesso non riusciti e l'ora di blocco possono essere impostati con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) e [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). Di seguito consente di configurare il blocco degli account per 10 minuti dopo 10 tentativi di accesso non riusciti:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Confermare che [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) imposta `lockoutOnFailure` su `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
