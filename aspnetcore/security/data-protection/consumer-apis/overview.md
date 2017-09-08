---
title: Panoramica di API di consumer
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f69beb9d-a519-43a8-857c-f6b01886a903
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: d23a6ce50eef71f393124b9420f4ba473904d8b4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="consumer-apis-overview"></a>Panoramica di API di consumer

Le interfacce IDataProtectionProvider e oggetto IDataProtector sono le interfacce di base tramite cui i consumer utilizzano il sistema di protezione dati. Si trovano nel pacchetto Microsoft.AspNetCore.DataProtection.Abstractions.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

L'interfaccia del provider rappresenta la radice del sistema di protezione dati. E non può essere utilizzato direttamente per proteggere o rimuovere la protezione dati. Al contrario, il consumer deve ottenere un riferimento a un oggetto IDataProtector chiamando IDataProtectionProvider.CreateProtector(purpose), in cui scopo è una stringa che descrive il caso di utilizzo previsto di consumer. Vedere [scopo stringhe](purpose-strings.md) per molte più informazioni allo scopo di questo parametro e come scegliere un valore appropriato.

## <a name="idataprotector"></a>Oggetto IDataProtector

L'interfaccia di protezione viene restituito da una chiamata a CreateProtector ed è questa interfaccia che i consumer possono utilizzare per eseguire proteggere e annullare la protezione di operazioni.

Per proteggere una parte dei dati, passare i dati per il metodo di protezione. L'interfaccia di base definisce un metodo che converte di byte [] -> byte [], ma è disponibile anche un overload (fornito come un metodo di estensione) che converte una stringa -> string. La sicurezza offerta da due metodi è identica. lo sviluppatore deve scegliere a seconda del valore overload è più semplice per il caso di utilizzo. Indipendentemente l'overload scelto, il valore restituito da Protect metodo ora protetto (crittografato e prova di manomissioni) e l'applicazione può inviare a un client non attendibile.

Per rimuovere la protezione di un dato precedentemente protetto, passare i dati protetti per il metodo Unprotect. (Esistono byte []-overload in base e basato su stringa per praticità per sviluppatori.) Se il payload protetto è stato generato da una precedente chiamata a Protect su questo oggetto stesso IDataProtector, il metodo Unprotect restituirà il payload originale non protetto. Se il payload protetto è stato manomesso o è stato generato da un oggetto IDataProtector diversi, il metodo Unprotect genererà CryptographicException.

Il concetto di stesso oggetto IDataProtector diverso rispetto a un collegamento al concetto di scopo. Se due istanze di oggetto IDataProtector generate dalla stessa radice IDataProtectionProvider ma tramite stringhe scopo diverso nella chiamata a IDataProtectionProvider.CreateProtector, quindi vengono considerati [protezioni diversi](purpose-strings.md), e uno non sarà in grado di rimuovere la protezione di payload generati dagli altri.

## <a name="consuming-these-interfaces"></a>Utilizzo di queste interfacce

Per un componente DI supporto, l'utilizzo previsto è che il componente accetta un parametro di IDataProtectionProvider nel relativo costruttore e che il sistema DI fornisce automaticamente il servizio quando il componente viene creata un'istanza.

> [!NOTE]
> Alcune applicazioni (ad esempio applicazioni console o le applicazioni ASP.NET 4. x) potrebbero non essere DI supporto in modo non è possibile utilizzare il meccanismo descritto qui. Per questi scenari, consultare il [Non scenari di supporto DI](../configuration/non-di-scenarios.md) per ulteriori informazioni su come ottenere un'istanza di un provider di IDataProtection senza passare attraverso DI.

L'esempio seguente illustra i tre concetti:

1. [Aggiunta di sistema di protezione dati](../configuration/overview.md) al contenitore del servizio,

2. Utilizzo DI per la ricezione di un'istanza di un IDataProtectionProvider, e

3. Creazione di un oggetto IDataProtector da un IDataProtectionProvider e usarlo per proteggere e annullare la protezione dati.

[!code-csharp[Principale](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Il pacchetto Microsoft.AspNetCore.DataProtection.Abstractions contiene un metodo di estensione IServiceProvider.GetDataProtector praticità per sviluppatori. Incapsula come una singola operazione di recupero di un IDataProtectionProvider dal provider del servizio sia la chiamata IDataProtectionProvider.CreateProtector. L'esempio seguente viene illustrato l'utilizzo.

[!code-csharp[Principale](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Le istanze di IDataProtectionProvider e oggetto IDataProtector sono thread-safe per più i chiamanti. È progettato che una volta che un componente ha ottenuto un riferimento a un oggetto IDataProtector tramite una chiamata a CreateProtector, tale riferimento verrà utilizzato per più chiamate a proteggere e chiamata Unprotect.A a Unprotect genererà CryptographicException se non può essere il payload protetto Verificare o decifrati. Alcuni componenti preferibile ignorare gli errori durante rimuovere la protezione di operazioni. un componente che legge i cookie di autenticazione potrebbe gestire questo errore e gestire la richiesta, come se non contenesse alcun cookie affatto anziché rifiuteranno la richiesta di definitiva. Componenti che questo comportamento devono in particolare intercettare CryptographicException anziché integrare tutte le eccezioni.
