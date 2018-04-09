---
uid: whitepapers/ms03-32-issue
title: Correzione di errore "Applicazione Server non disponibile" dopo aver applicato l'aggiornamento della sicurezza per Internet Explorer | Documenti Microsoft
author: rick-anderson
description: Questo documento descrive la patch che corregge un problema con l'aggiornamento della sicurezza MS03-32 per Internet Explorer che influisce sulle applicazioni ASP.NET 1.0 in esecuzione nell'elemento di lavoro...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: dd1a564cd347364abc3ca5ac0a9ffda448bcede8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Correggere l'errore "Applicazione Server non disponibile" dopo aver applicato l'aggiornamento della sicurezza per Internet Explorer
====================
> Questo documento descrive la patch che corregge un problema con l'aggiornamento della sicurezza MS03-32 per Internet Explorer che influisce sulle applicazioni ASP.NET 1.0 in esecuzione in Windows XP Professional.
> 
> Si applica a ASP.NET 1.0 e Windows XP Professional.


Microsoft identificato un problema con l'aggiornamento della sicurezza per la patch di sicurezza di Internet Explorer MS03-32 e versione 1.0 di ASP.NET in esecuzione in Windows XP. Questa patch può essere installata manualmente o per ottenere gli aggiornamenti critici di recente dal sito Windows Update.

Il sintomo di questo problema è che dopo l'installazione di patch in un computer Windows XP, tutte le richieste di applicazioni ASP.NET in esecuzione nel server web IIS 5.1 locale dare come risultato un messaggio di errore che indica "Applicazione Server non disponibile". Le richieste ai server web remoto non sono interessate.

Questo problema influisce solo sulle installazioni in esecuzione ASP.NET 1.0 in Windows XP. Non influisce sulle macchine che eseguono Windows 2000 o Windows Server 2003. Non influisce inoltre i computer che eseguono Windows XP con ASP.NET 1.1 installato.

Si noti che questo problema **non** un bug di sicurezza con ASP.NET. Si **non** aprire o consentire eventuali attacchi contro un server o un'applicazione ASP.NET. Al contrario, è semplicemente un bug funzionale causato dalla patch stesso.

Si sta lavorando attivamente in una soluzione permanente per questo problema. Nel frattempo, è possibile eseguire il seguente file batch come soluzione alternativa per il problema. Il file batch effettua le operazioni seguenti:

1. Arresta i servizi di stato IIS e ASP.NET
2. Elimina e ricrea l'account ASPNET con una password temporanea nota
3. Usa Windows `runas` comando per avviare un eseguibile che consente di creare un profilo utente ASPNET
4. Registrare nuovamente ASP.NET. Verrà creata una nuova password casuale per l'account e si applica impostazioni di controllo di accesso predefinite ASP.NET per tale
5. Riavvia il servizio IIS

Il file batch contiene una password temporanea hardcoded di "<strong>1pass@word</strong>" quale sarà chiesto di immettere per il runas comando quando viene eseguito il file batch. Al termine dell'esecuzione del comando runas, la password dell'account ASPNET viene ricreata con un valore casuale complesso. Si noti che il file batch potrebbe non riuscire se la password hardcoded non soddisfa i requisiti di complessità delle password nell'ambiente in uso. Se è questo il caso, è possibile modificarlo in un altro valore appropriato per l'ambiente.

*> [!IMPORTANT]* Se si hanno aggiunto le impostazioni di controllo di accesso personalizzati o database account le autorizzazioni per l'account ASPNET, dovranno essere ricreati dopo il completamento di questo file batch. Infatti, quando viene ricreato l'account, riceverà un nuovo ID di sicurezza (SID).

*> [!IMPORTANT]* Se si esegue il processo di lavoro ASP.NET con un account personalizzato diverso dall'account ASPNET, non è necessario eseguire questo file batch. Invece, si deve accedere modo interattivo o utilizzare il comando runas con tale account verrà creato un profilo utente per tale account.

Il file batch è incluso nell'archivio autoestraente riportato di seguito. Utilizzo:

1. È necessario essere in esecuzione come account con privilegi di amministratore
2. [Scaricare e aprire il file eseguibile autoestraente](ms03-32-issue/_static/fixup1.exe)
3. Estrarre il contenuto in c:\.
4. Scegliere Esegui... dal menu start e immettere `cmd.exe`
5. Nella finestra comando Apri, digitare `c:\fixup.cmd`.
6. Quando richiesto, immettere <strong>1pass@word</strong> come password.
7. Se si dispone di impostazioni di controllo di accesso personalizzate in precedenza o le autorizzazioni per l'account ASPNET account di database, è necessario riapplicare le impostazioni.

Molti scusiamo per l'inconveniente che questo ha causato. È possibile registrare informazioni aggiuntive appena sarà disponibile.

La matrice di seguito in dettaglio le piattaforme e versioni interessate dal problema.

| .NET Framework | Piattaforma | Interessati |
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
