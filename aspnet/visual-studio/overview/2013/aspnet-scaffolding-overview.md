---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Scaffolding di ASP.NET in Visual Studio 2013 | Microsoft Docs
author: Rick-Anderson
description: Scaffolding di ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 4246a52ad1d10da04a2a214f9dba6a935a9e9e72
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020976"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Scaffolding di ASP.NET in Visual Studio 2013
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Scaffolding di ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013.


## <a name="overview"></a>Panoramica

Scaffolding di ASP.NET è un framework di generazione di codice per applicazioni Web ASP.NET. Visual Studio 2013 include generatori di codice pre-installato per i progetti MVC e API Web. Aggiungere lo scaffolding al progetto quando si desidera aggiungere rapidamente il codice che interagisce con i modelli di dati. Usare lo scaffolding, è possibile ridurre la quantità di tempo per lo sviluppo di operazioni di dati standard nel progetto.

Per impostazione predefinita, Visual Studio 2013 non supporta la generazione di codice per un progetto di Web Form, ma è possibile usare lo scaffolding con Web Form mediante l'aggiunta di dipendenze MVC al progetto o installare un'estensione. Entrambi gli approcci sono illustrati di seguito.

Visual Studio 2013 Update 2 (attualmente RC) offre la possibilità di estendere lo Scaffolding di ASP.NET per soddisfare i requisiti dello scenario. Con questa funzionalità, è possibile creare un modello di scaffolding personalizzato e aggiungerlo alla finestra di dialogo Add Scaffold nuovo. All'interno del modello personalizzato, specificare il codice che viene generato quando si aggiunge un elemento di scaffolding. Per altre informazioni, vedere [creazione di un'utilità di scaffolding personalizzati per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Prerequisiti

Per usare lo Scaffolding di ASP.NET, è necessario disporre di:

- Microsoft Visual Studio 2013
- Strumenti di sviluppo Web (parte dell'installazione predefinita di Visual Studio 2013)
- ASP.NET Web Frameworks e Tools 2013 (parte dell'installazione predefinita di Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Aggiungere un elemento di scaffolding di MVC o API Web

Per aggiungere lo scaffolding, il progetto o una cartella all'interno del progetto e scegliere **Add** – **nuovo elemento di scaffolding**, come illustrato nell'immagine seguente.

![Aggiungi elemento di scaffolding](aspnet-scaffolding-overview/_static/image1.png)

Dal **Add Scaffold** finestra, selezionare il tipo di scaffolding da aggiungere.

![Selezionare il tipo di scaffolding](aspnet-scaffolding-overview/_static/image2.png)

Il **Aggiungi Controller** finestra ti offre la possibilità di selezionare le opzioni per la generazione di controller, ad esempio se si desidera utilizzare le nuove funzionalità asincrone da Entity Framework 6.

![Aggiungi controller](aspnet-scaffolding-overview/_static/image3.png)

Le classi pertinenti e le pagine vengono create per il proprio scenario. Ad esempio, l'immagine seguente mostra il controller MVC e le viste che sono state create tramite scaffolding di una classe di modello denominata film.

![I file creati](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Aggiungere un elemento di scaffolding per Web Form

Per aggiungere lo scaffolding che genera il codice di Web Form, è necessario installare un'estensione per Visual Studio o Aggiungi dipendenze MVC. Entrambi gli approcci sono illustrati di seguito, ma è sufficiente eseguire uno di questi approcci.

### <a name="web-forms-scaffolding-extension"></a>Web Forms Scaffolding estensione

È possibile installare un'estensione di Visual Studio che consentono di usare lo scaffolding con un progetto di Web Form. In Visual Studio, selezionare **degli strumenti** e quindi **estensioni e aggiornamenti**. Questa finestra di dialogo di ricerca per Visual Studio Gallery **estensione Web Forms Scaffolding**.

![installare lo scaffolding di web form](aspnet-scaffolding-overview/_static/image5.png)

Per altre informazioni, vedere [estensione Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Dipendenze MVC

Per aggiungere dipendenze MVC, selezionare **Add** - **nuovo elemento di scaffolding**. Nella finestra Aggiungi scaffolding, selezionare **dipendenze MVC**, come illustrato di seguito.

![Aggiungi dipendenze MVC](aspnet-scaffolding-overview/_static/image6.png)

Sono disponibili due opzioni per lo scaffolding di MVC; Minimal e complete. Se si seleziona minima, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto. Se si seleziona l'opzione completa, vengono aggiunte le dipendenze minime, nonché i necessari file di contenuto per un progetto MVC. Per utilizzare facilmente lo scaffolding, selezionare dipendenze complete.

![Selezionare le dipendenze complete](aspnet-scaffolding-overview/_static/image7.png)

Dopo aver aggiunto le dipendenze, si noterà una **Readme. txt** file. Seguire attentamente le istruzioni in questo file per garantire che il progetto funziona correttamente.

Dopo aver completato i passaggi nel file Readme. txt, è possibile aggiungere un nuovo elemento di scaffolding, come illustrato nella sezione precedente su MVC e API Web. Generato automaticamente visualizzazioni e controller funzionerà correttamente all'interno del progetto.

## <a name="tutorials"></a>Esercitazioni

Per creare un'utilità di scaffolding personalizzato, vedere [creazione di un'utilità di scaffolding personalizzati per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Per personalizzare i file generati, vedere [come personalizzare i file generati dalla finestra di dialogo Nuovo elemento di scaffolding](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Per un esempio d'uso con scaffolding **lo sviluppo di Database First**, vedere [Entity Framework Database First con ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Per un esempio dell'uso di scaffolding in un' **MVC** del progetto, vedere [Introduzione a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Per un esempio dell'uso di scaffolding in una **API Web** del progetto, vedere [creare un'API REST con il Routing con attributi nell'API Web 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
