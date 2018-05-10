---
title: Struttura di directory ASP.NET Core
author: guardrex
description: Informazioni sulla struttura di directory delle app ASP.NET Core pubblicate.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: a5cc1f23d624643facddc9e2006fb246e5ae66dc
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/07/2018
---
# <a name="aspnet-core-directory-structure"></a>Struttura di directory ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

In ASP.NET Core, la directory dell'applicazione pubblicata *pubblicare*, è costituito da file dell'applicazione, file di configurazione, asset statico, pacchetti e la fase di esecuzione (per [distribuzioni indipendente](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Tipo di App | Struttura di directory |
| -------- | ------------------- |
| [Distribuzione dipendenti dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>Pubblicazione&dagger;<ul><li>registri&dagger; (facoltativo, a meno che deve ricevere i log di stdout)</li><li>Viste&dagger; (app MVC; se non sono precompilate viste)</li><li>Pagine&dagger; (MVC o pagine Razor App; se le pagine non vengono precompilate)</li><li>Wwwroot&dagger;</li><li>*\.file DLL</li><li>\<nome assembly >. deps.json</li><li>\<nome assembly >. dll</li><li>\<assembly-name > con estensione pdb</li><li>\<nome assembly >. PrecompiledViews.dll</li><li>\<nome assembly >. PrecompiledViews.pdb</li><li>\<nome assembly >. runtimeconfig.json</li><li>Web. config (distribuzioni IIS)</li></ul></li></ul> |
| [Distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Pubblicazione&dagger;<ul><li>registri&dagger; (facoltativo, a meno che deve ricevere i log di stdout)</li><li>Refs&dagger;</li><li>Viste&dagger; (app MVC; se non sono precompilate viste)</li><li>Pagine&dagger; (MVC o pagine Razor App; se le pagine non vengono precompilate)</li><li>Wwwroot&dagger;</li><li>\*file DLL</li><li>\<nome assembly >. deps.json</li><li>\<nome assembly > .exe</li><li>\<assembly-name > con estensione pdb</li><li>\<nome assembly >. PrecompiledViews.dll</li><li>\<nome assembly >. PrecompiledViews.pdb</li><li>\<nome assembly >. runtimeconfig.json</li><li>Web. config (distribuzioni IIS)</li></ul></li></ul> |

&dagger;Indica una directory

Il *pubblicare* directory rappresenta il *percorso radice del contenuto*, definita anche la *percorso base dell'applicazione*, della distribuzione. Qualsiasi nome viene fornito per il *pubblicare* directory dell'applicazione distribuita nel server, il relativo percorso viene utilizzato come percorso fisico del server per l'applicazione ospitata.

Il *wwwroot* directory, se presente, contiene solo gli asset statici.

Stdout *registri* directory può essere creata per la distribuzione utilizzando uno dei due approcci seguenti:

* Aggiungere il seguente `<Target>` elemento al file di progetto:

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

   Il `<MakeDir>` elemento crea un oggetto vuoto *registri* cartella nell'output pubblicato. L'elemento Usa il `PublishDir` proprietà per determinare il percorso di destinazione per la creazione della cartella. Diversi metodi di distribuzione, ad esempio di distribuzione Web, ignorano le cartelle vuote durante la distribuzione. Il `<WriteLinesToFile>` elemento genera un file nel *registri* cartella, che garantisce la distribuzione della cartella in cui il server. Si noti che la creazione di cartelle può avere comunque esito negativo se il processo di lavoro non dispone dell'accesso in scrittura alla cartella di destinazione.

* Creare fisicamente il *registri* directory nel server nella distribuzione.

La directory di distribuzione richiede le autorizzazioni di lettura/esecuzione. Il *registri* directory è necessarie autorizzazioni di lettura/scrittura. Directory aggiuntive in cui vengono scritti i file richiedono autorizzazioni di lettura/scrittura.
