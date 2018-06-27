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
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="be14b-102">Che cosa è stato introdotta in ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="be14b-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="be14b-103">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="be14b-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="be14b-104">In questo argomento sono descritte le novità per ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="be14b-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="be14b-105">Download</span><span class="sxs-lookup"><span data-stu-id="be14b-105">Download</span></span>](#download)
- [<span data-ttu-id="be14b-106">Documentazione</span><span class="sxs-lookup"><span data-stu-id="be14b-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="be14b-107">Nuove funzionalità in ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="be14b-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="be14b-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="be14b-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="be14b-109">Attributo miglioramenti di Routing</span><span class="sxs-lookup"><span data-stu-id="be14b-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="be14b-110">Supporto Client per le API Web per Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="be14b-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="be14b-111">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="be14b-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="be14b-112">Correzioni di bug</span><span class="sxs-lookup"><span data-stu-id="be14b-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="be14b-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="be14b-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="be14b-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="be14b-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="be14b-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="be14b-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="be14b-116">Download</span><span class="sxs-lookup"><span data-stu-id="be14b-116">Download</span></span>

<span data-ttu-id="be14b-117">Le funzionalità di runtime vengono rilasciate come dei pacchetti NuGet in Raccolta NuGet.</span><span class="sxs-lookup"><span data-stu-id="be14b-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="be14b-118">Seguono tutti i pacchetti di runtime di [controllo delle versioni semantico](http://semver.org/) specifica.</span><span class="sxs-lookup"><span data-stu-id="be14b-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="be14b-119">Il pacchetto più recente di ASP.NET Web API 2.2 è la seguente versione: "5.2.0".</span><span class="sxs-lookup"><span data-stu-id="be14b-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="be14b-120">È possibile installare o aggiornare i pacchetti attraverso [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="be14b-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="be14b-121">Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.</span><span class="sxs-lookup"><span data-stu-id="be14b-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="be14b-122">È possibile installare o aggiornare i pacchetti NuGet rilasciati mediante la Console di gestione pacchetti NuGet:</span><span class="sxs-lookup"><span data-stu-id="be14b-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="be14b-123">Documentazione</span><span class="sxs-lookup"><span data-stu-id="be14b-123">Documentation</span></span>

<span data-ttu-id="be14b-124">Esercitazioni e altre informazioni su ASP.NET Web API 2.2 sono disponibili dal sito web ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="be14b-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="be14b-125">Nuove funzionalità in ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="be14b-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="be14b-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="be14b-126">OData v4</span></span>

<span data-ttu-id="be14b-127">Questa versione aggiunge il supporto per il protocollo di OData v4.</span><span class="sxs-lookup"><span data-stu-id="be14b-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="be14b-128">Per altre informazioni, vedere il [Web API OData v4 documentazione.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="be14b-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="be14b-129">Di seguito sono riportate alcune funzionalità chiave e le modifiche per OData v4:</span><span class="sxs-lookup"><span data-stu-id="be14b-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="be14b-130">Supporto per le proprietà alias nel modello di OData</span><span class="sxs-lookup"><span data-stu-id="be14b-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="be14b-131">Supporto per ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute e ConcurrencyCheckAttribute in ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="be14b-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="be14b-132">Offrono possibilità di specificare un titolo descrittivo per le azioni</span><span class="sxs-lookup"><span data-stu-id="be14b-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="be14b-133">Integrazione con UriParser ODL</span><span class="sxs-lookup"><span data-stu-id="be14b-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="be14b-134">Supporto per [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [contenimento](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) e [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="be14b-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="be14b-135">Supporto per eseguire il cast per i tipi primitivi</span><span class="sxs-lookup"><span data-stu-id="be14b-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="be14b-136">Aggiunta del supporto di OData (funzione)</span><span class="sxs-lookup"><span data-stu-id="be14b-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="be14b-137">Supporta gli alias dei parametri per le chiamate di funzione</span><span class="sxs-lookup"><span data-stu-id="be14b-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="be14b-138">Supporta la convenzione di denominazione maiuscole iniziali nel modello</span><span class="sxs-lookup"><span data-stu-id="be14b-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="be14b-139">Supporto per cast () in $filter</span><span class="sxs-lookup"><span data-stu-id="be14b-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="be14b-140">Supporto per il tipo complesso aperto</span><span class="sxs-lookup"><span data-stu-id="be14b-140">Support for open complex type</span></span>
- <span data-ttu-id="be14b-141">EntitySetController rimosso e AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="be14b-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="be14b-142">$Link modificate a $ref</span><span class="sxs-lookup"><span data-stu-id="be14b-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="be14b-143">Aggiunta del supporto routing attributo</span><span class="sxs-lookup"><span data-stu-id="be14b-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="be14b-144">Usa le librerie di base di OData 6.4.0</span><span class="sxs-lookup"><span data-stu-id="be14b-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="be14b-145">Attributo miglioramenti di Routing</span><span class="sxs-lookup"><span data-stu-id="be14b-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="be14b-146">Attributo Routing ora fornisce un punto di estensibilità chiamato IDirectRouteProvider, consentendo a controllo completo sulla modalità di individuazione e configurate route di attributi.</span><span class="sxs-lookup"><span data-stu-id="be14b-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="be14b-147">Un IDirectRouteProvider è responsabile di fornire un elenco di azioni e controller insieme alle informazioni di route associata per specificare esattamente quale configurazione di routing è desiderate per tali azioni.</span><span class="sxs-lookup"><span data-stu-id="be14b-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="be14b-148">Un'implementazione IDirectRouteProvider può essere specificata quando si chiama MapAttributes/MapHttpAttributeRoutes.</span><span class="sxs-lookup"><span data-stu-id="be14b-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="be14b-149">Personalizzazione IDirectRouteProvider sarà più semplice estendendo l'implementazione predefinita, DefaultDirectRouteProvider.</span><span class="sxs-lookup"><span data-stu-id="be14b-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="be14b-150">Questa classe fornisce metodi separati virtuali sottoponibile a override per modificare la logica per l'individuazione di attributi, la creazione di voci della route e individuare il prefisso di route e il prefisso dell'area.</span><span class="sxs-lookup"><span data-stu-id="be14b-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="be14b-151">Di seguito sono riportati alcuni esempi su cosa si può fare con il nuovo punto di estendibilità:</span><span class="sxs-lookup"><span data-stu-id="be14b-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="be14b-152">Supportano l'ereditarietà di attributi delle Route</span><span class="sxs-lookup"><span data-stu-id="be14b-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="be14b-153">Esempio:</span><span class="sxs-lookup"><span data-stu-id="be14b-153">Example:</span></span>

    <span data-ttu-id="be14b-154">Di seguito una richiesta like "/ api/valori/10" restituirebbe correttamente "Operazione riuscita: 10"</span><span class="sxs-lookup"><span data-stu-id="be14b-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="be14b-155">Fornire un nome di route predefinita per la route di attributi seguendo alcuni convenzione desiderato.</span><span class="sxs-lookup"><span data-stu-id="be14b-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="be14b-156">Per impostazione predefinita, routing degli attributi non crea automaticamente i nomi per le route di attributo.</span><span class="sxs-lookup"><span data-stu-id="be14b-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="be14b-157">Modificare modello di route le route attributo in una posizione centrale prima che finiscono con nella tabella di route.</span><span class="sxs-lookup"><span data-stu-id="be14b-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="be14b-158">Supporto dei Client per le API Web per Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="be14b-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="be14b-159">È ora possibile usare il pacchetto NuGet di Web API Client per implementare la logica di client Web API quando la destinazione è Windows Phone 8.1 o dall'interno di un'App universale.</span><span class="sxs-lookup"><span data-stu-id="be14b-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="be14b-160">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="be14b-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="be14b-161">In questa sezione vengono descritti problemi noti e modifiche di rilievo nel 2.2 di API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="be14b-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="be14b-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="be14b-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="be14b-163">Generatore di modelli</span><span class="sxs-lookup"><span data-stu-id="be14b-163">Model builder</span></span>

<span data-ttu-id="be14b-164">Problema: Le funzioni in overload potrebbe non essere esposto come FunctionImport</span><span class="sxs-lookup"><span data-stu-id="be14b-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="be14b-165">Se sono presenti funzioni in overload 2 e sono inoltre FunctionImport come mostrato di seguito quindi richiedere risultati ~/GetAllConventionCustomers(CustomerName={customerName}) in System. InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="be14b-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="be14b-166">Soluzione alternativa: La soluzione alternativa per questo problema consiste nell'aggiungere entrambi gli overload della funzione come FunctionImports.</span><span class="sxs-lookup"><span data-stu-id="be14b-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="be14b-167">Routing OData</span><span class="sxs-lookup"><span data-stu-id="be14b-167">OData Routing</span></span>

<span data-ttu-id="be14b-168">Valori letterali stringa che includono l'URL codificato barra (% 2F) e backslash(%5C) causa un errore 404 quando vengono utilizzati nei percorsi delle risorse OData.</span><span class="sxs-lookup"><span data-stu-id="be14b-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="be14b-169">Ad esempio, i valori letterali stringa utilizzabile nei percorsi delle risorse OData come parametri delle funzioni o i valori chiavi del set di entità.</span><span class="sxs-lookup"><span data-stu-id="be14b-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="be14b-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="be14b-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="be14b-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="be14b-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="be14b-172">Quando i servizi ricevono l'escape di annullare la host verranno richieste le sequenze di escape prima di passarli al runtime di API Web.</span><span class="sxs-lookup"><span data-stu-id="be14b-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="be14b-173">Ciò consente di proteggere da attacchi di tipo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="be14b-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="be14b-174">In questo modo lo stack di Web API OData restituire un errore 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="be14b-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="be14b-175">Per evitare questo errore, il client deve usare le sequenze di escape doppia per barra (% 252F) e barra rovesciata (% C 255).</span><span class="sxs-lookup"><span data-stu-id="be14b-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="be14b-176">Questo non accade per le stringhe di query, ad esempio /Employees? $filter = Name eq 'Nome % 2F'</span><span class="sxs-lookup"><span data-stu-id="be14b-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="be14b-177">**Prendere nota delle barre sottoposti a escape ('/') e barre rovesciate (") non sono valide nei valori letterali stringa di percorso risorse di OData. Le barre dovrebbero essere visualizzati solo come separatori di percorso e le barre rovesciate non devono essere visualizzati nel percorso delle risorse OData affatto. (Entrambi sono utilizzabili in alcune parti della stringa di query OData).**</span><span class="sxs-lookup"><span data-stu-id="be14b-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="be14b-178">Soluzione alternativa: È eseguire l'override del metodo Parse di DefaultODataPathHandler per la barra e una barra rovesciata nei valori letterali stringa di escape prima effettivamente durante l'analisi.</span><span class="sxs-lookup"><span data-stu-id="be14b-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="be14b-179">È possibile trovare un esempio di questo approccio qui.</span><span class="sxs-lookup"><span data-stu-id="be14b-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="be14b-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="be14b-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="be14b-181">[Disponibile per query]</span><span class="sxs-lookup"><span data-stu-id="be14b-181">[Queryable]</span></span>

