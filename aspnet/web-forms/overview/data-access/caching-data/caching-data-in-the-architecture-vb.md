---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
title: La memorizzazione nella cache i dati nell'architettura (VB) | Microsoft Docs
author: rick-anderson
description: Nell'esercitazione precedente abbiamo appreso come applicare la memorizzazione nella cache a livello di presentazione. In questa esercitazione viene illustrato come sfruttare i vantaggi del nostro architectu a più livelli...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 5e189dd7-f4f9-4f28-9b3a-6cb7d392e9c7
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
msc.type: authoredcontent
ms.openlocfilehash: 45a717f9b68a1465d3446b06358a062f6b640c9e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836518"
---
<a name="caching-data-in-the-architecture-vb"></a>La memorizzazione nella cache i dati nell'architettura (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_VB.exe) o [Scarica il PDF](caching-data-in-the-architecture-vb/_static/datatutorial59vb1.pdf)

> Nell'esercitazione precedente abbiamo appreso come applicare la memorizzazione nella cache a livello di presentazione. In questa esercitazione è informazioni su come sfruttare i vantaggi della nostra architettura a più livelli per memorizzare i dati a livello di logica di Business. Facciamo estendendo l'architettura per includere un livello di memorizzazione nella cache.


## <a name="introduction"></a>Introduzione

Come illustrato nell'esercitazione precedente, la memorizzazione nella cache i dati di s ObjectDataSource è semplice come l'impostazione di un paio di proprietà. Sfortunatamente, ObjectDataSource si applica la memorizzazione nella cache a livello di presentazione, che i criteri di memorizzazione nella cache è strettamente collegato con la pagina ASP.NET. Uno dei motivi per la creazione di un'architettura a più livelli consiste nel consentire tali accorto venga interrotto. Il livello di logica di Business, ad esempio, consente di disaccoppiare la logica di business dalle pagine di ASP.NET, mentre il livello di accesso ai dati consente di disaccoppiare i dettagli di accesso ai dati. Questa separazione dei dettagli di accesso per la logica e i dati di business è preferibile, in parte, perché il sistema diventa più leggibile, più facili da gestire e più flessibile per modificare. Consente inoltre di conoscenza del settore e divisione del lavoro, gli sviluppatori che lavorano sul livello di presentazione t devono conoscere i dettagli del database s per svolgere il proprio lavoro. I criteri di memorizzazione nella cache dal livello di presentazione di separazione offre vantaggi simili.

In questa esercitazione forniamo nostra architettura per includere un *memorizzazione nella cache di livello* (o CL breve) che usa i criteri di memorizzazione nella cache. Il livello di memorizzazione nella cache includerà un `ProductsCL` classe che fornisce l'accesso alle informazioni sul prodotto con metodi come `GetProducts()`, `GetProductsByCategoryID(categoryID)`e così via, che, quando richiamata, verranno primo tentativo di recuperare i dati dalla cache. Se la cache è vuota, questi metodi richiamerà appropriato `ProductsBLL` nel livello BLL, che a sua volta si ottengono i dati DAL metodo. Il `ProductsCL` metodi di memorizzare nella cache i dati recuperati da BLL prima di restituirlo.

Come illustrato nella figura 1, il CL si trova tra la presentazione e livelli di logica di Business.


