---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Visualizzazione Video in un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: In questo capitolo viene illustrato come visualizzare i video in un ASP.NET Web Pages con pagina di sintassi Razor.
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 43a806bb5419d32eb8b03e3fc6de05af5d9e2427
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826101"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Visualizzazione Video in un sito di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come usare un lettore video (supporti) in un sito Web ASP.NET Web Pages (Razor) per consentire agli utenti di visualizzare video in cui vengono archiviati nel sito. ASP.NET Web Pages con sintassi Razor consente di riprodurre Flash (*SWF*), Media Player (*WMV*) e Silverlight (*XAP*) video.
> 
> Che cosa si apprenderà come:
> 
> - Come scegliere un lettore video.
> - Come aggiungere video a una pagina web.
> - Come impostare gli attributi di lettore video.
> 
> Si tratta di ASP.NET Razor pages funzionalità introdotte nell'articolo:
> 
> - Il `Video` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Questa esercitazione si integra inoltre con WebMatrix 3.


## <a name="introduction"></a>Introduzione

Si potrebbe voler visualizzare un video sul proprio sito. Un modo per farlo consiste nel collegare a un sito che dispone già di tutto il video, ad esempio YouTube. Se si vuole incorporare un video da tali siti direttamente nelle proprie pagine, è possibile ottenere in genere il markup HTML dal sito e quindi copiarlo nella pagina. Ad esempio, nell'esempio seguente viene illustrato come incorporare un YouTube video:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Se si vuole riprodurre un video, nel tuo sito Web (non in un sito di condivisione video pubblico), è possibile collegare direttamente tramite markup embedded simile al seguente. Tuttavia, è possibile riprodurre i video dal sito usando il `Video` helper, che esegue il rendering di un lettore multimediale direttamente in una pagina.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Scelta di un lettore Video

Esistono molti dei formati per i file video e ogni formato in genere richiede un altro lettore e un metodo diverso per configurare il giocatore. Nelle pagine Razor di ASP.NET, è possibile riprodurre un video in una pagina web mediante il `Video` helper. Il `Video` helper semplifica il processo di incorporare i video in una pagina web in quanto genera automaticamente il `object` e `embed` elementi HTML che vengono generalmente utilizzati per aggiungere video alla pagina.

Il `Video` helper supporta i lettori multimediali seguenti:

- Adobe Flash
- Windows Media Player
- Microsoft Silverlight

### <a name="the-flash-player"></a>Il lettore Flash

Il `Flash` player del `Video` helper consentono di riprodurre video Flash (*con estensione SWF* file) in una pagina web. Come minimo, è necessario specificare un percorso per il file video. Se si specifica solo il percorso, il lettore la Usa valori predefiniti impostati dalla versione corrente di Flash. Le impostazioni predefinite tipiche sono:

- La visualizzazione del video con la larghezza predefinita e l'altezza e senza un colore di sfondo.
- Il video viene riprodotto automaticamente quando il caricamento della pagina.
- Il video esegue il ciclo continua fino a quando non viene arrestato in modo esplicito.
- Il video viene ridimensionato per visualizzare tutti i video, anziché ritagliare il video per adattarsi a dimensioni specifiche.
- Il video viene riprodotto in una finestra.

### <a name="the-mediaplayer-player"></a>Il lettore di MediaPlayer

Il `MediaPlayer` assegnatario del `Video` helper consente di riprodurre video Windows Media (*WMV* file), Windows Media audio (*WMA* file) e MP3 (*MP3* i file) in una pagina web. È necessario includere percorso del file multimediale da riprodurre; tutti gli altri parametri sono facoltativi. Se si specifica solo un percorso, il lettore la Usa impostazioni predefinite configurate per la versione corrente di Media Player, ad esempio:

- La visualizzazione del video con la larghezza predefinita e l'altezza.
- Il video viene riprodotto automaticamente quando il caricamento della pagina.
- Il video viene riprodotto una sola volta (non ciclo).
- Windows Media player consente di visualizzare il set completo di controlli nell'interfaccia utente.
- Il video viene riprodotto in una finestra.

### <a name="the-silverlight-player"></a>Il lettore Silverlight

Il `Silverlight` assegnatario del `Video` helper consente di riprodurre Windows Media Video (*WMV* file), Windows Media Audio (*WMA* file) e MP3 (*MP3* file). È necessario impostare il parametro path in modo che punti a un pacchetto di applicazione basata su Silverlight (*XAP* file). È anche necessario impostare i parametri di larghezza e altezza. Tutti gli altri parametri sono facoltativi. Quando si usa il lettore Silverlight per video, se si imposta solo i parametri obbligatori, il lettore Silverlight consente di visualizzare il video senza un colore di sfondo.

