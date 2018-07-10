---
title: Struttura di directory di ASP.NET Core
author: guardrex
description: Informazioni sulla struttura di directory delle app ASP.NET Core pubblicate.
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 8e2693397f826d0e9a36ff52aa1d1d623b31043d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960827"
---
# <a name="aspnet-core-directory-structure"></a>Struttura di directory di ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

In ASP.NET Core la directory dell'applicazione pubblicata, *publish*, è costituita da file dell'applicazione, file di configurazione, asset statici, pacchetti e il runtime (per le [distribuzioni autonome](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Tipo di app | Struttura di directory |
| -------- | ------------------- |
| [Distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>publish&dagger;<ul><li>Logs&dagger; (facoltativa, a meno che non sia necessaria per ricevere i log stdout)</li><li>Views&dagger; (app MVC; se non sono precompilate visualizzazioni)</li><li>Pages&dagger; (MVC o app Razor Pages; se non sono precompilate pagine)</li><li>wwwroot&dagger;</li><li>*\.dll files</li><li>\<nome-assembly>.deps.json</li><li>\<nome-assembly>.dll</li><li>\<nome-assembly>.pdb</li><li>\<nome-assembly>.PrecompiledViews.dll</li><li>\<nome-assembly>.PrecompiledViews.pdb</li><li>\<nome-assembly>.runtimeconfig.json</li><li>web.config (distribuzioni IIS)</li></ul></li></ul> |
| [Distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>Logs&dagger; (facoltativa, a meno che non sia necessaria per ricevere i log stdout)</li><li>refs&dagger;</li><li>Views&dagger; (app MVC; se non sono precompilate visualizzazioni)</li><li>Pages&dagger; (MVC o app Razor Pages; se non sono precompilate pagine)</li><li>wwwroot&dagger;</li><li>\*.dll files</li><li>\<nome-assembly>.deps.json</li><li>\<nome-assembly>.exe</li><li>\<nome-assembly>.pdb</li><li>\<nome-assembly>.PrecompiledViews.dll</li><li>\<nome-assembly>.PrecompiledViews.pdb</li><li>\<nome-assembly>.runtimeconfig.json</li><li>web.config (distribuzioni IIS)</li></ul></li></ul> |

&dagger;Indica una directory

La directory *publish* rappresenta il *percorso radice del contenuto*, anche denominato *percorso di base dell'applicazione*, della distribuzione. Qualsiasi nome venga assegnato alla directory *publish* dell'applicazione distribuita sul server, il relativo percorso viene usato come percorso fisico del server per l'app ospitata.

La directory *wwwroot*, se presente, contiene solo gli asset statici.

La directory *Logs* di stdout può essere creata per la distribuzione usando uno dei due approcci seguenti:

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

   L'elemento `<MakeDir>` crea una cartella *Logs* vuota nell'output pubblicato. L'elemento usa la proprietà `PublishDir` per determinare il percorso di destinazione per la creazione della cartella. Diversi metodi di distribuzione, ad esempio Distribuzione Web, ignorano le cartelle vuote durante la distribuzione. L'elemento `<WriteLinesToFile>` genera un file nella cartella *Logs*, che garantisce la distribuzione della cartella nel server. Si noti che la creazione della cartella può comunque avere esito negativo se il processo di lavoro non dispone dell'accesso in scrittura alla cartella di destinazione.

* Creare fisicamente la directory *Logs* sul server nella distribuzione.

La directory di distribuzione richiede autorizzazioni di lettura/esecuzione. La directory *Logs* richiede autorizzazioni di lettura/scrittura. Le directory aggiuntive in cui vengono scritti i file richiedono autorizzazioni di lettura/scrittura.