![La memorizzazione nella cache livello (CL) è un altro livello nell'architettura la nostra](caching-data-in-the-architecture-vb/_static/image1.png)

**Figura 1**: la memorizzazione nella cache livello (CL) è un altro livello nell'architettura la nostra


## <a name="step-1-creating-the-caching-layer-classes"></a>Passaggio 1: Creazione di classi del livello di memorizzazione nella cache

In questa esercitazione si creerà un CL molto semplice con una singola classe `ProductsCL` che ha solo un numero limitato di metodi. Creazione di un livello di memorizzazione nella cache completa per l'intera applicazione sarebbe necessario creando `CategoriesCL`, `EmployeesCL`, e `SuppliersCL` classi e fornendo un metodo in queste classi di livello la memorizzazione nella cache per ogni metodo di accesso o la modifica dei dati nel livello BLL. Come con il livello BLL e DAL, idealmente il livello di memorizzazione nella cache deve essere implementato come un progetto libreria di classi separato. Tuttavia, è lo implementeranno come classe nel `App_Code` cartella.

Per altre classi separate correttamente il CL dalle classi di DAL e BLL, ti permettono di s, creare una nuova sottocartella di `App_Code` cartella. Fare clic sui `App_Code` cartella in Esplora soluzioni, scegliere Nuova cartella e denominare la nuova cartella `CL`. Dopo aver creato questa cartella, aggiungere una nuova classe denominata `ProductsCL.vb`.


![Aggiungere una nuova cartella denominata CL e una classe denominata ProductsCL.vb](caching-data-in-the-architecture-vb/_static/image2.png)

**Figura 2**: aggiungere una nuova cartella denominata `CL` e una classe denominata `ProductsCL.vb`


Il `ProductsCL` classe deve includere lo stesso set di metodi di accesso e modifica dati, disponibile nella relativa classe di livello per la logica di Business corrispondente (`ProductsBLL`). Anziché creare tutti questi metodi, s "Let" semplicemente compilazione qui un paio per acquisire familiarità con i modelli usati da CL. In particolare, si aggiungerà il `GetProducts()` e `GetProductsByCategoryID(categoryID)` metodi nel passaggio 3 e un `UpdateProduct` overload nel passaggio 4. È possibile aggiungere il rimanente `ProductsCL` metodi e `CategoriesCL`, `EmployeesCL`, e `SuppliersCL` classi in base alle esigenze specifiche.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Passaggio 2: Lettura e scrittura alla Cache dei dati

ObjectDataSource la memorizzazione nella cache funzionalità esplorati internamente nell'esercitazione precedente Usa la cache dei dati ASP.NET per archiviare i dati recuperati da BLL. La cache dei dati è possibile accedere anche a livello di codice da classi di code-behind di pagine ASP.NET o dalle classi nell'architettura s delle applicazioni web. Per leggere e scrivere la cache dei dati da una classe code-behind s della pagina ASP.NET, usare il modello seguente:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample1.vb)]

