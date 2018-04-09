---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Caching | Microsoft Docs
author: microsoft
description: La comprensione della memorizzazione nella cache è importante per un'applicazione ASP.NET buone prestazioni. ASP.NET 1. x disponibili tre diverse opzioni per la memorizzazione nella cache; cache di output,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 90faaae75cc85585efa05e6e50eabe8c990d076e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="caching"></a>Memorizzazione nella cache
====================
by [Microsoft](https://github.com/microsoft)

> La comprensione della memorizzazione nella cache è importante per un'applicazione ASP.NET buone prestazioni. ASP.NET 1. x disponibili tre diverse opzioni per la memorizzazione nella cache; la memorizzazione nella cache di output, la memorizzazione nella cache di frammento e l'API della cache.


La comprensione della memorizzazione nella cache è importante per un'applicazione ASP.NET buone prestazioni. ASP.NET 1. x disponibili tre diverse opzioni per la memorizzazione nella cache; la memorizzazione nella cache di output, la memorizzazione nella cache di frammento e l'API della cache. ASP.NET 2.0 offre tutte e tre questi metodi, ma aggiunge alcune importanti funzionalità aggiuntive. Esistono diverse nuove dipendenze della cache e gli sviluppatori ora la possibilità di creare anche le dipendenze della cache personalizzato. La configurazione della memorizzazione nella cache è stata migliorata anche in modo significativo in ASP.NET 2.0.

## <a name="new-features"></a>Nuove funzionalità

## <a name="cache-profiles"></a>Profili della cache

Profili della cache consentono agli sviluppatori di definire impostazioni specifiche della cache che possono essere applicate a singole pagine. Ad esempio, se si dispone di alcune pagine che devono essere scaduti dalla cache dopo 12 ore, è possibile creare facilmente un profilo della cache che può essere applicato a tali pagine. Per aggiungere un nuovo profilo di cache, utilizzare il &lt;outputCacheSettings&gt; sezione nel file di configurazione. Ad esempio, di seguito è la configurazione di un profilo di cache denominata *twoday* che consente di configurare una durata della cache di 12 ore.

[!code-xml[Main](caching/samples/sample1.xml)]

Per applicare questo profilo di cache a una pagina specifica, utilizzare l'attributo della direttiva @ OutputCache CacheProfile, come illustrato di seguito:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dipendenze della Cache personalizzato

Gli sviluppatori di 1. x ASP.NET cried out per le dipendenze della cache personalizzato. In ASP.NET 1. x, la classe CacheDependency è sealed dagli sviluppatori possano derivare proprie classi da esso non consentiti. In ASP.NET 2.0, tale limitazione è stata rimossa e gli sviluppatori hanno la possibilità di sviluppare le proprie dipendenze della cache personalizzato. La classe CacheDependency consente la creazione di una dipendenza della cache personalizzato basata su file, directory o chiavi della cache.

Ad esempio, il codice seguente crea una nuova dipendenza della cache personalizzato in base a un file denominato stuff.xml situato nella radice dell'applicazione Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

In questo scenario, quando viene modificato il file stuff.xml, l'elemento memorizzato nella cache viene invalidata.

È anche possibile creare una dipendenza della cache personalizzato utilizzando le chiavi della cache. In questo modo, la rimozione della chiave di cache invaliderà i dati memorizzati nella cache. Questa condizione è illustrata nell'esempio che segue.

[!code-csharp[Main](caching/samples/sample4.cs)]

Per invalidare l'elemento che è stato inserito in precedenza, rimuovere semplicemente l'elemento che è stato inserito nella cache come la chiave di cache.

[!code-csharp[Main](caching/samples/sample5.cs)]

Si noti che la chiave dell'elemento che funge da chiave della cache deve essere lo stesso come il valore aggiunto per la matrice delle chiavi della cache.

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>Dipendenze della Cache SQL basate sul polling<em>(detto anche le dipendenze basate su tabella)</em>

SQL Server 7 e 2000 utilizzare il modello di polling per le dipendenze della cache SQL. Il modello di polling viene utilizzato un trigger in una tabella di database che viene attivata quando si modifica dei dati nella tabella. Che devono attivare aggiornamenti un **changeId** nella tabella che ASP.NET verifica periodicamente la notifica. Se il **changeId** campo è stato aggiornato, ASP.NET riconosce che i dati sono stati modificati e non invalida i dati memorizzati nella cache.

> [!NOTE]
> SQL Server 2005 consente inoltre il modello di polling, ma poiché il modello di polling non è il modello più efficiente, è consigliabile utilizzare un modello basato su query (illustrato più avanti) con SQL Server 2005.


Affinché una dipendenza della cache SQL utilizzando il modello di polling per funzionare correttamente, le tabelle devono avere abilitate le notifiche. Questa operazione può essere eseguita a livello di codice utilizzando la classe SqlCacheDependencyAdmin o utilizzando aspnet\_regsql.exe utilità.

La seguente riga di comando registra la tabella Products del database Northwind si trova in un'istanza di SQL Server denominata *dbase* per SQL della dipendenza dalla cache.

[!code-console[Main](caching/samples/sample6.cmd)]

Di seguito è riportato una spiegazione delle opzioni della riga di comando utilizzate nel comando precedente:

| **Opzione della riga di comando** | **Scopo** |
| --- | --- |
| -S *server* | Specifica il nome del server. |
| -ed | Specifica che il database deve essere abilitato per la dipendenza della cache SQL. |
| -d *database\_nome* | Specifica il nome del database che deve essere abilitato per la dipendenza della cache SQL. |
| -E | Specifica che aspnet\_regsql deve utilizzare l'autenticazione di Windows quando ci si connette al database. |
| -et | Specifica che ci stiamo abilitazione di una tabella di database per la dipendenza della cache SQL. |
| -t *tabella\_nome* | Specifica il nome della tabella di database per abilitare per la dipendenza della cache SQL. |

> [!NOTE]
> Sono disponibili altre opzioni per aspnet\_regsql.exe. Per un elenco completo, eseguire aspnet\_regsql.exe-? dalla riga di comando.


Quando viene eseguito questo comando vengono apportate le seguenti modifiche al database di SQL Server:

- Un **AspNet\_SqlCacheTablesForChangeNotification** tabella viene aggiunta. Questa tabella contiene una riga per ogni tabella nel database per cui è stata abilitata una dipendenza della cache SQL.
- Le stored procedure seguenti vengono create all'interno del database:


| AspNet\_SqlCachePollingStoredProcedure | Esegue una query AspNet\_SqlCacheTablesForChangeNotification tabella e restituisce tutte le tabelle abilitate per la dipendenza della cache SQL e il valore di changeId per ogni tabella. Questa stored procedure viene utilizzata per il polling per determinare se i dati sono stati modificati. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Restituisce tutte le tabelle abilitate per la dipendenza della cache SQL, l'esecuzione di query AspNet\_SqlCacheTablesForChangeNotification tabella e restituisce tutte le tabelle abilitate per SQL della dipendenza dalla cache. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Registra una tabella per la dipendenza della cache SQL aggiungendo la voce necessaria nella tabella di notifica e aggiunge il trigger. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Annulla la registrazione di una tabella per la dipendenza della cache SQL rimuovendo la voce nella tabella di notifica e rimuove il trigger. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Aggiorna la tabella delle notifiche mediante l'incremento di changeId per la tabella modificata. ASP.NET utilizza questo valore per determinare se i dati sono stati modificati. Come indicato di seguito, questa stored procedure viene eseguita dal trigger creato quando la tabella è abilitata. |


- Chiamata di un trigger di SQL Server ***tabella\_nome *\_AspNet\_SqlCacheNotification\_Trigger** viene creato per la tabella. Questo trigger venga eseguito AspNet\_SqlCacheUpdateChangeIdStoredProcedure quando viene eseguita un'istruzione INSERT, UPDATE o DELETE nella tabella.
- Un ruolo del Server SQL denominato **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** viene aggiunto al database.

Il **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** ruolo di SQL Server dispone di autorizzazioni EXEC per AspNet\_SqlCachePollingStoredProcedure. Affinché il modello di polling funzionare correttamente, è necessario aggiungere l'account di processo aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess ruolo. Aspnet\_regsql.exe strumento non verrà eseguita automaticamente.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configurazione delle dipendenze della Cache SQL basate sul Polling

Esistono diversi passaggi necessari per la configurazione basate sul polling dipendenze della cache SQL. Il primo passaggio consiste per abilitare il database e la tabella, come illustrato in precedenza. Una volta completato questo passaggio, il resto della configurazione è come segue:

- Configurazione del file di configurazione ASP.NET.
- Configurazione SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Configurazione del File di configurazione ASP.NET

Oltre ad aggiungere una stringa di connessione come descritto in un modulo precedente, è necessario configurare anche un &lt;cache&gt; elemento con un &lt;sqlCacheDependency&gt; elemento come illustrato di seguito:

[!code-xml[Main](caching/samples/sample7.xml)]

Questa configurazione consente una dipendenza della cache SQL nel *pubs* database. Si noti che l'attributo di pollTime il &lt;sqlCacheDependency&gt; elemento valore predefinito è impostata su 60.000 millisecondi o 1 minuto. (Questo valore non può essere inferiore a 500 millisecondi). In questo esempio, il &lt;aggiungere&gt; elemento viene aggiunto un nuovo database ed esegue l'override pollTime, impostarlo su 9000000 millisecondi.

#### <a name="configuring-the-sqlcachedependency"></a>Configurazione SqlCacheDependency

Il passaggio successivo consiste nel configurare SqlCacheDependency. Il modo più semplice per eseguire questa operazione è per specificare il valore per l'attributo SqlDependency nella direttiva @ Outcache come segue:

[!code-aspx[Main](caching/samples/sample8.aspx)]

Nella direttiva @ OutputCache precedente, una dipendenza della cache SQL è configurata per il *autori* tabella il *pubs* database. Possono essere configurate più dipendenze, separandoli con un punto e virgola come illustrato di seguito:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Un altro metodo di configurazione SqlCacheDependency è a livello di programmazione. Il codice seguente crea una nuova dipendenza della cache SQL nel *autori* tabella il *pubs* database.

[!code-csharp[Main](caching/samples/sample10.cs)]

Uno dei vantaggi a livello di codice, definire la dipendenza della cache SQL è che è possibile gestire tutte le eccezioni che potrebbero verificarsi. Se si tenta di definire una dipendenza della cache SQL per un database che non è stato abilitato per la notifica, ad esempio un **DatabaseNotEnabledForNotificationException** verrà generata l'eccezione. In tal caso, è possibile tentare di abilitare il database per le notifiche chiamando il **SqlCacheDependencyAdmin.EnableNotifications** metodo e passando il nome del database.

Analogamente, se si tenta di definire una dipendenza della cache SQL per una tabella che non è stata abilitata per la notifica, un **TableNotEnabledForNotificationException** verrà generata. È quindi possibile chiamare il **SqlCacheDependencyAdmin** metodo passando il nome del database e il nome di tabella.

Esempio di codice seguente viene illustrato come configurare correttamente la gestione delle eccezioni durante la configurazione di una dipendenza della cache SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Ulteriori informazioni: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dipendenze della Cache SQL basato su query (solo SQL Server 2005)

Quando si utilizza SQL Server 2005 per la dipendenza della cache SQL, il modello di polling non è più necessario. Se utilizzato con SQL Server 2005, le dipendenze della cache SQL comunicano direttamente tramite le connessioni SQL all'istanza di SQL Server (è necessaria alcuna configurazione aggiuntiva) tramite le notifiche delle query di SQL Server 2005.

È il modo più semplice per abilitare la notifica basata su query per eseguire in modo dichiarativo mediante l'impostazione di **SqlCacheDependency** attributo dell'oggetto origine dati da **CommandNotification** e l'impostazione di **EnableCaching** attributo **true**. In questo modo, è necessario alcun codice. Se il risultato di un comando eseguito per i dati delle modifiche di origine, invalida i dati della cache.

Nell'esempio seguente consente di configurare un controllo origine dati per la dipendenza della cache SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

In questo caso, se la query specificata nel **SelectCommand** restituisce un risultato diverso da è stato originariamente, i risultati vengono memorizzati nella cache vengono invalidati.

È inoltre possibile specificare che tutte le origini dati sia attivata per le dipendenze della cache SQL impostando il **SqlDependency** attributo del **@ OutputCache** direttiva **CommandNotification** . Questa condizione è illustrata nell'esempio seguente.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Per ulteriori informazioni sulle notifiche di query in SQL Server 2005, vedere la documentazione in linea di SQL Server.


Un altro metodo di configurazione di una dipendenza della cache SQL basato su query è eseguire questa operazione a livello di codice utilizzando la classe SqlCacheDependency. Esempio di codice riportato di seguito viene illustrato come eseguire questa operazione.

[!code-csharp[Main](caching/samples/sample14.cs)]

Ulteriori informazioni: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Sostituzione post-Cache

La memorizzazione nella cache di una pagina può aumentare notevolmente le prestazioni di un'applicazione Web. In alcuni casi è necessario, tuttavia, la maggior parte della pagina da memorizzare nella cache e alcuni frammenti all'interno della pagina dinamica. Ad esempio, se si crea una pagina di notizie completamente statica per impostare periodi di tempo, è possibile impostare l'intera pagina da memorizzare nella cache. Se si desidera includere un banner pubblicitario a rotazione modificata a ogni richiesta di pagina, la parte della pagina contenente l'annuncio deve essere dinamici. Per consentire di memorizzare nella cache di una pagina, ma sostituire alcuni contenuti in modo dinamico, è possibile utilizzare la sostituzione post-cache ASP.NET. Con la sostituzione post-cache, l'intera pagina è nella cache con parti specifiche contrassegnate come esente dalla memorizzazione nella cache di output. Nell'esempio di pubblicitari, il controllo AdRotator consente di sfruttare la sostituzione post-cache in modo che annunci creata in modo dinamico per ogni utente e per ogni aggiornamento della pagina.

Esistono tre modi per implementare la sostituzione post-cache:

- In modo dichiarativo, utilizzando il controllo di sostituzione.
- Livello di programmazione utilizzando le API di controllo di sostituzione.
- In modo implicito, utilizzando il controllo AdRotator.

### <a name="substitution-control"></a>Controllo Substitution

Il controllo Substitution ASP.NET specifica una sezione di una pagina memorizzata nella cache che viene creata in modo dinamico anziché memorizzati nella cache. Inserire un controllo Substitution in corrispondenza della posizione della pagina in cui si desidera visualizzare il contenuto dinamico. In fase di esecuzione, il controllo Substitution chiama un metodo specificato con la proprietà MethodName. Il metodo deve restituire una stringa, che quindi sostituisce il contenuto del controllo di sostituzione. Il metodo deve essere un metodo statico del controllo contenitore Page o UserControl. Utilizzo del controllo di sostituzione comporta cache sul lato client per cache di server, in modo che la pagina non verrà memorizzata nel client. In questo modo si garantisce che le richieste successive di pagina di chiamare il metodo per generare il contenuto dinamico.

### <a name="substitution-api"></a>API di sostituzione

Per creare contenuto dinamico per una pagina memorizzata nella cache a livello di codice, è possibile chiamare il [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) metodo nel codice della pagina, passando il nome di un metodo come parametro. Il metodo che gestisce la creazione del contenuto dinamico accetta un singolo [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) parametro e restituisce una stringa. La stringa restituita è il contenuto che verrà sostituito nella posizione specificata. La chiamata al metodo WriteSubstitution anziché il controllo di sostituzione in modo dichiarativo un vantaggio è che è possibile chiamare un metodo di qualsiasi oggetto arbitrario, anziché chiamare un metodo statico della pagina o l'oggetto UserControl.

La chiamata al metodo WriteSubstitution causa cache sul lato client per cache di server, in modo che la pagina non verrà memorizzata nel client. In questo modo si garantisce che le richieste successive di pagina di chiamare il metodo per generare il contenuto dinamico.

### <a name="adrotator-control"></a>Controllo AdRotator

AdRotator controllo server implementa il supporto per la sostituzione post-cache internamente. Se si inserisce un controllo AdRotator nella pagina, verrà eseguito il rendering di annunci univoci per ogni richiesta, indipendentemente dal fatto che viene memorizzata nella cache della pagina padre. Di conseguenza, una pagina che include un controllo AdRotator viene solo memorizzata nella cache sul lato server.

## <a name="controlcachepolicy-class"></a>Classe ControlCachePolicy

La classe ControlCachePolicy consente di controllare a livello di codice del frammento di memorizzazione nella cache utilizzando i controlli utente. ASP.NET consente di incorporare controlli utente all'interno di un [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) istanza. La classe BasePartialCachingControl rappresenta un controllo utente è abilitata la cache di output.

Quando si accede di [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) proprietà di un [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) controllo, viene sempre restituito un oggetto ControlCachePolicy valido. Tuttavia, se si accede di [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) proprietà di un [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) controllo, viene restituito un oggetto ControlCachePolicy valido solo se il controllo utente è già stato eseguito il wrapping un Controllo BasePartialCachingControl. Se non viene incluso, l'oggetto ControlCachePolicy restituito dalla proprietà genererà eccezioni quando si tenta di modificarlo perché non dispone di un BasePartialCachingControl associato. Per determinare se un'istanza di UserControl supporta la memorizzazione nella cache senza la generazione di eccezioni, controllare il [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) proprietà.

Utilizzo della classe ControlCachePolicy è uno dei modi diversi, che è possibile abilitare la memorizzazione nella cache di output. L'elenco seguente descrive i metodi che è possibile utilizzare per abilitare la memorizzazione nella cache di output:

- Utilizzare il [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) direttiva per abilitare la memorizzazione nella cache in scenari dichiarativi di output.
- Utilizzare il [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) attributo per abilitare la memorizzazione nella cache per un controllo utente in un file code-behind.
- Utilizzare la classe ControlCachePolicy per specificare le impostazioni della cache in scenari a livello di codice in cui si lavora con BasePartialCachingControl istanze cache abilitata utilizzando uno dei metodi precedenti e caricata in modo dinamico mediante il [System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) metodo.

Un'istanza di ControlCachePolicy può essere modificata correttamente solo tra le fasi del ciclo di vita di controllo di Init e PreRender. Se si modifica un oggetto ControlCachePolicy dopo la fase PreRender, ASP.NET genera un'eccezione in quanto le modifiche apportate dopo il rendering del controllo non possono influire sulle impostazioni della cache (un controllo viene memorizzato nella cache durante la fase di rendering). Infine, un'istanza del controllo utente (e pertanto il relativo oggetto ControlCachePolicy) è disponibile solo per la modifica a livello di codice quando il rendering effettivo.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Modifiche alla configurazione, la memorizzazione nella cache il &lt;la memorizzazione nella cache&gt; elemento

Esistono numerose modifiche alla configurazione di memorizzazione nella cache in ASP.NET 2.0. Il &lt;la memorizzazione nella cache&gt; elemento è stato introdotto in ASP.NET 2.0 e consente di apportare modifiche alla configurazione di memorizzazione nella cache nel file di configurazione. Gli attributi seguenti sono disponibili.

| **Elemento** | **Descrizione** |
| --- | --- |
| **cache** | Elemento facoltativo. Definisce le impostazioni della cache globale dell'applicazione. |
| **outputCache** | Elemento facoltativo. Specifica le impostazioni della cache di output a livello di applicazione. |
| **outputCacheSettings** | Elemento facoltativo. Specifica le impostazioni della cache di output che possono essere applicate alle pagine dell'applicazione. |
| **sqlCacheDependency** | Elemento facoltativo. Consente di configurare le dipendenze della cache SQL per un'applicazione ASP.NET. |

### <a name="the-ltcachegt-element"></a>Il &lt;cache&gt; elemento

Gli attributi seguenti sono disponibili nel &lt;cache&gt; elemento:

| **Attributo** | **Descrizione** |
| --- | --- |
| **disableMemoryCollection** | Parametro facoltativo **booleano** attributo. Ottiene o imposta un valore che indica se la raccolta di memoria cache che si verifica quando il computer è eccessivo della memoria è disabilitata. |
| **disableExpiration** | Parametro facoltativo **booleano** attributo. Ottiene o imposta un valore che indica se la scadenza della cache è disabilitata. Se disabilitata, gli elementi memorizzati nella cache non scadono e lo scavenging in background di elementi della cache scaduti non vengono generati. |
| **privateBytesLimit** | Parametro facoltativo **Int64** attributo. Ottiene o imposta un valore che indica la dimensione massima dei byte privati di un'applicazione prima di inizia a scaricare la cache di elementi scaduti e il tentativo di recuperare memoria. Questo limite include sia memoria utilizzata dalla cache come normale sovraccarico di memoria dell'applicazione in esecuzione. Un'impostazione zero indica che ASP.NET utilizzerà le regole euristiche per determinare quando avviare il recupero della memoria. |
| **percentagePhysicalMemoryUsedLimit** | Parametro facoltativo **Int32** attributo. Ottiene o imposta un valore che indica la percentuale massima di memoria fisica di un computer che può essere utilizzata da un'applicazione prima di inizia a scaricare la cache di elementi scaduti e il tentativo di recuperare memoria che questo utilizzo della memoria include entrambi memoria utilizzata dalla cache anche come l'utilizzo della memoria normale dell'applicazione in esecuzione. Un'impostazione zero indica che ASP.NET utilizzerà le regole euristiche per determinare quando avviare il recupero della memoria. |
| **privateBytesPollTime** | Parametro facoltativo **TimeSpan** attributo. Ottiene o imposta un valore che indica l'intervallo di tempo di polling per l'utilizzo della memoria di byte privati dell'applicazione. |

### <a name="the-ltoutputcachegt-element"></a>Il &lt;outputCache&gt; elemento

Gli attributi seguenti sono disponibili per il &lt;outputCache&gt; elemento.


|       <strong>Attributo</strong>        |                                                                                                                                                                                                                                                       <strong>Descrizione</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Parametro facoltativo <strong>booleano</strong> attributo. Abilita o disabilita la cache di output della pagina. Se disabilitato, non le pagine vengono memorizzati nella cache, indipendentemente dalle impostazioni a livello di codice o dichiarative. Valore predefinito è <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Parametro facoltativo <strong>booleano</strong> attributo. Abilita o disabilita la cache dei frammenti di applicazione. Se disabilitato, non le pagine vengono memorizzati nella cache indipendentemente la [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) direttiva o la memorizzazione nella cache del profilo utilizzato. Include un'intestazione cache-control che indica che i server proxy upstream, così come i client del browser non devono tentare di output delle pagine della cache. Valore predefinito è <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Parametro facoltativo <strong>booleano</strong> attributo. Ottiene o imposta un valore che indica se il <strong>cache-controllo: private</strong> intestazione viene inviata dal modulo di cache di output per impostazione predefinita. Valore predefinito è <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Parametro facoltativo <strong>booleano</strong> attributo. Abilita/disabilita l'invio di un Http "<strong>Vary: \</ strong ><em>" intestazione nella risposta. Con l'impostazione predefinita false, un "</em>* Vary: \* <strong>" intestazione viene inviata per le pagine nella cache di output. Quando viene inviato l'intestazione Vary, consente di diversa versioni da memorizzare nella cache in base alle quali è specificato nell'intestazione Vary. Ad esempio <em>Vary: utente-agenti</em> archivierà versioni diverse di una pagina in base all'agente utente crea la richiesta. Valore predefinito è * * false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>Il &lt;outputCacheSettings&gt; elemento

Il &lt;outputCacheSettings&gt; elemento consente la creazione dei profili di cache come descritto in precedenza. L'unico elemento figlio per il &lt;outputCacheSettings&gt; elemento è il &lt;outputCacheProfiles&gt; elemento per la configurazione di profili di cache.

### <a name="the-ltsqlcachedependencygt-element"></a>Il &lt;sqlCacheDependency&gt; elemento

Gli attributi seguenti sono disponibili per il &lt;sqlCacheDependency&gt; elemento.

| **Attributo** | **Descrizione** |
| --- | --- |
| **enabled** | Richiesto **booleano** attributo. Indica se le modifiche sono viene eseguito il polling per. |
| **pollTime** | Parametro facoltativo **Int32** attributo. Imposta la frequenza con cui SqlCacheDependency esegue il polling delle modifiche nella tabella di database. Questo valore corrisponde al numero di millisecondi tra due polling successivi. Non può essere impostata su minore di 500 millisecondi. Valore predefinito è 1 minuto. |

### <a name="more-information"></a>Altre informazioni

Non sussiste alcune informazioni aggiuntive che è necessario essere consapevoli sulla configurazione della cache.

- Se il limite di byte privati di processo di lavoro non è impostato, la cache verrà utilizzato uno dei seguenti limiti: 

    - x86 2 GB: 800 MB o 60% della RAM fisica, a seconda del valore è minore di
    - x86 3 GB: 1800 MB o 60% della RAM fisica, a seconda del valore è minore di
    - x 64: 1 terabyte o 60% della RAM fisica, a seconda del valore è minore di
- Se sia il processo di lavoro privata byte limitare e &lt;cache privateBytesLimit /&gt; sono impostati, la cache verrà utilizzato il valore minimo di due.
- Come in 1. x, è eliminare le voci della cache e chiamare GC. Raccolta per due motivi: 

    - È molto sta per raggiungere il limite di byte privati
    - La memoria disponibile è prossima o inferiore al 10%
- Puoi disabilitare trim e memorizzare nella cache per le condizioni di scarsa disponibilità di memoria impostando in modo efficace &lt;cache percentagePhysicalMemoryUseLimit /&gt; a 100.
- A differenza di 1. x, 2.0 sospenderà le chiamate di taglio e raccogliere se ultimo Garbage Collection. Raccolta non consente di ridurre le dimensioni degli heap gestito da più di 1% del limite di memoria (cache) o byte privati.

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Le dipendenze della Cache personalizzato

1. Creare un nuovo sito Web.
2. Aggiungere un nuovo file XML denominato cache.xml e salvarlo nella radice dell'applicazione Web.
3. Aggiungere il codice seguente alla pagina\_metodo nel code-behind di default.aspx carico: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Nella parte superiore di default.aspx in visualizzazione origine, aggiungere quanto segue: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Sfoglia Default.aspx. Qual è il tempo?
6. Aggiornare il browser. Qual è il tempo?
7. Aprire cache.xml e aggiungere il codice seguente: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Salvare cache.xml.
9. Aggiornare il browser. Qual è il tempo?
10. Spiegare perché il tempo di aggiornamento anziché visualizzare i valori precedentemente memorizzati nella cache:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratorio 2: Utilizzando le dipendenze della Cache basate sul Polling

Questa esercitazione viene utilizzato il progetto creato nel modulo precedente che consente di modificare i dati nel database Northwind tramite un controllo GridView e DetailsView.

1. Aprire il progetto in Visual Studio 2005.
2. Eseguire aspnet\_utilità regsql sul database Northwind per abilitare il database e la tabella Products. Utilizzare il comando seguente da un Visual Studio prompt dei comandi: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Aggiungere quanto segue al file Web. config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Aggiungere un nuovo Web Form denominato showdata.aspx.
5. Aggiungere la seguente direttiva outputcache @ showdata.aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Aggiungere il codice seguente alla pagina\_carico di showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Aggiungere un nuovo controllo SqlDataSource showdata.aspx e configurarlo per utilizzare la connessione al database Northwind. Fare clic su Avanti.
8. Selezionare le caselle di controllo ProductName e ProductID e fare clic su Avanti.
9. Fare clic su Fine.
10. Aggiungere un nuovo GridView showdata.aspx.
11. Scegliere SqlDataSource1 nell'elenco a discesa.
12. Salvare e visualizzare showdata.aspx. Annotare l'ora visualizzata.
