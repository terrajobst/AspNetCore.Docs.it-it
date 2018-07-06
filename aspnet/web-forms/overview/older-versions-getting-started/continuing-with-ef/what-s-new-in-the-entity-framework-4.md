---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: What ' s New in Entity Framework 4.0 | Microsoft Docs
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione web di Contoso University specificano che viene creato da Getting Started with serie di esercitazioni in Entity Framework 4.0. POSSO...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 960aaf7aa7c2cff5c51079ecfe50031fa82d5508
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827841"
---
<a name="whats-new-in-the-entity-framework-40"></a>What ' s New in Entity Framework 4.0
====================
da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web Contoso University specificano che viene creato per il [Introduzione a Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie di esercitazioni. Se si non è stato completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) verrebbe creato. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creato dalla serie di esercitazioni complete. Se si hanno domande sulle esercitazioni, è possibile pubblicarli per i [forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Nell'esercitazione precedente si è visto alcuni metodi per ottimizzare le prestazioni di un'applicazione web che usa Entity Framework. Questa esercitazione esamina alcune delle nuove funzionalità più importanti nella versione 4 di Entity Framework ed è collegato a risorse che forniscono un'introduzione più completa per tutte le nuove funzionalità. Le funzionalità evidenziate in questa esercitazione includono quanto segue:

- Associazioni di chiavi esterne.
- L'esecuzione di comandi SQL definite dall'utente.
- Sviluppo Model-first.
- Supporto POCO.

Inoltre, l'esercitazione presenteranno brevemente *lo sviluppo code first*, una funzionalità che provenga nella prossima versione di Entity Framework.

Per avviare l'esercitazione, avviare Visual Studio e aprire l'applicazione web di Contoso University specificano che si stava lavorando nell'esercitazione precedente.

## <a name="foreign-key-associations"></a>Associazioni di chiavi esterne

Nella versione 3.5 di Entity Framework sono incluse le proprietà di navigazione, ma non include le proprietà di chiave esterna nel modello di dati. Ad esempio, il `CourseID` e `StudentID` le colonne del `StudentGrade` verrebbe omesso dalla tabella il `StudentGrade` entità.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Il motivo di questo approccio è che, in teoria, le chiavi esterne sono un dettaglio di implementazione fisica e non appartengono a un modello di dati concettuale. Tuttavia, per praticità, è spesso più facile lavorare con le entità nel codice quando si ha accesso diretto alle chiavi esterne.

Per un esempio di chiavi esterne come nel modello di dati può semplificare il codice, prendere in considerazione come sarebbe al codice il *DepartmentsAdd.aspx* pagina senza di essi. Nel `Department` entità, il `Administrator` proprietà è una chiave esterna che corrisponde a `PersonID` nel `Person` entità. Per stabilire l'associazione tra un nuovo reparto e amministratore, è stato sufficiente è stato impostato il valore per il `Administrator` proprietà nel `ItemInserting` gestore dell'evento di controllo con associazione a dati:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Senza chiavi esterne nel modello di dati, è necessario gestire il `Inserting` eventi del controllo origine dati anziché il `ItemInserting` eventi del controllo con associazione a dati, per ottenere un riferimento all'entità stessa prima che l'entità viene aggiunta al set di entità. Quando si dispone di tale riferimento, è necessario stabilire l'associazione usando codice simile negli esempi seguenti:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Come si può notare del team di Entity Framework [post di blog in associazioni di chiave esterna](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), esistono altri casi in cui la differenza tra la complessità del codice è molto più elevata. Per soddisfare le esigenze di quelli che preferiscono in tempo reale con i dettagli di implementazione del modello di dati concettuale ai fini di semplificare il codice, Entity Framework offre ora la possibilità di includere chiavi esterne nel modello di dati.

Nella terminologia di Entity Framework, se si includono le chiavi esterne nel modello di dati si usa *associazioni di chiavi esterne*, e se si esclude le chiavi esterne in uso *associazioni indipendenti*.

## <a name="executing-user-defined-sql-commands"></a>L'esecuzione di comandi SQL definite dall'utente

Nelle versioni precedenti di Entity Framework, si è verificato alcun approccio facile per creare i propri comandi SQL in tempo reale ed eseguirle. Entity Framework generati in modo dinamico i comandi SQL per l'utente oppure era necessario creare una stored procedure e importarlo come una funzione. Versione 4 è stato aggiunto `ExecuteStoreQuery` e `ExecuteStoreCommand` metodi di `ObjectContext` classe che rendono più semplice per poter passare tutte le query direttamente al database.

Si supponga che gli amministratori di Contoso University vogliono essere in grado di eseguire modifiche di massa nel database senza dover passare attraverso il processo di creazione di una stored procedure e importandoli nel modello di dati. La prima richiesta è per una pagina che consente di modificare il numero di crediti per tutti i corsi nel database. Nella pagina web, desiderano essere in grado di immettere un numero da usare per moltiplicare il valore di ogni `Course` della riga `Credits` colonna.

Creare una nuova pagina che utilizza il *Site. master* pagina master e denominarlo *UpdateCredits.aspx*. Quindi aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Questo codice crea un `TextBox` controllo in cui l'utente può immettere il valore del moltiplicatore, una `Button` controllo fare clic per eseguire il comando e un `Label` controllo per indicare il numero di righe interessate.

Aprire *UpdateCredits.aspx.cs*e aggiungere quanto segue `using` informativa e un gestore per il pulsante `Click` evento:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Questo codice esegue il SQL `Update` utilizzando il valore nella casella di testo di comando e Usa l'etichetta per visualizzare il numero di righe interessate. Prima di eseguire la pagina, eseguire la *Courses.aspx* pagina per ottenere una panoramica "precedenti" di alcuni dati.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Eseguire *UpdateCredits.aspx*, immettere "10" come moltiplicatore e quindi fare clic su **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Eseguire la *Courses.aspx* pagina nuovamente per visualizzare i dati modificati.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Se si desidera impostare il numero di crediti sui valori originali, in *UpdateCredits.aspx.cs* cambiare `Credits * {0}` a `Credits / {0}` ed eseguire di nuovo la pagina, immettere 10 del divisore.)

Per altre informazioni sull'esecuzione di query definiti nel codice, vedere [procedura: direttamente eseguire comandi con l'origine dati](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Sviluppo Model-First

In queste procedure dettagliate è stato creato prima di tutto il database e quindi generato il modello di dati basato sulla struttura di database. In Entity Framework 4 è possibile avviare invece con il modello di dati e generare il database basato sulla struttura del modello dati. Se si sta creando un'applicazione per cui il database non esiste già, l'approccio model-first consente di creare entità e relazioni significativi a livello concettuale per l'applicazione, mentre non doversi preoccupare dei dettagli di implementazione fisica . (Questa situazione si verifica solo attraverso le fasi iniziali dello sviluppo, tuttavia. Infine il database verrà creato e avrà i dati di produzione in esso e ricrearla dal modello non potrà più pratico; a questo punto si sarà nuovamente all'approccio database-first.)

