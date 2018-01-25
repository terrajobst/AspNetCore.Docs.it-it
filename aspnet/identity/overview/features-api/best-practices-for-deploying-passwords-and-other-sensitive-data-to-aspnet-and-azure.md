---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Procedure consigliate per la distribuzione delle password e altri dati sensibili per ASP.NET e servizio App di Azure | Documenti Microsoft
author: Rick-Anderson
description: "Questa esercitazione viene illustrato come il codice è possibile archiviare e accedere alle informazioni protette in modo sicuro. L'aspetto più importante è che evitare di archiviare le password o altre sen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2015
ms.topic: article
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 995d9a088e3095f36a01d2adb19ec08e6a6d1b3e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Procedure consigliate per la distribuzione delle password e altri dati sensibili per ASP.NET e servizio App di Azure
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> Questa esercitazione viene illustrato come il codice è possibile archiviare e accedere alle informazioni protette in modo sicuro. L'aspetto più importante è evitare di archiviare le password o altri dati sensibili nel codice sorgente e non è consigliabile utilizzare i segreti di produzione in modalità di sviluppo e test.
> 
> Il codice di esempio è una semplice applicazione console processo Web e un'applicazione ASP.NET MVC che deve accedere a un database connection string password, Twilio, Google e SendGrid le chiavi di sicurezza.
> 
> In locale le impostazioni e PHP viene inoltre riportato.


