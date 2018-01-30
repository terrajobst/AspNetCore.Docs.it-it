---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: shirhatti
description: Informazioni sul supporto del debug di applicazioni ASP.NET Core durante l'esecuzione dietro IIS in Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a5f727dd21ac0c6702691df2215c42f4adc0ec27
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core

Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)

L'articolo descrive il supporto del debug di applicazioni ASP.NET Core in [Visual Studio](https://www.visualstudio.com/vs/) durante l'esecuzione dietro IIS in Windows Server. Questo argomento vengono illustrati l'abilitazione di questa funzionalità e l'impostazione di un progetto.

## <a name="prerequisites"></a>Prerequisiti

* Visual Studio 2017 15.3 o versione successiva
* ASP.NET e il carico di lavoro Sviluppo Web *o* il carico di lavoro Sviluppo multipiattaforma .NET Core

## <a name="enable-iis"></a>Abilitare IIS

Abilitare IIS. Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo). Selezionare la casella di controllo **Internet Information Services**.

![Funzionalità di Windows con la casella di controllo Internet Information Services selezionata come quadrato nero (non con un segno di spunta) ad indicare che alcune funzionalità di IIS sono abilitate](development-time-iis-support/_static/enable_iis.png)

Se l'installazione di IIS richiede un riavvio, riavviare il sistema.

## <a name="enable-development-time-iis-support"></a>Abilitare il supporto IIS in fase di sviluppo

Dopo aver installato IIS, è possibile avviare il programma di installazione di Visual Studio per modificare l'installazione esistente di Visual Studio. Nel programma di installazione selezionare il componente **Supporto IIS in fase di sviluppo** . Il componente è elencato come componente facoltativo nel pannello **Riepilogo** del carico di lavoro **Sviluppo ASP.NET e Web**. Viene installato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), un modulo IIS nativo necessario per l'esecuzione delle applicazioni ASP.NET Core.

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata. Nella sezione Web e Cloud è selezionato il pannello Sviluppo ASP.NET e Web. A destra nell'area del pannello riepilogo facoltativo, è una casella di controllo per la fase di sviluppo che supportano IIS.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configurare il progetto

Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo. In **Esplora soluzioni** di Visual Studio fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**. Selezionare la scheda **Debug**. Selezionare **IIS** nell'elenco a discesa **Avvia**. Verificare che la funzionalità **Avvia browser** sia abilitata con l'URL corretto.

![Finestra delle proprietà del progetto con la scheda Debug selezionata. Le impostazioni Profilo e Avvia sono impostate su IIS. La funzionalità Avvia browser è abilitata con l'indirizzo http://localhost/WebApplication2. Lo stesso indirizzo viene visualizzato anche nel campo URL dell'app nell'area Impostazioni server Web con l'opzione Abilita autenticazione anonima attivata.](development-time-iis-support/_static/project_properties.png)

In alternativa, aggiungere manualmente un profilo di avvio per il [launchSettings.json](http://json.schemastore.org/launchsettings) file nell'app:

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

Visual Studio potrebbe richiedere un riavvio, se non è in esecuzione come amministratore. In tal caso riavviare Visual Studio.

La procedura è stata completata. A questo punto, il progetto è configurato per il supporto in fase di sviluppo IIS. 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Host ASP.NET Core in Windows con IIS](xref:host-and-deploy/iis/index)
* [Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
