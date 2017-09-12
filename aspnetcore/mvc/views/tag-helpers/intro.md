---
title: Helper di tag in ASP.NET Core
author: rick-anderson
description: Informazioni su cosa sono gli helper di tag e sul loro utilizzo in ASP.NET Core.
keywords: ASP.NET Core, gli helper di tag
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 53a31ed8ca6ff24a19a33a56c3a896aa58cbb62a
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a>Introduzione agli helper di Tag in ASP.NET Core 

Da [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>Quali sono gli helper di Tag?

Gli helper di tag consentono al codice lato server partecipare alla creazione e il rendering di elementi HTML in file Razor. Ad esempio, l'elemento predefinito `ImageTagHelper` possibile aggiungere un numero di versione per il nome dell'immagine. Ogni volta che cambia l'immagine, il server genera una nuova versione univoca per l'immagine, in modo i client sono garantiti per ottenere l'immagine corrente (anziché un'immagine memorizzati nella cache non aggiornata). Vi sono molti gli helper di Tag predefiniti per le attività comuni, ad esempio la creazione di moduli, collegamenti, asset durante il caricamento e più - e ancora più disponibili nel repository GitHub pubblici e come NuGet pacchetti. Vengono creati gli helper di tag in c# e di destinazione gli elementi HTML in base al nome di elemento, il nome dell'attributo o tag padre. Ad esempio, l'elemento predefinito `LabelTagHelper` possono avere come destinazione l'HTML `<label>` elemento quando il `LabelTagHelper` vengono applicati gli attributi. Se si ha familiarità con [helper HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), gli helper di Tag ridurre le esplicita transizioni tra HTML e c# nelle visualizzazioni Razor. In molti casi, l'helper HTML forniscono un'alternativa a un Helper Tag specifici, ma è importante tenere presente che gli helper di Tag non sostituiscono l'helper HTML e non esiste un Helper di Tag per ogni HTML Helper. [Gli helper confrontati con l'helper HTML tag](#tag-helpers-compared-to-html-helpers) vengono illustrate le differenze nel modo più dettagliato.

## <a name="what-tag-helpers-provide"></a>Cosa fornisce gli helper di Tag

**Un'esperienza di sviluppo orientato HTML** per nella maggior parte, markup Razor utilizzando l'helper di Tag è simile a HTML standard. Finestre di progettazione front-end familiarità con HTML/CSS/JavaScript è possibile modificare Razor senza conoscere la sintassi di c# Razor.

