---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: La memorizzazione nella cache i dati con ObjectDataSource (c#) | Microsoft Docs
author: rick-anderson
description: La memorizzazione nella cache può fare la differenza tra una lenta e una rapida applicazione Web. Questa esercitazione è la prima delle quattro proprietà che accettano un quadro dettagliato di memorizzazione nella cache in ASP.NET...
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: f6ca84dc11eb8ecd03aee91198e74b723cfdce7c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803045"
---
<a name="caching-data-with-the-objectdatasource-c"></a>La memorizzazione nella cache i dati con ObjectDataSource (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) o [Scarica il PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> La memorizzazione nella cache può fare la differenza tra una lenta e una rapida applicazione Web. Questa esercitazione è la prima delle quattro proprietà che accettano un quadro dettagliato di memorizzazione nella cache in ASP.NET. Informazioni su come applicare la memorizzazione nella cache a livello di presentazione tramite il controllo ObjectDataSource e i concetti principali di memorizzazione nella cache.


## <a name="introduction"></a>Introduzione

In informatica, *memorizzazione nella cache* è il processo di recupero dati o informazioni che è dispendiosa ottenere e archiviare una copia in una posizione che è più veloce per l'accesso. Per le applicazioni guidate dai dati, le query di grandi e complesse usano in genere la maggior parte del tempo di esecuzione dell'applicazione s. Questo tipo una s le prestazioni dell'applicazione, quindi, spesso possono essere migliorate, archiviando i risultati delle query di database costose nella memoria dell'applicazione s.

ASP.NET 2.0 offre un'ampia gamma di opzioni di memorizzazione nella cache. Un'intera pagina web o markup s viene eseguito il rendering del controllo utente possono essere memorizzati nella cache attraverso *memorizzazione nella cache di output*. I controlli SqlDataSource e ObjectDataSource forniscono funzionalità di memorizzazione anche, consentendo di dati da memorizzare nella cache a livello di controllo. E ASP.NET s' *cache dei dati* offre un'API di memorizzazione nella cache completa che consente agli sviluppatori di pagina a livello di programmazione gli oggetti della cache. In questa esercitazione e i successivi tre che verranno presi in esame usando gli oggetti ObjectDataSource la memorizzazione nella cache le funzionalità, nonché la cache dei dati. Verrà anche illustrato come memorizzare nella cache i dati a livello di applicazione all'avvio e su come mantenere i dati memorizzati nella cache aggiornata tramite l'utilizzo di dipendenze della cache SQL. Queste esercitazioni illustrano non la memorizzazione nella cache di output. Per un'analisi approfondita la memorizzazione nella cache di output, vedere [la memorizzazione nella cache di Output di ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

La memorizzazione nella cache possono essere applicate in qualsiasi luogo nell'architettura, dal livello di accesso ai dati backup tramite il livello di presentazione. In questa esercitazione verrà esaminato l'applicazione di memorizzazione nella cache a livello di presentazione tramite il controllo ObjectDataSource. Nella prossima esercitazione che verrà esaminato la memorizzazione nella cache i dati a livello della logica di Business.

## <a name="key-caching-concepts"></a>La memorizzazione nella cache i concetti chiave

La memorizzazione nella cache può migliorare notevolmente una s applicazione complessiva delle prestazioni e scalabilità prendendo i dati che è dispendioso generare e archiviare una copia in una posizione accessibile in modo più efficiente. Poiché la cache contiene solo una copia dei dati effettivi, sottostante, possono diventare obsoleto, oppure *obsoleti*, se i dati sottostanti cambiano. Per evitare questo inconveniente, lo sviluppatore della pagina può indicare i criteri mediante il quale l'elemento della cache verrà *rimossi* dalla cache, utilizzando:

- **I criteri basati sul tempo** può essere aggiunto un elemento nella cache per un assoluta o variabile di durata. Ad esempio, lo sviluppatore della pagina può indicare una durata di, ad esempio, 60 secondi. Con una durata assoluta, l'elemento memorizzato nella cache viene eliminato 60 secondi dopo che è stato aggiunto alla cache, indipendentemente dalla frequenza con cui l'accesso. Con una durata temporale scorrevole, l'elemento memorizzato nella cache viene eliminato dopo l'ultimo accesso di 60 secondi.
- **Criteri basati su dipendenza** una dipendenza può essere associata a un elemento quando aggiunto alla cache. Quando viene modificato l'elemento s dipendenza viene rimosso dalla cache. La dipendenza può essere un file, un altro elemento della cache o una combinazione dei due. ASP.NET 2.0 consente anche le dipendenze della cache SQL che consentono agli sviluppatori di aggiungere un elemento alla cache che verrà rimosso quando cambiano i dati di database sottostanti. Verranno esaminate le dipendenze della cache SQL nell'imminente [usando le dipendenze della Cache di SQL](using-sql-cache-dependencies-cs.md) esercitazione.

Indipendentemente dai criteri di rimozione specificati, potrebbe essere un elemento nella cache *scavenging* prima i criteri basati sul tempo o basata su dipendenza è stata soddisfatta. Se la cache ha raggiunto la capacità, è necessario rimuovere gli elementi esistenti prima di potere aggiungere quelli nuovi. Di conseguenza, quando a livello di codice si lavora con dati memorizzati nella cache, s fondamentali che è sempre presupporre che i dati memorizzati nella cache potrebbero non essere presenti. Verrà esaminato il modello da utilizzare per l'accesso ai dati dalla cache a livello di codice in questa esercitazione successiva *dati la memorizzazione nella cache nell'architettura*.

La memorizzazione nella cache rappresenta un modo economico per stesso come concentrare prestazioni più elevate da un'applicazione. Come [Steven Smith](http://aspadvice.com/blogs/ssmith/) indica nel suo articolo [la memorizzazione nella cache di ASP.NET: tecniche e procedure consigliate](https://msdn.microsoft.com/library/aa478965.aspx):

La memorizzazione nella cache può essere un buon metodo per ottenere buone prestazioni adeguate senza richiedere parecchio tempo e l'analisi. Memoria è economica, pertanto, se è possibile ottenere le prestazioni desiderate per la memorizzazione nella cache l'output per 30 secondi, anziché dedicare un giorno o settimana tenta di ottimizzare il codice o un database, eseguire la soluzione di memorizzazione nella cache (presupponendo che 30 - secondo i dati meno recenti è ok) e continuare. Infine, costituite da scarsa progettazione si probabilmente riprenderà all'utente, in modo che naturalmente è consigliabile provare a progettare correttamente le applicazioni. Ma se è necessario solo ottenere buone prestazioni adeguate oggi, la memorizzazione nella cache può essere un eccellente [approccio], il tempo necessario per l'applicazione in un secondo momento quando si ha tempo a scopo di effettuare il refactoring di acquisto.

Anche se la memorizzazione nella cache può fornire miglioramenti significativi delle prestazioni, non è applicabile in tutte le situazioni, ad esempio con le applicazioni che usano dati in tempo reale, aggiorna continuamente o in cui i dati non aggiornati anche a breve durata sono accettabile. Ma per la maggior parte delle applicazioni, la memorizzazione nella cache deve essere usato. Per maggiori informazioni sulla memorizzazione nella cache in ASP.NET 2.0, vedere la [memorizzazione nella cache per le prestazioni](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) sezione del [ASP.NET 2.0 QuickStart Tutorials](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Passaggio 1: Creazione di memorizzazione nella cache le pagine Web

Prima di iniziare l'esplorazione delle funzionalità di caching s ObjectDataSource, consentire s prima di tutto si consiglia di creare le pagine ASP.NET nel progetto di sito Web che è necessario per questa esercitazione e i successivi tre. Iniziare aggiungendo una nuova cartella denominata `Caching`. Successivamente, aggiungere le seguenti pagine ASP.NET per quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative alla memorizzazione nella cache](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni relative alla memorizzazione nella cache


In altre cartelle, analogo a `Default.aspx` nella `Caching` cartella elencherà le esercitazioni nella relativa sezione. Si tenga presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere questo controllo utente da `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Figura 2: Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Figura 2**: nella figura 2: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-with-the-objectdatasource-cs/_static/image4.png))


Infine, aggiungere queste pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo l'utilizzo di dati binari `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, si consiglia di visualizzare il sito Web di esercitazioni tramite un browser. Il menu a sinistra ora include elementi per le esercitazioni di memorizzazione nella cache.


![Mappa del sito include ora voci per le esercitazioni di memorizzazione nella cache](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Figura 3**: la mappa del sito include ora voci per le esercitazioni di memorizzazione nella cache


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Passaggio 2: Visualizzare un elenco di prodotti in una pagina Web

Questa esercitazione illustra come usare ObjectDataSource controllo s incorporate funzionalità di caching. Prima di poter esaminare queste funzionalità, tuttavia, è innanzitutto necessario di lavorare da una pagina. S ti permettono di creare una pagina web che usa un controllo GridView per elencare le informazioni di prodotto recuperate da ObjectDataSource dal `ProductsBLL` classe.

Iniziare aprendo il `ObjectDataSource.aspx` nella pagina di `Caching` cartella. Trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, impostare relativi `ID` proprietà `Products`, quindi, dal suo smart tag da associare a un nuovo controllo ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per lavorare con i `ProductsBLL` classe.


[![Configurare ObjectDataSource per usare la classe ProductsBLL](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Figura 4**: configurare ObjectDataSource per usare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-with-the-objectdatasource-cs/_static/image8.png))


Per questa pagina, consentire s di creare un controllo GridView modificabili in modo che possa essere esaminato cosa accade quando vengono modificati i dati memorizzati nella cache in ObjectDataSource tramite l'interfaccia s GridView. Lasciare l'elenco a discesa nella scheda Imposta sul valore predefinito, seleziona `GetProducts()`, ma modificare l'elemento selezionato nella scheda aggiornamenti per il `UpdateProduct` overload che accetta `productName`, `unitPrice`, e `productID` come i parametri di input.


[![Impostare l'elenco di riepilogo a discesa aggiornamento scheda s sull'Overload appropriato UpdateProduct](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Figura 5**: impostare la scheda aggiornamento s elenco a discesa scegliere per le Appropriate `UpdateProduct` eseguire l'Overload ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-with-the-objectdatasource-cs/_static/image11.png))


Infine, impostare gli elenchi a discesa nelle schede INSERT e DELETE su (nessuno) e fare clic su Fine. Dopo aver completato la procedura guidata Configura origine dati, Visual Studio imposta la s ObjectDataSource `OldValuesParameterFormatString` proprietà `original_{0}`. Come descritto nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esercitazione, questa proprietà deve essere rimosso dalla sintassi dichiarativa o reimpostate sul valore predefinito, `{0}`, affinché l'aggiornamento del flusso di lavoro continuare senza errori.

Inoltre, al termine della procedura guidata Visual Studio aggiunge un campo a GridView per ognuno dei campi di dati del prodotto. Rimuovi tutto tranne le `ProductName`, `CategoryName`, e `UnitPrice` BoundField. A questo punto, aggiornare il `HeaderText` le proprietà della ognuno di questi BoundField per prodotto, categoria e prezzo, rispettivamente. Poiché il `ProductName` campo è obbligatorio, convertire i BoundField in un TemplateField e aggiungere un RequiredFieldValidator al `EditItemTemplate`. Analogamente, convertire il `UnitPrice` BoundField in un TemplateField e aggiungere un controllo CompareValidator per assicurarsi che il valore immesso dall'utente sia valore di valuta valida che s maggiore o uguale a zero. Oltre a queste modifiche, è possibile eseguire le modifiche esterne, ad esempio allineare a destra il `UnitPrice` valore o la specifica la formattazione per il `UnitPrice` testo nelle proprie interfacce di sola lettura e la modifica.

Rendere modificabile il controllo GridView selezionando la casella di controllo Abilita modifica nello smart tag s GridView. Controllare anche le caselle di controllo Attiva Paging e abilitare l'ordinamento.

> [!NOTE]
> Richiedono un'attenzione da parte di come personalizzare l'interfaccia di modifica s GridView? In questo caso, vedere la [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) esercitazione.


[![Abilitare il supporto di GridView per la modifica, l'ordinamento e Paging](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Figura 6**: abilitare il supporto GridView per la modifica, l'ordinamento e Paging ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-with-the-objectdatasource-cs/_static/image14.png))


Dopo aver apportato queste modifiche di GridView, il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Come illustrato nella figura 7, il controllo GridView modificabile Elenca il nome, categoria e prezzo della ognuno dei prodotti nel database. Si consiglia di testare i risultati, spostarsi tra loro, l'ordinamento di funzionalità pagina s e modificare un record.


[![Ogni nome di prodotto, categoria e il prezzo è elencati in un ordinabile, Pageable, GridView modificabile](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Figura 7**: ogni prodotto s nome, categoria e il prezzo è elencati in un ordinabile, Pageable, GridView modificabile ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-with-the-objectdatasource-cs/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Passaggio 3: Esame ObjectDataSource the quando è richiesta dati

Il `Products` GridView recupera i dati da visualizzare tramite la chiamata di `Select` metodo il `ProductsDataSource` ObjectDataSource. ObjectDataSource crea un'istanza di s Business Logic Layer `ProductsBLL` classi e le chiamate relative `GetProducts()` metodo, che a sua volta chiama il livello di accesso ai dati di s `ProductsTableAdapter` s `GetProducts()` (metodo). Il metodo DAL si connette al database Northwind e problemi dell'applicazione configurata `SELECT` query. Questi dati viene quindi restituiti al DAL, li Assembla backup in un `NorthwindDataTable`. L'oggetto DataTable viene restituito per il livello BLL, che lo restituisce a ObjectDataSource, che lo restituisce a GridView. GridView crea quindi una `GridViewRow` per ciascun `DataRow` DataTable, mentre ogni `GridViewRow` alla fine viene eseguito il rendering in formato HTML di cui viene restituito al client e visualizzato nel browser del visitatore s.

Questa sequenza di eventi si verifica ogni volta che il controllo GridView deve essere associato ai relativi dati sottostanti. Ciò si verifica quando viene innanzitutto visitata, quando si spostano da una pagina di dati a un'altra, durante l'ordinamento di GridView, o quando si modificano i dati di s GridView mediante relativo predefinito, modificare o eliminare le interfacce. Se lo stato di visualizzazione GridView s è disabilitato, il controllo GridView verrà riassociato su ogni postback anche. GridView possa anche essere esplicitamente riassociato ai relativi dati chiamando il `DataBind()` (metodo).

Per apprezzare pienamente la frequenza con cui i dati vengono recuperati dal database, lasciare s visualizzato un messaggio che indica quando i dati vengono recuperati nuovamente. Aggiungere un controllo etichetta Web sopra il controllo GridView denominato `ODSEvents`. Cancellare il contenuto relativo `Text` proprietà e set relativo `EnableViewState` proprietà `false`. Sotto l'etichetta, aggiungere un controllo Web pulsante e impostarne il `Text` proprietà per il Postback.


[![Aggiungere un'etichetta e un pulsante alla pagina sopra il controllo GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Figura 8**: aggiungere un'etichetta e un pulsante alla pagina sopra il controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-with-the-objectdatasource-cs/_static/image20.png))


Durante il flusso di lavoro di accesso dati s ObjectDataSource `Selecting` viene generato l'evento prima che venga creato l'oggetto sottostante e il relativo metodo configurato richiamato. Creare un gestore eventi per questo evento e aggiungere il codice seguente:


[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Ogni volta che ObjectDataSource effettua una richiesta per l'architettura per i dati, l'etichetta visualizzerà l'evento di selezione di testo generato.

Visitare questa pagina in un browser. Quando la pagina viene prima visitata, viene visualizzato l'evento di selezione di testo generato. Fare clic sul pulsante di Postback e notare che il testo scomparirà (supponendo che i dispositivi di GridView `EnableViewState` è impostata su `true`, il valore predefinito). Infatti, durante il postback, il controllo GridView viene ricostruito dal relativo stato di visualizzazione e pertanto t disattivare a ObjectDataSource per i propri dati. L'ordinamento, paging, o la modifica dei dati, tuttavia, provoca GridView riassociare l'origine dati, e pertanto l'evento di selezione generato viene visualizzato nuovamente il testo.


[![Ogni volta che il controllo GridView è riassociato alla relativa origine dati, viene visualizzato quando si seleziona evento generato](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Figura 9**: GridView in ogni volta che è riassociata alla relativa origine dati, viene visualizzato se si seleziona evento attivato ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-with-the-objectdatasource-cs/_static/image23.png))


[![Facendo clic su pulsante Postback fa sì che il controllo GridView per essere ricostruiti dal relativo stato di visualizzazione](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Figura 10**: fare clic sul pulsante Postback fa sì che il controllo GridView essere ricostruiti dal relativo stato di visualizzazione ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-with-the-objectdatasource-cs/_static/image26.png))


Può sembrare inutili recuperare i dati del database ogni volta che i dati di paging tramite o ordinati. Dopo tutto, poiché si stia usando il paging predefinito, ObjectDataSource ha recuperato tutti i record quando si visualizzano la prima pagina. Anche se il controllo GridView non fornisce l'ordinamento e il supporto di paging, i dati devono essere recuperati dal database ogni volta che viene prima visitata da altri utenti (e in ogni postback, se lo stato di visualizzazione è disabilitato). Ma se il controllo GridView verrà visualizzati gli stessi dati a tutti gli utenti, queste richieste di database aggiuntivi sono superflue. Perché non memorizzare nella cache i risultati restituiti dai `GetProducts()` metodo e associare il controllo GridView a quelli memorizzati nella cache i risultati?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Passaggio 4: La memorizzazione nella cache i dati con ObjectDataSource

Semplicemente impostando alcune proprietà, ObjectDataSource può essere configurato per automaticamente nella cache i dati recuperati nella cache dei dati ASP.NET. Nell'elenco seguente vengono riepilogate le proprietà correlate alla cache di ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) deve essere impostata su `true` per abilitare la memorizzazione nella cache. Il valore predefinito è `false`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) la quantità di tempo, espresso in secondi, che viene memorizzato nella cache i dati. Il valore predefinito è 0. ObjectDataSource solo memorizza nella cache i dati se `EnableCaching` viene `true` e `CacheDuration` è impostata su un valore maggiore di zero.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) può essere impostata su `Absolute` o `Sliding`. Se `Absolute`, l'oggetto ObjectDataSource memorizza nella cache i dati recuperati per `CacheDuration` secondi; se `Sliding`, i dati scadono solo dopo che non ha ricevuto accessi per `CacheDuration` secondi. Il valore predefinito è `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) utilizzare questa proprietà per associare le voci della cache di ObjectDataSource s con una dipendenza della cache esistente. Le voci di dati di ObjectDataSource s è possibile dedurlo prematuramente dalla cache impostando come scaduti a esso associata `CacheKeyDependency`. Questa proprietà viene in genere utilizzata per associare una dipendenza della cache SQL con la cache s ObjectDataSource, un argomento esamineremo qui in futuro [usando le dipendenze della Cache di SQL](using-sql-cache-dependencies-cs.md) esercitazione.

Configurare automaticamente s il `ProductsDataSource` ObjectDataSource per memorizzare nella cache i dati per 30 secondi su una scala assoluta. Impostare la s ObjectDataSource `EnableCaching` proprietà `true` e il relativo `CacheDuration` proprietà su 30. Lasciare il `CacheExpirationPolicy` impostata sul valore predefinito, proprietà `Absolute`.


[![Configurare ObjectDataSource per memorizzare nella Cache i dati per 30 secondi](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Figura 11**: configurare ObjectDataSource per memorizzare nella Cache i dati per 30 secondi ([fare clic per visualizzare l'immagine con dimensioni normali](caching-data-with-the-objectdatasource-cs/_static/image29.png))


Salvare le modifiche e visitare di nuovo questa pagina in un browser. Il testo dell'evento generato quando si seleziona rimarrà durante la prima visita la pagina, inizialmente i dati non sono nella cache. A differenza di postback successivi attivato facendo clic sul pulsante di Postback, l'ordinamento, paging, o utilizzare i pulsanti di modifica o annullamento *non* nuovamente l'evento di selezione viene generato il testo. Infatti, il `Selecting` evento viene generato solo quando l'oggetto ObjectDataSource Ottiene i dati dal relativo oggetto sottostante; il `Selecting` evento non viene generato se i dati vengono spostati dalla cache dei dati.

Dopo 30 secondi, i dati verranno rimossi dalla cache. I dati verranno rimossi anche dalla cache se s ObjectDataSource `Insert`, `Update`, o `Delete` vengono richiamati i metodi. Di conseguenza, dopo che sono stati superati i 30 secondi o il pulsante di aggiornamento è stato fatto clic, ordinamento, paging, oppure fare clic sui pulsanti di modifica o annullamento causerà ObjectDataSource per ottenerne i dati dal relativo oggetto sottostante, la visualizzazione dell'evento di selezione attivato testo quando il `Selecting` viene generato l'evento. I risultati restituiti vengono inseriti in cache dei dati.

> [!NOTE]
> Se il testo dell'evento generato quando si seleziona viene visualizzato spesso, anche quando si prevede che ObjectDataSource a lavorare con i dati memorizzati nella cache, è possibile a causa dei vincoli di memoria. Se non vi è memoria sufficiente, i dati aggiunti alla cache da ObjectDataSource possono avere stato lo scavenging. Se la t ObjectDataSource sembra essere correttamente la memorizzazione nella cache i dati o solo le cache i dati sporadicamente, chiudere alcune applicazioni per liberare memoria e ripetere l'operazione.


Figura 12 illustra gli oggetti ObjectDataSource la memorizzazione nella cache del flusso di lavoro. Quando viene generato l'evento quando si seleziona il testo visualizzato sullo schermo, è perché i dati non è stato nella cache e dovevano essere recuperata dall'oggetto sottostante. Quando questo testo è presente, tuttavia, è s perché i dati erano disponibili dalla cache. Quando i dati vengono restituiti dalla cache lì s alcuna chiamata all'oggetto sottostante e, di conseguenza, nessuna query di database eseguiti.


![Gli archivi di ObjectDataSource e recupera i dati dalla Cache dei dati](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Figura 12**: ObjectDataSource archivia e recupera i relativi dati dalla Cache dei dati


Ogni applicazione ASP.NET ha la propria cache dei dati dell'istanza che s condivisi tra tutte le pagine e i visitatori. Ciò significa che i dati memorizzati nella cache dei dati da ObjectDataSource sono condiviso in modo analogo a tutti gli utenti che visitano la pagina. Per verificarlo, aprire il `ObjectDataSource.aspx` pagina in un browser. Prima di tutto gli utenti in visita la pagina, verrà visualizzato il testo dell'evento generato quando si seleziona (presupponendo che i dati aggiunti nella cache per i test precedenti, a questo punto, eliminati). Aprire una seconda istanza del browser e copiare e incollare l'URL della prima istanza del browser al secondo. Nella seconda istanza del browser, il testo dell'evento generato quando si seleziona non viene visualizzato perché si usano lo stesso memorizzato nella cache i dati del primo.

Quando si inseriscono i dati recuperati nella cache, ObjectDataSource Usa un valore di chiave di cache che include: il `CacheDuration` e `CacheExpirationPolicy` i valori delle proprietà; il tipo dell'oggetto business sottostante utilizzato da ObjectDataSource, che viene specificato tramite il [ `TypeName` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, in questo esempio); il valore della `SelectMethod` proprietà e il nome e i valori dei parametri nel `SelectParameters` insieme; e i valori delle relative `StartRowIndex`e `MaximumRows` delle proprietà, che vengono usate quando si implementa [paging personalizzato.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

Relativi alla creazione il valore della chiave della cache come una combinazione di queste proprietà assicura una voce della cache univoco come modificano tali valori. Ad esempio, nelle esercitazioni precedenti abbiamo va preso in esame usando le `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)`, che restituisce tutti i prodotti per una categoria specificata. Un utente potrebbe derivare per beverages pagina e visualizzazione, che ha un `CategoryID` pari a 1. Se ObjectDataSource memorizzato nella cache i risultati senza tener conto per il `SelectParameters` valori, quando proviene da un altro utente alla pagina per visualizzare i condimenti durante i prodotti di bevande sono stati nella cache, d visualizzano la prodotti bevande memorizzato nella cache anziché condiments. Modificando la chiave di cache base a queste proprietà, che includono i valori del `SelectParameters`, ObjectDataSource mantiene una voce della cache separato per beverages e condiments.

## <a name="stale-data-concerns"></a>Problemi di dati non aggiornati

ObjectDataSource rimuove automaticamente i relativi elementi dalla cache quando uno dei relativi `Insert`, `Update`, o `Delete` metodi viene richiamato. Ciò consente di proteggersi da dati non aggiornati cancellando le voci della cache quando vengono modificati i dati tramite la pagina. Tuttavia, è possibile che ObjectDataSource usando la memorizzazione nella cache comunque visualizzare i dati non aggiornati. Nel caso più semplice, è possibile perché i dati modificando direttamente all'interno del database. Un amministratore del database probabilmente è stato appena eseguito uno script che modifichi alcuni record nel database.

Questo scenario è stato possibile anche espandere in modo più complesso. Sebbene ObjectDataSource rimuove gli elementi dalla cache quando viene chiamato uno dei relativi metodi di modifica dei dati, gli elementi memorizzati nella cache rimossi sono per la combinazione di particolare s ObjectDataSource dei valori delle proprietà (`CacheDuration`, `TypeName`, `SelectMethod`, e così via). Se si dispone di due ObjectDataSource che usano diverse `SelectMethods` o `SelectParameters`, ma comunque possibile aggiornare gli stessi dati, quindi un ObjectDataSource può aggiornare una riga e invalidare la propria voci della cache, ma la riga corrispondente per il secondo oggetto ObjectDataSource verranno comunque serviti dal contenuto della cache. Suggeriamo di creare pagine per presentare questa funzionalità. Creare una pagina che visualizza un GridView modificabile che esegue il pull dei dati da ObjectDataSource che usa la memorizzazione nella cache e viene configurato per ottenere i dati di `ProductsBLL` classe s `GetProducts()` (metodo). Aggiungere un altro controllo GridView e ObjectDataSource per questa pagina (o un altro), ma per ObjectDataSource secondo modificabile modo che utilizzi il `GetProductsByCategoryID(categoryID)` (metodo). Poiché i due ObjectDataSource `SelectMethod` sono diverse proprietà, essi ll ogni hanno i propri valori memorizzati nella cache. Se si modifica un prodotto in una griglia, al successivo che è associare i dati a altra griglia (di paging, ordinamento e così via), verrà comunque servire i dati precedenti, memorizzato nella cache e non riflettere le modifiche apportate da altri griglia.

In breve, usare solo oggetti scaduti nella basati sul tempo se si è disposti a disporre le potenzialità dei dati non aggiornati e usare oggetti scaduti nella più breve per scenari in cui la validità dei dati è importante. Se i dati non aggiornati non sono accettabili, rinunciano alla memorizzazione nella cache o usare le dipendenze della cache SQL (presupponendo che si tratta di dati di database si ri la memorizzazione nella cache). Esploreremo dipendenze della cache SQL in un'esercitazione futura.

## <a name="summary"></a>Riepilogo

In questa esercitazione abbiamo esaminato le funzionalità di memorizzazione nella cache incorporate s di ObjectDataSource. Semplicemente impostando alcune proprietà, è possibile indicare a ObjectDataSource per memorizzare nella cache i risultati restituiti dall'oggetto specificato `SelectMethod` nella cache di dati ASP.NET. Il `CacheDuration` e `CacheExpirationPolicy` proprietà indicano la durata viene memorizzato nella cache l'elemento e se si tratta di un oggetto assoluto o la scadenza variabile. Il `CacheKeyDependency` proprietà associa tutte le voci di cache s ObjectDataSource con una dipendenza della cache esistente. Ciò consente di rimuovere le voci di s ObjectDataSource dalla cache prima della scadenza basati sul tempo viene raggiunto, in genere viene usata con le dipendenze della cache SQL.

Poiché ObjectDataSource semplicemente memorizza nella cache i relativi valori alla cache dei dati, è stato possibile replicare la funzionalità incorporata di ObjectDataSource s a livello di codice. T ha senso eseguire questa operazione a livello di presentazione, poiché ObjectDataSource offre questa funzionalità predefinite, ma è possibile implementare funzionalità di memorizzazione nella cache in un altro livello di architettura. A tale scopo, è necessario ripetere la stessa logica impiegata da ObjectDataSource. Verrà illustrato come a livello di codice con la cache dei dati all'interno dell'architettura nell'esercitazione successiva.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [La memorizzazione nella cache di ASP.NET: Tecniche e procedure consigliate](https://msdn.microsoft.com/library/aa478965.aspx)
- [Guida all'architettura di memorizzazione nella cache per applicazioni .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [La cache di output in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](caching-data-in-the-architecture-cs.md)
