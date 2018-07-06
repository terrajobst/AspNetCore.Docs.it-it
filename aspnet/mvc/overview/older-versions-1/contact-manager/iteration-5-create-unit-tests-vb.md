---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: 'Iterazione #5-creare gli unit test (VB) | Microsoft Docs'
author: microsoft
description: Nell'iterazione del quinto, si semplifica l'applicazione di manutenzione e la modifica mediante l'aggiunta di unit test. È simulare le classi di modello di dati e generare unit test per o...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: f2a535128816153d9ff9b14d1895dabd967ad871
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806381"
---
<a name="iteration-5--create-unit-tests-vb"></a>Iterazione #5-creare gli unit test (VB)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> Nell'iterazione del quinto, si semplifica l'applicazione di manutenzione e la modifica mediante l'aggiunta di unit test. È simulare le classi di modello di dati e generare unit test per i controller e logica di convalida.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creazione di un'applicazione di gestione dei contatti ASP.NET MVC (VB)

In questa serie di esercitazioni, creiamo un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica - per un elenco di persone.

Si compilerà l'applicazione a varie iterazioni. A ogni iterazione, abbiamo migliorare gradualmente l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1 - creare l'applicazione. Nella prima iterazione, verranno create Contact Manager nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base dei database: crea, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2 - migliorare l'applicazione aspetto interessante. In questa iterazione è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master visualizzazione MVC ASP.NET e foglio di stile CSS.

- Iterazione #3 - aggiungere la convalida dei form. Nella terza iterazione, aggiungiamo la convalida dei form di base. Persone è impedire l'invio di un modulo senza completare i campi del modulo richiesto. È anche convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4 - rendere l'applicazione a regime. In questa terza iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice da gestire e modificare l'applicazione Contact Manager. Ad esempio, è eseguire il refactoring l'applicazione per usare il modello di Repository e il modello di inserimento delle dipendenze.

- Iterazione #5 - creare unit test. Nell'iterazione del quinto, si semplifica l'applicazione di manutenzione e la modifica mediante l'aggiunta di unit test. È simulare le classi di modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione #6 - usare lo sviluppo basato su test. In questa iterazione sesta, viene aggiunto nuove funzionalità all'applicazione scrivendo unit test prima e scrivere codice sull'unit test. In questa iterazione, viene aggiunto gruppi di contatti.

- Iterazione #7 - aggiungere funzionalità Ajax. Nell'iterazione del settimo, aggiungendo il supporto per Ajax è migliorare la velocità di risposta e le prestazioni dell'applicazione.


## <a name="this-iteration"></a>Questa iterazione

Nell'iterazione precedente dell'applicazione Contact Manager, viene effettuato il refactoring all'applicazione di essere più regime di controllo. Abbiamo separato l'applicazione in livelli di repository, servizio e controller distinti. Ogni livello interagisce con il livello sottostante tramite le interfacce.

Abbiamo effettuato il refactoring l'applicazione per rendere più semplice da gestire e modificare l'applicazione. Ad esempio, se è necessario usare una nuova tecnologia di accesso ai dati, è possibile modificare semplicemente il livello di repository senza modificare il controller o il livello di servizio. Rendendo il Contact Manager regime di controllo è ve reso più resiliente per modificare l'applicazione.

Ma, cosa accade quando è necessario aggiungere una nuova funzionalità per l'applicazione Contact Manager? Oppure, cosa accade quando si corregge un bug? Una verità triste ma ben collaudato, della scrittura del codice è che ogni volta che si tocca codice si crea il rischio di introdurre nuovi bug.

Ad esempio, un giorno di fine, i manager potrebbe essere richiesta per aggiungere una nuova funzione a Gestione contatti. Anna vuole è possibile aggiungere il supporto per i gruppi di contatto. Anna vuole consentire agli utenti di organizzare i propri contatti in gruppi come elementi Friend, Business e così via.

Per implementare questa nuova funzionalità, è necessario modificare tutti i tre livelli dell'applicazione Contact Manager. È necessario aggiungere nuove funzionalità per i controller, il livello di servizio e il repository. Non appena si avvia la modifica del codice, si rischia di compromettere funzionalità che ha lavorato prima.

Refactoring l'applicazione in livelli distinti, come è stato fatto nell'iterazione precedente, è stato positivo. Era un aspetto positivo perché ci permette di apportare modifiche ai livelli interi senza toccare il resto dell'applicazione. Tuttavia, se si desidera rendere più semplice da gestire e modificare il codice all'interno di un livello, è necessario creare unit test per il codice.

È usare una unit test per testare una singola unità di codice. Queste unità di codice sono inferiori a livelli dell'intera applicazione. In genere, utilizzare uno unit test per verificare se un particolare metodo nel codice si comporta nel modo previsto. Ad esempio, si creerà uno unit test per il metodo CreateContact() esposto dalla classe ContactManagerService.

