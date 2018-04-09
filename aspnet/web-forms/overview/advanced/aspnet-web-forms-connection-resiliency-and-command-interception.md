---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.NET Web Forms resilienza di connessione e comando intercettazione | Documenti Microsoft
author: Erikre
description: In questa esercitazione viene descritto come modificare un'applicazione di esempio per supportare la resilienza di connessione e l'intercettazione di comando.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: d5c4e46209e1b21a303fdf1fb16c6c868b3ca923
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Resilienza di connessione di ASP.NET Web Form e l'intercettazione di comando
====================
da [Erik Reitan](https://github.com/Erikre)

In questa esercitazione si modificherà l'applicazione di esempio Wingtip Toys per supportare la resilienza di connessione e l'intercettazione di comando. Abilita la resilienza di connessione, l'applicazione di esempio Wingtip Toys tenterà automaticamente di ripetere le chiamate di dati quando si verificano errori temporanei tipici di un ambiente cloud. Inoltre, implementando l'intercettazione di comando, l'applicazione di esempio Wingtip Toys intercetterà tutte le query SQL inviate al database per accedere o apportare modifiche.

> [!NOTE] 
> 
> In questa esercitazione di Web Form è basata su seguente MVC esercitazione di Tom Dykstra:  
> [Resilienza di connessione e comando di intercettazione con Entity Framework in un'applicazione MVC ASP.NET](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Illustra quanto segue:

- Come fornire resilienza di connessione.
- Come implementare l'intercettazione di comando.

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, assicurarsi di aver installato nel computer il software seguente:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework viene installato automaticamente.
- Progetto di esempio Wingtip Toys in modo che è possibile implementare le funzionalità indicate in questa esercitazione all'interno del progetto Wingtip Toys. Il collegamento seguente fornisce dettagli sul download:

    - [Getting Started with ASP.NET 4.5.1 Web Form - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (c#)
- Prima del completamento di questa esercitazione, è consigliabile rivedere l'esercitazione serie correlate, [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). La serie di esercitazioni consentono di acquisire familiarità con la **WingtipToys** codice e il progetto.

## <a name="connection-resiliency"></a>Resilienza della connessione

Quando si prevede di distribuire un'applicazione in Windows Azure, si consiglia di provare la distribuzione di database da **Windows** **Database SQL di Azure**, un servizio di database cloud. Errori di connessione temporanei sono in genere più di frequenti quando ci si connette a un servizio di database cloud rispetto a quando il server web e il server di database vengono connessi direttamente insieme nello stesso data center. Anche se un server web di cloud e un servizio cloud di database sono ospitati nello stesso data center, sono presenti più connessioni di rete tra di essi che può avere problemi, ad esempio servizi di bilanciamento del carico.

Anche un servizio cloud è in genere condivisi da altri utenti, che indica che i tempi di risposta può essere influenzato da essi. E l'accesso al database potrebbe essere soggetto a limitazione. La limitazione delle richieste indica il servizio di database genera eccezioni quando si tenta di accedervi più frequentemente di quanto è consentito nel *Service Level Agreement* (SLA).

Numero o la maggior parte dei problemi di connessione che si verificano quando si accede a un servizio cloud sono temporanei, vale a dire si risolvono spontaneamente in un breve periodo di tempo. Pertanto, quando si tenta un'operazione di database e ottenere un tipo di errore che è in genere temporaneo, è Impossibile ripetere l'operazione dopo un breve periodo di attesa e l'operazione potrebbe essere eseguita correttamente. È possibile fornire una migliore esperienza per gli utenti se si gestiscono gli errori temporanei automaticamente tentando nuovamente invisibili la maggior parte di essi al cliente. Consente di automatizzare la funzionalità di resilienza di connessione in Entity Framework 6 processo di ripetizione non è stato possibile query SQL.

La funzionalità di resilienza di connessione deve essere configurata correttamente per un servizio di database specifico:

1. È necessario indicare le eccezioni che possono essere temporanei. Si desidera tentare di errori causati da una perdita temporanea della connettività di rete, non gli errori causati da errori di programma, ad esempio.
2. È necessario attendere un periodo di tempo tra i tentativi di un'operazione non riuscita appropriato. È possibile attendere più tra i tentativi per un processo batch rispetto a quanto possibile per una pagina web online in cui un utente è in attesa di una risposta.
3. È necessario ripetere un numero appropriato di volte prima di rinunciare. Si potrebbe voler ripetere più volte in un processo batch che ci si trovasse in un'applicazione in linea.

È possibile configurare queste impostazioni manualmente per un ambiente di database supportate da un provider di Entity Framework.

Per abilitare la resilienza di connessione è creare una classe nell'assembly da cui deriva il `DbConfiguration` e in tale classe impostare la strategia di esecuzione di Database SQL, che in Entity Framework è un altro termine per i criteri di ripetizione.

### <a name="implementing-connection-resiliency"></a>Implementazione di resilienza di connessione

1. Scaricare e aprire il [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) applicazione Web Form in Visual Studio di esempio.
2. Nel *logica* cartella la **WingtipToys** applicazione, aggiungere un file di classe denominato *WingtipToysConfiguration.cs*.
3. Sostituire il codice esistente con il seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework esegue automaticamente il codice si trova in una classe che deriva da `DbConfiguration`. È possibile utilizzare il `DbConfiguration` classe per eseguire attività di configurazione nel codice che si procede in caso contrario il *Web. config* file. Per ulteriori informazioni, vedere [EntityFramework basato sul codice di configurazione](https://msdn.microsoft.com/data/jj680699).

1. Nel *logica* cartella, aprire il *AddProducts.cs* file.
2. Aggiungere un `using` istruzione per `System.Data.Entity.Infrastructure` evidenziati in giallo:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Aggiungere un `catch` blocco per il `AddProduct` metodo in modo che il `RetryLimitExceededException` viene registrato come evidenziato in giallo:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Aggiungendo il `RetryLimitExceededException` eccezione, è possibile fornire una migliore registrazione o visualizzare un messaggio di errore all'utente in cui è possibile scegliere di tentare nuovamente il processo. Rilevando il `RetryLimitExceededException` eccezione, solo gli errori potrebbe essere temporaneo verrà già hanno tentato e non è riuscito più volte. Ha restituito l'eccezione verrà incapsulato nel `RetryLimitExceededException` eccezione. È inoltre aggiunto un blocco catch generale. Per ulteriori informazioni sul `RetryLimitExceededException` eccezione, vedere [resilienza di connessione Entity Framework o logica di tentativi](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Intercettazione di comando

Ora che hai attivato in un criterio di ripetizione, come test per verificare che funzioni come previsto? Non è così semplice forzare un errore temporaneo si verifica, in particolare quando si esegue in locale e sarebbe particolarmente difficile integrare gli errori temporanei effettivi in un test di unit test automatizzati. Per testare la funzionalità di resilienza di connessione, è necessario un modo per intercettare le query che Entity Framework Invia a SQL Server e sostituire la risposta di SQL Server con un tipo di eccezione che è in genere temporaneo.

È inoltre possibile utilizzare l'intercettazione di query per implementare una procedura consigliata per le applicazioni cloud: log chiama all'esterno di latenza e l'esito positivo o negativo di tutti i servizi, ad esempio servizi di database.

In questa sezione dell'esercitazione si userà l'Entity Framework [ *funzionalità di intercettazione* ](https://msdn.microsoft.com/data/dn469464) per la registrazione e per la simulazione di errori temporanei.

### <a name="create-a-logging-interface-and-class"></a>Creare una classe e interfaccia di registrazione

È una procedura consigliata per la registrazione per eseguire questa operazione utilizzando un [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) anziché chiamate hardcoded a `System.Diagnostics.Trace` o una classe di registrazione. Che rende più semplice modificare il meccanismo di registrazione in un secondo momento, se è necessario eseguire questa operazione. Pertanto in questa sezione si creerà una classe per l'implementazione e interfaccia di registrazione.

In base alla procedura descritta sopra, aver scaricato e aperto il **WingtipToys** applicazione in Visual Studio di esempio.

1. Creare una cartella di **WingtipToys** del progetto e denominarlo *registrazione*.
2. Nel *registrazione* cartella, creare un file di classe denominato *ILogger.cs* e sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   L'interfaccia fornisce tre livelli di traccia per indicare l'importanza relativa dei log e progettata per fornire informazioni sulla latenza per le chiamate al servizio esterno, ad esempio le query di database. Metodi di registrazione dispongono di overload che consentono di passare un'eccezione. Si tratta in modo che informazioni sulle eccezioni, incluse le eccezioni interna e traccia dello stack viene registrati in modo affidabile tramite la classe che implementa l'interfaccia, invece di basarsi sui cui viene eseguita in ogni chiamata di metodo di registrazione in tutta l'applicazione.  
  
   Il `TraceApi` metodi consentono di tenere traccia della latenza di ogni chiamata a un servizio esterno, ad esempio Database SQL.
3. Nel *registrazione* cartella, creare un file di classe denominato *Logger.cs* e sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

L'implementazione Usa `System.Diagnostics` per eseguire la traccia. Questa è una funzionalità incorporata di .NET che rende più semplice generare e usare le informazioni di analisi. Esistono molti &quot;listener&quot; è possibile utilizzare con `System.Diagnostics` traccia, per scrivere i log per i file, ad esempio, o per scriverli in archiviazione blob di Windows Azure. Alcune opzioni e collegamenti ad altre risorse per altre informazioni, vedere in [risoluzione dei problemi di siti Web di Windows Azure in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Per questa esercitazione, sarà solo esaminare i log in Visual Studio **Output** finestra.

In un'applicazione di produzione è consigliabile considerare l'uso di Framework di traccia diverso da `System.Diagnostics`e `ILogger` interfaccia rende relativamente semplice passare a un meccanismo di traccia diverso se si decide di eseguire tale operazione.

### <a name="create-interceptor-classes"></a>Creare classi dell'intercettore

Successivamente, si creeranno le classi di Entity Framework chiamerà in ogni volta che verrà inviata una query per il database, uno per simulare gli errori temporanei e uno per eseguire la registrazione. Queste classi dell'intercettore devono derivare dalla `DbCommandInterceptor` classe. In, viene scritto l'override dei metodi che vengono chiamati automaticamente quando la query deve essere eseguita. In questi metodi è possibile esaminare o eseguire la query che verrà inviata al database ed è possibile modificare la query prima che venga inviato al database o restituire un valore a Entity Framework manualmente senza anche passare la query al database.

1. Per creare la classe dell'intercettore che verrà registrati ogni query SQL prima che venga inviato al database, creare un file di classe denominato *InterceptorLogging.cs* nel *logica* cartella e sostituire il valore predefinito di codice con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Per la query ha esito positivo o comandi, il codice scrive un file di registro con informazioni sulla latenza. Per le eccezioni, viene creato un log degli errori.
2. Per creare la classe dell'intercettore che genera errori temporanei fittizi quando si immette &quot;generare&quot; nel **nome** casella di testo nella pagina denominata *AdminPage.aspx*, creare una classe file denominato *InterceptorTransientErrors.cs* nel *logica* cartella e sostituire il valore predefinito di codice con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Questo codice esegue l'override solo di `ReaderExecuting` metodo, che viene chiamato per le query che possono restituire più righe di dati. Se si desidera controllare la resilienza di connessione per altri tipi di query, è possibile anche eseguire l'override di `NonQueryExecuting` e `ScalarExecuting` metodi nell'intercettore registrazione effettua.  
  
   In un secondo momento, accedere come "Admin" e selezionare il **Admin** collegamento sulla barra di spostamento superiore. Scegliere il *AdminPage.aspx* pagina si aggiungerà un prodotto denominato &quot;generare&quot;. Il codice crea un'eccezione di Database SQL fittizia per il numero di errore pari a 20, un tipo noto per essere in genere è temporaneo. Altri numeri di errore attualmente riconosciuti come oggetto temporaneo sono 64 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 e 40613, ma questi sono soggetti a modifiche nelle nuove versioni del Database SQL. Il prodotto verrà rinominato in "TransientErrorExample", nel codice di cui è possibile seguire il *InterceptorTransientErrors.cs* file.  
  
   Il codice restituisce l'eccezione a Entity Framework, anziché eseguire la query e passando risultati indietro. Viene restituita l'eccezione temporaneo *quattro* volte, e quindi il codice verrà ripristinata la normale procedura di passare la query al database.

    Perché tutto ciò che viene eseguito l'accesso, sarà possibile vedere che Entity Framework tenta di eseguire la query quattro volte prima di essere completata e l'unica differenza nell'applicazione è che richiede più tempo per il rendering di una pagina di risultati della query.  
  
   Il numero massimo di che tentativi di Entity Framework è configurabile; il codice specifica quattro volte perché è il valore predefinito per i criteri di esecuzione del Database SQL. Se si modificano i criteri di esecuzione, è necessario anche modificare qui il codice che specifica quante volte vengono generati errori temporanei. È inoltre possibile modificare il codice per generare più eccezioni in modo da Entity Framework genererà il `RetryLimitExceededException` eccezione.
3. In *Global. asax*, aggiungere le seguenti istruzioni using:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Quindi, aggiungere le righe evidenziate per il `Application_Start` metodo:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Le righe di codice sono gli eventi che generano il codice dell'intercettore da eseguire quando Entity Framework consente di inviare query al database. Si noti che, in quanto sono create classi intercettore separato per la simulazione di errore temporaneo e la registrazione, è possibile in modo indipendente abilitare e disabilitare le.   
  
 È possibile aggiungere intercettatori utilizzando il `DbInterception.Add` in qualsiasi punto nel codice; non deve necessariamente essere il `Application_Start` metodo. Un'altra opzione, se non sono state aggiunte intercettori nel `Application_Start` , è possibile aggiornare o aggiungere la classe denominata *WingtipToysConfiguration.cs* e inserire il codice sopra riportato alla fine del costruttore del `WingtipToysbConfiguration` classe.

Ovunque inserire questo codice, è possibile evitare eseguire `DbInterception.Add` dell'intercettore stesso più di una volta o si otterrà istanze aggiuntive dell'intercettore. Ad esempio, se si aggiunta due volte l'intercettore di registrazione, si noterà due log per ogni query SQL.

Gli intercettori vengono eseguiti nell'ordine di registrazione (l'ordine in cui il `DbInterception.Add` metodo viene chiamato). L'ordine può essere rilevante a seconda di ciò che si sta eseguendo nell'intercettore. Ad esempio, un intercettore può modificare il comando SQL che ottiene nel `CommandText` proprietà. Se modifica il comando SQL, l'intercettore successivo verrà visualizzato il comando SQL modificato, non il comando SQL originale.

Si è scritto il codice di simulazione errore temporaneo in modo da consentire a causa di errori temporanei immettendo un valore diverso nell'interfaccia utente. In alternativa, è possibile scrivere il codice dell'intercettore per generare sempre la sequenza di eccezioni temporanee senza verificare la presenza di un valore di parametro specifico. È quindi possibile aggiungere l'intercettore solo quando si desidera generare errori temporanei. In questo caso, tuttavia, non aggiungere l'intercettore fino a quando una volta completata l'inizializzazione del database. In altre parole, eseguire operazioni di almeno un database, ad esempio una query su uno dei set di entità prima di avviare la generazione di errori temporanei. Entity Framework esegue diverse query durante l'inizializzazione del database e non vengono eseguite in una transazione, in modo errori durante l'inizializzazione potrebbe causare il contesto in uno stato incoerente.

## <a name="test-logging-and-connection-resiliency"></a>Resilienza di connessione e la registrazione di test

1. In Visual Studio, premere **F5** per eseguire l'applicazione in modalità di debug, quindi effettuare l'accesso come "Admin" con "Pa$ $word" come password.
2. Selezionare **Admin** dalla barra di navigazione nella parte superiore.
3. Immettere un nuovo prodotto denominato "Throw" con file di descrizione, prezzo e image appropriato.
4. Premere il **Add Product** pulsante.  
   Si noterà che il browser sembra bloccarsi per alcuni secondi, durante il Entity Framework è ritentare la query più volte. Il primo tentativo avviene molto rapidamente, quindi aumenta il tempo di attesa prima di ogni tentativo aggiuntiva. Questo processo di attesa più prima che venga chiamato ogni tentativo *backoff esponenziale* .
5. Attendere che la pagina non è più atttempting da caricare.
6. Arresta il progetto ed esaminare di Visual Studio **Output** finestra per visualizzare l'output di traccia. È possibile trovare il **Output** selezionando **Debug**  - &gt; **Windows**  - &gt;  **Output**. Potrebbe essere necessario scorrere oltre diversi altri log scritto da parte del logger.  
  
   Si noti che è possibile visualizzare le query SQL effettive inviate al database. Vedrai alcune query iniziale e i comandi per Entity Framework per iniziare, verificare la tabella di cronologia versione e la migrazione di database.   
    ![Finestra di output](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Si noti che non è possibile ripetere questo test, a meno che non si arresta l'applicazione e riavviarla. Se si desidera essere in grado di resilienza di connessione di test più volte in una singola esecuzione dell'applicazione, è possibile scrivere codice per reimpostare il contatore degli errori in `InterceptorTransientErrors` .
7. Per visualizzare la differenza la strategia di esecuzione (criteri di ripetizione) rende, commento di `SetExecutionStrategy` riga in *WingtipToysConfiguration.cs* file nel *logica* cartella, eseguire il **Admin**  pagina in modalità di debug, nuovo, quindi aggiungere il prodotto denominato &quot;generare&quot; nuovamente.  
  
   Questa volta il debugger si arresta la prima eccezione generato immediatamente quando si tenta di eseguire la query come la prima volta.  
    ![Debug - Visualizza dettagli](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Rimuovere il commento di `SetExecutionStrategy` riga il *WingtipToysConfiguration.cs* file.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato spiegato come modificare un'applicazione Web Form di esempio per supportare la resilienza di connessione e l'intercettazione di comando.

## <a name="next-steps"></a>Passaggi successivi

Dopo aver esaminato la resilienza di connessione e l'intercettazione di comando in Web Form ASP.NET, vedere l'argomento di Web Form ASP.NET [metodi asincroni in ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). L'argomento verrà fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET asincrona utilizzando Visual Studio.
