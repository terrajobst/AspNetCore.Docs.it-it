---
title: Guida introduttiva con le API di protezione dati
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 9489b55b1de626b77bcbe21cb9678e561b559099
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a>Guida introduttiva con le API di protezione dati

<a name="security-data-protection-getting-started"></a>

Nei dati di protezione più semplici prevede i passaggi seguenti:

1. Creare un dati protezione da un provider di protezione dati.

2. Chiamare il metodo di protezione con i dati da proteggere.

3. Chiamare il metodo Unprotect con i dati che si desidera riconvertire in testo normale.

La maggior parte dei Framework, ad esempio ASP.NET o SignalR già configurare il sistema di protezione dati e aggiungerlo a un contenitore del servizio che accedere tramite l'inserimento di dipendenze. L'esempio seguente illustra la configurazione di un contenitore del servizio per l'inserimento di dipendenze e lo stack di protezione dati di registrazione, il provider di protezione dati tramite DI ricezione, creazione di una protezione e la protezione, quindi la rimozione della protezione dati

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Quando si crea un programma di protezione è necessario fornire uno o più [scopo stringhe](consumer-apis/purpose-strings.md). Una stringa scopo fornisce isolamento tra il consumer, ad esempio un programma di protezione creato con una stringa a scopo di "green" non sarebbe in grado di rimuovere la protezione dei dati forniti da una protezione con lo scopo di "viola".

>[!TIP]
> Le istanze di IDataProtectionProvider e oggetto IDataProtector sono thread-safe per più i chiamanti. Lo scopo è che quando un componente ha ottenuto un riferimento a un oggetto IDataProtector tramite una chiamata a CreateProtector, utilizzerà il riferimento per più chiamate a Protect e Unprotect.
>
>Se il payload protetto può essere verificato o decifrato, una chiamata a Unprotect genererà CryptographicException. Alcuni componenti preferibile ignorare gli errori durante rimuovere la protezione di operazioni. un componente che legge i cookie di autenticazione potrebbe gestire questo errore e gestire la richiesta, come se non contenesse alcun cookie affatto anziché rifiuteranno la richiesta di definitiva. Componenti che questo comportamento devono in particolare intercettare CryptographicException anziché integrare tutte le eccezioni.
