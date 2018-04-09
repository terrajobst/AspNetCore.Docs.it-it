---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Come aggiornare un ASP.NET MVC 4 e API di progetto di Web API 2 e MVC ASP.NET 5 Web | Documenti Microsoft
author: Rick-Anderson
description: ASP.NET MVC 5 e l'API Web 2 portare un host di nuove funzionalità, tra cui routing degli attributi, filtri di autenticazione e molto altro ancora.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f61502933a5ba92896ee97cef9cff915fe23831d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Come aggiornare un ASP.NET MVC 4 e il progetto API Web MVC ASP.NET 5 e Web API 2
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 e l'API Web 2 portare un host di nuove funzionalità, tra cui routing degli attributi, filtri di autenticazione e molto altro ancora. Vedere [ https://www.asp.net/vnext ](https://www.asp.net/core) per altri dettagli.
> 
> Questa procedura dettagliata fornisce le istruzioni con i passaggi necessari per aggiornare l'applicazione alla versione più recente.  
> 
> > [!NOTE]
> > Vedere [ASP.NET e strumenti Web per note sulla versione di Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md) per informazioni sulle modifiche di rilievo rispetto MVC 4 e API Web alla versione successiva.
> 
>   
> 
> In questo articolo è stato scritto dal Youngjune Hong e Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Passaggi di aggiornamento

1. Eseguire il backup del progetto. Questa procedura dettagliata è necessario apportare modifiche al file di progetto, configurazione del pacchetto e il file Web. config.
2. Per l'aggiornamento dall'API Web a Web API 2, in Global. asax, modificare:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   in

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Assicurarsi che tutti i pacchetti che utilizzano i progetti sono compatibili con MVC 5 e l'API Web 2. La seguente tabella sono riportati MVC 4 e API Web relativi pacchetti che devono essere modificati. Se si dispone di un pacchetto che dipende da uno dei pacchetti elencati di seguito, contattare il server di pubblicazione per ottenere le versioni più recenti che sono compatibili con MVC 5 e l'API Web 2. Se si dispone del codice sorgente per tali pacchetti, è necessario ricompilarla con i nuovi assembly MVC 5 e 2 di API Web.   

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
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | <o:p> </o:p> | Rimosso |
    | Microsoft.AspNet.WebPages.Administration | <o:p> </o:p> | Rimosso |
    | Microsoft-Web-Helpers | <o:p> </o:p> | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Gli helper Web di Microsoft è stata sostituita con Microsoft.AspNet.WebHelpers. Rimuovere innanzitutto il pacchetto precedente e quindi installare il pacchetto più recente.   
    >   
    > Non vi è Nessuna compatibilità di versione tra i vari pacchetti principali di ASP.NET. Ad esempio, MVC 5 è compatibile con solo 3 Razor e non Razor 2.
4. Aprire il progetto in Visual Studio 2013.
5. Rimuovere i seguenti pacchetti NuGet di ASP.NET installati. Utilizzare la Console di gestione pacchetto (PMC) saranno rimossi. Per aprire PMC, selezionare il **strumenti** menu e quindi selezionare **Gestione pacchetti di librerie,** selezionare **Package Manager Console**. Il progetto potrebbe non includere tutti i componenti.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Questo pacchetto viene in genere aggiunto durante l'aggiornamento da MVC 3 a MVC 4. Per rimuoverlo, eseguire il comando seguente in PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Questo pacchetto di branding come `Microsoft.AspNet.WebHelpers`. Per rimuoverlo, eseguire il comando seguente in PMC:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Questo pacchetto contiene una soluzione alternativa per un bug in MVC 4 che è stato risolto in MVC 5. Per rimuoverlo, eseguire il comando seguente in PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Aggiornare tutti i pacchetti di NuGet ASP.NET utilizzando PMC. In PMC, eseguire il comando seguente:  
    `Update-Package`  
   Il `Update-Package` comando senza parametri aggiornerà tutti i pacchetti. È possibile aggiornare i pacchetti singolarmente tramite l'argomento di tipo ID. Per ulteriori informazioni sul comando di aggiornamento, eseguire `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Aggiornare l'applicazione *Web. config* File

Assicurarsi di effettuare queste modifiche nell'app *Web. config* file, non il *Web. config* file nel *viste* cartella.

Individuare il `<runtime>/<assemblyBinding>` sezione e apportare le modifiche seguenti:

1. Negli elementi con l'attributo di nome "System", modificare il numero di versione da "4.0.0.0" a "5.0.0.0". (Due modifiche di quell'elemento).
2. Negli elementi con l'attributo nome &quot;System.Web.Helpers "e &quot;System.Web.WebPages&quot; modificare il numero di versione da"2.0.0.0"a"3.0.0.0". Si verifica quattro modifiche, due in ognuno degli elementi.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Individuare il `<appSettings>` sezione e aggiornare il webpages:version da 2.0.0.0.0 a 3.0.0.0, come illustrato di seguito:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Rimuovere eventuali livelli di attendibilità totale. Ad esempio:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Aggiornamento di *Web. config* file presenti nella cartella viste

Se l'applicazione utilizza le aree, sarà anche necessario aggiornare ognuno *Web. config* file nel *viste* sottocartella della cartella ogni Area.

1. Aggiornare tutti gli elementi che contengono "System" dalla versione "4.0.0.0" alla versione "5.0.0.0".  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Aggiornare tutti gli elementi che contengono "Webpages" dalla versione "2.0.0.0" alla versione "3.0.0.0". Se in questa sezione contiene "System.Web.WebPages", aggiornare gli elementi della versione "2.0.0.0" alla versione "3.0.0.0"  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Se è stato rimosso il `Microsoft-Web-Helpers` installazione del pacchetto NuGet in un passaggio precedente, `Microsoft.AspNet.WebHelpers` con il comando seguente in PMC:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Se l'app Usa la [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) (metodo), il comando seguente per aggiungere il *Web. config* file.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Passaggi finali

Compilare e testare l'applicazione.

Rimuovere il tipo di progetto MVC 4 GUID dei file di progetto.

1. In Esplora soluzioni fare doppio clic sul nome del progetto e quindi selezionare **Scarica progetto**.
2. Fare clic sul progetto e scegliere Modifica NomeProgetto.csproj.
3. Individuare il `ProjectTypeGuids` elemento e quindi GUID di progetto MVC 4 remove `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Salvare e chiudere il file di progetto aperto.
5. Fare clic sul progetto e selezionare **Ricarica progetto**.
