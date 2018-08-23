---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: 'Iterazione #6-usare lo sviluppo basato su test (c#) | Microsoft Docs'
author: microsoft
description: In questa iterazione sesta, viene aggiunto nuove funzionalità all'applicazione scrivendo unit test prima e scrivere codice sull'unit test. In questa iterazione,...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c4358a1b979ab95d8ac25551e21ee95d75e5eae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831348"
---
<a name="iteration-6--use-test-driven-development-c"></a>Iterazione #6-usare lo sviluppo basato su test (c#)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> In questa iterazione sesta, viene aggiunto nuove funzionalità all'applicazione scrivendo unit test prima e scrivere codice sull'unit test. In questa iterazione, viene aggiunto gruppi di contatti.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creazione di un'applicazione MVC ASP.NET di gestione dei contatti (c#)
  

In questa serie di esercitazioni, creiamo un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica - per un elenco di persone.

Si compilerà l'applicazione a varie iterazioni. A ogni iterazione, abbiamo migliorare gradualmente l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1 - creare l'applicazione. Nella prima iterazione, verranno create Contact Manager nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base dei database: crea, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2 - migliorare l'applicazione aspetto interessante. In questa iterazione è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master visualizzazione MVC ASP.NET e foglio di stile CSS.

- Iterazione #3 - aggiungere la convalida dei form. Nella terza iterazione, aggiungiamo la convalida dei form di base. Persone è impedire l'invio di un modulo senza completare i campi del modulo richiesto. È anche convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4 - rendere l'applicazione a regime. In questo quarta iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice da gestire e modificare l'applicazione Contact Manager. Ad esempio, è eseguire il refactoring l'applicazione per usare il modello di Repository e il modello di inserimento delle dipendenze.

- Iterazione #5 - creare unit test. Nell'iterazione del quinto, si semplifica l'applicazione di manutenzione e la modifica mediante l'aggiunta di unit test. È simulare le classi di modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione #6 - usare lo sviluppo basato su test. In questa iterazione sesta, viene aggiunto nuove funzionalità all'applicazione scrivendo unit test prima e scrivere codice sull'unit test. In questa iterazione, viene aggiunto gruppi di contatti.

- Iterazione #7 - aggiungere funzionalità Ajax. Nell'iterazione del settimo, aggiungendo il supporto per Ajax è migliorare la velocità di risposta e le prestazioni dell'applicazione.

## <a name="this-iteration"></a>Questa iterazione

Nell'iterazione precedente dell'applicazione Contact Manager, è stato creato gli unit test per specificare una rete di protezione per il nostro codice. La motivazione per la creazione di unit test consisteva nel rendere più resilienti per modificare il codice. Con gli unit test posto, possiamo Fortunatamente apportare qualsiasi modifica al codice e sapere immediatamente se abbiamo interrotto le funzionalità esistenti.

In questa iterazione, usiamo gli unit test per uno scopo completamente diverso. In questa iterazione, usiamo gli unit test come parte di una filosofia di progettazione dell'applicazione denominata *lo sviluppo basato su test*. Quando si pratica lo sviluppo basato su test, si scrivono innanzitutto i test e quindi scrivere codice per i test.

Più precisamente, quando lo sviluppo basato su test mettendo in pratica, esistono tre passaggi completate durante la creazione di codice (Red / Green/refactoring):

1. Scrivere uno unit test che si verifica un errore (rosso)
2. Scrivere codice che supera il test di unità (verde)
3. Il refactoring del codice (refactoring)

Prima di tutto è scrivere lo unit test. Lo unit test debba esprimere l'intenzione per la modalità con cui si prevede che il codice di comportamento. Quando si crea lo unit test, lo unit test deve avere esito negativo. Il test deve avere esito negativo perché non è stato ancora scritto codice che soddisfa il test dell'applicazione.

Successivamente, scrivere codice sufficiente affinché lo unit test da passare. L'obiettivo consiste nello scrivere il codice nel modo laziest, sloppiest e più veloci possibile. È non deve sprecare tempo a pensare l'architettura dell'applicazione. In alternativa, è consigliabile concentrarsi sulla scrittura di una quantità minima di codice necessario per soddisfare lo scopo espresso dallo unit test.

