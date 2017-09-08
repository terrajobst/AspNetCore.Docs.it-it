---
title: "Configurare l'identità"
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a><span data-ttu-id="acf17-102">Configurare l'identità</span><span class="sxs-lookup"><span data-stu-id="acf17-102">Configure Identity</span></span>

<span data-ttu-id="acf17-103">Identità di ASP.NET Core dispone di alcuni comportamenti predefiniti che è possibile sostituire facilmente in una classe di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="acf17-103">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="acf17-104">Criteri password</span><span class="sxs-lookup"><span data-stu-id="acf17-104">Passwords policy</span></span>

<span data-ttu-id="acf17-105">Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, carattere minuscolo e cifre.</span><span class="sxs-lookup"><span data-stu-id="acf17-105">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="acf17-106">Esistono inoltre alcune altre restrizioni.</span><span class="sxs-lookup"><span data-stu-id="acf17-106">There are also some other restrictions.</span></span> <span data-ttu-id="acf17-107">Se si desidera semplificare restrizioni per le password, è possibile farlo nella classe di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="acf17-107">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

<span data-ttu-id="acf17-108">[!code-csharp[Principale](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span><span class="sxs-lookup"><span data-stu-id="acf17-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="acf17-109">Impostazioni dei cookie dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="acf17-109">Application's cookie settings</span></span>

<span data-ttu-id="acf17-110">Come i criteri password, tutte le impostazioni dei cookie dell'applicazione possono essere modificate nella classe di avvio.</span><span class="sxs-lookup"><span data-stu-id="acf17-110">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

<span data-ttu-id="acf17-111">[!code-csharp[Principale](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span><span class="sxs-lookup"><span data-stu-id="acf17-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span></span>

## <a name="users-lockout"></a><span data-ttu-id="acf17-112">Blocco dell'utente</span><span class="sxs-lookup"><span data-stu-id="acf17-112">User's lockout</span></span>

<span data-ttu-id="acf17-113">[!code-csharp[Principale](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span><span class="sxs-lookup"><span data-stu-id="acf17-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span></span>
