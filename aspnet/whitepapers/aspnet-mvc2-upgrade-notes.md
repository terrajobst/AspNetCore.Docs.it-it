---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: L'aggiornamento di un'applicazione ASP.NET MVC 1.0 ad ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive entrambi come aggiornare una procedura guidata e manualmente un'applicazione ASP.NET MVC 1.0 per ASP.NET MVC 2. Questo documento è disponibile anche per d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 354dbab3057068eb13b16c9aa31f66c72371ce7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381916"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>L'aggiornamento di un'applicazione ASP.NET MVC 1.0 ad ASP.NET MVC 2
====================
> Questo documento descrive entrambi come aggiornare una procedura guidata e manualmente un'applicazione ASP.NET MVC 1.0 per ASP.NET MVC 2. Questo documento è disponibile anche per [scaricare](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Introduzione

ASP.NET MVC 2 può essere installato affiancato con ASP.NET MVC 1.0 nello stesso server. Questo offre applicazioni sviluppatori flessibilità nella scelta del momento aggiornare un'applicazione ASP.NET MVC 1.0 ad ASP.NET MVC 2.

Visual Studio 2010 include una procedura guidata che gli aggiornamenti di progetti ASP.NET MVC 1.0 esistenti compilati con Visual Studio 2008 per ASP.NET MVC 2. Viene avviato aggiornamento guidato, aprire un progetto ASP.NET MVC 1.0 in Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Aggiornamento guidato per ASP.NET MVC 1.0 in Visual Studio 2008 SP1

Per aggiornare un'applicazione ASP.NET MVC 1.0 ad ASP.NET MVC 2 in Visual Studio 2008 SP1, usare l'applicazione MvcAppConverter (non supportato). È possibile scaricare l'applicazione dall'URL seguente:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Aggiornare manualmente un progetto ASP.NET MVC 1.0

Per aggiornare manualmente un'applicazione ASP.NET MVC 1.0 esistente alla versione 2, seguire questa procedura:

1. Eseguire il backup del progetto esistente.
2. In un editor di testo aprire il file di progetto (file con estensione csproj o vbproj) e trovare l'elemento Generators\projecttypeguid. Il valore di quell'elemento, sostituire il GUID {603c0e0b-db56-11dc-be95-000d561079b0} con {F85E285D-A4E0-4152-9332-AB1D724D3325}. Al termine, il valore di tale elemento deve essere come segue: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Nella cartella radice dell'applicazione Web, modificare il file Web. config. Cercare System, versione = 1.0.0.0 e sostituire tutte le istanze con System, versione = 2.0.0.0.
4. Ripetere il passaggio precedente per il file Web. config che si trova nella cartella Views.
5. Aprire il progetto con Visual Studio e nella **Esplora soluzioni**, espandere il **riferimenti** nodo. Eliminare il riferimento a System (che fa riferimento all'assembly versione 1.0). Aggiungere un riferimento a System (v2.0.0.0).
6. Aggiungere l'elemento bindingRedirect seguente al file Web. config nella radice dell'applicazione sotto la sezione di configurazione:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Creare una nuova applicazione ASP.NET MVC 2 vuota. Copiare i file dalla cartella degli script della nuova applicazione nella cartella degli script dell'applicazione esistente.
8. Aggiornare il € dell'applicazione esistente™ file CSS s con le definizioni di stile CSS nel file Site CSS.
9. Compilare l'applicazione ed eseguirla. Se si verificano errori, fare riferimento alla sezione delle modifiche di rilievo nel [What ' s New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) pagina.
