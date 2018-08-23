---
uid: mvc/overview/performance/bundling-and-minification
title: Creazione di bundle e minimizzazione | Microsoft Docs
author: Rick-Anderson
description: Creazione di bundle e minimizzazione sono due tecniche è possibile usare in ASP.NET 4.5 per migliorare il tempo di caricamento di richiesta. Creazione di bundle e minimizzazione migliora il tempo di caricamento reducin...
ms.author: riande
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 7e54bdd2f50edb5001982ada9b6ce023584ce5b0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834421"
---
<a name="bundling-and-minification"></a>Creazione di bundle e minimizzazione
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Creazione di bundle e minimizzazione sono due tecniche è possibile usare in ASP.NET 4.5 per migliorare il tempo di caricamento di richiesta. Creazione di bundle e minimizzazione aumenta il tempo di caricamento riducendo il numero di richieste al server e ridurre le dimensioni dell'asset richiesti (ad esempio CSS e JavaScript.)


La maggior parte dei browser principali corrente limitare il numero di [connessioni simultanee](http://www.browserscope.org/?category=network) per ogni nome host a 6. Ciò significa che durante l'elaborazione le sei richieste, richieste aggiuntive per gli asset in un host verranno inserito nella coda dal browser. Nell'immagine seguente, le schede di rete F12 di Internet Explorer developer tools indicato l'intervallo per gli asset necessari per la visualizzazione di informazioni su un'applicazione di esempio.

![B/M](bundling-and-minification/_static/image1.png)

Le barre grigie indicano il tempo impiegato che dal browser in attesa sul limite di sei connessione viene accodata la richiesta. La barra gialla è il tempo di richiesta per il primo byte, vale a dire, il tempo impiegato per inviare la richiesta e ricevere la prima risposta dal server. Le barre blue mostrano il tempo impiegato per ricevere i dati di risposta dal server. È possibile fare doppio clic su un asset per ottenere informazioni dettagliate sugli intervalli. Ad esempio, l'immagine seguente mostra i dettagli dell'intervallo per il caricamento di */Scripts/MyScripts/JavaScript6.js* file.

![](bundling-and-minification/_static/image2.png)

Illustrato nell'immagine precedente il **avviare** evento, l'ora della richiesta è stata accodata a causa del browser che consente di limitano il numero di connessioni simultanee. In questo caso, la richiesta è stata accodata per 46 millisecondi in attesa di un'altra richiesta di completamento.

## <a name="bundling"></a>Creazione di bundle

L'aggregazione è una nuova funzionalità di ASP.NET 4.5 che rende più semplice combinare o più file del bundle in un unico file. È possibile creare altre aggregazioni, JavaScript e CSS. Minor numero di file indica un minor numero di richieste HTTP e che possono migliorare le prestazioni di carico prima pagina.

L'immagine seguente mostra la visualizzazione stessa temporizzazione della visualizzazione About illustrata in precedenza, ma questa volta con creazione di bundle e minimizzazione abilitata.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minimizzazione

Minimizzazione eseguirà una serie di ottimizzazioni di codice diverse per gli script o css, ad esempio la rimozione di commenti e spazi vuoti non necessari e ad abbreviare i nomi delle variabili per un carattere. Si consideri la seguente funzione di JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Dopo la minimizzazione, la funzione viene ridotto al seguente:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Oltre a rimuovere i commenti e spazi vuoti non necessari, i parametri seguenti e i nomi delle variabili sono state rinominate (abbreviato) come indicato di seguito:

| **Originale** | **Rinominato** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Impatto della creazione di bundle e minimizzazione

La tabella seguente illustra alcune differenze importanti tra elencando tutte le risorse singolarmente e usando creazione di bundle e minimizzazione (B/M) nel programma di esempio.

|  | **Uso di B/M** | **Senza B/M** | **Modifica** |
| --- | --- | --- | --- |
| **Richieste di file** | 9 | 34 | 256% |
| **KB inviati** | 3.26 | 11.92 | 266% |
| **KB ricevuti** | 388.51 | 530 | 36% |
| **Tempo di caricamento** | 510 MS | 780 MS | 53% |

I byte inviati aveva una significativa riduzione con creazione di bundle come i browser sono piuttosto dettagliati con le intestazioni HTTP che si applicano alle richieste. La riduzione di byte ricevuti non è più grande poiché i file più grandi (*gli script\\jquery-ui-1.8.11.min.js* e *script\\jquery 1.7.1.min.js*) sono già minimizzati . Nota: Gli intervalli di tempo nel programma di esempio usata la [Fiddler](http://www.fiddler2.com/fiddler2/) dello strumento per simulare una rete lenta. (Da di Fiddler **regole** dal menu **prestazioni** quindi **simulare le velocità dei Modem**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Debug in bundle e minimizzato JavaScript

È possibile eseguire il debug di JavaScript in un ambiente di sviluppo (in cui il [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) nel *Web. config* file sia impostato su `debug="true"` ) perché non sono inclusi i file JavaScript o minimizzati. È anche possibile eseguire il debug di una build di rilascio in cui i file JavaScript sono in bundle e minimizzati. Usa gli strumenti di sviluppo F12 di Internet Explorer, si esegue il debug funzione JavaScript incluso in un bundle minimizzato usando l'approccio seguente:

1. Selezionare il **Script** scheda e quindi selezionare la **Avvia debug** pulsante.
2. Selezionare il pacchetto che contiene la funzione JavaScript da sottoporre a debug utilizzando il pulsante di asset.  
    ![](bundling-and-minification/_static/image4.png)
3. Formattare il codice JavaScript minimizzato selezionando il **pulsante di configurazione** ![](bundling-and-minification/_static/image5.png), quindi selezionando **formato JavaScript**.
4. Nel **Script di ricerca** casella di input, selezionare il nome della funzione da sottoporre a debug. Nell'immagine seguente, **AddAltToImg** è stato immesso con la **Script di ricerca** casella di input.  
    ![](bundling-and-minification/_static/image6.png)

Per altre informazioni sul debug con gli strumenti di sviluppo F12, vedere l'articolo MSDN [usando gli strumenti di sviluppo F12 per eseguire il Debug degli errori JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Controllo creazione di bundle e minimizzazione

Creazione di bundle e minimizzazione è abilitato o disabilitato impostando il valore dell'attributo debug nel [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) nel *Web. config* file. Il codice XML seguente, `debug` è impostato su true, creazione di bundle e minimizzazione è disabilitato.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Per abilitare la creazione di bundle e minimizzazione, impostare il `debug` valore su "false". È possibile eseguire l'override di *Web. config* impostazione con il `EnableOptimizations` proprietà il `BundleTable` classe. Il codice seguente consente di creazione di bundle e minimizzazione e sostituisce qualsiasi impostazione nel *Web. config* file.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> A meno che non `EnableOptimizations` viene `true` o l'attributo di debug nel [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) nel *Web. config* file sia impostato su `false`, i file non verrà incluso o minimizzati. Inoltre, la versione .min dei file non verrà utilizzata, verranno selezionate le versioni di debug completi. `EnableOptimizations` sostituisce l'attributo di debug nel [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) nel *Web. config* file


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Tramite creazione di bundle e minimizzazione con Web Form ASP.NET e pagine Web

- Per le pagine Web, vedere il post di blog [aggiunta di ottimizzazione Web per un sito Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Per Web Form, vedere il post di blog [aggiunta Bundling and Minification a Web Form](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Tramite creazione di bundle e minimizzazione ASP.NET MVC

In questa sezione si creerà un ASP.NET MVC progetto esaminare la creazione di bundle e minimizzazione. In primo luogo, creare un nuovo progetto ASP.NET MVC internet denominato **MvcBM** senza modificare le impostazioni predefinite.

Aprire il *App\\\_avviare\\BundleConfig.cs* file ed esaminare il `RegisterBundles` metodo che consente di creare, registrare e configurare bundle. Il codice seguente illustra una parte di `RegisterBundles` (metodo).

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Il codice precedente crea un nuovo bundle di JavaScript denominato *~/bundles/jquery* che include tutti i (debug o che ma non minimizzati. *vsdoc*) file con il *script* cartella che corrisponde alla stringa con caratteri jolly "~/Scripts/jquery-{version}. js". Per ASP.NET MVC 4, questo significa che con una configurazione di debug, il file *jquery 1.7.1.js* verrà aggiunto al bundle. In una configurazione rilascio *jquery 1.7.1.min.js* verrà aggiunto. Il framework di creazione di bundle segue le convenzioni comuni diverse, ad esempio:

- La selezione di file ".min" per versione quando *FileX.min.js* e *FileX.js* esiste.
- Selezionare la versione non ".min" per il debug.
- Verrà ignorato "-vsdoc" i file (ad esempio *jquery-1.7.1-vsdoc. js.*), che vengono usati solo da IntelliSense.

Il `{version}` con caratteri jolly corrispondente illustrato in precedenza viene usato per creare automaticamente un bundle di jQuery con la versione appropriata di jQuery nel *script* cartella. In questo esempio, usando un carattere jolly offre i vantaggi seguenti:

- Consente di usare NuGet per l'aggiornamento a una versione più recente di jQuery senza modificare il codice di creazione di bundle precedente o jQuery riferimenti nelle pagine di visualizzazione.
- Seleziona la versione completa per le configurazioni di debug e la versione ".min" per il rilascio crea automaticamente.

## <a name="using-a-cdn"></a>Con una rete CDN

 Nel codice seguente sostituisce il bundle di jQuery locale con un bundle di jQuery della rete CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Nel codice precedente, jQuery viene richiesto dalla rete CDN mentre nella versione modalità e la versione di debug di jQuery verranno recuperati in locale in modalità di debug. Quando si usa una rete CDN, è necessario un meccanismo di fallback nel caso in cui la richiesta della rete CDN non riesce. Il markup seguente frammento dalla fine dello script Mostra file di layout aggiunta alla richiesta di jQuery deve gli avranno esito negativo della rete CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Creazione di un Bundle

Il [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `Include` metodo accetta una matrice di stringhe, dove ogni stringa è un percorso virtuale alla risorsa. Nell'esempio di codice dal `RegisterBundles` metodo nella *App\\\_Start\\BundleConfig.cs* file Mostra come più file vengono aggiunti a un bundle:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

Il [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `IncludeDirectory` metodo è fornito per aggiungere tutti i file in una directory (e, facoltativamente, tutte le sottodirectory) che corrispondono a un criterio di ricerca. Il [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `IncludeDirectory` API è illustrato di seguito:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Bundle vengono fatto riferimento nelle viste tramite il metodo Render, (`Styles.Render` per CSS e `Scripts.Render` per JavaScript). Il markup seguente dal *viste\\Shared\\\_layout. cshtml* file Mostra come impostazione predefinita ASP.NET internet progetto un riferimento a bundle CSS e JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Si noti che i metodi di rendering accetta una matrice di stringhe, pertanto è possibile aggiungere più aggregazioni in una riga di codice. In genere desiderato da usare i metodi di rendering che creano il codice HTML necessario per fare riferimento all'asset. È possibile usare il `Url` metodo per generare l'URL per l'asset senza il markup necessario per fare riferimento all'asset. Supponiamo di voler usare HTML5 nuove [async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) attributo. Il codice seguente viene illustrato come fare riferimento modernizr mediante il `Url` (metodo).

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Uso di "\*" carattere jolly per selezionare i file

Il percorso virtuale specificato nel `Include` metodo e la ricerca di creare una serie di `IncludeDirectory` metodo può accettare uno "\*" carattere jolly come prefisso o suffisso nell'ultimo segmento del percorso. La stringa di ricerca è maiuscole e minuscole. Il `IncludeDirectory` metodo ha la possibilità di cercare nelle sottodirectory.

Si consideri un progetto con i seguenti file JavaScript:

- *Gli script\\comuni\\AddAltToImg.js*
- *Gli script\\comuni\\ToggleDiv.js*
- *Gli script\\comuni\\ToggleImg.js*
- *Gli script\\comuni\\Sub1\\ToggleLinks.js*

![imag Dir](bundling-and-minification/_static/image7.png)

La tabella seguente illustra i file aggiunti a un bundle utilizzando il carattere jolly, come illustrato:

| **Call** | **Eccezione generata o i file aggiunti** |
| --- | --- |
| Include ("~/Scripts/Common/\*. js") | *AddAltToImg.js*, *ToggleDiv.js*, *ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Eccezione di modello non valido. Il carattere jolly è consentito solo nel prefisso o suffisso. |
| Include ("~/Scripts/Common/\*og.\*") | Eccezione di modello non valido. È consentito un solo carattere jolly. |
| Include ("~/Scripts/Common/T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| Include ("~/Scripts/Common/\*") | Eccezione di modello non valido. Un segmento con carattere jolly pure non è valido. |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*": true) | *ToggleDiv.js*, *ToggleImg.js*, *ToggleLinks.js* |

In modo esplicito l'aggiunta di ogni file a un bundle è in genere il valore desiderato tramite il caricamento con caratteri jolly dei file per i motivi seguenti:

- Aggiunta di script per impostazione predefinita con caratteri jolly per il caricamento in ordine alfabetico, che è in genere non si desidera. File CSS e JavaScript devono spesso essere aggiunti in un ordine specifico (non alfabetici). È possibile ridurre questo rischio mediante l'aggiunta di una classe personalizzata [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementazione, ma in modo esplicito l'aggiunta di ogni file è meno soggetto a errori. Ad esempio, è possibile aggiungere nuovi asset in una cartella in futuro che potrebbero richiedere di modificare il [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementazione.
- File specifici di vista aggiunti a una directory tramite il caricamento di carattere jolly possono essere incluso in tutte le viste che fanno riferimento a tale bundle. Se lo script specifici di visualizzazione viene aggiunto a un bundle, possibile che si verifichi un errore di JavaScript in altre viste che fanno riferimento l'aggregazione.
- File CSS che altri file di importazione generare i file importati caricati due volte. Ad esempio, il codice seguente crea un bundle con la maggior parte dei file jQuery UI temi CSS caricati due volte. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Il selettore con caratteri jolly "\*CSS" porta in ogni file CSS nella cartella, tra cui la *contenuto\\temi\\base\\jquery.ui.all.css* file. Il *jquery.ui.all.css* file Importa altri file CSS.

## <a name="bundle-caching"></a>Creare un bundle di memorizzazione nella cache

Bundle di impostare l'intestazione HTTP scade un anno da quando viene creato il bundle. Se si passa a una pagina visualizzata in precedenza, Mostra Fiddler IE non esegue una richiesta condizionale per il bundle, vale a dire, esistono nessuna richiesta HTTP GET da Internet Explorer per i bundle e delle risposte HTTP 304 dal server. È possibile forzare Internet Explorer per effettuare una richiesta condizionale per ogni aggregazione con il tasto F5 (risultante in una risposta HTTP 304 per ogni aggregazione). È possibile forzare un aggiornamento completo utilizzando ^ F5 (risultante in una risposta HTTP 200 per ogni aggregazione).

La figura seguente mostra le **Caching** scheda del riquadro di risposta di Fiddler:

![immagine di memorizzazione nella cache di Fiddler](bundling-and-minification/_static/image8.png)

La richiesta   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 è per il bundle **AllMyScripts** e contiene una coppia di stringa di query **v = r0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. La stringa di query **v** ha un valore di token che rappresenta un identificatore univoco utilizzato per la memorizzazione nella cache. Fino a quando non viene modificati il bundle, l'applicazione ASP.NET richiede la **AllMyScripts** del bundle usando il token. Se qualsiasi file nel bundle cambia, il framework di ottimizzazione di ASP.NET genera un nuovo token, garantendo che le richieste del browser per il bundle consentirà di ottenere il pacchetto più recente.

Se si eseguono strumenti di sviluppo F12 di Internet Explorer 9 e passare a una pagina caricata in precedenza, Internet Explorer in modo non corretto Mostra richieste GET condizionale apportate a ogni bundle e il server di restituzione di HTTP 304. È possibile leggere il motivo per cui Internet Explorer 9 presenta problemi che determina se è stata effettuata una richiesta condizionale nella voce del blog [usando le reti CDN ed Expires per migliorare le prestazioni dei siti Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>CoffeeScript, SCSS, Sass meno, creazione di bundle.

Il framework di creazione di bundle e minimizzazione fornisce un meccanismo per l'elaborazione, ad esempio linguaggi intermedi [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [meno](http://www.dotlesscss.org/) o [Coffeescript ](http://coffeescript.org/)e applicare trasformazioni, ad esempio minimizzazione per il pacchetto risulta. Ad esempio, per aggiungere [.less](http://www.dotlesscss.org/) file al progetto MVC 4:

1. Creare una cartella per il contenuto di LESS. L'esempio seguente usa il *contenuti\\MyLess* cartella.
2. Aggiungere il [.less](http://www.dotlesscss.org/) pacchetto NuGet **senza punto** al progetto.  
    ![Installazione di NuGet senza punto](bundling-and-minification/_static/image9.png)
3. Aggiungere una classe che implementa il [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) interfaccia. Per la trasformazione .less, aggiungere il codice seguente al progetto.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Creare un bundle di file di LESS con il `LessTransform` e il [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) trasformare. Aggiungere il codice seguente per il `RegisterBundles` metodo nella *App\\avvia\\BundleConfig.cs* file.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Aggiungere il codice seguente per tutte le viste che fa riferimento il bundle di LESS.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Considerazioni di aggregazione

Una buona convenzione da seguire durante la creazione di bundle consiste nell'includere "del bundle" come prefisso sotto il nome del bundle. Ciò impedirà a un possibile [conflitto routing](https://forums.asp.net/post/5012037.aspx).

Dopo aver aggiornato un file in un bundle, viene generato un nuovo token per il parametro di stringa di query di bundle e l'aggregazione completa deve essere scaricato la volta successiva che un client richiede una pagina che contiene il bundle. Nel markup tradizionale in cui ogni asset viene elencata singolarmente, potrebbe essere scaricato solo il file modificato. Gli asset che cambiano di frequente non siano buoni candidati per la creazione di bundle.

Creazione di bundle e minimizzazione principalmente migliorare il tempo di caricamento di prima pagina richiesta. Dopo che è stata richiesta una pagina Web, il browser vengono memorizzati nella cache gli asset (JavaScript, CSS e immagini) e creazione di bundle e minimizzazione non offriranno alcun miglioramento delle prestazioni quando si richiede la stessa pagina oppure pagine nello stesso computer del sito che richiede le stesse risorse. Se non si imposta l'intestazione correttamente gli asset, scade e non si usa creazione di bundle e minimizzazione, l'euristica freshness browser contrassegnerà l'asset non aggiornato dopo alcuni giorni e il browser richiederà una richiesta di convalida per ogni asset. Creazione di bundle e minimizzazione in questo caso, fornire un aumento delle prestazioni dopo la prima richiesta di pagina. Per informazioni dettagliate, vedere il blog [usando le reti CDN ed Expires per migliorare le prestazioni dei siti Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

La limitazione di browser di sei connessioni simultanee per ogni nome host può essere attenuata con un [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Poiché la rete CDN avrà un nome host diverso rispetto al sito di hosting, le richieste di asset dalla rete CDN non verranno calcolata rispetto al limite di connessioni simultanee sei al proprio ambiente di hosting. Una rete CDN può anche fornire più comuni pacchetti di memorizzazione nella cache e i vantaggi di memorizzazione nella cache perimetrale.

Aggregazioni devono essere partizionate dalle pagine che ne hanno bisogno. Ad esempio, il modello MVC di ASP.NET per un'applicazione internet predefinito crea un bundle di convalida di jQuery separato dal jQuery. Poiché le visualizzazioni predefinite create non dispone di alcun input e non pubblicare i valori, non includono il bundle di convalida.

Il `System.Web.Optimization` dello spazio dei nomi viene implementato in *System.Web.Optimization.dll*. Sfrutta la libreria WebGrease (*WebGrease.dll*) per le funzionalità di minimizzazione, che a sua volta utilizza *Antlr3.Runtime.dll*.

*Usare Twitter per rendere rapide post e condividere collegamenti. L'handle di Twitter*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Risorse aggiuntive

- Video:[creazione di bundle e ottimizzazione](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) da [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Aggiunta di ottimizzazione Web a un sito Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Aggiunta Bundling and Minification a Web Form](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Implicazioni sulle prestazioni di creazione di bundle e minimizzazione in esplorazione sul Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) da [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Usando le reti CDN e scade per migliorare le prestazioni dei siti Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) di Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Ridurre al minimo i tempo RTT (tempi di round trip)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Contributors

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
