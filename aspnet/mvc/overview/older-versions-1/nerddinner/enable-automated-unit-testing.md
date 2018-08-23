---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Abilitare Unit test automatici | Microsoft Docs
author: microsoft
description: Passaggio 12 mostra come sviluppare una suite di unit test automatizzati che consentono di verificare la funzionalità di NerdDinner e che fornirà la fiducia necessaria per apportare modifiche...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 74abf391bb4aab3ff0d5079e0a24ba20287e18fb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831547"
---
<a name="enable-automated-unit-testing"></a>Abilitare Unit test automatici
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 12 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 12 illustra come sviluppare una suite di unit test automatizzati che consentono di verificare la funzionalità di NerdDinner e che fornirà la fiducia necessaria per apportare modifiche e miglioramenti per l'applicazione in futuro.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner passaggio 12: Testing unità

È possibile sviluppare una suite di unit test automatizzati che consentono di verificare la funzionalità di NerdDinner e che fornirà la fiducia necessaria per apportare modifiche e miglioramenti per l'applicazione in futuro.

### <a name="why-unit-test"></a>Il motivo per cui Unit Test?

Nell'unità di lavoro una mattina è necessario un attimo improvviso di ispirazione relative a un'applicazione che si sta lavorando. Si può notare, viene apportata una modifica che è possibile implementare in modo significativo migliorare l'applicazione. Potrebbe essere un'operazione di refactoring che pulisce il codice, aggiunge una nuova funzionalità o corregge un bug.

La domanda che confronts è quando si arriva a computer in uso è: "la modalità sicura per verificare questo miglioramento?" Cosa accade se apportare la modifica ha effetti collaterali o interruzioni qualcosa? La modifica potrebbe essere semplice e richiedere solo alcuni minuti per implementare, ma cosa accade se sono necessarie ore per testare manualmente tutti gli scenari dell'applicazione? Cosa accade se si dimentica di coprire uno scenario e un'applicazione passa in produzione? Effettua questo miglioramento davvero la pena di tutti gli sforzi?

Unit test automatizzati possono fornire una rete di protezione che consente di migliorare continuamente le applicazioni e possibile evitare di incorrere paura del codice di cui che si sta lavorando. Operazioni con i test automatizzati che consentono di verificare rapidamente la funzionalità consente di codice in tutta sicurezza – e ottimizzare le prestazioni di apportare miglioramenti è possibile che in caso contrario non ritenevano familiarità. Consentono inoltre di creare soluzioni che sono più facili da gestire e hanno una durata più lunga - che comporta un maggiore ritorno sull'investimento.

Il Framework di MVC ASP.NET rende più semplice e naturale alle funzionalità di unit test dell'applicazione. Inoltre, consente un flusso di lavoro di Test Driven Development (TDD) che consente lo sviluppo basato su test preventivi.

### <a name="nerddinnertests-project"></a>Progetto NerdDinner.Tests

Quando abbiamo creato l'applicazione di NerdDinner all'inizio di questa esercitazione, alla richiesta con una finestra di dialogo che chiede se si desidera creare un progetto di unit test per passare insieme al progetto di applicazione:

![](enable-automated-unit-testing/_static/image1.png)

Abbiamo mantenuto "Sì, crea un progetto unit test" pulsante di opzione selezionato, questo ha comportato un progetto "NerdDinner.Tests" aggiunto alla soluzione:

![](enable-automated-unit-testing/_static/image2.png)

Il progetto NerdDinner.Tests fa riferimento all'assembly del progetto di applicazione NerdDinner e ci permette di aggiungere facilmente i test automatizzati che verifica la funzionalità dell'applicazione.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Creazione di Unit test per la nostra classe di modello Dinner

Aggiungiamo alcuni test per il progetto NerdDinner.Tests che verificano la classe Dinner creato quando è stato creato il livello del modello.

Si inizierà creando una nuova cartella nel nostro progetto di test denominata "Models" dove posizioneremo test correlata al modello. Abbiamo quindi fare doppio clic sulla cartella e scegliere il **Add -&gt;nuovo Test** comando di menu. Verrà visualizzata la finestra di dialogo "Aggiungi nuovo Test".

Verrà scelto creare un "Unit Test" e denominarlo "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Quando si fa clic sul pulsante "ok" Visual Studio verrà aggiunto (e aperto) un file DinnerTest.cs al progetto:

