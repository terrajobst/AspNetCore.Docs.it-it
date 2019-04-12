---
title: Layout di Razor componenti
author: guardrex
description: Informazioni su come creare componenti riutilizzabili di layout per App i componenti di Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/layouts
ms.openlocfilehash: 31ed940ce40e3ae6e3744418cf241d396308f4fe
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515524"
---
# <a name="razor-components-layouts"></a>Layout di Razor componenti

Da [Stropek Rainer](https://www.timecockpit.com)

Le App in genere contengono più di un componente. Gli elementi di layout, ad esempio menu, i messaggi sul copyright e i loghi, devono essere presenti in tutti i componenti. Copia il codice di questi elementi di layout in tutti i componenti di un'app non è un approccio efficace. Tale funzionalità è difficile da gestire e probabilmente conduce al contenuto incoerente nel corso del tempo. *Layout* risolvere questo problema.

Tecnicamente, un layout è semplicemente un altro componente. Un layout è definito in un modello Razor o in C# del codice e può contenere [associazione di dati](xref:razor-components/components#data-binding), [inserimento di dipendenze](xref:razor-components/dependency-injection)e altre funzionalità comuni dei componenti.

Due aspetti aggiuntivi attiva una *component* in un *layout*

* Il componente del layout deve ereditare da `LayoutComponentBase`. `LayoutComponentBase` definisce un `Body` proprietà che contiene il contenuto da sottoporre a rendering all'interno del layout.
* Il componente layout utilizza il `Body` proprietà per specificare dove deve essere il contenuto del corpo viene eseguito il rendering tramite la sintassi Razor `@Body`. Durante il rendering, `@Body` viene sostituito dal contenuto del layout.

Esempio di codice seguente viene illustrato il modello di un componente del layout Razor. Si noti l'uso del `LayoutComponentBase` e `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a>Usare un layout in un componente

Usare la direttiva Razor `@layout` per applicare un layout a un componente. Il compilatore converte questa direttiva in un `LayoutAttribute`, cui viene applicata alla classe del componente.

Esempio di codice seguente viene illustrato il concetto. Il contenuto di questo componente viene inserito il *MasterLayout* in corrispondenza della posizione di `@Body`:

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a>Selezione di layout centralizzato

Ogni cartella di un un'app può contenere un file di modello denominato *viewimports. cshtml*. Il compilatore include le direttive specificate nel file di importazioni di visualizzazione in tutti i modelli Razor nella stessa cartella e in modo ricorsivo in tutte le relative sottocartelle. Pertanto, un *viewimports. cshtml* file contenente `@layout MainLayout` assicura che tutti i componenti in un cartella, usare i *MainLayout* layout. Non è necessario aggiungere ripetutamente `@layout` a tutte le *.razor* file.

Si noti che il modello predefinito Usa la *viewimports. cshtml* meccanismo per la selezione di layout. Un'app appena creata contiene il *viewimports. cshtml* del file nel *componenti o delle pagine* cartella.

## <a name="nested-layouts"></a>Layout annidati

Le app possono essere costituito da layout annidati. Un componente può fare riferimento a un layout che a sua volta fa riferimento a un altro layout. Ad esempio, il layout di annidamento è utilizzabile in base a una struttura a più livelli.

Esempi di codice seguenti mostrano come usare layout annidati. Il *EpisodesComponent.cshtml* file è il componente da visualizzare. Si noti che il componente vi fa riferimento il layout `MasterListLayout`.

*EpisodesComponent.cshtml*:

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

Il *MasterListLayout.cshtml* fornisce file di `MasterListLayout`. Il layout fa riferimento a un altro layout, `MasterLayout`, in cui verrà incorporato.

*MasterListLayout.cshtml*:

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

Infine, `MasterLayout` contiene gli elementi di layout di primo livello, ad esempio l'intestazione, piè di pagina e menu principale.

*MasterLayout.cshtml*:

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
