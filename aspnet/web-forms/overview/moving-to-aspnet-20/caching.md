---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: La memorizzazione nella cache | Microsoft Docs
author: microsoft
description: La comprensione della memorizzazione nella cache è importante per un'applicazione ASP.NET con buone prestazioni. ASP.NET 1.x disponibili tre diverse opzioni per la memorizzazione nella cache; la memorizzazione dell'output,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 4f52a88680db54de6271b17bd52cbdace66425e9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387699"
---
<a name="caching"></a>Memorizzazione nella cache
====================
da [Microsoft](https://github.com/microsoft)

> La comprensione della memorizzazione nella cache è importante per un'applicazione ASP.NET con buone prestazioni. ASP.NET 1.x disponibili tre diverse opzioni per la memorizzazione nella cache; la memorizzazione nella cache di output, la memorizzazione nella cache di frammento e l'API della cache.


La comprensione della memorizzazione nella cache è importante per un'applicazione ASP.NET con buone prestazioni. ASP.NET 1.x disponibili tre diverse opzioni per la memorizzazione nella cache; la memorizzazione nella cache di output, la memorizzazione nella cache di frammento e l'API della cache. ASP.NET 2.0 offre tutte e tre questi metodi, ma aggiunge alcune funzionalità aggiuntive significativo. Esistono diverse nuove dipendenze della cache e gli sviluppatori ora hanno la possibilità di creare anche le dipendenze della cache personalizzate. La configurazione della memorizzazione nella cache è stata migliorata in modo significativo anche in ASP.NET 2.0.

## <a name="new-features"></a>Nuove funzionalità

## <a name="cache-profiles"></a>Profili della cache

I profili della cache consentono agli sviluppatori di definire impostazioni specifiche della cache che possono quindi essere applicate alle singole pagine. Ad esempio, se si dispone di alcune pagine che devono essere scaduti dalla cache dopo 12 ore, è possibile creare facilmente un profilo della cache che può essere applicato a tali pagine. Per aggiungere un nuovo profilo di cache, usare il &lt;outputCacheSettings&gt; sezione nel file di configurazione. Di seguito è ad esempio, la configurazione di un profilo di cache denominata *twoday* che consente di configurare una durata della cache di 12 ore.

[!code-xml[Main](caching/samples/sample1.xml)]

Per applicare questo profilo della cache in una pagina specifica, usare l'attributo della direttiva @ OutputCache CacheProfile come illustrato di seguito:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dipendenze della Cache personalizzate

Gli sviluppatori ASP.NET 1.x eseguito per le dipendenze della cache personalizzate. In ASP.NET 1.x, la classe CacheDependency è stato bloccato impediva dagli sviluppatori possano derivare proprie classi da esso. In ASP.NET 2.0, tale limitazione è stata rimossa e gli sviluppatori sono liberi di sviluppare le proprie dipendenze della cache personalizzate. La classe CacheDependency consente la creazione di una dipendenza della cache personalizzate basata su file, directory o le chiavi della cache.

Ad esempio, il codice seguente crea una nuova dipendenza della cache personalizzato basata su un file denominato stuff.xml situato nella radice dell'applicazione Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

In questo scenario, quando viene modificato il file stuff.xml, l'elemento memorizzato nella cache viene invalidata.

È anche possibile creare una dipendenza della cache personalizzato usando le chiavi della cache. In questo modo, la rimozione della chiave di cache invaliderà i dati memorizzati nella cache. Questa condizione è illustrata nell'esempio che segue.

[!code-csharp[Main](caching/samples/sample4.cs)]

Per invalidare l'elemento che è stato inserito in precedenza, è sufficiente rimuovere l'elemento che è stato inserito nella cache da usare come chiave di cache.

[!code-csharp[Main](caching/samples/sample5.cs)]

Si noti che la chiave dell'elemento che funge da chiave di cache deve essere lo stesso come il valore aggiunto nella matrice delle chiavi della cache.

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>Dipendenze della Cache di SQL basati su polling<em>(detto anche le dipendenze basate su tabella)</em>

SQL Server 7 e 2000 usano il modello basati su polling per le dipendenze della cache SQL. Il modello basato su polling Usa un trigger in una tabella di database che viene attivata quando cambiano i dati nella tabella. Che attivano gli aggiornamenti una **changeId** campo nella tabella di notifica che ASP.NET controlla periodicamente. Se il **changeId** campo è stato aggiornato, ASP.NET non saprà che i dati sono stati modificati e non invalida i dati memorizzati nella cache.

> [!NOTE]
> SQL Server 2005 possono anche usare il modello basato su polling, ma poiché il modello basato su polling non è il modello più efficiente, è consigliabile usare un modello basato su query (illustrato più avanti) con SQL Server 2005.


Affinché una dipendenza della cache SQL usando il modello basato su polling per funzionare correttamente, le tabelle devono avere abilitate le notifiche. A tale scopo a livello di codice usando la classe SqlCacheDependencyAdmin o utilizzando aspnet\_regsql.exe utilità.

La seguente riga di comando registra la tabella Products del database Northwind che si trova in un'istanza di SQL Server denominata *dbase* per SQL della dipendenza dalla cache.

[!code-console[Main](caching/samples/sample6.cmd)]

Di seguito è riportato una spiegazione delle opzioni della riga di comando utilizzate nel comando precedente:

| **Opzione della riga di comando** | **Scopo** |
| --- | --- |
| -S *server* | Specifica il nome del server. |
| -ed | Specifica che il database deve essere abilitato per la dipendenza della cache SQL. |
| -d *database\_nome* | Specifica il nome del database che deve essere abilitato per la dipendenza della cache SQL. |
| -E | Specifica che aspnet\_regsql deve utilizzare l'autenticazione di Windows quando ci si connette al database. |
| -et | Specifica che verrà abilitata una tabella di database per la dipendenza della cache SQL. |
| -t *tabella\_nome* | Specifica il nome della tabella di database da abilitare per la dipendenza della cache SQL. |

> [!NOTE]
> Sono disponibili altre opzioni per aspnet\_regsql.exe. Per un elenco completo, eseguire aspnet\_regsql.exe-? dalla riga di comando.


Quando questo comando esegue le seguenti modifiche sono apportate al database di SQL Server:

- Un' **AspNet\_SqlCacheTablesForChangeNotification** tabella viene aggiunta. Questa tabella contiene una riga per ogni tabella nel database per cui è stata abilitata una dipendenza della cache SQL.
- Le stored procedure seguenti vengono create all'interno del database:


| AspNet\_SqlCachePollingStoredProcedure | Esegue una query AspNet\_SqlCacheTablesForChangeNotification tabella e restituisce tutte le tabelle abilitate per la dipendenza della cache SQL e il valore di changeId per ogni tabella. Questa stored procedure viene utilizzata per il polling per determinare se i dati sono stati modificati. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Restituisce tutte le tabelle abilitate per la dipendenza della cache SQL eseguendo una query AspNet\_SqlCacheTablesForChangeNotification tabella e restituisce tutte le tabelle abilitate per SQL memorizzano nella cache delle dipendenze. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Registra una tabella per la dipendenza della cache SQL aggiungendo la voce necessaria nella tabella di notifica e aggiunge il trigger. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Annulla la registrazione di una tabella per la dipendenza della cache SQL rimuovendo la voce nella tabella di notifica e rimuove il trigger. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Aggiorna la tabella di notifica mediante l'incremento di changeId per la tabella modificata. ASP.NET utilizza questo valore per determinare se i dati sono stati modificati. Come indicato di seguito, questa stored procedure viene eseguita dal trigger creato quando la tabella è abilitata. |


- Un trigger di SQL Server denominato ***tabella\_nome *\_AspNet\_SqlCacheNotification\_Trigger** viene creato per la tabella. Questo trigger esegue AspNet\_SqlCacheUpdateChangeIdStoredProcedure quando un INSERT, UPDATE o DELETE viene eseguita sulla tabella.
- Un ruolo di SQL Server denominata **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** viene aggiunto al database.

Il **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** ruolo di SQL Server disponga delle autorizzazioni EXEC per AspNet\_SqlCachePollingStoredProcedure. Affinché il modello di polling funzionare correttamente, è necessario aggiungere l'account di processo per aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess ruolo. Aspnet\_strumento regsql.exe non eseguirà questa operazione automaticamente.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configurazione delle dipendenze della Cache SQL basati su Polling

Sono disponibili diversi passaggi necessari per la configurazione basata su polling dipendenze della cache SQL. Il primo passaggio sia per abilitare il database e la tabella, come illustrato in precedenza. Una volta completato questo passaggio, il resto della configurazione è come segue:

- Configurazione del file di configurazione ASP.NET.
- Configurazione di SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Configurazione del File di configurazione ASP.NET

Oltre ad aggiungere una stringa di connessione come illustrato in un modulo precedente, è necessario configurare anche un &lt;cache&gt; elemento con un &lt;sqlCacheDependency&gt; elemento, come illustrato di seguito:

[!code-xml[Main](caching/samples/sample7.xml)]

Questa configurazione consente una dipendenza della cache SQL on la *pubs* database. Si noti che il pollTime dell'attributo nel &lt;sqlCacheDependency&gt; elemento valore predefinito è 60000 millisecondi o di un minuto. (Questo valore non può essere minore di 500 millisecondi). In questo esempio, il &lt;aggiungere&gt; elemento viene aggiunto un nuovo database ed esegue l'override di pollTime, impostandola su 9000000 millisecondi.

#### <a name="configuring-the-sqlcachedependency"></a>Configurazione di SqlCacheDependency

Il passaggio successivo consiste nel configurare SqlCacheDependency. Il modo più semplice per eseguire questa operazione consiste nello specificare il valore dell'attributo SqlDependency nella direttiva @ Outcache come indicato di seguito:

[!code-aspx[Main](caching/samples/sample8.aspx)]

Nella direttiva @ OutputCache precedente, una dipendenza della cache SQL è configurata per il *autori* nella tabella di *pubs* database. Più dipendenze possono essere configurate, separandole con punti e virgola come segue:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Un altro metodo di configurazione di SqlCacheDependency consiste nell'eseguire questa operazione a livello di codice. Il codice seguente crea una nuova dipendenza della cache SQL sul *autori* nella tabella di *pubs* database.

[!code-csharp[Main](caching/samples/sample10.cs)]

Uno dei vantaggi offerti a livello di codice che definisce la dipendenza della cache SQL è che è possibile gestire tutte le eccezioni che potrebbero verificarsi. Ad esempio, se si tenta di definire una dipendenza della cache SQL per un database che non è stato abilitato per la notifica, un **DatabaseNotEnabledForNotificationException** verrà generata l'eccezione. In tal caso, è possibile provare ad abilitare il database per le notifiche chiamando il **SqlCacheDependencyAdmin.EnableNotifications** metodo e passando il nome del database.

Analogamente, se si tenta di definire una dipendenza della cache SQL per una tabella che non è stata abilitata per la notifica, un **TableNotEnabledForNotificationException** verrà generata. È quindi possibile chiamare il **SqlCacheDependencyAdmin** metodo e passargli il nome del database e il nome di tabella.

Esempio di codice seguente viene illustrato come configurare correttamente la gestione delle eccezioni durante la configurazione di una dipendenza della cache SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Altre informazioni: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dipendenze della Cache SQL basati su query (solo SQL Server 2005)

Quando si usa SQL Server 2005 per la dipendenza della cache SQL, il modello basato su polling non è più necessario. Se usato con SQL Server 2005, le dipendenze della cache SQL comunicano direttamente tramite le connessioni di SQL all'istanza di SQL Server (è necessaria alcuna ulteriore configurazione) tramite le notifiche delle query di SQL Server 2005.

È il modo più semplice per abilitare la notifica basata su query per eseguire in modo dichiarativo impostando il **SqlCacheDependency** attributo dell'oggetto origine dati da **CommandNotification** e impostando il **EnableCaching** dell'attributo **true**. In questo modo, è necessario alcun codice. Se il risultato di un comando eseguito sui dati di origine viene modificato, lo invaliderà i dati della cache.

L'esempio seguente configura un controllo origine dati per la dipendenza della cache SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

In questo caso, se la query specificata nel **SelectCommand** restituisce un risultato diverso da ha originariamente, i risultati memorizzati nella cache vengono invalidati.

È inoltre possibile specificare che tutte le origini dati siano abilitate per le dipendenze della cache SQL impostando il **SqlDependency** attributo il **@ OutputCache** direttiva per **CommandNotification** . Questa condizione è illustrata nell'esempio seguente.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Per altre informazioni sulle notifiche di query in SQL Server 2005, vedere la documentazione in linea di SQL Server.


Un altro metodo per la configurazione di una dipendenza della cache SQL basati su query consiste nell'eseguire questa operazione a livello di codice usando la classe SqlCacheDependency. Esempio di codice seguente viene illustrato come eseguire questa operazione.

[!code-csharp[Main](caching/samples/sample14.cs)]

Altre informazioni: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Sostituzione post-Cache

La memorizzazione nella cache una pagina può aumentare notevolmente le prestazioni di un'applicazione Web. Tuttavia, in alcuni casi è necessario la maggior parte della pagina da memorizzare nella cache e alcuni frammenti all'interno della pagina per essere dinamici. Ad esempio, se si crea una pagina di notizie che è interamente statica per determinati periodi di tempo, è possibile impostare l'intera pagina memorizzazione nella cache. Se si desidera includere un banner pubblicitario a rotazione modificata a ogni richiesta di pagina, quindi la parte della pagina contenente l'annuncio deve essere dinamica. Per consentire di memorizzare nella cache una pagina, ma sostituire alcuni contenuti in modo dinamico, è possibile usare la sostituzione post-cache ASP.NET. Con sostituzione post-cache, tutta la pagina è nella cache con parti specifiche contrassegnate come esente dalla memorizzazione nella cache di output. Nell'esempio di pubblicitari, il controllo AdRotator consente di sfruttare i vantaggi di sostituzione post-cache in modo che annunci creata in modo dinamico per ogni utente e per ogni aggiornamento della pagina.

Esistono tre modi per implementare sostituzione post-cache:

- In modo dichiarativo, tramite il controllo di sostituzione.
- A livello di codice usando l'API di controllo di sostituzione.
- In modo implicito, utilizzando il controllo AdRotator.

### <a name="substitution-control"></a>Controllo Substitution

Il controllo Substitution ASP.NET specifica una sezione di una pagina memorizzata nella cache che viene creata in modo dinamico anziché memorizzato nella cache. Inserire un controllo Substitution in corrispondenza della posizione nella pagina in cui si desidera visualizzare il contenuto dinamico. In fase di esecuzione, il controllo Substitution chiama un metodo specificato con la proprietà MethodName. Il metodo deve restituire una stringa che sostituisce quindi il contenuto del controllo sostituzione. Il metodo deve essere un metodo statico in controllo pagina o un elemento UserControl che lo contiene. Tramite il controllo substitution, nella cache lato client è passato alla cache di server, in modo che la pagina non verrà memorizzata nel client. Ciò garantisce che le richieste successive alla pagina di chiamare il metodo per generare il contenuto dinamico.

### <a name="substitution-api"></a>API di sostituzione

Per creare il contenuto dinamico per una pagina memorizzata nella cache a livello di codice, è possibile chiamare il [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) metodo nel codice della pagina, passando il nome di un metodo come parametro. Il metodo che gestisce la creazione del contenuto dinamico accetta un singolo [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) parametro e restituisce una stringa. La stringa restituita è il contenuto che verrà sostituito in corrispondenza della posizione specificata. Un vantaggio della chiamata al metodo WriteSubstitution invece di usare in modo dichiarativo il controllo Substitution è che è possibile chiamare un metodo di qualsiasi oggetto arbitrario, anziché chiamare un metodo statico della pagina o l'oggetto UserControl.

La chiamata al metodo WriteSubstitution, nella cache lato client è passato alla cache di server, in modo che la pagina non verrà memorizzata nel client. Ciò garantisce che le richieste successive alla pagina di chiamare il metodo per generare il contenuto dinamico.

### <a name="adrotator-control"></a>Controllo AdRotator

Il controllo AdRotator controllo server implementa il supporto per la sostituzione post-cache internamente. Se si inserisce un controllo AdRotator della pagina, verrà eseguito il rendering di annunci pubblicitari univoci per ogni richiesta, indipendentemente dal fatto che viene memorizzato nella cache della pagina padre. Di conseguenza, una pagina che include un controllo AdRotator viene solo memorizzata nella cache lato server.

## <a name="controlcachepolicy-class"></a>Classe ControlCachePolicy

La classe ControlCachePolicy consente di controllare a livello di codice del frammento di memorizzazione nella cache usando i controlli utente. ASP.NET consente di incorporare i controlli utente all'interno di un [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) istanza. La classe BasePartialCachingControl rappresenta un controllo utente che ha la memorizzazione dell'output abilitato.

Quando si accede il [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) proprietà di un [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) (controllo), si riceverà sempre un oggetto ControlCachePolicy valido. Tuttavia, se si accede il [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) proprietà di un [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) (controllo), viene restituito un oggetto ControlCachePolicy valido solo se il controllo utente è già stato eseguito il wrapping un Controllo BasePartialCachingControl. Se non ne viene eseguito il wrapping, l'oggetto ControlCachePolicy restituito dalla proprietà genererà le eccezioni quando si prova a modificarlo perché non dispone di un BasePartialCachingControl associato. Per determinare se un'istanza di UserControl supporta la memorizzazione nella cache senza generare eccezioni, controllare la [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) proprietà.

Utilizzo della classe ControlCachePolicy è uno dei diversi modi, che è possibile abilitare la memorizzazione nella cache di output. Nell'elenco seguente vengono descritti i metodi che è possibile usare per abilitare la memorizzazione nella cache di output:

- Usare la [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) direttiva per abilitare la memorizzazione dell'output in scenari dichiarativi.
- Usare la [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) attributo per abilitare la memorizzazione nella cache per un controllo utente in un file code-behind.
- Usare la classe ControlCachePolicy per specificare le impostazioni della cache in scenari a livello di codice in cui si lavora con le istanze di BasePartialCachingControl che sono stati abilitati cache usando uno dei metodi precedenti e caricati in modo dinamico usando il [System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) (metodo).

Un'istanza di ControlCachePolicy può essere modificata correttamente solo tra le fasi di Init e PreRender il controllo del ciclo di vita. Se si modifica un oggetto ControlCachePolicy dopo la fase di PreRender, ASP.NET genera un'eccezione perché tutte le modifiche apportate dopo il rendering del controllo non possono influire sulle impostazioni della cache (un controllo viene memorizzato nella cache durante la fase di rendering). Infine, un'istanza del controllo utente (e pertanto il relativo oggetto ControlCachePolicy) è disponibile per la manipolazione a livello di codice solo quando ne viene effettivamente eseguito il rendering.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Modifiche in configurazione memorizzazione nella cache - i &lt;memorizzazione nella cache&gt; elemento

Sono disponibili numerose modifiche alla configurazione di memorizzazione nella cache in ASP.NET 2.0. Il &lt;memorizzazione nella cache&gt; elemento è stato introdotto in ASP.NET 2.0, consente di apportare modifiche alla configurazione di memorizzazione nella cache nel file di configurazione. Gli attributi seguenti sono disponibili.

| **Elemento** | **Descrizione** |
| --- | --- |
| **cache** | Elemento facoltativo. Definisce le impostazioni della cache di applicazione globale. |
| **outputCache** | Elemento facoltativo. Specifica le impostazioni della cache di output a livello di applicazione. |
| **outputCacheSettings** | Elemento facoltativo. Specifica le impostazioni della cache di output che possono essere applicate alle pagine nell'applicazione. |
| **sqlCacheDependency** | Elemento facoltativo. Consente di configurare le dipendenze della cache SQL per un'applicazione ASP.NET. |

### <a name="the-ltcachegt-element"></a>Il &lt;cache&gt; elemento

Gli attributi seguenti sono disponibili nel &lt;cache&gt; elemento:

| **Attributo** | **Descrizione** |
| --- | --- |
| **disableMemoryCollection** | Facoltativo **booleana** attributo. Ottiene o imposta un valore che indica se la raccolta della memoria della cache che si verifica quando il computer è eccessivo della memoria è disabilitata. |
| **disableExpiration** | Facoltativo **booleana** attributo. Ottiene o imposta un valore che indica se la scadenza della cache è disabilitata. Se disabilitato, gli elementi memorizzati nella cache non scadono e non si verifica in background lo scavenging degli elementi nella cache scaduti. |
| **privateBytesLimit** | Facoltativo **Int64** attributo. Ottiene o imposta un valore che indica la dimensione massima di byte privati di un'applicazione prima di inizia a scaricare la cache scaduta elementi e si prova a recuperare memoria. Questo limite include memoria utilizzata dalla cache oltre a normali sovraccarico di memoria dell'applicazione in esecuzione. Un'impostazione pari a zero indica che ASP.NET utilizzerà le regole euristiche per determinare quando avviare il recupero della memoria. |
| **percentagePhysicalMemoryUsedLimit** | Facoltativo **Int32** attributo. Ottiene o imposta un valore che indica la percentuale massima di memoria fisica del computer che può essere utilizzata da un'applicazione prima di inizia a scaricare la cache scaduta elementi e si prova a recuperare memoria che questo utilizzo della memoria include sia la memoria utilizzata dalla cache anche come l'utilizzo della memoria normale dell'applicazione in esecuzione. Un'impostazione pari a zero indica che ASP.NET utilizzerà le regole euristiche per determinare quando avviare il recupero della memoria. |
| **privateBytesPollTime** | Facoltativo **TimeSpan** attributo. Ottiene o imposta un valore che indica l'intervallo di tempo di polling per l'utilizzo di memoria dei byte privati dell'applicazione. |

### <a name="the-ltoutputcachegt-element"></a>Il &lt;outputCache&gt; elemento

Gli attributi seguenti sono disponibili per il &lt;outputCache&gt; elemento.


|       <strong>Attributo</strong>        |                                                                                                                                                                                                                                                       <strong>Descrizione</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Facoltativo <strong>booleana</strong> attributo. Abilita o disabilita la cache di output di pagina. Se disabilitato, non le pagine vengono memorizzate nella cache indipendentemente dalle impostazioni a livello di codice o dichiarative. Valore predefinito è <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Facoltativo <strong>booleana</strong> attributo. Abilita o disabilita la cache di frammento dell'applicazione. Se disabilitato, non le pagine vengono memorizzati nella cache indipendentemente i [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) direttiva o la memorizzazione nella cache del profilo utilizzato. Include un'intestazione cache-control che indica che i server proxy a monte, nonché i client del browser non devono tentare di output delle pagine della cache. Valore predefinito è <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Facoltativo <strong>booleana</strong> attributo. Ottiene o imposta un valore che indica se il <strong>cache-controllo: private</strong> intestazione viene inviata dal modulo di cache di output per impostazione predefinita. Valore predefinito è <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Facoltativo <strong>booleana</strong> attributo. Abilita o disabilita l'invio di Http "<strong>Vary: \</ strong ><em>" intestazione nella risposta. Con l'impostazione predefinita false, un "</em>* possono variare: \* <strong>" intestazione viene inviata per le pagine nella cache di output. Quando viene inviata l'intestazione Vary, consente di diversa versioni da memorizzare nella cache basano su quanto specificato nell'intestazione Vary. Ad esempio, <em>Vary: utente-agenti</em> archivierà versioni diverse di una pagina in base all'agente utente crea la richiesta. Valore predefinito è * * false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>Il &lt;outputCacheSettings&gt; elemento

Il &lt;outputCacheSettings&gt; elemento consente la creazione di profili per la cache come descritto in precedenza. L'unico elemento figlio per il &lt;outputCacheSettings&gt; elemento è la &lt;outputCacheProfiles&gt; (elemento) per la configurazione della cache dei profili.

### <a name="the-ltsqlcachedependencygt-element"></a>Il &lt;sqlCacheDependency&gt; elemento

Gli attributi seguenti sono disponibili per il &lt;sqlCacheDependency&gt; elemento.

| **Attributo** | **Descrizione** |
| --- | --- |
| **enabled** | Obbligatorio **booleana** attributo. Indica se le modifiche vengono effettuato il polling per. |
| **pollTime** | Facoltativo **Int32** attributo. Imposta la frequenza con cui SqlCacheDependency esegue il polling delle modifiche nella tabella di database. Questo valore corrisponde al numero di millisecondi trascorsi tra due polling successivi. Non può essere impostata su meno di 500 millisecondi. Valore predefinito è 1 minuto. |

### <a name="more-information"></a>Altre informazioni

È alcune informazioni aggiuntive che è necessario essere consapevoli dei relativi alla configurazione della cache.

- Se il limite di byte privati di processo di lavoro non è impostato, verrà utilizzato dalla cache tra i limiti seguenti: 

    - x86 2 GB: 800 MB o al 60% della RAM fisica, a seconda del valore è minore di
    - x86 3 GB: 1800 MB o al 60% della RAM fisica, a seconda del valore è minore di
    - x 64: 1 TB o al 60% della RAM fisica, a seconda del valore è minore di
- Se sia il processo di lavoro privata byte limitano e &lt;memorizza nella cache privateBytesLimit /&gt; vengono impostate, il valore minimo tra i due verrà utilizzato dalla cache.
- Esattamente come nella versione 1.x, è eliminare le voci della cache e chiamare GC. Raccolta per due motivi: 

    - Siamo molto prossimo al limite di byte privati
    - La memoria disponibile è prossima o inferiore al 10%
- È effettivamente possibile disabilitare trim e memorizzare nella cache per condizione di memoria disponibile insufficiente impostando &lt;memorizza nella cache percentagePhysicalMemoryUseLimit /&gt; a 100.
- A differenza di 1.x, 2.0 sospenderà le chiamate di taglia e raccogliere se l'ultima Garbage Collection. Raccolta non consente di ridurre le dimensioni degli heap gestiti più di 1% del limite di memoria (cache) o byte privati.

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Dipendenze della Cache personalizzate

1. Creare un nuovo sito Web.
2. Aggiungere un nuovo file XML denominato cache.xml e salvarlo nella radice dell'applicazione Web.
3. Aggiungere il codice seguente alla pagina\_caricamento del metodo nel code-behind di default. aspx: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Nella parte superiore di default. aspx nella visualizzazione origine, aggiungere quanto segue: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Sfoglia default. aspx. Qual è l'ora?
6. Aggiornare il browser. Qual è l'ora?
7. Aprire cache.xml e aggiungere il codice seguente: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Salvare cache.xml.
9. Aggiornare il browser. Qual è l'ora?
10. Spiegare il motivo per cui l'ora dell'aggiornamento invece di visualizzare i valori precedentemente memorizzati nella cache:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratorio 2: Uso delle dipendenze della Cache basata su Polling

Questa esercitazione Usa il progetto creato nel modulo precedente che consente la modifica dei dati nel database Northwind tramite un controllo GridView e DetailsView.

1. Aprire il progetto in Visual Studio 2005.
2. Eseguire aspnet\_utilità regsql sul database Northwind per abilitare il database e la tabella Products. Usare il comando seguente da un Visual Studio prompt dei comandi: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Aggiungere quanto segue al file Web. config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Aggiungere un nuovo Web Form denominato showdata.aspx.
5. Aggiungere quanto segue @ outputcache (direttiva) alla pagina showdata.aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Aggiungere il codice seguente alla pagina\_carico di showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Aggiungere un nuovo controllo SqlDataSource per showdata.aspx e configurarlo per usare la connessione al database Northwind. Fare clic su Avanti.
8. Selezionare le caselle di controllo ProductID e ProductName e fare clic su Avanti.
9. Fare clic su Fine.
10. Aggiungere un controllo GridView di nuovo alla pagina showdata.aspx.
11. Scegliere SqlDataSource1 dall'elenco a discesa.
12. Salvare e Sfoglia showdata.aspx. Assicurarsi di annotare l'ora visualizzata.
