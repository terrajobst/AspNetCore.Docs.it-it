---
title: Formato di archiviazione chiavi
author: tdykstra
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e8996478-f7bf-4b58-bab4-7fdb5d8556c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: e761eaa406a9691e3fa36881d42c1a0c1bd8a206
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="key-storage-format"></a><span data-ttu-id="063a0-103">Formato di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="063a0-103">Key Storage Format</span></span>

<a name=data-protection-implementation-key-storage-format></a>

<span data-ttu-id="063a0-104">Gli oggetti vengono archiviati nel resto nella rappresentazione XML.</span><span class="sxs-lookup"><span data-stu-id="063a0-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="063a0-105">La directory predefinita per l'archiviazione delle chiavi è % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="063a0-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="063a0-106">Il \<chiave > elemento</span><span class="sxs-lookup"><span data-stu-id="063a0-106">The \<key> element</span></span>

<span data-ttu-id="063a0-107">Chiavi esistono come oggetti di livello superiore nel repository di chiave.</span><span class="sxs-lookup"><span data-stu-id="063a0-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="063a0-108">Per convenzione, le chiavi hanno il nome del file **chiave a {guid}. XML**, dove {guid} è l'id della chiave.</span><span class="sxs-lookup"><span data-stu-id="063a0-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="063a0-109">Ognuno di questi file contiene una chiave singola.</span><span class="sxs-lookup"><span data-stu-id="063a0-109">Each such file contains a single key.</span></span> <span data-ttu-id="063a0-110">Il formato del file è come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="063a0-110">The format of the file is as follows.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

<span data-ttu-id="063a0-111">Il \<chiave > elemento contiene i seguenti attributi e gli elementi figlio:</span><span class="sxs-lookup"><span data-stu-id="063a0-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="063a0-112">Id della chiave.</span><span class="sxs-lookup"><span data-stu-id="063a0-112">The key id.</span></span> <span data-ttu-id="063a0-113">Questo valore viene trattato come autorevoli; il nome del file è semplicemente un nicety leggibile.</span><span class="sxs-lookup"><span data-stu-id="063a0-113">This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="063a0-114">La versione di \<chiave > elemento, attualmente è fissato a 1.</span><span class="sxs-lookup"><span data-stu-id="063a0-114">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="063a0-115">Data di creazione, attivazione e la scadenza della chiave.</span><span class="sxs-lookup"><span data-stu-id="063a0-115">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="063a0-116">Oggetto \<descrittore > elemento che contiene informazioni sull'implementazione di crittografia autenticata contenuto all'interno di questa chiave.</span><span class="sxs-lookup"><span data-stu-id="063a0-116">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="063a0-117">Nell'esempio precedente, l'id della chiave è {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, è stato creato e attivato 19 marzo 2015, e ha una durata di 90 giorni.</span><span class="sxs-lookup"><span data-stu-id="063a0-117">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="063a0-118">(In alcuni casi la data di attivazione potrebbe essere leggermente prima della data di creazione come nel seguente esempio.</span><span class="sxs-lookup"><span data-stu-id="063a0-118">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="063a0-119">Ciò è dovuto a un nit nel funzionamento le API e non provoca problemi in pratica.)</span><span class="sxs-lookup"><span data-stu-id="063a0-119">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="063a0-120">Il \<descrittore > elemento</span><span class="sxs-lookup"><span data-stu-id="063a0-120">The \<descriptor> element</span></span>

