---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Utilizzo di immagini in un sito ASP.NET Web Pages (Razor) | Microsoft Docs
author: tfitzmac
description: In questo capitolo viene illustrato come aggiungere, visualizzare e manipolare immagini (ridimensionare, capovolgere e aggiungere filigrane) nel sito Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: e609cd1c6ab74b5b40d28bde353501dbacb5d544
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826783"
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Utilizzo di immagini in un sito di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come aggiungere, visualizzare e manipolare immagini (ridimensionare, capovolgere e aggiungere filigrane) in un sito Web ASP.NET Web Pages (Razor).
> 
> Che cosa si apprenderà come:
> 
> - Come aggiungere un'immagine a una pagina in modo dinamico.
> - Come consentire agli utenti di caricare un'immagine.
> - Come ridimensionare un'immagine.
> - Come capovolge o Ruota un'immagine.
> - Come aggiungere una filigrana a un'immagine.
> - Come usare un'immagine come filigrana.
> 
> Queste sono le funzionalità introdotte nell'articolo di programmazione ASP.NET:
> 
> - Il `WebImage` helper.
> - Il `Path` oggetto, che fornisce i metodi che consentono di modificare i nomi di file e percorso.
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


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Aggiunta di un'immagine a una pagina Web in modo dinamico

È possibile aggiungere immagini al sito Web e alle singole pagine mentre si sviluppa il sito Web. È anche possibile consentire agli utenti di caricare le immagini, che potrebbero essere utile per attività come il modo che possano aggiungere foto del profilo.

Se un'immagine è già disponibile nel sito di e si desidera semplicemente visualizzarlo in una pagina, si usa un elemento HTML `<img>` elemento simile al seguente:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

In alcuni casi, tuttavia, è necessario essere in grado di visualizzare immagini in modo dinamico &#8212; , ovvero non si conosce quali immagini per visualizzare fino a quando la pagina è in esecuzione.

La procedura descritta in questa sezione viene illustrato come visualizzare un'immagine in tempo reale in cui gli utenti specificano il nome di file di immagine da un elenco di nomi di immagine. Si seleziona il nome dell'immagine da un elenco di riepilogo a discesa e quando si invia la pagina, viene visualizzata l'immagine selezionata.

