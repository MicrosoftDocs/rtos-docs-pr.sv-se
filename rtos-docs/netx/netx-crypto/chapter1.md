---
title: Kapitel 1 – Introduktion till Azure återställnings tider NetX-kryptografi
description: NetX-kryptografi är en real tids implementering av kryptografiska algoritmer som har utformats för att tillhandahålla data krypterings-och autentiserings tjänster.
author: philmea
ms.author: philmea
ms.date: 05/19/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 0bde9be472584308894cfd702ccd014578afe753
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/22/2021
ms.locfileid: "104825596"
---
# <a name="chapter-1---introduction-to-azure-rtos-netx-crypto"></a><span data-ttu-id="7f747-103">Kapitel 1 – Introduktion till Azure återställnings tider NetX-kryptografi</span><span class="sxs-lookup"><span data-stu-id="7f747-103">Chapter 1 - Introduction to Azure RTOS NetX Crypto</span></span>

<span data-ttu-id="7f747-104">Azure återställnings tider NetX-kryptografi är en real tids implementering av kryptografiska algoritmer som är utformad för att tillhandahålla data krypterings-och autentiserings tjänster.</span><span class="sxs-lookup"><span data-stu-id="7f747-104">Azure RTOS NetX Crypto is a high-performance real-time implementation of cryptographic algorithms designed to provide data encryption and authentication services.</span></span> <span data-ttu-id="7f747-105">NetX-kryptografi är utformat för att ansluta för NetX säkra TLS-, DTLS-och IPsec-moduler.</span><span class="sxs-lookup"><span data-stu-id="7f747-105">NetX Crypto is designed to plug in for NetX Secure TLS, DTLS, and IPsec modules.</span></span> <span data-ttu-id="7f747-106">Program kan också använda NetX-kryptografi som en fristående modul utanför nätverks säkerheten.</span><span class="sxs-lookup"><span data-stu-id="7f747-106">Applications may also use NetX Crypto as a standalone module outside network security.</span></span>

## <a name="netx-crypto-unique-features"></a><span data-ttu-id="7f747-107">Unika funktioner för NetX-kryptografi</span><span class="sxs-lookup"><span data-stu-id="7f747-107">NetX Crypto Unique Features</span></span>

<span data-ttu-id="7f747-108">NetX-kryptering implementeras på standard språket C (C99), kompatibel med praktiskt taget alla C/C++-kompilatorer.</span><span class="sxs-lookup"><span data-stu-id="7f747-108">NetX Crypto is implemented in the standard C language (C99), compatible with virtually all C/C++ compilers.</span></span> <span data-ttu-id="7f747-109">Den modulära designen gör det möjligt för ett program att endast länka i de krypteringsalgoritmer som den behöver använda, vilket ger minimal kod storlek.</span><span class="sxs-lookup"><span data-stu-id="7f747-109">Its modular design allows an application to only link in the crypto algorithms it needs to use, therefore achieving minimal code size.</span></span> <span data-ttu-id="7f747-110">Implementeringen har utformats för att fungera med de flesta 32-bitars mikroprocessorer och använder bara de grundläggande matematiska åtgärderna (addition, subtraktion, multiplikation, Division, logisk och, eller, eller, eller, och bit växlings åtgärder).</span><span class="sxs-lookup"><span data-stu-id="7f747-110">The implementation is designed to work with most 32-bit microprocessors and uses only the basic math operations (addition, subtraction, multiplication, division, logical AND, OR, NOR, and bit shift operations).</span></span> <span data-ttu-id="7f747-111">Alla dessa åtgärder används med 32-bitars mängder, vilket gör NetX-kryptering portabelt över de flesta 32-bitars mikroprocessorer.</span><span class="sxs-lookup"><span data-stu-id="7f747-111">All these operations are used with 32-bit quantities, making NetX Crypto portable across most 32-bit microprocessors.</span></span> <span data-ttu-id="7f747-112">Implementeringen är särskilt optimerad för att köras på resurs begränsade mikroprocessorer, vilket riktar sig till djupt inbäddade program.</span><span class="sxs-lookup"><span data-stu-id="7f747-112">The implementation is specifically optimized to run on resource constrained microprocessors, targeting deeply embedded applications.</span></span>

## <a name="algorithms-supported-by-netx-crypto"></a><span data-ttu-id="7f747-113">Algoritmer som stöds av NetX-kryptografi</span><span class="sxs-lookup"><span data-stu-id="7f747-113">Algorithms supported by NetX Crypto</span></span>

