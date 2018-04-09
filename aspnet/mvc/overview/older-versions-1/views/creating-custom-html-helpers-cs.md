---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Creazione di helper HTML personalizzati (c#) | Documenti Microsoft
author: microsoft
description: L'obiettivo di questa esercitazione è dimostrare come è possibile creare l'helper HTML personalizzati che è possibile utilizzare all'interno di visualizzazioni MVC. L'utilizzo degli HTML Helper...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: ebc9aa2aa8dbc02dc01833d671c3bfd19141ba74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-html-helpers-c"></a>Creazione di helper HTML personalizzati (c#)
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> L'obiettivo di questa esercitazione è dimostrare come è possibile creare l'helper HTML personalizzati che è possibile utilizzare all'interno di visualizzazioni MVC. Sfruttando degli helper HTML, è possibile ridurre la quantità di noiosa digitazione dei tag HTML che è necessario eseguire per creare una pagina HTML standard.


L'obiettivo di questa esercitazione è dimostrare come è possibile creare l'helper HTML personalizzati che è possibile utilizzare all'interno di visualizzazioni MVC. Sfruttando degli helper HTML, è possibile ridurre la quantità di noiosa digitazione dei tag HTML che è necessario eseguire per creare una pagina HTML standard.

Nella prima parte di questa esercitazione, descrivo alcuni degli helper HTML esistente incluso con il framework di MVC ASP.NET. Successivamente, descrivo due metodi di creazione di helper HTML personalizzati: illustrano come creare l'helper HTML personalizzati creando un metodo statico e la creazione di un metodo di estensione.

## <a name="understanding-html-helpers"></a>Comprendere l'helper HTML

Un HTML Helper è semplicemente un metodo che restituisce una stringa. La stringa può rappresentare qualsiasi tipo di contenuto che si desidera. Ad esempio, è possibile utilizzare l'helper HTML per eseguire il rendering di tag HTML standard come HTML `<input>` e `<img>` tag. È inoltre possibile utilizzare helper HTML per il rendering del contenuto più complesso, ad esempio un elenco di schede o una tabella HTML di dati del database.

Il framework di MVC ASP.NET include il seguente set di standard helper HTML (ciò non è un elenco completo):

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Si consideri ad esempio il modulo nel listato 1. Questo modulo viene eseguito il rendering con l'aiuto di due degli helper HTML standard (vedere la figura 1). Questo modulo Usa la `Html.BeginForm()` e `Html.TextBox()` metodi Helper per il rendering di un semplice form HTML.


[![Pagina sottoposta a rendering con helper HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Figura 01**: pagina sottoposta a rendering con helper HTML ([fare clic per visualizzare l'immagine ingrandita](creating-custom-html-helpers-cs/_static/image3.png))


**Elenco 1: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Il metodo Helper Html.BeginForm() viene utilizzato per creare il codice HTML di apertura e chiusura `<form>` tag. Si noti che il `Html.BeginForm()` metodo viene chiamato all'interno di un utilizzo istruzione. L'istruzione using garantisce che il `<form>` tag viene chiuso alla fine della mediante blocco.

Se si preferisce, invece di creare un utilizzando blocco, è possibile chiamare il metodo di supporto Html.EndForm() per chiudere la `<form>` tag. Utilizzare qualsiasi approccio alla creazione di apertura e chiusura `<form>` tag apparentemente più intuitiva.

Il `Html.TextBox()` vengono utilizzati metodi Helper nel listato 1 per il rendering HTML `<input>` tag. Se si seleziona HTML nel browser viene visualizzato l'origine HTML nel listato 2. Si noti che l'origine contiene tag HTML standard.

> [!IMPORTANT]
> Si noti che il `Html.TextBox()`-HTML Helper viene eseguito il rendering con `<%= %>` tag anziché `<% %>` tag. Se non si include il segno di uguale, non viene eseguito nel browser.

Il framework ASP.NET MVC contiene un piccolo set di supporti. Molto probabilmente, è necessario estendere il framework MVC con helper HTML personalizzati. Nel resto di questa esercitazione, si apprenderà due metodi di creazione di helper HTML personalizzati.

**Elenco 2: `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Creazione di helper HTML con metodi statici

Il modo più semplice per creare un nuovo HTML Helper consiste nel creare un metodo statico che restituisce una stringa. Si supponga, ad esempio, che si decide di creare un nuovo HTML Helper che esegue il rendering HTML `<label>` tag. È possibile utilizzare la classe nel listato 2 per il rendering di un `<label>` .

**Elenco 2: `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Non c'è niente di speciale sulla classe listato 2. Il `Label()` metodo restituisce semplicemente una stringa.

La visualizzazione dell'indice modificata nel listato 3 Usa il `LabelHelper` per il rendering HTML `<label>` tag. Si noti che la visualizzazione include un `<%@ imports %>` direttiva che importa il `Application1.Helpers` dello spazio dei nomi.

**Elenco 2: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Creazione di helper HTML con metodi di estensione

Se si desidera creare helper HTML che funzionano solo come gli helper HTML standard incluso nel framework di MVC ASP.NET è necessario creare metodi di estensione. Metodi di estensione consentono di aggiungere nuovi metodi a una classe esistente. Quando si crea un metodo HTML Helper, aggiungere nuovi metodi alla classe HtmlHelper rappresentata dalla proprietà di una visualizzazione Html.

La classe listato 3 aggiunge un metodo di estensione per il `HtmlHelper` classe denominata `Label()`. Esistono un paio di aspetti che è opportuno notare su questa classe. In primo luogo, si noti che la classe è una classe statica. È necessario definire un metodo di estensione con una classe statica.

In secondo luogo, si noti che il primo parametro del `Label()` metodo è preceduto dalla parola chiave `this`. Il primo parametro di un metodo di estensione indica la classe che estende il metodo di estensione.

**Elenco di 3: `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Dopo aver creare un metodo di estensione e compilare correttamente l'applicazione, il metodo di estensione viene visualizzato in Visual Studio Intellisense, come tutti gli altri metodi di una classe (vedere la figura 2). L'unica differenza è che estensione metodi vengono visualizzati con un simbolo speciale accanto agli (un'icona di una freccia verso il basso).


[![Utilizzando il metodo di estensione Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Figura 02**: tramite il metodo di estensione Html.Label() ([fare clic per visualizzare l'immagine ingrandita](creating-custom-html-helpers-cs/_static/image6.png))


La visualizzazione dell'indice modificata listato 4 utilizza il metodo di estensione Html.Label() per eseguire il rendering di tutti i relativi `<label>` tag.

**Elenco di 4: `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato due metodi di creazione di helper HTML personalizzati. In primo luogo, si è appreso come creare una classe personalizzata `Label()` HTML Helper mediante la creazione di un metodo statico che restituisce una stringa. Successivamente, si è appreso come creare una classe personalizzata `Label()` metodo HTML Helper mediante la creazione di un metodo di estensione sulla `HtmlHelper` classe.

In questa esercitazione è incentrata sulla creazione di un metodo HTML Helper estremamente semplice. Tenere presente che un HTML Helper può essere complesso desiderato. È possibile compilare l'helper HTML che eseguono il rendering di contenuto complesso, ad esempio le visualizzazioni albero, menu o tabelle di dati di database.

> [!div class="step-by-step"]
> [Precedente](asp-net-mvc-views-overview-cs.md)
> [Successivo](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
