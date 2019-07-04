---
title: Profili di pubblicazione di Visual Studio per la distribuzione di app ASP.NET Core
author: rick-anderson
description: Informazioni su come creare profili di pubblicazione in Visual Studio e usarli per la gestione delle distribuzioni di app ASP.NET Core in varie destinazioni.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/18/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: ac243a3898553b2e14a6c15d311afaf62f112a24
ms.sourcegitcommit: a1283d486ac1dcedfc7ea302e1cc882833e2c515
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207818"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Profili di pubblicazione di Visual Studio per la distribuzione di app ASP.NET Core

Di [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo documento descrive come usare Visual Studio 2017 o versioni successive per la creazione e l'uso di profili di pubblicazione. I profili di pubblicazione creati con Visual Studio possono essere eseguiti da MSBuild e Visual Studio. Per istruzioni sulla pubblicazione in Azure, vedere [Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).

Il comando `dotnet new mvc` genera un file di progetto che contiene l'elemento `<Project>` di primo livello seguente:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

L'attributo `Sdk` dell'elemento `<Project>` precedente importa le [proprietà](/visualstudio/msbuild/msbuild-properties) e le [destinazioni](/visualstudio/msbuild/msbuild-targets) di MSBuild rispettivamente da *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.props* e *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*. Il percorso predefinito per `$(MSBuildSDKsPath)` (con Visual Studio 2019 Enterprise) è la cartella *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks*.

`Microsoft.NET.Sdk.Web` (Web SDK) dipende da altri SDK, tra cui `Microsoft.NET.Sdk` (.NET Core SDK) e `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)). Vengono importate le proprietà e le destinazioni di MSBuild associate a ogni SDK dipendente. Le destinazioni di pubblicazione importano il set appropriato di destinazioni in base al metodo di pubblicazione usato.

Quando MSBuild o Visual Studio carica un progetto, vengono eseguite le azioni di alto livello seguenti:

* Compilare un progetto
* Calcolare i file da pubblicare
* Pubblicare i file nella destinazione

## <a name="compute-project-items"></a>Calcolare gli elementi del progetto

Quando viene caricato il progetto, vengono calcolati gli [elementi del progetto di MSBuild](/visualstudio/msbuild/common-msbuild-project-items) (file). Il tipo di elemento determina la modalità di elaborazione del file. Per impostazione predefinita, i file *cs* sono inclusi nell'elenco di elementi `Compile`. I file presenti nell'elenco di elementi `Compile` vengono compilati.

L'elenco di elementi `Content` contiene i file che sono pubblicati in aggiunta agli output di compilazione. Per impostazione predefinita, i file corrispondenti ai criteri `wwwroot\**`, `**\*.config` e `**\*.json` vengono inclusi nell'elenco di elementi `Content`. Il [criterio GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns) `wwwroot\**`, ad esempio, specifica tutti i file nella cartella *wwwroot* **e** relative sottocartelle.

::: moniker range=">= aspnetcore-3.0"

Web SDK importa [Razor SDK](xref:razor-pages/sdk). Di conseguenza, anche i file corrispondenti ai criteri `**\*.cshtml` e `**\*.razor` vengono inclusi nell'elenco di elementi `Content`.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Web SDK importa [Razor SDK](xref:razor-pages/sdk). Di conseguenza, anche i file corrispondenti al criterio `**\*.cshtml` vengono inclusi nell'elenco di elementi `Content`.

::: moniker-end

Per aggiungere esplicitamente un file all'elenco di pubblicazione, aggiungere il file direttamente nel file con estensione *csproj*, come illustrato nella sezione [File di inclusione](#include-files).

Quando si seleziona il pulsante **Pubblica** in Visual Studio o quando si pubblica dalla riga di comando:

* Vengono calcolati le proprietà/gli elementi (i file necessari per compilare).
* **Solo Visual Studio**: i pacchetti NuGet vengono ripristinati. (Il ripristino deve essere esplicito da parte dell'utente nell'interfaccia della riga di comando.)
* Il progetto viene compilato.
* Vengono calcolati gli elementi di pubblicazione (i file necessari per pubblicare).
* Il progetto viene pubblicato (i file calcolati vengono copiati nella destinazione di pubblicazione).

Quando un progetto ASP.NET Core fa riferimento a `Microsoft.NET.Sdk.Web` nel file di progetto, nella radice della directory dell'app Web viene posizionato un file *app_offline.htm*. Quando il file è presente, il modulo ASP.NET Core arresta normalmente l'app e rende disponibile il file *app_offline.htm* durante la distribuzione. Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Pubblicazione di base dalla riga di comando

La pubblicazione dalla riga di comando funziona su tutte le piattaforme supportate da .NET Core e non richiede Visual Studio. Negli esempi seguenti il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) viene eseguito dalla directory del progetto (che contiene il file con estensione *csproj*). Se non si è nella cartella del progetto, passare esplicitamente il percorso del file del progetto. Ad esempio:

```console
dotnet publish C:\Webs\Web1
```

Eseguire i comandi seguenti per creare e pubblicare un'app Web:

```console
dotnet new mvc
dotnet publish
```

Il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) genera un output analogo a quello illustrato di seguito:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {version} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\publish\
```

La cartella di pubblicazione predefinita è `bin\$(Configuration)\netcoreapp<version>\publish`. Il valore predefinito per `$(Configuration)` è *Debug*. Nell'esempio precedente, `<TargetFramework>` è `netcoreapp{X.Y}`.

`dotnet publish -h` visualizza informazioni della Guida per la pubblicazione.

Il comando seguente specifica un build `Release` e la directory di pubblicazione:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

Il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) chiama MSBuild, che richiama la destinazione `Publish`. Tutti i parametri passati a `dotnet publish` vengono passati a MSBuild. Il parametro `-c` esegue il mapping alla proprietà di MSBuild `Configuration`. Il parametro `-o` esegue il mapping a `OutputPath`.

