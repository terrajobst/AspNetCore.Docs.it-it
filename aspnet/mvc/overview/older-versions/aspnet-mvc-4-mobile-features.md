---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Funzionalità di ASP.NET MVC 4 mobili | Documenti Microsoft
author: Rick-Anderson
description: È ora disponibile una versione di MVC 5 di questa esercitazione con esempi di codice in una distribuzione di un'applicazione Web ASP.NET MVC 5 Mobile nei siti Web di Azure.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 5f38fcdd8e71ce12f7899214b6b2133e21f9910c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-4-mobile-features"></a>Funzionalità per dispositivi mobili ASP.NET MVC 4
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> È ora disponibile una versione di MVC 5 di questa esercitazione con esempi di codice in [distribuire un'applicazione Web ASP.NET MVC 5 Mobile nei siti Web di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


In questa esercitazione verrà fornite le nozioni di base lavorare con le funzionalità di dispositivi mobili in un'applicazione Web ASP.NET MVC 4. Per questa esercitazione, è possibile utilizzare [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer o VWD&quot;). Se si dispone già di, è possibile utilizzare la versione di Visual Studio professional.

Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (scelta consigliata) o Visual Studio Web Developer Express SP1. Visual Studio 2012 contiene ASP.NET MVC 4. Se si utilizza Visual Web Developer 2010, è necessario installare [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

È necessario anche un emulatore del browser per dispositivi mobili. Verranno illustrate le seguenti:

- [Emulatore Windows Phone Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Questo è l'emulatore utilizzato nella maggior parte delle schermate in questa esercitazione).
- Modificare la stringa agente utente per emulare un iPhone. Vedere [questo](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) post di blog.
- [Opera Mobile emulatore](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) con l'agente utente impostato su iPhone. Per istruzioni su come impostare l'agente utente in Safari su "iPhone", vedere [descritto come consentire Safari fingono è IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) sul blog di David Alison.

Progetti di Visual Studio con codice sorgente c# sono disponibili a complemento di questo argomento:

- [Download di progetto di avvio](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Completato il download di progetto](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Cosa Compilerai

Per questa esercitazione si aggiungerà mobili funzionalità all'applicazione conferenza elenco semplice fornito nel [progetto iniziale](https://go.microsoft.com/fwlink/?LinkId=228307). Nella schermata seguente mostra la pagina di tag dell'applicazione completata, come illustrato nel [emulatore di Windows Phone 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Vedere [Mapping di tastiera per Windows Phone emulatore](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) per semplificare l'input da tastiera.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

È possibile utilizzare Internet Explorer versione 9 o 10, FireFox o Chrome per sviluppare l'applicazione per dispositivi mobili impostando il [stringa agente utente](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). La figura seguente mostra l'esercitazione completata utilizzando Internet Explorer che emula un iPhone. È possibile utilizzare gli strumenti di sviluppo di Internet Explorer F-12 e [strumento Fiddler](http://www.fiddler2.com/fiddler2/) per facilitare il debug dell'applicazione.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Si apprenderà competenze

Ecco cosa si apprenderà:

- Come i modelli di ASP.NET MVC 4 utilizzano il HTML5 `viewport` attributo e il rendering adattivo per migliorare la visualizzazione nei dispositivi mobili.
- Come creare visualizzazioni mobili specifiche.
- Come creare una selezione di visualizzazione che consente agli utenti alterna una vista mobile e una visualizzazione desktop dell'applicazione.

### <a name="getting-started"></a>Introduzione

Scaricare l'applicazione di conferenza elenco per il progetto iniziale utilizzando il collegamento seguente: [scaricare](https://go.microsoft.com/fwlink/?LinkId=228307). In Esplora risorse, quindi fare clic il *MvcMobile.zip* file e scegliere **proprietà**. Nel **MvcMobile.zip proprietà** finestra di dialogo scegliere la **Unblock** pulsante. (Lo sblocco impedisce a un avviso di sicurezza che si verifica quando si tenta di utilizzare un *zip* file scaricato dal web.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Fare doppio clic su di *MvcMobile.zip* file e selezionare **Estrai tutto** per decomprimere il file. In Visual Studio, aprire il *MvcMobile.sln* file.

Premere CTRL + F5 per eseguire l'applicazione, cui viene visualizzata nel browser desktop. Avviare l'emulatore di browser per dispositivi mobili, copiare l'URL per l'applicazione di conferenza nell'emulatore e quindi fare clic su di **Sfoglia per tag** collegamento. Se si utilizza l'emulatore Windows Phone, fare clic nella barra dell'URL e premere il tasto di sospensione per ottenere l'accesso da tastiera. L'immagine seguente viene illustrato il *AllTags* vista (dalla scelta di **Sfoglia per tag**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

La visualizzazione è molto leggibile in un dispositivo mobile. Scegliere il collegamento ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

La visualizzazione di tag ASP.NET è molto poco chiara. Ad esempio, il **data** colonna è molto difficile da leggere. Più avanti nell'esercitazione si creerà una versione di *AllTags* visualizzazione che è specifico per i browser per dispositivi mobili e in questo modo la visualizzazione leggibile.

Nota: Attualmente esiste un bug nel motore di memorizzazione nella cache per dispositivi mobili. Per le applicazioni di produzione, è necessario installare il [DisplayModes fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacchetto nugget. Vedere [ASP.NET MVC 4 Mobile la memorizzazione nella cache Bug predefinito](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) per informazioni dettagliate su per risolvere il problema.

## <a name="css-media-queries"></a>Query di supporto CSS

[Query supporti CSS](http://www.w3.org/TR/css3-mediaqueries/) sono un'estensione di CSS per i tipi di supporti. Consentono di creare regole che ignorano le regole CSS predefinito per il browser specifici (agente utente). Una regola comune per CSS destinato browser per dispositivi mobili consiste nel definire la dimensione massima della schermata. Il *Content\Site.css* file creato quando si crea un nuovo progetto ASP.NET MVC 4 Internet contiene la query di supporto seguenti:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Se la finestra del browser è 850 pixel di larghezza o meno, utilizza le regole CSS all'interno del blocco di supporto. È possibile utilizzare le query di supporto CSS simile al seguente per fornire una visualizzazione ottimale del contenuto HTML nei browser di piccole dimensioni (ad esempio, il browser per dispositivi mobili) rispetto alle regole CSS predefiniti che sono progettati per le visualizzazioni più ampie dei browser desktop.

## <a name="the-viewport-meta-tag"></a>Il Tag Viewport Meta

Browser per più dispositivi mobili definire una larghezza della finestra del browser virtuale (la *viewport*) che è maggiore della larghezza effettiva del dispositivo mobile. In questo modo il browser per dispositivi mobili adattare l'intera pagina web all'interno di visualizzazione virtuale. Gli utenti possono quindi ingrandire interessanti contenuti. Tuttavia, se si imposta la larghezza del viewport per la larghezza di un dispositivo effettivo, lo zoom non è necessario, perché il contenuto si adatta nel browser per dispositivi mobili.

Il viewport `<meta>` tag nel file di layout di ASP.NET MVC 4 imposta il viewport per la larghezza del dispositivo. La riga seguente viene illustrato il viewport `<meta>` tag nel file di layout di ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Esaminare gli effetti delle query di supporto CSS e il Tag Viewport Meta

Aprire il *Views\Shared\\layout. cshtml* file nell'editor e impostare come commento il viewport `<meta>` tag. Il markup seguente viene illustrata la riga di commento.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Aprire il *MvcMobile\Content\Site.css* file nell'editor e modificare la larghezza massima della query supporti per zero pixel. Ciò impedirà le regole CSS venga utilizzato nel browser per dispositivi mobili. La riga seguente viene illustrata la query di supporto modificato:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Salvare le modifiche e passare all'applicazione conferenza in un emulatore del browser per dispositivi mobili. Il testo di piccole dimensioni nella figura seguente è il risultato della rimozione del viewport `<meta>` tag. Con non viewport `<meta>` tag, il browser è lo zoom indietro la larghezza del riquadro di visualizzazione predefinito (850 pixel o per i browser più dispositivi mobili.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Annullare le modifiche, rimuovere il commento del viewport `<meta>` tag nel file di layout e il ripristino della query di supporto a 850 pixel di *Site* file. Salvare le modifiche e aggiornare il browser per dispositivi mobili per verificare che sia stata ripristinata la visualizzazione mobile descrittivi.

Il viewport `<meta>` tag e la query di supporto CSS non sono specifici di ASP.NET MVC 4 ed è possibile sfruttare queste funzionalità in qualsiasi applicazione web. Ma vengono ora compilate nei file che vengono generati quando si crea un nuovo progetto ASP.NET MVC 4.

Per ulteriori informazioni sul viewport `<meta>` tag, vedere [A tale di due finestre, parte 2](http://www.quirksmode.org/mobile/viewports2.html).

Nella sezione successiva verrà visualizzato come fornire visualizzazioni specifiche del browser di dispositivi mobili.

## <a name="overriding-views-layouts-and-partial-views"></a>Si esegue l'override di viste, layout e le visualizzazioni parziali

Una nuova funzionalità significative di ASP.NET MVC 4 è un semplice meccanismo che consente di eseguire l'override di qualsiasi visualizzazione (inclusi i layout e le visualizzazioni parziali) per i browser per dispositivi mobili in generale, per un browser per dispositivi mobili singoli o per qualsiasi browser specifico. Per fornire una visualizzazione specifica di dispositivi mobili, è possibile copiare un file di visualizzazione e aggiungere *. Mobile* al nome del file. Ad esempio, per creare un mobile *indice* visualizzare, copiare *Views\Home\Index.cshtml* a *Views\Home\Index.Mobile.cshtml*.

In questa sezione si creerà un file di layout specifici dispositivi mobili.

Per iniziare, copiare *Views\Shared\\layout. cshtml* a *Views\Shared\\_Layout.Mobile.cshtml*. Aprire  *\_Layout.Mobile.cshtml* e modificare il titolo da **MVC4 conferenza** a **conferenza (Mobile)**.

In ogni `Html.ActionLink` chiamare, rimuovere "Sfoglia per" in ogni collegamento *ActionLink*. Il codice seguente mostra una sezione del corpo completo del file di layout per dispositivi mobili.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Copia il *Views\Home\AllTags.cshtml* file *Views\Home\AllTags.Mobile.cshtml*. Aprire il nuovo file e modificare il `<h2>` elemento da "Tags" a "tag (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Passare alla pagina di tag, utilizzando un browser per desktop e l'utilizzo dell'emulatore di browser per dispositivi mobili. L'emulatore di browser per dispositivi mobili Mostra le due modifiche apportate.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Al contrario, la visualizzazione del desktop non è stato modificato.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Visualizzazioni specifiche del browser

Oltre a visualizzazioni specifiche di desktop e mobile, è possibile creare viste per un determinato browser. Ad esempio, è possibile creare viste in modo specifico per il browser iPhone. In questa sezione si creerà un layout per il browser iPhone e una versione di iPhone del *AllTags* visualizzazione.

Aprire il *Global. asax* file e aggiungere il codice seguente per il `Application_Start` metodo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Questo codice definisce una nuova modalità di visualizzazione denominata "iPhone" che verrà confrontato con ogni richiesta in ingresso. Se la richiesta in ingresso soddisfa la condizione che è stato definito (vale a dire, se l'agente utente contiene la stringa "iPhone"), il cui nome contiene il suffisso "iPhone" viste verranno ricercati ASP.NET MVC.

Nel codice, fare doppio clic su `DefaultDisplayMode`, scegliere **risolvere**, quindi scegliere `using System.Web.WebPages;`. Verrà aggiunto un riferimento per il `System.Web.WebPages` spazio dei nomi, dove il `DisplayModes` e `DefaultDisplayMode` tipi sono definiti.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

In alternativa, è possibile aggiungere manualmente solo la riga seguente di `using` sezione del file.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Il contenuto completo del *Global. asax* file è illustrato di seguito.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Salvare le modifiche. Copia il *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Aprire il nuovo file e quindi modificare il `h1` intestazione da `Conference (Mobile)` a `Conference (iPhone)`.

Copia il *MvcMobile\Views\Home\AllTags.Mobile.cshtml* file *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. Nel nuovo file, modificare il `<h2>` elemento da "tag (M)" per "Tags (iPhone)".

Eseguire l'applicazione. Eseguire un emulatore del browser per dispositivi mobili, assicurarsi che l'agente utente è impostato su "iPhone" e selezionare il *AllTags* visualizzazione. La schermata seguente mostra il *AllTags* vista di cui è stato eseguito il rendering nel [Safari](http://www.apple.com/safari/download/) browser. È possibile scaricare Safari per Windows [qui](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

In questa sezione abbiamo visto come creare visualizzazioni e layout di dispositivi mobili e sulla creazione di layout e le visualizzazioni per i dispositivi specifici, ad esempio iPhone. Nella sezione successiva verrà visualizzato come sfruttare jQuery Mobile per le altre visualizzazioni mobili accattivanti.

## <a name="using-jquery-mobile"></a>Utilizzo di jQuery Mobile

Il [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) libreria fornisce un framework dell'interfaccia utente che funzionano su tutti i principali browser per dispositivi mobili. si applica di jQuery Mobile *progressivo miglioramento* al browser per dispositivi mobili che supportano CSS e JavaScript. Progressivo miglioramento consente a tutti i browser visualizzare il contenuto di base di una pagina web, consentendo più potente browser e dispositivi per avere una visualizzazione più dettagliata. I file CSS e JavaScript jQuery Mobile inclusi numerosi elementi per adattare i browser per dispositivi mobili senza apportare modifiche al markup di stile.

In questa sezione verrà installato il *jQuery.Mobile.MVC* pacchetto NuGet, che installa jQuery Mobile e un widget di selezione di visualizzazione.

Per avviare, eliminare il *Shared\\_Layout.Mobile.cshtml* e *Shared\\_Layout.iPhone.cshtml* file creato in precedenza.

Rinominare *Views\Home\AllTags.Mobile.cshtml* e *Views\Home\AllTags.iPhone.cshtml* file *Views\Home\AllTags.iPhone.cshtml.hide* e  *Views\Home\AllTags.Mobile.cshtml.hide*. Poiché i file non è più necessario un *. cshtml* estensione, non utilizzati dal runtime di ASP.NET MVC per eseguire il rendering di *AllTags* visualizzazione.

Installare il *jQuery.Mobile.MVC* pacchetto NuGet in questo modo:

1. Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. Nel **Console di gestione pacchetti**, immettere `Install-Package jQuery.Mobile.MVC -version 1.0.0`

La figura seguente mostra i file aggiunti e modificati per il progetto MvcMobile dal pacchetto NuGet di jQuery.Mobile.MVC. File che vengono aggiunti [Aggiungi] aggiunti dopo il nome del file. L'immagine non sono visualizzati GIF e PNG file aggiunti per il *Content\images* cartella.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Il pacchetto NuGet jQuery.Mobile.MVC viene installato quanto segue:

- Il *App\_Start\BundleMobileConfig.cs* file, è necessario fare riferimento ai file CSS e JavaScript jQuery aggiunti. È necessario seguire le istruzioni seguenti e fare riferimento a mobile bundle definito in questo file.
- file di jQuery Mobile CSS.
- Oggetto `ViewSwitcher` widget controller (*Controllers\ViewSwitcherController.cs*).
- file di jQuery Mobile JavaScript.
- Un file di layout in stile Mobile jQuery (*Views\Shared\\_Layout.Mobile.cshtml*).
- Una visualizzazione parziale di selezione di visualizzazione *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) che fornisce un collegamento nella parte superiore di ogni pagina per passare dalla visualizzazione desktop in visualizzazione mobile e viceversa.
- Diversi<em>PNG</em> e <em>GIF</em> file di immagine nel <em>Content\images</em> cartella.

Aprire il *Global. asax* file e aggiungere il codice seguente come l'ultima riga del `Application_Start` metodo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Nel codice seguente viene illustrato l'intero *Global. asax* file.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Se si utilizza Internet Explorer 9 e non viene visualizzato il `BundleMobileConfig` riga di sopra di evidenziazione di colore giallo, fare clic su di [pulsante visualizzazione compatibilità](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![immagine del pulsante visualizzazione compatibilità (disattivato)] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Immagine del pulsante visualizzazione compatibilità (disattivato)") in Internet Explorer per modificare l'icona da una struttura ![immagine del pulsante visualizzazione compatibilità (disattivato)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "immagine del pulsante visualizzazione compatibilità (disattivato) ") un colore a tinta unita ![immagine del pulsante visualizzazione compatibilità (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "immagine del pulsante visualizzazione compatibilità (on)"). In alternativa è possibile visualizzare in questa esercitazione in FireFox o Chrome.


Aprire il *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file e aggiungere il markup seguente direttamente dopo il `Html.Partial` chiamare:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

L'intero *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file è illustrato di seguito:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Compilare l'applicazione e nell'emulatore del browser per dispositivi mobili passare il *AllTags* visualizzazione. Verrà visualizzato quanto segue:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> È possibile eseguire il debug di codice specifico per dispositivi mobili da [impostando la stringa agente utente](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) per Internet Explorer o Chrome per iPhone e quindi utilizzando gli strumenti di sviluppo F-12. Se il browser per dispositivi mobili non viene visualizzato il **Home**, **altoparlante**, **Tag**, e **data** collegamenti come pulsanti, i riferimenti a jQuery Mobile gli script e file CSS probabilmente non sono corretti.


Oltre alle modifiche dello stile, viene visualizzato **visualizzazione mobile** e un collegamento che consente di passare dalla visualizzazione mobili a vista desktop. Scegliere il **visualizzazione Desktop** collegamento e la visualizzazione desktop viene visualizzato.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

La visualizzazione desktop non fornisce un modo per passare direttamente alla visualizzazione mobile. Correggerai che ora. Aprire il *Views\Shared\\layout. cshtml* file. Sotto la pagina `body` elemento, aggiungere il codice seguente, che esegue il rendering del widget Selezione tipo di visualizzazione:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Aggiornare il *AllTags* Visualizza nel browser per dispositivi mobili. È ora possibile spostarsi tra le visualizzazioni di desktop e mobile.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Eseguire il debug Nota: È possibile aggiungere il codice seguente alla fine del Views\Shared\\_ViewSwitcher.cshtml per facilitare il debug delle viste quando tramite un browser, la stringa agente utente impostato su un dispositivo mobile.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  e aggiungere la seguente voce per il *Views\Shared\\layout. cshtml* file.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Individuare il *AllTags* pagina in un browser desktop. Il widget Selezione tipo di visualizzazione non viene visualizzato in un browser desktop perché viene aggiunto solo alla pagina di layout per dispositivi mobili. Più avanti nell'esercitazione verrà visualizzato come è possibile aggiungere il widget Selezione tipo di visualizzazione per la visualizzazione desktop.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Migliorare l'elenco degli altoparlanti

Nel browser per dispositivi mobili, selezionare il **altoparlanti** collegamento. Poiché non esiste alcuna visualizzazione mobile (*AllSpeakers.Mobile.cshtml*), visualizzare gli altoparlanti predefinito (*AllSpeakers.cshtml*) viene eseguito il rendering con la visualizzazione layout per dispositivi mobili ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

A livello globale, è possibile disabilitare una visualizzazione (non mobile) predefinito per il rendering all'interno di un layout per dispositivi mobili impostando `RequireConsistentDisplayMode` a `true` nel *viste\\viewstart* file, simile al seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Quando `RequireConsistentDisplayMode` è impostato su `true`, il layout per dispositivi mobili (<em>\_Layout.Mobile.cshtml</em>) viene utilizzato solo per le visualizzazioni mobili. (Vale a dire, il file della visualizzazione è nel formato <em>* * ViewName</em><em>. Mobile.cshtml</em>.) È possibile impostare `RequireConsistentDisplayMode` per `true` se il layout per dispositivi mobili non funziona bene con le visualizzazioni non mobili. Nella schermata seguente viene illustrato come la <em>altoparlanti</em> pagina esegue il rendering quando `RequireConsistentDisplayMode` è impostato su `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

È possibile disabilitare la modalità di visualizzazione coerente in una vista impostando `RequireConsistentDisplayMode` a `false` nel file di visualizzazione. Il markup seguente nel *Views\Home\AllSpeakers.cshtml* file imposta `RequireConsistentDisplayMode` a `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Creazione di una vista Mobile altoparlanti

Come già visto, la *altoparlanti* visualizzazione è leggibile, ma i collegamenti sono piccoli e difficili da toccare su un dispositivo mobile. In questa sezione si creerà un oggetto specifico mobile *altoparlanti* visualizzazione che ha un aspetto analogo al seguente un'applicazione per dispositivi mobili moderni, sono visualizzati di grandi dimensioni, facile tap collegamenti e contiene una casella di ricerca per trovare rapidamente altoparlanti.

Copia *AllSpeakers.cshtml* a *AllSpeakers.Mobile.cshtml*. Aprire il *AllSpeakers.Mobile.cshtml* file e rimuovere il `<h2>` elemento intestazione.

Nel `<ul>` tag, aggiungere il `data-role` attributo e impostarne il valore su `listview`. Analogamente ad altri [ `data-*` attributi](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` rende più semplice gli elementi dell'elenco di grandi dimensioni per la scelta. Questo è il markup completato l'aspetto seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Aggiornare il browser per dispositivi mobili. La vista aggiornata è simile al seguente:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Anche se è stata migliorata la visualizzazione per dispositivi mobili è difficile spostarsi lungo elenco di altoparlanti. Per risolvere il problema, nel `<ul>` tag, aggiungere il `data-filter` attributo e impostarlo su `true`. Il codice seguente viene illustrato il `ul` markup.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

La figura seguente mostra la casella del filtro di ricerca nella parte superiore della pagina risultante dal `data-filter` attributo.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Durante la digitazione ogni lettera nella casella di ricerca, jQuery Mobile Filtra l'elenco visualizzato come illustrato nell'immagine seguente.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Migliorare l'elenco di tag

Ad esempio il valore predefinito *altoparlanti* visualizzazione, il *tag* visualizzazione è leggibile, ma i collegamenti sono di piccole dimensioni e difficili da toccare su un dispositivo mobile. In questa sezione, sarà possibile risolvere il *tag* visualizzare allo stesso modo è stata corretta la *altoparlanti* visualizzazione.

Rimuovere il &quot;nascondere&quot; suffisso per il *Views\Home\AllTags.Mobile.cshtml.hide* è il nome del file *Views\Home\AllTags.Mobile.cshtml*. Aprire il file rinominato e rimuovere il `<h2>` elemento.

Aggiungere il `data-role` e `data-filter` gli attributi di `<ul>` tag, come illustrato di seguito:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

L'immagine seguente mostra la pagina di tag filtrando la lettera `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Migliorare l'elenco di date

È possibile migliorare la *date* visualizzare come è stato migliorato il *altoparlanti* e *tag* viste, in modo che sia più facile da utilizzare in un dispositivo mobile.

Copia il *Views\Home\AllDates.cshtml* file *Views\Home\AllDates.Mobile.cshtml*. Aprire il nuovo file e rimuovere il `<h2>` elemento.

Aggiungere `data-role="listview"` per il `<ul>` tag, simile al seguente:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

L'immagine seguente mostra cosa il **data** pagina simile con la `data-role` attributo sul posto.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) sostituire il contenuto del *Views\Home\AllDates.Mobile.cshtml* file con il codice seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Questo codice consente di raggruppare tutte le sessioni di giorni. Crea un separatore di elenco per ogni giorno nuove, ed elenca tutte le sessioni per ogni giorno in un divisore. Ecco l'aspetto quando si esegue questo codice:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Migliorare la visualizzazione SessionsTable

In questa sezione si creerà una vista mobile specifiche di sessioni. Modifiche apportate verrà portata più ampia di nelle altre visualizzazioni che è stata creata.

Nel browser per dispositivi mobili, toccare il **altoparlante** pulsante, quindi immettere `Sc` nella casella di ricerca.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Toccare il **Scott Hanselman** collegamento.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Come si può notare, la visualizzazione è difficile da leggere in un browser per dispositivi mobili. La colonna delle date è difficile lettura e la colonna tag è la visualizzazione. Per risolvere questo problema, copiare *Views\Home\SessionsTable.cshtml* a *Views\Home\SessionsTable.Mobile.cshtml*, quindi sostituire il contenuto del file con il codice seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Il codice rimuove la chat e le colonne tag e formatta il titolo, altoparlante e data verticalmente in modo che tutte queste informazioni sono leggibile in un browser per dispositivi mobili. Nell'immagine seguente riflette le modifiche al codice.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Migliorare la visualizzazione SessionByCode

Infine, si creerà una vista mobile specifica del *SessionByCode* visualizzazione. Nel browser per dispositivi mobili, toccare il **altoparlante** pulsante, quindi immettere `Sc` nella casella di ricerca.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Toccare il **Scott Hanselman** collegamento. Le sessioni di Scott Hanselman vengono visualizzate.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Scegliere il **una panoramica di Stack Web MS di piace** collegamento.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

La visualizzazione desktop predefinita è corretta, ma è possibile migliorare le.

Copia il *Views\Home\SessionByCode.cshtml* a *Views\Home\SessionByCode.Mobile.cshtml* e sostituire il contenuto del *Views\Home\SessionByCode.Mobile.cshtml*file con il markup seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Il nuovo codice Usa il `data-role` attributo per migliorare il layout della visualizzazione.

Aggiornare il browser per dispositivi mobili. Nell'immagine seguente riflette le modifiche appena apportate al codice:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Riepilogo e revisione

In questa esercitazione ha introdotto le nuove funzionalità per dispositivi mobili di ASP.NET MVC 4 Developer Preview. Le funzionalità per dispositivi mobili includono:

- La possibilità di eseguire l'override di layout, viste e le visualizzazioni parziali, sia a livello globale che per una singola visualizzazione.
- Controllo sul layout e l'imposizione di override parziale usando il `RequireConsistentDisplayMode` proprietà.
- Un widget di selezione di visualizzazione per dispositivi mobili viste che possono essere visualizzate anche nelle viste del desktop.
- Supporto per il supporto di browser specifici, ad esempio il browser iPhone.

## <a name="see-also"></a>Vedere anche

- [jQuery Mobile](http://jquerymobile.com) sito.
- [jQuery Mobile Panoramica](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C raccomandazione Web per dispositivi mobili applicazione le procedure consigliate](http://www.w3.org/TR/mwabp/)
- [W3C Candidate Recommendation per le query supporti](http://www.w3.org/TR/css3-mediaqueries/)
