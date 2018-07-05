---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Distribuzione Web ASP.NET tramite Visual Studio: distribuzione da riga di comando | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: f079beabccd253ff13c19d3192181ddbdf00b3bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830967"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Distribuzione Web ASP.NET tramite Visual Studio: distribuzione da riga di comando
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come richiamare il web di Visual Studio pipeline dalla riga di comando di pubblicazione. Ciò è utile per scenari in cui si desidera [automatizzare il processo di distribuzione](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) anziché farlo manualmente in Visual Studio, in genere usando una [origine sistema di controllo della versione codice](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Apportare una modifica per la distribuzione

Attualmente, la pagina About vengono visualizzati il codice del modello.

![Pagina About con il codice del modello](command-line-deployment/_static/image1.png)

Che sarà sostituito con il codice che visualizza un riepilogo della registrazione per studenti.

Aprire il *About* pagina, eliminare tutti i commenti all'interno di `MainContent` `Content` elemento e inserire il markup seguente al suo posto:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Eseguire il progetto e selezionare il **sulle** pagina.

![Pagina About (Informazioni)](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Distribuire Test usando la riga di comando

Non distribuisce un'altra modifica di database, in tal caso Disabilita dbDacFx database distribuzione per il database aspnet-ContosoUniversity. Aprire il **pubblica sul Web** guidato e in ognuno dei tre profili, non crittografati di pubblicazione il **aggiornare il Database** casella di controllo la **impostazioni** scheda.

Nella pagina iniziale di Windows 8, cercare **prompt dei comandi per gli sviluppatori per VS2012**.

Fare doppio clic sull'icona **prompt dei comandi per gli sviluppatori per VS2012** e fare clic su **Esegui come amministratore**.

Immettere il comando seguente al prompt dei comandi, sostituendo il percorso del file di soluzione con il percorso al file della soluzione:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild compila la soluzione e la distribuisce nell'ambiente di test.

![Output della riga di comando](command-line-deployment/_static/image3.png)

Aprire un browser e passare a `http://localhost/ContosoUniversity`, quindi scegliere il **sulle** pagina per verificare che la distribuzione è riuscita.

Se è stata creata di studenti nel test, si noterà una pagina vuota sotto la **studente corpo statistiche** intestazione. Passare al **studenti** pagina, fare clic su **aggiungere studente**e aggiungere alcuni studenti e quindi tornare al **sulle** pagina per visualizzare le statistiche degli studenti.

![Informazioni sulla pagina in ambiente di Test](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Opzioni della riga di comando chiave

Il comando immesso passato il percorso del file di soluzione e due proprietà di MSBuild:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>La distribuzione della soluzione rispetto alla distribuzione di progetti singoli

Se si specifica il file della soluzione, tutti i progetti della soluzione da compilare. Se si hanno più progetti web nella soluzione, si applica il comportamento di MSBuild seguente:

- Le proprietà che specificano nella riga di comando vengono passate a ogni progetto. Pertanto, ciascun progetto web deve avere un profilo di pubblicazione con il nome specificato. Se si specifica `/p:PublishProfile=Test`, ciascun progetto web deve avere un profilo di pubblicazione denominato *Test*.
- È possibile pubblicare un progetto è stato quando non viene compilato anche un altro. Per altre informazioni, vedere il thread di stackoverflow [si verifica un errore di MSBuild con due pacchetti](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Se si specifica un singolo progetto invece di una soluzione, è necessario aggiungere un parametro che specifica la versione di Visual Studio. Se si usa Visual Studio 2012 è possibile che la riga di comando sarà simile all'esempio seguente:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Il numero di versione per Visual Studio 2010 è 10,0. Per altre informazioni, vedere [compatibilità dei progetti di Visual Studio e VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) sul blog di Sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Specifica il profilo di pubblicazione

È possibile specificare il profilo di pubblicazione con nome o il percorso completo per il *pubxml* file, come illustrato nell'esempio seguente:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Metodi supportati per la pubblicazione della riga di comando di pubblicazione sul Web

Tre metodi di pubblicazione sono supportati per la pubblicazione della riga di comando:

- `MSDeploy` -Pubblicazione con distribuzione Web.
- `Package` -Pubblicazione mediante la creazione di un pacchetto di distribuzione Web. È necessario installare separatamente il pacchetto del comando di MSBuild che lo crea.
- `FileSystem` -Pubblicazione copiando i file in una cartella specificata.

### <a name="specifying-the-build-configuration-and-platform"></a>Specifica la piattaforma e configurazione della build

La configurazione della build e la piattaforma deve essere impostate in Visual Studio o dalla riga di comando. I profili di pubblicazione includono proprietà che sono denominate `LastUsedBuildConfiguration` e `LastUsedPlatform`, ma non è possibile impostare queste proprietà per determinare come viene compilato il progetto. Per altre informazioni, vedere [MSBuild: come impostare la proprietà di configurazione](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) sul blog di Sayed Hashimi.

## <a name="deploy-to-staging"></a>Distribuire in gestione temporanea

Per distribuire in Azure, è necessario aggiungere la password nella riga di comando. Se è stata salvata la password nel profilo di pubblicazione in Visual Studio, è stato archiviato in forma crittografata nel il *. pubxml* file. Tale file non è accessibile da MSBuild quando si esegue una distribuzione da riga di comando, pertanto è necessario passare la password in un parametro della riga di comando.

1. Copiare la password necessari dal *publishsettings* file scaricato in precedenza per il sito web di staging. La password è il valore della `userPWD` attributo per la distribuzione Web `publishProfile` elemento.

    ![Password di distribuzione Web](command-line-deployment/_static/image5.png)
2. Nella pagina iniziale di Windows 8, cercare **prompt dei comandi per gli sviluppatori per VS2012**e fare clic sull'icona per aprire il prompt dei comandi. (Non è necessario aprirlo come amministratore in questo momento perché non distribuzione in IIS nel computer locale.)
3. Immettere il comando seguente al prompt dei comandi, sostituendo il percorso del file di soluzione con il percorso di file della soluzione e la password con la password:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Si noti che questa riga di comando include un parametro aggiuntivo: `/p:AllowUntrustedCertificate=true`. Come viene scritto in questa esercitazione, il `AllowUntrustedCertificate` deve essere impostata quando pubblica in Azure dalla riga di comando. Quando viene rilasciata la correzione per questo bug, non sarà necessario tale parametro.
4. Aprire un browser e passare all'URL del sito di gestione temporanea e quindi scegliere il **sulle** pagina per verificare che la distribuzione è riuscita.

    Come illustrato in precedenza per l'ambiente di test, potrebbe essere necessario creare alcuni studenti per visualizzare le statistiche sul **sulle** pagina.

## <a name="deploy-to-production"></a>Distribuire nell'ambiente di produzione

Il processo per la distribuzione nell'ambiente di produzione è simile al processo per la gestione temporanea.

1. Copiare la password necessari dal *publishsettings* file scaricato in precedenza per il sito web di produzione.
2. Aprire **prompt dei comandi per gli sviluppatori per VS2012**.
3. Immettere il comando seguente al prompt dei comandi, sostituendo il percorso del file di soluzione con il percorso di file della soluzione e la password con la password:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Per un sito di produzione reale, se si è verificato anche una modifica al database, si usa in genere per copiare il *app\_offline.htm* al sito prima della distribuzione di file ed eliminarlo al termine della distribuzione.
4. Aprire un browser e passare all'URL del sito di gestione temporanea e quindi scegliere il **sulle** pagina per verificare che la distribuzione è riuscita.

## <a name="summary"></a>Riepilogo

È stato distribuito un aggiornamento dell'applicazione tramite la riga di comando.

![Informazioni sulla pagina in ambiente di Test](command-line-deployment/_static/image6.png)

Nella prossima esercitazione, verrà visualizzato un esempio di come estendere il web della pipeline di pubblicazione. L'esempio illustra come distribuire i file che non sono inclusi nel progetto.

> [!div class="step-by-step"]
> [Precedente](deploying-a-database-update.md)
> [Successivo](deploying-extra-files.md)
