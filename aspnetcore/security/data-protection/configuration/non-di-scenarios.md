---
title: Scenari non compatibili con la protezione dei dati in ASP.NET Core
author: rick-anderson
description: Informazioni su come supportare scenari di protezione dei dati in cui non è possibile o non si vuole usare un servizio fornito dall'inserimento delle dipendenze.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 62280a9f911b003383cbe348b9b62942766a2b99
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666236"
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>Scenari non compatibili con la protezione dei dati in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Il sistema di protezione dei dati ASP.NET Core viene [in genere aggiunto a un contenitore di servizi](xref:security/data-protection/consumer-apis/overview) e utilizzato dai componenti dipendenti tramite l'inserimento di dipendenze. In alcuni casi, tuttavia, non è fattibile o auspicabile, soprattutto quando si importa il sistema in un'app esistente.

Per supportare questi scenari, il pacchetto [Microsoft. AspNetCore. dataprotection. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) fornisce un tipo concreto, [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), che offre un modo semplice per usare la protezione dei dati senza basarsi su di. Il tipo di `DataProtectionProvider` implementa [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). La costruzione di `DataProtectionProvider` richiede solo la fornitura di un'istanza di [DirectoryInfo](/dotnet/api/system.io.directoryinfo) per indicare la posizione in cui devono essere archiviate le chiavi crittografiche del provider, come illustrato nell'esempio di codice seguente:

[!code-csharp[](non-di-scenarios/_static/nodisample1.cs)]

Per impostazione predefinita, il tipo concreto `DataProtectionProvider` non crittografa il materiale della chiave RAW prima di renderlo persistente nel file system. Questo è il supporto per gli scenari in cui lo sviluppatore punta a una condivisione di rete e il sistema di protezione dei dati non può dedurre automaticamente un meccanismo di crittografia della chiave at-rest appropriato.

Inoltre, il tipo concreto `DataProtectionProvider` non [isola le app](xref:security/data-protection/configuration/overview#per-application-isolation) per impostazione predefinita. Tutte le app che usano la stessa directory della chiave possono condividere i payload purché i [parametri degli scopi](xref:security/data-protection/consumer-apis/purpose-strings) corrispondano.

Il costruttore [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) accetta un callback di configurazione facoltativo che può essere usato per modificare i comportamenti del sistema. Nell'esempio seguente viene illustrato il ripristino dell'isolamento con una chiamata esplicita a [MyApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). Nell'esempio viene inoltre illustrata la configurazione del sistema per la crittografia automatica delle chiavi salvate in modo permanente mediante Windows DPAPI. Se la directory punta a una condivisione UNC, potrebbe essere necessario distribuire un certificato condiviso tra tutti i computer pertinenti e configurare il sistema per l'utilizzo della crittografia basata su certificato con una chiamata a [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-csharp[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Le istanze del `DataProtectionProvider` tipo concreto sono costose da creare. Se un'app gestisce più istanze di questo tipo e se tutti usano la stessa directory di archiviazione chiavi, le prestazioni dell'app potrebbero peggiorare. Se si usa il tipo di `DataProtectionProvider`, è consigliabile creare questo tipo una volta e riutilizzarlo il più possibile. Il tipo di `DataProtectionProvider` e tutte le istanze di [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) create da esso sono thread-safe per più chiamanti.
