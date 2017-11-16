---
title: Creare profili di pubblicazione per Visual Studio e MSBuild
author: rick-anderson
description: Illustra la pubblicazione sul Web in Visual Studio.
keywords: ASP.NET Core, pubblicazione sul Web, pubblicazione, msbuild, distribuzione Web, pubblicazione dotnet, Visual Studio 2017
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a>Creare profili di pubblicazione per Visual Studio e MSBuild, per distribuire applicazioni ASP.NET Core

Di [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo articolo tratta dell'uso di Visual Studio 2017 per creare profili di pubblicazione. I profili di pubblicazione creati con Visual Studio possono essere eseguiti da MSBuild e Visual Studio 2017. L'articolo illustra i dettagli del processo di pubblicazione. Per istruzioni sulla pubblicazione in Azure, vedere [Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).

Il file *.csproj* seguente è stato creato con il comando `dotnet new mvc`:

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
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

L'attributo `Sdk` nell'elemento `<Project>` (nella prima riga) del markup riportato sopra esegue le operazioni seguenti:

* Importa il file `props` da *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* all'inizio.
* Importa il file di destinazioni da  *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* alla fine.

Il percorso predefinito per `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) è la cartella *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

`Microsoft.NET.Sdk.Web` dipende da:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Che determina l'importazione dei `props` e `targets` seguenti:

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

Le destinazioni di pubblicazione importeranno nuovamente il set corretto di destinazioni in base al metodo di pubblicazione usato.

Quando MSBuild o Visual Studio carica un progetto, vengono eseguite le seguenti azioni rilevanti:

* Compilare un progetto
* Calcolare i file da pubblicare
* Pubblicare i file nella destinazione

### <a name="compute-project-items"></a>Calcolare gli elementi del progetto

Quando viene caricato il progetto, vengono calcolati gli elementi del progetto (file). L'attributo `item type` determina la modalità di elaborazione del file. Per impostazione predefinita, i file *cs* sono inclusi nell'elenco di elementi `Compile`. I file presenti nell'elenco di elementi `Compile` vengono compilati.

L'elenco di elementi `Content` contiene i file che saranno pubblicati in aggiunta agli output di compilazione. Per impostazione predefinita, i file corrispondenti al criterio wwwroot/** saranno inclusi nell'elemento `Content`. [wwwroot/** è un criterio GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns) che specifica tutti i file nella cartella *wwwroot* **e** relative sottocartelle. Per aggiungere esplicitamente un file all'elenco di pubblicazione, aggiungere il file direttamente nel file *.csproj*, come illustrato in [Inclusione di file](#including-files).

Quando si seleziona il pulsante **Pubblica** in Visual Studio o quando si pubblica dalla riga di comando:

- Vengono calcolati le proprietà/gli elementi (i file necessari per compilare).
- Solo Visual Studio: i pacchetti NuGet vengono ripristinati.  (Il ripristino deve essere esplicito da parte dell'utente nell'interfaccia della riga di comando.)
- Il progetto viene compilato.
- Vengono calcolati gli elementi di pubblicazione (i file necessari per pubblicare).
- Il progetto viene pubblicato. (I file calcolati vengono copiati nella destinazione di pubblicazione.)

## <a name="basic-command-line-publishing"></a>Pubblicazione di base dalla riga di comando

Questa sezione funziona su tutte le piattaforme .NET Core supportate e non richiede Visual Studio. Negli esempi seguenti, il comando `dotnet publish` viene eseguito dalla directory del progetto (che contiene il file *.csproj*). Se non si è nella cartella del progetto, è possibile passare esplicitamente nel percorso del file del progetto. Ad esempio:

```console
dotnet publish  c:/webs/web1
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

--------------

`dotnet publish` produce un output simile al seguente:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

La cartella di pubblicazione predefinita è `bin\$(Configuration)\netcoreapp<version>\publish`. Il valore predefinito per `$(Configuration)` è Debug. Nel campione precedente, il `<TargetFramework>` è `netcoreapp2.0`.

`dotnet publish -h` visualizza informazioni della Guida per la pubblicazione.

Il comando seguente specifica un build `Release` e la directory di pubblicazione:

```console
dotnet publish -c Release -o C:/MyWebs/test
```

Il comando `dotnet publish` chiama `MSBuild` che richiama la destinazione `Publish`. Tutti i parametri passati a `dotnet publish` vengono passati a `MSBuild`. Il parametro `-c` esegue il mapping alla proprietà di MSBuild `Configuration`. Il parametro `-o` esegue il mapping a `OutputPath`.

È possibile passare le proprietà di MSBuild usando uno dei formati seguenti:

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

Il comando seguente pubblica un build `Release` a una condivisione di rete:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

La condivisione di rete è specificata con le barre (*//r8/*) e funziona su tutte le piattaforme .NET Core supportate.

Verificare che l'app pubblicata per la distribuzione non sia in esecuzione. I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app. La distribuzione non viene eseguita perché non è possibile copiare i file bloccati.

## <a name="publish-profiles"></a>Profili di pubblicazione

Questa sezione usa Visual Studio 2017 e versioni successive per creare profili di pubblicazione. Una volta creati, è possibile pubblicare da Visual Studio o dalla riga di comando.

I profili di pubblicazione possono semplificare il processo di pubblicazione. È possibile avere più profili di pubblicazione. Per creare un profilo di pubblicazione in Visual Studio, fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e selezionare **Pubblica**. In alternativa, è possibile selezionare **Pubblica... \<nome progetto >** dal menu di compilazione. Viene visualizzata la scheda **Pubblica** della pagina di capacità dell'applicazione. Se il progetto non contiene un profilo di pubblicazione, viene visualizzata la pagina seguente:

![Scheda **Pubblica** della pagina di capacità dell'applicazione che visualizza la cartella Azure, IIS, FTB con Azure selezionato. Inoltre, mostra i pulsanti di opzione Crea nuovo e Seleziona esistente](web-publishing-vs/_static/az.png)

Selezionando **Cartella**, il pulsante **Pubblica** crea un profilo di pubblicazione della cartella ed esegue la pubblicazione.

![Scheda **Pubblica** della pagina di capacità dell'applicazione che visualizza la cartella Azure, IIS, FTB](web-publishing-vs/_static/pub1.png)

Dopo aver creato un profilo di pubblicazione, la scheda **Pubblica** cambia ed è possibile selezionare **Crea nuovo profilo** per creare un nuovo profilo.

![Scheda **Pubblica** della pagina di capacità dell'applicazione che visualizza il FolderProfile creato nell'ultimo passaggio e il collegamento Crea nuovo profilo](web-publishing-vs/_static/create_new.png)

La pubblicazione guidata supporta le seguenti destinazioni di pubblicazione:

- Servizio app di Microsoft Azure
- IIS, FTP e così via (per qualunque server Web)
- Cartella
- Importa profilo (consente di importare un profilo).
- Macchine virtuali di Microsoft Azure

Per altre informazioni vedere [Quali sono le opzioni di pubblicazione più adatte?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)

Quando si crea un profilo di pubblicazione con Visual Studio, viene creato un file MSBuild *Properties/PublishProfiles/\<publish name>.pubxml*. Questo file *.pubxml* è un file MSBuild e contiene le impostazioni di configurazione della pubblicazione. È possibile modificare questo file per personalizzare il processo di compilazione e di pubblicazione. Questo file viene letto dal processo di pubblicazione. `<LastUsedBuildConfiguration>`è speciale perché è una proprietà globale e non deve essere presente in qualunque file importato nella compilazione. Per altre informazioni vedere [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: come impostare la proprietà di configurazione).
Il file *.pubxml* non deve essere selezionato nel controllo del codice sorgente perché dipende dal file *.user*. Il file *.user* non deve mai essere selezionato nel controllo del codice sorgente perché può contenere informazioni riservate ed è valido solo per un unico utente e computer.

Le informazioni riservate (come la password di pubblicazione) sono crittografate a livello di ogni utente/computer e archiviate nel file *Properties/PublishProfiles/\<publish name>.pubxml.user*. Poiché questo file può contenere informazioni riservate, **non** deve essere selezionato nel controllo del codice sorgente.

Per una panoramica su come pubblicare un'app Web in ASP.NET Core vedere [Publishing and Deployment](index.md) (Pubblicazione e distribuzione). [Publishing and Deployment](index.md).(Pubblicazione e distribuzione) è un progetto open source in https://github.com/aspnet/websdk.

 `dotnet publish` può usare profili di pubblicazione di tipo cartella, Msdeploy e [KUDU](https://github.com/projectkudu/kudu/wiki):
 
Cartella (multipiattaforma) `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

Msdeploy (attualmente funziona solo in Windows poiché Msdeploy non è multipiattaforma): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

Pacchetto Msdeploy (attualmente funziona solo in Windows poiché Msdeploy non è multipiattaforma): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

Negli esempi precedenti **non** passare `deployonbuild` a `dotnet publish`.

Per altre informazioni, vedere [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)

`dotnet publish` supporta le API KUDU per la pubblicazione in Azure da qualsiasi piattaforma. La pubblicazione con Visual Studio supporta le API KUDU ma è supportata da websdk per la pubblicazione multipiattaforma in Azure.

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

Il comando seguente comprimerà il contenuto per la pubblicazione e lo pubblicherà in Azure tramite le API KUDU.

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

Quando si usa un profilo di pubblicazione, impostare le proprietà di MSBuild seguenti:

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

Ad esempio, durante la pubblicazione con un profilo denominato *FolderProfile* è possibile eseguire uno dei comandi riportati di seguito.

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Richiamando `dotnet build` viene chiamato `msbuild` per eseguire il processo di compilazione e di pubblicazione. È sostanzialmente equivalente chiamare `dotnet build` o `msbuild` quando si passa in un profilo cartella. Chiamando MSBuild direttamente in Windows si ottiene la versione .NET Framework di MSBuild.  MSDeploy è attualmente limitato ai computer Windows per la pubblicazione. Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild e MSBuild usa MSDeploy sui profili diversi dalle cartelle. Chiamando `dotnet build` in un profilo diverso dalla cartella viene richiamato MSBuild (mediante MSDeploy) e si ha come risultato un errore (anche nell'esecuzione su una piattaforma Windows). Per la pubblicazione con un profilo diverso dalla cartella, chiamare MSBuild direttamente.

Il profilo di pubblicazione cartella seguente è stato creato con Visual Studio ed esegue la pubblicazione in una condivisione di rete:

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

Tenere presente che `<LastUsedBuildConfiguration>` è impostato su `Release`.  Nella pubblicazione da Visual Studio, il valore della proprietà di configurazione `<LastUsedBuildConfiguration>` viene impostato usando il valore, quando viene avviato il processo di pubblicazione. La proprietà di configurazione `<LastUsedBuildConfiguration>` è speciale e non deve essere sottoposta a override in un file di MSBuild importato. È possibile eseguire l'override di questa proprietà dalla riga di comando. Ad esempio:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Mediante MSBuild:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Pubblicare in un endpoint di MSDeploy dalla riga di comando

Come menzionato in precedenza, è possibile pubblicare usando `dotnet publish` o il comando `msbuild`. `dotnet publish` viene eseguito nel contesto di .NET Core. `msbuild` richiede .NET framework e pertanto è limitato agli ambienti Windows.

Il modo più semplice per pubblicare con MSDeploy è quello di creare prima un profilo di pubblicazione in Visual Studio 2017, quindi usare il profilo dalla riga di comando.

Nell'esempio seguente è stata creata un'app Web di ASP.NET Core usando `dotnet new mvc` ed è stato aggiunto un profilo di pubblicazione di Azure con Visual Studio.

Eseguire `msbuild` da un **Prompt dei comandi per gli sviluppatori per VS 2017**. Il prompt dei comandi per sviluppatori avrà il file *msbuild.exe* corrette nel proprio percorso e imposterà alcune variabili di MSBuild.

MSBuild usa la sintassi seguente:

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

È possibile ottenere la `Password` dal file *\<Publish name>.PublishSettings*. È possibile scaricare il file *.PublishSettings* da:

- Esplora soluzioni: fare clic con il pulsante destro del mouse sull'app Web e selezionare **Scarica profilo di pubblicazione**.
- Portale di gestione di Azure: selezionare **Recupera profilo di pubblicazione** nel pannello App Web.

Lo `Username` può essere trovato nel profilo di pubblicazione.

Nel campione seguente viene usato il profilo di pubblicazione "Web11112 - Distribuzione Web":

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>Esclusione di file

Quando si pubblicano le app Web di ASP.NET Core, vengono inclusi gli artefatti di compilazione e il contenuto della cartella *wwwroot*. `msbuild` supporta i [criteri GLOB](https://gruntjs.com/configuring-tasks#globbing-patterns). Ad esempio, il markup dell'elemento `<Content>` seguente escluderà tutti i file di testo (*.txt*) dalla cartella *wwwroot/content* e da tutte le relative sottocartelle.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Il markup riportato sopra può essere aggiunto a un profilo di pubblicazione o al file *.csproj*. Quando viene aggiunta al file *.csproj*, la regola viene aggiunta a tutti i profili di pubblicazione del progetto.

Il markup dell'elemento `<MsDeploySkipRules>` seguente esclude tutti i file dalla cartella *wwwroot/content*:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` non eliminerà le destinazioni da *ignorare* dal sito di distribuzione. Verranno eliminati dal sito di distribuzione i file e le cartelle di destinazione `<Content>`. Si supponga, ad esempio, di aver distribuito un'app Web con i file seguenti:

- *Views/Home/About1.cshtml*
- *Views/Home/About2.cshtml*
- *Views/Home/About3.cshtml*

Se è stato aggiunto il markup `<MsDeploySkipRules>` seguente, questi file non verranno eliminati nel sito di distribuzione.

``` xml
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

Il markup `<MsDeploySkipRules>` visualizzato sopra impedisce che i file *ignorati* vengano distribuiti, ma non elimina tali file una volta distribuiti.

Il markup `<Content>` seguente eliminerebbe i file di destinazione sul sito di distribuzione:

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

L'uso della distribuzione della riga di comando con il markup `<Content>` riportato sopra produrrebbe un risultato simile al seguente:

``` console
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

## <a name="including-files"></a>Inclusione di file

Il markup seguente può essere usato per includere una cartella *immagini* all'esterno della directory di progetto nella cartella *wwwroot/images* del sito di pubblicazione.

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Il markup può essere aggiunto al file *.csproj* o al profilo di pubblicazione. Se viene aggiunto al file *.csproj*, verrà incluso in ogni profilo di pubblicazione nel progetto.

Il markup evidenziato seguente illustra come:

* Copiare un file dall'esterno del progetto nella cartella *wwwroot*.
* Escludere la cartella *wwwroot\Content*.
* Escludere *Views\Home\About2.cshtml*.

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

Per altri campioni di distribuzione vedere [WebSDK Readme](https://github.com/aspnet/websdk).

### <a name="run-a-target-before-or-after-publishing"></a>Eseguire una destinazione prima o dopo la pubblicazione

Le destinazioni `BeforePublish` e `AfterPublish` incorporate possono essere usate per eseguire una destinazione prima o dopo la destinazione di pubblicazione. È possibile aggiungere il markup seguente al profilo di pubblicazione per registrare messaggi nell'output di console prima e dopo la pubblicazione:

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Servizio Kudu

Per visualizzare i file nella propria applicazione Web di Azure, usare il [servizio kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Accodare il token `scm` al nome della propria app Web. Ad esempio:

| URL               | Risultato|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | App Web |
| `http://mysite.scm.azurewebsites.net/` | Servizio Kudu |

Selezionare la voce di menu [Console di debug](https://github.com/projectkudu/kudu/wiki/Kudu-console) per visualizzare/modificare/eliminare/aggiungere file.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Distribuzione Web](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) semplifica la distribuzione di applicazioni Web e siti Web su server IIS.

- [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemi di file e funzionalità di richiesta per la distribuzione.
