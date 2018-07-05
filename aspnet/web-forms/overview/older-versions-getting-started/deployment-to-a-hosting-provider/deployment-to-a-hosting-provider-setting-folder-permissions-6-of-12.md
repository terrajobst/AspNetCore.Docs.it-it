---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: impostazione delle autorizzazioni di cartella - 6 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 50b67c20ee374f1c7ab846446f836411132ddf78
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803681"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: impostazione delle autorizzazioni di cartella - 6, 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e Mostra come distribuire in App Web di servizio App di Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione, impostare le autorizzazioni per il *Elmah* cartella web distribuito in modo che l'applicazione può creare i file di log nella cartella del sito.

Quando si testa un'applicazione web in Visual Studio con Visual Studio Development Server (Cassini), l'applicazione viene eseguita con la tua identità. Si è molto probabile che un amministratore nel computer di sviluppo e disporre di autorità completa eseguire alcuna azione per qualsiasi file in una cartella specifica. Ma quando si esegue un'applicazione in IIS, può essere eseguito con l'identità definita per il pool di applicazioni assegnato al sito. Si tratta in genere di un account definito dal sistema che dispone di autorizzazioni limitate. Per impostazione predefinita dispone di autorizzazioni read ed execute sul file e cartelle dell'applicazione web, ma non ha accesso in scrittura.

Questo diventa un problema se l'applicazione crea o aggiorna i file, che è un comune necessarie nelle applicazioni web. Nell'applicazione di Contoso University, Elmah crea i file XML nel *Elmah* cartella per salvare i dettagli sugli errori. Anche se non si usa un elemento, ad esempio Elmah, il sito potrebbe consentire agli utenti di caricare i file o eseguire altre attività di scrittura dei dati in una cartella nel sito.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Test di registrazione e segnalazione errori

Per vedere come l'applicazione non funziona correttamente in IIS (anche se ha quando è stata testata in Visual Studio), si può causare un errore che viene in genere registrato da Elmah e quindi aprirla il log degli errori Elmah per visualizzare i dettagli. Se Elmah non è riuscito a creare un file XML e archiviare i dettagli dell'errore, viene visualizzato un report degli errori vuoto.

Aprire un browser e passare a `http://localhost/ContosoUniversity`, e quindi richiedere un URL non valido, ad esempio *Studentsxxx.aspx*. Verrà visualizzata una pagina di errore generati dal sistema anziché il *GenericErrorPage* pagina perché il `customErrors` impostazione nel file Web. config è "RemoteOnly" ed è in esecuzione IIS in locale:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

A questo punto eseguire *Elmah.axd* per visualizzare il report di errore. È possibile visualizzare una pagina di log degli errori vuoto perché non è riuscito a creare un file XML in Elmah il *Elmah* cartella:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Impostazione di autorizzazioni di scrittura sulla cartella Elmah

È possibile impostare manualmente le autorizzazioni della cartella oppure è possibile renderlo una parte del processo di distribuzione automatica. Rendendo automatica richiede codice MSBuild complesso e poiché è sufficiente eseguire questa operazione la prima volta che si distribuisce, questa esercitazione illustra solo come farlo manualmente. (Per informazioni su come effettuare questa parte del processo di distribuzione, vedere [impostazione delle autorizzazioni di cartella nel sito Web-pubblicazione](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) sul blog di Sayed Hashimi.)

Nelle **Windows Explorer**, passare alla *C:\inetpub\wwwroot\ContosoUniversity*. Fare doppio clic sui *Elmah* cartella, selezionare **delle proprietà**e quindi selezionare il **sicurezza** scheda.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Se non viene visualizzata **DefaultAppPool** nel **nomi utente o gruppo** elenco, probabilmente utilizzato un altro metodo rispetto a quello specificato in questa esercitazione per configurare IIS e ASP.NET 4 nel computer. In tal caso, scoprire quali identità viene usata dal pool di applicazioni assegnato all'applicazione Contoso University e concedere l'autorizzazione di scrittura per tale identità. Vedere i collegamenti relativi a identità del pool di applicazioni alla fine di questa esercitazione).

Fare clic su **Modifica**. Nel **le autorizzazioni per Elmah** finestra di dialogo **DefaultAppPool**e quindi selezionare il **scrivere** casella di controllo la **Consenti** colonna.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Fare clic su **OK** in entrambe le finestre di dialogo.

## <a name="retesting-error-logging-and-reporting"></a>Riesecuzione di test di registrazione e segnalazione errori

Per testare questo causa un errore nuovamente nello stesso modo (richiesta di un URL non valido) ed eseguire la **Log degli errori** pagina. Questa volta l'errore viene visualizzato nella pagina.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

È anche necessario scrivere l'autorizzazione nel *App\_dati* cartella perché sono file di database SQL Server Compact in tale cartella e si desidera essere in grado di aggiornare i dati in tali database. In tal caso, tuttavia, non devi eseguire alcuna operazione aggiuntiva poiché il processo di distribuzione viene impostato automaticamente l'autorizzazione di scrittura sul *App\_dati* cartella.

È stata ora completata tutte le attività necessarie per ottenere Contoso University funziona correttamente in IIS nel computer locale. Nella prossima esercitazione, si apporterà del sito disponibili pubblicamente tramite distribuzione su un provider di hosting.

## <a name="more-information"></a>Altre informazioni

In questo esempio, il motivo per cui non è riuscito a salvare file di log Elmah è abbastanza ovvio. È possibile utilizzare la traccia di IIS nei casi in cui non è così ovvia; la causa del problema visualizzare [risoluzione dei problemi Failed Requests Using Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) sul sito IIS.net.

Per altre informazioni su come concedere le autorizzazioni per le identità del pool di applicazioni, vedere [Application Pool Identities](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [proteggere contenuti in IIS tramite ACL del File System](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) sul sito IIS.net.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
