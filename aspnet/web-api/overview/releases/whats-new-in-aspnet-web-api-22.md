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
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="f127d-102">Novità di ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="f127d-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="f127d-103">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f127d-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="f127d-104">In questo argomento vengono descritte le nuove per ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="f127d-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="f127d-105">Download</span><span class="sxs-lookup"><span data-stu-id="f127d-105">Download</span></span>](#download)
- [<span data-ttu-id="f127d-106">Documentazione</span><span class="sxs-lookup"><span data-stu-id="f127d-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="f127d-107">Nuove funzionalità in ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="f127d-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="f127d-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="f127d-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="f127d-109">Attributo di miglioramenti di Routing</span><span class="sxs-lookup"><span data-stu-id="f127d-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="f127d-110">Supporto di API Client Web per Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="f127d-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="f127d-111">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="f127d-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="f127d-112">Correzioni di bug</span><span class="sxs-lookup"><span data-stu-id="f127d-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="f127d-113">Italiano 5.2.1</span><span class="sxs-lookup"><span data-stu-id="f127d-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="f127d-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="f127d-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="f127d-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="f127d-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="f127d-116">Download</span><span class="sxs-lookup"><span data-stu-id="f127d-116">Download</span></span>

<span data-ttu-id="f127d-117">Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in Raccolta NuGet.</span><span class="sxs-lookup"><span data-stu-id="f127d-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="f127d-118">Seguono tutti i pacchetti di runtime di [controllo delle versioni semantico](http://semver.org/) specifica.</span><span class="sxs-lookup"><span data-stu-id="f127d-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="f127d-119">Il pacchetto più recente di ASP.NET Web API 2.2 è la seguente versione: "5.2.0".</span><span class="sxs-lookup"><span data-stu-id="f127d-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="f127d-120">È possibile installare o aggiornare i pacchetti tramite [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="f127d-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="f127d-121">Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.</span><span class="sxs-lookup"><span data-stu-id="f127d-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="f127d-122">È possibile installare o aggiornare i pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:</span><span class="sxs-lookup"><span data-stu-id="f127d-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="f127d-123">Documentazione</span><span class="sxs-lookup"><span data-stu-id="f127d-123">Documentation</span></span>

<span data-ttu-id="f127d-124">Esercitazioni e altre informazioni su ASP.NET Web API 2.2 sono disponibili dal sito web ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="f127d-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="f127d-125">Nuove funzionalità in ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="f127d-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="f127d-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="f127d-126">OData v4</span></span>

<span data-ttu-id="f127d-127">Questa versione aggiunge il supporto per il protocollo di OData v4.</span><span class="sxs-lookup"><span data-stu-id="f127d-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="f127d-128">Per ulteriori informazioni, vedere il [Web API OData v4 documentazione.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="f127d-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="f127d-129">Ecco alcune delle funzionalità chiave e le modifiche per OData v4:</span><span class="sxs-lookup"><span data-stu-id="f127d-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="f127d-130">Supporto per la proprietà alias nel modello di OData</span><span class="sxs-lookup"><span data-stu-id="f127d-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="f127d-131">Supporto per ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute e ConcurrencyCheckAttribute in ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="f127d-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="f127d-132">Consente di specificare un titolo descrittivo per le azioni</span><span class="sxs-lookup"><span data-stu-id="f127d-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="f127d-133">Integrazione con UriParser ODL</span><span class="sxs-lookup"><span data-stu-id="f127d-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="f127d-134">Supporto per [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [contenimento](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) e [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="f127d-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="f127d-135">Supporto di cui eseguire il cast per i tipi primitivi</span><span class="sxs-lookup"><span data-stu-id="f127d-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="f127d-136">Aggiunta del supporto di OData (funzione)</span><span class="sxs-lookup"><span data-stu-id="f127d-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="f127d-137">Supporta gli alias dei parametri per le chiamate di funzione</span><span class="sxs-lookup"><span data-stu-id="f127d-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="f127d-138">Supporta la convenzione di denominazione camel case nel modello</span><span class="sxs-lookup"><span data-stu-id="f127d-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="f127d-139">Supporto per cast () in $filter</span><span class="sxs-lookup"><span data-stu-id="f127d-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="f127d-140">Supporto per il tipo complesso aperto</span><span class="sxs-lookup"><span data-stu-id="f127d-140">Support for open complex type</span></span>
- <span data-ttu-id="f127d-141">Rimosso EntitySetController e AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="f127d-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="f127d-142">$Link modificate per $ref</span><span class="sxs-lookup"><span data-stu-id="f127d-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="f127d-143">Aggiunta del supporto routing attributo</span><span class="sxs-lookup"><span data-stu-id="f127d-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="f127d-144">Usa le librerie di base di OData 6.4.0</span><span class="sxs-lookup"><span data-stu-id="f127d-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="f127d-145">Attributo di miglioramenti di Routing</span><span class="sxs-lookup"><span data-stu-id="f127d-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="f127d-146">Attributo Routing ora fornisce un punto di estensibilità chiamato IDirectRouteProvider, che consente il controllo completo sulla modalità di route di attributi vengono individuate e configurate.</span><span class="sxs-lookup"><span data-stu-id="f127d-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="f127d-147">Un IDirectRouteProvider è responsabile di fornire un elenco di azioni e controller insieme alle informazioni relative all'itinerario associato per specificare esattamente quale configurazione di routing è desiderate per tali azioni.</span><span class="sxs-lookup"><span data-stu-id="f127d-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="f127d-148">Un'implementazione IDirectRouteProvider può essere specificata quando si chiama MapAttributes/MapHttpAttributeRoutes.</span><span class="sxs-lookup"><span data-stu-id="f127d-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="f127d-149">Personalizzazione IDirectRouteProvider sarà più semplice estendendo l'implementazione predefinita, DefaultDirectRouteProvider.</span><span class="sxs-lookup"><span data-stu-id="f127d-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="f127d-150">Questa classe fornisce metodi virtuali separati sottoponibile a override per modificare la logica per individuare gli attributi, la creazione di voci della route e individuare il prefisso di route e il prefisso dell'area.</span><span class="sxs-lookup"><span data-stu-id="f127d-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="f127d-151">Di seguito sono riportati alcuni esempi su cosa si può fare con questo nuovo punto di estendibilità:</span><span class="sxs-lookup"><span data-stu-id="f127d-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="f127d-152">Supporto dell'ereditarietà di attributi delle Route</span><span class="sxs-lookup"><span data-stu-id="f127d-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="f127d-153">Esempio:</span><span class="sxs-lookup"><span data-stu-id="f127d-153">Example:</span></span>

    <span data-ttu-id="f127d-154">Di seguito una richiesta like "/ api/valori/10" restituirebbe correttamente "Esito positivo: 10"</span><span class="sxs-lookup"><span data-stu-id="f127d-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="f127d-155">Fornire un nome di route predefinita per la route di attributi seguendo alcuni convenzione desiderato.</span><span class="sxs-lookup"><span data-stu-id="f127d-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="f127d-156">Per impostazione predefinita, il routing di attributo non crea automaticamente i nomi per le route di attributo.</span><span class="sxs-lookup"><span data-stu-id="f127d-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="f127d-157">Modificare il modello di route delle route di attributi in una posizione centrale prima di finire nella tabella di route.</span><span class="sxs-lookup"><span data-stu-id="f127d-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="f127d-158">Supporto di API Client Web per Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="f127d-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="f127d-159">È ora possibile utilizzare il pacchetto NuGet di Web API Client per implementare la logica di client Web API quando la destinazione è Windows Phone 8.1 o da all'interno di un'App universale.</span><span class="sxs-lookup"><span data-stu-id="f127d-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="f127d-160">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="f127d-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="f127d-161">In questa sezione vengono descritti problemi noti e modifiche di rilievo in 2.2 di API Web di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f127d-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="f127d-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="f127d-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="f127d-163">Generatore di modelli</span><span class="sxs-lookup"><span data-stu-id="f127d-163">Model builder</span></span>

<span data-ttu-id="f127d-164">Problema: Le funzioni in overload potrebbe non essere esposta come FunctionImport</span><span class="sxs-lookup"><span data-stu-id="f127d-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="f127d-165">Se sono presenti funzioni in overload 2 e sono inoltre FunctionImport come illustrato di seguito quindi richiedere risultati ~/GetAllConventionCustomers(CustomerName={customerName}) in System. InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="f127d-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="f127d-166">Soluzione alternativa: La soluzione alternativa per questo problema è aggiungere entrambi gli overload della funzione come FunctionImports.</span><span class="sxs-lookup"><span data-stu-id="f127d-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="f127d-167">Routing OData</span><span class="sxs-lookup"><span data-stu-id="f127d-167">OData Routing</span></span>

<span data-ttu-id="f127d-168">Valori letterali stringa che includono l'URL con codifica barra (% 2F) e backslash(%5C) causa un errore 404 quando vengono utilizzati nei percorsi delle risorse di OData.</span><span class="sxs-lookup"><span data-stu-id="f127d-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="f127d-169">Ad esempio, valori letterali stringa utilizzabile nei percorsi delle risorse OData come parametri di funzioni o i valori di chiave di set di entità.</span><span class="sxs-lookup"><span data-stu-id="f127d-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="f127d-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="f127d-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="f127d-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="f127d-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="f127d-172">Quando i servizi ricevono richieste l'escape di un host verrà le sequenze di escape prima di passarli al runtime di API Web.</span><span class="sxs-lookup"><span data-stu-id="f127d-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="f127d-173">Ciò consente di proteggere da attacchi simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f127d-173">This protects against attacks like the following:</span></span>  
  
 <span data-ttu-id="f127d-174">http://www.contoso.com/..%2f..%2f/Windows/System32/cmd.exe?/c+dir+c:</span><span class="sxs-lookup"><span data-stu-id="f127d-174">http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:</span></span>

<span data-ttu-id="f127d-175">In questo modo lo stack di Web API OData restituire un errore 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="f127d-175">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="f127d-176">Per evitare questo errore, il client deve utilizzare le sequenze di escape doppie per barra (% 252F) e barra rovesciata (% C 255).</span><span class="sxs-lookup"><span data-stu-id="f127d-176">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="f127d-177">Questo non accade per le stringhe di query, ad esempio /Employees? $filter = Name eq 'Nome % 2F'</span><span class="sxs-lookup"><span data-stu-id="f127d-177">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="f127d-178">**Prendere nota delle barre sottoposti a escape ('/') e le barre rovesciate (') non sono valide nei valori letterali stringa di percorso risorsa OData. Barre dovrebbero essere visualizzati solo come separatori di percorso e le barre rovesciate non devono essere visualizzati nel percorso delle risorse OData affatto. (Entrambi sono utilizzabili in alcune parti della stringa di query OData).**</span><span class="sxs-lookup"><span data-stu-id="f127d-178">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="f127d-179">Soluzione alternativa: È Impossibile sostituire il metodo di analisi di DefaultODataPathHandler per la barra e una barra rovesciata nei valori letterali stringa di escape prima effettivamente durante l'analisi.</span><span class="sxs-lookup"><span data-stu-id="f127d-179">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="f127d-180">È possibile trovare un esempio di questo approccio qui.</span><span class="sxs-lookup"><span data-stu-id="f127d-180">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="f127d-181">OData v3</span><span class="sxs-lookup"><span data-stu-id="f127d-181">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="f127d-182">[Query]</span><span class="sxs-lookup"><span data-stu-id="f127d-182">[Queryable]</span></span>

<span data-ttu-id="f127d-183">L'attributo [Queryable] è deprecato.</span><span class="sxs-lookup"><span data-stu-id="f127d-183">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="f127d-184">Nuove OData v3 applicazioni devono utilizzare **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="f127d-184">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="f127d-185">Il **ODataHttpConfigurationExtensions.EnableQuerySupport** metodo di estensione si aggiunge un **EnableQueryAttribute** all'insieme di filtri globali.</span><span class="sxs-lookup"><span data-stu-id="f127d-185">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="f127d-186">Se dispongono di tutti i controller di **[Queryable]** attributo, la chiamata `config.EnableQuerySupport()` causerà la **[Queryable]** attributo esito negativo</span><span class="sxs-lookup"><span data-stu-id="f127d-186">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="f127d-187">Il metodo consigliato per risolvere questo problema consiste nel sostituire tutte le istanze di **QueryableAttribute** con **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="f127d-187">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="f127d-188">Una soluzione alternativa consiste nell'utilizzare il codice seguente nella configurazione dell'API Web:</span><span class="sxs-lookup"><span data-stu-id="f127d-188">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="f127d-189">Routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="f127d-189">Attribute Routing</span></span>

<span data-ttu-id="f127d-190">Problema: Associazione di modelli di tipo complesso decorata con l'attributo FromUri si comporta in modo diverso quando si utilizza il Routing di attributo.</span><span class="sxs-lookup"><span data-stu-id="f127d-190">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="f127d-191">Collegamento riportato di seguito si verifica il problema e contiene anche informazioni dettagliate su una soluzione alternativa.</span><span class="sxs-lookup"><span data-stu-id="f127d-191">Following link is tracking the issue and also has details about a workaround.</span></span>  
[<span data-ttu-id="f127d-192">http://aspnetwebstack.codeplex.com/WorkItem/1944</span><span class="sxs-lookup"><span data-stu-id="f127d-192">http://aspnetwebstack.codeplex.com/workitem/1944</span></span>](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="f127d-193">Problema: Lo Scaffolding MVC o Web API in un progetto con 5.2.0 risultati di pacchetti in 5.1.2 pacchetti per quelle che non esistono già nel progetto</span><span class="sxs-lookup"><span data-stu-id="f127d-193">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="f127d-194">Aggiornamento dei pacchetti NuGet per ASP.NET MVC 5.2 non aggiornare gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET o il modello di progetto applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f127d-194">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="f127d-195">Usano la versione precedente dei pacchetti di runtime ASP.NET (ad esempio 5.1.2 in Update 2).</span><span class="sxs-lookup"><span data-stu-id="f127d-195">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="f127d-196">Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (ad esempio 5.1.2 in Update 2) di pacchetti richiesti, se non sono già disponibili nei progetti.</span><span class="sxs-lookup"><span data-stu-id="f127d-196">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="f127d-197">Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti nei progetti più recenti.</span><span class="sxs-lookup"><span data-stu-id="f127d-197">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="f127d-198">Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento di pacchetti dei progetti Web API 2.2 o ASP.NET MVC 5.2, assicurarsi che le versioni di API Web e ASP.NET MVC sono coerenti.</span><span class="sxs-lookup"><span data-stu-id="f127d-198">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="f127d-199">Correzioni di bug e aggiornamenti delle funzionalità secondarie</span><span class="sxs-lookup"><span data-stu-id="f127d-199">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="f127d-200">Questa versione include inoltre diverse correzioni di bug e le funzionalità secondarie gli aggiornamenti.</span><span class="sxs-lookup"><span data-stu-id="f127d-200">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="f127d-201">È possibile trovare l'elenco completo di seguito:</span><span class="sxs-lookup"><span data-stu-id="f127d-201">You can find the complete list here:</span></span>

- [<span data-ttu-id="f127d-202">pacchetto 5.2</span><span class="sxs-lookup"><span data-stu-id="f127d-202">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="f127d-203">Italiano 5.2.1</span><span class="sxs-lookup"><span data-stu-id="f127d-203">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="f127d-204">Il pacchetto italiano 5.2.1 contiene aggiornamenti dipendenza NuGet ma nessun correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="f127d-204">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="f127d-205">Con questo aggiornamento, non è più una dipendenza rigida in Microsoft.OData.Core 6.4.0, ma una possibile eseguire l'aggiornamento a una versione qualsiasi tra 6.4.0 e 7.0.0.</span><span class="sxs-lookup"><span data-stu-id="f127d-205">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="f127d-206">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="f127d-206">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="f127d-207">In questa versione è stata introdotta una dipendenza modificare per `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="f127d-207">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="f127d-208">Per ulteriori informazioni sulle novità in questa versione di `Json.NET`, vedere [Json.NET 6.0 versione 4 - Merge JSON, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f127d-208">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="f127d-209">Questa versione non dispone di altre funzionalità nuove o correzioni di bug nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="f127d-209">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="f127d-210">Sono state aggiornate successivamente tutti gli altri pacchetti dipendente che è proprietario per dipendere questa nuova versione dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="f127d-210">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="f127d-211">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="f127d-211">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="f127d-212">È possibile leggere informazioni sulle versioni [qui](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="f127d-212">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="f127d-213">Questa versione include solo correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="f127d-213">This release contains only bug fixes.</span></span> <span data-ttu-id="f127d-214">È possibile utilizzare [questa query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) per visualizzare l'elenco dei problemi risolti in questa versione.</span><span class="sxs-lookup"><span data-stu-id="f127d-214">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
