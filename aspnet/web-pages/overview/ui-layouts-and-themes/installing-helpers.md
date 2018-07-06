---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installazione di un Helper in un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: Questo articolo descrive come installare un helper in un sito Web ASP.NET Web Pages (Razor). Un helper è un componente riutilizzabile che include il codice e markup per...
ms.author: aspnetcontent
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: db3dff9f2d70577bb0618335c0100b9899e87727
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838614"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Installazione di un Helper in un sito di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come installare un helper in un sito Web ASP.NET Web Pages (Razor). Oggetto *helper* è un componente riutilizzabile che include il codice e markup per eseguire un'attività che potrebbe essere noioso o complessi.
> 
> Che cosa si apprenderà come:
> 
> - Come installare un helper in un sito Web creato utilizzando WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>Panoramica degli helper

Alcune attività che spesso gli utenti vogliono eseguire operazioni sulle pagine web richiedono una grande quantità di codice o della Knowledge Base aggiuntiva. Gli esempi includono la visualizzazione di un grafico per i dati. inserimento di un pulsante "Segui" Twitter in una pagina. Invio messaggio di posta elettronica tramite il sito Web; ritaglio o ridimensionamento delle immagini; utilizzo di PayPal per il sito. Per renderlo semplice eseguire questo tipo di attività, ASP.NET Web Pages ti permette di usare *helper*. Gli helper sono componenti installare per un sito e che consentono di eseguono attività tipiche usando solo una o due righe di codice Razor.

ASP.NET Web Pages ha alcuni helper incorporato. Tuttavia, molti helper sono disponibili nei pacchetti forniti tramite Gestione pacchetti NuGet (componenti aggiuntivi). NuGet consente di selezionare un pacchetto da installare e quindi si occupa di tutti i dettagli dell'installazione.

## <a name="installing-a-helper-in-webmatrix-3"></a>Installazione di un Helper di WebMatrix 3

1. In WebMatrix 3, fare clic sui **NuGet** pulsante.

    ![Finestra di dialogo NuGet Gallery in WebMatrix](installing-helpers/_static/image1.png)
2. Verrà avviato Gestione pacchetti NuGet e visualizza i pacchetti disponibili. Nella casella di ricerca, immettere una parola chiave per l'helper in cui che si vuole installare.

    ![Finestra di dialogo NuGet Gallery in WebMatrix](installing-helpers/_static/image2.png)
3. Selezionare il pacchetto e quindi fare clic su **installare**. Fare clic su **Sì** quando viene richiesto se si desidera installare il pacchetto e indicare che si accettano le condizioni.

     Se questa è la prima volta che è stato installato un file di supporto, NuGet crea le cartelle nel sito Web per il codice che costituisce l'helper.
4. Per disinstallare un helper, fare clic sui **Gallery** e fare clic il **installato** scheda e selezionare il pacchetto da disinstallare.

## <a name="installing-the-twitter-helper"></a>Installare l'helper di Twitter

La versione più recente dell'API di Twitter non è compatibile con l'helper di Twitter che si installa tramite NuGet. Al contrario, vedere la [Helper di Twitter con WebMatrix](twitter-helper.md) argomento per informazioni su come configurare l'helper di Twitter nel progetto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Introduzione ad ASP.NET Web Pages 2 - nozioni fondamentali di programmazione](../getting-started/introducing-razor-syntax-c.md)

[Helper di Twitter con WebMatrix](twitter-helper.md)
