---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: la distribuzione della riga di comando | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8446b3fc05e3ef4a5a30c753c989252fd7f1a56f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: la distribuzione della riga di comando
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come richiamare web Visual Studio pipeline dalla riga di comando di pubblicazione. Ciò è utile per scenari in cui si desidera [automatizzare il processo di distribuzione](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) anziché farlo manualmente in Visual Studio, in genere utilizzando un [origine sistema di controllo del codice versione](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Apportare una modifica per la distribuzione

Attualmente, la pagina di informazioni su Visualizza il codice del modello.

![Informazioni sulla pagina con codice di modello](command-line-deployment/_static/image1.png)

Che verrà sostituito con il codice che visualizza un riepilogo della registrazione di studenti.

Aprire il *About* pagina, eliminare tutti i commenti all'interno di `MainContent` `Content` elemento e inserire il seguente markup al suo posto:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Eseguire il progetto e selezionare il **su** pagina.

![Informazioni sulla pagina](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Distribuire i Test dalla riga di comando

La modifica di un altro database, così Disabilita dbDacFx database distribuzione per il database aspnet ContosoUniversity non distribuzione. Aprire il **pubblica sul Web** procedura guidata e in ognuno dei tre profili, deselezionare di pubblicazione il **aggiornamento Database** casella di controllo di **impostazioni** scheda.

Nella pagina iniziale di Windows 8, cercare **prompt dei comandi per sviluppatori per VS2012**.

Fare doppio clic sull'icona **prompt dei comandi per sviluppatori per VS2012** e fare clic su **Esegui come amministratore**.

Immettere il comando seguente al prompt dei comandi, sostituendo il percorso del file di soluzione con il percorso del file di soluzione:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild compila la soluzione e lo distribuisce nell'ambiente di testing.

![Output della riga di comando](command-line-deployment/_static/image3.png)

Aprire un browser e passare a `http://localhost/ContosoUniversity`, quindi fare clic su di **su** pagina per verificare che la distribuzione ha avuto esito positivo.

Se è ancora stato creato alcun studenti nel test, si noterà una pagina vuota sotto il **studente corpo statistiche** intestazione. Passare al **studenti** pagina, fare clic su **studente aggiungere**, aggiungere alcuni studenti e quindi tornare al **su** pagina per visualizzare le statistiche studente.

![Informazioni sulla pagina in ambiente di Test](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Opzioni della riga di comando chiave

Il comando immesso passato il percorso del file di soluzione e due proprietà MSBuild:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>La distribuzione della soluzione e la distribuzione di progetti singoli

Se si specifica il file della soluzione, tutti i progetti nella soluzione da compilare. Se si dispone di più progetti web nella soluzione, si applica il seguente comportamento di MSBuild:

- Le proprietà specificate nella riga di comando vengono passate a ogni progetto. Pertanto, ogni progetto web deve disporre di un profilo di pubblicazione con il nome specificato. Se si specifica `/p:PublishProfile=Test`, ogni progetto web deve disporre di un profilo di pubblicazione denominato *Test*.
- È possibile pubblicare un progetto correttamente quando un altro non compila anche. Per ulteriori informazioni, vedere il thread di stackoverflow [MSBuild non riesce con due pacchetti](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Se si specifica un singolo progetto invece di una soluzione, è necessario aggiungere un parametro che specifica la versione di Visual Studio. Se si utilizza Visual Studio 2012 è possibile che la riga di comando sarà simile all'esempio seguente:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Il numero di versione per Visual Studio 2010 è 10.0. Per ulteriori informazioni, vedere [compatibilità di progetto di Visual Studio e VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) sul blog di Hashimi Sayed.

### <a name="specifying-the-publish-profile"></a>Specifica il profilo di pubblicazione

È possibile specificare il profilo di pubblicazione in base al nome o il percorso completo di *pubxml* file, come illustrato nell'esempio seguente:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Metodi supportati per la pubblicazione della riga di comando di pubblicazione sul Web

Tre metodi di pubblicazione sono supportati per la pubblicazione della riga di comando:

- `MSDeploy`-Pubblicazione tramite distribuzione Web.
- `Package`-Pubblicazione mediante la creazione di un pacchetto di distribuzione Web. È necessario installare separatamente il pacchetto del comando di MSBuild che lo crea.
- `FileSystem`-Pubblicare copiando i file in una cartella specificata.

### <a name="specifying-the-build-configuration-and-platform"></a>Specifica la piattaforma e configurazione di compilazione

La piattaforma e configurazione di compilazione deve essere impostati in Visual Studio o nella riga di comando. I profili di pubblicazione includono proprietà che sono denominate `LastUsedBuildConfiguration` e `LastUsedPlatform`, ma non è possibile impostare queste proprietà per determinare come viene compilato il progetto. Per ulteriori informazioni, vedere [MSBuild: come impostare la proprietà di configurazione](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) nel blog del Hashimi Sayed.

## <a name="deploy-to-staging"></a>Distribuzione di gestione temporanea

Per distribuire in Azure, è necessario aggiungere la password per la riga di comando. Se la password è stato salvato nel profilo di pubblicazione in Visual Studio, il messaggio è stato archiviato in forma crittografata nel il *. pubxml.user* file. Tale file non è accessibile da MSBuild quando si esegue una distribuzione della riga di comando, è necessario passare la password in un parametro della riga di comando.

1. Copiare la password necessari dal *publishsettings* file scaricato in precedenza per il sito web di gestione temporanea. La password è il valore di `userPWD` attributo per la distribuzione di Web `publishProfile` elemento.

    ![Password di distribuzione Web](command-line-deployment/_static/image5.png)
2. Nella pagina iniziale di Windows 8, cercare **prompt dei comandi per sviluppatori per VS2012**e fare clic sull'icona per aprire il prompt dei comandi. (Non è necessario aprirlo come amministratore in questo momento perché non distribuzione in IIS nel computer locale.)
3. Immettere il comando seguente al prompt dei comandi, sostituendo il percorso del file di soluzione con il percorso del file di soluzione e la password con la password:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Si noti che questa riga di comando include un parametro aggiuntivo: `/p:AllowUntrustedCertificate=true`. Come viene scritto in questa esercitazione, il `AllowUntrustedCertificate` deve essere impostata quando pubblica in Azure dalla riga di comando. Quando viene rilasciata la correzione di bug, non è necessario tale parametro.
4. Aprire un browser e passare all'URL del sito di gestione temporanea e quindi fare clic su di **su** pagina per verificare che la distribuzione ha avuto esito positivo.

    Come illustrato in precedenza per l'ambiente di test, è necessario creare alcuni studenti per visualizzare le statistiche di **su** pagina.

## <a name="deploy-to-production"></a>Distribuire nell'ambiente di produzione

Il processo di distribuzione nell'ambiente di produzione è simile al processo per la gestione temporanea.

1. Copiare la password necessari dal *publishsettings* file scaricato in precedenza per il sito web di produzione.
2. Aprire **prompt dei comandi per sviluppatori per VS2012**.
3. Immettere il comando seguente al prompt dei comandi, sostituendo il percorso del file di soluzione con il percorso del file di soluzione e la password con la password:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Per un sito di produzione reali, se si è verificato anche una modifica al database, è in genere necessario copiare il *app\_offline.htm* al sito prima della distribuzione di file ed eliminarlo al termine della distribuzione.
4. Aprire un browser e passare all'URL del sito di gestione temporanea e quindi fare clic su di **su** pagina per verificare che la distribuzione ha avuto esito positivo.

## <a name="summary"></a>Riepilogo

Ora stato distribuito un aggiornamento dell'applicazione tramite la riga di comando.

![Informazioni sulla pagina in ambiente di Test](command-line-deployment/_static/image6.png)

Nella prossima esercitazione, verrà visualizzato un esempio di come estendere web pipeline di pubblicazione. L'esempio mostra come distribuire i file che non sono inclusi nel progetto.

>[!div class="step-by-step"]
[Precedente](deploying-a-database-update.md)
[Successivo](deploying-extra-files.md)
