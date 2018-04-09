---
title: Immutabilità chiave e le impostazioni della chiave in ASP.NET Core
author: rick-anderson
description: Informazioni su dettagli di implementazione di immutabilità chiave ASP.NET Core Data Protection API.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: e918b00562aca9821de87c38f10242177517d8a5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>Immutabilità chiave e le impostazioni della chiave in ASP.NET Core

Una volta che un oggetto è persistente nell'archivio di backup, rappresentazione forever è fissa. Nuovi dati possono essere aggiunti all'archivio di backup, ma i dati esistenti non possono essere modificati. Lo scopo principale di questo comportamento è per evitare il danneggiamento dei dati.

Una conseguenza di questo comportamento è che una volta che una chiave viene scritta all'archivio di backup, non modificabile. Le date di creazione, attivazione e la scadenza non possono essere modificate, anche se è possibile revocare `IKeyManager`. Inoltre, il sottostante informazioni algoritmiche, materiale della chiave master e la crittografia a proprietà rest anche sono modificabili.

Se lo sviluppatore modifica qualsiasi impostazione che influisce sulla chiave persistenza, tali modifiche non verranno esaminate in effetto fino al successivo verrà generata una chiave, tramite una chiamata esplicita a `IKeyManager.CreateNewKey` o tramite dati protezione del sistema proprio [chiave automatico generazione](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) comportamento. Le impostazioni che influiscono sulla persistenza chiave sono i seguenti:

* [Durata della chiave predefinita](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [Crittografia della chiave al meccanismo di rest](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [Le informazioni algoritmiche contenute all'interno della chiave](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Se è necessario per avviare precedenti la successiva chiave automatica in sequenza ora queste impostazioni, prendere in considerazione una chiamata esplicita a `IKeyManager.CreateNewKey` per imporre la creazione di una nuova chiave. Ricordarsi di fornire una data di attivazione esplicita ({ora + 2 giorni} è una buona regola pratica di tempo affinché la modifica si propaghi) e data di scadenza nella chiamata.

>[!TIP]
> Tocca il repository tutte le applicazioni specifichino le stesse impostazioni con la `IDataProtectionBuilder` metodi di estensione. In caso contrario, le proprietà della chiave persistente saranno dipende l'applicazione specifica che ha richiamato la routine di generazione delle chiavi.
