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
ms.openlocfilehash: 5688ff2c36907231f88d45cef4ae1b1c60ab44ab
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2017
---
# <a name="context-headers"></a><span data-ttu-id="24597-103">Intestazioni di contesto</span><span class="sxs-lookup"><span data-stu-id="24597-103">Context headers</span></span>

<a name=data-protection-implementation-context-headers></a>

## <a name="background-and-theory"></a><span data-ttu-id="24597-104">Sfondo e la teoria</span><span class="sxs-lookup"><span data-stu-id="24597-104">Background and theory</span></span>

<span data-ttu-id="24597-105">Nel sistema di protezione dati, una "chiave" significa che un oggetto che può fornire servizi di crittografia di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="24597-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="24597-106">Ogni chiave è identificato da un id univoco (GUID) e che comporta algoritmiche informazioni e materiale entropic.</span><span class="sxs-lookup"><span data-stu-id="24597-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="24597-107">Lo scopo è che ogni chiave trasportare entropia univoca, ma il sistema non possa imporre che ed è anche necessario tenere conto per gli sviluppatori che potrebbero modificare la gestione delle chiavi manualmente modificando le informazioni di una chiave esistente nell'anello chiave algoritmiche.</span><span class="sxs-lookup"><span data-stu-id="24597-107">It is intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="24597-108">Per ottenere i requisiti di sicurezza specificati questi casi il sistema di protezione dati è un concetto di [agilità di crittografia](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), che consente in modo sicuro per un singolo valore entropic attraverso più algoritmi di crittografia.</span><span class="sxs-lookup"><span data-stu-id="24597-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="24597-109">La maggior parte dei sistemi che supportano l'agilità di crittografia tale scopo, incluse alcune informazioni di identificazione dell'algoritmo all'interno del payload.</span><span class="sxs-lookup"><span data-stu-id="24597-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="24597-110">OID dell'algoritmo è in genere un buon candidato per l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="24597-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="24597-111">Tuttavia, si è verificato un problema è che esistono più modi per specificare lo stesso algoritmo: "AES" (CNG) e di Aes, AesManaged, AesCryptoServiceProvider, AesCng e RijndaelManaged (assegnati parametri specifici) gestito classi sono tutte effettivamente cosa ed è necessario mantenere un mapping di tutti questi all'OID corretto.</span><span class="sxs-lookup"><span data-stu-id="24597-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="24597-112">Se uno sviluppatore desidera fornire un algoritmo personalizzato (o anche un'altra implementazione di AES), dovranno essere segnalare all'OID.</span><span class="sxs-lookup"><span data-stu-id="24597-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="24597-113">Questo passaggio di registrazione supplementare rende particolarmente faticosa configurazione di sistema.</span><span class="sxs-lookup"><span data-stu-id="24597-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="24597-114">Eseguire di nuovo, si è deciso che si stava per raggiungere il problema dalla direzione errata.</span><span class="sxs-lookup"><span data-stu-id="24597-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="24597-115">Un identificatore di oggetto viene identificato l'algoritmo, ma è indifferente effettivamente su questo argomento.</span><span class="sxs-lookup"><span data-stu-id="24597-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="24597-116">Se è necessario utilizzare un singolo valore entropic in modo sicuro in due diversi algoritmi, non è necessario a Microsoft per sapere quali effettivamente sono gli algoritmi.</span><span class="sxs-lookup"><span data-stu-id="24597-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="24597-117">Ciò che effettivamente interessante è il comportamento.</span><span class="sxs-lookup"><span data-stu-id="24597-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="24597-118">Qualsiasi algoritmo di crittografia simmetrico ragionevole è anche una permutazione pseudocasuale sicura (PRP): correggere gli input (chiave, il concatenamento di testo normale modalità, IV) e l'output di testo crittografato con una probabilità di sovraccaricare sarà diverso da quello di qualsiasi altro tipo di crittografia simmetrico algoritmo ha gli stessi input.</span><span class="sxs-lookup"><span data-stu-id="24597-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="24597-119">Analogamente, qualsiasi funzione hash con chiave ragionevole è anche una funzione di pseudocasuale sicura (PRF) e fornito un set fisso di input relativo output estremamente sarà diverso da quello di qualsiasi altra funzione hash con chiave.</span><span class="sxs-lookup"><span data-stu-id="24597-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="24597-120">Questo concetto di sicuro PRPs e PRFs è usare per creare un'intestazione del contesto.</span><span class="sxs-lookup"><span data-stu-id="24597-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="24597-121">Questa intestazione del contesto funge essenzialmente da un'identificazione personale del stabile tramite gli algoritmi in uso per qualsiasi operazione, e fornisce la flessibilità di crittografia necessita per il sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="24597-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="24597-122">Questa intestazione è riproducibile e viene utilizzata in un secondo momento come parte di [sottochiave processo derivazione](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="24597-122">This header is reproducible and is used later as part of the [subkey derivation process](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="24597-123">Esistono due modi diversi per compilare l'intestazione del contesto a seconda di modalità di funzionamento degli algoritmi sottostanti.</span><span class="sxs-lookup"><span data-stu-id="24597-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="24597-124">La crittografia in modalità CBC + autenticazione HMAC</span><span class="sxs-lookup"><span data-stu-id="24597-124">CBC-mode encryption + HMAC authentication</span></span>

