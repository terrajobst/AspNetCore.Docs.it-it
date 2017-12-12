---
title: Guida introduttiva con le API di protezione dati
author: rick-anderson
description: Questo documento viene illustrato come utilizzare le API di protezione dati ASP.NET Core per la protezione e la rimozione della protezione dati in un'applicazione.
keywords: ASP.NET Core, protezione dei dati
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 535bfaf2077cda91c27e7d0d68b9804e8596070e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a>Guida introduttiva con le API di protezione dati

<a name="security-data-protection-getting-started"></a>

Nei dati più semplici, protezione prevede i passaggi seguenti:

1. Creare un dati protezione da un provider di protezione dati.

2. Chiamare il `Protect` (metodo) con i dati da proteggere.

3. Chiamare il `Unprotect` (metodo) con i dati a cui si desidera riconvertire in testo normale.

La maggior parte delle strutture e modelli di app, ad esempio ASP.NET o SignalR, configurare il sistema di protezione dati già e aggiungerlo a un contenitore del servizio che accedere tramite l'inserimento di dipendenze. L'esempio seguente illustra la configurazione di un contenitore del servizio per l'inserimento di dipendenze e lo stack di protezione dati di registrazione, il provider di protezione dati tramite DI ricezione, creazione di una protezione e la protezione, quindi la rimozione della protezione dati

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Quando si crea un programma di protezione è necessario fornire uno o più [scopo stringhe](consumer-apis/purpose-strings.md). Una stringa scopo garantisce l'isolamento tra i consumer. Ad esempio, non sarebbe in grado di rimuovere la protezione dei dati forniti da una protezione con lo scopo di "viola" un programma di protezione creato con una stringa a scopo di "green".

>[!TIP]
> Le istanze di `IDataProtectionProvider` e `IDataProtector` sono thread-safe per più i chiamanti. È previsto che una volta che un componente ottiene un riferimento a un `IDataProtector` tramite una chiamata a `CreateProtector`, tale riferimento verrà utilizzato per più chiamate a `Protect` e `Unprotect`.
>
>Una chiamata a `Unprotect` genererà CryptographicException se il payload protetto può essere verificato o decifrato. Alcuni componenti preferibile ignorare gli errori durante rimuovere la protezione di operazioni. un componente che legge i cookie di autenticazione potrebbe gestire questo errore e gestire la richiesta, come se non contenesse alcun cookie affatto anziché rifiuteranno la richiesta di definitiva. Componenti che questo comportamento devono in particolare intercettare CryptographicException anziché integrare tutte le eccezioni.
