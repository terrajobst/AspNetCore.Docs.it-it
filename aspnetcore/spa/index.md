---
title: Usare i modelli di applicazione a pagina singola con ASP.NET Core
author: SteveSandersonMS
description: Informazioni su come installare e iniziare a usare i modelli di progetto per applicazioni a pagina singola di ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: eda4817de007f3c3184b2ba6ed6c97989ff17da5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Usare i modelli di applicazione a pagina singola con ASP.NET Core

> [!NOTE]
> La versione rilasciata di .NET Core 2.0.x SDK include i modelli di progetto precedenti per Angular, React e React con Redux. Questa documentazione non riguarda i modelli di progetto precedenti. Questa documentazione si riferisce ai modelli Angular, React e React con Redux più recenti che possono essere installati manualmente in ASP.NET Core 2.0. I modelli sono inclusi per impostazione predefinita in ASP.NET Core 2.1.

## <a name="prerequisites"></a>Prerequisiti

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org), versione 6 o successive

## <a name="installation"></a>Installazione

Se è stato installato ASP.NET Core 2.0, eseguire il comando seguente per installare i modelli di ASP.NET Core aggiornati per Angular, React e React con Redux:

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a>Usare i modelli

- [Usare il modello di progetto per Angular](xref:spa/angular)
- [Usare il modello di progetto per React](xref:spa/react)
- [Usare il modello di progetto per React con Redux](xref:spa/react-with-redux)
