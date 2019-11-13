---
title: Piattaforme supportate da ASP.NET Core Blazor
author: guardrex
description: Informazioni sulle piattaforme supportate per ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/supported-platforms
ms.openlocfilehash: de51296cc8785474e1c1406cfd5d4e5bd4050172
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962736"
---
# <a name="aspnet-core-opno-locblazor-supported-platforms"></a>Piattaforme supportate da ASP.NET Core Blazor

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Requisiti del browser

### <a name="opno-locblazor-webassembly"></a>Blazor webassembly

| Browser                          | Versione               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Corrente               |
| Mozilla Firefox                  | Corrente               |
| Google Chrome, incluso Android | Corrente               |
| Safari, incluso iOS            | Corrente               |
| Microsoft Internet Explorer      | Non supportato&dagger; |

&dagger;Microsoft Internet Explorer non supporta [webassembly](https://webassembly.org).

### <a name="opno-locblazor-server"></a>Server Blazor

| Browser                          | Versione    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Corrente    |
| Mozilla Firefox                  | Corrente    |
| Google Chrome, incluso Android | Corrente    |
| Safari, incluso iOS            | Corrente    |
| Microsoft Internet Explorer      | 11&dagger; |

sono necessarie le compilazioni &dagger;Additional (ad esempio, le promesse possono essere aggiunte tramite un bundle [polyfill.io](https://polyfill.io/v3/) ).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:blazor/hosting-models>
