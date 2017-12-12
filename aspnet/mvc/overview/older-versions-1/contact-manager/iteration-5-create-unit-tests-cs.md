---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
title: 'Iterazione #5: creazione di unit test (c#) | Documenti Microsoft'
author: microsoft
description: "Nella quinta iterazione, si rende l'applicazione di più facile da gestire e modificare tramite l'aggiunta di unit test. È simulare il nostro classi del modello di dati e generare unit test per o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 28ad8f80-b8a5-444e-b478-8b15a846060c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
msc.type: authoredcontent
ms.openlocfilehash: f9b2d05ec8756d68f6bd2f387c85faf03abd167e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="iteration-5--create-unit-tests-c"></a>Iterazione #5: creare unit test (c#)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-5-create-unit-tests-cs/_static/contactmanager_5_cs1.zip)

> Nella quinta iterazione, si rende l'applicazione di più facile da gestire e modificare tramite l'aggiunta di unit test. È simulare il nostro classi del modello di dati e generare unit test per i controller e logica di convalida.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creazione di un'applicazione ASP.NET MVC di gestione dei contatti (c#)

In questa serie di esercitazioni, si compila un'intera applicazione di gestione dei contatti dall'inizio completamento. L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica: per un elenco di persone.

È compilare l'applicazione più iterazioni. A ogni iterazione, gradualmente è migliorare l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1 - creare l'applicazione. Nella prima iterazione, verranno create Contact Manager in modo più semplice possibile. Viene aggiunto il supporto per le operazioni di database basic: creazione, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2 - verificare l'applicazione l'aspetto. In questa iterazione, è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master di visualizzazione ASP.NET MVC e foglio di stile CSS.

- Iterazione #3 - aggiungere la convalida dei form. Nella terza iterazione, è aggiungere la convalida di form di base. È impedire agli utenti di inviare un modulo senza completare i campi modulo necessari. È inoltre possibile convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4: verificare l'applicazione ad accoppiamento debole. In questa terza iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice gestire e modificare l'applicazione Gestione contatti. Ad esempio, si effettua il refactoring l'applicazione di utilizzare il modello di Repository e il modello di inserimento di dipendenze.

- Iterazione #5 - creare unit test. Nella quinta iterazione, si rende l'applicazione di più facile da gestire e modificare tramite l'aggiunta di unit test. È simulare il nostro classi del modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione &#6; - utilizzare sviluppo basato su test. In questa iterazione sesto è aggiungere nuove funzionalità per l'applicazione scrivendo unit test prima e la scrittura di codice per gli unit test. In questa iterazione, è aggiungere gruppi di contatti.

- Iterazione #7 - aggiunta di funzionalità Ajax. Nella settima iterazione, è migliorare la velocità di risposta e prestazioni dell'applicazione aggiunta del supporto per Ajax.


## <a name="this-iteration"></a>Questa iterazione

Nell'iterazione precedente dell'applicazione Gestione contatti, abbiamo suddiviso all'applicazione di essere più strettamente collegati. L'applicazione in controller distinti, servizio e livelli di repository, abbiamo separato. Ogni livello interagisce con il livello sottostante tramite le interfacce.

Abbiamo suddiviso l'applicazione per rendere più facile da gestire e modificare l'applicazione. Ad esempio, se è necessario utilizzare una nuova tecnologia di accesso ai dati, è possibile modificare semplicemente il livello di repository senza toccare il controller o il livello di servizio. Rendendo il responsabile del contatto ad accoppiamento debole, abbiamo ve apportate più flessibile per modificare l'applicazione.

Ma cosa accade quando è necessario aggiungere una nuova funzionalità per l'applicazione Gestione contatti? In alternativa, cosa accade quando è possibile risolvere un bug? Un verità sad, ma anche si basa sulla collaudata, della scrittura del codice è che ogni volta che si tocca codice creato il rischio di introdurre nuovi bug.

Ad esempio, un giorno di fine, il manager potrebbe essere richiesta per aggiungere una nuova funzionalità per la gestione del contatto. È possibile aggiungere il supporto per i gruppi di contatto desiderate. Si desidera, è possibile consentire agli utenti di organizzare i contatti in gruppi, ad esempio amici, Business e così via.

