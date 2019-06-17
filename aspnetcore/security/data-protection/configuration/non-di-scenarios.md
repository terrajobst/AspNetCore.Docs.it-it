---
title: Scenari non compatibili con protezione dei dati in ASP.NET Core
author: rick-anderson
description: Informazioni su come supportare scenari di protezione dei dati in cui non è possibile o non si vuole usare un servizio fornito dall'inserimento delle dipendenze.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 62280a9f911b003383cbe348b9b62942766a2b99
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048236"
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>Scenari non compatibili con protezione dei dati in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Il sistema di protezione dei dati di ASP.NET Core è in genere [aggiunto a un contenitore del servizio](xref:security/data-protection/consumer-apis/overview) e utilizzate dai componenti dipendenti tramite l'inserimento delle dipendenze (dipendenze). Tuttavia, vi sono casi in cui non è fattibile o desiderata, in particolare quando si importano il sistema in un'app esistente.

Per supportare questi scenari, il [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacchetto fornisce un tipo concreto, [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), che offre un modo semplice per usare la protezione dati non si basa sull'inserimento delle dipendenze. Il `DataProtectionProvider` il tipo implementa [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Costruzione `DataProtectionProvider` è necessario solo fornire un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) istanza per indicare dove archiviare le chiavi di crittografia del provider, come illustrato nell'esempio di codice seguente:

[!code-csharp[](non-di-scenarios/_static/nodisample1.cs)]

Per impostazione predefinita, il `DataProtectionProvider` tipo concreto non crittografa il materiale della chiave non elaborato prima salvarli nel file System. Si tratta per supportare scenari in cui i punti per gli sviluppatori a una condivisione di rete e il sistema di protezione dati automaticamente non è possibile dedurre un meccanismo di crittografia con chiave appropriata dei dati inattivi.

Inoltre, il `DataProtectionProvider` non di tipo concreto [isolare le app](xref:security/data-protection/configuration/overview#per-application-isolation) per impostazione predefinita. Tutte le app usando la stessa chiave directory condivisibile, purché i payload loro [allo scopo di parametri](xref:security/data-protection/consumer-apis/purpose-strings) corrispondono.

Il [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) costruttore accetta un callback di configurazione facoltativa che può essere utilizzato per regolare i comportamenti del sistema. L'esempio seguente viene illustrato il ripristino isolamento con una chiamata esplicita a [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). L'esempio illustra anche la configurazione del sistema per la crittografia automatica delle chiavi persistenti con Windows DPAPI. Se la directory punta a una condivisione UNC, è consigliabile distribuire un certificato condiviso tra tutti i computer interessati e per configurare il sistema per usare la crittografia basata su certificati con una chiamata a [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-csharp[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Le istanze del `DataProtectionProvider` tipo concreto sono costose da creare. Se un'app gestisce più istanze di questo tipo e se tutte utilizzano la stessa directory di archiviazione delle chiavi, potrebbe compromettere le prestazioni delle app. Se si usa il `DataProtectionProvider` tipo, è consigliabile creare questo tipo una sola volta e riutilizzarlo quanto più possibile. Il `DataProtectionProvider` tipo e tutti i [oggetto IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) istanze create da quest'ultimo sono thread-safe per i chiamanti di più.
