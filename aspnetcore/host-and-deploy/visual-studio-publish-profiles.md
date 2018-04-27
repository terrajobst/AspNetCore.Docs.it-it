---
title: Visual Studio profili di pubblicazione per la distribuzione di app ASP.NET Core
author: rick-anderson
description: Informazioni su come creare profili di pubblicazione in Visual Studio e utilizzarli per la gestione delle distribuzioni di app ASP.NET Core per destinazioni diverse.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio profili di pubblicazione per la distribuzione di app ASP.NET Core

Di [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo documento si concentra sull'uso di Visual Studio 2017 per creare e usare profili di pubblicazione. I profili di pubblicazione creati con Visual Studio possono essere eseguiti da MSBuild e Visual Studio 2017. Per istruzioni sulla pubblicazione in Azure, vedere [Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).

Il seguente file di progetto è stato creato con il comando `dotnet new mvc`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

Il `<Project>` dell'elemento `Sdk` attributo eseguite le attività seguenti:

* Importa il file delle proprietà da *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* all'inizio.
* Importa il file di destinazioni da  *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* alla fine.

Il percorso predefinito per `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) è la cartella *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

Il `Microsoft.NET.Sdk.Web` dipende SDK:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Che genera le seguenti proprietà e le destinazioni da importare:

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

Pubblicare l'importazione delle destinazioni destra set di destinazioni in base al metodo di pubblicazione utilizzato.

Quando viene caricato un progetto MSBuild o Visual Studio, si verificano le azioni di alto livello seguenti:

* Compilare un progetto
* Calcolare i file da pubblicare
* Pubblicare i file nella destinazione

## <a name="compute-project-items"></a>Calcolare gli elementi del progetto

Quando viene caricato il progetto, vengono calcolati gli elementi del progetto (file). L'attributo `item type` determina la modalità di elaborazione del file. Per impostazione predefinita, i file *cs* sono inclusi nell'elenco di elementi `Compile`. I file presenti nell'elenco di elementi `Compile` vengono compilati.

Il `Content` elenco di elementi contiene i file che vengono pubblicati oltre gli output di compilazione. Per impostazione predefinita, i file corrispondenti al criterio `wwwroot/**` sono inclusi nel `Content` elemento. Il `wwwroot/\*\*` [motivo il glob](https://gruntjs.com/configuring-tasks#globbing-patterns) corrisponde a tutti i file nel *wwwroot* cartella **e** le sottocartelle. Per aggiungere in modo esplicito un file all'elenco di pubblicazione, aggiungere il file direttamente nella *csproj* del file come illustrato nel [i file di inclusione](#include-files).

Quando si seleziona il **pubblica** button in Visual Studio o quando si pubblicano dalla riga di comando:

* Vengono calcolati le proprietà/gli elementi (i file necessari per compilare).
* **Visual Studio solo**: vengono ripristinati i pacchetti NuGet. (Il ripristino deve essere esplicito da parte dell'utente nell'interfaccia della riga di comando.)
* Il progetto viene compilato.
* Vengono calcolati gli elementi di pubblicazione (i file necessari per pubblicare).
* Il progetto viene pubblicato (calcolate vengono copiati nella destinazione di pubblicazione).

Quando fa riferimento a un progetto ASP.NET Core `Microsoft.NET.Sdk.Web` nel file di progetto, un *app_offline.htm* file si trova nella radice della directory dell'applicazione web. Quando il file è presente, il modulo ASP.NET Core arresta normalmente l'app e rende disponibile il file *app_offline.htm* durante la distribuzione. Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Pubblicazione della riga di comando di base

La pubblicazione della riga di comando funziona su tutte le piattaforme supportate da .NET Core e non richiede Visual Studio. Negli esempi seguenti, il [dotnet pubblicare](/dotnet/core/tools/dotnet-publish) comando viene eseguito dalla directory del progetto (che contiene il *csproj* file). Se non è nella cartella del progetto, passare in modo esplicito il percorso del file di progetto. Ad esempio:

```console
dotnet publish C:\Webs\Web1
```

Eseguire i comandi seguenti per creare e pubblicare un'app Web:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

Il [dotnet pubblicare](/dotnet/core/tools/dotnet-publish) comando restituisce un output simile al seguente:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

La cartella di pubblicazione predefinita è `bin\$(Configuration)\netcoreapp<version>\publish`. Il valore predefinito per `$(Configuration)` viene *Debug*. Nell'esempio precedente, il `<TargetFramework>` è `netcoreapp2.0`.

`dotnet publish -h` visualizza informazioni della Guida per la pubblicazione.

Il comando seguente specifica un build `Release` e la directory di pubblicazione:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

Il [dotnet pubblicare](/dotnet/core/tools/dotnet-publish) comando MSBuild, che consente di visualizzare le chiamate di `Publish` destinazione. I parametri passati a `dotnet publish` vengono passati a MSBuild. Il parametro `-c` esegue il mapping alla proprietà di MSBuild `Configuration`. Il parametro `-o` esegue il mapping a `OutputPath`.

Proprietà di MSBuild può essere passata tramite uno dei formati seguenti:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Il comando seguente pubblica un build `Release` a una condivisione di rete:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

La condivisione di rete è specificata con le barre (*//r8/*) e funziona su tutte le piattaforme .NET Core supportate.

Verificare che l'app pubblicata per la distribuzione non sia in esecuzione. I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app. La distribuzione non viene eseguita perché non è possibile copiare i file bloccati.

## <a name="publish-profiles"></a>Profili di pubblicazione

In questa sezione Usa 2017 di Visual Studio per creare un profilo di pubblicazione. Una volta creata, la pubblicazione da Visual Studio o la riga di comando è disponibile.

Pubblicare i profili possono semplificare il processo di pubblicazione e può contenere qualsiasi numero di profili. Creare un profilo di pubblicazione in Visual Studio scegliendo uno dei seguenti percorsi:

* Fare clic sul progetto in Esplora soluzioni e selezionare **pubblica**.
* Selezionare **pubblica &lt;project_name&gt;**  dal **compilare** menu.

Il **pubblica** viene visualizzata la scheda della pagina capacità app. Se il progetto non dispone di un profilo di pubblicazione, viene visualizzata la pagina seguente:

![Scheda pubblica la pagina di capacità di app](visual-studio-publish-profiles/_static/az.png)

Quando si **cartella** è selezionata, specificare un percorso di cartella per archiviare le risorse pubblicate. La cartella predefinita è *bin\Release\PublishOutput*. Fare clic sui **Crea profilo** pulsante Fine.

Una volta creato un profilo di pubblicazione, il **pubblica** scheda modifiche. Il profilo appena creato viene visualizzato in un elenco a discesa. Fare clic su **Crea nuovo profilo** per creare un altro nuovo profilo.

![Scheda pubblica la pagina di capacità di app che mostra FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

La pubblicazione guidata supporta le seguenti destinazioni di pubblicazione:

* Servizio app di Azure
* Macchine virtuali di Azure
* IIS, FTP e così via (per qualsiasi server web)
* Cartella
* Importare il profilo

Per altre informazioni, vedere [quali opzioni di pubblicazione sono adatta alle mie esigenze](/visualstudio/ide/not-in-toc/web-publish-options).

Quando si crea un profilo di pubblicazione con Visual Studio, un *proprietà/PublishProfiles/&lt;profile_name&gt;pubxml* viene creato il file MSBuild. Il *pubxml* file è un file di MSBuild e contiene le impostazioni di configurazione di pubblicazione. Questo file può essere modificato per personalizzare la compilazione e processo di pubblicazione. Questo file viene letto dal processo di pubblicazione. `<LastUsedBuildConfiguration>` è uno speciale perché è una proprietà globale e non deve essere in qualsiasi file che viene importato nella compilazione. Vedere [MSBuild: come impostare la proprietà di configurazione](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) per altre informazioni.

Quando si pubblica in una destinazione di Azure, il *pubxml* file contiene l'identificatore della sottoscrizione Azure. Con tale tipo di destinazione, è sconsigliato l'aggiunta di questo file al controllo del codice sorgente. Quando si pubblica in una destinazione non Azure, è consigliabile archiviare il *pubxml* file.

Informazioni riservate (ad esempio la password di pubblicazione) vengono crittografate in un livello di utente/computer. In cui è memorizzato il *proprietà/PublishProfiles/&lt;profile_name&gt;. pubxml.user* file. Poiché questo file è possibile archiviare le informazioni riservate, non deve essere verificato nel controllo del codice sorgente.

Per una panoramica di come pubblicare un'app web in ASP.NET Core, vedere [Host e distribuire](xref:host-and-deploy/index). Le attività di MSBuild e destinazioni necessarie per pubblicare un'app di ASP.NET Core sono open source in https://github.com/aspnet/websdk.

`dotnet publish` può utilizzare cartella, MSDeploy, e [Kudu](https://github.com/projectkudu/kudu/wiki) profili di pubblicazione:

Cartella (e multipiattaforma):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (attualmente questo funziona solo in Windows poiché MSDeploy non multipiattaforma):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

Pacchetto di MSDeploy (attualmente questo funziona solo in Windows poiché MSDeploy non multipiattaforma):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

Negli esempi precedenti **non** passare `deployonbuild` a `dotnet publish`.

Per ulteriori informazioni, vedere [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` supporta le API Kudu per pubblicare in Azure da qualsiasi piattaforma. Visual Studio la pubblicazione supporta le API Kudu, ma è supportato dal WebSDK per multipiattaforma pubblicare in Azure.

Aggiungere un profilo di pubblicazione per il *proprietà/PublishProfiles* cartella con il seguente contenuto:

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

Eseguire il comando seguente per comprimere il contenuto di pubblicazione e pubblicarla in Azure utilizzando le API Kudu:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Quando si usa un profilo di pubblicazione, impostare le proprietà di MSBuild seguenti:

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

Durante la pubblicazione con un profilo denominato *FolderProfile*, è possibile eseguire uno dei comandi riportati di seguito:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Quando si richiama [compilazione dotnet](/dotnet/core/tools/dotnet-build), chiama `msbuild` per eseguire la compilazione e processo di pubblicazione. Una chiamata a `dotnet build` o `msbuild` equivale quando vengono passati in un profilo di cartella. Quando si chiama MSBuild direttamente in Windows, viene utilizzata la versione di .NET Framework di MSBuild. MSDeploy è attualmente limitato ai computer Windows per la pubblicazione. Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild e MSBuild usa MSDeploy sui profili diversi dalle cartelle. Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild (mediante MSDeploy) e si ha come risultato un errore (anche nell'esecuzione su una piattaforma Windows). Per la pubblicazione con un profilo diverso dalla cartella, chiamare MSBuild direttamente.

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

Tenere presente che `<LastUsedBuildConfiguration>` è impostato su `Release`. Nella pubblicazione da Visual Studio, il valore della proprietà di configurazione `<LastUsedBuildConfiguration>` viene impostato usando il valore, quando viene avviato il processo di pubblicazione. Il `<LastUsedBuildConfiguration>` proprietà di configurazione è speciale e non deve essere sottoposto a override in un file di MSBuild importato. Questa proprietà può essere sottoposto a override dalla riga di comando.

Utilizzo di .NET Core CLI:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

Mediante MSBuild:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Pubblicare in un endpoint di MSDeploy dalla riga di comando

La pubblicazione può essere eseguita utilizzando l'interfaccia CLI di .NET Core o MSBuild. `dotnet publish` viene eseguito nel contesto di .NET Core. Il `msbuild` comando richiede .NET Framework, che limita agli ambienti di Windows.

Il modo più semplice per pubblicare con MSDeploy è quello di creare prima un profilo di pubblicazione in Visual Studio 2017, quindi usare il profilo dalla riga di comando.

Nell'esempio seguente viene creata un'app web ASP.NET Core (utilizzando `dotnet new mvc`), e viene aggiunto un profilo di pubblicazione di Azure con Visual Studio.

Eseguire `msbuild` da un **prompt dei comandi per sviluppatori per Visual Studio 2017**. Prompt dei comandi per sviluppatori è corrette *msbuild.exe* nel percorso con un set di variabili MSBuild.

MSBuild usa la sintassi seguente:

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

Ottenere il `Password` dal  *\<nome pubblicazione >. PublishSettings* file. Scaricare il *. PublishSettings* file da uno:

* Esplora soluzioni: Nell'App Web e scegliere **Scarica profilo di pubblicazione**.
* Portale di Azure: fare clic su **profilo di pubblicazione Get** dell'App Web **Panoramica** pannello.

Lo `Username` può essere trovato nel profilo di pubblicazione.

L'esempio seguente usa il *Web11112 - distribuzione Web* profilo di pubblicazione:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>Escludere i file

Quando si pubblicano le app Web di ASP.NET Core, vengono inclusi gli artefatti di compilazione e il contenuto della cartella *wwwroot*. `msbuild` supporta i [criteri GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns). Ad esempio, il seguente `<Content>` elemento esclude tutto il testo (*. txt*) i file dal *wwwroot/content* cartella e tutte le relative sottocartelle.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Il markup precedente può essere aggiunti a un profilo di pubblicazione o il *csproj* file. Quando viene aggiunta al file *.csproj*, la regola viene aggiunta a tutti i profili di pubblicazione del progetto.

I seguenti `<MsDeploySkipRules>` elemento esclude tutti i file dal *wwwroot/content* cartella:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` non verrà eliminato il *ignorare* destinazioni dal sito di distribuzione. `<Content>` cartelle e file di destinazione vengono eliminate dal sito di distribuzione. Si supponga, ad esempio, che un'app web distribuite ha i seguenti file:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Se i seguenti `<MsDeploySkipRules>` gli elementi vengono aggiunti, non è possibile eliminare tali file nel sito di distribuzione.

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

Precedenti `<MsDeploySkipRules>` elementi impediscono la *ignorata* file venga distribuito. Tali file non verrà eliminato una volta vengano distribuite.

Nell'esempio `<Content>` elemento Elimina i file di destinazione nel sito di distribuzione:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Utilizzando la distribuzione della riga di comando con il precedente `<Content>` elemento produce il seguente output:

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

Il markup seguente include un *immagini* cartella all'esterno della directory di progetto per il *wwwroot/immagini* cartella del sito pubblica:

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Il markup può essere aggiunto al file *.csproj* o al profilo di pubblicazione. Se viene aggiunto il *csproj* file, è incluso in ogni profilo di pubblicazione nel progetto.

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

Per altri campioni di distribuzione vedere [WebSDK Readme](https://github.com/aspnet/websdk).

## <a name="run-a-target-before-or-after-publishing"></a>Eseguire una destinazione prima o dopo la pubblicazione

L'elemento predefinito `BeforePublish` e `AfterPublish` destinazioni eseguire una destinazione prima o dopo la destinazione di pubblicazione. Aggiungere i seguenti elementi per il profilo di pubblicazione per registrare i messaggi della console prima e dopo la pubblicazione:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>La pubblicazione in un server utilizzando un certificato non attendibile

Aggiungere il `<AllowUntrustedCertificate>` proprietà con un valore di `True` per il profilo di pubblicazione:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Servizio Kudu

Per visualizzare i file in una distribuzione di app web di servizio App di Azure, usare il [servizio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Aggiungere il `scm` token per il nome dell'app web. Ad esempio:

| URL                                    | Risultato       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | App Web      |
| `http://mysite.scm.azurewebsites.net/` | Servizio kudu |

Selezionare il [Console di Debug](https://github.com/projectkudu/kudu/wiki/Kudu-console) voce di menu per visualizzare, modificare, eliminare o aggiungere i file.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) semplifica la distribuzione di App web e siti Web ai server IIS.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): I problemi di file e richiedere le funzionalità per la distribuzione.