Per implementare questa nuova funzionalità, è necessario modificare tutti i tre livelli dell'applicazione Contact Manager. È necessario aggiungere nuove funzionalità per i controller, il livello di servizio e del repository. Non appena si inizia la modifica del codice, si rischia di funzionalità che funzionavano prima di rilievo.

Refactoring l'applicazione in livelli distinti, come è stato fatto nell'iterazione precedente, è un vantaggio. È un vantaggio poiché consente di apportare modifiche ai livelli intera senza toccare il resto dell'applicazione. Tuttavia, se si desidera rendere più facile da gestire e modificare il codice all'interno di un livello, è necessario creare unit test per il codice.

Utilizzare una unit test a test una singola unità di codice. Queste unità di codice sono inferiori a livelli dell'intera applicazione. In genere, si utilizza uno unit test per verificare se un particolare metodo nel codice si comporta nel modo previsto. Ad esempio, si creerà uno unit test per il metodo CreateContact() esposto dalla classe ContactManagerService.

Gli unit test per un lavoro dell'applicazione solo come rete di protezione. Quando si modifica il codice in un'applicazione, è possibile eseguire un set di unit test per verificare se la modifica si interrompe la funzionalità esistente. Unit test rendere sicuro modificare il codice. Unit test creare tutto il codice dell'applicazione più flessibile per modificare.

In questa iterazione, è aggiungere unit test per l'applicazione di gestione di contatto. In questo modo, nell'iterazione successiva, è possibile aggiungere gruppi di contatto per l'applicazione senza doversi preoccupare di compromettere le funzionalità esistenti.

> [!NOTE] 
> 
> Sono disponibili un'ampia gamma di unit test Framework, ad esempio NUnit xUnit.net e MbUnit. In questa esercitazione, si usa il framework unit test inclusi in Visual Studio. Tuttavia, è possibile leggerlo altrettanto facilmente utilizzare uno di questi Framework alternativi.


## <a name="what-gets-tested"></a>Ciò che verrà sottoposta a test

Nel mondo ideale, tutto il codice rientrerebbero unit test. Nel mondo ideale, sarebbe necessario perfetta rete di protezione. È possibile modificare qualsiasi riga di codice nell'applicazione e sapere immediatamente, tramite l'esecuzione di unit test, se la modifica ha superato le funzionalità esistenti.

Tuttavia, non abbiamo t in tempo reale in una situazione ideale. In pratica, durante la scrittura di unit test, è concentrarsi sulla scrittura di test per la logica di business (ad esempio, la logica di convalida). In particolare, si *non* scrivere unit test per i dati di accesso logica o la logica di visualizzazione.

Per essere utile, è necessario eseguire gli unit test molto rapidamente. È facilmente possibile accumulare centinaia o migliaia di unit test per un'applicazione. Se gli unit test richiedere molto tempo per l'esecuzione, quindi permette di evitare di eseguirle. In altre parole, tempo di esecuzione di unit test sono inutilizzabili per la codifica di un giorno a altro scopo.

Per questo motivo, è in genere non scrivere unit test per codice che interagisce con un database. Che eseguono centinaia di unit test su un database in tempo reale sarebbe troppo lento. Al contrario, simulare il database e scrivere codice che interagisce con il database fittizio (approfondiremo tali un database sotto).

Per un motivo simile, è in genere non scrivere unit test per le viste. Per testare una vista, è necessario attivare un server web. Poiché la rotazione di un server web è un processo relativamente lento, non è consigliabile creare unit test per le visualizzazioni.

Se la vista contiene una logica complessa quindi è consigliabile spostare la logica nei metodi di supporto. È possibile scrivere unit test per metodi Helper per l'esecuzione senza la rotazione di un server web.

> [!NOTE] 
> 
> Durante la scrittura di test per la logica di accesso ai dati o di logica di visualizzazione non è consigliabile verificare durante la scrittura di unit test, questi test possono essere molto utili quando edificio funzionale o integrazione test.


> [!NOTE] 
> 
> ASP.NET MVC è il motore di visualizzazione Web Form. Mentre il motore di visualizzazione Web Form è dipendente da un server web, potrebbero non essere altri motori di visualizzazione.


