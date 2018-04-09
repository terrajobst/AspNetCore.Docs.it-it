---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: Panoramica e File -> Nuovo progetto | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Viene illustrato come parte 1 Panoramica e File -> Nuovo progetto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-file-new-project"></a>Parte 1: Panoramica e File -> Nuovo progetto
====================
da [Jon Galloway](https://github.com/jongalloway)

> L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.  
>   
> L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 1 copre panoramica e File -&gt;nuovo progetto.


## <a name="overview"></a>Panoramica

L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Web Developer per lo sviluppo web. Si verrà avviata lenta, pertanto l'esperienza di sviluppo web di livello per principianti è accettabile.

L'applicazione che è da generare è un archivio di musica semplice. Esistono tre parti principali per l'applicazione: market, estrazione e l'amministrazione.

![](mvc-music-store-part-1/_static/image1.jpg)

I visitatori possono passare album per genere:

![](mvc-music-store-part-1/_static/image2.jpg)

È possibile visualizzare album di un singolo e aggiungerlo al carrello:

![](mvc-music-store-part-1/_static/image3.jpg)

È possibile esaminarne carrello, la rimozione di tutti gli elementi che non si desidera più:

![](mvc-music-store-part-1/_static/image4.jpg)

Procedere con l'acquisto verrà richiesto loro di accedere o registrare un account utente.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Dopo aver creato un account, completano l'ordine inserendo le informazioni di pagamento e di spedizione. Per semplicità, in corso una promozione straordinaria: tutto ciò che è disponibile se si immette il codice di innalzamento di livello "FREE".

![](mvc-music-store-part-1/_static/image5.jpg)

Dopo aver ordinato, viene visualizzato una schermata di conferma semplice:

![](mvc-music-store-part-1/_static/image6.jpg)

Oltre alle pagine faceing di clienti, è inoltre sarà compilare una sezione di amministratore che mostra un elenco di album da cui gli amministratori possono creare, modificare ed eliminare album:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. File -&gt; nuovo progetto

### <a name="installing-the-software"></a>L'installazione del software

In questa esercitazione si inizierà creando un nuovo progetto ASP.NET MVC 3 con il libero Visual Web Developer 2010 Express (che è gratuito) e quindi verrà aggiunto in modo incrementale le funzionalità per creare un'applicazione funzionante completezza. Inoltre, ci occuperemo accesso al database, gli scenari di registrazione di modulo, la convalida dei dati, utilizzando le pagine master per il layout di pagina coerente, AJAX per gli aggiornamenti della pagina e la convalida, account di accesso utente e altro ancora.

È possibile seguire la procedura dettagliata, oppure è possibile scaricare l'applicazione finita da [negozio di musica MVC](https://github.com/evilDave/MVC-Music-Store).

Per compilare l'applicazione, è possibile utilizzare Visual Studio 2010 SP1 o Visual Web Developer 2010 Express SP1 (versione gratuita di Visual Studio 2010). Verrà usato SQL Server Compact (anche gratuito) per ospitare il database. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito.


- [Prerequisiti di visual Studio Web Developer Express SP1]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4.0 -] incluso il supporto degli strumenti e runtime


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Crea un nuovo progetto ASP.NET MVC 3

Si inizierà selezionando "Nuovo progetto" dal menu File in Visual Web Developer. Verrà visualizzata la finestra di dialogo Nuovo progetto.

![](mvc-music-store-part-1/_static/image5.png)

Si selezionerà il Visual c# -&gt; modelli Web gruppo a sinistra, quindi scegliere il modello "Applicazione Web di ASP.NET MVC 3" nella colonna centrale. Denominare il progetto MvcMusicStore e fare clic sul pulsante OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Verrà visualizzata una finestra di dialogo secondario che consente di verificare alcune impostazioni specifiche per il progetto MVC in. Selezionare gli elementi seguenti:

Modello di progetto - selezionare vuoto

Motore di visualizzazione: selezionare Razor

Utilizza markup semantico HTML5 - selezionata

Verificare che le impostazioni sono come illustrato di seguito, quindi fare clic sul pulsante OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Verrà creato il progetto. Esaminiamo le cartelle che sono stati aggiunti all'applicazione in Esplora soluzioni sul lato destro.

![](mvc-music-store-part-1/_static/image10.jpg)

Il modello vuoto MVC 3 non è completamente vuoto e aggiunge una struttura di cartelle di base:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC si avvalgono di alcune convenzioni di denominazione di base per i nomi delle cartelle:

| **Cartella** | **Scopo** |
| --- | --- |
| **/Controllers** | Controller rispondono a input dal browser, decidere cosa fare con esso e restituire una risposta all'utente. |
| **/Views** | Viste contengono i nostri modelli dell'interfaccia utente |
| **O i modelli** | I modelli contengono e modificare i dati |
| **/Content** | Questa cartella contiene il nostro immagini, CSS e qualsiasi altro contenuto statico |
| **/Scripts** | Questa cartella contiene i file JavaScript |

Queste cartelle sono inclusi anche in un'applicazione MVC ASP.NET vuoto poiché il framework ASP.NET MVC per impostazione predefinita viene utilizzato un approccio "convenzione sulla configurazione" e rende alcuni presupposti in base alle convenzioni di denominazione di cartella. Ad esempio, i controller di cercano le viste nella cartella Views per impostazione predefinita senza dover specificare in modo esplicito nel codice. Trovarsi bene con le convenzioni predefinite riduce la quantità di codice da scrivere, e può rendere più semplice per gli altri sviluppatori comprendere il progetto. Verrà illustrato queste convenzioni ulteriori mentre si compila l'applicazione.

> [!div class="step-by-step"]
> [avanti](mvc-music-store-part-2.md)
