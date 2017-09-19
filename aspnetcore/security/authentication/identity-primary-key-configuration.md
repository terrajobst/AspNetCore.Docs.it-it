---
title: "Configurare il tipo di dati di identità chiavi primarie"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a>Configurare il tipo di dati di identità chiavi primarie

Identità di ASP.NET Core consente di configurare con facilità il tipo di dati desiderato per le chiavi primarie. Per impostazione predefinita, Usa identità di tipo di dati stringa, ma è possibile ignorare molto rapidamente questo comportamento.

## <a name="how-to"></a>Procedura

1.  Il primo passaggio consiste nell'implementare il modello di identità e il tipo di stringa con il tipo di dati che si desidera eseguire l'override.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  Implementare il contesto del database di identità con i modelli e il tipo di dati desiderato per le chiavi primarie

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  Utilizzare i modelli e il tipo di dati desiderato per le chiavi primarie quando si dichiara il servizio di identità nella classe di avvio dell'applicazione

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
