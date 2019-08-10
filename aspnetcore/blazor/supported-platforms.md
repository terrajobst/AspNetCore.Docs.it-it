---
title: Piattaforme supportate da ASP.NET Core Blazer
author: guardrex
description: Informazioni sulle piattaforme supportate per ASP.NET Core blazer.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 01f3a55a8536feedf713e07ea3724a0bc51e7c63
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2019
ms.locfileid: "68948251"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Piattaforme supportate da ASP.NET Core Blazer

Di [Luke Latham](https://github.com/guardrex)

## <a name="browser-requirements"></a>Requisiti del browser

### <a name="blazor-client-side"></a>Blazor sul lato client

| Browser                          | Version               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Corrente               |
| Mozilla Firefox                  | Corrente               |
| Google Chrome, incluso Android | Corrente               |
| Safari, incluso iOS            | Corrente               |
| Microsoft Internet Explorer      | Non supportato&dagger; |

&dagger;Microsoft Internet Explorer non supporta [webassembly](https://webassembly.org).

### <a name="blazor-server-side"></a>Blazor sul lato server

| Browser                          | Version    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Corrente    |
| Mozilla Firefox                  | Corrente    |
| Google Chrome, incluso Android | Corrente    |
| Safari, incluso iOS            | Corrente    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;Sono necessari altri riempimenti, ad esempio le promesse possono essere aggiunte tramite un bundle [polyfill.io](https://polyfill.io/v3/) .

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:blazor/hosting-models>
