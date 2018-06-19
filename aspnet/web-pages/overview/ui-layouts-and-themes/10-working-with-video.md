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
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="aea41-103">Visualizzazione di Video in un sito Web di ASP.NET di pagine (Razor)</span><span class="sxs-lookup"><span data-stu-id="aea41-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="aea41-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="aea41-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="aea41-105">In questo articolo viene illustrato come usare un lettore video (supporto) in un sito Web ASP.NET Web Pages (Razor) per consentire agli utenti di visualizzare video in cui vengono archiviati nel sito.</span><span class="sxs-lookup"><span data-stu-id="aea41-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="aea41-106">ASP.NET Web Pages con sintassi Razor consente di riprodurre Flash (*SWF*), Media Player (*WMV*) e Silverlight (*XAP*) video.</span><span class="sxs-lookup"><span data-stu-id="aea41-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="aea41-107">Illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="aea41-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="aea41-108">Come scegliere un lettore video.</span><span class="sxs-lookup"><span data-stu-id="aea41-108">How to choose a video player.</span></span>
> - <span data-ttu-id="aea41-109">Come aggiungere video a una pagina web.</span><span class="sxs-lookup"><span data-stu-id="aea41-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="aea41-110">Come impostare gli attributi di lettore video.</span><span class="sxs-lookup"><span data-stu-id="aea41-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="aea41-111">Si tratta di ASP.NET Razor delle pagine delle funzionalità introdotte in questo articolo:</span><span class="sxs-lookup"><span data-stu-id="aea41-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="aea41-112">Il `Video` helper.</span><span class="sxs-lookup"><span data-stu-id="aea41-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="aea41-113">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="aea41-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="aea41-114">Pagine Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="aea41-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="aea41-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="aea41-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="aea41-116">In questa esercitazione si integra inoltre con 3 di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="aea41-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="aea41-117">Introduzione</span><span class="sxs-lookup"><span data-stu-id="aea41-117">Introduction</span></span>

<span data-ttu-id="aea41-118">Si potrebbe voler visualizzare un video sul proprio sito.</span><span class="sxs-lookup"><span data-stu-id="aea41-118">You might want to display a video on your site.</span></span> <span data-ttu-id="aea41-119">Un modo per eseguire questa operazione consiste nel collegare a un sito che dispone già del video, ad esempio YouTube.</span><span class="sxs-lookup"><span data-stu-id="aea41-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="aea41-120">Se si desidera incorporare un video da questi siti direttamente in pagine, è possibile ottenere in genere il markup HTML dal sito e quindi copiarlo in una pagina.</span><span class="sxs-lookup"><span data-stu-id="aea41-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="aea41-121">Ad esempio, nell'esempio seguente viene illustrato come incorporare un YouTube video:</span><span class="sxs-lookup"><span data-stu-id="aea41-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="aea41-122">Se si desidera riprodurre un video presente nel proprio sito Web (non in un sito di video di condivisione pubblico), non è possibile collegare direttamente al markup incorporato simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="aea41-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="aea41-123">Tuttavia, è possibile riprodurre video dal sito utilizzando il `Video` helper, che esegue il rendering di un lettore multimediale direttamente in una pagina.</span><span class="sxs-lookup"><span data-stu-id="aea41-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="aea41-124">Scelta di un lettore Video</span><span class="sxs-lookup"><span data-stu-id="aea41-124">Choosing a Video Player</span></span>

<span data-ttu-id="aea41-125">Esistono molti dei formati per i file video, e ogni formato in genere richiede un altro lettore e un metodo diverso per configurare Windows Media player.</span><span class="sxs-lookup"><span data-stu-id="aea41-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="aea41-126">Nelle pagine ASP.NET Razor, è possibile riprodurre un video in una pagina web utilizzando il `Video` helper.</span><span class="sxs-lookup"><span data-stu-id="aea41-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="aea41-127">Il `Video` helper semplifica il processo di incorporamento video in una pagina web, perché genera automaticamente il `object` e `embed` HTML che vengono generalmente utilizzati per aggiungere video alla pagina.</span><span class="sxs-lookup"><span data-stu-id="aea41-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="aea41-128">Il `Video` helper supporta i seguenti lettori multimediali:</span><span class="sxs-lookup"><span data-stu-id="aea41-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="aea41-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="aea41-129">Adobe Flash</span></span>
- <span data-ttu-id="aea41-130">Windows Media Player</span><span class="sxs-lookup"><span data-stu-id="aea41-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="aea41-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="aea41-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="aea41-132">Il lettore Flash</span><span class="sxs-lookup"><span data-stu-id="aea41-132">The Flash Player</span></span>

