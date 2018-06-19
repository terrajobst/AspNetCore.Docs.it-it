---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: L'aggiornamento di un'applicazione MVC ASP.NET 1.0 per ASP.NET MVC 2 | Documenti Microsoft
author: rick-anderson
description: Questo documento descritta come eseguire l'aggiornamento di una procedura guidata e manualmente un'applicazione ASP.NET MVC 1.0 per ASP.NET MVC 2. Questo documento è disponibile anche per d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530060"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>L'aggiornamento di un'applicazione MVC ASP.NET 1.0 per ASP.NET MVC 2
====================
> Questo documento descritta come eseguire l'aggiornamento di una procedura guidata e manualmente un'applicazione ASP.NET MVC 1.0 per ASP.NET MVC 2. Questo documento è disponibile anche per [scaricare](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Introduzione

ASP.NET MVC 2 può essere installato in modo affiancato con ASP.NET MVC 1.0 nello stesso server. Questo offre flessibilità agli sviluppatori dell'applicazione scegliere quando eseguire l'aggiornamento di un'applicazione MVC ASP.NET 1.0 per ASP.NET MVC 2.

Visual Studio 2010 include una procedura guidata che gli aggiornamenti di progetti ASP.NET MVC 1.0 esistenti compilati con Visual Studio 2008 per ASP.NET MVC 2. L'aggiornamento guidato è stato avviato, aprire un progetto ASP.NET MVC 1.0 in Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Aggiornamento guidato per ASP.NET MVC 1.0 in Visual Studio 2008 SP1

Per aggiornare un'applicazione MVC ASP.NET 1.0 per ASP.NET MVC 2 in Visual Studio 2008 SP1, utilizzare l'applicazione MvcAppConverter (non supportato). È possibile scaricare l'applicazione dall'URL seguente:

[https://go.microsoft.com/fwlink/?LinkId=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Aggiornamento manuale di un progetto MVC ASP.NET 1.0

Per aggiornare manualmente un'applicazione MVC ASP.NET 1.0 alla versione 2, seguire questi passaggi:

1. Eseguire un backup del progetto esistente.
2. In un editor di testo aprire il file di progetto (file con estensione csproj o vbproj) e trovare l'elemento ProjectTypeGuid. Se il valore di tale elemento, sostituire il GUID {603c0e0b-db56-11dc-be95-000d561079b0} con {F85E285D-A4E0-4152-9332-AB1D724D3325}. Al termine, il valore di tale elemento deve essere come segue: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Nella cartella radice dell'applicazione Web, modificare il file Web. config. Ricerca per System, versione = 1.0.0.0 e sostituire tutte le istanze con System, versione = 2.0.0.0.
4. Ripetere il passaggio precedente per il file Web. config che si trova nella cartella Views.
5. Aprire il progetto con Visual Studio e in **Esplora**, espandere il **riferimenti** nodo. Eliminare il riferimento a System (che fa riferimento all'assembly versione 1.0). Aggiungere un riferimento a System (v2.0.0.0).
6. Aggiungere l'elemento bindingRedirect seguente al file Web. config nella radice dell'applicazione nella sezione di configurazione:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Creare una nuova applicazione ASP.NET MVC 2 vuota. Copiare i file dalla cartella Scripts della nuova applicazione nella cartella degli script dell'applicazione esistente.
8. Aggiornare il € applicazioni esistenti™ file CSS s con le definizioni di stile CSS nel file Site.
9. Compilare l'applicazione ed eseguirlo. Se si verificano errori, vedere la sezione di modifiche di rilievo del [novità di ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) pagina.
