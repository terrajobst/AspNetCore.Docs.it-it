---
title: Abilitare la generazione di codice a matrice per le app TOTP Authenticator in ASP.NET Core
author: rick-anderson
description: Scopri come abilitare la generazione di codice a matrice per le app TOTP Authenticator che funzionano con ASP.NET Core Autenticazione a due fattori.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: a7fdc86b3fe94e714e5147c89a32fce13757d1c1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665312"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>Abilitare la generazione di codice a matrice per le app TOTP Authenticator in ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

Per i codici QR è necessario ASP.NET Core 2,0 o versione successiva.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core fornisce supporto per le applicazioni di autenticazione per l'autenticazione singola. Due factor authentication (2FA) le app di autenticazione, usando un basati sul tempo monouso Password algoritmo (TOTP), sono il settore approccio autenticazione 2fa consigliato. 2FA usando TOTP è preferibile a SMS 2FA. Un'app Authenticator fornisce un codice da 6 a 8 cifre che gli utenti devono immettere dopo aver confermato il nome utente e la password. In genere, un'app autenticatore viene installata in uno smartphone.

I modelli di app Web ASP.NET Core supportano gli autenticatori, ma non forniscono il supporto per la generazione di QRCode. I generatori QRCode semplificano la configurazione di 2FA. Questo documento illustra come aggiungere la generazione di [codice](https://wikipedia.org/wiki/QR_code) a matrice alla pagina di configurazione di 2FA.

L'autenticazione a due fattori non viene eseguita con un provider di autenticazione esterno, ad esempio [Google](xref:security/authentication/google-logins) o [Facebook](xref:security/authentication/facebook-logins). Gli account di accesso esterni sono protetti da qualsiasi meccanismo fornito dal provider di accesso esterno. Si consideri, ad esempio, che il provider di autenticazione [Microsoft](xref:security/authentication/microsoft-logins) richiede una chiave hardware o un altro approccio 2FA. Se i modelli predefiniti applicavano "local" 2FA, gli utenti sarebbero necessari per soddisfare due approcci 2FA, che non è uno scenario di uso comune.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Aggiunta di codici QR alla pagina di configurazione di 2FA

Queste istruzioni usano *QRCode. js* dal repository https://davidshimjs.github.io/qrcodejs/.

* Scaricare la [libreria JavaScript QRCode. js](https://davidshimjs.github.io/qrcodejs/) nella cartella `wwwroot\lib` nel progetto.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Seguire le istruzioni in [impalcatura Identity](xref:security/authentication/scaffold-identity) per generare */areas/Identity/Pages/account/Manage/EnableAuthenticator.cshtml*.
* In */areas/Identity/Pages/account/Manage/EnableAuthenticator.cshtml*individuare la sezione `Scripts` alla fine del file:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* In *pages/account/Manage/EnableAuthenticator. cshtml* (Razor Pages) o *views/Manage/EnableAuthenticator. cshtml* (MVC) individuare la sezione `Scripts` alla fine del file:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Aggiornare la sezione `Scripts` per aggiungere un riferimento alla libreria di `qrcodejs` aggiunta e una chiamata per generare il codice a matrice. Dovrebbe avere un aspetto simile al seguente:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* Eliminare il paragrafo che fornisce collegamenti a queste istruzioni.

Eseguire l'app e verificare che sia possibile eseguire la scansione del codice a matrice e convalidare il codice dimostrato dall'autenticatore.

## <a name="change-the-site-name-in-the-qr-code"></a>Modificare il nome del sito nel codice a matrice

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Il nome del sito nel codice a matrice viene ricavato dal nome del progetto scelto durante la creazione iniziale del progetto. È possibile modificarlo cercando il metodo `GenerateQrCodeUri(string email, string unformattedKey)` in */areas/Identity/Pages/account/Manage/EnableAuthenticator.cshtml.cs*.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Il nome del sito nel codice a matrice viene ricavato dal nome del progetto scelto durante la creazione iniziale del progetto. È possibile modificarlo cercando il metodo `GenerateQrCodeUri(string email, string unformattedKey)` nel file *pages/account/Manage/EnableAuthenticator. cshtml. cs* (Razor Pages) o nel file *Controllers/ManageController. cs* (MVC).

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Il codice predefinito del modello è simile al seguente:

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenticatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Il secondo parametro nella chiamata a `string.Format` è il nome del sito, tratto dal nome della soluzione. Può essere modificato in qualsiasi valore, ma deve sempre essere codificato in URL.

## <a name="using-a-different-qr-code-library"></a>Uso di una libreria di codice QR diversa

È possibile sostituire la libreria di codici QR con la libreria preferita. Il codice HTML contiene un elemento `qrCode` in cui è possibile inserire un codice a matrice in base a qualsiasi meccanismo fornito dalla libreria.

L'URL correttamente formattato per il codice a matrice è disponibile in:

* `AuthenticatorUri` proprietà del modello.
* `data-url` proprietà nell'elemento `qrCodeData`.

## <a name="totp-client-and-server-time-skew"></a>Sfasamento dell'ora del client e del server TOTP

L'autenticazione TOTP (Time-based password monouso) dipende sia dal dispositivo server che dall'autenticatore che ha un'ora esatta. I token durano solo per 30 secondi. Se gli accessi 2FA di TOTP non riescono, verificare che l'ora del server sia accurata e preferibilmente sincronizzata con un servizio NTP accurato.

::: moniker-end