![](enable-automated-unit-testing/_static/image4.png)

Il modello di Visual Studio unit test predefinito contiene una serie di codice boilerplate interno che è possibile individuare un po' complessa. È possibile pulire, contenga solo il codice seguente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

L'attributo [TestClass] sulla classe DinnerTest lo identifica come una classe che conterrà i test, nonché l'inizializzazione del test facoltativo e il codice di teardown. È possibile definire i test all'interno di esso mediante l'aggiunta di metodi pubblici che hanno un attributo [TestMethod] su di essi.

Di seguito sono il primo dei due test verrà aggiunto che verificano il comportamento nostra classe cena. Il primo test verifica che la cena non è valida se viene creata una nuova cena senza tutte le proprietà impostate correttamente. Il secondo test verifica che la cena è valida quando un Dinner ha tutte le proprietà impostate con i valori validi:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Si noterà che in precedenza che ai nomi di test sono molto esplicite (e un po' verbose). Ciò viene ottenuto perché Microsoft congestione creazione centinaia o migliaia di test di piccole dimensioni e si vuole semplificare determinare rapidamente la finalità e il comportamento della ognuno di essi (in particolare quando ci stiamo impegnando tramite un elenco di errori in un test runner). I nomi di test devono essere denominati dopo la funzionalità di che test. Sopra si sta usando un "sostantivo\_dovrebbero\_verbo" modello di denominazione.

Si sta strutturare i test tramite il test con modello che è l'acronimo di "Arrange, Act, Assert" "AAA":

- Disponi: Configurare l'unità sottoposto a test
- ACT: Verificare l'unità sottoposta a test e acquisire i risultati
- Assert: Verificare il comportamento

Quando si scrivono test che si vuole evitare che i singoli test eseguire troppe operazioni. Ogni test dovrebbe verificare, invece, solo un singolo concetto (che renderà molto più facile individuare la causa di errori). Una buona indicazione consiste nel provare e avere solo una singola istruzione per ogni test di asserzione. Se si dispone di più di una istruzione in un metodo di test di asserzione, assicurarsi che sono tutti in uso per testare lo stesso concetto. In caso di dubbi, effettuare un altro test.

### <a name="running-tests"></a>Esecuzione test

Visual Studio 2008 Professional (e versioni successive) includono un test runner predefinito che può essere utilizzato per eseguire i progetti di Visual Studio Unit Test all'interno dell'IDE. È possibile selezionare i **Test -&gt;Run -&gt;tutti i test nella soluzione** menu comando (o tipo Ctrl R, A) per eseguire tutti i test di unità. Oppure in alternativa è possibile posizionare il cursore all'interno di un metodo di test o classe di test specifici e usare la **Test -&gt;Run -&gt;test nel contesto corrente** comando di menu (oppure digitare Ctrl R, T) per eseguire un subset degli unit test.

È possibile posizionare il cursore all'interno della classe DinnerTest e digitare "Ctrl R, T" per eseguire i due test che è stato appena definito. Quando si esegue questa operazione una "finestra Risultati Test" verrà visualizzato all'interno di Visual Studio e vedremo i risultati del test eseguito elencati all'interno di esso:

![](enable-automated-unit-testing/_static/image5.png)

*Nota: La finestra Risultati test di Visual Studio non visualizza la colonna del nome di classe per impostazione predefinita. È possibile aggiungere questo facendo clic all'interno della finestra risultati del Test e usando il comando di menu Aggiungi/Rimuovi colonne.*

I due test richiedevano solo una frazione di secondo per eseguire – e come si può vedere entrambi passati. Ora possiamo passare e aumentare li creando altri test che verifica le convalide di regole specifici, nonché coprire i due metodi helper - IsUserHost() e IsUserRegisterd() – che abbiamo aggiunto alla classe Dinner. Con le verifiche in atto per la classe Dinner renderà molto più semplice e sicura per aggiungervi nuove regole business e le convalide in futuro. È possibile aggiungere la nuova logica della regola a cena e quindi verificare che non è stato interrotto qualsiasi della funzionalità per la logica precedente entro pochi secondi.

Si noti come usando un nome descrittivo test rende facile da comprendere rapidamente ciò che si verifica ogni test. È consigliabile utilizzare il **strumenti -&gt;opzioni** comando di menu, aprire Strumenti di Test -&gt;schermata di configurazione di esecuzione di Test e verifica la "facendo doppio clic su un risultato del test non riusciti o senza risultati unit Visualizza casella di controllo al punto di errore nel test". In questo modo sarà possibile fare doppio clic su un errore nella finestra dei risultati dei test e passare immediatamente all'errore di asserzione.

### <a name="creating-dinnerscontroller-unit-tests"></a>Creazione di DinnersController Unit test

Creare ora alcuni unit test per verificare la funzionalità DinnersController. Verrà avviare facendo clic sulla cartella "Controller" entro il progetto di Test e quindi scegliere il **Add -&gt;nuovo Test** comando di menu. Si verrà creato uno "Unit Test" e denominarlo "DinnersControllerTest.cs".

Si creerà due metodi di test che verificano il metodo di azione Details() sul DinnersController. La prima verifica che una visualizzazione viene restituita quando viene richiesto un Dinner esistente. La seconda verifica che una visualizzazione "NotFound" viene restituita quando viene richiesto un Dinner inesistente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Il codice sopra riportato consente di compilare pulita. Quando si esegue il test, tuttavia, hanno entrambi esito negativo:

![](enable-automated-unit-testing/_static/image6.png)

Se si esamina i messaggi di errore, si vedrà che il test abbia avuto esito negativo era la classe DinnersRepository non è riuscita a connettersi a un database. La nostra applicazione NerdDinner Usa una stringa di connessione in un file locale di SQL Server Express che si trova sotto il \App\_directory dei dati del progetto dell'applicazione NerdDinner. Poiché il progetto NerdDinner.Tests viene compilato ed eseguito in una directory diversa quindi il progetto di applicazione, il percorso relativo del nostro della stringa di connessione non è corretto.

Abbiamo *è stato possibile* risolvere il problema, copiare il file di database SQL Express per il progetto di test e quindi aggiungere una stringa di connessione appropriato di test ad esso nell'app. config del progetto test. Si otterrebbero i test precedenti ha sbloccato e in esecuzione.

Testing unità di codice usando un database effettivo, tuttavia, è accompagnata da una serie di difficoltà. In particolare:

- Rallenta notevolmente il tempo di esecuzione di unit test. Il tempo necessario per eseguire i test, sono meno probabilità di eseguirli frequentemente. È opportuno che gli unit test per poter essere eseguito in pochi secondi – e potervi operazione come modo naturale come la compilazione del progetto.
- Complica la logica di programma di installazione e pulizia in ambito di test. Si vuole che ogni unit test sia isolato e indipendente dalle altre (con senza effetti collaterali o dipendenze). Quando si usa un database reale che è necessario tenere conto dello stato e ripristinare le impostazioni tra i test.

Verrà ora esaminato uno schema progettuale chiamato "inserimento di dipendenze" che può contribuire a risolvere questi problemi ed evitare la necessità di usare un database reale con i test.

### <a name="dependency-injection"></a>Inserimento di dipendenze

Ora DinnersController è strettamente "a" per la classe DinnerRepository. "Accoppiamento" si riferisce a una situazione in cui una classe in modo esplicito si basa su un'altra classe per poter funzionare:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Poiché la classe DinnerRepository richiede l'accesso a un database, la dipendenza fortemente accoppiata la classe DinnersController presenta alle estremità DinnerRepository backup che richiedono siamo riusciti ad avere un database in ordine per i metodi di azione DinnersController da sottoporre a test.

È possibile aggirare il problema tramite l'uso di uno schema progettuale chiamato "inserimento di dipendenze:" che è un approccio in cui le dipendenze (ad esempio le classi di repository che forniscono l'accesso ai dati) non è più in modo implicito vengono create all'interno delle classi che li utilizzano. Al contrario, possono essere passate in modo esplicito le dipendenze per la classe che li Usa usando gli argomenti del costruttore. Se le dipendenze vengono definite utilizzando le interfacce, abbiamo la flessibilità di passare le implementazioni di dipendenza "fittizio" per gli scenari di unit test. In questo modo sarà possibile creare le implementazioni di dipendenza specifica del test che non necessitano effettivamente dell'accesso a un database.

