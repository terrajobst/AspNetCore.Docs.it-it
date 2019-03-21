---
title: Intestazioni di contesto in ASP.NET Core
author: rick-anderson
description: Scopri i dettagli di implementazione delle intestazioni di contesto di protezione dei dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 2b8fd594672bf623d38bfae90d05a984f92ce6a3
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208366"
---
# <a name="context-headers-in-aspnet-core"></a>Intestazioni di contesto in ASP.NET Core

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>In background e la teoria

Nel sistema di protezione dati, una "chiave" significa che un oggetto che può fornire servizi di crittografia di autenticazione. Ogni chiave è identificata da un id univoco (GUID) e che comporta però algoritmiche informazioni e materiale entropic. Lo scopo è che ogni chiave trasportare univoco entropia, ma il sistema non può imporre quella e dobbiamo anche per gli sviluppatori che potrebbero modificare il gruppo di chiavi manualmente modificando le informazioni su algoritmi di una chiave esistente nel gruppo di chiavi dell'account. Per ottenere i requisiti di sicurezza assegnati questi casi il sistema di protezione dati è un concetto di [agilità di crittografia](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), consentendo in modo sicuro usando un singolo valore entropic tra più algoritmi di crittografia.

La maggior parte dei sistemi che supportano la agilità di crittografia farlo includendo alcune informazioni di identificazione dell'algoritmo all'interno del payload. OID dell'algoritmo è in genere un buon candidato per l'oggetto. Tuttavia, un problema che si è verificato è che esistono diversi modi per specificare lo stesso algoritmo: "AES" (CNG) e di Aes, AesManaged allo standard, AesCryptoServiceProvider, AesCng e RijndaelManaged (determinati parametri specifici) gestito classi sono tutti effettivamente la stessa cosa e dobbiamo mantengono un mapping di tutti questi all'OID corretti. Se uno sviluppatore desidera fornire un algoritmo personalizzato (o anche un'altra implementazione di AES), si dovrà indicare all'OID. Questo passaggio di registrazione aggiuntiva rende particolarmente dolorosa configurazione del sistema.

Ritorno al passaggio precedente, abbiamo deciso che si stava per raggiungere il problema dalla direzione errata. Un identificatore di oggetto viene identificato l'algoritmo, ma non sono effettivamente importanti questo. Se è necessario usare un singolo valore entropic in modo sicuro in due diversi algoritmi, non è necessario sapere quali effettivamente sono gli algoritmi. Che cos'è effettivamente necessario occuparsi è il comportamento. Qualsiasi algoritmo di crittografia a blocchi simmetriche ragionevole è anche una permutazione pseudocasuale sicura (criteri di replica password): correggere gli input (chiave, concatenamento in testo normale modalità, IV) e l'output di testo crittografato con sovraccaricare probabilità saranno distinto da qualsiasi altra crittografia a blocchi simmetriche algoritmo ha gli stessi input. Analogamente, qualsiasi funzione di hash con chiave ragionevole è anche una funzione pseudocasuale sicura PRF () e dato un set di input predefinito relativo output fu estremamente saranno distinto da qualsiasi altra funzione di hash con chiave.

Utilizziamo questo concetto di avanzata Criteri di replica password e PRFs per creare un'intestazione del contesto. Questa intestazione del contesto funge essenzialmente un'identificazione personale stabile tramite gli algoritmi in uso per qualsiasi operazione, e offre l'agilità di crittografia necessita per il sistema di protezione dati. Questa intestazione è riproducibile e viene usata in un secondo momento come parte del [processo di derivazione di sottochiave](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). Esistono due modi diversi per compilare l'intestazione del contesto in base la modalità di funzionamento degli algoritmi sottostanti.

## <a name="cbc-mode-encryption--hmac-authentication"></a>Crittografia in modalità CBC + HMAC autenticazione

<a name="data-protection-implementation-context-headers-cbc-components"></a>

L'intestazione del contesto include i componenti seguenti:

* [16 bit] Il valore 00 00, ovvero un marcatore vale a dire "crittografia CBC + autenticazione HMAC".

* [32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo di crittografia a blocchi simmetriche.

* [32 bit] Dimensione del blocco (in byte, big-endian) dell'algoritmo di crittografia a blocchi simmetriche.

* [32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo HMAC. (Attualmente la dimensione della chiave corrisponde sempre le dimensioni del digest.)

* [32 bit] Dimensione del digest (in byte, big-endian) dell'algoritmo HMAC.

* EncCBC (K_E, IV, ""), che rappresenta l'output dell'algoritmo di crittografia a blocchi simmetriche dato un input di stringa vuota e dove vettore di Inizializzazione è un vettore interamente da zeri. La costruzione di K_E è descritta di seguito.

* MAC (K_H, ""), ovvero l'output dell'algoritmo HMAC dato un input di stringa vuota. La costruzione di K_H è descritta di seguito.

In teoria, è possibile passare tutti zero vettori per K_E e K_H. Tuttavia, si vuole evitare la situazione in cui l'algoritmo sottostante verifica l'esistenza delle chiavi vulnerabili prima di eseguire qualsiasi operazione (in particolare DES e 3DES), che impedisce l'uso di un modello semplice o ripetibile, ad esempio un vettore tutti zero.

Invece, utilizziamo il KDF SP800 108 NIST in modalità contatore (vedere [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), 5.1 sec.) con una chiave a lunghezza zero, l'etichetta e contesto e HMACSHA512 come il PRF sottostante. Si dedurrà | K_E | + | K_H | byte di output, quindi scomporre il risultato in K_E e K_H autonomamente. Matematica, ciò è rappresentato come indicato di seguito.

( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>Esempio: AES-192-CBC + HMACSHA256

Ad esempio, si consideri il caso in cui l'algoritmo di crittografia a blocchi simmetriche è AES-192-CBC e l'algoritmo di convalida è HMACSHA256. Il sistema genererà l'intestazione del contesto utilizzando la procedura seguente.

In primo luogo, let (K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = ""), dove | K_E | = 192 bit e | K_H | = 256 bit per gli algoritmi specificati. Ciò comporta K_E = 5BB6... 21DD e K_H = A04A... 00A9 nell'esempio seguente:

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

Successivamente, calcolare Enc_CBC (K_E, IV, "") per AES-192-CBC dato IV = 0 * e K_E come illustrato in precedenza.

result := F474B1872B3B53E4721DE19C0841DB6F

Successivamente, calcolare MAC (K_H, "") per HMACSHA256 dato K_H come illustrato in precedenza.

result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

Questo codice produce l'intestazione del contesto completo riportato di seguito:

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

Questa intestazione del contesto è l'identificazione personale della coppia di algoritmo di crittografia autenticata (la crittografia AES-192-CBC + HMACSHA256 convalida). I componenti, come descritto [sopra](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) sono:

* il marcatore (00 00)

* il blocco lunghezza chiave di crittografia (00 00 00 18)

* la dimensione del blocco crittografato blocco (00 00 00 10)

* la lunghezza della chiave HMAC (00 00 00 20)

* la dimensione del digest HMAC (00 00 00 20)

* la crittografia a blocchi output Criteri replica password (F4 74 - DB 6F) e

* l'output di HMAC PRF (79 D4 - fine).

> [!NOTE]
> La crittografia in modalità CBC + HMAC intestazione del contesto di autenticazione è creata allo stesso modo indipendentemente dal fatto che le implementazioni di algoritmi sono fornite da CNG di Windows o da tipi SymmetricAlgorithm e KeyedHashAlgorithm gestiti. In questo modo le applicazioni in esecuzione su sistemi operativi diversi da generare in modo affidabile l'intestazione del contesto stesso anche se le implementazioni degli algoritmi sono diversi tra i sistemi operativi. (In pratica, il KeyedHashAlgorithm non deve essere un HMAC appropriato. Può essere qualsiasi tipo di algoritmo hash con chiave.)

### <a name="example-3des-192-cbc--hmacsha1"></a>Esempio: 3DES-192-CBC + HMACSHA1

In primo luogo, let (K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = ""), dove | K_E | = 192 bit e | K_H | = 160 bit per gli algoritmi specificati. Ciò comporta K_E = A219... E2BB e K_H = DC4A... B464 nell'esempio seguente:

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

Successivamente, calcolare Enc_CBC (K_E, IV, "") per 3DES-192-CBC dato IV = 0 * e K_E come illustrato in precedenza.

risultato: = ABB100F81E53E10E

Successivamente, calcolare MAC (K_H, "") per HMACSHA1 dato K_H come illustrato in precedenza.

result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

Questa operazione produce l'intestazione del contesto completo che è un'identificazione personale dell'autenticato coppia di algoritmo encryption (crittografia 3DES-192-CBC + HMACSHA1 convalida), i illustrato di seguito:

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

I componenti si suddividono come indicato di seguito:

* il marcatore (00 00)

* il blocco lunghezza chiave di crittografia (00 00 00 18)

* la dimensione del blocco crittografato blocco (00 00 00 08)

* la lunghezza della chiave HMAC (00 00 00 14)

* la dimensione del digest HMAC (00 00 00 14)

* la crittografia a blocchi output Criteri replica password (B1 AB - 0E E1) e

* l'output di HMAC PRF (76 EB - fine).

## <a name="galoiscounter-mode-encryption--authentication"></a>Crittografia in modalità/contatore Galois + autenticazione

L'intestazione del contesto include i componenti seguenti:

* [16 bit] Il valore 00 01, ovvero un marcatore vale a dire "crittografia GCM + autenticazione".

* [32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo di crittografia a blocchi simmetriche.

* [32 bit] Le dimensioni nonce (in byte, big-endian) utilizzate durante le operazioni di crittografia autenticata. (Per il nostro sistema, questo problema è risolto occupando nonce = 96 bit.)

* [32 bit] Dimensione del blocco (in byte, big-endian) dell'algoritmo di crittografia a blocchi simmetriche. (Per GCM, questo problema è risolto alla dimensione del blocco = 128 bit.)

* [32 bit] Autenticazione tag dimensioni (in byte, big-endian) generata dalla funzione di crittografia autenticata. (Per il nostro sistema, questo problema è risolto occupando tag = 128 bit.)

* [128 bit] Il tag di Enc_GCM (K_E, nonce, ""), che rappresenta l'output dell'algoritmo di crittografia a blocchi simmetriche dato un input di stringa vuota e dove nonce è un vettore di tutti zero 96 bit.

K_E viene ottenuto utilizzando lo stesso meccanismo come la crittografia CBC + scenario di autenticazione di HMAC. Tuttavia, poiché non esiste alcun K_H nel gioco, essenzialmente abbiamo | K_H | = 0, l'algoritmo o comprimere per il sotto forma.

K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-256-gcm"></a>Esempio: AES-256-GCM

Prima di tutto consentire K_E = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = ""), dove | K_E | = 256 bit.

K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

Successivamente, calcolare il tag di autenticazione di Enc_GCM (K_E, nonce, "") per AES-256-GCM dato nonce = 096 e K_E come illustrato in precedenza.

result := E7DCCE66DF855A323A6BB7BD7A59BE45

Questo codice produce l'intestazione del contesto completo riportato di seguito:

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

I componenti si suddividono come indicato di seguito:

* il marcatore (00 01)

* il blocco lunghezza chiave di crittografia (00 00 00 20)

* le dimensioni nonce (00 00 00 0 C)

* la dimensione del blocco crittografato blocco (00 00 00 10)

* la dimensione del tag di autenticazione (00 00 00 10) e

* il tag di autenticazione di eseguire la crittografia a blocchi (controller di dominio E7 - fine).
