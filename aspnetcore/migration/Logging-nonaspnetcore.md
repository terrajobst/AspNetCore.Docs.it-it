---
title: Eseguire la migrazione da Microsoft. Extensions. Logging 2,1 a 2,2 o 3,0
author: pakrym
description: Informazioni su come eseguire la migrazione di un'applicazione non-ASP.NET Core che usa Microsoft. Extensions. Logging da 2,1 a 2,2 o 3,0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659320"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>Eseguire la migrazione da Microsoft. Extensions. Logging 2,1 a 2,2 o 3,0

Questo articolo illustra i passaggi comuni per la migrazione di un'applicazione non-ASP.NET Core che usa `Microsoft.Extensions.Logging` da 2,1 a 2,2 o 3,0.

## <a name="21-to-22"></a>Da 2.1 a 2.2

Creare manualmente `ServiceCollection` e chiamare `AddLogging`.

2,1 esempio:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

2,2 esempio:

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>da 2,1 a 3,0

In 3,0 usare `LoggingFactory.Create`.

2,1 esempio:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

3,0 esempio:

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>Risorse aggiuntive

<xref:fundamentals/logging/index>