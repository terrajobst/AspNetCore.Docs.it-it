---
title: Abilitare la generazione di codice a matrice per le app di autenticazione in ASP.NET Core
author: rick-anderson
description: Abilitare la generazione di codice a matrice per le app di autenticazione in ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 87a6d3f17216625e0f7ce206dddd72cb2f371e9a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>Abilitare la generazione di codice a matrice per le app di autenticazione in ASP.NET Core

Nota: In questo argomento si applica a ASP.NET Core 2. x

ASP.NET Core viene fornito con supporto per le applicazioni per l'autenticazione singoli autenticatore. Due fattori (2FA) di autenticazione le app di autenticazione, utilizzando un basati sul tempo monouso Password algoritmo (TOTP), sono nel settore dell'approccio per 2FA consigliato. 2FA utilizzando TOTP è preferibile a 2FA SMS. Un'app di autenticazione fornisce un codice di 6 a 8 cifre che gli utenti devono immettere dopo la conferma di nome utente e password. In genere un'app di autenticazione viene installata uno Smartphone.

I modelli di applicazione web ASP.NET Core supportano gli autenticatori, ma non forniscono supporto per la generazione di QRCode. I generatori QRCode facilitano l'installazione di 2FA. Questo documento illustra aggiunta [codice QR](https://wikipedia.org/wiki/QR_code) generazione alla pagina di configurazione 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Aggiunta di codici a matrice alla pagina di configurazione 2FA

Queste istruzioni si usa *qrcode.js* dal repository https://davidshimjs.github.io/qrcodejs/.

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

* Eliminare il paragrafo in cui vengono forniti collegamenti a queste istruzioni.

Eseguire l'app e assicurarsi che è possibile scansionare il codice a matrice e convalidare il codice che dimostra l'autenticatore.

## <a name="change-the-site-name-in-the-qr-code"></a>Modificare il nome del sito nel codice a matrice

Il nome del sito nel codice QR viene ricavato dal nome del progetto che scelto durante la creazione del progetto. È possibile modificarla mediante la ricerca del `GenerateQrCodeUri(string email, string unformattedKey)` metodo il *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* file (pagine Razor) o *Controllers\ManageController.cs* file (MVC). 

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

L'URL formattato correttamente per il codice a matrice è disponibile nel:

* `AuthenticatorUri`proprietà del modello.
* `data-url`proprietà di `qrCodeData` elemento. 

Utilizzare `@Html.Raw` per accedere alla proprietà di modello in una vista (in caso contrario le e commerciali nell'url verranno codificate double e verrà ignorato il parametro etichetta del codice QR).

## <a name="totp-client-and-server-time-skew"></a>TOTP client e server sfasamento dell'ora

Autenticazione TOTP dipende da dispositivo autenticatore sia il server con l'ora esatta. Token solo una durata di 30 secondi. Se l'account di accesso 2FA TOTP non riesce, verificare che l'ora del server è precisa e preferibilmente sincronizzato a un servizio NTP accurato.
