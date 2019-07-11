---
title: Intestazioni di contesto in ASP.NET Core
author: rick-anderson
description: Scopri i dettagli di implementazione delle intestazioni di contesto di protezione dei dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 518423f5df93924d3df144994e4beb1755cd0bfc
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814019"
---
# <a name="context-headers-in-aspnet-core"></a><span data-ttu-id="4fd77-103">Intestazioni di contesto in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4fd77-103">Context headers in ASP.NET Core</span></span>

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a><span data-ttu-id="4fd77-104">In background e la teoria</span><span class="sxs-lookup"><span data-stu-id="4fd77-104">Background and theory</span></span>

<span data-ttu-id="4fd77-105">Nel sistema di protezione dati, una "chiave" significa che un oggetto che può fornire servizi di crittografia di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="4fd77-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="4fd77-106">Ogni chiave è identificata da un id univoco (GUID) e che comporta però algoritmiche informazioni e materiale entropic.</span><span class="sxs-lookup"><span data-stu-id="4fd77-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="4fd77-107">Lo scopo è che ogni chiave trasportare univoco entropia, ma il sistema non può imporre quella e dobbiamo anche per gli sviluppatori che potrebbero modificare il gruppo di chiavi manualmente modificando le informazioni su algoritmi di una chiave esistente nel gruppo di chiavi dell'account.</span><span class="sxs-lookup"><span data-stu-id="4fd77-107">It's intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="4fd77-108">Per ottenere i requisiti di sicurezza assegnati questi casi il sistema di protezione dati è un concetto di [agilità di crittografia](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), consentendo in modo sicuro usando un singolo valore entropic tra più algoritmi di crittografia.</span><span class="sxs-lookup"><span data-stu-id="4fd77-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="4fd77-109">La maggior parte dei sistemi che supportano la agilità di crittografia farlo includendo alcune informazioni di identificazione dell'algoritmo all'interno del payload.</span><span class="sxs-lookup"><span data-stu-id="4fd77-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="4fd77-110">OID dell'algoritmo è in genere un buon candidato per l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="4fd77-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="4fd77-111">Tuttavia, un problema che si è verificato è che esistono diversi modi per specificare lo stesso algoritmo: "AES" (CNG) e di Aes, AesManaged allo standard, AesCryptoServiceProvider, AesCng e RijndaelManaged (determinati parametri specifici) gestito classi sono tutti effettivamente la stessa cosa e dobbiamo mantengono un mapping di tutti questi all'OID corretti.</span><span class="sxs-lookup"><span data-stu-id="4fd77-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="4fd77-112">Se uno sviluppatore desidera fornire un algoritmo personalizzato (o anche un'altra implementazione di AES), si dovrà indicare all'OID.</span><span class="sxs-lookup"><span data-stu-id="4fd77-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="4fd77-113">Questo passaggio di registrazione aggiuntiva rende particolarmente dolorosa configurazione del sistema.</span><span class="sxs-lookup"><span data-stu-id="4fd77-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="4fd77-114">Ritorno al passaggio precedente, abbiamo deciso che si stava per raggiungere il problema dalla direzione errata.</span><span class="sxs-lookup"><span data-stu-id="4fd77-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="4fd77-115">Un identificatore di oggetto viene identificato l'algoritmo, ma non sono effettivamente importanti questo.</span><span class="sxs-lookup"><span data-stu-id="4fd77-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="4fd77-116">Se è necessario usare un singolo valore entropic in modo sicuro in due diversi algoritmi, non è necessario sapere quali effettivamente sono gli algoritmi.</span><span class="sxs-lookup"><span data-stu-id="4fd77-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="4fd77-117">Che cos'è effettivamente necessario occuparsi è il comportamento.</span><span class="sxs-lookup"><span data-stu-id="4fd77-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="4fd77-118">Qualsiasi algoritmo di crittografia a blocchi simmetriche ragionevole è anche una permutazione pseudocasuale sicura (criteri di replica password): correggere gli input (chiave, concatenamento in testo normale modalità, IV) e l'output di testo crittografato con sovraccaricare probabilità saranno distinto da qualsiasi altra crittografia a blocchi simmetriche algoritmo ha gli stessi input.</span><span class="sxs-lookup"><span data-stu-id="4fd77-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="4fd77-119">Analogamente, qualsiasi funzione di hash con chiave ragionevole è anche una funzione pseudocasuale sicura PRF () e dato un set di input predefinito relativo output fu estremamente saranno distinto da qualsiasi altra funzione di hash con chiave.</span><span class="sxs-lookup"><span data-stu-id="4fd77-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="4fd77-120">Utilizziamo questo concetto di avanzata Criteri di replica password e PRFs per creare un'intestazione del contesto.</span><span class="sxs-lookup"><span data-stu-id="4fd77-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="4fd77-121">Questa intestazione del contesto funge essenzialmente un'identificazione personale stabile tramite gli algoritmi in uso per qualsiasi operazione, e offre l'agilità di crittografia necessita per il sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="4fd77-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="4fd77-122">Questa intestazione è riproducibile e viene usata in un secondo momento come parte del [processo di derivazione di sottochiave](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="4fd77-122">This header is reproducible and is used later as part of the [subkey derivation process](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="4fd77-123">Esistono due modi diversi per compilare l'intestazione del contesto in base la modalità di funzionamento degli algoritmi sottostanti.</span><span class="sxs-lookup"><span data-stu-id="4fd77-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="4fd77-124">Crittografia in modalità CBC + HMAC autenticazione</span><span class="sxs-lookup"><span data-stu-id="4fd77-124">CBC-mode encryption + HMAC authentication</span></span>

