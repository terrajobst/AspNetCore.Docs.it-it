---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: La memorizzazione nella cache i dati all'avvio dell'applicazione (VB) | Microsoft Docs
author: rick-anderson
description: In qualsiasi applicazione Web alcuni dati verranno frequentemente utilizzati e alcuni dati verranno utilizzati raramente. Migliorare le prestazioni dei nostri ASP.NET applicazione b...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: 9d997cb0af8487b2a9d849f24f3bff8df8611f6d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829275"
---
<a name="caching-data-at-application-startup-vb"></a>La memorizzazione nella cache i dati all'avvio dell'applicazione (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> In qualsiasi applicazione Web alcuni dati verranno frequentemente utilizzati e alcuni dati verranno utilizzati raramente. È possibile migliorare le prestazioni dell'applicazione ASP.NET mediante il caricamento in anticipo i dati usati di frequente, una tecnica nota come. Questa esercitazione viene illustrato un approccio proattivo delle operazioni di caricamento, che consiste nel caricare i dati nella cache all'avvio dell'applicazione.


## <a name="introduction"></a>Introduzione

Le due esercitazioni precedenti esaminato la memorizzazione nella cache i dati della presentazione e i livelli di memorizzazione nella cache. Nelle [dati la memorizzazione nella cache con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md), è stata esaminata tramite la s ObjectDataSource la memorizzazione nella cache le funzionalità per memorizzare i dati nel livello di presentazione. [La memorizzazione nella cache i dati nell'architettura](caching-data-in-the-architecture-vb.md) esaminato la memorizzazione nella cache in un livello nuovo e separato di memorizzazione nella cache. Entrambe queste esercitazioni usate *caricamento reattivo* nell'uso di cache di dati. Con caricamento reattivo, ogni volta che vengono richiesti i dati, il sistema controlla innanzitutto se si s nella cache. In caso contrario, acquisisce i dati dall'origine origine, ad esempio il database e quindi lo archivia nella cache. Il vantaggio principale delle operazioni di caricamento reattivo è relativa semplicità di implementazione. Uno dei relativi svantaggi è le sue prestazioni non uniforme in tutte le richieste. Si immagini una pagina che usa il livello di memorizzazione nella cache dall'esercitazione precedente per visualizzare le informazioni sul prodotto. Quando questa pagina viene visitata per la prima volta o visitata per la prima volta dopo che i dati memorizzati nella cache sono stati eliminati a causa di limitazioni di memoria o la scadenza specificata che è stato raggiunto, i dati devono essere recuperati dal database. Pertanto, queste richieste di utenti richiederà più tempo rispetto alle richieste degli utenti che possono essere servite dalla cache.

*Caricamento proattivo* fornisce una strategia di gestione della cache alternativi che smussa le prestazioni tra le richieste per il caricamento dei dati memorizzati nella cache prima di esso necessari. Caricamento proattivo Usa in genere, un processo che periodicamente controlla oppure riceve una notifica quando si è verificato un aggiornamento ai dati sottostanti. Quindi, questo processo di aggiornamento della cache per mantenerla aggiornata. Attivo durante il caricamento è particolarmente utile se i dati sottostanti provengano da una connessione lenta al database, un servizio Web o un'altra origine dati particolarmente lenta. Ma questo approccio proattivo delle operazioni di caricamento è più difficile da implementare, poiché richiede la creazione, gestione e distribuzione di un processo per verificare la presenza di modifiche e aggiornare la cache.

Un'altra versione del caricamento proattivo e il tipo che è verranno illustrate in questa esercitazione, è il caricamento dei dati nella cache all'avvio dell'applicazione. Questo approccio è particolarmente utile per la memorizzazione nella cache i dati statici, ad esempio i record nelle tabelle di ricerca del database.

> [!NOTE]
> Per un approfondimento sulle differenze tra caricamento proattivo e reattivo, nonché elenchi di vantaggi, svantaggi e consigli di implementazione, vedere la [la gestione del contenuto di una Cache](https://msdn.microsoft.com/library/ms978503.aspx) sezione il [ Guida all'architettura delle applicazioni per .NET Framework di memorizzazione nella cache](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Passaggio 1: Determinare quali dati da memorizzare nella Cache all'avvio dell'applicazione

Gli esempi di memorizzazione nella cache usando il caricamento reattivo che abbiamo esaminato in precedente due esercitazioni elaborazione anche dati che possono cambiare periodicamente e non accettano exorbitantly lungo per generare. Ma se i dati memorizzati nella cache non cambiano mai, la scadenza usata da reattivo durante il caricamento è superflua. Analogamente, se i dati la memorizzazione nella cache richiede un periodo di tempo eccessivo per generare, tali utenti le cui richieste trovare vuoto cache sarà necessario tollerare una lunga attendere fino a quando i dati sottostanti vengono recuperati. Prendere in considerazione la memorizzazione nella cache i dati statici e i dati che richiede un tempo particolarmente lungo per generare all'avvio dell'applicazione.

Mentre i database hanno molti dinamico, i valori cambiano di frequente, la maggior parte anche avere una notevole quantità di dati statici. Praticamente in tutti i modelli di dati, ad esempio, avere una o più colonne che contengono un valore specifico da un set fisso di scelte. Oggetto `Patients` tabella di database potrebbe disporre di un `PrimaryLanguage` colonna, il cui set di valori potrebbe essere in lingua inglese, spagnolo, francese, russo, giapponese e così via. Spesso, questi tipi di colonne vengono implementati usando *tabelle di ricerca*. Invece di memorizzare la stringa di lingua inglese o in francese di `Patients` tabella, una seconda tabella viene creata, in genere, due colonne - identificatore univoco e una stringa descrittiva - con un record per ogni possibile valore. Il `PrimaryLanguage` colonna il `Patients` tabella archivia l'identificatore univoco corrispondente nella tabella di ricerca. Nella figura 1, paziente linguaggio primario s John Doe è inglese, mentre s Ed Johnson è russa.


![La tabella di lingue è una tabella di ricerca utilizzata dalla tabella Patients](caching-data-at-application-startup-vb/_static/image1.png)

**Figura 1**: il `Languages` la tabella è una tabella di ricerca usata dal `Patients` tabella


L'interfaccia utente per la modifica o si crea un nuovo paziente includerà un elenco di riepilogo a discesa delle lingue consentite popolata dal record nel `Languages` tabella. Senza la memorizzazione nella cache, ogni volta che questa interfaccia è visitato il sistema deve eseguire una query di `Languages` tabella. Si tratta gli sprechi poiché i valori di tabella di ricerca molto raramente, modificare mai.

È possibile memorizzare nella cache il `Languages` dati usando le stesse tecniche di caricamento reattivo esaminate nelle esercitazioni precedenti. Il caricamento reattivo, tuttavia, Usa una scadenza basati sul tempo, che non è necessaria per i dati nella tabella di ricerca statici. Anche se la memorizzazione nella cache tramite il caricamento reattivo sarebbe meglio nessuna memorizzazione nella cache, l'approccio migliore sarebbe caricare in modo proattivo i dati di tabella di ricerca nella cache all'avvio dell'applicazione.

In questa esercitazione si esaminerà come dati di tabella di ricerca nella cache e altre informazioni statiche.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Passaggio 2: Esaminare i diversi modi per memorizzare nella Cache dei dati

Le informazioni possono essere a livello di codice memorizzato nella cache in un'applicazione ASP.NET usando un'ampia gamma di approcci. È stato già spiegato come usare la cache dei dati nelle esercitazioni precedenti. In alternativa, gli oggetti possono essere a livello di programmazione memorizzati nella cache usando *i membri statici* oppure *lo stato dell'applicazione*.

Quando si usa una classe, in genere la classe deve essere creata un'istanza prima che i relativi membri sono accessibili. Ad esempio, per richiamare un metodo da una delle classi nel nostro Business Logic Layer, è necessario creare innanzitutto un'istanza della classe:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Prima che possiamo richiamare *SomeMethod* o utilizzare *SomeProperty*, è innanzitutto necessario creare un'istanza della classe utilizzando il `New` (parola chiave). *SomeMethod* e *SomeProperty* sono associati a una determinata istanza. La durata di questi membri è legata alla durata del relativo oggetto associato. *I membri statici*, d'altra parte, sono variabili, proprietà e metodi che vengono condivisi tra *tutto* istanze della classe e, di conseguenza, hanno una durata fin quando la classe. I membri statici vengono indicati dalla parola chiave `Shared`.

Oltre ai membri statici, i dati possono essere memorizzati nella cache dello stato dell'applicazione. Ogni applicazione ASP.NET gestisce una raccolta nome/valore che s condivise tra tutti gli utenti e le pagine dell'applicazione. Questa raccolta sono accessibili tramite il [ `HttpContext` classe](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` proprietà](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)e usato da una classe code-behind di pagina s ASP.NET come segue:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

La cache dei dati fornisce un'API molto più completa per la memorizzazione nella cache i dati, che fornisce meccanismi per oggetti scaduti nella basati su tempo e delle dipendenze delle priorità degli elementi della cache e così via. Con membri statici e lo stato dell'applicazione, queste funzionalità devono essere aggiunte manualmente dallo sviluppatore della pagina. Quando la memorizzazione nella cache di dati all'avvio dell'applicazione per la durata dell'applicazione, tuttavia, i vantaggi di cache s dei dati sono opinabile. In questa esercitazione verrà esaminato il codice che utilizza tutti e tre le tecniche per la memorizzazione nella cache i dati statici.

## <a name="step-3-caching-thesupplierstable-data"></a>Passaggio 3: La memorizzazione nella cache il`Suppliers`tabella dati

Northwind tabelle di database a è ve implementata a oggi non includono alcun tabelle di ricerca tradizionale. I quattro oggetti DataTable implementata nel nostro DAL tutte le tabelle del modello i cui valori sono non statici. Invece di impiegare il tempo per aggiungere un nuovo DataTable DAL e quindi una nuova classe e metodi per il livello BLL, per questa esercitazione lasciare s appena fingendo che il `Suppliers` i dati della tabella s sono statici. Pertanto, potremmo nella cache i dati all'avvio dell'applicazione.

Per iniziare, creare una nuova classe denominata `StaticCache.cs` nella `CL` cartella.


![Creare la classe StaticCache.vb nella cartella CL](caching-data-at-application-startup-vb/_static/image2.png)

**Figura 2**: creare le `StaticCache.vb` classe la `CL` cartella


È necessario aggiungere un metodo che carica i dati all'avvio nell'archivio cache appropriata, nonché i metodi che restituiscono i dati da questa cache.


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

Il codice precedente Usa una variabile membro statico, `suppliers`, per contenere i risultati del `SuppliersBLL` classe s `GetSuppliers()` metodo, che viene chiamato dal `LoadStaticCache()` (metodo). Il `LoadStaticCache()` metodo è destinato a essere chiamato durante l'avvio di s dell'applicazione. Una volta che questi dati sono stati caricati all'avvio dell'applicazione, è possono chiamare tutte le pagine che devono essere utilizzato con i dati fornitore il `StaticCache` classe s `GetSuppliers()` (metodo). Pertanto, la chiamata al database per ottenere i fornitori si verifica solo una volta, all'avvio dell'applicazione.

Invece di usare una variabile membro statica come l'archivio della cache, si sarebbe potuto in alternativa usare lo stato dell'applicazione o la cache dei dati. Il codice seguente viene illustrata la classe retooled per usare lo stato dell'applicazione:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

Nelle `LoadStaticCache()`, alla variabile dell'applicazione vengono archiviate le informazioni di supplier *chiave*. Lo s restituito come tipo appropriato (`Northwind.SuppliersDataTable`) da `GetSuppliers()`. Mentre lo stato dell'applicazione sono accessibili nelle classi di pagine ASP.NET con code-behind `Application("key")`, nell'architettura è necessario usare `HttpContext.Current.Application("key")` per ottenere l'oggetto corrente `HttpContext`.

Analogamente, la cache dei dati può costituire un archivio della cache, come illustrato nel codice seguente:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Per aggiungere un elemento alla cache dei dati senza alcuna scadenza basati sul tempo, usare il `System.Web.Caching.Cache.NoAbsoluteExpiration` e `System.Web.Caching.Cache.NoSlidingExpiration` valori come parametri di input. Questo specifico overload della cache dei dati s `Insert` metodo è stato selezionato in modo che potremmo specificare il *priorità* dell'elemento della cache. Consente di determinare quali elementi eliminare dalla cache quando la memoria disponibile è insufficiente per la priorità. Qui viene usata la priorità `NotRemovable`, che assicura che questo elemento della cache ha vinto t scavenging.

> [!NOTE]
> Questo download di esercitazione s implementa il `StaticCache` classe Usa l'approccio di variabile membro statico. Il codice per le tecniche di cache dello stato e i dati dell'applicazione è disponibile nei commenti nel file di classe.


## <a name="step-4-executing-code-at-application-startup"></a>Passaggio 4: Esecuzione di codice all'avvio dell'applicazione

Per eseguire il codice al primo avvio di un'applicazione web, è necessario creare un file speciale denominato `Global.asax`. Questo file può contenere i gestori eventi per l'applicazione-, sessione-, e gli eventi a livello di richiesta ed è qui in cui è possibile aggiungere codice che verrà eseguito ogni volta che viene avviata l'applicazione.

Aggiungi il `Global.asax` file alla directory radice s facendo clic sul nome del progetto sito Web in Visual Studio s Esplora soluzioni e scegliendo Aggiungi nuovo elemento dell'applicazione web. Nella finestra di dialogo Aggiungi nuovo elemento selezionare il tipo di elemento di classe di applicazione globale e quindi fare clic sul pulsante Aggiungi.

> [!NOTE]
> Se hai già un `Global.asax` file nel progetto, la classe di applicazione globale, tipo di elemento non verrà elencato nella finestra di dialogo Aggiungi nuovo elemento.


[![Aggiungere il File Global. asax alla Directory radice s dell'applicazione Web](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Figura 3**: aggiungere i `Global.asax` File dell'applicazione Web s Directory radice ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-at-application-startup-vb/_static/image5.png))


Il valore predefinito `Global.asax` file modello include cinque metodi all'interno di un server-side `<script>` tag:

- **`Application_Start`** viene eseguito quando l'applicazione web di primo avvio
- **`Application_End`** viene eseguito quando l'applicazione è in fase di arresto
- **`Application_Error`** viene eseguita ogni volta che un'eccezione non gestita raggiunge l'applicazione
- **`Session_Start`** viene eseguito quando viene creata una nuova sessione
- **`Session_End`** viene eseguito quando una sessione è scaduta o abbandonata

Il `Application_Start` gestore eventi viene chiamato una sola volta durante un ciclo di vita dell'applicazione s. L'applicazione viene avviata la prima volta che può verificarsi modificando il contenuto di una risorsa ASP.NET viene richiesto dall'applicazione e continua l'esecuzione fino a quando l'applicazione viene riavviata, il `/Bin` cartella, modifica `Global.asax`, modifica il contenuto nel `App_Code` cartella o la modifica di `Web.config` file, fra le altre cause. Fare riferimento a [Panoramica del ciclo di vita di applicazioni ASP.NET](https://msdn.microsoft.com/library/ms178473.aspx) per una discussione più dettagliata sul ciclo di vita dell'applicazione.

Per queste esercitazioni è sufficiente aggiungere il codice per il `Application_Start` metodo, in tal caso è possibile rimuovere gli altri. Nelle `Application_Start`, è sufficiente chiamare il `StaticCache` classe s `LoadStaticCache()` metodo, che verrà caricato e memorizzare nella cache le informazioni sui fornitori:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

Che s ne è a esso. All'avvio dell'applicazione, il `LoadStaticCache()` acquisire informazioni sul fornitore da BLL, metodo e archiviarlo in una variabile membro statica (o qualsiasi cache di memorizzare si finisce utilizzando il `StaticCache` classe). Per verificare questo comportamento, impostare un punto di interruzione il `Application_Start` metodo ed eseguire l'applicazione. Si noti che il punto di interruzione viene raggiunto dopo l'avvio dell'applicazione. Le richieste successive, tuttavia, non causano la `Application_Start` metodo da eseguire.


[![Usare un punto di interruzione per verificare che il gestore dell'evento Application_Start sia in esecuzione](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Figura 4**: usare un punto di interruzione per verificare che il `Application_Start` gestore dell'evento è in esecuzione ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-at-application-startup-vb/_static/image8.png))


> [!NOTE]
> Se non si raggiunge il `Application_Start` punto di interruzione quando si inizia il debug, è perché l'applicazione è già avviato. Forzare l'applicazione di riavviare modificando la `Global.asax` o `Web.config` file, quindi riprovare. È possibile semplicemente aggiungere (o rimuovere) una riga vuota alla fine di uno di questi file di riavviare rapidamente l'applicazione.


## <a name="step-5-displaying-the-cached-data"></a>Passaggio 5: Visualizzare i dati memorizzati nella cache

A questo punto il `StaticCache` classe dispone di una versione dei dati fornitore nella cache all'avvio dell'applicazione che è possibile accedere tramite relativo `GetSuppliers()` (metodo). Per lavorare con i dati dal livello di presentazione, è possibile utilizzare ObjectDataSource o richiamare a livello di codice le `StaticCache` classe s `GetSuppliers()` metodo da una classe code-behind pagina s ASP.NET. Let s spiega come usare i controlli di ObjectDataSource e GridView per visualizzare le informazioni memorizzate nella cache fornitore.

Iniziare aprendo il `AtApplicationStartup.aspx` nella pagina di `Caching` cartella. Trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` proprietà `Suppliers`. Successivamente, da GridView tag smart s scegliere di creare un nuovo oggetto ObjectDataSource denominato `SuppliersCachedDataSource`. Configurare ObjectDataSource per usare la `StaticCache` classe s `GetSuppliers()` (metodo).


[![Configurare ObjectDataSource per usare la classe StaticCache](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Figura 5**: configurare ObjectDataSource per usare il `StaticCache` classe ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-at-application-startup-vb/_static/image11.png))


[![Usare il metodo GetSuppliers() per recuperare i dati memorizzati nella cache del fornitore](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Figura 6**: usare il `GetSuppliers()` metodo per recuperare i dati memorizzati nella cache del fornitore ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-at-application-startup-vb/_static/image14.png))


Dopo aver completato la procedura guidata, Visual Studio aggiungerà automaticamente BoundField per ognuno dei campi dati `SuppliersDataTable`. Il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Figura 7 mostra la pagina quando viene visualizzato tramite un browser. L'output è lo stesso se avessimo sono estratti i dati da s BLL `SuppliersBLL` classe, ma l'uso di `StaticCache` classe restituisce i dati fornitore come memorizzati nella cache all'avvio dell'applicazione. È possibile impostare i punti di interruzione nella `StaticCache` classe s `GetSuppliers()` metodo per verificare questo comportamento.


[![I dati fornitore memorizzato nella cache vengono visualizzati in un oggetto GridView](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Figura 7**: il dati fornitore memorizzato nella cache vengono visualizzati in un controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-at-application-startup-vb/_static/image17.png))


## <a name="summary"></a>Riepilogo

La maggior parte delle ogni modello di dati contiene una notevole quantità di dati statici, in genere implementati sotto forma di tabelle di ricerca. Poiché queste informazioni sono statiche, qui 1!s nessun motivo per l'accesso continuo al database ogni volta che queste informazioni devono essere visualizzati. Inoltre, grazie alla sua natura statica, quando la memorizzazione nella cache i dati lì s non è necessaria una scadenza. In questa esercitazione è stato illustrato come sfruttare tali dati e memorizzare nella cache nella cache dei dati, lo stato dell'applicazione e tramite una variabile membro statica. Queste informazioni sono memorizzato nella cache all'avvio dell'applicazione e rimangono nella cache per l'intera durata dell'applicazione s.

In questa esercitazione e i due precedenti, è stato preso in esame la memorizzazione nella cache i dati per la durata della durata s dell'applicazione, nonché tramite oggetti scaduti nella basati sul tempo. Quando la memorizzazione nella cache i dati del database, tuttavia, una scadenza basati sul tempo può essere inferiore a quello ideale. Invece di scaricare periodicamente la cache, sarebbe ottimale per rimuovere solo l'elemento memorizzato nella cache quando vengono modificati i dati di database sottostante. Questa soluzione ideale è possibile grazie all'utilizzo di dipendenze della cache SQL, che verrà esaminato nell'esercitazione successiva.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Teresa Murphy e Zack Jones. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](caching-data-in-the-architecture-vb.md)
> [Successivo](using-sql-cache-dependencies-vb.md)
