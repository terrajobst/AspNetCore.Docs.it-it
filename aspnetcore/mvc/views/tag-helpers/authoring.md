---
title: Creazione e modifica gli helper di Tag in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare gli helper di Tag in ASP.NET Core.
keywords: ASP.NET Core, gli helper di tag
ms.author: riande
manager: wpickett
ms.date: 06/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1a5222da1380c2fe768b287bfa1a49b300c02f2b
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="c571d-104">Gli helper di Tag in ASP.NET di base, una procedura dettagliata con esempi di creazione</span><span class="sxs-lookup"><span data-stu-id="c571d-104">Authoring Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="c571d-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c571d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="c571d-106">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="c571d-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a><span data-ttu-id="c571d-107">Introduzione agli helper di Tag</span><span class="sxs-lookup"><span data-stu-id="c571d-107">Getting started with Tag Helpers</span></span>

<span data-ttu-id="c571d-108">In questa esercitazione fornisce un'introduzione alla programmazione gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-108">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="c571d-109">[Introduzione agli helper di Tag](intro.md) vengono descritti i vantaggi che forniscono gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-109">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="c571d-110">Un helper tag è qualsiasi classe che implementa il `ITagHelper` interfaccia.</span><span class="sxs-lookup"><span data-stu-id="c571d-110">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="c571d-111">Tuttavia, quando si crea un helper tag, in genere deriva da `TagHelper`, pertanto consente di accedere a questa il `Process` metodo.</span><span class="sxs-lookup"><span data-stu-id="c571d-111">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="c571d-112">Creare un nuovo progetto ASP.NET Core denominato **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="c571d-112">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="c571d-113">Autenticazione non è necessario per questo progetto.</span><span class="sxs-lookup"><span data-stu-id="c571d-113">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="c571d-114">Creare una cartella per contenere gli helper di Tag chiamato *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="c571d-114">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="c571d-115">Il *TagHelpers* cartella *non* obbligatorio, ma è una convenzione ragionevole.</span><span class="sxs-lookup"><span data-stu-id="c571d-115">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="c571d-116">Ora è possibile iniziare subito la scrittura di un helper tag semplice.</span><span class="sxs-lookup"><span data-stu-id="c571d-116">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="c571d-117">Un Helper Tag minima</span><span class="sxs-lookup"><span data-stu-id="c571d-117">A minimal Tag Helper</span></span>

<span data-ttu-id="c571d-118">In questa sezione, scrivere un helper tag che aggiorna un tag di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c571d-118">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="c571d-119">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c571d-119">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="c571d-120">Il server utilizzerà l'helper di tag di posta elettronica per convertire il markup in quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c571d-120">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

