---
title: Protezione dati in ASP.NET Core
author: rick-anderson
description: Questo documento funge da sommario per vari argomenti sulla protezione dati di ASP.NET Core.
keywords: ASP.NET Core,protezione dati
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2018
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

  * [Condivisione di cookie tra app](compatibility/cookie-sharing.md)

  * [Sostituzione di <machineKey> in ASP.NET](compatibility/replacing-machinekey.md)
