---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Distribuzione Web ASP.NET tramite Visual Studio: le proprietà del progetto | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: 86e4e3ef5f126a5bf8abc91c2d5ce3d14b1e1c6c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837665"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Distribuzione Web ASP.NET tramite Visual Studio: le proprietà del progetto
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).


## <a name="overview"></a>Panoramica

Alcune opzioni di distribuzione configurate nelle proprietà di progetto che vengono archiviate nel file di progetto (il *file con estensione csproj* oppure *vbproj* file). Nella maggior parte dei casi, i valori predefiniti di queste impostazioni siano quelle corrette, ma è possibile usare la **proprietà del progetto** dell'interfaccia utente incorporati in Visual Studio per lavorare con queste impostazioni se è necessario modificarle. In questa esercitazione esaminare le impostazioni di distribuzione nel **proprietà del progetto**. È anche possibile creare un file segnaposto che fa sì che una cartella vuota per la distribuzione.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Configurare le impostazioni di distribuzione nella finestra delle proprietà del progetto

La maggior parte delle impostazioni che influiscono sulle operazioni eseguite durante la distribuzione sono inclusi nel profilo di pubblicazione, come verrà illustrato nelle esercitazioni seguenti. Alcune impostazioni che è necessario essere consapevoli di si trovano nel **pubblicazione/creazione pacchetto** schede della finestra di **le proprietà del progetto** finestra. Queste impostazioni vengono specificate per ogni configurazione della build, vale a dire, è possibile avere impostazioni diverse per una build di rilascio è superiore al numero di build di Debug.

In **Esplora soluzioni**, fare doppio clic il **ContosoUniversity** progetto, selezionare **proprietà**e quindi selezionare il **pubblicazione/creazione pacchetto Web**scheda.

![Scheda Pubblicazione/creazione pacchetto Web](project-properties/_static/image1.png)

Quando viene visualizzata la finestra, per impostazione predefinita che mostra le impostazioni per qualsiasi configurazione della build è attualmente attivo per la soluzione. Se il **Configuration** casella non è segnalato **attivo (versione)**, selezionare **versione** per visualizzare le impostazioni per la configurazione della build di rilascio. Si distribuirà le build di rilascio per gli ambienti di test e di produzione.

![Selezione configurazione di build di rilascio](project-properties/_static/image2.png)

Con **attiva (rilascio)** oppure **rilascio** selezionata, vengono visualizzati i valori che hanno effetto quando si distribuisce tramite la configurazione della build di rilascio:

- Nel **gli elementi da distribuire** finestra **solo i file necessari per eseguire l'applicazione** sia selezionata. Altre opzioni sono **tutti i file del progetto** oppure **tutti i file nella cartella di progetto**. Se si lascia invariata la selezione predefinita è evitare la distribuzione di file del codice sorgente, ad esempio. Questa impostazione è il motivo per cui le cartelle che contengono i file binari di SQL Server Compact dovevano essere incluso nel progetto. Per altre informazioni su questa impostazione, vedere **perché non tutti i file nella cartella del progetto vengono distribuiti?** nelle [domande frequenti sulla distribuzione di ASP.NET Web Application progetto](https://msdn.microsoft.com/library/ee942158.aspx).
- **I simboli di debug generate Exclude** sia selezionata. È non verrà eseguito il debug quando si usa questa configurazione della build.
- **Includere tutti i database configurati nella scheda Pubblicazione/creazione pacchetto SQL** sia selezionata. Specifica se Visual Studio distribuirà i database, nonché i file. Anche se la casella di controllo etichetta menziona solo il **pubblicazione/creazione pacchetto SQL** scheda deselezionando questa casella di controllo Disattiva anche la distribuzione di database che è configurata nel profilo di pubblicazione. La procedura è che in un secondo momento, in modo che la casella di controllo deve rimanere selezionata. Il **pubblicazione/creazione pacchetto SQL** scheda viene usata per un database legacy, la pubblicazione di metodo che non è possibile utilizzare queste esercitazioni.
- Il **impostazioni del pacchetto di distribuzione Web** sezione non è valido perché si usa un solo clic pubblicare in queste esercitazioni.

Modifica il **configurazione** casella a discesa di Debug per visualizzare le impostazioni predefinite per le compilazioni di Debug. I valori sono uguali, tranne **Escludi simboli di debug generati** è deselezionata, in modo che è possibile eseguire il debug quando si distribuisce una build di Debug.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Assicurarsi che la cartella Elmah Ottiene distribuita

Come illustrato nell'esercitazione precedente, il [pacchetto Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornisce funzionalità per la registrazione e segnalazione errori. Nell'applicazione di Contoso University Elmah è stato configurato per archiviare i dettagli dell'errore in una cartella denominata *Elmah*:

![Cartella Elmah](project-properties/_static/image3.png)

Esclusione di specifici file o cartelle dalla distribuzione è un requisito comune; un altro esempio sarebbe una cartella in cui gli utenti possono caricare i file. Non desidera che i file di log o caricato i file creati nell'ambiente di sviluppo per la distribuzione nell'ambiente di produzione. E se si distribuisce un aggiornamento nell'ambiente di produzione non è auspicabile il processo di distribuzione per eliminare i file esistenti nell'ambiente di produzione. (A seconda del modo in cui si imposta un'opzione di distribuzione, se esiste un file nel sito di destinazione ma non dal sito di origine durante la distribuzione, distribuzione Web lo elimina dalla destinazione.)

Come si è visto in precedenza in questa esercitazione, il **gli elementi da distribuire** opzione il **pubblicazione/creazione pacchetto Web** scheda è impostata su **solo file necessari per eseguire questa applicazione**. Di conseguenza, i file di log creati da Elmah in fase di sviluppo non verranno distribuiti, che è ciò che si desidera ottenere. (Per essere distribuita, dovranno essere inclusi nel progetto e i relativi **Build Action** proprietà dovrà essere impostata su **contenuto**. Per altre informazioni, vedere **perché non tutti i file nella cartella del progetto vengono distribuiti?** nelle [domande frequenti sulla distribuzione di ASP.NET Web Application progetto](https://msdn.microsoft.com/library/ee942158.aspx)). Tuttavia, distribuzione Web non verrà creata una cartella nel sito di destinazione a meno che non è presente almeno un file da copiare ad esso. Pertanto, si aggiungerà un *txt* file nella cartella che funga da segnaposto in modo che verrà copiata nella cartella.

Nelle **Esplora soluzioni**, fare doppio clic il *Elmah* cartella, selezionare **Aggiungi nuovo elemento**e creare un file di testo denominato *assegnategli*. Inserire il testo seguente: "Si tratta di un file segnaposto per garantire che la cartella viene distribuita". e salvare il file. Questo è tutto è necessario eseguire per fare in modo che Visual Studio distribuisce il file e la cartella è, in quanto il **Build Action** proprietà di *. txt* file è impostato su **contenuto**per impostazione predefinita.

## <a name="summary"></a>Riepilogo

A questo punto è stata completata a tutte le attività di configurazione di distribuzione. Nella prossima esercitazione, verrà distribuire il sito Contoso University all'ambiente di prova e testarlo.

> [!div class="step-by-step"]
> [Precedente](web-config-transformations.md)
> [Successivo](deploying-to-iis.md)