Per visualizzare questa azione, ora si implementerà l'inserimento di dipendenze con nostri DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Estrazione di un'interfaccia IDinnerRepository

Il primo passaggio sarà possibile creare una nuova interfaccia IDinnerRepository che incapsula il contratto di repository che richiedono ai controller per recuperare e aggiornare Dinners.

È possibile definire questo contratto di interfaccia manualmente facendo clic sulla cartella \Models, e quindi scegliendo il **Add -&gt;nuovo elemento** comando di menu e la creazione di una nuova interfaccia denominata IDinnerRepository.cs.

In alternativa sia possibile usare l'estratto il refactoring di strumenti integrata in Visual Studio Professional (e versioni successive) per automaticamente e creare un'interfaccia per noi dalla nostra classe DinnerRepository esistente. Per estrarre l'interfaccia usando Visual Studio, è sufficiente posizionare il cursore nell'editor di testo sulla classe DinnerRepository e quindi fare clic e scegliere il **refactoring -&gt;Estrai interfaccia** comando di menu:

![](enable-automated-unit-testing/_static/image7.png)

Verrà aperta la finestra di dialogo "Estrai interfaccia" e ci richiesto per il nome dell'interfaccia da creare. Verrà IDinnerRepository per impostazione predefinita e seleziona automaticamente tutti i metodi pubblici della classe DinnerRepository esistente da aggiungere all'interfaccia:

