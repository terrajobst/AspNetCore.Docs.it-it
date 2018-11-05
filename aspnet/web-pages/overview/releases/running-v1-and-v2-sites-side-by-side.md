---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Che eseguono versioni diverse delle pagine Web ASP.NET (Razor) Side-by-Side | Microsoft Docs
author: Rick-Anderson
description: Questo articolo illustra come eseguire siti Web di ASP.NET Web Pages (Razor) nello stesso computer o server quando i siti Web configurati per l'uso di diverse versioni...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: e587398b430795c12a1dcee394852b4e2b8a0e44
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021173"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>Esecuzione side-by-Side di versioni diverse di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come eseguire siti Web di ASP.NET Web Pages (Razor) nello stesso computer o server quando i siti Web configurati per usare versioni diverse di ASP.NET Web Pages.
> 
> Che cosa si apprenderà come:
> 
> - Il comportamento predefinito novità di ASP.NET quando si dispongono di siti creati con ASP.NET Web Pages.
> - Come configurare un nuovo sito per l'esecuzione con una versione precedente di ASP.NET Web Pages.
>   
> 
> Si tratta della funzionalità ASP.NET introdotta nell'articolo:
> 
> - Il `webPages:Version` impostazione di configurazione.
>   
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2 e le pagine Web ASP.NET 1.0.


ASP.NET Web Pages supporta la possibilità di eseguire siti Web di fianco a fianco. È possibile continuare a eseguire le applicazioni ASP.NET Web Pages precedenti, creare nuove applicazioni ASP.NET Web Pages ed eseguire tutti gli elementi nello stesso computer.

Ecco alcuni aspetti da ricordare quando si installa le pagine Web con WebMatrix:

- Per impostazione predefinita, le applicazioni Web Pages esistente verranno eseguita come la versione più recente nel computer. (Gli assembly vengono installati nella global assembly cache (GAC) e vengono usati automaticamente).
- Se si desidera eseguire un sito usando una versione diversa di ASP.NET Web Pages, è possibile configurare il sito a tale scopo. Se il sito non ha ancora un *Web. config* file nella radice del sito, crearne uno nuovo e copiarvi il codice XML seguente, sovrascrivendo il contenuto esistente. Se il sito contiene già un *Web. config* Aggiungi un' `<appSettings>` elemento simile a quello riportato di seguito per il `<configuration>` sezione.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-Se non si specifica una versione nel *Web. config* file, un sito viene distribuito come la versione più recente. (Gli assembly vengono copiati i *bin* cartella nel sito distribuito.)
- Le nuove applicazioni create con i modelli di sito in WebMatrix includono gli assembly della versione di pagine Web del sito *bin* cartella.

In generale, è sempre possibile controllare quale versione di pagine Web da utilizzare con il sito con NuGet per installare gli assembly adatti al sito *bin* cartella. Per trovare i pacchetti, visitare [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Risorse aggiuntive

[Le funzionalità migliori in ASP.NET Web Pages 2](top-features-in-web-pages-2.md)
