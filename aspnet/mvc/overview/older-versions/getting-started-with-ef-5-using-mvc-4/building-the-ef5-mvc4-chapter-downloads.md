---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Compilazione il capitolo di download per il MVC EF 5 4 esercitazioni | Documenti Microsoft
author: Rick-Anderson
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 912a1383ed170b49782657372abc1801140df8dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Compilazione il capitolo di download per il MVC EF 5 4 esercitazioni
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 utilizzando Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


## <a name="building-the-chapter-downloads"></a>Compilazione di download di capitolo

1. Scaricare e decomprimere i file zip di esempio di progetto. Il pacchetto di download decompresso, sono disponibili i file zip aggiuntivi, uno per il completamento di ogni capitolo.
2. Fare clic con il pulsante destro sul file zip desiderato, fare clic su **proprietà**, fare clic su di **Unblock** pulsante.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Decomprimere il file.
4. Fare doppio clic su di *CUx.sln* file per avviare Visual Studio.
5. Dal **strumenti** menu, fare clic su **Gestione pacchetti libreria**, quindi **Package Manager Console**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. Fare clic su nel Package Manager Console (PMC) **ripristinare**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Uscire da Visual Studio.
8. Riavviare Visual Studio, aprire il file di soluzione che è stata chiusa nel passaggio precedente.
9. In Package Manager Console (PMC), immettere il `Update-Database` comando:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Se viene visualizzato l'errore seguente:  
    >   
    >  *Il termine 'Update-Database' non è riconosciuto come nome di un cmdlet, funzione, file di script o programma eseguibile. Controllare l'ortografia del nome, o se è stato incluso un percorso, verificare che il percorso sia corretto e riprovare.*  
    > Chiudere e riavviare Visual Studio.

    Verrà eseguito ogni migrazione, quindi verrà eseguito il metodo di inizializzazione. È ora possibile eseguire l'app.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

>[!div class="step-by-step"]
[Precedente](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