<a name=data-protection-implementation-context-headers-cbc-components></a>

<span data-ttu-id="24597-125">L'intestazione del contesto è costituito dai componenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="24597-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="24597-126">[16 bit] Il valore 00 00, che è un indicatore vale a dire "crittografia CBC + autenticazione HMAC".</span><span class="sxs-lookup"><span data-stu-id="24597-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="24597-127">[32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo di crittografia simmetrico.</span><span class="sxs-lookup"><span data-stu-id="24597-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="24597-128">[32 bit] Dimensione del blocco (in byte, big-endian) dell'algoritmo di crittografia simmetrico.</span><span class="sxs-lookup"><span data-stu-id="24597-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="24597-129">[32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo HMAC.</span><span class="sxs-lookup"><span data-stu-id="24597-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="24597-130">(Attualmente la dimensione della chiave corrisponde sempre le dimensioni del digest.)</span><span class="sxs-lookup"><span data-stu-id="24597-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="24597-131">[32 bit] Le dimensioni del digest (in byte, big-endian) dell'algoritmo HMAC.</span><span class="sxs-lookup"><span data-stu-id="24597-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="24597-132">EncCBC (K_E, IV, ""), che è l'output dell'algoritmo di crittografia simmetrico, dato un input di stringa vuota e dove vettore di Inizializzazione è un vettore tutti zero.</span><span class="sxs-lookup"><span data-stu-id="24597-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="24597-133">La costruzione di K_E è descritta di seguito.</span><span class="sxs-lookup"><span data-stu-id="24597-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="24597-134">MAC (K_H, ""), che è l'output dell'algoritmo HMAC dato un input di stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="24597-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="24597-135">La costruzione di K_H è descritta di seguito.</span><span class="sxs-lookup"><span data-stu-id="24597-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="24597-136">In teoria è possibile passare tutti zero vettori per K_E e K_H.</span><span class="sxs-lookup"><span data-stu-id="24597-136">Ideally we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="24597-137">Tuttavia, si vuole evitare questa situazione in cui l'algoritmo sottostante verifica l'esistenza delle chiavi vulnerabili prima di eseguire le operazioni (in particolare DES e 3DES), che impedisce l'uso di un modello semplice o repeatable come un vettore tutti zero.</span><span class="sxs-lookup"><span data-stu-id="24597-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="24597-138">È usare invece l'utilizzo di SP800-108 NIST in modalità di contatore (vedere [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), sec. 5.1) con una chiave di lunghezza zero, l'etichetta e contesto e HMACSHA512 come il PRF sottostante.</span><span class="sxs-lookup"><span data-stu-id="24597-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="24597-139">Abbiamo derivare | K_E | + | K_H | byte di output, quindi scomporre il risultato in K_E e K_H autonomamente.</span><span class="sxs-lookup"><span data-stu-id="24597-139">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="24597-140">Matematicamente, rappresentati come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="24597-140">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="24597-141">(K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = "")</span><span class="sxs-lookup"><span data-stu-id="24597-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="24597-142">Esempio: AES-192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="24597-142">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="24597-143">Ad esempio, considerare il caso in cui l'algoritmo di crittografia simmetrico è AES-192-CBC e l'algoritmo di convalida è HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="24597-143">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="24597-144">Il sistema genererà l'intestazione di contesto utilizzando la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="24597-144">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="24597-145">In primo luogo, let (K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = ""), dove | K_E | = 192 bit e | K_H | = 256 bit per gli algoritmi specificati.</span><span class="sxs-lookup"><span data-stu-id="24597-145">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="24597-146">Questo porta K_E = 5BB6... 21DD e K_H = A04A... 00A9 nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="24597-146">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
   61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
   49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
   B7 92 3D BF 59 90 00 A9
   ```

<span data-ttu-id="24597-147">Successivamente, calcolare Enc_CBC (K_E, IV, "") per AES-192-CBC dato IV = 0 * e K_E come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="24597-147">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="24597-148">risultato: = F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="24597-148">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="24597-149">Successivamente, calcolo MAC (K_H, "") per HMACSHA256 dato K_H come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="24597-149">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="24597-150">risultato: = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span><span class="sxs-lookup"><span data-stu-id="24597-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="24597-151">Viene prodotto l'intestazione del contesto completo riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="24597-151">This produces the full context header below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
   00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
   DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
   8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
   22 0C
   ```

