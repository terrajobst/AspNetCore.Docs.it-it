---
title: Moduli IIS con ASP.NET Core
author: guardrex
description: Individuare i moduli IIS attivi e inattivi per le app ASP.NET Core e come gestire i moduli IIS.
ms.author: riande
ms.custom: mvc
ms.date: 10/12/2018
uid: host-and-deploy/iis/modules
ms.openlocfilehash: b417d479d0c3f8b3e739d4c72b52247de0e88e56
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325952"
---
# <a name="iis-modules-with-aspnet-core"></a>Moduli IIS con ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Alcuni dei moduli nativi e tutti i moduli gestiti di IIS non sono disponibili per l'elaborazione delle richieste per le app ASP.NET Core. In molti casi, ASP.NET Core offre un'alternativa agli scenari gestiti dai moduli nativi e gestiti di IIS.

## <a name="native-modules"></a>Moduli nativi

La tabella indica i moduli di IIS nativi che funzionano con le richieste del proxy inverso per le app ASP.NET Core.

| Modulo | Funzionante con le app ASP.NET Core | Opzione di ASP.NET Core |
| --- | :---: | --- |
| **Autenticazione anonima**<br>`AnonymousAuthenticationModule`                                  | Yes | |
| **Autenticazione base**<br>`BasicAuthenticationModule`                                          | Yes | |
| **Autenticazione mapping certificazione client**<br>`CertificateMappingAuthenticationModule`      | Yes | |
| **CGI**<br>`CgiModule`                                                                           | No  | |
| **Convalida della configurazione**<br>`ConfigurationValidationModule`                                  | Yes | |
| **Errori HTTP**<br>`CustomErrorModule`                                                           | No  | [Middleware delle tabelle codici di stato](xref:fundamentals/error-handling#configure-status-code-pages) |
| **Registrazione personalizzata**<br>`CustomLoggingModule`                                                      | Yes | |
| **Documento predefinito**<br>`DefaultDocumentModule`                                                  | No  | [Middleware dei file predefiniti](xref:fundamentals/static-files#serve-a-default-document) |
| **Autenticazione digest**<br>`DigestAuthenticationModule`                                        | Yes | |
| **Esplorazione directory**<br>`DirectoryListingModule`                                               | No  | [Middleware di esplorazione directory](xref:fundamentals/static-files#enable-directory-browsing) |
| **Compressione dinamica**<br>`DynamicCompressionModule`                                            | Yes | [Middleware di compressione delle risposte](xref:performance/response-compression) |
| **Traccia**<br>`FailedRequestsTracingModule`                                                     | Yes | [Registrazione di ASP.NET Core](xref:fundamentals/logging/index#tracesource-provider) |
| **Memorizzazione nella cache dei file**<br>`FileCacheModule`                                                            | No  | [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware) |
| **Memorizzazione nella cache HTTP**<br>`HttpCacheModule`                                                            | No  | [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware) |
| **Registrazione HTTP**<br>`HttpLoggingModule`                                                          | Yes | [Registrazione di ASP.NET Core](xref:fundamentals/logging/index) |
| **Reindirizzamento HTTP**<br>`HttpRedirectionModule`                                                  | Yes | [Middleware di riscrittura URL](xref:fundamentals/url-rewriting) |
| **Autenticazione mapping certificati client IIS**<br>`IISCertificateMappingAuthenticationModule` | Yes | |
| **Restrizioni per IP e domini**<br>`IpRestrictionModule`                                          | Yes | |
| **Filtri ISAPI**<br>`IsapiFilterModule`                                                         | Yes | [Middleware](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule`                                                                       | Yes | [Middleware](xref:fundamentals/middleware/index) |
| **Supporto del protocollo**<br>`ProtocolSupportModule`                                                  | Yes | |
| **Filtro richieste**<br>`RequestFilteringModule`                                                | Yes | [Middleware di riscrittura URL `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Monitoraggio richieste**<br>`RequestMonitorModule`                                                    | Yes | |
| **Riscrittura degli URL**&#8224;<br>`RewriteModule`                                                      | Yes | [Middleware di riscrittura URL](xref:fundamentals/url-rewriting) |
| **Server-Side Include**<br>`ServerSideIncludeModule`                                            | No  | |
| **Compressione statica**<br>`StaticCompressionModule`                                              | No  | [Middleware di compressione delle risposte](xref:performance/response-compression) |
| **Contenuto statico**<br>`StaticFileModule`                                                         | No  | [Middleware dei file statici](xref:fundamentals/static-files) |
| **Memorizzazione nella cache dei token**<br>`TokenCacheModule`                                                          | Yes | |
| **Memorizzazione nella cache degli URI**<br>`UriCacheModule`                                                              | Yes | |
| **Autorizzazione URL**<br>`UrlAuthorizationModule`                                                | Yes | [Identità di ASP.NET Core](xref:security/authentication/identity) |
| **Autenticazione di Windows**<br>`WindowsAuthenticationModule`                                      | Yes | |

&#8224;I tipi corrispondenti `isFile` e `isDirectory` di URL Rewrite Module non funzionano con le app ASP.NET Core a causa delle modifiche apportate alla [struttura di directory](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Moduli gestiti

I moduli gestiti *non* funzionano con le app ASP.NET Core ospitate quando la versione CLR .NET del pool di app è impostata su **Nessun codice gestito**. In molti casi, ASP.NET Core offre alternative a livello di middleware.

| Modulo                  | Opzione di ASP.NET Core |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Cookie del middleware di autenticazione](xref:security/authentication/cookie) |
| OutputCache             | [Middleware di memorizzazione nella cache delle risposte](xref:performance/caching/middleware) |
| Profilo                 | |
| RoleManager             | |
| ScriptModule-4.0        | |
| Sessione                 | [Middleware di sessione](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [Middleware di riscrittura URL](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | [Identità di ASP.NET Core](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>Modifiche dell'applicazione Gestione IIS

Quando si usa Gestione IIS per configurare le impostazioni, il file *web.config* dell'app viene modificato. Se si distribuisce un'app e si include *web.config*, le eventuali modifiche apportate con Gestione IIS vengono sovrascritte dal file *web.config* distribuito. Se vengono apportate modifiche al file *web.config* sul server, copiare immediatamente il file *web.config* aggiornato sul server nel progetto locale.

## <a name="disabling-iis-modules"></a>Disabilitazione dei moduli IIS

Se un modulo IIS configurato a livello di server deve essere disabilitato per un'app, un'aggiunta al file *web.config* dell'app consente di disabilitare il modulo. È possibile mantenere il modulo e disattivarlo tramite un'impostazione di configurazione (se disponibile) oppure rimuovere il modulo dall'app.

### <a name="module-deactivation"></a>Disattivazione del modulo

Molti moduli offrono un'impostazione di configurazione che consente di disabilitarli senza rimuovere il modulo dall'app. Questo è il modo più semplice e rapido per disattivare un modulo. Ad esempio, il modulo di reindirizzamento HTTP può essere disabilitato tramite l'elemento `<httpRedirect>` in *web.config*:

```xml
<configuration>
  <system.webServer>
    <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Per altre informazioni sulla disabilitazione dei moduli con le impostazioni di configurazione, usare i collegamenti nella sezione *Elementi figlio* di [IIS \<system.webServer>](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Rimozione dei moduli

Se si sceglie di rimuovere un modulo con un'impostazione in *web.config*, sbloccare innanzitutto il modulo e la sezione `<modules>` di *web.config*:

1. Sbloccare il modulo a livello di server. Selezionare il server IIS nella barra laterale **Connessioni** di Gestione IIS. Aprire **Moduli** nell'area **IIS**. Selezionare il modulo nell'elenco. Nella barra laterale **Azioni** sulla destra selezionare **Sblocca**. Sbloccare tutti i moduli che si prevede di rimuovere da *web.config* in un secondo momento.

2. Distribuire l'app senza una sezione `<modules>` in *web.config*. Se un'app viene distribuita con un file *web.config* che contiene la sezione `<modules>` senza aver prima sbloccato la sezione in Gestione IIS, Configuration Manager genera un'eccezione quando si tenta di sbloccare la sezione. Di conseguenza, distribuire l'app senza una sezione `<modules>`.

3. Sbloccare la sezione `<modules>` di *web.config*. Nella barra laterale **Connessioni** selezionare il sito Web in **Siti**. Nell'area **Gestione** aprire **Editor configurazione**. Usare i controlli di navigazione per selezionare la sezione `system.webServer/modules`. Nella barra laterale **Azioni** sulla destra selezionare **Sblocca** per la sezione.

4. A questo punto, è possibile aggiungere una sezione `<modules>` al file *web.config* con un elemento `<remove>` per rimuovere il modulo dall'app. È possibile aggiungere più elementi `<remove>` per rimuovere più moduli. Se le modifiche a *web.config* vengono apportate sul server, apportare immediatamente le stesse modifiche al file *web.config* del progetto in locale. La rimozione di un modulo in questo modo non influisce sull'uso del modulo con le altre app sul server.

   ```xml
   <configuration>
    <system.webServer>
      <modules>
        <remove name="MODULE_NAME" />
      </modules>
    </system.webServer>
   </configuration>
   ```

Un modulo IIS può anche essere rimosso con *Appcmd.exe*. Specificare `MODULE_NAME` e `APPLICATION_NAME` nel comando:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Ad esempio, rimuovere `DynamicCompressionModule` dal sito Web predefinito:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Configurazione minima dei moduli

I soli moduli necessari per eseguire un'app ASP.NET Core sono il modulo di autenticazione anonima e il modulo ASP.NET Core.

Il modulo di memorizzazione nella cache degli URI (`UriCacheModule`) consente a IIS di memorizzare nella cache la configurazione del sito Web a livello di URL. Senza questo modulo, IIS deve leggere e analizzare la configurazione a ogni richiesta, anche quando lo stesso URL viene richiesto ripetutamente. L'analisi della configurazione a ogni richiesta comporta una riduzione significativa delle prestazioni. *Anche se il modulo di memorizzazione nella cache degli URI non è strettamente necessario per l'esecuzione di un'app ASP.NET Core ospitata, è consigliabile abilitare il modulo di memorizzazione nella cache degli URI per tutte le distribuzioni di ASP.NET Core.*

Il modulo di memorizzazione nella cache HTTP (`HttpCacheModule`) implementa la cache di output di IIS e anche la logica per la memorizzazione degli elementi nella cache HTTP.sys. Senza questo modulo, il contenuto non viene più memorizzato nella cache in modalità kernel e i profili cache vengono ignorati. La rimozione del modulo di memorizzazione nella cache HTTP in genere ha effetti negativi sulle prestazioni e l'uso delle risorse. *Anche se il modulo di memorizzazione nella cache HTTP non è strettamente necessario per l'esecuzione di un'app ASP.NET Core ospitata, è consigliabile abilitare il modulo di memorizzazione nella cache HTTP per tutte le distribuzioni di ASP.NET Core.*

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:host-and-deploy/iis/index>
* [Introduction to IIS Architectures: Modules in IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis) (Introduzione alle architetture IIS: moduli di IIS)
* [IIS Modules Overview](/iis/get-started/introduction-to-iis/iis-modules-overview) (Panoramica dei moduli IIS)
* [Customizing IIS 7.0 Roles and Modules](https://technet.microsoft.com/library/cc627313.aspx) (Personalizzazione di ruoli e moduli di IIS 7.0)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
