---
title: Configurare il tipo di dati della chiave primaria identità in ASP.NET Core
author: AdrienTorris
description: Informazioni sulla procedura per la configurazione del tipo di dati desiderato, utilizzato per la chiave primaria di ASP.NET Identity Core.
ms.author: scaddie
ms.date: 09/28/2017
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: cfec91e1194556385bb884ee44cf79c1fcbbcb56
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274356"
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a>Configurare il tipo di dati della chiave primaria identità in ASP.NET Core

Identità di ASP.NET Core consente di configurare il tipo di dati utilizzato per rappresentare una chiave primaria. Usa identità di `string` il tipo di dati per impostazione predefinita. È possibile eseguire l'override di questo comportamento.

## <a name="customize-the-primary-key-data-type"></a>Personalizzare il tipo di dati della chiave primaria

1. Creare un'implementazione personalizzata del [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) classe. Rappresenta il tipo da utilizzare per la creazione di oggetti utente. Nell'esempio seguente, il valore predefinito `string` tipo viene sostituito con `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. Creare un'implementazione personalizzata del [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) classe. Rappresenta il tipo da utilizzare per la creazione di oggetti di ruolo. Nell'esempio seguente, il valore predefinito `string` tipo viene sostituito con `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. Creare una classe del contesto del database personalizzato. Eredita dalla classe di contesto di database di Entity Framework utilizzata per l'identità. Il `TUser` e `TRole` argomenti fanno riferimento le classi utente e il ruolo personalizzate create nel passaggio precedente, rispettivamente. Il `Guid` tipo di dati definito per la chiave primaria.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. Quando si aggiunge il servizio di identità nella classe di avvio dell'app, registrare la classe di contesto del database personalizzato.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   Il `AddEntityFrameworkStores` metodo non accetta un `TKey` argomento perché è stato in ASP.NET Core 1. x. Tipo di dati della chiave primaria è dedotto analizzando il `DbContext` oggetto.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   Il `AddEntityFrameworkStores` metodo accetta un `TKey` argomento indica il tipo di dati della chiave primaria.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   ---

## <a name="test-the-changes"></a>Testare le modifiche

Dopo il completamento delle modifiche di configurazione, la proprietà che rappresenta la chiave primaria riflette il nuovo tipo di dati. Nell'esempio seguente viene illustrato l'accesso alla proprietà in un controller MVC.

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
