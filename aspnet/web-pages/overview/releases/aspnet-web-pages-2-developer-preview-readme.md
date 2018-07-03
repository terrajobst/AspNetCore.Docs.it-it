---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: File Leggimi di ASP.NET Web Pages 2 Developer Preview | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 914e99491908294cea1e04dd23ccdb66c1dc05a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364430"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>File Leggimi di ASP.NET Web Pages 2 Developer Preview
====================
da [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>File Leggimi di ASP.NET Web Pages 2 Developer Preview

14 settembre 2011

### <a name="contents"></a>Sommario

#### <a id="_Toc303701284"></a>  Note sull'installazione

Per installare Web Pages 2 Developer Preview, sono disponibili queste opzioni:

- Installa WebMatrix 2 Beta utilizzando la [instalace Webové Platformy](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix è un set di strumenti di sviluppo web gratuito che include le pagine Web ASP.NET. Per altre informazioni, vedere la sezione installazione nel [le funzionalità migliori in ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

- Installare Web Pages 2 Developer Preview direttamente usando il [collegamento di download](https://go.microsoft.com/fwlink/?LinkID=226335). Usare questo approccio se si desidera creare applicazioni pagine Web con un testo dell'editor, ad esempio Blocco note. Per eseguire le applicazioni Web Pages 2, è necessario disporre di IIS 7.5 Express. (Questo è incluso automaticamente con WebMatrix). Per suggerimenti su come testare una pagina Web Pages utilizza IIS Express, vedere l'intestazione laterale "Creazione e test ASP.NET le pagine usando Your proprio Editor di testo" nella [Introduzione a WebMatrix e ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).

Pagine Web ASP.NET 2 Developer Preview può essere installato e può eseguire side-by-side con ASP.NET Web Pages 1. <a id="a"></a>Per informazioni dettagliate, vedere la sezione "Esecuzione pagine Web applicazioni Side-by-Side" in [le funzionalità migliori in Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Documentazione

Esercitazioni e altre informazioni su ASP.NET Web Pages sono disponibili nella pagina del sito Web ASP.NET Web Pages ([https://www.asp.net/web-pages/](../../index.md)). Per informazioni sulle nuove funzionalità e miglioramenti in Web Pages 2, vedere [le funzionalità migliori in Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Supporto

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> Questa è una versione di anteprima e non è ufficialmente supportata. Se hai domande sull'uso di questa versione, pubblicare un post nel forum ASP.NET Web Pages ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), in cui i membri della community di ASP.NET sono spesso in grado di fornire supporto informale.

#### <a id="_Toc303701287"></a>  Requisiti software

ASP.NET Web Pages 2 richiede .NET Framework 4. Funziona anche con la versione di anteprima per sviluppatori di .NET Framework 4.5.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Problemi noti, correzioni e modifiche di rilievo

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **È\* metodi (ad esempio, IsDateTime) ora corretto i valori restituiti per tutte le impostazioni cultura.** Alcuni metodi come *IsDateTime* precedentemente restituito *false* quando essi sono tornati *true* perché in precedenza si sono eseguendo verifiche delle impostazioni cultura specifiche. Questi metodi sono stati corretti per conto delle impostazioni cultura a questo punto. Si tratta di una modifica sostanziale; Se l'applicazione si basa sul comportamento obsoleto, verrà interrotto.
- **È stato modificato il comportamento del metodo Href.** In precedenza, la chiamata a Href("~/SomeFile") restituirebbe un URL relativo al file attualmente in esecuzione. A questo punto Href("~/SomeFile") restituisce sempre un percorso assoluto dalla radice dell'applicazione. Per la maggior parte dei casi, questo comportamento non essenziali nel valore restituito. Questa modifica è stata apportata per risolvere determinati scenari Ajax. Ad esempio, si consideri l'esempio di codice seguente: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Questo codice in precedenza si collegava al Images/Logo.jpg, che potrebbe essere corretto per una richiesta Ajax a tale pagina. A questo punto verrà risolto nella radice del (/ MySite/Images/Logo.jpg).
- **È stato modificato il metodo HttpContext.RedirectLocal**. Ora, questo metodo accetta solo gli URL relativi all'applicazione corrente. Gli URL completamente qualificato vengono rifiutati.
- **Il metodo ModelState richiede ora è possibile chiamare innanzitutto Validate**. Se si sta convertendo l'applicazione per usare i nuovi metodi di convalida dell'input e chiama il *ModelState* metodo, è ora necessario chiamare *Validation.Validate* in anticipo. Ad esempio, è ora necessario seguire questo modello: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  Tuttavia, è consigliabile che se si utilizzano i nuovi metodi di convalida dell'input, non utilizzare *ModelState*. Al contrario, strutturare il codice simile al seguente: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **In Internet Explorer 7 e Internet Explorer 8, la convalida lato client non funziona**. La convalida lato client non funziona a causa di incompatibilità con jQuery 1.6.2, incluso con il modello di progetto predefinito. (La convalida sul lato server funziona.).

#### <a id="_Toc303701289"></a>  Disclaimer

© 2011 Microsoft Corporation. Tutti i diritti sono riservati. Questo documento viene fornito "come-viene." Informazioni e le opinioni espresse nel presente documento, inclusi URL e altri riferimenti a siti Web Internet, possono cambiare senza preavviso. L'utente le utilizza a proprio rischio.