**Un ambiente completo di IntelliSense per la creazione di markup HTML e Razor** in contrasto per l'helper HTML, l'approccio precedente per la creazione di markup in visualizzazioni Razor sul lato server. [Gli helper confrontati con l'helper HTML tag](#tag-helpers-compared-to-html-helpers) vengono illustrate le differenze nel modo più dettagliato. [Il supporto IntelliSense per gli helper di Tag](#intellisense-support-for-tag-helpers) viene illustrato l'ambiente di IntelliSense. Anche gli sviluppatori esperti con sintassi Razor c# sono più produttivi usando gli helper di Tag rispetto alla scrittura di codice c# Razor.

**Un modo per verificare più produttivo e in grado di produrre più efficiente, affidabile e codice di facile manutenibilità con informazioni è disponibile solo nel server** , ad esempio, in passato è stato il motto sull'aggiornamento di immagini per modificare il nome dell'immagine quando si modifica l'immagine. Le immagini devono essere in modo aggressivo memorizzati nella cache per motivi di prestazioni e, a meno che non si modifica il nome di un'immagine, si rischia di client di ottenere una copia non aggiornata. In passato, dopo la modifica di un'immagine, il nome deve essere modificato e ogni riferimento a un'immagine nell'app web deve essere aggiornato. Non solo è molto lavoro che con utilizzo intensivo, è anche errori (potrebbe mancare un riferimento, accidentalmente immettere la stringa non corretto e così via.) L'elemento predefinito `ImageTagHelper` per questo scopo è automaticamente. Il `ImageTagHelper` possibile aggiungere una versione numero per il nome dell'immagine, pertanto ogni volta che cambia l'immagine, il server genera automaticamente una nuova versione univoca per l'immagine. I client sono garantiti per ottenere l'immagine corrente. Il risparmio di affidabilità e manodopera proviene essenzialmente disponibile tramite il `ImageTagHelper`.

La maggior parte degli helper Tag predefiniti come destinazione gli elementi HTML esistenti e fornire attributi sul lato server per l'elemento. Ad esempio, il `<input>` elemento utilizzato in molte delle visualizzazioni del *viste/Account* cartella contiene il `asp-for` attributo, che estrae il nome della proprietà del modello specificato in codice HTML visualizzabile. Il markup Razor seguente:

```html
<label asp-for="Email"></label>
```

Genera il codice HTML seguente:

```html
<label for="Email">Email</label>
```

Il `asp-for` attributo è reso disponibile per il `For` proprietà il `LabelTagHelper`. Vedere [creazione gli helper di Tag](authoring.md) per ulteriori informazioni.

## <a name="managing-tag-helper-scope"></a>La gestione di ambito Helper Tag

Ambito di helper di tag è controllata da una combinazione di `@addTagHelper`, `@removeTagHelper`e "!" rinuncia carattere.

<a name=add-helper-label></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper`rende disponibili gli helper di Tag

Se si crea una nuova app web ASP.NET Core denominata *AuthoringTagHelpers* (con nessuna autenticazione), le operazioni seguenti *Views/_ViewImports.cshtml* file verrà aggiunto al progetto:

[!code-html[Principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

Il `@addTagHelper` direttiva rende disponibile per la visualizzazione gli helper di Tag. In questo caso, il file della visualizzazione è *Views/_ViewImports.cshtml*, che per impostazione predefinita viene ereditata da tutti i file di visualizzazione di *viste* cartella e le sottodirectory; rendere disponibili gli helper di Tag. Il codice precedente utilizza la sintassi con caratteri jolly ("\*") per specificare che tutti gli helper di Tag nell'assembly specificato (*Microsoft.AspNetCore.Mvc.TagHelpers*) sarà disponibile per tutti i file nella visualizzazione di *viste* directory o sottodirectory. Il primo parametro dopo `@addTagHelper` specifica gli helper di Tag da caricare (usiamo "\*" per tutti gli helper di Tag), e il secondo parametro "Microsoft.AspNetCore.Mvc.TagHelpers" Specifica l'assembly contenente gli helper di Tag. *Microsoft.AspNetCore.Mvc.TagHelpers* è l'assembly per gli helper di Tag predefiniti di ASP.NET Core.

Per esporre tutti gli helper di Tag in questo progetto (che consente di creare un assembly denominato *AuthoringTagHelpers*), è necessario utilizzare la seguente:

[!code-html[Principale](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Se il progetto contiene un `EmailTagHelper` con spazio dei nomi predefinito (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), è possibile fornire il nome completo (FQN) dell'Helper di Tag:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3]}} -->

```html
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Per aggiungere un Helper Tag a una vista utilizzando un FQN, aggiungere innanzitutto i FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) e quindi il nome dell'assembly (*AuthoringTagHelpers*). La maggior parte degli sviluppatori preferiscono utilizzare il "\*" sintassi dei caratteri jolly. La sintassi con caratteri jolly consente di inserire il carattere jolly "\*" come suffisso in un FQN. Ad esempio, una delle seguenti direttive fornirà `EmailTagHelper`:

```csharp
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Come indicato in precedenza, l'aggiunta di `@addTagHelper` direttiva per il *Views/_ViewImports.cshtml* file rende disponibili a tutti i file di visualizzazione dell'Helper di Tag di *viste* directory e sottodirectory. È possibile utilizzare il `@addTagHelper` direttiva nei file di visualizzazione specifico se si desidera registrarsi per esporre l'Helper di Tag solo tali viste.

<a name=remove-razor-directives-label></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper`Rimuove gli helper di Tag

