---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: La memorizzazione nella cache di dati all'avvio dell'applicazione (VB) | Documenti Microsoft
author: rick-anderson
description: "In qualsiasi applicazione Web alcuni dati vengono usati di frequente e alcuni dati verranno utilizzati raramente. È possibile migliorare le prestazioni dei nostri b applicazione ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: 06370e31d27aeab50e56e0b0b860aca7c3ad683b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-at-application-startup-vb"></a>Memorizzazione nella cache i dati all'avvio dell'applicazione (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica il PDF](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> In qualsiasi applicazione Web alcuni dati vengono usati di frequente e alcuni dati verranno utilizzati raramente. Caricando in anticipo i dati utilizzati frequentemente, una tecnica nota come migliorare le prestazioni dell'applicazione ASP.NET. Questa esercitazione viene illustrato uno degli approcci disponibili per il caricamento, caricare i dati nella cache all'avvio dell'applicazione.


## <a name="introduction"></a>Introduzione

Le due esercitazioni precedenti esaminato la memorizzazione nella cache di dati della presentazione e livelli di memorizzazione nella cache. In [la memorizzazione nella cache di dati con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md), abbiamo esaminato utilizzando s ObjectDataSource la memorizzazione nella cache le funzionalità per memorizzare i dati nel livello di presentazione. [La memorizzazione nella cache di dati nell'architettura](caching-data-in-the-architecture-vb.md) esaminata la memorizzazione nella cache in un livello separato, nuova memorizzazione nella cache. Entrambe queste esercitazioni utilizzate *caricamento reattivo* lavora con la cache dei dati. Con reattivo durante il caricamento, ogni volta che vengono richiesti i dati, il sistema controlla innanzitutto se si s nella cache. In caso contrario, acquisisce i dati all'origine, ad esempio il database e quindi lo archivia nella cache. Il vantaggio principale di caricamento reattivo è più semplice dell'implementazione. Uno degli svantaggi è le prestazioni non uniforme in tutte le richieste. Si supponga di una pagina che utilizza il livello di memorizzazione nella cache dall'esercitazione precedente per visualizzare informazioni sul prodotto. Quando questa pagina viene visitata per la prima volta o visitata per la prima volta dopo che i dati memorizzati nella cache sono stati eliminati a causa di limitazioni di memoria o la scadenza specificata che è stato raggiunto, i dati devono essere recuperati dal database. Pertanto, queste richieste di utenti richiederà più tempo rispetto le richieste di utenti che possono essere servite dalla cache.

*Caricamento proattivo* fornisce una strategia di gestione della cache alternativo che consente di ottenere le prestazioni nelle richieste caricando i dati memorizzati nella cache prima di esso necessari. In genere, il caricamento utilizza un processo che periodicamente verifica o viene informato quando si è verificato un aggiornamento dei dati sottostanti. Questo processo è quindi aggiorna la cache per mantenerla aggiornata. Attiva durante il caricamento è particolarmente utile se i dati sottostanti provengono da una connessione lenta al database, un servizio Web o un'altra origine dati particolarmente ridotte. Ma questo approccio per il caricamento è più difficile da implementare, poiché richiede la creazione, gestione e distribuzione di un processo per verificare le modifiche e aggiornare la cache.

Un altro tipo di caricamento proattivo e il tipo che è sarà esplorazione in questa esercitazione, il caricamento dei dati nella cache all'avvio dell'applicazione. Questo approccio è particolarmente utile per la memorizzazione nella cache i dati statici, ad esempio i record nelle tabelle di ricerca del database.

> [!NOTE]
> Per un approfondimento le differenze tra caricamento attivo e reattivo, nonché gli elenchi di vantaggi e svantaggi consigli di implementazione, vedere il [la gestione del contenuto di una Cache](https://msdn.microsoft.com/en-us/library/ms978503.aspx) sezione la [ Guida all'architettura di applicazioni per .NET Framework di memorizzazione nella cache](https://msdn.microsoft.com/en-us/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Passaggio 1: Determinare quali dati da memorizzare nella Cache all'avvio dell'applicazione

Gli esempi di memorizzazione nella cache tramite il caricamento reattivo che abbiamo esaminato nel lavoro due esercitazioni precedenti con i dati possono cambiare periodicamente e non richiedere exorbitantly per generare. Ma se i dati memorizzati nella cache non modifica mai, la scadenza utilizzata dal caricamento reattivo è superflua. Analogamente, se i dati nella cache richiedono un periodo di tempo eccessivo per generare, tali utenti le cui richieste trovare vuoto cache avranno resistere a un periodo di attesa lungo durante i dati sottostanti vengono recuperati. Prendere in considerazione la memorizzazione nella cache i dati statici e i dati che richiedono un tempo particolarmente lungo per generare all'avvio dell'applicazione.

Mentre i database dispongono di molti dinamico, i valori cambiano di frequente, la maggior parte hanno anche una notevole quantità di dati statici. Ad esempio, praticamente tutti i modelli di dati dispongono di una o più colonne che contengono un valore specifico da un set fisso di scelte. Oggetto `Patients` tabella di database potrebbe disporre di un `PrimaryLanguage` colonna il cui set di valori potrebbe essere in lingua inglese, spagnolo, francese, russo, giapponese e così via. Spesso, questi tipi di colonne vengono implementati utilizzando *tabelle di ricerca*. Invece di archiviare la stringa di lingua inglese o francese nel `Patients` tabella, una seconda tabella viene creata con, in genere, due colonne, un identificatore univoco e una stringa descrittiva - con un record per ogni possibile valore. Il `PrimaryLanguage` colonna il `Patients` tabella archivia l'identificatore univoco corrispondente nella tabella di ricerca. Nella figura 1, paziente lingua principale di John Doe s è l'inglese, mentre s Ed Johnson è russa.


![La tabella di lingue è una tabella di ricerca utilizzato dalla tabella pazienti](caching-data-at-application-startup-vb/_static/image1.png)

**Figura 1**: il `Languages` tabella è una tabella di ricerca utilizzato dal `Patients` tabella


L'interfaccia utente per la modifica o la creazione di un nuovo paziente includerà un elenco di riepilogo a discesa delle lingue consentite popolata dal record di `Languages` tabella. Senza la memorizzazione nella cache, ogni volta che questa interfaccia viene visitato il sistema deve eseguire una query di `Languages` tabella. Si tratta dispendiosa e necessaria in quanto i valori di tabella di ricerca cambiano molto raramente, mai.

È possibile memorizzare nella cache il `Languages` dati con le stesse tecniche di caricamento reattivo esaminate nelle esercitazioni precedenti. Caricamento reattivo, tuttavia, Usa una scadenza basati sul tempo, che non è necessaria per i dati della tabella statici della ricerca. Mentre la memorizzazione nella cache tramite il caricamento reattivo sarebbe migliore nessuna memorizzazione nella cache del tutto, l'approccio migliore sarebbe di caricare in modo proattivo i dati della tabella di ricerca nella cache all'avvio dell'applicazione.

In questa esercitazione verranno esaminati come dati di tabella di ricerca nella cache e altre informazioni statiche.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Passaggio 2: Esaminare i diversi modi per memorizzare nella Cache di dati

Informazioni possono essere a livello di programmazione memorizzati nella cache in un'applicazione ASP.NET tramite diversi approcci. Si sposta già spiegato come utilizzare la cache dei dati nelle esercitazioni precedenti. In alternativa, gli oggetti possono essere a livello di programmazione memorizzati nella cache utilizzando *membri statici* o *lo stato dell'applicazione*.

Quando si utilizza una classe, in genere la classe deve essere creata un'istanza prima che i relativi membri sono accessibili. Ad esempio, per richiamare un metodo da una delle classi in questo livello di logica di Business, è innanzitutto necessario creare un'istanza della classe:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Prima di è possibile richiamare *SomeMethod* o funzionano con *SomeProperty*, è innanzitutto necessario creare un'istanza della classe utilizzando il `New` (parola chiave). *SomeMethod* e *SomeProperty* sono associati a una particolare istanza. La durata di questi membri è legata alla durata dell'oggetto associato. *I membri statici*, d'altra parte, sono variabili, proprietà e metodi che vengono condivisi tra *tutti* istanze della classe e, di conseguenza, hanno una durata, purché la classe. I membri statici sono contrassegnati dalla parola chiave `Shared`.

Oltre ai membri statici, dati possono essere memorizzati nella cache dello stato dell'applicazione. Ogni applicazione ASP.NET gestisce una raccolta nome/valore che s condiviso tra tutti gli utenti e pagine dell'applicazione. Questa raccolta è possibile accedere tramite il [ `HttpContext` classe](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.aspx) s [ `Application` proprietà](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.application.aspx)e utilizzati da una classe code-behind di pagine ASP.NET come illustrato di seguito:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

La cache dei dati fornisce una quantità API più completa per la memorizzazione nella cache di dati, fornendo meccanismi per basate sul tempo e dipendenze oggetti scaduti nella priorità degli elementi della cache e così via. Con i membri statici e lo stato dell'applicazione, è necessario aggiungere manualmente tali funzionalità dallo sviluppatore della pagina. Quando la memorizzazione nella cache di dati all'avvio dell'applicazione per la durata dell'applicazione, tuttavia, i vantaggi di cache s dati sono opinabile. In questa esercitazione verrà esaminato il codice che Usa tutti e tre le tecniche per la memorizzazione nella cache i dati statici.

## <a name="step-3-caching-thesupplierstable-data"></a>Passaggio 3: La memorizzazione nella cache il`Suppliers`tabella dati

Tabelle di database è ve implementata data Northwind non includono tutte le tabelle di ricerca tradizionale. Le quattro DataTable implementati nel nostro DAL tutte le tabelle del modello i cui valori non sono statici. Anziché il tempo necessario per aggiungere un nuovo DataTable DAL quindi una nuova classe e metodi per il livello Business LOGIC, di spesa per questa esercitazione consente solo di s fingono che il `Suppliers` dati della tabella s sono statici. Di conseguenza, è Impossibile memorizzare nella cache dati all'avvio dell'applicazione.

Per iniziare, creare una nuova classe denominata `StaticCache.cs` nel `CL` cartella.


![Creare la classe StaticCache.vb nella cartella CL](caching-data-at-application-startup-vb/_static/image2.png)

**Figura 2**: creare il `StaticCache.vb` classe il `CL` cartella


È necessario aggiungere un metodo che carica i dati all'avvio nell'archivio di cache appropriata, nonché metodi che restituiscono dati da questa cache.


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

Il codice precedente utilizza una variabile membro statico, `suppliers`, per contenere i risultati del `SuppliersBLL` classe s `GetSuppliers()` , chiamato dal metodo, il `LoadStaticCache()` metodo. Il `LoadStaticCache()` metodo deve essere chiamata durante l'avvio dell'applicazione s. Una volta questi dati sono stati caricati all'avvio dell'applicazione, è possono chiamare tutte le pagine che devono essere utilizzato con i dati fornitore di `StaticCache` classe s `GetSuppliers()` metodo. Pertanto, la chiamata al database per ottenere i fornitori solo si verifica una volta all'avvio dell'applicazione.

Anziché utilizzare una variabile membro statica come archivio della cache, sarebbe stato in alternativa possibile utilizzare lo stato dell'applicazione o la cache dei dati. Il codice seguente illustra la classe retooled per utilizzare lo stato dell'applicazione:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

In `LoadStaticCache()`, informazioni sul fornitore viene archiviati nella variabile di applicazione *chiave*. È s restituito come tipo appropriato (`Northwind.SuppliersDataTable`) da `GetSuppliers()`. Mentre lo stato dell'applicazione sono accessibili nelle classi di codice delle pagine ASP.NET utilizzando `Application("key")`, nell'architettura di cui è necessario utilizzare `HttpContext.Current.Application("key")` per ottenere l'oggetto corrente `HttpContext`.

Analogamente, la cache dei dati può essere utilizzata come archivio della cache, come illustrato nel codice seguente:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Per aggiungere un elemento nella cache dati senza alcuna scadenza basati sul tempo, utilizzare il `System.Web.Caching.Cache.NoAbsoluteExpiration` e `System.Web.Caching.Cache.NoSlidingExpiration` valori come parametri di input. Questo overload specifico della cache dei dati s `Insert` metodo è stato selezionato in modo che è possibile specificare il *priorità* dell'elemento della cache. La priorità viene utilizzata per determinare quali elementi da eliminare dalla cache quando la memoria disponibile è insufficiente. Nell'esempio si utilizza la priorità `NotRemovable`, che assicura che questo elemento della cache ha vinto t viene eseguito lo scavenging.

> [!NOTE]
> Questo download esercitazione s implementa la `StaticCache` classe Usa l'approccio di variabile membro statico. Il codice per le tecniche di cache stato e i dati dell'applicazione è disponibile nei commenti nel file di classe.


## <a name="step-4-executing-code-at-application-startup"></a>Passaggio 4: Esecuzione di codice all'avvio dell'applicazione

Per eseguire il codice al primo avvio di un'applicazione web, è necessario creare un file speciale denominato `Global.asax`. Questo file può contenere i gestori eventi per l'applicazione-, sessione-, e gli eventi a livello di richiesta e è in questo caso in cui è possibile aggiungere codice che verrà eseguito ogni volta che viene avviata l'applicazione.

Aggiungi il `Global.asax` file alla directory radice s facendo clic sul nome del progetto di sito Web in Visual Studio s Esplora soluzioni e scegliendo Aggiungi nuovo elemento applicazione web. Nella finestra di dialogo Aggiungi nuovo elemento, selezionare il tipo di elemento di classe di applicazione globale e quindi fare clic sul pulsante Aggiungi.

> [!NOTE]
> Se si dispone già di un `Global.asax` file nel progetto, la classe di applicazione globale, tipo di elemento non verrà elencato nella finestra di dialogo Aggiungi nuovo elemento.


[![Aggiungere il File Global. asax a Directory radice di s dell'applicazione Web](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Figura 3**: aggiungere il `Global.asax` File dell'applicazione Web s Directory radice ([fare clic per visualizzare l'immagine ingrandita](caching-data-at-application-startup-vb/_static/image5.png))


Il valore predefinito `Global.asax` file modello include cinque metodi all'interno di un server-side `<script>` tag:

- **`Application_Start`**viene eseguita al primo avvio dell'applicazione web
- **`Application_End`**viene eseguito quando l'applicazione è in corso l'arresto
- **`Application_Error`**viene eseguito ogni volta che un'eccezione non gestita raggiunge l'applicazione
- **`Session_Start`**viene eseguito quando viene creata una nuova sessione
- **`Session_End`**viene eseguito quando una sessione è scaduta o abbandonata

Il `Application_Start` gestore eventi viene chiamato una sola volta durante un ciclo di vita dell'applicazione s. L'applicazione viene avviata la prima volta che il problema può verificarsi modificando il contenuto di una risorsa ASP.NET è richiesto dall'applicazione e continua l'esecuzione fino al riavvio dell'applicazione, il `/Bin` cartella, modifica `Global.asax`, modifica il contenuto nel `App_Code` cartella o la modifica di `Web.config` file, fra le altre cause. Fare riferimento a [ciclo di vita delle applicazioni ASP.NET](https://msdn.microsoft.com/en-us/library/ms178473.aspx) per informazioni più dettagliate sul ciclo di vita dell'applicazione.

Per queste esercitazioni è solo necessario aggiungere il codice di `Application_Start` metodo, pertanto è possibile rimuovere le altre. In `Application_Start`, chiamare semplicemente il `StaticCache` classe s `LoadStaticCache()` metodo, che verrà caricato e memorizzare nella cache le informazioni del fornitore:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

Sono disponibili tutte le che s è! All'avvio dell'applicazione, il `LoadStaticCache()` metodo acquisire informazioni sul fornitore da BLL e archiviarlo in una variabile membro statico (o qualsiasi cache archiviare si finisce utilizzando la `StaticCache` classe). Per verificare questo comportamento, impostare un punto di interruzione nel `Application_Start` metodo ed eseguire l'applicazione. Si noti che il punto di interruzione viene raggiunto dopo l'avvio dell'applicazione. Tuttavia, le richieste successive, non causano il `Application_Start` metodo da eseguire.


[![Utilizzare un punto di interruzione per verificare che il gestore dell'evento Application_Start sia in esecuzione](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Figura 4**: utilizzare un punto di interruzione per verificare che il `Application_Start` gestore eventi è in esecuzione ([fare clic per visualizzare l'immagine ingrandita](caching-data-at-application-startup-vb/_static/image8.png))


> [!NOTE]
> Se non si raggiunge il `Application_Start` punto di interruzione quando si inizia il debug, è perché l'applicazione ha già avviato. Forzare l'applicazione di riavviare modificando il `Global.asax` o `Web.config` file, quindi riprovare. È possibile semplicemente aggiungere (o rimuovere) una riga vuota alla fine di uno di questi file riavviare rapidamente l'applicazione.


## <a name="step-5-displaying-the-cached-data"></a>Passaggio 5: Visualizzazione dei dati memorizzati nella cache

A questo punto il `StaticCache` classe dispone di una versione dei dati fornitore memorizzati nella cache all'avvio dell'applicazione che è possibile accedere tramite il relativo `GetSuppliers()` metodo. Per lavorare con i dati dal livello di presentazione, è possibile utilizzare un ObjectDataSource o richiamare a livello di codice il `StaticCache` classe s `GetSuppliers()` metodo da una classe code-behind di pagine ASP.NET. Consente di esaminare utilizzando i controlli ObjectDataSource e GridView per visualizzare le informazioni memorizzate nella cache fornitore s.

Aprire il `AtApplicationStartup.aspx` nella pagina di `Caching` cartella. Trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` proprietà `Suppliers`. Successivamente, dal controllo GridView smart tag di s scegliere di creare un nuovo oggetto ObjectDataSource denominato `SuppliersCachedDataSource`. Configurare ObjectDataSource per utilizzare il `StaticCache` classe s `GetSuppliers()` metodo.


[![Configurare ObjectDataSource per utilizzare la classe StaticCache](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Figura 5**: ObjectDataSource consente di configurare il `StaticCache` classe ([fare clic per visualizzare l'immagine ingrandita](caching-data-at-application-startup-vb/_static/image11.png))


[![Utilizzare il metodo GetSuppliers() per recuperare i dati memorizzati nella cache del fornitore](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Figura 6**: utilizzo di `GetSuppliers()` metodo per recuperare i dati fornitore memorizzati nella cache ([fare clic per visualizzare l'immagine ingrandita](caching-data-at-application-startup-vb/_static/image14.png))


Dopo aver completato la procedura guidata, Visual Studio aggiungerà automaticamente BoundField per ognuno dei campi dati `SuppliersDataTable`. Il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Figura 7 mostra la pagina quando viene visualizzato tramite un browser. L'output è lo stesso è fossero stati estratti i dati da s BLL `SuppliersBLL` classe, ma utilizza il `StaticCache` classe restituisce i dati fornitore come memorizzati nella cache all'avvio dell'applicazione. È possibile impostare i punti di interruzione nel `StaticCache` classe s `GetSuppliers()` metodo per verificare questo comportamento.


[![I dati fornitore memorizzati nella cache viene visualizzato in un controllo GridView.](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Figura 7**: dei dati fornitore memorizzati nella cache viene visualizzato in un controllo GridView ([fare clic per visualizzare l'immagine ingrandita](caching-data-at-application-startup-vb/_static/image17.png))


## <a name="summary"></a>Riepilogo

La maggior parte delle ogni modello di dati contiene una grande quantità di dati statici, in genere implementati sotto forma di tabelle di ricerca. Poiché queste informazioni sono statiche, qui s non è necessario accedere continuamente al database ogni volta che queste informazioni devono essere visualizzati. Inoltre, a causa di sua natura statica, quando la memorizzazione nella cache i dati esiste s non è necessaria una scadenza. In questa esercitazione è stato illustrato come eseguire tali dati e memorizzare nella cache nella cache dei dati, lo stato dell'applicazione e tramite una variabile membro statico. Questa informazione viene memorizzata nella cache all'avvio dell'applicazione e rimane nella cache per tutta la durata di s applicazione.

In questa esercitazione e le ultime due, si va esaminato la memorizzazione nella cache di dati per la durata della durata s dell'applicazione, nonché tramite basati sul tempo oggetti scaduti nella. Quando la memorizzazione nella cache dei dati del database, tuttavia, una scadenza basati sul tempo potrebbe essere minore di ideale. Anziché periodicamente svuotamento della cache, sarebbe ottimale per rimuovere solo l'elemento memorizzato nella cache quando vengono modificati i dati del database sottostante. L'ideale è possibile tramite l'utilizzo di dipendenze della cache SQL, che verrà esaminato nell'esercitazione successiva.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Teresa Murphy e Zack Jones. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](caching-data-in-the-architecture-vb.md)
[Successivo](using-sql-cache-dependencies-vb.md)
