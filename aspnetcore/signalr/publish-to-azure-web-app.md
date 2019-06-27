---
title: Pubblicare un ASP.NET Core SignalR app servizio App di Azure
author: bradygaster
description: Informazioni su come pubblicare un'app ASP.NET Core SignalR in servizio App di Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406107"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a>Pubblicare un ASP.NET Core SignalR app servizio App di Azure

Da [Brady Gaster](https://twitter.com/bradygaster)

[Servizio App di Azure](/azure/app-service/app-service-web-overview) è un [Microsoft il cloud computing](https://azure.microsoft.com/) servizio della piattaforma per l'hosting di App web, tra cui ASP.NET Core.

> [!NOTE]
> Questo articolo si riferisce alla pubblicazione di un'app ASP.NET Core SignalR da Visual Studio. Per altre informazioni, vedere [servizio SignalR per Azure](https://azure.microsoft.com/services/signalr-service).

## <a name="publish-the-app"></a>Pubblicare l'app

Questo articolo illustra pubblicazione usando gli strumenti in Visual Studio. Gli utenti di Visual Studio Code è possono usare [CLI Azure](/cli/azure) comandi per pubblicare le App in Azure. Per altre informazioni, vedere [pubblicare un'app ASP.NET Core in Azure con gli strumenti da riga di comando](/azure/app-service/app-service-web-get-started-dotnet).

1. Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica**.

1. Verificare che **servizio App** e **Crea nuovo** selezionati nel **selezionare una destinazione di pubblicazione** finestra di dialogo.

1. Selezionare **Crea profilo** dalle **Publish** pulsante elenco a discesa.

   Immettere le informazioni descritte nella tabella riportata di seguito il **Crea servizio App** finestra di dialogo e selezionare **crea**.

   | Elemento               | Descrizione |
   | ------------------ | ----------- |
   | **Name**           | Nome univoco dell'app. |
   | **Sottoscrizione**   | Sottoscrizione di Azure usati dall'app. |
   | **Gruppo di risorse** | Gruppo di risorse correlate a cui appartiene l'app. |
   | **Piano di hosting**   | Piano tariffario per l'app web. |

1. Selezionare il **servizio Azure SignalR** nel **dipendenze** > **Aggiungi** elenco a discesa:

   ![La selezione del servizio Azure SignalR visualizzata nell'elenco a discesa Aggiungi area di dipendenze](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. Nel **servizio Azure SignalR** finestra di dialogo, seleziona **creare una nuova istanza di servizio Azure SignalR**.

1. Fornire una **Name**, **gruppo di risorse**, e **percorso**. Tornare al **servizio Azure SignalR** finestra di dialogo e selezionare **Add**.

Visual Studio completa le attività seguenti:

* Crea un profilo di pubblicazione che contiene le impostazioni di pubblicazione.
* Crea un' *App Web di Azure* con i dettagli forniti.
* Pubblica l'app.
* Avvia un browser, che carica l'app web.

Il formato dell'URL dell'app è `{APP SERVICE NAME}.azurewebsites.net`. Ad esempio, un'app denominata `SignalRChatApp` dispone di un URL di `https://signalrchatapp.azurewebsites.net`.

Se un HTTP *502.2 - Gateway non valido* errore si verifica quando si distribuisce un'app destinata a una versione di .NET Core preview, vedere [versione di anteprima di distribuzione di ASP.NET Core nel servizio App di Azure di](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) per risolvere il problema.

## <a name="configure-the-app-in-azure-app-service"></a>Configurare l'app nel servizio App di Azure

> [!NOTE]
> *In questa sezione si applica solo alle App non usa il servizio Azure SignalR.*
>
> Se l'app Usa il servizio Azure SignalR, il servizio App non richiede la configurazione dell'affinità Application Request Routing (ARR) e Web socket descritte in questa sezione. I client si connettono i WebSocket per il servizio Azure SignalR, non direttamente all'app.

Per le app ospitate senza il servizio Azure SignalR, abilitare:

* [Affinità ARR](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) per indirizzare le richieste da un utente di tornare all'istanza del servizio App stesso. L'impostazione predefinita è **su**.
* [Web socket](xref:fundamentals/websockets) per consentire il trasporto WebSocket alla funzione. L'impostazione predefinita è **disattivata**.

1. Nel portale di Azure, passare all'app web in **servizi App**.
1. Aprire **Configuration** > **impostazioni generali**.
1. Impostare **Web socket** al **su**.
1. Verificare che **affinità ARR** è impostata su **su**.

## <a name="app-service-plan-limits"></a>Piano di servizio App limita

Web socket e altri tipi di trasporto sono limitati basati sul piano di servizio App selezionato. Per altre informazioni, vedere la *servizi Cloud di Azure limita* e *limiti del servizio App* sezioni del [sottoscrizione di Azure e limiti, quote e vincoli](/azure/azure-subscription-service-limits#app-service-limits) articolo.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Che cos'è servizio Azure SignalR?](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Pubblicare un'app ASP.NET Core in Azure con gli strumenti da riga di comando](/azure/app-service/app-service-web-get-started-dotnet)
* [Ospitare e distribuire le App in anteprima di ASP.NET Core in Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
