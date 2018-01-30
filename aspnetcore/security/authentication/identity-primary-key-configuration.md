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
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a><span data-ttu-id="e1300-103">Configurare il tipo di dati della chiave primaria di ASP.NET Identity Core</span><span class="sxs-lookup"><span data-stu-id="e1300-103">Configure the ASP.NET Core Identity primary key data type</span></span>

<span data-ttu-id="e1300-104">Identità di ASP.NET Core consente di configurare il tipo di dati utilizzato per rappresentare una chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="e1300-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="e1300-105">Usa identità di `string` il tipo di dati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e1300-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="e1300-106">È possibile eseguire l'override di questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="e1300-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="e1300-107">Personalizzare il tipo di dati della chiave primaria</span><span class="sxs-lookup"><span data-stu-id="e1300-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="e1300-108">Creare un'implementazione personalizzata del [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) classe.</span><span class="sxs-lookup"><span data-stu-id="e1300-108">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="e1300-109">Rappresenta il tipo da utilizzare per la creazione di oggetti utente.</span><span class="sxs-lookup"><span data-stu-id="e1300-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="e1300-110">Nell'esempio seguente, il valore predefinito `string` tipo viene sostituito con `Guid`.</span><span class="sxs-lookup"><span data-stu-id="e1300-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. <span data-ttu-id="e1300-111">Creare un'implementazione personalizzata del [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) classe.</span><span class="sxs-lookup"><span data-stu-id="e1300-111">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="e1300-112">Rappresenta il tipo da utilizzare per la creazione di oggetti di ruolo.</span><span class="sxs-lookup"><span data-stu-id="e1300-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="e1300-113">Nell'esempio seguente, il valore predefinito `string` tipo viene sostituito con `Guid`.</span><span class="sxs-lookup"><span data-stu-id="e1300-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. <span data-ttu-id="e1300-114">Creare una classe del contesto del database personalizzato.</span><span class="sxs-lookup"><span data-stu-id="e1300-114">Create a custom database context class.</span></span> <span data-ttu-id="e1300-115">Eredita dalla classe di contesto di database di Entity Framework utilizzata per l'identità.</span><span class="sxs-lookup"><span data-stu-id="e1300-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="e1300-116">Il `TUser` e `TRole` argomenti fanno riferimento le classi utente e il ruolo personalizzate create nel passaggio precedente, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="e1300-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="e1300-117">Il `Guid` tipo di dati definito per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="e1300-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. <span data-ttu-id="e1300-118">Quando si aggiunge il servizio di identità nella classe di avvio dell'app, registrare la classe di contesto del database personalizzato.</span><span class="sxs-lookup"><span data-stu-id="e1300-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1300-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1300-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="e1300-120">Il `AddEntityFrameworkStores` metodo non accetta un `TKey` argomento perché è stato in ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="e1300-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="e1300-121">Tipo di dati della chiave primaria è dedotto analizzando il `DbContext` oggetto.</span><span class="sxs-lookup"><span data-stu-id="e1300-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1300-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1300-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="e1300-123">Il `AddEntityFrameworkStores` metodo accetta un `TKey` argomento indica il tipo di dati della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="e1300-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a><span data-ttu-id="e1300-124">Testare le modifiche</span><span class="sxs-lookup"><span data-stu-id="e1300-124">Test the changes</span></span>

<span data-ttu-id="e1300-125">Dopo il completamento delle modifiche di configurazione, la proprietà che rappresenta la chiave primaria riflette il nuovo tipo di dati.</span><span class="sxs-lookup"><span data-stu-id="e1300-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="e1300-126">Nell'esempio seguente viene illustrato l'accesso alla proprietà in un controller MVC.</span><span class="sxs-lookup"><span data-stu-id="e1300-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
