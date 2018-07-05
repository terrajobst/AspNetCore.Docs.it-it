---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: Panoramica e creazione del progetto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0540f3142d73fef616e30544bb1130b75c0bb436
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816305"
---
<a name="part-1-overview-and-creating-the-project"></a>Parte 1: Panoramica e creazione del progetto
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework è un framework di mapping relazionale a oggetti /. Viene eseguito il mapping di oggetti di dominio nel codice le entità in un database relazionale. Nella maggior parte, non è necessario preoccuparsi che il livello di database, perché Entity Framework si occupa di esso per te. Il codice lo modifica gli oggetti e le modifiche vengono mantenute per un database.

## <a name="about-the-tutorial"></a>In merito all'esercitazione

In questa esercitazione si creerà un'applicazione semplice archivio. Esistono due parti principali per l'applicazione. Gli utenti normali possono visualizzare i prodotti e creano gli ordini:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Gli amministratori possono creare, eliminare o modificare i prodotti:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Competenze

Ecco cosa si apprenderà:

- Come usare Entity Framework con ASP.NET Web API.
- Come usare Knockout. js per creare un client dell'interfaccia utente dinamico.
- Come usare l'autenticazione basata su form con l'API Web per autenticare gli utenti.

Sebbene in questa esercitazione è autonoma, è possibile leggere prima di tutto le esercitazioni seguenti:

- [Prima API Web ASP.NET](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Creazione di un'API Web che supporta operazioni CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Conoscenza degli [ASP.NET MVC](../../../../mvc/index.md) è inoltre utile.

## <a name="overview"></a>Panoramica

A livello generale, ecco l'architettura dell'applicazione:

- ASP.NET MVC genera le pagine HTML per il client.
- API Web ASP.NET espone le operazioni CRUD sui dati (i prodotti e ordini).
- Entity Framework traduce i modelli c# usati dalle API Web nelle entità di database.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Il diagramma seguente illustra come gli oggetti di dominio sono rappresentati a vari livelli dell'applicazione: il livello di database, il modello a oggetti e infine il formato wire, che consente di trasmettere i dati al client tramite HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

È possibile creare il progetto esercitazione utilizzando la versione completa di Visual Studio o Visual Web Developer Express.

Dal **avviare** pagina, fare clic su **nuovo progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo. Sotto **Visual c#**, selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto "ProductStore" e fare clic su **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona **applicazione Internet** e fare clic su **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Il modello "Applicazione Internet" consente di creare un'applicazione MVC ASP.NET che supporta l'autenticazione basata su form. Se si esegue l'applicazione a questo punto, ha già alcune funzionalità:

- Nuovi utenti possono registrare facendo clic sul collegamento "Register" nell'angolo superiore destro.
- Gli utenti registrati possono accedere facendo clic sul collegamento "Accedi".

Le informazioni di appartenenza sono persistente in un database che viene creato automaticamente. Per altre informazioni sull'autenticazione basata su form in ASP.NET MVC, vedere [procedura dettagliata: uso autenticazione basata su form in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Aggiornare il File CSS

Questo passaggio è generico; serve, ma verranno effettuate le pagine di eseguire il rendering, come le schermate precedenti.

In Esplora soluzioni espandere la cartella del contenuto e aprire il file denominato CSS. Aggiungere gli stili CSS seguenti:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [avanti](using-web-api-with-entity-framework-part-2.md)
