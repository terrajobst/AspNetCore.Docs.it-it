---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: Panoramica e File -> Nuovo progetto | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 1 viene illustrato come panoramica e File -> Nuovo progetto.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: d84a525221e40b79853be55069367134d17fb5ec
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838386"
---
<a name="part-1-overview-and-file-new-project"></a>Parte 1: Panoramica e File -> Nuovo progetto
====================
da [Jon Galloway](https://github.com/jongalloway)

> Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.  
>   
> La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 1 illustra panoramica e File -&gt;nuovo progetto.


## <a name="overview"></a>Panoramica

Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Web Developer per lo sviluppo web. Si sarà possibile avviare lentamente, in modo che l'esperienza di sviluppo di livello web per principianti è corretto.

L'applicazione che sarà compilata si è un archivio musicale semplice. Sono disponibili tre sezioni principali per l'applicazione: market, estrazione e l'amministrazione.

![](mvc-music-store-part-1/_static/image1.jpg)

I visitatori possono passare gli album in base al genere:

![](mvc-music-store-part-1/_static/image2.jpg)

Essi possono visualizzare un album singolo e aggiungerlo al carrello acquisti:

![](mvc-music-store-part-1/_static/image3.jpg)

È possibile esaminarne carrello, la rimozione di tutti gli elementi che non sono più necessarie:

![](mvc-music-store-part-1/_static/image4.jpg)

Procedere con l'acquisto verrà loro richiesto di accedere o registrarsi per un account utente.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Dopo aver creato un account, completano l'ordine inserendo le informazioni di pagamento e spedizione. Per semplicità, eseguiamo una promozione straordinaria: è tutto gratuito se si immette il codice di promozione "Gratuito".

![](mvc-music-store-part-1/_static/image5.jpg)

Dopo l'ordinamento, visualizzano una schermata di conferma semplice:

![](mvc-music-store-part-1/_static/image6.jpg)

Oltre alle pagine cliente faceing, si creerà anche una sezione di amministratore che visualizza un elenco di album da cui gli amministratori possono creare, modificare ed Elimina gli album:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. File -&gt; nuovo progetto

### <a name="installing-the-software"></a>Installazione del software

Questa esercitazione si inizierà creando un nuovo progetto ASP.NET MVC 3 con il gratuito Visual Web Developer 2010 Express (che è gratuita) e quindi verrà aggiunto in modo incrementale le funzionalità per creare un'applicazione funziona completa. Lungo il percorso, si affronterà l'accesso al database, gli scenari di registrazione di form, la convalida dei dati, utilizzando le pagine master per il layout delle pagine coerente, l'utilizzo di AJAX per gli aggiornamenti della pagina e convalida, account di accesso utente e altro ancora.

È possibile seguire la procedura dettagliata, oppure è possibile scaricare l'applicazione completata [MVC-musica-Store](https://github.com/evilDave/MVC-Music-Store).

Per compilare l'applicazione, è possibile usare Visual Studio 2010 SP1 o Visual Web Developer 2010 Express SP1 (una versione gratuita di Visual Studio 2010). Verrà usato SQL Server Compact (anche gratuito) per ospitare il database. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito.


- [Visual Studio Web Developer Express SP1 prerequisiti]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4.0 -] incluso il supporto degli strumenti e runtime


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Crea un nuovo progetto ASP.NET MVC 3

Si inizierà selezionando "Nuovo progetto" nel menu File in Visual Web Developer. Verrà visualizzata la finestra di dialogo Nuovo progetto.

![](mvc-music-store-part-1/_static/image5.png)

Scegliamo Visual c# -&gt; modelli Web Raggruppa in base a sinistra, quindi scegliere il modello "Applicazione Web di ASP.NET MVC 3" nella colonna centrale. Denominare il progetto MvcMusicStore e premere il pulsante OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Verrà visualizzata una finestra di dialogo secondaria che consentono di configurare alcune impostazioni specifiche di MVC per il progetto. Selezionare gli elementi seguenti:

Modello di progetto: seleziona vuoto

Motore di visualizzazione - seleziona Razor

Utilizza markup semantico HTML5 - selezionato

Verificare che le impostazioni sono come illustrato di seguito, quindi fare clic su OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Verrà creato il progetto. Diamo un'occhiata le cartelle che sono stati aggiunti all'applicazione in Esplora soluzioni sul lato destro.

![](mvc-music-store-part-1/_static/image10.jpg)

Il modello MVC 3 vuota non è completamente vuoto, che aggiunge una struttura di cartelle di base:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC Usa alcune convenzioni di denominazione di base per i nomi delle cartelle:

| **Cartella** | **Scopo** |
| --- | --- |
| **/ Controller** | Controller rispondono a input dal browser, definire l'azione da eseguire su di essi e restituire la risposta all'utente. |
| **/ Viste** | Le visualizzazioni contengono i modelli dell'interfaccia utente |
| **O i modelli** | I modelli contengono e utilizzano i dati |
| **/Content** | Questa cartella contiene le nostre immagini, CSS e qualsiasi altro contenuto statico |
| **/ Script** | Questa cartella contiene i file JavaScript |

Queste cartelle sono inclusi anche in un'applicazione MVC ASP.NET vuoto perché il framework ASP.NET MVC per impostazione predefinita Usa un approccio "convention over configuration" e fa alcune supposizioni predefinito basati sulle convenzioni di denominazione delle cartelle. Ad esempio, i controller di cercano le visualizzazioni nella cartella Views per impostazione predefinita senza dover specificare in modo esplicito nel codice. Partendo con le convenzioni predefinite riduce la quantità di codice è necessario scrivere, e può anche rendere più semplice per gli altri sviluppatori comprendere il progetto. Spiegheremo queste convenzioni più poiché noi creiamo la nostra applicazione.

> [!div class="step-by-step"]
> [avanti](mvc-music-store-part-2.md)
