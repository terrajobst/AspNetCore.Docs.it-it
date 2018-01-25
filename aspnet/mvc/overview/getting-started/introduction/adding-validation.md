---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Aggiunta della convalida | Documenti Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: b59965b2fab00cb64db06574d5ca3c6388daa7c8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation"></a>Aggiunta della convalida
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

In questo in questa sezione si aggiungerà la logica di convalida per il `Movie` modello e si farà in modo che le regole di convalida vengono applicate ogni volta che un utente cerca di creare o modificare un filmato utilizzando l'applicazione.

## <a name="keeping-things-dry"></a>Mantenere la secca

Uno dei principi di progettazione fondamentali di ASP.NET MVC è [secca](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;non ripetere manualmente&quot;). ASP.NET MVC incoraggia gli utenti a specificare funzionalità o il comportamento una sola volta e quindi fare in modo riflettersi ovunque in un'applicazione. Ciò riduce la quantità di codice, che è necessario scrivere e rende il codice che scritto meno errore soggetto e facile da gestire.

Il supporto della convalida fornito da ASP.NET MVC e Code First di Entity Framework è un ottimo esempio del principio secco nell'azione. È possibile specificare in modo dichiarativo le regole di convalida in un'unica posizione (modello) nella classe e le regole vengono applicate ovunque nell'applicazione.

Vediamo come è possibile sfruttare il supporto della convalida nell'applicazione film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Aggiunta di regole di convalida per il modello di film

Iniziare con l'aggiunta di logica di convalida per il `Movie` classe.

Aprire il file *Movie.cs*. Si noti il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi non contiene `System.Web`. DataAnnotations fornisce un set predefinito di attributi di convalida che è possibile applicare in modo dichiarativo a qualsiasi classe o proprietà. (Contiene anche gli attributi di formattazione come [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) che Guida in linea con la formattazione e non forniscono alcuna convalida.)

A questo punto aggiornare il `Movie` classe per poter sfruttare predefinito [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), e [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) gli attributi di convalida. Sostituire il `Movie` classe con le operazioni seguenti:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

