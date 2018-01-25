---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Il rendering di ASP.NET Web Pages siti (Razor) per i dispositivi mobili | Documenti Microsoft
author: tfitzmac
description: "In questo articolo viene descritto come creare pagine in un sito di pagine Web ASP.NET (Razor) che verrà eseguito il rendering in modo appropriato nei dispositivi mobili. Si apprenderà: come è..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 899bbdef82d689be81cd77ea6805e0484fb614aa
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Il rendering di siti (Razor) le pagine Web ASP.NET per dispositivi mobili
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene descritto come creare pagine in un sito di pagine Web ASP.NET (Razor) che verrà eseguito il rendering in modo appropriato nei dispositivi mobili.
> 
> Illustra quanto segue:
> 
> - Come utilizzare una convenzione di denominazione per specificare che una pagina è progettato specificamente per i dispositivi mobili.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


Le pagine Web ASP.NET consente di creare visualizzazioni personalizzate per il rendering del contenuto su dispositivi mobili o altri dispositivi.

Il modo più semplice per creare la pagina specifico del dispositivo in un sito ASP.NET Web Pages è utilizzando un modello di denominazione dei file simile al seguente: *FileName. **Mobile**. cshtml*. È possibile creare due versioni di una pagina (ad esempio, uno denominato *MyFile.cshtml* e uno denominato *MyFile.Mobile.cshtml*). In fase di esecuzione, quando un dispositivo mobile richiede *MyFile.cshtml*, ASP.NET esegue il rendering del contenuto dalla *MyFile.Mobile.cshtml*. In caso contrario, *MyFile.cshtml* viene eseguito il rendering.

Nell'esempio seguente viene illustrato come attivare il rendering di dispositivi mobili mediante l'aggiunta di una pagina contenuto per i dispositivi mobili. *Page1.cshtml* contiene contenuto più di una barra laterale di navigazione. *Page1.Mobile.cshtml* contiene lo stesso contenuto, ma omette l'intestazione laterale.

1. In un sito ASP.NET Web Pages, creare un file denominato *Page1.cshtml* e sostituire il contenuto corrente con il markup seguente.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Creare un file denominato *Page1.Mobile.cshtml* e sostituire il contenuto esistente con il markup seguente. Si noti che la versione per dispositivi mobili della pagina omette la sezione di spostamento per una migliore riproduzione su uno schermo piccolo.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Eseguire un browser per desktop e passare a *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Eseguire un browser per dispositivi mobili (o un emulatore di dispositivo mobile) e passare a *Page1.cshtml*. (Si noti che non si include *.mobile.* come parte dell'URL.) Anche se la richiesta è su *Page1.cshtml*, ASP.NET esegue il rendering *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Per testare pagine per dispositivi mobili, è possibile utilizzare un simulatore di dispositivi mobili che viene eseguito in un computer desktop. Questo strumento consente di testare le pagine web come appaiono nei dispositivi mobili (ovvero, in genere con molto piccola Visualizza area). È un esempio di un simulatore di [componente aggiuntivo di selezione dell'agente utente](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) per Mozilla Firefox, che consente di emulare diversi browser per dispositivi mobili da una versione desktop di Firefox.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Emulatore Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
