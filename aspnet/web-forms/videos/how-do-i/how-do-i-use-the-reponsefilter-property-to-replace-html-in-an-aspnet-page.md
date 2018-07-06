---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Procedura:] Usare la proprietà Reponse. Filter per sostituire il codice HTML in una pagina ASP.NET | Microsoft Docs'
author: rick-anderson
description: In questo video Chris Pels illustra come usare la proprietà Reponse. Filter per intercettare e modificare il codice HTML inviati a una pagina. Prima di tutto una pagina di esempio viene creata w...
ms.author: aspnetcontent
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: bf1936e5710b67185977f71bea03240e6c6abf41
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808103"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Procedura:] Usare la proprietà Reponse. Filter per sostituire il codice HTML in una pagina ASP.NET
====================
da [Chris Pels](https://twitter.com/chrispels)

In questo video Chris Pels illustra come usare la proprietà Reponse. Filter per intercettare e modificare il codice HTML inviati a una pagina. Prima di tutto una pagina di esempio viene creata con un semplice testo. Quindi, viene creata una classe di Stream personalizzata che agisce come il flusso di sostituzione per il flusso corrente inviato al browser dell'utente. In tale classe di flusso personalizzata il contenuto della pagina viene recuperato dal flusso, modificato e quindi vengono scritti nel flusso di risposta. In questa classe Stream personalizzata del metodo Write viene personalizzato per sostituire il codice HTML nel flusso di risposta di base, in tal modo la modifica di ciò che viene inviato al browser dell'utente. Infine, la nuova classe di flusso viene assegnata alla proprietà Response. Filter nella pagina\_evento di caricamento, in tal modo, che fornisce il meccanismo per la modifica del contenuto della pagina.

[&#9654;Guarda il video (13 minuti)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