## <a name="using-a-mock-object-framework"></a>Utilizzo di un Framework di oggetti fittizi

Durante la creazione di unit test, è quasi sempre necessario sfruttare i vantaggi di un framework di simulare oggetto. Un framework di simulare oggetto consente di creare oggetti fittizi e stub per le classi nell'applicazione.

Ad esempio, è possibile utilizzare un framework di simulare oggetto per generare una versione fittizia della classe del repository. In questo modo, è possibile utilizzare la classe repository fittizio anziché la classe repository reale negli unit test. Il repository fittizio consente di evitare l'esecuzione del codice del database durante l'esecuzione di uno unit test.

Visual Studio non include un framework di simulare oggetto. Tuttavia, sono disponibili diversi framework di simulare oggetto commerciali e open source di .NET framework:

1. Moq - questo framework è disponibile con la licenza BSD open source. È possibile scaricare Moq da [https://code.google.com/p/moq/](https://code.google.com/p/moq/).
2. Rhino Mocks, questo framework è disponibile con la licenza BSD open source. È possibile scaricare Rhino Mocks da [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock parimenti - si tratta di un framework commerciale. È possibile scaricare una versione di valutazione da [http://www.typemock.com/](http://www.typemock.com/).

In questa esercitazione, ho deciso di utilizzare Moq. Tuttavia, è possibile utilizzare la stessa facilità Rhino Mocks o parimenti Typemock per la simulazione di creare oggetti per l'applicazione Gestione contatti.

Prima di poter utilizzare Moq, è necessario completare i passaggi seguenti:

1. .
2. Prima di aver decompresso il download, assicurarsi che il file e di fare clic sul pulsante **Unblock** (vedere la figura 1).
3. Decomprimere il download.
4. Aggiungere un riferimento all'assembly Moq facendo clic sulla cartella riferimenti nel progetto ContactManager.Tests e selezionando **Aggiungi riferimento**. Nella scheda Sfoglia, selezionare la cartella in cui è stato decompresso Moq e selezionare l'assembly Moq.dll. Fare clic su di **OK** pulsante.
5. Dopo aver completato questi passaggi, la cartella Riferimenti dovrebbe essere come nella figura 2.


[![Sblocco Moq](iteration-5-create-unit-tests-cs/_static/image1.jpg)](iteration-5-create-unit-tests-cs/_static/image1.png)

**Figura 01**: Moq sblocco ([fare clic per visualizzare l'immagine ingrandita](iteration-5-create-unit-tests-cs/_static/image2.png))


[![Riferimenti dopo l'aggiunta di Moq](iteration-5-create-unit-tests-cs/_static/image2.jpg)](iteration-5-create-unit-tests-cs/_static/image3.png)

**Figura 02**: riferimenti dopo l'aggiunta di Moq ([fare clic per visualizzare l'immagine ingrandita](iteration-5-create-unit-tests-cs/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Creazione di Unit test per il livello di servizio

Consente di iniziare creando un set di unit test per il livello di servizio di Contact Manager applicazione s s. Si userà questi test per verificare la logica di convalida.

Creare una nuova cartella denominata modelli nel progetto ContactManager.Tests. Successivamente, la cartella modelli e scegliere **Aggiungi, nuovo Test**. Il **Aggiungi nuovo Test** viene visualizzata la finestra illustrata nella figura 3. Selezionare il **Unit Test** modello e denominare il nuovo test ContactManagerServiceTest.cs. Fare clic su di **OK** pulsante per aggiungere il nuovo test al progetto di Test.

> [!NOTE] 
> 
> In generale, si desidera la struttura di cartelle del progetto di Test per corrispondere alla struttura della cartella del progetto ASP.NET MVC. Ad esempio, inserire i test controller in una cartella controller, il test del modello in una cartella di modelli e così via.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-cs/_static/image3.jpg)](iteration-5-create-unit-tests-cs/_static/image5.png)

**Figura 03**: Models\ContactManagerServiceTest.cs ([fare clic per visualizzare l'immagine ingrandita](iteration-5-create-unit-tests-cs/_static/image6.png))


Inizialmente, si vuole testare il metodo CreateContact() esposto dalla classe ContactManagerService. Si creerà i cinque test seguenti:

- Test che CreateContact() CreateContact() - restituisce il valore true quando un contatto valido viene passato al metodo.
- CreateContactRequiredFirstName() - test che un messaggio di errore viene aggiunto allo stato di modello quando un contatto con un nome mancano viene passato al metodo CreateContact().
- CreateContactRequredLastName() - test che un messaggio di errore viene aggiunto allo stato di modello quando un contatto con un cognome mancano viene passato al metodo CreateContact().
- CreateContactInvalidPhone() - test, che viene aggiunto un messaggio di errore per lo stato del modello quando un contatto con un numero di telefono non valido viene passato al metodo CreateContact().
- CreateContactInvalidEmail() - test che un messaggio di errore viene aggiunto per lo stato del modello quando un contatto con un indirizzo di posta elettronica non valido viene passato al metodo CreateContact()...

Il primo test verifica che un contatto valido non genera un errore di convalida. I restanti test consente di controllare ogni delle regole di convalida.

Il codice per questi test è contenuto in elenco 1.

**Elenco 1 - Models\ContactManagerServiceTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample1.cs)]


Poiché si utilizza la classe di contatto nel listato 1, è necessario aggiungere un riferimento a Entity Framework Microsoft per il progetto di Test. Aggiungere un riferimento all'assembly System.Data.Entity.


Elenco 1 contiene un metodo denominato Initialize () che è decorata con l'attributo [TestInitialize]. Questo metodo viene chiamato automaticamente prima di eseguita ognuno degli unit test (viene chiamato 5 volte immediatamente prima di ogni unit test). Il metodo Initialize () crea un repository fittizio con la riga di codice seguente:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample2.cs)]

Questa riga di codice utilizza il framework Moq per generare un repository fittizio dall'interfaccia IContactManagerRepository. Il repository fittizio viene utilizzato anziché il EntityContactManagerRepository effettivo per evitare l'accesso al database quando viene eseguito ogni unit test. Il repository fittizio implementa i metodi dell'interfaccia IContactManagerRepository, ma in realtà non t don metodi.

> [!NOTE] 
> 
> Quando si utilizza il framework Moq, viene fatta distinzione tra \_mockRepository e \_mockRepository.Object. Il primo si intende la simulazione&lt;IContactManagerRepository&gt; classe che contiene metodi per specificare il comportamento di repository fittizio. Il secondo si riferisce al repository fittizio effettivo che implementa l'interfaccia IContactManagerRepository.


Il repository fittizio viene utilizzato nel metodo Initialize () quando si crea un'istanza della classe ContactManagerService. Tutti i singoli unit test utilizzare questa istanza della classe ContactManagerService.

Elenco 1 contiene cinque metodi che corrispondono a ognuno degli unit test. Ognuno di questi metodi è decorata con l'attributo [TestMethod]. Quando si esegue gli unit test, viene chiamato qualsiasi metodo che dispone di questo attributo. In altre parole, qualsiasi metodo che è decorata con l'attributo [TestMethod] è uno unit test.

Il primo unit test, denominato CreateContact(), verifica che la chiamata CreateContact() restituisce il valore true quando un'istanza della classe contatto valida viene passata al metodo. Il test crea un'istanza della classe di contatto, chiama il metodo CreateContact() e verifica che CreateContact() restituisce il valore true.

I restanti test di verificare che quando viene chiamato il metodo CreateContact() con un contatto valido quindi il metodo restituisce false e il messaggio di errore di convalida previsto è aggiunto allo stato del modello. Ad esempio, il test CreateContactRequiredFirstName() crea un'istanza della classe contatto con una stringa vuota per la relativa proprietà FirstName. Successivamente, il metodo CreateContact() viene chiamato con il contatto non valido. Infine, il test di verifica che CreateContact() restituisce false e che lo stato del modello contiene il messaggio di errore di convalida previsto "First name è obbligatorio."

È possibile eseguire gli unit test nel listato 1 selezionando l'opzione di menu **Test eseguire, tutti i test nella soluzione (CTRL + R, A)**. I risultati dei test vengono visualizzati nella finestra Risultati Test (vedere la figura 4).


[![Risultati dei test](iteration-5-create-unit-tests-cs/_static/image4.jpg)](iteration-5-create-unit-tests-cs/_static/image7.png)

**Figura 04**: risultati del Test ([fare clic per visualizzare l'immagine ingrandita](iteration-5-create-unit-tests-cs/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Creazione di Unit test per i controller

Applicazione ASP.NETMVC controllare il flusso dell'interazione dell'utente. Quando un controller di test, si desidera verificare se il controller restituisca l'azione a destra di risultati e visualizzare i dati. È anche consigliabile verificare se un controller interagisce con le classi di modello nel modo previsto.

Ad esempio, il listato 2 contiene due unit test per il metodo di metodo di creazione del controller di contatto. Il primo unit test verifica quindi che quindi reindirizza il metodo di creazione per l'operazione sull'indice, un contatto valido viene passato al metodo del metodo di creazione. In altre parole, quando viene passato un contatto valido, il metodo di creazione metodo deve restituire un RedirectToRouteResult che rappresenta l'operazione sull'indice.

Non abbiamo t desidera testare il livello di servizio ContactManager quando sottoposti a test il livello di controller. Pertanto, è simulare il livello di servizio con il codice seguente nel metodo di inizializzazione:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample3.cs)]

Nello unit test, CreateValidContact() è simulare il comportamento della chiamata a livello di servizio metodo CreateContact() con la riga di codice seguente:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample4.cs)]

Questa riga di codice, il servizio di ContactManager fittizio restituire il valore true, quando viene chiamato il metodo CreateContact(). Da tali il livello di servizio, è possibile testare il comportamento di questo controller senza la necessità di eseguire qualsiasi codice del livello di servizio.

Il secondo unit test verifica che l'azione del metodo di creazione restituisce la visualizzazione Create quando un contatto non valido viene passato al metodo. Si causa il livello di servizio CreateContact() metodo per restituire il valore false, con la riga di codice seguente:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample5.cs)]

Se il metodo di metodo di creazione si comporta esattamente come si prevede quindi deve restituire la visualizzazione Create quando il livello di servizio restituisce il valore false. In questo modo, il controller può visualizzare i messaggi di errore di convalida nella visualizzazione di creazione e l'utente ha la possibilità di correggere tale proprietà di contatto non valido.


Se si prevede di unit test per i controller di compilazione è necessario restituire i nomi delle viste esplicita dalle azioni del controller. Ad esempio, non restituiscono una visualizzazione simile al seguente:

restituire View();

Al contrario, restituire la visualizzazione simile al seguente:

restituire View("Create");

Se non si esplicita per la restituzione di una visualizzazione di proprietà ViewResult.ViewName restituisce una stringa vuota.


**Elenco di 2 - Controllers\ContactControllerTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample6.cs)]

## <a name="summary"></a>Riepilogo

In questa iterazione, è creare unit test per l'applicazione Contact Manager. In qualsiasi momento per verificare che l'applicazione continuerà a funzionare in modo che si prevede che è possibile eseguire gli unit test. Gli unit test agire come rete di protezione per l'applicazione permette di modificare in modo sicuro l'applicazione in futuro.

Due set di unit test è stato creato. In primo luogo, è stato testato la logica di convalida tramite la creazione di unit test per il livello di servizio. Successivamente, è stato testato la logica del flusso di controllo mediante la creazione di unit test per il livello di controller. Quando si testa il livello di servizio, è isolata i test per il livello di servizio da questo livello di repository da tali il livello di repository. Quando il livello di controller di test, i test è isolato per il livello di controller in base a tali il livello di servizio.

Nell'iterazione successiva, modificare è l'applicazione Gestione contatti in modo che supporti i gruppi di contatto. Questa nuova funzionalità verrà aggiunto all'applicazione mediante un processo di progettazione software denominato sviluppo basato su test.

>[!div class="step-by-step"]
[Precedente](iteration-4-make-the-application-loosely-coupled-cs.md)
[Successivo](iteration-6-use-test-driven-development-cs.md)