Infine, dopo avere scritto codice sufficiente, è possibile passo indietro e considerare l'architettura complessiva dell'applicazione. In questo passaggio si riscrive (refactoring) il codice, sfruttando i vantaggi di un progetto software pattern:, ad esempio il modello di repository, in modo che quest'ultima è più facile da gestire. È possibile riscrivere il codice in questo passaggio tutta sicurezza perché il codice è coperto da unit test.

Esistono diversi vantaggi che derivano da mettendo in pratica lo sviluppo basato su test. In primo luogo, test-driven development impone all'utente di concentrarsi sul codice che effettivamente deve essere scritto. Poiché costantemente riguardano solo la scrittura di codice sufficiente per il passaggio di un determinato test, viene impedito di supporto le erbacce e scrivere grandi quantità di codice che non viene mai utilizzato.

In secondo luogo, una metodologia di progettazione "prima di tutto testare" forza la scrittura di codice dal punto di vista della modalità di utilizzo del codice. In altre parole, quando mettendo in pratica lo sviluppo basato su test, si siano scrivendo continuamente i test dalla prospettiva dell'utente. Pertanto, lo sviluppo basato su test può comportare le API più pulite e più comprensibile.

Infine, lo sviluppo basato su test forza la scrittura di unit test come parte del normale processo di creazione di un'applicazione. Quando si avvicina la scadenza di un progetto, il test è in genere la prima cosa che si estende la finestra. Durante lo sviluppo basato su test, mettendo in pratica d'altra parte, si hanno più probabilità di essere davvero virtuoso sulla scrittura di unit test in quanto lo sviluppo basato su test rende centrale unit test per il processo di compilazione di un'applicazione.

> [!NOTE] 
> 
> Per altre informazioni sullo sviluppo basato su test, consiglia di leggere il libro di Michael Feathers **Working Effectively with Legacy Code**.


In questa iterazione, aggiungiamo una nuova funzionalità all'applicazione Contact Manager. Viene aggiunto il supporto per i gruppi di contatto. È possibile usare gruppi di contatto per organizzare i contatti in categorie, ad esempio aziendale e Friend.

Si aggiungerà questa nuova funzionalità all'applicazione seguendo un processo di sviluppo basato su test. Unit test verranno scritti prima di tutto e verrà scritto tutto il codice per questi test.

## <a name="what-gets-tested"></a>Che cosa verrà sottoposta a test

Come accennato nell'iterazione precedente, in genere non si scrivono unit test per la logica di accesso ai dati o visualizzare per la logica. Non si t scrivere unit test per la logica di accesso ai dati perché l'accesso a un database è un'operazione relativamente lenta. Poiché l'accesso a una vista richiede la rotazione di un server web che è un'operazione relativamente lenta non si t scrivere unit test per la logica di visualizzazione. Si t non deve scrivere uno unit test, a meno che il test può essere eseguito ripetutamente molto veloce

Poiché è guidato lo sviluppo basato su test dagli unit test, viene analizzata inizialmente la scrittura di controller e logica di business. Microsoft non toccare il database o le viste. Abbiamo vinto t modificare il database oppure creare viste fino alla fine di questa esercitazione. Si inizia con ciò che può essere testato.

## <a name="creating-user-stories"></a>Creazione di storie utente

