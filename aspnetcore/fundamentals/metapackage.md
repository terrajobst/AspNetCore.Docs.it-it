---
title: Metapackage Microsoft.AspNetCore.All per ASP.NET Core 2. x e versioni successive
author: Rick-Anderson
description: Il metapackage Microsoft.AspNetCore.All include tutte supportate dei pacchetti di ASP.NET Core e di Entity Framework Core, con le relative dipendenze.
keywords: Core,NuGet,package,Microsoft.AspNetCore.All,metapackage ASP.NET
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: ff25d80be907994f7ac3d64a8ffa39ae53278ba6
ms.sourcegitcommit: 73bf6b222474d9f1f6aba3feaca4e191069d2121
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/03/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Metapackage Microsoft.AspNetCore.All per ASP.NET Core 2. x

Questa funzionalità richiede ASP.NET Core 2. x destinazione .NET Core 2. x.

Il metapacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) per ASP.NET include:

* Tutti i pacchetti supportati dal team ASP.NET Core.
* Tutti i pacchetti supportati da Entity Framework Core. 
* Le dipendenze interne e di terze parti usate da ASP.NET Core e da Entity Framework Core. 

Tutte le funzionalità di ASP.NET Core 2. x e Entity Framework Core 2. x sono incluse nel `Microsoft.AspNetCore.All` pacchetto. I modelli di progetto predefiniti utilizzano questo pacchetto.

Il numero di versione di `Microsoft.AspNetCore.All` metapackage rappresenta la versione di ASP.NET Core e la versione di Entity Framework Core (allineato con la versione di .NET Core).

Le applicazioni che utilizzano il `Microsoft.AspNetCore.All` metapackage automaticamente sfruttare il [archivio di Runtime di .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). L'archivio di Runtime contiene tutte le risorse di runtime necessari per eseguire applicazioni a 2. x di ASP.NET Core. Quando si utilizza il `Microsoft.AspNetCore.All` metapackage, **non** asset dai pacchetti NuGet Core ASP.NET riferimento vengono distribuiti con l'applicazione &mdash; l'archivio di Runtime .NET Core contiene queste risorse. Le risorse nell'archivio di Runtime sono precompilate per migliorare i tempi di avvio dell'applicazione.

Per rimuovere i pacchetti che non si utilizzano, è possibile utilizzare il processo di rimozione del pacchetto. Pacchetti tagliati vengono esclusi nell'output applicazione pubblicata.

Nell'esempio *csproj* i riferimenti di file di `Microsoft.AspNetCore.All` metapackage per ASP.NET Core:

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
