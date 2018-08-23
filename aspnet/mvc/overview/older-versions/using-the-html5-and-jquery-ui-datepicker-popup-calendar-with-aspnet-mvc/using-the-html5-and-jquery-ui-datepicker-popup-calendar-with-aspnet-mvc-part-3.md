---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Uso di HTML5 e jQuery UI Datepicker Popup Calendar con ASP.NET MVC - parte 3 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base di come usare i modelli nell'editor di modelli di visualizzazione e il calendario jQuery UI datepicker popup in MV un ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 184d93323ac6f2cb7f14d32b401cf6a400eb79a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835296"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Uso di HTML5 e jQuery UI Datepicker Popup Calendar con ASP.NET MVC - parte 3
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Questa esercitazione insegnerà le nozioni di base di come usare i modelli nell'editor di modelli di visualizzazione e il calendario di jQuery UI datepicker popup in un'applicazione Web ASP.NET MVC.


## <a name="working-with-complex-types"></a>Utilizzo di tipi complessi

In questa sezione si verrà creato una classe indirizzo e informazioni su come creare un modello per visualizzarlo.

Nel *modelli* cartella, creare un nuovo file di classe denominato *Person.cs* in cui verrà inserito due tipi: una `Person` classe e un `Address` classe. Il `Person` contiene una proprietà tipizzata come classe `Address`. Il `Address` tipo è un tipo complesso, vale a dire non è uno dei tipi incorporati, ad esempio `int`, `string`, o `double`. Ma ne contiene diverse proprietà. Il codice per le nuove classi è simile alla seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

Nel `Movie` controller, aggiungere il codice seguente `PersonDetail` azione per visualizzare un'istanza di persona:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Quindi aggiungere il codice seguente per il `Movie` controller per popolare il `Person` modello con alcuni dati di esempio:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Aprire il *Views\Movies\PersonDetail.cshtml* file e aggiungere il markup seguente per il `PersonDetail` vista.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Premere CTRL+F5 per eseguire l'applicazione e passare a *Movies/PersonDetail*.

Il `PersonDetail` vista non contiene il `Address` tipo complesso, come illustrato in questo screenshot. (Nessun indirizzo figura).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

Il `Address` i dati del modello non viene visualizzati perché è un tipo complesso. Per visualizzare le informazioni sull'indirizzo, aprire il *Views\Movies\PersonDetail.cshtml* file nuovo e aggiungere il markup seguente.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Il markup completo per il `PersonDetail` ora Vista si presenta come segue:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Eseguire nuovamente l'applicazione e visualizzare il `PersonDetail` vista. Vengono ora visualizzate le informazioni di indirizzo:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Creazione di un modello per un tipo complesso

In questa sezione si creerà un modello che verrà usato per eseguire il rendering di `Address` tipo complesso. Quando si crea un modello per il `Address` tipo, ASP.NET MVC può automaticamente utilizzarlo per formattare un modello di indirizzo in un punto qualsiasi nell'applicazione. Ciò offre un modo per controllare il rendering del `Address` tipo da un'unica posizione nell'applicazione.

Nel *Views\Shared\DisplayTemplates.* cartella, creare una visualizzazione parziale fortemente tipizzata denominata **indirizzo**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Fare clic su **Add**, quindi aprire il nuovo *Views\Shared\DisplayTemplates\Address.cshtml* file. La nuova visualizzazione contiene il markup generato seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Eseguire l'applicazione e visualizzare il `PersonDetail` vista. Questa volta, il `Address` modello appena creato viene usato per visualizzare il `Address` tipo complesso, in modo che la visualizzazione è simile a quanto segue:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Riepilogo: Modi per specificare il formato di visualizzazione del modello e il modello

Si è visto che è possibile specificare il formato o modello per una proprietà del modello utilizzando gli approcci seguenti:

- Applicando la `DisplayFormat` attributo a una proprietà nel modello. Ad esempio, il codice seguente causa la data da visualizzare senza l'ora:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Applicazione di un [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo a una proprietà nel modello e specificare il tipo di dati. Ad esempio, il codice seguente fa sì che la data da visualizzare senza l'ora.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Se l'applicazione contiene un *date.cshtml* modello nel *Views\Shared\DisplayTemplates.* cartella o nella *Views\Movies\DisplayTemplates* cartella, tale modello verrà usato per eseguire il rendering di `DateTime` proprietà. Il sistema di creazione modello ASP.NET predefinito in caso contrario, viene visualizzata la proprietà come Data.
- Creazione di un modello di visualizzazione nel *Views\Shared\DisplayTemplates.* cartella o il *Views\Movies\DisplayTemplates* cartella il cui nome corrisponde al tipo di dati che si desidera formattare. Ad esempio, si è visto che il *Views\Shared\DisplayTemplates\DateTime.cshtml* utilizzato per eseguire il rendering `DateTime` proprietà in un modello, senza l'aggiunta di un attributo nel modello e senza l'aggiunta di eventuali tag alle visualizzazioni.
- Usando il [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attributo sul modello per specificare il modello per visualizzare le proprietà del modello.
- Aggiungere in modo esplicito il nome del modello di visualizzazione per il [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) chiamare in una vista.

L'approccio che si utilizza varia a seconda cosa da eseguire nell'applicazione. Non è insolito combinare questi approcci per ottenere esattamente il tipo di formattazione che è necessario.

Nella sezione successiva, sarà possibile passare un po' analizzato e spostare da personalizzare la modalità di visualizzazione per personalizzare la modalità di immissione dati. Sarà associare il controllo datepicker jQuery per le viste modifica dell'applicazione per fornire un buon metodo per specificare le date.

> [!div class="step-by-step"]
> [Precedente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Successivo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
