---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Lo Scaffolding di ASP.NET in Visual Studio 2013 | Documenti Microsoft
author: tfitzmac
description: "Lo Scaffolding di ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Scaffolding di ASP.NET in Visual Studio 2013
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Lo Scaffolding di ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013.


## <a name="overview"></a>Panoramica

Lo Scaffolding di ASP.NET è un framework di generazione di codice per applicazioni Web ASP.NET. Visual Studio 2013 sono inclusi generatori di codice pre-installato per i progetti MVC e Web API. Aggiungere lo scaffolding al progetto quando si desidera aggiungere rapidamente il codice che interagisce con i modelli di dati. Utilizzare lo scaffolding, è possibile ridurre la quantità di tempo per sviluppare le operazioni standard per i dati nel progetto.

Per impostazione predefinita, Visual Studio 2013 non supporta la generazione di codice per un progetto di Web Form, ma è possibile usare lo scaffolding con Web Form aggiungendo le dipendenze MVC al progetto o l'installazione di un'estensione. Entrambi gli approcci sono illustrati di seguito.

Visual Studio 2013 Update 2 (attualmente RC) offre la possibilità di estendere lo Scaffolding di ASP.NET per soddisfare i requisiti dello scenario. Con questa funzionalità, è possibile creare un modello di scaffolding personalizzato e aggiungerlo alla finestra di dialogo Aggiungi nuova pagina. All'interno del modello personalizzato, specificare il codice che viene generato quando si aggiunge un elemento di scaffolding. Per ulteriori informazioni, vedere [la creazione di un Scaffolder personalizzato per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Prerequisiti

Per usare lo Scaffolding di ASP.NET, è necessario disporre di:

- Microsoft Visual Studio 2013
- Strumenti di sviluppo Web (parte dell'installazione predefinita di Visual Studio 2013)
- Framework Web ASP.NET e strumenti 2013 (parte dell'installazione predefinita di Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Aggiungere un elemento di scaffolding MVC o Web API

Per aggiungere lo scaffolding, il progetto o una cartella all'interno del progetto e scegliere **Aggiungi** – **nuovo elemento di scaffolding**, come illustrato nella figura seguente.

![Aggiungere lo scaffolding elemento](aspnet-scaffolding-overview/_static/image1.png)

Dal **aggiungere lo scaffolding** finestra, selezionare il tipo di pagina da aggiungere.

![Selezionare il tipo di pagina](aspnet-scaffolding-overview/_static/image2.png)

Il **Aggiungi Controller** finestra consente di selezionare le opzioni per la generazione di controller, ad esempio se si desidera utilizzare le nuove funzionalità asincrone di Entity Framework 6.

![Aggiungi controller](aspnet-scaffolding-overview/_static/image3.png)

Le relative classi e le pagine vengono create per il proprio scenario. Ad esempio, l'immagine seguente mostra le controller MVC e le viste che sono state create tramite lo scaffolding per una classe di modello denominato film.

![I file creati](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Aggiungere un elemento di scaffolding al Web Form

Per aggiungere lo scaffolding che genera il codice di Web Form, è necessario installare un'estensione di Visual Studio o aggiungere le dipendenze MVC. Entrambi gli approcci sono illustrati di seguito, ma è necessario procedere in uno di questi approcci.

### <a name="web-forms-scaffolding-extension"></a>Web Form lo Scaffolding di estensione

È possibile installare un'estensione di Visual Studio che consentono di usare lo scaffolding con un progetto di Web Form. In Visual Studio, selezionare **strumenti** e quindi **estensioni e aggiornamenti**. Da questa finestra di dialogo di Visual Studio Gallery per ricerca **lo Scaffolding di Web Form**.

![installare lo scaffolding di web form](aspnet-scaffolding-overview/_static/image5.png)

Per ulteriori informazioni, vedere [lo Scaffolding di Web Form](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Dipendenze MVC

Per aggiungere le dipendenze MVC, selezionare **Aggiungi** - **nuovo elemento di scaffolding**. Nella finestra, aggiungere lo scaffolding selezionare **le dipendenze MVC**, come illustrato di seguito.

![aggiungere le dipendenze MVC](aspnet-scaffolding-overview/_static/image6.png)

Sono disponibili due opzioni per lo scaffolding MVC; Minimo e completo. Se si seleziona minima, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto. Se si seleziona l'opzione completo, vengono aggiunte le dipendenze minime, nonché i file di contenuto richiesti per un progetto MVC. Per utilizzare facilmente lo scaffolding, selezionare le dipendenze complete.

![Selezionare le dipendenze complete](aspnet-scaffolding-overview/_static/image7.png)

Dopo aver aggiunto le dipendenze, verrà visualizzato un **readme.txt** file. Seguire attentamente le istruzioni in questo file per garantire il corretto funzionamento del progetto.

Quando si hanno completato i passaggi nel file Readme. txt, è possibile aggiungere un nuovo elemento di scaffolding come illustrato nella sezione precedente relativa MVC e Web API. Generato automaticamente visualizzazioni e controller funzionerà correttamente all'interno del progetto.

## <a name="tutorials"></a>Esercitazioni

Per creare un scaffolder personalizzato, vedere [la creazione di un Scaffolder personalizzato per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Per personalizzare i file generati, vedere [come personalizzare i file generati dalla finestra di dialogo Nuovo elemento di scaffolding](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Per un esempio dell'utilizzo di scaffolding con **lo sviluppo del Database First**, vedere [Entity Framework Database First con ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Per un esempio di utilizzo delle pagine di supporto temporaneo in un **MVC** del progetto, vedere [Introduzione a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Per un esempio di utilizzo delle pagine di supporto temporaneo in un **API Web** del progetto, vedere [creare un'API REST con il Routing degli attributi in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
