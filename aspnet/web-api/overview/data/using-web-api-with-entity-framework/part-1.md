---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Using Web API 2 con Entity Framework 6 | Documenti Microsoft
author: MikeWasson
description: "In questa esercitazione verrà fornite le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end. L'esercitazione Usa Entity Framework 6 per il layout dei dati..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: cceefa128f90b4c3e23dd31119f44e6ffc55f46f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-web-api-2-with-entity-framework-6"></a>Using Web API 2 con Entity Framework 6
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](https://github.com/MikeWasson/BookService)

> In questa esercitazione verrà fornite le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end. L'esercitazione Usa Entity Framework 6 per il livello dati e Knockout.js per l'applicazione di JavaScript sul lato client. Nell'esercitazione viene inoltre illustrato come distribuire l'app in App Web di servizio App di Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - 2.1 API Web
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


In questa esercitazione Usa ASP.NET Web API 2 con Entity Framework 6 per creare un'applicazione web che consente di modificare un database back-end. Di seguito è riportata una schermata dell'applicazione che verrà creato.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

L'app Usa una progettazione di applicazioni a pagina singola (SPA). "L'applicazione a pagina singola" è il termine generale per un'applicazione web che consente di caricare una singola pagina HTML e quindi aggiorna la pagina in modo dinamico, invece di caricare nuove pagine. Dopo il caricamento della pagina iniziale, l'applicazione comunica con il server tramite le richieste AJAX. Il AJAX richiede dati JSON restituito, che l'app Usa per aggiornare l'interfaccia utente.

AJAX non è nuovo, ma esistono oggi Framework JavaScript che rendono più semplice compilare e gestire un'applicazione SPA sofisticata di grandi dimensioni. Questa esercitazione viene utilizzato [Knockout.js](http://knockoutjs.com/), ma è possibile utilizzare qualsiasi framework JavaScript di client.

Di seguito sono i blocchi principali per l'app:

- ASP.NET MVC consente di creare la pagina HTML.
- API Web ASP.NET gestisce le richieste AJAX e restituisce i dati JSON.
- Knockout.js Associa dati degli elementi HTML per i dati JSON.
- Entity Framework comunica con il database.

## <a name="see-this-app-running-on-azure"></a>Vedere l'App in esecuzione in Azure

Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale? È possibile distribuire una versione completa dell'app al proprio account Azure, facendo semplicemente clic sul pulsante seguente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

È necessario un account Azure per distribuire questa soluzione in Azure. Se si dispone già di un account, sono disponibili le opzioni seguenti:

- [Aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -si ottiene crediti è possibile utilizzare per provare i servizi di Azure a pagamento e anche dopo l'uso massimo è possibile mantenere l'account e utilizzare senza servizi di Azure.
- [Attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Your sottoscrizione MSDN offre crediti ogni mese in cui è possibile utilizzare per i servizi di Azure a pagamento.

## <a name="create-the-project"></a>Creare il progetto

Aprire Visual Studio. Dal **File** dal menu **New**, quindi selezionare **progetto**. (Oppure fare clic su **nuovo progetto** nella pagina Start.)

Nel **nuovo progetto** finestra di dialogo, fare clic su **Web** nel riquadro a sinistra e **applicazione Web ASP.NET** nel riquadro centrale. Denominare il progetto BookService e fare clic su **OK**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **API Web** modello.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Se si desidera ospitare il progetto in un servizio App di Azure, lasciare il **Host nel cloud** casella di controllo.

Fare clic su **OK** per creare il progetto.

## <a name="configure-azure-settings-optional"></a>Configurare le impostazioni di Azure (facoltative)

Se si lascia il **Host nel Cloud** opzione selezionata, Visual Studio verrà richiesto di accedere a Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Dopo l'accesso ad Azure, Visual Studio viene richiesto di configurare l'app web. Immettere un nome per il sito, selezionare la sottoscrizione di Azure e selezionare un'area geografica. In **server di Database**selezionare **Crea nuovo server**. Immettere un nome utente amministratore e una password.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

>[!div class="step-by-step"]
[avanti](part-2.md)
