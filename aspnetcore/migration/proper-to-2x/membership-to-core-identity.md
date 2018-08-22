---
title: Eseguire la migrazione dall'autenticazione di appartenenza ASP.NET a ASP.NET Core 2.0 Identity
author: isaac2004
description: Informazioni su come eseguire la migrazione di App ASP.NET esistenti utilizzando l'autenticazione di appartenenza ad ASP.NET Core 2.0 Identity.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 82158ec500151a0bb61fb1da55a53684367d9a4e
ms.sourcegitcommit: 2e054638b69f2b14f6d67d9fa3664999172ee1b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2018
ms.locfileid: "41823717"
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

* Selezionare **File** > **Nuovo** > **Progetto**.
* Creare una nuova **applicazione Web ASP.NET Core** e denominare il progetto *CoreIdentitySample*.
* Selezionare **ASP.NET Core 2.0** nell'elenco a discesa e quindi selezionare **applicazione Web**. Questo modello genera un [pagine Razor](xref:razor-pages/index) app. Prima di fare clic **OK**, fare clic su **Modifica autenticazione**.
* Scegli **singoli account utente** per i modelli di identità. Infine, fare clic su **OK**, quindi **OK**. Visual Studio crea un progetto usando il modello di ASP.NET Core Identity.

Usa identità di ASP.NET Core 2.0 [Entity Framework Core](/ef/core) per interagire con il database di archiviare i dati di autenticazione. Affinché l'app appena creata per funzionare, deve essere un database per archiviare i dati. Dopo aver creato una nuova app, il modo più rapido per controllare lo schema in un ambiente di database consiste nel creare il database utilizzando le migrazioni di Entity Framework. Questo processo crea un database, sia localmente o in altro modo, che simula quello schema. Vedere la documentazione precedente per altre informazioni.

Per creare un database con lo schema di ASP.NET Core Identity, eseguire la `Update-Database` comando in Visual Studio **Console di gestione pacchetti** finestra&mdash;si trova in corrispondenza **strumenti**  >  **Gestisci pacchetti NuGet** > **Package Manager Console**. Console di gestione pacchetti supporta i comandi di Entity Framework in esecuzione.

I comandi Entity Framework utilizzano la stringa di connessione per il database specificato nel *appSettings. JSON*. La stringa di connessione seguente destinato a un database sul *localhost* denominate *asp-net-core-identity*. In questa impostazione, Entity Framework è configurato per usare il `DefaultConnection` stringa di connessione.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

Questo comando compila il database specificato con lo schema e tutti i dati necessari per l'inizializzazione dell'app. La figura seguente viene illustrata la struttura della tabella che viene creata con i passaggi precedenti.

   ![Tabelle di identità](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Eseguire la migrazione dello schema

Esistono differenze minime nei campi relativi all'appartenenza e ASP.NET Core Identity e strutture di tabella. Il modello è stato modificato in modo sostanziale per l'autenticazione/autorizzazione con le app ASP.NET Core e ASP.NET. Gli oggetti chiave vengono ancora utilizzati con identità sono *gli utenti* e *ruoli*. Di seguito sono le tabelle di mapping per *gli utenti*, *ruoli*, e *UserRoles*.

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
> Non tutti i mapping dei campi sono simili alle relazioni uno a uno dall'appartenenza ad ASP.NET Core Identity. Nella tabella precedente accetta lo schema utente di appartenenza predefinito e ne esegue il mapping allo schema di ASP.NET Core Identity. Gli altri campi personalizzati che sono stati usati per l'appartenenza è necessario eseguire il mapping manualmente. In questo mapping, non è Nessuna mappa per le password, come i criteri password e password sali non esegue la migrazione tra i due. **Si consiglia di lasciare la password come null e chiedere agli utenti di reimpostare le password.** In ASP.NET Core Identity, `LockoutEnd` deve essere impostato su una data in futuro se l'utente è bloccato. Come illustrato nello script di migrazione.

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

Fare riferimento a tabelle di mapping precedenti durante la creazione di uno script di migrazione per *gli utenti* e *ruoli*. Nell'esempio seguente si presuppone due database in un server di database. Un database contiene lo schema di appartenenza ASP.NET esistente e i dati. L'altro database è stato creato usando i passaggi descritti in precedenza. I commenti sono incluse inline per altri dettagli.

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
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be initialized as a new ID.
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

Dopo il completamento di questo script, l'app ASP.NET Core Identity creato in precedenza viene popolato con gli utenti di appartenenza. Gli utenti dovranno modificare le password prima di accedere.

> [!NOTE]
> Se il sistema di appartenenze era gli utenti con i nomi utente che non corrispondono l'indirizzo di posta elettronica, le modifiche sono necessarie per l'app creata in precedenza per rispondere a questa esigenza. Prevede che il modello predefinito `UserName` e `Email` sia lo stesso. Per le situazioni in cui si diverso, deve essere modificato per usare la procedura di accesso `UserName` invece di `Email`.

Nel `PageModel` della pagina di accesso, disponibile all'indirizzo *Pages\Account\Login.cshtml.cs*, rimuovere il `[EmailAddress]` dell'attributo dal *messaggio di posta elettronica* proprietà. Rinominarlo *UserName*. Questa operazione richiede una modifica ovunque `EmailAddress` vengono indicati nel *visualizzazione* e *PageModel*. Il risultato è simile al seguente:

 ![Account di accesso predefinito](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato descritto come trasferire gli utenti dall'appartenenza SQL ad ASP.NET Core 2.0 Identity. Per altre informazioni su ASP.NET Core Identity, vedere [Introduzione all'identità](xref:security/authentication/identity).
