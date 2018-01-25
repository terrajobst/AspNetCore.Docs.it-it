---
uid: whitepapers/aspnet-web-deployment-content-map
title: Distribuzione Web ASP.NET - consigliato risorse | Documenti Microsoft
author: rick-anderson
description: In questo argomento vengono forniti collegamenti alla documentazione (pubblicazione) ASP.NET web risorse su come distribuire le applicazioni in IIS utilizzando Visual Studio 2010, Visual Web De...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2014
ms.topic: article
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 78ff183394b5ff92f789b50551d01d28f9bff93b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment---recommended-resources"></a>Distribuzione Web ASP.NET - consigliato risorse
====================
> In questo argomento vengono forniti collegamenti alla documentazione (pubblicazione) ASP.NET web risorse su come distribuire le applicazioni in IIS utilizzando Visual Studio 2010, Visual Web Developer 2010 e versioni successive.
> 
> Se si conosce un grande blog post, [stackoverflow](http://stackoverflow.com) thread o qualsiasi altro tipo di collegamento che può essere utile, [inviare un messaggio di posta elettronica](mailto:aspnetue@microsoft.com?subject=Deployment Content Map) con il collegamento.
> 
> > [!NOTE] 
> > 
> > Molte di queste risorse descrivono le funzionalità di distribuzione che sono disponibili solo se si installa una versione recente del [Visual Studio Web pubblica Update](https://go.microsoft.com/fwlink/?LinkID=208120). Alcune delle funzionalità sono disponibili solo in Visual Studio 2012 o Visual Studio 2013.


Di seguito sono elencate le diverse sezioni di questo argomento:

- [Informazioni sulle opzioni di distribuzione per i progetti web](#understanding)
- [Ricerca di provider per un'applicazione ASP.NET di hosting](#findinghosting)
- [Distribuzione di un'applicazione web da Visual Studio](#fromvs)
- [Distribuzione di un'applicazione web creando e installando un pacchetto di distribuzione web](#package)
- [Distribuzione di un'applicazione web tramite un processo di integrazione continua (CI)](#ci)
- [Utilizzo di trasformazioni di Web. config per modificare le impostazioni nel file Web. config di destinazione o nel file app. config durante la distribuzione](#transforms)
- [Utilizzo di parametri di distribuzione Web per modificare le impostazioni dell'applicazione web di destinazione durante la distribuzione](#webdeployparms)
- [Impostazione di un'applicazione che non sia in linea durante la distribuzione](#appoffline)
- [La distribuzione di un database o le modifiche apportate a un database come parte della distribuzione di applicazioni web](#databasewithweb)
- [Distribuzione di un database separatamente dalla distribuzione di applicazioni web](#databaseseparate)
- [Distribuzione di un'applicazione web che utilizza l'applicazione ASP.NET di servizi quali appartenenza e di profilatura](#aspnetmembership)
- [Precompilazione per la distribuzione](#precompiling)
- [Distribuzione di un'applicazione web intranet](#intranet)
- [Automazione delle attività comuni di distribuzione che non sono automatiche predefinita](#automating)
- [Configurazione dei server web in modo che gli sviluppatori possono distribuire applicazioni web tramite distribuzione Web](#configuringservers)
- [Configurazione dei server per un provider di hosting](#hostingprovider)
- [Risoluzione dei problemi di distribuzione](#troubleshooting)
- [Ottenere informazioni della Guida con una domanda specifica distribuzione](#gettinghelp)
- [Risorse aggiuntive](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Informazioni sulle opzioni di distribuzione per i progetti web

- [Cenni preliminari sulla distribuzione di Web per Visual Studio e ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Come distribuire un sito Web di Azure Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Illustra le opzioni e i collegamenti alle risorse per la distribuzione di progetti web per siti Web di Azure, tra cui [il recapito continuo](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatizzato da [controllo del codice sorgente](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) nonché l'utilizzo di Visual Studio.
- [Visual Studio 2012 miglioramenti di pubblicazione Web](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (Video di Scott Hanselman).
- [Post di panoramica per la distribuzione Web in Visual Studio 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (blog di Vishal Joshi). Un post di blog precedente, ma alcune delle risorse di Visual Studio 2010 che fornisca un collegamento per le informazioni che sono ancora rilevanti per Visual Studio 2012.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Ricerca di provider per un'applicazione ASP.NET di hosting

- [ASP.NET Hosting](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Distribuzione di un'applicazione web da Visual Studio

- [Come distribuire un sito Web di Azure Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Illustra le opzioni e fornisce collegamenti a risorse per la distribuzione di progetti web per siti Web di Azure. Include una sezione sulla distribuzione da Visual Studio.
- [Distribuzione Web ASP.NET utilizzando Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie di esercitazioni 12 parti, viene illustrato come distribuire le applicazioni web con i database di SQL Server. Per database di distribuzione utilizza sia il provider dbDacFx migrazioni di Entity Framework Code First. Include anche informazioni su [trasformazioni di Web. config file](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [distribuzione singoli file](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [distribuzione della riga di comando](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), e [come personalizzare il web di Visual Studio pipeline di pubblicazione modificando i file con estensione pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Si applica a tutti i progetti web ASP.NET, inclusi Web Form, MVC e Web API).
- [Procedura: distribuire un utilizzando un solo clic pubblicazione dei progetti Web in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (fare riferimento a informazioni per la procedura guidata di pubblicazione sul Web Visual Studio).
- [Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Si tratta di una versione precedente di **distribuzione Web ASP.NET utilizzando Visual Studio** elencate nella parte superiore di questa sezione. Ora principalmente utile per informazioni su come distribuire database di SQL Server Compact e su come eseguire la migrazione da SQL Server Compact a una versione completa di SQL Server.
- [Applicazione multilivello .NET utilizzando archiviazione tabelle, code e blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sito Web di Microsoft Azure). serie di esercitazioni-parte 5, viene illustrato come creare un progetto MVC e distribuirlo a un servizio Cloud di Windows Azure.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Distribuzione di un'applicazione web creando e installando un pacchetto di distribuzione web

- [Procedura: creare un pacchetto di distribuzione Web in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Procedura: installare un pacchetto di distribuzione utilizzando il File deploy creato da Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Utilizzo di un pacchetto di distribuzione Web per distribuire a IIS nella finestra di sviluppo e in un host di terze parti](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (blog di Hashimi Sayed). Come utilizzare Gestione IIS per installare un pacchetto di distribuzione in IIS nel computer locale e all'hosting di una società che supporta la gestione IIS per l'amministrazione remota.
- [La creazione di un Web distribuire pacchetti da Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (sito web IIS.NET). Sono incluse istruzioni per la creazione del pacchetto dalla riga di comando e l'installazione.
- [Una volta pubblicare qualsiasi parte del pacchetto](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (blog di Hashimi Sayed). Introduce un pacchetto NuGet che consente di automatizzare il processo di trasformazione il file Web. config per più ambienti di destinazione, in modo che è possibile distribuire un pacchetto in più server. Vedere anche il [PackageWeb video](https://www.youtube.com/watch?v=-LvUJFI8CzM) da Hashimi Sayed.

Per ulteriori informazioni, consultare la sezione seguente.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Distribuzione di un'applicazione web tramite un processo di integrazione continua (CI)

- [Integrazione continua e il recapito continuo (creazione di applicazioni Cloud del mondo reale con Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Capitolo di E-book che introduce l'integrazione continua e il recapito continuo.
- [Come distribuire un sito Web di Azure Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Illustra le opzioni e i collegamenti alle risorse per la distribuzione di progetti web per siti Web di Azure. Include una sezione su come automatizzare la distribuzione dal controllo del codice sorgente.
- [Distribuzione di applicazioni Web in scenari aziendali](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). serie di esercitazioni 40 parte, viene illustrato come automatizzare la distribuzione in un processo CI usando Visual Studio 2010 e Team Foundation Server 2010.
- [All'interno del motore di Microsoft Build: utilizzo di MSBuild e Team Foundation Build, Hashimi Sayed e William Bartholomew](http://msbuildbook.com). Si tratta di un libro, non è una risorsa web, ma si tratta di una Guida essenziale per imparare a configurare MSBuild per gli scenari di integrazione continua.
- [Estensione di MSBuild Pack](https://github.com/mikefourie/MSBuildExtensionPack). Include attività di distribuzione.
- [Team Foundation Build personalizzazione Guida](https://aka.ms/vsarsolutions). Documentazione da ALM Rangers sulla configurazione di Team Foundation Server illustrata distribuzione web e include esercitazioni e video.
- [Trasforma SlowCheetah XML da un server CI](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (blog di Hashimi Sayed). Viene illustrato come utilizzare SlowCheetah, componente aggiuntivo di Visual Studio per la trasformazione di App. config e altri file XML.

Vedere anche [assicurandosi di un'applicazione non sia in linea durante la distribuzione](aspnet-web-deployment-content-map.md#appoffline) più avanti in questa pagina.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Utilizzo di trasformazioni di Web. config per modificare le impostazioni nel file Web. config di destinazione o nel file app. config durante la distribuzione

- [Trasformazioni di Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Sintassi di trasformazione Web. config per la distribuzione del progetto Web in Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Strumenti 2012.2 - trasformazioni di Web. config Web](https://www.youtube.com/watch?v=HdPK8mxpKEI) (video di YouTube di Hashimi Sayed). Viene illustrato come impostare e visualizzare in anteprima le trasformazioni di Web. config.
- [Come è possibile disattivare trasformazione Web. config?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Quando utilizzare i parametri di distribuzione Web anziché le trasformazioni di Web. config](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (documento XML Transform) rilasciato sul sito codeplex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (blog di strumenti e sviluppo Web .NET). Annuncia la disponibilità del codice sorgente per il motore di trasformazione del file Web. config e vengono elencati alcuni strumenti che lo utilizzano.
- [Siti Web: Come applicazione stringhe di lavoro di stringhe di connessione Windows Azure e](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog di Microsoft Azure). Consente di trasformare un'alternativa a Web. config se l'ambiente di destinazione sia i siti Web di Azure e si desidera trasformare `appSettings` o `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Utilizzo di parametri di distribuzione Web per modificare le impostazioni dell'applicazione web di destinazione durante la distribuzione

- [Procedura: utilizzare Web distribuire parametri in un pacchetto di distribuzione Web](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: Come aggiornare le impostazioni dell'app nella pubblicazione basato sul profilo pubblica](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (blog di Hashimi Sayed). Viene illustrato come integrare distribuzione Web parametri in Visual Studio di profili di pubblicazione.
- [Distribuire la parametrizzazione di Web](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (sito web IIS.NET).
- [Distribuire la parametrizzazione nell'azione Web](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (blog di Vishal Joshi).
- [Visual Studio la parametrizzazione di distribuzione Web. Trasformazione Web. config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (blog di Vishal Joshi).
- [Siti Web: Come applicazione stringhe di lavoro di stringhe di connessione Windows Azure e](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog di Microsoft Azure). Un'alternativa alla Web distribuire parametri se l'ambiente di destinazione sia i siti Web di Azure e si vuole parametrizzare `appSettings` o `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Impostazione di un'applicazione che non sia in linea durante la distribuzione

- [Distribuzione Web ASP.NET utilizzando Visual Studio: distribuzione di un aggiornamento del codice](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Vedere la sezione **eseguire l'applicazione durante la distribuzione.**
- [Disconnettere un'applicazione prima della pubblicazione](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.net sito). Viene illustrata una funzionalità incorporata di Web Deploy 3.0 che consente di automatizzare la gestione di un'app\_offline.htm file. Questa funzionalità non funziona con app personalizzata\_offline.htm file.
- [Come eseguire l'app web non in linea durante la pubblicazione](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (blog di Hashimi Sayed). Come automatizzare il processo di utilizzo di un'app personalizzata\_offline.htm file.
- [Pubblicazione di aggiornamenti per app non in linea e usechecksum Web](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (blog di sviluppo Web Microsoft). Un'altra opzione per l'automatizzazione di uso dell'app\_offline.htm file.
- [Web distribuire RTW 3.5](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net sito). Nuova funzionalità di 3.5 di distribuzione Web per app personalizzata\_offline.htm file.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>La distribuzione di un database o le modifiche apportate a un database come parte della distribuzione di applicazioni web

- [Configurazione della distribuzione di Database in Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Panoramica delle opzioni per la distribuzione di un database con un progetto web.
- [Distribuzione Web ASP.NET utilizzando Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie di esercitazioni 12 parti, Mostra la distribuzione del database utilizzando provider dbDacFx e migrazioni di Entity Framework Code First.
- [Procedura: distribuire un sito Web progetto con un clic pubblica in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Distribuire un'app protetta ASP.NET MVC 5 con appartenenza, OAuth e il Database SQL in un sito Web di Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un'esercitazione lungo che crea e distribuisce un'applicazione che utilizza un singolo SQL Server database sia per i dati di appartenenza e l'applicazione.
- [Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). serie di esercitazioni 12 parti, viene illustrato come distribuire i database di SQL Server Compact e come eseguire la migrazione da SQL Server Compact a una versione completa di SQL Server.

Vedere anche la distribuzione di un'applicazione web per la creazione e l'installazione di un pacchetto di distribuzione web e la distribuzione di un'applicazione web tramite un processo di integrazione continua (CI) precedentemente in questa pagina.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Distribuzione di un database separatamente dalla distribuzione di applicazioni web

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Inclusi i dati in un progetto di Database di SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (blog del team di SQL Server Data Tools). Come distribuire schema e dei dati durante la distribuzione di un database.
- [Come distribuire un Database in Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (sito Web di Microsoft Azure)
- [Migrazione di database a Database SQL di Azure (precedentemente denominato SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migrazione di un Database a SQL Azure mediante SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (blog del team di SQL Server Data Tools).
- [Migrazione di applicazioni incentrate sui dati in Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [La migrazione di database di SQL Server per Database SQL di Azure](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Distribuzione di un'applicazione web che utilizza l'applicazione ASP.NET di servizi quali appartenenza e di profilatura

- [Distribuire un'app protetta ASP.NET MVC 5 con appartenenza, OAuth e il Database SQL in un sito Web di Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un'esercitazione lungo che crea e distribuisce un'applicazione che utilizza un singolo SQL Server database sia per i dati di appartenenza e l'applicazione.
- [ASP.NET Identity](https://asp.net/identity/). Risorse per l'identità ASP.NET.
- [Distribuzione Web ASP.NET utilizzando Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie di esercitazioni 12 parti, viene illustrato come distribuire un database delle appartenenze ASP.NET.
- [Configurazione di un sito Web che utilizza i servizi delle applicazioni](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Per il sito web progetti ma risulta utile anche per i progetti di applicazione web.
- [Utenti e ruoli del sito Web di produzione](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Per il sito web progetti ma risulta utile anche per i progetti di applicazione web.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Precompilazione per la distribuzione

- [Cenni preliminari sulla precompilazione di progetto applicazione Web ASP.NET](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Pubblicazione/creazione pacchetto Web scheda, le proprietà del progetto](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Advanced precompilare la finestra di dialogo Impostazioni](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Distribuzione di un'applicazione web intranet

- [Utilizzare l'opzione di autenticazione aziendale locale (ADFS) con ASP.NET in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog da Vittorio Bertocci).
- [Come creare un sito Intranet mediante ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Scritti procedura dettagliata precedente di Visual Studio 2010, non riflette le modifiche principali nei modelli di progetto intranet introdotti in Visual Studio 2013.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automazione delle attività comuni di distribuzione che non sono automatiche predefinita

- [Distribuzione Web ASP.NET utilizzando Visual Studio: distribuire file aggiuntivi](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Impostazione delle autorizzazioni di cartella per la pubblicazione Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (blog di Hashimi Sayed).
- [Come estendere il file targets per includere le impostazioni del Registro di sistema per un pacchetto del progetto web](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (blog di strumenti di sviluppo Web).
- [Estensione di trasformazione XML (Web. config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (blog di Hashimi Sayed). Viene illustrato come creare trasformazioni XDT personalizzate.
- [Web (MSDeploy) dello strumento di distribuzione personalizzato Provider Take 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (blog di Hashimi Sayed). Viene illustrato come creare un provider personalizzato di distribuzione Web.
- [Come creare un pacchetto e distribuire i componenti COM](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (blog di strumenti di sviluppo Web).
- [Assembly .NET come](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (blog di strumenti di sviluppo Web). Come distribuire l'assembly nella Global Assembly Cache.
- [Nello script tutti gli elementi - inizializzazione di macchina virtuale di Windows Azure per il Server Web con IIS, distribuzione Web e altri elementi](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (blog di Tugberk Ugurlu).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Configurazione dei server web in modo che gli sviluppatori possono distribuire applicazioni web tramite distribuzione Web

- [Installazione e configurazione di distribuzione Web per l'amministratore e le distribuzioni non amministratore](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net sito).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Configurazione dei server per un provider di hosting

- [Guida alla distribuzione di Hosting di Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (area Download Microsoft).
- [Generare un File XML del profilo](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.net sito).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Risoluzione dei problemi di distribuzione

- [Risoluzione dei problemi di siti Web di Azure in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (sito Web di Microsoft Azure).
- [Distribuzione Web ASP.NET utilizzando Visual Studio: risoluzione dei problemi](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Risoluzione dei problemi comuni con Web distribuire](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Codici di errore di distribuzione Web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net sito).
- [Domande frequenti sulla distribuzione per Visual Studio e ASP.NET Web](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Principali differenze tra IIS e il Server di sviluppo ASP.NET](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Differenze di configurazione comuni tra lo sviluppo e produzione](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hosting di applicazioni ASP.NET in attendibilità media](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 Guy dal sito Rolla).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Ottenere informazioni della Guida con una domanda specifica distribuzione

- [Forum sulla distribuzione e configurazione di ASP.NET](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Risorse aggiuntive

In questa sezione vengono forniti collegamenti a risorse aggiuntive che sono utili per acquisire ulteriori informazioni sull'utilizzo degli strumenti di distribuzione di Visual Studio e IIS.

I seguenti blog spesso contengono informazioni sulla distribuzione web di Visual Studio:

- [Web gli strumenti di sviluppo Microsoft blog](https://blogs.msdn.com/b/webdevtools/).
- [Blog del Hashimi sayed](http://www.sedodream.com/).

Le seguenti risorse forniscono la documentazione sulla distribuzione Web, il framework IIS utilizzate in Visual Studio per eseguire attività di distribuzione progetto applicazione web. È possibile porre domande su distribuzione Web nel [forum di strumento di distribuzione Web](https://go.microsoft.com/fwlink/?LinkId=149411) nel sito web IIS.net.

- [Introduzione a Web distribuire](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Distribuire l'installazione e configurazione del sito Web](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Il programma di installazione di distribuire gli script di PowerShell per automatizzare Web](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Strumento di distribuzione Web](https://go.microsoft.com/fwlink/?LinkId=151481). Tabella di primo livello del nodo di contenuto per la documentazione di distribuzione Web nel sito TechNet. Include informazioni di riferimento utili ma la maggior parte delle pagine non sono state aggiornate per gli anni di TechNet.
- [Microsoft Namespace](https://go.microsoft.com/fwlink/?LinkId=148630). Documentazione dell'API, non è stato aggiornato dalla versione 1.0.
- [Blog del Team di distribuzione Web Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [Scheda di pubblicazione nel sito web IIS.net](https://www.iis.net/learn/publish).
