---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Compilazione il capitolo download per Entity Framework 5 MVC 4 esercitazioni | Microsoft Docs
author: Rick-Anderson
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 6f1a28a2703fa543430d0210cc7792cb19439136
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379920"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Compilazione il capitolo download per Entity Framework 5 MVC 4 esercitazioni
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[Download progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


## <a name="building-the-chapter-downloads"></a>Compilazione di download dei capitoli

1. Scaricare e decomprimere il file con estensione zip di esempio progetto. Nel pacchetto di download decompresso, si noterà il file zip aggiuntivi, uno per il completamento di ogni capitolo.
2. Fare clic con il pulsante destro sul file zip desiderata, fare clic su **delle proprietà**, fare clic sui **Unblock** pulsante.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Decomprimere il file.
4. Fare doppio clic il *CUx.sln* file per avviare Visual Studio.
5. Dal **degli strumenti** menu, fare clic su **Library Package Manager**, quindi **Package Manager Console**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. In Package Manager Console (PMC), fare clic su **ripristinare**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Uscire da Visual Studio.
8. Riavviare Visual Studio, aprire il file della soluzione che è stata chiusa nel passaggio precedente.
9. In Package Manager Console (PMC), immettere il `Update-Database` comando:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Se viene visualizzato l'errore seguente:  
    >   
    >  *Il termine "Update-Database" non è riconosciuto come il nome di un cmdlet, funzione, file di script o programma eseguibile. Controllare l'ortografia del nome, o se è stato incluso un percorso, verificare che il percorso sia corretto e riprovare.*  
    > Chiudere e riavviare Visual Studio.

    Verrà eseguita ogni migrazione, quindi verrà eseguito il metodo di inizializzazione. È ora possibile eseguire l'app.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Precedente](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
