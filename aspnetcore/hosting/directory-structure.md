---
title: Struttura di directory ASP.NET Core
author: guardrex
description: La struttura di directory delle applicazioni ASP.NET di base pubblicate.
keywords: ASP.NET di base, struttura di directory
ms.author: riande
manager: wpickett
ms.date: 03/15/2017
ms.topic: article
ms.assetid: e55eb131-d42e-4bf6-b130-fd626082243c
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/directory-structure
ms.openlocfilehash: 60797bff85a44dd10caad4aabc109ee12dedfe61
ms.sourcegitcommit: 7efdc4b6025ad70c15c26bf7451c3c0411123794
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Struttura di directory di App ASP.NET Core pubblicate

Di [Luke Latham](https://github.com/guardrex)

Nella directory dell'applicazione ASP.NET Core *pubblicare*, è costituito da file dell'applicazione, i file di configurazione, asset statico, pacchetti e il runtime (per applicazioni indipendenti). Questa è la stessa struttura di directory di versioni precedenti di ASP.NET, in cui risiede l'intera applicazione all'interno della directory radice web.

| Tipo di App | Struttura di directory |
| --- | --- |
| Distribuzione dipendenti dal framework | <ul><li>pubblicazione\*<ul><li>registri\* (se incluso in publishOptions)</li><li>Refs\*</li><li>Runtime\*</li><li>Viste\* (se incluso in publishOptions)</li><li>wwwroot\* (se incluso in publishOptions)</li><li>.dll (file)</li><li>MyApp.deps.JSON</li><li>MyApp</li><li>MyApp</li><li>MyApp. PrecompiledViews.dll (se precompilazione visualizzazioni Razor)</li><li>MyApp. PrecompiledViews.pdb (se precompilazione visualizzazioni Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>Web. config (se incluso in publishOptions)</li></ul></li></ul> |
| Distribuzione autonoma | <ul><li>pubblicazione\*<ul><li>registri\* (se incluso in publishOptions)</li><li>Refs\*</li><li>Viste\* (se incluso in publishOptions)</li><li>wwwroot\* (se incluso in publishOptions)</li><li>.dll (file)</li><li>MyApp.deps.JSON</li><li>MyApp.exe</li><li>MyApp</li><li>MyApp. PrecompiledViews.dll (se precompilazione visualizzazioni Razor)</li><li>MyApp. PrecompiledViews.pdb (se precompilazione visualizzazioni Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>Web. config (se incluso in publishOptions)</li></ul></li></ul> |
\*Indica una directory

Il contenuto del *pubblicare* directory rappresenta il *percorso radice del contenuto*, denominati anche il *percorso base dell'applicazione*, della distribuzione. Verrà assegnato qualsiasi nome di *pubblicare* directory nella distribuzione, il percorso viene utilizzato come percorso fisico del server l'applicazione ospitata. Il *wwwroot* directory, se presente, contiene solo gli asset statici. Il *registri* directory può essere inclusi nella distribuzione creazione del progetto e aggiungendo il `<Target>` elemento mostrata di seguito per il *csproj* file oppure creando fisicamente la directory sul Server.

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

La directory di distribuzione è necessarie autorizzazioni di lettura/esecuzione, mentre il *registri* directory richiede autorizzazioni di lettura/scrittura. Directory aggiuntive in cui verranno scritto asset richiedono autorizzazioni di lettura/scrittura.
