---
title: Eseguire la migrazione dall'autenticazione di appartenenza ASP.NET all'identità di ASP.NET Core 2.0
author: isaac2004
description: Informazioni su come eseguire la migrazione di applicazioni ASP.NET esistenti utilizzando l'autenticazione di appartenenza di ASP.NET Core 2.0 Identity.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Eseguire la migrazione dall'autenticazione di appartenenza ASP.NET all'identità di ASP.NET Core 2.0

Di [Isaac Levin](https://isaaclevin.com)

Questo articolo illustra lo schema del database per applicazioni ASP.NET utilizzando l'autenticazione di appartenenza di ASP.NET Core 2.0 Identity di migrazione.

> [!NOTE]
> Questo documento fornisce i passaggi necessari per eseguire la migrazione lo schema del database per le app basate su appartenenza ASP.NET allo schema del database utilizzato per l'identità di ASP.NET Core. Per ulteriori informazioni sulla migrazione dall'autenticazione basata sull'appartenenza ASP.NET in ASP.NET Identity, vedere [eseguire la migrazione di un'app esistente dall'appartenenza SQL per ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Per ulteriori informazioni su ASP.NET Identity Core, vedere [Introduzione all'identità su ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Revisione dello schema di appartenenza

Prima di ASP.NET 2.0, gli sviluppatori sono state occupano con la creazione dell'intero processo di autenticazione e autorizzazione per le proprie app. Con ASP.NET 2.0, è stato introdotto l'appartenenza, fornendo una soluzione di boilerplate per la gestione della sicurezza all'interno di App ASP.NET. Gli sviluppatori sono in grado di eseguire l'avvio di uno schema in un database di SQL Server con il [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) comando. Dopo aver eseguito questo comando, le tabelle seguenti sono state create nel database.

  ![Tabelle delle appartenenze](identity/_static/membership-tables.png)

Per migrare le app esistenti ASP.NET Core 2.0 Identity, i dati in queste tabelle devono eseguire la migrazione di tabelle utilizzate per il nuovo schema di identità.

## <a name="aspnet-core-identity-20-schema"></a>Schema ASP.NET 2.0 di identità dei componenti di base

ASP.NET Core 2.0 segue il [identità](/aspnet/identity/index) principio introdotto in ASP.NET 4.5. Sebbene il principio è condiviso, l'implementazione tra i Framework è diversa, anche tra le versioni di ASP.NET Core (vedere [eseguire la migrazione di autenticazione e identità per ASP.NET 2.0 Core](xref:migration/1x-to-2x/index)).

Il modo più rapido per visualizzare lo schema per l'identità di ASP.NET Core 2.0 consiste nel creare una nuova app di ASP.NET Core 2.0. Seguire questi passaggi in Visual Studio 2017:

* Selezionare **File** > **Nuovo** > **Progetto**.
* Creare un nuovo **applicazione Web di ASP.NET Core**e denominare il progetto *CoreIdentitySample*.
* Selezionare **ASP.NET Core 2.0** nell'elenco a discesa, quindi selezionare **applicazione Web**. Questo modello genera un [pagine Razor](xref:mvc/razor-pages/index) app. Prima di fare clic **OK**, fare clic su **Modifica autenticazione**.
* Scegliere **singoli account utente di** per i modelli di identità. Infine, fare clic su **OK**, quindi **OK**. Visual Studio crea un progetto usando il modello ASP.NET Identity Core.

Usa identità ASP.NET Core 2.0 [Entity Framework Core](/ef/core) per interagire con il database di archiviare i dati di autenticazione. Affinché l'app appena creato funzionare, deve essere presente un database per archiviare i dati. Dopo aver creato una nuova app, il modo più rapido per controllare lo schema in un ambiente di database consiste nel creare il database utilizzando le migrazioni di Entity Framework. Questo processo viene creato un database, sia localmente o in altre posizioni, che riproduce tale schema. Vedere la documentazione precedente per ulteriori informazioni.

Per creare un database con lo schema di ASP.NET Identity Core, eseguire la `Update-Database` comando in Visual Studio **Package Manager Console** finestra (PMC)&mdash;si trova in corrispondenza **strumenti**  >  **Gestione pacchetti NuGet** > **Console di gestione pacchetti**. PMC supporta l'esecuzione di comandi Entity Framework.

Entity Framework comandi usano la stringa di connessione per il database specificato *appSettings*. La seguente stringa di connessione è destinato a un database nel *localhost* denominata *asp-net-core-identity*. In questa impostazione, Entity Framework è configurato per utilizzare il `DefaultConnection` stringa di connessione.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

Questo comando crea il database specificato con lo schema e tutti i dati necessari per l'inizializzazione di app. Nella figura seguente viene illustrata la struttura della tabella che viene creata con i passaggi precedenti.

   ![Tabelle di identità](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>eseguire la migrazione dello schema

Esistono differenze minime nei campi relativi all'appartenenza e ASP.NET Identity Core e strutture di tabelle. Il modello è cambiato sostanzialmente per l'autenticazione/autorizzazione con le app ASP.NET e ASP.NET Core. Gli oggetti principali che sono ancora utilizzati con identità sono *gli utenti* e *ruoli*. Ecco le tabelle di mapping per *gli utenti*, *ruoli*, e *UserRoles*.

### <a name="users"></a>Utenti

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **Nome del campo** | **Type**  |   **Nome del campo** | **Type**  |
|`Id` | stringa | `aspnet_Users.UserId` | stringa
|`UserName` | stringa | `aspnet_Users.UserName` | stringa
|`Email` | stringa | `aspnet_Membership.Email` | stringa
|`NormalizedUserName` | stringa | `aspnet_Users.LoweredUserName` | stringa
|`NormalizedEmail` | stringa | `aspnet_Membership.LoweredEmail` | stringa
|`PhoneNumber` | stringa | `aspnet_Users.MobileAlias` | stringa
|`LockoutEnabled` | bit | `aspnet_Membership.IsLockedOut` | bit

> [!NOTE]
> Non tutti i mapping dei campi sono simili alle relazioni uno a uno rispetto all'appartenenza di ASP.NET Identity Core. Nella tabella precedente accetta lo schema utente di appartenenza predefinito e viene eseguito il mapping allo schema di ASP.NET Identity Core. Altri campi personalizzati che sono stati utilizzati per l'appartenenza è necessario eseguire il mapping manualmente. In questo mapping, è disponibile alcuna mappa per le password, come i criteri password e password sali non eseguire la migrazione tra i due. **Si consiglia di lasciare la password come null e chiedere agli utenti di reimpostare le proprie password.** In ASP.NET Identity Core, `LockoutEnd` deve essere impostato su una data in futuro se l'utente è bloccato. Come illustrato nello script di migrazione.

### <a name="roles"></a>Ruoli

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **Nome del campo** | **Type**  |   **Nome del campo** | **Type**  |
|`Id` | stringa | `RoleId` | stringa
|`Name` | stringa | `RoleName` | stringa
|`NormalizedName` | stringa | `LoweredRoleName` | stringa

### <a name="user-roles"></a>Ruoli utente

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **Nome del campo** | **Type**  |   **Nome del campo** | **Type**  |
|`RoleId` | stringa | `RoleId` | stringa
|`UserId` | stringa | `UserId` | stringa

Fare riferimento a tabelle di mapping precedenti durante la creazione di uno script di migrazione per *gli utenti* e *ruoli*. Nell'esempio seguente si presuppone che si dispone di due database in un server di database. Un database contiene lo schema di appartenenza ASP.NET esistente e i dati. L'altro database è stato creato utilizzando i passaggi descritti in precedenza. I commenti sono incluse inline per altri dettagli.

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

Dopo il completamento di questo script, l'app di ASP.NET Identity Core creato in precedenza viene popolato con gli utenti di appartenenza. Gli utenti devono modificare le password prima di accedere.

> [!NOTE]
> Se il sistema di appartenenze aveva utenti con i nomi utente non corrispondente indirizzo di posta elettronica, le modifiche sono necessari per l'app creata in precedenza questo obiettivo. Il modello predefinito prevede `UserName` e `Email` dello stesso. Per le situazioni in cui sono diversi, deve essere modificata per utilizzare la procedura di accesso `UserName` invece di `Email`.

Nel `PageModel` della pagina di accesso, disponibile all'indirizzo *Pages\Account\Login.cshtml.cs*, rimuovere il `[EmailAddress]` dell'attributo dal *posta elettronica* proprietà. Rinominarlo in *UserName*. Questa operazione richiede una modifica ovunque `EmailAddress` vengono indicati nel *vista* e *PageModel*. Il risultato è simile al seguente:

 ![Account di accesso predefinito](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato descritto come trasferire gli utenti dall'appartenenza SQL all'identità di ASP.NET Core 2.0. Per ulteriori informazioni su ASP.NET Identity Core, vedere [Introduzione all'identità](xref:security/authentication/identity).
