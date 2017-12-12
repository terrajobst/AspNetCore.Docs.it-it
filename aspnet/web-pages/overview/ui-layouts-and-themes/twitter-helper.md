---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Helper con le pagine Web ASP.NET di Twitter | Documenti Microsoft
author: tfitzmac
description: Questo argomento e l'applicazione viene illustrato come aggiungere un Helper di Twitter per WebMatrix 3 progetto. Contiene il codice Helper di Twitter e viene illustrato come chiamare il supporto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Helper di Twitter con le pagine Web ASP.NET
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo argomento e l'applicazione viene illustrato come aggiungere un Helper di Twitter per WebMatrix 3 progetto. Contiene il codice Helper di Twitter e viene illustrato come chiamare i metodi di supporto.
> 
> Questo codice per il file Twitter.cshtml è stato sviluppato da **Tian Pan** di Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="introduction"></a>Introduzione

In questo argomento viene illustrato come aggiungere un Helper di Twitter per l'applicazione e usare la sintassi Razor per chiamare i metodi di supporto. L'Helper di Twitter semplifica incorporare i pulsanti di Twitter e widget nell'applicazione. Per utilizzare un widget di Twitter, ad esempio la sequenza temporale di un utente o i risultati della ricerca per un hashtag, è necessario creare innanzitutto il [widget su Twitter](https://twitter.com/settings/widgets). Dopo aver creato il widget, si riceverà un id widget. Questo id widget è passare come parametro quando si chiama i metodi di supporto che mostrano i widget.

In questo argomento è stato scritto per la versione 1.1 dell'API di Twitter. Aggiungendo direttamente il codice Helper di Twitter al progetto, è possibile aggiornare il codice di supporto, se viene modificata l'API di Twitter.

Per informazioni sull'installazione di WebMatrix, vedere [Introduzione a ASP.NET Web Pages 2 - Guida introduttiva](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Aggiungere al progetto di Helper di Twitter

Per aggiungere l'Helper di Twitter, aggiungere innanzitutto una cartella denominata **App\_codice** al progetto. Quindi, creare un file denominato **Twitter.cshtml**.

![Nella cartella App_Code](twitter-helper/_static/image1.png)

Sostituire il codice predefinito in Twitter.cshtml con il codice seguente.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Chiamare i metodi di Twitter dalle pagine web

Nell'esempio seguente viene illustrato come utilizzare i metodi Helper di Twitter da una pagina nel progetto. Nel progetto, si dovranno sostituire i valori dei parametri con valori che sono rilevanti per le esigenze. È possibile utilizzare gli ID widget fornito per l'esplorazione di funzionamento dei metodi, ma è possibile generare i propri widget per il progetto.

Non tutti i parametri riportati di seguito sono obbligatori. I parametri facoltativi consentono di personalizzare come viene visualizzato il pulsante o il widget. Ad esempio, il pulsante seguire richiede solo il nome utente da seguire, ma nell'esempio viene illustrato come includere il numero di anelli e come specificare le dimensioni del pulsante e la lingua.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Visualizzare i risultati

Il codice precedente produce i seguenti pulsanti e widget. Questi pulsanti e widget sono completamente funzionali, non le schermate. Il pulsante seguire viene visualizzato in spagnolo perché il parametro language è impostato su **es**.

### <a name="follow-button"></a>Seguire pulsante

[Seguire @aspnet)](https://twitter.com/aspnet)<script>! funzione (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'. Se (! d.getElementById(id)) {js = d.createElement(s); js.id = id, js.src = p + ': / / platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore (con estensione js, fjs);}} (documento, 'script', 'twitter wjs');</script>

### <a name="tweet-button"></a>Pulsante TWEET

[TWEET](https://twitter.com/share)<script>! funzione (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'. Se (! d.getElementById(id)) {js = d.createElement(s); js.id = id, js.src = p + ': / / platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore (con estensione js, fjs);}} (documento, 'script', 'twitter wjs');</script>

### <a name="user-timeline-profile"></a>Sequenza temporale utente (profilo)

[TWEET da @aspnet ](https://twitter.com/aspnet) <script>! funzione (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'. Se (! d.getElementById(id)) {js = d.createElement(s); js.id = id, js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (con estensione js, fjs);}} (documento, "script", "twitter wjs");</script>

### <a name="favorites"></a>Preferiti

[TWEET preferito da @Microsoft ](https://twitter.com/Microsoft/favorites) <script>! funzione (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'. Se (! d.getElementById(id)) {js = d.createElement(s); js.id = id, js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (con estensione js, fjs);}} (documento, "script", "twitter wjs");</script>

### <a name="list"></a>Elenco

[TWEET da @Microsoft/MS \_Consumer\_bande](https://twitter.com/microsoft/ms-consumer-brands/)<script>! funzione (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'. Se (! d.getElementById(id)) {js = d.createElement(s); js.id = id, js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (con estensione js, fjs);}} (documento, "script", "twitter wjs");</script>

### <a name="search"></a>Cerca

[TWEET su &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>! funzione (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'. Se (! d.getElementById(id)) {js = d.createElement(s); js.id = id, js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (con estensione js, fjs);}} (documento, "script", "twitter wjs");</script>
