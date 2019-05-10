---
title: Introduzione a gRPC su ASP.NET Core
author: juntaoluo
description: Informazioni sui servizi gRPC con il server Kestrel e lo stack di ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
ms.openlocfilehash: dd1c42744bfda965df91ea1fcc0b71814317b969
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085545"
---
# <a name="introduction-to-grpc-on-aspnet-core"></a><span data-ttu-id="61bc9-103">Introduzione a gRPC su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61bc9-103">Introduction to gRPC on ASP.NET Core</span></span>

<span data-ttu-id="61bc9-104">A cura di [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="61bc9-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="61bc9-105">[gRPC](https://grpc.io/docs/guides/) è un framework RPC indipendente dal linguaggio a elevate prestazioni.</span><span class="sxs-lookup"><span data-stu-id="61bc9-105">[gRPC](https://grpc.io/docs/guides/) is a language agnostic, high-performance Remote Procedure Call (RPC) framework.</span></span> <span data-ttu-id="61bc9-106">Per altre nozioni fondamentali su gRPC, vedere la [pagina della documentazione di gRPC](https://grpc.io/docs/).</span><span class="sxs-lookup"><span data-stu-id="61bc9-106">For more on gRPC fundamentals, see the [gRPC documentation page](https://grpc.io/docs/).</span></span>

<span data-ttu-id="61bc9-107">I principali vantaggi di gRPC sono:</span><span class="sxs-lookup"><span data-stu-id="61bc9-107">The main benefits of gRPC are:</span></span>
* <span data-ttu-id="61bc9-108">Framework RPC leggero e moderno ad alte prestazioni.</span><span class="sxs-lookup"><span data-stu-id="61bc9-108">Modern high-performance lightweight RPC framework.</span></span>
* <span data-ttu-id="61bc9-109">Sviluppo di API con priorità al contratto usando i buffer del protocollo per impostazione predefinita e implementazioni indipendenti dal linguaggio.</span><span class="sxs-lookup"><span data-stu-id="61bc9-109">Contract-first API development, using Protocol Buffers by default, allowing for language agnostic implementations.</span></span>
* <span data-ttu-id="61bc9-110">Strumenti disponibili per molte linguaggi consentono di generare client e server fortemente tipizzati.</span><span class="sxs-lookup"><span data-stu-id="61bc9-110">Tooling available for many languages to generate strongly-typed servers and clients.</span></span>
* <span data-ttu-id="61bc9-111">Supporto per chiamate client, server e di streaming bidirezionale.</span><span class="sxs-lookup"><span data-stu-id="61bc9-111">Supports client, server, and bi-directional streaming calls.</span></span>
* <span data-ttu-id="61bc9-112">Utilizzo di rete ridotto grazie alla serializzazione binaria Protobuf.</span><span class="sxs-lookup"><span data-stu-id="61bc9-112">Reduced network usage with Protobuf binary serialization.</span></span>

<span data-ttu-id="61bc9-113">Questi vantaggi rendono gRPC ideale per:</span><span class="sxs-lookup"><span data-stu-id="61bc9-113">These benefits make gRPC ideal for:</span></span>
* <span data-ttu-id="61bc9-114">Microservizi leggeri in cui l'efficienza è fondamentale.</span><span class="sxs-lookup"><span data-stu-id="61bc9-114">Lightweight microservices where efficiency is critical.</span></span>
* <span data-ttu-id="61bc9-115">Sistemi poliglotti che richiedono l'uso di più linguaggi per lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="61bc9-115">Polyglot systems where multiple languages are required for development.</span></span>
* <span data-ttu-id="61bc9-116">Servizi in tempo reale da punto a punto che devono gestire richieste o risposte di streaming.</span><span class="sxs-lookup"><span data-stu-id="61bc9-116">Point-to-point real-time services that need to handle streaming requests or responses.</span></span>

<span data-ttu-id="61bc9-117">Nonostante nella [pagina gRPC](https://grpc.io/docs/quickstart/csharp.html) ufficiale sia attualmente disponibile un'implementazione C#, l'implementazione corrente si basa sulla libreria nativa scritta in C ([C-core](https://grpc.io/blog/grpc-stacks) gRPC).</span><span class="sxs-lookup"><span data-stu-id="61bc9-117">While a C# implementation is currently available on the official [gRPC page](https://grpc.io/docs/quickstart/csharp.html), the current implementation relies on the native library written in C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span></span> <span data-ttu-id="61bc9-118">È attualmente in corso la preparazione di una nuova implementazione completamente gestita basata sul server HTTP Kestrel e sullo stack di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61bc9-118">Work is currently in progress to provide a new implementation based on the Kestrel HTTP server and the ASP.NET Core stack that is fully managed.</span></span> <span data-ttu-id="61bc9-119">I documenti seguenti contengono un'introduzione alla creazione di servizi gRPC con questa nuova implementazione.</span><span class="sxs-lookup"><span data-stu-id="61bc9-119">The following documents provide an introduction to building gRPC services with this new implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61bc9-120">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="61bc9-120">Additional resources</span></span>

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>