<span data-ttu-id="7f747-114">NetX-kryptering stöder följande krypteringsalgoritmer.</span><span class="sxs-lookup"><span data-stu-id="7f747-114">NetX Crypto supports the following cryptographic algorithms.</span></span> <span data-ttu-id="7f747-115">NetX-kryptering följer alla allmänna rekommendationer och grundläggande krav inom begränsningar i ett operativ system med real tid och plattformar som kräver liten minnes storlek och effektiv körning.</span><span class="sxs-lookup"><span data-stu-id="7f747-115">NetX Crypto follows all general recommendations and basic requirements within the constraints of a real-time operating system and platforms requiring a small memory footprint and efficient execution.</span></span>

| <span data-ttu-id="7f747-116">Integritetsalgoritm</span><span class="sxs-lookup"><span data-stu-id="7f747-116">Algorithm</span></span>       | <span data-ttu-id="7f747-117">Nyckel längd (bitar)</span><span class="sxs-lookup"><span data-stu-id="7f747-117">Key Length (bits)</span></span>      |
| --------------- | ---------------------- |
| <span data-ttu-id="7f747-118">AES (CBC,/MASKIN)</span><span class="sxs-lookup"><span data-stu-id="7f747-118">AES(CBC, CTR)</span></span>   | <span data-ttu-id="7f747-119">128, 192, 256</span><span class="sxs-lookup"><span data-stu-id="7f747-119">128, 192, 256</span></span>          |
| <span data-ttu-id="7f747-120">AES (XCBC)</span><span class="sxs-lookup"><span data-stu-id="7f747-120">AES(XCBC)</span></span>       | <span data-ttu-id="7f747-121">128</span><span class="sxs-lookup"><span data-stu-id="7f747-121">128</span></span>                    |
| <span data-ttu-id="7f747-122">AES-CCM 8</span><span class="sxs-lookup"><span data-stu-id="7f747-122">AES-CCM 8</span></span>       | <span data-ttu-id="7f747-123">128</span><span class="sxs-lookup"><span data-stu-id="7f747-123">128</span></span>                    |
| <span data-ttu-id="7f747-124">3DES (CBC)</span><span class="sxs-lookup"><span data-stu-id="7f747-124">3DES(CBC)</span></span>       | <span data-ttu-id="7f747-125">192</span><span class="sxs-lookup"><span data-stu-id="7f747-125">192</span></span>                    |
| <span data-ttu-id="7f747-126">HMAC-SHA1</span><span class="sxs-lookup"><span data-stu-id="7f747-126">HMAC-SHA1</span></span>       | <span data-ttu-id="7f747-127">Valfri längd</span><span class="sxs-lookup"><span data-stu-id="7f747-127">Any length</span></span>             |
| <span data-ttu-id="7f747-128">HMAC-SHA224</span><span class="sxs-lookup"><span data-stu-id="7f747-128">HMAC-SHA224</span></span>     | <span data-ttu-id="7f747-129">Valfri längd</span><span class="sxs-lookup"><span data-stu-id="7f747-129">Any length</span></span>             |
| <span data-ttu-id="7f747-130">HMAC-SHA256</span><span class="sxs-lookup"><span data-stu-id="7f747-130">HMAC-SHA256</span></span>     | <span data-ttu-id="7f747-131">Valfri längd</span><span class="sxs-lookup"><span data-stu-id="7f747-131">Any length</span></span>             |
| <span data-ttu-id="7f747-132">HMAC-SHA384</span><span class="sxs-lookup"><span data-stu-id="7f747-132">HMAC-SHA384</span></span>     | <span data-ttu-id="7f747-133">Valfri längd</span><span class="sxs-lookup"><span data-stu-id="7f747-133">Any length</span></span>             |
| <span data-ttu-id="7f747-134">HMAC-SHA512</span><span class="sxs-lookup"><span data-stu-id="7f747-134">HMAC-SHA512</span></span>     | <span data-ttu-id="7f747-135">Valfri längd</span><span class="sxs-lookup"><span data-stu-id="7f747-135">Any length</span></span>             |
| <span data-ttu-id="7f747-136">HMAC-SHA512/224</span><span class="sxs-lookup"><span data-stu-id="7f747-136">HMAC-SHA512/224</span></span> | <span data-ttu-id="7f747-137">Valfri längd</span><span class="sxs-lookup"><span data-stu-id="7f747-137">Any length</span></span>             |
| <span data-ttu-id="7f747-138">HMAC-SHA512/256</span><span class="sxs-lookup"><span data-stu-id="7f747-138">HMAC-SHA512/256</span></span> | <span data-ttu-id="7f747-139">Valfri längd</span><span class="sxs-lookup"><span data-stu-id="7f747-139">Any length</span></span>             |
| <span data-ttu-id="7f747-140">HMAC-MD5</span><span class="sxs-lookup"><span data-stu-id="7f747-140">HMAC-MD5</span></span>        | <span data-ttu-id="7f747-141">Valfri längd</span><span class="sxs-lookup"><span data-stu-id="7f747-141">Any length</span></span>             |
| <span data-ttu-id="7f747-142">RSA</span><span class="sxs-lookup"><span data-stu-id="7f747-142">RSA</span></span>             | <span data-ttu-id="7f747-143">1024, 2048, 3072, 4096</span><span class="sxs-lookup"><span data-stu-id="7f747-143">1024, 2048, 3072, 4096</span></span> |

