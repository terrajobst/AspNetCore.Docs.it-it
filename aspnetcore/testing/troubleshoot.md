---
title: Risoluzione dei problemi per ASP.NET Core
author: Rick-Anderson
description: Comprendere e risolvere i problemi di avvisi ed errori con i progetti ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: 98077081409949db14b19c7934bc162990ffc302
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a>Risolvere i progetti ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

I collegamenti seguenti forniscono informazioni aggiuntive sulla risoluzione dei problemi:

* [Risolvere i problemi di ASP.NET Core in Servizio app di Azure](xref:host-and-deploy/azure-apps/troubleshoot)
* [Risolvere i problemi di ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot)
* [Errori comuni di Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a>Avvisi di .NET core SDK

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Entrambi 32 e 64 bit di .NET Core SDK siano installate le versioni
Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si potrebbe vedere il seguente avviso vengono visualizzati nella parte superiore: 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/both32and64bit.png)

Questo avviso viene visualizzato quando (x86) 32 bit sia versioni a 64 bit (x64) di [.NET Core SDK](https://www.microsoft.com/net/download/all) installate. È possono installare entrambe le versioni di cause comuni includono:

* Originariamente scaricato il programma di installazione di .NET Core SDK utilizzando un computer a 32 bit, ma quindi copiato quest'ultimo, installato in un computer a 64 bit. 
* il SDK di 32 bit .NET Core è stata installata mediante un'altra applicazione.
* La versione non corretta è stata scaricata e installata.

Disinstallare il SDK dei componenti di base .NET a 32 bit per evitare questo avviso. Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**. Se si comprende perché l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK viene installato in più posizioni
Nel **nuovo progetto** finestra di dialogo per ASP.NET Core venga visualizzato il seguente messaggio di avviso vengono visualizzati nella parte superiore: 

 .NET Core SDK è installato in più posizioni. Solo i modelli dal SDK installato in "c:\Programmi\Microsoft Files\dotnet\sdk\' verranno visualizzati.

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/multiplelocations.png)

Questo messaggio viene visualizzato perché sono presenti almeno un'installazione di .NET Core SDK in una directory all'esterno di * c:\Programmi\Microsoft Files\dotnet\sdk\*. In genere che si verifica quando il SDK di .NET Core è stato distribuito in una macchina mediante copia/incolla anziché il programma di installazione MSI.

Disinstallare il SDK dei componenti di base .NET a 32 bit per evitare questo avviso. Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**. Se si comprende perché l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.
