---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Configurazione e la strumentazione | Documenti Microsoft
author: microsoft
description: Sono disponibili le principali modifiche alla configurazione e strumentazione di ASP.NET 2.0. La nuova API di configurazione di ASP.NET consente di apportare modifiche di configurazione da apportare pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 16dfe3c899dfa028d8a52b4b5f9c2868887e8fa9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
---
<a name="configuration-and-instrumentation"></a>Configurazione e la strumentazione
====================
by [Microsoft](https://github.com/microsoft)

> Sono disponibili le principali modifiche alla configurazione e strumentazione di ASP.NET 2.0. La nuova API di configurazione di ASP.NET consente di apportare modifiche di configurazione da eseguire a livello di codice. Inoltre, molte nuove impostazioni di configurazione esistano Consenti per le nuove configurazioni e la strumentazione.


Sono disponibili le principali modifiche alla configurazione e strumentazione di ASP.NET 2.0. La nuova API di configurazione di ASP.NET consente di apportare modifiche di configurazione da eseguire a livello di codice. Inoltre, molte nuove impostazioni di configurazione esistano Consenti per le nuove configurazioni e la strumentazione.

In questo modulo, si parlerà dell'API di configurazione ASP.NET come si riferisce a lettura e scrittura ai file di configurazione ASP.NET, e verrà inoltre discussa strumentazione ASP.NET. Verranno inoltre illustrate le nuove funzionalità disponibili nella traccia di ASP.NET.

## <a name="aspnet-configuration-api"></a>API di configurazione ASP.NET

L'API di configurazione di ASP.NET consente di sviluppare, distribuire e gestire i dati di configurazione dell'applicazione utilizzando una singola interfaccia di programmazione. È possibile utilizzare l'API di configurazione per sviluppare e modificare a livello di programmazione di completare le configurazioni di ASP.NET senza modificare direttamente il codice XML nel file di configurazione. Inoltre, è possibile utilizzare l'API di configurazione in applicazioni console e script che si sviluppa, in strumenti di gestione basato su Web e negli snap-in Microsoft Management Console (MMC).

I due strumenti di gestione della configurazione seguente usano l'API di configurazione e sono inclusi in .NET Framework versione 2.0:

- Lo snap-in MMC di ASP.NET, che utilizza l'API di configurazione per semplificare le attività amministrative, fornendo una visualizzazione di dati di configurazione locale di tutti i livelli della gerarchia di configurazione integrata.
- Lo strumento Amministrazione sito Web, che consente di gestire le impostazioni di configurazione per le applicazioni locali e remote, inclusi i siti ospitati.

L'API di configurazione di ASP.NET è costituito da un set di oggetti di gestione di ASP.NET che è possibile utilizzare per configurare siti Web e applicazioni a livello di codice. Gli oggetti di gestione vengono implementati come una libreria di classi .NET Framework. Il modello di programmazione di API di configurazione consente di garantire la coerenza di codice e l'affidabilità applicando i tipi di dati in fase di compilazione. Per rendere più semplice gestire le configurazioni di applicazione, l'API di configurazione consente di visualizzare i dati uniti di tutti i punti della gerarchia di configurazione come una singola raccolta, anziché la visualizzazione dei dati come raccolte separate da diversi file di configurazione. Inoltre, l'API di configurazione consente di modificare le configurazioni di tutta l'applicazione senza modificare direttamente il codice XML nel file di configurazione. Infine, l'API semplifica le attività di configurazione tramite il supporto di strumenti di amministrazione, ad esempio lo strumento Amministrazione sito Web. L'API di configurazione semplifica la distribuzione per supportare la creazione di file di configurazione in un computer ed eseguire gli script di configurazione tra più computer.

> [!NOTE]
> L'API di configurazione non supporta la creazione di applicazioni IIS.


## <a name="working-with-local-and-remote-configuration-settings"></a>Utilizzo delle impostazioni di configurazione locale e remoto

Un oggetto di configurazione rappresenta la visualizzazione unita delle impostazioni di configurazione che si applicano a un'entità fisica specifica, ad esempio un computer, o a un'entità logica, ad esempio un'applicazione o un sito Web. L'entità logica specificata è disponibile nel computer locale o in un server remoto. Quando è presente alcun file di configurazione per un'entità specificata, l'oggetto di configurazione rappresenta le impostazioni di configurazione predefinite, come definito dal file Machine. config.

È possibile ottenere un oggetto di configurazione utilizzando uno dei metodi di configurazione aperti dalle classi seguenti:

1. La classe di ConfigurationManager, se l'entità è un'applicazione client.
2. La classe WebConfigurationManager, se l'entità è un'applicazione Web.

Questi metodi restituirà un oggetto di configurazione, che a sua volta fornisce le proprietà per gestire i file di configurazione sottostanti e i metodi necessari. È possibile accedere a questi file per la lettura o scrittura.

### <a name="reading"></a>Lettura

Utilizzare il metodo GetSection o GetSectionGroup per leggere le informazioni di configurazione. L'utente o processo che legge deve avere autorizzazioni di lettura in tutti i file di configurazione nella gerarchia di.

> [!NOTE]
> Se si utilizza un metodo GetSection statico che accetta un parametro di percorso, il parametro path deve fare riferimento all'applicazione in cui viene eseguito il codice. In caso contrario, il parametro viene ignorato e vengono restituite le informazioni di configurazione per l'applicazione attualmente in esecuzione.


### <a name="writing"></a>Scrittura

Utilizzare uno dei metodi di salvataggio per scrivere le informazioni di configurazione. L'utente o il processo di scrittura deve disporre delle autorizzazioni per il file di configurazione e la directory a livello di gerarchia di configurazione corrente di scrittura, nonché tutti i file di configurazione nella gerarchia di autorizzazioni di lettura.

Per generare un file di configurazione che rappresenta le impostazioni di configurazione ereditate per un'entità specificata, utilizzare uno dei seguenti metodi di configurazione di salvataggio:

1. Il metodo Save per creare un nuovo file di configurazione.
2. Il metodo SaveAs per generare un nuovo file di configurazione in un altro percorso.

## <a name="configuration-classes-and-namespaces"></a>Spazi dei nomi e le classi di configurazione

Numerosi metodi e classi di configurazione sono simili tra loro. La tabella seguente descrive le classi di configurazione utilizzate più di frequente e spazi dei nomi.

| **Spazio dei nomi o classe di configurazione** | **Descrizione** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) namespace | Contiene le classi di configurazione principale per tutte le applicazioni .NET Framework. Le classi dei gestori di sezione consentono di ottenere i dati di configurazione per una sezione da metodi quali GetSection e GetSectionGroup. Questi due metodi non sono statici. |
| Classe System.Configuration.Configuration | Rappresenta un set di dati di configurazione per un computer, applicazioni, directory Web o un'altra risorsa. Questa classe contiene metodi, ad esempio GetSection e GetSectionGroup, utili per l'aggiornamento delle impostazioni di configurazione e per ottenere riferimenti a sezioni e gruppi. Questa classe è utilizzata come tipo restituito per i metodi che ottengono i dati di configurazione design-time, ad esempio i metodi delle classi WebConfigurationManager e ConfigurationManager. |
| Spazio dei nomi System.Web.Configuration | Contiene le classi del gestore per le sezioni di configurazione di ASP.NET definiti [impostazioni di configurazione ASP.NET](https://msdn.microsoft.com/library/b5ysx397.aspx). Le classi dei gestori di sezione consentono di ottenere i dati di configurazione per una sezione da metodi quali GetSection e GetSectionGroup. |
| System.Web.Configuration.WebConfigurationManager class | Fornisce metodi utili per ottenere riferimenti a impostazioni di configurazione in fase di esecuzione e in fase di progettazione. Questi metodi usano la classe di System.Configuration.Configuration come un tipo restituito. È possibile utilizzare il metodo GetSection statico di questa classe o il metodo GetSection non statici della classe System.Configuration.ConfigurationManager in modo intercambiabile. Per le configurazioni di applicazioni Web, è consigliabile utilizzare la classe System.Web.Configuration.WebConfigurationManager anziché la classe System.Configuration.ConfigurationManager. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) namespace | Consente di personalizzare ed estendere il provider di configurazione. Questa è la classe base per tutte le classi provider nel sistema di configurazione. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) dello spazio dei nomi | Contiene classi e interfacce per la gestione e monitoraggio dell'integrità delle applicazioni Web. In teoria, questo spazio dei nomi non viene considerato parte dell'API di configurazione. Traccia e la generazione di eventi, ad esempio, viene eseguito tramite le classi nello spazio dei nomi. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) dello spazio dei nomi | Fornisce le classi necessarie per la strumentazione di applicazioni di esporre le informazioni di gestione e ai potenziali utenti eventi tramite Strumentazione gestione Windows (WMI). Il monitoraggio dello stato ASP.NET utilizza WMI per il recapito degli eventi. In teoria, questo spazio dei nomi non viene considerato parte dell'API di configurazione. |