![](enable-automated-unit-testing/_static/image8.png)

Quando si fa clic sul pulsante "ok", Visual Studio aggiungerà una nuova interfaccia IDinnerRepository all'applicazione:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

E la classe DinnerRepository esistente verrà aggiornata in modo che implementi l'interfaccia:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>L'aggiornamento DinnersController per supportare l'inserimento del costruttore

La classe DinnersController per usare la nuova interfaccia verrà ora aggiornata.

Attualmente DinnersController è hardcoded in modo che il campo "dinnerRepository" è sempre una classe DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Verranno modificate, in modo che il campo "dinnerRepository" è di tipo IDinnerRepository anziché DinnerRepository. Quindi, aggiungiamo due costruttori DinnersController pubblici. Uno dei costruttori consente un IDinnerRepository deve essere passato come argomento. L'altro è un costruttore predefinito che usa la nostra implementazione DinnerRepository esistente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Poiché ASP.NET MVC per impostazione predefinita crea classi controller usando i costruttori predefiniti, nostro DinnersController in fase di esecuzione continuerà a usare la classe DinnerRepository per eseguire l'accesso ai dati.

È ora possibile aggiornare i test di unità, tuttavia, per il passaggio in un'implementazione di repository "fittizio" cena usando il costruttore di parametro. Questo repository "fittizio" dinner non richiedono l'accesso al database reale e invece userà i dati di esempio in memoria.

#### <a name="creating-the-fakedinnerrepository-class"></a>Creazione della classe FakeDinnerRepository

È possibile creare una classe FakeDinnerRepository.

Si verrà iniziare creando una directory "Fakes" all'interno del progetto NerdDinner.Tests e quindi aggiungere una nuova classe FakeDinnerRepository (pulsante destro del mouse sulla cartella e scegliere **Add -&gt;nuova classe**):

![](enable-automated-unit-testing/_static/image9.png)

Il codice verrà aggiornato in modo che la classe FakeDinnerRepository implementa l'interfaccia IDinnerRepository. È quindi possibile fare doppio clic su di esso e scegliere il comando di menu di scelta rapida "Implementa interfaccia IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

In questo modo Visual Studio aggiungere automaticamente tutti i membri di interfaccia IDinnerRepository alla nostra classe FakeDinnerRepository con implementazioni predefinite "stub out":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

È quindi possibile aggiornare l'implementazione FakeDinnerRepository per lavorare di fuori di un elenco in memoria&lt;Dinner&gt; raccolta passata come argomento del costruttore:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

È ora disponibile un'implementazione IDinnerRepository fittizio che non richiede un database e può invece usare un elenco in memoria degli oggetti Dinner.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Uso di FakeDinnerRepository con gli Unit test

Si esamini il DinnersController unit test che non è riuscita in precedenza perché il database non era disponibile. È possibile aggiornare i metodi di test per usare un FakeDinnerRepository popolate con dati la cena in memoria per il DinnersController usando il codice seguente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Passiamo ora quando si eseguono questi test sono entrambi:

