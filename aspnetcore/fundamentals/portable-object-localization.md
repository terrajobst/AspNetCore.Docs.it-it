---
title: Configurare la localizzazione oggetto portabile
author: sebastienros
description: In questo articolo vengono introdotti i file oggetto portabile e vengono delineati i passaggi per il loro utilizzo in un'applicazione ASP.NET di base con il framework di base Orchard.
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: ad68c8a7df5a8ea0f7ef42137c29cd3b37657052
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="configure-portable-object-localization-with-orchard-core"></a><span data-ttu-id="3f138-103">Configurare la localizzazione oggetto portabile con Orchard Core</span><span class="sxs-lookup"><span data-stu-id="3f138-103">Configure portable object localization with Orchard Core</span></span>

<span data-ttu-id="3f138-104">Da [Sébastien Ros](https://github.com/sebastienros) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="3f138-104">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="3f138-105">In questo articolo vengono illustrati i passaggi per l'utilizzo di file oggetto portatile (PO) in un'applicazione ASP.NET Core con il [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span><span class="sxs-lookup"><span data-stu-id="3f138-105">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="3f138-106">**Nota:** Orchard Core non è un prodotto Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3f138-106">**Note:** Orchard Core isn't a Microsoft product.</span></span> <span data-ttu-id="3f138-107">Di conseguenza, Microsoft non fornisce supporto per questa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="3f138-107">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="3f138-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3f138-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="3f138-109">Che cos'è un file di ordine di acquisto?</span><span class="sxs-lookup"><span data-stu-id="3f138-109">What is a PO file?</span></span>

<span data-ttu-id="3f138-110">I file di ordine di acquisto vengono distribuiti come file di testo contenente le stringhe tradotte per una lingua specifica.</span><span class="sxs-lookup"><span data-stu-id="3f138-110">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="3f138-111">Alcuni vantaggi dell'utilizzo di file di ordine di acquisto invece *resx* file includono:</span><span class="sxs-lookup"><span data-stu-id="3f138-111">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="3f138-112">I file di ordine di acquisto supportano pluralizzazione; *resx* i file non supportano la pluralizzazione.</span><span class="sxs-lookup"><span data-stu-id="3f138-112">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="3f138-113">Non vengono compilati i file di ordine di acquisto come *resx* file.</span><span class="sxs-lookup"><span data-stu-id="3f138-113">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="3f138-114">Di conseguenza, specializzati passaggi di strumenti e di compilazione non sono necessari.</span><span class="sxs-lookup"><span data-stu-id="3f138-114">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="3f138-115">File di ordine di acquisto funzionano bene con gli strumenti di collaborazione modifica online.</span><span class="sxs-lookup"><span data-stu-id="3f138-115">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="3f138-116">Esempio</span><span class="sxs-lookup"><span data-stu-id="3f138-116">Example</span></span>

<span data-ttu-id="3f138-117">Ecco un file di ordine di acquisto di esempio che contiene la traduzione per le due stringhe in francese, tra cui quella con la forma plurale:</span><span class="sxs-lookup"><span data-stu-id="3f138-117">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="3f138-118">*fr.po*</span><span class="sxs-lookup"><span data-stu-id="3f138-118">*fr.po*</span></span>

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

<span data-ttu-id="3f138-119">Questo esempio viene utilizzata la sintassi seguente:</span><span class="sxs-lookup"><span data-stu-id="3f138-119">This example uses the following syntax:</span></span>

- <span data-ttu-id="3f138-120">`#:`: Un commento che indica il contesto della stringa da convertire.</span><span class="sxs-lookup"><span data-stu-id="3f138-120">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="3f138-121">La stessa stringa potrebbe essere tradotte in modo diverso a seconda di dove è in uso.</span><span class="sxs-lookup"><span data-stu-id="3f138-121">The same string might be translated differently depending on where it's being used.</span></span>
- <span data-ttu-id="3f138-122">`msgid`: La stringa non tradotto.</span><span class="sxs-lookup"><span data-stu-id="3f138-122">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="3f138-123">`msgstr`: La stringa tradotta.</span><span class="sxs-lookup"><span data-stu-id="3f138-123">`msgstr`: The translated string.</span></span>

<span data-ttu-id="3f138-124">Nel caso di supporto di pluralizzazione, è possibile definire altre voci.</span><span class="sxs-lookup"><span data-stu-id="3f138-124">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="3f138-125">`msgid_plural`: La stringa plurale non tradotto.</span><span class="sxs-lookup"><span data-stu-id="3f138-125">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="3f138-126">`msgstr[0]`: La stringa tradotta per il caso, 0.</span><span class="sxs-lookup"><span data-stu-id="3f138-126">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="3f138-127">`msgstr[N]`: La stringa tradotta per il n maiuscole.</span><span class="sxs-lookup"><span data-stu-id="3f138-127">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="3f138-128">La specifica di file di ordine di acquisto è reperibile [qui](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span><span class="sxs-lookup"><span data-stu-id="3f138-128">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="3f138-129">Configurazione del supporto di file di ordine di acquisto in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f138-129">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="3f138-130">Questo esempio è basato su un'applicazione ASP.NET MVC Core generata da un modello di progetto di Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3f138-130">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="3f138-131">Facendo riferimento al pacchetto</span><span class="sxs-lookup"><span data-stu-id="3f138-131">Referencing the package</span></span>

<span data-ttu-id="3f138-132">Aggiungere un riferimento di `OrchardCore.Localization.Core` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="3f138-132">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="3f138-133">È disponibile nel [MyGet](https://www.myget.org/) nell'origine pacchetto seguenti: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="3f138-133">It's available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="3f138-134">Il *csproj* file ora contiene una riga simile alla seguente (numero di versione può variare):</span><span class="sxs-lookup"><span data-stu-id="3f138-134">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="3f138-135">Registrazione del servizio</span><span class="sxs-lookup"><span data-stu-id="3f138-135">Registering the service</span></span>

<span data-ttu-id="3f138-136">Aggiungere i servizi necessari per il `ConfigureServices` metodo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f138-136">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="3f138-137">Aggiungere il middleware necessari per il `Configure` metodo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f138-137">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="3f138-138">Aggiungere il codice seguente alla visualizzazione Razor della scelta.</span><span class="sxs-lookup"><span data-stu-id="3f138-138">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="3f138-139">*About.cshtml* in questo esempio viene usato.</span><span class="sxs-lookup"><span data-stu-id="3f138-139">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="3f138-140">Un `IViewLocalizer` istanza inserito e utilizzato per tradurre il testo "Hello world!".</span><span class="sxs-lookup"><span data-stu-id="3f138-140">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="3f138-141">Creazione di un file di ordine di acquisto</span><span class="sxs-lookup"><span data-stu-id="3f138-141">Creating a PO file</span></span>

<span data-ttu-id="3f138-142">Creare un file denominato  *<culture code>PO* nella cartella radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3f138-142">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="3f138-143">In questo esempio, il nome del file è *fr.po* perché viene utilizzata la lingua francese:</span><span class="sxs-lookup"><span data-stu-id="3f138-143">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="3f138-144">Questo file contiene la stringa da convertire e la stringa convertita francese.</span><span class="sxs-lookup"><span data-stu-id="3f138-144">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="3f138-145">Traduzioni ripristino le impostazioni cultura padre, se necessario.</span><span class="sxs-lookup"><span data-stu-id="3f138-145">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="3f138-146">In questo esempio, il *fr.po* file viene utilizzato se la lingua richiesta è `fr-FR` o `fr-CA`.</span><span class="sxs-lookup"><span data-stu-id="3f138-146">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="3f138-147">Test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="3f138-147">Testing the application</span></span>

<span data-ttu-id="3f138-148">Eseguire l'applicazione e passare all'URL `/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="3f138-148">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="3f138-149">Il testo **Hello world!**</span><span class="sxs-lookup"><span data-stu-id="3f138-149">The text **Hello world!**</span></span> <span data-ttu-id="3f138-150">viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="3f138-150">is displayed.</span></span>

<span data-ttu-id="3f138-151">Passare all'URL `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="3f138-151">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="3f138-152">Il testo **Bonjour le monde!**</span><span class="sxs-lookup"><span data-stu-id="3f138-152">The text **Bonjour le monde!**</span></span> <span data-ttu-id="3f138-153">viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="3f138-153">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="3f138-154">Pluralizzazione</span><span class="sxs-lookup"><span data-stu-id="3f138-154">Pluralization</span></span>

<span data-ttu-id="3f138-155">I file di ordine di acquisto supportano form pluralizzazione, è utile quando la stessa stringa deve essere convertito in modo diverso in base a una cardinalità.</span><span class="sxs-lookup"><span data-stu-id="3f138-155">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="3f138-156">Questa attività viene eseguita complicata dal fatto che ogni linguaggio definisce regole personalizzate per selezionare quali stringa da usare in base alla cardinalità.</span><span class="sxs-lookup"><span data-stu-id="3f138-156">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="3f138-157">Il pacchetto di localizzazione Orchard fornisce un'API per richiamare automaticamente queste diverse forme plurale.</span><span class="sxs-lookup"><span data-stu-id="3f138-157">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="3f138-158">La creazione di file di ordine di acquisto di pluralizzazione</span><span class="sxs-lookup"><span data-stu-id="3f138-158">Creating pluralization PO files</span></span>

<span data-ttu-id="3f138-159">Aggiungere il seguente contenuto indicati in precedenza *fr.po* file:</span><span class="sxs-lookup"><span data-stu-id="3f138-159">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="3f138-160">Vedere [cos'è un file di ordine di acquisto?](#what-is-a-po-file) per una spiegazione di ciò che rappresenta ogni voce in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="3f138-160">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="3f138-161">Aggiunta di una lingua utilizzando pluralizzazione diversi moduli</span><span class="sxs-lookup"><span data-stu-id="3f138-161">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="3f138-162">Le stringhe in lingua inglese e francese utilizzate nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="3f138-162">English and French strings were used in the previous example.</span></span> <span data-ttu-id="3f138-163">Inglese e francese hanno solo due formati di pluralizzazione e condividere le stesse regole di maschera, che viene eseguito il mapping di una cardinalità di uno per la prima forma plurale.</span><span class="sxs-lookup"><span data-stu-id="3f138-163">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="3f138-164">Viene eseguito il mapping di qualsiasi altra cardinalità per la seconda forma plurale.</span><span class="sxs-lookup"><span data-stu-id="3f138-164">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="3f138-165">Non tutti i linguaggi condividono le stesse regole.</span><span class="sxs-lookup"><span data-stu-id="3f138-165">Not all languages share the same rules.</span></span> <span data-ttu-id="3f138-166">Come illustrato con ceco, che ha tre plurale.</span><span class="sxs-lookup"><span data-stu-id="3f138-166">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="3f138-167">Creare il `cs.po` file come indicato di seguito e prendere nota come la pluralizzazione richiede tre traduzioni diverse:</span><span class="sxs-lookup"><span data-stu-id="3f138-167">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[Main](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="3f138-168">Per accettare processi cechi, aggiungere `"cs"` all'elenco delle lingue supportate nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="3f138-168">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

<span data-ttu-id="3f138-169">Modificare il *Views/Home/About.cshtml* file per il rendering delle stringhe localizzate, plurale per cardinalità diversi:</span><span class="sxs-lookup"><span data-stu-id="3f138-169">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="3f138-170">**Nota:** In uno scenario reale, una variabile potrebbe essere utilizzata per rappresentare il conteggio.</span><span class="sxs-lookup"><span data-stu-id="3f138-170">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="3f138-171">In questo caso, si ripete lo stesso codice con tre valori diversi per esporre un case specifico.</span><span class="sxs-lookup"><span data-stu-id="3f138-171">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="3f138-172">Quando si passa a impostazioni cultura, verrà visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="3f138-172">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="3f138-173">Per `/Home/About`:</span><span class="sxs-lookup"><span data-stu-id="3f138-173">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="3f138-174">Per `/Home/About?culture=fr`:</span><span class="sxs-lookup"><span data-stu-id="3f138-174">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="3f138-175">Per `/Home/About?culture=cs`:</span><span class="sxs-lookup"><span data-stu-id="3f138-175">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="3f138-176">Si noti che per le impostazioni cultura ceca, le tre traduzioni diversi.</span><span class="sxs-lookup"><span data-stu-id="3f138-176">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="3f138-177">Le impostazioni cultura in inglese e francese condividono stessa costruzione per le due stringhe tradotte ultimo.</span><span class="sxs-lookup"><span data-stu-id="3f138-177">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="3f138-178">Attività avanzate</span><span class="sxs-lookup"><span data-stu-id="3f138-178">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="3f138-179">Stringhe contextualizing</span><span class="sxs-lookup"><span data-stu-id="3f138-179">Contextualizing strings</span></span>

<span data-ttu-id="3f138-180">Le applicazioni spesso contengono le stringhe da convertire in diverse posizioni.</span><span class="sxs-lookup"><span data-stu-id="3f138-180">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="3f138-181">La stessa stringa può avere una conversione diversa in determinati percorsi all'interno di un'app (visualizzazioni Razor o file di classe).</span><span class="sxs-lookup"><span data-stu-id="3f138-181">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="3f138-182">Un file di ordine di acquisto supporta la nozione di un contesto di file, che può essere usato per suddividere la stringa viene rappresentata.</span><span class="sxs-lookup"><span data-stu-id="3f138-182">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="3f138-183">Utilizzando il contesto di un file, una stringa può essere convertita in modo diverso, a seconda del contesto di file (o mancanza di un contesto di file).</span><span class="sxs-lookup"><span data-stu-id="3f138-183">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="3f138-184">Il nome della classe completa o la vista che viene utilizzata quando si converte una stringa di utilizzare i servizi di localizzazione di ordine di acquisto.</span><span class="sxs-lookup"><span data-stu-id="3f138-184">The PO localization services use the name of the full class or the view that's used when translating a string.</span></span> <span data-ttu-id="3f138-185">Questa operazione viene eseguita impostando il valore per il `msgctxt` voce.</span><span class="sxs-lookup"><span data-stu-id="3f138-185">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="3f138-186">Si consideri una secondaria aggiunta a quelli *fr.po* esempio.</span><span class="sxs-lookup"><span data-stu-id="3f138-186">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="3f138-187">Posizione di una visualizzazione Razor *Views/Home/About.cshtml* può essere definito come il contesto di file mediante l'impostazione riservato `msgctxt` valore della voce:</span><span class="sxs-lookup"><span data-stu-id="3f138-187">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="3f138-188">Con il `msgctxt` impostare di conseguenza, la conversione di testo si verifica durante il passaggio alla `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="3f138-188">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="3f138-189">La traduzione non si verifica durante la navigazione in `/Home/Contact?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="3f138-189">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="3f138-190">Quando nessuna voce specifica sia associata a un contesto di file specificato, meccanismo di fallback del Core Orchard Cerca un file di ordine di acquisto appropriato senza un contesto.</span><span class="sxs-lookup"><span data-stu-id="3f138-190">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="3f138-191">Presupponendo che non vi è alcun contesto di file specifico definito per *Views/Home/Contact.cshtml*, la navigazione a `/Home/Contact?culture=fr-FR` carica un file di ordine di acquisto, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3f138-191">Assuming there's no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="3f138-192">Modifica il percorso dei file di ordine di acquisto</span><span class="sxs-lookup"><span data-stu-id="3f138-192">Changing the location of PO files</span></span>

<span data-ttu-id="3f138-193">Il percorso predefinito dei file di ordine di acquisto può essere modificato `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3f138-193">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="3f138-194">In questo esempio, i file di ordine di acquisto vengono caricati dal *localizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="3f138-194">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="3f138-195">Implementazione di una logica personalizzata per la ricerca dei file di localizzazione</span><span class="sxs-lookup"><span data-stu-id="3f138-195">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="3f138-196">Quando è necessario individuare il file di ordine di acquisto, una logica più complessa la `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interfaccia può essere implementata e registrata come un servizio.</span><span class="sxs-lookup"><span data-stu-id="3f138-196">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="3f138-197">Ciò risulta utile quando i file di ordine di acquisto possono essere archiviati in punti diversi oppure quando i file devono trovarsi all'interno di una gerarchia di cartelle.</span><span class="sxs-lookup"><span data-stu-id="3f138-197">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="3f138-198">Utilizzando una lingua predefinita diversa pluralizzato</span><span class="sxs-lookup"><span data-stu-id="3f138-198">Using a different default pluralized language</span></span>

<span data-ttu-id="3f138-199">Il pacchetto include un `Plural` metodo di estensione che è specifico di due plurale.</span><span class="sxs-lookup"><span data-stu-id="3f138-199">The package includes a `Plural` extension method that's specific to two plural forms.</span></span> <span data-ttu-id="3f138-200">Per le lingue che richiedono ulteriori plurale, creare un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="3f138-200">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="3f138-201">Con un metodo di estensione, non sarà necessario fornire qualsiasi file di localizzazione per la lingua predefinita &mdash; stringhe originali sono già disponibili direttamente nel codice.</span><span class="sxs-lookup"><span data-stu-id="3f138-201">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="3f138-202">È possibile utilizzare il cmdlet più generico `Plural(int count, string[] pluralForms, params object[] arguments)` overload che accetta una matrice di stringhe di traduzioni.</span><span class="sxs-lookup"><span data-stu-id="3f138-202">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>
