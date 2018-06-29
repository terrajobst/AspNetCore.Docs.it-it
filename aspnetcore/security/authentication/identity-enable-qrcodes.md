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
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="7db28-103">Abilitare la generazione di codice a matrice per le app di autenticazione TOTP in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7db28-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="7db28-104">ASP.NET Core viene fornito con supporto per le applicazioni autenticatore per l'autenticazione singoli.</span><span class="sxs-lookup"><span data-stu-id="7db28-104">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="7db28-105">Due fattori (2FA) autenticatore applicazioni per l'autenticazione, utilizzando un basati sul tempo monouso Password algoritmo (TOTP), sono il settore per 2FA approccio consigliato.</span><span class="sxs-lookup"><span data-stu-id="7db28-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="7db28-106">2FA utilizzando TOTP è preferibile a 2FA SMS.</span><span class="sxs-lookup"><span data-stu-id="7db28-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="7db28-107">Un'app di autenticazione fornisce un codice di 6 a 8 cifre che gli utenti devono immettere dopo la conferma il nome utente e password.</span><span class="sxs-lookup"><span data-stu-id="7db28-107">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="7db28-108">In genere un'app di autenticazione viene installata uno Smartphone.</span><span class="sxs-lookup"><span data-stu-id="7db28-108">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="7db28-109">I modelli di app web ASP.NET Core supportano gli autenticatori, ma non offrono supporto per la generazione di QRCode.</span><span class="sxs-lookup"><span data-stu-id="7db28-109">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="7db28-110">I generatori QRCode facilitano l'installazione di 2FA.</span><span class="sxs-lookup"><span data-stu-id="7db28-110">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="7db28-111">Questo documento illustra aggiunta [codice QR](https://wikipedia.org/wiki/QR_code) generazione alla pagina di configurazione 2FA.</span><span class="sxs-lookup"><span data-stu-id="7db28-111">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="7db28-112">Aggiunta di codici a matrice alla pagina di configurazione 2FA</span><span class="sxs-lookup"><span data-stu-id="7db28-112">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="7db28-113">Queste istruzioni si usa *qrcode.js* dal https://davidshimjs.github.io/qrcodejs/ repository.</span><span class="sxs-lookup"><span data-stu-id="7db28-113">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="7db28-114">Scaricare il [libreria javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) per il `wwwroot\lib` cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="7db28-114">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="7db28-115">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (pagine Razor) o *Views\Manage\EnableAuthenticator.cshtml* (MVC), individuare il `Scripts` sezione alla fine del file:</span><span class="sxs-lookup"><span data-stu-id="7db28-115">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="7db28-116">Aggiornamento di `Scripts` sezione per aggiungere un riferimento al `qrcodejs` libreria è stato aggiunto e da una chiamata per generare il codice a matrice.</span><span class="sxs-lookup"><span data-stu-id="7db28-116">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="7db28-117">Dovrebbe apparire come segue:</span><span class="sxs-lookup"><span data-stu-id="7db28-117">It should look as follows:</span></span>

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

* <span data-ttu-id="7db28-118">Eliminare il paragrafo che si collega a queste istruzioni.</span><span class="sxs-lookup"><span data-stu-id="7db28-118">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="7db28-119">Eseguire l'app e assicurarsi che è possibile Scansionare il codice e convalidare il codice che dimostra l'autenticatore.</span><span class="sxs-lookup"><span data-stu-id="7db28-119">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="7db28-120">Modificare il nome del sito nel codice a matrice</span><span class="sxs-lookup"><span data-stu-id="7db28-120">Change the site name in the QR Code</span></span>

<span data-ttu-id="7db28-121">Il nome del sito nel codice a matrice viene ricavato dal nome del progetto che scelto durante la creazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="7db28-121">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="7db28-122">È possibile modificarla mediante la ricerca del `GenerateQrCodeUri(string email, string unformattedKey)` metodo il *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* file (pagine Razor) o il *Controllers\ManageController.cs* file (MVC).</span><span class="sxs-lookup"><span data-stu-id="7db28-122">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span>

<span data-ttu-id="7db28-123">Il codice predefinito con il modello è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7db28-123">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="7db28-124">Il secondo parametro nella chiamata a `string.Format` è il nome del sito, ricavato il nome della soluzione.</span><span class="sxs-lookup"><span data-stu-id="7db28-124">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="7db28-125">Può essere modificata in qualsiasi valore, ma deve essere sempre codificati nell'URL.</span><span class="sxs-lookup"><span data-stu-id="7db28-125">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="7db28-126">Utilizzo di una libreria di codice a matrice diversi</span><span class="sxs-lookup"><span data-stu-id="7db28-126">Using a different QR Code library</span></span>

<span data-ttu-id="7db28-127">È possibile sostituire la libreria di codice a matrice con la libreria preferita.</span><span class="sxs-lookup"><span data-stu-id="7db28-127">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="7db28-128">Il codice HTML contiene un `qrCode` elemento in cui è possibile inserire un codice a matrice da qualsiasi meccanismo nella libreria siano forniti.</span><span class="sxs-lookup"><span data-stu-id="7db28-128">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="7db28-129">L'URL formattato in modo corretto per il codice a matrice è disponibile nel:</span><span class="sxs-lookup"><span data-stu-id="7db28-129">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="7db28-130">`AuthenticatorUri` proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="7db28-130">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="7db28-131">`data-url` proprietà di `qrCodeData` elemento.</span><span class="sxs-lookup"><span data-stu-id="7db28-131">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="7db28-132">TOTP client e server sfasamento dell'ora</span><span class="sxs-lookup"><span data-stu-id="7db28-132">TOTP client and server time skew</span></span>

<span data-ttu-id="7db28-133">Autenticazione TOTP (basati sul tempo One-Time Password) dipende da dispositivo autenticatore sia del server con un'accurata del tempo.</span><span class="sxs-lookup"><span data-stu-id="7db28-133">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="7db28-134">Token solo una durata di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="7db28-134">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="7db28-135">Se gli account di accesso TOTP 2FA hanno esito negativo, verificare che l'ora del server sia preciso e preferibilmente sincronizzato a un servizio NTP accurato.</span><span class="sxs-lookup"><span data-stu-id="7db28-135">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
