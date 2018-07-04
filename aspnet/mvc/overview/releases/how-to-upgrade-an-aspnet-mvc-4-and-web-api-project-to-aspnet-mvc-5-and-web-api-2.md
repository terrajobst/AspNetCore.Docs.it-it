---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Come aggiornare un ASP.NET MVC 4 e Web del progetto API ASP.NET MVC 5 e API Web 2 | Microsoft Docs
author: Rick-Anderson
description: ASP.NET MVC 5 e API Web 2 offrono una serie di nuove funzionalità, tra cui routing con attributi filtri di autenticazione e molto altro ancora.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7ccdc1bf7a1b1b8d5d9c5906eeeab9535b26df6c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369247"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Come aggiornare un ASP.NET MVC 4 e un progetto API Web ASP.NET MVC 5 e API Web 2
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 e API Web 2 offrono una serie di nuove funzionalità, tra cui routing con attributi filtri di autenticazione e molto altro ancora. Visualizzare [ https://www.asp.net/vnext ](https://www.asp.net/core) per altri dettagli.
> 
> Questa procedura dettagliata guiderà l'utente con i passaggi necessari per aggiornare l'applicazione per la versione più recente.  
> 
> > [!NOTE]
> > Vedi [ASP.NET and Web Tools per Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md) per informazioni sulle modifiche di rilievo rispetto MVC 4 e API Web alla versione successiva.
> 
>   
> 
> Questo articolo è stato scritto da Youngjune Hong e Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Passaggi di aggiornamento

1. Eseguire il backup del progetto. Questa procedura dettagliata è necessario apportare modifiche al file di progetto, configurazione del pacchetto e i file Web. config.
2. Per l'aggiornamento dall'API Web a Web API 2, in Global. asax, modificare:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   in

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Assicurarsi che tutti i pacchetti che usano i progetti siano compatibili con MVC 5 e API Web 2. La seguente tabella vengono mostrate le MVC 4 e API Web correlati i pacchetti che devono essere modificati. Se si dispone di un pacchetto che dipende da uno dei pacchetti elencati di seguito, contattare il server di pubblicazione per ottenere le versioni più recenti che sono compatibili con MVC 5 e API Web 2. Se hai il codice sorgente per tali pacchetti, è necessario ricompilarla con i nuovi assembly di Web API 2 e MVC 5.   

    | **Id del pacchetto** | **Versione precedente** | **Nuova versione** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0.x. | 2.2.x. |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System. Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | <o:p> </o:p> | Rimosso |
    | Microsoft.AspNet.WebPages.Administration | <o:p> </o:p> | Rimosso |
    | Microsoft-Web-Helpers | <o:p> </o:p> | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft-Web-helper è stata sostituita con Microsoft.AspNet.WebHelpers. È necessario rimuovere innanzitutto il pacchetto precedente e quindi installare il pacchetto più recente.   
    >   
    > Non è Nessuna compatibilità di versione tra i vari pacchetti principali di ASP.NET. Ad esempio, MVC 5 è compatibile con solo 3 Razor e non 2 Razor.
4. Aprire il progetto in Visual Studio 2013.
5. Rimuovere i seguenti pacchetti NuGet ASP.NET installati. Si rimuoverà le using Package Manager Console (console di gestione pacchetti). Per aprire la console di gestione pacchetti, selezionare il **degli strumenti** menu e quindi selezionare **Gestione pacchetti libreria** quindi selezionare **Console di gestione pacchetti**. Il progetto potrebbe non includere tutti questi componenti.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Questo pacchetto viene in genere aggiunto l'aggiornamento da MVC 3 a MVC 4. Per rimuoverlo, eseguire il comando seguente nella console di gestione pacchetti:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Questo pacchetto branding `Microsoft.AspNet.WebHelpers`. Per rimuoverlo, eseguire il comando seguente nella console di gestione pacchetti:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Questo pacchetto contiene una soluzione alternativa per un bug in MVC 4 che è stato risolto in MVC 5. Per rimuoverlo, eseguire il comando seguente nella console di gestione pacchetti:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Eseguire l'aggiornamento di tutti i pacchetti NuGet ASP.NET usando la console di gestione pacchetti. Nella console di gestione pacchetti, eseguire il comando seguente:  
    `Update-Package`  
   Il `Update-Package` comando senza alcun parametro aggiornerà tutti i pacchetti. È possibile aggiornare singolarmente i pacchetti tramite l'argomento di ID. Per altre informazioni sul comando di aggiornamento, eseguire `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Aggiornare l'applicazione *Web. config* File

Assicurarsi di apportare queste modifiche nell'app *Web. config* file, non il *Web. config* del file nei *viste* cartella.

Individuare il `<runtime>/<assemblyBinding>` sezione e apportare le modifiche seguenti:

1. Negli elementi con l'attributo di nome "System", impostare il numero di versione da "4.0.0.0" su "versione=5.0.0.0". (Due modifiche in tale elemento).
2. Negli elementi con l'attributo name &quot;spazi "e &quot;System.Web.WebPages&quot; impostare il numero di versione da"2.0.0.0"su"3.0.0.0". Si verifica quattro modifiche, due in ognuno degli elementi.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Individuare il `<appSettings>` sezione e aggiornare il webpages:version da 2.0.0.0.0 a 3.0.0.0, come illustrato di seguito:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Rimuovere eventuali livelli di attendibilità totale. Ad esempio:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Aggiorna il *Web. config* i file nella cartella Views

Se l'applicazione usa le aree, è necessario anche aggiornare ognuno *Web. config* del file nei *viste* sottocartella di ogni cartella Area.

1. Aggiornare tutti gli elementi che contengono "System" dalla versione "4.0.0.0" versione "versione=5.0.0.0".  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Aggiornare tutti gli elementi che contengono "Webpages" dalla versione "2.0.0.0" versione "3.0.0.0". Se in questa sezione contiene "System.Web.WebPages", aggiornare gli elementi dalla versione "2.0.0.0" versione "3.0.0.0"  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Se è stato rimosso il `Microsoft-Web-Helpers` installare il pacchetto NuGet in un passaggio precedente, `Microsoft.AspNet.WebHelpers` con il comando seguente nella console di gestione pacchetti:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Se l'app Usa la [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) metodo, aggiungere quanto segue per il *Web. config* file.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Passaggi finali

Compilare e testare l'applicazione.

Rimuovere il tipo di progetto MVC 4 GUID dai file di progetto.

1. In Esplora soluzioni fare doppio clic il nome del progetto e quindi selezionare **Scarica progetto**.
2. Fare clic sul progetto e scegliere Modifica NomeProgetto.
3. Individuare il `ProjectTypeGuids` elemento e quindi GUID, del progetto MVC 4 remove `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Salvare e chiudere il file di progetto aperto.
5. Fare clic sul progetto e selezionare **Ricarica progetto**.
