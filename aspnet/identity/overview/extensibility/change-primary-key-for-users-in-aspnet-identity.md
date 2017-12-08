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
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="57117-104">Modifica della chiave primaria per gli utenti in ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="57117-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="57117-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="57117-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="57117-106">In Visual Studio 2013, l'applicazione web predefinita viene utilizzato un valore stringa per la chiave per gli account utente.</span><span class="sxs-lookup"><span data-stu-id="57117-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="57117-107">Identità di ASP.NET consente di modificare il tipo di chiave per soddisfare i requisiti di dati.</span><span class="sxs-lookup"><span data-stu-id="57117-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="57117-108">Ad esempio, è possibile modificare il tipo della chiave da una stringa in un intero.</span><span class="sxs-lookup"><span data-stu-id="57117-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="57117-109">Questo argomento viene illustrato come iniziare con il valore predefinito di applicazioni web e modificare la chiave dell'account utente in un intero.</span><span class="sxs-lookup"><span data-stu-id="57117-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="57117-110">È possibile utilizzare le stesse modifiche per implementare qualsiasi tipo di chiave nel progetto.</span><span class="sxs-lookup"><span data-stu-id="57117-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="57117-111">Viene illustrato come apportare queste modifiche nell'applicazione web predefinita, ma è possibile applicare le modifiche simili a un'applicazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="57117-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="57117-112">Mostra le modifiche necessarie quando si lavora con MVC o Web Form.</span><span class="sxs-lookup"><span data-stu-id="57117-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="57117-113">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="57117-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="57117-114">Visual Studio 2013 con Update 2 (o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="57117-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="57117-115">ASP.NET Identity 2.1 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="57117-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="57117-116">Per eseguire i passaggi in questa esercitazione, è necessario disporre di Visual Studio 2013 Update 2 (o versione successiva) e un'applicazione web creata dal modello di applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="57117-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="57117-117">Il modello modificato in Update 3.</span><span class="sxs-lookup"><span data-stu-id="57117-117">The template changed in Update 3.</span></span> <span data-ttu-id="57117-118">In questo argomento viene illustrato come modificare il modello in Update 2 e 3 di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="57117-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="57117-119">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="57117-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="57117-120">Modificare il tipo di chiave nella classe di identità utente</span><span class="sxs-lookup"><span data-stu-id="57117-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="57117-121">Aggiungere le classi di identità personalizzate che utilizzano il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="57117-122">Modificare la gestione di utente e di contesto per utilizzare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="57117-123">Modificare la configurazione di avvio da utilizzare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="57117-124">Per MVC con Update 2, modificare il AccountController per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="57117-125">Per MVC con Update 3, per modificare il nome AccountController e ManageController passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="57117-126">Per Web Form con Update 2, modificare le pagine per passare il tipo di chiave Account</span><span class="sxs-lookup"><span data-stu-id="57117-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="57117-127">Per Web Form con Update 3, modificare le pagine per passare il tipo di chiave Account</span><span class="sxs-lookup"><span data-stu-id="57117-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="57117-128">Eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="57117-128">Run application</span></span>](#run)
- [<span data-ttu-id="57117-129">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="57117-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="57117-130">Modificare il tipo di chiave nella classe di identità utente</span><span class="sxs-lookup"><span data-stu-id="57117-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="57117-131">Nel progetto creato dal modello di applicazione Web ASP.NET, specificare che la classe ApplicationUser utilizza un numero intero per la chiave per gli account utente.</span><span class="sxs-lookup"><span data-stu-id="57117-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="57117-132">In IdentityModels.cs, modificare la classe ApplicationUser da cui ereditare IdentityUser con un tipo di **int** per il parametro generico TKey.</span><span class="sxs-lookup"><span data-stu-id="57117-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="57117-133">È inoltre possibile passare i nomi di tre classe personalizzata che non ancora implementata.</span><span class="sxs-lookup"><span data-stu-id="57117-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="57117-134">Il tipo della chiave è stato modificato, ma, per impostazione predefinita, il resto dell'applicazione ancora si presuppone che la chiave è una stringa.</span><span class="sxs-lookup"><span data-stu-id="57117-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="57117-135">È necessario indicare in modo esplicito il tipo di chiave nel codice che assume una stringa.</span><span class="sxs-lookup"><span data-stu-id="57117-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="57117-136">Nel **ApplicationUser** classe, modificare il **GenerateUserIdentityAsync** metodo per includere int, come illustrato nel seguente codice evidenziato.</span><span class="sxs-lookup"><span data-stu-id="57117-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="57117-137">Questa modifica non è necessaria per i progetti Web Form con il modello di Update 3.</span><span class="sxs-lookup"><span data-stu-id="57117-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="57117-138">Aggiungere le classi di identità personalizzate che utilizzano il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="57117-139">Le altre classi di identità, ad esempio IdentityUserRole IdentityUserClaim, IdentityUserLogin, IdentityRole, nello UserStore, oggetto RoleStore, vengono ancora impostate per usare una chiave di stringa.</span><span class="sxs-lookup"><span data-stu-id="57117-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="57117-140">Creazione di nuove versioni di queste classi che specificano un valore integer per la chiave.</span><span class="sxs-lookup"><span data-stu-id="57117-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="57117-141">Non è necessario fornire più codice di implementazione di queste classi, si imposta int principalmente solo come chiave.</span><span class="sxs-lookup"><span data-stu-id="57117-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="57117-142">Consente di aggiungere le classi seguenti nel file IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="57117-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="57117-143">Modificare la gestione di utente e di contesto per utilizzare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="57117-144">In IdentityModels.cs, modificare la definizione di **ApplicationDbContext** a utilizzare la nuova classe di personalizzata le classi e un **int** per la chiave, come illustrato nel codice evidenziato di seguito.</span><span class="sxs-lookup"><span data-stu-id="57117-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="57117-145">Il parametro ThrowIfV1Schema non è più valido nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="57117-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="57117-146">Modificare il costruttore in modo non supera un valore ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="57117-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="57117-147">Aprire IdentityConfig.cs e modificare il **ApplicationUserManger** classe da utilizzare il nuovo utente archiviare classe per rendere persistenti i dati e un **int** per la chiave.</span><span class="sxs-lookup"><span data-stu-id="57117-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="57117-148">Nel modello di Update 3, è necessario modificare la classe ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="57117-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="57117-149">Modificare la configurazione di avvio da utilizzare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="57117-150">In Startup.Auth.cs, sostituire il codice di OnValidateIdentity, come evidenziato qui sotto.</span><span class="sxs-lookup"><span data-stu-id="57117-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="57117-151">Si noti che la definizione di getUserIdCallback, analizza il valore di stringa in un numero intero.</span><span class="sxs-lookup"><span data-stu-id="57117-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="57117-152">Se il progetto non riconosce l'implementazione generica del **Recuperaidutente** (metodo), potrebbe essere necessario aggiornare il pacchetto NuGet di identità di ASP.NET alla versione 2.1</span><span class="sxs-lookup"><span data-stu-id="57117-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="57117-153">Per le classi di infrastruttura utilizzate da ASP.NET Identity apportate numerose modifiche.</span><span class="sxs-lookup"><span data-stu-id="57117-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="57117-154">Se si tenta la compilazione del progetto, si noterà numerosi errori.</span><span class="sxs-lookup"><span data-stu-id="57117-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="57117-155">Fortunatamente, gli errori rimanenti sono molto simili.</span><span class="sxs-lookup"><span data-stu-id="57117-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="57117-156">La classe di identità prevede un numero intero per la chiave, ma il controller (o Web Form) passa un valore di stringa.</span><span class="sxs-lookup"><span data-stu-id="57117-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="57117-157">In ogni caso, è necessario eseguire la conversione da una stringa e un numero intero chiamando **Recuperaidutente&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="57117-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="57117-158">È possibile esaminare l'elenco di errori di compilazione o effettuare le seguenti modifiche.</span><span class="sxs-lookup"><span data-stu-id="57117-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="57117-159">Le modifiche rimanenti dipendono dal tipo di progetto che si sta creando e quali aggiornamenti sono installati in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="57117-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="57117-160">È possibile passare direttamente alla sezione pertinente tramite i collegamenti seguenti</span><span class="sxs-lookup"><span data-stu-id="57117-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="57117-161">Per MVC con Update 2, modificare il AccountController per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="57117-162">Per MVC con Update 3, per modificare il nome AccountController e ManageController passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="57117-163">Per Web Form con Update 2, modificare le pagine per passare il tipo di chiave Account</span><span class="sxs-lookup"><span data-stu-id="57117-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="57117-164">Per Web Form con Update 3, modificare le pagine per passare il tipo di chiave Account</span><span class="sxs-lookup"><span data-stu-id="57117-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="57117-165">Per MVC con Update 2, modificare il AccountController per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="57117-166">Aprire il file AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="57117-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="57117-167">È necessario modificare i metodi seguenti.</span><span class="sxs-lookup"><span data-stu-id="57117-167">You need to change the following methods.</span></span>

<span data-ttu-id="57117-168">**ConfirmEmail** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="57117-169">**Annullare l'associazione** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="57117-170">**Manage(ManageUserViewModel)** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="57117-171">**LinkLoginCallback** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="57117-172">**RemoveAccountList** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="57117-173">**HasPassword** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="57117-174">È ora possibile [eseguire l'applicazione](#run) e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="57117-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="57117-175">Per MVC con Update 3, per modificare il nome AccountController e ManageController passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="57117-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="57117-176">Aprire il file AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="57117-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="57117-177">È necessario modificare il metodo seguente.</span><span class="sxs-lookup"><span data-stu-id="57117-177">You need to change the following method.</span></span>

<span data-ttu-id="57117-178">**ConfirmEmail** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="57117-179">**SendCode** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="57117-180">Aprire il file ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="57117-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="57117-181">È necessario modificare i metodi seguenti.</span><span class="sxs-lookup"><span data-stu-id="57117-181">You need to change the following methods.</span></span>

<span data-ttu-id="57117-182">**Indice** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="57117-183">**RemoveLogin** metodi</span><span class="sxs-lookup"><span data-stu-id="57117-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="57117-184">**AddPhoneNumber** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="57117-185">**EnableTwoFactorAuthentication** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="57117-186">**DisableTwoFactorAuthentication** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="57117-187">**VerifyPhoneNumber** metodi</span><span class="sxs-lookup"><span data-stu-id="57117-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="57117-188">**RemovePhoneNumber** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="57117-189">**ChangePassword** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="57117-190">**SetPassword** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="57117-191">**ManageLogins** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="57117-192">**LinkLoginCallback** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="57117-193">**HasPassword** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="57117-194">**HasPhoneNumber** (metodo)</span><span class="sxs-lookup"><span data-stu-id="57117-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="57117-195">È ora possibile [eseguire l'applicazione](#run) e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="57117-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="57117-196">Per Web Form con Update 2, modificare le pagine per passare il tipo di chiave Account</span><span class="sxs-lookup"><span data-stu-id="57117-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="57117-197">Per Web Form con Update 2, è necessario modificare le pagine seguenti.</span><span class="sxs-lookup"><span data-stu-id="57117-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="57117-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="57117-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="57117-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="57117-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="57117-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="57117-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="57117-201">È ora possibile [eseguire l'applicazione](#run) e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="57117-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="57117-202">Per Web Form con Update 3, modificare le pagine per passare il tipo di chiave Account</span><span class="sxs-lookup"><span data-stu-id="57117-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="57117-203">Per Web Form con Update 3, è necessario modificare le pagine seguenti.</span><span class="sxs-lookup"><span data-stu-id="57117-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="57117-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="57117-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="57117-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="57117-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="57117-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="57117-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="57117-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="57117-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="57117-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="57117-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="57117-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="57117-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="57117-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="57117-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="57117-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="57117-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="57117-212">Eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="57117-212">Run application</span></span>

<span data-ttu-id="57117-213">Sono state tutte le modifiche necessarie per il modello di applicazione Web predefinita.</span><span class="sxs-lookup"><span data-stu-id="57117-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="57117-214">Eseguire l'applicazione e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="57117-214">Run the application and register a new user.</span></span> <span data-ttu-id="57117-215">Dopo la registrazione dell'utente, si noterà che la tabella AspNetUsers include una colonna di Id che è un numero intero.</span><span class="sxs-lookup"><span data-stu-id="57117-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nuova chiave primaria](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="57117-217">Se è stata creata l'identità ASP.NET tabelle con una chiave primaria diversa, è necessario apportare alcune modifiche aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="57117-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="57117-218">Se possibile, è sufficiente eliminare il database esistente.</span><span class="sxs-lookup"><span data-stu-id="57117-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="57117-219">Il database sarà creato di nuovo con la progettazione corretta quando si esegue l'applicazione web e aggiungere un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="57117-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="57117-220">Se l'eliminazione non è possibile, eseguire le migrazioni code first per modificare le tabelle.</span><span class="sxs-lookup"><span data-stu-id="57117-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="57117-221">Tuttavia, la nuova chiave primaria di integer verrà non essere configurata come una proprietà IDENTITY di SQL nel database.</span><span class="sxs-lookup"><span data-stu-id="57117-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="57117-222">È necessario impostare manualmente la colonna Id come un'identità.</span><span class="sxs-lookup"><span data-stu-id="57117-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="57117-223">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="57117-223">Other resources</span></span>

- [<span data-ttu-id="57117-224">Panoramica dei provider di archiviazione personalizzato per l'identità ASP.NET</span><span class="sxs-lookup"><span data-stu-id="57117-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="57117-225">La migrazione di un sito Web esistente dall'appartenenza SQL per ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="57117-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="57117-226">Migrazione dei dati per l'appartenenza e i profili di ASP.NET Identity Provider Universal</span><span class="sxs-lookup"><span data-stu-id="57117-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="57117-227">[Applicazione di esempio](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) con la chiave primaria modificata</span><span class="sxs-lookup"><span data-stu-id="57117-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
