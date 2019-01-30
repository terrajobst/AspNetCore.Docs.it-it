---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Using Web API 2 con Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Questa esercitazione insegnerà le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end. L'esercitazione Usa Entity Framework 6 per il layout dei dati...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236523"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Using Web API 2 con Entity Framework 6
====================

[Download progetto completato](https://github.com/MikeWasson/BookService)

> Questa esercitazione illustra le nozioni di base di creazione di un'applicazione web con un'API Web ASP.NET di back-end. L'esercitazione Usa Entity Framework 6 per il livello di dati e Knockout. js per l'applicazione JavaScript lato client. L'esercitazione illustra anche come distribuire l'app in App Web di servizio App di Azure.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
> - API Web 2.1
> - Visual Studio 2017 (download di Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.7
> - [Knockout.js](http://knockoutjs.com/) 3.1

Questa esercitazione Usa l'API Web ASP.NET 2 con Entity Framework 6 per creare un'applicazione web che consente di modificare un database back-end. Di seguito è riportata una schermata dell'applicazione che verrà creato.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

L'app Usa un progetto di applicazione a singola pagina (SPA). "Applicazione a singola pagina" è il termine generale per un'applicazione web che carica una singola pagina HTML, quindi aggiorna la pagina in modo dinamico, invece di caricare nuove pagine. Dopo il caricamento della pagina iniziale, l'app comunica con il server tramite le richieste AJAX. Il AJAX richiede i dati JSON restituiti, l'app Usa per aggiornare l'interfaccia utente.

AJAX non è una novità, ma oggi esistono Framework JavaScript che rendono più semplice compilare e gestire un'applicazione SPA sofisticata di grandi dimensioni. Questa esercitazione viene usato [Knockout. js](http://knockoutjs.com/), ma è possibile usare qualsiasi framework JavaScript sul client.

Ecco gli elementi fondamentali per questa app:

- ASP.NET MVC consente di creare la pagina HTML.
- API Web ASP.NET gestisce le richieste AJAX e restituisce i dati JSON.
- Knockout. js Associa dati gli elementi HTML per i dati JSON.
- Entity Framework comunica con il database.

## <a name="see-this-app-running-on-azure"></a>Vedere questa app in esecuzione in Azure

Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale? È possibile distribuire una versione completa dell'app all'account di Azure selezionando il pulsante seguente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

È necessario un account di Azure per distribuire questa soluzione in Azure. Se non hai già un account, sono disponibili le opzioni seguenti:

- [Aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -ricevono crediti è possibile usare per provare i servizi di Azure a pagamento e anche dopo che sono abituati fino è possibile mantenere l'account e usare i servizi Azure gratuiti.
- [Attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -la sottoscrizione MSDN si accumulano crediti ogni mese in cui è possibile usare per i servizi di Azure a pagamento.

## <a name="create-the-project"></a>Creare il progetto

Aprire Visual Studio. Dal **File** dal menu **New**, quindi selezionare **progetto**. (O selezionare **nuovo progetto** nella paginainiziale.)

Nel **nuovo progetto** finestra di dialogo, seleziona **Web** nel riquadro sinistro e **applicazione Web ASP.NET (.NET Framework)** nel riquadro centrale. Denominare il progetto **BookService** e selezionare **OK**.

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **API Web** modello.

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


Selezionare **OK** per creare il progetto.

## <a name="configure-azure-settings-optional"></a>Configurare le impostazioni di Azure (facoltative)

Dopo aver creato il progetto, è possibile distribuire in App Web di servizio App di Azure in qualsiasi momento. 

1. In Esplora soluzioni fare doppio clic sul progetto, quindi scegliere **pubblica**.

2. Nella finestra visualizzata, selezionare **avviare**. Il **selezionare una destinazione di pubblicazione** verrà visualizzata la finestra di dialogo.

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. Selezionare **Crea profilo**. Viene visualizzata la finestra di dialogo **Crea servizio app**.

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   Accettare le impostazioni predefinite oppure immettere valori diversi per il nome dell'applicazione, gruppo di risorse, area geografica, sottoscrizione di Azure e piano di hosting. 

4. Selezionare **creare un database SQL**. Il **configurare SQL Server** verrà visualizzata la finestra di dialogo. 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   Accettare i valori predefiniti o immettere valori diversi. Immettere un **Adminstrator Username** e **Password dell'amministratore** per il nuovo database. Selezionare **OK** dopo aver completato. Il **Crea servizio App** pagina viene visualizzata di nuovo.

5. Selezionare **Create** per creare il profilo. Viene visualizzato un messaggio nell'angolo in basso a destra che indica che la distribuzione è in corso. Dopo un breve periodo di tempo, il **pubblica** viene visualizzata la finestra.

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    Il profilo creato per distribuire l'app è ora disponibile. 


> [!div class="step-by-step"]
> [avanti](part-2.md)