Gli unit test per un solo il lavoro dell'applicazione, ad esempio una rete di protezione. Ogni volta che si modifica il codice in un'applicazione, è possibile eseguire un set di unit test per verificare se la modifica interrompe la funzionalità esistente. Gli unit test rendono sicuro modificare il codice. Gli unit test rendere tutto il codice nell'applicazione più resiliente a cambiare.

In questa iterazione, unit test viene aggiunto all'applicazione Contact Manager. In questo modo, nell'iterazione successiva, è possibile aggiungere gruppi di contatto per l'applicazione senza preoccuparsi di infrangere le funzionalità esistenti.

> [!NOTE] 
> 
> Sono disponibili un'ampia gamma di unit test Framework, tra cui MbUnit NUnit e xUnit.net. In questa esercitazione, si usa il framework di unit test inclusi in Visual Studio. Tuttavia, è possibile semplicemente utilizzare uno di questi Framework alternativi.


## <a name="what-gets-tested"></a>Che cosa verrà sottoposta a test

Nel mondo perfetto, tutto il codice potrebbe essere coperto da unit test. In teoria, sarebbe necessario la rete di protezione perfetta. È possibile modificare qualsiasi riga di codice nell'applicazione e sapere immediatamente, eseguendo gli unit test, se la modifica si è interrotta funzionalità esistenti.

Tuttavia, non abbiamo t in tempo reale in un mondo perfetto. In pratica, durante la scrittura di unit test, di concentrarsi sulla scrittura di test per la logica di business (ad esempio, la logica di convalida). In particolare, si *non li* scrivere unit test per i dati di accesso per la logica o la logica di visualizzazione.

Per essere utile, è necessario eseguire gli unit test molto rapidamente. È facilmente possibile accumulare centinaia o anche migliaia di unit test per un'applicazione. Se gli unit test richiedere molto tempo da eseguire, si eviteranno eseguirli. In altre parole, tempo esecuzione di unit test sono inutili per la codifica a scopo di giorno in giorno.

Per questo motivo, in genere non scrivere unit test per codice che interagisce con un database. Esecuzione di centinaia di unit test in un database in tempo reale sarebbe troppo lenta. In alternativa, simulare il database e scrivere codice che interagisce con il database fittizio (verrà descritto il comportamento fittizio di un database sottostante).

Per un motivo simile, in genere non scrivere unit test per le visualizzazioni. Per testare una vista, è necessario attivare un server web. Poiché la rotazione di un server web è un processo relativamente lento, non è consigliabile creare gli unit test per le visualizzazioni.

Se la vista contiene logica complessa quindi è consigliabile spostare la logica in metodi di supporto. È possibile scrivere unit test per metodi Helper per l'esecuzione senza avviare un server web.

> [!NOTE] 
> 
> Durante la scrittura di test per la logica di accesso ai dati o la logica di visualizzazione non è una buona idea durante la scrittura di unit test, questi test possono risultare estremamente utili edificio funzionale o integrazione dei test.


> [!NOTE] 
> 
> ASP.NET MVC è il motore di visualizzazione Web Form. Mentre il motore di visualizzazione Web Form è dipendente da un server web, potrebbero non essere altri motori di visualizzazione.


## <a name="using-a-mock-object-framework"></a>Grazie a una struttura oggetto fittizio

Durante la creazione di unit test, è necessario quasi sempre possibile sfruttare un framework di oggetti fittizi. Un framework di oggetti fittizi consente di creare gli oggetti fittizi e stub per le classi nell'applicazione.

Ad esempio, è possibile utilizzare un framework di oggetti fittizi per generare una versione fittizia della propria classe di repository. In questo modo, è possibile usare la classe di repository fittizio anziché la classe di repository reali negli unit test. Usando il repository fittizio consente di evitare l'esecuzione di codice del database durante l'esecuzione di uno unit test.

Visual Studio non include un framework di oggetti fittizi. Tuttavia, sono disponibili diversi framework di oggetti fittizi commerciali e open source per .NET framework:

