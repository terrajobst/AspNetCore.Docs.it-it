---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: impostazione delle autorizzazioni di cartella - 6, 12 | Documenti Microsoft"
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 573e75221a1c0018bded7544e584b0c75f47d607
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: impostazione delle autorizzazioni di cartella - 6, 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione di serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire l'App Web di servizio App di Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione, impostare le autorizzazioni per il *Elmah* cartella nel sito web distribuito del sito in modo che l'applicazione può creare file di log in tale cartella.

Quando si testa un'applicazione web in Visual Studio usando Visual Studio Development Server (Cassini), l'applicazione viene eseguito con la tua identità. Si è molto probabile che un amministratore nel computer di sviluppo e di autorità completa eseguire alcuna azione per qualsiasi file in qualsiasi cartella. Tuttavia, quando un'applicazione viene eseguita in IIS, viene eseguito in base all'identità definita per il pool di applicazioni che il sito è assegnato a. In genere si tratta di un account definito dal sistema che dispone di autorizzazioni limitate. Per impostazione predefinita è di lettura e le autorizzazioni di esecuzione per file e cartelle dell'applicazione web, ma non ha accesso in scrittura.

Se l'applicazione crea o aggiorna i file, ovvero un comune nelle applicazioni web, questo diventa un problema. Nell'applicazione Contoso University, Elmah crea file XML di *Elmah* cartella per salvare i dettagli sugli errori. Anche se non si utilizza un risultato simile Elmah, il sito potrebbe consentire agli utenti di caricare i file o eseguire altre operazioni di scrittura dei dati in una cartella del sito.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Errore di registrazione e report di test

Per vedere come l'applicazione non funziona correttamente in IIS (anche se ha quando è stato testato in Visual Studio), è possibile causare un errore che viene registrato in genere da Elmah e quindi aprirla il log degli errori Elmah per visualizzare i dettagli. Se Elmah non è riuscito a creare un file XML e archiviare i dettagli dell'errore, viene visualizzato un report degli errori vuoto.

Aprire un browser e passare a `http://localhost/ContosoUniversity`, e quindi richiedere un URL non valido come *Studentsxxx.aspx*. Viene visualizzata una pagina di errore generati dal sistema anziché il *GenericErrorPage* pagina perché il `customErrors` impostazione nel file Web. config è "RemoteOnly" e si esegue IIS in locale:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

A questo punto eseguire *Elmah.axd* per visualizzare il report di errore. È visualizzata una pagina di log degli errori vuoto perché non è riuscito a creare un file XML in Elmah di *Elmah* cartella:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Impostazione di autorizzazione di scrittura sulla cartella Elmah

È possibile impostare autorizzazioni per le cartelle manualmente o può essere utilizzato una parte del processo di distribuzione automatica. Rende automatico richiede codice complesso di MSBuild e poiché è necessario eseguire questa operazione la prima volta che si distribuisce, questa esercitazione viene illustrato solo come eseguire l'operazione manualmente. (Per informazioni su come effettuare questa parte del processo di distribuzione, vedere [impostazione delle autorizzazioni di cartella nel sito Web-pubblicazione](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) sul blog di Hashimi Sayed.)

In **Esplora**, passare a *C:\inetpub\wwwroot\ContosoUniversity*. Fare doppio clic sul *Elmah* cartella, selezionare **proprietà**, quindi selezionare il **sicurezza** scheda.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Se non viene visualizzato **DefaultAppPool** nel **nomi utente o gruppo** elenco, è stato probabilmente usato un altro metodo rispetto a quello specificato in questa esercitazione per configurare IIS e ASP.NET 4 nel computer in uso. In tal caso, individuare il tipo di identità viene utilizzato dal pool di applicazioni assegnati all'applicazione Contoso University e concedere l'autorizzazione di scrittura a tale identità. Vedere i collegamenti sull'identità del pool di applicazioni alla fine di questa esercitazione).

Fare clic su **Modifica**. Nel **le autorizzazioni per Elmah** nella finestra di dialogo **DefaultAppPool**e quindi selezionare il **scrivere** casella di controllo di **Consenti** colonna.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Fare clic su **OK** in entrambe le finestre di dialogo.

## <a name="retesting-error-logging-and-reporting"></a>Ripetizione di registrazione e report errori

Test, generando un errore nuovamente nello stesso modo (richiesta di un URL non valido) ed eseguire il **Log degli errori** pagina. Questa volta l'errore viene visualizzato nella pagina.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

È inoltre necessario autorizzazione di scrittura su di *App\_dati* cartella poiché sono file di database di SQL Server Compact in tale cartella e si desidera essere in grado di aggiornare i dati in tali database. In tal caso, tuttavia, non è possibile eseguire alcuna operazione aggiuntiva poiché il processo di distribuzione viene impostato automaticamente dell'autorizzazione di scrittura sul *App\_dati* cartella.

Sono stati completati tutte le attività necessarie per ottenere Contoso University funziona correttamente in IIS sul computer locale. Nella prossima esercitazione, verranno apportate al sito disponibile pubblicamente mediante la distribuzione di un provider di hosting.

## <a name="more-information"></a>Altre informazioni

In questo esempio, il motivo per cui non è riuscito a salvare file di log Elmah è abbastanza ovvio. È possibile utilizzare la traccia di IIS nei casi in cui non è ovvia; la causa del problema vedere [Troubleshooting Failed Requests utilizzando Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) nel sito IIS.net.

Per ulteriori informazioni su come concedere le autorizzazioni per le identità del pool di applicazioni, vedere [le identità del Pool di applicazioni](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [contenuto sicuro in IIS tramite ACL del File System](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) nel sito IIS.net.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
