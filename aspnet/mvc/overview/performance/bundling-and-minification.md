---
uid: mvc/overview/performance/bundling-and-minification
title: Bundling and Minification | Documenti Microsoft
author: Rick-Anderson
description: Creazione di bundle e minimizzazione sono due tecniche è possibile utilizzare in ASP.NET 4.5 per migliorare il tempo di caricamento richiesta. Come aggregare e minimizzazione migliora i tempi di caricamento di reducin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/23/2012
ms.topic: article
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 001ebf89cda66a50cddcd7e4944f27b9396d4450
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
---
<a name="bundling-and-minification"></a>Bundling and Minification
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Creazione di bundle e minimizzazione sono due tecniche è possibile utilizzare in ASP.NET 4.5 per migliorare il tempo di caricamento richiesta. Come aggregare e minimizzazione migliora il tempo di carico riducendo il numero di richieste al server e riduzione delle dimensioni di un asset richiesti (ad esempio CSS e JavaScript).


La maggior parte dei browser principali corrente limitare il numero di [connessioni simultanee](http://www.browserscope.org/?category=network) per ogni nome host e sei. Ciò significa che, durante l'elaborazione, le richieste di sei ulteriori richieste per l'asset in un host verranno accodate dal browser. Nell'immagine seguente, le schede di rete strumenti per sviluppatori F12 di Internet Explorer è indicato l'intervallo per le risorse necessarie per la visualizzazione di informazioni su un'applicazione di esempio.

![B/M](bundling-and-minification/_static/image1.png)

Le barre grigie mostrano il tempo che la richiesta viene accodata dal browser in attesa sul limite di sei connessione. La barra gialla è il tempo di richiesta per il primo byte, vale a dire, il tempo impiegato per inviare la richiesta e ricevere la prima risposta dal server. Le barre blu mostrano il tempo impiegato per ricevere i dati di risposta dal server. È possibile fare doppio clic su una risorsa per ottenere informazioni dettagliate sulla tempistica. Ad esempio, l'immagine seguente mostra i dettagli dell'intervallo per il caricamento di */Scripts/MyScripts/JavaScript6.js* file.

![](bundling-and-minification/_static/image2.png)

Nell'immagine precedente viene illustrato il **avviare** evento, l'ora in cui la richiesta è stata accodata a causa del browser che consente di limitare il numero di connessioni simultanee. In questo caso, la richiesta è stata accodata per 46 millisecondi attendere il completamento di un'altra richiesta.

## <a name="bundling"></a>Creazione di bundle

Creazione di bundle è una nuova funzionalità in ASP.NET 4.5 che rende più semplice combinare o aggregare più file in un singolo file. È possibile creare CSS, JavaScript e altri pacchetti. Un minor numero di file indica un minor numero di richieste HTTP e che possono migliorare le prestazioni di caricamento prima pagina.

L'immagine seguente mostra la vista durata della visualizzazione di informazioni su indicata in precedenza, ma questa volta con l'aggregazione e minimizzazione abilitato.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minimizzazione

Minimizzazione esegue diverse ottimizzazioni di codice diverse per gli script o css, ad esempio rimuovendo gli spazi vuoti non necessari e i commenti e abbreviare i nomi delle variabili per un carattere. Si consideri la seguente funzione JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Dopo la minimizzazione, la funzione viene ridotto al seguente:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Oltre a rimuovere i commenti e gli spazi vuoti non necessari, i seguenti parametri e i nomi delle variabili sono stati rinominati (abbreviato) come indicato di seguito:

| **Originale** | **Rinominato** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Impatto di Bundling and Minification

La tabella seguente illustra alcune importanti differenze tra l'elenco di tutte le risorse singolarmente e l'utilizzo di aggregazione e minimizzazione (B/M) nel programma di esempio.

|  | **Utilizzo di B/M** | **Senza B/M** | **Modifica** |
| --- | --- | --- | --- |
| **Richieste di file** | 9 | 34 | 256% |
| **KB inviati** | 3.26 | 11.92 | 266% |
| **KB ricevuti** | 388.51 | 530 | 36% |
| **Tempo di caricamento** | 510 MS | 780 MS | 53% |

I byte inviati aveva una significativa riduzione con aggregazione come browser sono piuttosto dettagliati con le intestazioni HTTP che si applicano alle richieste. La riduzione di byte ricevuto non è più grande poiché i file più grande (*Scripts\jquery-ui-1.8.11.min.js* e *Scripts\jquery-1.7.1.min.js*) sono già minimizzare. Nota: Gli intervalli di tempo nel programma di esempio utilizzati la [Fiddler](http://www.fiddler2.com/fiddler2/) strumento per simulare una rete lenta. (Da di Fiddler **regole** dal menu **prestazioni** quindi **simulare velocità Modem**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Debug in bundle e minimizzare JavaScript

È possibile eseguire il debug di JavaScript in un ambiente di sviluppo (in cui il [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) nel *Web. config* file è impostato su `debug="true"` ) perché non sono inclusi i file JavaScript o minimizzare. È anche possibile eseguire il debug di una build di rilascio in cui i file JavaScript sono collegati e minimizzare. Utilizzando gli strumenti di sviluppo F12 di Internet Explorer, si esegue il debug una funzione JavaScript inclusa in un bundle minimizzato utilizzando l'approccio seguente:

1. Selezionare il **Script** e quindi selezionare il **avviare il debug** pulsante.
2. Selezionare il pacchetto che contiene la funzione JavaScript che si desidera eseguire il debug utilizzando il pulsante di asset.  
    ![](bundling-and-minification/_static/image4.png)
3. Formattare il codice JavaScript minimizzato selezionando il **pulsante configurazione** ![](bundling-and-minification/_static/image5.png)e quindi selezionando **JavaScript formato**.
4. Nel **ricerca scripting** casella di input di t, selezionare il nome della funzione per eseguire il debug. Nella figura seguente, **AddAltToImg** è stato immesso il **ricerca scripting** casella di input t.  
    ![](bundling-and-minification/_static/image6.png)

Per ulteriori informazioni sul debug con gli strumenti di sviluppo F12, vedere l'articolo MSDN [utilizzando gli strumenti di sviluppo F12 per eseguire il Debug di errori JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Controllo Bundling and Minification

Come aggregare e riduzione è abilitato o disabilitato impostando il valore dell'attributo debug il [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) nel *Web. config* file. Il codice XML seguente, `debug` è impostato su true, aggregazione e riduzione è disabilitata.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Per abilitare l'aggregazione e riduzione, impostare il `debug` valore su "false". È possibile eseguire l'override di *Web. config* impostazione con il `EnableOptimizations` proprietà la `BundleTable` classe. Il codice seguente consente di aggregazione e riduzione e sostituisce qualsiasi impostazione configurata nel *Web. config* file.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> A meno che non `EnableOptimizations` è `true` o l'attributo di debug nel [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) nel *Web. config* file è impostato su `false`, i file non verranno inseriti o minimizzare. Inoltre, non verrà utilizzata la versione .min dei file, verranno selezionate le versioni di debug complete. `EnableOptimizations` sostituisce l'attributo di debug nel [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) nel *Web. config* file


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Utilizzando Bundling and Minification con Web Form ASP.NET e pagine Web

- Per le pagine Web, vedere il post di blog [aggiunta di ottimizzazione Web a un sito Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Per Web Form, vedere il post di blog [aggiunta Bundling and Minification per Web Form](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Utilizzando Bundling and Minification con ASP.NET MVC

In questa sezione si creerà un ASP.NET MVC progetto per esaminare l'aggregazione e riduzione. Innanzitutto, creare un nuovo progetto ASP.NET MVC internet denominato **MvcBM** senza modificare le impostazioni predefinite.

Aprire il *App\_Start\BundleConfig.cs* file ed esaminare il `RegisterBundles` metodo utilizzato per creare, registrare e configurare pacchetti. Il codice seguente viene mostrata una parte di `RegisterBundles` metodo.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Nel codice precedente viene creata una nuova aggregazione JavaScript denominata *~/bundles/jquery* che include tutti i (debug o minimizzare ma non. *vsdoc*) file di *script* cartella che corrisponde alla stringa con caratteri jolly "~/Scripts/jquery-{version}. js". Per ASP.NET MVC 4, questo significa che con una configurazione di debug, il file *jquery 1.7.1.js* verrà aggiunto al bundle. In una configurazione di rilascio, *jquery 1.7.1.min.js* verrà aggiunto. Il framework di aggregazione conforme alle convenzioni più comuni, ad esempio:

- Selezione file ".min" per il rilascio quando esistono "FileX.min.js" e "FileX.js".
- Selezionare la versione non ".min" per il debug.
- Verrà ignorato "-vsdoc" file (ad esempio jquery-1.7.1-vsdoc.js), che vengono utilizzati solo da IntelliSense.

Il `{version}` con caratteri jolly corrispondente illustrato in precedenza viene utilizzato per creare automaticamente un pacchetto jQuery con la versione appropriata di jQuery nel *script* cartella. In questo esempio, con un carattere jolly offre i vantaggi seguenti:

- Consente di usare NuGet per l'aggiornamento a una versione più recente di jQuery senza modificare il codice precedente di aggregazione o riferimenti jQuery nelle pagine di visualizzazione.
- Consente di selezionare la versione completa per le configurazioni di debug e la versione ".min" per il rilascio crea automaticamente.

## <a name="using-a-cdn"></a>Utilizzo di una rete CDN

 Nel codice seguente sostituisce il pacchetto jQuery locale con un bundle di jQuery CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Nel codice precedente, verrà richiesta jQuery dalla rete CDN mentre nella versione modalità e la versione di debug di jQuery verranno recuperati in locale in modalità di debug. Quando si utilizza una rete CDN, è necessario un meccanismo di fallback nel caso in cui la richiesta della rete CDN non riesce. Il markup seguente frammento dalla fine dello script Mostra file layout aggiunta alla richiesta di jQuery è necessario il failover della rete CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Creazione di un pacchetto

Il [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `Include` metodo accetta una matrice di stringhe, in cui ogni stringa è un percorso virtuale alla risorsa. Il codice riportato di seguito dal metodo RegisterBundles il *App\_Start\BundleConfig.cs* file viene illustrato come più file vengono aggiunti a un bundle:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

Il [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `IncludeDirectory` metodo è fornito per aggiungere tutti i file in una directory (e, facoltativamente, tutte le sottodirectory) che corrispondono a un criterio di ricerca. Il [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `IncludeDirectory` API è illustrato di seguito:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Bundle vengono fatto riferimento nelle viste tramite il metodo Render, ( `Styles.Render` per CSS e `Scripts.Render` per JavaScript). Il markup seguente dal *Views\Shared\\layout. cshtml* file viene illustrato come le visualizzazioni predefinite internet ASP.NET riferimento bundle CSS e JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Si noti come i metodi di rendering accetta una matrice di stringhe, pertanto è possibile aggiungere più bundle in una riga di codice. In genere è possibile utilizzare i metodi di rendering che creano il codice HTML necessario per fare riferimento all'asset. È possibile utilizzare il `Url` metodo per generare l'URL della risorsa senza il tag deve fare riferimento all'asset. Si supponga che si desidera utilizzare il nuovo HTML5 [async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) attributo. Il codice seguente viene illustrato come fare riferimento a modernizr utilizzando il `Url` metodo.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Utilizzo di "\*" carattere jolly per selezionare i file

Il percorso virtuale specificato nel `Include` (metodo) e la ricerca di schema nel `IncludeDirectory` metodo può accettare uno "\*" carattere jolly come prefisso o suffisso per l'ultimo segmento del percorso. La stringa di ricerca viene fatta distinzione tra maiuscole e minuscole. Il `IncludeDirectory` metodo ha la possibilità di cercare nelle sottodirectory.

Si consideri un progetto con i seguenti file JavaScript:

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![imag Dir](bundling-and-minification/_static/image7.png)

La tabella seguente illustra i file aggiunti a un bundle utilizzando il carattere jolly come illustrato:

| **Call** | **Eccezione generata o i file aggiunti** |
| --- | --- |
| Includere ("~/Scripts/Common/\*. js") | *AddAltToImg.js, ToggleDiv.js, ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Eccezione di modello non valido. Il carattere jolly è consentito solo nel prefisso o suffisso. |
| Includere ("~/Scripts/Common/\*og.\*") | Eccezione di modello non valido. È consentito un solo carattere jolly. |
| "Includono (" ~/Scripts/Common/T\*") | *ToggleDiv.js, ToggleImg.js* |
| "Includono (" ~/Scripts/Common/\*") | Eccezione di modello non valido. Un segmento con carattere jolly pure non è valido. |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js, ToggleImg.js* |
| IncludeDirectory("~/Scripts/Common", "T\*",true) | *ToggleDiv.js, ToggleImg.js, ToggleLinks.js* |

Aggiunta di ogni file in modo esplicito a un bundle è generalmente preferite tramite caricamento con caratteri jolly di file per i motivi seguenti:

- Aggiunta di script per impostazione predefinita con caratteri jolly per caricarli in ordine alfabetico, che è in genere non si desidera. File CSS e JavaScript spesso devono essere aggiunti in un ordine specifico (non alfabetici). È possibile ridurre questo rischio mediante l'aggiunta di un oggetto personalizzato [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementazione, ma in modo esplicito l'aggiunta di ogni file è meno soggetto a errori. Ad esempio, è possibile aggiungere nuove attività in una cartella in futuro potrebbe essere necessario modificare il [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementazione.
- Visualizza file specifici aggiunti a una directory tramite il caricamento di carattere jolly possono essere incluso in tutte le viste che fanno riferimento a tale bundle. Se lo script di visualizzazione specifico viene aggiunto a un bundle, si potrebbe visualizzare un errore di JavaScript in altre visualizzazioni che fa riferimento il pacchetto.
- File CSS che importano altri file di generare i file importati caricati due volte. Ad esempio, il codice seguente crea un bundle con la maggior parte dei file jQuery UI tema CSS caricati due volte. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Il selettore con caratteri jolly "\*CSS" porta in ogni file CSS nella cartella, inclusi il *Content\themes\base\jquery.ui.all.css* file. Il *jquery.ui.all.css* file Importa altri file CSS.

## <a name="bundle-caching"></a>Aggregare la memorizzazione nella cache

Bundle di impostare l'intestazione HTTP di scadenza un anno dalla creazione di bundle. Se si passa a una pagina visualizzata in precedenza, Mostra Fiddler IE non effettuare una richiesta condizionale per il bundle, vale a dire, esistono non richieste HTTP GET in Internet Explorer per le aggregazioni e nessuna risposta 304 HTTP dal server. È possibile forzare l'inserimento/espulsione per effettuare una richiesta condizionale per ogni bundle con il tasto F5 (risultante in una risposta HTTP 304 per ogni bundle). È possibile forzare un aggiornamento completo utilizzando ^ F5 (risultante in una risposta HTTP 200 per ogni bundle).

La figura seguente mostra il **la memorizzazione nella cache** scheda del riquadro Fiddler risposta:

![immagine di memorizzazione nella cache di Fiddler](bundling-and-minification/_static/image8.png)

La richiesta   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 è per il bundle **AllMyScripts** e contiene una coppia di stringa di query **v = r0sLDicvP58AIXN\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. La stringa di query **v** ha un valore di token che rappresenta un identificatore univoco utilizzato per la memorizzazione nella cache. Fino a quando non viene modificato il bundle, l'applicazione ASP.NET richiede la **AllMyScripts** bundle con questo token. Se viene modificato un file nel bundle, il framework di ottimizzazione di ASP.NET genera un nuovo token, garantendo che alle richieste del browser per il bundle consentirà di ottenere il pacchetto più recente.

Se si eseguono gli strumenti di sviluppo F12 IE9 e passare a una pagina caricata in precedenza, Internet Explorer in modo non corretto Mostra GET condizionale richieste per ogni pacchetto e il server di restituzione di HTTP 304. È possibile leggere perché IE9 presenta problemi di determinare se è stata effettuata una richiesta condizionale nella voce del blog [CDN utilizzando e scadenza per migliorare le prestazioni del sito Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>CoffeeScript, SCSS, Sass minore, creazione di bundle.

Il framework di aggregazione e minimizzazione fornisce un meccanismo per l'elaborazione, ad esempio linguaggi intermedi [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [meno](http://www.dotlesscss.org/) o [Coffeescript ](http://coffeescript.org/)e applicare trasformazioni, ad esempio riduzione per il pacchetto risulta. Ad esempio, per aggiungere [.less](http://www.dotlesscss.org/) file al progetto MVC 4:

1. Creare una cartella per il contenuto meno. L'esempio seguente usa il *Content\MyLess* cartella.
2. Aggiungere il [.less](http://www.dotlesscss.org/) pacchetto NuGet **punto** al progetto.  
    ![Installazione senza punti NuGet](bundling-and-minification/_static/image9.png)
3. Aggiungere una classe che implementa il [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) interfaccia. Per la trasformazione .less, aggiungere il codice seguente al progetto.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Creare un bundle di meno i file con il `LessTransform` e [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) trasformare. Aggiungere il codice seguente per il `RegisterBundles` metodo il *App\_Start\BundleConfig.cs* file.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Aggiungere il codice seguente a visualizzazioni che fa riferimento il pacchetto di minore.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Considerazioni di aggregazione

Una convenzione valida da seguire durante la creazione di bundle consiste nell'includere "aggregare" come prefisso nel nome del pacchetto. Ciò impedirà un possibile [conflitto routing](https://forums.asp.net/post/5012037.aspx).

Quando si aggiorna un file in un bundle, viene generato un nuovo token per il parametro di stringa di query di aggregazione e il pacchetto completo deve essere scaricato alla successiva esecuzione di che un client richiede una pagina contenente il bundle. Nel markup tradizionale in cui ogni asset viene elencata singolarmente, potrebbe essere scaricato solo il file modificato. Asset modificati di frequente non sono buoni candidati per la creazione di bundle.

Come aggregare e minimizzazione principalmente migliorare il tempo di caricamento di prima pagina richiesta. Una volta che è stata richiesta una pagina Web, il browser memorizza nella cache le risorse (JavaScript, CSS e immagini) e aggregazione e riduzione non forniscono alcun miglioramento delle prestazioni quando si richiede la stessa pagina oppure pagine nello stesso sito richiede le stesse risorse. Se non si imposta la scadenza testata correttamente le risorse e non si utilizza come aggregare e minimizzazione, l'euristica di aggiornamento del browser contrassegnerà l'asset non aggiornati dopo alcuni giorni e il browser richiederà una richiesta di convalida per ogni asset. In questo caso, aggregazione e minimizzazione forniscono un aumento delle prestazioni dopo la prima richiesta di pagina. Per informazioni dettagliate, vedere il blog [CDN utilizzando e scadenza per migliorare le prestazioni del sito Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

La limitazione di browser di sei connessioni simultanee per ogni nome host può essere ridotto utilizzando un [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Poiché la rete CDN avrà un nome host diverso rispetto al sito di hosting, le richieste di asset dalla rete CDN non verranno considerata rispetto al limite di connessioni simultanee sei all'ambiente di hosting. Una rete CDN può anche fornire comuni pacchetto di memorizzazione nella cache e la memorizzazione nella cache i vantaggi di bordo.

Bundle devono essere partizionati dalle pagine di cui sono necessari. Ad esempio, l'impostazione predefinita, il modello MVC ASP.NET per un'applicazione internet crea un bundle di convalida jQuery separato dal jQuery. Poiché le visualizzazioni predefinite create non dispone di alcun input e non registra i valori, non includono il bundle di convalida.

Il `System.Web.Optimization` dello spazio dei nomi viene implementato in System.Web.Optimization.DLL. Sfrutta la libreria WebGrease (WebGrease.dll) per le funzionalità di riduzione, che a sua volta utilizza Antlr3.Runtime.dll.

*È possibile utilizzare Twitter rendere rapido post e condividere i collegamenti. Handle Twitter è*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Risorse aggiuntive

- Video:[Bundling and ottimizzazione](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) da [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Aggiunta di ottimizzazione Web a un sito Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Aggiunta Bundling and Minification per Web Form](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Implicazioni sulle prestazioni di Bundling and Minification nel browser Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) da [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Usando le reti CDN e scade per migliorare le prestazioni del sito Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) di Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Ridurre al minimo RTT (tempi di round trip)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Contributors

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