<span data-ttu-id="24597-152">Questa intestazione del contesto è l'identificazione personale della coppia di algoritmo di crittografia autenticato (la crittografia AES-192-CBC + HMACSHA256 convalida).</span><span class="sxs-lookup"><span data-stu-id="24597-152">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="24597-153">I componenti, come descritto [sopra](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) sono:</span><span class="sxs-lookup"><span data-stu-id="24597-153">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="24597-154">il marcatore (00 00)</span><span class="sxs-lookup"><span data-stu-id="24597-154">the marker (00 00)</span></span>

* <span data-ttu-id="24597-155">il blocco lunghezza chiave di crittografia (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="24597-155">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="24597-156">la dimensione del blocco di blocco crittografato (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="24597-156">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="24597-157">la lunghezza della chiave HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="24597-157">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="24597-158">le dimensioni di digest HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="24597-158">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="24597-159">la crittografia a blocchi output Criteri replica password (F4 74 - DB 6F) e</span><span class="sxs-lookup"><span data-stu-id="24597-159">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="24597-160">l'output di HMAC PRF (79 D4 - fine).</span><span class="sxs-lookup"><span data-stu-id="24597-160">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="24597-161">La crittografia in modalità CBC + HMAC intestazione del contesto di autenticazione è create allo stesso modo indipendentemente dal fatto che le implementazioni di algoritmi fornite da CNG di Windows o da tipi gestiti SymmetricAlgorithm e KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="24597-161">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="24597-162">In questo modo le applicazioni in esecuzione in sistemi operativi diversi generare in modo affidabile l'intestazione del contesto stesso anche se le implementazioni degli algoritmi differiscono tra i sistemi operativi.</span><span class="sxs-lookup"><span data-stu-id="24597-162">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="24597-163">(In pratica, il KeyedHashAlgorithm non deve essere un codice HMAC corretto.</span><span class="sxs-lookup"><span data-stu-id="24597-163">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="24597-164">Può essere qualsiasi tipo di algoritmo hash con chiave.)</span><span class="sxs-lookup"><span data-stu-id="24597-164">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="24597-165">Esempio: 3DES-192-CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="24597-165">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="24597-166">In primo luogo, let (K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = ""), dove | K_E | = 192 bit e | K_H | = 160 bit per gli algoritmi specificati.</span><span class="sxs-lookup"><span data-stu-id="24597-166">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="24597-167">Questo porta K_E = A219... E2BB e K_H = DC4A... B464 nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="24597-167">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
   61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
   D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
   ```

<span data-ttu-id="24597-168">Successivamente, calcolare Enc_CBC (K_E, IV, "") per 3DES-192-CBC dato IV = 0 * e K_E come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="24597-168">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="24597-169">risultato: = ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="24597-169">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="24597-170">Successivamente, calcolo MAC (K_H, "") per HMACSHA1 dato K_H come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="24597-170">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="24597-171">risultato: = 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="24597-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="24597-172">Viene prodotto l'intestazione del contesto completo che è un'identificazione personale dell'autenticato algoritmo coppia di crittografia (crittografia 3DES-192-CBC + convalida HMACSHA1), illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="24597-172">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
   00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
   03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
   ```

<span data-ttu-id="24597-173">I componenti si suddividono come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="24597-173">The components break down as follows:</span></span>

* <span data-ttu-id="24597-174">il marcatore (00 00)</span><span class="sxs-lookup"><span data-stu-id="24597-174">the marker (00 00)</span></span>

* <span data-ttu-id="24597-175">il blocco lunghezza chiave di crittografia (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="24597-175">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="24597-176">la dimensione del blocco di blocco crittografato (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="24597-176">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="24597-177">la lunghezza della chiave HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="24597-177">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="24597-178">le dimensioni di digest HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="24597-178">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="24597-179">la crittografia a blocchi output Criteri replica password (B1 AB - E1 0E) e</span><span class="sxs-lookup"><span data-stu-id="24597-179">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="24597-180">l'output di HMAC PRF (76 EB - fine).</span><span class="sxs-lookup"><span data-stu-id="24597-180">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="24597-181">La crittografia della modalità Galois/contatore + autenticazione</span><span class="sxs-lookup"><span data-stu-id="24597-181">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="24597-182">L'intestazione del contesto è costituito dai componenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="24597-182">The context header consists of the following components:</span></span>

* <span data-ttu-id="24597-183">[16 bit] Il valore 00 01, ovvero un marcatore vale a dire "crittografia GCM + autenticazione".</span><span class="sxs-lookup"><span data-stu-id="24597-183">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="24597-184">[32 bit] La lunghezza della chiave (in byte, big-endian) dell'algoritmo di crittografia simmetrico.</span><span class="sxs-lookup"><span data-stu-id="24597-184">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="24597-185">[32 bit] La dimensione nonce (in byte, big-endian) utilizzata durante le operazioni di crittografia autenticata.</span><span class="sxs-lookup"><span data-stu-id="24597-185">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="24597-186">(Per il sistema, questo problema è risolto dimensioni nonce = 96 bit.)</span><span class="sxs-lookup"><span data-stu-id="24597-186">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="24597-187">[32 bit] Dimensione del blocco (in byte, big-endian) dell'algoritmo di crittografia simmetrico.</span><span class="sxs-lookup"><span data-stu-id="24597-187">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="24597-188">(Per GCM, questo è fissato a dimensioni del blocco = 128 bit.)</span><span class="sxs-lookup"><span data-stu-id="24597-188">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="24597-189">[32 bit] Autenticazione tag dimensioni (in byte, big-endian) prodotta dalla funzione di crittografia autenticata.</span><span class="sxs-lookup"><span data-stu-id="24597-189">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="24597-190">(Per il sistema, questo problema è risolto dimensioni tag = 128 bit.)</span><span class="sxs-lookup"><span data-stu-id="24597-190">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="24597-191">[128 bit] Il tag di Enc_GCM (K_E, nonce, ""), che è l'output dell'algoritmo di crittografia simmetrico, dato un input di stringa vuota e dove nonce è un vettore di bit 96 tutti zero.</span><span class="sxs-lookup"><span data-stu-id="24597-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="24597-192">K_E viene ottenuto utilizzando lo stesso meccanismo come la crittografia CBC + scenario di autenticazione HMAC.</span><span class="sxs-lookup"><span data-stu-id="24597-192">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="24597-193">Tuttavia, poiché non esiste alcun K_H in play qui, essenzialmente abbiamo | K_H | = 0, e consente di comprimere l'algoritmo per il form seguente.</span><span class="sxs-lookup"><span data-stu-id="24597-193">However, since there is no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="24597-194">K_E = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", contesto = "")</span><span class="sxs-lookup"><span data-stu-id="24597-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="24597-195">Esempio: AES-256-GCM</span><span class="sxs-lookup"><span data-stu-id="24597-195">Example: AES-256-GCM</span></span>

<span data-ttu-id="24597-196">In primo luogo, let K_E = SP800_108_CTR (prf = HMACSHA512, chiave = "", label = "", contesto = ""), dove | K_E | = 256 bit.</span><span class="sxs-lookup"><span data-stu-id="24597-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="24597-197">K_E: = 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="24597-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="24597-198">Successivamente, calcolare il tag di autenticazione di Enc_GCM (K_E, nonce, "") per AES-256-GCM specificato parametro nonce = 096 e K_E come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="24597-198">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="24597-199">risultato: = E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="24597-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="24597-200">Viene prodotto l'intestazione del contesto completo riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="24597-200">This produces the full context header below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
   00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
   BE 45
   ```

<span data-ttu-id="24597-201">I componenti si suddividono come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="24597-201">The components break down as follows:</span></span>

   * <span data-ttu-id="24597-202">il marcatore (00 01)</span><span class="sxs-lookup"><span data-stu-id="24597-202">the marker (00 01)</span></span>

   * <span data-ttu-id="24597-203">il blocco lunghezza chiave di crittografia (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="24597-203">the block cipher key length (00 00 00 20)</span></span>

   * <span data-ttu-id="24597-204">le dimensioni del parametro nonce (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="24597-204">the nonce size (00 00 00 0C)</span></span>

   * <span data-ttu-id="24597-205">la dimensione del blocco di blocco crittografato (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="24597-205">the block cipher block size (00 00 00 10)</span></span>

   * <span data-ttu-id="24597-206">le dimensioni del tag di autenticazione (00 00 00 10) e</span><span class="sxs-lookup"><span data-stu-id="24597-206">the authentication tag size (00 00 00 10) and</span></span>

   * <span data-ttu-id="24597-207">il tag di autenticazione di eseguire la crittografia a blocchi (controller di dominio E7 - fine).</span><span class="sxs-lookup"><span data-stu-id="24597-207">the authentication tag from running the block cipher (E7 DC - end).</span></span>
