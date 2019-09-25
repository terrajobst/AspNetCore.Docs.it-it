---
title: Helper tag di script in ASP.NET Core
author: rick-anderson
ms.author: riande
description: Individuare gli attributi degli helper tag di script ASP.NET Core e il ruolo di ciascun attributo per estendere il comportamento del tag di script HTML.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256498"
---
# <a name="script-tag-helper-in-aspnet-core"></a>Helper tag di script in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

L' [Helper tag script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) genera un collegamento a un file di script primario o di fallback. In genere il file di script primario si trova in una rete per la [distribuzione di contenuti](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

L'helper tag script consente di specificare una rete CDN per il file di script e un fallback quando la rete CDN non è disponibile. L'helper tag script offre il vantaggio in merito alle prestazioni di una rete CDN con l'affidabilità dell'hosting locale.

Il markup Razor seguente mostra l' `script` elemento di un file di layout creato con il modello di app Web di ASP.NET Core:

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

L'esempio seguente è simile al codice HTML sottoposto a rendering del codice precedente (in un ambiente non di sviluppo):

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

Nel codice precedente, l'helper tag di script ha generato il secondo elemento script `<script>  (window.jQuery || document.write(`(), che `window.jQuery`verifica. Se `window.jQuery` non viene trovato, `document.write(` esegue e crea uno script 

## <a name="commonly-used-script-tag-helper-attributes"></a>Attributi di helper tag di script di uso comune

Vedere [script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) per tutti gli attributi, le proprietà e i metodi dell'helper tag di script.

### <a name="href"></a>href

Indirizzo preferenziale della risorsa collegata. All'indirizzo viene passato un pensiero al codice HTML generato in tutti i casi.

### <a name="asp-fallback-href"></a>ASP-fallback-href

URL di un foglio di stile CSS di cui eseguire il fallback in caso di errore dell'URL primario.

### <a name="asp-fallback-test-class"></a>ASP-fallback-test-Class

Nome della classe definito nel foglio di stile da utilizzare per il test di fallback. Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.

### <a name="asp-fallback-test-property"></a>ASP-fallback-test-proprietà

Nome della proprietà CSS da utilizzare per il test di fallback. Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.

### <a name="asp-fallback-test-value"></a>ASP-fallback-test-value

Valore della proprietà CSS da utilizzare per il test di fallback. Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.

### <a name="asp-fallback-test-value"></a>ASP-fallback-test-value

Valore della proprietà CSS da utilizzare per il test di fallback. Per altre informazioni, vedere <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
