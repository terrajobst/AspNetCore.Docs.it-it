---
title: La migrazione della configurazione in ASP.NET Core
author: ardalis
description: Informazioni su come eseguire la migrazione di configurazione da un progetto ASP.NET MVC per un progetto ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 5a1c4d0cbbdf74a00073c654e78a05f44948caae
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205912"
---
# <a name="migrate-configuration-to-aspnet-core"></a>La migrazione della configurazione in ASP.NET Core

[Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)

Nell'articolo precedente, abbiamo iniziato [eseguire la migrazione di un progetto ASP.NET MVC ad ASP.NET Core MVC](xref:migration/mvc). In questo articolo, si esegue la migrazione configurazione.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Configurazione dell'installazione

ASP.NET Core non vengono più usati di *Global. asax* e *Web. config* file utilizzato le versioni precedenti di ASP.NET. Nelle versioni precedenti di ASP.NET, la logica di avvio dell'applicazione è stata inserita un' `Application_StartUp` metodo all'interno *Global. asax*. Più avanti, in ASP.NET MVC, una *Startup.cs* file è stato incluso nella radice del progetto; e, è stato chiamato quando l'applicazione è stata avviata. ASP.NET Core ha adottato questo approccio completamente inserendo tutta la logica di avvio nel *Startup.cs* file.

Il *Web. config* file è stato sostituito anche in ASP.NET Core. Configurazione stessa è ora possibile configurare, come parte della procedura di avvio dell'applicazione descritto in *Startup.cs*. Configurazione puoi comunque usare i file XML, ma in genere i progetti ASP.NET Core inseriranno i valori di configurazione in un file in formato JSON, ad esempio *appSettings. JSON*. Sistema di configurazione di ASP.NET Core anche facilmente accessibili le variabili di ambiente, che possono fornire una [posizione più protetta e solida](xref:security/app-secrets) per valori specifici dell'ambiente. Ciò vale soprattutto per i segreti, ad esempio le stringhe di connessione e le chiavi API che non devono essere verificate nel controllo del codice sorgente. Visualizzare [configurazione](xref:fundamentals/configuration/index) per altre informazioni sulla configurazione in ASP.NET Core.

In questo articolo si inizierà il progetto ASP.NET Core migrato parzialmente dalla [articolo precedente](xref:migration/mvc). Configurazione di installazione, aggiungere il seguente costruttore e proprietà per il *Startup.cs* file che si trova nella radice del progetto:

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

Si noti che a questo punto, il *Startup.cs* file non verrà compilato, poiché è necessario aggiungere il codice seguente `using` istruzione:

```csharp
using Microsoft.Extensions.Configuration;
```

Aggiungere un *appSettings. JSON* file nella radice del progetto usando il modello di elemento appropriato:

![Aggiungere AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Migrazione delle impostazioni di configurazione dal Web. config

Il progetto ASP.NET MVC incluso la stringa di connessione di database necessari nel *Web. config*, nella `<connectionStrings>` elemento. Nel progetto di ASP.NET Core, si intende archiviare queste informazioni nel *appSettings. JSON* file. Aprire *appSettings. JSON*e notare che già include quanto segue:

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

Nella riga evidenziata illustrata in precedenza, modificare il nome del database da **_CHANGE_ME** al nome del database.

## <a name="summary"></a>Riepilogo

ASP.NET Core colloca tutta la logica di avvio per l'applicazione in un singolo file, in cui i servizi necessari e le dipendenze possono essere definite e configurate. Sostituisce il *Web. config* file con una funzionalità di configurazione flessibili che può sfruttare un'ampia gamma di formati di file, ad esempio JSON, nonché le variabili di ambiente.
