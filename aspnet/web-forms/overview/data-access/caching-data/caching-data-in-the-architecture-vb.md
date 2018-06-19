---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
title: La memorizzazione nella cache i dati dell'architettura della finestra (VB) | Documenti Microsoft
author: rick-anderson
description: Nell'esercitazione precedente è stato descritto come applicare la memorizzazione nella cache a livello di presentazione. In questa esercitazione è imparare a sfruttare il nostro architectu a più livelli...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 5e189dd7-f4f9-4f28-9b3a-6cb7d392e9c7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
msc.type: authoredcontent
ms.openlocfilehash: 08f83c129d589859723249becb818386bfff19bf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876384"
---
<a name="caching-data-in-the-architecture-vb"></a>Memorizzazione nella cache i dati dell'architettura della finestra (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_VB.exe) o [Scarica il PDF](caching-data-in-the-architecture-vb/_static/datatutorial59vb1.pdf)

> Nell'esercitazione precedente è stato descritto come applicare la memorizzazione nella cache a livello di presentazione. In questa esercitazione è imparare a sfruttare l'architettura a più livelli per memorizzare i dati a livello di logica di Business. Facciamo estendendo l'architettura per includere un livello di memorizzazione nella cache.


## <a name="introduction"></a>Introduzione

Come illustrato nell'esercitazione precedente, la memorizzazione nella cache i dati di s ObjectDataSource è semplice come l'impostazione di un paio di proprietà. Purtroppo, ObjectDataSource si applica la memorizzazione nella cache a livello di presentazione, quali i criteri di memorizzazione nella cache è strettamente collegato con la pagina ASP.NET. Uno dei motivi per la creazione di un'architettura a più livelli consiste nel consentire tali giunti venga interrotto. Il livello di logica di Business, separa, ad esempio, la logica di business dalle pagine ASP.NET, mentre il livello di accesso ai dati separa i dettagli di accesso ai dati. Grazie a questa separazione dei dettagli di accesso dati e logica di business è preferibile, in parte, in quanto rende il sistema più leggibile, migliore e più flessibile per modificare. Consente inoltre di informazioni di dominio e divisione del lavoro, uno sviluppatore che lavora su t livello di presentazione è necessario avere familiarità con i dettagli del database s per svolgere il proprio lavoro. I criteri di memorizzazione nella cache dal livello di presentazione di separazione offre vantaggi simili.

In questa esercitazione forniamo architettura per includere un *la memorizzazione nella cache di livello* (o CL abbreviato) che utilizza i criteri di memorizzazione nella cache. Il livello di memorizzazione nella cache includerà un `ProductsCL` classe che fornisce accesso alle informazioni sui prodotti con metodi come `GetProducts()`, `GetProductsByCategoryID(categoryID)`e così via, che, quando richiamata, verrà primo tentativo di recuperare i dati dalla cache. Se la cache è vuota, questi metodi richiamerà appropriata `ProductsBLL` BLL, che a sua volta otterrebbe i dati DAL metodo. Il `ProductsCL` metodi di memorizzare nella cache i dati recuperati da BLL prima di restituirlo.

Come illustrato nella figura 1, CL si trova tra la presentazione e il livello di logica di Business.


![La memorizzazione nella cache livello (CL) è un altro livello di architettura la nostra](caching-data-in-the-architecture-vb/_static/image1.png)

**Figura 1**: la memorizzazione nella cache livello (CL) è un altro livello di architettura la nostra


## <a name="step-1-creating-the-caching-layer-classes"></a>Passaggio 1: Creazione di classi del livello di memorizzazione nella cache

In questa esercitazione si creerà un CL molto semplice con una singola classe `ProductsCL` che ha solo un numero limitato di metodi. La creazione di un livello di memorizzazione nella cache completa per l'intera applicazione richiederebbe la creazione di `CategoriesCL`, `EmployeesCL`, e `SuppliersCL` classi e fornendo un meccanismo di queste classi di livello la memorizzazione nella cache per ogni metodo di accesso o la modifica di dati in BLL. Con BLL e DAL, idealmente il livello di memorizzazione nella cache deve essere implementato come un progetto libreria di classi separato. Tuttavia, verrà implementata, come una classe di `App_Code` cartella.

Per altre classi separate correttamente CL dalle classi DAL e BLL, s consentono di creare una nuova sottocartella nel `App_Code` cartella. Fare clic su di `App_Code` cartella in Esplora soluzioni, scegliere Nuova cartella e denominare la nuova cartella `CL`. Dopo aver creato la cartella, aggiungere una nuova classe denominata `ProductsCL.vb`.


