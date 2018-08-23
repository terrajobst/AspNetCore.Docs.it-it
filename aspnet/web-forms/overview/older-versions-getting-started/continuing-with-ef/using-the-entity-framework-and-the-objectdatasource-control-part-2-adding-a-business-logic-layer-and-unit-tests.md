---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Utilizzo di Entity Framework 4.0 e controllo ObjectDataSource, parte 2: aggiunta di un livello di logica di Business e Unit test | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione web di Contoso University specificano che viene creato da Getting Started with serie di esercitazioni in Entity Framework 4.0. POSSO...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 6517f037a03bb520ee4f3b3185a255b0eaa9de9a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834788"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Utilizzo di Entity Framework 4.0 e controllo ObjectDataSource, parte 2: aggiunta di un livello di logica di Business e Unit test
====================
da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web Contoso University specificano che viene creato per il [Introduzione a Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni. Se si non è stato completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) verrebbe creato. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creato dalla serie di esercitazioni complete. Se si hanno domande sulle esercitazioni, è possibile pubblicarli per i [forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Nell'esercitazione precedente è stata creata un'applicazione web a più livelli con Entity Framework e `ObjectDataSource` controllo. Questa esercitazione illustra come aggiungere la logica di business, mantenendo separate il livello di logica di business (BLL) e il livello di accesso ai dati (DAL) e viene illustrato come creare unit test automatizzati per il livello BLL.

In questa esercitazione si completeranno le attività seguenti:

- Creare un'interfaccia del repository che dichiara i metodi di accesso ai dati che necessari.
- Implementare l'interfaccia del repository nella classe di repository.
- Creare una classe di logica di business che chiama la classe di repository per eseguire le funzioni di accesso ai dati.
- Connettere il `ObjectDataSource` controllo alla classe della logica di business invece che per la classe di repository.
- Creare un progetto di unit test e una classe di repository che usa raccolte in memoria per il proprio archivio dati.
- Creare uno unit test per la logica di business che si desidera aggiungere alla classe di logica di business, quindi eseguire il test e vederlo esito negativo.
- Implementare la logica di business nella classe della logica di business, quindi eseguire nuovamente l'unit test e assicurarsi che vengano superati.

Verrà usata la *Departments.aspx* e *DepartmentsAdd.aspx* pagine creato nell'esercitazione precedente.

## <a name="creating-a-repository-interface"></a>Creazione di un'interfaccia del Repository

È possibile iniziare creando l'interfaccia del repository.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

Nel *DAL* cartella, creare un nuovo file di classe, denominarlo *ISchoolRepository.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

L'interfaccia definisce un metodo per ogni di CRUD (create, leggere, aggiornare ed eliminare) i metodi che è stato creato nella classe di repository.

Nel `SchoolRepository` classe *SchoolRepository.cs*, indicare che questa classe implementa il `ISchoolRepository` interfaccia:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Creazione di una classe di logica di Business

Successivamente, si creerà la classe di logica di business. Si esegue questa operazione in modo che sia possibile aggiungere logica di business che verrà eseguita dal `ObjectDataSource` controllare, anche se non si effettuerà che ancora. Per il momento, solo la nuova classe di logica di business eseguirà le operazioni CRUD che esegue il repository.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Creare una nuova cartella e denominarla *BLL*. (In un'applicazione reale, il livello di logica di business in genere implementato come una libreria di classi, ovvero un progetto separato, ma per rendere semplice l'esercitazione, le classi BLL verranno mantenute in una cartella di progetto.)

Nel *BLL* cartella, creare un nuovo file di classe, denominarlo *SchoolBL.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Questo codice crea gli stessi metodi CRUD che si è visto in precedenza nella classe di repository, ma invece di accedere direttamente i metodi di Entity Framework, chiama il repository di metodi della classe.

La variabile di classe che contiene un riferimento alla classe di repository è definita come un tipo di interfaccia e il codice che crea un'istanza della classe di repository è contenuto in due costruttori. Il costruttore senza parametri da utilizzare per il `ObjectDataSource` controllo. Crea un'istanza del `SchoolRepository` classe creata in precedenza. L'altro costruttore consente a qualsiasi riga di codice che crea un'istanza della classe di logica di business da passare in qualsiasi oggetto che implementa l'interfaccia del repository.

I metodi CRUD di chiamare la classe di repository e i due costruttori consentono di usare la classe di logica di business con qualsiasi archivio dati back-end scelto. La classe di logica di business non è necessario essere a conoscenza del modo in cui la classe che esegue chiamate rende persistenti i dati. (Ciò viene spesso definito *mancato riconoscimento della persistenza*.) Ciò semplifica gli unit test, perché la classe di logica di business è possibile connettersi a un'implementazione di repository che usa un elemento come semplice come in memoria `List` raccolte per archiviare i dati.

> [!NOTE]
> Tecnicamente, gli oggetti entità sono comunque non riconoscono la persistenza, perché ne è creata l'istanza da classi che ereditano da di Entity Framework `EntityObject` classe. Mancato riconoscimento della persistenza completa, è possibile utilizzare *plain old CLR Object*, o *poco*, al posto degli oggetti da cui ereditare il `EntityObject` classe. Uso poco esula dall'ambito di questa esercitazione. Per altre informazioni, vedere [testabilità ed Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) sul sito Web MSDN.)


