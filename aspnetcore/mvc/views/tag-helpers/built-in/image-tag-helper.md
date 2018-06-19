---
title: Helper tag di immagine in ASP.NET Core
author: pkellner
description: Informazioni sull'uso dell'helper tag di immagine
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 6aa9175f873c4ea62e0319c812e5312cd3331141
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072262"
---
# <a name="image-tag-helper-in-aspnet-core"></a>Helper tag di immagine in ASP.NET Core

Di [Peter Kellner](http://peterkellner.net) 

L'helper tag di immagine migliora il tag `img` (`<img>`). Richiede un tag `src` e l'attributo `boolean` `asp-append-version`.

Se l'origine dell'immagine (`src`) è un file statico nel server Web host, una stringa di cache busting univoca viene accodata come parametro di query all'origine dell'immagine. Questo garantisce che se il file nel server Web host cambia viene generato un URL di richiesta univoco, che include il parametro di richiesta di aggiornamento. La stringa di cache busting è un valore univoco che rappresenta l'hash del file immagine statico.

Se l'origine dell'immagine (`src`) non è un file statico (ad esempio se è un URL remoto o se il file non esiste nel server), l'attributo `src` del tag `<img>` viene generato senza il parametro di query con stringa di cache busting.

## <a name="image-tag-helper-attributes"></a>Attributi dell'helper tag di immagine


### <a name="asp-append-version"></a>asp-append-version

Quando viene specificato insieme a un attributo `src`, viene chiamato l'helper tag di immagine.

Un esempio di helper tag `img` valido è il seguente:

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

Se il file statico esiste nella directory *..wwwroot/images/asplogo.png* il codice HTML generato è simile al seguente (il codice hash è diverso):

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

Il valore assegnato al parametro `v` è il valore hash dei file su disco. Se il server Web non è in grado di ottenere l'accesso in lettura al file statico di riferimento, non viene aggiunto nessun parametro `v` all'attributo `src`.

- - -

### <a name="src"></a>src

Per attivare l'helper tag di immagine l'attributo src è obbligatorio nell'elemento `<img>`. 

> [!NOTE]
> L'helper tag di immagine usa il provider `Cache` nel server Web locale per archiviare l'elemento `Sha512` calcolato di un file dato. Se il file viene richiesto di nuovo, non è necessario ricalcolare `Sha512`. La cache viene invalidata da un watcher di file che viene associato al file quando si calcola l'elemento `Sha512` del file stesso.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:performance/caching/memory>
