---
title: Piattaforme supportate da ASP.NET Core Blazor
author: guardrex
description: Informazioni sulle piattaforme supportate per ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: 505974280b5c96ec2bcae42c6e076ab67a15bb07
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658858"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Piattaforme supportate da ASP.NET Core Blazor

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
| Microsoft Internet Explorer      | Non supportato&dagger; |

&dagger;Microsoft Internet Explorer non supporta [webassembly](https://webassembly.org).

### <a name="blazor-server"></a>Server Blazor

| Browser                          | Versione    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Corrente    |
| Mozilla Firefox                  | Corrente    |
| Google Chrome, incluso Android | Corrente    |
| Safari, incluso iOS            | Corrente    |
| Microsoft Internet Explorer      | 11&dagger; |

sono necessari &dagger;ulteriori riempimenti, ad esempio le promesse possono essere aggiunte tramite un bundle [polyfill.io](https://polyfill.io/v3/) .

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:blazor/hosting-models>
