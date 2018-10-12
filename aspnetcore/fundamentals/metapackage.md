---
title: Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.0
author: Rick-Anderson
description: Il metapacchetto Microsoft.AspNetCore.All non è consigliato per ASP.NET Core 2.1 e versioni successive.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2018
uid: fundamentals/metapackage
ms.openlocfilehash: 1942426dbd5c15ae4a5fa5fbb931b94f50aa6043
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454739"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.0

> [!NOTE]
> È consigliabile che le applicazioni destinate a ASP.NET Core 2.1 e versioni successive usino [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) invece di questo pacchetto. Vedere [Migrazione da Microsoft.AspNetCore.All a Microsoft.AspNetCore.App](#migrate) in questo articolo.

Questa funzionalità richiede ASP.NET Core 2.x con destinazione .NET Core 2.x.

Il metapacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) per ASP.NET include:

* Tutti i pacchetti supportati dal team ASP.NET Core.
* Tutti i pacchetti supportati da Entity Framework Core.
* Le dipendenze interne e di terze parti usate da ASP.NET Core e da Entity Framework Core.

Tutte le funzionalità di ASP.NET Core 2.x e Entity Framework Core 2.x sono incluse nel pacchetto `Microsoft.AspNetCore.All`. I modelli di progetto predefiniti destinati ad ASP.NET Core 2.0 usano questo pacchetto.

Il numero di versione del metapacchetto `Microsoft.AspNetCore.All` rappresenta la versione di ASP.NET Core e la versione di Entity Framework Core.

Le applicazioni che usano il metapacchetto `Microsoft.AspNetCore.All` sfruttano automaticamente l'[archivio di runtime di .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). L'archivio di runtime contiene tutti gli asset di runtime necessari per eseguire le applicazioni ASP.NET Core 2.x. Quando si usa il metapacchetto `Microsoft.AspNetCore.All`, con l'applicazione **non** vengono distribuiti asset dai pacchetti NuGet di riferimento di ASP.NET Core. Questi asset sono infatti inclusi nell'archivio di runtime di .NET Core. Gli asset contenuti nell'archivio di runtime sono precompilati per migliorare i tempi di avvio dell'applicazione.

Per rimuovere i pacchetti che non vengono usati è possibile usare il processo di trimming dei pacchetti. I pacchetti di cui è stato eseguito il trimming sono esclusi dall'output dell'applicazione pubblicata.

Il file con estensione *csproj* seguente fa riferimento al metapacchetto `Microsoft.AspNetCore.All` per ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Migrazione da Microsoft.AspNetCore.All a Microsoft.AspNetCore.App

I pacchetti seguenti sono inclusi in `Microsoft.AspNetCore.All` ma non il pacchetto `Microsoft.AspNetCore.App`. 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

Per passare da `Microsoft.AspNetCore.All` a `Microsoft.AspNetCore.App`, se l'app usa qualsiasi API dai pacchetti sopra o pacchetti inseriti da tali pacchetti, aggiungere i riferimenti a tali pacchetti nel progetto.

Tutte le dipendenze dei pacchetti precedenti che non sono in altro modo dipendenze di `Microsoft.AspNetCore.App` non sono incluse in modo implicito. Ad esempio:

* `StackExchange.Redis` come dipendenza di `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` come dipendenza di `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`

## <a name="update-aspnet-core-21"></a>Aggiornare ASP.NET Core 2.1

È consigliabile eseguire la migrazione al metapacchetto `Microsoft.AspNetCore.App` per la versione 2.1 e successive. Per continuare a usare il metapacchetto `Microsoft.AspNetCore.All` e assicurarsi che venga distribuita la versione della patch più recente:

* Nei computer di sviluppo e nei server di compilazione: installare il [.NET Core SDK](https://www.microsoft.com/net/download) più recente.
* Nei server di distribuzione: installare il [runtime di .NET Core](https://www.microsoft.com/net/download) più recente.
 L'app eseguirà il roll forward all'ultima versione installata al riavvio dell'applicazione.