> [!NOTE]
> Nel caso in cui non si conosce già Silverlight: il *XAP* file è un file compresso che contiene informazioni sul layout in un *XAML* file, codice gestito nell'assembly e risorse facoltative. È possibile creare un *XAP* file in Visual Studio come un progetto di applicazione Silverlight.


Il `Silverlight` lettore video utilizza sia le impostazioni fornite per il lettore e le impostazioni disponibili nel *XAP* file.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Tipi MIME
> 
> Quando un browser scarica un file, il browser garantisce che il tipo di file corrisponde al tipo MIME specificato per il documento che viene eseguito il rendering. Il tipo MIME è il tipo di supporti o tipo di contenuto di un file. Il `Video` helper Usa i tipi MIME seguenti:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Riproduzione di video Flash (con estensione SWF)

Questa procedura viene illustrato come riprodurre un video Flash denominato *sample.swf*. Nella procedura si presuppone che hai una cartella denominata *supporti* nel sito e il *con estensione SWF* file si trova nella cartella.

1. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non si già aggiunti.
2. Nel sito Web, aggiungere una pagina e denominarla *FlashVideo.cshtml*.
3. Aggiungere il markup seguente alla pagina: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Eseguire la pagina in un browser. (Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.) Viene visualizzata la pagina e il video viene riprodotto automaticamente. 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

È possibile impostare il `quality` parametro per un video Flash `low`, `autolow`, `autohigh`, `medium`, `high`, e `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

È possibile modificare il video Flash per riprodurre in una dimensione specifica usando il `scale` parametro, che è possibile impostare il testo seguente:

- `showall`. In questo modo l'intero video visibili, mantenendo le proporzioni originali. Tuttavia, si potrebbe ottenere dei bordi su ogni lato.
- `noorder`. Questa configurazione è possibile il video, mantenendo le proporzioni originali, ma potrà essere ritagliata.
- `exactfit`. In questo modo l'intero video visibili senza mantenendo le proporzioni originali, ma possono verificarsi distorsioni.

Se non si specifica un `scale` parametro, l'intero video sarà visibile e verranno mantenute le proporzioni originali senza eventuali ritagli. Nell'esempio seguente viene illustrato come utilizzare il `scale` parametro:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Il lettore Flash supporta una modalità video impostazione denominata `windowMode`. È possibile impostarlo su `window`, `opaque`, e `transparent`. Per impostazione predefinita, il `windowMode` è impostata su `window`, che consente di visualizzare il video in una finestra separata nella pagina web. Il `opaque` impostazione nasconde tutto dietro il video nella pagina web. Il `transparent` impostazione consente lo sfondo della pagina web sono visibili attraverso video, presupponendo che qualsiasi parte del video è trasparente.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Riproduzione di MediaPlayer (*WMV*) video

La procedura seguente viene illustrato come riprodurre un video di finestra supporti denominato *Sample. wmv* che si trova nel *supporti* cartella.

1. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se già stato fatto.
2. Creare una nuova pagina denominata *MediaPlayerVideo.cshtml*.
3. Aggiungere il markup seguente alla pagina: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Eseguire la pagina in un browser. Il video viene caricato e viene eseguito automaticamente. 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

È possibile impostare `playCount` a un intero che indica il numero di volte per riprodurre il video automaticamente:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

Il `uiMode` parametro consente di specificare quali controlli visualizzati nell'interfaccia utente. È possibile impostare `uiMode` al `invisible`, `none`, `mini`, o `full`. Se non si specifica un `uiMode` parametro, il video verrà visualizzata la finestra di stato, seek a barre, controllare i pulsanti e i controlli volume oltre alla finestra del video. Questi controlli verranno visualizzati anche se si usa Windows Media player per riprodurre un file audio. Ecco un esempio di come usare il `uiMode` parametro:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Per impostazione predefinita, audio è su quando la riproduzione del video. È possibile disattivare l'audio impostando il `mute` parametro su true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

È possibile controllare il livello audio del video MediaPlayer impostando il `volume` parametro su un valore compreso tra 0 e 100. Il valore predefinito è 50. Di seguito è riportato un esempio:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Riproduzione di video di Silverlight

Questa procedura viene illustrato come riprodurre video contenuto in un Silverlight *XAP* pagina in una cartella denominata *supporti*.

1. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se già stato fatto.
2. Creare una nuova pagina denominata *SilverlightVideo.cshtml*.
3. Aggiungere il markup seguente alla pagina: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Eseguire la pagina in un browser. 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Panoramica di Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Flash oggetto e INCORPORAMENTO degli attributi del tag](http://kb2.adobe.com/cps/127/tn_12701.html)

[I tag PARAM SDK di Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
