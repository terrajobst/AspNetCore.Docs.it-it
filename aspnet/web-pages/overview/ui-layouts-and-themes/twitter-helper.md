---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Helper di Twitter con pagine Web ASP.NET | Microsoft Docs
author: tfitzmac
description: Questo argomento e l'applicazione illustrano come aggiungere un Helper di Twitter per WebMatrix 3 progetto. Contiene il codice Helper di Twitter e viene illustrato come chiamare il supporto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 2c84a986a39f6802a78df53510847cb70efbb0f2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373023"
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Helper di Twitter con pagine Web ASP.NET
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo argomento e l'applicazione illustrano come aggiungere un Helper di Twitter per WebMatrix 3 progetto. Contiene il codice Helper di Twitter e viene illustrato come chiamare i metodi di supporto.
> 
> Questo codice per il file Twitter.cshtml sviluppato **Tian Pan** di Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="introduction"></a>Introduzione

In questo argomento viene illustrato come aggiungere un Helper di Twitter per l'applicazione e usare la sintassi Razor per chiamare i metodi helper. L'Helper di Twitter rende più facile incorporare widget nell'applicazione e i pulsanti di Twitter. Per usare un widget di Twitter, ad esempio diario dell'utente o i risultati della ricerca per un hashtag, è necessario creare innanzitutto le [widget su Twitter](https://twitter.com/settings/widgets). Dopo aver creato il widget, si riceverà un id widget. Questo id widget passare come parametro quando si chiamano i metodi di supporto che mostrano i widget.

In questo argomento è stato scritto per la versione 1.1 dell'API di Twitter. Aggiungendo direttamente il codice Helper di Twitter per il progetto, è possibile aggiornare il codice helper se cambia l'API di Twitter.

Per informazioni sull'installazione di WebMatrix, vedere [Introduzione a ASP.NET Web Pages 2 - Introduzione a](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Aggiungere al progetto Helper di Twitter

Per aggiungere l'Helper di Twitter, in primo luogo, aggiungere una cartella denominata **App\_codice** al progetto. Quindi, creare un file denominato **Twitter.cshtml**.

![Nella cartella App_Code](twitter-helper/_static/image1.png)

Sostituire il codice predefinito in Twitter.cshtml con il codice seguente.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Chiamare i metodi di Twitter dalle pagine web

Nell'esempio seguente viene illustrato come utilizzare i metodi Helper di Twitter da una pagina nel progetto. Nel progetto, si dovranno sostituire i valori dei parametri con valori attinenti alle proprie esigenze. È possibile usare gli ID widget forniti per esplorare il funzionamento dei metodi, ma è opportuno generare widget personalizzati per il progetto.

Non tutti i parametri riportati di seguito sono obbligatori. I parametri facoltativi vengono utilizzati per personalizzare come viene visualizzato il pulsante o un widget. Ad esempio, il pulsante seguire richiede solo il nome utente da seguire, ma l'esempio illustra come includere il numero di follower e come specificare le dimensioni del pulsante e la lingua.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Visualizzare i risultati

Il codice precedente produce i seguenti pulsanti e widget. Questi pulsanti e widget sono completamente funzionali, non schermate. Il pulsante seguire viene visualizzato in spagnolo perché il parametro language è impostato su **es**.

### <a name="follow-button"></a>Seguire pulsante

[Seguire @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Pulsante del TWEET

[TWEET](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>Sequenza temporale dell'utente (profilo)

[TWEET da @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Preferiti

[TWEET a Preferiti @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>List

[Da un TWEET @Microsoft/MS \_Consumer\_bande](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Cerca

[I TWEET su &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
