---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: "Distribuzione Web ASP.NET utilizzando Visual Studio: le proprietà del progetto | Documenti Microsoft"
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: 85b6dbcc8d40c168a49513ef6b549f9ec7fa5097
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: le proprietà del progetto
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).


## <a name="overview"></a>Panoramica

Alcune opzioni di distribuzione configurate nelle proprietà di progetto che vengono archiviate nel file di progetto (la *csproj* o *vbproj* file). Nella maggior parte dei casi, i valori predefiniti di queste impostazioni siano quelle desiderate, ma è possibile utilizzare il **le proprietà del progetto** dell'interfaccia utente integrate in Visual Studio per funzionare con queste impostazioni se è necessario modificarle. In questa esercitazione si rivedere le impostazioni di distribuzione in **le proprietà del progetto**. È inoltre possibile creare un file di segnaposto che ha causato una cartella vuota da distribuire.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Configurare le impostazioni di distribuzione nella finestra Proprietà progetto

La maggior parte delle impostazioni che influiscono sulle operazioni eseguite durante la distribuzione sono inclusi nel profilo di pubblicazione, come si vedrà nelle esercitazioni seguenti. Alcune impostazioni che è necessario essere consapevoli di si trovano nel **pubblicazione/creazione pacchetto** schede del **le proprietà del progetto** finestra. Queste impostazioni sono specificate per ogni configurazione di compilazione, vale a dire, si può avere impostazioni diverse per una build di rilascio che per una build di Debug.

In **Esplora**, fare doppio clic su di **ContosoUniversity** progetto, selezionare **proprietà**e quindi selezionare il **pubblicazione/creazione pacchetto Web**scheda.

![Scheda Pubblicazione/creazione pacchetto Web](project-properties/_static/image1.png)

Quando viene visualizzata la finestra, per impostazione predefinita che mostra impostazioni per qualsiasi configurazione di compilazione è attualmente attivo per la soluzione. Se il **configurazione** casella non è segnalato **attiva (rilascio)**selezionare **versione** per visualizzare le impostazioni per la configurazione della build di rilascio. Distribuire le build di rilascio per gli ambienti di test e di produzione.

![Selezione configurazione di compilazione di rilascio](project-properties/_static/image2.png)

Con **attiva (rilascio)** o **versione** selezionata, vengono visualizzati i valori che vengono applicati quando si distribuisce tramite la configurazione della build di rilascio:

- Nel **gli elementi da distribuire** casella **solo i file necessari per eseguire l'applicazione** è selezionata. Altre opzioni sono **tutti i file in questo progetto** o **tutti i file nella cartella di progetto**. Lasciando invariata la selezione predefinita evitare la distribuzione di file del codice sorgente, ad esempio. Questa impostazione è il motivo perché le cartelle che contengono i file binari di SQL Server Compact devono essere incluso nel progetto. Per ulteriori informazioni su questa impostazione, vedere **perché non tutti i file nella cartella di progetto vengono distribuiti?** in [domande frequenti sulla distribuzione di ASP.NET Web applicazione progetto](https://msdn.microsoft.com/library/ee942158.aspx).
- **I simboli di debug Exclude generato** è selezionata. È non verrà eseguito il debug quando si utilizza questa configurazione della build.
- **Includere tutti i database configurati nella scheda Pubblicazione/creazione pacchetto SQL** è selezionata. Specifica se Visual Studio verrà distribuito al database e i file. Anche se la casella di controllo etichetta menziona solo il **pubblicazione/creazione pacchetto SQL** scheda, deselezionare questa casella di controllo Disattiva anche la distribuzione di database che è configurata nel profilo di pubblicazione. È necessario effettuare che in un secondo momento, pertanto la casella di controllo deve rimanere selezionata. Il **pubblicazione/creazione pacchetto SQL** scheda viene usata per un metodo che non è possibile utilizzare queste esercitazioni di pubblicazione di database legacy.
- Il **impostazioni pacchetto di distribuzione Web** sezione non è applicabile poiché si utilizza un solo clic pubblicare in queste esercitazioni.

Modifica il **configurazione** casella a discesa di Debug per visualizzare le impostazioni predefinite per le build di Debug. I valori sono identici, tranne **Escludi generati i simboli di debug** viene cancellato in modo che è possibile eseguire il debug quando si distribuisce una build di Debug.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Verificare che la cartella Elmah Ottiene distribuita

Come osservato nell'esercitazione precedente, il [pacchetto Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornisce funzionalità per la registrazione e report errori. Nell'applicazione Contoso University Elmah è stato configurato per archiviare i dettagli dell'errore in una cartella denominata *Elmah*:

![Cartella Elmah](project-properties/_static/image3.png)

Esclusione di cartelle o file specifici dalla distribuzione è un requisito comune; un altro esempio sarebbe una cartella in cui gli utenti possono caricare i file. Si desidera i file di log o caricati i file creati nell'ambiente di sviluppo da distribuire nell'ambiente di produzione. E, se si distribuisce un aggiornamento nell'ambiente di produzione non si desidera il processo di distribuzione per eliminare i file presenti nell'ambiente di produzione. (A seconda della configurazione dell'opzione di distribuzione, se esiste un file nel sito di destinazione ma non nel sito di origine quando si distribuisce, distribuzione Web eliminata dalla destinazione.)

Come osservato in precedenza in questa esercitazione, il **gli elementi da distribuire** opzione il **pubblicazione/creazione pacchetto Web** scheda è impostata su **solo i file necessari per eseguire questa applicazione**. Di conseguenza, i file di log creati da Elmah in fase di sviluppo non verranno distribuiti, ovvero che si desidera ottenere. (Per essere distribuita, dovranno essere inclusi nel progetto e i relativi **azione di compilazione** proprietà dovranno essere impostato su **contenuto**. Per ulteriori informazioni, vedere **perché non tutti i file nella cartella di progetto vengono distribuiti?** in [domande frequenti sulla distribuzione di ASP.NET Web applicazione progetto](https://msdn.microsoft.com/library/ee942158.aspx)). Tuttavia, distribuzione Web non creerà una cartella nel sito di destinazione se non è presente almeno un file da copiare a esso. Pertanto, si aggiungerà un *. txt* file nella cartella di agire come un segnaposto in modo che verrà copiata nella cartella.

In **Esplora**, fare doppio clic su di *Elmah* cartella, selezionare **Aggiungi nuovo elemento**e creare un file di testo denominato *assegnategli*. Inserire il testo seguente: "This is a un file segnaposto per garantire che la cartella Ottiene distribuita". E salvare il file. Questo è tutto è necessario eseguire per assicurarsi che Visual Studio distribuisce il file e la cartella è, perché il **azione di compilazione** proprietà di *. txt* file è impostato su **contenuto**per impostazione predefinita.

## <a name="summary"></a>Riepilogo

Tutte le attività di configurazione di distribuzione sono stati completati. Nella prossima esercitazione, verrà di distribuire il sito Contoso University nell'ambiente di testing e di testarlo.

>[!div class="step-by-step"]
[Precedente](web-config-transformations.md)
[Successivo](deploying-to-iis.md)
