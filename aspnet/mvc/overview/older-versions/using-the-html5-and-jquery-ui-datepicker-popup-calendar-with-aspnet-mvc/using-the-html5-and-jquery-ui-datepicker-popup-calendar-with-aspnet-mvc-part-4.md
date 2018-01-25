---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Utilizzo di HTML5 e jQuery UI Datepicker Popup Calendar with ASP.NET MVC - parte 4 | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione verrà fornite le nozioni di base lavorare con modelli di editor, modelli di visualizzazione e il calendario di jQuery UI datepicker popup in una macchina virtuale di ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 32211465adeb1353908daa1014d188b84389e1a7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Utilizzo di HTML5 e jQuery UI Datepicker Popup Calendar with ASP.NET MVC - parte 4
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base lavorare con modelli di editor, modelli di visualizzazione e il calendario di jQuery UI datepicker popup in un'applicazione Web ASP.NET MVC.


### <a name="adding-a-template-for-editing-dates"></a>Aggiunta di un modello per la modifica di date

In questa sezione si creerà un modello per la modifica di date che verranno applicate quando ASP.NET MVC Visualizza un'interfaccia utente per la modifica delle proprietà del modello che sono contrassegnate con il **data** enumerazione del [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo. Il modello verrà eseguito il rendering solo alla data. ora non verrà visualizzato. Nel modello si userà il [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) calendario popup per fornire un modo per modificare le date.

Per iniziare, aprire il *Movie.cs* file e aggiungere il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo con il **data** enumerazione per la `ReleaseDate` proprietà, come illustrato nel codice seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Questo codice causa il `ReleaseDate` campo da visualizzare senza l'ora in entrambi i modelli visualizzati e modificare i modelli. Se l'applicazione contiene un *date.cshtml* modello nel *disponibile* cartella o il *Views\Movies\EditorTemplates* cartella, tale modello verrà utilizzato per il rendering di qualsiasi `DateTime` proprietà durante la modifica. In caso contrario il sistema di modelli ASP.NET predefinito visualizzerà la proprietà come Data.

Premere CTRL+F5 per eseguire l'applicazione. Selezionare un collegamento per verificare che il campo di input per la data di rilascio viene visualizzata solo la data di modifica.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

In **Esplora soluzioni**, espandere il *viste* cartella, espandere il *Shared* cartella e quindi fare di *disponibile* cartella.

Fare clic su **Aggiungi**, quindi fare clic su **vista**. Il **Aggiungi visualizzazione** viene visualizzata la finestra di dialogo.

Nel **nome vista** digitare &quot;data&quot;.

Selezionare il **Crea come visualizzazione parziale** casella di controllo. Assicurarsi che il **utilizza una layout o pagina master** e **creare una visualizzazione fortemente tipizzata** caselle di controllo non vengono selezionate.

Fare clic su **Aggiungi**. Il *Views\Shared\EditorTemplates\Date.cshtml* modello viene creato.

Aggiungere il codice seguente per il *Views\Shared\EditorTemplates\Date.cshtml* modello.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

