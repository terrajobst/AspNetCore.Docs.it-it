---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async e Stored procedure con Entity Framework in un'applicazione ASP.NET MVC | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Code First di Entity Framework 6 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 7412b32ac29179dfa319544781d4c7165c58196b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async e Stored procedure con Entity Framework in un'applicazione MVC ASP.NET
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Visual Studio 2013 e Code First di Entity Framework 6. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nelle esercitazioni precedenti è stato descritto come leggere e aggiornare i dati utilizzando il modello di programmazione sincrona. In questa esercitazione viene visualizzato come implementare il modello di programmazione asincrono. Codice asincrono consente a un'applicazione di prestazioni migliori perché rende un migliore utilizzo delle risorse del server.

In questa esercitazione viene illustrato anche come utilizzare stored procedure per l'inserimento, aggiornamento e le operazioni di eliminazione su un'entità.

Infine, si sarà ridistribuire l'applicazione in Azure, insieme a tutte le modifiche di database che sono stati implementati dopo la prima volta che è stato distribuito.

Le illustrazioni seguenti mostrano alcune delle pagine che verranno utilizzate.

![Pagina reparti](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Creare reparto](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Perché preoccuparsi di codice asincrono

Un server web ha un numero limitato di thread disponibili, e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso. In questo caso, il server non è possibile elaborare nuove richieste fino a quando non verranno liberati i thread. Con il codice sincrono, molti thread può essere vincolato mentre sono in realtà non sono svolgere alcuna operazione perché in attesa di completamento dei / o. Con il codice asincrono, quando un processo è in attesa dei / o di completamento, il thread viene liberato per il server da utilizzare per l'elaborazione di altre richieste. Di conseguenza, codice asincrono consente alle risorse di server da utilizzare in modo più efficiente e il server è abilitato per gestire più traffico senza ritardi.

Nelle versioni precedenti di .NET, scrittura e il test di codice asincrono è complesso, errore soggetto e difficili da eseguire il debug. In .NET 4.5, è pertanto molto più semplice che devono essere scritti in genere codice asincrono a meno che non esista un motivo non a scrittura, test e debug di codice asincrono. Codice asincrono che presenta una piccola quantità di overhead, ma il potenziale miglioramento delle prestazioni in situazioni con traffico ridotto il calo di prestazioni è irrilevante, mentre per le situazioni di traffico elevato, è sostanziale.

Per ulteriori informazioni sulla programmazione asincrona, vedere [supporto asincrono del utilizzare .NET 4.5 per evitare di bloccare le chiamate](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Creare il controller di reparto

Creare un controller di reparto allo stesso modo è stato eseguito il controller precedente, ma questa volta selezionare il **controller asincroni utilizzare** casella di controllo di azioni.

![Scaffolding di controller di reparto](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Le caratteristiche principali seguenti mostrano ciò che è stato aggiunto al codice sincrono per la `Index` metodo per rendere asincrona:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Quattro modifiche sono state applicate per consentire alla query di database di Entity Framework di eseguire in modo asincrono:

- Il metodo è contrassegnato con il `async` (parola chiave), che indica al compilatore di generare i callback per parti del corpo del metodo e per la creazione automatica di `Task<ActionResult>` oggetto restituito.
- Il tipo restituito è stato modificato da `ActionResult` a `Task<ActionResult>`. Il `Task<T>` tipo rappresenta il lavoro in corso con un risultato di tipo `T`.
- Il `await` (parola chiave) è stato applicato a una chiamata al servizio web. Quando il compilatore rileva questa parola chiave, dietro le quinte suddivide il metodo in due parti. La prima parte termina con l'operazione che viene avviato in modo asincrono. La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.
- Versione asincrona del `ToList` è stato chiamato il metodo di estensione.

Perché è la `departments.ToList` istruzione è stata modificata ma non il `departments = db.Departments` istruzione? Il motivo è che solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono. Il `departments = db.Departments` istruzione imposta una query ma non viene eseguita la query fino a quando il `ToList` metodo viene chiamato. Pertanto, solo il `ToList` metodo viene eseguito in modo asincrono.

Nel `Details` (metodo) e `HttpGet` `Edit` e `Delete` metodi, il `Find` metodo è quella che ha causato una query da inviare al database, in modo che sia il metodo che viene eseguito in modo asincrono:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

Nel `Create`, `HttpPost Edit`, e `DeleteConfirmed` metodi, è il `SaveChanges` chiamata al metodo che fa sì che un comando da eseguire, non le istruzioni, ad esempio `db.Departments.Add(department)` pertanto solo le entità in memoria da modificare.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Aprire *Views\Department\Index.cshtml*e sostituire il codice del modello con il codice seguente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Questo codice viene modificato il titolo dall'indice di reparti, passa il nome dell'amministratore a destra e fornisce il nome completo dell'amministratore.

Nella creazione, eliminazione, i dettagli e modificare le visualizzazioni, modificare la didascalia per il `InstructorID` campo "Administrator" esattamente il campo nome di reparto è stato modificato in "Department" nelle viste del corso.

Nella creazione e modifica viste il codice seguente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Nelle visualizzazioni Delete e i dettagli, utilizzare il codice seguente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Eseguire l'applicazione e fare clic su di **reparti** scheda.

![Pagina reparti](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Tutto ciò che funziona esattamente come gli altri controller, ma in questo controller di tutte le query SQL sono in esecuzione in modo asincrono.

Alcuni aspetti da tenere in considerazione quando si utilizza la programmazione asincrona con Entity Framework:

- Il codice asincrono non è thread-safe. In altre parole, in altre parole, non tenta di eseguire più operazioni in parallelo utilizzando la stessa istanza di contesto.
- Se si desidera sfruttare i vantaggi delle prestazioni del codice asincrono, assicurarsi che qualsiasi libreria di pacchetti in uso (ad esempio per il paging), usano anche il metodo async se esse chiamano metodi di Entity Framework che vengono generate query da inviare al database.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Utilizzare le stored procedure per l'inserimento, aggiornamento ed eliminazione

Alcuni sviluppatori e amministratori preferiscono utilizzare stored procedure per l'accesso al database. Nelle versioni precedenti di Entity Framework è possibile recuperare i dati tramite una stored procedure da [l'esecuzione di una query SQL non elaborata](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ma non è possibile indicare a EF utilizzare stored procedure per le operazioni di aggiornamento. In Entity Framework 6 è facile da configurare Code First per l'utilizzo di stored procedure.

1. In *DAL\SchoolContext.cs*, aggiungere il codice evidenziato per il `OnModelCreating` metodo.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Questo codice indica a Entity Framework per utilizzare stored procedure per l'inserimento, aggiornamento ed eliminazione di operazioni sul `Department` entità.
2. Nella Console di gestione pacchetti, immettere il comando seguente:

    `add-migration DepartmentSP`

    Aprire *migrazioni\&lt; timestamp&gt;\_DepartmentSP.cs* per visualizzare il codice di `Up` metodo che crea Insert, Update e Delete stored procedure:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
- Nella Console di gestione pacchetti, immettere il comando seguente:

    `update-database`
- Eseguire l'applicazione in modalità di debug, fare clic su di **reparti** scheda e quindi fare clic su **Crea nuovo**.
- Immettere i dati per una nuova categoria e quindi fare clic su **crea**.

    ![Creare reparto](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
- In Visual Studio, esaminare i registri di **Output** finestra per verificare che una stored procedure è stata usata per inserire una nuova riga reparto.

    ![Reparto Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Codice crea prima i nomi delle procedure predefinito archiviato. Se si utilizza un database esistente, si potrebbe essere necessario personalizzare i nomi di stored procedure per utilizzare le stored procedure già definite nel database. Per informazioni su come eseguire questa operazione, vedere [Entity Framework prima Insert/Update/Delete Stored procedure codice](https://msdn.microsoft.com/data/dn468673).

Se si desidera personalizzare cosa generati eseguire stored procedure, è possibile modificare il codice di supporto temporaneo per le migrazioni `Up` metodo che crea la stored procedure. In questo modo le modifiche verranno riflesse ogni volta che la migrazione viene eseguita e verrà applicata al database di produzione durante le migrazioni viene eseguito automaticamente nell'ambiente di produzione dopo la distribuzione.

Se si desidera modificare una stored procedure esistente creato in una migrazione precedente, è possibile utilizzare il comando Add-Migration per generare una migrazione vuota e quindi scrivere manualmente il codice che chiama il [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) (metodo) .

## <a name="deploy-to-azure"></a>Distribuire in Azure

In questa sezione è necessario aver completato l'opzione facoltativa **distribuzione dell'applicazione in Azure** sezione la [migrazioni e distribuzione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione di questa serie. Se si sono verificati errori di migrazioni risolti eliminando il database nel progetto locale, è possibile ignorare questa sezione.

1. In Visual Studio, fare clic sul progetto in **Esplora** e selezionare **pubblica** dal menu di scelta rapida.
2. Fare clic su **Pubblica**.

    Visual Studio distribuisce l'applicazione in Azure e l'applicazione verrà visualizzata nel browser predefinito, in esecuzione in Azure.
3. Testare l'applicazione per verificare il funzionamento.

    La prima volta che si esegue una pagina che accede al database, Entity Framework viene eseguito tutte le migrazioni `Up` metodi necessari per portare il database aggiornato con il modello di dati corrente. È ora possibile utilizzare tutte le pagine web che è stato aggiunto dall'ultima volta che è stato distribuito, incluse le pagine di reparto che è stato aggiunto in questa esercitazione.

## <a name="summary"></a>Riepilogo

In questa esercitazione si è visto come migliorare l'efficienza di server scrivendo codice che esegue in modo asincrono e come utilizzare stored procedure per inserire, aggiornare ed eliminare le operazioni. Nella prossima esercitazione, si noterà come evitare la perdita di dati quando più utenti tentano di modificare lo stesso record contemporaneamente.

Collegamenti ad altre risorse di Entity Framework, vedere il [accesso ai dati ASP.NET - risorse](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Precedente](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Successivo](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
