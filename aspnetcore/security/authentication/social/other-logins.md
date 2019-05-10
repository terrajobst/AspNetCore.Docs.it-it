---
title: Provider di autenticazione OAuth esterno
author: rick-anderson
description: Informazioni su provider di autenticazione OAuth esterno che funzionano con le app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/otherlogins
ms.openlocfilehash: b69c366ec1bf12ccf434991fc8a79eaf8c09da3d
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897508"
---
# <a name="external-oauth-authentication-providers"></a><span data-ttu-id="e3c0a-103">Provider di autenticazione OAuth esterno</span><span class="sxs-lookup"><span data-stu-id="e3c0a-103">External OAuth authentication providers</span></span>

<span data-ttu-id="e3c0a-104">Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), e [Valeriy Novytskyy](https://github.com/01binary)</span><span class="sxs-lookup"><span data-stu-id="e3c0a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), and [Valeriy Novytskyy](https://github.com/01binary)</span></span>

<span data-ttu-id="e3c0a-105">Nell'elenco seguente include comuni provider di autenticazione esterni OAuth che funzionano con le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e3c0a-105">The following list includes common external OAuth authentication providers that work with ASP.NET Core apps.</span></span> <span data-ttu-id="e3c0a-106">I pacchetti NuGet di terze parti, ad esempio quelli gestiti da [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth), può essere utilizzato per integrare il provider di autenticazione implementato dal team di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e3c0a-106">Third-party NuGet packages, such as the ones maintained by [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth), can be used to complement the authentication providers implemented by the ASP.NET Core team.</span></span>

* <span data-ttu-id="e3c0a-107">[LinkedIn](https://www.linkedin.com/developer/apps) ([istruzioni](https://developer.linkedin.com/docs/oauth2))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-107">[LinkedIn](https://www.linkedin.com/developer/apps) ([Instructions](https://developer.linkedin.com/docs/oauth2))</span></span>

* <span data-ttu-id="e3c0a-108">[Instagram](https://www.instagram.com/developer/register/) ([istruzioni](https://www.instagram.com/developer/authentication/))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-108">[Instagram](https://www.instagram.com/developer/register/) ([Instructions](https://www.instagram.com/developer/authentication/))</span></span>

* <span data-ttu-id="e3c0a-109">[Reddit](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps) ([istruzioni](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-109">[Reddit](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps) ([Instructions](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example))</span></span>

* <span data-ttu-id="e3c0a-110">[Github](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew) ([istruzioni](https://developer.github.com/v3/oauth/))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-110">[Github](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew) ([Instructions](https://developer.github.com/v3/oauth/))</span></span>

* <span data-ttu-id="e3c0a-111">[Yahoo](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F) ([istruzioni](https://developer.yahoo.com/bbauth/user.html))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-111">[Yahoo](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F) ([Instructions](https://developer.yahoo.com/bbauth/user.html))</span></span>

* <span data-ttu-id="e3c0a-112">[Tumblr](https://www.tumblr.com/oauth/apps) ([istruzioni](https://www.tumblr.com/docs/api/v2#auth))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-112">[Tumblr](https://www.tumblr.com/oauth/apps) ([Instructions](https://www.tumblr.com/docs/api/v2#auth))</span></span>

* <span data-ttu-id="e3c0a-113">[Pinterest](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F) ([istruzioni](https://developers.pinterest.com/docs/api/overview/?))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-113">[Pinterest](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F) ([Instructions](https://developers.pinterest.com/docs/api/overview/?))</span></span>

* <span data-ttu-id="e3c0a-114">[Pocket](https://getpocket.com/developer/apps/new) ([istruzioni](https://getpocket.com/developer/docs/authentication))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-114">[Pocket](https://getpocket.com/developer/apps/new) ([Instructions](https://getpocket.com/developer/docs/authentication))</span></span>

* <span data-ttu-id="e3c0a-115">[Flickr](https://www.flickr.com/services/apps/create) ([istruzioni](https://www.flickr.com/services/api/auth.oauth.html))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-115">[Flickr](https://www.flickr.com/services/apps/create) ([Instructions](https://www.flickr.com/services/api/auth.oauth.html))</span></span>

* <span data-ttu-id="e3c0a-116">[Dribble](https://dribbble.com/signup) ([istruzioni](http://developer.dribbble.com/v1/oauth/))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-116">[Dribble](https://dribbble.com/signup) ([Instructions](http://developer.dribbble.com/v1/oauth/))</span></span>

* <span data-ttu-id="e3c0a-117">[Vimeo](https://vimeo.com/join) ([istruzioni](https://developer.vimeo.com/api/authentication))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-117">[Vimeo](https://vimeo.com/join) ([Instructions](https://developer.vimeo.com/api/authentication))</span></span>

* <span data-ttu-id="e3c0a-118">[SoundCloud](https://soundcloud.com/you/apps/new) ([Instructions](https://developers.soundcloud.com/blog/we-love-oauth-2))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-118">[SoundCloud](https://soundcloud.com/you/apps/new) ([Instructions](https://developers.soundcloud.com/blog/we-love-oauth-2))</span></span>

* <span data-ttu-id="e3c0a-119">[VK](https://vk.com/apps?act=manage) ([istruzioni](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites))</span><span class="sxs-lookup"><span data-stu-id="e3c0a-119">[VK](https://vk.com/apps?act=manage) ([Instructions](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites))</span></span>

[!INCLUDE[Multiple authentication providers](includes/chain-auth-providers.md)]

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
