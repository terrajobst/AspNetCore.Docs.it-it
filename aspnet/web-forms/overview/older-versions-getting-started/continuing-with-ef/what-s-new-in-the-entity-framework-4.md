---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Novità di Entity Framework 4.0 | Documenti Microsoft
author: tdykstra
description: Questa serie di esercitazioni compila nell'applicazione web di Contoso University creato da introduttiva la serie di esercitazioni di Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 04444ce98fa60045cf617a6c518dd55677258148
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889186"
---
<a name="whats-new-in-the-entity-framework-40"></a>Novità di Entity Framework 4.0
====================
da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web di Contoso University creando il [Introduzione a Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie di esercitazioni. Se non ha completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) che consente di creare. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creati tramite la serie di esercitazioni completo. Nel caso di problemi con le esercitazioni, è possibile registrarli per il [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Nell'esercitazione precedente si è visto alcuni metodi per ottimizzare le prestazioni di un'applicazione web che usa Entity Framework. In questa esercitazione illustra alcune delle nuove funzionalità più importanti nella versione 4 di Entity Framework e fornisca un collegamento alle risorse che forniscono un'introduzione più completa a tutte le nuove funzionalità. Funzionalità trattate in questa esercitazione, tra cui:

- Associazioni di chiave esterna.
- L'esecuzione di comandi SQL definiti dall'utente.
- Prima di modello di sviluppo.
- Supporto POCO.

Inoltre, l'esercitazione introdurrà brevemente *sviluppo di codice-first*, una funzionalità che proviene nella prossima versione di Entity Framework.

Per avviare l'esercitazione, avviare Visual Studio e aprire l'applicazione web di Contoso università che si stava lavorando nell'esercitazione precedente.

## <a name="foreign-key-associations"></a>Associazioni di chiave esterna

Nella versione 3.5 di Entity Framework sono incluse le proprietà di navigazione, ma non includere le proprietà di chiave esterna nel modello di dati. Ad esempio, il `CourseID` e `StudentID` colonne di `StudentGrade` verrebbe omesso dalla tabella il `StudentGrade` entità.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Il motivo di questo approccio è che, in senso stretto, le chiavi esterne sono un dettaglio di implementazione fisica e non fanno parte di un modello di dati concettuale. Tuttavia, per praticità, è spesso più facile lavorare con le entità nel codice, quando si ha accesso diretto alle chiavi esterne.

Per un esempio di chiavi esterne come nel modello di dati è possibile semplificare il codice, prendere in considerazione come sarebbe necessario al codice di *DepartmentsAdd.aspx* pagina senza di essi. Nel `Department` entità, il `Administrator` proprietà è una chiave esterna che corrisponde a `PersonID` nel `Person` entità. Per stabilire l'associazione tra una nuova categoria e amministratore, tutti richiesto è stato impostato il valore per il `Administrator` proprietà il `ItemInserting` gestore dell'evento del controllo con associazione a dati:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Senza le chiavi esterne nel modello di dati, è necessario gestire il `Inserting` evento del controllo origine dati anziché il `ItemInserting` evento del controllo con associazione a dati, per ottenere un riferimento all'entità stessa prima che l'entità viene aggiunto al set di entità. Quando si dispone di tale riferimento, stabilire l'associazione utilizzando codice analogo negli esempi seguenti:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Come si può notare del team di Entity Framework [post di blog su associazioni di chiave esterna](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), esistono altri casi in cui la differenza di complessità del codice è molto più elevata. Per soddisfare le esigenze degli utenti che preferiscono in tempo reale con i dettagli di implementazione del modello di dati concettuale per semplificare il codice, Entity Framework ora offre la possibilità di includere le chiavi esterne nel modello di dati.

Nella terminologia di Entity Framework, se si includono le chiavi esterne nel modello di dati in uso *associazioni di chiavi esterne*, e se si esclude le chiavi esterne in uso *associazioni indipendenti*.

## <a name="executing-user-defined-sql-commands"></a>L'esecuzione di comandi SQL definite dall'utente

Nelle versioni precedenti di Entity Framework, si è verificato alcun metodo semplice per creare i comandi SQL in tempo reale ed eseguirle. Entity Framework generati dinamicamente i comandi SQL per l'utente o era necessario creare una stored procedure e importarlo come una funzione. Versione 4 aggiunge `ExecuteStoreQuery` e `ExecuteStoreCommand` metodi di `ObjectContext` classe che rendono più semplice per il passaggio di una query direttamente al database.

Si supponga che gli amministratori University Contoso desiderano essere in grado di eseguire modifiche di massa nel database senza dover passare attraverso il processo di creazione di una stored procedure e importarlo nel modello di dati. La prima richiesta è per una pagina che consente di modificare il numero di crediti per tutti i corsi nel database. Nella pagina web, si vuole essere in grado di immettere un numero da usare per moltiplicare il valore di ogni `Course` riga `Credits` colonna.

Creare una nuova pagina che utilizza il *Site. master* pagina master e denominarlo *UpdateCredits.aspx*. Quindi aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Questo codice crea un `TextBox` controllo in cui l'utente può immettere il valore del moltiplicatore un `Button` per eseguire il comando, fare clic sul controllo e un `Label` controllo per indicare il numero di righe interessate.

Aprire *UpdateCredits.aspx.cs*e aggiungere le seguenti `using` istruzione e un gestore per il pulsante `Click` evento:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Questo codice viene eseguito SQL `Update` comando utilizzando il valore nella casella di testo e utilizza l'etichetta per visualizzare il numero di righe interessate. Prima di eseguire la pagina, eseguire il *Courses.aspx* pagina per ottenere un quadro "prima" di alcuni dati.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Eseguire *UpdateCredits.aspx*, immettere "10" come moltiplicatore e quindi fare clic su **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Eseguire il *Courses.aspx* pagina per visualizzare i dati modificati.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Se si desidera impostare il numero di crediti i relativi valori originali, in *UpdateCredits.aspx.cs* modificare `Credits * {0}` a `Credits / {0}` ed eseguire di nuovo la pagina, immettere 10 come divisore.)

Per ulteriori informazioni sull'esecuzione di query definite nel codice, vedere [come: direttamente eseguire i comandi con l'origine dati](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Prima di modello di sviluppo

In queste procedure dettagliate è stato creato il database prima e quindi generato il modello di dati basato sulla struttura di database. In Entity Framework 4 è possibile iniziare con il modello di dati invece e generare il database in base alla struttura di modello di dati. Se si sta creando un'applicazione per cui il database non esiste già, l'approccio model first consente di creare entità e relazioni che ha senso concettualmente per l'applicazione, mentre non preoccuparsi dei dettagli di implementazione fisica . (Questa situazione si verifica solo nelle fasi iniziali di sviluppo, tuttavia. Infine il database verrà creato e disporrà di dati di produzione in essa contenuti e ricrearla dal modello non sarà più possibile pratica; a questo punto sarà nuovamente per l'approccio di database-first.)