La prima riga viene dichiarato il modello da un `DateTime` tipo. Anche se non è necessario dichiarare il tipo di modello in modalità di modifica e visualizzare i modelli, è buona norma in modo che sia possibile ottenere in fase di compilazione del modello passato alla visualizzazione il controllo. (Un altro vantaggio è quindi ottenere IntelliSense per il modello di visualizzazione in Visual Studio). Se non è dichiarato il tipo di modello, ASP.NET MVC considera un [dinamica](https://msdn.microsoft.com/library/dd264741.aspx) digitare e non sono previsti tempi di compilazione il controllo dei tipi. Se si dichiara il modello da un `DateTime` tipo, diventa fortemente tipizzato.

La seconda riga viene semplicemente letterale markup HTML che visualizza &quot;utilizzando il modello di data&quot; prima di un campo della data. Si utilizzerà questa riga temporaneamente per verificare che venga utilizzato il modello di Data.

La riga successiva è un [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) helper che esegue il rendering di un `input` campo di una casella di testo. Il terzo parametro per il supporto utilizza un tipo anonimo per impostare la classe per la casella di testo `datefield` e il tipo da `date`. (Poiché `class` è riservata in c#, è necessario utilizzare il `@` carattere di escape di `class` attributo nel parser di c#.)

Il `date` tipo è un tipo di input HTML5 che consente ai browser compatibile con HTML5 eseguire il rendering di un controllo di calendario HTML5. In un secondo momento si aggiungeranno alcuni JavaScript il datepicker jQuery per collegare il `Html.TextBox` elemento utilizzando la `datefield` classe.

Premere CTRL+F5 per eseguire l'applicazione. È possibile verificare che il `ReleaseDate` utilizza il modello di modifica perché il modello viene visualizzato nella visualizzazione di modifica proprietà &quot;utilizzando il modello di data&quot; immediatamente prima di `ReleaseDate` input di testo casella, come illustrato in questa immagine:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Nel browser, visualizzare l'origine della pagina. (Ad esempio, la pagina e scegliere **Visualizza origine**.) Il seguente esempio vengono illustrate alcune il markup della pagina, che illustra il `class` e `type` attributi nell'HTML.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Restituito per il *Views\Shared\EditorTemplates\Date.cshtml* modello e rimuovere il &quot;utilizzando il modello di data&quot; markup. Il modello completato ora simile al seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Aggiunta di un jQuery UI Datepicker Popup Calendar tramite NuGet

In questa sezione si aggiungerà il [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendario popup per il modello di Data modifica. Il [jQuery UI](http://jqueryui.com/) libreria fornisce il supporto per l'animazione, advanced effetti e widget personalizzabili. Viene creato all'inizio la libreria JavaScript jQuery. Il calendario popup datepicker rende semplice e naturale di immettere le date utilizzando un calendario anziché immettere una stringa. Il calendario popup limita inoltre agli utenti di date valide, ovvero testo normale per una data consente di immettere un valore come `2/33/1999` (febbraio 33rd, 1999), ma la [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendario popup non consentirà di.

In primo luogo, è necessario installare le librerie di jQuery UI. A tale scopo, si userà NuGet, è una gestione di pacchetti che è incluso nelle versioni SP1 di Visual Studio 2010 e Visual Web Developer.

In Visual Web Developer dal **strumenti** dal menu **Gestione pacchetti libreria** e quindi selezionare **Gestisci pacchetti NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Nota: Se il **strumenti** menu non viene visualizzato il **Gestione pacchetti libreria** comando, è necessario installare NuGet seguendo le istruzioni nella [installazione di NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) pagina di il sito Web NuGet.   
  
Se si utilizza Visual Studio anziché Visual Web Developer dal **strumenti** dal menu **Gestione pacchetti libreria** e quindi selezionare **Aggiungi riferimento al pacchetto di librerie**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

Nel **MVCMovie - Gestisci pacchetti NuGet** la finestra di dialogo, fare clic su di **Online** scheda a sinistra e quindi immettere &quot;jQuery.UI&quot; nella casella di ricerca. Selezionare j **widget dell'interfaccia utente di Query: Datepicker**, quindi selezionare il **installare** pulsante.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet aggiunge queste versioni di debug e minimizzata di jQuery UI Core e selezione data jQuery UI al progetto:

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

Nota: Nelle versioni di debug (i file senza il *. min.js* estensione) sono utili per il debug, ma in un sito di produzione, è necessario includere solo le versioni minimizzate.

Per utilizzare effettivamente il controllo selezione data jQuery, è necessario creare uno script jQuery che verrà associato il widget di calendario per il modello di modifica. In **Esplora**, fare doppio clic su di *script* cartella e selezionare **Aggiungi**, quindi **nuovo elemento**e quindi **JScript File**. Nome del file *DatePickerReady.js*.

Aggiungere il codice seguente per il *DatePickerReady.js* file:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Se non si ha familiarità con jQuery, ecco una breve spiegazione di questa operazione: la prima riga è il &quot;jQuery pronto&quot; funzione, viene chiamato quando tutti gli elementi DOM in una pagina è sono caricata. La seconda riga Seleziona tutti gli elementi DOM con il nome della classe `datefield`, quindi richiama il `datepicker` funzione per ognuno di essi. (Tenere presente che è stato aggiunto il `datefield` classe per il *Views\Shared\EditorTemplates\Date.cshtml* modello precedenza nell'esercitazione.)

Aprire quindi il *Views\Shared\\layout. cshtml* file. È necessario aggiungere riferimenti ai seguenti file sono necessari in modo che è possibile utilizzare il controllo selezione data:

- *Content/Themes/base/jQuery.UI.Core.CSS*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/Themes/base/jQuery.UI.theme.CSS*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

Nell'esempio seguente viene illustrato il codice effettivo che è necessario aggiungere nella parte inferiore del `head` elemento il *Views\Shared\\layout. cshtml* file.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

L'intero `head` sezione è illustrata di seguito:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

Il [helper dell'URL contenuto](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) metodo converte il percorso della risorsa in un percorso assoluto. È necessario utilizzare `@URL.Content` correttamente fare riferimento a queste risorse quando l'applicazione è in esecuzione in IIS.

Premere CTRL+F5 per eseguire l'applicazione. Selezionare un collegamento di modifica, quindi inserire il punto di inserimento di **ReleaseDate** campo. Viene visualizzato il calendario popup di jQuery UI.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Come la maggior parte dei controlli di jQuery, la datepicker consente di personalizzare in modo esteso. Per informazioni, vedere [personalizzazione Visual: progettazione di un tema dell'interfaccia utente jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) sul [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) sito.

### <a name="supporting-the-html5-date-input-control"></a>Supporta il controllo di Input data HTML5

Supporto per più browser HTML5, è opportuno utilizzare il nativo HTML5 input, ad esempio il `date` elemento di input e non utilizzare il calendario di jQuery UI. È possibile aggiungere logica per l'applicazione per utilizzare automaticamente dei controlli HTML5 se il browser li supporta. A tale scopo, sostituire il contenuto del *DatePickerReady.js* file con le operazioni seguenti:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

La prima riga di questo script utilizza Modernizr per verificare che l'input data HTML5 è supportato. Se non è supportata, è associato il controllo selezione data di jQuery UI. ([Modernizr](http://www.modernizr.com/docs/) è una libreria JavaScript open source che rileva la disponibilità di implementazioni native di HTML5 e CSS3. Modernizr è incluso nei nuovi progetti ASP.NET MVC creata.)

Dopo aver apportato questa modifica, è possibile eseguirne il test utilizzando un browser che supporta HTML5, ad esempio 11 Opera. Eseguire l'applicazione tramite un browser compatibile con HTML5 e modificare una voce di film. Il controllo Data HTML5 viene utilizzato anziché il calendario popup dell'interfaccia utente di jQuery:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Poiché le nuove versioni di browser implementa HTML5 in modo incrementale, un approccio valido per il momento consiste nell'aggiungere codice per il sito Web che supporta un'ampia gamma di supporto per HTML5. Ad esempio, una più solida *DatePickerReady.js* script è illustrato di seguito che consente di browser di supporto del sito che supportano solo parzialmente il controllo Data HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Questo script consente di selezionare HTML5 `input` gli elementi di tipo `date` che non supportano completamente il controllo Data HTML5. Per tali elementi, Aggancia calendario popup jQuery UI e quindi passa il `type` dall'attributo `date` a `text`. Modificando il `type` dall'attributo `date` a `text`, supporto delle date HTML5 parziale viene eliminato. Un ancora più potente *DatePickerReady.js* script è reperibile in [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Aggiunta di date che ammette valori null per i modelli

Se si utilizza uno dei modelli di data esistente e passa a una data null, si otterrà un errore di run-time. Per rendere più affidabile i modelli di data, sarà necessario sostituire in modo da gestire valori null. Per supportare le date che ammette valori null, modificare il codice di *Views\Shared\DisplayTemplates\DateTime.cshtml* al seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Il codice restituisce una stringa vuota se il modello è **null**.

Modificare il codice di *Views\Shared\EditorTemplates\Date.cshtml* file per le operazioni seguenti:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Quando questo codice viene eseguito, se il modello non è null, il modello `DateTime` valore viene utilizzato. Se il modello è null, viene invece utilizzata la data corrente.

### <a name="wrapup"></a>Riepilogo

In questa esercitazione è illustrati i concetti fondamentali dell'helper basati su modelli ASP.NET e viene illustrato come utilizzare il calendario di jQuery UI datepicker popup in un'applicazione MVC ASP.NET. Per ulteriori informazioni, provare a queste risorse:

- Per informazioni sulla localizzazione, vedere il blog del Rajeesh [JQueryUI Datepicker in ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Per informazioni su jQuery UI, vedere [jQuery UI](http://docs.jquery.com/UI).
- Per informazioni su come localizzare il controllo datepicker, vedere [UI Datepicker/la/localizzazione](http://docs.jquery.com/UI/Datepicker/Localization).
- Per ulteriori informazioni sui modelli di MVC ASP.NET, vedere serie di blog di Brad Wilson su [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Anche se la serie è stato scritto per ASP.NET MVC 2, il materiale ancora valide per la versione corrente di ASP.NET MVC.

>[!div class="step-by-step"]
[Precedente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
