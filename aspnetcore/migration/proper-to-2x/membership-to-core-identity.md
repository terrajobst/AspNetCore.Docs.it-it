---
title: Eseguire la migrazione dall'autenticazione di appartenenza ASP.NET all'identità di ASP.NET Core 2.0
author: isaac2004
description: Informazioni su come eseguire la migrazione di applicazioni ASP.NET esistenti utilizzando l'autenticazione di appartenenza di ASP.NET Core 2.0 Identity.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3ec22713997a74b587ef5d18e71a28668a5481e2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274105"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="481bc-103">Eseguire la migrazione dall'autenticazione di appartenenza ASP.NET all'identità di ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="481bc-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="481bc-104">Di [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="481bc-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="481bc-105">Questo articolo illustra lo schema del database per applicazioni ASP.NET utilizzando l'autenticazione di appartenenza di ASP.NET Core 2.0 Identity di migrazione.</span><span class="sxs-lookup"><span data-stu-id="481bc-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="481bc-106">Questo documento fornisce i passaggi necessari per eseguire la migrazione lo schema del database per le app basate su appartenenza ASP.NET allo schema del database utilizzato per l'identità di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="481bc-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="481bc-107">Per ulteriori informazioni sulla migrazione dall'autenticazione basata sull'appartenenza ASP.NET in ASP.NET Identity, vedere [eseguire la migrazione di un'app esistente dall'appartenenza SQL per ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="481bc-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="481bc-108">Per ulteriori informazioni su ASP.NET Identity Core, vedere [Introduzione all'identità su ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="481bc-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="481bc-109">Revisione dello schema di appartenenza</span><span class="sxs-lookup"><span data-stu-id="481bc-109">Review of Membership schema</span></span>

<span data-ttu-id="481bc-110">Prima di ASP.NET 2.0, gli sviluppatori sono state occupano con la creazione dell'intero processo di autenticazione e autorizzazione per le proprie app.</span><span class="sxs-lookup"><span data-stu-id="481bc-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="481bc-111">Con ASP.NET 2.0, è stato introdotto l'appartenenza, fornendo una soluzione di boilerplate per la gestione della sicurezza all'interno di App ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="481bc-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="481bc-112">Gli sviluppatori sono in grado di eseguire l'avvio di uno schema in un database di SQL Server con il [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) comando.</span><span class="sxs-lookup"><span data-stu-id="481bc-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="481bc-113">Dopo aver eseguito questo comando, le tabelle seguenti sono state create nel database.</span><span class="sxs-lookup"><span data-stu-id="481bc-113">After running this command, the following tables were created in the database.</span></span>

  ![Tabelle delle appartenenze](identity/_static/membership-tables.png)

<span data-ttu-id="481bc-115">Per migrare le app esistenti ASP.NET Core 2.0 Identity, i dati in queste tabelle devono eseguire la migrazione di tabelle utilizzate per il nuovo schema di identità.</span><span class="sxs-lookup"><span data-stu-id="481bc-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="481bc-116">Schema ASP.NET 2.0 di identità dei componenti di base</span><span class="sxs-lookup"><span data-stu-id="481bc-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="481bc-117">ASP.NET Core 2.0 segue il [identità](/aspnet/identity/index) principio introdotto in ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="481bc-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="481bc-118">Sebbene il principio è condiviso, l'implementazione tra i Framework è diversa, anche tra le versioni di ASP.NET Core (vedere [eseguire la migrazione di autenticazione e identità per ASP.NET 2.0 Core](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="481bc-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="481bc-119">Il modo più rapido per visualizzare lo schema per l'identità di ASP.NET Core 2.0 consiste nel creare una nuova app di ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="481bc-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="481bc-120">Seguire questi passaggi in Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="481bc-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="481bc-121">Selezionare **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="481bc-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="481bc-122">Creare un nuovo **applicazione Web di ASP.NET Core**e denominare il progetto *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="481bc-122">Create a new **ASP.NET Core Web Application**, and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="481bc-123">Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="481bc-123">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span> <span data-ttu-id="481bc-124">Questo modello genera un [pagine Razor](xref:razor-pages/index) app.</span><span class="sxs-lookup"><span data-stu-id="481bc-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="481bc-125">Prima di fare clic **OK**, fare clic su **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="481bc-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="481bc-126">Scegliere **singoli account utente di** per i modelli di identità.</span><span class="sxs-lookup"><span data-stu-id="481bc-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="481bc-127">Infine, fare clic su **OK**, quindi **OK**.</span><span class="sxs-lookup"><span data-stu-id="481bc-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="481bc-128">Visual Studio crea un progetto usando il modello ASP.NET Identity Core.</span><span class="sxs-lookup"><span data-stu-id="481bc-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="481bc-129">Usa identità ASP.NET Core 2.0 [Entity Framework Core](/ef/core) per interagire con il database di archiviare i dati di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="481bc-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="481bc-130">Affinché l'app appena creato funzionare, deve essere presente un database per archiviare i dati.</span><span class="sxs-lookup"><span data-stu-id="481bc-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="481bc-131">Dopo aver creato una nuova app, il modo più rapido per controllare lo schema in un ambiente di database consiste nel creare il database utilizzando le migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="481bc-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="481bc-132">Questo processo viene creato un database, sia localmente o in altre posizioni, che riproduce tale schema.</span><span class="sxs-lookup"><span data-stu-id="481bc-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="481bc-133">Vedere la documentazione precedente per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="481bc-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="481bc-134">Per creare un database con lo schema di ASP.NET Identity Core, eseguire la `Update-Database` comando in Visual Studio **Package Manager Console** finestra (PMC)&mdash;si trova in corrispondenza **strumenti**  >  **Gestione pacchetti NuGet** > **Console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="481bc-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="481bc-135">PMC supporta l'esecuzione di comandi Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="481bc-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="481bc-136">Entity Framework comandi usano la stringa di connessione per il database specificato *appSettings*.</span><span class="sxs-lookup"><span data-stu-id="481bc-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="481bc-137">La seguente stringa di connessione è destinato a un database nel *localhost* denominata *asp-net-core-identity*.</span><span class="sxs-lookup"><span data-stu-id="481bc-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="481bc-138">In questa impostazione, Entity Framework è configurato per utilizzare il `DefaultConnection` stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="481bc-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="481bc-139">Questo comando crea il database specificato con lo schema e tutti i dati necessari per l'inizializzazione di app.</span><span class="sxs-lookup"><span data-stu-id="481bc-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="481bc-140">Nella figura seguente viene illustrata la struttura della tabella che viene creata con i passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="481bc-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Tabelle di identità](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="481bc-142">eseguire la migrazione dello schema</span><span class="sxs-lookup"><span data-stu-id="481bc-142">Migrate the schema</span></span>

<span data-ttu-id="481bc-143">Esistono differenze minime nei campi relativi all'appartenenza e ASP.NET Identity Core e strutture di tabelle.</span><span class="sxs-lookup"><span data-stu-id="481bc-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="481bc-144">Il modello è cambiato sostanzialmente per l'autenticazione/autorizzazione con le app ASP.NET e ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="481bc-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="481bc-145">Gli oggetti principali che sono ancora utilizzati con identità sono *gli utenti* e *ruoli*.</span><span class="sxs-lookup"><span data-stu-id="481bc-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="481bc-146">Ecco le tabelle di mapping per *gli utenti*, *ruoli*, e *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="481bc-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="481bc-147">Utenti</span><span class="sxs-lookup"><span data-stu-id="481bc-147">Users</span></span>

| <span data-ttu-id="481bc-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="481bc-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="481bc-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="481bc-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="481bc-150">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="481bc-150">**Field Name**</span></span> | <span data-ttu-id="481bc-151">**Type**</span><span class="sxs-lookup"><span data-stu-id="481bc-151">**Type**</span></span>  |   <span data-ttu-id="481bc-152">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="481bc-152">**Field Name**</span></span> | <span data-ttu-id="481bc-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="481bc-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="481bc-154">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="481bc-155">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-155">string</span></span>
|`UserName` | <span data-ttu-id="481bc-156">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="481bc-157">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-157">string</span></span>
|`Email` | <span data-ttu-id="481bc-158">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="481bc-159">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="481bc-160">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="481bc-161">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="481bc-162">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="481bc-163">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="481bc-164">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="481bc-165">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="481bc-166">bit</span><span class="sxs-lookup"><span data-stu-id="481bc-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="481bc-167">bit</span><span class="sxs-lookup"><span data-stu-id="481bc-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="481bc-168">Non tutti i mapping dei campi sono simili alle relazioni uno a uno rispetto all'appartenenza di ASP.NET Identity Core.</span><span class="sxs-lookup"><span data-stu-id="481bc-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="481bc-169">Nella tabella precedente accetta lo schema utente di appartenenza predefinito e viene eseguito il mapping allo schema di ASP.NET Identity Core.</span><span class="sxs-lookup"><span data-stu-id="481bc-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="481bc-170">Altri campi personalizzati che sono stati utilizzati per l'appartenenza è necessario eseguire il mapping manualmente.</span><span class="sxs-lookup"><span data-stu-id="481bc-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="481bc-171">In questo mapping, è disponibile alcuna mappa per le password, come i criteri password e password sali non eseguire la migrazione tra i due.</span><span class="sxs-lookup"><span data-stu-id="481bc-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="481bc-172">**Si consiglia di lasciare la password come null e chiedere agli utenti di reimpostare le proprie password.**</span><span class="sxs-lookup"><span data-stu-id="481bc-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="481bc-173">In ASP.NET Identity Core, `LockoutEnd` deve essere impostato su una data in futuro se l'utente è bloccato. Come illustrato nello script di migrazione.</span><span class="sxs-lookup"><span data-stu-id="481bc-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="481bc-174">Ruoli</span><span class="sxs-lookup"><span data-stu-id="481bc-174">Roles</span></span>

| <span data-ttu-id="481bc-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="481bc-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="481bc-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="481bc-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="481bc-177">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="481bc-177">**Field Name**</span></span> | <span data-ttu-id="481bc-178">**Type**</span><span class="sxs-lookup"><span data-stu-id="481bc-178">**Type**</span></span>  |   <span data-ttu-id="481bc-179">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="481bc-179">**Field Name**</span></span> | <span data-ttu-id="481bc-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="481bc-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="481bc-181">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-181">string</span></span> | `RoleId` | <span data-ttu-id="481bc-182">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-182">string</span></span>
|`Name` | <span data-ttu-id="481bc-183">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-183">string</span></span> | `RoleName` | <span data-ttu-id="481bc-184">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="481bc-185">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="481bc-186">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="481bc-187">Ruoli utente</span><span class="sxs-lookup"><span data-stu-id="481bc-187">User Roles</span></span>

| <span data-ttu-id="481bc-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="481bc-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="481bc-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="481bc-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="481bc-190">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="481bc-190">**Field Name**</span></span> | <span data-ttu-id="481bc-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="481bc-191">**Type**</span></span>  |   <span data-ttu-id="481bc-192">**Nome del campo**</span><span class="sxs-lookup"><span data-stu-id="481bc-192">**Field Name**</span></span> | <span data-ttu-id="481bc-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="481bc-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="481bc-194">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-194">string</span></span> | `RoleId` | <span data-ttu-id="481bc-195">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-195">string</span></span>
|`UserId` | <span data-ttu-id="481bc-196">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-196">string</span></span> | `UserId` | <span data-ttu-id="481bc-197">stringa</span><span class="sxs-lookup"><span data-stu-id="481bc-197">string</span></span>

<span data-ttu-id="481bc-198">Fare riferimento a tabelle di mapping precedenti durante la creazione di uno script di migrazione per *gli utenti* e *ruoli*.</span><span class="sxs-lookup"><span data-stu-id="481bc-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="481bc-199">Nell'esempio seguente si presuppone che si dispone di due database in un server di database.</span><span class="sxs-lookup"><span data-stu-id="481bc-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="481bc-200">Un database contiene lo schema di appartenenza ASP.NET esistente e i dati.</span><span class="sxs-lookup"><span data-stu-id="481bc-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="481bc-201">L'altro database è stato creato utilizzando i passaggi descritti in precedenza.</span><span class="sxs-lookup"><span data-stu-id="481bc-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="481bc-202">I commenti sono incluse inline per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="481bc-202">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="481bc-203">Dopo il completamento di questo script, l'app di ASP.NET Identity Core creato in precedenza viene popolato con gli utenti di appartenenza.</span><span class="sxs-lookup"><span data-stu-id="481bc-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="481bc-204">Gli utenti devono modificare le password prima di accedere.</span><span class="sxs-lookup"><span data-stu-id="481bc-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="481bc-205">Se il sistema di appartenenze aveva utenti con i nomi utente non corrispondente indirizzo di posta elettronica, le modifiche sono necessari per l'app creata in precedenza questo obiettivo.</span><span class="sxs-lookup"><span data-stu-id="481bc-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="481bc-206">Il modello predefinito prevede `UserName` e `Email` dello stesso.</span><span class="sxs-lookup"><span data-stu-id="481bc-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="481bc-207">Per le situazioni in cui sono diversi, deve essere modificata per utilizzare la procedura di accesso `UserName` invece di `Email`.</span><span class="sxs-lookup"><span data-stu-id="481bc-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="481bc-208">Nel `PageModel` della pagina di accesso, disponibile all'indirizzo *Pages\Account\Login.cshtml.cs*, rimuovere il `[EmailAddress]` dell'attributo dal *posta elettronica* proprietà.</span><span class="sxs-lookup"><span data-stu-id="481bc-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="481bc-209">Rinominarlo in *UserName*.</span><span class="sxs-lookup"><span data-stu-id="481bc-209">Rename it to *UserName*.</span></span> <span data-ttu-id="481bc-210">Questa operazione richiede una modifica ovunque `EmailAddress` vengono indicati nel *vista* e *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="481bc-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="481bc-211">Il risultato è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="481bc-211">The result looks like the following:</span></span>

 ![Account di accesso predefinito](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="481bc-213">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="481bc-213">Next steps</span></span>

<span data-ttu-id="481bc-214">In questa esercitazione è stato descritto come trasferire gli utenti dall'appartenenza SQL all'identità di ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="481bc-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="481bc-215">Per ulteriori informazioni su ASP.NET Identity Core, vedere [Introduzione all'identità](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="481bc-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