Il [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributo imposta la lunghezza massima della stringa e imposta questa limitazione per il database, pertanto cambia lo schema del database. Fare clic con il pulsante destro sul **filmati** tabella **Esplora Server** e fare clic su **Apri definizione tabella**:

![](adding-validation/_static/image1.png)

Nell'immagine precedente, è possibile visualizzare tutti i campi stringa vengono impostati su [NVARCHAR (MAX)](https://technet.microsoft.com/library/ms186939.aspx). Si utilizzerà le migrazioni per aggiornare lo schema. Compilare la soluzione e quindi aprire il **Package Manager Console** finestra e immettere i comandi seguenti:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` classe derivata con il nome specificato (`DataAnnotations`) e il `Up` metodo è possibile visualizzare il codice che aggiorna i vincoli dello schema:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

Il `Genre` campo sono non nullable (ovvero, è necessario immettere un valore). Il `Rating` campo ha una lunghezza massima di 5 e `Title` ha una lunghezza massima di 60. La lunghezza minima di 3 su `Title` e l'intervallo su `Price` non ha creato le modifiche dello schema.

Esaminare lo schema del filmato:

![](adding-validation/_static/image2.png)

I campi stringa mostrano i nuovi limiti di lunghezza e `Genre` non viene più controllata come nullable.

Gli attributi di convalida specificano il comportamento da applicare per le proprietà del modello a cui vengono applicati. Gli attributi `Required` e `MinimumLength` indicano che una proprietà deve avere un valore, ma nulla impedisce all'utente di inserire spazi vuoti per soddisfare questa convalida. Il [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attributo viene utilizzato per limitare i caratteri che possono essere di input. Nel codice precedente, per `Genre` e `Rating` è necessario usare solo lettere (spazi, numeri e caratteri speciali non consentiti). Il [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attributo vincola un valore all'interno di un intervallo specificato. L'attributo `StringLength` consente di impostare la lunghezza massima di una proprietà stringa e, facoltativamente, la lunghezza minima. I tipi di valore (ad esempio `decimal, int, float, DateTime`) sono intrinsecamente necessari e non è necessaria la `Required` attributo.

Codice innanzitutto assicura che le regole di convalida specificate in una classe di modello vengono applicate prima che l'applicazione salva le modifiche nel database. Ad esempio, il codice seguente genera un [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) eccezione quando il `SaveChanges` metodo viene chiamato, perché è più necessario `Movie` mancano i valori delle proprietà:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Il codice precedente, verrà generata l'eccezione seguente:

*Convalida non riuscita per uno o più entità. La proprietà 'EntityValidationErrors' per ulteriori informazioni, vedere.*

Con le regole di convalida viene applicate automaticamente da .NET Framework consente di rendere l'applicazione più affidabile. In questo modo inoltre non è possibile omettere la convalida di un elemento e quindi inserire involontariamente dati errati nel database.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Errore di convalida dell'interfaccia utente in ASP.NET MVC

Eseguire l'applicazione e passare il */Movies* URL.

Fare clic su di **Crea nuovo** collegamento per aggiungere un nuovo film. Completare il modulo con alcuni valori non validi. Non appena la convalida del lato client jQuery rileva l'errore, viene visualizzato un messaggio di errore.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> per supportare la convalida jQuery per inglesi che utilizzano una virgola (",") per un punto decimale, è necessario includere NuGet globalizzare come descritto in precedenza in questa esercitazione.


Si noti come il modulo è automaticamente utilizzato il colore del bordo rosso per evidenziare le caselle di testo che contengono dati non validi e ha generato un messaggio di errore di convalida appropriato accanto a ciascuna di esse. Gli errori vengono applicati sia sul lato client (utilizzo di JavaScript e jQuery) sia sul lato server (nel caso di un utente con JavaScript disabilitato).

Un vantaggio è che non è necessario modificare una singola riga di codice nel `MoviesController` classe oppure il *Create.cshtml* vista per permettere la convalida dell'interfaccia utente. Il controller e le viste creati in una fase precedente di questa esercitazione hanno selezionato automaticamente le regole di convalida specificate usando gli attributi di convalida delle proprietà della classe `Movie` del modello. Eseguire il test della convalida usando il metodo di azione `Edit` e viene applicata la stessa convalida.

I dati del modulo non vengono inviati al server fino a quando non sono più presenti errori di convalida sul lato client. È possibile verificare questa inserendo un punto di interruzione nel metodo HTTP Post, utilizzando il [strumento fiddler](http://fiddler2.com/fiddler2/), Internet Explorer o [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Come la convalida viene eseguita Create a visualizzare e creare il metodo di azione

Ci si potrebbe chiedere come la convalida dell'interfaccia utente sia stata generata senza aggiornamenti al codice nel controller o nelle viste. La voce successiva vengono mostrate le operazioni di `Create` metodi nel `MovieController` classe è simile a. Ma sono cambiati rispetto a come si creati precedentemente in questa esercitazione.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

Il primo metodo di azione (HTTP GET) `Create` visualizza il modulo di creazione iniziale. La seconda versione (`[HttpPost]`) gestisce l'invio del modulo. Il secondo metodo `Create` (la versione `HttpPost`) chiama `ModelState.IsValid` per verificare se esistono errori di convalida per il film. La chiamata a questo metodo valuta tutti gli attributi di convalida applicati all'oggetto. Se l'oggetto presenta errori di convalida, il metodo `Create` visualizza di nuovo il modulo. Se non sono presenti errori, il metodo salva il nuovo film nel database. Nell'esempio movie **il modulo non viene registrato nel server quando sono presenti errori di convalida rilevati sul lato client; il secondo** `Create` **non viene mai chiamato**. Se si disabilita JavaScript nel browser, la convalida del client è disabilitato e HTTP POST `Create` chiamate al metodo `ModelState.IsValid` per verificare se il film è gli errori di convalida.

È possibile impostare un punto di interruzione nel metodo `HttpPost Create` e verificare che il metodo non venga mai chiamato, la convalida sul lato client non invierà i dati del modulo in caso di rilevamento di errori di convalida. Se si disabilita JavaScript nel browser, quindi si invia il modulo con errori, verrà raggiunto il punto di interruzione. Si ottiene comunque la convalida completa senza JavaScript. Nella figura seguente viene illustrato come disabilitare JavaScript in Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

La figura seguente illustra come disabilitare JavaScript nel browser FireFox.

![](adding-validation/_static/image7.png)

La figura seguente illustra come disabilitare JavaScript nel browser Chrome.

![](adding-validation/_static/image8.png)

Di seguito è riportato il *Create.cshtml* modello di visualizzazione che scaffolding precedenza nell'esercitazione. Viene usata dai metodi di azione illustrati in precedenza per visualizzare il modulo iniziale e per visualizzarlo nuovamente in caso di errore.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Si noti che il codice utilizza un `Html.EditorFor` helper per l'output di `<input>` per ogni elemento `Movie` proprietà. Accanto a questo supporto è presente una chiamata al `Html.ValidationMessageFor` metodo di supporto. Questi due metodi di supporto di lavoro con l'oggetto modello che viene passato dal controller per la vista (in questo caso, un `Movie` oggetto). Vengano visualizzate automaticamente per gli attributi di convalida specificati nei messaggi di errore di modello e la visualizzazione come appropriato.

L'aspetto molto interessante di questo approccio è che né il controller né il modello di vista `Create` sono consapevoli delle regole di convalida effettive applicate o dei messaggi di errore specifici visualizzati. Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`. Le stesse regole di convalida vengono applicate automaticamente alla vista `Edit` e ad altri modelli di vista eventualmente creati che modificano il modello.

Se si desidera modificare la logica di convalida in un secondo momento, è possibile farlo in esattamente un'unica posizione tramite l'aggiunta di attributi di convalida per il modello (in questo esempio, la `movie` classe). Non è necessario preoccuparsi dell'incoerenza delle diverse parti dell'applicazione con la modalità di applicazione delle regole perché tutta la logica di convalida verrà definita in un'unica posizione e usata ovunque. In questo modo il codice rimane molto pulito e facile da gestire e sviluppare. E ciò significa che è possibile essere completamente rispettando la distinzione tra il *secca* principio.

## <a name="using-datatype-attributes"></a>Utilizzo degli attributi DataType

Aprire il file *Movie.cs* ed esaminare la classe `Movie`. Il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) spazio dei nomi fornisce gli attributi di formattazione oltre al set di attributi di convalida predefinito. È già stato applicato un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) il valore di enumerazione per la data di rilascio e per i campi dei prezzi. Il codice seguente illustra il `ReleaseDate` e `Price` proprietà con l'appropriato [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) gli attributi forniscono solo gli hint per il motore di visualizzazione formattare i dati (e specificare gli attributi, ad esempio `<a>` per gli URL e `<a href="mailto:EmailAddress.com">` per la posta elettronica. È possibile utilizzare il [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attributo da convalidare il formato dei dati. Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo viene utilizzato per specificare un tipo di dati che è più specifico di tipo intrinseco del database, sono ***non*** gli attributi di convalida. In questo caso si vuole inserire solo tenere traccia delle date, non la data e ora. Il [enumerazione DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornisce per molti tipi di dati, ad esempio *data, ora, numero di telefono, valuta, EmailAddress* e altro ancora. L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo. Ad esempio, un `mailto:` possibile creare un collegamento per [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), e un selettore data può essere fornito per [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) nei browser che supportano [HTML5](http://html5.org/). Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributi genera HTML 5 [dati -](http://ejohn.org/blog/html-5-data-attributes/) (si pronuncia *dash dati*) gli attributi che è in grado di riconoscere i browser HTML 5. Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributi non forniscono alcuna convalida.