<span data-ttu-id="c571d-121">Vale a dire un tag di ancoraggio che è un collegamento di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c571d-121">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="c571d-122">È possibile eseguire questa operazione se si sta scrivendo un motore di blog ed è necessario per inviare posta elettronica di marketing, supporto e altri contatti, tutti allo stesso dominio.</span><span class="sxs-lookup"><span data-stu-id="c571d-122">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="c571d-123">Aggiungere il seguente `EmailTagHelper` classe per il *TagHelpers* cartella.</span><span class="sxs-lookup"><span data-stu-id="c571d-123">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    <span data-ttu-id="c571d-124">**Note:**</span><span class="sxs-lookup"><span data-stu-id="c571d-124">**Notes:**</span></span>
    
    * <span data-ttu-id="c571d-125">Gli helper di tag utilizzano una convenzione di denominazione che ha come destinazione gli elementi del nome della classe radice (meno di *helper tag* parte del nome della classe).</span><span class="sxs-lookup"><span data-stu-id="c571d-125">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="c571d-126">In questo esempio, il nome radice del **posta elettronica**helper tag è *posta elettronica*, pertanto il `<email>` saranno destinati i tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-126">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="c571d-127">Questa convenzione di denominazione dovrebbe funzionare per la maggior parte degli helper di tag, in seguito verrà illustrato come eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="c571d-127">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="c571d-128">La classe `EmailTagHelper` deriva da `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="c571d-128">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="c571d-129">La `TagHelper` classe fornisce metodi e proprietà per la scrittura gli helper di Tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-129">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="c571d-130">Sottoposto a override `Process` metodo controlla l'helper di tag cosa quando eseguita.</span><span class="sxs-lookup"><span data-stu-id="c571d-130">The  overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="c571d-131">Il `TagHelper` classe fornisce anche una versione asincrona (`ProcessAsync`) con gli stessi parametri.</span><span class="sxs-lookup"><span data-stu-id="c571d-131">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="c571d-132">Il parametro di contesto per `Process` (e `ProcessAsync`) contiene informazioni associate all'esecuzione del tag HTML corrente.</span><span class="sxs-lookup"><span data-stu-id="c571d-132">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="c571d-133">Il parametro di output per `Process` (e `ProcessAsync`) contiene un elemento HTML con stato rappresentativo di origine originale utilizzato per generare un tag HTML e il contenuto.</span><span class="sxs-lookup"><span data-stu-id="c571d-133">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="c571d-134">Il nome della classe è un suffisso di **helper tag**, ovvero *non* obbligatorio, ma è considerata una convenzione prassi migliori.</span><span class="sxs-lookup"><span data-stu-id="c571d-134">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="c571d-135">È possibile dichiarare la classe come:</span><span class="sxs-lookup"><span data-stu-id="c571d-135">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="c571d-136">Per rendere il `EmailTagHelper` classe disponibile per tutti i nostri visualizzazioni Razor, aggiungere il `addTagHelper` direttiva per il *Views/_ViewImports.cshtml* file:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="c571d-136">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="c571d-137">Il codice precedente Usa la sintassi con caratteri jolly per specificare che tutti gli helper di tag in questo assembly saranno disponibili.</span><span class="sxs-lookup"><span data-stu-id="c571d-137">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="c571d-138">La prima stringa dopo `@addTagHelper` specifica l'helper di tag per caricare (usare "*" per tutti gli helper di tag), e la seconda stringa "AuthoringTagHelpers" Specifica l'assembly è l'helper di tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-138">The first string after `@addTagHelper` specifies the tag helper to load (Use "*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="c571d-139">Si noti inoltre che la seconda riga riporta gli helper di tag di ASP.NET MVC di base utilizzando la sintassi con caratteri jolly (tali helper vengono discussi in [introduzione per gli helper di Tag](intro.md).) È il `@addTagHelper` direttiva che rende disponibili per la visualizzazione Razor l'helper di tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-139">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="c571d-140">In alternativa, è possibile fornire il nome completo (FQN) di un helper di tag, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c571d-140">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
    
    <span data-ttu-id="c571d-141">Per aggiungere un helper tag a una vista utilizzando un FQN, aggiungere innanzitutto i FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) e quindi il nome dell'assembly (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="c571d-141">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="c571d-142">La maggior parte degli sviluppatori preferiranno utilizzare la sintassi con caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="c571d-142">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="c571d-143">[Introduzione agli helper di Tag](intro.md) entra in dettaglio nella sintassi di aggiunta, rimozione, gerarchia e caratteri jolly di supporto dei tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-143">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="c571d-144">Aggiornare il markup di *Views/Home/Contact.cshtml* file con queste modifiche:</span><span class="sxs-lookup"><span data-stu-id="c571d-144">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  <span data-ttu-id="c571d-145">Eseguire l'app e utilizzare il browser preferito per visualizzare l'origine HTML, per poter verificare che i tag di posta elettronica vengono sostituiti con il tag di ancoraggio (ad esempio, `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="c571d-145">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="c571d-146">*Supporto* e *Marketing* il rendering viene eseguito come collegamenti, ma che non dispongono di un `href` attributo in modo funzionale.</span><span class="sxs-lookup"><span data-stu-id="c571d-146">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="c571d-147">Che verrà risolto nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="c571d-147">We'll fix that in the next section.</span></span>

<span data-ttu-id="c571d-148">Nota: Come tag HTML e gli attributi, tag, i nomi delle classi e attributi in Razor e c# non sono rilevanti.</span><span class="sxs-lookup"><span data-stu-id="c571d-148">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="c571d-149">SetAttribute e SetContent</span><span class="sxs-lookup"><span data-stu-id="c571d-149">SetAttribute and SetContent</span></span>

