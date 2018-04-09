---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Che eseguono versioni diverse delle pagine Web ASP.NET (Razor) Side-by | Documenti Microsoft
author: tfitzmac
description: In questo articolo viene illustrato come eseguire i siti Web di ASP.NET Web Pages (Razor) nello stesso computer o server quando i siti Web sono configurati per utilizzare versioni diverse...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 1729f3201013926b221afc92d23416b0081d8efb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>Esecuzione side-by-Side di versioni diverse delle pagine Web ASP.NET (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come eseguire i siti Web di ASP.NET Web Pages (Razor) nello stesso computer o server quando i siti Web sono configurati per utilizzare versioni diverse di ASP.NET Web Pages.
> 
> Illustra quanto segue:
> 
> - Il comportamento predefinito novità in ASP.NET quando si dispone di siti creati con ASP.NET Web Pages.
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
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2 e pagine Web ASP.NET 1.0.


Le pagine Web ASP.NET supporta la possibilità di eseguire siti Web side-by. Ciò consente di continuare a eseguire le applicazioni ASP.NET Web Pages precedenti, creare nuove applicazioni ASP.NET Web Pages ed eseguire tutti gli elementi nello stesso computer.

Di seguito sono illustrati alcuni aspetti da ricordare quando si installano le pagine Web con WebMatrix:

- Per impostazione predefinita, le applicazioni esistenti di pagine Web verranno eseguita come la versione più recente nel computer in uso. (Gli assembly vengono installati nella global assembly cache (GAC) e vengono usati automaticamente).
- Se si desidera eseguire un sito con una versione diversa di ASP.NET Web Pages, è possibile configurare il sito a tale scopo. Se il sito non ha ancora un *Web. config* file nella radice del sito, crearne uno nuovo e copiarvi il seguente codice XML, sovrascrivendo il contenuto esistente. Se il sito contiene già un *Web. config* file, aggiungere un `<appSettings>` elemento simile a quello riportato di seguito per il `<configuration>` sezione.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-Se non si specifica una versione di *Web. config* file, un sito viene distribuito come la versione più recente. (Gli assembly vengono copiati il *bin* cartella nel sito distribuito.)
- Le nuove applicazioni create utilizzando i modelli di sito Web matrice includono gli assembly della versione pagine Web del sito *bin* cartella.

In generale, è sempre possibile controllare quale versione delle pagine Web da usare con il sito usando NuGet per installare gli assembly appropriati al sito *bin* cartella. Per trovare i pacchetti, visitare [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Risorse aggiuntive

[Le funzionalità principali di ASP.NET Web Pages 2](top-features-in-web-pages-2.md)
