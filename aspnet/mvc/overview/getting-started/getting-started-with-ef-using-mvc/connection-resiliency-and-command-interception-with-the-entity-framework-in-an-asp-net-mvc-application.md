---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: Usare l'intercettazione di comando e la resilienza di connessione con Entity Framework in un'app ASP.NET MVC"
author: tdykstra
description: In questa esercitazione si apprenderà come usare l'intercettazione di comando e la resilienza di connessione. Sono due importanti funzionalità di Entity Framework 6.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4ef13e676340f1bfac1a25219e6455c89b69df5e
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837624"
---
# <a name="tutorial-use-connection-resiliency-and-command-interception-with-entity-framework-in-an-aspnet-mvc-app"></a>Esercitazione: Usare l'intercettazione di comando e la resilienza di connessione con Entity Framework in un'app ASP.NET MVC

Finora l'applicazione è stata in esecuzione in locale in IIS Express nel computer di sviluppo. Per rendere disponibile ad altri utenti per usare la rete Internet un'applicazione reale, è necessario distribuirla in un provider di hosting web, ed è necessario distribuire il database in un server di database.

In questa esercitazione si apprenderà come usare l'intercettazione di comando e la resilienza di connessione. Sono due importanti funzionalità di Entity Framework 6 che sono particolarmente utili quando esegue la distribuzione nell'ambiente cloud: (tentativi automatici per errori temporanei) resilienza della connessione e intercettazione dei comandi (catch tutte le query SQL inviate al database Per poter accedere o apportare modifiche).

Questa esercitazione l'intercettazione resilienza e comando di connessione è facoltativa. Se si ignora questa esercitazione, saranno necessario alcune piccole modifiche da apportare nelle esercitazioni successive.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Abilitare la resilienza della connessione
> * Abilitare l'intercettazione dei comandi
> * Testare la nuova configurazione

## <a name="prerequisites"></a>Prerequisiti

