---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e ASP.NET 4 di Web Form, parte 7 | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET tramite Entity Framework. È l'applicazione di esempio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: cb84f4f3e130fedb3e2f1a17d630767ff65bfa05
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886979"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e form ASP.NET Web 4 - parte 7
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET utilizzando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Utilizzo delle stored procedure

Nell'esercitazione precedente è stato implementato un modello di ereditarietà tabella per gerarchia. In questa esercitazione verrà descritto come utilizzare stored procedure per controllare ulteriormente l'accesso al database.

Entity Framework consente di specificare che utilizzi stored procedure per l'accesso al database. Per qualsiasi tipo di entità, è possibile specificare una stored procedure da utilizzare per la creazione, aggiornamento o l'eliminazione delle entità di quel tipo. Nel modello di dati è quindi possibile aggiungere i riferimenti alle stored procedure che è possibile utilizzare per eseguire attività quali il recupero dei set di entità.

Utilizzo di stored procedure è un requisito comune per l'accesso al database. In alcuni casi, un amministratore del database potrebbe richiedere che l'accesso al database passare attraverso le stored procedure per motivi di sicurezza. In altri casi si desidera compilare una logica di business in alcuni dei processi di Entity Framework viene utilizzato quando aggiorna il database. Ogni volta che viene eliminata un'entità, ad esempio, potrebbe essere desidera copiare in un database di archivio. O ogni volta che viene aggiornata una riga potrebbe voler scrivere una riga in una tabella di registrazione che registra che ha apportato la modifica. È possibile eseguire questi tipi di attività in una stored procedure che viene chiamata ogni volta che Entity Framework consente di eliminare un'entità o aggiorna un'entità.

Come l'esercitazione precedente, si creeranno non tutte le nuove pagine. Al contrario, si modificherà il modo di che Entity Framework accede al database per alcune delle pagine di cui già stato creato.

In questa esercitazione si creerà le stored procedure nel database per l'inserimento di `Student` e `Instructor` entità. Verranno aggiunti al modello di dati e si specificano che Entity Framework devono utilizzarli per l'aggiunta di `Student` e `Instructor` entità al database. Si creerà inoltre una stored procedure che è possibile utilizzare per recuperare `Course` entità.

## <a name="creating-stored-procedures-in-the-database"></a>Creazione di Stored procedure nel Database

(Se si usa il *School. mdf* file dal progetto disponibile per il download in questa esercitazione, è possibile ignorare questa sezione perché esistono già stored procedure.)

