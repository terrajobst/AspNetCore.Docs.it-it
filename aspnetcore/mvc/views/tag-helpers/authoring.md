---
title: Creazione e modifica gli helper di Tag in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare gli helper di Tag in ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 01/19/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9aaf40377e07e53fd0b7ebb177bcbb2df52b7553
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="author-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a>Helper di Tag autore in ASP.NET di base, una procedura dettagliata con esempi

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>Iniziare a utilizzare gli helper di Tag

In questa esercitazione fornisce un'introduzione alla programmazione gli helper di Tag. [Introduzione agli helper di Tag](intro.md) vengono descritti i vantaggi che forniscono gli helper di Tag.

Un helper tag è qualsiasi classe che implementa il `ITagHelper` interfaccia. Tuttavia, quando si crea un helper tag, in genere deriva da `TagHelper`, pertanto consente di accedere a questa il `Process` metodo.

1. Creare un nuovo progetto ASP.NET Core denominato **AuthoringTagHelpers**. Autenticazione non è necessario per questo progetto.

2. Creare una cartella per contenere gli helper di Tag chiamato *TagHelpers*. Il *TagHelpers* cartella *non* obbligatorio, ma è una convenzione ragionevole. Ora è possibile iniziare subito la scrittura di un helper tag semplice.

## <a name="a-minimal-tag-helper"></a>Un Helper Tag minima

In questa sezione, scrivere un helper tag che aggiorna un tag di posta elettronica. Ad esempio:

```html
<email>Support</email>
   ```

Il server utilizzerà l'helper di tag di posta elettronica per convertire il markup in quanto segue:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

Vale a dire un tag di ancoraggio che è un collegamento di posta elettronica. È possibile eseguire questa operazione se si sta scrivendo un motore di blog ed è necessario per inviare posta elettronica di marketing, supporto e altri contatti, tutti allo stesso dominio.

1.  Aggiungere il seguente `EmailTagHelper` classe per il *TagHelpers* cartella.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    **Note:**
    
    * Gli helper di tag utilizzano una convenzione di denominazione che ha come destinazione gli elementi del nome della classe radice (meno di *helper tag* parte del nome della classe). In questo esempio, il nome radice del **posta elettronica**helper tag è *posta elettronica*, pertanto il `<email>` saranno destinati i tag. Questa convenzione di denominazione dovrebbe funzionare per la maggior parte degli helper di tag, in seguito verrà illustrato come eseguire l'override.
    
    * La classe `EmailTagHelper` deriva da `TagHelper`. La `TagHelper` classe fornisce metodi e proprietà per la scrittura gli helper di Tag.
    
    * Sottoposto a override `Process` metodo controlla l'helper di tag cosa quando eseguita. Il `TagHelper` classe fornisce anche una versione asincrona (`ProcessAsync`) con gli stessi parametri.
    
    * Il parametro di contesto per `Process` (e `ProcessAsync`) contiene informazioni associate all'esecuzione del tag HTML corrente.
    
    * Il parametro di output per `Process` (e `ProcessAsync`) contiene un elemento HTML con stato rappresentativo di origine originale utilizzato per generare un tag HTML e il contenuto.
    
    * Il nome della classe è un suffisso di **helper tag**, ovvero *non* obbligatorio, ma è considerata una convenzione prassi migliori. È possibile dichiarare la classe come:
    
    ```csharp
    public class Email : TagHelper
    ```

2.  Per rendere il `EmailTagHelper` classe disponibile per tutti i nostri visualizzazioni Razor, aggiungere il `addTagHelper` direttiva per il *Views/_ViewImports.cshtml* file:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
    Il codice precedente Usa la sintassi con caratteri jolly per specificare che tutti gli helper di tag in questo assembly saranno disponibili. La prima stringa dopo `@addTagHelper` specifica l'helper di tag per caricare (usare "*" per tutti gli helper di tag), e la seconda stringa "AuthoringTagHelpers" Specifica l'assembly è l'helper di tag. Si noti inoltre che la seconda riga riporta gli helper di tag di ASP.NET MVC di base utilizzando la sintassi con caratteri jolly (tali helper vengono discussi in [introduzione per gli helper di Tag](intro.md).) È il `@addTagHelper` direttiva che rende disponibili per la visualizzazione Razor l'helper di tag. In alternativa, è possibile fornire il nome completo (FQN) di un helper di tag, come illustrato di seguito:
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
Per aggiungere un helper tag a una vista utilizzando un FQN, aggiungere innanzitutto i FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) e quindi il nome dell'assembly (*AuthoringTagHelpers*). La maggior parte degli sviluppatori preferiranno utilizzare la sintassi con caratteri jolly. [Introduzione agli helper di Tag](intro.md) entra in dettaglio nella sintassi di aggiunta, rimozione, gerarchia e caratteri jolly di supporto dei tag.
    
