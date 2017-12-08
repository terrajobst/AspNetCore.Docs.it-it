---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Modificare la chiave primaria per gli utenti in ASP.NET Identity | Documenti Microsoft
author: tfitzmac
description: "In Visual Studio 2013, l'applicazione web predefinita viene utilizzato un valore stringa per la chiave per gli account utente. Identità di ASP.NET consente di modificare il tipo del..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>Modifica della chiave primaria per gli utenti in ASP.NET Identity
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In Visual Studio 2013, l'applicazione web predefinita viene utilizzato un valore stringa per la chiave per gli account utente. Identità di ASP.NET consente di modificare il tipo di chiave per soddisfare i requisiti di dati. Ad esempio, è possibile modificare il tipo della chiave da una stringa in un intero.
> 
> Questo argomento viene illustrato come iniziare con il valore predefinito di applicazioni web e modificare la chiave dell'account utente in un intero. È possibile utilizzare le stesse modifiche per implementare qualsiasi tipo di chiave nel progetto. Viene illustrato come apportare queste modifiche nell'applicazione web predefinita, ma è possibile applicare le modifiche simili a un'applicazione personalizzata. Mostra le modifiche necessarie quando si lavora con MVC o Web Form.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Visual Studio 2013 con Update 2 (o versioni successive)
> - ASP.NET Identity 2.1 o versioni successive


