---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: What ' s New in ASP.NET Web API 2.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 12/25/2014
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 0f0794988da712897092ab808a08fca5eeebb6d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813278"
---
<a name="whats-new-in-aspnet-web-api-22"></a>What ' s New in API Web ASP.NET 2.2
====================
da [Microsoft](https://github.com/microsoft)

Questo argomento descrive cosa sono le novità di ASP.NET Web API 2.2.

- [Download](#download)
- [Documentazione](#documentation)
- [Nuove funzionalità in ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Miglioramenti di Routing dell'attributo](#ARI)
    - [Supporto di Client dell'API Web per Windows Phone 8.1](#phone)
- [Problemi noti e modifiche di rilievo](#known-issues)
- [Correzioni di bug](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Versione Beta Microsoft.AspNet.WebAPI 5.2.3](#523)

<a id="download"></a>
## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in NuGet gallery. Seguono tutti i pacchetti di runtime di [Versionamento semantico](http://semver.org/) specifica. Il pacchetto più recente di ASP.NET Web API 2.2 è la seguente versione: "5.2.0". È possibile installare o aggiornare i pacchetti attraverso [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.

È possibile installare o aggiornare ai pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET Web API 2.2 sono disponibili dal sito web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Nuove funzionalità in ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Questa versione aggiunge il supporto per il protocollo OData v4. Per altre informazioni, vedere il [Web API OData v4 documentazione.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Ecco alcune delle funzionalità chiave e le modifiche per OData v4:

- [Supporto per le proprietà alias nel modello di OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Supporto per ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute e ConcurrencyCheckAttribute in ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Offrono possibilità di specificare un titolo descrittivo per le azioni](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Integrare con UriParser ODL
- Supporto per [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [contenimento](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) e [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Supporto per eseguire il cast per i tipi primitivi
- [Aggiunta del supporto di OData (funzione)](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Supporta gli alias dei parametri per le chiamate di funzione](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Supporto di convenzione di denominazione camel case nel modello](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Supporto per cast () in $filter
- Supporto per il tipo complesso open
- EntitySetController rimosso e AsyncEntitySetController
- [$Link modificate a $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Aggiunta del supporto routing attributo](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Usa le librerie OData Core 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Miglioramenti di Routing dell'attributo

A questo punto il Routing con attributi offre un punto di estensibilità chiamato IDirectRouteProvider, che consente il controllo completo su come le route con attributi vengono individuate e configurate. Un IDirectRouteProvider è responsabile di fornire un elenco di azioni e controller insieme alle informazioni sulle route associata a specificare esattamente quali configurazione di routing è desiderato per le azioni. Quando si chiama MapAttributes/MapHttpAttributeRoutes si può specificare un'implementazione IDirectRouteProvider.

Personalizzazione IDirectRouteProvider sarà più semplice estendendo l'implementazione predefinita, DefaultDirectRouteProvider. Questa classe fornisce metodi virtuali separati sottoponibile a override per modificare la logica per l'individuazione di attributi, la creazione di voci della route e l'individuazione di prefisso di route e il prefisso dell'area.

Di seguito sono riportati alcuni esempi su cosa si può fare con questo nuovo punto di estendibilità:

1. Supporta l'ereditarietà degli attributi di Route

    Esempio:

    Ecco una richiesta di like "/ api/valori/10" correttamente restituirebbe "Esito positivo: 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Specificare un nome di route predefinita per le route con attributi seguendo alcune convenzioni desiderato. Per impostazione predefinita, il routing con attributi non crea automaticamente i nomi per le route con attributi.
3. Modificare modello di route le route di attributi in un'unica posizione centrale prima entrino nella tabella di route.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Supporto dei Client per le API Web per Windows Phone 8.1

È ora possibile usare il pacchetto NuGet di Client API Web per implementare la logica di client API Web quando la destinazione è Windows Phone 8.1 o dall'interno di un'App universale.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti problemi noti e modifiche di rilievo nell'API Web ASP.NET 2.2.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Generatore di modelli

Problema: Le funzioni in overload potrebbe non essere esposto come FunctionImport

Se sono presenti 2 funzioni in overload e sono anche FunctionImport come illustrato di seguito richiede quindi i risultati ~/GetAllConventionCustomers(CustomerName={customerName}) in System. InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Soluzione alternativa: La soluzione alternativa per risolvere questo problema consiste nell'aggiungere entrambi gli overload della funzione come FunctionImports.

#### <a name="odata-routing"></a>Routing di OData

Valori letterali stringa che includono l'URL codificato barra (2F %) e backslash(%5C) causa un errore 404 quando vengono usati nei percorsi delle risorse OData.

Ad esempio, i valori letterali stringa utilizzabile nei percorsi delle risorse OData come parametri di funzioni o valori di chiave del set di entità.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Quando i servizi ricevono richieste di questo tipo l'escape di un host verrà le sequenze di escape prima di passarli al runtime di API Web. Ciò consente di proteggere da attacchi simile al seguente:  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

In questo modo lo stack di Web API OData restituire un errore 404 (non trovato). Per evitare questo errore, il client deve utilizzare le sequenze di escape doppio per la barra (% 252F) e barra rovesciata (% C 255). Ciò non avviene per le stringhe di query, ad esempio /Employees? $filter = Name eq 'Nome % 2F'

**Prendere nota delle barre senza carattere di escape ('/') e le barre rovesciate (' ') non sono valide nei valori letterali stringa di percorso risorse OData. Le barre dovrebbero essere visualizzati solo come separatori di percorso e le barre rovesciate non devono essere visualizzati nel percorso delle risorse OData affatto. (Entrambe sono utilizzabili in alcune parti della stringa di query OData).**

Soluzione alternativa: È possibile ignorare il metodo di analisi di DefaultODataPathHandler di escape prima dell'analisi effettivamente la barra e una barra rovesciata nei valori letterali stringa. È possibile trovare un esempio di questo approccio qui.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Disponibile per query]

L'attributo [Queryable] è deprecato. Nuove OData v3 applicazioni utilizzino **System.Web.Http.OData.EnableQueryAttribute**.

Il **ODataHttpConfigurationExtensions.EnableQuerySupport** metodo di estensione si aggiunge un' **EnableQueryAttribute** all'insieme di filtri globali. In presenza di eventuali controller la **[Queryable]** dell'attributo, la chiamata `config.EnableQuerySupport()` causerà il **[Queryable]** attributo esito negativo

Il metodo consigliato per risolvere questo problema consiste nel sostituire tutte le istanze del **QueryableAttribute** con **System.Web.Http.OData.EnableQueryAttribute**.

Una soluzione alternativa consiste nell'usare il codice seguente nella configurazione dell'API Web:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Routing con attributi

Problema: L'associazione di modelli di tipo complesso che è decorata con l'attributo FromUri si comporta in modo diverso quando si usa il Routing con attributi.

Collegamento seguente tiene traccia del problema e ha anche informazioni dettagliate su una soluzione alternativa.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problema: Lo Scaffolding MVC/API Web in un progetto con 5.2.0 risultati i pacchetti in 5.1.2 di pacchetti per quelle che non esistono già nel progetto

Aggiornamento di pacchetti NuGet per ASP.NET MVC 5.2 non aggiorna il modello di progetto applicazione Web ASP.NET o gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET. Usano la versione precedente dei pacchetti di runtime ASP.NET (ad esempio 5.1.2 nell'aggiornamento 2). Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (ad esempio 5.1.2 nell'aggiornamento 2) dei pacchetti necessari, se non sono già disponibili nei progetti. Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti più recenti nei progetti. Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento dei pacchetti dei progetti Web API 2.2 o ASP.NET MVC 5.2, assicurarsi che le versioni dell'API Web e ASP.NET MVC sono coerenti.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correzioni di bug e gli aggiornamenti delle funzionalità secondarie

Questa versione include anche diverse correzioni di bug e la funzionalità secondaria degli aggiornamenti. È possibile trovare l'elenco completo di seguito:

- [pacchetto 5.2](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Il pacchetto Microsoft.AspNet.OData 5.2.1 contiene gli aggiornamenti delle dipendenze di NuGet, ma non le correzioni di bug. Con questo aggiornamento, non è più una dipendenza rigida in Microsoft.OData.Core 6.4.0, ma uno possa eseguire l'aggiornamento a qualsiasi versione 6.4.0 quella 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

In questa versione è stata apportata una dipendenza di modifica per `Json.Net 6.0.4`. Per altre informazioni sulle novità in questa versione di `Json.NET`, vedere [inserimento delle dipendenze di Json.NET 6.0 versione 4 - JSON di tipo Merge,](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Questa versione non include altre nuove funzionalità o correzioni di bug nell'API Web. Successivamente, sono stati aggiornati tutti gli altri pacchetti dipendenti affinché dipenda da questa nuova versione dell'API Web.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Versione Beta Microsoft.AspNet.WebAPI 5.2.3

È possibile leggere informazioni sul rilascio [qui](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Questa versione include correzioni di bug solo. È possibile usare [questa query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) per visualizzare l'elenco dei problemi risolti in questa versione.
