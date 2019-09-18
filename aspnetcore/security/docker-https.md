---
title: Hosting di immagini ASP.NET Core con Docker su HTTPS
author: rick-anderson
description: Informazioni su come ospitare ASP.NET Core immagini con Docker su HTTPS
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
uid: security/docker-https
ms.openlocfilehash: c13ba02845eef5c53a939feec2be8a01bc4ca128
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082537"
---
# <a name="hosting-aspnet-core-images-with-docker-over-https"></a>Hosting di immagini ASP.NET Core con Docker su HTTPS

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core utilizza [https per impostazione predefinita](/aspnet/core/security/enforcing-ssl). [Https](https://en.wikipedia.org/wiki/HTTPS) si basa su [certificati](https://en.wikipedia.org/wiki/Public_key_certificate) per l'attendibilità, l'identità e la crittografia.

Questo documento illustra come eseguire le immagini del contenitore predefinite con HTTPS.

Per gli scenari di sviluppo, vedere [sviluppo di applicazioni ASP.NET Core con Docker su HTTPS](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md) .

Questo esempio richiede [docker 17,06](https://docs.docker.com/release-notes/docker-ce) o versione successiva del [client Docker](https://www.docker.com/products/docker).

## <a name="prerequisites"></a>Prerequisiti

Per alcune istruzioni di questo documento è necessario [.NET Core 2,2 SDK](https://www.microsoft.com/net/download) o versione successiva.

## <a name="certificates"></a>Certificati

Un certificato di un' [autorità di certificazione](https://en.wikipedia.org/wiki/Certificate_authority) è necessario per l' [hosting di produzione](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) per un dominio.  Verrà [ora crittografata](https://letsencrypt.org/) un'autorità di certificazione che offre certificati gratuiti.

Questo documento usa [certificati di sviluppo autofirmati](https://en.wikipedia.org/wiki/Self-signed_certificate) per l'hosting di immagini predefinite `localhost`rispetto a. Le istruzioni sono simili all'uso dei certificati di produzione.

Per i certificati di produzione:

* Lo `dotnet dev-certs` strumento non è obbligatorio.
* Non è necessario archiviare i certificati nel percorso utilizzato nelle istruzioni. Qualsiasi località dovrebbe funzionare, anche se non è consigliabile archiviare i certificati nella directory del sito.

Il volume delle istruzioni monta i certificati in contenitori. È possibile aggiungere certificati alle immagini del contenitore con `COPY` un comando in un Dockerfile. Non è consigliabile copiare i certificati in un'immagine:

* Risulta difficile usare la stessa immagine per i test con certificati per sviluppatori.
* Risulta difficile utilizzare la stessa immagine per l'hosting con certificati di produzione.
* Il rischio di divulgazione dei certificati è significativo.

## <a name="running-pre-built-container-images-with-https"></a>Esecuzione di immagini del contenitore predefinite con HTTPS

Usare le istruzioni seguenti per la configurazione del sistema operativo.

### <a name="windows-using-linux-containers"></a>Windows con contenitori Linux

Generare il certificato e configurare il computer locale:

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

Nei comandi precedenti sostituire `{ password here }` con una password.

Eseguire l'immagine del contenitore con ASP.NET Core configurato per HTTPS:

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

La password deve corrispondere alla password utilizzata per il certificato.

### <a name="macos-or-linux"></a>macOS o Linux

Generare il certificato e configurare il computer locale:

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

`dotnet dev-certs https --trust`è supportato solo in macOS e Windows. È necessario considerare attendibili i certificati in Linux nel modo supportato dalla distribuzione. È probabile che sia necessario considerare attendibile il certificato nel browser.

Nei comandi precedenti sostituire `{ password here }` con una password.

Eseguire l'immagine del contenitore con ASP.NET Core configurato per HTTPS:

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v ${HOME}/.aspnet/https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

La password deve corrispondere alla password utilizzata per il certificato.

### <a name="windows-using-windows-containers"></a>Windows con i contenitori di Windows

Generare il certificato e configurare il computer locale:

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

Nei comandi precedenti sostituire `{ password here }` con una password.

Eseguire l'immagine del contenitore con ASP.NET Core configurato per HTTPS:

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=\https\aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:C:\https\ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

La password deve corrispondere alla password utilizzata per il certificato.