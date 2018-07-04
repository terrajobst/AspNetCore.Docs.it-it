---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: Appartenenza e autorizzazione | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 7 riguarda l'appartenenza e l'autorizzazione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 41b17315cbe1f6d93001a736bc24bf003df24061
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393170"
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="cceac-104">Parte 7: Appartenenza e autorizzazione</span><span class="sxs-lookup"><span data-stu-id="cceac-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="cceac-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="cceac-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="cceac-106">Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="cceac-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="cceac-107">La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="cceac-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="cceac-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="cceac-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="cceac-109">Parte 7 riguarda l'appartenenza e l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="cceac-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="cceac-110">Il controller di gestione Store è attualmente accessibile a tutti gli utenti che visitano il sito.</span><span class="sxs-lookup"><span data-stu-id="cceac-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="cceac-111">È possibile modificare questa impostazione per limitare le autorizzazioni per gli amministratori del sito.</span><span class="sxs-lookup"><span data-stu-id="cceac-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="cceac-112">Aggiunta di AccountController e viste</span><span class="sxs-lookup"><span data-stu-id="cceac-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="cceac-113">Una differenza tra il modello di applicazione Web ASP.NET MVC 3 completo e il modello di applicazione Web vuota di ASP.NET MVC 3 è che il modello vuoto non include un Controller di Account.</span><span class="sxs-lookup"><span data-stu-id="cceac-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="cceac-114">Si aggiungerà un Controller di Account copiando alcuni file da una nuova applicazione ASP.NET MVC creata dal modello di applicazione Web ASP.NET MVC 3 completo.</span><span class="sxs-lookup"><span data-stu-id="cceac-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="cceac-115">Creare una nuova applicazione MVC ASP.NET usando il modello di applicazione Web MVC 3 ASP.NET completo e copiare i file seguenti nelle stesse directory nel progetto:</span><span class="sxs-lookup"><span data-stu-id="cceac-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="cceac-116">AccountController.cs copia nella directory del controller</span><span class="sxs-lookup"><span data-stu-id="cceac-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="cceac-117">AccountModels copia nella directory di modelli</span><span class="sxs-lookup"><span data-stu-id="cceac-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="cceac-118">Creare una directory di Account all'interno della directory di viste e copiare tutte le quattro viste in</span><span class="sxs-lookup"><span data-stu-id="cceac-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="cceac-119">Modificare lo spazio dei nomi per le classi Controller e il modello in modo che inizino con MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="cceac-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="cceac-120">La classe AccountController deve utilizzare lo spazio dei nomi MvcMusicStore.Controllers e la classe AccountModels deve utilizzare lo spazio dei nomi MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="cceac-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="cceac-121">*Nota: Questi file sono anche disponibili nel download MvcMusicStore Assets.zip da cui viene copiato i file di progettazione del sito all'inizio dell'esercitazione. I file di appartenenza si trovano nella directory del codice.*</span><span class="sxs-lookup"><span data-stu-id="cceac-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="cceac-122">La soluzione aggiornata dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="cceac-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="cceac-123">Aggiunta di un utente amministratore con il sito di configurazione ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cceac-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="cceac-124">Prima si richiede autorizzazione nel nostro sito Web, è necessario creare un utente con accesso.</span><span class="sxs-lookup"><span data-stu-id="cceac-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="cceac-125">Il modo più semplice per creare un utente deve usare il sito Web di configurazione di ASP.NET predefinito.</span><span class="sxs-lookup"><span data-stu-id="cceac-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="cceac-126">Avviare il sito Web ASP.NET configurazione facendo clic sul pulsante seguente l'icona visualizzata in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="cceac-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="cceac-127">Verrà avviato un sito Web di configurazione.</span><span class="sxs-lookup"><span data-stu-id="cceac-127">This launches a configuration website.</span></span> <span data-ttu-id="cceac-128">Fare clic sulla scheda sicurezza nella schermata iniziale, quindi fare clic sul collegamento "Abilitare i ruoli" al centro della schermata.</span><span class="sxs-lookup"><span data-stu-id="cceac-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="cceac-129">Fare clic sul collegamento "Crea o Gestisci ruoli".</span><span class="sxs-lookup"><span data-stu-id="cceac-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="cceac-130">Immettere "Administrator" come nome del ruolo e fare clic su Add Role.</span><span class="sxs-lookup"><span data-stu-id="cceac-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="cceac-131">Fare clic sul pulsante Indietro, quindi fare clic sul collegamento Crea utente sul lato sinistro.</span><span class="sxs-lookup"><span data-stu-id="cceac-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="cceac-132">Compilare i campi di informazioni utente a sinistra con le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="cceac-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="cceac-133">**Campo**</span><span class="sxs-lookup"><span data-stu-id="cceac-133">**Field**</span></span> | <span data-ttu-id="cceac-134">**Valore**</span><span class="sxs-lookup"><span data-stu-id="cceac-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="cceac-135">**Nome utente**</span><span class="sxs-lookup"><span data-stu-id="cceac-135">**User Name**</span></span> | <span data-ttu-id="cceac-136">Amministratore</span><span class="sxs-lookup"><span data-stu-id="cceac-136">Administrator</span></span> |
| <span data-ttu-id="cceac-137">**Password**</span><span class="sxs-lookup"><span data-stu-id="cceac-137">**Password**</span></span> | <span data-ttu-id="cceac-138">password123!</span><span class="sxs-lookup"><span data-stu-id="cceac-138">password123!</span></span> |
| <span data-ttu-id="cceac-139">**Conferma password**</span><span class="sxs-lookup"><span data-stu-id="cceac-139">**Confirm Password**</span></span> | <span data-ttu-id="cceac-140">password123!</span><span class="sxs-lookup"><span data-stu-id="cceac-140">password123!</span></span> |
| <span data-ttu-id="cceac-141">**Posta elettronica**</span><span class="sxs-lookup"><span data-stu-id="cceac-141">**E-mail**</span></span> | <span data-ttu-id="cceac-142">(qualsiasi indirizzo di posta elettronica funzionerà)</span><span class="sxs-lookup"><span data-stu-id="cceac-142">(any email address will work)</span></span> |
| <span data-ttu-id="cceac-143">**Domanda segreta**</span><span class="sxs-lookup"><span data-stu-id="cceac-143">**Security Question**</span></span> | <span data-ttu-id="cceac-144">(qualsiasi)</span><span class="sxs-lookup"><span data-stu-id="cceac-144">(whatever you like)</span></span> |
| <span data-ttu-id="cceac-145">**Risposta segreta**</span><span class="sxs-lookup"><span data-stu-id="cceac-145">**Security Answer**</span></span> | <span data-ttu-id="cceac-146">(qualsiasi)</span><span class="sxs-lookup"><span data-stu-id="cceac-146">(whatever you like)</span></span> |

