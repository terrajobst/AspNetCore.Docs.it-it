---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Convalida con un livello di servizio (VB) | Microsoft Docs
author: StephenWalther
description: Informazioni su come spostare la logica di convalida all'esterno di azioni del controller e a un livello di servizio separato. In questa esercitazione, Stephen Walther spiega come è...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: ecce8e4f0a901ce8c185d2b085f4d706bd57fa1f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823834"
---
<a name="validating-with-a-service-layer-vb"></a>Convalida con un livello di servizio (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Informazioni su come spostare la logica di convalida all'esterno di azioni del controller e a un livello di servizio separato. In questa esercitazione, Stephen Walther spiega come è possibile gestire una separazione delle problematiche sharp isolando il livello di servizio dal livello del controller.


L'obiettivo di questa esercitazione è descrivere un metodo di esecuzione della convalida in un'applicazione ASP.NET MVC. In questa esercitazione descrive come spostare la logica di convalida del controller e a un livello di servizio separato.

## <a name="separating-concerns"></a>La separazione delle problematiche

Quando si compila un'applicazione ASP.NET MVC, è consigliabile non inserire la logica di database all'interno di azioni del controller. Combinare la logica di database e controller rende più difficile da gestire nel corso del tempo l'applicazione. Si consiglia di posizionare tutta la logica di database in un livello di repository separati.

Listato 1, ad esempio, contiene un repository semplice denominato il ProductRepository. Il repository di prodotto contiene tutto il codice di accesso di dati per l'applicazione. L'elenco include anche l'interfaccia IProductRepository che implementa il repository di prodotto.

**Listato 1 - Models\ProductRepository.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

Il controller nel listato 2 Usa il livello di repository le azioni di entrambe le proprietà Index () e create (). Si noti che questo controller non contiene alcuna logica di database. Creazione di un livello di repository consente di mantenere una netta separazione delle problematiche. I controller sono responsabili della logica di controllo di flusso dell'applicazione e il repository è responsabile della logica di accesso ai dati.

**Listato 2 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a>Creazione di un livello di servizio

Pertanto, logica di controllo di flusso dell'applicazione appartiene in un controller e logica di accesso ai dati a cui appartiene un repository. In tal caso, in cui si gestiscono la logica di convalida? Una possibilità consiste nell'inserire logica di convalida in un *livello di servizio*.

Un livello di servizio è un ulteriore livello in un'applicazione ASP.NET MVC che consente di eseguire la comunicazione tra un controller e il livello di repository. Il livello di servizio contiene la logica di business. In particolare, contiene la logica di convalida.

Ad esempio, il livello di servizio del prodotto nel listato 3 ha un metodo CreateProduct(). Il metodo CreateProduct() chiama il metodo ValidateProduct() per convalidare un nuovo prodotto prima di passare il prodotto nel repository di prodotto.

**Listato 3 - Models\ProductService.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

Il controller di prodotto è stato aggiornato nel listato 4 per utilizzare il livello di servizio anziché al livello del repository. Il livello di servizio comunica con il livello di controller. Il livello di servizio comunica con il livello di repository. Ogni livello avrà responsabilità separate.

**Listato 4 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

Si noti che il servizio di prodotto viene creato nel costruttore del controller di prodotto. Quando viene creato il servizio di prodotto, dizionario di stato del modello viene passato al servizio. Il servizio del prodotto Usa lo stato del modello per superare la convalida dei messaggi di errore al controller.

## <a name="decoupling-the-service-layer"></a>Disaccoppiamento il livello di servizio

Non è stato possibile isolare il livello di servizio per un aspetto e il controller. Il livello di servizio e il controller di comunicazione attraverso lo stato del modello. In altre parole, il livello di servizio presenta una dipendenza su una particolare funzionalità del framework ASP.NET MVC.

Si vuole isolare il livello di servizio dal nostro livello di controller quanto più possibile. In teoria, dovremmo sarà in grado di usare il livello di servizio con qualsiasi tipo di applicazione e non solo un'applicazione ASP.NET MVC. Ad esempio, in futuro, potremmo voler compilare un front-end per la nostra applicazione WPF. Occorre individuare un modo per rimuovere la dipendenza su ASP.NET MVC lo stato del modello dal nostro livello di servizio.

Nel listato 5, il livello di servizio è stato aggiornato in modo che non usi più lo stato del modello. Usa invece una classe che implementa l'interfaccia IValidationDictionary.

**Listato 5 - Models\ProductService.vb (separato)**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

L'interfaccia IValidationDictionary viene definita nel listato 6. Questa interfaccia semplice include un solo metodo e una singola proprietà.

**Listato 6 - Models\IValidationDictionary.cs**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

La classe nel listato 7, denominare la classe ModelStateWrapper, implementa l'interfaccia IValidationDictionary. È possibile creare istanze della classe ModelStateWrapper passando un dizionario di stato del modello al costruttore.

**Listato 7 - Models\ModelStateWrapper.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

Infine, il controller aggiornato nel listato 8 Usa il ModelStateWrapper quando si crea il livello di servizio nel relativo costruttore.

**Listato 8 - Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

Usando il IValidationDictionary interfaccia e la classe ModelStateWrapper ci consente di isolare completamente il livello di servizio dal nostro livello di controller. Il livello di servizio non è più dipendente dallo stato del modello. È possibile passare qualsiasi classe che implementa l'interfaccia IValidationDictionary al livello di servizio. Ad esempio, un'applicazione WPF può implementare l'interfaccia IValidationDictionary con una classe collection semplice.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è illustrare un approccio all'esecuzione della convalida in un'applicazione ASP.NET MVC. In questa esercitazione è stato descritto come spostare tutta la logica di convalida del controller e a un livello di servizio separato. Inoltre appreso come isolare il livello di servizio di livello il controller creando una classe ModelStateWrapper.

> [!div class="step-by-step"]
> [Precedente](validating-with-the-idataerrorinfo-interface-vb.md)
> [Successivo](validation-with-the-data-annotation-validators-vb.md)
