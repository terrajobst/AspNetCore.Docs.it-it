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
ms.openlocfilehash: b846fb7cb28eeceb8c0bdb47135e1cf014ae08a7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Protezione dati in ASP.NET Core: le API di Consumer, configurazione, API di estensibilità e implementazione

* [Introduzione alla protezione dati](introduction.md)

* [Introduzione alle Data Protection API](using-data-protection.md)

* [API utente](consumer-apis/index.md)

  * [Panoramica delle API consumer](consumer-apis/overview.md)

  * [Stringhe di scopi](consumer-apis/purpose-strings.md)

  * [Gerarchia di scopi e multi-tenancy](consumer-apis/purpose-strings-multitenancy.md)

  * [Hashing della password](consumer-apis/password-hashing.md)

  * [Limitazione della durata di payload protetti](consumer-apis/limited-lifetime-payloads.md)

  * [Rimozione della protezione di payload le cui chiavi sono state revocate](consumer-apis/dangerous-unprotect.md)

* [Configurazione](configuration/index.md)

  * [Configurazione della protezione dati](configuration/overview.md)

  * [Impostazioni predefinite](configuration/default-settings.md)

  * [Criteri a livello di computer](configuration/machine-wide-policy.md)

  * [Scenari non compatibili con l'inserimento di dipendenze](configuration/non-di-scenarios.md)

* [API di estendibilità](extensibility/index.md)

  * [Estendibilità della crittografia Core](extensibility/core-crypto.md)

  * [Estendibilità della gestione delle chiavi](extensibility/key-management.md)

  * [Varie API](extensibility/misc-apis.md)

* [Implementazione](implementation/index.md)

  * [Dettagli di crittografia autenticata](implementation/authenticated-encryption-details.md)

  * [Derivazione di sottochiavi e crittografia autenticata](implementation/subkeyderivation.md)

  * [Intestazioni di contesto](implementation/context-headers.md)

  * [Gestione delle chiavi](implementation/key-management.md)

  * [Provider di archiviazione chiavi](implementation/key-storage-providers.md)

  * [Crittografia a chiave inattiva](implementation/key-encryption-at-rest.md)

  * [Immutabilità delle chiavi e modifica delle impostazioni](implementation/key-immutability.md)

  * [Formato di archiviazione chiavi](implementation/key-storage-format.md)

  * [Provider di protezione dati temporanea](implementation/key-storage-ephemeral.md)

* [Compatibilità](compatibility/index.md)

  * [Condivisione di cookie tra le app](xref:security/data-protection/compatibility/cookie-sharing)

  * [Sostituzione di <machineKey> in ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