<span data-ttu-id="cceac-147">*Nota: È ovviamente possibile usare qualsiasi password desiderato. Le impostazioni di sicurezza predefinite password richiedono una password che è di 7 caratteri e contiene un carattere non alfanumerico.*</span><span class="sxs-lookup"><span data-stu-id="cceac-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="cceac-148">Selezionare il ruolo di amministratore per questo utente e fare clic sul pulsante Create User.</span><span class="sxs-lookup"><span data-stu-id="cceac-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="cceac-149">A questo punto, si dovrebbe vedere un messaggio che indica che l'utente è stato creato correttamente.</span><span class="sxs-lookup"><span data-stu-id="cceac-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="cceac-150">È ora possibile chiudere la finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="cceac-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="cceac-151">Autorizzazione basata sui ruoli</span><span class="sxs-lookup"><span data-stu-id="cceac-151">Role-based Authorization</span></span>

<span data-ttu-id="cceac-152">A questo punto è possibile limitare l'accesso di StoreManagerController usando l'attributo [Authorize], che specifica che l'utente deve essere il ruolo di amministratore per accedere a qualsiasi azione del controller nella classe.</span><span class="sxs-lookup"><span data-stu-id="cceac-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="cceac-153">*Nota: L'attributo [Authorize] può essere inserita nei metodi di azione specifici, nonché a livello di classe Controller.*</span><span class="sxs-lookup"><span data-stu-id="cceac-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="cceac-154">Passare ora al /StoreManager viene visualizzata una finestra di dialogo Accedi:</span><span class="sxs-lookup"><span data-stu-id="cceac-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="cceac-155">Dopo l'accesso con il nuovo account di amministratore, siamo in grado di passare alla schermata di modifica Album come prima.</span><span class="sxs-lookup"><span data-stu-id="cceac-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cceac-156">[Precedente](mvc-music-store-part-6.md)
> [Successivo](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="cceac-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
