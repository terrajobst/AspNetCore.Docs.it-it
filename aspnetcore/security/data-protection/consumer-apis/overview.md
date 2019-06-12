---
title: Panoramica delle API consumer per ASP.NET Core
author: rick-anderson
description: Ricevere una breve panoramica del consumer di varie API disponibili all'interno della libreria di protezione dei dati di ASP.NET Core.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: ff9badb55813cae0aa72d3a95dc53792332f109b
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837371"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Panoramica delle API consumer per ASP.NET Core

Il `IDataProtectionProvider` e `IDataProtector` interfacce sono le interfacce di base tramite cui i consumer utilizzano il sistema di protezione dati. Che si trovano nel [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) pacchetto.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

L'interfaccia del provider rappresenta la radice del sistema di protezione dati. Non è direttamente utilizzabile per proteggere o rimuovere la protezione dei dati. Al contrario, il consumer deve ottenere un riferimento a un `IDataProtector` chiamando `IDataProtectionProvider.CreateProtector(purpose)`, in cui scopo è una stringa che descrive il caso d'uso previsto consumer. Visualizzare [stringhe di scopi](xref:security/data-protection/consumer-apis/purpose-strings) per molte più informazioni all'obiettivo di questo parametro e come scegliere un valore appropriato.

## <a name="idataprotector"></a>IDataProtector

L'interfaccia di protezione viene restituito da una chiamata a `CreateProtector`ed è questa interfaccia che gli utenti possono usare per eseguire proteggere e rimuovere la protezione di operazioni.

Per proteggere una porzione di dati, passare i dati per il `Protect` (metodo). L'interfaccia di base definisce un metodo che converte byte [] -> byte [], ma è presente anche un overload (fornito come metodo di estensione) che lo converte in stringa -> stringa. La sicurezza offerta da due metodi è identica. lo sviluppatore deve scegliere qualsiasi overload è più semplice per i casi d'uso. Indipendentemente dall'overload scelto, il valore restituito da Protect metodo è ora protetto (cifrati e prova di manomissione) e l'applicazione può inviare a un client non attendibile.

Per disattivare la protezione una porzione di dati protetti in precedenza, passare i dati protetti per il `Unprotect` (metodo). (Sono presenti byte []-overload in base e basate su stringhe per comodità dello sviluppatore.) Se il payload protetto è stato generato da una precedente chiamata a `Protect` questa stessa `IDataProtector`, il `Unprotect` metodo restituirà il payload non protetto originale. Se il payload protetto è stato alterato o è stato generato da un'altra `IDataProtector`, il `Unprotect` metodo genererà CryptographicException.

Il concetto di uguale e diversi `IDataProtector` ties indietro al concetto di utilizzo generico. Se due `IDataProtector` istanze sono state generate dalla stessa radice `IDataProtectionProvider` ma tramite stringhe di scopi diversi nella chiamata a `IDataProtectionProvider.CreateProtector`, quindi considerate [protezioni diversi](xref:security/data-protection/consumer-apis/purpose-strings), e uno non saranno in grado di rimuovere la protezione payload generati dagli altri.

## <a name="consuming-these-interfaces"></a>Utilizzo di queste interfacce

Per un componente compatibile con l'inserimento delle dipendenze, l'utilizzo previsto è che il componente accetta un `IDataProtectionProvider` parametro nel relativo costruttore e che il sistema di inserimento delle dipendenze offre questo servizio automaticamente quando viene creata un'istanza al componente.

> [!NOTE]
> Alcune applicazioni (ad esempio applicazioni console o applicazioni ASP.NET 4.x) potrebbero non essere l'inserimento delle dipendenze in grado di riconoscere in modo che non è possibile utilizzare il meccanismo descritto di seguito. Per questi scenari, consultare il [scenari Non compatibili con](xref:security/data-protection/configuration/non-di-scenarios) documento per altre informazioni su come ottenere un'istanza di un `IDataProtection` provider senza passare attraverso l'inserimento delle dipendenze.

L'esempio seguente illustra tre concetti:

1. [Aggiungere il sistema di protezione dati](xref:security/data-protection/configuration/overview) al contenitore del servizio,

2. Tramite l'inserimento delle dipendenze per la ricezione di un'istanza di un `IDataProtectionProvider`, e

3. Creazione di un' `IDataProtector` da un `IDataProtectionProvider` e usandolo per proteggere e rimuovere la protezione dei dati.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Il pacchetto Microsoft.AspNetCore.DataProtection.Abstractions contiene un metodo di estensione `IServiceProvider.GetDataProtector` comodità per gli sviluppatori. Incapsula un'unica operazione entrambi il recupero di un `IDataProtectionProvider` dal provider del servizio e chiamare il metodo `IDataProtectionProvider.CreateProtector`. L'esempio seguente viene illustrato l'utilizzo.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Le istanze di `IDataProtectionProvider` e `IDataProtector` sono thread-safe per i chiamanti di più. È destinato che una volta che un componente ottiene un riferimento a un `IDataProtector` tramite una chiamata verso `CreateProtector`, che fanno riferimento a verranno utilizzate per più chiamate a `Protect` e `Unprotect`. Una chiamata a `Unprotect` genererà CryptographicException se il payload protetto non può essere verificato o decifrato. Alcuni componenti può essere utile ignorare gli errori durante rimuovere la protezione di operazioni; un componente che legge i cookie di autenticazione può gestire questo errore e trattare la richiesta come se non avesse alcun cookie affatto anziché Nega la richiesta direttamente. I componenti che questo comportamento devono in particolare intercettare CryptographicException invece di assorbimento di tutte le eccezioni.
