---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: Aggiunta di una vista di amministrazione | Documenti Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-4-adding-an-admin-view"></a>Parte 4: Aggiunta di una vista di amministrazione
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Aggiungere una visualizzazione di amministrazione

A questo punto verrà attiva al lato client e aggiungere una pagina che è possibile utilizzare i dati dal controller di amministrazione. La pagina consentirà agli utenti di creare, modificare o eliminare i prodotti, inviando le richieste AJAX al controller.

In Esplora soluzioni, espandere la cartella Controllers e aprire il file denominato HomeController. cs. Questo file contiene un controller MVC. Aggiungere un metodo denominato `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

Il **HttpRouteUrl** metodo crea l'URI per l'API web e si archiviarlo nel contenitore delle visualizzazioni per un momento successivo.

Successivamente, posizionare il cursore di testo all'interno di `Admin` metodo di azione, quindi fare clic e scegliere **Aggiungi visualizzazione**. Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

Nel **Aggiungi visualizzazione** finestra di dialogo, nome visualizzazione "Admin". Selezionare la casella di controllo **creare una visualizzazione fortemente tipizzata**. In **classe modello**, selezionare "Prodotto (ProductStore.Models)". Lasciare tutte le altre opzioni come valori predefiniti.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Fare clic su **Aggiungi** aggiunge un file denominato Admin.cshtml in Views/Home. Aprire il file e aggiungere il seguente codice HTML. Questo codice HTML definisce la struttura della pagina, ma non è associata alcuna funzionalità ancora.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Creare un collegamento alla pagina di amministrazione

In Esplora soluzioni, espandere la cartella Views e quindi espandere la cartella condivisa. Aprire il file denominato \_cshtml. Individuare il **ul** elemento con id = "menu" e un collegamento all'azione per la visualizzazione di amministrazione:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> Nel progetto di esempio, apportate alcune altre modifiche cosmetici, ad esempio sostituendo la stringa "Inserire qui il logo". Questi non influisce sulla funzionalità dell'applicazione. È possibile scaricare il progetto e confrontare i file.


Eseguire l'applicazione e fare clic sul collegamento "Admin" che viene visualizzato nella parte superiore della home page. Pagina di amministrazione di dovrebbe essere simile al seguente:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Diritto a questo punto, la pagina non esegue alcuna operazione. Nella sezione successiva, useremo Knockout.js per creare un'interfaccia utente dinamica.

## <a name="add-authorization"></a>Aggiungi autorizzazione

La pagina di amministrazione è attualmente accessibile a tutti gli utenti che visitano il sito. È necessario modificare questa opzione per limitare le autorizzazioni per gli amministratori.

Per iniziare, aggiungere un ruolo "Administrator" e un utente amministratore. In Esplora soluzioni, espandere la cartella di filtri e aprire il file denominato InitializeSimpleMembershipAttribute.cs. Individuare il `SimpleMembershipInitializer` costruttore. Dopo la chiamata a **WebSecurity.InitializeDatabaseConnection**, aggiungere il codice seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Questo è un modo rapido per aggiungere il ruolo "Administrator" e creare un utente per il ruolo.

In Esplora soluzioni, espandere la cartella Controllers e aprire il file HomeController. Aggiungere il **Authorize** attributo la `Admin` metodo.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Aprire il file AdminController.cs e aggiungere il **Authorize** attributo per l'intero `AdminController` classe.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC e Web API definiscono entrambi **Authorize** gli attributi, in spazi dei nomi diversi. MVC utilizza **System.Web.Mvc.AuthorizeAttribute**, mentre utilizza l'API Web **System.Web.Http.AuthorizeAttribute**.


Ora solo gli amministratori possono visualizzare la pagina di amministrazione. Inoltre, se si invia una richiesta HTTP al controller di amministrazione, la richiesta deve contenere un cookie di autenticazione. In caso contrario, il server invia una risposta HTTP 401 (non autorizzato). È possibile notarlo in Fiddler inviando una richiesta GET al `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-3.md)
> [Successivo](using-web-api-with-entity-framework-part-5.md)
