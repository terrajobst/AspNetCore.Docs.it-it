---
title: Panoramica delle API consumer per ASP.NET Core
author: rick-anderson
description: Viene visualizzata una breve panoramica delle diverse API per i consumer disponibili nell'ASP.NET Core libreria di protezione dei dati.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: ff9badb55813cae0aa72d3a95dc53792332f109b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666586"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Panoramica delle API consumer per ASP.NET Core

Le interfacce `IDataProtectionProvider` e `IDataProtector` sono le interfacce di base attraverso le quali i consumer utilizzano il sistema di protezione dei dati. Si trovano nel pacchetto [Microsoft. AspNetCore. dataprotection. abstracts](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) .

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

L'interfaccia del provider rappresenta la radice del sistema di protezione dei dati. Non può essere usato direttamente per proteggere o rimuovere la protezione dei dati. Al contrario, il consumer deve ottenere un riferimento a un `IDataProtector` chiamando `IDataProtectionProvider.CreateProtector(purpose)`, dove scopo è una stringa che descrive il caso d'uso previsto del consumer. Per altre informazioni sullo scopo di questo parametro e su come scegliere un valore appropriato, vedere [stringhe di scopo](xref:security/data-protection/consumer-apis/purpose-strings) .

## <a name="idataprotector"></a>IDataProtector

L'interfaccia di protezione viene restituita da una chiamata a `CreateProtector`ed è questa interfaccia che i consumer possono utilizzare per eseguire operazioni di protezione e di rimozione della protezione.

Per proteggere una porzione di dati, passare i dati al metodo `Protect`. L'interfaccia di base definisce un metodo che converte Byte []-> byte [], ma esiste anche un overload (fornito come metodo di estensione) che converte la stringa > stringa. La sicurezza offerta dai due metodi è identica; lo sviluppatore deve scegliere qualsiasi overload più appropriato per il caso d'uso. Indipendentemente dall'overload scelto, il valore restituito dal metodo Protect è ora protetto (crittografato e a prova di manomissione) e l'applicazione può inviarlo a un client non attendibile.

Per rimuovere la protezione di un pezzo di dati protetto in precedenza, passare i dati protetti al metodo `Unprotect`. Sono disponibili overload basati su byte [] e basati su stringa per praticità degli sviluppatori. Se il payload protetto è stato generato da una chiamata precedente a `Protect` sulla stessa `IDataProtector`, il metodo `Unprotect` restituirà il payload originale non protetto. Se il payload protetto è stato manomesso o è stato prodotto da un `IDataProtector`diverso, il metodo `Unprotect` genererà CryptographicException.

Il concetto di confronto tra gli stessi e i `IDataProtector` diversi si ricollega al concetto di scopo. Se due istanze di `IDataProtector` sono state generate dalla stessa `IDataProtectionProvider` radice ma tramite stringhe per finalità diverse nella chiamata a `IDataProtectionProvider.CreateProtector`, vengono considerate [protezioni diverse](xref:security/data-protection/consumer-apis/purpose-strings)e non sarà possibile rimuovere la protezione dei payload generati dall'altro.

## <a name="consuming-these-interfaces"></a>Utilizzo di queste interfacce

Per un componente compatibile con, l'utilizzo previsto è che il componente accetta un parametro `IDataProtectionProvider` nel relativo costruttore e che il sistema DI fornisce automaticamente questo servizio quando viene creata un'istanza del componente.

> [!NOTE]
> Alcune applicazioni, ad esempio le applicazioni console o le applicazioni ASP.NET 4. x, potrebbero non essere compatibili con, quindi non è possibile usare il meccanismo descritto DI seguito. Per questi scenari, vedere il documento relativo agli [scenari non compatibili](xref:security/data-protection/configuration/non-di-scenarios) per ulteriori informazioni sul recupero di un'istanza di un provider di `IDataProtection` senza passare a.

Nell'esempio seguente vengono illustrati tre concetti:

1. [Aggiungere il sistema di protezione dei dati](xref:security/data-protection/configuration/overview) al contenitore del servizio

2. Utilizzo di per ricevere un'istanza di un `IDataProtectionProvider`e

3. Creazione di un `IDataProtector` da un `IDataProtectionProvider` e utilizzo per la protezione e la rimozione della protezione dei dati.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Il pacchetto Microsoft. AspNetCore. dataprotection. abstracts contiene un metodo di estensione `IServiceProvider.GetDataProtector` come praticità per gli sviluppatori. Incapsula come singola operazione il recupero di un `IDataProtectionProvider` dal provider di servizi e la chiamata di `IDataProtectionProvider.CreateProtector`. Nell'esempio seguente viene illustrato l'utilizzo.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Le istanze di `IDataProtectionProvider` e `IDataProtector` sono thread-safe per più chiamanti. Quando un componente ottiene un riferimento a un `IDataProtector` tramite una chiamata a `CreateProtector`, utilizzerà tale riferimento per più chiamate a `Protect` e `Unprotect`. Una chiamata a `Unprotect` genererà CryptographicException se non è possibile verificare o decifrare il payload protetto. Alcuni componenti potrebbero voler ignorare gli errori durante le operazioni di rimozione della protezione. un componente che legge i cookie di autenticazione potrebbe gestire questo errore e trattare la richiesta come se non fosse disponibile alcun cookie anziché interrompere la richiesta in modo non corretto. I componenti che vogliono questo comportamento devono rilevare in modo specifico CryptographicException anziché ingoiare tutte le eccezioni.
