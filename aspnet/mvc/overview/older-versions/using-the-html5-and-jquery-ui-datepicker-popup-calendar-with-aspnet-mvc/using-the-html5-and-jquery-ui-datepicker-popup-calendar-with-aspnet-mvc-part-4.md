---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Uso di HTML5 e jQuery UI Datepicker Popup Calendar con ASP.NET MVC - parte 4 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base di come usare i modelli nell'editor di modelli di visualizzazione e il calendario jQuery UI datepicker popup in MV un ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6768472b0c75757c9f368cfea58d5084c26719e1
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913164"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Uso di HTML5 e jQuery UI Datepicker Popup Calendar con ASP.NET MVC - parte 4
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Questa esercitazione insegnerà le nozioni di base di come usare i modelli nell'editor di modelli di visualizzazione e il calendario di jQuery UI datepicker popup in un'applicazione Web ASP.NET MVC.


### <a name="adding-a-template-for-editing-dates"></a>Aggiunta di un modello per la modifica di date

In questa sezione si creerà un modello per la modifica di date che verranno applicate quando ASP.NET MVC Visualizza l'interfaccia utente per la modifica delle proprietà del modello che sono contrassegnate con il **data** enumerazione del [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo. Il modello verrà eseguito il rendering solo alla data. ora non verrà visualizzata. Nel modello si userà il [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) calendario popup per fornire un modo per modificare le date.

Per iniziare, aprire il *Movie.cs* file e aggiungere il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo con il **data** enumerazione per il `ReleaseDate` proprietà, come illustrato nel codice seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Questo codice causa il `ReleaseDate` campo da visualizzare senza l'ora in entrambi visualizzare i modelli e modificare i modelli. Se l'applicazione contiene un *date.cshtml* modello nel *Views\Shared\EditorTemplates* cartella o nel *Views\Movies\EditorTemplates* cartella, tale modello verrà usato per il rendering di qualsiasi `DateTime` proprietà durante la modifica. In caso contrario, il sistema di creazione modello ASP.NET predefinito visualizzerà la proprietà come Data.

Premere CTRL+F5 per eseguire l'applicazione. Selezionare un collegamento di modifica per verificare che il campo di input per la data di rilascio viene visualizzata solo la data.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

In **Esplora soluzioni**, espandere il *viste* cartella, espandere il *condiviso* cartella e quindi fare di *Views\Shared\EditorTemplates* cartella.

Fare clic su **Add**, quindi fare clic su **visualizzazione**. Il **Aggiungi visualizzazione** verrà visualizzata la finestra di dialogo.

Nel **nome della vista** , digitare &quot;data&quot;.

Selezionare il **Crea come visualizzazione parziale** casella di controllo. Assicurarsi che il **usano una pagina master o layout** e **creare una visualizzazione fortemente tipizzata** caselle di controllo non vengono selezionate.

Fare clic su **Aggiungi**. Il *Views\Shared\EditorTemplates\Date.cshtml* modello viene creato.

Aggiungere il codice seguente per il *Views\Shared\EditorTemplates\Date.cshtml* modello.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

