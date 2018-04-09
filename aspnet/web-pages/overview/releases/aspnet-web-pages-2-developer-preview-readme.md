---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: File Leggimi di ASP.NET Web Pages 2 Developer Preview | Documenti Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 1a43b2b12af9cd223d8a3622239743f7c431f617
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>File Leggimi di ASP.NET Web Pages 2 Developer Preview
====================
by [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>File Leggimi di ASP.NET Web Pages 2 Developer Preview

14 settembre 2011

### <a name="contents"></a>Sommario

#### <a id="_Toc303701284"></a>  Note sull'installazione

Per installare le pagine Web 2 Developer Preview, si dispone di queste opzioni:

- Installa WebMatrix 2 Beta utilizzando il [installazione guidata piattaforma Web](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix è un set di strumenti di sviluppo web gratuito che include pagine Web ASP.NET. Per ulteriori informazioni, vedere la sezione installazione in [la parte superiore funzionalità in anteprima sviluppatori di ASP.NET Web Pages 2](https://go.microsoft.com/fwlink/?LinkID=227824).

- Installare Web Pages 2 Developer Preview utilizzando direttamente la [collegamento per il download](https://go.microsoft.com/fwlink/?LinkID=226335). Utilizzare questo approccio se si desidera creare applicazioni di pagine Web con un testo editor, ad esempio Blocco note. Per eseguire applicazioni Web Pages 2, è necessario disporre di IIS 7.5 Express. (Questo è incluso automaticamente con WebMatrix). Per suggerimenti su come testare una pagina di pagine Web con IIS Express, vedere "Creazione e test ASP.NET pagine utilizzando il proprio Editor di testo" barra laterale in [Introduzione a WebMatrix e ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).

ASP.NET Web Pages 2 Developer Preview può essere installato e può eseguire side-by-side con ASP.NET Web Pages 1. <a id="a"></a>Per informazioni dettagliate, vedere la sezione "Esecuzione delle pagine Web applicazioni Side-by-Side" in [la parte superiore funzionalità in anteprima sviluppatori di Web Pages 2](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Documentazione

Esercitazioni e altre informazioni sulle pagine Web ASP.NET sono disponibili nella pagina del sito Web ASP.NET Web Pages ([https://www.asp.net/web-pages/](../../index.md)). Per informazioni sulle nuove caratteristiche e miglioramenti in 2 pagine Web, vedere [la parte superiore funzionalità in anteprima sviluppatori di Web Pages 2](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Supporto

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> Questa è una versione di anteprima e non è ufficialmente supportata. Se si hanno domande sull'utilizzo di questa versione, pubblicare un post nel forum di ASP.NET Web Pages ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), in cui i membri della community ASP.NET sono spesso in grado di fornire supporto informale.

#### <a id="_Toc303701287"></a>  Requisiti software

ASP.NET Web Pages 2 richiede .NET Framework 4. Funziona anche con la versione di anteprima per sviluppatori di .NET Framework 4.5.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Correzioni di problemi noti e modifiche di rilievo

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **È\* metodi (ad esempio, IsDateTime) ora corretto i valori restituiti per tutte le impostazioni cultura.** Alcuni metodi come *IsDateTime* restituiva *false* quando deve avere restituito *true* perché in precedenza si sono eseguendo controlli specifici delle impostazioni cultura. Questi metodi sono stati corretti per considerare le impostazioni cultura ora. Si tratta di una modifica di rilievo; Se l'applicazione si basa sul comportamento precedente, verrà interrotto.
- **È stato modificato il comportamento del metodo Href.** In precedenza, la chiamata Href("~/SomeFile") restituirebbe un URL relativo al file attualmente in esecuzione. Ora Href("~/SomeFile") restituisce sempre un percorso assoluto dalla radice dell'applicazione. Per la maggior parte dei casi, questo comportamento non creare una differenza nel valore restituito. Questa modifica è stata apportata per risolvere determinati scenari Ajax. Ad esempio, si consideri l'esempio di codice seguente: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Questo codice in precedenza viene risolto in Images/Logo.jpg, che non saranno corretti per una richiesta Ajax a tale pagina. Ora risolverà alla radice di (o MySite/Images/Logo.jpg).
- **È stato modificato il metodo HttpContext.RedirectLocal**. Questo metodo accetta ora solo gli URL relativo all'applicazione corrente. Nome completo gli URL vengono rifiutati.
- **Il metodo ModelState.IsValid richiede ora è possibile chiamare Validate prima**. Se si sta convertendo l'applicazione per utilizzare i nuovi metodi di convalida dell'input e chiama il *ModelState.IsValid* (metodo), è ora necessario chiamare *Validation.Validate* in anticipo. Ad esempio, è ora devono seguire questo modello: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  Tuttavia, è consigliabile che se si utilizzano i nuovi metodi di convalida dell'input, non utilizzare *ModelState.IsValid*. In alternativa, strutturare il codice simile al seguente: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **In Internet Explorer 7 e Internet Explorer 8, la convalida lato client non funziona**. La convalida lato client non funziona a causa di incompatibilità con jQuery 1.6.2, incluso con il modello di progetto predefinito. (La convalida sul lato server funziona.).

#### <a id="_Toc303701289"></a>  Disclaimer

© 2011 Microsoft Corporation. Tutti i diritti sono riservati. Questo documento viene fornito "come-viene." Informazioni e le opinioni espresse nel presente documento, inclusi URL e altri riferimenti a siti Web possono cambiare senza preavviso. L'utente le utilizza a proprio rischio.
