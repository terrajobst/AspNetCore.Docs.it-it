---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Utilizzo di HTML5 e jQuery UI Datepicker Popup Calendar with ASP.NET MVC - parte 3 | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione verrà fornite le nozioni di base lavorare con modelli di editor, modelli di visualizzazione e il calendario di jQuery UI datepicker popup in una macchina virtuale di ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 7d4ed67254c2b0fc2aef748cfab1c8f628b25641
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Utilizzo di HTML5 e jQuery UI Datepicker Popup Calendar with ASP.NET MVC - parte 3
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base lavorare con modelli di editor, modelli di visualizzazione e il calendario di jQuery UI datepicker popup in un'applicazione Web ASP.NET MVC.


## <a name="working-with-complex-types"></a>Utilizzo di tipi complessi

In questa sezione sarà di creare una classe di indirizzo e informazioni su come creare un modello per visualizzarlo.

Nel *modelli* cartella, creare un nuovo file di classe denominato *Person* in cui verrà inserita due tipi: una `Person` classe e un `Address` classe. Il `Person` contiene una proprietà tipizzata come classe `Address`. Il `Address` tipo è un tipo complesso, ovvero non è uno dei tipi incorporati come `int`, `string`, o `double`. Al contrario, contiene diverse proprietà. Il codice per le nuove classi è simile al seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

Nel `Movie` controller, aggiungere il seguente `PersonDetail` azione per visualizzare un'istanza di persona:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Quindi aggiungere il codice seguente per il `Movie` controller per popolare il `Person` modello con alcuni dati di esempio:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Aprire il *Views\Movies\PersonDetail.cshtml* file e aggiungere il markup seguente per il `PersonDetail` visualizzazione.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Premere Ctrl + F5 per eseguire l'applicazione e passare a *filmati/PersonDetail*.

Il `PersonDetail` vista non contiene il `Address` tipo complesso, come è possibile osservare in questa schermata. (Nessun indirizzo figura).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

Il `Address` dati del modello non viene visualizzati perché è un tipo complesso. Per visualizzare le informazioni sull'indirizzo, aprire il *Views\Movies\PersonDetail.cshtml* file nuovo e aggiungere il markup seguente.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Il markup completo per il `PersonDetail` ora visualizzazione è simile al seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Eseguire nuovamente l'applicazione e visualizzare il `PersonDetail` visualizzazione. Le informazioni sull'indirizzo viene ora visualizzati:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Creazione di un modello per un tipo complesso

In questa sezione si creerà un modello che verrà utilizzato per eseguire il rendering di `Address` tipo complesso. Quando si crea un modello per il `Address` tipo, ASP.NET MVC possono usare automaticamente il formato di un modello di indirizzo in un punto qualsiasi nell'applicazione. In questo modo si consente di controllare il rendering di un modo di `Address` tipo da un unico posto, nell'applicazione.

Nel *Views\Shared\DisplayTemplates.* cartella, creare una visualizzazione parziale fortemente tipizzata denominata **indirizzo**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Fare clic su **Aggiungi**e quindi aprire il nuovo *Views\Shared\DisplayTemplates\Address.cshtml* file. La nuova vista contiene il seguente codice generato:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Eseguire l'applicazione e visualizzare il `PersonDetail` visualizzazione. Questa volta il `Address` modello appena creato viene utilizzato per visualizzare il `Address` tipo complesso, pertanto la visualizzazione è simile al seguente:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Riepilogo: Modi per specificare il formato di visualizzazione del modello e un modello

Si è visto che è possibile specificare il formato o un modello per una proprietà del modello utilizzando gli approcci seguenti:

- L'applicazione di `DisplayFormat` attributo a una proprietà nel modello. Ad esempio, il codice seguente causa la data da visualizzare senza l'ora:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- L'applicazione di un [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo a una proprietà nel modello e specificare il tipo di dati. Ad esempio, il codice seguente causa la data da visualizzare senza l'ora.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Se l'applicazione contiene un *date.cshtml* modello il *Views\Shared\DisplayTemplates.* cartella o il *Views\Movies\DisplayTemplates* cartella, tale modello verrà utilizzato per eseguire il rendering di `DateTime` proprietà. In caso contrario il sistema di modello predefinito di ASP.NET consente di visualizzare la proprietà come Data.
- Creazione di un modello di visualizzazione nel *Views\Shared\DisplayTemplates.* cartella o il *Views\Movies\DisplayTemplates* cartella il cui nome corrisponde al tipo di dati che si desidera formattare. Ad esempio, si è visto che il *Views\Shared\DisplayTemplates\DateTime.cshtml* è stato usato per il rendering `DateTime` proprietà in un modello, senza aggiunta di un attributo per il modello e senza aggiunta di tag alle visualizzazioni.
- Utilizzo di [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attributo sul modello per specificare il modello per visualizzare la proprietà del modello.
- Aggiungere in modo esplicito il nome del modello di visualizzazione per il [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) chiamare in una vista.

L'approccio adottato dipende da ciò che è necessario eseguire l'applicazione. Non è insolito per combinare questi approcci per ottenere esattamente il tipo di formattazione desiderati.

Nella sezione successiva, sarà possibile passare gli attrezzi un bit e spostamento di personalizzare la modalità di visualizzazione per personalizzare la modalità di immissione dati. Sarà collegare il datepicker jQuery alle viste modifica dell'applicazione per fornire un buon metodo per specificare le date.

>[!div class="step-by-step"]
[Precedente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Successivo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
