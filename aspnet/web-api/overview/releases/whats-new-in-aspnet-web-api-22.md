---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Novità di ASP.NET Web API 2.2 | Documenti Microsoft
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
ms.openlocfilehash: 400329dd852ca3c527387ee45e3e902b725e771b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508400"
---
<a name="whats-new-in-aspnet-web-api-22"></a>Novità di ASP.NET Web API 2.2
====================
da [Microsoft](https://github.com/microsoft)

In questo argomento vengono descritte le nuove per ASP.NET Web API 2.2.

- [Download](#download)
- [Documentazione](#documentation)
- [Nuove funzionalità in ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Attributo di miglioramenti di Routing](#ARI)
    - [Supporto di API Client Web per Windows Phone 8.1](#phone)
- [Problemi noti e modifiche di rilievo](#known-issues)
- [Correzioni di bug](#bug-fixes)
- [Italiano 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in Raccolta NuGet. Seguono tutti i pacchetti di runtime di [controllo delle versioni semantico](http://semver.org/) specifica. Il pacchetto più recente di ASP.NET Web API 2.2 è la seguente versione: "5.2.0". È possibile installare o aggiornare i pacchetti tramite [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.

È possibile installare o aggiornare i pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET Web API 2.2 sono disponibili dal sito web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Nuove funzionalità in ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Questa versione aggiunge il supporto per il protocollo di OData v4. Per ulteriori informazioni, vedere il [Web API OData v4 documentazione.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Ecco alcune delle funzionalità chiave e le modifiche per OData v4:

- [Supporto per la proprietà alias nel modello di OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Supporto per ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute e ConcurrencyCheckAttribute in ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Consente di specificare un titolo descrittivo per le azioni](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Integrazione con UriParser ODL
- Supporto per [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [contenimento](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) e [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Supporto di cui eseguire il cast per i tipi primitivi
- [Aggiunta del supporto di OData (funzione)](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Supporta gli alias dei parametri per le chiamate di funzione](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Supporta la convenzione di denominazione camel case nel modello](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Supporto per cast () in $filter
- Supporto per il tipo complesso aperto
- Rimosso EntitySetController e AsyncEntitySetController
- [$Link modificate per $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Aggiunta del supporto routing attributo](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Usa le librerie di base di OData 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Attributo di miglioramenti di Routing

Attributo Routing ora fornisce un punto di estensibilità chiamato IDirectRouteProvider, che consente il controllo completo sulla modalità di route di attributi vengono individuate e configurate. Un IDirectRouteProvider è responsabile di fornire un elenco di azioni e controller insieme alle informazioni relative all'itinerario associato per specificare esattamente quale configurazione di routing è desiderate per tali azioni. Un'implementazione IDirectRouteProvider può essere specificata quando si chiama MapAttributes/MapHttpAttributeRoutes.

Personalizzazione IDirectRouteProvider sarà più semplice estendendo l'implementazione predefinita, DefaultDirectRouteProvider. Questa classe fornisce metodi virtuali separati sottoponibile a override per modificare la logica per individuare gli attributi, la creazione di voci della route e individuare il prefisso di route e il prefisso dell'area.

Di seguito sono riportati alcuni esempi su cosa si può fare con questo nuovo punto di estendibilità:

1. Supporto dell'ereditarietà di attributi delle Route

    Esempio:

    Di seguito una richiesta like "/ api/valori/10" restituirebbe correttamente "Esito positivo: 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Fornire un nome di route predefinita per la route di attributi seguendo alcuni convenzione desiderato. Per impostazione predefinita, il routing di attributo non crea automaticamente i nomi per le route di attributo.
3. Modificare il modello di route delle route di attributi in una posizione centrale prima di finire nella tabella di route.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Supporto di API Client Web per Windows Phone 8.1

È ora possibile utilizzare il pacchetto NuGet di Web API Client per implementare la logica di client Web API quando la destinazione è Windows Phone 8.1 o da all'interno di un'App universale.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti problemi noti e modifiche di rilievo in 2.2 di API Web di ASP.NET.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Generatore di modelli

Problema: Le funzioni in overload potrebbe non essere esposta come FunctionImport

Se sono presenti funzioni in overload 2 e sono inoltre FunctionImport come illustrato di seguito quindi richiedere risultati ~/GetAllConventionCustomers(CustomerName={customerName}) in System. InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Soluzione alternativa: La soluzione alternativa per questo problema è aggiungere entrambi gli overload della funzione come FunctionImports.

#### <a name="odata-routing"></a>Routing OData

Valori letterali stringa che includono l'URL con codifica barra (% 2F) e backslash(%5C) causa un errore 404 quando vengono utilizzati nei percorsi delle risorse di OData.

Ad esempio, valori letterali stringa utilizzabile nei percorsi delle risorse OData come parametri di funzioni o i valori di chiave di set di entità.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Quando i servizi ricevono richieste l'escape di un host verrà le sequenze di escape prima di passarli al runtime di API Web. Ciò consente di proteggere da attacchi simile al seguente:  
  
 http://www.contoso.com/..%2f..%2f/Windows/System32/cmd.exe?/c+dir+c:

In questo modo lo stack di Web API OData restituire un errore 404 (non trovato). Per evitare questo errore, il client deve utilizzare le sequenze di escape doppie per barra (% 252F) e barra rovesciata (% C 255). Questo non accade per le stringhe di query, ad esempio /Employees? $filter = Name eq 'Nome % 2F'

**Prendere nota delle barre sottoposti a escape ('/') e le barre rovesciate (') non sono valide nei valori letterali stringa di percorso risorsa OData. Barre dovrebbero essere visualizzati solo come separatori di percorso e le barre rovesciate non devono essere visualizzati nel percorso delle risorse OData affatto. (Entrambi sono utilizzabili in alcune parti della stringa di query OData).**

Soluzione alternativa: È Impossibile sostituire il metodo di analisi di DefaultODataPathHandler per la barra e una barra rovesciata nei valori letterali stringa di escape prima effettivamente durante l'analisi. È possibile trovare un esempio di questo approccio qui.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Query]

L'attributo [Queryable] è deprecato. Nuove OData v3 applicazioni devono utilizzare **System.Web.Http.OData.EnableQueryAttribute**.

Il **ODataHttpConfigurationExtensions.EnableQuerySupport** metodo di estensione si aggiunge un **EnableQueryAttribute** all'insieme di filtri globali. Se dispongono di tutti i controller di **[Queryable]** attributo, la chiamata `config.EnableQuerySupport()` causerà la **[Queryable]** attributo esito negativo

Il metodo consigliato per risolvere questo problema consiste nel sostituire tutte le istanze di **QueryableAttribute** con **System.Web.Http.OData.EnableQueryAttribute**.

Una soluzione alternativa consiste nell'utilizzare il codice seguente nella configurazione dell'API Web:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Routing degli attributi

Problema: Associazione di modelli di tipo complesso decorata con l'attributo FromUri si comporta in modo diverso quando si utilizza il Routing di attributo.

Collegamento riportato di seguito si verifica il problema e contiene anche informazioni dettagliate su una soluzione alternativa.  
[http://aspnetwebstack.codeplex.com/WorkItem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problema: Lo Scaffolding MVC o Web API in un progetto con 5.2.0 risultati di pacchetti in 5.1.2 pacchetti per quelle che non esistono già nel progetto

Aggiornamento dei pacchetti NuGet per ASP.NET MVC 5.2 non aggiornare gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET o il modello di progetto applicazione Web ASP.NET. Usano la versione precedente dei pacchetti di runtime ASP.NET (ad esempio 5.1.2 in Update 2). Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (ad esempio 5.1.2 in Update 2) di pacchetti richiesti, se non sono già disponibili nei progetti. Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti nei progetti più recenti. Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento di pacchetti dei progetti Web API 2.2 o ASP.NET MVC 5.2, assicurarsi che le versioni di API Web e ASP.NET MVC sono coerenti.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correzioni di bug e aggiornamenti delle funzionalità secondarie

Questa versione include inoltre diverse correzioni di bug e le funzionalità secondarie gli aggiornamenti. È possibile trovare l'elenco completo di seguito:

- [pacchetto 5.2](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Italiano 5.2.1

Il pacchetto italiano 5.2.1 contiene aggiornamenti dipendenza NuGet ma nessun correzioni di bug. Con questo aggiornamento, non è più una dipendenza rigida in Microsoft.OData.Core 6.4.0, ma una possibile eseguire l'aggiornamento a una versione qualsiasi tra 6.4.0 e 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

In questa versione è stata introdotta una dipendenza modificare per `Json.Net 6.0.4`. Per ulteriori informazioni sulle novità in questa versione di `Json.NET`, vedere [Json.NET 6.0 versione 4 - Merge JSON, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Questa versione non dispone di altre funzionalità nuove o correzioni di bug nell'API Web. Sono state aggiornate successivamente tutti gli altri pacchetti dipendente che è proprietario per dipendere questa nuova versione dell'API Web.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

È possibile leggere informazioni sulle versioni [qui](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Questa versione include solo correzioni di bug. È possibile utilizzare [questa query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) per visualizzare l'elenco dei problemi risolti in questa versione.
