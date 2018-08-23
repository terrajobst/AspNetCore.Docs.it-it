---
title: Abilitare la generazione di codice a matrice per le app di autenticazione TOTP in ASP.NET Core
author: rick-anderson
description: Informazioni su come abilitare la generazione di codice a matrice per le app di autenticazione TOTP che funzionano con l'autenticazione a due fattori di ASP.NET Core.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 4535efdde7340436c6a508848bff86e103df570e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834408"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>Abilitare la generazione di codice a matrice per le app di autenticazione TOTP in ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

I codici a matrice richiede ASP.NET Core 2.0 o versione successiva.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core viene fornito con supporto per le applicazioni di authenticator per la singola autenticazione. Due factor authentication (2FA) le app di autenticazione, usando un basati sul tempo monouso Password algoritmo (TOTP), sono il settore approccio autenticazione 2fa consigliato. 2FA usando TOTP è preferibile a SMS 2FA. Un'app di autenticazione fornisce un codice di 6-8 cifre che gli utenti devono immettere dopo avere verificato il nome utente e password. In genere un'app di autenticazione viene installata in uno Smartphone.

I modelli di app web ASP.NET Core supportano gli autenticatori, ma non offrono supporto per la generazione di QRCode. I generatori di QRCode facilitano l'installazione di 2FA. Questo documento illustra l'aggiunta [codice a matrice](https://wikipedia.org/wiki/QR_code) generazione alla pagina di configurazione 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Aggiunta di codici a matrice per la pagina di configurazione 2FA

Usano queste istruzioni *qrcode.js* dal https://davidshimjs.github.io/qrcodejs/ repository.

* Scaricare il [libreria javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) per il `wwwroot\lib` cartella nel progetto.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Seguire le istruzioni in [identità Scaffold](xref:security/authentication/scaffold-identity) generare */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.
* Nelle */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, individuare il `Scripts` sezione alla fine del file:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Nelle *Pages/Account/Manage/EnableAuthenticator.cshtml* (pagine Razor) o *Views/Manage/EnableAuthenticator.cshtml* (MVC), individuare il `Scripts` sezione alla fine del file:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Aggiorna il `Scripts` sezione per aggiungere un riferimento al `qrcodejs` libreria è stato aggiunto e una chiamata per generare il codice a matrice. Dovrebbe apparire come segue:

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

Eseguire l'app e assicurarsi che è possibile Scansionare il codice e convalidare il codice che dimostra l'autenticatore.

## <a name="change-the-site-name-in-the-qr-code"></a>Modificare il nome del sito nel codice a matrice

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Il nome del sito nel codice a matrice viene ottenuto dal nome del progetto che scelto durante la creazione iniziale del progetto. È possibile modificarlo cercando il `GenerateQrCodeUri(string email, string unformattedKey)` metodo nella */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Il nome del sito nel codice a matrice viene ottenuto dal nome del progetto che scelto durante la creazione iniziale del progetto. È possibile modificarlo mediante la ricerca del `GenerateQrCodeUri(string email, string unformattedKey)` metodo nella *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* file (pagine Razor) o il *Controllers/ManageController.cs* file (MVC).

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Il codice predefinito dal modello è simile al seguente:

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Il secondo parametro nella chiamata a `string.Format` è il nome del sito, tratto dal nome della soluzione. Può essere modificata in qualsiasi valore, ma deve sempre essere codificato in URL.

## <a name="using-a-different-qr-code-library"></a>Usando un'altra libreria di codice a matrice

È possibile sostituire la libreria di codice a matrice con la libreria preferita. Il codice HTML contiene un `qrCode` elemento in cui è possibile inserire un codice a matrice da qualsiasi meccanismo nella libreria siano forniti.

L'URL formattato in modo corretto per il codice a matrice è disponibile nel:

* `AuthenticatorUri` proprietà del modello.
* `data-url` proprietà di `qrCodeData` elemento.

## <a name="totp-client-and-server-time-skew"></a>TOTP client e server sfasamento dell'ora

Autenticazione TOTP (basati sul tempo One-Time Password) dipende da dispositivo authenticator sia il server con l'ora esatta. Token durano solo per 30 secondi. Se gli account di accesso 2FA TOTP hanno esito negativo, verificare che l'ora del server è precisa e preferibilmente sincronizzato a un servizio NTP accurato.

::: moniker-end
