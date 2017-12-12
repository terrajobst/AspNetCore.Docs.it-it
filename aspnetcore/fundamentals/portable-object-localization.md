---
title: Configurare la localizzazione oggetto portabile
author: sebastienros
description: In questo articolo vengono introdotti i file oggetto portabile e vengono delineati i passaggi per il loro utilizzo in un'applicazione ASP.NET di base con il framework di base Orchard.
keywords: ASP.NET Core, localizzazione, impostazioni cultura, lingua, oggetto portabile
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 4fa73ae08b10217de657681a27f6097fc3443737
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="configure-portable-object-localization-with-orchard-core"></a>Configurare la localizzazione oggetto portabile con Orchard Core

Da [Sébastien Ros](https://github.com/sebastienros) e [Scott Addie](https://twitter.com/Scott_Addie)

In questo articolo vengono illustrati i passaggi per l'utilizzo di file oggetto portatile (PO) in un'applicazione ASP.NET Core con il [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.

**Nota:** Orchard Core non è un prodotto Microsoft. Di conseguenza, Microsoft non fornisce supporto per questa funzionalità.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-po-file"></a>Che cos'è un file di ordine di acquisto?

I file di ordine di acquisto vengono distribuiti come file di testo contenente le stringhe tradotte per una lingua specifica. Alcuni vantaggi dell'utilizzo di file di ordine di acquisto invece *resx* file includono:
- I file di ordine di acquisto supportano pluralizzazione; *resx* i file non supportano la pluralizzazione.
- Non vengono compilati i file di ordine di acquisto come *resx* file. Di conseguenza, specializzati passaggi di strumenti e di compilazione non sono necessari.
- File di ordine di acquisto funzionano bene con gli strumenti di collaborazione modifica online.

### <a name="example"></a>Esempio

Ecco un file di ordine di acquisto di esempio che contiene la traduzione per le due stringhe in francese, tra cui quella con la forma plurale:

*fr.PO*

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

Questo esempio viene utilizzata la sintassi seguente:

- `#:`: Un commento che indica il contesto della stringa da convertire. La stessa stringa potrebbe essere tradotte in modo diverso a seconda di dove è in uso.
- `msgid`: La stringa non tradotto.
- `msgstr`: La stringa tradotta.

Nel caso di supporto di pluralizzazione, è possibile definire altre voci.

- `msgid_plural`: La stringa plurale non tradotto.
- `msgstr[0]`: La stringa tradotta per il caso, 0.
- `msgstr[N]`: La stringa tradotta per il n maiuscole.

La specifica di file di ordine di acquisto è reperibile [qui](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).

## <a name="configuring-po-file-support-in-aspnet-core"></a>Configurazione del supporto di file di ordine di acquisto in ASP.NET Core

Questo esempio è basato su un'applicazione ASP.NET MVC Core generata da un modello di progetto di Visual Studio 2017.

### <a name="referencing-the-package"></a>Facendo riferimento al pacchetto

Aggiungere un riferimento di `OrchardCore.Localization.Core` pacchetto NuGet. È disponibile nel [MyGet](https://www.myget.org/) nell'origine pacchetto seguenti: https://www.myget.org/F/orchardcore-preview/api/v3/index.json

Il *csproj* file ora contiene una riga simile alla seguente (numero di versione può variare):

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a>Registrazione del servizio

Aggiungere i servizi necessari per il `ConfigureServices` metodo *Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

Aggiungere il middleware necessari per il `Configure` metodo *Startup.cs*:

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

Aggiungere il codice seguente alla visualizzazione Razor della scelta. *About.cshtml* in questo esempio viene usato.

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

Un `IViewLocalizer` istanza inserito e utilizzato per tradurre il testo "Hello world!".

### <a name="creating-a-po-file"></a>Creazione di un file di ordine di acquisto

Creare un file denominato  *<culture code>PO* nella cartella radice dell'applicazione. In questo esempio, il nome del file è *fr.po* perché viene utilizzata la lingua francese:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

Questo file contiene la stringa da convertire e la stringa convertita francese. Traduzioni ripristino le impostazioni cultura padre, se necessario. In questo esempio, il *fr.po* file viene utilizzato se la lingua richiesta è `fr-FR` o `fr-CA`.

### <a name="testing-the-application"></a>Test dell'applicazione

Eseguire l'applicazione e passare all'URL `/Home/About`. Il testo **Hello world!** viene visualizzato.

Passare all'URL `/Home/About?culture=fr-FR`. Il testo **Bonjour le monde!** viene visualizzato.

## <a name="pluralization"></a>Pluralizzazione

I file di ordine di acquisto supportano form pluralizzazione, è utile quando la stessa stringa deve essere convertito in modo diverso in base a una cardinalità. Questa attività viene eseguita complicata dal fatto che ogni linguaggio definisce regole personalizzate per selezionare quali stringa da usare in base alla cardinalità.

Il pacchetto di localizzazione Orchard fornisce un'API per richiamare automaticamente queste diverse forme plurale.

### <a name="creating-pluralization-po-files"></a>La creazione di file di ordine di acquisto di pluralizzazione

Aggiungere il seguente contenuto indicati in precedenza *fr.po* file:

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

Vedere [cos'è un file di ordine di acquisto?](#what-is-a-po-file) per una spiegazione di ciò che rappresenta ogni voce in questo esempio.

### <a name="adding-a-language-using-different-pluralization-forms"></a>Aggiunta di una lingua utilizzando pluralizzazione diversi moduli

Le stringhe in lingua inglese e francese utilizzate nell'esempio precedente. Inglese e francese hanno solo due formati di pluralizzazione e condividere le stesse regole di maschera, che viene eseguito il mapping di una cardinalità di uno per la prima forma plurale. Viene eseguito il mapping di qualsiasi altra cardinalità per la seconda forma plurale.

Non tutti i linguaggi condividono le stesse regole. Come illustrato con ceco, che ha tre plurale.

Creare il `cs.po` file come indicato di seguito e prendere nota come la pluralizzazione richiede tre traduzioni diverse:

[!code-text[Main](localization/sample/POLocalization/cs.po)]

Per accettare processi cechi, aggiungere `"cs"` all'elenco delle lingue supportate nel `ConfigureServices` metodo:

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

Modificare il *Views/Home/About.cshtml* file per il rendering delle stringhe localizzate, plurale per cardinalità diversi:

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

**Nota:** In uno scenario reale, una variabile potrebbe essere utilizzata per rappresentare il conteggio. In questo caso, si ripete lo stesso codice con tre valori diversi per esporre un case specifico.

Quando si passa a impostazioni cultura, verrà visualizzato quanto segue:

Per `/Home/About`:

```html
There is one item.
There are 2 items.
There are 5 items.
```

Per `/Home/About?culture=fr`:

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

Per `/Home/About?culture=cs`:

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

Si noti che per le impostazioni cultura ceca, le tre traduzioni diversi. Le impostazioni cultura in inglese e francese condividono stessa costruzione per le due stringhe tradotte ultimo.

## <a name="advanced-tasks"></a>Attività avanzate

### <a name="contextualizing-strings"></a>Stringhe contextualizing

Le applicazioni spesso contengono le stringhe da convertire in diverse posizioni. La stessa stringa può avere una conversione diversa in determinati percorsi all'interno di un'app (visualizzazioni Razor o file di classe). Un file di ordine di acquisto supporta la nozione di un contesto di file, che può essere usato per suddividere la stringa viene rappresentata. Utilizzando il contesto di un file, una stringa può essere convertita in modo diverso, a seconda del contesto di file (o mancanza di un contesto di file).

Il nome della classe completa o la vista che viene utilizzata quando si converte una stringa di utilizzare i servizi di localizzazione di ordine di acquisto. Questa operazione viene eseguita impostando il valore per il `msgctxt` voce.

Si consideri una secondaria aggiunta a quelli *fr.po* esempio. Posizione di una visualizzazione Razor *Views/Home/About.cshtml* può essere definito come il contesto di file mediante l'impostazione riservato `msgctxt` valore della voce:

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

Con il `msgctxt` impostare di conseguenza, la conversione di testo si verifica durante il passaggio alla `/Home/About?culture=fr-FR`. La traduzione non si verifica durante la navigazione in `/Home/Contact?culture=fr-FR`.

Quando nessuna voce specifica sia associata a un contesto di file specificato, meccanismo di fallback del Core Orchard Cerca un file di ordine di acquisto appropriato senza un contesto. Presupponendo che non vi è alcun contesto di file specifico definito per *Views/Home/Contact.cshtml*, la navigazione a `/Home/Contact?culture=fr-FR` carica un file di ordine di acquisto, ad esempio:

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a>Modifica il percorso dei file di ordine di acquisto

Il percorso predefinito dei file di ordine di acquisto può essere modificato `ConfigureServices`:

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

In questo esempio, i file di ordine di acquisto vengono caricati dal *localizzazione* cartella.

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a>Implementazione di una logica personalizzata per la ricerca dei file di localizzazione

Quando è necessario individuare il file di ordine di acquisto, una logica più complessa la `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interfaccia può essere implementata e registrata come un servizio. Ciò risulta utile quando i file di ordine di acquisto possono essere archiviati in punti diversi oppure quando i file devono trovarsi all'interno di una gerarchia di cartelle.

### <a name="using-a-different-default-pluralized-language"></a>Utilizzando una lingua predefinita diversa pluralizzato

Il pacchetto include un `Plural` metodo di estensione che è specifico di due plurale. Per le lingue che richiedono ulteriori plurale, creare un metodo di estensione. Con un metodo di estensione, non sarà necessario fornire qualsiasi file di localizzazione per la lingua predefinita &mdash; stringhe originali sono già disponibili direttamente nel codice.

È possibile utilizzare il cmdlet più generico `Plural(int count, string[] pluralForms, params object[] arguments)` overload che accetta una matrice di stringhe di traduzioni.