---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: Le annotazioni dei dati di utilizzo per la convalida del modello | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 6 viene illustrato l'utilizzo di annotazioni di dati per il modello V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b2083d5604741a0142f504f92779c9f931d0d6d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Parte 6: Utilizzando le annotazioni dei dati per la convalida del modello
====================
da [Jon Galloway](https://github.com/jongalloway)

> L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.  
>   
> L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 6 viene illustrato l'utilizzo di annotazioni di dati per la convalida del modello.


Abbiamo un problema con la creazione e modifica di form: non dovranno più svolgere alcuna operazione di convalida. È possibile eseguire operazioni quali lasciare vuoti i campi obbligatori o lettere tipo nel campo Prezzo ed è il primo errore che vedremo dal database.

È possibile aggiungere facilmente la convalida per l'applicazione aggiungendo le annotazioni dei dati per il nostro classi del modello. Le annotazioni dei dati consentono di descrivere le regole che vogliamo applicate per la proprietà del modello e MVC ASP.NET si occuperà di applicandoli e visualizzare i messaggi appropriati per gli utenti.

## <a name="adding-validation-to-our-album-forms"></a>Aggiunta della convalida per il form Album

Si userà gli attributi di annotazione dei dati seguenti:

- **Obbligatorio** : indica che la proprietà è un campo obbligatorio
- **DisplayName** -definisce il testo che si desidera utilizzare i campi del form e i messaggi di convalida
- **StringLength** – definisce una lunghezza massima per un campo stringa
- **Intervallo** : fornisce un valore minimo e massimo per un campo numerico
- **Associare** : Elenca i campi da escludere o includere durante l'associazione dei valori di parametro o modulo alle proprietà del modello
- **ScaffoldColumn** : consente di nascondere i campi da moduli di editor

*Nota: Per ulteriori informazioni sulla convalida del modello utilizzando gli attributi di annotazione dei dati, vedere la documentazione MSDN all'indirizzo*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Aprire la classe Album e aggiungere le seguenti *utilizzando* istruzioni all'inizio.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Aggiornare quindi le proprietà per aggiungere gli attributi di visualizzazione e la convalida, come illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Mentre si è presente, è stato modificato il Genre e artista alle proprietà virtuali. In questo modo di Entity Framework caricamento lazy in base alle necessità.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Dopo avere aggiunto questi attributi il nostro modello Album, la schermata di creare e modificare immediatamente di convalida dei campi e i nomi visualizzati utilizzando abbiamo scelto (ad esempio, Album Art Url anziché AlbumArtUrl). Eseguire l'applicazione e passare a /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Successivamente, di interruzione alcune regole di convalida. Immettere un prezzo pari a 0 e lasciare il campo vuoto. Quando si fa clic sul pulsante Crea, è possibile vedere il form visualizzato con messaggi di errore che mostra i campi che non soddisfano le regole di convalida che sono state definite.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Test di convalida lato Client

La convalida sul lato server è molto importante dalla prospettiva dell'applicazione, perché gli utenti possono aggirare la convalida lato client. Pagina Web Form che implementano solo la convalida sul lato server presentano tuttavia tre problemi significativi.

1. L'utente deve rimanere in attesa per il form da registrare, convalidato sul server e per la risposta da inviare al browser.
2. L'utente non riceve un feedback immediato quando si corregge un campo in modo che passa a questo punto le regole di convalida.
3. Ci stiamo un inutile consumo di risorse del server per eseguire la logica di convalida anziché sfruttando il browser dell'utente.

Fortunatamente, i modelli dello scaffolding di ASP.NET MVC 3 hanno la convalida lato client incorporata, che richiedono alcun intervento aggiuntivo qualunque.

Digitare una sola lettera nel campo titolo soddisfa i requisiti di convalida, quindi il messaggio di convalida viene rimosso immediatamente.

![](mvc-music-store-part-6/_static/image3.png)


>[!div class="step-by-step"]
[Precedente](mvc-music-store-part-5.md)
[Successivo](mvc-music-store-part-7.md)
