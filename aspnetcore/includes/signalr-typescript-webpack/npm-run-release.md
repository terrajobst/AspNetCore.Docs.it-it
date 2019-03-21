---
ms.openlocfilehash: c82571d3cfa57ccd6e7c83f654f119bdd8991486
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210438"
---
```console
npm run release
```

Questo comando restituisce le risorse lato client che devono essere servite durante l'esecuzione dell'app. Le risorse si trovano nella cartella *wwwroot*.

Webpack ha completato le attività seguenti:

* Ha ripulito il contenuto della directory *wwwroot*.
* Ha convertito TypeScript in JavaScript, un processo noto con il termine tecnico *transpiling*.
* Ha modificato il codice JavaScript generato per ridurre le dimensioni del file, un processo noto con il termine tecnico *minimizzazione*.
* Ha copiato i file JavaScript, CSS e HTML elaborati dalla directory *src* alla directory *wwwroot*.
* Ha inserito gli elementi seguenti nel file *wwwroot/index.html*:
  * Un tag `<link>` che fa riferimento al file *wwwroot/main.\<hash\>.css*. Questo tag viene inserito immediatamente prima del tag `</head>` di chiusura.
  * Un tag `<script>` che fa riferimento al file *wwwroot/main.\<hash\>.js* minimizzato. Questo tag viene inserito immediatamente prima del tag `</body>` di chiusura.
