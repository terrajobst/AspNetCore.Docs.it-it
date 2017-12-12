---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: Resilienza di connessione e comando di intercettazione con Entity Framework in un'applicazione MVC ASP.NET | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Code First di Entity Framework 6 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2015
ms.topic: article
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: fecdd582918a61f3d01519c75d159f9c601c8223
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Resilienza di connessione e comando di intercettazione con Entity Framework in un'applicazione MVC ASP.NET
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Visual Studio 2013 e Code First di Entity Framework 6. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Fino a questo punto l'applicazione è stato in esecuzione in locale in IIS Express nel computer di sviluppo. Per rendere disponibile ad altri utenti di utilizzare in Internet un'applicazione reale, è possibile distribuirlo in un provider di hosting web che è necessario distribuire il database in un server di database.

In questa esercitazione verrà illustrato come usare due funzionalità di Entity Framework 6 che sono particolarmente utili durante la distribuzione nell'ambiente cloud: resilienza delle connessioni (tentativi automatici per errori temporanei) e l'intercettazione di comando (catch tutte le query SQL inviato al database per accedere o apportare modifiche).

In questa esercitazione l'intercettazione resilienza e comando di connessione è facoltativa. Se si ignora questa esercitazione, saranno necessario alcune piccole modifiche da apportare nelle esercitazioni successive.

## <a name="enable-connection-resiliency"></a>Abilitare la resilienza delle connessioni

Quando si distribuisce l'applicazione in Windows Azure, il database verrà distribuita a Database SQL di Azure, un servizio di database cloud. Errori di connessione temporanei sono in genere più di frequenti quando ci si connette a un servizio di database cloud rispetto a quando il server web e il server di database vengono connessi direttamente insieme nello stesso data center. Anche se un server web di cloud e un servizio cloud di database sono ospitati nello stesso data center, sono presenti più connessioni di rete tra di essi che può avere problemi, ad esempio servizi di bilanciamento del carico.

Anche un servizio cloud è in genere condivisi da altri utenti, che indica che i tempi di risposta può essere influenzato da essi. E l'accesso al database potrebbe essere soggetto a limitazione. La limitazione delle richieste indica il servizio di database genera eccezioni quando si tenta di accedere più frequentemente maggiore di quello consentito nel contratto di servizio (SLA).

Numero o la maggior parte dei problemi di connessione quando si accede a un servizio cloud sono temporanei, vale a dire si risolvono spontaneamente in un breve periodo di tempo. Pertanto, quando si tenta un'operazione di database e ottenere un tipo di errore che è in genere temporaneo, è Impossibile ripetere l'operazione dopo un breve periodo di attesa e l'operazione potrebbe essere eseguita correttamente. È possibile fornire una migliore esperienza per gli utenti se si gestiscono gli errori temporanei automaticamente tentando nuovamente invisibili la maggior parte di essi al cliente. Consente di automatizzare la funzionalità di resilienza di connessione in Entity Framework 6 processo di ripetizione non è stato possibile query SQL.

La funzionalità di resilienza di connessione deve essere configurata correttamente per un servizio di database specifico:

- È necessario indicare le eccezioni che possono essere temporanei. Si desidera tentare di errori causati da una perdita temporanea della connettività di rete, non gli errori causati da errori di programma, ad esempio.
- È necessario attendere un periodo di tempo tra i tentativi di un'operazione non riuscita appropriato. È possibile attendere più tra i tentativi per un processo batch rispetto a quanto possibile per una pagina web online in cui un utente è in attesa di una risposta.
- È necessario ripetere un numero appropriato di volte prima di rinunciare. Si potrebbe voler ripetere più volte in un processo batch che ci si trovasse in un'applicazione in linea.

È possibile configurare queste impostazioni manualmente per un ambiente di database supportate da un provider di Entity Framework, ma i valori predefiniti che in genere funzionano anche per un'applicazione in linea che utilizza Database SQL di Azure sono già stati configurati, e Queste sono le impostazioni implementate per l'applicazione Contoso University.

È necessario eseguire per abilitare la resilienza di connessione è creare una classe nell'assembly da cui deriva il [DbConfiguration](https://msdn.microsoft.com/en-us/data/jj680699.aspx) e in tale classe impostare il Database SQL *strategia di esecuzione*, che in EF è un altro termine per *criteri di ripetizione*.

