---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Visualizzazione Video in un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: In questo capitolo viene illustrato come visualizzare i video in un ASP.NET Web Pages con pagina di sintassi Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: d80fd8ab54bf1c049fa4ee6b7592925ebe0549f8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369387"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="17eb1-103">Visualizzazione Video in un sito di ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="17eb1-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="17eb1-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="17eb1-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="17eb1-105">Questo articolo illustra come usare un lettore video (supporti) in un sito Web ASP.NET Web Pages (Razor) per consentire agli utenti di visualizzare video in cui vengono archiviati nel sito.</span><span class="sxs-lookup"><span data-stu-id="17eb1-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="17eb1-106">ASP.NET Web Pages con sintassi Razor consente di riprodurre Flash (*SWF*), Media Player (*WMV*) e Silverlight (*XAP*) video.</span><span class="sxs-lookup"><span data-stu-id="17eb1-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="17eb1-107">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="17eb1-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="17eb1-108">Come scegliere un lettore video.</span><span class="sxs-lookup"><span data-stu-id="17eb1-108">How to choose a video player.</span></span>
> - <span data-ttu-id="17eb1-109">Come aggiungere video a una pagina web.</span><span class="sxs-lookup"><span data-stu-id="17eb1-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="17eb1-110">Come impostare gli attributi di lettore video.</span><span class="sxs-lookup"><span data-stu-id="17eb1-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="17eb1-111">Si tratta di ASP.NET Razor pages funzionalità introdotte nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="17eb1-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="17eb1-112">Il `Video` helper.</span><span class="sxs-lookup"><span data-stu-id="17eb1-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="17eb1-113">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="17eb1-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="17eb1-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="17eb1-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="17eb1-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="17eb1-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="17eb1-116">Questa esercitazione si integra inoltre con WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="17eb1-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="17eb1-117">Introduzione</span><span class="sxs-lookup"><span data-stu-id="17eb1-117">Introduction</span></span>

<span data-ttu-id="17eb1-118">Si potrebbe voler visualizzare un video sul proprio sito.</span><span class="sxs-lookup"><span data-stu-id="17eb1-118">You might want to display a video on your site.</span></span> <span data-ttu-id="17eb1-119">Un modo per farlo consiste nel collegare a un sito che dispone già di tutto il video, ad esempio YouTube.</span><span class="sxs-lookup"><span data-stu-id="17eb1-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="17eb1-120">Se si vuole incorporare un video da tali siti direttamente nelle proprie pagine, è possibile ottenere in genere il markup HTML dal sito e quindi copiarlo nella pagina.</span><span class="sxs-lookup"><span data-stu-id="17eb1-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="17eb1-121">Ad esempio, nell'esempio seguente viene illustrato come incorporare un YouTube video:</span><span class="sxs-lookup"><span data-stu-id="17eb1-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="17eb1-122">Se si vuole riprodurre un video, nel tuo sito Web (non in un sito di condivisione video pubblico), è possibile collegare direttamente tramite markup embedded simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="17eb1-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="17eb1-123">Tuttavia, è possibile riprodurre i video dal sito usando il `Video` helper, che esegue il rendering di un lettore multimediale direttamente in una pagina.</span><span class="sxs-lookup"><span data-stu-id="17eb1-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="17eb1-124">Scelta di un lettore Video</span><span class="sxs-lookup"><span data-stu-id="17eb1-124">Choosing a Video Player</span></span>

<span data-ttu-id="17eb1-125">Esistono molti dei formati per i file video e ogni formato in genere richiede un altro lettore e un metodo diverso per configurare il giocatore.</span><span class="sxs-lookup"><span data-stu-id="17eb1-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="17eb1-126">Nelle pagine Razor di ASP.NET, è possibile riprodurre un video in una pagina web mediante il `Video` helper.</span><span class="sxs-lookup"><span data-stu-id="17eb1-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="17eb1-127">Il `Video` helper semplifica il processo di incorporare i video in una pagina web in quanto genera automaticamente il `object` e `embed` elementi HTML che vengono generalmente utilizzati per aggiungere video alla pagina.</span><span class="sxs-lookup"><span data-stu-id="17eb1-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="17eb1-128">Il `Video` helper supporta i lettori multimediali seguenti:</span><span class="sxs-lookup"><span data-stu-id="17eb1-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="17eb1-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="17eb1-129">Adobe Flash</span></span>
- <span data-ttu-id="17eb1-130">Windows Media Player</span><span class="sxs-lookup"><span data-stu-id="17eb1-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="17eb1-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="17eb1-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="17eb1-132">Il lettore Flash</span><span class="sxs-lookup"><span data-stu-id="17eb1-132">The Flash Player</span></span>

