---
title: Helper di Tag di ambiente in ASP.NET Core
author: pkellner
description: "Helper di Tag di ambiente ASP.Net Core definito tra tutte le proprietà"
keywords: ASP.NET Core, helper tag
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper
ms.openlocfilehash: 5870d9ebd02becf29f892c91310022d3b9a6b7af
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Helper di Tag di ambiente in ASP.NET Core

Da [Peter Kellner](http://peterkellner.net) e [Ateya Hisham Bin](https://twitter.com/hishambinateya)

L'Helper di Tag di ambiente in modo condizionale esegue il rendering di contenuto incluso in base all'ambiente di hosting corrente. L'unico attributo `names` è un elenco separato da virgole di ambiente di nomi, che se qualsiasi corrispondono all'ambiente corrente, verrà attivato il contenuto incluso da sottoporre a rendering.

## <a name="environment-tag-helper-attributes"></a>Attributi Helper di Tag di ambiente

### <a name="names"></a>nomi

Accetta un singolo nome di ambiente host o un elenco delimitato da virgole di nomi di ambiente che attivano il rendering del contenuto tra parentesi di hosting.

Questi valori vengono confrontati con il valore corrente restituito dalla proprietà statica ASP.NET Core `HostingEnvironment.EnvironmentName`.  Questo valore è uno dei seguenti: **gestione temporanea**; **Sviluppo** o **produzione**. Ignora il confronto.

Un esempio di un oggetto valido `environment` helper di tag è:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>includere ed escludere gli attributi

ASP.NET Core 2. x aggiunge il `include`  &  `exclude` attributi. Questi attributi controllano il rendering del contenuto tra parentesi, in base ai nomi ambiente hosting incluso o escluso.

### <a name="include-aspnet-core-20-and-later"></a>includere Core ASP.NET 2.0 e versioni successive

Il `include` proprietà presenta un comportamento simile del `names` attributo nella versione 1.0 di ASP.NET Core.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>escludere i Core ASP.NET 2.0 e versioni successive

Al contrario, il `exclude` proprietà consente di `EnvironmentTagHelper` il rendering del contenuto per tutti i nomi di ambiente host, ad eccezione di indicare che è stato specificato tra parentesi.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>