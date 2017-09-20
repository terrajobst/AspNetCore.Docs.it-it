---
title: La precompilazione e la compilazione di visualizzazione razor
author: rick-anderson
description: Un documento di riferimento che descrivono come abilitare la compilazione di visualizzazione MVC Razor e precompilazione nelle applicazioni ASP.NET Core.
keywords: ASP.NET Core, la compilazione di visualizzazione Razor, Razor pre-compilazione, la precompilazione Razor
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 0cb61315916d1b38f7cab3339e150c446fb69d98
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Compilazione di visualizzazione Razor e precompilazione in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Visualizzazioni Razor vengono compilate in fase di esecuzione quando viene richiamata la vista. ASP.NET Core 1.1.0 e versione successiva possono facoltativamente compilare visualizzazioni Razor e distribuirli con l'app &mdash; un processo noto come la precompilazione. Per impostazione predefinita, i modelli di progetto ASP.NET Core 2. x abilitare precompilazione.

> [!NOTE]
> Precompilazione di visualizzazione Razor non è disponibile quando si esegue un [Self-Contained distribuzione](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) nelle versioni di ASP.NET Core 2.0.0 e versioni precedenti.

Considerazioni sulla precompilazione:

* La precompilazione di visualizzazioni comporta un più piccolo pubblicato bundle e tempi di avvio.
* È possibile modificare i file Razor dopo la precompilazione di viste. Le viste modificate non saranno presenti nel bundle pubblicato. 

Per distribuire precompilate viste:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se il progetto è destinato a .NET Framework, includere un riferimento pacchetto `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Se il progetto è destinato a .NET Core, non sono necessarie modifiche.

I modelli di progetto ASP.NET Core 2. x viene impostata in modo implicito `MvcRazorCompileOnPublish` a `true` per impostazione predefinita, ovvero il nodo può essere rimosso in modo sicuro dal *csproj* file. Se si preferisce, è necessario essere espliciti sono non crea alcun problema nell'impostazione di `MvcRazorCompileOnPublish` proprietà `true`. Nell'esempio *csproj* ed evidenzia questa impostazione:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Impostare `MvcRazorCompileOnPublish` a `true`e includere un riferimento pacchetto `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Nell'esempio *csproj* ed evidenzia queste impostazioni:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---
