---
title: Abilitare la generazione di codice a matrice per le app di autenticazione TOTP in ASP.NET Core
author: rick-anderson
description: Informazioni su come abilitare la generazione di codice a matrice per le app di autenticazione TOTP che funzionano con l'autenticazione a due fattori di ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/24/2017
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: b0d8f104119340b97bd65f1826bb921ca875acf8
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089971"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>Abilitare la generazione di codice a matrice per le app di autenticazione TOTP in ASP.NET Core

ASP.NET Core viene fornito con supporto per le applicazioni autenticatore per l'autenticazione singoli. Due fattori (2FA) autenticatore applicazioni per l'autenticazione, utilizzando un basati sul tempo monouso Password algoritmo (TOTP), sono il settore per 2FA approccio consigliato. 2FA utilizzando TOTP è preferibile a 2FA SMS. Un'app di autenticazione fornisce un codice di 6 a 8 cifre che gli utenti devono immettere dopo la conferma il nome utente e password. In genere un'app di autenticazione viene installata uno Smartphone.

I modelli di app web ASP.NET Core supportano gli autenticatori, ma non offrono supporto per la generazione di QRCode. I generatori QRCode facilitano l'installazione di 2FA. Questo documento illustra aggiunta [codice QR](https://wikipedia.org/wiki/QR_code) generazione alla pagina di configurazione 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Aggiunta di codici a matrice alla pagina di configurazione 2FA

Queste istruzioni si usa *qrcode.js* dal https://davidshimjs.github.io/qrcodejs/ repository.

* Scaricare il [libreria javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) per il `wwwroot\lib` cartella nel progetto.

* In *Pages\Account\Manage\EnableAuthenticator.cshtml* (pagine Razor) o *Views\Manage\EnableAuthenticator.cshtml* (MVC), individuare il `Scripts` sezione alla fine del file:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Aggiornamento di `Scripts` sezione per aggiungere un riferimento al `qrcodejs` libreria è stato aggiunto e da una chiamata per generare il codice a matrice. Dovrebbe apparire come segue:

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

* Eliminare il paragrafo che si collega a queste istruzioni.

Eseguire l'app e assicurarsi che è possibile Scansionare il codice e convalidare il codice che dimostra l'autenticatore.

## <a name="change-the-site-name-in-the-qr-code"></a>Modificare il nome del sito nel codice a matrice

Il nome del sito nel codice a matrice viene ricavato dal nome del progetto che scelto durante la creazione del progetto. È possibile modificarla mediante la ricerca del `GenerateQrCodeUri(string email, string unformattedKey)` metodo il *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* file (pagine Razor) o il *Controllers\ManageController.cs* file (MVC).

Il codice predefinito con il modello è simile al seguente:

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

Il secondo parametro nella chiamata a `string.Format` è il nome del sito, ricavato il nome della soluzione. Può essere modificata in qualsiasi valore, ma deve essere sempre codificati nell'URL.

## <a name="using-a-different-qr-code-library"></a>Utilizzo di una libreria di codice a matrice diversi

È possibile sostituire la libreria di codice a matrice con la libreria preferita. Il codice HTML contiene un `qrCode` elemento in cui è possibile inserire un codice a matrice da qualsiasi meccanismo nella libreria siano forniti.

L'URL formattato in modo corretto per il codice a matrice è disponibile nel:

* `AuthenticatorUri` proprietà del modello.
* `data-url` proprietà di `qrCodeData` elemento.

## <a name="totp-client-and-server-time-skew"></a>TOTP client e server sfasamento dell'ora

Autenticazione TOTP (basati sul tempo One-Time Password) dipende da dispositivo autenticatore sia del server con un'accurata del tempo. Token solo una durata di 30 secondi. Se gli account di accesso TOTP 2FA hanno esito negativo, verificare che l'ora del server sia preciso e preferibilmente sincronizzato a un servizio NTP accurato.