3.  Aggiornare il markup di *Views/Home/Contact.cshtml* file con queste modifiche:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  Eseguire l'app e utilizzare il browser preferito per visualizzare l'origine HTML, per poter verificare che i tag di posta elettronica vengono sostituiti con il tag di ancoraggio (ad esempio, `<a>Support</a>`). *Supporto* e *Marketing* il rendering viene eseguito come collegamenti, ma che non dispongono di un `href` attributo in modo funzionale. Che verrà risolto nella sezione successiva.

Nota: Come tag HTML e gli attributi, tag, i nomi delle classi e attributi in Razor e c# non sono rilevanti.

## <a name="setattribute-and-setcontent"></a>SetAttribute e SetContent

In questa sezione verrà aggiornata la `EmailTagHelper` in modo che verrà creato un tag di ancoraggio valido per la posta elettronica. Verrà aggiornata in modo da richiedere informazioni da una visualizzazione Razor (sotto forma di un `mail-to` attributo) e utilizzarlo per generare il punto di ancoraggio.

Aggiornamento di `EmailTagHelper` classe con le operazioni seguenti:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**Note:**

* Base alla convezione Pascal proprietà nomi di classe e per gli helper di tag vengono convertiti in loro [inferiore case kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Pertanto, utilizzare il `MailTo` attributo, si userà `<email mail-to="value"/>` equivalente.

* L'ultima riga imposta il contenuto completato per l'helper di tag minima funzionale.

* La riga evidenziata mostra la sintassi per l'aggiunta di attributi:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Questo approccio funziona per l'attributo "href" fino a quando non esiste nella raccolta di attributi. È inoltre possibile utilizzare il `output.Attributes.Add` metodo per aggiungere un attributo di helper di tag di fine della raccolta di attributi di tag.

1.  Aggiornare il markup di *Views/Home/Contact.cshtml* file con queste modifiche:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2.  Eseguire l'applicazione e verificare che genera collegamenti corretti.
    
    > [!NOTE]
    >Se si volesse scrivere il messaggio di posta elettronica tag chiusura automatica (`<email mail-to="Rick" />`), l'output finale potrebbe anche essere a chiusura automatica. Per abilitare la possibilità di scrivere il tag con un tag di inizio (`<email mail-to="Rick">`) è necessario contrassegnare la classe con le operazioni seguenti:
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    Con un helper di tag di posta elettronica chiusura automatica, l'output sarà `<a href="mailto:Rick@contoso.com" />`. Chiusura automatica di tag di ancoraggio non sono validi HTML, in modo che non si desidera crearne uno, ma si potrebbe voler creare un helper di tag di chiusura automatica. Gli helper di tag impostato il tipo di `TagMode` proprietà dopo la lettura di un tag.
    
### <a name="processasync"></a>ProcessAsync

In questa sezione, è necessario scrivere un helper del messaggio di posta elettronica asincrona.

1.  Sostituire il `EmailTagHelper` classe con il codice seguente:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    **Note:**

    * Questa versione utilizza asincrona `ProcessAsync` metodo. Asincrona `GetChildContentAsync` restituisce un `Task` contenente il `TagHelperContent`.

    * Utilizzare il `output` parametro per ottenere il contenuto dell'elemento HTML.

2.  Apportare le seguenti modifiche per il *Views/Home/Contact.cshtml* file pertanto l'helper di tag è possibile recuperare il messaggio di posta elettronica di destinazione.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  Eseguire l'applicazione e verificare che genera collegamenti di posta elettronica valido.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent.SetHtmlContent e PostContent.SetHtmlContent

1.  Aggiungere il seguente `BoldTagHelper` classe per il *TagHelpers* cartella.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    **Note:**
    
    * Il `[HtmlTargetElement]` attributo passa un parametro di attributo che specifica che qualsiasi elemento HTML che contiene un attributo HTML denominato "bold" corrisponderanno, e `Process` verrà eseguito il metodo di override nella classe. In questo esempio, il `Process` metodo rimuove l'attributo "bold" e racchiude contenente markup con `<strong></strong>`.
    
    * Perché non si desidera sostituire il contenuto del tag esistente, è necessario scrivere l'apertura `<strong>` tag con il `PreContent.SetHtmlContent` metodo e la chiusura `</strong>` tag con il `PostContent.SetHtmlContent` metodo.
    
2.  Modificare il *About.cshtml* vista per contenere un `bold` valore dell'attributo. Il codice completo è illustrato di seguito.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  Eseguire l'app. È possibile utilizzare il browser preferito per controllare l'origine e verificare il markup.

    Il `[HtmlTargetElement]` attributo destinata solo il markup HTML che fornisce un nome di attributo di "bold". Il `<bold>` elemento non è stato modificato per l'helper di tag.

4. Impostare come commento il `[HtmlTargetElement]` riga dell'attributo e si utilizzerà come destinazione `<bold>` tag, vale a dire i tag HTML nel formato `<bold>`. Tenere presente che la convenzione di denominazione predefinito corrisponderà al nome di classe **grassetto**helper tag per `<bold>` tag.

5. Eseguire l'applicazione e verificare che il `<bold>` tag viene elaborata l'helper di tag.

La decorazione di una classe con più `[HtmlTargetElement]` attributi risulta un OR logico delle destinazioni. Ad esempio, tramite il codice riportato di seguito, un tag in grassetto o un attributo bold corrisponderanno.

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Quando più attributi vengono aggiunti alla stessa istruzione, il runtime li gestisce come un AND logico. Ad esempio, il codice riportato di seguito, un elemento HTML deve essere denominato "bold" con un attributo denominato "bold" (`<bold bold />`) per trovare la corrispondenza.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

È inoltre possibile utilizzare il `[HtmlTargetElement]` per modificare il nome dell'elemento di destinazione. Ad esempio se si desidera che il `BoldTagHelper` alla destinazione `<MyBold>` tag, utilizzare l'attributo seguente:

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a>Passare a un modello a un Helper Tag

1.  Aggiungere un *modelli* cartella.

2.  Aggiungere la classe `WebsiteContext` seguente alla cartella *Models*:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  Aggiungere il seguente `WebsiteInformationTagHelper` classe per il *TagHelpers* cartella.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    **Note:**
    
    * Come accennato in precedenza, gli helper di tag converte i nomi delle classi in base alla convezione Pascal c# e le proprietà per gli helper di tag in [inferiore case kebab](http://wiki.c2.com/?KebabCase). Pertanto, utilizzare il `WebsiteInformationTagHelper` in Razor, verrà scritto `<website-information />`.
    
    * Non si desidera identificare in modo esplicito l'elemento di destinazione con il `[HtmlTargetElement]` attributo, pertanto il valore predefinito di `website-information` saranno destinati. Se è stato applicato l'attributo seguente (notare non case kebab ma corrisponde al nome di classe):
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    Il tag del case kebab inferiore `<website-information />` non corrispondono. Se si desidera utilizzare il `[HtmlTargetElement]` attributo, utilizzare case kebab come illustrato di seguito:
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * Gli elementi che sono a chiusura automatica non abbiano contenuto. Per questo esempio, il markup Razor utilizzerà un tag di chiusura automatica, ma l'helper di tag creeranno un [sezione](http://www.w3.org/TR/html5/sections.html#the-section-element) elemento (che non è chiusura automatica e si scrive il contenuto all'interno di `section` elemento). Pertanto, è necessario impostare `TagMode` a `StartTagAndEndTag` per scrivere l'output. In alternativa, è possibile impostare come commento la riga che imposta `TagMode` e la scrittura di markup con un tag di chiusura. (Più avanti in questa esercitazione viene fornito codice di esempio).
    
    * Il `$` (segno di dollaro) nella riga seguente viene utilizzato un [interpolati stringa](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  Aggiungere il markup seguente per il *About.cshtml* visualizzazione. Il markup evidenziato Visualizza le informazioni sul sito web.
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > Nel codice Razor illustrato di seguito:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    >Razor conosca il `info` attributo è una classe, non è una stringa, e si desidera scrivere codice c#. Qualsiasi attributo helper di tag non di tipo stringa deve essere scritta senza la `@` carattere.
    
5.  Eseguire l'app e passare alla visualizzazione di informazioni su per visualizzare le informazioni sul sito web.

    >[!NOTE]
    >È possibile utilizzare il markup seguente con un tag di chiusura e rimuovere la riga con `TagMode.StartTagAndEndTag` nell'helper tag:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>Helper di Tag di condizione

L'helper di tag di condizione esegue il rendering di output quando viene passato un valore true.

1.  Aggiungere il seguente `ConditionTagHelper` classe per il *TagHelpers* cartella.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  Sostituire il contenuto del *Views/Home/Index.cshtml* file con il markup seguente:

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  Sostituire il `Index` metodo il `Home` controller con il codice seguente:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  Eseguire l'app e passare alla home page. Il markup in condizionale `div` non vengono visualizzati. Aggiungere la stringa di query `?approved=true` all'URL (ad esempio, `http://localhost:1235/Home/Index?approved=true`). `approved`è impostato su true e condizionale markup verrà visualizzato.

>[!NOTE]
>Utilizzare il [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operatore per specificare l'attributo di destinazione, anziché specificare una stringa, come avviene per l'helper di tag in grassetto:
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
>Il [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operatore proteggerà il codice necessario sempre il refactoring (potrebbe essere necessario modificare il nome in `RedCondition`).

### <a name="avoid-tag-helper-conflicts"></a>Evitare conflitti di Helper Tag

In questa sezione, scrivere una coppia degli helper di tag di collegamento automatico. Il primo sostituirà markup contenente un URL che inizia con HTTP in un HTML ancoraggio tag che contiene lo stesso URL (e generando un collegamento all'URL). Il secondo sarà eseguire la stessa operazione per un URL a partire da WWW.

Poiché questi due helper sono strettamente correlati e potrebbero refactoring in futuro, si sarà mantenerli nello stesso file.

1.  Aggiungere il seguente `AutoLinkerHttpTagHelper` classe per il *TagHelpers* cartella.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    >Il `AutoLinkerHttpTagHelper` classe destinazioni `p` elementi e viene utilizzato [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) per creare il punto di ancoraggio.

2.  Aggiungere il markup seguente alla fine del *Views/Home/Contact.cshtml* file:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  Eseguire l'app e verificare che l'helper di tag visualizzi correttamente il punto di ancoraggio.

4.  Aggiornamento di `AutoLinker` classe per includere il `AutoLinkerWwwTagHelper` che convertirà www testo in un tag di ancoraggio che contiene anche il testo www originale. Il codice aggiornato è evidenziato di seguito:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  Eseguire l'app. Si noti il testo www viene visualizzato come collegamento ma non è il testo HTTP. Se si inserisce un punto di interruzione in entrambe le classi, si noterà che la classe helper di tag HTTP viene eseguito per prima. Il problema è che viene memorizzato nella cache l'output di helper di tag e, quando viene eseguito l'helper di tag WWW, sovrascrive l'output memorizzato nella cache l'helper di tag HTTP. Più avanti nell'esercitazione vedremo come controllare l'ordine in cui eseguire gli helper di tag in. Il codice, è possibile risolvere con gli elementi seguenti:

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    >Nell'edizione prima degli helper di tag di collegamento automatico, è stato ottenuto il contenuto di destinazione con il codice seguente:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    >Ovvero, si chiama `GetChildContentAsync` utilizzando il `TagHelperOutput` passato il `ProcessAsync` (metodo). Come accennato in precedenza, perché l'output viene memorizzato nella cache, l'ultimo tag helper per l'esecuzione di wins. È stato risolto il problema con il codice seguente:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    >Il codice precedente verifica se il contenuto è stato modificato e, in caso affermativo, ottiene il contenuto del buffer di output.

6.  Eseguire l'applicazione e verificare che i due collegamenti funzionino come previsto. Sebbene potrebbe essere visualizzato che l'helper di tag del linker automatico sia corretto e completo, con un problema complesso. Se l'helper di tag WWW viene eseguito per primo, i collegamenti www non saranno corretti. Aggiornare il codice aggiungendo i `Order` overload per controllare l'ordine in cui viene eseguito il tag in. Il `Order` proprietà determina l'ordine di esecuzione rispetto alla altri helper di tag come destinazione lo stesso elemento. Il valore dell'ordine predefinito è zero e le istanze con i valori più bassi vengono eseguite per prime.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    Il codice sopra riportato garantisce che l'helper di tag HTTP viene eseguito prima dell'helper di tag WWW. Modifica `Order` a `MaxValue` e verificare che il markup generato per il tag WWW non è corretto.

## <a name="inspect-and-retrieve-child-content"></a>Controllare e recuperare il contenuto figlio

Gli helper di tag forniscono diverse proprietà per recuperare il contenuto.

-  Il risultato di `GetChildContentAsync` può essere aggiunta alla fine `output.Content`.
-  È possibile controllare il risultato di `GetChildContentAsync` con `GetContent`.
-  Se si modifica `output.Content`, non verrà eseguito o visualizzato a meno che si chiama il corpo di helper tag `GetChildContentAsync` come esempio automatica linker:

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  Più chiamate al metodo `GetChildContentAsync` restituirà lo stesso valore e non verrà eseguita nuovamente la `TagHelper` corpo a meno che non viene passato un parametro false che indica di non utilizzare il risultato memorizzato nella cache.
