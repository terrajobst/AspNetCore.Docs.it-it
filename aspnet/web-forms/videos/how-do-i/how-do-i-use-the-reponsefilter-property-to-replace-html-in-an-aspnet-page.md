---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Eseguire la ricerca per categorie:] Utilizzare la proprietà Reponse.Filter per sostituire HTML in una pagina ASP.NET | Documenti Microsoft'
author: rick-anderson
description: In questo video di Chris Pels viene illustrato come utilizzare la proprietà Reponse.Filter per intercettare e modificare il codice HTML inviati a una pagina. Una pagina di esempio viene innanzitutto creata w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525530"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Eseguire la ricerca per categorie:] Utilizzare la proprietà Reponse.Filter per sostituire HTML in una pagina ASP.NET
====================
da [Chris PEL](https://twitter.com/chrispels)

In questo video di Chris Pels viene illustrato come utilizzare la proprietà Reponse.Filter per intercettare e modificare il codice HTML inviati a una pagina. In primo luogo, una pagina di esempio viene creata con un testo semplice. Quindi, viene creata una classe di flusso personalizzata che serve come flusso di sostituzione per il flusso corrente viene inviato al browser dell'utente. In tale classe di flusso personalizzato il contenuto della pagina viene recuperato dal flusso, modificato e quindi vengono scritti nel flusso di risposta. In questa classe di flusso personalizzata al metodo Write è stato personalizzato per sostituire il codice HTML nel flusso di risposta di base, in tal modo la modifica viene inviato ai browser dell'utente. Infine, la nuova classe di flusso viene assegnata per la proprietà Response nella pagina\_evento di caricamento, in tal modo, che fornisce il meccanismo per la modifica del contenuto della pagina.

[&#9654; Guardare video (13 minuti)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
