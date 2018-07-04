---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async e Stored procedure con Entity Framework in un'applicazione ASP.NET MVC | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 5 con Entity Framework 6 Code First e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4c2deb53856d79a52415db48e04ea96111833b02
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381929"
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async e Stored procedure con Entity Framework in un'applicazione ASP.NET MVC
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 5 con Entity Framework 6 Code First e Visual Studio 2013. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nelle esercitazioni precedenti è stato descritto come leggere e aggiornare i dati usando il modello di programmazione sincrona. In questa esercitazione viene illustrato come implementare il modello di programmazione asincrono. Codice asincrono consente a un'applicazione di offrono prestazioni migliori perché rende un migliore utilizzo delle risorse del server.

In questa esercitazione verrà anche illustrato come utilizzare stored procedure di inserimento, aggiornamento e le operazioni di eliminazione su un'entità.

Infine, si sarà ridistribuire l'applicazione in Azure, insieme a tutte le modifiche del database che sono stati implementati dopo la prima volta che è stato distribuito.

Le figure seguenti illustrano alcune delle pagine che verranno usate.

![Pagina Departments](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Creare reparto](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Perché è necessario eseguire codice asincrono

Per un server Web è disponibile un numero limitato di thread e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso. In queste circostanze il server non può elaborare nuove richieste finché i thread non saranno liberi. Con il codice sincrono, può succedere che molti thread siano vincolati nonostante in quel momento non stiano eseguendo alcuna operazione. Rimangono tuttavia in attesa che l'operazione I/O sia completata. Con il codice asincrono, se un processo è in attesa del completamento dell'operazione I/O, il thread viene liberato e il server lo può usare per l'elaborazione di altre richieste. Di conseguenza, codice asincrono consente alle risorse di server da usare in modo più efficiente e il server è abilitato per gestire più traffico senza ritardi.

Nelle versioni precedenti di .NET, la scrittura e test del codice asincrono era complessa, soggetto a errori e difficile eseguire il debug. In .NET 4.5, la scrittura di test e debug del codice asincrono è molto più semplice che è consigliabile scrivere codice asincrono a meno che non si abbia un motivo non. Codice asincrono che presenta una piccola quantità di overhead, ma per le situazioni di traffico ridotto il calo delle prestazioni è irrilevante, mentre per le situazioni di traffico elevato, il potenziale miglioramento delle prestazioni è notevole.

Per altre informazioni sulla programmazione asincrona, vedere [supporto di usare .NET 4.5 asincrono per evitare di bloccare le chiamate](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Creare il controller di reparto

Creare un controller Department allo stesso modo è stato eseguito il precedente controller, ma questa volta selezionare il **controller asincrono Usa** casella di controllo di azioni.

![Scaffolding di controller Department](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Le caratteristiche principali seguenti illustrano ciò che è stato aggiunto al codice sincrono per il `Index` metodo per renderla asincrona:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Quattro modifiche sono state applicate per consentire alla query di database di Entity Framework di eseguire in modo asincrono:

- Il metodo è contrassegnato con il `async` parola chiave, che indica al compilatore di generare callback per parti del corpo del metodo e per la creazione automatica di `Task<ActionResult>` oggetto restituito.
- Il tipo restituito è stato modificato da `ActionResult` a `Task<ActionResult>`. Il `Task<T>` tipo rappresenta il lavoro in corso con un risultato di tipo `T`.
- Il `await` è stata applicata la parola chiave per la chiamata al servizio web. Quando il compilatore rileva questa parola chiave, dietro le quinte suddivide il metodo in due parti. La prima parte termina con l'operazione che viene avviato in modo asincrono. La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.
- La versione asincrona del `ToList` è stato chiamato il metodo di estensione.

Il motivo per cui è il `departments.ToList` istruzione modificata ma non la `departments = db.Departments` istruzione? Il motivo è che solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono. Il `departments = db.Departments` istruzione imposta una query, ma finché non viene eseguita la query di `ToList` viene chiamato il metodo. Pertanto, solo il `ToList` metodo viene eseguito in modo asincrono.

Nel `Details` metodo e il `HttpGet` `Edit` e `Delete` metodi, il `Find` metodo è quello che fa sì che una query da inviare al database, in modo che il metodo che viene eseguito in modo asincrono:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

Nel `Create`, `HttpPost Edit`, e `DeleteConfirmed` metodi, è il `SaveChanges` chiamata al metodo che fa sì che un comando da eseguire, non le istruzioni, ad esempio `db.Departments.Add(department)` che causano solo le entità in memoria da modificare.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Aprire *Views\Department\Index.cshtml*e sostituire il codice del modello con il codice seguente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Questo codice imposta il titolo dall'indice in reparti, sposta il nome dell'amministratore a destra e fornisce il nome completo dell'amministratore.

Nella creazione, eliminazione, dettagli e modificare le viste, modificare la didascalia per il `InstructorID` campo su "Administrator" simile a quello è modificato il campo del nome reparto "Department" le visualizzazioni dei corsi.

Nella creazione e modifica le visualizzazioni usano il codice seguente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Nelle visualizzazioni Delete e dettagli usare il codice seguente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Eseguire l'applicazione e scegliere il **reparti** scheda.

![Pagina Departments](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Tutto funziona esattamente come in altri controller, ma in questo controller di tutte le query SQL sono in esecuzione in modo asincrono.

Alcuni aspetti da tenere presenti quando si usa la programmazione asincrona con Entity Framework:

- Il codice asincrono non è thread-safe. In altre parole, in altre parole, non tentare di eseguire più operazioni in parallelo usando la stessa istanza di contesto.
- Se si vogliono sfruttare i vantaggi del codice asincrono in termini di prestazioni, verificare che i pacchetti della libreria impiegati, ad esempio per il paging, usino la modalità asincrona per chiamare i metodi di Entity Framework che generano query da inviare al database.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Utilizzare le stored procedure di inserimento, aggiornamento ed eliminazione

Alcuni sviluppatori e amministratori di database preferiscono usare stored procedure per l'accesso al database. Nelle versioni precedenti di Entity Framework è possibile recuperare i dati utilizzando una stored procedure dal [l'esecuzione di una query SQL non elaborata](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ma è non è possibile indicare a Entity Framework per utilizzare le stored procedure per le operazioni di aggiornamento. In EF 6 è semplice da configurare Code First per utilizzare le stored procedure.

1. Nelle *DAL\SchoolContext.cs*, aggiungere il codice evidenziato per il `OnModelCreating` (metodo).

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Questo codice indica a Entity Framework per utilizzare stored procedure per l'inserimento, aggiornamento ed eliminazione di operazioni su di `Department` entità.
2. Nella Console di gestione pacchetti, immettere il comando seguente:

    `add-migration DepartmentSP`

    Aprire *migrazioni\&lt; timestamp&gt;\_DepartmentSP.cs* per visualizzare il codice nel `Up` metodo che crea Insert, Update e Delete stored procedure:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. Nella Console di gestione pacchetti, immettere il comando seguente:

     `update-database`
4. Esegue l'applicazione in modalità di debug, scegliere il **reparti** scheda e quindi fare clic su **Crea nuovo**.
5. Immettere i dati per un nuovo reparto e quindi fare clic su **Create**.

     ![Creare reparto](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. Esaminare i log in Visual Studio, il **Output** finestra per verificare che una stored procedure è stata usata per inserire la nuova riga di reparto.

     ![Reparto Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Codice crea prima di tutto i nomi di procedura predefinita memorizzata. Se si usa un database esistente, si potrebbe essere necessario personalizzare i nomi di stored procedure per utilizzare le stored procedure già definite nel database. Per informazioni su come eseguire questa operazione, vedere [Entity Framework Code prima Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).

Se si desidera personalizzare cosa generato eseguire stored procedure, è possibile modificare il codice con scaffolding per le migrazioni `Up` metodo che crea la stored procedure. In questo modo le modifiche verranno riflesse ogni volta che la migrazione viene eseguita e verrà applicata al database di produzione quando migrazioni viene eseguito automaticamente nell'ambiente di produzione dopo la distribuzione.

Se si desidera modificare una stored procedure esistente che è stata creata in una migrazione precedente, è possibile usare il comando Add-Migration per generare una migrazione vuota e quindi scrivere manualmente il codice che chiama il [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) (metodo) .

## <a name="deploy-to-azure"></a>Distribuire in Azure

In questa sezione è necessario aver completato l'opzione facoltativa **la distribuzione dell'app in Azure** sezione il [migrazioni e la distribuzione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione della serie. Se si sono verificati errori di migrazione che è stato risolto, eliminare il database nel progetto locale, ignorare questa sezione.

1. In Visual Studio, fare clic sul progetto in **Esplora soluzioni** e selezionare **Publish** dal menu di scelta rapida.
2. Fare clic su **Pubblica**.

    Visual Studio distribuisce l'applicazione in Azure e l'applicazione viene aperta nel browser predefinito, in esecuzione in Azure.
3. Testare l'applicazione per verificare funzioni.

    Alla prima esecuzione di una pagina che accede al database, Entity Framework esegue tutte le migrazioni `Up` metodi necessari per portare il database aggiornato con il modello di dati corrente. È ora possibile usare tutte le pagine web che è stato aggiunto dopo l'ultima che è stata distribuita, incluse le pagine di reparto che è stato aggiunto in questa esercitazione.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come migliorare l'efficienza di server scrivendo codice che esegue in modo asincrono e su come usare stored procedure per inserire, aggiornare ed eliminare le operazioni. Nella prossima esercitazione, si noterà come impedire la perdita di dati quando più utenti tentano di modificare lo stesso record nello stesso momento.

Sono disponibili collegamenti ad altre risorse di Entity Framework nel [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
