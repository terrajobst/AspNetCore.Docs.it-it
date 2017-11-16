---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: shirhatti
description: Informazioni sul supporto del debug di applicazioni ASP.NET Core durante l'esecuzione dietro IIS in Windows Server.
keywords: ASP.NET Core,internet information services,iis,windows server,modulo asp.net core,debug
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core

Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)

L'articolo descrive il supporto del debug di applicazioni ASP.NET Core in [Visual Studio](https://www.visualstudio.com/vs/) durante l'esecuzione dietro IIS in Windows Server. Questo argomento illustra nel dettaglio l'abilitazione della funzionalità e la configurazione del progetto.

## <a name="prerequisites"></a>Prerequisiti

* Visual Studio 2017 15.3 o versione successiva
* ASP.NET e il carico di lavoro Sviluppo Web *o* il carico di lavoro Sviluppo multipiattaforma .NET Core

## <a name="enable-iis"></a>Abilitare IIS

Abilitare IIS nel sistema. Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo). Selezionare la casella di controllo **Internet Information Services**.

![Funzionalità di Windows con la casella di controllo Internet Information Services selezionata come quadrato nero (non con un segno di spunta) ad indicare che alcune funzionalità di IIS sono abilitate](development-time-iis-support/_static/enable_iis.png)

Se l'installazione di IIS richiede un riavvio, riavviare il sistema.

## <a name="enable-development-time-iis-support"></a>Abilitare il supporto IIS in fase di sviluppo

Dopo aver installato IIS avviare il programma di installazione di Visual Studio per modificare l'installazione esistente di Visual Studio. Nel programma di installazione selezionare il componente **Supporto IIS in fase di sviluppo** . Il componente è elencato come componente facoltativo nel pannello **Riepilogo** del carico di lavoro **Sviluppo ASP.NET e Web**. Viene installato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), un modulo IIS nativo necessario per l'esecuzione delle applicazioni ASP.NET Core.

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata. Nella sezione Web e Cloud è selezionato il pannello Sviluppo ASP.NET e Web. A destra, nell'area Facoltativi del pannello Riepilogo è presente la casella di controllo Supporto IIS in fase di sviluppo.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configurare il progetto

Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo. In **Esplora soluzioni** di Visual Studio fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**. Selezionare la scheda **Debug**. Selezionare **IIS** nell'elenco a discesa **Avvia**. Verificare che la funzionalità **Avvia browser** sia abilitata con l'URL corretto.

![Finestra delle proprietà del progetto con la scheda Debug selezionata. Le impostazioni Profilo e Avvia sono impostate su IIS. La funzionalità Avvia browser è abilitata con l'indirizzo http://localhost/WebApplication2. Lo stesso indirizzo viene visualizzato anche nel campo URL dell'app nell'area Impostazioni server Web con l'opzione Abilita autenticazione anonima attivata.](development-time-iis-support/_static/project_properties.png)

In alternativa è possibile aggiungere manualmente un profilo di avvio per il file [launchSettings.json](http://json.schemastore.org/launchsettings) nell'app:

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

Se non si sta eseguendo Visual Studio come amministratore è possibile che venga richiesto di riavviare il programma. In tal caso riavviare Visual Studio.

La procedura è stata completata. A questo punto il progetto è configurato per il supporto IIS in fase di sviluppo. 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Host ASP.NET Core in Windows con IIS](xref:publishing/iis)
* [Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:hosting/aspnet-core-module)
