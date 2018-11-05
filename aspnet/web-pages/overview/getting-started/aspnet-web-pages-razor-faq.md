---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web Pages (Razor) domande frequenti | Microsoft Docs
author: Rick-Anderson
description: Questo articolo elenca alcune domande frequenti su WebMatrix e ASP.NET Web Pages (Razor). Versioni del software utilizzate nell'esercitazione ASP.NET Web Pages (r....
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 27bfbe782455a5f8cf5096953c91712988b8dcab
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021053"
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages (Razor) domande frequenti
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix non è più consigliata come ambiente di sviluppo integrato per ASP.NET Web Pages. Uso [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) oppure [Visual Studio Code](https://code.visualstudio.com/).
>
> Questo articolo elenca alcune domande frequenti su WebMatrix e ASP.NET Web Pages (Razor).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Questa esercitazione funziona anche con ASP.NET Web Pages 2, 2 WebMatrix e Visual Studio 2012.


- [Che cos'è la differenza tra le pagine Web ASP.NET, Web Form ASP.NET e ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [È necessario WebMatrix per funzionare con le pagine Web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [È possibile usare controlli di Web Form ASP.NET in una pagina di pagine Web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [È possibile distribuire un sito con pagine Web ASP.NET senza l'utilizzo di WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [È necessario usare l'helper WebSecurity per supportare gli account di accesso?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [ASP.NET Web Pages supporta HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [È possibile usare JavaScript e jQuery con pagine Web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Risorse aggiuntive](#AdditionalResources)

Per domande relative a errori e altri problemi, vedere la [Guida alla risoluzione dei problemi di ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Che cos'è la differenza tra le pagine Web ASP.NET, Web Form ASP.NET e ASP.NET MVC?

Tutte e tre sono tecnologie ASP.NET per la creazione di applicazioni web dinamiche:

- ASP.NET Web Pages è incentrato sulla aggiunta dinamica del codice (lato server) e l'accesso al database per le pagine HTML e la sintassi semplice e leggera di funzionalità.
- Web Form ASP.NET si basa su un modello a oggetti pagina e controlli tradizionale-tipo di finestra (pulsanti, elenchi e così via). Web Form viene usato un modello basato su eventi familiare a coloro che hanno lavorato con sviluppo di (Windows Form) basata su client.
- ASP.NET MVC implementa il pattern model-view-controller per ASP.NET. L'attenzione viene focalizzata "separazione delle problematiche" (l'elaborazione dati e livelli dell'interfaccia utente).

Tutti i tre Framework sono completamente supportati e continuano a essere sviluppati dal team ASP.NET. In generale, la scelta di quali framework da usare dipende proprie competenze ed esperienze con ASP.NET.

In particolare, ASP.NET Web Pages è stato progettato per semplificare per gli utenti che già conoscono HTML per aggiungere l'elaborazione del server alle pagine. È una scelta ottimale per gli studenti, dilettanti, gli utenti in genere chi si accosta alla programmazione. Può anche essere la scelta ideale per gli sviluppatori che hanno esperienza con le tecnologie web non ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>È necessario WebMatrix per funzionare con le pagine Web?

No. WebMatrix non è più consigliata come ambiente di sviluppo integrato per ASP.NET Web Pages. Uso [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) oppure [Visual Studio Code](https://code.visualstudio.com/).

Se non si desidera usare Visual Studio o Visual Studio Code, è possibile installare i prodotti di componente utilizzando singolarmente [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Sono necessari i seguenti prodotti:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (che viene installato anche il framework di ASP.NET Web Pages)
- IIS Express (il server web)
- Microsoft SQL Server Compact 4.0 (database)

È possibile usare un editor di testo per modificare *. cshtml* (o *vbhtml*) pagine.

La gestione di database SQL Server Compact (*sdf* file) senza uno strumento è un po' più difficoltosa. Strumenti di Visual Studio containds per gestire *sdf* i database. È anche possibile eseguire i comandi SQL nel codice per eseguire molte attività di gestione di SQL Server.

Per testare *cshtml* pagine senza usare un ambiente di sviluppo integrato (IDE), è possibile distribuirli a un server attivo. (Vedere [è possibile distribuire un sito con pagine Web ASP.NET senza l'utilizzo di WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Esecuzione di IIS Express senza usare un IDE

Se si installa IIS Express nel computer come server web, è possibile utilizzarlo per testare le pagine. È possibile eseguire IIS Express dalla riga di comando e associarlo a un numero di porta specifico. È quindi specificare tale porta quando si richiede *cshtml* file nel browser.

In Windows, aprire un prompt dei comandi con privilegi di amministratore e passare alla *c:\Programmi\Microsoft Files\IIS Express.* (Per sistemi a 64 bit, usare la cartella *C:\Program Files (x86) \IIS Express.)* Immettere quindi il comando seguente, usando il percorso effettivo nel sito:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

È possibile usare qualsiasi numero di porta che non è già riservato da un altro processo. (I numeri di porta superiore a 1024 sono in genere gratuiti). Per il `path` valore, utilizzare il percorso della cartella del sito Web in cui il *cshtml* file sono.

Dopo aver eseguito questo comando per configurare IIS Express per gestire le pagine, è possibile aprire un browser e passare a un *cshtml* file. Usare un URL simile al seguente:

`http://localhost:35896/default.cshtml`

Per informazioni su IIS Express opzioni della riga di comando, immettere `iisexpress.exe /?` nella riga di comando.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>È possibile usare controlli di Web Form ASP.NET in una pagina di pagine Web?

No. Controlli Web Form, ad esempio la [casella di controllo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) (controllo), il [i controlli di convalida](https://msdn.microsoft.com/library/bwd43d0x)e il [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) controllare funzionano solo nelle pagine Web Form (*aspx* file). Questi controlli richiedono il framework della pagina Web Form.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>È possibile distribuire un sito con pagine Web ASP.NET senza l'utilizzo di WebMatrix?

Sì. È possibile copiare manualmente i file di siti Web a un server (in genere usando FTP). Se si esegue una copia manuale, è inoltre necessario copiare i file che supportano SQL Server Compact (database). Per informazioni dettagliate, vedere il post di blog [applicazioni distribuzione Web Pages senza uno strumento](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>È necessario usare l'helper WebSecurity per supportare gli account di accesso?

No. Il `SimpleMembership` provider che fa parte di ASP.NET Web Pages è una delle opzioni. Sono disponibili anche i provider di sicurezza che fanno parte di ASP.NET (che possono essere utilizzati per l'utilizzo di Web Form). Ad esempio, è possibile utilizzare autenticazione basata su form in ASP.NET Web Pages esattamente come farebbe in Web Form. Per un esempio di come usare i moduli di autenticazione, vedere l'articolo di supporto tecnico Microsoft [implementazione di autenticazione in un'applicazione ASP.NET usando C# .NET](https://support.microsoft.com/kb/301240). Per scaricare un esempio semplice, vedere [versione ASP.NET di "account di accesso &amp; Password](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Per informazioni su come usare l'autenticazione di Windows, vedere il post di blog [autenticazione di Windows usando in ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>ASP.NET Web Pages supporta HTML5?

Sì. Le pagine create con ASP.NET Web Pages (*. cshtml* oppure *vbhtml* pagine) sono essenzialmente le pagine HTML contenenti anche codice che viene eseguito nel server, prima che il rendering della pagina. Purché il browser dell'utente supporta HTML5, è possibile usare HTML5 elementi in un *. cshtml* oppure *vbhtml* pagina.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>È possibile usare JavaScript e jQuery con pagine Web?

Certamente. Le pagine create con ASP.NET Web Pages (*. cshtml* oppure *vbhtml* pagine) sono solo pagine HTML con il codice server in essi contenuti. Pertanto, tutte le operazioni eseguibili in una normale pagina HTML dal usando JavaScript o jQuery è inoltre possibile eseguire un *. cshtml* oppure *vbhtml* pagina.

Il **Starter Site** modello in WebMatrix contiene una serie di librerie jQuery. Se si crea un sito con tale modello, il *gli script* cartella contiene una libreria di base di jQuery (*jquery 1.6.2.js)* e le librerie per la convalida di jQuery (*jquery.validate.js*e così via.).

Ecco alcuni post di blog che illustrano alcuni modi per usare jQuery con ASP.NET Web Pages:

- [Aggiunta di ASP.NET Web Pages usando WebMatrix jQuery per la bontà](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) da Rachel Appel
- [5 min: WebMatrix interfaccia utente di jQuery json + jQuery modelli](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) da Jonas Eriksson
- [WebMatrix e form jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) da Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Guida alla risoluzione dei problemi delle pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[ASP.NET Web Pages e WebMatrix forum](https://forums.asp.net/1224.aspx/1?WebMatrix) sul sito Web ASP.NET
