---
title: Non DI scenari compatibile con protezione dei dati in ASP.NET Core
author: rick-anderson
description: "Informazioni su come supportare gli scenari di protezione dati in cui non è possibile o non si desidera utilizzare un servizio fornito dall'inserimento di dipendenze."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: eccf914d20e04adbb113f17e262766ed2dd1a554
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>Non DI scenari compatibile con protezione dei dati in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Il sistema di protezione dei dati di ASP.NET Core è in genere [aggiunto a un contenitore di servizi](xref:security/data-protection/consumer-apis/overview) e utilizzate dai componenti dipendenti tramite l'inserimento di dipendenze (DI). Tuttavia, vi sono casi in cui questo non è fattibile o desiderata, soprattutto quando si importa il sistema in un'app di esistente.

Per supportare questi scenari, il [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacchetto fornisce un tipo concreto, [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), che offre un modo semplice per utilizzare la protezione dei dati senza basarsi su DI. Il `DataProtectionProvider` il tipo implementa [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Costruzione di `DataProtectionProvider` è necessario solo fornire un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) istanza per indicare le chiavi di crittografia del provider in cui devono essere archiviate, come illustrato nell'esempio di codice seguente:

[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]

Per impostazione predefinita, il `DataProtectionProvider` tipo concreto non crittografa il materiale della chiave non elaborato prima renderli persistenti nel file System. Si tratta per supportare scenari in cui i punti di sviluppatore per una condivisione di rete e il sistema di protezione dei dati non è possibile dedurre automaticamente un meccanismo di crittografia con chiave appropriate nel resto.

Inoltre, il `DataProtectionProvider` non di tipo concreto [isolare le app](xref:security/data-protection/configuration/overview#per-application-isolation) per impostazione predefinita. Tutte le app usando la stessa chiave directory possono condividere i payload, purché i [allo scopo di parametri](xref:security/data-protection/consumer-apis/purpose-strings) corrispondono.

Il [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) costruttore accetta un callback di configurazione facoltativa che può essere usato per modificare i comportamenti del sistema. Nell'esempio seguente viene illustrato il ripristino di isolamento con una chiamata esplicita a [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). L'esempio illustra anche la configurazione del sistema per crittografare automaticamente le chiavi persistenti con DPAPI di Windows. Se la directory punta a una condivisione UNC, è preferibile distribuire un certificato condiviso tra tutti i computer interessati e per configurare il sistema per utilizzare la crittografia basata su certificati con una chiamata a [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Le istanze del `DataProtectionProvider` tipo concreto sono costose da creare. Se un'applicazione gestisce più istanze di questo tipo e se tutti utilizzano la stessa directory di archiviazione delle chiavi, potrebbero influire negativamente sulle prestazioni dell'applicazione. Se si utilizza il `DataProtectionProvider` tipo, si consiglia di creare questo tipo una sola volta e riutilizzarla in quanto possibile. Il `DataProtectionProvider` tipo e tutti [oggetto IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) istanze create da quest'ultimo sono thread-safe per più i chiamanti.
