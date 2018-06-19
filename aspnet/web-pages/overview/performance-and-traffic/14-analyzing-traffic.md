---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Informazioni relative ai visitatori (Analitica) di rilevamento per un ASP.NET Web Pages sito (Razor) | Documenti Microsoft
author: tfitzmac
description: Dopo è stato utilizzato il sito Web prevede, è possibile analizzare il traffico del sito Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528760"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Informazioni di rilevamento visitatore (Analitica) per un sito Web di ASP.NET di pagine (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come utilizzare un file di supporto per aggiungere analitica sito Web alle pagine in un sito Web ASP.NET Web Pages (Razor).
> 
> Illustra quanto segue:
> 
> - Come inviare il traffico del sito Web di informazioni a un provider analitica.
> 
> Si tratta di programmazione delle funzionalità introdotte nell'articolo ASP.NET:
> 
> - Il `Analytics` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - ASP.NET Web Helpers Library (pacchetto NuGet)


Analitica è un termine generale per la tecnologia che misura il traffico nel sito Web in modo è possibile comprendere la modalità di utilizzo del sito. Molti servizi analitica sono disponibili, inclusi i servizi da Google, Yahoo, StatCounter e altri utenti.

Analitica il modo works è che si esegue l'iscrizione per un account con il provider analitica, in cui si registra il sito che si desidera tenere traccia. Il provider invia un frammento di codice JavaScript che include un ID o il rilevamento del codice per l'account. Aggiungere il frammento di codice JavaScript per iniziare a utilizzare le pagine web per il sito che si desidera tenere traccia. (Si aggiunge in genere il frammento analitica a una pagina di piè di pagina o il layout o altro markup HTML che viene visualizzato in ogni pagina del sito.) Quando gli utenti richiedono una pagina contenente uno di questi frammenti di codice JavaScript, il frammento di codice invia informazioni relative alla pagina corrente per il provider analitica, che registra diversi dettagli sulla pagina.

Quando si desidera esaminare le statistiche del sito, accedere al sito Web del provider analitica. È quindi possibile visualizzare tutti i tipi di report sul sito, ad esempio:

- Il numero di visualizzazioni di pagina per le singole pagine. In questo modo si (approssimativamente) quante persone hanno visita il sito e le pagine del sito sono i più diffusi.
- Il tempo dedicato alle attività in pagine specifiche. Questo può indicare ad esempio se la home page è mantenendo interesse degli utenti.
- Le persone siti presenti prima che visita il sito. Questo aiuta a capire se il traffico in ingresso dai collegamenti, da ricerche e così via.
- Quando gli utenti visitano il sito e tempi di permanenza.
- In quali paesi sono compresi tra i visitatori.
- Il browser e sistemi operativi utilizza i visitatori.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Utilizzo di un Helper per aggiungere Analitica a una pagina

Pagine Web ASP.NET include diversi helper analitica (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, e `Analytics.GetStatCounterHtml`) che rendono più semplice gestire i frammenti di codice JavaScript utilizzati per analitica. Anziché pensare a come e dove inserire il codice JavaScript, è necessario effettuare è aggiungere il supporto a una pagina. L'unica informazione che è necessario fornire è il nome dell'account, un ID o un codice di rilevamento. (Per StatCounter, è inoltre necessario fornire alcuni valori aggiuntivi.)

In questa procedura, si creerà una pagina di layout che utilizza il `GetGoogleHtml` helper. Se si dispone già di un account con uno degli altri provider analitica, è possibile utilizzare tale account e apportare piccole modifiche alle esigenze.

> [!NOTE]
> Quando si crea un account analitica, registrare l'URL del sito che si desidera essere rilevamento. Se si sta testando tutti gli elementi presenti nel computer locale, non sarà necessario registrare traffico effettivo (il solo traffico è è), in modo non sarà in grado di registrare e visualizzare le statistiche del sito. Ma questa procedura viene illustrato come aggiungere un helper analitica a una pagina. Quando si pubblica il sito, il sito in tempo reale invia informazioni al provider analitica.


1. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non si già aggiunti.
2. Creare un account con Google Analitica e registrare il nome dell'account.
3. Creare una pagina di layout denominata *Analytics.cshtml* e aggiungere il markup seguente:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > È necessario inserire la chiamata al `Analytics` helper nel corpo della pagina web (prima di `</body>` tag). In caso contrario, il browser non eseguirà lo script.

    Se si utilizza un provider diverso analitica, utilizzare uno degli helper seguenti:

    - (Yahoo)`@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`
4. Sostituire `myaccount` con il nome dell'account, ID o codice di rilevamento creato nel passaggio 1.
5. Eseguire la pagina nel browser. (Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.)
6. Nel browser, visualizzare l'origine della pagina. Sarà in grado di visualizzare il codice sottoposto a rendering analitica:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Accedere al sito di Google Analitica ed esaminare le statistiche per il sito. Se si esegue la pagina in un sito in tempo reale, è visualizzata una voce che registra la visita a una pagina.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Sito di Google Analitica](https://www.google.com/analytics/)
- [Yahoo! Analitica sito Web](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Sito StatCounter](http://statcounter.com/)
