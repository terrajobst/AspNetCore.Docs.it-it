---
title: Abilitare la generazione di codice a matrice per le app di autenticazione in ASP.NET Core
author: rick-anderson
description: Abilitare la generazione di codice a matrice per le app di autenticazione in ASP.NET Core
keywords: Componenti di base di ASP.NET, MVC, generazione di codice a matrice, l'autenticatore, 2FA
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: a3029e68164dd91d1bc43704c5e96bd591bcae05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="9f46b-104">Abilitare la generazione di codice a matrice per le app di autenticazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f46b-104">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="9f46b-105">Nota: In questo argomento si applica a ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="9f46b-105">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="9f46b-106">ASP.NET Core viene fornito con supporto per le applicazioni per l'autenticazione singoli autenticatore.</span><span class="sxs-lookup"><span data-stu-id="9f46b-106">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="9f46b-107">Due fattori (2FA) di autenticazione le app di autenticazione, utilizzando un basati sul tempo monouso Password algoritmo (TOTP), sono nel settore dell'approccio per 2FA consigliato.</span><span class="sxs-lookup"><span data-stu-id="9f46b-107">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="9f46b-108">2FA utilizzando TOTP è preferibile a 2FA SMS.</span><span class="sxs-lookup"><span data-stu-id="9f46b-108">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="9f46b-109">Un'app di autenticazione fornisce un codice di 6 a 8 cifre che gli utenti devono immettere dopo la conferma di nome utente e password.</span><span class="sxs-lookup"><span data-stu-id="9f46b-109">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="9f46b-110">In genere un'app di autenticazione viene installata uno Smartphone.</span><span class="sxs-lookup"><span data-stu-id="9f46b-110">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="9f46b-111">I modelli di applicazione web ASP.NET Core supportano gli autenticatori, ma non forniscono supporto per la generazione di QRCode.</span><span class="sxs-lookup"><span data-stu-id="9f46b-111">The ASP.NET Core web app templates support authenticators, but do not provide support for QRCode generation.</span></span> <span data-ttu-id="9f46b-112">I generatori QRCode facilitano l'installazione di 2FA.</span><span class="sxs-lookup"><span data-stu-id="9f46b-112">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="9f46b-113">Questo documento illustra aggiunta [codice QR](https://wikipedia.org/wiki/QR_code) generazione alla pagina di configurazione 2FA.</span><span class="sxs-lookup"><span data-stu-id="9f46b-113">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="9f46b-114">Aggiunta di codici a matrice alla pagina di configurazione 2FA</span><span class="sxs-lookup"><span data-stu-id="9f46b-114">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="9f46b-115">Queste istruzioni si usa *qrcode.js* dal repository https://davidshimjs.github.io/qrcodejs/.</span><span class="sxs-lookup"><span data-stu-id="9f46b-115">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="9f46b-116">Scaricare il [libreria javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) per il `wwwroot\lib` cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="9f46b-116">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="9f46b-117">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (pagine Razor) o *Views\Manage\EnableAuthenticator.cshtml* (MVC), individuare il `Scripts` sezione alla fine del file:</span><span class="sxs-lookup"><span data-stu-id="9f46b-117">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="9f46b-118">Aggiornamento di `Scripts` sezione per aggiungere un riferimento al `qrcodejs` libreria è stato aggiunto e da una chiamata per generare il codice a matrice.</span><span class="sxs-lookup"><span data-stu-id="9f46b-118">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="9f46b-119">Dovrebbe apparire come segue:</span><span class="sxs-lookup"><span data-stu-id="9f46b-119">It should look as follows:</span></span>

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

* <span data-ttu-id="9f46b-120">Eliminare il paragrafo in cui vengono forniti collegamenti a queste istruzioni.</span><span class="sxs-lookup"><span data-stu-id="9f46b-120">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="9f46b-121">Eseguire l'app e assicurarsi che è possibile scansionare il codice a matrice e convalidare il codice che dimostra l'autenticatore.</span><span class="sxs-lookup"><span data-stu-id="9f46b-121">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="9f46b-122">Modificare il nome del sito nel codice a matrice</span><span class="sxs-lookup"><span data-stu-id="9f46b-122">Change the site name in the QR Code</span></span>

<span data-ttu-id="9f46b-123">Il nome del sito nel codice QR viene ricavato dal nome del progetto che scelto durante la creazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="9f46b-123">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="9f46b-124">È possibile modificarla mediante la ricerca del `GenerateQrCodeUri(string email, string unformattedKey)` metodo il *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* file (pagine Razor) o *Controllers\ManageController.cs* file (MVC).</span><span class="sxs-lookup"><span data-stu-id="9f46b-124">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="9f46b-125">Il codice predefinito con il modello è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="9f46b-125">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="9f46b-126">Il secondo parametro nella chiamata a `string.Format` è il nome del sito, ricavato il nome della soluzione.</span><span class="sxs-lookup"><span data-stu-id="9f46b-126">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="9f46b-127">Può essere modificata in qualsiasi valore, ma deve essere sempre codificati nell'URL.</span><span class="sxs-lookup"><span data-stu-id="9f46b-127">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="9f46b-128">Utilizzo di una libreria di codice a matrice diversi</span><span class="sxs-lookup"><span data-stu-id="9f46b-128">Using a different QR Code library</span></span>

<span data-ttu-id="9f46b-129">È possibile sostituire la libreria di codice a matrice con la libreria preferita.</span><span class="sxs-lookup"><span data-stu-id="9f46b-129">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="9f46b-130">Il codice HTML contiene un `qrCode` elemento in cui è possibile inserire un codice a matrice da qualsiasi meccanismo nella libreria siano forniti.</span><span class="sxs-lookup"><span data-stu-id="9f46b-130">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="9f46b-131">L'URL formattato correttamente per il codice a matrice è disponibile nel:</span><span class="sxs-lookup"><span data-stu-id="9f46b-131">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="9f46b-132">`AuthenticatorUri`proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="9f46b-132">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="9f46b-133">`data-url`proprietà di `qrCodeData` elemento.</span><span class="sxs-lookup"><span data-stu-id="9f46b-133">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="9f46b-134">Utilizzare `@Html.Raw` per accedere alla proprietà di modello in una vista (in caso contrario le e commerciali nell'url verranno codificate double e verrà ignorato il parametro etichetta del codice QR).</span><span class="sxs-lookup"><span data-stu-id="9f46b-134">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="9f46b-135">TOTP client e server sfasamento dell'ora</span><span class="sxs-lookup"><span data-stu-id="9f46b-135">TOTP client and server time skew</span></span>

<span data-ttu-id="9f46b-136">Autenticazione TOTP dipende da dispositivo autenticatore sia il server con l'ora esatta.</span><span class="sxs-lookup"><span data-stu-id="9f46b-136">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="9f46b-137">Token solo una durata di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="9f46b-137">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="9f46b-138">Se l'account di accesso 2FA TOTP non riesce, verificare che l'ora del server è precisa e preferibilmente sincronizzato a un servizio NTP accurato.</span><span class="sxs-lookup"><span data-stu-id="9f46b-138">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
