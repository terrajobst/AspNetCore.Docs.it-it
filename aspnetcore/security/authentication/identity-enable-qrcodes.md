---
title: Abilitare la generazione di codice a matrice per le app di autenticazione TOTP in ASP.NET Core
author: rick-anderson
description: Informazioni su come abilitare la generazione di codice a matrice per le app di autenticazione TOTP che funzionano con l'autenticazione a due fattori di ASP.NET Core.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: cf99cc21a7a1bb4d01c7cc092106d23375a1a76f
ms.sourcegitcommit: ca5f03210bedc61c6639a734ae5674bfe095dee8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2019
ms.locfileid: "55073127"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="9d648-103">Abilitare la generazione di codice a matrice per le app di autenticazione TOTP in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d648-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="9d648-104">I codici a matrice richiede ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="9d648-104">QR Codes requires ASP.NET Core 2.0 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9d648-105">ASP.NET Core viene fornito con supporto per le applicazioni di authenticator per la singola autenticazione.</span><span class="sxs-lookup"><span data-stu-id="9d648-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="9d648-106">Due factor authentication (2FA) le app di autenticazione, usando un basati sul tempo monouso Password algoritmo (TOTP), sono il settore approccio autenticazione 2fa consigliato.</span><span class="sxs-lookup"><span data-stu-id="9d648-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="9d648-107">2FA usando TOTP è preferibile a SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="9d648-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="9d648-108">Un'app di autenticazione fornisce un codice di 6-8 cifre che gli utenti devono immettere dopo avere verificato il nome utente e password.</span><span class="sxs-lookup"><span data-stu-id="9d648-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="9d648-109">In genere un'app di autenticazione viene installata in uno Smartphone.</span><span class="sxs-lookup"><span data-stu-id="9d648-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="9d648-110">I modelli di app web ASP.NET Core supportano gli autenticatori, ma non offrono supporto per la generazione di QRCode.</span><span class="sxs-lookup"><span data-stu-id="9d648-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="9d648-111">I generatori di QRCode facilitano l'installazione di 2FA.</span><span class="sxs-lookup"><span data-stu-id="9d648-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="9d648-112">Questo documento illustra l'aggiunta [codice a matrice](https://wikipedia.org/wiki/QR_code) generazione alla pagina di configurazione 2FA.</span><span class="sxs-lookup"><span data-stu-id="9d648-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

<span data-ttu-id="9d648-113">Autenticazione a due fattori non viene eseguita tramite un provider di autenticazione esterni, ad esempio [Google](xref:security/authentication/google-logins) oppure [Facebook](xref:security/authentication/facebook-logins).</span><span class="sxs-lookup"><span data-stu-id="9d648-113">Two factor authentication does not happen using an external authentication provider, such as [Google](xref:security/authentication/google-logins) or [Facebook](xref:security/authentication/facebook-logins).</span></span> <span data-ttu-id="9d648-114">Account di accesso esterni sono protetti da qualsiasi meccanismo fornisce il provider di accesso esterno.</span><span class="sxs-lookup"><span data-stu-id="9d648-114">External logins are protected by whatever mechanism the external login provider provides.</span></span> <span data-ttu-id="9d648-115">Si consideri, ad esempio, il [Microsoft](xref:security/authentication/microsoft-logins) provider di autenticazione richiede una chiave hardware o un altro approccio 2FA.</span><span class="sxs-lookup"><span data-stu-id="9d648-115">Consider, for example, the [Microsoft](xref:security/authentication/microsoft-logins) authentication provider requires a hardware key or another 2FA approach.</span></span> <span data-ttu-id="9d648-116">Se i modelli predefiniti applicati 2FA "locale" agli utenti verranno richiesto per soddisfare due approcci 2FA, che non è uno scenario di uso comune.</span><span class="sxs-lookup"><span data-stu-id="9d648-116">If the default templates enforced "local" 2FA then users would be required to satisfy two 2FA approaches, which is not a commonly used scenario.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="9d648-117">Aggiunta di codici a matrice per la pagina di configurazione 2FA</span><span class="sxs-lookup"><span data-stu-id="9d648-117">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="9d648-118">Usano queste istruzioni *qrcode.js* dal https://davidshimjs.github.io/qrcodejs/ repository.</span><span class="sxs-lookup"><span data-stu-id="9d648-118">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="9d648-119">Scaricare il [libreria javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) per il `wwwroot\lib` cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="9d648-119">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="9d648-120">Seguire le istruzioni in [identità Scaffold](xref:security/authentication/scaffold-identity) generare */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9d648-120">Follow the instructions in [Scaffold Identity](xref:security/authentication/scaffold-identity) to generate */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>
* <span data-ttu-id="9d648-121">Nelle */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, individuare il `Scripts` sezione alla fine del file:</span><span class="sxs-lookup"><span data-stu-id="9d648-121">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="9d648-122">Nelle *Pages/Account/Manage/EnableAuthenticator.cshtml* (pagine Razor) o *Views/Manage/EnableAuthenticator.cshtml* (MVC), individuare il `Scripts` sezione alla fine del file:</span><span class="sxs-lookup"><span data-stu-id="9d648-122">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) or *Views/Manage/EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="9d648-123">Aggiorna il `Scripts` sezione per aggiungere un riferimento al `qrcodejs` libreria è stato aggiunto e una chiamata per generare il codice a matrice.</span><span class="sxs-lookup"><span data-stu-id="9d648-123">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="9d648-124">Dovrebbe apparire come segue:</span><span class="sxs-lookup"><span data-stu-id="9d648-124">It should look as follows:</span></span>

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

