---
title: Formato di archiviazione delle chiavi in ASP.NET Core
author: rick-anderson
description: Informazioni sui dettagli di implementazione del formato di archiviazione della chiave di protezione dei dati ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667755"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="8974d-103">Formato di archiviazione delle chiavi in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8974d-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="8974d-104">Gli oggetti vengono archiviati in formato Rest nella rappresentazione XML.</span><span class="sxs-lookup"><span data-stu-id="8974d-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="8974d-105">La directory predefinita per l'archiviazione delle chiavi è%LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="8974d-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="8974d-106">Elemento > chiave \<</span><span class="sxs-lookup"><span data-stu-id="8974d-106">The \<key> element</span></span>

<span data-ttu-id="8974d-107">Le chiavi esistono come oggetti di primo livello nel repository di chiavi.</span><span class="sxs-lookup"><span data-stu-id="8974d-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="8974d-108">Per chiavi convenzione è presente la **chiave FileName-{GUID}. XML**, dove {GUID} è l'ID della chiave.</span><span class="sxs-lookup"><span data-stu-id="8974d-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="8974d-109">Ogni file di questo tipo contiene una singola chiave.</span><span class="sxs-lookup"><span data-stu-id="8974d-109">Each such file contains a single key.</span></span> <span data-ttu-id="8974d-110">Il formato del file è il seguente.</span><span class="sxs-lookup"><span data-stu-id="8974d-110">The format of the file is as follows.</span></span>

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

<span data-ttu-id="8974d-111">L'elemento \<Key > contiene gli attributi e gli elementi figlio seguenti:</span><span class="sxs-lookup"><span data-stu-id="8974d-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="8974d-112">ID della chiave. Questo valore viene considerato autorevole; il nome file è semplicemente un delicatezza per la leggibilità umana.</span><span class="sxs-lookup"><span data-stu-id="8974d-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="8974d-113">Versione dell'elemento \<Key >, attualmente fissata su 1.</span><span class="sxs-lookup"><span data-stu-id="8974d-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="8974d-114">Le date di creazione, attivazione e scadenza della chiave.</span><span class="sxs-lookup"><span data-stu-id="8974d-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="8974d-115">Elemento > descrittore \<, che contiene informazioni sull'implementazione di crittografia autenticata contenuta in questa chiave.</span><span class="sxs-lookup"><span data-stu-id="8974d-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="8974d-116">Nell'esempio precedente, l'ID della chiave è {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, è stato creato e attivato il 19 marzo 2015 e ha una durata di 90 giorni.</span><span class="sxs-lookup"><span data-stu-id="8974d-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="8974d-117">(Talvolta la data di attivazione potrebbe essere leggermente precedente alla data di creazione, come in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="8974d-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="8974d-118">Ciò è dovuto a un nit nel funzionamento delle API e in pratica.</span><span class="sxs-lookup"><span data-stu-id="8974d-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="8974d-119">Elemento > descrittore \<</span><span class="sxs-lookup"><span data-stu-id="8974d-119">The \<descriptor> element</span></span>

<span data-ttu-id="8974d-120">L'elemento > descrittore \<esterno contiene un attributo deserializerType, che è il nome qualificato dall'assembly di un tipo che implementa IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="8974d-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="8974d-121">Questo tipo è responsabile della lettura dell'elemento > del descrittore di \<interno e dell'analisi delle informazioni contenute all'interno di.</span><span class="sxs-lookup"><span data-stu-id="8974d-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="8974d-122">Il formato particolare dell'elemento > del descrittore di \<dipende dall'implementazione del componente di crittografia autenticata incapsulata dalla chiave e ogni tipo di deserializzatore prevede un formato leggermente diverso per questo oggetto.</span><span class="sxs-lookup"><span data-stu-id="8974d-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="8974d-123">In generale, tuttavia, questo elemento conterrà informazioni algoritmiche (nomi, tipi, OID o simili) e materiale della chiave privata.</span><span class="sxs-lookup"><span data-stu-id="8974d-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="8974d-124">Nell'esempio precedente, il descrittore specifica che questa chiave esegue il wrapping della convalida AES-256-CBC Encryption + HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="8974d-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="8974d-125">Elemento > \<encryptedSecret</span><span class="sxs-lookup"><span data-stu-id="8974d-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="8974d-126">È possibile che sia presente un elemento **&lt;encryptedSecret&gt;** che contiene il formato crittografato del materiale della chiave privata se la [crittografia dei segreti inattivi è abilitata](xref:security/data-protection/implementation/key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="8974d-126">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="8974d-127">L'attributo `decryptorType` è il nome qualificato dall'assembly di un tipo che implementa [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span><span class="sxs-lookup"><span data-stu-id="8974d-127">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="8974d-128">Questo tipo è responsabile della lettura dell'elemento interno **&lt;encryptedKey&gt;** e della relativa decrittografia per recuperare il testo non crittografato originale.</span><span class="sxs-lookup"><span data-stu-id="8974d-128">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="8974d-129">Come con `<descriptor>`, il formato specifico dell'elemento `<encryptedSecret>` dipende dal meccanismo di crittografia dei dati inattivi in uso.</span><span class="sxs-lookup"><span data-stu-id="8974d-129">As with `<descriptor>`, the particular format of the `<encryptedSecret>` element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="8974d-130">Nell'esempio precedente, la chiave master viene crittografata utilizzando Windows DPAPI per il commento.</span><span class="sxs-lookup"><span data-stu-id="8974d-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="8974d-131">Elemento > di revoca \<</span><span class="sxs-lookup"><span data-stu-id="8974d-131">The \<revocation> element</span></span>

<span data-ttu-id="8974d-132">Le revoche sono disponibili come oggetti di primo livello nel repository di chiavi.</span><span class="sxs-lookup"><span data-stu-id="8974d-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="8974d-133">Le revoche di convenzione hanno la revoca del nome file **-{timestamp}. XML** (per revocare tutte le chiavi prima di una data specifica) o la revoca **-{GUID}. XML** (per revocare una chiave specifica).</span><span class="sxs-lookup"><span data-stu-id="8974d-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="8974d-134">Ogni file contiene un singolo elemento di > di revoca \<.</span><span class="sxs-lookup"><span data-stu-id="8974d-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="8974d-135">Per le revoche di singole chiavi, il contenuto del file sarà come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="8974d-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="8974d-136">In questo caso, viene revocata solo la chiave specificata.</span><span class="sxs-lookup"><span data-stu-id="8974d-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="8974d-137">Se l'ID chiave è "\*", tuttavia, come nell'esempio seguente, tutte le chiavi la cui data di creazione è precedente alla data di revoca specificata vengono revocate.</span><span class="sxs-lookup"><span data-stu-id="8974d-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="8974d-138">Il motivo \<> elemento non viene mai letto dal sistema.</span><span class="sxs-lookup"><span data-stu-id="8974d-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="8974d-139">Si tratta semplicemente di una posizione comoda in cui archiviare un motivo leggibile per la revoca.</span><span class="sxs-lookup"><span data-stu-id="8974d-139">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
