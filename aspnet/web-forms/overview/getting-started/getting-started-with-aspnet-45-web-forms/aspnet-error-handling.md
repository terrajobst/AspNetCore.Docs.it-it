---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Gestione degli errori di ASP.NET | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Microsoft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 4db91929b78e882b2a8a95fb36ba97db4d29d240
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392724"
---
<a name="aspnet-error-handling"></a>Gestione degli errori di ASP.NET
====================
da [Erik Reitan](https://github.com/Erikre)

[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento a questa serie di esercitazioni è disponibile.


In questa esercitazione si modificherà l'applicazione di esempio Wingtip Toys per includere la gestione degli errori e registrazione degli errori. Gestione degli errori consentirà all'applicazione gestire gli errori correttamente e visualizzare i messaggi di errore di conseguenza. La registrazione degli errori consentirà di trovare e correggere gli errori che si sono verificati. Questa esercitazione si basa sull'esercitazione precedente "URL di Routing" e fa parte della serie di esercitazioni di Wingtip Toys.

## <a name="what-youll-learn"></a>Che cosa si apprenderà come:

- Come aggiungere globale gestione degli errori per la configurazione dell'applicazione.
- Come aggiungere a livello di applicazione, pagina e codice di gestione degli errori.
- Come registrare gli errori per una revisione successiva.
- Come visualizzare i messaggi di errore non compromettano la sicurezza.
- Come implementare la registrazione degli errori Error Logging Modules and gestori (ELMAH).

## <a name="overview"></a>Panoramica

Le applicazioni ASP.NET devono essere in grado di gestire gli errori che si verificano durante l'esecuzione in modo coerente. ASP.NET utilizza common language runtime (CLR), che fornisce un modo per notificare alle applicazioni di errori in modo uniforme. Quando si verifica un errore, viene generata un'eccezione. Un'eccezione è qualsiasi errore, condizione o un comportamento imprevisto che un'applicazione si verifica.

In .NET Framework, un'eccezione è un oggetto che eredita dalla classe `System.Exception`. Le eccezioni vengono generate dalle aree di codice in cui si è verificato un problema. L'eccezione viene passato lo stack di chiamate in una posizione in cui l'applicazione fornisce il codice per gestire l'eccezione. Se l'applicazione non gestisce l'eccezione, viene forzato il browser per visualizzare i dettagli dell'errore.

Come procedura consigliata, gestire gli errori in a livello di codice nel `Try` / `Catch` / `Finally` blocchi all'interno del codice. Provare a posizionare questi blocchi in modo che l'utente può risolvere i problemi nel contesto in cui si verificano. Se i blocchi di gestione degli errori sono troppo lontano da dove si è verificato l'errore, diventa più difficile fornire agli utenti con le informazioni necessarie per risolvere il problema.

### <a name="exception-class"></a>Classe di eccezione

La classe di eccezione è la classe di base da cui vengono ereditate le eccezioni. La maggior parte degli oggetti di eccezione sono le istanze di classi derivate della classe di eccezione, ad esempio la `SystemException` (classe), il `IndexOutOfRangeException` (classe), o `ArgumentNullException` classe. La classe Exception presenta le proprietà, ad esempio la `StackTrace` proprietà, il `InnerException` proprietà e il `Message` proprietà, che forniscono informazioni specifiche sull'errore che si è verificato.

### <a name="exception-inheritance-hierarchy"></a>Gerarchia di ereditarietà di eccezione

Il runtime dispone di un set di base di eccezioni derivate dal `SystemException` classe che il runtime genera un'eccezione quando viene rilevata un'eccezione. La maggior parte delle classi che ereditano dalla classe di eccezione, ad esempio la `IndexOutOfRangeException` classi e `ArgumentNullException` classe, non implementano membri aggiuntivi. Di conseguenza, le informazioni più importanti per un'eccezione sono reperibile nella gerarchia di eccezioni, il nome dell'eccezione e le informazioni contenute nell'eccezione.

### <a name="exception-handling-hierarchy"></a>Gerarchia di gestione delle eccezioni

In un'applicazione Web Form ASP.NET, le eccezioni possono essere gestite in base a una gerarchia di gestione specifica. Un'eccezione può essere gestita ai livelli seguenti:

- Livello di applicazione
- A livello di pagina
- Livello di codice

Quando un'applicazione gestisce le eccezioni, informazioni aggiuntive sull'eccezione che viene ereditata dalla classe di eccezione possono spesso essere recuperate e visualizzate all'utente. Oltre all'applicazione, pagina e a livello di codice, è anche possibile gestire le eccezioni a livello di modulo HTTP e tramite un gestore personalizzato di IIS.

### <a name="application-level-error-handling"></a>Gestione degli errori a livello applicazione

È possibile gestire gli errori di impostazione predefinita a livello di applicazione modificando la configurazione dell'applicazione o aggiungendo un `Application_Error` gestore nel *Global. asax* file dell'applicazione.

È possibile gestire gli errori di impostazione predefinita e gli errori HTTP mediante l'aggiunta di un `customErrors` sezione il *Web. config* file. Il `customErrors` sezione consente di specificare una pagina predefinita che gli utenti verranno reindirizzati al caso di errore. Consente inoltre di specificare le singole pagine per gli errori di codice di stato specifici.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Sfortunatamente, quando si usa la configurazione per reindirizzare l'utente a una pagina diversa, non è i dettagli dell'errore che si è verificato.

Tuttavia, è possibile registrare gli errori che si verificano in un punto qualsiasi nell'applicazione aggiungendo il codice per il `Application_Error` gestore nel *Global. asax* file.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Gestione degli eventi di errore a livello di pagina

Un gestore a livello di pagina restituisce l'utente alla pagina in cui si è verificato l'errore, ma poiché non vengono mantenute le istanze dei controlli, non è più esisterà alcuna operazione nella pagina. Per fornire i dettagli dell'errore all'utente dell'applicazione, è necessario scrivere in modo specifico i dettagli dell'errore per la pagina.

È in genere utilizzato un gestore degli errori a livello di pagina per registrare gli errori non gestiti o per richiedere all'utente di una pagina che può visualizzare informazioni utili.

Questo esempio di codice viene illustrato un gestore per l'evento di errore in una pagina Web ASP.NET. Questo handler cattura tutte le eccezioni che non sono già gestite all'interno `try` / `catch` blocchi nella pagina.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Dopo avere gestito un errore, è necessario deselezionare chiamando il `ClearError` metodo dell'oggetto Server (`HttpServerUtility` classe), in caso contrario, verrà visualizzato un errore che in precedenza si è verificato.

### <a name="code-level-error-handling"></a>Gestione degli errori a livello di codice

L'istruzione try-catch è costituita da un blocco try è seguito da uno o più clausole catch che specificano i gestori per eccezioni diverse. Quando viene generata un'eccezione, common language runtime (CLR) Cerca l'istruzione catch che gestisce questa eccezione. Se il metodo attualmente in esecuzione non contiene un blocco catch, CLR cerca il metodo che ha chiamato il metodo corrente e così via, lo stack di chiamate. Se non viene trovato alcun blocco catch, CLR visualizza un messaggio di eccezione non gestita all'utente e interrompe l'esecuzione del programma.

Esempio di codice seguente illustra un modo comune di utilizzo `try` / `catch` / `finally` per gestire gli errori.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

Nel codice precedente, il blocco try contiene il codice che deve essere protetto da una possibile eccezione. Il blocco viene eseguito finché non viene generata un'eccezione o il blocco viene completato correttamente. Se un `FileNotFoundException` eccezione o un `IOException` si verifica un'eccezione, l'esecuzione viene trasferito a un'altra pagina. Quindi, il codice contenuto nel blocco finally viene eseguito, se si è verificato un errore o non.

## <a name="adding-error-logging-support"></a>Aggiunta del supporto per la registrazione errore

Prima di aggiungere all'applicazione di esempio Wingtip Toys di gestione degli errori, si aggiungerà il supporto di registrazione degli errori tramite l'aggiunta di un `ExceptionUtility` classe per il *logica* cartella. In questo modo, ogni volta che l'applicazione gestisce un errore, i dettagli dell'errore verranno aggiunto nel file di log degli errori.

1. Fare doppio clic il *per la logica* cartella e quindi selezionare **Add**  - &gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **codice** gruppo di modelli a sinistra. Quindi, selezionare **classe**dalla parte centrale elencare e denominarlo **ExceptionUtility.cs**.
3. Scegliere **Aggiungi**. Viene visualizzato il nuovo file di classe.
4. Sostituire il codice esistente con quello seguente:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Se si verifica un'eccezione, l'eccezione può essere scritto in un file di log eccezione chiamando il `LogException` (metodo). Questo metodo accetta due parametri, l'oggetto eccezione e una stringa che contiene dettagli sull'origine dell'eccezione. Il log dell'eccezione viene scritto il *Errorlog* del file nel *App\_dati* cartella.

### <a name="adding-an-error-page"></a>Aggiunta di una pagina di errore

Nell'applicazione di esempio Wingtip Toys, verrà utilizzata una tabella per visualizzare gli errori. La pagina di errore è progettata per visualizzare un messaggio di errore sicuro agli utenti del sito. Tuttavia, se l'utente è uno sviluppatore effettua una richiesta HTTP resi disponibili localmente nel computer in cui si trova il codice, dettagli aggiuntivi sull'errore verrà visualizzato nella pagina di errore.

1. Fare clic sul nome del progetto (**Wingtip Toys**) nel **Esplora soluzioni** e selezionare **Add**  - &gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Nell'elenco al centro, selezionare **Web Form con pagina Master**e denominarla **ErrorPage**.
3. Fare clic su **Aggiungi**.
4. Selezionare il *Site. master* del file della pagina master e quindi scegliere **OK**.
5. Sostituire il markup esistente con il codice seguente:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Sostituire il codice di code-behind esistente (*ErrorPage.aspx.cs*) in modo che venga visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Quando viene visualizzata la pagina di errore, il `Page_Load` viene eseguito il gestore eventi. Nel `Page_Load` gestore, viene determinato il percorso di in cui l'errore prima di tutto è stato gestito. Quindi, l'ultimo errore che si è verificato è determinato dalla chiamata di `GetLastError` metodo dell'oggetto Server. Se l'eccezione non esiste più, viene creata un'eccezione generica. Quindi, se la richiesta HTTP è stata eseguita in locale, vengono visualizzati tutti i dettagli dell'errore. In questo caso, solo il computer locale che esegue l'applicazione web verranno visualizzati questi dettagli di errore. Dopo che le informazioni sull'errore è stati visualizzati, l'errore viene aggiunto al file di log e l'errore viene cancellato dal server.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Visualizzazione dei messaggi di errore non gestito per l'applicazione

Aggiungendo un `customErrors` sezione il *Web. config* file, è possibile gestire rapidamente gli errori semplici che si verificano in tutta l'applicazione. È anche possibile specificare come gestire gli errori in base al relativo valore codice di stato, ad esempio 404 - File non trovato.

#### <a name="update-the-configuration"></a>Aggiornare la configurazione

Aggiornare la configurazione aggiungendo un `customErrors` sezione il *Web. config* file.

1. Nelle **Esplora soluzioni**individuare e aprire il *Web. config* file nella radice dell'applicazione di esempio Wingtip Toys.
2. Aggiungere il `customErrors` sezione per il *Web. config* file all'interno di `<system.web>` nodo come indicato di seguito:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Salvare il *Web. config* file.

Il `customErrors` sezione specifica la modalità che è impostata su "Sì". Specifica inoltre il `defaultRedirect`, che indica quale pagina a cui passare quando si verifica un errore all'applicazione. Inoltre, è stato aggiunto un elemento di errore specifico che specifica come gestire un errore 404 quando una pagina non viene trovata. Più avanti in questa esercitazione si aggiungerà ulteriore errore, gestione, che consente di acquisire i dettagli di un errore a livello di applicazione.

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione adesso per visualizzare le route aggiornate.

1. Premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 Il browser si apre e Mostra le *default. aspx* pagina.
2. Immettere l'URL seguente nel browser (assicurarsi di usare **il** il numero di porta):  
    `https://localhost:44300/NoPage.aspx`
3. Rivedere le *ErrorPage* visualizzata nel browser. 

    ![Gestione degli errori di ASP.NET: errore di pagina non trovata](aspnet-error-handling/_static/image1.png)

Quando si richiede la *NoPage.aspx* pagina, che non esiste, la pagina di errore verrà visualizzato il messaggio di errore semplice e le informazioni dettagliate sull'errore se sono disponibili dettagli aggiuntivi. Tuttavia, se l'utente ha richiesto una pagina inesistente da una posizione remota, la pagina di errore visualizzerà solo il messaggio di errore in rosso.

### <a name="including-an-exception-for-testing-purposes"></a>Tra cui un'eccezione per scopi di test

Per verificare come funzionerà l'applicazione quando un errore si verifica, è possibile creare condizioni di errore deliberatamente in ASP.NET. Nell'applicazione di esempio Wingtip Toys, si genererà un'eccezione di test quando viene caricata la pagina predefinita per vedere cosa succede.

1. Aprire il code-behind del *default. aspx* pagina in Visual Studio.   
   Il *Default.aspx.cs* verrà visualizzata la pagina code-behind.
2. Nel `Page_Load` gestore, aggiungere il codice in modo che il gestore viene visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

È possibile creare diversi tipi di eccezioni. Nel codice precedente, si sta creando un `InvalidOperationException` quando il *default. aspx* pagina viene caricata.

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione per vedere come l'applicazione gestisce l'eccezione.

1. Premere **CTRL+F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 L'applicazione genera un'eccezione InvalidOperationException. 

    > [!NOTE] 
    > 
    > È necessario premere **CTRL+F5** per visualizzare la pagina senza interrompere il codice per visualizzare l'origine dell'errore in Visual Studio.
2. Rivedere le *ErrorPage* visualizzata nel browser. 

    ![Gestione degli errori di ASP.NET: pagina di errore](aspnet-error-handling/_static/image2.png)

Come può vedere nei dettagli dell'errore, l'eccezione è stata intercettata dal `customError` sezione il *Web. config* file.

### <a name="adding-application-level-error-handling"></a>Aggiunta di gestione degli errori a livello di applicazione

Invece di trap l'eccezione utilizzando il `customErrors` sezione il *Web. config* file, in cui ottenere informazioni limitate sull'eccezione, è possibile intercettare l'errore a livello di applicazione e recuperare i dettagli dell'errore.

1. Nelle **Esplora soluzioni**individuare e aprire il *Global.asax.cs* file.
2. Aggiungere un **Application\_errore** gestore in modo che venga visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Quando si verifica un errore nell'applicazione, il `Application_Error` gestore viene chiamato. In questo gestore, l'ultima eccezione viene recuperato ed esaminato. Se l'eccezione non gestita e l'eccezione contiene i dettagli dell'eccezione interna (vale a dire, `InnerException` non null), l'applicazione trasferisce l'esecuzione alla pagina di errore vengono visualizzati i dettagli dell'eccezione.

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione per vedere i dettagli dell'errore aggiuntivi forniti da Gestione dell'eccezione a livello di applicazione.

1. Premere **CTRL+F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 L'applicazione genera il `InvalidOperationException` .
2. Rivedere le *ErrorPage* visualizzata nel browser. 

    ![Gestione degli errori di ASP.NET: errore di livello applicazione](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Aggiunta di gestione degli errori a livello di pagina

È possibile aggiungere la gestione degli errori a livello di pagina a una pagina tramite l'aggiunta di un `ErrorPage` attributo la `@Page` direttiva della pagina, oppure aggiungendo un `Page_Error` gestore dell'evento per il code-behind di una pagina. In questa sezione si aggiungerà un `Page_Error` gestore eventi che verrà trasferiti esecuzione per il *ErrorPage* pagina.

1. Nelle **Esplora soluzioni**individuare e aprire il *Default.aspx.cs* file.
2. Aggiungere un `Page_Error` gestore in modo che il code-behind viene visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Quando si verifica un errore nella pagina di `Page_Error` gestore eventi viene chiamato. In questo gestore, l'ultima eccezione viene recuperato ed esaminato. Se un' `InvalidOperationException` si verifica, il `Page_Error` gestore dell'evento trasferisce l'esecuzione alla pagina di errore vengono visualizzati i dettagli dell'eccezione.

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione adesso per visualizzare le route aggiornate.

1. Premere **CTRL+F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 L'applicazione genera il `InvalidOperationException` .
2. Rivedere le *ErrorPage* visualizzata nel browser. 

    ![Gestione degli errori di ASP.NET - errore a livello di pagina](aspnet-error-handling/_static/image4.png)
3. Chiudere la finestra del browser.

### <a name="removing-the-exception-used-for-testing"></a>La rimozione eccezione utilizzata per il test

Per consentire l'applicazione di esempio Wingtip Toys a funzionare senza generazione dell'eccezione che è stato aggiunto in precedenza in questa esercitazione, la rimozione dell'eccezione.

1. Aprire il code-behind del *default. aspx* pagina.
2. Nel `Page_Load` gestore, rimuovere il codice che genera l'eccezione in modo che il gestore viene visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Aggiungere la registrazione degli errori a livello di codice

Come accennato in precedenza in questa esercitazione, è possibile aggiungere istruzioni try/catch per tentare di eseguire una sezione di codice e gestire l'errore prima che si verifica. In questo esempio è solo scriverà i dettagli dell'errore nel file di log degli errori in modo che l'errore può essere esaminato in un secondo momento.

1. Nella **Esplora soluzioni**, nella *per la logica* cartella, trovare e aprire il *PayPalFunctions.cs* file.
2. Aggiornamento di `HttpCall` metodo in modo che il codice viene visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

L'esempio di codice precedente chiama il `LogException` contenuta nel metodo il `ExceptionUtility` classe. Si è aggiunta la *ExceptionUtility.cs* file di classe per il *per la logica* cartella precedentemente in questa esercitazione. Il metodo `LogException` accetta due parametri. Il primo parametro è l'oggetto eccezione. Il secondo parametro è una stringa utilizzata per riconoscere l'origine dell'errore.

### <a name="inspecting-the-error-logging-information"></a>Esaminare le informazioni di registrazione errore

Come accennato in precedenza, è possibile usare il log degli errori per determinare quali errori nell'applicazione devono essere corretto prima di tutto. Naturalmente, verranno registrati solo gli errori che sono stati intercettati e scritti nel log degli errori.

1. In **Esplora soluzioni**individuare e aprire il *Errorlog* del file nei *App\_dati* cartella.   
 Potrebbe essere necessario selezionare il "**Mostra tutti i file**" opzione o il "**aggiornare**" opzione nella parte superiore della **Esplora soluzioni** per visualizzare il *Errorlog*file.
2. Esaminare il log degli errori visualizzato in Visual Studio: 

    ![Gestione degli errori di ASP.NET - Errorlog](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Messaggi di errore di protezione

Si tratta **importante notare** che quando l'applicazione vengono visualizzati i messaggi di errore, consigliabile non forniscano informazioni che un utente malintenzionato potrebbe risultare utile per attaccare l'applicazione. Ad esempio, se l'applicazione non riesce a scrivere un database, non dovrà essere indicato un messaggio di errore che include il nome utente in uso. Per questo motivo, all'utente viene visualizzato un messaggio di errore generico in rosso. Tutti i dettagli aggiuntivi sull'errore vengono visualizzati solo per gli sviluppatori nel computer locale.

## <a name="using-elmah"></a>Usando ELMAH

ELMAH (errore Logging Modules and Handlers) è una funzione di registrazione di errori che si collega all'applicazione ASP.NET come pacchetto NuGet. ELMAH offre le funzionalità seguenti:

- Registrazione delle eccezioni non gestite.
- Una pagina web per visualizzare l'intero log ricodificato eccezioni non gestite.
- Una pagina web per visualizzare i dettagli completi della ognuno registrate eccezioni.
- Una notifica tramite posta elettronica di ogni errore in fase di che esecuzione.
- Un feed RSS degli ultimi 15 errori dal log.

Prima di lavorare con i ELMAH, è necessario installarlo. È semplice usando il *NuGet* programma di installazione del pacchetto. Come accennato in precedenza in questa serie di esercitazioni, NuGet è un'estensione di Visual Studio che rende più semplice installare e aggiornare librerie open source e gli strumenti in Visual Studio.

1. All'interno di Visual Studio, dal **degli strumenti** dal menu **Library Package Manager**  - &gt; **Gestisci pacchetti NuGet per la soluzione**. 

    ![Gestione degli errori di ASP.NET - Gestisci pacchetti NuGet per la soluzione](aspnet-error-handling/_static/image6.png)
2. Il **Gestisci pacchetti NuGet** all'interno di Visual Studio verrà visualizzata la finestra di dialogo.
3. Nel **Gestisci pacchetti NuGet** finestra di dialogo espandere **Online** a sinistra, quindi selezionare **nuget.org**. Quindi, trovare e installare il **ELMAH** pacchetto dall'elenco dei pacchetti disponibili online. 

    ![Gestione degli errori di ASP.NET - ELMA Mobileengagement](aspnet-error-handling/_static/image7.png)
4. Dovrai disporre di una connessione internet per scaricare il pacchetto.
5. Nel **Seleziona progetti** finestra di dialogo verificare che il **WingtipToys** selezione è selezionata e quindi fare clic su **OK**. 

    ![Gestione degli errori di ASP.NET: finestra di dialogo progetti seleziona](aspnet-error-handling/_static/image8.png)
6. Fare clic su **Close** nelle **Gestisci pacchetti NuGet** finestra di dialogo se necessario.
7. Se Visual Studio è richiesto di ricaricare i file aperti, selezionare "**Sì a tutto**".
8. Il pacchetto ELMAH aggiunge voci per se stesso nel *Web. config* file nella radice del progetto. Se Visual Studio chiede all'utente se si vuole ricaricare il modificata *Web. config* del file, fare clic su **Yes**.

ELMAH è ora pronto per l'archiviazione che si verificano errori non gestiti.

### <a name="viewing-the-elmah-log"></a>Visualizzazione del Log ELMAH

Visualizzazione del log ELMAH è facile, ma prima di tutto si creerà un'eccezione non gestita che verrà registrata nel log ELMAH.

1. Premere **CTRL+F5** per eseguire l'applicazione di esempio Wingtip Toys.
2. Per scrivere un'eccezione non gestita nel log ELMAH, passare nel browser all'URL seguente (usando il numero di porta):  
    `https://localhost:44300/NoPage.aspx` Verrà visualizzata la pagina di errore.
3. Per visualizzare il log ELMAH, passare nel browser all'URL seguente (usando il numero di porta):  
    `https://localhost:44300/elmah.axd`

    ![Gestione degli errori di ASP.NET: Log degli errori ELMAH](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Riepilogo

In questa esercitazione si è appreso sulla gestione degli errori a livello di applicazione, il livello di pagina e a livello di codice. Inoltre, si è appreso come registrare gli errori gestiti e non gestiti per una revisione successiva. È stato aggiunto l'utilità ELMAH per fornire la registrazione delle eccezioni e la notifica all'applicazione tramite NuGet. Inoltre, si è appreso l'importanza dei messaggi di errore-safe.

## <a name="tutorial-series-conclusion"></a>Serie di esercitazioni conclusioni

*Grazie per seguire. Mi auguro che questa serie di esercitazioni pratiche permesso di acquisire maggiore familiarità con Web Form ASP.NET. Se sono necessarie altre informazioni sulle funzionalità di Web Form disponibili in ASP.NET 4.5 e Visual Studio 2013, vedere* [ *ASP.NET and Web Tools per Visual Studio 2013 Release Notes* ](../../../../visual-studio/overview/2013/release-notes.md)  *. Inoltre, assicurarsi di esaminare l'esercitazione indicata nella* ***passaggi successivi * * * sezione e defintely provare il* [ *gratuita di Azure* ](https://azure.microsoft.com/pricing/free-trial/)*.*

![Grazie aver - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulla distribuzione dell'applicazione web in Microsoft Azure, vedere [distribuire una Secure Web Form App ASP.NET con appartenenza, OAuth e Database SQL in un sito Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Versione di valutazione gratuita

[Microsoft Azure - versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/)  
 Pubblicazione del sito Web in Microsoft Azure consentirà di risparmiare tempo, la manutenzione e delle spese. È un processo rapido per distribuire l'app web in Azure. Quando è necessario gestire e monitorare l'app web, Azure offre un'ampia gamma di strumenti e servizi. Gestire i dati, il traffico, identità, i backup, messaggistica, multimediali e le prestazioni in Azure. E, tutto ciò è disponibile in un approccio molto conveniente.

## <a name="additional-resources"></a>Risorse aggiuntive

[I dettagli dell'errore di registrazione con monitoraggio dell'integrità di ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Riconoscimenti

Vorrei ringraziare i seguenti persone che hanno apportato contributi significativi per il contenuto di questa serie di esercitazioni:

- [Alberto Poblacion, MVP &amp; MCT, Spagna](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Paesi Bassi](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, Stati Uniti](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slovenia](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brasile](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Dos Carlos Santos, Brasile](http://www.carloscds.net/)
- [David Campbell, Stati Uniti](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Microsoft, Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Stati Uniti, Michael cancelletti](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike Pope
- [Stati Uniti, i venditori Mitchel](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portogallo](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Contributi della community

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Visual Studio 2012 correlati di esempio di codice su MSDN: [navigazione Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Visual Studio 2012 correlati di esempio di codice su MSDN: [serie ASP.NET 4.5 Web Form esercitazione in Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo - Microsoft Technical Audience Collaboratore (twitter: @driazevedo)  
  Conversione di Visual Studio 2012: [Iniciando com e ASP.NET Web Forms 4.5 - Parte 1 - Introdução Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Precedente](url-routing.md)