La prima riga dichiara il modello sia un `DateTime` tipo. Anche se non è necessario dichiarare il tipo di modello in modalità di modifica e visualizzare i modelli, è consigliabile che in fase di compilazione di ottenere il controllo del modello passato alla visualizzazione. (Un altro vantaggio è che quindi di ottenere IntelliSense per il modello nella visualizzazione in Visual Studio). Se il tipo di modello non è dichiarato, ASP.NET MVC considera un [dinamica](https://msdn.microsoft.com/library/dd264741.aspx) digitare e non sono previsti tempi di compilazione il controllo dei tipi. Se si dichiara il modello sia un `DateTime` tipo, diventa fortemente tipizzato.

La seconda riga viene semplicemente letterale markup HTML che consente di visualizzare &quot;modello di data usando&quot; prima di un campo Data. Si userà questa riga temporaneamente per verificare che questo modello di data è in uso.

La riga successiva è un' [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) helper che esegue il rendering di un `input` campo che rappresenta una casella di testo. Il terzo parametro per l'helper utilizza un tipo anonimo per impostare la classe per la casella di testo `datefield` e il tipo su `date`. (Poiché `class` è una proprietà riservata in c#, è necessario usare il `@` carattere di escape di `class` attributo nel parser c#.)

Il `date` tipo è un tipo di input HTML5 che consente ai browser compatibili con HTML5 per il rendering di un controllo di calendario HTML5. In seguito si aggiungerà codice JavaScript per associare il controllo datepicker jQuery per la `Html.TextBox` elemento usando il `datefield` classe.

Premere CTRL+F5 per eseguire l'applicazione. È possibile verificare che il `ReleaseDate` proprietà nella visualizzazione di modifica è usando il modello di modifica perché il modello consente di visualizzare &quot;con modello di data&quot; immediatamente prima di `ReleaseDate` testo di input casella, come illustrato in questa immagine:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Nel browser visualizzare l'origine della pagina. (Ad esempio, la pagina e scegliere **Visualizza origine**.) Il seguente esempio vengono illustrate alcune il markup della pagina, che illustra il `class` e `type` attributi nel codice HTML sottoposto a rendering.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Tornare al *Views\Shared\EditorTemplates\Date.cshtml* modello e rimuovere le &quot;usando modelli di data&quot; markup. A questo punto il modello completato aspetto simile al seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Aggiunta di un jQuery UI Datepicker Popup Calendar tramite NuGet

In questa sezione si aggiungerà il [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendario popup per il modello di data-edit. Il [interfaccia utente di jQuery](http://jqueryui.com/) libreria fornisce il supporto per l'animazione, effetti e widget personalizzabili avanzate. Poiché è basato sulla libreria JavaScript jQuery. Il calendario popup per datepicker rende più semplice e naturale per immettere le date usando un calendario invece di immettere una stringa. Il calendario popup limita inoltre gli utenti a date legali, immissione di testo normale per una data consente di immettere un nome, ad esempio `2/33/1999` (febbraio 33rd, 1999), ma la [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendario popup per non consentirà l'associazione.

In primo luogo, è necessario installare le librerie dell'interfaccia utente di jQuery. A tale scopo, si userà NuGet, che è un pacchetto di gestione che è incluso nelle versioni SP1 di Visual Studio 2010 e Visual Web Developer.

In Visual Web Developer, dalla **strumenti** dal menu **Gestione pacchetti NuGet** , quindi selezionare **Gestisci pacchetti NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Nota: Se il **strumenti** menu non viene visualizzato il **NuGet Gestione pacchetti** comando, è necessario installare NuGet seguendo le istruzioni [installazione NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) pagina di il sito Web NuGet.   
  
Se si utilizza Visual Studio anziché Visual Web Developer, dal **strumenti** dal menu **Gestione pacchetti NuGet** e quindi selezionare **aggiungere riferimenti alla libreria del pacchetto**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

Nel **MVCMovie - Gestisci pacchetti NuGet** finestra di dialogo, fare clic sui **Online** scheda a sinistra e quindi immettere &quot;jQuery.UI&quot; nella casella di ricerca. Selezionare j **widget dell'interfaccia utente di Query: Datepicker**, quindi selezionare la **installare** pulsante.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet aggiunge queste versioni di debug e minimizzati le versioni di jQuery Core dell'interfaccia utente e la selezione di data dell'interfaccia utente di jQuery al progetto:

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

Nota: Nelle versioni di debug (i file senza il *. min.js* estensione) sono utili per eseguire il debug, ma in un sito di produzione, è necessario includere solo le versioni minimizzate.

Per utilizzare effettivamente la selezione della data jQuery, è necessario creare uno script jQuery che si collegherà il widget di calendario per il modello di modifica. In **Esplora soluzioni**, fare doppio clic il *script* cartella e selezionare **Add**, quindi **nuovo elemento**e quindi **JScript File**. Denominare il file *DatePickerReady.js*.

Aggiungere il codice seguente per il *DatePickerReady.js* file:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Se non si ha familiarità con jQuery, ecco una breve spiegazione di questa operazione: la prima riga è il &quot;jQuery pronto&quot; funzione, che viene chiamato quando sono caricati tutti gli elementi DOM in una pagina. La seconda riga Seleziona tutti gli elementi DOM che hanno il nome della classe `datefield`, quindi richiama il `datepicker` funzione per ognuno di essi. (Tenere presente che è stato aggiunto il `datefield` classe per il *Views\Shared\EditorTemplates\Date.cshtml* modello in precedenza nell'esercitazione.)

Successivamente, aprire il *Views\Shared\\layout. cshtml* file. È necessario aggiungere i riferimenti ai file seguenti, che sono tutti obbligatori in modo che è possibile usare il controllo selezione data:

- *Content/Themes/base/jQuery.UI.Core.CSS*
- *Content/Themes/base/jQuery.UI.DatePicker.CSS*
- *Content/Themes/base/jQuery.UI.theme.CSS*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

L'esempio seguente illustra il codice effettivo che è necessario aggiungere nella parte inferiore della `head` elemento il *Views\Shared\\layout. cshtml* file.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

L'intero `head` sezione è illustrata di seguito:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

Il [helper dell'URL contenuto](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) metodo converte il percorso della risorsa in un percorso assoluto. È necessario usare `@URL.Content` correttamente fare riferimento a queste risorse quando l'applicazione è in esecuzione in IIS.

Premere CTRL+F5 per eseguire l'applicazione. Selezionare un collegamento di modifica e quindi inserire il punto di inserimento di **ReleaseDate** campo. Viene visualizzato il calendario popup dell'interfaccia utente di jQuery.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Come la maggior parte dei controlli di jQuery, il controllo datepicker consente di personalizzare in modo esteso. Per informazioni, vedere [personalizzazione Visual: progettazione di un tema dell'interfaccia utente di jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) nel [interfaccia utente di jQuery](http://learn.jquery.com/jquery-ui/getting-started/) sito.

### <a name="supporting-the-html5-date-input-control"></a>Che supportano il controllo di Input data HTML5

Come maggiore di browser supporta HTML5, è opportuno usare HTML5 nativo di input, ad esempio il `date` elemento di input e non usano il calendario dell'interfaccia utente di jQuery. È possibile aggiungere logica all'applicazione per usare automaticamente i controlli HTML5 se il browser li supporta. A tale scopo, sostituire il contenuto del *DatePickerReady.js* file con il codice seguente:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

La prima riga di questo script Usa Modernizr per verificare che l'input data HTML5 è supportato. Se non è supportato, la selezione di data dell'interfaccia utente di jQuery è immune invece. ([Modernizr](http://www.modernizr.com/docs/) è una libreria JavaScript open source che consente di rilevare la disponibilità delle implementazioni native di HTML5 e CSS3. Modernizr è incluso in tutti i nuovi progetti ASP.NET MVC che si creano).

Dopo aver apportato questa modifica, è possibile testarla usando un browser che supporta HTML5, ad esempio 11 Opera. Eseguire l'applicazione tramite un browser compatibili con HTML5 e modificare una voce di film. Viene usato il controllo Data HTML5 anziché il calendario popup dell'interfaccia utente di jQuery:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Poiché le nuove versioni dei browser di implementazione di HTML5 in modo incrementale, un buon approccio per ora consiste nell'aggiungere codice al sito Web che supporta un'ampia gamma di supporto per HTML5. Ad esempio, una più solida *DatePickerReady.js* script è illustrato di seguito che consente i browser di supporto del sito che supportano solo parzialmente il controllo data e HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Questo script consente di selezionare HTML5 `input` elementy typu `date` che non supportano completamente il controllo data e HTML5. Per tali elementi, collega il calendario popup dell'interfaccia utente di jQuery e quindi modificato il `type` dall'attributo `date` a `text`. Modificando la `type` dall'attributo `date` a `text`, è stato eliminato un supporto parziale della data HTML5. Un ancora più solida *DatePickerReady.js* script reperibili [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Aggiunta di date che ammette valori null per i modelli

Se si usa uno dei modelli di data esistente e passa a una data null, si otterrà un errore di run-time. Per rendere più affidabile i modelli di data, sarà necessario modificare in modo da gestire valori null. Modificare il codice per supportare le date che ammette valori null, il *Views\Shared\DisplayTemplates\DateTime.cshtml* al seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Il codice restituisce una stringa vuota quando il modello è **null**.

Modificare il codice il *Views\Shared\EditorTemplates\Date.cshtml* file come segue:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Quando questo codice viene eseguito, se il modello non è null, il modello `DateTime` valore viene utilizzato. Se il modello è null, viene invece utilizzata la data corrente.

### <a name="wrapup"></a>Riepilogo

Questa esercitazione ha illustrato le nozioni di base di helper basati su modelli di ASP.NET e illustra come usare il calendario di jQuery UI datepicker popup in un'applicazione ASP.NET MVC. Per altre informazioni, provare a eseguire queste risorse:

- Per informazioni sulla localizzazione, vedere il blog del Rajeesh [JQueryUI Datepicker in ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Per informazioni sull'interfaccia utente di jQuery, vedere [interfaccia utente di jQuery](http://docs.jquery.com/UI).
- Per informazioni su come localizzare il controllo datepicker, vedere [dell'interfaccia utente/Datepicker/localizzazione](http://docs.jquery.com/UI/Datepicker/Localization).
- Per altre informazioni sui modelli di ASP.NET MVC, vedere serie di blog di Brad Wilson sul [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Anche se la serie è stato scritto per ASP.NET MVC 2, il materiale comunque valido per la versione corrente di ASP.NET MVC.

> [!div class="step-by-step"]
> [Precedente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
