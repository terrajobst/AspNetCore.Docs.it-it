---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: Le annotazioni dei dati di utilizzo per la convalida del modello | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 6 viene illustrato l'utilizzo di annotazioni di dati per il modello V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b2083d5604741a0142f504f92779c9f931d0d6d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="b7a70-104">Parte 6: Utilizzando le annotazioni dei dati per la convalida del modello</span><span class="sxs-lookup"><span data-stu-id="b7a70-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="b7a70-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="b7a70-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="b7a70-106">L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="b7a70-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="b7a70-107">L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="b7a70-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="b7a70-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio.</span><span class="sxs-lookup"><span data-stu-id="b7a70-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="b7a70-109">Parte 6 viene illustrato l'utilizzo di annotazioni di dati per la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="b7a70-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="b7a70-110">Abbiamo un problema con la creazione e modifica di form: non dovranno più svolgere alcuna operazione di convalida.</span><span class="sxs-lookup"><span data-stu-id="b7a70-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="b7a70-111">È possibile eseguire operazioni quali lasciare vuoti i campi obbligatori o lettere tipo nel campo Prezzo ed è il primo errore che vedremo dal database.</span><span class="sxs-lookup"><span data-stu-id="b7a70-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="b7a70-112">È possibile aggiungere facilmente la convalida per l'applicazione aggiungendo le annotazioni dei dati per il nostro classi del modello.</span><span class="sxs-lookup"><span data-stu-id="b7a70-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="b7a70-113">Le annotazioni dei dati consentono di descrivere le regole che vogliamo applicate per la proprietà del modello e MVC ASP.NET si occuperà di applicandoli e visualizzare i messaggi appropriati per gli utenti.</span><span class="sxs-lookup"><span data-stu-id="b7a70-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="b7a70-114">Aggiunta della convalida per il form Album</span><span class="sxs-lookup"><span data-stu-id="b7a70-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="b7a70-115">Si userà gli attributi di annotazione dei dati seguenti:</span><span class="sxs-lookup"><span data-stu-id="b7a70-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="b7a70-116">**Obbligatorio** : indica che la proprietà è un campo obbligatorio</span><span class="sxs-lookup"><span data-stu-id="b7a70-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="b7a70-117">**DisplayName** -definisce il testo che si desidera utilizzare i campi del form e i messaggi di convalida</span><span class="sxs-lookup"><span data-stu-id="b7a70-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="b7a70-118">**StringLength** – definisce una lunghezza massima per un campo stringa</span><span class="sxs-lookup"><span data-stu-id="b7a70-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="b7a70-119">**Intervallo** : fornisce un valore minimo e massimo per un campo numerico</span><span class="sxs-lookup"><span data-stu-id="b7a70-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="b7a70-120">**Associare** : Elenca i campi da escludere o includere durante l'associazione dei valori di parametro o modulo alle proprietà del modello</span><span class="sxs-lookup"><span data-stu-id="b7a70-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="b7a70-121">**ScaffoldColumn** : consente di nascondere i campi da moduli di editor</span><span class="sxs-lookup"><span data-stu-id="b7a70-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="b7a70-122">*Nota: Per ulteriori informazioni sulla convalida del modello utilizzando gli attributi di annotazione dei dati, vedere la documentazione MSDN all'indirizzo*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="b7a70-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="b7a70-123">Aprire la classe Album e aggiungere le seguenti *utilizzando* istruzioni all'inizio.</span><span class="sxs-lookup"><span data-stu-id="b7a70-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="b7a70-124">Aggiornare quindi le proprietà per aggiungere gli attributi di visualizzazione e la convalida, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b7a70-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="b7a70-125">Mentre si è presente, è stato modificato il Genre e artista alle proprietà virtuali.</span><span class="sxs-lookup"><span data-stu-id="b7a70-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="b7a70-126">In questo modo di Entity Framework caricamento lazy in base alle necessità.</span><span class="sxs-lookup"><span data-stu-id="b7a70-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="b7a70-127">Dopo avere aggiunto questi attributi il nostro modello Album, la schermata di creare e modificare immediatamente di convalida dei campi e i nomi visualizzati utilizzando abbiamo scelto (ad esempio, Album Art Url anziché AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="b7a70-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="b7a70-128">Eseguire l'applicazione e passare a /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="b7a70-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="b7a70-129">Successivamente, di interruzione alcune regole di convalida.</span><span class="sxs-lookup"><span data-stu-id="b7a70-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="b7a70-130">Immettere un prezzo pari a 0 e lasciare il campo vuoto.</span><span class="sxs-lookup"><span data-stu-id="b7a70-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="b7a70-131">Quando si fa clic sul pulsante Crea, è possibile vedere il form visualizzato con messaggi di errore che mostra i campi che non soddisfano le regole di convalida che sono state definite.</span><span class="sxs-lookup"><span data-stu-id="b7a70-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="b7a70-132">Test di convalida lato Client</span><span class="sxs-lookup"><span data-stu-id="b7a70-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="b7a70-133">La convalida sul lato server è molto importante dalla prospettiva dell'applicazione, perché gli utenti possono aggirare la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="b7a70-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="b7a70-134">Pagina Web Form che implementano solo la convalida sul lato server presentano tuttavia tre problemi significativi.</span><span class="sxs-lookup"><span data-stu-id="b7a70-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="b7a70-135">L'utente deve rimanere in attesa per il form da registrare, convalidato sul server e per la risposta da inviare al browser.</span><span class="sxs-lookup"><span data-stu-id="b7a70-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="b7a70-136">L'utente non riceve un feedback immediato quando si corregge un campo in modo che passa a questo punto le regole di convalida.</span><span class="sxs-lookup"><span data-stu-id="b7a70-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="b7a70-137">Ci stiamo un inutile consumo di risorse del server per eseguire la logica di convalida anziché sfruttando il browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="b7a70-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="b7a70-138">Fortunatamente, i modelli dello scaffolding di ASP.NET MVC 3 hanno la convalida lato client incorporata, che richiedono alcun intervento aggiuntivo qualunque.</span><span class="sxs-lookup"><span data-stu-id="b7a70-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="b7a70-139">Digitare una sola lettera nel campo titolo soddisfa i requisiti di convalida, quindi il messaggio di convalida viene rimosso immediatamente.</span><span class="sxs-lookup"><span data-stu-id="b7a70-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


>[!div class="step-by-step"]
<span data-ttu-id="b7a70-140">[Precedente](mvc-music-store-part-5.md)
[Successivo](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="b7a70-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