In questa sezione dell'esercitazione, si sarà creare un semplice modello di dati e generare il database da esso.

In **Esplora**, fare doppio clic su di *DAL* cartella e selezionare **Aggiungi nuovo elemento**. Nel **Aggiungi nuovo elemento** nella finestra di dialogo **modelli installati** selezionare **dati** e quindi selezionare il **ADO.NET Entity Data Model** modello . Denominare il nuovo file *AlumniAssociationModel.edmx* e fare clic su **Aggiungi**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Verrà avviata la procedura guidata Entity Data Model. Nel **Scegli contenuto Model** passaggio seleziona **modello vuoto** e quindi fare clic su **fine**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

Il **Entity Data Model Designer** apre con un'area di progettazione vuoto. Trascinare un **entità** articolo dal **della casella degli strumenti** nell'area di progettazione.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Modificare il nome dell'entità da `Entity1` a `Alumnus`, modificare il `Id` nome della proprietà `AlumnusId`e aggiungere una nuova proprietà scalari denominata `Name`. Per aggiungere nuove proprietà, è possibile premere INVIO dopo la modifica del nome del `Id` colonna, o l'entità e scegliere **Aggiungi proprietà scalari**. È il tipo predefinito per le nuove proprietà `String`, che è appropriato per questa dimostrazione semplice, ma ovviamente è possibile modificare elementi come tipo di dati di **proprietà** finestra.

Creare un'altra entità in modo analogo e denominarlo `Donation`. Modifica il `Id` proprietà `DonationId` e aggiungere una proprietà scalare denominata `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Per aggiungere un'associazione tra queste due entità, fare doppio clic su di `Alumnus` entità, selezionare **Aggiungi**, quindi selezionare **associazione**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

I valori predefiniti nel **Aggiungi associazione** la finestra di dialogo sono (uno-a-molti, includere le proprietà di navigazione, includere chiavi esterne), quindi fare clic su **OK**.

