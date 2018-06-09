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
uid: test/troubleshoot
ms.openlocfilehash: 64a353a9bdf0753c63f676f9d07f42ba45acdcab
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "35217651"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Risolvere i progetti ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

I collegamenti seguenti forniscono informazioni aggiuntive sulla risoluzione dei problemi:

* [Risolvere i problemi di ASP.NET Core in Servizio app di Azure](xref:host-and-deploy/azure-apps/troubleshoot)
* [Risolvere i problemi di ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot)
* [Errori comuni di Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [YouTube: Diagnosing issues in ASP.NET Core Applications](https://www.youtube.com/watch?v=RYI0DHoIVaA) (Problemi di diagnosi nelle applicazioni ASP.NET Core)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a>Avvisi di .NET core SDK

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Entrambe le versioni a 64 bit del SDK .NET Core e a 32 bit installate
Nel **nuovo progetto** dialogo ASP.NET Core, si potrebbe notare l'avviso seguente: 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/both32and64bit.png)

Questo avviso viene visualizzato quando (x86) 32 bit sia versioni a 64 bit (x64) di [.NET Core SDK](https://www.microsoft.com/net/download/all) installate. È possono installare entrambe le versioni di cause comuni includono:

* Originariamente scaricato il programma di installazione di .NET Core SDK utilizzando un computer a 32 bit, ma quindi copiato quest'ultimo, installato in un computer a 64 bit. 
* il SDK di 32 bit .NET Core è stata installata mediante un'altra applicazione.
* La versione non corretta è stata scaricata e installata.

Disinstallare il SDK dei componenti di base .NET a 32 bit per evitare questo avviso. Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**. Se si comprende perché l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK viene installato in più posizioni
Nel **nuovo progetto** finestra di dialogo per ASP.NET Core venga visualizzato l'avviso seguente: 

 .NET Core SDK è installato in più posizioni. Solo i modelli dal SDK installato in "c:\Programmi\Microsoft Files\dotnet\sdk\' verranno visualizzati.

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/multiplelocations.png)

Questo messaggio viene visualizzato quando si dispone di almeno un'installazione di .NET Core SDK in una directory all'esterno di * c:\Programmi\Microsoft Files\dotnet\sdk\*. In genere che si verifica quando il SDK di .NET Core è stato distribuito in una macchina mediante copia/incolla anziché il programma di installazione MSI.

Disinstallare il SDK dei componenti di base .NET a 32 bit per evitare questo avviso. Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**. Se si comprende perché l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.

### <a name="no-net-core-sdks-were-detected"></a>Nessun SDK di .NET Core sono stato rilevato
Nel **nuovo progetto** finestra di dialogo per ASP.NET Core venga visualizzato l'avviso seguente: 

**Nessun SDK di .NET Core sono stato rilevato, verificare che siano incluse nella variabile di ambiente 'PATH'**

![Uno screenshot della finestra di dialogo OneASP.NET che mostra il messaggio di avviso](troubleshoot/_static/NoNetCore.png)

Questo avviso viene visualizzato quando la variabile di ambiente `PATH` non fa riferimento a qualsiasi .NET Core SDK nel computer. Per risolvere questo problema:

* Installare o verificare che sia installato il SDK dei componenti di base di .NET.
* Verificare il `PATH` variabile di ambiente punta alla posizione è installato SDK. Il programma di installazione in genere imposta la `PATH`.

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-application-deadlocks"></a>Utilizzo di IHtmlHelper.Partial può comportare deadlock di applicazioni

In ASP.NET Core 2.1 e versioni successive, la chiamata `Html.Partial` notificata tramite un avviso analyzer a causa del potenziale deadlock. Il messaggio di avviso è:

*Utilizzo di IHtmlHelper.Partial può comportare deadlock di applicazioni. Provare a usare `<partial>` Helper di Tag o `IHtmlHelper.PartialAsync`.*

Le chiamate a `@Html.Partial` deve essere sostituito dal `@await Html.PartialAsync` o l'helper di tag parziali `<partial name="_Partial" />`.

::: moniker-end
