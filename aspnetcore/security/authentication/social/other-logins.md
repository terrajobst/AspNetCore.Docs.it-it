---
title: Sondaggio di altri provider di autenticazione.
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 11/3/2016
ms.topic: article
ms.assetid: BC36CA84-3DE8-496E-9AA2-2F1B74AE8309
ms.prod: asp.net-core
uid: security/authentication/otherlogins
ms.openlocfilehash: 421db8c89e01cebba0c1f98cc9286288a75777f2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="short-survey-of-other-authentication-providers"></a><span data-ttu-id="1e0b3-102">Sondaggio di altri provider di autenticazione</span><span class="sxs-lookup"><span data-stu-id="1e0b3-102">Short survey of other authentication providers</span></span>

<a name=security-authentication-other-logins></a>

<span data-ttu-id="1e0b3-103">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), e [Valeriy Novytskyy](https://github.com/01binary)</span><span class="sxs-lookup"><span data-stu-id="1e0b3-103">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), and [Valeriy Novytskyy](https://github.com/01binary)</span></span>

<span data-ttu-id="1e0b3-104">Qui vengono configurate le istruzioni per altri provider OAuth comuni.</span><span class="sxs-lookup"><span data-stu-id="1e0b3-104">Here are set up instructions for some other common OAuth providers.</span></span> <span data-ttu-id="1e0b3-105">Pacchetti NuGet di terze parti, ad esempio quelli gestiti da [aspnet pensionistici](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) può essere utilizzato per integrare il provider di autenticazione implementato dal team di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e0b3-105">Third-party NuGet packages such as the ones maintained by [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) can be used to complement authentication providers implemented by the ASP.NET Core team.</span></span>

* <span data-ttu-id="1e0b3-106">Impostare **LinkedIn** Accedi: [https://developer.linkedin.com/my-apps](https://developer.linkedin.com/my-apps).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-106">Set up **LinkedIn** sign in: [https://developer.linkedin.com/my-apps](https://developer.linkedin.com/my-apps).</span></span> <span data-ttu-id="1e0b3-107">Vedere [passaggi ufficiali](https://developer.linkedin.com/docs/oauth2).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-107">See [official steps](https://developer.linkedin.com/docs/oauth2).</span></span>

* <span data-ttu-id="1e0b3-108">Impostare **Instagram** Accedi: [https://www.instagram.com/developer/clients/manage](https://www.instagram.com/developer/clients/manage).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-108">Set up **Instagram** sign in: [https://www.instagram.com/developer/clients/manage](https://www.instagram.com/developer/clients/manage).</span></span> <span data-ttu-id="1e0b3-109">Vedere [passaggi ufficiali](https://www.instagram.com/developer/authentication/).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-109">See [official steps](https://www.instagram.com/developer/authentication/).</span></span>

* <span data-ttu-id="1e0b3-110">Impostare **Reddit** Accedi: [https://www.reddit.com/prefs/apps](https://www.reddit.com/prefs/apps).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-110">Set up **Reddit** sign in: [https://www.reddit.com/prefs/apps](https://www.reddit.com/prefs/apps).</span></span> <span data-ttu-id="1e0b3-111">Vedere [passaggi ufficiali](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-111">See [official steps](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example).</span></span>

* <span data-ttu-id="1e0b3-112">Impostare **Github** Accedi: [https://github.com/settings/applications/new](https://github.com/settings/applications/new).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-112">Set up **Github** sign in: [https://github.com/settings/applications/new](https://github.com/settings/applications/new).</span></span> <span data-ttu-id="1e0b3-113">Vedere [passaggi ufficiali](https://developer.github.com/v3/oauth/).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-113">See [official steps](https://developer.github.com/v3/oauth/).</span></span>

* <span data-ttu-id="1e0b3-114">Impostare **Yahoo** Accedi: [https://developer.yahoo.com/apps/create/](https://developer.yahoo.com/apps/create/).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-114">Set up **Yahoo** sign in: [https://developer.yahoo.com/apps/create/](https://developer.yahoo.com/apps/create/).</span></span> <span data-ttu-id="1e0b3-115">Vedere [passaggi ufficiali](https://developer.yahoo.com/bbauth/user.html).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-115">See [official steps](https://developer.yahoo.com/bbauth/user.html).</span></span>

* <span data-ttu-id="1e0b3-116">Impostare **Tumblr** Accedi: [https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-116">Set up **Tumblr** sign in: [https://www.tumblr.com/oauth/apps](https://www.tumblr.com/oauth/apps).</span></span> <span data-ttu-id="1e0b3-117">Vedere [passaggi ufficiali](https://www.tumblr.com/docs/en/api/v2#auth).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-117">See [official steps](https://www.tumblr.com/docs/en/api/v2#auth).</span></span>

* <span data-ttu-id="1e0b3-118">Impostare **Pinterest** Accedi: [https://developers.pinterest.com/apps](https://developers.pinterest.com/apps).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-118">Set up **Pinterest** sign in: [https://developers.pinterest.com/apps](https://developers.pinterest.com/apps).</span></span> <span data-ttu-id="1e0b3-119">Vedere [passaggi ufficiali](https://developers.pinterest.com/docs/api/overview/?).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-119">See [official steps](https://developers.pinterest.com/docs/api/overview/?).</span></span>

* <span data-ttu-id="1e0b3-120">Impostare **Pocket** Accedi: [http://getpocket.com/developer/apps/new](http://getpocket.com/developer/apps/new).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-120">Set up **Pocket** sign in: [http://getpocket.com/developer/apps/new](http://getpocket.com/developer/apps/new).</span></span> <span data-ttu-id="1e0b3-121">Vedere [passaggi ufficiali](https://getpocket.com/developer/docs/authentication).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-121">See [official steps](https://getpocket.com/developer/docs/authentication).</span></span>

* <span data-ttu-id="1e0b3-122">Impostare **Flickr** Accedi: [https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-122">Set up **Flickr** sign in: [https://www.flickr.com/services/apps/create](https://www.flickr.com/services/apps/create).</span></span> <span data-ttu-id="1e0b3-123">Vedere [passaggi ufficiali](https://www.flickr.com/services/api/auth.oauth.html).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-123">See [official steps](https://www.flickr.com/services/api/auth.oauth.html).</span></span>

* <span data-ttu-id="1e0b3-124">Impostare **Dribble** Accedi: [https://dribbble.com/signup](https://dribbble.com/signup).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-124">Set up **Dribble** sign in: [https://dribbble.com/signup](https://dribbble.com/signup).</span></span> <span data-ttu-id="1e0b3-125">Vedere [passaggi ufficiali](http://developer.dribbble.com/v1/oauth/).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-125">See [official steps](http://developer.dribbble.com/v1/oauth/).</span></span>

* <span data-ttu-id="1e0b3-126">Impostare **Vimeo** Accedi: [https://developer.vimeo.com/apps](https://developer.vimeo.com/apps).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-126">Set up **Vimeo** sign in: [https://developer.vimeo.com/apps](https://developer.vimeo.com/apps).</span></span> <span data-ttu-id="1e0b3-127">Vedere [passaggi ufficiali](https://developer.vimeo.com/api/authentication).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-127">See [official steps](https://developer.vimeo.com/api/authentication).</span></span>

* <span data-ttu-id="1e0b3-128">Impostare **SoundCloud** Accedi: [http://soundcloud.com/you/apps/new](http://soundcloud.com/you/apps/new).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-128">Set up **SoundCloud** sign in: [http://soundcloud.com/you/apps/new](http://soundcloud.com/you/apps/new).</span></span> <span data-ttu-id="1e0b3-129">Vedere [passaggi ufficiali](https://developers.soundcloud.com/blog/we-love-oauth-2).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-129">See [official steps](https://developers.soundcloud.com/blog/we-love-oauth-2).</span></span>

* <span data-ttu-id="1e0b3-130">Impostare **VK** Accedi: [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-130">Set up **VK** sign in: [https://vk.com/apps?act=manage](https://vk.com/apps?act=manage).</span></span> <span data-ttu-id="1e0b3-131">Vedere [passaggi ufficiali](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites).</span><span class="sxs-lookup"><span data-stu-id="1e0b3-131">See [official steps](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites).</span></span>
