---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Utilizzo di Entity Framework 4.0 e il controllo ObjectDataSource, parte 2: aggiunta di un livello di logica di Business e gli Unit test | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni compila nell'applicazione web di Contoso University creato da introduttiva la serie di esercitazioni di Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 0440f807c7baa7b92e5f05590eca9cc237b5aef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Utilizzo di Entity Framework 4.0 e il controllo ObjectDataSource, parte 2: aggiunta di un livello di logica di Business e gli Unit test
====================
Da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web di Contoso University creando il [introduzione di Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni. Se non ha completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) che consente di creare. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creati tramite la serie di esercitazioni completo. Nel caso di problemi con le esercitazioni, è possibile registrarli per il [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Nell'esercitazione precedente è stato creato un'applicazione web a più livelli tramite Entity Framework e `ObjectDataSource` controllo. In questa esercitazione viene illustrato come aggiungere logica di business, mantenendo il livello di logica di business (BLL) e il livello di accesso ai dati (DAL) separato e viene illustrato come creare unit test automatizzati per il livello Business LOGIC.

In questa esercitazione si completeranno le attività seguenti:

- Creare un repository di interfaccia che dichiara i metodi di accesso ai dati, che è necessario.
- Implementare l'interfaccia del repository nella classe del repository.
- Creare una classe di logica di business che chiama la classe di repository per eseguire funzioni di accesso ai dati.
- Connettere il `ObjectDataSource` controllo alla classe della logica di business anziché per la classe di repository.
- Creare un progetto di unit test e una classe di repository che usa raccolte in memoria per il proprio archivio dati.
- Creare uno unit test per la logica di business che si desidera aggiungere alla classe della logica di business, quindi esegue il test e visualizzarlo esito negativo.
- Implementare la logica di business nella classe della logica di business, quindi eseguire nuovamente l'unità di test e visualizzarlo passare.

Si utilizzerà il *Departments.aspx* e *DepartmentsAdd.aspx* pagine creato nell'esercitazione precedente.

## <a name="creating-a-repository-interface"></a>Creazione di un'interfaccia del Repository

Inizierai creando l'interfaccia del repository.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

Nel *DAL* cartella, creare un nuovo file di classe, denominarlo *ISchoolRepository.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

L'interfaccia definisce un metodo per ogni il CRUD (create, leggere, aggiornare ed eliminare) i metodi che è stato creato nella classe del repository.

Nel `SchoolRepository` classe *SchoolRepository.cs*, indicare che questa classe implementa il `ISchoolRepository` interfaccia:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Creazione di una classe di logica di Business

Successivamente, si creerà la classe di logica di business. Questo caso, in modo che sia possibile aggiungere logica di business che verrà eseguita dal `ObjectDataSource` controllare, anche se non si verrà eseguita che ancora. Per il momento, solo la nuova classe di logica di business eseguirà le stesse operazioni CRUD che esegue il repository.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Creare una nuova cartella e denominarla *BLL*. (In un'applicazione reale, il livello di logica di business verrebbe in genere implementato come libreria di classi, ovvero un progetto separato, ma per rendere semplice l'esercitazione, classi BLL verranno conservate in una cartella di progetto.)

Nel *BLL* cartella, creare un nuovo file di classe, denominarlo *SchoolBL.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Questo codice crea gli stessi metodi CRUD che è stato illustrato in precedenza nella classe del repository, ma anziché accedere direttamente i metodi di Entity Framework, chiama il repository metodi della classe.

La variabile di classe che contiene un riferimento alla classe del repository è definita come un tipo di interfaccia e il codice che crea un'istanza della classe di repository è contenuto in due costruttori. Il costruttore senza parametri da utilizzare per il `ObjectDataSource` controllo. Crea un'istanza di `SchoolRepository` classe creata in precedenza. L'altro costruttore consente qualsiasi codice che crea un'istanza della classe di logica di business di passare qualsiasi oggetto che implementa l'interfaccia del repository.

I metodi CRUD che chiamano la classe di repository e i due costruttori consentono di utilizzare la classe di logica di business con qualsiasi archivio dati back-end prescelto. La classe di logica di business non è necessario essere a conoscenza di come la classe che viene chiamata rende persistenti i dati. (Si tratta spesso *mancato riconoscimento della persistenza*.) Ciò facilita l'esecuzione di unit test, perché la classe di logica di business è possibile connettersi all'implementazione di un repository che vengono utilizzati come semplice come in memoria `List` raccolte per archiviare i dati.

> [!NOTE]
> Tecnicamente, gli oggetti entità sono ancora non persistenza, perché si è creata un'istanza da classi che ereditano da Entity Framework `EntityObject` classe. Mancato riconoscimento della persistenza completa, è possibile utilizzare *plain old CLR Object*, o *POCOs*, al posto di oggetti da cui ereditare la `EntityObject` classe. Utilizzando POCOs non rientra nell'ambito di questa esercitazione. Per ulteriori informazioni, vedere [testabilità ed Entity Framework 4.0](https://msdn.microsoft.com/en-us/library/ff714955.aspx) nel sito Web MSDN.)


Ora è possibile connettere il `ObjectDataSource` controlli per la logica di business classe invece che al repository e verificare che tutto funzioni come in precedenza.

In *Departments.aspx* e *DepartmentsAdd.aspx*, sostituire ogni occorrenza del `TypeName="ContosoUniversity.DAL.SchoolRepository"` a `TypeName="ContosoUniversity.BLL.SchoolBL`". (Sono presenti quattro istanze in tutte.)

Eseguire il *Departments.aspx* e *DepartmentsAdd.aspx* pagine per verificare che funzionino ancora come in precedenza.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Creazione di un progetto di Unit Test e l'implementazione di Repository

Aggiungere un nuovo progetto alla soluzione utilizzando la **progetto di Test** , modello e specificare il nome `ContosoUniversity.Tests`.

Nel progetto di test, aggiungere un riferimento a `System.Data.Entity` e aggiungere un riferimento al progetto il `ContosoUniversity` progetto.

È ora possibile creare la classe di repository che si userà con unit test. L'archivio dati per questo repository sarà all'interno della classe.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

Nel progetto di test, creare un nuovo file di classe, denominarlo *MockSchoolRepository.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Questa classe repository ha gli stessi metodi CRUD di quello che accede direttamente a Entity Framework, ma funzionano con `List` raccolte in memoria anziché con un database. Questo rende più semplice per una classe di test impostare e convalidare gli unit test per la classe di logica di business.

## <a name="creating-unit-tests"></a>Creazione di Unit test

Il **Test** modello di progetto creata una classe stub di unit test e l'attività successiva consiste nella modifica di questa classe aggiungendo metodi di unit test per la logica di business che si desidera aggiungere alla classe della logica di business.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Contoso University, qualsiasi singolo istruttore può essere solo l'amministratore di un singolo reparto ed è necessario aggiungere logica di business per applicare questa regola. Si inizierà con aggiunta di test e l'esecuzione dei test per visualizzarli esito negativo. Si verrà quindi aggiungere il codice e rieseguire i test, è previsto per visualizzarli passare.

Aprire il *UnitTest1.cs* file e aggiungere `using` istruzioni per i livelli di accesso ai dati e di logica di business creati nel progetto ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Sostituire il `TestMethod1` (metodo) con i metodi seguenti:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

Il `CreateSchoolBL` metodo crea un'istanza della classe repository creato per il progetto, che viene quindi passato a una nuova istanza della classe della logica di business unit test. Il metodo utilizza quindi la classe di logica di business per inserire tre reparti che è possibile utilizzare nei metodi di test.

I metodi di test è verificare che la classe di logica di business genera un'eccezione se un utente tenta di inserire una nuova categoria con lo stesso tipo di amministratore una categoria esistente oppure se un utente tenta di aggiornare l'amministratore di un reparto mediante l'impostazione per l'ID di una persona chi è già l'amministratore di un altro reparto.

Ancora creato la classe di eccezione, pertanto questo codice non verrà compilato. Per far sì che la compilazione, fare doppio clic su `DuplicateAdministratorException` e selezionare **genera**e quindi **classe**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Viene creata una classe nel progetto di test che è possibile eliminare dopo aver creato la classe di eccezione nel progetto principale. e implementare la logica di business.

Eseguire il progetto di test. Come previsto, superati.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Aggiunta di logica di Business in modo che passi del Test

Verrà quindi implementare la logica di business che rende impossibile impostato come amministratore di un reparto, un utente è già amministratore di un altro reparto. Verranno genera un'eccezione dal livello di logica di business e quindi viene rilevato nel livello di presentazione se un utente modifica un reparto e fa clic su **aggiornamento** dopo aver selezionato un utente è già un amministratore. (È anche possibile rimuovere i docenti nell'elenco a discesa che sono già amministratori prima che si esegue il rendering della pagina, ma in questo caso lo scopo è usare il livello di logica di business).

Iniziare creando la classe di eccezione che sarà generata quando un utente tenta di stabilire un docente l'amministratore di più di un reparto. Il progetto principale, creare un nuovo file di classe nel *BLL* cartella, il nome *DuplicateAdministratorException.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Ora di eliminazione temporanea *DuplicateAdministratorException.cs* file creato nel progetto di test in precedenza per poter essere in grado di compilare.

Il progetto principale, aprire il *SchoolBL.cs* file e aggiungere il metodo seguente che contiene la logica di convalida. (Il codice fa riferimento a un metodo che verrà creato in un secondo momento).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Si verrà chiamato questo metodo quando si è l'inserimento o aggiornamento `Department` entità per verificare se un altro reparto ha già lo stesso di amministratore.

Il codice chiama un metodo per eseguire ricerche nel database per un `Department` entità che ha lo stesso `Administrator` valore della proprietà dell'entità che viene inserito o aggiornato. Se viene trovata, il codice genera un'eccezione. Nessun controllo di convalida è necessaria se l'entità che viene inserito o aggiornato non ha alcun `Administrator` valore e non l'eccezione viene generata se il metodo viene chiamato durante un aggiornamento e `Department` entità trovare corrispondenze il `Department` entità da aggiornare.

Il nuovo metodo da chiamare il `Insert` e `Update` metodi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

In *ISchoolRepository.cs*, aggiungere la seguente dichiarazione per il nuovo metodo di accesso ai dati:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

In *SchoolRepository.cs*, aggiungere il seguente `using` istruzione:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

In *SchoolRepository.cs*, aggiungere il nuovo metodo di accesso ai dati seguenti:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Questo codice viene recuperato `Department` le entità che dispongono di un amministratore specificato. (Se presente) deve trovare un solo reparto. Tuttavia, poiché nessun vincolo viene compilato nel database, il tipo restituito è una raccolta nel caso in cui vengono trovati più reparti.

Per impostazione predefinita, quando il contesto dell'oggetto consente di recuperare entità dal database, tiene traccia di essi nella gestione dello stato degli oggetti. Il `MergeOption.NoTracking` parametro specifica che questo rilevamento non verrà eseguito per questa query. Ciò è necessario perché la query potrebbe restituire l'entità esatto che si sta tentando di aggiornare e quindi non sarebbe in grado di connettersi a tale entità. Ad esempio, se si modifica il reparto di cronologia di *Departments.aspx* pagina e lasciare invariato l'amministratore, questa query restituirà il reparto di cronologia. Se `NoTracking` non è impostata, il contesto dell'oggetto già avrebbe all'entità reparto cronologia nella console di gestione dello stato relativo oggetto. Quindi quando si collega all'entità reparto cronologia che viene ricreato dallo stato di visualizzazione, genera un'eccezione che indica il contesto dell'oggetto `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Come un'alternativa alla specifica `MergeOption.NoTracking`, è possibile creare un nuovo contesto di oggetto per questa query. Perché il nuovo contesto dell'oggetto ha un proprio gestore degli stati di oggetto, non vi sarà alcun conflitto quando si chiama il `Attach` metodo. Il nuovo contesto dell'oggetto condividerebbero connessione di database e i metadati con il contesto di oggetto originale, quindi la riduzione delle prestazioni di questo approccio alternativo devono essere minimo. L'approccio illustrato di seguito, tuttavia, introduce la `NoTracking` opzione sono disponibili utili in altri contesti. Il `NoTracking` verrà discusso più avanti in un'esercitazione successiva di questa serie.)

Nel progetto di test, aggiungere il nuovo metodo di accesso ai dati per *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Questo codice Usa LINQ per eseguire la stessa selezione di dati che il `ContosoUniversity` repository progetto usa LINQ to Entities per.

Eseguire nuovamente il progetto di test. Questa volta il test ha esito positivo.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Gestione delle eccezioni ObjectDataSource

Nel `ContosoUniversity` progetto, eseguire il *Departments.aspx* pagina e provare a modificare l'amministratore di un reparto a un utente è già amministratore per un altro reparto. (Tenere presente che è possibile modificare solo i reparti aggiunto durante questa esercitazione, poiché il database proviene precaricato con dati non validi). Ottenere la pagina di errore server seguenti:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Non si desidera che gli utenti visualizzino questo tipo di pagina di errore, pertanto è necessario aggiungere il codice di gestione degli errori. Aprire *Departments.aspx* e specificare un gestore per il `OnUpdated` evento del `DepartmentsObjectDataSource`. Il `ObjectDataSource` tag di apertura ora simile all'esempio seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

In *Departments.aspx.cs*, aggiungere il seguente `using` istruzione:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Aggiungere il seguente gestore per il `Updated` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Se il `ObjectDataSource` controllo rileva un'eccezione quando tenta di eseguire l'aggiornamento, passa l'eccezione nell'argomento dell'evento (`e`) a questo gestore. Il codice nel gestore di verifica per verificare se l'eccezione è l'eccezione amministratore duplicati. Questo caso, il codice crea un controllo di convalida che contiene un messaggio di errore per il `ValidationSummary` controllo da visualizzare.

Eseguire la pagina e tenta di rendere un utente amministratore due reparti nuovamente. Questa volta il `ValidationSummary` controllo Visualizza un messaggio di errore.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Apportare modifiche simili per il *DepartmentsAdd.aspx* pagina. In *DepartmentsAdd.aspx*, specificare un gestore per il `OnInserted` evento del `DepartmentsObjectDataSource`. Il tag risulta sarà simile all'esempio seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

In *DepartmentsAdd.aspx.cs*, aggiungere la stessa `using` istruzione:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Aggiungere il seguente gestore eventi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

È ora possibile testare il *DepartmentsAdd.aspx.cs* pagina per verificare che gestisce anche correttamente tenta di rendere l'amministratore del più reparto di una persona.

Questo completa l'introduzione di implementazione del modello di repository per l'utilizzo di `ObjectDataSource` controllo con Entity Framework. Per ulteriori informazioni sul modello di repository e testabilità, vedere il white paper MSDN [testabilità ed Entity Framework 4.0](https://msdn.microsoft.com/en-us/library/ff714955.aspx).

Nell'esercitazione seguente si noterà come aggiungere l'ordinamento e filtro funzionalità all'applicazione.

>[!div class="step-by-step"]
[Precedente](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
[Successivo](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
