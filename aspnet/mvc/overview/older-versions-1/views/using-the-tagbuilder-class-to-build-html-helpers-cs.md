---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Utilizzo della classe TagBuilder per creazione di helper HTML (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther presenta una classe di utilità vantaggiosi nel framework di MVC ASP.NET denominata della classe TagBuilder. È possibile utilizzare facilmente la classe TagBuilder per...
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9990fc7ad8093643a564a5e02ff65264d4a7fe15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813226"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>Utilizzo della classe TagBuilder per creazione di helper HTML (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther presenta una classe di utilità vantaggiosi nel framework di MVC ASP.NET denominata della classe TagBuilder. È possibile usare la classe TagBuilder per creare con facilità i tag HTML.


Il framework ASP.NET MVC include una classe di utilità denominata della classe TagBuilder che è possibile usare durante la creazione di helper HTML. Classe TagBuilder, come suggerisce il nome della classe, consente di compilare facilmente i tag HTML. In questa breve esercitazione vengono fornite una panoramica della classe TagBuilder e descrive come utilizzare questa classe quando si compila un helper HTML semplice che esegue il rendering HTML &lt;img&gt; tag.

## <a name="overview-of-the-tagbuilder-class"></a>Panoramica della classe TagBuilder

Classe TagBuilder è contenuta nello spazio dei nomi System. Dispone di cinque metodi:

- AddCssClass() - consente di aggiungere una nuova *classe = ""* attributo a un tag.
- GenerateId() - consente di aggiungere un attributo id a un tag. Questo metodo sostituisce automaticamente i punti nell'id (per impostazione predefinita, i periodi di vengono sostituiti da caratteri di sottolineatura)
- MergeAttribute() - consente di aggiungere attributi ad un tag. Sono disponibili più overload di questo metodo.
- SetInnerText() - consente di impostare il testo interno del tag. Il testo interno viene automaticamente codifica HTML.
- Metodo ToString () - consente di eseguire il rendering del tag. È possibile specificare se si desidera creare un tag normale, un tag di inizio, un tag di fine o un tag di chiusura automatica.
  

Classe TagBuilder ha quattro proprietà importanti:

- Attributi - rappresenta tutti gli attributi del tag.
- IdAttributeDotReplacement - rappresenta il carattere utilizzato dal metodo GenerateId() per sostituire i (il valore predefinito è un carattere di sottolineatura).
- InnerHTML - rappresenta il contenuto interno del tag. Assegnare una stringa per questa proprietà *non* HTML codificare la stringa.
- TagName - rappresenta il nome del tag.

Questi metodi e proprietà offrono tutti i metodi di base e proprietà che è necessario creare un tag HTML. Non è davvero necessario utilizzare la classe TagBuilder. Impossibile utilizzare una classe StringBuilder. Tuttavia, la classe TagBuilder semplifica la vita un po'.

## <a name="creating-an-image-html-helper"></a>Creazione di un HTML Helper immagine

Quando si crea un'istanza della classe TagBuilder, si passa il nome del tag che si desidera compilare al costruttore TagBuilder. Successivamente, è possibile chiamare i metodi, ad esempio i metodi AddCssClass e MergeAttribute() per modificare gli attributi del tag. Infine, chiamare il metodo ToString () per il rendering del tag.

Listato 1, ad esempio, contiene un helper HTML di immagine. L'helper di immagine viene implementato internamente con un TagBuilder che rappresenta un elemento HTML &lt;img&gt; tag.

**Listato 1 - Helpers\ImageHelper.cs**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

La classe nel listato 1 contiene due metodi di overload statici denominati immagine. Quando si chiama il metodo Image(), è possibile passare un oggetto che rappresenta un set di attributi HTML o meno.

Si noti come il metodo TagBuilder.MergeAttribute() consente di aggiungere singoli attributi, ad esempio l'attributo src per il TagBuilder. Si noti, inoltre, come il metodo TagBuilder.MergeAttributes() consente di aggiungere una raccolta di attributi per il TagBuilder. Il metodo MergeAttributes() accetta un dizionario&lt;stringa, oggetto&gt; parametro. La classe RouteValueDictionary viene usata per convertire l'oggetto che rappresenta la raccolta di attributi in un dizionario&lt;stringa, oggetto&gt;.

Dopo aver creato l'helper di immagine, è possibile usare l'helper nelle visualizzazioni ASP.NET MVC come qualsiasi degli altri helper HTML standard. La visualizzazione nel listato 2 viene usato l'helper di immagine per visualizzare due volte la stessa immagine di Xbox (vedere la figura 1). L'helper Image() viene chiamato con e senza una raccolta di attributi HTML.

**Listato 2 - Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


[![La finestra di dialogo Nuovo progetto](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**Figura 01**: usando l'helper di immagine ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))


Si noti che è necessario importare lo spazio dei nomi associato all'helper immagine nella parte superiore della visualizzazione index. aspx. L'helper viene importato con la direttiva seguente:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> [Precedente](creating-custom-html-helpers-cs.md)
> [Successivo](creating-page-layouts-with-view-master-pages-cs.md)
