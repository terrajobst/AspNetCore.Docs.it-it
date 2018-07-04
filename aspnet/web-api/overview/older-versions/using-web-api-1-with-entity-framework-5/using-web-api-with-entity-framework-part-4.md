---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: Aggiunta di una visualizzazione di amministrazione | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 599f684ba200821d7fcd33819937c5a5b5a29ce8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371051"
---
<a name="part-4-adding-an-admin-view"></a>Parte 4: Aggiunta di una visualizzazione di amministrazione
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Aggiungere una visualizzazione di amministrazione

A questo punto si sarà attiva sul lato client e aggiungere una pagina che può usare dati dal controller di amministrazione. La pagina consentirà agli utenti di creare, modificare o eliminare i prodotti, inviando le richieste AJAX al controller.

In Esplora soluzioni espandere la cartella Controllers e aprire il file denominato HomeController.cs. Questo file contiene un controller MVC. Aggiungere un metodo denominato `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

Il **HttpRouteUrl** metodo consente di creare l'URI per l'API web e viene memorizzato nel contenitore di visualizzazione per un uso successivo.

Successivamente, posizionare il cursore del testo all'interno di `Admin` metodo di azione, quindi fare clic e scegliere **Aggiungi visualizzazione**. Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

Nel **Aggiungi visualizzazione** finestra di dialogo Nome visualizzazione "Admin". Selezionare la casella di controllo etichettata **creare una visualizzazione fortemente tipizzata**. Sotto **classe di modello**, selezionare "Prodotto (ProductStore.Models)". Lasciare tutte le altre opzioni sui valori predefiniti.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Facendo clic **Aggiungi** aggiunge un file denominato Admin.cshtml sotto le visualizzazioni/Home. Aprire il file e aggiungere il codice HTML seguente. Questo codice HTML definisce la struttura della pagina, ma non è collegata alcuna funzionalità ancora.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Creare un collegamento alla pagina di amministrazione

In Esplora soluzioni espandere la cartella visualizzazioni e quindi espandere la cartella condivisa. Aprire il file denominato \_layout. cshtml. Individuare il **ul** elemento con id = "menu" e un collegamento all'azione per la visualizzazione di amministrazione:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> Nel progetto di esempio, ho apportato alcune altre modifiche descrittivo, ad esempio sostituendo la stringa "Inserire qui il logo". Queste non influiscono sulle funzionalità dell'applicazione. È possibile scaricare il progetto e confrontare i file.


Eseguire l'applicazione e fare clic sul collegamento "Admin" che viene visualizzato nella parte superiore della home page. Pagina di amministrazione dovrebbe essere simile al seguente:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Diritto a questo punto, la pagina non esegue alcuna operazione. Nella sezione successiva, si userà Knockout. js per creare un'interfaccia utente dinamica.

## <a name="add-authorization"></a>Aggiungere l'autorizzazione

Pagina di amministrazione è attualmente accessibile a tutti gli utenti che visitano il sito. È possibile modificare questa impostazione per limitare le autorizzazioni per gli amministratori.

Iniziare aggiungendo un ruolo di "Amministratore" e un utente amministratore. In Esplora soluzioni espandere la cartella di filtri e aprire il file denominato InitializeSimpleMembershipAttribute.cs. Individuare il `SimpleMembershipInitializer` costruttore. Dopo la chiamata a **WebSecurity.InitializeDatabaseConnection**, aggiungere il codice seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Questo è un modo rapido per aggiungere il ruolo "Amministratore" e creare un utente per il ruolo.

In Esplora soluzioni espandere la cartella Controllers e aprire il file HomeController.cs. Aggiungere il **Authorize** dell'attributo di `Admin` (metodo).

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Aprire il file AdminController.cs e aggiungere il **Authorize** dell'attributo all'intero `AdminController` classe.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC e API Web definiscono entrambi **Authorize** attributi, in diversi spazi dei nomi. Usa MVC **System.Web.Mvc.AuthorizeAttribute**, mentre l'API Web Usa **System.Web.Http.AuthorizeAttribute**.


Solo gli amministratori possono ora visualizzare la pagina di amministrazione. Inoltre, se si invia una richiesta HTTP al controller di amministrazione, la richiesta deve contenere un cookie di autenticazione. In caso contrario, il server invia una risposta HTTP 401 (non autorizzato). È possibile verificarlo in Fiddler inviando una richiesta GET a `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-3.md)
> [Successivo](using-web-api-with-entity-framework-part-5.md)
