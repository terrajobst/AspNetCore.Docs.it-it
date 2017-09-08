---
title: "Configurare l'identità"
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a>Configurare l'identità

Identità di ASP.NET Core dispone di alcuni comportamenti predefiniti che è possibile sostituire facilmente in una classe di avvio dell'applicazione.

## <a name="passwords-policy"></a>Criteri password

Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, carattere minuscolo e cifre. Esistono inoltre alcune altre restrizioni. Se si desidera semplificare restrizioni per le password, è possibile farlo nella classe di avvio dell'applicazione.

[!code-csharp[Principale](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a>Impostazioni dei cookie dell'applicazione

Come i criteri password, tutte le impostazioni dei cookie dell'applicazione possono essere modificate nella classe di avvio.

[!code-csharp[Principale](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a>Blocco dell'utente

[!code-csharp[Principale](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