In questa sezione dell'esercitazione, si sarà creare un semplice modello di dati e generare il database da quest'ultimo.

Nelle **Esplora soluzioni**, fare doppio clic il *DAL* cartella e selezionare **Aggiungi nuovo elemento**. Nel **Aggiungi nuovo elemento** nella finestra di dialogo **modelli installati** seleziona **Data** e quindi selezionare il **ADO.NET Entity Data Model** modello . Denominare il nuovo file *AlumniAssociationModel.edmx* e fare clic su **Add**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Verrà avviata la procedura guidata Entity Data Model. Nel **Scegli contenuto Model** passaggio, seleziona **modello vuoto** e quindi fare clic su **fine**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

Il **Entity Data Model Designer** viene aperto con un'area di progettazione vuoto. Trascinare un' **Entity** articolo dalle **della casella degli strumenti** nell'area di progettazione.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Modificare il nome dell'entità da `Entity1` al `Alumnus`, modificare il `Id` nome della proprietà `AlumnusId`e aggiungere una nuova proprietà scalare denominata `Name`. Per aggiungere nuove proprietà è possibile premere INVIO dopo la modifica del nome del `Id` colonna, o l'entità e scegliere **Aggiungi proprietà scalari**. Il tipo predefinito per le nuove proprietà viene `String`, che è corretto per questa dimostrazione semplice, ma naturalmente è possibile modificare elementi come tipo di dati nel **proprietà** finestra.

Creare un'altra entità allo stesso modo e denominarla `Donation`. Modifica il `Id` proprietà `DonationId` e aggiungere una proprietà scalare denominata `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Per aggiungere un'associazione tra queste due entità, fare doppio clic sui `Alumnus` entità, selezionare **Add**, quindi selezionare **associazione**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

I valori predefiniti in di **Aggiungi associazione** finestra di dialogo siano quelle corrette (uno-a-molti, includere le proprietà di navigazione, includere chiavi esterne), quindi fare clic sul **OK**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

La finestra di progettazione aggiunge una linea di associazione e una proprietà di chiave esterna.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

A questo punto si è pronti per creare il database. Fare doppio clic su area di progettazione e seleziona **genera Database da modello**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Verrà avviata la procedura guidata Crea Database. (Se vengono visualizzati avvisi che indicano che non sono mappate le entità, è possibile ignorare quelli per il momento.)

Nel **Seleziona connessione dati** passaggio, fare clic su **nuova connessione**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

Nel **proprietà connessione** finestra di dialogo casella, selezionare l'istanza locale di SQL Server Express e il nome del database `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Fare clic su **Sì** quando viene chiesto se si desidera creare il database. Quando la **Seleziona connessione dati** passaggio viene visualizzato anche in questo caso, fare clic su **successivo**.