È possibile passare le proprietà di MSBuild usando uno dei formati seguenti:

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Il comando seguente pubblica un build `Release` a una condivisione di rete:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

La condivisione di rete è specificata con le barre ( *//r8/* ) e funziona su tutte le piattaforme .NET Core supportate.

Verificare che l'app pubblicata per la distribuzione non sia in esecuzione. I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app. La distribuzione non viene eseguita perché non è possibile copiare i file bloccati.

## <a name="publish-profiles"></a>Profili di pubblicazione

Questa sezione usa Visual Studio 2017 o versioni successive per creare un profilo di pubblicazione. Dopo la creazione del profilo, la pubblicazione è disponibile da Visual Studio o dalla riga di comando.

I profili di pubblicazione possono semplificare il processo di pubblicazione e può essere presente un numero illimitato di profili. Creare un profilo di pubblicazione in Visual Studio scegliendo uno dei seguenti percorsi:

* Fare clic con il pulsante destro del mouse in **Esplora soluzioni**, quindi scegliere **Pubblica**.
* Selezionare **Pubblica {NOME PROGETTO}** dal menu **Compila**.

Verrà visualizzata la scheda **Pubblica** della pagina di capacità dell'app. Se il progetto non dispone di un profilo di pubblicazione, viene visualizzata la pagina seguente:

![Scheda Pubblica della pagina di capacità dell'app](visual-studio-publish-profiles/_static/az.png)

Quando è selezionata l'opzione **Cartella**, specificare il percorso di una cartella in cui archiviare gli asset pubblicati. La cartella predefinita è *bin\Release\PublishOutput*. Fare clic sul pulsante **Crea profilo** per terminare.

Dopo aver creato un profilo di pubblicazione, la scheda **Pubblica** cambia. Il nuovo profilo creato viene visualizzato in un elenco a discesa. Fare clic su **Crea nuovo profilo** per creare un altro nuovo profilo.

![Scheda Pubblica della pagina di capacità dell'app che visualizza FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

La pubblicazione guidata supporta le seguenti destinazioni di pubblicazione:

* Servizio app di Azure
* Macchine virtuali di Azure
* IIS, FTP e così via (per qualunque server Web)
* Cartella
* Importa profilo

Per altre informazioni, vedere [Quali sono le opzioni di pubblicazione più adatte?](/visualstudio/ide/not-in-toc/web-publish-options).

Quando si crea un profilo di pubblicazione con Visual Studio, viene creato un file di MSBuild *Properties/PublishProfiles/{NOME PROFILO}.pubxml*. Il file *.pubxml* è un file di MSBuild e contiene le impostazioni di configurazione della pubblicazione. Questo file può essere modificato per personalizzare il processo di compilazione e di pubblicazione. Questo file viene letto dal processo di pubblicazione. `<LastUsedBuildConfiguration>` è speciale perché è una proprietà globale e non deve essere presente in alcun file importato nella compilazione. Per altre informazioni, vedere [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: come impostare la proprietà di configurazione).

Quando si esegue la pubblicazione in una destinazione di Azure, il file *.pubxml* contiene l'identificatore della sottoscrizione Azure. Con tale tipo di destinazione, è sconsigliato aggiungere questo file al controllo del codice sorgente. Quando si pubblica in una destinazione non Azure, è possibile archiviare il file *.pubxml*.

Le informazioni riservate, come la password di pubblicazione, vengono crittografate a livello di singolo utente/computer. Sono memorizzate nel file *Properties/PublishProfiles/{NOME PROFILO}.pubxml.user*. Poiché questo file può contenere informazioni riservate, non deve essere archiviato nel controllo del codice sorgente.

Per una panoramica su come pubblicare un'app Web in ASP.NET Core, vedere [Ospitare e distribuire](xref:host-and-deploy/index). Le attività e le destinazioni di MSBuild necessarie per pubblicare un'app ASP.NET Core sono open source in [aspnet/websdk repository](https://github.com/aspnet/websdk).

`dotnet publish` può usare profili di pubblicazione di tipo cartella, MSDeploy e [Kudu](https://github.com/projectkudu/kudu/wiki):

Cartella (multipiattaforma):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (attualmente funziona solo in Windows poiché MSDeploy non è multipiattaforma):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

Pacchetto MSDeploy (attualmente funziona solo in Windows poiché MSDeploy non è multipiattaforma):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

Negli esempi precedenti non passare `deployonbuild` a `dotnet publish`.

Per altre informazioni, vedere [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` supporta le API Kudu per la pubblicazione in Azure da qualsiasi piattaforma. La pubblicazione con Visual Studio supporta le API Kudu, ma è supportata da WebSDK per la pubblicazione multipiattaforma in Azure.

Aggiungere un profilo di pubblicazione alla cartella *Properties/PublishProfiles* con il contenuto seguente:

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

Eseguire il comando seguente per comprimere il contenuto per la pubblicazione e pubblicarlo in Azure tramite le API Kudu:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Quando si usa un profilo di pubblicazione, impostare le proprietà di MSBuild seguenti:

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

Durante la pubblicazione con un profilo denominato *FolderProfile*, è possibile eseguire uno dei comandi riportati di seguito:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Richiamando [dotnet build](/dotnet/core/tools/dotnet-build), viene chiamato `msbuild` per eseguire il processo di compilazione e di pubblicazione. Quando si passa un profilo di cartella, è equivalente chiamare `dotnet build` o `msbuild`. Chiamando MSBuild direttamente in Windows, viene usata la versione .NET Framework di MSBuild. MSDeploy è attualmente limitato ai computer Windows per la pubblicazione. Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild e MSBuild usa MSDeploy sui profili diversi dalle cartelle. Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild (mediante MSDeploy) e si ha come risultato un errore (anche nell'esecuzione su una piattaforma Windows). Per la pubblicazione con un profilo diverso dalla cartella, chiamare MSBuild direttamente.

Il profilo di pubblicazione cartella seguente è stato creato con Visual Studio ed esegue la pubblicazione in una condivisione di rete:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

Nell'esempio precedente `<LastUsedBuildConfiguration>` è impostato su `Release`. Nella pubblicazione da Visual Studio, il valore della proprietà di configurazione `<LastUsedBuildConfiguration>` viene impostato usando il valore, quando viene avviato il processo di pubblicazione. La proprietà di configurazione `<LastUsedBuildConfiguration>` è speciale e non deve essere sottoposta a override in un file di MSBuild importato. È possibile eseguire l'override di questa proprietà dalla riga di comando.

Mediante l'interfaccia della riga di comando di .NET Core:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

Mediante MSBuild:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Pubblicare in un endpoint di MSDeploy dalla riga di comando

L'esempio seguente usa un'app Web ASP.NET Core creata da Visual Studio e denominata *AzureWebApp*. Un profilo di pubblicazione app di Azure viene aggiunto con Visual Studio. Per altre informazioni su come creare un profilo, vedere la sezione [Profili di pubblicazione](#publish-profiles).

Per distribuire l'app usando un profilo di pubblicazione, eseguire il comando `msbuild` dal **prompt dei comandi per gli sviluppatori** di Visual Studio. Il prompt dei comandi è disponibile nella cartella *Visual Studio* del menu **Start** della barra delle applicazioni di Windows. Per semplificare l'accesso, è possibile aggiungere il prompt dei comandi al menu **Strumenti** di Visual Studio. Per altre informazioni, vedere [Prompt dei comandi per gli sviluppatori per Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).

MSBuild usa la sintassi dei comandi seguente:

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* {PERCORSO} &ndash; Percorso del file di progetto dell'app.
* {PROFILO} &ndash; Nome del profilo di pubblicazione.
* {NOMEUTENTE} &ndash; Nome utente MSDeploy. {NOMEUTENTE} è disponibile nel profilo di pubblicazione.
* {PASSWORD} &ndash; Password di MSDeploy. Il valore {PASSWORD} è disponibile nel file *{PROFILO}.PublishSettings*. Scaricare il file *.PublishSettings* da:
  * Esplora soluzioni: Selezionare **Visualizza** > **Cloud Explorer**. Connettersi alla sottoscrizione di Azure. Aprire **Servizi app**. Fare clic con il pulsante destro del mouse sull'app. Selezionare **Scarica profilo di pubblicazione**.
  * Portale di Azure: selezionare **Recupera profilo di pubblicazione** nel pannello **Panoramica** dell'app Web.

L'esempio seguente usa un profilo di pubblicazione denominato *AzureWebApp - Web Deploy*:

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

Un profilo di pubblicazione può essere usato anche con il comando [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) dell'interfaccia della riga di comando di .NET Core al prompt dei comandi di Windows:

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> Il comando [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) è multipiattaforma e consente di compilare app ASP.NET Core in macOS e Linux. Tuttavia, MSBuild in macOS e Linux non è in grado di distribuire un'app in Azure o in un altro endpoint MSDeploy. MSDeploy è disponibile solo in Windows.

## <a name="set-the-environment"></a>Impostare l'ambiente

Includere la proprietà `<EnvironmentName>` nel profilo di pubblicazione ( *.pubxml*) o nel file di progetto per impostare l'[ambiente](xref:fundamentals/environments) dell'app:

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

Se sono necessarie trasformazioni *web.config* (ad esempio, l'impostazione di variabili di ambiente in base a configurazione, profilo o ambiente), vedere <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="exclude-files"></a>Escludere file

Quando si pubblicano app Web ASP.NET Core vengono inclusi gli asset seguenti:

* Artefatti della compilazione
* Cartelle e file corrispondenti ai criteri GLOB seguenti:
  * `**\*.config` (ad esempio, *web.config*)
  * `**\*.json` (ad esempio, *appsettings.json*)
  * `wwwroot\**`

MSBuild supporta i [criteri GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns). Ad esempio, l'elemento `<Content>` seguente elimina la copia dei file di testo ( *.txt*) nella cartella *wwwroot\content* e nelle relative sottocartelle:

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Il markup precedente può essere aggiunto a un profilo di pubblicazione o al file *.csproj*. Quando viene aggiunta al file *.csproj*, la regola viene aggiunta a tutti i profili di pubblicazione del progetto.

L'elemento `<MsDeploySkipRules>` seguente esclude tutti i file dalla cartella *wwwroot\content*:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` non elimina le destinazioni da *ignorare* dal sito di distribuzione. Vengono eliminati dal sito di distribuzione i file e le cartelle di destinazione `<Content>`. Si supponga, ad esempio, che un'app Web distribuita contenga i file seguenti:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Se si aggiungono gli elementi `<MsDeploySkipRules>` seguenti, questi file non vengono eliminati nel sito di distribuzione.

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

Gli elementi `<MsDeploySkipRules>` precedenti impediscono che i file *ignorati* vengano distribuiti. Tali file non verranno eliminati una volta distribuiti.

L'elemento `<Content>` seguente elimina i file di destinazione sul sito di distribuzione:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

L'uso della distribuzione dalla riga di comando con l'elemento `<Content>` precedente produce il seguente output:

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a>File di inclusione

Il markup seguente:

* Include una cartella *images* all'esterno della directory di progetto nella cartella *wwwroot/images* del sito di pubblicazione.
* Può essere aggiunto al file con estensione *csproj* o al profilo di pubblicazione. Se viene aggiunto al file *.csproj*, è incluso in ogni profilo di pubblicazione nel progetto.

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Il markup evidenziato seguente illustra come:

* Copiare un file dall'esterno del progetto nella cartella *wwwroot*.
* Escludere la cartella *wwwroot\Content*.
* Escludere *Views\Home\About2.cshtml*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

Vedere il [file Leggimi del repository di Web SDK](https://github.com/aspnet/websdk) per altri esempi di distribuzione.

## <a name="run-a-target-before-or-after-publishing"></a>Eseguire una destinazione prima o dopo la pubblicazione

Le destinazioni incorporate `BeforePublish` e `AfterPublish` consentono di eseguire una destinazione prima o dopo la destinazione di pubblicazione. Aggiungere gli elementi seguenti al profilo di pubblicazione per registrare i messaggi della console prima e dopo la pubblicazione:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Pubblicazione in un server tramite un certificato non attendibile

Aggiungere la proprietà `<AllowUntrustedCertificate>` con un valore `True` al profilo di pubblicazione:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Servizio Kudu

Per visualizzare i file in una distribuzione di app Web di Servizio app di Azure, usare il [servizio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Accodare il token `scm` al nome dell'app Web. Ad esempio:

| URL                                    | Risultato       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | App Web      |
| `http://mysite.scm.azurewebsites.net/` | Servizio Kudu |

Selezionare la voce di menu [Console di debug](https://github.com/projectkudu/kudu/wiki/Kudu-console) per visualizzare, modificare, eliminare o aggiungere file.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Distribuzione Web](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) semplifica la distribuzione di app Web e siti Web sui server IIS.
* [Repository GitHub di Web SDK](https://github.com/aspnet/websdk/issues): problemi di file e funzionalità di richiesta per la distribuzione.
* [Pubblicare un'app Web ASP.NET in una macchina virtuale di Azure da Visual Studio](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
