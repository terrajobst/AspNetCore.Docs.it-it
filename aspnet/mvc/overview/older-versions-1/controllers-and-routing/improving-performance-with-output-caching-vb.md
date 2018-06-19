---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: Miglioramento delle prestazioni con Output memorizzazione nella cache (VB) | Documenti Microsoft
author: microsoft
description: In questa esercitazione viene illustrato come è possibile migliorare notevolmente le prestazioni delle applicazioni web ASP.NET MVC, sfruttando la cache di output. Si...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: 8ee933b477307f5c3f2377e112a1a98d3d6bc337
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874421"
---
<a name="improving-performance-with-output-caching-vb"></a>Miglioramento delle prestazioni con Output la memorizzazione nella cache (VB)
====================
by [Microsoft](https://github.com/microsoft)

> In questa esercitazione viene illustrato come è possibile migliorare notevolmente le prestazioni delle applicazioni web ASP.NET MVC, sfruttando la cache di output. Informazioni su come memorizzare nella cache il risultato restituito da un'azione del controller in modo che lo stesso contenuto non devono essere creati ogni volta che un nuovo utente richiama l'azione.


L'obiettivo di questa esercitazione è illustrare come è possibile migliorare notevolmente le prestazioni di un'applicazione ASP.NET MVC, sfruttando la cache di output. La cache di output consente di memorizzare nella cache il contenuto restituito da un'azione del controller. In questo modo, lo stesso contenuto non è necessario essere generato ogni volta che viene richiamata l'azione del controller stesso.

Si supponga, ad esempio, che l'applicazione ASP.NET MVC consente di visualizzare un elenco di record del database in una vista denominata Index. In genere, ogni volta che un utente richiama l'azione del controller che restituisce la visualizzazione dell'indice, il set di record del database deve recuperare dal database eseguendo una query sul database.

Se, d'altra parte, sfruttare i vantaggi della cache di output è possibile evitare l'esecuzione di una query di database ogni volta che un utente richiama la stessa azione del controller. La vista può essere recuperata dalla cache anziché il modello viene rigenerato dall'azione del controller. La memorizzazione nella cache consente di evitare l'esecuzione di ridondanza di operare sul server.

#### <a name="enabling-output-caching"></a>Abilitare la memorizzazione nella cache di Output

Abilitare la memorizzazione nella cache di output aggiungendo un &lt;OutputCache&gt; attributo per un'azione del controller singoli o una classe controller intero. Ad esempio, il controller nel listato 1 espone un'azione denominata index (). L'output dell'azione Index () viene memorizzato nella cache per 10 secondi.

**Elenco 1 – Controllers\HomeController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]


