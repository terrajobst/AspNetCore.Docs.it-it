---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Aggiunta della convalida | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 22e59a99a179a7a414447036b973e3ad3b462515
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577951"
---
<a name="adding-validation"></a>Aggiunta della convalida
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In questa sezione si aggiungerà la logica di convalida per il `Movie` modello e si farà in modo che le regole di convalida vengono applicate ogni volta che un utente prova a creare o modificare un film tramite l'applicazione.

## <a name="keeping-things-dry"></a>Mantenere la trattazione DRY

Uno dei principi di progettazione fondamentali di ASP.NET MVC [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;non ' t Repeat Yourself&quot;). ASP.NET MVC consiglia di specificare la funzionalità o il comportamento una sola volta e quindi viene applicata ovunque in un'applicazione. Ciò riduce la quantità di codice che è necessario scrivere e rende il codice che scrive meno soggetto a errori e più facili da gestire.

Il supporto della convalida fornito da ASP.NET MVC ed Entity Framework Code First è un ottimo esempio del principio DRY in azione. È possibile specificare in modo dichiarativo le regole di convalida in un'unica posizione (nella classe del modello) e le regole vengono applicate ovunque nell'applicazione.

Si esaminerà come è possibile sfruttare il supporto per la convalida nell'applicazione di film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Aggiunta di regole di convalida al modello Movie

Iniziare aggiungendo una logica di convalida per il `Movie` classe.

Aprire il file *Movie.cs*. Si noti che il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi non contiene `System.Web`. DataAnnotations fornisce un set predefinito di attributi di convalida che è possibile applicare in modo dichiarativo a qualsiasi classe o proprietà. (Include anche gli attributi di formattazione, ad esempio [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) che guidano nella formattazione e non forniscono alcuna convalida.)

A questo punto aggiornare i `Movie` classe possa sfruttare i vantaggi di integrato [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), e [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) gli attributi di convalida. Sostituire il `Movie` classe con quanto segue:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

Il [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributo imposta la lunghezza massima della stringa e imposta questa limitazione sul database, pertanto verrà modificato lo schema del database. Fare clic con il pulsante destro sul **film** nella tabella **Esplora Server** e fare clic su **Apri definizione tabella**:

![](adding-validation/_static/image1.png)

Nell'immagine precedente, è possibile visualizzare tutti i campi stringa vengono impostati su [NVARCHAR (MAX)](https://technet.microsoft.com/library/ms186939.aspx). Si userà migrations per aggiornare lo schema. Compilare la soluzione e quindi aprire il **Console di gestione pacchetti** finestra e immettere i comandi seguenti:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` classe derivata con il nome specificato (`DataAnnotations`) e nel `Up` metodo è possibile visualizzare il codice che aggiorna i vincoli dello schema:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

Il `Genre` campo ammette valori null non è più (vale a dire, è necessario immettere un valore). Il `Rating` campo ha una lunghezza massima pari a 5 e `Title` ha una lunghezza massima pari a 60. La lunghezza minima pari a 3 nella `Title` e l'intervallo su `Price` non ha creato le modifiche dello schema.

Esaminare lo schema di film:

![](adding-validation/_static/image2.png)

I campi stringa mostrano i nuovi limiti di lunghezza e `Genre` non verrà più archiviato come nullable.

Gli attributi di convalida specificano il comportamento da applicare per le proprietà del modello a cui vengono applicati. Gli attributi `Required` e `MinimumLength` indicano che una proprietà deve avere un valore, ma nulla impedisce all'utente di inserire spazi vuoti per soddisfare questa convalida. Il [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attributo viene usato per limitare i caratteri che possono essere di input. Nel codice precedente, per `Genre` e `Rating` è necessario usare solo lettere (spazi, numeri e caratteri speciali non consentiti). Il [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attributo vincola un valore all'interno di un intervallo specificato. L'attributo `StringLength` consente di impostare la lunghezza massima di una proprietà stringa e, facoltativamente, la lunghezza minima. I tipi di valore (ad esempio `decimal, int, float, DateTime`) sono intrinsecamente necessari e non richiedono il `Required` attributo.

Codice, in primo luogo, assicura che le regole di convalida che si specifica in una classe di modello vengono applicate prima che l'applicazione deve salvare le modifiche nel database. Ad esempio, il codice seguente genera una [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) eccezione quando il `SaveChanges` viene chiamato il metodo in quanto alcuni richiesta `Movie` mancano i valori delle proprietà:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Il codice riportato sopra, verrà generata l'eccezione seguente:

*Convalida non riuscita per uno o più entità. Vedere la proprietà 'EntityValidationErrors' per altri dettagli.*

Con le regole di convalida applicate automaticamente da .NET Framework consente di rendere l'applicazione più affidabile. In questo modo inoltre non è possibile omettere la convalida di un elemento e quindi inserire involontariamente dati errati nel database.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Errore di convalida dell'interfaccia utente in ASP.NET MVC

Eseguire l'applicazione e passare al */Movies* URL.

Scegliere il **Crea nuovo** collegamento per aggiungere un nuovo film. Completare il modulo con alcuni valori non validi. Non appena la convalida del lato client jQuery rileva l'errore, viene visualizzato un messaggio di errore.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> per supportare la convalida di jQuery per impostazioni locali di lingua diversa dall'inglese che usano la virgola (",") per un separatore decimale, è necessario includere il NuGet globalizzare come descritto in precedenza in questa esercitazione.


Si noti come il form è automaticamente usavano un colore bordo rosso per evidenziare le caselle di testo che contengono dati non validi e ha generato un messaggio di errore di convalida appropriato accanto a ciascuna di esse. Gli errori vengono applicati sia sul lato client (utilizzo di JavaScript e jQuery) sia sul lato server (nel caso di un utente con JavaScript disabilitato).

Dei vantaggi reale è che non è necessario modificare una singola riga di codice nel `MoviesController` classe o nel *create. cshtml* visualizzazione per consentire la convalida dell'interfaccia utente. Il controller e le viste creati in una fase precedente di questa esercitazione hanno selezionato automaticamente le regole di convalida specificate usando gli attributi di convalida delle proprietà della classe `Movie` del modello. Eseguire il test della convalida usando il metodo di azione `Edit` e viene applicata la stessa convalida.

I dati del modulo non vengono inviati al server fino a quando non sono più presenti errori di convalida sul lato client. È possibile verificarlo inserendo un punto di interruzione nel metodo HTTP Post, utilizzando il [strumento fiddler](http://fiddler2.com/fiddler2/), o l'inserimento/espulsione [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>La modalità di convalida viene eseguita nella creazione consente di visualizzare e creare il metodo di azione

Ci si potrebbe chiedere come la convalida dell'interfaccia utente sia stata generata senza aggiornamenti al codice nel controller o nelle viste. Il listato successivo illustra ciò che il `Create` metodi nel `MovieController` classi simili. Sono cambiati rispetto a come si creati in precedenza in questa esercitazione.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

Il primo metodo di azione (HTTP GET) `Create` visualizza il modulo di creazione iniziale. La seconda versione (`[HttpPost]`) gestisce l'invio del modulo. Il secondo metodo `Create` (la versione `HttpPost`) chiama `ModelState.IsValid` per verificare se esistono errori di convalida per il film. La chiamata a questo metodo valuta tutti gli attributi di convalida applicati all'oggetto. Se l'oggetto presenta errori di convalida, il metodo `Create` visualizza di nuovo il modulo. Se non sono presenti errori, il metodo salva il nuovo film nel database. Nell'esempio movie **il modulo non viene inviato al server quando sono presenti errori di convalida rilevati sul lato client; il secondo** `Create` **metodo non viene mai chiamato**. Se si disabilita JavaScript nel browser, la convalida del client è disabilitata e la richiesta HTTP POST `Create` chiamate al metodo `ModelState.IsValid` per verificare se il film è errori di convalida.

È possibile impostare un punto di interruzione nel metodo `HttpPost Create` e verificare che il metodo non venga mai chiamato, la convalida sul lato client non invierà i dati del modulo in caso di rilevamento di errori di convalida. Se si disabilita JavaScript nel browser, quindi si invia il modulo con errori, verrà raggiunto il punto di interruzione. Si ottiene comunque la convalida completa senza JavaScript. La figura seguente viene illustrato come disabilitare JavaScript in Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

La figura seguente illustra come disabilitare JavaScript nel browser FireFox.

![](adding-validation/_static/image7.png)

La figura seguente illustra come disabilitare JavaScript nel browser Chrome.

![](adding-validation/_static/image8.png)

Di seguito è riportato il *create. cshtml* modello di visualizzazione che è stato eseguito lo scaffolding in precedenza nell'esercitazione. Viene usata dai metodi di azione illustrati in precedenza per visualizzare il modulo iniziale e per visualizzarlo nuovamente in caso di errore.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Si noti come il codice usa un' `Html.EditorFor` helper per restituire il `<input>` per ogni elemento `Movie` proprietà. Accanto a questo helper è una chiamata al `Html.ValidationMessageFor` metodo helper. Questi due metodi di supporto di lavoro con l'oggetto modello che viene passato dal controller alla vista (in questo caso, un `Movie` oggetto). Risultino automaticamente per gli attributi di convalida specificati nei messaggi di errore di modello e la visualizzazione come appropriato.

L'aspetto molto interessante di questo approccio è che né il controller né il modello di vista `Create` sono consapevoli delle regole di convalida effettive applicate o dei messaggi di errore specifici visualizzati. Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`. Le stesse regole di convalida vengono applicate automaticamente alla vista `Edit` e ad altri modelli di vista eventualmente creati che modificano il modello.

Se si desidera modificare la logica di convalida in un secondo momento, è possibile farlo in una posizione tramite l'aggiunta di attributi di convalida al modello (in questo esempio, il `movie` classe). Non è necessario preoccuparsi dell'incoerenza delle diverse parti dell'applicazione con la modalità di applicazione delle regole perché tutta la logica di convalida verrà definita in un'unica posizione e usata ovunque. In questo modo il codice rimane molto pulito e facile da gestire e sviluppare. E ciò significa che sarà ampiamente rispettato il *DRY* principio.

## <a name="using-datatype-attributes"></a>Utilizzo degli attributi DataType

Aprire il file *Movie.cs* ed esaminare la classe `Movie`. Il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi fornisce gli attributi di formattazione oltre al set predefinito di attributi di convalida. È già stato applicato un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valore di enumerazione per la data di rilascio e per i campi relativi ai prezzi. Il codice seguente illustra il `ReleaseDate` e `Price` delle proprietà con l'appropriato [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributi forniscono solo gli hint per il motore di vista formatti i dati (e fornisca gli attributi, ad esempio `<a>` per gli URL e `<a href="mailto:EmailAddress.com">` per la posta elettronica. È possibile usare la [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attributo da convalidare il formato dei dati. Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo viene usato per specificare un tipo di dati che è più specifico del tipo intrinseco del database, sono ***non*** gli attributi di convalida. In questo caso si vuole tenere traccia solo della data e non di data e ora. Il [enumerazione DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornisce per molti tipi di dati, ad esempio *Date, Time, PhoneNumber, Currency, EmailAddress* e altro ancora. L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo. Ad esempio, un `mailto:` possibile creare un collegamento per [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), e un selettore data può essere fornito per [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) nei browser che supportano [HTML5](http://html5.org/). Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributi emette HTML 5 [data -](http://ejohn.org/blog/html-5-data-attributes/) (si pronuncia *dash di dati*) gli attributi HTML browser HTML5. Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributi non forniscono alcuna convalida.

`DataType.Date` non specifica il formato della data visualizzata. Per impostazione predefinita, viene visualizzato il campo dati in base ai formati predefiniti del server [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


Il `ApplyFormatInEditMode` impostazione specifica che la formattazione specificata deve essere applicata anche quando il valore viene visualizzato in una casella di testo per la modifica. (Non è consigliabile che per alcuni campi, ad esempio, per i valori di valuta, è possibile evitare il simbolo di valuta nella casella di testo per la modifica.)

È possibile usare la [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo da solo, ma in genere è consigliabile utilizzare il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) anche dell'attributo. Il `DataType` attributo fornisce il *semantica* dei dati come rispetto alla modalità di rendering in una schermata e offre i vantaggi seguenti che non si ottengono con `DisplayFormat`:

- Il browser può abilitare le funzionalità HTML5 (ad esempio per visualizzare un controllo di calendario, il simbolo della valuta appropriato delle impostazioni locali, collegamenti di posta elettronica, e così via.).
- Per impostazione predefinita, il browser eseguirà il rendering dei dati usando il formato corretto in base il [delle impostazioni locali](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo consente di abilitare MVC scegliere il modello di campo corretto per il rendering dei dati (la [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) se usato da se stessa Usa il modello di stringa). Per altre informazioni, vedere Brad Wilson [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Anche se scritte per MVC 2, in questo articolo ancora si applica alla versione corrente di ASP.NET MVC.)

Se si usa la `DataType` attributo con un campo Data, è necessario specificare il `DisplayFormat` attributi anche per garantire che il campo correttamente il rendering nel browser Chrome. Per altre informazioni, vedere [thread di StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> convalida jQuery non funziona con il [Range](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attributo e [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Ad esempio, il codice seguente visualizzerà sempre un errore di convalida sul lato client, anche quando la data è compreso nell'intervallo specificato:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> È necessario disabilitare la convalida della data jQuery per usare il [Range](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) dell'attributo con [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). In genere non è consigliabile compilare date reali nei modelli, usando il [Range](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attributo e [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx) è sconsigliato.


Il codice seguente illustra la combinazione di attributi in una sola riga:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

Nella parte successiva della serie verrà esaminata l'applicazione e verranno apportati alcuni miglioramenti ai metodi `Details` e `Delete` generati automaticamente.

> [!div class="step-by-step"]
> [Precedente](adding-a-new-field.md)
> [Successivo](examining-the-details-and-delete-methods.md)
