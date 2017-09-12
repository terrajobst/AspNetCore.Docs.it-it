---
title: Intestazioni di contesto
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d026a58c-67f4-411e-a410-c35f29c2c517
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 7befd983f6a45839868639708ec5cf45bf2df35f
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="context-headers"></a>Intestazioni di contesto

<a name=data-protection-implementation-context-headers></a>

## <a name="background-and-theory"></a>Sfondo e la teoria

Nel sistema di protezione dati, una "chiave" significa che un oggetto che può fornire servizi di crittografia di autenticazione. Ogni chiave è identificato da un id univoco (GUID) e che comporta algoritmiche informazioni e materiale entropic. Lo scopo è che ogni chiave trasportare entropia univoca, ma il sistema non possa imporre che ed è anche necessario tenere conto per gli sviluppatori che potrebbero modificare la gestione delle chiavi manualmente modificando le informazioni di una chiave esistente nell'anello chiave algoritmiche. Per ottenere i requisiti di sicurezza specificati questi casi il sistema di protezione dati è un concetto di [agilità di crittografia](https://www.microsoft.com/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/?from=http%3A%2F%2Fresearch.microsoft.com%2Fapps%2Fpubs%2Fdefault.aspx%3Fid%3D121045), che consente in modo sicuro per un singolo valore entropic attraverso più algoritmi di crittografia.