Nelle versioni Beta di ASP.NET MVC, la memorizzazione nella cache di output non funziona per un URL, ad esempio [ http://www.MySite.com/ ](http://www.mysite.com/). Al contrario, è necessario immettere un URL, ad esempio [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index).


Nel listato 1, l'output dell'azione Index () viene memorizzato nella cache per 10 secondi. Se si preferisce, è possibile specificare una durata maggiore di cache. Ad esempio, se si desidera memorizzare nella cache l'output di un'azione del controller per un giorno è possibile specificare una durata della cache di 86400 secondi (60 secondi \* 60 minuti \* 24 ore).

Vi è alcuna garanzia che il contenuto non nella cache per la quantità di tempo specificato. Quando le risorse di memoria diventino insufficiente, la cache viene avviata automaticamente la rimozione del contenuto.

Il controller Home nel listato 1 restituisce la visualizzazione dell'indice nel listato 2. Non c'è niente di speciale su questa vista. La visualizzazione dell'indice vengono visualizzate l'ora corrente (vedere la figura 1).

**Elenco 2 – Views\Home\Index.aspx.**

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

**Figura 1 – visualizzazione dell'indice memorizzata nella cache**

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

Se si richiama l'azione Index () più volte immettendo l'URL /Home/indice nella barra degli indirizzi del browser e premendo il pulsante di aggiornamento/Ricarica nel browser più volte, l'ora visualizzata per la visualizzazione dell'indice non cambierà per 10 secondi. Contemporaneamente viene visualizzato perché la vista viene memorizzato nella cache.

È importante comprendere che la stessa vista viene memorizzato nella cache per tutti gli utenti che visita l'applicazione. Tutti gli utenti che richiama l'azione Index () otterrà la stessa versione memorizzata nella cache della visualizzazione dell'indice. Ciò significa che viene ridotta notevolmente la quantità di lavoro che il server web deve eseguire per gestire la visualizzazione dell'indice.

La visualizzazione nel listato 2 avviene per eseguire un'operazione estremamente semplice. La vista visualizza solo l'ora corrente. Tuttavia, è possibile semplicemente come facilmente cache di una vista che viene visualizzato un set di record del database. In tal caso, il set di record del database non dovrebbe essere recuperate dal database ogni volta che viene richiamata l'azione del controller che restituisce la vista. La memorizzazione nella cache, è possibile ridurre la quantità di lavoro che deve eseguire il server web e server di database.

Non utilizzare la pagina &lt;% @ OutputCache %&gt; direttiva in una visualizzazione MVC. Questa direttiva è di uscita dall'ambiente di Web Form e non deve essere utilizzata in un'applicazione MVC ASP.NET. 

#### <a name="where-content-is-cached"></a>In cui contenuto viene memorizzato nella cache

Per impostazione predefinita, quando si utilizza il &lt;OutputCache&gt; attributo contenuto nella cache in tre posizioni: il server web, tutti i server proxy e il web browser. È possibile controllare esattamente in cui contenuto viene memorizzato nella cache modificando la proprietà Location del &lt;OutputCache&gt; attributo.

È possibile impostare la proprietà Location a uno dei valori seguenti:

> · Qualsiasi
> 
> · Client
> 
> · Downstream
> 
> · Server
> 
> · Nessuno
> 
> · ServerAndClient


Per impostazione predefinita, la proprietà Location contiene il valore Any. Tuttavia, esistono casi in cui potrebbe memorizzare nella cache solo nel browser o solo sul server. Ad esempio, se si memorizza nella cache informazioni personalizzati per ogni utente quindi non devono essere memorizzati le informazioni sul server. Se si visualizzano informazioni diverse a utenti diversi è consigliabile memorizzare nella cache le informazioni solo sul client.

Ad esempio, il controller nel listato 3 espone un'azione denominata GetName() che restituisce il nome utente corrente. Se presa accede al sito Web e richiama l'azione GetName() quindi l'azione restituisce la stringa "Hi presa". Se, successivamente, Jill accede al sito Web e richiama l'azione GetName() quindi potrà anche otterrà la stringa "Hi presa". La stringa viene memorizzato nella cache sul server web per tutti gli utenti dopo presa richiama inizialmente l'azione del controller.

**Elenco di 3: Controllers\BadUserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

È probabile che il controller nel listato 3 non funziona il modo in cui si desidera. Non si desidera visualizzare il messaggio "Presa Hi" a Jill.

È mai deve memorizzare nella cache il contenuto personalizzato nella cache del server. Tuttavia, è consigliabile memorizzare nella cache il contenuto personalizzato nella cache del browser per migliorare le prestazioni. Se si memorizza nella cache il contenuto nel browser e un utente richiama la stessa azione controller più volte, può recuperare il contenuto dalla cache del browser invece che nel server.

Il controller modificato nel listato 4 memorizza nella cache l'output dell'azione GetName(). Tuttavia, il contenuto rimane nella cache solo nel browser e non nel server. In questo modo, quando più utenti richiamano il metodo GetName(), ogni persona Ottiene il proprio nome utente e nome utente della persona non in un altro.

**Listato 4 – Controllers\UserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

Si noti che il &lt;OutputCache&gt; attributo listato 4 include una proprietà di percorso impostata sul valore OutputCacheLocation.Client. Il &lt;OutputCache&gt; attributo include anche una proprietà di NoStore. La proprietà NoStore viene utilizzata per informare i browser e server proxy che non devono archiviare una copia permanente del contenuto della cache.

#### <a name="varying-the-output-cache"></a>Variare la Cache di Output

In alcune situazioni, è possibile utilizzare versioni diverse di memorizzati nella cache del contenuto della stessa. Si supponga, ad esempio, che si sta creando una pagina master-details. La pagina master consente di visualizzare un elenco di titoli dei film. Quando si fa clic su un titolo, si ricevono dettagli per il film selezionato.

Se si memorizza nella cache la pagina dei dettagli, i dettagli per lo stesso filmato verranno visualizzati indipendentemente da quale film si fa clic su. Verrà visualizzato il primo film selezionato per il primo utente a tutti gli utenti future.

È possibile risolvere questo problema sfruttando della proprietà VaryByParam il &lt;OutputCache&gt; attributo. Questa proprietà consente di creare versioni diverse di memorizzati nella cache del contenuto quando un parametro form molto stesso o se il parametro di stringa di query varia.

Ad esempio, il controller nel listato 5 espone due azioni denominate Master() e Details(). L'azione Master() restituisce un elenco di titoli dei film e l'azione Details() restituisce i dettagli per il film selezionato.

**Nel listato 5 – Controllers\MoviesController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

L'azione Master() include una proprietà VaryByParam con il valore "none". Viene restituito quando l'azione viene richiamata, Master() la stessa versione memorizzata nella cache del Master Visualizza. Eventuali parametri di modulo o la stringa di query i parametri vengono ignorati (vedere Figura 2).

**Figura 2 – vista /Movies/Master**

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

**Figura 3 – la visualizzazione dei dettagli/filmati /**

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

L'azione Details() include una proprietà VaryByParam con il valore "Id". Quando diversi valori del parametro Id vengono passati all'azione del controller, vengono generate diverse versioni memorizzate nella cache della vista Dettagli.

È importante comprendere i risultati della proprietà VaryByParam in più di memorizzazione nella cache in uso e non inferiore. Una versione diversa memorizzati nella cache della visualizzazione dei dettagli viene creata per ogni versione del parametro Id.

È possibile impostare la proprietà VaryByParam per i valori seguenti:

> \* = Creare una versione memorizzata nella cache diversa ogni volta che varia in un parametro di stringa di formato o la query.
> 
> None = mai creare diverse versioni memorizzate nella cache
> 
> Elenco di punti e virgola di parametri = crea diverse versioni memorizzate nella cache ogni volta che uno dei parametri di stringa di modulo o una query nell'elenco varia


#### <a name="creating-a-cache-profile"></a>Creazione di un profilo di Cache

In alternativa alla configurazione delle proprietà della cache di output modificando le proprietà del &lt;OutputCache&gt; attributo, è possibile creare un profilo della cache nel file di configurazione (Web. config) di web. Creazione di un profilo di cache nel file di configurazione web offre due vantaggi importanti.

In primo luogo, configurando la memorizzazione nella cache di output nel file di configurazione web, è possibile controllare come le azioni del controller memorizzano nella cache il contenuto in un'unica posizione centrale. È possibile creare un profilo di cache e applicare il profilo a diversi controller o le azioni del controller.

In secondo luogo, è possibile modificare il file di configurazione web senza ricompilare l'applicazione. Se è necessario disabilitare la memorizzazione nella cache per un'applicazione che è già stata distribuita nell'ambiente di produzione, è possibile modificare semplicemente i profili di cache definiti nel file di configurazione web. Verranno rilevate automaticamente e applicate le modifiche al file di configurazione web.

Ad esempio, il &lt;la memorizzazione nella cache&gt; sezione di configurazione web nel listato 6 definisce un profilo della cache denominato Cache1Hour. Il &lt;la memorizzazione nella cache&gt; sezione deve figurare all'interno di &lt;System. Web&gt; sezione di un file di configurazione web.

**Elenco 6 – sezione di memorizzazione nella cache per Web. config**

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

Il controller nel listato 7 viene illustrato come è possibile applicare il profilo Cache1Hour a un'azione del controller con la &lt;OutputCache&gt; attributo.

**Listing 7 – Controllers\ProfileController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

Se si richiama l'azione Index () esposto dal controller nel listato 7 contemporaneamente verrà restituito per 1 ora.

#### <a name="summary"></a>Riepilogo

La memorizzazione nella cache di output fornisce un metodo molto semplice di migliorare notevolmente le prestazioni delle applicazioni ASP.NET MVC. In questa esercitazione è stato descritto come utilizzare il &lt;OutputCache&gt; attributo per memorizzare nella cache l'output di azioni del controller. È stato inoltre descritto come modificare le proprietà del &lt;OutputCache&gt; attributo, ad esempio le proprietà di durata e VaryByParam per modificare il contenuto Ottiene memorizzato nella cache. Infine, è stato descritto come definire i profili di cache nel file di configurazione web.

> [!div class="step-by-step"]
> [Precedente](understanding-action-filters-vb.md)
> [Successivo](adding-dynamic-content-to-a-cached-page-vb.md)