<span data-ttu-id="17eb1-133">Il `Flash` player del `Video` helper consentono di riprodurre video Flash (*con estensione SWF* file) in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="17eb1-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="17eb1-134">Come minimo, è necessario specificare un percorso per il file video.</span><span class="sxs-lookup"><span data-stu-id="17eb1-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="17eb1-135">Se si specifica solo il percorso, il lettore la Usa valori predefiniti impostati dalla versione corrente di Flash.</span><span class="sxs-lookup"><span data-stu-id="17eb1-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="17eb1-136">Le impostazioni predefinite tipiche sono:</span><span class="sxs-lookup"><span data-stu-id="17eb1-136">Typical default settings are:</span></span>

- <span data-ttu-id="17eb1-137">La visualizzazione del video con la larghezza predefinita e l'altezza e senza un colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="17eb1-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="17eb1-138">Il video viene riprodotto automaticamente quando il caricamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="17eb1-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="17eb1-139">Il video esegue il ciclo continua fino a quando non viene arrestato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="17eb1-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="17eb1-140">Il video viene ridimensionato per visualizzare tutti i video, anziché ritagliare il video per adattarsi a dimensioni specifiche.</span><span class="sxs-lookup"><span data-stu-id="17eb1-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="17eb1-141">Il video viene riprodotto in una finestra.</span><span class="sxs-lookup"><span data-stu-id="17eb1-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="17eb1-142">Il lettore di MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="17eb1-142">The MediaPlayer Player</span></span>

<span data-ttu-id="17eb1-143">Il `MediaPlayer` assegnatario del `Video` helper consente di riprodurre video Windows Media (*WMV* file), Windows Media audio (*WMA* file) e MP3 (*MP3* i file) in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="17eb1-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="17eb1-144">È necessario includere percorso del file multimediale da riprodurre; tutti gli altri parametri sono facoltativi.</span><span class="sxs-lookup"><span data-stu-id="17eb1-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="17eb1-145">Se si specifica solo un percorso, il lettore la Usa impostazioni predefinite configurate per la versione corrente di Media Player, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="17eb1-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="17eb1-146">La visualizzazione del video con la larghezza predefinita e l'altezza.</span><span class="sxs-lookup"><span data-stu-id="17eb1-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="17eb1-147">Il video viene riprodotto automaticamente quando il caricamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="17eb1-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="17eb1-148">Il video viene riprodotto una sola volta (non ciclo).</span><span class="sxs-lookup"><span data-stu-id="17eb1-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="17eb1-149">Windows Media player consente di visualizzare il set completo di controlli nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="17eb1-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="17eb1-150">Il video viene riprodotto in una finestra.</span><span class="sxs-lookup"><span data-stu-id="17eb1-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="17eb1-151">Il lettore Silverlight</span><span class="sxs-lookup"><span data-stu-id="17eb1-151">The Silverlight Player</span></span>

