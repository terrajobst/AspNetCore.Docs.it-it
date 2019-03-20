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
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Eseguire la migrazione dall'autenticazione di appartenenza ASP.NET a ASP.NET Core 2.0 Identity

Di [Isaac Levin](https://isaaclevin.com)

Questo articolo illustra la migrazione lo schema del database per le app ASP.NET usando l'autenticazione di appartenenza ad ASP.NET Core 2.0 Identity.

> [!NOTE]
> Questo documento fornisce i passaggi necessari per eseguire la migrazione lo schema del database per le app basate sul sistema di appartenenze ASP.NET per lo schema del database utilizzato per ASP.NET Core Identity. Per altre informazioni sulla migrazione dall'autenticazione basata sull'appartenenza ASP.NET ad ASP.NET Identity, vedere [eseguire la migrazione di un'app esistente dall'appartenenza SQL ad ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Per altre informazioni su ASP.NET Core Identity, vedere [Introduzione all'identità in ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Revisione dello schema di appartenenza

Prima di ASP.NET 2.0, gli sviluppatori sono state il compito di creare l'intero processo di autenticazione e autorizzazione per le proprie app. Con ASP.NET 2.0, è stata introdotta l'appartenenza, fornendo una soluzione standard per la gestione della sicurezza all'interno delle App ASP.NET. Gli sviluppatori sono in grado di eseguire l'avvio di uno schema in un database di SQL Server con il [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) comando. Dopo aver eseguito questo comando, le tabelle seguenti sono state create nel database.

  ![Tabelle di appartenenza](identity/_static/membership-tables.png)

Per eseguire la migrazione delle App esistenti per identità di ASP.NET Core 2.0, i dati in queste tabelle devono essere eseguita la migrazione per le tabelle usate dal nuovo schema di identità.

## <a name="aspnet-core-identity-20-schema"></a>Schema ASP.NET Core Identity 2.0

ASP.NET Core 2.0 segue il [identità](/aspnet/identity/index) principio introdotta in ASP.NET 4.5. Anche se il principio è condiviso, l'implementazione tra i Framework è diversa, anche tra le versioni di ASP.NET Core (vedere [eseguire la migrazione di autenticazione e identità ad ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).

Il modo più rapido per visualizzare lo schema per l'identità di ASP.NET Core 2.0 consiste nel creare una nuova app ASP.NET Core 2.0. Seguire questi passaggi in Visual Studio 2017:

1. Selezionare **File** > **Nuovo** > **Progetto**.
1. Creare una nuova **applicazione Web ASP.NET Core** progetto denominato *CoreIdentitySample*.
1. Selezionare **ASP.NET Core 2.0** nell'elenco a discesa e quindi selezionare **applicazione Web**. Questo modello genera un [pagine Razor](xref:razor-pages/index) app. Prima di fare clic **OK**, fare clic su **Modifica autenticazione**.
1. Scegli **singoli account utente** per i modelli di identità. Infine, fare clic su **OK**, quindi **OK**. Visual Studio crea un progetto usando il modello di ASP.NET Core Identity.
1. Selezionare **degli strumenti** > **Gestione pacchetti NuGet** > **Package Manager Console** per aprire la **Consoledigestionepacchetti** Finestra.
1. Passare alla radice del progetto nella console di gestione pacchetti ed eseguire la [Entity Framework (EF) Core](/ef/core) `Update-Database` comando.

    Identità di ASP.NET Core 2.0 usa EF Core per interagire con il database di archiviare i dati di autenticazione. Affinché l'app appena creata per funzionare, deve essere un database per archiviare i dati. Dopo aver creato una nuova app, il modo più rapido per controllare lo schema in un ambiente di database consiste nel creare il database usando [migrazioni EF Core](/ef/core/managing-schemas/migrations/). Questo processo crea un database, sia localmente o in altro modo, che simula quello schema. Vedere la documentazione precedente per altre informazioni.

    Comandi di EF Core usano la stringa di connessione per il database specificato nel *appSettings. JSON*. La stringa di connessione seguente destinato a un database sul *localhost* denominate *asp-net-core-identity*. In questa impostazione, EF Core è configurato per usare il `DefaultConnection` stringa di connessione.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. Selezionare **View** > **Esplora oggetti di SQL Server**. Espandere il nodo corrispondente al nome del database specificato nella `ConnectionStrings:DefaultConnection` proprietà di *appSettings. JSON*.

    Il `Update-Database` comando ha creato il database specificato con lo schema e tutti i dati necessari per l'inizializzazione dell'app. La figura seguente viene illustrata la struttura della tabella che viene creata con i passaggi precedenti.

    ![Tabelle di identità](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Eseguire la migrazione dello schema

Esistono differenze minime nei campi relativi all'appartenenza e ASP.NET Core Identity e strutture di tabella. Il modello è stato modificato in modo sostanziale per l'autenticazione/autorizzazione con le app ASP.NET Core e ASP.NET. Gli oggetti chiave vengono ancora utilizzati con identità sono *gli utenti* e *ruoli*. Di seguito sono le tabelle di mapping per *gli utenti*, *ruoli*, e *UserRoles*.

### <a name="users"></a>Utenti

|*Identity<br>(dbo.AspNetUsers)*        ||*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*||
|----------------------------------------|-----------------------------------------------------------|
|**Nome del campo**                 |**Type**|**Nome del campo**                                    |**Type**|
|`Id`                           |string  |`aspnet_Users.UserId`                             |string  |
|`UserName`                     |string  |`aspnet_Users.UserName`                           |string  |
|`Email`                        |string  |`aspnet_Membership.Email`                         |string  |
|`NormalizedUserName`           |string  |`aspnet_Users.LoweredUserName`                    |string  |
|`NormalizedEmail`              |string  |`aspnet_Membership.LoweredEmail`                  |string  |
|`PhoneNumber`                  |string  |`aspnet_Users.MobileAlias`                        |string  |
|`LockoutEnabled`               |bit     |`aspnet_Membership.IsLockedOut`                   |bit     |

> [!NOTE]
> Non tutti i mapping dei campi sono simili alle relazioni uno a uno dall'appartenenza ad ASP.NET Core Identity. Nella tabella precedente accetta lo schema utente di appartenenza predefinito e ne esegue il mapping allo schema di ASP.NET Core Identity. Gli altri campi personalizzati che sono stati usati per l'appartenenza è necessario eseguire il mapping manualmente. In questo mapping, non è Nessuna mappa per le password, come i criteri password e password sali non esegue la migrazione tra i due. **Si consiglia di lasciare la password come null e chiedere agli utenti di reimpostare le password.** In ASP.NET Core Identity, `LockoutEnd` deve essere impostato su una data in futuro se l'utente è bloccato. Come illustrato nello script di migrazione.

### <a name="roles"></a>Ruoli

|*Identity<br>(dbo.AspNetRoles)*        ||*Membership<br>(dbo.aspnet_Roles)*||
|----------------------------------------|-----------------------------------|
|**Nome del campo**                 |**Type**|**Nome del campo**   |**Type**         |
|`Id`                           |string  |`RoleId`         | string          |
|`Name`                         |string  |`RoleName`       | string          |
|`NormalizedName`               |string  |`LoweredRoleName`| string          |

### <a name="user-roles"></a>Ruoli utente

|*Identity<br>(dbo.AspNetUserRoles)*||*Membership<br>(dbo.aspnet_UsersInRoles)*||
|------------------------------------|------------------------------------------|
|**Nome del campo**           |**Type**  |**Nome del campo**|**Type**                   |
|`RoleId`                 |string    |`RoleId`      |string                     |
|`UserId`                 |string    |`UserId`      |string                     |

Fare riferimento a tabelle di mapping precedenti durante la creazione di uno script di migrazione per *gli utenti* e *ruoli*. Nell'esempio seguente si presuppone due database in un server di database. Un database contiene lo schema di appartenenza ASP.NET esistente e i dati. L'altra *CoreIdentitySample* database è stato creato utilizzando i passaggi descritti in precedenza. I commenti sono incluse inline per altri dettagli.

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

Dopo il completamento dello script precedente, l'app ASP.NET Core Identity creato in precedenza viene popolato con gli utenti di appartenenza. Gli utenti dovranno modificare le password prima di accedere.

> [!NOTE]
> Se il sistema di appartenenze era gli utenti con i nomi utente che non corrispondono l'indirizzo di posta elettronica, le modifiche sono necessarie per l'app creata in precedenza per rispondere a questa esigenza. Prevede che il modello predefinito `UserName` e `Email` sia lo stesso. Per le situazioni in cui si diverso, deve essere modificato per usare la procedura di accesso `UserName` invece di `Email`.

Nel `PageModel` della pagina di accesso, disponibile all'indirizzo *Pages\Account\Login.cshtml.cs*, rimuovere il `[EmailAddress]` dell'attributo dal *messaggio di posta elettronica* proprietà. Rinominarlo *UserName*. Questa operazione richiede una modifica ovunque `EmailAddress` vengono indicati nel *visualizzazione* e *PageModel*. Il risultato è simile al seguente:

 ![Account di accesso predefinito](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato descritto come trasferire gli utenti dall'appartenenza SQL ad ASP.NET Core 2.0 Identity. Per altre informazioni su ASP.NET Core Identity, vedere [Introduzione all'identità](xref:security/authentication/identity).
