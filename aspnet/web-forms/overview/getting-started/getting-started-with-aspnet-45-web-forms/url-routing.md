---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Routing basato su URL | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Microsoft...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: d9f0779d560d6ec7796a16dc2996b959dd171c80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834739"
---
<a name="url-routing"></a>Routing degli URL
====================
da [Erik Reitan](https://github.com/Erikre)

[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento a questa serie di esercitazioni è disponibile.


In questa esercitazione si modificherà l'applicazione di esempio Wingtip Toys per supportare il routing di URL. Routing consente alle applicazioni web usare gli URL descrittivo facile da ricordare e meglio supportati dai motori di ricerca. Questa esercitazione si basa sull'esercitazione precedente "Appartenenza e amministrazione" e fa parte della serie di esercitazioni di Wingtip Toys.

## <a name="what-youll-learn"></a>Che cosa si apprenderà come:

- Come registrare le route per un'applicazione Web Form ASP.NET.
- Come aggiungere le route a una pagina web.
- Come selezionare i dati da un database per supportare le route.

## <a name="aspnet-routing-overview"></a>Panoramica del Routing di ASP.NET

Routing degli URL consente di configurare un'applicazione di accettare richieste di URL che non sono mappati a file fisici. URL della richiesta è semplicemente l'URL di un utente immette il browser per trovare una pagina nel sito web. Usare il routing per definire l'URL che sono significativi semanticamente per gli utenti e che può aiutare con l'ottimizzazione motore di ricerca (SEO).

Per impostazione predefinita, il modello Web Form include [URL brevi di ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Molte delle operazioni di routing basic verrà implementato usando *URL brevi*. Tuttavia, in questa esercitazione si aggiungerà la funzionalità di routing personalizzata.

Prima di personalizzare il routing dell'URL, l'applicazione di esempio Wingtip Toys possibile collegare a un prodotto utilizzando l'URL seguente:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Personalizzazione di routing degli URL, l'applicazione di esempio Wingtip Toys collegherà al prodotto stesso con un più facile da leggere URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Route

Una route è un modello di URL che viene eseguito il mapping a un gestore. Il gestore può essere un file fisico, ad esempio un file con estensione aspx in un'applicazione Web Form. Un gestore può anche essere una classe che elabora la richiesta. Per definire una route, creare un'istanza della classe di Route, specificando il modello di URL, il gestore e facoltativamente un nome per la route.

Si aggiunta la route per l'applicazione aggiungendo il `Route` oggetto alla proprietà statica `Routes` proprietà del `RouteTable` classe. La proprietà di route è una `RouteCollection` oggetto che archivia tutte le route per l'applicazione.

### <a name="url-patterns"></a>Modelli di URL

Un modello di URL può contenere valori letterali e segnaposto di variabili (definiti come parametri URL). I valori letterali e segnaposto si trovano in segmenti dell'URL a cui sono racchiusi tra la barra (`/`) caratteri.

Quando viene effettuata una richiesta all'applicazione web, l'URL viene analizzato nei segmenti e segnaposti e vengono forniti i valori delle variabili per il gestore di richieste. Questo processo è simile al modo i dati in una stringa di query vengano analizzati e passati al gestore di richieste. In entrambi i casi, le informazioni sulla variabile è incluso nell'URL e passati al gestore sotto forma di coppie chiave-valore. Per le stringhe di query, i valori e le chiavi sono nell'URL. Per le route, le chiavi sono i nomi di segnaposto definiti nel modello di URL, e solo i valori sono nell'URL.

In un modello di URL, si definiscono i segnaposto racchiudendoli tra parentesi graffe ( `{` e `}` ). È possibile definire più di un segnaposto in un segmento, ma i segnaposto devono essere separati da un valore letterale. Ad esempio, `{language}-{country}/{action}` è un modello di route valida. Tuttavia, `{language}{country}/{action}` non è valido, perché non esiste un valore letterale o il delimitatore tra i segnaposto. Pertanto, il routing non è possibile determinare dove separare il valore del segnaposto della lingua dal valore del segnaposto di paese.

### <a name="mapping-and-registering-routes"></a>Mapping e la registrazione delle route

Prima che sia possibile includere le route alle pagine dell'applicazione di esempio Wingtip Toys, è necessario registrare le route all'avvio dell'applicazione. Per registrare le route, si modificherà il `Application_Start` gestore dell'evento.

1. Nelle **Esplora soluzioni**di Visual Studio, trovare e aprire il *Global.asax.cs* file.
2. Aggiungere il codice evidenziato in giallo per il *Global.asax.cs* file come segue:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Quando vengono avviati dell'applicazione di esempio Wingtip Toys, chiama il `Application_Start` gestore dell'evento. Alla fine di questo gestore dell'evento, il `RegisterCustomRoutes` viene chiamato il metodo. Il `RegisterCustomRoutes` metodo aggiunge ogni route chiamando il `MapPageRoute` metodo il `RouteCollection` oggetto. Le route vengono definite utilizzando un nome di route, un URL di route e un URL fisico.

Il primo parametro ("`ProductsByCategoryRoute`") è il nome della route. Viene utilizzato per chiamare la route quando è necessaria. Il secondo parametro ("`Category/{categoryName}`") definisce la sostituzione descrittiva URL che può essere dinamicamente basato sul codice. Si utilizza questa route quando si popola un controllo dei dati con i collegamenti che vengono generati basati sui dati. Una route viene visualizzata come segue:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Il secondo parametro di route include un valore dinamico specificato da parentesi graffe (`{ }`). In questo caso, il `categoryName` è una variabile che verrà utilizzata per determinare il percorso di routing corretto.

> [!NOTE] 
> 
> **Optional**
> 
> Può risultare più semplice gestire il codice spostando il `RegisterCustomRoutes` metodo in una classe separata. Nel *per la logica* cartella, creare un oggetto separato `RouteActions` classe. Spostare il codice precedente `RegisterCustomRoutes` metodo di *Global.asax.cs* file nel nuovo `RoutesActions` classe. Usare la `RoleActions` classe e il `createAdmin` un esempio di come chiamare il metodo la `RegisterCustomRoutes` metodo dal *Global.asax.cs* file.


È inoltre possibile aver notato il `RegisterRoutes` chiamata di metodo utilizzando il `RouteConfig` oggetto all'inizio del `Application_Start` gestore dell'evento. Per implementare il routing predefinito viene effettuata la chiamata. Era incluso come codice predefinito quando è stata creata l'applicazione usando il modello di Web Form Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Il recupero e usando i dati di Route

Come indicato in precedenza, è possibile definire le route. Il codice aggiunto per il `Application_Start` gestore dell'evento nel *Global.asax.cs* file carica le route definibili.

### <a name="setting-routes"></a>L'impostazione di route

Le route è necessario scrivere codice aggiuntivo. In questa esercitazione si utilizzerà l'associazione di modelli per recuperare un `RouteValueDictionary` oggetto usato durante la generazione di route usando i dati di un controllo dati. Il `RouteValueDictionary` oggetto conterrà un elenco di nomi di prodotto che appartengono a una categoria di prodotti. Viene creato un collegamento per ogni prodotto in base i dati e la route.

#### <a name="enable-routes-for-categories-and-products"></a>Abilitare le route per i prodotti e le categorie

Successivamente, si aggiornerà l'applicazione per l'uso di `ProductsByCategoryRoute` per determinare la route corretta da includere per ogni collegamento di categoria di prodotto. Sarà inoltre possibile aggiornare il *ProductList. aspx* pagina per includere un collegamento indirizzato per ogni prodotto. I collegamenti verranno visualizzati come avveniva prima della modifica, tuttavia, i collegamenti utilizzerà ora il routing dell'URL.

1. Nelle **Esplora soluzioni**, aprire il *Site. master* se non è già aperto.
2. Aggiorna il **ListView** controllo denominato "`categoryList`" con le modifiche evidenziate in giallo, pertanto, il markup viene visualizzato come segue:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. Nelle **Esplora soluzioni**, aprire il *ProductList. aspx* pagina.
4. Aggiorna il `ItemTemplate` elemento del *ProductList. aspx* pagina con gli aggiornamenti evidenziati in giallo, in modo che il markup viene visualizzato come segue:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Aprire il code-behind della *ProductList.aspx.cs* e aggiungere il seguente spazio dei nomi come evidenziato in giallo:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Sostituire il `GetProducts` metodo di code-behind (*ProductList.aspx.cs*) con il codice seguente:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Aggiungere il codice per i dettagli sul prodotto

A questo punto, aggiornare il code-behind (*ProductDetails.aspx.cs*) per il *ProductDetails.aspx* codici da utilizzare i dati della route. Si noti che il nuovo `GetProduct` accetta anche un valore di stringa di query nel caso in cui l'utente ha un collegamento con segnalibro che usa l'URL non brevi, non indirizzate meno recente.

1. Sostituire il `GetProduct` metodo di code-behind (*ProductDetails.aspx.cs*) con il codice seguente:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione adesso per visualizzare le route aggiornate.

1. Premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 Il browser si apre e Mostra le *default. aspx* pagina.
2. Scegliere il **prodotti** collegamento nella parte superiore della pagina.  
 Tutti i prodotti vengono visualizzati nei *ProductList. aspx* pagina. Per il browser viene visualizzato l'URL seguente (usando il numero di porta):  
    `https://localhost:44300/ProductList`
3. Successivamente, fare clic il **automobili** categoria collegamento nella parte superiore della pagina.  
 Solo Auto viene visualizzate nei *ProductList. aspx* pagina. Per il browser viene visualizzato l'URL seguente (usando il numero di porta):  
    `https://localhost:44300/Category/Cars`
4. Fare clic sul collegamento contenente il nome della prima Auto elencato nella pagina ("**convertibile Car**") per visualizzare i dettagli del prodotto.  
 Per il browser viene visualizzato l'URL seguente (usando il numero di porta):  
    `https://localhost:44300/Product/Convertible%20Car`
5. Successivamente, immettere l'URL seguente non indirizzate (usando il numero di porta) nel browser:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Il codice riconosce ancora un URL che include una stringa di query, nel caso in cui un utente dispone di un collegamento con segnalibro.

## <a name="summary"></a>Riepilogo

In questa esercitazione, si sono aggiunte le route per i prodotti e le categorie. Si è appreso come le route possono essere integrate con i controlli di dati che usano l'associazione di modelli. Nella prossima esercitazione, si implementerà la gestione degli errori globali.

## <a name="additional-resources"></a>Risorse aggiuntive

[URL brevi di ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Distribuire un'App di moduli Web ASP.NET sicura con appartenenza, OAuth e Database SQL in servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Precedente](membership-and-administration.md)
> [Successivo](aspnet-error-handling.md)
