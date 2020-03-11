---
title: SDK Web di ASP.NET Core
author: Rick-Anderson
description: Panoramica di Microsoft. NET. Sdk. Web.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 6a9d531efd2188aed525c949bb124914c31119db
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661056"
---
# <a name="aspnet-core-web-sdk"></a>SDK Web di ASP.NET Core

### <a name="overview"></a>Panoramica

`Microsoft.NET.Sdk.Web` è un [SDK di progetto MSBuild](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) per la compilazione di app ASP.NET Core. È possibile creare un'app ASP.NET Core senza questo SDK, ma il Web SDK è:

* Personalizzata per offrire un'esperienza di prima classe.
* Destinazione consigliata per la maggior parte degli utenti.

Usare Web. SDK in un progetto:

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

Funzionalità abilitate tramite Web SDK:

* I progetti destinati a .NET Core 3,0 o versioni successive fanno riferimento in modo implicito:

  * [Framework condiviso ASP.NET Core](xref:fundamentals/metapackage-app).
  * [Analizzatori](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) progettati per la compilazione di app ASP.NET Core.
* Web SDK Importa destinazioni MSBuild che consentono di usare i profili di pubblicazione e la pubblicazione tramite WebDeploy.

### <a name="properties"></a>Proprietà

| Proprietà | Descrizione |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | Disabilita il riferimento implicito al Framework condiviso `Microsoft.AspNetCore.App`. |
| `DisableImplicitAspNetCoreAnalyzers` | Disabilita il riferimento implicito agli analizzatori ASP.NET Core. |
| `DisableImplicitComponentsAnalyzers` | Disabilita il riferimento implicito agli analizzatori di componenti Razor durante la compilazione di applicazioni Blazor (Server). |
