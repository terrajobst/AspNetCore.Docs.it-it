---
title: "Immutabilità chiave e la modifica delle impostazioni"
author: rick-anderson
description: "Questo documento descrive i dettagli di implementazione di ASP.NET Core dati protezione chiave immutabilità API."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 8e46e634266fa5f082c47f3be306009eb54bcbcc
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="key-immutability-and-changing-settings"></a>Immutabilità chiave e la modifica delle impostazioni

Una volta che un oggetto è persistente nell'archivio di backup, rappresentazione forever è fissa. Nuovi dati possono essere aggiunti all'archivio di backup, ma i dati esistenti non possono essere modificati. Lo scopo principale di questo comportamento è per evitare il danneggiamento dei dati.

Una conseguenza di questo comportamento è che una volta che una chiave viene scritta all'archivio di backup, non modificabile. Le date di creazione, attivazione e la scadenza non possono essere modificate, anche se è possibile revocare `IKeyManager`. Inoltre, il sottostante informazioni algoritmiche, materiale della chiave master e la crittografia a proprietà rest anche sono modificabili.

Se lo sviluppatore modifica qualsiasi impostazione che influisce sulla chiave persistenza, tali modifiche non diventeranno effettive fino alla successiva esecuzione viene generata una chiave, tramite una chiamata esplicita a `IKeyManager.CreateNewKey` o tramite dati protezione del sistema proprio [chiave automatico generazione](key-management.md#data-protection-implementation-key-management) comportamento. Le impostazioni che influiscono sulla persistenza chiave sono i seguenti:

* [Durata della chiave predefinita](key-management.md#data-protection-implementation-key-management)

* [La crittografia della chiave al meccanismo di rest](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [Le informazioni algoritmiche contenute all'interno della chiave](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Se è necessario per avviare precedenti la successiva chiave automatica in sequenza ora queste impostazioni, prendere in considerazione una chiamata esplicita a `IKeyManager.CreateNewKey` per imporre la creazione di una nuova chiave. Ricordarsi di fornire una data di attivazione esplicita ({ora + 2 giorni} è una buona regola pratica di tempo affinché la modifica si propaghi) e data di scadenza nella chiamata.

>[!TIP]
> Tocca il repository tutte le applicazioni specifichino le stesse impostazioni con la `IDataProtectionBuilder` metodi di estensione. In caso contrario, le proprietà della chiave persistente saranno dipende l'applicazione specifica che ha richiamato la routine di generazione delle chiavi.