| <span data-ttu-id="7f747-144">Integritetsalgoritm</span><span class="sxs-lookup"><span data-stu-id="7f747-144">Algorithm</span></span>       | <span data-ttu-id="7f747-145">Digest-längd (bitar)</span><span class="sxs-lookup"><span data-stu-id="7f747-145">Digest Length (bits)</span></span> | <span data-ttu-id="7f747-146">Block storlek (bitar)</span><span class="sxs-lookup"><span data-stu-id="7f747-146">Block Size (bits)</span></span> |
| --------------- | -------------------- | ----------------- |
| <span data-ttu-id="7f747-147">SHA1</span><span class="sxs-lookup"><span data-stu-id="7f747-147">SHA1</span></span>            | <span data-ttu-id="7f747-148">160</span><span class="sxs-lookup"><span data-stu-id="7f747-148">160</span></span>                  | <span data-ttu-id="7f747-149">512</span><span class="sxs-lookup"><span data-stu-id="7f747-149">512</span></span>               |
| <span data-ttu-id="7f747-150">SHA224</span><span class="sxs-lookup"><span data-stu-id="7f747-150">SHA224</span></span>          | <span data-ttu-id="7f747-151">224</span><span class="sxs-lookup"><span data-stu-id="7f747-151">224</span></span>                  | <span data-ttu-id="7f747-152">512</span><span class="sxs-lookup"><span data-stu-id="7f747-152">512</span></span>               |
| <span data-ttu-id="7f747-153">SHA256</span><span class="sxs-lookup"><span data-stu-id="7f747-153">SHA256</span></span>          | <span data-ttu-id="7f747-154">256</span><span class="sxs-lookup"><span data-stu-id="7f747-154">256</span></span>                  | <span data-ttu-id="7f747-155">512</span><span class="sxs-lookup"><span data-stu-id="7f747-155">512</span></span>               |
| <span data-ttu-id="7f747-156">SHA384</span><span class="sxs-lookup"><span data-stu-id="7f747-156">SHA384</span></span>          | <span data-ttu-id="7f747-157">384</span><span class="sxs-lookup"><span data-stu-id="7f747-157">384</span></span>                  | <span data-ttu-id="7f747-158">1024</span><span class="sxs-lookup"><span data-stu-id="7f747-158">1024</span></span>              |
| <span data-ttu-id="7f747-159">SHA512</span><span class="sxs-lookup"><span data-stu-id="7f747-159">SHA512</span></span>          | <span data-ttu-id="7f747-160">512</span><span class="sxs-lookup"><span data-stu-id="7f747-160">512</span></span>                  | <span data-ttu-id="7f747-161">1024</span><span class="sxs-lookup"><span data-stu-id="7f747-161">1024</span></span>              |
| <span data-ttu-id="7f747-162">MD5</span><span class="sxs-lookup"><span data-stu-id="7f747-162">MD5</span></span>             | <span data-ttu-id="7f747-163">128</span><span class="sxs-lookup"><span data-stu-id="7f747-163">128</span></span>                  | <span data-ttu-id="7f747-164">512</span><span class="sxs-lookup"><span data-stu-id="7f747-164">512</span></span>               |
| <span data-ttu-id="7f747-165">HMAC-SHA1</span><span class="sxs-lookup"><span data-stu-id="7f747-165">HMAC-SHA1</span></span>       | <span data-ttu-id="7f747-166">160</span><span class="sxs-lookup"><span data-stu-id="7f747-166">160</span></span>                  | <span data-ttu-id="7f747-167">512</span><span class="sxs-lookup"><span data-stu-id="7f747-167">512</span></span>               |
| <span data-ttu-id="7f747-168">HMAC-SHA224</span><span class="sxs-lookup"><span data-stu-id="7f747-168">HMAC-SHA224</span></span>     | <span data-ttu-id="7f747-169">224</span><span class="sxs-lookup"><span data-stu-id="7f747-169">224</span></span>                  | <span data-ttu-id="7f747-170">512</span><span class="sxs-lookup"><span data-stu-id="7f747-170">512</span></span>               |
| <span data-ttu-id="7f747-171">HMAC-SHA256</span><span class="sxs-lookup"><span data-stu-id="7f747-171">HMAC-SHA256</span></span>     | <span data-ttu-id="7f747-172">256</span><span class="sxs-lookup"><span data-stu-id="7f747-172">256</span></span>                  | <span data-ttu-id="7f747-173">512</span><span class="sxs-lookup"><span data-stu-id="7f747-173">512</span></span>               |
| <span data-ttu-id="7f747-174">HMAC-SHA384</span><span class="sxs-lookup"><span data-stu-id="7f747-174">HMAC-SHA384</span></span>     | <span data-ttu-id="7f747-175">384</span><span class="sxs-lookup"><span data-stu-id="7f747-175">384</span></span>                  | <span data-ttu-id="7f747-176">1024</span><span class="sxs-lookup"><span data-stu-id="7f747-176">1024</span></span>              |
| <span data-ttu-id="7f747-177">HMAC-SHA512</span><span class="sxs-lookup"><span data-stu-id="7f747-177">HMAC-SHA512</span></span>     | <span data-ttu-id="7f747-178">512</span><span class="sxs-lookup"><span data-stu-id="7f747-178">512</span></span>                  | <span data-ttu-id="7f747-179">1024</span><span class="sxs-lookup"><span data-stu-id="7f747-179">1024</span></span>              |
| <span data-ttu-id="7f747-180">HMAC-SHA512/224</span><span class="sxs-lookup"><span data-stu-id="7f747-180">HMAC-SHA512/224</span></span> | <span data-ttu-id="7f747-181">224</span><span class="sxs-lookup"><span data-stu-id="7f747-181">224</span></span>                  | <span data-ttu-id="7f747-182">1024</span><span class="sxs-lookup"><span data-stu-id="7f747-182">1024</span></span>              |
| <span data-ttu-id="7f747-183">HMAC-SHA512/256</span><span class="sxs-lookup"><span data-stu-id="7f747-183">HMAC-SHA512/256</span></span> | <span data-ttu-id="7f747-184">256</span><span class="sxs-lookup"><span data-stu-id="7f747-184">256</span></span>                  | <span data-ttu-id="7f747-185">1024</span><span class="sxs-lookup"><span data-stu-id="7f747-185">1024</span></span>              |
| <span data-ttu-id="7f747-186">HMAC-MD5</span><span class="sxs-lookup"><span data-stu-id="7f747-186">HMAC-MD5</span></span>        | <span data-ttu-id="7f747-187">128</span><span class="sxs-lookup"><span data-stu-id="7f747-187">128</span></span>                  | <span data-ttu-id="7f747-188">512</span><span class="sxs-lookup"><span data-stu-id="7f747-188">512</span></span>               |
| <span data-ttu-id="7f747-189">Elliptic kurva</span><span class="sxs-lookup"><span data-stu-id="7f747-189">Elliptic Curve</span></span>  | <span data-ttu-id="7f747-190">P192/224/256/384/521</span><span class="sxs-lookup"><span data-stu-id="7f747-190">P192/224/256/384/521</span></span> |                   |

## <a name="netx-crypto-requirements"></a><span data-ttu-id="7f747-191">Krav för NetX-kryptering</span><span class="sxs-lookup"><span data-stu-id="7f747-191">NetX Crypto Requirements</span></span>

<span data-ttu-id="7f747-192">TBD: minnes krav..</span><span class="sxs-lookup"><span data-stu-id="7f747-192">TBD: Memory Requirement..</span></span> <span data-ttu-id="7f747-193">Vill du avbryta/återställa?</span><span class="sxs-lookup"><span data-stu-id="7f747-193">Interrupt/re-entrant safe?</span></span> <span data-ttu-id="7f747-194">Behöver diskussion</span><span class="sxs-lookup"><span data-stu-id="7f747-194">Needs discussion</span></span>

## <a name="netx-crypto-constraints"></a><span data-ttu-id="7f747-195">Begränsningar för NetX-kryptografi</span><span class="sxs-lookup"><span data-stu-id="7f747-195">NetX Crypto Constraints</span></span>

<span data-ttu-id="7f747-196">Inga.</span><span class="sxs-lookup"><span data-stu-id="7f747-196">None.</span></span>