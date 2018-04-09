---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: 'Iterazione 6 #: utilizzare sviluppo basato su test (c#) | Documenti Microsoft'
author: microsoft
description: In questa iterazione sesto è aggiungere nuove funzionalità per l'applicazione scrivendo unit test prima e la scrittura di codice per gli unit test. In questa iterazione,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: 94502625f66d3eb08a24b8f2a369bf456a3367b1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="iteration-6--use-test-driven-development-c"></a>Iterazione 6 #: utilizzare sviluppo basato su test (c#)
====================
by [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> In questa iterazione sesto è aggiungere nuove funzionalità per l'applicazione scrivendo unit test prima e la scrittura di codice per gli unit test. In questa iterazione, è aggiungere gruppi di contatti.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creazione di un'applicazione ASP.NET MVC di gestione dei contatti (c#)
  

In questa serie di esercitazioni, si compila un'intera applicazione di gestione dei contatti dall'inizio completamento. L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica: per un elenco di persone.

È compilare l'applicazione più iterazioni. A ogni iterazione, gradualmente è migliorare l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1 - creare l'applicazione. Nella prima iterazione, verranno create Contact Manager in modo più semplice possibile. Viene aggiunto il supporto per le operazioni di database basic: creazione, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2 - verificare l'applicazione l'aspetto. In questa iterazione, è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master di visualizzazione ASP.NET MVC e foglio di stile CSS.

- Iterazione #3 - aggiungere la convalida dei form. Nella terza iterazione, è aggiungere la convalida di form di base. È impedire agli utenti di inviare un modulo senza completare i campi modulo necessari. È inoltre possibile convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4: verificare l'applicazione ad accoppiamento debole. In questa terza iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice gestire e modificare l'applicazione Gestione contatti. Ad esempio, si effettua il refactoring l'applicazione di utilizzare il modello di Repository e il modello di inserimento di dipendenze.

- Iterazione #5 - creare unit test. Nella quinta iterazione, si rende l'applicazione di più facile da gestire e modificare tramite l'aggiunta di unit test. È simulare il nostro classi del modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione 6 # - utilizzare sviluppo basato su test. In questa iterazione sesto è aggiungere nuove funzionalità per l'applicazione scrivendo unit test prima e la scrittura di codice per gli unit test. In questa iterazione, è aggiungere gruppi di contatti.

- Iterazione #7 - aggiunta di funzionalità Ajax. Nella settima iterazione, è migliorare la velocità di risposta e prestazioni dell'applicazione aggiunta del supporto per Ajax.

## <a name="this-iteration"></a>Questa iterazione

Nell'iterazione precedente dell'applicazione Contact Manager, è creare unit test per fornire una rete di protezione per il nostro codice. Incluse la motivazione per la creazione di unit test è stata per rendere più flessibile per modificare il codice. Con gli unit test sul posto, è possibile tranquillamente apportare qualsiasi modifica al codice e sapere immediatamente se è stato interrotto le funzionalità esistenti.

In questa iterazione, utilizziamo unit test per uno scopo completamente diverso. In questa iterazione, utilizziamo unit test come parte della filosofia di progettazione applicazione denominata *sviluppo basato su test*. Quando lo sviluppo basato su test, pratica scrivere i test e quindi scrivere codice per i test.

Più precisamente, quando si è abituati a sviluppo basato su test, sono disponibili tre passaggi da effettuare durante la creazione di codice (rosso / verde/refactoring):

1. Scrivere uno unit test che si verifica un errore (rosso)
2. Scrivere codice che passa lo unit test (verde)
3. Il refactoring del codice (effettuare il refactoring)

In primo luogo, scrivere lo unit test. Lo unit test esprimere l'intenzione di come si prevede che il codice di comportamento. Quando si crea lo unit test, lo unit test non riesce. Il test non riesce perché non sono ancora stati scritti qualsiasi codice che soddisfa il test dell'applicazione.

Scrivere quindi solo del codice sufficiente affinché lo unit test da passare. L'obiettivo consiste nello scrivere il codice in modo più rapido possibile e sloppiest laziest. Si deve sprecare tempo a pensare l'architettura dell'applicazione. In alternativa, è consigliabile concentrarsi sulla scrittura di una quantità minima di codice necessario per soddisfare l'intenzione di esprimere tramite lo unit test.

Infine, dopo avere scritto codice sufficiente, è possibile passo indietro e considerare l'architettura complessiva dell'applicazione. In questo passaggio si riscrive (effettuare il refactoring) lo sfruttando di progettazione del software modelli di codice - ad esempio il modello di repository, in modo che il codice è più gestibile. È possibile riscrivere il codice in questo passaggio fearlessly perché il codice è coperto da unit test.

Esistono numerosi vantaggi derivanti dall'esercitazione sviluppo basato su test. In primo luogo, test-driven development obbliga a concentrarsi sul codice che effettivamente deve essere scritto. Poiché è sufficiente scrivere codice sufficiente per passare un determinato test costantemente attivo, non è consentito i malerbe di supporto e la scrittura di grandi quantità di codice che non viene mai utilizzato.

In secondo luogo, una metodologia di progettazione "prima di tutto testare" forza la scrittura di codice dal punto di vista della modalità di utilizzo del codice. In altre parole, quando si è abituati a sviluppo basato su test, si siano scrivendo continuamente i test dalla prospettiva dell'utente. Pertanto, lo sviluppo basato su test può comportare le API più semplice e più comprensibile.

Infine, lo sviluppo basato su test forza la scrittura di unit test come parte del normale processo di scrittura di un'applicazione. Quando si avvicina la scadenza di un progetto, il test è in genere la prima operazione che si estende la finestra. Quando si è abituati a sviluppo basato su test, d'altra parte, si è probabilmente virtuoso sulla scrittura di unit test semplifica lo sviluppo basato su test unit test centrale per il processo di compilazione di un'applicazione.

> [!NOTE] 
> 
> Per ulteriori informazioni sullo sviluppo basato su test, consiglia di leggere libro penne Michael **funziona in modo efficace con codice Legacy**.


In questa iterazione, è aggiungere una nuova funzionalità per l'applicazione di gestione di contatto. Viene aggiunto il supporto per i gruppi di contatto. È possibile utilizzare gruppi di contatto per organizzare i contatti in categorie, ad esempio Business e Friend.

Questa nuova funzionalità verrà aggiunto all'applicazione seguendo un processo di sviluppo basato su test. È necessario scrivere innanzitutto il nostro unit test e verrà scritto tutto il codice rispetto a questi test.

## <a name="what-gets-tested"></a>Ciò che verrà sottoposta a test

Come accennato nell'iterazione precedente, in genere non scrivere unit test per la logica di accesso ai dati o visualizzazione logica. Non è t scrivere unit test per la logica di accesso ai dati poiché l'accesso a un database è un'operazione relativamente lenta. Non è t scrivere unit test per la logica di visualizzazione perché l'accesso a una vista richiede la rotazione di un server web che è un'operazione relativamente lenta. Data t scrivere uno unit test, a meno che il test può essere eseguito ripetutamente molto veloce

Poiché lo sviluppo basato su test è definito dagli unit test, focalizzata inizialmente sulla scrittura di controller e logica di business. Sarà possibile evitare di toccare il database o viste. È stata acquisita t modificare il database o creare il nostro visualizzazioni fino alla fine di questa esercitazione. Iniziamo con ciò che può essere testato.

## <a name="creating-user-stories"></a>Creazione di storie utente

Quando si è abituati a sviluppo basato su test, viene sempre avviato mediante la scrittura di un test. Questo genera immediatamente la domanda: come si decide quali test prima di scrivere? Per rispondere a questa domanda, è necessario scrivere un set di [ **storie utente**](http://en.wikipedia.org/wiki/User_stories).

Una storia utente è una breve descrizione (in genere una frase) di un requisito software. Deve essere una descrizione di un requisito scritto da una prospettiva di utente meno esperti.

Qui s il set di storie utente che descrivono le funzionalità necessarie per la nuova funzionalità gruppo di contatto:

1. Utente può visualizzare un elenco di gruppi di contatti.
2. Utente può creare un nuovo gruppo di contatto.
3. Utente può eliminare un gruppo di contatto esistente.
4. Utente può selezionare un gruppo di contatti quando si crea un nuovo contatto.
5. Utente può selezionare un gruppo di contatti quando si modifica un contatto esistente.
6. Un elenco di gruppi di contatto viene visualizzato nella visualizzazione dell'indice.
7. Quando un utente fa clic su un gruppo di contatti, viene visualizzato un elenco di contatti corrispondenti.

Si noti che questo elenco di storie utente completamente comprensibile da un cliente. Non è menzionato dei dettagli di implementazione tecnica.

Durante il processo di compilazione dell'applicazione, il set di storie utente può diventare più precise. Potrebbe non funzionare correttamente una storia utente in più storie (requisiti). Ad esempio, è possibile che la creazione di un nuovo gruppo di contatto comportano la convalida. Invio di un gruppo senza nome contatto deve restituire un errore di convalida.

Dopo aver creato un elenco di storie utente, si è pronti per la scrittura di unit test. Si inizierà creando uno unit test per visualizzare l'elenco di gruppi di contatti.

## <a name="listing-contact-groups"></a>Elenco di gruppi di contatti

Il primo storia utente è che un utente deve essere in grado di visualizzare un elenco di gruppi di contatti. È necessario esprimere la storia con un test.

Facendo clic sulla cartella controller nel progetto ContactManager.Tests, creare un nuovo unit test selezionando **Aggiungi, nuovo Test**e selezionando il **Unit Test** modello (vedere la figura 1). Nome della nuova unità GroupControllerTest.cs di test, scegliere il **OK** pulsante.


[![Aggiunta di GroupControllerTest unit test](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**Figura 01**: aggiungere lo unit test GroupControllerTest ([fare clic per visualizzare l'immagine ingrandita](iteration-6-use-test-driven-development-cs/_static/image2.png))


Il primo unit test è contenuto in elenco 1. Questo test verifica che il metodo di Index () del controller gruppo restituisce un set di gruppi. Il test di verifica che una raccolta di gruppi viene restituita nella visualizzazione dati.

**Elenco 1 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

Quando si digita prima di tutto il codice nel listato 1 in Visual Studio, si otterrà molte righe ondulate rosse. Non sono state create le classi GroupController o un gruppo.

A questo punto, è possibile compilazione anche t il primo unit test di eseguire l'applicazione in modo è possibile t. Buona che s. Che viene considerata un test non superato. Pertanto, è ora disponibile l'autorizzazione per iniziare a scrivere il codice dell'applicazione. È necessario scrivere codice sufficiente per eseguire il testing.

La classe del gruppo controller nel listato 2 contiene il livello minimo di codice necessario per passare lo unit test. L'azione Index () restituisce un elenco in modo statico codificato di gruppi (la classe del gruppo è definita nel listato 3).

**Il listato 2 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**Elenco di 3 - Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

Dopo che le classi GroupController e di gruppo al progetto, il primo unit test viene completato correttamente (vedere la figura 2). È stato eseguito il lavoro minimo richiesto per superare il test. È ora che annuncia.


[![Operazione completata](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**Figura 02**: operazione riuscita! ( [Fare clic per visualizzare l'immagine ingrandita](iteration-6-use-test-driven-development-cs/_static/image4.png))


## <a name="creating-contact-groups"></a>Creazione di gruppi di contatti

A questo punto sarà possibile procedere alla storia utente secondo. È necessario essere in grado di creare nuovi gruppi di contatto. È necessario esprimere l'intenzione con un test.

Il test nel listato 4 verifica che la chiamata di metodo con un nuovo gruppo aggiunge il gruppo all'elenco di gruppi restituito dal metodo Index () il metodo di creazione. In altre parole, se si crea un nuovo gruppo consigliabile sarà in grado di ottenere il nuovo gruppo dall'elenco dei gruppi restituito dal metodo Index ().

**Listato 4 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

Il test nel listato 4 chiama il metodo metodo di creazione con un nuovo contatto, gruppo di controller di gruppo. Successivamente, il test di verifica che chiama il metodo Index () del controller di gruppo restituisce il nuovo gruppo di visualizzare i dati.

Il controller di gruppo modificato nel listato 5 contiene il livello minimo di modifiche è necessario passare il nuovo test.

**Nel listato 5 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Il controller di gruppo nel listato 5 dispone di una nuova azione di metodo di creazione. Questa azione aggiunge un gruppo a un insieme di gruppi. Si noti che l'azione Index () è stato modificato per restituire il contenuto della raccolta di gruppi.

In questo caso, sono stati eseguiti la quantità minima di lavoro necessaria per passare lo unit test. Dopo che è apportare queste modifiche al controller di gruppo, tutti i nostri unit test vengono superati.

## <a name="adding-validation"></a>Aggiunta della convalida

Questo requisito non è stato dichiarato in modo esplicito nella storia utente. Tuttavia, è ragionevole richiedere che un gruppo sia un nome. In caso contrario, l'organizzazione di contatti in gruppi non sarebbe molto utile.

Elenco 6 contiene un nuovo test che esprime intenzione. Questo test verifica che il tentativo di creare un gruppo senza fornire risultati un nome in un messaggio di errore di convalida nello stato del modello.

**Elenco 6 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

Per soddisfare questo test, che è necessario aggiungere una proprietà del nome per la classe di gruppo (vedere listato 7). Inoltre, è necessario aggiungere trascurabile della logica di convalida per il controller gruppo s azione metodo di creazione (vedere listato 8).

**Elenco 7 - Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**Elenco 8 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

Si noti che il controller di gruppo ora azione metodo di creazione contiene la logica di convalida sia nel database. Attualmente, il database utilizzato dal controller di gruppo costituito da più di una raccolta in memoria.

## <a name="time-to-refactor"></a>Tempo per effettuare il refactoring

Il terzo passaggio rosso/verde/refactoring è la parte di effettuare il refactoring. A questo punto, è necessario passare dal codice e valutare come è possibile effettuare il refactoring per migliorare la progettazione applicazione. La fase di effettuare il refactoring è la fase in cui riteniamo rigidi il metodo di implementazione di modelli e i principi di progettazione del software.

Ci sono disponibili per modificare il codice in modo che si sceglie di migliorare la progettazione del codice. Si dispone di una rete di protezione di unit test che ci impedire l'interruzione delle funzionalità esistenti.

Al momento, il controller di gruppo è dal punto di vista di progettazione di software. Il controller di gruppo contiene provare seri problemi di convalida e il codice di accesso ai dati. Per evitare di violare il principio di responsabilità singolo, è necessario separare queste problematiche in classi diverse.

La classe di controller gruppo refactoring è contenuta in elenco 9. Il controller è stato modificato per utilizzare il livello di servizio ContactManager. Questo è lo stesso livello di servizio utilizzato con il controller di contatto.

Elenco di 10 contiene i nuovi metodi aggiunti a livello di servizio ContactManager per supportare la convalida, visualizzazione e la creazione di gruppi. L'interfaccia IContactManagerService è stato aggiornato per includere i nuovi metodi.

Listato 11 contiene una nuova classe FakeContactManagerRepository che implementa l'interfaccia IContactManagerRepository. A differenza della classe EntityContactManagerRepository che implementa anche l'interfaccia IContactManagerRepository, la nuova classe FakeContactManagerRepository non comunica con il database. La classe FakeContactManagerRepository Usa una raccolta in memoria come proxy per il database. Questa classe nei nostri test unità utilizzeremo come livello repository fittizio.

**Elenco 9 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**Elenco di 10 - Controllers\ContactManagerService.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**Listato 11 - Controllers\FakeContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

Modifica il IContactManagerRepository interfaccia è necessario utilizzare per implementare i metodi CreateGroup() e ListGroups() nella classe EntityContactManagerRepository. Il modo più rapido e laziest per eseguire questa operazione consiste nell'aggiungere i metodi stub simili al seguente:   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]


Infine, queste modifiche alla progettazione dell'applicazione richiedono la apportare alcune modifiche a questo unit test. È ora necessario utilizzare il FakeContactManagerRepository durante l'esecuzione di unit test. La classe GroupControllerTest aggiornata è contenuta in elenco di 12.

**Elenco di 12 - Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

Dopo aver rendiamo tutti questi elementi cambia, in questo caso, tutti i nostri unit test superati. È stata completata l'intero ciclo di rosso/verde/eseguire il refactoring. È stata implementata prima due storie. È ora disponibile supporto per unit test per i requisiti stabiliti nelle storie utente. Implementare il resto delle storie utente consiste nel ripetere lo stesso ciclo di rosso/verde/eseguire il refactoring.

## <a name="modifying-our-database"></a>Modifica il Database

Purtroppo, anche se è stato soddisfatto tutti i requisiti espressi il nostro unit test, il lavoro non viene eseguita. È comunque necessario modificare il database.

È necessario creare una nuova tabella di database del gruppo. Attenersi ai passaggi riportati di seguito.

1. Nella finestra di Esplora Server, fare doppio clic sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**.
2. Immettere le due colonne descritte di seguito in Progettazione tabelle.
3. Contrassegnare la colonna Id come una chiave primaria e una colonna Identity.
4. Salvare la nuova tabella con i gruppi di nome facendo clic sull'icona dell'unità disco floppy.

<a id="0.11_table01"></a>


| **Nome colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | int | False |
| nome | nvarchar (50) | False |


Successivamente, è necessario eliminare tutti i dati dalla tabella Contacts (in caso contrario, viene acquisita t in grado di creare una relazione tra le tabelle di contatti e gruppi). Attenersi ai passaggi riportati di seguito.

1. La tabella Contacts e scegliere l'opzione di menu **Mostra dati tabella**.
2. Eliminare tutte le righe.

Successivamente, è necessario definire una relazione tra la tabella di database di gruppi e la tabella di database esistente di contatti. Attenersi ai passaggi riportati di seguito.

1. Fare doppio clic sulla tabella Contacts nella finestra di Esplora Server per aprire Progettazione tabelle.
2. Aggiungere una nuova colonna di tipo integer per la tabella contatti denominata GroupId.
3. Fare clic sul pulsante di relazione per aprire la finestra di dialogo Relazioni chiavi esterne (vedere la figura 3).
4. Fare clic sul pulsante Aggiungi.
5. Fare clic sul pulsante con puntini di sospensione che viene visualizzato accanto al pulsante tabella specifica e colonne.
6. Nella finestra di dialogo tabelle e colonne, selezionare i gruppi come la tabella di chiave primaria e l'Id come colonna chiave primaria. Selezionare i contatti come tabella chiave esterna e GroupId come colonna chiave esterna (vedere la figura 4). Fare clic sul pulsante OK.
7. In **specifica INSERT e UPDATE**, selezionare il valore **Cascade** per **Elimina regola**.
8. Fare clic sul pulsante chiude per chiudere la finestra di dialogo Relazioni chiavi esterne.
9. Fare clic sul pulsante Salva per salvare le modifiche alla tabella Contacts.


[![Creazione di una relazione tra tabelle di database](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**Figura 03**: creazione di una relazione tra tabelle di database ([fare clic per visualizzare l'immagine ingrandita](iteration-6-use-test-driven-development-cs/_static/image6.png))


[![Specificare le relazioni tra tabelle](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**Figura 04**: specificare le relazioni tra tabelle ([fare clic per visualizzare l'immagine ingrandita](iteration-6-use-test-driven-development-cs/_static/image8.png))


### <a name="updating-our-data-model"></a>Aggiornare il modello di dati

Successivamente, è necessario aggiornare il modello di dati per rappresentare la nuova tabella di database. Attenersi ai passaggi riportati di seguito.

1. Doppio clic sul file ContactManagerModel.edmx nella cartella Models per aprire la finestra di progettazione di entità.
2. Fare doppio clic nell'area di progettazione e selezionare l'opzione di menu **il modello di aggiornamento dal Database**.
3. Nell'aggiornamento guidato, selezionare i gruppi di tabella e scegliere la data di fine pulsante (vedere Figura 5).
4. L'entità di gruppi e scegliere l'opzione di menu **rinominare**. Modificare il nome del *gruppi* entità *gruppo* (singolare).
5. Fare clic sulla proprietà di navigazione di gruppi che viene visualizzato nella parte inferiore dell'entità Contact. Modificare il nome del *gruppi* proprietà di navigazione per *gruppo* (singolare).


[![L'aggiornamento di un modello di Entity Framework dal database](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**Figura 05**: l'aggiornamento di un modello di Entity Framework dal database ([fare clic per visualizzare l'immagine ingrandita](iteration-6-use-test-driven-development-cs/_static/image10.png))


Dopo aver completato questi passaggi, il modello di dati rappresenta sia i contatti e gruppi di tabelle. Entity Designer dovrebbe visualizzare entrambe le entità (vedere Figura 6).


[![Finestra di progettazione entità visualizzando Group e Contact](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**Figura 06**: finestra di progettazione entità visualizzando Group e Contact ([fare clic per visualizzare l'immagine ingrandita](iteration-6-use-test-driven-development-cs/_static/image12.png))


### <a name="creating-our-repository-classes"></a>La creazione di classi il Repository

Successivamente, è necessario implementare la classe di repository. Nel corso dell'iterazione, abbiamo aggiunto diversi nuovi metodi per l'interfaccia IContactManagerRepository durante la scrittura di codice per soddisfare il nostro unit test. La versione finale dell'interfaccia IContactManagerRepository è contenuta in elenco 14.

**Listato 14 - Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

È ora ancora non implementato uno dei metodi correlati all'utilizzo dei gruppi di contatti. La classe EntityContactManagerRepository attualmente dispone di metodi stub per ciascuno dei metodi di contatto gruppo elencati nell'interfaccia IContactManagerRepository. Ad esempio, il metodo ListGroups() attualmente è simile al seguente:

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

I metodi stub abilitata per compilare l'applicazione e passare gli unit test. Tuttavia, questo punto è effettivamente implementare questi metodi. La versione finale della classe EntityContactManagerRepository è contenuta in elenco 13.

**Elenco di 13 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>Creazione di viste

Applicazione MVC ASP.NET quando si utilizza il motore di visualizzazione ASP.NET predefinito. In tal caso, non si t creare viste in risposta a uno specifico unit test. Tuttavia, poiché un'applicazione è inutile senza visualizzazioni, è possibile t completare l'iterazione senza creare e modificare le visualizzazioni contenute nell'applicazione Gestione contatti.

È necessario creare le nuove viste seguenti per gestire i gruppi di contatto (vedere la figura 7):

- Views\Group\Index.aspx - Visualizza l'elenco di gruppi di contatti
- Views\Group\Delete.aspx - modulo di conferma consente di visualizzare per l'eliminazione di un gruppo di contatti


[![La visualizzazione dell'indice di gruppo](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**Figura 07**: Visualizza l'indice di gruppo ([fare clic per visualizzare l'immagine ingrandita](iteration-6-use-test-driven-development-cs/_static/image14.png))


È necessario modificare le seguenti viste esistenti in modo che includono gruppi di contatto:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

È possibile visualizzare le viste modificate esaminando l'applicazione di Visual Studio che accompagna questa esercitazione. La visualizzazione dell'indice di contatto, ad esempio, illustrato nella figura 8.


[![La visualizzazione dell'indice di contatto](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**Figura 08**: Visualizza l'indice di contatto ([fare clic per visualizzare l'immagine ingrandita](iteration-6-use-test-driven-development-cs/_static/image16.png))


## <a name="summary"></a>Riepilogo

In questa iterazione, è aggiunto nuove funzionalità per l'applicazione Contact Manager seguendo una metodologia di progettazione dell'applicazione di sviluppo basato su test. È stato avviato mediante la creazione di un set di storie utente. È stato creato un set di unit test che corrisponde ai requisiti espressi per le storie utente. Infine, abbiamo scritto codice appena sufficiente per soddisfare i requisiti stabiliti per gli unit test.

Dopo che è stata completata la scrittura di codice sufficiente per soddisfare i requisiti stabiliti per gli unit test, è stata aggiornata del database e visualizzazioni. Abbiamo aggiunto una nuova tabella di gruppi per il database e aggiornato il modello di dati di Entity Framework. È anche di creare e di modificare un set di viste.

Nell'iterazione successiva, l'iterazione finale - è riscrivere l'applicazione possa sfruttare i vantaggi di Ajax. Sfruttando la possibilità di Ajax, si sarà migliorare la velocità di risposta e prestazioni dell'applicazione Contact Manager.

> [!div class="step-by-step"]
> [Precedente](iteration-5-create-unit-tests-cs.md)
> [Successivo](iteration-7-add-ajax-functionality-cs.md)