![Aggiungere una nuova cartella denominata CL e una classe denominata ProductsCL.vb](caching-data-in-the-architecture-vb/_static/image2.png)

**Figura 2**: aggiungere una nuova cartella denominata `CL` e una classe denominata `ProductsCL.vb`


Il `ProductsCL` classe deve includere lo stesso set di metodi di accesso e modifica di dati come presente nella relativa classe di livello di logica di Business corrispondente (`ProductsBLL`). Anziché creare tutti questi metodi, s consentono solo compilazione qui un paio di acquisire familiarità con i modelli utilizzati da CL. In particolare, verrà aggiunto il `GetProducts()` e `GetProductsByCategoryID(categoryID)` metodi nel passaggio 3 e un `UpdateProduct` overload nel passaggio 4. È possibile aggiungere altri `ProductsCL` metodi e `CategoriesCL`, `EmployeesCL`, e `SuppliersCL` classi in base alle esigenze specifiche.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Passaggio 2: Leggere e scrivere nella Cache dei dati

ObjectDataSource la memorizzazione nella cache sono stati illustrati nell'esercitazione precedente internamente funzionalità utilizza la cache di dati ASP.NET per archiviare i dati recuperati da BLL. La cache dei dati sono accessibili anche a livello di codice da classi code-behind di pagine ASP.NET o dalle classi nell'architettura di s dell'applicazione web. Per leggere e scrivere la cache dei dati da una classe code-behind ASP.NET pagina s, usare il modello seguente:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample1.vb)]

