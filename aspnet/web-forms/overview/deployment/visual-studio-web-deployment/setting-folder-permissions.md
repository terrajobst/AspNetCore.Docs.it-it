---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Distribuzione Web ASP.NET tramite Visual Studio: impostazione delle autorizzazioni di cartella | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 0660a464063783406a69caf663036811c8ac818e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802033"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Distribuzione Web ASP.NET tramite Visual Studio: impostazione delle autorizzazioni di cartella
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione, impostare le autorizzazioni per il *Elmah* cartella web distribuito in modo che l'applicazione può creare i file di log nella cartella del sito.

Quando si testa un'applicazione web in Visual Studio usando il Visual Studio Development Server (Cassini) o IIS Express, l'applicazione viene eseguita con la tua identità. Si è molto probabile che un amministratore nel computer di sviluppo e disporre di autorità completa eseguire alcuna azione per qualsiasi file in una cartella specifica. Ma quando si esegue un'applicazione in IIS, può essere eseguito con l'identità definita per il pool di applicazioni assegnato al sito. Si tratta in genere di un account definito dal sistema che dispone di autorizzazioni limitate. Per impostazione predefinita dispone di autorizzazioni read ed execute sul file e cartelle dell'applicazione web, ma non ha accesso in scrittura.

Questo diventa un problema se l'applicazione crea o aggiorna i file, che è un comune necessarie nelle applicazioni web. Nell'applicazione di Contoso University, Elmah crea i file XML nel *Elmah* cartella per salvare i dettagli sugli errori. Anche se non si usa un elemento, ad esempio Elmah, il sito potrebbe consentire agli utenti di caricare i file o eseguire altre attività di scrittura dei dati in una cartella nel sito.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Segnalazione e registrazione degli errori di test

Per vedere come l'applicazione non funziona correttamente in IIS (anche se ha quando è stata testata in Visual Studio), si può causare un errore che viene in genere registrato da Elmah e quindi aprirla il log degli errori Elmah per visualizzare i dettagli. Se Elmah non è riuscito a creare un file XML e archiviare i dettagli dell'errore, viene visualizzato un report degli errori vuoto.

Aprire un browser e passare a `http://localhost/ContosoUniversity`, e quindi richiedere un URL non valido, ad esempio *Studentsxxx.aspx*. Verrà visualizzata una pagina di errore generati dal sistema anziché il *GenericErrorPage* pagina perché il `customErrors` impostazione nel file Web. config è "RemoteOnly" ed è in esecuzione IIS in locale:

![Pagina di errore 404 HTTP](setting-folder-permissions/_static/image1.png)

A questo punto eseguire *Elmah.axd* per visualizzare il report di errore. Dopo l'accesso con le credenziali dell'account amministratore (&quot;admin&quot; e &quot;devpwd&quot;), è possibile visualizzare una pagina di log degli errori vuoto perché non è riuscito a creare un file XML in Elmah il *Elmah*cartella:

![Errore del log vuoto](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Impostare le autorizzazioni di scrittura sulla cartella Elmah

È possibile impostare manualmente le autorizzazioni della cartella oppure è possibile renderlo una parte del processo di distribuzione automatica. Rendendo automatica richiede codice MSBuild complesso e poiché è sufficiente eseguire questa operazione la prima volta che si distribuisce, i passaggi seguenti illustrano come eseguire l'operazione manualmente. (Per informazioni su come effettuare questa parte del processo di distribuzione, vedere [impostazione delle autorizzazioni di cartella nel sito Web-pubblicazione](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) sul blog di Sayed Hashimi.)

1. Nelle **Esplora File**, passare alla *C:\inetpub\wwwroot\ContosoUniversity*. Fare doppio clic sui *Elmah* cartella, selezionare **delle proprietà**e quindi selezionare il **sicurezza** scheda.
2. Fare clic su **Modifica**.
3. Nel **le autorizzazioni per Elmah** finestra di dialogo **DefaultAppPool**e quindi selezionare il **scrivere** casella di controllo la **Consenti** colonna.

    ![Autorizzazioni per la cartella ELMAH](setting-folder-permissions/_static/image3.png)

    (Se non viene visualizzata **DefaultAppPool** nel **nomi utente o gruppo** elenco, probabilmente utilizzato un altro metodo rispetto a quello specificato in questa esercitazione per configurare IIS e ASP.NET 4 nel computer. In tal caso, scoprire quali identità viene usata dal pool di applicazioni assegnato all'applicazione Contoso University e concedere l'autorizzazione di scrittura per tale identità. Vedere i collegamenti relativi a identità del pool di applicazioni alla fine di questa esercitazione). Fare clic su **OK** in entrambe le finestre di dialogo.

## <a name="retest-error-logging-and-reporting"></a>Testare nuovamente la registrazione degli errori e creazione di report

Per testare questo causa un errore nuovamente nello stesso modo (richiesta di un URL non valido) ed eseguire la **Log degli errori** pagina. Questa volta l'errore viene visualizzato nella pagina.

![Pagina di Log degli errori ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Riepilogo

È stata ora completata tutte le attività necessarie per ottenere Contoso University funziona correttamente in IIS nel computer locale. Nella prossima esercitazione, si apporterà del sito disponibili pubblicamente per distribuirla in Azure.

## <a name="more-information"></a>Altre informazioni

In questo esempio, il motivo per cui non è riuscito a salvare file di log Elmah è abbastanza ovvio. È possibile utilizzare la traccia di IIS nei casi in cui non è così ovvia; la causa del problema visualizzare [risoluzione dei problemi Failed Requests Using Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) sul sito IIS.net.

Per altre informazioni su come concedere le autorizzazioni per le identità del pool di applicazioni, vedere [Application Pool Identities](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [proteggere contenuti in IIS tramite ACL del File System](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) sul sito IIS.net.

> [!div class="step-by-step"]
> [Precedente](deploying-to-iis.md)
> [Successivo](deploying-to-production.md)
