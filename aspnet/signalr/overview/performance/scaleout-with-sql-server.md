---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Scalabilità orizzontale SignalR con SQL Server | Documenti Microsoft
author: MikeWasson
description: Versioni del software utilizzato in questo argomento di Visual Studio 2013 .NET 4.5 SignalR le versioni precedenti di versione 2 di questo argomento per informazioni sulle versioni precedenti di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: b3189c36fc076333c0c6007bd039b12e03d63bc8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874369"
---
<a name="signalr-scaleout-with-sql-server"></a>Scalabilità orizzontale SignalR con SQL Server
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


In questa esercitazione si utilizzerà SQL Server per distribuire i messaggi tra un'applicazione di SignalR distribuito in due istanze separate di IIS. È anche possibile eseguire questa esercitazione in un computer singolo test, ma per ottenere l'effetto completo, è necessario distribuire l'applicazione di SignalR per due o più server. È inoltre necessario installare SQL Server in uno dei server o in un server dedicato. Un'altra opzione consiste nell'eseguire l'esercitazione con macchine virtuali in Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Prerequisiti

Microsoft SQL Server 2005 o versione successiva. Backplane supporta le edizioni di desktop e server di SQL Server. Non supporta Database SQL Azure o SQL Server Compact Edition. (Se l'applicazione è ospitata in Azure, è consigliabile backplane Bus di servizio invece.)

## <a name="overview"></a>Panoramica

Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.

1. Creare un nuovo database vuoto. Backplane creerà le tabelle necessarie nel database.
2. Aggiungere questi pacchetti NuGet per l'applicazione: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Creare un'applicazione di SignalR.
4. Aggiungere il codice seguente al Startup.cs configurare backplane: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   Questo codice consente di configurare backplane con i valori predefiniti per [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Per informazioni sulla modifica di questi valori, vedere [delle prestazioni di SignalR: metriche di scalabilità orizzontale](signalr-performance.md#scaleout_metrics). 

## <a name="configure-the-database"></a>Configurare il Database

Decidere se l'applicazione utilizzerà l'autenticazione di Windows o autenticazione di SQL Server per accedere al database. Verificare in ogni caso, che l'utente del database disponga delle autorizzazioni per accedere, creare gli schemi e creare tabelle.

Creare un nuovo database per backplane da utilizzare. È possibile assegnare il database di qualsiasi nome. Non è necessario creare tutte le tabelle nel database. backplane creerà le tabelle necessarie.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Abilitare Service Broker

È consigliabile abilitare Service Broker per il database backplane. Service Broker fornisce supporto nativo per la messaggistica e Accodamento in SQL Server, che consente di ricevere gli aggiornamenti in modo più efficiente backplane. (Tuttavia, backplane funziona anche senza Service Broker.)

Per controllare se Service Broker è abilitato, eseguire una query di **è\_broker\_abilitato** colonna il **Sys. Databases** vista del catalogo.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Per abilitare Service Broker, usare la seguente query SQL:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Se questa query viene visualizzato per verificare un deadlock, assicurarsi che non sono presenti applicazioni connesse al database.


Se è stata abilitata la traccia, le tracce verranno visualizzata anche se Service Broker è abilitato.

## <a name="create-a-signalr-application"></a>Creare un'applicazione di SignalR

Creare un'applicazione di SignalR una di queste esercitazioni:

- [Introduzione a SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introduzione a SignalR 2.0 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Successivamente, verrà modificata l'applicazione di chat per il supporto di scalabilità orizzontale con SQL Server. In primo luogo, aggiungere il pacchetto SignalR.SqlServer NuGet al progetto. In Visual Studio, dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Successivamente, aprire il file Startup.cs. Aggiungere il codice seguente per il **configura** metodo:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Distribuire ed eseguire l'applicazione

Preparare le istanze del Server di Windows per distribuire l'applicazione di SignalR.

Aggiungere il ruolo IIS. Includere le funzionalità di "Sviluppo di applicazioni", inclusi il protocollo WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Includere anche il servizio di gestione (elencati in "Strumenti di gestione").

![](scaleout-with-sql-server/_static/image5.png)

**Installare Web Deploy 3.0.** Quando si esegue Gestione IIS, verrà chiesto di installare una piattaforma Web Microsoft oppure è possibile [scaricare il intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). Nel programma di installazione di piattaforma, eseguire la ricerca di distribuzione Web e installare distribuzione Web 3.0

![](scaleout-with-sql-server/_static/image6.png)

Verificare che il servizio gestione Web è in esecuzione. In caso contrario, avviare il servizio. (Se il servizio di gestione Web non viene visualizzato nell'elenco dei servizi di Windows, assicurarsi che il servizio di gestione è installato quando si aggiunge il ruolo IIS.)

Infine, aprire la porta 8172 per TCP. Questa è la porta che utilizza lo strumento di distribuzione Web.

A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server. In Esplora soluzioni fare doppio clic la soluzione e fare clic su **pubblica**.

Per informazioni dettagliate documentazione sulla distribuzione web, vedere [mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Se si distribuisce l'applicazione a due server, è possibile aprire ogni istanza in una finestra distinta del browser e vedere che possano ricevere messaggi SignalR da altro. (Naturalmente, in un ambiente di produzione, i due server sarebbe sit dietro il bilanciamento del carico.)

![](scaleout-with-sql-server/_static/image7.png)

Dopo aver eseguito l'applicazione, è possibile vedere che SignalR ha creato automaticamente tabelle nel database:

![](scaleout-with-sql-server/_static/image8.png)

SignalR gestisce le tabelle. Fino a quando l'applicazione viene distribuita, non eliminare le righe, modificare la tabella e così via.