1. Nella cartella di DAL, aggiungere un file di classe denominato *SchoolConfiguration.cs*.
2. Sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework esegue automaticamente il codice si trova in una classe che deriva da `DbConfiguration`. È possibile utilizzare il `DbConfiguration` classe per eseguire attività di configurazione nel codice che si procede in caso contrario il *Web. config* file. Per ulteriori informazioni, vedere [EntityFramework basato sul codice di configurazione](https://msdn.microsoft.com/en-us/data/jj680699).
3. In *StudentController.cs*, aggiungere un `using` istruzione per `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Modificare tutte le `catch` blocca tale catch `DataException` eccezioni in modo che rilevano `RetryLimitExceededException` eccezioni invece. Ad esempio:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Si utilizzassero `DataException` per tentare di identificare gli errori che potrebbero essere temporanei per fornire un messaggio descrittivo "riprovare". Ora che hai attivato in un criterio di ripetizione, solo gli errori potrebbe essere temporaneo verranno già è stati tentati non è riusciti più volte e ha restituito l'eccezione verrà incapsulato in, ma il `RetryLimitExceededException` eccezione.

Per ulteriori informazioni, vedere [resilienza di connessione Entity Framework o logica di tentativi](https://msdn.microsoft.com/en-us/data/dn456835).

## <a name="enable-command-interception"></a>Attiva l'intercettazione di comando

Ora che hai attivato in un criterio di ripetizione, come test per verificare che funzioni come previsto? Non è così semplice forzare un errore temporaneo si verifica, in particolare quando si esegue in locale e sarebbe particolarmente difficile integrare gli errori temporanei effettivi in un test di unit test automatizzati. Per testare la funzionalità di resilienza di connessione, è necessario un modo per intercettare le query che Entity Framework Invia a SQL Server e sostituire la risposta di SQL Server con un tipo di eccezione che è in genere temporaneo.

È inoltre possibile utilizzare l'intercettazione di query per implementare una procedura consigliata per le applicazioni cloud: [accedere la latenza e l'esito positivo o negativo di tutte le chiamate ai servizi esterni](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) , ad esempio servizi di database. EF6 fornisce un [dedicato API registrazione](https://msdn.microsoft.com/en-us/data/dn469464) che rendono più semplice eseguire la registrazione, ma in questa sezione dell'esercitazione verrà illustrato come utilizzare il Framework di entità [funzionalità di intercettazione](https://msdn.microsoft.com/en-us/data/dn469464) direttamente, sia per registrazione e per la simulazione di errori temporanei.

### <a name="create-a-logging-interface-and-class"></a>Creare una classe e interfaccia di registrazione

Oggetto [consigliata per la registrazione](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) consiste nell'eseguire questa operazione utilizzando un'interfaccia anziché a livello di codice le chiamate a Trace o una classe di registrazione. Che rende più semplice modificare il meccanismo di registrazione in un secondo momento, se è necessario eseguire questa operazione. Pertanto in questa sezione si creerà l'interfaccia di registrazione e una classe per implementare tale/p > 

1. Creare una cartella nel progetto e denominarlo *registrazione*.
2. Nel *registrazione* cartella, creare un file di classe denominato *ILogger.cs*e sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    L'interfaccia fornisce tre livelli di traccia per indicare l'importanza relativa dei log e progettata per fornire informazioni sulla latenza per le chiamate al servizio esterno, ad esempio le query di database. Metodi di registrazione dispongono di overload che consentono di passare un'eccezione. Si tratta in modo che informazioni sulle eccezioni, incluse le eccezioni interna e traccia dello stack viene registrati in modo affidabile tramite la classe che implementa l'interfaccia, invece di basarsi sui cui viene eseguita in ogni chiamata di metodo di registrazione in tutta l'applicazione.

    I metodi TraceApi consentono di tenere traccia della latenza di ogni chiamata a un servizio esterno, ad esempio Database SQL.
3. Nel *registrazione* cartella, creare un file di classe denominato *Logger.cs*e sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    L'implementazione utilizza System. Diagnostics per eseguire la traccia. Questa è una funzionalità incorporata di .NET che rende più semplice generare e usare le informazioni di analisi. Esistono molti "listener" è possibile utilizzare con funzionalità di traccia System. Diagnostics, per scrivere i log per i file, ad esempio, o per scriverli in archiviazione blob di Azure. Alcune opzioni e collegamenti ad altre risorse per altre informazioni, vedere in [risoluzione dei problemi di siti Web di Azure in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Per questa esercitazione verrà esaminato solo i registri di Visual Studio **Output** finestra.

    In un'applicazione di produzione è possibile prendere in considerazione i pacchetti di traccia diverso da System. Diagnostics e l'interfaccia ILogger rende relativamente semplice passare a un meccanismo di traccia diverso se si decide di eseguire tale operazione.

### <a name="create-interceptor-classes"></a>Creare classi dell'intercettore

Successivamente, si creeranno le classi di Entity Framework chiamerà in ogni volta che verrà inviata una query per il database, uno per simulare gli errori temporanei e uno per eseguire la registrazione. Queste classi dell'intercettore devono derivare dalla `DbCommandInterceptor` classe. In tali scrivere override dei metodi che vengono chiamati automaticamente quando sta per essere eseguite query. In questi metodi è possibile esaminare o eseguire la query che verrà inviata al database ed è possibile modificare la query prima che venga inviato al database o restituire un valore a Entity Framework manualmente senza anche passare la query al database.

1. Per creare la classe dell'intercettore che verrà registrati ogni query SQL che viene inviato al database, creare un file di classe denominato *SchoolInterceptorLogging.cs* nel *DAL* cartella, quindi sostituire il modello di codice con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Per la query ha esito positivo o comandi, il codice scrive un file di registro con informazioni sulla latenza. Per le eccezioni, viene creato un log degli errori.
2. Per creare la classe dell'intercettore che genera errori temporanei fittizi quando si immette "Throw" nel **ricerca** casella, creare un file di classe denominato *SchoolInterceptorTransientErrors.cs* nel *DAL* cartella, quindi sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Questo codice esegue l'override solo di `ReaderExecuting` metodo, che viene chiamato per le query che possono restituire più righe di dati. Se si desidera controllare la resilienza di connessione per altri tipi di query, è possibile anche eseguire l'override di `NonQueryExecuting` e `ScalarExecuting` metodi nell'intercettore registrazione effettua.

    Quando si esegue la pagina di studenti e immettere "Throw" come stringa di ricerca, questo codice crea un'eccezione di Database SQL fittizia per il numero di errore pari a 20, un tipo noto per essere in genere è temporaneo. Altri numeri di errore attualmente riconosciuti come oggetto temporaneo sono 64 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 e 40613, ma questi sono soggetti a modifiche nelle nuove versioni del Database SQL.

    Il codice restituisce l'eccezione a Entity Framework, anziché eseguire la query e passando i risultati della query indietro. Eccezione temporanea viene restituito quattro volte e quindi il codice verrà ripristinata la normale procedura di passare la query al database.

    Perché tutto ciò che viene eseguito l'accesso, sarà possibile vedere che Entity Framework tenta di eseguire la query quattro volte prima di essere completata e l'unica differenza nell'applicazione è che richiede più tempo per il rendering di una pagina di risultati della query.

    Il numero massimo di che tentativi di Entity Framework è configurabile; il codice specifica quattro volte perché è il valore predefinito per i criteri di esecuzione del Database SQL. Se si modificano i criteri di esecuzione, è necessario anche modificare qui il codice che specifica quante volte vengono generati errori temporanei. È inoltre possibile modificare il codice per generare più eccezioni in modo da Entity Framework genererà il `RetryLimitExceededException` eccezione.

    Il valore immesso nella casella di ricerca sarà `command.Parameters[0]` e `command.Parameters[1]` (uno viene utilizzato per il nome e uno per il cognome). Quando viene trovato il valore "% Throw %", "Throw" viene sostituito in tali parametri dalla "an", in modo che alcuni studenti verranno trovati e restituiti.

    Si tratta semplicemente un modo pratico per testare la resilienza di connessione in base alle variazioni di alcuni input per l'interfaccia utente dell'applicazione. È anche possibile scrivere codice che genera errori temporanei per tutte le query o aggiornamenti, come descritto più avanti nei commenti sul *DbInterception.Add* metodo.
3. In *Global. asax*, aggiungere il seguente `using` istruzioni:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Aggiungere le righe evidenziate per il `Application_Start` metodo:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Le righe di codice sono gli eventi che generano il codice dell'intercettore da eseguire quando Entity Framework consente di inviare query al database. Si noti che, in quanto sono create classi intercettore separato per la simulazione di errore temporaneo e la registrazione, è possibile in modo indipendente abilitare e disabilitare le.

    È possibile aggiungere intercettatori utilizzando il `DbInterception.Add` in qualsiasi punto nel codice; non deve necessariamente essere il `Application_Start` metodo. Un'altra opzione consiste nell'inserire questo codice nella classe DbConfiguration creato in precedenza per configurare i criteri di esecuzione.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Ovunque inserire questo codice, è possibile evitare eseguire `DbInterception.Add` dell'intercettore stesso più di una volta o si otterrà istanze aggiuntive dell'intercettore. Ad esempio, se si aggiunta due volte l'intercettore di registrazione, si noterà due log per ogni query SQL.

    Gli intercettori vengono eseguiti nell'ordine di registrazione (l'ordine in cui il `DbInterception.Add` metodo viene chiamato). L'ordine può essere rilevante a seconda di ciò che si sta eseguendo nell'intercettore. Ad esempio, un intercettore può modificare il comando SQL che ottiene nel `CommandText` proprietà. Se modifica il comando SQL, l'intercettore successivo verrà visualizzato il comando SQL modificato, non il comando SQL originale.

    Si è scritto il codice di simulazione errore temporaneo in modo da consentire a causa di errori temporanei immettendo un valore diverso nell'interfaccia utente. In alternativa, è possibile scrivere il codice dell'intercettore per generare sempre la sequenza di eccezioni temporanee senza verificare la presenza di un valore di parametro specifico. È quindi possibile aggiungere l'intercettore solo quando si desidera generare errori temporanei. In questo caso, tuttavia, non aggiungere l'intercettore fino a quando una volta completata l'inizializzazione del database. In altre parole, eseguire operazioni di almeno un database, ad esempio una query su uno dei set di entità prima di avviare la generazione di errori temporanei. Entity Framework esegue diverse query durante l'inizializzazione del database e non vengono eseguite in una transazione, in modo errori durante l'inizializzazione potrebbe causare il contesto in uno stato incoerente.

## <a name="test-logging-and-connection-resiliency"></a>Resilienza di connessione e la registrazione di test

1. Premere F5 per eseguire l'applicazione in modalità di debug e quindi fare clic su di **studenti** scheda.
2. Visual Studio esaminare **Output** finestra per visualizzare l'output di traccia. Potrebbe essere necessario scorrere verso l'alto dopo alcuni errori JavaScript per ottenere i log scritti da parte del logger.

    Si noti che è possibile visualizzare le query SQL effettive inviate al database. Vengono visualizzati alcuni query iniziale e i comandi per Entity Framework per iniziare, controllo della versione del database e tabella di cronologia di migrazione (si apprenderà sulle migrazioni nella prossima esercitazione). Viene visualizzata una query per il paging, per scoprire quanti gli studenti sono presenti, e infine per la query che ottiene i dati di studenti.

    ![Registrazione per le query normale](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Nel **studenti** pagina, immettere "Throw" come stringa di ricerca e fare clic su **ricerca**.

    ![Generare una stringa di ricerca](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Si noterà che il browser sembra bloccarsi per alcuni secondi, durante il Entity Framework è ritentare la query più volte. Il primo tentativo si verifica molto rapidamente, quindi il tempo di attesa prima di aumentare prima di ogni tentativo aggiuntiva. Questo processo di attesa più prima che venga chiamato ogni tentativo *backoff esponenziale*.

    Quando la pagina è visualizzato, che mostra gli studenti sono "un" nei relativi nomi, esaminare la finestra di output e si noterà che la stessa query è stata tentata cinque volte, il primo quattro volte restituzione temporanei eccezioni. Per ogni errore temporaneo, verrà visualizzato il log scritto quando si genera l'errore temporaneo nel `SchoolInterceptorTransientErrors` classe ("restituzione errore temporaneo per il comando...") per visualizzare il log scritto quando `SchoolInterceptorLogging` Ottiene l'eccezione.

    ![Output di registrazione con tentativi](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Poiché è stato immesso una stringa di ricerca, la query che restituisce i dati di student con parametri:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Non ci si connette il valore dei parametri, ma è possibile farlo. Se si desidera visualizzare i valori dei parametri, è possibile scrivere il codice di registrazione per ottenere i valori di parametro di `Parameters` proprietà del `DbCommand` oggetto che ottiene i metodi dell'intercettore.

    Si noti che non è possibile ripetere questo test, a meno che non si arresta l'applicazione e riavviarla. Se si desidera essere in grado di resilienza di connessione di test più volte in una singola esecuzione dell'applicazione, è possibile scrivere codice per reimpostare il contatore degli errori in `SchoolInterceptorTransientErrors`.
4. Per visualizzare la differenza la strategia di esecuzione (criteri di ripetizione) rende, commento di `SetExecutionStrategy` riga in *SchoolConfiguration.cs*, eseguire di nuovo la pagina di studenti in modalità di debug e cercare "Throw" nuovamente.

    Questa volta il debugger si arresta la prima eccezione generato immediatamente quando si tenta di eseguire la query come la prima volta.

    ![Eccezione fittizia](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Rimuovere il commento di *SetExecutionStrategy* riga in *SchoolConfiguration.cs*.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato spiegato come abilitare la resilienza delle connessioni e registrare i comandi SQL che compone e invia al database di Entity Framework. Nella prossima esercitazione verrà distribuita l'applicazione a Internet, utilizzare migrazioni Code First per distribuire il database.

Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione ed è stato possibile apportare miglioramenti. È inoltre possibile richiedere nuovi argomenti in [Mostra Me come con codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Sono disponibili collegamenti ad altre risorse di Entity Framework in [accesso ai dati ASP.NET - risorse](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Precedente](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Successivo](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
