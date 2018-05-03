---
title: Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.x e versioni successive
author: Rick-Anderson
description: Il metapacchetto Microsoft.AspNetCore.All include tutti i pacchetti ASP.NET Core e Entity Framework Core supportati con le relative dipendenze.
manager: wpickett
monikerRange: = aspnetcore-2.0
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 4c11f15e659565325bfe8b8d91188b62177b251d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2018
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.x

Questa funzionalità richiede ASP.NET Core 2.x con destinazione .NET Core 2.x.

Il metapacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) per ASP.NET include:

* Tutti i pacchetti supportati dal team ASP.NET Core.
* Tutti i pacchetti supportati da Entity Framework Core. 
* Le dipendenze interne e di terze parti usate da ASP.NET Core e da Entity Framework Core. 

Tutte le funzionalità di ASP.NET Core 2.x e Entity Framework Core 2.x sono incluse nel pacchetto `Microsoft.AspNetCore.All`. I modelli di progetto predefiniti destinati ad ASP.NET Core 2.0 usano questo pacchetto.

Il numero di versione del metapacchetto `Microsoft.AspNetCore.All` rappresenta la versione di ASP.NET Core e la versione di Entity Framework Core (allineata alla versione di .NET Core).

Le applicazioni che usano il metapacchetto `Microsoft.AspNetCore.All` sfruttano automaticamente l'[archivio di runtime di .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). L'archivio di runtime contiene tutti gli asset di runtime necessari per eseguire le applicazioni ASP.NET Core 2.x. Quando si usa il metapacchetto `Microsoft.AspNetCore.All`, con l'applicazione **non** vengono distribuiti asset dai pacchetti NuGet di riferimento di ASP.NET Core. Questi asset sono infatti inclusi nell'archivio di runtime di .NET Core. Gli asset contenuti nell'archivio di runtime sono precompilati per migliorare i tempi di avvio dell'applicazione.

Per rimuovere i pacchetti che non vengono usati è possibile usare il processo di trimming dei pacchetti. I pacchetti di cui è stato eseguito il trimming sono esclusi dall'output dell'applicazione pubblicata.

Il file con estensione *csproj* seguente fa riferimento al metapacchetto `Microsoft.AspNetCore.All` per ASP.NET Core:

[!code-xml[](../mvc/views/view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=9)]
