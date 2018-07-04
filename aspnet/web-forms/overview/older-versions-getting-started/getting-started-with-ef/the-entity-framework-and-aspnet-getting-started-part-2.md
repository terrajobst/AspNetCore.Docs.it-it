---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 2 | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET utilizzando Entity Framework. L'applicazione di esempio è...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: 476f3e45608bf79a6d2665424eba09cbfccd78fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371103"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 2
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>Il controllo EntityDataSource

Nell'esercitazione precedente è creato un sito web, un database e un modello di dati. In questa esercitazione viene usata la `EntityDataSource` controllo fornita da ASP.NET per renderlo più facile da usare con un modello di dati Entity Framework. Si creerà una `GridView` controllo per visualizzare e modificare i dati student, una `DetailsView` controllo per l'aggiunta di nuovi studenti e un `DropDownList` controllo per la selezione di un reparto (che verrà usato in un secondo momento per la visualizzazione associati corsi).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Si noti che in questa applicazione si non verrà aggiunta la convalida dell'input alle pagine che aggiornano il database e in parte la gestione degli errori non saranno così solido come potrebbe essere necessario eseguire in un'applicazione di produzione. Che mantiene l'esercitazione incentrata su Entity Framework ed evita la troppo lunga. Per informazioni dettagliate su come aggiungere queste funzionalità all'applicazione, vedere [Validating User Input in ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx) e [gestione degli errori nelle applicazioni e le pagine ASP.NET](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Aggiunta e configurazione del controllo EntityDataSource

Si inizierà configurando un `EntityDataSource` controllo leggere `Person` entità dal `People` set di entità.

Assicurarsi di avere Visual Studio aprire e che si lavora con il progetto creato nella parte 1. Se ancora stato compilato il progetto poiché è stato creato il modello di data o dopo l'ultima modifica apportata a tale, compilare il progetto a questo punto. Le modifiche al modello di dati non vengono resi disponibili nella finestra di progettazione fino a quando non viene compilato il progetto.

Creare una nuova pagina web usando il **Web Form mediante pagina Master** modello e denominarla *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Specificare *Site. master* come pagina master. Tutte le pagine create per queste esercitazioni userà questa pagina master.

[![image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

Nelle **origine** visualizzare, aggiungere un' `h2` sull'intestazione per il `Content` controllo denominato `Content2`, come illustrato nell'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Dal **dati** scheda della finestra di **della casella degli strumenti**, trascinare un' `EntityDataSource` controllo alla pagina, rilasciarla sotto l'intestazione e modificare l'ID per `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Passare a **Design** consente di visualizzare, fare clic su smart tag del controllo origine dati e quindi fare clic su **Configura origine dati** per avviare la **Configura origine dati** procedura guidata.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

Nel **Configura ObjectContext** passaggio della procedura guidata, seleziona **SchoolEntities** come valore per **denominata connessione**e selezionare **SchoolEntities**come il **DefaultContainerName** valore. Scegliere quindi **Avanti**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Nota: Se viene visualizzato nella finestra di dialogo seguente a questo punto, è necessario compilare il progetto prima di procedere.

[![image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

Nel **Configura selezione dati** passaggio, seleziona **persone** come valore per **EntitySetName**. Sotto **seleziona**, assicurarsi che il **selezionate a** ll casella di controllo è selezionata. Selezionare quindi le opzioni per abilitare l'aggiornamento ed eliminazione. Al termine, fare clic su **fine**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Configurazione delle regole di Database per consentire l'eliminazione

Si creerà una pagina che consente agli utenti di studenti da eliminare i `Person` tabella che include tre relazioni con altre tabelle (`Course`, `StudentGrade`, e `OfficeAssignment`). Per impostazione predefinita, il database impedirà l'eliminazione di una riga in `Person` se esistono righe correlate in una delle altre tabelle. È possibile eliminare manualmente le righe correlate prima di tutto, oppure è possibile configurare il database per eliminarli automaticamente quando si elimina un `Person` riga. Per i record degli studenti in questa esercitazione, si configurerà il database per eliminare automaticamente i dati correlati. Poiché gli studenti possono avere correlate righe solo nella `StudentGrade` tabella, è necessario configurare solo una delle tre relazioni.

Se si usa la *School. mdf* file che è stato scaricato il progetto da associare a questa esercitazione, è possibile ignorare questa sezione perché queste modifiche alla configurazione sono già stato fatto. Se è stato creato il database eseguendo uno script, configurare il database eseguendo le procedure seguenti.

Nelle **Esplora Server**, aprire il diagramma di database creato nella parte 1. Fare doppio clic la relazione tra `Person` e `StudentGrade` (la riga tra le tabelle), quindi selezionare **proprietà**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

Nel **delle proprietà** finestra, espandere **specifica INSERT e UPDATE** e impostare il **DeleteRule** proprietà **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Salvare e chiudere il diagramma. Se viene chiesto se si desidera aggiornare il database, fare clic su **Sì**.

Per assicurarsi che il modello mantiene le entità presenti in memoria sincronizzata con operazioni di database, è necessario impostare regole corrispondenti nel modello di dati. Aprire *SchoolModel*, fare doppio clic su questa linea di associazione tra `Person` e `StudentGrade`, quindi selezionare **proprietà**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

Nel **delle proprietà** impostare nella finestra **End1 OnDelete** al **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Salvare e chiudere il *SchoolModel* file e quindi ricompilare il progetto.

In generale, quando il database viene modificato, sono disponibili diverse opzioni per informazioni su come sincronizzare il modello:

- Per alcuni tipi di modifiche (ad esempio l'aggiunta o l'aggiornamento di tabelle, viste o stored procedure), fare doppio clic su nella finestra di progettazione e selezionare **Aggiorna modello da Database** disporre automaticamente le modifiche della casa automobilistica della finestra di progettazione.
- Rigenerare il modello di dati.
- Eseguire gli aggiornamenti manuali simile alla seguente.

In questo caso, si potrebbe avere rigenerare il modello o aggiornare le tabelle interessate dalla modifica relazione, ma è quindi necessario rendere di nuovo la modifica del nome di campo (da `FirstName` a `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Utilizzo di un controllo GridView per leggere e aggiornare le entità

In questa sezione si userà un `GridView` controllo da visualizzare, aggiornare o eliminare gli studenti.

Apre o passa al *Students.aspx* e passare alla **progettazione** visualizzazione. Dal **dati** scheda della finestra di **della casella degli strumenti**, trascinare un `GridView` controllo a destra del `EntityDataSource` di controllo, assegnargli il nome `StudentsGridView`, fare clic sullo smart tag e quindi selezionare  **StudentsEntityDataSource** come origine dati.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Fare clic su **Aggiorna Schema** (fare clic su **Yes** se viene chiesto di confermare), quindi fare clic su **Abilita Paging**, **Abilita ordinamento**, **Abilita modifica**, e **Abilita eliminazione**.

Fare clic su **Upravit Sloupce**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

Nel **campi selezionati** casella, eliminare **PersonID**, **LastName**, e **HireDate**. In genere non viene visualizzata una chiave del record per gli utenti, la data di assunzione non è rilevante per gli studenti e inserire entrambe le parti del nome in un campo, pertanto è necessario solo uno dei campi di nome.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Selezionare il **FirstMidName** campo e quindi fare clic su **Converti il campo in un TemplateField**.

Eseguire la stessa operazione per **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Fare clic su **OK** e quindi passare alla **origine** visualizzazione. Le modifiche rimanenti sarà più semplice eseguire direttamente nel markup. Il `GridView` controllare markup avrà l'aspetto simile all'esempio seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

La prima colonna dopo il comando è un modello campo che attualmente viene visualizzato il nome. Modificare il markup per il campo di modello come illustrato nell'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

In modalità di visualizzazione, due `Label` controlli visualizzano il nome e cognome. In modalità di modifica, vengono fornite due caselle di testo in modo che è possibile modificare il nome e cognome. Come con le `Label` controlli in modalità di visualizzazione, si utilizza `Bind` e `Eval` espressioni esattamente come si farebbero con controlli origine dati ASP.NET che si connettono direttamente ai database. L'unica differenza è che si sta specificando le proprietà delle entità anziché le colonne del database.

L'ultima colonna è un campo di modello che visualizza la data di registrazione. Modificare il codice per questo campo come illustrato nell'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

In entrambi visualizzare e modificare la modalità, la stringa di formato "{0, d}" fa in modo che la data da visualizzare nel formato "Data breve". (Il computer potrebbe essere configurato per visualizzare il formato specificato in modo diverso dalle immagini dello schermo illustrati in questa esercitazione).

Si noti che in ognuno di questi campi modello, la finestra di progettazione utilizzata una `Bind` espressione per impostazione predefinita, ma è stato modificato da un' `Eval` espressione nel `ItemTemplate` elementi. Il `Bind` espressione rende i dati disponibili in `GridView` controllare le proprietà nel caso in cui è necessario accedere ai dati nel codice. In questa pagina non è necessario accedere a tali dati nel codice, pertanto è possibile utilizzare `Eval`, che risulta più efficiente. Per altre informazioni, vedere [recupero dei dati all'esterno di controlli dati](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Revisione dei Markup del controllo EntityDataSource per migliorare le prestazioni

Nel markup per il `EntityDataSource` controllano, rimuovere il `ConnectionString` e `DefaultContainerName` gli attributi e sostituirle con un `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` attributo. Si tratta di una modifica è riempire ogni volta che crea un `EntityDataSource` controllare, a meno che non è necessario usare una connessione che è diversa da quello che è hardcoded nella classe del contesto di oggetto. Uso di `ContextTypeName` attributo offre i vantaggi seguenti:

- Prestazioni migliori. Quando la `EntityDataSource` controllo Inizializza il modello di dati usando il `ConnectionString` e `DefaultContainerName` attributi, consente di eseguire operazioni aggiuntive per caricare i metadati per ogni richiesta. Non è necessaria se si specificano le `ContextTypeName` attributo.
- Il caricamento lazy è attivato per impostazione predefinita nelle classi di contesto di oggetto generato (ad esempio `SchoolEntities` in questa esercitazione) in Entity Framework 4.0. Ciò significa che le proprietà di navigazione vengono caricate con i dati correlati automaticamente a destra quando ti serve. Il caricamento lazy è illustrato più dettagliatamente più avanti in questa esercitazione.
- Tutte le personalizzazioni che è stato applicato alla classe contesto di oggetto (in questo caso, il `SchoolEntities` classe) saranno disponibili per i controlli che utilizzano il `EntityDataSource` controllo. Personalizzare la classe del contesto di oggetto è un argomento avanzato che non è illustrato in questa serie di esercitazioni. Per altre informazioni, vedere [estendendo Entity Framework generati tipi](https://msdn.microsoft.com/library/dd456844.aspx).

Il markup sarà ora simile all'esempio seguente (l'ordine delle proprietà potrebbe essere diverso):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

Il `EnableFlattening` attributo fa riferimento a una funzionalità che è stata necessaria nelle versioni precedenti di Entity Framework perché le colonne chiave esterna non sono stati esposti come proprietà dell'entità. La versione corrente consente di usare *associazioni di chiavi esterne*, ovvero le proprietà di chiave esterna esposte per le associazioni di tutto tranne molti-a-molti. Se l'entità dispone di proprietà di chiave esterna e nessun [i tipi complessi](https://msdn.microsoft.com/library/bb738472.aspx), è possibile lasciare questo attributo è impostato su `False`. Non rimuovere l'attributo di markup, poiché il valore predefinito è `True`. Per altre informazioni, vedere [bidimensionalità degli oggetti (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Eseguire la pagina e viene visualizzato un elenco degli studenti e i dipendenti (si filtreranno per studenti semplicemente nella prossima esercitazione). Il nome e cognome sono visualizzati insieme.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Per ordinare la visualizzazione, fare clic su un nome di colonna.

Fare clic su **modifica** in qualsiasi riga. Vengono visualizzate le caselle di testo in cui è possibile modificare il nome e cognome.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

Il **eliminare** pulsante anche works. Fare clic su Elimina per una riga che presenta una data di registrazione e la riga viene rimossa. (Righe senza una data di registrazione rappresentano instructors (insegnanti) e potrebbe verificarsi un errore di integrità referenziale. Nella prossima esercitazione è possibile filtrare questo elenco per includere solo gli studenti.)

## <a name="displaying-data-from-a-navigation-property"></a>Visualizzazione dei dati da una proprietà di navigazione

Ora si supponga di voler conoscere quanti corsi ogni studente è registrato in. Entity Framework fornisce tali informazioni nel `StudentGrades` proprietà di navigazione del `Person` entità. Poiché la progettazione del database non supporta uno studente a essere registrati senza la necessità di un voto assegnato a un corso, per questa esercitazione è possibile presupporre tale presenza di una riga nel `StudentGrade` riga della tabella associata a un corso è quello utilizzato per la registrazione in corso. (Il `Courses` proprietà di navigazione è solo per gli insegnanti.)

Quando si usa la `ContextTypeName` attributo del `EntityDataSource` (controllo), Entity Framework automaticamente recupera le informazioni per una proprietà di navigazione quando si accede a tale proprietà. Questa operazione viene definita *caricamento lazy*. Tuttavia, ciò può risultare inefficiente, perché comporta una chiamata distinta per il database che sono necessarie informazioni aggiuntive ogni ora. Se sono necessari dati dalla proprietà di navigazione per ogni entità restituita dal `EntityDataSource` (controllo), è più efficiente per recuperare i dati correlati insieme all'entità in una singola chiamata al database. Questa operazione viene definita *caricamento eager*, e si specifica il caricamento eager per una proprietà di navigazione impostando le `Include` proprietà del `EntityDataSource` controllo.

Nelle *Students.aspx*, si desidera mostrare il numero di corsi per ogni studente, il caricamento eager così è la scelta migliore. Se sono stati la visualizzazione di tutti gli studenti ma che mostra il numero di corsi solo per alcune di esse (che sarebbe necessario scrivere il codice oltre al markup), il caricamento lazy potrebbe essere una scelta migliore.

Apre o passa al *Students.aspx*, passare alla **progettazione** visualizzarlo, selezionare `StudentsEntityDataSource`e nel **proprietà** set finestra il **inclusione**alla proprietà **StudentGrades**. (Se si desidera ottenere più proprietà di navigazione, è possibile specificare i nomi separati da virgole, ad esempio, **StudentGrades, corsi**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Passare a **origine** visualizzazione. Nel `StudentsGridView` (controllo), dopo l'ultimo `asp:TemplateField` elemento, aggiungere il nuovo campo modello seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

Nel `Eval` espressione, è possibile fare riferimento alla proprietà di navigazione `StudentGrades`. Poiché questa proprietà contiene una raccolta, dispone di un `Count` proprietà che è possibile usare per visualizzare il numero di corsi in cui viene registrato lo studente. In un'esercitazione successiva verrà illustrato come visualizzare i dati dalle proprietà di navigazione che contengono una singola entità invece raccolte. (Si noti che non è possibile usare `BoundField` elementi per visualizzare i dati dalle proprietà di navigazione.)

Eseguire la pagina e ora visualizzato come molti corsi ogni studente è registrato in.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Utilizzo di un controllo DetailsView l'inserimento di entità

Il passaggio successivo consiste nel creare una pagina con un `DetailsView` controllo che consentirà di aggiungere nuovi studenti. Chiudere il browser e quindi creare una nuova pagina web usando il *Site. master* pagina master. Denominare la pagina *StudentsAdd.aspx*, quindi passare alla **origine** visualizzazione.

Aggiungere il markup seguente per sostituire il markup esistente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Questo codice crea un' `EntityDataSource` controllo che è simile a quella creata nel *Students.aspx*, ad eccezione del fatto che consente l'inserimento. Come con le `GridView` controllare, i campi associati del `DetailsView` controllo sono codificate esattamente come avviene per un controllo dati che si connette direttamente a un database, ad eccezione del fatto che fanno riferimento a proprietà dell'entità. In questo caso, il `DetailsView` controllo viene usato solo per l'inserimento di righe, quindi impostare la modalità predefinita su `Insert`.

Eseguire la pagina e aggiungere un nuovo studente.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Non accade nulla dopo aver inserito un nuovo studente, ma se si esegue adesso *Students.aspx*, si noterà che le nuove informazioni sugli studenti.

## <a name="displaying-data-in-a-drop-down-list"></a>Visualizzazione dei dati in un elenco a discesa

Nei passaggi seguenti si imposterà un metodo databind una `DropDownList` controllo a un'entità impostata con un `EntityDataSource` controllo. In questa parte dell'esercitazione, non altre operazioni con un elenco. Nelle parti successive, tuttavia, si userà l'elenco per consentire agli utenti di selezionare un reparto per visualizzare i corsi associati con il reparto.

Creare una nuova pagina web denominata *Courses.aspx*. Nelle **origine** consente di visualizzare, aggiungere un'intestazione per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

Nella **Design** consente di visualizzare, aggiungere un' `EntityDataSource` controllo alla pagina come descritto in precedenza, ma questa volta denominarlo `DepartmentsEntityDataSource`. Selezionare **reparti** come la **EntitySetName** valore e selezionare solo il **DepartmentID** e **nome** proprietà.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Dal **Standard** scheda della finestra di **della casella degli strumenti**, trascinare un `DropDownList` alla pagina di controllo, assegnargli il nome `DepartmentsDropDownList`, fare clic sullo smart tag e selezionare **Scegli origine dati** a avviare il **configurazione guidata origine dati**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

Nel **scegliere un'origine dati** passaggio, seleziona **DepartmentsEntityDataSource** come origine dati, fare clic su **Aggiorna Schema**, quindi selezionare **nome** come campo di dati da visualizzare e **DepartmentID** come campo di dati del valore. Fare clic su **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Il metodo usato da associare a dati di controllo usando Entity Framework è lo stesso come con altri dati ASP.NET sta specificando i controlli di origine, ad eccezione di entità e proprietà dell'entità.

Passare a **origine** consente di visualizzare e aggiungere "selezionare un reparto:" immediatamente prima di `DropDownList` controllo.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Come promemoria, modificare il markup per il `EntityDataSource` controllo a questo punto, sostituendo il `ConnectionString` e `DefaultContainerName` gli attributi con un `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` attributo. Spesso è meglio attendere fino a dopo aver creato il controllo con associazione a dati che è collegato al controllo origine dati prima di modificare la `EntityDataSource` controllo markup, perché dopo aver apportato la modifica, la finestra di progettazione non fornirà all'utente un **Aggiorna Schema** opzione nel controllo associato a dati.

Eseguire la pagina ed è possibile selezionare un reparto dall'elenco a discesa.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Questo argomento completa l'introduzione all'uso di `EntityDataSource` controllo. Utilizzo di questo controllo a livello generale non è diverso dall'utilizzo di altri dati ASP.NET controlli origine, ad eccezione del fatto che si fanno riferimento a entità e proprietà invece di tabelle e colonne. L'unica eccezione è quando si desidera accedere alle proprietà di navigazione. Nella prossima esercitazione si noterà che la sintassi è utilizzare con `EntityDataSource` controllo dipendono anche da altri controlli origine dati quando si filtrare, raggruppare e ordinare i dati.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-3.md)
