---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Creazione e utilizzo di un Helper in un Web ASP.NET, le pagine del sito (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo articolo viene descritto come creare un supporto in un sito Web ASP.NET Web Pages (Razor). Un helper è un componente riutilizzabile che include codice e markup perf...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530000"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Creazione e utilizzo di un Helper in un sito ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene descritto come creare un supporto in un sito Web ASP.NET Web Pages (Razor). Oggetto *helper* è un componente riutilizzabile che include codice e markup per eseguire un'attività che potrebbe essere noiosa o complessi.
> 
> **Illustra quanto segue:** 
> 
> - Come creare e utilizzare un helper semplice.
> 
> Queste sono le funzionalità ASP.NET introdotte nell'articolo:
> 
> - Il `@helper` sintassi.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="overview-of-helpers"></a>Panoramica di helper

Se è necessario eseguire le stesse operazioni in diverse pagine del sito, è possibile utilizzare un file di supporto. Esistono molti altri che è possibile scaricare e installare ASP.NET Web Pages include un numero di helper. (Un elenco degli helper incorporato in ASP.NET Web Pages è disponibile nel [ASP.NET riferimento rapido di API](https://go.microsoft.com/fwlink/?LinkId=202907).) Se nessuno degli helper esistente alle proprie esigenze, è possibile creare la propria helper.

Un helper consente di usare un blocco di codice comune in più pagine. Si supponga che nella pagina è spesso necessario creare un elemento di nota che è separato paragrafi normali. Ad esempio la nota viene creata come un `<div>` elemento che ha l'aspetto di una casella con un bordo. Anziché aggiungere il markup stesso a una pagina ogni volta che si desidera visualizzare una nota, è possibile comprimere il markup come supporto. È quindi possibile inserire la nota con una singola riga di codice in qualsiasi punto è necessario.

Utilizzando un supporto di questo rende il codice in ognuna delle pagine più semplice e più facile da leggere. Rende inoltre più semplice gestire il sito, poiché se è necessario modificare l'aspetto delle note, è possibile modificare il markup in un'unica posizione.

## <a name="creating-a-helper"></a>Creazione di un Helper

In questa procedura viene illustrato come creare l'helper che crea la nota, come descritto in precedenza. Questo è un esempio semplice, ma l'helper personalizzato può includere qualsiasi markup e codice ASP.NET che è necessario.

1. Nella cartella radice del sito Web, creare una cartella denominata *App\_codice*. Questo è un nome di cartella riservato in ASP.NET in cui è possibile inserire codice per i componenti come helper.
2. Nel *App\_codice* cartella creare un nuovo *. cshtml* file e denominarlo *MyHelpers.cshtml*.
3. Sostituire il contenuto esistente con il seguente:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Il codice Usa il `@helper` sintassi per dichiarare un nuovo supporto denominato `MakeNote`. Questo particolare supporto consente di passare un parametro denominato `content` che può contenere una combinazione di testo e markup. L'helper inserisce la stringa nel corpo nota usando il `@content` variabile.

    Si noti che il file è denominato *MyHelpers.cshtml*, ma l'helper è denominata `MakeNote`. È possibile inserire più helper personalizzati in un singolo file.
4. Salvare e chiudere il file.

## <a name="using-the-helper-in-a-page"></a>Utilizzando l'Helper in una pagina

1. Nella cartella radice, creare un nuovo file vuoto denominato *TestHelper.cshtml*.
2. Aggiungere al file il codice seguente:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Per chiamare il supporto è stato creato, utilizzare `@` aggiungendo il nome del file in cui l'helper è un punto, quindi il nome di supporto. (Se si dispone di più cartelle *App\_codice* cartella, è possibile utilizzare la sintassi `@FolderName.FileName.HelperName` per chiamare il supporto all'interno di qualsiasi annidate a livello di cartella). Il testo che aggiunta virgolette all'interno delle parentesi è il testo da Visualizza come parte della nota nella pagina web l'helper.
3. Salvare la pagina ed eseguirlo in un browser. L'helper genera l'elemento di nota a destra in cui è stato chiamato l'helper: tra i due paragrafi.

    ![Screenshot che illustra la pagina nel browser e la modalità di generazione di codice che inserisce una casella di testo specificato l'helper.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Risorse aggiuntive


[Menu orizzontale come supporto Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Questo post di blog da Mike Pope viene illustrato come creare un menu orizzontale come supporto tramite markup, CSS e codice.

[Sfruttando HTML5 in ASP.NET Web Pages helper per WebMatrix e ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Questo post di blog da Sam Abraham Mostra un helper che esegue il rendering di un HTML5 `Canvas` elemento.

[La differenza tra @Helpers e @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Viene descritto questo post di blog da Mike Brind `@helper` sintassi e `@function` sintassi e il loro utilizzo.
