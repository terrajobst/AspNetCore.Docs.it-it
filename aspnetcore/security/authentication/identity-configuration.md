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
# <a name="configure-identity"></a>Configurare l'identità

Identità di ASP.NET Core dispone di alcuni comportamenti predefiniti che è possibile sostituire facilmente in una classe di avvio dell'applicazione.

## <a name="passwords-policy"></a>Criteri password

Per impostazione predefinita, l'identità richiede che le password deve contenere un carattere maiuscolo, carattere minuscolo e cifre. Esistono inoltre alcune altre restrizioni. Se si desidera semplificare restrizioni per le password, è possibile farlo nella classe di avvio dell'applicazione.

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a>Impostazioni dei cookie dell'applicazione

Come i criteri password, tutte le impostazioni dei cookie dell'applicazione possono essere modificate nella classe di avvio.

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a>Blocco dell'utente

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
