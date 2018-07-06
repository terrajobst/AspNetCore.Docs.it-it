---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Aggiunta della convalida al modello | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro e molto più semplice da seguire e demo...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 50911920ed80f09033cfc53a137a6bbba3fc62f3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834491"
---
<a name="adding-validation-to-the-model"></a>Aggiunta della convalida al modello
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.


In questa sezione si aggiungerà la logica di convalida per il `Movie` modello e si farà in modo che le regole di convalida vengono applicate ogni volta che un utente prova a creare o modificare un film tramite l'applicazione.

## <a name="keeping-things-dry"></a>Mantenere la trattazione DRY

Uno dei principi di progettazione fondamentali di ASP.NET MVC è DRY (&quot;t Repeat Yourself&quot;). ASP.NET MVC consiglia di specificare la funzionalità o il comportamento una sola volta e quindi viene applicata ovunque in un'applicazione. Ciò riduce la quantità di codice che è necessario scrivere e rende il codice che scrive meno soggetto a errori e più facili da gestire.

Il supporto della convalida fornito da ASP.NET MVC ed Entity Framework Code First è un ottimo esempio del principio DRY in azione. È possibile specificare in modo dichiarativo le regole di convalida in un'unica posizione (nella classe del modello) e le regole vengono applicate ovunque nell'applicazione.

Si esaminerà come è possibile sfruttare il supporto per la convalida nell'applicazione di film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Aggiunta di regole di convalida al modello Movie

Iniziare aggiungendo una logica di convalida per il `Movie` classe.

Aprire il file *Movie.cs*. Aggiungere un `using` all'inizio del file che fa riferimento l'istruzione il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Si noti lo spazio dei nomi non contengono `System.Web`. DataAnnotations fornisce un set predefinito di attributi di convalida che è possibile applicare in modo dichiarativo a qualsiasi classe o proprietà.

A questo punto aggiornare i `Movie` classe possa sfruttare i vantaggi di integrato [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), e [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) gli attributi di convalida . Usare il codice seguente come esempio in cui applicare gli attributi.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Eseguire l'applicazione e si otterrà anche in questo caso l'errore in fase di esecuzione seguenti:

***Il modello supporta il contesto 'MovieDBContext' è stato modificato dal momento della creazione del database. È consigliabile usare migrazioni Code First per aggiornare il database ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Si userà migrations per aggiornare lo schema. Compilare la soluzione e quindi aprire il **Console di gestione pacchetti** finestra e immettere i comandi seguenti:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` classe derivata con il nome specificato (*AddDataAnnotationsMig*) e nel `Up` metodo è possibile visualizzare il codice che aggiorna i vincoli dello schema. Il `Title` e `Genre` campi non sono più che ammette valori null (vale a dire, è necessario immettere un valore) e il `Rating` campo ha una lunghezza massima pari a 5.

Gli attributi di convalida specificano il comportamento da applicare per le proprietà del modello a cui vengono applicati. Il `Required` attributo indica che una proprietà deve avere un valore; in questo esempio, un filmato deve avere i valori per il `Title`, `ReleaseDate`, `Genre`, e `Price` proprietà affinché sia valida. L'attributo `Range` vincola un valore all'interno di un intervallo specificato. L'attributo `StringLength` consente di impostare la lunghezza massima di una proprietà stringa e, facoltativamente, la lunghezza minima. Tipi intrinseci (ad esempio `decimal, int, float, DateTime`) sono obbligatorie per impostazione predefinita e non è necessario il `Required` attributo.

Codice, in primo luogo, assicura che le regole di convalida che si specifica in una classe di modello vengono applicate prima che l'applicazione deve salvare le modifiche nel database. Ad esempio, il codice seguente verrà generata un'eccezione quando la `SaveChanges` viene chiamato il metodo in quanto alcuni richiesta `Movie` mancano i valori delle proprietà e il prezzo è zero (che è compreso nell'intervallo valido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Con le regole di convalida applicate automaticamente da .NET Framework consente di rendere l'applicazione più affidabile. In questo modo inoltre non è possibile omettere la convalida di un elemento e quindi inserire involontariamente dati errati nel database.

Ecco un elenco di codice per l'aggiornamento *Movie.cs* file:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Errore di convalida dell'interfaccia utente in ASP.NET MVC

Eseguire nuovamente l'applicazione e passare al */Movies* URL.

Scegliere il **Crea nuovo** collegamento per aggiungere un nuovo film. Compilare il modulo con alcuni valori non validi e quindi scegliere il **Create** pulsante.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> per supportare la convalida di jQuery per impostazioni locali di lingua diversa dall'inglese che usano la virgola (&quot;,&quot;) per un separatore decimale, è necessario includere *globalize.js* specifiche e *cultures/globalize.cultures.js* file (da [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) e JavaScript usare `Globalize.parseFloat`. Il codice seguente illustra le modifiche al file Views\Movies\Edit.cshtml per lavorare con i &quot;fr-FR&quot; delle impostazioni cultura:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Si noti come il form è automaticamente usavano un colore bordo rosso per evidenziare le caselle di testo che contengono dati non validi e ha generato un messaggio di errore di convalida appropriato accanto a ciascuna di esse. Gli errori vengono applicati sia sul lato client (utilizzo di JavaScript e jQuery) sia sul lato server (nel caso di un utente con JavaScript disabilitato).

Dei vantaggi reale è che non è necessario modificare una singola riga di codice nel `MoviesController` classe o nel *create. cshtml* visualizzazione per consentire la convalida dell'interfaccia utente. Il controller e le viste creati in una fase precedente di questa esercitazione hanno selezionato automaticamente le regole di convalida specificate usando gli attributi di convalida delle proprietà della classe `Movie` del modello.

Si potrebbe aver notato per le proprietà `Title` e `Genre`, l'attributo obbligatorio non viene applicata fino a quando non si invia il modulo (premere il **crea** pulsante), o immettere il testo nel campo di input e rimuoverlo. Per un campo che è inizialmente vuoto (ad esempio, i campi nella visualizzazione di creazione) e che contiene solo l'attributo obbligatorio e non altri attributi di convalida, è possibile eseguire il comando seguente per attivare la convalida:

1. Scheda nel campo.
2. Immettere il testo.
3. Uscire dalla scheda tramite tabulazione.
4. Scheda nuovamente nel campo.
5. Rimuovere il testo.
6. Uscire dalla scheda tramite tabulazione.

La sequenza precedente attiverà la convalida richiesta senza dover fare clic sul pulsante Invia. Semplicemente premendo il pulsante di invio senza immettere uno dei campi verrà attivata la convalida lato client. I dati del modulo non vengono inviati al server fino a quando non sono più presenti errori di convalida sul lato client. È possibile verificarlo tramite l'inserimento di un punto di interruzione nel metodo HTTP Post o mediante il [strumento fiddler](http://fiddler2.com/fiddler2/) o di Internet Explorer 9 [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>La modalità di convalida viene eseguita nella creazione consente di visualizzare e creare il metodo di azione

Ci si potrebbe chiedere come la convalida dell'interfaccia utente sia stata generata senza aggiornamenti al codice nel controller o nelle viste. Il listato successivo illustra ciò che il `Create` metodi nel `MovieController` classi simili. Sono cambiati rispetto a come si creati in precedenza in questa esercitazione.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

Il primo metodo di azione (HTTP GET) `Create` visualizza il modulo di creazione iniziale. La seconda versione (`[HttpPost]`) gestisce l'invio del modulo. Il secondo metodo `Create` (la versione `HttpPost`) chiama `ModelState.IsValid` per verificare se esistono errori di convalida per il film. La chiamata a questo metodo valuta tutti gli attributi di convalida applicati all'oggetto. Se l'oggetto presenta errori di convalida, il metodo `Create` visualizza di nuovo il modulo. Se non sono presenti errori, il metodo salva il nuovo film nel database. Nell'esempio del film viene usato **il modulo non viene inviato al server quando sono presenti errori di convalida rilevati sul lato client; il secondo** `Create` **metodo non viene mai chiamato**. Se si disabilita JavaScript nel browser, la convalida del client è disabilitata e la richiesta HTTP POST `Create` chiamate al metodo `ModelState.IsValid` per verificare se il film è errori di convalida.

È possibile impostare un punto di interruzione nel metodo `HttpPost Create` e verificare che il metodo non venga mai chiamato, la convalida sul lato client non invierà i dati del modulo in caso di rilevamento di errori di convalida. Se si disabilita JavaScript nel browser, quindi si invia il modulo con errori, verrà raggiunto il punto di interruzione. Si ottiene comunque la convalida completa senza JavaScript. La figura seguente viene illustrato come disabilitare JavaScript in Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

La figura seguente illustra come disabilitare JavaScript nel browser FireFox.

![](adding-validation-to-the-model/_static/image5.png)

La figura seguente viene illustrato come disabilitare JavaScript con il browser Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Di seguito è riportato il *create. cshtml* modello di visualizzazione che è stato eseguito lo scaffolding in precedenza nell'esercitazione. Viene usata dai metodi di azione illustrati in precedenza per visualizzare il modulo iniziale e per visualizzarlo nuovamente in caso di errore.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Si noti come il codice usa un' `Html.EditorFor` helper per restituire il `<input>` per ogni elemento `Movie` proprietà. Accanto a questo helper è una chiamata al `Html.ValidationMessageFor` metodo helper. Questi due metodi di supporto di lavoro con l'oggetto modello che viene passato dal controller alla vista (in questo caso, un `Movie` oggetto). Risultino automaticamente per gli attributi di convalida specificati nei messaggi di errore di modello e la visualizzazione come appropriato.

Che cos'è ottimo su questo approccio è che il controller né il modello di vista crea consapevoli sulle regole di convalida effettiva viene applicate o sui messaggi di errore specifici visualizzati. Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`. Le stesse regole di convalida applicate automaticamente per la visualizzazione di modifica e di eventuali altri modelli di vista è possibile creare che modificano il modello.