Il [ `Cache` classe](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` metodo](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) dispone di un numero di overload. `Cache("key") = value` e `Cache.Insert(key, value)` sono sinonimi e sia aggiungere un elemento alla cache usando la chiave specificata senza una scadenza definita. In genere, si vuole specificare una scadenza quando si aggiunge un elemento alla cache, come una dipendenza, una scadenza basati sul tempo o entrambi. Usare uno degli altri `Insert` overload del metodo s per fornire informazioni di scadenza delle dipendenze - o basati sul tempo.

Il livello di memorizzazione nella cache s metodi devono prima di tutto verificare se sono presente nella cache i dati richiesti e, in caso affermativo, restituirlo da tale posizione. Se i dati richiesti non sono presente nella cache, il metodo BLL appropriato deve essere richiamato. Il valore restituito deve essere memorizzato nella cache e sono tornato, come illustrato nel seguente diagramma di sequenza.


![I metodi di s la memorizzazione nella cache di livello restituiscono i dati dalla Cache Se si s disponibile](caching-data-in-the-architecture-vb/_static/image3.png)

**Figura 3**: il livello di memorizzazione nella cache s metodi restituisca i dati dalla Cache Se si s disponibile


La sequenza illustrata nella figura 3 viene eseguita nelle classi CL adottando il modello seguente:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample2.vb)]

In questo caso, *tipo* è il tipo di dati da archiviare nella cache `Northwind.ProductsDataTable`, ad esempio mentre *chiave* è la chiave che identifica in modo univoco l'elemento della cache. Se l'elemento con l'oggetto specificato *key* non è nella cache, quindi *istanza* saranno `Nothing` e i dati verranno recuperati dal metodo BLL appropriato e aggiunto alla cache. Nel momento `Return instance` raggiungimento *istanza* contiene un riferimento ai dati, dalla cache o il pull da BLL.

Assicurarsi di usare il modello precedente, l'accesso ai dati dalla cache. Il modello seguente, che, a prima vista, è equivalente, contiene una sottile differenza che introduce una race condition. Le race condition sono difficili da eseguire il debug perché rivelano la propria presenza sporadicamente e difficili da riprodurre.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample3.vb)]

La differenza in questo secondo frammento di codice non corretto è che invece di memorizzare un riferimento all'elemento memorizzato nella cache in una variabile locale, la cache dei dati avviene direttamente nell'istruzione condizionale *e* nel `Return`. Si supponga che, quando viene raggiunto questo codice, `Cache("key")` non è `Nothing`, ma prima la `Return` istruzione viene raggiunto, il sistema rimuove *chiave* dalla cache. In questi rari casi, verrà restituito il codice `Nothing` anziché un oggetto del tipo previsto.

> [!NOTE]
> La cache dei dati è thread-safe, quindi non è necessario sincronizzare l'accesso thread per semplici operazioni di lettura o scrittura. Tuttavia, se è necessario eseguire più operazioni sui dati nella cache che devono essere atomici, si è responsabili dell'implementazione di un blocco o un altro meccanismo per garantire la thread safety. Visualizzare [la sincronizzazione di accesso alla Cache ASP.NET](http://www.ddj.com/184406369) per altre informazioni.


Un elemento è possibile dedurlo a livello di codice nella cache i dati usando il [ `Remove` metodo](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) come segue:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample4.vb)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Passaggio 3: Restituzione di informazioni sui prodotti dal`ProductsCL`classe

Per questa esercitazione lasciare s implementare due metodi per la restituzione di informazioni sui prodotti dal `ProductsCL` classe: `GetProducts()` e `GetProductsByCategoryID(categoryID)`. Come con le `ProductsBL` classe nel livello di logica di Business, il `GetProducts()` metodo CL restituisce informazioni su tutti i prodotti come una `Northwind.ProductsDataTable` oggetto, mentre `GetProductsByCategoryID(categoryID)` restituisce tutti i prodotti da una categoria specificata.

Il codice seguente viene illustrata una parte dei metodi di `ProductsCL` classe:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample5.vb)]

In primo luogo, si noti il `DataObject` e `DataObjectMethodAttribute` attributi applicati ai metodi e classi. Questi attributi forniscono informazioni per la procedura guidata s ObjectDataSource, che indica che le classi e metodi devono apparire nella procedura guidata s. Poiché i metodi e classi CL saranno accessibili da ObjectDataSource nel livello di presentazione, ho aggiunto questi attributi per migliorare l'esperienza in fase di progettazione. Fare riferimento al [creazione di un livello di logica di Business](../introduction/creating-a-business-logic-layer-vb.md) esercitazione per una descrizione più completa su questi attributi e i relativi effetti.

Nel `GetProducts()` e `GetProductsByCategoryID(categoryID)` metodi, i dati restituiti dai `GetCacheItem(key)` metodo viene assegnato a una variabile locale. Il `GetCacheItem(key)` metodo, che esamineremo a breve, restituisce un elemento specifico dalla cache basata sull'oggetto specificato *chiave*. Se tali dati non viene trovati nella cache, viene recuperato dal corrispondente `ProductsBLL` metodo della classe e quindi aggiunto alla cache usando il `AddCacheItem(key, value)` (metodo).

Il `GetCacheItem(key)` e `AddCacheItem(key, value)` metodi di interfaccia con la cache dei dati, la lettura e scrittura dei valori, rispettivamente. Il `GetCacheItem(key)` metodo è più semplice delle due. Restituisce semplicemente il valore dalla classe Cache usando passato *chiave*:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample6.vb)]

`GetCacheItem(key)` non usa *chiave* valore specificato, ma le chiamate di `GetCacheKey(key)` metodo, che restituisce il *chiave* preceduto ProductsCache-. Il `MasterCacheKeyArray`, che contiene la stringa ProductsCache, viene usato anche per il `AddCacheItem(key, value)` metodo, come vedremo momentaneamente.

Da una classe code-behind s pagina ASP.NET, la cache dei dati sono accessibili tramite il `Page` classe s [ `Cache` proprietà](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)e consente una sintassi simile a quella `Cache("key") = value`, come illustrato nel passaggio 2. Da una classe all'interno dell'architettura, la cache dei dati sono accessibili tramite uno `HttpRuntime.Cache` o `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)del post di blog [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) il vantaggio di lieve miglioramento delle prestazioni dell'utilizzo di notes `HttpRuntime` invece di `HttpContext.Current`; di conseguenza, `ProductsCL` Usa `HttpRuntime`.

> [!NOTE]
> Se l'architettura è implementata utilizzando i progetti libreria di classi, è necessario aggiungere un riferimento per la `System.Web` assembly per poter usare il [ `HttpRuntime` ](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) e [ `HttpContext` ](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) classi.


Se l'elemento non viene trovato nella cache, il `ProductsCL` metodi di classe s ottenere i dati da BLL e aggiungerlo alla cache usando il `AddCacheItem(key, value)` (metodo). Per aggiungere *valore* alla cache potremmo utilizziamo il codice seguente, che viene utilizzata una scadenza di 60 secondi ora:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample7.vb)]

`DateTime.Now.AddSeconds(CacheDuration)` Specifica la durata basati sul tempo di 60 secondi in while futuri [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) non indica ciò che s alcuna scadenza variabile. Mentre questo `Insert` overload del metodo con parametri di input per entrambi un assoluto e variabile scadenza, è possibile fornire solo una delle due. Se si tenta di specificare un tempo assoluto e un intervallo di tempo, il `Insert` metodo genererà un' `ArgumentException` eccezione.