<span data-ttu-id="be14b-182">L'attributo [Queryable] è deprecato.</span><span class="sxs-lookup"><span data-stu-id="be14b-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="be14b-183">Nuove OData v3 applicazioni devono utilizzare **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="be14b-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="be14b-184">Il **ODataHttpConfigurationExtensions.EnableQuerySupport** metodo di estensione si aggiunge un **EnableQueryAttribute** all'insieme di filtri globali.</span><span class="sxs-lookup"><span data-stu-id="be14b-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="be14b-185">La presenza di eventuali controller il **[Queryable]** attributo, la chiamata `config.EnableQuerySupport()` determinerà il **[Queryable]** attributo esito negativo</span><span class="sxs-lookup"><span data-stu-id="be14b-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="be14b-186">Il metodo consigliato per risolvere questo problema è per sostituire tutte le istanze di **QueryableAttribute** con **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="be14b-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="be14b-187">Una soluzione alternativa consiste nell'utilizzare il codice seguente nella configurazione dell'API Web:</span><span class="sxs-lookup"><span data-stu-id="be14b-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="be14b-188">Routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="be14b-188">Attribute Routing</span></span>

<span data-ttu-id="be14b-189">Problema: Associazione di modelli di tipo complesso che è decorato con attributi FromUri si comporta in modo diverso quando si utilizza Routing degli attributi.</span><span class="sxs-lookup"><span data-stu-id="be14b-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="be14b-190">Collegamento seguente fa riferimento al monitoraggio il problema e dispone inoltre informazioni dettagliate su una soluzione alternativa.</span><span class="sxs-lookup"><span data-stu-id="be14b-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="be14b-191">Problema: Lo Scaffolding MVC o Web API in un progetto con 5.2.0 i risultati di pacchetti in 5.1.2 pacchetti per quelle che non esistono già nel progetto</span><span class="sxs-lookup"><span data-stu-id="be14b-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="be14b-192">Aggiornamento dei pacchetti NuGet per ASP.NET MVC 5.2 non aggiornare gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET o il modello di progetto applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="be14b-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="be14b-193">Usano la versione precedente dei pacchetti di runtime ASP.NET (ad esempio 5.1.2 in Update 2).</span><span class="sxs-lookup"><span data-stu-id="be14b-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="be14b-194">Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (ad esempio 5.1.2 in Update 2) di pacchetti richiesti, se non sono già disponibili nei progetti.</span><span class="sxs-lookup"><span data-stu-id="be14b-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="be14b-195">Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti più recenti nei progetti.</span><span class="sxs-lookup"><span data-stu-id="be14b-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="be14b-196">Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento di pacchetti dei progetti Web API 2.2 o ASP.NET MVC 5.2, assicurarsi che le versioni di API Web e ASP.NET MVC sono coerenti.</span><span class="sxs-lookup"><span data-stu-id="be14b-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="be14b-197">Correzioni di bug e aggiornamenti delle funzionalità secondarie</span><span class="sxs-lookup"><span data-stu-id="be14b-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="be14b-198">Questa versione include inoltre diverse correzioni di bug e le funzionalità secondarie gli aggiornamenti.</span><span class="sxs-lookup"><span data-stu-id="be14b-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="be14b-199">È possibile trovare l'elenco completo di seguito:</span><span class="sxs-lookup"><span data-stu-id="be14b-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="be14b-200">pacchetto 5.2</span><span class="sxs-lookup"><span data-stu-id="be14b-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="be14b-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="be14b-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="be14b-202">Il pacchetto Microsoft.AspNet.OData 5.2.1 contiene aggiornamenti dipendenza NuGet ma nessun correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="be14b-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="be14b-203">Con questo aggiornamento, non è più una dipendenza rigida in Microsoft.OData.Core 6.4.0, ma uno può eseguire l'aggiornamento a qualsiasi versione tra 6.4.0 e 7.0.0.</span><span class="sxs-lookup"><span data-stu-id="be14b-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="be14b-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="be14b-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="be14b-205">In questa versione è stata introdotta una dipendenza modificare per `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="be14b-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="be14b-206">Per ulteriori informazioni sulle novità in questa versione di `Json.NET`, vedere [Json.NET 6.0 versione 4 - esegue il Merge JSON, inserimento di dipendenze](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="be14b-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="be14b-207">Questa versione non dispone di altre funzionalità nuove o correzioni di bug in Web API.</span><span class="sxs-lookup"><span data-stu-id="be14b-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="be14b-208">È stata aggiornata successivamente tutti gli altri pacchetti dipendente che è proprietario affinché dipenda da questa nuova versione dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="be14b-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="be14b-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="be14b-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="be14b-210">È possibile leggere sulla versione [qui](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="be14b-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="be14b-211">Questa versione include solo correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="be14b-211">This release contains only bug fixes.</span></span> <span data-ttu-id="be14b-212">È possibile utilizzare [questa query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) per visualizzare l'elenco di problemi risolti in questa versione.</span><span class="sxs-lookup"><span data-stu-id="be14b-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
