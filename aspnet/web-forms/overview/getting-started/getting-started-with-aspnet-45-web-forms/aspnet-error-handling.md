---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Gestione degli errori ASP.NET | Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET tramite ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per abbiamo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: ac5508334bf6d471471a719b98618bdcd3214fb5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-error-handling"></a>Gestione degli errori ASP.NET
====================
da [Erik Reitan](https://github.com/Erikre)

[Scarica progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con il codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento questa serie di esercitazioni è disponibile.


In questa esercitazione si modificherà l'applicazione di esempio Wingtip Toys per includere la gestione degli errori e registrazione degli errori. Gestione degli errori consente all'applicazione di gestire gli errori correttamente e visualizzare i messaggi di errore di conseguenza. La registrazione degli errori consente di individuare e correggere gli errori che si sono verificati. In questa esercitazione si basa sull'esercitazione precedente "Routing degli URL" e fa parte della serie di esercitazioni Wingtip Toys.

## <a name="what-youll-learn"></a>Illustra quanto segue:

- Come aggiungere globale gestione degli errori per la configurazione dell'applicazione.
- Come aggiungere a livello di applicazione, pagina e codice di gestione degli errori.
- Come registrare gli errori per un'analisi successiva.
- Come visualizzare i messaggi di errore non compromettano la sicurezza.
- Come implementare la registrazione degli errori di moduli di registrazione errore e i gestori (ELMAH).

## <a name="overview"></a>Panoramica

Applicazioni ASP.NET devono essere in grado di gestire gli errori che si verificano durante l'esecuzione in modo coerente. ASP.NET Usa common language runtime (CLR), che fornisce un modo per notificare alle applicazioni di errori in modo uniforme. Quando si verifica un errore, viene generata un'eccezione. Un'eccezione è qualsiasi errore, condizione o un comportamento imprevisto che rileva un'applicazione.

In .NET Framework, un'eccezione è un oggetto che eredita dalla classe `System.Exception`. Le eccezioni vengono generate dalle aree di codice in cui si è verificato un problema. L'eccezione viene passata lo stack di chiamate in una posizione in cui l'applicazione fornisce il codice per gestire l'eccezione. Se l'applicazione gestisce l'eccezione, viene forzato il browser per visualizzare i dettagli dell'errore.

Come procedura consigliata, gestire gli errori in a livello di codice in `Try` / `Catch` / `Finally` blocchi all'interno del codice. Provare a inserire tali blocchi in modo che l'utente può correggere problemi nel contesto in cui si verificano. Se i blocchi di gestione degli errori sono troppo lontano in cui si è verificato l'errore, diventa più difficile fornire agli utenti con le informazioni necessarie per risolvere il problema.

### <a name="exception-class"></a>Classe di eccezione

La classe di eccezione è la classe base da cui ereditare le eccezioni. La maggior parte degli oggetti eccezione sono istanze di classi derivate della classe di eccezione, ad esempio il `SystemException` (classe), il `IndexOutOfRangeException` (classe), o `ArgumentNullException` classe. La classe Exception presenta le proprietà, ad esempio il `StackTrace` proprietà, il `InnerException` , proprietà e `Message` proprietà, che forniscono informazioni specifiche sull'errore che si è verificato.

### <a name="exception-inheritance-hierarchy"></a>Gerarchia di ereditarietà di eccezione

Il runtime è disponibile un set di base di eccezioni derivate dalla `SystemException` classe che il runtime genera un'eccezione quando viene rilevata un'eccezione. La maggior parte delle classi che ereditano dalla classe di eccezione, ad esempio il `IndexOutOfRangeException` classe e `ArgumentNullException` classe, non implementare membri aggiuntivi. Pertanto, le informazioni più importanti per un'eccezione sono reperibile nella gerarchia di eccezioni, il nome dell'eccezione e le informazioni contenute nell'eccezione.

### <a name="exception-handling-hierarchy"></a>Gerarchia di gestione delle eccezioni

In un'applicazione Web Form ASP.NET, le eccezioni possono essere gestite in base a una gerarchia di gestione specifico. Un'eccezione può essere gestita ai livelli seguenti:

- Livello di applicazione
- Livello di pagina
- Livello di codice

Quando un'applicazione gestisce le eccezioni, le informazioni aggiuntive sull'eccezione che viene ereditata dalla classe di eccezione possono spesso essere recuperate e visualizzate all'utente. Oltre alle applicazioni, pagina e il livello di codice, è anche possibile gestire le eccezioni a livello di modulo HTTP e tramite un gestore personalizzato di IIS.

### <a name="application-level-error-handling"></a>Gestione degli errori di livello applicazione

È possibile gestire gli errori di impostazione predefinita a livello di applicazione modificando la configurazione dell'applicazione o aggiungendo un `Application_Error` gestore di *Global. asax* file dell'applicazione.

È possibile gestire gli errori di impostazione predefinita e gli errori HTTP mediante l'aggiunta di un `customErrors` sezione per la *Web. config* file. Il `customErrors` sezione consente di specificare una pagina predefinita che gli utenti verranno reindirizzati al quando si verifica un errore. È anche possibile specificare singole pagine per errori del codice di stato specifici.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Purtroppo, quando si utilizza la configurazione per reindirizzare l'utente a una pagina diversa, non si dispone i dettagli dell'errore che si è verificato.

Tuttavia, è possibile registrare gli errori che si verificano in un punto qualsiasi nell'applicazione aggiungendo il codice per il `Application_Error` gestore nel *Global. asax* file.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Gestione degli eventi di errore a livello di pagina

Un gestore a livello di pagina restituisce l'utente alla pagina in cui si è verificato l'errore, ma non vengono mantenute le istanze dei controlli, non esisterà alcun elemento nella pagina. Per fornire i dettagli dell'errore per l'utente dell'applicazione, è necessario scrivere in modo specifico i dettagli dell'errore per la pagina.

Viene utilizzata in genere un gestore degli errori a livello di pagina per registrare gli errori non gestiti o per richiedere all'utente di una pagina in cui è possibile visualizzare informazioni utili.

Questo esempio di codice viene illustrato un gestore per l'evento di errore in una pagina Web ASP.NET. Questo gestore intercetta tutte le eccezioni non gestite già all'interno di `try` / `catch` Blocca nella pagina.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Dopo avere gestito un errore, è necessario cancellarlo chiamando il `ClearError` metodo dell'oggetto Server (`HttpServerUtility` classe), in caso contrario verrà visualizzato un errore che in precedenza si è verificato.

### <a name="code-level-error-handling"></a>Gestione degli errori di livello di codice

L'istruzione try-catch è costituita da un blocco try seguito da uno o più clausole catch che specificano i gestori per eccezioni diverse. Quando viene generata un'eccezione, common language runtime (CLR) Cerca l'istruzione catch che gestisce questa eccezione. Se il metodo attualmente in esecuzione non contiene un blocco catch, CLR cerca il metodo che ha chiamato il metodo corrente e così via, lo stack di chiamate. Se non viene trovato alcun blocco catch, CLR visualizza un messaggio di eccezione non gestita all'utente e interrompe l'esecuzione del programma.

Esempio di codice seguente viene illustrato un modo comune di utilizzo `try` / `catch` / `finally` per gestire gli errori.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

Nel codice precedente, il blocco try contiene il codice che deve essere protetto da una possibile eccezione. Il blocco viene eseguito finché non viene generata un'eccezione o il blocco è stato completata. Se un `FileNotFoundException` eccezione o un `IOException` si verifica un'eccezione, l'esecuzione viene trasferita a una pagina diversa. Quindi, il codice contenuto nel blocco finally viene eseguito, se si è verificato un errore o non.

## <a name="adding-error-logging-support"></a>Aggiunta del supporto di registrazione errore

Prima di aggiungere all'applicazione di esempio Wingtip Toys di gestione degli errori, si verrà aggiunto il supporto di registrazione degli errori tramite l'aggiunta di un `ExceptionUtility` classe per il *logica* cartella. In questo modo, ogni volta che l'applicazione gestisce un errore, i dettagli dell'errore verranno aggiunto nel file di log degli errori.

1. Fare doppio clic su di *logica* cartella e quindi selezionare **Aggiungi**  - &gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **codice** gruppo di modelli a sinistra. Selezionare quindi **classe**dal centro elenco e denominarlo **ExceptionUtility.cs**.
3. Scegliere **Aggiungi**. Viene visualizzato il nuovo file di classe.
4. Sostituire il codice esistente con quello seguente:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Quando si verifica un'eccezione, l'eccezione può essere scritto in un file di log di eccezione chiamando il `LogException` metodo. Questo metodo accetta due parametri, l'oggetto eccezione e una stringa che contiene dettagli sull'origine dell'eccezione. Il log dell'eccezione viene scritto il *Errorlog* file nel *App\_dati* cartella.

### <a name="adding-an-error-page"></a>Aggiunta di una pagina di errore

Nell'applicazione di esempio Wingtip Toys, una pagina da utilizzare per visualizzare gli errori. La pagina di errore è progettata per visualizzare un messaggio di errore sicuro agli utenti del sito. Tuttavia, se l'utente è uno sviluppatore effettua una richiesta HTTP che viene gestita in locale nel computer in cui si trova il codice, dettagli aggiuntivi sull'errore verrà visualizzato nella pagina di errore.

1. Fare doppio clic sul nome del progetto (**Wingtip Toys**) in **Esplora** e selezionare **Aggiungi**  - &gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Selezionare dall'elenco al centro, **Web Form con pagina Master**e denominarla **ErrorPage.aspx**.
3. Fare clic su **Aggiungi**.
4. Selezionare il *Site. master* file della pagina master e quindi scegliere **OK**.
5. Sostituire il codice esistente con il seguente:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Sostituire il codice esistente di code-behind (*ErrorPage.aspx.cs*) in modo che venga visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Quando viene visualizzata la pagina di errore, il `Page_Load` esecuzione del gestore eventi. Nel `Page_Load` gestore, la posizione di in cui è stato gestito innanzitutto l'errore è determinata. Quindi, l'ultimo errore che si è verificato è determinato dalla chiamata di `GetLastError` metodo dell'oggetto Server. Se l'eccezione non esiste, viene creata un'eccezione generica. Quindi, se la richiesta HTTP è stata eseguita in locale, vengono visualizzati tutti i dettagli dell'errore. In questo caso, solo il computer locale che esegue l'applicazione web visualizzeranno i dettagli dell'errore. Dopo che è stati visualizzati le informazioni sull'errore, l'errore viene aggiunto al file di log e l'errore viene cancellato dal server.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Visualizza i messaggi di errore non gestito per l'applicazione

Aggiungendo un `customErrors` sezione per la *Web. config* file, è possibile gestire rapidamente errori semplici che si verificano in tutta l'applicazione. È inoltre possibile specificare come gestire gli errori in base al relativo valore di codice di stato, ad esempio 404 - File non trovato.

#### <a name="update-the-configuration"></a>Aggiornare la configurazione

Aggiornare la configurazione mediante l'aggiunta di un `customErrors` sezione per la *Web. config* file.

1. In **Esplora**, trovare e aprire il *Web. config* file alla radice dell'applicazione di esempio Wingtip Toys.
2. Aggiungere il `customErrors` sezione per la *Web. config* file all'interno di `<system.web>` nodo come indicato di seguito:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Salvare il *Web. config* file.

Il `customErrors` sezione specifica la modalità, che è impostata su "On". Specifica inoltre il `defaultRedirect`, che indica l'applicazione, la pagina a cui passare quando si verifica un errore. Inoltre, è stato aggiunto un elemento di errore specifico che specifica come gestire un errore 404 quando non viene individuata una pagina. Più avanti in questa esercitazione si aggiungeranno ulteriori gestione degli errori che consente di acquisire i dettagli di un errore a livello di applicazione.

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione per visualizzare gli itinerari aggiornati.

1. Premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 Il browser viene visualizzata e viene illustrato il *Default.aspx* pagina.
2. Immettere l'URL seguente nel browser (assicurarsi di utilizzare **il** il numero di porta):  
    `https://localhost:44300/NoPage.aspx`
3. Esaminare il *ErrorPage.aspx* visualizzata nel browser. 

    ![Gestione degli errori di ASP.NET - errore di pagina non trovata](aspnet-error-handling/_static/image1.png)

Quando si richiede il *NoPage.aspx* pagina, che non esiste, la pagina di errore verrà visualizzati il messaggio di errore semplice e le informazioni dettagliate sull'errore se sono disponibili dettagli aggiuntivi. Tuttavia, se l'utente ha richiesto una pagina non esistente da una posizione remota, la pagina di errore permetterebbe solo di visualizzare il messaggio di errore in rosso.

### <a name="including-an-exception-for-testing-purposes"></a>Tra cui un'eccezione per scopi di test

Per verificare di funzionamento dell'applicazione durante un errore si verifica, è possibile creare deliberatamente le condizioni di errore in ASP.NET. Nell'applicazione di esempio Wingtip Toys, si verrà generata un'eccezione di test quando viene caricata la pagina predefinita per vedere cosa accade.

1. Aprire il code-behind del *Default.aspx* pagina in Visual Studio.   
   Il *Default.aspx.cs* pagina code-behind verrà visualizzato.
2. Nel `Page_Load` gestore, aggiungere il codice in modo che il gestore viene visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

È possibile creare diversi tipi di eccezioni. Nel codice precedente, si sta creando un `InvalidOperationException` quando il *Default.aspx* caricamento della pagina.

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione per vedere come l'applicazione gestisce l'eccezione.

1. Premere **CTRL + F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 L'applicazione genera un'eccezione InvalidOperationException. 

    > [!NOTE] 
    > 
    > È necessario premere **CTRL + F5** per visualizzare la pagina senza interrompere il codice per visualizzare l'origine dell'errore in Visual Studio.
2. Esaminare il *ErrorPage.aspx* visualizzata nel browser. 

    ![Gestione degli errori di ASP.NET - pagina di errore](aspnet-error-handling/_static/image2.png)

Come si può notare nei dettagli dell'errore, l'eccezione è stato intercettato dal `customError` sezione la *Web. config* file.

### <a name="adding-application-level-error-handling"></a>Aggiunta di gestione degli errori a livello di applicazione

Anziché l'eccezione utilizzando trap di `customErrors` sezione la *Web. config* file, in cui si ottengono little informazioni sull'eccezione, è possibile intercettare l'errore a livello di applicazione e recuperare i dettagli dell'errore.

1. In **Esplora**, trovare e aprire il *Global.asax.cs* file.
2. Aggiungere un **applicazione\_errore** gestore in modo che venga visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Quando si verifica un errore nell'applicazione, il `Application_Error` gestore viene chiamato. In questo gestore, l'ultima eccezione viene recuperato ed esaminato. Se l'eccezione non gestita e l'eccezione contiene i dettagli dell'eccezione interna (ovvero, `InnerException` non è null), l'applicazione trasferisce l'esecuzione alla pagina di errore vengono visualizzati i dettagli dell'eccezione.

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione per visualizzare i dettagli di errore aggiuntivi forniti da Gestione dell'eccezione a livello di applicazione.

1. Premere **CTRL + F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 L'applicazione genera il `InvalidOperationException` .
2. Esaminare il *ErrorPage.aspx* visualizzata nel browser. 

    ![Gestione degli errori di ASP.NET - errore di livello applicazione](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Aggiunta di gestione degli errori a livello di pagina

È possibile aggiungere la gestione degli errori a livello di pagina a una pagina tramite l'aggiunta di un `ErrorPage` attributo la `@Page` direttiva della pagina o aggiungendo un `Page_Error` gestore eventi per il code-behind di una pagina. In questa sezione si aggiungerà un `Page_Error` gestore dell'evento che verrà trasferita l'esecuzione di *ErrorPage.aspx* pagina.

1. In **Esplora**, trovare e aprire il *Default.aspx.cs* file.
2. Aggiungere un `Page_Error` gestore in modo che il code-behind viene visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Quando si verifica un errore nella pagina di `Page_Error` gestore eventi viene chiamato. In questo gestore, l'ultima eccezione viene recuperato ed esaminato. Se un `InvalidOperationException` si verifica, il `Page_Error` gestore trasferisce l'esecuzione alla pagina di errore vengono visualizzati i dettagli dell'eccezione.

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione per visualizzare gli itinerari aggiornati.

1. Premere **CTRL + F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 L'applicazione genera il `InvalidOperationException` .
2. Esaminare il *ErrorPage.aspx* visualizzata nel browser. 

    ![Gestione degli errori di ASP.NET - errore a livello di pagina](aspnet-error-handling/_static/image4.png)
3. Chiudere la finestra del browser.

### <a name="removing-the-exception-used-for-testing"></a>Rimozione di eccezione utilizzata per il test

Per consentire l'applicazione di esempio Wingtip Toys a funzionare senza generare l'eccezione che è stato aggiunto in precedenza in questa esercitazione, rimuovere l'eccezione.

1. Aprire il code-behind del *Default.aspx* pagina.
2. Nel `Page_Load` gestore, rimuovere il codice che genera l'eccezione in modo che il gestore viene visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Aggiunta di registrazione degli errori a livello di codice

Come accennato in precedenza in questa esercitazione, è possibile aggiungere istruzioni try/catch per tentare di eseguire una sezione di codice e gestire il primo errore che si verifica. In questo esempio, si verranno scritti solo i dettagli dell'errore nel file di log degli errori in modo che l'errore è possibile esaminare in un secondo momento.

1. In **Esplora**nella *logica* cartella, trovare e aprire il *PayPalFunctions.cs* file.
2. Aggiornamento di `HttpCall` metodo in modo che il codice viene visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Le chiamate di codice precedente il `LogException` metodo contenuto nel `ExceptionUtility` classe. Aggiunto il *ExceptionUtility.cs* file di classe per il *logica* cartella precedentemente in questa esercitazione. Il metodo `LogException` accetta due parametri. Il primo parametro è l'oggetto eccezione. Il secondo parametro è una stringa utilizzata per riconoscere l'origine dell'errore.

### <a name="inspecting-the-error-logging-information"></a>Esaminare le informazioni di registrazione errore

Come accennato in precedenza, è possibile utilizzare il log degli errori per determinare quali errori nell'applicazione devono essere corretto prima di tutto. Naturalmente, verranno registrati solo gli errori che sono stati intercettati e scritti nel log degli errori.

1. In **Esplora**, trovare e aprire il *Errorlog* file nel *App\_dati* cartella.   
 Potrebbe essere necessario selezionare il "**Mostra tutti i file**" opzione o "**aggiornare**" opzione dall'inizio di **Esplora** per visualizzare il *Errorlog*file.
2. Esaminare il log degli errori visualizzato in Visual Studio: 

    ![Gestione degli errori di ASP.NET - Errorlog](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Messaggi di errore di protezione

È **importante notare** che quando l'applicazione visualizza messaggi di errore, consigliabile non fornire informazioni che un utente malintenzionato potrebbe risultare utile per attaccare l'applicazione. Se, ad esempio, l'applicazione non riesce a scrivere un database, un messaggio di errore che include il nome utente in uso non deve visualizzare. Per questo motivo, all'utente viene visualizzato un messaggio di errore generico in rosso. Tutti i dettagli aggiuntivi sull'errore vengono visualizzati solo allo sviluppatore di nel computer locale.

## <a name="using-elmah"></a>Utilizzando ELMAH

ELMAH (gestori e i moduli di registrazione errore) è una funzione di registrazione di errori che si collega all'interno dell'applicazione ASP.NET come pacchetto NuGet. ELMAH offre le funzionalità seguenti:

- Registrazione di eccezioni non gestite.
- Una pagina web per visualizzare l'intero log ricodificate eccezioni non gestite.
- Una pagina web per visualizzare tutti i dettagli di ogni registrati eccezione.
- Una notifica tramite posta elettronica di ciascun errore in fase di cui che si verifica.
- Un feed RSS degli ultimi 15 errori dal log.

Prima di poter utilizzare con il ELMAH, è necessario installarlo. È facile che sia utilizzando la *NuGet* programma di installazione del pacchetto. Come accennato in precedenza in questa serie di esercitazioni, NuGet è un'estensione di Visual Studio che rende più semplice installare e aggiornare librerie open source e strumenti in Visual Studio.

1. All'interno di Visual Studio, dal **strumenti** dal menu **Gestione pacchetti libreria**  - &gt; **Gestisci pacchetti NuGet per la soluzione**. 

    ![Gestione degli errori di ASP.NET - Gestisci pacchetti NuGet per la soluzione](aspnet-error-handling/_static/image6.png)
2. Il **Gestisci pacchetti NuGet** la finestra di dialogo viene visualizzata all'interno di Visual Studio.
3. Nel **Gestisci pacchetti NuGet** finestra di dialogo espandere **Online** a sinistra, quindi selezionare **nuget.org**. Quindi, individuare e installare il **ELMAH** pacchetto dall'elenco dei pacchetti disponibili online. 

    ![Gestione degli errori di ASP.NET - pacchetto NuGet ELMA](aspnet-error-handling/_static/image7.png)
4. È necessario disporre di una connessione internet per scaricare il pacchetto.
5. Nel **selezionare progetti** finestra di dialogo, accertarsi di **WingtipToys** selezione è selezionata e quindi fare clic su **OK**. 

    ![Gestione degli errori di ASP.NET - finestra di dialogo progetti seleziona](aspnet-error-handling/_static/image8.png)
6. Fare clic su **Chiudi** in **Gestisci pacchetti NuGet** la finestra di dialogo, se necessario.
7. Se Visual Studio è richiesto di ricaricare i file aperti, selezionare "**Sì a tutto**".
8. Il pacchetto ELMAH aggiunge voci per se stessa nella *Web. config* file alla radice del progetto. Se Visual Studio viene richiesto se si desidera ricaricare modificato *Web. config* file, fare clic su **Sì**.

ELMAH è ora pronto per l'archiviazione che si verificano errori non gestiti.

### <a name="viewing-the-elmah-log"></a>Visualizzazione del Log ELMAH

Visualizzazione del registro ELMAH è semplice, ma prima di tutto si creerà un'eccezione non gestita che verrà registrata nel registro ELMAH.

1. Premere **CTRL + F5** per eseguire l'applicazione di esempio Wingtip Toys.
2. Per scrivere un'eccezione non gestita nel registro ELMAH, spostarsi nel browser all'URL seguente (utilizzando il numero di porta):  
    `https://localhost:44300/NoPage.aspx` Verrà visualizzata la pagina di errore.
3. Per visualizzare il registro ELMAH, spostarsi nel browser all'URL seguente (utilizzando il numero di porta):  
    `https://localhost:44300/elmah.axd`

    ![Gestione degli errori di ASP.NET - Log degli errori ELMAH](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Riepilogo

In questa esercitazione, si è appreso sulla gestione degli errori a livello di applicazione, il livello di pagina e il livello di codice. Inoltre, si è appreso come registrare gli errori gestiti e non gestiti per un'analisi successiva. È stato aggiunto l'utilità ELMAH per fornire la registrazione delle eccezioni e notifica all'applicazione tramite NuGet. Inoltre, si è appreso sull'importanza dei messaggi di errore di protezione.

## <a name="tutorial-series-conclusion"></a>Serie di esercitazioni conclusione

*Grazie per la seguente lungo. Spero di questa serie di esercitazioni utile acquisire familiarità con i Web Form ASP.NET. Se sono necessarie ulteriori informazioni sulle funzionalità di Web Form in ASP.NET 4.5 e Visual Studio 2013, vedere* [ *ASP.NET e strumenti Web per note sulla versione di Visual Studio 2013* ](../../../../visual-studio/overview/2013/release-notes.md)  *. Inoltre, assicurarsi di esaminare l'esercitazione citata nel* ***passaggi successivi * * * sezione e defintely provare il* [ *Azure versione di valutazione gratuita* ](https://azure.microsoft.com/pricing/free-trial/)*.*

![Grazie - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Passaggi successivi

Ulteriori informazioni sulla distribuzione dell'applicazione web in Microsoft Azure, vedere [distribuire un Secure App Web ASP.NET form con appartenenza, OAuth e il Database di SQL per un sito Web di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Versione di valutazione gratuita

[Microsoft Azure - versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/)  
 Pubblicazione del sito Web in Microsoft Azure consentirà di risparmiare tempo, manutenzione e spese. È un processo rapido per la distribuzione di app web in Azure. Quando è necessario gestire e monitorare l'app web, Azure offre un'ampia gamma di strumenti e servizi. Gestire i dati del traffico, identità, i backup, messaggistica, supporto e le prestazioni in Azure. E, tutto ciò viene fornito in un approccio molto conveniente.

## <a name="additional-resources"></a>Risorse aggiuntive

[Dettagli di errore di registrazione con monitoraggio dello stato ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Riconoscimenti

Come è possibile grazie seguenti persone che hanno apportato contributi significativi per il contenuto di questa serie di esercitazioni:

- [Alberto Poblacion, MVP &amp; MCT, (Spagna)](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Paesi Bassi](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, USA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slovenia](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brazil](http://msmvps.com/blogs/bsonnino) (twitter: [@bsonnino](http://twitter.com/bsonnino))
- [Dos Carlos Santos, Brasile](http://www.carloscds.net/)
- [David Campbell, USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael cancelletti, USA](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike Pope
- [I venditori Mitchel, USA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Contributi della community

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Visual Studio 2012 correlate nell'esempio di codice su MSDN: [navigazione Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Visual Studio 2012 correlate nell'esempio di codice su MSDN: [esercitazione serie di ASP.NET 4.5 Web Form in Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo - collaboratore destinatari tecnici Microsoft (twitter: @driazevedo)  
  Conversione di Visual Studio 2012: [Iniciando com e ASP.NET Web Forms 4.5 - Parte 1 - Introdução Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Precedente](url-routing.md)
