---
title: Panoramica di API consumer per ASP.NET Core
author: rick-anderson
description: Ricevere una breve panoramica del consumer varie API disponibili all'interno della libreria di protezione dei dati di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: 5d161ed8fbc39bcf4a970644480b4e909810b555
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Panoramica di API consumer per ASP.NET Core

Il `IDataProtectionProvider` e `IDataProtector` interfacce sono le interfacce di base tramite cui i consumer utilizzano il sistema di protezione dati. E si trovano nel [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) pacchetto.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

L'interfaccia del provider rappresenta la radice del sistema di protezione dati. E non può essere utilizzato direttamente per proteggere o rimuovere la protezione dati. Al contrario, il consumer deve ottenere un riferimento a un `IDataProtector` chiamando `IDataProtectionProvider.CreateProtector(purpose)`, in cui scopo è una stringa che descrive il caso di utilizzo previsto di consumer. Vedere [scopo stringhe](xref:security/data-protection/consumer-apis/purpose-strings) per molte più informazioni allo scopo di questo parametro e come scegliere un valore appropriato.

## <a name="idataprotector"></a>IDataProtector

L'interfaccia di protezione viene restituito da una chiamata a `CreateProtector`e il consumer possono utilizzare per eseguire questa interfaccia proteggere e annullare la protezione di operazioni.

Per proteggere una parte dei dati, passare i dati per il `Protect` metodo. L'interfaccia di base definisce un metodo che converte di byte [] -> byte [], ma è anche un overload (fornito come un metodo di estensione) che converte una stringa -> string. La sicurezza offerta da due metodi è identica. lo sviluppatore deve scegliere a seconda del valore overload è più semplice per il caso di utilizzo. Indipendentemente l'overload scelto, il valore restituito da Protect metodo ora protetto (crittografato e prova di manomissioni) e l'applicazione può inviare a un client non attendibile.

Per rimuovere la protezione di un dato precedentemente protetto, passare i dati protetti per il `Unprotect` metodo. (Esistono byte []-overload in base e basato su stringa per praticità per sviluppatori.) Se il payload protetto è stato generato da una precedente chiamata a `Protect` questa stessa `IDataProtector`, `Unprotect` metodo restituirà il payload originale non protetto. Se il payload protetto è stato manomesso o è stato generato da un'altra `IDataProtector`, `Unprotect` metodo genererà CryptographicException.

Il concetto di uguale e diversi `IDataProtector` ties nuovamente al concetto di scopo. Se due `IDataProtector` istanze generate dalla stessa radice `IDataProtectionProvider` ma tramite stringhe scopo diverso nella chiamata a `IDataProtectionProvider.CreateProtector`, è considerato [protezioni diversi](xref:security/data-protection/consumer-apis/purpose-strings), e uno non sarà in grado di rimuovere la protezione payload generati dagli altri.

## <a name="consuming-these-interfaces"></a>Utilizzo di queste interfacce

Per un componente DI supporto, l'utilizzo previsto è che il componente un `IDataProtectionProvider` parametro nel costruttore e che il sistema DI fornisce automaticamente il servizio quando il componente viene creata un'istanza.

> [!NOTE]
> Alcune applicazioni (ad esempio applicazioni console o le applicazioni ASP.NET 4. x) potrebbero non essere DI supporto in modo non è possibile utilizzare il meccanismo descritto qui. Per questi scenari, consultare il [Non scenari di supporto DI](xref:security/data-protection/configuration/non-di-scenarios) per ulteriori informazioni su come ottenere un'istanza di un `IDataProtection` provider senza passare attraverso DI.

L'esempio seguente illustra i tre concetti:

1. [Aggiungere il sistema di protezione dati](xref:security/data-protection/configuration/overview) al contenitore del servizio,

2. Utilizzo DI per la ricezione di un'istanza di un `IDataProtectionProvider`, e

3. Creazione di un `IDataProtector` da un `IDataProtectionProvider` da utilizzare per proteggere e annullare la protezione dati.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Il pacchetto Microsoft.AspNetCore.DataProtection.Abstractions contiene un metodo di estensione `IServiceProvider.GetDataProtector` praticità per sviluppatori. Incapsula come una singola operazione di recupero di entrambi un `IDataProtectionProvider` dal provider di servizi e chiamare il metodo `IDataProtectionProvider.CreateProtector`. L'esempio seguente viene illustrato l'utilizzo.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Le istanze di `IDataProtectionProvider` e `IDataProtector` sono thread-safe per più i chiamanti. È destinata che una volta che un componente ottiene un riferimento a un `IDataProtector` tramite una chiamata a `CreateProtector`, tale riferimento verrà utilizzato per più chiamate a `Protect` e `Unprotect`. Una chiamata a `Unprotect` genererà CryptographicException se il payload protetto può essere verificato o decifrato. Alcuni componenti preferibile ignorare gli errori durante rimuovere la protezione di operazioni. un componente che legge i cookie di autenticazione potrebbe gestire questo errore e gestire la richiesta, come se non contenesse alcun cookie affatto anziché rifiuteranno la richiesta di definitiva. Componenti che questo comportamento devono in particolare intercettare CryptographicException anziché integrare tutte le eccezioni.
