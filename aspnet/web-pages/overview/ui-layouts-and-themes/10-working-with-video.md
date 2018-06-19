---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Visualizzazione di Video in un Web ASP.NET di pagine del sito (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo capitolo viene illustrato come visualizzare video in un ASP.NET Web Pages con pagina sintassi Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 4e7dc50fb60546d1e1f10a16ed863c0b812ec82b
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
ms.locfileid: "29153680"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Visualizzazione di Video in un sito Web di ASP.NET di pagine (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come usare un lettore video (supporto) in un sito Web ASP.NET Web Pages (Razor) per consentire agli utenti di visualizzare video in cui vengono archiviati nel sito. ASP.NET Web Pages con sintassi Razor consente di riprodurre Flash (*SWF*), Media Player (*WMV*) e Silverlight (*XAP*) video.
> 
> Illustra quanto segue:
> 
> - Come scegliere un lettore video.
> - Come aggiungere video a una pagina web.
> - Come impostare gli attributi di lettore video.
> 
> Si tratta di ASP.NET Razor delle pagine delle funzionalità introdotte in questo articolo:
> 
> - Il `Video` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> In questa esercitazione si integra inoltre con 3 di WebMatrix.


## <a name="introduction"></a>Introduzione

Si potrebbe voler visualizzare un video sul proprio sito. Un modo per eseguire questa operazione consiste nel collegare a un sito che dispone già del video, ad esempio YouTube. Se si desidera incorporare un video da questi siti direttamente in pagine, è possibile ottenere in genere il markup HTML dal sito e quindi copiarlo in una pagina. Ad esempio, nell'esempio seguente viene illustrato come incorporare un YouTube video:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Se si desidera riprodurre un video presente nel proprio sito Web (non in un sito di video di condivisione pubblico), non è possibile collegare direttamente al markup incorporato simile al seguente. Tuttavia, è possibile riprodurre video dal sito utilizzando il `Video` helper, che esegue il rendering di un lettore multimediale direttamente in una pagina.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Scelta di un lettore Video

Esistono molti dei formati per i file video, e ogni formato in genere richiede un altro lettore e un metodo diverso per configurare Windows Media player. Nelle pagine ASP.NET Razor, è possibile riprodurre un video in una pagina web utilizzando il `Video` helper. Il `Video` helper semplifica il processo di incorporamento video in una pagina web, perché genera automaticamente il `object` e `embed` HTML che vengono generalmente utilizzati per aggiungere video alla pagina.

Il `Video` helper supporta i seguenti lettori multimediali:

- Adobe Flash
- Windows Media Player
- Microsoft Silverlight

### <a name="the-flash-player"></a>Il lettore Flash

Il `Flash` player del `Video` helper consentono di riprodurre video Flash (*SWF* file) in una pagina web. Come minimo, è necessario specificare un percorso per il file video. Se si specifica solo il percorso, il lettore utilizza valori predefiniti impostati dalla versione corrente di Flash. Impostazioni predefinite tipici sono:

- La visualizzazione del video usando la larghezza predefinita e l'altezza e senza un colore di sfondo.
- Il video viene riprodotto automaticamente quando il caricamento della pagina.
- Il video esegue il ciclo continua fino a quando non è stato arrestato in modo esplicito.
- Il video viene ridimensionato per visualizzare tutto il video, anziché ritagliare il video per adattarsi a dimensioni specifiche.
- La riproduzione del video in una finestra.

### <a name="the-mediaplayer-player"></a>Il lettore di Media Player

Il `MediaPlayer` player del `Video` supporto consente di riprodurre il video di Windows Media (*WMV* file), Windows Media audio (*WMA* file), MP3 e (*MP3* i file) in una pagina web. È necessario includere percorso del file multimediale da riprodurre; tutti gli altri parametri sono facoltativi. Se si specifica solo un percorso, il lettore utilizza impostazioni predefinite configurate per la versione corrente di Media Player, ad esempio:

- La visualizzazione del video usando predefinito larghezza e altezza.
- Il video viene riprodotto automaticamente quando il caricamento della pagina.
- Il video viene riprodotto una sola volta (non ciclo).
- Windows Media player consente di visualizzare l'elenco completo dei controlli nell'interfaccia utente.
- La riproduzione del video in una finestra.

### <a name="the-silverlight-player"></a>Il lettore Silverlight

Il `Silverlight` player del `Video` supporto consente di riprodurre Windows Media Video (*WMV* file), Windows Media Audio (*WMA* file), MP3 e (*MP3* file). È necessario impostare il parametro path in modo che punti a un pacchetto di applicazione basata su Silverlight (*XAP* file). È inoltre necessario impostare i parametri di larghezza e altezza. Tutti gli altri parametri sono facoltativi. Quando si utilizza il lettore Silverlight per video, se si imposta solo i parametri obbligatori, il lettore Silverlight consente di visualizzare il video senza un colore di sfondo.

> [!NOTE]
> Nel caso in cui non si conosce già Silverlight: il *XAP* file è un file compresso che contiene informazioni sul layout in un *XAML* file, codice gestito nell'assembly e risorse facoltative. È possibile creare un *XAP* file in Visual Studio come un progetto di applicazione Silverlight.


Il `Silverlight` lettore video utilizza le impostazioni fornite per il lettore sia le impostazioni che sono state fornite nel *XAP* file.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Tipi MIME
> 
> Quando un browser scarica un file, il browser assicura che il tipo di file corrisponde il tipo MIME specificato per il documento che viene eseguito il rendering. Il tipo MIME è il tipo di supporti o di tipo di contenuto di un file. Il `Video` helper utilizza i tipi MIME seguenti:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Riproduzione di video Flash (SWF)

In questa procedura viene illustrato come riprodurre un video dimostrativo denominato *sample.swf*. Nella procedura si presuppone che si dispone di una cartella denominata *Media* nel sito e che il *SWF* file si trova in tale cartella.

1. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non si già aggiunti.
2. Nel sito Web, aggiungere una pagina e denominarlo *FlashVideo.cshtml*.
3. Aggiungere il markup seguente: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Eseguire la pagina in un browser. (Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.) Verrà visualizzata la pagina e il video viene riprodotto automaticamente. 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

È possibile impostare il `quality` parametro per un video dimostrativo per `low`, `autolow`, `autohigh`, `medium`, `high`, e `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

È possibile modificare il Flash video viene riprodotto in una dimensione specifica usando il `scale` parametro, è possibile impostare per le operazioni seguenti:

- `showall`. In questo modo l'intero video visibili mantenendo le proporzioni originali. Tuttavia, potrebbero finire con bordi su ciascun lato.
- `noorder`. Si ridimensiona il video, mantenendo le proporzioni originali, ma potrà essere ritagliata.
- `exactfit`. In questo modo l'intero video visibili senza mantenendo le proporzioni originali, ma potrebbe verificarsi una distorsione.

Se non si specifica un `scale` parametro, l'intero video sarà visibile e verranno mantenute le proporzioni originali senza eventuali ritagli. Nell'esempio seguente viene illustrato come utilizzare il `scale` parametro:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Il lettore Flash supporta una modalità video impostazione denominata `windowMode`. È possibile impostarlo su `window`, `opaque`, e `transparent`. Per impostazione predefinita, il `windowMode` è impostato su `window`, che consente di visualizzare il video in una finestra separata della pagina web. Il `opaque` impostazione nasconde tutti gli elementi dietro il video sulla pagina web. Il `transparent` impostazione consente di sfondo della pagina web sono visibili attraverso video, presupponendo che qualsiasi parte del video è trasparente.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Riproduzione di Media Player (*WMV*) video

La procedura seguente viene illustrato come riprodurre un video Media finestra denominato *Sample. wmv* che si trova nel *Media* cartella.

1. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se hai già fatto.
2. Creare una nuova pagina denominata *MediaPlayerVideo.cshtml*.
3. Aggiungere il markup seguente: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Eseguire la pagina in un browser. Nel video viene caricato e viene eseguito automaticamente. 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

È possibile impostare `playCount` in un intero che indica il numero di volte per riprodurre il video automaticamente:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

Il `uiMode` parametro consente di specificare quali controlli visualizzati nell'interfaccia utente. È possibile impostare `uiMode` a `invisible`, `none`, `mini`, o `full`. Se non si specifica un `uiMode` parametro, il video verrà visualizzata la finestra di stato, seek barre, controllare i pulsanti e i controlli volume oltre la finestra video. Questi controlli verranno visualizzati anche se si utilizza il lettore per riprodurre un file audio. Di seguito è riportato un esempio di come utilizzare il `uiMode` parametro:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Per impostazione predefinita, audio è durante la riproduzione del video. È possibile disattivare l'audio impostando il `mute` su true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

È possibile controllare il livello audio del video MediaPlayer impostando il `volume` parametro con un valore compreso tra 0 e 100. Il valore predefinito è 50. Di seguito è riportato un esempio:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Riproduzione di video di Silverlight

Questa procedura viene illustrato come riprodurre video contenuto Silverlight *XAP* pagina in una cartella denominata *Media*.

1. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se hai già fatto.
2. Creare una nuova pagina denominata *SilverlightVideo.cshtml*.
3. Aggiungere il markup seguente: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Eseguire la pagina in un browser. 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Panoramica di Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Flash gli attributi di tag di oggetto e INCORPORAMENTO](http://kb2.adobe.com/cps/127/tn_12701.html)

[I tag PARAM SDK di Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
