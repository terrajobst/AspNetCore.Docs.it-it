---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: Aggiunta della convalida per il modello (VB) | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: a58b4a4893fca66800c012bebae4a8bbfedf7a6a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-to-the-model-vb"></a>Aggiunta della convalida per il modello (VB)
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)
> 
> Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente VB.NET complemento è disponibile in questo argomento. [Scaricare la versione di Visual Basic.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare il [c# versione](../cs/adding-validation-to-the-model.md) di questa esercitazione.


In questa sezione verrà aggiunta la logica di convalida per il `Movie` modello e si farà in modo che le regole di convalida vengono applicate ogni volta che un utente cerca di creare o modificare un filmato utilizzando l'applicazione.

## <a name="keeping-things-dry"></a>Mantenere la secca

Uno dei principi di progettazione fondamentali di ASP.NET MVC è sorgente ("non ripetere manualmente"). ASP.NET MVC incoraggia gli utenti a specificare funzionalità o il comportamento una sola volta e quindi fare in modo riflettersi ovunque in un'applicazione. Ciò riduce la quantità di codice, che è necessario scrivere e rende il codice che scritto molto più semplice per mantenere.

Il supporto della convalida fornito da ASP.NET MVC e Code First di Entity Framework è un ottimo esempio del principio secco nell'azione. È possibile specificare in modo dichiarativo le regole di convalida in un punto (la classe di modello) e quindi vengono applicate le regole ovunque nell'applicazione.

Vediamo come è possibile sfruttare il supporto della convalida nell'applicazione film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Aggiunta di regole di convalida per il modello di film

Iniziare con l'aggiunta di logica di convalida per il `Movie` classe.

Aprire il *Movie.vb* file. Aggiungere un `Imports` istruzione all'inizio del file che fa riferimento il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi:

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

Lo spazio dei nomi fa parte di .NET Framework. Fornisce un set predefinito di attributi di convalida che è possibile applicare in modo dichiarativo a qualsiasi classe o proprietà.

A questo punto aggiornare il `Movie` classe per poter sfruttare predefinito [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), e [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) gli attributi di convalida . Utilizzare il codice seguente ad esempio in cui applicare gli attributi.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Gli attributi di convalida specificano il comportamento da applicare per le proprietà del modello a cui vengono applicati. Il `Required` attributo indica che una proprietà deve avere un valore; in questo esempio, un film deve disporre di valori per il `Title`, `ReleaseDate`, `Genre`, e `Price` le proprietà per essere valido. L'attributo `Range` vincola un valore all'interno di un intervallo specificato. L'attributo `StringLength` consente di impostare la lunghezza massima di una proprietà stringa e, facoltativamente, la lunghezza minima.

Codice innanzitutto assicura che le regole di convalida specificate in una classe di modello vengono applicate prima che l'applicazione salva le modifiche nel database. Ad esempio, il codice riportato di seguito verrà generata un'eccezione quando il `SaveChanges` metodo viene chiamato, perché è più necessario `Movie` mancano i valori delle proprietà e il prezzo è zero (che è compreso nell'intervallo valido).

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

Con le regole di convalida viene applicate automaticamente da .NET Framework consente di rendere l'applicazione più affidabile. In questo modo inoltre non è possibile omettere la convalida di un elemento e quindi inserire involontariamente dati errati nel database.

Ecco un elenco di codice per l'aggiornamento *Movie.vb* file:

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Errore di convalida dell'interfaccia utente in ASP.NET MVC

Eseguire nuovamente l'applicazione e passare il */Movies* URL.

Fare clic su di **creare film** collegamento per aggiungere un nuovo film. Compilare il modulo con alcuni valori non validi e quindi fare clic su di **crea** pulsante.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Si noti come il modulo automaticamente ha utilizzato un colore di sfondo per evidenziare le caselle di testo che contengono dati non validi e ha generato un messaggio di errore di convalida appropriato accanto a ciascuna di esse. I messaggi di errore corrispondono alle stringhe di errore specificato quando è annotata la `Movie` classe. Gli errori vengono applicati entrambi (tramite JavaScript) sul lato client e lato server (nel caso in cui un utente dispone di JavaScript disabilitato).

Un vantaggio è che non è necessario modificare una singola riga di codice nel `MoviesController` classe oppure il *Create.vbhtml* vista per permettere la convalida dell'interfaccia utente. Prelevato il controller e visualizzazioni create in precedenza in questa esercitazione automaticamente le regole di convalida specificato utilizzando gli attributi sulla `Movie` classe del modello.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Come la convalida viene eseguita Create a visualizzare e creare il metodo di azione

Ci si potrebbe chiedere come la convalida dell'interfaccia utente sia stata generata senza aggiornamenti al codice nel controller o nelle viste. La voce successiva vengono mostrate le operazioni di `Create` metodi nel `MovieController` classe è simile a. Ma sono cambiati rispetto a come si creati precedentemente in questa esercitazione.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

Il primo metodo di azione consente di visualizzare il modulo Crea iniziale. La seconda gestisce il post del form. Il secondo `Create` chiamate al metodo `ModelState.IsValid` per verificare se il film è gli errori di convalida. La chiamata a questo metodo valuta tutti gli attributi di convalida applicati all'oggetto. Se l'oggetto presenta errori di convalida, il `Create` metodo rivisualizzata il form. Se non sono presenti errori, il metodo salva il nuovo film nel database.

Di seguito è riportato il *Create.vbhtml* modello di visualizzazione che scaffolding precedenza nell'esercitazione. Viene usata dai metodi di azione illustrati in precedenza per visualizzare il modulo iniziale e per visualizzarlo nuovamente in caso di errore.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Si noti che il codice utilizza un `Html.EditorFor` helper per l'output di `<input>` per ogni elemento `Movie` proprietà. Accanto a questo supporto è presente una chiamata al `Html.ValidationMessageFor` metodo di supporto. Questi due metodi di supporto di lavoro con l'oggetto modello che viene passato dal controller per la vista (in questo caso, un `Movie` oggetto). Vengano visualizzate automaticamente per gli attributi di convalida specificati nei messaggi di errore di modello e la visualizzazione come appropriato.

Che cos'è davvero utili su questo approccio è che il controller né il modello di visualizzazione crea sappia alcuna operazione sulle regole di convalida effettiva imposizione o sui messaggi di errore specifico visualizzati. Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`.

Se si desidera modificare la logica di convalida in un secondo momento, è possibile farlo in esattamente un'unica posizione. Non è necessario preoccuparsi dell'incoerenza delle diverse parti dell'applicazione con la modalità di applicazione delle regole perché tutta la logica di convalida verrà definita in un'unica posizione e usata ovunque. In questo modo il codice rimane molto pulito e facile da gestire e sviluppare. E significa che sarà completamente rispettando la distinzione tra il principio secco.

## <a name="adding-formatting-to-the-movie-model"></a>Aggiunta di formattazione per il modello di film

Aprire il *Movie.vb* file. Il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) spazio dei nomi fornisce gli attributi di formattazione oltre al set di attributi di convalida predefinito. Si applicherà il [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo e un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) il valore di enumerazione per la data di rilascio e per i campi dei prezzi. Il codice seguente illustra il `ReleaseDate` e `Price` proprietà con l'appropriato [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

In alternativa, è possibile impostare in modo esplicito un [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valore. Il codice seguente viene illustrata la proprietà di data di rilascio con una stringa di formato data (vale a dire, "d"). Si utilizzerà per specificare che non si desidera ora come parte della data di rilascio.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

Nell'esempio di codice formati di `Price` proprietà come valuta.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

L'intero `Movie` classe è illustrata di seguito.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Eseguire l'applicazione e individuare il `Movies` controller.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Nella parte successiva della serie, viene esaminare l'applicazione e apportare alcuni miglioramenti alla generato automaticamente `Details` e `Delete` metodi...

>[!div class="step-by-step"]
[Precedente](adding-a-new-field.md)
[Successivo](improving-the-details-and-delete-methods.md)
