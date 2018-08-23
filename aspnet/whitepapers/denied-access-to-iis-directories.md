---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET negato l'accesso alle directory IIS | Microsoft Docs
author: rick-anderson
description: Questo white paper vengono descritte le operazioni da eseguire se una richiesta all'applicazione ASP.NET restituisce l'errore "accesso negato alla directory DirectoryName. Non è riuscito a s...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: c3a14f51df7aaf5c5935cf60ee4e687c10048e91
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833416"
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET negato l'accesso a directory di IIS
====================
> Questo white paper vengono descritte le operazioni da eseguire se una richiesta all'applicazione ASP.NET restituisce l'errore "non può accedervi *nomedirectory* directory. Non è stato possibile avviare il monitoraggio delle modifiche alla directory."
> 
> Si applica a ASP.NET 1.0 e ASP.NET 1.1.


ASP.NET V1 RTM viene ora eseguita usando un minore di account di windows - registrato come account "ASPNET" in un computer locale con privilegi.

In alcune bloccato sistemi, questo account potrebbe non per impostazione predefinita hanno accesso in lettura sicurezza per le directory contenuto di un sito Web, la directory radice dell'applicazione o la directory radice del sito web. In questo caso si riceverà l'errore seguente quando viene richiesto di pagine da un'applicazione web specificato:

![](denied-access-to-iis-directories/_static/image1.jpg)

Per risolvere questo problema è necessario modificare le autorizzazioni di sicurezza nella directory appropriate.

In particolare, ASP.NET richiede la lettura, esecuzione e visualizzazione di accesso per l'account ASPNET per la radice del sito web (ad esempio: c:\inetpub\wwwroot o in qualsiasi directory sito alternativo potrebbe aver configurato in IIS), la directory del contenuto e la directory radice dell'applicazione per monitorare le modifiche ai file di configurazione. La radice dell'applicazione corrisponde al percorso della cartella associato alla directory virtuale dell'applicazione nello strumento di amministrazione IIS (inetmgr).

Ad esempio, prendere in considerazione la seguente gerarchia applicazione all'interno della cartella wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

Per questo esempio, l'account ASPNET richiede le autorizzazioni di lettura definite in precedenza per il contenuto di myapp sia della directory wwwroot. Un singolo ACL ereditato nella cartella radice, facoltativamente, nonché per entrambe le directory se si sta annidati.

Per aggiungere autorizzazioni in una directory, seguire i passaggi seguenti:

- Con Windows explorer, passare alla directory
- Fare clic con il pulsante destro sulla cartella directory e scegliere "Proprietà"
- Passare alla scheda "Sicurezza" nella finestra di dialogo proprietà
- Fare clic sul pulsante "Aggiungi" e immettere il nome del computer seguito dal nome dell'account ASPNET. Ad esempio, in un computer denominato "webdev", si potrebbe immettere webdev\ASPNET e faccio clic su "OK".
- Assicurarsi che l'account ASPNET disponga di "lettura &amp; Execute", "Visualizzazione contenuto cartella" e "Lettura" caselle di controllo selezionate.
- Fare clic su OK per chiudere la finestra di dialogo e salvare le modifiche.

![](denied-access-to-iis-directories/_static/image2.jpg)

Se si desidera, queste modifiche possono essere automatizzate tramite script o lo strumento "cacls.exe" fornito con Windows. Per altre informazioni sull'account ASPNET, vedere la [documento di FAQ](https://go.microsoft.com/fwlink/?LinkId=5828).

Se una determinata applicazione web si basa sulla disponibilità di scrittura o modificare le autorizzazioni per file o una cartella particolare, ciò può essere concessa seguendo la stessa procedura e deselezionando le caselle di controllo "Scrittura" e/o "Modifica".

Nei computer che consentono a tutti gli utenti o l'accesso in lettura gruppo utenti in queste directory, ovvero la configurazione predefinita, non viene rilevato alcun problema e i passaggi precedenti non saranno più necessari.