<span data-ttu-id="aea41-133">Il `Flash` player del `Video` helper consentono di riprodurre video Flash (*SWF* file) in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="aea41-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="aea41-134">Come minimo, è necessario specificare un percorso per il file video.</span><span class="sxs-lookup"><span data-stu-id="aea41-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="aea41-135">Se si specifica solo il percorso, il lettore utilizza valori predefiniti impostati dalla versione corrente di Flash.</span><span class="sxs-lookup"><span data-stu-id="aea41-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="aea41-136">Impostazioni predefinite tipici sono:</span><span class="sxs-lookup"><span data-stu-id="aea41-136">Typical default settings are:</span></span>

- <span data-ttu-id="aea41-137">La visualizzazione del video usando la larghezza predefinita e l'altezza e senza un colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="aea41-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="aea41-138">Il video viene riprodotto automaticamente quando il caricamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="aea41-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="aea41-139">Il video esegue il ciclo continua fino a quando non è stato arrestato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="aea41-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="aea41-140">Il video viene ridimensionato per visualizzare tutto il video, anziché ritagliare il video per adattarsi a dimensioni specifiche.</span><span class="sxs-lookup"><span data-stu-id="aea41-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="aea41-141">La riproduzione del video in una finestra.</span><span class="sxs-lookup"><span data-stu-id="aea41-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="aea41-142">Il lettore di Media Player</span><span class="sxs-lookup"><span data-stu-id="aea41-142">The MediaPlayer Player</span></span>

<span data-ttu-id="aea41-143">Il `MediaPlayer` player del `Video` supporto consente di riprodurre il video di Windows Media (*WMV* file), Windows Media audio (*WMA* file), MP3 e (*MP3* i file) in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="aea41-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="aea41-144">È necessario includere percorso del file multimediale da riprodurre; tutti gli altri parametri sono facoltativi.</span><span class="sxs-lookup"><span data-stu-id="aea41-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="aea41-145">Se si specifica solo un percorso, il lettore utilizza impostazioni predefinite configurate per la versione corrente di Media Player, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="aea41-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="aea41-146">La visualizzazione del video usando predefinito larghezza e altezza.</span><span class="sxs-lookup"><span data-stu-id="aea41-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="aea41-147">Il video viene riprodotto automaticamente quando il caricamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="aea41-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="aea41-148">Il video viene riprodotto una sola volta (non ciclo).</span><span class="sxs-lookup"><span data-stu-id="aea41-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="aea41-149">Windows Media player consente di visualizzare l'elenco completo dei controlli nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="aea41-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="aea41-150">La riproduzione del video in una finestra.</span><span class="sxs-lookup"><span data-stu-id="aea41-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="aea41-151">Il lettore Silverlight</span><span class="sxs-lookup"><span data-stu-id="aea41-151">The Silverlight Player</span></span>

