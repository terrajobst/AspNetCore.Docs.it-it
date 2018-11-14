---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: Uso delle annotazioni dei dati per la convalida del modello | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 6 viene illustrato l'utilizzo di annotazioni dei dati per il modello V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b666c3cef0b09c6d68cee581a3c27c08e3357cca
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833366"
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="1638f-104">Parte 6: Uso delle annotazioni dei dati per la convalida del modello</span><span class="sxs-lookup"><span data-stu-id="1638f-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="1638f-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="1638f-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="1638f-106">Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="1638f-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="1638f-107">Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="1638f-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="1638f-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="1638f-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="1638f-109">Nella Parte 6 viene illustrato l'utilizzo di annotazioni dei dati per la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="1638f-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="1638f-110">È presente un problema principale con la creazione e modifica di moduli: non dovranno più svolgere alcuna operazione di convalida.</span><span class="sxs-lookup"><span data-stu-id="1638f-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="1638f-111">È possibile, ad esempio, lasciare vuoti i campi obbligatori o lettere tipo nel campo di prezzo ed è il primo errore che vedremo dal database.</span><span class="sxs-lookup"><span data-stu-id="1638f-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="1638f-112">Convalida possiamo aggiungere con facilità all'applicazione tramite l'aggiunta di annotazioni dei dati per le classi di modello.</span><span class="sxs-lookup"><span data-stu-id="1638f-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="1638f-113">Le annotazioni dei dati consentono di descrivere le regole che vogliamo applicate per la proprietà del modello e ASP.NET MVC si occuperà di applicandoli e visualizzazione dei messaggi appropriati per i nostri utenti.</span><span class="sxs-lookup"><span data-stu-id="1638f-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="1638f-114">Aggiunta della convalida per il form Album</span><span class="sxs-lookup"><span data-stu-id="1638f-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="1638f-115">Si useranno gli attributi di annotazione dei dati seguenti:</span><span class="sxs-lookup"><span data-stu-id="1638f-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="1638f-116">**Obbligatorio** – indica che la proprietà è un campo obbligatorio</span><span class="sxs-lookup"><span data-stu-id="1638f-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="1638f-117">**DisplayName** : definisce il testo che si desidera utilizzare i campi del form e i messaggi di convalida</span><span class="sxs-lookup"><span data-stu-id="1638f-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="1638f-118">**StringLength** -definisce una lunghezza massima per un campo stringa</span><span class="sxs-lookup"><span data-stu-id="1638f-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="1638f-119">**Intervallo** : fornisce un valore minimo e massimo per un campo numerico</span><span class="sxs-lookup"><span data-stu-id="1638f-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="1638f-120">**Associare** : Elenca i campi da escludere o includere quando si associano i valori di parametro o modulo alle proprietà del modello</span><span class="sxs-lookup"><span data-stu-id="1638f-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="1638f-121">**ScaffoldColumn** : consente di nascondere i campi da form editor</span><span class="sxs-lookup"><span data-stu-id="1638f-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="1638f-122">*Nota: Per ulteriori informazioni sulla convalida del modello utilizzando gli attributi di annotazione dei dati, vedere la documentazione MSDN all'indirizzo*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="1638f-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="1638f-123">Aprire la classe di Album e aggiungere quanto segue *usando* istruzioni all'inizio.</span><span class="sxs-lookup"><span data-stu-id="1638f-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="1638f-124">Successivamente, aggiornare le proprietà per aggiungere gli attributi di convalida e visualizzazione, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="1638f-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="1638f-125">È possibile, è stato modificato il genere e artista alle proprietà virtuale.</span><span class="sxs-lookup"><span data-stu-id="1638f-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="1638f-126">Ciò consente a Entity Framework caricamento lazy in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="1638f-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="1638f-127">Dopo avere aggiunto questi attributi per il nostro modello di Album, nostra schermata di creazione e modifica iniziare immediatamente la convalida dei campi e usando i nomi visualizzati abbiamo scelto (ad esempio, Album Art Url anziché AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="1638f-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="1638f-128">Eseguire l'applicazione e passare a /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="1638f-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="1638f-129">Successivamente, di interruzione alcune regole di convalida.</span><span class="sxs-lookup"><span data-stu-id="1638f-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="1638f-130">Immettere un prezzo pari a 0 e lasciare vuoto il titolo.</span><span class="sxs-lookup"><span data-stu-id="1638f-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="1638f-131">Quando si fa clic sul pulsante Crea, si vedrà il modulo visualizzato convalida dei messaggi di errore che mostra i campi che non soddisfa le regole di convalida che è stato definito.</span><span class="sxs-lookup"><span data-stu-id="1638f-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="1638f-132">Test di convalida sul lato Client</span><span class="sxs-lookup"><span data-stu-id="1638f-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="1638f-133">La convalida sul lato server è molto importante dal punto di vista dell'applicazione, perché gli utenti possono aggirare la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="1638f-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="1638f-134">Pagina Web Form che implementano solo la convalida sul lato server, tuttavia, presentano tre problemi significativi.</span><span class="sxs-lookup"><span data-stu-id="1638f-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="1638f-135">L'utente deve attendere per il form da registrare, convalidati nel server e per la risposta da inviare al browser.</span><span class="sxs-lookup"><span data-stu-id="1638f-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="1638f-136">L'utente non ottiene un feedback immediato quando si corregge un campo in modo che passa a questo punto le regole di convalida.</span><span class="sxs-lookup"><span data-stu-id="1638f-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="1638f-137">Si sta inutile consumo di risorse del server per eseguire la logica di convalida anziché sfruttando il browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="1638f-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="1638f-138">Per fortuna, i modelli di scaffolding di ASP.NET MVC 3 hanno la convalida lato client incorporata, che richiedono alcun intervento aggiuntivo danno.</span><span class="sxs-lookup"><span data-stu-id="1638f-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="1638f-139">Digitare una sola lettera nel campo del titolo soddisfa i requisiti di convalida, in modo che il messaggio di convalida viene rimosso immediatamente.</span><span class="sxs-lookup"><span data-stu-id="1638f-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="1638f-140">[Precedente](mvc-music-store-part-5.md)
> [Successivo](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="1638f-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
