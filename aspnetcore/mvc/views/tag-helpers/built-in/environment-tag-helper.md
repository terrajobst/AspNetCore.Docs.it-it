---
title: Helper tag di ambiente in ASP.NET Core
author: pkellner
description: "Helper tag di ambiente ASP.NET Core definito con tutte le proprietà"
manager: wpickett
ms.author: riande
ms.date: 07/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 7a99ee0e59c7f49a3208d2c86c11cabce4294889
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Helper tag di ambiente in ASP.NET Core

Di [Peter Kellner](http://peterkellner.net) e [Hisham Bin Ateya](https://twitter.com/hishambinateya)

L'helper tag di ambiente esegue il rendering condizionale del proprio contenuto in base all'ambiente host corrente. L'unico attributo `names` dell'helper tag è un elenco di nomi di ambiente separati da virgole. Se uno qualsiasi di questi nomi corrisponde all'ambiente corrente, viene attivato il rendering del contenuto dell'helper tag.

## <a name="environment-tag-helper-attributes"></a>Attributi dell'helper tag di ambiente

### <a name="names"></a>nomi

Accetta un singolo nome di ambiente host o un elenco di nomi di ambiente separati da virgole, che attiva il rendering del contenuto.

Il o i valori vengono confrontati con il valore corrente restituito dalla proprietà statica `HostingEnvironment.EnvironmentName` di ASP.NET Core.  Il valore è uno dei seguenti: **Staging**, **Development** o **Production**. Il confronto non applica la distinzione tra maiuscole e minuscole.

Un esempio di helper tag `environment` valido è il seguente:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>Attributi include ed exclude

In ASP.NET Core 2.x vengono aggiunti gli attributi `include` & `exclude`. Questi attributi controllano il rendering del contenuto dell'helper tag in base ai nomi ambiente host inclusi o esclusi.

### <a name="include-aspnet-core-20-and-later"></a>include (ASP.NET Core 2.0 e versioni successive)

La proprietà `include` ha un comportamento simile a quello dell'attributo `names` in ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>exclude (ASP.NET Core 2.0 e versioni successive)

Al contrario la proprietà `exclude` consente a `EnvironmentTagHelper` di eseguire il rendering del contenuto per tutti i nomi di ambiente host, ad eccezione di quelli specificati dall'utente.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
