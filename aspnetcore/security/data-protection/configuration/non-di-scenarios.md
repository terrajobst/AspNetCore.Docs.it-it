---
title: Scenari di supporto DI non
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a7d8a962-80ff-48e3-96f6-8472b7ba2df9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 54a930c26f9f48ea0e6f7865e2927bcde0f4d6c0
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="non-di-aware-scenarios"></a>Scenari di supporto DI non

Il sistema di protezione dati è in genere progettato [da aggiungere a un contenitore di servizi](../consumer-apis/overview.md) e deve essere fornito per componenti dipendenti tramite un meccanismo DI. Tuttavia, potrebbero esserci alcuni casi in cui questo non è possibile, soprattutto quando si importa il sistema in un'applicazione esistente.

Per supportare questi scenari il pacchetto Microsoft.AspNetCore.DataProtection.Extensions fornisce un tipo concreto DataProtectionProvider che offre un modo semplice per utilizzare il sistema di protezione dati senza passare attraverso i percorsi del codice DI specifiche. Il tipo stesso implementa IDataProtectionProvider e costruzione, è sufficiente fornire DirectoryInfo in cui le chiavi di crittografia del provider devono essere archiviate.

Ad esempio:

[!code-none[Principale](non-di-scenarios/_static/nodisample1.cs)]

>[!WARNING]
> Per impostazione predefinita il DataProtectionProvider tipo concreto non consente di crittografare il materiale della chiave non elaborato prima renderli persistenti nel file System. Si tratta per supportare scenari in cui condividono i punti di sviluppatori a una rete, nel qual caso il sistema di protezione dati automaticamente non è possibile dedurre un un meccanismo di crittografia con chiave appropriate nel resto.
>
>Inoltre, il tipo concreto DataProtectionProvider non [isolare le applicazioni](overview.md#data-protection-configuration-per-app-isolation) per impostazione predefinita, in modo che tutte le applicazioni a cui fa riferimento alla stessa directory nella chiave possono condividere i payload, purché i relativi parametri di fine corrispondono.

Lo sviluppatore di applicazioni può soddisfare entrambi questi se lo si desidera. Il costruttore DataProtectionProvider accetta un [callback configurazione facoltativa](overview.md#data-protection-configuration-callback) che può essere usato per modificare i comportamenti del sistema. Nell'esempio seguente viene illustrato il ripristino di isolamento tramite una chiamata esplicita a SetApplicationName, e viene inoltre illustrato come configurare il sistema per crittografare automaticamente le chiavi persistenti con DPAPI di Windows. Se la directory punta a una condivisione UNC, si consiglia di distribuire un certificato condiviso tra tutti i computer interessati e di configurare il sistema per utilizzare invece la crittografia basata sui certificati mediante una chiamata a [ProtectKeysWithCertificate](overview.md#configuring-x509-certificate).

[!code-none[Principale](non-di-scenarios/_static/nodisample2.cs)]

>[!TIP]
> Le istanze del tipo concreto DataProtectionProvider sono costose da creare. Se un'applicazione gestisce più istanze di questo tipo e se sta tutti riferimento alla stessa directory di archiviazione chiavi, le prestazioni dell'applicazione potrebbero essere compromesse. L'utilizzo previsto è che lo sviluppatore dell'applicazione, creare un'istanza di questo tipo di una volta quindi mantenere il più possibile riutilizzare questo singolo riferimento. Il tipo DataProtectionProvider e tutte le istanze di oggetto IDataProtector create da quest'ultimo sono thread-safe per più i chiamanti.
