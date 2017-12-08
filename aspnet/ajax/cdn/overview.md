---
uid: ajax/cdn/overview
title: Microsoft Ajax Content Delivery Network | Documenti Microsoft
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 2955888bc745fa3d49e40d364196283f16ad95ef
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2017
---
<a name="microsoft-ajax-content-delivery-network"></a>Microsoft Ajax Content Delivery Network
====================
Nota: La rete CDN di Microsoft Ajax non dispone di alcun contratto di servizio dopo l'utilizzo di una rete CDN di Azure.

## <a name="table-of-contents"></a>Sommario

**[AJAX.microsoft.com rinominato in ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**  
**[Supporto di Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**  
**[Utilizzando ASP.NET Ajax dalla rete CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**  
**[Tramite jQuery dalla rete CDN](#Using_jQuery_from_the_CDN_21)**  
**[Utilizzo di jQuery UI dalla rete CDN](#Using_jQuery_UI_from_the_CDN_22)**  
**[File di terze parti nella rete CDN](#Third-Party_Files_on_the_CDN_23)**  
  
 [Versioni di jQuery nella rete CDN](#jQuery_Releases_on_the_CDN_0)  
 [Versioni di eseguire la migrazione di jQuery nella rete CDN](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [jQuery versioni dell'interfaccia utente nella rete CDN](#jQuery_UI_Releases_on_the_CDN_2)  
 [Versioni di convalida nella rete CDN jQuery](#jQuery_Validation_Releases_on_the_CDN_3)  
 [jQuery Mobile versioni nella rete CDN](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [jQuery rilasci di modelli di rete CDN](#jQuery_Templates_Releases_on_the_CDN_5)  
 [jQuery versioni ciclo nella rete CDN](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [jQuery versioni DataTable nella rete CDN](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [Versioni di Modernizr nella rete CDN](#Modernizr_Releases_on_the_CDN_8)  
 [Versioni di JSHint nella rete CDN](#JSHint_Releases_on_the_CDN_10)  
 [Versioni di Knockout nella rete CDN](#Knockout_Releases_on_the_CDN_11)  
 [Globalizzare le versioni su una rete CDN](#Globalize_Releases_on_the_CDN_12)  
 [Rispondere versioni su una rete CDN](#Respond_Releases_on_the_CDN_13)  
 [Versioni di bootstrap nella rete CDN](#Bootstrap_Releases_on_the_CDN_14)  
 [Versioni di bootstrap TouchCarousel nella rete CDN](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [Versioni di Hammer.js nella rete CDN](#Hammerjs_Releases_on_the_CDN_19)  
 [Web Form ASP.NET e Ajax versioni nella rete CDN](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [ASP.NET MVC rilascia nella rete CDN](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [ASP.NET SignalR rilascia nella rete CDN](#ASPNET_SignalR_Releases_on_the_CDN_17)

Il Microsoft Ajax rete CDN (Content Delivery) ospita le librerie JavaScript più diffusi di terze parti, ad esempio jQuery e consente di aggiungere facilmente alle applicazioni Web. Ad esempio, iniziare a utilizzare jQuery che è ospitato in questa rete CDN semplicemente aggiungendo un &lt;script&gt; tag a una pagina che punta a ajax.aspnetcdn.com.

È possibile sfruttare la rete CDN, è possibile migliorare notevolmente le prestazioni delle applicazioni Ajax. Il contenuto della rete CDN viene memorizzati nella cache nei server presenti in tutto il mondo. Inoltre, la rete CDN consente ai browser di riutilizzare i file JavaScript memorizzati nella cache di terze parti per siti web che si trovano in domini diversi.

La rete CDN supporta SSL (HTTPS), nel caso in cui sia necessario presentare una pagina web utilizzando Secure Sockets Layer.

La rete CDN ospita le seguenti librerie di script di terze parti che sono state caricate e sono concessi in licenza, ai proprietari di tali raccolte:

- jQuery (www.jquery.com)
- jQuery UI (www.jqueryui.com)
- jQuery Mobile (www.jquerymobile.com)
- jQuery convalida (www.jquery.com)
- jQuery ciclo (www.malsup.com/jquery/cycle/)
- jQuery DataTable (http://datatables.net/)

Rete CDN di Microsoft Ajax include anche le librerie seguenti che sono state caricate da Microsoft:

- ASP.NET Ajax
- File JavaScript di ASP.NET MVC
- File di ASP.NET SignalR JavaScript

Microsoft non rivendica la proprietà di tutte le librerie di terze parti ospitate in questa rete CDN. I proprietari delle librerie di copyright licenza queste librerie all'utente. Eventuali diritti che è possibile scaricare e usare tali librerie vengono concesse unicamente dai rispettivi proprietari copyright. Poiché questi non sono librerie di Microsoft, Microsoft non offre alcuna garanzia o licenze di diritti di proprietà intellettuale (non incluso alcun diritto di brevetto implicito) per le librerie di terze parti ospitate in questa rete CDN.

Se si desidera inviare la libreria JavaScript e la libreria è uno dei superiore estensioni/plug-in a queste librerie o librerie JavaScript (come indicato in http://trends.builtwith.com) (a) più diffusi; (b) o utili per utilizzare in ASP.NET, quindi contattare AjaxCDNSubmission@Microsoft.com.

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a>AJAX.microsoft.com rinominato in ajax.aspnetcdn.com

La rete CDN consentono di utilizzare il nome di dominio microsoft.com ed è stata modificata per utilizzare il nome di dominio aspnetcdn.com. Questa modifica è stata apportata per migliorare le prestazioni, perché quando un browser a cui fa riferimento il dominio microsoft.com invierebbe i cookie da quel dominio in rete con ogni richiesta. È stato rinominato in un nome di dominio diverso da microsoft.com prestazioni possono essere incrementata quanto a 25%. Si noti ajax.microsoft.com continuerà a funzionare, ma è consigliabile ajax.aspnetcdn.com.

- Formato precedente: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js
- Nuovo formato: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a>Supporto di Visual Studio .vsdoc

Per utilizzare correttamente i file .vsdoc con Visual Studio 2008, è necessario assicurarsi di disporre di Visual Studio 2008 SP1 installato e l'installazione dell'hotfix per i file vsdoc. È possibile ottenere questi da qui:

- [Scaricare Visual Studio 2008 SP1](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "scaricare Visual Studio 2008 SP1")
- [Scaricare l'hotfix .vsdoc per Visual Studio 2008 SP1](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "scaricare .vsdoc hotfix per Visual Studio 2008 SP1")

Visual Studio 2010 supporta file .vsdoc senza tutte le patch aggiuntive.

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a>Utilizzando ASP.NET Ajax dalla rete CDN

Quando si utilizza ASP.NET 4, è possibile reindirizzare tutte le richieste per gli script del framework ASP.NET per la rete CDN. Il recupero degli script dalla rete CDN anziché il server web locale può sostanzialmente di migliorare le prestazioni dei siti Web ASP.NET pubblico.

Utilizzare la proprietà ScriptManager EnableCDN per reindirizzare tutte le richieste di script framework ASP.NET Ajax CDN Microsoft:

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a>Tramite jQuery dalla rete CDN

È possibile utilizzare script jQuery ospitato nella rete CDN nell'applicazione Web aggiungendo il seguente elemento script a una pagina:

[!code-html[Main](overview/samples/sample2.html)]

La rete CDN include anche la versione minimizzata dello script jQuery, che è possibile ottenere utilizzando l'elemento seguente:

[!code-html[Main](overview/samples/sample3.html)]

Per consentire alla pagina di fallback per il caricamento di jQuery da un percorso locale nel proprio sito Web, se la rete CDN non disponibile, aggiungere l'elemento seguente immediatamente dopo l'elemento che fa riferimento la rete CDN:

[!code-html[Main](overview/samples/sample4.html)]

La pagina di esempio seguente usa la versione della rete CDN della libreria jQuery (con fallback a una copia locale) per visualizzare il contenuto di un elemento div quando viene premuto un pulsante.

[!code-html[Main](overview/samples/sample5.html)]

È possibile approfondire la conoscenza di jQuery e scaricare una copia locale di jQuery visitando il [jQuery](http://jquery.com/) sito Web.

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a>Utilizzo di jQuery UI dalla rete CDN

La rete CDN ospita anche la libreria dell'interfaccia utente di jQuery. La libreria dell'interfaccia utente jQuery include una vasta gamma di widget e gli effetti che è possibile utilizzare nelle applicazioni ASP.NET. Ad esempio, la pagina seguente viene illustrato come è possibile utilizzare jQuery UI Datepicker nel contesto di un'applicazione Web Form ASP.NET per visualizzare un calendario popup:

[!code-aspx[Main](overview/samples/sample6.aspx)]

Quando si sposta lo stato attivo al controllo TextBox con la tastiera, viene visualizzato un calendario:

![Calendario popup creato con Datepicker](overview/_static/image1.png)

Si noti che è necessario includere tre file dalla rete CDN nel codice precedente:

- La libreria jQuery &mdash; libreria jQuery UI dipende la libreria jQuery. È necessario aggiungere la libreria jQuery alla pagina prima di aggiungere la libreria dell'interfaccia utente di jQuery.
- La libreria dell'interfaccia utente jQuery &mdash; libreria jQuery UI contiene tutti gli effetti dell'interfaccia utente jQuery e widget, ad esempio il widget Datepicker utilizzate nella pagina precedente.
- Un tema dell'interfaccia utente jQuery &mdash; jQuery UI supporta diversi temi. La pagina precedente include un collegamento a un file CSS per importare il tema di Redmond.

Tutti i temi di jQuery standard dell'interfaccia utente sono ospitati nella rete CDN. [Visitare questa pagina](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sulla rete CDN di Microsoft Ajax") per visualizzare le anteprime per ognuno di essi.

Per ulteriori informazioni sulla libreria jQuery UI, visitare ufficiali [sito Web di jQuery UI](http://jQueryUI.com "sito Web di jQuery UI").

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a>File di terze parti nella rete CDN

La rete CDN ospita alcune delle librerie di JavaScript più diffusi di terze parti. Microsoft non rivendica la proprietà di tutte le librerie di terze parti ospitate in questa rete CDN. I proprietari delle librerie di copyright licenza queste librerie all'utente. Eventuali diritti che è possibile scaricare e usare tali librerie vengono concesse unicamente dai rispettivi proprietari copyright. Poiché questi non sono librerie di Microsoft, Microsoft non offre alcuna garanzia o licenze di diritti di proprietà intellettuale (non incluso alcun diritto di brevetto implicito) per le librerie di terze parti ospitate in questa rete CDN.

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a>Versioni di jQuery nella rete CDN

Le versioni seguenti di jQuery ospitate nella rete CDN:

#### <a name="jquery-version-321"></a>versione di jQuery 3.2.1
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.Map
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.Map

#### <a name="jquery-version-320"></a>versione di jQuery 3.2.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.Map
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.Map

#### <a name="jquery-version-311"></a>versione di jQuery 3.1.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.Map
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.Map

#### <a name="jquery-version-310"></a>jQuery versione 3.1.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.Map
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.Map

#### <a name="jquery-version-300"></a>versione di jQuery 3.0.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.Map
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.Map

#### <a name="jquery-version-224"></a>versione di jQuery 2.2.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.Map

#### <a name="jquery-version-223"></a>versione di jQuery 2.2.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.Map

#### <a name="jquery-version-222"></a>versione di jQuery 2.2.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.Map

#### <a name="jquery-version-221"></a>jQuery versione 2.2.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.Map

#### <a name="jquery-version-220"></a>versione di jQuery 2.2.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.Map

#### <a name="jquery-version-214"></a>versione di jQuery 2.1.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.Map

#### <a name="jquery-version-213"></a>versione di jQuery 2.1.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.Map

#### <a name="jquery-version-212"></a>versione di jQuery 2.1.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js

#### <a name="jquery-version-211"></a>versione di jQuery 2.1.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.Map

#### <a name="jquery-version-210"></a>versione di jQuery 2.1.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.Map

#### <a name="jquery-version-203"></a>versione di jQuery 2.0.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.Map

#### <a name="jquery-version-202"></a>versione di jQuery 2.0.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.Map

#### <a name="jquery-version-201"></a>versione di jQuery 2.0.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.Map

#### <a name="jquery-version-200"></a>versione di jQuery 2.0.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.Map

#### <a name="jquery-version-1124"></a>versione di jQuery 1.12.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.Map

#### <a name="jquery-version-1123"></a>versione di jQuery 1.12.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.Map

#### <a name="jquery-version-1122"></a>versione di jQuery 1.12.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.Map

#### <a name="jquery-version-1121"></a>versione di jQuery 1.12.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.Map

#### <a name="jquery-version-1120"></a>versione di jQuery 1.12.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.Map

#### <a name="jquery-version-1113"></a>versione di jQuery 1.11.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.Map

#### <a name="jquery-version-1112"></a>versione di jQuery 1.11.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.Map

#### <a name="jquery-version-1111"></a>versione di jQuery 1.11.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.Map

#### <a name="jquery-version-1110"></a>versione di jQuery 1.11.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.Map

#### <a name="jquery-version-1102"></a>versione di jQuery 1.10.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.Map

#### <a name="jquery-version-1101"></a>versione di jQuery 1.10.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.Map

#### <a name="jquery-version-1100"></a>versione di jQuery 1.10.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.Map

#### <a name="jquery-version-191"></a>versione di jQuery 1.9.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.Map

#### <a name="jquery-version-190"></a>versione di jQuery alla 1.9.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.Map

#### <a name="jquery-version-183"></a>versione di jQuery 1.8.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a>versione di jQuery 1.8.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a>versione di jQuery 1.8.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a>versione di jQuery 1.8.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a>versione di jQuery 1.7.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js

#### <a name="jquery-version-171"></a>versione di jQuery 1.7.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a>jQuery versione 1.7

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a>versione di jQuery 1.6.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a>versione di jQuery 1.6.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a>versione di jQuery 1.6.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a>versione di jQuery 1.6.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a>jQuery versione 1.6

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a>versione di jQuery 1.5.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a>versione di jQuery 1.5.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a>jQuery versione 1.5

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a>versione di jQuery 1.4.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a>versione di jQuery 1.4.3

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a>versione di jQuery 1.4.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a>versione di jQuery 1.4.1

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a>jQuery versione 1.4

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js

#### <a name="jquery-version-132"></a>versione di jQuery 1.3.2

- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-vsdoc.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a>Versioni di eseguire la migrazione di jQuery nella rete CDN

Le versioni seguenti di jQuery migrazione sono ospitate nella rete CDN:

#### <a name="jquery-migrate-version-300"></a>eseguire la migrazione della versione 3.0.0 jQuery

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-3.0.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a>Migrazione versione 1.2.1 jQuery

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.2.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.2.1.min.js

jQuery migrazione versione 1.2.0

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.2.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a>Migrazione versione 1.1.1 jQuery

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.1.1.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a>Migrazione versione 1.1.0 jQuery

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.1.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a>jQuery versione 1.0.0 di migrazione

- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.0.0.js
- http://AJAX.aspnetcdn.com/AJAX/jQuery.migrate/jQuery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a>jQuery versioni dell'interfaccia utente nella rete CDN

Le versioni della libreria jQuery UI seguenti sono ospitate in questa rete CDN. Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.

- [jQuery UI 1.12.1](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.12.0](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.11.4](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.11.3](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.11.2](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.11.1](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.11.0](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.10.4](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.10.3](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.10.2](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.10.1](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.10.0](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.9.2](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.9.1](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 sulla rete CDN di Microsoft Ajax")
- [jQuery UI alla 1.9.0](jquery-ui/cdnjqueryui190.md "jQuery UI alla 1.9.0 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.24](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.23](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.22](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.21](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.20](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.19](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.18](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.17](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.16](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.15](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.14](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.13](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.12](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.11](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.10](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.9](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.8](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.7](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.6](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 sulla rete CDN di Microsoft Ajax")
- [jQuery UI 1.8.5](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a>Versioni di convalida nella rete CDN jQuery

Le versioni della libreria di convalida jQuery seguenti sono ospitate in questa rete CDN. Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.

- [jQuery convalida 1.17.0](jquery-validate/cdnjqueryvalidate1170.md "1.17.0 convalida jQuery")
- [jQuery convalida 1.16.0](jquery-validate/cdnjqueryvalidate1160.md "1.16.0 convalida jQuery")
- [jQuery convalida 1.15.1](jquery-validate/cdnjqueryvalidate1151.md "1.15.1 convalida jQuery")
- [jQuery convalida 1.15.0](jquery-validate/cdnjqueryvalidate1150.md "1.15.0 convalida jQuery")
- [jQuery convalida 1.14.0](jquery-validate/cdnjqueryvalidate1140.md "1.14.0 convalida jQuery")
- [jQuery convalida 1.13.1](jquery-validate/cdnjqueryvalidate1131.md "1.13.1 convalida jQuery")
- [jQuery convalida 1.13.0](jquery-validate/cdnjqueryvalidate1130.md "1.13.0 convalida jQuery")
- [jQuery convalida 1.12.0](jquery-validate/cdnjqueryvalidate1120.md "1.12.0 convalida jQuery")
- [jQuery convalida 1.11.1](jquery-validate/cdnjqueryvalidate1111.md "1.11.1 convalida jQuery")
- [jQuery convalida 1.11.0](jquery-validate/cdnjqueryvalidate111.md "1.11.0 convalida jQuery")
- [jQuery convalida 1.10.0](jquery-validate/cdnjqueryvalidate110.md "1.10.0 convalida jQuery")
- [jQuery convalida 1.9](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versione 1.9")
- [jQuery convalida 1.8.1](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versione 1.8.1")
- [jQuery convalida 1.8](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versione 1.8")
- [jQuery convalida 1.7](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versione 1.7")
- [jQuery convalida 1.6](jquery-validate/cdnjqueryvalidate16.md "jQuery convalida 1.6")
- [jQuery convalida 1.5.5](jquery-validate/cdnjqueryvalidate155.md "1.5.5 convalida jQuery")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a>jQuery Mobile versioni nella rete CDN

Le seguenti versioni della libreria jQuery Mobile sono ospitate in questa rete CDN. Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.

- [jQuery Mobile 1.4.5](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.4.2](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.4.1](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.4.0](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.3.2](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.3.1](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.3.0](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.2.0](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.1.2](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.1.1](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.1.0](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.1.0 RC 2](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.0.1](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.0](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.0 RC 2](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 nella rete CDN di Microsoft Ajax")
- [jQuery Mobile 1.0 RC 1](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 nella rete CDN di Microsoft Ajax")
- [la versione beta di jQuery Mobile 1.0 3](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 nella rete CDN di Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a>jQuery rilasci di modelli di rete CDN

Le seguenti versioni di plug-in modelli jQuery sono ospitate in questa rete CDN. Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.

- [jQuery modelli Beta 1](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery modelli Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a>jQuery versioni ciclo nella rete CDN

Le versioni seguenti di plug-in del ciclo di jQuery sono ospitate in questa rete CDN. Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.

- [jQuery 2,99 ciclo](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 ciclo")
- [jQuery ciclo 2.94](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 ciclo")
- [jQuery 2,88 ciclo](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 ciclo")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a>jQuery versioni DataTable nella rete CDN

Le seguenti versioni di jQuery DataTable plug-in sono ospitate in questa rete CDN. Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.

- [jQuery DataTable 1.10.5](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTable 1.10.5")
- [jQuery DataTable 1.10.4](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTable 1.10.4")
- [jQuery DataTable 1.9.4](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTable 1.9.4")
- [jQuery DataTable 1.9.3](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTable 1.9.3")
- [jQuery DataTable 1.9.2](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTable 1.9.2")
- [jQuery DataTable 1.9.1](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTable 1.9.1")
- [jQuery DataTable alla 1.9.0](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTable alla 1.9.0")
- [jQuery DataTable 1.8.2](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTable 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a>Versioni di Modernizr nella rete CDN

Le seguenti versioni di [Modernizr](http://www.modernizr.com "Modernizr") ospitate nella rete CDN:

- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js
- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js
- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js
- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js
- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js
- http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a>Versioni di JSHint nella rete CDN

Le seguenti versioni di [JSHint](http://www.jshint.com "JSHint") ospitate nella rete CDN:

- http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a>Versioni di Knockout nella rete CDN

Le seguenti versioni di [Knockout](http://www.knockoutjs.com "Knockout") ospitate nella rete CDN:

- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.debug.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.debug.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.debug.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.debug.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.debug.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.debug.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.debug.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.debug.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.debug.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.js
- http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a>Globalizzare le versioni su una rete CDN

Le seguenti versioni di [Globalize](https://github.com/jquery/globalize "Globalize") ospitate nella rete CDN:

#### <a name="globalize-version-100"></a>La versione 1.0.0 di globalizzazione

- http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js
- http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/Node-Main.js
- http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js
- http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/date.js
- http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js
- http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js
- http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js
- http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/relative-Time.js

#### <a name="globalize-version-011"></a>Globalizzazione versione 0.1.1

- http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.min.js
- http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js
- http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Cultures.js

    - tutte le impostazioni cultura
- http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Culture. {codice delle impostazioni cultura} js

    - Sostituire "{codice delle impostazioni cultura}" con il codice della lingua desiderata, ad esempio, dei file in rete CDN di Microsoft globalize.culture.en GB.js== = = queste librerie sono state caricate da Microsoft.

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a>Rispondere versioni su una rete CDN

Le seguenti versioni di [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") rispondono ospitate nella rete CDN:

#### <a name="respond-version-142"></a>Rispondere versione 1.4.2

- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js
- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.min.js
- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.js
- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a>Rispondere versione 1.4.1

- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js
- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.min.js
- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.js
- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a>Rispondere versione 1.4.0

- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js
- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.min.js
- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.js
- http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a>Rispondere versione 1.3.0

- http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js

#### <a name="respond-version-120"></a>Rispondere versione 1.2.0

- http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a>Versioni di bootstrap nella rete CDN

Le seguenti versioni di [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap ospitate nella rete CDN:

#### <a name="bootstrap-version-337"></a>Versione bootstrap 3.3.7

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2

#### <a name="bootstrap-version-336"></a>Versione bootstrap 3.3.6

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2

#### <a name="bootstrap-version-335"></a>Bootstrap versione 3.3.5

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2

#### <a name="bootstrap-version-334"></a>Versione bootstrap 3.3.4

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2

#### <a name="bootstrap-version-332"></a>Versione bootstrap 3.3.2

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2

#### <a name="bootstrap-version-331"></a>Versione bootstrap 3.3.1

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-330"></a>Versione bootstrap 3.3.0

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-320"></a>Versione bootstrap 3.2.0

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-311"></a>Versione bootstrap 3.1.1

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-310"></a>Bootstrap versione 3.1.0

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.CSS.Map
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-303"></a>Versione bootstrap 3.0.3

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-302"></a>Versione bootstrap 3.0.2

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-301"></a>Versione bootstrap 3.0.1

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-300"></a>Versione bootstrap 3.0.0

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap-theme.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/CSS/bootstrap-theme.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.EOT
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff

#### <a name="bootstrap-version-232"></a>Bootstrap versione 2.3.2

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap-responsive.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/CSS/bootstrap-responsive.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/IMG/glyphicons-halflings.PNG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.2/IMG/glyphicons-halflings-white.PNG

#### <a name="bootstrap-version-231"></a>Bootstrap versione 2.3.1

- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/bootstrap.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/bootstrap.min.js
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap-responsive.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/CSS/bootstrap-responsive.min.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/IMG/glyphicons-halflings.PNG
- http://AJAX.aspnetcdn.com/AJAX/bootstrap/2.3.1/IMG/glyphicons-halflings-white.PNG

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a>Versioni di bootstrap TouchCarousel nella rete CDN

Le seguenti versioni di [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") TouchCarousel Bootstrap versioni sono ospitate nella rete CDN :

#### <a name="bootstrap-touchcarousel-version-080"></a>Bootstrap TouchCarousel versione 0.8.0

- http://AJAX.aspnetcdn.com/AJAX/bootstrap-touch-carousel/0.8.0/CSS/bootstrap-touch-carousel.CSS
- http://AJAX.aspnetcdn.com/AJAX/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a>Versioni di Hammer.js nella rete CDN

Le seguenti versioni di [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js versioni ospitate nella rete CDN:

#### <a name="hammerjs-version-204"></a>Versione Hammer.js 2.0.4

- http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.js
- http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.min.js
- http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.min.Map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a>Web Form ASP.NET e Ajax versioni nella rete CDN

Le seguenti versioni di ASP.NET Ajax Library sono ospitate nella rete CDN. Fare clic su ogni collegamento per visualizzare l'elenco effettivo dei file.

- [Web Form ASP.NET e Ajax versione 4.5.2](cdnajax452.md "Web Form ASP.NET e Ajax 4.5.2")
- [Web Form ASP.NET e Ajax versione 4](cdnajax4.md "Web Form ASP.NET e Ajax 4")
- [ASP.NET Ajax versione 3.5](cdnajax35.md "Ajax ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a>ASP.NET MVC rilascia nella rete CDN

I seguenti file JavaScript di ASP.NET MVC sono ospitati in questa rete CDN:

#### <a name="aspnet-mvc-523"></a>ASP.NET MVC 5.2.3

- http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a>ASP.NET MVC 5.1

- http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a>ASP.NET MVC 5.0

- http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a>MVC ASP.NET 4.0

- http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a>ASP.NET MVC 3.0

- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a>MVC ASP.NET 2.0

- http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a>ASP.NET MVC 1,0

- http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js
- http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a>ASP.NET SignalR rilascia nella rete CDN

I seguenti file di ASP.NET SignalR JavaScript sono ospitati in questa rete CDN:

#### <a name="aspnet-signalr-222"></a>ASP.NET SignalR 2.2.2

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.js

#### <a name="aspnet-signalr-221"></a>ASP.NET SignalR 2.2.1

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.js

#### <a name="aspnet-signalr-220"></a>ASP.NET SignalR 2.2.0

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.js

#### <a name="aspnet-signalr-210"></a>ASP.NET SignalR 2.1.0

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.js

#### <a name="aspnet-signalr-203"></a>ASP.NET SignalR 2.0.3

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.js

#### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.js

#### <a name="aspnet-signalr-201"></a>ASP.NET SignalR 2.0.1

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.js

#### <a name="aspnet-signalr-200"></a>SignalR ASP.NET 2.0.0

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.js

#### <a name="aspnet-signalr-113"></a>ASP.NET SignalR 1.1.3

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.js

#### <a name="aspnet-signalr-112"></a>ASP.NET SignalR 1.1.2

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.js

#### <a name="aspnet-signalr-111"></a>ASP.NET SignalR 1.1.1

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.js

#### <a name="aspnet-signalr-110"></a>ASP.NET SignalR 1.1.0

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.js

#### <a name="aspnet-signalr-101"></a>ASP.NET SignalR 1.0.1

- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.min.js
- http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.js

Per informazioni sulle condizioni di utilizzo per la rete CDN, vedere [Microsoft Ajax CDN condizioni di utilizzo](https://www.asp.net/terms-of-use "Microsoft Ajax CDN condizioni di utilizzo").
