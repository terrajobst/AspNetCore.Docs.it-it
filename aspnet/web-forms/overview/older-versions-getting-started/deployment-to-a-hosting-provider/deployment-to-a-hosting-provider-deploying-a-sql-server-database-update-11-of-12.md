---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Distribuzione di un''applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di un aggiornamento di SQL Server Database - 11 12 | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: aeec69c7373a111d30e8f32a374a9f02fb4c080a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di un aggiornamento di SQL Server Database - 11 12
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione di serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire siti Web di Windows Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come distribuire un aggiornamento del database in un database di SQL Server completo. Poiché le migrazioni Code First esegue tutte le operazioni di aggiornamento del database, il processo è quasi identico a quella eseguita per SQL Server Compact nel [un aggiornamento del Database di distribuzione](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) esercitazione.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Aggiunta di una nuova colonna a una tabella

In questa sezione dell'esercitazione che si verrà modificato un database e i corrispondenti alle modifiche al codice, quindi eseguirne il test in Visual Studio in preparazione per la distribuzione per gli ambienti di test e produzione. La modifica comporta l'aggiunta di un `OfficeHours` colonna per il `Instructor` entità e le nuove informazioni nella visualizzazione di **i docenti** pagina web.

Nel progetto ContosoUniversity.DAL, aprire *Instructor.cs* e aggiungere la seguente proprietà tra il `HireDate` e `Courses` proprietà:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Aggiornare la classe di inizializzatore in modo che la nuova colonna con dati di test esegue il seeding. Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con blocco di codice seguente, che include la nuova colonna:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

Nel progetto ContosoUniversity, aprire *Instructors.aspx* e aggiungere un nuovo campo di modello per l'orario di ufficio appena prima della chiusura `</Columns>` tag nel primo `GridView` controllo:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Compilare la soluzione.

Aprire il **Package Manager Console** finestra e selezionare ContosoUniversity.DAL come il **progetto predefinito**.

Immettere i comandi seguenti:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Eseguire l'applicazione e selezionare il **i docenti** pagina. La pagina richiede un po' più tempo del solito da caricare, poiché Entity Framework consente di ricreare il database e si esegue il seeding con dati di test.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>L'aggiornamento del Database di distribuzione nell'ambiente di testing

Quando si utilizza migrazioni Code First, il metodo per la distribuzione di una modifica al database di SQL Server è la stessa di SQL Server Compact. Tuttavia, è necessario modificare il Test di profilo di pubblicazione, perché è ancora stato impostato per la migrazione da SQL Server Compact a SQL Server.

Il primo passaggio consiste nel rimuovere le trasformazioni di stringa di connessione che è stato creato nell'esercitazione precedente. Questi non sono più necessarie poiché è necessario specificare le trasformazioni di stringa di connessione nel profilo di pubblicazione, seguendo la prima è stato configurato il **pubblicazione/creazione pacchetto SQL** scheda per la migrazione a SQL Server.

Aprire il *Web.Test.config* file e rimuovere il `connectionStrings` elemento. La trasformazione rimanente sola nel *Web.Test.config* file è per il `Environment` valore il `appSettings` elemento.

È ora possibile aggiornare il profilo di pubblicazione e la pubblicazione nell'ambiente di testing.

Aprire il **pubblica sul Web** procedura guidata, quindi passi al **profilo** scheda.

Selezionare il **Test** profilo di pubblicazione.

Selezionare il **impostazioni** scheda.

Fare clic su **attivare il nuovo database di pubblicazione miglioramenti**.

Nella casella stringa di connessione per **SchoolContext**, immettere lo stesso valore utilizzato nel *Web.Test.config* file di trasformazione nell'esercitazione precedente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Selezionare **eseguire migrazioni Code First (esecuzione all'avvio dell'applicazione)**. (Nella versione di Visual Studio, la casella di controllo potrebbe essere denominata **applicare le migrazioni Code First**.)

Nella casella stringa di connessione per **DefaultConnection**, immettere lo stesso valore utilizzato nel *Web.Test.config* file di trasformazione nell'esercitazione precedente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Lasciare **Aggiorna database** deselezionata.

Fare clic su **Pubblica**.

Visual Studio consente di distribuire le modifiche al codice nell'ambiente di testing e apre il browser alla home page di Contoso University.

Selezionare la pagina istruttori.

Quando l'applicazione viene eseguita questa pagina, tenta di accedere al database. Migrazioni Code First controlla se il database corrente e rileva che la migrazione di più recente non è ancora stata applicata. Migrazioni Code First si applica la migrazione di più recente, viene eseguito il `Seed` (metodo), quindi selezionare la pagina viene eseguito normalmente. Visualizzare la nuova colonna di ore di Office con i dati di seeding.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>L'aggiornamento del Database di distribuzione nell'ambiente di produzione

