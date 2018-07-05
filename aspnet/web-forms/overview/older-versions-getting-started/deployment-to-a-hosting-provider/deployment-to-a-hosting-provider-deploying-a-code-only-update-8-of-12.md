---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione di un aggiornamento Code-Only - 8 pari a 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 39075d2e979076f88200ea595c92f95f38f35911
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374013"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione di un aggiornamento Code-Only - 8 pari a 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e Mostra come distribuire in App Web di servizio App di Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

Dopo la distribuzione iniziale, continua il lavoro della manutenzione e il sito web di sviluppo e prima di prolungata è opportuno distribuire un aggiornamento. Questa esercitazione illustra il processo di distribuzione di aggiornamenti al codice dell'applicazione. Questo aggiornamento non comporta una modifica del database; scoprirai che cos'è diversi sulla distribuzione di una modifica al database nella prossima esercitazione.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Apportare una modifica del codice

Un esempio semplice di un aggiornamento all'applicazione, si aggiungeranno al **instructors (insegnanti)** pagina un elenco dei corsi tenuti dall'insegnante selezionato.

Se si esegue la **instructors (insegnanti)** pagina, si noterà che esistono **selezionare** collegamenti nella griglia, ma non esegue alcuna operazione oltre marca il grigio turni di riga in background.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

A questo punto si aggiungerà codice che viene eseguito quando il **seleziona** collegamento fa e visualizza un elenco dei corsi tenuti dall'insegnante selezionato.

Nelle *Instructors.aspx*, aggiungere il markup seguente immediatamente dopo il **ErrorMessageLabel** `Label` controllo:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Eseguire la pagina e selezionare un insegnante. Viene visualizzato un elenco dei corsi tenuti da tale insegnante.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Distribuire l'aggiornamento del codice nell'ambiente di Test

La distribuzione nell'ambiente di test è davvero semplicissimo dell'esecuzione con un clic pubblicare di nuovo. Per rendere più veloce il processo, è possibile usare la **Web-pubblicazione con un clic** sulla barra degli strumenti.

Nel **View** menu, scegliere **barre degli strumenti** e quindi selezionare **Web-pubblicazione con un clic**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

Nelle **Esplora soluzioni**, selezionare il progetto ContosoUniversity.

il **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web** (l'icona con frecce che puntano a sinistra e destra).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio distribuisce l'applicazione aggiornata e il browser viene aperta automaticamente alla home page. Eseguire la pagina instructors (insegnanti) e selezionare un insegnante per verificare che l'aggiornamento è stato distribuito correttamente.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

In genere viene usato anche eseguire test di regressione (vale a dire, il resto del sito per assicurarsi che la nuova modifica non interrompe con le funzionalità esistenti di test). Ma per questa esercitazione è possibile ignorare il passaggio e andare al distribuire l'aggiornamento nell'ambiente di produzione.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Impedisce la ridistribuzione dello stato del Database iniziale nell'ambiente di produzione

In un'applicazione reale, gli utenti interagiscono con il sito di produzione dopo la distribuzione iniziale e i database vengono popolati con dati in tempo reale. Pertanto, non si vuole ridistribuire il database di appartenenza nello stato iniziale, si cancellazione tutti i dati in tempo reale. Poiché i database di SQL Server Compact sono file nei *App\_dati* cartella, è necessario evitare questo problema, modificare le impostazioni di distribuzione in modo che i file nei *App\_dati* cartella non sono distribuiti.

Aprire il **proprietà progetto** finestra per il progetto ContosoUniversity e selezionare il **pubblicazione/creazione pacchetto Web** scheda. Assicurarsi che il **Configuration** casella di riepilogo è stata **attivo (versione)** o **rilascio** selezionato, selezionare **escludere i file dall'App\_Cartella data**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

Nel caso in cui si decide di distribuire una build di debug in futuro, è consigliabile apportare la stessa modifica per la configurazione di build di Debug: cambiare **Configuration** al **Debug** e quindi selezionare **escludere i file dall'App\_cartella dati**.

Salvare e chiudere il **pubblicazione/creazione pacchetto Web** scheda.

> [!NOTE] 
> 
> [!IMPORTANT]
> Assicurarsi che non hai **Rimuovi file aggiuntivi nella destinazione** selezionato nei profili di pubblicazione. Se si seleziona questa opzione, il processo di distribuzione verrà eliminati i database presenti nell'App\_l'App verranno eliminata dati nel sito distribuito e\_cartella dati stesso.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Impedire l'accesso utente al sito di produzione durante l'aggiornamento

La modifica che si esegue la distribuzione è ora una semplice modifica a una singola pagina. Ma in alcuni casi si distribuiscono le modifiche più significative, e in tal caso il sito può si comportano in modo se un utente richiede una pagina prima del completamento della distribuzione. Per evitare questo problema, è possibile usare un *app\_offline.htm* file. Quando si inserisce un file denominato *app\_offline.htm* nella cartella radice dell'applicazione, IIS consente di visualizzare automaticamente tale file invece di eseguire l'applicazione. Per impedire l'accesso durante la distribuzione, per l'inserimento *app\_offline.htm* nella cartella radice, eseguire il processo di distribuzione e quindi rimuovere *app\_offline.htm*.

Nelle **Esplora soluzioni**, fare doppio clic la soluzione (non uno dei progetti) e selezionare **nuova cartella soluzione**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Denominare la cartella *SolutionFiles*.

Nella nuova cartella creare una pagina HTML denominata *app\_offline.htm*. Sostituire il contenuto esistente con il markup seguente:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

È possibile copiare il *app\_offline.htm* file al sito tramite una connessione FTP o il **gestione File** utilità nel Pannello di controllo del provider di hosting. Per questa esercitazione si userà il **gestione File**.

Aprire il pannello di controllo e selezionare **gestione File** seguendo la [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione. Selezionare **contosouniversity.com** e quindi **wwwroot** per accedere alla cartella radice dell'applicazione e quindi fare clic su **caricare**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

Nel **carica File** finestra di dialogo, seleziona la *app\_offline.htm* file e quindi fare clic su **caricare**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Passare all'URL del sito. Si può osservare che il *app\_offline.htm* viene visualizzata la pagina anziché la home page.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

A questo punto si è pronti per la distribuzione nell'ambiente di produzione.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Distribuire l'aggiornamento del codice nell'ambiente di produzione

Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **produzione** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.

Visual Studio distribuisce l'applicazione aggiornata e verrà aperto il browser alla pagina iniziale del sito. Il *app\_offline.htm* file viene visualizzato. Prima di testare per verificare la corretta distribuzione, è necessario rimuovere il *app\_offline.htm* file.

Tornare al **gestione File** applicazioni nel Pannello di controllo. Selezionare **contosouniversity.com** e **wwwroot**, selezionare **app\_offline.htm**, quindi fare clic su **Elimina**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

Nel browser, aprire la pagina instructors (insegnanti) nel sito pubblico e selezionare un insegnante per verificare che l'aggiornamento è stato distribuito correttamente.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

È stato distribuito un aggiornamento dell'applicazione che non ha comportato una modifica al database. L'esercitazione successiva illustra come distribuire una modifica al database.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