<span data-ttu-id="c571d-150">In questa sezione verrà aggiornata la `EmailTagHelper` in modo che verrà creato un tag di ancoraggio valido per la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c571d-150">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="c571d-151">Verrà aggiornata in modo da richiedere informazioni da una visualizzazione Razor (sotto forma di un `mail-to` attributo) e utilizzarlo per generare il punto di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="c571d-151">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="c571d-152">Aggiornamento di `EmailTagHelper` classe con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c571d-152">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="c571d-153">**Note:**</span><span class="sxs-lookup"><span data-stu-id="c571d-153">**Notes:**</span></span>

* <span data-ttu-id="c571d-154">Base alla convezione Pascal proprietà nomi di classe e per gli helper di tag vengono convertiti in loro [inferiore case kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="c571d-154">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="c571d-155">Pertanto, utilizzare il `MailTo` attributo, si userà `<email mail-to="value"/>` equivalente.</span><span class="sxs-lookup"><span data-stu-id="c571d-155">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="c571d-156">L'ultima riga imposta il contenuto completato per l'helper di tag minima funzionale.</span><span class="sxs-lookup"><span data-stu-id="c571d-156">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="c571d-157">La riga evidenziata mostra la sintassi per l'aggiunta di attributi:</span><span class="sxs-lookup"><span data-stu-id="c571d-157">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="c571d-158">Questo approccio funziona per l'attributo "href" fino a quando non esiste nella raccolta di attributi.</span><span class="sxs-lookup"><span data-stu-id="c571d-158">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="c571d-159">È inoltre possibile utilizzare il `output.Attributes.Add` metodo per aggiungere un attributo di helper di tag di fine della raccolta di attributi di tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-159">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="c571d-160">Aggiornare il markup di *Views/Home/Contact.cshtml* file con queste modifiche:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="c571d-160">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="c571d-161">Eseguire l'applicazione e verificare che genera collegamenti corretti.</span><span class="sxs-lookup"><span data-stu-id="c571d-161">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="c571d-162">Se si volesse scrivere il messaggio di posta elettronica tag chiusura automatica (`<email mail-to="Rick" />`), l'output finale potrebbe anche essere a chiusura automatica.</span><span class="sxs-lookup"><span data-stu-id="c571d-162">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="c571d-163">Per abilitare la possibilità di scrivere il tag con un tag di inizio (`<email mail-to="Rick">`) è necessario contrassegnare la classe con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c571d-163">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    <span data-ttu-id="c571d-164">Con un helper di tag di posta elettronica chiusura automatica, l'output sarà `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="c571d-164">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="c571d-165">Chiusura automatica di tag di ancoraggio non sono validi HTML, in modo che non si desidera crearne uno, ma si potrebbe voler creare un helper di tag di chiusura automatica.</span><span class="sxs-lookup"><span data-stu-id="c571d-165">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="c571d-166">Gli helper di tag impostato il tipo di `TagMode` proprietà dopo la lettura di un tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-166">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="c571d-167">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="c571d-167">ProcessAsync</span></span>

<span data-ttu-id="c571d-168">In questa sezione, è necessario scrivere un helper del messaggio di posta elettronica asincrona.</span><span class="sxs-lookup"><span data-stu-id="c571d-168">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="c571d-169">Sostituire il `EmailTagHelper` classe con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c571d-169">Replace the `EmailTagHelper` class with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    <span data-ttu-id="c571d-170">**Note:**</span><span class="sxs-lookup"><span data-stu-id="c571d-170">**Notes:**</span></span>

    * <span data-ttu-id="c571d-171">Questa versione utilizza asincrona `ProcessAsync` metodo.</span><span class="sxs-lookup"><span data-stu-id="c571d-171">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="c571d-172">Asincrona `GetChildContentAsync` restituisce un `Task` contenente il `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="c571d-172">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="c571d-173">Utilizzare il `output` parametro per ottenere il contenuto dell'elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="c571d-173">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="c571d-174">Apportare le seguenti modifiche per il *Views/Home/Contact.cshtml* file pertanto l'helper di tag è possibile recuperare il messaggio di posta elettronica di destinazione.</span><span class="sxs-lookup"><span data-stu-id="c571d-174">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  <span data-ttu-id="c571d-175">Eseguire l'applicazione e verificare che genera collegamenti di posta elettronica valido.</span><span class="sxs-lookup"><span data-stu-id="c571d-175">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="c571d-176">RemoveAll, PreContent.SetHtmlContent e PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="c571d-176">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="c571d-177">Aggiungere il seguente `BoldTagHelper` classe per il *TagHelpers* cartella.</span><span class="sxs-lookup"><span data-stu-id="c571d-177">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    <span data-ttu-id="c571d-178">**Note:**</span><span class="sxs-lookup"><span data-stu-id="c571d-178">**Notes:**</span></span>
    
    * <span data-ttu-id="c571d-179">Il `[HtmlTargetElement]` attributo passa un parametro di attributo che specifica che qualsiasi elemento HTML che contiene un attributo HTML denominato "bold" corrisponderanno, e `Process` verrà eseguito il metodo di override nella classe.</span><span class="sxs-lookup"><span data-stu-id="c571d-179">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="c571d-180">In questo esempio, il `Process` metodo rimuove l'attributo "bold" e racchiude contenente markup con `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="c571d-180">In our sample, the `Process`  method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="c571d-181">Perché non si desidera sostituire il contenuto del tag esistente, è necessario scrivere l'apertura `<strong>` tag con il `PreContent.SetHtmlContent` metodo e la chiusura `</strong>` tag con il `PostContent.SetHtmlContent` metodo.</span><span class="sxs-lookup"><span data-stu-id="c571d-181">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="c571d-182">Modificare il *About.cshtml* vista per contenere un `bold` valore dell'attributo.</span><span class="sxs-lookup"><span data-stu-id="c571d-182">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="c571d-183">Il codice completo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c571d-183">The completed code is shown below.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  <span data-ttu-id="c571d-184">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="c571d-184">Run the app.</span></span> <span data-ttu-id="c571d-185">È possibile utilizzare il browser preferito per controllare l'origine e verificare il markup.</span><span class="sxs-lookup"><span data-stu-id="c571d-185">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="c571d-186">Il `[HtmlTargetElement]` attributo destinata solo il markup HTML che fornisce un nome di attributo di "bold".</span><span class="sxs-lookup"><span data-stu-id="c571d-186">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="c571d-187">Il `<bold>` elemento non è stato modificato per l'helper di tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-187">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="c571d-188">Impostare come commento il `[HtmlTargetElement]` riga dell'attributo e si utilizzerà come destinazione `<bold>` tag, vale a dire i tag HTML nel formato `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="c571d-188">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="c571d-189">Tenere presente che la convenzione di denominazione predefinito corrisponderà al nome di classe **grassetto**helper tag per `<bold>` tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-189">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="c571d-190">Eseguire l'applicazione e verificare che il `<bold>` tag viene elaborata l'helper di tag.</span><span class="sxs-lookup"><span data-stu-id="c571d-190">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="c571d-191">La decorazione di una classe con più `[HtmlTargetElement]` attributi risulta un OR logico delle destinazioni.</span><span class="sxs-lookup"><span data-stu-id="c571d-191">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="c571d-192">Ad esempio, tramite il codice riportato di seguito, un tag in grassetto o un attributo bold corrisponderanno.</span><span class="sxs-lookup"><span data-stu-id="c571d-192">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="c571d-193">Quando più attributi vengono aggiunti alla stessa istruzione, il runtime li gestisce come un AND logico.</span><span class="sxs-lookup"><span data-stu-id="c571d-193">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="c571d-194">Ad esempio, il codice riportato di seguito, un elemento HTML deve essere denominato "bold" con un attributo denominato "bold" (`<bold bold />`) per trovare la corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="c571d-194">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="c571d-195">È inoltre possibile utilizzare il `[HtmlTargetElement]` per modificare il nome dell'elemento di destinazione.</span><span class="sxs-lookup"><span data-stu-id="c571d-195">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="c571d-196">Ad esempio se si desidera che il `BoldTagHelper` alla destinazione `<MyBold>` tag, utilizzare l'attributo seguente:</span><span class="sxs-lookup"><span data-stu-id="c571d-196">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a><span data-ttu-id="c571d-197">Il passaggio di un modello a un Helper Tag</span><span class="sxs-lookup"><span data-stu-id="c571d-197">Passing a model to a Tag Helper</span></span>

1.  <span data-ttu-id="c571d-198">Aggiungere un *modelli* cartella.</span><span class="sxs-lookup"><span data-stu-id="c571d-198">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="c571d-199">Aggiungere la classe `WebsiteContext` seguente alla cartella *Models*:</span><span class="sxs-lookup"><span data-stu-id="c571d-199">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  <span data-ttu-id="c571d-200">Aggiungere il seguente `WebsiteInformationTagHelper` classe per il *TagHelpers* cartella.</span><span class="sxs-lookup"><span data-stu-id="c571d-200">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    <span data-ttu-id="c571d-201">**Note:**</span><span class="sxs-lookup"><span data-stu-id="c571d-201">**Notes:**</span></span>
    
    * <span data-ttu-id="c571d-202">Come accennato in precedenza, gli helper di tag converte i nomi delle classi in base alla convezione Pascal c# e le proprietà per gli helper di tag in [inferiore case kebab](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="c571d-202">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="c571d-203">Pertanto, utilizzare il `WebsiteInformationTagHelper` in Razor, verrà scritto `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="c571d-203">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="c571d-204">Non si desidera identificare in modo esplicito l'elemento di destinazione con il `[HtmlTargetElement]` attributo, pertanto il valore predefinito di `website-information` saranno destinati.</span><span class="sxs-lookup"><span data-stu-id="c571d-204">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="c571d-205">Se è stato applicato l'attributo seguente (notare non case kebab ma corrisponde al nome di classe):</span><span class="sxs-lookup"><span data-stu-id="c571d-205">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="c571d-206">Il tag del case kebab inferiore `<website-information />` non corrispondono.</span><span class="sxs-lookup"><span data-stu-id="c571d-206">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="c571d-207">Se si desidera utilizzare il `[HtmlTargetElement]` attributo, utilizzare case kebab come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c571d-207">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="c571d-208">Gli elementi che sono a chiusura automatica non abbiano contenuto.</span><span class="sxs-lookup"><span data-stu-id="c571d-208">Elements that are self-closing have no content.</span></span> <span data-ttu-id="c571d-209">Per questo esempio, il markup Razor utilizzerà un tag di chiusura automatica, ma l'helper di tag creeranno un [sezione](http://www.w3.org/TR/html5/sections.html#the-section-element) elemento (che non è chiusura automatica e si scrive il contenuto all'interno di `section` elemento).</span><span class="sxs-lookup"><span data-stu-id="c571d-209">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="c571d-210">Pertanto, è necessario impostare `TagMode` a `StartTagAndEndTag` per scrivere l'output.</span><span class="sxs-lookup"><span data-stu-id="c571d-210">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="c571d-211">In alternativa, è possibile impostare come commento la riga che imposta `TagMode` e la scrittura di markup con un tag di chiusura.</span><span class="sxs-lookup"><span data-stu-id="c571d-211">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="c571d-212">(Più avanti in questa esercitazione viene fornito codice di esempio).</span><span class="sxs-lookup"><span data-stu-id="c571d-212">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="c571d-213">Il `$` (segno di dollaro) nella riga seguente viene utilizzato un [interpolati stringa](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="c571d-213">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="c571d-214">Aggiungere il markup seguente per il *About.cshtml* visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c571d-214">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="c571d-215">Il markup evidenziato Visualizza le informazioni sul sito web.</span><span class="sxs-lookup"><span data-stu-id="c571d-215">The highlighted markup displays the web site information.</span></span>
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > <span data-ttu-id="c571d-216">Nel codice Razor illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c571d-216">In the Razor markup shown below:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    ><span data-ttu-id="c571d-217">Razor conosca il `info` attributo è una classe, non è una stringa, e si desidera scrivere codice c#.</span><span class="sxs-lookup"><span data-stu-id="c571d-217">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="c571d-218">Qualsiasi attributo helper di tag non di tipo stringa deve essere scritta senza la `@` carattere.</span><span class="sxs-lookup"><span data-stu-id="c571d-218">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="c571d-219">Eseguire l'app e passare alla visualizzazione di informazioni su per visualizzare le informazioni sul sito web.</span><span class="sxs-lookup"><span data-stu-id="c571d-219">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="c571d-220">È possibile utilizzare il markup seguente con un tag di chiusura e rimuovere la riga con `TagMode.StartTagAndEndTag` nell'helper tag:</span><span class="sxs-lookup"><span data-stu-id="c571d-220">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="c571d-221">Helper di Tag di condizione</span><span class="sxs-lookup"><span data-stu-id="c571d-221">Condition Tag Helper</span></span>

<span data-ttu-id="c571d-222">L'helper di tag di condizione esegue il rendering di output quando viene passato un valore true.</span><span class="sxs-lookup"><span data-stu-id="c571d-222">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="c571d-223">Aggiungere il seguente `ConditionTagHelper` classe per il *TagHelpers* cartella.</span><span class="sxs-lookup"><span data-stu-id="c571d-223">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  <span data-ttu-id="c571d-224">Sostituire il contenuto del *Views/Home/Index.cshtml* file con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="c571d-224">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

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
    
3.  <span data-ttu-id="c571d-225">Sostituire il `Index` metodo il `Home` controller con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c571d-225">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  <span data-ttu-id="c571d-226">Eseguire l'app e passare alla home page.</span><span class="sxs-lookup"><span data-stu-id="c571d-226">Run the app and browse to the home page.</span></span> <span data-ttu-id="c571d-227">Il markup in condizionale `div` non vengono visualizzati.</span><span class="sxs-lookup"><span data-stu-id="c571d-227">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="c571d-228">Aggiungere la stringa di query `?approved=true` all'URL (ad esempio, `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="c571d-228">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="c571d-229">`approved`è impostato su true e condizionale markup verrà visualizzato.</span><span class="sxs-lookup"><span data-stu-id="c571d-229">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="c571d-230">Utilizzare il [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operatore per specificare l'attributo di destinazione, anziché specificare una stringa, come avviene per l'helper di tag in grassetto:</span><span class="sxs-lookup"><span data-stu-id="c571d-230">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
><span data-ttu-id="c571d-231">Il [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operatore proteggerà il codice necessario sempre il refactoring (potrebbe essere necessario modificare il nome in `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="c571d-231">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoiding-tag-helper-conflicts"></a><span data-ttu-id="c571d-232">Evitare i conflitti di Helper Tag</span><span class="sxs-lookup"><span data-stu-id="c571d-232">Avoiding Tag Helper conflicts</span></span>

<span data-ttu-id="c571d-233">In questa sezione, scrivere una coppia degli helper di tag di collegamento automatico.</span><span class="sxs-lookup"><span data-stu-id="c571d-233">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="c571d-234">Il primo sostituirà markup contenente un URL che inizia con HTTP in un HTML ancoraggio tag che contiene lo stesso URL (e generando un collegamento all'URL).</span><span class="sxs-lookup"><span data-stu-id="c571d-234">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="c571d-235">Il secondo sarà eseguire la stessa operazione per un URL a partire da WWW.</span><span class="sxs-lookup"><span data-stu-id="c571d-235">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="c571d-236">Poiché questi due helper sono strettamente correlati e potrebbero refactoring in futuro, si sarà mantenerli nello stesso file.</span><span class="sxs-lookup"><span data-stu-id="c571d-236">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="c571d-237">Aggiungere il seguente `AutoLinkerHttpTagHelper` classe per il *TagHelpers* cartella.</span><span class="sxs-lookup"><span data-stu-id="c571d-237">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    ><span data-ttu-id="c571d-238">Il `AutoLinkerHttpTagHelper` classe destinazioni `p` elementi e viene utilizzato [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) per creare il punto di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="c571d-238">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="c571d-239">Aggiungere il markup seguente alla fine del *Views/Home/Contact.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="c571d-239">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  <span data-ttu-id="c571d-240">Eseguire l'app e verificare che l'helper di tag visualizzi correttamente il punto di ancoraggio.</span><span class="sxs-lookup"><span data-stu-id="c571d-240">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="c571d-241">Aggiornamento di `AutoLinker` classe per includere il `AutoLinkerWwwTagHelper` che convertirà www testo in un tag di ancoraggio che contiene anche il testo www originale.</span><span class="sxs-lookup"><span data-stu-id="c571d-241">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="c571d-242">Il codice aggiornato è evidenziato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c571d-242">The updated code is highlighted below:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  <span data-ttu-id="c571d-243">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="c571d-243">Run the app.</span></span> <span data-ttu-id="c571d-244">Si noti il testo www viene visualizzato come collegamento ma non è il testo HTTP.</span><span class="sxs-lookup"><span data-stu-id="c571d-244">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="c571d-245">Se si inserisce un punto di interruzione in entrambe le classi, si noterà che la classe helper di tag HTTP viene eseguito per prima.</span><span class="sxs-lookup"><span data-stu-id="c571d-245">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="c571d-246">Il problema è che viene memorizzato nella cache l'output di helper di tag e, quando viene eseguito l'helper di tag WWW, sovrascrive l'output memorizzato nella cache l'helper di tag HTTP.</span><span class="sxs-lookup"><span data-stu-id="c571d-246">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="c571d-247">Più avanti nell'esercitazione vedremo come controllare l'ordine in cui eseguire gli helper di tag in.</span><span class="sxs-lookup"><span data-stu-id="c571d-247">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="c571d-248">Il codice, è possibile risolvere con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c571d-248">We'll fix the code with the following:</span></span>

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    ><span data-ttu-id="c571d-249">Nell'edizione prima degli helper di tag di collegamento automatico, è stato ottenuto il contenuto di destinazione con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c571d-249">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    ><span data-ttu-id="c571d-250">Ovvero, si chiama `GetChildContentAsync` utilizzando il `TagHelperOutput` passato il `ProcessAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="c571d-250">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="c571d-251">Come accennato in precedenza, perché l'output viene memorizzato nella cache, l'ultimo tag helper per l'esecuzione di wins.</span><span class="sxs-lookup"><span data-stu-id="c571d-251">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="c571d-252">È stato risolto il problema con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c571d-252">You fixed that problem with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    ><span data-ttu-id="c571d-253">Il codice precedente verifica se il contenuto è stato modificato e, in caso affermativo, ottiene il contenuto del buffer di output.</span><span class="sxs-lookup"><span data-stu-id="c571d-253">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="c571d-254">Eseguire l'applicazione e verificare che i due collegamenti funzionino come previsto.</span><span class="sxs-lookup"><span data-stu-id="c571d-254">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="c571d-255">Sebbene potrebbe essere visualizzato che l'helper di tag del linker automatico sia corretto e completo, con un problema complesso.</span><span class="sxs-lookup"><span data-stu-id="c571d-255">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="c571d-256">Se l'helper di tag WWW viene eseguito per primo, i collegamenti www non saranno corretti.</span><span class="sxs-lookup"><span data-stu-id="c571d-256">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="c571d-257">Aggiornare il codice aggiungendo i `Order` overload per controllare l'ordine in cui viene eseguito il tag in.</span><span class="sxs-lookup"><span data-stu-id="c571d-257">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="c571d-258">Il `Order` proprietà determina l'ordine di esecuzione rispetto alla altri helper di tag come destinazione lo stesso elemento.</span><span class="sxs-lookup"><span data-stu-id="c571d-258">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="c571d-259">Il valore dell'ordine predefinito è zero e le istanze con i valori più bassi vengono eseguite per prime.</span><span class="sxs-lookup"><span data-stu-id="c571d-259">The default order value is zero and instances with lower values are executed first.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    <span data-ttu-id="c571d-260">Il codice sopra riportato garantisce che l'helper di tag HTTP viene eseguito prima dell'helper di tag WWW.</span><span class="sxs-lookup"><span data-stu-id="c571d-260">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="c571d-261">Modifica `Order` a `MaxValue` e verificare che il markup generato per il tag WWW non è corretto.</span><span class="sxs-lookup"><span data-stu-id="c571d-261">Change `Order` to `MaxValue` and verify that the markup generated for the  WWW tag is incorrect.</span></span>

## <a name="inspecting-and-retrieving-child-content"></a><span data-ttu-id="c571d-262">Controllo e il recupero del contenuto figlio</span><span class="sxs-lookup"><span data-stu-id="c571d-262">Inspecting and retrieving child content</span></span>

<span data-ttu-id="c571d-263">Gli helper di tag forniscono diverse proprietà per recuperare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="c571d-263">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="c571d-264">Il risultato di `GetChildContentAsync` può essere aggiunta alla fine `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="c571d-264">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="c571d-265">È possibile controllare il risultato di `GetChildContentAsync` con `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="c571d-265">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="c571d-266">Se si modifica `output.Content`, non verrà eseguito o visualizzato a meno che si chiama il corpo di helper tag `GetChildContentAsync` come esempio automatica linker:</span><span class="sxs-lookup"><span data-stu-id="c571d-266">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="c571d-267">Più chiamate al metodo `GetChildContentAsync` restituirà lo stesso valore e non verrà eseguita nuovamente la `TagHelper` corpo a meno che non viene passato un parametro false che indica di non utilizzare il risultato memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="c571d-267">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating  not use the cached result.</span></span>
