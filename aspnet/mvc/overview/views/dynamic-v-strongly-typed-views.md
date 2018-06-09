---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: V dinamico. Fortemente tipizzate viste | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "26504090"
---
<a name="dynamic-v-strongly-typed-views"></a>V dinamico. Visualizzazioni fortemente tipizzate
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

Esistono tre modi per passare informazioni da un controller a una vista in ASP.NET MVC 3:

1. Come un oggetto modello fortemente tipizzato.
2. Come un tipo dinamico (con @model dinamica)
3. Utilizzando ViewBag

Ho scritto una semplice applicazione MVC 3 Top Blog per mettere a confronto le visualizzazioni fortemente tipizzate e dinamiche. Il controller viene avviata con un semplice elenco di blog:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Fare clic con il metodo IndexNotStonglyTyped() e aggiungere una visualizzazione Razor.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Verificare che il **creare una visualizzazione fortemente tipizzata** non sia selezionata. La visualizzazione risultante non contenga molte:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Poiché si sta usando un dinamico e non una visualizzazione fortemente tipizzata, non Aiutaci a intellisense. Il codice completo è illustrato di seguito:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

A questo punto verrà aggiunto una visualizzazione fortemente tipizzata. Aggiungere il codice seguente al controller:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Si noti che è esattamente la stessa View(topBlogs) restituito; chiamare della vista non fortemente tipizzato. Fare clic con il pulsante destro all'interno di *StonglyTypedIndex()* e selezionare **Aggiungi visualizzazione**. Questa volta selezionare il **Blog** classe del modello e selezionare **elenco** come il modello di scaffolding.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

All'interno del nuovo modello di visualizzazione è il supporto intellisense.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Può essere scaricato il progetto c# [qui](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
