---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Configurazione e la strumentazione | Microsoft Docs
author: microsoft
description: Esistono importanti modifiche alla configurazione e la strumentazione in ASP.NET 2.0. La nuova API di configurazione di ASP.NET consente di apportare modifiche di configurazione da apportare delle richieste pull...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 8437b13cae83208983f26e0a5042a5f6c19e516e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822907"
---
<a name="configuration-and-instrumentation"></a>Configurazione e strumentazione
====================
da [Microsoft](https://github.com/microsoft)

> Esistono importanti modifiche alla configurazione e la strumentazione in ASP.NET 2.0. La nuova API di configurazione di ASP.NET consente modifiche di configurazione da apportare a livello di codice. Inoltre, esistono molte nuove impostazioni di configurazione consentono le nuove configurazioni e la strumentazione.


Esistono importanti modifiche alla configurazione e la strumentazione in ASP.NET 2.0. La nuova API di configurazione di ASP.NET consente modifiche di configurazione da apportare a livello di codice. Inoltre, esistono molte nuove impostazioni di configurazione consentono le nuove configurazioni e la strumentazione.

In questo modulo, si parlerà dell'API di configurazione ASP.NET perché in relazione alla lettura e scrittura ai file di configurazione di ASP.NET e ti accenneremo anche la strumentazione ASP.NET. Verranno inoltre illustrate le nuove funzionalità disponibili nella traccia di ASP.NET.

## <a name="aspnet-configuration-api"></a>API di configurazione ASP.NET

L'API di configurazione di ASP.NET consente di sviluppare, distribuire e gestire i dati di configurazione dell'applicazione usando una singola interfaccia di programmazione. È possibile utilizzare l'API di configurazione per sviluppare e modificare a livello di programmazione di completare le configurazioni di ASP.NET senza modificare direttamente il codice XML nei file di configurazione. Inoltre, è possibile usare l'API di configurazione nelle applicazioni console e negli script sviluppati personalmente, negli strumenti di gestione basato sul Web e negli snap-in Microsoft Management Console (MMC).

I due strumenti di gestione della configurazione seguente usano l'API di configurazione e sono inclusi in .NET Framework versione 2.0:

- Lo snap-in MMC di ASP.NET, che usa l'API di configurazione per semplificare le attività amministrative, che fornisce una visualizzazione integrata dei dati di configurazione locale di tutti i livelli della gerarchia di configurazione.
- Lo strumento Amministrazione sito Web, che consente di gestire le impostazioni di configurazione per le applicazioni locali e remote, inclusi i siti ospitati.

L'API di configurazione di ASP.NET è costituito da un set di oggetti di gestione di ASP.NET che è possibile usare per configurare siti Web e applicazioni a livello di codice. Gli oggetti di gestione vengono implementati come una libreria di classi .NET Framework. Il modello di programmazione di API di configurazione consente di garantire affidabilità e coerenza codice tramite l'applicazione dei tipi di dati in fase di compilazione. Per renderlo più facile da gestire configurazioni dell'applicazione, l'API di configurazione consente di visualizzare i dati uniti da tutti i punti nella gerarchia di configurazione come una singola raccolta, anziché visualizzare i dati come raccolte separate da diversi file di configurazione. Inoltre, l'API di configurazione consente di modificare le configurazioni di tutta l'applicazione senza modificare direttamente il codice XML nei file di configurazione. Infine, l'API semplifica le attività di configurazione tramite il supporto di strumenti di amministrazione, ad esempio lo strumento Amministrazione sito Web. L'API di configurazione semplifica la distribuzione che supportano la creazione dei file di configurazione in un computer ed eseguendo gli script di configurazione tra più computer.

> [!NOTE]
> L'API di configurazione non supporta la creazione di applicazioni IIS.


## <a name="working-with-local-and-remote-configuration-settings"></a>Uso delle impostazioni di configurazione locale e remoto

Un oggetto di configurazione rappresenta la visualizzazione unita delle impostazioni di configurazione che si applicano a un'entità fisica specifica, ad esempio un computer, o a un'entità logica, ad esempio un'applicazione o un sito Web. L'entità specificata logico può esistere nel computer locale o in un server remoto. Quando è presente alcun file di configurazione per un'entità specificata, l'oggetto di configurazione rappresenta le impostazioni di configurazione predefinite come definito dal file Machine. config.

È possibile ottenere un oggetto di configurazione usando uno dei metodi di configurazione open dalle classi seguenti:

1. La classe ConfigurationManager se l'entità è un'applicazione client.
2. La classe WebConfigurationManager, se l'entità è un'applicazione Web.

Questi metodi restituirà un oggetto di configurazione, che a sua volta fornisce i metodi richiesti e le proprietà per gestire i file di configurazione sottostante. È possibile accedere a questi file per la lettura o scrittura.

### <a name="reading"></a>Lettura

Utilizzare il metodo GetSection o GetSectionGroup per leggere le informazioni di configurazione. L'utente o processo che legge deve avere autorizzazioni di lettura in tutti i file di configurazione della gerarchia.

> [!NOTE]
> Se si usa un metodo statico GetSection che accetta un parametro di percorso, il parametro path deve fare riferimento all'applicazione in cui viene eseguito il codice. In caso contrario, il parametro viene ignorato e vengono restituite le informazioni di configurazione per l'applicazione attualmente in esecuzione.


### <a name="writing"></a>Scrittura

Utilizzare uno dei metodi Save per scrivere le informazioni di configurazione. L'utente o il processo di scrittura deve disporre delle autorizzazioni per il file di configurazione e la directory a livello di gerarchia di configurazione corrente di scrittura, nonché tutti i file di configurazione della gerarchia di autorizzazioni di lettura.

Per generare un file di configurazione che rappresenta le impostazioni di configurazione ereditato per un'entità specificata, usare uno dei metodi save-configurazione seguenti:

1. Il metodo Save per creare un nuovo file di configurazione.
2. Il metodo SaveAs per generare un nuovo file di configurazione in un altro percorso.

## <a name="configuration-classes-and-namespaces"></a>Spazi dei nomi e classi di configurazione

Numerosi metodi e classi di configurazione sono simili tra loro. La tabella seguente descrive le classi di configurazione più usati e gli spazi dei nomi.

| **Classe di configurazione o lo spazio dei nomi** | **Descrizione** |
| --- | --- |
| [System. Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) dello spazio dei nomi | Contiene le classi di configurazione principale per tutte le applicazioni .NET Framework. Sezione gestore classi vengono utilizzate per ottenere dati di configurazione per una sezione da metodi, ad esempio GetSection e GetSectionGroup. Questi due metodi sono non statici. |
| Classe System.Configuration.Configuration | Rappresenta un set di dati di configurazione per un computer, applicazioni, directory Web o un'altra risorsa. Questa classe contiene metodi, ad esempio GetSection e GetSectionGroup, utili per l'aggiornamento delle impostazioni di configurazione e per ottenere riferimenti a sezioni e i gruppi. Questa classe viene utilizzata come tipo restituito per metodi che ottengono i dati di configurazione design-time, ad esempio i metodi delle classi WebConfigurationManager e ConfigurationManager. |
| Spazio dei nomi System | Contiene le classi del gestore per le sezioni di configurazione di ASP.NET definiti nella [impostazioni di configurazione ASP.NET](https://msdn.microsoft.com/library/b5ysx397.aspx). Sezione gestore classi vengono utilizzate per ottenere dati di configurazione per una sezione da metodi, ad esempio GetSection e GetSectionGroup. |
| Classe System.Web.Configuration.WebConfigurationManager | Fornisce metodi utili per ottenere i riferimenti alle impostazioni di configurazione di runtime e fase di progettazione. Questi metodi usano la classe System.Configuration.Configuration come tipo restituito. È possibile usare il metodo statico GetSection di questa classe o il metodo GetSection non statici della classe System.Configuration.ConfigurationManager in modo intercambiabile. Per le configurazioni dell'applicazione Web, è consigliabile utilizzare la classe System.Web.Configuration.WebConfigurationManager anziché la classe System.Configuration.ConfigurationManager. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) dello spazio dei nomi | Fornisce un modo per personalizzare ed estendere il provider di configurazione. Questa è la classe base per tutte le classi del provider nel sistema di configurazione. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) dello spazio dei nomi | Contiene classi e interfacce per la gestione e monitoraggio dell'integrità delle applicazioni Web. In teoria, questo spazio dei nomi non viene considerata parte dell'API di configurazione. Ad esempio, analisi e generazione di eventi viene eseguita tramite le classi in questo spazio dei nomi. |
| [Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) dello spazio dei nomi | Fornisce le classi necessarie per la strumentazione delle applicazioni di esporre le informazioni e gestione degli eventi tramite Strumentazione gestione Windows (WMI) a potenziali consumer. Monitoraggio dell'integrità ASP.NET utilizza WMI per il recapito degli eventi. In teoria, questo spazio dei nomi non viene considerata parte dell'API di configurazione. |

## <a name="reading-from-aspnet-configuration-files"></a>La lettura dal file di configurazione ASP.NET

La classe WebConfigurationManager è la classe di base per la lettura da file di configurazione ASP.NET. Esistono sostanzialmente tre passaggi per la lettura dei file di configurazione di ASP.NET:

1. Ottiene un oggetto di configurazione usando il metodo OpenWebConfiguration.
2. Ottenere un riferimento alla sezione desiderata nel file di configurazione.
3. Leggere le informazioni desiderate dal file di configurazione.

La configurazione oggetto rappresenta non rappresenta un file di configurazione specifico. Invece, rappresenta una visualizzazione unita della configurazione di un computer, l'applicazione o sito Web. Esempio di codice seguente crea un'istanza di un oggetto di configurazione che rappresenta la configurazione di un'applicazione Web chiamata *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Si noti che se il percorso /ProductInfo non esiste, il codice precedente restituirà la configurazione predefinita come specificato nel file Machine. config.


Dopo aver creato l'oggetto di configurazione, è quindi possibile usare il metodo GetSection o GetSectionGroup per esaminare le impostazioni di configurazione. Nell'esempio seguente ottiene un riferimento per le impostazioni di rappresentazione per l'applicazione ProductInfo precedente:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>La scrittura in file di configurazione ASP.NET

Come la lettura dai file di configurazione, la classe WebConfigurationManager è la base per la scrittura di file di configurazione Asp.NET. Esistono anche tre passaggi per la scrittura in file di configurazione ASP.NET.

1. Ottiene un oggetto di configurazione usando il metodo OpenWebConfiguration.
2. Ottenere un riferimento alla sezione desiderata nel file di configurazione.
3. Scrivere le informazioni desiderate dal file di configurazione usando la salva o Salva con nome metodo.

Il codice seguente cambia il **debug** attributo delle &lt;compilazione&gt; elemento su false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Quando viene eseguito questo codice, il **debug** attributo delle &lt;compilazione&gt; elemento verrà impostato su false per il *webApp* file Web. config dell'applicazione.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

Lo spazio dei nomi System.Web.Management fornisce le classi e interfacce per la gestione e monitoraggio dell'integrità delle applicazioni ASP.NET.

La registrazione viene effettuata definendo una regola che associa un provider di eventi. La regola definisce il tipo di eventi che vengono inviati al provider. Sono disponibili per poter registrare gli eventi di base seguenti:

| **WebBaseEvent** | La classe di evento di base per tutti gli eventi. Contiene la proprietà per tutti gli eventi, ad esempio di codice dell'evento, codice dettagliato dell'evento, la data e ora in cui è stato generato l'evento, sequenza di numero, il messaggio di evento e i dettagli dell'evento. |
| --- | --- |
| **WebManagementEvent** | La classe di evento di base per eventi di gestione, ad esempio durata dell'applicazione, richieste, errori e gli eventi di controllo. |
| **WebHeartbeatEvent** | L'evento generato dall'applicazione a intervalli regolari per acquisire le informazioni sullo stato di runtime utile. |
| **WebAuditEvent** | La classe base per gli eventi di controllo di sicurezza, che vengono usati per contrassegnare condizioni, ad esempio errore di autorizzazione, errore di decrittografia, *e così via.* |
| **WebRequestEvent** | Classe di base per tutti gli eventi informativi richiesta. |
| **WebBaseErrorEvent** | La classe base per tutti gli eventi che indicano le condizioni di errore. |

I tipi di provider disponibili consentono di inviare l'output di eventi per il Visualizzatore eventi, SQL Server, Strumentazione gestione Windows (WMI) e messaggio di posta elettronica. I provider configurati in precedenza e mapping eventi ridurre la quantità di operazioni necessarie per ottenere l'output dell'evento registrato.

ASP.NET 2.0 usa il registro eventi provider out-of-the-box per registrare eventi in base ai domini applicazione avvio e arresto, nonché la registrazione delle eccezioni non gestite. Ciò consente di coprire alcuni degli scenari di base. Ad esempio, si supponga che l'applicazione genera un'eccezione, ma l'utente non viene salvato l'errore e non è possibile riprodurre. Con la regola predefinita del registro eventi, sarebbe in grado di raccogliere le informazioni sull'eccezione e lo stack per farsi un'idea di che tipo di errore si è verificato. Un altro esempio si applica se l'applicazione sta per perdere lo stato della sessione. In tal caso, è possibile esaminare il registro eventi per determinare se il riciclo del dominio applicazione e la causa dell'arresto in primo luogo il dominio dell'applicazione.

Inoltre, l'integrità di sistema di monitoraggio è estendibile. Ad esempio, è possibile definire eventi personalizzati Web attivarli all'interno dell'applicazione e quindi definire una regola per inviare le informazioni sull'evento a un provider, ad esempio la posta elettronica. Ciò consente di collegare facilmente la strumentazione a provider di monitoraggio dell'integrità. Come ulteriore esempio, si potrebbe generare un evento ogni volta che un ordine viene elaborato e configurare una regola che ogni evento viene inviato al database di SQL Server. È anche possibile generare un evento quando un utente non riesce ad accedere più volte in una riga e impostare l'evento da utilizzare il provider basati su indirizzo di posta elettronica.

La configurazione per i provider predefiniti e gli eventi verrà archiviata nel file Web. config globale. Il file Web. config globale archivia tutte le impostazioni basate su Web che sono state archiviate nel file Machine. config in ASP.NET 1 x. Il file Web. config globale si trova nella directory seguente:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

Il &lt;healthMonitoring&gt; sezione del file Web. config globale sono disponibili impostazioni di configurazione predefinite. È possibile eseguire l'override di queste impostazioni o configurare le proprie impostazioni implementando il &lt;healthMonitoring&gt; sezione nel file Web. config per l'applicazione.

Il &lt;healthMonitoring&gt; sezione del file Web. config globale contiene gli elementi seguenti:

| **provider** | Contiene i provider impostato per il Visualizzatore eventi, WMI e SQL Server. |
| --- | --- |
| **eventMappings** | Contiene i mapping per le varie classi WebBase. È possibile estendere questo elenco se si genera la propria classe di evento. Generare la propria classe di evento offre una maggiore granularità il provider per che è inviare informazioni. Ad esempio, è possibile configurare le eccezioni non gestite da inviare al Server SQL, durante l'invio di eventi personalizzati al messaggio di posta elettronica. |
| **regole** | Collega eventMappings al provider. |
| **La memorizzazione nel buffer** | Utilizzato con i provider di posta elettronica e SQL Server per determinare la frequenza con cui gli eventi di scaricamento al provider. |

Di seguito è riportato un esempio di codice dal file Web. config globale.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Come archiviare gli eventi nel Visualizzatore eventi

Come accennato in precedenza, il provider per la registrazione di eventi nell'evento visualizzatore è configurato per l'utente nel file Web. config globale. Per impostazione predefinita, tutti gli eventi di base **WebBaseErrorEvent** e **WebFailureAuditEvent** vengono registrati. È possibile aggiungere regole aggiuntive per registrare informazioni aggiuntive nel log eventi. Ad esempio, se si desidera registrare tutti gli eventi (*vale a dire*, tutti gli eventi in base **WebBaseEvent**), è possibile aggiungere la regola seguente al file Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Questa regola collega il **tutti gli eventi** mappa eventi per il provider del registro eventi. EventMapping sia il provider sono inclusi nel file Web. config globale.

## <a name="how-to-store-events-to-sql-server"></a>Come archiviare gli eventi a SQL Server

Questo metodo Usa il **ASPNETDB** database, che viene generato da Aspnet\_regsql.exe strumento. Il provider predefinito Usa la stringa di connessione LocalSqlServer, che usa un database basato su file nell'App\_cartella di dati o l'istanza SQLExpress locale di SQL Server. La stringa di connessione LocalSqlServer e il SqlProvider sono configurati nel file Web. config globale.

La stringa di connessione LocalSqlServer nel file Web. config globale è simile alla seguente:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Se si desidera usare un'altra istanza di SQL Server, è necessario usare Aspnet\_strumento regsql.exe, reperibile nella % windir%\Microsoft.Net\Framework\v2.0.\* cartella. Usare Aspnet\_regsql.exe strumento per generare una classe personalizzata **ASPNETDB** del database nell'istanza di SQL Server, quindi aggiungere la stringa di connessione nel file di configurazione delle applicazioni e quindi aggiungere un provider tramite il nuovo stringa di connessione. Dopo aver creato il **ASPNETDB** database creato, è necessario impostare una regola per collegare un eventMapping il sqlProvider.

Se si usa il valore predefinito SqlProvider o configura un provider personalizzato, è necessario aggiungere una regola di collegamento il provider con una mappa eventi. La regola seguente collega il nuovo provider creata in precedenza per il **tutti gli eventi** mappa eventi. Questa regola registrerà tutti gli eventi in base **WebBaseEvent** e inviarli a MySqlWebEventProvider che utilizzerà la stringa di connessione MYASPNETDB. Il codice seguente aggiunge una regola per collegare il provider con una mappa eventi:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Se si desidera solo inviare gli errori di SQL Server, è possibile aggiungere la regola seguente:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Come inoltrare gli eventi WMI

È anche possibile inoltrare gli eventi WMI. Il provider WMI è configurato per l'utente nel file Web. config globale per impostazione predefinita.

Esempio di codice seguente aggiunge una regola per inoltrare gli eventi WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

È necessario aggiungere una regola per associare un eventMapping al provider e, inoltre, un'applicazione listener WMI per l'ascolto degli eventi. Esempio di codice seguente aggiunge una regola per collegare il provider WMI per la **tutti gli eventi** mappa eventi:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Come inoltrare gli eventi di posta elettronica

È possibile anche inoltrare gli eventi alla posta elettronica. Prestare attenzione che le regole eventi eseguire il mapping al provider di posta elettronica, come è possibile involontariamente inviare a se stessi una grande quantità di informazioni che potrebbero essere più adatti per SQL Server o nel registro eventi. Sono disponibili due provider di posta elettronica; SimpleMailWebEventProvider e TemplatedMailWebEventProvider. Ognuno ha gli stessi attributi di configurazione, fatta eccezione per gli attributi "template" e "detailedTemplateErrors", che sono disponibili solo nel TemplatedMailWebEventProvider.

> [!NOTE]
> Nessuno di questi provider di posta elettronica viene configurato automaticamente. È necessario aggiungerli al file Web. config.


La differenza principale tra questi provider di posta due elettronica è che SimpleMailWebEventProvider invia messaggi di posta elettronica in un modello generico che non può essere modificato. Il file Web. config di esempio aggiunge questo provider di posta elettronica all'elenco di provider configurati tramite la regola seguente:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

La regola seguente viene aggiunto anche di collegare il provider di posta elettronica per il **tutti gli eventi** mappa eventi:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 Tracing

Esistono tre principali miglioramenti apportati alla traccia di ASP.NET 2.0.

1. Funzionalità di traccia integrata
2. Accesso a livello di codice per i messaggi di traccia
3. Migliorata la traccia a livello di applicazione

## <a name="integrated-tracing-functionality"></a>Integrata la funzionalità di traccia

È ora possibile indirizzare i messaggi generati dalla classe Trace per l'output di traccia di ASP.NET e indirizzare i messaggi generati dall'analisi di ASP.NET a Trace. È possibile anche inoltrare gli eventi di strumentazione di ASP.NET a Trace. Questa funzionalità viene fornita dalla nuova **writeToDiagnosticsTrace** attributo il &lt;traccia&gt; elemento. Quando questo valore booleano è true, i messaggi di traccia di ASP.NET vengono inoltrati all'infrastruttura di traccia System. Diagnostics per l'utilizzo da qualsiasi listener che sono registrati per visualizzare i messaggi di traccia.

## <a name="programmatic-access-to-trace-messages"></a>Accesso a livello di codice per i messaggi di traccia

In ASP.NET 2.0 per l'accesso programmatico a tutti i messaggi di traccia tramite il **TraceContextRecord** classe e il **TraceRecords** raccolta. Il modo più efficiente l'accesso ai messaggi di traccia consiste nel registrare un' **TraceContextEventHandler** delegato (altra novità in ASP.NET 2.0) per gestire il nuovo **TraceFinished** evento. È quindi possibile eseguire un ciclo di messaggi di traccia come desiderato.

Questa condizione è illustrata nell'esempio di codice seguente:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

Nell'esempio precedente, scorrere la raccolta TraceRecords e quindi scrivere ogni messaggio del flusso di risposta.

## <a name="improved-application-level-tracing"></a>Migliorata la traccia a livello di applicazione

Traccia a livello di applicazione è stata migliorata tramite l'introduzione della nuova **mostRecent** attributo il &lt;traccia&gt; elemento. Questo attributo specifica se viene visualizzato l'output di traccia a livello di applicazione più recente e vengono eliminati i dati di traccia meno recenti che sono contrassegnati da requestLimit superano i limiti. Se false, i dati di traccia vengono visualizzati per le richieste finché non viene raggiunto l'attributo requestLimit.

## <a name="aspnet-command-line-tools"></a>Strumenti da riga di comando ASP.NET

Sono disponibili diversi strumenti da riga di comando per facilitare la configurazione di ASP.NET. Gli sviluppatori ASP.NET dovrebbero avere familiari con aspnet\_regiis.exe strumento. ASP.NET 2.0 fornisce tre altri strumenti da riga di comando per facilitare la configurazione.

Sono disponibili gli strumenti da riga di comando seguenti:

| **Strumento** | **Usare** |
| --- | --- |
| **aspnet\_regiis.exe** | Consente la registrazione di ASP.NET con IIS. Sono disponibili due versioni di questo strumenti forniti con ASP.NET 2.0, uno per i sistemi a 32 bit (nella cartella Framework) e uno per i sistemi a 64 bit (nella cartella Framework64.) La versione a 64 bit non verrà installata in un sistema operativo a 32 bit. |
| **aspnet\_regsql.exe** | Lo strumento di registrazione del Server SQL ASP.NET viene utilizzato per creare un database di Microsoft SQL Server per l'uso da parte dei provider di SQL Server in ASP.NET o per aggiungere o rimuovere le opzioni da un database esistente. Aspnet\_regsql.exe file si trova in [cartella drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber sul server Web. |
| **aspnet\_regbrowsers.exe** | Lo strumento di registrazione del Browser ASP.NET analizza e compila tutte le definizioni del browser a livello di sistema in un assembly e installa l'assembly nella global assembly cache. Lo strumento Usa i file di definizione del browser (. File del BROWSER) dalla sottodirectory browser di .NET Framework. Lo strumento è reperibile nella directory %SystemRoot%\Microsoft.NET\Framework\version\. |
| **aspnet\_compiler.exe** | Lo strumento di compilazione ASP.NET consente di compilare un'applicazione Web ASP.NET, sul posto o per la distribuzione in un percorso di destinazione, ad esempio un server di produzione. Compilazione sul posto consente le prestazioni dell'applicazione perché gli utenti finali non si verifichino un ritardo per la prima richiesta all'applicazione mentre l'applicazione viene compilata. |

Poiché aspnet\_regiis.exe strumento non è una novità di ASP.NET 2.0, non si parlerà viene qui.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Strumento di registrazione di Server SQL ASP.NET - aspnet\_regsql.exe

È possibile impostare diversi tipi di opzioni usando lo strumento di registrazione del Server SQL ASP.NET. È possibile specificare una connessione SQL, consente di specificare quali servizi dell'applicazione ASP.NET utilizzano SQL Server per la gestione delle informazioni, indicare il database o la tabella viene utilizzata per la dipendenza della cache SQL e aggiungere o rimuovere il supporto per l'uso di SQL Server per archiviare le procedure e lo stato della sessione.

Alcuni servizi applicativi ASP.NET si basano su un provider per gestire l'archiviazione e il recupero dei dati da un'origine dati. Ogni provider è specifico per l'origine dati. ASP.NET include un provider di SQL Server per le funzionalità ASP.NET seguenti:

- Appartenenze (il [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) classe).
- Gestione dei ruoli (il [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) classe).
- Profilo (il [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) classe).
- Personalizzazione di Web part (il [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) classe).
- Eventi Web (il [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) classe).

Quando si installa ASP.NET, il file Machine. config per il server include gli elementi di configurazione che specificano i provider di SQL Server per ognuna delle funzionalità che si basano su un provider di ASP.NET. Questi provider sono configurati, per impostazione predefinita, per connettersi a un'istanza utente locale di SQL Server Express 2005. Se si modifica la stringa di connessione predefinito utilizzata dai provider, quindi prima di poter usare le funzionalità di ASP.NET configurato nella configurazione del computer, è necessario installare il database di SQL Server e gli elementi del database per la funzionalità scelta utilizzando Aspnet\_regsql.exe. Se il database specificato con lo strumento di registrazione SQL non esiste già (aspnetdb sarà il seguente database predefinito se non è specificato nella riga di comando), quindi l'utente corrente deve disporre dei diritti per creare i database in SQL Server, nonché lo schema e degli elementi procede all'interno di un database.

### <a name="sql-cache-dependency"></a>Dipendenza dalla cache SQL

Una funzionalità avanzata di memorizzazione nella cache di output ASP.NET è la dipendenza della cache SQL. Dipendenza della cache SQL supporta due modalità diverse di funzionamento: uno che usa un'implementazione di ASP.NET di polling di tabella e una seconda modalità che utilizza le caratteristiche di notifica di query di SQL Server 2005. Lo strumento di registrazione SQL utilizzabile per configurare la modalità tabella polling dell'operazione.

### <a name="session-state"></a>Stato sessione

Per impostazione predefinita, le informazioni e valori di stato della sessione vengono archiviati in memoria all'interno del processo ASP.NET. In alternativa, è possibile archiviare i dati della sessione in un database di SQL Server, in cui possono essere condivisi da più server Web. Se il database specificato per lo stato della sessione con lo strumento di registrazione SQL non esiste già, quindi l'utente corrente deve disporre dei diritti per creare i database in SQL Server, nonché gli elementi dello schema all'interno di un database. Se il database esiste, quindi l'utente corrente deve disporre dei diritti per creare gli elementi dello schema del database esistente.

Per installare il database dello stato sessione SQL Server, eseguire Aspnet\_regsql.exe strumento e fornire le informazioni con il comando seguente:

- Il nome del Server SQL dell'istanza, tramite il **-S** opzione.
- Le credenziali di accesso per un account che dispone dell'autorizzazione per creare un database in un computer che esegue SQL Server. Usare la **-E** opzione per utilizzare l'utente attualmente connesso oppure utilizzare il **- U** opzione per specificare un ID utente insieme al **-P** opzione per specificare una password.
- Il **ssadd -** opzione della riga di comando per aggiungere il database dello stato sessione.

Per impostazione predefinita, non è possibile usare Aspnet\_regsql.exe strumento per installare il database dello stato della sessione in un computer che esegue SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>Lo strumento di registrazione ASP.NET Browser - aspnet\_regbrowsers.exe

In ASP.NET versione 1.1, il file Machine. config contiene una sezione denominata &lt;browserCaps&gt;. In questa sezione è contenuta una serie di voci XML che definiscono le configurazioni dei diversi browser basato su un'espressione regolare. Per ASP.NET versione 2.0, una nuova. BROWSER file definisce i parametri di un determinato browser utilizzando le voci XML. Aggiungere informazioni su un nuovo browser mediante l'aggiunta di una nuova. File del BROWSER per la cartella che si trova in %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers nel sistema.

Poiché un'applicazione non legge un file con estensione config ogni volta che richiede informazioni sul browser, è possibile creare una nuova. File del BROWSER e Aspnet esecuzione\_regbrowsers.exe per aggiungere le modifiche necessarie all'assembly. In questo modo al server di accedere immediatamente le nuove informazioni del browser in modo che non è necessario arrestare tutte le applicazioni per prelevare le informazioni. Un'applicazione può accedere a funzionalità del browser tramite la proprietà di Browser della richiesta HTTP corrente.

Le opzioni seguenti sono disponibili quando si esegue aspnet\_regbrowser.exe:

| **Opzione** | **Descrizione** |
| --- | --- |
| **-?** | Visualizza Aspnet\_regbbrowsers.exe testo della Guida nella finestra di comando. |
| **-i** | Crea l'assembly di funzionalità del browser di runtime e lo installa nella global assembly cache. |
| **-u** | Disinstalla l'assembly di funzionalità di runtime del browser dalla global assembly cache. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>Lo strumento di compilazione ASP.NET - aspnet\_compiler.exe

Lo strumento di compilazione ASP.NET può essere utilizzato in due modi generali: per la compilazione sul posto e la compilazione per la distribuzione, in cui viene specificata una directory di output di destinazione.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Compila un'applicazione posto](https://msdn.microsoft.com/library/ms229863.aspx)

Lo strumento di compilazione ASP.NET è possibile compilare un'applicazione posto, vale a dire riproduce il comportamento di effettuare più richieste per l'applicazione, con conseguente normale compilazione. Gli utenti di un sito di pre-compilato non possa sperimentare ritardi causato dalla compilazione la pagina alla prima richiesta.

Quando si precompila un sito sul posto, si applicano i seguenti elementi:

- Il sito mantiene il file e una struttura di directory.
- È necessario che i compilatori per tutti i linguaggi di programmazione utilizzati dal sito nel server.
- In caso di compilazione, tutti i file dell'intero sito ha esito negativo di compilazione.

È inoltre possibile ricompilare un'applicazione posto dopo l'aggiunta di nuovi file di origine a esso. Lo strumento consente di compilare solo i file nuovi o modificati a meno che non si include il **- c** opzione.

> [!NOTE]
> Compilazione di un'applicazione che contiene un'applicazione annidata non viene compilato l'applicazione annidato. L'applicazione annidato deve essere compilato separatamente.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[La compilazione di un'applicazione per la distribuzione](https://msdn.microsoft.com/library/ms229863.aspx)

Si compila un'applicazione per la distribuzione (compilazione di un percorso di destinazione), specificando il parametro targetDir. Che può rappresentare la posizione finale per l'applicazione Web o l'applicazione compilata può essere distribuito ulteriormente. Usando il **-u** opzione Compila l'applicazione in modo tale che è possibile apportare modifiche a determinati file nell'applicazione compilata senza ricompilazione. ASPNET\_compiler.exe opera una distinzione tra i tipi di file statici e dinamici e li gestisce in modo diverso quando si crea l'applicazione risultante.

- Tipi di file statici sono quelli che non è associato un compilatore o provider, ad esempio file la cui compilazione denominato avere estensioni, ad esempio CSS,. gif,. htm, HTML, jpg,. js e così via. Questi file vengono semplicemente copiati nel percorso di destinazione, la propria posizione nella struttura di directory mantenuto.
- Tipi di file dinamici sono quelli che è associato un compilatore o compilare provider, inclusi i file con estensioni file specifiche di ASP.NET, ad esempio asax,. ascx,. ashx,. aspx,. browser, con estensione master e così via. Lo strumento di compilazione ASP.NET genera gli assembly da questi file. Se il **-u** opzione viene omessa, lo strumento crea anche i file con l'estensione del nome file. COMPILED che eseguono il mapping di file di origine per l'assembly. Per verificare che venga mantenuta la struttura di directory di origine dell'applicazione, lo strumento genera file segnaposto nei percorsi corrispondenti nell'applicazione di destinazione.

È necessario usare il **-u** opzione per indicare che il contenuto dell'applicazione compilata può essere modificato. In caso contrario, le modifiche successive vengono ignorate o causano errori di run-time.

La tabella seguente descrive come il compilazione ASP.NET lo strumento gestisce diversi tipi di file quando le **-u** opzione è inclusa.

| **Tipo di file** | **Azione di compilazione** |
| --- | --- |
| .ascx, .aspx, .master | Questi file vengono suddivisi in markup e codice sorgente, che include i file code-behind e il codice che è racchiuso tra parentesi &lt;script runat = "server"&gt; elementi. Codice sorgente viene compilato nell'assembly, con nomi che derivano da un algoritmo di hash, e gli assembly vengono inseriti nella directory Bin. Qualsiasi codice inline, ovvero il codice racchiuso tra i **&lt; %** e **% &gt;** le parentesi quadre, incluso con il markup e non viene compilato. I nuovi file con lo stesso nome di file di origine vengono creati per contenere il codice e inseriti nella directory di output corrispondenti. |
| .ashx, .asmx | Questi file non vengono compilati e vengono spostati nelle directory di output e non compilata. Se si vuole avere il codice del gestore di compilazione, inserire il codice nel file del codice sorgente nell'App\_directory del codice. |
| . cs, vb, JLS, con estensione cpp (senza includere il file code-behind per i tipi di file elencati in precedenza) | Questi file vengono compilati e inclusa come risorsa nell'assembly che vi fanno riferimento. File di origine non vengono copiati nella directory di output. Se un file di codice non viene fatto riferimento, non viene compilata. |
| Tipi di file personalizzati | Questi file non vengono compilati. Questi file vengono copiati nelle directory di output corrispondente. |
| File di codice nell'App di origine\_sottodirectory del codice | Questi file vengono compilati in assembly e inseriti nella directory Bin. |
| i file resx e Resources nell'App\_GlobalResources sottodirectory | Questi file vengono compilati in assembly e inseriti nella directory Bin. Nessuna App\_GlobalResources sottodirectory viene creato nella directory di output principale e nessun file RESX o. resources che si trova nella directory di origine vengono copiato nelle directory di output. |
| i file resx e Resources nell'App\_LocalResources sottodirectory | Questi file non vengono compilati e vengono copiati nella directory di output corrispondenti. |
| i file nell'App. skin\_sottodirectory di temi | I file. skin e tema statici non vengono compilati e vengono copiati nella directory di output corrispondenti. |
| gli assembly già presenti nella directory Bin di tipi di browser file statico Web. config | Questi file vengono copiati quando è nelle directory di output. |

La tabella seguente descrive come il compilazione ASP.NET lo strumento gestisce diversi tipi di file quando le **-u** opzione viene omessa.

| **Tipo di file** | **Azione di compilazione** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Questi file vengono suddivisi in markup e codice sorgente, che include i file code-behind e il codice che è racchiuso tra parentesi &lt;script runat = "server"&gt; elementi. Codice sorgente viene compilato nell'assembly, con nomi che derivano da un algoritmo di hash. Gli assembly risultanti vengono inseriti nella directory Bin. Qualsiasi codice inline, ovvero il codice racchiuso tra i **&lt; %** e **% &gt;** le parentesi quadre, incluso con il markup e non viene compilato. Il compilatore crea nuovi file per contenere il codice con lo stesso nome di file di origine. Questi file risultanti vengono inseriti nella directory Bin. Il compilatore crea anche i file con lo stesso nome di file di origine, ma con l'estensione. COMPILED che contengono informazioni di mapping. Il file con estensione I file compilati vengono inseriti nella directory di output corrispondente nel percorso originale dei file di origine. |
| . ascx | Questi file vengono suddivisi in markup e codice sorgente. Codice sorgente è compilato nell'assembly e inserito nella directory Bin, con nomi che derivano da un algoritmo di hash. Viene generato alcun file di markup. |
| . cs, vb, JLS, con estensione cpp (senza includere il file code-behind per i tipi di file elencati in precedenza) | Il codice sorgente che fa riferimento l'assembly generato dal file con estensione aspx,. ashx o. ascx è compilato nell'assembly e inserito nella directory Bin. Nessun file di origine vengono copiati. |
| Tipi di file personalizzati | Questi file vengono compilati, ad esempio file dinamici. A seconda del tipo di file su che sono basate, il compilatore può inserire i file di mapping nella directory di output. |
| I file nell'App\_sottodirectory del codice | File del codice sorgente in questa sottodirectory sono compilati in assembly e inseriti nella directory Bin. |
| I file nell'App\_GlobalResources sottodirectory | Questi file vengono compilati in assembly e inseriti nella directory Bin. Nessuna App\_GlobalResources sottodirectory viene creato nella directory di output principale. Se il file di configurazione specifica appliesTo = "All", vengono copiati i file con estensione resx e Resources nelle directory di output. Non vengono copiati se vi viene fatto riferimento da un [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| i file resx e Resources nell'App\_LocalResources sottodirectory | Questi file vengono compilati in assembly con nomi univoci e inseriti nella directory Bin. Nessun file con estensione resource o RESX vengono copiato nelle directory di output. |
| i file nell'App. skin\_sottodirectory di temi | I temi sono compilati in assembly e inseriti nella directory Bin. File stub vengono creati per i file. skin e inseriti nella directory di output corrispondente. I file statici (ad esempio CSS) vengono copiati nelle directory di output. |
| gli assembly già presenti nella directory Bin di tipi di browser file statico Web. config | Questi file vengono copiati quando è nella directory di output. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Nomi di Assembly fissa](https://msdn.microsoft.com/library/ms229863.aspx##)

Alcuni scenari, quali la distribuzione di un'applicazione Web tramite il programma di installazione di Windows MSI, richiedono l'uso di nomi di file coerente e contenuto, nonché le strutture di directory coerente per identificare gli assembly o le impostazioni di configurazione per gli aggiornamenti. In questi casi, è possibile usare la **- fixednames** opzione per specificare che lo strumento di compilazione ASP.NET deve compilare un assembly per ogni file di origine invece di usare where più pagine vengono compilate in assembly. Questo può causare un numero elevato di assembly, se si è interessati alla scalabilità è necessario utilizzare questa opzione con cautela.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Compilazione con nome sicuro](https://msdn.microsoft.com/library/ms229863.aspx##)

Il **- aptca**, **- delaysign**, **- keycontainer** e **- keyfile** sono disponibili opzioni in modo che è possibile usare Aspnet\_ assembly con nomi sicuri Compiler.exe creare fortemente senza usare la [strumento nome sicuro (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) separatamente. Queste opzioni corrispondono rispettivamente a **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**e  **AssemblyKeyFileAttribute**.

Descrizione di questi attributi è esterna all'ambito di questo corso.

## <a name="labs"></a>Labs

Ognuno dei laboratori di seguente si basa sulle esercitazioni precedenti. È necessario completarli nell'ordine.

## <a name="lab-1-using-the-configuration-api"></a>Laboratorio 1: Usando l'API di configurazione

1. Creare un nuovo sito Web chiamato *mod9lab*.
2. Aggiungere un nuovo File di configurazione Web al sito.
3. Aggiungere quanto segue al file Web. config:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Ciò garantisce di avere l'autorizzazione per salvare le modifiche apportate al file Web. config.

1. Aggiungere un nuovo controllo etichetta a default. aspx e modificare l'ID **lblDebugStatus**.
2. Aggiungere un controllo pulsante di nuovo a default. aspx.
3. Modificare l'ID del controllo Button al **btnToggleDebug** e il testo da **attiva/disattiva stato di Debug**.
4. Aprire la visualizzazione del codice per il file code-behind di default. aspx e aggiungere una **usando** informativa **System** come indicato di seguito:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Aggiungere due variabili private alla classe e una pagina\_metodo Init, come illustrato di seguito:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Aggiungere il codice seguente alla pagina\_carico:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Salvare e Sfoglia default. aspx. Si noti che il controllo etichetta Visualizza lo stato di debug corrente.
2. Fare doppio clic sul controllo pulsante nella finestra di progettazione e aggiungere il codice seguente all'evento Click per il controllo pulsante:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Salvare e Sfoglia default. aspx e fare clic sul pulsante.
2. Aprire il file Web. config dopo ogni pulsante di selezione e osservare il **debug** attributo il &lt;compilazione&gt; sezione.

## <a name="lab-2-logging-application-restarts"></a>Laboratorio 2: Registrazione riavvio dell'applicazione

In questo laboratorio si creerà il codice che è possibile attivare o disattivare la registrazione dell'applicazione chiusure, neo-imprese e ricompilazioni nel Visualizzatore eventi.

1. Aggiungere un controllo DropDownList a default. aspx e modificare l'ID in ddlLogAppEvents.
2. Impostare il **AutoPostBack** proprietà per il controllo DropDownList per **true**.
3. Aggiungere tre elementi alla raccolta di elementi per il controllo DropDownList. Verificare i **testo** per la prima voce *Select Value* e il valore -1. Rendere la **testo** e **valore** dell'elemento del secondo **True** e il **testo** e **valore** del terzo elemento **False**.
4. Aggiungere una nuova etichetta a default. aspx. Modificare l'ID **lblLogAppEvents**.
5. Aprire la visualizzazione di code-behind per default. aspx e aggiungere una nuova dichiarazione per una variabile di tipo HealthMonitoringSection come illustrato di seguito:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Aggiungere il codice seguente al codice esistente nella pagina\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Fare doppio clic su DropDownList e aggiungere il codice seguente all'evento SelectedIndexChanged:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Sfoglia default. aspx.
2. Impostare l'elenco a discesa **False**.
3. Cancellare il registro applicazioni nel Visualizzatore eventi.
4. Fare clic sul pulsante per modificare l'attributo di Debug per l'applicazione.
5. Aggiornare il registro applicazioni nel Visualizzatore eventi. 

    1. Sono stati registrati eventuali eventi?
    2. I motivi?
6. Impostare l'elenco a discesa **True.**
7. Fare clic sul pulsante per attivare o disattivare l'attributo di Debug per l'applicazione.
8. Aggiornare l'account di accesso dell'applicazione nel Visualizzatore eventi. 

    1. Sono stati registrati eventuali eventi?
    2. Qual è il motivo per l'arresto dell'app?
9. Sperimentare abilitare o disabilitare la registrazione ed esaminare le modifiche apportate al file Web. config.

## <a name="more-information"></a>Altre informazioni:

ASP.NET 2.0 Provider model consente di creare i propri provider di strumentazione delle applicazioni non solo, ma molti altri usi oltre, ad esempio l'appartenenza, i profili e così via. Per informazioni dettagliate su come scrivere un provider personalizzato per registrare gli eventi dell'applicazione in un file di testo, visitare [questo collegamento](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
