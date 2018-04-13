---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Utilizzo di HTML5 e jQuery UI Datepicker Popup Calendar with ASP.NET MVC - parte 2 | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione verrà fornite le nozioni di base lavorare con modelli di editor, modelli di visualizzazione e il calendario di jQuery UI datepicker popup in una macchina virtuale di ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84112316a9ace732cb7d75d7cbaeb071c72de822
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Utilizzo di HTML5 e jQuery UI Datepicker Popup Calendar with ASP.NET MVC - parte 2
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base lavorare con modelli di editor, modelli di visualizzazione e il calendario di jQuery UI datepicker popup in un'applicazione Web ASP.NET MVC.


## <a name="adding-an-automatic-datetime-template"></a>Aggiunta di un modello di data/ora automatica

Nella prima parte di questa esercitazione, è stato illustrato come è possibile aggiungere attributi al modello per specificare in modo esplicito di formattazione, e come è possibile specificare in modo esplicito il modello utilizzato per eseguire il rendering del modello. Ad esempio, il [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo esplicitamente il codice seguente specifica la formattazione per il `ReleaseDate` proprietà.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

Nell'esempio seguente, il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo, mediante il `Date` enumerazione, specifica che il modello di data da utilizzare per il rendering del modello. Se è presente alcun modello di data nel progetto, viene utilizzato il modello di data predefinito.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Tuttavia, ASP. MVC è possibile eseguire usando la convenzione-over-configurazione, mediante la ricerca di un modello che corrisponde al nome di un tipo di corrispondenza del tipo. Ciò consente di creare un modello che formatta automaticamente i dati senza usare affatto attributi o codice. Per questa parte dell'esercitazione, si creerà un modello che viene applicato automaticamente alle proprietà del modello di tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Non è necessario utilizzare un attributo o altre operazioni di configurazione per specificare che il modello deve essere utilizzato per eseguire il rendering di tutte le proprietà di modello di tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Si apprenderà inoltre un modo per personalizzare la visualizzazione delle singole proprietà o persino singoli campi.

Per iniziare, rimuovere le informazioni di formattazione esistente e visualizzare le date di complete nell'applicazione.

Aprire il *Movie.cs* file e impostare come commento il `DataType` attributo la `ReleaseDate` proprietà:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Premere CTRL+F5 per eseguire l'applicazione.

Si noti che il `ReleaseDate` proprietà Visualizza ora la data e l'ora, perché è l'impostazione predefinita, quando viene fornita alcuna informazione di formattazione.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Aggiunta di stili CSS per il test di nuovi modelli

Prima di creare un modello per la formattazione delle date, si aggiungeranno alcune regole di stile CSS che è possibile applicare ai nuovi modelli. Queste considerazioni consentono di verificare che la pagina utilizza il nuovo modello.

Aprire il *Content\Site.cs*s file e aggiungere le regole CSS seguente alla fine del file:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Aggiunta di modelli di visualizzazione DateTime

È ora possibile creare il nuovo modello. Nel *Views\Movies* cartella, creare un *DisplayTemplates* cartella.

Nel *Views\Shared* cartella, creare un *DisplayTemplates* cartella e un *EditorTemplates* cartella.

I modelli di visualizzazione di *Views\Shared\DisplayTemplates.* cartella verrà utilizzata da tutti i controller. I modelli di visualizzazione nel *Views\Movie\DisplayTemplates* verrà utilizzata solo dalla cartella di `Movie` controller. (Se viene visualizzato un modello con lo stesso nome in entrambe le cartelle, il modello nel *Views\Movie\DisplayTemplates* cartella, ovvero, ovvero il modello più specifico, ha la precedenza per le visualizzazioni restituito dal `Movie` controller.)

In **Esplora soluzioni**, espandere il *viste* cartella, espandere il *Shared* cartella e quindi fare di *Views\Shared\DisplayTemplates.* cartella.

Fare clic su **Aggiungi** e quindi fare clic su **vista**. Il **Aggiungi visualizzazione** viene visualizzata la finestra di dialogo.

Nel **nome vista** digitare `DateTime`. (È necessario utilizzare questo nome per la corrispondenza con il nome del tipo).

Selezionare il **Crea come visualizzazione parziale** casella di controllo. Assicurarsi che il **utilizza una layout o pagina master** e **creare una visualizzazione fortemente tipizzata** caselle di controllo non vengono selezionate.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Fare clic su **Aggiungi**. Oggetto *DateTime.cshtml* modello viene creato nel *Views\Shared\DisplayTemplates*.

La figura seguente mostra il *viste* cartella **Esplora** dopo il `DateTime` editor e visualizzare i modelli vengono creati.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Aprire il *Views\Shared\DisplayTemplates\DateTime.cshtml* file e aggiungere il markup seguente, che utilizza il [String. Format](https://msdn.microsoft.com/library/system.string.format.aspx) metodo per formattare la proprietà come una data senza l'ora. (Il `{0:d}` formato specifica il formato di data breve.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Ripetere questo passaggio per creare un `DateTime` modello il *Views\Movie\DisplayTemplates* cartella. Utilizzare il codice seguente nel *Views\Movie\DisplayTemplates\DateTime.cshtml* file.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

La `loud-1` classe CSS, la data visualizzare testo in grassetto di colore rosso. Si è aggiunta la `loud-1` classe CSS come una misura temporanea, in modo da visualizzare più facilmente quando viene utilizzato in questo particolare modello.

Le modifiche apportate, viene creato e modelli utilizzati per visualizzare le date ASP.NET personalizzati. Il modello più generale (nel *Views\Shared\DisplayTemplates.* cartella) viene visualizzata una data breve semplice. Il modello in modo specifico per il `Movie` controller (nel *Views\Movies\DisplayTemplates* cartella) consente di visualizzare una data breve formattata anch ' esso come testo in grassetto di colore rosso.

Premere CTRL+F5 per eseguire l'applicazione. Il browser esegue il rendering della visualizzazione dell'indice per l'applicazione.

Il `ReleaseDate` proprietà Visualizza ora la data in grassetto rosso senza l'ora. Consente di verificare che il `DateTime` helper basato su modelli nel *Views\Movies\DisplayTemplates* viene selezionata la cartella tramite il `DateTime` helper basato su modelli nella cartella condivisa (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Ora di rinominare il *Views\Movies\DisplayTemplates\DateTime.cshtml* file *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Premere CTRL+F5 per eseguire l'applicazione.

Questa volta il `ReleaseDate` proprietà vengono visualizzate una data senza l'ora e il carattere di colore rosso in grassetto. Nell'esempio viene illustrato che il tipo di un modello con il nome dei dati (in questo caso `DateTime`) viene usato automaticamente per visualizzare tutte le proprietà di modello di quel tipo. Dopo aver rinominato il *DateTime.cshtml* file *LoudDateTime.cshtml*, ASP.NET non è possibile trovare un modello nel *Views\Movies\DisplayTemplates* cartella, pertanto utilizzato il *DateTime.cshtml* modello dal * Views\Movies\Shared\* cartella.

(La corrispondenza del modello viene fatta distinzione tra maiuscole e minuscole, pertanto potrebbero essere stati creati nel nome del modello con le maiuscole e minuscole. Ad esempio, *DATETIME.chstml, datetime.cshtml*, e *DaTeTiMe.cshtml* tutti corrisponderebbe il `DateTime` tipo.)

Per esaminare: a questo punto, il `ReleaseDate` campo viene visualizzato utilizzando il *Views\Movies\DisplayTemplates\DateTime.cshtml* modello, che consente di visualizzare i dati utilizzando un formato di data breve, ma in caso contrario non aggiunge alcun formato speciale.

### <a name="using-uihint-to-specify-a-display-template"></a>Utilizzo di UIHint per specificare un modello di visualizzazione

Se l'applicazione web dispone di molti `DateTime` campi e per impostazione predefinita, si desidera visualizzare tutte o la maggior parte di essi in formato data-only, di *DateTime.cshtml* modello è un buon approccio. Ma cosa accade se si dispone alcune date in cui si desidera visualizzare la data e ora complete? Nessun problema. È possibile creare un modello aggiuntivo e utilizzare il [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attributo per specificare la formattazione per la data e ora complete. Quindi è possibile applicare tale modello in modo selettivo. È possibile utilizzare il [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attributo al livello del modello oppure è possibile specificare il modello all'interno di una vista. In questa sezione si vedrà come utilizzare il `UIHint` attributo da modificare in modo selettivo la formattazione ad alcune istanze di campi di data e ora.

Aprire il *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* file e sostituire il codice esistente con il seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

In questo modo la data e ora complete da visualizzare e aggiunge la classe CSS che rende il testo, verde e grandi dimensioni.

Aprire il *Movie.cs* file e aggiungere il [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attributo la `ReleaseDate` proprietà, come illustrato nell'esempio seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

In questo modo ASP.NET MVC che quando viene visualizzata la `ReleaseDate` proprietà (in particolare e non a qualsiasi `DateTime` oggetto), è necessario utilizzare il *LoudDateTime.cshtml* modello.

Premere CTRL+F5 per eseguire l'applicazione.

Si noti che il `ReleaseDate` proprietà Visualizza ora la data e ora in caratteri grandi verde.

Restituito per il `UIHint` attributo il *Movie.cs* file e impostarlo come commento il *LoudDateTime.cshtml* modello non può essere utilizzato. Eseguire di nuovo l'applicazione. La data di rilascio non viene visualizzata grandi e verde. Verifica che il *Views\Shared\DisplayTemplates\DateTime.cshtml* modello viene utilizzato nelle visualizzazioni dei dettagli e di indice.

Come accennato in precedenza, è inoltre possibile applicare un modello in una vista, che consente di applicare il modello a una singola istanza di alcuni dati. Aprire il *Views\Movies\Details.cshtml* visualizzazione. Aggiungere `"LoudDateTime"` come secondo parametro del [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) chiamata per il `ReleaseDate` campo. Ecco il codice completo:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Specifica che il `LoudDateTime` modello deve essere utilizzato per visualizzare la proprietà del modello, indipendentemente dagli attributi che vengono applicati al modello.

Premere CTRL+F5 per eseguire l'applicazione.

Verificare che la pagina di indice film stia utilizzando il *Views\Shared\DisplayTemplates\DateTime.cshtml* modello (rosso in grassetto) e *Movie\Details* pagina utilizza il *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* modello (grandi e verde).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Nella sezione successiva, si creerà un modello per un tipo complesso.

> [!div class="step-by-step"]
> [Precedente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Successivo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