In **Esplora Server**, espandere *School. mdf*, fare doppio clic su **Stored procedure**e selezionare **Aggiungi nuova Stored Procedure**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Copiare le istruzioni SQL seguenti e incollarli nella finestra della stored procedure, sostituendo la stored procedure di base.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` le entità sono disponibili quattro proprietà: `PersonID`, `LastName`, `FirstName`, e `EnrollmentDate`. Il database genera il valore ID automaticamente e la stored procedure accetta parametri per le altre tre. La stored procedure restituisce il valore della chiave di record della nuova riga affinché Entity Framework può tenere traccia di che la versione dell'entità che conserva in memoria.

Salvare e chiudere la finestra di stored procedure.

Creare un `InsertInstructor` stored procedure allo stesso modo, tramite le istruzioni SQL seguenti:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Creare `Update` stored procedure per `Student` e `Instructor` entità anche. (Il database contiene già un `DeletePerson` stored procedure che funzionerà per entrambe `Instructor` e `Student` entità.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

In questa esercitazione si saranno eseguire il mapping di queste tre funzioni: insert, update e delete, per ogni tipo di entità. Entity Framework versione 4 consente di eseguire il mapping solo da uno o due di queste funzioni per le stored procedure senza mapping altri con un'unica eccezione: se si associa la funzione di aggiornamento, ma non la funzione di eliminazione, Entity Framework genererà un'eccezione quando si tentativo di eliminare un'entità. In Entity Framework versione 3.5 non è stato questo molto flessibile per eseguire il mapping di stored procedure: se è stato eseguito il mapping di una funzione veniva richiesto di eseguire il mapping di tutte e tre.

Per creare una stored procedure che legge piuttosto che aggiorna i dati, crearne uno che seleziona tutti gli `Course` entità, utilizzando le istruzioni SQL seguenti:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Le Stored procedure di aggiunta al modello di dati

Le stored procedure vengono ora definite nel database, ma devono essere aggiunti al modello di dati per renderli disponibili per Entity Framework. Aprire *SchoolModel*, fare doppio clic nell'area di progettazione e selezionare **il modello di aggiornamento dal Database**. Nel **Aggiungi** scheda della finestra il **Seleziona oggetti di Database** finestra di dialogo espandere **Stored procedure**, selezionare le stored procedure appena create e `DeletePerson` stored procedure e quindi fare clic su **fine**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Mapping di Stored procedure

In Progettazione modelli di dati, fare doppio clic su di `Student` entità e selezionare **Stored Procedure Mapping**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

Il **Dettagli Mapping** viene visualizzata la finestra in cui è possibile specificare una stored procedure che Entity Framework deve utilizzare per l'inserimento, aggiornamento ed eliminazione di entità di questo tipo.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Impostare il **inserire** funzione **InsertStudent**. La finestra viene visualizzato un elenco di parametri di stored procedure, ognuno dei quali deve essere mappato a una proprietà di entità. Due di queste vengono mappate automaticamente perché i nomi sono gli stessi. Nessuna proprietà di entità denominata `FirstName`, pertanto è necessario selezionare manualmente `FirstMidName` da un elenco di riepilogo a discesa che visualizza le proprietà delle entità disponibile. (In questo modo è stato modificato il nome del `FirstName` proprietà `FirstMidName` nella prima esercitazione.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

Nello stesso **Dettagli Mapping** finestra, mappa il `Update` funzione la `UpdateStudent` stored procedure (assicurarsi di specificare `FirstMidName` come valore del parametro per `FirstName`, anche per il `Insert` stored procedure di) e `Delete` funzione la `DeletePerson` stored procedure.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Seguire la stessa procedura per eseguire il mapping di insert, update e delete stored procedure per i docenti per il `Instructor` entità.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Per le stored procedure che leggono anziché aggiornare i dati, utilizzare il **Browser modello** restituisce digitarla finestra per eseguire il mapping di stored procedure per l'entità. In Progettazione modelli di dati, fare doppio clic su area di progettazione e seleziona **Browser modello**. Aprire il **SchoolModel.Store** nodo e quindi aprire il **Stored procedure** nodo. Quindi il `GetCourses` stored procedure e selezionare **Aggiungi funzione importazione**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

Nel **Aggiungi funzione importazione** nella finestra di dialogo **restituisce una raccolta di** selezionare **entità**, quindi selezionare `Course` come il tipo di entità restituito. Al termine, fare clic su **OK**. Salvare e chiudere il *edmx* file.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Utilizza inserimento, aggiornamento ed eliminazione di Stored procedure

Stored procedure per inserire, aggiornare ed eliminare i dati vengono usati da Entity Framework automaticamente dopo aver aggiunte al modello di dati e li mappato alle entità appropriata. È ora possibile eseguire il *StudentsAdd.aspx* pagina e ogni volta che si crea un nuovo studente, Entity Framework userà la `InsertStudent` stored procedure per aggiungere la nuova riga per il `Student` tabella.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Eseguire il *Students.aspx* pagina e il nuovo studente viene visualizzato nell'elenco.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Modificare il nome per verificare che la funzione update funzioni e quindi eliminare gli studenti per verificare che l'eliminazione di funzioni.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Utilizzo di Stored procedure Select

Entity Framework non viene eseguita automaticamente stored procedure, ad esempio `GetCourses`, e non possono essere utilizzati con il `EntityDataSource` controllo. A questo scopo, si chiamarle da codice.

Aprire il *InstructorsCourses.aspx.cs* file. Il `PopulateDropDownLists` metodo utilizza una query LINQ-a-entità per recuperare tutte le entità course in modo che può scorrere l'elenco e determinare quali assegnato a un docente e quali sono non assegnati:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Sostituire con il codice seguente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

La pagina utilizza ora la `GetCourses` stored procedure per recuperare l'elenco di tutti i corsi. Eseguire la pagina per verificarne il funzionamento di quello precedente.

(Le proprietà di navigazione di entità recuperate da una stored procedure potrebbero non venire popolate automaticamente con i dati relativi a tali entità, a seconda `ObjectContext` impostazioni predefinite. Per ulteriori informazioni, vedere [durante il caricamento di oggetti correlati](https://msdn.microsoft.com/library/bb896272.aspx) in MSDN Library.)

Nella prossima esercitazione, si apprenderà come usare la funzionalità di Dynamic Data per renderne più semplice programma e i test di convalida e formattazione regole dati. Anziché specificare le regole di ogni pagina web come stringhe di formato di dati e che sia o meno un campo obbligatorio, è possibile specificare regole di questo tipo nei metadati del modello di dati e vengono applicati automaticamente in ogni pagina.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-8.md)