<span data-ttu-id="063a0-121">Esterna \<descrittore > elemento contiene un deserializerType di attributo, ovvero il nome completo dell'assembly di un tipo che implementa IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="063a0-121">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="063a0-122">Questo tipo è responsabile della lettura interna \<descrittore > elemento e per l'analisi delle informazioni contenute all'interno.</span><span class="sxs-lookup"><span data-stu-id="063a0-122">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="063a0-123">Il formato specifico del \<descrittore > elemento dipende dall'implementazione del componente di crittografia autenticata incapsulata dalla chiave e ogni tipo deserializzatore prevede un formato leggermente diverso per questo oggetto.</span><span class="sxs-lookup"><span data-stu-id="063a0-123">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="063a0-124">In generale, tuttavia, questo elemento conterrà informazioni algoritmiche (i nomi di tipi, gli identificatori di oggetto, o simile) e il materiale della chiave privato.</span><span class="sxs-lookup"><span data-stu-id="063a0-124">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="063a0-125">Nell'esempio precedente, il descrittore specifica che questa chiave esegue il wrapping di crittografia AES-256-CBC + convalida HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="063a0-125">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="063a0-126">Il \<encryptedSecret > elemento</span><span class="sxs-lookup"><span data-stu-id="063a0-126">The \<encryptedSecret> element</span></span>

<span data-ttu-id="063a0-127">Un <encryptedSecret> elemento che contiene la forma crittografata del materiale della chiave privata può essere presente se [è abilitata la crittografia dei segreti inattivi](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="063a0-127">An <encryptedSecret> element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span></span> <span data-ttu-id="063a0-128">L'attributo decryptorType sarà il nome completo dell'assembly di un tipo che implementa IXmlDecryptor.</span><span class="sxs-lookup"><span data-stu-id="063a0-128">The attribute decryptorType will be the assembly-qualified name of a type which implements IXmlDecryptor.</span></span> <span data-ttu-id="063a0-129">Questo tipo è responsabile della lettura interna <encryptedKey> elemento e la decrittografia per recuperare il testo non crittografato originale.</span><span class="sxs-lookup"><span data-stu-id="063a0-129">This type is responsible for reading the inner <encryptedKey> element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="063a0-130">Come con \<descrittore >, il formato specifico del <encryptedSecret> elemento dipende dal meccanismo di crittografia a riposo in uso.</span><span class="sxs-lookup"><span data-stu-id="063a0-130">As with \<descriptor>, the particular format of the <encryptedSecret> element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="063a0-131">Nell'esempio precedente, la chiave master viene crittografata utilizzando DPAPI di Windows per il commento.</span><span class="sxs-lookup"><span data-stu-id="063a0-131">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="063a0-132">Il \<revoca > elemento</span><span class="sxs-lookup"><span data-stu-id="063a0-132">The \<revocation> element</span></span>

<span data-ttu-id="063a0-133">Revoca esiste come oggetti di livello superiore nel repository di chiave.</span><span class="sxs-lookup"><span data-stu-id="063a0-133">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="063a0-134">Per convenzione revoca presentano il nome del file **revoca-{timestamp}. XML** (per la revoca di tutte le chiavi prima di una data specifica) o **revoca-{guid}. XML** (per la revoca di una chiave specifica).</span><span class="sxs-lookup"><span data-stu-id="063a0-134">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="063a0-135">Ogni file contiene un singolo \<revoca > elemento.</span><span class="sxs-lookup"><span data-stu-id="063a0-135">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="063a0-136">Per le decisioni di singole chiavi, il contenuto del file saranno come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="063a0-136">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="063a0-137">In questo caso, viene revocata solo la chiave specificata.</span><span class="sxs-lookup"><span data-stu-id="063a0-137">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="063a0-138">Se l'id chiave è "*", tuttavia, come nell'esempio seguente vengono revocate tutte le chiavi la cui data di creazione è precedente alla data di revoche di certificati specificato.</span><span class="sxs-lookup"><span data-stu-id="063a0-138">If the key id is "*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="063a0-139">Il \<motivo > elemento non viene mai letto dal sistema.</span><span class="sxs-lookup"><span data-stu-id="063a0-139">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="063a0-140">È semplicemente una posizione comoda in cui archiviare un motivo della revoca leggibile.</span><span class="sxs-lookup"><span data-stu-id="063a0-140">It is simply a convenient place to store a human-readable reason for revocation.</span></span>
