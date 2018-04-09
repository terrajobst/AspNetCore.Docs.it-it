---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: shirhatti
description: Individuare il supporto per il debug di applicazioni ASP.NET Core durante l'esecuzione di base IIS in Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core

Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Questo articolo descrive [Visual Studio](https://www.visualstudio.com/vs/) supporto per il debug di App di ASP.NET Core in esecuzione dietro IIS su Windows Server. Questo argomento vengono illustrati l'abilitazione di questa funzionalità e l'impostazione di un progetto.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>Abilitare IIS

Abilitare IIS. Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo). Selezionare la casella di controllo **Internet Information Services**.

![Funzionalità di Windows con la casella di controllo Internet Information Services selezionata come quadrato nero (non con un segno di spunta) ad indicare che alcune funzionalità di IIS sono abilitate](development-time-iis-support/_static/enable_iis.png)

Se l'installazione di IIS richiede un riavvio, riavviare il sistema.

## <a name="enable-development-time-iis-support"></a>Abilitare il supporto IIS in fase di sviluppo

Avviare il programma di installazione di Visual Studio. Selezionare il **fase di sviluppo IIS supporta** componente. Il componente è elencato come facoltativi nel **riepilogo** pannello per il **sviluppo web ASP.NET e** carico di lavoro. Questo modo vengono installati il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module), ovvero un modulo nativo di IIS necessario per eseguire app di ASP.NET Core.

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata. Nella sezione Web e Cloud è selezionato il pannello Sviluppo ASP.NET e Web. A destra nell'area del pannello riepilogo facoltativo, è una casella di controllo per la fase di sviluppo che supportano IIS.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configurare il progetto

Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo. In **Esplora soluzioni** di Visual Studio fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**. Selezionare la scheda **Debug**. Selezionare **IIS** nell'elenco a discesa **Avvia**. Verificare che la funzionalità **Avvia browser** sia abilitata con l'URL corretto.

![Finestra delle proprietà del progetto con la scheda Debug selezionata. Le impostazioni Profilo e Avvia sono impostate su IIS. La funzionalità di browser di avvio è abilitata con l'indirizzo http://localhost/WebApplication2. Lo stesso indirizzo viene visualizzato anche nel campo URL dell'app nell'area Impostazioni server Web con l'opzione Abilita autenticazione anonima attivata.](development-time-iis-support/_static/project_properties.png)

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

## <a name="additional-resources"></a>Risorse aggiuntive

* [Host ASP.NET Core in Windows con IIS](xref:host-and-deploy/iis/index)
* [Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
