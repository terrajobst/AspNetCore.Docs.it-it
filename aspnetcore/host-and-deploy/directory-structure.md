---
title: Struttura di directory di ASP.NET Core
author: guardrex
description: Informazioni sulla struttura di directory delle app ASP.NET Core pubblicate.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: host-and-deploy/directory-structure
ms.openlocfilehash: c3c05e6bc461ea4a3bfefa2c7a49d524562f7e5b
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172262"
---
# <a name="aspnet-core-directory-structure"></a>Struttura di directory di ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

La directory *publish* contiene gli asset distribuibili prodotti dal comando [dotnet publish](/dotnet/core/tools/dotnet-publish). La directory contiene:

* File dell'applicazione
* File di configurazione
* Asset statici
* Pacchetti
* Un runtime (solo [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd))

| Tipo di app | Struttura di directory |
| -------- | ------------------- |
| [Eseguibile dipendente dal Framework (supportano)](/dotnet/core/deploying/#framework-dependent-executables-fde) | <ul><li>publish&dagger;<ul><li>Viste&dagger; app MVC; Se le visualizzazioni non sono precompilate</li><li>Pagine&dagger; MVC o Razor Pages app, se le pagine non sono precompilate</li><li>wwwroot&dagger;</li><li>*i file con estensione dll</li><li>{nome assembly}. Deps. json</li><li>{nome assembly}. dll</li><li>{nome assembly} {. Estensione} estensione *exe* in Windows, nessuna estensione in MacOS o Linux</li><li>{nome assembly}. pdb</li><li>{nome assembly}. Views. dll</li><li>{nome ASSEMBLY}. Views. pdb</li><li>{nome ASSEMBLY}. runtimeconfig. JSON</li><li>Web. config (distribuzioni IIS)</li><li>createdump ([Linux createdump Utility](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy))</li>* <li>. so (libreria di oggetti condivisi Linux)</li><li>*. a (archivio MacOS)</li><li>* . dylib (libreria dinamica MacOS)</li></ul></li></ul> |
| [Distribuzione autonoma (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>Viste&dagger; app MVC, se le visualizzazioni non sono precompilate</li><li>Pagine&dagger; MVC o Razor Pages app, se le pagine non sono precompilate</li><li>wwwroot&dagger;</li><li>file *. dll</li><li>{NOME ASSEMBLY}.deps.json</li><li>{NOME ASSEMBLY}.dll</li><li>{NOME ASSEMBLY}.exe</li><li>{NOME ASSEMBLY}.pdb</li><li>{NOME ASSEMBLY}.Views.dll</li><li>{NOME ASSEMBLY}.Views.pdb</li><li>{NOME ASSEMBLY}.runtimeconfig.json</li><li>web.config (distribuzioni IIS)</li></ul></li></ul> |

&dagger;Indica una directory

La directory *publish* rappresenta il *percorso radice del contenuto*, anche denominato *percorso di base dell'applicazione*, della distribuzione. Qualsiasi nome venga assegnato alla directory *publish* dell'applicazione distribuita sul server, il relativo percorso viene usato come percorso fisico del server per l'app ospitata.

La directory *wwwroot*, se presente, contiene solo gli asset statici.

## <a name="additional-resources"></a>Risorse aggiuntive

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [Distribuzione di applicazioni .NET Core](/dotnet/core/deploying/)
* [Framework di destinazione](/dotnet/standard/frameworks)
* [Catalogo RID di .NET Core](/dotnet/core/rid-catalog)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

La directory *publish* contiene gli asset distribuibili prodotti dal comando [dotnet publish](/dotnet/core/tools/dotnet-publish). La directory contiene:

* File dell'applicazione
* File di configurazione
* Asset statici
* Pacchetti
* Un runtime (solo [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd))

| Tipo di app | Struttura di directory |
| -------- | ------------------- |
| [Eseguibile dipendente dal Framework (supportano)](/dotnet/core/deploying/#framework-dependent-executables-fde) | <ul><li>publish&dagger;<ul><li>Viste&dagger; app MVC; Se le visualizzazioni non sono precompilate</li><li>Pagine&dagger; MVC o Razor Pages app, se le pagine non sono precompilate</li><li>wwwroot&dagger;</li><li>*i file con estensione dll</li><li>{nome assembly}. Deps. json</li><li>{nome assembly}. dll</li><li>{nome assembly} {. Estensione} estensione *exe* in Windows, nessuna estensione in MacOS o Linux</li><li>{nome assembly}. pdb</li><li>{nome assembly}. Views. dll</li><li>{nome ASSEMBLY}. Views. pdb</li><li>{nome ASSEMBLY}. runtimeconfig. JSON</li><li>Web. config (distribuzioni IIS)</li><li>createdump ([Linux createdump Utility](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy))</li>* <li>. so (libreria di oggetti condivisi Linux)</li><li>*. a (archivio MacOS)</li><li>* . dylib (libreria dinamica MacOS)</li></ul></li></ul> |
| [Distribuzione autonoma (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>Viste&dagger; app MVC, se le visualizzazioni non sono precompilate</li><li>Pagine&dagger; MVC o Razor Pages app, se le pagine non sono precompilate</li><li>wwwroot&dagger;</li><li>file *. dll</li><li>{NOME ASSEMBLY}.deps.json</li><li>{NOME ASSEMBLY}.dll</li><li>{NOME ASSEMBLY}.exe</li><li>{NOME ASSEMBLY}.pdb</li><li>{NOME ASSEMBLY}.Views.dll</li><li>{NOME ASSEMBLY}.Views.pdb</li><li>{NOME ASSEMBLY}.runtimeconfig.json</li><li>web.config (distribuzioni IIS)</li></ul></li></ul> |

&dagger;Indica una directory

La directory *publish* rappresenta il *percorso radice del contenuto*, anche denominato *percorso di base dell'applicazione*, della distribuzione. Qualsiasi nome venga assegnato alla directory *publish* dell'applicazione distribuita sul server, il relativo percorso viene usato come percorso fisico del server per l'app ospitata.

La directory *wwwroot*, se presente, contiene solo gli asset statici.

La creazione di una cartella *Logs* è utile per la [registrazione di debug avanzata del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs). Le cartelle nel percorso specificato per il valore `<handlerSetting>` non vengono create automaticamente dal modulo e devono essere già presenti nella distribuzione per consentire al modulo di scrivere il log di debug.

È possibile creare una directory *Logs* per la distribuzione usando uno dei due approcci seguenti:

* Aggiungere l'elemento `<Target>` seguente al file di progetto:

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   L'elemento `<MakeDir>` crea una cartella *Logs* vuota nell'output pubblicato. L'elemento usa la proprietà `PublishDir` per determinare il percorso di destinazione per la creazione della cartella. Diversi metodi di distribuzione, ad esempio Distribuzione Web, ignorano le cartelle vuote durante la distribuzione. L'elemento `<WriteLinesToFile>` genera un file nella cartella *Logs*, che garantisce la distribuzione della cartella nel server. La creazione della cartella con questo metodo ha esito negativo se il processo di lavoro non dispone dell'accesso in scrittura alla cartella di destinazione.

* Creare fisicamente la directory *Logs* sul server nella distribuzione.

La directory di distribuzione richiede autorizzazioni di lettura/esecuzione. La directory *Logs* richiede autorizzazioni di lettura/scrittura. Le directory aggiuntive in cui vengono scritti i file richiedono autorizzazioni di lettura/scrittura.

## <a name="additional-resources"></a>Risorse aggiuntive

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [Distribuzione di applicazioni .NET Core](/dotnet/core/deploying/)
* [Framework di destinazione](/dotnet/standard/frameworks)
* [Catalogo RID di .NET Core](/dotnet/core/rid-catalog)

::: moniker-end