> [!NOTE]
> Questa implementazione del `AddCacheItem(key, value)` metodo attualmente presenta alcuni difetti. Illustreremo affrontare e risolvere questi problemi nel passaggio 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Passaggio 4: Invalidare i dati di Cache quando viene modificato tramite l'architettura

Insieme ai metodi di recupero dei dati, il livello di memorizzazione nella cache deve fornire gli stessi metodi come il livello BLL per inserimento, aggiornamento ed eliminazione dei dati. I metodi di modifica dei dati di s CL non modificano i dati memorizzati nella cache, ma piuttosto chiamare il metodo modifica di dati della versione corrispondente di BLL s e poi invalidare la cache. Come illustrato nell'esercitazione precedente, questo è lo stesso comportamento che ObjectDataSource si applica quando sono abilitate le funzionalità di memorizzazione nella cache e il relativo `Insert`, `Update`, o `Delete` vengono richiamati i metodi.

Nell'esempio `UpdateProduct` overload viene illustrato come implementare i metodi di modifica dei dati in CL:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample8.vb)]

La modifica dei dati appropriati metodo Business Logic Layer viene richiamata, ma prima che venga restituita la risposta è necessario invalidare la cache. Sfortunatamente, invalidare la cache non è estremamente semplice perché il `ProductsCL` classe s `GetProducts()` e `GetProductsByCategoryID(categoryID)` metodi ognuno consentono di aggiungere elementi alla cache con chiavi diverse e il `GetProductsByCategoryID(categoryID)` metodo aggiunge un elemento della cache diverso per ogni univoco *categoryID*.

Quando l'invalidazione della cache, è necessario rimuovere *tutte* degli elementi che potrebbero essere stati aggiunti dal `ProductsCL` classe. Questa operazione può essere eseguita associando un *memorizza nella cache delle dipendenze* con ogni elemento aggiunto alla cache nel server di `AddCacheItem(key, value)` (metodo). In generale, una dipendenza della cache può essere un altro elemento nella cache, un file nel file system, o dati da un database Microsoft SQL Server. Quando la dipendenza viene modificato o viene rimosso dalla cache, gli elementi della cache è associato vengono automaticamente rimossi dalla cache. Per questa esercitazione, è necessario creare un elemento aggiuntivo nella cache che funge da una dipendenza della cache per tutti gli elementi aggiunti tramite il `ProductsCL` classe. In questo modo, tutti questi elementi possono essere rimossi dalla cache rimuovendo semplicemente la dipendenza della cache.

Aggiornamento di "Let" s il `AddCacheItem(key, value)` metodo in modo che ogni elemento aggiunto alla cache tramite questo metodo è associato a una dipendenza della singola cache:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample9.vb)]

`MasterCacheKeyArray` è una matrice di stringhe che contiene un singolo valore, ProductsCache. In primo luogo, un elemento della cache viene aggiunto alla cache e assegnato la data e ora correnti. Se l'elemento della cache esiste già, viene aggiornata. Successivamente, viene creata una dipendenza della cache. Il [ `CacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) s costruttore ha un numero di overload, ma quello usata qui prevede due `String` gli input di matrice. La prima specifica il set di file da utilizzare come dipendenze. Poiché non abbiamo da usare eventuali dipendenze basate su file, un valore di t `Nothing` viene usato per il primo parametro di input. Il secondo parametro di input specifica il set di chiavi della cache da utilizzare come dipendenze. In questo caso specifichiamo la dipendenza singola, `MasterCacheKeyArray`. Il `CacheDependency` viene quindi passato il `Insert` (metodo).

Grazie a questa modifica a `AddCacheItem(key, value)`, invaliding è semplice quanto rimuovendo la dipendenza della cache.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample10.vb)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Passaggio 5: Chiamare il livello di memorizzazione nella cache dal livello di presentazione

Metodi e le classi di memorizzazione nella cache di livello s utilizzabile per lavorare con i dati usando le tecniche si va esaminato in queste esercitazioni. Per illustrare l'utilizzo dei dati memorizzati nella cache, salvare le modifiche per il `ProductsCL` classe e quindi aprire la `FromTheArchitecture.aspx` nella pagina il `Caching` cartella e aggiungere un controllo GridView. GridView s nello smart tag, creare un nuovo oggetto ObjectDataSource. Nel primo passaggio s guidata dovrebbe essere il `ProductsCL` classe come una delle opzioni nell'elenco a discesa.