## <a name="reading-from-aspnet-configuration-files"></a>Lettura dal file di configurazione ASP.NET

La classe WebConfigurationManager è la classe di base per la lettura da file di configurazione ASP.NET. Esistono sostanzialmente tre passaggi per la lettura dei file di configurazione di ASP.NET:

1. Ottenere un oggetto di configurazione utilizzando il metodo OpenWebConfiguration.
2. Ottenere un riferimento alla sezione desiderata nel file di configurazione.
3. Leggere le informazioni desiderate dal file di configurazione.

La configurazione di oggetto non rappresenta un file di configurazione specifico. Rappresenta invece una visualizzazione unita della configurazione di un computer, l'applicazione o sito Web. Esempio di codice seguente viene creata un'istanza di un oggetto di configurazione che rappresenta la configurazione di un'applicazione Web denominata *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Si noti che se il percorso di /ProductInfo non esiste, il codice precedente restituirà la configurazione predefinita, come specificato nel file Machine. config.


Dopo aver creato l'oggetto di configurazione, è quindi possibile utilizzare il metodo GetSection o GetSectionGroup per analizzare le impostazioni di configurazione. Nell'esempio seguente ottiene un riferimento per le impostazioni di rappresentazione per l'applicazione ProductInfo precedente:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Scrittura in file di configurazione ASP.NET

Come la lettura da file di configurazione, la classe WebConfigurationManager è la base per la scrittura in file di configurazione Asp.NET. Esistono tre passaggi per la scrittura in file di configurazione ASP.NET.

1. Ottenere un oggetto di configurazione utilizzando il metodo OpenWebConfiguration.
2. Ottenere un riferimento alla sezione desiderata nel file di configurazione.
3. Scrivere le informazioni desiderate dal file di configurazione utilizzando Salva o Salva con nome metodo.

