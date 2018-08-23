---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Visualizzazione dei dati in un grafico con ASP.NET Web Pages (Razor) | Microsoft Docs
author: microsoft
description: In questo capitolo viene illustrato come visualizzare i dati in un grafico. Nei capitoli precedenti, è stato descritto come visualizzare i dati manualmente e in una griglia. In questo capitolo viene...
ms.author: riande
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 00529355476e88c47ab790121ae77202aa5e7b76
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832280"
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Visualizzazione dei dati in un grafico con ASP.NET Web Pages (Razor)
====================
da [Microsoft](https://github.com/microsoft)

> Questo articolo illustra come usare un grafico per visualizzare i dati in un sito Web ASP.NET Web Pages (Razor) utilizzando il `Chart` helper.
> 
> **Si apprenderà**:
> 
> - Come visualizzare i dati in un grafico.
> - Viene descritto come applicare uno stile a grafici di utilizzo dei temi predefiniti.
> - Come salvare i grafici e come memorizzarli nella cache per migliorare le prestazioni.
> 
> Queste sono le funzionalità introdotte nell'articolo di programmazione ASP.NET:
> 
> - Il `Chart` helper.
> 
> > [!NOTE]
> > Le informazioni contenute in questo articolo si applicano a 1.0 di pagine Web ASP.NET e Web Pages 2.


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>L'Helper grafico

Quando si desidera visualizzare i dati sotto forma di grafico, è possibile usare `Chart` helper. Il `Chart` helper può eseguire il rendering di un'immagine che visualizza i dati in un'ampia gamma di tipi di grafico. Supporta numerose opzioni per la formattazione e l'assegnazione di etichette. Il `Chart` helper può eseguire il rendering di più di 30 tipi di grafici, inclusi tutti i tipi di grafici che è possibile avere familiarità con da Microsoft Excel o altri strumenti di &#8212; grafici ad area, i grafici a barre, istogrammi, grafici a linee e grafici a torta, insieme a informazioni grafici speciali, ad esempio i grafici azionari.

| **Grafico ad aree** ![descrizione: immagine del tipo di grafico ad Area](7-displaying-data-in-a-chart/_static/image1.jpg) | **Grafico a barre** ![descrizione: immagine del tipo di grafico a barre](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Istogramma** ![descrizione: immagine del tipo di grafico](7-displaying-data-in-a-chart/_static/image3.jpg) | **Grafico a linee** ![descrizione: immagine del tipo di grafico a linee](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Grafico a torta** ![descrizione: immagine del tipo di grafico a torta](7-displaying-data-in-a-chart/_static/image5.jpg) | **Grafico azionario** ![descrizione: immagine del tipo di grafico azionario](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elementi del grafico

I grafici mostrano i dati e altri elementi quali legende, assi, serie e così via. La figura seguente vengono illustrati molti degli elementi del grafico che è possibile personalizzare quando si usa il `Chart` helper. Questo articolo illustra come impostare alcune (non tutti) di questi elementi.

![Descrizione: Immagine che mostra gli elementi del grafico](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Creazione di un grafico dai dati

I dati visualizzati in un grafico possono essere da una matrice, dai risultati restituiti da un database o da dati che si trova in un file XML.

### <a name="using-an-array"></a>Utilizzando una matrice

Come illustrato in [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890), una matrice consente di archiviare una raccolta di elementi simili in una singola variabile. È possibile utilizzare matrici per contenere i dati che si desidera includere nel grafico.

Questa procedura viene illustrato come creare un grafico dai dati nell'array, utilizzando il tipo di grafico predefinito. Viene inoltre illustrato come visualizzare il grafico all'interno della pagina.

1. Creare un nuovo file denominato *ChartArrayBasic.cshtml*.
2. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Prima di tutto, il codice crea un nuovo grafico e imposta la larghezza e altezza. Specificare il titolo del grafico utilizzando il `AddTitle` (metodo). Per aggiungere dati, si utilizza il `AddSeries` (metodo). In questo esempio, si utilizza il `name`, `xValue`, e `yValues` i parametri del `AddSeries` (metodo). Il `name` parametro viene visualizzato nella legenda del grafico. Il `xValue` parametro contiene una matrice di dati che viene visualizzati lungo l'asse orizzontale del grafico. Il `yValues` parametro contiene una matrice di dati utilizzato per tracciare i punti verticali del grafico.

    Il `Write` metodo esegue effettivamente il rendering del grafico. In questo caso, dato che non è stato specificato un tipo di grafico, il `Chart` helper esegue il rendering relativo grafico predefinita, in cui un istogramma.
3. Eseguire la pagina nel browser. Il browser viene visualizzato il grafico. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Usando una query sul Database per i dati del grafico

Se le informazioni di che cui tracciare il grafico si trova in un database, è possibile eseguire una query sul database e quindi usare i dati dai risultati per creare il grafico. Questa procedura viene illustrato come leggere e visualizzare i dati dal database creato in questo articolo [Introduzione all'uso di un Database nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Aggiungere un *App\_dati* cartella nella radice del sito Web se la cartella non esiste già.
2. Nel *App\_Data* cartella, aggiungere il file di database denominato *SmallBakery.sdf* che è descritti [Introduzione all'uso di un Database nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Creare un nuovo file denominato *ChartDataQuery.cshtml*.
4. Sostituire il contenuto esistente con il codice seguente:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Il codice viene aperto il database SmallBakery prima di tutto e lo assegna a una variabile denominata `db`. Questa variabile rappresenta un `Database` oggetto che può essere utilizzato per leggere e scrivere nel database. Successivamente, il codice esegue una query SQL per ottenere il nome e il prezzo di ogni prodotto. Il codice crea un nuovo grafico e passa la query di database ad esso tramite la chiamata del grafico `DataBindTable` (metodo). Questo metodo accetta due parametri: la `dataSource` parametro è per i dati dalla query e il `xField` parametro consente di impostare la colonna di dati utilizzata per l'asse x del grafico.

    Come alternativa all'uso di `DataBindTable` metodo, è possibile usare il `AddSeries` metodo del `Chart` helper. Il `AddSeries` metodo consente di impostare il `xValue` e `yValues` parametri. Ad esempio, invece di usare il `DataBindTable` metodo simile al seguente:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    È possibile usare il `AddSeries` metodo simile al seguente:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Entrambi eseguono il rendering gli stessi risultati. Il `AddSeries` metodo è più flessibile perché è possibile specificare il tipo di grafico e i dati in modo più esplicito, ma il `DataBindTable` metodo è più facile da usare se non è necessaria la flessibilità aggiuntiva.
5. Eseguire la pagina in un browser. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Utilizzo di dati XML

La terza opzione per la creazione di grafici consiste nell'utilizzare un file XML dei dati per il grafico. Ciò richiede che il file XML dispone anche di un file di schema (*XSD* file) che descrive la struttura XML. Questa procedura viene illustrato come leggere i dati da un file XML.

1. Nel *App\_Data* cartella, creare un nuovo file XML denominato *Data. XML*.
2. Sostituire il codice XML esistente con il codice seguente, ovvero alcuni dati XML relativi ai dipendenti di una società fittizia. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. Nel *App\_Data* cartella, creare un nuovo file XML denominato *XSD*. (Si noti che l'estensione in questo caso si *XSD*.)
4. Sostituire il codice XML esistente con il codice seguente: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Nella radice del sito Web, creare un nuovo file denominato *ChartDataXML.cshtml*.
6. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Il codice crea prima un `DataSet` oggetto. Questo oggetto viene usato per gestire i dati che viene letto dal file XML e organizzandoli però in base alle informazioni nel file di schema. (Si noti che nella parte superiore del codice inclusa l'istruzione `using SystemData`. Questa operazione è necessaria per poter lavorare con i `DataSet` oggetto. Per altre informazioni, vedere [ &quot;Using&quot; istruzioni e nomi completi](#SB_UsingStatements) più avanti in questo articolo.)

    Successivamente, il codice crea un `DataView` oggetto in base al set di dati. La visualizzazione di dati fornisce un oggetto che il grafico è possibile associare a &#8212; , leggere e creare un tracciato. Il grafico viene associato a dati mediante la `AddSeries` metodo, come illustrato in precedenza durante la creazione di grafici di dati della matrice, con la differenza che questa volta il `xValue` e `yValues` parametri vengono impostati i `DataView` oggetto.

    In questo esempio illustra inoltre come specificare un particolare tipo di grafico. Quando i dati vengono aggiunti nel `AddSeries` metodo, il `chartType` parametro viene impostato anche per visualizzare un grafico a torta.
7. Eseguire la pagina in un browser. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Istruzioni "Using" e nomi completi
> 
> .NET Framework basata su ASP.NET Web Pages con sintassi Razor è costituito da molte migliaia di componenti (classi). Per rendere più gestibile a funzionare con tutte queste classi, sono organizzati nelle *spazi dei nomi*, che sono pressoché simile alle raccolte. Ad esempio, il `System.Web` dello spazio dei nomi contiene classi che supportano la comunicazione browser/server, il `System.Xml` dello spazio dei nomi contiene classi che consentono di creare e leggere i file XML, e il `System.Data` dello spazio dei nomi contiene classi che consentono di lavorare con i dati.
> 
> Per poter accedere a qualsiasi classe fornita in .NET Framework, deve conoscere non solo il nome della classe, ma anche lo spazio dei nomi della classe nel codice. Ad esempio, per poter usare il `Chart` helper, il codice deve trovare la `System.Web.Helpers.Chart` (classe), che combina lo spazio dei nomi (`System.Web.Helpers`) con il nome della classe (`Chart`). Questo è noto come la classe *completi* nome &#8212; relativo percorso completo e non ambiguo all'interno di vastness di .NET Framework. Nel codice, ciò sarebbe simile al seguente:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Tuttavia, è difficile (e soggetta a errori) desidera utilizzare questi nomi lunghi, completo, ogni volta che si vuole fare riferimento a una classe o helper. Pertanto, per renderlo più semplice usare nomi di classe, è possibile *importare* gli spazi dei nomi si è interessati, ovvero in genere è una semplice manciata tra i molti spazi dei nomi in .NET Framework. Se è stato importato uno spazio dei nomi, è possibile usare solo un nome di classe (`Chart`) anziché il nome completo (`System.Web.Helpers.Chart`). Quando il codice viene eseguito e viene rilevato un nome di classe, è possibile esaminare solo gli spazi dei nomi che è stato importato per trovare quella classe.
> 
> Quando si usa ASP.NET Web Pages con sintassi Razor per creare pagine web, è in genere usare lo stesso set di classi ogni volta che, tra cui il `WebPage` classe, gli helper diversi e così via. Per salvare il lavoro di importazione di spazi dei nomi rilevante ogni volta che si crea un sito Web, ASP.NET è configurato in modo che importato automaticamente un set di spazi dei nomi core per ogni sito Web. Ecco perché ancora stato disponeva di spazi dei nomi o l'importazione fino a questo punto; tutte le classi di cui che è lavorato con sono negli spazi dei nomi già importati automaticamente.
> 
> Tuttavia, talvolta è necessario lavorare con una classe che non si trova in uno spazio dei nomi che viene importato automaticamente. In tal caso, è possibile usare nome completo di tale classe oppure è possibile importare manualmente lo spazio dei nomi che contiene la classe. Per importare uno spazio dei nomi, si utilizza il `using` istruzione (`import` in Visual Basic), come illustrato in precedenza in un esempio dell'articolo.
> 
> Ad esempio, il `DataSet` classe si trova nel `System.Data` dello spazio dei nomi. Il `System.Data` dello spazio dei nomi non è automaticamente disponibile alle pagine Razor di ASP.NET. Pertanto, per lavorare con i `DataSet` classe utilizzando il relativo nome completo, è possibile usare codice simile al seguente:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Se è necessario utilizzare il `DataSet` classe più volte è possibile importare uno spazio dei nomi simile al seguente e quindi usare solo il nome della classe nel codice:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> È possibile aggiungere `using` istruzioni per qualsiasi altro .NET Framework gli spazi dei nomi che si desidera fare riferimento. Tuttavia, come indicato, non dovrai eseguire questa operazione, spesso, in quanto la maggior parte delle classi che verranno usate sono negli spazi dei nomi che vengono importati automaticamente da ASP.NET per l'utilizzo in *. cshtml* e *vbhtml* pagine.


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Visualizzazione di grafici all'interno di una pagina Web

Negli esempi visti finora, si crea un grafico e quindi viene eseguito il rendering grafico direttamente al browser come un oggetto grafico. In molti casi, tuttavia, si desidera visualizzare un grafico come parte di una pagina, non solo da solo nel browser. A tale scopo è necessario un processo in due passaggi. Il primo passaggio consiste nel creare una pagina che viene generato l'errore al grafico, come abbiamo già visto.

Il secondo passaggio consiste nel visualizzare l'immagine risultante in un'altra pagina. Per visualizzare l'immagine, si utilizza un elemento HTML `<img>` elemento, nello stesso modo, si farebbe per visualizzare tutte le immagini. Tuttavia, anziché fare riferimento a un *jpg* o *PNG* file, il `<img>` i riferimenti agli elementi il *. cshtml* file che contiene il `Chart` helper che Crea il grafico. Quando si esegue la pagina di visualizzazione, il `<img>` elemento ottiene l'output del `Chart` helper ed esegue il rendering del grafico.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Creare un file denominato *ShowChart.cshtml*.
2. Sostituire il contenuto esistente con il codice seguente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Il codice Usa il `<img>` elemento per visualizzare il grafico creato in precedenza nel *ChartArrayBasic.cshtml* file.
3. Eseguire la pagina web in un browser. Il *ShowChart.cshtml* file consente di visualizzare l'immagine del grafico in base al codice contenuto nel *ChartArrayBasic.cshtml* file.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Applicazione di stili a un grafico

Il `Chart` helper supporta una vasta gamma di opzioni che consentono di personalizzare l'aspetto del grafico. È possibile impostare colori, tipi di carattere, i bordi e così via. Un modo semplice per personalizzare l'aspetto di un grafico consiste nell'utilizzare un *tema*. I temi sono raccolte di informazioni che specificano come eseguire il rendering di un grafico mediante i tipi di carattere, colori, etichette, tavolozze, bordi e gli effetti. Si noti che lo stile di un grafico non indica il tipo di grafico.

La tabella seguente elenca i temi predefiniti.

| Tema | Descrizione |
| --- | --- |
| `Vanilla` | Consente di visualizzare le colonne rosso su sfondo bianco. |
| `Blue` | Consente di visualizzare blu colonne su uno sfondo blu sfumato. |
| `Green` | Consente di visualizzare blu colonne su uno sfondo sfumato verde. |
| `Yellow` | Consente di visualizzare colonne arancione in un Sfumatura sfondo giallo. |
| `Vanilla3D` | Consente di visualizzare le colonne di colore rosso 3D su sfondo bianco. |

È possibile specificare il tema da usare quando si crea un nuovo grafico.

1. Creare un nuovo file denominato *ChartStyleGreen.cshtml*.
2. Sostituire il contenuto esistente nella pagina con il codice seguente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Questo codice è lo stesso dell'esempio precedente che usa il database per i dati, ma aggiunge il `theme` parametro durante la creazione di `Chart` oggetto. Di seguito viene illustrato il codice modificato:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Eseguire la pagina in un browser. Si vedranno gli stessi dati come prima, ma l'aspetto del grafico più raffinato: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Salvataggio di un grafico

Quando si usa il `Chart` helper come descritto finora in questo articolo, l'helper crea nuovamente il grafico da zero ogni volta che viene richiamato. Se necessario, il codice per il grafico esegue una nuova query del database o legge nuovamente il file XML per ottenere i dati. In alcuni casi, questa operazione può essere un'operazione complessa, ad esempio se il database che sta eseguendo la query è di grandi dimensioni o se il file XML contiene una grande quantità di dati. Anche se il grafico non richiede una notevole quantità di dati, il processo di creazione dinamica di un'immagine occupa risorse server e se molte persone richiedono le pagine che consentono di visualizzare il grafico, può esserci un impatto sulle prestazioni del sito Web.

Per ridurre il potenziale impatto sulle prestazioni della creazione di un grafico, è possibile creare una la prima volta è necessario e quindi salvare il file. Quando il grafico è necessario anche in questo caso, anziché rigenerarlo, puoi semplicemente recuperare la versione salvata ed eseguirne il rendering.

È possibile salvare un grafico nei modi seguenti:

- Memorizzare nella cache il grafico nella memoria del computer (nel server).
- Salvare il grafico come file di immagine.
- Salvare il grafico come file XML. Questa opzione consente di modificare il grafico prima di salvarla.

### <a name="caching-a-chart"></a>La memorizzazione nella cache un grafico

Dopo aver creato un grafico, può memorizzarlo nella cache. La memorizzazione nella cache un grafico significa che non deve necessariamente essere ricreati se deve essere visualizzata nuovamente. Quando si salva un grafico nella cache, occorre assegnargli una chiave che deve essere univoca per il grafico.

I grafici salvati nella cache potrebbero essere rimosso se il server in memoria sia insufficiente. Inoltre, la cache viene cancellata se l'applicazione viene riavviato per qualsiasi motivo. Pertanto, il metodo standard per lavorare con un grafico memorizzato nella cache è sempre prima di tutto verificare se è disponibile nella cache e in caso contrario, quindi per creare o crearlo nuovamente.

1. Nella radice del sito Web, creare un file denominato *ShowCachedChart.cshtml*.
2. Sostituire il contenuto esistente con il codice seguente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    Il `<img>` tag include un `src` che punta all'attributo il *ChartSaveToCache.cshtml* file e passa una chiave per la pagina come una stringa di query. La chiave contiene il valore &quot;myChartKey&quot;. Il *ChartSaveToCache.cshtml* file contiene il `Chart` helper che crea il grafico. Questa pagina verrà creata più avanti.

    Alla fine della pagina è presente un collegamento a una pagina denominata *ClearCache.cshtml*. Che è una pagina che si creeranno anche a breve. È necessario il *ClearCache.cshtml* solo per testare la memorizzazione nella cache per questo esempio, non è un collegamento o una pagina in cui è in genere necessario includere quando si lavora con i grafici memorizzato nella cache.
3. Nella radice del sito Web, creare un nuovo file denominato *ChartSaveToCache.cshtml*.
4. Sostituire il contenuto esistente con il codice seguente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Il codice di verifica innanzitutto se qualsiasi elemento è stato passato come il valore della chiave nella stringa di query. Se, pertanto, il codice tenta di leggere un grafico dalla cache chiamando il `GetFromCache` metodo e passando la chiave. Se si scopre che non c'è nulla nella cache con la chiave (che verrà eseguite la prima volta che viene richiesto il grafico), il codice crea il grafico come di consueto. Al termine il grafico, il codice lo salva nella cache chiamando `SaveToCache`. Questo metodo richiede una chiave (in modo che il grafico può essere richiesta in un secondo momento) e la quantità di tempo che il grafico deve essere salvato nella cache. (L'ora esatta che è un grafico nella cache dipendono frequenza pensavi potrebbero modificare i dati rappresentati.) Il `SaveToCache` metodo richiede anche un `slidingExpiration` parametro &#8212; se questa opzione è impostata su true, il timeout del contatore viene reimpostato ogni volta che si accede nel grafico. In questo caso, in effetti significa che voce della cache del grafico scade due minuti dopo l'ultima volta che qualcuno accede al grafico. (L'alternativa per la scadenza variabile è la scadenza assoluta, vale a dire che la voce della cache verrebbe scadono esattamente 2 minuti dopo che è stato inserito nella cache, indipendentemente dalla frequenza era stato eseguito l'accesso).

    Infine, il codice Usa il `WriteFromCache` metodo per recuperare ed eseguire il rendering del grafico dalla cache. Si noti che questo metodo è di fuori di `if` blocco che controlla la cache, perché otterrà il grafico dalla cache se il grafico è stato presente per iniziare o doveva essere generati e salvati nella cache.

    Si noti che nell'esempio di `AddTitle` metodo include un timestamp. (Aggiunge la data e ora correnti &#8212; `DateTime.Now` &#8212; per il titolo.)
5. Creare una nuova pagina denominata *ClearCache.cshtml* e sostituirne il contenuto con quanto segue:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Questa pagina Usa la `WebCache` helper per rimuovere il piano memorizzato nella cache *ChartSaveToCache.cshtml*. Come indicato in precedenza, non è in genere necessario avere una pagina simile alla seguente. Si sta creando qui solo per renderne più semplice testare la memorizzazione nella cache.
6. Eseguire la *ShowCachedChart.cshtml* pagina web in un browser. La pagina viene visualizzata l'immagine del grafico in base al codice contenuto nel *ChartSaveToCache.cshtml* file. Prendere nota di Cosa dice il timestamp del titolo del grafico. 

    ![Descrizione: Immagine del grafico di base con un timestamp del titolo del grafico](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Chiudere il browser.
8. Eseguire la *ShowCachedChart.cshtml* nuovamente. Si noti che il timestamp è lo stesso come in precedenza, che indica che il grafico non è stato rigenerato, ma invece è stato letto dalla cache.
9. Nelle *ShowCachedChart.cshtml*, fare clic sui **Cancella cache** collegamento. Verrà visualizzata *ClearCache.cshtml*, che segnala che è stata cancellata la cache.
10. Fare clic sui **tornare al ShowCachedChart.cshtml** collegare o rieseguire *ShowCachedChart.cshtml* da WebMatrix. Si noti che questa volta il timestamp è stato modificato, perché la cache è stata cancellata. Pertanto, il codice era necessario rigenerare il grafico e riportarla in cache.

### <a name="saving-a-chart-as-an-image-file"></a>Salvataggio di un grafico come File di immagine

È anche possibile salvare un grafico come file di immagine (ad esempio un *jpg* file) sul server. È quindi possibile usare il file di immagine si farebbe con qualsiasi immagine. Il vantaggio è il file è archiviato anziché essere salvato in una cache temporanea. È possibile salvare una nuova immagine del grafico in momenti diversi (ad esempio, ogni ora) e quindi mantenere un record permanente delle modifiche che si verificano nel corso del tempo. Si noti che è necessario assicurarsi che l'applicazione web dispone dell'autorizzazione per salvare un file nella cartella nel server in cui si desidera inserire il file di immagine.

1. Nella radice del sito Web, creare una cartella denominata  *\_ChartFiles* se non esiste già.
2. Nella radice del sito Web, creare un nuovo file denominato *ChartSave.cshtml*.
3. Sostituire il contenuto esistente con il codice seguente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Il codice controlla per verificare se il *jpg* file esiste, chiamare il `File.Exists` (metodo). Se il file non esiste, il codice crea un nuovo `Chart` da una matrice. Questa volta, il codice chiama il `Save` metodo e passa il `path` parametro per specificare il percorso del file e il nome di file in cui salvare il grafico. Nel corpo della pagina, un `<img>` elemento utilizza il percorso in modo che punti la *jpg* file da visualizzare.
4. Eseguire la *ChartSave.cshtml* file.
5. Tornare a WebMatrix. Si noti che un file di immagine denominato *chart01.jpg* è stata salvata nel  *\_ChartFiles* cartella.

### <a name="saving-a-chart-as-an-xml-file"></a>Salvataggio di un grafico come File XML

Infine, è possibile salvare un grafico come file XML nel server. Un vantaggio dell'uso di questo metodo tramite la memorizzazione nella cache il grafico o salvataggio del grafico in un file è che è possibile modificare il codice XML prima di visualizzare il grafico se si desidera. L'applicazione deve disporre delle autorizzazioni di lettura/scrittura per la cartella nel server in cui si desidera inserire il file di immagine.

1. Nella radice del sito Web, creare un nuovo file denominato *ChartSaveXml.cshtml*.
2. Sostituire il contenuto esistente con il codice seguente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Questo codice è simile al codice che hai visto in precedenza per l'archiviazione di un grafico nella cache, ad eccezione del fatto che usa un file XML. Il codice controlla se esiste il file XML chiamando la `File.Exists` (metodo). Se il file esiste, il codice crea un nuovo `Chart` dell'oggetto e passa il nome del file come il `themePath` parametro. Ciò consente di creare il grafico basato su qualsiasi oggetto presente in file XML. Se il file XML non esiste già, il codice crea un grafico come normale e quindi chiama `SaveXml` per salvarlo. Il grafico viene rappresentato tramite il `Write` metodo, come si è visto prima.

    Come con la pagina che illustra la memorizzazione nella cache, questo codice include un timestamp del titolo del grafico.
3. Creare una nuova pagina denominata *ChartDisplayXMLChart.cshtml* e aggiungervi il markup seguente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Eseguire la *ChartDisplayXMLChart.cshtml* pagina. Il grafico viene visualizzato. Prendere nota del timestamp nel titolo del grafico.
5. Chiudere il browser.
6. In WebMatrix, fare doppio clic il  *\_ChartFiles* cartella, fare clic su **Aggiorna**e quindi aprire la cartella. Il *XMLChart.xml* file in questa cartella è stato creato dal `Chart` helper. 

    ![Descrizione: _ChartFiles cartella che mostra il file XMLChart.xml creato dall'helper grafico.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Eseguire la *ChartDisplayXMLChart.cshtml* pagina nuovamente. Il grafico mostra lo stesso timestamp come la prima volta che è stata eseguita la pagina. Ciò avviene perché il grafico viene generato dal file XML salvato in precedenza.
8. In WebMatrix aprire il  *\_ChartFiles* cartella ed eliminare le *XMLChart.xml* file.
9. Eseguire la *ChartDisplayXMLChart.cshtml* pagina ancora una volta. Questa volta, il timestamp viene aggiornato, in quanto il `Chart` helper era necessario ricreare il file XML. Se si desidera, verificare i  *\_ChartFiles* cartella e notare che il file XML è nuovo.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione all'uso di un Database in ASP.NET Web Pages siti](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Tramite la memorizzazione nella cache in ASP.NET Web Pages siti per migliorare le prestazioni](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Classe grafico](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (riferimenti alle API di pagine Web ASP.NET su MSDN)