[![La classe ProductsCL è incluso nell'elenco a discesa di oggetti Business](caching-data-in-the-architecture-vb/_static/image5.png)](caching-data-in-the-architecture-vb/_static/image4.png)

**Figura 4**: il `ProductsCL` classe è inclusa nell'elenco a discesa di oggetti Business ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-in-the-architecture-vb/_static/image6.png))


Dopo aver selezionato `ProductsCL`, fare clic su Avanti. L'elenco di riepilogo a discesa della scheda selezionare dispone di due elementi - `GetProducts()` e `GetProductsByCategoryID(categoryID)` e la scheda aggiornamento ha unico `UpdateProduct` rapporto di overload. Scegliere il `GetProducts()` metodo nella scheda Seleziona e `UpdateProducts` metodo la scheda aggiornamento e fare clic su Fine.


[![Nell'elenco a discesa Elenca sono elencati i metodi della classe ProductsCL s](caching-data-in-the-architecture-vb/_static/image8.png)](caching-data-in-the-architecture-vb/_static/image7.png)

**Figura 5**: il `ProductsCL` sono racchiusi l'elenco a discesa sono elencati i metodi della classe s ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-in-the-architecture-vb/_static/image9.png))


Dopo aver completato la procedura guidata, Visual Studio imposterà la s ObjectDataSource `OldValuesParameterFormatString` proprietà `original_{0}` e aggiungere i campi appropriati a GridView. Modifica il `OldValuesParameterFormatString` il valore predefinito, proprietà `{0}`e configurare il controllo GridView per supportare il paging, ordinamento e la modifica. Poiché il `UploadProducts` overload utilizzato da CL accetta solo il nome di prodotto modificato s e prezzo, limitare il controllo GridView in modo che solo questi campi sono modificabili.

Nell'esercitazione precedente è stato definito un controllo GridView per includere i campi per il `ProductName`, `CategoryName`, e `UnitPrice` campi. È possibile replicare questa formattazione e la struttura, nel qual caso la s GridView e ObjectDataSource dichiarativo markup dovrebbe essere simile al seguente:


[!code-aspx[Main](caching-data-in-the-architecture-vb/samples/sample11.aspx)]

A questo punto si dispone di una pagina che usa il livello di memorizzazione nella cache. Per visualizzare la cache in azione, impostare i punti di interruzione `ProductsCL` classe s `GetProducts()` e `UpdateProduct` metodi. Visita la pagina in un browser e di eseguire il codice durante l'ordinamento e paging per visualizzare i dati estratti dalla cache. Quindi aggiornare un record e si noti che la cache viene invalidata e, di conseguenza, vengono recuperato da BLL quando i dati sono riassociati per il controllo GridView.

> [!NOTE]
> Il livello di memorizzazione nella cache fornito nel download che accompagna questo articolo non è stato completato. Contiene una sola classe, `ProductsCL`, che propone solo un numero limitato di metodi. Inoltre, solo una singola pagina ASP.NET viene utilizzato il CL (`~/Caching/FromTheArchitecture.aspx`) tutti gli altri ancora fare riferimento il livello BLL direttamente. Se si intende usare un CL nell'applicazione, tutte le chiamate dal livello di presentazione devono accedere al CL, condizione che richiede che le classi di s CL e i metodi illustrati tali classi e metodi nel livello BLL attualmente usato dal livello di presentazione.


## <a name="summary"></a>Riepilogo

Anche se la memorizzazione nella cache può essere applicato nel server di livello di presentazione con ASP.NET 2.0 s SqlDataSource e ObjectDataSource controlli, in una situazione ideale la memorizzazione nella cache le responsabilità potrebbe essere delegato a un livello separato nell'architettura. In questa esercitazione sono stati creati un livello di memorizzazione nella cache che si trova tra il livello di presentazione e il livello di logica di Business. Il livello di memorizzazione nella cache deve fornire lo stesso set di classi e metodi che esistono nel livello BLL e vengono chiamati dal livello di presentazione.

Gli esempi di livello la memorizzazione nella cache sono stati presentati in questa e le esercitazioni precedenti esibiti *caricamento reattivo*. Con il caricamento reattivo, i dati vengono caricati nella cache solo quando viene effettuata una richiesta per i dati e che i dati sono mancanti dalla cache. I dati possono anche essere *caricato in modo proattivo* nella cache, una tecnica che carica i dati nella cache prima che sia effettivamente necessario. Nella prossima esercitazione vedremo un esempio di caricamento attiva quando si esamina come archiviare valori statici nella cache all'avvio dell'applicazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](caching-data-with-the-objectdatasource-vb.md)
> [Successivo](caching-data-at-application-startup-vb.md)
