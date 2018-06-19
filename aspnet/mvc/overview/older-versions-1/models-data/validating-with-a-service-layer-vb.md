---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Convalida con un livello di servizio (VB) | Documenti Microsoft
author: StephenWalther
description: Informazioni su come spostare la logica di convalida le azioni del controller e in un livello di servizio separato. In questa esercitazione, Stephen Walther illustra come è...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: bb1191b663f863bf881def620efab4f2f03edc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868243"
---
<a name="validating-with-a-service-layer-vb"></a>Convalida con un livello di servizio (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Informazioni su come spostare la logica di convalida le azioni del controller e in un livello di servizio separato. In questa esercitazione, Stephen Walther viene illustrato come è possibile mantenere una separazione delle problematiche acuta isolando il livello di servizio dal livello del controller.


L'obiettivo di questa esercitazione viene descritto un metodo di eseguire la convalida in un'applicazione MVC ASP.NET. In questa esercitazione informazioni su come spostare la logica di convalida, i controller e in un livello di servizio separato.

## <a name="separating-concerns"></a>Separazione dei problemi

Quando si compila un'applicazione MVC ASP.NET, non inserire la logica del database all'interno di azioni del controller. Combinazione logica del database e controller rende più difficile da gestire nel tempo l'applicazione. Si consiglia di inserire tutta la logica di database in un livello di repository separato.

Ad esempio, listato 1 contiene un repository semplice denominato il ProductRepository. Il repository di prodotto contiene tutto il codice di accesso ai dati per l'applicazione. L'elenco include anche l'interfaccia IProductRepository che implementa il repository di prodotto.

**Elenco 1 - Models\ProductRepository.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

Il controller nel listato 2 utilizza il livello di repository in entrambe le proprietà Index () e il metodo di creazione di azioni. Si noti che il controller non contiene alcuna logica di database. Creazione di un livello di repository consente di mantenere una netta separazione delle problematiche. I controller sono responsabili della logica di controllo di flusso dell'applicazione e il repository è responsabile della logica di accesso ai dati.

**Listing 2 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a>Creazione di un livello di servizio

In tal caso, logica di controllo di flusso dell'applicazione fa parte di un controller e logica di accesso ai dati appartiene in un repository. In tal caso, in cui si gestiscono la logica di convalida? È possibile inserire la logica di convalida in un *livello di servizio*.

Un livello di servizio è un ulteriore livello di un'applicazione MVC ASP.NET che consente di eseguire la comunicazione tra un controller e il livello del repository. Il livello di servizio contiene la logica di business. In particolare, include la logica di convalida.

Ad esempio, il livello di servizio prodotto listato 3 ha un metodo CreateProduct(). Il metodo CreateProduct() chiama il metodo ValidateProduct() per convalidare un nuovo prodotto prima di passare il prodotto nel repository di prodotto.

**Listing 3 - Models\ProductService.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

Il controller di prodotto è stato aggiornato nel listato 4 per utilizzare il livello di servizio anziché al livello del repository. Il livello di controller comunica con il livello di servizio. Il livello di servizio comunica con il livello di repository. Ogni livello è una responsabilità separata.

**Listing 4 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

Si noti che il servizio di prodotto viene creato nel costruttore controller prodotto. Quando viene creato il servizio di prodotto, dizionario di stato del modello viene passato al servizio. Il servizio prodotto Usa lo stato del modello per passare i messaggi di errore di convalida al controller.

## <a name="decoupling-the-service-layer"></a>Separazione il livello di servizio

Stato non è stato possibile isolare i livelli di servizio per un aspetto e il controller. Il controller e i livelli di servizio comunicano tramite lo stato del modello. In altre parole, il livello di servizio presenta una dipendenza su una determinata funzionalità del framework di MVC ASP.NET.

Si desidera isolare il livello di servizio di livello il controller quanto possibile. In teoria, è necessario essere in grado di utilizzare il livello di servizio con qualsiasi tipo di applicazione e non solo di un'applicazione MVC ASP.NET. Ad esempio, in futuro, potrebbe essere opportuno compilare un front-end per l'applicazione WPF. Occorre individuare un modo per rimuovere la dipendenza su ASP.NET MVC lo stato del modello da questo livello di servizio.

Nel listato 5, il livello di servizio è stato aggiornato in modo che non utilizzi più lo stato del modello. Utilizza invece qualsiasi classe che implementa l'interfaccia IValidationDictionary.

**Listing 5 - Models\ProductService.vb (decoupled)**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

L'interfaccia IValidationDictionary è definito nel listato 6. Questa semplice interfaccia dispone di un singolo metodo e una singola proprietà.

**Elenco 6 - Models\IValidationDictionary.cs**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

La classe nel listato 7, la classe ModelStateWrapper, denominato implementa l'interfaccia IValidationDictionary. È possibile creare un'istanza della classe ModelStateWrapper passando un dizionario di stato del modello al costruttore.

**Elenco 7 - Models\ModelStateWrapper.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

Infine, il controller aggiornato nel listato 8 Usa il ModelStateWrapper quando si crea il livello di servizio nel relativo costruttore.

**Elenco 8 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

Utilizzando il IValidationDictionary interfaccia e la classe ModelStateWrapper consente di isolare completamente il livello di servizio di livello il controller. Il livello di servizio non è più dipendente sullo stato del modello. È possibile passare qualsiasi classe che implementa l'interfaccia IValidationDictionary a livello di servizio. Ad esempio, un'applicazione WPF potrebbe implementare l'interfaccia IValidationDictionary con una classe collection semplice.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è stata per illustrare un approccio per eseguire la convalida in un'applicazione MVC ASP.NET. In questa esercitazione è stato descritto come spostare tutta la logica di convalida, i controller e in un livello di servizio separato. È stato inoltre descritto come isolare il livello di servizio dal livello del controller tramite la creazione di una classe ModelStateWrapper.

> [!div class="step-by-step"]
> [Precedente](validating-with-the-idataerrorinfo-interface-vb.md)
> [Successivo](validation-with-the-data-annotation-validators-vb.md)
