---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Pubblicare l'App Azure servizio App di Azure | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 08994815cb339800619caacdcb8d717e9986f9d5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Pubblicare l'App Azure servizio App di Azure
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](https://github.com/MikeWasson/BookService)

Come ultimo passaggio, si pubblicherà l'applicazione in Azure. In Esplora soluzioni, fare clic sul progetto e selezionare **pubblica**.

![](part-10/_static/image1.png)

Fare clic su **pubblica** richiama il **pubblica sul Web** finestra di dialogo. Se è stata selezionata **Host nel Cloud** quando si crea il progetto, quindi la connessione e le impostazioni sono già configurate. In tal caso, scegliere il **impostazioni** e controllare &quot;eseguire le migrazioni Code First&quot;. (Se non è stata selezionata **Host nel Cloud** all'inizio, quindi seguire i passaggi di [nella sezione successiva](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Per distribuire l'app, fare clic su **pubblica**. È possibile visualizzare lo stato nella pubblicazione di **attività di pubblicazione Web** finestra. (Dal **vista** dal menu **altre finestre**, quindi selezionare **attività di pubblicazione Web**.)

![](part-10/_static/image4.png)

Al termine Visual Studio distribuisce l'app, il browser predefinito verrà aperta automaticamente per l'URL del sito Web distribuito e l'applicazione creata è in esecuzione nel cloud. L'URL nella barra degli indirizzi del browser mostra che il sito viene caricato da Internet.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>La distribuzione in un nuovo sito Web

Se non si seleziona **Host nel Cloud** quando si crea il progetto, è possibile configurare una nuova app web. In Esplora soluzioni, fare clic sul progetto e selezionare **pubblica**. Selezionare il **profilo** scheda e fare clic su **siti Web di Microsoft Azure**. Se non è attualmente effettuato l'accesso ad Azure, verrà richiesto di accedere.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

Nel **i siti Web esistenti** finestra di dialogo, fare clic su **New**.

![](part-10/_static/image9.png)

Immettere un nome di sito. Selezionare la sottoscrizione di Azure e l'area. In **server di Database**selezionare **creare un nuovo Server**, oppure selezionare un server esistente. Scegliere **Crea**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Fare clic su di **impostazioni** e controllare &quot;eseguire le migrazioni Code First&quot;. Quindi fare clic su **pubblica**.

>[!div class="step-by-step"]
[Precedente](part-9.md)
