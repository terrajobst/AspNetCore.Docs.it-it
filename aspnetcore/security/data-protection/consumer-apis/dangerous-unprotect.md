---
title: Rimuovere la protezione di payload le cui chiavi sono state revocate in ASP.NET Core
author: rick-anderson
description: Informazioni su come rimuovere la protezione dei dati, protetti con chiavi che poiché revocate in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 26061d048dcd9c1e3d8909e9388d8b565376fa2f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897108"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>Rimuovere la protezione di payload le cui chiavi sono state revocate in ASP.NET Core

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

Le API di protezione dati ASP.NET Core non sono principalmente destinati indefinita persistenza del payload riservati. Come altre tecnologie [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) e [Azure Rights Management](/rights-management/) sono più adatte allo scenario di archiviazione illimitata, e hanno le funzionalità di gestione delle chiavi sicuro o ridotta di conseguenza. Ciò premesso, non c'è niente divieto di uno sviluppatore di usare le API di protezione dati ASP.NET Core per la protezione a lungo termine dei dati riservati. Le chiavi non vengono mai rimossi dal Keyring, pertanto `IDataProtector.Unprotect` sempre possibile recuperare i payload esistenti, purché le chiavi sono disponibili e validi.

Tuttavia, un problema si verifica quando lo sviluppatore prova a rimuovere la protezione dei dati che sono stato protetto con una chiave revocata, come `IDataProtector.Unprotect` genererà un'eccezione in questo caso. Questo può essere adatto per i payload di breve durati o temporanei (ad esempio, i token di autenticazione), perché questi tipi di payload possono essere ricreati facilmente dal sistema e nel peggiore dei casi i visitatori del sito potrebbe essere necessario accedere di nuovo. Ma per i payload persistenti, visto `Unprotect` throw può causare la perdita di dati inaccettabile.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Per supportare lo scenario di consentire il payload per essere protetti anche in caso di chiavi revocate, il sistema di protezione dati contiene un `IPersistedDataProtector` tipo. Per ottenere un'istanza di `IPersistedDataProtector`, è sufficiente ottenere un'istanza di `IDataProtector` in modo normale e provare a eseguire il cast di `IDataProtector` a `IPersistedDataProtector`.

> [!NOTE]
> Non tutti i `IDataProtector` istanze possono essere convertite in `IPersistedDataProtector`. Gli sviluppatori devono usare il C# per evitare eccezioni di runtime causato da un cast non valido come operatore o simile e devono essere preparati a gestire il caso di errore in modo appropriato.

`IPersistedDataProtector` espone la superficie dell'API seguente:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Questa API richiede il payload protetto (come una matrice di byte) e restituisce il payload non protetto. Non vi è Nessun overload basato su stringa. Di seguito sono riportati i due parametri out.

* `requiresMigration`: impostato su true se la chiave utilizzata per proteggere questo payload non è più la chiave predefinita attiva, ad esempio, la chiave usata per proteggere questo payload è vecchia e dispone di un'operazione di rollover della chiave poiché vengono intraprese sul posto. Il chiamante può essere opportuno provare a eseguire la riprotezione del payload in base alla proprie esigenze aziendali.

* `wasRevoked`: verrà impostato su true se la chiave utilizzata per proteggere questo payload è stata revocata.

>[!WARNING]
> Prestare molta attenzione quando si passano `ignoreRevocationErrors: true` per il `DangerousUnprotect` (metodo). Se dopo la chiamata a questo metodo il `wasRevoked` valore è true, quindi la chiave utilizzata per proteggere questo payload è stata revocata e autenticità del payload deve essere trattato come sospetto. In questo caso, solo senza smettere di funzionare il payload non protetti se hai una discreta garanzia distinto che è autenticato, ad esempio, che proviene da un database sicuro, anziché inviati da un client web non attendibili.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
