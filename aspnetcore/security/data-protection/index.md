---
title: Protezione dati in ASP.NET Core
author: rick-anderson
description: Questo documento funge da sommario per vari argomenti sulla protezione dati di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071696"
---
# <a name="data-protection-in-aspnet-core"></a>Protezione dati in ASP.NET Core

* [Introduzione alla protezione dati](xref:security/data-protection/introduction)

* [Introduzione alle Data Protection API](xref:security/data-protection/using-data-protection)

* [API utente](xref:security/data-protection/consumer-apis/index)

  * [Panoramica delle API consumer](xref:security/data-protection/consumer-apis/overview)

  * [Stringhe di scopi](xref:security/data-protection/consumer-apis/purpose-strings)

  * [Gerarchia di scopi e multi-tenancy](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [Password con hash](xref:security/data-protection/consumer-apis/password-hashing)

  * [Limitare la durata di payload protetti](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [Payload non protetti le cui chiavi sono state revocate](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [Configurazione](xref:security/data-protection/configuration/index)

  * [Configurare la protezione dati di ASP.NET Core](xref:security/data-protection/configuration/overview)

  * [Impostazioni predefinite](xref:security/data-protection/configuration/default-settings)

  * [Criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy)

  * [Scenari non compatibili con l'inserimento di dipendenze](xref:security/data-protection/configuration/non-di-scenarios)

* [API di estendibilità](xref:security/data-protection/extensibility/index)

  * [Estendibilità della crittografia Core](xref:security/data-protection/extensibility/core-crypto)

  * [Estendibilità della gestione delle chiavi](xref:security/data-protection/extensibility/key-management)

  * [Varie API](xref:security/data-protection/extensibility/misc-apis)

* [Implementazione](xref:security/data-protection/implementation/index)

  * [Dettagli di crittografia autenticata](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [Derivazione di sottochiavi e crittografia autenticata](xref:security/data-protection/implementation/subkeyderivation)

  * [Intestazioni di contesto](xref:security/data-protection/implementation/context-headers)

  * [Gestione delle chiavi](xref:security/data-protection/implementation/key-management)

  * [Provider di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers)

  * [Crittografia a chiave inattiva](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [Immutabilità delle chiavi e impostazioni](xref:security/data-protection/implementation/key-immutability)

  * [Formato di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-format)

  * [Provider di protezione dati temporanea](xref:security/data-protection/implementation/key-storage-ephemeral)

* [Compatibilità](xref:security/data-protection/compatibility/index)

  * [Sostituzione di ASP.NET <machineKey> in ASP.NET Core](xref:security/data-protection/compatibility/replacing-machinekey)
