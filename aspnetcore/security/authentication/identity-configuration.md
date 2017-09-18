---
title: "Configurare l'identità di ASP.NET Core"
author: AdrienTorris
description: "Comprendere i valori predefiniti di ASP.NET Identity Core e configurare le varie proprietà di identità per l'utilizzo di valori personalizzati."
keywords: "Autenticazione ASP.NET Core, identità, sicurezza"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 629fcc2941b2d2fda9604a3eac04be3d0f5294b2
ms.sourcegitcommit: ddefc78270bd9b5ae0b1bd8de6c45f6977e7dceb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2017
---
# <a name="configure-identity"></a><span data-ttu-id="5a9cf-104">Configurare l'identità</span><span class="sxs-lookup"><span data-stu-id="5a9cf-104">Configure Identity</span></span>

<span data-ttu-id="5a9cf-105">Identità di ASP.NET Core dispone di alcuni comportamenti predefiniti che è possibile sostituire facilmente in una classe di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5a9cf-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="5a9cf-106">Criteri password</span><span class="sxs-lookup"><span data-stu-id="5a9cf-106">Passwords policy</span></span>

<span data-ttu-id="5a9cf-107">Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, carattere minuscolo e cifre.</span><span class="sxs-lookup"><span data-stu-id="5a9cf-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="5a9cf-108">Esistono inoltre alcune altre restrizioni.</span><span class="sxs-lookup"><span data-stu-id="5a9cf-108">There are also some other restrictions.</span></span> <span data-ttu-id="5a9cf-109">Se si desidera semplificare restrizioni per le password, è possibile farlo nella classe di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5a9cf-109">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a><span data-ttu-id="5a9cf-110">Impostazioni dei cookie dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="5a9cf-110">Application's cookie settings</span></span>

<span data-ttu-id="5a9cf-111">Come i criteri password, tutte le impostazioni dei cookie dell'applicazione possono essere modificate nella classe di avvio.</span><span class="sxs-lookup"><span data-stu-id="5a9cf-111">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a><span data-ttu-id="5a9cf-112">Blocco dell'utente</span><span class="sxs-lookup"><span data-stu-id="5a9cf-112">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
