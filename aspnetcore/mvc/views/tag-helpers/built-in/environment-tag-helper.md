---
title: Helper tag di ambiente in ASP.NET Core
author: pkellner
description: Helper tag di ambiente ASP.NET Core definito con tutte le proprietà
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: e2e038fe69da696b67f7aef61795e23dc8512fdf
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856125"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Helper tag di ambiente in ASP.NET Core

Di [Peter Kellner](https://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya) e [Luke Latham](https://github.com/guardrex)

L'helper tag di ambiente esegue il rendering condizionale del proprio contenuto in base all'[ambiente host](xref:fundamentals/environments) corrente. L'unico attributo dell'helper tag di ambiente, `names`, è un elenco delimitato da virgole di nomi di ambiente. Se nessuno dei nomi di ambiente specificato corrisponde all'ambiente corrente, viene eseguito il rendering del contenuto incluso.

Per una panoramica degli helper tag, vedere <xref:mvc/views/tag-helpers/intro>.

## <a name="environment-tag-helper-attributes"></a>Attributi dell'helper tag di ambiente

### <a name="names"></a>nomi

`names` accetta un singolo nome di ambiente host o un elenco delimitato da virgole di nomi di ambiente, che attiva il rendering del contenuto.

I valori di ambiente vengono confrontati con il valore corrente restituito da [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*). Il confronto non applica la distinzione tra maiuscole e minuscole.

L'esempio seguente usa un helper tag di ambiente. Il rendering del contenuto viene eseguito se l'ambiente host è Staging o Production:

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a>Attributi include ed exclude

Gli attributi `include` ed `exclude` controllano il rendering del contenuto incluso in base ai nomi di ambiente host inclusi o esclusi.

### <a name="include"></a>include

La proprietà `include` ha un comportamento simile all'attributo `names`. Un ambiente elencato nel valore dell'attributo `include` deve corrispondere all'ambiente host dell'app ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) per il rendering del contenuto del tag `<environment>`.

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a>exclude

Al contrario dell'attributo `include`, il rendering del contenuto del tag `<environment>` viene eseguito quando l'ambiente host non corrisponde a un ambiente elencato nel valore dell'attributo `exclude`.

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/environments>