<a name="data-protection-implementation-context-headers-cbc-components"></a>

<span data-ttu-id="4fd77-125">L'intestazione del contesto include i componenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="4fd77-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="4fd77-126">[16 bit] Il valore 00 00, ovvero un marcatore vale a dire "crittografia CBC + autenticazione HMAC".</span><span class="sxs-lookup"><span data-stu-id="4fd77-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="4fd77-127">[32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo di crittografia a blocchi simmetriche.</span><span class="sxs-lookup"><span data-stu-id="4fd77-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="4fd77-128">[32 bit] Dimensione del blocco (in byte, big-endian) dell'algoritmo di crittografia a blocchi simmetriche.</span><span class="sxs-lookup"><span data-stu-id="4fd77-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="4fd77-129">[32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo HMAC.</span><span class="sxs-lookup"><span data-stu-id="4fd77-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="4fd77-130">(Attualmente la dimensione della chiave corrisponde sempre le dimensioni del digest.)</span><span class="sxs-lookup"><span data-stu-id="4fd77-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="4fd77-131">[32 bit] Dimensione del digest (in byte, big-endian) dell'algoritmo HMAC.</span><span class="sxs-lookup"><span data-stu-id="4fd77-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="4fd77-132">EncCBC (K_E, IV, ""), che rappresenta l'output dell'algoritmo di crittografia a blocchi simmetriche dato un input di stringa vuota e dove vettore di Inizializzazione è un vettore interamente da zeri.</span><span class="sxs-lookup"><span data-stu-id="4fd77-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="4fd77-133">La costruzione di K_E è descritta di seguito.</span><span class="sxs-lookup"><span data-stu-id="4fd77-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="4fd77-134">MAC (K_H, ""), ovvero l'output dell'algoritmo HMAC dato un input di stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="4fd77-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="4fd77-135">La costruzione di K_H è descritta di seguito.</span><span class="sxs-lookup"><span data-stu-id="4fd77-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="4fd77-136">In teoria, è possibile passare tutti zero vettori per K_E e K_H.</span><span class="sxs-lookup"><span data-stu-id="4fd77-136">Ideally, we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="4fd77-137">Tuttavia, si vuole evitare la situazione in cui l'algoritmo sottostante verifica l'esistenza delle chiavi vulnerabili prima di eseguire qualsiasi operazione (in particolare DES e 3DES), che impedisce l'uso di un modello semplice o ripetibile, ad esempio un vettore tutti zero.</span><span class="sxs-lookup"><span data-stu-id="4fd77-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="4fd77-138">Invece, utilizziamo il KDF SP800 108 NIST in modalità contatore (vedere [NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), 5.1 sec.) con una chiave a lunghezza zero, l'etichetta e contesto e HMACSHA512 come il PRF sottostante.</span><span class="sxs-lookup"><span data-stu-id="4fd77-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="4fd77-139">Si dedurrà | K_E | + | K_H | byte di output, quindi scomporre il risultato in K_E e K_H autonomamente.</span><span class="sxs-lookup"><span data-stu-id="4fd77-139">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="4fd77-140">Matematica, ciò è rappresentato come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="4fd77-140">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="4fd77-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span><span class="sxs-lookup"><span data-stu-id="4fd77-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="4fd77-142">Esempio: AES-192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="4fd77-142">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="4fd77-143">Ad esempio, si consideri il caso in cui l'algoritmo di crittografia a blocchi simmetriche è AES-192-CBC e l'algoritmo di convalida è HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="4fd77-143">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="4fd77-144">Il sistema genererà l'intestazione del contesto utilizzando la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="4fd77-144">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="4fd77-145">In primo luogo, let (K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = ""), dove | K_E | = 192 bit e | K_H | = 256 bit per gli algoritmi specificati.</span><span class="sxs-lookup"><span data-stu-id="4fd77-145">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="4fd77-146">Ciò comporta K_E = 5BB6... 21DD e K_H = A04A... 00A9 nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="4fd77-146">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

