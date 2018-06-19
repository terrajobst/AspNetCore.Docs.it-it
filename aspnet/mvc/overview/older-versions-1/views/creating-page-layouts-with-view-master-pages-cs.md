---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Creazione di layout di pagina con pagine Master di visualizzazione (c#) | Documenti Microsoft
author: microsoft
description: In questa esercitazione imparare a creare un layout di pagina comune per più pagine dell'applicazione per sfruttare i vantaggi della visualizzazione pagine master. È possibile utilizzare un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 82500a311e1110713a60604027d018ba16539b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871246"
---
<a name="creating-page-layouts-with-view-master-pages-c"></a>Creazione di layout di pagina con pagine Master di visualizzazione (c#)
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> In questa esercitazione imparare a creare un layout di pagina comune per più pagine dell'applicazione per sfruttare i vantaggi della visualizzazione pagine master. È possibile utilizzare una pagina master di visualizzazione, ad esempio, per definire un layout di pagina a due colonne e usare il layout di due colonne per tutte le pagine dell'applicazione web.


## <a name="creating-page-layouts-with-view-master-pages"></a>Creazione di layout di pagina con pagine Master di visualizzazione

In questa esercitazione imparare a creare un layout di pagina comune per più pagine dell'applicazione per sfruttare i vantaggi della visualizzazione pagine master. È possibile utilizzare una pagina master di visualizzazione, ad esempio, per definire un layout di pagina a due colonne e usare il layout di due colonne per tutte le pagine dell'applicazione web.

È possibile usufruire della vista pagine master per condividere contenuto comune su più pagine dell'applicazione. Ad esempio, è possibile inserire in una pagina master di visualizzazione del logo del sito Web, i collegamenti di navigazione e pubblicitari. In questo modo, ogni pagina nell'applicazione verrebbe visualizzato automaticamente questo contenuto.

In questa esercitazione imparare a creare una nuova pagina master di visualizzazione e creare una nuova pagina contenuto visualizzazione della pagina master.

### <a name="creating-a-view-master-page"></a>Creazione di una pagina Master di visualizzazione

Iniziamo creando una pagina master che definisce un layout a due colonne. Aggiungere una nuova pagina master visualizzazione a un progetto MVC facendo cartella Views\Shared selezionando l'opzione di menu **Aggiungi, elemento nuovo**e selezionando il **pagina Master visualizzazione MVC** modello (vedere la figura 1).


[![Aggiunta di una pagina master visualizzazione](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Figura 01**: aggiunta di una pagina master visualizzazione ([fare clic per visualizzare l'immagine ingrandita](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))


È possibile creare più di una pagina master di visualizzazione in un'applicazione. Ogni pagina master visualizzazione è possibile definire un layout di pagina diversa. Ad esempio, è necessario determinate pagine di un layout a due colonne e le altre pagine di un layout a tre colonne.

Una pagina master di visualizzazione è molto simile a una visualizzazione ASP.NET MVC standard. Tuttavia, a differenza di una visualizzazione normale, una pagina master visualizzazione contiene uno o più `<asp:ContentPlaceHolder>` tag. Il `<contentplaceholder>` tag vengono utilizzati per contrassegnare le aree della pagina master che può essere sottoposto a override in una singola pagina contenuto.

Ad esempio, la pagina master visualizzazione nel listato 1 definisce un layout a due colonne. Contiene due `<contentplaceholder>` tag. Un `<ContentPlaceHolder>` per ogni colonna.

**Elenco 1: `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Il corpo della visualizzazione pagina master nel listato 1 contiene due `<div>` tag che corrispondono alle due colonne. La classe di colonna del foglio di stile viene applicata a entrambi `<div>` tag. Questa classe è definita nel foglio di stile dichiarato nella parte superiore della pagina master. È possibile visualizzare l'anteprima come verrà visualizzata la pagina master visualizzazione passando alla visualizzazione progettazione. Fare clic sulla scheda Progettazione nella parte inferiore sinistra dell'editor del codice sorgente (vedere la figura 2).


[![Visualizzazione in anteprima una pagina master nella finestra di progettazione](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Figura 02**: la visualizzazione in anteprima una pagina master nella finestra di progettazione ([fare clic per visualizzare l'immagine ingrandita](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Creazione di una pagina di visualizzazione del contenuto

Dopo aver creato una pagina master di visualizzazione, è possibile creare vista di uno o più pagine di contenuto in base alla pagina di visualizzazione master. Ad esempio, è possibile creare una pagina contenuta visualizzazione di indice per il controller Home facendo cartella Views\Home selezionando **Aggiungi, elemento nuovo**, se si seleziona il **pagina contenuto visualizzazione MVC** modello, immettere il nome Index.aspx e fare clic su di **Aggiungi** pulsante (vedere la figura 3).


[![Aggiunta di una pagina contenuto visualizzazione](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Figura 03**: aggiunta di una pagina contenuto visualizzazione ([fare clic per visualizzare l'immagine ingrandita](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))


Dopo aver fatto clic sul pulsante Aggiungi viene visualizzata una nuova finestra di dialogo che consente di selezionare una pagina master per associare la pagina di visualizzazione del contenuto (vedere la figura 4). È possibile passare alla pagina Site. master visualizzazione master creato nella sezione precedente.


[![Selezione di una pagina master](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Figura 04**: selezione di una pagina master ([fare clic per visualizzare l'immagine ingrandita](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))


Dopo aver creato una nuova pagina contenuto visualizzazione della pagina master Site. master, ottenere il file di listato 2.

**Elenco 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Si noti che questa vista contiene un `<asp:Content>` tag che corrisponde a ogni il `<asp:ContentPlaceHolder>` tag nella pagina master. Ogni `<asp:Content>` tag include un attributo ContentPlaceHolderID che punta a particolari `<asp:ContentPlaceHolder>` sottoposto a override.

Si noti inoltre che la pagina di visualizzazione del contenuto nel listato 2 non contengano normale tag di apertura e chiusura HTML. Ad esempio, non contiene l'apertura e chiusura `<html>` o `<head>` tag. Tutti i normale tag di apertura e chiusura sono contenuti nella pagina master.

Qualsiasi contenuto che si desidera visualizzare in una pagina di visualizzazione del contenuto deve essere inserito in un `<asp:Content>` tag. Se si inserisce codice HTML o altro contenuto di fuori di questi tag, quindi si verificherà un errore quando si tenta di visualizzare la pagina.

Non è necessario eseguire l'override di ogni `<asp:ContentPlaceHolder>` tag da una pagina master in una pagina di visualizzazione del contenuto. È sufficiente eseguire l'override di un `<asp:ContentPlaceHolder>` tag quando si desidera sostituire il tag con contenuto specifico.

Ad esempio, la visualizzazione dell'indice modificata nel listato 3 contiene solo due `<asp:Content>` tag. Ogni il `<asp:Content>` tag include del testo.

**Elenco di 3: `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Quando viene richiesta la visualizzazione nel listato 3, rendering della pagina nella figura 5. Si noti che la visualizzazione esegue il rendering di una pagina con due colonne. Si noti inoltre che il contenuto pagina della visualizzazione del contenuto viene unito con il contenuto della pagina master visualizzazione


[![La pagina di indice visualizzazione contenuto](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Figura 05**: pagina contenuto visualizzazione l'indice ([fare clic per visualizzare l'immagine ingrandita](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Modifica del contenuto della pagina Master di visualizzazione

Uno dei problemi che si verifica quasi immediatamente quando si lavora con pagine master di visualizzazione è il problema di modificare il contenuto pagina master visualizzazione quando vengono richieste le pagine di contenuto visualizzazione diversa. Ad esempio, si desidera che ogni pagina nell'applicazione web per un titolo univoco. Tuttavia, il titolo viene dichiarato nella pagina master di visualizzazione e non nella visualizzazione contenuto. In tal caso, come personalizzare il titolo della pagina per ogni pagina di visualizzazione del contenuto?

Esistono due modi che è possibile modificare il titolo visualizzato da una pagina di visualizzazione del contenuto. In primo luogo, è possibile assegnare un titolo per l'attributo title del `<%@ page %>` direttiva dichiarati all'inizio di una pagina di visualizzazione del contenuto. Ad esempio, se si desidera assegnare il titolo della pagina "Sito Web grande Super" per la visualizzazione dell'indice, è possibile includere la direttiva seguente nella parte superiore della visualizzazione dell'indice:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Quando la visualizzazione dell'indice viene eseguito il rendering nel browser, viene visualizzato il titolo desiderato nella barra del titolo del browser:


[![Barra del titolo del browser](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)


È un requisito importante che una pagina di visualizzazione master deve soddisfare in modo che l'attributo title funzionare. La pagina master visualizzazione deve contenere un `<head runat="server">` tag anziché una normale `<head>` tag per l'intestazione specificata. Se il `<head>` tag non include il runat = attributo "server", quindi non sarà più visualizzato il titolo. La visualizzazione predefinita include la pagina master `<head runat="server">` tag.

È un approccio alternativo alla modifica del contenuto della pagina master da una pagina di contenuto di visualizzazione per eseguire il wrapping l'area che si desidera modificare un `<asp:ContentPlaceHolder>` tag. Si supponga, ad esempio, che si desidera modificare non solo il titolo, ma anche il tag meta, eseguito il rendering da una pagina di visualizzazione master. La pagina di visualizzazione master listato 4 contiene un `<asp:ContentPlaceHolder>` tag relativo `<head>` tag.

**Elenco di 4: `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Si noti che il `<asp:ContentPlaceHolder>` tag listato 4 include il contenuto predefinito: un titolo predefinito e il tag meta predefiniti. Se si non esegue l'override di questo `<asp:ContentPlaceHolder>` tag in una pagina di contenuto di visualizzazione, verrà visualizzato il contenuto predefinito.

La pagina di visualizzazione del contenuto nel listato 5 esegue l'override di `<asp:ContentPlaceHolder>` tag per visualizzare un titolo personalizzato e metatag personalizzati.

**Elenco 5: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Riepilogo

In questa esercitazione viene fornita un'introduzione di base per visualizzare le pagine master e pagine di contenuto. È stato descritto come creare nuova vista pagine master e visualizzazione di pagine di contenuto basati su di essi. È inoltre esaminato come è possibile modificare il contenuto di una pagina master di visualizzazione da una pagina contenuto visualizzazione specifica.

> [!div class="step-by-step"]
> [Precedente](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [Successivo](passing-data-to-view-master-pages-cs.md)
