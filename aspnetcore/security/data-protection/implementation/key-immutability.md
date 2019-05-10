---
title: Immutabilità delle chiavi e le impostazioni della chiave in ASP.NET Core
author: rick-anderson
description: Scopri i dettagli di implementazione di immutabilità delle chiavi di protezione dei dati di ASP.NET Core le API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892288"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>Immutabilità delle chiavi e le impostazioni della chiave in ASP.NET Core

Dopo che un oggetto è persistente nell'archivio di backup, relativa rappresentazione in forma è fissa per sempre. Nuovi dati possono essere aggiunti all'archivio di backup, ma i dati esistenti non possono essere modificati. Lo scopo principale di questo comportamento è per evitare il danneggiamento dei dati.

Una conseguenza di questo comportamento è che una volta che una chiave verrà scritti in archivio di backup, non è modificabile. Le date di creazione, l'attivazione e scadenza non possono mai essere modificate, anche se è possibile revocare `IKeyManager`. Inoltre, il sottostante informazioni algoritmiche, materiale della chiave master e le proprietà rimanenti della crittografia dei dati anche sono modificabili.

Se lo sviluppatore modifica qualsiasi impostazione che influisce sulla persistenza chiave, tali modifiche non verranno esaminate effetto fino alla successiva esecuzione verrà generata una chiave, tramite una chiamata esplicita a `IKeyManager.CreateNewKey` o tramite i dati protezione del sistema proprio [chiave automatico generazione](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) comportamento. Le impostazioni che influiscono sulla persistenza chiave sono i seguenti:

* [Durata della chiave predefinita](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [La chiave di crittografia al meccanismo di rest](xref:security/data-protection/implementation/key-encryption-at-rest)

* [Le informazioni algoritmiche contenute all'interno della chiave](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Se è necessario per avviare la successiva chiave automatica in sequenza ora anteriori queste impostazioni, prendere in considerazione una chiamata esplicita a `IKeyManager.CreateNewKey` per forzare la creazione di una nuova chiave. Specificare una data di attivazione esplicita ({ora + 2 giorni} è una buona norma consentire tempo propagare le modifiche) e data di scadenza nella chiamata.

>[!TIP]
> Toccare il repository di tutte le applicazioni devono specificare le stesse impostazioni con la `IDataProtectionBuilder` i metodi di estensione. In caso contrario, le proprietà della chiave persistente saranno dipendenti da questa applicazione che ha richiamato la routine di generazione della chiave.
