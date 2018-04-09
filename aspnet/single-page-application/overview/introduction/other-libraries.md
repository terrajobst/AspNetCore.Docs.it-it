---
uid: single-page-application/overview/introduction/other-libraries
title: Stabilire una libreria di tipi diversi da Knockout? | Microsoft Docs
author: madskristensen
description: Stabilire una libreria di tipi diversi da Knockout?
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 6ac260e88fd156bad4b414e93325d5a04c490c88
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="know-a-library-other-than-knockout"></a>Stabilire una libreria di tipi diversi da Knockout?
====================
da [Mads Kristensen](https://github.com/madskristensen)

Il [modello di applicazione a pagina singola (SPA)](knockoutjs-template.md) è un ottimo modo per iniziare la scrittura di applicazioni a pagina singola. Il modello utilizza [Knockout.js](http://knockoutjs.com/) per associare i dati dell'applicazione per gli elementi DOM.

Ma Knockout non è la libreria JavaScript sola per la creazione di applicazioni rich client. Altre librerie di risolvere le problematiche simili in modi diversi. È possibile scegliere una raccolta di uno rispetto a un altro, pertanto sono stati apportati diversi i modelli creati dalla community disponibili per il download. Ognuno di questi modelli Usa una combinazione diversa di librerie JavaScript client.

Per installare un modello creati dalla community, visitare uno dei modelli le pagine elencate di seguito e fare clic sul pulsante Download. I modelli vengono forniti come file VSIX.

## <a name="backbonejs"></a>BackboneJS

[Modello SPA backbone.js](../templates/backbonejs-template.md). Questo modello offre uno scheletro iniziale per lo sviluppo di un [Backbone.js](http://backbonejs.org/) applicazione MVC ASP.NET. Fornisce funzionalità di accesso utente di base, tra cui la reimpostazione della password di iscrizione, accedi, utente e la conferma dell'utente con i modelli di posta elettronica di base predefinita.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) è una libreria open source per la gestione dei dati complessi in un client JavaScript. Semplicissimo gestisce l'esecuzione di query, memorizzazione nella cache, il rilevamento delle modifiche, convalida e altro ancora. Due modelli funzionalità molto semplice:

- Il [semplicissimo/Knockout](../templates/breezeknockout-template.md) modello estende il modello di SPA Knockout, visualizzazione con quanta facilità è possibile compilare un'applicazione a pagina singola con molto semplice per la gestione di dati e Knockout.js per l'associazione dati.
- Il [semplicissimo/angolare](../templates/breezeangular-template.md) modello estende anche il modello di SPA Knockout con molto semplice, ma utilizza il [AngularJS](http://angularjs.org) libreria per il data binding, l'inserimento di dipendenze e gestione dello schermo.

Inoltre, il [modello Hot degli asciugamani SPA](../templates/hottowel-template.md) utilizza BreezeJS.

## <a name="emberjs"></a>EmberJS

[Modello di SPA EmberJS](../templates/emberjs-template.md). Questo modello Usa [Ember](http://emberjs.com/), una libreria JavaScript MVC potente che consente di risolvere una vasta gamma di problemi per la compilazione di applicazioni rich client.

Il modello di SPA Ember è una nuova implementazione del modello di SPA Knockout, utilizzando il modello EmberJS e hl.

## <a name="hot-towel"></a>Scelta degli asciugamani

[Modello di SPA asciugamani a caldo](../templates/hottowel-template.md). Questo modello unisce diverse librerie JavaScript, tra cui molto semplice, Knockout, RequireJS e Twitter Bootstrap.

Confrontato con altri modelli elencati di seguito, teample la scelta degli asciugamani fornisce un'applicazione più completa da cui è possibile compilare la propria. Esistono più concetti da tenere presenti, ma dopo aver appreso li, questo modello potrebbe essere semplicemente cosa si sta cercando. Se si desidera compilare un SPA ma non è possibile decidere dove avviare, utilizzare degli asciugamani a caldo e, in secondi è necessario un SPA e tutti gli strumenti, è necessario creare su di esso.

## <a name="feature-table"></a>Tabella delle funzionalità

Di seguito sono le funzionalità fornite da ogni modello SPA:


|                        | ASP.NET SPA | Backbone | Semplicissimo/Angular | Semplicissimo/KO |  Ember   | Scelta degli asciugamani |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Esempio di attività       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Modello bare      |             | &#10003; |                |           |          | &#10003;  |
| Navigazione e cronologia |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Indicazione        |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         Molto semplice         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