1. Moq - questo framework è disponibile con la licenza BSD open source. È possibile scaricare da Moq [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/).
2. Rhino Mocks, questo framework è disponibile con la licenza BSD open source. È possibile scaricare Rhino Mocks dal [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Isolator - questo è un framework commercio. È possibile scaricare una versione di valutazione [ http://www.typemock.com/ ](http://www.typemock.com/).

In questa esercitazione, ho deciso di utilizzare Moq. Tuttavia, è possibile utilizzare altrettanto facilmente Rhino Mocks o Typemock Isolator per creare la simulazione degli oggetti per l'applicazione Contact Manager.

Prima di poter usare Moq, è necessario completare i passaggi seguenti:

1. .
2. Prima di aver decompresso il download, assicurarsi di fare doppio clic su file e fare clic sul pulsante con etichetta **Unblock** (vedere la figura 1).
3. Decomprimere il download.
4. Aggiungere un riferimento all'assembly Moq al progetto di Test selezionando l'opzione di menu **progetto, Aggiungi riferimento** per aprire il **Aggiungi riferimento** finestra di dialogo. Nella scheda esplorazione, passare alla cartella in cui è stato decompresso Moq e selezionare l'assembly Moq.dll. Scegliere il **OK** pulsante (vedere la figura 2).


[![Moq sblocco](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Figura 01**: Moq sblocco ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-5-create-unit-tests-vb/_static/image2.png))


[![Riferimenti dopo l'aggiunta di Moq](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Figura 02**: i riferimenti dopo l'aggiunta di Moq ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Creazione di Unit test per il livello di servizio

Let s iniziare creando un set di unit test per il livello di servizio s applicazione Contact Manager. Si userà questi test per verificare la logica di convalida.

Creare una nuova cartella denominata Models nel progetto ContactManager.Tests. Successivamente, la cartella Models e scegliere **Aggiungi, nuovo Test**. Il **Aggiungi nuovo Test** finestra di dialogo illustrata nella figura 3 viene visualizzato. Selezionare il **Unit Test** modello e denominare il nuovo test ContactManagerServiceTest.vb. Scegliere il **OK** pulsante per aggiungere il nuovo test al progetto di Test.

> [!NOTE] 
> 
> In generale, si desidera che la struttura di cartelle del progetto di Test per corrispondere alla struttura di cartelle del progetto ASP.NET MVC. Ad esempio, inserire i test controller in una cartella Controllers, i test del modello in una cartella modelli e così via.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Figura 03**: Models\ContactManagerServiceTest.cs ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-5-create-unit-tests-vb/_static/image6.png))


Inizialmente, si vuole testare il metodo CreateContact() esposto dalla classe ContactManagerService. Si creeranno i cinque test seguenti:

- I test che CreateContact() CreateContact() - restituisce il valore true quando un contatto valido viene passato al metodo.
- CreateContactRequiredFirstName() - test che un messaggio di errore viene aggiunto a quando un contatto con un nome mancano lo stato del modello viene passato al metodo CreateContact().
- CreateContactRequredLastName() - test che un messaggio di errore viene aggiunto a quando un contatto con un cognome mancano lo stato del modello viene passato al metodo CreateContact().
- CreateContactInvalidPhone() - test che viene aggiunto un messaggio di errore per lo stato del modello quando un contatto con un numero di telefono non valido viene passato al metodo CreateContact().
- CreateContactInvalidEmail() - test che viene aggiunto un messaggio di errore per lo stato del modello quando un contatto con un indirizzo di posta elettronica non valido viene passato al metodo CreateContact()...

Il primo test verifica che un contatto valido non genera un errore di convalida. I restanti test verificare ognuna delle regole di convalida.

Il codice per questi test è contenuto nel listato 1.

**Listato 1 - Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


Poiché si usa la classe di contatto nel listato 1, è necessario aggiungere un riferimento a Microsoft Entity Framework per il progetto di Test. Aggiungere un riferimento all'assembly Data. Entity.


L'elenco 1 contiene un metodo denominato Initialize () che è decorata con l'attributo [TestInitialize]. Questo metodo viene chiamato automaticamente prima esecuzione della ognuno degli unit test (viene chiamato 5 volte subito prima della ognuno degli unit test). Il metodo Initialize () consente di creare un repository fittizio con la riga di codice seguente:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Questa riga di codice Usa il framework Moq per generare un repository fittizio dall'interfaccia IContactManagerRepository. Il repository fittizio viene utilizzato anziché il EntityContactManagerRepository effettivo per evitare l'accesso al database quando viene eseguito ogni unit test. Il repository fittizio implementa i metodi dell'interfaccia IContactManagerRepository, ma in realtà non t don metodi.

> [!NOTE] 
> 
> Quando si usa il framework Moq, vi è una differenza tra \_mockRepository e \_mockRepository.Object. Il primo si riferisce alla classe di simulazione (di IContactManagerRepository) che contiene i metodi che consentono di specificare come si comporterà il repository fittizio. Quest'ultimo si riferisce al repository fittizio effettivo che implementa l'interfaccia IContactManagerRepository.


Il repository fittizio viene usato nel metodo Initialize () quando si crea un'istanza della classe ContactManagerService. Tutti i singoli unit test utilizzare questa istanza della classe ContactManagerService.

L'elenco 1 contiene cinque metodi che corrispondono a ognuno degli unit test. Ognuno di questi metodi è decorata con l'attributo [TestMethod]. Quando si eseguono gli unit test, viene chiamato qualsiasi metodo con questo attributo. In altre parole, qualsiasi metodo che è decorata con l'attributo [TestMethod] è uno unit test.

Il primo unit test, denominato CreateContact(), verifica che la chiamata a CreateContact() restituisce il valore true quando un'istanza valida alla classe Contact viene passata al metodo. Il test viene creata un'istanza della classe di contatto, chiama il metodo CreateContact() e verifica che CreateContact() restituisce il valore true.

I restanti test verificare che quando viene chiamato il metodo CreateContact() con un contatto valido quindi il metodo restituisce false e il messaggio di errore di convalida previsto viene aggiunto per lo stato del modello. Ad esempio, il test CreateContactRequiredFirstName() crea un'istanza della classe contatto con una stringa vuota per la relativa proprietà FirstName. Successivamente, il metodo CreateContact() viene chiamato con il contatto non valido. Infine, il test verifica che CreateContact() restituisce false e che lo stato del modello contiene il messaggio di errore di convalida previsto "First name è obbligatoria".

È possibile eseguire gli unit test nel listato 1, selezionando l'opzione di menu **esecuzione dei Test, tutti i test nella soluzione (CTRL + R, A)**. I risultati dei test vengono visualizzati nella finestra Risultati Test (vedere la figura 4).


[![Risultati dei test](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Figura 04**: i risultati del Test ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Creazione di Unit test per i controller

Applicazione ASP.NET MVC controllare il flusso di interazione dell'utente. Quando un controller di test, si desidera verificare se il controller restituisce i risultato e visualizzare i dati di azione a destra. È anche possibile verificare se un controller interagisce con le classi di modello nel modo previsto.

Listato 2, ad esempio, contiene due unit test per il controller di contatto metodo Create (). Il primo unit test verifica quindi che un contatto valido viene passato al metodo Create (), quindi il metodo Create () reindirizza l'operazione sull'indice. In altre parole, quando viene passato un contatto valido, il metodo Create () deve restituire un RedirectToRouteResult che rappresenta l'operazione sull'indice.

Temiamo t vuole testare il livello di servizio ContactManager quando sono sottoposti a test il livello di controller. Pertanto, è simulare il livello di servizio con il codice seguente nel metodo di inizializzazione:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

Nello unit test, CreateValidContact() è simulare il comportamento della chiamata a livello di servizio CreateContact() metodo con la riga di codice seguente:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Questa riga di codice, il servizio ContactManager fittizio restituire il valore true quando viene chiamato il metodo CreateContact(). Per il livello di servizio di simulazione, è possibile testare il comportamento del controller senza la necessità di eseguire il codice nel livello di servizio.

Il secondo di unit test verifica che l'azione create () restituisce la visualizzazione Create quando un contatto non valido viene passato al metodo. Abbiamo causano il livello di servizio CreateContact() metodo per restituire il valore false con la riga di codice seguente:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Se il metodo Create () si comporta come prevedevamo quindi deve restituire la visualizzazione Create quando il livello di servizio restituisce il valore false. In questo modo, il controller può visualizzare i messaggi di errore di convalida nella visualizzazione di creazione e l'utente ha la possibilità di correggere tale proprietà di contatto non valide.


Se si prevede di unit test per i controller di compilazione è necessario restituire i nomi di visualizzazione esplicita da azioni del controller. Ad esempio, non restituiscono una visualizzazione simile al seguente:

Restituire View()

Al contrario, restituire la visualizzazione simile al seguente:

Restituire View("Create")

Se non si è esplicita la restituzione di una visualizzazione della proprietà ViewResult.ViewName restituisce una stringa vuota.


**Listato 2 - Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Riepilogo

In questa iterazione, abbiamo creato gli unit test per la nostra applicazione Contact Manager. È possibile eseguire questi unit test in qualsiasi momento per verificare che l'applicazione continuerà a funzionare nel modo che presumibilmente. Gli unit test fungono da una rete di protezione per la nostra applicazione permette di modificare in modo sicuro l'applicazione in futuro.

Abbiamo creato due insiemi di unit test. In primo luogo, è stato testato la logica di convalida tramite la creazione di unit test per il livello di servizio. Successivamente, abbiamo testato la logica del flusso di controllo tramite la creazione di unit test per il livello di controller. Quando si testa il livello di servizio, è isolato i test per il livello di servizio dal nostro livello di repository per il livello del repository di simulazione. Quando il livello di controller di test, abbiamo isolato i test per il livello di controller in base al livello di servizio di simulazione.

Nell'iterazione successiva, si modifica l'applicazione Contact Manager, in modo che supporti i gruppi di contatto. Si aggiungerà questa nuova funzionalità all'applicazione tramite un processo di progettazione software denominato test-driven development.

> [!div class="step-by-step"]
> [Precedente](iteration-4-make-the-application-loosely-coupled-vb.md)
> [Successivo](iteration-6-use-test-driven-development-vb.md)