Il codice seguente modifica il **debug** attributo del &lt;compilazione&gt; elemento su false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Quando viene eseguito questo codice, il **debug** attributo del &lt;compilazione&gt; elemento verrà impostato su false per il *webApp* file Web. config dell'applicazione.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

Lo spazio dei nomi System.Web.Management fornisce le classi e interfacce per la gestione e monitoraggio dell'integrità delle applicazioni ASP.NET.

L'operazione di registrazione viene eseguita mediante la definizione di una regola che associa un provider di eventi. La regola definisce il tipo di eventi che vengono inviati al provider. Gli eventi di base seguenti sono disponibili per consentire l'accesso:

| **WebBaseEvent** | La classe di evento di base per tutti gli eventi. Contiene le proprietà per tutti gli eventi, ad esempio di codice dell'evento, codice dettagliato dell'evento, data e ora dell'evento, numero di sequenza, il messaggio di evento e dettagli dell'evento. |
| --- | --- |
| **WebManagementEvent** | La classe di evento di base per gli eventi di gestione, ad esempio la durata dell'applicazione, richiesta, errore e gli eventi di controllo. |
| **WebHeartbeatEvent** | Evento generato dall'applicazione in base a intervalli regolari per acquisire informazioni sullo stato di runtime utile. |
| **WebAuditEvent** | La classe base per gli eventi di controllo di sicurezza, che vengono usati per contrassegnare le condizioni, ad esempio errore di autorizzazione, errore di decrittografia, *e così via.* |
| **WebRequestEvent** | Classe di base per tutti gli eventi informativi richiesta. |
| **WebBaseErrorEvent** | Classe di base per tutti gli eventi che indica le condizioni di errore. |

I tipi di provider disponibili consentono di inviare l'output di eventi in Visualizzatore eventi, SQL Server, Strumentazione gestione Windows (WMI) e messaggio di posta elettronica. I mapping di evento e il preconfigurati provider ridurre la quantità di lavoro necessaria per ottenere l'output dell'evento registrato.

ASP.NET 2.0 utilizza il registro eventi provider out-of-the-box per registrare gli eventi in base al dominio di applicazione, avvio e arresto, nonché la registrazione di eventuali eccezioni non gestite. Ciò consente di coprire alcuni degli scenari di base. Ad esempio, si supponga che l'applicazione genera un'eccezione, ma l'utente non viene salvato l'errore e non è possibile riprodurre. Con la regola predefinita del registro eventi è in grado di raccogliere le informazioni di eccezione e lo stack per farsi un'idea del tipo di errore si è verificato. Un altro esempio si applica se l'applicazione sta per perdere lo stato della sessione. In tal caso, è possibile esaminare nel registro eventi per determinare se il riciclo del dominio applicazione e la causa dell'arresto in primo luogo il dominio dell'applicazione.

Inoltre, l'integrità di sistema di monitoraggio è estensibile. Ad esempio, è possibile definire eventi personalizzati di Web attivarli all'interno dell'applicazione e quindi definire una regola per inviare le informazioni sull'evento a un provider, ad esempio la posta elettronica. Ciò consente di associare facilmente la strumentazione per il provider di monitoraggio dello stato. Ad esempio, è possibile generare un evento ogni volta che un ordine viene elaborato e impostare una regola che ogni evento viene inviato al database di SQL Server. È anche possibile generare un evento quando un utente non riesce ad accedere più volte in una riga e impostare l'evento da utilizzare i provider di posta elettronica.

La configurazione per gli eventi e i provider predefiniti viene archiviata nel file Web. config globale. Il file Web. config globale memorizza tutte le impostazioni basate sul Web che sono state archiviate nel file Machine. config in ASP.NET 1 x. Il file Web. config globale si trova nella directory seguente:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

Il &lt;healthMonitoring&gt; sezione del file Web. config globale sono disponibili impostazioni di configurazione predefinite. È possibile eseguire l'override di queste impostazioni o configurare impostazioni personalizzate implementando il &lt;healthMonitoring&gt; sezione nel file Web. config dell'applicazione.

Il &lt;healthMonitoring&gt; sezione del file Web. config globale contiene gli elementi seguenti:

| **providers** | Contiene i provider impostato per il Visualizzatore eventi, WMI e SQL Server. |
| --- | --- |
| **eventMappings** | Contiene i mapping per le varie classi WebBase. È possibile estendere questo elenco se si genera la propria classe di evento. Generazione di una classe di evento offre una maggiore granularità tramite i provider a che inviare informazioni. Ad esempio, è possibile configurare le eccezioni non gestite da inviare al Server SQL, durante l'invio di eventi personalizzati per posta elettronica. |
| **rules** | Collegamenti eventMappings al provider. |
| **buffering** | Utilizzato con i provider di posta elettronica e di SQL Server per determinare la frequenza con cui scaricare gli eventi per il provider. |

Di seguito è riportato un esempio di codice dal file Web. config globale.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Come archiviare gli eventi nel Visualizzatore eventi

Come accennato in precedenza, il provider per la registrazione degli eventi nel visualizzatore viene configurato automaticamente nel file Web. config globale. Per impostazione predefinita, tutti gli eventi basati su **WebBaseErrorEvent** e **WebFailureAuditEvent** vengono registrati. È possibile aggiungere regole aggiuntive per la registrazione di informazioni aggiuntive nel registro eventi. Ad esempio, se si desidera registrare tutti gli eventi (*ad esempio*, ogni evento in base alle **WebBaseEvent**), è possibile aggiungere la regola seguente al file Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Questa regola collega il **tutti gli eventi** mappa eventi per il provider del registro eventi. EventMapping sia il provider sono inclusi nel file Web. config globale.

