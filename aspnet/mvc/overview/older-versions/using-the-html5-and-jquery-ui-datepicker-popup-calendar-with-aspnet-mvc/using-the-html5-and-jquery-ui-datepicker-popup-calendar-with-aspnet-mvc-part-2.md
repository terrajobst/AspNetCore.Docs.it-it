---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Uso di HTML5 e jQuery UI Datepicker Popup Calendar con ASP.NET MVC - parte 2 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base di come usare i modelli nell'editor di modelli di visualizzazione e il calendario jQuery UI datepicker popup in MV un ASP.NET...
ms.author: aspnetcontent
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 0dc9957bbb13394a4ee225d9966f4004de5714b9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839307"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Uso di HTML5 e jQuery UI Datepicker Popup Calendar con ASP.NET MVC - parte 2
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Questa esercitazione insegnerà le nozioni di base di come usare i modelli nell'editor di modelli di visualizzazione e il calendario di jQuery UI datepicker popup in un'applicazione Web ASP.NET MVC.


## <a name="adding-an-automatic-datetime-template"></a>Aggiunta di un modello di data/ora automatica

Nella prima parte di questa esercitazione, è stato illustrato come è possibile aggiungere attributi al modello per specificare in modo esplicito la formattazione e come è possibile specificare esplicitamente il modello utilizzato per il rendering del modello. Ad esempio, il [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo in esplicitamente il codice seguente specifica la formattazione per il `ReleaseDate` proprietà.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

Nell'esempio seguente, il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) dell'attributo, utilizzando il `Date` enumerazione, specifica che il modello di data deve essere utilizzato per il rendering del. Se è presente alcun modello di data nel progetto, viene utilizzato il modello di data predefinita.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Tuttavia, ASP. MVC è possibile eseguire usando la convenzione-over-configurazione, mediante la ricerca di un modello che corrisponde al nome di un tipo di corrispondenza del tipo. Ciò consente di creare un modello che formatta automaticamente i dati senza usare affatto attributi o codice. Per questa parte dell'esercitazione, si creerà un modello che viene applicato automaticamente alle proprietà del modello di tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Non dovrai usare un attributo o un'altra configurazione per specificare che il modello deve essere usato per eseguire il rendering di tutte le proprietà del modello di tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Scoprirai anche un modo per personalizzare la visualizzazione di singole proprietà o persino singoli campi.

Per iniziare, è possibile rimuovere informazioni di formattazione esistente e visualizzare le date complete nell'applicazione.

Aprire il *Movie.cs* del file e impostare come commento il `DataType` dell'attributo sul `ReleaseDate` proprietà:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Premere CTRL+F5 per eseguire l'applicazione.

Si noti che il `ReleaseDate` proprietà ora visualizza la data e l'ora, perché è l'impostazione predefinita quando viene fornita alcuna informazione di formattazione.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Aggiunta di stili CSS per i nuovi modelli di test

Prima di creare un modello per la formattazione delle date, si aggiungeranno alcune regole di stile CSS che è possibile applicare ai nuovi modelli. Queste considerazioni consentono di verificare che la pagina sottoposta a rendering viene utilizzato il nuovo modello.

Aprire il *Content\Site.cs*s file e aggiungere le regole CSS seguente alla fine del file:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Aggiunta di modelli di visualizzazione di data/ora

A questo punto è possibile creare il nuovo modello. Nel *viste\filmati* cartella, creare un *DisplayTemplates* cartella.

Nel *Views\Shared* cartella, creare un *DisplayTemplates* cartella e un *EditorTemplates* cartella.

I modelli di visualizzazione nel *Views\Shared\DisplayTemplates.* cartella verrà utilizzato da tutti i controller. I modelli di visualizzazione nel *Views\Movie\DisplayTemplates* cartella verrà utilizzato solo per il `Movie` controller. (Se viene visualizzato in entrambe le cartelle, il modello in un modello con lo stesso nome il *Views\Movie\DisplayTemplates* cartella, vale a dire, il modello più specifico, ovvero ha la precedenza per le viste restituite dal `Movie` controller.)

In **Esplora soluzioni**, espandere il *viste* cartella, espandere il *condiviso* cartella e quindi fare di *Views\Shared\DisplayTemplates.* cartella.

Fare clic su **Add** e quindi fare clic su **visualizzazione**. Il **Aggiungi visualizzazione** verrà visualizzata la finestra di dialogo.

Nel **nome della vista** , digitare `DateTime`. (È necessario utilizzare questo nome per la corrispondenza con il nome del tipo).

Selezionare il **Crea come visualizzazione parziale** casella di controllo. Assicurarsi che il **usano una pagina master o layout** e **creare una visualizzazione fortemente tipizzata** caselle di controllo non vengono selezionate.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Fare clic su **Aggiungi**. Oggetto *DateTime.cshtml* modello viene creato nel *Views\Shared\DisplayTemplates*.

L'immagine seguente mostra il *viste* cartella **Esplora soluzioni** dopo la `DateTime` editor e visualizzare i modelli vengono creati.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Aprire il *Views\Shared\DisplayTemplates\DateTime.cshtml* file e aggiungere il markup seguente, che usa le [String. Format](https://msdn.microsoft.com/library/system.string.format.aspx) metodo per formattare la proprietà come una data senza l'ora. (Il `{0:d}` formato specifica di formato di data breve.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Ripetere questo passaggio per creare un `DateTime` modello nel *Views\Movie\DisplayTemplates* cartella. Usare il codice seguente nel *Views\Movie\DisplayTemplates\DateTime.cshtml* file.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

Il `loud-1` classe CSS fa sì che la data visualizzare testo in grassetto di colore rosso. Si è aggiunta il `loud-1` classe CSS come misura temporanea in modo da visualizzare con facilità quando viene usato in questo particolare modello.

Le modifiche apportate, viene creato e modelli che ASP.NET utilizzerà per visualizzare le date personalizzati. Il modello più generale (nelle *Views\Shared\DisplayTemplates.* cartella) viene visualizzata una data breve semplice. Il modello è specifico per il `Movie` controller (nelle *Views\Movies\DisplayTemplates* cartella) viene visualizzata una data breve che viene anche formattata come testo di colore rosso in grassetto.

Premere CTRL+F5 per eseguire l'applicazione. Il browser visualizza la visualizzazione dell'indice per l'applicazione.

Il `ReleaseDate` proprietà Visualizza ora la data in grassetto escluso il tempo di colore rosso. Ciò consente di verificare che il `DateTime` helper basato su modelli nel *Views\Movies\DisplayTemplates* cartella viene selezionata tramite la `DateTime` helper basato su modelli nella cartella condivisa (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Ora rinominare le *Views\Movies\DisplayTemplates\DateTime.cshtml* del file ai *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Premere CTRL+F5 per eseguire l'applicazione.

Questa volta il `ReleaseDate` proprietà viene visualizzata una data senza l'ora e il tipo di carattere rosso in grassetto. Ciò dimostra che il tipo di un modello con il nome dei dati (in questo caso `DateTime`) viene utilizzata automaticamente per visualizzare tutte le proprietà del modello di quel tipo. Dopo aver rinominato il *DateTime.cshtml* file *LoudDateTime.cshtml*, ASP.NET non è possibile trovare un modello nel *Views\Movies\DisplayTemplates* cartella, quindi usato il *DateTime.cshtml* dal modello di * Views\Movies\Shared\* cartella.

(Modello corrispondente è maiuscole e minuscole, pertanto potrebbero essere stati creati il nome del file modello con le maiuscole e minuscole. Ad esempio, *DATETIME.chstml, datetime.cshtml*, e *DaTeTiMe.cshtml* tutti corrisponderebbe la `DateTime` tipo.)

Esaminare: a questo punto, il `ReleaseDate` campo viene visualizzato utilizzando il *Views\Movies\DisplayTemplates\DateTime.cshtml* modello, che consente di visualizzare i dati usando un formato di data breve, ma in caso contrario, viene aggiunto alcun formato speciale.

### <a name="using-uihint-to-specify-a-display-template"></a>Utilizzo di UIHint per specificare un modello di visualizzazione

Se l'applicazione web dispone di molte `DateTime` i campi e per impostazione predefinita da visualizzare tutte o la maggior parte di essi in formato data-only, il *DateTime.cshtml* modello è un buon approccio. Ma cosa accade se si hanno alcune date in cui si desidera visualizzare la data e ora complete? Nessun problema. È possibile creare un modello aggiuntivo e usare la [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attributo per specificare la formattazione per la data e ora complete. È quindi possibile applicare in modo selettivo tale modello. È possibile usare la [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attributo a livello di modello o è possibile specificare il modello all'interno di una vista. In questa sezione verrà illustrato come usare il `UIHint` attributo da modificare in modo selettivo la formattazione per alcune istanze di campi di data e ora.

Aprire il *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* file e sostituire il codice esistente con il codice seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Questo fa sì che la data e ora complete da visualizzare e aggiunge la classe CSS che rende il testo, verde e grandi dimensioni.

Aprire il *Movie.cs* file e aggiungere il [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) dell'attributo di `ReleaseDate` proprietà, come illustrato nell'esempio seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Ciò indica a MVC ASP.NET che quando questo viene visualizzato il `ReleaseDate` proprietà (in particolare e non a qualsiasi `DateTime` oggetto), deve utilizzare il *LoudDateTime.cshtml* modello.

Premere CTRL+F5 per eseguire l'applicazione.

Si noti che il `ReleaseDate` proprietà Visualizza ora la data e ora in caratteri grandi verde.

Tornare al `UIHint` attributo la *Movie.cs* del file e impostarlo come commento in modo che il *LoudDateTime.cshtml* modello non verrà usato. Eseguire di nuovo l'applicazione. Data di rilascio non è visualizzata grandi e verde. Verifica che il *Views\Shared\DisplayTemplates\DateTime.cshtml* modello viene utilizzato nelle viste di Index e Details.

Come accennato in precedenza, è inoltre possibile applicare un modello in una vista, che consente di applicare il modello a una singola istanza di alcuni dati. Aprire il *Views\Movies\Details.cshtml* visualizzazione. Aggiungere `"LoudDateTime"` come secondo parametro del [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) chiamare per il `ReleaseDate` campo. Ecco il codice completo:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Specifica che il `LoudDateTime` modello deve essere usato per visualizzare le proprietà del modello, indipendentemente dal fatto che gli attributi vengono applicati al modello.

Premere CTRL+F5 per eseguire l'applicazione.

Verificare che la pagina di indice di film utilizza il *Views\Shared\DisplayTemplates\DateTime.cshtml* modello (rosso in grassetto) e il *Movie\Details* pagina Usa la *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* modello (grande e verde).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Nella sezione successiva, si creerà un modello per un tipo complesso.

> [!div class="step-by-step"]
> [Precedente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Successivo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
