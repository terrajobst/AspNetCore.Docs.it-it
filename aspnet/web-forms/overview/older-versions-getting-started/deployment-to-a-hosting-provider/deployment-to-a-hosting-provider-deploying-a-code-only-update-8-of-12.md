---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di un aggiornamento Code-Only - 8 a 12 | Documenti Microsoft"
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6305a8cb87d9b00b6bb4c4f8fa6114bddc4eb89f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886914"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di un aggiornamento Code-Only - 8 a 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione di serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire l'App Web di servizio App di Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

Dopo la distribuzione iniziale, il lavoro di gestione e il sito web di sviluppo continua e poco tempo è possibile distribuire un aggiornamento. In questa esercitazione illustra il processo di distribuzione di un aggiornamento per il codice dell'applicazione. Questo aggiornamento non comporta la modifica un database. che cos'è diverso sulla distribuzione di una modifica al database nella prossima esercitazione verrà visualizzato.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Effettua una modifica del codice

Un esempio semplice di un aggiornamento per l'applicazione, aggiungerai il **i docenti** pagina un elenco di corsi tenuti da istruttore selezionato.

Se si esegue il **i docenti** pagina, si noterà che non esistono **selezionare** collegamenti nella griglia, ma non se si diverso da rendere il grigio attiva in background di riga.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Ora si aggiungerà il codice che viene eseguita quando il **selezionare** collegamento viene selezionato e visualizza un elenco di corsi tenuti da istruttore selezionato.

In *Instructors.aspx*, aggiungere il markup seguente immediatamente dopo il **ErrorMessageLabel** `Label` controllo:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Eseguire la pagina e selezionare un docente. Viene visualizzato un elenco di corsi tenuti da tale istruttore.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Distribuzione dell'aggiornamento di codice per l'ambiente di Test

La distribuzione nell'ambiente di testing è in esecuzione un solo clic sufficiente pubblicare di nuovo. Per rendere più veloce questo processo, è possibile utilizzare il **Web-pubblicazione con un clic** barra degli strumenti.

Nel **vista** menu, scegliere **barre degli strumenti** e quindi selezionare **Web-pubblicazione con un clic**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

In **Esplora**, selezionare il progetto ContosoUniversity.

il **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web** (l'icona con frecce rivolte a sinistra e destra).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio distribuisce l'applicazione aggiornata e il browser apre automaticamente alla home page. Eseguire la pagina istruttori e selezionare un docente per verificare che l'aggiornamento è stato distribuito correttamente.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

È normalmente anche eseguire test di regressione (vale a dire il resto del sito per assicurarsi che la nuova modifica non è stato interrotto le funzionalità esistenti di test). Ma per questa esercitazione verrà ignorare tale passaggio e continuare a distribuire l'aggiornamento nell'ambiente di produzione.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Ridistribuzione impedendo dello stato del Database iniziale nell'ambiente di produzione

In un'applicazione reale, gli utenti interagiscono con il sito di produzione dopo la distribuzione iniziale, e i database vengono popolati con dati in tempo reale. Pertanto, non si desidera ridistribuire il database delle appartenenze nello stato iniziale, si cancellazione tutti i dati in tempo reale. Poiché i database di SQL Server Compact sono file di *App\_dati* cartella, è necessario evitare questo problema, modificare le impostazioni di distribuzione in modo che i file nella *App\_dati* cartella non è stato distribuito.

Aprire il **le proprietà del progetto** finestra per il progetto ContosoUniversity e selezionare il **pubblicazione/creazione pacchetto Web** scheda. Assicurarsi che il **configurazione** casella di riepilogo a discesa è **attiva (rilascio)** o **versione** selezionata, selezionare **escludere i file dall'App\_Cartella dati**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

Nel caso in cui si decide di distribuire una build di debug in futuro, è consigliabile apportare la stessa modifica per la configurazione di compilazione di Debug: modificare **configurazione** a **Debug** e quindi selezionare **escludere i file dall'App\_cartella dati**.

Salvare e chiudere il **pubblicazione/creazione pacchetto Web** scheda.

> [!NOTE] 
> 
> [!IMPORTANT]
> Assicurarsi che non è necessario **Rimuovi file aggiuntivi nella destinazione** selezionato nei profili di pubblicazione. Se si seleziona questa opzione, il processo di distribuzione verrà eliminati i database disponibili in App\_dati nel sito distribuito e verranno eliminati l'App\_cartella dati stessa.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Impedire l'accesso utente per il sito di produzione durante l'aggiornamento

La modifica che si esegue la distribuzione è ora una semplice modifica a una singola pagina. Ma in alcuni casi si distribuiscono le modifiche più significative e in tal caso il sito può si comportano in modo se un utente richiede una pagina prima del completamento della distribuzione. Per evitare questo problema, è possibile utilizzare un *app\_offline.htm* file. Quando si inserisce un file denominato *app\_offline.htm* nella cartella radice dell'applicazione, IIS viene visualizzato automaticamente tale file anziché eseguire l'applicazione. Per impedire l'accesso durante la distribuzione, per l'inserimento *app\_offline.htm* nella cartella radice, eseguire il processo di distribuzione, quindi rimuovere *app\_offline.htm*.

In **Esplora**, la soluzione (non uno dei progetti) e scegliere **nuova cartella soluzione**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Denominare la cartella *SolutionFiles*.

Nella nuova cartella, creare una pagina HTML denominata *app\_offline.htm*. Sostituire il contenuto esistente con il markup seguente:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

È possibile copiare il *app\_offline.htm* file al sito tramite una connessione FTP o **gestione File** utilità nel Pannello di controllo del provider di hosting. Per questa esercitazione, si userà il **gestione File**.

Aprire il pannello di controllo e selezionare **gestione File** seguendo il [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione. Selezionare **contosouniversity.com** e quindi **wwwroot** per accedere alla cartella radice dell'applicazione e quindi fare clic su **caricare**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

Nel **carica File** la finestra di dialogo, seleziona il *app\_offline.htm* file e quindi fare clic su **caricare**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Passare all'URL del sito. Vedrai che il *app\_offline.htm* pagina viene visualizzata invece la home page.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

A questo punto si è pronti distribuire nell'ambiente di produzione.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Distribuzione dell'aggiornamento di codice nell'ambiente di produzione

Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **produzione** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.

Visual Studio distribuisce l'applicazione aggiornata e viene aperto il browser alla home page del sito. Il *app\_offline.htm* file viene visualizzato. Poter eseguire il test per verificare la corretta distribuzione, è necessario rimuovere il *app\_offline.htm* file.

Restituito per il **gestione File** applicazioni nel Pannello di controllo. Selezionare **contosouniversity.com** e **wwwroot**selezionare **app\_offline.htm**, quindi fare clic su **eliminare**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

Nel browser, aprire la pagina istruttori pubblica del sito e selezionare un docente per verificare che l'aggiornamento è stato distribuito correttamente.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Ora aver distribuito un aggiornamento di un'applicazione che non ha comportato una modifica al database. L'esercitazione successiva viene illustrato come distribuire una modifica al database.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