Il [ `Cache` classe](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` metodo](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) ha un numero di overload. `Cache("key") = value` e `Cache.Insert(key, value)` sono sinonimi ed entrambi aggiungere un elemento alla cache utilizzando la chiave specificata senza una scadenza definita. In genere, è necessario specificare una scadenza quando si aggiunge un elemento alla cache, come una dipendenza, una scadenza basati sull'ora o entrambi. Utilizzare uno degli altri `Insert` overload del metodo s per fornire informazioni dipendenza - o basate sul tempo di scadenza.

Metodi s necessario per verificare se i dati richiesti nella cache e, in tal caso, il livello di memorizzazione nella cache restituirlo da qui. Se i dati richiesti non sono presente nella cache, deve essere richiamato il metodo BLL appropriato. Il valore restituito deve essere memorizzato nella cache e quindi restituito, come illustrato nel seguente diagramma di sequenza.


![I metodi di memorizzazione nella cache di livello s restituiscono dati dalla Cache, se si s disponibile](caching-data-in-the-architecture-vb/_static/image3.png)

**Figura 3**: il livello di memorizzazione nella cache s restituiscono dati dalla Cache Se si s disponibile


La sequenza illustrata nella figura 3 viene eseguita nelle classi CL usando il modello seguente:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample2.vb)]

In questo caso, *tipo* è il tipo di dati archiviati nella cache `Northwind.ProductsDataTable`, ad esempio *chiave* è la chiave che identifica in modo univoco l'elemento della cache. Se l'elemento con la proprietà specificata *chiave* non è nella cache, quindi *istanza* sarà `Nothing` e i dati verranno recuperati dal metodo BLL appropriato e aggiunto alla cache. Una volta `Return instance` viene raggiunto, *istanza* contiene un riferimento per i dati dalla cache o il pull da BLL.

Assicurarsi di utilizzare il modello precedente, quando l'accesso ai dati dalla cache. Il modello seguente, che, a prima vista, è equivalente, contiene una sottile differenza che introduce una race condition. Le condizioni di competizione sono difficili da eseguire il debug perché rivelare stessi sporadicamente e difficili da riprodurre.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample3.vb)]

La differenza in questo secondo frammento di codice non corretto è che anziché di archiviare un riferimento all'elemento memorizzato nella cache in una variabile locale, la cache dei dati si accede direttamente nell'istruzione condizionale *e* nel `Return`. Si supponga che, quando viene raggiunto questo codice, `Cache("key")` non `Nothing`, ma prima che il `Return` istruzione viene raggiunta, il sistema rimuove *chiave* dalla cache. In questi rari casi, verrà restituito il codice `Nothing` anziché un oggetto del tipo previsto.

> [!NOTE]
> La cache dei dati è thread-safe, pertanto non è necessario sincronizzare l'accesso di thread per semplice letture o scritture. Tuttavia, se è necessario eseguire più operazioni sui dati nella cache che devono essere atomici, è responsabile per l'implementazione di un blocco o un altro meccanismo per garantire la thread safety. Vedere [sincronizzare l'accesso alla Cache ASP.NET](http://www.ddj.com/184406369) per ulteriori informazioni.


Un elemento può essere a livello di codice rimossa dalla cache dei dati mediante il [ `Remove` metodo](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) come illustrato di seguito:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample4.vb)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Passaggio 3: Restituzione di informazioni sui prodotti dalla`ProductsCL`classe

Per questa esercitazione consente s implementare due metodi per la restituzione di informazioni sui prodotti dal `ProductsCL` classe: `GetProducts()` e `GetProductsByCategoryID(categoryID)`. Come con la `ProductsBL` classe nel livello di logica di Business, il `GetProducts()` metodo CL restituisce informazioni su tutti i prodotti come un `Northwind.ProductsDataTable` oggetto, mentre `GetProductsByCategoryID(categoryID)` restituisce tutti i prodotti da una categoria specificata.

Il codice seguente viene mostrata una parte dei metodi di `ProductsCL` classe:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample5.vb)]

In primo luogo, si noti il `DataObject` e `DataObjectMethodAttribute` gli attributi applicati alla classe e i metodi. Questi attributi forniscono informazioni per la procedura guidata s ObjectDataSource, che indica le classi e metodi devono essere visualizzato nella procedura guidata s. Poiché i metodi e classi CL verranno eseguiti da ObjectDataSource nel livello di presentazione, aggiunto questi attributi per migliorare l'esperienza in fase di progettazione. Fare riferimento al [la creazione di un livello di logica di Business](../introduction/creating-a-business-logic-layer-vb.md) per una descrizione più completa su questi attributi e gli effetti dell'esercitazione.

Nel `GetProducts()` e `GetProductsByCategoryID(categoryID)` metodi, i dati restituiti dal `GetCacheItem(key)` metodo viene assegnato a una variabile locale. Il `GetCacheItem(key)` metodo, che verranno esaminati al più presto, restituisce un elemento specifico dalla cache in base all'opzione *chiave*. Se tali dati non viene trovati nella cache, viene recuperato dal corrispondente `ProductsBLL` metodo della classe e quindi aggiunto alla cache utilizzando il `AddCacheItem(key, value)` metodo.

Il `GetCacheItem(key)` e `AddCacheItem(key, value)` metodi di interfaccia con la cache dei dati, lettura e scrittura dei valori, rispettivamente. Il `GetCacheItem(key)` metodo è più semplice delle due. Restituisce semplicemente il valore dalla classe Cache utilizzando passato *chiave*:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample6.vb)]

`GetCacheItem(key)` non utilizza *chiave* valore come specificato, ma che invece chiamate di `GetCacheKey(key)` metodo, che restituisce il *chiave* anteposta una n ProductsCache-. Il `MasterCacheKeyArray`, che contiene la stringa ProductsCache, viene utilizzato anche dal `AddCacheItem(key, value)` (metodo), come si vedrà solo temporaneamente.

Da una classe code-behind ASP.NET pagina s, la cache dei dati è possibile accedere tramite il `Page` classe s [ `Cache` proprietà](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)e consente una sintassi simile `Cache("key") = value`, come illustrato nel passaggio 2. Da una classe all'interno dell'architettura, la cache dei dati è possibile accedere utilizzando `HttpRuntime.Cache` o `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)del post di blog [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) note il vantaggio lieve miglioramento delle prestazioni di `HttpRuntime` anziché `HttpContext.Current`; di conseguenza, `ProductsCL` utilizza `HttpRuntime`.

> [!NOTE]
> Se l'architettura è implementata utilizzando i progetti libreria di classi, sarà necessario aggiungere un riferimento per il `System.Web` assembly per poter utilizzare il [ `HttpRuntime` ](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) e [ `HttpContext` ](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) classi.


Se l'elemento non viene trovato nella cache, il `ProductsCL` metodi della classe s ottenere i dati da BLL e aggiungerlo alla cache utilizzando il `AddCacheItem(key, value)` metodo. Per aggiungere *valore* nella cache è possibile usare il codice seguente, che utilizza una scadenza del tempo di 60 secondi:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample7.vb)]

`DateTime.Now.AddSeconds(CacheDuration)` Specifica la scadenza basati sull'ora 60 secondi in futuro mentre [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) non indica che in questo caso s alcuna scadenza. Durante questo `Insert` overload del metodo dispone di parametri di input per entrambi un assoluto e scorrevoli scadenza, è possibile specificare solo uno dei due. Se si tenta di specificare un tempo assoluto e un intervallo di tempo, il `Insert` metodo genererà un' `ArgumentException` eccezione.

