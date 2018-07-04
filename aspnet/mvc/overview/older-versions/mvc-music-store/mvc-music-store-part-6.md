---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: Uso delle annotazioni dei dati per la convalida del modello | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 6 viene illustrato l'utilizzo di annotazioni dei dati per il modello V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: ca0ac2ca909f838f1c91e6cc01b8aafa90c0b193
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397654"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Parte 6: Uso delle annotazioni dei dati per la convalida del modello
====================
da [Jon Galloway](https://github.com/jongalloway)

> Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.  
>   
> La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 6 viene illustrato l'utilizzo di annotazioni dei dati per la convalida del modello.


È presente un problema principale con la creazione e modifica di moduli: non dovranno più svolgere alcuna operazione di convalida. È possibile, ad esempio, lasciare vuoti i campi obbligatori o lettere tipo nel campo di prezzo ed è il primo errore che vedremo dal database.

Convalida possiamo aggiungere con facilità all'applicazione tramite l'aggiunta di annotazioni dei dati per le classi di modello. Le annotazioni dei dati consentono di descrivere le regole che vogliamo applicate per la proprietà del modello e ASP.NET MVC si occuperà di applicandoli e visualizzazione dei messaggi appropriati per i nostri utenti.

## <a name="adding-validation-to-our-album-forms"></a>Aggiunta della convalida per il form Album

Si userà gli attributi di annotazione dei dati seguenti:

- **Obbligatorio** – indica che la proprietà è un campo obbligatorio
- **DisplayName** : definisce il testo che si desidera utilizzare i campi del form e i messaggi di convalida
- **StringLength** -definisce una lunghezza massima per un campo stringa
- **Intervallo** : fornisce un valore minimo e massimo per un campo numerico
- **Associare** : Elenca i campi da escludere o includere quando si associano i valori di parametro o modulo alle proprietà del modello
- **ScaffoldColumn** : consente di nascondere i campi da form editor

*Nota: Per ulteriori informazioni sulla convalida del modello utilizzando gli attributi di annotazione dei dati, vedere la documentazione MSDN all'indirizzo*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Aprire la classe di Album e aggiungere quanto segue *usando* istruzioni all'inizio.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Successivamente, aggiornare le proprietà per aggiungere gli attributi di convalida e visualizzazione, come illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

È possibile, è stato modificato il genere e artista alle proprietà virtuale. Ciò consente a Entity Framework caricamento lazy in base alle esigenze.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Dopo avere aggiunto questi attributi per il nostro modello di Album, nostra schermata di creazione e modifica iniziare immediatamente la convalida dei campi e usando i nomi visualizzati abbiamo scelto (ad esempio, Album Art Url anziché AlbumArtUrl). Eseguire l'applicazione e passare a /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Successivamente, di interruzione alcune regole di convalida. Immettere un prezzo pari a 0 e lasciare vuoto il titolo. Quando si fa clic sul pulsante Crea, si vedrà il modulo visualizzato convalida dei messaggi di errore che mostra i campi che non soddisfa le regole di convalida che è stato definito.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Test di convalida sul lato Client

La convalida sul lato server è molto importante dal punto di vista dell'applicazione, perché gli utenti possono aggirare la convalida lato client. Pagina Web Form che implementano solo la convalida sul lato server, tuttavia, presentano tre problemi significativi.

1. L'utente deve attendere per il form da registrare, convalidati nel server e per la risposta da inviare al browser.
2. L'utente non ottiene un feedback immediato quando si corregge un campo in modo che passa a questo punto le regole di convalida.
3. Si sta inutile consumo di risorse del server per eseguire la logica di convalida anziché sfruttando il browser dell'utente.

Per fortuna, i modelli di scaffolding di ASP.NET MVC 3 hanno la convalida lato client incorporata, che richiedono alcun intervento aggiuntivo danno.

Digitare una sola lettera nel campo del titolo soddisfa i requisiti di convalida, in modo che il messaggio di convalida viene rimosso immediatamente.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-5.md)
> [Successivo](mvc-music-store-part-7.md)
