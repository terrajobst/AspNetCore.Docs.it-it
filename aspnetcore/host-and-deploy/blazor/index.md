---
title: Ospitare e distribuire Blazor
author: guardrex
description: Informazioni su come ospitare e distribuire app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: a7739e2b240d7fd6c85e68105892c802228ebeb5
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614587"
---
# <a name="host-and-deploy-blazor"></a>Ospitare e distribuire Blazor

Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Pubblicare l'app

Le app vengono pubblicate per la distribuzione nella configurazione di rilascio con il comando [dotnet publish](/dotnet/core/tools/dotnet-publish). Un ambiente di sviluppo integrato (IDE, Integrated Development Environment) può gestire l'esecuzione del comando `dotnet publish` automaticamente usando le funzionalità di pubblicazione predefinite, quindi potrebbe non essere necessario eseguire manualmente il comando da un prompt dei comandi a seconda degli strumenti di sviluppo in uso.

```console
dotnet publish -c Release
```

`dotnet publish` attiva un [ripristino](/dotnet/core/tools/dotnet-restore) delle dipendenze del progetto e [compila](/dotnet/core/tools/dotnet-build) il progetto prima di creare gli asset per la distribuzione. Come parte del processo di compilazione, vengono rimossi gli assembly e i metodi non usati per ridurre le dimensioni del download e i tempi di caricamento dell'app. La distribuzione viene creata nella cartella */bin/Release/{FRAMEWORK DESTINAZIONE}/publish*.

Gli asset nella cartella *publish* vengono distribuiti nel server Web. La distribuzione potrebbe essere un processo manuale o automatizzato a seconda degli strumenti di sviluppo in uso.

## <a name="deployment"></a>Distribuzione

Per indicazioni sulla distribuzione, vedere gli argomenti seguenti:

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