> [!NOTE]
> Questa implementazione del `AddCacheItem(key, value)` metodo attualmente presenta alcuni difetti. Viene affrontare e risolvere questi problemi nel passaggio 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Passaggio 4: Invalidando così i dati di Cache quando viene modificato tramite l'architettura

Insieme ai metodi di recupero dei dati, il livello di memorizzazione nella cache deve fornire gli stessi metodi di BLL per inserimento, aggiornamento ed eliminazione di dati. I metodi di modifica dati s CL non modificano i dati memorizzati nella cache, ma piuttosto chiamare il metodo BLL s corrispondente dati Modifica e quindi invalidano la cache. Come illustrato nell'esercitazione precedente, questo è lo stesso comportamento che ObjectDataSource si applica quando sono abilitate le funzionalità di memorizzazione nella cache e il relativo `Insert`, `Update`, o `Delete` metodi vengono richiamati.

Le operazioni seguenti `UpdateProduct` overload viene illustrato come implementare i metodi di modifica di dati di CL:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample8.vb)]

La modifica dei dati appropriati metodo a livello di logica di Business viene richiamata, ma prima che venga restituita la risposta è necessario che invalidano la cache. Sfortunatamente, invalidazione della cache non è semplice perché il `ProductsCL` classe s `GetProducts()` e `GetProductsByCategoryID(categoryID)` metodi ogni consentono di aggiungere elementi alla cache con chiavi diverse e `GetProductsByCategoryID(categoryID)` metodo aggiunge un elemento della cache diverse per ogni univoco *categoryID*.

Quando l'invalidazione della cache, è necessario rimuovere *tutti* degli elementi che possono essere stati aggiunti per la `ProductsCL` classe. Questo può essere eseguito mediante l'associazione di un *della dipendenza dalla cache* con ogni elemento aggiunto alla cache nel `AddCacheItem(key, value)` metodo. In generale, una dipendenza della cache può essere un altro elemento nella cache, un file nel file system, o dati da un database di Microsoft SQL Server. Quando la dipendenza viene modificato o rimosso dalla cache, gli elementi della cache è associato vengono automaticamente rimossi dalla cache. Per questa esercitazione, si desidera creare un elemento aggiuntivo nella cache che agisce come una dipendenza della cache per tutti gli elementi aggiunti tramite la `ProductsCL` classe. In questo modo, tutti questi elementi possono essere rimossi dalla cache rimuovendo semplicemente la dipendenza della cache.

Aggiornamento s consentono di `AddCacheItem(key, value)` metodo in modo che ogni elemento aggiunto alla cache tramite questo metodo viene associato a una dipendenza di sola cache:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample9.vb)]

`MasterCacheKeyArray` è una matrice di stringhe che contiene un singolo valore, ProductsCache. Innanzitutto, un elemento della cache viene aggiunto alla cache e assegnato la data e ora correnti. Se l'elemento della cache esiste già, viene aggiornata. Successivamente, viene creata una dipendenza della cache. Il [ `CacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) costruttore s è un numero di overload, ma quella utilizzata in questa pagina prevede due `String` matrice di input. Il primo criterio specifica il set di file da utilizzare come dipendenze. Poiché non abbiamo da usare eventuali dipendenze basate su file, il valore t `Nothing` viene utilizzato per il primo parametro di input. Il secondo parametro di input specifica il set di chiavi della cache da utilizzare come dipendenze. In questo caso specifichiamo la dipendenza, `MasterCacheKeyArray`. Il `CacheDependency` viene quindi passato il `Insert` metodo.

Con questa modifica `AddCacheItem(key, value)`, invaliding la cache è semplice come rimuovendo la dipendenza.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample10.vb)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Passaggio 5: La chiamata a livello di memorizzazione nella cache dal livello di presentazione

Metodi e le classi di memorizzazione nella cache di livello s utilizzabile per funzionare con i dati utilizzando le tecniche è ve esaminate in queste esercitazioni. Per illustrare l'utilizzo dei dati memorizzati nella cache, salvare le modifiche per il `ProductsCL` classe e quindi aprire il `FromTheArchitecture.aspx` nella pagina di `Caching` cartella e aggiungere un controllo GridView. Da GridView s smart tag, creare un nuovo oggetto ObjectDataSource. Nel primo passaggio s guidata dovrebbe essere il `ProductsCL` classe come una delle opzioni nell'elenco a discesa.