A questo punto è possibile connettere il `ObjectDataSource` controlli per la logica di business classe anziché al repository e verificare che tutto funzioni come in precedenza.

Nelle *Departments.aspx* e *DepartmentsAdd.aspx*, modificare tutte le occorrenze dei `TypeName="ContosoUniversity.DAL.SchoolRepository"` a `TypeName="ContosoUniversity.BLL.SchoolBL`". (Sono presenti quattro istanze in tutto.)

Eseguire la *Departments.aspx* e *DepartmentsAdd.aspx* pagine per verificare che funzionino ancora come in precedenza.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Creazione di un progetto di Unit Test e implementazione di Repository

Aggiungere un nuovo progetto alla soluzione utilizzando il **progetto di Test** modello e denominarla `ContosoUniversity.Tests`.

Nel progetto di test, aggiungere un riferimento a `System.Data.Entity` e aggiungere un riferimento al progetto il `ContosoUniversity` progetto.

È ora possibile creare la classe di repository che si userà con gli unit test. L'archivio dati per questo repository sarà all'interno della classe.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

Nel progetto di test, creare un nuovo file di classe, denominarlo *MockSchoolRepository.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Questa classe di repository ha gli stessi metodi CRUD cui si accede direttamente a Entity Framework, ma con cui lavorano `List` raccolte in memoria anziché con un database. Questo rende più semplice per una classe di test configurare e convalidare gli unit test per la classe di logica di business.

## <a name="creating-unit-tests"></a>Creazione di Unit test

Il **testare** modello di progetto creato una classe stub di unit test e l'attività successiva consiste nella modifica di questa classe mediante l'aggiunta metodi di unit test per la logica di business che si desidera aggiungere alla classe della logica di business.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Presso Contoso University, qualsiasi instructor singola può essere solo l'amministratore di un singolo reparto ed è necessario aggiungere logica di business per applicare questa regola. Inizierà aggiungendo i test e l'esecuzione dei test per visualizzarli esito negativo. Quindi aggiungere il codice e rieseguire i test, è previsto per visualizzarli passare.

Aprire il *UnitTest1.cs* file e aggiungere `using` istruzioni per i livelli di accesso ai dati e logica di business che è stato creato nel progetto ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Sostituire il `TestMethod1` metodo con i metodi seguenti:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

Il `CreateSchoolBL` metodo crea un'istanza della classe di repository creato per il progetto, che viene quindi passato a una nuova istanza della classe di logica di business unit test. Il metodo utilizza quindi la classe di logica di business per inserire tre i reparti che è possibile usare nei metodi di test.

