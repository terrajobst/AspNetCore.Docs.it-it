---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: Utilizzo della classe TagBuilder per compilare l'helper HTML (VB) | Documenti Microsoft
author: StephenWalther
description: "Stephen Walther presenta una classe di utilità nel framework di MVC ASP.NET denominato la classe TagBuilder. È possibile utilizzare facilmente la classe TagBuilder per..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 8d0b3665e9bac6856a3fe1b50b05215f2747e354
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a>Utilizzo della classe TagBuilder per compilare l'helper HTML (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther presenta una classe di utilità nel framework di MVC ASP.NET denominato la classe TagBuilder. È possibile utilizzare la classe TagBuilder per compilare con facilità i tag HTML.


Il framework di MVC ASP.NET include una classe di utilità denominata la classe TagBuilder che è possibile utilizzare quando si compila l'helper HTML. La classe TagBuilder, come suggerisce il nome della classe, consente di compilare con facilità i tag HTML. In questa breve esercitazione verranno fornite una panoramica della classe TagBuilder e illustrato come utilizzare questa classe quando la compilazione di un helper HTML semplice che esegue il rendering HTML &lt;img&gt; tag.

## <a name="overview-of-the-tagbuilder-class"></a>Panoramica della classe TagBuilder

La classe TagBuilder è contenuta nello spazio dei nomi System. Contiene i cinque metodi:

- AddCssClass(): consente di aggiungere un nuovo *classe = ""* attributo a un tag.
- GenerateId(): consente di aggiungere un attributo id a un tag. Questo metodo sostituisce automaticamente i punti nell'id (per impostazione predefinita, punti vengono sostituiti da caratteri di sottolineatura)
- MergeAttribute(): consente di aggiungere attributi a un tag. Sono disponibili più overload di questo metodo.
- SetInnerText(): consente di impostare il testo interno del tag. Il testo interno è HTML codifica automaticamente.
- ToString (): consente di eseguire il rendering del tag. È possibile specificare se si desidera creare un tag normale, un tag di inizio, un tag di fine o un tag di chiusura automatica.
  

La classe TagBuilder ha quattro proprietà importanti:

- Attributi: rappresenta tutti gli attributi del tag.
- IdAttributeDotReplacement – rappresenta il carattere utilizzato dal metodo GenerateId() per sostituire i (il valore predefinito è un carattere di sottolineatura).
- InnerHTML: rappresenta il contenuto interno del tag. Assegnare una stringa per questa proprietà *non* HTML codificare la stringa.
- TagName: rappresenta il nome del tag.

Questi metodi e proprietà consentono di tutti i metodi di base e proprietà che è necessario creare un tag HTML. Non è effettivamente necessario utilizzare la classe TagBuilder. In alternativa, è possibile utilizzare una classe StringBuilder. Tuttavia, la classe TagBuilder semplifica la vita leggermente.

## <a name="creating-an-image-html-helper"></a>Creazione di un HTML Helper immagine

Quando si crea un'istanza della classe TagBuilder, si passa il nome del tag che si desidera compilare al costruttore TagBuilder. Successivamente, è possibile chiamare i metodi, ad esempio i metodi AddCssClass e MergeAttribute() per modificare gli attributi del tag. Infine, chiamare il metodo ToString () per il rendering del tag.

Ad esempio, listato 1 contiene un helper HTML di immagine. L'helper di immagine viene implementato internamente con un TagBuilder che rappresenta un elemento HTML &lt;img&gt; tag.

**Elenco 1 – Helpers\ImageHelper.vb**

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

Il modulo nel listato 1 contiene due metodi di overload denominati Image(). Quando si chiama il metodo Image(), è possibile passare un oggetto che rappresenta un set di attributi HTML o meno.

Si noti come il metodo di TagBuilder.MergeAttribute() viene utilizzato per aggiungere singoli attributi, ad esempio l'attributo src di TagBuilder. Si noti, inoltre, come il metodo di TagBuilder.MergeAttributes() viene utilizzato per aggiungere una raccolta di attributi per il TagBuilder. Il metodo MergeAttributes() accetta un dizionario&lt;stringa, oggetto&gt; parametro. La classe di RouteValueDictionary viene usata per convertire l'oggetto che rappresenta la raccolta di attributi in un dizionario&lt;stringa, oggetto&gt;.

Dopo aver creato il supporto di immagine, è possibile utilizzare l'helper in visualizzazioni MVC ASP.NET come uno degli altri helper HTML standard. La visualizzazione nel listato 2 utilizza l'helper di immagine da visualizzare due volte la stessa immagine di Xbox (vedere la figura 1). L'helper Image() viene chiamato con e senza una raccolta di attributi HTML.

**Elenco di 2 – Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


[![La finestra di dialogo Nuovo progetto](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)

**Figura 01**: utilizzo dell'helper di immagine ([fare clic per visualizzare l'immagine ingrandita](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))


Si noti che è necessario importare lo spazio dei nomi associato all'helper immagine nella parte superiore della visualizzazione Index.aspx. L'helper viene importata con la direttiva seguente:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

In un'applicazione Visual Basic, lo spazio dei nomi predefinito è lo stesso nome dell'applicazione.

>[!div class="step-by-step"]
[Precedente](creating-custom-html-helpers-vb.md)
[Successivo](creating-page-layouts-with-view-master-pages-vb.md)
