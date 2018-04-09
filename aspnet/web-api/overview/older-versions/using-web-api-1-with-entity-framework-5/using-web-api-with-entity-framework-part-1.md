---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: Panoramica e la creazione del progetto | Documenti Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-creating-the-project"></a>Parte 1: Panoramica e la creazione del progetto
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework è un framework di mapping relazionale a oggetti /. Viene eseguito il mapping di oggetti dominio nel codice per le entità in un database relazionale. La maggior parte, non è necessario preoccuparsi il livello di database, poiché Entity Framework si occupa di esso per l'utente. Il codice consente di modificare gli oggetti, le modifiche vengono applicate a un database.

## <a name="about-the-tutorial"></a>Informazioni sull'esercitazione

In questa esercitazione si creerà un'applicazione di archiviazione semplice. Esistono due parti principali per l'applicazione. Agli utenti normali possono visualizzare i prodotti e creano gli ordini:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Gli amministratori possono creare, eliminare o modificare i prodotti:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Si apprenderà competenze

Ecco cosa si apprenderà:

- Modalità di utilizzo di Entity Framework con l'API Web ASP.NET.
- Descrive come usare knockout.js per creare un'interfaccia utente client dinamica.
- Come utilizzare l'autenticazione basata su form con l'API Web per autenticare gli utenti.

Sebbene in questa esercitazione è indipendente, è possibile leggere innanzitutto le esercitazioni seguenti:

- [Your First ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Creazione di un'API Web che supporta operazioni CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Conoscenza di [ASP.NET MVC](../../../../mvc/index.md) è inoltre utile.

## <a name="overview"></a>Panoramica

In generale, di seguito è l'architettura dell'applicazione:

- ASP.NET MVC genera le pagine HTML per il client.
- ASP.NET Web API espone operazioni CRUD sui dati (products e orders).
- Entity Framework traduce i modelli c# usati dall'API Web in entità di database.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Il diagramma seguente illustra come gli oggetti di dominio sono rappresentati a vari livelli dell'applicazione: il livello di database, il modello a oggetti e infine il formato di trasmissione, viene utilizzato per trasmettere dati al client tramite HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

È possibile creare il progetto tutorial utilizzando la versione completa di Visual Studio o Visual Web Developer Express.

Dal **avviare** pagina, fare clic su **nuovo progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo. In **Visual c#**selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto "ProductStore" e fare clic su **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo Seleziona **applicazione Internet** e fare clic su **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Il modello "Applicazione Internet" crea un'applicazione MVC ASP.NET che supporta l'autenticazione basata su form. Se si esegue ora l'applicazione, dispone già di alcune funzionalità:

- Nuovi utenti possono registrare facendo clic sul collegamento "Register" in alto a destra.
- Gli utenti registrati possono accedere facendo clic sul collegamento "Accedi".

Le informazioni di appartenenza sono persistente in un database che viene creato automaticamente. Per ulteriori informazioni sull'autenticazione basata su form in ASP.NET MVC, vedere [procedura dettagliata: utilizzo autenticazione basata su form in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Aggiornare il File CSS

Questo passaggio è cosmetico, ma verranno effettuate le pagine di eseguire il rendering come le schermate precedenti.

In Esplora soluzioni, espandere la cartella del contenuto e aprire il file denominato Site. Aggiungere gli stili CSS seguenti:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [avanti](using-web-api-with-entity-framework-part-2.md)
