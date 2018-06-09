---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: impostazione delle autorizzazioni di cartella | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 7efe267975835e889950983126088f1b637c28fb
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "30890398"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: impostazione delle autorizzazioni di cartella
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione, impostare le autorizzazioni per il *Elmah* cartella nel sito web distribuito del sito in modo che l'applicazione può creare file di log in tale cartella.

Quando si testa un'applicazione web in Visual Studio usando il Server di sviluppo Visual Studio (Cassini) o IIS Express, l'applicazione viene eseguito con la tua identità. Si è molto probabile che un amministratore nel computer di sviluppo e di autorità completa eseguire alcuna azione per qualsiasi file in qualsiasi cartella. Tuttavia, quando un'applicazione viene eseguita in IIS, viene eseguito in base all'identità definita per il pool di applicazioni che il sito è assegnato a. In genere si tratta di un account definito dal sistema che dispone di autorizzazioni limitate. Per impostazione predefinita è di lettura e le autorizzazioni di esecuzione per file e cartelle dell'applicazione web, ma non ha accesso in scrittura.

Se l'applicazione crea o aggiorna i file, ovvero un comune nelle applicazioni web, questo diventa un problema. Nell'applicazione Contoso University, Elmah crea file XML di *Elmah* cartella per salvare i dettagli sugli errori. Anche se non si utilizza un risultato simile Elmah, il sito potrebbe consentire agli utenti di caricare i file o eseguire altre operazioni di scrittura dei dati in una cartella del sito.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Segnalazione e registrazione degli errori di test

Per vedere come l'applicazione non funziona correttamente in IIS (anche se ha quando è stato testato in Visual Studio), è possibile causare un errore che viene registrato in genere da Elmah e quindi aprirla il log degli errori Elmah per visualizzare i dettagli. Se Elmah non è riuscito a creare un file XML e archiviare i dettagli dell'errore, viene visualizzato un report degli errori vuoto.

Aprire un browser e passare a `http://localhost/ContosoUniversity`, e quindi richiedere un URL non valido come *Studentsxxx.aspx*. Viene visualizzata una pagina di errore generati dal sistema anziché il *GenericErrorPage* pagina perché il `customErrors` impostazione nel file Web. config è "RemoteOnly" e si esegue IIS in locale:

![Pagina di errore 404 HTTP](setting-folder-permissions/_static/image1.png)

A questo punto eseguire *Elmah.axd* per visualizzare il report di errore. Dopo l'accesso con le credenziali dell'account amministratore (&quot;admin&quot; e &quot;devpwd&quot;), è visualizzata una pagina di log degli errori vuoto perché non è riuscito a creare un file XML in Elmah di *Elmah*cartella:

![Errore registro vuoto](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Impostare le autorizzazioni di scrittura sulla cartella Elmah

È possibile impostare autorizzazioni per le cartelle manualmente o può essere utilizzato una parte del processo di distribuzione automatica. Rende automatico richiede codice complesso di MSBuild, e poiché è sufficiente eseguire questa operazione la prima volta che si distribuisce, i seguenti passaggi come crearlo manualmente. (Per informazioni su come effettuare questa parte del processo di distribuzione, vedere [impostazione delle autorizzazioni di cartella nel sito Web-pubblicazione](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) sul blog di Hashimi Sayed.)

1. In **Esplora File**, passare a *C:\inetpub\wwwroot\ContosoUniversity*. Fare doppio clic sul *Elmah* cartella, selezionare **proprietà**, quindi selezionare il **sicurezza** scheda.
2. Fare clic su **Modifica**.
3. Nel **le autorizzazioni per Elmah** nella finestra di dialogo **DefaultAppPool**e quindi selezionare il **scrivere** casella di controllo di **Consenti** colonna.

    ![Autorizzazioni per la cartella ELMAH](setting-folder-permissions/_static/image3.png)

    (Se non viene visualizzato **DefaultAppPool** nel **nomi utente o gruppo** elenco, è stato probabilmente usato un altro metodo rispetto a quello specificato in questa esercitazione per configurare IIS e ASP.NET 4 nel computer in uso. In tal caso, individuare il tipo di identità viene utilizzato dal pool di applicazioni assegnati all'applicazione Contoso University e concedere l'autorizzazione di scrittura a tale identità. Vedere i collegamenti sull'identità del pool di applicazioni alla fine di questa esercitazione). Fare clic su **OK** in entrambe le finestre di dialogo.

## <a name="retest-error-logging-and-reporting"></a>Testare nuovamente la registrazione degli errori e creazione di report

Test, generando un errore nuovamente nello stesso modo (richiesta di un URL non valido) ed eseguire il **Log degli errori** pagina. Questa volta l'errore viene visualizzato nella pagina.

![Pagina di Log degli errori ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Riepilogo

Sono stati completati tutte le attività necessarie per ottenere Contoso University funziona correttamente in IIS sul computer locale. Nella prossima esercitazione, verranno apportate al sito disponibile pubblicamente mediante la distribuzione in Azure.

## <a name="more-information"></a>Altre informazioni

In questo esempio, il motivo per cui non è riuscito a salvare file di log Elmah è abbastanza ovvio. È possibile utilizzare la traccia di IIS nei casi in cui non è ovvia; la causa del problema vedere [Troubleshooting Failed Requests utilizzando Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) nel sito IIS.net.

Per ulteriori informazioni su come concedere le autorizzazioni per le identità del pool di applicazioni, vedere [le identità del Pool di applicazioni](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [contenuto sicuro in IIS tramite ACL del File System](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) nel sito IIS.net.

> [!div class="step-by-step"]
> [Precedente](deploying-to-iis.md)
> [Successivo](deploying-to-production.md)
