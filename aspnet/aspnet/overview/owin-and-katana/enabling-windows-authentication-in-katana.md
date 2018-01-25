---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Abilitazione dell'autenticazione di Windows in Katana | Documenti Microsoft
author: MikeWasson
description: 'In questo articolo viene illustrato come abilitare l''autenticazione di Windows in Katana. Illustra due scenari: utilizzo di IIS per ospitare Katana e HttpListener per indipendente Kat...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="enabling-windows-authentication-in-katana"></a>Abilitazione dell'autenticazione di Windows in Katana
====================
da [Mike Wasson](https://github.com/MikeWasson)

> In questo articolo viene illustrato come abilitare l'autenticazione di Windows in Katana. Illustra due scenari: utilizzo di IIS per ospitare Katana e HttpListener per indipendente Katana in un processo personalizzato. Grazie a Chris Ross Barry Dorrans e David Matson per la revisione dell'articolo.


Katana è l'implementazione Microsoft di [OWIN](http://owin.org/), l'interfaccia Web aperta per .NET. È possibile leggere un'introduzione ai OWIN e Katana [qui](an-overview-of-project-katana.md). L'architettura OWIN ha diversi livelli:

- Host: Gestisce il processo in cui viene eseguita la pipeline OWIN.
- Server: Consente di aprire un socket di rete e in ascolto delle richieste.
- Middleware: Elabora la richiesta e risposta HTTP.

Katana offre attualmente due server, che supportano entrambi l'autenticazione integrata di Windows:

- **Microsoft.Owin.Host.SystemWeb**. Usa IIS con la pipeline ASP.NET.
- **Microsoft.Owin.Host.HttpListener**. Usa [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Il server è l'opzione predefinita quando self-hosting Katana.

> [!NOTE]
> Katana non attualmente fornisce middleware OWIN per l'autenticazione di Windows, perché questa funzionalità è già disponibile nel server.


## <a name="windows-authentication-in-iis"></a>Autenticazione di Windows in IIS

Utilizza systemweb, è semplicemente possibile abilitare l'autenticazione di Windows in IIS.

Iniziamo creando una nuova applicazione ASP.NET, utilizzando il modello di progetto "Applicazione Web ASP.NET vuota".

![](enabling-windows-authentication-in-katana/_static/image1.png)

Successivamente, aggiungere pacchetti NuGet. Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Ora aggiungere una classe denominata `Startup` con il codice seguente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Questo è tutto che è necessario creare un'applicazione "Hello world" per OWIN, in esecuzione in IIS. ‎Premere F5 per eseguire il debug dell'applicazione. Dovrebbe essere "Hello World!" Nella finestra del browser.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Successivamente, si verrà abilitata l'autenticazione di Windows in IIS Express. Dal **vista** dal menu **proprietà**. Fare clic sul nome del progetto in Esplora soluzioni per visualizzare le proprietà del progetto.

Nel **proprietà** finestra impostare **l'autenticazione anonima** a **disabilitato** e impostare **l'autenticazione di Windows** per  **Abilitato**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Quando si esegue l'applicazione da Visual Studio, IIS Express richiederà le credenziali dell'utente Windows. È possibile vedere questo utilizzando [Fiddler](http://fiddler2.com/home) o HTTP di un altro strumento di debug. Di seguito è riportato un esempio di risposta HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Le intestazioni WWW-Authenticate nella risposta indicano che il server supporta il [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocollo, che utilizza Kerberos o NTLM.

In un secondo momento, quando si distribuisce l'applicazione a un server, seguire [procedura](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) per abilitare l'autenticazione di Windows in IIS su tale server.

## <a name="windows-authentication-in-httplistener"></a>Autenticazione di Windows in HttpListener

Se si utilizza HttpListener per l'hosting indipendente Katana, è possibile abilitare l'autenticazione di Windows direttamente nel **HttpListener** istanza.

Innanzitutto, creare una nuova applicazione console. Successivamente, aggiungere pacchetti NuGet. Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Ora aggiungere una classe denominata `Startup` con il codice seguente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Questa classe implementa lo stesso esempio "Hello world" dalla prima, ma imposta inoltre l'autenticazione di Windows come schema di autenticazione.

All'interno di `Main` di funzione, avviare la pipeline OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

È possibile inviare una richiesta in Fiddler per confermare che l'applicazione utilizza l'autenticazione di Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Argomenti correlati

[Panoramica del progetto Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Informazioni sui OWIN autenticazione basata su form in MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
