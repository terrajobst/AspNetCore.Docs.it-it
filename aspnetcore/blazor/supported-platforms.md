---
title: Piattaforme supportate da ASP.NET Core Blazer
author: guardrex
description: Informazioni sulle piattaforme supportate per ASP.NET Core blazer.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 042fbb1b2c7f92b7dc6443319f3f195a12a55adc
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963887"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Piattaforme supportate da ASP.NET Core Blazer

Di [Luke Latham](https://github.com/guardrex)

## <a name="browser-requirements"></a>Requisiti del browser

### <a name="blazor-webassembly"></a>Webassembly Blazer

| Browser                          | Versione               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Corrente               |
| Mozilla Firefox                  | Corrente               |
| Google Chrome, incluso Android | Corrente               |
| Safari, incluso iOS            | Corrente               |
| Microsoft Internet Explorer      | Non supportato&dagger; |

&dagger;Microsoft Internet Explorer non supporta [webassembly](https://webassembly.org).

### <a name="blazor-server"></a>Server Blazer

| Browser                          | Versione    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Corrente    |
| Mozilla Firefox                  | Corrente    |
| Google Chrome, incluso Android | Corrente    |
| Safari, incluso iOS            | Corrente    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;Sono necessari altri riempimenti, ad esempio le promesse possono essere aggiunte tramite un bundle [polyfill.io](https://polyfill.io/v3/) .

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:blazor/hosting-models>
