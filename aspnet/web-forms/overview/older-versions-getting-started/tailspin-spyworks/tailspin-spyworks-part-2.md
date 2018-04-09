---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: Livello di accesso ai dati | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 2 viene illustrata l'aggiunta del livello di accesso ai dati.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f734b04a0f4cec3c33bc5b42ef283ea64cdb463
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-2-data-access-layer"></a>Parte 2: Livello di accesso ai dati
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET. Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 2 viene illustrata l'aggiunta del livello di accesso ai dati.


## <a id="_Toc260221668"></a>  Aggiunta di livello di accesso ai dati

L'applicazione di e-commerce dipenderà da due database.

Per informazioni sul cliente verrà usato il database di sistema di appartenenze ASP.NET standard. Per il nostro carrello catalogo carrello e il prodotto è un database di SQL Express viene implementato come indicato di seguito.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Dopo aver creato il database (Commerce.mdf) nell'App dell'applicazione\_cartella dati è possibile procedere per creare il livello di accesso ai dati tramite Entity Framework .NET.

Verrà creata una cartella denominata "dati\_accesso" e fare clic con il pulsante destro per tale cartella e scegliere "Aggiungi nuovo elemento".

In "Modelli installati" elemento e quindi selezionare "ADO.NET Entity Data Model" Immettere EDM\_Commerce.edmx come il nome e fare clic sul pulsante "Aggiungi".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Scegliere "Genera da Database".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Salvare e compilare.

Si è ora pronti per aggiungere la funzionalità: prima di un menu di categoria di prodotto.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-1.md)
> [Successivo](tailspin-spyworks-part-3.md)
