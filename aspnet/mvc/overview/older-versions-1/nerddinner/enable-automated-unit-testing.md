---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Abilita automatizzata Unit test | Documenti Microsoft
author: microsoft
description: Passaggio 12 illustra come sviluppare un gruppo di unit test automatizzati per verificare la funzionalità NerdDinner e che ci consentirà di confidenza per apportare modifiche...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: fede08be7e06327c6d04fa5d36f7dd818d79b380
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="enable-automated-unit-testing"></a>Abilitare Unit test automatici
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 12 della gratuito [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 12 illustra come sviluppare un gruppo di unit test automatizzati per verificare la funzionalità NerdDinner e che ci consentirà di confidenza per apportare modifiche e miglioramenti per l'applicazione in futuro.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner passaggio 12: Unit test

Consente lo sviluppo di un gruppo di unit test automatizzati per verificare la funzionalità NerdDinner e che ci consentirà di confidenza per apportare modifiche e miglioramenti per l'applicazione in futuro.

### <a name="why-unit-test"></a>Motivo per cui Unit Test?

Nell'unità di lavoro uno mattina è un improvviso flash di ispirato relative a un'applicazione che si sta lavorando. Si rende conto viene apportata una modifica è possibile implementare che consentirà l'applicazione notevolmente migliori. Potrebbe essere un'operazione di refactoring che pulisce il codice, viene aggiunta una nuova funzionalità o corregge un bug.

La domanda che confronts è arrivati a computer in uso è: "modalità provvisoria per rendere questo miglioramento?" Cosa accade se la modifica ha effetti collaterali o interruzioni di un elemento? La modifica potrebbe essere semplice e richiedere solo alcuni minuti per implementare, ma cosa accade se richiede ore per testare manualmente tutti gli scenari di applicazione? Cosa accade se si dimentica di coprire un scenario e un'applicazione passa in produzione? Effettua questo miglioramento effettivamente tutti la pena?

Unit test automatizzati consentono di rete di protezione che consente di migliorare continuamente le proprie applicazioni ed evitando teme il codice che si sta lavorando. Esecuzione con test per verificare rapidamente funzionalità consente di codice con confidenza – e consentono di migliorare i prodotti potrebbe in caso contrario non hanno ritenuto preferisci automatizzati. Consentono inoltre di creare soluzioni che sono più facili da gestire e hanno una durata più lunga, che garantisce un maggiore dell'utile sugli investimenti.

Il Framework di MVC ASP.NET è più semplice e naturale alle funzionalità di unit test dell'applicazione. Consente inoltre di un flusso di lavoro di Test Driven Development (TDD) che consente lo sviluppo basato su test preliminare.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests Project

Quando abbiamo creato l'applicazione NerdDinner all'inizio di questa esercitazione, alla richiesta con una finestra di dialogo che chiede se si desidera creare un progetto di unit test per passare insieme al progetto di applicazione:

![](enable-automated-unit-testing/_static/image1.png)

È stata mantenuta "Sì, crea un progetto di unit test" pulsante di opzione selezionato, che hanno comportato un progetto "NerdDinner.Tests" da aggiungere per la soluzione:

![](enable-automated-unit-testing/_static/image2.png)

Il progetto NerdDinner.Tests fa riferimento all'assembly del progetto di applicazione NerdDinner e consente di aggiungere facilmente i test automatizzati di cui verificare la funzionalità dell'applicazione.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Creazione di Unit test per la classe modello cena

Aggiungere alcuni test al progetto NerdDinner.Tests per verificare la classe Dinner creata quando è stato creato il livello del modello.

Si inizierà creando una nuova cartella all'interno del progetto di test denominato "Modelli" è in cui inserire i test correlati al modello. Verrà quindi pulsante destro del mouse sulla cartella e scegliere il **Add -&gt;nuovo Test** comando di menu. Verrà visualizzata la finestra di dialogo "Aggiungi nuovo Test".

Verrà scelto creare un "Unit Test" e denominarla "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Quando si fa clic sul pulsante "ok" Visual Studio verrà aggiunto (e aperto) un file DinnerTest.cs al progetto:

![](enable-automated-unit-testing/_static/image4.png)

Il modello di test predefinito Visual Studio unitario dispone di un gruppo di codice di stampa o editoriale all'interno che è possibile individuare un po' disordinato. Di seguito liberare spazio per contenere solo il codice riportato di seguito:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

Identifica l'attributo [TestClass] sulla classe DinnerTest come una classe che conterrà i test, nonché l'inizializzazione di testing facoltativo e il codice di disinstallazione. È possibile definire i test all'interno di esso tramite l'aggiunta di metodi pubblici che hanno un attributo [TestMethod] su di essi.

Di seguito sono il primo di due test verrà aggiunto che utilizzano la classe Dinner. Il primo test verifica che il nostro Dinner non è valido se viene creato un nuovo Dinner senza tutte le proprietà siano impostate correttamente. Il secondo test verifica che il nostro Dinner è valido quando un Dinner dispone di tutte le proprietà impostate con i valori validi:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Si noterà che in precedenza che i nomi di test sono molto esplicite (e in qualche modo dettagliato). Ciò viene poiché è possibile finire la creazione di centinaia o migliaia di test di piccole dimensioni e si vuole rendono più facile individuare rapidamente la finalità e il comportamento di ognuno di essi (in particolare quando si analizzi un elenco di errori in un test runner). I nomi dei test devono essere denominati dopo la funzionalità che sottoposto a test. Sopra si sta usando un "sostantivo\_deve\_verbo" modello di denominazione.

Ci stiamo strutturare i test tramite il modello: è l'acronimo di "Arrange, Act, Assert" test "AAA":

- Disponi: Configurare l'unità sottoposto a test
- ACT: Rispettare l'unità sottoposta a test e acquisire i risultati
- Asserzione: Verificare il comportamento

Quando si scrivono test che si desidera evitare che i singoli test eseguire troppo elevato. Ogni test deve verificare invece solo un singolo concetto (che renderà molto più semplice individuare la causa di errori). Buona consiste nel provare e avere solo una singola istruzione per ogni test di asserzione. Se si dispone di più istruzioni in un metodo di test di asserzione, assicurarsi che sono tutti utilizzati per testare lo stesso concetto. In caso di dubbi, rendere un altro test.

### <a name="running-tests"></a>Esecuzione test

Visual Studio 2008 Professional (e versioni successive) includono un programma test runner che può essere utilizzato per eseguire progetti di Visual Studio di Unit Test all'interno dell'IDE. È possibile selezionare il **Test -&gt;esecuzione -&gt;tutti i test nella soluzione** menu comando (o tipo Ctrl R,) per eseguire tutti i test di unità. Oppure in alternativa è possibile posizionare il cursore all'interno di un metodo di test o di classe di test specifici e utilizzare il **Test -&gt;esecuzione -&gt;test nel contesto corrente** comando di menu (o tipo Ctrl R, T) per eseguire un subset degli unit test.

È ora possibile posizionare il cursore all'interno della classe DinnerTest e digitare "Ctrl R, T" per eseguire i due test definita. Quando si esegue questa operazione una "finestra Risultati Test" verrà visualizzato all'interno di Visual Studio e verranno esaminati i risultati di test eseguire elencati all'interno di essa:

![](enable-automated-unit-testing/_static/image5.png)

*Nota: La finestra Risultati test di Visual Studio non visualizza la colonna nome della classe per impostazione predefinita. È possibile aggiungere questo facendo clic all'interno della finestra risultati del Test e utilizzando il comando di menu Aggiungi/Rimuovi colonne.*

I due test ha richiesto solo una frazione di secondo per l'esecuzione e come si può vedere entrambi passati. È possibile passare e li aumentare mediante la creazione di altri test che verifica le convalide di regole specifici, nonché coprire i due metodi helper - IsUserHost() e IsUserRegisterd(): aggiunto alla classe di Dinner. Quando tutti questi test in posizione centralizzata per la classe Dinner renderà molto più semplice e sicuro per aggiungervi nuove regole business e le convalide in futuro. È possibile aggiungere la nuova logica della regola a cena e quindi verificare che non è stata interrotta la precedente funzionalità logica entro pochi secondi.

Si noti come utilizzando un nome descrittivo test semplifica comprendere rapidamente cosa si verifica ogni test. Si consiglia di utilizzare il **Tools -&gt;opzioni** comando di menu, aprire Strumenti di Test -&gt;schermata di configurazione di esecuzione di Test e verifica il "facendo doppio clic su un risultato del test non riusciti o senza risultati unità Visualizza casella di controllo al punto di errore nel test". In questo modo sarà possibile fare doppio clic su un errore nella finestra dei risultati di test e passare immediatamente all'errore di asserzione.

### <a name="creating-dinnerscontroller-unit-tests"></a>Creazione di DinnersController Unit test

Creare ora alcuni unit test per verificare la funzionalità DinnersController. Viene innanzitutto facendo clic sulla cartella "Controller" all'interno del progetto di Test e quindi scegliere il **Add -&gt;nuovo Test** comando di menu. Viene creato uno "Unit Test" e denominarla "DinnersControllerTest.cs".

Verranno create due metodi di test che verificano il metodo di azione Details() sul DinnersController. Il primo consente di verificare che una vista viene restituita quando viene richiesto un Dinner esistente. Il secondo verificherà che una visualizzazione "NotFound" viene restituita quando viene richiesto un Dinner inesistente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Il codice sopra riportato compila pulito. Quando si esegue il test, tuttavia, hanno entrambi esito negativo:

![](enable-automated-unit-testing/_static/image6.png)

Esaminare i messaggi di errore, si vedrà che il test abbia avuto esito negativo è perché la classe DinnersRepository non è riuscita a connettersi a un database. L'applicazione NerdDinner utilizza una stringa di connessione in un file locale di SQL Server Express che si trova sotto il \App\_directory dei dati del progetto di applicazione NerdDinner. Poiché il progetto NerdDinner.Tests compilato ed eseguito in una directory diversa quindi il progetto di applicazione, il percorso relativo della stringa di connessione non è corretto.

Abbiamo *Impossibile* risolvere il problema, copiare il file di database SQL Express per il progetto di test e quindi aggiungervi una stringa di connessione appropriato di test di App. config del progetto test. Si potrebbe ottenere questi test sbloccato e in esecuzione.

Unit test del codice con un database reale, tuttavia, è accompagnata da una serie di difficoltà. In particolare:

- Rallenta notevolmente il tempo di esecuzione di unit test. Il tempo necessario per eseguire i test, minore probabilità che vengano eseguite di frequente. Idealmente si desidera che gli unit test per poter essere eseguiti in secondi e fare in modo essere è eseguire come naturalmente come la compilazione del progetto.
- Rende più difficile la logica di programma di installazione e pulizia all'interno di test. Si desidera che ogni unit test per essere isolato e indipendente dalle altre (senza effetti collaterali o con dipendenze). Quando si lavora su un database reale che è necessario tenere conto dello stato e ripristinare le impostazioni tra i test.

Esaminiamo un modello di struttura denominato "inserimento di dipendenze" che è possibile contribuire a risolvere questi problemi ed evitare la necessità di utilizzare un database reale con i test.

### <a name="dependency-injection"></a>Inserimento di dipendenze

Ora DinnersController è strettamente "a" alla classe DinnerRepository. "Associazione" si riferisce a una situazione in cui una classe in modo esplicito si basa su un'altra classe per poter funzionare:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Poiché la classe DinnerRepository richiede l'accesso a un database, la dipendenza fortemente accoppiata la classe DinnersController ha alle estremità DinnerRepository le necessità di disporre di un database in modo che i metodi di azione DinnersController da testare.

È possibile ottenere il problema, è necessario distribuire un modello di struttura denominato "inserimento di dipendenze:" che è un approccio in cui le dipendenze (ad esempio le classi di repository che forniscono accesso ai dati) non è più in modo implicito vengono create all'interno di classi che li utilizzano. Al contrario, le dipendenze possono essere passate in modo esplicito alla classe in cui vengono utilizzati con gli argomenti del costruttore. Se le dipendenze vengono definite utilizzando le interfacce, è necessario disporre della flessibilità di passare in implementazioni di dipendenza "false" per gli scenari di unit test. In questo modo sarà possibile creare le implementazioni di dipendenza specifiche che in realtà non richiedono l'accesso a un database.

Per vedere un esempio pratico, si implementa l'inserimento di dipendenze con il nostro DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Estrazione di un'interfaccia IDinnerRepository

Il primo passaggio è necessario creare una nuova interfaccia IDinnerRepository che incapsula il controller necessari per recuperare e aggiornare Dinners il contratto di repository.

È possibile definire questa interfaccia di contratto manualmente facendo clic sulla cartella \Models, e quindi scegliendo il **Add -&gt;nuovo elemento** comando di menu e la creazione di una nuova interfaccia denominata IDinnerRepository.cs.

In alternativa è possibile usare il refactoring automaticamente compilato in Visual Studio Professional strumenti (e versioni successive) per estrarre e creare un'interfaccia per noi dalla classe DinnerRepository esistente. Per estrarre l'interfaccia usando Visual Studio, è sufficiente posizionare il cursore nell'editor di testo sulla classe DinnerRepository, quindi fare doppio clic su e scegliere il **refactoring -&gt;Estrai interfaccia** comando di menu:

![](enable-automated-unit-testing/_static/image7.png)

Verrà aperta la finestra di dialogo "Estrai interfaccia" e ci richiedere il nome dell'interfaccia da creare. Verrà IDinnerRepository per impostazione predefinita e selezionare automaticamente tutti i metodi pubblici della classe DinnerRepository esistente da aggiungere all'interfaccia:

![](enable-automated-unit-testing/_static/image8.png)

Quando si fa clic sul pulsante "ok", Visual Studio aggiungerà una nuova interfaccia IDinnerRepository all'applicazione:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

E la classe DinnerRepository esistente verrà aggiornata in modo che implementi l'interfaccia:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Aggiornamento DinnersController per supportare l'inserimento del costruttore

A questo punto verrà aggiornata la classe DinnersController per utilizzare la nuova interfaccia.

Attualmente DinnersController è codificato in modo che il campo "dinnerRepository" è sempre una classe DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

E verranno modificate in modo che il campo "dinnerRepository" è di tipo IDinnerRepository anziché DinnerRepository. Verrà quindi aggiunto due costruttori DinnersController pubblici. Uno dei costruttori consente un IDinnerRepository deve essere passato come argomento. L'altro è un costruttore predefinito che utilizza l'implementazione di DinnerRepository esistente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Poiché ASP.NET MVC per impostazione predefinita crea classi controller utilizzando i costruttori predefiniti, i nostri DinnersController in fase di esecuzione continuerà a utilizzare la classe DinnerRepository per eseguire l'accesso ai dati.

È ora possibile aggiornare il nostro unit test, tuttavia, per passare in un'implementazione di repository dinner "fittizio" utilizzando il parametro di costruttore. Questo repository "fittizio" dinner invece utilizzare dati di esempio in memoria non richiedono l'accesso a un database reale.

#### <a name="creating-the-fakedinnerrepository-class"></a>Creazione della classe FakeDinnerRepository

È possibile creare una classe FakeDinnerRepository.

Viene innanzitutto la creazione di una directory "Fakes" all'interno del progetto NerdDinner.Tests e quindi aggiungere una nuova classe FakeDinnerRepository (fare doppio clic sulla cartella e scegliere **Add -&gt;nuova classe**):

![](enable-automated-unit-testing/_static/image9.png)

Il codice verrà aggiornata in modo che la classe FakeDinnerRepository implementa l'interfaccia IDinnerRepository. È quindi possibile fare doppio clic su di esso e scegliere il comando del menu contestuale "Implementa interfaccia IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

In questo modo Visual Studio aggiungere automaticamente tutti i membri di interfaccia IDinnerRepository alla classe FakeDinnerRepository con implementazioni "stub out" predefinite:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

È quindi possibile aggiornare l'implementazione di FakeDinnerRepository funzionamento di fuori di un elenco in memoria&lt;Dinner&gt; raccolta passata come argomento costruttore:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

È ora disponibile un'implementazione IDinnerRepository falsa che non richiede un database e può funzionare invece un elenco in memoria degli oggetti Dinner.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Utilizzo di FakeDinnerRepository con Unit test

Torniamo per gli unit test di DinnersController che non è riuscita in precedenza perché il database non era disponibile. È possibile aggiornare i metodi di test per l'utilizzo di un FakeDinnerRepository popolate con dati cena in memoria per il DinnersController utilizzando il codice riportato di seguito:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

E, quando si eseguono questi test sono entrambi passare:

![](enable-automated-unit-testing/_static/image11.png)

Soprattutto, richiedere solo una frazione di secondo per l'esecuzione e non richiedono alcuna logica di pulizia o l'installazione complessa. È possibile ora unit test tutto il codice di metodo di azione DinnersController (elenco di inclusa, paging, dettagli, creare, aggiornare ed eliminare) senza dover fare per connettersi a un database reale.

| **Lato dell'argomento: Framework di Injection dipendenza** |
| --- |
| Esegue l'inserimento delle dipendenze manuale (ad esempio, ci sono di sopra) funziona correttamente, ma diventa più difficile da gestire come il numero di dipendenze e componenti in un'applicazione aumenta. Sono disponibili più Framework di inserimento di dipendenza per .NET che consentono di fornire una maggiore flessibilità di gestione di dipendenza. Questi Framework, anche detti contenitori "Inversion of Control" (IoC), forniscono meccanismi che consentano di un ulteriore livello di supporto della configurazione per l'impostazione e il passaggio di dipendenze per gli oggetti in fase di esecuzione (in genere tramite inserimento del costruttore ). Alcune delle più comuni inserimento di dipendenze di sistemi operativi / Framework di inversione di controllo in .NET: AutoFac, Ninject, Spring.NET, StructureMap e Windsor. ASP.NET MVC espone API che consentono agli sviluppatori di partecipare alla risoluzione e la creazione di un'istanza del controller e che consente l'inserimento di dipendenze di estendibilità / Framework IoC integrare correttamente all'interno di questo processo. Utilizzare un framework DI/IOC sarebbe consentono inoltre di rimuovere il costruttore predefinito dal nostro DinnersController – cui verrebbe rimuovere completamente l'accoppiamento tra i file e il DinnerRepositorys. Si prevede di usare un inserimento di dipendenze / framework IOC con l'applicazione NerdDinner. Ma è un elemento che è stato possibile prendere in considerazione per il futuro se le funzionalità base di codice NerdDinner aumento delle dimensioni. |

### <a name="creating-edit-action-unit-tests"></a>Creazione di Unit test azione di modifica

Creare ora alcuni unit test per verificare la funzionalità di modifica del DinnersController. Inizieremo con la versione di HTTP-GET l'azione di modifica di test:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Si creerà un test che verifica che una vista supportata da un oggetto DinnerFormViewModel viene eseguito il rendering quando viene richiesto un dinner valido:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Quando si esegue il test, tuttavia, verrà individuato che abbia esito negativo perché viene generata un'eccezione di riferimento null quando il metodo di modifica accede alla proprietà User.Identity.Name per eseguire il controllo Dinner.IsHostedBy().

L'oggetto utente nella classe di base Controller incapsula i dettagli relativi all'utente connesso e viene popolato da MVC ASP.NET quando crea il controller in fase di esecuzione. Poiché il DinnersController all'esterno di un ambiente server web sottoposti a test, non è impostato l'oggetto utente (pertanto l'eccezione riferimento null).

### <a name="mocking-the-useridentityname-property"></a>Tali proprietà User.Identity.Name

Framework di simulazione per facilitare il testing, consentendo di creare dinamicamente false versioni degli oggetti dipendenti che supportano i test. Ad esempio, è possibile utilizzare un framework di simulazione al test di azione di modifica per creare dinamicamente un oggetto utente che il nostro DinnersController è possibile utilizzare per cercare un nome utente simulato. Questo modo si evita un riferimento null viene generata quando si esegue il test.

Esistono molti .NET tali Framework che può essere utilizzato con ASP.NET MVC da (è possibile visualizzare un elenco di queste impostazioni qui: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). Per il test dell'applicazione NerdDinner si userà un open-source mocking framework denominato "Moq", che può essere scaricato gratuitamente dal [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

Una volta scaricato, verrà aggiunto un riferimento nel progetto NerdDinner.Tests all'assembly Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

Si aggiungerà quindi un metodo di supporto "CreateDinnersControllerAs(username)" per la classe di test che accetta un nome utente come parametro e che quindi la proprietà User.Identity.Name nell'istanza DinnersController "mocks":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Moq sopra viene utilizzato per creare un oggetto fittizi che fakes un oggetto ControllerContext (ossia quali ASP.NET MVC passa alle classi Controller per esporre gli oggetti di runtime come utente, richiesta, risposta e sessione). Si sta chiamando il metodo "SetupGet" per la simulazione per indicare che la proprietà HttpContext.User.Identity.Name ControllerContext deve restituire la stringa del nome utente che è passati al metodo di supporto.

È possibile simulare un numero qualsiasi di metodi e proprietà ControllerContext. Per illustrare questo concetto è stata aggiunta anche una chiamata di SetupGet() per la proprietà Request.IsAuthenticated (che non è effettivamente necessaria per i test seguenti: ma che consente di illustrare come è possibile simulare le proprietà delle richieste). Al termine è assegnare un'istanza della simulazione ControllerContext per il DinnersController restituisce il metodo di supporto.

In questo punto, è possibile scrivere unit test che utilizzano questo metodo di supporto per testare gli scenari di modifica che interessa diversi utenti:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

E ora quando si eseguono i test vengono superati:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Scenari di test UpdateModel()

Abbiamo creato i test che coprono la versione HTTP-GET dell'azione di modifica. Creare ora alcuni test che verificano la versione HTTP-POST dell'azione di modifica:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Lo scenario di test nuove interessano a Microsoft per supporto con questo metodo di azione è l'utilizzo del metodo UpdateModel() helper nella classe di base Controller. Si sta usando questo metodo di supporto per associare i valori di post dei form per l'istanza dell'oggetto Dinner.

Di seguito sono due test che dimostra come sia possibile fornire modulo inviato i valori per il metodo helper UpdateModel() da utilizzare. È sarà questo scopo, creare e popolare un oggetto FormCollection e quindi assegnarlo alla proprietà "ValueProvider" nel Controller.

Il primo test verifica che in un'operazione di salvataggio il browser viene reindirizzato all'azione di dettagli. Il secondo test verifica quando l'input non valido viene registrato l'azione visualizzata nuovamente la visualizzazione di modifica nuovamente un messaggio di errore.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Test conclusioni

Stati trattati i concetti principali coinvolti nelle classi di unit test controller. È possibile utilizzare queste tecniche per creare facilmente centinaia di semplici test per verificare il comportamento dell'applicazione.

Poiché il controller e il test del modello non richiedono un database reale, sono molto semplice e veloce per l'esecuzione. È possibile eseguire centinaia di test automatizzati in secondi viene immediatamente visualizzato feedback sulla se ha superato una modifica apportate è un elemento. Questo consente di fornire il livello di confidenza per migliorare, eseguire il refactoring e ottimizzare l'applicazione continuamente.

Test trattati come ultimo argomento in questo capitolo, ma non perché il test è un elemento alla fine di un processo di sviluppo. Al contrario, è consigliabile scrivere test automatizzati più presto nel processo di sviluppo. Ciò consente pertanto ottenere un feedback immediato quando si sviluppa, consente di considerare attentamente scenari di casi di utilizzo dell'applicazione e consente di progettare l'applicazione con pulita disposizione su livelli e accoppiamento presente.

Sviluppo basato su Test (TDD) e come utilizzarla con ASP.NET MVC, si parlerà un capitolo successivo nel libro. TDD è una procedura di codifica iterativa, in cui devi innanzitutto scrivere i test che soddisferà il codice risulta. Con TDD iniziare creando un test che verifica la funzionalità che si sta per implementare ogni funzionalità. Scrittura di unit test innanzitutto contribuisce a garantire comprendere chiaramente la funzionalità e come deve operare. Solo dopo che il test è scritto (e aver verificato che abbia esito negativo) non è, implementare la funzionalità di che test di verifica. Perché è già stato speso pensare di tempo nel caso di utilizzo di presunto per utilizzare la funzionalità, si avrà una migliore comprensione dei requisiti e il modo migliore per implementarli. Una volta con l'implementazione è possibile eseguire nuovamente il test: e ottenere un feedback immediato come per se la funzionalità funziona correttamente. Spiegheremo TDD più nel capitolo 10.

### <a name="next-step"></a>Passo successivo

Alcuni incapsulamento finale i commenti.

> [!div class="step-by-step"]
> [Precedente](use-ajax-to-implement-mapping-scenarios.md)
> [Successivo](nerddinner-wrap-up.md)