Se si desidera modificare la logica di convalida in un secondo momento, è possibile farlo in una posizione tramite l'aggiunta di attributi di convalida al modello (in questo esempio, il `movie` classe). Non è necessario preoccuparsi dell'incoerenza delle diverse parti dell'applicazione con la modalità di applicazione delle regole perché tutta la logica di convalida verrà definita in un'unica posizione e usata ovunque. In questo modo il codice rimane molto pulito e facile da gestire e sviluppare. Il principio DRY sarà ampiamente rispettato.

## <a name="adding-formatting-to-the-movie-model"></a>Aggiunta di formattazione al modello Movie

Aprire il file *Movie.cs* ed esaminare la classe `Movie`. Il [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi fornisce gli attributi di formattazione oltre al set predefinito di attributi di convalida. È già stato applicato un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valore di enumerazione per la data di rilascio e per i campi relativi ai prezzi. Il codice seguente illustra il `ReleaseDate` e `Price` delle proprietà con l'appropriato [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Il [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) gli attributi non sono gli attributi di convalida, vengono usati per indicare al motore di visualizzazione come eseguire il rendering HTML. Nell'esempio precedente, il `DataType.Date` attributo vengono visualizzate le date di film come solo date e senza l'ora. Ad esempio, il seguente [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributi non convalidare il formato dei dati:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Gli attributi elencati in precedenza forniscono solo gli hint per il motore di vista formatti i dati (e fornisca gli attributi, ad esempio &lt;una&gt; per gli URL e &lt;href =&quot;mailto:EmailAddress.com&quot; &gt; per la posta elettronica. È possibile usare la [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attributo da convalidare il formato dei dati.

Un approccio alternativo all'utilizzo di `DataType` attributi, è possibile impostare esplicitamente un [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valore. Il codice seguente illustra la proprietà data di rilascio con una stringa di formato data (vale a dire &quot;1!d&quot;). Si utilizzerà per specificare che non si vuole ora come parte della data di rilascio.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

L'intero `Movie` classe è illustrata di seguito.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Eseguire l'applicazione e individuare il `Movies` controller. La data di rilascio e il prezzo viene formattati in modo corretto. L'immagine seguente mostra la data di rilascio e l'utilizzo di prezzo &quot;fr-FR&quot; come impostazioni cultura.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

L'immagine seguente mostra gli stessi dati visualizzati con le impostazioni cultura predefinite (in lingua inglese Stati Uniti).

![](adding-validation-to-the-model/_static/image8.png)

Nella parte successiva della serie verrà esaminata l'applicazione e verranno apportati alcuni miglioramenti ai metodi `Details` e `Delete` generati automaticamente.

> [!div class="step-by-step"]
> [Precedente](adding-a-new-field-to-the-movie-model-and-table.md)
> [Successivo](examining-the-details-and-delete-methods.md)