* <span data-ttu-id="9d648-125">Eliminare il paragrafo che fornisce collegamenti a queste istruzioni.</span><span class="sxs-lookup"><span data-stu-id="9d648-125">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="9d648-126">Eseguire l'app e assicurarsi che è possibile Scansionare il codice e convalidare il codice che dimostra l'autenticatore.</span><span class="sxs-lookup"><span data-stu-id="9d648-126">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="9d648-127">Modificare il nome del sito nel codice a matrice</span><span class="sxs-lookup"><span data-stu-id="9d648-127">Change the site name in the QR Code</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9d648-128">Il nome del sito nel codice a matrice viene ottenuto dal nome del progetto che scelto durante la creazione iniziale del progetto.</span><span class="sxs-lookup"><span data-stu-id="9d648-128">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="9d648-129">È possibile modificarlo cercando il `GenerateQrCodeUri(string email, string unformattedKey)` metodo nella */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9d648-129">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9d648-130">Il nome del sito nel codice a matrice viene ottenuto dal nome del progetto che scelto durante la creazione iniziale del progetto.</span><span class="sxs-lookup"><span data-stu-id="9d648-130">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="9d648-131">È possibile modificarlo mediante la ricerca del `GenerateQrCodeUri(string email, string unformattedKey)` metodo nella *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* file (pagine Razor) o il *Controllers/ManageController.cs* file (MVC).</span><span class="sxs-lookup"><span data-stu-id="9d648-131">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers/ManageController.cs* (MVC) file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9d648-132">Il codice predefinito dal modello è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="9d648-132">The default code from the template looks as follows:</span></span>

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

<span data-ttu-id="9d648-133">Il secondo parametro nella chiamata a `string.Format` è il nome del sito, tratto dal nome della soluzione.</span><span class="sxs-lookup"><span data-stu-id="9d648-133">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="9d648-134">Può essere modificata in qualsiasi valore, ma deve sempre essere codificato in URL.</span><span class="sxs-lookup"><span data-stu-id="9d648-134">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="9d648-135">Usando un'altra libreria di codice a matrice</span><span class="sxs-lookup"><span data-stu-id="9d648-135">Using a different QR Code library</span></span>

<span data-ttu-id="9d648-136">È possibile sostituire la libreria di codice a matrice con la libreria preferita.</span><span class="sxs-lookup"><span data-stu-id="9d648-136">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="9d648-137">Il codice HTML contiene un `qrCode` elemento in cui è possibile inserire un codice a matrice da qualsiasi meccanismo nella libreria siano forniti.</span><span class="sxs-lookup"><span data-stu-id="9d648-137">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="9d648-138">L'URL formattato in modo corretto per il codice a matrice è disponibile nel:</span><span class="sxs-lookup"><span data-stu-id="9d648-138">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="9d648-139">`AuthenticatorUri` proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="9d648-139">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="9d648-140">`data-url` proprietà di `qrCodeData` elemento.</span><span class="sxs-lookup"><span data-stu-id="9d648-140">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="9d648-141">TOTP client e server sfasamento dell'ora</span><span class="sxs-lookup"><span data-stu-id="9d648-141">TOTP client and server time skew</span></span>

<span data-ttu-id="9d648-142">Autenticazione TOTP (basati sul tempo One-Time Password) dipende da dispositivo authenticator sia il server con l'ora esatta.</span><span class="sxs-lookup"><span data-stu-id="9d648-142">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="9d648-143">Token durano solo per 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="9d648-143">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="9d648-144">Se gli account di accesso 2FA TOTP hanno esito negativo, verificare che l'ora del server è precisa e preferibilmente sincronizzato a un servizio NTP accurato.</span><span class="sxs-lookup"><span data-stu-id="9d648-144">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>

::: moniker-end
