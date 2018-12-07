---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione di un aggiornamento di SQL Server Database - 11 su 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2018b58207365a0a3829f1f359d46d765aad0a77
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834423"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione di un aggiornamento di SQL Server Database - 11 su 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e illustra come distribuire siti Web di Windows Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come distribuire un aggiornamento del database in un database di SQL Server completo. Poiché le migrazioni Code First esegue tutte le operazioni di aggiornamento del database, il processo è quasi identico a quella usata per SQL Server Compact nel [distribuzione di un aggiornamento del Database](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) esercitazione.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Aggiunta di una nuova colonna a una tabella

In questa sezione dell'esercitazione si apporteranno corrispondenti modifiche al codice, quindi eseguirne il test in Visual Studio in preparazione per la loro distribuzione agli ambienti di test e produzione e un database di modifica. La modifica comporta l'aggiunta di un `OfficeHours` colonna per il `Instructor` entità e le nuove informazioni nella visualizzazione di **instructors (insegnanti)** pagina web.

Nel progetto ContosoUniversity.DAL, aprire *Instructor.cs* e aggiungere la proprietà seguente tra i `HireDate` e `Courses` proprietà:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Aggiornare la classe inizializzatore in modo che esegue il seeding la nuova colonna con dati di test. Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con blocco di codice seguente che include la nuova colonna:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

Nel progetto ContosoUniversity, aprire *Instructors.aspx* e aggiungere un nuovo campo modello per le ore di office subito prima del tag `</Columns>` tag nel primo `GridView` controllo:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Compilare la soluzione.

Aprire il **Console di gestione pacchetti** finestra e ContosoUniversity.DAL selezionare come il **progetto predefinito**.

Immettere i comandi seguenti:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Eseguire l'applicazione e selezionare il **instructors (insegnanti)** pagina. La pagina richiede un po' più tempo del solito caricare, perché Entity Framework ricrea il database ed esegue il seeding con dati di test.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Distribuzione di aggiornamento del Database nell'ambiente di Test

Quando si usa migrazioni Code First, il metodo per la distribuzione di una modifica al database di SQL Server è identico a quello di SQL Server Compact. Tuttavia, è necessario modificare il Test profilo di pubblicazione è ancora stato configurato per eseguire la migrazione da SQL Server Compact a SQL Server.

Il primo passaggio consiste nel rimuovere le trasformazioni di stringa di connessione che è stato creato nell'esercitazione precedente. Questi non sono più necessarie poiché non è possibile specificare le trasformazioni di stringa di connessione nel profilo di pubblicazione, come accadeva prima della configurazione di **pubblicazione/creazione pacchetto SQL** scheda per la migrazione a SQL Server.

Aprire il *Web.Test.config* del file e rimuovere il `connectionStrings` elemento. L'unica trasformazione rimanente nel *Web.Test.config* file è per il `Environment` valore nel `appSettings` elemento.

A questo punto è possibile aggiornare il profilo di pubblicazione e pubblicazione nell'ambiente di test.

Aprire il **pubblica sul Web** procedura guidata e quindi passare al **profilo** scheda.

Selezionare il **Test** profilo di pubblicazione.

Selezionare il **impostazioni** scheda.

Fare clic su **Abilita il nuovo database di pubblicazione miglioramenti**.

Nella casella stringa di connessione per **SchoolContext**, immettere lo stesso valore usato nel *Web.Test.config* file trasformazione nell'esercitazione precedente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Selezionare **Esegui migrazioni Code First (inizia all'avvio dell'applicazione)**. (Nella versione di Visual Studio, la casella di controllo potrebbe essere etichettata **applicare le migrazioni Code First**.)

Nella casella stringa di connessione per **DefaultConnection**, immettere lo stesso valore usato nel *Web.Test.config* file trasformazione nell'esercitazione precedente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Lasciare **aggiornare il database** deselezionata.

Fare clic su **Pubblica**.

Visual Studio consente di distribuire le modifiche al codice nell'ambiente di test e apre il browser alla home page di Contoso University.

Selezionare la pagina instructors (insegnanti).

Quando l'applicazione viene eseguita questa pagina, tenta di accedere al database. Migrazioni Code First controlla se il database corrente e rileva che la migrazione di più recente non è ancora stata applicata. Migrazioni Code First si applica la migrazione di più recente, viene eseguito il `Seed` (metodo) e quindi la pagina viene eseguito normalmente. Visualizzare la nuova colonna orario di ufficio con dati di seeding.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Distribuzione di aggiornamento del Database nell'ambiente di produzione

