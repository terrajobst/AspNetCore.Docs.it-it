---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: "Parte 7: L'appartenenza e l'autorizzazione | Documenti Microsoft"
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 7 copre l'appartenenza e l'autorizzazione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a0f599da4691c5bb7c8e6f01625fc0e94ce0eac8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879504"
---
<a name="part-7-membership-and-authorization"></a>Parte 7: L'appartenenza e l'autorizzazione
====================
da [Jon Galloway](https://github.com/jongalloway)

> L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.  
>   
> L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 7 copre l'appartenenza e l'autorizzazione.


Il controller di gestione di Store è attualmente accessibile a tutti gli utenti che visitano il sito. È necessario modificare questa opzione per limitare le autorizzazioni per gli amministratori del sito.

## <a name="adding-the-accountcontroller-and-views"></a>Aggiunta di AccountController e viste

Una differenza tra il modello di applicazione Web di ASP.NET MVC 3 completo e il modello di applicazione Web vuoto di ASP.NET MVC 3 è che il modello vuoto non include un Controller di Account. Verrà aggiunto un Controller di Account copiando alcuni file da una nuova applicazione MVC ASP.NET creata dal modello di applicazione Web di ASP.NET MVC 3 completo.

Creare una nuova applicazione MVC ASP.NET utilizzando il modello di applicazione Web di ASP.NET MVC 3 completo e copiare i file seguenti nella directory nel progetto:

1. AccountController.cs copia nella directory del controller
2. AccountModels copia nella directory di modelli
3. Creare una directory di Account all'interno della directory di viste e copiare tutte le quattro viste in

Modificare lo spazio dei nomi per le classi Controller e il modello in modo che iniziano con MvcMusicStore. La classe AccountController deve utilizzare lo spazio dei nomi MvcMusicStore.Controllers, e la classe AccountModels deve utilizzare lo spazio dei nomi MvcMusicStore.Models.

*Nota: Questi file sono anche disponibili nel download MvcMusicStore Assets.zip cui abbiamo copiato i file di progettazione del sito all'inizio dell'esercitazione. I file di appartenenza si trovano nella directory del codice.*

La soluzione aggiornata dovrebbe essere simile al seguente:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Aggiunta di un utente amministratore con il sito di configurazione di ASP.NET

Prima è necessaria autorizzazione nel sito, è necessario creare un utente con accesso. Il modo più semplice per creare un utente è utilizzare il sito Web configurazione ASP.NET predefinito.

Avviare il sito Web ASP.NET configurazione facendo clic sul pulsante seguente l'icona visualizzata in Esplora soluzioni.

![](mvc-music-store-part-7/_static/image2.png)

Consente di avviare un sito Web di configurazione. Fare clic sulla scheda sicurezza nella schermata iniziale, quindi fare clic sul collegamento "Abilita ruoli" al centro dello schermo.

![](mvc-music-store-part-7/_static/image3.png)

Fare clic sul collegamento "Crea o Gestisci ruoli".

![](mvc-music-store-part-7/_static/image4.png)

Immettere "Administrator" come nome del ruolo e fare clic sul pulsante Aggiungi ruolo.

![](mvc-music-store-part-7/_static/image5.png)

Fare clic sul pulsante Indietro, quindi fare clic sul collegamento Crea utente sul lato sinistro.

![](mvc-music-store-part-7/_static/image6.png)

Compilare i campi di informazioni utente sulla sinistra usando le informazioni seguenti:

| **Campo** | **Valore** |
| --- | --- |
| **Nome utente** | Amministratore |
| **Password** | password123! |
| **Conferma password** | password123! |
| **Posta elettronica** | (qualsiasi indirizzo di posta elettronica funzionerà) |
| **Domanda segreta** | (nome desiderato) |
| **Risposta segreta** | (nome desiderato) |

*Nota: Naturalmente è possibile utilizzare qualsiasi password desiderato. Le impostazioni di sicurezza predefinite password richiedono una password che è di 7 caratteri e contiene un carattere non alfanumerico.*

Selezionare il ruolo di amministratore per l'utente e fare clic sul pulsante Crea utente.

![](mvc-music-store-part-7/_static/image7.png)

A questo punto, si verrà visualizzato un messaggio che indica che l'utente è stato creato correttamente.

![](mvc-music-store-part-7/_static/image8.png)

È ora possibile chiudere la finestra del browser.

## <a name="role-based-authorization"></a>Autorizzazione basata sui ruoli

È ora possibile limitare l'accesso per il StoreManagerController utilizzando l'attributo [Authorize], specifica che l'utente deve essere il ruolo di amministratore per accedere a qualsiasi azione del controller nella classe.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Nota: L'attributo [Authorize] può essere inserita nei metodi di azione specifici, nonché a livello di classe Controller.*

A questo punto la selezione di /StoreManager visualizzata una finestra di dialogo accesso:

![](mvc-music-store-part-7/_static/image9.png)

Dopo l'accesso con il nuovo account di amministratore, siamo in grado di passare alla schermata di modifica Album come prima.

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-6.md)
> [Successivo](mvc-music-store-part-8.md)