Nel **riepilogo e impostazioni** passaggio, fare clic su **fine**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Oggetto *con estensione SQL* file con i comandi data definition language (DDL) viene creato, ma i comandi non sono stati ancora in esecuzione.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Usare uno strumento, ad esempio **SQL Server Management Studio** per eseguire lo script e creare le tabelle, perché è già presente quando è stato creato il `School` per il database [la prima esercitazione di introduzione a serie di esercitazioni in ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (A meno che non è scaricato il database.)

È ora possibile usare la `AlumniAssociation` modello di dati in web pages esattamente sia stata usata la `School` modello. Per provare questo timeout, aggiungere alcuni dati alle tabelle e creare una pagina web che consente di visualizzare i dati.

Usando **Esplora Server**, aggiungere le righe seguenti per il `Alumnus` e `Donation` tabelle.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Creare una nuova pagina web denominata *Alumni.aspx* che usa le *Site. master* pagina master. Aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Questo markup crea nidificato `GridView` controlla, quello più esterno per visualizzare i nomi alunni e quello interno per visualizzare una donazione date e importi.

Aprire *Alumni.aspx.cs*. Aggiungere un `using` istruzione per i dati di accesso di livello e un gestore per l'outer `GridView` del controllo `RowDataBound` evento:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Questo codice associa interna `GridView` controllo usando il `Donations` proprietà di navigazione della riga corrente `Alumnus` entità.

Eseguire la pagina.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Nota: questa pagina è incluso nel progetto scaricabile, ma per consentire il funzionamento è necessario creare il database nell'istanza locale di SQL Server Express, il database non è incluso come un *mdf* del file nei *App\_ Dati* cartella.)

Per altre informazioni sull'utilizzo della funzionalità model-first di Entity Framework, vedere [Model-First in Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Supporto POCO

Quando si usa la metodologia di design basato su dominio, è progettare le classi di dati che rappresentano i dati e il comportamento che è rilevante per il dominio aziendale. Queste classi devono essere indipendenti di alcuna tecnologia specifica utilizzata per archiviare (rendere persistente) i dati. in altre parole, deve trattarsi *non riconoscono la persistenza*. Mancato riconoscimento della persistenza può anche semplificare una classe di unit test perché il progetto di unit test può usare qualsiasi tecnologia di persistenza è più pratica per i test. Le versioni precedenti di Entity Framework offerto un supporto limitato per mancato riconoscimento della persistenza poiché le classi di entità da cui ereditare il `EntityObject` classe e quindi inclusi una vasta gamma di funzionalità specifiche di Entity Framework.

Entity Framework 4 introduce la possibilità di usare le classi di entità che non ereditano dal `EntityObject` classe e quindi che non riconoscono la persistenza. Nel contesto di Entity Framework, le classi come segue in genere vengono definite *plain-old CLR Object* (POCO o poco). È possibile scrivere classi POCO manualmente, oppure è possibile generare automaticamente in base a un modello di dati esistente tramite modelli di Toolkit di trasformazione di modelli di testo (T4) forniti da Entity Framework.

Per altre informazioni sull'uso di oggetti poco in Entity Framework, vedere le risorse seguenti:

- [Utilizzano entità POCO](https://msdn.microsoft.com/library/dd456853.aspx). Si tratta di un documento MSDN che viene fornita una panoramica di oggetti poco, con collegamenti ad altri documenti che contengono informazioni più dettagliate.
- [Procedura dettagliata: POCO modello per Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) si tratta di un post di blog del team di sviluppo di Entity Framework, con collegamenti ad altri post di blog su poco.

## <a name="code-first-development"></a>Sviluppo Code First

Supporto POCO in Entity Framework 4 è necessario comunque creare un modello di dati e collegare le classi di entità nel modello di dati. La prossima versione di Entity Framework includerà una funzionalità denominata *lo sviluppo code first*. Questa funzionalità consente di usare Entity Framework con classi POCO personalizzate senza dover usare la finestra di progettazione modello di dati o un file XML del modello di dati. (Di conseguenza, questa opzione anche EnableNotification *solo codice*; *code first* e *solo codice* entrambi si riferiscono alla stessa funzionalità di Entity Framework.)

Per altre informazioni sull'uso l'approccio code first per lo sviluppo, vedere le risorse seguenti:

- [Code First lo sviluppo con Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Si tratta di un post di blog di sviluppo code first presentazione di Scott Guthrie.
- [Blog del Team di sviluppo Framework entità - post con tag CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Blog del Team di sviluppo Framework entità - post con tag Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Esercitazione su MVC Music Store - parte 4: accesso ai dati e modelli](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Introduzione a MVC 3 - parte 4: lo sviluppo Code First di Entity Framework](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Inoltre, una nuova esercitazione MVC Code-First che consente di creare un'applicazione simile all'applicazione Contoso University viene proiettata da pubblicare nella primavera del 2011 in [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Altre informazioni

Questo passaggio completa la panoramica di What ' s new in Entity Framework e questa se si continua con la serie di esercitazioni di Entity Framework. Per altre informazioni sulle nuove funzionalità di Entity Framework 4 che non sono trattati in questo caso, vedere le risorse seguenti:

- [Novità in ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) argomento di MSDN sulla nuova funzionalità nella versione 4 di Entity Framework.
- [Annuncio della versione di Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) post di blog del team di sviluppo di Entity Framework sulle nuove funzionalità nella versione 4.

> [!div class="step-by-step"]
> [Precedente](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
