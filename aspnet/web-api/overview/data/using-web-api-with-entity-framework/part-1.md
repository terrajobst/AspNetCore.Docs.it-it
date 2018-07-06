---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Using Web API 2 con Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Questa esercitazione insegnerà le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end. L'esercitazione Usa Entity Framework 6 per il layout dei dati...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: bc853413e814e6ef1a44841d114853003d7328f9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827233"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Using Web API 2 con Entity Framework 6
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](https://github.com/MikeWasson/BookService)

> Questa esercitazione insegnerà le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end. L'esercitazione Usa Entity Framework 6 per il livello di dati e Knockout. js per l'applicazione JavaScript lato client. L'esercitazione illustra anche come distribuire l'app in App Web di servizio App di Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - API Web 2.1
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout. js](http://knockoutjs.com/) 3.1


Questa esercitazione Usa l'API Web ASP.NET 2 con Entity Framework 6 per creare un'applicazione web che consente di modificare un database back-end. Di seguito è riportata una schermata dell'applicazione che verrà creato.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

L'app Usa un progetto di applicazione a singola pagina (SPA). "Applicazione a singola pagina" è il termine generale per un'applicazione web che carica una singola pagina HTML, quindi aggiorna la pagina in modo dinamico, invece di caricare nuove pagine. Dopo il caricamento della pagina iniziale, l'app comunica con il server tramite le richieste AJAX. Il AJAX richiede i dati JSON restituiti, l'app Usa per aggiornare l'interfaccia utente.

AJAX non è una novità, ma oggi esistono Framework JavaScript che rendono più semplice compilare e gestire un'applicazione SPA sofisticata di grandi dimensioni. Questa esercitazione viene usato [Knockout. js](http://knockoutjs.com/), ma è possibile usare qualsiasi framework JavaScript sul client.

Ecco gli elementi fondamentali per questa app:

- ASP.NET MVC consente di creare la pagina HTML.
- API Web ASP.NET gestisce le richieste AJAX e restituisce i dati JSON.
- Knockout. js Associa dati gli elementi HTML per i dati JSON.
- Entity Framework comunica con il database.

## <a name="see-this-app-running-on-azure"></a>Vedere questa App in esecuzione in Azure

Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale? È possibile distribuire una versione completa dell'app al tuo account Azure, facendo semplicemente clic sul pulsante seguente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

È necessario un account di Azure per distribuire questa soluzione in Azure. Se non hai già un account, sono disponibili le opzioni seguenti:

- [Aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -ricevono crediti è possibile usare per provare i servizi di Azure a pagamento e anche dopo che sono abituati fino è possibile mantenere l'account e usare i servizi Azure gratuiti.
- [Attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -la sottoscrizione MSDN si accumulano crediti ogni mese in cui è possibile usare per i servizi di Azure a pagamento.

## <a name="create-the-project"></a>Creare il progetto

Aprire Visual Studio. Dal **File** dal menu **New**, quindi selezionare **progetto**. (Oppure fare clic su **nuovo progetto** nella paginainiziale.)

Nel **nuovo progetto** finestra di dialogo, fare clic su **Web** nel riquadro sinistro e **applicazione Web ASP.NET** nel riquadro centrale. Denominare il progetto BookService e fare clic su **OK**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **API Web** modello.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Se si vuole ospitare il progetto in un servizio App di Azure, lasciare il **ospita nel cloud** casella selezionata.

Fare clic su **OK** per creare il progetto.

## <a name="configure-azure-settings-optional"></a>Configurare le impostazioni di Azure (facoltative)

Se si lascia il **ospita nel Cloud** opzione selezionata, Visual Studio verrà richiesto di accedere a Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Dopo l'accesso ad Azure, Visual Studio viene richiesto di configurare l'app web. Immettere un nome per il sito, selezionare la sottoscrizione di Azure e selezionare un'area geografica. Sotto **passava**, selezionare **Crea nuovo server**. Immettere un nome utente dell'amministratore e una password.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [avanti](part-2.md)
