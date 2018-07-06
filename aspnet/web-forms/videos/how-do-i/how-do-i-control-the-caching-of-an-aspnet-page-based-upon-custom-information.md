---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Procedura:] Controllo la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate | Microsoft Docs'
author: rick-anderson
description: In questo video Chris Pels illustra come controllare i criteri per la memorizzazione nella cache una pagina ASP.NET in base alle informazioni personalizzate. Viene creata una pagina di esempio e quindi gli O....
ms.author: aspnetcontent
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: d069b7798d3659e9f6786fb8d63862817fbdd68b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808694"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a>[Procedura:] Controllo la memorizzazione nella cache di una pagina ASP.NET in base alle informazioni personalizzate
====================
da [Chris Pels](https://twitter.com/chrispels)

In questo video Chris Pels illustra come controllare i criteri per la memorizzazione nella cache una pagina ASP.NET in base alle informazioni personalizzate. Viene creata una pagina di esempio e quindi la direttiva OutputCache viene usata con il VaryByCustom (attributo) che contiene un valore personalizzato. Successivamente, il metodo GetVaryCustomByString() viene sottoposto a override nel modulo di Global. asax che fornisce la gestione dell'attributo personalizzato. In tale metodo viene restituita una stringa che identifica in modo univoco la versione memorizzata nella cache della pagina. Infine, è disponibile una discussione su come la memorizzazione nella cache usando un valore personalizzato è possibile usare in diversi modi per un sito web.

[&#9654;Guarda il video (12 minuti)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
