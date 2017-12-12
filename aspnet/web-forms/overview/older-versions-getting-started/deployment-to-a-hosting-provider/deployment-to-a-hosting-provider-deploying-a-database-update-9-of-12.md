---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Distribuzione di un''applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di un aggiornamento del Database - 9 di 12 | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 44faca6ac2cdb9193f5c7f6b1d7f5a26e6e22248
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di un aggiornamento del Database - 9 di 12
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione di serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire l'App Web di servizio App di Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione è apportare una modifica al database e le modifiche correlate, verificare le modifiche in Visual Studio, quindi distribuire l'aggiornamento in ambienti di test e di produzione.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Aggiunta di una nuova colonna a una tabella

In questa sezione, si aggiunge una colonna di data di nascita per il `Person` classe di base per il `Student` e `Instructor` entità. È quindi necessario aggiornare la pagina che visualizza i dati di istruttore in modo da visualizzare la nuova colonna.

Nel *ContosoUniversity.DAL* progetto, aprire *Person* e aggiungere la proprietà seguente alla fine del `Person` classe (dovrebbero essere presenti due parentesi graffe riportata di seguito):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Aggiornare quindi il metodo di inizializzazione in modo che fornisca un valore per la nuova colonna. Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con blocco di codice seguente, che include informazioni sulla data di nascita:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

Nel progetto ContosoUniversity, aprire *Instructors.aspx* e aggiungere un nuovo modello di campo per visualizzare la data di nascita. Aggiungere il progetto tra quelle per l'assegnazione di office e di data di assunzione:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Se il rientro di codice ottiene sincronizzato, è possibile premere CTRL-K e CTRL + D per riformattare automaticamente il file.)

Compilare la soluzione e quindi aprire il **Package Manager Console** finestra. Assicurarsi che ContosoUniversity.DAL sia ancora selezionato come il **progetto predefinito**.

Nel **Package Manager Console** selezionare **ContosoUniversity.DAL** come il **progetto predefinito**, quindi immettere il comando seguente:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` (classe) e il `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Compilare la soluzione e quindi immettere il comando seguente nel **Package Manager Console** finestra (assicurarsi che sia ancora selezionato il progetto ContosoUniversity.DAL):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Quando il completamento del comando, eseguire l'applicazione e selezionare la pagina istruttori. Quando il caricamento della pagina, vedrai che contiene il nuovo campo Data di nascita.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>L'aggiornamento del Database di distribuzione nell'ambiente di testing

In **Esplora** selezionare il progetto ContosoUniversity.

Nel **Web-pubblicazione con un clic** barra degli strumenti, seleziona il **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**. (Se la barra degli strumenti è disabilitato, selezionare il progetto ContosoUniversity in **Esplora**.)

Visual Studio distribuisce l'applicazione aggiornata e il browser viene visualizzata la pagina home. Eseguire la pagina istruttori per verificare che l'aggiornamento è stato distribuito correttamente. Quando l'applicazione tenta di accedere al database per questa pagina, il primo codice aggiorna lo schema del database e viene eseguito il `Seed` metodo. Quando viene visualizzata la pagina, viene visualizzato il previsto **data di nascita** colonna delle date in essa contenuti.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>L'aggiornamento del Database di distribuzione nell'ambiente di produzione

È ora possibile distribuire nell'ambiente di produzione. L'unica differenza è che si userà *app\_offline.htm* per impedire agli utenti l'accesso al sito e pertanto l'aggiornamento del database mentre si esegue la distribuzione delle modifiche. Per la distribuzione di produzione, eseguire la procedura seguente:

- Caricare il *app\_offline.htm* file per il sito di produzione.
- In Visual Studio, scegliere il profilo di produzione nel **Web-pubblicazione con un clic** barra degli strumenti e fare clic su **pubblica sul Web**.
- Eliminare il *app\_offline.htm* file dal sito di produzione.

> [!NOTE]
> Mentre l'applicazione è in uso nell'ambiente di produzione deve implementare un piano di backup. Ovvero, si deve essere periodicamente la copia il *dell'istituto di istruzione-Prod.sdf* e *aspnet Prod.sdf* file dalla produzione del sito in un percorso di archiviazione sicura e si devono mantenere diverse generazioni di tali backup. Quando si aggiorna il database, è necessario creare una copia di backup da immediatamente prima della modifica. Quindi, se si commette un errore e non individuarla solo dopo aver distribuito nell'ambiente di produzione, sarà comunque in grado di ripristinare il database allo stato che precedente danneggiamento.


Quando Visual Studio apre l'URL della home page nel browser, il *app\_offline.htm* viene visualizzata la pagina. Dopo aver eliminato il *app\_offline.htm* file, è possibile passare alla home page per verificare che l'aggiornamento è stato distribuito correttamente.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Ora aver distribuito un aggiornamento di un'applicazione che include una modifica di database di prova e di produzione. L'esercitazione successiva viene illustrato come eseguire la migrazione del database da SQL Server Compact in SQL Server Express e SQL Server.

>[!div class="step-by-step"]
[Precedente](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Successivo](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
