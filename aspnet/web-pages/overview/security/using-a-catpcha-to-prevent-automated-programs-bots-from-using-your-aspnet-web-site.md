---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Sito utilizzando un CAPTCHA per impedire l'uso di Razor di ASP.NET Web Bot) | Microsoft Docs
author: microsoft
description: Questo articolo illustra come usare ReCaptcha (una misura di sicurezza) per impedire l'esecuzione di attività in un ASP.NET Web Pages (Razor) i programmi automatizzati (Bot) abbiamo...
ms.author: aspnetcontent
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: f67eb60c23e0eec46089ceea9b04779492dfa15e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803071"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Sito utilizzando un CAPTCHA per impedire l'uso di Razor di ASP.NET Web Bot)
====================
da [Microsoft](https://github.com/microsoft)

> Questo articolo illustra come usare ReCaptcha (una misura di sicurezza) per impedire l'esecuzione di attività in un sito Web di ASP.NET Web Pages (Razor) i programmi automatizzati (Bot).
> 
> **Che cosa si apprenderà come:** 
> 
> - Come aggiungere un test CAPTCHA al sito.
> 
> Queste sono le funzionalità ASP.NET introdotte nell'articolo:
> 
> - Il `ReCaptcha` helper.
> 
> > [!NOTE]
> > Le informazioni contenute in questo articolo si applicano a 1.0 di pagine Web ASP.NET e Web Pages 2.


## <a name="about-captchas"></a>Sulle CAPTCHAs

Ogni volta che si consentono di registrare nel sito, o anche semplicemente immettere un nome e un URL (ad esempio per un commento di blog), si potrebbe ricevere un flusso eccessivo di specificando nomi falsi. Spesso, questi vengono lasciati da programmi automatizzati (Bot) che tentano di mantenere gli URL in ogni sito Web che sono disponibili. (Una motivazione comune consiste nel registrare l'URL dei prodotti in vendita).

Si può assicurare che un utente sia persona reale e non un programma con un *CAPTCHA* per convalidare gli utenti quando si registra o in caso contrario, immettere il nome e il sito. CAPTCHA è l'acronimo di test completamente automatizzata pubblico Turing indicare i computer e gli esseri umani distanti. Un CAPTCHA è un *challenge / response* test in cui l'utente viene richiesto di eseguire un'operazione facile per un utente a eseguire operazioni ma rigido per un programma automatico eseguire. Il tipo più comune di CAPTCHA è uno in cui si vedere alcune lettere distorta e viene richiesto di digitarli. (La distorsione dovrebbe per risultare difficile per Bot possono essere quindi decifrate le lettere.)

## <a name="adding-a-recaptcha-test"></a>Aggiunta di un Test di ReCaptcha

Nelle pagine ASP.NET, è possibile usare la `ReCaptcha` helper per eseguire il rendering di un test CAPTCHA che si basa sul servizio ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). Il `ReCaptcha` helper Visualizza un'immagine di due parole distorte che gli utenti devono immettere correttamente prima che la pagina viene convalidata. La risposta dell'utente viene convalidata dal servizio Recaptcha.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registrare il sito Web all'indirizzo ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Dopo aver completato la registrazione, si otterrà una chiave pubblica e una chiave privata.
2. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se già stato fatto.
3. Se si ha già un  *\_AppStart.cshtml* file, nella cartella radice di un sito Web, creare un file denominato  *\_AppStart.cshtml*.
4. Aggiungere il codice seguente `Recaptcha` le impostazioni di helper nel  *\_AppStart.cshtml* file: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Impostare il `PublicKey` e `PrivateKey` proprietà mediante le proprie chiavi pubbliche e private.
6. Salvare il  *\_AppStart.cshtml* file e chiuderlo.
7. Nella cartella radice di un sito Web, creare nuova pagina denominata *Recaptcha.cshtml*.
8. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Eseguire la *Recaptcha.cshtml* pagina in un browser. Se il `PrivateKey` valore sia valido, la pagina Visualizza il controllo ReCaptcha e un pulsante. Se non è stata impostata a livello globale in quelle  *\_AppStart.html*, la pagina verrà visualizzato un errore. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Immettere le parole per il test. Se si passa il test ReCaptcha, viene visualizzato un messaggio in tal senso. In caso contrario, viene visualizzato un messaggio di errore e viene visualizzata di nuovo il controllo di ReCaptcha.

> [!NOTE]
> Se il computer si trova in un dominio che utilizza un server proxy, si potrebbe essere necessario configurare il `defaultproxy` dell'elemento il *Web. config* file. L'esempio seguente mostra una *Web. config* file con il `defaultproxy` elemento configurato per consentire il funzionamento del servizio ReCaptcha.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


- [Personalizzazione del comportamento a livello di sito per siti con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sito ReCaptcha](https://www.google.com/recaptcha)