![[immagine]](9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. Creare un nuovo sito Web in WebMatrix.
2. Aggiungere una nuova pagina denominata *DynamicImage.cshtml*.
3. Nella cartella radice del sito Web, aggiungere una nuova cartella e denominarlo *immagini*.
4. Aggiungere quattro immagini per le *immagini* cartella appena creata. (Tutte le immagini sono sarà utile eseguire, ma rientrino in una pagina). Rinominare le immagini *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, e *Photo4.jpg*. (Non verranno usati *Photo4.jpg* in questa procedura, ma si sarà usarlo più avanti nell'articolo.)
5. Verificare che le quattro immagini non siano contrassegnate come di sola lettura.
6. Sostituire il contenuto esistente nella pagina con il codice seguente:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Il corpo della pagina presenta un elenco di riepilogo a discesa (un `<select>` elemento) denominata `photoChoice`. Nell'elenco è disponibili tre opzioni e il `value` attributo di ogni opzione di elenco con il nome di una delle immagini che si inserisce nel *immagini* cartella. In pratica, l'elenco consente all'utente di selezionare un nome descrittivo, ad esempio &quot;1 foto&quot;, e quindi passa le *jpg* nome del file quando viene inviata la pagina.

    Nel codice, è possibile ottenere la selezione dell'utente (in altre parole, il nome file immagine) dall'elenco leggendo `Request["photoChoice"]`. Prima di tutto verificare se esiste una selezione affatto. Se è presente, è costruire un percorso per l'immagine che include il nome della cartella per le immagini e nome file di immagine dell'utente. (Se si è provato a creare un percorso, ma non c'era in `Request["photoChoice"]`, verrà visualizzato un errore.) Ciò comporta un percorso relativo simile al seguente:

    *le immagini/Photo1.jpg*

    Il percorso è archiviato nella variabile denominata `imagePath` che sarà necessario in un secondo momento nella pagina.

    Nel corpo, è inoltre disponibile un `<img>` elemento usato per visualizzare l'immagine che l'utente selezionato. Il `src` attributo non è impostato su un nome file o un URL, come si farebbe per visualizzare un elemento statico. Al contrario, impostarlo su `@imagePath`, vale a dire che ottiene il relativo valore dal percorso è impostato nel codice.

    La prima volta che viene eseguita la pagina, tuttavia, non è presente alcuna immagine da visualizzare, perché l'utente non è stato selezionato nulla. Ciò in genere significa che il `src` attributo potrebbe essere vuoto e l'immagine viene visualizzato come una linea rossa &quot;x&quot; (o qualunque sia il browser esegue il rendering quando non è possibile trovare un'immagine). Per evitare questo problema, inserire il `<img>` elemento in un `if` blocco che verifichi se il `imagePath` variabile sia. Se l'utente ha effettuato una selezione, `imagePath` contiene il percorso. Se l'utente non ha selezionato un'immagine o se questa è la prima volta che viene visualizzata la pagina, il `<img>` elemento non viene ancora eseguito il rendering.
7. Salvare il file ed eseguire la pagina in un browser. (Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.)
8. Selezionare un'immagine nell'elenco a discesa e quindi fare clic su **immagine di esempio**. Assicurarsi che sia visualizzato diverse immagini per diverse opzioni.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Caricamento di un'immagine

Nell'esempio precedente è stato illustrato come visualizzare un'immagine in modo dinamico, ma funzionava solo con le immagini che erano già presenti nel tuo sito Web. Questa procedura illustra come consentire agli utenti di caricare un'immagine, che viene quindi visualizzata nella pagina. In ASP.NET, è possibile modificare immagini in tempo reale usando la `WebImage` helper, che dispone di metodi che consentono di creare, modificare e salvare le immagini. Il `WebImage` helper supporta tutti i web immagine tipi di file comuni, tra cui *jpg*, *PNG*, e *bmp*. In questo articolo, si userà *jpg* immagini, ma è possibile usare uno dei tipi di immagine.

![[immagine]](9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Aggiungere una nuova pagina e denominarla *UploadImage.cshtml*.
2. Sostituire il contenuto esistente nella pagina con il codice seguente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Il corpo del testo è un `<input type="file">` elemento, che consente agli utenti di selezionare un file da caricare. Quando si fa clic su **Submit**, il file che viene viene inviato insieme a form.

    Per ottenere l'immagine caricata, si utilizza il `WebImage` helper, con tutti i tipi di metodi utili per l'utilizzo di immagini. In particolare, si utilizza `WebImage.GetImageFromRequest` per ottenere l'immagine caricata (se presente) e archiviarlo in una variabile denominata `photo`.

    Molte delle operazioni in questo esempio prevede ottenendo e impostando i nomi di file e percorso. Il problema è che si desidera ottenere il nome (e solo il nome) dell'immagine che l'utente ha caricato e quindi creare un nuovo percorso in cui si intende archiviare l'immagine. Poiché gli utenti potenzialmente è stato possibile caricare più immagini che hanno lo stesso nome, è utilizzare un po' di codice aggiuntivo per creare nomi univoci e assicurarsi che gli utenti non sovrascrivere le immagini esistenti.

    Se un'immagine è effettivamente stata caricata (il test `if (photo != null)`), ottenuto il nome dell'immagine dell'immagine `FileName` proprietà. Quando l'utente carica l'immagine, `FileName` contiene il nome dell'utente originale, che include il percorso dal computer dell'utente. Potrebbe essere simile al seguente:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Non si desidera tutte queste informazioni di percorso, anche se &#8212; è sufficiente il nome del file effettivo (*SamplePhoto1.jpg*). È possibile eliminare le solo il file da un percorso tramite il `Path.GetFileName` metodo, simile al seguente:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Quindi possibile creare un nuovo nome univoco aggiungendo un GUID per il nome originale. (Per altre informazioni su GUID, vedere [sui GUID](#SB_AboutGUIDs) più avanti in questo articolo.) Quindi necessario creare un percorso completo che è possibile usare per salvare l'immagine. Salva percorso è costituito da nome del nuovo file, la cartella (immagini) e il percorso di sito Web corrente.

    > [!NOTE]
    > Affinché il codice salvare i file nei *immagini* cartella, l'applicazione deve lettura / scrittura delle autorizzazioni per tale cartella. Nel computer di sviluppo non si tratta in genere un problema. Tuttavia, quando si pubblica il sito al server web del provider di hosting, potrebbe essere necessario impostare in modo esplicito tali autorizzazioni. Se si esegue questo codice nel server del provider di hosting e ottenere gli errori, controllare con il provider di hosting per scoprire come impostare queste autorizzazioni.

    Infine, si passa il salvataggio percorso per il `Save` metodo del `WebImage` helper. Consente di archiviare l'immagine caricata con il nuovo nome. Il salvataggio metodo aspetto simile al seguente: `photo.Save(@"~\" + imagePath)`. Il percorso completo viene aggiunto alla `@"~\"`, ovvero il percorso di sito Web corrente. (Per informazioni sul `~` operatore, vedere [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Come nell'esempio precedente, il corpo della pagina contiene un `<img>` elemento per visualizzare l'immagine. Se `imagePath` è stata impostata, il `<img>` elemento sottoposto a rendering e la relativa `src` attributo è impostato sul `imagePath` valore.
3. Eseguire la pagina in un browser.
4. Caricare un'immagine e verificare che viene visualizzato nella pagina.
5. Nel sito, aprire il *immagini* cartella. Noterete che è stato aggiunto un nuovo file il cui nome di file simile al seguente: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Questa è l'immagine che è stato caricato con un GUID come preceduto al nome. (Il proprio file avrà un GUID diverso e probabilmente viene denominato qualcosa di diverso da *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Sui GUID
> 
> Un GUID (globally unique ID) è un identificatore che in genere viene eseguito il rendering in un formato simile al seguente: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. I numeri e lettere (da A-F) sono diversi per ogni GUID, ma seguono il modello d'uso dei gruppi di 8-4-4-4-12 caratteri. (Tecnicamente, un GUID è un numero di 16 byte/128 bit) Quando è necessario un GUID, è possibile chiamare codice specializzato che genera un GUID per l'utente. L'idea alla base GUID è che tra le dimensioni del numero enorme (3,4 x 10<sup>38</sup>) e l'algoritmo per generarlo, il numero risulta è praticamente garantito a essere uno di questi tipi. I GUID sono pertanto un ottimo modo per generare nomi per le cose quando è necessario garantire che non usi lo stesso nome due volte. Lo svantaggio, naturalmente, è che i GUID non sono particolarmente intuitivo, tendono a essere utilizzato quando il nome viene usato solo nel codice.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Ridimensionamento di un'immagine

Se il sito Web accetta le immagini da un utente, si potrebbe voler ridimensionare le immagini prima di visualizzare o salvarli. È possibile usare nuovamente il `WebImage` helper per l'oggetto.

Questa procedura illustra come ridimensionare un'immagine caricata per creare un'immagine di anteprima e quindi salvare l'anteprima e l'immagine originale nel sito Web. È l'anteprima vengono visualizzate nella pagina e usare un collegamento ipertestuale per reindirizzare gli utenti per l'immagine ingrandita.

![[immagine]](9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Aggiungere una nuova pagina denominata *Thumbnail.cshtml*.
2. Nel *immagini* cartella, creare una sottocartella denominata *thumbs*.
3. Sostituire il contenuto esistente nella pagina con il codice seguente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Questo codice è simile al codice dell'esempio precedente. La differenza è che questo codice consente di salvare l'immagine di due volte, una volta in genere e una volta dopo aver creato una copia dell'immagine di anteprima. Prima di tutto è possibile ottenere l'immagine caricata e salvarlo nel *immagini* cartella. Quindi si crea un nuovo percorso dell'immagine di anteprima. Per creare effettivamente l'anteprima, si chiama il `WebImage` dell'helper `Resize` metodo per creare un'immagine di 60 pixel da 60 x pixel. Nell'esempio viene illustrato come si conservi le proporzioni e come è possibile impedire l'immagine viene ingrandita (nel caso in cui la nuova dimensione effettivamente renderebbero l'immagine più grande). Le immagini ridimensionate vengono salvati nel *thumbs* sottocartella.

    Alla fine del markup, usare la stessa `<img>` elemento con dinamica `src` attributo che è possibile vedere negli esempi precedenti per visualizzare in modo condizionale l'immagine. In questo caso, si visualizza l'anteprima. È anche possibile usare un `<a>` elemento per creare un collegamento ipertestuale per la versione grande dell'immagine. Come con la `src` attributo del `<img>` elemento, si imposta la `href` attributo del `<a>` elemento in modo dinamico a qualsiasi oggetto presente in `imagePath`. Per assicurarsi che il percorso sia possibile utilizzarlo come URL, si passa `imagePath` per il `Html.AttributeEncode` metodo, che converte i caratteri riservati nel percorso in caratteri che si è disposti in un URL.
4. Eseguire la pagina in un browser.
5. Caricare una foto e verificare che venga visualizzato l'anteprima.
6. Fare clic sull'anteprima per visualizzare l'immagine con dimensioni normali.
7. Nel *immagini* e *immagini/thumbs*, si noti che sono stati aggiunti nuovi file.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>La rotazione e capovolgimento di un'immagine

Il `WebImage` helper consente inoltre di capovolgimento e rotazione di immagini. Questa procedura viene illustrato come ottenere un'immagine dal server, capovolgere l'immagine capovolta (verticale), salvarlo e quindi visualizzare l'immagine capovolta nella pagina. In questo esempio si usa solo un file già presenti nel server (*Photo2.jpg*). In un'applicazione reale, è probabile che non farebbe altro un'immagine il cui nome si ottiene in modo dinamico, come negli esempi precedenti.

![[immagine]](9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Aggiungere una nuova pagina denominata *FlipImage.cshtml*.
2. Sostituire il contenuto esistente nella pagina con il codice seguente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Il codice Usa il `WebImage` helper per ottenere un'immagine dal server. Si crea il percorso dell'immagine usando la stessa tecnica utilizzata negli esempi precedenti per il salvataggio delle immagini e si passa tale percorso quando si crea un'immagine utilizzando `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Se viene trovata un'immagine, è costruire un nuovo percorso e nome file, come negli esempi precedenti. Per capovolgere l'immagine, si chiama il `FlipVertical` (metodo) e quindi salvare l'immagine di nuovo.

    L'immagine viene nuovamente visualizzata nella pagina utilizzando la `<img>` elemento con il `src` attributo impostato su `imagePath`.
3. Eseguire la pagina in un browser. L'immagine per *Photo2.jpg* viene visualizzato capovolto.
4. Aggiornare la pagina o richiedere la pagina per visualizzare che l'immagine viene capovolta sul lato destro di nuovamente.

Per ruotare un'immagine, si usa lo stesso codice, tranne il fatto che anziché chiamare il `FlipVertical` oppure `FlipHorizontal`, si chiama `RotateLeft` o `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Aggiungere una filigrana all'immagine

Quando si aggiungere immagini al tuo sito Web, è possibile aggiungere una filigrana all'immagine prima di salvarlo o visualizzarlo in una pagina. Le persone usano spesso le filigrane per aggiungere le informazioni sul copyright di un'immagine o per annunciare il proprio nome aziendale.

![[immagine]](9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Aggiungere una nuova pagina denominata *Watermark.cshtml*.
2. Sostituire il contenuto esistente nella pagina con il codice seguente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Questo codice è simile al codice nel *FlipImage.cshtml* pagina illustrato in precedenza (anche se in questo caso viene utilizzato il *Photo3.jpg* file). Per aggiungere il limite, si chiama il `WebImage` dell'helper `AddTextWatermark` metodo prima di salvare l'immagine. Nella chiamata a `AddTextWatermark`, è possibile passare il testo &quot;filigrana My&quot;, impostare il colore del carattere in giallo e impostare la famiglia di caratteri viene impostato su Arial. (Anche se non è visualizzato in questo caso, il `WebImage` helper consente anche di specificare l'opacità, famiglia di caratteri e dimensioni del carattere e la posizione del testo della filigrana.) Quando si salva l'immagine non deve essere sola lettura.

    Come si è visto prima, nella pagina viene visualizzata l'immagine usando il `<img>` elemento con l'attributo src impostato su `@imagePath`.
3. Eseguire la pagina in un browser. Si noti che il testo "My della filigrana" nell'angolo in basso a destra dell'immagine.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Usando un'immagine come filigrana

Invece di usare il testo per un limite, è possibile usare un'altra immagine. Talvolta le persone utilizzano immagini quali logo della società come filigrana, o un'immagine di filigrana usano invece del testo per le informazioni sul copyright.

![[immagine]](9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Aggiungere una nuova pagina denominata *ImageWatermark.cshtml*.
2. Aggiungere un'immagine per il *immagini* cartella che è possibile usare come logo e rinominare l'immagine *MyCompanyLogo.jpg*. Questa immagine deve essere un'immagine che si può vedere chiaramente quando è impostata su 80 pixel di larghezza e 20 pixel di altezza.
3. Sostituire il contenuto esistente nella pagina con il codice seguente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Si tratta di un'altra variante del codice degli esempi precedenti. In questo caso, si chiama `AddImageWatermark` per aggiungere l'immagine di filigrana all'immagine di destinazione (*Photo3.jpg*) prima di salvare l'immagine. Quando si chiama `AddImageWatermark`, si imposta la larghezza su 80 pixel e l'altezza e 20 pixel. Il *MyCompanyLogo.jpg* immagine è allineata al centro orizzontalmente e allineata verticalmente nella parte inferiore dell'immagine di destinazione. L'opacità è impostato su 100% e la spaziatura interna è impostata su 10 pixel. Se l'immagine della filigrana è più grande rispetto all'immagine di destinazione, non accade nulla. Se l'immagine della filigrana è più grande rispetto all'immagine di destinazione e si imposta la spaziatura interna per la filigrana di immagine su zero, la filigrana viene ignorata.

    Poiché in precedenza, vengono visualizzati l'immagine usando il `<img>` elementi e una dinamica `src` attributo.
4. Eseguire la pagina in un browser. Si noti che l'immagine di filigrana viene visualizzata nella parte inferiore dell'immagine principale.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Uso di file in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202896)

[Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=251587)
