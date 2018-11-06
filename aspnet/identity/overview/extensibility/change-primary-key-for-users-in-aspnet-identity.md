---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Modificare la chiave primaria per gli utenti in ASP.NET Identity | Microsoft Docs
author: Rick-Anderson
description: In Visual Studio 2013, l'applicazione web predefinita Usa un valore stringa per la chiave per gli account utente. ASP.NET Identity consente di modificare il tipo di...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: d2856ce1ca61a29e091bfbd16647b673e6fc659b
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021105"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="518b0-104">Modificare la chiave primaria per gli utenti in ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="518b0-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="518b0-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="518b0-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="518b0-106">In Visual Studio 2013, l'applicazione web predefinita Usa un valore stringa per la chiave per gli account utente.</span><span class="sxs-lookup"><span data-stu-id="518b0-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="518b0-107">ASP.NET Identity consente di modificare il tipo della chiave per soddisfare i requisiti dei dati.</span><span class="sxs-lookup"><span data-stu-id="518b0-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="518b0-108">Ad esempio, è possibile modificare il tipo della chiave da una stringa in un numero intero.</span><span class="sxs-lookup"><span data-stu-id="518b0-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="518b0-109">Questo argomento illustra come iniziare con il valore predefinito di applicazioni web e modificare la chiave dell'account utente in un numero intero.</span><span class="sxs-lookup"><span data-stu-id="518b0-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="518b0-110">È possibile utilizzare le stesse modifiche per implementare qualsiasi tipo di chiave nel progetto.</span><span class="sxs-lookup"><span data-stu-id="518b0-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="518b0-111">Viene illustrato come apportare queste modifiche nell'applicazione web predefinita, ma è possibile applicare modifiche simili a un'applicazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="518b0-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="518b0-112">Mostra le modifiche necessarie quando si lavora con MVC o Web Form.</span><span class="sxs-lookup"><span data-stu-id="518b0-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="518b0-113">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="518b0-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="518b0-114">Visual Studio 2013 con Update 2 (o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="518b0-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="518b0-115">ASP.NET Identity 2.1 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="518b0-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="518b0-116">Per eseguire i passaggi descritti in questa esercitazione, è necessario disporre di Visual Studio 2013 Update 2 (o versioni successive) e un'applicazione web creata dal modello di applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="518b0-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="518b0-117">Il modello modificato in Update 3.</span><span class="sxs-lookup"><span data-stu-id="518b0-117">The template changed in Update 3.</span></span> <span data-ttu-id="518b0-118">In questo argomento viene illustrato come modificare il modello in Update 2 e Update 3.</span><span class="sxs-lookup"><span data-stu-id="518b0-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="518b0-119">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="518b0-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="518b0-120">Modificare il tipo della chiave nella classe di identità utente</span><span class="sxs-lookup"><span data-stu-id="518b0-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="518b0-121">Aggiungere le classi di identità personalizzate che usano il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="518b0-122">Modificare il gestore di contesto utente e per usare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="518b0-123">Modificare la configurazione di avvio per usare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="518b0-124">Per MVC con Update 2, modificare il AccountController per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="518b0-125">Per MVC con Update 3, per modificare il AccountController e ManageController passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="518b0-126">Per i moduli Web con Update 2, modificare le pagine di Account per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="518b0-127">Per i moduli Web con Update 3, modificare le pagine di Account per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="518b0-128">Esegui l'applicazione</span><span class="sxs-lookup"><span data-stu-id="518b0-128">Run application</span></span>](#run)
- [<span data-ttu-id="518b0-129">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="518b0-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="518b0-130">Modificare il tipo della chiave nella classe di identità utente</span><span class="sxs-lookup"><span data-stu-id="518b0-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="518b0-131">Nel progetto creato dal modello di applicazione Web ASP.NET, specificare che la classe ApplicationUser utilizza un numero intero per la chiave per gli account utente.</span><span class="sxs-lookup"><span data-stu-id="518b0-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="518b0-132">In IdentityModels.cs, modificare la classe ApplicationUser da cui ereditare IdentityUser che è di tipo **int** per il parametro generico TKey.</span><span class="sxs-lookup"><span data-stu-id="518b0-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="518b0-133">È anche possibile passare i nomi delle tre classi personalizzate che non è stato ancora implementato.</span><span class="sxs-lookup"><span data-stu-id="518b0-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="518b0-134">È stato modificato il tipo della chiave, ma, per impostazione predefinita, il resto dell'applicazione ancora si presuppone che la chiave è una stringa.</span><span class="sxs-lookup"><span data-stu-id="518b0-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="518b0-135">È necessario indicare in modo esplicito il tipo della chiave nel codice che presuppone che una stringa.</span><span class="sxs-lookup"><span data-stu-id="518b0-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="518b0-136">Nel **ApplicationUser** classe, modificare il **GenerateUserIdentityAsync** metodo includere int, come illustrato nel codice evidenziato seguente.</span><span class="sxs-lookup"><span data-stu-id="518b0-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="518b0-137">Questa modifica non è necessaria per i progetti Web Form con il modello di Update 3.</span><span class="sxs-lookup"><span data-stu-id="518b0-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="518b0-138">Aggiungere le classi di identità personalizzate che usano il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="518b0-139">Le altre classi di identità, ad esempio IdentityUserRole IdentityUserClaim, IdentityUserLogin, IdentityRole, nello UserStore, oggetto RoleStore, vengono comunque configurate per usare una chiave di stringa.</span><span class="sxs-lookup"><span data-stu-id="518b0-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="518b0-140">Creazione di nuove versioni di queste classi che specificano un numero intero per la chiave.</span><span class="sxs-lookup"><span data-stu-id="518b0-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="518b0-141">Non è necessario fornire altro codice di implementazione di queste classi, si sono principalmente solo l'impostazione int come chiave.</span><span class="sxs-lookup"><span data-stu-id="518b0-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="518b0-142">Aggiungere le classi seguenti al file IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="518b0-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="518b0-143">Modificare il gestore di contesto utente e per usare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="518b0-144">In IdentityModels.cs, modificare la definizione del **ApplicationDbContext** usare la nuova classe personalizzata classi e una **int** per la chiave, come illustrato nel codice evidenziato.</span><span class="sxs-lookup"><span data-stu-id="518b0-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="518b0-145">Il parametro ThrowIfV1Schema non è più valido nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="518b0-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="518b0-146">Modificare il costruttore in modo che non ha superato un valore ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="518b0-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="518b0-147">Aprire IdentityConfig.cs e modificare il **ApplicationUserManger** classe da utilizzare il nuovo utente archiviare classe per rendere persistenti i dati e un **int** per la chiave.</span><span class="sxs-lookup"><span data-stu-id="518b0-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="518b0-148">Nel modello di Update 3, è necessario modificare la classe ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="518b0-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="518b0-149">Modificare la configurazione di avvio per usare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="518b0-150">In Startup.Auth.cs, sostituire il codice OnValidateIdentity, come evidenziato di seguito.</span><span class="sxs-lookup"><span data-stu-id="518b0-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="518b0-151">Si noti che la definizione di getUserIdCallback, analizza il valore di stringa in un numero intero.</span><span class="sxs-lookup"><span data-stu-id="518b0-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="518b0-152">Se il progetto non riconosce l'implementazione generica del **GetUserId** (metodo), potrebbe essere necessario aggiornare il pacchetto NuGet di identità di ASP.NET alla versione 2.1</span><span class="sxs-lookup"><span data-stu-id="518b0-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="518b0-153">Si è apportate numerose modifiche alle classi di infrastruttura usate da ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="518b0-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="518b0-154">Se si tenta la compilazione del progetto, si noterà una grande quantità di errori.</span><span class="sxs-lookup"><span data-stu-id="518b0-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="518b0-155">Fortunatamente, gli errori rimanenti sono molto simili.</span><span class="sxs-lookup"><span data-stu-id="518b0-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="518b0-156">La classe di identità prevede un numero intero per la chiave, ma il controller (o Web Form) passa un valore di stringa.</span><span class="sxs-lookup"><span data-stu-id="518b0-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="518b0-157">In ogni caso, è necessario convertire da una stringa e integer chiamando **GetUserId&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="518b0-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="518b0-158">È possibile esaminare l'elenco di errori di compilazione o seguire le seguenti modifiche.</span><span class="sxs-lookup"><span data-stu-id="518b0-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="518b0-159">Le modifiche rimanenti dipendono dal tipo di progetto che si sta creando e quali aggiornamenti sono installati in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="518b0-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="518b0-160">È possibile passare direttamente alla sezione rilevante tramite i collegamenti seguenti</span><span class="sxs-lookup"><span data-stu-id="518b0-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="518b0-161">Per MVC con Update 2, modificare il AccountController per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="518b0-162">Per MVC con Update 3, per modificare il AccountController e ManageController passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="518b0-163">Per i moduli Web con Update 2, modificare le pagine di Account per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="518b0-164">Per i moduli Web con Update 3, modificare le pagine di Account per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="518b0-165">Per MVC con Update 2, modificare il AccountController per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="518b0-166">Aprire il file AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="518b0-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="518b0-167">È necessario modificare i metodi seguenti.</span><span class="sxs-lookup"><span data-stu-id="518b0-167">You need to change the following methods.</span></span>

<span data-ttu-id="518b0-168">**ConfirmEmail** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="518b0-169">**Annullare l'associazione** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="518b0-170">**Manage(ManageUserViewModel)** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="518b0-171">**LinkLoginCallback** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="518b0-172">**RemoveAccountList** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="518b0-173">**HasPassword** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="518b0-174">Ora puoi [eseguire l'applicazione](#run) e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="518b0-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="518b0-175">Per MVC con Update 3, per modificare il AccountController e ManageController passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="518b0-176">Aprire il file AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="518b0-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="518b0-177">È necessario modificare il metodo seguente.</span><span class="sxs-lookup"><span data-stu-id="518b0-177">You need to change the following method.</span></span>

<span data-ttu-id="518b0-178">**ConfirmEmail** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="518b0-179">**SendCode** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="518b0-180">Aprire il file ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="518b0-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="518b0-181">È necessario modificare i metodi seguenti.</span><span class="sxs-lookup"><span data-stu-id="518b0-181">You need to change the following methods.</span></span>

<span data-ttu-id="518b0-182">**Indice** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="518b0-183">**RemoveLogin** metodi</span><span class="sxs-lookup"><span data-stu-id="518b0-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="518b0-184">**AddPhoneNumber** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="518b0-185">**EnableTwoFactorAuthentication** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="518b0-186">**DisableTwoFactorAuthentication** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="518b0-187">**VerifyPhoneNumber** metodi</span><span class="sxs-lookup"><span data-stu-id="518b0-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="518b0-188">**RemovePhoneNumber** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="518b0-189">**ChangePassword** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="518b0-190">**SetPassword** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="518b0-191">**ManageLogins** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="518b0-192">**LinkLoginCallback** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="518b0-193">**HasPassword** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="518b0-194">**HasPhoneNumber** (metodo)</span><span class="sxs-lookup"><span data-stu-id="518b0-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="518b0-195">Ora puoi [eseguire l'applicazione](#run) e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="518b0-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="518b0-196">Per i moduli Web con Update 2, modificare le pagine di Account per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="518b0-197">Per i moduli Web con Update 2, è necessario modificare le pagine seguenti.</span><span class="sxs-lookup"><span data-stu-id="518b0-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="518b0-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="518b0-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="518b0-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="518b0-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="518b0-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="518b0-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="518b0-201">Ora puoi [eseguire l'applicazione](#run) e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="518b0-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="518b0-202">Per i moduli Web con Update 3, modificare le pagine di Account per passare il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="518b0-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="518b0-203">Per i moduli Web con Update 3, è necessario modificare le pagine seguenti.</span><span class="sxs-lookup"><span data-stu-id="518b0-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="518b0-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="518b0-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="518b0-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="518b0-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="518b0-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="518b0-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="518b0-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="518b0-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="518b0-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="518b0-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="518b0-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="518b0-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="518b0-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="518b0-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="518b0-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="518b0-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="518b0-212">Esegui l'applicazione</span><span class="sxs-lookup"><span data-stu-id="518b0-212">Run application</span></span>

<span data-ttu-id="518b0-213">Aver apportato tutte le modifiche necessarie per il modello di applicazione Web predefinita.</span><span class="sxs-lookup"><span data-stu-id="518b0-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="518b0-214">Eseguire l'applicazione e registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="518b0-214">Run the application and register a new user.</span></span> <span data-ttu-id="518b0-215">Dopo la registrazione utente, si noterà che nella tabella AspNetUsers dispone di una colonna Id è un numero intero.</span><span class="sxs-lookup"><span data-stu-id="518b0-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nuova chiave primaria](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="518b0-217">Se si have creato in precedenza l'identità di ASP.NET le tabelle con una chiave primaria diversa, è necessario apportare alcune modifiche aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="518b0-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="518b0-218">Se possibile, sarà sufficiente eliminare il database esistente.</span><span class="sxs-lookup"><span data-stu-id="518b0-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="518b0-219">Il database verrà ricreato con la progettazione corretta quando si esegue l'applicazione web e aggiungere un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="518b0-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="518b0-220">Se l'eliminazione non è possibile, eseguire le migrazioni code first per modificare le tabelle.</span><span class="sxs-lookup"><span data-stu-id="518b0-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="518b0-221">Tuttavia, la nuova chiave primaria integer verrà non essere configurata come proprietà IDENTITY di SQL nel database.</span><span class="sxs-lookup"><span data-stu-id="518b0-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="518b0-222">È necessario impostare manualmente la colonna Id come un'identità.</span><span class="sxs-lookup"><span data-stu-id="518b0-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="518b0-223">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="518b0-223">Other resources</span></span>

- [<span data-ttu-id="518b0-224">Panoramica dei provider di archiviazione personalizzati per ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="518b0-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="518b0-225">Migrazione di un sito Web esistente dall'appartenenza SQL ad ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="518b0-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="518b0-226">La migrazione dei dati del Provider universali per appartenenza e i profili utente mobili ad ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="518b0-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="518b0-227">[Applicazione di esempio](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) con la chiave primaria modificata</span><span class="sxs-lookup"><span data-stu-id="518b0-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
