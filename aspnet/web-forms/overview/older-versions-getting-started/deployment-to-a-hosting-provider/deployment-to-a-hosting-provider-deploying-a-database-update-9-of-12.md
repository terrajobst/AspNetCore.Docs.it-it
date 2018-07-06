---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione di un aggiornamento di Database - 9 pari a 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5edb3cae785d578674f47f1aa3d5be7dfa6a8791
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822095"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione di un aggiornamento di Database - 9 pari a 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e Mostra come distribuire in App Web di servizio App di Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione, si apportano una modifica al database e le modifiche al codice correlato, testare le modifiche in Visual Studio, quindi distribuire l'aggiornamento in ambienti di test e di produzione.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Aggiunta di una nuova colonna a una tabella

In questa sezione si aggiunge una colonna di date di nascita per il `Person` classe di base per il `Student` e `Instructor` entità. Quindi necessario aggiornare la pagina che visualizza i dati di instructor in modo da visualizzare la nuova colonna.

Nel *ContosoUniversity.DAL* progetto aprire *Person.cs* e aggiungere la proprietà seguente alla fine del `Person` classe (dovrebbero essere presenti due parentesi graffe riportata di seguito):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Aggiornare quindi il metodo di inizializzazione in modo che fornisca un valore per la nuova colonna. Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con blocco di codice seguente che include informazioni sulla data di nascita:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

Nel progetto ContosoUniversity, aprire *Instructors.aspx* e aggiungere un nuovo modello di campo per visualizzare la data di nascita. Aggiungerlo tra quelle per l'assegnazione di office e data di assunzione:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Se rientro del codice viene sincronizzato, è possibile premere CTRL-K e CTRL + D per riformattare automaticamente il file.)

Compilare la soluzione e quindi aprire il **Console di gestione pacchetti** finestra. Assicurarsi che ContosoUniversity.DAL sia ancora selezionato come le **progetto predefinito**.

Nel **Console di gestione pacchetti** finestra, seleziona **ContosoUniversity.DAL** come il **progetto predefinito**, quindi immettere il comando seguente:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` (classe) e nel `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Compilare la soluzione e quindi immettere il comando seguente nel **Console di gestione pacchetti** finestra (assicurarsi che il progetto ContosoUniversity.DAL sia ancora selezionato):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Quando il completamento del comando, eseguire l'applicazione e selezionare la pagina instructors (insegnanti). Quando la pagina viene caricata, si noterà che che contiene il nuovo campo di data di nascita.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Distribuzione di aggiornamento del Database nell'ambiente di Test

Nelle **Esplora soluzioni** selezionare il progetto ContosoUniversity.

Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, seleziona la **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**. (Se la barra degli strumenti è disabilitata, selezionare il progetto ContosoUniversity **Esplora soluzioni**.)

Visual Studio distribuisce l'applicazione aggiornata e nel browser verrà aperta la home page. Eseguire la pagina instructors (insegnanti) per verificare che l'aggiornamento è stato distribuito correttamente. Quando l'applicazione prova ad accedere al database per questa pagina, Code First Aggiorna lo schema del database ed esegue il `Seed` (metodo). Quando viene visualizzata la pagina, noterete che quella prevista **data di nascita** colonna delle date in esso.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Distribuzione di aggiornamento del Database nell'ambiente di produzione

È ora possibile distribuire nell'ambiente di produzione. L'unica differenza è che si userà *app\_offline.htm* per impedire agli utenti di accedere al sito e in questo modo l'aggiornamento del database mentre si esegue la distribuzione delle modifiche. Per la distribuzione di produzione, seguire i passaggi seguenti:

- Caricare il *app\_offline.htm* file nel sito di produzione.
- In Visual Studio, scegliere il profilo di produzione nel **Web-pubblicazione con un clic** sulla barra degli strumenti e fare clic su **pubblica sul Web**.
- Eliminare il *app\_offline.htm* file dal sito di produzione.

> [!NOTE]
> Mentre l'applicazione è in uso nell'ambiente di produzione si deve implementare un piano di backup. Vale a dire, si dovrebbe essere periodicamente la copia di *School-Prod.sdf* e *aspnet-Prod.sdf* file dalla produzione del sito in un percorso di archiviazione protetta e si devono mantenere varie generazioni di tali backup. Quando si aggiorna il database, è necessario eseguire una copia di backup da immediatamente prima della modifica. Quindi, se si commette un errore e non individua solo dopo aver distribuito nell'ambiente di produzione, è comunque sarà in grado di ripristinare il database allo stato che precedente danneggiamento.


Quando Visual Studio si apre l'URL della home page nel browser, il *app\_offline.htm* viene visualizzata la pagina. Dopo aver eliminato il *app\_offline.htm* file, è possibile passare a home page per verificare che l'aggiornamento è stato distribuito correttamente.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

È stato distribuito un aggiornamento dell'applicazione che include una modifica di database per i test e produzione. L'esercitazione successiva illustra come eseguire la migrazione del database da SQL Server Compact a SQL Server Express e SQL Server.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