[![image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

La finestra di progettazione aggiunge una linea di associazione e una proprietà di chiave esterna.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

A questo punto si è pronti per creare il database. Fare doppio clic su area di progettazione e seleziona **genera Database da modello**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Verrà avviata la generazione guidata Database. (Se si ricevono avvisi che indicano le entità non sono stato eseguito il mapping, è possibile ignorare quelli per il momento.)

Nel **Seleziona connessione dati** passaggio, fare clic su **nuova connessione**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

Nel **le proprietà di connessione** finestra di dialogo, selezionare l'istanza locale di SQL Server Express e denominare il database `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Fare clic su **Sì** quando viene chiesto se si desidera creare il database. Quando il **Seleziona connessione dati** verrà nuovamente visualizzata, fare clic su **Avanti**.

Nel **riepilogo e le impostazioni** passaggio, fare clic su **fine**.

[![image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Oggetto *SQL* file con i comandi di data definition language (DDL) viene creato, ma i comandi non sono stati ancora eseguiti.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Utilizzare uno strumento come **SQL Server Management Studio** per eseguire lo script e creare le tabelle, come potrebbe aver eseguito una volta creato il `School` database per [la prima esercitazione di serie di esercitazioni Guida introduttiva ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (A meno che non è scaricato il database.)

È ora possibile usare il `AlumniAssociation` il modello di dati nel web pagine esattamente come si utilizza già il `School` modello. Per provare la natura, aggiungere alcuni dati alle tabelle e creare una pagina web che consente di visualizzare i dati.

Utilizzando **Esplora Server**, aggiungere le seguenti righe di `Alumnus` e `Donation` tabelle.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Creare una nuova pagina web denominata *Alumni.aspx* che utilizza il *Site. master* pagina master. Aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Questo codice crea nidificato `GridView` controlli, quella esterna per visualizzare i nomi sostiene e quello interno per visualizzare le date donato e quantità.

Aprire *Alumni.aspx.cs*. Aggiungere un `using` istruzione per i dati di accesso di livello e un gestore per l'outer `GridView` del controllo `RowDataBound` evento:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Associa dati di questo codice interna `GridView` controllare mediante il `Donations` proprietà di navigazione della riga corrente `Alumnus` entità.

Eseguire la pagina.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Nota: questa pagina è incluso nel progetto scaricabile, ma per consentire il funzionamento è necessario creare il database nell'istanza locale di SQL Server Express, il database non è incluso come un *con estensione mdf* file nel *App\_ Dati* cartella.)

Per ulteriori informazioni sull'utilizzo della funzionalità del modello-first di Entity Framework, vedere [Model-First in Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Supporto POCO

Quando si utilizza una metodologia di progettazione basati su dominio, è progettare le classi di dati che rappresentano i dati e il comportamento relativo al dominio aziendale. Queste classi sono indipendenti da qualsiasi tecnologia specifico utilizzato per archiviare (mantenere) dei dati. in altre parole, è necessario *persistenza*. Mancato riconoscimento della persistenza può anche semplificare una classe allo unit test perché il progetto di unit test può usare qualsiasi tecnologia di persistenza è più semplice per il test. Le versioni precedenti di Entity Framework offerto un supporto limitato per mancato riconoscimento della persistenza poiché devono ereditare da classi di entità di `EntityObject` classe e pertanto inclusi una vasta gamma di funzionalità specifiche di Entity Framework.

Entity Framework 4 offre la possibilità di utilizzare le classi di entità che non ereditano la `EntityObject` classe e quindi che non riconoscono la persistenza. Nel contesto di Entity Framework, classi come questo in genere vengono definite *oggetti CLR normale precedente* (POCO o POCOs). È possibile scrivere classi POCO manualmente, oppure è possibile generare automaticamente in base a un modello di dati esistente tramite i modelli di Toolkit di trasformazione di modelli di testo (T4) forniti da Entity Framework.

Per ulteriori informazioni sull'utilizzo di POCOs in Entity Framework, vedere le risorse seguenti:

- [Utilizzo di entità POCO](https://msdn.microsoft.com/library/dd456853.aspx). Si tratta di un documento MSDN che viene fornita una panoramica di POCOs, con collegamenti ad altri documenti che contengono informazioni più dettagliate.
- [Procedura dettagliata: POCO modello per Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) si tratta di un post di blog dal team di sviluppo di Entity Framework, con collegamenti ad altri post di blog sulla POCOs.

## <a name="code-first-development"></a>Sviluppo Code First

Supporto POCO in Entity Framework 4 ancora è necessario creare un modello di dati e collegare le classi di entità nel modello di dati. La prossima versione di Entity Framework includerà una funzionalità denominata *sviluppo di codice-first*. Questa funzionalità consente di usare Entity Framework con le classi POCO senza dover per utilizzare la finestra di progettazione del modello di dati o un file XML del modello di dati. (Di conseguenza, questa opzione anche è stata chiamata *solo codice*; *codice prima* e *solo codice* entrambi si riferiscono alla stessa funzionalità di Entity Framework.)

Per ulteriori informazioni sull'utilizzo l'approccio di codice prima di sviluppo, vedere le risorse seguenti:

- [Sviluppo con Entity Framework 4 Code-First](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Si tratta di un post di blog di sviluppo di codice prima introduzione di Scott Guthrie.
- [Post di blog del Team di sviluppo Framework Entity - tag CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Post di blog del Team di sviluppo Framework Entity - tag Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Esercitazione per l'archivio di musica MVC - parte 4: accesso ai dati e modelli](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Introduzione a MVC 3 - parte 4: lo sviluppo Code First di Entity Framework](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Inoltre, una nuova esercitazione MVC Code-First che consente di creare un'applicazione simile all'applicazione Contoso University è previsto per essere pubblicata in primavera del 2011 alle [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Altre informazioni

In questo passaggio si completa la panoramica per novità di Entity Framework e questa se si continua con la serie di esercitazioni di Entity Framework. Per ulteriori informazioni sulle nuove funzionalità di Entity Framework 4 non sono descritti in questo argomento, vedere le risorse seguenti:

- [Novità in ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) argomento MSDN sulle nuove funzionalità nella versione 4 di Entity Framework.
- [Annuncio del rilascio di Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) post di blog del team di sviluppo di Entity Framework sulle nuove funzionalità nella versione 4.

> [!div class="step-by-step"]
> [Precedente](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
