---
title: Pubblicare un Core di ASP.NET SignalR app per App Web di Azure
author: rachelappel
description: Pubblicare un Core di ASP.NET SignalR app per App Web di Azure
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: fd7d38ad47d9004db2ae7b5858dc22609943f601
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2018
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Pubblicare un Core di ASP.NET SignalR app a un'App Web di Azure

[Azure Web App](/azure/app-service/app-service-web-overview) è un [Microsoft per il cloud computing](https://azure.microsoft.com/) servizio di piattaforma per l'hosting di applicazioni web, tra cui ASP.NET Core.

## <a name="publish-the-app"></a>Pubblicare l'app

Visual Studio fornisce gli strumenti predefiniti per la pubblicazione in un'App Web di Azure. Visual Studio Code utente può utilizzare [CLI di Azure](/cli/azure) comandi per pubblicare le App in Azure. Questo articolo descrive pubblicazione usando gli strumenti in Visual Studio. Per pubblicare un'app usando Azure CLI, vedere [pubblicare un'app di ASP.NET Core in Azure con gli strumenti da riga di comando](xref:tutorials/publish-to-azure-webapp-using-cli).

Pulsante destro del mouse sul progetto in **Esplora soluzioni** e selezionare **pubblica**. Verificare che **Crea nuovo** viene archiviato il **selezionare una destinazione di pubblicazione** finestra di dialogo, quindi selezionare **pubblica**.

![Selezione destinazione di pubblicazione](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Immettere le informazioni seguenti nel **servizio App di creare** finestra di dialogo e selezionare **crea**.

| Elemento | Descrizione |
| ---- | ----------- |
| **Nome dell'App** | Un nome univoco dell'app. |
| **Sottoscrizione** | La sottoscrizione di Azure che usa l'app. |
| **Gruppo di risorse** | Il gruppo di risorse correlati a cui appartiene l'app.  |
| **Piano di hosting** | Il piano dei prezzi per l'app web. |

![Creare servizio app](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio consente di completare le attività seguenti:

* Crea un profilo di pubblicazione che contiene le impostazioni di pubblicazione.
* Crea o utilizza un oggetto esistente *App Web di Azure* con i dettagli forniti.
* Pubblica l'app.
* Avvia un browser, con l'app web pubblicata caricato.

Si noti il formato dell'URL per l'app è stata *.azurewebsites {nome app} .net*. Ad esempio, un'app denominata `SignalRChattR` dispone di un URL simile `https://signalrchattr.azurewebsites.net`.

Se si verifica un errore HTTP 502.2, vedere [versione di anteprima di distribuire ASP.NET Core servizio App di Azure](xref:host-and-deploy/azure-apps/index) per risolvere il problema.

## <a name="configure-signalr-web-app"></a>Configura app web di SignalR

ASP.NET SignalR Core App pubblicate come un'App Web di Azure deve includere [affinità ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) abilitato. [I WebSocket](xref:fundamentals/websockets) deve essere abilitata, per consentire il trasporto WebSocket alla funzione.

Nel portale di Azure, passare a **le impostazioni dell'App** per l'app web. Impostare **WebSockets** per **sul**e verificare **ARR affinità** è **su**.

![Impostazioni di app Web di Azure nel portale di Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 I WebSocket e degli altri trasporti [sono limitati in base al piano di servizio App](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Risorse correlate

* [Pubblicare un'app di ASP.NET Core in Azure con gli strumenti da riga di comando](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Pubblicare un'app di ASP.NET Core in Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Ospitare e distribuire le app ASP.NET Core anteprima in Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