Quando mettendo in pratica lo sviluppo basato su test, sempre innanzitutto scrivere un test. Questo genera subito la domanda: come si decide quale scrivere prima del test? Per rispondere a questa domanda, è consigliabile scrivere un set di [ **storie utente**](http://en.wikipedia.org/wiki/User_stories).

Una storia utente è una breve descrizione (in genere una frase) di un requisito software. Deve essere una descrizione di un requisito scritta dalla prospettiva dell'utente non tecnica.

Ecco il set di storie utente che descrivono le funzionalità necessarie per la nuova funzionalità gruppo di contatti s:

1. Utente può visualizzare un elenco di gruppi di contatti.
2. Utente può creare un nuovo gruppo di contatto.
3. Utente può eliminare un gruppo di contatto esistente.
4. Utente può selezionare un gruppo di contatti quando si crea un nuovo contatto.
5. Utente può selezionare un gruppo di contatti quando si modifica un contatto esistente.
6. Un elenco di gruppi di contatto viene visualizzato nella vista Index.
7. Quando un utente fa clic su un gruppo di contatto, viene visualizzato un elenco di contatti.

Si noti che questo elenco di storie utente è totalmente comprensibile da un cliente. Non include informazioni sui dettagli di implementazione tecnica.

Durante il processo di compilazione dell'applicazione, il set di storie utente può diventare più avanzato. È possibile interrompere una storia utente in storie più (requisiti). Ad esempio, si potrebbe decidere che crea un nuovo gruppo di contatto implica la convalida. Invio di un gruppo senza nome contatto deve restituire un errore di convalida.

Dopo aver creato un elenco di storie utente, si è pronti a scrivere il primo unit test. Si inizierà creando uno unit test per la visualizzazione dell'elenco dei gruppi di contatti.

## <a name="listing-contact-groups"></a>Elencare i gruppi di contatto

La nostra storia utente prima è che un utente deve essere in grado di visualizzare un elenco di gruppi di contatti. È necessario esprimere la storia con un test.

Creare un nuovo unit test facendo clic su cartella controller nel progetto ContactManager.Tests selezionando **Aggiungi, nuovo Test**e selezionando le **Unit Test** modello (vedere la figura 1). Nome della nuova unità GroupControllerTest.cs di test, scegliere il **OK** pulsante.


[![Aggiunta di GroupControllerTest unit test](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**Figura 01**: aggiunta di unit test GroupControllerTest ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-6-use-test-driven-development-cs/_static/image2.png))


Il primo unit test è contenuta nel listato 1. Questo test verifica che il metodo Index () del controller gruppo restituisce un set di gruppi. Il test verifica che una raccolta di gruppi viene restituita nella visualizzazione dei dati.

**Listato 1 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

Quando si digita prima di tutto il codice nel listato 1, in Visual Studio, si otterrà una grande quantità di righe ondulate rosse. Non sono state create le classi GroupController o un gruppo.

A questo punto, siamo in grado di compilazione anche t nostro primo unit test di eseguire l'applicazione in modo che possiamo t. Che s valido. Che viene viene conteggiata come un test non superato. Pertanto, è ora disponibile l'autorizzazione per iniziare a scrivere il codice dell'applicazione. È necessario scrivere codice sufficiente per eseguire il testing.

La classe controller del gruppo nel listato 2 contiene il livello minimo di codice necessario per passare lo unit test. L'azione Index () restituisce un elenco in modo statico codificato di gruppi (la classe del gruppo è definita nel listato 3).

**Listato 2 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**Listato 3 - Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

Dopo che le classi GroupController e gruppo per il progetto, il primo unit test viene completato correttamente (vedere la figura 2). È stato eseguito il lavoro minimo richiesto per superare il test. È ora festeggiare.


[![Vero successo!](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**Figura 02**: operazione riuscita! ( [Fare clic per visualizzare l'immagine con dimensioni normali](iteration-6-use-test-driven-development-cs/_static/image4.png))


## <a name="creating-contact-groups"></a>Creazione di gruppi di contatto

A questo punto sarà possibile procedere alla storia utente secondo. È necessario essere in grado di creare nuovi gruppi di contatto. È necessario esprimere l'intenzione con un test.

Il test nel listato 4 verifica che la chiamata di metodo metodo con un nuovo gruppo aggiunge il gruppo all'elenco di gruppi restituiti dal metodo Index (). In altre parole, se si crea un nuovo gruppo quindi è necessario essere in grado di ottenere nuovamente il nuovo gruppo di nell'elenco di gruppi restituiti dal metodo Index ().

**Listato 4 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

Il test nel listato 4 chiama il metodo Create () con un nuovo contatto gruppo di controller di gruppo. Successivamente, il test verifica che la chiamata a controller di gruppo metodo Index () restituisce il nuovo gruppo nei dati di visualizzazione.

Il controller di gruppo modificato nel listato 5 contiene il livello minimo di modifiche necessarie per passare il nuovo test.

**Listato 5 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Il controller di gruppo nel listato 5 è una nuova azione create (). Questa azione aggiunge un gruppo a una raccolta di gruppi. Si noti che l'azione Index () è stato modificato per restituire il contenuto della raccolta di gruppi.

Ancora una volta, sono stati eseguiti la quantità minima di lavoro richiesta per passare lo unit test. Dopo che si apportano queste modifiche al controller di gruppo, vengono superati tutti i nostri unit test.

## <a name="adding-validation"></a>Aggiunta della convalida

Questo requisito non è stato dichiarato in modo esplicito nella storia utente. Tuttavia, è ragionevole richiedere che un gruppo di avere un nome. In caso contrario, l'organizzazione di contatti in gruppi non sarebbe molto utile.

Listato 6 contiene un nuovo test che esprime questo obiettivo. Questo test verifica che il tentativo di creare un gruppo senza fornire un nome viene importata in un messaggio di errore di convalida nello stato del modello.

**Listato 6 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

Per soddisfare questo test, che è necessario aggiungere una proprietà del nome per la classe del gruppo (vedere listato 7). Inoltre, è necessario aggiungere un po' di logica di convalida al controller gruppo s azione create () (vedere listato 8).

**Listato 7 - Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**Listato 8 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

Si noti che il controller di gruppo di azione create () ora contiene sia la convalida del database per la logica. Attualmente, il database utilizzato dal controller di gruppo è costituito nient'altro che una raccolta in memoria.

## <a name="time-to-refactor"></a>Ora effettuare il refactoring

Il terzo passaggio Red/Green/eseguire il refactoring è la parte di refactoring. A questo punto, è necessario eseguire il failback da codice e tenere in considerazione come è possibile effettuare il refactoring per migliorare la progettazione applicazione. La fase di effettuare il refactoring è la fase in cui si pensa rigidi sul modo migliore di implementazione di modelli e i principi di progettazione software.

Microsoft è possibile modificare il codice in alcun modo che si sceglie di migliorare la progettazione del codice. È disponibile una rete di protezione di unit test che impediscono di invalidare la funzionalità esistente.

Al momento, il controller di gruppo è complicato dal punto di vista della progettazione software di qualità. Il controller di gruppo contiene radici lontane seri problemi di convalida e codice di accesso ai dati. Per evitare che violano il principio di singola responsabilità, è necessario separare questi problemi in diverse classi.

La classe controller gruppo sottoposta a refactoring è contenuta nel listato 9. Il controller è stato modificato per usare il livello di servizio ContactManager. Questo è lo stesso livello di servizio che viene usato con il controller di contatto.

Listato 10 contiene i nuovi metodi aggiunti al livello di servizio ContactManager per supportare la convalida, elenco e creazione di gruppi. L'interfaccia IContactManagerService è stato aggiornato per includere i nuovi metodi.

Listato 11 contiene una nuova classe FakeContactManagerRepository che implementa l'interfaccia IContactManagerRepository. A differenza della classe EntityContactManagerRepository che implementa anche l'interfaccia IContactManagerRepository, la nuova classe FakeContactManagerRepository non comunica con il database. La classe FakeContactManagerRepository Usa una raccolta in memoria come proxy per il database. Si userà questa classe nei nostri test unità come un livello di repository fittizio.

**Listato 9 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**Listato 10 - Controllers\ContactManagerService.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**Listato 11 - Controllers\FakeContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

Modifica il IContactManagerRepository richiede l'interfaccia consentono di implementare i metodi CreateGroup() e ListGroups() nella classe EntityContactManagerRepository. Il modo più rapido e laziest per eseguire questa operazione consiste nell'aggiungere i metodi stub con un aspetto simile al seguente:   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]


Infine, queste modifiche alla progettazione della nostra applicazione rendono necessario apportare alcune modifiche al nostro unit test. È ora necessario usare il FakeContactManagerRepository durante l'esecuzione di unit test. La classe GroupControllerTest aggiornata è contenuta nel listato 12.

**Listato 12 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

Dopo aver tutte queste modifiche, ancora una volta, tutti i nostri unit test superati. È stata completata l'intero ciclo di colore rosso/verde/eseguire il refactoring. Sono state implementate le storie due utente prima. È ora disponibile il supporto di unit test per i requisiti espressi nelle storie utente. Implementare il resto delle storie utente consiste nel ripetere lo stesso ciclo di colore rosso/verde/eseguire il refactoring.

## <a name="modifying-our-database"></a>Modifica il Database

Purtroppo, anche se è stata soddisfatta tutti i requisiti espressi l'unit test, il lavoro non viene eseguito. È comunque necessario modificare il database.

È necessario creare una nuova tabella di database di gruppo. Attenersi ai passaggi riportati di seguito.

1. Nella finestra Esplora Server, fare doppio clic sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**.
2. Immettere le due colonne descritte di seguito in Progettazione tabelle.
3. Contrassegnare la colonna Id come una chiave primaria e una colonna Identity.
4. Salvare la nuova tabella con i gruppi di nome facendo clic sull'icona di unità disco floppy.

<a id="0.11_table01"></a>


| **Nome della colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | int | False |
| nome | nvarchar (50) | False |


Successivamente, è necessario eliminare tutti i dati dalla tabella di contatti (in caso contrario, abbiamo vinto t in grado di creare una relazione tra le tabelle di contatti e gruppi). Attenersi ai passaggi riportati di seguito.

1. La tabella Contacts e scegliere l'opzione di menu **Mostra dati tabella**.
2. Eliminare tutte le righe.

Successivamente, è necessario definire una relazione tra la tabella di database di gruppi e la tabella di database dei contatti esistente. Attenersi ai passaggi riportati di seguito.

1. Fare doppio clic nella tabella di contatti nella finestra Esplora Server per aprire Progettazione tabelle.
2. Aggiungere una nuova colonna di tipo integer per la tabella di contatti denominata GroupId.
3. Fare clic sul pulsante Relationship per aprire la finestra di dialogo Relazioni chiavi esterne (vedere la figura 3).
4. Fare clic sul pulsante Aggiungi.
5. Fare clic sul pulsante con puntini di sospensione visualizzato accanto al pulsante che specifica le colonne e tabelle.
6. Nella finestra di dialogo tabelle e colonne, selezionare i gruppi come la tabella di chiave primaria e l'Id come colonna chiave primaria. Selezione contatti la tabella di chiave esterna e GroupId come colonna chiave esterna (vedere la figura 4). Fare clic sul pulsante OK.
7. Sotto **specifica INSERT e UPDATE**, selezionare il valore **Cascade** per **Elimina regola**.
8. Fare clic sul pulsante chiude per chiudere la finestra di dialogo Relazioni chiavi esterne.
9. Fare clic sul pulsante Salva per salvare le modifiche apportate alla tabella Contacts.


[![Creazione di una relazione tra tabelle di database](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**Figura 03**: la creazione di una relazione tra tabelle di database ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-6-use-test-driven-development-cs/_static/image6.png))


[![Specifica di relazioni tra tabelle](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**Figura 04**: specifica di relazioni tra tabelle ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-6-use-test-driven-development-cs/_static/image8.png))


### <a name="updating-our-data-model"></a>Aggiornare il modello di dati

Successivamente, è necessario aggiornare il modello di dati per rappresentare la nuova tabella di database. Attenersi ai passaggi riportati di seguito.

1. Fare doppio clic il file ContactManagerModel.edmx nella cartella modelli per aprire la finestra di progettazione entità.
2. Fare doppio clic nell'area di progettazione e selezionare l'opzione di menu **Aggiorna modello da Database**.
3. Nell'aggiornamento guidato, selezionare i gruppi di tabella e scegliere la data di fine sul pulsante (vedere la figura 5).
4. L'entità di gruppi e scegliere l'opzione di menu **Rinomina**. Modificare il nome del *gruppi* entità *gruppo* (singolare).
5. Fare doppio clic su proprietà di navigazione i gruppi che viene visualizzato nella parte inferiore dell'entità Contact. Modificare il nome del *gruppi* proprietà di navigazione *gruppo* (singolare).


[![Aggiornamento di un modello di Entity Framework dal database](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**Figura 05**: aggiornamento di un modello di Entity Framework dal database ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-6-use-test-driven-development-cs/_static/image10.png))


Dopo aver completato questi passaggi, il modello di dati rappresenta sia i contatti e gruppi di tabelle. La finestra di progettazione entità deve visualizzare entrambe le entità (vedere la figura 6).


[![Finestra di progettazione entità visualizzando Group e Contact](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**Figura 06**: finestra di progettazione entità visualizzando Group e Contact ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-6-use-test-driven-development-cs/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Creare le classi di Repository

Successivamente, è necessario implementare la classe di repository. Nel corso di questa iterazione, abbiamo aggiunto diversi nuovi metodi per l'interfaccia IContactManagerRepository durante la scrittura di codice per soddisfare i nostri unit test. La versione finale dell'interfaccia IContactManagerRepository è contenuta nel listato 14.

**Listato 14 - Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

È ora implementato uno dei metodi correlati all'utilizzo dei gruppi di contatti haven t. Attualmente, la classe EntityContactManagerRepository dispone di metodi stub per ognuno dei metodi di contatto gruppo elencati nell'interfaccia IContactManagerRepository. Ad esempio, il metodo ListGroups() attualmente aspetto simile al seguente:

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

I metodi stub ha permesso di compilare l'applicazione e passare gli unit test. Tuttavia, ora è possibile implementare effettivamente questi metodi. La versione finale della classe EntityContactManagerRepository è contenuta nel listato 13.

**Listato 13 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>Creazione di viste

Applicazione MVC ASP.NET quando si usa il motore di visualizzazione ASP.NET predefinito. Pertanto, non si t creare viste in risposta a uno specifico unit test. Tuttavia, poiché un'applicazione sarebbe inutile senza le visualizzazioni, è possibile t completare questa iterazione senza creare e modificare le visualizzazioni contenute nell'applicazione Contact Manager.

È necessario creare le seguenti nuove viste per la gestione dei gruppi di contatto (vedere la figura 7):

- Views\Group\Index.aspx - Visualizza l'elenco di gruppi di contatto
- Views\Group\Delete.aspx - modulo di conferma viene visualizzato per l'eliminazione di un gruppo di contatti


[![La visualizzazione dell'indice di gruppo](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**Figura 07**: Visualizza l'indice di gruppo ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-6-use-test-driven-development-cs/_static/image14.png))


È necessario modificare le viste esistenti seguenti in modo che includano i gruppi di contatto:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

È possibile visualizzare le visualizzazioni modificate esaminando l'applicazione di Visual Studio che accompagna questa esercitazione. Ad esempio, la figura 8 mostra la visualizzazione Index dei contatti.


[![La visualizzazione Index dei contatti](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**Figura 08**: Visualizza l'indice di contatto ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-6-use-test-driven-development-cs/_static/image16.png))


## <a name="summary"></a>Riepilogo

Questa iterazione, abbiamo aggiunto nuove funzionalità all'applicazione Contact Manager seguendo una metodologia di progettazione dell'applicazione lo sviluppo basato su test. Abbiamo iniziato creando un set di storie utente. È stato creato un set di unit test che corrisponde ai requisiti espressi per le storie utente. Infine, abbiamo scritto codice sufficiente a soddisfare i requisiti espressi dagli unit test.

Dopo che abbiamo completato la scrittura di codice sufficiente per soddisfare i requisiti stabiliti per gli unit test, abbiamo aggiornato il database e le viste. Abbiamo aggiunto una nuova tabella di gruppi per il database e aggiornato il modello di dati di Entity Framework. È anche creare e modificare un set di viste.

Nell'iterazione successiva, l'iterazione finale - si riscrive l'applicazione possa sfruttare i vantaggi di Ajax. Grazie all'uso di Ajax, si sarà di migliorare la velocità di risposta e le prestazioni dell'applicazione Contact Manager.

> [!div class="step-by-step"]
> [Precedente](iteration-5-create-unit-tests-cs.md)
> [Successivo](iteration-7-add-ajax-functionality-cs.md)
