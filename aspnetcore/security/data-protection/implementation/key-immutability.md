---
title: "Immutabilità chiave e la modifica delle impostazioni"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 3afce8f84ebe3b709ea169c7db27f99829f157ab
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="key-immutability-and-changing-settings"></a>Immutabilità chiave e la modifica delle impostazioni

Una volta che un oggetto è persistente nell'archivio di backup, rappresentazione forever è fissa. Nuovi dati possono essere aggiunti all'archivio di backup, ma i dati esistenti non possono essere modificati. Lo scopo principale di questo comportamento è per evitare il danneggiamento dei dati.

Una conseguenza di questo comportamento è che una volta che una chiave viene scritta all'archivio di backup, non modificabile. Le date di creazione, attivazione e la scadenza non possono essere modificate, anche se è possibile revocare IKeyManager. Inoltre, il sottostante informazioni algoritmiche, materiale della chiave master e la crittografia a proprietà rest anche sono modificabili.

Se lo sviluppatore modifica qualsiasi impostazione che influisce sulla chiave persistenza, tali modifiche non diventeranno effettive fino alla successiva, viene generata una chiave tramite una chiamata esplicita a IKeyManager.CreateNewKey o tramite dati protezione del sistema proprio [automatico generazione di chiavi](key-management.md#data-protection-implementation-key-management) comportamento. Le impostazioni che influiscono sulla persistenza chiave sono i seguenti:

* [Durata della chiave predefinita](key-management.md#data-protection-implementation-key-management)

* [La crittografia della chiave al meccanismo di rest](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [Le informazioni algoritmiche contenute all'interno della chiave](../configuration/overview.md#data-protection-changing-algorithms)

Se è necessario per avviare precedenti la successiva chiave automatica in sequenza ora queste impostazioni, effettuare una chiamata esplicita a IKeyManager.CreateNewKey per imporre la creazione di una nuova chiave. Ricordarsi di fornire una data di attivazione esplicita ({ora + 2 giorni} è una buona regola pratica di tempo affinché la modifica si propaghi) e data di scadenza nella chiamata.

>[!TIP]
> Tutte le applicazioni tocca il repository è necessario specificare le stesse impostazioni con i metodi di estensione IDataProtectionBuilder, in caso contrario le proprietà della chiave persistente sarà dipende l'applicazione specifica che ha richiamato la routine di generazione delle chiavi .
