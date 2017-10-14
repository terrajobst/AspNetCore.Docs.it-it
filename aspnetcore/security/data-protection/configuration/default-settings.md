---
title: Durata e la gestione delle chiavi
author: rick-anderson
description: Descrive una durata e la gestione delle chiavi.
keywords: ASP.NET Core, chiave di gestione, DPAPI, DataProtection
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ef7dad2a-7029-4ae5-8f06-1fbebedccaa4
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 5ac2d80e7d1cebcbc792e1247608e67991ec36f4
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="key-management-and-lifetime"></a>Durata e la gestione delle chiavi

<a name="data-protection-default-settings"></a>

## <a name="key-management"></a>Gestione delle chiavi

Il sistema tenta di rilevare l'ambiente operativo e fornire buone impostazioni predefinite di comportamento senza operazioni di configurazione. L'euristica utilizzata è la seguente.

1. Se il sistema è ospitato in siti Web di Azure, le chiavi vengono mantenute nella cartella "% HOME%\ASP.NET\DataProtection-Keys". Questa cartella viene supportata dall'archiviazione di rete e viene sincronizzata in tutti i computer che ospita l'applicazione. Le chiavi non sono protette inattivi. Questa cartella fornisce la gestione delle chiavi a tutte le istanze di un'applicazione in uno slot di distribuzione singolo. Gli slot di distribuzione separato, ad esempio gestione temporanea e produzione, non condividono un anello di chiave. Durante lo scambio tra gli slot di distribuzione, ad esempio lo scambio di gestione temporanea nell'ambiente di produzione o utilizzando A / B test, qualsiasi sistema utilizzando la protezione dei dati non sarà in grado di decrittografare i dati archiviati tramite la gestione delle chiavi all'interno di slot precedente. Questo conduce agli utenti viene disconnettersi dall'applicazione ASP.NET che utilizza middleware del cookie ASP.NET standard, in quanto utilizza la protezione dei dati per proteggere i propri cookie. Se si desiderano anelli chiave indipendente di slot, usare un provider di chiavi esterne, ad esempio l'archiviazione di Blob di Azure, insieme di credenziali chiave di Azure, un archivio SQL, oppure cache Redis.

2. Se il profilo utente è disponibile, le chiavi vengono mantenute nella cartella "% LOCALAPPDATA%\ASP.NET\DataProtection-Keys". Inoltre, se il sistema operativo è Windows, verranno crittografate a riposo usando la DPAPI.

3. Se l'applicazione è ospitata in IIS, le chiavi vengono mantenute nel Registro di sistema HKLM in una chiave del Registro di sistema speciale solo per l'account del processo di lavoro. Le chiavi vengono crittografate a riposo tramite la DPAPI.

4. Se corrisponde a nessuna di queste condizioni, le chiavi non sono persistenti all'esterno del processo corrente. Quando il processo viene arrestato, tutti generati chiavi andranno perse.

Lo sviluppatore è sempre il controllo completo e può eseguire l'override di come e dove sono memorizzate chiavi. Le prime tre opzioni sopra devono buona valori predefiniti per la maggior parte delle applicazioni allo stesso modo in ASP.NET <machineKey> routine la generazione automatica funzionavano in passato. Finale, l'opzione di failback rientrano è l'unico scenario che realmente richiede allo sviluppatore di specificare [configurazione](overview.md) iniziale se desiderano persistenza chiave, ma questo fallback si verifica solo in rare situazioni.

>[!WARNING]
> Se lo sviluppatore esegue l'override di questa euristica e indica al sistema di protezione dati in un repository di chiave specifico, la crittografia automatica delle chiavi inattivi verrà disabilitata. Inattivi protezione può essere riabilitata tramite [configurazione](overview.md).

## <a name="key-lifetime"></a>Durata della chiave

Le chiavi per impostazione predefinita hanno una durata di 90 giorni. Quando una chiave scade, il sistema verrà automaticamente generare una nuova chiave e impostare la nuova chiave come chiave attiva. Fino a quando le chiavi ritirate restano nel sistema sarà comunque in grado di decrittografare i dati protetti con essi. Vedere [gestione delle chiavi](../implementation/key-management.md#data-protection-implementation-key-management-expiration) per ulteriori informazioni.

## <a name="default-algorithms"></a>Algoritmi predefiniti

L'algoritmo di protezione dati di payload predefinito utilizzato è AES-256-CBC la riservatezza e HMACSHA256 l'autenticità. Una chiave master di 512 bit, il rollback di ogni 90 giorni, viene usata per derivare le due chiavi secondario utilizzate per questi algoritmi in base al payload. Vedere [sottochiave derivazione](../implementation/subkeyderivation.md#data-protection-implementation-subkey-derivation-aad) per ulteriori informazioni.