I metodi di test verificare che la classe di logica di business genera un'eccezione se un utente tenta di inserire un nuovo reparto con lo stesso amministratore come un dipartimento esistente o se qualcuno prova ad aggiornare l'amministratore del reparto mediante l'impostazione per l'ID di una persona chi è già l'amministratore di un altro reparto.

Ancora creato la classe di eccezione, pertanto questo codice non verrà compilato. Per far sì che la compilazione, fare doppio clic su `DuplicateAdministratorException` e selezionare **Generate**, quindi **classe**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Viene creata una classe nel progetto di test che è possibile eliminare dopo aver creato la classe di eccezione nel progetto principale. e implementare la logica di business.

Eseguire il progetto di test. Come previsto, i test hanno esito negativo.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Aggiunta di logica di Business in modo che passi del Test

Successivamente, si sarà implementare la logica di business che rende impossibile da impostare come l'amministratore di un reparto, un utente che è già amministratore di un altro reparto. Si verrà generata un'eccezione dal livello della logica di business e quindi intercettarla nel livello di presentazione se un utente modifica un reparto e fa clic su **Update** dopo aver selezionato un utente che ha già un amministratore. (È anche possibile rimuovere instructors (insegnanti) nell'elenco a discesa che sono già amministratori prima che si esegue il rendering della pagina, ma lo scopo qui è per lavorare con il livello di logica di business).

Iniziare creando la classe di eccezione verrà generata quando un utente prova a eseguire un insegnante l'amministratore di più di un reparto. Nel progetto principale, creare un nuovo file di classe nel *BLL* cartella, denominarla *DuplicateAdministratorException.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Ora di eliminazione temporanea *DuplicateAdministratorException.cs* file creato nel progetto di test in precedenza per poter compilare.

Nel progetto principale, aprire il *SchoolBL.cs* file e aggiungere il metodo seguente che contiene la logica di convalida. (Il codice si riferisce a un metodo che verrà creata più avanti).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

È possibile chiamare questo metodo quando sta inserendo o aggiornando `Department` entità per verificare se un altro reparto ha già lo stesso amministratore.

Il codice chiama un metodo per eseguire ricerche nel database per un `Department` entità che ha gli stessi `Administrator` valore della proprietà dell'entità viene inserita o aggiornata. Se viene trovata, il codice genera un'eccezione. Nessun controllo di convalida è necessaria se l'entità che viene inserito o aggiornato non ha alcun `Administrator` valore e non fa eccezione viene generata se il metodo viene chiamato durante un aggiornamento e il `Department` entità è stata trovata corrispondenza il `Department` entità da aggiornare.

Il nuovo metodo da chiamare il `Insert` e `Update` metodi:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

Nelle *ISchoolRepository.cs*, aggiungere la dichiarazione seguente per il nuovo metodo di accesso ai dati:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

Nelle *SchoolRepository.cs*, aggiungere il codice seguente `using` istruzione:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

Nelle *SchoolRepository.cs*, aggiungere il nuovo metodo di accesso ai dati seguenti:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Questo codice recupera `Department` entità che dispongono di un amministratore specificato. Deve trovarsi un solo reparto (se presente). Tuttavia, poiché nessun vincolo viene compilato nel database, il tipo restituito è una raccolta nel caso in cui vengono trovati più reparti.

Per impostazione predefinita, quando il contesto dell'oggetto recupera le entità dal database, tiene traccia della loro nel relativo gestore di stato di oggetto. Il `MergeOption.NoTracking` parametro specifica che questo rilevamento non verrà eseguito per questa query. Ciò è necessario perché la query potrebbe restituire l'esatta entità che si sta tentando di aggiornare e quindi non sarebbe in grado di connettersi a tale entità. Ad esempio, se si modifica il reparto di cronologia nel *Departments.aspx* pagina e lasciare invariato l'amministratore, questa query restituirà il reparto di cronologia. Se `NoTracking` non è impostato, il contesto dell'oggetto dovrebbe già disporre di entità department cronologia nel relativo gestore di stato di oggetto. Quindi quando si associa l'entità department cronologia che viene creato nuovamente dallo stato di visualizzazione, il contesto dell'oggetto genererebbe un'eccezione con la dicitura `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Come alternativa alla specifica `MergeOption.NoTracking`, è possibile creare un nuovo contesto di oggetto solo per questa query. Poiché il nuovo contesto dell'oggetto avrebbe la propria gestione dello stato degli oggetti, non vi sarà alcun conflitto quando si chiama il `Attach` (metodo). Il nuovo contesto dell'oggetto condividerebbero connessione di database e i metadati con il contesto dell'oggetto originale, in modo che la riduzione delle prestazioni di questo approccio alternativo, sarebbe minima. L'approccio illustrato in questo caso, tuttavia, introduce il `NoTracking` opzione, che sono disponibili utili in altri contesti. Il `NoTracking` verrà discusso più in un'esercitazione successiva di questa serie.)

