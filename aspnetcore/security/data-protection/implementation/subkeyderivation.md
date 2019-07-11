---
title: Derivazione di sottochiavi e crittografia autenticata in ASP.NET Core
author: rick-anderson
description: Scopri i dettagli di implementazione della protezione dei dati di ASP.NET Core sottochiave derivazione e crittografia autenticata.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: bbfde378755b09cd5b1217b8cf66249b9fa1d6ad
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814379"
---
# <a name="subkey-derivation-and-authenticated-encryption-in-aspnet-core"></a>Derivazione di sottochiavi e crittografia autenticata in ASP.NET Core

<a name="data-protection-implementation-subkey-derivation"></a>

La maggior parte delle chiavi nel Keyring conterrà qualche forma di entropia e disporrà di informazioni algoritmiche indicante "crittografia in modalità CBC + convalida HMAC" o "crittografia GCM + convalida". In questi casi, si fa riferimento per l'entropia incorporata come il materiale della chiave master (o KM) per questa chiave e si esegue una funzione di derivazione della chiave per derivare le chiavi che verranno usate per le operazioni di crittografia effettive.

> [!NOTE]
> Le chiavi sono astratte e un'implementazione personalizzata potrebbe non comportarsi come indicato di seguito. Se la chiave fornisce la propria implementazione di `IAuthenticatedEncryptor` invece di usare uno dei nostri factory predefinita, il meccanismo descritte in questa sezione non è più valido.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Derivazione di sottochiavi e dati di autenticazione aggiuntivi

Il `IAuthenticatedEncryptor` interfaccia funge da interfaccia principale per tutte le operazioni di crittografia autenticata. Relativo `Encrypt` metodo accetta due buffer: in testo normale e additionalAuthenticatedData (AAD). Il flusso di contenuto di testo normale invariata la chiamata a `IDataProtector.Protect`, ma Azure ad è generato dal sistema ed è costituito da tre componenti:

1. L'intestazione magic 32 bit 09 F0 C9 F0 che identifica questa versione del sistema di protezione dati.

2. Id della chiave a 128 bit.

3. Una stringa a lunghezza variabile costituita dalla catena di utilizzo generico che ha creato il `IDataProtector` che sta eseguendo questa operazione.

Poiché Azure ad è univoco per la tupla di tre componenti, è possibile usarlo da cui per derivare nuove chiavi KM anziché KM stesso in tutte le operazioni crittografiche. Per ogni chiamata a `IAuthenticatedEncryptor.Encrypt`, avviene il processo di derivazione della chiave seguente:

( K_E, K_H ) = SP800_108_CTR_HMACSHA512(K_M, AAD, contextHeader || keyModifier)

In questo caso, stiamo chiamando il KDF SP800 108 NIST in modalità contatore (vedere [NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), 5.1 sec.) con i parametri seguenti:

* Chiave di derivazione della chiave (KDK) = K_M

* PRF = HMACSHA512

* label = additionalAuthenticatedData

* context = contextHeader || keyModifier

L'intestazione del contesto è di lunghezza variabile e funge essenzialmente un'identificazione personale degli algoritmi per il quale si sta derivazione K_E e K_H. Il modificatore di chiave è una stringa di 128 bit generata casualmente per ogni chiamata a `Encrypt` e serve a verificare con sovraccaricare probabilità che KE e KH siano univoci per l'operazione di crittografia di autenticazione specifica, anche se tutti gli altri input per il KDF è costante.

Per la crittografia in modalità CBC + operazioni di convalida del valore HMAC, | K_E | è la lunghezza della chiave di crittografia a blocchi simmetriche, e | K_H | è la dimensione del digest della routine HMAC. Per la crittografia di GCM e operazioni di convalida, | K_H | = 0.

## <a name="cbc-mode-encryption--hmac-validation"></a>Crittografia in modalità CBC + convalida HMAC

Una volta K_E viene generato tramite il meccanismo precedente, si genera un vettore di inizializzazione casuale ed esegue l'algoritmo di crittografia a blocchi simmetriche per software il testo non crittografato. Il vettore di inizializzazione e il testo crittografato vengono quindi eseguite tramite la routine HMAC inizializzata con la chiave K_H per produrre il Mac. Questo processo e il valore restituito è rappresentato graficamente di seguito.

![Return e processo in modalità CBC](subkeyderivation/_static/cbcprocess.png)

*output:= keyModifier || iv || E_cbc (K_E,iv,data) || HMAC(K_H, iv || E_cbc (K_E,iv,data))*

> [!NOTE]
> Il `IDataProtector.Protect` implementazione verrà [anteporre l'intestazione magic e l'id chiave](xref:security/data-protection/implementation/authenticated-encryption-details) all'output prima di restituirlo al chiamante. Perché i magic intestazione e un id chiave in modo implicito in parte [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), perché il modificatore di chiave viene inserito come input per il KDF, ciò significa che ogni singolo byte del payload restituito finale viene autenticato in Mac.

## <a name="galoiscounter-mode-encryption--validation"></a>Crittografia in modalità/contatore Galois + convalida

Una volta K_E viene generato tramite il meccanismo precedente, si genera un nonce 96 bit casuali ed esegue l'algoritmo di crittografia a blocchi simmetriche per software il testo non crittografato e produrre i tag di autenticazione a 128 bit.

![Return e processo della modalità di GCM](subkeyderivation/_static/galoisprocess.png)

*output := keyModifier || nonce || E_gcm (K_E,nonce,data) || authTag*

> [!NOTE]
> Anche se GCM supporta in modo nativo il concetto di AAD, abbiamo stiamo ancora somministrare AAD solo a KDF originale, queste ultime scelgono di passare una stringa vuota in GCM per il parametro AAD. Il motivo è duplice. Prima di tutto [per supportare agilità](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers) non si vuole mai usare K_M direttamente come chiave di crittografia. Inoltre, GCM impone requisiti di univocità molto rigorose per gli input. Imposta la probabilità che la routine di crittografia GCM sia mai richiamato su due o più distinct di dati di input con lo stesso (nonce chiave) coppia non deve superare i 2 ^ 32. Se si correggono K_E non è possibile eseguire più di 2 ^ 32 operazioni di crittografia prima che abbiamo eseguito afoul delle di 2 ^ limitare -32. Ciò può sembrare un numero molto elevato di operazioni, ma un server web a traffico elevato può attraversare 4 miliardi di richieste in giorni della semplice, anche all'interno di quella normale per queste chiavi. Per garantire la conformità di 2 ^ limite probabilità-32, si continuerà a usare un modificatore di chiave a 128 bit e a 96 bit nonce, che amplia notevolmente il conteggio delle operazioni utilizzabile per qualsiasi K_M specificato. Per motivi di semplicità di progettazione è condividere il percorso del codice KDF tra le operazioni CBC e GCM, e poiché AAD è già considerata nell'aspetto non è necessario per lo inoltra alla routine GCM.