Il `@removeTagHelper` ha gli stessi due parametri di `@addTagHelper`, e viene rimosso un Helper Tag che è stato aggiunto in precedenza. Ad esempio, `@removeTagHelper` applicati per rimuovere una visualizzazione specifica l'Helper di Tag specificato dalla vista. Utilizzando `@removeTagHelper` in un *Views/Folder/_ViewImports.cshtml* file rimuove l'Helper di Tag da tutte le viste in *cartella*.

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>Controllo dell'ambito di Helper di Tag con il *viewimports. cshtml* file

È possibile aggiungere un *viewimports. cshtml* per qualsiasi cartella di visualizzazione e la visualizzazione motore applica le direttive da tale file di entrambi e *Views/_ViewImports.cshtml* file. Se è stato aggiunto un oggetto vuoto *Views/Home/_ViewImports.cshtml* file per il *Home* visualizzazioni, non vi sarà alcuna modifica perché il *viewimports. cshtml* file additivo. Qualsiasi `@addTagHelper` aggiungere le direttive di *Views/Home/_ViewImports.cshtml* file (che non sono presenti il valore predefinito *Views/_ViewImports.cshtml* file) consente di esporre tali gli helper di Tag alle viste solo nel *Home* cartella.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>Rifiuto esplicito singoli elementi

È possibile disabilitare un Helper Tag a livello di elemento con il carattere di rinuncia Helper di Tag ("!"). Ad esempio, `Email` convalida è disabilitata nel `<span>` con il carattere di rinuncia Helper di Tag:

```csharp
<!span asp-validation-for="Email" class="text-danger"></!span>
```

È necessario applicare il carattere di rinuncia Helper di Tag per il tag di apertura e chiusura. (Editor di Visual Studio aggiunge automaticamente il carattere di aderire al tag di chiusura quando si aggiunge uno al tag di apertura). Dopo aver aggiunto il carattere di rifiuto esplicito, l'elemento e gli attributi di Helper di Tag non sono più visualizzati in un unico tipo di carattere.

<a name=prefix-razor-directives-label></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>Utilizzando `@tagHelperPrefix` rendere esplicito l'utilizzo di Helper Tag

Il `@tagHelperPrefix` direttiva consente di specificare una stringa di prefisso di tag per abilitare il supporto di Helper di Tag e rendere esplicito l'utilizzo di Helper di Tag. Ad esempio, è possibile aggiungere il markup seguente per il *Views/_ViewImports.cshtml* file:

```html
@tagHelperPrefix th:
```
Nell'immagine di codice seguente, il prefisso di Tag Helper è impostato su `th:`, in modo che solo gli elementi utilizzando il prefisso `th:` supportano gli helper di Tag (abilitato Helper di Tag di elementi di un carattere distintivo). Il `<label>` e `<input>` elementi dispongono del prefisso dell'Helper di Tag e sono abilitati per il supporto di Tag, durante il `<span>` non elemento.

![immagine](intro/_static/thp.png)

Le stesse regole di gerarchia che si applicano a `@addTagHelper` si applicano anche alle `@tagHelperPrefix`.

## <a name="intellisense-support-for-tag-helpers"></a>Supporto IntelliSense per gli helper di Tag

Quando si crea una nuova app web ASP.NET in Visual Studio, aggiunge il pacchetto NuGet "Microsoft.AspNetCore.Razor.Tools". Questo è il pacchetto che aggiunge gli strumenti di supporto di Tag.

Si consiglia di scrivere l'HTML `<label>` elemento. Non appena immette `<l` nell'editor di Visual Studio, IntelliSense visualizza gli elementi corrispondenti:

![immagine](intro/_static/label.png)

Non solo ottenere della Guida HTML, ma l'icona (il "@" simbolo con "<>" sotto di esso).

![immagine](intro/_static/tagSym.png)

Identifica l'elemento come destinazione gli helper di Tag. Gli elementi HTML puri (ad esempio il `fieldset`) l'icona di "<>".

Un HTML puro `<label>` tag viene visualizzato il tag HTML (con il tema colori di Visual Studio predefinito) in un tipo di carattere marrone, gli attributi in rosso, e i valori di attributo in blu.

![immagine](intro/_static/LableHtmlTag.png)

