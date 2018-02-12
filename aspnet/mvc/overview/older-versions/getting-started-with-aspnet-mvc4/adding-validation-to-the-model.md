---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Aggiunta della convalida per il modello | Documenti Microsoft
author: Rick-Anderson
description: "Nota: Una versione aggiornata di questa esercitazione è disponibile qui che utilizza ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice seguire e demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 6de7d279677c7bbf220b956767a97aaaff8da9a1
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
<a name="adding-validation-to-the-model"></a>Aggiunta della convalida per il modello
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013. È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.


In questa sezione verrà aggiunta la logica di convalida per il `Movie` modello e si farà in modo che le regole di convalida vengono applicate ogni volta che un utente cerca di creare o modificare un filmato utilizzando l'applicazione.

## <a name="keeping-things-dry"></a>Mantenere la secca

Uno dei principi di progettazione fondamentali di ASP.NET MVC è sorgente (&quot;non ripetere manualmente&quot;). ASP.NET MVC incoraggia gli utenti a specificare funzionalità o il comportamento una sola volta e quindi fare in modo riflettersi ovunque in un'applicazione. Ciò riduce la quantità di codice, che è necessario scrivere e rende il codice che scritto meno errore soggetto e facile da gestire.

Il supporto della convalida fornito da ASP.NET MVC e Code First di Entity Framework è un ottimo esempio del principio secco nell'azione. È possibile specificare in modo dichiarativo le regole di convalida in un'unica posizione (modello) nella classe e le regole vengono applicate ovunque nell'applicazione.

Vediamo come è possibile sfruttare il supporto della convalida nell'applicazione film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Aggiunta di regole di convalida per il modello di film

Iniziare con l'aggiunta di logica di convalida per il `Movie` classe.

Aprire il file *Movie.cs*. Aggiungere un `using` istruzione all'inizio del file che fa riferimento il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Si noti lo spazio dei nomi non contiene `System.Web`. DataAnnotations fornisce un set predefinito di attributi di convalida che è possibile applicare in modo dichiarativo a qualsiasi classe o proprietà.

A questo punto aggiornare il `Movie` classe per poter sfruttare predefinito [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), e [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) gli attributi di convalida . Utilizzare il codice seguente ad esempio in cui applicare gli attributi.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Eseguire l'applicazione e verrà nuovamente visualizzato il seguente errore di runtime:

***Il modello di esecuzione del backup il contesto 'MovieDBContext' è stato modificato dopo la creazione del database. Provare a utilizzare migrazioni Code First per aggiornare il database ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Si utilizzerà le migrazioni per aggiornare lo schema. Compilare la soluzione e quindi aprire il **Package Manager Console** finestra e immettere i comandi seguenti:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` classe derivata con il nome specificato (*AddDataAnnotationsMig*) e il `Up` metodo è possibile visualizzare il codice che aggiorna i vincoli dello schema. Il `Title` e `Genre` campi non sono più nullable (ovvero, è necessario immettere un valore) e `Rating` campo ha una lunghezza massima di 5.

Gli attributi di convalida specificano il comportamento da applicare per le proprietà del modello a cui vengono applicati. Il `Required` attributo indica che una proprietà deve avere un valore; in questo esempio, un film deve disporre di valori per il `Title`, `ReleaseDate`, `Genre`, e `Price` le proprietà per essere valido. L'attributo `Range` vincola un valore all'interno di un intervallo specificato. L'attributo `StringLength` consente di impostare la lunghezza massima di una proprietà stringa e, facoltativamente, la lunghezza minima. Tipi intrinseci (ad esempio `decimal, int, float, DateTime`) sono necessari per impostazione predefinita e non è necessaria la `Required` attributo.

Codice innanzitutto assicura che le regole di convalida specificate in una classe di modello vengono applicate prima che l'applicazione salva le modifiche nel database. Ad esempio, il codice riportato di seguito verrà generata un'eccezione quando il `SaveChanges` metodo viene chiamato, perché è più necessario `Movie` mancano i valori delle proprietà e il prezzo è zero (che è compreso nell'intervallo valido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Con le regole di convalida viene applicate automaticamente da .NET Framework consente di rendere l'applicazione più affidabile. In questo modo inoltre non è possibile omettere la convalida di un elemento e quindi inserire involontariamente dati errati nel database.

Ecco un elenco di codice per l'aggiornamento *Movie.cs* file:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Errore di convalida dell'interfaccia utente in ASP.NET MVC

Eseguire nuovamente l'applicazione e passare il */Movies* URL.

Fare clic su di **Crea nuovo** collegamento per aggiungere un nuovo film. Compilare il modulo con alcuni valori non validi e quindi fare clic su di **crea** pulsante.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> per supportare la convalida jQuery per inglesi che utilizzano una virgola (&quot;,&quot;) per un punto decimale, è necessario includere *globalize.js* specifici e *cultures/globalize.cultures.js* file (da [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript per utilizzare `Globalize.parseFloat`. Il codice seguente illustra le modifiche al file Views\Movies\Edit.cshtml per lavorare con i &quot;fr-FR&quot; delle impostazioni cultura:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Si noti come il modulo è automaticamente utilizzato il colore del bordo rosso per evidenziare le caselle di testo che contengono dati non validi e ha generato un messaggio di errore di convalida appropriato accanto a ciascuna di esse. Gli errori vengono applicati sia sul lato client (utilizzo di JavaScript e jQuery) sia sul lato server (nel caso di un utente con JavaScript disabilitato).

Un vantaggio è che non è necessario modificare una singola riga di codice nel `MoviesController` classe oppure il *Create.cshtml* vista per permettere la convalida dell'interfaccia utente. Il controller e le viste creati in una fase precedente di questa esercitazione hanno selezionato automaticamente le regole di convalida specificate usando gli attributi di convalida delle proprietà della classe `Movie` del modello.

È possibile aver notato per le proprietà `Title` e `Genre`, l'attributo richiesto non viene applicata fino a quando non si invia il modulo (raggiunto il **crea** pulsante), o immettere il testo nel campo di input e rimosso. Per un campo che è inizialmente vuota (ad esempio i campi della visualizzazione di creazione) e che contiene solo l'attributo obbligatorio e non altri attributi di convalida, è possibile eseguire il comando seguente per attivare la convalida:

1. Scheda nel campo.
2. Immettere il testo.
3. Scheda.
4. Scheda nuovamente nel campo.
5. Rimuovere il testo.
6. Scheda.

La sequenza precedente verrà attivata la convalida richiesta senza dover fare clic sul pulsante Invia. Semplicemente premendo il pulsante di invio senza immettere uno dei campi verrà attivata la convalida lato client. I dati del modulo non vengono inviati al server fino a quando non sono più presenti errori di convalida sul lato client. È possibile testare questo posizionando un punto di interruzione nel metodo HTTP Post o utilizzando il [strumento fiddler](http://fiddler2.com/fiddler2/) o Internet Explorer 9 [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Come la convalida viene eseguita Create a visualizzare e creare il metodo di azione

Ci si potrebbe chiedere come la convalida dell'interfaccia utente sia stata generata senza aggiornamenti al codice nel controller o nelle viste. La voce successiva vengono mostrate le operazioni di `Create` metodi nel `MovieController` classe è simile a. Ma sono cambiati rispetto a come si creati precedentemente in questa esercitazione.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

Il primo metodo di azione (HTTP GET) `Create` visualizza il modulo di creazione iniziale. La seconda versione (`[HttpPost]`) gestisce l'invio del modulo. Il secondo metodo `Create` (la versione `HttpPost`) chiama `ModelState.IsValid` per verificare se esistono errori di convalida per il film. La chiamata a questo metodo valuta tutti gli attributi di convalida applicati all'oggetto. Se l'oggetto presenta errori di convalida, il metodo `Create` visualizza di nuovo il modulo. Se non sono presenti errori, il metodo salva il nuovo film nel database. Nell'esempio si sta usando, film **il modulo non viene registrato nel server quando sono presenti errori di convalida rilevati sul lato client; il secondo** `Create` **non viene mai chiamato**. Se si disabilita JavaScript nel browser, la convalida del client è disabilitato e HTTP POST `Create` chiamate al metodo `ModelState.IsValid` per verificare se il film è gli errori di convalida.

È possibile impostare un punto di interruzione nel metodo `HttpPost Create` e verificare che il metodo non venga mai chiamato, la convalida sul lato client non invierà i dati del modulo in caso di rilevamento di errori di convalida. Se si disabilita JavaScript nel browser, quindi si invia il modulo con errori, verrà raggiunto il punto di interruzione. Si ottiene comunque la convalida completa senza JavaScript. Nella figura seguente viene illustrato come disabilitare JavaScript in Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

La figura seguente illustra come disabilitare JavaScript nel browser FireFox.

![](adding-validation-to-the-model/_static/image5.png)

Nella figura seguente viene illustrato come disabilitare JavaScript con il browser Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Di seguito è riportato il *Create.cshtml* modello di visualizzazione che scaffolding precedenza nell'esercitazione. Viene usata dai metodi di azione illustrati in precedenza per visualizzare il modulo iniziale e per visualizzarlo nuovamente in caso di errore.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Si noti che il codice utilizza un `Html.EditorFor` helper per l'output di `<input>` per ogni elemento `Movie` proprietà. Accanto a questo supporto è presente una chiamata al `Html.ValidationMessageFor` metodo di supporto. Questi due metodi di supporto di lavoro con l'oggetto modello che viene passato dal controller per la vista (in questo caso, un `Movie` oggetto). Vengano visualizzate automaticamente per gli attributi di convalida specificati nei messaggi di errore di modello e la visualizzazione come appropriato.

Che cos'è davvero utili su questo approccio è che il controller né il modello di visualizzazione crea sappia alcuna operazione sulle regole di convalida effettiva imposizione o sui messaggi di errore specifico visualizzati. Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`. Le stesse regole di convalida vengono applicate automaticamente per la visualizzazione di modifica ed eventuali altri visualizzazioni modelli è possibile creare che modifica il modello.