- [Utilizzo delle password nell'ambiente di sviluppo](#pwd)
- [Utilizzo delle stringhe di connessione nell'ambiente di sviluppo](#con)
- [Applicazioni console processi Web](#wj)
- [Distribuzione di segreti in Azure](#da)
- [Note per On-Premise e PHP](#not)
- [Risorse aggiuntive](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Utilizzo delle password nell'ambiente di sviluppo

Le esercitazioni illustrano spesso i dati sensibili nel codice sorgente, probabilmente con un problema che non è consigliabile archiviare i dati sensibili nel codice sorgente. Ad esempio, my [app ASP.NET MVC 5 con posta elettronica e SMS 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) esercitazione vengono illustrate le operazioni seguenti nel *Web. config* file:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

Il *Web. config* file è il codice sorgente, pertanto questi segreti non devono mai essere archiviati in tale file. Fortunatamente, la `<appSettings>` elemento ha un `file` attributo che consente di specificare un file esterno che contiene le impostazioni di configurazione app sensibili. È possibile spostare tutti i segreti in un file esterno come file esterno non viene archiviato nella struttura ad albero di origine. Ad esempio, nel markup seguente, il file *AppSettingsSecrets.config* contiene tutti i segreti dell'app:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Il codice nel file esterno (*AppSettingsSecrets.config* in questo esempio), è lo stesso markup, vedere il *Web. config* file:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Il runtime di ASP.NET unisce il contenuto del file esterno con il markup in &lt;appSettings&gt; elemento. L'attributo di file viene ignorato se non viene trovato il file specificato.

> [!WARNING]
> Sicurezza: non si aggiunge il *config segreti* file al progetto o archiviarla nel controllo del codice sorgente. Per impostazione predefinita, Visual Studio imposta la `Build Action` a `Content`, ovvero il file viene distribuito. Per ulteriori informazioni vedere [perché non tutti i file nella cartella di progetto vengono distribuiti?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Sebbene sia possibile usare qualsiasi estensione per il *config segreti* file, è consigliabile mantenere *config*, come i file di configurazione non vengono gestiti da IIS. Si noti inoltre che il *AppSettingsSecrets.config* file è di due livelli di directory superiore dal *Web. config* file, pertanto è completamente fuori della directory della soluzione. Spostando il file dalla directory della soluzione, &quot;git aggiungere \* &quot; non verranno aggiunte al repository.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Utilizzo delle stringhe di connessione nell'ambiente di sviluppo

Visual Studio crea nuovi progetti ASP.NET che utilizzano [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB è stato creato in modo specifico per l'ambiente di sviluppo. Non richiede una password, pertanto non è necessario eseguire alcuna operazione per impedire che i segreti viene verificata nel codice sorgente. Alcuni team di sviluppo utilizzare le versioni complete di SQL Server (o altri DBMS) che richiedono una password.

È possibile utilizzare il `configSource` attributo per sostituire l'intero `<connectionStrings>` markup. A differenza di `<appSettings>` `file` attributo che unisce il markup, il `configSource` attributo sostituisce il markup. Nell'esempio di codice seguente viene illustrata la `configSource` attributo la *Web. config* file:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Se si utilizza il `configSource` attributo come illustrato in precedenza per spostare le stringhe di connessione a un file esterno e dispone di Visual Studio creare un nuovo sito web, non sarà in grado di rilevare si utilizza un database e non si otterrà l'opzione di configurazione del database quando si pu pubblica in Azure da Visual Studio. Se si utilizza il `configSource` attributo, è possibile utilizzare PowerShell per creare e distribuire il sito web e il database oppure è possibile creare il sito web e il database nel portale, prima della pubblicazione. Il [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) script creerà un nuovo sito web e un database.


> [!WARNING]
> Sicurezza: a differenza di *AppSettingsSecrets.config* file, il file di stringhe di connessione esterna deve essere nella stessa directory radice *Web. config* file, pertanto è necessario adottare le precauzioni per accertarsi di non archiviarlo nel repository del codice sorgente.


> [!NOTE]
> **Avviso di sicurezza nel file di segreti:** si consiglia di non utilizzare i segreti di produzione nel test e sviluppo. Utilizzo di password di produzione in prova o di sviluppo di perdite di segreti.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Applicazioni console processi Web

Il *app* file utilizzato da un'applicazione console non supporta i percorsi relativi, ma supporta percorsi assoluti. Per spostare i segreti dalla directory del progetto, è possibile utilizzare un percorso assoluto. Il markup seguente mostra i segreti nel *C:\secrets\AppSettingsSecrets.config* file e dati non sensibili nel *app* file.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Distribuzione di segreti in Azure

Quando si distribuisce l'app web in Azure, il *AppSettingsSecrets.config* file non verrà distribuito (che è quello desiderato). È possibile utilizzare il [il portale di gestione di Azure](https://azure.microsoft.com/services/management-portal/) e impostarli manualmente, a tale scopo:

1. Passare a [https://portal.azure.com](https://portal.azure.com)e accedere con le credenziali di Azure.
2. Fare clic su **Sfoglia &gt; App Web**, quindi fare clic sul nome dell'app web.
3. Fare clic su **tutte le impostazioni &gt; le impostazioni dell'applicazione**.

Il **impostazioni app** e **stringa di connessione** valori override le impostazioni del *Web. config* file. In questo esempio, si distribuzione non è riuscita queste impostazioni in Azure, ma se queste chiavi sono stati nel *Web. config* file, le impostazioni sul portale avrebbe la precedenza.

Una procedura consigliata consiste nel seguire un [flusso di lavoro DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) e utilizzare [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (o un altro framework, ad esempio [Chef](http://www.opscode.com/chef/) o [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) per automatizzare l'impostazione di questi valori in Azure. Il seguente script di PowerShell Usa [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) per esportare i segreti crittografati su disco:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

Nello script, 'Name' è il nome della chiave privata, ad esempio '&quot;FB\_AppSecret&quot; o "TwitterSecret". È possibile visualizzare il file ".credential" creato dallo script nel browser. Nel frammento seguente verifica ognuno dei file di credenziali e imposta i segreti per l'app web denominata:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Sicurezza: non includere password o altri segreti nello script di PowerShell, eseguire vanifica in questo caso lo scopo di utilizzo di uno script di PowerShell per distribuire i dati sensibili. Il [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet fornisce un meccanismo protetto per ottenere una password. Uso di una richiesta dell'interfaccia utente può impedire la perdita di una password.


### <a name="deploying-db-connection-strings"></a>Le stringhe di connessione di database di distribuzione

Le stringhe di connessione di database vengono gestite in modo analogo alle impostazioni app. Se si distribuisce l'app web da Visual Studio, la stringa di connessione verrà configurata automaticamente. È possibile verificare questa nel portale. È consigliabile impostare la stringa di connessione con PowerShell. Per un esempio di uno script di PowerShell di crea un sito Web e un database e imposta la stringa di connessione nel sito Web, scaricare [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) dal [libreria Script Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Note per PHP

Poiché le coppie chiave-valore per entrambi **impostazioni app** e **le stringhe di connessione** vengono archiviati nelle variabili di ambiente nel servizio App di Azure, gli sviluppatori che utilizzano facilmente qualsiasi può Framework (ad esempio PHP) di app web recuperare questi valori. Vedere di Stefan Schackow [siti Web di Azure: come le stringhe dell'applicazione e lavoro di stringhe di connessione](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) post di blog che mostra un frammento PHP per la lettura di stringhe di connessione e le impostazioni dell'app.

## <a name="notes-for-on-premises-servers"></a>Note per i server locali

Se si distribuiscono server web locale, ti consente di segreti protetti da [crittografare sezioni di configurazione di file di configurazione](https://msdn.microsoft.com/library/ff647398.aspx). In alternativa, è possibile utilizzare lo stesso approccio consigliato per siti Web di Azure: mantenere le impostazioni di sviluppo nei file di configurazione e utilizzare i valori di variabile di ambiente per le impostazioni di produzione. In questo caso, tuttavia, è necessario scrivere codice dell'applicazione per la funzionalità è automatica in siti Web di Azure: recuperare le impostazioni dalle variabili di ambiente e utilizzare tali valori al posto delle impostazioni del file di configurazione oppure le impostazioni di file di configurazione quando variabili di ambiente non vengono trovate.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

Per un esempio di un PowerShell script che crea un'app web e database, imposta la stringa di connessione + le impostazioni dell'app, download [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) dal [libreria Script Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Vedere di Stefan Schackow [siti Web di Azure di Windows: come le stringhe dell'applicazione e connessione funzionamento delle stringhe](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Ringraziamenti Barry Dorrans speciali ( [ @blowdart ](https://twitter.com/blowdart) ) e Carlos Farre per la revisione.