Nel progetto di test, aggiungere il nuovo metodo di accesso ai dati per *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Questo codice Usa LINQ per eseguire la stessa selezione di dati che il `ContosoUniversity` repository del progetto usa LINQ to Entities per.

Eseguire nuovamente il progetto di test. Questa volta il test ha esito positivo.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Gestione delle eccezioni di ObjectDataSource

Nel `ContosoUniversity` progetto, eseguire il *Departments.aspx* pagina e provare a modificare l'amministratore per un reparto a un utente che è già amministratore per un altro reparto. (Tenere presente che è possibile modificare solo i dipartimenti che è stato aggiunto durante questa esercitazione, perché il database viene portato precaricato con dati non validi). È visualizzata la pagina di errore server seguenti:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Preferibile che gli utenti per visualizzare questo tipo di pagina di errore, pertanto è necessario aggiungere il codice di gestione degli errori. Aprire *Departments.aspx* e specificare un gestore per il `OnUpdated` evento del `DepartmentsObjectDataSource`. Il `ObjectDataSource` ora tag di apertura è simile al seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

Nelle *Departments.aspx.cs*, aggiungere il codice seguente `using` istruzione:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Aggiungere il gestore seguente per il `Updated` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Se il `ObjectDataSource` controllo rileva un'eccezione quando tenta di eseguire l'aggiornamento, l'argomento dell'evento passa l'eccezione (`e`) a questo gestore. Il codice nel gestore controlla per verificare se l'eccezione è l'eccezione amministrazione duplicati. Se si tratta, il codice crea un controllo validator che contiene un messaggio di errore per il `ValidationSummary` controllo da visualizzare.

Eseguire la pagina e provare a effettuare nuovamente un utente amministratore di due reparti. Questa volta il `ValidationSummary` controllo Visualizza un messaggio di errore.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Apportare modifiche simili per il *DepartmentsAdd.aspx* pagina. Nelle *DepartmentsAdd.aspx*, specificare un gestore per il `OnInserted` evento del `DepartmentsObjectDataSource`. Il codice risulta sarà simile all'esempio seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

Nelle *DepartmentsAdd.aspx.cs*, aggiungere la stessa `using` istruzione:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Aggiungere il gestore eventi seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

È ora possibile testare il *DepartmentsAdd.aspx.cs* pagina per verificare che gestisca correttamente anche tentativi per rendere l'amministratore del più reparto di una persona.

In questo argomento completa l'introduzione all'implementazione il modello di repository per l'uso di `ObjectDataSource` controllo con Entity Framework. Per altre informazioni sul modello di repository e testabilità, vedere il white paper MSDN [testabilità ed Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

Nell'esercitazione verrà illustrato come aggiungere l'ordinamento e filtrare le funzionalità all'applicazione.

> [!div class="step-by-step"]
> [Precedente](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [Successivo](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
