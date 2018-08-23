---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Funzionalità per dispositivi mobili ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: È ora disponibile una versione di MVC 5 di questa esercitazione con esempi di codice in Deploy un'applicazione Web ASP.NET MVC 5 per dispositivi mobili su siti Web di Azure.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: a3cb2414775b2c29617296dcf0c2dc5729153cdd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834839"
---
<a name="aspnet-mvc-4-mobile-features"></a>Funzionalità per dispositivi mobili ASP.NET MVC 4
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> È ora disponibile una versione di MVC 5 con esempi di codice in questa esercitazione [distribuire un'applicazione Web ASP.NET MVC 5 per dispositivi mobili su siti Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


Questa esercitazione insegnerà le nozioni di base di come usare le funzionalità per dispositivi mobili in un'applicazione Web ASP.NET MVC 4. Per questa esercitazione, è possibile usare [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o Visual Web Developer 2010 Express Service Pack 1 (&quot;VWD o Visual Web Developer&quot;). Se si dispone già di questo, è possibile utilizzare la versione professional di Visual Studio.

Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (scelta consigliata) o Visual Studio Web Developer Express SP1. Visual Studio 2012 contiene ASP.NET MVC 4. Se si usa Visual Web Developer 2010, è necessario installare [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

È necessario anche un emulatore di browser per dispositivi mobili. Verrà usata una delle seguenti:

- [Emulatore di Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Questo è l'emulatore che in questa esercitazione viene usato nella maggior parte delle schermate).
- Modificare la stringa agente utente per emulare un iPhone. Visualizzare [ciò](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) post di blog.
- [Emulatore Mobile di opera](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) con l'agente utente impostato su iPhone. Per istruzioni su come impostare l'agente utente in Safari su "iPhone", vedere [come consentire Safari fingono è IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) sul blog di David Alison.

Progetti di Visual Studio con il codice sorgente c# sono disponibili a complemento di questo argomento:

- [Download progetto iniziale](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Download progetto completato](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Scopo dell'esercitazione

Per questa esercitazione si aggiungeranno funzionalità mobili alla semplice applicazione di elenco conferenze fornita nel [progetto iniziale](https://go.microsoft.com/fwlink/?LinkId=228307). Lo screenshot seguente mostra la pagina di tag dell'applicazione completata come mostrato nel [emulatore di Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Visualizzare [Mapping di tastiera per Windows Phone emulatore](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) per semplificare l'input da tastiera.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

È possibile usare Internet Explorer versione 9 o 10, FireFox o Chrome per sviluppare l'applicazione per dispositivi mobili impostando il [stringa agente utente](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). L'immagine seguente mostra l'esercitazione completa con Internet Explorer che emula un iPhone. È possibile usare gli strumenti di sviluppo F-12 di Internet Explorer e il [strumento Fiddler](http://www.fiddler2.com/fiddler2/) per facilitare il debug dell'applicazione.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Competenze

Ecco cosa si apprenderà:

- Come i modelli ASP.NET MVC 4 usano HTML5 `viewport` attributo e il rendering adattivo per migliorare la visualizzazione su dispositivi mobili.
- Come creare visualizzazioni specifiche per dispositivi mobili.
- Come creare un cambio di visualizzazione che attiva/disattiva gli utenti consente tra una visualizzazione per dispositivi mobili e una visualizzazione desktop dell'applicazione.

### <a name="getting-started"></a>Introduzione

Scaricare l'applicazione di elenco conferenze per il progetto iniziale usando il collegamento seguente: [scaricare](https://go.microsoft.com/fwlink/?LinkId=228307). Quindi, in Windows Explorer, fare doppio clic il *MvcMobile.zip* del file e scegliere **proprietà**. Nel **proprietà MvcMobile.zip** finestra di dialogo scegliere la **Unblock** pulsante. (Lo sblocco impedisce a un avviso di sicurezza che si verifica quando si prova a usare una *zip* file scaricato dal web.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Fare doppio clic il *MvcMobile.zip* del file e selezionare **Estrai tutto** per decomprimere il file. In Visual Studio, aprire il *MvcMobile.sln* file.

Premere CTRL+F5 per eseguire l'applicazione, che verrà visualizzato nel browser desktop. Avviare l'emulatore di browser per dispositivi mobili, copiare l'URL dell'applicazione per conferenze nell'emulatore e quindi scegliere il **Sfoglia per tag** collegamento. Se si usa l'emulatore Windows Phone, fare clic su nella barra dell'URL e premere il tasto di sospensione per ottenere l'accesso della tastiera. L'immagine seguente mostra le *AllTags* view (dalla scelta **Sfoglia per tag**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

La visualizzazione è molto leggibile su un dispositivo mobile. Scegliere il collegamento ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

La visualizzazione di tag ASP.NET è molto poco chiara. Ad esempio, il **data** colonna è molto difficile da leggere. Più avanti nell'esercitazione si creerà una versione del *AllTags* visualizzazione che è specifico per browser per dispositivi mobili e che renderà la visualizzazione leggibile.

Nota: Attualmente esiste un bug nel motore di memorizzazione nella cache per dispositivi mobili. Per le applicazioni di produzione, è necessario installare il [DisplayModes fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacchetto NuGet. Visualizzare [ASP.NET MVC 4 per dispositivi mobili la memorizzazione nella cache Bug fisso](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) per i dettagli della correzione.

## <a name="css-media-queries"></a>Query supporti CSS

[Query supporti CSS](http://www.w3.org/TR/css3-mediaqueries/) sono un'estensione a CSS per i tipi di supporti. Consentono di creare regole che sostituiscono le regole CSS predefinite per browser specifici (agente utente). Una regola per CSS destinata a browser per dispositivi mobili più comune consiste nel definire la dimensione massima della schermata. Il *Content\Site.css* file che viene creato quando si crea un nuovo progetto ASP.NET MVC 4 Internet contiene la query di supporto seguenti:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Se la finestra del browser è 850 pixel wide o meno, userà le regole CSS all'interno del blocco supporti. È possibile usare le query supporti CSS simile al seguente per fornire una migliore visualizzazione del contenuto HTML nei browser piccole (ad esempio, il browser per dispositivi mobili) rispetto alle regole CSS predefiniti che sono progettati per le visualizzazioni più ampie di browser desktop.

## <a name="the-viewport-meta-tag"></a>Il Tag Viewport Meta

Browser per dispositivi mobili più definire una larghezza della finestra del browser virtuale (il *viewport*) che è molto maggiore della larghezza effettiva del dispositivo mobile. In questo modo il browser per dispositivi mobili per adattarla all'intera pagina web all'interno di schermo virtuale. Gli utenti possono quindi ingrandire contenuti interessanti. Tuttavia, se si imposta la larghezza del viewport per la larghezza effettiva del dispositivo, lo zoom non è necessario, perché il contenuto viene adattato nel browser per dispositivi mobili.

Il viewport `<meta>` tag nel file di layout di ASP.NET MVC 4 imposta il viewport per la larghezza del dispositivo. La riga seguente mostra il riquadro di visualizzazione `<meta>` tag nel file di layout di ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Analisi dell'effetto di query supporti CSS e il Tag Viewport Meta

Aprire il *Views\Shared\\layout. cshtml* file nell'editor e rimuovere i commenti per il viewport `<meta>` tag. Il markup seguente illustra la riga di commento.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Aprire il *MvcMobile\Content\Site.css* file nell'editor e modificare la larghezza massima della query supporti per zero pixel. Ciò impedirà le regole CSS venga utilizzato nel browser per dispositivi mobili. La riga seguente mostra la query modificata multimediali:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Salvare le modifiche e passare all'applicazione conferenza in un emulatore di browser per dispositivi mobili. Il testo di piccole dimensioni nell'immagine seguente è il risultato della rimozione del viewport `<meta>` tag. Con alcun viewport `<meta>` tag, il browser è uno zoom indietro per la larghezza del riquadro di visualizzazione predefinita (850 pixel o più ampio per i browser per dispositivi mobili più.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Annullare le modifiche, rimuovere il commento del viewport `<meta>` tag nel file di layout e il ripristino di query supporti a 850 pixel nel *CSS* file. Salvare le modifiche e aggiornare il browser per dispositivi mobili per verificare che sia stata ripristinata la visualizzazione per dispositivi mobili.

Il viewport `<meta>` tag e le query supporti CSS non sono specifici di ASP.NET MVC 4 e si possono sfruttare i vantaggi di queste funzionalità in qualsiasi applicazione web. Ma vengono ora compilate nei file che vengono generati quando si crea un nuovo progetto ASP.NET MVC 4.

Per altre informazioni sul viewport `<meta>` tag, vedere [una precisazione di due finestre, parte 2](http://www.quirksmode.org/mobile/viewports2.html).

Nella sezione successiva verrà illustrato come fornire visualizzazioni specifiche del browser per dispositivi mobili.

## <a name="overriding-views-layouts-and-partial-views"></a>Si esegue l'override di viste, layout e visualizzazioni parziali

Una nuova funzionalità significative di ASP.NET MVC 4 è un semplice meccanismo che consente di eseguire l'override di qualsiasi visualizzazione (inclusi i layout e visualizzazioni parziali) per i browser per dispositivi mobili in generale, per un singolo browser per dispositivi mobili o per qualsiasi browser specifico. Per fornire una visualizzazione specifica per dispositivi mobili, è possibile copiare un file di visualizzazione e aggiungere *. Mobile* al nome del file. Ad esempio, per creare un cellulare *indice* consente di visualizzare, copiare *Views\Home\Index.cshtml* al *Views\Home\Index.Mobile.cshtml*.

In questa sezione si creerà un file di layout specifici del dispositivo mobile.

Per iniziare, copiare *Views\Shared\\layout. cshtml* al *Views\Shared\\_Layout.Mobile.cshtml*. Aprire  *\_cshtml* e modificare il titolo da **MVC4 Conference** alla **Conference (Mobile)**.

In ognuno `Html.ActionLink` chiamare, rimuovere "Browse by" di ogni collegamento *ActionLink*. Il codice seguente mostra la sezione del corpo completo del file di layout per dispositivi mobili.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Copia il *Views\Home\AllTags.cshtml* del file ai *Views\Home\AllTags.Mobile.cshtml*. Aprire il nuovo file e cambiare il `<h2>` elemento da "Tags" per "Tags (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Passare alla pagina tags utilizzando un browser desktop e l'uso dell'emulatore di browser per dispositivi mobili. L'emulatore di browser per dispositivi mobili Mostra le due modifiche apportate.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Al contrario, la visualizzazione del desktop non è stato modificato.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Visualizzazioni specifiche del browser

Oltre alle visualizzazioni specifici del dispositivo mobile e specifiche del desktop, è possibile creare visualizzazioni per un particolare browser. Ad esempio, è possibile creare visualizzazioni specifiche per il browser di iPhone. In questa sezione si creerà un layout per il browser di iPhone e una versione iPhone della *AllTags* visualizzazione.

Aprire il *Global. asax* file e aggiungere il codice seguente per il `Application_Start` (metodo).

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Questo codice definisce una nuova modalità di visualizzazione denominata "iPhone" che verrà confrontato con ogni richiesta in ingresso. Se la richiesta in ingresso soddisfa la condizione definita (vale a dire, se l'agente utente contiene la stringa "iPhone"), ASP.NET MVC cercherà le visualizzazioni il cui nome contiene il suffisso "iPhone".

Nel codice, fare doppio clic su `DefaultDisplayMode`, scegliere **risolvere**, quindi scegliere `using System.Web.WebPages;`. Verrà aggiunto un riferimento per la `System.Web.WebPages` spazio dei nomi, in cui è il `DisplayModes` e `DefaultDisplayMode` tipi sono definiti.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

In alternativa, è possibile aggiungere manualmente la riga seguente al `using` sezione del file.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Il contenuto completo della *Global. asax* file è illustrato di seguito.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Salvare le modifiche. Copia il *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Aprire il nuovo file e quindi modificare la `h1` titolo da `Conference (Mobile)` a `Conference (iPhone)`.

Copia il *MvcMobile\Views\Home\AllTags.Mobile.cshtml* del file ai *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. Nel nuovo file, modificare il `<h2>` elemento da "Tags (M)" in "Tags (iPhone)".

Eseguire l'applicazione. Eseguire un emulatore di browser per dispositivi mobili, assicurarsi che il relativo agente utente è impostato su "iPhone" e selezionare il *AllTags* visualizzazione. La schermata seguente mostra le *AllTags* sottoposto a rendering nella visualizzazione di [Safari](http://www.apple.com/safari/download/) browser. È possibile scaricare Safari per Windows [qui](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

In questa sezione abbiamo visto come creare visualizzazioni e layout per dispositivi mobili e come creare layout e visualizzazioni per dispositivi specifici, ad esempio iPhone. Nella sezione successiva verrà illustrato come sfruttare jQuery Mobile per più accattivanti visualizzazioni per dispositivi mobili.

## <a name="using-jquery-mobile"></a>Uso di jQuery Mobile

Il [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) libreria fornisce un framework dell'interfaccia utente che funziona su tutti i principali browser per dispositivi mobili. jQuery Mobile applicabile *miglioramento progressivo* a browser per dispositivi mobili che supportano JavaScript e CSS. Miglioramento progressivo consente a tutti i browser visualizzare il contenuto di base di una pagina web, consentendo più potente browser e dispositivi per avere una visualizzazione più completa. I file CSS e JavaScript inclusi con jQuery Mobile applicare uno stile a molti elementi base browser per dispositivi mobili senza apportare alcuna modifica di markup.

In questa sezione verrà installato il *jQuery.Mobile.MVC* pacchetto NuGet, che installa jQuery Mobile e un widget di cambio di visualizzazione.

Per avviare, eliminare il *Shared\\_Layout.Mobile.cshtml* e *condiviso\\_Layout.iPhone.cshtml* file creato in precedenza.

Rinominare *Views\Home\AllTags.Mobile.cshtml* e *Views\Home\AllTags.iPhone.cshtml* file *Views\Home\AllTags.iPhone.cshtml.hide* e  *Views\Home\AllTags.Mobile.cshtml.hide*. Poiché i file non sono presenti un *. cshtml* estensione, non verranno usati dal runtime di ASP.NET MVC per eseguire il rendering le *AllTags* visualizzazione.

Installare il *jQuery.Mobile.MVC* pacchetto NuGet in questo modo:

1. Dal **degli strumenti** dal menu **Library Package Manager**e quindi selezionare **Package Manager Console**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. Nel **Console di gestione pacchetti**, immettere `Install-Package jQuery.Mobile.MVC -version 1.0.0`

L'immagine seguente mostra i file aggiunti e modificati per il progetto MvcMobile dal pacchetto NuGet di jQuery.Mobile.MVC. File che vengono aggiunti [aggiungere] aggiunti dopo il nome del file. L'immagine non viene visualizzato il GIF e PNG file aggiunti per il *Content\images* cartella.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Il pacchetto NuGet jQuery.Mobile.MVC viene installato quanto segue:

- Il *App\_Start\BundleMobileConfig.cs* file, che è necessario per fare riferimento ai file CSS e JavaScript di jQuery aggiunti. È necessario seguire le istruzioni seguenti e fare riferimento l'aggregazione per dispositivi mobili definito in questo file.
- file CSS per dispositivi mobili di jQuery.
- Oggetto `ViewSwitcher` widget controller (*Controllers\ViewSwitcherController.cs*).
- jQuery Mobile JavaScript file.
- Un file di layout nello stile Mobile jQuery (*Views\Shared\\_Layout.Mobile.cshtml*).
- Una visualizzazione parziale di cambio di visualizzazione *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) che fornisce un collegamento nella parte superiore di ogni pagina per passare dalla visualizzazione desktop in visualizzazione per dispositivi mobili e viceversa.
- Diversi<em>PNG</em> e <em>GIF</em> nel file di immagine i <em>Content\images</em> cartella.

Aprire il *Global. asax* file e aggiungere il codice seguente come l'ultima riga del `Application_Start` (metodo).

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Il codice seguente illustra l'intero *Global. asax* file.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Se si usa Internet Explorer 9 e non viene visualizzato il `BundleMobileConfig` riga sopra nell'evidenziazione di colore giallo, fare clic sui [pulsante visualizzazione compatibilità](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![immagine del pulsante visualizzazione compatibilità (disattivato)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Immagine del pulsante visualizzazione compatibilità (disattivato)") in Internet Explorer per verificare l'icona di modifica da un contorno ![immagine del pulsante visualizzazione compatibilità (disattivato)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "immagine del pulsante visualizzazione compatibilità (disattivato) ") a un colore a tinta unita ![immagine del pulsante visualizzazione compatibilità (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "immagine del pulsante visualizzazione compatibilità (on)"). In alternativa è possibile visualizzare in questa esercitazione in FireFox o Chrome.


Aprire il *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file e aggiungere il markup seguente direttamente dopo il `Html.Partial` chiamare:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

L'intero *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file è illustrato di seguito:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Compilare l'applicazione e nell'emulatore di browser per dispositivi mobili selezionare il *AllTags* visualizzazione. Verrà visualizzato quanto segue:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> È possibile eseguire il debug di codice specifico per dispositivi mobili dal [impostando la stringa agente utente](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) per Internet Explorer o Chrome per iPhone e quindi usare gli strumenti di sviluppo F-12. Se il browser per dispositivi mobili non viene visualizzato il **casa**, **relatore**, **Tag**, e **data** collegamenti come pulsanti, i riferimenti a jQuery Mobile gli script e file CSS probabilmente non sono corretti.


Oltre alle modifiche di stile, vedrai **visualizzazione versione mobile** e un collegamento che consente di passare dalla visualizzazione per dispositivi mobili per la visualizzazione desktop. Scegliere il **visualizzazione Desktop** collegamento e visualizzazione desktop viene visualizzato.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

La visualizzazione desktop non fornisce un modo per passare direttamente alla visualizzazione per dispositivi mobili. Correggerai che ora. Aprire il *Views\Shared\\layout. cshtml* file. Immediatamente sotto la pagina `body` elemento, aggiungere il codice seguente, che esegue il rendering del widget in cambio di visualizzazione:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Aggiorna il *AllTags* Visualizza nel browser per dispositivi mobili. È ora possibile spostarsi tra le visualizzazioni per dispositivi mobili e desktop.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Eseguire il debug Nota: È possibile aggiungere il codice seguente alla fine della finestra di Views\Shared\\_ViewSwitcher.cshtml per facilitare il debug delle viste quando tramite un browser, la stringa agente utente impostato su un dispositivo mobile.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  e aggiungere l'intestazione seguente per il *Views\Shared\\layout. cshtml* file.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Selezionare il *AllTags* pagina in un browser desktop. Il widget di cambio di visualizzazione non viene visualizzato in un browser desktop, perché viene aggiunta solo alla pagina di layout per dispositivi mobili. Più avanti nell'esercitazione scoprirai come è possibile aggiungere il widget di cambio di visualizzazione per la visualizzazione desktop.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Migliorare l'elenco Speakers

Nella finestra del browser per dispositivi mobili, selezionare la **relatori** collegamento. Poiché l'assenza di visualizzazioni per dispositivi mobili (*AllSpeakers.Mobile.cshtml*), visualizzare relatori predefinita (*allspeakers. cshtml*) viene eseguito il rendering tramite la visualizzazione di layout per dispositivi mobili ( *\_ Cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

A livello globale, è possibile disabilitare una visualizzazione (non mobile) predefinita dal rendering all'interno di un layout mobile impostando `RequireConsistentDisplayMode` al `true` nel *viste\\viewstart. cshtml* file, simile al seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Quando `RequireConsistentDisplayMode` è impostata su `true`, il layout per dispositivi mobili (<em>\_cshtml</em>) viene usato solo per le visualizzazioni per dispositivi mobili. (Vale a dire, il file di visualizzazione è nel formato <em>* * ViewName</em><em>. Cshtml</em>.) È consigliabile impostare `RequireConsistentDisplayMode` a `true` se il layout per dispositivi mobili non funziona bene con le visualizzazioni non mobili. Lo screenshot seguente mostra come la <em>Speakers</em> la pagina viene visualizzata quando `RequireConsistentDisplayMode` è impostata su `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

È possibile disabilitare la modalità di visualizzazione coerente in una vista, impostando `RequireConsistentDisplayMode` a `false` nel file della vista. Il markup seguente nel *Views\Home\AllSpeakers.cshtml* file imposta `RequireConsistentDisplayMode` a `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Creazione di una vista Mobile relatori

Come già visto, la *relatori* visualizzazione è leggibile, ma i collegamenti sono di dimensioni ridotte e difficili da toccare su un dispositivo mobile. In questa sezione si creerà un specifici del dispositivo mobile *relatori* vista simile alla seguente un'applicazione per dispositivi mobili moderni, sono visualizzati di grandi dimensioni, facili da toccare i collegamenti e contiene una casella di ricerca per trovare rapidamente i relatori.

Copia *allspeakers. cshtml* al *AllSpeakers.Mobile.cshtml*. Aprire il *AllSpeakers.Mobile.cshtml* del file e rimuovere il `<h2>` elemento titolo.

Nel `<ul>` tag, aggiungere i `data-role` dell'attributo e impostarne il valore su `listview`. Come altri [ `data-*` attributi](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` rende più semplice gli elementi dell'elenco di grandi dimensioni a toccare. Si tratta di come appare il markup completato:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Aggiornare il browser per dispositivi mobili. La vista aggiornata è simile alla seguente:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Anche se è stata migliorata la visualizzazione per dispositivi mobili, è difficile scorrere il lungo elenco di relatori. Per risolvere il problema, nelle `<ul>` tag, aggiungere i `data-filter` dell'attributo e impostarlo su `true`. Il codice seguente mostra il `ul` markup.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

L'immagine seguente mostra la casella di filtro di ricerca nella parte superiore della pagina risultante dal `data-filter` attributo.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Durante la digitazione ogni lettera nella casella di ricerca, jQuery Mobile Filtra l'elenco visualizzato come illustrato nell'immagine seguente.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Migliorare l'elenco Tags

Ad esempio il valore predefinito *relatori* visualizzazione, il *tag* visualizzazione è leggibile, ma i collegamenti sono di dimensioni ridotte e difficili da toccare su un dispositivo mobile. In questa sezione si modificherà il *tag* visualizzare allo stesso modo è stata corretta la *relatori* visualizzazione.

Rimuovere il &quot;nascondere&quot; suffisso per il *Views\Home\AllTags.Mobile.cshtml.hide* file in modo che il nome è *Views\Home\AllTags.Mobile.cshtml*. Aprire il file rinominato e rimuovere il `<h2>` elemento.

Aggiungere il `data-role` e `data-filter` attributi per il `<ul>` tag, come illustrato di seguito:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

L'immagine seguente mostra la pagina di tag filtro in base alla lettera `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Migliorare l'elenco Dates

È possibile migliorare il *date* visualizzare state migliorate le *relatori* e *tag* visualizzazioni, in modo che risulti più facile da usare in un dispositivo mobile.

Copia il *Views\Home\AllDates.cshtml* del file ai *Views\Home\AllDates.Mobile.cshtml*. Aprire il nuovo file e rimuovere il `<h2>` elemento.

Aggiungere `data-role="listview"` per il `<ul>` tag, simile al seguente:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

L'immagine seguente mostra cosa il **data** pagina simile con la `data-role` attributo posto.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) sostituire il contenuto del *Views\Home\AllDates.Mobile.cshtml* file con il codice seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Questo codice raggruppa tutte le sessioni per giorni. Crea un divisore di elenco per ogni nuovo giorno, ed elenca tutte le sessioni per ogni giorno in un divisore. Ecco l'aspetto quando si esegue questo codice:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Migliorare la vista SessionsTable

In questa sezione si creerà una vista specifica per dispositivi mobili di sessioni. Modifiche apportate saranno più ampi nelle altre visualizzazioni che sono stati creati.

Nella finestra del browser per dispositivi mobili, toccare il **Speaker** e quindi immettere `Sc` nella casella di ricerca.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Toccare il **Scott Hanselman** collegamento.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Come può notare, la visualizzazione è difficile da leggere in un browser per dispositivi mobili. La colonna delle date di difficile lettura e la colonna di tag è all'esterno della visualizzazione. Per risolvere questo problema, copiare *Views\Home\SessionsTable.cshtml* al *Views\Home\SessionsTable.Mobile.cshtml*e quindi sostituire il contenuto del file con il codice seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Il codice rimuove la stanza del tag di colonne e formatta il titolo, relatore e date in verticale, in modo che tutte queste informazioni sono leggibile in un browser per dispositivi mobili. Immagine riportata di seguito riflette le modifiche al codice.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Migliorare la vista SessionByCode

Infine, si creerà una vista specifica per dispositivi mobili del *SessionByCode* visualizzazione. Nella finestra del browser per dispositivi mobili, toccare il **Speaker** e quindi immettere `Sc` nella casella di ricerca.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Toccare il **Scott Hanselman** collegamento. Vengono visualizzate le sessioni di Scott Hanselman.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Scegliere il **An Overview of Stack Web MS di Love** collegamento.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

La visualizzazione desktop va bene, ma è possibile migliorarla.

Copia il *Views\Home\SessionByCode.cshtml* al *Views\Home\SessionByCode.Mobile.cshtml* e sostituire il contenuto del *Views\Home\SessionByCode.Mobile.cshtml*file con il markup seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Il nuovo markup Usa il `data-role` attributi per migliorare il layout della visualizzazione.

Aggiornare il browser per dispositivi mobili. L'immagine seguente riflette le modifiche al codice apportate:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Riepilogo e revisione

Questa esercitazione ha introdotto le nuove funzionalità per dispositivi mobili di ASP.NET MVC 4 Developer Preview. Le funzionalità per dispositivi mobili includono:

- La possibilità di eseguire l'override di layout, visualizzazioni e visualizzazioni parziali, sia globalmente che per una singola visualizzazione.
- Controllo sul layout e l'imposizione di override parziale usando il `RequireConsistentDisplayMode` proprietà.
- Un widget di cambio di visualizzazione per dispositivi mobili le viste che possono essere visualizzate anche nelle viste del desktop.
- Supporto per il supporto di browser specifici, ad esempio il browser di iPhone.

## <a name="see-also"></a>Vedere anche

- [jQuery Mobile](http://jquerymobile.com) sito.
- [jQuery Mobile Panoramica](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Procedure guidate applicazioni Web per dispositivi mobili raccomandazione W3C](http://www.w3.org/TR/mwabp/)
- [Candidate Recommendation W3C per query sui supporti](http://www.w3.org/TR/css3-mediaqueries/)
