---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: What ' s New in ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive le nuove funzionalità e i miglioramenti introdotti con ASP.NET MVC 2. Questo documento è anche disponibile per il download.
ms.author: riande
ms.date: 04/20/2010
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 82a3fd4fe74202ed9a23298390322458cfc029f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831563"
---
<a name="whats-new-in-aspnet-mvc-2"></a>What ' s New in ASP.NET MVC 2
====================
> Questo documento descrive le nuove funzionalità e i miglioramenti introdotti con ASP.NET MVC 2. Questo documento è disponibile anche per [scaricare](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[Introduzione](#_TOC1)   
[L'aggiornamento di un progetto ASP.NET MVC 1.0 ad ASP.NET MVC 2](#_TOC2)   
[Nuove funzionalità](#_TOC3)   
[Helper basati su modelli](#_TOC3_1)   
[Aree](#_TOC3_2)   
[Supporto per i controller asincroni](#_TOC3_3)   
[Supporto per DefaultValueAttribute nei parametri del metodo di azione](#_TOC3_4)   
[Supporto per l'associazione di dati binari con gli strumenti di associazione](#_TOC3_5)   
[Classi ModelMetadataProvider e ModelMetadata](#_TOC3_6)   
[Supporto per gli attributi DataAnnotations](#_TOC3_7)   
[Provider di Validator del modello](#_TOC3_8)   
[Convalida lato client](#_TOC3_9)   
[Nuovi frammenti di codice per Visual Studio 2010](#_TOC3_10)   
[Nuovo filtro azione RequireHttpsAttribute](#_TOC3_11)   
[Si esegue l'override del verbo HTTP (metodo)](#_TOC3_12)   
[Nuova classe HiddenInputAttribute per helper basati su modelli](#_TOC3_13)   
[Metodo Html.ValidationSummary Helper può visualizzare gli errori a livello di modello](#_TOC3_14)   
[Modelli T4 in Visual Studio genera codice che è specifico per la versione di destinazione di .NET Framework](#_TOC3_15)[i miglioramenti delle API](#_TOC4)  
[Modifiche di rilievo](#_TOC5)  
[Dichiarazione di non responsabilità](#_TOC6)  

## <a id="_TOC1"></a>  Introduzione

ASP.NET MVC 2 si basa su ASP.NET MVC 1.0 e introduce un ampio set di miglioramenti e funzionalità incentrate su come aumentare la produttività. Questa versione è compatibile con ASP.NET MVC 1.0, in modo che tutte le Knowledge Base, competenze, codice e le estensioni per ASP.NET MVC 1.0 continuano ad applicare.

Per altre informazioni su ASP.NET MVC, visitare le risorse seguenti:

- [Documentazione di ASP.NET MVC su MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [Il sito Web ASP.NET MVC](https://asp.net/mvc/)
- [Forum di ASP.NET MVC](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>  L'aggiornamento di un progetto ASP.NET MVC 1.0 ad ASP.NET MVC 2

ASP.NET MVC 2 può essere installato affiancato con ASP.NET MVC 1.0 nello stesso server, che offre una maggiore flessibilità agli sviluppatori nella scelta del momento aggiornare un'applicazione ASP.NET MVC 1.0 ad ASP.NET MVC 2. Per informazioni su come eseguire l'aggiornamento, vedere il documento [l'aggiornamento di un'applicazione ASP.NET MVC 1.0 per ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a id="_TOC3"></a>  Nuove funzionalità

Questa sezione vengono descritte le funzionalità che sono state introdotte nella versione 2 di MVC.

### <a id="_TOC3_1"></a>  Helper basati su modelli

Helper basati su modelli consentono di associare automaticamente gli elementi HTML per la modifica e vengono visualizzati con i tipi di dati. Ad esempio, quando i dati di tipo System. DateTime viene visualizzati in una vista, un elemento dell'interfaccia utente di selezione data può essere eseguito automaticamente il rendering. È simile alla modalità di funzionamento di modelli di campo in ASP.NET Dynamic Data. Per altre informazioni, vedere [uso di helper basati su modelli per visualizzare i dati](https://go.microsoft.com/fwlink/?LinkId=159062) sul sito Web MSDN.

### <a id="_TOC3_2"></a>  Areas

Le aree consentono di organizzare un progetto di grandi dimensioni in più sezioni più piccole per poter gestire la complessità di un'applicazione Web di grandi dimensioni. Ogni sezione ("area") in genere rappresenta una sezione separata di un sito Web di grandi dimensioni e viene usato per raggruppare insiemi correlati di controller e visualizzazioni. Per altre informazioni, vedere [procedura dettagliata: organizzazione di un'applicazione MVC ASP.NET da aree](https://go.microsoft.com/fwlink/?LinkId=158978) sul sito Web MSDN.

Per creare una nuova area, in Esplora soluzioni, fare clic sul progetto, fare clic su Aggiungi e quindi fare clic su Area. Verrà visualizzata una finestra di dialogo che richiede il nome dell'area. Dopo aver immesso il nome dell'area, una nuova area di Visual Studio aggiunge al progetto.

La figura seguente mostra un layout di esempio per un progetto con due aree, amministrazione e blog.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Quando si crea un'area, Visual Studio aggiunge una classe che deriva da AreaRegistration a ogni area. Questa classe è necessaria per registrare l'area e le route, come illustrato nell'esempio seguente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

Il modello di progetto predefinito per ASP.NET MVC 2 include una chiamata al metodo RegisterAllAreas nel codice per il file Global. asax. Questo metodo registra ogni area nel progetto cercando tutti i tipi che derivano dalla classe AreaRegistration, creando un'istanza del tipo e quindi la chiamata al metodo RegisterArea nell'istanza. Nell'esempio seguente viene illustrato come questa operazione viene eseguita.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Se non si specifica lo spazio dei nomi nel metodo RegisterArea chiamando il contesto. Metodo Namespaces.Add, lo spazio dei nomi della classe di registrazione viene utilizzato per impostazione predefinita.

### <a id="_TOC3_3"></a>  Supporto per i controller asincroni

ASP.NET MVC 2 consente ora ai controller elaborare le richieste in modo asincrono. Questo può causare miglioramenti delle prestazioni consentendo ai server che spesso chiamano le operazioni di blocco (ad esempio, le richieste di rete) per chiamare invece le controparti non bloccante. Per altre informazioni, vedere la [utilizzando un Controller asincrono in ASP.NET MVC](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) argomento in MSDN.

### <a id="_TOC3_4"></a>  Supporto per DefaultValueAttribute nei parametri del metodo di azione

La classe DefaultValueAttribute consente un valore predefinito da fornire per il parametro di argomento per un metodo di azione. Si supponga, ad esempio, che viene definita la route predefinita seguente:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Si supponga anche che è definito il metodo di azione e del controller seguente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Uno dei seguente richiesta URL richiamerà il metodo di azione di visualizzazione che è definito nell'esempio precedente.

- / Articolo/View/123
- / Articolo/View/123? pagina = 1 (in modo efficace lo stesso nome nella richiesta precedente)
- / Articolo/View/123? pagina = 2

Senza l'attributo DefaultValueAttribute, il primo URL dall'elenco precedente non funzionerà, perché l'argomento della pagina è un tipo di valore non nullable il cui valore non è stato specificato.

Se il codice è scritto in Visual Basic 2010 o Visual c# 2010, è possibile utilizzare i parametri facoltativi anziché l'attributo DefaultValueAttribute, come illustrato nell'esempio seguente:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>  Supporto per l'associazione di dati binari con gli strumenti di associazione

Sono disponibili due nuovi overload dell'helper Html.Hidden che codifica valori binari sotto forma di stringhe con codifica base 64:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Un utilizzo tipico consiste nell'incorporare un timestamp per un oggetto nella visualizzazione. Ad esempio, l'applicazione potrebbe includere l'oggetto prodotto seguente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Un modulo di modifica è possibile eseguire il rendering la proprietà TimeStamp nel formato come illustrato nell'esempio seguente:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Questo markup esegue il rendering di un elemento input nascosto con il valore di timestamp come stringa codificata in base 64 che è simile al seguente:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Questa forma potrà essere registrata per un metodo di azione che ha un argomento di tipo prodotto, come illustrato nell'esempio seguente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

Nel metodo di azione, la proprietà TimeStamp viene compilata correttamente perché la stringa con codifica base-64 registrata viene convertita in una matrice di byte.

### <a id="_TOC3_6"></a>  Classi ModelMetadataProvider e ModelMetadata

La classe ModelMetadataProvider fornisce un'astrazione per ottenere metadati per il modello all'interno di una vista. MVC 2 include un provider predefinito che rende disponibili i metadati che viene esposto dagli attributi nello spazio dei nomi System.ComponentModel.DataAnnotations. È possibile creare provider di metadati che forniscono metadati da altri archivi dati, ad esempio database o file XML.

La classe del ViewDataDictionary espone un oggetto ModelMetadata che contiene i metadati estratti dalla classe ModelMetadataProvider dal modello. In questo modo gli helper basati su modelli usare i metadati e modificare di conseguenza l'output delle applicazioni.

Per altre informazioni, vedere la documentazione per il [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) e [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) classi.

### <a id="_TOC3_7"></a>  Supporto per gli attributi DataAnnotations

ASP.NET MVC 2 supporta l'utilizzo di attributi di convalida RangeAttribute, RequiredAttribute, oggetto StringLengthAttribute e RegexAttribute (definiti nello spazio dei nomi System.ComponentModel.DataAnnotations) quando si associa a un modello per fornire input convalida.

Per altre informazioni, vedere [procedura: convalida del modello dati usando gli attributi DataAnnotations](https://go.microsoft.com/fwlink/?LinkId=159063) sul sito Web MSDN. Un progetto di esempio che illustra l'uso di questi attributi è disponibile per il download nel [ https://go.microsoft.com/fwlink/?LinkId=157753 ](https://go.microsoft.com/fwlink/?LinkId=157753).

### <a id="_TOC3_8"></a>  Provider di Validator del modello

La classe del provider di convalida del modello rappresenta un'astrazione che fornisce la logica di convalida per il modello. ASP.NET MVC include un provider predefinito in base agli attributi di convalida che vengono inclusi nello spazio dei nomi System.ComponentModel.DataAnnotations. È anche possibile creare il proprio provider di convalida che definiscono le regole di convalida personalizzate e di mapping personalizzate di regole di convalida al modello. Per altre informazioni, vedere la documentazione per il [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) classe.

### <a id="_TOC3_9"></a>  Convalida lato client

La classe del provider di validator del modello espone i metadati di convalida nel browser sotto forma di dati serializzati con JSON che possono essere usati da una libreria di convalida lato client. ASP.NET MVC 2 include un adattatore che supporta gli attributi di convalida dello spazio dei nomi DataAnnotations indicati in precedenza e la libreria client di convalida. La classe del provider consente anche di usare altre librerie client convalida creando un adattatore che elabora i dati JSON e chiamate nella libreria alternativa.

### <a id="_TOC3_10"></a>  Nuovi frammenti di codice per Visual Studio 2010

Un set di frammenti di codice HTML per ASP.NET MVC 2 viene installato con Visual Studio 2010. Per visualizzare un elenco di questi frammenti di codice, nel menu Strumenti, selezionare Gestione frammenti di codice. Per la lingua, selezionare HTML e per il percorso, selezionare ASP.NET MVC 2. Per altre informazioni su come usare frammenti di codice, vedere la documentazione di Visual Studio.

### <a id="_TOC3_11"></a>  Nuovo filtro azione RequireHttpsAttribute

ASP.NET MVC 2 include una nuova classe RequireHttpsAttribute che può essere applicata ai controller e metodi di azione. Per impostazione predefinita, il filtro reindirizza una richiesta non SSL HTTP nell'equivalente abilitate per SSL (HTTPS).

### <a id="_TOC3_12"></a>  Si esegue l'override del verbo HTTP (metodo)

Quando si compila un sito Web usando lo stile architetturale REST, verbi HTTP vengono utilizzati per determinare l'azione da eseguire per una risorsa. REST è necessario che supportano applicazioni l'intera gamma dei verbi HTTP comuni, inclusi GET, PUT, POST e DELETE.

ASP.NET MVC 2 include nuovi attributi che è possibile applicare tale sintassi compatta di funzionalità e i metodi di azione. Questi attributi consentono di ASP.NET MVC selezionare un metodo di azione in base al verbo HTTP. Nell'esempio seguente, una richiesta POST chiamerà il primo metodo di azione e una richiesta PUT chiamerà il secondo metodo di azione.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

Nelle versioni precedenti di ASP.NET MVC, questi metodi di azione necessaria sintassi più dettagliata, come illustrato nell'esempio seguente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Poiché i browser supportano solo i verbi GET e HTTP POST, non è possibile eseguire il post in un'azione che richiede un verbo diverso. Non è pertanto possibile supportare in modo nativo tutte le richieste REST.

Tuttavia, per supportare RESTful richieste durante le operazioni POST, ASP.NET MVC 2 introduce un nuovo metodo di helper HTML HttpMethodOverride. Questo metodo esegue il rendering di un elemento input nascosto che determini il form emulare efficacemente qualsiasi metodo HTTP. Ad esempio, tramite il metodo di helper HTML HttpMethodOverride, è possibile avere un invio del form vengono visualizzati una richiesta PUT o DELETE. Il comportamento di HttpMethodOverride interessa gli attributi seguenti:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

Elemento input nascosto con il relativo nome X-HTTP-Method-Override e il relativo valore impostato per il verbo HTTP per emulare. Il valore di sostituzione può anche essere specificato in un'intestazione HTTP o in un valore di stringa di query come una coppia nome/valore.

La sostituzione è utilizzabile solo quando la richiesta reale è una richiesta POST. Il valore di sostituzione verrà ignorato per le richieste che usano qualsiasi altro verbo HTTP.

### <a id="_TOC3_13"></a>  Nuova classe HiddenInputAttribute per helper basati su modelli

È possibile applicare il nuovo attributo HiddenInputAttribute a una proprietà del modello per indicare se è necessario visualizzare un elemento input nascosto quando la visualizzazione del modello in un modello di editor. (L'attributo imposta un valore di UIHint implicito di HiddenInput). La proprietà dell'attributo disponibili DisplayValue consente di specificare se il valore viene visualizzato nell'editor e le modalità di visualizzazione. Quando disponibili DisplayValue è impostata su false, viene visualizzato nulla, nemmeno il markup HTML che in genere racchiude un campo. Il valore predefinito per disponibili DisplayValue è true.

È possibile utilizzare l'attributo HiddenInputAttribute negli scenari seguenti:

- Quando una visualizzazione consente agli utenti di modificare l'ID di un oggetto ed è necessario visualizzare il valore anche per fornire un elemento input nascosto che contiene l'ID precedente in modo che può essere passato nuovamente al controller.
- Quando una visualizzazione consente agli utenti modificare una proprietà binaria che non deve essere mai visualizzata, ad esempio una proprietà di timestamp. In tal caso, il valore e il codice HTML circostante (ad esempio l'etichetta e valore) non vengono visualizzati.

Nell'esempio seguente viene illustrato come utilizzare la classe HiddenInputAttribute.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Quando l'attributo è impostato su true o si specifica alcun parametro, si verifica quanto segue:

- Nei modelli di visualizzazione, viene eseguito il rendering di un'etichetta e il valore viene visualizzato all'utente.
- Nei modelli di editor, viene eseguito il rendering di un'etichetta e il valore viene eseguito il rendering in un elemento input nascosto.

Quando l'attributo è impostato su false, si verifica quanto segue:

- Nei modelli di visualizzazione, non viene eseguito il rendering per il campo.
- Nei modelli di editor, l'etichetta non viene eseguito il rendering e il valore viene eseguito il rendering in un elemento input nascosto.

### <a id="_TOC3_14"></a>  Metodo Html.ValidationSummary Helper può visualizzare gli errori a livello di modello

Anziché visualizzare sempre tutti gli errori di convalida, il metodo helper Html.ValidationSummary include una nuova opzione per visualizzare solo gli errori a livello di modello. In questo modo gli errori a livello di modello da visualizzare nel riepilogo di convalida ed errori di specifiche del campo da visualizzare accanto a ogni campo.

### <a id="_TOC3_15"></a>  Modelli T4 in Visual Studio genera codice che è specifico per la versione di destinazione di .NET Framework

Una nuova proprietà è disponibile per i file di T4 dall'host ASP.NET MVC T4 che specifica la versione di .NET Framework che viene usato dall'applicazione. In questo modo, i modelli T4 generare il codice e markup specifico per una versione di .NET Framework. In Visual Studio 2008, il valore è sempre .NET 3.5. In Visual Studio 2010, il valore è .NET 3.5 o .NET 4.

## <a id="_TOC4"></a>  Miglioramenti delle API

In questa sezione vengono descritte le modifiche ai tipi di ASP.NET MVC esistenti e i membri.

- Aggiunto metodo CreateActionInvoker virtuale protetto nella classe Controller. Questo metodo viene richiamato dalla proprietà ActionInvoker del Controller e consente la creazione di istanze differita dell'invoker di se nessun invoker è già impostato.
- Aggiunto metodo HandleUnauthorizedRequest virtuale protetto nella classe AuthorizeAttribute. In questo modo i filtri che derivano da AuthorizeAttribute per controllare il comportamento quando si verifica un errore di autorizzazione.
- Aggiungere un metodo Add (stringa, oggetto o valore chiave) nella classe ValueProviderDictionary. In questo modo è possibile utilizzare la sintassi dell'inizializzatore di dizionario per ValueProviderDictionary, come nell'esempio seguente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Aggiunta di un'operazione get\_metodo nella classe Sys.Mvc.AjaxContext dell'oggetto. Si tratta di un metodo JavaScript simile a get\_metodo di dati, ma se il tipo di contenuto della risposta è application/json, ottenere\_object restituisce l'oggetto JSON.
- Aggiungere una proprietà di ActionDescriptor nella classe AuthorizationContext.
- Aggiunta di un token UrlParameter.Optional che può essere utilizzato per risolvere i problemi durante l'associazione a un modello che contiene una proprietà ID quando la proprietà non è presente in un post dei form. Per altre informazioni, vedere l'intervento [ASP.NET MVC 2 URL i parametri facoltativi](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) sul blog di Phil Haack.

## <a id="_TOC5"></a>  Modifiche di rilievo

Le modifiche seguenti potrebbero causare errori nelle applicazioni ASP.NET MVC 1.0 esistente.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Modifica del comportamento di convalida delle proprietà per le classi che implementano l'interfaccia IDataErrorInfo

Per gli oggetti modello che usano l'interfaccia IDataErrorInfo per eseguire la convalida, ogni proprietà viene convalidata, indipendentemente dal fatto che è stato impostato un nuovo valore. In ASP.NET MVC 1.0, sono state convalidate solo delle proprietà con i nuovi valori impostato. In ASP.NET MVC 2, la proprietà Error della IDataErrorInfo viene chiamata solo se tutte le convalide di proprietà hanno avuto esito positivo.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>Script di mapping di script IIS non è più disponibile nel programma di installazione

Lo script di mapping di script IIS è uno script della riga di comando che consente di configurare mapping dello script per IIS 6 e IIS 7 in modalità classica. Lo script di mapping di script non è necessaria se si usa Visual Studio Development Server oppure se si utilizza IIS 7 in modalità integrata. Gli script sono disponibili come download separato non supportato nel [WebStack ASP.NET](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>Il metodo helper Html.Substitute in MVC Futures non è più disponibile

A causa di modifiche nel comportamento per il rendering dei motori di visualizzazione MVC, il metodo helper Html.Substitute non funziona ed è stata rimossa.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>L'interfaccia IValueProvider sostituisce tutti gli utilizzi di IDictionary

Ogni argomento di metodo o proprietà accettati IDictionary in MVC 1.0 ora accetta IValueProvider. Questa modifica influisce solo sulle applicazioni che includono provider di valori personalizzati o gli strumenti di associazione di modelli personalizzato. Esempi di proprietà e metodi che sono interessati da questa modifica sono i seguenti:

- La proprietà ValueProvider delle classi ControllerBase e ModelBindingContext.
- I metodi TryUpdateModel della classe Controller.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Sono state aggiunte nuove classi CSS nel file Site. CSS

Il file CSS nei modelli di progetto ASP.NET MVC è stato aggiornato per includere nuovi stili utilizzati dalla funzionalità di convalida e da helper basati su modelli.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Gli helper ora restituiscono un oggetto MvcHtmlString

Per sfruttare la nuova sintassi di espressione di codifica HTML in ASP.NET 4, il tipo restituito per gli helper HTML è ora MvcHtmlString anziché una stringa. Se si usa ASP.NET MVC 2 e il nuovo helper su ASP.NET 3.5, non sarà in grado di sfruttare la sintassi di codifica HTML. la nuova sintassi è disponibile solo quando si esegue ASP.NET MVC 2 in ASP.NET 4.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult ora risponde solo alle richieste HTTP POST

Per ridurre i rischi JSON con il rischio di intercettazione di informazioni personali, per impostazione predefinita, attacchi di falsificazione della classe JsonResult ora risponde solo alle richieste HTTP POST. AJAX ottenere le chiamate ai metodi di azione che restituiscono un oggetto JsonResult devono essere modificate per usare POST invece. Se necessario, è possibile eseguire l'override di questo comportamento impostando la nuova proprietà JsonRequestBehavior di JsonResult. Per altre informazioni sulla potenziale exploit, vedere il post di blog [JSON Hijack](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) sul blog di Phil Haack.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Modello e i setter delle proprietà ModelType su ModelBindingContext sono obsoleti

Una nuova proprietà ModelMetadata impostabili aggiunti alla classe ModelBindingContext. La nuova proprietà incapsula le proprietà ModelType sia nel modello. Anche se le proprietà del modello e ModelType sono obsolete, per garantire la compatibilità con le versioni precedenti il getter proprietà continuano a funzionare; delegano alla proprietà ModelMetadata per recuperare il valore.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Le modifiche apportate alla classe DefaultControllerFactory interrompano le factory del controller personalizzato che derivano da essa

La classe DefaultControllerFactory è stato risolto rimuovendo la proprietà RequestContext. Al posto di questa proprietà, l'istanza di contesto di richiesta viene passato ai metodi GetControllerInstance e GetControllerType virtuali protetti. Questa modifica interessa le factory del controller personalizzato che derivano da DefaultControllerFactory.

Factory del controller personalizzato vengono spesso usate per offrire ad applicazioni di inserimento delle dipendenze per ASP.NET MVC. Per aggiornare le factory del controller personalizzato per il supporto di ASP.NET MVC 2, modificare la firma del metodo o firme corrispondano nuove firme e usare il parametro di contesto richiesta anziché la proprietà.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Area" è ora una chiave valore di route riservato

A questo punto, la stringa "area" nei valori di Route ha un significato speciale in ASP.NET MVC, nello stesso modo che "controller" e "azione". Un'implicazione è che se vengono forniti gli helper HTML con un dizionario di valori di route che contiene "area", gli helper non è più aggiungerà "area" nella stringa di query.

Se si usa la funzionalità zone, assicurarsi di non usare {area} come parte dell'URL di route.


## <a id="_TOC6"></a>  Disclaimer

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute nel presente documento rappresentano l'attuale opinione di Microsoft Corporation circa le problematiche discusse alla data della pubblicazione. Poiché Microsoft deve rispondere ai cambiamenti delle condizioni di mercato, il presente documento non deve essere interpretato quale un impegno da parte di Microsoft e Microsoft non può garantire l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Il presente white paper è fornito solo a scopi informativi. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O LEGALE, IN MERITO ALLE INFORMAZIONI DEL PRESENTE DOCUMENTO.

Il rispetto di tutte le leggi applicabili in materia di copyright è a esclusivo carico dell'utente. Senza limitare i diritti sanciti dal copyright, nessuna parte di questo documento può essere riprodotta, memorizzata o inserita in un sistema di ricerca o trasmessa in qualsiasi forma o mezzo (elettronico o meccanico, mediante fotocopia, registrazione o altro), per alcuno scopo, senza l'autorizzazione scritta di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto del presente documento. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna del presente documento non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non diversamente specificato, la società, organizzazioni, prodotti, nomi di dominio, indirizzi di posta elettronica, logo, persone, luoghi ed eventi citati in questo documento sono fittizio e nessuna associazione con qualsiasi società, organizzazione, prodotto, nome di dominio, indirizzo di posta elettronica indirizzo, logo, persona, luogo o evento è intenzionale o può essere presupposta.

© 2010 Microsoft Corporation. Tutti i diritti sono riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

I nomi di società e prodotti reali citati nel presente documento possono essere marchi dei rispettivi proprietari.
