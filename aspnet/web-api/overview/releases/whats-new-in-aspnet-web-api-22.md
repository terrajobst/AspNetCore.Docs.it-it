---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Che cosa è stato introdotta in ASP.NET Web API 2.2 | Documenti Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 89b065fccd0e4864f4a24c37b4caa29a1e127840
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961299"
---
<a name="whats-new-in-aspnet-web-api-22"></a>Che cosa è stato introdotta in ASP.NET Web API 2.2
====================
da [Microsoft](https://github.com/microsoft)

In questo argomento sono descritte le novità per ASP.NET Web API 2.2.

- [Download](#download)
- [Documentazione](#documentation)
- [Nuove funzionalità in ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Attributo miglioramenti di Routing](#ARI)
    - [Supporto Client per le API Web per Windows Phone 8.1](#phone)
- [Problemi noti e modifiche di rilievo](#known-issues)
- [Correzioni di bug](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come dei pacchetti NuGet in Raccolta NuGet. Seguono tutti i pacchetti di runtime di [controllo delle versioni semantico](http://semver.org/) specifica. Il pacchetto più recente di ASP.NET Web API 2.2 è la seguente versione: "5.2.0". È possibile installare o aggiornare i pacchetti attraverso [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.

È possibile installare o aggiornare i pacchetti NuGet rilasciati mediante la Console di gestione pacchetti NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET Web API 2.2 sono disponibili dal sito web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Nuove funzionalità in ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Questa versione aggiunge il supporto per il protocollo di OData v4. Per altre informazioni, vedere il [Web API OData v4 documentazione.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Di seguito sono riportate alcune funzionalità chiave e le modifiche per OData v4:

- [Supporto per le proprietà alias nel modello di OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Supporto per ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute e ConcurrencyCheckAttribute in ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Offrono possibilità di specificare un titolo descrittivo per le azioni](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Integrazione con UriParser ODL
- Supporto per [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [contenimento](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) e [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Supporto per eseguire il cast per i tipi primitivi
- [Aggiunta del supporto di OData (funzione)](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Supporta gli alias dei parametri per le chiamate di funzione](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Supporta la convenzione di denominazione maiuscole iniziali nel modello](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Supporto per cast () in $filter
- Supporto per il tipo complesso aperto
- EntitySetController rimosso e AsyncEntitySetController
- [$Link modificate a $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Aggiunta del supporto routing attributo](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Usa le librerie di base di OData 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Attributo miglioramenti di Routing

Attributo Routing ora fornisce un punto di estensibilità chiamato IDirectRouteProvider, consentendo a controllo completo sulla modalità di individuazione e configurate route di attributi. Un IDirectRouteProvider è responsabile di fornire un elenco di azioni e controller insieme alle informazioni di route associata per specificare esattamente quale configurazione di routing è desiderate per tali azioni. Un'implementazione IDirectRouteProvider può essere specificata quando si chiama MapAttributes/MapHttpAttributeRoutes.

Personalizzazione IDirectRouteProvider sarà più semplice estendendo l'implementazione predefinita, DefaultDirectRouteProvider. Questa classe fornisce metodi separati virtuali sottoponibile a override per modificare la logica per l'individuazione di attributi, la creazione di voci della route e individuare il prefisso di route e il prefisso dell'area.

Di seguito sono riportati alcuni esempi su cosa si può fare con il nuovo punto di estendibilità:

1. Supportano l'ereditarietà di attributi delle Route

    Esempio:

    Di seguito una richiesta like "/ api/valori/10" restituirebbe correttamente "Operazione riuscita: 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Fornire un nome di route predefinita per la route di attributi seguendo alcuni convenzione desiderato. Per impostazione predefinita, routing degli attributi non crea automaticamente i nomi per le route di attributo.
3. Modificare modello di route le route attributo in una posizione centrale prima che finiscono con nella tabella di route.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Supporto dei Client per le API Web per Windows Phone 8.1

È ora possibile usare il pacchetto NuGet di Web API Client per implementare la logica di client Web API quando la destinazione è Windows Phone 8.1 o dall'interno di un'App universale.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti problemi noti e modifiche di rilievo nel 2.2 di API Web ASP.NET.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Generatore di modelli

Problema: Le funzioni in overload potrebbe non essere esposto come FunctionImport

Se sono presenti funzioni in overload 2 e sono inoltre FunctionImport come mostrato di seguito quindi richiedere risultati ~/GetAllConventionCustomers(CustomerName={customerName}) in System. InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Soluzione alternativa: La soluzione alternativa per questo problema consiste nell'aggiungere entrambi gli overload della funzione come FunctionImports.

#### <a name="odata-routing"></a>Routing OData

Valori letterali stringa che includono l'URL codificato barra (% 2F) e backslash(%5C) causa un errore 404 quando vengono utilizzati nei percorsi delle risorse OData.

Ad esempio, i valori letterali stringa utilizzabile nei percorsi delle risorse OData come parametri delle funzioni o i valori chiavi del set di entità.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Quando i servizi ricevono l'escape di annullare la host verranno richieste le sequenze di escape prima di passarli al runtime di API Web. Ciò consente di proteggere da attacchi di tipo simile al seguente:  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

In questo modo lo stack di Web API OData restituire un errore 404 (non trovato). Per evitare questo errore, il client deve usare le sequenze di escape doppia per barra (% 252F) e barra rovesciata (% C 255). Questo non accade per le stringhe di query, ad esempio /Employees? $filter = Name eq 'Nome % 2F'

**Prendere nota delle barre sottoposti a escape ('/') e barre rovesciate (") non sono valide nei valori letterali stringa di percorso risorse di OData. Le barre dovrebbero essere visualizzati solo come separatori di percorso e le barre rovesciate non devono essere visualizzati nel percorso delle risorse OData affatto. (Entrambi sono utilizzabili in alcune parti della stringa di query OData).**

Soluzione alternativa: È eseguire l'override del metodo Parse di DefaultODataPathHandler per la barra e una barra rovesciata nei valori letterali stringa di escape prima effettivamente durante l'analisi. È possibile trovare un esempio di questo approccio qui.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Disponibile per query]

L'attributo [Queryable] è deprecato. Nuove OData v3 applicazioni devono utilizzare **System.Web.Http.OData.EnableQueryAttribute**.

Il **ODataHttpConfigurationExtensions.EnableQuerySupport** metodo di estensione si aggiunge un **EnableQueryAttribute** all'insieme di filtri globali. La presenza di eventuali controller il **[Queryable]** attributo, la chiamata `config.EnableQuerySupport()` determinerà il **[Queryable]** attributo esito negativo

Il metodo consigliato per risolvere questo problema è per sostituire tutte le istanze di **QueryableAttribute** con **System.Web.Http.OData.EnableQueryAttribute**.

Una soluzione alternativa consiste nell'utilizzare il codice seguente nella configurazione dell'API Web:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Routing degli attributi

Problema: Associazione di modelli di tipo complesso che è decorato con attributi FromUri si comporta in modo diverso quando si utilizza Routing degli attributi.

Collegamento seguente fa riferimento al monitoraggio il problema e dispone inoltre informazioni dettagliate su una soluzione alternativa.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problema: Lo Scaffolding MVC o Web API in un progetto con 5.2.0 i risultati di pacchetti in 5.1.2 pacchetti per quelle che non esistono già nel progetto

Aggiornamento dei pacchetti NuGet per ASP.NET MVC 5.2 non aggiornare gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET o il modello di progetto applicazione Web ASP.NET. Usano la versione precedente dei pacchetti di runtime ASP.NET (ad esempio 5.1.2 in Update 2). Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (ad esempio 5.1.2 in Update 2) di pacchetti richiesti, se non sono già disponibili nei progetti. Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti più recenti nei progetti. Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento di pacchetti dei progetti Web API 2.2 o ASP.NET MVC 5.2, assicurarsi che le versioni di API Web e ASP.NET MVC sono coerenti.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correzioni di bug e aggiornamenti delle funzionalità secondarie

Questa versione include inoltre diverse correzioni di bug e le funzionalità secondarie gli aggiornamenti. È possibile trovare l'elenco completo di seguito:

- [pacchetto 5.2](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Il pacchetto Microsoft.AspNet.OData 5.2.1 contiene aggiornamenti dipendenza NuGet ma nessun correzioni di bug. Con questo aggiornamento, non è più una dipendenza rigida in Microsoft.OData.Core 6.4.0, ma uno può eseguire l'aggiornamento a qualsiasi versione tra 6.4.0 e 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

In questa versione è stata introdotta una dipendenza modificare per `Json.Net 6.0.4`. Per ulteriori informazioni sulle novità in questa versione di `Json.NET`, vedere [Json.NET 6.0 versione 4 - esegue il Merge JSON, inserimento di dipendenze](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Questa versione non dispone di altre funzionalità nuove o correzioni di bug in Web API. È stata aggiornata successivamente tutti gli altri pacchetti dipendente che è proprietario affinché dipenda da questa nuova versione dell'API Web.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

È possibile leggere sulla versione [qui](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Questa versione include solo correzioni di bug. È possibile utilizzare [questa query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) per visualizzare l'elenco di problemi risolti in questa versione.
