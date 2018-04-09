---
title: Configurare il tipo di dati della chiave primaria identità in ASP.NET Core
author: AdrienTorris
description: Informazioni sulla procedura per la configurazione del tipo di dati desiderato, utilizzato per la chiave primaria di ASP.NET Identity Core.
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: db47055aecc5252dbb3991f29a8255b946deaeb7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a><span data-ttu-id="ee68d-103">Configurare il tipo di dati della chiave primaria identità in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee68d-103">Configure Identity primary key data type in ASP.NET Core</span></span>

<span data-ttu-id="ee68d-104">Identità di ASP.NET Core consente di configurare il tipo di dati utilizzato per rappresentare una chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="ee68d-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="ee68d-105">Usa identità di `string` il tipo di dati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ee68d-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="ee68d-106">È possibile eseguire l'override di questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="ee68d-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="ee68d-107">Personalizzare il tipo di dati della chiave primaria</span><span class="sxs-lookup"><span data-stu-id="ee68d-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="ee68d-108">Creare un'implementazione personalizzata del [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) classe.</span><span class="sxs-lookup"><span data-stu-id="ee68d-108">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="ee68d-109">Rappresenta il tipo da utilizzare per la creazione di oggetti utente.</span><span class="sxs-lookup"><span data-stu-id="ee68d-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="ee68d-110">Nell'esempio seguente, il valore predefinito `string` tipo viene sostituito con `Guid`.</span><span class="sxs-lookup"><span data-stu-id="ee68d-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. <span data-ttu-id="ee68d-111">Creare un'implementazione personalizzata del [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) classe.</span><span class="sxs-lookup"><span data-stu-id="ee68d-111">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="ee68d-112">Rappresenta il tipo da utilizzare per la creazione di oggetti di ruolo.</span><span class="sxs-lookup"><span data-stu-id="ee68d-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="ee68d-113">Nell'esempio seguente, il valore predefinito `string` tipo viene sostituito con `Guid`.</span><span class="sxs-lookup"><span data-stu-id="ee68d-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. <span data-ttu-id="ee68d-114">Creare una classe del contesto del database personalizzato.</span><span class="sxs-lookup"><span data-stu-id="ee68d-114">Create a custom database context class.</span></span> <span data-ttu-id="ee68d-115">Eredita dalla classe di contesto di database di Entity Framework utilizzata per l'identità.</span><span class="sxs-lookup"><span data-stu-id="ee68d-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="ee68d-116">Il `TUser` e `TRole` argomenti fanno riferimento le classi utente e il ruolo personalizzate create nel passaggio precedente, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="ee68d-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="ee68d-117">Il `Guid` tipo di dati definito per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="ee68d-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. <span data-ttu-id="ee68d-118">Quando si aggiunge il servizio di identità nella classe di avvio dell'app, registrare la classe di contesto del database personalizzato.</span><span class="sxs-lookup"><span data-stu-id="ee68d-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ee68d-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ee68d-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
    <span data-ttu-id="ee68d-120">Il `AddEntityFrameworkStores` metodo non accetta un `TKey` argomento perché è stato in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="ee68d-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="ee68d-121">Tipo di dati della chiave primaria è dedotto analizzando il `DbContext` oggetto.</span><span class="sxs-lookup"><span data-stu-id="ee68d-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>

    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ee68d-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ee68d-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
    <span data-ttu-id="ee68d-123">Il `AddEntityFrameworkStores` metodo accetta un `TKey` argomento indica il tipo di dati della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="ee68d-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   * * *
## <a name="test-the-changes"></a><span data-ttu-id="ee68d-124">Testare le modifiche</span><span class="sxs-lookup"><span data-stu-id="ee68d-124">Test the changes</span></span>

<span data-ttu-id="ee68d-125">Dopo il completamento delle modifiche di configurazione, la proprietà che rappresenta la chiave primaria riflette il nuovo tipo di dati.</span><span class="sxs-lookup"><span data-stu-id="ee68d-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="ee68d-126">Nell'esempio seguente viene illustrato l'accesso alla proprietà in un controller MVC.</span><span class="sxs-lookup"><span data-stu-id="ee68d-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