## <a name="how-to-store-events-to-sql-server"></a>Come archiviare eventi per SQL Server

Questo metodo Usa il **ASPNETDB** database, viene generato da Aspnet\_regsql.exe strumento. Il provider predefinito utilizza la stringa di connessione LocalSqlServer, che utilizza un database basato su file nell'App\_cartella di dati o di istanza SQLExpress locale di SQL Server. La stringa di connessione LocalSqlServer sia il SqlProvider configurati nel file Web. config globale.

La stringa di connessione LocalSqlServer nel file Web. config globale è simile al seguente:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Se si desidera utilizzare un'altra istanza di SQL Server, è necessario utilizzare Aspnet\_regsql.exe. exe è reperibile nel % windir%\Microsoft.Net\Framework\v2.0.\* cartella. Utilizzare Aspnet\_regsql.exe strumento per generare un oggetto personalizzato **ASPNETDB** del database nell'istanza di SQL Server, quindi aggiungere la stringa di connessione nel file di configurazione di applicazioni e quindi aggiungere un provider tramite la nuova stringa di connessione. Dopo aver creato il **ASPNETDB** database creato, è necessario impostare una regola per collegare un eventMapping il sqlProvider.

Se si utilizza il valore predefinito SqlProvider o configura un provider personalizzato, è necessario aggiungere una regola con una mappa eventi il provider di collegamento. La regola seguente collega il nuovo provider appena creato per il **tutti gli eventi** mappa eventi. Questa regola registrerà tutti gli eventi in base a **WebBaseEvent** e li inviano alla MySqlWebEventProvider che utilizzerà la stringa di connessione MYASPNETDB. Il codice seguente aggiunge una regola per creare un collegamento al provider di una mappa eventi:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Se si desidera inviare solo gli errori di SQL Server, è possibile aggiungere la regola seguente:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Come inoltrare gli eventi di WMI

È anche possibile inoltrare gli eventi a WMI. Il provider WMI è configurato per l'utente nel file Web. config globale per impostazione predefinita.

Esempio di codice seguente aggiunge una regola per inoltrare gli eventi a WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

È necessario aggiungere una regola per associare un eventMapping per il provider e un'applicazione listener WMI per l'ascolto degli eventi. Esempio di codice seguente aggiunge una regola per il provider WMI per creare un collegamento di **tutti gli eventi** mappa eventi:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Come inoltrare gli eventi di posta elettronica

È anche possibile inoltrare gli eventi di posta elettronica. Prestare attenzione le regole quali eventi si esegue il mapping al provider di posta elettronica, come è possibile involontariamente inviare manualmente numerose informazioni che potrebbero essere più adatto per SQL Server o il registro eventi. Sono disponibili due provider di posta elettronica; SimpleMailWebEventProvider e TemplatedMailWebEventProvider. Ognuno ha gli stessi attributi di configurazione, fatta eccezione per gli attributi "modello" e "detailedTemplateErrors", che sono disponibili solo nel TemplatedMailWebEventProvider.

> [!NOTE]
> Nessuno di questi provider di posta elettronica viene configurato automaticamente. È necessario aggiungerli al file Web. config.


La differenza principale tra questi provider di posta due elettronica è che SimpleMailWebEventProvider invia messaggi di posta elettronica in un modello generico che non può essere modificato. Il file Web. config di esempio aggiunge il provider di posta elettronica all'elenco di provider configurati tramite la regola seguente:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

La regola seguente viene aggiunto anche per collegare il provider di posta elettronica per il **tutti gli eventi** mappa eventi:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 Tracing

Esistono tre principali miglioramenti apportati alla traccia di ASP.NET 2.0.

1. Funzionalità di traccia integrata
2. Accesso a livello di codice per i messaggi di traccia
3. Miglioramento dell'analisi a livello di applicazione

## <a name="integrated-tracing-functionality"></a>Integrazione di funzionalità di traccia

È possibile indirizzare i messaggi generati dalla classe Trace per l'output di traccia di ASP.NET e indirizzare i messaggi generati dall'analisi di ASP.NET a Trace. È anche possibile inoltrare gli eventi di strumentazione ASP.NET a Trace. Questa funzionalità viene fornita dalla nuova **writeToDiagnosticsTrace** attributo del &lt;traccia&gt; elemento. Quando questo valore booleano è true, i messaggi di traccia di ASP.NET vengono inoltrati all'infrastruttura di traccia System. Diagnostics per l'utilizzo da qualsiasi listener che sono registrati per visualizzare i messaggi di traccia.

## <a name="programmatic-access-to-trace-messages"></a>Accesso a livello di codice per i messaggi di traccia

In ASP.NET 2.0 per l'accesso a tutti i messaggi di traccia tramite il **TraceContextRecord** classe e **TraceRecords** insieme. Accesso ai messaggi di traccia il modo più efficiente consiste nel registrare un **TraceContextEventHandler** delegato (nuovo anche in ASP.NET 2.0) per gestire il nuovo **TraceFinished** evento. È quindi possibile scorrere i messaggi di traccia nel modo desiderato.

Questa condizione è illustrata nell'esempio di codice seguente:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

Nell'esempio precedente, scorrere la raccolta TraceRecords e quindi scriverà ogni messaggio nel flusso di risposta.

## <a name="improved-application-level-tracing"></a>Miglioramento dell'analisi a livello di applicazione

