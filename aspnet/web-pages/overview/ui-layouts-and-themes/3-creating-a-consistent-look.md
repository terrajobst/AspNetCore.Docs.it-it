---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Creazione di un Layout coerente in ASP.NET Web Pages (Razor) siti | Microsoft Docs
author: tfitzmac
description: Per rendere più efficiente creare pagine web per il sito, è possibile creare blocchi riutilizzabili di contenuto (ad esempio, le intestazioni e piè di pagina) per il sito Web e si c...
ms.author: riande
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 82d55bb8eee6c8121a5e4d3d8121bf2b075e3df8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832169"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Creazione di un Layout coerente in ASP.NET Web Pages (Razor) Sites
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come è possibile usare le pagine di layout in un sito Web ASP.NET Web Pages (Razor) per creare blocchi riutilizzabili di contenuto (ad esempio, le intestazioni e piè di pagina) e creare un aspetto coerente per tutte le pagine nel sito.
> 
> **Che cosa si apprenderà come:** 
> 
> - Come creare blocchi riutilizzabili di contenuto, ad esempio le intestazioni e piè di pagina.
> - Come creare un aspetto coerente per tutte le pagine del sito utilizzando un layout.
> - Come passare i dati in fase di esecuzione a una pagina di layout.
> 
> Queste sono le funzionalità ASP.NET introdotte nell'articolo:
> 
> - Blocchi di contenuto, ovvero file contenenti contenuto in formato HTML da inserire in più pagine.
> - Pagine di layout, ovvero le pagine contenenti contenuto in formato HTML che può essere condiviso dalle pagine sul sito Web.
> - Il `RenderPage`, `RenderBody`, e `RenderSection` metodi, che indicano dove inserire gli elementi della pagina ASP.NET.
> - Il `PageData` dizionario che ti permette di condividere dati tra i blocchi di contenuto e pagine di layout.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="about-layout-pages"></a>Informazioni sulle pagine di Layout

Molti siti Web con contenuto che viene visualizzato in ogni pagina, come un'intestazione e piè di pagina o una finestra che indica gli utenti che si è connessi. ASP.NET consente di creare un file separato con un blocco di contenuto che può contenere testo, markup e codice, proprio come una normale pagina web. È quindi possibile inserire il blocco di contenuto in altre pagine nel sito in cui si desidera visualizzare le informazioni. In questo modo che non è necessario copiare e incollare lo stesso contenuto in ogni pagina. Creazione di contenuto comune simile anche rende più semplice aggiornare il sito. Se è necessario modificare il contenuto, è possibile aggiornare solo un singolo file e le modifiche vengono quindi applicate ovunque è stato inserito il contenuto.

Il diagramma seguente mostra come contenuto Blocca lavoro. Quando un browser richiede una pagina dal server web, ASP.NET inserisce i blocchi di contenuto nel punto in cui il `RenderPage` viene chiamato nella pagina principale. La pagina (unita) completata viene quindi inviata al browser.

![Diagramma concettuale che mostra come il metodo RenderPage inserisce una pagina di cui viene fatto riferimento nella pagina corrente.](3-creating-a-consistent-look/_static/image1.jpg)

