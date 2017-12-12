---
title: Uso di moduli IIS con ASP.NET Core
author: guardrex
description: Documento di riferimento che descrive i moduli IIS attivi e inattivi per le applicazioni ASP.NET Core.
keywords: Componenti di base di ASP.NET, iis, modulo, proxy inverso
ms.author: riande
manager: wpickett
ms.date: 03/08/2017
ms.topic: article
ms.assetid: 492b3a7e-04c5-461b-b96a-38ecee5c64bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/iis-modules
ms.openlocfilehash: fee8e830ab43f731de9c90fad06b577662760f87
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="using-iis-modules-with-aspnet-core"></a>Uso di moduli IIS con ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Le applicazioni ASP.NET Core sono ospitate da IIS in una configurazione di proxy inverso. Alcuni dei moduli nativi IIS e tutti i moduli IIS gestiti non sono disponibili per elaborare le richieste per le applicazioni ASP.NET Core. In molti casi, ASP.NET Core offre un'alternativa alle funzionalità native e gestite dei moduli di IIS.

## <a name="native-modules"></a>Moduli nativi
Modulo | .NET core attivo | Opzione di ASP.NET Core
--- | :---: | ---
**Autenticazione anonima**<br>`AnonymousAuthenticationModule` | Sì | 
**Autenticazione base**<br>`BasicAuthenticationModule` | Sì | 
**Autenticazione Mapping certificazione di client**<br>`CertificateMappingAuthenticationModule` | Sì | 
**CGI**<br>`CgiModule` | No | 
**Convalida della configurazione**<br>`ConfigurationValidationModule` | Sì | 
**Errori HTTP**<br>`CustomErrorModule` | No | [Middleware pagine codice di stato](xref:fundamentals/error-handling#configuring-status-code-pages)
**Registrazioni personalizzate**<br>`CustomLoggingModule` | Sì | 
**Documento predefinito**<br>`DefaultDocumentModule` | No | [Middleware di file predefinito](xref:fundamentals/static-files#serving-a-default-document)
**Autenticazione del digest**<br>`DigestAuthenticationModule` | Sì | 
**Esplorazione directory**<br>`DirectoryListingModule` | No | [Middleware di esplorazione directory](xref:fundamentals/static-files#enabling-directory-browsing)
**Compressione dinamica**<br>`DynamicCompressionModule` | Sì | [Middleware di compressione delle risposte](xref:performance/response-compression)
**Traccia**<br>`FailedRequestsTracingModule` | Sì | [Registrazione di ASP.NET Core](xref:fundamentals/logging/index#the-tracesource-provider)
**La memorizzazione nella cache di file**<br>`FileCacheModule` | No | [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
**La memorizzazione nella cache di HTTP**<br>`HttpCacheModule` | No | [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
**Registrazione HTTP**<br>`HttpLoggingModule` | Sì | [Registrazione di ASP.NET Core](xref:fundamentals/logging/index)<br>Le implementazioni: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
**Reindirizzamento HTTP**<br>`HttpRedirectionModule` | Sì | [Middleware di riscrittura URL](xref:fundamentals/url-rewriting)
**Autenticazione Mapping certificati Client IIS**<br>`IISCertificateMappingAuthenticationModule` | Sì | 
**Restrizioni per IP e domini**<br>`IpRestrictionModule` | Sì | 
**Filtri ISAPI**<br>`IsapiFilterModule` | Sì | [Middleware](xref:fundamentals/middleware)
**ISAPI**<br>`IsapiModule` | Sì | [Middleware](xref:fundamentals/middleware)
**Supporto del protocollo**<br>`ProtocolSupportModule` | Sì | 
**Filtro richieste**<br>`RequestFilteringModule` | Sì | [Middleware di riscrittura URL`IRule`](xref:fundamentals/url-rewriting#irule-based-rule)
**Monitoraggio richieste**<br>`RequestMonitorModule` | Sì | 
**La riscrittura URL**<br>`RewriteModule` | Yes† | [Middleware di riscrittura URL](xref:fundamentals/url-rewriting)
**Server Side Includes**<br>`ServerSideIncludeModule` | No | 
**Compressione statica**<br>`StaticCompressionModule` | No | [Middleware di compressione delle risposte](xref:performance/response-compression)
**Contenuto statico**<br>`StaticFileModule` | No | [Middleware di File statici](xref:fundamentals/static-files)
**La memorizzazione nella cache token**<br>`TokenCacheModule` | Sì | 
**Memorizzazione nella cache URI**<br>`UriCacheModule` | Sì | 
**Autorizzazione URL**<br>`UrlAuthorizationModule` | Sì | [Identità di ASP.NET Core](xref:security/authentication/identity)
**Autenticazione di Windows**<br>`WindowsAuthenticationModule` | Sì | 

Del †the URL Rewrite Module `isFile` e `isDirectory` non funzionano con le applicazioni ASP.NET Core a causa di modifiche in [struttura di directory](xref:hosting/directory-structure).

## <a name="managed-modules"></a>Moduli gestiti
Modulo | .NET core attivo | Opzione di ASP.NET Core
--- | :---: | ---
AnonymousIdentification | No | 
DefaultAuthentication | No | 
FileAuthorization | No | 
FormsAuthentication | No | [Cookie di Middleware di autenticazione](xref:security/authentication/cookie)
OutputCache | No | [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware)
Profilo | No | 
RoleManager | No | 
ScriptModule 4.0 | No | 
Sessione | No | [Middleware di sessione](xref:fundamentals/app-state)
UrlAuthorization | No | 
UrlMappingsModule | No | [Middleware di riscrittura URL](xref:fundamentals/url-rewriting)
UrlRoutingModule 4.0 | No | [Identità di ASP.NET Core](xref:security/authentication/identity)
WindowsAuthentication | No | 

## <a name="iis-manager-application-changes"></a>Modifiche all'applicazione di gestione IIS
Quando si utilizza Gestione IIS per configurare le impostazioni, che si desidera modificare direttamente il *Web. config* file dell'app. Se si distribuisce l'app e includono *Web. config*, tutte le modifiche apportate con Gestione IIS verranno sovrascritto da distribuito *file Web. config*. Pertanto se si apportano modifiche al server *Web. config* file, copiare l'aggiornamento *Web. config* file al progetto locale immediatamente.

## <a name="disabling-iis-modules"></a>La disabilitazione di moduli IIS
Se si dispone di un modulo IIS configurato a livello di server che si desidera disabilitare per un'applicazione, è possibile farlo con un componente aggiuntivo per il *Web. config* file. Lasciare il modulo sul posto e disattivarlo tramite un'impostazione di configurazione (se disponibile) o rimuovere il modulo dall'app.

### <a name="module-deactivation"></a>Disattivazione di modulo
Molti moduli offrono un'impostazione di configurazione che consente di disabilitare le senza rimuoverli dall'applicazione. Questo è il modo più semplice e rapido per disattivare un modulo. Ad esempio, se si desidera disabilitare IIS URL Rewrite Module, utilizzare il `<httpRedirect>` elemento come illustrato di seguito. Per ulteriori informazioni sulla disabilitazione di moduli con le impostazioni di configurazione, seguire i collegamenti nel *gli elementi figlio* sezione [IIS `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/).

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>Rimozione del modulo
Se si sceglie di rimuovere un modulo con un'impostazione in *Web. config*, è necessario sbloccare il modulo e sbloccare il `<modules>` sezione *Web. config* prima. Di seguito sono descritti i passaggi:

1. Sbloccare il modulo a livello di server. Fare clic sul server IIS in Gestione IIS **connessioni** barra laterale. Aprire il **moduli** nel **IIS** area. Fare clic sul modulo nell'elenco. Nel **azioni** barra laterale a destra, fare clic su **Unlock**. Sbloccare tutti i moduli come si intende rimuovere con *Web. config* in un secondo momento.

2. Distribuire l'applicazione senza un `<modules>` sezione *Web. config*. Se si distribuisce un'app con un *Web. config* contenente il `<modules>` sezione senza avere sbloccato la sezione prima in Gestione IIS, Configuration Manager verrà generata un'eccezione quando si tenta di sbloccare la sezione. Pertanto, distribuire l'applicazione senza un `<modules>` sezione.

3. Sbloccare il `<modules>` sezione *Web. config*. Nel **connessioni** barra laterale, selezionare il sito Web in **siti**. Nel **Management** area, aprire il **Editor di configurazione**. Utilizzare i controlli di spostamento selezionare il `system.webServer/modules` sezione. Nel **azioni** barra laterale a destra, fare clic su **Unlock** la sezione.

4. A questo punto, sarà possibile aggiungere un `<modules>` sezione per la *Web. config* file con un `<remove>` elemento da rimuovere il modulo dall'applicazione. È possibile aggiungere più `<remove>` elementi da rimuovere più moduli. Tenere presente che se si apportano *Web. config* modifiche nel server per rendere immediatamente nel progetto in locale. Rimozione di un modulo in questo modo non influisce sull'uso del modulo con altre applicazioni nel server.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Per un'installazione di IIS con i moduli predefiniti installati, è possibile utilizzare la seguente `<module>` sezione per rimuovere i moduli predefiniti.

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

È inoltre possibile rimuovere un modulo IIS con *Appcmd.exe*. Fornire il `MODULE_NAME` e `APPLICATION_NAME` nella riga di comando mostrata di seguito:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Di seguito viene illustrato come rimuovere il `DynamicCompressionModule` dal sito Web predefinito:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimal-module-configuration"></a>Configurazione del modulo minima
I moduli soli necessari per eseguire un'applicazione ASP.NET di base sono il modulo di autenticazione anonima e il modulo di base di ASP.NET.

![Aprire Gestione IIS moduli con la configurazione minima modulo illustrata](iis-modules/_static/modules.png)

## <a name="resources"></a>Risorse
* [Pubblicazione in IIS](xref:publishing/iis)
* [Panoramica di moduli IIS](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Personalizzazione di IIS 7.0 ruoli e i moduli](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS`<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)