Per eseguire i passaggi in questa esercitazione, è necessario disporre di Visual Studio 2013 Update 2 (o versione successiva) e un'applicazione web creata dal modello di applicazione Web ASP.NET. Il modello modificato in Update 3. In questo argomento viene illustrato come modificare il modello in Update 2 e 3 di aggiornamento.

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Modificare il tipo di chiave nella classe di identità utente](#userclass)
- [Aggiungere le classi di identità personalizzate che utilizzano il tipo di chiave](#customclass)
- [Modificare la gestione di utente e di contesto per utilizzare il tipo di chiave](#context)
- [Modificare la configurazione di avvio da utilizzare il tipo di chiave](#startup)
- [Per MVC con Update 2, modificare il AccountController per passare il tipo di chiave](#mvcupdate2)
- [Per MVC con Update 3, per modificare il nome AccountController e ManageController passare il tipo di chiave](#mvcupdate3)
- [Per Web Form con Update 2, modificare le pagine per passare il tipo di chiave Account](#webformsupdate2)
- [Per Web Form con Update 3, modificare le pagine per passare il tipo di chiave Account](#webformsupdate3)
- [Eseguire l'applicazione](#run)
- [Altre risorse](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Modificare il tipo di chiave nella classe di identità utente

Nel progetto creato dal modello di applicazione Web ASP.NET, specificare che la classe ApplicationUser utilizza un numero intero per la chiave per gli account utente. In IdentityModels.cs, modificare la classe ApplicationUser da cui ereditare IdentityUser con un tipo di **int** per il parametro generico TKey. È inoltre possibile passare i nomi di tre classe personalizzata che non ancora implementata.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Il tipo della chiave è stato modificato, ma, per impostazione predefinita, il resto dell'applicazione ancora si presuppone che la chiave è una stringa. È necessario indicare in modo esplicito il tipo di chiave nel codice che assume una stringa.

Nel **ApplicationUser** classe, modificare il **GenerateUserIdentityAsync** metodo per includere int, come illustrato nel seguente codice evidenziato. Questa modifica non è necessaria per i progetti Web Form con il modello di Update 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Aggiungere le classi di identità personalizzate che utilizzano il tipo di chiave

Le altre classi di identità, ad esempio IdentityUserRole IdentityUserClaim, IdentityUserLogin, IdentityRole, nello UserStore, oggetto RoleStore, vengono ancora impostate per usare una chiave di stringa. Creazione di nuove versioni di queste classi che specificano un valore integer per la chiave. Non è necessario fornire più codice di implementazione di queste classi, si imposta int principalmente solo come chiave.

Consente di aggiungere le classi seguenti nel file IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Modificare la gestione di utente e di contesto per utilizzare il tipo di chiave

In IdentityModels.cs, modificare la definizione di **ApplicationDbContext** a utilizzare la nuova classe di personalizzata le classi e un **int** per la chiave, come illustrato nel codice evidenziato di seguito.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Il parametro ThrowIfV1Schema non è più valido nel costruttore. Modificare il costruttore in modo non supera un valore ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Aprire IdentityConfig.cs e modificare il **ApplicationUserManger** classe da utilizzare il nuovo utente archiviare classe per rendere persistenti i dati e un **int** per la chiave.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

Nel modello di Update 3, è necessario modificare la classe ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Modificare la configurazione di avvio da utilizzare il tipo di chiave

In Startup.Auth.cs, sostituire il codice di OnValidateIdentity, come evidenziato qui sotto. Si noti che la definizione di getUserIdCallback, analizza il valore di stringa in un numero intero.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Se il progetto non riconosce l'implementazione generica del **Recuperaidutente** (metodo), potrebbe essere necessario aggiornare il pacchetto NuGet di identità di ASP.NET alla versione 2.1

Per le classi di infrastruttura utilizzate da ASP.NET Identity apportate numerose modifiche. Se si tenta la compilazione del progetto, si noterà numerosi errori. Fortunatamente, gli errori rimanenti sono molto simili. La classe di identità prevede un numero intero per la chiave, ma il controller (o Web Form) passa un valore di stringa. In ogni caso, è necessario eseguire la conversione da una stringa e un numero intero chiamando **Recuperaidutente&lt;int&gt;**. È possibile esaminare l'elenco di errori di compilazione o effettuare le seguenti modifiche.

Le modifiche rimanenti dipendono dal tipo di progetto che si sta creando e quali aggiornamenti sono installati in Visual Studio. È possibile passare direttamente alla sezione pertinente tramite i collegamenti seguenti

- [Per MVC con Update 2, modificare il AccountController per passare il tipo di chiave](#mvcupdate2)
- [Per MVC con Update 3, per modificare il nome AccountController e ManageController passare il tipo di chiave](#mvcupdate3)
- [Per Web Form con Update 2, modificare le pagine per passare il tipo di chiave Account](#webformsupdate2)
- [Per Web Form con Update 3, modificare le pagine per passare il tipo di chiave Account](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Per MVC con Update 2, modificare il AccountController per passare il tipo di chiave

Aprire il file AccountController.cs. È necessario modificare i metodi seguenti.

**ConfirmEmail** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Annullare l'associazione** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

È ora possibile [eseguire l'applicazione](#run) e registrare un nuovo utente.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Per MVC con Update 3, per modificare il nome AccountController e ManageController passare il tipo di chiave

Aprire il file AccountController.cs. È necessario modificare il metodo seguente.

**ConfirmEmail** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Aprire il file ManageController.cs. È necessario modificare i metodi seguenti.

**Indice** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin** metodi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber** metodi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber** (metodo)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

È ora possibile [eseguire l'applicazione](#run) e registrare un nuovo utente.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Per Web Form con Update 2, modificare le pagine per passare il tipo di chiave Account

Per Web Form con Update 2, è necessario modificare le pagine seguenti.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

È ora possibile [eseguire l'applicazione](#run) e registrare un nuovo utente.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Per Web Form con Update 3, modificare le pagine per passare il tipo di chiave Account

Per Web Form con Update 3, è necessario modificare le pagine seguenti.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Eseguire l'applicazione

Sono state tutte le modifiche necessarie per il modello di applicazione Web predefinita. Eseguire l'applicazione e registrare un nuovo utente. Dopo la registrazione dell'utente, si noterà che la tabella AspNetUsers include una colonna di Id che è un numero intero.

![nuova chiave primaria](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Se è stata creata l'identità ASP.NET tabelle con una chiave primaria diversa, è necessario apportare alcune modifiche aggiuntive. Se possibile, è sufficiente eliminare il database esistente. Il database sarà creato di nuovo con la progettazione corretta quando si esegue l'applicazione web e aggiungere un nuovo utente. Se l'eliminazione non è possibile, eseguire le migrazioni code first per modificare le tabelle. Tuttavia, la nuova chiave primaria di integer verrà non essere configurata come una proprietà IDENTITY di SQL nel database. È necessario impostare manualmente la colonna Id come un'identità.

<a id="other"></a>
## <a name="other-resources"></a>Altre risorse

- [Panoramica dei provider di archiviazione personalizzato per l'identità ASP.NET](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [La migrazione di un sito Web esistente dall'appartenenza SQL per ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrazione dei dati per l'appartenenza e i profili di ASP.NET Identity Provider Universal](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Applicazione di esempio](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) con la chiave primaria modificata
