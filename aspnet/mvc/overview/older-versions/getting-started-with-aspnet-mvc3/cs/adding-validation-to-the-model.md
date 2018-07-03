---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: Aggiunta della convalida al modello (c#) | Microsoft Docs
author: Rick-Anderson
description: Creazione di un Controller
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: bf0a0d96ed41d69f4baa693d35a1a08ef62866c8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388659"
---
<a name="adding-validation-to-the-model-c"></a>Aggiunta della convalida al modello (c#)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.
> 
> 
> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare al [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.


In questa sezione si aggiungerà la logica di convalida per il `Movie` modello e si farà in modo che le regole di convalida vengono applicate ogni volta che un utente prova a creare o modificare un film tramite l'applicazione.

## <a name="keeping-things-dry"></a>Mantenere la trattazione DRY

Uno dei principi di progettazione fondamentali di ASP.NET MVC è DRY ("t Repeat Yourself"). ASP.NET MVC consiglia di specificare la funzionalità o il comportamento una sola volta e quindi viene applicata ovunque in un'applicazione. Ciò riduce la quantità di codice che è necessario scrivere e il codice che scritto molto più semplice da gestire.

Il supporto della convalida fornito da ASP.NET MVC ed Entity Framework Code First è un ottimo esempio del principio DRY in azione. È possibile specificare in modo dichiarativo le regole di convalida in un'unica posizione (nella classe del modello) e quindi tali regole vengono applicate ovunque nell'applicazione.

Si esaminerà come è possibile sfruttare il supporto per la convalida nell'applicazione di film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Aggiunta di regole di convalida al modello Movie

Iniziare aggiungendo una logica di convalida per il `Movie` classe.

Aprire il file *Movie.cs*. Aggiungere un `using` all'inizio del file che fa riferimento l'istruzione il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Lo spazio dei nomi fa parte di .NET Framework. Fornisce un set predefinito di attributi di convalida che è possibile applicare in modo dichiarativo a qualsiasi classe o proprietà.

A questo punto aggiornare i `Movie` classe possa sfruttare i vantaggi di integrato [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), e [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) gli attributi di convalida . Usare il codice seguente come esempio in cui applicare gli attributi.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Gli attributi di convalida specificano il comportamento da applicare per le proprietà del modello a cui vengono applicati. Il `Required` attributo indica che una proprietà deve avere un valore; in questo esempio, un filmato deve avere i valori per il `Title`, `ReleaseDate`, `Genre`, e `Price` proprietà affinché sia valida. L'attributo `Range` vincola un valore all'interno di un intervallo specificato. L'attributo `StringLength` consente di impostare la lunghezza massima di una proprietà stringa e, facoltativamente, la lunghezza minima.

Codice, in primo luogo, assicura che le regole di convalida che si specifica in una classe di modello vengono applicate prima che l'applicazione deve salvare le modifiche nel database. Ad esempio, il codice seguente verrà generata un'eccezione quando la `SaveChanges` viene chiamato il metodo in quanto alcuni richiesta `Movie` mancano i valori delle proprietà e il prezzo è zero (che è compreso nell'intervallo valido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

Con le regole di convalida applicate automaticamente da .NET Framework consente di rendere l'applicazione più affidabile. In questo modo inoltre non è possibile omettere la convalida di un elemento e quindi inserire involontariamente dati errati nel database.

Ecco un elenco di codice per l'aggiornamento *Movie.cs* file:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Errore di convalida dell'interfaccia utente in ASP.NET MVC

Eseguire nuovamente l'applicazione e passare al */Movies* URL.

Scegliere il **film creare** collegamento per aggiungere un nuovo film. Compilare il modulo con alcuni valori non validi e quindi scegliere il **Create** pulsante.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Si noti come il modulo automaticamente ha usato un colore di sfondo per evidenziare le caselle di testo che contengono dati non validi e ha generato un messaggio di errore di convalida appropriato accanto a ciascuna di esse. I messaggi di errore corrispondono alle stringhe di errore è stato specificato quando è annotata la `Movie` classe. Gli errori vengono applicati entrambi (tramite JavaScript) sul lato client e lato server (nel caso in cui un utente ha JavaScript disabilitato).

Dei vantaggi reale è che non è necessario modificare una singola riga di codice nel `MoviesController` classe o nel *create. cshtml* visualizzazione per consentire la convalida dell'interfaccia utente. Il controller e viste create automaticamente in precedenza in questa esercitazione hanno selezionato le regole di convalida specificato utilizzando gli attributi nel `Movie` classe del modello.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>La modalità di convalida viene eseguita nella creazione consente di visualizzare e creare il metodo di azione

Ci si potrebbe chiedere come la convalida dell'interfaccia utente sia stata generata senza aggiornamenti al codice nel controller o nelle viste. Il listato successivo illustra ciò che il `Create` metodi nel `MovieController` classi simili. Sono cambiati rispetto a come si creati in precedenza in questa esercitazione.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

Il primo metodo di azione consente di visualizzare il modulo di creazione iniziale. La seconda gestisce il post del form. La seconda `Create` chiamate al metodo `ModelState.IsValid` per verificare se il film è errori di convalida. La chiamata a questo metodo valuta tutti gli attributi di convalida applicati all'oggetto. Se l'oggetto presenta errori di convalida, il `Create` metodo rivisualizza il form. Se non sono presenti errori, il metodo salva il nuovo film nel database.

Di seguito è riportato il *create. cshtml* modello di visualizzazione che è stato eseguito lo scaffolding in precedenza nell'esercitazione. Viene usata dai metodi di azione illustrati in precedenza per visualizzare il modulo iniziale e per visualizzarlo nuovamente in caso di errore.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Si noti come il codice usa un' `Html.EditorFor` helper per restituire il `<input>` per ogni elemento `Movie` proprietà. Accanto a questo helper è una chiamata al `Html.ValidationMessageFor` metodo helper. Questi due metodi di supporto di lavoro con l'oggetto modello che viene passato dal controller alla vista (in questo caso, un `Movie` oggetto). Risultino automaticamente per gli attributi di convalida specificati nei messaggi di errore di modello e la visualizzazione come appropriato.

Che cos'è ottimo su questo approccio è che il controller né il modello di vista crea consapevoli sulle regole di convalida effettiva viene applicate o sui messaggi di errore specifici visualizzati. Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`.

Se si desidera modificare la logica di convalida in un secondo momento, è possibile farlo in una posizione. Non è necessario preoccuparsi dell'incoerenza delle diverse parti dell'applicazione con la modalità di applicazione delle regole perché tutta la logica di convalida verrà definita in un'unica posizione e usata ovunque. In questo modo il codice rimane molto pulito e facile da gestire e sviluppare. Il principio DRY sarà ampiamente rispettato.

## <a name="adding-formatting-to-the-movie-model"></a>Aggiunta di formattazione al modello Movie

Aprire il file *Movie.cs*. Il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi fornisce gli attributi di formattazione oltre al set predefinito di attributi di convalida. Verrà applicata il [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo e un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valore di enumerazione per la data di rilascio e per i campi relativi ai prezzi. Il codice seguente illustra il `ReleaseDate` e `Price` delle proprietà con l'appropriato [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

In alternativa, è possibile impostare in modo esplicito un [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valore. Il codice seguente illustra la proprietà data di rilascio con una stringa di formato data (vale a dire, "d"). Si utilizzerà per specificare che non si vuole ora come parte della data di rilascio.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

Nell'esempio di codice formati il `Price` proprietà come valuta.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

L'intero `Movie` classe è illustrata di seguito.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Eseguire l'applicazione e individuare il `Movies` controller.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Nella parte successiva della serie verrà esaminata l'applicazione e verranno apportati alcuni miglioramenti ai metodi `Details` e `Delete` generati automaticamente.

> [!div class="step-by-step"]
> [Precedente](adding-a-new-field.md)
> [Successivo](improving-the-details-and-delete-methods.md)