![](enable-automated-unit-testing/_static/image11.png)

Soprattutto, richiedere solo una frazione di secondo per l'esecuzione e non richiedono alcuna logica complessa installazione/pulizia. È possibile ora unit test tutto il codice metodo azione DinnersController (tra cui listato, paging, dettagli, creare, aggiornare ed eliminare) senza dover fare per connettersi a un database effettivo.

| **Sul lato dell'argomento: Framework di inserimento delle dipendenze** |
| --- |
| Esecuzione di inserimento delle dipendenze manuale (ad esempio, Microsoft è di sopra) funziona correttamente, ma diventa difficile da gestire come il numero di dipendenze e i componenti in un'applicazione aumenta. Sono disponibili diversi framework di inserimento delle dipendenze per .NET che consentono di offrire un'enorme flessibilità di gestione delle dipendenze. Questi Framework, detti anche contenitori "Inversion of Control" (IoC), forniscono meccanismi che consentono a un livello aggiuntivo di supporto della configurazione per specificare e passare le dipendenze per gli oggetti in fase di esecuzione (in genere tramite inserimento del costruttore ). Alcune dell'inserimento di dipendenze OSS più diffusi / infrastrutture IOC in .NET includono: AutoFac Ninject, Spring.NET, StructureMap e Windsor. ASP.NET MVC espone API che consentono agli sviluppatori di partecipare alla risoluzione e alla creazione di un'istanza dei controller e che consente l'inserimento di dipendenze di estendibilità / infrastrutture IoC correttamente l'applicazione da integrare all'interno di questo processo. Usando un framework di inserimento delle dipendenze/IOC sarebbe consentono inoltre di rimuovere il costruttore predefinito dal nostro DinnersController – cui verrebbe rimuovere completamente l'accoppiamento tra i dati e il DinnerRepositorys. Non verrà usato un inserimento di dipendenze / infrastruttura IOC con la nostra applicazione NerdDinner. Ma è un elemento che è stato possibile prendere in considerazione per il futuro se la codebase NerdDinner e funzionalità di aumento delle dimensioni. |

### <a name="creating-edit-action-unit-tests"></a>Creazione di Unit test di modifica azione

Creare ora alcuni unit test per verificare la funzionalità di modifica del DinnersController. Si inizierà da testare la versione HTTP-GET del nostro Modifica azione:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Verrà creato un test che verifica che una visualizzazione supportata da un oggetto DinnerFormViewModel viene eseguito il rendering quando viene richiesto un dinner valido:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Quando si esegue il test, tuttavia, si noterà che abbia esito negativo perché viene generata un'eccezione con riferimento null quando il metodo di modifica accede alla proprietà User.Identity.Name per eseguire il controllo Dinner.IsHostedBy().

L'oggetto utente nella classe base Controller incapsula i dettagli relativi all'utente connesso e viene popolato da ASP.NET MVC durante la creazione del controller in fase di esecuzione. Poiché i test in corso la DinnersController all'esterno di un ambiente server web, l'oggetto utente non è impostato (di conseguenza il riferimento null eccezione).

### <a name="mocking-the-useridentityname-property"></a>Il comportamento fittizio la proprietà User.Identity.Name

Framework di simulazione per facilitare il testing da ci permette di creare dinamicamente fittizie versioni degli oggetti dipendenti che supportano i test. Ad esempio, è possibile usare un framework di simulazione nel test Modifica azione per creare dinamicamente un oggetto utente che nostro DinnersController può usare per cercare un nome utente simulato. Questo modo si evita un riferimento null che venga generata quando si esegue il test.

Esistono molti .NET Framework che può essere usato con ASP.NET MVC di simulazione (è possibile visualizzare un elenco di essi di seguito: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). L'applicazione di NerdDinner useremo un open source il comportamento fittizio denominato "Moq" framework di test, che può essere scaricato gratuitamente dalla [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

Al termine del download, viene aggiunto un riferimento nel progetto NerdDinner.Tests all'assembly Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