<span data-ttu-id="4fd77-147">Successivamente, calcolare Enc_CBC (K_E, IV, "") per AES-192-CBC dato IV = 0 \* e K_E come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4fd77-147">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="4fd77-148">result := F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="4fd77-148">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="4fd77-149">Successivamente, calcolare MAC (K_H, "") per HMACSHA256 dato K_H come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4fd77-149">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="4fd77-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span><span class="sxs-lookup"><span data-stu-id="4fd77-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="4fd77-151">Questo codice produce l'intestazione del contesto completo riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4fd77-151">This produces the full context header below:</span></span>

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

<span data-ttu-id="4fd77-152">Questa intestazione del contesto è l'identificazione personale della coppia di algoritmo di crittografia autenticata (la crittografia AES-192-CBC + HMACSHA256 convalida).</span><span class="sxs-lookup"><span data-stu-id="4fd77-152">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="4fd77-153">I componenti, come descritto [sopra](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) sono:</span><span class="sxs-lookup"><span data-stu-id="4fd77-153">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="4fd77-154">il marcatore (00 00)</span><span class="sxs-lookup"><span data-stu-id="4fd77-154">the marker (00 00)</span></span>

* <span data-ttu-id="4fd77-155">il blocco lunghezza chiave di crittografia (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="4fd77-155">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="4fd77-156">la dimensione del blocco crittografato blocco (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="4fd77-156">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="4fd77-157">la lunghezza della chiave HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="4fd77-157">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="4fd77-158">la dimensione del digest HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="4fd77-158">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="4fd77-159">la crittografia a blocchi output Criteri replica password (F4 74 - DB 6F) e</span><span class="sxs-lookup"><span data-stu-id="4fd77-159">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="4fd77-160">l'output di HMAC PRF (79 D4 - fine).</span><span class="sxs-lookup"><span data-stu-id="4fd77-160">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="4fd77-161">La crittografia in modalità CBC + HMAC intestazione del contesto di autenticazione è creata allo stesso modo indipendentemente dal fatto che le implementazioni di algoritmi sono fornite da CNG di Windows o da tipi SymmetricAlgorithm e KeyedHashAlgorithm gestiti.</span><span class="sxs-lookup"><span data-stu-id="4fd77-161">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="4fd77-162">In questo modo le applicazioni in esecuzione su sistemi operativi diversi da generare in modo affidabile l'intestazione del contesto stesso anche se le implementazioni degli algoritmi sono diversi tra i sistemi operativi.</span><span class="sxs-lookup"><span data-stu-id="4fd77-162">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="4fd77-163">(In pratica, il KeyedHashAlgorithm non deve essere un HMAC appropriato.</span><span class="sxs-lookup"><span data-stu-id="4fd77-163">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="4fd77-164">Può essere qualsiasi tipo di algoritmo hash con chiave.)</span><span class="sxs-lookup"><span data-stu-id="4fd77-164">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="4fd77-165">Esempio: 3DES-192-CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="4fd77-165">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="4fd77-166">In primo luogo, let (K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = ""), dove | K_E | = 192 bit e | K_H | = 160 bit per gli algoritmi specificati.</span><span class="sxs-lookup"><span data-stu-id="4fd77-166">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="4fd77-167">Ciò comporta K_E = A219... E2BB e K_H = DC4A... B464 nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="4fd77-167">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

<span data-ttu-id="4fd77-168">Successivamente, calcolare Enc_CBC (K_E, IV, "") per 3DES-192-CBC dato IV = 0 \* e K_E come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4fd77-168">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="4fd77-169">risultato: = ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="4fd77-169">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="4fd77-170">Successivamente, calcolare MAC (K_H, "") per HMACSHA1 dato K_H come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4fd77-170">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="4fd77-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="4fd77-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="4fd77-172">Questa operazione produce l'intestazione del contesto completo che è un'identificazione personale dell'autenticato coppia di algoritmo encryption (crittografia 3DES-192-CBC + HMACSHA1 convalida), i illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4fd77-172">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

<span data-ttu-id="4fd77-173">I componenti si suddividono come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4fd77-173">The components break down as follows:</span></span>

* <span data-ttu-id="4fd77-174">il marcatore (00 00)</span><span class="sxs-lookup"><span data-stu-id="4fd77-174">the marker (00 00)</span></span>

* <span data-ttu-id="4fd77-175">il blocco lunghezza chiave di crittografia (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="4fd77-175">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="4fd77-176">la dimensione del blocco crittografato blocco (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="4fd77-176">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="4fd77-177">la lunghezza della chiave HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="4fd77-177">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="4fd77-178">la dimensione del digest HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="4fd77-178">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="4fd77-179">la crittografia a blocchi output Criteri replica password (B1 AB - 0E E1) e</span><span class="sxs-lookup"><span data-stu-id="4fd77-179">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="4fd77-180">l'output di HMAC PRF (76 EB - fine).</span><span class="sxs-lookup"><span data-stu-id="4fd77-180">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="4fd77-181">Crittografia in modalità/contatore Galois + autenticazione</span><span class="sxs-lookup"><span data-stu-id="4fd77-181">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="4fd77-182">L'intestazione del contesto include i componenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="4fd77-182">The context header consists of the following components:</span></span>

* <span data-ttu-id="4fd77-183">[16 bit] Il valore 00 01, ovvero un marcatore vale a dire "crittografia GCM + autenticazione".</span><span class="sxs-lookup"><span data-stu-id="4fd77-183">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="4fd77-184">[32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo di crittografia a blocchi simmetriche.</span><span class="sxs-lookup"><span data-stu-id="4fd77-184">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="4fd77-185">[32 bit] Le dimensioni nonce (in byte, big-endian) utilizzate durante le operazioni di crittografia autenticata.</span><span class="sxs-lookup"><span data-stu-id="4fd77-185">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="4fd77-186">(Per il nostro sistema, questo problema è risolto occupando nonce = 96 bit.)</span><span class="sxs-lookup"><span data-stu-id="4fd77-186">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="4fd77-187">[32 bit] Dimensione del blocco (in byte, big-endian) dell'algoritmo di crittografia a blocchi simmetriche.</span><span class="sxs-lookup"><span data-stu-id="4fd77-187">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="4fd77-188">(Per GCM, questo problema è risolto alla dimensione del blocco = 128 bit.)</span><span class="sxs-lookup"><span data-stu-id="4fd77-188">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="4fd77-189">[32 bit] Autenticazione tag dimensioni (in byte, big-endian) generata dalla funzione di crittografia autenticata.</span><span class="sxs-lookup"><span data-stu-id="4fd77-189">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="4fd77-190">(Per il nostro sistema, questo problema è risolto occupando tag = 128 bit.)</span><span class="sxs-lookup"><span data-stu-id="4fd77-190">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="4fd77-191">[128 bit] Il tag di Enc_GCM (K_E, nonce, ""), che rappresenta l'output dell'algoritmo di crittografia a blocchi simmetriche dato un input di stringa vuota e dove nonce è un vettore di tutti zero 96 bit.</span><span class="sxs-lookup"><span data-stu-id="4fd77-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="4fd77-192">K_E viene ottenuto utilizzando lo stesso meccanismo come la crittografia CBC + scenario di autenticazione di HMAC.</span><span class="sxs-lookup"><span data-stu-id="4fd77-192">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="4fd77-193">Tuttavia, poiché non esiste alcun K_H nel gioco, essenzialmente abbiamo | K_H | = 0, l'algoritmo o comprimere per il sotto forma.</span><span class="sxs-lookup"><span data-stu-id="4fd77-193">However, since there's no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="4fd77-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span><span class="sxs-lookup"><span data-stu-id="4fd77-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="4fd77-195">Esempio: AES-256-GCM</span><span class="sxs-lookup"><span data-stu-id="4fd77-195">Example: AES-256-GCM</span></span>

<span data-ttu-id="4fd77-196">Prima di tutto consentire K_E = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = ""), dove | K_E | = 256 bit.</span><span class="sxs-lookup"><span data-stu-id="4fd77-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="4fd77-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="4fd77-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="4fd77-198">Successivamente, calcolare il tag di autenticazione di Enc_GCM (K_E, nonce, "") per AES-256-GCM dato nonce = 096 e K_E come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4fd77-198">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="4fd77-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="4fd77-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="4fd77-200">Questo codice produce l'intestazione del contesto completo riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4fd77-200">This produces the full context header below:</span></span>

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

<span data-ttu-id="4fd77-201">I componenti si suddividono come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="4fd77-201">The components break down as follows:</span></span>

* <span data-ttu-id="4fd77-202">il marcatore (00 01)</span><span class="sxs-lookup"><span data-stu-id="4fd77-202">the marker (00 01)</span></span>

* <span data-ttu-id="4fd77-203">il blocco lunghezza chiave di crittografia (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="4fd77-203">the block cipher key length (00 00 00 20)</span></span>

* <span data-ttu-id="4fd77-204">le dimensioni nonce (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="4fd77-204">the nonce size (00 00 00 0C)</span></span>

* <span data-ttu-id="4fd77-205">la dimensione del blocco crittografato blocco (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="4fd77-205">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="4fd77-206">la dimensione del tag di autenticazione (00 00 00 10) e</span><span class="sxs-lookup"><span data-stu-id="4fd77-206">the authentication tag size (00 00 00 10) and</span></span>

* <span data-ttu-id="4fd77-207">il tag di autenticazione di eseguire la crittografia a blocchi (controller di dominio E7 - fine).</span><span class="sxs-lookup"><span data-stu-id="4fd77-207">the authentication tag from running the block cipher (E7 DC - end).</span></span>