La maggior parte dei sistemi che supportano l'agilità di crittografia tale scopo, incluse alcune informazioni di identificazione dell'algoritmo all'interno del payload. OID dell'algoritmo è in genere un buon candidato per l'oggetto. Tuttavia, si è verificato un problema è che esistono più modi per specificare lo stesso algoritmo: "AES" (CNG) e di Aes, AesManaged, AesCryptoServiceProvider, AesCng e RijndaelManaged (assegnati parametri specifici) gestito classi sono tutte effettivamente cosa ed è necessario mantenere un mapping di tutti questi all'OID corretto. Se uno sviluppatore desidera fornire un algoritmo personalizzato (o anche un'altra implementazione di AES), dovranno essere segnalare all'OID. Questo passaggio di registrazione supplementare rende particolarmente faticosa configurazione di sistema.

Eseguire di nuovo, si è deciso che si stava per raggiungere il problema dalla direzione errata. Un identificatore di oggetto viene identificato l'algoritmo, ma è indifferente effettivamente su questo argomento. Se è necessario utilizzare un singolo valore entropic in modo sicuro in due diversi algoritmi, non è necessario a Microsoft per sapere quali effettivamente sono gli algoritmi. Ciò che effettivamente interessante è il comportamento. Qualsiasi algoritmo di crittografia simmetrico ragionevole è anche una permutazione pseudocasuale sicura (PRP): correggere gli input (chiave, il concatenamento di testo normale modalità, IV) e l'output di testo crittografato con una probabilità di sovraccaricare sarà diverso da quello di qualsiasi altro tipo di crittografia simmetrico algoritmo ha gli stessi input. Analogamente, qualsiasi funzione hash con chiave ragionevole è anche una funzione di pseudocasuale sicura (PRF) e fornito un set fisso di input relativo output estremamente sarà diverso da quello di qualsiasi altra funzione hash con chiave.

Questo concetto di sicuro PRPs e PRFs è usare per creare un'intestazione del contesto. Questa intestazione del contesto funge essenzialmente da un'identificazione personale del stabile tramite gli algoritmi in uso per qualsiasi operazione, e fornisce la flessibilità di crittografia necessita per il sistema di protezione dati. Questa intestazione è riproducibile e viene utilizzata in un secondo momento come parte di [sottochiave processo derivazione](subkeyderivation.md#data-protection-implementation-subkey-derivation). Esistono due modi diversi per compilare l'intestazione del contesto a seconda di modalità di funzionamento degli algoritmi sottostanti.

## <a name="cbc-mode-encryption--hmac-authentication"></a>La crittografia in modalità CBC + autenticazione HMAC

<a name=data-protection-implementation-context-headers-cbc-components></a>

L'intestazione del contesto è costituito dai componenti seguenti:

* [16 bit] Il valore 00 00, che è un indicatore vale a dire "crittografia CBC + autenticazione HMAC".

* [32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo di crittografia simmetrico.

* [32 bit] Dimensione del blocco (in byte, big-endian) dell'algoritmo di crittografia simmetrico.

* [32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo HMAC. (Attualmente la dimensione della chiave corrisponde sempre le dimensioni del digest.)

* [32 bit] Le dimensioni del digest (in byte, big-endian) dell'algoritmo HMAC.

* EncCBC (K_E, IV, ""), che è l'output dell'algoritmo di crittografia simmetrico, dato un input di stringa vuota e dove vettore di Inizializzazione è un vettore tutti zero. La costruzione di K_E è descritta di seguito.

* MAC (K_H, ""), che è l'output dell'algoritmo HMAC dato un input di stringa vuota. La costruzione di K_H è descritta di seguito.

In teoria è possibile passare tutti zero vettori per K_E e K_H. Tuttavia, si vuole evitare questa situazione in cui l'algoritmo sottostante verifica l'esistenza delle chiavi vulnerabili prima di eseguire le operazioni (in particolare DES e 3DES), che impedisce l'uso di un modello semplice o repeatable come un vettore tutti zero.

È usare invece l'utilizzo di SP800-108 NIST in modalità di contatore (vedere [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), sec. 5.1) con una chiave di lunghezza zero, l'etichetta e contesto e HMACSHA512 come il PRF sottostante. Abbiamo derivare | K_E | + | K_H | byte di output, quindi scomporre il risultato in K_E e K_H autonomamente. Matematicamente, rappresentati come indicato di seguito.

(K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>Esempio: AES-192-CBC + HMACSHA256

Ad esempio, considerare il caso in cui l'algoritmo di crittografia simmetrico è AES-192-CBC e l'algoritmo di convalida è HMACSHA256. Il sistema genererà l'intestazione di contesto utilizzando la procedura seguente.

In primo luogo, let (K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = ""), dove | K_E | = 192 bit e | K_H | = 256 bit per gli algoritmi specificati. Questo porta K_E = 5BB6... 21DD e K_H = A04A... 00A9 nell'esempio seguente:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
   61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
   49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
   B7 92 3D BF 59 90 00 A9
   ```

Successivamente, calcolare Enc_CBC (K_E, IV, "") per AES-192-CBC dato IV = 0 * e K_E come illustrato in precedenza.

risultato: = F474B1872B3B53E4721DE19C0841DB6F

Successivamente, calcolo MAC (K_H, "") per HMACSHA256 dato K_H come illustrato in precedenza.

risultato: = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

Viene prodotto l'intestazione del contesto completo riportato di seguito:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
   00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
   DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
   8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
   22 0C
   ```

Questa intestazione del contesto è l'identificazione personale della coppia di algoritmo di crittografia autenticato (la crittografia AES-192-CBC + HMACSHA256 convalida). I componenti, come descritto [sopra](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) sono:

* il marcatore (00 00)

* il blocco lunghezza chiave di crittografia (00 00 00 18)

* la dimensione del blocco di blocco crittografato (00 00 00 10)

* la lunghezza della chiave HMAC (00 00 00 20)

* le dimensioni di digest HMAC (00 00 00 20)

* la crittografia a blocchi output Criteri replica password (F4 74 - DB 6F) e

* l'output di HMAC PRF (79 D4 - fine).

> [!NOTE]
> La crittografia in modalità CBC + HMAC intestazione del contesto di autenticazione è create allo stesso modo indipendentemente dal fatto che le implementazioni di algoritmi fornite da CNG di Windows o da tipi gestiti SymmetricAlgorithm e KeyedHashAlgorithm. In questo modo le applicazioni in esecuzione in sistemi operativi diversi generare in modo affidabile l'intestazione del contesto stesso anche se le implementazioni degli algoritmi differiscono tra i sistemi operativi. (In pratica, il KeyedHashAlgorithm non deve essere un codice HMAC corretto. Può essere qualsiasi tipo di algoritmo hash con chiave.)

### <a name="example-3des-192-cbc--hmacsha1"></a>Esempio: 3DES-192-CBC + HMACSHA1

In primo luogo, let (K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = ""), dove | K_E | = 192 bit e | K_H | = 160 bit per gli algoritmi specificati. Questo porta K_E = A219... E2BB e K_H = DC4A... B464 nell'esempio seguente:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
   61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
   D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
   ```

Successivamente, calcolare Enc_CBC (K_E, IV, "") per 3DES-192-CBC dato IV = 0 * e K_E come illustrato in precedenza.

risultato: = ABB100F81E53E10E

Successivamente, calcolo MAC (K_H, "") per HMACSHA1 dato K_H come illustrato in precedenza.

risultato: = 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

Viene prodotto l'intestazione del contesto completo che è un'identificazione personale dell'autenticato algoritmo coppia di crittografia (crittografia 3DES-192-CBC + convalida HMACSHA1), illustrato di seguito:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
   00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
   03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
   ```

I componenti si suddividono come indicato di seguito:

* il marcatore (00 00)

* il blocco lunghezza chiave di crittografia (00 00 00 18)

* la dimensione del blocco di blocco crittografato (00 00 00 08)

* la lunghezza della chiave HMAC (00 00 00 14)

* le dimensioni di digest HMAC (00 00 00 14)

* la crittografia a blocchi output Criteri replica password (B1 AB - E1 0E) e

* l'output di HMAC PRF (76 EB - fine).

## <a name="galoiscounter-mode-encryption--authentication"></a>La crittografia della modalità Galois/contatore + autenticazione

L'intestazione del contesto è costituito dai componenti seguenti:

* [16 bit] Il valore 00 01, ovvero un marcatore vale a dire "crittografia GCM + autenticazione".

* [32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo di crittografia simmetrico.

* [32 bit] La dimensione nonce (in byte, big-endian) utilizzata durante le operazioni di crittografia autenticata. (Per il sistema, questo problema è risolto dimensioni nonce = 96 bit.)

* [32 bit] Dimensione del blocco (in byte, big-endian) dell'algoritmo di crittografia simmetrico. (Per GCM, questo è fissato a dimensioni del blocco = 128 bit.)

* [32 bit] Autenticazione tag dimensioni (in byte, big-endian) prodotta dalla funzione di crittografia autenticata. (Per il sistema, questo problema è risolto dimensioni tag = 128 bit.)

* [128 bit] Il tag di Enc_GCM (K_E, nonce, ""), che è l'output dell'algoritmo di crittografia simmetrico, dato un input di stringa vuota e dove nonce è un vettore di bit 96 tutti zero.

K_E viene ottenuto utilizzando lo stesso meccanismo come la crittografia CBC + scenario di autenticazione HMAC. Tuttavia, poiché non esiste alcun K_H in play qui, essenzialmente abbiamo | K_H | = 0, e consente di comprimere l'algoritmo per il form seguente.

K_E = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = "")

### <a name="example-aes-256-gcm"></a>Esempio: AES-256-GCM

In primo luogo, let K_E = SP800_108_CTR (prf = HMACSHA512, chiave = "", label = "", contesto = ""), dove | K_E | = 256 bit.

K_E: = 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

Successivamente, calcolare il tag di autenticazione di Enc_GCM (K_E, nonce, "") per AES-256-GCM specificato parametro nonce = 096 e K_E come illustrato in precedenza.

risultato: = E7DCCE66DF855A323A6BB7BD7A59BE45

Viene prodotto l'intestazione del contesto completo riportato di seguito:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
   00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
   BE 45
   ```

I componenti si suddividono come indicato di seguito:

   * il marcatore (00 01)

   * il blocco lunghezza chiave di crittografia (00 00 00 20)

   * le dimensioni del parametro nonce (00 00 00 0 C)

   * la dimensione del blocco di blocco crittografato (00 00 00 10)

   * le dimensioni del tag di autenticazione (00 00 00 10) e

   * il tag di autenticazione di eseguire la crittografia a blocchi (controller di dominio E7 - fine).
