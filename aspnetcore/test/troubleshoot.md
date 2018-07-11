---
title: Risolvere i problemi di progetti ASP.NET Core
author: Rick-Anderson
description: Riconoscere e risolvere i problemi di avvisi ed errori con i progetti ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: c72dd93f6ba705d7f03ade556c7a037dadeb6295
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938394"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Risolvere i problemi di progetti ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

I collegamenti seguenti forniscono indicazioni sulla risoluzione dei problemi:

* [Risolvere i problemi di ASP.NET Core in Servizio app di Azure](xref:host-and-deploy/azure-apps/troubleshoot)
* [Risolvere i problemi di ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot)
* [Errori comuni di Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [NDC Conference (Londra, 2018): La diagnosi dei problemi nelle applicazioni ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog su ASP.NET: Risoluzione dei problemi di prestazioni di ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Avvisi di .NET core SDK

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Entrambe le versioni a 64 bit di .NET Core SDK e a 32 bit installate

Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:

> Vengono installate sia 32 e 64 versioni di bit di .NET Core SDK. Solo i modelli dalle versioni a 64 bit installate in ' c:\\Program Files\\dotnet\\sdk\\' verranno visualizzati.

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/both32and64bit.png)

Questo avviso viene visualizzato quando (x86) 32 bit sia versioni a 64 bit (x64) del [.NET Core SDK](https://www.microsoft.com/net/download/all) siano installati. Cause più comuni che è possibile installare entrambe le versioni includono:

* Si originariamente scaricato il programma di installazione di .NET Core SDK Usa un computer a 32 bit, ma quindi copiata quest'ultimo e viene installato in un computer a 64 bit.
* 32 bit .NET Core SDK è stato installato da un'altra applicazione.
* La versione errata è stata scaricata e installata.

Disinstallare il SDK di .NET Core a 32 bit per evitare questo avviso. Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**. Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK è installato in più posizioni

Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:

> .NET Core SDK è installato in più posizioni. Solo i modelli dagli SDK installati in ' c:\\Program Files\\dotnet\\sdk\\' verranno visualizzati.

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/multiplelocations.png)

Viene visualizzato questo messaggio quando si dispone di almeno un'installazione di .NET Core SDK in una directory fuori *c:\\Program Files\\dotnet\\sdk\\*. In genere ciò si verifica quando .NET Core SDK è stato distribuito in un computer tramite Copia/Incolla anziché il programma di installazione MSI.

Disinstallare il SDK di .NET Core a 32 bit per evitare questo avviso. Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**. Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.

### <a name="no-net-core-sdks-were-detected"></a>Nessun SDK per .NET Core sono stati rilevati

Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:

> Nessun SDK per .NET Core sono stati rilevati, assicurarsi che siano inclusi nella variabile di ambiente 'PATH'.

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/NoNetCore.png)

Questo avviso viene visualizzato quando la variabile di ambiente `PATH` non punta a qualsiasi SDK per .NET Core nel computer. Per risolvere questo problema:

* Installare o verificare che sia installato .NET Core SDK.
* Verificare che il `PATH` variabile di ambiente punta alla posizione in cui è installato il SDK. Il programma di installazione è imposta in genere il `PATH`.
