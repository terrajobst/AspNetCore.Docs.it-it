---
title: Usare il modello di progetto per React con Redux con ASP.NET Core
author: SteveSandersonMS
description: Informazioni su come iniziare a usare il modello di progetto per applicazioni a pagina singola di ASP.NET Core per React con Redux e create-react-app.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: c1aedcf1e14a9e7b339b60dd02c4267cd5945a49
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341620"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>Usare il modello di progetto per React con Redux con ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Questa documentazione non sono relativi al modello di progetto React con Redux incluso in ASP.NET Core 2.0. Fa riferimento al modello di React con Redux più recente che è possibile aggiornare manualmente. Il modello è disponibile in ASP.NET Core 2.1 o versione successiva.

::: moniker-end

Il modello di progetto aggiornato per React con Redux fornisce un ottimo punto di partenza per le app ASP.NET Core che usano le convenzioni React, Redux e [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) per implementare un'interfaccia utente avanzata sul lato client.

Fatta eccezione per il comando di creazione del progetto, tutte le informazioni relative al modello per React con Redux sono uguali a quelle relative al modello per React. Per creare questo tipo di progetto, eseguire `dotnet new reactredux` anziché `dotnet new react`. Per altre informazioni sulle funzionalità comuni a entrambi i modelli basati su React, vedere la [documentazione del modello per React](xref:spa/react).

Per informazioni sulla configurazione di una sotto-applicazione React con Redux in IIS, vedere [per ReactRedux modello 2.1: Non è possibile utilizzare SPA in IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).