In questa procedura si creerà una pagina che fa riferimento a due blocchi di contenuto (un'intestazione e piè di pagina) che si trovano in file separati. È possibile usare questi stessi blocchi di contenuto in qualsiasi pagina nel sito. Al termine, si otterrà una pagina simile alla seguente:

![Screenshot che mostra una pagina nel browser risultante dall'esecuzione di una pagina che include le chiamate al metodo RenderPage.](3-creating-a-consistent-look/_static/image2.jpg)

1. Nella cartella radice del sito Web, creare un file denominato *index. cshtml*.
2. Sostituire il markup esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Nella cartella radice, creare una cartella denominata *condiviso*.

    > [!NOTE]
    > È pratica comune per archiviare i file che vengono condivisi tra pagine web in una cartella denominata *condiviso*.
4. Nel *Shared* cartella, creare un file denominato  *\_Header.cshtml*.
5. Sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Si noti che è il nome del file  *\_Header.cshtml*, con un carattere di sottolineatura (\_) come prefisso. ASP.NET non invierà una pagina nel browser se il nome inizia con un carattere di sottolineatura. Onde evitare che gli utenti richiedano (inavvertitamente o in caso contrario) queste pagine direttamente. È consigliabile usare un carattere di sottolineatura alle pagine di nome dotati di blocchi di contenuto, perché non è realmente gli utenti possano richiedere queste pagine &#8212; esiste esclusivamente per essere inserita in altre pagine.
6. Nel *Shared* cartella, creare un file denominato  *\_Footer.cshtml* e sostituire il contenuto con quanto segue:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. Nel *index. cshtml* pagina, aggiungere due chiamate al `RenderPage` metodo, come illustrato di seguito:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Viene illustrato come inserire un blocco di contenuto in una pagina web. Si chiama il `RenderPage` metodo e passa il nome del file di cui si desidera inserire in quel punto il contenuto. In questo caso, si sta inserendo il contenuto del  *\_Header.cshtml* e  *\_Footer.cshtml* di file nei *index. cshtml* file.
8. Eseguire la *index. cshtml* pagina in un browser. (In WebMatrix, nelle **file** dell'area di lavoro, il pulsante destro e quindi selezionare **Avvia nel browser**.)
9. Nel browser, visualizzare l'origine della pagina. (Ad esempio, in Internet Explorer, fare doppio clic su pagina e quindi fare clic su **Visualizza origine**.)

    Ciò consente di visualizzare il markup della pagina web che viene inviato al browser, che combina il markup della pagina index con i blocchi di contenuto. L'esempio seguente illustra l'origine della pagina che viene eseguito il rendering per *index. cshtml*. Le chiamate a `RenderPage` inseriti *index. cshtml* sono stati sostituiti con il contenuto effettivo dei file di intestazione e piè di pagina.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Creazione di un aspetto coerente Usando le pagine di Layout

Finora si è visto che è facile includere lo stesso contenuto in più pagine. Un approccio più strutturato per la creazione di un aspetto coerente per un sito consiste nell'usare le pagine di layout. Una pagina di layout definisce la struttura di una pagina web, ma non contiene alcun contenuto effettivo. Dopo aver creato una pagina di layout, è possibile creare pagine web che contengono il contenuto e quindi collegarli alla pagina di layout. Quando queste pagine vengono visualizzate, essi verranno formattati in base alla pagina di layout. (In questo senso, una pagina di layout agisce come un tipo di modello per il contenuto definito in altre pagine.)

Pagina di layout è simile a qualsiasi pagina HTML, ad eccezione del fatto che contiene una chiamata al `RenderBody` (metodo). La posizione del `RenderBody` metodo nella pagina di layout determina in cui le informazioni dalla pagina di contenuto verrà incluse.

Il diagramma seguente mostra le pagine come contenuto e pagine di layout vengono combinate in fase di esecuzione per produrre la pagina web completata. Il browser richiede una pagina di contenuto. Nella pagina di contenuto contiene codice che specifica la pagina di layout da usare per la struttura della pagina. Nella pagina di layout, viene inserito il contenuto nel punto in cui il `RenderBody` viene chiamato il metodo. Contenuto di blocchi possono anche essere inseriti nella pagina layout chiamando il `RenderPage` metodo, il modo in cui è stato fatto nella sezione precedente. Una volta completata la pagina web, viene inviato al browser.

![Screenshot che mostra una pagina nel browser risultante dall'esecuzione di una pagina che include le chiamate al metodo RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

La procedura seguente viene illustrato come creare un layout di pagine di contenuto di pagina e un collegamento a esso.

1. Nel *Shared* cartella del sito Web, creare un file denominato  *\_Layout1.cshtml*.
2. Sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Si utilizza il `RenderPage` metodo in una pagina di layout per inserire i blocchi di contenuto. Una pagina di layout può contenere solo una chiamata al `RenderBody` (metodo).
3. Nel *Shared* cartella, creare un file denominato  *\_Header2.cshtml* e sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Nella cartella radice, creare una nuova cartella e denominarlo *stili*.
5. Nel *stili* cartella, creare un file denominato *CSS* e aggiungere le definizioni di stile seguente:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Queste definizioni di stile sono qui solo per mostrare come i fogli di stile utilizzabili con pagine di layout. Se si desidera, è possibile definire gli stili personalizzati per questi elementi.
6. Nella cartella radice, creare un file denominato *Content1.cshtml* e sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Si tratta di una pagina che verrà utilizzata una pagina di layout. Il blocco di codice nella parte superiore della pagina indica quale pagina di layout da usare per formattare questo contenuto.
7. Eseguire *Content1.cshtml* in un browser. La pagina sottoposta a rendering utilizza il formato e foglio di stile definiti in  *\_Layout1.cshtml* e il testo (contenuto) definito nella *Content1.cshtml*.

    ![[immagine]](3-creating-a-consistent-look/_static/image4.jpg)

    È possibile ripetere il passaggio 6 per creare pagine di contenuto aggiuntive che possono quindi condividere la stessa pagina di layout.

    > [!NOTE]
    > È possibile impostare il sito in modo che è possibile usare la stessa pagina di layout automaticamente per tutte le pagine di contenuto in una cartella. Per informazioni dettagliate, vedere [personalizzazione del comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Progettazione di pagine di Layout che hanno più sezioni di contenuto

Una pagina di contenuto può avere più sezioni, che risulta utile se si desidera usare i layout sono presenti più aree con contenuto sostituibile. Nella pagina di contenuto, assegnare ogni sezione con un nome univoco. (La sezione predefinita viene lasciata senza nome.) Nella pagina di layout, aggiungere un `RenderBody` metodo per specificare dove dovrebbe essere visualizzata la sezione senza nome (impostazione predefinita). È quindi possibile aggiungere separato `RenderSection` metodi per eseguire il rendering di sezioni denominate singolarmente.

Il diagramma seguente mostra come ASP.NET gestisce il contenuto che è suddivisa in più sezioni. Ogni sezione denominata è contenuta in un blocco di sezione nella pagina di contenuto. (Sono denominati `Header` e `List` nell'esempio.) Il framework inserisce sezione contenuto nella pagina di layout nel punto in cui il `RenderSection` viene chiamato il metodo. La sezione senza nome (impostazione predefinita) viene inserita in corrispondenza del punto in cui il `RenderBody` viene chiamato il metodo come illustrato in precedenza.

![Diagramma concettuale che mostra come il metodo RenderSection inserisce riferimenti sezioni nella pagina corrente.](3-creating-a-consistent-look/_static/image5.jpg)

Questa procedura viene illustrato come creare una pagina di contenuto che include più sezioni di contenuto e come eseguire il rendering usando una pagina di layout che supporta più sezioni di contenuto.

1. Nel *Shared* cartella, creare un file denominato  *\_Layout2.cshtml*.
2. Sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Si utilizza il `RenderSection` metodo per eseguire il rendering sia l'intestazione e un elenco di sezioni.
3. Nella cartella radice, creare un file denominato *Content2.cshtml* e sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Questa pagina di contenuto contiene un blocco di codice nella parte superiore della pagina. Ogni sezione denominata è contenuta in un blocco di sezione. Il resto della pagina contiene la sezione di contenuto predefinito (senza nome).
4. Eseguire *Content2.cshtml* in un browser.

    ![Screenshot che mostra una pagina nel browser risultante dall'esecuzione di una pagina che include le chiamate al metodo RenderSection.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>Rendendo facoltativo sezioni di contenuto

Le sezioni creati in una pagina di contenuto sono in genere in modo che corrispondano sezioni definite nella pagina di layout. È possibile ottenere errori se si verifica una delle operazioni seguenti:

- Nella pagina di contenuto contiene una sezione che non dispone di alcuna sezione corrispondente nella pagina di layout.
- Pagina di layout contiene una sezione per cui non è disponibile alcun contenuto.
- La pagina di layout include chiamate al metodo che tenta di eseguire il rendering più volte la stessa sezione.

Tuttavia, è possibile eseguire l'override di questo comportamento per una sezione denominata dichiarando la sezione come facoltative nella pagina di layout. È possibile definire più pagine di contenuto che possono condividere una pagina di layout, ma che possono o non può includere contenuto per una sezione specifica.

1. Aprire *Content2.cshtml* e rimuovere la sezione seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Salvare la pagina e quindi eseguirlo in un browser. Viene visualizzato un messaggio di errore, perché la pagina di contenuto non include il contenuto per una sezione definita nella pagina di layout, vale a dire la sezione di intestazione.

    ![Screenshot che mostra l'errore che si verifica se si esegue una pagina che chiama il metodo RenderSection, ma la sezione corrispondente non viene specificata.](3-creating-a-consistent-look/_static/image7.jpg)
3. Nel *Shared* cartella, aprire il  *\_Layout2.cshtml* pagina e sostituire questa riga:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    con il codice seguente:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    In alternativa, è Impossibile sostituire la riga di codice precedente con il blocco di codice seguente, che produce gli stessi risultati:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Eseguire la *Content2.cshtml* pagina in un browser nuovamente. (Se è ancora aperto in questa pagina nel browser, è possibile solo aggiornarlo.) Questa volta verrà visualizzata la pagina senza errori, anche se non dispone di alcuna intestazione.

## <a name="passing-data-to-layout-pages"></a>Passaggio di dati a pagine di Layout

È possibile usare i dati definiti nella pagina di contenuto che è necessario fare riferimento a una pagina di layout. In questo caso, è necessario passare i dati dalla pagina di contenuto alla pagina di layout. Ad esempio, si potrebbe voler visualizzare lo stato di accesso di un utente oppure si potrebbe voler mostrare o nascondere le aree del contenuto in base all'input utente.

Per passare dati da una pagina di contenuto a una pagina di layout, è possibile inserire valori nel `PageData` proprietà della pagina contenuto. Il `PageData` proprietà è una raccolta di coppie nome/valore che contengono i dati che si desidera passare tra le pagine. Nella pagina di layout, è quindi possibile leggere i valori fuori il `PageData` proprietà.

Ecco un altro diagramma. Qui viene illustrato il modo in cui è possibile usare ASP.NET il `PageData` proprietà per passare i valori da una pagina di contenuto alla pagina di layout. Quando ASP.NET inizia la creazione della pagina web, viene creato il `PageData` raccolta. Nella pagina di contenuto, si scrive codice per inserire dati nel `PageData` raccolta. I valori di `PageData` raccolta sono accessibili anche da altre sezioni nella pagina di contenuto o da altri blocchi di contenuto.

![Diagramma concettuale che mostra come una pagina di contenuto può popolare un dizionario PageData e passare tali informazioni per la pagina di layout.](3-creating-a-consistent-look/_static/image8.jpg)

La procedura seguente viene illustrato come passare i dati da una pagina di contenuto a una pagina di layout. Quando la pagina viene eseguita, viene visualizzato un pulsante che consente all'utente di nascondere o mostrare un elenco che viene definito nella pagina di layout. Quando gli utenti di fare clic sul pulsante, viene impostato un valore true o false (booleana) `PageData` proprietà. Pagina di layout legge tale valore e se è false, vengono nascosti nell'elenco. Il valore viene utilizzato anche nella pagina di contenuto per determinare se visualizzare il **Nascondi elenco** pulsante o il **Mostra elenco** pulsante.

![[immagine]](3-creating-a-consistent-look/_static/image9.jpg)

1. Nella cartella radice, creare un file denominato *Content3.cshtml* e sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Il codice archivia due tipi di dati nel `PageData` proprietà &#8212; il titolo della pagina web e true o false per specificare se visualizzare un elenco.

    Si noti che ASP.NET consente di inserire codice HTML nella pagina utilizzando in modo condizionale un blocco di codice. Ad esempio, il `if/else` blocco nel corpo della pagina determina quale forma da visualizzare a seconda che `PageData["ShowList"]` è impostato su true.
2. Nel *Shared* cartella, creare un file denominato  *\_Layout3.cshtml* e sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Pagina di layout includa un'espressione nel `<title>` elemento che ottiene il valore del titolo dal `PageData` proprietà. Viene inoltre utilizzata la `ShowList` pari al `PageData` proprietà per determinare se visualizzare il blocco di contenuto di elenco.
3. Nel *Shared* cartella, creare un file denominato  *\_List.cshtml* e sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Eseguire la *Content3.cshtml* pagina in un browser. La pagina viene visualizzata con l'elenco visibile sul lato sinistro della pagina e un **Nascondi elenco** nella parte inferiore.

    ![Screenshot che mostra la pagina che include l'elenco e un pulsante con la dicitura "Nascondi elenco".](3-creating-a-consistent-look/_static/image10.jpg)
5. Fare clic su **Nascondi elenco**. L'elenco viene rimosso e il pulsante diventa **Mostra elenco**.

    ![Screenshot che mostra la pagina che non include l'elenco e un pulsante con la dicitura 'Mostra elenco'.](3-creating-a-consistent-look/_static/image11.jpg)
6. Scegliere il **Mostra elenco** pulsante e l'elenco viene visualizzata nuovamente.

## <a name="additional-resources"></a>Risorse aggiuntive


[Personalizzazione del comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
