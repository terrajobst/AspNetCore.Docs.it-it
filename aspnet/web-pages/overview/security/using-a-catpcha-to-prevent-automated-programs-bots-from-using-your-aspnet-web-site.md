---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Sito con un CAPTCHA per impedire che i componenti utilizzando il Razor Web ASP.NET) | Documenti Microsoft
author: microsoft
description: "In questo articolo viene illustrato come utilizzare ReCaptcha (una misura di sicurezza) per impedire l'esecuzione di attività in un pagine Web ASP.NET (Razor) i programmi automatizzati (Bot) è..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Sito con un CAPTCHA per impedire che i componenti utilizzando il Razor Web ASP.NET)
====================
da [Microsoft](https://github.com/microsoft)

> In questo articolo viene illustrato come utilizzare ReCaptcha (una misura di sicurezza) per impedire l'esecuzione di attività in un sito Web ASP.NET Web Pages (Razor) i programmi automatizzati (componenti).
> 
> **Illustra quanto segue:** 
> 
> - Come aggiungere un test CAPTCHA al sito.
> 
> Queste sono le funzionalità ASP.NET introdotte nell'articolo:
> 
> - Il `ReCaptcha` helper.
> 
> > [!NOTE]
> > Le informazioni contenute in questo articolo si applicano a pagine Web ASP.NET 1.0 e 2 di pagine Web.


## <a name="about-captchas"></a>Su CAPTCHAs

Ogni volta che si vuol far registrare del sito, o semplicemente immettere un nome e l'URL (come per un commento di blog), si potrebbe ricevere un flusso eccessivo di nomi falsi. Spesso, questi vengono lasciati da programmi automatizzati (Bot) che tenta di lasciare gli URL in ogni sito Web che sono disponibili. (Una motivazione comune consiste nel registrare gli URL dei prodotti in vendita).

Consente di assicurarsi che un utente sia persona reale e non un programma con un *CAPTCHA* per convalidare gli utenti quando si registra o in caso contrario, immettere il nome e il sito. CAPTCHA è l'acronimo di test completamente automatizzato pubblica Turing indicare i computer e comprensibile distanti. È un CAPTCHA un *richiesta-risposta* test in cui l'utente viene richiesto di eseguire un'operazione semplice per una persona eseguire ma difficile per un programma automatico eseguire. Il tipo più comune di CAPTCHA è uno in cui vedere alcune lettere distorto e viene richiesto di digitarli. (La distorsione è progettato per renderlo difficile Bot da decifrare le lettere).

## <a name="adding-a-recaptcha-test"></a>Aggiunta di un Test di ReCaptcha

Nelle pagine ASP.NET, è possibile utilizzare il `ReCaptcha` helper per il rendering di un test CAPTCHA che si basa sul servizio ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). Il `ReCaptcha` helper Visualizza un'immagine di due parole distorte che gli utenti debbano immettere correttamente prima che la pagina venga convalidata. La risposta dell'utente viene convalidata dal servizio ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registrare il sito Web all'indirizzo ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Dopo aver completato la registrazione, si otterrà una chiave pubblica e una chiave privata.
2. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se hai già fatto.
3. Se dispone già di un  *\_AppStart.cshtml* file, nella cartella radice di un sito Web, creare un file denominato  *\_AppStart.cshtml*.
4. Aggiungere il seguente `Recaptcha` impostazioni helper di  *\_AppStart.cshtml* file: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Impostare il `PublicKey` e `PrivateKey` proprietà usando la propria chiavi pubbliche e private.
6. Salvare il  *\_AppStart.cshtml* file e chiuderlo.
7. Nella cartella radice di un sito Web, creare nuova pagina denominata *Recaptcha.cshtml*.
8. Sostituire il contenuto esistente con il seguente: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Eseguire il *Recaptcha.cshtml* pagina in un browser. Se il `PrivateKey` valore è valido, la pagina viene visualizzato il controllo ReCaptcha e un pulsante. Se non è stata impostata a livello globale nelle chiavi  *\_AppStart.html*, la pagina verrà visualizzato un errore. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Immettere le parole per il test. Se si supera il test di ReCaptcha, viene visualizzato un messaggio in tal senso. In caso contrario viene visualizzato un messaggio di errore e viene visualizzata di nuovo il controllo ReCaptcha.

> [!NOTE]
> Se il computer in un dominio che utilizza un server proxy, potrebbe essere necessario configurare il `defaultproxy` elemento il *Web. config* file. Nell'esempio seguente un *Web. config* file con il `defaultproxy` elemento configurato per consentire il funzionamento del servizio ReCaptcha.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


- [Personalizzazione del comportamento a livello di sito per i siti di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sito ReCaptcha](https://www.google.com/recaptcha)
