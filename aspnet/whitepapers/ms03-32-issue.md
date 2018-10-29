---
uid: whitepapers/ms03-32-issue
title: Correzione per errore Server "applicazione non disponibile" dopo aver applicato l'aggiornamento della sicurezza per IE | Microsoft Docs
author: rick-anderson
description: In questo documento descrive la patch che corregge un problema con l'aggiornamento della sicurezza 32 MS03 per Internet Explorer, che influisce sulle applicazioni ASP.NET 1.0 in esecuzione nell'elemento di lavoro...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: 9041f8d15a449a517594f8051c3d9f0ceb18a8a3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207017"
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Correzione per errore Server "applicazione non disponibile" dopo aver applicato l'aggiornamento della sicurezza per Internet Explorer
====================
> In questo documento descrive la patch che corregge un problema con l'aggiornamento della sicurezza 32 MS03 per Internet Explorer, che influisce sulle applicazioni ASP.NET 1.0 in esecuzione in Windows XP Professional.
> 
> Si applica a Windows XP Professional e ASP.NET 1.0.


Microsoft identificato un problema con l'aggiornamento della sicurezza 32 MS03 patch di sicurezza di Internet Explorer e ASP.NET 1.0 in esecuzione in Windows XP. Questa patch può essere installata manualmente o scaricando gli aggiornamenti critici di recente dal sito Windows Update.

Il sintomo di questo problema è che, dopo l'installazione di patch in un computer Windows XP, tutte le richieste alle applicazioni ASP.NET in esecuzione nel server web IIS 5.1 locale restituito in un messaggio di errore che informa che "Server applicazione non disponibile". Le richieste ai server web remoto non sono interessate.

Questo problema influisce solo sulle installazioni in esecuzione di ASP.NET 1.0 in Windows XP. Non influisce sulle macchine che eseguono Windows 2000 o Windows Server 2003. Inoltre non influisce sulle macchine che eseguono Windows XP con ASP.NET 1.1 installato.

Si noti che questo problema **non è** un bug di sicurezza con ASP.NET. Si **non** aprire o consentire eventuali attacchi dannosi contro un'applicazione ASP.NET o server. È invece semplicemente un bug funzionale causato dalla patch di se stesso.

Stiamo lavorando sodo su una soluzione definitiva per risolvere questo problema. Nel frattempo, è possibile eseguire il seguente file batch come soluzione alternativa per il problema. Il file batch esegue le operazioni seguenti:

1. Arresta i servizi di stato IIS e ASP.NET
2. Elimina e ricrea l'account ASPNET con una password temporanea nota
3. Usa i Windows `runas` comando per avviare un eseguibile che consente di creare un profilo utente ASPNET
4. Registrare nuovamente ASP.NET. Ciò crea una nuova password casuale per l'account e applica impostazioni di controllo di accesso predefinita ASP.NET per tale
5. Riavvia il servizio IIS

Il file batch contiene una password temporanea hardcoded di "<strong>1pass\@word</strong>" quale sarà chiesto di immettere per il runas comando quando viene eseguito il file batch. Al termine dell'esecuzione del comando runas, la password dell'account ASPNET viene ricreata con un valore casuale complesso. Si noti che il file batch potrebbe non riuscire se la password specificata non soddisfa i requisiti di complessità delle password nell'ambiente in uso. In tal caso, è possibile modificarlo in un altro valore appropriato per l'ambiente.

*> [!IMPORTANT]* Se si hanno aggiunto le impostazioni di controllo di accesso personalizzati o database account le autorizzazioni per l'account ASPNET, dovranno essere ricreati dopo il completamento di questo file batch. Questo avviene perché quando viene ricreata l'account, otterrà un nuovo ID di sicurezza (SID).

*> [!IMPORTANT]* Se si esegue il processo di lavoro ASP.NET con un account personalizzato diverso dall'account ASPNET, non è necessario eseguire questo file batch. Al contrario, si deve accedere in modo interattivo o usare il comando runas con tale account verrà creato un profilo utente per l'account.

Il file batch è incluso nell'archivio autoestraente riportato di seguito. Per usarla:

1. È necessario essere in esecuzione come account con privilegi di amministratore
2. [Scaricare e aprire il file eseguibile autoestraente](ms03-32-issue/_static/fixup1.exe)
3. Estrarre il contenuto in c:\.
4. Selezionare Esegui... dal menu start e immettere `cmd.exe`
5. Nelle finestre di comando di apertura, digitare `c:\fixup.cmd`.
6. Quando richiesto, immettere <strong>1pass\@word</strong> come password.
7. Se si dispone di autorizzazioni dell'account del database per l'account ASPNET o le impostazioni di controllo di accesso personalizzati in precedenza, è necessario applicare nuovamente tali impostazioni a questo punto.

Molti scusiamo per eventuali inconvenienti che ciò ha causato. È possibile registrare informazioni aggiuntive appena sarà disponibile.

La matrice seguente descrive in dettaglio le piattaforme e alle versioni interessate dal problema.

| .NET Framework | Piattaforma | Interessate |
| --- | --- | --- |
| Versione 1.0 | Windows 2000 Professional | No |
| Versione 1.0 | Windows 2000 Server | No |
| Versione 1.0 | Windows XP Professional | Yes |
| Versione 1.0 | Windows Server 2003 | No |
| Versione 1.0 | Windows XP Home Edition con Cassini | No |
| Versione 1.1 | Windows 2000 Professional | No |
| Versione 1.1 | Windows 2000 Server | No |
| Versione 1.1 | Windows XP Professional | No |
| Versione 1.1 | Windows Server 2003 | No |
| Versione 1.1 | Windows XP Home Edition con Cassini | No |

Grazie,   
 Il Team di ASP.NET
