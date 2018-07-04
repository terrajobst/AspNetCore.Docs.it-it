---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.NET Web Forms resilienza della connessione e intercettazione dei comandi | Microsoft Docs
author: Erikre
description: Questa esercitazione descrive come modificare un'applicazione di esempio per supportare la resilienza della connessione e intercettazione dei comandi.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 8b8f592c4e79cf9dbe1255c93fac2fe18f1d9409
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368321"
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web Forms resilienza della connessione e intercettazione dei comandi
====================
da [Erik Reitan](https://github.com/Erikre)

In questa esercitazione si modificherà l'applicazione di esempio Wingtip Toys per supportare la resilienza della connessione e intercettazione dei comandi. Abilitando la resilienza di connessione, l'applicazione di esempio Wingtip Toys tenterà automaticamente di ripetere le chiamate di dati quando si verificano errori temporanei tipici di un ambiente cloud. Inoltre, mediante l'implementazione di intercettazione dei comandi, l'applicazione di esempio Wingtip Toys rileverà tutte le query SQL inviate al database per accedere o apportare modifiche.

> [!NOTE] 
> 
> Questa esercitazione Web Form è stata basata sull'esercitazione su MVC di Tom Dykstra seguenti:  
> [Resilienza della connessione e intercettazione dei comandi con Entity Framework in un'applicazione ASP.NET MVC](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Che cosa si apprenderà come:

- Come fornire resilienza della connessione.
- Come implementare l'intercettazione dei comandi.

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, assicurarsi di aver installato nel computer il software seguente:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oppure [Microsoft Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework viene installato automaticamente.
- Progetto di esempio Wingtip Toys, in modo che sia possibile implementare le funzionalità menzionate in questa esercitazione all'interno del progetto di Wingtip Toys. Il collegamento seguente fornisce dettagli del download:

    - [Introduzione ad ASP.NET 4.5.1 Web Form - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (c#)
- Prima di completare questa esercitazione, è consigliabile rivedere la serie di esercitazioni correlata [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Serie di esercitazioni consentono di acquisire familiarità con le **WingtipToys** progetto e il codice.

## <a name="connection-resiliency"></a>Resilienza della connessione

Quando si prende in considerazione la distribuzione di un'applicazione in Azure, il database è la distribuzione di un'opzione da prendere in considerazione **Windows** **Database SQL di Azure**, un servizio di database cloud. Gli errori di connessione temporanei sono in genere più frequenti quando ci si connette a un servizio di database cloud rispetto a quando il server web e il server di database sono direttamente connesse tra loro nello stesso data center. Anche se un server web cloud e un servizio di database cloud sono ospitate nello stesso data center, sono presenti altre connessioni di rete tra di essi che può avere problemi, ad esempio servizi di bilanciamento del carico.

Anche un servizio cloud è in genere condivisi da altri utenti, ovvero che la velocità di risposta può essere influenzata da essi. E l'accesso al database potrà essere soggetti alla limitazione. La limitazione significa che il servizio di database genera eccezioni quando si tenta di accedere più frequentemente di quanto è consentito nel *contratto di servizio* (SLA).

Numero o la maggior parte dei problemi di connessione che si verificano quando si accede a un servizio cloud sono temporanei, vale a dire, risolvono autonomamente in un breve periodo di tempo. Pertanto, quando si tenta un'operazione di database e ottenere un tipo di errore che è in genere temporaneo, è Impossibile ripetere l'operazione dopo un breve periodo di attesa e l'operazione potrebbe essere eseguita correttamente. È possibile fornire migliorare notevolmente la per gli utenti se si gestiscono gli errori temporanei automaticamente tentando nuovamente invisibili la maggior parte di essi al cliente. La funzionalità di resilienza di connessione in Entity Framework 6 consente di automatizzare processo di ripetizione non è stato possibile query SQL.

La funzionalità di resilienza di connessione deve essere configurata in modo appropriato per un servizio di database particolare:

1. È necessario indicare le eccezioni che possono risultare temporanea. Si desidera ripetere gli errori causati da una perdita temporanea della connettività di rete, non gli errori causati da bug del programma, ad esempio.
2. Deve attendere una quantità appropriata di tempo tra i tentativi di un'operazione non riuscita. È possibile attendere più tra i tentativi per un processo batch rispetto a quanto possibile per una pagina web online in cui un utente è in attesa di una risposta.
3. È necessario ripetere un numero appropriato di volte prima di rinunciare. È possibile ripetere più volte in un processo batch che si farebbe in un'applicazione online.

È possibile configurare queste impostazioni manualmente per qualsiasi ambiente di database supportato da un provider di Entity Framework.

Tutto è necessario eseguire per abilitare la resilienza della connessione è creare una classe nell'assembly da cui deriva il `DbConfiguration` classe e impostarne la strategia di esecuzione di Database SQL, che in Entity Framework è un altro termine per i criteri di ripetizione dei tentativi nella classe.

### <a name="implementing-connection-resiliency"></a>Implementare resilienza della connessione

1. Scaricare e aprire il [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) applicazione Web Form in Visual Studio di esempio.
2. Nel *per la logica* cartella della **WingtipToys** dell'applicazione, aggiungere un file di classe denominato *WingtipToysConfiguration.cs*.
3. Sostituire il codice esistente con il seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework esegue automaticamente il codice si trova in una classe che deriva da `DbConfiguration`. È possibile usare la `DbConfiguration` classe per eseguire attività di configurazione nel codice che si farebbe in caso contrario, il *Web. config* file. Per altre informazioni, vedere [EntityFramework configurazione basata su codice](https://msdn.microsoft.com/data/jj680699).

1. Nel *per la logica* cartella, aprire il *AddProducts.cs* file.
2. Aggiungere un `using` istruzione per `System.Data.Entity.Infrastructure` come evidenziato in giallo:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Aggiungere un `catch` bloccare per il `AddProduct` metodo in modo che il `RetryLimitExceededException` viene registrato come evidenziato in giallo:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Aggiungendo il `RetryLimitExceededException` eccezione, è possibile fornire una migliore registrazione o visualizzare un messaggio di errore all'utente in cui è possibile scegliere di riprovare il processo. Operazione rilevando il `RetryLimitExceededException` eccezione, solo gli errori presumibilmente temporaneo sarà già hanno provato e non è riuscito più volte. L'eccezione effettivo restituito verrà arrotondato nel `RetryLimitExceededException` eccezione. Inoltre, inoltre aggiunto un blocco catch generale. Per altre informazioni sul `RetryLimitExceededException` eccezione, vedere [Entity Framework resilienza della connessione / logica di tentativi](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Intercettazione dei comandi

Ora che hai attivato un criterio di ripetizione dei tentativi, come testare per verificare che funzioni come previsto? Non è così facile forzare un errore temporaneo si verifica, in particolare quando si esegue in locale e sarebbe particolarmente difficile da integrare effettivo degli errori temporanei in un test di unit test automatizzati. Per testare la funzionalità di resilienza di connessione, è necessario un modo per intercettare le query che Entity Framework Invia a SQL Server e sostituire la risposta di SQL Server con un tipo di eccezione che è in genere temporaneo.

È anche possibile usare l'intercettazione di query per implementare una procedura consigliata per le applicazioni cloud: log di latenza e l'esito positivo o negativo di tutte le chiamate all'esterno di servizi, ad esempio servizi di database.

In questa sezione dell'esercitazione si userà di Entity Framework [ *funzionalità di intercettazione* ](https://msdn.microsoft.com/data/dn469464) sia per la registrazione per simulare gli errori temporanei.

### <a name="create-a-logging-interface-and-class"></a>Creare una classe e l'interfaccia di registrazione

Una procedura consigliata per la registrazione è eseguire questa operazione tramite un [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) anziché impostare come hardcoded le chiamate a `System.Diagnostics.Trace` o una classe di registrazione. Che rende più semplice modificare il meccanismo di registrazione in un secondo momento, se è necessario eseguire questa operazione. Quindi, in questa sezione si creerà una classe per l'implementazione e l'interfaccia di registrazione.

In base alla procedura descritta sopra, è stato scaricato e aperto il **WingtipToys** applicazione in Visual Studio di esempio.

1. Creare una cartella nel **WingtipToys** del progetto e denominarlo *Logging*.
2. Nel *Logging* cartella, creare un file di classe denominato *ILogger.cs* e sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   L'interfaccia fornisce tre livelli di traccia per indicare l'importanza relativa dei log e uno stato progettato per fornire le informazioni sulla latenza per le chiamate al servizio esterno, ad esempio le query di database. Metodi di registrazione presentano overload che consentono di passare un'eccezione. Si tratta in modo che le informazioni sull'eccezione, compresi stack trace e le eccezioni interne viene registrati in modo affidabile tramite la classe che implementa l'interfaccia, anziché basarsi su esso viene eseguita in ogni chiamata di metodo di registrazione in tutta l'applicazione.  
  
   Il `TraceApi` metodi consentono di tenere traccia della latenza di ogni chiamata a un servizio esterno, ad esempio Database SQL.
3. Nel *Logging* cartella, creare un file di classe denominato *Logger.cs* e sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

L'implementazione Usa `System.Diagnostics` per eseguire la traccia. Si tratta di una funzionalità incorporata di .NET che rende più semplice generare e usare le informazioni di traccia. Esistono molte &quot;listener&quot; è possibile usare con `System.Diagnostics` traccia, per scrivere i log di file, ad esempio, o per scriverli nell'archivio blob di Windows Azure. Alcune delle opzioni e collegamenti ad altre risorse per altre informazioni, vedere nella [risoluzione dei problemi di siti Web di Microsoft Azure in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Per questa esercitazione, esaminati solo i log in Visual Studio **Output** finestra.

In un'applicazione di produzione è possibile provare a usare il framework di traccia diverso da `System.Diagnostics`e il `ILogger` interfaccia rende relativamente semplice passare a un meccanismo di traccia diverso se si decide di eseguire questa operazione.

### <a name="create-interceptor-classes"></a>Creare le classi dell'intercettore

Successivamente, si creerà le classi di Entity Framework chiamerà in ogni volta che provvederà a inviare una query al database, uno per simulare gli errori temporanei e uno per eseguire la registrazione. Queste classi intercettore devono derivare dal `DbCommandInterceptor` classe. In, si scrive gli override dei metodi che vengono chiamati automaticamente quando la query sta per essere eseguita. In questi metodi è possibile esaminare o le query che viene inviata al database di log ed è possibile modificare la query prima che venga inviata al database o restituire un valore Entity Framework manualmente senza nemmeno il passaggio della query al database.

1. Per creare la classe dell'intercettore che registrerà tutte le query SQL prima che venga inviata al database, creare un file di classe denominato *InterceptorLogging.cs* nel *logica* cartella e sostituire il valore predefinito di codice con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Per la query ha esito positivo o comandi, questo codice scrive un file di registro con le informazioni sulla latenza. Per le eccezioni, viene creato un log degli errori.
2. Per creare la classe dell'intercettore che genererà errori temporanei fittizi quando si immette &quot;Throw&quot; nel **Name** casella di testo nella pagina denominata *AdminPage.aspx*, creare una classe file denominato *InterceptorTransientErrors.cs* nel *logica* cartella e sostituire il valore predefinito di codice con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Questo codice esegue l'override solo di `ReaderExecuting` metodo, che viene chiamato per le query che possono restituire più righe di dati. Se si vuole verificare la resilienza di connessione per gli altri tipi di query, è possibile anche eseguire l'override di `NonQueryExecuting` e `ScalarExecuting` metodi, come l'intercettore di registrazione viene.  
  
   In un secondo momento, verrà Accedi come "Admin" e selezionare il **Admin** collegamento sulla barra di spostamento superiore. Quindi, nella *AdminPage.aspx* pagina, si aggiungerà un prodotto denominato &quot;Throw&quot;. Il codice crea un'eccezione del Database SQL fittizia per numero di errore pari a 20, un tipo noto per essere in genere temporaneo. Altri numeri di errore attualmente riconosciuti temporanei sono 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 e 40613, ma questi sono soggetti a modifiche nelle nuove versioni del Database SQL. Il prodotto verrà rinominato in "TransientErrorExample", nel codice di cui è possibile seguire le *InterceptorTransientErrors.cs* file.  
  
   Il codice restituisce l'eccezione a Entity Framework anziché eseguendo la query e passando i risultati. Viene restituita l'eccezione temporaneo *quattro* volte, e quindi il codice ripristina la normale procedura del passaggio della query al database.

    Perché tutto ciò che viene eseguito l'accesso, sarà in grado di visualizzare i che Entity Framework tenta di eseguire la query quattro volte prima di essere completata e l'unica differenza nell'applicazione è che richiede più tempo per il rendering di una pagina con i risultati della query.  
  
   Il numero di tentativi di Entity Framework è configurabile; il codice specifica quattro volte, perché questo è il valore predefinito per i criteri di esecuzione di Database SQL. Se si modificano i criteri di esecuzione, è stato anche modificato il codice che specifica quante volte vengono generati gli errori temporanei. È inoltre possibile modificare il codice per generare più eccezioni in modo che Entity Framework genererà il `RetryLimitExceededException` eccezione.
3. Nelle *Global. asax*, aggiungere quanto segue usando istruzioni:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Quindi, aggiungere le righe evidenziate per il `Application_Start` metodo:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Queste righe di codice sono gli eventi che generano il codice dell'intercettore da eseguire quando Entity Framework di inviare query al database. Si noti che poiché sono create classi di intercettore separato per la simulazione di errore temporaneo e la registrazione, è possibile in modo indipendente abilitare e disabilitarli.   
  
 È possibile aggiungere intercettori tramite il `DbInterception.Add` ovunque nel codice; non deve necessariamente trovarsi nel `Application_Start` (metodo). Un'altra opzione, se non è stato aggiunto intercettori nel `Application_Start` , è possibile aggiornare o aggiungere la classe denominata *WingtipToysConfiguration.cs* e inserire il codice sopra riportato alla fine del costruttore del `WingtipToysbConfiguration` classe.

Quando si inserisce questo codice, prestare attenzione a non eseguire `DbInterception.Add` dell'intercettore stesso più di una volta o si otterrà le istanze aggiuntive dell'intercettore. Ad esempio, se si aggiunta due volte l'intercettore di registrazione, si noterà due log per ogni query SQL.

Gli intercettori vengono eseguiti nell'ordine di registrazione (l'ordine in cui il `DbInterception.Add` viene chiamato). L'ordine può essere rilevante a seconda di ciò che si sta effettuando nell'intercettore. Ad esempio, un intercettore può modificare il comando SQL che ottiene nel `CommandText` proprietà. Se viene modificato il comando SQL, l'intercettore successivo verrà visualizzato il comando SQL modificato, non il comando SQL originale.

Il codice di simulazione di errore temporaneo scritto in modo che è possibile causare errori temporanei, immettere un valore diverso nell'interfaccia utente. In alternativa, è possibile scrivere il codice dell'intercettore per generare sempre la sequenza di eccezioni temporanee senza verificare la presenza di un valore di parametro specifico. È quindi possibile aggiungere l'intercettore solo quando si desidera generare errori temporanei. Se in questo caso, tuttavia, non aggiungere l'intercettore fino a dopo l'inizializzazione del database è stata completata. In altre parole, eseguire un'operazione di almeno un database, ad esempio una query su uno dei set di entità prima di avviare la generazione di errori temporanei. Entity Framework esegue diverse query durante l'inizializzazione del database e che non vengono eseguiti in una transazione, in modo che gli errori durante l'inizializzazione può causare il contesto in uno stato incoerente.

## <a name="test-logging-and-connection-resiliency"></a>Resilienza di connessione e la registrazione dei test

1. In Visual Studio, premere **F5** per eseguire l'applicazione in modalità di debug, quindi eseguire l'accesso come "Admin" parola "Pa$ $" come password.
2. Selezionare **Admin** dalla barra di navigazione nella parte superiore.
3. Immettere un nuovo prodotto denominato "Throw" con file di descrizione, prezzo e image appropriato.
4. Premere il **Aggiungi prodotto** pulsante.  
   Si noterà che il browser sembra bloccarsi per diversi secondi mentre Entity Framework ritenterà l'esecuzione di query più volte. Il primo tentativo avviene molto rapidamente, quindi aumenta il tempo di attesa prima di ogni tentativo aggiuntivo. Questo processo di attesa più prima che venga chiamato ogni nuovo tentativo *backoff esponenziale* .
5. Attendere che la pagina non è più atttempting da caricare.
6. Arresta il progetto ed esaminare il Visual Studio **Output** finestra per visualizzare l'output di traccia. È possibile trovare il **Output** finestra selezionando **Debug**  - &gt; **Windows**  - &gt;  **Output**. Si potrebbe essere necessario scorrere oltre diversi altri log scritto da un logger.  
  
   Si noti che è possibile visualizzare le query SQL inviate al database. Vengono visualizzate alcune query iniziale e i comandi che Entity Framework esegue per iniziare, controllare la tabella di cronologia versione e la migrazione di database.   
    ![Finestra di output](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Si noti che non è possibile ripetere questo test prima di arresta l'applicazione e riavviarlo. Se si desidera essere in grado di resilienza della connessione di test più volte in una singola esecuzione dell'applicazione, è possibile scrivere codice per reimpostare il contatore degli errori in `InterceptorTransientErrors` .
7. Per visualizzare le differenze la strategia di esecuzione (criteri di ripetizione dei tentativi) rende, commento il `SetExecutionStrategy` riga in *WingtipToysConfiguration.cs* del file nei *per la logica* cartella, eseguire il **Admin**  pagina in modalità di debug anche in questo caso e aggiungere il prodotto denominato &quot;Throw&quot; nuovamente.  
  
   Questa volta il debugger si arresta nella prima eccezione generata immediatamente quando si tenta di eseguire la prima volta che la query.  
    ![Debug - Visualizza dettagli](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Rimuovere il commento il `SetExecutionStrategy` riga nel *WingtipToysConfiguration.cs* file.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato spiegato come modificare un'applicazione di esempio Web Form per supportare la resilienza della connessione e intercettazione dei comandi.

## <a name="next-steps"></a>Passaggi successivi

Dopo aver esaminato la resilienza di connessione e intercettazione dei comandi in Web Form ASP.NET, vedere l'argomento di Web Form ASP.NET [metodi asincroni in ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). L'argomento insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET asincrona con Visual Studio.
