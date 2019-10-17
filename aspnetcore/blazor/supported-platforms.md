---
title: Piattaforme supportate da ASP.NET Core Blazer
author: guardrex
description: Informazioni sulle piattaforme supportate per ASP.NET Core blazer.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 4e86bd6967a747a59c99a515c1c838cc2c21770f
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391224"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Piattaforme supportate da ASP.NET Core Blazer

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Requisiti del browser

### <a name="blazor-webassembly"></a>WebAssembly Blazor

| Browser                          | Versione               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Corrente               |
| Mozilla Firefox                  | Corrente               |
| Google Chrome, incluso Android | Corrente               |
| Safari, incluso iOS            | Corrente               |
| Microsoft Internet Explorer      | Non supportato @ no__t-0 |

&dagger;Microsoft Internet Explorer non supporta [webassembly](https://webassembly.org).

### <a name="blazor-server"></a>Server Blazor

| Browser                          | Versione    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Corrente    |
| Mozilla Firefox                  | Corrente    |
| Google Chrome, incluso Android | Corrente    |
| Safari, incluso iOS            | Corrente    |
| Microsoft Internet Explorer      | 11 @ no__t-0 |

sono necessarie le compilazioni &dagger;Additional (ad esempio, le promesse possono essere aggiunte tramite un bundle [polyfill.io](https://polyfill.io/v3/) ).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:blazor/hosting-models>
