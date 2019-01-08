---
title: Eseguire la migrazione da Logging 3.0 o 2.1 a 2.2
author: pakrym
description: Informazioni su come eseguire la migrazione di un'applicazione non ASP.NET Core che usa Logging da 2.1 a 2.2 o 3.0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099471"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="6fb1e-103">Eseguire la migrazione da Logging 3.0 o 2.1 a 2.2</span><span class="sxs-lookup"><span data-stu-id="6fb1e-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="6fb1e-104">Questo articolo illustra i passaggi comuni per la migrazione di un'applicazione non ASP.NET Core che usa `Microsoft.Extensions.Logging` da 2.1 a 2.2 o 3.0.</span><span class="sxs-lookup"><span data-stu-id="6fb1e-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="6fb1e-105">2.1 a 2.2</span><span class="sxs-lookup"><span data-stu-id="6fb1e-105">2.1 to 2.2</span></span>

<span data-ttu-id="6fb1e-106">Creare manualmente `ServiceCollection` e chiamare `AddLogging`.</span><span class="sxs-lookup"><span data-stu-id="6fb1e-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="6fb1e-107">2.1 esempio:</span><span class="sxs-lookup"><span data-stu-id="6fb1e-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="6fb1e-108">esempio 2.2:</span><span class="sxs-lookup"><span data-stu-id="6fb1e-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="6fb1e-109">2.1 a 3.0</span><span class="sxs-lookup"><span data-stu-id="6fb1e-109">2.1 to 3.0</span></span>

<span data-ttu-id="6fb1e-110">In 3.0, usare `LoggingFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="6fb1e-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="6fb1e-111">2.1 esempio:</span><span class="sxs-lookup"><span data-stu-id="6fb1e-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="6fb1e-112">esempio 3.0:</span><span class="sxs-lookup"><span data-stu-id="6fb1e-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="6fb1e-113">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6fb1e-113">Additional resources</span></span>

<xref:fundamentals/logging/index>