---
title: Protezione dati in ASP.NET Core
author: rick-anderson
description: Questo documento funge da sommario per vari argomenti sulla protezione dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277124"
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
