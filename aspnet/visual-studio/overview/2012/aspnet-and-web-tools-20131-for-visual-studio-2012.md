---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Note sulla versione di ASP.NET e strumenti Web 2013.1 per Visual Studio 2012 | Documenti Microsoft
author: microsoft
description: Questo documento descrive la versione di ASP.NET e 2013.1 strumenti Web per Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Note sulla versione di ASP.NET e strumenti Web 2013.1 per Visual Studio 2012
====================
by [Microsoft](https://github.com/microsoft)

> Questo documento descrive la versione di ASP.NET e 2013.1 strumenti Web per Visual Studio 2012.


## <a name="contents"></a>Sommario

- [Note sull'installazione](#install)
- [Requisiti software](#requirements)
- Nuove funzionalità di ASP.NET e strumenti Web 2013.1 per Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Modelli](#templates)

        - [Modello di MVC ASP.NET 5](#mvc5template)
        - [Modello ASP.NET Web API 2](#apitemplate)
        - [Modelli di elemento](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [Scaffolding di ASP.NET](#scaffold)
    - [Editor Razor](#razor)
    - [NuGet 2.7](#nuget)
- Problemi noti e modifiche di rilievo

    - [Scaffolding di ASP.NET](#issuescaffolding)

        - [MVC e Web API Scaffolding - HTTP 404, errore non trovato](#404issue)
        - [Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di un elemento di scaffolding](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Visualizzazione di file cshtml con Esplora con o F5 provoca un errore del server](#browseissue)
        - [Tilde(~) e riscrittura Url](#rewriteissue)
    - [Modelli](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Note sull'installazione

[Installare](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) Web ASP.NET e strumenti 2013.1 per Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Requisiti software

È necessario disporre di Visual Studio 2012 o Visual Studio Express 2012 per Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nuove funzionalità di ASP.NET e strumenti Web 2013.1 per Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Durante lo scaffolding controller MVC 5 e visualizzazioni, viene utilizzato il markup per le viste [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Modelli

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Modello di MVC ASP.NET 5

È stato aggiunto un nuovo modello MVC 5. Fa riferimento a pacchetti MVC 5 NuGet più recenti e lo scaffolding consente di aggiungere controller e visualizzazioni.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Modello ASP.NET Web API 2

È stato aggiunto un nuovo modello di API Web 2. Fa riferimento a pacchetti Web API 2 NuGet più recenti e lo scaffolding consente di aggiungere controller e visualizzazioni.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Modelli di elemento

Nuovi modelli di elemento di visualizzazioni MVC 5, pagine Web (Razor 3) e controller Web API 2 è stato aggiunto. L'installazione dei pacchetti NuGet correlati al progetto durante l'aggiunta di nuovi elementi.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Durante lo scaffolding un controller MVC o Web API Usa Entity Framework, utilizziamo Framework 6. Per ulteriori informazioni su Entity Framework, vedere il [cronologia delle versioni di Entity Framework](https://msdn.com/data/jj574253).

È anche possibile scaricare e installare gli strumenti di Entity Framework 6 per Visual Studio 2012. Vedere il [ottenere Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding di ASP.NET

Lo Scaffolding di ASP.NET è un framework di generazione di codice per applicazioni Web ASP.NET. Che consente di aggiungere il codice di boilerplate al progetto che interagisce con un modello di dati.

Nelle versioni precedenti di Visual Studio, lo scaffolding è limitato ai progetti ASP.NET MVC. Con questo aggiornamento, è ora possibile usare lo scaffolding per qualsiasi progetto ASP.NET, inclusi Web Form. Questo aggiornamento non supporta la generazione di pagine per un progetto di Web Form, ma è comunque possibile utilizzare lo scaffolding con Web Form aggiungendo le dipendenze MVC al progetto. Verrà aggiunto il supporto per la generazione di pagine Web Form in un aggiornamento futuro.

Quando si usa lo scaffolding, è assicurare che tutti obbligatori le dipendenze siano installate nel progetto. Ad esempio, se si avvia con un progetto di Web Form ASP.NET e quindi Usa lo scaffolding per aggiungere un Controller API Web, i pacchetti NuGet necessari e i riferimenti vengono aggiunti al progetto automaticamente.

Per aggiungere lo scaffolding di MVC a un progetto di Web Form, aggiungere un **nuovo elemento di scaffolding** e selezionare **MVC 5 dipendenze** nella finestra di dialogo. Sono disponibili due opzioni per lo scaffolding MVC; Minimo e completo. Se si seleziona minima, solo i pacchetti NuGet e i riferimenti per ASP.NET MVC vengono aggiunti al progetto. Se si seleziona l'opzione completo, vengono aggiunte le dipendenze minime, nonché i file di contenuto richiesti per un progetto MVC.

Supporto per lo scaffolding controller asincrono utilizza le nuove funzionalità asincrone di Entity Framework 6.

Per ulteriori informazioni ed esercitazioni, vedere [Panoramica lo Scaffolding di ASP.NET](../2013/aspnet-scaffolding-overview.md). Queste esercitazioni viene illustrato lo scaffolding con Visual Studio 2013, ma sono inoltre applicabili ad ASP.NET e 2013.1 strumenti Web per Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Editor Razor

Con questo aggiornamento, Visual Studio 2012 supporta ora gli strumenti o modifica 3 Razor.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

2.7 NuGet include un set completo delle nuove funzionalità descritte in dettaglio [note sulla versione 2.7 di NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Questa versione di NuGet Elimina la necessità per gli utenti consentire in modo esplicito NuGet per ripristinare i pacchetti mancanti. Il consenso per ripristinare automaticamente i pacchetti mancanti durante l'installazione di NuGet 2.7, gli utenti in modo implicito. Gli utenti possono escludere in modo esplicito di ripristino del pacchetto tramite le impostazioni di NuGet in Visual Studio. Questa modifica semplifica il funzionamento di ripristino del pacchetto.

## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding di ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC e Web API Scaffolding - HTTP 404, errore non trovato

Se si verifica un errore durante l'aggiunta di un elemento di scaffolding per un progetto, è possibile che il progetto verrà lasciato in uno stato incoerente. Alcune delle modifiche apportate lo scaffolding di essere eseguito il rollback, ma altre modifiche, ad esempio i pacchetti NuGet installati, non sarà possibile eseguire il rollback. Se le modifiche alla configurazione di routing rollback, gli utenti riceveranno un errore HTTP 404 quando passando a elementi di scaffolding.

Per correggere l'errore per MVC, aggiungere un nuovo elemento di scaffolding e selezionare le dipendenze di MVC 5 (minima o completa). Questo processo verrà aggiungere tutte le modifiche necessarie al progetto.

Per correggere l'errore per l'API Web:

1. Aggiungere alla classe WebApiConfig seguente al progetto.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Configurare WebApiConfig.Register nell'applicazione\_Start (metodo) in Global. asax, come indicato di seguito:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta di un elemento di scaffolding

Se Visual Studio Express 2012 per Web smette di funzionare dopo l'aggiunta dell'elemento di scaffolding con Entity Framework (ad esempio Web 2 Controller API con azioni, mediante Entity Framework), è possibile che Visual Studio Express non è stato possibile caricare l'immagine nativa di un assembly dipende Extensions.

Per risolvere il problema, configurare Visual Studio Express per funzionare con l'immagine MSIL di Extensions:

1. Aprire il prompt dei comandi in modalità amministratore.
2. Passare a %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE o % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (per Windows a 64 bit).
3. Aprire VWDExpress.exe.config in un editor di testo.
4. Aggiungere la riga seguente sotto il &lt;configurazione&gt;/&lt;runtime&gt; elemento:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Riavviare Visual Studio Express 2012 per Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Visualizzazione di un errore del server withBrowse file cshtml WithorF5causes

Quando si crea un progetto MVC 5 in Visual Studio 2012 (o aprire il progetto di Visual Studio 2012 un MVC 5 che è stato creato in Visual Studio 2013) e si tenta di visualizzare un file cshtml utilizzando Sfoglia con o F5, si riceverà un errore che indica - **errore Server nel Applicazione '/'**. Il server tenta di passare a`http://localhost:XXXX/Views/../XXXX.cshtml`

Per risolvere questo problema, modificare il **azione di avvio** impostazione nel progetto per **pagina specifica**. Non è necessario fornire un valore per la pagina.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Dopo aver apportato questa modifica, la selezione di F5 consente di passare alla radice dell'applicazione (`http://localhost:XXXX`). Questo comportamento non è lo stesso come il comportamento per i progetti MVC 5 in Visual Studio 2013, in cui il **pagina corrente** impostazione consente di avviare la pagina aperta.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Tilde(~) e riscrittura Url

Dopo l'aggiornamento a 3 Razor di ASP.NET o ASP.NET MVC 5, la notazione tilde(~) potrebbe non funzionare correttamente se si utilizza la riscrittura URL. URL rewrite interessa la notazione tilde(~) negli elementi HTML quali &lt;A /&gt;, &lt;SCRIPT /&gt;, &lt;collegamento /&gt;, e di conseguenza la tilde non viene eseguito il mapping alla directory radice.

Ad esempio, se si riscrivono le richieste per **asp.net/content** a **asp.net**, l'attributo href nel &lt;href = "~/content/" /&gt; si risolve in **/content/ contenuto /** anziché  **/** . Per eliminare questa modifica, è possibile impostare il **IIS\_WasUrlRewritten** contesto su false in ogni pagina Web o in **applicazione\_BeginRequest** in Global. asax.

<a id="templateissue"></a>
### <a name="templates"></a>Modelli

Quando ASP.NET MVC progetti creati con Visual Studio 2012, Windows 8.1 o Windows Server 2012 R2, Visual Studio viene visualizzato un messaggio di errore che indica "Configurazione Web [url] per ASP.NET 4.5 non è riuscita."

![Errore di configurazione](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Questo errore viene visualizzato perché Visual Studio 2012 non abilita la funzionalità ASP.NET 4.5 quando viene installato in tali versioni di Windows. Per abilitare ASP.NET 4.5, eseguire i passaggi descritti in [o disattivazione delle funzionalità Windows attivare](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![attivare o disattivare le funzionalità di Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

In alternativa, è possibile abilitare ASP.NET 4.5 tramite la riga di comando.

1. Aprire il prompt dei comandi in modalità amministratore.
2. Eseguire il comando seguente per abilitare ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
