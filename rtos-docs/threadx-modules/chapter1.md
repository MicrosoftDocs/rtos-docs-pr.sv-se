---
title: Kapitel 1 – Översikt
author: philmea
description: Den här artikeln är en översikt över Azure återställnings tider ThreadX-moduler
ms.author: philmea
ms.date: 07/15/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 0c9698086468d7bb41c33ebe9fa9d1ebb61b5f1f
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/22/2021
ms.locfileid: "104825497"
---
# <a name="chapter-1-overview"></a><span data-ttu-id="1c6a6-103">Kapitel 1: översikt</span><span class="sxs-lookup"><span data-stu-id="1c6a6-103">Chapter 1: Overview</span></span>

<span data-ttu-id="1c6a6-104">ThreadX module-komponenten innehåller en infrastruktur för program för att dynamiskt läsa in moduler som skapats separat från den inhemska delen av programmet.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-104">The ThreadX Module component provides an infrastructure for applications to dynamically load modules that are built separately from the resident portion of the application.</span></span> <span data-ttu-id="1c6a6-105">Detta är särskilt användbart i situationer där program kod storleken överskrider tillgängligt minne.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-105">This is especially useful in situations where the application code size exceeds available memory.</span></span> <span data-ttu-id="1c6a6-106">Det kan också hjälpa när nya funktioner måste läggas till när kärn avbildningen har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-106">It can also help when new features are required to be added after the core image is deployed.</span></span> <span data-ttu-id="1c6a6-107">Dessutom kan modulerna för dynamisk inläsning användas när en partiell uppdatering av den inbyggda program varan krävs.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-107">In addition, dynamically loading modules can be used when partial firmware updates are required.</span></span>

<span data-ttu-id="1c6a6-108">Minnes skydd för den inlästa modulen är valfritt, baserat på de egenskaper som anges i modulen inledning.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-108">Memory protection for the loaded module is optional, based on the properties specified in the module preamble.</span></span> <span data-ttu-id="1c6a6-109">När du har angett minnes skydd är processorns minnes hanterings maskin vara konfigurerad så att alla trådar i modulen endast beviljas åtkomst till modulens kod och data minne.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-109">When memory protection is specified, the processor's memory management hardware is configured such that all threads of the module are only allowed access to the module's code and data memory.</span></span> <span data-ttu-id="1c6a6-110">Eventuell ovidkommande minnes åtkomst eller körning resulterar i ett minnes fel och den felaktiga modulens tråd avbryts.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-110">Any extraneous memory access or execution will result in a memory fault and the offending module thread will be terminated.</span></span> <span data-ttu-id="1c6a6-111">Om programmet registrerar ett återanrop för minnes Fels meddelanden kallas detta även för att varna programmet för minnes felet.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-111">If the application registers a memory fault notification callback, this will also be called to alert the application of the memory fault.</span></span>

<span data-ttu-id="1c6a6-112">ThreadX module-komponenten använder programmet för att tillhandahålla ett minnes utrymme där moduler kan läsas in.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-112">The ThreadX Module component relies on the application to provide a memory area where modules can be loaded.</span></span> <span data-ttu-id="1c6a6-113">Instruktions området i varje modul kan köras på plats eller kopieras till minnes området RAM-modul för körning.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-113">The instruction area of each module may execute in place or be copied into the RAM module memory area for execution.</span></span> <span data-ttu-id="1c6a6-114">I samtliga fall allokeras data minnets krav från modulens minnes yta.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-114">In all cases, the module data memory requirements are allocated from the module memory area.</span></span>

<span data-ttu-id="1c6a6-115">Det finns ingen gräns för hur många moduler som kan läsas in samtidigt (utöver mängden tillgängligt minne), medan det bara finns en kopia av den inhemska modul Manager-koden.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-115">There are no limits on the number of modules that can be loaded at the same time (aside from the amount of memory available), while there is only one copy of the resident Module Manager code.</span></span> <span data-ttu-id="1c6a6-116">Bild 1 illustrerar förhållandet mellan modul hanteraren och själva moduler.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-116">Figure 1 illustrates the relationship of the Module Manager and the modules themselves.</span></span>

![Moduler och modul Manager-relation](media/image2.png)

<span data-ttu-id="1c6a6-118">**Bild 1** Moduler och modul Manager</span><span class="sxs-lookup"><span data-stu-id="1c6a6-118">**Figure 1** Modules and Module Manager</span></span>

<span data-ttu-id="1c6a6-119">Varje modul måste ha ett eget minnes område, vilket är programmets ansvar att definiera.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-119">Each module must have its own memory area, which is the application's responsibility to define.</span></span> <span data-ttu-id="1c6a6-120">Modulen och modul hanteraren interagerar med en program varu sändnings funktion via fördefinierade fråge-ID: n som motsvarar ThreadX-tjänster som begärts av modulen.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-120">The Module and the Module Manager interact through a software dispatch function via pre-defined request IDs that correspond to ThreadX services requested by the module.</span></span> <span data-ttu-id="1c6a6-121">Dessutom krävs modulen för att tillhandahålla en enda tråd start punkt samt den nödvändiga stack storleken, prioritet, modul-ID, återanrops stackens storlek/prioritet osv. Den här informationen definieras i varje moduls inledning.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-121">In addition, the module is required to provide a single thread entry point as well as the required stack size, priority, module ID, callback thread stack size/priority, etc. This information is defined in each module's preamble.</span></span>

<span data-ttu-id="1c6a6-122">Module manager ansvarar för att skapa den första modulens tråd och initiera körningen.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-122">The Module Manager is responsible for creating the initial module thread and initiating its execution.</span></span> <span data-ttu-id="1c6a6-123">När modulens första tråd körs ansvarar module Manager för att ange alla ThreadX API-begäranden som gjorts av modulen.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-123">Once the module's initial thread is executing, the Module Manager is responsible for fielding all ThreadX API requests made by the module.</span></span> <span data-ttu-id="1c6a6-124">En modul har fullständig åtkomst till ThreadX-API: et, inklusive möjligheten att skapa ytterligare trådar i modulen.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-124">A module has full access to the ThreadX API, including the ability to create additional threads within the module.</span></span>  
  
<span data-ttu-id="1c6a6-125">Namngivnings konventionerna för modulens käll kod är enkla: alla källfiler för module-hanteraren heter ***txm_module_manager_ \**** och alla filer som associeras exklusivt med modulen utelämnar delen "**_chef_*_" i namnet. Huvud include-filen _*_txm_module. h_** delas av käll koden Manager och module.</span><span class="sxs-lookup"><span data-stu-id="1c6a6-125">The module source code naming conventions are straightforward: all Module Manager source files are named ***txm_module_manager_\**** and all files associated exclusively with the module omit the "**_manager_*_" portion of the name. The main include file _*_txm_module.h_** is shared by the manager and module source code.</span></span>