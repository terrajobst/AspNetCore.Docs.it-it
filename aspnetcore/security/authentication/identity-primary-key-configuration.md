---
title: "Configurare il tipo di dati della chiave primaria di identità"
author: AdrienTorris
description: Questo articolo vengono illustrati i passaggi per configurare il tipo di dati desiderato utilizzato per la chiave primaria di ASP.NET Identity Core.
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 66631e46640e294c934aa563518509b96f5cd158
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a>Configurare il tipo di dati della chiave primaria di ASP.NET Identity Core

Identità di ASP.NET Core consente di configurare il tipo di dati utilizzato per rappresentare una chiave primaria. Usa identità di `string` il tipo di dati per impostazione predefinita. È possibile eseguire l'override di questo comportamento.

## <a name="customize-the-primary-key-data-type"></a>Personalizzare il tipo di dati della chiave primaria

1. Creare un'implementazione personalizzata del [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) classe. Rappresenta il tipo da utilizzare per la creazione di oggetti utente. Nell'esempio seguente, il valore predefinito `string` tipo viene sostituito con `Guid`.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. Creare un'implementazione personalizzata del [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) classe. Rappresenta il tipo da utilizzare per la creazione di oggetti di ruolo. Nell'esempio seguente, il valore predefinito `string` tipo viene sostituito con `Guid`.
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. Creare una classe del contesto del database personalizzato. Eredita dalla classe di contesto di database di Entity Framework utilizzata per l'identità. Il `TUser` e `TRole` argomenti fanno riferimento le classi utente e il ruolo personalizzate create nel passaggio precedente, rispettivamente. Il `Guid` tipo di dati definito per la chiave primaria.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. Quando si aggiunge il servizio di identità nella classe di avvio dell'app, registrare la classe di contesto del database personalizzato.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    Il `AddEntityFrameworkStores` metodo non accetta un `TKey` argomento perché è stato in ASP.NET Core 1. x. Tipo di dati della chiave primaria è dedotto analizzando il `DbContext` oggetto.
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    Il `AddEntityFrameworkStores` metodo accetta un `TKey` argomento indica il tipo di dati della chiave primaria.
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a>Testare le modifiche

Dopo il completamento delle modifiche di configurazione, la proprietà che rappresenta la chiave primaria riflette il nuovo tipo di dati. Nell'esempio seguente viene illustrato l'accesso alla proprietà in un controller MVC.

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
