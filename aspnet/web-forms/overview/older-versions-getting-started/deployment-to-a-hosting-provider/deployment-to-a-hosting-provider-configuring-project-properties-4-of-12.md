---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: configurazione delle proprietà del progetto - 4 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4aa1e55f21e74ae8da67d6d13c35a8de5693ec3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396268"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: configurazione delle proprietà del progetto - 4 di 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e Mostra come distribuire in App Web di servizio App di Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

Alcune opzioni di distribuzione configurate nelle proprietà di progetto che vengono archiviate nel file di progetto (il *file con estensione csproj* oppure *vbproj* file). Nella maggior parte dei casi, i valori predefiniti di queste impostazioni siano quelle corrette, ma è possibile usare la **proprietà del progetto** dell'interfaccia utente incorporati in Visual Studio per lavorare con queste impostazioni se è necessario modificarle. In questa esercitazione esaminare le impostazioni di distribuzione nel **proprietà del progetto**. È anche possibile creare un file segnaposto che fa sì che una cartella vuota per la distribuzione.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Configurazione delle impostazioni di distribuzione nella finestra delle proprietà del progetto

La maggior parte delle impostazioni che influiscono sulle operazioni eseguite durante la distribuzione sono inclusi nel profilo di pubblicazione, come verrà illustrato nelle esercitazioni seguenti. Alcune impostazioni che è necessario essere consapevoli di si trovano nel **pubblicazione/creazione pacchetto** schede della finestra di **le proprietà del progetto** finestra. Queste impostazioni vengono specificate per ogni configurazione della build, vale a dire, è possibile avere impostazioni diverse per una build di rilascio è superiore al numero di build di Debug.

In **Esplora soluzioni**, fare doppio clic il **ContosoUniversity** progetto, selezionare **proprietà**e quindi selezionare il **pubblicazione/creazione pacchetto Web**scheda.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Quando viene visualizzata la finestra, per impostazione predefinita che mostra le impostazioni per qualsiasi configurazione della build è attualmente attivo per la soluzione. Se il **Configuration** casella non è segnalato **attivo (versione)**, selezionare **versione** per visualizzare le impostazioni per la configurazione della build di rilascio. Si distribuirà le build di rilascio per gli ambienti di test e di produzione.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Con **attiva (rilascio)** oppure **rilascio** selezionata, vengono visualizzati i valori che hanno effetto quando si distribuisce tramite la configurazione della build di rilascio:

- Nel **gli elementi da distribuire** finestra **solo i file necessari per eseguire l'applicazione** sia selezionata. Altre opzioni sono **tutti i file del progetto** oppure **tutti i file nella cartella di progetto**. Se si lascia invariata la selezione predefinita è evitare la distribuzione di file del codice sorgente, ad esempio. Questa impostazione è il motivo per cui le cartelle che contengono i file binari di SQL Server Compact dovevano essere incluso nel progetto. Per altre informazioni su questa impostazione, vedere **perché non tutti i file nella cartella del progetto vengono distribuiti?** nelle [domande frequenti sulla distribuzione di ASP.NET Web Application progetto](https://msdn.microsoft.com/library/ee942158.aspx).
- **I simboli di debug generate Exclude** sia selezionata. È non verrà eseguito il debug quando si usa questa configurazione della build.
- **Escludere i file dall'App\_cartella dati** non è selezionata. Il file per il database di appartenenze SQL Server Compact è in tale cartella ed è necessario distribuirlo. Quando si distribuiscono gli aggiornamenti che non includono le modifiche del database, è possibile selezionare questa casella di controllo.
- **Precompilare l'applicazione prima della pubblicazione** non è selezionata. Nella maggior parte degli scenari, non è necessario per la precompilazione progetti applicazione web. Per altre informazioni su questa opzione, vedere [pubblicazione/creazione pacchetto Web scheda, le proprietà del progetto](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) e [precompilare le impostazioni di finestra di dialogo Avanzate](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Includere tutti i database configurati nella scheda Pubblicazione/creazione pacchetto SQL** è selezionata, questa opzione ha tuttavia alcun effetto a questo punto perché non si configurando il **pubblicazione/creazione pacchetto SQL** scheda. Tale scheda è per un metodo di distribuzione di database legacy che era l'unica opzione per la distribuzione di database di SQL Server. Si userà il **pubblicazione/creazione pacchetto SQL** scheda le [migrazione a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) esercitazione.
- Il **impostazioni del pacchetto di distribuzione Web** sezione non è valido perché si usa un solo clic pubblicare in queste esercitazioni.

Modifica il **configurazione** casella a discesa di Debug per visualizzare le impostazioni predefinite per le compilazioni di Debug. I valori sono uguali, tranne **Escludi simboli di debug generati** è deselezionata, in modo che è possibile eseguire il debug quando si distribuisce una build di Debug.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Rendendo che che viene distribuita nella cartella Elmah

Come illustrato nell'esercitazione precedente, il [pacchetto Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornisce funzionalità per la registrazione e segnalazione errori. Nell'applicazione di Contoso University Elmah è stato configurato per archiviare i dettagli dell'errore in una cartella denominata *Elmah*:

![Cartella Elmah](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Esclusione di specifici file o cartelle dalla distribuzione è un requisito comune; un altro esempio sarebbe una cartella in cui gli utenti possono caricare i file. Non desidera che i file di log o caricato i file creati nell'ambiente di sviluppo per la distribuzione nell'ambiente di produzione. E se si distribuisce un aggiornamento nell'ambiente di produzione non è auspicabile il processo di distribuzione per eliminare i file esistenti nell'ambiente di produzione. (A seconda del modo in cui si imposta un'opzione di distribuzione, se esiste un file nel sito di destinazione ma non dal sito di origine durante la distribuzione, distribuzione Web lo elimina dalla destinazione.)

Come si è visto in precedenza in questa esercitazione, il **gli elementi da distribuire** opzione il **pubblicazione/creazione pacchetto Web** scheda è impostata su **solo file necessari per eseguire questa applicazione**. Di conseguenza, i file di log creati da Elmah in fase di sviluppo non verranno distribuiti, che è ciò che si desidera ottenere. (Per essere distribuita, dovranno essere inclusi nel progetto e i relativi **Build Action** proprietà dovrà essere impostata su **contenuto**. Per altre informazioni, vedere **perché non tutti i file nella cartella del progetto vengono distribuiti?** nelle [domande frequenti sulla distribuzione di ASP.NET Web Application progetto](https://msdn.microsoft.com/library/ee942158.aspx)). Tuttavia, distribuzione Web non verrà creata una cartella nel sito di destinazione a meno che non è presente almeno un file da copiare ad esso. Pertanto, si aggiungerà un *txt* file nella cartella che funga da segnaposto in modo che verrà copiata nella cartella.

Nelle **Esplora soluzioni**, fare doppio clic il *Elmah* cartella, selezionare **Aggiungi nuovo elemento**e creare un file denominato *assegnategli*. Inserire il testo seguente: "Si tratta di un file segnaposto per garantire che la cartella viene distribuita". e salvare il file. Questo è tutto è necessario eseguire per fare in modo che Visual Studio distribuisce il file e la cartella è, in quanto il **Build Action** proprietà di *. txt* file è impostato su **contenuto**per impostazione predefinita.

A questo punto è stata completata a tutte le attività di configurazione di distribuzione. Nella prossima esercitazione, verrà distribuire il sito Contoso University all'ambiente di prova e testarlo.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
