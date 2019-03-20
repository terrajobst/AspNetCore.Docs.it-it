---
title: Eseguire la migrazione dall'autenticazione di appartenenza ASP.NET a ASP.NET Core 2.0 Identity
author: isaac2004
description: Informazioni su come eseguire la migrazione di App ASP.NET esistenti utilizzando l'autenticazione di appartenenza ad ASP.NET Core 2.0 Identity.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3b708da13ff9f2887eee87ea17844312a4fe1b8d
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264727"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="2f807-103">Eseguire la migrazione dall'autenticazione di appartenenza ASP.NET a ASP.NET Core 2.0 Identity</span><span class="sxs-lookup"><span data-stu-id="2f807-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="2f807-104">Di [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="2f807-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="2f807-105">Questo articolo illustra la migrazione lo schema del database per le app ASP.NET usando l'autenticazione di appartenenza ad ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="2f807-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="2f807-106">Questo documento fornisce i passaggi necessari per eseguire la migrazione lo schema del database per le app basate sul sistema di appartenenze ASP.NET per lo schema del database utilizzato per ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="2f807-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="2f807-107">Per altre informazioni sulla migrazione dall'autenticazione basata sull'appartenenza ASP.NET ad ASP.NET Identity, vedere [eseguire la migrazione di un'app esistente dall'appartenenza SQL ad ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="2f807-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="2f807-108">Per altre informazioni su ASP.NET Core Identity, vedere [Introduzione all'identità in ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="2f807-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="2f807-109">Revisione dello schema di appartenenza</span><span class="sxs-lookup"><span data-stu-id="2f807-109">Review of Membership schema</span></span>

<span data-ttu-id="2f807-110">Prima di ASP.NET 2.0, gli sviluppatori sono state il compito di creare l'intero processo di autenticazione e autorizzazione per le proprie app.</span><span class="sxs-lookup"><span data-stu-id="2f807-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="2f807-111">Con ASP.NET 2.0, è stata introdotta l'appartenenza, fornendo una soluzione standard per la gestione della sicurezza all'interno delle App ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2f807-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="2f807-112">Gli sviluppatori sono in grado di eseguire l'avvio di uno schema in un database di SQL Server con il [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) comando.</span><span class="sxs-lookup"><span data-stu-id="2f807-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="2f807-113">Dopo aver eseguito questo comando, le tabelle seguenti sono state create nel database.</span><span class="sxs-lookup"><span data-stu-id="2f807-113">After running this command, the following tables were created in the database.</span></span>

  ![Tabelle di appartenenza](identity/_static/membership-tables.png)

<span data-ttu-id="2f807-115">Per eseguire la migrazione delle App esistenti per identità di ASP.NET Core 2.0, i dati in queste tabelle devono essere eseguita la migrazione per le tabelle usate dal nuovo schema di identità.</span><span class="sxs-lookup"><span data-stu-id="2f807-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="2f807-116">Schema ASP.NET Core Identity 2.0</span><span class="sxs-lookup"><span data-stu-id="2f807-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="2f807-117">ASP.NET Core 2.0 segue il [identità](/aspnet/identity/index) principio introdotta in ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="2f807-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="2f807-118">Anche se il principio è condiviso, l'implementazione tra i Framework è diversa, anche tra le versioni di ASP.NET Core (vedere [eseguire la migrazione di autenticazione e identità ad ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="2f807-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="2f807-119">Il modo più rapido per visualizzare lo schema per l'identità di ASP.NET Core 2.0 consiste nel creare una nuova app ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2f807-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="2f807-120">Seguire questi passaggi in Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="2f807-120">Follow these steps in Visual Studio 2017:</span></span>

1. <span data-ttu-id="2f807-121">Selezionare **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="2f807-121">Select **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="2f807-122">Creare una nuova **applicazione Web ASP.NET Core** progetto denominato *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="2f807-122">Create a new **ASP.NET Core Web Application** project named *CoreIdentitySample*.</span></span>
1. <span data-ttu-id="2f807-123">Selezionare **ASP.NET Core 2.0** nell'elenco a discesa e quindi selezionare **applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="2f807-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="2f807-124">Questo modello genera un [pagine Razor](xref:razor-pages/index) app.</span><span class="sxs-lookup"><span data-stu-id="2f807-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="2f807-125">Prima di fare clic **OK**, fare clic su **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="2f807-125">Before clicking **OK**, click **Change Authentication**.</span></span>
1. <span data-ttu-id="2f807-126">Scegli **singoli account utente** per i modelli di identità.</span><span class="sxs-lookup"><span data-stu-id="2f807-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="2f807-127">Infine, fare clic su **OK**, quindi **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f807-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="2f807-128">Visual Studio crea un progetto usando il modello di ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="2f807-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>
1. <span data-ttu-id="2f807-129">Selezionare **degli strumenti** > **Gestione pacchetti NuGet** > **Package Manager Console** per aprire la **Consoledigestionepacchetti** Finestra.</span><span class="sxs-lookup"><span data-stu-id="2f807-129">Select **Tools** > **NuGet Package Manager** > **Package Manager Console** to open the **Package Manager Console** (PMC) window.</span></span>
1. <span data-ttu-id="2f807-130">Passare alla radice del progetto nella console di gestione pacchetti ed eseguire la [Entity Framework (EF) Core](/ef/core) `Update-Database` comando.</span><span class="sxs-lookup"><span data-stu-id="2f807-130">Navigate to the project root in PMC, and run the [Entity Framework (EF) Core](/ef/core) `Update-Database` command.</span></span>

    <span data-ttu-id="2f807-131">Identità di ASP.NET Core 2.0 usa EF Core per interagire con il database di archiviare i dati di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2f807-131">ASP.NET Core 2.0 Identity uses EF Core to interact with the database storing the authentication data.</span></span> <span data-ttu-id="2f807-132">Affinché l'app appena creata per funzionare, deve essere un database per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="2f807-132">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="2f807-133">Dopo aver creato una nuova app, il modo più rapido per controllare lo schema in un ambiente di database consiste nel creare il database usando [migrazioni EF Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="2f807-133">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using [EF Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="2f807-134">Questo processo crea un database, sia localmente o in altro modo, che simula quello schema.</span><span class="sxs-lookup"><span data-stu-id="2f807-134">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="2f807-135">Vedere la documentazione precedente per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="2f807-135">Review the preceding documentation for more information.</span></span>

    <span data-ttu-id="2f807-136">Comandi di EF Core usano la stringa di connessione per il database specificato nel *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="2f807-136">EF Core commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="2f807-137">La stringa di connessione seguente destinato a un database sul *localhost* denominate *asp-net-core-identity*.</span><span class="sxs-lookup"><span data-stu-id="2f807-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="2f807-138">In questa impostazione, EF Core è configurato per usare il `DefaultConnection` stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="2f807-138">In this setting, EF Core is configured to use the `DefaultConnection` connection string.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. <span data-ttu-id="2f807-139">Selezionare **View** > **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="2f807-139">Select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="2f807-140">Espandere il nodo corrispondente al nome del database specificato nella `ConnectionStrings:DefaultConnection` proprietà di *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="2f807-140">Expand the node corresponding to the database name specified in the `ConnectionStrings:DefaultConnection` property of *appsettings.json*.</span></span>

    <span data-ttu-id="2f807-141">Il `Update-Database` comando ha creato il database specificato con lo schema e tutti i dati necessari per l'inizializzazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="2f807-141">The `Update-Database` command created the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="2f807-142">La figura seguente viene illustrata la struttura della tabella che viene creata con i passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="2f807-142">The following image depicts the table structure that's created with the preceding steps.</span></span>

    ![Tabelle di identità](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="2f807-144">Eseguire la migrazione dello schema</span><span class="sxs-lookup"><span data-stu-id="2f807-144">Migrate the schema</span></span>

<span data-ttu-id="2f807-145">Esistono differenze minime nei campi relativi all'appartenenza e ASP.NET Core Identity e strutture di tabella.</span><span class="sxs-lookup"><span data-stu-id="2f807-145">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="2f807-146">Il modello è stato modificato in modo sostanziale per l'autenticazione/autorizzazione con le app ASP.NET Core e ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2f807-146">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="2f807-147">Gli oggetti chiave vengono ancora utilizzati con identità sono *gli utenti* e *ruoli*.</span><span class="sxs-lookup"><span data-stu-id="2f807-147">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="2f807-148">Di seguito sono le tabelle di mapping per *gli utenti*, *ruoli*, e *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="2f807-148">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="2f807-149">Utenti</span><span class="sxs-lookup"><span data-stu-id="2f807-149">Users</span></span>

|<span data-ttu-id="2f807-150">*Identity<br>(dbo.AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="2f807-150">*Identity<br>(dbo.AspNetUsers)*</span></span>        ||<span data-ttu-id="2f807-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="2f807-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span></span>||
|----------------------------------------|-----------------------------------------------------------|
|<span data-ttu-id="2f807-152">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="2f807-152">**Field Name**</span></span>                 |<span data-ttu-id="2f807-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="2f807-153">**Type**</span></span>|<span data-ttu-id="2f807-154">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="2f807-154">**Field Name**</span></span>                                    |<span data-ttu-id="2f807-155">**Type**</span><span class="sxs-lookup"><span data-stu-id="2f807-155">**Type**</span></span>|
|`Id`                           |<span data-ttu-id="2f807-156">string</span><span class="sxs-lookup"><span data-stu-id="2f807-156">string</span></span>  |`aspnet_Users.UserId`                             |<span data-ttu-id="2f807-157">string</span><span class="sxs-lookup"><span data-stu-id="2f807-157">string</span></span>  |
|`UserName`                     |<span data-ttu-id="2f807-158">string</span><span class="sxs-lookup"><span data-stu-id="2f807-158">string</span></span>  |`aspnet_Users.UserName`                           |<span data-ttu-id="2f807-159">string</span><span class="sxs-lookup"><span data-stu-id="2f807-159">string</span></span>  |
|`Email`                        |<span data-ttu-id="2f807-160">string</span><span class="sxs-lookup"><span data-stu-id="2f807-160">string</span></span>  |`aspnet_Membership.Email`                         |<span data-ttu-id="2f807-161">string</span><span class="sxs-lookup"><span data-stu-id="2f807-161">string</span></span>  |
|`NormalizedUserName`           |<span data-ttu-id="2f807-162">string</span><span class="sxs-lookup"><span data-stu-id="2f807-162">string</span></span>  |`aspnet_Users.LoweredUserName`                    |<span data-ttu-id="2f807-163">string</span><span class="sxs-lookup"><span data-stu-id="2f807-163">string</span></span>  |
|`NormalizedEmail`              |<span data-ttu-id="2f807-164">string</span><span class="sxs-lookup"><span data-stu-id="2f807-164">string</span></span>  |`aspnet_Membership.LoweredEmail`                  |<span data-ttu-id="2f807-165">string</span><span class="sxs-lookup"><span data-stu-id="2f807-165">string</span></span>  |
|`PhoneNumber`                  |<span data-ttu-id="2f807-166">string</span><span class="sxs-lookup"><span data-stu-id="2f807-166">string</span></span>  |`aspnet_Users.MobileAlias`                        |<span data-ttu-id="2f807-167">string</span><span class="sxs-lookup"><span data-stu-id="2f807-167">string</span></span>  |
|`LockoutEnabled`               |<span data-ttu-id="2f807-168">bit</span><span class="sxs-lookup"><span data-stu-id="2f807-168">bit</span></span>     |`aspnet_Membership.IsLockedOut`                   |<span data-ttu-id="2f807-169">bit</span><span class="sxs-lookup"><span data-stu-id="2f807-169">bit</span></span>     |

> [!NOTE]
> <span data-ttu-id="2f807-170">Non tutti i mapping dei campi sono simili alle relazioni uno a uno dall'appartenenza ad ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="2f807-170">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="2f807-171">Nella tabella precedente accetta lo schema utente di appartenenza predefinito e ne esegue il mapping allo schema di ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="2f807-171">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="2f807-172">Gli altri campi personalizzati che sono stati usati per l'appartenenza è necessario eseguire il mapping manualmente.</span><span class="sxs-lookup"><span data-stu-id="2f807-172">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="2f807-173">In questo mapping, non è Nessuna mappa per le password, come i criteri password e password sali non esegue la migrazione tra i due.</span><span class="sxs-lookup"><span data-stu-id="2f807-173">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="2f807-174">**Si consiglia di lasciare la password come null e chiedere agli utenti di reimpostare le password.**</span><span class="sxs-lookup"><span data-stu-id="2f807-174">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="2f807-175">In ASP.NET Core Identity, `LockoutEnd` deve essere impostato su una data in futuro se l'utente è bloccato. Come illustrato nello script di migrazione.</span><span class="sxs-lookup"><span data-stu-id="2f807-175">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="2f807-176">Ruoli</span><span class="sxs-lookup"><span data-stu-id="2f807-176">Roles</span></span>

|<span data-ttu-id="2f807-177">*Identity<br>(dbo.AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="2f807-177">*Identity<br>(dbo.AspNetRoles)*</span></span>        ||<span data-ttu-id="2f807-178">*Membership<br>(dbo.aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="2f807-178">*Membership<br>(dbo.aspnet_Roles)*</span></span>||
|----------------------------------------|-----------------------------------|
|<span data-ttu-id="2f807-179">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="2f807-179">**Field Name**</span></span>                 |<span data-ttu-id="2f807-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="2f807-180">**Type**</span></span>|<span data-ttu-id="2f807-181">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="2f807-181">**Field Name**</span></span>   |<span data-ttu-id="2f807-182">**Type**</span><span class="sxs-lookup"><span data-stu-id="2f807-182">**Type**</span></span>         |
|`Id`                           |<span data-ttu-id="2f807-183">string</span><span class="sxs-lookup"><span data-stu-id="2f807-183">string</span></span>  |`RoleId`         | <span data-ttu-id="2f807-184">string</span><span class="sxs-lookup"><span data-stu-id="2f807-184">string</span></span>          |
|`Name`                         |<span data-ttu-id="2f807-185">string</span><span class="sxs-lookup"><span data-stu-id="2f807-185">string</span></span>  |`RoleName`       | <span data-ttu-id="2f807-186">string</span><span class="sxs-lookup"><span data-stu-id="2f807-186">string</span></span>          |
|`NormalizedName`               |<span data-ttu-id="2f807-187">string</span><span class="sxs-lookup"><span data-stu-id="2f807-187">string</span></span>  |`LoweredRoleName`| <span data-ttu-id="2f807-188">string</span><span class="sxs-lookup"><span data-stu-id="2f807-188">string</span></span>          |

### <a name="user-roles"></a><span data-ttu-id="2f807-189">Ruoli utente</span><span class="sxs-lookup"><span data-stu-id="2f807-189">User Roles</span></span>

|<span data-ttu-id="2f807-190">*Identity<br>(dbo.AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="2f807-190">*Identity<br>(dbo.AspNetUserRoles)*</span></span>||<span data-ttu-id="2f807-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="2f807-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span></span>||
|------------------------------------|------------------------------------------|
|<span data-ttu-id="2f807-192">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="2f807-192">**Field Name**</span></span>           |<span data-ttu-id="2f807-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="2f807-193">**Type**</span></span>  |<span data-ttu-id="2f807-194">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="2f807-194">**Field Name**</span></span>|<span data-ttu-id="2f807-195">**Type**</span><span class="sxs-lookup"><span data-stu-id="2f807-195">**Type**</span></span>                   |
|`RoleId`                 |<span data-ttu-id="2f807-196">string</span><span class="sxs-lookup"><span data-stu-id="2f807-196">string</span></span>    |`RoleId`      |<span data-ttu-id="2f807-197">string</span><span class="sxs-lookup"><span data-stu-id="2f807-197">string</span></span>                     |
|`UserId`                 |<span data-ttu-id="2f807-198">string</span><span class="sxs-lookup"><span data-stu-id="2f807-198">string</span></span>    |`UserId`      |<span data-ttu-id="2f807-199">string</span><span class="sxs-lookup"><span data-stu-id="2f807-199">string</span></span>                     |

<span data-ttu-id="2f807-200">Fare riferimento a tabelle di mapping precedenti durante la creazione di uno script di migrazione per *gli utenti* e *ruoli*.</span><span class="sxs-lookup"><span data-stu-id="2f807-200">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="2f807-201">Nell'esempio seguente si presuppone due database in un server di database.</span><span class="sxs-lookup"><span data-stu-id="2f807-201">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="2f807-202">Un database contiene lo schema di appartenenza ASP.NET esistente e i dati.</span><span class="sxs-lookup"><span data-stu-id="2f807-202">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="2f807-203">L'altra *CoreIdentitySample* database è stato creato utilizzando i passaggi descritti in precedenza.</span><span class="sxs-lookup"><span data-stu-id="2f807-203">The other *CoreIdentitySample* database was created using steps described earlier.</span></span> <span data-ttu-id="2f807-204">I commenti sono incluse inline per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="2f807-204">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
USE aspnetdb

-- INSERT USERS
INSERT INTO CoreIdentitySample.dbo.AspNetUsers
            (Id,
             UserName,
             NormalizedUserName,
             PasswordHash,
             SecurityStamp,
             EmailConfirmed,
             PhoneNumber,
             PhoneNumberConfirmed,
             TwoFactorEnabled,
             LockoutEnd,
             LockoutEnabled,
             AccessFailedCount,
             Email,
             NormalizedEmail)
SELECT aspnet_Users.UserId,
       aspnet_Users.UserName,
       -- The NormalizedUserName value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Users.UserName),
       -- Creates an empty password since passwords don't map between the 2 schemas
       '',
       /*
        The SecurityStamp token is used to verify the state of an account and
        is subject to change at any time. It should be initialized as a new ID.
       */
       NewID(),
       /*
        EmailConfirmed is set when a new user is created and confirmed via email.
        Users must have this set during migration to reset passwords.
       */
       1,
       aspnet_Users.MobileAlias,
       CASE
         WHEN aspnet_Users.MobileAlias IS NULL THEN 0
         ELSE 1
       END,
       -- 2FA likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         -- Setting lockout date to time in the future (1,000 years)
         WHEN aspnet_Membership.IsLockedOut = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_Membership.IsLockedOut,
       /*
        AccessFailedAccount is used to track failed logins. This is stored in
        Membership in multiple columns. Setting to 0 arbitrarily.
       */
       0,
       aspnet_Membership.Email,
       -- The NormalizedEmail value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Membership.Email)
FROM   aspnet_Users
       LEFT OUTER JOIN aspnet_Membership
                    ON aspnet_Membership.ApplicationId =
                       aspnet_Users.ApplicationId
                       AND aspnet_Users.UserId = aspnet_Membership.UserId
       LEFT OUTER JOIN CoreIdentitySample.dbo.AspNetUsers
                    ON aspnet_Membership.UserId = AspNetUsers.Id
WHERE  AspNetUsers.Id IS NULL

-- INSERT ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetRoles(Id, Name)
SELECT RoleId, RoleName
FROM aspnet_Roles;

-- INSERT USER ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetUserRoles(UserId, RoleId)
SELECT UserId, RoleId
FROM aspnet_UsersInRoles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="2f807-205">Dopo il completamento dello script precedente, l'app ASP.NET Core Identity creato in precedenza viene popolato con gli utenti di appartenenza.</span><span class="sxs-lookup"><span data-stu-id="2f807-205">After completion of the preceding script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="2f807-206">Gli utenti dovranno modificare le password prima di accedere.</span><span class="sxs-lookup"><span data-stu-id="2f807-206">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="2f807-207">Se il sistema di appartenenze era gli utenti con i nomi utente che non corrispondono l'indirizzo di posta elettronica, le modifiche sono necessarie per l'app creata in precedenza per rispondere a questa esigenza.</span><span class="sxs-lookup"><span data-stu-id="2f807-207">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="2f807-208">Prevede che il modello predefinito `UserName` e `Email` sia lo stesso.</span><span class="sxs-lookup"><span data-stu-id="2f807-208">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="2f807-209">Per le situazioni in cui si diverso, deve essere modificato per usare la procedura di accesso `UserName` invece di `Email`.</span><span class="sxs-lookup"><span data-stu-id="2f807-209">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="2f807-210">Nel `PageModel` della pagina di accesso, disponibile all'indirizzo *Pages\Account\Login.cshtml.cs*, rimuovere il `[EmailAddress]` dell'attributo dal *messaggio di posta elettronica* proprietà.</span><span class="sxs-lookup"><span data-stu-id="2f807-210">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="2f807-211">Rinominarlo *UserName*.</span><span class="sxs-lookup"><span data-stu-id="2f807-211">Rename it to *UserName*.</span></span> <span data-ttu-id="2f807-212">Questa operazione richiede una modifica ovunque `EmailAddress` vengono indicati nel *visualizzazione* e *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="2f807-212">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="2f807-213">Il risultato è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="2f807-213">The result looks like the following:</span></span>

 ![Account di accesso predefinito](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="2f807-215">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="2f807-215">Next steps</span></span>

<span data-ttu-id="2f807-216">In questa esercitazione è stato descritto come trasferire gli utenti dall'appartenenza SQL ad ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="2f807-216">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="2f807-217">Per altre informazioni su ASP.NET Core Identity, vedere [Introduzione all'identità](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="2f807-217">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