<span data-ttu-id="17eb1-152">Il `Silverlight` assegnatario del `Video` helper consente di riprodurre Windows Media Video (*WMV* file), Windows Media Audio (*WMA* file) e MP3 (*MP3* file).</span><span class="sxs-lookup"><span data-stu-id="17eb1-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="17eb1-153">È necessario impostare il parametro path in modo che punti a un pacchetto di applicazione basata su Silverlight (*XAP* file).</span><span class="sxs-lookup"><span data-stu-id="17eb1-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="17eb1-154">È anche necessario impostare i parametri di larghezza e altezza.</span><span class="sxs-lookup"><span data-stu-id="17eb1-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="17eb1-155">Tutti gli altri parametri sono facoltativi.</span><span class="sxs-lookup"><span data-stu-id="17eb1-155">All other parameters are optional.</span></span> <span data-ttu-id="17eb1-156">Quando si usa il lettore Silverlight per video, se si imposta solo i parametri obbligatori, il lettore Silverlight consente di visualizzare il video senza un colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="17eb1-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="17eb1-157">Nel caso in cui non si conosce già Silverlight: il *XAP* file è un file compresso che contiene informazioni sul layout in un *XAML* file, codice gestito nell'assembly e risorse facoltative.</span><span class="sxs-lookup"><span data-stu-id="17eb1-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="17eb1-158">È possibile creare un *XAP* file in Visual Studio come un progetto di applicazione Silverlight.</span><span class="sxs-lookup"><span data-stu-id="17eb1-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="17eb1-159">Il `Silverlight` lettore video utilizza sia le impostazioni fornite per il lettore e le impostazioni disponibili nel *XAP* file.</span><span class="sxs-lookup"><span data-stu-id="17eb1-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="17eb1-160">Tipi MIME</span><span class="sxs-lookup"><span data-stu-id="17eb1-160">MIME Types</span></span>
> 
> <span data-ttu-id="17eb1-161">Quando un browser scarica un file, il browser garantisce che il tipo di file corrisponde al tipo MIME specificato per il documento che viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="17eb1-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="17eb1-162">Il tipo MIME è il tipo di supporti o tipo di contenuto di un file.</span><span class="sxs-lookup"><span data-stu-id="17eb1-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="17eb1-163">Il `Video` helper Usa i tipi MIME seguenti:</span><span class="sxs-lookup"><span data-stu-id="17eb1-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="17eb1-164">Riproduzione di video Flash (con estensione SWF)</span><span class="sxs-lookup"><span data-stu-id="17eb1-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="17eb1-165">Questa procedura viene illustrato come riprodurre un video Flash denominato *sample.swf*.</span><span class="sxs-lookup"><span data-stu-id="17eb1-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="17eb1-166">Nella procedura si presuppone che hai una cartella denominata *supporti* nel sito e il *con estensione SWF* file si trova nella cartella.</span><span class="sxs-lookup"><span data-stu-id="17eb1-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="17eb1-167">Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non si già aggiunti.</span><span class="sxs-lookup"><span data-stu-id="17eb1-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="17eb1-168">Nel sito Web, aggiungere una pagina e denominarla *FlashVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="17eb1-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="17eb1-169">Aggiungere il markup seguente alla pagina:</span><span class="sxs-lookup"><span data-stu-id="17eb1-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="17eb1-170">Eseguire la pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="17eb1-170">Run the page in a browser.</span></span> <span data-ttu-id="17eb1-171">(Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.) Viene visualizzata la pagina e il video viene riprodotto automaticamente.</span><span class="sxs-lookup"><span data-stu-id="17eb1-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="17eb1-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span><span class="sxs-lookup"><span data-stu-id="17eb1-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="17eb1-173">È possibile impostare il `quality` parametro per un video Flash `low`, `autolow`, `autohigh`, `medium`, `high`, e `best`:</span><span class="sxs-lookup"><span data-stu-id="17eb1-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="17eb1-174">È possibile modificare il video Flash per riprodurre in una dimensione specifica usando il `scale` parametro, che è possibile impostare il testo seguente:</span><span class="sxs-lookup"><span data-stu-id="17eb1-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="17eb1-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="17eb1-175">`showall`.</span></span> <span data-ttu-id="17eb1-176">In questo modo l'intero video visibili, mantenendo le proporzioni originali.</span><span class="sxs-lookup"><span data-stu-id="17eb1-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="17eb1-177">Tuttavia, si potrebbe ottenere dei bordi su ogni lato.</span><span class="sxs-lookup"><span data-stu-id="17eb1-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="17eb1-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="17eb1-178">`noorder`.</span></span> <span data-ttu-id="17eb1-179">Questa configurazione è possibile il video, mantenendo le proporzioni originali, ma potrà essere ritagliata.</span><span class="sxs-lookup"><span data-stu-id="17eb1-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="17eb1-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="17eb1-180">`exactfit`.</span></span> <span data-ttu-id="17eb1-181">In questo modo l'intero video visibili senza mantenendo le proporzioni originali, ma possono verificarsi distorsioni.</span><span class="sxs-lookup"><span data-stu-id="17eb1-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="17eb1-182">Se non si specifica un `scale` parametro, l'intero video sarà visibile e verranno mantenute le proporzioni originali senza eventuali ritagli.</span><span class="sxs-lookup"><span data-stu-id="17eb1-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="17eb1-183">Nell'esempio seguente viene illustrato come utilizzare il `scale` parametro:</span><span class="sxs-lookup"><span data-stu-id="17eb1-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="17eb1-184">Il lettore Flash supporta una modalità video impostazione denominata `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="17eb1-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="17eb1-185">È possibile impostarlo su `window`, `opaque`, e `transparent`.</span><span class="sxs-lookup"><span data-stu-id="17eb1-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="17eb1-186">Per impostazione predefinita, il `windowMode` è impostata su `window`, che consente di visualizzare il video in una finestra separata nella pagina web.</span><span class="sxs-lookup"><span data-stu-id="17eb1-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="17eb1-187">Il `opaque` impostazione nasconde tutto dietro il video nella pagina web.</span><span class="sxs-lookup"><span data-stu-id="17eb1-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="17eb1-188">Il `transparent` impostazione consente lo sfondo della pagina web sono visibili attraverso video, presupponendo che qualsiasi parte del video è trasparente.</span><span class="sxs-lookup"><span data-stu-id="17eb1-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="17eb1-189">Riproduzione di MediaPlayer (*WMV*) video</span><span class="sxs-lookup"><span data-stu-id="17eb1-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="17eb1-190">La procedura seguente viene illustrato come riprodurre un video di finestra supporti denominato *Sample. wmv* che si trova nel *supporti* cartella.</span><span class="sxs-lookup"><span data-stu-id="17eb1-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="17eb1-191">Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se già stato fatto.</span><span class="sxs-lookup"><span data-stu-id="17eb1-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="17eb1-192">Creare una nuova pagina denominata *MediaPlayerVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="17eb1-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="17eb1-193">Aggiungere il markup seguente alla pagina:</span><span class="sxs-lookup"><span data-stu-id="17eb1-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="17eb1-194">Eseguire la pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="17eb1-194">Run the page in a browser.</span></span> <span data-ttu-id="17eb1-195">Il video viene caricato e viene eseguito automaticamente.</span><span class="sxs-lookup"><span data-stu-id="17eb1-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="17eb1-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span><span class="sxs-lookup"><span data-stu-id="17eb1-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="17eb1-197">È possibile impostare `playCount` a un intero che indica il numero di volte per riprodurre il video automaticamente:</span><span class="sxs-lookup"><span data-stu-id="17eb1-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="17eb1-198">Il `uiMode` parametro consente di specificare quali controlli visualizzati nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="17eb1-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="17eb1-199">È possibile impostare `uiMode` al `invisible`, `none`, `mini`, o `full`.</span><span class="sxs-lookup"><span data-stu-id="17eb1-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="17eb1-200">Se non si specifica un `uiMode` parametro, il video verrà visualizzata la finestra di stato, seek a barre, controllare i pulsanti e i controlli volume oltre alla finestra del video.</span><span class="sxs-lookup"><span data-stu-id="17eb1-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="17eb1-201">Questi controlli verranno visualizzati anche se si usa Windows Media player per riprodurre un file audio.</span><span class="sxs-lookup"><span data-stu-id="17eb1-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="17eb1-202">Ecco un esempio di come usare il `uiMode` parametro:</span><span class="sxs-lookup"><span data-stu-id="17eb1-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="17eb1-203">Per impostazione predefinita, audio è su quando la riproduzione del video.</span><span class="sxs-lookup"><span data-stu-id="17eb1-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="17eb1-204">È possibile disattivare l'audio impostando il `mute` parametro su true:</span><span class="sxs-lookup"><span data-stu-id="17eb1-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="17eb1-205">È possibile controllare il livello audio del video MediaPlayer impostando il `volume` parametro su un valore compreso tra 0 e 100.</span><span class="sxs-lookup"><span data-stu-id="17eb1-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="17eb1-206">Il valore predefinito è 50.</span><span class="sxs-lookup"><span data-stu-id="17eb1-206">The default value is 50.</span></span> <span data-ttu-id="17eb1-207">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="17eb1-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="17eb1-208">Riproduzione di video di Silverlight</span><span class="sxs-lookup"><span data-stu-id="17eb1-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="17eb1-209">Questa procedura viene illustrato come riprodurre video contenuto in un Silverlight *XAP* pagina in una cartella denominata *supporti*.</span><span class="sxs-lookup"><span data-stu-id="17eb1-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="17eb1-210">Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se già stato fatto.</span><span class="sxs-lookup"><span data-stu-id="17eb1-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="17eb1-211">Creare una nuova pagina denominata *SilverlightVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="17eb1-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="17eb1-212">Aggiungere il markup seguente alla pagina:</span><span class="sxs-lookup"><span data-stu-id="17eb1-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="17eb1-213">Eseguire la pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="17eb1-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="17eb1-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span><span class="sxs-lookup"><span data-stu-id="17eb1-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="17eb1-215">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="17eb1-215">Additional Resources</span></span>


<span data-ttu-id="17eb1-216">[Panoramica di Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="17eb1-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="17eb1-217">Flash oggetto e INCORPORAMENTO degli attributi del tag</span><span class="sxs-lookup"><span data-stu-id="17eb1-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="17eb1-218">[I tag PARAM SDK di Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="17eb1-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
