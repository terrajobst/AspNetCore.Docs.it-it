---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: La visualizzazione dei dati in un grafico con le pagine Web ASP.NET (Razor) | Documenti Microsoft
author: microsoft
description: "In questo capitolo viene illustrato come visualizzare i dati in un grafico. Nei capitoli precedenti, è stato descritto come visualizzare i dati manualmente e in una griglia. Viene spiegato come..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: f252b74bc42d0ea65b8b1150973c4f3c50cc9cf4
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>La visualizzazione dei dati in un grafico con le pagine Web ASP.NET (Razor)
====================
by [Microsoft](https://github.com/microsoft)

> In questo articolo viene illustrato come utilizzare un grafico per visualizzare i dati in un sito Web ASP.NET Web Pages (Razor) utilizzando il `Chart` helper.
> 
> **Si apprenderà**:
> 
> - Come visualizzare i dati in un grafico.
> - Viene descritto come applicare uno stile grafici tramite i temi predefiniti.
> - Come salvare i grafici e come memorizzarli nella cache per migliorare le prestazioni.
> 
> Si tratta di programmazione delle funzionalità introdotte nell'articolo ASP.NET:
> 
> - Il `Chart` helper.
> 
> > [!NOTE]
> > Le informazioni contenute in questo articolo si applicano a pagine Web ASP.NET 1.0 e 2 di pagine Web.


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>L'Helper grafico

Quando si desidera visualizzare i dati sotto forma di grafico, è possibile utilizzare `Chart` helper. Il `Chart` helper può eseguire il rendering di un'immagine che visualizza i dati in una vasta gamma di tipi di grafico. Supporta numerose opzioni di formattazione e l'assegnazione di etichette. Il `Chart` supporto può rendere più di 30 tipi di grafici, inclusi tutti i tipi di grafici che è possibile avere familiarità con da Microsoft Excel o altri strumenti &#8212; grafici ad area, grafici a barre, istogrammi, grafici a linee e i grafici a torta, insieme a informazioni i grafici azionari, ad esempio grafici speciali.

| **Grafico ad area** ![descrizione: immagine del tipo di grafico ad Area](7-displaying-data-in-a-chart/_static/image1.jpg) | **Grafico a barre** ![descrizione: immagine del tipo di grafico a barre](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Istogramma** ![descrizione: immagine del tipo di grafico](7-displaying-data-in-a-chart/_static/image3.jpg) | **Grafico a linee** ![descrizione: immagine del tipo di grafico a linee](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Grafico a torta** ![descrizione: immagine del tipo di grafico a torta](7-displaying-data-in-a-chart/_static/image5.jpg) | **Grafico azionario** ![descrizione: immagine del tipo di grafico azionario](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elementi del grafico

I grafici mostrano dati e altri elementi quali legende, assi, serie e così via. Nell'immagine seguente sono illustrati molti degli elementi del grafico che è possibile personalizzare quando si utilizza il `Chart` helper. In questo articolo viene illustrato come impostare alcune (non tutte) di questi elementi.

![Descrizione: Immagine che mostra gli elementi del grafico](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Creazione di un grafico dai dati

I dati visualizzati in un grafico possono essere da una matrice, dai risultati restituiti da un database o da dati presenti in un file XML.

### <a name="using-an-array"></a>Utilizzo di una matrice

Come spiegato in [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890), una matrice consente di archiviare una raccolta di elementi simili in una singola variabile. È possibile utilizzare matrici per contenere i dati che si desidera includere nel grafico.

Questa procedura viene illustrato come creare un grafico di dati nelle matrici, utilizzando il tipo di grafico predefinito. Viene inoltre illustrato come visualizzare il grafico all'interno della pagina.

1. Creare un nuovo file denominato *ChartArrayBasic.cshtml*.
2. Sostituire il contenuto esistente con il seguente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Innanzitutto, il codice crea un nuovo grafico e imposta la larghezza e altezza. Specificare il titolo del grafico utilizzando il `AddTitle` metodo. Per aggiungere dati, si utilizza il `AddSeries` metodo. In questo esempio, utilizzare il `name`, `xValue`, e `yValues` parametri del `AddSeries` metodo. Il `name` parametro viene visualizzato nella legenda del grafico. Il `xValue` parametro contiene una matrice di dati che viene visualizzati lungo l'asse orizzontale del grafico. Il `yValues` parametro contiene una matrice di dati utilizzato per tracciare i punti verticali del grafico.

    Il `Write` metodo effettivamente esegue il rendering del grafico. In questo caso, poiché non hai specificato un tipo di grafico, il `Chart` helper esegue il rendering il grafico predefinito, ovvero un grafico a colonne.
3. Eseguire la pagina nel browser. Il browser viene visualizzato il grafico. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Utilizzo di una Query di Database per i dati del grafico

Se le informazioni desiderate per creare un grafico in un database, è possibile eseguire una query sul database e quindi utilizzare i dati dai risultati per creare il grafico. Questa procedura viene illustrato come leggere e visualizzare i dati dal database creato nell'articolo [Introduzione all'uso di un Database nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Aggiungere un *App\_dati* cartella radice del sito Web se la cartella non esiste già.
2. Nel *App\_dati* cartella, aggiungere il file di database denominato *SmallBakery.sdf* descritto nel [Introduzione all'uso di un Database nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Creare un nuovo file denominato *ChartDataQuery.cshtml*.
4. Sostituire il contenuto esistente con il seguente:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Il codice innanzitutto apre il database SmallBakery e assegnarlo a una variabile denominata `db`. Questa variabile rappresenta un `Database` oggetto che può essere utilizzato per leggere e scrivere nel database. Successivamente, il codice esegue una query SQL per ottenere il nome e il prezzo di ogni prodotto. Il codice crea un nuovo grafico e passa la query di database a esso chiamando il grafico `DataBindTable` metodo. Questo metodo accetta due parametri: il `dataSource` parametro è per i dati dalla query e `xField` parametro consente di impostare la colonna di dati utilizzata per l'asse x del grafico.

    Come alternativa all'utilizzo di `DataBindTable` (metodo), è possibile utilizzare il `AddSeries` metodo il `Chart` helper. Il `AddSeries` metodo consente di impostare il `xValue` e `yValues` parametri. Ad esempio, anziché utilizzare il `DataBindTable` metodo simile al seguente:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    È possibile utilizzare il `AddSeries` metodo simile al seguente:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Entrambi eseguono il rendering gli stessi risultati. Il `AddSeries` metodo è più flessibile perché è possibile specificare il tipo di grafico e i dati in modo più esplicito, ma la `DataBindTable` metodo è più facile da utilizzare se non sono necessarie più flessibile.
5. Eseguire la pagina in un browser. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Utilizzo di dati XML

La terza opzione per la creazione di grafici consiste nell'utilizzare un file XML dei dati per il grafico. Ciò richiede che il file XML dispone anche di un file di schema (*XSD* file) che descrive la struttura XML. In questa procedura viene illustrato come leggere dati da un file XML.

1. Nel *App\_dati* cartella, creare un nuovo file XML denominato *data.xml*.
2. Sostituire il documento XML esistente con i seguenti elementi, alcuni dati XML relativi ai dipendenti di una società fittizia. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. Nel *App\_dati* cartella, creare un nuovo file XML denominato *data.xsd*. (Si noti che l'estensione in questo caso è *XSD*.)
4. Sostituire il codice XML esistente con il seguente: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Nella radice del sito Web, creare un nuovo file denominato *ChartDataXML.cshtml*.
6. Sostituire il contenuto esistente con il seguente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Il codice crea prima una `DataSet` oggetto. Questo oggetto viene utilizzato per gestire i dati che viene letto dal file XML e organizzarli in base alle informazioni nel file di schema. (Si noti che nella parte superiore del codice inclusa l'istruzione `using SystemData`. Questa operazione è necessaria per poter funzionare con il `DataSet` oggetto. Per ulteriori informazioni, vedere [ &quot;Using&quot; istruzioni e i nomi completi](#SB_UsingStatements) più avanti in questo articolo.)

    Successivamente, il codice crea un `DataView` oggetto basato sul set di dati. La visualizzazione di dati fornisce un oggetto che è possibile associare il grafico a &#8212; , leggere e tracciare. Il grafico viene associato a dati mediante il `AddSeries` (metodo), come visto prima quando grafici i dati, tranne il fatto che questa volta il `xValue` e `yValues` sono impostati sul `DataView` oggetto.

    In questo esempio viene inoltre illustrato come specificare un particolare tipo di grafico. Quando i dati vengono aggiunti nel `AddSeries` (metodo), il `chartType` parametro viene impostato anche per visualizzare un grafico a torta.
7. Eseguire la pagina in un browser. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP] 
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Istruzioni "Using" e i nomi completi
> 
> .NET Framework basato su ASP.NET Web Pages con sintassi Razor è costituito da molte migliaia di componenti (classi). Per rendere più gestibile per utilizzare tutte queste classi, sono organizzati in *gli spazi dei nomi*, che sono paragonabili alle librerie. Ad esempio, il `System.Web` dello spazio dei nomi contiene classi che supportano la comunicazione browser/server, il `System.Xml` spazio dei nomi contiene classi che consentono di creare e leggere i file XML, e `System.Data` dello spazio dei nomi contiene classi che consentono di lavorare con i dati.
> 
> Per accedere a qualsiasi classe fornita in .NET Framework, deve conoscere non solo il nome della classe, ma anche lo spazio dei nomi della classe nel codice. Ad esempio, per poter utilizzare il `Chart` helper, il codice deve trovare il `System.Web.Helpers.Chart` (classe), che combina lo spazio dei nomi (`System.Web.Helpers`) con il nome di classe (`Chart`). Questo è noto come la classe *completo* nome &#8212; relativo percorso completo e non ambiguo all'interno di vastness di .NET Framework. Nel codice, questo avrà un aspetto simile al seguente:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Tuttavia, è un'operazione complessa (e soggetta a errori) per utilizzare questi nomi lunghi, completo ogni volta che si desidera fare riferimento a una classe o il supporto. Pertanto, per rendere più facile da utilizzare nomi di classe, è possibile *importare* gli spazi dei nomi si è interessati, in genere è un numero limitato tra i molti spazi dei nomi in .NET Framework. Se è stato importato uno spazio dei nomi, è possibile utilizzare un nome di classe (`Chart`) anziché il nome completo (`System.Web.Helpers.Chart`). Quando il codice viene eseguito e viene rilevato un nome di classe, è possibile esaminare solo gli spazi dei nomi importati per trovare la classe.
> 
> Quando si utilizza ASP.NET Web Pages con sintassi Razor per creare pagine web, di solito utilizza lo stesso set di classi ogni volta, tra cui la `WebPage` classe, gli helper diversi e così via. Per salvare il lavoro di importare gli spazi dei nomi pertinenti, ogni volta che si crea un sito Web, ASP.NET è configurato in modo che importa automaticamente un set di spazi dei nomi core per ogni sito Web. Ecco perché non disponeva di spazi dei nomi o l'importazione fino ad ora; tutte le classi che con cui si è lavorato sono negli spazi dei nomi già importati automaticamente.
> 
> Tuttavia, talvolta è necessario utilizzare una classe che non si trova in uno spazio dei nomi viene importato automaticamente. In tal caso, è possibile utilizzare il nome completo della classe oppure è possibile importare manualmente lo spazio dei nomi che contiene la classe. Per importare uno spazio dei nomi, si utilizza il `using` istruzione (`import` in Visual Basic), come illustrato nell'esempio precedente l'articolo.
> 
> Ad esempio, il `DataSet` classe si trova nel `System.Data` dello spazio dei nomi. Il `System.Data` dello spazio dei nomi non è automaticamente disponibile per le pagine ASP.NET Razor. Pertanto, per funzionare con la `DataSet` classe utilizzando il nome completo, è possibile usare codice simile al seguente:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Se è necessario utilizzare la `DataSet` classe più volte è possibile importare uno spazio dei nomi simile al seguente e quindi utilizzare solo il nome della classe nel codice:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> È possibile aggiungere `using` per qualsiasi altro .NET Framework spazi dei nomi che si desidera fare riferimento. Tuttavia, come già indicato, non dovrai eseguire questa operazione, è spesso perché la maggior parte delle classi che verranno utilizzate negli spazi dei nomi che vengono importati automaticamente da ASP.NET per l'utilizzo in *. cshtml* e *. vbhtml* pagine.


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>La visualizzazione grafici all'interno di una pagina Web

Negli esempi si è visto fino a questo punto, si crea un grafico e quindi il grafico viene eseguito il rendering direttamente al browser come un oggetto grafico. In molti casi, tuttavia, si desidera visualizzare un grafico come parte di una pagina, non solo da solo nel browser. Per eseguire questa operazione richiede un processo in due passaggi. Il primo passaggio consiste nel creare una pagina che genera il grafico, come abbiamo già visto.

Il secondo passaggio consiste per visualizzare l'immagine risultante in un'altra pagina. Per visualizzare l'immagine, utilizzare un elemento HTML `<img>` elemento, nello stesso modo, si farebbe per visualizzare un'immagine. Tuttavia, invece che fanno riferimento a un *jpg* o *PNG* file, il `<img>` riferimenti a elementi il *. cshtml* file che contiene il `Chart` helper che Crea il grafico. Quando viene eseguita la pagina di visualizzazione, la `<img>` elemento ottiene l'output del `Chart` supporto ed esegue il rendering del grafico.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Creare un file denominato *ShowChart.cshtml*.
2. Sostituire il contenuto esistente con il seguente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Il codice Usa il `<img>` elemento per visualizzare il grafico creato in precedenza nel *ChartArrayBasic.cshtml* file.
3. Eseguire la pagina web in un browser. Il *ShowChart.cshtml* file consente di visualizzare l'immagine del grafico in base al codice contenuto nel *ChartArrayBasic.cshtml* file.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Stile di un grafico

Il `Chart` helper supporta una vasta gamma di opzioni che consentono di personalizzare l'aspetto del grafico. È possibile impostare colori, tipi di carattere, i bordi e così via. Un modo semplice per personalizzare l'aspetto di un grafico consiste nell'utilizzare un *tema*. I temi sono raccolte di informazioni che specificano come eseguire il rendering di un grafico utilizzando i tipi di carattere, colori, etichette, tavolozze, bordi ed effetti. Si noti che lo stile di un grafico non indica il tipo di grafico.

Nella tabella seguente sono elencati i temi predefiniti.

| Tema | Descrizione |
| --- | --- |
| `Vanilla` | Visualizza le colonne di colore rosse su sfondo bianco. |
| `Blue` | Consente di visualizzare blu colonne su uno sfondo sfumato blu. |
| `Green` | Consente di visualizzare blu colonne su uno sfondo sfumato verde. |
| `Yellow` | Visualizza le colonne arancione su uno sfondo giallo sfumato. |
| `Vanilla3D` | Visualizza le colonne di colore rosse 3D su sfondo bianco. |

È possibile specificare il tema da usare quando si crea un nuovo grafico.

1. Creare un nuovo file denominato *ChartStyleGreen.cshtml*.
2. Sostituire il contenuto esistente nella pagina con le operazioni seguenti:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Questo codice è identico all'esempio precedente che utilizza il database per dati, ma aggiunge il `theme` parametro durante la creazione di `Chart` oggetto. Di seguito viene illustrato il codice modificato:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Eseguire la pagina in un browser. Visualizzare gli stessi dati, ma l'aspetto del grafico più elegante: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Salvataggio di un grafico

Quando si utilizza il `Chart` helper come visto finora in questo articolo, l'helper ricrea il grafico da zero a ogni volta che viene richiamato. Se necessario, il codice per il grafico nuovamente interroga il database o viene letto di nuovo il file XML per ottenere i dati. In alcuni casi, questa operazione può essere un'operazione complessa, ad esempio se il database che sta eseguendo la query è di grandi dimensioni o se il file XML contiene una grande quantità di dati. Anche se il grafico non prevede l'utilizzo di una grande quantità di dati, il processo di creazione di un'immagine in modo dinamico richiede le risorse del server e, se molte persone richiedono le pagine che consentono di visualizzare il grafico, è possibile che un impatto sulle prestazioni del sito Web.

Per ridurre il potenziale impatto sulle prestazioni della creazione di un grafico, è possibile creare una grafico la prima ora è necessario e quindi salvarlo. Quando il grafico diventa di nuovo necessario, anziché rigenerarlo, è possibile recuperare la versione salvata ed eseguire il rendering che.

È possibile salvare un grafico nei modi seguenti:

- Memorizzare nella cache il grafico nella memoria del computer (nel server).
- Salvare il grafico come file di immagine.
- Salvare il grafico come file XML. Questa opzione consente di modificare il grafico prima di salvarlo.

### <a name="caching-a-chart"></a>La memorizzazione nella cache di un grafico

Dopo aver creato un grafico, è possibile memorizzarlo nella cache. La memorizzazione nella cache di un grafico significa che non è necessario creare di nuovo se deve essere visualizzato di nuovo. Quando si salva un grafico nella cache, assegnare una chiave che deve essere univoca per il grafico.

Grafici salvati nella cache potrebbero essere rimossi se il server è insufficiente memoria. Inoltre, la cache viene cancellata se si riavvia l'applicazione per qualsiasi motivo. Pertanto, la modalità standard di gestione di un grafico memorizzato nella cache è sempre a controllare innanzitutto se è disponibile nella cache in caso contrario, quindi creare o ricrearlo.

1. Alla radice del sito Web, creare un file denominato *ShowCachedChart.cshtml*.
2. Sostituire il contenuto esistente con il seguente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    Il `<img>` tag include un `src` attributo che punta al *ChartSaveToCache.cshtml* file e passa una chiave alla pagina come una stringa di query. La chiave contiene il valore &quot;myChartKey&quot;. Il *ChartSaveToCache.cshtml* file contiene il `Chart` helper che crea il grafico. Si creerà in questa pagina tra qualche secondo.

    Alla fine della pagina è presente un collegamento a una pagina denominata *ClearCache.cshtml*. Che è una pagina che verrà creata anche a breve. È necessario il *ClearCache.cshtml* solo per testare la memorizzazione nella cache per questo esempio, non è un collegamento o una pagina che include, in genere quando si utilizzano grafici memorizzati nella cache.
3. Alla radice del sito Web, creare un nuovo file denominato *ChartSaveToCache.cshtml*.
4. Sostituire il contenuto esistente con il seguente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Il codice controlla innanzitutto se qualsiasi elemento è stato passato come il valore della chiave nella stringa di query. Se in tal caso, il codice tenta di leggere un grafico dalla cache chiamando il `GetFromCache` metodo e passando la chiave. Se si scopre che non c'è nulla nella cache in tale chiave (che verrà eseguite la prima volta che viene richiesto il grafico), il codice crea il grafico come di consueto. Al termine il grafico, il codice salva nella cache chiamando `SaveToCache`. Tale metodo richiede una chiave (in modo grafico può essere richiesta in un secondo momento) e la quantità di tempo che il grafico deve essere salvato nella cache. (L'ora esatta, che è necessario memorizzare nella cache di un grafico dipendono la frequenza con cui è considerato potrebbero modificare i dati che rappresenta.) Il `SaveToCache` metodo richiede inoltre un `slidingExpiration` parametro &#8212; se questa proprietà è impostata su true, il timeout del contatore viene reimpostato ogni volta che si accede al grafico. In questo caso, in pratica significa che voce della cache del grafico scade due minuti dopo l'ultima volta che un utente accede al grafico. (L'alternativa per la scadenza variabile è la scadenza assoluta, vale a dire che la voce della cache scade esattamente 2 minuti inserito nella cache, indipendentemente dalla frequenza con cui era stato possibile accedervi).

    Infine, il codice Usa il `WriteFromCache` metodo per recuperare ed eseguire il rendering del grafico dalla cache. Si noti che questo metodo è di fuori di `if` blocco che controlla la cache, perché questa recupera il grafico dalla cache se il grafico è inizialmente o deve essere generato e salvato nella cache.

    Si noti che nell'esempio di `AddTitle` metodo include un timestamp. (Aggiunge la data e ora correnti &#8212; `DateTime.Now` &#8212; al titolo.)
5. Creare una nuova pagina denominata *ClearCache.cshtml* e sostituirne il contenuto con quanto segue:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Questa pagina utilizza il `WebCache` helper per rimuovere il piano memorizzato nella cache *ChartSaveToCache.cshtml*. Come notato in precedenza, non è in genere necessario avere una pagina simile al seguente. Si sta creando qui solo per rendere più semplice testare la memorizzazione nella cache.
6. Eseguire il *ShowCachedChart.cshtml* pagina web in un browser. La pagina consente di visualizzare l'immagine del grafico in base al codice contenuto nel *ChartSaveToCache.cshtml* file. Prendere nota dei quali è indicato il timestamp del titolo del grafico. 

    ![Descrizione: Immagine del grafico di base con un timestamp del titolo del grafico](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Chiudere il browser.
8. Eseguire il *ShowCachedChart.cshtml* nuovamente. Si noti che il timestamp è lo stesso come in precedenza, che indica che il grafico non è stato rigenerato, ma che invece è stato letto dalla cache.
9. In *ShowCachedChart.cshtml*, fare clic su di **Cancella cache** collegamento. Viene visualizzata *ClearCache.cshtml*, che segnala che la cache è stata cancellata.
10. Fare clic su di **tornare a ShowCachedChart.cshtml** collegare, o eseguire di nuovo *ShowCachedChart.cshtml* da WebMatrix. Si noti che questa volta il timestamp è stato modificato, perché la cache è stata cancellata. Pertanto, il codice era necessario rigenerare il grafico e inserirlo nuovamente nella cache.

### <a name="saving-a-chart-as-an-image-file"></a>Salvataggio di un grafico come File di immagine

È inoltre possibile salvare un grafico come file di immagine (ad esempio, un *jpg* file) nel server. È quindi possibile utilizzare il file di immagine si farebbe con qualsiasi immagine. Il vantaggio è il file è archiviato anziché essere salvato in una cache temporanea. È possibile salvare una nuova immagine del grafico in momenti diversi (ad esempio, ogni ora) e quindi mantenere un record permanente delle modifiche che avvengono nel tempo. Si noti che è necessario assicurarsi che l'applicazione web dispone dell'autorizzazione per salvare un file nella cartella sul server in cui si desidera inserire il file di immagine.

1. Alla radice del sito Web, creare una cartella denominata  *\_ChartFiles* se non esiste già.
2. Alla radice del sito Web, creare un nuovo file denominato *ChartSave.cshtml*.
3. Sostituire il contenuto esistente con il seguente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Il codice innanzitutto verificato se il *jpg* file esiste, chiamare il `File.Exists` (metodo). Se il file non esiste, il codice crea un nuovo `Chart` da una matrice. Questa volta, il codice chiama il `Save` metodo e passa il `path` parametro per specificare il percorso del file e il nome di file in cui salvare il grafico. Nel corpo della pagina, un `<img>` elemento utilizza il percorso per puntare il *jpg* file da visualizzare.
4. Eseguire il *ChartSave.cshtml* file.
5. Tornare a WebMatrix. Si noti che un file di immagine denominato *chart01.jpg* è stato salvato il  *\_ChartFiles* cartella.

### <a name="saving-a-chart-as-an-xml-file"></a>Salvataggio di un grafico come File XML

Infine, è possibile salvare un grafico come file XML nel server. Un vantaggio dell'utilizzo di questo metodo tramite la memorizzazione nella cache il grafico o salvataggio del grafico in un file è che è possibile modificare il codice XML prima di visualizzare il grafico se si desidera. L'applicazione deve disporre delle autorizzazioni di lettura/scrittura per la cartella nel server in cui si desidera inserire il file di immagine.

1. Alla radice del sito Web, creare un nuovo file denominato *ChartSaveXml.cshtml*.
2. Sostituire il contenuto esistente con il seguente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Questo codice è simile al codice illustrato in precedenza per l'archiviazione di un grafico nella cache, ad eccezione del fatto che usa un file XML. Il codice controlla innanzitutto se il file XML esistente chiamando il `File.Exists` metodo. Se il file esiste, il codice crea un nuovo `Chart` dell'oggetto e passa il nome di file come il `themePath` parametro. Crea il grafico in base a qualunque sia nel file XML. Se il file XML non esiste già, il codice crea un grafico come normale e quindi chiama `SaveXml` per salvarlo. Il grafico viene eseguito utilizzando il `Write` (metodo), come si è visto prima.

    Come con la pagina che indica che la memorizzazione nella cache, questo codice include un timestamp del titolo del grafico.
3. Creare una nuova pagina denominata *ChartDisplayXMLChart.cshtml* e aggiungere il markup seguente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Eseguire il *ChartDisplayXMLChart.cshtml* pagina. Il grafico viene visualizzato. Prendere nota del timestamp nel titolo del grafico.
5. Chiudere il browser.
6. In WebMatrix, fare doppio clic su di  *\_ChartFiles* cartella, fare clic su **aggiornamento**e quindi aprire la cartella. Il *XMLChart.xml* file in questa cartella è stato creato dal `Chart` helper. 

    ![Descrizione: _ChartFiles cartella con il file XMLChart.xml creato dall'helper della grafico.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Eseguire il *ChartDisplayXMLChart.cshtml* pagina nuovamente. Il grafico mostra lo stesso timestamp durante la prima che esecuzione della pagina. Ciò avviene perché il grafico viene generato il file XML salvato in precedenza.
8. In WebMatrix, aprire il  *\_ChartFiles* cartella ed eliminare il *XMLChart.xml* file.
9. Eseguire il *ChartDisplayXMLChart.cshtml* pagina ancora una volta. Questa volta, il timestamp viene aggiornato, in quanto il `Chart` helper deve ricreare il file XML. Se si desidera, controllare il  *\_ChartFiles* cartella e si noti che il file XML è nuovo.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione all'uso di un Database in ASP.NET Web Pages siti](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Tramite la memorizzazione nella cache in ASP.NET Web Pages siti per migliorare le prestazioni](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Classe grafico](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (riferimenti alle API di pagine Web ASP.NET su MSDN)
