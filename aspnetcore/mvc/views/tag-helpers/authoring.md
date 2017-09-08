---
title: Creazione e modifica gli helper di Tag in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare gli helper di Tag in ASP.NET Core.
keywords: ASP.NET Core, gli helper di tag
ms.author: riande
manager: wpickett
ms.date: 6/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f16af1184a29b891a9aab0b38ab833836c326c44
ms.sourcegitcommit: e6a8f171f26fab1b2195a2d7f14e7d258a2e690e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/23/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="81fd0-104">Gli helper di Tag in ASP.NET di base, una procedura dettagliata con esempi di creazione</span><span class="sxs-lookup"><span data-stu-id="81fd0-104">Authoring Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="81fd0-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="81fd0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="81fd0-106">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="81fd0-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a><span data-ttu-id="81fd0-107">Introduzione agli helper di Tag</span><span class="sxs-lookup"><span data-stu-id="81fd0-107">Getting started with Tag Helpers</span></span>

<span data-ttu-id="81fd0-108">In questa esercitazione fornisce un'introduzione alla programmazione gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-108">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="81fd0-109">[Introduzione agli helper di Tag](intro.md) vengono descritti i vantaggi che forniscono gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-109">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="81fd0-110">Un helper tag è qualsiasi classe che implementa il `ITagHelper` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="81fd0-110">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="81fd0-111">Tuttavia, quando si crea un helper tag, in genere deriva da `TagHelper`, pertanto consente di accedere a questa il `Process` metodo.</span><span class="sxs-lookup"><span data-stu-id="81fd0-111">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="81fd0-112">Creare un nuovo progetto ASP.NET Core denominato **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="81fd0-112">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="81fd0-113">Autenticazione non è necessario per questo progetto.</span><span class="sxs-lookup"><span data-stu-id="81fd0-113">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="81fd0-114">Creare una cartella per contenere gli helper di Tag chiamato *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="81fd0-114">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="81fd0-115">Il *TagHelpers* cartella *non* obbligatorio, ma è una convenzione ragionevole.</span><span class="sxs-lookup"><span data-stu-id="81fd0-115">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="81fd0-116">Ora è possibile iniziare subito la scrittura di un helper tag semplice.</span><span class="sxs-lookup"><span data-stu-id="81fd0-116">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="81fd0-117">Un Helper Tag minima</span><span class="sxs-lookup"><span data-stu-id="81fd0-117">A minimal Tag Helper</span></span>

<span data-ttu-id="81fd0-118">In questa sezione, scrivere un helper tag che aggiorna un tag di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="81fd0-118">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="81fd0-119">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="81fd0-119">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="81fd0-120">Il server utilizzerà l'helper di tag di posta elettronica per convertire il markup in quanto segue:</span><span class="sxs-lookup"><span data-stu-id="81fd0-120">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

