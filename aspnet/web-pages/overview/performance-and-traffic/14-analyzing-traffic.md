---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Rilevamento delle informazioni relative ai visitatori (Analitica) per un ASP.NET Web Pages (Razor) del sito | Microsoft Docs
author: Rick-Anderson
description: Dopo che è stato utilizzato il sito Web sarà, è possibile analizzare il traffico dei siti Web.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 57e6a0d4681f147faa5e9ca3b6ed0ef287d6a381
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020884"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Informazioni di rilevamento del visitatore (Analitica) per un sito di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come usare un file di supporto per aggiungere analitica di sito Web alle pagine in un sito Web ASP.NET Web Pages (Razor).
> 
> Che cosa si apprenderà come:
> 
> - Come inviare le informazioni sul traffico del sito Web a un provider di analitica.
> 
> Queste sono le funzionalità introdotte nell'articolo di programmazione ASP.NET:
> 
> - Il `Analytics` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web Helpers Library (pacchetto NuGet)


Analitica è un termine generale per la tecnologia che misura il traffico nel sito Web in modo che è possibile comprendere come le persone usano il sito. Molti servizi di analitica sono disponibili, inclusi i servizi di Google, Yahoo, StatCounter e ad altri utenti.

L'analitica modo funziona è che si iscriversi a un account con il provider di analitica, in cui si registra il sito che si vuole tenere traccia. Il provider invia un frammento di codice JavaScript che include un ID o il rilevamento del codice per l'account. Aggiungere il frammento JavaScript alle pagine web per il sito che si vuole tenere traccia. (È in genere aggiungere il frammento di analitica a una pagina layout o nel piè di pagina o altro markup HTML visualizzato in ogni pagina nel sito.) Quando gli utenti richiedono una pagina che contiene uno di questi frammenti di codice JavaScript, il frammento di codice invia informazioni relative alla pagina corrente per il provider di analitica, che registra informazioni dettagliate diverse relative alla pagina.

Quando si desidera esaminare le statistiche del sito, accedere al sito Web del provider analitica. È quindi possibile visualizzare tutti i tipi di report sul sito, ad esempio:

- Il numero di visualizzazioni di pagina per le singole pagine. Ciò indica (approssimativamente) quante persone hanno sta visitando il sito e le pagine nel sito sono quelli più diffusi.
- Le persone quanto tempo impiegano in pagine specifiche. Ciò può indicare elementi come se la home page mantiene gli interessi delle persone.
- Le persone siti presenti prima che visita il sito. Questo aiuta a capire se il traffico proviene da collegamenti, da ricerche e così via.
- Quando gli utenti visitano il sito e i tempi di permanenza.
- In quali paesi sono visitatori.
- Quali browser e sistemi operativi usano visitatori.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Uso di un Helper per aggiungere Analitica a una pagina

Pagine Web ASP.NET include alcuni helper analitica (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, e `Analytics.GetStatCounterHtml`) che rendono più semplice gestire i frammenti di codice JavaScript utilizzati per analitica. Invece di consiste nel capire come e dove inserire il codice JavaScript, tutto ciò che devi fare è aggiungere l'helper a una pagina. Le uniche informazioni da specificare sono il nome dell'account, ID o codice di rilevamento. (Per StatCounter, è anche necessario specificare alcuni valori aggiuntivi.)

In questa procedura si creerà una pagina di layout che utilizza il `GetGoogleHtml` helper. Se hai già un account con uno degli altri provider di analitica, è possibile usare invece l'account e apportare delle lievi modifiche necessarie.

> [!NOTE]
> Quando si crea un account di analitica, registrare l'URL del sito che si desidera essere tracciati. Se si sta testando tutto nel computer locale, non sarà necessario registrare effettivo del traffico (il solo traffico è si), in modo non sarà in grado di registrare e visualizzare le statistiche del sito. Ma questa procedura viene illustrato come aggiungere un helper di analitica a una pagina. Quando si pubblica il sito, il sito live verrà inviate informazioni al provider di analitica.


1. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non si già aggiunti.
2. Creare un account con Google Analitica e registrare il nome dell'account.
3. Creare una pagina di layout denominata *Analytics.cshtml* e aggiungere il markup seguente:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > È necessario inserire la chiamata ai `Analytics` helper nel corpo della pagina web (prima di `</body>` tag). In caso contrario, il browser non verrà eseguito lo script.

    Se si usa un provider di analitica diverso, usare uno degli helper seguenti:

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Sostituire `myaccount` con il nome dell'account, ID o codice di rilevamento creato nel passaggio 1.
5. Eseguire la pagina nel browser. (Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.)
6. Nel browser, visualizzare l'origine della pagina. Sarà in grado di visualizzare il codice sottoposto a rendering analitica:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Accedere al sito di Google Analitica ed esaminare le statistiche per il sito. Se si esegue la pagina in un sito live, viene visualizzata una voce che registra la visita la pagina.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Sito di Google Analitica](https://www.google.com/analytics/)
- [Yahoo! Sito Web Analytics](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Sito StatCounter](http://statcounter.com/)