Se si desidera modificare la logica di convalida in un secondo momento, è possibile farlo in esattamente un'unica posizione tramite l'aggiunta di attributi di convalida per il modello (in questo esempio, la `movie` classe). Non è necessario preoccuparsi dell'incoerenza delle diverse parti dell'applicazione con la modalità di applicazione delle regole perché tutta la logica di convalida verrà definita in un'unica posizione e usata ovunque. In questo modo il codice rimane molto pulito e facile da gestire e sviluppare. Il principio DRY sarà ampiamente rispettato.

## <a name="adding-formatting-to-the-movie-model"></a>Aggiunta di formattazione per il modello di film

Aprire il file *Movie.cs* ed esaminare la classe `Movie`. Il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) spazio dei nomi fornisce gli attributi di formattazione oltre al set di attributi di convalida predefinito. È già stato applicato un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) il valore di enumerazione per la data di rilascio e per i campi dei prezzi. Il codice seguente illustra il `ReleaseDate` e `Price` proprietà con l'appropriato [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Il [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributi non sono gli attributi di convalida, vengono usati per indicare il motore di visualizzazione come eseguire il rendering HTML. Nell'esempio precedente, il `DataType.Date` attributo consente di visualizzare le date di film come date solo, senza tempo. Ad esempio, [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributi non convalidare il formato dei dati:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Gli attributi elencati in precedenza solo di fornire suggerimenti per il motore di visualizzazione formattare i dati (e specificare gli attributi, ad esempio &lt;un&gt; per gli URL e &lt;href =&quot;mailto:EmailAddress.com&quot; &gt; per la posta elettronica. È possibile utilizzare il [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attributo da convalidare il formato dei dati.

Un approccio alternativo all'utilizzo di `DataType` gli attributi, è possibile impostare in modo esplicito un [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valore. Il codice seguente viene illustrata la proprietà di data di rilascio con una stringa di formato data (vale a dire, &quot;d&quot;). Si utilizzerà per specificare che non si desidera ora come parte della data di rilascio.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

L'intero `Movie` classe è illustrata di seguito.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Eseguire l'applicazione e individuare il `Movies` controller. La data di rilascio e il prezzo correttamente formattati. L'immagine seguente mostra la data di rilascio e prezzi utilizzando &quot;fr-FR&quot; come impostazioni cultura.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

L'immagine seguente mostra gli stessi dati visualizzati con le impostazioni cultura predefinite (inglese Stati Uniti).

![](adding-validation-to-the-model/_static/image8.png)

Nella parte successiva della serie verrà esaminata l'applicazione e verranno apportati alcuni miglioramenti ai metodi `Details` e `Delete` generati automaticamente.

>[!div class="step-by-step"]
[Precedente](adding-a-new-field-to-the-movie-model-and-table.md)
[Successivo](examining-the-details-and-delete-methods.md)
