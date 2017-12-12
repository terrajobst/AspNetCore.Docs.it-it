---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: L'installazione di un Helper in un Web ASP.NET di pagine del sito (Razor) | Documenti Microsoft
author: tfitzmac
description: "In questo articolo viene descritto come installare un helper in un sito Web ASP.NET Web Pages (Razor). Un helper è un componente riutilizzabile che include codice e markup per..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 842c5a56d14314217c1e6ad6d48ded28d3cc5b4e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>L'installazione di un file di supporto nel sito Web ASP.NET (Razor) pagine
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene descritto come installare un helper in un sito Web ASP.NET Web Pages (Razor). Oggetto *helper* è un componente riutilizzabile che include codice e markup per eseguire un'attività che potrebbe essere noiosa o complessi.
> 
> Illustra quanto segue:
> 
> - Come installare un helper in un sito Web creato utilizzando WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>Panoramica di helper

Alcune attività che gli utenti desiderano spesso nelle pagine web richiedono una grande quantità di codice o richiedono la conoscenza aggiuntivo. Esempi includono la visualizzazione di un grafico per i dati. inserimento di un pulsante di "Seguire" Twitter in una pagina. Invio messaggio di posta elettronica dal sito Web; il ritaglio e ridimensionamento di immagini. utilizzo di PayPal per il sito. Per consentire un facile eseguire questi tipi di elementi, le pagine Web ASP.NET consente di utilizzare *helper*. Gli helper sono componenti che si installa per un sito e che consentono di eseguono attività comuni usando solo una riga o codice Razor.

Le pagine Web ASP.NET dispone di alcuni helper compilato. Tuttavia, molte funzioni di supporto sono disponibili nei pacchetti che sono forniti usando Gestione pacchetti NuGet (componenti aggiuntivi). NuGet consente di selezionare un pacchetto da installare e quindi si occupa di tutti i dettagli dell'installazione.

## <a name="installing-a-helper-in-webmatrix-3"></a>L'installazione di un Helper in WebMatrix 3

1. In WebMatrix 3, fare clic su di **NuGet** pulsante.

    ![Nella finestra di dialogo Raccolta NuGet WebMatrix](installing-helpers/_static/image1.png)
2. Verrà avviato Gestione pacchetti NuGet e visualizza i pacchetti disponibili. Nella casella di ricerca, immettere una parola chiave per il supporto di cui che si desidera installare.

    ![Nella finestra di dialogo Raccolta NuGet WebMatrix](installing-helpers/_static/image2.png)
- Selezionare il pacchetto e quindi fare clic su **installare**. Fare clic su **Sì** quando viene richiesto se si desidera installare il pacchetto e indicare che si accettano le condizioni.

    Se questa è la prima volta che è stato installato un helper, NuGet consente di creare cartelle nel sito Web per il codice che costituisce l'helper.
- Per disinstallare un helper, fare clic su di **raccolta** fare clic su di **installato** scheda e selezionare il pacchetto che si desidera disinstallare.

## <a name="installing-the-twitter-helper"></a>L'installazione l'helper di Twitter

La versione più recente dell'API di Twitter non è compatibile con l'helper di Twitter che si installa tramite NuGet. In alternativa, vedere il [Helper di Twitter con WebMatrix](twitter-helper.md) argomento per informazioni su come configurare l'helper di Twitter nel progetto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Introduzione di ASP.NET Web Pages 2 - nozioni di base sulla programmazione](../getting-started/introducing-razor-syntax-c.md)

[Helper di Twitter con WebMatrix](twitter-helper.md)
