---
title: Rimuovere la protezione dei payload le cui chiavi sono stati revocati in ASP.NET Core
author: rick-anderson
description: Informazioni su come rimuovere la protezione dati, protetti con chiavi che poiché revocate in un'applicazione ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: b721bba63d0673f4e22fd9d1456af33489a2a389
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077417"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>Rimuovere la protezione dei payload le cui chiavi sono stati revocati in ASP.NET Core


<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

Le API di protezione dati ASP.NET Core non sono principalmente destinati indefinita persistenza del payload riservato. Altre tecnologie come [DPAPI CNG di Windows](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) e [Azure Rights Management](https://docs.microsoft.com/rights-management/) sono più adatti per lo scenario di memorizzazione indefinito, e hanno funzionalità di gestione delle chiavi conseguentemente sicuro. Ciò premesso, non c'è niente che uno sviluppatore di utilizzare le API di protezione dati ASP.NET Core per la protezione a lungo termine dei dati riservati. Le chiavi non vengono mai rimosse dalla sequenza di chiave, pertanto `IDataProtector.Unprotect` può sempre il ripristino payload esistenti, purché le chiavi sono disponibili e validi.

Tuttavia, si verifica un problema quando lo sviluppatore tenta di rimuovere la protezione dati che sono stato protetto con una chiave revocata, come `IDataProtector.Unprotect` genererà un'eccezione in questo caso. Potrebbe essere appropriato per payload di breve durato o temporanee (ad esempio, il token di autenticazione), questi tipi di payload possono facilmente essere ricreati mediante il sistema e nel peggiore dei casi i visitatori del sito potrebbe essere necessario eseguire nuovamente l'accesso. Ma per il payload persistente, con `Unprotect` throw può causare la perdita di dati accettabile.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Per supportare lo scenario di consentire il payload essere protetto anche in caso di chiavi revocate, il sistema di protezione dati contiene un `IPersistedDataProtector` tipo. Per ottenere un'istanza di `IPersistedDataProtector`, semplicemente ottenere un'istanza di `IDataProtector` in modo normale e provare a eseguire il cast di `IDataProtector` a `IPersistedDataProtector`.

> [!NOTE]
> Non tutti i `IDataProtector` possono eseguire il cast di istanze per `IPersistedDataProtector`. Gli sviluppatori devono usare c# come operatore o simili per evitare eccezioni di runtime dovuto a cast non valido e deve essere preparato a gestire il caso di errore in modo appropriato.

`IPersistedDataProtector` espone la superficie dell'API seguente:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Questa API richiede il payload protetto (come una matrice di byte) e restituisce il payload non protetto. Non vi è alcun overload basato su stringa. Di seguito sono riportati i due parametri out.

* `requiresMigration`: verrà impostato su true se la chiave utilizzata per proteggere il payload non è più la chiave predefinita attiva, ad esempio, la chiave utilizzata per proteggere il payload è precedente ed è una chiave di sequenza di operazione eseguita sul posto. Il chiamante potrebbe essere necessario prendere in considerazione proteggere di nuovo il payload a seconda delle esigenze aziendali.

* `wasRevoked`: verrà impostato su true se la chiave utilizzata per proteggere il payload è stata revocata.

>[!WARNING]
> Prestare molta attenzione quando si passa `ignoreRevocationErrors: true` per il `DangerousUnprotect` metodo. Se dopo aver chiamato questo metodo il `wasRevoked` valore è true, quindi la chiave utilizzata per proteggere il payload è stata revocata e l'autenticità del payload devono essere considerate come sospetto. In questo caso, solo smettere di funzionare nel payload di non protetto se si dispone di una discreta garanzia separato che sia autentico, ad esempio, che provengono da un database protetto anziché inviati da un client web non attendibili.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