<span data-ttu-id="aea41-152">Il `Silverlight` player del `Video` supporto consente di riprodurre Windows Media Video (*WMV* file), Windows Media Audio (*WMA* file), MP3 e (*MP3* file).</span><span class="sxs-lookup"><span data-stu-id="aea41-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="aea41-153">È necessario impostare il parametro path in modo che punti a un pacchetto di applicazione basata su Silverlight (*XAP* file).</span><span class="sxs-lookup"><span data-stu-id="aea41-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="aea41-154">È inoltre necessario impostare i parametri di larghezza e altezza.</span><span class="sxs-lookup"><span data-stu-id="aea41-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="aea41-155">Tutti gli altri parametri sono facoltativi.</span><span class="sxs-lookup"><span data-stu-id="aea41-155">All other parameters are optional.</span></span> <span data-ttu-id="aea41-156">Quando si utilizza il lettore Silverlight per video, se si imposta solo i parametri obbligatori, il lettore Silverlight consente di visualizzare il video senza un colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="aea41-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="aea41-157">Nel caso in cui non si conosce già Silverlight: il *XAP* file è un file compresso che contiene informazioni sul layout in un *XAML* file, codice gestito nell'assembly e risorse facoltative.</span><span class="sxs-lookup"><span data-stu-id="aea41-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="aea41-158">È possibile creare un *XAP* file in Visual Studio come un progetto di applicazione Silverlight.</span><span class="sxs-lookup"><span data-stu-id="aea41-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="aea41-159">Il `Silverlight` lettore video utilizza le impostazioni fornite per il lettore sia le impostazioni che sono state fornite nel *XAP* file.</span><span class="sxs-lookup"><span data-stu-id="aea41-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="aea41-160">Tipi MIME</span><span class="sxs-lookup"><span data-stu-id="aea41-160">MIME Types</span></span>
> 
> <span data-ttu-id="aea41-161">Quando un browser scarica un file, il browser assicura che il tipo di file corrisponde il tipo MIME specificato per il documento che viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="aea41-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="aea41-162">Il tipo MIME è il tipo di supporti o di tipo di contenuto di un file.</span><span class="sxs-lookup"><span data-stu-id="aea41-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="aea41-163">Il `Video` helper utilizza i tipi MIME seguenti:</span><span class="sxs-lookup"><span data-stu-id="aea41-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="aea41-164">Riproduzione di video Flash (SWF)</span><span class="sxs-lookup"><span data-stu-id="aea41-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="aea41-165">In questa procedura viene illustrato come riprodurre un video dimostrativo denominato *sample.swf*.</span><span class="sxs-lookup"><span data-stu-id="aea41-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="aea41-166">Nella procedura si presuppone che si dispone di una cartella denominata *Media* nel sito e che il *SWF* file si trova in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="aea41-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="aea41-167">Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non si già aggiunti.</span><span class="sxs-lookup"><span data-stu-id="aea41-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="aea41-168">Nel sito Web, aggiungere una pagina e denominarlo *FlashVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aea41-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="aea41-169">Aggiungere il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="aea41-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="aea41-170">Eseguire la pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="aea41-170">Run the page in a browser.</span></span> <span data-ttu-id="aea41-171">(Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.) Verrà visualizzata la pagina e il video viene riprodotto automaticamente.</span><span class="sxs-lookup"><span data-stu-id="aea41-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="aea41-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span><span class="sxs-lookup"><span data-stu-id="aea41-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="aea41-173">È possibile impostare il `quality` parametro per un video dimostrativo per `low`, `autolow`, `autohigh`, `medium`, `high`, e `best`:</span><span class="sxs-lookup"><span data-stu-id="aea41-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="aea41-174">È possibile modificare il Flash video viene riprodotto in una dimensione specifica usando il `scale` parametro, è possibile impostare per le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="aea41-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="aea41-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="aea41-175">`showall`.</span></span> <span data-ttu-id="aea41-176">In questo modo l'intero video visibili mantenendo le proporzioni originali.</span><span class="sxs-lookup"><span data-stu-id="aea41-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="aea41-177">Tuttavia, potrebbero finire con bordi su ciascun lato.</span><span class="sxs-lookup"><span data-stu-id="aea41-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="aea41-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="aea41-178">`noorder`.</span></span> <span data-ttu-id="aea41-179">Si ridimensiona il video, mantenendo le proporzioni originali, ma potrà essere ritagliata.</span><span class="sxs-lookup"><span data-stu-id="aea41-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="aea41-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="aea41-180">`exactfit`.</span></span> <span data-ttu-id="aea41-181">In questo modo l'intero video visibili senza mantenendo le proporzioni originali, ma potrebbe verificarsi una distorsione.</span><span class="sxs-lookup"><span data-stu-id="aea41-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="aea41-182">Se non si specifica un `scale` parametro, l'intero video sarà visibile e verranno mantenute le proporzioni originali senza eventuali ritagli.</span><span class="sxs-lookup"><span data-stu-id="aea41-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="aea41-183">Nell'esempio seguente viene illustrato come utilizzare il `scale` parametro:</span><span class="sxs-lookup"><span data-stu-id="aea41-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="aea41-184">Il lettore Flash supporta una modalità video impostazione denominata `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="aea41-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="aea41-185">È possibile impostarlo su `window`, `opaque`, e `transparent`.</span><span class="sxs-lookup"><span data-stu-id="aea41-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="aea41-186">Per impostazione predefinita, il `windowMode` è impostato su `window`, che consente di visualizzare il video in una finestra separata della pagina web.</span><span class="sxs-lookup"><span data-stu-id="aea41-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="aea41-187">Il `opaque` impostazione nasconde tutti gli elementi dietro il video sulla pagina web.</span><span class="sxs-lookup"><span data-stu-id="aea41-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="aea41-188">Il `transparent` impostazione consente di sfondo della pagina web sono visibili attraverso video, presupponendo che qualsiasi parte del video è trasparente.</span><span class="sxs-lookup"><span data-stu-id="aea41-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="aea41-189">Riproduzione di Media Player (*WMV*) video</span><span class="sxs-lookup"><span data-stu-id="aea41-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="aea41-190">La procedura seguente viene illustrato come riprodurre un video Media finestra denominato *Sample. wmv* che si trova nel *Media* cartella.</span><span class="sxs-lookup"><span data-stu-id="aea41-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="aea41-191">Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se hai già fatto.</span><span class="sxs-lookup"><span data-stu-id="aea41-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="aea41-192">Creare una nuova pagina denominata *MediaPlayerVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aea41-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="aea41-193">Aggiungere il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="aea41-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="aea41-194">Eseguire la pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="aea41-194">Run the page in a browser.</span></span> <span data-ttu-id="aea41-195">Nel video viene caricato e viene eseguito automaticamente.</span><span class="sxs-lookup"><span data-stu-id="aea41-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="aea41-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span><span class="sxs-lookup"><span data-stu-id="aea41-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="aea41-197">È possibile impostare `playCount` in un intero che indica il numero di volte per riprodurre il video automaticamente:</span><span class="sxs-lookup"><span data-stu-id="aea41-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="aea41-198">Il `uiMode` parametro consente di specificare quali controlli visualizzati nell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="aea41-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="aea41-199">È possibile impostare `uiMode` a `invisible`, `none`, `mini`, o `full`.</span><span class="sxs-lookup"><span data-stu-id="aea41-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="aea41-200">Se non si specifica un `uiMode` parametro, il video verrà visualizzata la finestra di stato, seek barre, controllare i pulsanti e i controlli volume oltre la finestra video.</span><span class="sxs-lookup"><span data-stu-id="aea41-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="aea41-201">Questi controlli verranno visualizzati anche se si utilizza il lettore per riprodurre un file audio.</span><span class="sxs-lookup"><span data-stu-id="aea41-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="aea41-202">Di seguito è riportato un esempio di come utilizzare il `uiMode` parametro:</span><span class="sxs-lookup"><span data-stu-id="aea41-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="aea41-203">Per impostazione predefinita, audio è durante la riproduzione del video.</span><span class="sxs-lookup"><span data-stu-id="aea41-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="aea41-204">È possibile disattivare l'audio impostando il `mute` su true:</span><span class="sxs-lookup"><span data-stu-id="aea41-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="aea41-205">È possibile controllare il livello audio del video MediaPlayer impostando il `volume` parametro con un valore compreso tra 0 e 100.</span><span class="sxs-lookup"><span data-stu-id="aea41-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="aea41-206">Il valore predefinito è 50.</span><span class="sxs-lookup"><span data-stu-id="aea41-206">The default value is 50.</span></span> <span data-ttu-id="aea41-207">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="aea41-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="aea41-208">Riproduzione di video di Silverlight</span><span class="sxs-lookup"><span data-stu-id="aea41-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="aea41-209">Questa procedura viene illustrato come riprodurre video contenuto Silverlight *XAP* pagina in una cartella denominata *Media*.</span><span class="sxs-lookup"><span data-stu-id="aea41-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="aea41-210">Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se hai già fatto.</span><span class="sxs-lookup"><span data-stu-id="aea41-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="aea41-211">Creare una nuova pagina denominata *SilverlightVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aea41-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="aea41-212">Aggiungere il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="aea41-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="aea41-213">Eseguire la pagina in un browser.</span><span class="sxs-lookup"><span data-stu-id="aea41-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="aea41-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span><span class="sxs-lookup"><span data-stu-id="aea41-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="aea41-215">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="aea41-215">Additional Resources</span></span>


<span data-ttu-id="aea41-216">[Panoramica di Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="aea41-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="aea41-217">Flash gli attributi di tag di oggetto e INCORPORAMENTO</span><span class="sxs-lookup"><span data-stu-id="aea41-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="aea41-218">[I tag PARAM SDK di Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="aea41-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