* [Ordinamento, filtro e paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-connection-resiliency"></a>Abilitare la resilienza della connessione

Quando si distribuisce l'applicazione in Azure, si verrà distribuito il database al Database SQL di Azure, un servizio di database cloud. Gli errori di connessione temporanei sono in genere più frequenti quando ci si connette a un servizio di database cloud rispetto a quando il server web e il server di database sono direttamente connesse tra loro nello stesso data center. Anche se un server web cloud e un servizio di database cloud sono ospitate nello stesso data center, sono presenti altre connessioni di rete tra di essi che può avere problemi, ad esempio servizi di bilanciamento del carico.

Anche un servizio cloud è in genere condivisi da altri utenti, ovvero che la velocità di risposta può essere influenzata da essi. E l'accesso al database potrà essere soggetti alla limitazione. La limitazione significa che il servizio di database genera eccezioni quando si tenta di accedere più frequentemente maggiore di quello consentito nel contratto di servizio (SLA).

Numero o la maggior parte dei problemi di connessione quando si accede a un servizio cloud sono temporanei, vale a dire, risolvono autonomamente in un breve periodo di tempo. Pertanto, quando si tenta un'operazione di database e ottenere un tipo di errore che è in genere temporaneo, è Impossibile ripetere l'operazione dopo un breve periodo di attesa e l'operazione potrebbe essere eseguita correttamente. È possibile fornire migliorare notevolmente la per gli utenti se si gestiscono gli errori temporanei automaticamente tentando nuovamente invisibili la maggior parte di essi al cliente. La funzionalità di resilienza di connessione in Entity Framework 6 consente di automatizzare processo di ripetizione non è stato possibile query SQL.

La funzionalità di resilienza di connessione deve essere configurata in modo appropriato per un servizio di database particolare:

- È necessario indicare le eccezioni che possono risultare temporanea. Si desidera ripetere gli errori causati da una perdita temporanea della connettività di rete, non gli errori causati da bug del programma, ad esempio.
- Deve attendere una quantità appropriata di tempo tra i tentativi di un'operazione non riuscita. È possibile attendere più tra i tentativi per un processo batch rispetto a quanto possibile per una pagina web online in cui un utente è in attesa di una risposta.
- È necessario ripetere un numero appropriato di volte prima di rinunciare. È possibile ripetere più volte in un processo batch che si farebbe in un'applicazione online.

È possibile configurare queste impostazioni manualmente per qualsiasi ambiente di database supportato da un provider di Entity Framework, ma i valori predefiniti che in genere funzionano bene per un'applicazione online che usa Database SQL di Azure sono già stati configurati per l'utente, e Queste sono le impostazioni che verrà implementato per l'applicazione Contoso University.

Tutto è necessario eseguire per abilitare la resilienza della connessione è creare una classe nell'assembly da cui deriva il [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) classe e in quella classe impostare il Database SQL *strategia di esecuzione*, che in Entity Framework è un altro termine per *criterio di ripetizione*.

1. Nella cartella di DAL, aggiungere un file di classe denominato *SchoolConfiguration.cs*.
2. Sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework esegue automaticamente il codice si trova in una classe che deriva da `DbConfiguration`. È possibile usare la `DbConfiguration` classe per eseguire attività di configurazione nel codice che si farebbe in caso contrario, il *Web. config* file. Per altre informazioni, vedere [EntityFramework configurazione basata su codice](https://msdn.microsoft.com/data/jj680699).
3. Nelle *StudentController.cs*, aggiungere un `using` informativa per `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Modifica di tutte le `catch` blocchi di cui vengono intercettate `DataException` eccezioni in modo che rilevano `RetryLimitExceededException` eccezioni invece. Ad esempio:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Si usava `DataException` per tentare di identificare gli errori che potrebbero essere temporanei per fornire un messaggio descrittivo "riprovare". Ma ora che hai attivato un criterio di ripetizione dei tentativi, solo gli errori presumibilmente temporaneo verranno già è stati cercati non è riusciti più volte e l'eccezione effettivo restituito verrà arrotondato nel `RetryLimitExceededException` eccezione.

Per altre informazioni, vedere [Entity Framework resilienza delle connessioni / logica di tentativi](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Abilitare l'intercettazione dei comandi

Ora che hai attivato un criterio di ripetizione dei tentativi, come testare per verificare che funzioni come previsto? Non è così facile forzare un errore temporaneo si verifica, in particolare quando si esegue in locale e sarebbe particolarmente difficile da integrare effettivo degli errori temporanei in un test di unit test automatizzati. Per testare la funzionalità di resilienza di connessione, è necessario un modo per intercettare le query che Entity Framework Invia a SQL Server e sostituire la risposta di SQL Server con un tipo di eccezione che è in genere temporaneo.

È anche possibile usare l'intercettazione di query per implementare una procedura consigliata per le applicazioni cloud: [log di latenza e l'esito positivo o negativo di tutte le chiamate ai servizi esterni](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) , ad esempio servizi di database. EF6 fornisce una [dedicata API di registrazione](https://msdn.microsoft.com/data/dn469464) che può rendere più semplice eseguire la registrazione, ma in questa sezione dell'esercitazione verrà illustrato come usare il Framework di entità [funzionalità di intercettazione](https://msdn.microsoft.com/data/dn469464) direttamente, sia per registrazione e per la simulazione di errori temporanei.

### <a name="create-a-logging-interface-and-class"></a>Creare una classe e l'interfaccia di registrazione

Oggetto [procedure consigliate per la registrazione](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) consiste nell'eseguire questa operazione usando un'interfaccia anziché impostare come hardcoded le chiamate a Diagnostics. Trace o una classe di registrazione. Che rende più semplice modificare il meccanismo di registrazione in un secondo momento, se è necessario eseguire questa operazione. In modo che in questa sezione si creerà l'interfaccia di registrazione e una classe per implementare tale/p >

1. Creare una cartella nel progetto e denominarla *Logging*.
2. Nel *Logging* cartella, creare un file di classe denominato *ILogger.cs*e sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    L'interfaccia fornisce tre livelli di traccia per indicare l'importanza relativa dei log e uno stato progettato per fornire le informazioni sulla latenza per le chiamate al servizio esterno, ad esempio le query di database. Metodi di registrazione presentano overload che consentono di passare un'eccezione. Si tratta in modo che le informazioni sull'eccezione, compresi stack trace e le eccezioni interne viene registrati in modo affidabile tramite la classe che implementa l'interfaccia, anziché basarsi su esso viene eseguita in ogni chiamata di metodo di registrazione in tutta l'applicazione.

    I metodi TraceApi consentono di tenere traccia della latenza di ogni chiamata a un servizio esterno, ad esempio Database SQL.
3. Nel *Logging* cartella, creare un file di classe denominato *Logger.cs*e sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    L'implementazione Usa System. Diagnostics per eseguire la traccia. Si tratta di una funzionalità incorporata di .NET che rende più semplice generare e usare le informazioni di traccia. Esistono molti "listener" è possibile usare con la traccia System. Diagnostics, scrivere i log per i file, ad esempio, o per scriverli nell'archivio blob di Azure. Alcune delle opzioni e collegamenti ad altre risorse per altre informazioni, vedere nella [risoluzione dei problemi di siti Web di Azure in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Per questa esercitazione esaminati solo i registri in Visual Studio **Output** finestra.

    In un'applicazione di produzione si potrebbe voler prendere in considerazione i pacchetti di traccia diverso da System. Diagnostics e l'interfaccia ILogger rende relativamente semplice passare a un meccanismo di traccia diverso se si decide di eseguire questa operazione.

### <a name="create-interceptor-classes"></a>Creare le classi dell'intercettore

Successivamente si creerà le classi di Entity Framework chiamerà in ogni volta che provvederà a inviare una query al database, uno per simulare gli errori temporanei e uno per eseguire la registrazione. Queste classi intercettore devono derivare dal `DbCommandInterceptor` classe. In essi contenuti è scrivere gli override dei metodi che vengono chiamati automaticamente quando query sta per essere eseguita. In questi metodi è possibile esaminare o le query che viene inviata al database di log ed è possibile modificare la query prima che venga inviata al database o restituire un valore Entity Framework manualmente senza nemmeno il passaggio della query al database.

1. Per creare la classe dell'intercettore che eseguirà l'accesso a ogni query SQL che viene inviato al database, creare un file di classe denominato *SchoolInterceptorLogging.cs* nel *DAL* cartella, quindi sostituire il modello di codice con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Per la query ha esito positivo o comandi, questo codice scrive un file di registro con le informazioni sulla latenza. Per le eccezioni, viene creato un log degli errori.
2. Per creare la classe dell'intercettore che genererà errori temporanei fittizi quando si immette "Throw" nel **ricerca** casella, creare un file di classe denominato *SchoolInterceptorTransientErrors.cs* nel *DAL* cartella, quindi sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Questo codice esegue l'override solo di `ReaderExecuting` metodo, che viene chiamato per le query che possono restituire più righe di dati. Se si vuole verificare la resilienza di connessione per gli altri tipi di query, è possibile anche eseguire l'override di `NonQueryExecuting` e `ScalarExecuting` metodi, come l'intercettore di registrazione viene.

    Quando si esegue la pagina degli studenti e immettere "Throw" come stringa di ricerca, questo codice crea un'eccezione del Database SQL fittizia per numero di errore pari a 20, un tipo noto per essere in genere temporaneo. Altri numeri di errore attualmente riconosciuti temporanei sono 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 e 40613, ma questi sono soggetti a modifiche nelle nuove versioni del Database SQL.

    Il codice restituisce l'eccezione a Entity Framework invece di eseguire la query e passando i risultati della query indietro. L'eccezione temporanea viene restituito quattro volte e quindi il codice ripristina la normale procedura del passaggio della query al database.

    Perché tutto ciò che viene eseguito l'accesso, sarà in grado di visualizzare i che Entity Framework tenta di eseguire la query quattro volte prima di essere completata e l'unica differenza nell'applicazione è che richiede più tempo per il rendering di una pagina con i risultati della query.

    Il numero di tentativi di Entity Framework è configurabile; il codice specifica quattro volte, perché questo è il valore predefinito per i criteri di esecuzione di Database SQL. Se si modificano i criteri di esecuzione, è stato anche modificato il codice che specifica quante volte vengono generati gli errori temporanei. È inoltre possibile modificare il codice per generare più eccezioni in modo che Entity Framework genererà il `RetryLimitExceededException` eccezione.

    Il valore immesso nella casella di ricerca si trovano `command.Parameters[0]` e `command.Parameters[1]` (uno viene usato per il nome e uno per il cognome). Quando viene trovato il valore "% Throw %", "Throw" viene sostituito in tali parametri dalla "an" in modo che alcuni studenti h verranno trovati e restituiti.

    Si tratta semplicemente un modo pratico per testare la resilienza di connessione in base alle modifiche alcuni input per l'applicazione dell'interfaccia utente. È anche possibile scrivere codice che genera gli errori temporanei per tutte le query o aggiornamenti, come illustrato in un secondo momento nei commenti sui *DbInterception.Add* (metodo).
3. Nelle *Global. asax*, aggiungere il codice seguente `using` istruzioni:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Aggiungere le righe evidenziate per il `Application_Start` metodo:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Queste righe di codice sono gli eventi che generano il codice dell'intercettore da eseguire quando Entity Framework di inviare query al database. Si noti che poiché sono create classi di intercettore separato per la simulazione di errore temporaneo e la registrazione, è possibile in modo indipendente abilitare e disabilitarli.

    È possibile aggiungere intercettori tramite il `DbInterception.Add` ovunque nel codice; non deve necessariamente trovarsi nel `Application_Start` (metodo). Un'altra opzione consiste nell'inserire questo codice nella classe DbConfiguration creato in precedenza per configurare i criteri di esecuzione.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Quando si inserisce questo codice, prestare attenzione a non eseguire `DbInterception.Add` dell'intercettore stesso più di una volta o si otterrà le istanze aggiuntive dell'intercettore. Ad esempio, se si aggiunta due volte l'intercettore di registrazione, si noterà due log per ogni query SQL.

    Gli intercettori vengono eseguiti nell'ordine di registrazione (l'ordine in cui il `DbInterception.Add` viene chiamato). L'ordine può essere rilevante a seconda di ciò che si sta effettuando nell'intercettore. Ad esempio, un intercettore può modificare il comando SQL che ottiene nel `CommandText` proprietà. Se viene modificato il comando SQL, l'intercettore successivo verrà visualizzato il comando SQL modificato, non il comando SQL originale.

    Il codice di simulazione di errore temporaneo scritto in modo che è possibile causare errori temporanei, immettere un valore diverso nell'interfaccia utente. In alternativa, è possibile scrivere il codice dell'intercettore per generare sempre la sequenza di eccezioni temporanee senza verificare la presenza di un valore di parametro specifico. È quindi possibile aggiungere l'intercettore solo quando si desidera generare errori temporanei. Se in questo caso, tuttavia, non aggiungere l'intercettore fino a dopo l'inizializzazione del database è stata completata. In altre parole, eseguire un'operazione di almeno un database, ad esempio una query su uno dei set di entità prima di avviare la generazione di errori temporanei. Entity Framework esegue diverse query durante l'inizializzazione del database e che non vengono eseguiti in una transazione, in modo che gli errori durante l'inizializzazione può causare il contesto in uno stato incoerente.

## <a name="test-the-new-configuration"></a>Testare la nuova configurazione

1. Premere **F5** per eseguire l'applicazione in modalità di debug e quindi fare clic sui **studenti** scheda.
2. Esaminare il Visual Studio **Output** finestra per visualizzare l'output di traccia. Si potrebbe essere necessario scorrere verso l'alto precedenti alcuni errori JavaScript per ottenere i log scritti dal logger.

    Si noti che è possibile visualizzare le query SQL inviate al database. Vengono visualizzate alcune query iniziale e i comandi che Entity Framework esegue per iniziare, controllare la versione del database e tabella di cronologia della migrazione (apprenderanno le migrazioni nella prossima esercitazione). E viene visualizzata una query per il paging, per scoprire il numero di studenti sono presenti, e infine viene visualizzata la query che recupera i dati degli studenti.

    ![Registrazione di query normali](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Nel **studenti** pagina, immettere "Throw" come stringa di ricerca e fare clic su **ricerca**.

    ![Generare una stringa di ricerca](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Si noterà che il browser sembra bloccarsi per diversi secondi mentre Entity Framework ritenterà l'esecuzione di query più volte. Il primo tentativo si verifica molto rapidamente, quindi il tempo di attesa prima dell'aumento prima di ogni tentativo aggiuntivo. Questo processo di attesa più prima che venga chiamato ogni nuovo tentativo *backoff esponenziale*.

    Quando la pagina Visualizza, che mostra dove gli studenti hanno "un" nei relativi nomi, esaminare la finestra di output e si noterà che la stessa query è stata tentata cinque volte, prima di tutto quattro volte ottenuto temporanei delle eccezioni. Per ogni errore temporaneo verrà visualizzato il log di cui si scrive quando si genera l'errore temporaneo nel `SchoolInterceptorTransientErrors` classe ("restituzione errore temporaneo per comando...") e vedrai i log scritti quando `SchoolInterceptorLogging` Ottiene l'eccezione.

    ![Output di registrazione che indica i tentativi](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Poiché è stato immesso una stringa di ricerca, la query che restituisce i dati degli studenti con parametri:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Non ci si connette il valore dei parametri, ma è possibile farlo. Se si desidera visualizzare i valori dei parametri, è possibile scrivere codice di registrazione per ottenere i valori di parametro i `Parameters` proprietà del `DbCommand` oggetto che si ottiene nei metodi dell'intercettore.

    Si noti che non è possibile ripetere questo test prima di arresta l'applicazione e riavviarlo. Se si desidera essere in grado di resilienza della connessione di test più volte in una singola esecuzione dell'applicazione, è possibile scrivere codice per reimpostare il contatore degli errori in `SchoolInterceptorTransientErrors`.
4. Per visualizzare le differenze la strategia di esecuzione (criteri di ripetizione dei tentativi) rende, commento il `SetExecutionStrategy` linea *SchoolConfiguration.cs*, eseguire nuovamente la pagina di studenti in modalità di debug e ripetere la ricerca per "Throw".

    Questa volta il debugger si arresta nella prima eccezione generata immediatamente quando si tenta di eseguire la prima volta che la query.

    ![Eccezione fittizia](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Rimuovere il commento il *SetExecutionStrategy* linea *SchoolConfiguration.cs*.

## <a name="get-the-code"></a>Ottenere il codice

[Download progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>Risorse aggiuntive

Collegamenti ad altre risorse di Entity Framework sono disponibili nel [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Resilienza della connessione abilitata
> * Intercettazione dei comandi abilitato
> * Testare la nuova configurazione

Passare all'articolo successivo per informazioni sulle migrazioni Code First e distribuzione di Azure.
> [!div class="nextstepaction"]
> [Codice migrazioni First e distribuzione di Azure](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
