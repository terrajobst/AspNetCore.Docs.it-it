---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Prevenzione degli attacchi di reindirizzamento aperti (c#) | Microsoft Docs
author: jongalloway
description: Questa esercitazione illustra come evitare attacchi di reindirizzamento aperti nelle applicazioni ASP.NET MVC. Questa esercitazione vengono illustrate le modifiche apportate...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 767f9c85527fbcdf34e700eb32fe0c6cad30bf0c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829506"
---
<a name="preventing-open-redirection-attacks-c"></a>Prevenzione degli attacchi di reindirizzamento aperti (c#)
====================
da [Jon Galloway](https://github.com/jongalloway)

> Questa esercitazione illustra come evitare attacchi di reindirizzamento aperti nelle applicazioni ASP.NET MVC. Questa esercitazione vengono illustrate le modifiche apportate in AccountController in ASP.NET MVC 3 e viene illustrato come è possibile applicare queste modifiche nelle 2 applicazioni e la versione esistente di ASP.NET MVC 1.0.


## <a name="what-is-an-open-redirection-attack"></a>Che cos'è un attacco di reindirizzamento aperti?

Qualsiasi applicazione web che reindirizza a un URL che viene specificato tramite la richiesta, ad esempio i dati della stringa di query o form potenzialmente può essere manomessi per reindirizzare gli utenti a un URL esterno, dannoso. Questo manomissioni, si tratta di un attacco di reindirizzamento aperti.

Ogni volta che la logica dell'applicazione reindirizza a un URL specificato, è necessario verificare che l'URL di reindirizzamento non è stato manomesso. Account di accesso utilizzato nel AccountController predefinito per ASP.NET MVC 1.0 e ASP.NET MVC 2 è vulnerabile agli attacchi di reindirizzamento di aprire. Fortunatamente, è facile aggiornare le applicazioni esistenti per utilizzare le correzioni alla versione di anteprima di ASP.NET MVC 3.

Per comprendere la vulnerabilità, verrà ora esaminato il funzionamento del reindirizzamento di account di accesso in un progetto di applicazione ASP.NET MVC 2 Web predefinito. In questa applicazione, provare a visitare un'azione del controller che ha l'attributo [Authorize] reindirizzerà gli utenti non autorizzati alla visualizzazione /Account/LogOn. Il reindirizzamento a /Account/LogOn sarà incluso un parametro di stringa di query returnUrl in modo che l'utente può essere restituito all'URL richiesto originariamente dopo che hanno eseguito l'accesso.

Nella schermata seguente, si noterà che un tentativo di accedere alla visualizzazione /Account/ChangePassword quando non è connesso determina un reindirizzamento /Account/LogOn? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Figura 01**: pagina di accesso con un reindirizzamento aperto

Poiché il parametro di stringa di query ReturnUrl non viene convalidato, un utente malintenzionato può modificare in modo da inserire qualsiasi indirizzo URL il parametro per sferrare un attacco di reindirizzamento aperti. Per dimostrare questo concetto, è possibile modificare il parametro ReturnUrl [ http://bing.com ](http://bing.com), quindi l'URL di accesso risultante sarà/Account/LogOn? ReturnUrl =<http://www.bing.com/>. Dopo l'accesso correttamente al sito, si verrà reindirizzati alla [ http://bing.com ](http://bing.com). Poiché il reindirizzamento non viene convalidato, potrebbe invece fare riferimento a un sito dannoso che tenta di ingannare l'utente.

### <a name="a-more-complex-open-redirection-attack"></a>Un attacco di reindirizzamento aperti più complessi

Attacchi di reindirizzamento aperti sono particolarmente pericolosi in quanto un autore dell'attacco sa che stiamo tentando di accedere a un sito Web specifico, che ci rende vulnerabile a un [attacchi di phishing](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Ad esempio, un utente malintenzionato invia messaggi di posta elettronica agli utenti di siti Web nel tentativo di acquisire le proprie password. Esaminiamo questo funzionamento nel sito di NerdDinner. Si noti che il sito di NerdDinner in tempo reale è stato aggiornato per proteggersi da attacchi di reindirizzamento aperti.

Prima di tutto un utente malintenzionato invia a un collegamento alla pagina di accesso su NerdDinner che include un reindirizzamento alla rispettiva pagina falsificato:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Si noti che l'URL restituito punta nerddiner.com, che non è presente una "n" dalla cena word. In questo esempio, si tratta di un dominio che controlla l'autore dell'attacco. Quando si accede al collegamento precedente, abbiamo stiamo impiegati per la pagina di accesso di NerdDinner.com legittima.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Figura 02**: pagina di accesso di NerdDinner con un reindirizzamento aperto

Quando verrà eseguito correttamente l'accesso, azione di accesso del AccountController MVC ASP.NET us reindirizza all'URL specificato nel parametro di stringa di query returnUrl. In questo caso, è l'URL che ha immesso l'autore dell'attacco, ovvero [ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn). A meno che non siamo estremamente vigile, che è molto probabile che è non sarà possibile notare, in particolare perché l'autore dell'attacco è stato attenzione per assicurarsi che le pagine manomesse cerca esattamente come la pagina di accesso legittimo. Questa pagina di accesso include un messaggio di errore che richiede che si accedere di nuovo. Il goffo us, è necessario avere digitato correttamente la password.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Figura 03**: schermata di accesso di NerdDinner contraffatte

Quando si ridigitare il nome utente e password, la pagina di accesso falsificati Salva le informazioni e ci è stata inviata nuovamente al sito di NerdDinner.com legittimo. A questo punto, il sito di NerdDinner.com abbia già autenticato, in modo che la pagina di accesso falsificati può reindirizzare direttamente a tale pagina. Il risultato finale è che l'autore dell'attacco ha il nome utente e password e siamo a conoscenza che è stato fornito ad essi.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Esaminando il codice vulnerabile nell'azione di accesso AccountController

Il codice per l'azione di accesso in un'applicazione ASP.NET MVC 2 è illustrato di seguito. Si noti che al momento di un account di accesso ha esito positivo, il controller restituisce un reindirizzamento a returnUrl. È possibile vedere che non viene eseguita alcuna convalida rispetto al parametro returnUrl.

**Listato 1-azione di accesso di ASP.NET MVC 2 in `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Ora esaminiamo le modifiche per l'azione di accesso di ASP.NET MVC 3. Questo codice è stato modificato per convalidare il parametro returnUrl chiamando un nuovo metodo nella classe helper System.Web.Mvc.Url denominata `IsLocalUrl()`.

**Listato 2-azione di accesso di ASP.NET MVC 3 in `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Questo è stato modificato per convalidare il parametro dell'URL restituito chiamando un nuovo metodo nella classe helper System.Web.Mvc.Url `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Protezione di ASP.NET MVC 1.0 e MVC 2 applicazioni

Abbiamo possiamo sfruttare le modifiche di ASP.NET MVC 3 nel nostro ASP.NET MVC 1.0 esistente e 2 applicazioni aggiungendo il metodo helper IsLocalUrl() e aggiornando l'azione di accesso per convalidare il parametro returnUrl.

Il metodo UrlHelper IsLocalUrl() semplicemente chiamare un metodo in System.Web.WebPages, come la convalida viene usato anche dalle applicazioni ASP.NET Web Pages.

**Listato 3 – il metodo IsLocalUrl() dal UrlHelper 3 di ASP.NET MVC `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Il metodo IsUrlLocalToHost contiene la logica di convalida effettiva, come illustrato nel listato 4.

**Listato 4: metodo IsUrlLocalToHost() dalla classe System.Web.WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

Nel nostro ASP.NET MVC 1.0 o 2, verrà aggiunto un metodo IsLocalUrl() al AccountController, ma si consiglia di aggiungerlo a una classe helper distinta, se possibile. Microsoft farà due piccole modifiche alla versione ASP.NET MVC 3 di IsLocalUrl() in modo che funzioni all'interno di AccountController. In primo luogo, verranno modificate, da un metodo pubblico a un metodo privato, poiché i metodi pubblici nel controller sono accessibili come azioni del controller. In secondo luogo, si modificherà la chiamata che controlla l'host dell'URL in base all'host dell'applicazione. Che chiamata viene utilizzato un elemento RequestContext locale campo nella classe UrlHelper. Invece di questa. RequestContext.HttpContext.Request.Url.Host, si userà questo. Request.Url.Host. Il codice seguente illustra il metodo IsLocalUrl() modificato per l'uso con una classe controller in ASP.NET MVC 1.0 e 2 applicazioni.

**Listato 5 – IsLocalUrl() (metodo), che viene modificato per l'uso con una classe Controller MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Ora che il metodo IsLocalUrl() è in uso, è possibile chiamarlo dall'azione di accesso per convalidare il parametro returnUrl, come illustrato nel codice seguente.

**Listato 6-metodo di accesso aggiornato che convalida il parametro di returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Ora è possibile testare un attacco di reindirizzamento aperti tentando di accedere usando un URL restituito esterno. È possibile usare /Account/LogOn? ReturnUrl =<http://www.bing.com/> nuovamente.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Figura 04**: l'azione di accesso aggiornata di test

Dopo aver completato l'accesso, si verrà reindirizzati l'azione del Controller Home/Index anziché l'URL esterno.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Figura 05**: sconfitto attacchi di reindirizzamento aperti

## <a name="summary"></a>Riepilogo

Attacchi di reindirizzamento aperti possono verificarsi quando il reindirizzamento URL vengono passati come parametri nell'URL per un'applicazione. Il modello include il codice per la protezione da ASP.NET MVC 3 aprire gli attacchi di reindirizzamento. È possibile aggiungere questo codice con alcune modifiche alle 2 applicazioni e MVC ASP.NET 1.0. Per proteggersi da attacchi di reindirizzamento aperti durante la registrazione in ASP.NET 1.0 e 2 applicazioni, aggiungere un metodo IsLocalUrl() e convalidare il parametro returnUrl nell'azione di accesso.
