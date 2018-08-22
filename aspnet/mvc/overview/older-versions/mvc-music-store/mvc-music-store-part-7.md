---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: Appartenenza e autorizzazione | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 7 riguarda l'appartenenza e l'autorizzazione.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 44205f0ef59e00ad9fb1c45fdc0ba8934b5804cc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826953"
---
<a name="part-7-membership-and-authorization"></a>Parte 7: Appartenenza e autorizzazione
====================
da [Jon Galloway](https://github.com/jongalloway)

> Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.  
>   
> La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 7 riguarda l'appartenenza e l'autorizzazione.


Il controller di gestione Store è attualmente accessibile a tutti gli utenti che visitano il sito. È possibile modificare questa impostazione per limitare le autorizzazioni per gli amministratori del sito.

## <a name="adding-the-accountcontroller-and-views"></a>Aggiunta di AccountController e viste

Una differenza tra il modello di applicazione Web ASP.NET MVC 3 completo e il modello di applicazione Web vuota di ASP.NET MVC 3 è che il modello vuoto non include un Controller di Account. Si aggiungerà un Controller di Account copiando alcuni file da una nuova applicazione ASP.NET MVC creata dal modello di applicazione Web ASP.NET MVC 3 completo.

Creare una nuova applicazione MVC ASP.NET usando il modello di applicazione Web MVC 3 ASP.NET completo e copiare i file seguenti nelle stesse directory nel progetto:

1. AccountController.cs copia nella directory del controller
2. AccountModels copia nella directory di modelli
3. Creare una directory di Account all'interno della directory di viste e copiare tutte le quattro viste in

Modificare lo spazio dei nomi per le classi Controller e il modello in modo che inizino con MvcMusicStore. La classe AccountController deve utilizzare lo spazio dei nomi MvcMusicStore.Controllers e la classe AccountModels deve utilizzare lo spazio dei nomi MvcMusicStore.Models.

*Nota: Questi file sono anche disponibili nel download MvcMusicStore Assets.zip da cui viene copiato i file di progettazione del sito all'inizio dell'esercitazione. I file di appartenenza si trovano nella directory del codice.*

La soluzione aggiornata dovrebbe essere simile al seguente:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Aggiunta di un utente amministratore con il sito di configurazione ASP.NET

Prima si richiede autorizzazione nel nostro sito Web, è necessario creare un utente con accesso. Il modo più semplice per creare un utente deve usare il sito Web di configurazione di ASP.NET predefinito.

Avviare il sito Web ASP.NET configurazione facendo clic sul pulsante seguente l'icona visualizzata in Esplora soluzioni.

![](mvc-music-store-part-7/_static/image2.png)

Verrà avviato un sito Web di configurazione. Fare clic sulla scheda sicurezza nella schermata iniziale, quindi fare clic sul collegamento "Abilitare i ruoli" al centro della schermata.

![](mvc-music-store-part-7/_static/image3.png)

Fare clic sul collegamento "Crea o Gestisci ruoli".

![](mvc-music-store-part-7/_static/image4.png)

Immettere "Administrator" come nome del ruolo e fare clic su Add Role.

![](mvc-music-store-part-7/_static/image5.png)

Fare clic sul pulsante Indietro, quindi fare clic sul collegamento Crea utente sul lato sinistro.

![](mvc-music-store-part-7/_static/image6.png)

Compilare i campi di informazioni utente a sinistra con le informazioni seguenti:

| **Campo** | **Valore** |
| --- | --- |
| **Nome utente** | Amministratore |
| **Password** | password123! |
| **Conferma password** | password123! |
| **Posta elettronica** | (qualsiasi indirizzo di posta elettronica funzionerà) |
| **Domanda segreta** | (qualsiasi) |
| **Risposta segreta** | (qualsiasi) |

*Nota: È ovviamente possibile usare qualsiasi password desiderato. Le impostazioni di sicurezza predefinite password richiedono una password che è di 7 caratteri e contiene un carattere non alfanumerico.*

Selezionare il ruolo di amministratore per questo utente e fare clic sul pulsante Create User.

![](mvc-music-store-part-7/_static/image7.png)

A questo punto, si dovrebbe vedere un messaggio che indica che l'utente è stato creato correttamente.

![](mvc-music-store-part-7/_static/image8.png)

È ora possibile chiudere la finestra del browser.

## <a name="role-based-authorization"></a>Autorizzazione basata sui ruoli

A questo punto è possibile limitare l'accesso di StoreManagerController usando l'attributo [Authorize], che specifica che l'utente deve essere il ruolo di amministratore per accedere a qualsiasi azione del controller nella classe.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Nota: L'attributo [Authorize] può essere inserita nei metodi di azione specifici, nonché a livello di classe Controller.*

Passare ora al /StoreManager viene visualizzata una finestra di dialogo Accedi:

![](mvc-music-store-part-7/_static/image9.png)

Dopo l'accesso con il nuovo account di amministratore, siamo in grado di passare alla schermata di modifica Album come prima.

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-6.md)
> [Successivo](mvc-music-store-part-8.md)
