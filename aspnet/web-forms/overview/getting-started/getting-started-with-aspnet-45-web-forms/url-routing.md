---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Routing degli URL | Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET tramite ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per abbiamo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 279617e4ebb475d935c0d1e01e08a3a2def0f9e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="url-routing"></a>Routing degli URL
====================
Da [Erik Reitan](https://github.com/Erikre)

[Scarica progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con il codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento questa serie di esercitazioni è disponibile.


In questa esercitazione si modificherà l'applicazione di esempio Wingtip Toys per supportare il routing dell'URL. Routing consente all'applicazione web di utilizzare gli URL descrittivi, più facile da ricordare e meglio supportati dai motori di ricerca. In questa esercitazione si basa sull'esercitazione precedente, "Appartenenza e amministrazione" e fa parte della serie di esercitazioni Wingtip Toys.

## <a name="what-youll-learn"></a>Illustra quanto segue:

- Come registrare le route per un'applicazione Web Form ASP.NET.
- Come aggiungere le route a una pagina web.
- Come selezionare i dati da un database per supportare le route.

## <a name="aspnet-routing-overview"></a>Cenni preliminari sul Routing di ASP.NET

Routing degli URL consente di configurare un'applicazione per accettare richieste di URL che non fanno riferimento a file fisici. Un URL della richiesta è semplicemente l'URL di un utente immette nel browser per trovare una pagina nel sito web. Utilizzare il routing per definire gli URL che sono semanticamente significativi per gli utenti e che possono aiutare a con l'ottimizzazione motore di ricerca (SEO).

Per impostazione predefinita, il modello Web Form include [URL brevi di ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Molte delle operazioni di routing di base viene implementata tramite *URL brevi*. Tuttavia, in questa esercitazione si aggiungerà la funzionalità di routing personalizzata.

Prima di personalizzare l'URL di routing, l'applicazione di esempio Wingtip Toys possibile collegare a un prodotto utilizzando l'URL seguente:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Personalizzazione di routing degli URL, l'applicazione di esempio Wingtip Toys consente di collegare lo stesso prodotto con una più facile da leggere URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Route

Una route è un modello di URL che viene eseguito il mapping a un gestore. Il gestore può essere un file fisico, ad esempio un file in un'applicazione Web Form aspx. Un gestore può anche essere una classe che elabora la richiesta. Per definire una route, creare un'istanza della classe di Route, specificando il modello di URL, il gestore e facoltativamente un nome per la route.

Aggiungere la route per l'applicazione aggiungendo il `Route` oggetto statico `Routes` proprietà del `RouteTable` classe. La proprietà di route è una `RouteCollection` oggetto che archivia tutte le route per l'applicazione.

### <a name="url-patterns"></a>Modelli di URL

Un modello di URL può contenere valori letterali e segnaposto variabili (definiti parametri URL). I valori letterali e segnaposto si trovano in segmenti di URL che sono delimitati da barre (`/`) caratteri.

Quando viene effettuata una richiesta all'applicazione web, l'URL viene analizzato in segmenti e segnaposto e i valori delle variabili disponibili per il gestore di richieste. Questo processo è simile a quello che i dati in una stringa di query vengano analizzati e passati al gestore di richieste. In entrambi i casi, informazioni sulla variabile è incluso nell'URL e passati al gestore sotto forma di coppie chiave-valore. Per le stringhe di query, chiavi e i valori si trovano nell'URL. Per le route, le chiavi sono i nomi di segnaposto definiti nel modello di URL e solo i valori si trovano nell'URL.

In un modello di URL, è possibile definire segnaposto racchiudendoli tra parentesi graffe ( `{` e `}` ). È possibile definire più di un segnaposto in un segmento, ma i segnaposto devono essere separati da un valore letterale. Ad esempio, `{language}-{country}/{action}` è un modello di route valida. Tuttavia, `{language}{country}/{action}` non è valido, poiché non esiste alcun valore letterale o delimitatori tra i segnaposto. Pertanto, il routing non può determinare il percorso di separare il valore del segnaposto della lingua dal valore del segnaposto del paese.

### <a name="mapping-and-registering-routes"></a>Mapping e la registrazione delle route

Prima di poter includere le route per le pagine dell'applicazione di esempio Wingtip Toys, è necessario registrare le route all'avvio dell'applicazione. Per registrare le route, si modificherà il `Application_Start` gestore dell'evento.

1. In **Esplora**di Visual Studio, trovare e aprire il *Global.asax.cs* file.
2. Aggiungere il codice evidenziato in giallo per il *Global.asax.cs* file come segue:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Quando viene avviata dell'applicazione di esempio Wingtip Toys, chiama il `Application_Start` gestore dell'evento. Alla fine di questo gestore dell'evento, il `RegisterCustomRoutes` metodo viene chiamato. Il `RegisterCustomRoutes` metodo aggiunge ogni route chiamando il `MapPageRoute` metodo il `RouteCollection` oggetto. Le route vengono definite utilizzando un nome di route, un URL di route e un URL fisico.

Il primo parametro ("`ProductsByCategoryRoute`") è il nome della route. Consente di chiamare la route quando necessario. Il secondo parametro ("`Category/{categoryName}`") definisce la sostituzione descrittiva URL che possono essere dinamiche in base al codice. Utilizzare questo percorso quando si popola un controllo dati con i collegamenti che vengono generati in base ai dati. Una route viene visualizzata come segue:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Il secondo parametro della route include un valore dinamico specificato da parentesi graffe (`{ }`). In questo caso, il `categoryName` è una variabile che verrà utilizzata per determinare il percorso di routing corretto.

> [!NOTE] 
> 
> **Optional**
> 
> Può risultare più semplice gestire il codice spostando il `RegisterCustomRoutes` metodo in una classe separata. Nel *logica* cartella, creare un apposito `RouteActions` classe. Spostare il precedente `RegisterCustomRoutes` metodo il *Global.asax.cs* file nel nuovo `RoutesActions` classe. Utilizzare il `RoleActions` classe e il `createAdmin` un esempio di come chiamare il metodo di `RegisterCustomRoutes` metodo il *Global.asax.cs* file.


Si può anche notare il `RegisterRoutes` metodo chiamata utilizzando il `RouteConfig` oggetto all'inizio del `Application_Start` gestore dell'evento. Questa chiamata viene eseguita per implementare il routing predefinito. È incluso come codice predefinito quando è stata creata l'applicazione utilizzando il modello di Web Form di Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Recupero e sull'utilizzo dei dati sull'itinerario

Come indicato in precedenza, è possibile definire le route. Il codice aggiunto per il `Application_Start` gestore dell'evento nel *Global.asax.cs* file carica le route definibili.

### <a name="setting-routes"></a>Impostazione delle route

Le route è necessario scrivere codice aggiuntivo. In questa esercitazione si utilizzerà l'associazione di modelli per recuperare un `RouteValueDictionary` oggetto utilizzato durante la generazione delle route usando i dati di un controllo dati. Il `RouteValueDictionary` oggetto conterrà un elenco di nomi di prodotto che appartengono a una specifica categoria di prodotti. Viene creato un collegamento per ogni prodotto in base ai dati e una route.

#### <a name="enable-routes-for-categories-and-products"></a>Abilitare le route per le categorie e prodotti

Successivamente, si aggiornerà l'applicazione utilizzi la `ProductsByCategoryRoute` per determinare la route corretta da includere per ciascun collegamento di categoria di prodotto. Sarà inoltre possibile aggiornare il *ProductList.aspx* pagina per includere un collegamento di routing per ogni prodotto. I collegamenti verranno visualizzati come avveniva prima della modifica, tuttavia, i collegamenti verranno utilizzato il routing dell'URL.

1. In **Esplora**, aprire il *Site. master* se non è già aperto.
2. Aggiornamento di **ListView** controllo denominato "`categoryList`" con le modifiche evidenziate in giallo, pertanto il markup viene visualizzato come segue:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. In **Esplora**, aprire il *ProductList.aspx* pagina.
4. Aggiornamento di `ItemTemplate` elemento del *ProductList.aspx* pagina con gli aggiornamenti evidenziati in giallo, pertanto il markup viene visualizzato come segue:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Aprire il code-behind di *ProductList.aspx.cs* e aggiungere il seguente spazio dei nomi come evidenziato in giallo:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Sostituire il `GetProducts` metodo code-behind (*ProductList.aspx.cs*) con il codice seguente:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Aggiungere codice per i dettagli sul prodotto

A questo punto, aggiornare il code-behind (*ProductDetails.aspx.cs*) per il *ProductDetails.aspx* codici da utilizzare i dati della route. Si noti che il nuovo `GetProduct` accetta anche un valore di stringa di query per il caso in cui l'utente ha un collegamento con il segnalibro che utilizza l'URL non brevi, non indirizzate precedente.

1. Sostituire il `GetProduct` metodo code-behind (*ProductDetails.aspx.cs*) con il codice seguente:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione per visualizzare gli itinerari aggiornati.

1. Premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 Il browser viene visualizzata e viene illustrato il *Default.aspx* pagina.
2. Fare clic su di **prodotti** collegamento nella parte superiore della pagina.  
 Tutti i prodotti vengono visualizzati di *ProductList.aspx* pagina. Per il browser, viene visualizzato l'URL seguente (con il numero di porta):  
    `https://localhost:44300/ProductList`
3. Fare clic su, il **automobili** collegamento della categoria nella parte superiore della pagina.  
 Vengono visualizzate solo automobili nel *ProductList.aspx* pagina. Per il browser, viene visualizzato l'URL seguente (con il numero di porta):  
    `https://localhost:44300/Category/Cars`
4. Fare clic sul collegamento contenente il nome della prima Auto elencato nella pagina ("**convertibili Car**") per visualizzare i dettagli del prodotto.  
 Per il browser, viene visualizzato l'URL seguente (con il numero di porta):  
    `https://localhost:44300/Product/Convertible%20Car`
5. Quindi, immettere l'URL seguente non indirizzate (utilizzando il numero di porta) nel browser:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Il codice è ancora riconosce un URL che include una stringa di query, nel caso in cui un utente dispone di un collegamento con il segnalibro.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stata aggiunta una route per le categorie e i prodotti. Si è appreso come route possono essere integrate con i controlli di dati che utilizzano l'associazione del modello. Nella prossima esercitazione, verrà implementata la gestione degli errori globale.

## <a name="additional-resources"></a>Risorse aggiuntive

[URL brevi di ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Distribuire un'App di moduli Web ASP.NET in modo sicuro con appartenenza, OAuth e Database SQL di servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/)

>[!div class="step-by-step"]
[Precedente](membership-and-administration.md)
[Successivo](aspnet-error-handling.md)