Dopo avere immesso `<label`, IntelliSense Elenca gli attributi HTML/CSS disponibili e gli attributi di Helper di Tag di destinazione:

![immagine](intro/_static/labelattr.png)

Completamento delle istruzioni IntelliSense consente di immettere il tasto tab per completare l'istruzione con il valore selezionato:

![immagine](intro/_static/stmtcomplete.png)

Non appena viene immesso un attributo Helper di Tag, modificare i tipi di carattere tag e attributi. Usa il tema colori "Light" o predefinito Visual Studio "Blue", il carattere è in grassetto viola. Se si usa il tema "Scuro" il carattere è in grassetto verde acqua. Le immagini in questo documento sono state eseguite utilizzando il tema predefinito.

![immagine](intro/_static/labelaspfor2.png)

È possibile immettere Visual Studio *CompleteWord* scelta rapida (Ctrl + barra spaziatrice è il [predefinito](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) all'interno delle virgolette doppie (""), ed è ora in c#, esattamente come sarebbe in una classe c#. IntelliSense consente di visualizzare tutti i metodi e proprietà sul modello di pagina. I metodi e proprietà sono disponibili perché il tipo della proprietà è `ModelExpression`. Nell'immagine seguente, si modifica il `Register` vista, pertanto la `RegisterViewModel` è disponibile.

![immagine](intro/_static/intellemail.png)

IntelliSense Elenca le proprietà e i metodi disponibili per il modello nella pagina. L'ambiente completo di IntelliSense consente di selezionare la classe CSS:

![immagine](intro/_static/iclass.png)

![immagine](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>Helper di tag rispetto a helper HTML

Collegare gli helper di tag agli elementi HTML in visualizzazioni Razor, mentre [helper HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) vengono chiamati come metodi frammisto HTML in visualizzazioni Razor. Si consideri il seguente markup Razor, che consente di creare un'etichetta HTML con la classe CSS "titolo":

```html
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

L'in (`@`) simbolo indica Razor, si tratta dell'inizio del codice. I due parametri successivi ("FirstName" e "nome:") sono stringhe, pertanto [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) non consente di. L'ultimo argomento:

```html
new {@class="caption"}
```

Un oggetto anonimo viene utilizzato per rappresentare gli attributi. Poiché **classe** è una parola chiave riservata in c#, utilizzare il `@` simbolo per forzare in c# per interpretare "@class=" come simbolo (nome della proprietà). Per una finestra di progettazione front-end (un utente hanno dimestichezza con HTML/CSS/JavaScript e altre tecnologie client, ma non hanno familiarità con c# e Razor), la maggior parte della riga è esterno. L'intera riga deve essere creata con alcuna informazione della Guida da IntelliSense.

Utilizzo di `LabelTagHelper`, gli stessi tag possono essere scritti come:

![immagine](intro/_static/label2.png)

Con la versione dell'Helper di Tag, non appena immette `<l` nell'editor di Visual Studio, IntelliSense visualizza gli elementi corrispondenti:

![immagine](intro/_static/label.png)

IntelliSense consente di scrivere l'intera riga. Il `LabelTagHelper` anche per impostazione predefinita il contenuto dell'impostazione di `asp-for` valore ("FirstName") dell'attributo "Nome"; Proprietà maiuscole/minuscole camel converte in una frase composta dal nome di proprietà con uno spazio in cui si verifica ogni nuova lettera maiuscola. Nel codice seguente:

![immagine](intro/_static/label2.png)

Genera l'errore:

```html
<label class="caption" for="FirstName">First Name</label>
```

Le maiuscole/minuscole camel al contenuto in base alla frase non viene usata se si aggiunge contenuto per il `<label>`. Ad esempio:

![immagine](intro/_static/1stName.png)

Genera l'errore:

```html
<label class="caption" for="FirstName">Name First</label>
```

L'immagine di codice seguente mostra la parte di *Views/Account/Register.cshtml* visualizzazione Razor generata dal legacy ASP.NET 4.5.x MVC modello incluso con Visual Studio 2015.

![immagine](intro/_static/regCS.png)

Editor di Visual Studio consente di visualizzare il codice c# con uno sfondo grigio. Ad esempio, il `AntiForgeryToken` HTML Helper:

```html
@Html.AntiForgeryToken()
```

viene visualizzato con uno sfondo grigio. La maggior parte del markup nella visualizzazione registro è c#. Stabilire un confronto con l'approccio equivalente utilizzando l'helper di Tag:

![immagine](intro/_static/regTH.png)

Il markup è molto più semplice e più facile da leggere, modificare e gestire rispetto a quella di helper HTML. Il codice c# è ridotto al minimo, che il server deve conoscere. L'editor di Visual Studio Visualizza markup di destinazione di un Helper di Tag in un unico tipo di carattere.

Si consideri il *posta elettronica* gruppo:

[!code-csharp[Principale](intro/sample/Register.cshtml?range=12-18)]

Gli attributi "asp-" ha un valore di "Email" e "Email" non è una stringa. In questo contesto, "Email" è la proprietà expression di modello c# per il `RegisterViewModel`.

Editor di Visual Studio consente di scrivere **tutti** del markup nell'approccio Helper di Tag del modulo di registrazione, mentre Visual Studio non fornisce alcuna informazione della Guida per la maggior parte del codice nell'approccio helper HTML. [Il supporto IntelliSense per gli helper di Tag](#intellisense-support-for-tag-helpers) entra dettagli sull'utilizzo di helper di Tag nell'editor di Visual Studio.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Helper di tag rispetto ai controlli Server Web

* Gli helper di tag non proprietari dell'elemento che sono associate; fanno parte semplicemente il rendering dell'elemento e il contenuto. ASP.NET [controlli Server Web](https://msdn.microsoft.com/library/7698y1f0.aspx) vengono dichiarati e richiamato in una pagina.

* [Controlli Server Web](https://msdn.microsoft.com/library/zsyt68f1.aspx) hanno un ciclo di vita non semplice che può rendere difficile lo sviluppo e debug.

* I controlli Server Web consentono di aggiungere funzionalità per gli elementi del modello DOM (Document Object) client tramite un controllo client. Gli helper di tag non dispone di alcun DOM.

* Controlli Server Web includono rilevamento automatico del browser. Gli helper di tag non sono a conoscenza del browser.

* Gli helper di Tag più può fungere da sullo stesso elemento (vedere [conflitti evitando Helper Tag](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) mentre è in genere non è possibile comporre i controlli Server Web.

* Gli helper di tag è possono modificare il tag e il contenuto degli elementi HTML che sta ambiti, ma non modificare direttamente qualsiasi altra in una pagina. I controlli Server Web hanno un ambito specifico meno e possono eseguire azioni che influiscono su altre parti della pagina; abilitazione di effetti collaterali imprevisti.

* Controlli Server Web utilizzano i convertitori di tipi per convertire le stringhe in oggetti. Gli helper di Tag, si lavora in modo nativo in c#, pertanto non è necessario al tipo di conversione.

* Utilizzare i controlli Server Web [System. ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) per implementare il comportamento in fase di esecuzione e in fase di progettazione di componenti e controlli. `System.ComponentModel`include le classi base e interfacce per l'implementazione di convertitori di tipi e gli attributi, associazioni a origini dati e componenti della licenza. Invece che per gli helper di Tag, che in genere derivano da `TagHelper`e `TagHelper` classe di base espone due metodi, `Process` e `ProcessAsync`.

## <a name="customizing-the-tag-helper-element-font"></a>Il tipo di carattere elemento Helper di Tag di personalizzazione

È possibile personalizzare il tipo di carattere e la colorazione da **strumenti** > **opzioni** > **ambiente** > **tipi di carattere e i colori**:

![immagine](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Creazione e modifica gli helper di Tag](authoring.md)
* [Utilizzo dei moduli](../working-with-forms.md)
* [TagHelperSamples su GitHub](https://github.com/dpaquette/TagHelperSamples) sono inclusi esempi di Helper di Tag per l'utilizzo di [Bootstrap](http://getbootstrap.com/).

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
