---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Creazione e utilizzo di un Helper in un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: Questo articolo descrive come creare un helper in un sito Web di ASP.NET Web Pages (Razor). Un helper è un componente riutilizzabile che include il codice e markup per le prestazioni...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5e73beabf973a20335112a0d7dc5ecb6b8817744
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394612"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Creazione e uso di un Helper in un sito ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come creare un helper in un sito Web di ASP.NET Web Pages (Razor). Oggetto *helper* è un componente riutilizzabile che include il codice e markup per eseguire un'attività che potrebbe essere noioso o complessi.
> 
> **Che cosa si apprenderà come:** 
> 
> - Come creare e usare un semplice helper.
> 
> Queste sono le funzionalità ASP.NET introdotte nell'articolo:
> 
> - Il `@helper` sintassi.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="overview-of-helpers"></a>Panoramica degli helper

Se è necessario eseguire le stesse attività in diverse pagine del sito, è possibile usare un file di supporto. Pagine Web ASP.NET include una serie di helper, ed esistono molte altre che è possibile scaricare e installare. (Un elenco degli helper incorporato in ASP.NET Web Pages è disponibile nel [riferimento rapido API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Se nessuno degli helper esistenti soddisfano le esigenze, è possibile creare il proprio supporto.

Un helper consente di usare un blocco di codice comune su più pagine. Si supponga che nella pagina spesso necessario creare un elemento di nota che è separato paragrafi normali. Forse la nota viene creata come un `<div>` elemento che ha l'aspetto di una casella con un bordo. Anziché aggiungere questo stesso markup per una pagina ogni volta che si desidera visualizzare una nota, è possibile creare un pacchetto del markup come supporto. È quindi possibile inserire la nota con una singola riga di codice ovunque ti serve.

Uso di un helper simile al seguente rende il codice in ognuna delle pagine più semplici e facili da leggere. Inoltre rende più semplice manutenzione del sito, poiché se è necessario modificare l'aspetto di note, è possibile modificare il markup in un'unica posizione.

## <a name="creating-a-helper"></a>Creazione di un Helper

Questa procedura illustra come creare l'helper che crea la nota, come descritto in precedenza. Questo è un esempio semplice, ma l'helper personalizzato può includere qualsiasi markup e codice ASP.NET che è necessario.

1. Nella cartella radice del sito Web, creare una cartella denominata *App\_codice*. Si tratta di un nome di cartella riservato in ASP.NET in cui è possibile inserire codice per i componenti come helper.
2. Nel *App\_codice* creare una nuova cartella *. cshtml* file e assegnargli il nome *MyHelpers.cshtml*.
3. Sostituire il contenuto esistente con il codice seguente:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Il codice Usa il `@helper` sintassi per dichiarare un nuovo helper denominato `MakeNote`. Questo particolare helper consente di passare un parametro denominato `content` che può contenere una combinazione di testo e markup. L'helper inserisce la stringa nella nota del corpo tramite la `@content` variabile.

    Si noti che il file è denominato *MyHelpers.cshtml*, ma l'helper è denominato `MakeNote`. È possibile inserire più helper personalizzati in un unico file.
4. Salvare e chiudere il file.

## <a name="using-the-helper-in-a-page"></a>Usando l'Helper in una pagina

1. Nella cartella radice, creare un nuovo file vuoto denominato *TestHelper.cshtml*.
2. Aggiungere al file il codice seguente:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Per chiamare il supporto è stato creato, usare `@` aggiungendo il nome del file in cui l'helper è, un punto e quindi il nome di supporto. (Se si dispone di più cartelle *App\_codice* cartella, è possibile utilizzare la sintassi `@FolderName.FileName.HelperName` per chiamare l'helper all'interno di qualsiasi nidificate a livello di cartella). Il testo che aggiunta in virgolette doppie all'interno delle parentesi è il testo che l'helper verrà visualizzato come parte della nota nella pagina web.
3. Salvare la pagina ed eseguirlo in un browser. L'helper che genera l'errore nota elemento a destra in cui è stato chiamato l'helper: tra i due paragrafi.

    ![Screenshot che mostra la pagina nel browser e modalità di generazione di markup che inserisce una casella intorno al testo specificato dell'helper.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Risorse aggiuntive


[Menu orizzontale come helper Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). In questo intervento sul blog di Mike Pope viene illustrato come creare un menu orizzontale come helper con markup, CSS e codice.

[Uso di HTML5 in ASP.NET Web Pages gli helper per WebMatrix e ASP.NET MVC 3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Questo post di blog da Sam Abraham Mostra un helper che esegue il rendering di un HTML5 `Canvas` elemento.

[La differenza tra @Helpers e @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Viene descritto questo intervento sul blog di Mike Brind `@helper` sintassi e `@function` sintassi e il loro utilizzo.
