---
title: Introduzione a Data Protection API in ASP.NET Core
author: rick-anderson
description: Informazioni su come usare le API di protezione dati ASP.NET Core per la protezione e rimozione della protezione dei dati in un'app.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836861"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>Introduzione a Data Protection API in ASP.NET Core

<a name="security-data-protection-getting-started"></a>

Nei più semplici, la protezione dei dati prevede i passaggi seguenti:

1. Creare un dati protezione da un provider di protezione dati.

2. Chiamare il `Protect` metodo con i dati da proteggere.

3. Chiamare il `Unprotect` metodo con i dati a cui si desidera riconvertire in testo normale.

La maggior parte dei Framework e modelli di app, ad esempio ASP.NET Core o SignalR, configurare il sistema di protezione dati già e aggiungerlo a un contenitore del servizio che accedere tramite inserimento delle dipendenze. L'esempio seguente viene illustrata la configurazione di un contenitore del servizio per l'inserimento delle dipendenze e la registrazione lo stack di protezione dati, riceve il provider di protezione dati tramite l'inserimento delle dipendenze, creazione di una protezione e rimozione della protezione e la protezione dei dati.

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Quando si crea un programma di protezione è necessario fornire uno o più [stringhe di scopi](xref:security/data-protection/consumer-apis/purpose-strings). Una stringa scopo fornisce isolamento tra i consumer. Ad esempio, una protezione creata con una stringa di utilizzo generico di "green" sarebbe in grado di rimuovere la protezione dei dati forniti da una protezione con uno scopo della "viola".

>[!TIP]
> Le istanze di `IDataProtectionProvider` e `IDataProtector` sono thread-safe per i chiamanti di più. È destinato che una volta che un componente ottiene un riferimento a un `IDataProtector` tramite una chiamata verso `CreateProtector`, che fanno riferimento a verranno utilizzate per più chiamate a `Protect` e `Unprotect`.
>
>Una chiamata a `Unprotect` genererà CryptographicException se il payload protetto non può essere verificato o decifrato. Alcuni componenti può essere utile ignorare gli errori durante rimuovere la protezione di operazioni; un componente che legge i cookie di autenticazione può gestire questo errore e trattare la richiesta come se non avesse alcun cookie affatto anziché Nega la richiesta direttamente. I componenti che questo comportamento devono in particolare intercettare CryptographicException invece di assorbimento di tutte le eccezioni.
