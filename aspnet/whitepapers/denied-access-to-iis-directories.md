---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET negato l'accesso a directory IIS | Documenti Microsoft
author: rick-anderson
description: Questo white paper vengono descritte le operazioni da eseguire se una richiesta all'applicazione ASP.NET restituisce l'errore "accesso negato alla directory nomedirectory. Non è riuscito a s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: d95423776a6b58fc67ae6c791685543dadd2480c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30070771"
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET negato l'accesso a directory IIS
====================
> Questo white paper vengono descritte le operazioni da eseguire se una richiesta all'applicazione ASP.NET restituisce l'errore "accesso negato a *nomedirectory* directory. Impossibile avviare il monitoraggio delle modifiche alla directory."
> 
> Si applica a ASP.NET 1.0 e 1.1 di ASP.NET.


Versione RTM di ASP.NET V1 viene ora eseguito con un minore di account di windows - registrato come account "ASPNET" in un computer locale con privilegi.

In alcune bloccati a sistemi, questo account potrebbe non per impostazione predefinita di lettura accesso di sicurezza directory di contenuto di un sito Web, directory radice dell'applicazione o la directory radice del sito web. In questo caso verrà visualizzato l'errore seguente quando viene richiesto di pagine da una determinata applicazione web:

![](denied-access-to-iis-directories/_static/image1.jpg)

Per risolvere questo problema, è necessario modificare le autorizzazioni di sicurezza nelle directory appropriate.

In particolare, ASP.NET richiede la lettura, esecuzione e l'elenco di accesso per l'account ASPNET per la radice del sito web (ad esempio: c:\inetpub\wwwroot o in qualsiasi directory sito alternativo configurate in IIS), la directory del contenuto e delle directory radice dell'applicazione per monitorare le modifiche di file di configurazione. La radice dell'applicazione corrisponde al percorso della cartella associato alla directory virtuale dell'applicazione nello strumento di amministrazione di IIS (inetmgr).

Ad esempio, si consideri la seguente gerarchia di applicazione nella cartella wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

Per questo esempio, l'account ASPNET richiede le autorizzazioni di lettura definite in precedenza per il contenuto di myapp sia la directory wwwroot. Un singolo ACL ereditato per la cartella radice, facoltativamente, nonché per entrambe le directory se si sta annidati.

Per aggiungere autorizzazioni a una directory, eseguire la procedura seguente:

- Utilizzando Esplora risorse, passare alla directory
- Fare clic con il pulsante destro sulla cartella directory, quindi scegliere "Proprietà"
- Passare alla scheda "Sicurezza" nella finestra di dialogo proprietà
- Fare clic sul pulsante "Aggiungi" e immettere il nome del computer seguito dal nome dell'account ASPNET. Ad esempio, in un computer denominato "webdev", si immettere webdev\ASPNET e fare clic su "OK".
- Verificare che l'account ASPNET disponga di "lettura &amp; Execute", "Visualizzazione contenuto cartella" e "Lettura" caselle di controllo selezionata.
- Fare clic su OK per chiudere la finestra di dialogo e salvare le modifiche.

![](denied-access-to-iis-directories/_static/image2.jpg)

Se si desidera, queste modifiche possono essere automatizzate tramite script o lo strumento "cacls.exe" fornito con Windows. Per ulteriori informazioni sull'account ASPNET, vedere il [documento domande frequenti su](https://go.microsoft.com/fwlink/?LinkId=5828).

Se una determinata applicazione web si basa sulla disponibilità di scrittura o modificare le autorizzazioni per un file o cartella particolare, questo può essere concessa seguendo la stessa procedura e selezionando le caselle di controllo "Scrittura" e/o "Modifica".

Nei computer che consentono a tutti gli utenti o l'accesso in lettura gruppo utenti di queste directory (ovvero la configurazione predefinita), non si verificheranno problemi e i passaggi precedenti non saranno più necessari.
