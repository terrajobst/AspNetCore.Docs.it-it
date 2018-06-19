---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e ASP.NET 4 di Web Form - parte 2 | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET tramite Entity Framework. È l'applicazione di esempio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: a6c95a92aa77e2bb73aa513a207e0469d1aedbd2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890554"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e form ASP.NET Web 4 - parte 2
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET utilizzando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>Il controllo EntityDataSource

Nell'esercitazione precedente è stato creato un sito web, un database e un modello di dati. In questa esercitazione è possibile utilizzare con il `EntityDataSource` controllo fornita da ASP.NET per rendere più facile da utilizzare con un modello di dati di Entity Framework. Si creerà un `GridView` controllo per visualizzare e modificare i dati degli studenti, un `DetailsView` controllo per l'aggiunta di nuovi studenti e un `DropDownList` controllo per la selezione di un reparto (che verranno usati in un secondo momento per la visualizzazione dei corsi associati).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Si noti che in questa applicazione è non verrà aggiunta la convalida dell'input alle pagine in cui aggiornano il database e tra la gestione degli errori non sarà così solido come potrebbe essere necessario eseguire in un'applicazione di produzione. Che mantiene l'esercitazione con stato attivo su Entity Framework e ne troppo lunga. Per informazioni dettagliate su come aggiungere queste funzionalità a un'applicazione, vedere [convalida dell'Input utente in ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx) e [gestione degli errori nelle pagine ASP.NET e applicazioni](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Aggiunta e configurazione del controllo EntityDataSource

Iniziare configurando un `EntityDataSource` controllo leggere `Person` entità dal `People` set di entità.

Assicurarsi che Visual Studio aprire e che sta utilizzando il progetto è creata nella parte 1. Se ancora stato compilato il progetto dopo la creazione del modello di dati o dopo l'ultima modifica apportata, compilare il progetto ora. Modifiche al modello di dati non sono disponibili nella finestra di progettazione fino a quando non viene compilato il progetto.

Creare una nuova pagina web utilizzando il **Web Form mediante pagina Master** , modello e denominarlo *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Specificare *Site. master* come pagina master. Tutte le pagine create per queste esercitazioni verranno utilizzare questa pagina master.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

In **origine** visualizzare, aggiungere un `h2` intestazione per il `Content` controllo denominato `Content2`, come illustrato nell'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Dal **dati** scheda della finestra il **della casella degli strumenti**, trascinare un `EntityDataSource` alla pagina, rilasciarla sotto l'intestazione e modificare l'ID `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Passare a **progettazione** consente di visualizzare, fare clic smart tag del controllo origine dati e quindi fare clic su **Configura origine dati** per avviare il **Configura origine dati** procedura guidata.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

Nel **configurare ObjectContext** passaggio della procedura guidata, seleziona **SchoolEntities** come valore per **connessione denominata**e selezionare **SchoolEntities**come il **DefaultContainerName** valore. Scegliere quindi **Avanti**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Nota: Se viene visualizzato nella finestra di dialogo seguente a questo punto, è necessario compilare il progetto prima di procedere.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

Nel **Configura selezione dati** passaggio seleziona **persone** come valore per **EntitySetName**. In **selezionare**, assicurarsi che il **seleziona** ll casella di controllo è selezionata. Selezionare quindi le opzioni per abilitare l'aggiornamento ed eliminazione. Al termine, fare clic su **fine**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Configurazione delle regole di Database per consentire l'eliminazione

Verrà creata una pagina che consente agli utenti di eliminare studenti di `Person` tabella che dispone di tre relazioni con altre tabelle (`Course`, `StudentGrade`, e `OfficeAssignment`). Per impostazione predefinita, il database verrà impedire l'eliminazione di una riga in `Person` se esistono righe correlate in una delle altre tabelle. È possibile eliminare manualmente le righe correlate prima di tutto, oppure è possibile configurare il database per eliminarli automaticamente quando si elimina un `Person` riga. Per i record dello studente in questa esercitazione, si configurerà il database per eliminare automaticamente i dati correlati. Poiché gli studenti possono avere le righe correlate solo nel `StudentGrade` tabella, è necessario configurare solo una delle tre relazioni.

Se si usa il *School. mdf* file sia stato scaricato dal progetto che va inserito in questa esercitazione, è possibile ignorare questa sezione perché queste modifiche alla configurazione già eseguiti. Se è stato creato il database eseguendo uno script, è possibile configurare il database eseguendo le procedure seguenti.

In **Esplora Server**, aprire il diagramma di database creato nella parte 1. Fare clic sulla relazione tra `Person` e `StudentGrade` (la riga tra le tabelle), quindi selezionare **proprietà**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

Nel **proprietà** finestra, espandere **specifica INSERT e UPDATE** e impostare il **DeleteRule** proprietà **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Salvare e chiudere il diagramma. Se viene chiesto se si desidera aggiornare il database, fare clic su **Sì**.

Per assicurarsi che il modello consente di mantenere le entità che sono in memoria in sincronia con operazioni di database, è necessario impostare regole corrispondenti nel modello di dati. Aprire *SchoolModel*, fare doppio clic su questa linea di associazione tra `Person` e `StudentGrade`, quindi selezionare **proprietà**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

Nel **proprietà** finestra impostare **End1 OnDelete** a **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Salvare e chiudere il *SchoolModel* file e quindi ricompilare il progetto.

In generale, quando viene modificato il database, sono disponibili diverse opzioni per la sincronizzazione con il modello:

- Per alcuni tipi di modifiche (ad esempio aggiunta o aggiornamento di tabelle, viste o stored procedure), fare doppio clic su nella finestra di progettazione e selezionare **il modello di aggiornamento dal Database** disporre automaticamente le modifiche di creazione della finestra di progettazione.
- Rigenerare il modello di dati.
- Effettuare aggiornamenti manuali simile alla seguente.

In questo caso, si potrebbe avere rigenerato il modello o aggiornare le tabelle interessate dalla modifica relazione, ma quindi è necessario apportare la modifica del nome di campo nuovo (da `FirstName` a `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Utilizzo di un controllo GridView per leggere e aggiornare le entità

In questa sezione si userà un `GridView` controllo per visualizzare, aggiornare o eliminare gli studenti.

Aprire o passare a *Students.aspx* e passare a **progettazione** visualizzazione. Dal **dati** scheda della finestra il **della casella degli strumenti**, trascinare un `GridView` controllo a destra del `EntityDataSource` di controllo, il nome `StudentsGridView`, fare clic sullo smart tag e quindi selezionare  **StudentsEntityDataSource** come origine dati.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Fare clic su **Aggiorna Schema** (fare clic su **Sì** se viene chiesto di confermare), quindi fare clic su **Abilita Paging**, **Abilita ordinamento**, **Abilita modifica**, e **consente di eliminare**.

Fare clic su **modificare colonne**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

Nel **campi selezionati** eliminare **PersonID**, **LastName**, e **HireDate**. In genere non viene visualizzata una chiave di record per gli utenti, la data di assunzione non è pertinente per studenti e verrà inserita entrambe le parti del nome in un campo, pertanto è necessario solo uno dei campi del nome.)

[![image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Selezionare il **FirstMidName** campo e quindi fare clic su **Converti il campo in un TemplateField**.

Eseguire la stessa operazione **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Fare clic su **OK** e quindi passare a **origine** visualizzazione. Le modifiche rimanenti alle risulterà più semplice eseguire direttamente nel markup. Il `GridView` controllare markup avrà l'aspetto illustrato nell'esempio seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

Dopo il comando campo è un modello che attualmente la prima colonna Visualizza il nome. Modificare il markup per il campo del modello come illustrato nell'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

In modalità di visualizzazione, due `Label` controlli visualizzano il nome e cognome. In modalità di modifica vengono fornite due caselle di testo, pertanto è possibile modificare il nome e cognome. Come con la `Label` controlli in modalità di visualizzazione, utilizzare `Bind` e `Eval` espressioni esattamente come si farebbe con controlli di origine dati ASP.NET che si connettono direttamente al database. L'unica differenza è che si sta specificando le proprietà delle entità anziché colonne di database.

L'ultima colonna è un campo di modello che visualizza la data di registrazione. Modificare il markup per questo campo, come illustrato nell'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

In entrambi visualizzare e modificare la modalità, la stringa di formato "{0, d}", la data da visualizzare nel formato "Data breve". (Il computer potrebbe essere configurato per visualizzare il formato specificato in modo diverso dalle immagini schermata illustrata in questa esercitazione).

Si noti che in ognuno di questi campi del modello, la finestra di progettazione utilizzata una `Bind` espressione per impostazione predefinita, ma è stato modificato da un `Eval` espressione il `ItemTemplate` elementi. Il `Bind` espressione rende i dati disponibili in `GridView` le proprietà del controllo nel caso in cui è necessario accedere ai dati nel codice. In questa pagina non è necessario accedere a tali dati nel codice, pertanto è possibile utilizzare `Eval`, che risulta più efficiente. Per ulteriori informazioni, vedere [ottenere i dati dai controlli dati](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Revisione dei Markup del controllo EntityDataSource per migliorare le prestazioni

Nel markup di `EntityDataSource` di controllo, rimuovere il `ConnectionString` e `DefaultContainerName` gli attributi e sostituirle con una `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` attributo. Si tratta di una modifica apportata ogni volta che si crea un `EntityDataSource` controllare, a meno che non è necessario utilizzare una connessione diversa da quella a livello di codice nella classe del contesto di oggetto. Utilizzo di `ContextTypeName` attributo offre i vantaggi seguenti:

- Prestazioni migliori. Quando il `EntityDataSource` controllo Inizializza il modello di dati tramite il `ConnectionString` e `DefaultContainerName` gli attributi, esegue ulteriori operazioni per caricare i metadati per ogni richiesta. Questo non è necessario se si specificano le `ContextTypeName` attributo.
- Caricamento lazy è attivato per impostazione predefinita nelle classi di oggetti generati contesto (ad esempio `SchoolEntities` in questa esercitazione) in Entity Framework 4.0. Ciò significa che le proprietà di navigazione vengono caricate con i dati correlati automaticamente destra quando è necessario. Caricamento lazy è illustrato più dettagliatamente più avanti in questa esercitazione.
- Tutte le personalizzazioni applicate alla classe contesto di oggetto (in questo caso, il `SchoolEntities` classe) sarà disponibile per i controlli che utilizzano il `EntityDataSource` controllo. Personalizzazione della classe di contesto di oggetto è un argomento avanzato che non viene descritta in questa serie di esercitazioni. Per ulteriori informazioni, vedere [tipi generati di estensione Entity Framework](https://msdn.microsoft.com/library/dd456844.aspx).

Il markup sarà ora simile nell'esempio seguente (l'ordine delle proprietà potrà essere diverso):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

Il `EnableFlattening` attributo fa riferimento a una funzionalità che è stata necessaria nelle versioni precedenti di Entity Framework, in quanto le colonne chiave esterna non sono stati esposti come proprietà dell'entità. La versione corrente consente di utilizzare *associazioni di chiavi esterne*, ovvero una proprietà di chiave esterna vengono esposte per le associazioni di tutto tranne molti-a-molti. Se l'entità dispone di proprietà di chiave esterna e nessun [tipi complessi](https://msdn.microsoft.com/library/bb738472.aspx), è possibile lasciare questo attributo è impostato su `False`. Non rimuovere l'attributo dal markup, poiché il valore predefinito è `True`. Per ulteriori informazioni, vedere [bidimensionalità degli oggetti (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Eseguire la pagina e viene visualizzato un elenco di studenti e dipendenti (si filtreranno per studenti soli nella prossima esercitazione). Vengono visualizzati insieme al nome e cognome.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Per ordinare la visualizzazione, fare clic su un nome di colonna.

Fare clic su **modificare** in qualsiasi riga. Vengono visualizzate le caselle di testo in cui è possibile modificare il nome e cognome.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

Il **eliminare** pulsante anche works. Per una riga che ha una data di registrazione e la riga non viene più visualizzata, fare clic su Elimina. (Righe senza una data di registrazione rappresentano i docenti e potrebbe verificarsi un errore di integrità referenziale. Nella prossima esercitazione si filtrerà questo elenco per includere solo gli studenti.)

## <a name="displaying-data-from-a-navigation-property"></a>Visualizzazione dei dati da una proprietà di navigazione

Ora si supponga di voler conoscere quanti corsi ogni studente è iscritto. Entity Framework fornisce le informazioni nel `StudentGrades` proprietà di navigazione del `Person` entità. Poiché la progettazione del database non consenta uno studente di iscriversi a un corso senza la necessità di un voto assegnato, per questa esercitazione si può presupporre che la presenza di una riga nel `StudentGrade` riga della tabella è associata a un corso equivale la registrazione in corso. (Il `Courses` proprietà di navigazione è solo per i docenti.)

Quando si utilizza il `ContextTypeName` attributo del `EntityDataSource` (controllo), Entity Framework recupera automaticamente le informazioni per una proprietà di navigazione quando si accede a tale proprietà. Si tratta di *caricamento lazy*. Tuttavia, ciò può risultare inefficiente, perché comporta una chiamata distinta per il database che sono necessaria ogni ulteriori informazioni sul tempo. Se sono necessari i dati dalla proprietà di navigazione per ogni entità restituita dal `EntityDataSource` (controllo), risulta più efficiente per recuperare i dati correlati con l'entità in una singola chiamata al database. Si tratta di *caricamento eager*, e si specifica un caricamento eager per una proprietà di navigazione impostando il `Include` proprietà del `EntityDataSource` controllo.

In *Students.aspx*, che si desidera visualizzare il numero di corsi per tutti gli studenti, caricamento eager così è la scelta migliore. Se la visualizzazione di tutti gli studenti ma che mostra il numero di corsi solo per alcuni di essi (che richiedono la scrittura di codice oltre il markup), il caricamento lazy potrebbe essere una scelta migliore.

Apre o passa al *Students.aspx*, passare alla **progettazione** visualizzazione, selezionare `StudentsEntityDataSource`e il **proprietà** finestra set il **Include**proprietà **StudentGrades**. (Se si desidera ottenere più proprietà di navigazione, è possibile specificare i nomi separati da virgole, ad esempio, **StudentGrades, corsi**.)

[![image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Passare a **origine** visualizzazione. Nel `StudentsGridView` controllo, dopo l'ultimo `asp:TemplateField` elemento, aggiungere il nuovo campo di modello seguenti:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

Nel `Eval` espressione, è possibile fare riferimento la proprietà di navigazione `StudentGrades`. Poiché questa proprietà contiene una raccolta, dispone di un `Count` proprietà che è possibile utilizzare per visualizzare il numero dei corsi in cui viene registrato lo studente. In un'esercitazione successiva si noterà come visualizzare i dati di proprietà di navigazione che contengono una singola entità anziché le raccolte. (Si noti che non è possibile utilizzare `BoundField` elementi per visualizzare i dati dalle proprietà di navigazione.)

Eseguire la pagina e verranno visualizzati come molti corsi ogni studente è registrato in.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Utilizzo di un controllo DetailsView per inserire entità

Il passaggio successivo consiste nel creare una pagina con un `DetailsView` controllo che consente di aggiungere nuovi studenti. Chiudere il browser e quindi creare una nuova pagina web utilizzando il *Site. master* pagina master. Denominare la pagina *StudentsAdd.aspx*, quindi passare alla **origine** visualizzazione.

Aggiungere il markup seguente per sostituire il codice esistente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Questo codice crea un `EntityDataSource` controllo che è simile a quello creato in *Students.aspx*, ad eccezione del fatto che consente l'inserimento. Come con la `GridView` controllare, i campi associati del `DetailsView` controllo sono codificate esattamente come avviene per un controllo dati che si connette direttamente a un database, ad eccezione del fatto che fanno riferimento a proprietà dell'entità. In questo caso, il `DetailsView` controllo viene utilizzato solo per l'inserimento di righe, in modo da impostare la modalità predefinita `Insert`.

Eseguire la pagina e aggiungere un nuovo studente.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Non si verifica dopo aver inserito un nuovo studente, ma se si esegue ora *Students.aspx*, si noterà che le informazioni sul nuovo studente.

## <a name="displaying-data-in-a-drop-down-list"></a>La visualizzazione dei dati in un elenco a discesa

Nei passaggi seguenti, sarà necessario databind un `DropDownList` controllo a un'entità impostata utilizzando un `EntityDataSource` controllo. In questa parte dell'esercitazione, non sarà sufficiente gran parte a questo elenco. Nelle parti successive, tuttavia, si userà l'elenco per consentire agli utenti di selezionare un reparto per visualizzare i corsi associati al reparto.

Creare una nuova pagina web denominata *Courses.aspx*. In **origine** visualizzare, aggiungere un'intestazione per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

In **progettazione** visualizzare, aggiungere un `EntityDataSource` controllo alla pagina come descritto in precedenza, ma questa volta il nome `DepartmentsEntityDataSource`. Selezionare **reparti** come il **EntitySetName** valore e selezionare solo la **DepartmentID** e **nome** proprietà.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Dal **Standard** scheda della finestra il **della casella degli strumenti**, trascinare un `DropDownList` alla pagina di controllo, il nome `DepartmentsDropDownList`, fare clic sullo smart tag e selezionare **Scegli origine dati** per avviare il **configurazione guidata origine dati**.

[![image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

Nel **scegliere un'origine dati** passaggio seleziona **DepartmentsEntityDataSource** come origine dati, fare clic su **Aggiorna Schema**, quindi selezionare **nome** come il campo dei dati per visualizzare e **DepartmentID** del campo dati valore. Fare clic su **OK**.

[![image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Il metodo da utilizzare come associare il controllo tramite Entity Framework è la stessa, come con altri dati ASP.NET specifica i controlli di origine, ad eccezione di entità e proprietà dell'entità.

Passare a **origine** consente di visualizzare e aggiungere "selezionare un reparto:" immediatamente prima di `DropDownList` controllo.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Come promemoria, modificare il markup per il `EntityDataSource` controllo a questo punto, sostituendo il `ConnectionString` e `DefaultContainerName` gli attributi con un `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` attributo. È spesso consigliabile attendere fino a dopo aver creato il controllo con associazione a dati che è collegato al controllo origine dati prima di modificare il `EntityDataSource` controllare markup, poiché dopo aver apportato la modifica, la finestra di progettazione non fornirà con un **Aggiorna Schema** opzione nel controllo con associazione a dati.

Eseguire la pagina ed è possibile selezionare un reparto dall'elenco a discesa.

[![image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Introduzione all'uso è stata completata la `EntityDataSource` controllo. Utilizzo di questo controllo è in genere non è diverso dall'utilizzo di altri dati ASP.NET controlli origine, ad eccezione del fatto che si fa riferimento a entità e proprietà invece di tabelle e colonne. L'unica eccezione è quando si desidera accedere alle proprietà di navigazione. Nella prossima esercitazione si noterà che la sintassi è utilizzare con `EntityDataSource` controllo potrebbero essere diversi anche da altri controlli origine dati quando il filtro, raggruppamento e ordinamento dei dati.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-3.md)