È necessario modificare anche il profilo di pubblicazione per l'ambiente di produzione. In questo caso si sarà rimuovere il profilo esistente e crearne una nuova importando un file con estensione publishsettings aggiornato. Il file aggiornato includerà la stringa di connessione per il database di SQL Server nel Cytanium.

Come si è visto durante la distribuzione nell'ambiente di test, le trasformazioni di stringa di connessione in non sono più necessari i *Web.Production.config* file di trasformazione. Apertura di file e rimuovere il `connectionStrings` elemento. Le trasformazioni rimanenti sono per la `Environment` valore il `appSettings` elemento e il `location` elemento che limita l'accesso ai report di errore Elmah.

Prima di creare un nuovo profilo di pubblicazione per la produzione, scaricare un file con estensione publishsettings aggiornato allo stesso modo è stato fatto in precedenza nella [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione. (Nel Pannello di controllo Cytanium, fare clic su **siti Web**, quindi fare clic sui **contosouniversity.com** sito Web. Selezionare il **pubblicazione sul Web** scheda e quindi fare clic su **Download Publishing Profile per il sito web**.) Il motivo che si esegue il processo consiste nello scegliere la stringa di connessione di database nel file con estensione publishsettings. La stringa di connessione non era disponibile la prima volta che hai scaricato il file, poiché si sta ancora utilizzando SQL Server Compact e ancora non veniva creato il database di SQL Server in Cytanium.

A questo punto è possibile aggiornare il profilo di pubblicazione e pubblicazione nell'ambiente di produzione.

Aprire il **pubblica sul Web** procedura guidata e quindi passare al **profilo** scheda.

Fare clic su **gestire i profili**e quindi eliminare il profilo di produzione.

Chiudi il **pubblica sul Web** guidata consente di salvare questa modifica.

Aprire il **pubblica sul Web** Creazione guidata nuovo e quindi fare clic su **importazione**.

Nel **Connection** scheda, modificare **URL di destinazione** sul valore appropriato se si usa un URL temporaneo.

Scegliere **Avanti**.

Nel **le impostazioni** scheda, fare clic su **Abilita il nuovo database di pubblicazione miglioramenti**.

Nell'elenco di riepilogo a discesa stringa di connessione per **SchoolContext**, selezionare la stringa di connessione Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Selezionare **migrazioni Code First Execute (inizia all'avvio dell'applicazione)**.

Nell'elenco di riepilogo a discesa stringa di connessione per **DefaultConnection**, selezionare la stringa di connessione Cytanium.

Selezionare il **profilo** scheda, fare clic su **Gestione profili**e rinomina il profilo da "contosouniversity.com - distribuzione Web" a "Production".

Chiudere il profilo di pubblicazione per salvare le modifiche, quindi aprirlo nuovamente.

Fare clic su **Pubblica**. (Per siti Web di produzione reali, copiare *app\_offline.htm* alla produzione e put che nella cartella del progetto prima della pubblicazione, quindi di rimuovere una volta completata la distribuzione.)

Visual Studio consente di distribuire le modifiche al codice nell'ambiente di test e apre il browser alla home page di Contoso University.

Selezionare la pagina instructors (insegnanti).

Migrazioni Code First aggiorna il database allo stesso modo di che quanto accadeva in ambiente di Test. Visualizzare la nuova colonna orario di ufficio con dati di seeding.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Ora aver distribuito un aggiornamento dell'applicazione che include una modifica di database, usando un database di SQL Server.

## <a name="more-information"></a>Altre informazioni

In questo passaggio si completa questa serie di esercitazioni sulla distribuzione di un'applicazione web ASP.NET in un provider di hosting di terze parti. Per altre informazioni su uno qualsiasi degli argomenti trattati in queste esercitazioni, vedere la [mappa del contenuto ASP.NET distribuzione](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) nel sito web MSDN.

## <a name="acknowledgements"></a>Riconoscimenti

Vorrei ringraziare i seguenti persone che hanno apportato contributi significativi per il contenuto di questa serie di esercitazioni:

- [Alberto Poblacion, MVP &amp; MCT, Spagna](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, sviluppo della piattaforma dati MVP, Stati Uniti
- Harsh Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi (Italia)](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Microsoft, sayed Hashimi](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Microsoft, Scott Hunter](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, (ex) Serbia e Montenegro](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
