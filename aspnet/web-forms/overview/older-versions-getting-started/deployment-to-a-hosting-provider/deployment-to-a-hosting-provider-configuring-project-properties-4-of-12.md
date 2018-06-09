---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: configurazione delle proprietà del progetto - 4 di 12 | Documenti Microsoft"
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: b8352c1832ffc79db93b6324dd673afaff6b0d74
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "30887200"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: configurazione delle proprietà del progetto - 4 di 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione di serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire l'App Web di servizio App di Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

Alcune opzioni di distribuzione configurate nelle proprietà di progetto che vengono archiviate nel file di progetto (la *csproj* o *vbproj* file). Nella maggior parte dei casi, i valori predefiniti di queste impostazioni siano quelle desiderate, ma è possibile utilizzare il **le proprietà del progetto** dell'interfaccia utente integrate in Visual Studio per funzionare con queste impostazioni se è necessario modificarle. In questa esercitazione si rivedere le impostazioni di distribuzione in **le proprietà del progetto**. È inoltre possibile creare un file di segnaposto che ha causato una cartella vuota da distribuire.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Configurazione delle impostazioni di distribuzione nella finestra Proprietà progetto

La maggior parte delle impostazioni che influiscono sulle operazioni eseguite durante la distribuzione sono inclusi nel profilo di pubblicazione, come si vedrà nelle esercitazioni seguenti. Alcune impostazioni che è necessario essere consapevoli di si trovano nel **pubblicazione/creazione pacchetto** schede del **le proprietà del progetto** finestra. Queste impostazioni sono specificate per ogni configurazione di compilazione, vale a dire, si può avere impostazioni diverse per una build di rilascio che per una build di Debug.

In **Esplora**, fare doppio clic su di **ContosoUniversity** progetto, selezionare **proprietà**e quindi selezionare il **pubblicazione/creazione pacchetto Web**scheda.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Quando viene visualizzata la finestra, per impostazione predefinita che mostra impostazioni per qualsiasi configurazione di compilazione è attualmente attivo per la soluzione. Se il **configurazione** casella non è segnalato **attiva (rilascio)** selezionare **versione** per visualizzare le impostazioni per la configurazione della build di rilascio. Distribuire le build di rilascio per gli ambienti di test e di produzione.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Con **attiva (rilascio)** o **versione** selezionata, vengono visualizzati i valori che vengono applicati quando si distribuisce tramite la configurazione della build di rilascio:

- Nel **gli elementi da distribuire** casella **solo i file necessari per eseguire l'applicazione** è selezionata. Altre opzioni sono **tutti i file in questo progetto** o **tutti i file nella cartella di progetto**. Lasciando invariata la selezione predefinita evitare la distribuzione di file del codice sorgente, ad esempio. Questa impostazione è il motivo perché le cartelle che contengono i file binari di SQL Server Compact devono essere incluso nel progetto. Per ulteriori informazioni su questa impostazione, vedere **perché non tutti i file nella cartella di progetto vengono distribuiti?** in [domande frequenti sulla distribuzione di ASP.NET Web applicazione progetto](https://msdn.microsoft.com/library/ee942158.aspx).
- **I simboli di debug Exclude generato** sia selezionata. È non verrà eseguito il debug quando si utilizza questa configurazione della build.
- **Escludere i file dall'App\_cartella dati** non è selezionata. È necessario distribuire il file di SQL Server Compact per il database delle appartenenze è in tale cartella. Quando si distribuiscono gli aggiornamenti che non includono modifiche del database, occorre selezionare questa casella di controllo.
- **Precompilazione di questa applicazione prima della pubblicazione** non è selezionata. Nella maggior parte degli scenari, non è necessario per la precompilazione progetti applicazione web. Per ulteriori informazioni su questa opzione, vedere [scheda di pubblicazione/creazione pacchetto Web, le proprietà del progetto](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) e [precompilare le impostazioni di finestra di dialogo Avanzate](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Includere tutti i database configurati nella scheda Pubblicazione/creazione pacchetto SQL** è selezionata, questa opzione ha tuttavia alcun effetto ora perché non configurando il **pubblicazione/creazione pacchetto SQL** scheda. Tale scheda è per un metodo di distribuzione di database legacy che consentono di essere l'unica opzione per la distribuzione di database di SQL Server. Si userà il **pubblicazione/creazione pacchetto SQL** nella scheda il [la migrazione a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) esercitazione.
- Il **impostazioni pacchetto di distribuzione Web** sezione non è applicabile poiché si utilizza un solo clic pubblicare in queste esercitazioni.

Modifica il **configurazione** casella a discesa di Debug per visualizzare le impostazioni predefinite per le build di Debug. I valori sono identici, tranne **Escludi generati i simboli di debug** viene cancellato in modo che è possibile eseguire il debug quando si distribuisce una build di Debug.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Rendendo che che viene distribuita nella cartella Elmah

Come osservato nell'esercitazione precedente, il [pacchetto Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornisce funzionalità per la registrazione e report errori. Nell'applicazione Contoso University Elmah è stato configurato per archiviare i dettagli dell'errore in una cartella denominata *Elmah*:

![Cartella Elmah](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Esclusione di cartelle o file specifici dalla distribuzione è un requisito comune; un altro esempio sarebbe una cartella in cui gli utenti possono caricare i file. Si desidera i file di log o caricati i file creati nell'ambiente di sviluppo da distribuire nell'ambiente di produzione. E, se si distribuisce un aggiornamento nell'ambiente di produzione non si desidera il processo di distribuzione per eliminare i file presenti nell'ambiente di produzione. (A seconda della configurazione dell'opzione di distribuzione, se esiste un file nel sito di destinazione ma non nel sito di origine quando si distribuisce, distribuzione Web eliminata dalla destinazione.)

Come osservato in precedenza in questa esercitazione, il **gli elementi da distribuire** opzione il **pubblicazione/creazione pacchetto Web** scheda è impostata su **solo i file necessari per eseguire questa applicazione**. Di conseguenza, i file di log creati da Elmah in fase di sviluppo non verranno distribuiti, ovvero che si desidera ottenere. (Per essere distribuita, dovranno essere inclusi nel progetto e i relativi **azione di compilazione** proprietà dovranno essere impostato su **contenuto**. Per ulteriori informazioni, vedere **perché non tutti i file nella cartella di progetto vengono distribuiti?** in [domande frequenti sulla distribuzione di ASP.NET Web applicazione progetto](https://msdn.microsoft.com/library/ee942158.aspx)). Tuttavia, distribuzione Web non creerà una cartella nel sito di destinazione se non è presente almeno un file da copiare a esso. Pertanto, si aggiungerà un *. txt* file nella cartella di agire come un segnaposto in modo che verrà copiata nella cartella.

In **Esplora**, fare doppio clic su di *Elmah* cartella, selezionare **Aggiungi nuovo elemento**e creare un file denominato *assegnategli*. Inserire il testo seguente: "This is a un file segnaposto per garantire che la cartella Ottiene distribuita". E salvare il file. Questo è tutto è necessario eseguire per assicurarsi che Visual Studio distribuisce il file e la cartella è, perché il **azione di compilazione** proprietà di *. txt* file è impostato su **contenuto**per impostazione predefinita.

Tutte le attività di configurazione di distribuzione sono stati completati. Nella prossima esercitazione, verrà di distribuire il sito Contoso University nell'ambiente di testing e di testarlo.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
