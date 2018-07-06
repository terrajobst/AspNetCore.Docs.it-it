---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Pubblicare l'App Azure servizio App di Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 66dde7b54ce084eed873afae56fd686d0dc8795f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808227"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Pubblicare l'App Azure servizio App di Azure
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](https://github.com/MikeWasson/BookService)

Come ultimo passaggio, si pubblicherà l'applicazione in Azure. In Esplora soluzioni fare clic sul progetto e selezionare **pubblica**.

![](part-10/_static/image1.png)

Facendo clic **Publish** richiama le **pubblica sul Web** finestra di dialogo. Se è stata selezionata **ospita nel Cloud** quando è stato creato prima di tutto il progetto, quindi la connessione e le impostazioni sono già configurate. In tal caso, semplicemente fare clic sui **le impostazioni** scheda e selezionare &quot;Esegui migrazioni Code First&quot;. (Se non è stata selezionata **ospita nel Cloud** all'inizio, quindi seguire i passaggi descritti nel [nella sezione successiva](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Per distribuire l'app, fare clic su **pubblica**. È possibile visualizzare lo stato della pubblicazione nel **attività pubblicazione sul Web** finestra. (Dal **View** dal menu **Other Windows**, quindi selezionare **attività pubblicazione sul Web**.)

![](part-10/_static/image4.png)

Quando Visual Studio ha completato la distribuzione dell'app, il browser predefinito verrà aperto automaticamente l'URL del sito Web distribuito e l'applicazione creata è in esecuzione nel cloud. L'URL nella barra degli indirizzi del browser mostra che il sito viene caricato da Internet.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Distribuzione in un nuovo sito Web

Se non si seleziona **ospita nel Cloud** quando si crea il progetto, è possibile configurare ora una nuova app web. In Esplora soluzioni fare clic sul progetto e selezionare **pubblica**. Selezionare il **profilo** scheda e fare clic su **siti Web di Microsoft Azure**. Se non sono attualmente connessi ad Azure, verrà richiesto di accedere.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

Nel **siti Web esistenti** finestra di dialogo, fare clic su **New**.

![](part-10/_static/image9.png)

Immettere un nome di sito. Selezionare la sottoscrizione di Azure e l'area. Sotto **passava**, selezionare **Crea nuovo Server**, o selezionare un server esistente. Scegliere **Crea**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Scegliere il **le impostazioni** scheda e selezionare &quot;Esegui migrazioni Code First&quot;. Quindi fare clic su **pubblica**.

> [!div class="step-by-step"]
> [Precedente](part-9.md)