Si aggiungerà quindi un metodo di supporto "CreateDinnersControllerAs(username)" per la classe di test che accetta un nome utente come parametro e che quindi la proprietà User.Identity.Name nell'istanza DinnersController "mocks":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Moq utilizziamo sopra per creare un oggetto di simulazione che fakes oggetto ControllerContext (ossia quali ASP.NET MVC passa alle classi Controller per esporre gli oggetti di runtime come utente, la richiesta, risposta e sessione). Si sta chiamando il metodo "SetupGet" per la simulazione per indicare che la proprietà HttpContext.User.Identity.Name ControllerContext deve restituire la stringa di nome utente che è passato al metodo helper.

È possibile simulare un numero qualsiasi di ControllerContext proprietà e metodi. Per illustrare questo concetto è stata aggiunta anche una chiamata SetupGet() per la proprietà Request.IsAuthenticated (che non è effettivamente necessaria per i test seguenti: ma in modo da illustrare come è possibile simulare le proprietà delle richieste). Quando è stata completata un'istanza di simulazione il ControllerContext viene assegnato al DinnersController restituisce il metodo helper.

È possibile scrivere unit test che usano questo metodo helper per testare gli scenari di modifica che interessa diversi utenti:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

E ora quando eseguiamo i test vengono superati:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>UpdateModel() scenari di test

Sono stati creati i test che coprono la versione HTTP-GET dell'azione di modifica. Creare ora alcuni test che verificano la versione HTTP-POST dell'azione di modifica:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Lo scenario di test nuove interessano per supportare con questo metodo di azione è l'utilizzo del metodo UpdateModel() helper nella classe base Controller. Questo metodo di supporto viene usato per associare i valori dei post dei form per l'istanza dell'oggetto cena.

Di seguito sono due test che dimostra come sia possibile fornire modulo inviato i valori per il metodo helper UpdateModel() da utilizzare. Si sarà eseguire questa operazione creando e popolando un oggetto FormCollection e quindi assegnarla alla proprietà "ValueProvider" nel Controller.

Il primo test verifica che in un'operazione di salvataggio è reindirizzato il browser per l'azione di dettagli. Il secondo test verifica che quando viene pubblicato un input non valido l'azione viene visualizzata nuovamente la visualizzazione di modifica nuovamente con un messaggio di errore.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Test conclusioni

Appena trattati i concetti di base coinvolti nelle classi di unit test controller. È possibile usare queste tecniche per creare facilmente centinaia di semplici test che verificano il comportamento dell'applicazione.

Poiché i controller e i test del modello non richiedono un database effettivo, sono estremamente veloce e facile da eseguire. Sarà possibile eseguire centinaia di test automatizzati in secondi e ottenere immediatamente feedback se una modifica apportate si Interrompeva qualcosa. Ciò consentirà di fornire il livello di confidenza per continuamente migliorare, refactoring e rifinire la nostra applicazione.

Abbiamo affrontato test come ultimo argomento in questo capitolo, ma non perché il test è un elemento è necessario eseguire la fine di un processo di sviluppo! Al contrario, è consigliabile scrivere i test automatizzati non appena possibile nel processo di sviluppo. Ciò consente quindi ottenere un feedback immediato, quando si sviluppa, consente di pensare attentamente a casi d'uso dell'applicazione e viene spiegato come progettare l'applicazione con pulita su livelli e accoppiamento presente.

Un capitolo del libro successivo verrà illustrati Test Driven Development (TDD) e come usarlo con ASP.NET MVC. TDD è una procedura di codifica iterativa in cui devi innanzitutto scrivere i test che può soddisfare il codice risulta. Con TDD iniziare mediante la creazione di un test che verifica la funzionalità che sta tentando di implementare ogni funzionalità. La scrittura di unit test prima di tutto aiuta a garantire comprendere chiaramente la funzionalità e come deve per operare. Solo dopo che il test è scritto (e aver verificato che abbia esito negativo) non è, implementare la funzionalità di che test di verifica. Poiché si è già trascorso del tempo a pensare il caso d'uso del modo in cui la funzionalità deve per operare, si avrà una migliore comprensione dei requisiti e il modo migliore per la relativa implementazione. Al termine con l'implementazione è possibile eseguire nuovamente il test e ottenere un feedback immediato agli come se la funzionalità funziona correttamente. Si affronterà TDD più nel capitolo 10.

### <a name="next-step"></a>Passo successivo

Alcuni finale a capo dei commenti.

> [!div class="step-by-step"]
> [Precedente](use-ajax-to-implement-mapping-scenarios.md)
> [Successivo](nerddinner-wrap-up.md)
