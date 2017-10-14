---
title: Derivazione della sottochiave e la crittografia autenticata
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 34bb58a3-5a9a-41e5-b090-08f75b4bbefa
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: e070742b5d9966c4772fd2f0a6d637d98a46137c
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="subkey-derivation-and-authenticated-encryption"></a>Derivazione della sottochiave e la crittografia autenticata

<a name="data-protection-implementation-subkey-derivation"></a>

La maggior parte delle chiavi nell'anello chiave conterrà una forma di entropia e disporrà di informazioni algoritmiche indicante "crittografia in modalità CBC + convalida HMAC" o "crittografia GCM + convalida". In questi casi, l'entropia incorporato noti come il materiale della chiave master (o KM) per la chiave e si esegue una funzione di derivazione della chiave per derivare le chiavi che verranno utilizzate per le operazioni di crittografia effettive.

> [!NOTE]
> Le chiavi sono classi astratte e un'implementazione personalizzata potrebbe non funzionare come indicato di seguito. Se la chiave fornisce la propria implementazione dell'IAuthenticatedEncryptor, anziché utilizzare uno dei nostri factory incorporate, il meccanismo descritto in questa sezione non è più applicabile.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Dati di autenticazione aggiuntivi e la derivazione della sottochiave

L'interfaccia IAuthenticatedEncryptor funge da interfaccia principale per tutte le operazioni di crittografia autenticata. Il metodo di crittografia accetta due buffer: testo non crittografato e additionalAuthenticatedData (AAD). Il flusso di contenuto di testo normale invariata la chiamata a IDataProtector.Protect, ma AAD è generato dal sistema ed è costituito da tre componenti:

1. L'intestazione magic 32-bit 09 F0 C9 F0 che identifica questa versione del sistema di protezione dati.

2. Id della chiave a 128 bit.

3. Stringa a lunghezza variabile formata dalla catena di scopo che ha creato l'oggetto IDataProtector che esegue questa operazione.

Poiché Azure ad è univoco per la tupla di tutti e tre i componenti, è possibile usarlo per derivare nuove chiavi KM anziché KM stesso in tutti gli operazioni di crittografia. Per ogni chiamata a IAuthenticatedEncryptor.Encrypt, il processo di derivazione della chiave seguente ha luogo:

(K_E, K_H) = SP800_108_CTR_HMACSHA512 (contextHeader K_M, AAD, | | keyModifier)

In questo caso, stiamo chiamando l'utilizzo di SP800-108 NIST in modalità di contatore (vedere [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), sec. 5.1) con i parametri seguenti:

* Chiave di derivazione della chiave (KDK) = K_M

* PRF = HMACSHA512

* etichetta = additionalAuthenticatedData

* contesto = contextHeader | | keyModifier

L'intestazione del contesto è di lunghezza variabile e funge essenzialmente da un'identificazione personale degli algoritmi per il quale si sta derivazione K_E e K_H. Il modificatore di chiave è una stringa di 128 bit generata in modo casuale per ogni chiamata a Encrypt e serve a garantire con sovraccaricare probabilità che KE e KH siano univoci per questa operazione di crittografia di autenticazione specifici, anche se tutti gli altri input per l'utilizzo è costanti.

Per la crittografia in modalità CBC + le operazioni di convalida HMAC, | K_E | è la lunghezza della chiave di crittografia simmetrico, e | K_H | è la dimensione del digest della routine HMAC. Per la crittografia GCM + operazioni di convalida, | K_H | = 0.

## <a name="cbc-mode-encryption--hmac-validation"></a>La crittografia in modalità CBC + convalida HMAC

Una volta K_E viene generato tramite il meccanismo precedente, si genera un vettore di inizializzazione casuale ed eseguire l'algoritmo di crittografia simmetrica blocco per crittografia il testo non crittografato. Il vettore di inizializzazione e il testo crittografato vengono quindi eseguite tramite la routine HMAC inizializzata con la chiave K_H per produrre il Mac. Questo processo e il valore restituito è rappresentato graficamente di seguito.

![Restituzione e il processo in modalità CBC](subkeyderivation/_static/cbcprocess.png)

*output: = keyModifier | | vettore di inizializzazione | | E_cbc (dati K_E, iv) | | Codice HMAC (K_H, iv | | E_cbc (dati K_E, iv))*

> [!NOTE]
> L'implementazione di IDataProtector.Protect verrà [anteporre l'intestazione magic e l'id chiave](authenticated-encryption-details.md#data-protection-implementation-authenticated-encryption-details) all'output prima di restituirlo al chiamante. Poiché l'intestazione magic e l'id chiave sono implicitamente in parte [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), e poiché il modificatore di chiave viene inserito come input per l'utilizzo, questo significa che ogni singolo byte del payload restituito finale è autenticato da Mac.

## <a name="galoiscounter-mode-encryption--validation"></a>La crittografia della modalità Galois/contatore + convalida

Una volta K_E viene generato tramite il meccanismo precedente, si genera un parametro nonce 96 bit casuale ed eseguire l'algoritmo di crittografia simmetrica blocco per crittografia il testo non crittografato e generare il tag di autenticazione a 128 bit.

![Restituzione e processo in modalità GCM](subkeyderivation/_static/galoisprocess.png)

*output: = keyModifier | | parametro nonce | | E_gcm (K_E, nonce, i dati) | | authTag*

> [!NOTE]
> Anche se GCM supporta in modo nativo il concetto di AAD, ci stiamo ancora alimentazione AAD solo per l'utilizzo originale, scelta per passare una stringa vuota in GCM per il parametro AAD. Il motivo è duplice. Prima di tutto, [per supportare l'agilità](context-headers.md#data-protection-implementation-context-headers) non si desidera mai utilizzare K_M direttamente come chiave di crittografia. Inoltre, GCM impone requisiti di univocità molto rigidi per gli input. Imposta la probabilità che la routine di crittografia GCM viene sempre richiamato su due o più distinct di dati di input con lo stesso (nonce chiave) coppia non deve superare i 2 ^ 32. Se è possibile risolvere K_E non è possibile eseguire più di 2 ^ 32 operazioni di crittografia, prima è eseguire afoul delle di 2 ^ limitare -32. Ciò potrebbe sembrare un numero molto elevato di operazioni, ma un server web a traffico elevato può passare attraverso le richieste di 4 miliardi in giorni semplice, comprese la durata normale per queste chiavi. Per garantire la conformità di 2 ^ limite probabilità-32, si continuerà a utilizzare un modificatore di chiave a 128 bit e parametro nonce 96 bit, che estende radicalmente il conteggio delle operazioni utilizzabile per qualsiasi K_M specificato. Per semplicità di progettazione è condividere il percorso del codice di utilizzo tra le operazioni CBC e GCM e poiché Azure ad è già considerata nell'utilizzo non è necessario inoltrarla alla routine GCM.
