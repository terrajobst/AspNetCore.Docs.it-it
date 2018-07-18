---
title: Pubblicare un ASP.NET Core SignalR app all'App Web di Azure
author: tdykstra
description: Pubblicare un ASP.NET Core SignalR app all'App Web di Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: b0126771a9ba3a28a7af14adf5b5959c7591e5fb
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095294"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Pubblicare un ASP.NET Core SignalR app a un'App Web di Azure

[App Web di Azure](/azure/app-service/app-service-web-overview) è un [Microsoft il cloud computing](https://azure.microsoft.com/) servizio della piattaforma per l'hosting di App web, tra cui ASP.NET Core.

> [!NOTE]
> Questo articolo si riferisce alla pubblicazione di un'app ASP.NET Core SignalR da Visual Studio. Visita [servizio SignalR per Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) per altre informazioni sull'uso di SignalR in Azure.

## <a name="publish-the-app"></a>Pubblicare l'app

Visual Studio offre strumenti incorporati per la pubblicazione in un'App Web di Azure. Visual Studio Code utente può utilizzare [CLI Azure](/cli/azure) comandi per pubblicare le App in Azure. Questo articolo illustra pubblicazione usando gli strumenti in Visual Studio. Per pubblicare un'app usando Azure CLI, vedere [pubblicare un'app ASP.NET Core in Azure con gli strumenti da riga di comando](xref:tutorials/publish-to-azure-webapp-using-cli).

Pulsante destro del mouse sul progetto in **Esplora soluzioni** e selezionare **Publish**. Verificare che **Crea nuovo** viene archiviato il **selezionare una destinazione di pubblicazione** finestra di dialogo e selezionare **Publish**.

![Selezione destinazione di pubblicazione](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Immettere le informazioni seguenti nel **Crea servizio App** finestra di dialogo e selezionare **crea**.

| Elemento | Descrizione |
| ---- | ----------- |
| **Nome dell'App** | Un nome univoco dell'app. |
| **Sottoscrizione** | Sottoscrizione di Azure usati dall'app. |
| **Gruppo di risorse** | Il gruppo di risorse correlate a cui appartiene l'app.  |
| **Piano di hosting** | Il piano tariffario per l'app web. |

![Crea servizio app](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio completa le attività seguenti:

* Crea un profilo di pubblicazione che contiene le impostazioni di pubblicazione.
* Crea o Usa esistente *App Web di Azure* con i dettagli forniti.
* Pubblica l'app.
* Avvia un browser, con l'app web pubblicata caricato.

Si noti che il formato dell'URL per l'app è *amp;#42;.azurewebsites.NET {nome dell'app} .net*. Ad esempio, un'app denominata `SignalRChattR` dispone di un URL simile a `https://signalrchattr.azurewebsites.net`.

Se si verifica un errore HTTP 502.2, vedere [versione di anteprima di distribuzione di ASP.NET Core nel servizio App di Azure di](xref:host-and-deploy/azure-apps/index) per risolvere il problema.

## <a name="configure-signalr-web-app"></a>Configurare l'app web di SignalR

ASP.NET Core SignalR App che vengono pubblicate come un'App Web di Azure deve disporre [affinità ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) abilitata. [WebSockets](xref:fundamentals/websockets) deve essere abilitata, per consentire il trasporto WebSocket alla funzione.

Nel portale di Azure, passare a **le impostazioni dell'App** per l'app web. Impostare **WebSockets** al **sul**e verificare **affinità ARR** è **su**.

![Impostazioni dell'app Web Azure nel portale di Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets e altri trasporti [sono limitati in base al piano di servizio App](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Risorse correlate

* [Pubblicare un'app ASP.NET Core in Azure con gli strumenti da riga di comando](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Pubblicare un'app ASP.NET Core in Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Ospitare e distribuire le App in anteprima di ASP.NET Core in Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
