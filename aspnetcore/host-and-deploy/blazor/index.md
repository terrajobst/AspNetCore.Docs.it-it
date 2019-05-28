---
title: Ospitare e distribuire Blazor
author: guardrex
description: Informazioni su come ospitare e distribuire app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/23/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 5def0356d13975211dd234f6a6a9f5a993d003b7
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223173"
---
# <a name="host-and-deploy-blazor"></a>Ospitare e distribuire Blazor

Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Pubblicare l'app

Le app vengono pubblicate per la distribuzione nella configurazione per il rilascio.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Selezionare **Compila** > **Pubblica {APPLICAZIONE}** sulla barra di spostamento.
1. Selezionare la *destinazione di pubblicazione*. Per pubblicare in locale, selezionare **Cartella**.
1. Accettare il percorso predefinito nel campo **Scegliere una cartella** oppure specificarne uno diverso. Selezionare il pulsante **Pubblica**.


# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code/Interfaccia della riga di comando di .NET Core](#tab/visual-studio-code+netcore-cli)

Usare il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) per pubblicare l'app con una configurazione per il rilascio:

```console
dotnet publish -c Release
```

---

La pubblicazione dell'app attiva un [ripristino](/dotnet/core/tools/dotnet-restore) delle dipendenze del progetto e [compila](/dotnet/core/tools/dotnet-build) il progetto prima di creare gli asset per la distribuzione. Come parte del processo di compilazione, vengono rimossi gli assembly e i metodi non usati per ridurre le dimensioni del download e i tempi di caricamento dell'app.

Un'app lato client Blazor viene pubblicata nella cartella */bin/Release/{FRAMEWORK DI DESTINAZIONE}/dist*. Un'app lato server Blazor viene pubblicata nella cartella */bin/Release/{FRAMEWORK DI DESTINAZIONE}/publish*.

Gli asset nella cartella vengono distribuiti nel server Web. La distribuzione potrebbe essere un processo manuale o automatizzato a seconda degli strumenti di sviluppo in uso.

## <a name="deployment"></a>Distribuzione

Per indicazioni sulla distribuzione, vedere gli argomenti seguenti:

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a>Hosting serverless in Blazor con Archiviazione di Azure

Le app Blazor sul lato client possono essere rese disponibili da [Archiviazione di Azure](https://azure.microsoft.com/services/storage/) come contenuto statico direttamente da un contenitore di archiviazione.

Per altre informazioni, vedere [Ospitare e distribuire Blazor sul lato client (distribuzione autonoma): Archiviazione di Azure](xref:host-and-deploy/blazor/client-side#azure-storage).