È necessario modificare anche il profilo di pubblicazione per l'ambiente di produzione. In questo caso si sarà rimuovere un profilo esistente e crearne una nuova importando un file con estensione publishsettings aggiornato. Il file aggiornato includerà la stringa di connessione per il database di SQL Server nel Cytanium.

Come specificato al momento della distribuzione nell'ambiente di testing, non è più necessario trasformazioni di stringa di connessione di *Web.Production.config* file di trasformazione. Apertura di file e rimuovere il `connectionStrings` elemento. Le trasformazioni rimanenti sono per la `Environment` valore il `appSettings` elemento e il `location` elemento che limita l'accesso ai report di errore Elmah.

Prima di creare un nuovo profilo di pubblicazione per la produzione, scaricare un file con estensione publishsettings aggiornato allo stesso modo è stato fatto in precedenza il [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione. (Nel Pannello di controllo Cytanium, fare clic su **siti Web**, quindi fare clic su di **contosouniversity.com** sito Web. Selezionare il **pubblicazione sul Web** scheda e quindi fare clic su **scaricare il profilo di pubblicazione per il sito web**.) Il motivo che questo tipo di procedura è prelevare la stringa di connessione di database nel file con estensione publishsettings. La stringa di connessione non è disponibile la prima volta che è stato scaricato il file, perché si sta ancora utilizzando SQL Server Compact e ancora non veniva creato il database di SQL Server in Cytanium.

È ora possibile aggiornare il profilo di pubblicazione e la pubblicazione nell'ambiente di produzione.

Aprire il **pubblica sul Web** procedura guidata, quindi passi al **profilo** scheda.

Fare clic su **Gestione profili**e quindi eliminare il profilo di produzione.

Chiudi il **pubblica sul Web** procedura guidata per salvare questa modifica.

Aprire il **pubblica sul Web** procedura guidata e quindi fare clic su **importazione**.

Nel **connessione** modificare **URL di destinazione** sul valore appropriato se si utilizza un URL temporaneo.

Scegliere **Avanti**.

Nel **impostazioni** scheda, fare clic su **attivare il nuovo database di pubblicazione miglioramenti**.

Nell'elenco di riepilogo a discesa stringhe di connessione per **SchoolContext**, selezionare la stringa di connessione Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Selezionare **migrazioni eseguire Code First (esecuzione all'avvio dell'applicazione)**.

Nell'elenco di riepilogo a discesa stringhe di connessione per **DefaultConnection**, selezionare la stringa di connessione Cytanium.

Selezionare il **profilo** scheda, fare clic su **Gestione profili**e rinomina il profilo da "contosouniversity.com - distribuzione Web" a "Produzione".

Chiudere il profilo di pubblicazione per salvare le modifiche, quindi aprirlo nuovamente.

Fare clic su **Pubblica**. (Per un sito Web di produzione reali, è necessario copiare *app\_offline.htm* alla produzione e put nella cartella del progetto prima della pubblicazione, quindi rimuoverlo quando la distribuzione è stata completata.)

Visual Studio consente di distribuire le modifiche al codice nell'ambiente di testing e apre il browser alla home page di Contoso University.

Selezionare la pagina istruttori.

Migrazioni Code First aggiorna il database stesso modo nell'ambiente di Test. Visualizzare la nuova colonna di ore di Office con i dati di seeding.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Ora aver distribuito un aggiornamento di un'applicazione che modifica un database, incluso l'utilizzo di un database di SQL Server.

## <a name="more-information"></a>Altre informazioni

In questo passaggio si completa questa serie di esercitazioni sulla distribuzione di un'applicazione web ASP.NET in un provider di hosting di terze parti. Per ulteriori informazioni sugli argomenti trattati in queste esercitazioni, vedere il [mappa del contenuto di distribuzione di ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) nel sito web MSDN.

## <a name="acknowledgements"></a>Riconoscimenti

Come è possibile grazie seguenti persone che hanno apportato contributi significativi per il contenuto di questa serie di esercitazioni:

- [Alberto Poblacion, MVP &amp; MCT, (Spagna)](https://mvp.support.microsoft.com/profile/Alberto)
- Stati Uniti Jarod Ferguson, MVP di sviluppo della piattaforma di dati,
- Harsh Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi (Italia)](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Hashimi sayed, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

>[!div class="step-by-step"]
[Precedente](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Successivo](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