[![La classe ProductsCL è incluso nell'elenco a discesa oggetto Business](caching-data-in-the-architecture-vb/_static/image5.png)](caching-data-in-the-architecture-vb/_static/image4.png)

**Figura 4**: il `ProductsCL` classe è inclusa nell'elenco a discesa oggetto Business ([fare clic per visualizzare l'immagine ingrandita](caching-data-in-the-architecture-vb/_static/image6.png))


Dopo aver selezionato `ProductsCL`, fare clic su Avanti. L'elenco a discesa nella scheda Seleziona ha due elementi - `GetProducts()` e `GetProductsByCategoryID(categoryID)` e la scheda aggiornamento è l'unico `UpdateProduct` rapporto di overload. Scegliere il `GetProducts()` metodo dalla scheda Seleziona e `UpdateProducts` metodo di aggiornamento e fare clic su Fine.


[![Sono racchiusi l'elenco a discesa sono elencati i metodi della classe ProductsCL s](caching-data-in-the-architecture-vb/_static/image8.png)](caching-data-in-the-architecture-vb/_static/image7.png)

**Figura 5**: il `ProductsCL` sono racchiusi l'elenco a discesa sono elencati i metodi della classe s ([fare clic per visualizzare l'immagine ingrandita](caching-data-in-the-architecture-vb/_static/image9.png))


Dopo aver completato la procedura guidata, Visual Studio verrà impostato il s ObjectDataSource `OldValuesParameterFormatString` proprietà `original_{0}` e aggiungere i campi appropriati a GridView. Modifica il `OldValuesParameterFormatString` proprietà sul valore predefinito, `{0}`e configurare il controllo GridView per supportare il paging, l'ordinamento e la modifica. Poiché il `UploadProducts` overload utilizzato da CL accetta solo il nome del prodotto modificato s e il prezzo, limitare il GridView in modo che solo questi campi sono modificabili.

Nell'esercitazione precedente è stato definito un controllo GridView per includere i campi per il `ProductName`, `CategoryName`, e `UnitPrice` campi. È possibile replicare la formattazione e la struttura, nel qual caso il s GridView e ObjectDataSource dichiarativa markup dovrebbe essere simile al seguente:


[!code-aspx[Main](caching-data-in-the-architecture-vb/samples/sample11.aspx)]

A questo punto si dispone di una pagina che utilizza il livello di memorizzazione nella cache. Per visualizzare la cache in azione, impostare punti di interruzione di `ProductsCL` classe s `GetProducts()` e `UpdateProduct` metodi. Visitare la pagina in un browser e di eseguire il codice durante l'ordinamento e paging per visualizzare i dati estratti dalla cache. Quindi, aggiornare un record e si noti che la cache viene invalidata e, di conseguenza, vengono recuperato da BLL quando i dati sono riassociati a GridView.

> [!NOTE]
> Il livello di memorizzazione nella cache fornito nel download di questo articolo non è stato completato. Contiene una sola classe, `ProductsCL`, quale Sport solo un numero limitato di metodi. Inoltre, una singola pagina ASP.NET utilizza CL (`~/Caching/FromTheArchitecture.aspx`) tutti gli altri fanno ancora riferimento BLL direttamente. Se si intende usare un CL nell'applicazione, tutte le chiamate dal livello di presentazione deve essere inoltrato al CL, che richiedono che le classi di s CL e metodi coperto tali classi e dei metodi BLL attualmente utilizzata dal livello di presentazione.


## <a name="summary"></a>Riepilogo

Durante la memorizzazione nella cache possono essere applicati i controlli di ObjectDataSource e il livello di presentazione con ASP.NET 2.0 s SqlDataSource, idealmente la memorizzazione nella cache le responsabilità sarebbe delegata a un livello separato dell'architettura della finestra. In questa esercitazione è stato creato un livello di memorizzazione nella cache che si trova tra il livello di presentazione e il livello di logica di Business. Il livello di memorizzazione nella cache deve fornire lo stesso set di classi e metodi che esistono nel BLL e vengono chiamati dal livello di presentazione.

Gli esempi di livello la memorizzazione nella cache è sono stati illustrati in questo esempio e le esercitazioni precedenti esibiti *caricamento reattivo*. Con caricamento reattivo, i dati vengono caricati nella cache solo quando viene effettuata una richiesta per i dati e i dati sono mancanti dalla cache. Dati possono anche essere *caricato in modo proattivo* nella cache, una tecnica che carica i dati nella cache prima che sia effettivamente necessario. Nella prossima esercitazione vedremo un esempio di caricamento attiva quando si esamina la modalità di archiviazione di valori statici nella cache all'avvio dell'applicazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](caching-data-with-the-objectdatasource-vb.md)
> [Successivo](caching-data-at-application-startup-vb.md)
