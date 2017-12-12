---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: La memorizzazione nella cache di dati con ObjectDataSource (c#) | Documenti Microsoft
author: rick-anderson
description: "La memorizzazione nella cache può indicare la differenza tra una lente e un rapido dell'applicazione Web. In questa esercitazione è la prima delle quattro che accettano un'analisi approfondita la memorizzazione nella cache in ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ce0bd1d3302ee68c9c65584686172a07143e4a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-with-the-objectdatasource-c"></a>La memorizzazione nella cache di dati con ObjectDataSource (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) o [Scarica il PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> La memorizzazione nella cache può indicare la differenza tra una lente e un rapido dell'applicazione Web. In questa esercitazione è la prima delle quattro che accettano un'analisi approfondita la memorizzazione nella cache in ASP.NET. Illustra i concetti principali di memorizzazione nella cache e come applicare la memorizzazione nella cache a livello di presentazione tramite il controllo ObjectDataSource.


## <a name="introduction"></a>Introduzione

In ambito informatico, *la memorizzazione nella cache* è il processo di recupero di dati o informazioni che è dispendiosa ottenere e archiviare una copia in una posizione che è più veloce per l'accesso. Per le applicazioni guidate dai dati, le query di grandi e complesse utilizzano in genere la maggior parte del tempo di esecuzione dell'applicazione s. Questo tipo una s le prestazioni dell'applicazione, quindi, possono essere migliorate spesso archiviando i risultati delle query di database costosi nella memoria s dell'applicazione.

ASP.NET 2.0 offre un'ampia gamma di opzioni di memorizzazione nella cache. Un'intera pagina web o markup sottoposto a rendering s controllo utente possono essere memorizzati nella cache tramite *la memorizzazione nella cache di output*. I controlli ObjectDataSource e SqlDataSource forniscono funzionalità di memorizzazione nonché, consentendo di dati da memorizzare nella cache a livello di controllo. E ASP.NET s *cache dei dati* fornisce un'API di memorizzazione nella cache completa che consente agli sviluppatori di pagina a livello di programmazione gli oggetti della cache. In questa esercitazione e i successivi tre che verranno presi in esame usando le s ObjectDataSource la memorizzazione nella cache funzionalità, nonché la cache dei dati. Inoltre, verrà illustrato come memorizzare nella cache di dati a livello di applicazione all'avvio e come mantenere i dati memorizzati nella cache aggiornata tramite l'utilizzo di dipendenze della cache SQL. Queste esercitazioni non esplorare la memorizzazione nella cache di output. Per un'analisi approfondita la memorizzazione nella cache di output, vedere [la memorizzazione nella cache di Output di ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

La memorizzazione nella cache possono essere applicate in qualsiasi punto dell'architettura, dal livello di accesso ai dati di tramite il livello di presentazione. In questa esercitazione verrà esaminato l'applicazione di memorizzazione nella cache a livello di presentazione tramite il controllo ObjectDataSource. Nella prossima esercitazione che verrà esaminato la memorizzazione nella cache dati al livello di logica di Business.

## <a name="key-caching-concepts"></a>La memorizzazione nella cache i concetti chiave

La memorizzazione nella cache può migliorare notevolmente una s applicazione complessivo delle prestazioni e scalabilità considerando i dati che è dispendioso generare e archiviare una copia in una posizione a cui è possibile accedere in modo più efficiente. Poiché la cache contiene solo una copia dei dati effettivi, sottostante, possono diventare obsoleto, o *obsoleti*, se i dati sottostanti vengono modificati. Per ovviare al problema, uno sviluppatore di pagina può indicare i criteri con cui l'elemento della cache verrà *eliminate* dalla cache, utilizzando:

- **Criteri basati sul tempo** può essere aggiunto un elemento nella cache per un assoluta o variabile di durata. Ad esempio, uno sviluppatore di pagina può indicare una durata, ad esempio, 60 secondi. Con una durata assoluta, l'elemento memorizzato nella cache viene eliminato dopo che è stato aggiunto alla cache, indipendentemente dalla frequenza di accesso di 60 secondi. Con una durata di scorrimento, l'elemento memorizzato nella cache viene eliminato 60 secondi dopo l'ultimo accesso.
- **Criteri basati su dipendenza** una dipendenza può essere associata a un elemento quando aggiunto alla cache. Quando viene modificata la dipendenza elemento s viene eliminato dalla cache. La dipendenza può essere un file, un altro elemento della cache o una combinazione dei due. ASP.NET 2.0 consente inoltre le dipendenze della cache SQL, che consentono agli sviluppatori di aggiungere un elemento alla cache e viene eliminato quando cambiano i dati di database sottostanti. Verranno presi in esame le dipendenze della cache SQL nella [con dipendenze della Cache SQL](using-sql-cache-dependencies-cs.md) esercitazione.

Indipendentemente dai criteri di eliminazione specificati, potrebbe essere un elemento nella cache *scavenging* prima i criteri basati sul tempo o basato su dipendenza è stata soddisfatta. Se la cache ha raggiunto la capacità, è necessario rimuovere gli elementi esistenti prima di possono aggiungere quelli nuovi. Di conseguenza, quando a livello di codice l'uso di dati memorizzati nella cache s fondamentali che è sempre presupporre che i dati memorizzati nella cache non possono essere presenti. Verrà esaminato il criterio da utilizzare quando si accede ai dati dalla cache a livello di codice nell'esercitazione successiva, *memorizzazione dei dati nell'architettura*.

La memorizzazione nella cache rappresenta un modo economico per stesso come concentrare prestazioni più elevate da un'applicazione. Come [Steven Smith](http://aspadvice.com/blogs/ssmith/) articulates nell'articolo [la memorizzazione nella cache di ASP.NET: tecniche e procedure consigliate](https://msdn.microsoft.com/en-us/library/aa478965.aspx):

La memorizzazione nella cache può essere un buon metodo per ottenere buone sufficiente prestazioni senza richiedere molto tempo e l'analisi. Memoria è basso, pertanto se è possibile ottenere le prestazioni, è necessario per la memorizzazione nella cache l'output per 30 secondi anziché un giorno o settimana tentare di ottimizzare un database o il codice di spesa, eseguire la soluzione di memorizzazione nella cache (presupponendo che 30 - precedenti secondo dati è accettabile) e passare. Infine, progettazione verrà probabilmente aggiornarsi, in modo naturalmente è necessario progettare correttamente le applicazioni. Ma se è sufficiente ottenere buone sufficiente prestazioni oggi, la memorizzazione nella cache può essere un eccellente [approccio], acquistare il tempo necessario per effettuare il refactoring dell'applicazione in un secondo momento quando si dispone il tempo necessario per eseguire questa operazione.

Durante la memorizzazione nella cache può offrire miglioramenti significativi delle prestazioni, non è applicabile in tutte le situazioni, ad esempio con le applicazioni che utilizzano dati in tempo reale, l'aggiornamento di frequente, o in cui i dati non aggiornati anche a breve durata sono accettabili. Ma per la maggior parte delle applicazioni, deve essere usata la memorizzazione nella cache. Per ulteriori informazioni sulla memorizzazione nella cache di ASP.NET 2.0, consultare il [la memorizzazione nella cache per le prestazioni](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) sezione il [esercitazioni delle Guide rapide di ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Passaggio 1: Creazione di pagine Web di memorizzazione nella cache

Prima di iniziare l'esplorazione delle funzionalità di caching s ObjectDataSource, consentire s attentamente prima di creare le pagine ASP.NET nel progetto sito Web che è necessario per questa esercitazione e i successivi tre. Per iniziare, aggiungere una nuova cartella denominata `Caching`. Successivamente, aggiungere le seguenti pagine ASP.NET a quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative alla cache](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni relative alla cache


Come in altre cartelle, `Default.aspx` nel `Caching` cartella elencherà le esercitazioni nella relativa sezione. Tenere presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere il controllo utente `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Figura 2: Aggiungere il controllo utente SectionLevelTutorialListing.ascx a Default.aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Figura 2**: figura 2: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente in `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](caching-data-with-the-objectdatasource-cs/_static/image4.png))


Infine, aggiungere tali pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo l'utilizzo di dati binari `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, dedicare alcuni minuti per visualizzare il sito Web esercitazioni tramite un browser. Il menu a sinistra ora include gli elementi per le esercitazioni di memorizzazione nella cache.


![Mappa del sito include ora le voci per le esercitazioni di memorizzazione nella cache](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Figura 3**: mappa del sito include ora le voci per le esercitazioni di memorizzazione nella cache


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Passaggio 2: Visualizzare un elenco di prodotti in una pagina Web

In questa esercitazione viene illustrato come utilizzare ObjectDataSource controllo s incorporate funzionalità di caching. Prima di poter esaminare queste funzionalità, tuttavia, è innanzitutto necessario una pagina di operare da. S consentono di creare una pagina web che utilizza un controllo GridView per elencare le informazioni prodotto recuperate da ObjectDataSource dalla `ProductsBLL` classe.

Aprire il `ObjectDataSource.aspx` nella pagina di `Caching` cartella. Trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, impostare il relativo `ID` proprietà `Products`e dal suo smart tag scegliere da associare a un nuovo controllo ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per funzionare con la `ProductsBLL` classe.


[![Configurare ObjectDataSource per utilizzare la classe ProductsBLL](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Figura 4**: configurare ObjectDataSource per utilizzare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine ingrandita](caching-data-with-the-objectdatasource-cs/_static/image8.png))


Per questa pagina consente s di creare un GridView modificabili in modo che è possibile esaminare cosa accade quando vengono modificati i dati memorizzati nella cache in ObjectDataSource tramite l'interfaccia s GridView. Lasciare l'elenco a discesa nella scheda Seleziona impostata sul valore predefinito, `GetProducts()`, ma l'elemento selezionato nella scheda UPDATE per modificare il `UpdateProduct` overload che accetta `productName`, `unitPrice`, e `productID` come parametri di input.


[![Impostazione dell'elenco di riepilogo a discesa di aggiornamento scheda s all'Overload appropriato UpdateProduct](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Figura 5**: impostare gli oggetti di aggiornamento scheda elenco a discesa l'appropriato `UpdateProduct` Overload ([fare clic per visualizzare l'immagine ingrandita](caching-data-with-the-objectdatasource-cs/_static/image11.png))


Infine, impostare gli elenchi a discesa nelle schede delle INSERT e DELETE su (nessuno) e fare clic su Fine. Dopo avere completato la configurazione guidata origine dati, Visual Studio imposta s ObjectDataSource `OldValuesParameterFormatString` proprietà `original_{0}`. Come descritto nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esercitazione, questa proprietà deve essere rimosso dalla sintassi dichiarativa o reimpostato sul valore predefinito, `{0}`, affinché l'aggiornamento del flusso di lavoro continuare senza errori.

Inoltre, al termine della procedura guidata Visual Studio aggiunge un campo a GridView per ognuno dei campi dati di prodotto. Rimuovere tutto tranne il `ProductName`, `CategoryName`, e `UnitPrice` BoundField. Aggiornare quindi il `HeaderText` proprietà di ciascuno di questi BoundField per prodotto, categoria e prezzo, rispettivamente. Poiché il `ProductName` campo è obbligatorio, convertire il BoundField in un TemplateField e aggiungere un controllo RequiredFieldValidator per il `EditItemTemplate`. Analogamente, convertire il `UnitPrice` BoundField in un TemplateField e aggiungere un controllo CompareValidator per assicurarsi che il valore immesso dall'utente è a valore di valuta valida che s maggiore o uguale a zero. Oltre a queste modifiche, è possibile eseguire qualsiasi modifica estetici, come l'allineamento a destra il `UnitPrice` valore o la specifica la formattazione per il `UnitPrice` testo nelle proprie interfacce di sola lettura e modificare.

Rendere modificabile il GridView selezionando la casella di controllo Abilita modifica nello smart tag GridView s. Controllare inoltre le caselle di controllo Abilita Paging e abilitare l'ordinamento.

> [!NOTE]
> È necessario una revisione di come personalizzare l'interfaccia di modifica di GridView s? In questo caso, si riferiscono nuovamente al [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) esercitazione.


[![Abilitare il supporto di GridView per la modifica, l'ordinamento e Paging](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Figura 6**: abilitare il supporto di GridView per la modifica, ordinamento e Paging ([fare clic per visualizzare l'immagine ingrandita](caching-data-with-the-objectdatasource-cs/_static/image14.png))


Dopo aver apportato queste modifiche GridView, GridView e ObjectDataSource s dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Come illustrato nella figura 7, GridView modificabile Elenca il nome, categoria e il prezzo di ciascuno dei prodotti nel database. È opportuno testare i risultati, spostarsi tra loro, l'ordinamento di funzionalità della pagina s e modificare un record.


[![Ogni nome di prodotto, categoria e prezzo è elencato un Sortable, Pageable, modificabile GridView](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Figura 7**: ogni prodotto s, nome, categoria e prezzo è elencato un GridView modificabile Sortable, Pageable ([fare clic per visualizzare l'immagine ingrandita](caching-data-with-the-objectdatasource-cs/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Passaggio 3: Esame ObjectDataSource di quando è richiesta di dati

Il `Products` GridView recupera i dati da visualizzare richiamando il `Select` metodo il `ProductsDataSource` ObjectDataSource. ObjectDataSource crea un'istanza di s Business Logic Layer `ProductsBLL` classe e viene chiamato il relativo `GetProducts()` metodo, che a sua volta chiama s il livello di accesso ai dati `ProductsTableAdapter` s `GetProducts()` metodo. Il metodo DAL si connette al database Northwind e rilascia l'oggetto configurato `SELECT` query. Questi dati viene quindi restituiti al DAL pacchetto backup in un `NorthwindDataTable`. L'oggetto DataTable viene restituito al livello Business Logic, che restituisce al ObjectDataSource, che restituisce il controllo GridView. GridView crea quindi un `GridViewRow` per ciascun `DataRow` nel DataTable e ogni `GridViewRow` alla fine viene eseguito il rendering in formato HTML di cui viene restituito al client e visualizzato nel browser del visitatore s.

Questa sequenza di eventi si verifica ogni volta che il controllo GridView deve essere associato ai relativi dati sottostanti. Ciò si verifica quando viene innanzitutto visitata, quando si sposta da una pagina di dati a un altro, quando si ordinano GridView, o quando si modificano i dati di GridView s tramite relativo predefinito, modificare o eliminare le interfacce. Se lo stato di visualizzazione s GridView viene disabilitato, GridView verranno riassociati a ogni postback anche. GridView può anche essere esplicitamente riassociati ai relativi dati chiamando il relativo `DataBind()` metodo.

Per apprezzare pienamente la frequenza con cui i dati vengono recuperati dal database, lasciare s viene visualizzato un messaggio che indica quando vengono nuovamente recuperati i dati. Aggiungere un controllo etichetta Web sopra GridView denominato `ODSEvents`. Cancellare il contenuto relativo `Text` proprietà e impostare il relativo `EnableViewState` proprietà `false`. Sotto l'etichetta, aggiungere un controllo pulsante Web e impostare il relativo `Text` proprietà Postback.


[![Aggiungere un'etichetta e un pulsante alla pagina sopra il controllo GridView.](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Figura 8**: aggiungere un'etichetta e un pulsante di pagina sopra il controllo GridView ([fare clic per visualizzare l'immagine ingrandita](caching-data-with-the-objectdatasource-cs/_static/image20.png))


Durante il flusso di lavoro di accesso ai dati s ObjectDataSource `Selecting` viene generato l'evento prima che venga creato l'oggetto sottostante e il relativo metodo configurato richiamato. Creare un gestore eventi per questo evento e aggiungere il codice seguente:


[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Ogni volta che ObjectDataSource effettua una richiesta per l'architettura per i dati, l'etichetta verrà visualizzato l'evento di selezione di testo generato.

Visitare questa pagina in un browser. Quando la pagina viene prima visitata, viene visualizzato l'evento di selezione di testo generato. Fare clic sul pulsante Postback e si noti che il testo scomparirà (supponendo che i dispositivi di GridView `EnableViewState` è impostata su `true`, il valore predefinito). In questo modo, durante il postback, GridView viene ricostruito dal relativo stato di visualizzazione e pertanto t attivare a ObjectDataSource per i propri dati. Ordinamento, spostamento o la modifica dei dati, tuttavia, comporta GridView riassociazione all'origine dati e pertanto l'evento Selecting generato viene visualizzato nuovamente il testo.


[![Ogni volta che il controllo GridView è riassociato all'origine dati, viene visualizzato l'evento Selecting generato](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Figura 9**: ogni volta che GridView in cui viene è riassociati all'origine dati, viene visualizzato l'evento Selecting generato ([fare clic per visualizzare l'immagine ingrandita](caching-data-with-the-objectdatasource-cs/_static/image23.png))


[![Fare clic sul pulsante Postback fa sì che GridView per essere ricostruiti dallo stato di visualizzazione](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Figura 10**: fare clic sul pulsante Postback provoca GridView essere ricostruiti dallo stato di visualizzazione ([fare clic per visualizzare l'immagine ingrandita](caching-data-with-the-objectdatasource-cs/_static/image26.png))


Può sembrare inutili recuperare i dati di database ogni volta che i dati del pool di paging tramite o ordinati. Dopo tutto, poiché è re l'utilizzo del paging predefinito, ObjectDataSource ha recuperato tutti i record per la prima pagina di visualizzazione. Anche se GridView non fornisce l'ordinamento e paging supporto, i dati devono recuperati dal database ogni volta che viene innanzitutto visitata da qualsiasi utente (e in ogni postback, se lo stato di visualizzazione è disabilitato). Ma se il controllo GridView verrà visualizzati gli stessi dati per tutti gli utenti, queste richieste di database aggiuntive sono superflue. Perché non memorizzare nella cache i risultati restituiti dal `GetProducts()` (metodo) e bind GridView a quelli memorizzati nella cache dei risultati?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Passaggio 4: La memorizzazione nella cache i dati utilizzando ObjectDataSource

Semplicemente impostando alcune proprietà ObjectDataSource può essere configurato per automaticamente nella cache i dati recuperati nella cache dei dati ASP.NET. Nell'elenco seguente vengono riepilogate le proprietà correlate alla cache di ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) deve essere impostato su `true` per abilitare la memorizzazione nella cache. Il valore predefinito è `false`.
- [CacheDuration](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) la quantità di tempo, espresso in secondi, che viene memorizzato nella cache i dati. Il valore predefinito è 0. ObjectDataSource solo memorizza nella cache dati se `EnableCaching` è `true` e `CacheDuration` è impostata su un valore maggiore di zero.
- [CacheExpirationPolicy](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) può essere impostato su `Absolute` o `Sliding`. Se `Absolute`, ObjectDataSource memorizza nella cache i dati recuperati per `CacheDuration` secondi; se `Sliding`, i dati scadono solo dopo che è stato eseguito l'accesso per `CacheDuration` secondi. Il valore predefinito è `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) utilizzare questa proprietà per associare le voci di cache s ObjectDataSource una dipendenza della cache esistente. Le voci di dati ObjectDataSource s possono essere rimossa in modo anomalo dalla cache impostando come scaduti associato `CacheKeyDependency`. Questa proprietà viene in genere utilizzata per associare una dipendenza della cache SQL con la cache s ObjectDataSource, un argomento si esamineranno in futuro [con dipendenze della Cache SQL](using-sql-cache-dependencies-cs.md) esercitazione.

Configurare automaticamente s il `ProductsDataSource` ObjectDataSource per memorizzare nella cache i dati per 30 secondi in una scala assoluta. Impostare il s ObjectDataSource `EnableCaching` proprietà `true` e il relativo `CacheDuration` proprietà a 30. Lasciare il `CacheExpirationPolicy` impostata sul valore predefinito, `Absolute`.


[![Configurare ObjectDataSource per memorizzare nella Cache i dati per 30 secondi](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Figura 11**: configurare ObjectDataSource per memorizzare nella Cache i dati per 30 secondi ([fare clic per visualizzare l'immagine ingrandita](caching-data-with-the-objectdatasource-cs/_static/image29.png))


Salvare le modifiche e rivedere questa pagina in un browser. Il testo dell'evento viene generato se si seleziona verrà visualizzato quando si visita prima pagina, inizialmente i dati non è nella cache. Ma non di postback successivi attivato facendo clic sul pulsante Postback, ordinamento, paging o fare clic sul pulsante di modifica o annullamento *non* nuovamente l'evento Selecting generato testo. In questo modo il `Selecting` evento viene generato solo quando ObjectDataSource Ottiene i dati dal relativo oggetto sottostante; il `Selecting` evento non viene generato se viene effettuato il pull dei dati dalla cache dei dati.

Dopo 30 secondi, i dati verranno rimossi dalla cache. Anche i dati verranno rimossi dalla cache se s ObjectDataSource `Insert`, `Update`, o `Delete` metodi vengono richiamati. Di conseguenza, dopo 30 secondi trascorsi o o di aggiornamento è stato fatto clic sul pulsante, ordinamento, spostamento, fare clic sul pulsante Modifica o l'annullamento causerà ObjectDataSource ottenere i dati dal relativo oggetto sottostante, visualizzare l'evento Selecting generato testo quando il `Selecting` viene generato l'evento. I risultati restituiti vengono reinseriti nella cache dei dati.

> [!NOTE]
> Se viene visualizzato il testo dell'evento viene generato se si seleziona spesso, anche quando si prevede di ObjectDataSource di lavorare con i dati memorizzati nella cache, è possibile a causa dei vincoli di memoria. Se non vi è memoria sufficiente, che i dati aggiunti alla cache da ObjectDataSource sono stati eliminati. Se t ObjectDataSource sembra essere correttamente la memorizzazione nella cache i dati o la cache soli i dati in alcuni casi, chiudere alcune applicazioni per liberare memoria e riprovare.


Figura 12 illustra s ObjectDataSource la memorizzazione nella cache del flusso di lavoro. Quando viene generato l'evento Selecting testo visualizzato sullo schermo, perché i dati non è nella cache e devono essere recuperato dall'oggetto sottostante. Quando questo testo è presente, tuttavia, è s perché i dati sono disponibili dalla cache. Quando i dati vengono restituiti dalla cache esiste s alcuna chiamata all'oggetto sottostante e, pertanto, nessuna query di database eseguiti.


![Archivi ObjectDataSource e recupera i dati dalla Cache dei dati](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Figura 12**: ObjectDataSource archivia e recupera i dati dalla Cache dei dati


Ogni applicazione ASP.NET ha la propria cache di dati di istanza che s condiviso tra tutte le pagine e i visitatori. Ciò significa che i dati memorizzati nella cache dei dati da ObjectDataSource in modo analogo sono condivisa tra tutti gli utenti, visitare la pagina. Per verificarlo, aprire il `ObjectDataSource.aspx` pagina in un browser. Durante la prima visita la pagina, verrà visualizzato il testo dell'evento viene generato se si seleziona (supponendo che i dati aggiunti alla cache per i test precedenti, a questo punto, eliminati). Aprire una seconda istanza del browser e copiare e incollare l'URL dalla prima istanza del browser al secondo. Nella seconda istanza del browser, il testo dell'evento viene generato se si seleziona non viene visualizzato perché il dati del primo memorizzati nella cache utilizzando lo stesso.

Quando si inserisce i dati recuperati nella cache, ObjectDataSource utilizza un valore di chiave di cache che include: il `CacheDuration` e `CacheExpirationPolicy` i valori delle proprietà; il tipo dell'oggetto business sottostante utilizzato da ObjectDataSource, specificata tramite il [ `TypeName` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, in questo esempio); il valore della `SelectMethod` proprietà e il nome e i valori dei parametri in di `SelectParameters` insieme; e i valori delle relative `StartRowIndex`e `MaximumRows` proprietà, vengono utilizzate quando si implementa [il paging personalizzato.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

Crea il valore della chiave della cache come una combinazione di queste proprietà garantisce una voce della cache univoco come questi valori vengono modificati. Ad esempio, nelle esercitazioni precedenti abbiamo ve descritto l'utilizzo di `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)`, che restituisce tutti i prodotti per una categoria specificata. Potrebbe essere un utente bevande pagina e visualizzazione, che presenta un `CategoryID` pari a 1. Se ObjectDataSource memorizzato nella cache i risultati senza considerare il `SelectParameters` valori, quando un altro utente alla pagina per visualizzare i condimenti durante i prodotti di bibite nella cache, d vedono prodotti bevande memorizzati nella cache anziché condiments. Variando la chiave di cache mediante queste proprietà, che includono i valori del `SelectParameters`, ObjectDataSource mantiene una voce della cache separato per beverages e condimenti.

## <a name="stale-data-concerns"></a>Problemi di dati non aggiornati

ObjectDataSource rimuove automaticamente gli elementi dalla cache quando uno qualsiasi dei relativi `Insert`, `Update`, o `Delete` metodi viene richiamato. Ciò facilita la protezione dati non aggiornati cancellando le voci della cache quando i dati vengono modificati tramite la pagina. Tuttavia, è possibile per ObjectDataSource utilizzando la memorizzazione nella cache per visualizzare dati non aggiornati. Nel caso più semplice, è possibile a causa di dati modificando direttamente all'interno del database. Ad esempio un amministratore del database è stata eseguita uno script che vengono modificati alcuni record nel database.

Questo scenario potrebbe anche espandere in modo più complesso. Mentre ObjectDataSource rimuove gli elementi dalla cache quando viene chiamato uno dei relativi metodi di modifica di dati, gli elementi memorizzati nella cache rimossi sono per la combinazione di particolare s ObjectDataSource dei valori delle proprietà (`CacheDuration`, `TypeName`, `SelectMethod`, e così via). Se si dispone di due ObjectDataSources che utilizzano diversi `SelectMethods` o `SelectParameters`, ma è comunque possibile aggiornare gli stessi dati, quindi uno ObjectDataSource può aggiornare una riga e invalidare i proprio voci della cache, ma la riga corrispondente per il secondo ObjectDataSource verrà utilizzato dal contenuto della cache. Consiglia di creare pagine per presentare questa funzionalità. Creare una pagina che visualizza un GridView modificabile che estrae i dati da ObjectDataSource che utilizza la memorizzazione nella cache ed è configurato per ottenere i dati di `ProductsBLL` classe s `GetProducts()` metodo. Aggiungere un altro GridView modificabili e ObjectDataSource a questa pagina (o un altro), ma per ObjectDataSource secondo è utilizzare il `GetProductsByCategoryID(categoryID)` metodo. Poiché i due ObjectDataSources `SelectMethod` proprietà differiscono, essi ll ogni hanno i propri valori memorizzati nella cache. Se si modifica un prodotto in una griglia, la volta successiva che si associare i dati alla griglia altre (per il paging, ordinamento e così via), verrà ancora servire i vecchi dati memorizzati nella cache e non rifletta le modifiche apportate da altri griglia.

In breve, utilizzare solo basate sul tempo oggetti scaduti nella se si è disposti a è il potenziale di dati non aggiornati e utilizzare oggetti scaduti nella più breve per gli scenari in cui la validità dei dati è importante. Se i dati non aggiornati non sono accettabili, evitare la memorizzazione nella cache o utilizzare le dipendenze della cache SQL (presupponendo che si tratta di dati di database si stanno la memorizzazione nella cache). Si esamineranno le dipendenze della cache SQL in un'esercitazione future.

## <a name="summary"></a>Riepilogo

In questa esercitazione sono state esaminate le funzionalità di memorizzazione nella cache incorporate di ObjectDataSource s. Semplicemente impostando alcune proprietà, è possibile indicare ObjectDataSource per memorizzare nella cache i risultati restituiti dall'oggetto specificato `SelectMethod` nella cache di dati ASP.NET. Il `CacheDuration` e `CacheExpirationPolicy` indicano la durata viene memorizzato nella cache l'elemento e che sia assoluto o la scadenza variabile. Il `CacheKeyDependency` proprietà associa a tutte le voci di cache ObjectDataSource s con una dipendenza della cache esistente. Ciò consente di rimuovere le voci di s ObjectDataSource dalla cache prima della scadenza basati sul tempo viene raggiunto, viene in genere utilizzata con le dipendenze della cache SQL.

Poiché ObjectDataSource semplicemente memorizza nella cache di valori per la cache dei dati, è stato possibile replicare le funzionalità predefinite di ObjectDataSource s a livello di codice. T ha senso eseguire questa operazione a livello di presentazione, poiché ObjectDataSource offre una funzionalità predefinita, ma è possibile implementare funzionalità di memorizzazione nella cache in un livello separato dell'architettura. A tale scopo, sarà necessario ripetere la stessa logica utilizzata da ObjectDataSource. Verrà illustrato come utilizzare a livello di codice con la cache dei dati all'interno dell'architettura nell'esercitazione successiva.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [La memorizzazione nella cache di ASP.NET: Le tecniche e procedure consigliate](https://msdn.microsoft.com/en-us/library/aa478965.aspx)
- [Guida all'architettura di memorizzazione nella cache per le applicazioni .NET Framework](https://msdn.microsoft.com/en-us/library/ee817645.aspx)
- [La cache di output in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Successivo](caching-data-in-the-architecture-cs.md)