Analisi a livello di applicazione sono stata migliorata tramite l'introduzione del nuovo **mostRecent** attributo del &lt;traccia&gt; elemento. Questo attributo specifica se viene visualizzato l'output di traccia a livello di applicazione più recente e vengono eliminati i dati di traccia meno recenti che sono contrassegnati da requestLimit superano i limiti. Se false, i dati di traccia vengono visualizzati per le richieste fino a raggiunta l'attributo requestLimit.

## <a name="aspnet-command-line-tools"></a>Strumenti da riga di comando ASP.NET

Sono disponibili diversi strumenti da riga di comando per facilitare la configurazione di ASP.NET. Gli sviluppatori ASP.NET devono acquisire familiari con aspnet\_regiis.exe strumento. ASP.NET 2.0 fornisce tre altri strumenti da riga di comando per facilitare la configurazione.

Gli strumenti da riga di comando seguenti sono disponibili:

| **Strumento** | **Utilizzo** |
| --- | --- |
| **aspnet\_regiis.exe** | Consente la registrazione di ASP.NET con IIS. Sono disponibili due versioni di questo strumenti forniti con ASP.NET 2.0, uno per i sistemi a 32 bit (nella cartella Framework) e uno per i sistemi a 64 bit (nella cartella Framework64.) La versione a 64 bit non verrà installata in un sistema operativo a 32 bit. |
| **aspnet\_regsql.exe** | Lo strumento di registrazione di ASP.NET SQL Server viene utilizzato per creare un database di Microsoft SQL Server da utilizzare dai provider di SQL Server in ASP.NET o per aggiungere o rimuovere opzioni da un database esistente. Aspnet\_regsql.exe file si trova in [cartella drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber sul server Web. |
| **aspnet\_regbrowsers.exe** | Lo strumento di registrazione del Browser ASP.NET consente di analizzare e compila tutte le definizioni del browser a livello di sistema in un assembly e installa l'assembly nella global assembly cache. Lo strumento utilizza i file di definizione del browser (. File del BROWSER) dalla sottodirectory browser di .NET Framework. Lo strumento è reperibile nella directory %SystemRoot%\Microsoft.NET\Framework\version\. |
| **aspnet\_compiler.exe** | Lo strumento di compilazione ASP.NET consente di compilare un'applicazione Web ASP.NET, sul posto o per la distribuzione in un percorso di destinazione, ad esempio un server di produzione. Compilazione sul posto consente le prestazioni dell'applicazione perché gli utenti finali non si rilevano un ritardo nella prima richiesta all'applicazione durante l'applicazione viene compilata. |

Poiché aspnet\_regiis.exe strumento non è una novità di ASP.NET 2.0, non si parlerà qui.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Strumento di registrazione di ASP.NET SQL Server - aspnet\_regsql.exe

È possibile impostare diversi tipi di opzioni usando lo strumento di registrazione di ASP.NET SQL Server. È possibile specificare una connessione SQL, servizi applicativi ASP.NET che utilizzano SQL Server per gestire le informazioni, indicare il database o la tabella viene utilizzata per la dipendenza della cache SQL, aggiungere o rimuovere il supporto per l'utilizzo di SQL Server per archiviare le procedure e lo stato della sessione.

Alcuni servizi applicativi ASP.NET si basano su un provider per gestire l'archiviazione e recupero di dati da un'origine dati. Ogni provider è specifico dell'origine dati. ASP.NET include un provider di SQL Server per le funzionalità ASP.NET seguenti:

- Appartenenza (il [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) classe).
- Gestione dei ruoli (il [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) classe).
- Profilo (il [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) classe).
- Personalizzazione di Web part (il [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) classe).
- Eventi Web (il [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) classe).

Quando si installa ASP.NET, il file Machine. config per il server include gli elementi di configurazione che specificano i provider di SQL Server per ogni funzionalità di ASP.NET che si basano su un provider. Questi provider sono configurati per impostazione predefinita, per connettersi a un'istanza di un utente locale di SQL Server Express 2005. Se si modifica la stringa di connessione predefinita utilizzata dai provider, quindi prima di poter usare le funzionalità di ASP.NET configurato nella configurazione del computer, è necessario installare il database di SQL Server e gli elementi del database per la funzionalità scelta utilizzando Aspnet\_regsql.exe. Se il database specificati con lo strumento di registrazione SQL non esiste già (aspnetdb sarà il database predefinito se non è specificata nella riga di comando), quindi l'utente corrente deve disporre dei diritti per creare database in SQL Server, nonché e schema degli elementi procede all'interno di un database.

### <a name="sql-cache-dependency"></a>Dipendenza dalla cache SQL

Una funzionalità avanzata di memorizzazione nella cache di output ASP.NET è una dipendenza della cache SQL. Dipendenza della cache SQL supporta due modalità diverse di funzionamento: uno che utilizza un'implementazione di ASP.NET del polling di tabella e una seconda è una modalità che utilizza la funzionalità di notifica delle query di SQL Server 2005. Per configurare la modalità operativa del polling di tabella, è possibile utilizzare lo strumento di registrazione SQL.

### <a name="session-state"></a>Stato sessione

Per impostazione predefinita, le informazioni e i valori dello stato sessione vengono archiviati in memoria all'interno del processo ASP.NET. In alternativa, è possibile archiviare i dati della sessione in un database di SQL Server, in cui può essere condiviso da più server Web. Se il database specificato per lo stato della sessione con lo strumento di registrazione SQL non esiste già, quindi l'utente corrente deve disporre di diritti per creare database in SQL Server, nonché gli elementi dello schema all'interno di un database. Se il database esiste, quindi l'utente corrente deve disporre di diritti per creare gli elementi dello schema nel database esistente.

Per installare il database dello stato sessione in SQL Server, eseguire Aspnet\_regsql.exe strumento e fornire le informazioni con il comando seguente:

- Il nome del Server SQL dell'istanza, tramite il **-S** opzione.
- Le credenziali di accesso per un account che dispone dell'autorizzazione per creare un database in un computer che esegue SQL Server. Utilizzare il **-E** opzione per utilizzare l'utente attualmente connesso oppure di **- U** opzione per specificare un ID utente con il **-P** opzione per specificare una password.
- Il **- ssadd** opzione della riga di comando per aggiungere il database dello stato sessione.

Per impostazione predefinita, non è possibile utilizzare Aspnet\_regsql.exe strumento per installare il database dello stato sessione in un computer che esegue SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>Lo strumento di registrazione del Browser ASP.NET - aspnet\_regbrowsers.exe

In ASP.NET versione 1.1, il file Machine. config contiene una sezione denominata &lt;browserCaps&gt;. In questa sezione è contenuta una serie di voci XML che definiscono le configurazioni dei diversi browser, in base a un'espressione regolare. Per ASP.NET versione 2.0, una nuova. File BROWSER definisce i parametri di un determinato browser utilizzando le voci XML. Aggiungere informazioni su un nuovo browser aggiungendo una nuova. File BROWSER per la cartella in %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers nel sistema.

Perché un'applicazione non sta leggendo un file con estensione config ogni volta che richiede informazioni sul browser, è possibile creare una nuova. File BROWSER e Aspnet esecuzione\_regbrowsers.exe per aggiungere le modifiche necessarie all'assembly. In questo modo il server accedere immediatamente le nuove informazioni sul browser, pertanto non è necessario arrestare tutte le applicazioni per raccogliere le informazioni. Un'applicazione può accedere a funzionalità del browser mediante la proprietà Browser della richiesta HTTP corrente.

Le opzioni seguenti sono disponibili quando si esegue aspnet\_regbrowser.exe:

| **Opzione** | **Descrizione** |
| --- | --- |
| **-?** | Visualizza Aspnet\_regbbrowsers.exe il testo della Guida nella finestra di comando. |
| **-i** | Crea l'assembly con funzionalità browser runtime e installarlo nella global assembly cache. |
| **-u** | Disinstalla l'assembly con funzionalità browser runtime dalla global assembly cache. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>Lo strumento di compilazione ASP.NET - aspnet\_compiler.exe

Lo strumento di compilazione ASP.NET può essere utilizzato in due modi: per la compilazione sul posto e la compilazione per la distribuzione, in cui è specificata una directory di output di destinazione.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[La compilazione di un'applicazione sul posto](https://msdn.microsoft.com/library/ms229863.aspx)

Lo strumento di compilazione di ASP.NET è possibile compilare un'applicazione sul posto, vale a dire riproduce il comportamento di esecuzione di più richieste all'applicazione, causando la normale compilazione. Gli utenti di un sito precompilato non otterranno un ritardo causato dalla compilazione della pagina nella prima richiesta.

Quando si esegue la precompilazione di un sito, si applicano i seguenti elementi:

- Il sito mantiene i file e la struttura di directory.
- È necessario che i compilatori per tutti i linguaggi di programmazione utilizzati dal sito nel server.
- Se qualsiasi file si verifica un errore di compilazione, la compilazione dell'intero sito.

È inoltre possibile ricompilare un'applicazione sul posto dopo l'aggiunta di nuovi file di origine a esso. Lo strumento compila solo i file nuovi o modificati a meno che non si include il **- c** opzione.

> [!NOTE]
> Compilazione di un'applicazione che contiene un'applicazione annidata non viene compilato dell'applicazione annidata. L'applicazione nidificata deve essere compilata separatamente.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Si compila un'applicazione per la distribuzione](https://msdn.microsoft.com/library/ms229863.aspx)

Si compila un'applicazione per la distribuzione (compilazione in un percorso di destinazione), specificando il parametro targetDir. Può rappresentare la posizione finale per l'applicazione Web o l'applicazione compilata può essere distribuito ulteriormente. Utilizzo di **-u** opzione consente di compilare l'applicazione in modo che è possibile apportare modifiche a determinati file nell'applicazione compilata senza ricompilarla. ASPNET\_compiler.exe viene fatta distinzione tra tipi di file statici e dinamici e li gestisce in modo diverso durante la creazione dell'applicazione risultante.

- Tipi di file statici sono quelli che non è associato un compilatore o compilare i provider, ad esempio file la cui denominata con estensioni quali CSS, GIF, htm, HTML, jpg, js e così via. Questi file vengono semplicemente copiati nel percorso di destinazione, la propria posizione nella struttura delle directory.
- Tipi di file dinamica sono quelli che è associato un compilatore o compilare provider, inclusi i file con estensioni del nome file specifici di ASP.NET, ad esempio asax, ascx,. ashx,. aspx, browser, master e così via. Lo strumento di compilazione di ASP.NET genera gli assembly da tali file. Se il **-u** opzione viene omessa, lo strumento crea anche i file con l'estensione del nome file. COMPILED che eseguono il mapping tra i file di origine per i relativi assembly. Per mantenere la struttura di directory di origine dell'applicazione, lo strumento genera file segnaposto nei percorsi corrispondenti nell'applicazione di destinazione.

È necessario utilizzare il **-u** opzione per indicare che il contenuto dell'applicazione compilata può essere modificato. In caso contrario, le modifiche successive vengono ignorate o causano errori di run-time.

La tabella seguente descrive come il compilazione ASP.NET strumento gestisce diversi tipi di file quando il **-u** opzione è inclusa.

| **Tipo di file** | **Azione del compilatore** |
| --- | --- |
| .ascx, .aspx, .master | Questi file vengono suddivisi in markup e codice sorgente, che include i file code-behind e qualsiasi codice che è racchiuso tra parentesi &lt;script runat = "server"&gt; elementi. Codice sorgente viene compilato in assembly, con nomi che derivano da un algoritmo di hash e gli assembly vengono inseriti nella directory Bin. Qualsiasi codice inline, ovvero il codice racchiuso tra i ** &lt; % ** e ** % &gt; ** le parentesi quadre, incluso il markup e non viene compilato. Nuovo file con lo stesso nome dei file di origine vengono creati per contenere il codice e inseriti nella directory di output corrispondenti. |
| .ashx, .asmx | Questi file non vengono compilati e vengono spostati nella directory di output e non è stato compilato. Se si desidera che il codice del gestore di compilazione, inserire il codice nel file del codice sorgente nell'App\_directory del codice. |
| con estensione cs, vb, jsl, con estensione cpp (escluso il file code-behind per i tipi di file elencati in precedenza) | Questi file vengono compilati e inclusa come risorsa nell'assembly che vi fanno riferimento. File di origine non vengono copiati nella directory di output. Se un file di codice non viene fatto riferimento, non viene compilata. |
| Tipi di file personalizzati | Questi file non vengono compilati. Questi file vengono copiati nelle directory di output corrispondente. |
| File di codice nell'App di origine\_sottodirectory del codice | Questi file vengono compilati in assembly e inseriti nella directory Bin. |
| file con estensione resx e Resource nell'App\_sottodirectory GlobalResources | Questi file vengono compilati in assembly e inseriti nella directory Bin. Nessuna App\_GlobalResources sottodirectory viene creato nella directory di output principale e nessun file con estensione resx o Resources si trova nella directory di origine vengono copiato nelle directory di output. |
| file con estensione resx e Resource nell'App\_sottodirectory LocalResources | Questi file non vengono compilati e vengono copiati nella directory di output corrispondenti. |
| file con estensione skin nell'App\_sottodirectory di temi | Il file skin e file di tema statici non vengono compilati e vengono copiati nella directory di output corrispondenti. |
| Assembly già presente nella directory Bin di tipi di browser file Web. config statico | Questi file vengono copiati così come directory di output. |

La tabella seguente descrive come il compilazione ASP.NET strumento gestisce diversi tipi di file quando il **-u** opzione viene omessa.

| **Tipo di file** | **Azione del compilatore** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Questi file vengono suddivisi in markup e codice sorgente, che include i file code-behind e qualsiasi codice che è racchiuso tra parentesi &lt;script runat = "server"&gt; elementi. Codice sorgente viene compilato in assembly, con nomi che derivano da un algoritmo di hash. Gli assembly risultanti vengono inseriti nella directory Bin. Qualsiasi codice inline, ovvero il codice racchiuso tra i ** &lt; % ** e ** % &gt; ** le parentesi quadre, incluso il markup e non viene compilato. Il compilatore crea nuovi file per contenere il codice con lo stesso nome dei file di origine. Questi file risultanti vengono inseriti nella directory Bin. Il compilatore crea anche i file con lo stesso nome dei file di origine, ma con l'estensione. COMPILED che contengono informazioni di mapping. Il file con estensione File compilati vengono inseriti nella directory di output corrispondente nel percorso originale dei file di origine. |
| con estensione ascx | Questi file vengono suddivisi in markup e il codice sorgente. Codice sorgente viene compilato in assembly e inserito nella directory Bin, con nomi che derivano da un algoritmo di hash. Viene generato alcun file di markup. |
| con estensione cs, vb, jsl, con estensione cpp (escluso il file code-behind per i tipi di file elencati in precedenza) | Codice sorgente che fa riferimento l'assembly generato dal file con estensione aspx,. ashx o ascx viene compilato in assembly e inserito nella directory Bin. Nessun file di origine vengono copiato. |
| Tipi di file personalizzati | Questi file vengono compilati come file dinamici. A seconda del tipo di file su che sono basate, il compilatore può inserire file di mapping nella directory di output. |
| I file nell'App\_sottodirectory del codice | File del codice sorgente nella sottodirectory vengono compilati in assembly e inseriti nella directory Bin. |
| I file nell'App\_sottodirectory GlobalResources | Questi file vengono compilati in assembly e inseriti nella directory Bin. Nessuna App\_GlobalResources sottodirectory viene creato nella directory di output principale. Se il file di configurazione specifica appliesTo = "All", vengono copiati i file con estensione resx e. Resources nelle directory di output. Non vengono copiati se vi viene fatto riferimento da un [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| file con estensione resx e Resource nell'App\_sottodirectory LocalResources | Questi file vengono compilati in assembly con nomi univoci e inseriti nella directory Bin. Nessun file. resx o resources vengono copiato nelle directory di output. |
| file con estensione skin nell'App\_sottodirectory di temi | Temi vengono compilati in assembly e inseriti nella directory Bin. File stub vengono creati per i file con estensione skin e inseriti nella directory di output corrispondente. File statici (CSS) vengono copiati nelle directory di output. |
| Assembly già presente nella directory Bin di tipi di browser file Web. config statico | Questi file vengono copiati come directory di output. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Nomi di Assembly fissa](https://msdn.microsoft.com/library/ms229863.aspx##)

Alcuni scenari, quali la distribuzione di un'applicazione Web tramite Windows Installer MSI, richiedono l'uso di nomi di file coerente e contenuto, nonché le strutture di directory coerente per identificare l'assembly o le impostazioni di configurazione per gli aggiornamenti. In questi casi, è possibile utilizzare il **- fixednames** opzione per specificare che lo strumento di compilazione ASP.NET deve compilare un assembly per ogni file di origine anziché utilizzare quello in più pagine vengono compilate in assembly. Questo può causare un numero elevato di assembly, se si è interessati alla scalabilità è necessario utilizzare questa opzione con cautela.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Compilazione con nome sicuro](https://msdn.microsoft.com/library/ms229863.aspx##)

Il **- aptca**, **- delaysign**, **- keycontainer** e **- keyfile** sono disponibili le opzioni in modo che è possibile utilizzare Aspnet\_ gli assembly con nome Compiler.exe fortemente creare senza utilizzare il [strumento nome sicuro (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) separatamente. Queste opzioni corrispondono rispettivamente a **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**e ** AssemblyKeyFileAttribute**.

Descrizione di questi attributi è esterno all'ambito di questo corso.

## <a name="labs"></a>Labs

Ognuno dei seguenti laboratori sfrutta le esercitazioni precedenti. È necessario eseguire tali passaggi nell'ordine.

## <a name="lab-1-using-the-configuration-api"></a>Esercitazione 1: Utilizzo dell'API di configurazione

1. Creare un nuovo sito Web denominato *mod9lab*.
2. Aggiungere un nuovo File di configurazione Web al sito.
3. Aggiungere quanto segue al file Web. config:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

In questo modo si dispone di autorizzazioni per salvare le modifiche apportate al file Web. config.

1. Aggiungere un nuovo controllo etichetta a Default.aspx e modificare l'ID in **lblDebugStatus**.
2. Aggiungere un controllo pulsante di nuovo a Default.aspx.
3. Modificare l'ID del controllo Button **btnToggleDebug** e il testo **attiva/disattiva stato di Debug**.
4. Visualizzare il codice per il file code-behind di Default.aspx e aggiungere un **utilizzando** istruzione per **System.Web.Configuration** come indicato di seguito:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Aggiungere due variabili private alla classe e una pagina\_Init (metodo) come illustrato di seguito:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Aggiungere il codice seguente alla pagina\_carico:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Salvare e visualizzare default.aspx. Si noti che il controllo etichetta Visualizza lo stato di debug corrente.
2. Fare doppio clic sul pulsante nella finestra di progettazione e aggiungere il codice seguente all'evento Click per il controllo Button:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Salvare e selezionare default.aspx e fare clic sul pulsante.
2. Aprire il file Web. config dopo ogni pulsante di selezione e osservare il **debug** attributo la &lt;compilazione&gt; sezione.

## <a name="lab-2-logging-application-restarts"></a>Laboratorio 2: Registrazione di riavvio dell'applicazione

In questo laboratorio si creerà il codice che consente di attivare o disattivare la registrazione di ricompilazioni nel Visualizzatore eventi, avvii e arresti applicazione.

1. Aggiungere un controllo DropDownList default.aspx e modificare l'ID in ddlLogAppEvents.
2. Impostare il **un postback automatico** proprietà DropDownList per **true**.
3. Aggiungere tre elementi alla raccolta di elementi per DropDownList. Rendere il **testo** per il primo elemento *Select Value* e il valore -1. Rendere il **testo** e **valore** del secondo elemento **True** e **testo** e **valore** del terzo elemento **False**.
4. Aggiungere un'etichetta di nuovo a default.aspx. Modificare l'ID in **lblLogAppEvents**.
5. Aprire la visualizzazione di codice per default.aspx e aggiungere una nuova dichiarazione per una variabile di tipo HealthMonitoringSection, come illustrato di seguito:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Aggiungere il codice seguente al codice esistente nella pagina\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Fare doppio clic su DropDownList e aggiungere il codice seguente all'evento SelectedIndexChanged:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Sfoglia default.aspx.
2. Impostare l'elenco a discesa **False**.
3. Cancellare il registro applicazioni nel Visualizzatore eventi.
4. Fare clic sul pulsante per modificare l'attributo di Debug per l'applicazione.
5. Aggiornare il registro applicazioni nel Visualizzatore eventi. 

    1. Sono stati registrati eventi?
    2. I motivi?
6. Impostare l'elenco a discesa **True.**
7. Fare clic sul pulsante per attivare o disattivare l'attributo di Debug per l'applicazione.
8. Aggiornare l'account di accesso dell'applicazione nel Visualizzatore eventi. 

    1. Sono stati registrati eventi?
    2. Qual era il motivo dell'arresto di app?
9. Sperimentare abilitare o disabilitare la registrazione e osservare le modifiche apportate al file Web. config.

## <a name="more-information"></a>Ulteriori informazioni:

ASP.NET del 2.0 modello di Provider consente di creare i propri provider di strumentazione delle applicazioni non solo, ma molti altri scopi oltre ad esempio l'appartenenza, profili e così via. Per informazioni dettagliate sulla scrittura di un provider personalizzato per registrare gli eventi dell'applicazione in un file di testo, visitare [questo collegamento](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
