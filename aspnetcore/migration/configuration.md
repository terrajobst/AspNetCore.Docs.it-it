---
title: Migrazione della configurazione
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: f258e12a95770909bff24fd5dd3611324179596f
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/11/2018
---
# <a name="migrating-configuration"></a>Migrazione della configurazione

[Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)

Nell'articolo precedente, abbiamo iniziato [la migrazione di un progetto MVC ASP.NET ad ASP.NET MVC Core](mvc.md). In questo articolo è la migrazione di configurazione.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Configurazione dell'installazione

ASP.NET Core non utilizzerà più il *Global. asax* e *Web. config* file utilizzato per le versioni precedenti di ASP.NET. Nelle versioni precedenti di ASP.NET, la logica di avvio dell'applicazione è stata inserita un `Application_StartUp` metodo all'interno di *Global. asax*. Più avanti, in ASP.NET MVC, un *Startup.cs* file è stato incluso nella radice del progetto; e, è stato chiamato quando l'applicazione avviata. ASP.NET Core ha adottato questo approccio completamente inserendo tutta la logica di avvio di *Startup.cs* file.

Il *Web. config* file è stato sostituito anche in ASP.NET Core. Configurazione stessa ora può essere configurati, come parte della procedura di avvio dell'applicazione descritto in *Startup.cs*. Configurazione può ancora utilizzare file XML, ma in genere i progetti ASP.NET Core verranno posizionati i valori di configurazione in un file in formato JSON, ad esempio *appSettings. JSON*. Sistema di configurazione di ASP.NET Core inoltre facilmente accessibili le variabili di ambiente, che possono fornire un [più sicura e affidabile la posizione di](xref:security/app-secrets) per valori specifici dell'ambiente. Ciò vale soprattutto per i segreti come stringhe di connessione e le chiavi API che non devono essere verificate nel controllo del codice sorgente. Vedere [configurazione](xref:fundamentals/configuration/index) per ulteriori informazioni sulla configurazione di ASP.NET Core.

Per questo articolo, si inizierà con il progetto ASP.NET Core parzialmente migrati da [l'articolo precedente](mvc.md). Configurazione del programma di installazione, aggiungere il seguente costruttore e proprietà per il *Startup.cs* file si trova nella radice del progetto:

[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

Si noti che a questo punto, il *Startup.cs* file non verrà compilato, come è necessario aggiungere il seguente `using` istruzione:

```csharp
using Microsoft.Extensions.Configuration;
```

Aggiungere un *appSettings. JSON* file nella radice del progetto utilizzando il modello di elemento appropriato:

![Aggiungere AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Migrazione delle impostazioni di configurazione dal Web. config

Il progetto ASP.NET MVC incluso la stringa di connessione di database richiesti in *Web. config*, nel `<connectionStrings>` elemento. Nel progetto ASP.NET di base, verrà conservata nel *appSettings. JSON* file. Aprire *appSettings. JSON*e si noti che include le operazioni seguenti:

[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


Nella riga evidenziata illustrata in precedenza, modificare il nome del database da **_CHANGE_ME** per il nome del database.

## <a name="summary"></a>Riepilogo

ASP.NET Core colloca tutti logica di avvio per l'applicazione in un singolo file, in cui i servizi necessari e le dipendenze possono essere definite e configurate. Sostituisce il *Web. config* file con una funzionalità di configurazione flessibile in grado di sfruttare un'ampia gamma di formati di file, ad esempio JSON, nonché le variabili di ambiente.
