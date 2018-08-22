---
uid: whitepapers/aspnet-web-deployment-content-map
title: Distribuzione Web ASP.NET - risorse consigliate | Microsoft Docs
author: rick-anderson
description: In questo argomento vengono forniti collegamenti alla documentazione di risorse su come distribuire, pubblicare, ASP.NET web alle applicazioni di IIS utilizzando Visual Studio 2010, Visual Web De...
ms.author: riande
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: c970d929c4e6b581bedd2947982926ac448facfa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828947"
---
<a name="aspnet-web-deployment---recommended-resources"></a>Distribuzione Web ASP.NET - risorse consigliate
====================
> In questo argomento vengono forniti collegamenti alla documentazione di risorse su come distribuire, pubblicare, ASP.NET web alle applicazioni di IIS utilizzando Visual Studio 2010, Visual Web Developer 2010 e versioni successive.
> 
> Se si conosce un grande blog post [stackoverflow](http://stackoverflow.com) thread o qualsiasi altro tipo di collegamento che può essere utile, [inviarci un messaggio di posta elettronica](mailto:aspnetue@microsoft.com?subject=Deployment Content Map) con il collegamento.
> 
> > [!NOTE] 
> > 
> > Molte di queste risorse vengono descritte le funzionalità di distribuzione che sono disponibili solo se si installa una versione recente del [Visual Studio Web pubblica Update](https://go.microsoft.com/fwlink/?LinkID=208120). Alcune delle funzionalità sono disponibili solo in Visual Studio 2012 o Visual Studio 2013.


Di seguito sono elencate le diverse sezioni di questo argomento:

- [Informazioni sulle opzioni di distribuzione per i progetti web](#understanding)
- [Individuazione provider per un'applicazione ASP.NET di hosting](#findinghosting)
- [Distribuzione di un'applicazione web da Visual Studio](#fromvs)
- [Distribuzione di un'applicazione web creando e installando un pacchetto di distribuzione web](#package)
- [Distribuzione di un'applicazione web tramite un processo di integrazione continua (CI)](#ci)
- [Uso di trasformazioni di Web. config per modificare le impostazioni nel file Web. config di destinazione o nel file app. config durante la distribuzione](#transforms)
- [Utilizzo di parametri di distribuzione Web per modificare le impostazioni dell'applicazione web di destinazione durante la distribuzione](#webdeployparms)
- [Assicurarsi di un'applicazione è offline durante la distribuzione](#appoffline)
- [Distribuzione di un database o le modifiche apportate a un database come parte della distribuzione di applicazioni web](#databasewithweb)
- [Distribuzione di un database separatamente dalla distribuzione di applicazioni web](#databaseseparate)
- [Distribuzione di un'applicazione web che usa l'applicazione ASP.NET di servizi, ad esempio l'appartenenza e la profilatura](#aspnetmembership)
- [Precompilazione per la distribuzione](#precompiling)
- [Distribuzione di un'applicazione web intranet](#intranet)
- [Automazione delle attività di distribuzione comuni che vengono automatizzate non predefiniti](#automating)
- [Configurazione dei server web in modo che gli sviluppatori possono distribuire applicazioni web tramite distribuzione Web](#configuringservers)
- [Configurare i server per un provider di hosting](#hostingprovider)
- [Risoluzione dei problemi di distribuzione](#troubleshooting)
- [Ottenere informazioni della Guida con una domanda di distribuzione specifico](#gettinghelp)
- [Risorse aggiuntive](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Informazioni sulle opzioni di distribuzione per i progetti web

- [Panoramica di distribuzione Web per Visual Studio e ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Come distribuire un sito Web di Azure Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Illustra le opzioni e i collegamenti alle risorse per la distribuzione di progetti web per siti Web di Azure, tra cui [recapito continuo](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatico dal [controllo del codice sorgente](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) e l'uso di Visual Studio.
- [Miglioramenti di pubblicazione Web di Visual Studio 2012](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (Video di Scott Hanselman).
- [Post di panoramica per la distribuzione Web in Visual Studio 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (blog di Vishal Joshi). Un post di blog precedenti, ma alcune delle risorse di Visual Studio 2010 che fornisca un collegamento per le informazioni che sono ancora rilevanti per Visual Studio 2012.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Individuazione provider per un'applicazione ASP.NET di hosting

- [Hosting di ASP.NET](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Distribuzione di un'applicazione web da Visual Studio

- [Come distribuire un sito Web di Azure Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Illustra le opzioni e vengono forniti collegamenti alle risorse per la distribuzione di progetti web per siti Web di Azure. Include una sezione sulla distribuzione da Visual Studio.
- [Distribuzione Web ASP.NET tramite Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie di esercitazioni 12 parti illustra come distribuire applicazioni web con database di SQL Server. Per database di distribuzione utilizza il provider dbDacFx e migrazioni di Entity Framework Code First. Include anche informazioni sulle [trasformazioni del file Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [distribuzione di file singoli](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [distribuzione da riga di comando](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), e [come personalizzare il web di Visual Studio pipeline di pubblicazione modificando i file con estensione pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Si applica a tutti i progetti web ASP.NET, tra cui Web Form, MVC e API Web).
- [Procedura: distribuire un Web progetto tramite un solo clic la pubblicazione in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (informazioni di riferimento per la procedura guidata di pubblicazione di Visual Studio Web.)
- [Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Si tratta di una versione precedente di **distribuzione Web ASP.NET tramite Visual Studio** elencati nella parte superiore di questa sezione. Ora principalmente utile per informazioni su come distribuire i database di SQL Server Compact e come eseguire la migrazione da SQL Server Compact a una versione completa di SQL Server.
- [Applicazione multilivello .NET usando archiviazione tabelle, code e blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sito Web Microsoft Azure). serie di esercitazioni 5 parti, viene illustrato come creare un progetto MVC e distribuirla in un servizio Cloud di Azure.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Distribuzione di un'applicazione web creando e installando un pacchetto di distribuzione web

- [Procedura: creare un pacchetto di distribuzione Web in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Procedura: installare un pacchetto di distribuzione usando il File deploy. cmd creato da Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Uso di un pacchetto di distribuzione Web per distribuire IIS nella finestra di sviluppo e a un host di terze parti](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (blog di Sayed Hashimi). Come usare Gestione IIS per installare un pacchetto di distribuzione in IIS nel computer locale e in un tipo di hosting della società che supporta la gestione IIS per amministrazione remota.
- [Creazione di una Web distribuire pacchetti da Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (sito web IIS.NET). Sono incluse istruzioni per la creazione del pacchetto dalla riga di comando e l'installazione.
- [Creare un pacchetto una volta pubblica ovunque](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (blog di Sayed Hashimi). Introduce un pacchetto NuGet che consente di automatizzare il processo di trasformazione del file Web. config per più ambienti di destinazione, in modo che è possibile distribuire un pacchetto in più server. Vedere anche il [PackageWeb video](https://www.youtube.com/watch?v=-LvUJFI8CzM) da Sayed Hashimi.

Vedere anche la sezione seguente.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Distribuzione di un'applicazione web tramite un processo di integrazione continua (CI)

- [Integrazione continua e recapito continuo (creazione di App Cloud reali con Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Capitolo di eBook che introduce l'integrazione continua e recapito continuo.
- [Come distribuire un sito Web di Azure Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Illustra le opzioni e i collegamenti alle risorse per la distribuzione di progetti web per siti Web di Azure. Include una sezione sull'automazione della distribuzione dal controllo del codice sorgente.
- [Distribuzione di applicazioni Web in scenari aziendali](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). serie di esercitazioni 40 parti illustra come automatizzare la distribuzione in un processo di integrazione continua usando Visual Studio 2010 e Team Foundation Server 2010.
- [All'interno del motore di Microsoft Build: utilizzo di MSBuild e Team Foundation Build, Sayed Hashimi, William Bartholomew](http://msbuildbook.com). Si tratta di un libro, non è una risorsa web, ma è una Guida essenziale per imparare a configurare MSBuild per scenari di integrazione continua.
- [Pacchetto di estensioni di MSBuild](https://github.com/mikefourie/MSBuildExtensionPack). Include attività di distribuzione.
- [Team Foundation Build Customization Guide](https://aka.ms/vsarsolutions). Documentazione da ALM Rangers sulla configurazione di Team Foundation Server include informazioni sulla distribuzione web e include le esercitazioni e video.
- [Le trasformazioni SlowCheetah XML da un server CI](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (blog di Sayed Hashimi). Illustra come usare SlowCheetah, componente aggiuntivo di Visual Studio per la trasformazione di App. config e altri file XML.

Vedere anche [assicurandosi di un'applicazione non in linea durante la distribuzione è](aspnet-web-deployment-content-map.md#appoffline) più avanti in questa pagina.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Uso di trasformazioni di Web. config per modificare le impostazioni nel file Web. config di destinazione o nel file app. config durante la distribuzione

- [Trasformazioni di Web. config File](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Sintassi di trasformazione Web. config per la distribuzione del progetto Web con Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Web Tools 2012.2 - trasformazioni di Web. config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (video di YouTube di Sayed Hashimi). Viene illustrato come configurare e visualizzare in anteprima le trasformazioni di Web. config.
- [Come si disabilita trasformazione Web. config?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Quando usare i parametri di distribuzione Web invece trasformazioni di Web. config?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (XML documento Transform) rilasciato sul sito codeplex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (blog di strumenti e sviluppo Web .NET). Annuncia la disponibilità del codice sorgente per il motore di trasformazione del file Web. config e vengono elencati alcuni strumenti che lo usano.
- [Siti Web di Azure di Windows: Come applicazione stringhe and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog di Microsoft Azure). Consente di trasformare un'alternativa al Web. config se l'ambiente di destinazione è siti Web di Azure e si desidera trasformare `appSettings` o `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Utilizzo di parametri di distribuzione Web per modificare le impostazioni dell'applicazione web di destinazione durante la distribuzione

- [Procedura: i parametri in un pacchetto di distribuzione Web di distribuzione Web usare](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: Come aggiornare le impostazioni dell'app nella pubblicazione basato sul profilo di pubblicazione](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (blog di Sayed Hashimi). Viene illustrato come integrare Web i parametri di distribuzione in Visual Studio i profili di pubblicazione.
- [Distribuire la parametrizzazione di Web](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (sito web IIS.NET).
- [Distribuire la parametrizzazione in azione Web](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (blog di Vishal Joshi).
- [Visual Studio distribuisce la parametrizzazione di Web. Trasformazione Web. config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (blog di Vishal Joshi).
- [Siti Web di Azure di Windows: Come applicazione stringhe and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog di Microsoft Azure). Un'alternativa al Web i parametri di distribuzione se l'ambiente di destinazione sia siti Web di Azure e si vuole parametrizzare `appSettings` o `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Assicurarsi di un'applicazione è offline durante la distribuzione

- [Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di un aggiornamento del codice](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Vedere la sezione **portare offline l'applicazione durante la distribuzione.**
- [Disconnettere un'applicazione prima della pubblicazione](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.net sito). Viene illustrata una funzionalità integrate di Web Deploy 3.0 che consente di automatizzare la gestione di un'app\_offline.htm file. Questa funzionalità non funziona con un'app personalizzata\_offline.htm file.
- [Come eseguire l'app web in modalità offline durante la pubblicazione](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (blog di Sayed Hashimi). Come automatizzare il processo d'uso di un'app personalizzata\_offline.htm file.
- [Web pubblicazione degli aggiornamenti per l'app non in linea e usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (blog di sviluppo Web Microsoft). Un'altra opzione per l'uso dell'app automazione\_offline.htm file.
- [Web distribuire RTW 3.5](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net sito). Nuova funzionalità di 3.5 distribuzione Web per app personalizzata\_offline.htm file.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Distribuzione di un database o le modifiche apportate a un database come parte della distribuzione di applicazioni web

- [Configura la distribuzione del Database in Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Panoramica delle opzioni per la distribuzione di un database con un progetto web.
- [Distribuzione Web ASP.NET tramite Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie esercitazioni in 12 parti, illustra la distribuzione del database tramite provider dbDacFx e migrazioni di Entity Framework Code First.
- [Procedura: distribuire un sito Web pubblica progetto usando un solo clic in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e Database SQL in un sito Web di Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un'esercitazione lunga che compila e distribuisce un'applicazione che usa un singolo SQL Server database sia per i dati di appartenenza e l'applicazione.
- [Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). serie esercitazioni in 12 parti, viene illustrato come distribuire i database di SQL Server Compact e come eseguire la migrazione da SQL Server Compact a una versione completa di SQL Server.

Vedere anche la distribuzione di un'applicazione web creando e installando un pacchetto di distribuzione web e distribuire un'applicazione web tramite un processo di integrazione continua (CI) in precedenza in questa pagina.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Distribuzione di un database separatamente dalla distribuzione di applicazioni web

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Inclusi i dati in un progetto di Database di SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (blog del team di SQL Server Data Tools). Come distribuire lo schema sia dei dati durante la distribuzione di un database.
- [Come distribuire un Database in Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (sito Web Microsoft Azure)
- [Migrazione di database a Database SQL di Azure (precedentemente SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migrazione di un Database a SQL Azure mediante SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (blog del team di SQL Server Data Tools).
- [Migrazione di applicazioni incentrate sui dati in Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [La migrazione di database di SQL Server al Database SQL di Azure Windows](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Distribuzione di un'applicazione web che usa l'applicazione ASP.NET di servizi, ad esempio l'appartenenza e la profilatura

- [Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e Database SQL in un sito Web di Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un'esercitazione lunga che compila e distribuisce un'applicazione che usa un singolo SQL Server database sia per i dati di appartenenza e l'applicazione.
- [ASP.NET Identity](https://asp.net/identity/). Risorse per ASP.NET Identity.
- [Distribuzione Web ASP.NET tramite Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie di esercitazioni 12 parti illustra come distribuire un database di appartenenze ASP.NET.
- [Configurazione di un sito Web che utilizza i servizi dell'applicazione](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Per il sito web progetti ma è anche rilevanti per i progetti applicazione web.
- [Utenti e ruoli nel sito Web di produzione](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Per il sito web progetti ma è anche rilevanti per i progetti applicazione web.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Precompilazione per la distribuzione

- [Cenni preliminari sulla precompilazione di progetto applicazione Web ASP.NET](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Pubblicazione/creazione pacchetto Web scheda, le proprietà del progetto](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Advanced precompilare finestra di dialogo Impostazioni](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Distribuzione di un'applicazione web intranet

- [Usare l'opzione di autenticazione aziendale locale (ADFS) con ASP.NET core in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog di Vittorio Bertocci.).
- [Come creare un sito Intranet mediante ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Precedente procedura dettagliata scritto per Visual Studio 2010, non riflette le modifiche principali nei modelli di progetto intranet introdotti in Visual Studio 2013.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automazione delle attività di distribuzione comuni che vengono automatizzate non predefiniti

- [Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di file aggiuntivi](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Impostazione delle autorizzazioni di cartella nel sito Web-pubblicazione](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (blog di Sayed Hashimi).
- [Come estendere il file di destinazioni per includere le impostazioni del Registro di sistema per un pacchetto del progetto web](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (blog di strumenti di sviluppo Web).
- [Estensione di trasformazione XML (Web. config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (blog di Sayed Hashimi). Viene illustrato come creare trasformazioni XDT personalizzate.
- [Web (MSDeploy) dello strumento di distribuzione personalizzato Provider Take 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (blog di Sayed Hashimi). Viene illustrato come creare un provider personalizzato di distribuzione Web.
- [Come creare un pacchetto e distribuire i componenti COM](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (blog di strumenti di sviluppo Web).
- [Come assembly .NET pacchetto](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (blog di strumenti di sviluppo Web). Come distribuire gli assembly nella Global Assembly Cache.
- [Nello script tutti gli elementi - Initialize Your Windows VM di Azure per il proprio Server Web con IIS, Web Deploy e altri elementi](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (blog di Tugberk Ugurlu).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Configurazione dei server web in modo che gli sviluppatori possono distribuire applicazioni web tramite distribuzione Web

- [Installazione e configurazione di distribuzione Web per le distribuzioni non di amministratore e amministratore](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net sito).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Configurare i server per un provider di hosting

- [Guida alla distribuzione di Hosting di Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (area download Microsoft).
- [Generare un File XML del profilo](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.net sito).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Risoluzione dei problemi di distribuzione

- [Risoluzione dei problemi di siti Web di Azure in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (sito Web Microsoft Azure).
- [Distribuzione Web ASP.NET tramite Visual Studio: risoluzione dei problemi](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Risoluzione dei problemi comuni con Web distribuire](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Codici di errore di distribuzione Web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net sito).
- [Domande frequenti sulla distribuzione per Visual Studio e ASP.NET Web](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Principali differenze tra IIS e ASP.NET Development Server](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Differenze di configurazione più comuni tra sviluppo e produzione](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hosting di applicazioni ASP.NET in attendibilità media](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 Guy dal sito Rolla).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Ottenere informazioni della Guida con una domanda di distribuzione specifico

- [Forum di distribuzione e configurazione di ASP.NET](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Risorse aggiuntive

In questa sezione vengono forniti collegamenti a risorse aggiuntive che sono utili per ulteriori informazioni su come usare gli strumenti di distribuzione di Visual Studio e IIS.

I blog seguenti sono spesso contengono informazioni sulla distribuzione web di Visual Studio:

- [Strumenti di sviluppo Web nei blog Microsoft](https://blogs.msdn.com/b/webdevtools/).
- [Blog di sayed Hashimi](http://www.sedodream.com/).

Le risorse seguenti forniscono documentazione sulla distribuzione Web, il framework IIS utilizzata da Visual Studio per eseguire attività di distribuzione progetto applicazione web. È possibile porre domande su distribuzione Web nel [forum di strumento di distribuzione Web](https://go.microsoft.com/fwlink/?LinkId=149411) sul sito web IIS.net.

- [Introduzione a Web distribuire](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Installazione e configurazione di Web distribuire](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Il programma di installazione di distribuire gli script di PowerShell per automatizzare Web](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Strumento di distribuzione Web](https://go.microsoft.com/fwlink/?LinkId=151481). Tabella di primo livello del nodo di contenuto per la distribuzione Web di documentazione nel sito TechNet. Include informazioni di riferimento utile, ma la maggior parte delle pagine non sono state aggiornate per anni TechNet.
- [Microsoft Namespace](https://go.microsoft.com/fwlink/?LinkId=148630). Documentazione dell'API, non è stato aggiornato dalla versione 1.0.
- [Blog del Team di distribuzione Web Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [Scheda pubblica nel sito web IIS.net](https://www.iis.net/learn/publish).
