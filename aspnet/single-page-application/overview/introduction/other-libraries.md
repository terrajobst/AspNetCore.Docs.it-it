---
uid: single-page-application/overview/introduction/other-libraries
title: Librerie diverse da Knockout sanno | Microsoft Docs
author: madskristensen
description: Librerie diverse da Knockout sanno
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: ''
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 0424d209cbd24756d1a840788bb3dc5b48d905ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375240"
---
<a name="know-a-library-other-than-knockout"></a>Librerie diverse da Knockout sanno
====================
da [Mads Kristensen](https://github.com/madskristensen)

Il [modello di applicazione a pagina singola (SPA)](knockoutjs-template.md) è un ottimo modo per iniziare a scrivere applicazioni a singola pagina. Il modello Usa [Knockout. js](http://knockoutjs.com/) per associare i dati dell'applicazione per gli elementi DOM.

Ma Knockout non è la libreria JavaScript sola per la creazione di applicazioni rich client. Altre librerie di risolvono problematiche simili in modi diversi. Si potrebbe preferire una unica libreria rispetto a un altro, in modo che sono stati apportati diversi i modelli creati dalla community disponibili per il download. Ognuno di questi modelli utilizza una diversa combinazione di librerie client JavaScript.

Per installare un modello creati dalla community, visitare uno dei modelli le pagine elencate di seguito e fare clic sul pulsante Download. I modelli vengono forniti come file VSIX.

## <a name="backbonejs"></a>BackboneJS

[Modello di applicazione a singola pagina backbone](../templates/backbonejs-template.md). Questo modello fornisce uno scheletro iniziale per lo sviluppo di un [backbone](http://backbonejs.org/) dell'applicazione in ASP.NET MVC. Impostazione predefinita fornisce funzionalità di accesso utente di base, tra cui la reimpostazione della password di iscrizione, accesso, utente e la conferma dell'utente con i modelli di base tramite posta elettronica.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) è una libreria open source per la gestione di dati complessi in un client JavaScript. Gioco da ragazzi gestisce l'esecuzione di query, memorizzazione nella cache, il rilevamento delle modifiche, convalida e altro ancora. Due modelli della funzionalità Breeze:

- Il [Breeze/Knockout](../templates/breezeknockout-template.md) modello estende il modello di applicazione a singola pagina Knockout, che mostra con quanta facilità è possibile compilare un'applicazione a singola pagina con gioco da ragazzi per la gestione dei dati e Knockout. js per il data binding.
- Il [Breeze/Angular](../templates/breezeangular-template.md) modello estende anche il modello di applicazione a singola pagina Knockout gioco da ragazzi, ma utilizzando il [AngularJS](http://angularjs.org) libreria per l'associazione dati, inserimento di dipendenze e la gestione dello schermo.

Inoltre, il [modello Hot Towel SPA](../templates/hottowel-template.md) Usa BreezeJS.

## <a name="emberjs"></a>EmberJS

[Modello EmberJS SPA](../templates/emberjs-template.md). Questo modello Usa [Ember](http://emberjs.com/), una libreria JavaScript di MVC potente che consente di risolvere un'ampia gamma di problematiche per la compilazione di applicazioni rich client.

Il modello di applicazione a singola pagina Ember è una nuova implementazione del modello di applicazione a singola pagina Knockout, usando creazione modello EmberJS e Handlebars.

## <a name="hot-towel"></a>Hot Towel

[Modello di hot Towel SPA](../templates/hottowel-template.md). Questo modello offre diverse librerie JavaScript, tra cui gioco da ragazzi, Knockout, RequireJS e Twitter Bootstrap.

Confrontato con altri modelli elencati di seguito, il teample Hot Towel fornisce un'applicazione più completa da cui è possibile compilare il proprio. Esistono altri concetti da tenere presenti, ma dopo aver appreso li, questo modello potrebbe essere semplicemente ciò che sta cercando. Se si vuole compilare un'applicazione a singola pagina, ma non è possibile decidere dove iniziare, usare Hot Towel e in pochi secondi si otterrà un'applicazione a singola pagina e tutti gli strumenti devi compilare su di esso.

## <a name="feature-table"></a>Tabella delle funzionalità

Ecco le funzionalità fornite da ogni modello di applicazione a singola pagina:


|                        | ASP.NET SPA | Backbone | Breeze/Angular | Breeze/KO |  Ember   | Hot Towel |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Esempio ToDo       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Il modello      |             | &#10003; |                |           |          | &#10003;  |
| Navigazione e la cronologia |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Indicazione        |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         Gioco da ragazzi         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

