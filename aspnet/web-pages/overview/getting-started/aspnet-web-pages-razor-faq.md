---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web Pages domande frequenti (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo articolo sono elencate alcune domande frequenti sulle pagine Web ASP.NET (Razor) e WebMatrix. Versioni del software utilizzate nell'esercitazione ASP.NET Web Pages r (....
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 60cc4ca364923cb131d5e91cd7b6307b1e68644b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042459"
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages domande frequenti (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix non è più consigliato come un ambiente di sviluppo integrato per ASP.NET Web Pages. Utilizzare [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) o [codice di Visual Studio](https://code.visualstudio.com/).
>
> In questo articolo sono elencate alcune domande frequenti sulle pagine Web ASP.NET (Razor) e WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2, 2 WebMatrix e Visual Studio 2012.


- [Che cos'è la differenza tra le pagine Web ASP.NET, Web Form ASP.NET e MVC ASP.NET?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [WebMatrix è necessario per poter funzionare con le pagine Web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [È possibile utilizzare i controlli Web Form ASP.NET in una pagina di pagine Web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [È possibile distribuire un sito ASP.NET Web Pages senza utilizzare WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [È necessario utilizzare l'helper WebSecurity per supportare gli account di accesso?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [ASP.NET Web Pages supporta HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [È possibile utilizzare JavaScript e jQuery con le pagine Web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Risorse aggiuntive](#AdditionalResources)

Per ulteriori informazioni su errori e altri problemi, vedere il [Guida alla risoluzione dei problemi nelle pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Che cos'è la differenza tra le pagine Web ASP.NET, Web Form ASP.NET e MVC ASP.NET?

Tutte e tre sono tecnologie ASP.NET per la creazione di applicazioni web dinamiche:

- ASP.NET Web Pages è incentrata sull'aggiunta di pagine HTML e la sintassi leggera e semplice funzionalità dinamica del codice (lato server) e l'accesso al database.
- Web Form ASP.NET si basa su un modello a oggetti pagina e controlli di tipo di finestra tradizionale (pulsanti, elenchi e così via). Web Form viene utilizzato un modello basato su eventi è noto a coloro che hanno lavorato con lo sviluppo (Windows Form) basata su client.
- ASP.NET MVC implementa il pattern model-view-controller per ASP.NET. L'attenzione viene focalizzata "separazione delle problematiche" (l'elaborazione dati e livelli dell'interfaccia utente).

Tutti e tre i Framework sono completamente supportati e continuano a essere sviluppato dal team di ASP.NET. In generale, la scelta di quali framework da utilizzare dipende lo sfondo e l'esperienza con ASP.NET.

In particolare, le pagine Web ASP.NET è stato progettato per semplificare per gli utenti che già conoscono HTML per aggiungere l'elaborazione di server alle pagine. È una scelta ottimale per gli sviluppatori amatoriali, studenti utenti in genere che sono esperienza di programmazione. Può essere anche una buona scelta per gli sviluppatori che hanno esperienza con tecnologie web non ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>WebMatrix è necessario per poter funzionare con le pagine Web?

No. WebMatrix non è più consigliato come un ambiente di sviluppo integrato per ASP.NET Web Pages. Utilizzare [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) o [codice di Visual Studio](https://code.visualstudio.com/).

Se non si desidera utilizzare Visual Studio o codice di Visual Studio, è possibile installare i prodotti componente singolarmente utilizzando [installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx). È necessario disporre dei seguenti prodotti:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (che viene installato anche il framework di ASP.NET Web Pages)
- IIS Express (il server web)
- Microsoft SQL Server Compact 4.0 (database)

È possibile utilizzare un editor di testo per modificare *. cshtml* (o *. vbhtml*) pagine.

Gestione dei database di SQL Server Compact (*sdf* file) senza uno strumento non è un po' più difficile. Strumenti di Visual Studio containds per la gestione *sdf* database. È anche possibile eseguire comandi SQL nel codice per eseguire molte attività di gestione di SQL Server.

Per testare *. cshtml* pagine senza utilizzare un ambiente di sviluppo integrato (IDE), è possibile distribuirli a un server attivo. (Vedere [è possibile distribuire un sito ASP.NET Web Pages senza utilizzare WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Esecuzione di IIS Express senza con l'IDE

Se si installa IIS Express nel computer in uso come server web, è possibile utilizzare per testare le pagine che. È possibile eseguire IIS Express dalla riga di comando e associarlo a un numero di porta specifico. Specificare quindi tale porta quando si richiede *. cshtml* file nel browser.

In Windows, aprire un prompt dei comandi con privilegi di amministratore e modificare in *c:\Programmi\Microsoft Files\IIS Express.* (Per sistemi a 64 bit, utilizzare la cartella *C:\Program Files (x86) \IIS Express.)* Quindi immettere il comando seguente, utilizzando il percorso effettivo per il sito:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

È possibile utilizzare qualsiasi numero di porta che non sia già riservata da un altro processo. (I numeri di porta superiori a 1024 sono in genere disponibile). Per il `path` valore, utilizzare il percorso della cartella del sito Web in cui il *. cshtml* sono i file.

Dopo aver eseguito questo comando per configurare IIS Express per servire le pagine, è possibile aprire un browser e passare a un *. cshtml* file. Utilizzare un URL simile al seguente:

`http://localhost:35896/default.cshtml`

Per informazioni su IIS Express opzioni della riga di comando, immettere `iisexpress.exe /?` nella riga di comando.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>È possibile utilizzare i controlli Web Form ASP.NET in una pagina di pagine Web?

No. Controlli Web Form come il [casella di controllo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) (controllo), il [i controlli di convalida](https://msdn.microsoft.com/library/bwd43d0x)e [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) controllano solo il lavoro in pagine Web Form (*aspx* file). Questi controlli richiedono il framework della pagina Web Form.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>È possibile distribuire un sito ASP.NET Web Pages senza utilizzare WebMatrix?

Sì. È possibile copiare manualmente i file del sito Web a un server (in genere tramite FTP). Se si esegue una copia manuale, è anche necessario copiare i file che supportano SQL Server Compact (database). Per informazioni dettagliate, vedere il post di blog [applicazioni distribuzione Web Pages senza un strumento](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>È necessario utilizzare l'helper WebSecurity per supportare gli account di accesso?

No. Il `SimpleMembership` provider che fa parte di ASP.NET Web Pages è una delle opzioni. Sono disponibili anche i provider di sicurezza che fanno parte di ASP.NET (che è possibile utilizzare per l'utilizzo di Web Form). Ad esempio, è possibile utilizzare autenticazione basata su form in ASP.NET Web Pages come avviene in Web Form. Per un esempio di come utilizzare autenticazione basata su form, vedere l'articolo del supporto Microsoft [l'autenticazione di implementazione in un'applicazione ASP.NET utilizzando C# .NET](https://support.microsoft.com/kb/301240). Per scaricare un esempio semplice, vedere [versione ASP.NET di "accesso &amp; Password](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Per informazioni su come usare l'autenticazione di Windows, vedere il post di blog [l'autenticazione di Windows utilizzando ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>ASP.NET Web Pages supporta HTML5?

Sì. Le pagine create con ASP.NET Web Pages (*. cshtml* o *. vbhtml* pagine) sono essenzialmente pagine HTML contenenti codice che viene eseguito nel server, prima del rendering della pagina. Fino a quando il browser dell'utente supporta HTML5, è possibile utilizzare gli elementi HTML5 un *. cshtml* o *. vbhtml* pagina.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>È possibile utilizzare JavaScript e jQuery con le pagine Web?

Certamente. Le pagine create con ASP.NET Web Pages (*. cshtml* o *. vbhtml* pagine) sono solo pagine HTML con il codice server in essi contenuti. Pertanto, le operazioni eseguibili in una pagina HTML normale mediante l'utilizzo di JavaScript o jQuery è inoltre possibile eseguire un *. cshtml* o *. vbhtml* pagina.

Il **Starter Site** modello in WebMatrix contiene un numero di librerie jQuery. Se si crea un sito con tale modello, il *script* cartella contiene una libreria di base di jQuery (*jquery 1.6.2.js)* e librerie per la convalida jQuery (*jquery.validate.js*, ecc.).

Ecco alcuni post di blog che illustrano le modalità di utilizzo con ASP.NET Web Pages di jQuery:

- [Aggiunta di jQuery adeguatezza di ASP.NET Web Pages con WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) da Rachel Appel
- [5 minuti: WebMatrix jQuery UI json + jQuery modelli](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) da Jonas Eriksson
- [WebMatrix e form jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) da Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Guida alla risoluzione dei problemi delle pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[ASP.NET Web Pages e WebMatrix forum](https://forums.asp.net/1224.aspx/1?WebMatrix) nel sito Web ASP.NET
