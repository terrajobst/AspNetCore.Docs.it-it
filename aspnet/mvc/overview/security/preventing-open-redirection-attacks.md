---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Prevenzione degli attacchi di reindirizzamento aprire (c#) | Documenti Microsoft
author: jongalloway
description: "In questa esercitazione viene illustrato come è possibile impedire gli attacchi di reindirizzamento aperti nelle applicazioni ASP.NET MVC. In questa esercitazione vengono illustrate le modifiche apportate..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 17944c0600a174176e3e9940f414b34f0835b800
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
<a name="preventing-open-redirection-attacks-c"></a>Prevenzione degli attacchi di reindirizzamento aprire (c#)
====================
da [Jon Galloway](https://github.com/jongalloway)

> In questa esercitazione viene illustrato come è possibile impedire gli attacchi di reindirizzamento aperti nelle applicazioni ASP.NET MVC. In questa esercitazione vengono illustrate le modifiche apportate in AccountController in ASP.NET MVC 3 e viene illustrato come è possibile applicare queste modifiche nelle 2 applicazioni e la versione 1.0 di MVC ASP.NET esistente.


## <a name="what-is-an-open-redirection-attack"></a>Che cos'è un attacco di reindirizzamento aprire?

Qualsiasi applicazione web che reindirizza a un URL che viene specificato tramite la richiesta, ad esempio i dati di stringa di query o i form può essere alterati potenzialmente per reindirizzare gli utenti a un URL esterno, dannoso. Questo tipo di manomissione viene chiamato un attacco di reindirizzamento aperto.

Ogni volta che la logica dell'applicazione reindirizza a un URL specificato, è necessario verificare che l'URL di reindirizzamento non è stato alterato. L'account di accesso utilizzato nel AccountController predefinito per la versione 1.0 di ASP.NET MVC e ASP.NET MVC 2 è vulnerabile agli attacchi di reindirizzamento di aprire. Fortunatamente, è facile aggiornare le applicazioni esistenti per utilizzare le correzioni dall'anteprima ASP.NET MVC 3.

Per comprendere la vulnerabilità, esaminiamo il funzionamento del reindirizzamento di account di accesso in un progetto di applicazione Web ASP.NET MVC 2 predefinito. In questa applicazione, il tentativo di visitare un'azione del controller che ha l'attributo [Authorize] reindirizzerà gli utenti non autorizzati alla visualizzazione /Account/LogOn. Il reindirizzamento a /Account/LogOn includerà un parametro querystring returnUrl in modo che l'utente può essere restituito all'URL richiesto originariamente dopo che hanno effettuato correttamente l'accesso.

Nella schermata seguente, possiamo vedere che un tentativo di accedere alla visualizzazione /Account/ChangePassword quando non è connesso determina un reindirizzamento per /Account/LogOn? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Figura 01**: pagina di accesso con un reindirizzamento aperto

Poiché il parametro querystring ReturnUrl non viene convalidato, un utente malintenzionato può modificarlo per inserire tutti gli indirizzi URL nel parametro per eseguire un attacco di reindirizzamento aperto. Per illustrare questo comportamento, è possibile modificare il parametro ReturnUrl in [http://bing.com](http://bing.com), in modo che l'URL di accesso risultante sarà/Account/accesso? ReturnUrl = http://www.bing.com/. Seguito all'accesso al sito, si viene reindirizzati alla [http://bing.com](http://bing.com). Poiché il reindirizzamento non viene convalidato, potrebbe invece fare riferimento a un sito che è stato eseguito un tentativo di ingannare l'utente.

### <a name="a-more-complex-open-redirection-attack"></a>Un attacco di reindirizzamento aprire più complessi

Gli attacchi di reindirizzamento aprire sono particolarmente pericolosi poiché un utente malintenzionato a conoscenza che si sta tentando di accedere a un sito Web specifico, rendendo ci vulnerabile a un [attacco di phishing](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Ad esempio, un utente malintenzionato potrebbe inviare messaggi di posta elettronica agli utenti del sito Web nel tentativo di acquisire le proprie password. Si esaminerà questo funzionamento nel sito NerdDinner. Si noti che il sito NerdDinner in tempo reale è stato aggiornato per proteggersi da attacchi di reindirizzamento aperto.

Prima di tutto, un utente malintenzionato invia a un collegamento alla pagina di accesso su NerdDinner che include un reindirizzamento alla pagina contraffatta:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Si noti che l'URL restituito punta a nerddiner.com, che non contiene una "n" da dinner di word. In questo esempio, si tratta di un dominio che controlla l'autore dell'attacco. Quando si accede al collegamento riportato sopra, si verrà visualizzate la pagina di account di accesso NerdDinner.com legittima.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Figura 02**: NerdDinner pagina di accesso con un reindirizzamento aperto

Quando verrà eseguito correttamente l'accesso, azione di accesso del AccountController di MVC ASP.NET ci reindirizza all'URL specificato nel parametro querystring returnUrl. In questo caso, è l'URL che ha immesso l'autore dell'attacco, ovvero [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn). A meno che non si è estremamente consentono, che è molto probabile che è non sarà possibile notare, soprattutto perché l'autore dell'attacco è stato attenzione per assicurarsi che le pagine manomesse aspetto esattamente la pagina di accesso legittimo. Questa pagina di accesso include un messaggio di errore che richiede che è nuovo l'accesso. Ingombranti, è necessario avere digitato correttamente la password.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Figura 03**: schermata di accesso di NerdDinner contraffatte

Quando si ridigitare il nome utente e password, la pagina di accesso contraffatta Salva le informazioni e ci è stata inviata al sito NerdDinner.com legittimo. A questo punto, il sito NerdDinner.com è già autenticato, in modo possibile reindirizzare la pagina di accesso contraffatta direttamente a tale pagina. Il risultato finale è che l'utente malintenzionato ha il nome utente e password e siamo consapevoli che è stato fornito ad essi.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Esaminando il codice nell'azione accesso AccountController vulnerabile

Il codice per l'azione di accesso in un'applicazione ASP.NET MVC 2 è illustrato di seguito. Si noti che al momento di un account di accesso ha esito positivo, il controller restituisca un reindirizzamento per l'elemento returnUrl. Si noterà che non viene eseguita alcuna convalida rispetto al parametro returnUrl.

**1: azione di accesso di ASP.NET MVC 2 nell'elenco`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Ora esaminiamo le modifiche all'azione di accesso di ASP.NET MVC 3. Questo codice è stato modificato per convalidare il parametro returnUrl chiamando un nuovo metodo nella classe helper System.Web.Mvc.Url denominata `IsLocalUrl()`.

**2: azione di ASP.NET MVC 3 accesso nell'elenco`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Questo è stato modificato per convalidare il parametro dell'URL restituito chiamando un nuovo metodo nella classe helper System.Web.Mvc.Url `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Protezione di MVC 2 ASP.NET MVC 1,0 e applicazioni

È possibile sfruttare le modifiche di ASP.NET MVC 3 nelle 2 applicazioni e la versione 1.0 di MVC ASP.NET esistente aggiungendo il metodo di supporto IsLocalUrl() e aggiornando l'azione di accesso per convalidare il parametro returnUrl.

Il metodo UrlHelper IsLocalUrl() semplicemente la chiamata in un metodo in System.Web.WebPages, come la convalida viene utilizzato anche dalle applicazioni ASP.NET Web Pages.

**Elenco di 3: il metodo IsLocalUrl() il UrlHelper 3 MVC ASP.NET`class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Il metodo IsUrlLocalToHost contiene la logica di convalida effettiva, come illustrato nel listato 4.

**Elenco di 4-metodo IsUrlLocalToHost() dalla classe System.Web.WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

La versione 1.0 di ASP.NET MVC o dell'applicazione 2, verrà aggiunto un metodo IsLocalUrl() per il AccountController, ma si consiglia di aggiungerlo a una classe di supporto separato, se possibile. Si renderà due piccole modifiche alla versione ASP.NET MVC 3 di IsLocalUrl() in modo che funzioni all'interno di AccountController. Prima di tutto, verranno modificate, da un metodo pubblico a un metodo privato, poiché i metodi pubblici nel controller accessibile come azioni del controller. In secondo luogo, verrà modificata la chiamata che controlla l'host dell'URL con l'applicazione host. Che chiamata viene utilizzato un RequestContext locale campo nella classe UrlHelper. Anziché. RequestContext.HttpContext.Request.Url.Host, questa tabella verrà usata. Request.Url.Host. Il codice seguente è illustrato il metodo di IsLocalUrl() modificato per l'utilizzo con una classe controller in ASP.NET MVC 1.0 e 2 applicazioni.

**Elenco di 5-metodo IsLocalUrl(), che viene modificato per l'utilizzo con una classe Controller MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Ora che il metodo IsLocalUrl() è sul posto, è possibile chiamarlo dall'azione di accesso per convalidare il parametro returnUrl, come illustrato nel codice seguente.

**Elenco di 6: metodo di accesso aggiornate che convalida il parametro returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

È ora possibile testare un attacco di reindirizzamento aprire tentando di accedere con un URL esterno restituito. Consente di usare Account/accesso? ReturnUrl = http://www.bing.com/ nuovamente.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Figura 04**: l'azione di accesso aggiornate di test

Dopo aver completato l'accesso, si viene reindirizzati di azione del Controller Home/Index anziché l'URL esterno.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Figura 05**: attacco reindirizzamento aprire annullato

## <a name="summary"></a>Riepilogo

Gli attacchi di reindirizzamento aprire possono verificarsi quando il reindirizzamento URL vengono passati come parametri nell'URL per un'applicazione. Il modello include il codice per la protezione da ASP.NET MVC 3 aprire gli attacchi di reindirizzamento. È possibile aggiungere questo codice con una modifica 2 applicazioni e MVC ASP.NET 1.0. Per proteggere da attacchi di reindirizzamento aperto durante l'accesso a ASP.NET 1.0 e 2 applicazioni, aggiungere un metodo IsLocalUrl() e convalidare il parametro returnUrl nell'azione di accesso.