`DataType.Date` non specifica il formato della data visualizzata. Per impostazione predefinita, viene visualizzato il campo dei dati in base ai formati predefiniti in base al server[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


Il `ApplyFormatInEditMode` impostazione specifica che la formattazione specificata deve essere applicata anche quando il valore viene visualizzato in una casella di testo per la modifica. (Non è consigliabile che per alcuni campi, ad esempio, per i valori di valuta, non è il simbolo di valuta nella casella di testo per la modifica.)

È possibile utilizzare il [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo da se stesso, ma in genere è consigliabile utilizzare il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) anche l'attributo. Il `DataType` attributo fornisce il *semantica* dei dati come anziché come eseguirne il rendering in una schermata e fornisce i seguenti vantaggi che non si ottengono con `DisplayFormat`:

- Il browser è possibile abilitare le funzionalità di HTML5 (ad esempio per visualizzare un controllo di calendario il simbolo di valuta delle impostazioni locali appropriata, i collegamenti di posta elettronica, e così via.).
- Per impostazione predefinita, il browser verrà eseguito il rendering di dati utilizzando il formato corretto in base il[internazionali](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- Il[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo è possibile abilitare MVC scegliere il modello di campo a destra per il rendering dei dati (il [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) se utilizzato da se stessa viene utilizzato il modello di stringa). Per ulteriori informazioni, vedere Brad Wilson[ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Anche se scritte per MVC 2, in questo articolo ancora si applica alla versione corrente di ASP.NET MVC).

Se si utilizza il `DataType` attributo con un campo Data, è necessario specificare il `DisplayFormat` attributo anche per garantire che il campo viene visualizzato correttamente in browser Chrome. Per ulteriori informazioni, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> convalida jQuery non funziona con il[intervallo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attributo e[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Ad esempio, il codice seguente visualizzerà sempre un errore di convalida sul lato client, anche quando la data è compreso nell'intervallo specificato:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> È necessario disattivare la convalida jQuery data da utilizzare il [intervallo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attributo con[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). È in genere non consigliabile per compilare date reali nei modelli, quindi l'uso di[intervallo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attributo e[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx) è sconsigliata.


Il codice seguente illustra la combinazione di attributi in una sola riga:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

Nella parte successiva della serie verrà esaminata l'applicazione e verranno apportati alcuni miglioramenti ai metodi `Details` e `Delete` generati automaticamente.

>[!div class="step-by-step"]
[Precedente](adding-a-new-field.md)
[Successivo](examining-the-details-and-delete-methods.md)