<span data-ttu-id="81fd0-121">Vale a dire un tag di ancoraggio che è un collegamento di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="81fd0-121">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="81fd0-122">È possibile eseguire questa operazione se si sta scrivendo un motore di blog ed è necessario per inviare posta elettronica di marketing, supporto e altri contatti, tutti allo stesso dominio.</span><span class="sxs-lookup"><span data-stu-id="81fd0-122">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="81fd0-123">Aggiungere il seguente `EmailTagHelper` classe per il *TagHelpers* cartella.</span><span class="sxs-lookup"><span data-stu-id="81fd0-123">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="81fd0-124">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-124">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]</span></span>
    
    <span data-ttu-id="81fd0-125">**Note:**</span><span class="sxs-lookup"><span data-stu-id="81fd0-125">**Notes:**</span></span>
    
    * <span data-ttu-id="81fd0-126">Gli helper di tag utilizzano una convenzione di denominazione che ha come destinazione gli elementi del nome della classe radice (meno di *helper tag* parte del nome della classe).</span><span class="sxs-lookup"><span data-stu-id="81fd0-126">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="81fd0-127">In questo esempio, il nome radice del **posta elettronica**helper tag è *posta elettronica*, pertanto il `<email>` saranno destinati i tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-127">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="81fd0-128">Questa convenzione di denominazione dovrebbe funzionare per la maggior parte degli helper di tag, in seguito verrà illustrato come eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="81fd0-128">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="81fd0-129">La classe `EmailTagHelper` deriva da `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="81fd0-129">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="81fd0-130">La `TagHelper` classe fornisce metodi e proprietà per la scrittura gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-130">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="81fd0-131">Sottoposto a override `Process` metodo controlla l'helper di tag cosa quando eseguita.</span><span class="sxs-lookup"><span data-stu-id="81fd0-131">The  overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="81fd0-132">Il `TagHelper` classe fornisce anche una versione asincrona (`ProcessAsync`) con gli stessi parametri.</span><span class="sxs-lookup"><span data-stu-id="81fd0-132">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="81fd0-133">Il parametro di contesto per `Process` (e `ProcessAsync`) contiene informazioni associate all'esecuzione del tag HTML corrente.</span><span class="sxs-lookup"><span data-stu-id="81fd0-133">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="81fd0-134">Il parametro di output per `Process` (e `ProcessAsync`) contiene un elemento HTML con stato rappresentativo di origine originale utilizzato per generare un tag HTML e il contenuto.</span><span class="sxs-lookup"><span data-stu-id="81fd0-134">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="81fd0-135">Il nome della classe è un suffisso di **helper tag**, ovvero *non* obbligatorio, ma è considerata una convenzione prassi migliori.</span><span class="sxs-lookup"><span data-stu-id="81fd0-135">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="81fd0-136">È possibile dichiarare la classe come:</span><span class="sxs-lookup"><span data-stu-id="81fd0-136">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="81fd0-137">Per rendere il `EmailTagHelper` classe disponibile per tutti i nostri visualizzazioni Razor, aggiungere il `addTagHelper` direttiva per il *Views/_ViewImports.cshtml* file: [!code-html [principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-137">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="81fd0-138">Il codice precedente Usa la sintassi con caratteri jolly per specificare che tutti gli helper di tag in questo assembly saranno disponibili.</span><span class="sxs-lookup"><span data-stu-id="81fd0-138">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="81fd0-139">La prima stringa dopo `@addTagHelper` specifica l'helper di tag per caricare (usare "*" per tutti gli helper di tag), e la seconda stringa "AuthoringTagHelpers" Specifica l'assembly è l'helper di tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-139">The first string after `@addTagHelper` specifies the tag helper to load (Use "*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="81fd0-140">Si noti inoltre che la seconda riga riporta gli helper di tag di ASP.NET MVC di base utilizzando la sintassi con caratteri jolly (tali helper vengono discussi in [introduzione per gli helper di Tag](intro.md).) È il `@addTagHelper` direttiva che rende disponibili per la visualizzazione Razor l'helper di tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-140">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="81fd0-141">In alternativa, è possibile fornire il nome completo (FQN) di un helper di tag, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="81fd0-141">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
    <span data-ttu-id="81fd0-142">[!code-html[Principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-142">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]</span></span>
    
    <span data-ttu-id="81fd0-143">Per aggiungere un helper tag a una vista utilizzando un FQN, aggiungere innanzitutto i FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) e quindi il nome dell'assembly (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="81fd0-143">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="81fd0-144">La maggior parte degli sviluppatori preferiranno utilizzare la sintassi con caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="81fd0-144">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="81fd0-145">[Introduzione agli helper di Tag](intro.md) entra in dettaglio nella sintassi di aggiunta, rimozione, gerarchia e caratteri jolly di supporto dei tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-145">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="81fd0-146">Aggiornare il markup di *Views/Home/Contact.cshtml* file con queste modifiche:</span><span class="sxs-lookup"><span data-stu-id="81fd0-146">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    <span data-ttu-id="81fd0-147">[!code-html[Principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-147">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span></span>

4.  <span data-ttu-id="81fd0-148">Eseguire l'app e utilizzare il browser preferito per visualizzare l'origine HTML, per poter verificare che i tag di posta elettronica vengono sostituiti con il tag di ancoraggio (ad esempio, `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="81fd0-148">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="81fd0-149">*Supporto* e *Marketing* il rendering viene eseguito come collegamenti, ma che non dispongono di un `href` attributo in modo funzionale.</span><span class="sxs-lookup"><span data-stu-id="81fd0-149">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="81fd0-150">Che verrà risolto nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="81fd0-150">We'll fix that in the next section.</span></span>

<span data-ttu-id="81fd0-151">Nota: Come tag HTML e gli attributi, tag, i nomi delle classi e attributi in Razor e c# non sono rilevanti.</span><span class="sxs-lookup"><span data-stu-id="81fd0-151">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="81fd0-152">SetAttribute e SetContent</span><span class="sxs-lookup"><span data-stu-id="81fd0-152">SetAttribute and SetContent</span></span>

<span data-ttu-id="81fd0-153">In questa sezione verrà aggiornata la `EmailTagHelper` in modo che verrà creato un tag di ancoraggio valido per la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="81fd0-153">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="81fd0-154">Verrà aggiornata in modo da richiedere informazioni da una visualizzazione Razor (sotto forma di un `mail-to` attributo) e utilizzarlo per generare il punto di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="81fd0-154">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="81fd0-155">Aggiornamento di `EmailTagHelper` classe con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="81fd0-155">Update the `EmailTagHelper` class with the following:</span></span>

<span data-ttu-id="81fd0-156">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-156">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]</span></span>

<span data-ttu-id="81fd0-157">**Note:**</span><span class="sxs-lookup"><span data-stu-id="81fd0-157">**Notes:**</span></span>

* <span data-ttu-id="81fd0-158">Base alla convezione Pascal proprietà nomi di classe e per gli helper di tag vengono convertiti in loro [inferiore case kebab](http://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101#12273101).</span><span class="sxs-lookup"><span data-stu-id="81fd0-158">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](http://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101#12273101).</span></span> <span data-ttu-id="81fd0-159">Pertanto, utilizzare il `MailTo` attributo, si userà `<email mail-to="value"/>` equivalente.</span><span class="sxs-lookup"><span data-stu-id="81fd0-159">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="81fd0-160">L'ultima riga imposta il contenuto completato per l'helper di tag minima funzionale.</span><span class="sxs-lookup"><span data-stu-id="81fd0-160">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="81fd0-161">La riga evidenziata mostra la sintassi per l'aggiunta di attributi:</span><span class="sxs-lookup"><span data-stu-id="81fd0-161">The highlighted line shows the syntax for adding attributes:</span></span>

<span data-ttu-id="81fd0-162">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-162">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]</span></span>

<span data-ttu-id="81fd0-163">Questo approccio funziona per l'attributo "href" fino a quando non esiste nella raccolta di attributi.</span><span class="sxs-lookup"><span data-stu-id="81fd0-163">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="81fd0-164">È inoltre possibile utilizzare il `output.Attributes.Add` metodo per aggiungere un attributo di helper di tag di fine della raccolta di attributi di tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-164">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="81fd0-165">Aggiornare il markup di *Views/Home/Contact.cshtml* file con queste modifiche: [!code-html [principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-165">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="81fd0-166">Eseguire l'applicazione e verificare che genera collegamenti corretti.</span><span class="sxs-lookup"><span data-stu-id="81fd0-166">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="81fd0-167">Se si volesse scrivere il messaggio di posta elettronica tag chiusura automatica (`<email mail-to="Rick" />`), l'output finale potrebbe anche essere a chiusura automatica.</span><span class="sxs-lookup"><span data-stu-id="81fd0-167">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="81fd0-168">Per abilitare la possibilità di scrivere il tag con un tag di inizio (`<email mail-to="Rick">`) è necessario contrassegnare la classe con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="81fd0-168">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > <span data-ttu-id="81fd0-169">[!code-csharp[Principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-169">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]</span></span>
    
    <span data-ttu-id="81fd0-170">Con un helper di tag di posta elettronica chiusura automatica, l'output sarà `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="81fd0-170">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="81fd0-171">Chiusura automatica di tag di ancoraggio non sono validi HTML, in modo che non si desidera crearne uno, ma si potrebbe voler creare un helper di tag di chiusura automatica.</span><span class="sxs-lookup"><span data-stu-id="81fd0-171">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="81fd0-172">Gli helper di tag impostato il tipo di `TagMode` proprietà dopo la lettura di un tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-172">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="81fd0-173">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="81fd0-173">ProcessAsync</span></span>

<span data-ttu-id="81fd0-174">In questa sezione, è necessario scrivere un helper del messaggio di posta elettronica asincrona.</span><span class="sxs-lookup"><span data-stu-id="81fd0-174">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="81fd0-175">Sostituire il `EmailTagHelper` classe con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="81fd0-175">Replace the `EmailTagHelper` class with the following code:</span></span>

    <span data-ttu-id="81fd0-176">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-176">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]</span></span>

    <span data-ttu-id="81fd0-177">**Note:**</span><span class="sxs-lookup"><span data-stu-id="81fd0-177">**Notes:**</span></span>

    * <span data-ttu-id="81fd0-178">Questa versione utilizza asincrona `ProcessAsync` metodo.</span><span class="sxs-lookup"><span data-stu-id="81fd0-178">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="81fd0-179">Asincrona `GetChildContentAsync` restituisce un `Task` contenente il `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="81fd0-179">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="81fd0-180">Utilizzare il `output` parametro per ottenere il contenuto dell'elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="81fd0-180">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="81fd0-181">Apportare le seguenti modifiche per il *Views/Home/Contact.cshtml* file pertanto l'helper di tag è possibile recuperare il messaggio di posta elettronica di destinazione.</span><span class="sxs-lookup"><span data-stu-id="81fd0-181">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    <span data-ttu-id="81fd0-182">[!code-html[Principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-182">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]</span></span>

3.  <span data-ttu-id="81fd0-183">Eseguire l'applicazione e verificare che genera collegamenti di posta elettronica valido.</span><span class="sxs-lookup"><span data-stu-id="81fd0-183">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="81fd0-184">RemoveAll, PreContent.SetHtmlContent e PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="81fd0-184">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="81fd0-185">Aggiungere il seguente `BoldTagHelper` classe per il *TagHelpers* cartella.</span><span class="sxs-lookup"><span data-stu-id="81fd0-185">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="81fd0-186">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-186">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]</span></span>

    <span data-ttu-id="81fd0-187">**Note:**</span><span class="sxs-lookup"><span data-stu-id="81fd0-187">**Notes:**</span></span>
    
    * <span data-ttu-id="81fd0-188">Il `[HtmlTargetElement]` attributo passa un parametro di attributo che specifica che qualsiasi elemento HTML che contiene un attributo HTML denominato "bold" corrisponderanno, e `Process` verrà eseguito il metodo di override nella classe.</span><span class="sxs-lookup"><span data-stu-id="81fd0-188">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="81fd0-189">In questo esempio, il `Process` metodo rimuove l'attributo "bold" e racchiude contenente markup con `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="81fd0-189">In our sample, the `Process`  method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="81fd0-190">Perché non si desidera sostituire il contenuto del tag esistente, è necessario scrivere l'apertura `<strong>` tag con il `PreContent.SetHtmlContent` metodo e la chiusura `</strong>` tag con il `PostContent.SetHtmlContent` metodo.</span><span class="sxs-lookup"><span data-stu-id="81fd0-190">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="81fd0-191">Modificare il *About.cshtml* vista per contenere un `bold` valore dell'attributo.</span><span class="sxs-lookup"><span data-stu-id="81fd0-191">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="81fd0-192">Il codice completo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="81fd0-192">The completed code is shown below.</span></span>

    <span data-ttu-id="81fd0-193">[!code-html[Principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-193">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]</span></span>

3.  <span data-ttu-id="81fd0-194">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="81fd0-194">Run the app.</span></span> <span data-ttu-id="81fd0-195">È possibile utilizzare il browser preferito per controllare l'origine e verificare il markup.</span><span class="sxs-lookup"><span data-stu-id="81fd0-195">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="81fd0-196">Il `[HtmlTargetElement]` attributo destinata solo il markup HTML che fornisce un nome di attributo di "bold".</span><span class="sxs-lookup"><span data-stu-id="81fd0-196">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="81fd0-197">Il `<bold>` elemento non è stato modificato per l'helper di tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-197">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="81fd0-198">Impostare come commento il `[HtmlTargetElement]` riga dell'attributo e si utilizzerà come destinazione `<bold>` tag, vale a dire i tag HTML nel formato `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="81fd0-198">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="81fd0-199">Tenere presente che la convenzione di denominazione predefinito corrisponderà al nome di classe **grassetto**helper tag per `<bold>` tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-199">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="81fd0-200">Eseguire l'applicazione e verificare che il `<bold>` tag viene elaborata l'helper di tag.</span><span class="sxs-lookup"><span data-stu-id="81fd0-200">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="81fd0-201">La decorazione di una classe con più `[HtmlTargetElement]` attributi risulta un OR logico delle destinazioni.</span><span class="sxs-lookup"><span data-stu-id="81fd0-201">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="81fd0-202">Ad esempio, tramite il codice riportato di seguito, un tag in grassetto o un attributo bold corrisponderanno.</span><span class="sxs-lookup"><span data-stu-id="81fd0-202">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

<span data-ttu-id="81fd0-203">[!code-csharp[Principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-203">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]</span></span>

<span data-ttu-id="81fd0-204">Quando più attributi vengono aggiunti alla stessa istruzione, il runtime li gestisce come un AND logico.</span><span class="sxs-lookup"><span data-stu-id="81fd0-204">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="81fd0-205">Ad esempio, il codice riportato di seguito, un elemento HTML deve essere denominato "bold" con un attributo denominato "bold" (`<bold bold />`) per trovare la corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="81fd0-205">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="81fd0-206">È inoltre possibile utilizzare il `[HtmlTargetElement]` per modificare il nome dell'elemento di destinazione.</span><span class="sxs-lookup"><span data-stu-id="81fd0-206">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="81fd0-207">Ad esempio se si desidera che il `BoldTagHelper` alla destinazione `<MyBold>` tag, utilizzare l'attributo seguente:</span><span class="sxs-lookup"><span data-stu-id="81fd0-207">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a><span data-ttu-id="81fd0-208">Il passaggio di un modello a un Helper Tag</span><span class="sxs-lookup"><span data-stu-id="81fd0-208">Passing a model to a Tag Helper</span></span>

1.  <span data-ttu-id="81fd0-209">Aggiungere un *modelli* cartella.</span><span class="sxs-lookup"><span data-stu-id="81fd0-209">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="81fd0-210">Aggiungere il seguente `WebsiteContext` classe per il *modelli* cartella:</span><span class="sxs-lookup"><span data-stu-id="81fd0-210">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    <span data-ttu-id="81fd0-211">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-211">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]</span></span>

3.  <span data-ttu-id="81fd0-212">Aggiungere il seguente `WebsiteInformationTagHelper` classe per il *TagHelpers* cartella.</span><span class="sxs-lookup"><span data-stu-id="81fd0-212">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="81fd0-213">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-213">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]</span></span>
    
    <span data-ttu-id="81fd0-214">**Note:**</span><span class="sxs-lookup"><span data-stu-id="81fd0-214">**Notes:**</span></span>
    
    * <span data-ttu-id="81fd0-215">Come accennato in precedenza, gli helper di tag converte i nomi delle classi in base alla convezione Pascal c# e le proprietà per gli helper di tag in [inferiore case kebab](http://c2.com/cgi/wiki?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="81fd0-215">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://c2.com/cgi/wiki?KebabCase).</span></span> <span data-ttu-id="81fd0-216">Pertanto, utilizzare il `WebsiteInformationTagHelper` in Razor, verrà scritto `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="81fd0-216">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="81fd0-217">Non si desidera identificare in modo esplicito l'elemento di destinazione con il `[HtmlTargetElement]` attributo, pertanto il valore predefinito di `website-information` saranno destinati.</span><span class="sxs-lookup"><span data-stu-id="81fd0-217">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="81fd0-218">Se è stato applicato l'attributo seguente (notare non case kebab ma corrisponde al nome di classe):</span><span class="sxs-lookup"><span data-stu-id="81fd0-218">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="81fd0-219">Il tag del case kebab inferiore `<website-information />` non corrispondono.</span><span class="sxs-lookup"><span data-stu-id="81fd0-219">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="81fd0-220">Se si desidera utilizzare il `[HtmlTargetElement]` attributo, utilizzare case kebab come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="81fd0-220">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="81fd0-221">Gli elementi che sono a chiusura automatica non abbiano contenuto.</span><span class="sxs-lookup"><span data-stu-id="81fd0-221">Elements that are self-closing have no content.</span></span> <span data-ttu-id="81fd0-222">Per questo esempio, il markup Razor utilizzerà un tag di chiusura automatica, ma l'helper di tag creeranno un [sezione](http://www.w3.org/TR/html5/sections.html#the-section-element) elemento (che non è chiusura automatica e si scrive il contenuto all'interno di `section` elemento).</span><span class="sxs-lookup"><span data-stu-id="81fd0-222">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="81fd0-223">Pertanto, è necessario impostare `TagMode` a `StartTagAndEndTag` per scrivere l'output.</span><span class="sxs-lookup"><span data-stu-id="81fd0-223">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="81fd0-224">In alternativa, è possibile impostare come commento la riga che imposta `TagMode` e la scrittura di markup con un tag di chiusura.</span><span class="sxs-lookup"><span data-stu-id="81fd0-224">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="81fd0-225">(Più avanti in questa esercitazione viene fornito codice di esempio).</span><span class="sxs-lookup"><span data-stu-id="81fd0-225">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="81fd0-226">Il `$` (segno di dollaro) nella riga seguente viene utilizzato un [interpolati stringa](https://msdn.microsoft.com/library/Dn961160.aspx):</span><span class="sxs-lookup"><span data-stu-id="81fd0-226">The `$` (dollar sign) in the following line uses an [interpolated string](https://msdn.microsoft.com/library/Dn961160.aspx):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="81fd0-227">Aggiungere il markup seguente per il *About.cshtml* visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="81fd0-227">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="81fd0-228">Il markup evidenziato Visualizza le informazioni sul sito web.</span><span class="sxs-lookup"><span data-stu-id="81fd0-228">The highlighted markup displays the web site information.</span></span>
    
    <span data-ttu-id="81fd0-229">[!code-html[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-229">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]</span></span>
    
    >[!NOTE]
    > <span data-ttu-id="81fd0-230">Nel codice Razor illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="81fd0-230">In the Razor markup shown below:</span></span>
    >
    ><span data-ttu-id="81fd0-231">[!code-html[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-231">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]</span></span>
    > 
    ><span data-ttu-id="81fd0-232">Razor conosca il `info` attributo è una classe, non è una stringa, e si desidera scrivere codice c#.</span><span class="sxs-lookup"><span data-stu-id="81fd0-232">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="81fd0-233">Qualsiasi attributo helper di tag non di tipo stringa deve essere scritta senza la `@` carattere.</span><span class="sxs-lookup"><span data-stu-id="81fd0-233">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="81fd0-234">Eseguire l'app e passare alla visualizzazione di informazioni su per visualizzare le informazioni sul sito web.</span><span class="sxs-lookup"><span data-stu-id="81fd0-234">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="81fd0-235">È possibile utilizzare il markup seguente con un tag di chiusura e rimuovere la riga con `TagMode.StartTagAndEndTag` nell'helper tag:</span><span class="sxs-lookup"><span data-stu-id="81fd0-235">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    ><span data-ttu-id="81fd0-236">[!code-html[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-236">[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]</span></span>

## <a name="condition-tag-helper"></a><span data-ttu-id="81fd0-237">Helper di Tag di condizione</span><span class="sxs-lookup"><span data-stu-id="81fd0-237">Condition Tag Helper</span></span>

<span data-ttu-id="81fd0-238">L'helper di tag di condizione esegue il rendering di output quando viene passato un valore true.</span><span class="sxs-lookup"><span data-stu-id="81fd0-238">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="81fd0-239">Aggiungere il seguente `ConditionTagHelper` classe per il *TagHelpers* cartella.</span><span class="sxs-lookup"><span data-stu-id="81fd0-239">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="81fd0-240">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-240">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]</span></span>

2.  <span data-ttu-id="81fd0-241">Sostituire il contenuto del *Views/Home/Index.cshtml* file con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="81fd0-241">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    <!-- literal_block {"xml:space": "preserve", "source": "mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->
    
    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=Model />
        <div condition="Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="81fd0-242">Sostituire il `Index` metodo il `Home` controller con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="81fd0-242">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    <span data-ttu-id="81fd0-243">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-243">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]</span></span>

4.  <span data-ttu-id="81fd0-244">Eseguire l'app e passare alla home page.</span><span class="sxs-lookup"><span data-stu-id="81fd0-244">Run the app and browse to the home page.</span></span> <span data-ttu-id="81fd0-245">Il markup in condizionale `div` non vengono visualizzati.</span><span class="sxs-lookup"><span data-stu-id="81fd0-245">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="81fd0-246">Aggiungere la stringa di query `?approved=true` all'URL (ad esempio, `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="81fd0-246">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="81fd0-247">`approved`è impostato su true e condizionale markup verrà visualizzato.</span><span class="sxs-lookup"><span data-stu-id="81fd0-247">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="81fd0-248">Utilizzare il [nameof](https://msdn.microsoft.com/library/dn986596.aspx) operatore per specificare l'attributo di destinazione, anziché specificare una stringa, come avviene per l'helper di tag in grassetto:</span><span class="sxs-lookup"><span data-stu-id="81fd0-248">Use the [nameof](https://msdn.microsoft.com/library/dn986596.aspx) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
><span data-ttu-id="81fd0-249">[!code-csharp[Principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-249">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]</span></span>
>
><span data-ttu-id="81fd0-250">Il [nameof](https://msdn.microsoft.com/library/dn986596.aspx) operatore proteggerà il codice necessario sempre il refactoring (potrebbe essere necessario modificare il nome in `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="81fd0-250">The [nameof](https://msdn.microsoft.com/library/dn986596.aspx) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoiding-tag-helper-conflicts"></a><span data-ttu-id="81fd0-251">Evitare i conflitti di Helper Tag</span><span class="sxs-lookup"><span data-stu-id="81fd0-251">Avoiding Tag Helper conflicts</span></span>

<span data-ttu-id="81fd0-252">In questa sezione, scrivere una coppia degli helper di tag di collegamento automatico.</span><span class="sxs-lookup"><span data-stu-id="81fd0-252">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="81fd0-253">Il primo sostituirà markup contenente un URL che inizia con HTTP in un HTML ancoraggio tag che contiene lo stesso URL (e generando un collegamento all'URL).</span><span class="sxs-lookup"><span data-stu-id="81fd0-253">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="81fd0-254">Il secondo sarà eseguire la stessa operazione per un URL a partire da WWW.</span><span class="sxs-lookup"><span data-stu-id="81fd0-254">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="81fd0-255">Poiché questi due helper sono strettamente correlati e potrebbero refactoring in futuro, si sarà mantenerli nello stesso file.</span><span class="sxs-lookup"><span data-stu-id="81fd0-255">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="81fd0-256">Aggiungere il seguente `AutoLinkerHttpTagHelper` classe per il *TagHelpers* cartella.</span><span class="sxs-lookup"><span data-stu-id="81fd0-256">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    <span data-ttu-id="81fd0-257">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-257">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]</span></span>

    >[!NOTE]
    ><span data-ttu-id="81fd0-258">Il `AutoLinkerHttpTagHelper` classe destinazioni `p` elementi e viene utilizzato [Regex](https://msdn.microsoft.com/library/system.text.regularexpressions.regex.aspx) per creare il punto di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="81fd0-258">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://msdn.microsoft.com/library/system.text.regularexpressions.regex.aspx) to create the anchor.</span></span>

2.  <span data-ttu-id="81fd0-259">Aggiungere il markup seguente alla fine del *Views/Home/Contact.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="81fd0-259">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    <span data-ttu-id="81fd0-260">[!code-html[Principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-260">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]</span></span>

3.  <span data-ttu-id="81fd0-261">Eseguire l'app e verificare che l'helper di tag visualizzi correttamente il punto di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="81fd0-261">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="81fd0-262">Aggiornamento di `AutoLinker` classe per includere il `AutoLinkerWwwTagHelper` che convertirà www testo in un tag di ancoraggio che contiene anche il testo www originale.</span><span class="sxs-lookup"><span data-stu-id="81fd0-262">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="81fd0-263">Il codice aggiornato è evidenziato di seguito:</span><span class="sxs-lookup"><span data-stu-id="81fd0-263">The updated code is highlighted below:</span></span>

    <span data-ttu-id="81fd0-264">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-264">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]</span></span>

5.  <span data-ttu-id="81fd0-265">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="81fd0-265">Run the app.</span></span> <span data-ttu-id="81fd0-266">Si noti il testo www viene visualizzato come collegamento ma non è il testo HTTP.</span><span class="sxs-lookup"><span data-stu-id="81fd0-266">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="81fd0-267">Se si inserisce un punto di interruzione in entrambe le classi, si noterà che la classe helper di tag HTTP viene eseguito per prima.</span><span class="sxs-lookup"><span data-stu-id="81fd0-267">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="81fd0-268">Il problema è che viene memorizzato nella cache l'output di helper di tag e, quando viene eseguito l'helper di tag WWW, sovrascrive l'output memorizzato nella cache l'helper di tag HTTP.</span><span class="sxs-lookup"><span data-stu-id="81fd0-268">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="81fd0-269">Più avanti nell'esercitazione vedremo come controllare l'ordine in cui eseguire gli helper di tag in.</span><span class="sxs-lookup"><span data-stu-id="81fd0-269">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="81fd0-270">Il codice, è possibile risolvere con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="81fd0-270">We'll fix the code with the following:</span></span>

    <span data-ttu-id="81fd0-271">[!code-csharp[Principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-271">[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]</span></span>

    >[!NOTE]
    ><span data-ttu-id="81fd0-272">Nell'edizione prima degli helper di tag di collegamento automatico, è stato ottenuto il contenuto di destinazione con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="81fd0-272">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    ><span data-ttu-id="81fd0-273">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-273">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]</span></span>
    >
    ><span data-ttu-id="81fd0-274">Ovvero, si chiama `GetChildContentAsync` utilizzando il `TagHelperOutput` passato il `ProcessAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="81fd0-274">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="81fd0-275">Come accennato in precedenza, perché l'output viene memorizzato nella cache, l'ultimo tag helper per l'esecuzione di wins.</span><span class="sxs-lookup"><span data-stu-id="81fd0-275">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="81fd0-276">È stato risolto il problema con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="81fd0-276">You fixed that problem with the following code:</span></span>
    >
    ><span data-ttu-id="81fd0-277">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-277">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]</span></span>
    >
    ><span data-ttu-id="81fd0-278">Il codice precedente verifica se il contenuto è stato modificato e, in caso affermativo, ottiene il contenuto del buffer di output.</span><span class="sxs-lookup"><span data-stu-id="81fd0-278">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="81fd0-279">Eseguire l'applicazione e verificare che i due collegamenti funzionino come previsto.</span><span class="sxs-lookup"><span data-stu-id="81fd0-279">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="81fd0-280">Sebbene potrebbe essere visualizzato che l'helper di tag del linker automatico sia corretto e completo, con un problema complesso.</span><span class="sxs-lookup"><span data-stu-id="81fd0-280">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="81fd0-281">Se l'helper di tag WWW viene eseguito per primo, i collegamenti www non saranno corretti.</span><span class="sxs-lookup"><span data-stu-id="81fd0-281">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="81fd0-282">Aggiornare il codice aggiungendo i `Order` overload per controllare l'ordine in cui viene eseguito il tag in.</span><span class="sxs-lookup"><span data-stu-id="81fd0-282">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="81fd0-283">Il `Order` proprietà determina l'ordine di esecuzione rispetto alla altri helper di tag come destinazione lo stesso elemento.</span><span class="sxs-lookup"><span data-stu-id="81fd0-283">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="81fd0-284">Il valore dell'ordine predefinito è zero e le istanze con i valori più bassi vengono eseguite per prime.</span><span class="sxs-lookup"><span data-stu-id="81fd0-284">The default order value is zero and instances with lower values are executed first.</span></span>

    <span data-ttu-id="81fd0-285">[!code-csharp[Principale](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-285">[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]</span></span>
    
    <span data-ttu-id="81fd0-286">Il codice sopra riportato garantisce che l'helper di tag HTTP viene eseguito prima dell'helper di tag WWW.</span><span class="sxs-lookup"><span data-stu-id="81fd0-286">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="81fd0-287">Modifica `Order` a `MaxValue` e verificare che il markup generato per il tag WWW non è corretto.</span><span class="sxs-lookup"><span data-stu-id="81fd0-287">Change `Order` to `MaxValue` and verify that the markup generated for the  WWW tag is incorrect.</span></span>

## <a name="inspecting-and-retrieving-child-content"></a><span data-ttu-id="81fd0-288">Controllo e il recupero del contenuto figlio</span><span class="sxs-lookup"><span data-stu-id="81fd0-288">Inspecting and retrieving child content</span></span>

<span data-ttu-id="81fd0-289">Gli helper di tag forniscono diverse proprietà per recuperare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="81fd0-289">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="81fd0-290">Il risultato di `GetChildContentAsync` può essere aggiunta alla fine `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="81fd0-290">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="81fd0-291">È possibile controllare il risultato di `GetChildContentAsync` con `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="81fd0-291">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="81fd0-292">Se si modifica `output.Content`, non verrà eseguito o visualizzato a meno che si chiama il corpo di helper tag `GetChildContentAsync` come esempio automatica linker:</span><span class="sxs-lookup"><span data-stu-id="81fd0-292">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

<span data-ttu-id="81fd0-293">[!code-csharp[Principale](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]</span><span class="sxs-lookup"><span data-stu-id="81fd0-293">[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]</span></span>

-  <span data-ttu-id="81fd0-294">Più chiamate al metodo `GetChildContentAsync` restituirà lo stesso valore e non verrà eseguita nuovamente la `TagHelper` corpo a meno che non viene passato un parametro false che indica di non utilizzare il risultato memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="81fd0-294">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating  not use the cached result.</span></span>
